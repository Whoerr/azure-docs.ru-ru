---
title: Сценарии автоматизации для выставления счетов и управления затратами в Azure
description: Узнайте, как соотносятся распространенные сценарии выставления счетов и управления затратами с разными API.
author: bandersmsft
ms.reviewer: adwise
tags: billing
ms.service: cost-management-billing
ms.subservice: common
ms.topic: reference
ms.date: 01/26/2021
ms.author: banders
ms.openlocfilehash: 12c13b8a65296fb0ee74e0ee0449b604facf2f48
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99051267"
---
# <a name="automation-scenarios-for-billing-and-cost-management"></a>Сценарии автоматизации для выставления счетов и управления затратами

В этой статье перечислены распространенные сценарии для выставления счетов и управления затратами Azure. Для сценариев указаны API, которые можно использовать. Под таблицей сопоставления сценариев с API можно найти сводку по этим API и функции, которые они предлагают.

## <a name="common-scenarios"></a>Распространенные сценарии

API-интерфейсы выставления счетов и управления затратами можно использовать в нескольких сценариях для решения вопросов, связанных с затратами и потреблением. Ниже приводится краткий обзор распространенных сценариев:

- **Сверка счетов**. Правильно ли корпорация Майкрософт определила сумму к оплате?  Сколько мне нужно платить и как мне проверить эти вычисления?

- **Взаимозачет**. Я знаю сумму моих расходов, а как их распределить между субъектами внутри организации?

- **Оптимизация затрат** Я знаю сумму своих расходов. Могу ли я более эффективно использовать средства, которые тратятся на Azure?

- **Отслеживание затрат**. Я хочу отслеживать суммы затрат и объемы потребления за разные периоды времени. Какие тенденции здесь можно заметить? Как можно это улучшить?

- **Затраты на Azure в течение месяца**. Каковы месячные затраты на сегодняшний день? Нужно ли скорректировать расходы и (или) потребление ресурсов Azure? В какой период месяца ресурсы Azure используются наиболее активно?

- **Оповещения**. Как настроить оповещения, основанные на объемах потребления ресурсов или суммах расходов в денежном выражении?

## <a name="scenario-to-api-mapping"></a>Сопоставление сценариев с интерфейсами API

|         API        | Сверка счетов    | Взаимозачет    | Оптимизация затрат    | Отслеживание расходов.    | Затраты за полмесяца    | видны узлы    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| сведения о бюджете;                     |                           |                  |           X          |                  |                    |     X     |
| сведения о расходах на Marketplace;                |             X             |         X        |           X          |         X        |          X         |     X     |
| Прейскурант                 |             X             |         X        |           X          |         X        |          X         |           |
| рекомендации по резервированию; |                           |                  |           X          |                  |                    |           |
| Сведения о резервировании         |                           |                  |           X          |         X        |                    |           |
| Сводные данные о резервировании       |                           |                  |           X          |         X        |                    |           |
| сведения о потреблении;               |             X             |         X        |           X          |         X        |          X         |     X     |
| Расчетный период             |             X             |         X        |           X          |         X        |                    |           |
| Счета                    |             X             |         X        |           X          |         X        |                    |           |
| Розничные цены Azure                    |             X             |                  |           X          |         X        |                    |           |


> [!NOTE]
> Приведенная ниже таблица "Сопоставления сценариев и API" не включает API-интерфейсы потребления для организаций. Используйте общие API потребления для новых сценариев разработки, когда это возможно.

## <a name="api-summaries"></a>Сводные данные по API

### <a name="consumption"></a>Потребление
Клиенты Web Direct и Enterprise могут использовать все следующие API, кроме тех, где явно указано иначе:

-    [API бюджетов](/rest/api/consumption/budgets) (*только для клиентов Enterprise*). Их можно использовать, чтобы создавать бюджеты затрат или потребления ресурсов, групп ресурсов или показателей для выставления счетов. Когда вы создали бюджет, вы можете настроить оповещения, чтобы получать уведомления о превышении определенных пороговых значений. Можно также настроить действия, которые будут выполняться при достижении пороговых значений.

