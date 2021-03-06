---
title: Рекомендации по загрузке данных
description: Рекомендации и оптимизация производительности для загрузки данных в выделенный пул SQL Azure синапсе Analytics.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 3c8c34cc3e23306f1d024cfa36b40c7975caa8c6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101674289"
---
# <a name="best-practices-for-loading-data-into-a-dedicated-sql-pool-azure-synapse-analytics"></a>Рекомендации по загрузке данных в выделенный пул SQL Azure синапсе Analytics

В этой статье вы найдете рекомендации и оптимизации производительности для загрузки данных.

## <a name="prepare-data-in-azure-storage"></a>Подготовка данных в службе хранилища Azure

Чтобы максимально сокращать задержку, соположите свой уровень хранилища и выделенный пул SQL.

При экспорте данных в файл формата ORC могут возникнуть ошибки, связанные с нехваткой памяти в Java, если есть текстовые столбцы больших размеров. Чтобы обойти это ограничение, экспортируйте только подмножество столбцов.

Polybase не может загружать строки, имеющие более 1 000 000 байт данных. Объем данных, загружаемых в текстовые файлы в хранилище BLOB-объектов Azure или Azure Data Lake Store, не должен превышать 1 000 000 байтов. Это ограничение байтов не зависит от определенной схемы таблицы.

Все форматы файлов имеют разные характеристики производительности. Чтобы максимально ускорить загрузку, используйте сжатые текстовые файлы с разделителями. Разница между производительностью UTF-8 и UTF-16 будет минимальной.

Разбейте большие сжатые файлы на сжатые файлы меньшего размера.

## <a name="run-loads-with-enough-compute"></a>Запуск нагрузок с достаточным количеством вычислений

Чтобы максимально ускорить загрузку, выполняйте только одно задание загрузки за раз. Если это нецелесообразно, выполняйте одновременно минимальное число загрузок. Если вы планируете большое задание загрузки, рассмотрите возможность масштабирования выделенного пула SQL до загрузки.

Чтобы запускать загрузки в соответствующих вычислительных ресурсах, создайте пользователей, назначенных для выполнения загрузок. Назначьте каждого пользователя загрузки определенному классу ресурсов или группе рабочей нагрузки. Чтобы выполнить загрузку, войдите в качестве одного из загрузок пользователей, а затем запустите загрузку. Загрузка выполняется с помощью класса ресурсов пользователя.  Использовать этот метод проще, чем пытаться изменить класс ресурсов пользователя в соответствии с текущими потребностями.

### <a name="create-a-loading-user"></a>Создание пользователя для загрузки

В этом примере создается пользователь, выполняющий загрузку, для класса ресурсов staticrc20. На первом шаге устанавливается **подключение к главному серверу** и создается имя входа.

```sql
   -- Connect to master
   CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
```

Подключитесь к хранилищу данных и создайте пользователя. В коде ниже предполагается, что вы установили подключение к базе данных с именем mySampleDataWarehouse. В примере показано, как создать пользователя с именем LoaderRC20 и предоставить ему разрешение на управление базой данных. Затем пользователь добавляется как член роли базы данных staticrc20.  

```sql
   -- Connect to the database
   CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
   GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
   EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
```

Чтобы выполнить загрузку с ресурсами для классов ресурсов staticRC20, войдите в систему как LoaderRC20 и запустите загрузку.

Запускайте загрузку в статических (не динамических) классах ресурсов. Статические классы ресурсов гарантируют, что вы используете одни и те же ресурсы, независимо от [единиц использования хранилища данных](resource-consumption-models.md). Если вы используете динамический класс ресурсов, ресурсы будут разными на разных уровнях обслуживания. При использовании динамических классов более низкий уровень обслуживания означает, что, возможно, для пользователя, выполняющего загрузку, потребуется более высокий класс ресурса.

## <a name="allow-multiple-users-to-load"></a>Разрешить загрузку нескольких пользователей

Зачастую появляется необходимость в том, чтобы несколько пользователей могли загружать данные в хранилище данных. Для загрузки с помощью инструкции [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?view=azure-sqldw-latest&preserve-view=true) требуются разрешения CONTROL базы данных.  Разрешение CONTROL предоставляет возможность контроля доступа ко всем схемам. Возможно, потребуется, чтобы не все пользователи, выполняющие загрузку, имели возможность контролировать доступ ко всем схемам. Чтобы ограничить разрешения, можно использовать инструкцию DENY CONTROL.

