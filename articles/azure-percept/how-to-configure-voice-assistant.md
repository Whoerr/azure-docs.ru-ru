---
title: Настройка приложения Voice Assistant с помощью центра Интернета вещей Azure
description: Настройка приложения Voice Assistant с помощью центра Интернета вещей Azure
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/15/2021
ms.custom: template-how-to
ms.openlocfilehash: ec3e06b2d161785b5e6978cdf4cc6415fc0eb592
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663088"
---
# <a name="configure-voice-assistant-application-using-azure-iot-hub"></a>Настройка приложения Voice Assistant с помощью центра Интернета вещей Azure

В этой статье описывается, как настроить приложение для работы с голосовым помощником с помощью центра Интернета вещей. Пошаговое руководство, в котором описывается процесс создания голосового помощника с помощью демонстрационного шаблона, см. в статье Создание [речевого помощника без кода с помощью Azure Перцепт Studio и Azure Перцепт Audio](./tutorial-no-code-speech.md).

## <a name="update-your-voice-assistant-configuration"></a>Обновление конфигурации помощника по голосовому обслуживанию

1. Откройте [портал Azure](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) и введите в строке поиска **центр Интернета вещей** . Щелкните значок, чтобы открыть страницу центра Интернета вещей.

1. На странице центр Интернета вещей выберите центр Интернета вещей, в котором было подготовлено устройство.

1. Выберите **IOT Edge** в разделе **Автоматическое управление устройствами** в меню навигации слева, чтобы просмотреть все устройства, подключенные к центру Интернета вещей.

1. Выберите устройство, на котором было развернуто приложение для работы с голосовым помощником.

1. Щелкните **Set modules (задать модули**).

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/set-modules.png" alt-text="Эскиз.":::

1. Убедитесь, что в разделе **учетные данные реестра контейнеров** указана следующая запись. При необходимости добавьте учетные данные.

    |name|Адрес|Имя пользователя|Пароль|
    |----|-------|--------|--------|
    |азуриджедевицес|azureedgedevices.azurecr.io|девкитприватепревиевпулл|

1. В разделе **модули IOT Edge** выберите **азуриарспичклиентмодуле**.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/modules.png" alt-text="Эскиз.":::

1. Перейдите на вкладку **Параметры модуля** . Проверьте следующую конфигурацию:

    |URI образа|Политика перезапуска|Требуемое состояние|
    |---------|--------------|--------------|
    |azureedgedevices.azurecr.io/azureearspeechclientmodule:preload-devkit |always|запуск|

    Если параметры не совпадают, измените их и нажмите кнопку **Обновить**.

1. Перейдите на вкладку **переменные среды** . Убедитесь, что переменные среды не определены.

1. Перейдите на вкладку **Параметры создания контейнера** . Убедитесь, что параметры **хостконфиг** соответствуют приведенным ниже. Если они не совпадают, обновите параметры.

    ```
    {
        "HostConfig": {
            "Privileged": true,
            "Binds": [
                "/dev:/dev"
            ]
        }
    }
    ```

1. Перейдите на вкладку **module двойника Settings (параметры модуля** ). Обновите раздел **спичконфигс** следующим образом:

    ```
    "speechConfigs": {
        "appId": "<Application id for custom command project>",
        "key": "<Speech Resource key for custom command project>",
        "region": "<Region for the speech service>",
        "keywordModelUrl": "https://aedspeechscenarios.blob.core.windows.net/keyword-tables/computer.table",
        "keyword": "computer"
    }
    ```

    > [!NOTE]
    > Ключевое слово, использованное выше, является общедоступным ключевым словом по умолчанию. Если вы хотите использовать собственное, можно добавить собственное пользовательское ключевое слово, загружая созданный файл таблицы в хранилище BLOB-объектов. Для хранилища BLOB-объектов необходимо настроить анонимный доступ к контейнеру или анонимный доступ к BLOB-объектам.

## <a name="how-to-find-out-appid-key-and-region"></a>Как узнать идентификатор appId, ключ и регион

Чтобы найти **AppID**, **ключ** и **регион**, перейдите в раздел " [речь Studio](https://speech.microsoft.com/)":

1. Войдите и выберите соответствующий ресурс речи.
1. На домашней странице **Speech Studio** щелкните **пользовательские команды** в разделе **голосовые помощники**.
1. Выберите целевой проект.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/project.png" alt-text="Эскиз.":::

1. На панели меню слева щелкните **Параметры** .
1. **AppID** и **Key** будут расположены на вкладке **Общие** параметры.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/general-settings.png" alt-text="Эскиз.":::

1. Чтобы найти свой **регион**, откройте вкладку **ресурсы Luis** в параметрах. Выбор **ресурсов разработки** будет содержать сведения о регионе.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/luis-resources.png" alt-text="Эскиз.":::

1. После ввода сведений о **спичконфигс** нажмите кнопку **Обновить**.

1. Щелкните вкладку **Routes (маршруты** ) в верхней части страницы **Установка модулей** . Убедитесь, что у вас есть маршрут со следующим значением:

    ```
    FROM /messages/modules/azureearspeechclientmodule/outputs/* INTO $upstream
    ```

    Добавьте маршрут, если он не существует.

1. Щелкните **Просмотр и создание**.

1. Нажмите кнопку **Создать**.

## <a name="next-steps"></a>Дальнейшие действия

После обновления конфигурации речевого помощника вернитесь к демонстрации в Azure Перцепт Studio, чтобы взаимодействовать с приложением.