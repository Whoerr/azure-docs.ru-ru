---
title: Настройка журналов и доступ к ним. гибкий сервер — база данных Azure для PostgreSQL
description: Как получить доступ к журналам базы данных Azure для PostgreSQL — гибкому серверу
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: e52f0f22065d89788d08659476d14af0351cc493
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100590580"
---
# <a name="configure-and-access-logs-in-azure-database-for-postgresql---flexible-server"></a>Настройка журналов и доступ к ним в базе данных Azure для PostgreSQL-гибкого сервера

> [!IMPORTANT]
> Гибкий сервер Базы данных Azure для PostgreSQL предоставляется в режиме предварительной версии

Журналы PostgreSQL доступны на каждом узле гибкого сервера. Журналы можно поставлять на сервер хранилища или в службу аналитики. Журналы могут использоваться для обнаружения, устранения и исправления ошибок конфигурации и неоптимальной производительности.

## <a name="configure-diagnostic-settings"></a>Настройка параметров диагностики

Вы можете включить параметры диагностики для сервера Postgres с помощью портал Azure, CLI, REST API и PowerShell. Категория журнала для выбора — **постгрескллогс**.

Чтобы включить журналы ресурсов с помощью портал Azure:

1. На портале выберите *параметры диагностики* в меню навигации сервера postgres.
   
2. Выберите *Добавить параметр диагностики*.
   :::image type="content" source="media/howto-logging/diagnostic-settings.png" alt-text="Кнопка добавления параметров диагностики":::

3. Назовите этот параметр. 

4. Выберите предпочитаемую конечную точку (учетную запись хранения, концентратор событий, log Analytics). 

5. Выберите тип журнала **постгрескллогс**.
   :::image type="content" source="media/howto-logging/diagnostic-create-setting.png" alt-text="Выбор журналов PostgreSQL":::

7. Сохраните вашу настройку.

Чтобы включить журналы ресурсов с помощью PowerShell, CLI или REST API, см. статью [параметры диагностики](../../azure-monitor/essentials/diagnostic-settings.md) .

### <a name="access-resource-logs"></a>Доступ к журналам ресурсов

Способ доступа к журналам зависит от выбранной конечной точки. Сведения о службе хранилища Azure см. в статье [учетная запись хранения журналов](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage) . Сведения о концентраторах событий см. в статье [Streaming Azure Logs](../../azure-monitor/essentials/resource-logs.md#send-to-azure-event-hubs) .

Для журналов Azure Monitor журналы отправляются в выбранную рабочую область. Журналы postgres используют режим сбора **AzureDiagnostics** , поэтому их можно запрашивать из таблицы AzureDiagnostics. Поля в таблице описаны ниже. Дополнительные сведения о запросах и предупреждениях см. в статье о [запросах Azure Monitor журналов](../../azure-monitor/logs/log-query-overview.md) .

Ниже приведены запросы, которые можно попытаться приступить к работе. Можно настроить оповещения на основе запросов.

Поиск всех журналов postgres для определенного сервера за последний день

```kusto
AzureDiagnostics
| where LogicalServerName_s == "myservername"
| where Category == "PostgreSQLLogs"
| where TimeGenerated > ago(1d) 
```

Поиск всех попыток подключения, отличных от localhost

```kusto
AzureDiagnostics
| where Message contains "connection received" and Message !contains "host=127.0.0.1"
| where Category == "PostgreSQLLogs" and TimeGenerated > ago(6h)
```

В приведенном выше запросе отобразятся результаты за последние 6 часов для любого журнала postgres сервера в этой рабочей области.

## <a name="next-steps"></a>Дальнейшие шаги

- [Приступая к работе с запросами log Analytics](../../azure-monitor/logs/log-analytics-tutorial.md)
- Сведения о [концентраторах событий Azure](../../event-hubs/event-hubs-about.md)