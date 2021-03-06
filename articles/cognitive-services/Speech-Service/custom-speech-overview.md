---
title: Обзор Пользовательское распознавание речи — служба речи
titleSuffix: Azure Cognitive Services
description: Пользовательское распознавание речи — это набор интерактивных средств, позволяющих оценивать и улучшать точность распознавания речи и текста в приложениях, средствах и продуктах Майкрософт.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: trbye
ms.custom: contperf-fy21q2; references_regions
ms.openlocfilehash: 39370659e71a7d281914b360eea83eb0b68b25ba
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101716573"
---
# <a name="what-is-custom-speech"></a>Что такое Пользовательское распознавание речи?

[Пользовательское распознавание речи](https://aka.ms/customspeech) — это набор средств на основе пользовательского интерфейса, позволяющий оценивать и улучшать точность распознавания речи и текста для приложений и продуктов Майкрософт. Все, что нужно для начала работы, — это несколько тестовых звуковых файлов. Перейдите по ссылкам в этой статье, чтобы приступить к созданию пользовательского интерфейса преобразования речи в текст.

## <a name="whats-in-custom-speech"></a>Что такое Пользовательское распознавание речи?

Прежде чем выполнять какие-либо действия с Пользовательское распознавание речи, вам потребуется учетная запись Azure и подписка на службу распознавания речи. После создания учетной записи вы можете подготовить данные, обучить и тестировать модели, проверять качество распознавания, оценивать точность и в конечном итоге развернуть и использовать пользовательскую модель преобразования речи в текст.

На этой схеме показаны части, составляющие [область пользовательское распознавание речи в Speech Studio](https://aka.ms/customspeech). Используйте приведенные ниже ссылки для получения дополнительных сведений о каждом шаге.

![Схема, в которой показаны компоненты, составляющие область Пользовательское распознавание речи в Speech Studio.](./media/custom-speech/custom-speech-overview.png)

1. [Подпишитесь и создайте проект](#set-up-your-azure-account). Создайте учетную запись Azure и подпишитесь на службу распознавания речи. Эта единая подписка предоставляет доступ к преобразованию речи в текст, преобразования текста в речь, перевода речи и [речевой](https://speech.microsoft.com/customspeech)среды. Затем используйте подписку на службу распознавания речи, чтобы создать первый проект Пользовательское распознавание речи.

1. [Отправка тестовых данных](./how-to-custom-speech-test-and-train.md). Отправьте тестовые данные (звуковые файлы), чтобы оценить предложение Microsoft Speech-Text для ваших приложений, средств и продуктов.

1. [Проверьте качество распознавания](how-to-custom-speech-inspect-data.md). Используйте [речевую студию](https://speech.microsoft.com/customspeech) для воспроизведения отправленного звука и проверки качества распознавания речи для тестовых данных. Сведения о количественных измерениях см. в разделе [Проверка данных](how-to-custom-speech-inspect-data.md).

1. [Оцените и улучшите точность](how-to-custom-speech-evaluate-data.md). Оцените и улучшайте точность модели преобразования речи в текст. В [речевом](https://speech.microsoft.com/customspeech) редакторе предусмотрена *Частота ошибок слов*, с помощью которой можно определить, требуется ли дополнительное обучение. Если вы удовлетворены точностью, вы можете напрямую использовать интерфейсы API речевой службы. Если требуется повысить точность по относительным значениям от 5% до 20%, используйте на портале вкладку " **обучение** " для отправки дополнительных обучающих данных, таких как транскрипции с меткой и связанный текст.

1. [Обучение и развертывание модели](how-to-custom-speech-train-model.md). Повысьте точность модели преобразования речи в текст путем предоставления записанных записей (от 10 до 1 000 часов) и связанного текста (<200 МБ) вместе с тестовыми данными. Эти данные помогают обучить модель преобразования речи в текст. После обучения повторно протестируйте. Если вы удовлетворены результатами, вы можете развернуть модель в пользовательской конечной точке.

## <a name="set-up-your-azure-account"></a>Настройка учетной записи Azure

Для создания настраиваемой модели необходимо иметь учетную запись Azure и подписку [на службу](https://speech.microsoft.com/customspeech) речи. Если у вас нет учетной записи и подписки, [попробуйте службу "Речь" бесплатно](overview.md#try-the-speech-service-for-free).

> [!NOTE]
> Обязательно создайте стандартную подписку (S0). Подписки Free (F0) не поддерживаются.

Если вы планируете обучить пользовательскую модель с помощью **звуковых данных**, выберите один из следующих регионов с выделенным оборудованием, доступным для обучения. Это сокращает время, затрачиваемое на обучение модели, и позволяет использовать больше звука для обучения. В этих регионах для обучения речевая служба будет использовать до 20 часов звука. в других регионах он будет использовать не более 8 часов.

* Восточная Австралия
* Центральная Канада
* Центральная Индия
* Восточная часть США
* восточная часть США 2
* Центрально-северная часть США
* Северная Европа
* Центрально-южная часть США
* Юго-Восточная Азия
* южная часть Соединенного Королевства
* US Gov (Аризона)
* US Gov (Вирджиния)
* Западная Европа
* западная часть США 2

После создания учетной записи Azure и подписки на службу речи необходимо войти в систему [Speech Studio](https://speech.microsoft.com/customspeech) и подключить подписку.

1. Войдите в систему [Speech Studio](https://aka.ms/custom-speech).
1. Выберите подписку, в которой необходимо работать, и создайте проект речи.
1. Если вы хотите изменить подписку, нажмите кнопку шестеренки в верхнем меню.

## <a name="how-to-create-a-project"></a>Создание проекта

Такие содержимое, как данные, модели, тесты и конечные точки, организованы в *проекты* в [Speech Studio](https://speech.microsoft.com/customspeech). Каждый проект относится к домену, стране или языку. Например, можно создать проект для центров обработки вызовов, использующих английский язык в США.

Чтобы создать первый проект, выберите речь **-to-Text/Custom Speech**, а затем выберите **Новый проект**. Следуйте инструкциям мастера для создания проекта. После создания проекта вы увидите четыре вкладки: **данные**, **тестирование**, **обучение** и **развертывание**. Используйте ссылки, приведенные в разделе [дальнейшие действия](#next-steps) , чтобы узнать, как использовать каждую вкладку.

> [!IMPORTANT]
> [Речь](https://aka.ms/custom-speech) , ранее известная как "пользовательское распознавание речи Portal", недавно обновлялась. Если вы создали предыдущие данные, модели, тесты и опубликованные конечные точки на портале CRIS.ai или с помощью API, необходимо создать новый проект на новом портале для подключения к этим старым сущностям.

## <a name="model-lifecycle"></a>Жизненный цикл модели

В Пользовательское распознавание речи используются как *базовые модели* , так и *пользовательские модели*. Каждый язык имеет одну или несколько базовых моделей. Как правило, при отпускания новой модели речи в обычную службу распознавания речи она также импортируется в службу Пользовательское распознавание речи в качестве новой базовой модели. Они обновляются каждые 3 – 6 месяцев. Более старые модели обычно становятся менее полезными, так как в самой новой модели обычно используется более высокая точность.

В отличие от этого, пользовательские модели создаются путем адаптации выбранной базовой модели к конкретному сценарию клиента. Вы можете использовать определенную настраиваемую модель в течение длительного времени после того, как она будет соответствовать вашим потребностям. Но рекомендуется периодически выполнять обновление до последней базовой модели и переучить ее с дополнительными данными. 

Ниже приведены другие ключевые термины, связанные с жизненным циклом модели.

* **Адаптация**: создание базовой модели и ее настройка в домене или сценарии с помощью текстовых данных и (или) звуковых данных.
* **Декодирование**: использование модели и распознавание речи (декодирование звука в текст).
* **Конечная точка**: конкретное пользовательское развертывание базовой модели или настраиваемой модели, доступ к которой возможен *только* для данного пользователя.

### <a name="expiration-timeline"></a>Временная шкала срока действия

По мере того как новые модели и новые функции становятся доступными и более старыми, менее точные модели удаляются, см. следующие временные шкалы для модели и истечения срока действия конечной точки:

**Базовые модели** 

* Адаптация: доступна в течение одного года. После импорта модели она будет доступна в течение одного года для создания пользовательских моделей. Через один год новые пользовательские модели должны быть созданы на основе более новой версии базовой модели.  
* Декодирование: доступно в течение двух лет после импорта. Таким образом, вы можете создать конечную точку и использовать в течение двух лет эту модель. 
* Конечные точки: доступны на той же временной шкале, что и декодирование.

**Настраиваемые модели**

* Декодирование: доступно в течение двух лет после создания модели. Поэтому можно использовать пользовательскую модель в течение двух лет (пакетная обработка/реальное время/тестирование) после ее создания. Через два года *следует переучить модель* , так как базовая модель, как правило, не является рекомендуемой для адаптации.  
* Конечные точки: доступны на той же временной шкале, что и декодирование.

При истечении срока действия базовой модели или пользовательской модели она всегда будет возвращаться к *последней версии базовой модели*. Поэтому ваша реализация никогда не будет прерываться, но она может стать менее точной для *конкретных данных* , если срок действия пользовательских моделей истек. Срок действия модели можно увидеть в следующих местах в области Пользовательское распознавание речи в Speech Studio:

* Сводка по обучению модели
* Сведения об обучении модели
* Коротко о развертывании
* Сведения о развертывании

Вы также можете проверить даты истечения срока действия с помощью [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) пользовательских API-интерфейсов распознавания речи в `deprecationDates` свойстве в ответе JSON.

Обратите внимание, что модель можно обновить на пользовательской конечной точке речи без простоев, изменив модель, используемую конечной точкой в разделе развертывания в Speech Studio, или через пользовательский API распознавания речи.

## <a name="next-steps"></a>Дальнейшие действия

* [Подготовка и тестирование данных](./how-to-custom-speech-test-and-train.md)
* [Проверка данных](how-to-custom-speech-inspect-data.md)
* [Оценка и улучшение точности модели](how-to-custom-speech-evaluate-data.md)
* [Обучение и развертывание модели](how-to-custom-speech-train-model.md)
