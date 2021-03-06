---
title: Обзор агентов мониторинга Azure | Документация Майкрософт
description: В этой статье представлен подробный обзор доступных агентов Azure, которые поддерживают мониторинг виртуальных машин, размещенных в Azure или гибридной среде.
services: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/12/2021
ms.openlocfilehash: af18356ef42f8796b972626da4567aac68a6de5a
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101719990"
---
# <a name="overview-of-azure-monitor-agents"></a>Общие сведения об агентах Azure Monitor

Виртуальным машинам и другим ресурсам для вычислений требуется, чтобы агент собирал данные мониторинга, необходимые для измерения производительности и доступности своей гостевой операционной системы и рабочих нагрузок. В этой статье описываются агенты, используемые Azure Monitor, и помогает определить, что необходимо удовлетворить требованиям конкретной среды.

> [!NOTE]
> В данный момент Azure Monitor несколько агентов из-за последней консолидации Azure Monitor и Log Analytics. Хотя в их функциях может быть перекрываться, каждая из них имеет уникальные возможности. В зависимости от требований на компьютерах может потребоваться один или несколько агентов. 

У вас может быть определенный набор требований, который не может быть полностью удовлетворен одним агентом для конкретного компьютера. Например, можно использовать оповещения метрик, для которых требуется расширение системы диагностики Azure, но также необходимо использовать функциональность VM Insights, для которой требуется агент Log Analytics и агент зависимостей. В таких случаях можно использовать несколько агентов, и это распространенный сценарий для клиентов, которым требуются функции каждого из них.

## <a name="summary-of-agents"></a>Сводка по агентам

В следующих таблицах представлено краткое сравнение агентов Azure Monitor для Windows и Linux. Дополнительные сведения о каждом из них приведены в разделе ниже.

> [!NOTE]
> В настоящее время агент Azure Monitor находится в режиме предварительной версии с ограниченными возможностями. Эта таблица будет обновлена 

### <a name="windows-agents"></a>Агенты Windows

| | Агент Azure Monitor (предварительная версия) | Диагностика<br>расширение (WAD) | Log Analytics<br>агент | Зависимость<br>агент |
|:---|:---|:---|:---|:---|
| **Поддерживаемые среды** | Azure<br>Другое облако (Дуга Azure)<br>Локально (Дуга Azure)  | Azure | Azure<br>Другое облако<br>В локальной среде | Azure<br>Другое облако<br>В локальной среде | 
| **Требования к агентам**  | Нет | Нет | Нет | Требуется агент Log Analytics |
| **Собираемые данные** | журналы событий;<br>Производительность | журналы событий;<br>ETW-события<br>Производительность<br>Файловые журналы<br>Журналы IIS<br>Журналы приложений .NET<br>Аварийные дампы<br>Журналы диагностики агента | журналы событий;<br>Производительность<br>Файловые журналы<br>Журналы IIS<br>Полезные сведения и решения<br>Другие службы | зависимости процессов.<br>Метрики сетевого подключения |
| **Отправленные данные** | Журналы Azure Monitor<br>Метрики Azure Monitor | Хранилище Azure<br>Метрики Azure Monitor<br>Концентратор событий | Журналы Azure Monitor | Журналы Azure Monitor<br>(с помощью агента Log Analytics) |
| **Службы и**<br>**features**<br>**поддерживается** | Log Analytics<br>Обозреватель метрик | Обозреватель метрик | Аналитика ВМ<br>Log Analytics<br>Служба автоматизации Azure<br>Центр безопасности Azure<br>Azure Sentinel | Аналитика ВМ<br>Сопоставление служб |

### <a name="linux-agents"></a>Агенты Linux

