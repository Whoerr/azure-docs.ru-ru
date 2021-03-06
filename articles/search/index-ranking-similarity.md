---
title: Настройка алгоритма подобия рангов
titleSuffix: Azure Cognitive Search
description: Как задать алгоритм подобия, чтобы опробовать новый алгоритм подобия для ранжирования
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 9f806b512ae8e118fca8f32115c8be3b493fd681
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101677781"
---
# <a name="configure-ranking-algorithms-in-azure-cognitive-search"></a>Настройка алгоритмов ранжирования в Azure Когнитивный поиск

Azure Когнитивный поиск поддерживает два алгоритма ранжирования подобия:

+ *Классический алгоритм подобия* , используемый всеми службами поиска вплоть до 15 июля 2020.
+ Реализация алгоритма *ОКАПИ BM25* , используемая во всех службах поиска, созданных после 15 июля.

BM25 ранжирование — это новое значение по умолчанию, так как оно создает ранжирование поиска, которое лучше всего соответствует ожиданиям пользователей. Он также включает параметры конфигурации для результатов настройки на основе таких факторов, как размер документа. Для новых служб, созданных после 15 июля 2020, BM25 используется автоматически и является единственным алгоритмом подобия. Если вы попытаетесь задать подобие для Классиксимиларити в новой службе, будет возвращена ошибка HTTP 400, так как этот алгоритм не поддерживается службой.

Для старых служб, созданных до 15 июля 2020, классическое сходство остается алгоритмом по умолчанию. Более старые службы могут задавать свойства индекса поиска для вызова BM25, как описано ниже. Если вы переходите с классической модели на BM25, вы можете увидеть некоторые отличия порядка результатов поиска.

> [!NOTE]
> Семантический поиск — это дополнительный алгоритм семантического ранжирования, который ограничивает зазор между ожиданиями и результатами еще больше. В отличие от других алгоритмов, это надстройка, которая выполняет итерацию по существующему результирующему набору. Чтобы использовать алгоритм предварительного просмотра семантического поиска, необходимо создать новую службу и указать [семантический тип запроса](semantic-how-to-query-request.md). Дополнительные сведения см. в разделе [Общие сведения о семантическом поиске](semantic-search-overview.md).

## <a name="create-a-search-index-for-bm25-scoring"></a>Создание индекса поиска для оценки BM25

Если вы используете службу поиска, которая была создана до 15 июля 2020, можно задать для свойства подобия значение BM25Similarity или Классиксимиларити в определении индекса. Если свойство подобия опущено или имеет значение null, индекс использует классический алгоритм.

Алгоритм подобия может быть установлен только во время создания индекса. Однако после создания индекса с помощью BM25 можно обновить существующий индекс, чтобы задать или изменить параметры BM25.

| Клиентская библиотека | Свойство подобия |
|----------------|---------------------|
| .NET  | [Сеарчиндекс. подобие](/dotnet/api/azure.search.documents.indexes.models.searchindex.similarity) |
| Java | [Сеарчиндекс. Сетсимиларити](/java/api/com.azure.search.documents.indexes.models.searchindex.setsimilarity) |
| JavaScript | [Сеарчиндекс. подобие](/javascript/api/@azure/search-documents/searchindex#similarity) |
| Python | [Свойство подобия для Сеарчиндекс](/python/api/azure-search-documents/azure.search.documents.indexes.models.searchindex) |

### <a name="rest-example"></a>Пример REST

Можно также использовать [REST API](/rest/api/searchservice/create-index), как показано в следующем примере:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2020-06-30
{
    "name": "indexName",
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": true
        },
        {
            "name": "name",
            "type": "Edm.String",
            "searchable": true,
            "analyzer": "en.lucene"
        },
        ...
    ],
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity"
    }
}
```

## <a name="bm25-similarity-parameters"></a>Параметры подобия BM25

BM25 подобие добавляет два настраиваемых пользователем параметра для управления вычисленным показателем релевантности. Параметры BM25 можно задать во время создания индекса или как обновление индекса, если во время создания индекса был указан алгоритм BM25.

| Свойство | Тип | Описание |
|----------|------|-------------|
| k1 | number | Управляет функцией масштабирования между термином частота соответствующих терминов и конечной показателем релевантности пары "документ-запрос". Обычно значения 0,0 – 3,0, а для по умолчанию — 1,2. </br></br>Значение 0,0 представляет "двоичную модель", где вклад одного совпадающего термина одинаков для всех соответствующих документов независимо от того, сколько раз этот термин встречается в тексте, а более крупное K1 значение позволяет продолжить увеличение по мере того, как в документе будет найдено больше экземпляров одного и того же термина. </br></br>Использование более высокого значения K1 может быть важным в случаях, когда предполагается, что несколько терминов будут входить в поисковый запрос. В таких случаях может потребоваться предпочитать документы, которые соответствуют многим различным терминам запроса, которые ищутся в документах, которые соответствуют только одному из них, несколько раз. Например, при запросе индекса для документов, содержащих термины «Apollo Спацефлигхт», нам может потребоваться уменьшить оценку статьи о греческом Мифологиие, содержащей слово «Apollo», в несколько десятков раз, без упоминания «Спацефлигхт» по сравнению с другой статьей, в которой явно упоминается «Apollo» и «Спацефлигхт» несколько раз. |
| b | number | Определяет, как длина документа влияет на показатель релевантности. Значения находятся в диапазоне от 0 до 1, по умолчанию 0,75. </br></br>Значение 0,0 означает, что длина документа не будет влиять на оценку, а значение 1,0 означает, что влияние частоты терминов на показатель релевантности будет нормализовано по длине документа. </br></br>Нормализация тактовой частоты по длине документа полезна в случаях, когда нам нужно пенализе более длинные документы. В некоторых случаях более длинные документы (например, полный роман), скорее всего, будут содержать множество незначимых терминов по сравнению с более короткими документами. |

### <a name="setting-k1-and-b-parameters"></a>Настройка параметров K1 и b

Чтобы задать или изменить значения b или K1, добавьте их в объект подобия BM25. При установке или изменении этих значений в существующем индексе индекс будет переведен в автономный режим по крайней мере через несколько секунд, что приведет к сбою активных запросов на индексирование и запросы. Следовательно, необходимо задать параметр "Алловиндексдовнтиме = true" для запроса на обновление:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2020-06-30&allowIndexDowntime=true
{
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity",
        "b" : 0.5,
        "k1" : 1.3
    }
}
```

## <a name="see-also"></a>См. также раздел  

+ [Справочник по REST API](/rest/api/searchservice/)
+ [Добавление профилей оценки в индекс](index-add-scoring-profiles.md)
+ [Создание API индекса](/rest/api/searchservice/create-index)
+ [Библиотеки службы "Поиск Azure" для .NET](/dotnet/api/overview/azure/search)