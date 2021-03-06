---
title: Модель данных для журналов Azure Monitor
description: В этой статье представлены сведения о модели данных Log Analytics в Azure Monitor для данных Azure Backup.
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 2d2146612763bac39eb636bb7da522586a0daee2
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100586776"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Модель данных Log Analytics для данных Azure Backup

Используйте модель данных Log Analytics для создания настраиваемых оповещений из Log Analytics.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

> [!NOTE]
>
> Эта модель данных является ссылкой на режим Диагностики Azure, заключающийся в отправке диагностических событий в Log Analytics (LA). Чтобы изучить модель данных для нового режима, относящегося к конкретному ресурсу, воспользуйтесь следующей статьей. [Модель данных для диагностических событий Azure Backup](./backup-azure-reports-data-model.md)

## <a name="using-azure-backup-data-model"></a>Использование модели данных Azure Backup

Для создания визуальных элементов, настраиваемых запросов и панелей мониторинга в соответствии с вашими требованиями можно использовать следующие поля, предоставляемые в составе модели данных.

### <a name="alert"></a>Предупреждение

Эта таблица содержит сведения о полях, связанных с предупреждением.

| Поле | Тип данных | Описание |
| --- | --- | --- |
| AlertUniqueId_s |текст |Уникальный идентификатор созданного предупреждения |
| AlertType_s |текст |Тип предупреждения, например "Резервная копия" |
| AlertStatus_s |текст |Состояние предупреждения, например активное |
| AlertOccurrenceDateTime_s |Дата и время |Дата и время создания предупреждения |
| AlertSeverity_s |текст |Серьезность предупреждения, например критическая |
|AlertTimeToResolveInMinutes_s    | Number        |Время, затраченное на разрешение оповещения. Пустое значение для активных оповещений         |
|AlertConsolidationStatus_s   |текст         |Определяет, является ли оповещение консолидированным         |
|CountOfAlertsConsolidated_s     |Number         |Число консолидированных предупреждений, если это консолидированное оповещение          |
|AlertRaisedOn_s     |текст         |Тип сущности, для которой выдается оповещение         |
|AlertCode_s     |текст         |Код для однозначной идентификации типа оповещения         |
|RecommendedAction_s   |текст         |Рекомендованное действие для разрешения оповещения         |
| EventName_s |текст |Имя события. Всегда AzureBackupCentralReport |
| BackupItemUniqueId_s |текст |Уникальный идентификатор архивируемого элемента, связанного с предупреждением |
| SchemaVersion_s |текст |Текущая версия схемы, например **V2** |
| State_s |текст |Текущее состояние объекта предупреждения, например "Активный" или "Удален" |
| BackupManagementType_s |текст |Тип поставщика для выполнения резервного копирования, например IaaSVM, FileFolder, которому принадлежит это оповещение. |
| OperationName |текст |Имя текущей операции, например "Предупреждение" |
| Категория |текст |Категория диагностических данных, отправленных в журналы Azure Monitor. Всегда AzureBackupReport |
| Ресурс |текст |Ресурс, для которого собираются данные. Отображается имя хранилища служб восстановления |
| ProtectedContainerUniqueId_s |текст |Уникальный идентификатор защищенного сервера, связанного с оповещением (имел значение ProtectedServerUniqueId_s в версии V1)|
| VaultUniqueId_s |текст |Уникальный идентификатор защищенного хранилища, связанного с предупреждением |
| SourceSystem |текст |Исходная система текущих данных (Azure) |
| ResourceId |текст |Уникальный идентификатор для ресурса, по которому собираются данные. Например, идентификатор ресурса для хранилища Служб восстановления |
| SubscriptionId |текст |Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст |Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст |Поставщик ресурсов, для которого собираются данные. Например, Microsoft.RecoveryServices |
| ResourceType |текст |Тип ресурса, для которого собираются данные. Например, "Хранилище" |

### <a name="backupitem"></a>BackupItem

Эта таблица содержит сведения о полях, связанных с архивируемым элементом.