-    [API расходов на Marketplace](/rest/api/consumption/marketplaces). С их помощью можно получать данные о затратах и использовании для всех ресурсов Azure Marketplace (предложения партнеров Azure). Эти сведения позволяют оценить совокупные расходы по всем ресурсам Marketplace или изучить показатели затрат и потребления для конкретных ресурсов.

-    [API прейскурантов](/rest/api/consumption/pricesheet) (*только для клиентов Enterprise*). Позволяет получать пользовательские сведения о ценах для всех счетчиков. Организации могут объединять эти данные с информацией о потреблении для собственных ресурсов и Marketplace, чтобы с их помощью рассчитывать затраты.

-    [API рекомендаций по резервированию](/rest/api/consumption/reservationrecommendations). Этот API позволяет получать рекомендации по приобретению зарезервированных экземпляров виртуальных машин. Эти рекомендации помогают пользователям оценить ожидаемую экономию и объемы закупок. Дополнительные сведения см. в статье [API-интерфейсы для автоматизации резервирования Azure](../reservations/reservation-apis.md).

-    [API сведений о резервировании](/rest/api/consumption/reservationsdetails). С помощью этих API вы можете просмотреть сведения о приобретенных ранее зарезервированных виртуальных машинах, например результаты сравнения зарезервированных и фактических объемов потребления. Эти данные можно отобразить с детализацией до конкретной виртуальной машины. Дополнительные сведения см. в статье [API-интерфейсы для автоматизации резервирования Azure](../reservations/reservation-apis.md).

-    [API сводок по резервированию](/rest/api/consumption/reservationssummaries). Используйте эти API для просмотра сводных сведений о приобретении вашей организацией зарезервированных виртуальных машин, например, чтобы получить результаты сравнения зарезервированных и использованных ресурсов. Дополнительные сведения см. в статье [API-интерфейсы для автоматизации резервирования Azure](../reservations/reservation-apis.md).

-    [API сведений о потреблении](/rest/api/consumption/usagedetails). Позволяет получить подробные сведения об оплате и использовании всех ресурсов Azure от корпорации Майкрософт. Эти данные предоставляются в форме записей с информацией о потреблении, которые в настоящее время создаются ежедневно для каждой средства измерения. На основе этих сведений можно оценить совокупные расходы по всем ресурсам или изучить показатели затрат и потребления для конкретных ресурсов.

-    [Розничные цены Azure.](/rest/api/cost-management/retail-prices/azure-retail-prices) Сведения о тарифах по разным показателям для метода оплаты по мере использования. Полученные данные в сочетании со сведениями о потреблении ресурсов позволят вручную вычислить ожидаемые расходы.

### <a name="billing"></a>Выставление счетов
-    [API расчетных периодов](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods). Используйте этот API, чтобы определить период выставления счетов, который нужно проанализировать, а также идентификаторы счетов за этот период. Идентификаторы счетов можно использовать с API счетов.

-    [API счетов](/rest/api/billing/2019-10-01-preview/invoices). Используйте этот API, чтобы получить URL-адрес, по которому можно скачать отчет в формате PDF за указанный расчетный период.

### <a name="enterprise-consumption"></a>Потребление организации
Следующие API предназначены только для клиентов Enterprise:

-    [API сводной информации о балансе](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary). Этот API возвращает ежемесячную сводку о состоянии баланса, новых покупках, расходах на службу Azure Marketplace, корректировках и плате за превышение. Эту информацию можно получить для текущего расчетного периода или за любой период в прошлом. Предприятия могут использовать эти данные для сравнения с суммарными расходами, вычисленными вручную. Этот API не предоставляет сведений по конкретным ресурсам или статистического представления затрат.

-    [API сведений о потреблении](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail). Используйте этот API, чтобы получить подробные сведения о потреблении собственных служб Azure (предложения корпорации Майкрософт) за текущий месяц, за определенный расчетный период или за произвольный период. Предприятия могут использовать эти данные, чтобы вручную рассчитать счета на основе тарифа и потребления. Предприятия также могут использовать сведения о подразделениях и организациях для распределения затрат между ними. Из этих данных можно узнать о потреблении и затратах с учетом специфики ресурсов.

