---
title: Управление индексированием в API Azure Cosmos DB для MongoDB
description: В этой статье представлен обзор возможностей индексирования Azure Cosmos DB с помощью API Azure Cosmos DB для MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: how-to
ms.date: 03/02/2021
author: timsander1
ms.author: tisande
ms.custom: devx-track-js
ms.openlocfilehash: 8d19a5dadffdfa26ccb2d84e6dab278ad272c7b0
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658052"
---
# <a name="manage-indexing-in-azure-cosmos-dbs-api-for-mongodb"></a>Управление индексированием в API Azure Cosmos DB для MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

API Azure Cosmos DB для MongoDB использует преимущества основных возможностей управления индексами Azure Cosmos DB. Эта статья посвящена добавлению индексов с помощью API Azure Cosmos DB для MongoDB. Вы также можете ознакомиться с [обзором индексирования в Azure Cosmos DB](index-overview.md) , которые относятся ко всем API.

## <a name="indexing-for-mongodb-server-version-36-and-higher"></a>Индексирование для сервера MongoDB версии 3,6 и более поздних версий

API Azure Cosmos DB для сервера MongoDB версии 3.6 + автоматически индексирует `_id` поле, которое не может быть удалено. Он автоматически обеспечивает уникальность `_id` поля для каждого ключа сегмента. В API Azure Cosmos DB для MongoDB сегментирование и индексирование являются отдельными концепциями. Вам не нужно индексировать ключ сегмента. Однако, как и в случае с любым другим свойством в документе, если это свойство является общим фильтром в запросах, рекомендуется индексировать ключ сегмента.

Для индексирования дополнительных полей применяются команды управления индексами MongoDB. Как и в MongoDB, API Azure Cosmos DB для MongoDB автоматически индексирует `_id` только поле. Эта политика индексирования по умолчанию отличается от API SQL для Azure Cosmos DB, где по умолчанию индексируются все поля.

Чтобы применить сортировку к запросу, необходимо создать индекс для полей, используемых в операции сортировки.

### <a name="editing-indexing-policy"></a>Изменение политики индексации

Рекомендуется изменить политику индексирования в обозреватель данных в портал Azure.
. В обозреватель данных можно добавить отдельные поля и индексы с подстановочными знаками из редактора политики индексирования:

:::image type="content" source="./media/mongodb-indexing/indexing-policy-editor.png" alt-text="Редактор политики индексации":::

> [!NOTE]
> Нельзя создавать составные индексы с помощью редактора политики индексирования в обозреватель данных.

## <a name="index-types"></a>Типы индексов

### <a name="single-field"></a>Одно поле

Индексы можно создавать для любого одного поля. Порядок сортировки индекса одного поля не имеет значения. Следующая команда создает индекс для поля `name` :

`db.coll.createIndex({name:1})`

В портал Azure можно создать один и тот же индекс одного поля `name` :

:::image type="content" source="./media/mongodb-indexing/add-index.png" alt-text="Добавление индекса имени в редакторе политики индексирования":::

Один запрос использует несколько индексов одного поля, где это возможно. Для одного контейнера можно создать до 500 однозначных индексов полей.

### <a name="compound-indexes-mongodb-server-version-36"></a>Составные индексы (MongoDB Server версии 3.6 +)

API Azure Cosmos DB для MongoDB поддерживает составные индексы для учетных записей, использующих протокол Wire версии 3,6 и 4,0. В составной индекс можно включить до восьми полей. В отличие от MongoDB, составной индекс следует создавать только в том случае, если запрос должен эффективно отсортировать несколько полей одновременно. Для запросов с несколькими фильтрами, которые не требуют сортировки, создайте несколько индексов отдельных полей вместо одного составного индекса. 

> [!NOTE]
> Нельзя создавать составные индексы для вложенных свойств или массивов.

Следующая команда создает составной индекс для полей `name` и `age` :

`db.coll.createIndex({name:1,age:1})`

Составные индексы можно использовать для эффективной сортировки нескольких полей одновременно, как показано в следующем примере:

`db.coll.find().sort({name:1,age:1})`

Можно также использовать предыдущий составной индекс для эффективной сортировки запроса с противоположным порядком сортировки для всех полей. Ниже приведен пример:

`db.coll.find().sort({name:-1,age:-1})`

Однако последовательность путей в составном индексе должна точно соответствовать запросу. Ниже приведен пример запроса, для которого требуется дополнительный составной индекс:

`db.coll.find().sort({age:1,name:1})`

> [!NOTE]
> Составные индексы используются только в запросах, которые сортируют результаты. Для запросов с несколькими фильтрами, которые не требуют сортировки, создайте несколько индексы одного поля.

