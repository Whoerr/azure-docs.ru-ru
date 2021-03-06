---
title: Лимиты и границы —QnA Maker
description: Для QnA Maker предусмотрены метаограничения для частей базы знаний и службы. Для тестирования и публикации важно хранить базу знаний, учитывая эти ограничения.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 11/09/2020
ms.openlocfilehash: 1e57ae537c271e61f0b2d37f5320cb177b04802b
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98164878"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Лимиты и границы базы данных QnA Maker

Приведенные ниже ограничения QnA Maker являются сочетанием [ограничений ценовой категории Azure когнитивный Поиск](../../search/search-limits-quotas-capacity.md) и [QnA Makerными ограничениями ценовой](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)категории. Необходимо знать оба набора ограничений, чтобы понять, сколько баз знаний можно создать для каждого ресурса, а также сколько может расти каждая база знаний.

## <a name="knowledge-bases"></a>Базы знаний

Максимальное число баз знаний основано на [ограничениях уровня когнитивный Поиск Azure](../../search/search-limits-quotas-capacity.md).

|**Уровень Когнитивный поиск Azure** | **Бесплатно** | **Основной** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Максимальное разрешенное число опубликованных баз знаний|2|14|49|199|199|2 999|

 Например, если для уровня допускается 15 индексов, можно опубликовать 14 баз знаний (1 индекс для каждой опубликованной базы знаний). Пятнадцатый индекс (`testkb`) используется для разработки и тестирования всех баз знаний.

## <a name="extraction-limits"></a>Ограничения на извлечение

### <a name="file-naming-constraints"></a>Ограничения именования файлов

Имена файлов не могут содержать следующие символы:

|Не использовать символ|
|--|
|Одинарная кавычка `'`|
|Двойная кавычка `"`|

### <a name="maximum-file-size"></a>Максимальный размер файла

|Формат|Максимальный размер файла (МБ)|
|--|--|
|`.docx`|10|
|`.pdf`|25|
|`.tsv`|10|
|`.txt`|10|
|`.xlsx`|3|

### <a name="maximum-number-of-files"></a>Максимальное число файлов

Максимальное число файлов, которые можно извлечь, и максимальный размер файла основаны на **[QnA Makerных ограничениях ценовой](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** категории.

> [!NOTE]
> QnA Maker Managed (Предварительная версия) — это бесплатная служба без ограничений на количество источников, которые можно добавить. Пропускная способность в настоящее время ограничена 10 транзакциями в секунду для API-интерфейсов управления и прогнозирующих API-интерфейсов.

### <a name="maximum-number-of-deep-links-from-url"></a>Максимальное число глубоких ссылок из URL-адреса

Максимальное число глубоких ссылок, которые можно просканировать для извлечения QnA из страницы URL-адреса, равно **20**.

## <a name="metadata-limits"></a>Ограничения на метаданные

Метаданные представлены в виде текстовой пары "ключ — значение", например `product:windows 10` . Он сохраняется и сравнивается в нижнем регистре.

### <a name="by-azure-cognitive-search-pricing-tier"></a>По ценовой категории Azure Когнитивный поиск

Максимальное число полей метаданных на базу знаний основано на **[ограничениях уровня когнитивный Поиск Azure](../../search/search-limits-quotas-capacity.md)**.

|**Уровень Когнитивный поиск Azure** | **Бесплатно** | **Основной** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Максимальное число полей метаданных на службу QnA Maker (для всех баз знаний)|1000|100*|1000|1000|1000|1000|

### <a name="by-name-and-value"></a>По имени и значению

В следующей таблице перечислены Длина и допустимые символы для имени и значения метаданных.

|Элемент|Допустимые символы|Соответствие шаблону регулярного выражения|Максимальное число знаков|
|--|--|--|--|
|Имя (ключ)|Здесь<br>буквенно-цифровые (буквы и цифры)<br>`_` подчеркивания<br> Не должно содержать пробелов.|`^[a-zA-Z0-9_]+$`|100|
|Значение|Разрешает все, Кроме<br>`:` ставит<br>`|` (вертикальный канал)<br>Разрешено только одно значение.|`^[^:|]+$`|500|
|||||

## <a name="knowledge-base-content-limits"></a>Ограничения на содержимое базы знаний
Общие ограничения на содержимое в базе знаний
* Длина текста ответа: 25 000 символов
* Длина текста вопроса: 1 000 символов
* Длина текста ключа метаданных: 100 символов
* Длина текста значения метаданных: 500 символов
* Поддерживаемые символы для имени метаданных: алфавиты, цифры и `_`
* Поддерживаемые символы для значения метаданных: все `:` , кроме и `|`
* Длина имени файла: 200
* Поддерживаемые форматы файлов: TSV, PDF, TXT, DOCX, XLSX.
* Максимальное число альтернативных вопросов: 300
* Максимальное число пар "вопрос — ответ". зависит от выбранного **[уровня когнитивный Поиск Azure](../../search/search-limits-quotas-capacity.md#document-limits)** . Пара вопросов и ответов сопоставляется с документом по индексу Azure Когнитивный поиск.
* URL-адрес страницы HTML: 1 000 000 символов

## <a name="create-knowledge-base-call-limits"></a>Ограничения на вызовы создания базы знаний
Эти значения представляют ограничения для каждого действия создания базы знаний, то есть нажатия кнопки *Создать базу знаний* или вызова API CreateKnowledgeBase.
* Рекомендуемое максимальное число альтернативных вопросов на один ответ: 300
* Максимальное число URL-адресов: 10
* Максимальное число файлов: 10
* Максимальное число QnA, разрешенное для одного вызова: 1000

## <a name="update-knowledge-base-call-limits"></a>Ограничения на вызовы обновления базы знаний
Эти значения представляют ограничения для каждого действия обновления, то есть нажатия кнопки *Сохранить и обучить* или вызова API UpdateKnowledgeBase.
* Длина имени каждого источника: 300
* Рекомендуемое максимальное количество альтернативных вопросов, добавленных или удаленных: 300
* Максимальное число добавленных или удаленных полей метаданных: 10
* Максимальное число URL-адресов, которые можно обновить: 5
* Максимальное число QnA, разрешенное для одного вызова: 1000

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, когда и как изменить [ценовые категории служб](How-To/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku).
