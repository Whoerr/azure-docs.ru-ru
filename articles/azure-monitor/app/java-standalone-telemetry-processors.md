---
title: Обработчики данных телеметрии (Предварительная версия) — Azure Monitor Application Insights для Java
description: Узнайте, как настроить обработчики телеметрии в Azure Monitor Application Insights для Java.
ms.topic: conceptual
ms.date: 10/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 35e53454e5b2c6265082bbedb4a8b60e82df7191
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734576"
---
# <a name="telemetry-processors-preview---azure-monitor-application-insights-for-java"></a>Обработчики данных телеметрии (Предварительная версия) — Azure Monitor Application Insights для Java

> [!NOTE]
> Функции обработчиков данных телеметрии находятся на этапе предварительной версии.

Агент Java 3,0 для Application Insights может обрабатывать данные телеметрии перед экспортом данных.

Ниже приведены некоторые варианты использования обработчиков данных телеметрии.
 * Создание конфиденциальных данных.
 * Условно добавлять пользовательские измерения.
 * Обновите имя диапазона, которое используется для агрегирования сходных данных телеметрии в портал Azure.
 * Перетащите атрибуты span, чтобы контролировать стоимость приема.

## <a name="terminology"></a>Терминология

Прежде чем приступить к изучению обработчиков данных телеметрии, необходимо ознакомиться с *диапазоном* терминов. Диапазон — это общий термин для:

* Входящий запрос.
* Исходящая зависимость (например, удаленный вызов другой службы).
* Внутрипроцессный набор зависимостей (например, работа, выполняемая подкомпонентами службы).

Для обработчиков телеметрии эти компоненты охватывают следующие значения:

* Имя
* Атрибуты

Имя диапазона является основным отображением запросов и зависимостей в портал Azure. Атрибуты span представляют стандартные и пользовательские свойства заданного запроса или зависимости.

## <a name="telemetry-processor-types"></a>Типы обработчиков данных телеметрии

В настоящее время два типа обработчиков данных телеметрии являются атрибутами обработчиков и охватывают процессоры.

Обработчик атрибутов может вставлять, обновлять, удалять или изменять хэш-атрибуты.
Для извлечения одного или нескольких новых атрибутов из существующего атрибута также можно использовать регулярное выражение.

Процессор SPAN может обновить имя телеметрии.
Также можно использовать регулярное выражение для извлечения одного или нескольких новых атрибутов из имени диапазона.

> [!NOTE]
> В настоящее время обработчики данных телеметрии обрабатывают только атрибуты типа String. Они не обрабатывают атрибуты типа Boolean или Number.

## <a name="getting-started"></a>Начало работы

Для начала создайте файл конфигурации с именем *applicationinsights.jsв*. Сохраните его в том же каталоге, что и *applicationinsights-Agent- \* . jar*. Используйте следующий шаблон.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        ...
      },
      {
        "type": "attribute",
        ...
      },
      {
        "type": "span",
        ...
      }
    ]
  }
}
```

## <a name="include-criteria-and-exclude-criteria"></a>Включить критерии и исключить критерии

Оба обработчика атрибутов и процессоров охватов поддерживают необязательные `include` `exclude` критерии и.
Процессор применяется только к диапазонам, соответствующим его `include` критериям (если он предоставляется) _, и_ не соответствует его `exclude` критериям (если он указан).

Чтобы настроить этот параметр, в разделе `include` или `exclude` (или в обоих случаях) укажите по меньшей мере один `matchType` и либо `spanNames` `attributes` .
Конфигурация включения и исключения позволяет использовать более одного заданного условия.
Все указанные условия должны иметь значение true, чтобы привести к совпадению. 

* **Обязательное поле**: `matchType` управляет `spanNames` интерпретацией элементов в массивах и `attributes` массивах. Возможные значения: `regexp` и `strict`. 

* **Необязательные поля**: 
    * `spanNames` должен совпадать по крайней мере с одним из элементов. 
    * `attributes` Задает список атрибутов для сопоставления. Все эти атрибуты должны точно совпадать, чтобы привести к совпадению.
    
> [!NOTE]
> Если `include` указаны и `exclude` , и, то `include` свойства проверяются до `exclude` проверки свойств.

### <a name="sample-usage"></a>Пример использования

```json
"processors": [
  {
    "type": "attribute",
    "include": {
      "matchType": "strict",
      "spanNames": [
        "spanA",
        "spanB"
      ]
    },
    "exclude": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "redact_trace",
          "value": "false"
        }
      ]
    },
    "actions": [
      {
        "key": "credit_card",
        "action": "delete"
      },
      {
        "key": "duplicate_key",
        "action": "delete"
      }
    ]
  }
]
```
Дополнительные сведения см. в разделе [примеры обработчика данных телеметрии](./java-standalone-telemetry-processors-examples.md).

## <a name="attribute-processor"></a>Обработчик атрибутов

Обработчик атрибутов изменяет атрибуты диапазона. Он может поддерживать возможность включать или исключать диапазоны. Он принимает список действий, выполняемых в порядке, указанном в файле конфигурации. Процессор поддерживает следующие действия:

- `insert`
- `update`
- `delete`
- `hash`
- `extract`
### `insert`

`insert`Действие вставляет новый атрибут в диапазоны, где ключ еще не существует.   

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "value1",
        "action": "insert"
      }
    ]
  }
]
```
Для `insert` действия требуются следующие параметры:
* `key`
* Либо `value``fromAttribute`
* `action`: `insert`

### `update`

