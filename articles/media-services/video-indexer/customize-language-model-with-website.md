---
title: Настройка языковой модели с помощью веб-сайта индексатора видео
titleSuffix: Azure Media Services
description: Узнайте, как настроить языковую модель с помощью веб-сайта индексатора видео.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 08/10/2020
ms.author: kumud
ms.openlocfilehash: dd8b36340deb6c785989107461dd420e7fc0d985
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2020
ms.locfileid: "97722578"
---
# <a name="customize-a-language-model-with-the-video-indexer-website"></a>Настройка языковой модели с помощью веб-сайта Индексатора видео

Индексатор видео позволяет создавать пользовательские языковые модели для настройки распознавания речи путем передачи текста адаптации, а именно текста из предметной области, к словарю которой вы хотите адаптировать подсистему. Как только вы обучите свою модель, новые слова, появляющиеся в тексте адаптации, будут распознаваться.

Подробный обзор и рекомендации по использованию пользовательских языковых моделей см. в статье [Настройка языковой модели с помощью индексатора видео](customize-language-model-overview.md).

Вы можете использовать веб-сайт Индексатора видео для создания и изменения пользовательских языковых моделей в своей учетной записи, как описано в этом разделе. Кроме того, вы можете применять API, как описано в [этой статье](customize-language-model-with-api.md).

## <a name="create-a-language-model"></a>Создание языковой модели

