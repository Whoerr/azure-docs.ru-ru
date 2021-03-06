---
title: Мониторинг виртуальных машин Azure с помощью Azure Monitor
description: Описывается, как выполнять сбор и анализ данных мониторинга с виртуальных машин Azure с помощью Azure Monitor.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/05/2020
ms.openlocfilehash: 6209389843b19d933bdce2726b55946b8839a264
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101731380"
---
# <a name="monitoring-azure-virtual-machines-with-azure-monitor"></a>Мониторинг виртуальных машин Azure с помощью Azure Monitor
В этой статье описывается, как использовать Azure Monitor для сбора и анализа данных мониторинга с виртуальных машин Azure для поддержания их работоспособности. С помощью Azure Monitor можно отслеживать доступность и производительность виртуальных машин так же, как аналогичные параметры любых [других ресурсов Azure](../essentials/monitor-azure-resource.md). Однако они отличаются от других ресурсов тем, что, помимо прочего, необходимо отслеживать операционную систему на виртуальной машине и выполняющиеся на ней рабочие нагрузки. 

> [!NOTE]
> В этой статье содержится полный обзор принципов мониторинга виртуальных машин в Azure Monitor и доступных параметров. Чтобы быстро приступить к мониторингу виртуальных машин, не углубляясь в базовые принципы, обратитесь к статье [Краткое руководство. Мониторинг виртуальной машины Azure с помощью Azure Monitor](./quick-monitor-azure-vm.md).


## <a name="differences-from-other-azure-resources"></a>Отличия от других ресурсов Azure
В статье [Мониторинг ресурсов Azure с помощью Azure Monitor](../essentials/monitor-azure-resource.md) описаны данные мониторинга, создаваемые ресурсами Azure, а также анализ таких данных и создание оповещений о них с помощью функций Azure Monitor. Вы можете собирать аналогичные данные мониторинга с виртуальных машин Azure и работать с ними, но с рядом отличий.

