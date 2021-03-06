---
title: Регистрация приложений управляющей программы, вызывающих веб-API — платформа Microsoft Identity | Службы
description: Узнайте, как создать управляющее приложение, вызывающее веб-API — регистрацию приложений
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/15/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 938a19276839d5e0d2bd3e0244510fc068cb029c
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "99582913"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Приложение управляющей программы, вызывающее веб-API — регистрация приложений

Для управляющего приложения это необходимо знать при регистрации приложения.

## <a name="supported-account-types"></a>Поддерживаемые типы учетных записей

Управляющие приложения имеют смысл только в клиентах Azure AD. Поэтому при создании приложения выберите один из следующих вариантов.

- **Учетные записи только в этом каталоге Организации**. Это наиболее распространенный вариант, поскольку управляющие приложения обычно пишутся бизнес-разработчиками.
- **Учетные записи в любом каталоге Организации**. Этот вариант можно выбрать, если вы являетесь независимым поставщиком программного обеспечения, предоставляющего служебную программу клиентам. Для утверждения вам потребуются администраторы клиента клиентов.

## <a name="authentication---no-reply-uri-needed"></a>Проверка подлинности-URI ответа не требуется

Если конфиденциальное клиентское приложение использует *только* поток учетных данных клиента, URI ответа не требуется регистрировать. Он не требуется для настройки или создания приложения. Поток учетных данных клиента не использует его.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>Разрешения API — разрешения приложения и согласие администратора

Управляющее приложение может запрашивать только разрешения приложения для API-интерфейсов (не делегированные разрешения). На странице **разрешения API** для регистрации приложения после выбора **Добавить разрешение** и выбрать семейство API, выберите **разрешения приложения**, а затем выберите нужные разрешения.

![Разрешения приложения и согласие администратора](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

> [!NOTE]
> Веб-API, который требуется вызвать, должен определять *разрешения приложения (роли приложения)*, а не делегированные разрешения. Дополнительные сведения о том, как предоставить такой API, см. в разделе [защищенный веб-API: регистрация приложения — при вызове веб-API с помощью управляющего приложения](scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app).

Для приложений управляющей программы требуется, чтобы администратор клиента предварительно предсогласия приложения, вызывающего веб-API. Администраторы клиента предоставляют согласие на ту же страницу **разрешений API** , выбрав пункт **предоставить согласие администратора для *нашей Организации*** .

Если вы являетесь независимым поставщиком программного обеспечения, создающим приложение с несколькими клиентами, то следует ознакомиться с разделом [Развертывание-вариант приложений управляющей программы для клиентов](scenario-daemon-production.md#deployment---multitenant-daemon-apps).

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Следующие шаги

Перейдите к следующей статье в этом сценарии: [Конфигурация кода приложения](./scenario-daemon-app-configuration.md).
