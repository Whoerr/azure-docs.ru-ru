---
title: Блокирование устаревших протоколов проверки подлинности в Azure AD
description: Узнайте, как и зачем организациям блокировать устаревшие протоколы проверки подлинности
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 01/26/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 24640254f32270b8c96c790dca7db31e285cc27f
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/27/2021
ms.locfileid: "98895294"
---
# <a name="blocking-legacy-authentication"></a>Блокировка устаревших методов проверки подлинности
 
Чтобы предоставить пользователям удобный доступ к облачным приложениям, Azure Active Directory (AAD) поддерживает широкий набор протоколов проверки подлинности, часть из которых уже устарела. Устаревшие методы проверки подлинности включают следующие запросы:

- Более старые клиенты Office, не использующие современную проверку подлинности (например, Office 2010 Client)
- Любой клиент, использующий устаревшие почтовые протоколы, такие как IMAP/SMTP/POP3

В настоящее время большинство возможных попыток входа в систему поступают от устаревшей проверки подлинности. Устаревшая проверка подлинности не поддерживает многофакторную проверку подлинности (MFA). Даже если в вашем каталоге включена политика MFA, Недопустимый субъект может пройти проверку подлинности с помощью устаревшего протокола и обойти MFA. Лучший способ защитить учетную запись от вредоносных запросов проверки подлинности, сделанных устаревшими протоколами, заключается в том, что эти попытки полностью блокируются.

## <a name="identify-legacy-authentication-use"></a>Выявление использования устаревших методов аутентификации

Прежде чем блокировать устаревшие методы аутентификации в каталоге, сначала определите, есть ли у пользователей приложения, использующие такие методы, и как они повлияют на каталог в целом. Определить, используете ли вы устаревшие методы аутентификации, можно с помощью журналов входа в Azure AD.

1. Выберите **портал Azure** > **Azure Active Directory** > **Sign-ins** (События входа).
1. Добавьте столбец **клиентского приложения** , если он не отображается, щелкнув элемент  **столбцы**   >  **клиентское приложение**.
1. Фильтровать по **клиентскому приложению** > проверять все **устаревшие параметры клиентов проверки подлинности** .
1. Фильтр по **состоянию**  >  **успешно завершен**. 
1. При необходимости разверните диапазон дат, используя фильтр **дат** .
1. Если вы активировали [новую предварительную версию отчетов о действиях входа](../reports-monitoring/concept-all-sign-ins.md), повторите описанные выше действия на вкладке **Вход пользователей (неинтерактивные))** .

При фильтрации будут показаны только успешные попытки входа, выполненные выбранными старыми протоколами проверки подлинности. При выборе каждой отдельной попытки входа будут показаны дополнительные сведения. В столбце клиентское приложение или в поле клиентское приложение на вкладке Основные сведения после выбора отдельной строки данных будет указано, какой протокол проверки подлинности использовался ранее. Эти журналы содержат сведения о том, какие пользователи все еще используют устаревшие методы аутентификации и какие приложения используют устаревшие протоколы для выполнения запросов аутентификации. Для пользователей, которые не отображаются в этих журналах и не используют устаревшую проверку подлинности, реализуйте политику условного доступа или включите базовую политику: блокировать предыдущую проверку подлинности только для этих пользователей.

## <a name="moving-away-from-legacy-authentication"></a>Переход от устаревшей проверки подлинности 

Когда вы получите более хорошее представление о том, кто использует устаревшую проверку подлинности в вашем каталоге и какие приложения зависят от него, следующим шагом является обновление пользователей для использования современной проверки подлинности. Современная проверка подлинности — это метод управления удостоверениями, обеспечивающий более безопасную проверку подлинности и авторизацию пользователей. Если в каталоге имеется политика MFA, современная проверка подлинности гарантирует, что при необходимости пользователю будет предложено выполнить MFA. Это более безопасная альтернатива устаревшим протоколам проверки подлинности.

В этом разделе приводятся пошаговые инструкции по обновлению среды до современной проверки подлинности. Прежде чем включить устаревшую политику блокировки проверки подлинности в Организации, ознакомьтесь с приведенными ниже шагами.

### <a name="step-1-enable-modern-authentication-in-your-directory"></a>Шаг 1. Включение современной проверки подлинности в каталоге

Первый шаг в включении современной проверки подлинности гарантирует, что ваш каталог поддерживает современную проверку подлинности. Современная проверка подлинности включена по умолчанию для каталогов, созданных в или после 1 августа 2017. Если ваш каталог был создан до этой даты, необходимо вручную включить современную проверку подлинности для каталога, выполнив следующие действия.

1. Убедитесь, что ваш каталог уже поддерживает современную проверку подлинности, запустив  `Get-CsOAuthConfiguration`   из [модуля PowerShell Skype для бизнеса Online](/office365/enterprise/powershell/manage-skype-for-business-online-with-office-365-powershell).
1. Если команда возвращает пустое  `OAuthServers`   свойство, то современная проверка подлинности отключена. Обновите параметр, чтобы включить современную проверку подлинности с помощью  `Set-CsOAuthConfiguration` . Если  `OAuthServers`   свойство содержит запись, то все готово.

Не забудьте выполнить этот шаг перед переходом вперед. Очень важно, чтобы конфигурации каталогов были изменены первыми, так как они определяют, какой протокол будет использоваться всеми клиентами Office. Даже если вы используете клиенты Office, которые поддерживают современную проверку подлинности, по умолчанию они будут использовать устаревшие протоколы, если в вашем каталоге отключена современная проверка подлинности.

### <a name="step-2-office-applications"></a>Шаг 2. приложения Office

