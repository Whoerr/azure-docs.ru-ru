---
title: Руководство по интеграции Azure Active Directory с Asana | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Asana.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/27/2021
ms.author: jeedes
ms.openlocfilehash: a06c94eed6c90d7b38f28d37f3c8145d4ac1151d
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101651014"
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Руководство. Интеграция Azure Active Directory с Asana

В этом руководстве описано, как интегрировать Asana с Azure Active Directory (Azure AD). Интеграция Asana с Azure AD обеспечивает следующие возможности:

* С помощью Azure AD вы можете контролировать доступ к Asana.
* Вы можете включить автоматический вход пользователей в Asana с учетными записями Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Asana с поддержкой единого входа

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Asana поддерживает единый вход, инициированный **поставщиком услуг**.

* Asana поддерживает [**автоматическую** подготовку пользователей](asana-provisioning-tutorial.md).

## <a name="add-asana-from-the-gallery"></a>Добавление Asana из коллекции

Чтобы настроить интеграцию Asana с Azure AD, необходимо добавить Asana из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Asana**.
1. Выберите **Asana** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-sso-for-asana"></a>Настройка и проверка единого входа Azure AD для Asana

Настройте и проверьте единый вход Azure AD в Asana с помощью тестового пользователя **B.Simon**. Чтобы обеспечить работу единого входа, нужно установить связь между пользователем Azure AD и соответствующим пользователем в Asana.

Чтобы настроить и проверить единый вход Azure AD в Asana, выполните следующие действия:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Asana](#configure-asana-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Asana](#create-asana-test-user)** требуется для того, чтобы в Asana существовал пользователь B.Simon, связанный с представлением того же пользователя в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

### <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Asana** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок карандаша, чтобы открыть диалоговое окно **Базовая конфигурация SAML** для изменения параметров.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Asana](common/sp-identifier.png)

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес: `https://app.asana.com/`

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес: `https://app.asana.com/`.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемый URL-адрес можно скопировать из раздела **Настройка Asana**.

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

В этом разделе описано, как включить единый вход Azure для пользователя B.Simon, предоставив этому пользователю доступ к приложению Asana.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Asana**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="configure-asana-sso"></a>Настройка единого входа в Asana

1. В другом окне браузера войдите в приложение Asana. Чтобы настроить единый вход в Asana, перейдите к параметрам рабочей области, щелкнув имя рабочей области в правом верхнем углу экрана. Затем щелкните **Параметры \<your workspace name\>** .

    ![Параметры единого входа Asana](./media/asana-tutorial/settings.png)

2. В окне **Organization settings** (Параметры организации) щелкните **Administration** (Администрирование). Затем щелкните **Members must log in via SAML** (Участники должны входить с помощью SAML), чтобы активировать конфигурацию единого входа. Выполните следующие действия.

    ![Настройка параметров единого входа организации](./media/asana-tutorial/save.png)  

    а. В текстовое поле **Sign-in page URL** (URL-адрес страницы входа) вставьте **URL-адрес входа**.

    b. Щелкните правой кнопкой мыши сертификат, скачанный с портала Azure, а затем откройте файл сертификата в Блокноте или в любом текстовом редакторе. Скопируйте содержимое между операторами begin и end заголовка сертификата в текстовое поле **X.509 Certificate** (Сертификат X.509).

3. Выберите команду **Сохранить**. Если вам нужна дополнительная помощь, изучите [руководство по настройке единого входа в Asana](https://asana.com/guide/help/premium/authentication#gl-saml) .

### <a name="create-asana-test-user"></a>Создание тестового пользователя Asana

Цель этого раздела — создать пользователя с именем Britta Simon в приложении Asana. Asana поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](asana-provisioning-tutorial.md).

**Если необходимо создать пользователя вручную, выполните следующие действия:**

В этом разделе описано, как создать пользователя Britta Simon в приложении Asana.

1. В приложении **Asana** откройте раздел **Teams** (Группы) на левой панели. Нажмите кнопку со знаком "плюс".

    ![Создание тестового пользователя Azure AD](./media/asana-tutorial/teams.png)

2. Введите адрес электронной почты, например **britta.simon\@contoso.com**, в текстовое поле и выберите **Invite** (Пригласить).

3. Щелкните **Send Invite**(Отправить приглашение). Новый пользователь получит сообщение электронной почты. Он должен создать и подтвердить учетную запись.

### <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в Asana, где можно инициировать поток входа. 

* Перейдите по URL-адресу для входа в Asana и инициируйте поток входа.

* Вы можете использовать портал "Мои приложения" корпорации Майкрософт. Щелкнув элемент Asana на портале "Мои приложения", вы перейдете по URL-адресу для входа в Asana. Дополнительные сведения о портале "Мои приложения" см. в [этой статье](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Дальнейшие действия

После настройки Asana вы можете применить управление сеансами, которое в реальном времени защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).