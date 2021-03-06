---
title: Декопия строк и поиск значений NULL с помощью фрагментов потока данных
description: Узнайте, как легко выпустить строки и найти значения NULL с помощью фрагментов кода в потоках данных.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/30/2020
ms.openlocfilehash: 1b49b2584c4cb462c7c0f520fe8d1b5bf69c8674
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100393672"
---
# <a name="dedupe-rows-and-find-nulls-by-using-data-flow-snippets"></a>Декопия строк и поиск значений NULL с помощью фрагментов потока данных

[!INCLUDE [appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

С помощью фрагментов кода при сопоставлении потоков данных можно легко выполнять такие распространенные задачи, как дедупликация данных и фильтрация значений NULL. В этой статье объясняется, как легко добавить эти функции в конвейеры с помощью фрагментов сценариев потока данных.
<br>
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4GnhH]

## <a name="create-a-pipeline"></a>Создание конвейера

1. Выберите **Новый конвейер**.

1. Добавьте действие потока данных.

1. Перейдите на вкладку **Параметры источника** , добавьте преобразование источник, а затем подключите его к одному из наборов данных.

    ![Снимок экрана: панель "Параметры источника" для добавления типа источника.](media/data-flow/snippet-adf-2.png)

    Фрагменты проверки дедупликации и NULL используют универсальные шаблоны, использующие преимущества смещения схемы потока данных. Фрагменты кода работают с любой схемой из набора данных или с наборами, не имеющими предварительно определенных схем.

1. В разделе "разные строки с использованием всех столбцов" [скрипта потока данных (DFS)](./data-flow-script.md#distinct-row-using-all-columns)Скопируйте фрагмент кода для дистинктровс.

1. [Перейдите на страницу документации по скрипту потока данных и скопируйте фрагмент кода для отдельных строк.](./data-flow-script.md#distinct-row-using-all-columns)

    ![Снимок экрана исходного фрагмента кода.](media/data-flow/snippet-adf-3.png)

1. В скрипте после определения `source1` нажмите клавишу ВВОД и вставьте фрагмент кода.

1. Выполните одно из следующих действий.

   * Подключите вставленный фрагмент кода к преобразованию источника, созданному ранее на графике, введя **Source1** перед вставленным кодом.

   * Кроме того, можно подключить новое преобразование в конструкторе, выбрав входящий поток из нового узла преобразования в графе.

     ![Снимок экрана: панель "Параметры условного разбиения".](media/data-flow/snippet-adf-4.png)

   Теперь поток данных удалит из источника дублирующиеся строки, используя преобразование «Статистическая обработка», которое группирует по всем строкам, используя общий хэш для всех значений столбцов.
    
1. Добавьте фрагмент кода для разбиения данных в один поток, содержащий строки со значениями NULL и другим потоком без значений NULL. Для этого сделайте следующее.

1. [Вернитесь к библиотеке фрагментов кода, и на этот раз скопируйте код для проверки значений NULL.](./data-flow-script.md#check-for-nulls-in-all-columns)

   b. В конструкторе потоков данных снова выберите **Скрипт** , а затем вставьте новый код преобразования в нижней части. Это действие подключает скрипт к предыдущему преобразованию, помещая имя этого преобразования перед вставленным фрагментом.

   Теперь граф потока данных должен выглядеть примерно так:

    ![Снимок экрана: диаграмма потока данных.](media/data-flow/snippet-adf-1.png)

  Теперь вы создали рабочий поток данных с общими проверками дедупинг и NULL, используя существующие фрагменты кода из библиотеки сценариев потока данных и добавляя их в существующую структуру.

## <a name="next-steps"></a>Следующие шаги

* Создайте оставшуюся часть логики потока данных с помощью [преобразования потоков данных](concepts-data-flow-overview.md)сопоставления.