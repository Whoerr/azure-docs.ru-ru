---
title: Вопросы и ответы по решению "Монитор производительности сети" (Azure) | Документация Майкрософт
description: В этой статье собираются часто задаваемые вопросы о Монитор производительности сети в Azure. Монитор производительности сети (NPM) помогает отслеживать производительность сетей практически в реальном времени и обнаруживать узкие места в производительности сети.
ms.topic: conceptual
author: vinynigam
ms.author: vinigam
ms.date: 10/12/2018
ms.openlocfilehash: 909cbd6174fe7eceaa8b53b5ba44fe72990b56d9
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101708056"
---
# <a name="network-performance-monitor-solution-faq"></a>Вопросы и ответы по решению "Монитор производительности сети"

![Символ монитора производительности сети](media/network-performance-monitor-faq/npm-symbol.png)

> [!IMPORTANT]
> Начиная с 1 июля 2021 вы не сможете добавлять новые тесты в существующую рабочую область или включать новую рабочую область в Монитор производительности сети. Вы можете продолжать использовать тесты, созданные до 1 июля 2021. Чтобы минимизировать перерыв в работе службы для текущих рабочих нагрузок, [перенесите тесты из монитор производительности сети в новый монитор подключений](../../network-watcher/migrate-to-connection-monitor-from-network-performance-monitor.md) в наблюдатель за сетями Azure до 29 февраля 2024.

В этой статье собраны часто задаваемые вопросы о решении "Монитор производительности сети" в Azure

[Монитор производительности сети](../../networking/network-monitoring-overview.md) — это облачное решение для [мониторинга гибридной сети](./network-performance-monitor-performance-monitor.md) , которое помогает отслеживать производительность сети между различными точками в сетевой инфраструктуре. Это решение также помогает отслеживать сетевое подключение к [конечным точкам служб и приложений](./network-performance-monitor-service-connectivity.md), а также [производительность Azure ExpressRoute](./network-performance-monitor-expressroute.md). 

Монитор производительности сети обнаруживает проблемы сети, такие как появление "черных дыр" в трафике, ошибки маршрутизации и проблемы, которые невозможно обнаружить с помощью традиционных методов мониторинга сети. Решение создает предупреждения и уведомляет о превышении порогового значения для сетевого соединения. Кроте того, оно обеспечивает своевременное обнаружение проблем с производительностью сети и определяет конкретный сегмент сети или сетевое устройство как источник проблемы. 

Дополнительные сведения о различных возможностях, поддерживаемых решение "Монитор производительности сети", см. в статье [Решения для мониторинга сетей](../../networking/network-monitoring-overview.md).

## <a name="set-up-and-configure-agents"></a>Установка и настройка агентов

### <a name="what-are-the-platform-requirements-for-the-nodes-to-be-used-for-monitoring-by-npm"></a>Каковы требования к платформе для узлов, которые будут использоваться решением "Монитор производительности сети"?
Ниже перечислены требования к платформе для разных функций решения "Монитор производительности сети".

- Возможности монитора производительности и подключения службы NPM поддерживают как Windows Server, так и клиентские операционные системы Windows. Поддерживаются версии ОС Windows Server 2008 SP1 или более поздней версии. Поддерживаются версии Windows 10, Windows 8.1, Windows 8 и Windows 7 для настольных компьютеров и клиентских версий Windows. 
- Мониторинг ExpressRoute, входящий в состав решения "Монитор производительности сети", поддерживает только операционную систему Windows Server (2008 с пакетом обновления 1 или более поздней версии).

### <a name="can-i-use-linux-machines-as-monitoring-nodes-in-npm"></a>Можно ли использовать компьютеры Linux в качестве узлов мониторинга в решении "Монитор производительности сети"?
Возможность наблюдения за сетями с помощью узлов на основе Linux теперь является общедоступной. Получите доступ к [агенту](../../virtual-machines/extensions/oms-linux.md). 

### <a name="what-are-the-size-requirements-of-the-nodes-to-be-used-for-monitoring-by-npm"></a>Каковы требования к размеру для узлов, которые будут использоваться решением "Монитор производительности сети"?
Чтобы решение "Монитор производительности сети" работало на виртуальных машинах узла и выполняло мониторинг сети, этот узел должен располагать как минимум 500 МБ памяти и одним ядром. Для запуска NPM не нужно использовать отдельные узлы. Это решение может выполняться на узлах, которые выполняют другие рабочие нагрузки. Решение может прерывать процесс мониторинга, если он использует более 5% ресурсов ЦП.

