---
title: Краткое руководство. Настройка и включение модуля безопасности для ОСРВ Azure
description: Узнайте, как подключить и включить модуль безопасности для службы ОСРВ Azure в Центре Интернета вещей Azure.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2021
ms.author: shhazam
ms.openlocfilehash: a11a8ec2d8eb82950cf0ab55eb6ca1913b41c84a
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2021
ms.locfileid: "100522943"
---
# <a name="quickstart-security-module-for-azure-rtos-preview"></a>Краткое руководство. Модуль безопасности для ОСРВ Azure (предварительная версия)

В этой статье описаны предварительные требования для начала работы и объясняется, как включить модуль безопасности для службы ОСРВ Azure в Центре Интернета вещей. Если у вас сейчас нет Центра Интернета вещей, ознакомьтесь со статьей [Создание Центра Интернета вещей с помощью портала Azure](../iot-hub/iot-hub-create-through-portal.md), чтобы приступить к работе.

## <a name="prerequisites"></a>Предварительные требования 

### <a name="supported-devices"></a>Поддерживаемые устройства

- пакет обнаружения ST STM32F746G;
- NXP i.MX RT1060 EVK;
- микросхема SAM E54 Xplained Pro EVK.

Скачайте, скомпилируйте и запустите один из ZIP-файлов для конкретной доски и средства (IAR, частичная IDE или ПК) по своему усмотрению из [ресурса GitHub для модуля безопасности ОСРВ Azure](https://github.com/azure-rtos/azure-iot-preview/releases).

### <a name="azure-resources"></a>Ресурсы Azure

Далее мы подготовим ресурсы Azure. Вам потребуется Центр Интернета вещей. Мы рекомендуем использовать рабочую область Log Analytics. Для подключения к устройству потребуется строка подключения к Центру Интернета вещей. 
  
### <a name="iot-hub-connection"></a>Подключение к Центру Интернета вещей

Для начала работы требуется подключение к Центру Интернета вещей. 

1. Откройте **Центр Интернета вещей** на портале Azure.

1. Выберите **Устройства Интернета вещей**.

1. Нажмите кнопку **создания**.

1. Скопируйте строку подключения Интернета вещей в [файл конфигурации](how-to-azure-rtos-security-module.md).

Учетные данные подключений можно взять из конфигурации пользовательского приложения **HOST_NAME**, **DEVICE_ID** и **DEVICE_SYMMETRIC_KEY**.

Модуль безопасности для ОСРВ Azure использует подключения ПО промежуточного слоя Интернета вещей на основе протокола **MQTT**.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы завершить конфигурацию и настройку вашего решения, перейдите к следующей статье.

> [!div class="nextstepaction"]
> [Настройка модуля безопасности для ОСРВ Azure](how-to-azure-rtos-security-module.md)