| Поле | Тип данных | Описание |
| --- | --- | --- |
| EventName_s |текст |Имя события. Всегда AzureBackupCentralReport |  
| BackupItemUniqueId_s |текст |Уникальный идентификатор архивируемого элемента |
| BackupItemId_s |текст |Идентификатор элемента резервного копирования (это поле предназначено только для схемы версии v1) |
| BackupItemName_s |текст |Имя архивируемого элемента |
| BackupItemFriendlyName_s |текст |Понятное имя архивируемого элемента |
| BackupItemType_s |текст |Тип архивируемого элемента, например виртуальная машина, папка с файлами |
| BackupItemProtectionState_s |текст |Состояние защиты элемента резервного копирования |
| BackupItemAppVersion_s |текст |Версия приложения для элемента резервного копирования |
| ProtectionState_s |текст |Текущее состояние защиты архивируемого элемента, например "Защищено" или "Защита остановлена" |
| ProtectionGroupName_s |текст | Имя группы защиты, в которой защищен элемент резервного копирования, для SC DPM и MABS, если применимо|
| SecondaryBackupProtectionState_s |текст |Включена ли дополнительная защита для элемента резервного копирования|
| SchemaVersion_s |текст |Версия схемы, например **V2** |
| State_s |текст |Состояние объекта архивируемого элемента, например "Активный" или "Удален" |
| BackupManagementType_s |текст |Тип поставщика для выполнения резервного копирования, например IaaSVM, FileFolder, к которому принадлежит этот элемент резервного копирования |
| OperationName |текст |Имя операции, например BackupItem |
| Категория |текст |Категория диагностических данных, отправленных в журналы Azure Monitor. Всегда AzureBackupReport |
| Ресурс |текст |Ресурс, для которого собираются данные, например имя хранилища служб восстановления |
| SourceSystem |текст |Исходная система текущих данных (Azure) |
| ResourceId |текст |Идентификатор ресурса для собираемых данных, например идентификатор ресурса хранилища Служб восстановления |
| SubscriptionId |текст |Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст |Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст |Поставщик ресурсов для собираемых данных, например Microsoft.RecoveryServices |
| ResourceType |текст |Тип ресурса для собираемых данных, например "Хранилище" |

### <a name="backupitemassociation"></a>BackupItemAssociation

Эта таблица содержит сведения об ассоциациях архивируемого элемента с различными сущностями.

| Поле | Тип данных | Описание |
| --- | --- | --- |
| EventName_s |текст |Это поле представляет имя этого события. Всегда Азуребаккупцентралрепорт |  
| BackupItemUniqueId_s |текст |Уникальный идентификатор элемента резервного копирования |
| SchemaVersion_s |текст |Это поле обозначает текущую версию схемы. Это **Версия 2** |
| State_s |текст |Текущее состояние ассоциации архивируемого элемента, например "Активный" или "Удален" |
| BackupManagementType_s |текст |Тип поставщика для сервера, выполняющего задание резервного копирования, например IaaSVM, FileFolder |
| BackupItemSourceSize_s |текст | Внешний размер элемента резервного копирования |
| BackupManagementServerUniqueId_s |текст | Поле для однозначной идентификации сервера управления резервным копированием, посредством которого защищен элемент резервного копирования, если это применимо |
| Категория |текст |Это поле представляет категорию диагностических данных, отправленных в Log Analytics. Это установите azurebackupreport |
| OperationName |текст |Имя текущей операции (BackupItemAssociation) |
| Ресурс |текст |Ресурс, для которого собираются данные. Отображается имя хранилища служб восстановления |
| ProtectedContainerUniqueId_s |текст |Уникальный идентификатор защищенного сервера, связанного с элементом резервного копирования (имел значение ProtectedServerUniqueId_s в версии V1) |
| VaultUniqueId_s |текст |Уникальный идентификатор хранилища, которое содержит архивируемый элемент |
| SourceSystem |текст |Исходная система текущих данных (Azure) |
| ResourceId |текст |Идентификатор ресурса для собираемых данных. Например, идентификатор ресурса хранилища Служб восстановления |
| SubscriptionId |текст |Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст |Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст |Поставщик ресурсов для собираемых данных, например Microsoft.RecoveryServices |
| ResourceType |текст |Тип ресурса для данных, которые собираются, например "Хранилище" |