`update`Действие обновляет атрибут в диапазонах, где ключ уже существует.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "newValue",
        "action": "update"
      }
    ]
  }
]
```
Для `update` действия требуются следующие параметры:
* `key`
* Либо `value``fromAttribute`
* `action`: `update`


### `delete` 

`delete`Действие удаляет атрибут из диапазона.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "delete"
      }
    ]
  }
]
```
Для `delete` действия требуются следующие параметры:
* `key`
* `action`: `delete`

### `hash`

`hash`Хэш действия (SHA1) существующего значения атрибута.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "hash"
      }
    ]
  }
]
```
Для `hash` действия требуются следующие параметры:
* `key`
* `action`: `hash`

### `extract`

> [!NOTE]
> Эта `extract` функция доступна только в версии 3.0.2 и более поздних версиях.

`extract`Действие извлекает значения с помощью правила регулярного выражения из входного ключа в целевые ключи, которые указывает правило. Если целевой ключ уже существует, он переопределяется. Это действие работает как параметр [процессора span](#extract-attributes-from-the-span-name) `toAttributes` , где существующий атрибут является источником.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "pattern": "<regular pattern with named matchers>",
        "action": "extract"
      }
    ]
  }
]
```
Для `extract` действия требуются следующие параметры:
* `key`
* `pattern`
* `action`: `extract`

Дополнительные сведения см. в разделе [примеры обработчика данных телеметрии](./java-standalone-telemetry-processors-examples.md).

## <a name="span-processor"></a>Охват процессора

Процессор span изменяет либо имя или атрибуты диапазона, исходя из имени диапазона. Он может поддерживать возможность включать или исключать диапазоны.

### <a name="name-a-span"></a>Назовите диапазон

Для `name` раздела требуется `fromAttributes` параметр. Значения этих атрибутов используются для создания нового имени, сцепленного в порядке, указанном конфигурацией. Процессор изменит имя диапазона только в том случае, если все эти атрибуты находятся в диапазоне.

Этот `separator` параметр является необязательным. Этот параметр является строкой. Он указан для разбиения значений.
> [!NOTE]
> Если переименование зависит от обработчика атрибутов для изменения атрибутов, убедитесь, что процессор span указан после обработчика атрибутов в спецификации конвейера.

```json
"processors": [
  {
    "type": "span",
    "name": {
      "fromAttributes": [
        "attributeKey1",
        "attributeKey2",
      ],
      "separator": "::"
    }
  }
] 
```

### <a name="extract-attributes-from-the-span-name"></a>Извлечь атрибуты из имени диапазона

В `toAttributes` разделе перечислены регулярные выражения, соответствующие именам диапазонов. Он извлекает атрибуты на основе частей выражения.

`rules`Параметр является обязательным. Этот параметр перечисляет правила, используемые для извлечения значений атрибутов из имени диапазона. 

Значения в имени диапазона заменяются извлеченными именами атрибутов. Каждое правило в списке является строкой шаблона регулярного выражения (Regex). 

Вот как значения заменяются извлеченными именами атрибутов:

1. Имя диапазона проверяется на соответствие регулярному выражению. 
1. Если регулярное выражение соответствует, все именованные части выражения Regex извлекаются как атрибуты. 
1. Извлеченные атрибуты добавляются в диапазон. 
1. Каждое имя части выражения преобразуется в имя атрибута. 
1. Сопоставленная часть выражения преобразуется в значение атрибута. 
1. Сопоставленная часть в имени диапазона заменяется извлеченным именем атрибута. Если атрибуты уже существуют в диапазоне, они будут перезаписаны. 
 
Этот процесс повторяется для всех правил в том порядке, в котором они указаны. Каждое последующее правило работает с именем диапазона, которое является выходным результатом предыдущего правила.

```json
"processors": [
  {
    "type": "span",
    "name": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]

```

## <a name="common-span-attributes"></a>Общие атрибуты span

В этом разделе перечислены некоторые общие атрибуты span, которые могут использоваться обработчиками данных телеметрии.

### <a name="http-spans"></a>Диапазоны HTTP

| attribute  | Тип | Описание | 
|---|---|---|
| `http.method` | строка | Метод HTTP-запроса.|
| `http.url` | строка | Полный URL-адрес запроса HTTP в форме `scheme://host[:port]/path?query[#fragment]` . Этот фрагмент обычно не передается по протоколу HTTP. Но если фрагмент известен, он должен быть включен.|
| `http.status_code` | number | [Код состояния ответа HTTP](https://tools.ietf.org/html/rfc7231#section-6).|
| `http.flavor` | строка | Тип протокола HTTP. |
| `http.user_agent` | строка | Значение заголовка [HTTP-агента пользователя](https://tools.ietf.org/html/rfc7231#section-5.5.3) , отправленное клиентом. |

### <a name="jdbc-spans"></a>Диапазоны JDBC

| attribute  | Тип | Описание  |
|---|---|---|
| `db.system` | строка | Идентификатор используемого продукта системы управления базами данных (СУБД). |
| `db.connection_string` | строка | Строка подключения, используемая для подключения к базе данных. Рекомендуется удалить внедренные учетные данные.|
| `db.user` | строка | Имя пользователя для доступа к базе данных. |
| `db.name` | строка | Строка, используемая для сообщения имени базы данных, к которой осуществляется доступ. Для команд, которые переключают базу данных, этой строке следует присвоить значение целевой базе данных, даже если команда завершается с ошибкой.|
| `db.statement` | строка | Выполняемая инструкция базы данных.|