| | Агент Azure Monitor (предварительная версия) | Диагностика<br>расширение (LAD) | Telegraf<br>агент | Log Analytics<br>агент | Зависимость<br>агент |
|:---|:---|:---|:---|:---|:---|
| **Поддерживаемые среды** | Azure<br>Другое облако (Дуга Azure)<br>Локально (Дуга Azure) | Azure | Azure<br>Другое облако<br>В локальной среде | Azure<br>Другое облако<br>В локальной среде | Azure<br>Другое облако<br>В локальной среде |
| **Требования к агентам**  | Нет | Нет | Нет | Нет | Требуется агент Log Analytics |
| **Собираемые данные** | Системный журнал<br>Производительность | Системный журнал<br>Производительность | Производительность | Системный журнал<br>Производительность| зависимости процессов.<br>Метрики сетевого подключения |
| **Отправленные данные** | Журналы Azure Monitor<br>Метрики Azure Monitor | Хранилище Azure<br>Концентратор событий | Метрики Azure Monitor | Журналы Azure Monitor | Журналы Azure Monitor<br>(с помощью агента Log Analytics) |
| **Службы и**<br>**features**<br>**поддерживается** | Log Analytics<br>Обозреватель метрик | | Обозреватель метрик | Аналитика ВМ<br>Log Analytics<br>Служба автоматизации Azure<br>Центр безопасности Azure<br>Azure Sentinel | Аналитика ВМ<br>Сопоставление служб |


## <a name="azure-monitor-agent-preview"></a>Агент Azure Monitor (предварительная версия)

Сейчас [агент Azure Monitor](azure-monitor-agent-overview.md) находится на этапе предварительной версии и заменит агент log Analytics и агент Telegraf для компьютеров Windows и Linux. Он может передавать данные в журналы Azure Monitor и Azure Monitor метрики и использует [правила сбора данных (ДКР)](data-collection-rule-overview.md) , которые предоставляют более масштабируемый метод настройки сбора и назначения данных для каждого агента.

Используйте агент Azure Monitor, если необходимо:

- Собирайте журналы и метрики гостей с любого компьютера в Azure, в других облаках или в локальной среде. ([Серверы, поддерживающие дугу Azure](../../azure-arc/servers/overview.md) , необходимы для компьютеров за пределами Azure.) 
- Отправка данных в журналы Azure Monitor и Azure Monitor метрики для анализа с помощью Azure Monitor. 
- Отправка данных в службу хранилища Azure для архивирования.
- Отправка данных сторонним средствам с помощью [концентраторов событий Azure](./diagnostics-extension-stream-event-hubs.md).
- Управляйте безопасностью компьютеров с помощью [центра безопасности Azure](../../security-center/security-center-introduction.md)  или [Azure Sentinel](../../sentinel/overview.md). (Недоступно в предварительной версии.)

К ограничениям агента Azure Monitor относятся:

