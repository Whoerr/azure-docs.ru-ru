---
title: Руководство. Настройка Цибсафе для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как автоматически подготавливать и отзывать учетные записи пользователей из Azure AD в Цибсафе.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 7255fe44-1662-4ae4-9ff3-9492911b7ce0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2020
ms.author: Zhchia
ms.openlocfilehash: b79a63d16229c761b29776667c771e940d8f69d7
ms.sourcegitcommit: e3151d9b352d4b69c4438c12b3b55413b4565e2f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2021
ms.locfileid: "100527128"
---
# <a name="tutorial-configure-cybsafe-for-automatic-user-provisioning"></a>Учебник. Настройка Цибсафе для автоматической подготовки пользователей

В этом руководстве описываются действия, которые необходимо выполнить в Цибсафе и Azure Active Directory (Azure AD) для настройки автоматической подготовки пользователей. При настройке Azure AD автоматически подготавливает и отменяет подготовку пользователей и групп для [цибсафе](https://app.cybsafe.com/login) с помощью службы подготовки Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Поддерживаемые возможности
> [!div class="checklist"]
> * Создание пользователей в Цибсафе
> * Удалить пользователей в Цибсафе, когда им больше не нужен доступ
> * Синхронизация атрибутов пользователей между Azure AD и Цибсафе
> * Подготавливайте группы и членство в группах в Цибсафе

## <a name="prerequisites"></a>Предварительные требования

В сценарии, описанном в этом руководстве, предполагается, что у вас уже имеется:

* [Клиент Azure AD.](../develop/quickstart-create-new-tenant.md) 
* Учетная запись пользователя в Azure AD с [разрешением](../roles/permissions-reference.md) настраивать подготовку (например, администратор приложений, администратор облачных приложений, владелец приложения или глобальный администратор). 
* Учетная запись администратора [цибсафе](https://app.cybsafe.com/login) с подпиской Enterprise.

## <a name="step-1-plan-your-provisioning-deployment"></a>Шаг 1. Планирование развертывания для подготовки
1. Узнайте, [как работает служба подготовки](../app-provisioning/user-provisioning.md).
2. Определите, кто будет находиться в [области подготовки](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Определите, какие данные должны [сопоставляться между Azure AD и цибсафе](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-cybsafe-to-support-provisioning-with-azure-ad"></a>Шаг 2. Настройка Цибсафе для поддержки подготовки с помощью Azure AD

1. **URL-адрес клиента** для цибсафе: `https://app.cybsafe.com/scim/v2/` . Это значение будет указано на вкладке "подготовка" приложения Цибсафе в портал Azure.
2. Войдите в [цибсафе](https://app.cybsafe.com/login) с помощью учетной записи администратора.
3. Перейдите в раздел [Параметры — >управление удостоверениями](https://app.cybsafe.com/settings/identity-management). 
4. Щелкните **создать токен** в разделе Подготовка пользователей. Это значение будет указано на вкладке "подготовка" приложения Цибсафе в портал Azure.

## <a name="step-3-add-cybsafe-from-the-azure-ad-application-gallery"></a>Шаг 3. Добавление Цибсафе из коллекции приложений Azure AD

Добавьте Цибсафе из коллекции приложений Azure AD, чтобы начать управление подготовкой в Цибсафе. Если вы ранее настроили Цибсафе для единого входа, можно использовать то же приложение. Но мы рекомендуем создать отдельное приложение для исходной проверки интеграции. Дополнительные сведения о добавлении приложения из коллекции см. [здесь](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Шаг 4. Определение пользователей для включения в область подготовки 

Служба подготовки Azure AD позволяет определить пользователей, которые будут подготовлены, на основе назначения приложению и (или) атрибутов пользователя или группы. Если вы решили определить пользователей на основе назначения, выполните следующие [действия](../manage-apps/assign-user-or-group-access-portal.md), чтобы назначить пользователей и группы приложению. Если вы решили определить пользователей только на основе атрибутов пользователя или группы, примените фильтр области, как описано [здесь](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* При назначении пользователей и групп для Цибсафе необходимо выбрать роль, отличную от **доступа по умолчанию**. Пользователи с такой ролью исключаются из подготовки и будут помечены в журналах подготовки как не имеющие разрешений. Кроме того, если эта роль является единственной, доступной в приложении, можно [изменить манифест приложения](../develop/howto-add-app-roles-in-azure-ad-apps.md), чтобы добавить дополнительные роли. 

* Начните с малого. Протестируйте небольшой набор пользователей и групп, прежде чем выполнять развертывание для всех. Если в область подготовки включены назначенные пользователи и группы, проверьте этот механизм, назначив приложению одного или двух пользователей либо одну или две группы. Если в область включены все пользователи и группы, можно указать [фильтр области на основе атрибутов](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-cybsafe"></a>Шаг 5. Настройка автоматической подготовки пользователей в Цибсафе 

В этом разделе описывается, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и (или) групп в TestApp на основе их назначений в Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-cybsafe-in-azure-ad"></a>Чтобы настроить автоматическую подготовку учетных записей пользователей для Цибсафе в Azure AD, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **Корпоративные приложения**, а затем **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **цибсафе**.

    ![Ссылка на Цибсафе в списке "приложения"](common/all-applications.png)

3. Выберите вкладку **Подготовка**.

    ![Вкладка "Подготовка"](common/provisioning.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Вкладка "Подготовка" с выделенным значением "Авто"](common/provisioning-automatic.png)

5. В разделе **учетные данные администратора** введите URL-адрес клиента Цибсафе и маркер секрета. Щелкните **проверить подключение** , чтобы убедиться, что Azure AD может подключиться к цибсафе. Если подключение не выполняется, убедитесь, что у учетной записи Цибсафе есть разрешения администратора, и повторите попытку.

    ![Токен](common/provisioning-testconnection-tenanturltoken.png)

6. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Отправить уведомление по электронной почте при сбое**.

    ![Почтовое уведомление](common/provisioning-notification-email.png)

7. Щелкните **Сохранить**.

8. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory пользователей с цибсафе**.

9. Проверьте атрибуты пользователя, которые синхронизированы из Azure AD в Цибсафе в разделе " **сопоставление атрибутов** ". Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в цибсафе для операций обновления. Если вы решили изменить [соответствующий целевой атрибут](../app-provisioning/customize-application-attributes.md), необходимо убедиться, что API цибсафе поддерживает фильтрацию пользователей на основе этого атрибута. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

   |attribute|Тип|Поддерживается для фильтрации|
   |---|---|---|
   |userName|Строка|&check;|
   |active|Логическое|
   |displayName|Строка|
   |title|Строка|
   |preferredLanguage|Строка|
   |name.givenName|Строка|
   |name.familyName|Строка|
   |name.formatted|Строка|
   |name.honorificPrefix|Строка|
   |name.honorificSuffix|Строка|
   |nickName|Строка|
   |phoneNumbers[type eq "mobile"].value|Строка|
   |emails[type eq "work"].value|Строка|
   |локаль|Строка|
   |timezone|Строка|
   |userType|Строка|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|Строка|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|Строка|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|Строка|
   

10. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory группы с цибсафе**.

11. Проверьте атрибуты группы, которые синхронизированы из Azure AD в Цибсафе в разделе " **сопоставление атрибутов** ". Атрибуты, выбранные как свойства **Matching** , используются для сопоставления групп в цибсафе для операций обновления. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

      |attribute|Тип|Поддерживается для фильтрации|
      |---|---|---|
      |displayName|Строка|&check;|
      |members|Справочник|

12. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Чтобы включить службу подготовки Azure AD для Цибсафе, измените значение параметра **состояние подготовки** на **включено** в разделе **Параметры** .

      ![Состояние подготовки "Включено"](common/provisioning-toggle-on.png)

14. Определите пользователей и (или) группы, которые вы хотите подготавливать к Цибсафе, выбрав нужные значения в **области** в разделе **Параметры** .

      ![Область действия подготовки](common/provisioning-scope.png)

15. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

      ![Сохранение конфигурации подготовки](common/provisioning-configuration-save.png)

После этого начнется цикл начальной синхронизации всех пользователей и групп, определенных в поле **Область** в разделе **Параметры**. Начальный цикл занимает больше времени, чем последующие циклы. Пока служба подготовки Azure AD запущена, они выполняются примерно каждые 40 минут. 

## <a name="step-6-monitor-your-deployment"></a>Шаг 6. Мониторинг развертывания
После настройки подготовки используйте следующие ресурсы для мониторинга развертывания.

1. Используйте [журналы подготовки](../reports-monitoring/concept-provisioning-logs.md), чтобы определить, какие пользователи были подготовлены успешно или неудачно.
2. Используйте [индикатор выполнения](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md), чтобы узнать состояние цикла подготовки и приблизительное время до его завершения.
3. Если конфигурация подготовки, вероятно, находится в неработоспособном состоянии, приложение перейдет в карантин. Дополнительные сведения о режимах карантина см. [здесь](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="change-log"></a>Журнал изменений

* 02/15/2021-пользовательское корпоративное расширение атрибута **Department**, **подразделение** и **Организация** добавлены.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md)