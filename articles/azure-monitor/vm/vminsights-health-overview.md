---
title: Работоспособность гостевых систем VM Insights (Предварительная версия)
description: Обзор функции работоспособности в службе "аналитика ВМ", в том числе сведения о том, как просматривать работоспособность виртуальных машин и отправлять оповещения при неработоспособности виртуальной машины.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/27/2020
ms.openlocfilehash: 3db6c2f4da28bba2d12aacc90b2fa8e420aa6fbf
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101707461"
---
# <a name="vm-insights-guest-health-preview"></a>Работоспособность гостевых систем VM Insights (Предварительная версия)
Работоспособность гостевых систем VM Insights позволяет просматривать работоспособность виртуальных машин на основе набора измерений производительности, выборке с регулярными интервалами от гостевой операционной системы. Вы можете быстро проверить работоспособность всех виртуальных машин в подписке или группе ресурсов, выполнить детализацию подробных сведений о работоспособности определенной виртуальной машины или заблаговременно получать уведомления о неработоспособности виртуальной машины. 

## <a name="enable-virtual-machine-health"></a>Включить работоспособность виртуальной машины
Дополнительные сведения о включении функции работоспособности гостевой виртуальной машины и подключении виртуальных машин см. в разделе [Включение работоспособности гостевых компонентов VM Insights (Предварительная версия)](vminsights-health-enable.md) .

## <a name="pricing"></a>Цены
Нет прямой платы за функцию работоспособности гостевой системы, но есть затраты на прием и хранение данных о состоянии работоспособности в рабочей области Log Analytics. Все данные хранятся в таблице *хеалсстатечанжеевент* . Дополнительные сведения о моделях ценообразования и затратах см. в статье [Управление использованием и затратами с помощью Azure Monitor журналов](../logs/manage-cost-storage.md) .

## <a name="view-virtual-machine-health"></a>Просмотр работоспособности виртуальной машины
Столбец **работоспособность гостевой виртуальной машины** на странице **Приступая к работе** позволяет быстро просмотреть сведения о работоспособности каждой виртуальной машины в определенной подписке или группе ресурсов. Текущая работоспособность каждой виртуальной машины отображается, а значки для каждой группы показывают количество виртуальных машин в каждом состоянии в этой группе.

