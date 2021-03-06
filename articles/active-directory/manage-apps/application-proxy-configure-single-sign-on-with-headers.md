---
title: Единый вход на основе заголовка для локальных приложений с Azure AD App прокси-сервером
description: Узнайте, как обеспечить единый вход для локальных приложений, защищенных с помощью проверки подлинности на основе заголовков.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/05/2020
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 512316b78a0d6422daf5e268ef30db72ccbcfaeb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101688320"
---
# <a name="header-based-single-sign-on-for-on-premises-apps-with-azure-ad-app-proxy"></a>Единый вход на основе заголовка для локальных приложений с Azure AD App прокси-сервером

Azure Active Directory прокси приложения (Azure AD) изначально поддерживает единый вход для приложений, использующих заголовки для проверки подлинности. Вы можете настроить значения заголовков, необходимые для приложения в Azure AD. Значения заголовков будут отправляться в приложение через прокси приложения. Некоторые преимущества использования встроенной поддержки проверки подлинности на основе заголовка с помощью прокси приложения включают:  

* **Упрощение предоставления удаленного доступа к локальным приложениям** — прокси приложения позволяет упростить существующую архитектуру удаленного доступа. Вы можете заменить VPN-доступ к этим приложениям. Также можно удалить зависимости от локальных решений идентификации для проверки подлинности. Пользователи не заметят никаких различий при входе в систему для использования корпоративных приложений. Они по-прежнему смогут работать в любом расположении и на любом устройстве.  

* **Нет дополнительного программного обеспечения или изменений в приложениях** . Вы можете использовать существующие соединители прокси приложения и не требуется устанавливать дополнительное программное обеспечение.  

