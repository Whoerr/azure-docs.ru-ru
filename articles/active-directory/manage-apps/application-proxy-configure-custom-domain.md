---
title: Пользовательские домены в Azure AD Application Proxy
description: Настройка личных доменов и управление ими в прокси приложения Azure AD.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 10/24/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 49edab5ed20749cb705d160e5cdc46a16bdee951
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99258037"
---
# <a name="configure-custom-domains-with-azure-ad-application-proxy"></a>Настройка личных доменов с помощью прокси приложения Azure AD.

При публикации приложения через прокси приложения Azure Active Directory создается внешний URL-адрес для ваших пользователей. Имя этого URL-адреса содержит домен по умолчанию *ваш_клиент-msappproxy.net*. Например, если вы опубликовали приложение *Expenses* и используете клиент *Contoso*, то внешним URL-адресом будет *https:\//expenses-contoso.msappproxy.net*. Если вы хотите использовать собственное доменное имя, вместо *msappproxy.net*, настройте личный домен для своего приложения. 

## <a name="benefits-of-custom-domains"></a>Преимущества личных доменов

Личные домены желательно настраивать для приложений, когда это возможно. Ниже приведены некоторые причины для использования пользовательских доменов.

- Связи между приложениями работают даже за пределами корпоративной сети. Без личного домена, если приложение имеет жестко запрограммированные внутренние ссылки на целевые объекты, находящиеся за пределами прокси приложения, и эти ссылки не разрешимы извне, они будут нарушены. Если внутренние и внешние URL-адреса одинаковы, этой проблемы можно избежать. Если вы не можете использовать личные домены, см. другие способы решения этой проблемы в статье [Перенаправление жестких ссылок для приложений, опубликованных с помощью Azure AD Application Proxy](./application-proxy-configure-hard-coded-link-translation.md). 
  
- Пользователям будет проще работать, так как они могут получать доступ к приложению по одному и тому же URL-адресу из сети или из-за ее пределов. Им не нужно будет изучать разные внутренние и внешние URL-адреса или отслеживать свое текущее расположение. 

- Вы сможете управлять своей фирменной символикой и создавать необходимые URL-адреса. Личный домен может помочь добиться доверия пользователей, поскольку пользователи видят и используют привычное имя вместо *msappproxy.net*.

- Некоторые конфигурации будут работать только с личными доменами. Например, для приложений, использующих язык разметки заявлений системы безопасности (SAML) (SAML), требуются личные домены, скажем, если вы используете службы федерации Active Directory (AD FS) (AD FS), но не можете использовать WS-Federation. Дополнительные сведения см. в разделе [Работа с приложениями, поддерживающими утверждения, в прокси приложения](application-proxy-configure-for-claims-aware-applications.md). 

Если вы не можете сделать внутренний и внешний URL-адреса одинаковыми, использование личных доменов не так важно, но все же позволяет воспользоваться другими преимуществами. 

## <a name="dns-configuration-options"></a>Параметры конфигурации DNS

Существует несколько вариантов настройки конфигурации DNS в зависимости от требований.

### <a name="same-internal-and-external-url-different-internal-and-external-behavior"></a>Одинаковый внутренний и внешний URL-адрес, разное внутренне и внешнее поведение. 

Если вы не хотите, чтобы внутренние пользователи перенаправлялись через прокси приложения, можно настроить *разделенную систему DNS*. Разделенная инфраструктура DNS направляет внутренние узлы на внутренний сервер доменных имен, а внешние узлы — на внешний сервер доменных имен для разрешения имен. 

![Разделенная система DNS](./media/application-proxy-configure-custom-domain/split-brain-dns.png)

### <a name="different-internal-and-external-urls"></a>Разные внутренние и внешние URL-адреса 

Если внутренние и внешние URL-адреса различаются, не нужно настраивать поведение разделенной системы, так как маршрутизация пользователей определяется по URL-адресу. В этом случае вы изменяете только внешний DNS и маршрутизируете внешний URL-адрес к конечной точке прокси приложения. 