### <a name="backupmanagementserver"></a>BackupManagementServer

Эта таблица содержит сведения об ассоциациях архивируемого элемента с различными сущностями.

| Поле | Тип данных | Описание |
| --- | --- | --- |
|BackupManagementServerName_s     |текст         |Имя сервера управления резервным копированием        |
|AzureBackupAgentVersion_s     |текст         |Версия агента Azure Backup на сервере управления резервным копированием          |
|BackupManagementServerVersion_s     |текст         |Версия сервера управления резервным копированием|
|BackupManagementServerOSVersion_s    |текст            |Версия ОС для сервера управления резервным копированием|
|BackupManagementServerType_s     |текст         |Тип сервера управления резервным копированием, например MABS, SC DPM|
|BackupManagementServerUniqueId_s     |текст         |Поле для уникальной идентификации сервера управления резервным копированием       |
| SourceSystem |текст |Исходная система текущих данных (Azure) |
| ResourceId |текст |Идентификатор ресурса для собираемых данных. Например, идентификатор ресурса хранилища Служб восстановления |
| SubscriptionId |текст |Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст |Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст |Поставщик ресурсов для собираемых данных, например Microsoft.RecoveryServices |
| ResourceType |текст |Тип ресурса для данных, которые собираются, например "Хранилище" |

### <a name="job"></a>Задание

Эта таблица содержит сведения о полях, связанных с заданием.

| Поле | Тип данных | Описание |
| --- | --- | --- |
| EventName_s |текст |Имя события. Всегда AzureBackupCentralReport |
| BackupItemUniqueId_s |текст |Уникальный идентификатор архивируемого элемента |
| SchemaVersion_s |текст |Версия схемы, например **V2** |
| State_s |текст |Текущее состояние объекта задания, например "Активный" или "Удален" |
| BackupManagementType_s |текст |Тип поставщика для сервера, выполняющего задание резервного копирования, например IaaSVM, FileFolder |
| OperationName |текст |Имя текущей операции (Job) |
| Категория |текст |Это поле представляет категорию диагностических данных, отправленных в журналы Azure Monitor. Это установите azurebackupreport |
| Ресурс |текст |Ресурс, для которого собираются данные. Отображается имя хранилища служб восстановления |
| ProtectedServerUniqueId_s |текст |Уникальный идентификатор защищенного сервера, связанного с заданием |
| ProtectedContainerUniqueId_s |текст | Уникальный идентификатор защищенного контейнера, в котором выполняется задание |
| VaultUniqueId_s |текст |Уникальный идентификатор защищенного хранилища |
| JobOperation_s |текст |Операция, для которой запущено задание, например архивация, восстановление, настройка архивации |
| JobStatus_s |текст |Состояние завершенного задания, например "Завершено" или "Ошибка" |
| JobFailureCode_s |текст |Строка кода ошибки, из-за которой произошел сбой задания |
| JobStartDateTime_s |Дата и время |Дата и время начала выполнения задания |
| BackupStorageDestination_s |текст |Место назначения хранилища резервных копий, например облако или диск  |
| AdHocOrScheduledJob_s |текст | Поле, определяющее, является ли задание специализированным или запланированным |
| JobDurationInSecs_s | Number |Общая длительность задания в секундах |
| DataTransferredInMB_s | Number |Переданные данные (в МБ) для этого задания|
| JobUniqueId_g |текст |Уникальный идентификатор для задания |
| RecoveryJobDestination_s |текст | Назначение задания восстановления, куда восстанавливаются данные |
| RecoveryJobRPDateTime_s |Дата и время | Дата и время создания восстанавливаемой точки восстановления |
| RecoveryJobRPLocation_s |текст | Место хранения восстанавливаемой точки восстановления|
| SourceSystem |текст |Исходная система текущих данных (Azure) |
| ResourceId |текст |Идентификатор ресурса для собираемых данных. Например, идентификатор ресурса хранилища Служб восстановления|
| SubscriptionId |текст |Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст |Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст |Поставщик ресурсов, для которого собираются данные. Например, Microsoft.RecoveryServices |
| ResourceType |текст |Тип ресурса, для которого собираются данные. Например, "Хранилище" |