### <a name="multikey-indexes"></a>Индексы многоключевые

Azure Cosmos DB создает индексы многоключевые для индексирования содержимого, хранящегося в массивах. При индексировании поля со значением массива Azure Cosmos DB автоматически индексирует каждый элемент массива.

### <a name="geospatial-indexes"></a>Геопространственные индексы

Многие геопространственные операторы получают преимущества от геопространственных индексов. В настоящее время API Azure Cosmos DB для MongoDB поддерживает `2dsphere` индексы. API пока не поддерживает `2d` индексы.

Ниже приведен пример создания геопространственного индекса для `location` поля.

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Текстовые индексы

API Azure Cosmos DB для MongoDB в настоящее время не поддерживает текстовые индексы. Для запросов текстового поиска в строках следует использовать интеграцию [Azure когнитивный Поиск](../search/search-howto-index-cosmosdb.md) с Azure Cosmos DB. 

## <a name="wildcard-indexes"></a>Подстановочные индексы

Для поддержки запросов к неизвестным полям можно использовать индексы с подстановочными знаками. Предположим, что у вас есть коллекция, содержащая данные о семействах.

Ниже приведена часть примера документа в этой коллекции:

```json
"children": [
   {
     "firstName": "Henriette Thaulow",
     "grade": "5"
   }
]
```

Вот еще один пример, на этот раз немного отличающийся набор свойств в `children` :

```json
"children": [
    {
     "familyName": "Merriam",
     "givenName": "Jesse",
     "pets": [
         { "givenName": "Goofy" },
         { "givenName": "Shadow" }
         ]
   },
   {
     "familyName": "Merriam",
     "givenName": "John",
   }
]
```

В этой коллекции документы могут иметь различные возможные свойства. Если вы хотите индексировать все данные в `children` массиве, у вас есть два варианта: создание отдельных индексов для каждого отдельного свойства или создание одного индекса с подстановочными знаками для всего `children` массива.

### <a name="create-a-wildcard-index"></a>Создание индекса с подстановочными знаками

Следующая команда создает индекс с подстановочными знаками для всех свойств в `children` :

`db.coll.createIndex({"children.$**" : 1})`

В **отличие от MongoDB, индексы с подстановочными знаками могут поддерживать несколько полей в предикатах запросов**. При использовании одного индекса вместо создания отдельного индекса для каждого свойства различие в производительности запросов не будет.

С помощью подстановочного знака можно создать следующие типы индексов:

* Одно поле
* Геопространственные данные

### <a name="indexing-all-properties"></a>Индексирование всех свойств

Вот как можно создать индекс с подстановочными знаками для всех полей:

`db.coll.createIndex( { "$**" : 1 } )`

Можно также создать индексы с подстановочными знаками, используя обозреватель данных в портал Azure:

:::image type="content" source="./media/mongodb-indexing/add-wildcard-index.png" alt-text="Добавление подстановочного индекса в редактор политики индексирования":::

> [!NOTE]
> Если вы только начинаете разработку, мы **настоятельно** рекомендуем начинать с подстановочного индекса для всех полей. Это может упростить разработку и упростить оптимизацию запросов.

В документах со многими полями может быть высокая стоимость единиц запросов (RU) для операций записи и обновления. Поэтому при наличии рабочей нагрузки, интенсивно использующей запись, следует использовать по отдельности индексы, в отличие от использования индексов с подстановочными знаками.

### <a name="limitations"></a>Ограничения

Индексы с подстановочными знаками не поддерживают следующие типы или свойства индекса:

* Составные
* СРОК ЖИЗНИ
* Уникальная идентификация

В **отличие от MongoDB**, в API Azure Cosmos DB для MongoDB **нельзя** использовать подстановочные индексы для:

* создания индекса с подстановочными знаками, содержащего несколько конкретных полей;

  ```json
  db.coll.createIndex(
      { "$**" : 1 },
      { "wildcardProjection " :
          {
             "children.givenName" : 1,
             "children.grade" : 1
          }
      }
  )
  ```

* создания индекса с подстановочными знаками, не содержащего несколько конкретных полей.

  ```json
  db.coll.createIndex(
      { "$**" : 1 },
      { "wildcardProjection" :
          {
             "children.givenName" : 0,
             "children.grade" : 0
          }
      }
  )
  ```

В качестве альтернативы можно создать несколько индексов с подстановочными знаками.

## <a name="index-properties"></a>Свойства индекса

