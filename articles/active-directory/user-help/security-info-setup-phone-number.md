---
title: Настройка номера телефона в качестве метода проверки в Azure AD
description: Как на странице сведений для защиты (предварительная версия) настроить проверку подлинности с использованием номера телефона и мобильного устройства.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 02/13/2019
ms.author: curtand
ms.openlocfilehash: 7500b6b146a627dcdebacd9d2e2c7c61bc43c105
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "83744451"
---
# <a name="set-up-a-phone-number-as-your-verification-method"></a>Настройка номера телефона в качестве метода проверки

Ниже приведены инструкции по добавлению двухфакторной проверки подлинности и описаны способы сброса пароля. После первоначальной настройки вы можете вернуться к странице **Сведения для защиты**, чтобы добавить, обновить или удалить сведения для защиты.

Если вы получите предложение настроить эти сведения сразу после входа в рабочую или учебную учетную запись, воспользуйтесь подробными инструкциями из статьи [Настройка сведений для защиты (предварительная версия) в приглашении на странице входа](security-info-setup-signin.md).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

> [!Note]
> В сведениях о безопасности невозможно указать добавочные номера. Даже если указать их в правильном формате (например, +1 4255551234X12345), они будут удалены перед осуществлением звонка.
>
> Если параметр для настройки телефона не отображается, возможно, ваша организация не разрешает использовать этот способ проверки. Если это так, вам нужно выбрать другой способ или обратиться за помощью в службу поддержки своей организации.

## <a name="security-verification-versus-password-reset-authentication"></a>Сравнение проверки безопасности и аутентификации для сброса пароля

Методы проверки сведений для защиты используются как при двухфакторной проверке безопасности, так и при сбросе пароля. При этом не все методы можно использовать для обоих видов аутентификации.

| Метод | Используется для |
| ------ | -------- |
| Приложение Authenticator | Двухфакторная проверка и аутентификация для сброса пароля. |
| Текстовые сообщения | Двухфакторная проверка и аутентификация для сброса пароля. |
| Телефонные звонки | Двухфакторная проверка и аутентификация для сброса пароля. |
| Ключ безопасности | Двухфакторная проверка и аутентификация для сброса пароля. |
| Учетная запись электронной почты | Только аутентификация для сброса пароля. Для двухфакторной проверки необходимо выбрать другой метод. |
| Контрольные вопросы | Только аутентификация для сброса пароля. Для двухфакторной проверки необходимо выбрать другой метод. |

## <a name="set-up-phone-calls-from-the-security-info-page"></a>Настройка телефонных звонков на странице сведений для защиты

В зависимости от параметров организации у вас может быть возможность настроить телефонные звонки в качестве одного из методов проверки сведений для защиты.

>[!Note]
>Если вы предпочитаете вместо звонка получать текстовые сообщения, выполните действия из статьи [Настройка использования обмена текстовыми сообщениями в сведениях для защиты (предварительная версия)](security-info-setup-text-msg.md).

### <a name="to-set-up-phone-calls"></a>Настройка телефонных звонков

1. Войдите в рабочую или учебную учетную запись, а затем перейдите на страницу https://myaccount.microsoft.com/.

    ![Страница "Мой профиль" с выделенными ссылками на сведения для защиты](media/security-info/securityinfo-myprofile-page.png)

