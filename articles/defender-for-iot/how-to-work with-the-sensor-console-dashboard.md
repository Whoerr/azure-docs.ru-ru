---
title: Работа с панелью мониторинга консоли датчика
description: Панель мониторинга позволяет быстро просматривать состояние безопасности сети. Он предоставляет общий обзор угроз для всей системы на временной шкале, а также сведения о связанных устройствах.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 11/03/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: eb37434213dd756ba5d7137b93a1cd37da5bb9ae
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2021
ms.locfileid: "100523640"
---
# <a name="the-dashboard"></a>Панель мониторинга

Панель мониторинга позволяет быстро просматривать состояние безопасности сети. Он предоставляет общий обзор угроз для всей системы на временной шкале, а также сведения о связанных устройствах, в том числе:

- Оповещения с разными уровнями серьезности:

- Критически важно

- Значительно

- Дополнительный номер

- Предупреждения

- Два индикатора в центре страницы показывают количество пакетов в секунду (PPS) и неподтвержденных предупреждений (UA). **PPS** — это количество пакетов, подтвержденных системой в секунду. **UA** — это количество предупреждений, которые еще не были подтверждены.

- Список неподтвержденных оповещений с их описанием.

- Временная шкала с описанием предупреждения.

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/dashboard-alert-screen-v2.png" alt-text="Панель мониторинга":::

## <a name="viewing-the-latest-alerts"></a>Просмотр последних оповещений

Счетчик неподтвержденных предупреждений (UA) в центре страницы указывает количество таких оповещений. Чтобы просмотреть список оповещений, выберите **больше предупреждений** в нижней части страницы панели мониторинга или щелкните **оповещения** в боковом меню.

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/unhandled-alerts-list.png" alt-text="Неподтвержденные предупреждения":::

## <a name="status-boxes"></a>Поля состояния

Каждое поле состояния описывается в этом разделе.

| Поле состояния и датчики | Описание: |
| -------------- | -------------- |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/critical-alert-status-box-v2.png" alt-text="Критические оповещения"::: | **Критические оповещения** . в поле в верхней части страницы отображается число критических оповещений. Установите этот флажок, чтобы отображать описания оповещений на временной шкале и в списке под датчиками, если таковые имеются.                              |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/major-alert-status-box-v2.png" alt-text="Основные оповещения"::: | **Основные оповещения** . в поле в правом верхнем углу страницы отображается количество основных оповещений. Установите этот флажок, чтобы отображать описания оповещений на временной шкале и в списке под датчиками, если таковые имеются.                                     |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/minor-alert-status-box-v2.png" alt-text="Незначительные предупреждения"::: | **Незначительные предупреждения** . в поле в левом нижнем углу страницы отображается число дополнительных оповещений. Установите этот флажок, чтобы отображать описания оповещений на временной шкале и в списке под датчиками, если таковые имеются.                                   |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/warnings-alert-status-box-v2.png" alt-text="предупреждающие оповещения;"::: | **Предупреждающие оповещения** . поле в нижней части страницы указывает число предупреждений. Установите этот флажок, чтобы отображать описания оповещений на временной шкале и в списке под датчиками, если таковые имеются.                             |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/all-alert-status-box-v2.png" alt-text="Все оповещения."::: | **Все оповещения** . поле в правом нижнем углу страницы указывает общее число критических, основных, дополнительных и предупреждающих оповещений. Установите этот флажок, чтобы отображать описания оповещений на временной шкале и в списке под датчиками, если таковые имеются. |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/packets-per-second-gauge-v2.png" alt-text="Пакетов в секунду"::: | **Пакетов в секунду (PPS)** — метрика PPS является индикатором производительности сети. |
| :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/unacknowledged-events-gauge-v2.png" alt-text="Неподтвержденные события (UA)"::: | **Неподтвержденные события** . Эта метрика указывает количество неподтвержденных событий.

## <a name="using-the-timeline"></a>Использование временной шкалы

Оповещения отображаются вдоль вертикальной временной шкалы, содержащей сведения о дате и времени.

График графика отображает:

- критические оповещения;

- Основные оповещения

- Незначительные предупреждения

- предупреждающие оповещения;

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/timeline-of-events.png" alt-text="График временной шкалы":::

## <a name="viewing-alerts"></a>Просмотр оповещений

Щелкните **стрелку вниз в нижней** части окна предупреждения, чтобы отобразить запись оповещения и сведения об устройствах.

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/extended-alert-screen.png" alt-text="Сведения об элементах и устройствах оповещений":::

- Выберите устройство, чтобы отобразить карту физического режима. Подсвеченные устройства выделены.

- Щелкните в любом месте окна оповещения, чтобы отобразить дополнительные сведения о предупреждении. Всплывающее окно будет выглядеть примерно так, как показано ниже.

- Выберите :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/excel-icon.png" alt-text="Excel"::: , чтобы экспортировать CSV-файл с предупреждением.

- Только администраторы и аналитики безопасности — выберите :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/approve-all-icon.png" alt-text="подтвердить все"::: , чтобы **подтвердить все** связанные оповещения.

- Выберите :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/pdf-icon.png" alt-text="PDF":::, чтобы скачать отчет о предупреждениях в PDF-файл.

- Выберите :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/pin-icon.png" alt-text="закрепить":::, чтобы закрепить или открепить оповещение. При выборе для закрепления он будет добавлен в окно **закрепленные оповещения** на экране **предупреждения** .

- Выберите :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/download-icon.png" alt-text="скачать"::: , чтобы исследовать оповещение, загрузив соответствующий файл ПКАП, содержащий анализ сетевого протокола.

- Выберите :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/cloud-download-icon.png" alt-text="облако"::: , чтобы скачать связанный фильтрованный файл ПКАП, который содержит только пакеты, относящиеся к предупреждениям, тем самым уменьшая размер выходного файла и позволяя более тщательно выполнять анализ. Его можно просмотреть с помощью [Wireshark](https://www.wireshark.org/).

- Выберите :::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/navigate-icon.png" alt-text="навигацию"::: , чтобы перейти к временной шкале события во время выполнения запрошенного оповещения. Это позволяет оценивать другие события, которые могут возникнуть в отношении конкретного предупреждения.

- Администраторы и аналитики безопасности — измените состояние предупреждения с неподтвержденного на подтвержденное. Выберите "сведения", чтобы утвердить обнаруженные действия.

:::image type="content" source="media/how-to-work with-the-sensor-console-dashboard/unauthorized-internet-connectivity-detection-v3.png" alt-text="Обнаружены несанкционированные подключения к Интернету":::

## <a name="next-steps"></a>Дальнейшие шаги

[Работа с оповещениями на датчике](how-to-work-with-alerts-on-your-sensor.md)
