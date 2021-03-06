---
title: Руководство по по интеграции Azure Active Directory с Tableau Online | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Tableau Online.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/28/2020
ms.author: jeedes
ms.openlocfilehash: 5318570ed77e3352f37c2306ecd003195992d010
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98732006"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-tableau-online"></a>Руководство по Интеграция единого входа Azure Active Directory с Tableau Online

В этом руководстве вы узнаете, как интегрировать Tableau Online с Azure Active Directory (Azure AD). Интеграция Tableau Online с Azure AD обеспечивает приведенные ниже возможности.

* Контроль доступа к Tableau Online с помощью Azure AD.
* Автоматический вход пользователей в Tableau Online с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Tableau Online с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Tableau Online поддерживает единый вход, инициируемый **поставщиком услуг**.

## <a name="adding-tableau-online-from-the-gallery"></a>Добавление Tableau Online из коллекции.

Чтобы настроить интеграцию Tableau Online с Azure AD, необходимо добавить Tableau Online из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Tableau Online**.
1. Выберите **Tableau Online** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-sso-for-tableau-online"></a>Настройка и проверка единого входа Azure AD для Tableau Online

В этом разделе описана настройка и проверка единого входа Azure AD в Tableau Online с использованием тестового пользователя **Britta Simon**.
Чтобы единый вход функционировал, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Tableau Online.

Чтобы настроить и протестировать единый вход Azure AD в Tableau Online, сделайте следующее.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Tableau Online](#configure-tableau-online-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Tableau Online](#create-tableau-online-test-user)** нужно для того, чтобы в Tableau Online существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

### <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Tableau Online** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес: `https://sso.online.tableau.com/public/sp/login?alias=<entityid>`

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес: `https://sso.online.tableau.com/public/sp/metadata?alias=<entityid>`.

    > [!NOTE]
    > Значение `<entityid>` см. на странице **настройки Tableau Online**. Значением идентификатора сущности является **идентификатор Azure AD** на странице **настройки Tableau Online**.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемые URL-адреса вы можете скопировать на странице **настройки Tableau Online**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B.Simon, предоставив этому пользователю доступ к приложению Tableau Online.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Tableau Online**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-tableau-online-sso"></a>Настройка единого входа в Tableau Online

1. Для автоматизации настройки в Tableau Online необходимо установить **Расширение браузера "Безопасный вход в мои приложения"** , щелкнув **Установить расширение**.

    ![Расширение "Мои приложения"](common/install-myappssecure-extension.png)

2. После добавления расширения в браузере щелкните **Установка Tableau Online**, чтобы перейти к приложению Tableau Online. После этого укажите учетные данные администратора для входа в Tableau Online. Расширение браузера автоматически настроит приложение и автоматизирует шаги 3–7.

    ![Настройка конфигурации](common/setup-sso.png)

3. Если вы хотите настроить Tableau Online вручную, в другом окне браузера войдите на свой корпоративный сайт Tableau Online с правами администратора.

1. Выберите **Параметры**, а затем щелкните **Проверка подлинности**.

    ![Снимок экрана: меню "Параметры" с выбранным пунктом "Аутентификация".](./media/tableauonline-tutorial/tutorial_tableauonline_09.png)

2. Чтобы включить SAML, в разделе **Типы проверки подлинности** установите флажок **Enable an additional authentication method** (Включить дополнительный метод проверки подлинности) и флажок **SAML**.

    ![Снимок экрана: раздел "Типы аутентификации", где можно выбрать значения.](./media/tableauonline-tutorial/tutorial_tableauonline_12.png)

3. Прокрутите вниз до раздела **Import metadata file into Tableau Online** (Импорт файла метаданных в Tableau Online).  Нажмите кнопку "Обзор" и импортируйте файл метаданных, скачанный из Azure AD. Нажмите кнопку **Применить**.

   ![Снимок экрана: раздел для импорта файла метаданных.](./media/tableauonline-tutorial/tutorial_tableauonline_13.png)

4. В разделе **Match assertions** (Сопоставление утверждений) вставьте соответствующее имя утверждения поставщика удостоверений для **электронной почты**, **имени** и **фамилии**. Чтобы получить эти сведения из Azure AD, сделайте следующее: 
  
    а. Перейдите на страницу интеграции с приложением **Tableau Online** портала Azure.

    b. В разделе **Атрибуты и утверждения пользователя** щелкните значок правки.

   ![Снимок экрана: раздел "Атрибуты и утверждения пользователя", где можно выбрать значок "Изменить".](./media/tableauonline-tutorial/attributesection.png)

    c. Скопируйте значение пространства имен для этих атрибутов: имя, электронная почта и фамилия, выполнив следующие действия:

   ![Снимок экрана: атрибуты "Заданное имя", "Фамилия" и "Адрес электронной почты".](./media/tableauonline-tutorial/tutorial_tableauonline_10.png)

    d. Щелкните значение **user.givenname**.

    д) Скопируйте значение из текстового поля **Пространство имен**.

    ![Снимок экрана: раздел "Управление утверждениями пользователя", где можно ввести пространство имен.](./media/tableauonline-tutorial/attributesection2.png)

    е) Чтобы скопировать значения пространства имен для электронной почты и фамилии, повторите описанные выше шаги.

    ж. Перейдите к приложению Tableau Online и задайте в разделе **Утверждения и атрибуты пользователя** следующее:

    * электронный адрес: **mail** или **userprincipalname**;

    * имя: **givenname**

    * фамилия: **surname**

    ![Снимок экрана: раздел "Сопоставление атрибутов", в котором можно ввести значения.](./media/tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="create-tableau-online-test-user"></a>Создание тестового пользователя Tableau Online

В этом разделе описано, как создать пользователя Britta Simon в приложении Tableau Online.

1. В приложении **Tableau Online** щелкните **Settings** (Параметры), а затем выберите раздел **Authentication** (Проверка подлинности). Прокрутите вниз до раздела **Управление пользователями**. Щелкните **Add Users** (Добавить пользователей), а затем — **Enter Email Addresses** (Ввести адреса электронной почты).
  
    ![Снимок экрана: раздел "Управление пользователями", в котором можно добавить пользователей.](./media/tableauonline-tutorial/tutorial_tableauonline_15.png)

2. Выберите **Add users for (SAML) authentication** (Добавить пользователей для проверки подлинности (SAML). В текстовое поле **Enter email addresses** (Введите адреса электронной почты) добавьте britta.simon\@contoso.com
  
    ![Снимок экрана: страница "Добавление пользователей", на которой можно ввести адрес электронной почты.](./media/tableauonline-tutorial/tutorial_tableauonline_11.png)

3. Щелкните **Добавить пользователей**.

### <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов.

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в Tableau Online, где можно инициировать поток входа.

* Перейдите по URL-адресу для входа в Tableau Online и инициируйте поток входа.

* Вы можете использовать портал "Мои приложения" корпорации Майкрософт. Щелкнув элемент Tableau Online на портале "Мои приложения", вы перейдете по URL-адресу для входа в Tableau Online. Дополнительные сведения о портале "Мои приложения" см. в [этой статье](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Дальнейшие действия

После настройки Tableau Online можно применять элементы управления сеансами, которые защищают от хищения и несанкционированного доступа к конфиденциальным данным вашей организации в режиме реального времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).