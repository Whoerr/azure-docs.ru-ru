---
title: Анализ непредвиденных расходов на Azure
description: Узнайте, как анализировать непредвиденные расходы на подписку Azure.
author: bandersmsft
ms.reviewer: adwise
ms.service: cost-management-billing
ms.subservice: cost-management
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: banders
ms.custom: contperf-fy21q1
ms.openlocfilehash: 6d5198f47b6089730ff345eb12ab522fc5ce58a1
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100575380"
---
# <a name="analyze-unexpected-charges"></a>Анализ непредвиденных расходов

Скорее всего, вы создали сложную инфраструктуру облачных ресурсов для своей организации. Многие типы ресурсов Azure могут иметь разные типы расходов. Ресурсы Azure могут принадлежать разным командам в организации и иметь разные типы моделей выставления счетов, которые применяются к различным ресурсам. Чтобы получить более полное представление о расходах, начните анализ с помощью одной или нескольких стратегий в разделах ниже.

## <a name="review-invoice-for-resource-responsible-for-charge"></a>Проверка счета на ресурс, за который взимается плата

Способ приобретения услуг Azure помогает определить методологию и средства, которые доступны при определении ресурса, связанного с оплатой. Чтобы определить, какая методология к вам применяется, сначала [определите тип предложения Azure](../costs/understand-cost-mgt-data.md#determine-your-offer-type). Затем укажите категорию клиента в списке [поддерживаемых предложений Azure](../costs/understand-cost-mgt-data.md#supported-microsoft-azure-offers).

В статьях ниже приведены подробные инструкции по проверке счета на основе типа клиента. В каждом приведенном ниже источнике содержатся инструкции по скачиванию CSV-файла со сведениями о потреблении и затратах за данный период выставления счетов:

- [процесс проверки счета с оплатой по мере использования](review-individual-bill.md#charges);
- [процесс проверки счета Соглашения Enterprise](review-enterprise-agreement-bill.md);
- [процесс проверки Клиентского соглашения Майкрософт](review-customer-agreement-bill.md#analyze-your-azure-usage-charges);
- [процесс проверки Соглашения с партнером Майкрософт](review-partner-agreement-bill.md#analyze-your-azure-usage-charges).

В счетах Azure вычисляются расходы за месяц на основе _единиц измерения_. Единица измерения используется для отслеживания потребления ресурсов на протяжении времени и для составления счета. При создании одного ресурса Azure, например виртуальной машины, вместе с ним создается один или несколько экземпляров счетчиков.

Отфильтруйте CSV-файл с данными об использовании на основе _MeterName_, как показано в счете, который нужно проанализировать, чтобы увидеть все элементы строки, применяющиеся к единице измерений. _InstanceID_ элемента строки соответствует фактическому ресурсу Azure, создавшему расходы.

Определив необходимый ресурс, вы можете воспользоваться анализом затрат в службе "Управление затратами Azure", чтобы проанализировать затраты, связанные с ресурсом. Дополнительные сведения об использовании анализа затрат см. в [этой статье](../costs/quick-acm-cost-analysis.md).

## <a name="review-invoiced-charges-in-cost-analysis"></a>Просмотр выставленных счетов в разделе анализа затрат

Чтобы просмотреть сведения о счете на портале Azure, перейдите в раздел анализа затрат для интересующего вас счета. Выберите представление **Сведения о счете**. Здесь сведения о расходах показаны так же, как в счете.

[![Пример сведений о счете](./media/analyze-unexpected-charges/invoice-details.png)](./media/analyze-unexpected-charges/invoice-details.png#lightbox)

Просматривая сведения о счете, можно определить, из-за какой службы возникли непредвиденные затраты и какие ресурсы непосредственно связанны с определенным ресурсом в анализе затрат. Например, если вы хотите проанализировать расходы на службу "Виртуальные машины", перейдите к представлению **Накопленные затраты**. Затем установите степень детализации **Ежедневно**, примените фильтр **Название службы: Виртуальные машины** и сгруппируйте расходы по **ресурсам**.

[![Пример накопленных затрат на виртуальные машины](./media/analyze-unexpected-charges/virtual-machines.png)](./media/analyze-unexpected-charges/virtual-machines.png#lightbox)

## <a name="identify-spikes-in-cost-over-time"></a>Определение максимальных расходов за период времени

Иногда вы можете не знать, какие последние затраты привели к изменениям оплаты счетов. Чтобы понять, что изменилось, можно воспользоваться анализом затрат, [просмотрев ежедневное или ежемесячное распределение затрат за период времени](../costs/cost-analysis-common-uses.md#view-costs-per-day-or-by-month). После создания представления сгруппируйте расходы по **службам** или **ресурсам**, чтобы найти изменения. Вы также можете отобразить данные в виде **графика**, чтобы лучше их визуализировать.

![Пример, демонстрирующий затраты за период времени в анализе затрат](./media/analyze-unexpected-charges/costs-over-time.png)

## <a name="determine-resource-pricing-and-billing-model"></a>Определение цен на ресурсы и модели выставления счетов

Один ресурс может увеличить плату за несколько продуктов и служб Azure. Дополнительные сведения о ценах на каждый продукт Azure см. на [этой странице](https://azure.microsoft.com/pricing/#product-pricing). Например, одна виртуальная машина, созданная в Azure, может иметь указанные ниже единицы измерения, созданные для отслеживания ее использования. У каждой из них могут быть разные цены.

- Часы вычислений
- Часы использования IP-адресов
- Входящий трафик передачи данных
- Исходящий трафик передачи данных
- Стандартный управляемый диск
- Операции с управляемыми дисками (цен. категория "Стандартный")
- Стандартные операции ввода-вывода на диске
- Стандартные операции ввода-вывода чтения блочных BLOB-объектов
- Стандартные операции ввода-вывода записи блочных BLOB-объектов
- Стандартные операции ввода-вывода удаления блочных BLOB-объектов

После создания виртуальной машины каждый счетчик начинает передавать данные об использовании. Эти данные и соответствующая стоимость отслеживаются в системе учета затрат Azure. В CSV-файле с данными об использовании вы видите те счетчики, которые использовались для составления счета.

## <a name="find-people-responsible-for-the-resource-and-engage"></a>Поиск ответственных за ресурс и их привлечение

Часто команда, ответственная за определенный ресурс, знает о его изменениях. Необходимо привлекать ответственных, когда вы определяете причину возникновения расходов. Например, ответственная рабочая группа могла недавно создать ресурс, обновить его номер SKU (таким образом изменив цену ресурса) или увеличить нагрузку на ресурс в связи с изменениями кода. Чтобы узнать о дополнительных методах определения владельца ресурса, ознакомьтесь со следующими разделами.

### <a name="analyze-the-audit-logs-for-the-resource"></a>Анализ журналов аудита ресурса

Если у вас есть разрешения на просмотр ресурса, вы сможете получить доступ к его журналам аудита. Проверьте журналы, чтобы найти пользователя, ответственного за последние изменения ресурса. Дополнительные сведения см. в статье [Просмотр и извлечение событий журнала действий Azure](../../azure-monitor/essentials/activity-log.md#view-the-activity-log).

### <a name="analyze-user-permissions-to-the-resources-parent-scope"></a>Анализ разрешений пользователя для родительской области ресурса

Пользователи с доступом на запись к подписке или группе ресурсов обычно имеют сведения о созданных ресурсах. Они должны объяснить назначение ресурса или направить вас к специалисту. Чтобы определить пользователей с разрешениями для области подписки, ознакомьтесь со статьей [Краткое руководство. Просмотр прав доступа пользователя к ресурсам Azure](../../role-based-access-control/check-access.md). Для групп ресурсов можно использовать аналогичную процедуру.

## <a name="get-help-to-identify-charges"></a>Получение справки для определения расходов

Если вы использовали предыдущие стратегии и по-прежнему не понимаете, почему у вас возникли расходы, или вам нужна другая помощь по вопросам выставления счетов, [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте о том, [как оптимизировать инвестиции в облако с помощью службы "Управление затратами Azure"](../costs/cost-mgt-best-practices.md).