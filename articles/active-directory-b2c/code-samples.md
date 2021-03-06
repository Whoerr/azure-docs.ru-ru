---
title: Примеры кода Azure Active Directory B2C | Документация Майкрософт
description: Примеры кода для мобильных, классических и одностраничных веб-приложений, а также веб-приложений Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/02/2020
ms.custom: mvc
ms.topic: sample
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 8642f4284770df54704a4e7066054d3bb4640486
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "96545878"
---
# <a name="azure-active-directory-b2c-code-samples"></a>Примеры кода Azure Active Directory B2C

В следующей таблице приведены ссылки на примеры приложений, включая iOS, Android, .NET и Node.js.

## <a name="mobile-and-desktop-apps"></a>Мобильные и классические приложения

| Образец | Описание |
|--------| ----------- |
| [ios-swift-native-msal](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal) | Приложение iOS в Swift для аутентификации пользователей Azure AD B2C и вызова API с помощью OAuth 2.0. |
| [android-native-msal](https://github.com/Azure-Samples/ms-identity-android-java#b2cmodefragment-class) | Простое приложение Android для аутентификации пользователей с помощью Azure Active Directory B2C и MSAL и получения доступа к веб-API с помощью полученных маркеров. |
| [ios-native-appauth](https://github.com/Azure-Samples/active-directory-b2c-ios-native-appauth) | Создаваемое с помощью сторонних библиотек приложение iOS на Objective-C для аутентификации пользователей Microsoft Identity в службе удостоверений Microsoft Azure AD B2C. |
| [android-native-appauth](https://github.com/Azure-Samples/active-directory-b2c-android-native-appauth) | Создаваемое с помощью сторонних библиотек приложение Android для аутентификации пользователей Microsoft Identity в службе удостоверений Microsoft Azure AD B2C и вызова веб-API с помощью маркеров доступа OAuth 2.0. |
| [dotnet-desktop](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) | Классическое приложение .NET (WPF) для входа пользователей с помощью Azure AD B2C, получения маркера доступа с помощью MSAL.NET и вызова API. |
| [xamarin-native](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Простое приложение Xamarin Forms для аутентификации пользователей с помощью Azure Active Directory B2C и MSAL и получения доступа к веб-API с помощью полученных маркеров. |

## <a name="web-apps-and-apis"></a>Веб-приложения и API

| Образец | Описание |
|--------| ----------- |
| [dotnet-webapp-and-webapi](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) | Веб-приложение .NET для вызова веб-API .NET (оба компонента защищены с помощью Azure AD B2C). |
| [dotnetcore-webapp-openidconnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-5-B2C) | Веб-приложение ASP.NET Core, использующее OpenID Connect для входа пользователей в Azure AD B2C. |
| [dotnetcore-webapp-msal-api](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/4-WebApp-your-API/4-2-B2C) | Веб-приложение ASP.NET Core для входа пользователей с помощью Azure AD B2C, получения маркера доступа с помощью MSAL.NET и вызова API. |
| [openidconnect-nodejs](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS) | Приложение Node.js для быстрой и простой настройки веб-приложения с помощью компонента "Экспресс" и OpenID Connect. |
| [javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | Небольшой веб-API Node.js для защиты веб-API и принятия маркеров доступа B2C с помощью passport.js в Azure AD B2C. |
| [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/master/README_B2C.md) | Демонстрация интеграции (на уровне B2C) платформы удостоверений Майкрософт с веб-приложением Python.  |

## <a name="single-page-apps"></a>Одностраничные приложения

| Образец | Описание |
|--------| ----------- |
| [ms-identity-b2c-javascript-spa](https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa) | Одностраничное приложение (SPA) для вызова веб-API. Аутентификация осуществляется с помощью Azure AD B2C и MSAL.js. В этом примере используется поток кода авторизации с PKCE. |
| [javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) | Одностраничное приложение (SPA) для вызова веб-API. Аутентификация осуществляется с помощью Azure AD B2C и MSAL.js. В этих примерах используется неявный поток.|

## <a name="saml-test-application"></a>Тестовое приложение SAML

| Образец | Описание |
|--------| ----------- |
| [saml-sp-tester](https://github.com/azure-ad-b2c/saml-sp-tester/tree/master/source-code) | Тестовое приложение SAML для тестирования Azure AD B2C настроено в качестве поставщика удостоверений SAML. |

## <a name="api-connectors"></a>Соединители API

В приведенных ниже таблицах содержатся ссылки на примеры кода для использования веб-API в потоках пользователей с применением [соединителей API](api-connectors-overview.md).

### <a name="azure-function-quickstarts"></a>Краткие руководства по функциям Azure

| Образец                                                                                                                          | Описание                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-api-connector-azure-function-validate) | В этом примере функции Azure для .NET Core показано, как ограничить число регистраций для определенных доменов электронной почты и проверить предоставляемые пользователями сведения. |
| [Node.js](https://github.com/Azure-Samples/active-directory-nodejs-external-identities-api-connector-azure-function-validate)   | В этом примере функции Azure для Node.js показано, как ограничить число регистраций для определенных доменов электронной почты и проверить предоставляемые пользователями сведения.  |
| [Python](https://github.com/Azure-Samples/active-directory-python-external-identities-api-connector-azure-function-validate)    | В этом примере функции Azure для Python показано, как ограничить число регистраций для определенных доменов электронной почты и проверить предоставляемые пользователями сведения.    |


### <a name="automated-fraud-protection-services--captcha"></a>Автоматизированные службы защиты от мошенничества и CAPTCHA
| Образец                                                                                                            | Описание                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [Arkose Labs: защита от мошенничества и нарушений](https://github.com/Azure-Samples/active-directory-b2c-node-sign-up-user-flow-arkose) | В этом примере показано, как обеспечить безопасность регистрации пользователей с помощью службы Arkose Labs для защиты от мошенничества и нарушений. |
| [reCAPTCHA](https://github.com/Azure-Samples/active-directory-b2c-node-sign-up-user-flow-captcha) | В этом примере показано, как обеспечить безопасность регистрации пользователей с помощью задачи reCAPTCHA, предотвращающей автоматизированные нарушения. |


### <a name="identity-verification"></a>Проверка личности

| Образец                                                                                                            | Описание                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [IDology](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-idology-identity-verification) | В этом примере показано, как проверить удостоверение пользователя в рамках потоков регистрации с помощью соединителя API для интеграции с IDology. |
| [Experian](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-experian-identity-verification) | В этом примере показано, как проверить удостоверение пользователя в рамках потоков регистрации с помощью соединителя API для интеграции с Experian. |


### <a name="other"></a>Другое

| Образец                                                                                                            | Описание                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [Код приглашения](https://github.com/Azure-Samples/active-directory-b2c-node-sign-up-user-flow-invitation-code) | В этом примере показано, как ограничить регистрацию определенной аудиторией с помощью кодов приглашения.|
| [Предоставленные сообществом примеры использования соединителей API](https://github.com/azure-ad-b2c/api-connector-samples) | В этом репозитории содержатся поддерживаемые сообществом примеры сценариев, реализуемых с помощью соединителей API.|