### <a name="policy"></a>Политика

Эта таблица содержит сведения о полях, связанных с политикой.

| Поле | Тип данных | Применимые версии | Описание |
| --- | --- | --- | --- |
| EventName_s |текст ||Это поле представляет имя этого события. Всегда Азуребаккупцентралрепорт |
| SchemaVersion_s |текст ||Это поле обозначает текущую версию схемы. Это **Версия 2** |
| State_s |текст ||Текущее состояние объекта политики, например "Активный" или "Удален" |
| BackupManagementType_s |текст ||Тип поставщика для сервера, выполняющего задание резервного копирования, например IaaSVM, FileFolder |
| OperationName |текст ||Имя текущей операции (Policy) |
| Категория |текст ||Это поле представляет категорию диагностических данных, отправленных в журналы Azure Monitor. Это установите azurebackupreport |
| Ресурс |текст ||Ресурс, для которого собираются данные. Отображается имя хранилища служб восстановления |
| PolicyUniqueId_g |текст ||Уникальный идентификатор для политики |
| PolicyName_s |текст ||Указанное имя политики |
| BackupFrequency_s |текст ||Частота выполнения архивации, например ежедневно или еженедельно |
| BackupTimes_s |текст ||Дата и время запланированной архивации |
| BackupDaysOfTheWeek_s |текст ||Дни недели, на которые запланированы задания архивации |
| RetentionDuration_s |Whole Number ||Длительность хранения настроенных резервных копий |
| DailyRetentionDuration_s |Whole Number ||Общая длительность хранения (в днях) для настроенных резервных копий |
| DailyRetentionTimes_s |текст ||Дата и время настройки ежедневного сохранения |
| WeeklyRetentionDuration_s |Десятичное число ||Общая длительность еженедельного хранения в неделях для настроенных резервных копий |
| WeeklyRetentionTimes_s |текст ||Дата и время настройки еженедельного хранения |
| WeeklyRetentionDaysOfTheWeek_s |текст ||Дни недели, выбранные для еженедельного хранения |
| MonthlyRetentionDuration_s |Десятичное число ||Общая длительность хранения (в месяцах) для настроенных резервных копий |
| MonthlyRetentionTimes_s |текст ||Дата и время настройки ежемесячного хранения |
| MonthlyRetentionFormat_s |текст ||Тип конфигурации для ежемесячного хранения, например ежедневно для хранения по дням, еженедельно для хранения по неделям |
| MonthlyRetentionDaysOfTheWeek_s |текст ||Дни недели, выбранные для ежемесячного хранения |
| MonthlyRetentionWeeksOfTheMonth_s |текст ||Недели месяца, когда настроено ежемесячное хранение, например первая, последняя |
| YearlyRetentionDuration_s |Десятичное число ||Общая длительность хранения (в годах) для настроенных резервных копий |
| YearlyRetentionTimes_s |текст ||Дата и время настройки ежегодного хранения |
| YearlyRetentionMonthsOfTheYear_s |текст ||Месяцы года, выбранные для ежегодного хранения |
| YearlyRetentionFormat_s |текст ||Тип конфигурации для ежегодного хранения, например ежедневно для хранения по дням, еженедельно для хранения по неделям | |
| YearlyRetentionDaysOfTheMonth_s |текст ||Дни месяца, выбранные для ежегодного хранения |
| SynchronisationFrequencyPerDay_s |Whole Number |Версия 2|Число синхронизаций резервной копии файла для SC DPM и MABS в день |
| DiffBackupFormat_s |текст |Версия 2|Формат для разностных резервных копий для SQL в резервной копии виртуальных машин Azure |
| DiffBackupTime_s |Time |Версия 2|Время для разностных резервных копий для SQL в резервной копии виртуальных машин Azure|
| DiffBackupRetentionDuration_s |Десятичное число |Версия 2|Срок хранения для разностных резервных копий для SQL в резервной копии виртуальных машин Azure|
| LogBackupFrequency_s |Десятичное число |Версия 2|Периодичность резервного копирования журналов для SQL|
| LogBackupRetentionDuration_s |Десятичное число |Версия 2|Срок хранения для резервных копий журналов для SQL в резервной копии виртуальных машин Azure|
| DiffBackupDaysofTheWeek_s |текст |Версия 2|Дни недели для разностных резервных копий для SQL в резервной копии виртуальных машин Azure|
| SourceSystem |текст ||Исходная система текущих данных (Azure) |
| ResourceId |текст ||Идентификатор ресурса для собираемых данных. Например, идентификатор ресурса хранилища Служб восстановления |
| SubscriptionId |текст ||Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст ||Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст ||Поставщик ресурсов, для которого собираются данные. Например, Microsoft.RecoveryServices |
| ResourceType |текст ||Тип ресурса, для которого собираются данные. Например, "Хранилище" |