### <a name="to-use-npm-should-i-connect-my-nodes-as-direct-agent-or-through-system-center-operations-manager"></a>Нужно ли подключать узлы как прямой агент? Или же для решения "Монитор производительности сети" следует использовать System Center Operations Manager?
Возможности монитора производительности и подключения службы поддерживают узлы [, подключенные как прямые агенты](../agents/agent-windows.md) и [подключенные с помощью Operations Manager](../agents/om-agents.md).

Для функции мониторинга ExpressRoute узлы Azure должны быть обязательно подключены как прямые агенты. Эта функция не поддерживает узлы Azure, подключенные через Operations Manager. Для локальных узлов узлы, подключенные как прямые агенты и с помощью Operations Manager, поддерживаются для мониторинга канала ExpressRoute.

### <a name="which-protocol-among-tcp-and-icmp-should-be-chosen-for-monitoring"></a>Какой протокол лучше выбрать для мониторинга — TCP или ICMP?
При мониторинге сети с помощью узлов на основе Windows Server рекомендуется использовать протокол TCP в качестве протокола мониторинга, так как он обеспечивает лучшую точность. 

ICMP рекомендуется для настольных компьютеров под управлением Windows и узлов на базе клиентской операционной системы. Эта платформа не разрешает отправку данных TCP через необработанные сокеты, которые NPM использует для обнаружения топологии сети.