Пример. Рассмотрим схемы базы данных schema_A для отдела A и schema_B для отдела B. Задайте пользователей базы данных user_A и user_B в качестве пользователей для загрузки PolyBase в отделе A и B соответственно. Им обоим предоставлены разрешения CONTROL для базы данных. Теперь создатели схем A и B блокируют свои схемы, используя инструкцию DENY:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```

User_A и user_B теперь заблокированы из схемы другого отдела.

## <a name="load-to-a-staging-table"></a>Загрузка в промежуточную таблицу

Чтобы добиться максимальной скорости загрузки при перемещении данных в таблицу хранилища данных, загрузите данные в промежуточную таблицу.  Определите промежуточную таблицу в виде кучи и используйте циклический перебор как вариант распространения.

Необходимо учитывать, что загрузка, как правило, представляет собой двухэтапный процесс, где сначала выполняется загрузка в промежуточную таблицу, а затем данные вставляются в рабочую таблицу хранилища данных. Если в рабочей таблице используется хэш-распределение, общее время загрузки и вставки можно сократить, если определить промежуточную таблицу с хэш-распределением. Загрузка в промежуточную таблицу занимает больше времени, но на втором шаге вставки строк в рабочую таблицу перемещение данных между распределениями не происходит.

## <a name="load-to-a-columnstore-index"></a>Загрузка в индекс columnstore

Индексы columnstore требуют огромные объемы памяти для сжатия данных в группы строк высокого качества. Чтобы обеспечить лучшее сжатие и эффективность индексов, индекс columnstore должен сжать в каждую группу строк не более 1 048 576 строк. При нехватке памяти индекс columnstore не сможет добиться максимального сжатия. Это влияет на производительность запросов. Дополнительные сведения см. в статье [Максимальное повышение качества группы строк для индекса columnstore](data-load-columnstore-compression.md).

- Чтобы пользователям, выполняющим загрузку, хватало памяти для максимального сжатия, они должны быть членами средних и крупных классов ресурсов.
- Загрузите достаточно строк, чтобы полностью заполнить новые группы строк. Во время выполнения групповой загрузки каждые 1 048 576 строки сжимаются непосредственно в columnstore как полный группы строк. При загрузках менее 102 400 строк строки отправляются в deltastore, где они хранятся в индексе сбалансированного дерева. Если загрузить слишком мало строк, они все могут перейти в deltastore и не будут сжаты непосредственно в формат columnstore.

## <a name="increase-batch-size-when-using-sqlbulkcopy-api-or-bcp"></a>Увеличение размера пакета при использовании API SQLBulkCopy или BCP

Как упоминалось ранее, Загрузка с помощью Polybase обеспечит максимальную пропускную способность с помощью пула синапсе SQL. Если вы не можете использовать Polybase для загрузки и должен использовать API SQLBulkCopy (или BCP), рекомендуется увеличить размер пакета для лучшей пропускной способности. хорошим правилом для бегунка является размер пакета в диапазоне от 100 до 1м строк.

## <a name="manage-loading-failures"></a>Управление сбоями при загрузке

Загрузка с помощью внешней таблицы может завершиться ошибкой *Запрос прерван — достигнуто максимальное пороговое значение отклонения при чтении из внешнего источника*. Это сообщение означает, что внешние данные содержат "грязные" записи. Запись данных считается "грязной", если типы данных и количество столбцов не соответствуют определениям столбцов из внешней таблицы или если данные не соответствуют указанному формату внешнего файла.

Чтобы устранить "грязные" записи, убедитесь в правильности определений внешней таблицы и формата внешнего файла, а также в том, что внешние данные соответствуют этим определениям. Если подмножество записей внешних данных "грязные", можно отклонить эти записи для запросов с помощью параметров отклонения в CREATE EXTERNAL TABLE.

## <a name="insert-data-into-a-production-table"></a>Вставка данных в производственную таблицу

Одноразовую загрузку в небольшую таблицу или даже периодическую перезагрузку результатов поиска рекомендуется выполнять, используя такой [синтаксис инструкции INSERT](/sql/t-sql/statements/insert-transact-sql?view=azure-sqldw-latest&preserve-view=true): `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Однако одноэлементные вставки не так эффективны, как при выполнении групповой загрузки.

Если вы выполняете тысячи или более вставок в течение дня, объедините вставки в пакет, чтобы выполнить массовую загрузку.  Разработайте процессы для добавления единоразовых вставок в файл, а затем создайте другой процесс, который периодически загружает файл.

## <a name="create-statistics-after-the-load"></a>Создание статистики после загрузки

Чтобы повысить производительность запросов, важно создать статистику по всем столбцам всех таблиц после первой загрузки или внести значительные изменения в данные. Создать статистику можно вручную или включить [Автоматическое создание статистики](../sql-data-warehouse/sql-data-warehouse-tables-statistics.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

Дополнительные сведения см. в статье об [управлении статистикой](develop-tables-statistics.md). В следующем примере показано, как вручную создать статистику для пяти столбцов таблицы Customer_Speed.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Смена ключей к хранилищу данных

В целях безопасности рекомендуем регулярно менять ключ доступа к хранилищу BLOB-объектов. У вас есть два ключа к хранилищу данных для учетной записи хранения больших двоичных объектов, что дает возможность перемещать ключи.

Для смены ключей учетной записи службы хранилища Azure выполните следующие действия.

Для каждой учетной записи хранения с измененным ключом выполните [ALTER DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/alter-database-scoped-credential-transact-sql?view=azure-sqldw-latest&preserve-view=true).

Пример

Исходный ключ создан

```sql
CREATE DATABASE SCOPED CREDENTIAL my_credential WITH IDENTITY = 'my_identity', SECRET = 'key1'
```

Замена ключа 1 ключом 2

```sql
ALTER DATABASE SCOPED CREDENTIAL my_credential WITH IDENTITY = 'my_identity', SECRET = 'key2'
```

Вносить изменения в базовые внешние источники данных больше не нужно.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о Polybase и проектировании процесса извлечения, загрузки и преобразования (ELT) см. в статье [проектирование ELT для Azure синапсе Analytics](../sql-data-warehouse/design-elt-data-loading.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).
- Чтобы получить руководство по загрузке, [Используйте polybase для загрузки данных из хранилища BLOB-объектов Azure в Azure синапсе Analytics](../sql-data-warehouse/load-data-from-azure-blob-storage-using-copy.md?bc=%2fazure%2fsynapse-analytics%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fsynapse-analytics%2ftoc.json).
- Инструкции по мониторингу загрузки данных см. в статье [Мониторинг рабочей нагрузки с помощью динамических административных представлений](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).