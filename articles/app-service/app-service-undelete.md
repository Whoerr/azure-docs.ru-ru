---
title: Восстановление удаленных приложений
description: Узнайте, как восстановить удаленное приложение в Службе приложений Azure. Избегайте проблем, вызванных случайным удалением приложения.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: 71f762ac0effc9ad14510c02679109362163f66d
ms.sourcegitcommit: 273c04022b0145aeab68eb6695b99944ac923465
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008543"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Восстановление удаленного приложения Службы приложений с помощью PowerShell

Если вы случайно удалили приложение в Службе приложений Azure, его можно восстановить с помощью команд из [модуля Az PowerShell](/powershell/azure/).

> [!NOTE]
> - Удаленные приложения очищаются из системы через 30 дней после первоначального удаления. После очистки приложения его нельзя восстановить.
> - Функция отмены удаления не поддерживается для плана потребления.
> - Приложения службы приложений, выполняющиеся в Среда службы приложений, не поддерживают моментальные снимки. Поэтому функции отмены удаления и клонирования не поддерживаются для приложений службы приложений, выполняющихся в Среда службы приложений.
>

## <a name="re-register-app-service-resource-provider"></a>Повторная регистрация поставщика ресурсов Службы приложений

Некоторые клиенты могут столкнуться с проблемой, при которой не удается получить список удаленных приложений. Устранить эту проблему можно, выполнив следующую команду.

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Вывод списка удаленных приложений

Чтобы получить коллекцию удаленных приложений, можно использовать `Get-AzDeletedWebApp`.

Дополнительные сведения о конкретном удаленном приложении можно получить следующим образом.

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

Подробные сведения включают в себя следующее.

- **DeletedSiteId**: уникальный идентификатор приложения на случай, если удалено несколько приложений с одним и тем же именем.
- **SubscriptionID**: подписка, содержащая удаленный ресурс;
- **Расположение.** расположение исходного приложения;
- **ResourceGroupName**: имя исходной группы ресурсов;
- **Name** (Имя). имя исходного приложения.
- **Slot**: имя слота.
- **Время удаления**: время удаления приложения.  

## <a name="restore-deleted-app"></a>Восстановление удаленного приложения

>[!NOTE]
> `Restore-AzDeletedWebApp` не поддерживается для приложений-функций.

Найдите приложение, которое вы хотите восстановить, и используйте `Restore-AzDeletedWebApp`.

```powershell
Restore-AzDeletedWebApp -TargetResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```
> [!NOTE]
> Слоты развертывания не восстанавливаются в составе приложения. Если необходимо восстановить промежуточный слот, используйте `-Slot <slot-name>`  флаг.
>

Входные данные для команды

- **Целевая группа ресурсов**: Целевая группа ресурсов, в которую будет восстановлено приложение
- **Name** (Имя). Имя приложения, должно быть глобально уникальным.
- **TargetAppServicePlanName**: план Службы приложений, связанный с приложением

По умолчанию `Restore-AzDeletedWebApp` восстановит как конфигурацию приложения, так и содержимое. Если вы хотите восстановить только содержимое, используйте флажок `-RestoreContentOnly` с этим командлетом.

> [!NOTE]
> Если приложение было размещено и затем удалено из Среда службы приложений, его можно восстановить только в том случае, если соответствующий Среда службы приложений по-прежнему существует.
>

Полный справочник по командлетам можно найти здесь: [Restore-AzDeletedWebApp](/powershell/module/az.websites/restore-azdeletedwebapp).
