---
title: Вопросы и ответы об имеющихся агентах Log Analytics в центре безопасности Azure
description: Это часто задаваемые вопросы о клиентах, которые уже используют агент Log Analytics и рассмотрены центр безопасности Azure, продукт, который помогает предотвращать, обнаруживать угрозы и реагировать на них.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 1a52b2fec6155959a570f2438a59c14d9f79f368
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101701966"
---
# <a name="faq-for-customers-already-using-azure-monitor-logs"></a>Вопросы и ответы для клиентов, уже использующих журналы Azure Monitor<a name="existingloganalyticscust"></a>

## <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Переопределяет ли центр безопасности все установленные подключения между виртуальными машинами и рабочими областями?

Если на виртуальной машине уже установлен агент Log Analytics в качестве расширения Azure, центр безопасности не переопределит существующее подключение к рабочей области. Он использует существующую рабочую область. Виртуальная машина будет защищена при условии, что решение "безопасность" или "SecurityCenterFree" установлено в рабочей области, на которую он сообщает. 

Решение центра безопасности устанавливается в рабочей области, выбранной на экране сбора данных, если она еще не существует, а решение применяется только к соответствующим виртуальным машинам. Когда вы добавляете решение, оно по умолчанию автоматически развертывается на всех агентах Windows и Linux, подключенных к рабочей области Log Analytics. Функция [нацеливания решений](../azure-monitor/insights/solution-targeting.md) позволяет применять к решениям область действия.

> [!TIP]
> Если агент Log Analytics устанавливается непосредственно на виртуальной машине (а не в виде расширения Azure), центр безопасности не устанавливает агент Log Analytics, а мониторинг безопасности ограничен.

## <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Устанавливает ли центр безопасности решения в моих существующих рабочих областях Log Analytics? Как это влияет на выставление счетов?
Когда центр безопасности обнаруживает, что виртуальная машина уже подключена к созданной вами рабочей области, центр безопасности включает решения в этой рабочей области в соответствии с вашей конфигурацией цен. Решения применяются только к релевантным виртуальным машинам Azure с помощью функции [нацеливания решений](../azure-monitor/insights/solution-targeting.md). Поэтому счета выставляются без изменений.

- **Защитник Azure отключен** — центр безопасности устанавливает решение "SecurityCenterFree" в рабочей области. Плата не взимается.
- **Защитник Azure в** — центр безопасности устанавливает решение "безопасность" в рабочей области.

   ![Решения в рабочей области по умолчанию](./media/security-center-platform-migration-faq/solutions.png)

## <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>В моей среде уже есть рабочие области. Можно ли использовать их для сбора данных безопасности?
Если на виртуальной машине уже установлен агент Log Analytics в качестве расширения Azure, центр безопасности использует имеющуюся подключенную рабочую область. Центр безопасности устанавливается в рабочей области (если он еще не установлен), и решение применяется только к релевантным виртуальным машинам при помощи функции [нацеливания решений](../azure-monitor/insights/solution-targeting.md).

Когда центр безопасности устанавливает агент Log Analytics на виртуальных машинах, он использует рабочие области по умолчанию, созданные центром безопасности, если центр безопасности не указывает на существующую рабочую область.

## <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>В моих рабочих областях уже есть решение для обеспечения безопасности. Как это влияет на выставление счетов?
Решение аудита & безопасности используется для включения **защитника Azure для серверов**. Если решение "Безопасность и аудит" уже установлено в рабочей области, центр безопасности использует существующее решение. Счета будут выставляться без изменений.