### <a name="policyassociation"></a>PolicyAssociation

Эта таблица содержит сведения об ассоциациях политики с различными сущностями.

| Поле | Тип данных | Применимые версии | Описание |
| --- | --- | --- | --- |
| EventName_s |текст ||Это поле представляет имя этого события. Всегда Азуребаккупцентралрепорт |
| SchemaVersion_s |текст ||Это поле обозначает текущую версию схемы. Это **Версия 2** |
| State_s |текст ||Текущее состояние объекта политики, например "Активный" или "Удален" |
| BackupManagementType_s |текст ||Тип поставщика для сервера, выполняющего задание резервного копирования, например IaaSVM, FileFolder |
| OperationName |текст ||Имя текущей операции (PolicyAssociation) |
| Категория |текст ||Это поле представляет категорию диагностических данных, отправленных в журналы Azure Monitor. Это установите azurebackupreport |
| Ресурс |текст ||Ресурс, для которого собираются данные. Отображается имя хранилища служб восстановления |
| PolicyUniqueId_g |текст ||Уникальный идентификатор для политики |
| VaultUniqueId_s |текст ||Уникальный идентификатор хранилища, к которому относится эта политика |
| BackupManagementServerUniqueId_s |текст |Версия 2 |Поле для однозначной идентификации сервера управления резервным копированием, посредством которого защищен элемент резервного копирования, если это применимо        |
| SourceSystem |текст ||Исходная система текущих данных (Azure) |
| ResourceId |текст ||Идентификатор ресурса для собираемых данных. Например, идентификатор ресурса хранилища Служб восстановления |
| SubscriptionId |текст ||Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст ||Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст ||Поставщик ресурсов, для которого собираются данные. Например, Microsoft.RecoveryServices |
| ResourceType |текст ||Тип ресурса, для которого собираются данные. Например, "Хранилище" |

### <a name="protected-container"></a>Защищенный контейнер

Эта таблица содержит основные поля для защищенных контейнеров (значение ProtectedServer в версии v1)

| Поле | Тип данных | Описание |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |текст | Поле для однозначной идентификации защищенного контейнера |
| ProtectedContainerOSType_s |текст |Тип ОС защищенного контейнера |
| ProtectedContainerOSVersion_s |текст |Версия ОС защищенного контейнера |
| AgentVersion_s |текст |Номер версии агента резервного копирования или агента защиты (в случае с SC DPM и MABS) |
| BackupManagementType_s |текст |Тип поставщика для выполнения резервного копирования. Например, IaaSVM, FileFolder |
| EntityState_s |текст |Текущее состояние объекта защищенного сервера. Например, "Активный" или "Удален" |
| ProtectedContainerFriendlyName_s |текст |Понятное имя защищенного сервера |
| ProtectedContainerName_s |текст |Имя защищенного контейнера |
| ProtectedContainerWorkloadType_s |текст |Тип защищенного контейнера, для которого была создана резервная копия. Например, IaaSVMContainer |
| ProtectedContainerLocation_s |текст |Указывает, находится ли защищенный контейнер в локальной среде или в Azure |
| ProtectedContainerType_s |текст |Указывает, является ли защищенный контейнер сервером или контейнером |
| ProtectedContainerProtectionState_s’  |текст |Состояние защиты для защищенного контейнера |

