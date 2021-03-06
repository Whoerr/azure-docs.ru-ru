---
title: Известные проблемы и устранение неполадок для гибридных FIDO2 ключей безопасности — Azure Active Directory
description: Узнайте о некоторых известных проблемах и способах устранения неполадок при входе в систему ключа безопасности гибридного FIDO2 с паролем с помощью Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: troubleshooting
ms.date: 02/22/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 435b27255ce58a3541d6b0d3a76bdf4080aa3962
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101648786"
---
# <a name="troubleshooting-for-hybrid-deployments-of-fido2-security-keys-in-azure-ad"></a>Устранение неполадок гибридных развертываний ключей безопасности FIDO2 в Azure AD 

В этой статье рассматриваются часто задаваемые вопросы о гибридных устройствах, присоединенных к Azure AD, и входе без пароля в локальные ресурсы. Эта функция без пароля позволяет включить проверку подлинности Azure AD на устройствах Windows 10 для гибридных устройств, присоединенных к Azure AD, с помощью ключей безопасности FIDO2. Пользователи могут входить в Windows на своих устройствах с помощью современных учетных данных, таких как FIDO2 ключи, и получать доступ к ресурсам на базе традиционных домен Active Directory Services (AD DS) с помощью простого интерфейса единого входа (SSO) для своих локальных ресурсов.

Поддерживаются следующие сценарии для пользователей в гибридной среде:

* Войдите в гибридные устройства, присоединенные к Azure AD, с помощью ключей безопасности FIDO2 и получите единый доступ к локальным ресурсам.
* Войдите в устройства, присоединенные к Azure AD, с помощью ключей безопасности FIDO2 и получите единый доступ к локальным ресурсам.

Чтобы приступить к работе с ключами безопасности FIDO2 и гибридным доступом к локальным ресурсам, ознакомьтесь со следующими статьями:

* [Ключи безопасности без пароля](howto-authentication-passwordless-security-key.md)
* [Вход без пароля в Windows 10](howto-authentication-passwordless-security-key-windows.md)
* [Вход без пароля в локальной среде](howto-authentication-passwordless-security-key-on-premises.md)

## <a name="known-issues"></a>Известные проблемы