* **Широкий список доступных атрибутов и преобразований** . все доступные значения заголовков основаны на стандартных утверждениях, выданных Azure AD. Все атрибуты и преобразования, доступные для [настройки утверждений для приложений SAML или OIDC](../develop/active-directory-saml-claims-customization.md#attributes) , также доступны для использования в качестве значений заголовков. 

## <a name="pre-requisites"></a>Предварительные требования
Прежде чем приступить к работе с единым входом для приложений проверки подлинности на основе заголовков, убедитесь, что среда готова со следующими параметрами и конфигурациями:
- Необходимо включить прокси приложения и установить соединитель, который содержит строку сайта для приложений. Ознакомьтесь с руководством [Добавление локального приложения для удаленного доступа через прокси приложения](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad) , чтобы узнать, как подготовить локальную среду, установить и зарегистрировать соединитель, а также проверить соединитель. 

## <a name="supported-capabilities"></a>Поддерживаемые возможности

В следующей таблице перечислены общие возможности, необходимые для приложений проверки подлинности на основе заголовков, которые поддерживаются прокси приложения. 

|Требование   |Описание|
|----------|-----------|
|Федеративный единый вход |В режиме предварительной проверки подлинности все приложения защищены проверкой подлинности Azure AD и позволяют пользователям использовать единый вход. |
|Удаленный доступ. |Прокси приложения обеспечивает удаленный доступ к приложению. Пользователи могут получать доступ к приложению из Интернета в любом браузере, используя внешний URL-адрес. Прокси приложения не предназначен для использования корпоративным доступом. |
|Интеграция на основе заголовков |Прокси приложения выполняет интеграцию единого входа с Azure AD, а затем передает удостоверение или другие данные приложения в качестве HTTP-заголовков в приложение. |
|Авторизация приложения |Общие политики можно указать в зависимости от приложения, к которому осуществляется доступ, членства пользователя в группах и других политик. В Azure AD политики реализуются с помощью [условного доступа](../conditional-access/overview.md). Политики авторизации приложений применяются только к первоначальному запросу проверки подлинности. |
|Поэтапная проверка подлинности |Политики можно определить, чтобы принудительно добавить проверку подлинности, например для получения доступа к конфиденциальным ресурсам. |
|Детализированная авторизация |Предоставляет контроль доступа на уровне URL-адреса. Добавленные политики могут быть применены на основе URL-адреса, к которому осуществляется доступ. Внутренний URL-адрес, настроенный для приложения, определяет область приложения, к которой применяется политика. Политика, настроенная для наиболее детализированного пути, применяется принудительно.  |

> [!NOTE] 
> В этой статье описывается подключение приложений проверки подлинности на основе заголовков к Azure AD с помощью прокси приложения и рекомендуемый шаблон. В качестве альтернативы также существует шаблон интеграции, который использует PingAccess с Azure AD для включения проверки подлинности на основе заголовков. Дополнительные сведения см. в статье [Проверка подлинности на основе заголовка для единого входа с помощью прокси приложения и PingAccess](application-proxy-ping-access-publishing-guide.md).

## <a name="how-it-works"></a>Принципы работы

:::image type="content" source="./media/application-proxy-configure-single-sign-on-with-headers/how-it-works.png" alt-text="Как единый вход на основе заголовков работает с прокси приложения." lightbox="./media/application-proxy-configure-single-sign-on-with-headers/how-it-works.png":::

1. Администратор настраивает сопоставления атрибутов, необходимые для приложения на портале Azure AD. 
2. Когда пользователь обращается к приложению, прокси приложения проверяет подлинность пользователя в Azure AD. 
3. Облачная служба прокси приложения осведомлена о необходимых атрибутах. Поэтому служба извлекает соответствующие утверждения из маркера идентификации, полученного во время проверки подлинности. Затем служба преобразует значения в необходимые заголовки HTTP в качестве части запроса к соединителю. 
4. Затем запрос передается в соединитель, который затем передается в серверное приложение. 
5. Приложение получает заголовки и может использовать эти заголовки по мере необходимости. 

## <a name="publish-the-application-with-application-proxy"></a>Публикация приложения с помощью прокси приложения

1. Опубликуйте приложение в соответствии с инструкциями, описанными в статье [Публикация приложений с помощью прокси приложения](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).  
    - Значение внутреннего URL-адреса определяет область действия приложения. При настройке внутреннего URL-адреса по корневому пути приложения все вложенные пути, расположенные под корнем, получат ту же конфигурацию заголовка и другую конфигурацию приложения. 
    - Создайте новое приложение, чтобы задать другую конфигурацию заголовка или назначение пользователя для более детального пути, чем настроенное вами приложение. В новом приложении настройте внутренний URL-адрес с конкретным требуемым путем, а затем настройте определенные заголовки, необходимые для этого URL-адреса. Прокси приложения всегда будет соответствовать параметрам конфигурации для наиболее детализированного пути, установленного для приложения. 

2. Выберите **Azure Active Directory** в   качестве **метода предварительной проверки подлинности**. 
3. Назначьте тестового пользователя, перейдя к **пользователям и группам** и назначив соответствующих пользователей и группы. 
4. Откройте браузер и перейдите по **внешнему URL-адресу**   из параметров прокси приложения. 
5. Убедитесь, что вы можете подключиться к приложению. Несмотря на то, что вы можете подключиться, вы не сможете получить доступ к приложению, так как заголовки не настроены. 

## <a name="configure-single-sign-on"></a>Настройка единого входа 
Прежде чем приступить к работе с единым входом для приложений на основе заголовков, необходимо уже установить соединитель прокси приложения, и соединитель сможет получить доступ к целевым приложениям. В противном случае выполните действия, описанные в [руководстве по Azure AD application proxy](application-proxy-add-on-premises-application.md),   а затем вернитесь сюда. 

1. После того как приложение появится в списке корпоративных приложений, выберите его и щелкните **единый вход**. 
2. Установите режим единого входа на **основе заголовков**. 
3. В поле Базовая конфигурация в качестве значения по умолчанию будет выбрано **Azure Active Directory**. 
4. Щелкните значок редактирования в заголовках, чтобы настроить заголовки для отправки в приложение. 
5. Выберите **Добавить новый заголовок**. Укажите **имя** для заголовка и выберите либо **атрибут** , либо **Преобразование** , а затем выберите из раскрывающегося списка заголовок, который требуется приложению.  
    - Дополнительные сведения о списке доступных атрибутов см. в разделе [настройки утверждений — атрибуты](../develop/active-directory-saml-claims-customization.md#attributes). 
    - Дополнительные сведения о списке доступных преобразований см. в разделе [настройки утверждений — преобразования утверждений](../develop/active-directory-saml-claims-customization.md#claim-transformations). 
    - Вы также можете добавить **заголовок группы**, чтобы отправить все группы, в которые входит пользователь, или группы, назначенные приложению в качестве заголовка. Дополнительные сведения о настройке групп в качестве значения см. в разделе [Configure Group Claims for Applications](../hybrid/how-to-connect-fed-group-claims.md#add-group-claims-to-tokens-for-saml-applications-using-sso-configuration). 
6. Щелкните "Сохранить". 

## <a name="test-your-app"></a>Тестирование приложения 

После выполнения всех этих действий приложение должно работать и быть доступным. Чтобы протестировать приложение, сделайте следующее: 
1. Откройте новый браузер или частное окно браузера, чтобы убедиться, что ранее кэшированные заголовки удалены. Затем перейдите по **внешнему URL-адресу**   из параметров прокси приложения.
2. Войдите с помощью тестовой учетной записи, назначенной приложению. Если вы можете загрузить приложение и войти в него с помощью единого входа, то это хорошо! 

## <a name="considerations"></a>Рекомендации

- Прокси приложения используется для предоставления удаленного доступа к приложениям локально или в частном облаке. Не рекомендуется использовать прокси приложения для управления трафиком, внутренним из корпоративной сети.
- Доступ к приложениям проверки подлинности на основе заголовков должен быть ограничен только трафиком из соединителя или другим разрешенным решением проверки подлинности на основе заголовков. Это обычно делается путем ограничения сетевого доступа к приложению с помощью брандмауэра или IP-адреса на сервере приложений.

## <a name="next-steps"></a>Дальнейшие действия

- [Сведения о едином входе](what-is-single-sign-on.md)
- [Что такое прокси приложения?](what-is-application-proxy.md)
- [Серия кратких руководств по управлению приложениями](view-applications-portal.md)
