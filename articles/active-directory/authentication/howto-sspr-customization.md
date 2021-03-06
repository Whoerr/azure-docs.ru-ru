---
title: Настройка самостоятельного сброса пароля — Azure Active Directory
description: Узнайте, как настроить параметры отображения и взаимодействия с пользователем для самостоятельного сброса пароля в Azure AD.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/17/2020
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3766db0d5070f15216ac612353e2c25737ad092a
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2020
ms.locfileid: "96741632"
---
# <a name="customize-the-user-experience-for-azure-active-directory-self-service-password-reset"></a>Настройка интерфейса пользователя для Azure Active Directory самостоятельного сброса пароля

Самостоятельный сброс пароля (SSPR) дает пользователям в Azure Active Directory (Azure AD) возможность изменить или сбросить пароль без участия администратора или службы поддержки. Если учетная запись пользователя заблокирована или пользователь забыл пароль, выполнив несложные инструкции, он сможет выполнить разблокировку и вернуться к работе. Эта возможность снижает количество обращений в службу технической поддержки и минимизирует время простоя из-за невозможности войти на устройство или в приложение.

Чтобы улучшить работу SSPR для пользователей, можно настроить внешний вид страницы сброса пароля, уведомлений по электронной почте или страниц входа. Эти параметры настройки позволяют ясно понять, что пользователь находится в нужном месте, и предоставить им уверенность в доступе к ресурсам компании.
    
В этой статье показано, как настроить ссылку на электронную почту SSPR для пользователей, фирменной символики компании и ссылки на страницу входа AD FS.

## <a name="customize-the-contact-your-administrator-link"></a>Настройка ссылки "Обратитесь к администратору"

Чтобы помочь пользователям обращаться за помощью к службе самостоятельного сброса пароля, на портале сброса паролей отображается ссылка "обратитесь к администратору". Если пользователь выбирает эту ссылку, он выполняет одно из двух действий:

* Если эта ссылка на контакт остается в состоянии по умолчанию, то администратору отправляется сообщение электронной почты и запрашивается помощь в изменении пароля пользователя. В следующем образце электронного сообщения отображается это сообщение электронной почты по умолчанию:

    ![Пример запроса на сброс электронной почты, отправленный администратору](./media/howto-sspr-customization/sspr-contact-admin.png)

* Если пользовательская настройка, отправляет пользователя на веб-страницу или адрес электронной почты, указанный администратором, для получения помощи.
    * При настройке этого параметра рекомендуется задать для этого пользователя, который уже знаком с для поддержки.

    > [!WARNING]
    > Если настроить этот параметр с помощью адреса электронной почты и учетной записи, требующей сброса пароля, пользователь не сможет обратиться за помощью.

### <a name="default-email-behavior"></a>Поведение электронной почты по умолчанию

Контактное письмо по умолчанию отправляется получателям в следующем порядке:

1. Если роль *администратора службы технической поддержки* или роль *администратора паролей* назначена, то уведомляются администраторы с этими ролями.
1. Если администратор службы технической поддержки или администратор паролей не назначен, то уведомляются администраторы с ролью *администратора пользователей* .
1. Если ни одна из предыдущих ролей не назначена, то уведомляются *Глобальные администраторы* .

Во всех случаях уведомление будет отправлено не более чем 100 получателям.

Дополнительные сведения о различных ролях администраторов и их назначении см. в статье [Назначение ролей администратора в Azure Active Directory](../roles/permissions-reference.md).

### <a name="disable-contact-your-administrator-emails"></a>Отключение отправки электронных сообщений при выборе ссылки "Обратитесь к администратору"

Если организации не нужно уведомлять администраторов о запросах на сброс пароля, можно использовать следующие параметры конфигурации:

* Настройте ссылку на службу технической поддержки для предоставления URL-адреса или адреса mailto, который пользователи могут использовать для получения помощи. Этот параметр находится в **Password Reset** папке  >  **Customization**  >  **настраиваемой службы технической поддержки или URL-адресе** настройки сброса пароля.
* Включите самостоятельный сброс пароля для всех пользователей. Этот параметр находится в разделе Свойства **сброса пароля**  >  **Properties**. Если вы не хотите, чтобы пользователи могли сбрасывать пароли, можно ограничить доступ, назначив пустую группу. *Этот вариант не рекомендуется.*

## <a name="customize-the-sign-in-page-and-access-panel"></a>Настройка страницы входа и панели доступа

Вы можете настроить страницу входа, например добавить эмблему, которая отображается вместе с изображением, которое соответствует фирменной символике вашей компании. Дополнительные сведения о настройке фирменной символики компании см. [в статье Добавление фирменной символики компании на страницу входа в Azure AD](../fundamentals/customize-branding.md).

Выбранные вами изображения отображаются в следующих случаях:

* После того, как пользователь вводит свое имя пользователя.
* Когда пользователь обращается к настраиваемому URL-адресу:
   * Передавая `whr` параметр на страницу сброса пароля, например `https://login.microsoftonline.com/?whr=contoso.com`
   * Передавая `username` параметр на страницу сброса пароля, например `https://login.microsoftonline.com/?username=admin@contoso.com`

### <a name="directory-name"></a>Имя каталога

Чтобы сделать представление более понятным для пользователей, можно изменить название организации на портале и в автоматизированном обмене данными. Чтобы изменить атрибут имени каталога в портал Azure, перейдите к **Azure Active Directory**  >  **Свойства**. Это понятное название организации является наиболее видимым в автоматизированных сообщениях электронной почты, как показано в следующих примерах:

* Понятное имя в письме, например "*Майкрософт от имени", демонстрация Contoso*"
* Строка темы в сообщении электронной почты, например "*код проверки электронной почты для демонстрационной учетной записи Contoso*"

## <a name="customize-the-ad-fs-sign-in-page"></a>Настройка страницы входа AD FS

Если для событий входа пользователя используется службы федерации Active Directory (AD FS) (AD FS), можно добавить ссылку на страницу входа, следуя указаниям в статье, чтобы [Добавить описание страницы входа](/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Предоставьте пользователям ссылку на страницу для ввода рабочего процесса SSPR, например *https://passwordreset.microsoftonline.com* . Чтобы добавить ссылку на страницу входа AD FS, используйте следующую команду на сервере AD FS.

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><a href='https://passwordreset.microsoftonline.com' target='_blank'>Can't access your account?</a></p>"
```

## <a name="next-steps"></a>Дальнейшие действия

Сведения об использовании SSPR в вашей среде см. в статье [Параметры отчетов для управления паролями в Azure AD](howto-sspr-reporting.md).

Если у вас или у пользователей возникли проблемы с SSPR, см. раздел [Устранение неполадок самостоятельного сброса пароля](./troubleshoot-sspr.md)