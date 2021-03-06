---
title: Обновление кластера для использования общего имени сертификата
description: Узнайте, как преобразовать сертификат кластера Azure Service Fabric из объявлений на основе отпечатков в общие имена.
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: f719b1eb39da776827c6babec61e9e6701bb4602
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97900796"
---
# <a name="convert-cluster-certificates-from-thumbprint-based-declarations-to-common-names"></a>Преобразование сертификатов кластера из объявлений на основе отпечатков в общие имена

Подпись сертификата (обычно известный как отпечаток) является уникальной. Сертификат кластера, объявленный отпечатком, относится к конкретному экземпляру сертификата. Эта конкретная сложность делает смену сертификатов, а управление в общем, сложное и явное. Каждое изменение требует согласованного обновления кластера и базовых вычислительных узлов.

Преобразование объявлений сертификатов кластера Azure Service Fabric из отпечатка на основе объявлений, основанных на общем имени субъекта сертификата (CN), значительно упрощает управление. В частности, для отката сертификата больше не требуется обновление кластера. В этой статье описывается преобразование существующего кластера в объявления на основе CN без простоев.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="move-to-certificate-authority-signed-certificates"></a>Перейти к сертификатам, подписанным центром сертификации

Безопасность кластера, сертификат которого объявлен по отпечатку, на тот факт, что он недоступен или нецелесообразно, для подделки сертификата с той же подписью, что и другой. В этом случае проверенное значение сертификата менее важно, поэтому самозаверяющие сертификаты подходят.

В отличие от этого, безопасность кластера, сертификаты которого объявляются в потоке CN, передается из неявного доверия владельцу кластера в поставщике сертификатов. Поставщик — это служба инфраструктуры открытых ключей (PKI), которая выдала сертификат. Отношения доверия основываются на методах сертификации PKI, будь то аудит и одобрение Рабочей безопасности, а также другие доверенные лица и т. д.

Владелец кластера также должен иметь подробные сведения о том, какие центры сертификации выдают сертификаты, поскольку это фундаментальный аспект проверки сертификатов по темам. Это также подразумевает, что самозаверяющие сертификаты полностью не подходят для этой цели. Буквально любой пользователь может создать сертификат с заданной темой.

Сертификат, объявленный с помощью CN, обычно считается допустимым, если:

* Цепочка может быть построена успешно.
* Субъект имеет ожидаемый элемент CN.
* Его издатель (в цепочке немедленно или выше) является доверенным для агента, выполняющего проверку.

Service Fabric поддерживает объявление сертификатов с помощью CN двумя способами:

* С *неявными* поставщиками, что означает, что цепочка должна заканчиваться якорем доверия.
* С издателями, объявленными по отпечатку, которая называется закреплением издателя.