### <a name="storage"></a>Память

Эта таблица содержит сведения о полях, связанных с хранилищем.

| Поле | Тип данных | Описание |
| --- | --- | --- |
| CloudStorageInBytes_s |Десятичное число |Объем облачного хранилища резервных копий, используемого для резервного копирования, вычисляемый на основе последнего значения (это поле предназначено только для схемы версии v1)|
| ProtectedInstances_s |Десятичное число |Число защищенных экземпляров, используемое для вычисления стоимости использования внешнего хранилища для выставления счетов на основе последнего значения |
| EventName_s |текст |Это поле представляет имя этого события. Всегда Азуребаккупцентралрепорт |
| SchemaVersion_s |текст |Это поле обозначает текущую версию схемы. Это **Версия 2** |
| State_s |текст |Текущее состояние объекта хранилища, например "Активный" или "Удален" |
| BackupManagementType_s |текст |Тип поставщика для сервера, выполняющего задание резервного копирования, например IaaSVM, FileFolder |
| OperationName |текст |Имя текущей операции (Storage) |
| Категория |текст |Это поле представляет категорию диагностических данных, отправленных в журналы Azure Monitor. Это установите azurebackupreport |
| Ресурс |текст |Ресурс, для которого собираются данные. Отображается имя хранилища служб восстановления |
| ProtectedServerUniqueId_s |текст |Уникальный идентификатор защищенного сервера, для которого вычисляется хранилище |
| VaultUniqueId_s |текст |Уникальный идентификатор хранилища, для которого вычисляется хранилище. |
| SourceSystem |текст |Исходная система текущих данных (Azure) |
| ResourceId |текст |Идентификатор ресурса для собираемых данных. Например, идентификатор ресурса хранилища Служб восстановления |
| SubscriptionId |текст |Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст |Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст |Поставщик ресурсов, для которого собираются данные. Например, Microsoft.RecoveryServices |
| ResourceType |текст |Тип ресурса, для которого собираются данные. Например, "Хранилище" |
| StorageUniqueId_s |текст |Уникальный идентификатор для сущности хранилища |
| StorageType_s |текст |Тип хранилища, например облако, том, диск |
| StorageName_s |текст |Имя сущности хранилища, например E:\ |
| StorageTotalSizeInGBs_s |текст |Общий размер хранилища в гигабайтах, потребляемый сущностью хранилища|

### <a name="storageassociation"></a>StorageAssociation

Эта таблица содержит основные поля, относящиеся к хранилищу, которые связывают его с другими сущностями.

| Поле | Тип данных | Описание |
| --- | --- |  --- |
| StorageUniqueId_s |текст |Уникальный идентификатор для сущности хранилища |
| SchemaVersion_s |текст |Это поле обозначает текущую версию схемы. Это **Версия 2** |
| BackupItemUniqueId_s |текст |Уникальный идентификатор для элемента резервного копирования, связанного с сущностью хранилища |
| BackupManagementServerUniqueId_s |текст |Уникальный идентификатор для сервера управления резервным копированием, связанного с сущностью хранилища|
| VaultUniqueId_s |текст |Уникальный идентификатор для хранилища, связанного с сущностью хранилища|
| StorageConsumedInMBs_s |Number|Объем места, используемого соответствующим элементом резервного копирования в соответствующем хранилище |
| StorageAllocatedInMBs_s |Number |Объем места, выделенного соответствующим элементом резервного копирования в соответствующем хранилище с типом "Диск"|

### <a name="vault"></a>Хранилище

Эта таблица содержит сведения о полях, связанных с хранилищем.

