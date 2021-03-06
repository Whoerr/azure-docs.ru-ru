---
title: Резервное копирование и восстановление SQL Server на виртуальных машинах Azure | Документация Майкрософт
description: Содержит сведения об архивации и восстановлении баз данных SQL Server на виртуальных машинах Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.subservice: backup
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/04/2018
ms.author: mikeray
ms.openlocfilehash: 2fcba81bcd20db321d791fcda589f40fb0699702
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2020
ms.locfileid: "97733079"
---
# <a name="backup-and-restore-for-sql-server-on-azure-vms"></a>Резервное копирование и восстановление SQL Server на виртуальных машинах Azure
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

В этой статье описаны функции резервного копирования и восстановления, доступные для SQL Server на виртуальных машинах Windows в Azure. В службе хранилища Azure хранятся три копии каждого диска виртуальной машины Azure для защиты от потери или физического повреждения данных. В отличие от SQL Server в локальной среде, в этом случае вы можете не беспокоиться об аппаратных сбоях. Но для баз данных SQL Server все равно следует создавать резервные копии, чтобы защитить их от ошибок приложений и (или) пользователей, например случайной вставки или удаления данных. В таких случаях важно иметь возможность выполнить восстановление до определенной точки во времени.

В первой части этой статьи описаны доступные функции резервного копирования и восстановления. За ней следуют разделы с более подробной информацией о каждой стратегии.

## <a name="backup-and-restore-options"></a>Функции резервного копирования и восстановления

В следующей таблице содержатся сведения о разных вариантах резервного копирования и восстановления SQL Server на виртуальных машинах Azure.