* [Пользователи не могут войти с помощью ключей безопасности FIDO2, так как поверхность Windows Hello слишком быстрая и является механизмом входа по умолчанию](#users-are-unable-to-sign-in-using-fido2-security-keys-as-windows-hello-face-is-too-quick-and-is-the-default-sign-in-mechanism)
* [Пользователи не могут использовать ключи безопасности FIDO2 сразу после создания гибридного компьютера, присоединенного к Azure AD](#users-arent-able-to-use-fido2-security-keys-immediately-after-they-create-a-hybrid-azure-ad-joined-machine)
* [Пользователи не могут получить единый вход к сетевому ресурсу NTLM после входа с помощью ключа безопасности FIDO2 и получения запроса учетных данных](#users-are-unable-to-get-sso-to-my-ntlm-network-resource-after-signing-in-with-a-fido2-security-key-and-receiving-a-credential-prompt)

### <a name="users-are-unable-to-sign-in-using-fido2-security-keys-as-windows-hello-face-is-too-quick-and-is-the-default-sign-in-mechanism"></a>Пользователи не могут войти с помощью ключей безопасности FIDO2, так как поверхность Windows Hello слишком быстрая и является механизмом входа по умолчанию

Windows Hello Face — это оптимальный способ работы с устройством, на котором зарегистрирован пользователь. Ключи безопасности FIDO2 предназначены для использования на общих устройствах или в том случае, если регистрация Windows Hello для бизнеса является барьером.

Если пользователь Windows Hello не попытается выполнить сценарий входа с помощью ключа безопасности FIDO2, пользователи могут отключить вход с помощью Hello Face, удалив регистрацию лиц в разделе **параметры > параметры Sign-In**.

### <a name="users-arent-able-to-use-fido2-security-keys-immediately-after-they-create-a-hybrid-azure-ad-joined-machine"></a>Пользователи не могут использовать ключи безопасности FIDO2 сразу после создания гибридного компьютера, присоединенного к Azure AD

После завершения процесса присоединения к домену и перезапуска при чистой установке гибридного компьютера, присоединенного к Azure AD, необходимо войти с помощью пароля и дождаться синхронизации политики, прежде чем вы сможете использовать для входа ключ безопасности FIDO2.

Такое поведение является известным ограничением для устройств, присоединенных к домену, и не относится к FIDO2 ключам безопасности.

Чтобы проверить текущее состояние, используйте `dsregcmd /status` команду. Убедитесь, что *AzureAdJoined* и *домаинжоинед* показывать *Да*.

### <a name="users-are-unable-to-get-sso-to-my-ntlm-network-resource-after-signing-in-with-a-fido2-security-key-and-receiving-a-credential-prompt"></a>Пользователи не могут получить единый вход к сетевому ресурсу NTLM после входа с помощью ключа безопасности FIDO2 и получения запроса учетных данных

Убедитесь, что все контроллеры домена исправлены на время ответа на обслуживание запроса ресурсов. Чтобы проверить, отображается ли сервер, на котором работает компонент, просмотрите выходные данные `nltest /dsgetdc:<dc name> /keylist /kdc`

Если вы видите контроллер домена с этим компонентом, возможно, пароль пользователя изменился с момента входа, или есть другая проблема. Собирайте журналы, как описано в следующем разделе группы поддержки Майкрософт для отладки.

## <a name="troubleshoot"></a>Диагностика

Существует две области для устранения неполадок, [связанных с клиентом окна](#windows-client-issues), или [проблемы развертывания](#deployment-issues).

### <a name="windows-client-issues"></a>Проблемы с клиентом Windows

Чтобы получить данные, помогающие устранять неполадки при входе в Windows или доступе к локальным ресурсам с устройств Windows 10, выполните следующие действия.

1. Откройте приложение **центра обратной связи** . Убедитесь, что ваше имя находится в левом нижнем углу приложения, а затем выберите **создать новый элемент отзыва**.

    Для типа элемента отзыва выберите *проблема*.

1. Выберите категорию *безопасность и конфиденциальность* , а затем подкатегорию *Fido* .
1. Установите флажок для *отправки вложенных файлов и диагностики в корпорацию Майкрософт вместе с моими отзывами*.
1. Выберите *повторно создать мои проблемы*, а затем *Запустите запись*.
1. Блокировка и разблокировка компьютера с помощью ключа безопасности FIDO2. В случае возникновения проблемы попробуйте разблокировать другие учетные данные.
1. Вернитесь в **центр обратной связи**, выберите **прерывать запись** и отправьте свой отзыв.
1. Перейдите на страницу *отзывов* , а затем на вкладку *Мои отзывы* . Выберите недавно отправленный отзыв.
1. Нажмите кнопку *поделиться* в правом верхнем углу, чтобы получить ссылку на отзыв. Включите эту ссылку при открытии обращения в службу поддержки или в ответ на инженер, назначенный существующему обращению в службу поддержки.

Собираются следующие журналы событий и сведения о ключах реестра:

**Разделы реестра**

* *HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\FIDO [ \* ]*
* *HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\PasswordForWork\* [ \* ]*
* *HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Policies\PasswordForWork\* [ \* ]*

**Диагностические сведения**

* Динамический дамп ядра
* Получение сведений о пакете AppX
* Файлы контекста если

**Журналы событий**

* *%SystemRoot%\System32\winevt\Logs\Microsoft-Windows-AAD%40Operational.evtx*
* *%SystemRoot%\System32\winevt\Logs\Microsoft-Windows-WebAuthN%40Operational.evtx*
* *%SystemRoot%\System32\winevt\Logs\Microsoft-Windows-HelloForBusiness%40Operational.evtx*

### <a name="deployment-issues"></a>Проблемы развертывания

Чтобы устранить проблемы с развертыванием сервера Kerberos Azure AD, используйте новый модуль PowerShell, входящий в состав Azure AD Connect.

#### <a name="viewing-the-logs"></a>Просмотр журналов

Командлеты PowerShell сервера Kerberos для Azure AD используют то же ведение журнала, что и стандартный мастер Azure AD Connect. Чтобы просмотреть сведения или сведения об ошибках из командлетов, выполните следующие действия.

1. На Azure AD Connect сервере перейдите к `C:\ProgramData\AADConnect\` . Эта папка скрыта по умолчанию.
1. Откройте и просмотрите последний файл, `trace-*.log` расположенный в каталоге.

#### <a name="viewing-the-azure-ad-kerberos-server-objects"></a>Просмотр объектов сервера Azure AD Kerberos

Чтобы просмотреть объекты сервера Kerberos Azure AD и убедиться, что они находятся в правильном порядке, выполните следующие действия:

1. На Azure AD Connect сервере откройте PowerShell и перейдите к `C:\Program Files\Microsoft Azure Active Directory Connect\AzureADKerberos\`
1. Выполните следующие команды PowerShell, чтобы просмотреть сервер Azure AD Kerberos как с помощью Azure AD, так и из локальной AD DS.

    Замените *Corp.contoso.com* именем локального домена AD DS.

    ```powershell
    Import-Module ".\AzureAdKerberos.psd1"

    # Specify the on-premises AD DS domain.
    $domain = "corp.contoso.com"

    # Enter an Azure Active Directory Global Admin username and password.
    $cloudCred = Get-Credential

    # Enter a Domain Admin username and password.
    $domainCred = Get-Credential

    # Get the Azure AD Kerberos Server Object
    Get-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential
    $domainCred
    ```

Команда выводит свойства сервера Kerberos Azure AD из Azure AD и локального AD DS. Проверьте свойства, чтобы убедиться, что все находится в правильном порядке. Используйте таблицу ниже для проверки свойств.

Первый набор свойств относится к объектам в локальной среде AD DS. Вторая половина (свойства, начинающиеся с * Cloud * *), относятся к объекту сервера Kerberos в Azure AD:

| Свойство           | Описание  |
|--------------------|--------------|
| Идентификатор                 | Уникальный *идентификатор* объекта AD DS контроллера домена. |
| DomainDnsName      | Доменное имя DNS домена AD DS. |
| ComputerAccount    | Объект учетной записи компьютера для объекта сервера Kerberos Azure AD (DC). |
| UserAccount        | Отключенный объект учетной записи пользователя, содержащий ключ шифрования TGT сервера Kerberos Azure AD. DN этой учетной записи — *CN = krbtgt_AzureAD, CN = Users, <Domain-DN>* |
| кэйверсион         | Версия ключа шифрования билета TGT сервера Azure AD Kerberos. Версия назначается при создании ключа. Затем версия увеличивается при каждом повороте ключа. Приращения основано на метаданных репликации и, скорее всего, будет больше единицы.<br /><br /> Например, начальным *кэйверсион* может быть *192272*. При первом повороте ключа версия может перейти на *212621*.<br /><br /> Важно убедиться, что *кэйверсион* для локального объекта и *клаудкэйверсион* для облачного объекта одинаковы. |
| кэйупдатедон       | Дата и время, когда ключ шифрования билета TGT сервера Azure AD был обновлен или создан. |
| кэйупдатедфром     | Контроллер домена, на котором последний раз был обновлен ключ шифрования билета TGT сервера Azure AD. |
| CloudId            | *Идентификатор* из объекта Azure AD. Должен соответствовать указанному выше *идентификатору* . |
| клауддомаинднснаме | *Домаинднснаме* из объекта Azure AD. Должен соответствовать *домаинднснаме* выше. |
| клаудкэйверсион    | *Кэйверсион* из объекта Azure AD. Должен соответствовать *кэйверсион* выше. |
| клаудкэйупдатедон  | *Кэйупдатедон* из объекта Azure AD. Должен соответствовать *кэйупдатедон* выше. |

## <a name="next-steps"></a>Дальнейшие действия

Чтобы приступить к работе с ключами безопасности FIDO2 и гибридным доступом к локальным ресурсам, ознакомьтесь со следующими статьями:

* [Ключи безопасности FIDO2 без пароля](howto-authentication-passwordless-security-key.md)
* [Вход без пароля в Windows 10](howto-authentication-passwordless-security-key-windows.md)
* [Вход без пароля в локальной среде](howto-authentication-passwordless-security-key-on-premises.md)
