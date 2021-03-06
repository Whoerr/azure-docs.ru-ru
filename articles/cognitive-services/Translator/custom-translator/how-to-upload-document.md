---
title: Отправка документа с помощью Custom Translator
titleSuffix: Azure Cognitive Services
description: Функция отправки документов отправляет параллельные документы (два документа, где один из них является источником, а другой — переводом) в службу.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: lajanuar
ms.topic: conceptual
ms.openlocfilehash: 83cce31d3eee9b5cca7b54e996e41a270fec0030
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/27/2021
ms.locfileid: "98896400"
---
# <a name="upload-a-document"></a>Отправка документа

В [Custom Translator](https://portal.customtranslator.azure.ai) вы можете отправлять параллельные документы для обучения своих моделей перевода. [Параллельные документы](what-are-parallel-documents.md) — это пары документов, в которых один является переводом другого. Один документ в паре содержит предложения на исходном языке, а другой — эти же предложения, переведенные на целевой язык.

Перед отправкой документов просмотрите [Руководство по форматам документов и соглашениям об именовании](document-formats-naming-convention.md), чтобы убедиться, что формат вашего файла поддерживается в Custom Translator.

## <a name="how-to-upload-document"></a>Отправка документа

На портале [Custom Translator](https://portal.customtranslator.azure.ai) щелкните вкладку "Документы", чтобы перейти на страницу документов.

![Ссылка на отправку документа](media/how-to/how-to-upload-1.png)


1.  На странице документов нажмите кнопку "Отправить файлы".

    ![Страница отправки документов](media/how-to/how-to-upload-2.png)

2.  В диалоговом окне заполните следующие сведения:

    а.  Типы документа:

    -  Обучающий. Этот документ будет использоваться для обучения набора.
    -  Настраиваемый. Этот документ будет использоваться для настройки набора.
    -  Проверочный. Этот документ будет использоваться для проверки набора.
    -  Словарь фраз. Этот документ будет использоваться для словаря фраз.
    -  Словарь предложений. Этот документ будет использоваться для словаря предложений.

    b.  Языковая пара

    c.  Переопределить имеющийся документ. Установите этот флажок, если вы хотите переписать все имеющиеся документы с одинаковым именем.

    d.  Заполните соответствующий раздел для параллельных или комбинированных данных.

    -  Параллельные данные:
        -  Исходный файл. Выберите файл исходного языка на локальном компьютере.
        -  Целевой файл. Выберите файл целевого языка на локальном компьютере.
        -  Имя документа. Используется при отправке параллельных файлов.

    - Комбинированные данные:
        -  Комбинированный файл. Выберите комбинированный файл на локальном компьютере. Ваш комбинированный файл содержит предложения как на исходном, так и на целевом языке. Для комбинированных файлов важно [соглашение об именовании](document-formats-naming-convention.md).

    д)  Щелкните "Отправить".

    ![Диалоговое окно отправки документа](media/how-to/how-to-upload-dialog.png)

3.  На этом этапе мы обрабатываем ваши документы и пытаемся извлечь предложения. Вы можете проверить состояние обработки документов, щелкнув View upload Progress (Посмотреть ход отправки).

    ![Диалоговое окно обработки отправки документов](media/how-to/how-to-upload-processing-dialog.png)

4.  На этой странице будут отображаться состояние и любые ошибки для каждого отправленного файла. Вы можете просмотреть состояние предыдущих отправок в любое время, щелкнув вкладку Upload history (История отправок).

    ![Диалоговое окно истории отправок документов](media/how-to/how-to-upload-document-history.png)


## <a name="view-upload-history"></a>Просмотр истории отправок

На странице истории отправок вы можете просмотреть подробные сведения обо всех отправляемых документах, такие как тип документа, языковая пара, состояние отправки и т. д.

1. На портале [Custom Translator](https://portal.customtranslator.azure.ai) щелкните вкладку Upload History (История отправок), чтобы просмотреть историю.

    ![Вкладка истории отправок](media/how-to/how-to-upload-history-1.png)

2. На этой странице отображается состояние всех ваших предыдущих отправок: от недавних до самых последних. Для каждой отправки отображается имя документа, состояние, дата, количество и тип отправленных файлов, а также языковая пара файла.

    ![Страница истории отправок](media/how-to/how-to-document-history-2.png)

3. Щелкните любую запись истории отправок. На странице сведений об истории отправок вы можете просматривать файлы, переданные как часть отправки, состояние отправки файла, язык файла и сообщения об ошибке (при наличии каких-либо ошибок во время отправки).

## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь со списком извлеченных предложений [на странице сведений о документе](how-to-view-document-details.md).
- Сведения об обучении модели см. в [этой статье](how-to-train-model.md).
