---
title: Соглашения Plug and Play IoT | Документация Майкрософт
description: Описание соглашений Plug and Play IoT, которое использует устройства при отправке телеметрии и свойствах, а также на обработку команд и обновлений свойств.
author: rido-min
ms.author: rmpablos
ms.date: 07/10/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 86c6ea9dded423e7bd513faf73adfd293f2bd38f
ms.sourcegitcommit: ab94795f9b8443eef47abae5bc6848bb9d8d8d01
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/27/2020
ms.locfileid: "96302615"
---
# <a name="iot-plug-and-play-conventions"></a>Соглашения IoT Plug and Play

Устройства IoT Plug and Play должны следовать набору соглашений при обмене сообщениями с центром Интернета вещей. Устройства IoT Plug and Play используют протокол MQTT для обмена данными с центром Интернета вещей.

Устройства могут включать [модули](../iot-hub/iot-hub-devguide-module-twins.md)или быть реализованы в [модуле IOT Edge](../iot-edge/about-iot-edge.md) , размещенном в среде выполнения IOT Edge.

Вы описываете телеметрию, свойства и команды, которые устройство IoT Plug and Play реализует с помощью _модели_ [Digital двойников Definition Language v2 (дтдл)](https://github.com/Azure/opendigitaltwins-dtdl) . В этой статье есть два типа модели, на которые ссылается:

- **Нет компонента** — модель без компонентов. Модель объявляет данные телеметрии, свойства и команды в качестве свойств верхнего уровня в разделе содержимого основного интерфейса. В средстве Azure IoT Explorer эта модель отображается как отдельный _компонент по умолчанию_.
- **Несколько компонентов** — модель, состоящая из двух или более интерфейсов. Основной интерфейс, который отображается как _компонент по умолчанию_ с данными телеметрии, свойствами и командами. Один или несколько интерфейсов, объявленных как компоненты с дополнительными данными телеметрии, свойствами и командами.

Дополнительные сведения см. [в разделе IoT Plug and Play Components in Models](concepts-components.md).

## <a name="identify-the-model"></a>Указание модели

Чтобы объявить реализуемую модель, устройство или модуль IoT Plug and Play включает идентификатор модели в пакет подключения MQTT, добавляя `model-id` к `USERNAME` полю.

Для идентификации модели, реализуемой устройством или модулем, служба может получить идентификатор модели из:

- Поле двойникаа устройства `modelId` .
- Поле Digital двойника `$metadata.$model` .
- Уведомление об изменении цифрового двойникаа.

## <a name="telemetry"></a>Телеметрия

Данные телеметрии, отправляемые с устройства без компонента, не нуждаются в дополнительных метаданных. Система добавляет `dt-dataschema` свойство.

Данные телеметрии, отправляемые с устройства с несколькими компонентами, должны добавляться `$.sub` в качестве свойства сообщения. Система добавляет `dt-subject` `dt-dataschema` Свойства и.

## <a name="read-only-properties"></a>Свойства только для чтения

### <a name="sample-no-component-read-only-property"></a>Образец нет компонента свойство только для чтения

Устройство или модуль могут отправить любой допустимый код JSON, следующий за правилами ДТДЛ v2.

ДТДЛ:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:example: Thermostat;1",
  "@type": "Interface",
  "contents": [
    {
      "@type": "Property",
      "name": "temperature",
      "schema": "double"
    }
  ]
}
```

Пример полезных данных свойства:

```json
"reported" :
{
  "temperature" : 21.3
}
```

### <a name="sample-multiple-components-read-only-property"></a>Пример использования нескольких компонентов свойство только для чтения

Устройство или модуль должны добавить `{"__t": "c"}` маркер, чтобы указать, что элемент ссылается на компонент.

ДТДЛ, ссылающийся на компонент:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:TemperatureController;1",
  "@type": "Interface",
  "displayName": "Temperature Controller",
  "contents": [
    {
      "@type" : "Component",
      "schema": "dtmi:com:example:Thermostat;1",
      "name": "thermostat1"
    }
  ]
}
```

