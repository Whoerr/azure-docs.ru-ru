---
title: Использование центра безопасности Azure для защиты узлов DOCKER и защиты контейнеров
description: Как защитить узлы DOCKER и убедиться, что они соответствуют требованиям к тестированию для DOCKER
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: b30e08a2739000d2a7ec14a95742f2654e1d2ea1
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/27/2021
ms.locfileid: "98916240"
---
# <a name="harden-your-docker-hosts"></a>Защита узлов Docker

Центр безопасности Azure выявляет неуправляемые контейнеры, размещенные на виртуальных машинах IaaS Linux или других компьютерах Linux с контейнерами Docker. Центр безопасности постоянно оценивает конфигурации этих контейнеров. Затем он сравнивает их с [эталонным контейнером Docker для Центра Интернет-безопасности (CIS)](https://www.cisecurity.org/benchmark/docker/).

Центр безопасности применяет весь набор правил эталонного Docker для CIS и выдает предупреждения, если контейнеры не соответствуют любому из этих элементов управления. При обнаружении ошибок в конфигурации Центр безопасности создает рекомендации по безопасности. На **странице рекомендаций** Центра безопасности вы можете просмотреть эти рекомендации и устранить проблемы.

При обнаружении уязвимостей они группируются в рамках одной рекомендации.

>[!NOTE]
> Эти проверки производительности CI не будут выполняться на управляемых виртуальными машинами экземплярах или модулях данных, управляемых AKS.

## <a name="availability"></a>Доступность

|Аспект|Сведения|
|----|:----|
|Состояние выпуска:|Общедоступная версия|
|Цены|Требуется [Azure Defender для серверов](defender-for-servers-introduction.md)|
|Требуемые роли и разрешения|**Читатель** в рабочей области, к которой подключается узел|
|Облако.|![Да](./media/icons/yes-icon.png) Коммерческие облака<br>![Да](./media/icons/yes-icon.png) Национальные и независимые (US Gov, China Gov, другие правительственные облака)|
|||

## <a name="identify-and-remediate-security-vulnerabilities-in-your-docker-configuration"></a>Выявление и исправление уязвимостей системы безопасности в конфигурации DOCKER

1. В меню центра безопасности откройте страницу **рекомендации** .

1. Отфильтруйте **уязвимости в настройках безопасности контейнеров** , чтобы устранить их, и выберите рекомендацию.

    На странице рекомендации отображаются затронутые ресурсы (узлы DOCKER). 

    :::image type="content" source="./media/monitor-container-security/docker-host-vulnerabilities-found.png" alt-text="Рекомендации по исправлению уязвимостей в конфигурациях безопасности контейнеров ":::

1. Чтобы просмотреть и исправить элементы управления CI, которые не удалось выполнить на определенном узле, выберите узел, который нужно исследовать. 

    > [!TIP]
    > Если вы начали работу на странице "Инвентаризация активов" и достигли этой рекомендации, ыделить на странице рекомендаций кнопку **предпринять действие** .
    >
    > :::image type="content" source="./media/monitor-container-security/host-security-take-action-button.png" alt-text="Воспользуйтесь кнопкой &quot;действие&quot;, чтобы запустить Log Analytics":::

    Log Analytics открывается с настраиваемой операцией, готовой к запуску. Пользовательский запрос по умолчанию содержит список всех проверенных правил, которые были оценены, а также рекомендации, помогающие устранить эти проблемы.

    :::image type="content" source="./media/monitor-container-security/docker-host-vulnerabilities-in-query.png" alt-text="Страница Log Analytics с запросом, содержащим все неудачные элементы управления CI":::

1. При необходимости настройте параметры запроса.

1. Если вы уверены, что команда подходит и готова для вашего узла, выберите **выполнить**.


## <a name="next-steps"></a>Дальнейшие действия

Усиление защиты DOCKER — это лишь один аспект функций безопасности контейнера центра безопасности. 

Дополнительные сведения о [безопасности контейнеров см. в центре безопасности](container-security.md).