---
title: Оптическое распознавание символов (OCR) — Компьютерное зрение
titleSuffix: Azure Cognitive Services
description: Основные понятия, связанные с оптическим распознаванием символов (OCR) изображений и документов с печатным и рукописным текстом с помощью API компьютерного зрения.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/11/2020
ms.author: pafarley
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 2833fd44b75f4bebf41b5100eb2350ca69436520
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100362800"
---
# <a name="optical-character-recognition-ocr"></a>Оптическое распознавание текста (OCR)

API компьютерного зрения Azure включает возможности оптического распознавания символов (OCR), которые извлекают печатный или рукописный текст из изображений. Вы можете извлекать текст из изображений, таких как фотографии печатных форм или контейнеров с серийными номерами, а также из документов — счетов, ведомостей, финансовых отчетов, статей и т. д.

## <a name="read-api"></a>API чтения 

Компьютерное зрение [Read API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) — это новейшая технология оптического распознавания[содержимого Azure (Узнайте о новых возможностях](./whats-new.md)), которая извлекает печатный текст (на нескольких языках), рукописный текст (только на английском языке), цифры и символы валют из изображений и многостраничных PDF-документов. Он оптимизирован для извлечения текста из изображений с большим объемом текста и многостраничных PDF-документов со смешанными языками. Он поддерживает обнаружение печатного и рукописного текста в одном изображении или документе.

![Как OCR преобразует изображения и документы в структурированный вывод с извлеченным текстом](./Images/how-ocr-works.svg)

## <a name="input-requirements"></a>Требования к входным данным
При вызове **Read** в качестве входных данных используются изображения и документы. Они имеют следующие требования.

* Поддерживаемые форматы файлов: JPEG, PNG, BMP, PDF и TIFF
* Для файлов PDF и TIFF, обрабатываются до 2000 страниц (только первые две страницы для уровня Free).
* Размер файла должен быть менее 50 МБ (4 МБ для уровня Free) и размер измерений не менее 50 x 50 пикселей и не более 10000 x 10000 пикселей. 
* Размеры PDF-файла должны составлять не более 17 x 17 дюймов, соответствующих юридическим или стандартным размерам бумаги и меньшему размеру.

> [!NOTE]
> **Языковой ввод** 
>
> В [вызове Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) имеется необязательный параметр запроса для языка. Чтение поддерживает автоматическую идентификацию языков и многоязычные документы, поэтому следует только указать код языка, если хотите, чтобы документ обрабатывался как конкретный язык.

## <a name="ocr-demo-examples"></a>Демонстрация OCR (примеры)

![Демонстрации OCR](./Images/ocr-demo.gif)

## <a name="step-1-the-read-operation"></a>Шаг 1. операция чтения

При [вызове Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) API чтения в качестве входных данных принимается изображение или документ PDF, а текст извлекается асинхронно. Вызов возвращает с помощью поля заголовка ответа с именем `Operation-Location` . `Operation-Location`Значение — это URL-адрес, СОДЕРЖАЩИЙ идентификатор операции, который будет использоваться на следующем шаге.

|Заголовок ответа| URL-адрес результата |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/vision/v3.1/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

> [!NOTE]
> **Выставление счетов** 
>
> Страница [цен на компьютерное зрение](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/) включает ценовую категорию для чтения. Каждое проанализированное изображение или страница является одной транзакцией. При вызове операции с документом PDF или TIFF, содержащим 100 страниц, операция чтения будет считаться 100ом транзакций, а плата будет взиматься за 100 транзакций. Если было выполнено 50 вызовов операции и каждый вызов отправлял документ с 100 страниц, вам будет выставлен счет за 50 X 100 = 5000 транзакций.

## <a name="step-2-the-get-read-results-operation"></a>Шаг 2. операция получения результатов чтения

Второй шаг — вызов операции [получения результатов чтения](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d9869604be85dee480c8750) . Эта операция принимает в качестве входных данных идентификатор операции, созданный операцией чтения. Он возвращает ответ JSON, содержащий поле **состояния** со следующими возможными значениями. Эта операция вызывается итеративно, пока она не вернется со значением, которое было **выполнено** . Используйте интервал от 1 до 2 секунд, чтобы избежать превышения скорости запросов в секунду (RPS).

|Поле| Тип | Возможные значения |
|:-----|:----:|:----|
|status | строка | notStarted: операция не запущена. |
| |  | выполняется: операция обрабатывается. |
| |  | Сбой: сбой операции. |
| |  | успешно: операция выполнена успешно. |

> [!NOTE]
> Уровень Free ограничивает частоту запросов до 20 вызовов в минуту. Платный уровень допускает 10 запросов в секунду (RP), которые можно увеличить при запросе. Используйте канал поддержки Azure или группу учетных записей, чтобы запросить более высокий уровень запросов в секунду (RPS).

