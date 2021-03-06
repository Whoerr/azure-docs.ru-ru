---
title: Использование рабочих процессов для интеграции приложения Azure IoT Central с другими облачными службами | Документация Майкрософт
description: В этом практическом руководстве показано, как разработчик может настраивать правила и действия для интеграции приложения IoT Central с другими облачными службами. Чтобы создать расширенное правило, используйте соединитель IoT Central, предоставленный в Power Automate или Azure Logic Apps.
author: dominicbetts
ms.author: dobett
ms.date: 05/12/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 257855b4f7b1fae56ed8d6a063acfb0588da9b6a
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92123326"
---
# <a name="use-workflows-to-integrate-your-azure-iot-central-application-with-other-cloud-services"></a>Использование рабочих процессов для интеграции приложения Azure IoT Central с другими облачными службами

*Эта статья предназначена для разработчиков решений.*

Вы можете создавать в IoT Central правила, которые активируют действия, например для отправки сообщений электронной почты, в ответ на определенные условия на основе телеметрии, включая превышение порогового значения температуры устройства.

Соединитель Azure IoT Central v3 для Power автоматизиру и Azure Logic Apps позволяет создавать более сложные правила для автоматизации операций в IoT Central.

- Когда в приложении IoT Central Azure срабатывает правило, оно может активировать рабочий процесс в Power Automate или Azure Logic Apps. Эти рабочие процессы могут выполнять действия в других облачных службах, таких как Microsoft 365 или сторонние службы.
- Событие в другой облачной службе, например Microsoft 365, может активировать рабочий процесс в Power автоматизиру или Azure Logic Apps. Эти рабочие процессы могут выполнять действия или извлекать данные из приложений IoT Central.

## <a name="prerequisites"></a>Предварительные условия

Чтобы выполнить действия, описанные в этом руководстве, вам потребуется активная подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

Для настройки решения требуется приложение IoT Central версии 3. Чтобы узнать, как проверить версию приложения, см. [сведения о приложении](./howto-get-app-info.md). Чтобы узнать, как создать приложение IoT Central, см. статью [Создание приложения IOT Central Azure](./quick-deploy-iot-central.md).

> [!NOTE]
> Если вы используете приложение IoT Central версии 2, см. статью [создание рабочих процессов с соединителем IOT Central в Azure Logic Apps](/previous-versions/azure/iot-central/core/howto-build-azure-logic-apps) на сайте документации по предыдущим версиям и использование соединителя Azure IOT Central v2.

## <a name="trigger-a-workflow-from-a-rule"></a>Активация рабочего процесса из правила

Прежде чем активировать рабочий процесс в Power Automate или Azure Logic Apps, необходимо создать правило в приложении IoT Central. См. сведения в статье [Краткое руководство по настройке правил и действий в Azure IoT Central](./howto-configure-rules.md).

Добавление соединителя **Azure IOT Central v3-Preview** в качестве триггера в Power автоматизиру:

1. В Power Automate щелкните **+ Создать**, а затем выберите вкладку **Пользовательские**.
1. Найдите *IOT Central*и выберите соединитель **Azure IOT Central v3-Preview** .
1. В списке триггеров выберите **При активации правила (предварительная версия)** .
1. Для шага **При активации правила** выберите приложение IoT Central и правило, которое вы используете.

Чтобы добавить соединитель **Azure IOT Central v3-Preview** в качестве триггера в Azure Logic Apps:

1. В **конструкторе Logic Apps** выберите шаблон **Пустое приложение логики**.
1. Там же в конструкторе перейдите на вкладку **Пользовательские**.
1. Найдите *IOT Central*и выберите соединитель **Azure IOT Central v3-Preview** .
1. В списке триггеров выберите **При активации правила (предварительная версия)** .
1. Для шага **При активации правила** выберите приложение IoT Central и правило, которое вы используете.

:::image type="content" source="./media/howto-configure-rules-advanced/triggers.png" alt-text="Найдите соединитель Azure IoT Central (предварительная версия) и выберите триггер":::

Теперь можно добавить дополнительные шаги в рабочий процесс, чтобы реализовать сценарий интеграции.

## <a name="run-an-action"></a>Выполнение действия

Действия в приложении IoT Central можно выполнять из рабочих процессов Power Automate и Azure Logic Apps. Для начала создайте свой рабочий процесс и с помощью соединителя определите триггер для запуска этого рабочего процесса. Затем используйте соединитель **Azure IOT Central v3-Preview** в качестве действия.

Добавление соединителя **Azure IOT Central v3-Preview** в качестве действия в Power автоматизиру:

1. В Power Automate в области **Выберите действие** перейдите на вкладку **Пользовательское**.
1. Найдите *IOT Central* и выберите соединитель **Azure IOT Central v3-Preview** .
1. В списке действий выберите нужное действие IoT Central.
1. На этом шаге настройте параметры для выбранного действия. Затем нажмите кнопку **Save** (Сохранить).

