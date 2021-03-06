---
title: Подключение к Azure Перцепт DK по последовательному каналу
description: Узнайте, как настроить последовательное подключение к Azure Перцепт DK с выводимыми данными и последовательным кабелем по USB-TTL.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/03/2021
ms.custom: template-how-to
ms.openlocfilehash: 93b8ab0ce53202402e86b059abe3c600590d549e
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101662919"
---
# <a name="connect-to-your-azure-percept-dk-over-serial"></a>Подключение к Azure Перцепт DK по последовательному каналу

Выполните следующие действия, чтобы настроить последовательное подключение к Azure Перцепт DK [с помощью инструкции](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

> [!WARNING]
> **Не** пытайтесь подключиться к DevKit через последовательное подключение, за исключением случаев крайнего сбоя (например, если вы выводили модуль устройства). Использование корпуса платной платы для подключения последовательного кабеля очень сложно и приведет к нарушению Wi-Fi кабелей антенны.

## <a name="prerequisites"></a>Предварительные требования

- [PuTTY;](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- Главный компьютер
- Azure Перцепт DK
- [Последовательный кабель USB-TTL](https://www.adafruit.com/product/954)

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/usb-serial-cable.png" alt-text="Последовательный кабель USB-TTL.":::

## <a name="initiate-the-serial-connection"></a>Инициация последовательного подключения

1. Если плата перевозчика подключена к шине 80/20, удалите ее с помощью шины, используя шестнадцатеричный ключ (включенный в DevKit карточку приветствия).

1. Удалите винты с недостороны корпуса платы перевозчика и извлеките материнскую плату.

    > [!WARNING]
    > Удаление материнской платы приведет к нарушению Wi-Fi кабелей антенны. **Не** продолжайте использовать последовательное подключение, если оно не является последним средством восстановления устройства.

1. Удалите теплоотвод.

1. Удалите плату перемычки с контактов GPIO.

    > [!TIP]
    > Обратите внимание на ориентацию доски перемычек перед ее удалением. Например, нарисуйте стрелку на доске перемычек или прикрепите к ней, указывающую на цепь для справки. Плата перемычек не имеет ключа и может быть случайно подключена обратно при сборке платы перевозчика.

1. Подключите [последовательный порт USB to TTL](https://www.adafruit.com/product/954) к контактам GPIO на материнской плате, как показано ниже. Обратите внимание, что красный канал не подключен.

    - Подключите черный кабель (земля) к контакту 6.
    - Подключите белый кабель (RX) к контакту 8.
    - Подключите зеленый кабель (TX) к разъему 10.

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/serial-connection-carrier-board.png" alt-text="Подключения последовательного ПИН-кода платы перевозчика.":::

1. Включите DevKit и подключите USB-часть последовательного кабеля к компьютеру.

1. В Windows последовательно выберите **Пуск**  ->  **Центр обновления Windows параметры**  ->  **View необязательные обновления**  ->  **драйверов** обновления. Найдите в списке последовательное обновление USB, установите флажок рядом с ним и нажмите кнопку **скачать и установить**.  

1. Затем откройте Диспетчер устройств Windows (**Запуск**  ->  **Диспетчер устройств**). Перейдите в раздел **порты** и щелкните **USB-порт UART** , чтобы открыть **Свойства**. Обратите внимание на порт COM, к которому подключено устройство.

1. Перейдите на вкладку **Параметры порта** . Убедитесь, что для **бит в секунду** установлено значение 115200.

1. Откройте PuTTY. Введите следующее и нажмите кнопку " **Открыть** ", чтобы подключиться к DevKit через последовательный порт:

    1. Последовательная линия: COM [порт #]
    1. Скорость: 115200
    1. Тип подключения: последовательный

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/putty-serial-session.png" alt-text="Окно сеанса выводимых параметров с выбранными последовательными параметрами.":::

## <a name="next-steps"></a>Дальнейшие действия

Чтобы обновить незагружаемое устройство по последовательному [кабелю, используя последовательный кабель](https://www.adafruit.com/product/954)с подключением по протоколу USB к TTL, ознакомьтесь с руководством по обновлению USB для нестандартных ситуаций.

[comment]: # (По возможности добавьте ссылку на обновление USB.)