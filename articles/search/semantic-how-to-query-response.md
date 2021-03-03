---
title: Структурирование семантического ответа
titleSuffix: Azure Cognitive Search
description: Описывает алгоритм семантического ранжирования в Когнитивный поиск и способы структурирования семантических ответов и семантических заголовков из результирующего набора.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: febbfd0f3def8e681107ef78d9478463b1c2a872
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680402"
---
# <a name="semantic-ranking-and-responses-in-azure-cognitive-search"></a>Семантические ранжирование и ответы в Azure Когнитивный поиск

> [!IMPORTANT]
> Алгоритм семантического ранжирования и семантические ответы и подписи находятся в общедоступной предварительной версии, доступной только в предварительной версии REST API. Предварительные версии функций предлагаются "как есть" в [дополнительных условиях использования](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Семантический рейтинг повышает точность результатов поиска, переранжирование верхних соответствий с помощью модели семантического ранжирования, обученной для запросов, выраженных на естественном языке, а не ключевых слов.

В этой статье описывается алгоритм семантического ранжирования и способ формирования семантического ответа. Ответ содержит заголовки как в виде обычного текста, так и с выделением и ответы (необязательно).

+ Семантические заголовки — это текстовые фрагменты, относящиеся к запросу, извлеченному из результатов поиска. Они могут помочь в суммировании результатов, если отдельные поля содержимого слишком велики для страницы результатов. Заголовков — семантические функции, позволяющие пользователям быстро считывает результаты запроса, чтобы найти наиболее релевантные документы, таким образом улучшая общий пользовательский интерфейс.

+ Семантические ответы используют модели машинного обучения из Bing для формулировки ответов на запросы, которые выглядят как вопросы. Ответы выбираются из списка передаваемых данных, наиболее релевантных для запроса, как извлечено из верхних документов в результирующем наборе запроса. Ответы возвращаются в виде независимого объекта верхнего уровня в полезных данных ответа на запрос, который можно выбрать для отображения на страницах поиска, а также в результатах поиска.

## <a name="prerequisites"></a>Предварительные требования

+ Запросы, которые были обработаны с помощью семантического типа запроса. Дополнительные сведения см. [в разделе Создание семантического запроса](semantic-how-to-query-request.md).

## <a name="understanding-a-semantic-response"></a>Основные сведения о семантическом ответе

Семантический ответ включает новые свойства для оценок, заголовков и ответов. Семантический ответ строится на основе стандартного ответа с использованием первых 50 результатов, возвращенных [механизмом полнотекстового поиска](search-lucene-query-architecture.md), который затем перестраивается с помощью семантического ранжирования. Если указано более 50, возвращаются дополнительные результаты, но они не будут семантически переупорядочены.

Как и во всех запросах, ответ состоит из всех полей, помеченных как доступные для извлечения, или только для полей, перечисленных в инструкции SELECT. Он также содержит поле "ответ" и "заголовки".

+ Для каждого семантического результата по умолчанию используется один ответ, который возвращается в виде отдельного поля, которое можно выбрать для отображения на странице поиска. Можно указать до пяти. Формулировку ответа автоматизирован: чтение всех документов в начальных результатах, выполнение изученой сводки, а также считывание данных с компьютера и, наконец, повышение прямого ответа на вопрос пользователя в поле ответа.

+ "Заголовок" — это сводка содержимого документа на основе извлечения, возвращаемая в виде обычного текста или с выделенными фрагментами. Субтитры включаются автоматически и не могут быть подавлены. Для указания того, какие строки должны быть выделены, применяются основные элементы с помощью функции чтения с компьютера. Подсветка позволяет привлечь внимание к наиболее релевантным деталям, чтобы можно было быстро просмотреть страницу результатов, чтобы найти нужный документ.

Семантический повторно ранжированный результирующий набор упорядочивает результаты по @search.rerankerScore значению. Если добавить предложение OrderBy, будет возвращена ошибка HTTP 400, так как это предложение не поддерживается в семантическом запросе.

## <a name="example"></a>Пример

Объект @search.rerankerScore существует рядом со стандартом @search.score и используется для повторного ранжирования результатов.

С учетом следующего запроса:

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "answers": "none",
    "searchFields": "HotelName,Category,Description",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

Вы можете увидеть следующий результат, репрезентативный семантический ответ. Эти результаты показывают только три первых ответа, упорядоченные по " @search.rerankerScore ". Обратите внимание на то, как Оцеансиденое курорт теперь ранжировано в первую очередь. При ранжировании по умолчанию " @search.score " Этот результат будет возвращен на втором месте после окончания следа.

```http
{
    "@odata.count": 31,
    "@search.answers": [],
    "value": [
        {
            "@search.score": 1.8920634,
            "@search.rerankerScore": 1.1091284966096282,
            "@search.captions": [
                {
                    "text": "Oceanside Resort. Budget. New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the piper, rooftop pool, waterfront dining & more.",
                    "highlights": "<em>Oceanside Resort.</em> Budget. New Luxury Hotel.  Be the first to stay.<em> Bay views</em> from every room, location near the piper, rooftop pool, waterfront dining & more."
                }
            ],
            "HotelId": "18",
            "HotelName": "Oceanside Resort",
            "Description": "New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the piper, rooftop pool, waterfront dining & more.",
            "Category": "Budget"
        },
        {
            "@search.score": 2.5204072,
            "@search.rerankerScore": 1.0731962407007813,
            "@search.captions": [
                {
                    "text": "Trails End Motel. Luxury. Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
                    "highlights": "<em>Trails End Motel.</em> Luxury. Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport."
                }
            ],
            "HotelId": "40",
            "HotelName": "Trails End Motel",
            "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
            "Category": "Luxury"
        },
        {
            "@search.score": 1.4104348,
            "@search.rerankerScore": 1.06992666143924,
            "@search.captions": [
                {
                    "text": "Winter Panorama Resort. Resort and Spa. Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs.",
                    "highlights": "<em>Winter Panorama Resort</em>. Resort and Spa. Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs."
                }
            ],
            "HotelId": "12",
            "HotelName": "Winter Panorama Resort",
            "Description": "Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs.",
            "Category": "Resort and Spa"
        }
```

## <a name="next-steps"></a>Дальнейшие действия

+ [Обзор семантического поиска](semantic-search-overview.md)
+ [Алгоритм проверки сходства](index-ranking-similarity.md)
+ [Создание семантического запроса](semantic-how-to-query-request.md)
+ [Создание простого запроса](search-query-create.md)