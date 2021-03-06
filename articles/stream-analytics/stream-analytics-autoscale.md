---
title: Автомасштабирование заданий Stream Analytics
description: В этой статье описано, как автоматически масштабировать задание Stream Analytics на основе предопределенного расписания или значений метрик заданий.
author: sidramadoss
ms.author: sidram
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/03/2020
ms.openlocfilehash: a8e089e302e9d40c69cf7ff2a3480c17894e1463
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98016292"
---
# <a name="autoscale-stream-analytics-jobs-using-azure-automation"></a>Автоматическое масштабирование заданий Stream Analytics с помощью службы автоматизации Azure

Затраты на Stream Analyticsные задания можно оптимизировать, настроив Автомасштабирование. Автомасштабирование увеличивает или уменьшает количество единиц потоковой передачи задания (SUs) в соответствии с изменениями во входной нагрузке. Вместо переподготовки задания можно масштабировать его по мере необходимости. Существует два способа настройки заданий для автомасштабирования:
1. **Предварительно определите расписание** при наличии предсказуемой входной нагрузки. Например, вы ожидаете более высокую частоту входных событий во время постоянную и хотите, чтобы задание выполнялось с большим количеством служб SUs.
2. **Активируйте операции увеличения и уменьшения масштаба на основе метрик заданий** , если у вас нет предсказуемой входной нагрузки. Вы можете динамически изменять число серверов на основе метрик задания, таких как количество входных событий или событий ввода, регистрируемых в журнале.

## <a name="prerequisites"></a>Предварительные требования
Прежде чем начать настройку автомасштабирования для задания, выполните следующие действия.
1. Задание оптимизировано для [параллельной топологии](./stream-analytics-parallelization.md). Если вы можете изменить масштаб задания во время его выполнения, задание будет иметь параллельную топологию и может быть настроено для автомасштабирования.
2. [Создайте учетную запись службы автоматизации Azure](../automation/automation-create-standalone-account.md) с включенным параметром "RunAsAccount". Эта учетная запись должна иметь разрешения на управление заданиями Stream Analytics.

## <a name="set-up-azure-automation"></a>Настройка службы автоматизации Azure
### <a name="configure-variables"></a>Настройка переменных
Добавьте следующие переменные в учетную запись службы автоматизации Azure. Эти переменные будут использоваться в модулях Runbook, описанных в следующих шагах.

| Имя | Тип | Значение |
| --- | --- | --- |
| **jobName** | Строка | Имя задания Stream Analytics, которое необходимо автоматически масштабировать. |
| **resourceGroupName** | Строка | Имя группы ресурсов, в которой содержится задание. |
| **subId** | Строка | Идентификатор подписки, в которой содержится задание. |
| **инкреаседсу** | Целое число | Большее значение SU, до которого должно масштабироваться задание в расписании. Это значение должно быть одним из допустимых параметров SU, отображаемых в параметрах **масштабирования** задания во время его выполнения. |
| **декреаседсу** | Целое число | Наименьшее значение SU, до которого должно масштабироваться задание в расписании. Это значение должно быть одним из допустимых параметров SU, отображаемых в параметрах **масштабирования** задания во время его выполнения. |
| **макссу** | Целое число | Максимальное значение SU, по которому должно масштабироваться задание, в шагах при автоматическом масштабировании по нагрузке. Это значение должно быть одним из допустимых параметров SU, отображаемых в параметрах **масштабирования** задания во время его выполнения. |
| **минсу** | Целое число | Минимальное значение SU, по которому должно масштабироваться задание, в шагах при автоматическом масштабировании по нагрузке. Это значение должно быть одним из допустимых параметров SU, отображаемых в параметрах **масштабирования** задания во время его выполнения. |

![Добавление переменных в службу автоматизации Azure](./media/autoscale/variables.png)

### <a name="create-runbooks"></a>Создание модулей runbook
Следующим шагом является создание двух модулей Runbook PowerShell. Один для увеличения масштаба, а другой для операций масштабирования.
1. В учетной записи службы автоматизации Azure перейдите в раздел **модули Runbook** в разделе **Автоматизация процессов**  и выберите **создать Runbook**.
2. Назовите первый модуль Runbook *скалеупрунбук* с типом, установленным в PowerShell. Используйте [сценарий PowerShell скалеупрунбук](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/ScaleUpRunbook.ps1) , доступный в GitHub. Сохраните и опубликуйте его.
3. Создайте еще один модуль Runbook с именем *скаледовнрунбук* и типом PowerShell. Используйте [сценарий PowerShell скаледовнрунбук](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/ScaleDownRunbook.ps1) , доступный в GitHub. Сохраните и опубликуйте его.

![Автомасштабирование модулей Runbook в службе автоматизации Azure](./media/autoscale/runbooks.png)

