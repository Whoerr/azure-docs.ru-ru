---
title: Фильтрация IP-адресов в Центре Интернета вещей Azure | Документация Майкрософт
description: Как использовать IP-фильтрацию для разрешения подключений с определенных IP-адресов к центру Интернета вещей Azure.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/19/2020
ms.author: jlian
ms.openlocfilehash: 6f83421d4ee56d56875e13ffbdd8ac9dbbf4b6bb
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101656369"
---
# <a name="use-ip-filters"></a>Использование фильтрации IP-адресов

Безопасность является важным аспектом любого решения Интернета вещей на базе Центра Интернета вещей Azure. Иногда при настройке системы безопасности необходимо явно указать IP-адреса, с которых могут подключаться устройства. Функция *фильтрации IP-адресов* позволяет настроить правила для отклонения или приема трафика с определенных IPv4-адресов.

## <a name="when-to-use"></a>Назначение

Используйте IP-фильтр для получения трафика только из указанного диапазона IP-адресов и отклоните все остальное. Например, вы используете центр Интернета вещей с помощью [Azure Express Route](../expressroute/expressroute-faqs.md#supported-services) для создания частных подключений между центром Интернета вещей и локальной инфраструктурой.

## <a name="default-setting"></a>Параметр по умолчанию

Чтобы перейти на страницу параметров фильтрации IP-адресов, выберите **Сеть**, **Общий доступ**, а затем **Выбранные диапазоны IP-адресов**:

:::image type="content" source="media/iot-hub-ip-filtering/ip-filter-default.png" alt-text="Параметры фильтрации IP-адресов по умолчанию для Центра Интернета вещей":::

По умолчанию раздел **Фильтрация IP-адресов** на портале для Центра Интернета вещей пуст. Этот параметр по умолчанию означает, что концентратор блокирует подключения со всех IP-адресов. Этот параметр по умолчанию эквивалентен правилу, блокирующему `0.0.0.0/0` диапазон IP-адресов.

## <a name="add-or-edit-an-ip-filter-rule"></a>Добавление или изменение правила фильтрации IP-адресов

Чтобы добавить правило фильтрации IP-адресов, выберите **+ Добавить правило фильтрации IP-адресов**.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-add-rule.png" alt-text="Добавление правила фильтрации IP-адресов в Центр Интернета вещей":::

После выбора параметра **Добавить правило фильтрации IP-адресов** заполните поля.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-after-selecting-add.png" alt-text="После выбора параметра &quot;Добавить правило фильтрации IP-адресов&quot;":::

* Укажите **имя** правила фильтрации IP-адресов. Это имя должно быть уникальным, без учета регистра, буквенно-цифровой строкой длиной до 128 символов. Допускаются только 7-разрядные буквы и цифры ASCII, а также `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}`.

* Укажите один IPv4-адрес или блок IP-адресов в нотации CIDR. Например, в нотации CIDR 192.168.100.0/22 обозначает диапазон из 1024 IPv4-адресов от 192.168.100.0 до 192.168.103.255.

После заполнения полей выберите **Сохранить**, чтобы сохранить правило. Появится предупреждение о том, что выполняется обновление.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png" alt-text="Уведомление о сохранении правила фильтрации IP-адресов":::

Параметр **Добавить** становится неактивным, когда достигается максимальное число правил фильтрации IP-адресов (10).

Чтобы изменить существующее правило, выберите данные, которые необходимо изменить, внесите изменения, а затем выберите **Сохранить**, чтобы сохранить изменения.

## <a name="delete-an-ip-filter-rule"></a>Удаление правила фильтрации IP-адресов

Чтобы удалить правило фильтрации IP-адресов, выберите значок корзины в этой строке, а затем выберите **Сохранить**. Правило удаляется, а изменения сохраняются.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-delete-rule.png" alt-text="Удаление правила фильтрации IP-адресов в Центре Интернета вещей":::

## <a name="apply-ip-filter-rules-to-the-built-in-event-hub-compatible-endpoint"></a>Применить правила IP-фильтра к встроенной конечной точке, совместимой с концентратором событий

Чтобы применить правила IP-фильтра к встроенной конечной точке, совместимой с концентратором событий, установите флажок **Применить IP-фильтры ко встроенной конечной точке?**, а затем нажмите кнопку **сохранить**.

:::image type="content" source="media/iot-hub-ip-filtering/ip-filter-built-in-endpoint.png" alt-text="Изображение, показывающее переключатель для встроенной конечной точки и сохранение":::

> [!NOTE]
> Этот параметр недоступен для бесплатных центров Интернета вещей (F1). Чтобы применить правила фильтрации IP-адресов к встроенной конечной точке, используйте платный центр Интернета вещей.

При включении этого параметра правила IP-фильтра реплицируются во встроенную конечную точку, поэтому доступ к нему могут получить только доверенные диапазоны IP-адресов.

Если отключить этот параметр, встроенная конечная точка будет доступна для всех IP-адресов. Такое поведение может быть полезно, если требуется считывать из конечной точки службы с изменяемыми IP-адресами, например Azure Stream Analytics. 

## <a name="how-filter-rules-are-applied"></a>Применение правил фильтрации

Правила фильтрации IP-адресов применяются на уровне службы Центра Интернета вещей. Поэтому они действуют для всех подключений с устройств и внутренних приложений, использующих любые поддерживаемые протоколы. Кроме того, можно выбрать, будет ли [Встроенная конечная точка, совместимая с концентратором событий](iot-hub-devguide-messages-read-builtin.md) (а не через строку подключения центра Интернета вещей), привязана к этим правилам. 

Любая попытка подключения с IP-адреса, который явно не разрешен, получает несанкционированный код состояния 401 и описание. В ответном сообщении не указывается правило фильтрации IP-адресов. Отклонение IP-адресов может препятствовать взаимодействию других служб Azure (таких как Azure Stream Analytics, виртуальные машины Azure или обозреватель устройств на портале Azure) с Центром Интернета вещей.

> [!NOTE]
> Если необходимо использовать Azure Stream Analytics (ASA) для чтения сообщений из центра Интернета вещей с включенным IP-фильтром, **Отключите** параметр **Применить фильтры IP к встроенной конечной точке** , а затем используйте имя и конечную точку, совместимые с концентратором событий, чтобы вручную добавить [входные данные концентратора событий](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-event-hubs) в ASA.

### <a name="ordering"></a>Упорядочение

Правила IP-фильтра *разрешают* применение правил и применяются без упорядочения. Только добавляемые IP-адреса могут подключаться к центру Интернета вещей. 

Например, если вы хотите принимать адреса в диапазоне `192.168.100.0/22` и отклонять все остальное, необходимо добавить в сетку только одно правило с диапазоном адресов `192.168.100.0/22` .

## <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Получение и обновление фильтров IP-адресов с помощью Azure CLI

Фильтры IP-адресов Центра Интернета вещей можно получать и обновлять с помощью [интерфейса командной строки Azure](/cli/azure/).

Чтобы получить текущие фильтры IP-адресов Центра Интернета вещей, выполните следующую команду:

```azurecli-interactive
az resource show -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs
```

Она возвратит объект JSON, в котором ваши имеющиеся фильтры IP-адресов отображаются в разделе `properties.networkRuleSets`:

```json
{
...
    "properties": {
        "networkRuleSets": {
            "defaultAction": "Deny",
            "applyToBuiltInEventHubEndpoint": true,
            "ipRules": [{
                    "filterName": "TrustedFactories",
                    "action": "Allow",
                    "ipMask": "1.2.3.4/5"
                },
                {
                    "filterName": "TrustedDevices",
                    "action": "Allow",
                    "ipMask": "1.1.1.1/1"
                }
            ]
        }
    }
}
```

Чтобы добавить новый фильтр IP-адреса для Центра Интернета вещей, выполните следующую команду:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.networkRuleSets.ipRules "{\"action\":\"Allow\",\"filterName\":\"TrustedIP\",\"ipMask\":\"192.168.0.1\"}"
```

Чтобы удалить имеющийся фильтр IP-адреса в Центре Интернета вещей, выполните следующую команду:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.networkRuleSets.ipRules <ipFilterIndexToRemove>
```

Здесь `<ipFilterIndexToRemove>` должна соответствовать порядок IP-фильтров в центре Интернета вещей `properties.networkRuleSets.ipRules` .

## <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Получение и обновление фильтров IP-адресов с помощью Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Фильтры IP-адресов Центра Интернета вещей можно получать и обновлять с помощью [Azure PowerShell](/powershell/azure/).

```powershell
# Get your IoT Hub resource using its name and its resource group name
$iothubResource = Get-AzResource -ResourceGroupName <resourceGroupNmae> -ResourceName <iotHubName> -ExpandProperties

# Access existing IP filter rules
$iothubResource.Properties.networkRuleSets.ipRules |% { Write-host $_ }

# Construct a new IP filter
$filter = @{'filterName'='TrustedIP'; 'action'='Allow'; 'ipMask'='192.168.0.1'}

# Add your new IP filter rule
$iothubResource.Properties.networkRuleSets.ipRules += $filter

# Remove an existing IP filter rule using its name, e.g., 'GoodIP'
$iothubResource.Properties.networkRuleSets.ipRules = @($iothubResource.Properties.networkRuleSets.ipRules | Where 'filterName' -ne 'GoodIP')

# Update your IoT Hub resource with your updated IP filters
$iothubResource | Set-AzResource -Force
```

## <a name="update-ip-filter-rules-using-rest"></a>Обновление правил фильтра IP-адреса с помощью REST


Вы также можете получать и изменять фильтры IP-адресов Центра Интернета вещей, используя конечную точку REST поставщика ресурсов Azure. Подробные сведения о `properties.networkRuleSets` см. в статье [Iot Hub Resource — Create Or Update](/rest/api/iothub/iothubresource/createorupdate) (Ресурс Центра Интернета вещей. Создание или обновление).

## <a name="ip-filter-classic-retirement"></a>Прекращение использования IP-фильтра (классическая модель)

Классический IP-фильтр был снят с учета. Дополнительные сведения см. в разделе [классический IP-фильтр центра Интернета вещей и как выполнить обновление](iot-hub-ip-filter-classic.md).

## <a name="next-steps"></a>Дальнейшие действия

Для дальнейшего изучения возможностей Центра Интернета вещей см. следующие статьи:

* [Общие сведения о метриках Центра Интернета вещей](./monitor-iot-hub.md)
* [Поддержка Центра Интернета вещей для виртуальных сетей с Приватным каналом и управляемым удостоверением](virtual-network-support.md)
* [Управление доступом к общедоступной сети для центра Интернета вещей](iot-hub-public-network-access.md)
* [Мониторинг Центра Интернета вещей](monitor-iot-hub.md)