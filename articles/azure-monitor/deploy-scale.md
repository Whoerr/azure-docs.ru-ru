---
title: Развертывание Azure Monitor в масштабе с помощью политики Azure
description: Развертывайте Azure Monitor функции в масштабе с помощью политики Azure.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/08/2020
ms.openlocfilehash: bcd56e464419312e74aec01cf22ae56f797991ad
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101731771"
---
# <a name="deploy-azure-monitor-at-scale-using-azure-policy"></a>Развертывание Azure Monitor в масштабе с помощью политики Azure
Хотя некоторые Azure Monitor компоненты настраиваются один раз или ограниченное число раз, другие необходимо повторить для каждого ресурса, который вы хотите отслеживать. В этой статье описываются методы использования политики Azure для реализации Azure Monitor в масштабе, чтобы обеспечить согласованную и точную настройку мониторинга для всех ресурсов Azure.

Например, необходимо создать параметр диагностики для всех имеющихся ресурсов Azure и для каждого создаваемого ресурса. Кроме того, необходимо установить агент и настроить его каждый раз при создании виртуальной машины. Для выполнения этих действий можно использовать такие методы, как PowerShell или CLI, так как эти методы доступны для всех функций Azure Monitor. С помощью политики Azure можно создать логику, которая будет автоматически выполнять соответствующую конфигурацию при каждом создании или изменении ресурса.


## <a name="azure-policy"></a>Политика Azure
В этом разделе приводится краткое введение в [политику Azure](../governance/policy/overview.md) , которая позволяет оценивать и применять организационные стандарты во всей подписке Azure или группе управления с минимальными усилиями. Подробные сведения см. в [документации по политике Azure](../governance/policy/overview.md) .

С помощью политики Azure можно указать требования к конфигурации для всех создаваемых ресурсов, а также определить ресурсы, которые не соответствуют требованиям, заблокировать создание ресурсов или добавить необходимую конфигурацию. Он работает путем перехвата вызовов для создания нового ресурса или изменения существующего ресурса. Он может реагировать на такие эффекты, как отклонить запрос, если он не соответствует свойствам, ожидаемым в определении политики, отмечать его для несоответствия или развертывать связанный ресурс. Существующие ресурсы можно исправить с помощью **deployIfNotExists** или **изменить** определение политики.