Дополнительные сведения см. в разделе [объявления проверки сертификатов на основе общих имен](cluster-security-certificates.md#common-name-based-certificate-validation-declarations).

Чтобы преобразовать кластер с помощью самозаверяющего сертификата, объявленного с помощью отпечатка в CN, целевой сертификат, подписанный ЦС, должен быть впервые добавлен в кластер по отпечатку. Только затем преобразование из отпечатка в CN.

В целях тестирования самозаверяющий сертификат *может* быть объявлен с помощью CN, но только в том случае, если издатель закреплен на собственном отпечатке. С точки зрения безопасности это действие почти эквивалентно объявлению того же сертификата по отпечатку. Успешное преобразование этого типа не гарантирует успешного преобразования отпечатка в CN с помощью сертификата, подписанного ЦС. Мы рекомендуем протестировать преобразование с помощью правильного сертификата, подписанного ЦС. Для этого тестирования существуют бесплатные варианты.

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Отправка сертификата и его установка в масштабируемом наборе

В Azure рекомендуемым механизмом для получения и подготовки сертификатов является Azure Key Vault и его средства. Сертификат, соответствующий объявлению сертификата кластера, должен быть подготовлен для каждого узла масштабируемых наборов виртуальных машин, составляющих кластер. Дополнительные сведения см. [в разделе секреты в масштабируемых наборах виртуальных машин](../virtual-machine-scale-sets/virtual-machine-scale-sets-faq.md#how-do-i-securely-ship-a-certificate-to-the-vm).

Перед внесением изменений в объявления сертификатов кластера важно установить как текущие, так и целевые сертификаты кластера на виртуальных машинах каждого типа кластера. Путешествие от выдачи сертификата к подготовке к Service Fabric узлу подробно рассматривается в [пути к сертификату](cluster-security-certificate-management.md#the-journey-of-a-certificate).

## <a name="bring-the-cluster-to-an-optimal-starting-state"></a>Переведите кластер в оптимальное начальное состояние

Преобразование объявления сертификата из отпечатка на основе общего с учетом CN:

- Как каждый узел в кластере находит и представляет свои учетные данные другим узлам.
- Как каждый узел проверяет учетные данные своего аналога при установлении безопасного подключения.

Прежде чем продолжать, проверьте [правила представления и проверки для обеих конфигураций](cluster-security-certificates.md#certificate-configuration-rules) . Наиболее важным моментом при выполнении преобразования отпечатка в CN является то, что обновленные и еще не обновленные узлы (т. е. узлы, принадлежащие разным доменам обновления) должны иметь возможность успешной взаимной проверки подлинности в любой момент обновления. Чтобы добиться такого поведения, рекомендуется объявить целевой или конечный сертификат по отпечатку при первоначальном обновлении. Затем завершите переход на CN в последующем. Если кластер уже находится в рекомендуемом начальном состоянии, этот раздел можно пропустить.

Существует несколько допустимых начальных состояний для преобразования. Инвариант заключается в том, что кластер уже использует целевой сертификат (объявленный отпечатком) в начале обновления до CN. Мы рассмотрим `GoalCert` , `OldCert1` и `OldCert2` в этой статье.

#### <a name="valid-starting-states"></a>Допустимые начальные состояния

- `Thumbprint: GoalCert, ThumbprintSecondary: None`
- `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1`, где `GoalCert` имеет более позднюю `NotBefore` дату, чем `OldCert1`
- `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert`, где `GoalCert` имеет более позднюю `NotBefore` дату, чем `OldCert1`

> [!NOTE]
> До версии 7.2.445 (7,2 CU4) Service Fabric выбрали сертификат с истекшим сроком действия (сертификат с самым крайним свойством «NotAfter»), поэтому для указанных выше состояний до 7,2 CU4 требуется, чтобы в Гоалцерт существовала более поздняя `NotAfter` Дата, чем `OldCert1`

Если кластер не находится в одном из указанных ранее состояний, см. сведения о достижении этого состояния в разделе в конце этой статьи.

## <a name="select-the-desired-cn-based-certificate-validation-scheme"></a>Выберите нужную схему проверки сертификатов на основе CN

Как было сказано ранее, Service Fabric поддерживает объявление сертификатов с помощью CN с помощью неявного якоря доверия или с явным закреплением отпечатков издателя. Дополнительные сведения см. в разделе [объявления проверки сертификатов на основе общих имен](cluster-security-certificates.md#common-name-based-certificate-validation-declarations).

Убедитесь, что у вас есть хорошее представление о различиях и последствиях выбора любого из этих механизмов. Синтаксически, это различие или Choice определяется значением `certificateIssuerThumbprintList` параметра. Пустые средства полагается на доверенный корневой ЦС (якоря доверия), тогда как набор отпечатков запрещает использование разрешенных прямых издателей сертификатов кластера.

   > [!NOTE]
   > `certificateIssuerThumbprint`Поле позволяет указать ожидаемых прямых издателей сертификатов, объявленных с помощью субъекта CN. Допустимые значения — один или несколько отпечатков SHA1 с разделителями-запятыми. Это действие повысит уровень проверки сертификата.
   >
   > Если поставщики не указаны или список пуст, сертификат будет принят для проверки подлинности, если его цепочку можно построить. Затем сертификат завершается в корне, доверенном для проверяющего элемента управления. Если указан один или несколько отпечатков издателя, сертификат будет принят, если отпечаток его прямого издателя, извлеченного из цепочки, соответствует любому значению, указанному в этом поле. Сертификат будет принят независимо от того, является ли корневой узел доверенным.
   >
   > PKI может использовать разные центры сертификации (называемые также *издателями*) для подписывания сертификатов с заданной темой. Поэтому важно указать все ожидаемые отпечатки поставщика для этой темы. Иными словами, обновление сертификата не обязательно будет подписано тем же издателем, что и обновляемый сертификат.
   >
   > Указание издателя считается рекомендуемым. Пропуск издателя будет продолжать работать для сертификатов, связанных с доверенным корнем, но это поведение имеет ограничения и может быть выделено в ближайшем будущем. Кластеры, развернутые в Azure, защищены с помощью сертификатов X509, выданных закрытой PKI и объявленные субъектом, могут не быть проверены Service Fabric (для обмена данными между кластером и службой). Для проверки требуется, чтобы политика сертификатов PKI была доступна для обнаружения, доступности и доступности.

## <a name="update-the-clusters-azure-resource-manager-template-and-deploy"></a>Обновление шаблона Azure Resource Manager кластера и развертывание

Управляйте кластерами Service Fabric с помощью шаблонов Azure Resource Manager (ARM). Альтернативой, который также использует артефакты JSON, является [Обозреватель ресурсов Azure (Предварительная версия)](https://resources.azure.com). Эквивалентный интерфейс в портал Azure в настоящее время недоступен.

Если исходный шаблон, соответствующий существующему кластеру, недоступен, можно получить эквивалентный шаблон в портал Azure. Перейдите к группе ресурсов, содержащей кластер, и выберите пункт **Экспорт шаблона** в меню **Автоматизация** слева. Затем выберите нужные ресурсы. Необходимо экспортировать как минимум набор масштабирования виртуальных машин и ресурсы кластера соответственно. Созданный шаблон также можно скачать. Этот шаблон может потребовать изменений до полного развертывания. Шаблон также может не совпадать с исходным. Это отражение текущего состояния ресурса кластера.

Ниже приведены необходимые изменения.

- Обновление определения расширения узла Service Fabric (в ресурсе виртуальной машины). Если кластер определяет несколько типов узлов, необходимо обновить определение каждого соответствующего масштабируемого набора виртуальных машин.
- Обновление определения ресурса кластера.

Здесь приведены подробные примеры.

### <a name="update-the-virtual-machine-scale-set-resources"></a>Обновление ресурсов масштабируемого набора виртуальных машин
От:
```json
"virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            ...
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            ...
                            "certificate": {
                                "thumbprint": "[parameters('certificateThumbprint')]",
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        ...
                    }
                },
```
В:
```json
"virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            ...
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            ...
                            "certificate": {
                                "commonNames": [
                                    "[parameters('certificateCommonName')]"
                                ],
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        ...
                    }
                },
```

### <a name="update-the-cluster-resource"></a>Обновление ресурса кластера

В ресурсе **Microsoft. ServiceFabric/Clusters** добавьте свойство **цертификатекоммоннамес** с параметром **коммоннамес** и полностью удалите свойство **сертификата** (все его параметры).

От:
```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            ...
        ],
        "properties": {
            "addonFeatures": [
                ...
            ],
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
```
В:
```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            ...
        ],
        "properties": {
            "addonFeatures": [
                ...
            ],
            "certificateCommonNames": {
                "commonNames": [
                    {
                        "certificateCommonName": "[parameters('certificateCommonName')]",
                        "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprintList')]"
                    }
                ],
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
```

Дополнительные сведения см. [в статье развертывание Service Fabric кластера, использующего общее имя сертификата вместо отпечатка](./service-fabric-create-cluster-using-cert-cn.md).

## <a name="deploy-the-updated-template"></a>Развертывание обновленного шаблона

Повторно разверните обновленный шаблон после внесения изменений.

```powershell
$groupname = "sfclustertutorialgroup"

New-AzResourceGroupDeployment -ResourceGroupName $groupname -Verbose `
    -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" 
```

## <a name="achieve-a-valid-starting-state-for-converting-a-cluster-to-cn-based-certificate-declarations"></a>Достижение допустимого начального состояния для преобразования кластера в объявления сертификатов на основе CN

| Начальное состояние | Обновление 1 | Обновление 2 |
| :--- | :--- | :--- |
| `Thumbprint: OldCert1, ThumbprintSecondary: None` и `GoalCert` имеет более позднюю `NotBefore` дату, чем `OldCert1` | `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert` | - |
| `Thumbprint: OldCert1, ThumbprintSecondary: None` и `OldCert1` имеет более позднюю `NotBefore` дату, чем `GoalCert` | `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1` | `Thumbprint: GoalCert, ThumbprintSecondary: None` |
| `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert`, где `OldCert1` имеет дату позже `NotBefore``GoalCert` | Обновить до `Thumbprint: GoalCert, ThumbprintSecondary: None` | - |
| `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1`, где `OldCert1` имеет дату позже `NotBefore``GoalCert` | Обновить до `Thumbprint: GoalCert, ThumbprintSecondary: None` | - |
| `Thumbprint: OldCert1, ThumbprintSecondary: OldCert2` | Удалите одно из `OldCert1` или `OldCert2` , чтобы получить состояние `Thumbprint: OldCertx, ThumbprintSecondary: None` | Продолжить с нового начального состояния |

> [!NOTE]
> Для кластера версии, более ранней, чем версия 7.2.445 (7,2 CU4), замените на `NotBefore` `NotAfter` в приведенных выше состояниях.

Инструкции по выполнению любого из этих обновлений см. [в статье Управление сертификатами в кластере Azure Service Fabric](service-fabric-cluster-security-update-certs-azure.md).

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о [безопасности кластера](service-fabric-cluster-security.md).
* Узнайте, как [выполнить переработку сертификата кластера по общему имени](service-fabric-cluster-rollover-cert-cn.md).
* Узнайте, как [настроить кластер для автоматической смены ключей](cluster-security-certificate-management.md).

[image1]: ./media/service-fabric-cluster-change-cert-thumbprint-to-cn/PortalViewTemplates.png
