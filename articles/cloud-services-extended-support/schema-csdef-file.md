---
title: Схема определения облачных служб (Расширенная поддержка) Azure (файл CSDEF) | Документация Майкрософт
description: Сведения, относящиеся к схеме определения (CSDEF) для облачных служб (Расширенная поддержка)
ms.topic: article
ms.service: cloud-services-extended-support
ms.date: 10/14/2020
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: ab85067184ebe5b34097a3c81aa521d509ae4b9a
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744862"
---
# <a name="azure-cloud-services-extended-support-definition-schema-csdef-file"></a>Схема определения облачных служб Azure (Расширенная поддержка) (файл CSDEF)

Файл определения службы определяет модель службы для приложения. Файл содержит определения ролей, доступных для облачной службы, указывает конечные точки службы и устанавливает параметры конфигурации для службы. Значения параметров конфигурации задаются в файле конфигурации службы, как описано в [схеме конфигурации облачной службы (Расширенная поддержка)](schema-cscfg-file.md)).

По умолчанию файл схемы конфигурации системы диагностики Azure устанавливается в каталог `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas`. Замените `<version>` установленной версией [пакета SDK для Azure](https://www.windowsazure.com/develop/downloads/).

Расширение по умолчанию для файла определения службы — CSDEF.

## <a name="basic-service-definition-schema"></a>Базовая схема определения службы
Файл определения службы должен содержать один элемент `ServiceDefinition`. Определение службы должно содержать как минимум один элемент роли (`WebRole` или `WorkerRole`). В одном определении могут содержаться до 25 ролей. Также вы можете смешивать типы ролей. Определение службы также содержит необязательный элемент `NetworkTrafficRules`, ограничивающий роли, которые могут взаимодействовать с указанными внутренними конечными точками. Определение службы также содержит необязательный элемент `LoadBalancerProbes`, содержащий определенные пользователем зонды работоспособности конечных точек.

Базовый формат файла определения службы приведен ниже.

```xml
<ServiceDefinition name="<service-name>" topologyChangeDiscovery="<change-type>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" upgradeDomainCount="<number-of-upgrade-domains>" schemaVersion="<version>">
  
  <LoadBalancerProbes>
         …
  </LoadBalancerProbes>
  
  <WebRole …>
         …
  </WebRole>
  
  <WorkerRole …>
         …
  </WorkerRole>
  
  <NetworkTrafficRules>
         …
  </NetworkTrafficRules>

</ServiceDefinition>
```

## <a name="schema-definitions"></a>Определения схем
В следующих разделах описаны схемы:

- [Схема LoadBalancerProbe](schema-csdef-loadbalancerprobe.md)
- [Схема WebRole](schema-csdef-webrole.md)
- [Схема WorkerRole](schema-csdef-workerrole.md)
- [Схема NetworkTrafficRules](schema-csdef-networktrafficrules.md)

##  <a name="servicedefinition-element"></a><a name="ServiceDefinition"></a> ServiceDefinition, элемент
Элемент `ServiceDefinition` — это элемент верхнего уровня файла определения службы.

В таблице ниже описаны атрибуты элемента `ServiceDefinition`.

| attribute               | Описание |
| ----------------------- | ----------- |
| name                    |Обязательный. Имя службы. Имя должно быть уникальным в пределах учетной записи службы.|
| topologyChangeDiscovery | Необязательный элемент. Указывает тип уведомления об изменении топологии. Доступны следующие значения:<br /><br /> -   `Blast` — как можно быстрее отправляет обновление всем экземплярам роли. Чтобы вы могли использовать этот параметр, роль должна иметь возможность обработать обновление топологии без перезапуска.<br />-   `UpgradeDomainWalk` — отправляет обновление каждому экземпляру роли в последовательном режиме после того, как предыдущий экземпляр успешно принял обновление.|
| schemaVersion           | Необязательный элемент. Указывает версию схемы определения службы. Версия схемы позволяет Visual Studio выбрать правильные средства пакета SDK для использования при проверке схемы, если установлено одновременно несколько версий пакета SDK.|
| upgradeDomainCount      | Необязательный элемент. Указывает число доменов обновления, между которыми распределены роли в этой службе. Экземпляры ролей назначаются домену обновления при развертывании службы. Дополнительные сведения см. в статьях [Обновление роли облачной службы или развертывание](sample-update-cloud-service.md) и [Управление доступностью виртуальных машин](../virtual-machines/manage-availability.md) . Вы можете указать до 20 доменов обновления. Если число доменов обновления не указано, по умолчанию оно равно 5.|

## <a name="see-also"></a>См. также раздел

[Схема конфигурации облачных служб Azure (Расширенная поддержка) (CSCFG-файл)](schema-cscfg-file.md).