| Поле | Тип данных | Описание |
| --- | --- | --- |
| EventName_s |текст |Это поле представляет имя этого события. Всегда Азуребаккупцентралрепорт |
| SchemaVersion_s |текст |Это поле обозначает текущую версию схемы. Это **Версия 2** |
| State_s |текст |Текущее состояние объекта хранилища, например "Активный" или "Удален" |
| OperationName |текст |Имя текущей операции (Vault) |
| Категория |текст |Это поле представляет категорию диагностических данных, отправленных в журналы Azure Monitor. Это установите azurebackupreport |
| Ресурс |текст |Ресурс, для которого собираются данные. Отображается имя хранилища служб восстановления |
| VaultUniqueId_s |текст |Уникальный идентификатор хранилища |
| VaultName_s |текст |Имя хранилища |
| AzureDataCenter_s |текст |Центр обработки данных, где находится хранилище |
| StorageReplicationType_s |текст |Тип репликации хранилища, например геоизбыточная |
| SourceSystem |текст |Исходная система текущих данных (Azure) |
| ResourceId |текст |Идентификатор ресурса для собираемых данных. Например, идентификатор ресурса хранилища Служб восстановления |
| SubscriptionId |текст |Идентификатор подписки ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceGroup |текст |Группа ресурсов ресурса (напр., Хранилище служб восстановления), для которого собираются данные |
| ResourceProvider |текст |Поставщик ресурсов, для которого собираются данные. Например, Microsoft.RecoveryServices |
| ResourceType |текст |Тип ресурса, для которого собираются данные. Например, "Хранилище" |

### <a name="backup-management-server"></a>Сервер управления резервным копированием

Эта таблица содержит основные поля, относящиеся к серверам управления резервным копированием.

|Поле  |Тип данных  | Описание  |
|---------|---------|----------|
|BackupManagementServerName_s     |текст         |Имя сервера управления резервным копированием        |
|AzureBackupAgentVersion_s     |текст         |Версия агента Azure Backup на сервере управления резервным копированием          |
|BackupManagementServerVersion_s     |текст         |Версия сервера управления резервным копированием|
|BackupManagementServerOSVersion_s     |текст            |Версия ОС для сервера управления резервным копированием|
|BackupManagementServerType_s     |текст         |Тип сервера управления резервным копированием, например MABS, SC DPM|
|BackupManagementServerUniqueId_s     |текст         |Поле для уникальной идентификации сервера управления резервным копированием       |

### <a name="preferredworkloadonvolume"></a>PreferredWorkloadOnVolume

В этой таблице указаны рабочие нагрузки, с которыми связан том.

| Поле | Тип данных | Описание |
| --- | --- | --- |
| StorageUniqueId_s |текст |Уникальный идентификатор для сущности хранилища |
| BackupItemType_s |текст |Рабочие нагрузки, для которых этот том является предпочтительным хранилищем|

### <a name="protectedinstance"></a>ProtectedInstance

Эта таблица содержит основные поля, связанные с защищенными экземплярами.

| Поле | Тип данных |Применимые версии | Описание |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |текст |Версия 2|Уникальный идентификатор, используемый для идентификации элемента резервного копирования для виртуальных машин, резервное копирование которых осуществлялось с помощью DPM, MABS|
| ProtectedContainerUniqueId_s |текст |Версия 2|Уникальный идентификатор, используемый для идентификации защищенного контейнера для всего, кроме виртуальных машин, резервное копирование которых осуществлялось с помощью DPM, MABS|
| ProtectedInstanceCount_s |текст |Версия 2|Число защищенных экземпляров для связанного элемента резервного копирования или защищенного контейнера на данный момент времени|

### <a name="recoverypoint"></a>RecoveryPoint

Эта таблица содержит основные поля, связанные с точкой восстановления.