Чтобы добавить соединитель **Azure IOT Central v3-Preview** в качестве действия в Azure Logic Apps:

1. В **конструкторе Logic Apps** в области **Выберите действие** перейдите на вкладку **Пользовательское**.
1. Найдите *IOT Central*и выберите соединитель **Azure IOT Central v3-Preview** .
1. В списке действий выберите нужное действие IoT Central.
1. На этом шаге настройте параметры для выбранного действия. Затем нажмите кнопку **Save** (Сохранить).

:::image type="content" source="./media/howto-configure-rules-advanced/actions.png" alt-text="Найдите соединитель Azure IoT Central (предварительная версия) и выберите триггер":::

## <a name="list-of-actions"></a>Список действий

В следующем списке перечислены все доступные действия IoT Central в соединителе **Azure IOT Central v3-Preview** и их параметры конфигурации. Многие поля могут иметь динамически создаваемое содержимое. Например, предыдущий шаг может определять идентификатор устройства для выполнения текущего шага.

### <a name="create-or-update-a-device"></a>Создание или обновление устройства

Это действие позволяет создать или обновить устройство в приложении IoT Central.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно создать или обновить. |
| Approved | Выберите, утверждено ли устройство для подключения к IoT Central. |
| Описание устройства | Подробное описание этого устройства. |
| Имя устройства | Отображаемое имя для этого устройства. |
| Шаблон устройства | Выберите шаблон устройств в списке в приложении IoT Central. |
| Имитировано | Выберите, является ли устройство имитированным. |

### <a name="delete-a-device"></a>Удаление устройства

Это действие позволяет удалить устройство из приложения IoT Central.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно удалить. |

### <a name="execute-a-device-command"></a>Выполнение команды на устройстве

Это действие используется для выполнения команды, определенной в одном из интерфейсов устройства.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно удалить. |
| Компонент устройства | Интерфейс в шаблоне устройства, который содержит нужную команду. |
| Команда устройства | Выберите одну из команд в выбранном интерфейсе. |
| Шаблон устройства | Выберите шаблон устройств в списке в приложении IoT Central. |
| Полезные данные запроса команды на устройстве | Если для команды нужны полезные данные запроса, добавьте их здесь.  |

> [!NOTE]
> Вы не сможете выбрать компонент устройства, пока не выбран шаблон устройства.

### <a name="get-a-device-by-id"></a>Получение устройства по идентификатору

Это действие используется для получения сведений об устройстве.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно удалить. |

Полученные сведения можно использовать в динамических выражениях в других действиях. Возвращаемые сведения об устройстве включают следующее: **Утверждено**, **текст**, **Описание устройства**, **Имя устройства**, **Шаблон устройства**, **Подготовлено** и **Имитировано**.

### <a name="get-device-cloud-properties"></a>Получение облачных свойств устройства

Это действие используется для получения значений облачных свойств для определенного устройства.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно удалить. |
| Шаблон устройства | Выберите шаблон устройств в списке в приложении IoT Central. |

Полученные значения свойств облака можно использовать в динамических выражениях в других действиях.

### <a name="get-device-properties"></a>Получение свойств устройства

Это действие используется для получения значений свойств для определенного устройства.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно удалить. |
| Шаблон устройства | Выберите шаблон устройств в списке в приложении IoT Central. |

Полученные значения свойств можно использовать в динамических выражениях в других действиях.

### <a name="get-device-telemetry-value"></a>Получение значений телеметрии устройства

Это действие используется для получения значений телеметрии для определенного устройства.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно удалить. |
| Шаблон устройства | Выберите шаблон устройств в списке в приложении IoT Central. |

Полученные значения телеметрии можно использовать в динамических выражениях в других действиях.

### <a name="update-device-cloud-properties"></a>Изменение облачных свойств устройства

Это действие используется для изменения значений облачных свойств для определенного устройства.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно удалить. |
| Шаблон устройства | Выберите шаблон устройств в списке в приложении IoT Central. |
| Свойства облака | После выбора шаблона устройства сюда добавляется поле для каждого облачного свойства, определенного в шаблоне. |

### <a name="update-device-properties"></a>Изменение свойств устройства

Это действие используется для изменения значений доступных для записи свойств для определенного устройства.

| Поле | Описание |
| ----- | ----------- |
| Приложение | Выберите приложение IoT Central в списке. |
| Устройство | Уникальный идентификатор устройства, которое нужно удалить. |
| Шаблон устройства | Выберите шаблон устройств в списке в приложении IoT Central. |
| Доступные для записи свойства | После выбора шаблона устройства сюда добавляется поле для каждого доступного для записи свойства, определенного в шаблоне. |

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали, как создать расширенное правило в приложении IoT Central Azure, вы можете научиться [анализировать данные устройств в приложении Azure IoT Central](howto-create-analytics.md).