-    [API затрат на Marketplace Store](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge). Используйте этот API, чтобы получить подробные сведения о потреблении служб Azure (предложения партнеров) за текущий месяц, за определенный расчетный период или за произвольный период. Предприятия могут использовать эти данные, чтобы вручную рассчитать счета на основе тарифа и потребления. Предприятия также могут использовать сведения о подразделениях и организациях для распределения затрат между ними. С помощью этого API можно узнать о потреблении и затратах с учетом специфики ресурсов.

-    [API прейскурантов](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet). Этот API предоставляет применимый тариф для каждой метрики в отдельной регистрации и за определенный расчетный период. Эти сведения о ценах в сочетании с данными о потреблении и использовании marketplace позволяют вручную рассчитать ожидаемые расходы.

-    [API расчетных периодов](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods). Этот API возвращает список расчетных периодов, а также предоставляет свойство, которое указывает на маршрут API для четырех наборов данных Enterprise API, относящихся к расчетному периоду: BalanceSummary, UsageDetails, Marketplace Charges и PriceSheet.

-    [API рекомендаций по резервированию экземпляров](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation). Этот API возвращает сведения об использовании клиентом виртуальных машин за 7, 30 или 60 дней и предлагает на их основе рекомендации по отдельному и общему резервированию. Использование этого API позволяет клиентам оценить ожидаемую экономию и рекомендуемые объемы закупок. Дополнительные сведения см. в статье [API-интерфейсы для автоматизации резервирования Azure](../reservations/reservation-apis.md).

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

### <a name="whats-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Каковы отличия между API для корпоративной отчетности и API потребления? Когда их нужно использовать?
Эти API имеют аналогичный набор функций и могут ответить на тот же широкий набор вопросов в сфере выставления счетов и управления затратами. Но они ориентированы на разных пользователей:

- Эти API доступны для клиентов, подписавших Соглашение Enterprise с корпорацией Майкрософт, которое предоставляет им доступ к согласованной предоплате Azure (прежнее название — денежное обязательство). и индивидуальным ценам. API требуется использовать ключ, который можно получить через [Enterprise Portal](https://ea.azure.com). Описание этих API см. в статье [Обзор API-интерфейсов отчетов для корпоративных клиентов](enterprise-api.md).

- Эти API доступны для всех клиентов, за некоторыми исключениями. Дополнительные сведения см. в статье [Общие сведения об API потребления ресурсов Azure](consumption-api-overview.md) и в [справочник по API потребления](/rest/api/consumption/). Предоставленные API являются рекомендуемым решением для последних сценариев разработки.

### <a name="whats-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Каковы отличия между API счетов и API сведений о потреблении?
Эти API обеспечивают различное представление тех же данных.

- [API счетов](/rest/api/billing/2019-10-01-preview/invoices) предназначен только для клиентов Web Direct. Он предоставляет ежемесячный сводный счет на основе совокупных затрат для каждого типа показателя.

- [API сведений об использовании](/rest/api/consumption/usagedetails) предоставляет подробное представление записей об использовании или затратах за каждый день. Его могут использовать как клиенты Enterprise, так и клиенты Web Direct.

### <a name="whats-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Каковы различия между API прейскурантов и API RateCard?
Эти API предоставляют похожие наборы данных, но имеют разные аудитории.

- [API прейскурантов](/rest/api/consumption/pricesheet) предоставляет пользовательские цены, которые были согласованы для корпоративного клиента.

- [API розничных цен Azure](/rest/api/cost-management/retail-prices/azure-retail-prices) предоставляет общедоступные цены для метода оплаты по мере использования, которые применяются для клиентов Web Direct.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения об использовании REST API для получения данных о ценах на другие службы Azure см. в статье [Общие сведения о розничных ценах Azure](/rest/api/cost-management/retail-prices/azure-retail-prices).

- Чтобы сравнить счет и файл с подробными данными о ежедневном использовании, а также отчеты об управлении затратами на портале Azure, ознакомьтесь со статьей [Расшифровка счета за использование Microsoft Azure](../understand/review-individual-bill.md).

- Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).