Теперь у вас есть модули Runbook, которые могут автоматически запускать операции увеличения масштаба и уменьшения масштаба для задания Stream Analytics. Эти модули Runbook могут быть активированы с помощью заранее определенного расписания или могут быть заданы динамически на основе метрик заданий.

## <a name="autoscale-based-on-a-schedule"></a>Автомасштабирование на основе расписания
Служба автоматизации Azure позволяет настроить расписание для активации модулей Runbook.
1. В учетной записи службы автоматизации Azure выберите **расписания** в разделе **Общие ресурсы**. Затем выберите **Добавить расписание**.
2. Например, можно создать два расписания. Один из которых представляет, когда требуется увеличить масштаб задания, а другое — в том случае, если требуется уменьшить масштаб задания. Для этих расписаний можно определить повторение.

   ![Расписания в службе автоматизации Azure](./media/autoscale/schedules.png)

3. Откройте **скалеупрунбук** и выберите **расписания** в разделе **ресурсы**. Затем можно связать модуль Runbook с расписанием, созданным на предыдущих шагах. С одним модулем Runbook может быть связано несколько расписаний, которые могут быть полезны, если необходимо выполнить одну операцию масштабирования в разное время суток.

![Планирование модулей Runbook в службе автоматизации Azure](./media/autoscale/schedulerunbook.png)

1. Повторите предыдущий шаг для **скаледовнрунбук**.

## <a name="autoscale-based-on-load"></a>Автомасштабирование на основе нагрузки
Возможны случаи, когда невозможно предсказать входную нагрузку. В таких случаях более оптимально увеличивать или уменьшать масштаб по шагам в пределах минимальной и максимальной границы. Вы можете настроить правила генерации оповещений в заданиях Stream Analytics, чтобы активировать модули Runbook, когда метрики задания выходят за пределы порогового значения.
1. В учетной записи службы автоматизации Azure создайте еще две целочисленные переменные с именами **минсу** и **макссу**. Здесь задаются границы, в которых задание будет масштабироваться по шагам.
2. Создайте два новых модуля Runbook. Вы можете использовать [сценарий PowerShell степскалеуп](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/StepScaleUp.ps1) , который увеличивает количество копий задания с шагом до **макссу** значения. Вы также можете использовать [сценарий PowerShell степскаледовн](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/StepScaleDown.ps1) , который сокращает количество своих заданий в шагах до тех пор, пока не будет достигнуто значение **минсу** . Кроме того, вы можете использовать модули Runbook из предыдущего раздела, если у вас есть конкретные значения SU, которые нужно масштабировать.
3. В Stream Analytics задании выберите **Правила оповещений** в разделе **мониторинг**. 
4. Создайте две группы действий. Один для использования при операции увеличения масштаба и другой для операции уменьшения масштаба. Выберите **Управление действиями** , а затем щелкните **Добавить группу действий**. 
5. Заполните обязательные поля. Выберите **Runbook службы автоматизации** при выборе **типа действия**. Выберите модуль Runbook, который нужно активировать при срабатывании оповещения. Затем создайте группу действий.

   ![Создание группы действий](./media/autoscale/create-actiongroup.png)
6. Создайте [**новое правило генерации оповещений**](./stream-analytics-set-up-alerts.md#set-up-alerts-in-the-azure-portal) в задании. Укажите условие на основе выбранной метрики. События [ *ввода*, *Использование SU%* или *входные события входа в систему*](./stream-analytics-monitoring.md#metrics-available-for-stream-analytics) являются рекомендуемыми метриками для определения логики автоматического масштабирования. Также рекомендуется использовать *детализацию статистической обработки* и *частоту вычисления* в течение 1 минут при активации операций масштабирования. Это гарантирует, что у вашего задания достаточно ресурсов для работы с большими пиковыми нагрузками во входном томе.
7. Выберите группу действий, созданную на последнем шаге, и Создайте оповещение.
8. Повторите шаги с 2 по 4 для всех дополнительных операций масштабирования, которые необходимо активировать на основе условий метрик заданий.

Рекомендуется выполнять масштабируемые тесты перед выполнением задания в рабочей среде. При тестировании задания в соответствии с различными входными нагрузками вы получаете представление о том, сколько серверов должно быть в вашем задании для различной пропускной способности ввода. Это может сообщить условиям, определенным в правилах генерации оповещений, которые активируют операции увеличения масштаба и уменьшения масштаба. 

## <a name="next-steps"></a>Дальнейшие действия
* [Leverage query parallelization in Azure Stream Analytics](stream-analytics-parallelization.md) (Использование параллелизации запросов в Azure Stream Analytics)
* [Масштабирование заданий Azure Stream Analytics для повышения пропускной способности](stream-analytics-scale-jobs.md)