---
title: Перенос баз данных в SQL Управляемый экземпляр с помощью службы воспроизведения журналов
description: Сведения о переносе баз данных из SQL Server в SQL Управляемый экземпляр с помощью службы воспроизведения журналов
services: sql-database
ms.service: sql-managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.reviewer: sstein
ms.date: 03/01/2021
ms.openlocfilehash: bc0dc72c7547c8f74aec53b7153fc5384c6b634b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101690793"
---
# <a name="migrate-databases-from-sql-server-to-sql-managed-instance-using-log-replay-service-preview"></a>Перенос баз данных из SQL Server в SQL Управляемый экземпляр с помощью службы воспроизведения журналов (Предварительная версия)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

В этой статье объясняется, как вручную настроить миграцию базы данных с SQL Server 2008-2019 на SQL Управляемый экземпляр с помощью службы воспроизведения журналов (LRS) в настоящее время в общедоступной предварительной версии. Это облачная служба, включенная для Управляемый экземпляр на основе технологии доставки журналов SQL Server. LRS следует использовать в случаях, когда существуют сложные пользовательские миграции и гибридные архитектуры, когда требуется дополнительный элемент управления, если существует небольшая погрешность для простоя или если не удается использовать службу миграции данных Azure (DMS).

И DMS, и LRS используют одну и ту же базовую технологию переноса и те же интерфейсы API. Выпустив LRS, мы дополнительно реализуем сложные пользовательские миграции и гибридную архитектуру между локальными средами. SQL Server и управляемые экземпляры SQL.

## <a name="when-to-use-log-replay-service"></a>Когда следует использовать службу воспроизведения журналов

В случаях, когда служба [DMS Azure](/azure/dms/tutorial-sql-server-to-managed-instance.md) не может использоваться для миграции, LRS облачную службу можно использовать непосредственно с PowerShell, командлетами CLI или API для ручной сборки и координации переноса баз данных в SQL управляемый экземпляр. 

Вы можете использовать облачную службу LRS в следующих случаях:
- Для проекта переноса базы данных требуется дополнительный контроль
- Существует небольшая погрешность для простой прямую миграцию миграции.
- Невозможно установить исполняемый файл DMS в вашей среде
- Исполняемый файл DMS не имеет доступа к резервным копиям базы данных
- Доступ к ОС узла невозможен или отсутствуют права администратора
- Не удалось открыть сетевые порты из вашей среды в Azure
- Резервные копии хранятся непосредственно в хранилище BLOB-объектов Azure с помощью параметра TO URL.
- Существует необходимость использовать разностные резервные копии

> [!NOTE]
> Рекомендуемый автоматизированный способ переноса баз данных из SQL Server в SQL Управляемый экземпляр — использование Azure DMS. Эта служба использует одну и ту же облачную службу LRS в серверной части с доставкой журналов в режиме NORECOVERY. Рекомендуется вручную использовать LRS для управления миграцией в тех случаях, когда Azure DMS не полностью поддерживает ваши сценарии.

## <a name="how-does-it-work"></a>Как это работает?

Для создания пользовательского решения с помощью LRS для переноса баз данных в облако требуется выполнить несколько действий по оркестрации, показанных на схеме и описанных в таблице ниже.

Миграция состоит из создания полных резервных копий базы данных на SQL Server с включенной КОНТРОЛЬной СУММой и копирования файлов резервных копий в хранилище BLOB-объектов Azure. LRS используется для восстановления файлов резервных копий из хранилища BLOB-объектов Azure в SQL Управляемый экземпляр. Хранилище BLOB-объектов Azure используется в качестве промежуточного хранилища между SQL Server и SQL Управляемый экземпляр.

LRS будет отслеживать хранилище больших двоичных объектов Azure на наличие новых разностных резервных копий, а также добавлять созданные после восстановления полной резервной копии журналы и автоматически восстанавливать новые добавленные файлы. Ход выполнения восстановления файлов резервной копии на SQL Управляемый экземпляр можно отслеживать с помощью службы. Кроме того, процесс может быть прерван при необходимости.