1. Перейдите на веб-сайт [индексатора видео](https://www.videoindexer.ai/) и выполните вход.
1. Чтобы настроить модель в учетной записи, нажмите кнопку **Настройка модели содержимого** в левой части страницы.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/customize-language-model/model-customization.png" alt-text="Настройка модели содержимого в индексаторе видео":::
1. Выберите вкладку **Язык**.

    Откроется список поддерживаемых языков.
1. Под нужным языком выберите **Добавить модель**.
1. Введите имя языковой модели и нажмите клавишу ВВОД.

    На этом шаге создается модель и предоставляется возможность передать текстовые файлы в модель.
1. Чтобы добавить текстовый файл, выберите **Добавить файл**. Откроется обозреватель файлов.
1. Перейдите к текстовому файлу и выберите его. Вы можете добавить несколько текстовых файлов в языковую модель.

    Можно также добавить текстовый файл, нажав кнопку **...** в правой части модели языка и выбрав **Добавить файл**.
1. Завершив передачу текстовых файлов, **выберите параметр «** зеленый».

Процесс обучения может занять несколько минут. По завершении рядом с моделью появится значение состояния **Trained** (Обученная). Вы можете просматривать, скачивать и удалять файл из модели.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/customize-language-model/customize-language-model.png" alt-text="Обучение модели":::

### <a name="using-a-language-model-on-a-new-video"></a>Использование языковой модели в новом видео

Чтобы использовать языковую модель на новом видео, выполните одно из следующих действий.

* Нажмите кнопку **Отправить** в верхней части страницы.

    ![Кнопка "Отправить" в индексаторе видео](./media/customize-language-model/upload.png)
* Удалите аудио или видеофайл или найдите файл.

У вас есть возможность выбрать **язык исходного видео**. Выберите раскрывающийся список и выберите модель языка, созданную из списка. В ней должны быть указаны язык вашей языковой модели и заданное имя в скобках. Пример:

![Выбор исходного языка видео — переиндексация видео с помощью индексатора видео](./media/customize-language-model/reindex.png)

Выберите параметр **Отправить** в нижней части страницы, и новое видео будет индексироваться с использованием языковой модели.

### <a name="using-a-language-model-to-reindex"></a>Повторная индексация с помощью языковой модели

Чтобы использовать языковую модель для переиндексации видео в коллекции, выполните следующие действия.

1. Войдите на домашнюю страницу [индексатора видео](https://www.videoindexer.ai/) .
1. Нажмите кнопку **...** на видео и выберите пункт **Повторное индексирование**.
1. Вы можете выбрать **язык исходного видео** для переиндексации видео. Выберите раскрывающийся список и выберите модель языка, созданную из списка. В ней должны быть указаны язык вашей языковой модели и заданное имя в скобках.
1. Нажмите кнопку **Повторное индексирование** , и ваше видео будет переиндексировано с использованием языковой модели.

## <a name="edit-a-language-model"></a>Изменение языковой модели

Вы можете отредактировать языковую модель, изменив ее имя, добавив в нее файлы и удалив их из нее.

При добавлении или удалении файлов из языковой модели необходимо еще раз обучить модель, выбрав зеленый параметр « **обучение** ».

### <a name="rename-the-language-model"></a>Переименование языковой модели

Имя модели языка можно изменить, нажав кнопку с многоточием (**...**) в правой части языковой модели и выбрав команду **Переименовать**.

Введите новое имя и нажмите клавишу ВВОД.

### <a name="add-files"></a>Добавление файлов

Чтобы добавить текстовый файл, выберите **Добавить файл**. Откроется обозреватель файлов.

Перейдите к текстовому файлу и выберите его. Вы можете добавить несколько текстовых файлов в языковую модель.

Можно также добавить текстовый файл, нажав кнопку с многоточием (**...**) в правой части языковой модели и выбрав **Добавить файл**.

### <a name="delete-files"></a>Удаление файлов

Чтобы удалить файл из языковой модели, нажмите кнопку с многоточием (**...**) в правой части текстового файла и выберите **Удалить**. Появится новое окно с сообщением о том, что удаление невозможно отменить. Выберите параметр **Удалить** в новом окне.

Это действие полностью удалит файл из языковой модели.

## <a name="delete-a-language-model"></a>Удаление языковой модели

Чтобы удалить языковую модель из учетной записи, нажмите кнопку с многоточием (**...**) в правой части модели языка и выберите **Удалить**.

Появится новое окно с сообщением о том, что удаление невозможно отменить. Выберите параметр **Удалить** в новом окне.

Это действие полностью удалит языковую модель из вашей учетной записи. Любое видео, использующее удаленную языковую модель, сохранит тот же самый индекс до тех пор, пока вы не переиндексируете видео. При повторном индексировании видео можно назначить для него новую языковую модель. В противном случае индексатор видео будет использовать модель по умолчанию для повторного индексирования видео.

## <a name="customize-language-models-by-correcting-transcripts"></a>Настройка языковых моделей путем исправления записей

Индексатор видео поддерживает автоматическую настройку языковых моделей на основе реальных исправлений, внесенных пользователями в транскрипции.

1. Чтобы внести исправления в запись, откройте видео, которое требуется изменить, из видео своей учетной записи. Перейдите на вкладку **временная шкала** .

    ![Настройка языковой модели вкладка временной шкалы — индексатор видео](./media/customize-language-model/timeline.png)

1. Щелкните значок с изображением карандаша, чтобы изменить запись транскрипции.

    ![Настройка языковой модели изменения транскрипции — индексатор видео](./media/customize-language-model/edits.png)

    Индексатор видео захватывает все строки, исправленные вами, в транскрипции видео и автоматически добавляет их в текстовый файл с именем "из изменений в записи". Эти изменения используются для переобучения конкретной языковой модели, которая использовалась для индексирования этого видео. 
    
    Также включаются изменения, выполненные на временной шкале [мини-приложения](video-indexer-embed-widgets.md) .
    
    Если вы не указали языковую модель при индексировании этого видео, все изменения для этого видео будут храниться в языковой модели по умолчанию с именем "Адаптация учетных записей" на обнаруженном языке видео.
    
    Если в одну и ту же строку внесено несколько изменений, для обновления языковой модели будет использоваться только последняя версия исправленной строки.  
    
    > [!NOTE]
    > Для настройки используются только текстовые исправления. Исправления, не затрагивающие реальные слова (например, знаки пунктуации и пробелы), не включаются.
    
1. Исправления в транскрипции отображаются на вкладке язык страницы Настройка модели содержимого.

   Чтобы просмотреть файл "от изменения записи в журнал" для каждой модели языка, выберите его, чтобы открыть.

    ![От правки записи — индексатор видео](./media/customize-language-model/from-transcript-edits.png)

## <a name="next-steps"></a>Дальнейшие действия

[Настройка языковой модели с помощью API](customize-language-model-with-api.md)
