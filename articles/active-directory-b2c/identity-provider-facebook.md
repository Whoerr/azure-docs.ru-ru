---
title: Настройка регистрации и входа с помощью учетной записи Facebook
titleSuffix: Azure AD B2C
description: Вы можете организовать в приложениях регистрацию и вход для клиентов с учетными записями Facebook, используя Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/19/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: e0e71bc0e3a81b5ab2f455224ed2ed4281532d55
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98952680"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Настройка регистрации и входа с учетной записью Facebook через Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-facebook-application"></a>Создание приложения Facebook

Чтобы включить вход для пользователей с учетной записью Facebook в Azure Active Directory B2C (Azure AD B2C), необходимо создать приложение на [панели мониторинга приложений Facebook](https://developers.facebook.com/). Дополнительные сведения см. в разделе [Разработка приложений](https://developers.facebook.com/docs/development). Если у вас нет учетной записи Facebook, вы можете зарегистрироваться здесь: [https://www.facebook.com/](https://www.facebook.com/).

1. Выполните вход на [веб-сайт разработчиков Facebook](https://developers.facebook.com/) с учетными данными от учетной записи Facebook.
1. Если вы этого еще не сделали, необходимо зарегистрироваться в качестве разработчика Facebook. Для этого щелкните **Get Started** (Начать работу) в правом верхнем углу страницы, примите политики Facebook и завершите регистрацию.
1. Щелкните **My Apps** (Мои приложения) и **Create App** (Создать приложение).
1. Выберите **сборка с подключением**.
1. Введите **Отображаемое название** и **Эл. адрес для связи**.
1. Щелкните **Create App ID** (Создать идентификатор приложения). Для этого может потребоваться принять условия политики платформы Facebook и пройти проверку безопасности в сети.
1. Выберите **Параметры** > **Основные**.
    1. Выберите **категорию**, например `Business and Pages`. Это значение требуется Facebook, но не используется для Azure AD B2C.
    1. Введите URL-адрес для **терминов URL службы**, например `http://www.contoso.com/tos` . URL-адрес политики — это страница, которую вы поддерживаете для предоставления условий для приложения.
    1. Введите **URL-адрес политики конфиденциальности**, например `http://www.contoso.com/privacy`. URL-адрес политики — это управляемая вами страница, где предоставляются сведения о конфиденциальности для вашего приложения.
1. В нижней части страницы, выберите **Добавить платформу**, а затем выберите **Веб-сайт**.
1. В поле **URL-адрес сайта** введите адрес веб-сайта, например `https://contoso.com` . 
1. Щелкните **Save changes** (Сохранить изменения).
1. В верхней части страницы скопируйте значение **APP ID** (Идентификатор приложения).
1. Щелкните **Show** (Показать) и скопируйте значение **App Secret** (Секрет приложения). Оба значения потребуются вам для настройки Facebook в качестве поставщика удостоверений для вашего клиента. **Секрет приложения** — это важные учетные данные безопасности.
1. В меню щелкните знак « **плюс»** рядом с пунктом **товары**. В разделе **Добавление продуктов в приложение** выберите **Настройка** в поле **имя входа Facebook**.
1. В меню выберите **имя входа Facebook** и щелкните **Параметры**.
1. В поле **Valid OAuth redirect URIs** (Допустимые URI перенаправления OAuth) введите `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Замените `your-tenant-name` именем вашего клиента. Щелкните **Save Changes** (Сохранить изменения) в нижней части страницы.
1. Чтобы сделать приложение Facebook доступным для Azure AD B2C, щелкните селектор состояния в правом верхнем углу страницы и **включите его,** чтобы сделать приложение общедоступным, а затем выберите **режим переключения**.  На этом этапе состояние должно измениться с **Разработка** на **Активно**.

::: zone pivot="b2c-user-flow"

## <a name="configure-facebook-as-an-identity-provider"></a>Настройка Facebook в качестве поставщика удостоверений

1. Войдите на [портал Azure](https://portal.azure.com/) с правами глобального администратора клиента Azure AD B2C.
1. Убедитесь, что используете каталог с клиентом Azure AD B2C, выбрав фильтр **Каталог и подписка** в меню вверху и каталог с вашим клиентом.
1. Выберите **Все службы** в левом верхнем углу окна портала Azure, найдите службу **Azure AD B2C** и выберите ее.
1. Щелкните элемент **Поставщики удостоверений** и выберите вариант **Facebook**.
1. Введите **Имя**. Например, *Facebook*.
1. В поле **Идентификатор клиента** введите идентификатор созданного ранее приложения Facebook.
1. В поле **Секрет клиента** введите секрет приложения, записанный ранее.
1. Щелкните **Сохранить**.

## <a name="add-facebook-identity-provider-to-a-user-flow"></a>Добавление поставщика удостоверений Facebook в поток пользователя 

1. В клиенте Azure AD B2C выберите **Потоки пользователей**.
1. Щелкните поток пользователя, для которого требуется добавить поставщик удостоверений Facebook.
1. В разделе **поставщики удостоверений социальных сетей** выберите **Facebook**.
1. Щелкните **Сохранить**.
1. Чтобы проверить политику, выберите пункт **выполнить пользовательскую последовательность**.
1. Для **приложения** выберите веб-приложение с именем *testapp1* , которое вы зарегистрировали ранее. В поле **URL-адрес ответа** должно содержаться значение `https://jwt.ms`.
1. Щелкните **выполнить поток пользователя**

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>Создание ключа политики

Необходимо сохранить секрет приложения, записанный ранее в клиент Azure AD B2C.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Убедитесь, что вы используете каталог, содержащий клиент Azure AD B2C. Выберите фильтр **Каталог и подписка** в верхнем меню и выберите каталог, который содержит ваш клиент.
3. Выберите **Все службы** в левом верхнем углу окна портала Azure, а затем найдите и выберите **Azure AD B2C**.
4. На странице "Обзор" выберите **Identity Experience Framework**.
5. Выберите **Ключи политики**, а затем щелкните **Добавить**.
6. Для пункта **Параметры** выберите `Manual`.
7. Введите **имя** ключа политики. Например, `FacebookSecret`. Префикс `B2C_1A_` будет автоматически добавлен к имени ключа.
8. В качестве **секрета** введите ранее записанный секрет приложения.
9. Для параметра **Использование ключа** выберите `Signature`.
10. Нажмите кнопку **Создать**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Настройка учетной записи Facebook в качестве поставщика удостоверений

1. В файле `SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`** замените значение `client_id` идентификатором приложения Facebook:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

## <a name="upload-and-test-the-policy"></a>Отправка и тестирование политики

Обновите файл проверяющей стороны, который активирует созданный путь взаимодействия пользователя.

1. Отправьте файл *TrustFrameworkExtensions.xml* в клиент.
1. В разделе **Настраиваемые политики** выберите **B2C_1A_signup_signin**.
1. В поле **выберите приложение** выберите веб-приложение с именем *testapp1* , которое вы зарегистрировали ранее. В поле **URL-адрес ответа** должно содержаться значение `https://jwt.ms`.
1. Щелкните **Запустить сейчас** и выберите Facebook, чтобы войти с учетной записью Facebook и проверить работу настраиваемой политики.

::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как [передать маркер Facebook в приложение](idp-pass-through-user-flow.md).
