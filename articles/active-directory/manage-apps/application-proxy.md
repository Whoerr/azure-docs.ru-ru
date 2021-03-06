---
title: Удаленный доступ к локальным приложениям — Azure AD Application Proxy
description: Azure Active Directory Application Proxy обеспечивает безопасный удаленный доступ к веб-приложениям, размещенным в локальной среде. Выполнив единый вход в AAD, пользователи получают доступ к облачным и локальным приложениям через внешний URL-адрес или внутренний портал приложений. Среди прочего, Application Proxy поддерживает удаленный доступ и единый вход для приложений "Удаленный рабочий стол", SharePoint, Teams, Tableau, Qlik и бизнес-приложений (LOB).
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 40472dc2446133ec23d1d62343a93d2d69fc90cd
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99260152"
---
# <a name="remote-access-to-on-premises-applications-through-azure-ad-application-proxy"></a>Удаленный доступ к локальным приложениям с помощью Azure AD Application Proxy

Azure Active Directory Application Proxy обеспечивает безопасный удаленный доступ к веб-приложениям, размещенным в локальной среде. Выполнив единый вход в AAD, пользователи получают доступ к облачным и локальным приложениям через внешний URL-адрес или внутренний портал приложений. Среди прочего, Application Proxy поддерживает удаленный доступ и единый вход для приложений "Удаленный рабочий стол", SharePoint, Teams, Tableau, Qlik и бизнес-приложений (LOB).

Характеристики Azure AD Application Proxy:

- **Простота в использовании**. Пользователи могут получить доступ к локальным приложениям так же, как к Microsoft 365 и другим приложениям SaaS, интегрированным с Azure AD. Вам не нужно изменять или обновлять приложения для работы с Application Proxy.

- **Безопасность**. Локальные приложения могут использовать элементы управления авторизацией и аналитику безопасности Azure. Например, локальные приложения могут использовать условный доступ и двухфакторную проверку подлинности. Application Proxy не требует открывать входящие подключения через брандмауэр.

- **Экономичность.** Для локальных решений вам, как правило, нужно устанавливать и поддерживать сети периметра, пограничные серверы или другие сложные инфраструктуры. Application Proxy выполняется в облаке, что упрощает работу с ней. Чтобы использовать Application Proxy, не нужно изменять сетевую инфраструктуру или устанавливать дополнительные устройства в локальной среде.

## <a name="what-is-application-proxy"></a>Что такое Application Proxy?
Azure Active Directory Application Proxy позволяет пользователям получать доступ с удаленного клиента к приложениям в локальной среде. Application Proxy включает в себя службу Application Proxy, которая работает в облаке, и соединитель Application Proxy, который выполняется на локальном сервере. Сочетание AAD, службы Application Proxy и соединителя Application Proxy позволяет безопасно передавать маркер входа из AAD в веб-приложение.

Application Proxy поддерживает:

* веб-приложения, использующие [встроенную проверку подлинности Windows](application-proxy-configure-single-sign-on-with-kcd.md);
* веб-приложения, использующие доступ на основе форм или [заголовков](./application-proxy-configure-single-sign-on-with-headers.md);
* веб-API, предоставляемые для полнофункциональных приложений на различных устройствах;
* Приложения, размещенные за [шлюзом удаленный рабочий стол](application-proxy-integrate-with-remote-desktop-services.md)
* полнофункциональные клиентские приложения, интегрированные с библиотекой проверки подлинности Майкрософт (MSAL).

Application Proxy поддерживает единый вход. Дополнительные сведения о поддерживаемых методах см. в статье [о выборе метода единого входа](sso-options.md#choosing-a-single-sign-on-method).

Прокси приложения рекомендуется использовать для предоставления удаленным пользователям доступа к внутренним ресурсам. Прокси приложения заменяет потребность в VPN или обратный прокси-сервер. Он не предназначен для внутренних пользователей в корпоративной сети.  Эти пользователи, которые необязательно используют прокси приложения, могут привести к непредвиденным и нежелательным проблемам с производительностью.

## <a name="how-application-proxy-works"></a>Схема работы Application Proxy

На следующей схеме показано, как AAD и Application Proxy работают в тандеме, организуя систему единого входа для локальных приложений.

![Схема работы прокси приложения Azure AD](./media/application-proxy/azureappproxxy.png)

1. Когда пользователь получит доступ к приложению через конечную точку, он будет перенаправлен на страницу входа в Azure AD.
2. После успешного входа AAD отправляет маркер на клиентское устройство пользователя.
3. Этот клиент отправляет маркер в службу Application Proxy, которая извлекает из него имя участника-пользователя (UPN) и имя субъекта безопасности (SPN). Затем Application Proxy направляет запрос к соединителю Application Proxy.
4. Если вы настроили единый вход, то соединитель выполняет требуемую дополнительную аутентификацию от имени пользователя.
5. Соединитель отправляет запрос локальному приложению.
6. Ответ отправляется пользователю через службу и соединитель Application Proxy.

> [!NOTE]
> Как и большинство гибридных агентов Azure AD, соединителю прокси приложения не требуется открывать входящие подключения через брандмауэр. Пользовательский трафик на шаге 3 завершается в службе прокси приложения (в Azure AD). Соединитель прокси приложения (локально) отвечает за остальную часть обмена данными.
>


| Компонент | Описание |
| --------- | ----------- |
| Конечная точка  | Конечной точкой называется URL-адрес или [пользовательский портал](end-user-experiences.md). Пользователи за пределами вашей сети могут получить доступ к приложениям, используя внешний URL-адрес. Пользователи в вашей сети могут получить доступ к приложениям, используя URL-адрес или пользовательский портал. При переходе к одной из этих конечных точек пользователи проходят аутентификацию в Azure AD и через соединитель направляются к локальному приложению.|
| Azure AD | Azure AD выполняет проверку подлинности с помощью каталога клиентом, хранящегося в облаке. |
| Служба Application Proxy | Служба Application Proxy работает в облаке как часть AAD. Она передает маркер единого входа от пользователя к соединителю Application Proxy. Application Proxy перенаправляет любой доступный заголовок запроса сохраняет IP-адрес клиента в новый заголовок в соответствии с используемым протоколом. Если нужный заголовок уже присутствует в запросе, поступившем на прокси-сервер, IP-адрес клиента добавляется в конец значения этого заголовка, которое представляет собой список с разделителями запятыми.|
| Соединитель Application Proxy | Соединителем называется упрощенный агент, который выполняется на сервере Windows Server в локальной сети. Соединитель управляет связью между службой Application Proxy в облаке и локальным приложением. Этот соединитель использует только исходящие подключения, а значит вам не нужно открывать входящие порты или помещать компоненты в сеть периметра. Соединители не имеют состояния и при необходимости извлекают сведения из облака. Дополнительные сведения о соединителях (например, как происходит их балансировка нагрузки и аутентификация) см. в статье [Сведения о соединителях прокси приложения Azure AD](application-proxy-connectors.md).|
| Active Directory | Active Directory работает локально и выполняет аутентификацию доменных учетных записей. Если настроен единый вход, соединитель взаимодействует со службой Active Directory для выполнения дополнительной аутентификации.
| Локальное приложение | Когда весь этот процесс завершится, пользователь получает доступ к локальному приложению.

## <a name="next-steps"></a>Дальнейшие действия
Чтобы приступить к использованию прокси приложения, см. раздел [учебник. Добавление локального приложения для удаленного доступа через прокси приложения](application-proxy-add-on-premises-application.md).