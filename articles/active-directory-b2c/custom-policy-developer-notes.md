---
title: Примечания о настраиваемых политиках для разработчиков
titleSuffix: Azure AD B2C
description: Примечания для разработчиков по настройке и обслуживанию Azure AD B2C с помощью настраиваемых политик.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/19/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 18a3216855516156792524dc577ecef725d3119d
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2021
ms.locfileid: "99218780"
---
# <a name="developer-notes-for-custom-policies-in-azure-active-directory-b2c"></a>Примечания о настраиваемых политика в Azure Active Directory B2C для разработчиков

Конфигурация настраиваемых политик в Azure Active Directory B2C стала общедоступной. Этот метод настройки предназначен для опытных разработчиков удостоверений, создающих сложные решения удостоверений. Настраиваемые политики Azure AD B2C позволяют использовать в арендаторах Azure AD B2C все возможности Identity Experience Framework.
Опытным разработчикам удостоверений, которые используют настраиваемые политики, следует ознакомиться с некоторыми пошаговыми руководствами и справочными материалами.

Хотя большинство доступных параметров настраиваемых политик стали общедоступными, некоторые базовые возможности, например типы технических профилей и API определения содержимого, находятся на разных этапах жизненного цикла программного обеспечения. И ожидается много новых возможностей. В таблице ниже уровни доступности описаны более подробно.

## <a name="features-that-are-generally-available"></a>Возможности, которые стали общедоступными

- Создавать и отправлять пользовательские процедуры проверки подлинности с помощью настраиваемых политик.
    - Опишите пути взаимодействия пользователей по шагам с обменом данными между поставщиками утверждений.
    - Определять условные ветвления путей взаимодействия пользователей.
- Взаимодействие со службами, поддерживающими REST API, в настраиваемых путях аутентификации пользователей.
- Федерация с поставщиками удостоверений, которые поддерживают протокол OpenIDConnect.
- Федерация с поставщиками удостоверений, которые поддерживают протокол SAML 2.0.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Обязанности разработчиков набора функций настраиваемой политики

Настройка политик вручную предоставляет низкоуровневый доступ к базовой платформе Azure AD B2C и позволяет создать уникальную инфраструктуру доверия. Многие из возможных перестановок пользовательских поставщиков удостоверений, отношений доверия, интеграции с внешними службами, а также пошаговые рабочие процессы нуждаются в методический подходе к проектированию и настройке.

Мы рекомендуем разработчикам, которые используют возможности настраиваемой политики, придерживаться следующих рекомендаций:

- Ознакомьтесь с языком конфигурации настраиваемых политик и научитесь управлять ключами и секретами. Дополнительные сведения см. в описании [TrustFrameworkPolicy](trustframeworkpolicy.md).
- Станьте владельцем сценариев и настраиваемой интеграции. Документируйте работу над сайтом и информируйте всех заинтересованных лиц.
- Выполняйте систематическое тестирование сценария.
- Следуйте рекомендациям по разработке и промежуточному развертыванию программного обеспечения. Рекомендуется использовать как минимум одну среду разработки и тестирования.
- Будьте в курсе последних разработок поставщика удостоверений и интегрируемых служб. Например, получайте информацию об изменениях в секретах, запланированных и незапланированных изменениях в службах и т. д.
- Настройте активный мониторинг и отслеживайте скорость реагирования рабочих сред. Дополнительные сведения об интеграции с Application Insights см. в статье [Отслеживание поведения пользователей в Azure Active Directory B2C с использованием Application Insights ](analytics-with-application-insights.md).
- Используйте текущий контактный адрес электронной почты в подписке Azure, чтобы реагировать на сообщения службы поддержки действующего сайта Майкрософт.
- Принимайте своевременные меры, рекомендованные службой поддержки активного сайта Майкрософт.

## <a name="terms-for-features-in-public-preview"></a>Условия использования предварительной версии возможностей, которая стала общедоступной

- Мы рекомендуем использовать предварительную версию функций, которая стала общедоступной, исключительно для ознакомления.
- К таким функциям не применяются соглашения об уровне обслуживания.
- Запросы в службу поддержки, имеющие отношение к этим возможностям, можно отправлять по обычным каналам поддержки.

## <a name="features-by-stage-and-known-issues"></a>Доступность функций на различных стадиях и известные проблемы

Возможности настраиваемой политики находятся в режиме постоянной разработки. Следующая таблица содержит сведения о доступности функций и компонентов.


### <a name="protocols-and-authorization-flows"></a>Протоколы и потоки проверки подлинности

| Компонент | Разработка | Preview (Предварительный просмотр) | GA | Примечания |
|-------- | :-----------: | :-------: | :--: | ----- |
| [Код авторизации OAuth2](authorization-code-flow.md) |  |  | X |  |
| Код авторизации OAuth2 с PKCE |  |  | X | [Открытые клиенты и одностраничные приложения](authorization-code-flow.md)  |
| [Неявный поток OAuth2](implicit-flow-single-page-application.md) |  |  | X |  |
| [Учетные данные владельца ресурса OAuth2](ropc-custom.md) |  | X |  |  |
| [OIDC Connect](openid-connect.md) |  |  | X |  |
| [SAML2](connect-with-saml-service-providers.md)  |  |  |X  | Привязки POST и перенаправления. |
| OAuth1 |  |  |  | Не поддерживается. |
| WSFED | X |  |  |  |

