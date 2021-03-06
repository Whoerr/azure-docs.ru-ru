---
title: Памятка по выделенному пулу SQL (ранее — Хранилище данных SQL)
description: Найдите ссылки и рекомендации по быстрому созданию выделенного пула SQL (ранее — Хранилище данных SQL) в Azure Synapse Analytics.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql-dw
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: cc54ae66cda6bf8ecde07d1830448ec39a15cc29
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98120229"
---
# <a name="cheat-sheet-for-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytic"></a>Памятка по выделенному пулу SQL (ранее — Хранилище данных SQL) в Azure Synapse Analytics

В этой памятке предоставляются полезные советы и рекомендации по созданию решений выделенного пула SQL (ранее — Хранилище данных SQL).

На рисунке ниже показан процесс проектирования хранилища данных с помощью выделенного пула SQL (ранее — Хранилище данных SQL):

![Эскиз](./media/cheat-sheet/picture-flow.png)

## <a name="queries-and-operations-across-tables"></a>Запросы и операции для нескольких таблиц

Если заранее известно, какие основные операции и запросы будут выполняться в хранилище данных, можно разработать архитектуру хранилища данных с соответствующими приоритетами. Эти запросы и операции могут включать в себя:

* Соединение одной или двух таблиц фактов и таблиц измерений, фильтрацию объединенной таблицы с последующим добавлением результатов в киоск данных.
* Создание больших или маленьких обновлений с учетом продажи.
* Добавление только данных в таблицы.

Зная тип операций заранее, вы можете оптимизировать структуру таблицы.

## <a name="data-migration"></a>Перенос данных

Сначала загрузите данные в [Azure Data Lake Storage](../../data-factory/connector-azure-data-lake-store.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) или в хранилище BLOB-объектов Azure. Затем воспользуйтесь [инструкцией COPY](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), чтоб отправить данные в промежуточные таблицы. Используйте следующую конфигурацию:

| Конструирование | Рекомендация |
|:--- |:--- |
| Distribution | Циклический перебор |
| Индексация | Куча |
| Секционирование | None |
| Класс ресурсов | largerc или xlargerc |

Подробнее о [переносе данных](/archive/blogs/sqlcat/migrating-data-to-azure-sql-data-warehouse-in-practice), [загрузке данных](design-elt-data-loading.md), а также о [процессе извлечения, загрузки и преобразования (ELT)](design-elt-data-loading.md).

## <a name="distributed-or-replicated-tables"></a>Распределенные или реплицируемые таблицы

В зависимости от свойств таблицы используйте следующие стратегии:

| Тип | Подходит...| Не подходит|
|:--- |:--- |:--- |
| Реплицированный | * Малые таблицы измерения в схеме типа "звезда" с хранилищем размером менее 2 ГБ после сжатия (приблизительно в 5 раз) |* Выполняется большое количество транзакций записей для таблицы (таких как вставка, операция upsert, удаление, обновление)<br></br>* Часто меняется подготовка единиц использования хранилища данных (DWU)<br></br>* Используются только 2–3 столбца, в то время как таблица содержит много столбцов<br></br>* Индексируется реплицированная таблица |
| Циклический перебор (по умолчанию) | * Временная или промежуточная таблица<br></br> * Нет очевидного ключа соединения или потенциального столбца |* Производительность снижается из-за перемещения данных |
| Хэш | * Таблицы фактов<br></br>* Большие таблицы измерений |* Ключ распределения нельзя обновить |

**Советы**

* Начните с циклического перебора, но стремитесь к реализации стратегии распределения хэша, чтобы воспользоваться архитектурой массового параллелизма.
* Убедитесь, что общие ключи хэша имеют одинаковый формат данных.
* Не распределяйте в формате varchar.
* Таблицы измерений с общим ключом хэша к таблице фактов с частыми операциями объединения могут быть распределены по хэшу.
* С помощью *[sys.dm_pdw_nodes_db_partition_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)* анализируйте любую асимметрию в данных.
* С помощью *[sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)* анализируйте перемещения данных по запросам, контролируйте время трансляции и операций смешения. Это помогает рассмотреть стратегию распределения.