Следующие операции являются общими для учетных записей, обслуживающих протоколы связи версии 4,0 и учетных записей, обслуживающих более ранние версии. Вы можете узнать больше о [поддерживаемых индексах и индексированных свойствах](mongodb-feature-support-40.md#indexes-and-index-properties).

### <a name="unique-indexes"></a>Уникальные индексы

[Уникальные индексы](unique-keys.md) полезны для принудительного применения двух или более документов, которые не содержат одинаковых значений для индексированных полей.

> [!IMPORTANT]
> Уникальные индексы могут создаваться только в том случае, если коллекция пуста (не содержит документов).

Следующая команда создает уникальный индекс для поля `student_id` :

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
    "_t" : "CreateIndexesResponse",
    "ok" : 1,
    "createdCollectionAutomatically" : false,
    "numIndexesBefore" : 1,
    "numIndexesAfter" : 4
}
```

Для сегментированных коллекций необходимо указать ключ сегмента (раздел), чтобы создать уникальный индекс. Иными словами, все уникальные индексы для сегментированной коллекции — это составные индексы, где одно из полей является ключом раздела.

Следующие команды создают сегментированную коллекцию ```coll``` (ключ сегмента ```university``` ) с уникальным индексом в полях `student_id` и `university` :

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "university" : 1, "student_id" : 1 }, {unique:true});
{
    "_t" : "CreateIndexesResponse",
    "ok" : 1,
    "createdCollectionAutomatically" : false,
    "numIndexesBefore" : 3,
    "numIndexesAfter" : 4
}
```

В предыдущем примере пропуск ```"university":1``` предложения возвращает ошибку со следующим сообщением:

*не удается создать уникальный индекс для {student_id: 1,0} с шаблоном ключа сегмента {университет: 1,0}*

### <a name="ttl-indexes"></a>Индексы срока жизни

Чтобы включить истечение срока действия документа в определенной коллекции, необходимо создать индекс срока [жизни (TTL)](../cosmos-db/time-to-live.md). Индекс TTL — это индекс `_ts` поля со `expireAfterSeconds` значением.

Пример

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Предыдущая команда удаляет все документы в ```db.coll``` коллекции, которые не были изменены за последние 10 секунд.

> [!NOTE]
> **_TS** поле зависит от Azure Cosmos DB и недоступно для клиентов MongoDB. Это зарезервированное свойство (System), содержащее отметку времени последнего изменения документа.

## <a name="track-index-progress"></a>Отслеживать ход выполнения индекса

Версия 3.6 + API Azure Cosmos DB для MongoDB поддерживает `currentOp()` команду для отслеживания хода выполнения индекса в экземпляре базы данных. Эта команда возвращает документ, содержащий сведения о выполняемых операциях в экземпляре базы данных. `currentOp`Команда используется для отслеживания всех выполняемых операций в машинном MongoDB. В API Azure Cosmos DB для MongoDB эта команда поддерживает только отслеживание операций с индексами.

Ниже приведены некоторые примеры, демонстрирующие использование `currentOp` команды для отслеживания хода выполнения индекса.

* Получение хода выполнения индекса для коллекции:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* Получение хода выполнения индекса для всех коллекций в базе данных:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* Получите ход выполнения индекса для всех баз данных и коллекций в учетной записи Azure Cosmos:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

### <a name="examples-of-index-progress-output"></a>Примеры выходных данных выполнения индекса

Сведения о ходе выполнения индекса показывают процент выполнения текущей операции с индексами. Ниже приведен пример формата выходного документа для различных этапов выполнения индекса.

* Операция с индексами для коллекции "foo" и "линейчатой" базы данных, которая составляет 60% завершения, будет иметь следующий выходной документ. В этом `Inprog[0].progress.total` поле отображается 100 в качестве целевого процента завершения.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 60 %",
                "progress" : {
                        "done" : 60,
                        "total" : 100
                },
                …………..…..
        }
        ],
        "ok" : 1
   }
   ```

* Если операция с индексами только что была запущена для коллекции "foo" и "линейчатой" базы данных, выходной документ может отобразить 0% хода выполнения, пока не достигнет измеряемого уровня.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 0 %",
                "progress" : {
                        "done" : 0,
                        "total" : 100
                },
                …………..…..
        }
        ],
       "ok" : 1
   }
   ```

* При завершении выполняемой операции с индексом выходной документ отображает пустые `inprog` операции.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

## <a name="background-index-updates"></a>Фоновые обновления индекса

Независимо от значения, указанного для свойства **фонового** индекса, обновления индекса всегда выполняются в фоновом режиме. Поскольку обновления индекса потребляют единицы запросов (с более низким приоритетом) по сравнению с другими операциями базы данных, изменения индекса не будут приводить к простою для записи, обновления или удаления.

