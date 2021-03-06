---
title: Управление резервными копиями с помощью управления доступом на основе ролей в Azure
description: Используйте управление доступом на основе ролей Azure для управления доступом к операциям управления резервным копированием в хранилище служб восстановления.
ms.reviewer: utraghuv
ms.topic: conceptual
ms.date: 06/24/2019
ms.openlocfilehash: 0dd8d08c4ee79082f47929cf7d453f3f4bbd60ee
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92090885"
---
# <a name="use-azure-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Использование управления доступом на основе ролей в Azure для управления Azure Backup точками восстановления

Управление доступом на основе ролей в Azure (Azure RBAC) обеспечивает детальное управление доступом для Azure. С помощью Azure RBAC вы можете распределить обязанности внутри команды и предоставить пользователям доступ на уровне, который им необходим для выполнения поставленных задач.

> [!IMPORTANT]
> Роли, предоставляемые Azure Backup, ограничены действиями, которые могут быть выполнены в портал Azure или с помощью REST API или командлетов PowerShell или хранилища служб восстановления. Действия, выполняемые в пользовательском интерфейсе клиента агента Azure Backup или в пользовательском интерфейсе пользователя System Center Data Protection Manager или пользовательском интерфейсе Azure Backup Server, не контролируют эти роли.

Azure Backup предоставляет три встроенных роли для управления операциями управления резервным копированием. Дополнительные сведения о [встроенных ролях Azure](../role-based-access-control/built-in-roles.md)