Если поле **Status** имеет значение **успех** , ответ JSON содержит извлеченное текстовое содержимое из изображения или документа. Ответ JSON поддерживает первоначальные группы строк распознанных слов. Он включает извлеченные текстовые строки и координаты ограничивающих прямоугольников. Каждая строка текста включает все извлеченные слова с их координатами и рейтингами достоверности.

> [!NOTE]
> Данные, отправленные в `Read` операцию, временно шифруются и хранятся в неактивных данных в течение короткой длительности, а затем удаляются. Это позволяет приложениям извлекать извлеченный текст как часть ответа службы.

## <a name="sample-json-output"></a>Пример выходных данных JSON

См. Следующий пример успешного ответа JSON:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0.8551,
        "width": 2661,
        "height": 1901,
        "unit": "pixel",
        "lines": [
          {
            "boundingBox": [
              67,
              646,
              2582,
              713,
              2580,
              876,
              67,
              821
            ],
            "text": "The quick brown fox jumps",
            "words": [
              {
                "boundingBox": [
                  143,
                  650,
                  435,
                  661,
                  436,
                  823,
                  144,
                  824
                ],
                "text": "The",
                "confidence": 0.958
              }
            ]
          }
        ]
      }
    ]
  }
}
```

## <a name="natural-reading-order-output-latin-only"></a>Выходные данные естественного порядка чтения (только для латиницы)
С помощью [предварительной версии API Read 3,2 для предварительного просмотра](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005)укажите порядок, в котором текстовые строки выводятся с помощью `readingOrder` параметра запроса. Используйте `natural` для более удобного для человека вывода в порядке чтения, как показано в следующем примере. Эта функция поддерживается только для латинских языков.

:::image border type="content" source="./Images/ocr-reading-order-example.png" alt-text="Пример порядка чтения с РАСПОЗНАВАНИем":::

## <a name="handwritten-classification-for-text-lines-latin-only"></a>Рукописная классификация для текстовых линий (только латиница)
Ответ на [API предварительной версии Read 3,2 содержит сведения](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005) о том, имеет ли каждая строка текста стиль рукописного ввода или нет, а также оценку достоверности. Эта функция поддерживается только для латинских языков. В следующем примере показана рукописная классификация для текста в изображении.

:::image border type="content" source="./Images/ocr-handwriting-classification.png" alt-text="Пример классификации рукописного ввода с оптическим распознаванием":::

## <a name="select-pages-or-page-ranges-for-text-extraction"></a>Выберите страницы или диапазоны страниц для извлечения текста
С помощью [API предварительной версии Read 3,2](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005)для больших многостраничных документов используйте `pages` параметр query, чтобы указать номера страниц или диапазоны страниц для извлечения текста только из этих страниц. В следующем примере показан документ с 10 страницами с текстом, извлеченным для обоих вариантов — все страницы (1-10) и выбранные страницы (3-6).

:::image border type="content" source="./Images/ocr-select-pages.png" alt-text="Выходные данные выбранных страниц":::

## <a name="supported-languages"></a>Поддерживаемые языки
API чтения поддерживают всего 73 языков для текста стиля печати. См. полный список языков, [поддерживаемых OCR](./language-support.md#optical-character-recognition-ocr). Рукописный текст в формате OCR поддерживается исключительно для английского языка.

## <a name="use-the-cloud-api-or-deploy-on-premise"></a>Использование облачного API или развертывание в локальной среде
Облачные API чтения 3. x являются предпочтительным вариантом для большинства клиентов из-за простоты интеграции и высокой продуктивности работы. Azure и служба Компьютерное зрение выполняют масштабирование, производительность, безопасность данных и соответствие требованиям, когда вы Сосредоточьтесь на соответствии потребностям ваших клиентов.

Для локального развертывания [контейнер DOCKER для чтения (Предварительная версия)](./computer-vision-how-to-install-containers.md) позволяет развертывать новые возможности OCR в собственной локальной среде. Контейнеры соответствуют конкретным требованиям к безопасности и управлению данными.

## <a name="ocr-api"></a>API OCR

[API OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f20d) использует старую модель распознавания, поддерживает только изображения и выполняется синхронно, возвращая немедленно с обнаруженным текстом. См. [языки, поддерживаемые OCR](./language-support.md#optical-character-recognition-ocr) , и API чтения.

> [!NOTE]
> Операции Рекогнизетекст Computer Висон 2,0 в данный процесс являются нерекомендуемыми в пользу нового API для чтения, рассмотренного в этой статье. Существующие клиенты должны [Перейти на использование операций чтения](upgrade-api-versions.md).

## <a name="next-steps"></a>Следующие шаги

- Приступая к работе с [краткими руководствами по Компьютерное зрение REST API или клиентской библиотеке](./quickstarts-sdk/client-library.md).
- Дополнительные сведения о [REST API Read 3,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005).
- Узнайте больше о [REST API общедоступной предварительной версии 3,2](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005) с поддержкой всего 73 языков.
