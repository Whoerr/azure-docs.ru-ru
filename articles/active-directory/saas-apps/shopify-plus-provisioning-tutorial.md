---
title: Руководство. Настройка Shopify Plus для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как автоматически подготавливать и отзывать учетные записи пользователей из Azure AD в Shopify Plus.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: e2fa3ac8-a30f-4dcd-8073-ed7c65909feb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2020
ms.author: Zhchia
ms.openlocfilehash: ce791337cb27d3704d1cfa007ed6dca3e8e7e0d9
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98729637"
---
# <a name="tutorial-configure-shopify-plus-for-automatic-user-provisioning"></a>Учебник. Настройка Shopify Plus для автоматической подготовки пользователей

В этом учебнике описываются действия, которые необходимо выполнить в Shopify Plus и Azure Active Directory (Azure AD) для настройки автоматической подготовки пользователей. После настройки Azure AD автоматически подготавливает и отменяет подготовку пользователей и групп для Shopify, а [также](https://www.shopify.com) с помощью службы подготовки Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Поддерживаемые возможности
> [!div class="checklist"]
> * Создание пользователей в Shopify Plus
> * Удалить пользователей в Shopify Plus, когда им больше не нужен доступ
> * Синхронизация атрибутов пользователей между Azure AD и Shopify Plus
> * [Единый вход](./shopify-plus-tutorial.md) в Shopify Plus (рекомендуется)

## <a name="prerequisites"></a>Предварительные требования

В сценарии, описанном в этом руководстве, предполагается, что у вас уже имеется:

* [Клиент Azure AD.](../develop/quickstart-create-new-tenant.md) 
* Учетная запись пользователя в Azure AD с [разрешением](../roles/permissions-reference.md) настраивать подготовку (например, администратор приложений, администратор облачных приложений, владелец приложения или глобальный администратор). 
* Проверьте домен и создайте конфигурацию SAML. Управлять можно только пользователями, связанными с проверенным доменом.

## <a name="step-1-plan-your-provisioning-deployment"></a>Шаг 1. Планирование развертывания для подготовки
1. Узнайте, [как работает служба подготовки](../app-provisioning/user-provisioning.md).
2. Определите, кто будет находиться в [области подготовки](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Определите, какие данные должны [сопоставляться между Azure AD и Shopify Plus](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-shopify-plus-to-support-provisioning-with-azure-ad"></a>Шаг 2. Настройка Shopify Plus для поддержки подготовки с помощью Azure AD

1. Войдите в [Shopify и администратор организации](https://shopify.plus ). Перейдите к разделу **пользователи > безопасность**.

2. Перейдите к разделу **Интеграция scim** и щелкните **создать токен API**.

3. Скопируйте и сохраните созданный маркер. Это значение будет указано в поле **секретный токен** на вкладке Подготовка приложения Shopify Plus в портал Azure.

4. Базовый URL-адрес — `https://shopifyscim.com/scim/v2/`. Это значение будет указано в поле **URL-адрес клиента** на вкладке Подготовка приложения Shopify Plus в портал Azure.

## <a name="step-3-add-shopify-plus-from-the-azure-ad-application-gallery"></a>Шаг 3. Добавление Shopify и из коллекции приложений Azure AD

Добавьте Shopify и из коллекции приложений Azure AD, чтобы начать управление подготовкой в Shopify Plus. Если вы ранее настроили Shopify Plus для единого входа, можно использовать то же приложение. Однако при первоначальном тестировании интеграции рекомендуется создать отдельное приложение. Дополнительные сведения о добавлении приложения из коллекции см. [здесь](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Шаг 4. Определение пользователей для включения в область подготовки 

Служба подготовки Azure AD позволяет определить пользователей, которые будут подготовлены, на основе назначения приложению и (или) атрибутов пользователя или группы. Если вы решили указать, кто именно будет подготовлен к работе в приложении, на основе назначения, можно выполнить следующие [действия](../manage-apps/assign-user-or-group-access-portal.md), чтобы назначить пользователей и группы приложению. Если вы решили указать, кто именно будет подготовлен, на основе одних только атрибутов пользователя или группы, можете использовать фильтр задания области, как описано [здесь](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* При назначении пользователей и групп Shopify Plus необходимо выбрать роль, отличную от **доступа по умолчанию**. Пользователи с ролью "Доступ по умолчанию" исключаются из подготовки и будут помечены в журналах подготовки как не назначенные явно. Кроме того, если эта роль является единственной, доступной в приложении, можно [изменить манифест приложения](../develop/howto-add-app-roles-in-azure-ad-apps.md), чтобы добавить дополнительные роли. 

* Начните с малого. Протестируйте небольшой набор пользователей и групп, прежде чем выполнять развертывание для всех. Если в область подготовки включены назначенные пользователи и группы, проверьте этот механизм, назначив приложению одного или двух пользователей либо одну или две группы. Если в область включены все пользователи и группы, можно указать [фильтр области на основе атрибутов](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-shopify-plus"></a>Шаг 5. Настройка автоматической подготовки пользователей в Shopify Plus 

В этом разделе описывается, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и (или) групп в TestApp на основе их назначений в Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-shopify-plus-in-azure-ad"></a>Чтобы настроить автоматическую подготовку учетных записей пользователей для Shopify Plus в Azure AD, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **Корпоративные приложения**, а затем **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **Shopify Plus**.

    ![Ссылка на Shopify Plus в списке "приложения"](common/all-applications.png)

3. Выберите вкладку **Подготовка**.

    ![Вкладка "Подготовка"](common/provisioning.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Вкладка "Подготовка" с выделенным значением "Авто"](common/provisioning-automatic.png)

5. В разделе **учетные данные администратора** введите URL-адрес клиента Shopify Plus и секретный маркер. Нажмите кнопку **проверить подключение** , чтобы убедиться, что Azure AD может подключиться к Shopify Plus. Если подключение не выполняется, убедитесь, что у учетной записи Shopify Plus есть разрешения администратора, и повторите попытку.

    ![Токен](common/provisioning-testconnection-tenanturltoken.png)

6. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Отправить уведомление по электронной почте при сбое**.

    ![Почтовое уведомление](common/provisioning-notification-email.png)

7. Щелкните **Сохранить**.

8. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory пользователей с Shopify Plus**.

9. Проверьте пользовательские атрибуты, которые синхронизированы из Azure AD, с Shopify Plus в разделе " **сопоставление атрибутов** ". Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в Shopify и для операций обновления. Если вы решили изменить [соответствующий целевой атрибут](../app-provisioning/customize-application-attributes.md), необходимо убедиться, что API Shopify Plus поддерживает фильтрацию пользователей на основе этого атрибута. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

   |attribute|Тип|Поддерживается для фильтрации|
   |---|---|---|
   |userName|Строка|&check;|
   |active|Логическое|
   |name.givenName|Строка|
   |name.familyName|Строка|
   

10. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Чтобы включить службу подготовки Azure AD для Shopify Plus, измените значение параметра **состояние подготовки** на **включено** в разделе **Параметры** .

    ![Состояние подготовки "Включено"](common/provisioning-toggle-on.png)

12. Определите пользователей и (или) группы, которые вы хотите подготавливать к Shopify Plus, выбрав нужные значения в **области** в разделе **Параметры** .

    ![Область действия подготовки](common/provisioning-scope.png)

13. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Сохранение конфигурации подготовки](common/provisioning-configuration-save.png)

После этого начнется цикл начальной синхронизации всех пользователей и групп, определенных в поле **Область** в разделе **Параметры**. Начальный цикл занимает больше времени, чем последующие циклы. Пока служба подготовки Azure AD запущена, они выполняются примерно каждые 40 минут. 

## <a name="step-6-monitor-your-deployment"></a>Шаг 6. Мониторинг развертывания
После настройки подготовки используйте следующие ресурсы для мониторинга развертывания.

1. Используйте [журналы подготовки](../reports-monitoring/concept-provisioning-logs.md), чтобы определить, какие пользователи были подготовлены успешно или неудачно.
2. Используйте [индикатор выполнения](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md), чтобы узнать состояние цикла подготовки и приблизительное время до его завершения.
3. Если конфигурация подготовки, вероятно, находится в неработоспособном состоянии, приложение перейдет в карантин. Дополнительные сведения о режимах карантина см. [здесь](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md)