| Стратегия | Версии SQL | Описание |
|---|---|---|
| [Автоматическое резервное копирование](#automated) | 2014<br/> 2016<br/> 2017 | Автоматическое резервное копирование позволяет запланировать регулярное создание резервных копий для всех баз данных на виртуальной машине с SQL Server. Резервные копии хранятся в хранилище Azure в течение 30 дней. Начиная с SQL Server 2016, применяется автоматическое резервное копирование версии 2, с рядом дополнительных возможностей, таких как настройка расписания вручную и настройка частоты полного резервного копирования и резервного копирования журналов. |
| [Azure Backup для виртуальных машин SQL](#azbackup) | 2008<br/> 2012<br/> 2014<br/> 2016<br/> 2017<br/> 2019 | Azure Backup предоставляет возможности резервного копирования корпоративного класса для SQL Server на виртуальных машинах Azure. Эта служба позволяет централизованно управлять резервным копированием для нескольких серверов и тысяч баз данных. Любую из баз данных с помощью портала можно восстановить до определенной точки во времени. Служба поддерживает настраиваемые политики хранения, в том числе хранение резервных копий в течение нескольких лет. |
| [Резервное копирование вручную](#manual) | All | В зависимости от установленной версии SQL Server существует несколько способов вручную выполнять резервное копирование и восстановление SQL Server на виртуальной машине Azure. В этом сценарии вы отвечаете за способ резервного копирования баз данных, место хранения резервных копий и управление ими. |

В следующих разделах каждый из этих вариантов описан подробнее. В последнем разделе этой статьи представлена сводка в виде матрицы функций.

## <a name="automated-backup"></a><a id="automated"></a> Автоматическое резервное копирование

Автоматическое резервное копирование реализует службу автоматического резервного копирования для выпусков SQL Server Standard и Enterprise на виртуальной машине Windows в Azure. Эта служба работает на базе [расширения агента SQL Server IaaS](sql-server-iaas-agent-extension-automate-management.md), которое автоматически устанавливается на все образы виртуальных машин Windows для SQL Server, представленные на портале Azure.

Резервные копии всех баз данных помещаются в настраиваемую учетную запись хранения Azure. Резервные копии можно шифровать и сохранять до 30 дней.

Виртуальные машины с SQL Server 2016 и более поздних версий предоставляют больше возможностей для настройки, так как поддерживают автоматическое резервное копирование версии 2. Среди прочего, добавлены следующие улучшения:

- резервное копирование системных баз данных;
- настройка расписания резервного копирования и интервала времени вручную;
- интервал для полного резервного копирования и резервного копирования файлов журнала.

Чтобы восстановить базу данных, следует найти нужный файл (или файлы) резервной копии и выполнить восстановление на виртуальной машине SQL с помощью SQL Server Management Studio или команд Transact-SQL.

Дополнительные сведения о настройке автоматического резервного копирования для виртуальных машин SQL см. в следующих статьях:

- **SQL Server 2016 и 2017:** [Автоматическое резервное копирование (версия 2) для виртуальных машин Azure](automated-backup.md)
- **SQL Server 2014:** [Автоматическая архивация для виртуальных машин SQL Server 2014 (Resource Manager)](automated-backup-sql-2014.md).

## <a name="azure-backup-for-sql-vms"></a><a id="azbackup"></a> Azure Backup для виртуальных машин SQL

[Azure Backup](../../../backup/index.yml) предоставляет возможности резервного копирования корпоративного класса для SQL Server на виртуальных машинах Azure. Все резервные копии хранятся в хранилище служб восстановления, где вы можете ими управлять. Это решение имеет ряд важных преимуществ, особенно для предприятий:

- **Полное отсутствие необходимости создавать резервные копии инфраструктуры.** Вам не нужно управлять серверами резервного копирования или расположениями хранилищ.
- **Масштабируемость.** Вы можете защитить большое число виртуальных машин SQL и тысячи баз данных.
- **Оплата по мере использования.** Эта функция реализована в виде отдельной услуги, предоставляемой в Azure Backup. Как и все остальные службы Azure, она оплачивается только по мере использования.
- **Централизованное управление и мониторинг.** Вы можете централизованно управлять всеми резервными копиями, включая и другие поддерживаемые в Azure Backup рабочие нагрузки, с помощью единой панели мониторинга Azure.
- **Резервное копирование и хранение на основе политик.** Для регулярных задач регулярного копирования можно создать стандартные политики. Политики хранения позволяют хранить резервные копии в течение многих лет.
- **Поддержка SQL Always On.** Вы можете определить и защитить конфигурацию SQL Server Always On с учетом параметров резервного копирования, настроенных для группы доступности.
- **15-минутная целевая точка восстановления (RPO).** Резервное копирование журналов транзакций SQL можно настроить на каждые 15 минут.
- **Восстановление до точки во времени.** С помощью портала базы данных можно восстанавливать до определенной точки во времени без необходимости вручную восстанавливать полные и разностные резервные копии, а также резервные копии журналов.
- **Консолидированные оповещения об ошибках по электронной почте.** Настройте отправку консолидированных уведомлений по электронной почте в случае возникновения каких-либо сбоев.
- **Управление доступом на основе ролей в Azure**. Определите, кто может управлять операциями резервного копирования и восстановления с помощью портала.

Краткий обзор и демонстрация этой возможности представлена в следующем видео:

> [!VIDEO https://www.youtube.com/embed/wmbANpHos_E]

Это решение Azure Backup для виртуальных машин SQL сейчас предоставляется в общедоступной версии. Дополнительные сведения см. в статье [Back up SQL Server database in Azure](../../../backup/backup-azure-sql-database.md) (Резервное копирование базы данных SQL Server в Azure).

## <a name="manual-backup"></a><a id="manual"></a> Резервное копирование вручную

Если вы хотите вручную управлять операциями резервного копирования и восстановления для виртуальных машин SQL, это можно сделать несколькими способами в зависимости от используемой версии SQL Server. Обзор функций резервного копирования и восстановления см. в следующих статьях для разных версий SQL Server:

- [Резервное копирование и восстановление для SQL Server 2016 и более поздних версий](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
- [Резервное копирование и восстановление для SQL Server 2014](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases?viewFallbackFrom=sql-server-2014)
- [Резервное копирование и восстановление для SQL Server 2012](/previous-versions/sql/sql-server-2012/ms187048(v=sql.110))
- [Резервное копирование и восстановление для SQL Server 2008 R2](/previous-versions/sql/sql-server-2008-r2/ms187048(v=sql.105))
- [Резервное копирование и восстановление для SQL Server 2008](/previous-versions/sql/sql-server-2008/ms187048(v=sql.100))

В следующих разделах подробнее описаны некоторые способы выполнять резервное копирование и восстановление вручную.

### <a name="backup-to-attached-disks"></a>Резервное копирование на подключенные диски

Для SQL Server на виртуальных машинах Azure вы можете применить встроенные функции резервного копирования и восстановления, используя подключенные диски в качестве целевого хранилища для файлов резервных копий. При этом существует ограничение на количество дисков, которые можно присоединить к виртуальной машине Azure, связанное с [размером виртуальной машины](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Также необходимо учитывать накладные расходы на управление дисками.

В статье [Создание полной резервной копии базы данных (SQL Server)](/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server) описано, как вручную создать полную резервную копию базы данных с помощью SQL Server Management Studio (SSMS) или Transact-SQL.

### <a name="backup-to-url"></a>Резервное копирование на URL-адрес

Начиная с SQL Server 2012 SP1 CU2, добавлена возможность резервного копирования непосредственно в хранилище и восстановления из него больших двоичных объектов Microsoft Azure. Этот метод также называется резервным копированием на URL-адрес. В SQL Server 2016 эта функция получила ряд усовершенствований.

| Улучшения в версии SQL Server 2016. | Сведения |
| --- | --- |
| **Чередование** |Когда выполняется резервное копирование в хранилище BLOB-объектов Microsoft Azure, SQL Server 2016 позволяет использовать сразу несколько больших двоичных объектов и, таким образом, создавать резервные копии больших баз данных размером до 12,8 ТБ. |
| **Резервное копирование моментальных снимков** |Благодаря использованию моментальных снимков Azure функция резервного копирования моментальных снимков файлов SQL Server дает возможность практически мгновенно создавать резервные копии и быстро восстанавливать файлы баз данных, сохраненные с помощью службы хранилища BLOB-объектов Azure. Это позволяет упростить политики архивации и восстановления политик. Функция архивации моментальных снимков файлов поддерживает также восстановление до определенной точки во времени. Дополнительные сведения см. в статье [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure). |

Дополнительные сведения см. в следующих статьях для разных версий SQL Server:

- **SQL Server 2016 и 2017:** [Резервное копирование SQL Server в указанное расположение](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service).
- **SQL Server 2014:** [Резервное копирование SQL Server 2014 в указанное расположение](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service?viewFallbackFrom=sql-server-2014).
- **SQL Server 2012**: [Резервное копирование SQL Server 2012 в указанное расположение](/previous-versions/sql/sql-server-2012/jj919148(v=sql.110)).

### <a name="managed-backup"></a>Управляемое резервное копирование

Начиная с SQL Server 2014 добавлена функция управляемого резервного копирования, которая автоматизирует создание резервных копий в хранилище Azure. Управляемое резервное копирование в фоновом режиме выполняет резервное копирование на URL-адрес (см. выше). Кроме того, управляемое резервное копирование является базовым компонентом, обеспечивающим поддержку службы автоматического резервного копирования виртуальной машины SQL Server.

Начиная с SQL Server 2016, управляемое резервное копирование поддерживает несколько новых функций, в том числе создание расписаний, резервное копирование системной базы данных, настройку частоты полного резервного копирования и резервного копирования журналов.

Дополнительные сведения см. в следующих статьях для разных версий SQL Server:

- [управляемое резервное копирование SQL Server в Microsoft Azure для SQL Server 2016 и более поздних версий](/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure);
- [управляемое резервное копирование SQL Server в Microsoft Azure для SQL Server 2014](/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure?viewFallbackFrom=sql-server-2014).

## <a name="decision-matrix"></a>Матрица принятия решений

Следующая таблица содержит сводку всех функций резервного копирования и восстановления, доступных для виртуальных машин SQL Server в Azure.

| Параметр | Автоматическое резервное копирование | Azure Backup для SQL | Резервное копирование вручную |
|---|---|---|---|
| Требуется дополнительная служба Azure |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Настройка политики резервного копирования на портале Azure | ![Зеленая галочка.](./media/backup-restore/yes.png) | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Восстановление баз данных на портале Azure |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Управление несколькими серверами на одной панели мониторинга |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Восстановление на момент времени | ![Зеленая галочка.](./media/backup-restore/yes.png) | ![Зеленая галочка.](./media/backup-restore/yes.png) | ![Зеленая галочка.](./media/backup-restore/yes.png) |
| Целевая точка восстановления (RPO) 15 минут | ![Зеленая галочка.](./media/backup-restore/yes.png) | ![Зеленая галочка.](./media/backup-restore/yes.png) | ![Зеленая галочка.](./media/backup-restore/yes.png) |
| Краткосрочная политика хранения резервных копий (в днях) | ![Зеленая галочка.](./media/backup-restore/yes.png) | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Долгосрочная политика хранения резервных копий (месяцы, годы) |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Встроенная поддержка SQL Server Always On |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Резервное копирование в учетную запись хранения Azure | ![Зеленая галочка.](./media/backup-restore/yes.png)(автоматически) | ![Зеленая галочка.](./media/backup-restore/yes.png)(автоматически) | ![Зеленая галочка.](./media/backup-restore/yes.png)(под управлением пользователя) |
| Управление хранилищами и файлами резервных копий | | ![Зеленая галочка.](./media/backup-restore/yes.png) |  |
| Резервное копирование на подключенные диски виртуальной машины |   |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |
| Централизованные настраиваемые отчеты о резервном копировании |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Объединенные оповещения об ошибках по электронной почте |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Настройка мониторинга на основе журналов Azure Monitor |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |   |
| Мониторинг заданий резервного копирования с помощью скриптов SSMS или Transact-SQL | ![Зеленая галочка.](./media/backup-restore/yes.png) | ![Зеленая галочка.](./media/backup-restore/yes.png) | ![Зеленая галочка.](./media/backup-restore/yes.png) |
| Восстановление баз данных с помощью скриптов SSMS или Transact-SQL | ![Зеленая галочка.](./media/backup-restore/yes.png) |   | ![Зеленая галочка.](./media/backup-restore/yes.png) |

## <a name="next-steps"></a>Дальнейшие действия

Если вы планируете развернуть SQL Server на виртуальной машине Azure, соответствующие инструкции по подготовке см. в статье [Как подготовить виртуальную машину SQL Server на платформе Windows на портале Azure](create-sql-vm-portal.md).

При том, что резервное копирование и восстановление можно использовать для переноса данных, существуют более простые способы переноса данных в SQL Server на виртуальной машине. Полное описание вариантов переноса и соответствующие рекомендации см. в статье [Перенос базы данных SQL Server в экземпляр SQL Server на виртуальной машине Azure](migrate-to-vm-from-sql-server.md).