Дополнительные сведения о сравнительных преимуществах протоколов вы можете получить [здесь](./network-performance-monitor-performance-monitor.md#choose-the-protocol).

### <a name="how-can-i-configure-a-node-to-support-monitoring-using-tcp-protocol"></a>Как настроить узел для поддержки мониторинга по протоколу TCP?
Чтобы узел поддерживал мониторинг по протоколу TCP, нужно сделать следующее: 
* Убедитесь, что узел использует платформу Windows Server (2008 с пакетом обновления 1 или более поздней версии).
* Запустите [EnableRules.ps1](https://aka.ms/npmpowershellscript) сценарий PowerShell на узле. Дополнительные сведения см. в разделе [инструкций](./network-performance-monitor.md#configure-log-analytics-agents-for-monitoring).


### <a name="how-can-i-change-the-tcp-port-being-used-by-npm-for-monitoring"></a>Как изменить порт TCP, используемый решением "Монитор производительности сети"?
Чтобы изменить TCP-порт, используемый решением "Монитор производительности сети", выполните скрипт [EnableRules.ps1](https://aka.ms/npmpowershellscript). В качестве параметра передайте ему новый номер порта, который вы намерены использовать. Например, для переключения TCP на порт 8060 запустите команду `EnableRules.ps1 8060`. Следите за тем, чтобы использовать один и тот же порт TCP на всех узлах, участвующих в мониторинге.

Сценарий настраивает брандмауэр Windows только локально. Если вы используете брандмауэр сети или правила группы безопасности сети (NSG), разрешите на них входящий трафик к TCP-порту, который настроен для решения "Монитор производительности сети".

### <a name="how-many-agents-should-i-use"></a>Сколько агентов нужно использовать?
Следует настроить хотя бы один агент для каждой подсети, к которой применяется мониторинг.

### <a name="what-is-the-maximum-number-of-agents-i-can-use-or-i-see-error--youve-reached-your-configuration-limit"></a>Каково максимальное количество агентов, которые я могу использовать, или отображается ошибка "..." Вы достигли предельного значения конфигурации "?
NPM ограничивает количество IP-адресов до 5000 IP-адресов на каждую рабочую область. Если у узла есть IP-адреса версий 4 и 6, то считается, что для этого узла задано два IP-адреса. Таким образом, это ограничение в 5000 IP-адресов определяет максимально допустимое число агентов. Неактивных агентов можно удалить на вкладке "Узлы" в разделе NPM > "Настройка". NPM также ведет журнал всех IP-адресов, которые были назначены виртуальной машине, на которой размещен агент, и каждый из них считается отдельным IP, участвующим в этом верхнем ограничении (5000 IP). Чтобы освободить IP-адреса для рабочей области, можно использовать страницу узлы, чтобы удалить неиспользуемые адреса IP.

## <a name="monitoring"></a>Наблюдение

### <a name="how-are-loss-and-latency-calculated"></a>Как вычисляются потери и задержки?
Агенты-источники отправляют запросы TCP SYN (если выбран протокол мониторинга TCP) или ICMP ECHO (если выбран протокол мониторинга ICMP) по IP-адресу назначения с регулярными интервалами. Этот механизм позволяет проверить все пути между разными сочетаниями IP-адресов источников и назначений. Для вычисления потерь и задержки по каждому пути измеряется процент полученных пакетов и время кругового пути для этих пакетов. Такие данные собираются на протяжении всего интервала опроса и по всем путям. На их основе вычисляются статистические значения потерь и задержек для конкретных сочетаний IP-адресов за конкретные интервалы опроса.

### <a name="with-what-frequency-does-the-source-agent-send-packets-to-the-destination-for-monitoring"></a>С какой частотой агенты-источники отправляют по адресу назначения пакеты для мониторинга?
Для функций монитора производительности и монитора ExpressRoute каждый источник отправляет пакеты каждые 5 секунд и фиксирует измеренные данные. Эти данные агрегируются по 3-минутным интервалам опроса, для которых вычисляются среднее и пиковые значения потерь и задержек. Что касается монитора подключений службы, частота отправки пакетов для анализа сети зависит от частоты, введенной пользователем при настройке конкретного теста.

### <a name="how-many-packets-are-sent-for-monitoring"></a>Сколько пакетов отправляется для мониторинга?
Количество пакетов, отправленных агентом-источником по адресу назначения в процессе опроса зависит от сетевой топологии. Для корректировки мы используем специальный алгоритм собственной разработки. В среднем, чем больше сетевых путей может использоваться между каждым сочетанием IP-адресов источника и назначения, тем больше отправляется пакетов. Система гарантирует, что в анализ включаются все пути между сочетаниями IP-адресов источника и назначения.

### <a name="how-does-npm-discover-network-topology-between-source-and-destination"></a>Как решение "Монитор производительности сети" определяет топологию сети между источником и назначением?
Решение "Монитор производительности сети" использует наш собственный алгоритм, основанный на команде Traceroute, для обнаружения всех путей и прыжков между источником и назначением.

### <a name="does-npm-provide-routing-and-switching-level-info"></a>Предоставляет ли решение "Монитор производительности сети" сведения об уровне маршрутизации и коммутации? 
Решение "Монитор производительности сети" может обнаруживать все возможные маршруты между агентом-источником и назначением. Но сведения о том, какой путь использовался для пакетов от конкретной рабочей нагрузки, не предоставляются. Зато это решение поможет вам выявить возможные пути и прыжки в сети на более низком уровне, по которым задержки превышают ожидаемый показатель.

### <a name="why-are-some-of-the-paths-unhealthy"></a>Какие могут быть причины плохой работоспособности путей?
Между IP-адресами источника и назначения может быть много разных сетевых путей, для каждого из которых будут получены разные значения потерь и задержек. Решение "Монитор производительности сети" помечает как неработоспособные (и обозначает красным цветом) пути, для которых измеренные значения потерь и (или) задержек оказались выше порогового значения, заданного в конфигурации мониторинга.

### <a name="what-does-a-hop-in-red-color-signify-in-the-network-topology-map"></a>Что означает красный цвет прыжка на карте топологии сети?
Если прыжок выделен красным цветом, значит он входит хотя бы в один неработоспособный путь. Решение "Монитор производительности сети" только помечает неработоспособные пути, но не анализирует состояние работоспособности подробно для каждого пути. Чтобы определить прыжки, создающие проблемы, просмотрите значения задержки для отдельных узлов и выделите среди них те, на которых задержка увеличивается больше ожидаемого.

### <a name="how-does-fault-localization-in-performance-monitor-work"></a>Как работает определение расположения ошибок в мониторе производительности?
Решение "Монитор производительности сети" использует вероятностный механизм для присвоения значений вероятности сбоя каждому сетевому пути, сегменту сети и входящим в них сетевым прыжком. Эта оценка основывается на количестве неработоспособных путей, частью которых является конкретный элемент. Чем больше неработоспособных путей проходит через определенный сегмент сети или содержит определенный прыжок, тем более высокая вероятность сбоя им присваивается. Этот алгоритм лучше всего работает при наличии большого числа подключенных друг к другу узлов с установленным агентом решения "Монитор производительности сети", так как это дает больше точек данных для вычисления вероятностей сбоя.

### <a name="how-can-i-create-alerts-in-npm"></a>Как создавать оповещения в решении "Монитор производительности сети"?
В настоящее время создание предупреждений из пользовательского интерфейса NPM завершается сбоем из-за известной проблемы. [Создайте оповещения вручную](../alerts/alerts-log.md).

### <a name="what-are-the-default-log-analytics-queries-for-alerts"></a>Что такое Log Analytics запросы по умолчанию для оповещений
Запрос системного монитора

```kusto
NetworkMonitoring
 | where (SubType == "SubNetwork" or SubType == "NetworkPath") 
 | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy") and RuleName == "<<your rule name>>"
```

Запрос монитора подключения службы

```kusto
NetworkMonitoring
 | where (SubType == "EndpointHealth" or SubType == "EndpointPath")
 | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or ServiceResponseHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy") and TestName == "<<your test name>>"
```

Запросы монитора ExpressRoute: запрос каналов

```kusto
NetworkMonitoring
 | where (SubType == "ERCircuitTotalUtilization") and (UtilizationHealthState == "Unhealthy") and CircuitResourceId == "<<your circuit resource ID>>"
```

Частный пиринг

```kusto
NetworkMonitoring
 | where (SubType == "ExpressRoutePeering" or SubType == "ERVNetConnectionUtilization" or SubType == "ExpressRoutePath")   
 | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or UtilizationHealthState == "Unhealthy") and CircuitName == "<<your circuit name>>" and VirtualNetwork == "<<vnet name>>"
```

Пиринг Майкрософт

```kusto
NetworkMonitoring
 | where (SubType == "ExpressRoutePeering" or SubType == "ERMSPeeringUtilization" or SubType == "ExpressRoutePath")
 | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or UtilizationHealthState == "Unhealthy") and CircuitName == ""<<your circuit name>>" and PeeringType == "MicrosoftPeering"
```

Общий запрос

```kusto
NetworkMonitoring
 | where (SubType == "ExpressRoutePeering" or SubType == "ERVNetConnectionUtilization" or SubType == "ERMSPeeringUtilization" or SubType == "ExpressRoutePath")
 | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or UtilizationHealthState == "Unhealthy")
```

### <a name="can-npm-monitor-routers-and-servers-as-individual-devices"></a>Может ли решение "Монитор производительности сети" отслеживать маршрутизаторы и серверы как отдельные устройства?
Решение "Монитор производительности сети" определяет IP-адреса и имена узлов для всех сетевых прыжков (коммутаторы, маршрутизаторы, серверы и др.) между IP-адресами источника и назначения. Также он вычисляет задержку между обнаруженными прыжками. Но мониторинг на уровне каждого прыжка не выполняется.

### <a name="can-npm-be-used-to-monitor-network-connectivity-between-azure-and-aws"></a>Можно ли использовать решение "Монитор производительности сети" для мониторинга сетевого подключения между Azure и AWS?
Да. Дополнительные сведения см. в статье о [мониторинге Azure, AWS и локальных сетей с помощью решения "Монитор производительности сети"](/archive/blogs/msoms/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor).

### <a name="is-the-expressroute-bandwidth-usage-incoming-or-outgoing"></a>Какая пропускная способность ExpressRoute учитывается в оценке потребления — входящая или исходящая?
Потребление пропускной способности оценивается по сумме входящей и исходящей пропускной способности. Этот параметр выражается в битах в секунду.

### <a name="can-we-get-incoming-and-outgoing-bandwidth-information-for-the-expressroute"></a>Могу ли я получить сведения о входящей и исходящей пропускной способности для ExpressRoute?
Есть возможность собирать данные о входящей и исходящей пропускной способности для основного и вторичного подключений.

Для получения сведений об уровне пиринга MS используйте приведенный ниже запрос в поиске по журналам.

```kusto
NetworkMonitoring
 | where SubType == "ERMSPeeringUtilization"
 | project CircuitName,PeeringName,BitsInPerSecond,BitsOutPerSecond 
```

Чтобы получить сведения об частном одноранговом уровне, используйте приведенный ниже запрос в поиске по журналам.

```kusto
NetworkMonitoring
 | where SubType == "ERVNetConnectionUtilization"
 | project CircuitName,PeeringName,BitsInPerSecond,BitsOutPerSecond
```

Для получения сведений об уровне канала используйте приведенный ниже запрос в поиске по журналам.

```kusto
NetworkMonitoring
 | where SubType == "ERCircuitTotalUtilization"
 | project CircuitName, BitsInPerSecond, BitsOutPerSecond
```

### <a name="which-regions-are-supported-for-npms-performance-monitor"></a>Какие регионы поддерживаются для монитора производительности, входящего в решение "Монитор производительности сети"?
Решение "Монитор производительности сети" может отслеживать подключение между сетями, расположенными в любых частях мира, если он размещен в рабочей области в одном из [поддерживаемых регионов](./network-performance-monitor.md#supported-regions).

### <a name="which-regions-are-supported-for-npms-service-connectivity-monitor"></a>Какие регионы поддерживаются для монитора подключений службы, входящего в решение "Монитор производительности сети"?
Решение "Монитор производительности сети" может отслеживать подключение к службам, расположенными в любых частях мира, если он размещен в рабочей области в одном из [поддерживаемых регионов](./network-performance-monitor.md#supported-regions).

### <a name="which-regions-are-supported-for-npms-expressroute-monitor"></a>Какие регионы поддерживаются для монитора ExpressRoute, входящего в решение "Монитор производительности сети"?
Решение "Монитор производительности сети" может отслеживать каналы ExpressRoute, расположенные в любом регионе Azure. Чтобы подключить канал к решению "Монитор производительности сети", нужно создать рабочую область Log Analytics в одном из [поддерживаемых регионов](../../expressroute/how-to-npm.md).

## <a name="troubleshoot"></a>Диагностика

### <a name="why-are-some-of-the-hops-marked-as-unidentified-in-the-network-topology-view"></a>Почему некоторые прыжки в представлении топологии сети обозначены как "не определено"?
Решение "Монитор производительности сети" использует модифицированную версию команды traceroute для определения топологии сети между агентом-источником и назначением. Неопределенным считается прыжок, который не предоставил ответ на запрос traceroute от агента-источника. Если три последовательных прыжка сети не отвечают на Traceroute агента, решение помечает неотвечающие прыжки как неидентифицированные и не пытается обнаружить больше прыжков.

Прыжок может не отвечать на запросы traceroute по одной или нескольким причинам из перечисленных ниже:

* настройки маршрутизатора запрещают ему идентифицировать себя;
* сетевые устройства не пропускают трафик типа ICMP_TTL_EXCEEDED;
* брандмауэр блокирует ответ ICMP_TTL_EXCEEDED от сетевого устройства.

Если какая либо из конечных точек находится в Azure, Traceroute отображает неидентифицированные промежуточные точки, так как инфраструктура Azure не раскрывает удостоверение Traceroute. 

### <a name="i-get-alerts-for-unhealthy-tests-but-i-do-not-see-the-high-values-in-npms-loss-and-latency-graph-how-do-i-check-what-is-unhealthy"></a>Я получаю оповещения о неработоспособных тестах, но не вижу высокие значения в графе потери и задержки NPM. Разделы справки проверить неработоспособность?
NPM создает оповещение, если сквозная задержка между источником и назначением пересекает пороговое значение для любого пути между ними. Некоторые сети имеют несколько путей, соединяющих один и тот же источник и назначение. NPM создает предупреждение — любой путь является неработоспособным. Потери и задержки, показанные на диаграммах, являются средним значением для всех путей, поэтому оно может не показывать точное значение одного пути. Чтобы понять, где произошло нарушение порога, найдите в предупреждении столбец "подтип". Если причиной проблемы является путь, то значение подтипа будет Нетворкпас (для тестов системного монитора), Ендпоинтпас (для тестов монитора подключения к службам) и Експрессраутепас (для тестов монитора Експрессротуе). 

Образец запроса для поиска — это неработоспособный путь:

```kusto
NetworkMonitoring
 | where ( SubType == "ExpressRoutePath")
 | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or UtilizationHealthState == "Unhealthy") and CircuitResourceID =="<your ER circuit ID>" and ConnectionResourceId == "<your ER connection resource id>"
 | project SubType, LossHealthState, LatencyHealthState, MedianLatency
```

### <a name="why-does-my-test-show-unhealthy-but-the-topology-does-not"></a>Почему тест отображается в неработоспособном состоянии, но топология не 
NPM отслеживает сквозные потери, задержки и топологию с разными интервалами. Потери и задержка измеряются каждые 5 секунд и суммируются каждые три минуты (для монитора производительности и монитора Express Route), а топология вычисляется с помощью Traceroute один раз каждые 10 минут. Например, между 3:44 и 4:04 топология может обновляться три раза (3:44, 3:54, 4:04), но потери и задержки обновляются примерно семь раз (3:44, 3:47, 3:50, 3:53, 3:56, 3:59, 4:02). Топология, созданная в 3:54, будет подготовлена к отсчету потерь и задержки, которая вычисляется в 3:56, 3:59 и 4:02. Предположим, вы получаете предупреждение о том, что канал аварийного восстановления был неработоспособен на 3:59. Войдите в NPM и попытайтесь задать время топологии равным 3:59. NPM будет отображать топологию, созданную в 3:54. Чтобы понять последнюю известную топологию сети, сравните поля Тимепроцессед (время, когда были вычислены потери и задержка) и Трацераутекомплетедтиме (время, когда была вычислена топология). 

### <a name="what-is-the-difference-between-the-fields-e2emedianlatency-and-avghoplatencylist-in-the-networkmonitoring-table"></a>В чем разница между полями E2EMedianLatency и Авгхоплатенцилист в таблице Нетворкмониторинг
E2EMedianLatency — задержка, обновляемая каждые три минуты после статистической обработки результатов проверок TCP-связи, в то время как Авгхоплатенцилист обновляется каждые 10 минут на основе Traceroute. Чтобы понять точное время вычисления E2EMedianLatency, используйте поле Тимепроцессед. Чтобы узнать точное время, когда Traceroute завершено и обновило Авгхоплатенцилист, используйте поле Трацераутекомплетедтиме

### <a name="why-does-hop-by-hop-latency-numbers-differ-from-hoplatencyvalues"></a>Почему значения задержки прыжка по прыжкам отличаются от Хоплатенцивалуес 
Хоплатенцивалуес являются источником для конечной точки.
Например: прыжков-A, B, C. Авгхоплатенци-10, 15, 20. Это означает, что от источника до задержки = 10, от источника до B задержки = 15 и от источника к задержке C будет 20. Пользовательский интерфейс будет рассчитывать задержку прыжка B как 5 в топологии

### <a name="the-solution-shows-100-loss-but-there-is-connectivity-between-the-source-and-destination"></a>Решение показывает 100 % потерь, но связь между источником и назначением работает нормально. Что происходит?
Такая ситуация возникает, если брандмауэр узла или промежуточный брандмауэр (сетевой брандмауэр или группа безопасности сети Azure) блокирует обмен данными между агентом-источником и назначением по тому порту, который назначен для решения "Монитор производительности сети" (по умолчанию используется порт 8084, если клиент не изменял этот параметр).

* Чтобы проверить, блокирует ли брандмауэр обмен данными по используемому порту, просмотрите состояния работоспособности для узлов источника и назначения в представлении "Монитор производительности сети"->"Конфигурация"->"Узлы". 
  Если они отмечены как неработоспособные, просмотрите инструкции и примите меры по исправлению. Если узлы находятся в работоспособном состоянии, перейдите к шагу "b". ниже.
* Чтобы убедиться, что промежуточный сетевой брандмауэр или группа безопасности сети Azure не блокируют обмен данными по используемому порту, примените служебную программу PsPing стороннего разработчика в соответствии с приведенными ниже инструкциями.
  * Служебную программу PsPing можно скачать [здесь](/sysinternals/downloads/psping). 
  * Выполните приведенную ниже команду на узле источника.
    * psping-n 15 \<destination node IPAddress\> :P Ортнумбер по умолчанию NPM использует порт 8084. Если вы явным образом изменяли это значение с помощью скрипта EnableRules.ps1, введите фактически настроенный номер порта. Это проверка связи от компьютера Azure до локального узла.
* Убедитесь, что проверка проходит успешно. Если это не так, значит, трафик на этот порт блокируется промежуточным сетевым брандмауэром и (или) группой безопасности сети.
* Теперь выполните аналогичную команду на узле назначения, указав IP-адрес источника.


### <a name="there-is-loss-from-node-a-to-b-but-not-from-node-b-to-a-why"></a>Я наблюдаю потери при передаче данных из узла A в узел B, но не из B в A. В чем может быть причина?
Возможно, используются разные сетевые пути для передачи данных из A в B и из B в A. При этом значения потерь и задержек могут оказаться разными.

### <a name="why-are-all-my-expressroute-circuits-and-peering-connections-not-being-discovered"></a>Почему не выполняется обнаружение всех моих каналов ExpressRoute и пиринговых подключений?
Решение "Монитор производительности сети" теперь обнаруживает каналы ExpressRoute и пиринговые подключения во всех подписках, к которым у пользователя есть доступ. Выберите все подписки, где есть ссылки на ресурсы Express Route и включите мониторинг для каждого обнаруженного ресурса. При обнаружении частного пиринга решение "Монитор производительности сети" ищет подключенные объекты, поэтому убедитесь, что виртуальная сеть связана с пирингом. NPM не обнаруживает цепи и пиринг, которые находятся в разных клиентах, из рабочей области Log Analytics.

### <a name="the-er-monitor-capability-has-a-diagnostic-message-traffic-is-not-passing-through-any-circuit-what-does-that-mean"></a>При использовании функции мониторинга ExpressRoute отображается диагностическое сообщение "Traffic is not passing through ANY circuit" (Не передается трафик через любой канал). Что это значит?

Может сложиться такая ситуация, в которой между локальными узлами и Azure есть работоспособное подключение, но трафик не проходит через канал ExpressRoute, который должен отслеживаться решением "Монитор производительности сети". 

Причины могут быть следующими:

* Канал ExpressRoute не работает.
* Фильтры маршрутов настроены таким образом, что приоритет имеет не отслеживаемый канал ExpressRoute, а другие маршруты (например, VPN-подключение или другой канал ExpressRoute). 
* Между локальными узлами и узлами Azure, настроенными для мониторинга канала ExpressRoute, нет подключения через отслеживаемый канал ExpressRoute. Убедитесь, что вы правильно выбрали узлы и у них есть возможность связи друг с другом через канал ExpressRoute, который вы хотите отслеживать.

### <a name="why-does-expressroute-monitor-report-my-circuitpeering-as-unhealthy-when-it-is-available-and-passing-data"></a>Почему монитор ExpressRoute сообщает мой канал или Пиринг как неработоспособный, когда он доступен и передает данные.
Монитор ExpressRoute сравнивает значения производительности сети (потери, задержка и использование пропускной способности), сообщаемые агентами или службой, с пороговыми значениями, заданными во время настройки. Для канала, если используемая пропускная способность больше порогового значения, установленного в конфигурации, цепь помечается как неработоспособная. Для пиринга, если степень потери, задержка или использование пропускной способности больше порогового значения, заданного в конфигурации, пиринг помечается как неработоспособный. NPM не использует метрики или другие формы данных для выбора состояния работоспособности.

### <a name="why-does-expressroute-monitorbandwidth-utilization-report-a-value-different-from-metrics-bits-inout"></a>Почему использование Монитор'бандвидс ExpressRoute сообщает о значении, отличном от метрик битов в/с.
Для монитора ExpressRoute использование пропускной способности — это среднее значение входящей и исходящей пропускной способности за последние 20 минут, которое выражается в битах/сек. Для метрик выпусков Express Route бит/с — это точка данных в минуту. На внутреннем уровне набор данных используется одинаково, но Статистическая обработка зависит от метрик NPM и ER. Для наблюдения за минутуми и быстрыми оповещениями рекомендуется устанавливать оповещения непосредственно на метриках ER.

### <a name="while-configuring-monitoring-of-my-expressroute-circuit-the-azure-nodes-are-not-being-detected"></a>При настройке мониторинга для канала ExpressRoute не удается обнаружить узлы Azure.
Такое может случиться, если узлы Azure подключены через Operations Manager. Функция мониторинга ExpressRoute поддерживает только узлы Azure, которые подключены как прямые агенты.

### <a name="i-cannot-discover-by-expressroute-circuits-in-the-oms-portal"></a>Почему мне не удается обнаружить каналы ExpressRoute на портале OMS?
Решение "Монитор производительности сети" можно использовать и на портале Azure, и на портале OMS. Но функция обнаружения каналов в ExpressRoute Monitor работает только на портале Azure. Успешно выполнив обнаружение каналов через портал Azure, вы можете использовать остальные функции на любом из двух порталов. 

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-network-loss-as-well-as-latency-are-shown-as-na"></a>В мониторе подключений службы отображаются значения "недоступно" для всех параметров отклика службы, потери пакетов в сети и задержек.
Такое может происходить при любом из следующих условий:

* Служба не работает.
* Узел, используемый для проверки сетевого подключения к службе, не работает.
* Неверный целевой объект, введенный в конфигурации теста.
* У узла нет сетевых подключений.

### <a name="in-the-service-connectivity-monitor-capability-a-valid-service-response-time-is-shown-but-network-loss-as-well-as-latency-are-shown-as-na"></a>В мониторе подключений службы отображаются разумные значения параметров отклика службы, но значения "недоступно" для параметров потери пакетов в сети и задержек.
 Такое может происходить при любом из следующих условий:

* Если узел, используемый для проверки сетевых подключений к службе, является клиентским компьютером Windows, целевая служба блокирует запросы ICMP или сетевой брандмауэр блокирует запросы ICMP, исходящие из узла.
* В конфигурации теста не установлен флажок "Проведите измерения сети".

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-is-na-but-network-loss-as-well-as-latency-are-valid"></a>В мониторе подключений службы отображаются значения "недоступно" для параметра отклика службы, хотя потери пакетов в сети и задержки отображаются правильно.
Это может происходить, если целевая служба не является веб-приложением, а тест для нее настроен как веб-тест. Измените конфигурацию теста, выбрав для него тип "Сеть" вместо "Веб".

## <a name="miscellaneous"></a>Прочее

### <a name="is-there-a-performance-impact-on-the-node-being-used-for-monitoring"></a>Как мониторинг влияет на производительность узла, на котором он настроен?
Решение "Монитор производительности сети" будет прекращать работу, если оно использует более 5 % ресурсов ЦП узла. Благодаря этому узлы можно всегда использовать для обычных рабочих нагрузок без ущерба для производительности.

### <a name="does-npm-edit-firewall-rules-for-monitoring"></a>Изменяет ли решение "Монитор производительности сети" правила брандмауэра?
NPM создает только локальное правило брандмауэра Windows на узлах, на которых выполняется сценарий EnableRules.ps1 PowerShell, чтобы позволить агентам создавать TCP-подключения друг к другу на указанном порту. Это решение никак не изменяет все остальные правила брандмауэра и (или) правил группы безопасности сети.

### <a name="how-can-i-check-the-health-of-the-nodes-being-used-for-monitoring"></a>Как проверить работоспособность узлов, используемых для мониторинга?
Состояние работоспособности узлов, используемых для мониторинга, вы можете проверить с помощью представления "Монитор производительности сети"->"Конфигурация"->"Узлы". Если вы найдете здесь узел в неработоспособном состоянии, просмотрите сведения об ошибке и выполните предложенные меры по исправлению.

### <a name="can-npm-report-latency-numbers-in-microseconds"></a>Можно ли в решении "Монитор производительности сети" узнать значения задержки в микросекундах?
Решение "Монитор производительности сети" округляет показатели задержки в пользовательском интерфейсе до миллисекунд. Но эти данные хранятся с большей точностью (иногда до четырех десятичных разрядов).

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о решении "Монитор производительности сети" в Azure вы можете получить [в этой статье](./network-performance-monitor.md).