При выборе личного домена для внешнего URL-адреса на панели информации отображается запись CNAME, которую необходимо добавить к внешнему поставщику DNS. Эти сведения всегда можно просмотреть, перейдя на страницу **прокси приложения**.

## <a name="set-up-and-use-custom-domains"></a>Настройка и использование личных доменов

Чтобы настроить локальное приложение на использование личного домена, необходимы проверенный личный домен Azure Active Directory, PFX-сертификат для личного домена и локальное приложение для настройки. 

### <a name="create-and-verify-a-custom-domain"></a>Создание и проверка личного домена

Для создания и проверки личного домена выполните следующие действия.

1. В Azure Active Directory выберите **Имена личных доменов** в левой области навигации, а затем выберите **Добавить личный домен**. 
1. Введите имя личного домена и выберите **Добавить домен**. 
1. На странице домена скопируйте сведения о записи TXT для своего домена. 
1. Перейдите к регистратору домена и создайте новую запись типа TXT для своего домена на основе скопированных данных DNS.
1. После регистрации домена на странице домена в Azure Active Directory выберите **Проверить**. Когда состояние домена станет **Проверен**, домен можно использовать во всех конфигурациях Azure AD, включая прокси приложения. 

Более подробные инструкции см. в разделе [Добавление имени личного домена с помощью портала Azure Active Directory](../fundamentals/add-custom-domain.md).

### <a name="configure-an-app-to-use-a-custom-domain"></a>Настройка приложения для использования личного домена

Чтобы опубликовать приложение через прокси приложения с помощью личного домена, выполните следующие действия.

1. Для нового приложения в Azure Active Directory выберите **Корпоративные приложения** в левой области навигации. Выберите **Новое приложение**. В разделе **Локальные приложения** выберите **Добавление локального приложения**. 
   
   Для приложения, которое уже находится в **Корпоративных приложениях**, выберите его из списка, а затем выберите **Прокси приложения** в левой области навигации. 

2. На странице "Параметры прокси приложения" введите **Имя**, если вы добавляете собственное локальное приложение.

3.  В поле **Внутренний URL-адрес** введите внутренний URL-адрес приложения.
   
4. В поле **Внешний URL-адрес** раскройте список и выберите личный домен, который хотите использовать.
   
5. Выберите **Добавить**.
   
   ![Выбор личного домена](./media/application-proxy-configure-custom-domain/application-proxy.png)
   
6. Если у домена уже есть сертификат, то поле **Сертификат** отображает сведения об этом сертификате. В ином случае выберите поле **Сертификат**.
   
   ![Щелкните, чтобы передать сертификат.](./media/application-proxy-configure-custom-domain/certificate.png)
   