После включения современной проверки подлинности в каталоге можно начать обновление приложений, включив современную проверку подлинности для клиентов Office. Клиенты Office 2016 и более поздних версий поддерживают современные проверки подлинности по умолчанию. Никаких дополнительных действий не требуется.

При использовании клиентов Office 2013 с Windows или более ранней версии рекомендуется обновить версию до Office 2016 или более позднюю. Даже после завершения предыдущего шага включения современной проверки подлинности в каталог старые приложения Office продолжат использовать устаревшие протоколы проверки подлинности. Если вы используете клиенты Office 2013 и не можете немедленно выполнить обновление до Office 2016 или более поздней версии, выполните действия, описанные в следующей статье, чтобы [включить современную проверку подлинности для office 2013 на устройствах Windows](/office365/admin/security-and-compliance/enable-modern-authentication). Для защиты учетной записи при использовании устаревшей проверки подлинности рекомендуется использовать надежные пароли в каталоге. Ознакомьтесь с [защитой паролей Azure AD](../authentication/concept-password-ban-bad.md)   , чтобы запретить ненадежные пароли в каталоге.

Office 2010 не поддерживает современную проверку подлинности. Вам потребуется обновить все пользователи с Office 2010 до более новой версии Office. Рекомендуется выполнить обновление до Office 2016 или более поздней версии, так как по умолчанию блокируется устаревшая проверка подлинности.

Если вы используете macOS, рекомендуем выполнить обновление до Office для Mac 2016 или более поздней версии. При использовании собственного почтового клиента на всех устройствах должна быть установлена версия macOS 10,14 или более поздняя.

### <a name="step-3-exchange-and-sharepoint"></a>Шаг 3. Exchange и SharePoint

Для клиентов Outlook на базе Windows для использования современной проверки подлинности также должна быть включена современная проверка подлинности. Если современная проверка подлинности отключена для Exchange Online, клиенты Outlook на базе Windows, поддерживающие современные проверки подлинности (Outlook 2013 или более поздней версии), будут использовать обычную проверку подлинности для подключения к почтовым ящикам Exchange

Для SharePoint Online включена современная проверка подлинности по умолчанию. Для каталогов, созданных после 1 августа 2017, современная проверка подлинности включена по умолчанию в Exchange Online. Однако если ранее была отключена современная проверка подлинности или вы используете каталог, созданный до этой даты, выполните действия, описанные в следующей статье, чтобы [включить современную проверку подлинности в Exchange Online](/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online).

### <a name="step-4-skype-for-business"></a>Шаг 4. Skype для бизнеса

Чтобы предотвратить устаревшие запросы проверки подлинности, совершенные в Skype для бизнеса, необходимо включить современную проверку подлинности для Skype для бизнеса Online. Для каталогов, созданных после 1 августа 2017, современная проверка подлинности для Skype для бизнеса включена по умолчанию.

Мы рекомендуем перейти на Microsoft Teams, который по умолчанию поддерживает современные проверки подлинности. Однако если вы не можете выполнить миграцию в это время, необходимо включить современную проверку подлинности для Skype для бизнеса Online, чтобы клиенты Skype для бизнеса начали использовать современную проверку подлинности. Выполните действия, описанные в этой статье, [топологии Skype для бизнеса, поддерживаемые при современной проверке подлинности](/skypeforbusiness/plan-your-deployment/modern-authentication/topologies-supported), чтобы включить современную проверку подлинности для Skype для бизнеса.

Помимо включения современной проверки подлинности для Skype для бизнеса Online, мы рекомендуем включить современную проверку подлинности для Exchange Online при включении современной проверки подлинности для Skype для бизнеса. Этот процесс поможет синхронизировать состояние современной проверки подлинности в Exchange Online и Skype для бизнеса Online и не позволит получить несколько запросов на вход для клиентов Skype для бизнеса.

### <a name="step-5-using-mobile-devices"></a>Шаг 5. Использование мобильных устройств

Приложения на мобильном устройстве должны также блокировать проверку подлинности прежних версий. Мы рекомендуем использовать Outlook для мобильных устройств. Outlook для мобильных устройств поддерживает современную аутентификацию по умолчанию и будет соответствовать другим политикам базовой защиты MFA.

Чтобы использовать собственный почтовый клиент iOS, необходимо запустить iOS версии 11,0 или более поздней, чтобы убедиться, что почтовый клиент был обновлен для блокирования проверки подлинности прежних версий.

### <a name="step-6-on-premises-clients"></a>Шаг 6. локальные клиенты

Если вы используете гибридный клиент, использующий локальную систему Exchange Server и локальную сеть Skype для бизнеса, обе службы потребуется обновить, чтобы включить современную проверку подлинности. При использовании современной проверки подлинности в гибридной среде все еще выполняется проверка подлинности локальных пользователей. История авторизации доступа к ресурсам (файлам или сообщениям электронной почты).

Прежде чем вы сможете включить современную локальную проверку подлинности, убедитесь, что выполнены все предварительные требования. Теперь вы можете включить современную локальную проверку подлинности.

Действия по включению современной проверки подлинности можно найти в следующих статьях:

* [Как настроить локальный сервер Exchange Server для использования гибридной современной проверки подлинности](/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
* [Использование современной проверки подлинности (ADAL) с Skype для бизнеса](/skypeforbusiness/manage/authentication/use-adal)

## <a name="next-steps"></a>Дальнейшие действия

- [Как настроить локальный сервер Exchange Server для использования гибридной современной проверки подлинности](/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
- [Использование современной проверки подлинности (ADAL) с Skype для бизнеса](/skypeforbusiness/manage/authentication/use-adal)
- [Блокировать устаревших методов проверки подлинности](../conditional-access/block-legacy-authentication.md)