| Поле | Тип данных | Описание |
| --- | --- | --- |
| BackupItemUniqueId_s |текст |Уникальный идентификатор, используемый для идентификации элемента резервного копирования для виртуальных машин, резервное копирование которых осуществлялось с помощью DPM, MABS|
| OldestRecoveryPointTime_s |текст |Дата и время самой старой точки восстановления для элемента резервного копирования|
| OldestRecoveryPointLocation_s |текст |Расположение самой старой точки восстановления для элемента резервного копирования|
| LatestRecoveryPointTime_s |текст |Дата и время самой новой точки восстановления для элемента резервного копирования|
| LatestRecoveryPointLocation_s |текст |Расположение самой новой точки восстановления для элемента резервного копирования|

## <a name="sample-kusto-queries"></a>Примеры запросов Kusto

Ниже приведено несколько примеров, помогающих создавать запросы к данным Azure Backup, находящимся в таблице Диагностики Azure.

- Все успешные задания резервного копирования

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Completed"
    ````

- Все неудачные задания резервного копирования

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Failed"
    ````

- Все успешные задания резервного копирования виртуальных машин Azure

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

- Все успешные задания резервного копирования журналов SQL

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

- Все успешные задания агента Azure Backup

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

## <a name="v1-schema-vs-v2-schema"></a>Сравнение схем версии V1 и V2

Ранее данные диагностики для агента Azure Backup и резервной копии виртуальной машины Azure были отправлены в система диагностики Azure таблицу в схеме, которая называется ***v1 Schema** _. Впоследствии были добавлены новые столбцы для поддержки других сценариев и рабочих нагрузок, а данные диагностики были помещены в новую схему, называемую _ *_v2 Schema_* *.  

По соображениям обратной совместимости сейчас данные диагностики для агента Azure Backup и резервной копии виртуальных машин Azure отправляются в таблицу Диагностики Azure в обеих схемах — V1 и V2 (при этом схема V1 запланирована к объявлению в качестве нерекомендуемой). Вы можете определить, какие записи в Log Analytics относятся к схеме V1, отфильтровав записи по SchemaVersion_s=="V1" в запросах журнала.

Чтобы узнать, какие столбцы относятся только к схеме V1, см. третий столбец, Description, в [модели данных](#using-azure-backup-data-model), описанной выше.

### <a name="modifying-your-queries-to-use-the-v2-schema"></a>Изменение запросов для использования схемы v2

Поскольку схема v1 находится в пути устаревания, рекомендуется использовать только схему v2 во всех пользовательских запросах Azure Backup диагностических данных. Ниже приведен пример обновления запросов для удаления зависимости от схемы v1.

1. Определяет, использует ли запрос любое поле, применимое только к схеме версии 1. Предположим, что у вас есть запрос на перечисление всех элементов резервных копий и связанных с ними защищенных серверов следующим образом:

    ````Kusto
    AzureDiagnostics
    | where Category=="AzureBackupReport"
    | where OperationName=="BackupItemAssociation"
    | distinct BackupItemUniqueId_s, ProtectedServerUniqueId_s
    ````

    В приведенном выше запросе используется поле ProtectedServerUniqueId_s, которое применимо только к схеме v1. Значение, эквивалентное этому полю в схеме v2, — ProtectedContainerUniqueId_s (см. таблицы выше). Поле BackupItemUniqueId_s применимо даже к схеме версии 2, и в этом запросе можно использовать одно и то же поле.

2. Обновите запрос, чтобы использовать имена полей схемы v2. Рекомендуется использовать фильтр, **где SchemaVersion_s = = "v2"** во всех запросах, так что только записи, соответствующие схеме v2, анализируются запросом:

    ````Kusto
    AzureDiagnostics
    | where Category=="AzureBackupReport"
    | where OperationName=="BackupItemAssociation"
    | where SchemaVersion_s=="V2"
    | distinct BackupItemUniqueId_s, ProtectedContainerUniqueId_s
    ````

## <a name="next-steps"></a>Дальнейшие действия

После изучения модели данных вы можете приступить к [созданию настраиваемых запросов](../azure-monitor/visualize/tutorial-logs-dashboards.md) в журналах Azure Monitor, чтобы создать собственную панель мониторинга.