-  [Метрики платформы](../essentials/data-platform-metrics.md) собираются автоматически для виртуальных машин, но только для [компьютера виртуальной машины](#monitoring-data). Для сбора данных по производительности из операционной системы на виртуальной машине необходим агент. 
- Виртуальные машины не создают [журналы ресурсов](../essentials/platform-logs-overview.md), которые позволяют подробно анализировать операции, выполненные с ресурсом Azure. Для сбора данных журналов из операционной системы на виртуальной машине используется агент.
- Вы можете создать [параметры диагностики](../essentials/diagnostic-settings.md) для виртуальной машины, чтобы передавать метрики платформы в другие места, например хранилище и концентраторы событий, но эти параметры нельзя настроить на портале Azure. 

## <a name="monitoring-data"></a>Данные мониторинга
Виртуальные машины в Azure создают [журналы](../logs/data-platform-logs.md) и [метрики](../essentials/data-platform-metrics.md) , как показано на следующей схеме.

![Обзор](media/monitor-vm-azure/logs-metrics.png)


### <a name="virtual-machine-host"></a>Компьютер виртуальной машины
Виртуальные машины Azure создают приведенные ниже данные для компьютера виртуальной машины так же, как и другие ресурсы Azure (см. статью [Мониторинг данных](../essentials/monitor-azure-resource.md#monitoring-data)).

- [Метрики платформы](../essentials/data-platform-metrics.md) — числовые значения, которые автоматически собираются через регулярные интервалы и описывают некоторый аспект ресурса на определенный момент времени. Метрики платформы собираются для компьютера виртуальной машины, но для сбора метрик для операционной системы на виртуальной машине требуется расширение диагностики.
- [Журнал действий](../essentials/platform-logs-overview.md) — предоставляет полезные сведения об операциях с каждым ресурсом Azure в подписке извне (плоскость управления). Для виртуальной машины сюда входят такие сведения, как время запуска и изменения конфигурации.


### <a name="guest-operating-system"></a>Операционная система на виртуальной машине
Для сбора данных из операционной системы на виртуальной машине требуется агент, который выполняется локально на каждой виртуальной машине и отправляет данные в Azure Monitor. Для Azure Monitor доступно несколько агентов, каждый из которых собирает различные данные и записывает их в разные места. Подробное сравнение различных агентов см. в статье [Обзор агентов Azure Monitor](../agents/agents-overview.md). 

- [Агент Log Analytics](../agents/agents-overview.md#log-analytics-agent) — доступен для виртуальных машин в Azure, других облачных средах и локальной среде. Собирает данные в журналах Azure Monitor. Поддерживает решения VM Insights и мониторинга. Это тот же агент, который используется для System Center Operations Manager.
- [Агент зависимостей](../agents/agents-overview.md#dependency-agent) — собирает данные по процессам, выполняемым на виртуальной машине, и их зависимостям. Использует агент Log Analytics для передачи данных в Azure и поддерживает решения VM Insights, Сопоставление служб и Wire Data 2.0.
- [Расширение диагностики Azure](../agents/agents-overview.md#azure-diagnostics-extension) — доступно только для виртуальных машин Azure Monitor. Может собирать данные в несколько мест, но в основном используется для сбора данных по производительности операционной системы Windows на виртуальных машинах в метрики Azure Monitor.
- [Агент Telegraf](../essentials/collect-custom-metrics-linux-telegraf.md) — собирает данные по производительности виртуальных машин Linux в метрики Azure Monitor.


## <a name="configuration-requirements"></a>Требования настройки
Чтобы включить все функции Azure Monitor для мониторинга виртуальной машины, необходимо собирать данные мониторинга с компьютера виртуальной машины и операционной системы на виртуальной машине в [метрики Azure Monitor](../logs/data-platform-logs.md) и [журналы Azure Monitor](../logs/data-platform-logs.md). В приведенной ниже таблице перечислены настройки, которые необходимо выполнить для включения сбора данных. В зависимости от требований можно не выполнять все эти действия.

| Этап настройки | Выполненные действия | Включенные функции |
|:---|:---|:---|
| Конфигурация отсутствует | — Сбор метрик платформы узла в компонент "Метрики".<br>— Сбор журналов действий. | — Обозреватель метрик для узла.<br>— Оповещения о метриках для узла.<br>— Оповещения журнала действий. |
| [Включение аналитики виртуальной машины](#enable-azure-monitor-for-vms) | — Агент Log Analytics установлен.<br>— Агент зависимостей установлен.<br>— Сбор данных по производительности операционной системы на виртуальных машинах в журналы.<br>— Сбор данных по процессам и зависимостям в журналы. | — Диаграммы производительности и книги для данных по производительности операционной системы на виртуальных машинах.<br>— Запросы к журналам для данных по производительности операционной системы на виртуальных машинах.<br>— Оповещения журналов для данных по производительности операционной системы на виртуальных машинах.<br>— Карта зависимостей. |
| [Установка расширения системы диагностики и агента telegraf](#enable-diagnostics-extension-and-telegraf-agent) | — Сбор данных по производительности операционной системы на виртуальных машинах в метрики. | — Обозреватель метрик для операционной системы на виртуальной машине.<br>— Оповещения о метриках для операционной системы на виртуальной машине.  |
| [Настройка рабочей области Log Analytics](#configure-log-analytics-workspace) | — Сбор событий из операционной системы на виртуальной машине. | — Запросы к журналам для событий операционной системы на виртуальной машине.<br>— Оповещения журналов для событий операционной системы на виртуальной машине. |
| [Создание параметра диагностики для виртуальной машины](#collect-platform-metrics-and-activity-log) | — Сбор метрик платформы в компонент "Журналы".<br>— Сбор журналов действий в компонент "Журналы". | — Заносить в журнал запросы для метрик узла.<br>— Оповещения журналов для метрик узла.<br>— Запросы к журналу действий.

Каждое из этих действий настройки описано в следующих разделах.

### <a name="enable-vm-insights"></a>Включение аналитики виртуальной машины
" [Аналитика ВМ](../vm/vminsights-overview.md) " — это [ценная информация](../monitor-reference.md) в Azure Monitor, которая является основным средством мониторинга виртуальных машин в Azure Monitor. Он предоставляет следующие дополнительные преимущества перед стандартными функциями Azure Monitor:

- упрощенное подключение агента Log Analytics и агента зависимостей для обеспечения мониторинга операционной системы на виртуальной машине и рабочих нагрузок; 
- предварительно определенные диаграммы тенденций производительности и книги, которые позволяют анализировать основные метрики производительности из операционной системы на виртуальной машине;
- карта зависимостей, на которой показаны процессы, выполняемые на каждой виртуальной машине, и взаимосвязь компонентов с другими компьютерами и внешними источниками.

![Представление производительности "VM Insights"](media/monitor-vm-azure/vminsights-01.png)

![Представление карт VM Insights](media/monitor-vm-azure/vminsights-02.png)


Включите аналитику виртуальных машин из параметра **Insights** в меню виртуальной машины портал Azure. Дополнительные сведения и другие методы настройки см. в разделе [Включение общих сведений о VM Insights](vminsights-enable-overview.md) .

![Включение аналитики виртуальной машины](media/monitor-vm-azure/enable-vminsights.png)

### <a name="configure-log-analytics-workspace"></a>Настройка рабочей области Log Analytics
Агент Log Analytics, используемый VM Insights, отправляет данные в [рабочую область log Analytics](../logs/data-platform-logs.md). Вы можете включить сбор дополнительных данных по производительности, событий и других данных мониторинга из агента, настроив рабочую область Log Analytics. Ее необходимо настроить только один раз, так как любой агент, подключающийся к рабочей области, автоматически скачивает конфигурацию и немедленно начинает сбор определенных данных. 

Вы можете получить доступ к конфигурации рабочей области непосредственно из Application Insights, выбрав пункт **Конфигурация рабочей области** в меню **Приступая к работе**. Щелкните имя рабочей области, чтобы открыть ее меню.

![Конфигурация рабочей области](media/monitor-vm-azure/workspace-configuration.png)

В меню рабочей области выберите пункт **Дополнительные параметры**, а затем пункт **Данные**, чтобы настроить источники данных. Для агентов Windows выберите **Журналы событий Windows** и добавьте общие журналы событий, такие как *Система* и *Приложение*. Для агентов Linux выберите **Syslog** и добавьте общие средства, такие как *kern* и *daemon*. Список доступных источников данных и сведения об их настройке см. в статье [Источники данных агента в Azure Monitor](../agents/agent-data-sources.md). 

![Настройка событий](media/monitor-vm-azure/configure-events.png)


> [!NOTE]
> Вы можете настроить сбор счетчиков производительности из конфигурации рабочей области, но это может быть избыточным счетчиками производительности, собранными службой "аналитика виртуальных машин". VM Insights собирает наиболее распространенный набор счетчиков с частотой один раз в минуту. Настраивайте счетчики производительности, которые будут собираться рабочей областью, только в том случае, если вы хотите собирать счетчики, которые еще не собраны с помощью VM Insights, или если у вас есть запросы, использующие данные о производительности.


### <a name="enable-diagnostics-extension-and-telegraf-agent"></a>Включение расширения системы диагностики и агента Telegraf
"Аналитика ВМ" основана на агенте Log Analytics, который отправляет данные в Log Analytics рабочую область. Он поддерживает несколько функций Azure Monitor, таких как [запросы к журналам](../logs/log-query-overview.md), [оповещения журналов](../alerts/alerts-log.md) и [книги](../visualize/workbooks-overview.md). [Расширение системы диагностики](../agents/diagnostics-extension-overview.md) собирает данные по производительности из операционной системы на виртуальных машинах Windows в службу хранилища Azure и при необходимости передает их в [метрики Azure Monitor](../essentials/data-platform-metrics.md). Для виртуальных машин Linux [агент Telegraf](../essentials/collect-custom-metrics-linux-telegraf.md) требуется для отправки данных в метрики Azure.  Это позволяет использовать дополнительные функции Azure Monitor, такие как [обозреватель метрик](../essentials/metrics-getting-started.md) и [оповещения о метриках](../alerts/alerts-metric.md). Расширение системы диагностики можно также настроить так, чтобы события и данные по производительности передавались за пределы Azure Monitor через Центры событий Azure.

Установите расширение системы диагностики для отдельной виртуальной машины Windows на портале Azure с помощью пункта **Параметры диагностики** в меню виртуальной машины. На вкладке **Приемники** выберите команду включения **Azure Monitor**. Инструкции по включению расширения для нескольких виртуальных машин из шаблона или командной строки см. в разделе [Установка и настройка](../agents/diagnostics-extension-overview.md#installation-and-configuration). В отличие от агента Log Analytics, собираемые данные определяются в конфигурации расширения на каждой виртуальной машине.

![Параметр диагностики](media/monitor-vm-azure/diagnostic-setting.png)

Подробные сведения о настройке агентов Telegraf на виртуальных машинах Linux см. в разделе [Установка и настройка Telegraf](../essentials/collect-custom-metrics-linux-telegraf.md#install-and-configure-telegraf). Пункт меню **Параметры диагностики** доступен для Linux, но позволяет передавать данные только в службу хранилища Azure.

### <a name="collect-platform-metrics-and-activity-log"></a>Сбор метрик платформы и журнала действий
Метрики платформы и журнал действий, собранные для каждого компьютера виртуальной машины, можно просмотреть на портале Azure. Собирайте эти данные в ту же рабочую область Log Analytics, что и VM Insights, чтобы проанализировать их с другими данными мониторинга, собранными для виртуальной машины. Сбор настраивается с помощью [параметра диагностики](../essentials/diagnostic-settings.md). Для сбора журнала действий используется [параметр диагностики для подписки](../essentials/diagnostic-settings.md#create-in-azure-portal).

Для сбора метрик платформы используется параметр диагностики для виртуальной машины. В отличие от других ресурсов Azure, вы не можете создать параметр диагностики для виртуальной машины на портале Azure. Для этого следует использовать [другой метод](../essentials/diagnostic-settings.md#create-using-powershell). Ниже приведены примеры сбора метрик для виртуальной машины с помощью PowerShell и CLI.

```powershell
Set-AzDiagnosticSetting -Name vm-diagnostics -ResourceId "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-vm" -Enabled $true -MetricCategory AllMetrics -workspaceId "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace"
```

```azurecli
az monitor diagnostic-settings create \
--name VM-Diagnostics 
--resource /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-vm \
--metrics '[{"category": "AllMetrics","enabled": true}]' \
--workspace /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace
```

## <a name="monitoring-in-the-azure-portal"></a>Мониторинг на портале Azure 
После настройки сбора данных мониторинга для виртуальной машины доступ к ним на портале Azure можно получить несколькими способами.

- Используйте меню **Azure Monitor** для доступа к данным из всех отслеживаемых ресурсов. 
- Используйте аналитику VM для мониторинга наборов виртуальных машин в масштабе.
- Анализируйте данные отдельной виртуальной машины из ее меню на портале Azure. В таблице ниже перечислены различные параметры мониторинга в меню виртуальных машин.

![Мониторинг на портале Azure](media/monitor-vm-azure/monitor-menu.png)

| Пункт меню | Описание |
|:---|:---|
| Обзор | Отображение [метрик платформы](../essentials/data-platform-metrics.md) для компьютера виртуальной машины. Щелкните граф для работы с этими данными в [обозревателе метрик](../essentials/metrics-getting-started.md). |
| Журнал действий | Записи [журнала действий](../essentials/activity-log.md#view-the-activity-log), отфильтрованные для текущей виртуальной машины. |
| Аналитика | Открывает [аналитику виртуальной машины](../vm/vminsights-overview.md) с картой для выбранной текущей виртуальной машины. |
| видны узлы | Просмотр [оповещений](../alerts/alerts-overview.md) для текущей виртуальной машины.  |
| Метрики | Открытие [обозревателя метрик](../essentials/metrics-getting-started.md) для текущей виртуальной машины. |
| Параметры диагностики | Включение и настройка [расширения системы диагностики](../agents/diagnostics-extension-overview.md) для текущей виртуальной машины. |
| Рекомендации Помощника | Рекомендации для текущей виртуальной машины из [Помощника по Azure](../../advisor/index.yml). |
| Журналы | Открытие [Log Analytics](../logs/log-analytics-overview.md) с [областью](../logs/scope.md) текущей виртуальной машины. |
| Монитор подключения | Откройте [Монитор подключений Наблюдателя за сетями](../../network-watcher/connection-monitor-overview.md), чтобы отслеживать подключения между текущей виртуальной машиной и другими виртуальными машинами. |


## <a name="analyzing-metric-data"></a>Анализ данных метрик
Вы можете анализировать метрики виртуальных машин с помощью обозревателя метрик. Для этого выберите пункт **Метрики** в меню виртуальной машины. Подробные сведения об использовании этого средства см. в статье [Начало работы с обозревателем метрик Azure](../essentials/metrics-getting-started.md). 

Для метрик виртуальных машин используются три пространства имен.

| Пространство имен | Описание | Требование |
|:---|:---|:---|
| Компьютер виртуальной машины | Метрики компьютера автоматически собираются для всех виртуальных машин Azure. Подробный список метрик: [Microsoft.Compute/virtualMachines](../essentials/metrics-supported.md#microsoftcomputevirtualmachines). | Собираются автоматически без настройки. |
| Гость (классическая версия) | Ограниченный набор данных по производительности операционной системы на виртуальном компьютере и производительности приложений. Доступно в обозревателе метрик, но не в других компонентах Azure Monitor, таких как оповещения о метриках.  | [Расширение системы диагностики](../agents/diagnostics-extension-overview.md) установлено. Данные считываются из службы хранилища Azure.  |
| Операционная система на виртуальной машине | Данные по операционной системе на виртуальной машине и производительности приложений доступны для всех компонентов Azure Monitor с помощью метрик. | Для Windows [расширение системы диагностики](../agents/diagnostics-extension-overview.md) должно быть установлено с включенным приемником Azure Monitor. Для Linux должен быть [установлен агент Telegraf](../essentials/collect-custom-metrics-linux-telegraf.md). |

![Обозреватель метрик в портал Azure](media/monitor-vm-azure/metrics.png)

## <a name="analyzing-log-data"></a>Анализ данных журналов
Виртуальные машины Azure будут собирать приведенные ниже данные в журналы Azure Monitor. 

VM Insights включает сбор предварительно определенного набора счетчиков производительности, записываемых в таблицу *инсигхтсметрикс* . Это та же самая таблица, которая используется в [контейнере аналитики](../containers/container-insights-overview.md). 

| Источник данных | Требования | Таблицы |
|:---|:---|:---|
| Аналитика ВМ | Включите на каждой виртуальной машине. | InsightsMetrics<br>VMBoundPort<br>VMComputer<br>VMConnection<br>VMProcess<br>Дополнительные сведения см. [в статье запросы к журналам из VM Insights](../vm/vminsights-log-search.md) . |
| Журнал действий | Параметр диагностики для подписки. | AzureActivity |
| Метрики узла | Параметр диагностики для виртуальной машины. | AzureMetrics |
| Источники данных из операционной системы на виртуальной машине | Включите агент Log Analytics и настройте источники данных. | См. документацию по каждому источнику данных. |


> [!NOTE]
> Данные о производительности, собираемые агентом Log Analytics, записываются в таблицу *производительности* , пока VM Insights соберет ее в таблице *инсигхтсметрикс* . Данные одинаковы, но таблицы имеют разную структуру. Если у вас есть существующие запросы на основе таблицы *Perf*, их потребуется переписать для использования *InsightsMetrics*.


## <a name="alerts"></a>видны узлы
[Оповещения](../alerts/alerts-overview.md) в Azure Monitor заранее уведомляют вас об обнаружении определенных состояний в данных мониторинга и могут выполнять действия, например запускать приложение логики или вызывать веб-перехватчик. Правила генерации оповещений определяют логику, используемую для определения того, когда необходимо создавать оповещения. Azure Monitor собирает данные, используемые правилами генерации оповещений, но вам необходимо создать правила для определения условий срабатывания оповещений в подписке Azure.

В следующих разделах описываются типы правил генерации оповещений и рекомендации по их использованию. Эта рекомендация основана на функциональности и стоимости типа правила генерации оповещений. Подробные сведения о ценах на оповещения см. в разделе [Цены на Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).


### <a name="activity-log-alert-rules"></a>Правила генерации оповещений журнала действий
[Правила генерации оповещений для журнала действий](../alerts/alerts-activity-log.md) срабатывают при создании в журнале действий записи, соответствующей определенным условиям. Они бесплатны, поэтому их следует использовать в первую очередь, если нужная логика находится в журнале действий. 

Целевым ресурсом для оповещений журнала действий может быть определенная виртуальная машина, все виртуальные машины в группе ресурсов или все виртуальные машины в подписке.

Например, создайте оповещение об остановке критически важной виртуальной машины, выбрав *Отключить питание виртуальной машины* в качестве имени сигнала.

![Оповещение журнала действий](media/monitor-vm-azure/activity-log-alert.png)


### <a name="metric-alert-rules"></a>Правила генерации оповещений метрик
[Правила генерации оповещений о метриках](../alerts/alerts-metric.md) срабатывают в случае превышения порогового значения метрикой. Вы можете определить конкретное пороговое значение или разрешить Azure Monitor динамически определять пороговое значение на основе исторических данных.  Используйте оповещения о метриках для данных метрик, когда это возможно, так как они дешевле и оперативнее, чем правила генерации оповещений для журналов. Они также являются оповещениями с отслеживанием состояния, то есть разрешаются, когда значение метрики опускается ниже порогового значения.

Целевым ресурсом для оповещений о метриках может быть определенная виртуальная машина или все виртуальные машины в группе ресурсов.

Например, чтобы создать оповещение о превышении определенного значения процессором виртуальной машины, создайте правило генерации оповещений о метрике с типом сигнала *Загрузка ЦП*. Задайте конкретное пороговое значение или разрешите Azure Monitor задавать пороговое значение динамически. 

![Оповещение о метрике](media/monitor-vm-azure/metric-alert.png)

### <a name="log-alerts"></a>Оповещения журналов
[Правила генерации оповещений для журналов](../alerts/alerts-log.md) срабатывают, когда результаты запланированного запроса к журналу соответствуют определенным условиям. Оповещения о запросах к журналам — наиболее дорогой и наименее оперативный тип правил генерации оповещений, но они обеспечивают доступ к наиболее разнообразным данным и позволяют выполнять сложную логику, которая невозможна при использовании других правил генерации оповещений. 

Целевым ресурсом для запроса к журналу является рабочая область Log Analytics. Фильтрация определенных компьютеров осуществляется в запросе.

Например, чтобы создать оповещение, которое проверяет, отключены ли какие-либо виртуальные машины в определенной группе ресурсов, используйте приведенный ниже запрос. Он возвращает запись для каждого компьютера, который пропустил пакет пульса за последние десять минут. Используйте пороговое значение 1, чтобы оповещение срабатывало, если по крайней мере один компьютер имеет пропущенный пакет пульса.

```kusto
Heartbeat
| where TimeGenerated > ago(10m)
| where ResourceGroup == "my-resource-group"
| summarize max(TimeGenerated) by Computer
```

![Оповещение журнала о пропущенных пульсах](media/monitor-vm-azure/log-alert-01.png)

Чтобы создать оповещение о слишком большом числе неудачных попыток входа на каких-либо виртуальных машинах Windows в подписке, используйте приведенный ниже запрос. Он возвращает запись для каждого события неудачного входа за последний час. В качестве порогового значения укажите допустимое число неудачных попыток входа. 

```kusto
Event
| where TimeGenerated > ago(1hr)
| where EventID == 4625
```

![Вести оповещение журнала о неудачных попытках входа](media/monitor-vm-azure/log-alert-02.png)


## <a name="system-center-operations-manager"></a>System Center Operations Manager
System Center Operations Manager обеспечивает детальный мониторинг рабочих нагрузок на виртуальных машинах. Сравнение платформ мониторинга и стратегий реализации см. в статье [Руководство по облачному мониторингу](/azure/cloud-adoption-framework/manage/monitor/).

При наличии существующей среды Operations Manager, которую вы собираетесь использовать, ее можно интегрировать с Azure Monitor для предоставления дополнительных функциональных возможностей. Агент Log Analytics, используемый Azure Monitor, аналогичен тому, который используется для Operations Manager, поэтому отслеживаемые виртуальные машины будут отсылать данные в оба. Вам по-прежнему необходимо добавить агент в Application Insights и настроить рабочую область для получения дополнительных данных, как указано выше, но виртуальные машины могут продолжать запускать существующие пакеты управления в среде Operations Manager без изменения.

К функциям Azure Monitor, дополняющим существующие функции Operations Manager, относятся следующие.

- использование Log Analytics для интерактивного анализа данных журналов и данных по производительности;
- Используйте оповещения журнала для определения условий предупреждений на нескольких виртуальных машинах и использования долгосрочных тенденций, которые невозможно использовать при использовании предупреждений в Operations Manager.   

Дополнительные сведения о подключении существующей группы управления Operations Manager к рабочей области Log Analytics см. в разделе [Connect Operations Manager to Azure Monitor](../agents/om-agents.md) .


## <a name="next-steps"></a>Дальнейшие действия

* [Узнайте, как анализировать данные в журналах Azure Monitor с помощью запросов к журналам.](../logs/get-started-queries.md)
* [Узнайте об оповещениях на основе метрик и журналов в Azure Monitor.](../alerts/alerts-overview.md)