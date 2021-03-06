---
title: Отказоустойчивость сборок в инфраструктуре IAM с Azure Active Directory
description: Рекомендации для архитекторов и ИТ-администраторов, не создающих устойчивость к нарушениям инфраструктуры IAM.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64fe4b8c217ec46cbb6dd046339c3ac65eebb121
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98724683"
---
# <a name="build-resilience-in-your-identity-and-access-management-infrastructure"></a>Отказоустойчивость сборок в инфраструктуре управления удостоверениями и доступом

Azure Active Directory — это глобальная система управления идентификацией и доступом в облаке, которая предоставляет важные службы, такие как проверка подлинности и авторизация для ресурсов Организации. В этом документе приведены рекомендации по определению, сопоставлению и уменьшению риска нарушения работы служб проверки подлинности или авторизации для ресурсов, зависящих от Azure Active Directory (Azure AD). 

Набор документов предназначен для

* Архитекторы удостоверений

* Владельцы службы удостоверений

* Группы операций идентификации

См. также документацию по [разработчикам приложений](./resilience-app-development-overview.md) и [Azure AD B2Cным системам](resilience-b2c.md).

## <a name="what-is-resilience"></a>Что такое устойчивость?

В контексте инфраструктуры идентификации устойчивость — это возможность превращаться нарушения работы таких служб, как проверка подлинности и авторизация, или сбой других компонентов с минимальными последствиями для бизнеса, пользователей и операций. Влияние нарушений может быть серьезно, а устойчивость требует тщательного планирования.

## <a name="why-worry-about-disruption"></a>Зачем беспокоиться о нарушении работы?

Каждый вызов системы проверки подлинности подлежит прерыванию, если какой-либо компонент вызова завершается с ошибкой. Если проверка подлинности нарушается из-за сбоев базовых компонентов, пользователи не будут получать доступ к своим приложениям. Таким образом, уменьшение числа вызовов проверки подлинности и количества зависимостей в этих вызовах важно для обеспечения устойчивости. Разработчики приложений могут утверждать некоторый контроль над тем, как часто запрашиваются маркеры. Например, обратитесь к разработчикам, чтобы убедиться, что они используют управляемые удостоверения Azure AD для своих приложений везде, где это возможно. 

В системе аутентификации на основе маркеров, такой как Azure AD, пользовательское приложение (клиент) должно получить маркер безопасности от системы удостоверений, прежде чем он сможет получить доступ к приложению или другому ресурсу. В течение срока действия клиент может несколько раз представлять один и тот же токен для доступа к приложению.

По истечении срока действия маркера, представленного для приложения, приложение отклоняет маркер, и клиент должен получить новый маркер из Azure AD. Получение нового маркера потенциально требует взаимодействия с пользователем, например запросов учетных данных или других требований системы проверки подлинности. Уменьшение частоты вызовов проверки подлинности с долгосрочными маркерами сокращает ненужные взаимодействия. Однако необходимо сбалансировать жизненный цикл маркера с риском, созданным меньшим числом оценок политики. Дополнительные сведения об управлении жизненным циклом маркеров см. в этой статье, посвященной [оптимизации запросов на проверку подлинности](../authentication/concepts-azure-multi-factor-authentication-prompts-session-lifetime.md).

## <a name="ways-to-increase-resilience"></a>Способы повышения устойчивости
На следующей схеме показаны шесть конкретных способов повышения устойчивости. Каждый метод подробно описан в статьях, связанных в разделе дальнейшие действия этой статьи.
  
![Схема, показывающая обзор устойчивости администратора](./media/resilience-in-infrastructure/admin-resilience-overview.png)

## <a name="next-steps"></a>Следующие шаги
Ресурсы по устойчивости для администраторов и архитекторов
 
* [Устойчивость к сборке с помощью управления учетными данными](resilience-in-credentials.md)

* [Устойчивость сборки к состояниям устройств](resilience-with-device-states.md)

* [Устойчивость сборки с помощью оценки непрерывного доступа (автоматизированного конструирования)](resilience-with-continuous-access-evaluation.md)

* [Устойчивость к сборке при аутентификации внешних пользователей](resilience-b2b-authentication.md)

* [Устойчивость к сборке в гибридной проверке подлинности](resilience-in-hybrid.md)

* [Устойчивость к сборке в доступе к приложениям с помощью прокси приложения](resilience-on-premises-access.md)

Обеспечение устойчивости ресурсов для разработчиков

* [Создание устойчивости IAM в приложениях](resilience-app-development-overview.md)

* [Устойчивость сборок в CIAM Systems](resilience-b2c.md)