---
title: Формат Avro в фабрике данных Azure
description: В этом разделе описывается, как работать с форматом Avro в фабрике данных Azure.
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/15/2020
ms.author: jingwang
ms.openlocfilehash: 4be499e8e304f34b1cab10aed41b5b98a5f24e9b
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100392601"
---
# <a name="avro-format-in-azure-data-factory"></a>Формат Avro в фабрике данных Azure

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Если требуется **выполнить синтаксический анализ файлов Avro или записать данные в формат Avro**, следуйте этой статье. 

Формат Avro поддерживается для следующих соединителей: [Amazon S3](connector-amazon-simple-storage-service.md), [большой двоичный объект Azure](connector-azure-blob-storage.md), [Azure Data Lake Storage 1-го поколения](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2-го поколения](connector-azure-data-lake-storage.md), [хранилище файлов Azure](connector-azure-file-storage.md), [Файловая система](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)и [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). В этом разделе содержится список свойств, поддерживаемых набором данных Avro.

| Свойство         | Описание                                                  | Обязательно |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Свойство Type набора данных должно иметь значение **Avro**. | Да      |
| location         | Параметры расположения файлов. Каждый файловый соединитель имеет собственный тип расположения и поддерживаемые свойства в разделе `location` . **См. сведения в статье о соединителе. раздел свойств набора данных >**. | Да      |
| аврокомпрессионкодек | Кодек сжатия, используемый при записи в файлы Avro. При чтении из файлов Avro фабрика данных автоматически определяет кодек сжатия на основе метаданных файла.<br>Поддерживаемые типы: "**нет**" (по умолчанию), "**Deflate**", "**Привязка**". Примечание. в настоящее время действие копирования не поддерживает привязку при чтении и записи файлов Avro. | Нет       |

> [!NOTE]
> Пробелы в имени столбца не поддерживаются для файлов Avro.

Ниже приведен пример набора данных Avro в хранилище BLOB-объектов Azure.

