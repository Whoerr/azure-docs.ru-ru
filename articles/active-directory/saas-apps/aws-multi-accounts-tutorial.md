---
title: Учебник по интеграции Azure Active Directory с Amazon Web Services (AWS) для подключения нескольких учетных записей | Документация Майкрософт
description: Сведения о том, как настроить единый вход между Azure AD и Amazon Web Services (AWS) (устаревший учебник).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/24/2020
ms.author: jeedes
ms.openlocfilehash: 0ddcb239ba106bcdc2f0a29d68eb395876fadc06
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "100384118"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services"></a>Учебник по интеграции Azure Active Directory с Amazon Web Services

В этом учебнике показано, как интегрировать приложение Azure Active Directory (Azure AD) с Amazon Web Services (AWS) (устаревший учебник).

Интеграция обеспечивает приведенные ниже преимущества.

- С помощью Azure AD вы можете контролировать доступ к AWS.
- Вы можете включить автоматический вход пользователей в AWS (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно, через портал Azure.

![Диаграмма интеграции Azure AD с AWS.](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

> [!NOTE]
> Мы *не* рекомендуем подключать одно приложение AWS ко всем учетным записям AWS. Вместо этого мы рекомендуем выполнить [интеграцию единого входа Azure AD с AWS](./amazon-web-service-tutorial.md), чтобы настроить несколько экземпляров учетной записи AWS для нескольких экземпляров приложений AWS в Azure AD. 

Мы *не* рекомендуем подключать одно приложение AWS ко всем своим учетным записям AWS по приведенным ниже причинам.

* Используйте этот подход, только если у вас небольшое количество учетных записей и ролей AWS. Эта модель не масштабируется по мере увеличения количества учетных записей AWS и ролей внутри них. Этот подход не использует функцию импорта ролей AWS с помощью подготовки пользователей Azure AD, поэтому роли нужно вручную добавлять, обновлять или удалять. 

* Вам нужно исправить все роли для приложения с помощью песочницы Microsoft Graph. Мы не рекомендуем использовать файлы манифестов.

* Отчеты клиентов свидетельствуют о том, что после добавления примерно 1200 ролей для одного приложения AWS любая дополнительная операция с приложением приводила к ошибкам, связанным с размером. Установлено жесткое ограничение размера объекта приложения.

* Вы должны вручную обновлять роли по мере их добавления в любую из учетных записей. К сожалению, это требует *замены* сведений, а не *добавления*. Кроме того, если число учетных записей растет, то между учетными записями и ролями устанавливается связь *n* &times; *n*.

* Все учетные записи AWS используют один и тот же XML-файл метаданных федерации. Во время смены сертификата будет непросто одновременно обновить сертификат для всех учетных записей AWS.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с AWS, вам потребуются следующие компоненты:

* Подписка Azure AD. Если у вас нет подписки Azure AD, вы можете получить [пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).
* Подписка AWS с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем без необходимости использовать рабочую среду для проверки действий, описанных в этом учебнике.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

AWS поддерживает единый вход, инициируемый поставщиком службы и поставщиком удостоверений.

## <a name="add-aws-from-the-gallery"></a>Добавление AWS из коллекции

Чтобы настроить интеграцию AWS с Azure AD, необходимо добавить AWS из коллекции в список управляемых приложений SaaS (программное обеспечение как услуга).

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. На левой панели выберите необходимую службу Azure AD.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Amazon Web Services (AWS)** .
1. Выберите **Amazon Web Services** в списке результатов и добавьте это приложение. Через несколько секунд приложение будет добавлено в ваш клиент.

1. Перейдите на панель **Свойства** и скопируйте значение, отображаемое в поле **Идентификатор объекта**.

    ![Снимок экрана с полем идентификатора объекта на панели свойств.](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-properties.png)

## <a name="configure-and-test-azure-ad-sso"></a>Настройка и проверка единого входа Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в AWS с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в AWS соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в AWS.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** AWS.

Чтобы настроить и протестировать единый вход Azure AD в AWS, выполните действия из следующих стандартных блоков:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Настройка единого входа в AWS](#configure-aws-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
1. **[Проверка единого входа](#test-sso)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Используя сведения этого раздела, вы включите единый вход Azure AD на портале Azure и настроите единый вход в приложении AWS. Для этого выполните приведенные ниже действия.

1. На портале Azure на левой панели страницы интеграции приложения **Amazon Web Services (AWS)** выберите **Единый вход**.

    ![Снимок экрана с командой "Единый вход".](common/select-sso.png)

1. В области **Выбрать метод единого входа** выберите режим **SAML/WS-FED**, чтобы включить единый вход.

    ![Снимок экрана панели "Выбрать метод единого входа".](common/select-saml-option.png)

1. На панели **Настройка единого входа с помощью SAML** нажмите кнопку **Изменить** (значок карандаша).

    ![Снимок экрана: кнопка "Изменить" на панели "Настройка единого входа с помощью SAML".](common/edit-urls.png)

1. Откроется панель **Базовая конфигурация SAML**. Пропустите этот раздел, потому что приложение уже предварительно интегрировано с Azure. Щелкните **Сохранить**.

   Приложение AWS ожидает утверждения SAML в определенном формате. Управлять значениями этих атрибутов можно в разделе **User Attributes & Claims** (Атрибуты пользователя и утверждения) на странице **интеграции приложения**. 
   
1. На странице **Настройка единого входа с помощью SAML** нажмите кнопку **Изменить**.

    ![Снимок экрана: кнопка "Изменить" на панели "Атрибуты пользователя".](common/edit-attribute.png)

1. В разделе **Утверждения пользователя** на панели **Атрибуты пользователя** настройте атрибут токена SAML, используя значения следующей таблицы:

    | Имя  | Атрибут источника  | Пространство имен |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | `https://aws.amazon.com/SAML/Attributes` |
    | Роль | user.assignedroles | `https://aws.amazon.com/SAML/Attributes`|
    | SessionDuration | "Укажите значение от 900 секунд (15 минут) до 43 200 секунд (12 часов)." |  `https://aws.amazon.com/SAML/Attributes` |
  
   а. Выберите **Добавить новое утверждение**, а затем на панели **Управление утверждениями пользователя** выполните приведенные ниже действия.

   ![Снимок экрана: кнопки "Добавить новое утверждение" и "Сохранить" на панели "Утверждения пользователя".](common/new-save-attribute.png)

   ![Снимок экрана: панель "Управление утверждениями пользователя".](common/new-attribute-details.png)

   b. В текстовом поле **Имя** введите имя атрибута.  

   c. В текстовом поле **Пространство имен** введите значение пространства имен.

   d. В поле **Источник** выберите **Атрибут**.

   д. Из раскрывающегося списка **Атрибут источника** выберите атрибут.

   f. Нажмите кнопку **ОK**, а затем выберите **Сохранить**.

   >[!NOTE]
   >Дополнительные сведения о ролях в Azure AD см. в разделе о [добавлении ролей приложения в приложение и их получении в токене](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui--preview).

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** выберите **Скачать**, чтобы скачать XML-файл метаданных федерации и сохранить его на компьютере.

   ![Снимок экрана со ссылкой для скачивания "XML-файла метаданных федерации" на панели "Сертификат подписи SAML".](common/metadataxml.png)

### <a name="configure-aws-sso"></a>Настройка единого входа в AWS

1. В другом окне браузера войдите на корпоративный сайт AWS в качестве администратора.

1. Щелкните значок **AWS Home** (Домашняя страница AWS).

   ![Снимок экрана со значком AWS Home (Домашняя страница AWS).][11]

1. На панели **службы AWS** в разделе **Security, Identity & Compliance** (Безопасность, идентификация и соответствие) выберите **IAM** (Система управления идентификацией и доступом).

    ![Снимок экрана со ссылкой на Систему управления идентификацией и доступом на панели службы AWS.][12]

1. На левой панели выберите **Identity Providers** (Поставщики удостоверений), а затем — **Create Provider** (Создать поставщика).

    ![Снимок экрана с кнопкой Create Provider (Создать поставщика).][13]

1. На панели **Configure Provider** (Настройка поставщика) выполните указанные ниже действия.

    ![Снимок экрана: панель Configure Provider (Настройка поставщика).][14]

    а. В раскрывающемся списке **Provider Type** (Тип поставщика) выберите **SAML**.

    b. В текстовом поле **Provider Name** (Имя поставщика) введите имя поставщика (например, *WAAD*).

    c. Рядом с полем **Metadata Document** (Документ метаданных) щелкните **Choose File** (Выбрать файл), чтобы отправить загруженный XML-файл метаданных федерации на портал Azure.

    d. Нажмите кнопку **Next Step** (Следующий шаг).

1. На панели **Verify Provider Information** (Проверка сведений о поставщике) выберите **Create** (Создать).

    ![Снимок экрана с панелью Verify Provider Information (Проверка сведений о поставщике).][15]

1. На левой панели выберите **Roles** (Роли), а затем — **Create role** (Создать роль).

    ![Снимок экрана: кнопка Create role (Создать роль) на панели Roles (Роли).][16]

    > [!NOTE]
    > Общая длина ARN (Имя ресурса Amazon) роли и ARN поставщика SAML для импортируемой роли не должна превышать 240 символов.

1. На странице **Create role** (Создание роли) выполните указанные ниже действия.  

    ![Снимок экрана: кнопка выбора доверенной сущности SAML 2.0 federation (Федерация SAML 2.0) на странице Create role (Создание роли).][19]

    а. В разделе **Select type of trusted entity** (Выбор типа доверенной сущности) выберите **SAML 2.0 federation** (Федерация SAML 2.0).

    b. В разделе **Choose a SAML 2.0 Provider** (Выбор поставщика SAML 2.0) выберите созданный ранее поставщик SAML (например: *WAAD*).

    c. Установите флажок **Allow programmatic and AWS Management Console access** (Разрешить программный доступ и доступ через консоль управления AWS).

    d. По завершении выберите **Next: разрешения**.

1. Выполните поиск по фразе **Administrator Access** (Доступ администратора) на панели поиска, установите флажок **AdministratorAccess** и выберите **Next: Tags** (Далее: теги).

    ![Снимок экрана со списком Policy name (Имя политики) и выбранной политикой AdministratorAccess.](./media/aws-multi-accounts-tutorial/administrator-access.png)

1. На панели **Add tags (optional)** (Добавление тегов (необязательно)) выполните указанные ниже действия.

    ![Снимок экрана с панелью Add tags (optional) (Добавление тегов (необязательно)).](./media/aws-multi-accounts-tutorial/config2.png)

    а. В текстовом поле **Key** (Ключ) введите имя ключа (например, *Azureadtest*).

    b. В текстовом поле **Value (optional)** (Значение (необязательно)) введите значение ключа в следующем формате: `<accountname-aws-admin>`. Имя учетной записи должно быть указано в нижнем регистре.

    c. По завершении выберите **Next: Review** (Далее: проверка).

1. На панели **Review** (Проверка) выполните указанные ниже действия.

    ![Снимок экрана с панелью Review (Проверка) и выделенными полями Role name (Имя роли) и Role description (Описание роли).][34]

    а. В текстовом поле **Role name** (Имя роли) введите значение в следующем формате: `<accountname-aws-admin>`.

    b. В поле **Role description** (Описание роли) введите значение, которое вы использовали для имени роли.

    c. Щелкните **Create Role** (Создать роль).

    d. Создайте необходимое количество ролей и свяжите их с поставщиком удостоверений.

    > [!NOTE]
    > Аналогичным образом можно создать оставшиеся роли, например *accountname-finance-admin*, *accountname-read-only-user*, *accountname-devops-user* или *accountname-tpm-user* с разными политиками. Вы можете изменить эти политики ролей позже в соответствии с требованиями для каждой учетной записи AWS. Рекомендуется использовать одинаковые политики для каждой роли в учетных записях AWS.

1. Обязательно запишите идентификатор для учетной записи AWS либо из панели свойств Amazon Elastic Compute Cloud (Amazon EC2), либо из панели IAM, как показано на следующем снимке экрана:

    ![Снимок экрана: расположение идентификатора учетной записи на панели Identity and Access Management (Управление удостоверениями и доступом).](./media/aws-multi-accounts-tutorial/aws-accountid.png)

1. Войдите на портал Azure и перейдите в раздел **Группы**.

1. Создайте группы с тем же именем, что и у созданных ранее ролей IAM, и запишите значение поля **Идентификаторы объектов** этих новых групп.

    ![Снимок экрана с данными учетной записи для новой группы.](./media/aws-multi-accounts-tutorial/copy-objectids.png)

1. Выйдите из текущей учетной записи AWS и войдите в систему, используя другую учетную запись, в которой нужно настроить единый вход с помощью Azure AD.

1. После создания всех ролей в учетных записях они отображаются в соответствующем списке **Роли**.

    ![Снимок экрана: список ролей с именем роли, описанием и доверенными сущностями.](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-listofroles.png)

Затем нужно зафиксировать все ARN роли и доверенные сущности для всех ролей и учетных записей. Вам нужно сопоставить их вручную с помощью приложения Azure AD. Для этого сделайте следующее.

1. Выберите каждую роль, чтобы скопировать ее ARN и значения доверенных сущностей. Эти значения нужны всем ролям, которые необходимо создать в Azure AD

    ![Снимок экрана с панелью "Сводка" для ARN роли и доверенных сущностей.](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-role-summary.png)

1. Выполните то же самое для всех ролей во всех учетных записях, а затем сохраните их в текстовом файле в следующем формате: `<Role ARN>,<Trusted entities>`.

1. Откройте [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) и выполните указанные действия.

    а. Войдите на сайт Microsoft Graph Explorer с помощью учетных данных глобального администратора или соадминистратора вашего арендатора.

    b. У вас должны быть разрешения на создание ролей. Щелкните **Изменить разрешения**.

      ![Снимок экрана со ссылкой "Изменить разрешения" на панели проверки подлинности Microsoft Graph Explorer.](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. В списке разрешений (если у вас еще нет разрешений, показанных на указанном ниже снимке экрана) выберите каждое из них, а затем щелкните **Изменить разрешения**. 

      ![Снимок экрана со списком разрешений Microsoft Graph Explorer и выделенными соответствующими разрешениями.](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Снова войдите в Graph Explorer и примите условия использования сайта. 

    д. В верхней части панели выберите **GET** в качестве метода, выберите версию **Бета**, а затем в поле запроса введите одно из указанных ниже значений. 
    
    * Чтобы получить все субъекты-службы клиента, используйте `https://graph.microsoft.com/beta/servicePrincipals`. 
    * При использовании нескольких каталогов можно применить шаблон `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`, где содержится ваш основной домен.

    ![Снимок экрана с областью "Текст запроса" в запросе Microsoft Graph Explorer.](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. В полученном списке субъектов-служб найдите ту, в которую нужно внести изменения. 
    
    Для поиска перечисленных субъектов-служб в приложении можно также использовать клавиши CTRL+F. Чтобы получить конкретную субъект-службу, добавьте в запрос идентификатор объекта субъекта-службы, который вы скопировали ранее из панели свойств Azure AD, как показано ниже:

      `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

      ![Снимок экрана: запрос субъекта-службы, которая содержит идентификатор объекта.](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    ж. Извлеките свойство appRoles из объекта субъекта-службы.

    ![Снимок экрана: код для извлечения свойства appRoles из объекта субъекта-службы.](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Теперь вам следует создать роли для приложения. 

    i. Указанный ниже код JSON является примером объекта appRoles. Создайте похожий объект, чтобы добавить необходимые роли в приложение.

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > Вы можете добавлять новые роли только после добавления *msiam_access* для операции исправления. Вы можете добавить столько ролей, сколько нужно вашей организации. Azure AD отправляет *значение* этих ролей в качестве значения утверждения в ответе SAML.

    j. В Microsoft Graph Explorer измените метод с **GET** на **PATCH**. Примените к объекту субъекта-службы нужные роли, обновив свойство appRoles, как показано в примере выше. Нажмите кнопку **Запуск запроса**, чтобы выполнить операцию исправления. Сообщение об успешном выполнении подтвердит создание роли для приложения AWS.

      ![Снимок экрана: панель Microsoft Graph Explorer и метод, который изменен на PATCH.](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

1. Добавив новые роли в субъект-службу, вы сможете назначить эти роли и группы пользователям. Для этого перейдите на портал и найдите приложение AWS. Откройте вкладку **​​Пользователи и группы** в верхней части окна.

1. Мы рекомендуем создавать группы для каждой роли AWS, чтобы вы могли присвоить определенную роль этой группе. Группа сопоставляется с ролью по принципу "один к одному". Это означает, что одной группе назначена одна роль. Затем можно добавить участников, которые принадлежат к этой группе.

1. После создания групп выберите группу и присвойте ее приложению.

    ![Снимок экрана с панелью "Пользователи и группы".](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > Вложенные группы не поддерживаются при назначении групп.

1. Чтобы назначить роль группе, выберите роль, а затем — **Назначить**.

    ![Снимок экрана с панелью "Добавление назначения".](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > После назначения ролей вы можете просмотреть их, обновив сеанс на портале Azure.

### <a name="test-sso"></a>Проверка единого входа

В этом разделе объясняется, как проверить конфигурацию единого входа Azure AD с помощью портала Microsoft "Мои приложения".

Когда вы выбираете плитку **AWS** на портале "Мои приложения", открывается страница приложения AWS с возможностью выбора роли.

![Снимок экрана: страница AWS для тестирования единого входа.](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-screen.png)

Также можно проверить ответ SAML, чтобы увидеть роли, передаваемые как утверждения.

![Снимок экрана с ответом SAML.](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-saml.png)

Дополнительные сведения о портале "Мои приложения" см. в статье о [входе на портал "Мои приложения" и запуске приложений на нем](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Следующие шаги

После настройки AWS вы можете применить функцию управления сеансом, которая в реальном времени защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. Подробные сведения см. в статье об [управлении сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

<!--Image references-->

[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/config3.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials-continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/
