---
title: Создание отчетов о векторах атак
description: Отчеты с векторами атак предоставляют графическое представление цепочки уязвимостей устройств, которые используют злоумышленники.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/17/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: e9960fc2120add845be8feda9bafdef95a9b5f94
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2021
ms.locfileid: "100522331"
---
# <a name="attack-vector-reporting"></a>Формирование векторов атак

## <a name="about-attack-vector-reports"></a>Отчеты о векторах атак

Отчеты с векторами атак предоставляют графическое представление цепочки уязвимостей устройств, которые используют злоумышленники. Эти уязвимости могут предоставить злоумышленнику доступ к ключевым сетевым устройствам. Имитатор векторов атак вычисляет направления атак в режиме реального времени и анализирует все векторы атак для определенного целевого объекта.

Работа с вектором атаки позволяет оценить влияние действий по устранению рисков в последовательности атак. Затем можно определить, например, если при обновлении системы нарушается путь злоумышленника, нарушая цепочку атак или при этом сохраняется альтернативный путь атаки. Эти сведения помогают определить приоритеты действий по исправлению и устранению рисков.

:::image type="content" source="media/how-to-generate-reports/control-center.png" alt-text="Просмотр оповещений в центре управления.":::

> [!NOTE]
> Администраторы и аналитики безопасности могут выполнять процедуры, описанные в этом разделе.

## <a name="create-an-attack-vector-report"></a>Создание отчета о векторе атак

Чтобы создать моделирование вектора атак, выполните следующие действия.

1. Выберите :::image type="content" source="media/how-to-generate-reports/plus.png" alt-text="знак &quot;плюс":::" в боковом меню, чтобы добавить имитацию.

 :::image type="content" source="media/how-to-generate-reports/vector.png" alt-text="Моделирование вектора атак.":::

2. Введите свойства имитации:

   - **Имя**: имя имитации.

   - **Максимальное число векторов**: максимальное количество векторов в одном моделировании.

   - **Показывать в схеме устройства**: отображать вектор атаки в виде фильтра на карте устройства.

   - **Все исходные устройства**. вектор атаки будет рассматривать все устройства как источник атаки.

   - **Источник атаки**. при атаке в качестве источника атаки будет учитываться только указанное устройство.

   - **Все целевые устройства**. Направление атаки будет рассматривать все устройства как цель атаки.

   - **Цель атаки**. вектор атаки учитывает только указанные устройства в качестве цели атаки.

   - **Исключение устройств**. указанные устройства будут исключены из моделирования вектора атак.

   - **Исключить подсети**. указанные подсети будут исключены из моделирования вектора атак.

3. Выберите **Добавить симуляцию**. Имитация будет добавлена в список имитаций.

   :::image type="content" source="media/how-to-generate-reports/new-simulation.png" alt-text="Добавление нового моделирования.":::

4. Выберите, :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false"::: Если требуется изменить имитацию.

   Выберите, :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false"::: если хотите удалить имитацию.

   Выберите, :::image type="icon" source="media/how-to-generate-reports/make-a-favorite-icon.png" border="false"::: Если необходимо пометить имитацию как избранную.

5. Появится список направлений атак, содержащий оценку вектора (от 100), исходное устройство атаки и целевое устройство атаки. Выберите конкретную атаку для графического описания направлений атак.

   :::image type="content" source="media/how-to-generate-reports/sample-attack-vectors.png" alt-text="Векторы атак.":::

## <a name="next-steps"></a>Дальнейшие шаги

[Формирование векторов атак](how-to-create-attack-vector-reports.md)