- В настоящее время общедоступная Предварительная версия. Ознакомьтесь с [текущими ограничениями](azure-monitor-agent-overview.md#current-limitations) для списка ограничений во время общедоступной предварительной версии.

## <a name="log-analytics-agent"></a>Агент Log Analytics

[Агент log Analytics](./log-analytics-agent.md) собирает данные мониторинга из операционной системы на виртуальной машине и рабочих нагрузок виртуальных машин в Azure, других поставщиках облачных служб и локальных компьютерах. Данные отправляются в рабочую область Log Analytics. Агент Log Analytics — это тот же агент, который используется System Center Operations Manager. Вы можете использовать компьютеры агента для многодомашнего подключения к группе управления и Azure Monitor одновременно. Этот агент также необходим для определенных аналитических сведений в Azure Monitor и других службах в Azure.

> [!NOTE]
> Агент Log Analytics для Windows часто называется Microsoft Monitoring Agent (MMA). Агент Log Analytics для Linux часто называется агентом OMS.

Используйте агент Log Analytics, если необходимо:

* Собирайте журналы и данные о производительности из виртуальных машин Azure или гибридных компьютеров, размещенных за пределами Azure.
* Отправляйте данные в рабочую область Log Analytics, чтобы воспользоваться преимуществами функций, поддерживаемых [Azure Monitor журналами](../logs/data-platform-logs.md) , такими как [запросы журналов](../logs/log-query-overview.md).
* Используйте [Application Insights](../vm/vminsights-overview.md) , которое позволяет отслеживать масштаб виртуальных машин и отслеживает их процессы и зависимости от других ресурсов и внешних процессов.  
* Управляйте безопасностью компьютеров с помощью [центра безопасности Azure](../../security-center/security-center-introduction.md)  или [Azure Sentinel](../../sentinel/overview.md).
* Используйте [Управление обновлениями службы автоматизации Azure](../../automation/update-management/overview.md), [настройку состояния службы автоматизации Azure](../../automation/automation-dsc-overview.md)или [Отслеживание изменений службы автоматизации Azure и инвентаризацию](../../automation/change-tracking/overview.md) , чтобы обеспечить комплексное управление компьютерами Azure и не на основе Azure.
* Используйте различные [решения](../monitor-reference.md#insights-and-core-solutions) для мониторинга определенной службы или приложения.

К ограничениям агента Log Analytics относятся:

- Не удается отправить данные в метрики Azure Monitor, службу хранилища Azure или концентраторы событий Azure.
- Трудно настроить уникальные определения мониторинга для отдельных агентов.
- Сложно управлять в масштабе, так как каждая виртуальная машина имеет уникальную конфигурацию.

## <a name="azure-diagnostics-extension"></a>Расширение системы диагностики Azure

[Расширение система диагностики Azure](./diagnostics-extension-overview.md) собирает данные мониторинга из операционной системы на виртуальной машине и рабочих нагрузок виртуальных машин Azure и других ресурсов вычислений. В основном он собирает данные в службу хранилища Azure, но также позволяет определять приемники данных для отправки данных в другие назначения, такие как метрики Azure Monitor и концентраторы событий Azure.

Используйте расширение диагностики Azure, если вам нужно:

- Отправка данных в службу хранилища Azure для архивирования или анализа с помощью таких средств, как [Обозреватель службы хранилища Azure](../../vs-azure-tools-storage-manage-with-storage-explorer.md).
- Отправляйте данные в [метрики Azure Monitor](../essentials/data-platform-metrics.md) , чтобы проанализировать ее с помощью [обозревателя метрик](../essentials/metrics-getting-started.md) и воспользоваться такими функциями, как оповещения на основе [метрик](../alerts/alerts-metric-overview.md) практически в реальном времени и [Автомасштабирование](../autoscale/autoscale-overview.md) (только для Windows).
- Отправка данных сторонним средствам с помощью [концентраторов событий Azure](./diagnostics-extension-stream-event-hubs.md).
- Собирайте [диагностику загрузки](../../virtual-machines/troubleshooting/boot-diagnostics.md) для изучения проблем загрузки виртуальной машины.

К ограничениям расширения системы диагностики Azure относятся:

- Можно использовать только с ресурсами Azure.
- Ограниченная возможность отправки данных в журналы Azure Monitor.

## <a name="telegraf-agent"></a>Агент Telegraf

[Агент Инфлуксдата Telegraf](../essentials/collect-custom-metrics-linux-telegraf.md) используется для сбора данных о производительности с компьютеров Linux для Azure Monitor метрик.

Используйте Telegraf Agent, если необходимо:

* Отправляйте данные в [метрики Azure Monitor](../essentials/data-platform-metrics.md) , чтобы проанализировать ее с помощью [обозревателя метрик](../essentials/metrics-getting-started.md) и воспользоваться такими функциями, как оповещения на основе [метрик](../alerts/alerts-metric-overview.md) практически в реальном времени и [Автомасштабирование](../autoscale/autoscale-overview.md) (только для Linux).

## <a name="dependency-agent"></a>Агент зависимостей

Агент зависимостей собирает обнаруженные данные о процессах, выполняющихся на компьютере, и зависимостях внешних процессов. 

Используйте агент зависимостей, если необходимо:

* Используйте функцию Map [Insights](../vm/vminsights-overview.md) или решение [сопоставление служб](../vm/service-map.md) .

При использовании агента зависимостей учитывайте следующее.

- Агенту зависимостей требуется установить агент Log Analytics на том же компьютере.
- На компьютерах Linux перед расширением диагностики Azure необходимо установить агент Log Analytics.

## <a name="virtual-machine-extensions"></a>Расширения виртуальных машин

Расширение Log Analytics для [Windows](../../virtual-machines/extensions/oms-windows.md) и [Linux](../../virtual-machines/extensions/oms-linux.md) установите агент log Analytics на виртуальных машинах Azure. Расширение зависимостей Azure Monitor для [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) и [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) устанавливает агент зависимостей на виртуальных машинах Azure. Это те же агенты, описанные выше, но позволяющие управлять ими с помощью [расширений виртуальной машины](../../virtual-machines/extensions/overview.md). При возможности для установки агентов и управления ими следует использовать расширения.

На гибридных компьютерах используйте [серверы с поддержкой ARC в Azure](../../azure-arc/servers/manage-vm-extensions.md) для развертывания Log Analytics и Azure Monitor расширений виртуальной машины зависимостей.

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

В следующих таблицах перечислены операционные системы, поддерживаемые агентами Azure Monitor. В документации по каждому агенту приведены уникальные рекомендации и процесс установки. См. документацию по Telegraf для поддерживаемых операционных систем. Предполагается, что все операционные системы имеют версию x64. Архитектура x86 не поддерживается для любой операционной системы.

### <a name="windows"></a>Windows

| Операционная система | Агент Azure Monitor | Агент Log Analytics | Агент зависимостей | Расширение системы диагностики | 
|:---|:---:|:---:|:---:|:---:|
| Windows Server 2019                                      | X | X | X | X |
| Windows Server 2016                                      | X | X | X | X |
| Windows Server 2016 Core                                 |   |   |   | X |
| Windows Server 2012 R2                                   | X | X | X | X |
| Windows Server 2012                                      | X | X | X | X |
| Windows Server 2008 R2 с пакетом обновления 1 (SP1)                               | X | X | X | X |
| Windows Server 2008 R2                                   |   | X | X | X |
| Windows 10 Корпоративная<br>(включая несколько сеансов) и Pro<br>(Только серверные сценарии)  | X | X | X | X |
| Windows 8 Корпоративная и Профессиональная<br>(Только серверные сценарии)  |   | X | X |   |
| Windows 7 с пакетом обновления 1 (SP1)<br>(Только серверные сценарии)                 |   | X | X |   |

### <a name="linux"></a>Linux

| Операционная система | Агент Azure Monitor | Агент Log Analytics | Агент зависимостей | Расширение системы диагностики | 
|:---|:---:|:---:|:---:|:---:
| Amazon Linux 2017.09                                        |   | X |   |   |
| CentOS Linux 8 <sup>1</sup> <sup>2</sup>                    | X | X | X |   |
| CentOS Linux 7                                              | X | X | X | X |
| CentOS Linux 6                                              |   | X |   |   |
| CentOS Linux 6.5 +                                           |   | X | X | X |
| Debian 10 <sup>1</sup>                                      | X |   |   |   |
| Debian 9                                                    | X | X | x | X |
| Debian 8                                                    |   | X | X |   |
| Debian 7                                                    |   |   |   | X |
| OpenSUSE 13.1+                                              |   |   |   | X |
| Oracle Linux 8 <sup>1</sup> <sup>2</sup>                    | X | X |   |   |
| Oracle Linux 7                                              | X | X |   | X |
| Oracle Linux 6                                              |   | X |   |   |
| Oracle Linux 6.4+                                           |   | X |   | X |
| Red Hat Enterprise Linux Server 8 <sup>1</sup> <sup>2</sup> | X | X | X |   |
| Red Hat Enterprise Linux Server 7                           | X | X | X | X |
| Red Hat Enterprise Linux Server 6                           |   | X | X |   |
| Red Hat Enterprise Linux Server 6.7 +                        |   | X | X | X |
| SUSE Linux Enterprise Server 15,2 <sup>1</sup> <sup>2</sup> | X |   |   |   |
| SUSE Linux Enterprise Server 15,1 <sup>1</sup> <sup>2</sup> | X | X |   |   |
| SUSE Linux Enterprise Server 15                             | X | X | X |   |
| SUSE Linux Enterprise Server 12                             | X | X | X | X |
| Ubuntu 20,04 LTS <sup>1</sup>                               | X | X | X |   |
| Ubuntu 18.04 LTS                                            | X | X | X | X |
| Ubuntu 16.04 LTS                                            | X | X | X | X |
| Ubuntu 14,04 LTS                                            |   | X |   | X |

<sup>1</sup> требует установки Python 3 на компьютере.

<sup>2</sup> известная ошибка при сборе событий системного журнала. В настоящее время поддерживаются только данные о производительности.
#### <a name="dependency-agent-linux-kernel-support"></a>Поддержка ядра Linux для агента зависимостей

Так как агент зависимостей работает на уровне ядра, поддержка также зависит от версии ядра. В следующей таблице перечислены основные и вспомогательные версии ОС Linux и поддерживаемые в агенте зависимости версиях ядра.

| Дистрибутив | Версия ОС | Версия ядра |
|:---|:---|:---|
|  Red Hat Linux 8   | 8.2     | 4.18.0-193.\*el8_2.x86_64 |
|                    | 8.1     | 4.18.0-147.\*el8_1.x86_64 |
|                    | 8.0     | 4.18.0-80. \* EL8.x86_64<br>4.18.0-80.\*el8_0.x86_64 |
|  Red Hat Linux 7   | 7.9     | 3.10.0 — 1160 |
|                    | 7.8     | 3.10.0 — 1136 |
|                    | 7.7     | 3.10.0 — 1062 |
|                    | 7.6     | 3.10.0-957  |
|                    | 7,5 %     | 3.10.0-862  |
|                    | 7.4     | 3.10.0-693  |
| Red Hat Linux 6    | 6.10    | 2.6.32-754 |
|                    | 6.9     | 2.6.32-696  |
| CentOS Linux 8     | 8.2     | 4.18.0-193.\*el8_2.x86_64 |
|                    | 8.1     | 4.18.0-147.\*el8_1.x86_64 |
|                    | 8.0     | 4.18.0-80. \* EL8.x86_64<br>4.18.0-80.\*el8_0.x86_64 |
| CentOS Linux 7     | 7.9     | 3.10.0 — 1160 |
|                    | 7.8     | 3.10.0 — 1136 |
|                    | 7.7     | 3.10.0 — 1062 |
| CentOS Linux 6     | 6.10    | 2.6.32 — 754.3.5<br>2.6.32 — 696.30.1 |
|                    | 6.9     | 2.6.32 — 696.30.1<br>2.6.32 — 696.18.7 |
| Сервер Ubuntu      | 20,04   | 5.4\* |
|                    | 18,04   | 5.3.0-1020<br>5,0 (включает ядро, настроенное Azure)<br>4,18 *<br> 4,15* |
|                    | 16.04.3 | 4,15.\* |
|                    | 16.04   | 4.13.\*<br>4.11.\*<br>4.10.\*<br>4.8.\*<br>4.4.\* |
| SUSE Linux 12 Enterprise Server | 15     | 4.12.14-150\*
|                                 | 12 SP4 | 4,12. * (включает в себя настраиваемый в Azure ядро) |
|                                 | 12 с пакетом обновления 3 | 4.4.* |
|                                 | 12 с пакетом обновления 2 | 4.4.* |
| Debian                          | 9      | 4.9  | 

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о каждом из агентов см. в следующих статьях:

- [Общие сведения об агенте Log Analytics](./log-analytics-agent.md)
- [Общие сведения о расширении "Диагностика Azure"](./diagnostics-extension-overview.md)
- [Сбор пользовательских метрик для виртуальной машины Linux с помощью агента Telegraf InfluxData](../essentials/collect-custom-metrics-linux-telegraf.md)