---
title: Создание семантического запроса
titleSuffix: Azure Cognitive Search
description: Задайте семантический тип запроса, чтобы присоединить модели глубокого обучения к обработке запросов, выложить намерение и контекст в рамках ранжирования и релевантности поиска.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 7551ef88c2251b64cf6f6db1de4fed22db2c69e2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101693651"
---
# <a name="create-a-semantic-query-in-cognitive-search"></a>Создание семантического запроса в Когнитивный поиск

> [!IMPORTANT]
> Тип семантического запроса — общедоступная Предварительная версия, доступная в предварительной версии REST API и портал Azure. Предварительные версии функций предлагаются "как есть" в [дополнительных условиях использования](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Во время первоначального запуска предварительной версии плата за семантический поиск не взимается. Дополнительные сведения см. в разделе [доступность и цены](semantic-search-overview.md#availability-and-pricing).

Из этой статьи вы узнаете, как сформулировать запрос поиска, использующий семантическое ранжирование, и создаст семантические заголовки и ответы.

## <a name="prerequisites"></a>Предварительные требования

+ Служба поиска на уровне "Стандартный" (S1, S2, S3), расположенная в одном из следующих регионов: северо-центральная часть США, Запад США и Западная часть США 2, Восточная часть США 2, Северная Европа, Западная Европа. При наличии существующей службы S1 или более поздней версии в одном из этих регионов вы можете запросить доступ без создания новой службы.

+ Доступ к предварительному просмотру семантического поиска: [Регистрация](https://aka.ms/SemanticSearchPreviewSignup)

+ Существующий индекс поиска, содержащий англоязычные материалы

+ Клиент поиска для отправки запросов

  Клиент поиска должен поддерживать предварительный просмотр API-интерфейсов RESTFUL в запросе. Вы можете использовать [POST](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md)или код, который вы изменили, чтобы вызовы интерфейса API для просмотра были вызваны другими компонентами. Можно также использовать [Обозреватель поиска](search-explorer.md) в портал Azure для отправки семантического запроса.

+ Запрос на [Поиск документов](/rest/api/searchservice/preview-api/search-documents) с параметром семантики и другими параметрами, описанными в этой статье.

## <a name="whats-a-semantic-query"></a>Что такое семантический запрос?

В Когнитивный поиск запрос представляет собой параметризованный запрос, который определяет обработку запросов и форму ответа. *Семантический запрос* добавляет параметры, которые вызывают алгоритм семантического ранжирования, который может оценивать контекст и значение соответствующих результатов, а также повышать соответствующие соответствия в верхней части.

Следующий запрос является образцом базового семантического запроса (без ответов).

```http
POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=2020-06-30-Preview      
{    
    "search": " Where was Alan Turing born?",    
    "queryType": "semantic",  
    "searchFields": "title,url,body",  
    "queryLanguage": "en-us",  
}
```

Как и в случае со всеми запросами в Когнитивный поиск, запрос обращается к коллекции Documents одного индекса. Более того, семантический запрос проходит ту же последовательность синтаксического анализа, анализа и сканирования, что и несемантический запрос. Разница заключается в порядке вычисления релевантности. Как определено в этом предварительном выпуске, семантический запрос — это тот, для которого *результаты* повторно обрабатываются с помощью расширенных алгоритмов, предоставляя способ отображения совпадений, наиболее релевантных для семантического ранжирования, а не оценок, назначенных алгоритмом ранжирования сходства по умолчанию. 

Только первые 50 соответствий от первоначальных результатов могут быть семантически ранжированы, а все включаемые заголовки в ответе. При необходимости можно указать **`answer`** параметр запроса, чтобы извлечь потенциальный ответ. Эта модель формирует до пяти возможных ответов на запрос, которые можно выбрать для отображения в верхней части страницы поиска.

## <a name="query-using-rest-apis"></a>Запрос с помощью API-интерфейсов RESTFUL

Полную спецификацию REST API можно найти в [документах поиска (Предварительная версия](/rest/api/searchservice/preview-api/search-documents)для дома).

Семантические запросы предназначены для вопросов с открытыми запросами, например "что такое лучшее предприятие для поллинаторс" или "как фри Egg". Если вы хотите, чтобы ответ включал ответы, можно добавить в запрос необязательный **`answer`** параметр.

В следующем примере для создания семантического запроса с семантическими ответами и заголовками используется Гостиница-Sample-index:

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview      
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "searchFields": "HotelName,Category,Description",
    "speller": "lexicon",
    "answers": "extractive|count-3",
    "highlightPreTag": "<strong>",
    "highlightPostTag": "</strong>",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

### <a name="formulate-the-request"></a>Формулировка запроса

1. Задайте значение **`"queryType"`** "семантика" и **`"queryLanguage"`** "en-US". Оба параметра являются обязательными.

   Куерилангуаже должен соответствовать любым [анализаторам языка](index-add-language-analyzers.md) , назначенным определениям полей в схеме индекса. Если Куерилангуаже имеет значение en-US, то все анализаторы языка должны также иметь английский вариант ("en. Microsoft" или "en. Lucene"). Анализаторы, не зависящие от языка, такие как ключевое слово или Simple, не конфликтуют со значениями Куерилангуаже.

   В запросе, если вы также используете [исправление правописания](speller-how-to-add.md), заданный куерилангуаже применяется к проверке орфографии, ответам и заголовкам. Для отдельных частей переопределение не предусмотрено. 

   Хотя содержимое в индексе поиска может состоять из нескольких языков, входные данные запроса, скорее всего, находятся в одном. Поисковая система не проверяет совместимость Куерилангуаже, анализатор языка и язык, на котором состоит содержимое, поэтому обязательно выполняйте запросы соответствующим образом, чтобы избежать получения неверных результатов.

<a name="searchfields"></a>

1. Задать **`"searchFields"`** (необязательно, но рекомендуется).

   В семантическом запросе порядок полей в "searchFields" отражает приоритет или относительную важность поля в семантических ранжированиях. Будут использоваться только поля строк верхнего уровня (автономные или в коллекции). Поскольку searchFields имеет другие поведения в простых и полных запросах Lucene (без подразумеваемого порядка приоритетов), любые нестроковые поля и вложенные поля не будут приводить к ошибке, но они также не будут использоваться в семантическом ранжировании.

   При указании searchFields следуйте приведенным ниже рекомендациям.

   + Краткие поля, такие как Хотелнаме или Title, должны предшествовать полям с подробными сведениями, например Description.

   + Если в индексе есть поле URL-адреса, которое является текстовым (например `www.domain.com/name-of-the-document-and-other-details` , и не имеет компьютера `www.domain.com/?id=23463&param=eis` , например), вставьте его вторым в списке (сначала вставьте его, если нет поля с кратким названием).

   + Если указано только одно поле, оно будет рассматриваться как описательное поле для семантического ранжирования документов.  

   + Если поля не указаны, то для семантического ранжирования документов будут учитываться все поля, доступные для поиска. Однако это не рекомендуется, так как оно может не дать наиболее оптимальных результатов из индекса поиска.

1. Удалите **`"orderBy"`** предложения, если они существуют в существующем запросе. Семантическая Оценка используется для упорядочивания результатов и при включении явной логики сортировки возвращается ошибка HTTP 400.

1. При необходимости добавьте параметр **`"answers"`** Set в значение "извлечение" и укажите количество ответов, если требуется больше 1.

1. При необходимости можно настроить стиль выделения, применяемый к заголовкам. Субтитры применяют форматирование выделения для ключевых фрагментов в документе, который обобщает ответ. Значение по умолчанию — `<em>`. Если необходимо указать тип форматирования (например, желтый фон), можно задать параметры Хигхлигхтпретаг и Хигхлигхтпосттаг.

1. Укажите любые другие параметры, которые должны быть в запросе. Такие параметры, как средство [проверки орфографии](speller-how-to-add.md), [SELECT](search-query-odata-select.md)и Count, улучшают качество запроса и удобочитаемость ответа.

### <a name="review-the-response"></a>Просмотр ответа

Ответ для приведенного выше запроса возвращает следующее совпадение в качестве верхнего подбора. Заголовки возвращаются автоматически, с обычным текстом и выделенными версиями. Дополнительные сведения об семантических ответах см. в разделе [семантическая ранжирование и ответы](semantic-how-to-query-response.md).

```json
"@odata.count": 29,
"value": [
    {
        "@search.score": 1.8920634,
        "@search.rerankerScore": 1.1091284966096282,
        "@search.captions": [
            {
                "text": "Oceanside Resort. Budget. New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
                "highlights": "<strong>Oceanside Resort.</strong> Budget. New Luxury Hotel. Be the first to stay.<strong> Bay views</strong> from every room, location near the pier, rooftop pool, waterfront dining & more."
            }
        ],
        "HotelId": "18",
        "HotelName": "Oceanside Resort",
        "Description": "New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
        "Category": "Budget"
    },
```

### <a name="parameters-used-in-a-semantic-query"></a>Параметры, используемые в семантическом запросе

В следующей таблице перечислены параметры запроса, используемые в семантическом запросе, чтобы их можно было увидеть как целостные. Список всех параметров см. в разделе [Поиск документов (Предварительная версия для просмотра)](/rest/api/searchservice/preview-api/search-documents)

| Параметр | Тип | Описание |
|-----------|-------|-------------|
| queryType | Строка | Допустимые значения: простая, полная и семантическая. Для семантических запросов требуется значение "семантика". |
| куерилангуаже | Строка | Требуется для семантических запросов. В настоящее время реализован только "en-US". |
| searchFields | Строка | Разделенный запятыми список полей с возможностью поиска. Необязательно, но рекомендуется. Указывает поля, для которых выполняется семантическое ранжирование. </br></br>В отличие от простых и полных типов запросов, порядок, в котором перечислены поля, определяет приоритет.|
| Вас |Строка | Необязательное поле для указания того, включены ли семантические ответы в результат. В настоящее время реализуется только "извлечение". Ответы можно настроить так, чтобы они возвращали не более пяти. Этот пример "извлечение|Count3 "" показывает число трех ответов. Значение по умолчанию — 1.|

## <a name="query-with-search-explorer"></a>Подача запросов с использованием обозревателя поиска

Следующий запрос обращается к встроенному индексу образца гостиниц, с помощью API версии 2020-06-30-Preview и выполняется в обозревателе поиска. `$select`Предложение ограничивает результаты только небольшими полями, что упрощает поиск в подробном JSON в обозревателе поиска.

### <a name="with-querytypesemantic"></a>С queryType = семантика

```json
search=I want a nice hotel on the water with a great restaurant&$select=HotelId,HotelName,Description,Tags&queryType=semantic&queryLanguage=english&searchFields=Description,Tags
```

Первые несколько результатов выглядят следующим образом.

```json
{
    "@search.score": 0.38330218,
    "@search.rerankerScore": 0.9754053303040564,
    "HotelId": "18",
    "HotelName": "Oceanside Resort",
    "Description": "New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
    "Tags": [
        "view",
        "laundry service",
        "air conditioning"
    ]
},
{
    "@search.score": 1.8920634,
    "@search.rerankerScore": 0.8829904259182513,
    "HotelId": "36",
    "HotelName": "Pelham Hotel",
    "Description": "Stunning Downtown Hotel with indoor Pool. Ideally located close to theatres, museums and the convention center. Indoor Pool and Sauna and fitness centre. Popular Bar & Restaurant",
    "Tags": [
        "view",
        "pool",
        "24-hour front desk service"
    ]
},
{
    "@search.score": 0.95706713,
    "@search.rerankerScore": 0.8538530203513801,
    "HotelId": "22",
    "HotelName": "Stone Lion Inn",
    "Description": "Full breakfast buffet for 2 for only $1.  Excited to show off our room upgrades, faster high speed WiFi, updated corridors & meeting space. Come relax and enjoy your stay.",
    "Tags": [
        "laundry service",
        "air conditioning",
        "restaurant"
    ]
},
```

### <a name="with-querytype-default"></a>С queryType (по умолчанию)

Для сравнения выполните тот же запрос, что и выше, удалив `&queryType=semantic&queryLanguage=english&searchFields=Description,Tags` . Обратите внимание, что `"@search.rerankerScore"` в этих результатах нет, и что разные Гостиницы отображаются в трех верхних позициях.

```json
{
    "@search.score": 8.633856,
    "HotelId": "3",
    "HotelName": "Triple Landscape Hotel",
    "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
    "Tags": [
        "air conditioning",
        "bar",
        "continental breakfast"
    ]
},
{
    "@search.score": 6.407289,
    "HotelId": "40",
    "HotelName": "Trails End Motel",
    "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
    "Tags": [
        "continental breakfast",
        "view",
        "view"
    ]
},
{
    "@search.score": 5.843788,
    "HotelId": "14",
    "HotelName": "Twin Vertex Hotel",
    "Description": "New experience in the Making.  Be the first to experience the luxury of the Twin Vertex. Reserve one of our newly-renovated guest rooms today.",
    "Tags": [
        "bar",
        "restaurant",
        "air conditioning"
    ]
},
```

## <a name="next-steps"></a>Дальнейшие действия

Помните, что семантические ранжирование и ответы создаются на основе исходного результирующего набора. Любая логика, улучшающая качество первоначальных результатов, будет переноситься в семантический поиск. В качестве следующего шага просмотрите функции, которые вносят первоначальные результаты, включая анализаторы, которые влияют на разметку строк, профили оценки, которые могут настраивать результаты, и алгоритм соответствия по умолчанию.

+ [Анализаторы для обработки текста](search-analyzers.md)
+ [Сходство и оценка в Когнитивный поиск](index-similarity-and-scoring.md)
+ [Добавление профилей повышения](index-add-scoring-profiles.md)
+ [Обзор семантического поиска](semantic-search-overview.md)
+ [Добавление проверки орфографии в термины запроса](speller-how-to-add.md)
