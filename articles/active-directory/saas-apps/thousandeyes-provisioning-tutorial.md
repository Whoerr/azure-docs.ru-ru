---
title: Руководство по Подготовка пользователей для ThousandEyes — Azure AD
description: Узнайте, как настроить Azure Active Directory для автоматической подготовки и отзыва учетных записей пользователей в ThousandEyes.
services: active-directory
author: ArvindHarinder1
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: arvinh
ms.openlocfilehash: fcb53dc22cfb4bf7308b92ee76e5aaaf3bb0dead
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735116"
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Руководство по настройке ThousandEyes для автоматической подготовки пользователей

Цель этого руководства — показать, как в ThousandEyes и Azure AD настроить автоматическую подготовку и отзыв учетных записей пользователей из Azure AD в ThousandEyes. 

## <a name="prerequisites"></a>Предварительные требования

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* клиент Azure Active Directory;
* клиент ThousandEyes с [планом Standard](https://www.thousandeyes.com/pricing) или выше; 
* учетная запись пользователя ThousandEyes с разрешениями администратора. 

> [!NOTE]
> Интеграция подготовки в Azure AD зависит от [API SCIM ThousandEyes](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), доступного для команд ThousandEyes, использующих план Standard или выше.

## <a name="assigning-users-to-thousandeyes"></a>Назначение пользователей в ThousandEyes

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки учетной записи будут синхронизированы только те записи и группы, которые были "назначены" приложению в Azure AD. 

Перед настройкой и включением службы подготовки необходимо решить, какие пользователи или группы в Azure AD представляют пользователей, которым требуется доступ к приложению ThousandEyes. Приняв это решение, можно будет назначить этих пользователей для приложения ThousandEyes, выполнив следующие действия.

[Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>Важные советы по назначению пользователей в ThousandEyes

* Рекомендуется назначить одного пользователя Azure AD в ThousandEyes для тестирования конфигурации подготовки. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователя в ThousandEyes в диалоговом окне назначения необходимо выбрать роль **пользователя** или другую действительную роль для конкретного приложения (если доступно). Роль **Доступ по умолчанию** не подходит для подготовки, и эти пользователи пропускаются.

## <a name="configuring-user-provisioning-to-thousandeyes"></a>Настройка подготовки учетных записей пользователей в ThousandEyes 

В этом разделе описывается подключение вашего каталога Azure AD к API подготовки учетных записей пользователей в ThousandEyes и настройка службы подготовки для создания, обновления и отключения назначенных учетных записей пользователей в ThousandEyes на основе назначения пользователей и групп в Azure AD.

> [!TIP]
> Для ThousandEyes можно также включить единый вход на основе SAML. Для этого следуйте инструкциям на [портале Azure](https://portal.azure.com). Единый вход можно настроить независимо от автоматической подготовки, хотя эти две возможности дополняют друг друга.

### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Настройка автоматической подготовки учетных записей пользователей для ThousandEyes в Azure AD

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **Корпоративные приложения**, а затем **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Если в ThousandEyes уже настроен единый вход, найдите свой экземпляр ThousandEyes с помощью поля поиска. В противном случае щелкните **Добавить** и выполните поиск **ThousandEyes** в коллекции приложений. Выберите ThousandEyes в результатах поиска и добавьте в список приложений.

    ![Ссылка на ThousandEyes в списке приложений](common/all-applications.png)
    
3. Выберите экземпляр ThousandEyes, а затем перейдите на вкладку **Подготовка**.

    ![Вкладка "Подготовка"](common/provisioning.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

![На снимке экрана показана вкладка подготовки для ThousandEyes с выбранным режимом подготовки Automatic (Автоматический).](./media/thousandeyes-provisioning-tutorial/ThousandEyes1.png)
    

5. В разделе **Учетные данные администратора** введите **Токен носителя OAuth**, созданный вашей учетной записью ThousandEyes (его можно найти и (или) сгенерировать в своей учетной записи ThousandEyes в разделе **Профиль**).

    ![На снимке экрана показано, где найти ссылку на параметры учетной записи для текущей группы учетных записей.](./media/thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. На портале Azure щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к приложению ThousandEyes. Если установить подключение не удалось, убедитесь, что у учетной записи ThousandEyes есть разрешения администратора, и повторите шаг 5.

7. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Отправить уведомление по электронной почте при сбое**.

    ![Почтовое уведомление](common/provisioning-notification-email.png)

8. Выберите команду **Сохранить**.

9. В разделе "Сопоставления" выберите **Synchronize Azure Active Directory Users to ThousandEyes** (Синхронизировать пользователей Azure Active Directory с ThousandEyes).

10. В разделе **Сопоставления атрибутов** просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD в ThousandEyes. Атрибуты, выбранные как **совпадающие** свойства, используются для сопоставления учетных записей пользователей в Parsable для операций обновления. Если вы решили изменить [совпадающий целевой атрибут](../app-provisioning/customize-application-attributes.md), потребуется убедиться, что API Parsable поддерживает фильтрацию пользователей по этому атрибуту. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

     |attribute|Тип|Поддерживается для фильтрации|
     |---|---|---|
     |externalId|Строка|&check;|
     |userName|Строка|&check;|
     |active|Логическое|
     |displayName|Строка|
     |emails[type eq "work"].value|Строка|
     |name.formatted|Строка|


11. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

12. Чтобы включить службу подготовки Azure AD для ThousandEyes, измените значение параметра **Состояние подготовки** на **Включено** в разделе **Параметры**.

    ![Состояние подготовки "Включено"](common/provisioning-toggle-on.png)

13. Определите пользователей и (или) группы для подготовки в ThousandEyes, выбрав нужные значения в поле **Область** в разделе **Параметры**.

    ![Область действия подготовки](common/provisioning-scope.png)

14. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

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