7. На странице **SSL-сертификат** перейдите к файлу сертификата PFX и выберите его. Введите пароль для сертификата и выберите **Отправить сертификат**. Дополнительные сведения о сертификатах см. в разделе [Сертификаты для личных доменов](#certificates-for-custom-domains). Если сертификат недействителен или возникла проблема с паролем, появится сообщение об ошибке. В [разделе часто задаваемые вопросы о прокси приложения](application-proxy-faq.md#application-configuration) содержатся некоторые действия по устранению неполадок.
   
   ![Загрузка сертификата](./media/application-proxy-configure-custom-domain/ssl-certificate.png)
   
   > [!TIP] 
   > Личному домену требуется только однократная отправка сертификата. После этого переданный сертификат применяется автоматически при использовании личного домена для других приложений.
   
8. Если вы добавили сертификат, на странице **Прокси приложения** выберите **Сохранить**. 
   
9. На панели информации страницы **Прокси приложения** обратите внимание на запись CNAME, которую необходимо добавить в зону DNS. 
   
   ![Добавление записи CNAME DNS](./media/application-proxy-configure-custom-domain/dns-info.png)
   
10. Следуйте инструкциям в разделе [Управление записями DNS и наборами записей с помощью портала Azure](../../dns/dns-operations-recordsets-portal.md), чтобы добавить запись DNS, которая перенаправляет новый внешний URL-адрес в домен *msappproxy.net*.
   
11. Чтобы проверить правильность настройки записи DNS, используйте команду [nslookup](https://social.technet.microsoft.com/wiki/contents/articles/29184.nslookup-for-beginners.aspx) для подтверждения того, что внешний URL-адрес доступен, а домен *msapproxy.net* отображается как псевдоним.

Теперь приложение настроено для использования личного домена. Не забудьте назначить пользователей приложению перед его тестированием или выпуском. 

Чтобы изменить домен для приложения, выберите другой домен из раскрывающегося списка **Внешний URL-адрес** на странице **Прокси приложения**. При необходимости отправьте сертификат для обновленного домена и обновите его запись DNS. Если вы не видите личный домен в раскрывающемся списке **Внешний URL-адрес**, он может быть не проверен.

Более подробные инструкции по использованию прокси приложения см. в разделе [Руководство. Добавление локального приложения для удаленного доступа через прокси приложения в Azure Active Directory](application-proxy-add-on-premises-application.md).

## <a name="certificates-for-custom-domains"></a>Сертификаты для личных доменов

Сертификат создает безопасное TLS-подключение для личного домена. 

### <a name="certificate-formats"></a>Форматы сертификатов

Необходимо использовать PFX-сертификат, чтобы обеспечить включение всех необходимых промежуточных сертификатов. Сертификат должен включать закрытый ключ.

Поддерживаются наиболее распространенные методы подписи сертификатов, такие как альтернативное имя субъекта (SAN). 

Можно использовать шаблон сертификата, если он соответствует внешнему URL-адресу. Для [приложений с подстановочными знаками необходимо использовать шаблоны сертификатов](application-proxy-wildcard.md). Если вы хотите также использовать сертификат для доступа к поддоменам, необходимо добавить подстановочные знаки поддоменов в качестве альтернативных имен субъектов в том же сертификате. Например, сертификат для *\*.adventure-works.com* не будет работать для *\*.apps.adventure-works.com*, если не добавить *\*.apps.adventure-works.com* в качестве альтернативного имени субъекта. 

Вы можете использовать сертификаты, выданные собственной инфраструктурой открытых ключей (PKI), если цепочка сертификатов установлена на клиентских устройствах. Intune может развертывать эти сертификаты на управляемых устройствах. Для неуправляемых устройств необходимо вручную устанавливать эти сертификаты. 

Не рекомендуется использовать частный корневой ЦС, так как частный корневой ЦС также потребуется отправить на клиентские компьютеры, что может привести к многочисленным проблемам.

### <a name="certificate-management"></a>Управление сертификатами

Все управление сертификатами осуществляется через страницы отдельных приложений. Перейдите на страницу **Прокси приложения** для доступа к полю **Сертификат**.

После отправки сертификата для приложения он также будет автоматически применен к **новым** приложениям, которые используют тот же сертификат. Необходимо будет повторно отправить сертификат для существующих приложений в клиенте.

По истечении срока действия сертификата вы получите предупреждение о том, что необходимо передать другой сертификат. Если сертификат отозван, пользователи смогут увидеть предупреждение системы безопасности при получении доступа к приложению. Чтобы обновить сертификат для приложения, перейдите на страницу **Прокси приложения** для этого приложения, выберите **Сертификат** и отправьте новый сертификат. Если старый сертификат не используется другими приложениями, он будет автоматически удален. 

## <a name="next-steps"></a>Дальнейшие действия

* [Включите единый вход](application-proxy-configure-single-sign-on-with-kcd.md) в опубликованные приложения с помощью аутентификации Azure AD.
* [Условный доступ](../conditional-access/concept-conditional-access-cloud-apps.md) к опубликованным в облаке приложениям.