[![Страница "Приступая к работе" с работоспособностью гостевой виртуальной машины](media/vminsights-health-overview/get-started-page.png)](media/vminsights-health-overview/get-started-page.png#lightbox)


## <a name="monitors"></a>Мониторы
Щелкните состояние работоспособности виртуальной машины, чтобы просмотреть подробные сведения о состоянии каждого из мониторов. Каждый монитор измеряет работоспособность определенного компонента. Общая работоспособность виртуальной машины определяется работоспособностью отдельных мониторов. 

![Пример мониторов](media/vminsights-health-overview/monitors.png)

В следующей таблице перечислены совокупные и базовые мониторы, доступные в настоящее время для каждой виртуальной машины. 

| Монитор | Тип | Описание |
|:---|:---|:---|
| загрузка ЦП; | Unit | Процент использования процессора. |
| Файловые системы | Статистическое | Совокупное состояние работоспособности всех файловых систем на виртуальной машине Linux. |
| Файловая система  | Статистическое | Работоспособность каждой отдельной файловой системы на виртуальной машине Linux. Имя монитора — это имя файловой системы. |
| Свободное пространство | Unit | Процент свободного места в файловой системе. |
| Логический диск | Статистическое | Совокупное состояние работоспособности всех логических дисков на виртуальной машине Windows. |
| Логический диск  | Статистическое | Работоспособность каждого отдельного логического диска на виртуальной машине Windows. Имя монитора — это имя диска. |
| Память | Статистическое | Совокупное состояние работоспособности памяти на виртуальной машине. |
| Объем доступной памяти | Unit | Доступный мегабайт на виртуальной машине. |

## <a name="health-states"></a>Состояния работоспособности
Каждый монитор выбирает значения в агенте каждую минуту и сравнивает значения выборки с критериями для каждого состояния работоспособности. Если критерий для определенного состояния имеет значение true, то монитору присваивается это состояние. Если ни одно из критериев не имеет значения true, то монитору присваивается работоспособное состояние. Данные отправляются от агента в Azure Monitor каждые три минуты или сразу же при изменении состояния.

Каждый монитор имеет лукбакк окно и анализирует все примеры, собранные в течение этого времени. Например, монитор может иметь лукбакк окно, в течение которого 240 секунд ищет максимальное значение по крайней мере из двух значений выборки, но не более чем за последние 3. Хотя значения вычисляются по обычному темпу, число выборки в конкретном окне может незначительно отличаться в случае нарушения работы агента.

Мониторы имеют потенциальные состояния работоспособности в следующей таблице и будут находиться в одной и только в одном заданном времени. При инициализации монитора он начинает работу в работоспособном состоянии.

| Состояние работоспособности | Описание |
|:---|:---|
| Работоспособно  | Сейчас монитор не превышает порог предупреждения или критическое значение. |
| Предупреждение  | Монитор превысил пороговое значение предупреждения (если определено). |
| Критически важно | Монитор превысил критическое пороговое значение (если определено). |
| Неизвестно  | Собрано недостаточно данных для определения состояния работоспособности. |
| Выключено | Монитор в настоящее время отключен. |
| Нет     | Монитор запущен, но еще не оценен или не отслеживается, а объект больше не существует. |



Существует два типа мониторов, как показано в следующей таблице.

| Монитор | Описание |
|:---|:---|
| Монитор модуля | Измеряет определенный аспект ресурса или приложения. Это может быть проверка счетчика производительности для определения производительности ресурса или его доступности. |
| Составной монитор | Группирует несколько мониторов, чтобы предоставить сведения о едином агрегированном состоянии работоспособности. Составной монитор может содержать один или несколько базовых мониторов и другие составные мониторы. |


  
### <a name="health-rollup-policy"></a>Политика сводного показателя работоспособности
Состояние работоспособности виртуальной машины определяется сводкой состояния работоспособности из каждой единицы и составных мониторов. Каждый составной монитор использует наихудшую политику работоспособности состояния, где состояние составного монитора соответствует состоянию дочернего монитора с наихудшим состоянием работоспособности.  

В следующем примере монитор **свободного места** находится в критическом состоянии, в котором роли вплоть до статистических данных для своего экземпляра, а затем в **файловые системы**. Несмотря на то, что **Загрузка ЦП** в состоянии предупреждения, виртуальная машина имеет значение критическое, так как это наихудшее состояние. Если свободное пространство должно возвращаться к работоспособному состоянию, то виртуальная машина изменится на состояние предупреждения, так как использование ЦП станет монитором с наихудшим состоянием.

![Пример сводного монитора работоспособности](media/vminsights-health-overview/health-rollup-example.png)


## <a name="monitor-details"></a>Сведения о мониторе
Выберите монитор, чтобы просмотреть его сведения, включая следующие вкладки.

**Обзор** содержит описание монитора, время последнего его вычисления и значения, выбранные для определения текущего состояния работоспособности. Количество выборок может меняться в зависимости от определения монитора и изменчивости значений.

[![Обзор сведений о мониторе](media/vminsights-health-overview/monitor-details-overview.png)](media/vminsights-health-overview/monitor-details-overview.png#lightbox)

**Журнал** содержит историю изменений состояния для данного монитора. Вы можете развернуть любое изменение состояния для просмотра значений, вычисленных для определения состояния работоспособности. Щелкните **Просмотр конфигурации применено** , чтобы просмотреть конфигурацию монитора во время изменения состояния работоспособности.



[![Журнал подробностей монитора](media/vminsights-health-overview/monitor-details-history.png)](media/vminsights-health-overview/monitor-details-history.png#lightbox)

### <a name="configuration"></a>Конфигурация
Просмотр и изменение конфигурации монитора для выбранной виртуальной машины. Дополнительные сведения см. [в статье Настройка мониторинга в работоспособности гостевых систем VM Insights (Предварительная версия)](vminsights-health-enable.md) .

[![Конфигурация сведений монитора](media/vminsights-health-overview/monitor-details-configuration.png)](media/vminsights-health-overview/monitor-details-configuration.png#lightbox)




## <a name="next-steps"></a>Дальнейшие действия

- [Включите работоспособность гостевых систем в VM Insights и подключите агенты.](vminsights-health-enable.md)
- [Настройка мониторов с помощью портал Azure.](vminsights-health-configure.md)
- [Настройка мониторов с помощью правил сбора данных.](vminsights-health-configure-dcr.md)