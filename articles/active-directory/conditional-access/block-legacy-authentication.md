---
title: Блокировка устаревших методов аутентификации в Azure Active Directory
description: Узнайте, как улучшить безопасность, заблокировав устаревшие методы аутентификации с помощью условного доступа в Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 01/26/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 09f98e3d6c7997d9cae2737b25f4323021e29bfb
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/27/2021
ms.locfileid: "98892445"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Руководство. Блокировка устаревших методов аутентификации в Azure AD с помощью условного доступа   

Чтобы предоставить пользователям удобный доступ к облачным приложениям, Azure Active Directory (AAD) поддерживает широкий набор протоколов проверки подлинности, часть из которых уже устарела. Проблема заключается в том, что устаревшие протоколы не поддерживают многофакторную проверку подлинности (MFA). Во многих средах MFA является стандартным методом защиты от кражи идентификаторов. 

Алекс Вайнерт (Alex Weinert), директор по безопасности удостоверений в корпорации Майкрософт, в записи блога от 12 марта 2020 года про [новые инструменты для блокировки устаревших методов аутентификации в организации](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/new-tools-to-block-legacy-authentication-in-your-organization/ba-p/1225302#) рассказывает о том, почему организациям следует блокировать такие методы и какие дополнительные инструменты предоставляет для этого корпорация Майкрософт:

> Чтобы обеспечить эффективность многофакторной проверки подлинности, необходимо также заблокировать устаревшие методы аутентификации. Это связано с тем, что устаревшие протоколы аутентификации, такие как POP, SMTP, IMAP и MAPI, не могут применять многофакторную проверку подлинности, что делает их предпочтительными направлениями атаки злоумышленников на вашу организацию.
> 
>...Результаты для устаревших методов аутентификации, полученные на основе анализа трафика Azure Active Directory (Azure AD), очень показательны:
> 
> - Более 99 % атак путем распыления пароля используют устаревшие методы аутентификации.
> - Более 97 % атак с подстановкой учетных данных используют устаревшие методы аутентификации.
> - Учетные записи Azure AD в организациях, в которых устаревшие методы аутентификации отключены, взламывают на 67 % реже, чем те, в которых эти методы включены.
>

Если вы готовы к тому, чтобы запретить в своей среде устаревшие методы аутентификации для повышения защищенности клиента, это можно выполнить с помощью условного доступа. В этой статье объясняется, как настроить политики условного доступа для блокирования устаревших методов аутентификации в клиенте.

## <a name="prerequisites"></a>Предварительные требования

В этой статье предполагается, что вы знакомы с [основными понятиями](overview.md) условного доступа Azure AD.

## <a name="scenario-description"></a>Описание сценария

AAD поддерживает несколько широко распространенные протоколы проверки подлинности и авторизации, часть из которых уже устарела. Устаревшими считаются все протоколы, которые используют обычную проверку подлинности. Эти протоколы обычно не поддерживают какие-либо методы двухфакторной идентификации. Вот несколько примеров приложений, которые используют устаревшую проверку подлинности:

- устаревшие приложения Microsoft Office;
- любые приложения, поддерживающие почтовые протоколы POP, IMAP и SMTP.

В наше время одного фактора аутентификации (например, имени пользователя и пароля) уже недостаточно. Пароли работают плохо, так как их можно легко угадать, и люди не умеют выбирать правильные пароли. Пароли очень уязвимы к нескольким видам атак, таких как фишинг и подбор. Одной из самых простых вещей, которые можно сделать для защиты от угроз паролей, является реализация многофакторной проверки подлинности (MFA). При наличии многофакторной проверки подлинности злоумышленник не сможет пройти аутентификацию и получить доступ к данным, даже если получит пароль пользователя в свое распоряжение.

Как же запретить доступ к ресурсам клиента всем приложениям, которые используют устаревшую проверку подлинности? Мы рекомендуем заблокировать их при помощи политики условного доступа. Если потребуется, вы сможете разрешить отдельным пользователям использовать конкретные приложения с устаревшей проверкой подлинности из конкретных сетевых расположений.

Политики условного доступа применяются после того, как пользователь прошел однофакторную аутентификацию. Поэтому условный доступ не может предотвратить атаки типа "отказ в обслуживании" (DoS). Но этот механизм может использовать сигналы, поступающие от этих событий (например, уровень риска при входе, расположение запроса и т. д.), для определения доступа.

## <a name="implementation"></a>Реализация

В этом разделе объясняется, как настроить политику условного доступа для блокировки устаревших методов аутентификации. 

### <a name="legacy-authentication-protocols"></a>Устаревшие протоколы аутентификации

Следующие протоколы аутентификации считаются устаревшими.

- SMTP, прошедший проверку подлинности, используется клиентами POP и IMAP для отправки сообщений электронной почты.
- Автообнаружение — используется клиентами Outlook и EAS для поиска почтовых ящиков в Exchange Online и подключения к ним.
- Exchange ActiveSync (EAS) — используется для подключения к почтовым ящикам в Exchange Online.
- PowerShell в Exchange Online — используется для подключения к Exchange Online с помощью удаленной оболочки PowerShell. Если вы заблокируете обычную аутентификацию для PowerShell в Exchange Online, для подключения понадобится использовать модуль PowerShell Exchange Online. Инструкции см. в статье [Подключение к PowerShell Exchange Online с помощью многофакторной проверки подлинности](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).
- Веб-службы Exchange (EWS) — программный интерфейс, используемый Outlook, Outlook для Mac и сторонними приложениями.
- IMAP4 — используется клиентами электронной почты IMAP.
- MAPI через HTTP (MAPI/HTTP) — используется в Outlook 2010 и более поздних версиях.
- Автономная адресная книга (OAB) — копия коллекций списков адресов, которые скачиваются и используются в Outlook.
- Мобильный Outlook (протокол RPC через HTTP) — используется в Outlook 2016 и более ранних версиях.
- Служба Outlook — используется приложениями "Почта" и "Календарь" для Windows 10.
- POP3 — используется клиентами электронной почты POP.
- Веб-службы отчетов — используются для получения данных отчетов в Exchange Online.
- Другие клиенты — другие протоколы, которые определены как использующие устаревший механизм аутентификации.

Дополнительные сведения об этих протоколах и службах аутентификации см. в статье [Отчеты о действиях входа на портале Azure Active Directory](../reports-monitoring/concept-sign-ins.md#filter-sign-in-activities).

### <a name="identify-legacy-authentication-use"></a>Выявление использования устаревших методов аутентификации

Прежде чем блокировать устаревшие методы аутентификации в каталоге, сначала определите, есть ли у пользователей приложения, использующие такие методы, и как они повлияют на каталог в целом. Определить, используете ли вы устаревшие методы аутентификации, можно с помощью журналов входа в Azure AD.

1. Выберите **портал Azure** > **Azure Active Directory** > **Sign-ins** (События входа).
1. Если столбец Client App (Клиентское приложение) не отображается, щелкните **Columns (Столбцы)**  > **Client App (Клиентское приложение)** .
1. **Добавление фильтров**  >  > **клиентского приложения** выберите все устаревшие протоколы проверки подлинности. Выберите вне диалогового окна Фильтрация, чтобы применить выбранные параметры и закрыть диалоговое окно.
1. Если вы активировали [новую предварительную версию отчетов о действиях входа](../reports-monitoring/concept-all-sign-ins.md), повторите описанные выше действия на вкладке **Вход пользователей (неинтерактивные))** .

После применения фильтра будут показаны только попытки входа с использованием устаревших протоколов аутентификации. При выборе каждой отдельной попытки входа будут показаны дополнительные сведения. В поле **Client App** (Клиентское приложение) на вкладке **Basic Info** (Основные сведения) указывается, какой протокол аутентификации использовался ранее.

Эти журналы содержат сведения о том, какие пользователи все еще используют устаревшие методы аутентификации и какие приложения используют устаревшие протоколы для выполнения запросов аутентификации. Для пользователей, которые не отображаются в этих журналах и точно не используют устаревшие методы аутентификации, можно реализовать политику условного доступа.

## <a name="block-legacy-authentication"></a>Блокировка устаревших методов проверки подлинности 

Существует два способа использования политик условного доступа для блокирования устаревшей проверки подлинности.

- [Непосредственная блокировка устаревшей проверки подлинности](#directly-blocking-legacy-authentication)
- [Косвенная блокировка устаревшей проверки подлинности](#indirectly-blocking-legacy-authentication)
 
### <a name="directly-blocking-legacy-authentication"></a>Непосредственная блокировка устаревшей проверки подлинности

Самый простой способ заблокировать устаревшую проверку подлинности во всей организации — настроить политику условного доступа, которая применяется специально для устаревших клиентов проверки подлинности и блокирует доступ. При назначении пользователям и приложениям политики необходимо исключить пользователей и учетные записи служб, которые по-прежнему должны входить в систему, используя устаревшую проверку подлинности. Настройте условия клиентских приложений, выбрав **Клиенты Exchange ActiveSync** и **другие клиенты**. Чтобы заблокировать доступ к этим клиентским приложениям, настройте элементы управления доступом для блокировки доступа.

![Условие для клиентских приложений, настроенное для блокировки проверки подлинности прежних версий](./media/block-legacy-authentication/client-apps-condition-configured-yes.png)

### <a name="indirectly-blocking-legacy-authentication"></a>Косвенная блокировка устаревшей проверки подлинности

Даже если ваша организация не готова блокировать проверку подлинности прежних версий во всей Организации, следует убедиться, что входы с использованием устаревшей проверки подлинности не обходят политики, требующие таких элементов управления, как требование многофакторной проверки подлинности или соответствующих или гибридных устройств, присоединенных к Azure AD. Во время проверки подлинности клиенты устаревшей проверки подлинности не поддерживают отправку MFA, соответствие устройства или сведения о состоянии подключения в Azure AD. Таким образом, примените политики с предоставлением элементов управления ко всем клиентским приложениям, чтобы не заблокировались устаревшие операции входа на основе проверки подлинности, которые не соответствуют этим элементам управления. В общей доступности условия клиентских приложений в августе 2020, вновь созданные политики условного доступа применяются ко всем клиентским приложениям по умолчанию.

![Конфигурация условий по умолчанию для клиентских приложений](./media/block-legacy-authentication/client-apps-condition-configured-no.png)

## <a name="what-you-should-know"></a>Необходимая информация

Блокировка доступа с помощью условия **Другие клиенты** также блокирует PowerShell в Exchange Online и Dynamics 365, использующие обычную аутентификацию.

Если задать для политики параметр **Другие клиенты**, доступ из определенных клиентов, например SPConnect, заблокируется для всей организации. Эта блокировка выполняется, так как клиенты предыдущих версий выполняют аутентификацию непредвиденным образом. Эта проблема не касается основных приложений Office, таких как клиенты Office прежних версий.

Чтобы политика вступила в силу, может потребоваться до 24 часов.

Вы можете настроить все доступные элементы управления для условия **Другие клиенты**, но для пользователей по сути ничего не изменится — доступ будет заблокирован.

### <a name="sharepoint-online-and-b2b-guest-users"></a>Гостевые пользователи SharePoint Online и B2B

Для блокировки доступа пользователей B2B с помощью устаревшей проверки подлинности в SharePoint Online Организации должны отключить устаревшую проверку подлинности в SharePoint с помощью `Set-SPOTenant` команды PowerShell и задать `-LegacyAuthProtocolsEnabled` для параметра значение `$false` . Дополнительные сведения о задании этого параметра можно найти в справочном документе по SharePoint PowerShell, посвященном командлету [Set-SPOTenant](/powershell/module/sharepoint-online/set-spotenant) .

## <a name="next-steps"></a>Дальнейшие действия

- [Определение влияния с помощью условного доступа в режиме "только отчет"](howto-conditional-access-insights-reporting.md)
- Если вы плохо знакомы с настройкой политик условного доступа, ознакомьтесь со статьей [Руководство. Защита событий входа с помощью Многофакторной идентификации Azure](../authentication/tutorial-enable-azure-mfa.md).
- Дополнительные сведения о современной аутентификации для клиентских приложений Office 2013 и Office 2016 см. в [этой статье](/office365/enterprise/modern-auth-for-office-2013-and-2016). 
- [Как настроить многофункциональное устройство или приложение для отправки электронной почты с помощью Microsoft 365](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365)