Подробнее о [реплицированных таблицах](design-guidance-for-replicated-tables.md) и [распределенных таблицах](sql-data-warehouse-tables-distribute.md).

## <a name="index-your-table"></a>Индексирование таблицы

Индексирование полезно для быстрого чтения таблицы. Существует уникальный набор технологий, которые можно использовать в зависимости от ваших потребностей.

| Тип | Подходит... | Не подходит|
|:--- |:--- |:--- |
| Куча | * Промежуточная или временная таблица<br></br>* Маленькие таблицы с небольшой областью поиска |* Любой поиск сканирует всю таблицу |
| Кластеризованный индекс | * Таблицы, содержащие не более 100 млн записей<br></br>* Большие таблицы (более 100 млн записей) с 1–2 используемыми столбцами |* Используется реплицированная таблица<br></br>* Выполняются сложные запросы, включающие несколько операций присоединения и группирования<br></br>* Создаются обновления в индексированных столбцах: это занимает память |
| Кластеризованный индекс columnstore (CCI) (по умолчанию) | * Большие таблицы (более 100 млн записей) | * Используется реплицированная таблица<br></br>* Выполняются массовые операции обновления в таблице<br></br>* Допущено избыточное секционирование таблицы: группы строк не охватывают разные узлы и секции распределения |

**Советы**

* Возможно, в дополнение к кластеризованному индексу в столбце, активно используемом для фильтрации, необходимо будет добавить некластеризованный индекс.
* Следите за управлением памятью для таблицы с CCI. Нужно, чтобы при загрузке пользователь (или запрос) использовал большой класс ресурсов. Убедитесь, что избежали обрезки и создания множества маленьких групп сжатых строк.
* На уровне Gen2 таблицы CCI таблиц кэшируются локально на вычислительных узлах, чтобы добиться максимальной производительности.
* При использовании CCI недостаточно эффективное сжатие групп может снизить производительность. В этом случае перестройте или реорганизуйте CCI. На сжатую группу строк потребуется не менее 100 000 строк. Идеальный вариант — 1 млн строк в каждой группе.
* С учетом того, как часто добавляется поэтапная нагрузка, а также ее размера при реорганизации и перестройке индексов можно автоматизировать процессы. Всегда можно использовать очистку Spring.
* Учитывайте различные факторы, когда необходимо обрезать группу строк. Каков размер открытых групп строк? Какой объем данных необходимо загрузить в течение нескольких дней?

Подробнее об [индексах](sql-data-warehouse-tables-index.md).

## <a name="partitioning"></a>Секционирование

Большие таблицы фактов (более 1 млрд строк) можно разделить на секции. В 99 процентах случаев ключ секции должен быть основан на дате. Старайтесь не допускать избыточного секционирования, особенно если у вас кластеризованный индекс columnstore.

Использовать разделение можно для промежуточных таблиц, которым необходимо извлечение, загрузка и преобразование. Это упрощает управление жизненным циклом данных.
Старайтесь не допустить избыточного секционирования данных, особенно в кластеризованном индексе columnstore.

Подробнее о [секциях](sql-data-warehouse-tables-partition.md).

## <a name="incremental-load"></a>Поэтапная загрузка

Если данные будут загружаться поэтапно, то сначала необходимо убедиться, что для загрузки данных выделены большие классы ресурсов.  Это особенно важно при загрузке данных в таблицы с кластеризованными индексами columnstore.  Дополнительные сведения см. в разделе о [классах ресурсов](resource-classes-for-workload-management.md).  

Для автоматизации конвейеров извлечения, загрузки и преобразования в хранилище данных рекомендуем использовать PolyBase и ADF V2.

