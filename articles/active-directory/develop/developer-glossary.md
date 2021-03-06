---
title: Глоссарий разработчика платформы удостоверений Майкрософт | Службы
description: Список терминов для часто используемых концепций и функций разработчика платформы идентификации Майкрософт.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jmprieur, saeeda, jesakowi, nacanuma
ms.openlocfilehash: 77a4e7389952b81df13dae929dc1aec664fcc0b7
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2021
ms.locfileid: "98755642"
---
# <a name="microsoft-identity-platform-developer-glossary"></a>Глоссарий разработчика платформы удостоверений Майкрософт

Эта статья содержит определения некоторых основных концепций и терминологии для разработчиков, которые полезны при изучении разработки приложений с помощью платформы Microsoft Identity.

## <a name="access-token"></a>Twitter,

Тип [маркера безопасности](#security-token), выдаваемого [сервером авторизации](#authorization-server), который используется в [клиентском приложении](#client-application) для доступа к [серверу защищенных ресурсов](#resource-server). Этот маркер (как правило, в виде [JSON Web Token (JWT)][JWT]) реализует право на авторизацию, которое [владелец ресурса](#resource-owner) предоставил клиенту, для запрошенного уровня доступа. Он содержит все применимые [утверждения](#claim) о субъекте, благодаря чему клиентское приложение может использовать его в качестве учетных данных при доступе к определенному ресурсу. Кроме того, владельцу ресурса не нужно предоставлять свои учетные данные клиенту.

Маркеры доступа допустимы только в течение короткого периода времени и не могут быть отменены. Сервер авторизации может также выдать [маркер обновления](#refresh-token) при выдаче маркера доступа. Маркеры обновления обычно предоставляются только конфиденциальным клиентским приложениям.

Маркеры доступа иногда называют "Пользователь + приложение" или "Только приложение" в зависимости от представляемых учетных данных. Ниже описано, при каком типе предоставления авторизации в клиентском приложении используется тот или иной маркер.

* [Предоставление авторизации "Код авторизации"](#authorization-grant)— сначала пользователь проходит аутентификацию как владелец ресурса, предоставляя клиенту права авторизации для доступа к ресурсу. Затем после получения маркера доступа проверку подлинности проходит клиент. Иногда этот маркер называют маркером "Пользователь + приложение", так как он представляет и пользователя, который авторизовал клиентское приложение, и само приложение.
* [Предоставление авторизации "Учетные данные клиента"](#authorization-grant)— проверку подлинности проходит только клиент, а проверка подлинности и авторизация владельца ресурса не выполняются, поэтому маркер иногда называют маркером "Только приложение".

Дополнительные сведения см. в [справочнике по маркерам платформы Microsoft Identity][AAD-Tokens-Claims] .

## <a name="application-id-client-id"></a>Идентификатор приложения (идентификатор клиента)

Уникальный идентификатор, который служба Azure AD присваивает зарегистрированному приложению. Идентификатор определяет конкретное приложение и связанные с ним конфигурации. Этот идентификатор приложения ([идентификатор клиента](https://tools.ietf.org/html/rfc6749#page-15)) используется при выполнении запросов проверки подлинности и предоставляется библиотекам проверки подлинности во время разработки. Идентификатор приложения (идентификатор клиента) не является секретом.

## <a name="application-manifest"></a>Манифест приложения

Возможность, предоставляемая на [портале Azure][AZURE-portal]. Он содержит представление JSON конфигурации удостоверения для приложения, которая используется как механизм обновления связанных сущностей [приложения][Graph-App-Resource] и [субъекта-службы][Graph-Sp-Resource]. Дополнительные сведения см. в статье [Основные сведения о манифесте приложения Azure Active Directory][AAD-App-Manifest].

## <a name="application-object"></a>Объект приложения

При регистрации или обновлении приложения на [портале Azure][AZURE-portal] создается или обновляется объект приложения и соответствующий [объект субъекта-службы](#service-principal-object) для этого клиента. Объект приложения глобально *определяет* конфигурацию удостоверения для приложения (для всех клиентов, где у него есть доступ) и предоставляет шаблон, *на основе* которого создаются соответствующие объекты субъектов-служб для локального использования во время выполнения (в конкретном клиенте).

Дополнительные сведения см. в статье [Объекты приложения и субъекта-службы в Azure Active Directory (Azure AD)][AAD-App-SP-Objects].

## <a name="application-registration"></a>Регистрация приложения

Чтобы разрешить приложению выполнять интеграцию с функциями управления удостоверениями и доступом и предоставлять права на их использование в Azure AD, необходимо зарегистрировать его в [клиенте](#tenant)Azure AD. При регистрации приложения в Azure AD вы предоставляете соответствующую конфигурацию удостоверения, что позволяет интегрировать приложение с Azure AD и использовать следующие возможности:

* Надежное управление единым входом за счет функции управления удостоверениями Azure AD и реализации протокола [OpenID Connect][OpenIDConnect].
* Доступ через брокер к [защищенным ресурсам](#resource-server) через [клиентские приложения](#client-application)через [сервер авторизации](#authorization-server) OAuth 2,0
* [Платформа согласия](#consent) для управления клиентским доступом к защищенным ресурсам на основе авторизации владельца ресурса.

Дополнительные сведения см. в статье [Интеграция приложений с Azure Active Directory][AAD-Integrating-Apps].

## <a name="authentication"></a>проверка подлинности

Запрос допустимых учетных данных у стороны, выполняющей вход. Это основа для создания субъекта безопасности, используемого для управления доступом и удостоверениями. К примеру, во время [предоставления авторизации OAuth2](#authorization-grant) сторона, выполняющая проверку подлинности, в зависимости от используемых разрешений выполняет роль [владельца ресурса](#resource-owner) или [клиентского приложения](#client-application).

## <a name="authorization"></a>авторизация

Предоставление разрешения на выполнение какого-либо действия субъекту безопасности, прошедшему проверку подлинности. Модель программирования Azure AD предусматривает два основных варианта:

* При [предоставлении авторизации OAuth2](#authorization-grant): [владелец ресурса](#resource-owner) предоставляет авторизацию для [клиентского приложения](#client-application), разрешая клиенту получать доступ к своим ресурсам.
* При доступе клиента к ресурсам: [сервер ресурсов](#resource-server) выполняет авторизацию, используя содержащиеся в [маркере доступа](#access-token) значения [утверждений](#claim), на основе которых можно принимать решения по контролю доступа.

## <a name="authorization-code"></a>Код авторизации

Кратковременный маркер, который [конечная точка авторизации](#authorization-endpoint) предоставляет для [клиентского приложения](#client-application) во время соответствующего потока. Это один из четырех типов [предоставления авторизации](#authorization-grant) OAuth2. Этот код возвращается в клиентское приложение в ответ на проверку подлинности [владельца ресурса](#resource-owner). В нем указано, что последний делегировал права на авторизацию для доступа к требуемым ресурсам. В рамках потока этот код позже меняется на [маркер доступа](#access-token).

## <a name="authorization-endpoint"></a>Конечная точка авторизации

Одна из конечных точек, реализуемых [сервером авторизации](#authorization-server). Она используется для взаимодействия с [владельцем ресурса](#resource-owner), чтобы [предоставить авторизацию](#authorization-grant) во время потока кода авторизации OAuth2. Фактическое предоставление, в том числе [код авторизации](#authorization-code) и [маркер безопасности](#security-token), может отличаться в зависимости от используемого потока предоставления авторизации.

Дополнительные сведения см. в разделах спецификации OAuth2 [по типам предоставления авторизации][OAuth2-AuthZ-Grant-Types] и [конечной точке авторизации][OAuth2-AuthZ-Endpoint], а также в [спецификации OpenIDConnect][OpenIDConnect-AuthZ-Endpoint].

## <a name="authorization-grant"></a>Предоставление авторизации

Учетные данные, представляющие [авторизацию](#authorization) [владельца ресурса](#resource-owner) для доступа к защищенным ресурсам, предоставленным [клиентскому приложению](#client-application). Клиентское приложение может использовать один из [четырех типов предоставления, определенных OAuth2 Authorization Framework][OAuth2-AuthZ-Grant-Types], в зависимости от требований или типа клиента: "предоставление кода авторизации", "предоставление учетных данных клиента", "неявное предоставление" и "предоставление учетных данных владельца ресурса". Клиенту возвращается [маркер доступа](#access-token) или [код авторизации](#authorization-code) (который позже меняется на маркер доступа) в зависимости от используемого типа предоставления авторизации.

## <a name="authorization-server"></a>сервера авторизации

Как определено в [OAuth2 Authorization Framework][OAuth2-Role-Def], это сервер, ответственный за выдачу маркеров доступа для [клиента](#client-application) после успешной проверки подлинности [владельца ресурса](#resource-owner) и получения его авторизации. [Клиентское приложение](#client-application) взаимодействует с сервером авторизации во время выполнения с использованием конечных точек [авторизации](#authorization-endpoint) и [маркеров](#token-endpoint) в соответствии с [предоставлениями авторизации](#authorization-grant), определенными OAuth2.

В случае интеграции приложений платформы идентификации Майкрософт платформа Microsoft Identity реализует роль сервера авторизации для приложений Azure AD и API службы Майкрософт, например [Microsoft Graph API][Microsoft-Graph].

## <a name="claim"></a>Утверждение

[Маркер безопасности](#security-token) предоставляет утверждения об одной сущности (например, о [клиентском приложении](#client-application) или [владельце ресурса](#resource-owner)) другой сущности (например, [серверу ресурсов](#resource-server)). Утверждения — это пары "имя — значение", которые ретранслируют факты о субъекте маркера (например, субъекте безопасности, прошедшем проверку подлинности с помощью [сервера авторизации](#authorization-server)). Утверждения, имеющиеся в заданном маркере, зависят от нескольких переменных, в том числе от типа маркера, типа учетных данных, используемых для проверки подлинности субъекта, конфигурации приложения и т. п.

Дополнительные сведения см. в [справочнике по маркерам платформы Microsoft Identity][AAD-Tokens-Claims] .

## <a name="client-application"></a>клиентское приложение

Как определено в [OAuth2 Authorization Framework][OAuth2-Role-Def], это приложение, которое выполняет запросы к защищенному ресурсу от имени [владельца ресурса](#resource-owner). Термин "клиент" не подразумевает какие-либо конкретные характеристики реализации оборудования (например, место выполнения приложения: на сервере, настольном компьютере или на других устройствах).

Клиентское приложение запрашивает [авторизацию](#authorization) от владельца ресурса для участия в потоке [предоставления авторизации OAuth2](#authorization-grant) и может получать доступ к API и данным от имени владельца ресурса. OAuth2 Authorization Framework [определяет два типа клиентов][OAuth2-Client-Types], "конфиденциальные" и "открытые", в зависимости от их возможности сохранять конфиденциальность учетных данных. Приложения могут реализовать [веб-клиент (конфиденциальный)](#web-client), выполняемый на веб-сервере, [клиент Native Client (открытый)](#native-client), установленный на устройстве, и [клиент на основе агента пользователя (открытый)](#user-agent-based-client), выполняемый в браузере устройства.

## <a name="consent"></a>Согласие

Процесс предоставления [владельцем ресурса](#resource-owner)[клиентскому приложению](#client-application) доступа к ресурсам, защищенным конкретными [разрешениями](#permissions), от своего имени. В зависимости от разрешений, запрошенных клиентом, администратору или пользователю будет отправлен запрос на разрешение доступа к рабочим или личным данным. Обратите внимание, в сценарии с [несколькими клиентами](#multi-tenant-application)[субъект-служба](#service-principal-object) приложения также записывается в клиенте пользователя, дающего согласие.

См. дополнительные сведения о [платформе предоставления согласия](consent-framework.md).

## <a name="id-token"></a>Маркер идентификации

[Токен безопасности](#security-token) [OpenID Connect Connect][OpenIDConnect-ID-Token] , предоставляемый [конечной точкой авторизации](#authorization-endpoint) [сервера авторизации](#authorization-server) , который содержит [утверждения](#claim) , относящиеся к проверке подлинности [владельца ресурса](#resource-owner)конечного пользователя. Подобно маркеру доступа, маркеры идентификации также представлены в виде маркеров [JSON Web Token (JWT)][JWT] с цифровой подписью. Тем не менее в отличие от маркера доступа утверждения маркера идентификации не используются для целей, связанных с доступом к ресурсам и контролем доступа.

Дополнительные сведения см. в [справочнике по маркерам платформы Microsoft Identity][AAD-Tokens-Claims] .

## <a name="microsoft-identity-platform"></a>Платформа удостоверений Майкрософт

Платформа Microsoft Identity — это эволюция службы идентификации Azure Active Directory (Azure AD) и платформы разработки. С ее помощью разработчики могут создавать приложения, которые обеспечивают вход с помощью любых удостоверений Майкрософт, получают маркеры для вызова Microsoft Graph, других программных интерфейсов Майкрософт или API, созданных разработчиками. Это полнофункциональная платформа, включающая в себя службу аутентификации, библиотеки, службу регистрации приложений и конфигурацию, полную документацию по разработке, примеры кода и другое содержимое, предназначенное для разработчиков. Платформа удостоверений Майкрософт поддерживает стандартные отраслевые протоколы, такие как OAuth 2.0 и OpenID Connect.

## <a name="multi-tenant-application"></a>Мультитенантное приложение

Класс клиентского приложения, который обеспечивает вход и [согласие](#consent) пользователей, подготовленных в любом [клиенте](#tenant) Azure AD, включая клиенты, отличные от того, в котором зарегистрирован клиентский компьютер. Приложения [собственного клиента](#native-client) являются мультитенантными по умолчанию, тогда как приложения [веб-клиента](#web-client) и [веб-ресурса или веб-API](#resource-server) предусматривают возможность выбора отдельного клиента или нескольких клиентов. Веб-приложение, зарегистрированное как одноклиентское, напротив, предоставляет разрешение на вход только из учетных записей пользователей, подготовленных в том же клиенте, где было зарегистрировано приложение.

Дополнительные сведения см. в статье [Как реализовать вход любого пользователя Azure Active Directory (AD) с помощью шаблона мультитенантного приложения][AAD-Multi-Tenant-Overview].

## <a name="native-client"></a>Собственный клиент

Тип [клиентского приложения](#client-application), которое изначально установлено на устройстве. Так как весь код выполняется на устройстве, этот клиент считается общедоступным из-за неспособности хранить учетные данные в частном порядке, т. е. конфиденциально. Дополнительные сведения см. в разделе о [типах и профилях клиента OAuth2][OAuth2-Client-Types].

## <a name="permissions"></a>разрешения

[Клиентское приложение](#client-application) получает доступ к [серверу ресурсов](#resource-server) путем объявления запросов на разрешения. Доступно два типа таких разрешений:

* Делегированные разрешения, которые определяют доступ [на основе области](#scopes) в рамках предоставленной авторизации от выполнившего вход [владельца ресурса](#resource-owner), предъявляются ресурсу во время выполнения в виде [SCP-утверждений](#claim) в [маркере доступа](#access-token) клиента.
* Разрешения приложения, которые определяют доступ [на основе ролей](#roles), используя удостоверение или учетные данные клиентского приложения, предъявляются ресурсу во время выполнения в виде [утверждений ролей](#claim) в маркере доступа клиента.

В процессе получения [согласия](#consent) можно использовать оба типа разрешений. Они предоставляют администратору или владельцу ресурса возможность разрешить или запретить клиентскому приложению доступ к ресурсам в их клиенте.

Запросы на разрешения настраиваются на странице **разрешений API** для приложения в [портал Azure][AZURE-portal], выбирая необходимые "делегированные разрешения" и "разрешения приложения" (в последнем случае требуется членство в роли глобального администратора). Так как [общедоступный клиент](#client-application) не может обеспечить безопасное хранение учетных данных, он может запросить только делегированные разрешения, в то время как [конфиденциальный клиент](#client-application) может запросить и делегированные разрешения, и разрешения приложения. [Объект приложения](#application-object) клиента сохраняет объявленные разрешения в [свойстве requiredResourceAccess][Graph-App-Resource].

## <a name="refresh-token"></a>маркер обновления

Тип [маркера безопасности](#security-token) , выданного [сервером авторизации](#authorization-server)и используемый [клиентским приложением](#client-application) для запроса нового [маркера доступа](#access-token) до истечения срока действия маркера доступа. Обычно в виде [JSON Web Token (JWT)][JWT].

В отличие от маркеров доступа маркеры обновления могут быть отозваны. Если клиентское приложение пытается запросить новый маркер доступа с помощью отмененного маркера обновления, сервер авторизации отклонит запрос, а клиентское приложение больше не будет иметь разрешения на доступ к [серверу ресурсов](#resource-server) от имени [владельца ресурса](#resource-owner).

## <a name="resource-owner"></a>владелец ресурса

Как определено в [OAuth2 Authorization Framework][OAuth2-Role-Def], это сущность, которая может предоставить доступ к защищенному ресурсу. Если владелец ресурса — человек, он называется пользователем. Например, если [клиентскому приложению](#client-application) требуется получить доступ к почтовому ящику пользователя через [API Microsoft Graph][Microsoft-Graph], ему необходимо получить разрешение у владельца ресурса почтового ящика.

## <a name="resource-server"></a>серверу ресурсов

Как определено в [OAuth2 Authorization Framework][OAuth2-Role-Def], это сервер, на котором размещены защищенные ресурсы и который может принимать запросы на ресурсы, поступающие из [клиентских приложений](#client-application), которые предоставляют [маркер доступа](#access-token), и отвечать на них. Он также называется сервером защищенных ресурсов или приложением ресурсов.

Сервер ресурсов предоставляет API-интерфейсы и обеспечивает доступ к размещенным на нем защищенным ресурсам на основе [областей](#scopes) и [ролей](#roles), используя OAuth 2.0 Authorization Framework. Примеры включают [Microsoft Graph API][Microsoft-Graph] , который обеспечивает доступ к данным клиента Azure AD, а также интерфейсы API Microsoft 365, которые предоставляют доступ к данным, таким как mail и Calendar.

Как и в клиентском приложении, конфигурация удостоверения приложения ресурсов устанавливается путем [регистрации](#application-registration) в клиенте Azure AD, при этом предоставляется и объект приложения, и объект субъекта-службы. Некоторые API, предоставляемые корпорацией Майкрософт, такие как Microsoft Graph API, имеют предварительно зарегистрированные субъекты-службы, доступные во всех клиентах во время подготовки.

## <a name="roles"></a>Роли

Как и [области](#scopes), роли предоставляют способ контроля доступа к защищенным ресурсам для [сервера ресурсов](#resource-server). Существует два типа ролей: роль "пользователь" реализует управление доступом на основе ролей для пользователей или групп, которым требуется доступ к ресурсу, а роль "приложение" реализует то же самое для [клиентских приложений](#client-application) , которым нужен доступ.

Роли — это определяемые ресурсом строки (например, "Утверждающий затраты", "Только чтение" или Directory.ReadWrite.All), которыми можно управлять на [портале Azure][AZURE-portal], используя [манифест приложения](#application-manifest) ресурса. Эти строки хранятся в [свойстве appRoles][Graph-Sp-Resource] ресурса. С помощью портала Azure можно также назначать пользователям роль "пользователь" и настраивать [разрешения приложений](#permissions) клиента для доступа к роли "приложение".

Подробное описание ролей приложений, предоставляемых Microsoft Graph API, см. в разделе [API Graph области разрешений][Graph-Perm-Scopes]. Пример пошаговой реализации см. [в разделе Добавление или удаление назначений ролей Azure с помощью портал Azure][AAD-RBAC].

## <a name="scopes"></a>Области

Как и [роли](#roles), области позволяют [серверу ресурсов](#resource-server) контролировать доступ к своим защищенным ресурсам. Области используются для реализацииконтроля доступа [на основе областей][OAuth2-Access-Token-Scopes] для [клиентского приложения](#client-application), которому предоставлен делегированный доступ к ресурсу его владельцем.

Области — это определяемые ресурсом строки (например, Mail.Read, Directory.ReadWrite.All), которыми можно управлять на [портале Azure][AZURE-portal], используя [манифест приложения](#application-manifest) ресурса. Эти строки хранятся в [свойстве oauth2Permissions][Graph-Sp-Resource] ресурса. Портал Azure также используется для настройки [делегированных разрешений](#permissions) клиентского приложения для доступа к области.

Рекомендуется использовать такой формат именования: resource.operation.constraint. Подробное описание областей, предоставляемых Microsoft Graph API, см. в разделе [API Graph области разрешений][Graph-Perm-Scopes]. Сведения об областях, предоставляемых службами Microsoft 365, см. в разделе [Справочник по разрешениям Microsoft 365 API][O365-Perm-Ref].

## <a name="security-token"></a>Маркер безопасности

Подписанный документ, содержащий утверждения, такие как маркер OAuth2 или утверждение SAML 2.0. Для [предоставления авторизации](#authorization-grant)OAuth2 [маркер доступа](#access-token) (OAuth2), [маркер обновления](#refresh-token)и [маркер идентификации](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) являются типами маркеров безопасности, которые реализуются как [JSON Web Token (JWT)][JWT].

## <a name="service-principal-object"></a>объект субъекта-службы

При регистрации или обновлении приложения в [портал Azure][AZURE-portal]портал создает или обновляет [объект приложения](#application-object) и соответствующий объект субъекта-службы для этого клиента. Объект приложения глобально *определяет* конфигурацию удостоверения для приложения (для всех клиентов, где связанному приложению предоставлен доступ) и шаблон, *на основе* которого создаются соответствующие объекты субъектов-служб для локального использования во время выполнения (в конкретном клиенте).

Дополнительные сведения см. в статье [Объекты приложения и субъекта-службы в Azure Active Directory (Azure AD)][AAD-App-SP-Objects].

## <a name="sign-in"></a>Вход

Процесс, в ходе которого [клиентское приложение](#client-application) инициирует проверку подлинности пользователя и фиксирует соответствующее состояние, чтобы получить [маркер безопасности](#security-token) и использовать это состояние для сеанса приложения. Состояние может включать в себя артефакты, такие как сведения о профиле пользователя и информация, извлеченная из утверждений маркера.

Функция входа в приложение обычно используется для реализации единого входа (SSO). Ей может также предшествовать функция регистрации как точка входа для пользователя, с помощью которой предоставляется доступ к приложению (после первого входа в систему). Функция регистрации используется для сбора и сохранения дополнительного состояния конкретного пользователя и может требовать [согласия пользователя](#consent).

## <a name="sign-out"></a>Выход

Отмена аутентификации пользователя, при которой отключается отслеживание состояния пользователя, связанное с сеансом [клиентского приложения](#client-application) при [входе](#sign-in).

## <a name="tenant"></a>tenant

Экземпляр каталога Azure AD, который называют клиентом Azure AD. Он обеспечивает несколько функций, включая следующие:

* служба реестра интегрированных приложений;
* аутентификация учетных записей пользователей и зарегистрированных приложений;
* конечные точки REST, необходимые для поддержки различных протоколов, включая OAuth2 и SAML, в том числе [конечная точка авторизации](#authorization-endpoint), [конечная точка маркера](#token-endpoint) и "общие" конечные точки, используемые [мультитенантными приложениями](#multi-tenant-application).

Клиенты Azure AD создаются или связываются с подписками Azure и подпиской Microsoft 365 во время регистрации, предоставляя функции управления доступом & удостоверений для подписки. Администраторы подписок Azure могут также создать дополнительные клиенты Azure AD с помощью портала Azure. Подробные сведения о различных способах получения доступа к клиенту см. в статье [Как получить клиент Azure Active Directory][AAD-How-To-Tenant]. Сведения о связи между подписками и клиентом Azure AD, а также инструкции по связыванию или добавлению подписки в клиент Azure AD см. в статье [связывание или Добавление подписки Azure в клиент Azure Active Directory][AAD-How-Subscriptions-Assoc] .

## <a name="token-endpoint"></a>Конечная точка маркера

Одна из конечных точек, реализуемых [сервером авторизации](#authorization-server) для поддержки [предоставлений авторизации](#authorization-grant) OAuth2. В зависимости от разрешений она может использоваться для получения [маркера доступа](#access-token) (и связанного маркера обновления) для [клиента](#client-application) или [маркера идентификации](#id-token) при использовании с протоколом [OpenID Connect][OpenIDConnect].

## <a name="user-agent-based-client"></a>Клиент на основе агента пользователя

Тип [клиентского приложения](#client-application), которое скачивает код с веб-сервера и выполняется в агенте пользователя (к примеру, в браузере), например одностраничное приложение (SPA). Так как весь код выполняется на устройстве, этот клиент считается общедоступным из-за неспособности хранить учетные данные в частном порядке, т. е. конфиденциально. Дополнительные сведения см. в разделе о [типах и профилях клиента OAuth2][OAuth2-Client-Types].

## <a name="user-principal"></a>Субъект-пользователь

Аналогично объекту субъекта-службы, который представляет экземпляр приложения, объект субъекта-пользователя является другим типом субъекта безопасности, который представляет пользователя. Тип пользовательского [ресурса][Graph-User-Resource] Microsoft Graph определяет схему для объекта пользователя, включая свойства, связанные с пользователем, такие как имя и фамилия, имя участника-пользователя, членство в роли каталога и т. д. Это обеспечивает настройку удостоверения пользователя в Azure AD для установки субъекта-пользователя во время выполнения. Субъект-пользователь представляет прошедшего проверку подлинности пользователя при едином входе, записи делегирования права [согласия](#consent), принятии решений по контролю доступа и т. п.

## <a name="web-client"></a>веб-клиента

Тип [клиентского приложения](#client-application), которое выполняет весь код на веб-сервере и может выступать в роли "конфиденциального" клиента, обеспечивая безопасное хранение учетных данных на сервере. Дополнительные сведения см. в разделе о [типах и профилях клиента OAuth2][OAuth2-Client-Types].

## <a name="next-steps"></a>Следующие шаги

[Руководства разработчика платформы удостоверений Майкрософт][AAD-Dev-Guide] — это Целевая страница, которая будет использоваться для всех разделов, связанных с разработками платформы идентификации Майкрософт, включая обзор [интеграции приложений][AAD-How-To-Integrate] и основы [проверки подлинности платформы идентификации Майкрософт и поддерживаемых сценариев проверки подлинности][AAD-Auth-Scenarios]. Руководства и примеры кода для быстрого начала работы можно также найти на сайте [GitHub](https://github.com/azure-samples?utf8=%E2%9C%93&q=active%20directory&type=&language=).

В следующем разделе комментариев вы можете поделиться своим мнением и помочь улучшить и сформировать материалы. Также вы можете запросить создание новых определений или обновление существующих.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]:reference-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[Graph-Perm-Scopes]: /graph/permissions-reference
[Graph-App-Resource]: /graph/api/resources/application
[Graph-Sp-Resource]: /graph/api/resources/serviceprincipal?view=graph-rest-beta&preserve-view=true
[Graph-User-Resource]: /graph/api/resources/user
[AAD-How-Subscriptions-Assoc]:../fundamentals/active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]:quickstart-create-new-tenant.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Multi-Tenant-Overview]:howto-convert-app-to-be-multi-tenant.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]:access-tokens.md
[AZURE-portal]: https://portal.azure.com
[AAD-RBAC]: ../../role-based-access-control/role-assignments-portal.md
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://developer.microsoft.com/graph
[O365-Perm-Ref]: /graph/permissions-reference
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: https://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: https://openid.net/specs/openid-connect-core-1_0.html#IDToken