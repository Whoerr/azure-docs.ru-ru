---
title: Создание устойчивых удостоверений и управление доступом с помощью Azure Active Directory
description: Руководством для архитекторов, ИТ-администраторов и разработчиков, создающих устойчивость к нарушениям систем идентификации.
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
ms.openlocfilehash: 39021fecb79f6b8e7536daad592378ed998760a7
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96454353"
---
# <a name="building-resilience-into-identity-and-access-management-with-azure-active-directory"></a>Создание устойчивости к управлению удостоверениями и доступом с помощью Azure Active Directory

Управление удостоверениями и доступом (IAM) — это платформа процессов, политик и технологий, которые упрощают управление удостоверениями и то, к чему они обращаются. Он включает в себя множество компонентов, поддерживающих проверку подлинности и авторизацию пользователя и других учетных записей в системе.

Устойчивость IAM — это возможность превращаться нарушения системных компонентов и восстановления с минимальными последствиями для бизнеса, пользователей, клиентов и операций. Уменьшение зависимостей, сложности и отдельных точек отказа, в то же время обеспечивая комплексную обработку ошибок, повысит устойчивость.

Перерывы могут поступать из любого компонента системы IAM. Чтобы создать устойчивую систему IAM, следует предположить, что произойдет прерывание и спланировать его. 

При планировании устойчивости решения IAM учитывайте следующие элементы. 

* Приложения, использующие систему IAM.

* Общедоступные инфраструктуры, используемые при проверке подлинности, включают в себя коммуникационные компании, поставщики услуг Интернета и поставщики открытых ключей.

* Облачные и локальные поставщики удостоверений.

* Другие службы, использующие IAM, и интерфейсы API, которые их соединяют.

* Любые другие локальные компоненты в системе.

Все, что важно, является источником, распознаванием и планированием непредвиденных обстоятельств. Однако добавление дополнительных систем идентификации, а также их результирующих зависимостей и сложности может снизить устойчивость, а не повышать ее.

Чтобы создать более устойчивость в системах, ознакомьтесь со следующими статьями:

* [Устойчивость к сборке в IAM-инфраструктуре](resilience-in-infrastructure.md)

* [Создание устойчивости IAM в приложениях](resilience-app-development-overview.md)

* [Отказоустойчивость сборок в системах управления удостоверениями и доступом клиентов (CIAM)](resilience-b2c.md)
