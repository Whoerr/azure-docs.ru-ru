---
title: Назначение подходящих владельцев и членов для групп привилегированного доступа — Azure Active Directory
description: Узнайте, как назначить соответствующих владельцев или членов группы, назначаемой ролью, в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 08/18/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48c4473cfafce1215219251c47bce1d5730645fc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91534426"
---
# <a name="assign-eligibility-for-a-privileged-access-group-preview-in-privileged-identity-management"></a>Назначение права на доступ к привилегированной группе доступа (Предварительная версия) в управление привилегированными пользователями

Azure Active Directory (Azure AD) управление привилегированными пользователями (PIM) может помочь в управлении допустимостью и активацией назначений для привилегированных групп доступа в Azure AD. Можно назначить права членам или владельцам группы.

>[!NOTE]
>Каждый пользователь, которому разрешено членство или владение привилегированной группой доступа, должен иметь лицензию Azure AD Premium P2. Дополнительные сведения см. [в статье требования к лицензиям для использования Управление привилегированными пользователями](subscription-requirements.md).

## <a name="assign-an-owner-or-member-of-a-group"></a>Назначение владельца или члена группы

Выполните следующие действия, чтобы предоставить пользователю возможность быть членом или владельцем группы привилегированного доступа.

1. [Войдите в Azure AD](https://aad.portal.azure.com) с разрешениями глобального администратора или владельца группы.
1. Выберите **группы** , а затем выберите назначаемую роль группу, которой требуется управлять. Список можно найти или отфильтровать.

    ![Поиск назначаемой роли группы для управления в PIM](./media/groups-assign-member-owner/groups-list-in-azure-ad.png)

1. Откройте группу и выберите **привилегированный доступ (Предварительная версия)**.

    ![Открытие управление привилегированными пользователями интерфейса](./media/groups-assign-member-owner/groups-discover-groups.png)

1. Выберите **добавить назначения**.

    ![Панель нового назначения](./media/groups-assign-member-owner/groups-add-assignment.png)

1. Выберите участников или владельцев, которые должны быть доступны для группы привилегированного доступа.

    ![Снимок экрана, на котором отображается страница "Добавление назначения" с открытой областью "Выбор элемента или группы" и Выделенная кнопка "выбрать".](./media/groups-assign-member-owner/add-assignments.png)

1. Нажмите кнопку **Далее** , чтобы задать длительность членства или владения.

    ![Панель выбора участника или группы](./media/groups-assign-member-owner/assignment-duration.png)

1. В списке **Тип назначения** выберите **Допустимое** или **Активное**. Группы привилегированного доступа предоставляют два различных типа присваивания:

    - Для **подходящих** назначений требуется, чтобы член роли выполнил действие для использования роли. Поддерживаются такие действия, как прохождение многофакторной проверки подлинности (MFA), предоставление коммерческого обоснования или запрос утверждения от назначенных утверждающих.

    - Для **активных** назначений член не должен выполнять никаких действий для использования роли. Члены, назначенные в качестве активных, постоянно имеют все полномочия, присвоенные роли.

1. Если назначение должно быть постоянным (постоянное или постоянное назначение), установите флажок **Постоянный** . В зависимости от параметров Организации этот флажок может не отображаться или быть недоступным для редактирования.

1. По завершении нажмите кнопку **назначить**.

1. Чтобы создать новое назначение ролей, нажмите кнопку **Добавить**. Отображается уведомление о состоянии.

    ![Новое назначение — уведомление](./media/groups-assign-member-owner/groups-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Обновление или удаление существующего назначения роли

Выполните следующие действия, чтобы обновить или удалить существующее назначение роли.

1. [Войдите в Azure AD](https://aad.portal.azure.com) с разрешениями глобального администратора или владельца группы.
1. Выберите **группы** , а затем выберите назначаемую роль группу, которой требуется управлять. Список можно найти или отфильтровать.

    ![Поиск назначаемой роли группы для управления в PIM](./media/groups-assign-member-owner/groups-list-in-azure-ad.png)

1. Откройте группу и выберите **привилегированный доступ (Предварительная версия)**.

    ![Открытие управление привилегированными пользователями интерфейса](./media/groups-assign-member-owner/groups-discover-groups.png)

1. Щелкните роль, которую нужно обновить или удалить.

1. Найдите назначение роли на вкладках **Доступные роли** или **Активные роли**.

    ![Обновление или удаление назначения роли](./media/groups-assign-member-owner/groups-bring-under-management.png)

1. Нажмите кнопку **Обновить** или **Удалить**, чтобы обновить или удалить назначение роли.

    Дополнительные сведения о расширении назначения ролей см. [в разделе расширение или обновление ролей ресурсов Azure в Управление привилегированными пользователями](pim-resource-roles-renew-extend.md).

## <a name="next-steps"></a>Дальнейшие шаги

- [Расширение или продление ролей ресурсов Azure в управление привилегированными пользователями](pim-resource-roles-renew-extend.md)
- [Настройка параметров роли ресурсов Azure в управление привилегированными пользователями](pim-resource-roles-configure-role-settings.md)
- [Назначение ролей Azure AD в управление привилегированными пользователями](pim-how-to-add-role-to-user.md)