```json
{
    "name": "AvroDataset",
    "properties": {
        "type": "Avro",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "avroCompressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). В этом разделе содержится список свойств, поддерживаемых источником и приемником Avro.

### <a name="avro-as-source"></a>Avro в качестве источника

В разделе ***\* Источник \**** действия копирования поддерживаются следующие свойства.

| Свойство      | Описание                                                  | Обязательно |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Свойство Type источника действия копирования должно иметь значение **авросаурце**. | Да      |
| сторесеттингс | Группа свойств для чтения данных из хранилища данных. Каждый файловый соединитель имеет собственные Поддерживаемые параметры чтения в разделе `storeSettings` . Дополнительные **сведения см. в статье о соединителе — > свойства действия копирования**. | Нет       |

### <a name="avro-as-sink"></a>Avro в качестве приемника

В разделе ***\* приемника \**** действия копирования поддерживаются следующие свойства.

| Свойство      | Описание                                                  | Обязательно |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Свойство Type источника действия копирования должно иметь значение **авросинк**. | Да      |
| форматсеттингс          | Группа свойств. См. таблицу **параметров записи Avro** ниже.| Нет      |
| сторесеттингс | Группа свойств для записи данных в хранилище данных. Каждый соединитель на основе файлов имеет собственные Поддерживаемые параметры записи в `storeSettings` . Дополнительные **сведения см. в статье о соединителе — > свойства действия копирования**. | Нет       |

Поддерживаемые **Параметры записи Avro** в `formatSettings` :

| Свойство      | Описание                                                  | Обязательно                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | Для типа Форматсеттингс должно быть задано значение **авровритесеттингс**. | Да                                                   |
| максровсперфиле | При записи данных в папку можно выбрать запись в несколько файлов и указать максимальное число строк на файл.  | Нет |
| филенамепрефикс | Применяется `maxRowsPerFile` , если настроено.<br> Укажите префикс имени файла при записи данных в несколько файлов в результате этого шаблона: `<fileNamePrefix>_00000.<fileExtension>` . Если не указано, то префикс имени файла будет создан автоматически. Это свойство не применяется, если источником является хранилище на основе файлов или [хранилище данных с поддержкой параметров разделов](copy-activity-performance-features.md).  | Нет |

## <a name="mapping-data-flow-properties"></a>Свойства потока данных для сопоставления

При сопоставлении потоков данных можно читать и записывать данные в формат Avro в следующих хранилищах данных: [хранилище BLOB-объектов Azure](connector-azure-blob-storage.md#mapping-data-flow-properties), [Azure Data Lake Storage 1-го поколения](connector-azure-data-lake-store.md#mapping-data-flow-properties)и [Azure Data Lake Storage 2-го поколения](connector-azure-data-lake-storage.md#mapping-data-flow-properties).

### <a name="source-properties"></a>Свойства источника

В таблице ниже перечислены свойства, поддерживаемые источником Avro. Эти свойства можно изменить на вкладке **Параметры источника** .

| Имя | Описание | Обязательно | Допустимые значения | Свойство сценария потока данных |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Подстановочные пути | Будут обработаны все файлы, соответствующие пути с подстановочными знаками. Переопределяет папку и путь к файлу, заданные в наборе данных. | Нет | String[] | вилдкардпасс |
| Корневой путь раздела | Для секционированных файловых данных можно ввести корневой путь к разделу, чтобы считывать секционированные папки в виде столбцов. | Нет | Строка | партитионрутпас |
| Список файлов | Указывает, указан ли источник на текстовый файл, в котором перечислены обрабатываемые файлы | Нет | `true` либо `false` | fileList |
| Столбец для хранения имени файла | Создать новый столбец с именем и путем к исходному файлу | Нет | Строка | ровурлколумн |
| После завершения | Удалите или переместите файлы после обработки. Путь к файлу начинается с корня контейнера | Нет | Delete: `true` или `false` <br> Поместить `['<from>', '<to>']` | пуржефилес <br> мовефилес |
| Фильтровать по дате последнего изменения | Выберите фильтр файлов в зависимости от времени последнего изменения | Нет | Отметка времени | modifiedAfter <br> modifiedBefore |
| Разрешить не найдены файлы | Если значение — true, ошибка не возникает, если файлы не найдены | Нет | `true` либо `false` | игноренофилесфаунд |

### <a name="sink-properties"></a>Свойства приемника

В таблице ниже перечислены свойства, поддерживаемые приемником Avro. Эти свойства можно изменить на вкладке **Параметры** .

| Имя | Описание | Обязательно | Допустимые значения | Свойство сценария потока данных |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Очистить папку | Если конечная папка будет удалена перед записью | Нет | `true` либо `false` | truncate |
| Параметр имени файла | Формат имени записанных данных. По умолчанию по одному файлу на раздел в формате `part-#####-tid-<guid>` | Нет | Шаблон: строка <br> На секцию: String [] <br> Как данные в столбце: строка <br> Вывод в один файл: `['<fileName>']`  | filePattern <br> партитионфиленамес <br> ровурлколумн <br> партитионфиленамес |
| Цитата все | Заключить все значения в кавычки | Нет | `true` либо `false` | куотеалл |

## <a name="data-type-support"></a>Поддержка типов данных

### <a name="copy-activity"></a>Действие копирования
[Сложные типы данных](https://avro.apache.org/docs/current/spec.html#schema_complex) Avro не поддерживаются (записи, перечисления, массивы, карты, объединения и фиксированные) в действии копирования.

### <a name="data-flows"></a>Потоки данных
При работе с файлами Avro в потоках данных можно читать и записывать сложные типы данных, но сначала следует очистить физическую схему из набора данных. В потоках данных можно задать логическую проекцию и производные столбцы, которые являются сложными структурами, а затем отображать эти поля в файле Avro.

## <a name="next-steps"></a>Следующие шаги

- [Действие копирования в фабрике данных Azure](copy-activity-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)
- [Действие получения метаданных в Фабрике данных Azure](control-flow-get-metadata-activity.md)