* [Участник резервного копирования](../role-based-access-control/built-in-roles.md#backup-contributor) . Эта роль имеет все разрешения на создание резервной копии и управление ею, за исключением удаления хранилища служб восстановления и предоставления доступа другим пользователям. Эта роль представляет собой администратора управления архивацией, который может выполнять любую операцию управления архивацией.
* [Оператор архивации.](../role-based-access-control/built-in-roles.md#backup-operator) Эта роль имеет те же разрешения, что и участник, но не может удалять резервные копии и управлять политикой архивации. Эта роль эквивалентна роли участника, за исключением того, что пользователь с этой ролью не может выполнять операции удаления, такие как прекращение архивации с удалением данных или удаление регистрации локальных ресурсов.
* [Читатель резервных копий](../role-based-access-control/built-in-roles.md#backup-reader). Эта роль имеет разрешения на просмотр всех операций управления архивацией. Используйте эту роль, чтобы осуществлять мониторинг.

Если вы хотите определить собственные роли для еще большего контроля, см. статью [Создание пользовательских ролей](../role-based-access-control/custom-roles.md) в Azure RBAC.

## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Сопоставление встроенных ролей архивации с операциями управления архивацией

В следующей таблице записаны действия по управлению архивацией и соответствующая минимальная роль Azure, необходимая для выполнения этой операции.

| Операция управления | Требуется минимальная роль Azure | Требуемая область |
| --- | --- | --- |
| Создание хранилища Служб восстановления | Участник резервного копирования | Группа ресурсов, содержащая хранилище |
| Включение архивации виртуальных машин Azure | Оператор резервного копирования | Группа ресурсов, содержащая хранилище |
| | Участник виртуальной машины | Ресурс виртуальной машины |
| Архивация виртуальной машины по запросу | Оператор резервного копирования | Хранилище Служб восстановления |
| Восстановление виртуальной машины | Оператор резервного копирования | Хранилище Служб восстановления |
| | Участник | Группа ресурсов, в которой будет развернута виртуальная машина |
| | Участник виртуальной машины | Исходная виртуальная машина, резервная копия которой была создана |
| Восстановление резервной копии виртуальной машины с неуправляемыми дисками | Оператор резервного копирования | Хранилище Служб восстановления |
| | Участник виртуальной машины | Исходная виртуальная машина, резервная копия которой была создана |
| | Участник учетной записи хранения | Ресурс учетной записи хранения, где будет восстанавливаться диски |
| Восстановление управляемых дисков из резервной копии виртуальной машины | Оператор резервного копирования | Хранилище Служб восстановления |
| | Участник виртуальной машины | Исходная виртуальная машина, резервная копия которой была создана |
| | Участник учетной записи хранения | Временная учетная запись хранения, выбранная в процессе восстановления, для хранения данных из хранилища перед их преобразованием в управляемые диски |
| | Участник | Группа ресурсов, в которую будут восстановлены управляемые диски |
| Восстановление отдельных файлов из резервной копии виртуальной машины | Оператор резервного копирования | Хранилище Служб восстановления |
| | Участник виртуальной машины | Исходная виртуальная машина, резервная копия которой была создана |
| Создание политики архивации виртуальных машин Azure | Участник резервного копирования | Хранилище Служб восстановления |
| Изменение политики архивации виртуальных машин Azure | Участник резервного копирования | Хранилище Служб восстановления |
| Удаление политики архивации виртуальных машин Azure | Участник резервного копирования | Хранилище Служб восстановления |
| Прекращение архивации (с сохранением или удалением данных) виртуальной машины | Участник резервного копирования | Хранилище Служб восстановления |
| Регистрация локального сервера Windows Server, клиента, SCDPM или Azure Backup Server | Оператор резервного копирования | Хранилище Служб восстановления |
| Удаление зарегистрированного локального сервера Windows Server, клиента, SCDPM или Azure Backup Server | Участник резервного копирования | Хранилище Служб восстановления |

> [!IMPORTANT]
> Если вы укажете участника ВИРТУАЛЬНОЙ машины в области ресурсов виртуальной машины и выберите **резервное копирование** как часть параметров виртуальной машины, откроется экран **включить резервное копирование** , даже если резервная копия виртуальной машины уже создана. Это вызвано тем, что вызов для проверки состояния резервного копирования работает только на уровне подписки. Чтобы избежать этого, перейдите в хранилище и откройте представление элемента резервного копирования виртуальной машины или укажите роль участника виртуальной машины на уровне подписки.

## <a name="minimum-role-requirements-for-the-azure-file-share-backup"></a>Минимальные требования к ролям для резервного копирования файлового ресурса Azure

В следующей таблице записаны действия по управлению архивацией и соответствующая роль, необходимая для выполнения операции с общей папкой Azure.

| Операция управления | Требуется роль | Ресурсы |
| --- | --- | --- |
| Включение резервного копирования файловых ресурсов Azure | Участник резервного копирования |Хранилище Служб восстановления |
| |Учетная запись хранения | Ресурс учетной записи хранения участника |
| Архивация виртуальной машины по запросу | Оператор резервного копирования | Хранилище Служб восстановления |
| Восстановить общую папку | Оператор резервного копирования | Хранилище Служб восстановления |
| | Участник учетной записи хранения | Ресурсы учетной записи хранения, в которой имеются исходные и целевые общие папки восстановления |
| Восстановление отдельных файлов | Оператор резервного копирования | Хранилище Служб восстановления |
| |Участник учетной записи хранения|Ресурсы учетной записи хранения, в которой имеются исходные и целевые общие папки восстановления |
| Остановить защиту |Участник резервного копирования | Хранилище Служб восстановления |
| Отмена регистрации учетной записи хранения в хранилище |Участник резервного копирования | Хранилище Служб восстановления |
| |Участник учетной записи хранения | Ресурс учетной записи хранения|

## <a name="next-steps"></a>Дальнейшие действия

* [Управление доступом на основе ролей Azure (Azure RBAC)](../role-based-access-control/role-assignments-portal.md): Начало работы с Azure rbac в портал Azure.
* Сведения об управлении доступом с помощью следующих средств:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Устранение неполадок в управлении доступом на основе ролей в Azure](../role-based-access-control/troubleshooting.md): получение предложений по устранению распространенных проблем.