LRS не требует специального соглашения об именовании файлов резервных копий, так как оно сканирует все файлы, размещенные в хранилище BLOB-объектов Azure, и конструирует цепочку резервных копий от считывания только заголовков файлов. Базы данных находятся в состоянии восстановления в процессе миграции, так как они восстанавливаются в режиме [NORECOVERY](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql#comparison-of-recovery-and-norecovery) и не могут использоваться для чтения или записи до полного завершения процесса миграции. 

При переносе нескольких баз данных резервные копии для каждой базы данных необходимо поместить в отдельную папку в хранилище BLOB-объектов Azure. LRS должен быть запущен отдельно для каждой базы данных, и необходимо указать разные пути для разделения папок хранилища BLOB-объектов Azure. 

LRS можно запустить в автозаполнении или непрерывном режиме. При запуске в режиме автозаполнения миграция будет выполнена автоматически, когда было восстановлено Последнее указанное имя файла резервной копии. При запуске в непрерывном режиме служба постоянно восстанавливает все добавленные файлы резервных копий, и миграция будет выполнена только в ручном прямую миграцию. Рекомендуется, чтобы ручная прямую миграцию выполнялась только после создания окончательной резервной копии с заключительным фрагментом журнала и показана как восстановленная в Управляемый экземпляр SQL. Окончательный шаг прямую миграцию сделает базу данных доступной для чтения и записи в SQL Управляемый экземпляр.

После остановки LRS либо автоматически на автозаполнении, либо вручную в прямую миграцию, процесс восстановления невозможно возобновить для базы данных, которая была подключена к SQL Управляемый экземпляр. Чтобы восстановить дополнительные файлы резервных копий после завершения миграции с помощью автозаполнения или вручную в прямую миграцию, необходимо удалить базу данных, а всю цепочку резервных копий необходимо восстановить с нуля, перезапуская LRS.

![Описание действий по оркестрации службы воспроизведения журналов для SQL Управляемый экземпляр](./media/log-replay-service-migrate/log-replay-service-conceptual.png)
    
| Операция | Сведения |
| :----------------------------- | :------------------------- |
| **1. Скопируйте резервные копии базы данных из SQL Server в хранилище BLOB-объектов Azure**. | — Копирование полных, разностных и резервных копий журналов из SQL Server в контейнер хранилища BLOB-объектов Azure с помощью [Azcopy](/azure/storage/common/storage-use-azcopy-v10)или [Обозреватель службы хранилища Azure](https://azure.microsoft.com/features/storage-explorer/). <br />— Используйте любые имена файлов, так как LRS не требует определенного соглашения об именовании файлов.<br />— При переносе нескольких баз данных для каждой базы данных требуется отдельная папка. |
| **2. Запустите службу LRS в облаке**. | -Service можно запустить с помощью командлетов: <br /> PowerShell [Start-азсклинстанцедатабаселогреплай](/powershell/module/az.sql/start-azsqlinstancedatabaselogreplay) <br /> [Командлеты CLI az_sql_midb_log_replay_start](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_start). <br /> -Start LRS отдельно для каждой базы данных, указывающей на другую папку резервных копий в хранилище BLOB-объектов Azure. <br />— После запуска служба выберет резервные копии из контейнера хранилища BLOB-объектов Azure и начнет их восстановление в SQL Управляемый экземпляр.<br /> -Если LRS был запущен в непрерывном режиме, то после восстановления всех начальных резервных копий служба будет следить за новыми файлами, отправленными в папку, и будет постоянно применять журналы на основе цепочки LSN, пока служба не будет остановлена. |
| **2,1. Отслеживайте ход выполнения операции**. | — Ход выполнения операции восстановления можно отслеживать с помощью командлетов или: <br /> PowerShell [Get-азсклинстанцедатабаселогреплай](/powershell/module/az.sql/get-azsqlinstancedatabaselogreplay) <br /> [Командлеты CLI az_sql_midb_log_replay_show](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_show). |
| **2,2. при необходимости стоп\аборт операцию**. | В случае, если процесс миграции необходимо прерывать, операция может быть остановлена с помощью командлетов: <br /> PowerShell [азсклинстанцедатабаселогреплай]/повершелл/модуле/аз.скл/стоп-азсклинстанцедатабаселогреплай) <br /> Командлеты CLI [az_sql_midb_log_replay_stop](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_stop) . <br /><br />— Это приведет к удалению восстанавливаемой базы данных на Управляемый экземпляр SQL. <br />-После остановки LRS невозможно возобновить работу базы данных. Процесс миграции необходимо перезапустить с нуля. |
| **3. прямую миграцию в облако при готовности**. | — Останавливает приложение и рабочую нагрузку. Выполните Последнее резервное копирование заключительного фрагмента журнала и отправьте его в хранилище BLOB-объектов Azure.<br /> — Завершите операцию прямую миграцию, инициируя LRS Complete с помощью командлетов: <br />PowerShell [Complete — азсклинстанцедатабаселогреплай](/powershell/module/az.sql/complete-azsqlinstancedatabaselogreplay) <br /> Командлеты CLI [az_sql_midb_log_replay_complete](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_complete) . <br /><br />— Это приведет к остановке службы LRS и включению базы данных в оперативный режим для чтения и записи в Управляемый экземпляр SQL.<br /> -Переведите строку подключения приложения с SQL Server на SQL Управляемый экземпляр. |

## <a name="requirements-for-getting-started"></a>Требования к началу работы

### <a name="sql-server-side"></a>SQL Serverная сторона
- SQL Server 2008-2019
- Полное резервное копирование баз данных (один или несколько файлов)
- Разностная резервная копия (один или несколько файлов)
- Резервная копия журнала (не разбиение для файла журнала транзакций)
- Для резервных копий **должна быть включена контрольная сумма** (обязательная)

### <a name="azure-side"></a>На стороне Azure
- PowerShell AZ. SQL Module версии 2.16.0 или выше ([Установка](https://www.powershellgallery.com/packages/Az.Sql/)или использование Azure [Cloud Shell](/azure/cloud-shell/))
- CLI версии 2.19.0 или выше ([Установка](/cli/azure/install-azure-cli))
- Контейнер хранилища BLOB-объектов Azure подготовлен
- Маркер безопасности SAS с разрешениями на **Чтение** и только **Список** , созданные для контейнера хранилища BLOB-объектов

### <a name="migrating-multiple-databases"></a>Перенос нескольких баз данных
- Файлы резервных копий для разных баз данных должны размещаться в отдельных папках в хранилище BLOB-объектов Azure.
- LRS должен быть запущен отдельно для каждой базы данных, указывающей на соответствующую папку в хранилище BLOB-объектов Azure.
- LRS может поддерживать до 100 одновременных процессов восстановления на один Управляемый экземпляр SQL.

### <a name="azure-rbac-permissions-required"></a>Требуются разрешения RBAC Azure
Для выполнения LRS с помощью указанных клиентов требуется одна из следующих ролей Azure:
- Роль владельца подписки или
- Роль [участника управляемый экземпляр](../../role-based-access-control/built-in-roles.md#sql-managed-instance-contributor) или
- Пользовательская роль со следующим разрешением:
  - `Microsoft.Sql/managedInstances/databases/*`

## <a name="best-practices"></a>Рекомендации

В качестве рекомендаций настоятельно рекомендуется использовать следующие рекомендации:
- Запустите [Помощник по миграции данных](/sql/dma/dma-overview) , чтобы проверить готовность баз данных к миграции в SQL управляемый экземпляр. 
- Разбиение полных и разностных резервных копий на несколько файлов вместо одного файла.
- Включите сжатие резервных копий.
- Используйте Cloud Shell для выполнения скриптов, так как они всегда будут обновлены до выпуска последних версий командлетов.
- Запланируйте Завершение миграции в течение 47 часов с момента запуска службы LRS. Это льготный период, препятствующий запуску программных обновлений, управляемых системой, после запуска LRS.

> [!IMPORTANT]
> - Базу данных, восстанавливаемую с помощью LRS, нельзя использовать до завершения процесса миграции.
> - Доступ только для чтения к базам данных во время миграции не поддерживается LRS.
> - После завершения миграции процесс миграции завершается, так как LRS не поддерживает восстановление после возобновления.

## <a name="steps-to-execute"></a>Шаги для выполнения

### <a name="make-backups-on-the-sql-server"></a>Создание резервных копий на SQL Server

Резервные копии на SQL Server можно создать одним из следующих двух вариантов:

- Выполните резервное копирование в хранилище локального диска, а затем отправьте файлы в хранилище BLOB-объектов Azure, если ваша среда является ограниченным режимом прямой архивации в хранилище больших двоичных объектов Azure.
- Резервное копирование непосредственно в хранилище BLOB-объектов Azure с параметром "TO URL" в T-SQL, если среда и процедуры безопасности позволяют это сделать. 

Настройте базы данных, которые требуется перенести в режим полного восстановления, чтобы разрешить резервное копирование журналов.

```SQL
-- To permit log backups, before the full database backup, modify the database to use the full recovery
USE master
ALTER DATABASE SampleDB
SET RECOVERY FULL
GO
```

Чтобы вручную создать полную резервную копию базы данных в локальном хранилище, используйте приведенные ниже примеры сценариев T-SQL. Убедитесь, что параметр CHECKSUM включен, так как это обязательное требование для LRS.

```SQL
-- Example on how to make full database backup to the local disk
BACKUP DATABASE [SampleDB]
TO DISK='C:\BACKUP\SampleDB_full.bak'
WITH INIT, COMPRESSION, CHECKSUM
GO

-- Example on how to make differential database backup to the locak disk
BACKUP DATABASE [SampleDB]
TO DISK='C:\BACKUP\SampleDB_diff.bak'
WITH DIFFERENTIAL, COMPRESSION, CHECKSUM
GO

-- Example on how to make the transactional log backup to the local disk
BACKUP LOG [SampleDB]
TO DISK='C:\BACKUP\SampleDB_log.trn'
WITH COMPRESSION, CHECKSUM
GO
```

### <a name="create-azure-blob-storage"></a>Создание хранилища BLOB-объектов Azure

Хранилище BLOB-объектов Azure используется в качестве промежуточного хранилища для файлов резервных копий между SQL Server и SQL Управляемый экземпляр. Чтобы создать новую учетную запись хранения и контейнер больших двоичных объектов в учетной записи хранения, выполните следующие действия.

1. [создать учетную запись хранения;](../../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Создавайте контейнер больших двоичных объектов](../../storage/blobs/storage-quickstart-blobs-portal.md) в учетной записи хранения

### <a name="copy-backups-from-sql-server-to-azure-blob-storage"></a>Копирование резервных копий из SQL Server в хранилище BLOB-объектов Azure

Некоторые из следующих подходов можно использовать для передачи резервных копий в хранилище BLOB-объектов в переносе баз данных в управляемый экземпляр с помощью LRS:
- С помощью [Azcopy](/azure/storage/common/storage-use-azcopy-v10)или [Обозреватель службы хранилища Azure](https://azure.microsoft.com/features/storage-explorer) для отправки резервных копий в контейнер больших двоичных объектов.
- Использование Обозреватель службы хранилища в портал Azure.

### <a name="make-backups-from-sql-server-directly-to-azure-blob-storage"></a>Создание резервных копий из SQL Server непосредственно в хранилище BLOB-объектов Azure

Если корпоративная и сетевая политики допускают его, альтернативным способом является создание резервных копий из SQL Server непосредственно в хранилище BLOB-объектов Azure с помощью функции SQL Server Native [BACKUP to URL](/sql/relational-databases/backup-restore/sql-server-backup-to-url) . Если вы можете попытаться выполнить этот параметр, создание резервных копий в локальном хранилище и их отправка в хранилище BLOB-объектов Azure не требуется.

В качестве первого шага для этой операции требуется создание маркера проверки подлинности SAS для хранилища BLOB-объектов Azure, а маркер необходимо импортировать в SQL Server. Вторым шагом является создание резервных копий с параметром "TO URL" в T-SQL. Убедитесь, что все резервные копии созданы с включенным параметром ЧЕКСУМ.

Пример кода для создания резервных копий в хранилище BLOB-объектов Azure приведен ниже. Этот пример не содержит инструкций по импорту маркера SAS. Подробные инструкции, в том числе создание и импорт маркера SAS для SQL Server, приведены в следующем руководстве: [Использование службы хранилища BLOB-объектов Azure с SQL Server](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016#1---create-stored-access-policy-and-shared-access-storage). 

```SQL
-- Example on how to make full database backup to URL
BACKUP DATABASE [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_full.bak'
WITH INIT, COMPRESSION, CHECKSUM
GO

-- Example on how to make differential database backup to URL
BACKUP DATABASE [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_diff.bak'  
WITH DIFFERENTIAL, COMPRESSION, CHECKSUM
GO

-- Example on how to make the transactional log backup to URL
BACKUP LOG [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_log.trn'  
WITH COMPRESSION, CHECKSUM
```

### <a name="generate-azure-blob-storage-sas-authentication-for-lrs"></a>Создание проверки подлинности SAS хранилища BLOB-объектов Azure для LRS

Хранилище BLOB-объектов Azure используется в качестве промежуточного хранилища для файлов резервных копий между SQL Server и SQL Управляемый экземпляр. Для использования службой LRS необходимо создать маркер проверки подлинности SAS с разрешениями "список" и "только чтение". Это позволит службе LRS получить доступ к хранилищу BLOB-объектов Azure и использовать файлы резервных копий для их восстановления в SQL Управляемый экземпляр. Выполните следующие действия, чтобы создать аутентификацию SAS для LRS use:

1. Доступ к Обозреватель службы хранилища из портал Azure
2. Разверните раздел Контейнеры больших двоичных объектов.
3. Щелкните правой кнопкой мыши контейнер больших двоичных объектов и выберите получить подпись общего доступа  ![ служба воспроизведения создать маркер проверки подлинности SAS.](./media/log-replay-service-migrate/lrs-sas-token-01.png)
4. Выберите срок действия маркера. Убедитесь, что токен действителен для периода миграции.
5. Выберите часовой пояс для маркера-UTC или местное время
    - Возможно, несоответствие часового пояса маркера и Управляемый экземпляр SQL. Убедитесь, что маркер SAS имеет соответствующие сроки, которые необходимо учитывать в часовых поясах. Если это возможно, задайте в качестве часового пояса более раннюю и позднее время планового окна миграции.
6. Выберите только чтение и вывод списка разрешений.
    - Другие разрешения не должны быть выбраны, или в противном случае LRS не сможет запуститься. Это требование безопасности предназначено для разработки.
7. Щелкните Создать кнопка  ![ служба воспроизведения журнала создать токен проверки подлинности SAS.](./media/log-replay-service-migrate/lrs-sas-token-02.png)

Проверка подлинности SAS будет создана с учетом срока действия, указанного ранее. Вам потребуется версия URI созданного маркера, как показано на снимке экрана ниже.

![Пример идентификатора URI проверки подлинности SAS созданной службой воспроизведения журналов](./media/log-replay-service-migrate/lrs-generated-uri-token.png)

### <a name="copy-parameters-from-sas-token-generated"></a>Копировать параметры из созданного маркера SAS

Чтобы иметь возможность правильно использовать маркер SAS для запуска LRS, необходимо понимать его структуру. URI созданного маркера SAS состоит из двух частей:
- Сторажеконтаинерури и 
- Сторажеконтаинерсастокен, разделенный вопросительным знаком (?), как показано на рисунке ниже.

    ![Пример идентификатора URI проверки подлинности SAS созданной службой воспроизведения журналов](./media/log-replay-service-migrate/lrs-token-structure.png)

- Первая часть, начинающаяся с "https://", до тех пор, пока не будет использован вопросительный знак (?) для параметра Сторажеконтаинерури, который поступает в качестве входных данных для LRS. Это дает LRS сведения о папке, в которой хранятся файлы резервной копии базы данных.
- Вторая часть, начиная с вопросительного знака (?), в примере «SP =» и до конца строки — Сторажеконтаинерсастокен параметра. Это фактически подписанный маркер проверки подлинности, допустимый в течение указанного времени. Эта часть не обязательно должна начинаться с "SP =", как показано, и может отличаться.

Скопируйте параметры следующим образом:

1. Скопируйте первую часть маркера, начиная с https://, до вопросительного знака (?) и используйте его в качестве параметра Сторажеконтаинерури в PowerShell или CLI для запуска LRS, как показано на снимке экрана ниже.

    ![Параметр Сторажеконтаинерури копирования службы воспроизведения журнала](./media/log-replay-service-migrate/lrs-token-uri-copy-part-01.png)

2. Скопируйте вторую часть маркера, начинающуюся с вопросительного знака (?), до конца строки и используйте ее в качестве параметра Сторажеконтаинерсастокен в PowerShell или CLI для запуска LRS, как показано на снимке экрана ниже.

    ![Параметр Сторажеконтаинерсастокен копирования службы воспроизведения журнала](./media/log-replay-service-migrate/lrs-token-uri-copy-part-02.png)

> [!IMPORTANT]
> - Разрешения для маркера SAS для хранилища BLOB-объектов Azure должны быть прочитаны и перечислены только в списке. Если для маркера проверки подлинности SAS предоставлены любые другие разрешения, запуск службы LRS завершится ошибкой. Эти требования безопасности предназначены для разработки.
> - Токен должен иметь соответствующую достоверность времени. Убедитесь, что часовые пояса между маркером и управляемым экземпляром учитываются.
> - Убедитесь, что параметр Сторажеконтаинерури для PowerShell или CLI скопирован из универсального кода ресурса (URI) созданного маркера, начиная с https://до появления вопросительного знака (?). Не включайте вопросительный знак.
> Убедитесь, что параметр Сторажеконтаинерсастокен для PowerShell интерфейса командной строки скопирован из универсального кода ресурса (URI) созданного маркера, начиная с вопросительного знака (?) до конца строки. Не включайте вопросительный знак.

### <a name="log-in-to-azure-and-select-subscription"></a>Войдите в Azure и выберите подписку.

Используйте следующий командлет PowerShell для входа в Azure:

```powershell
Login-AzAccount
```

Выберите подходящую подписку, в которой находится SQL Управляемый экземпляр, с помощью следующего командлета PowerShell:

```powershell
Select-AzSubscription -SubscriptionId <subscription ID>
```

## <a name="start-the-migration"></a>Начало миграции

Миграция начнется с запуска службы LRS. Службу можно запустить в автозаполнении или непрерывном режиме. При запуске в режиме автозаполнения миграция будет выполнена автоматически, когда был восстановлен последний указанный файл резервной копии. Для этого параметра требуется команда Start, чтобы указать имя файла последнего файла резервной копии. Когда LRS запускается в непрерывном режиме, служба постоянно восстанавливает все добавленные файлы резервных копий, а миграция закончится только в ручном прямую миграцию. 

### <a name="start-lrs-in-autocomplete-mode"></a>Запуск LRS в режиме автозаполнения

Чтобы запустить службу LRS в режиме автозаполнения, используйте следующую команду PowerShell или команды CLI. Укажите имя последнего файла резервной копии с параметром-Ластбаккупнаме. При восстановлении указанного имени файла последней резервной копии служба автоматически инициирует прямую миграцию.

Запуск LRS в режиме автозаполнения с примером PowerShell:

```PowerShell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" `
    -StorageContainerUri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D" `
    -AutoCompleteRestore `
    -LastBackupName "last_backup.bak"
```

Запуск LRS в режиме автозаполнения — пример CLI:

```CLI
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb -a --last-bn "backup.bak"
    --storage-uri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="start-lrs-in-continuous-mode"></a>Запуск LRS в непрерывном режиме

Запуск LRS в непрерывном режиме — пример PowerShell:

```PowerShell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" -StorageContainerUri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

Запуск LRS в непрерывном режиме — пример CLI:

```CLI
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb
    --storage-uri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="scripting-lrs-start-in-continuous-mode"></a>Запуск сценария LRS в непрерывном режиме

Клиенты PowerShell и CLI для запуска LRS в непрерывном режиме являются синхронными. Это означает, что клиенты будут ждать, пока ответ API сообщит об успешном или неудачном запуске задания. В ходе этого ожидания команда не вернет элемент управления обратно в командную строку. Если вы создаете скрипт для процесса миграции и захотите, чтобы команда LRS Start немедленно предвернет управление к остальной части скрипта, можно выполнить PowerShell как фоновое задание с параметром-AsJob. Пример.

```PowerShell
$lrsjob = Start-AzSqlInstanceDatabaseLogReplay <required parameters> -AsJob
```

При запуске фонового задания объект задания возвращается немедленно, даже если для завершения задания требуется дополнительное время. Пока задание выполняется, можно продолжать работу с данным сеансом. Дополнительные сведения о запуске PowerShell в качестве фонового задания см. в документации по [началу работы с PowerShell](/powershell/module/microsoft.powershell.core/start-job#description) .

Аналогичным образом для запуска команды CLI в Linux в качестве фонового процесса используйте знак амперсанда (&) в конце команды LRS Start.

```CLI
az sql midb log-replay start <required parameters> &
```

> [!IMPORTANT]
> После запуска LRS все системные исправления, управляемые системой, будут остановлены в течение следующих 47 часов. После передачи этого окна следующее автоматическое обновление программного обеспечения автоматически останавливает текущую LRS. В таком случае миграцию невозможно возобновить, и ее необходимо перезапустить с нуля. 

## <a name="monitor-the-migration-progress"></a>Мониторинг хода выполнения миграции

Чтобы отслеживать ход выполнения операции миграции, используйте следующую команду PowerShell:

```PowerShell
Get-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Чтобы отслеживать ход выполнения операции миграции, используйте следующую команду CLI:

```CLI
az sql midb log-replay show -g mygroup --mi myinstance -n mymanageddb
```

## <a name="stop-the-migration"></a>Завершение миграции

Если необходимо отключить миграцию, используйте следующие командлеты. Остановка миграции приведет к удалению базы данных, восстановленной на Управляемый экземпляр SQL, из-за чего возобновление миграции будет невозможно.

Чтобы стоп\аборт процесс миграции, используйте следующую команду PowerShell:

```PowerShell
Stop-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Чтобы стоп\аборт процесс миграции, используйте следующую команду интерфейса командной строки:

```CLI
az sql midb log-replay stop -g mygroup --mi myinstance -n mymanageddb
```

## <a name="complete-the-migration-continuous-mode"></a>Завершение миграции (непрерывный режим)

Если LRS запускается в непрерывном режиме, то после того, как вы убедитесь, что все резервные копии восстановлены, инициализация прямую миграцию завершит миграцию. После завершения прямую миграцию база данных будет перенесена и готова к доступу на чтение и запись.

Чтобы завершить процесс миграции в непрерывном режиме LRS, используйте следующую команду PowerShell:

```PowerShell
Complete-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
-InstanceName "ManagedInstance01" `
-Name "ManagedDatabaseName" `
-LastBackupName "last_backup.bak"
```

Чтобы завершить процесс миграции в непрерывном режиме LRS, используйте следующую команду интерфейса командной строки:

```CLI
az sql midb log-replay complete -g mygroup --mi myinstance -n mymanageddb --last-backup-name "backup.bak"
```

## <a name="functional-limitations"></a>Ограничения функциональных возможностей

Функциональные ограничения службы воспроизведения журналов (LRS):
- Восстанавливаемую базу данных нельзя использовать в процессе миграции для доступа только для чтения.
- Обновления программного обеспечения, управляемые системой, будут заблокированы в течение 47 часов с момента запуска LRS. После истечения срока действия этого временного окна следующее обновление программного обеспечения перестанет LRS. В таком случае LRS необходимо перезапустить с нуля.
- Для LRS требуются базы данных на SQL Server для резервного копирования с включенным параметром CHECKSUM.
- Маркер SAS для использования LRS должен быть создан для всего контейнера хранилища BLOB-объектов Azure, а также должен иметь только разрешения на чтение и вывод списка.
- Файлы резервных копий для разных баз данных должны размещаться в отдельных папках в хранилище BLOB-объектов Azure.
- LRS должен быть запущен отдельно для каждой базы данных, указывающей на отдельные папки с файлами резервных копий в хранилище BLOB-объектов Azure.
- LRS может поддерживать до 100 одновременных процессов восстановления на один Управляемый экземпляр SQL.

## <a name="troubleshooting"></a>Устранение неполадок

После запуска LRS используйте командлеты мониторинга (Get-азсклинстанцедатабаселогреплай или az_sql_midb_log_replay_show), чтобы просмотреть состояние операции. Если после некоторого времени LRS не сможет начаться с ошибкой, проверьте некоторые из наиболее распространенных проблем.
- Существует ли уже существующая база данных с тем же именем в SQL MI, которую вы пытаетесь перенести из SQL Server? Устраните этот конфликт, переименовав одну из баз данных.
- Была ли резервная копия базы данных на SQL Server, сделанная с помощью параметра **CHECKSUM** ?
- Доступны ли разрешения на **Чтение** МАРКЕРа SAS в **списке** только для службы LRS?
- Был ли маркер SAS для LRS, скопированный после вопросительного знака "?", с содержимым, начинающимся примерно так: "SV = 2020-02-10..."? 
- Допустимо ли время **действия маркера** SAS для временного интервала запуска и завершения миграции? Могут быть несоответствия из-за различных **часовых поясов** , используемых для управляемый экземпляр SQL и маркера SAS. Попробуйте повторно создать маркер SAS с расширением срока действия маркера временного окна до и после текущей даты.
- Правильно ли написано имя базы данных, имя группы ресурсов и имя управляемого экземпляра?
- Если LRS был запущен в режиме автозаполнения, было ли указано допустимое имя для последнего файла резервной копии?

## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения о [миграции SQL Server в управляемый экземпляр SQL](../migration-guides/managed-instance/sql-server-to-managed-instance-guide.md).
- Дополнительные сведения о [различиях между SQL Server и управляемый экземпляр SQL Azure](transact-sql-tsql-differences-sql-server.md).
- Узнайте больше о [рекомендациях по затратам и размеру рабочих нагрузок, перенесенных в Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs).