При добавлении нового индекса влияния на чтение доступности не возникает. Запросы будут использовать новые индексы только после завершения преобразования индекса. Во время преобразования индекса обработчик запросов будет продолжать использовать существующие индексы, поэтому во время преобразования индексирования вы научитесь выполнять аналогичные операции чтения до начала изменения индексирования. При добавлении новых индексов также возникает риск неполного или несоответствия результатов запроса.

При удалении индексов и немедленном выполнении запросов с фильтрами на удаленные индексы результаты могут быть непоследовательными и неполными до завершения преобразования индекса. При удалении индексов обработчик запросов не предоставляет единообразные или полные результаты при фильтрации запросов к этим только что удаленным индексам. Большинство разработчиков не удаляют индексы, а затем сразу же пытаются запросить их, так что на практике это маловероятно.

> [!NOTE]
> Можно [отслеживать ход выполнения индекса](#track-index-progress).

## <a name="reindex-command"></a>Команда "переиндексировать"

`reIndex`Команда воссоздаст все индексы в коллекции. В большинстве случаев это не требуется. Однако в некоторых редких случаях производительность запросов может улучшиться после выполнения `reIndex` команды.

Команду можно выполнить `reIndex` с помощью следующего синтаксиса:

`db.runCommand({ reIndex: <collection> })`

Чтобы проверить, нужно ли выполнить команду, можно использовать следующий синтаксис `reIndex` :

`db.runCommand({"customAction":"GetCollection",collection:<collection>, showIndexes:true})`

Пример результатов выполнения:

```
{
        "database" : "myDB",
        "collection" : "myCollection",
        "provisionedThroughput" : 400,
        "indexes" : [
                {
                        "v" : 1,
                        "key" : {
                                "_id" : 1
                        },
                        "name" : "_id_",
                        "ns" : "myDB.myCollection",
                        "requiresReIndex" : true
                },
                {
                        "v" : 1,
                        "key" : {
                                "b.$**" : 1
                        },
                        "name" : "b.$**_1",
                        "ns" : "myDB.myCollection",
                        "requiresReIndex" : true
                }
        ],
        "ok" : 1
}
```

Если `reIndex` необходимо, **рекуиресреиндекс** будет иметь значение true. Если `reIndex` не требуется, это свойство будет пропущено.

## <a name="migrate-collections-with-indexes"></a>Перенос коллекций с индексами

В настоящее время можно создавать уникальные индексы только в том случае, если коллекция не содержит документов. Популярные средства миграции MongoDB попытаются создать уникальные индексы после импорта данных. Чтобы обойти эту ошибку, можно вручную создать соответствующие коллекции и уникальные индексы вместо того, чтобы позволить средству миграции испытать попытки. (Это поведение можно достичь с ```mongorestore``` помощью `--noIndexRestore` флага в командной строке.)

## <a name="indexing-for-mongodb-version-32"></a>Индексирование для MongoDB версии 3,2

Доступные функции индексирования и значения по умолчанию отличаются для учетных записей Azure Cosmos, совместимых с версией 3,2 протокола проводной сети MongoDB. Вы можете [проверить версию вашей учетной записи](mongodb-feature-support-36.md#protocol-support) и выполнить [обновление до версии 3,6](mongodb-version-upgrade.md).

Если вы используете версию 3,2, в этом разделе описываются основные различия в версиях 3.6 и более поздних.

### <a name="dropping-default-indexes-version-32"></a>Удаление индексов по умолчанию (версия 3,2)

В отличие от версии 3.6 + API Azure Cosmos DB MongoDB, версия 3,2 индексирует каждое свойство по умолчанию. Чтобы удалить эти индексы по умолчанию для коллекции (), можно использовать следующую команду ```coll``` :

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

После удаления индексов по умолчанию можно добавить дополнительные индексы, как в версии 3.6 и более поздних.

### <a name="compound-indexes-version-32"></a>Составные индексы (версия 3,2)

Составные индексы содержат ссылки на множество полей документа. Если требуется создать составной индекс, [выполните обновление до версии 3,6 или 4,0](mongodb-version-upgrade.md).

### <a name="wildcard-indexes-version-32"></a>Индексы с подстановочными знаками (версия 3,2)

Если необходимо создать индекс с подстановочными знаками, [выполните обновление до версии 4,0 или 3,6](mongodb-version-upgrade.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Индексирование в Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Срок жизни для данных Azure Cosmos DB](../cosmos-db/time-to-live.md)
* Сведения о связи между секционированием и индексацией см. в статье как выполнить [запрос к контейнеру Azure Cosmos](how-to-query-container.md) .
