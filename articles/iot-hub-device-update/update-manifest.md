---
title: Обновление для манифеста обновления центра Интернета вещей | Документация Майкрософт
description: Сведения о том, как свойства отправляются из службы обновления устройств на устройство во время обновления
author: andbrown
ms.author: andbrown
ms.date: 2/17/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: ea438a4a8db8dce9cf2acb0b5b3ff4e250c3cf39
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663286"
---
# <a name="device-update-for-iot-hub-update-manifest"></a>Обновление для манифеста обновления центра Интернета вещей

## <a name="overview"></a>Обзор

Обновление устройства для центра Интернета вещей использует _манифест обновления_ для обмена действиями, а также метаданные, поддерживающие эти действия, с помощью свойств интерфейса [азуредевицеупдатекоре. орчестраторметадата: 4](./device-update-plug-and-play.md).
В этом документе описаны основные принципы `updateManifest` использования свойства в `AzureDeviceUpdateCore.OrchestratorMetadata:4` интерфейсе агентом обновления устройства. `AzureDeviceUpdateCore.OrchestratorMetadata:4`Свойства интерфейса отправляются из центра обновления устройства для службы центра Интернета вещей агенту обновления устройства. `updateManifest`— Это сериализованный объект JSON, который анализируется агентом обновления устройства.

### <a name="an-example-update-manifest"></a>Пример манифеста обновления

```JSON
{
    "manifestVersion": "1",
    "updateId": {
        "provider": "DuTest",
        "name": "DuTestUser",
        "version": "2020.611.534.16"
    },
    "updateType": "microsoft/swupdate:1",
    "installedCriteria": "1.0",
    "files": {
        "00000": {
            "fileName": "image.swu",
            "sizeInBytes": 256000,
            "hashes": {
                "sha256": "IhIIxBJpLfazQOk/PVi6SzR7BM0jf4HDqw+6gdZ3vp8="
            }
        }
    },
    "createdDateTime": "2020-06-12T00:38:13.9350278"
}
```

Цель манифеста обновления — описать содержимое обновления, а именно его удостоверение, тип, установленные критерии и обновить метаданные файла. Кроме того, манифест обновления криптографически подписывается, чтобы разрешить агенту обновления устройства проверять его подлинность. Дополнительные сведения о том, как манифест обновления используется для безопасного импорта содержимого, см. в документации по [безопасности обновления устройств](./device-update-security.md) .

## <a name="import-manifest-vs-update-manifest"></a>Импорт манифеста обновления для манифеста VS

Важно понимать различия между манифестом импорта и понятиями обновления манифестов в обновлении устройства для центра Интернета вещей. 
* [Манифест импорта](./import-concepts.md) создается тем, кто создает соответствующее обновление. Здесь описывается содержимое обновления, которое будет импортировано в обновление устройства для центра Интернета вещей. 
* Манифест обновления автоматически создается службой обновления устройства для службы центра Интернета вещей с использованием некоторых свойств, определенных в манифесте импорта. Он используется для передачи важной информации агенту обновления устройства во время процесса обновления. 

Каждый тип манифеста имеет собственную схему и версию схемы.

## <a name="update-manifest-properties"></a>Обновить свойства манифеста

Высокоуровневые определения свойств манифеста обновления можно найти в определениях интерфейса, приведенных [здесь](./device-update-plug-and-play.md). Чтобы обеспечить более глубокий контекст, давайте подробнее рассмотрим свойства и их использование в системе.

### <a name="updateid"></a>updateId

Содержит `provider` , `name` и `version` , представляющий точное обновление устройства для удостоверения обновления центра Интернета вещей, используемое для определения совместимых устройств для обновления.

### <a name="updatetype"></a>updateType

Представляет тип обновления, обрабатываемого конкретным типом обработчика обновлений. Он соответствует форме `microsoft/swupdate:1` обновления на основе образа и `microsoft/apt:1` пакетного обновления (см `Update Handler Types` . раздел ниже).

### <a name="installedcriteria"></a>инсталледкритериа

Строка, содержащая сведения, необходимые обработчику обновлений агента обновления устройства для определения того, установлено ли обновление на устройстве. В `Update Handler Types` разделе документируется формат `installedCriteria` для каждого типа обновления, поддерживаемого обновлением устройства для центра Интернета вещей.

### <a name="files"></a>файлы

Сообщает агенту обновления устройства, какие файлы загружать, и хэш, который будет использоваться для проверки правильности загрузки файлов.
Подробнее рассмотрим `files` содержимое свойства:

```json
"files":{
        <FILE_ID_STRING>:{
            "fileName":<STRING>,
            "sizeInBytes":<INTEGER>,
            "hashes":{
                <HASH-TYPE>:<HASH-STRING>
            }
        }
    }
```

За пределами объекта `updateManifest` является `fileUrls` массив объектов JSON.

```json
"fileUrls":{
      <FILE_ID_STRING>: <URL-in-String-Format>
 }
```

И в `FILE_ID_STRING` `fileUrls` , и одинаковы `files` (например, "0000" в `files` имеет URL-адрес в "0000" в `fileUrls` ).

### <a name="manifestversion"></a>манифестверсион

Строка, представляющая версию схемы.

## <a name="update-handler-types"></a>Обновить типы обработчиков

|Метод Update|Обновить тип обработчика|Тип обновления|Установленные критерии|Ожидаемые файлы для публикации|
|-------------|-------------------|----------|-----------------|--------------|
|На основе образа|свупдате|"Microsoft/свупдате: версия"|Эталонный образ сохраняет указание своей версии в файле/ЕТК/аду-версион.  |файл. СВУ, содержащий изображение Свупдате|
|На основе пакетов|APT|"Microsoft/APT: версия"|`<name>` + "-" + `<version>` (определенные свойства в файле манифеста APT|`<APT Update Manifest>`. JSON, содержащий конфигурацию APT и список пакетов|

