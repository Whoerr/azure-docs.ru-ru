---
title: Как единый вход для локальных ресурсов работает на подключенных к Azure AD устройствах | Документация Майкрософт
description: Узнайте, как расширить возможности единого входа, настроив гибридные Azure Active Directory присоединенные устройства.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: da22a4e5e9ab13ec18347e58bea6cfc5f45333de
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98630706"
---
# <a name="how-sso-to-on-premises-resources-works-on-azure-ad-joined-devices"></a>Как единый вход для локальных ресурсов работает на подключенных к Azure AD устройствах

Возможно, вам известно, что присоединенное к Azure Active Directory (AAD) устройство поддерживает возможность единого входа (SSO) в облачные приложения вашего клиента. Если в вашей среде используется локальная Active Directory (AD), вы также можете получить единый вход на устройства, присоединенные к Azure AD, к ресурсам и приложениям, использующим локальную службу AD. 

В этой статье описано, как это работает.

## <a name="prerequisites"></a>Предварительные требования

Для локального единого входа необходимо понимание сетевого взаимодействия с локальными контроллерами домена AD DS. Если устройства, присоединенные к Azure AD, не подключены к сети организации, требуется VPN или другая сетевая инфраструктура. 

## <a name="how-it-works"></a>Принцип работы 

Пользователи с устройствами, присоединенными к AAD, уже могут использовать единый вход для доступа к облачным приложениям в вашей среде. Если в вашей среде есть Azure AD и локальная служба AD, вам может потребоваться расширить возможности единого входа для локальных бизнес-приложений, файловых ресурсов и принтеров.

Устройства, присоединенные к AAD, не обмениваются данными со средой AD, так как они не присоединены к ней. Но вы можете предоставить этим устройствам дополнительные сведения о локальном экземпляре AD через Azure AD Connect.

Если у вас есть гибридная среда с Azure AD и локальной службой AD, скорее всего, вы уже развернули Azure AD Connect, чтобы синхронизировать локальную информацию об удостоверениях с облаком. В рамках процесса синхронизации Azure AD Connect синхронизирует данные локального пользователя и домена с Azure AD. При входе пользователя в устройство в гибридной среде, присоединенное к AAD, происходит следующее:

1. Azure AD отправляет сведения о локальном домене пользователя обратно на устройство вместе с [основным маркером обновления](concept-primary-refresh-token.md) .
1. Локальная служба безопасности (LSA) включает проверку подлинности Kerberos и NTLM на устройстве.

>[!NOTE]
> Чтобы включить для Windows Hello для бизнеса единый вход в локальной среде из устройства, присоединенного к AAD, требуется дополнительная настройка. Дополнительные сведения см. в руководстве по [настройке локального единого входа для устройств, присоединенных к AAD, с использованием Windows Hello для бизнеса](/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso-base). 

При попытке доступа к ресурсу, запрашивающему Kerberos или NTLM в локальной среде пользователя, устройство выполняет следующие действия:

1. Отправляет сведения о локальном домене и учетные данные пользователя в обнаруженный контроллер домена, чтобы выполнить аутентификацию пользователя.
1. Получает билет Kerberos [(TGT)](/windows/desktop/secauthn/ticket-granting-tickets) или токен NTLM на основе протокола, поддерживаемого локальным ресурсом или приложением. Если попытка получить TGT Kerberos или токен NTLM для домена завершается неудачно (связанное время ожидания DCLocator может вызвать задержку), записи диспетчера учетных данных или пользователь может получить всплывающее окно проверки подлинности, запрашивающее учетные данные для целевого ресурса.

Все приложения, на которых настроена **встроенная проверка подлинности Windows**, прозрачно применяют единый вход, когда пользователь обращается к ним.

## <a name="what-you-get"></a>Что вы получаете

С помощью единого входа вы можете выполнять следующие действия на устройствах, присоединенных к AAD: 

- получать доступ к UNC-пути на рядовом сервере AD;
- получать доступ к веб-серверу, входящему в AD, на котором настроена встроенная система безопасности Windows. 

Если вы хотите управлять локальной службой AD с устройства Windows, установите [средства удаленного администрирования сервера для Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).

Вы можете использовать:

- Оснастку "Active Directory — пользователи и компьютеры (ADUC)" для администрирования любых объектов AD. Но для этого нужно вручную указать домен, к которому вы хотите подключиться.
- Оснастку DHCP для администрирования сервера DHCP, присоединенного к домену AD. Но для этого нужно указать имя или адрес DHCP-сервера.
 
## <a name="what-you-should-know"></a>Необходимая информация

Возможно, вам нужно будет дополнительно настроить [фильтрацию по доменам](../hybrid/how-to-connect-sync-configure-filtering.md#domain-based-filtering) в Azure AD Connect, чтобы синхронизировать данные об используемых доменах.

Приложения и ресурсы, которые зависят от аутентификации на компьютере Active Directory, не смогут работать, так как у присоединенных к Azure AD устройств нет объектов-компьютеров в AD. 

Вы не сможете совместно с другими пользователями работать с файлами на устройстве, присоединенном к AAD.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в статье [Что такое управление устройствами в Azure Active Directory](overview.md). 
