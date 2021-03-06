---
title: Общие сведения об использовании единого входа в локальном приложении с помощью прокси приложения
description: Общие сведения об использовании единого входа в локальном приложении с помощью прокси приложения.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: kenwith
ms.reviewer: japere, asteen
ms.openlocfilehash: 10b722edbe8d70c92e617c78db3d2fb1d46da3a5
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99254110"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Как настроить единый вход для приложения прокси приложения

Единый вход (SSO) позволяет пользователям обращаться к приложению несколько раз без повторной проверки подлинности. Он позволяет выполнить одну проверку подлинности для Azure Active Directory в облаке, после чего соединитель или служба будут выполнять дополнительные проверки подлинности для приложения от лица пользователя.

## <a name="how-to-configure-single-sign-on"></a>Как настроить единый вход
Чтобы настроить единый вход, убедитесь, что для приложения настроена предварительная проверка подлинности с использованием Azure Active Directory. Чтобы выполнить эту настройку, перейдите в раздел **Azure Active Directory**  - &gt; **корпоративные приложения**  - &gt; **все приложения**  - &gt; **- &gt; прокси приложения** приложения. На этой странице найдите параметр "Предварительная проверка подлинности" и убедитесь, что для него установлено значение "Azure Active Directory". 

Дополнительные сведения о методах предварительной проверки подлинности см. в шаге 4 в [документе, посвященном публикации приложения](application-proxy-add-on-premises-application.md).

   ![Метод предварительной проверки подлинности на портале Azure](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Настройка режимов единого входа для приложения прокси приложения
Настройте конкретный тип единого входа. Методы входа подразделяются в зависимости от того, какой тип проверки подлинности используется серверным приложением. Приложения прокси поддерживают три типа входа:

-   **Вход на основе пароля**: может использоваться для любого приложения, в котором для входа используются поля с именем пользователя и паролем. Инструкции по настройке см. в статье [Настройка единого входа по паролю для приложения из коллекции Azure AD](configure-password-single-sign-on-non-gallery-applications.md).

-   **Встроенная проверка подлинности Windows**: для приложений, использующих встроенную проверку подлинности Windows, единый вход выполняется с использованием ограниченного делегирования Kerberos. Этот метод дает соединителям прокси приложения в Active Directory возможность действовать от имени пользователей (в частности, отправлять и получать маркеры). Дополнительные сведения о настройке ограниченного делегирования Kerberos можно найти в [документации по настройке единого входа на основе ограниченного делегирования Kerberos](application-proxy-configure-single-sign-on-with-kcd.md).

-   **Вход на основе заголовка**. вход на основе заголовка используется для предоставления возможностей единого входа с помощью заголовков HTTP. Дополнительные сведения см. в разделе [единый вход на основе заголовков](application-proxy-configure-single-sign-on-with-headers.md).

-   **Единый вход SAML**. с помощью единого входа SAML Azure AD выполняет проверку подлинности в приложении с помощью учетной записи пользователя Azure AD. Azure AD передает приложению данные для входа через протокол соединения. Если используется единый вход на основе SAML, вы можете сопоставлять пользователей с конкретными ролями приложений согласно правилам, определенным в утверждениях SAML. Сведения о настройке единого входа SAML см. в разделе [SAML для единого входа с помощью прокси приложения](application-proxy-configure-single-sign-on-on-premises-apps.md).

Каждый из этих вариантов входа можно выбрать, открыв свое приложение в разделе "Корпоративные приложения" и выбрав страницу **Единый вход** в меню слева. Обратите внимание, что если приложение создано на старом портале, эти параметры могут не отображаться.

На этой странице отображается еще один дополнительный вариант единого входа: связанный единый вход. Этот вариант также поддерживается прокси приложения. Однако при выборе этого варианта единый вход для приложения не настраивается. Предположим, что для приложения уже настроен единый вход с использованием другой службы, например служб федерации Active Directory. 

Этот вариант позволяет создать ссылку на приложение, которое будет открываться для пользователей первым при обращении к вашему приложению. Например, если приложение настроено для проверки подлинности пользователей с помощью службы федерации Active Directory (AD FS) 2,0, администратор может использовать параметр "связанный вход" для создания ссылки на него в моих приложениях.

## <a name="next-steps"></a>Дальнейшие действия
- [Хранение паролей для единого входа с помощью прокси приложения](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Реализация единого входа в приложения с помощью прокси приложения](application-proxy-configure-single-sign-on-with-kcd.md)
- [Публикация приложений с поддержкой аутентификации на основе заголовков с использованием прокси приложения Azure AD и PingAccess](application-proxy-configure-single-sign-on-with-headers.md) 
- [SAML для единого входа с помощью прокси приложения](application-proxy-configure-single-sign-on-on-premises-apps.md).