Для большого пакета обновлений устаревших данных попробуйте использовать [CTAS](sql-data-warehouse-develop-ctas.md), чтобы записать в таблицу данные, которые необходимо сохранить, вместо использования операций INSERT, UPDATE и DELETE.

## <a name="maintain-statistics"></a>Обеспечение статистики

Важно также обновлять статистику, так как данные могут быть *существенно* изменены. Просмотрите [обновленную статистику](sql-data-warehouse-tables-statistics.md#update-statistics), чтобы определить наличие *значительных* изменений. Обновленная статистика помогает оптимизировать планы запросов. Если ведение статистики занимает слишком много времени, нужно выбирать отдельные столбцы, для которых необходимо создавать статистику.

Кроме того, вы можете определить частоту обновлений. Например, можно ежедневно обновлять столбцы дат, в которые добавляются новые значения. Статистику рекомендуется вести в столбцах, которые являются частью объединения, используются в предложении WHERE или GROUP BY.

Подробнее о [статистике](sql-data-warehouse-tables-statistics.md).

## <a name="resource-class"></a>Класс ресурсов

Чтобы выделить память для выполнения запросов, используются группы ресурсов. Если для повышения скорости запроса или загрузки вам требуется больше памяти, необходимо выделить более высокие классы ресурсов. С другой стороны, использование больших классов ресурсов влияет на параллелизм. Обратите на это внимание, прежде чем перемещать всех своих пользователей в большой класс ресурсов.

Если вы заметили, что запросы занимают слишком много времени, убедитесь, что пользователи не работают в больших классах ресурсов. Большие классы потребляют значительно количество слотов выдачи. Они могут создать дополнительные запросы в очереди.

Наконец, при использовании [выделенного пула SQL](sql-data-warehouse-overview-what-is.md) уровня Gen2 (ранее —Хранилище данных SQL) каждый класс ресурсов получает в 2,5 раза больше памяти, чем на уровне Gen1.

Подробнее о работе с [классами ресурсов и параллелизмом](resource-classes-for-workload-management.md).

## <a name="lower-your-cost"></a>Сокращение расходов

Одна из основных функций Azure Synapse — возможность [управлять вычислительными ресурсами](sql-data-warehouse-manage-compute-overview.md). Вы можете приостанавливать работу выделенного пула SQL (ранее — Хранилище данных SQL), когда он не используется. Счет выставляется только за используемые вычислительные ресурсы. Вы можете масштабировать ресурсы в соответствии со своими требованиями к производительности. Приостановить выполнение можно на [портале Azure](pause-and-resume-compute-portal.md) или при помощи [PowerShell](pause-and-resume-compute-powershell.md). Для масштабирования используйте [портал Azure](quickstart-scale-compute-portal.md), [PowerShell](quickstart-scale-compute-powershell.md), [T-SQL](quickstart-scale-compute-tsql.md) или [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute).

Выполняйте автоматическое масштабирование с помощью службы "Функции Azure" в любое время.

[![Изображение с кнопкой "Развернуть в Azure".](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json)

## <a name="optimize-your-architecture-for-performance"></a>Оптимизация архитектуры для производительности

Рекомендуем рассматривать базу данных SQL и Azure Analysis Services в звездообразной архитектуре. Это решение может обеспечить изоляцию рабочей нагрузки между различными группами пользователей наряду с использованием некоторых расширенных функций безопасности базы данных SQL и Azure Analysis Services. Это также поможет обеспечить неограниченный параллелизм для ваших пользователей.

Дополнительные сведения о [стандартных архитектурах, использующих выделенный пул SQL (ранее — Хранилище данных SQL) в Azure Synapse Analytics](/archive/blogs/sqlcat/common-isv-application-patterns-using-azure-sql-data-warehouse).

Разверните периферийные зоны в базах данных SQL из выделенного пула SQL (ранее — Хранилище данных SQL) одним щелчком:

[![Изображение с кнопкой "Развернуть в Azure".](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json)