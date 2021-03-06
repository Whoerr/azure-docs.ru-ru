---
title: Диспетчер трафика Azure | Документация Майкрософт
description: В статье представлен обзор диспетчера трафика Azure. Узнайте, подходит ли она для балансировки нагрузки трафика пользователя для вашего приложения.
services: traffic-manager
author: duongau
ms.service: traffic-manager
customer intent: As an IT admin, I want to learn about Traffic Manager and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/19/2021
ms.author: duau
ms.openlocfilehash: 09b82eed5ad6a9ad121ca56d197eb9c003d027f5
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98624915"
---
# <a name="what-is-traffic-manager"></a>Что такое диспетчер трафика
Диспетчер трафика Azure — это балансировщик нагрузки трафика на основе DNS. Эта служба позволяет распределять трафик между общедоступными приложениями в глобальных регионах Azure. Диспетчер трафика также предоставляет общедоступные конечные точки с высоким уровнем доступности и быстрым реагированием.

Диспетчер трафика использует DNS для направления клиентских запросов к наиболее подходящей конечной точке службы в зависимости от метода маршрутизации трафика. Диспетчер трафика также обеспечивает мониторинг работоспособности для каждой конечной точки. Конечной точкой может быть любая служба с доступом в Интернет, размещенная в среде Azure или за ее пределами. Диспетчер трафика предоставляет целый ряд [методов маршрутизации трафика](traffic-manager-routing-methods.md) и [параметров мониторинга конечных точек](traffic-manager-monitoring.md), подходящих для приложений с различными потребностями и моделями автоматической отработки отказа. Диспетчер трафика устойчив к сбоям, включая сбой всего региона Azure.

>[!NOTE]
> Azure предоставляет набор полностью управляемых решений балансировки нагрузки для пользовательских сценариев. Если вам необходима обработка подключений по протоколу TLS (разгрузка SSL) или обработка прикладного уровня HTTP- или HTTPS-запросов, ознакомьтесь со статьей [Обзор шлюза приложений](../application-gateway/overview.md). Если вам нужна региональная балансировка нагрузки, ознакомьтесь с [Load Balancer](../load-balancer/load-balancer-overview.md). В комплексных сценариях может быть целесообразно объединить эти решения.
>
> Сравнение параметров балансировки нагрузки Azure см. в статье [Overview of load-balancing options in Azure](/azure/architecture/guide/technology-choices/load-balancing-overview) (Общие сведения о параметрах балансировки нагрузки в Azure).

В Диспетчере трафика предлагаются приведенные ниже функции.

## <a name="increase-application-availability"></a>Повышение уровня доступности приложения

Диспетчер трафика обеспечивает высокую доступность для критически важных приложений за счет мониторинга конечных точек и автоматической отработки отказа при выходе конечной точки из строя.
    
## <a name="improve-application-performance"></a>Улучшение производительности приложения

Платформа Azure позволяет размещать облачные службы и веб-сайты в распределенных по всему миру центрах данных. Диспетчер трафика может улучшить скорость реагирования вашего веб-сайта, направляя трафик на конечную точку с наименьшей задержкой.

## <a name="service-maintenance-without-downtime"></a>Обслуживание служб без простоя

Вы можете выполнять операции планового обслуживания приложений, не вызывая простоев всей системы. Во время обслуживания диспетчер трафика может направлять трафик к альтернативным конечным точкам.

## <a name="combine-hybrid-applications"></a>Объединение гибридных приложений

Диспетчер трафика поддерживает внешние конечные точки, не относящиеся к Azure, что позволяет использовать его в гибридных облачных и локальных развертываниях, включая сценарии [пакетной передачи данных в облако](https://azure.microsoft.com/overview/what-is-cloud-bursting/), миграции в облако и отработки отказа в облако.

## <a name="distribute-traffic-for-complex-deployments"></a>Распределение трафика сложных развертываний

Используя [вложенные профили диспетчера](traffic-manager-nested-profiles.md), можно объединить несколько методов маршрутизации трафика, чтобы создать комплексные и гибкие правила для масштабирования в соответствии с потребностями более крупных и сложных развертываний.

## <a name="pricing"></a>Цены

Сведения о ценах см. на [странице с ценами на диспетчер трафика](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как [создать профиль диспетчера трафика](./quickstart-create-traffic-manager-profile.md).
- Узнайте больше о том, [как работает диспетчер трафика](traffic-manager-how-it-works.md).
- Ознакомьтесь с [часто задаваемыми вопросами](traffic-manager-FAQs.md) о диспетчере трафика.