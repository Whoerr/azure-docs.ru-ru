---
title: Создание веб-API, вызывающего веб-API | Службы
titleSuffix: Microsoft identity platform
description: Узнайте, как создать веб-API, который вызывает нисходящие веб-API (обзор).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: a66f0a2de1d8239baffbe53dfb5d6f2dd275d448
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2021
ms.locfileid: "98756338"
---
# <a name="scenario-a-web-api-that-calls-web-apis"></a>Сценарий: веб-API, который вызывает веб-API

Узнайте, что нужно знать для создания веб-API, который вызывает веб-API.

## <a name="prerequisites"></a>Предварительные требования

Этот сценарий, в котором защищенный веб-API вызывает другие веб-API, строится на [сценарии: защищенный веб-API](scenario-protected-web-api-overview.md).

## <a name="overview"></a>Обзор

- Клиентское, настольное, мобильное или одностраничное приложение (не представленное в сопровождающей схеме) вызывает защищенный веб-API и предоставляет маркер носителя JSON Web Token (JWT) в своем HTTP-заголовке "Authorization".
- Защищенный веб-API проверяет маркер и использует метод библиотеки проверки подлинности Майкрософт (MSAL) `AcquireTokenOnBehalfOf` для запроса другого маркера из Azure Active Directory (Azure AD), чтобы защищенный веб-API мог вызвать второй веб-API или нисходящий веб-API от имени пользователя.
- Защищенный веб-API также может вызываться `AcquireTokenSilent` позже для запроса маркеров для других нисходящих API от имени того же пользователя. `AcquireTokenSilent` обновляет токен при необходимости.

![Схема веб-API, вызывающего веб-API](media/scenarios/web-api.svg)

## <a name="specifics"></a>Особенности

Часть регистрации приложения, связанная с разрешениями API, является классической. Конфигурация приложения включает использование потока OAuth 2,0 от имени пользователя для обмена токеном носителя JWT с маркером для подчиненного API. Этот маркер добавляется в кэш маркеров, где он доступен на контроллерах веб-API, и затем может получить маркер без уведомления для вызова интерфейсов API нисходящей связи.

## <a name="next-steps"></a>Следующие шаги

Перейдите к следующей статье в этом сценарии — [Регистрация приложения](scenario-web-api-call-api-app-registration.md).
