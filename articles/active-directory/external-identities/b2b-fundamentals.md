---
title: Рекомендации и рекомендации по Azure Active Directory B2B
description: Ознакомьтесь с рекомендациями и рекомендациями для доступа гостей "бизнес — бизнес" (B2B) в Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94fd488ceb7ddb3724dd576c97c9070481e95147
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100365639"
---
# <a name="azure-active-directory-b2b-best-practices"></a>Рекомендации по Azure Active Directory B2B
Эта статья содержит рекомендации и рекомендации по совместной работе B2B в Azure Active Directory (Azure AD).

   > [!IMPORTANT]
   > **Начиная с 2021 октября** Корпорация Майкрософт больше не будет поддерживать погашение приглашений, создавая неуправляемые ("вирусные" или "JIT") учетные записи Azure AD и клиенты для сценариев совместной работы B2B. В этот момент функция секретного кода одноразового пароля будет включена для всех существующих клиентов и включена по умолчанию для новых клиентов. Мы создаем функцию одноразового секретного кода по электронной почте, так как она обеспечивает простой метод проверки подлинности для гостевых пользователей. Однако вы можете отключить эту функцию, если вы решили не использовать ее. Дополнительные сведения см. в разделе [однократная проверка подлинности с помощью секретного кода](one-time-passcode.md)


## <a name="b2b-recommendations"></a>Рекомендации B2B
| Рекомендация | Комментарии |
| --- | --- |
| Для оптимизации процесса входа в Федерацию с поставщиками удостоверений | По возможности Федерацию напрямую с поставщиками удостоверений разрешает пользователям, которые приглашают войти в ваши общие приложения и ресурсы, без необходимости создавать учетные записи Майкрософт (MSAs) или учетные записи Azure AD. Вы можете использовать [функцию Google Federation](google-federation.md) , чтобы разрешить гостевым пользователям B2B входить в систему с использованием учетных записей Google. Также можно использовать [функцию Direct Federation (Предварительная версия)](direct-federation.md) , чтобы настроить прямую федерацию с любой организацией, поставщик удостоверений (IDP) которого поддерживает протокол SAML 2,0 или WS-Fed. |
| Используйте функцию одноразового секретного кода электронной почты для гостей B2B, которые не могут проходить проверку подлинности другими способами. | Функция [одноразового секретного кода для электронной почты](one-time-passcode.md) проверяет подлинность гостевых пользователей B2B, когда они не могут пройти проверку подлинности с помощью других средств, таких как Azure AD, учетная запись Майкрософт (MSA) или Google Federation. Если гостевой пользователь активирует приглашение или обращается к общему ресурсу, он может запросить временный код, который будет отправлен на адрес электронной почты этого пользователя. Этот код нужно ввести, чтобы войти в систему. |
| Добавление фирменной символики организации на страницу входа | Вы можете настроить страницу входа так, чтобы она была более интуитивно понятной для гостевых пользователей B2B. См. статью [Добавление фирменной символики компании для входа и страниц панели доступа](../fundamentals/customize-branding.md). |
| Добавление заявления о конфиденциальности в процесс активации гостевых пользователей B2B | Вы можете добавить URL-адрес заявления о конфиденциальности вашей организации в первый процесс активации приглашения, чтобы приглашенный пользователь должен согласиться с условиями конфиденциальности для продолжения. См. статью [как добавить сведения о конфиденциальности вашей организации в Azure Active Directory](../fundamentals/active-directory-properties-area.md). |
| Использование функции неполного приглашения (Предварительная версия) для одновременного приглашения нескольких гостевых пользователей B2B | Одновременное приглашение нескольких гостевых пользователей в Организации с помощью функции неполного просмотра приглашений в портал Azure. Эта функция позволяет отправить CSV-файл для создания гостевых пользователей B2B и отправки приглашений в массовой службе. См. [руководство по сбору пользователей B2B](tutorial-bulk-invite.md). |
| Применение политик условного доступа для многофакторной проверки подлинности (MFA) | Рекомендуется применять политики MFA к приложениям, которые вы хотите использовать совместно с пользователями партнера B2B. Таким образом, MFA будет согласованно применяться к приложениям в клиенте независимо от того, использует ли Партнерская организация MFA. См. раздел [Условный доступ для пользователей службы совместной работы B2B](conditional-access.md). |
| Если вы используете политики условного доступа на основе устройств, используйте списки исключений, чтобы разрешить доступ пользователям B2B. | Если в вашей организации включены политики условного доступа на основе устройств, то устройства с гостевой учетной записью B2B будут заблокированы, так как они не управляются вашей организацией. Вы можете создать списки исключений, содержащие конкретных пользователей-партнеров, чтобы исключить их из политики условного доступа на основе устройств. См. раздел [Условный доступ для пользователей службы совместной работы B2B](conditional-access.md). |
| Используйте URL-адрес, зависящий от клиента, при предоставлении прямых ссылок на гостевых пользователей B2B | В качестве альтернативы электронному письму с приглашением вы можете предоставить гостю прямую ссылку на ваше приложение или портал. Эта прямая ссылка должна быть зависящей от клиента, что означает, что она должна включать идентификатор клиента или проверенный домен, чтобы Гостевая учетная запись могла проходить проверку подлинности в клиенте, где расположено общее приложение. См. статью [Активация для гостевого пользователя](redemption-experience.md). |
| При разработке приложения используйте UserType для определения интерфейса пользователя гостевой системы.  | Если вы разрабатываете приложение и хотите предоставить различные возможности для пользователей и гостевых пользователей, используйте свойство UserType. Утверждение UserType сейчас не включено в токен. Приложения должны использовать API Microsoft Graph, чтобы запросить у пользователя каталог для получения их UserType. |
| Изменение свойства UserType *только* в случае изменения связи пользователя с Организацией | Хотя PowerShell можно использовать для преобразования свойства UserType для пользователя из члена в Guest (и наоборот), следует изменить это свойство только в том случае, если изменяется связь пользователя с Организацией. См. раздел [Свойства гостевого пользователя B2B](user-properties.md).|

## <a name="next-steps"></a>Следующие шаги

[Управление общим доступом B2B](delegate-invitations.md)