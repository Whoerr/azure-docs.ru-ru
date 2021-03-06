---
title: Краткое руководство. Управление устройством из Центра Интернета вещей Azure (Python) | Документация Майкрософт
description: 'В этом кратком руководстве описано, как запустить два примера приложений Python: внутреннее приложение, которое может удаленно управлять подключенными к центру устройствами, и приложение, которое имитирует подключенное к центру устройство, которым можно управлять удаленно.'
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom:
- mvc
- mqtt
- devx-track-python
- devx-track-azurecli
ms.date: 09/14/2020
ms.openlocfilehash: d7e7eb80d452620cdbb0f6688d6137ad34951dbe
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99070732"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-python"></a>Краткое руководство. Управление подключенным к Центру Интернета вещей устройством (Python)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

В этом кратком руководстве показано, как использовать прямой метод для управления имитированным устройством, подключенным к Центру Интернета вещей Azure. Центр Интернета вещей — это служба Azure, которая позволяет управлять устройствами Интернета вещей из облака и принимать большие объемы данных телеметрии, передаваемых с устройств в облако, для хранения или обработки. Этот метод позволяет удаленно изменить поведение подключенного к Центру Интернета вещей устройства. При работе с этим кратким руководством используются два приложения Python: приложение имитированного устройства, которое реагирует на прямые методы, и внутреннее приложение, которое вызывает эти прямые методы на имитированном устройстве.

## <a name="prerequisites"></a>Предварительные требования

* Учетная запись Azure с активной подпиской. [Создайте бесплатно](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

* [Python версии 3.7 и выше](https://www.python.org/downloads/). Сведения о других поддерживаемых версиях Python см. в разделе о [возможностях устройств Azure IoT](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device#azure-iot-device-features).

* [Пример проекта Python](https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip).

* Порт 8883, открытый в брандмауэре. Пример устройства в этом кратком руководстве использует протокол MQTT, который передает данные через порт 8883. В некоторых корпоративных и академических сетях этот порт может быть заблокирован. Дополнительные сведения и способы устранения этой проблемы см. в разделе о [подключении к Центру Интернета вещей по протоколу MQTT](iot-hub-mqtt-support.md#connecting-to-iot-hub).

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

Если вы закончили работу с предыдущим [руководством по отправке данных телеметрии с устройства в Центр Интернета вещей](quickstart-send-telemetry-python.md), этот шаг можно пропустить.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Регистрация устройства

Если вы закончили работу с предыдущим [руководством по отправке данных телеметрии с устройства в Центр Интернета вещей](quickstart-send-telemetry-python.md), этот шаг можно пропустить.

Устройство должно быть зарегистрировано в Центре Интернета вещей, прежде чем оно сможет подключиться. В этом кратком руководстве для регистрации имитируемого устройства используется Azure Cloud Shell.

1. Выполните приведенные ниже команды в Azure Cloud Shell, чтобы создать удостоверение устройства.

    **YourIoTHubName**. Замените этот заполнитель именем вашего центра Интернета вещей.

    **MyPythonDevice**. Это имя регистрируемого устройства. Рекомендуется использовать **MyPythonDevice**, как показано ниже. Если вы выбрали другое имя для устройства, используйте его при работе с этим руководством и обновите имя устройства в примерах приложений перед их запуском.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyPythonDevice
    ```

2. Выполните следующую команду в Azure Cloud Shell, чтобы получить _строку подключения_ зарегистрированного устройства:

    **YourIoTHubName**. Замените этот заполнитель именем вашего центра Интернета вещей.

    ```azurecli-interactive
    az iot hub device-identity connection-string show --hub-name {YourIoTHubName} --device-id MyPythonDevice --output table
    ```

    Запишите строку подключения устройства, которая выглядит так:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Это значение понадобится позже в рамках этого краткого руководства.

3. Чтобы разрешить внутреннему приложению подключаться к Центру Интернета вещей и получать сообщения, вам необходима _строка подключения к службе_. Следующая команда извлекает строку подключения службы для Центра Интернета вещей:

    **YourIoTHubName**. Замените этот заполнитель именем вашего Центра Интернета вещей.

    ```azurecli-interactive
    az iot hub connection-string show \
      --policy-name service \
      --name {YourIoTHubName} \
      --output table
    ```

    Запишите строку подключения к службе, которая выглядит так:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

    Это значение понадобится позже в рамках этого краткого руководства. Строка подключения к службе отличается от строки подключения к устройству из предыдущего шага.

## <a name="listen-for-direct-method-calls"></a>Ожидание передачи данных при вызове прямого метода

Приложение имитированного устройства подключается к конечной точке конкретного устройства в Центре Интернета вещей, отправляет имитированные данные телеметрии и ожидает передачи данных при вызове прямого метода из центра. В рамках этого краткого руководства вызов прямого метода из центра инициирует изменение интервала, с которым устройство отправляет данные телеметрии. После выполнения прямого метода имитированное устройство отправляет подтверждение в центр.

1. В окне терминала на локальном компьютере перейдите в корневую папку примера проекта Python. Затем перейдите в папку **iot-hub\Quickstarts\simulated-device-2**.

1. Откройте файл **SimulatedDevice.py** в любом текстовом редакторе.

    Замените значение переменной `CONNECTION_STRING` записанной ранее строкой подключения к устройству. Сохраните изменения в файле **SimulatedDevice.py**.

1. Установите необходимые библиотеки для приложения имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    pip install azure-iot-device
    ```

1. Запустите приложение имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    На следующем снимке экрана показан пример выходных данных, когда приложение имитированного устройства отправляет данные телеметрии в Центр Интернета вещей:

    ![Запуск виртуального устройства](./media/quickstart-control-device-python/simulated-device-1.png)

## <a name="call-the-direct-method"></a>Вызов прямого метода

Внутреннее приложение подключается к конечной точке на стороне службы в Центре Интернета вещей. Приложение выполняет вызовы прямых методов к устройству через Центр Интернета вещей и ожидает подтверждений. Внутреннее приложение Центра Интернета вещей обычно работает в облаке.

1. В другом окне терминала на локальном компьютере перейдите в корневую папку примера проекта Python. Затем перейдите в папку **iot-hub\Quickstarts\back-end-application**.

1. Откройте файл **BackEndApplication.py** в любом текстовом редакторе.

    Замените значение переменной `CONNECTION_STRING` записанной ранее строкой подключения к службе. Сохраните изменения в файле **BackEndApplication.py**.

1. Установите необходимые библиотеки для приложения имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    pip install azure-iot-hub
    ```

1. Запустите внутреннее приложение, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    python BackEndApplication.py
    ```

    На следующем снимке экрана показан пример выходных данных, когда приложение выполняет вызов прямого метода к устройству и получает подтверждение:

    ![Запуск внутреннего приложения](./media/quickstart-control-device-python/backend-application.png)

    После запуска внутреннего приложения в окне консоли, в котором выполняется имитированное устройство, отобразится сообщение и изменится интервал, с которым это приложение отправляет сообщения:

    ![Изменения в имитированном клиенте](./media/quickstart-control-device-python/simulated-device-2.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Дальнейшие действия

В рамках этого краткого руководства вы вызвали прямой метод в устройстве из внутреннего приложения, а также ответили на этот вызов в приложении имитированного устройства.

Чтобы узнать, как маршрутизировать сообщения с устройства в облако в разные расположения в облаке, перейдите к следующему руководству.

> [!div class="nextstepaction"]
> [Руководство. Маршрутизация телеметрии в разные конечные точки для обработки](tutorial-routing.md)