2. На панели навигации слева выберите **Сведения для защиты** или щелкните ссылку в блоке **Сведения для защиты**, а затем выберите **Добавить метод** на странице **Сведения для защиты**.

    ![Страница сведений для защиты с выделенным параметром "Добавить метод"](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. На странице **Добавить метод** выберите из раскрывающегося списка **Телефон**, а затем щелкните **Добавить**.

    ![Блок "Добавить метод" с выбранным методом "Телефон"](media/security-info/securityinfo-myprofile-addphonetext.png)

4. На странице **Телефон** введите номер телефона, который вы используете на мобильном устройстве, а затем выберите **Позвонить мне** и щелкните **Далее**.

    ![Добавление номера телефона и выбор метода телефонных звонков](media/security-info/securityinfo-myprofile-phonecall-addnumber.png)

5. Ответьте на проверочный телефонный вызов, который поступит на указанный номер телефона, и выполните голосовые инструкции.

    Результат этой проверки отобразится на странице.

    ![Уведомление об успехе, подключение номера телефона, выбор метода телефонных звонков и учетной записи](media/security-info/securityinfo-myprofile-phonetext-success.png)

    Сведения для защиты обновятся, и теперь вы сможете использовать телефонные звонки для подтверждения личности при двухфакторной проверке подлинности или самостоятельном сбросе пароля. Если вы хотите использовать телефонные звонки как основной метод, воспользуйтесь разделом [Изменение метода проверки сведений для защиты по умолчанию](#change-your-default-security-info-method) далее в этой статье.

## <a name="delete-phone-calls-from-your-security-info-methods"></a>Удаление телефонных звонков из списка методов получения сведений о безопасности

Если вы больше не хотите использовать телефонные звонки в качестве метода проверки сведений о защите, вы можете удалить его на странице **Сведения для защиты**.

>[!Important]
>Если вы удалите метод проверки с помощью телефонных звонков случайно, эту операцию нельзя будет отменить. Вам придется снова добавить этот метод проверки, как описано в разделе о [настройке телефонных звонков](#set-up-phone-calls-from-the-security-info-page) в этой статье.

### <a name="to-delete-phone-calls"></a>Удаление телефонных звонков

1. На странице **Сведения для защиты** щелкните ссылку **Удалить** рядом с параметром **Телефон**.

    ![Ссылка для удаления проверки с помощью телефона со страницы сведений для защиты](media/security-info/securityinfo-myprofile-phonetext-delete.png)

2. Выберите **Да** в окне подтверждения, чтобы удалить номер **телефона**. При этом номер телефона удаляется из сведений для защиты и со страницы **Сведения для защиты**. Если **телефон** был выбран как метод по умолчанию, вместо него будет автоматически выбран другой доступный метод.

## <a name="change-your-default-security-info-method"></a>Изменение метода проверки сведений для защиты по умолчанию

Если вам нужно, чтобы телефонные звонки использовались по умолчанию при входе в рабочую или учебную учетную запись с применением двухфакторной проверки подлинности или для сброса пароля, вы можете настроить этот метод на странице **Сведения для защиты**.

### <a name="to-change-your-default-security-info-method"></a>Чтобы изменить метод проверки сведений для защиты по умолчанию:

1. На странице **Сведения для защиты** выберите ссылку **Изменить** рядом с полем **Default sign-in method** (Метод входа по умолчанию).

    ![Ссылка на изменение метода входа по умолчанию](media/security-info/securityinfo-myprofile-phonetext-defaultchange.png)

2. Выберите вариант **Phone - call (Телефон — вызов) (*_номер_телефона_*)** из раскрывающегося списка доступных методов, а затем щелкните **Подтвердить**.

    ![Выбор метода для входа по умолчанию](media/security-info/securityinfo-myprofile-phonecall-changeddefault.png)

    Теперь по умолчанию будет использоваться новый метод входа в систему: **Phone - call (Телефон — вызов) (*_номер_телефона_*)** .

## <a name="additional-security-info-methods"></a>Дополнительные методы в сведениях для защиты

Чтобы подтвердить вашу личность, ваша организация может связаться с вами различными способами, в зависимости от того, что именно вы пытаетесь выполнить. Эти способы могут быть следующими:

- **Приложение Authenticator.** Скачайте и используйте приложение Authenticator, чтобы получать либо уведомление для утверждения, либо случайно созданный код утверждения для двухфакторной проверки подлинности или сброса пароля. Пошаговые инструкции о том, как настроить и использовать приложение Microsoft Authenticator, см. в статье о [настройке использования приложения проверки подлинности в сведениях для защиты](security-info-setup-auth-app.md).

- **Тестовое сообщение на мобильное устройство.** Введите номер своего мобильного устройства и получите сообщение с кодом, необходимым для двухфакторной проверки подлинности или сброса пароля. Пошаговые инструкции по проверке личности с помощью текстового сообщения (SMS) см. в статье о [настройке использования текстовых сообщений (SMS) в сведениях для защиты](security-info-setup-text-msg.md).

- **Ключ безопасности**. Зарегистрируйте ключ безопасности, совместимый с Майкрософт, и используйте его вместе с ПИН-кодом для двухфакторной проверки подлинности или сброса пароля. Пошаговые инструкции о том, как подтвердить свою личность с помощью ключа безопасности, см. в статье о [настройке использования ключа безопасности в сведениях для защиты](security-info-setup-security-key.md).

- **Адрес электронной почты.** Введите рабочий или учебный адрес электронной почты, чтобы получить электронное письмо для сброса пароля. Этот вариант недоступен для двухфакторной проверки подлинности. Пошаговые инструкции о том, как настроить адрес электронной почты, см. в статье о [настройке использования электронной почты в сведениях для защиты](security-info-setup-email.md).

- **Контрольные вопросы.** Ответьте на контрольные вопросы, созданные вашим администратором для организации. Этот параметр доступен только для сброса пароля, а не для двухфакторной проверки подлинности. Пошаговые инструкции по настройке контрольных вопросов в сведениях для защиты см. в [этой статье](security-info-setup-questions.md).

    >[!Note]
    >Если некоторые из этих параметров отсутствуют, скорее всего, ваша организация не разрешает данные методы. Если это так, вам нужно выбрать доступный метод или обратиться за помощью к администратору.

## <a name="next-steps"></a>Дальнейшие действия

- Если вы забыли свой пароль, сбросьте его на [портале сброса пароля](https://passwordreset.microsoftonline.com/) или выполните действия, описанные в статье о [сбросе пароля рабочей или учебной учетной записи](active-directory-passwords-update-your-own-password.md).

- Советы по устранению неполадок и справку по проблемам со входом в систему см. в статье [Не удается войти в учетную запись Майкрософт](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
