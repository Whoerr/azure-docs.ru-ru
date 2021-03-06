---
title: Настройка решения Azure Defender для Интернета вещей на основе агента
description: Узнайте, как настроить сбор данных в Azure Defender для решения на основе агента IoT
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/21/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: e32b9c690db70baeda7dec2f9f1270f82c6881bd
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101733301"
---
# <a name="configure-azure-defender-for-iot-agent-based-solution"></a>Настройка решения Azure Defender для Интернета вещей на основе агента  

В этой статье описывается, как настроить сбор данных в Azure Defender для решения на основе агента IoT.

## <a name="configure-data-collection"></a>Настройка сбора данных

Чтобы настроить сбор данных в Azure Defender для решения на основе агента IoT, выполните следующие действия. 

1. Перейдите к портал Azure и выберите центр Интернета вещей, к которому подключен защитник для Интернета вещей. 

1. В меню  **Безопасность**   выберите **Параметры**. 

1. Выберите **сбор данных**. 

    :::image type="content" source="media/how-to-configure-agent-based-solution/data-collection.png" alt-text="В меню Безопасность выберите сбор данных.":::

## <a name="geolocation-and-ip-address-handling"></a>Обработка геолокации и IP-адресов 

Чтобы защитить решение Интернета вещей, IP-адреса входящих и исходящих подключений для устройств IoT, IoT Edge и центров Интернета вещей собираются и сохраняются по умолчанию. Эти сведения являются обязательными и используются для обнаружения ненормальных подключений из подозрительных источников IP-адресов. Например, при наличии попыток установить соединения из источника IP-адресов известного ботнетом или из источника IP-адресов за пределами географического расположения. Защитник для службы IoT предоставляет гибкие возможности по включению и отключению сбора данных IP-адресов в любое время. 

Чтобы включить или отключить сбор данных IP-адреса, выполните следующие действия. 

1. Откройте центр Интернета вещей, а затем в ****   меню **Безопасность** выберите пункт Параметры   . 

1. Выберите экран **сбор данных**   и измените параметры географического расположения и обработки IP-адресов в соответствии с вашими потребностями. 

## <a name="log-analytics-creation"></a>Создание Log Analytics 

Защитник для Интернета вещей позволяет хранить оповещения системы безопасности, рекомендации и необработанные данные безопасности в рабочей области Log Analytics. Log Analytics прием в центре Интернета вещей по умолчанию установлен в значение **Off** для решения "защитник для Интернета вещей". Вы можете присоединить защитник для Интернета вещей к рабочей области аналитики журналов, а также сохранить данные безопасности. 

В рабочей области Log Analytics в защитнике для Интернета вещей хранятся два типа данных, хранимых по умолчанию:
 
- Оповещения системы безопасности.

- Проектирован. 

Вы можете добавить хранилище дополнительных сведений в качестве типа `raw events` . 

> [!Note] 
> Хранение  `raw events`   в log Analytics несет дополнительные затраты на хранение. 

Чтобы разрешить Log Analytics работать с микроагентом: 

1. Перейдите к коллекции данных **конфигурации рабочей области**  >  и переключите переключатель в положение  **вкл**. 

1. Создайте новую рабочую область Log Analytics или подключите существующую. 

1. Убедитесь, что выбран параметр **доступ к необработанным данным безопасности**   .  

    :::image type="content" source="media/how-to-configure-agent-based-solution/data-settings.png" alt-text="Убедитесь, что выбран доступ к необработанным данным безопасности.":::

1. Нажмите кнопку **Сохранить**.

Каждый месяц первые 5 гигабайт данных, принимаемых для каждого клиента в службу Log Analytics Azure, бесплатно. Каждый гигабайт данных, принимаемых в рабочую область Azure Log Analytics, не взимается за первые 31 день. Дополнительные сведения о ценах см. в разделе [log Analytics цены](https://azure.microsoft.com/pricing/details/monitor/). 

Чтобы изменить конфигурацию рабочей области Log Analytics: 

1. В центре Интернета вещей в меню **Безопасность** выберите  **Параметры**. 

1. Выберите экран **сбор данных**   и измените конфигурацию рабочей области log Analytics параметров в соответствии со своими потребностями. 

Чтобы получить доступ к оповещениям в рабочей области Log Analytics после настройки:

1. Выберите оповещение в защитнике для Интернета вещей.

1. Выберите **исследовать оповещения в рабочей области log Analytics**.

Чтобы получить доступ к оповещениям в рабочей области Log Analytics после настройки:

1. Выберите рекомендацию в защитнике для Интернета вещей.

1. Выберите **исследовать рекомендации в log Analytics рабочей области**. 
 
Дополнительные сведения о запросах данных из Log Analytics см. в разделе Начало [работы с запросами в log Analytics](../azure-monitor/logs/get-started-queries.md). 

## <a name="turn-off-defender-for-iot"></a>Отключение защитника для Интернета вещей 

Чтобы включить или отключить защитник для службы IoT в определенном центре Интернета вещей: 

1. В центре Интернета вещей в меню **Безопасность** выберите  **Параметры**.

1. Выберите экран **сбор данных**   и измените конфигурацию рабочей области log Analytics параметров в соответствии со своими потребностями.

## <a name="next-steps"></a>Дальнейшие действия 

Перейдите к следующей статье, чтобы [настроить решение](quickstart-configure-your-solution.md).