ДТДЛ, определяющий компонент:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:Thermostat;1",
  "@type": "Interface",
  "contents": [
    {
      "@type": "Property",
      "name": "temperature",
      "schema": "double"
    }
  ]
}
```

Пример полезных данных свойства:

```json
"reported": {
  "thermostat1": {
    "__t": "c",
    "temperature": 21.3
  }
}
```

## <a name="writable-properties"></a>Доступные для записи свойства

Устройство или модуль должны подтвердить, что оно получило свойство, отправив сообщаемое свойство. Сообщаемое свойство должно включать:

- `value` — фактическое значение свойства (обычно полученное значение, однако устройство может принять решение о другом значении).
- `ac` — код подтверждения, использующий код состояния HTTP.
- `av` — Версия подтверждения, относящаяся к `$version` требуемому свойству. Это значение можно найти в полезных данных JSON требуемого свойства.
- `ad` — Необязательное описание подтверждения.

Когда устройство запускается, оно должно запросить двойника устройства и проверить наличие обновлений свойств, доступных для записи. Если версия свойства, доступного для записи, увеличилась, когда устройство находится в автономном режиме, устройство должно отправить ответ сообщаемого свойства, чтобы подтвердить, что он получил обновление.

Когда устройство запускается впервые, оно может отправить исходное значение для сообщаемого свойства, если оно не получает от концентратора начальное требуемое свойство. В этом случае устройству должно быть присвоено значение `av` `1` . Пример:

```json
"reported": {
  "targetTemperature": {
    "value": 20.0,
    "ac": 200,
    "av": 1,
    "ad": "initialize"
  }
}
```

Устройство может использовать сообщаемое свойство для предоставления других сведений концентратору. Например, устройство может реагировать на ряд выполняющихся сообщений, таких как:

```json
"reported": {
  "targetTemperature": {
    "value": 35.0,
    "ac": 202,
    "av": 3,
    "ad": "In-progress - reporting current temperature"
  }
}
```

Когда устройство достигает целевой температуры, оно отправляет следующее сообщение:

```json
"reported": {
  "targetTemperature": {
    "value": 20.0,
    "ac": 200,
    "av": 3,
    "ad": "Reached target temperature"
  }
}
```

Устройство может сообщить об ошибке, например:

```json
"reported": {
  "targetTemperature": {
    "value": 120.0,
    "ac": 500,
    "av": 3,
    "ad": "Target temperature out of range. Valid range is 10 to 99."
  }
}
```

### <a name="sample-no-component-writable-property"></a>Пример свойства, доступного для записи без компонента

Когда устройство получает несколько сообщаемых свойств в одной полезной нагрузке, оно может отправлять Отправленные ответы свойств в нескольких полезных данных.

Устройство или модуль могут отправить любой допустимый код JSON, следующий за правилами ДТДЛ v2:

ДТДЛ:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:example: Thermostat;1",
  "@type": "Interface",
  "contents": [
    {
      "@type": "Property",
      "name": "targetTemperature",
      "schema": "double",
      "writable": true
    }
  ]
}
```

Пример полезных данных требуемого свойства:

```json
"desired" :
{
  "targetTemperature" : 21.3,
  "targetHumidity" : 80
},
"$version" : 3
```

Первые полезные данные образца сообщаемого свойства:

```json
"reported": {
  "targetTemperature": {
    "value": 21.3,
    "ac": 200,
    "av": 3,
    "ad": "complete"
  }
}
```

Образец полученного свойства, второй полезные данные:

```json
"reported": {
  "targetHumidity": {
    "value": 80,
    "ac": 200,
    "av": 3,
    "ad": "complete"
  }
}
```

### <a name="sample-multiple-components-writable-property"></a>Пример свойства, записываемого несколькими компонентами

Устройство или модуль должны добавить `{"__t": "c"}` маркер, чтобы указать, что элемент ссылается на компонент.

Маркер отправляется только для обновлений свойств, определенных в компоненте. Обновления свойств, определенных в компоненте по умолчанию, не включают маркер, см. раздел [выборка без компонента для записи свойства](#sample-no-component-writable-property)

Когда устройство получает несколько сообщаемых свойств в одной полезной нагрузке, оно может отправлять Отправленные ответы свойств в нескольких полезных данных.

Устройство или модуль должны подтвердить, что они получили свойства, отправив сообщаемые свойства:

ДТДЛ, ссылающийся на компонент:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:TemperatureController;1",
  "@type": "Interface",
  "displayName": "Temperature Controller",
  "contents": [
    {
      "@type" : "Component",
      "schema": "dtmi:com:example:Thermostat;1",
      "name": "thermostat1"
    }
  ]
}
```

ДТДЛ, определяющий компонент:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:Thermostat;1",
  "@type": "Interface",
  "contents": [
    {
      "@type": "Property",
      "name": "targetTemperature",
      "schema": "double",
      "writable": true
    }
  ]
}
```

Пример полезных данных требуемого свойства:

```json
"desired": {
  "thermostat1": {
    "__t": "c",
    "targetTemperature": 21.3,
    "targetHumidity": 80
  }
},
"$version" : 3
```

Первые полезные данные образца сообщаемого свойства:

```json
"reported": {
  "thermostat1": {
    "__t": "c",
    "targetTemperature": {
      "value": 23,
      "ac": 200,
      "av": 3,
      "ad": "complete"
    }
  }
}
```

Образец полученного свойства, второй полезные данные:

```json
"reported": {
  "thermostat1": {
    "__t": "c",
    "targetHumidity": {
      "value": 80,
      "ac": 200,
      "av": 3,
      "ad": "complete"
    }
  }
}
```

## <a name="commands"></a>Команды

Ни один из интерфейсов компонентов не использует имя команды без префикса.

На устройстве или модуле в интерфейсах с несколькими компонентами имена команд используются в следующем формате: `componentName*commandName` .

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали о соглашениях IoT Plug and Play, ниже приведены некоторые дополнительные ресурсы.

- [Язык определения цифровых двойников (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [Пакет SDK для устройств для C](/azure/iot-hub/iot-c-sdk-ref/)
- [REST API IoT](/rest/api/iothub/device)
- [Компоненты модели](./concepts-components.md)