### <a name="identify-providers-federation"></a>Федерация с поставщиками удостоверений 

| Компонент | Разработка | Preview (Предварительный просмотр) | GA | Примечания |
|-------- | :-----------: | :-------: | :--: | ----- |
| [OpenID Connect](openid-connect-technical-profile.md) |  |  | X | Например, Google+.  |
| [OAuth2](oauth2-technical-profile.md) |  |  | X | Например, Facebook.  |
| [OAuth1](oauth1-technical-profile.md) |  | X |  | Например, Twitter. |
| [SAML2](saml-identity-provider-technical-profile.md) |  |   | X | Например, Salesforce и AD FS. |
| WSFED| X |  |  |  |


### <a name="rest-api-integration"></a>Интеграция REST API.

| Компонент | Разработка | Preview (Предварительный просмотр) | GA | Примечания |
|-------- | :-----------: | :-------: | :--: | ----- |
| [REST API с обычной проверкой подлинности](secure-rest-api.md#http-basic-authentication) |  |  | X |  |
| [REST API с проверкой подлинности по сертификату клиента](secure-rest-api.md#https-client-certificate-authentication) |  |  | X |  |
| [REST API с проверкой подлинности по носителю OAuth2](secure-rest-api.md#oauth2-bearer-authentication) |  | X |  |  |

### <a name="component-support"></a>Поддержка компонентов

| Компонент | Разработка | Preview (Предварительный просмотр) | GA | Примечания |
| ------- | :-----------: | :-------: | :--: | ----- |
| [Проверка подлинности по телефону](phone-factor-technical-profile.md) |  |  | X |  |
| [Проверка подлинности Azure AD MFA](multi-factor-auth-technical-profile.md) |  | X |  |  |
| [Одноразовый пароль](one-time-password-technical-profile.md) |  | X |  |  |
| [Azure Active Directory](active-directory-technical-profile.md) в качестве локального каталога |  |  | X |  |
| Подсистема электронной почты Azure для проверки подлинности по электронной почте |  |  | X |  |
| [Сторонние поставщики услуг электронной почты](custom-email-mailjet.md) |  |X  |  |  |
| [Многоязыковая поддержка](localization.md)|  |  | X |  |
| [Проверки предикатов](predicates.md) |  |  | X | Например, сложность паролей. |
| [Элементы управления отображением](display-controls.md) |  |X  |  |  |


### <a name="app-ief-integration"></a>Интеграция приложения с Identity Experience Framework

| Компонент | Разработка | Preview (Предварительный просмотр) | GA | Примечания |
| ------- | :-----------: | :-------: | :--: | ----- |
| Параметр строки запроса `domain_hint` |  |  | X | Доступен в качестве утверждения; можно передать поставщику удостоверений. |
| Параметр строки запроса `login_hint` |  |  | X | Доступен в качестве утверждения; можно передать поставщику удостоверений. |
| Вставка данных в формате JSON в путь взаимодействия пользователя с помощью `client_assertion` | X |  |  | Будет считаться устаревшим. |
| Вставка данных в формате JSON в путь взаимодействия пользователя в виде `id_token_hint` |  | X |  | Правильный подход к передаче данных в формате JSON. |
| [Передача токена поставщика удостоверений в приложение](idp-pass-through-user-flow.md) |  | X |  | Например, из Facebook в приложение. |


### <a name="session-management"></a>Управление сеансом

| Компонент | Разработка | Preview (Предварительный просмотр) | GA | Примечания |
| ------- | :-----------: | :-------: | :--: | ----- |
| [Поставщик сеансов единого входа по умолчанию](custom-policy-reference-sso.md#defaultssosessionprovider) |  |  | X |  |
| [Внешний поставщик сеансов внешнего входа](custom-policy-reference-sso.md#externalloginssosessionprovider) |  |  | X |  |
| [Поставщик сеансов единого входа SAML](custom-policy-reference-sso.md#samlssosessionprovider) |  |  | X |  |
| [OAuthSSOSessionProvider](custom-policy-reference-sso.md#oauthssosessionprovider)  |  | X |  |  |
| [Единый выход](session-behavior.md#sign-out)  |  | X |  |  |

### <a name="security"></a>Безопасность

| Компонент | Разработка | Preview (Предварительный просмотр) | GA | Примечания |
|-------- | :-----------: | :-------: | :--: | ----- |
| Ключи политики — создание, смена вручную и передача |  |  | X |  |
| Ключи политики — RSA или сертификаты, секреты |  |  | X |  |


### <a name="developer-interface"></a>Интерфейс разработчика

| Компонент | Разработка | Preview (Предварительный просмотр) | GA | Примечания |
| ------- | :-----------: | :-------: | :--: | ----- |
| Пользовательский интерфейс Identity Experience Framework на портале Azure |  |  | X |  |
| Отправка политики |  |  | X |  |
| [Журналы пути взаимодействия пользователя в Application Insights](troubleshoot-with-application-insights.md) |  | X |  | Используется для устранения неполадок на этапе разработки.  |
| [Журналы событий в Application Insights](analytics-with-application-insights.md) |  | X |  | Используется для мониторинга маршрутов пользователя в рабочей среде. |


## <a name="next-steps"></a>Дальнейшие действия

- Изучите [операции Microsoft Graph, доступные для использования в Azure AD B2C](microsoft-graph-operations.md)
- Узнайте о [настраиваемых политиках и их отличиях от потоков пользователей](custom-policy-overview.md).