Политика Azure состоит из объектов, приведенных в следующей таблице. Более подробное описание каждого из них см. в разделе [объекты политики Azure](../governance/policy/overview.md#azure-policy-objects) .

| Элемент | Описание |
|:---|:---|
| Определение политики | Описывает условия соответствия ресурсов и результат выполнения условия при соблюдении условий. Это могут быть все ресурсы определенного типа или только ресурсы, соответствующие определенным свойствам. Этот результат может быть простым отметкой ресурса на соответствие или развертыванием связанного ресурса. Определения политик записываются с помощью JSON, как описано в разделе [структура определения политики Azure](../governance/policy/concepts/definition-structure.md). Эффекты описаны в разделе [Общие сведения о влиянии политики Azure](../governance/policy/concepts/effects.md).
| Инициатива политики | Группа определений политик, которые должны применяться вместе. Например, у вас может быть одно определение политики для отправки журналов ресурсов в Log Analytics рабочую область, а другой — для отправки журналов ресурсов в концентраторы событий. Создайте инициативу, включающую в себя оба определения политик, и примените эту инициативу к ресурсам, а не к отдельным определениям политик. Инициативы написаны с помощью JSON, как описано в разделе [Структура инициативы политики Azure](../governance/policy/concepts/initiative-definition-structure.md). |
| Назначение | Определение политики или инициатива не вступают в силу, пока она не будет назначена области. Например, назначьте политику группе ресурсов, чтобы применить ее ко всем ресурсам, созданным в этом ресурсе, или примените ее к подписке, чтобы применить ее ко всем ресурсам в этой подписке.  Дополнительные сведения см. в разделе [Структура назначения политики Azure](../governance/policy/concepts/assignment-structure.md). |

## <a name="built-in-policy-definitions-for-azure-monitor"></a>Встроенные определения политик для Azure Monitor
Политика Azure включает несколько предварительно созданных определений, связанных с Azure Monitor. Вы можете назначить эти определения политик существующей подписке или использовать их в качестве базиса для создания собственных пользовательских определений. Полный список встроенных политики в категории **мониторинга** см. в статье [определения встроенных политик политики Azure для Azure Monitor](.//policy-reference.md).

Чтобы просмотреть встроенные определения политик, связанные с мониторингом, выполните следующие действия.

1. Перейдите к **политике Azure** в портал Azure.
2. Выберите элемент **Определения**.
3. В качестве **типа** выберите *встроенный* и для **категории** выберите *мониторинг*.

  ![Снимок экрана со страницей "определения политик Azure" в портал Azure отображение списка определений политик для категории мониторинга и встроенного типа.](media/deploy-scale/builtin-policies.png)


## <a name="diagnostic-settings"></a>Параметры диагностики
[Параметры диагностики](essentials/diagnostic-settings.md) собираются журналы ресурсов и метрики из ресурсов Azure в несколько расположений, обычно в log Analytics рабочей области, которая позволяет анализировать данные с помощью [запросов журнала](logs/log-query-overview.md) и [оповещений журнала](alerts/alerts-log.md). Используйте политику для автоматического создания параметров диагностики при каждом создании ресурса.

Каждый тип ресурсов Azure имеет уникальный набор категорий, которые должны быть перечислены в параметре диагностики. Поэтому для каждого типа ресурсов требуется отдельное определение политики. Некоторые типы ресурсов имеют встроенные определения политик, которые можно назначить без изменения. Для других типов ресурсов необходимо создать пользовательское определение.

### <a name="built-in-policy-definitions-for-azure-monitor"></a>Встроенные определения политик для Azure Monitor
Для каждого типа ресурса существует два встроенных определения политик: один для отправки в Log Analytics рабочую область, а другой — в концентратор событий. Если требуется только одно расположение, назначьте эту политику для типа ресурса. Если требуется, назначьте оба определения политики для ресурса.

Например, на следующем рисунке показаны встроенные определения политик параметров диагностики для Data Lake Analytics.

  ![Частичный снимок экрана на странице определений политик Azure с двумя встроенными определениями политик диагностики для Data Lake Analytics.](media/deploy-scale/builtin-diagnostic-settings.png)

### <a name="custom-policy-definitions"></a>Определения настраиваемых политик
Для типов ресурсов, у которых нет встроенной политики, необходимо создать определение настраиваемой политики. Это можно сделать вручную в портал Azure путем копирования существующей встроенной политики и последующего изменения типа ресурса. Однако эффективнее создавать политику с помощью скрипта в коллекция PowerShell.

Сценарий [CREATE-аздиагполици](https://www.powershellgallery.com/packages/Create-AzDiagPolicy) создает файлы политики для определенного типа ресурсов, которые можно установить с помощью POWERSHELL или CLI. Используйте следующую процедуру, чтобы создать определение настраиваемой политики для параметров диагностики.


1. Убедитесь, что установлен [Azure PowerShell](/powershell/azure/install-az-ps) .
2. Установите скрипт с помощью следующей команды:
  
    ```azurepowershell
    Install-Script -Name Create-AzDiagPolicy
    ```

3. Запустите скрипт, используя параметры, чтобы указать, куда отправить журналы. Вам будет предложено указать подписку и тип ресурса. Например, чтобы создать определение политики, которое отправляется в Log Analytics рабочую область и концентратор событий, используйте следующую команду.

   ```azurepowershell
   Create-AzDiagPolicy.ps1 -ExportLA -ExportEH -ExportDir ".\PolicyFiles"  
   ```

4. Кроме того, в команде можно указать подписку и тип ресурса. Например, чтобы создать определение политики, которое отправляет в Log Analytics рабочую область и концентратор событий для SQL Server баз данных Azure, используйте следующую команду.

   ```azurepowershell
   Create-AzDiagPolicy.ps1 -SubscriptionID xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -ResourceType Microsoft.Sql/servers/databases  -ExportLA -ExportEH -ExportDir ".\PolicyFiles"  
   ```

5. Скрипт создает отдельные папки для каждого определения политики, каждый из которых содержит три файла с именем azurepolicy.jsв azurepolicy.rules.js, azurepolicy.parameters.jsна. Если вы хотите создать политику вручную в портал Azure, можно скопировать и вставить содержимое azurepolicy.js, так как оно включает в себя полное определение политики. Используйте два других файла с PowerShell или CLI, чтобы создать определение политики из командной строки.

    В следующих примерах показано, как установить определение политики из PowerShell и CLI. Каждый включает метаданные, чтобы указать категорию **мониторинга** для группировки нового определения политики с помощью встроенных определений политик.

      ```azurepowershell
      New-AzPolicyDefinition -name "Deploy Diagnostic Settings for SQL Server database to Log Analytics workspace" -policy .\Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.rules.json -parameter .\Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.parameters.json -mode All -Metadata '{"category":"Monitoring"}'
      ```

      ```azurecli
      az policy definition create --name 'deploy-diag-setting-sql-database--workspace' --display-name 'Deploy Diagnostic Settings for SQL Server database to Log Analytics workspace'  --rules 'Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.rules.json' --params 'Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.parameters.json' --subscription 'AzureMonitor_Docs' --mode All
      ```

### <a name="initiative"></a>Инициатива
Чтобы не создавать назначение для каждого определения политики, распространенной стратегией является создание инициативы, включающей определения политик для создания параметров диагностики для каждой службы Azure. Создайте назначение между инициативой и группой управления, подпиской или группой ресурсов в зависимости от того, как вы управляете средой. Эта стратегия обеспечивает следующие преимущества:

- Создайте одно назначение для инициативы вместо нескольких назначений для каждого типа ресурса. Используйте одну и ту же инициативу для нескольких групп мониторинга, подписок или групп ресурсов.
- Измените инициативу, когда необходимо добавить новый тип ресурса или назначение. Например, начальным требованием может быть отправка данных только в Log Analytics рабочую область, но позже необходимо добавить концентратор событий. Измените инициативу, а не создавайте новые назначения.

Сведения о создании инициативы см. в статье [Создание и назначение определения инициативы](../governance/policy/tutorials/create-and-manage.md#create-and-assign-an-initiative-definition) . Предлагаются следующие рекомендации.

- Задайте **категорию** **мониторинга** , чтобы сгруппировать ее со связанными встроенными и пользовательскими определениями политик.
- Вместо указания сведений для рабочей области Log Analytics и концентратора событий для определения политики, входящей в состав инициативы, используйте общий параметр инициативы. Это позволяет легко указать общее значение для всех определений политик и при необходимости изменить это значение.

![Определение инициативы](media/deploy-scale/initiative-definition.png)

### <a name="assignment"></a>Назначение 
Назначьте инициативу группе управления Azure, подписке или группе ресурсов в зависимости от области наблюдаемых ресурсов. [Группа управления](../governance/management-groups/overview.md) особенно полезна для политики области, особенно если ваша организация имеет несколько подписок.

![Снимок экрана параметров вкладки "основы" в разделе "назначение инициативы" параметров диагностики для Log Analytics рабочей области в портал Azure.](media/deploy-scale/initiative-assignment.png)

С помощью параметров инициативы можно указать рабочую область или любые другие сведения один раз для всех определений политик в инициативе. 

![Параметры инициативы](media/deploy-scale/initiative-parameters.png)

### <a name="remediation"></a>Серверы
Инициатива будет применена к каждой виртуальной машине в процессе ее создания. [Задача исправления](../governance/policy/how-to/remediate-resources.md) развертывает определения политик в рамках инициативы в отношении существующих ресурсов, поэтому позволяет создавать параметры диагностики для всех уже созданных ресурсов. При создании назначения с помощью портал Azure имеется возможность одновременного создания задачи исправления. Дополнительные сведения об исправлении см. в статье исправление [несоответствующих ресурсов с помощью политики Azure](../governance/policy/how-to/remediate-resources.md) .

![Исправление инициатив](media/deploy-scale/initiative-remediation.png)


## <a name="vm-insights"></a>Аналитика ВМ
[VM Insights](vm/vminsights-overview.md) — это основное средство в Azure Monitor для наблюдения за виртуальными машинами. При включении функции "аналитика VM" устанавливаются агент Log Analytics и агент зависимостей. Вместо выполнения этих задач вручную используйте политику Azure, чтобы обеспечить настройку каждой виртуальной машины при ее создании.

> [!NOTE]
> Функция VM Insights включает в себя функцию, именуемую " **покрытие политики VM Insights** ", которая позволяет обнаруживать и исправлять несоответствующие требованиям виртуальные машины в вашей среде. Эту функцию можно использовать вместо непосредственной работы с политикой Azure для виртуальных машин Azure и гибридных виртуальных компьютеров, подключенных к службе "Дуга Azure". Для масштабируемых наборов виртуальных машин Azure необходимо создать назначение с помощью политики Azure.
 

В состав VM Insights входят следующие встроенные инициативы, устанавливающие оба агента для обеспечения полного мониторинга. 

|Имя |Описание |
|:---|:---|
|Включение аналитики виртуальной машины | Устанавливает агент Log Analytics и агент зависимостей на виртуальные машины Azure и гибридные виртуальные машины, подключенные к службе "Дуга Azure". |
|Включение Azure Monitor для масштабируемых наборов виртуальных машин | Устанавливает агент Log Analytics и агент зависимостей в масштабируемом наборе виртуальных машин Azure. |


### <a name="virtual-machines"></a>Виртуальные машины
Вместо того чтобы создавать назначения для этих инициатив с помощью интерфейса политики Azure, служба VM Insights включает в себя функцию, позволяющую проверять количество виртуальных машин в каждой области, чтобы определить, применена ли инициатива. Затем можно настроить рабочую область и создать все необходимые назначения с помощью этого интерфейса.

Дополнительные сведения об этом процессе см. в статье [Включение аналитики виртуальной машины с помощью политики Azure](./vm/vminsights-enable-policy.md).

![Политика VM Insights](media/deploy-scale/vminsights-policy.png)

### <a name="virtual-machine-scale-sets"></a>Масштабируемые наборы виртуальных машин
Чтобы включить мониторинг для масштабируемых наборов виртуальных машин с помощью политики Azure, назначьте для группы управления Azure, подписки или группы ресурсов **функцию включить Azure Monitor для масштабируемых наборов виртуальных машин** в зависимости от области ресурсов, которые нужно отслеживать. [Группа управления](../governance/management-groups/overview.md) особенно полезна для политики области, особенно если ваша организация имеет несколько подписок.

![Снимок экрана со страницей "назначение инициативы" в портал Azure. Определение инициативы настроено для включения Azure Monitor для масштабируемых наборов виртуальных машин.](media/deploy-scale/virtual-machine-scale-set-assign-initiative.png)

Выберите рабочую область, куда будут отправляться данные. В этой рабочей области должно быть установлено решение *вминсигхтс* , как описано в разделе [настройка рабочей области log Analytics для VM Insights](vm/vminsights-configure-workspace.md).

![Выбор рабочей области](media/deploy-scale/virtual-machine-scale-set-workspace.png)

Создайте задачу исправления, если у вас уже есть масштабируемый набор виртуальных машин, которому необходимо назначить эту политику.

![Задача исправления](media/deploy-scale/virtual-machine-scale-set-remediation.png)

### <a name="log-analytics-agent"></a>Агент Log Analytics
В некоторых случаях требуется установить агент Log Analytics, но не агент зависимостей. Встроенная инициатива только для агента отсутствует, но ее можно создать на основе встроенных определений политик, предоставляемых VM Insights.

> [!NOTE]
> Не придется развертывать агент зависимостей самостоятельно, так как он требует от агента Log Analytics доставки своих данных в Azure Monitor.


|Имя |Описание |
|-----|------------|
|Аудит Log Analytics развертывания агента — образ виртуальной машины (ОС), который не был в списке |Сообщает о несоответствии виртуальных машин, если образ виртуальной машины (ОС) не определен в списке и агент не установлен. |
|Развертывание агента Log Analytics для виртуальных машин Linux |Развертывание агента Log Analytics для виртуальных машин Linux, если образ виртуальной машины (ОС) определен в списке и агент не установлен. |
|Развертывание агента Log Analytics для виртуальных машин Windows |Развертывание агента Log Analytics для виртуальных машин Windows, если образ виртуальной машины (ОС) определен в списке и агент не установлен. |
| [Предварительная версия]: Log Analytics агент должен быть установлен на компьютерах Arc Azure в Linux |Сообщает о гибридных машинах Azure в качестве несоответствующих для виртуальных машин Linux, если образ виртуальной машины (ОС) определен в списке, а агент не установлен. |
| [Предварительная версия]: на компьютерах Arc Windows Azure должен быть установлен агент Log Analytics |Сообщает о гибридных машинах Azure в качестве несоответствующих для виртуальных машин Windows, если образ виртуальной машины (ОС) определен в списке, а агент не установлен. |
| [Предварительная версия]: развертывание агента Log Analytics на компьютерах ARC в Azure на платформе Linux |Развертывание агента Log Analytics для гибридных машин Azure на базе Linux, если образ виртуальной машины (ОС) определен в списке, а агент не установлен. |
| [Предварительная версия]: развертывание агента Log Analytics на компьютерах Arc Windows Azure |Развертывание агента Log Analytics для гибридных компьютеров с Windows Azure Arc, если образ виртуальной машины (ОС) определен в списке, а агент не установлен. |
|Аудит развертывания агента зависимостей в масштабируемых наборах виртуальных машин — образ виртуальной машины (ОС), который не был в списке |Сообщает о том, что масштабируемый набор виртуальных машин не соответствует требованиям, если образ ВИРТУАЛЬНОЙ машины (ОС) не определен в списке и агент не установлен. |
|Аудит развертывания агента Log Analytics в масштабируемых наборах виртуальных машин — образ виртуальной машины (ОС), который не был в списке |Сообщает о том, что масштабируемый набор виртуальных машин не соответствует требованиям, если образ ВИРТУАЛЬНОЙ машины (ОС) не определен в списке и агент не установлен. |
|Развертывание агента Log Analytics для масштабируемых наборов виртуальных машин Linux |Развертывание агента Log Analytics для масштабируемых наборов виртуальных машин Linux, если образ виртуальной машины (ОС) определен в списке, а агент не установлен. |
|Развертывание агента Log Analytics для масштабируемых наборов виртуальных машин Windows |Развертывание агента Log Analytics для масштабируемых наборов виртуальных машин Windows, если образ ВИРТУАЛЬНОЙ машины (ОС) определен в списке, а агент не установлен. |


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше о [политике Azure](../governance/policy/overview.md).
- Узнайте больше о [параметрах диагностики](essentials/diagnostic-settings.md).