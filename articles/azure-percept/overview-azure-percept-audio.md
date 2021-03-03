---
title: Общие сведения о звуковом устройстве Azure Перцепт
description: Дополнительные сведения об Azure Перцепт Audio
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: a63f471498667f58fc80f89323ad93b0feb8bc95
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663687"
---
# <a name="introduction-to-azure-percept-audio"></a>Введение в службу Перцепт Azure Audio

Azure Перцепт Audio — это устройство, которое добавляет возможности преобразования речи в речь в Azure Перцепт DK. Он содержит предварительно настроенный ускоритель AI, а также линейный массив из четырех микрофонов, позволяющий применять пользовательские команды, распознавание ключевых слов и многофакторную речь на локальных устройствах прослушивания. Azure Перцепт Audio позволяет производителям устройств расширять возможности концепции Azure Перцепт DK на новые, устройства, активированные с помощью смарт-устройств. Она интегрирована с Azure Перцепт DK, Azure Перцепт Studio и другими службами управления Azure ребра. Его можно приобрести в [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270).

![Аудиоустройство Azure Перцепт.](./media/overview-azure-percept-audio/percept-audio.png)


## <a name="azure-percept-audio-components"></a>Компоненты звука Azure Перцепт

Служба Перцепт Audio Azure содержит следующие основные компоненты:

- Готовое к использованию рабочее устройство Azure Перцепт Audio (SoM) с четырьмя линейными массивами для микрофона
- Доска разработчика (включая кнопки с 2 разъемами, 3 раза светоиндикаторы, USB Micro и 3,5 mm)
- Необходимые кабели: кабель Flex, USB Micro Type-B to USB-A
- Приветственная карта
- Механическая монтирование пластин с интегрированным подключением серии 80/20 1010


<!---

## How it works

Azure Percept Audio passes the audio input to the Azure Percept DK carrier board in a hybrid edge-cloud manner. Specifically,

- The Azure Percept Audio device: processes the incoming speech input to the clearest format by executing beam forming and echo cancellation befor sending the input to the Azure Percept DK. 
- The Azure Percept DK uses edge processing to perform keyword spotting and then sends the relevant inputs to Azure speech services.
- Cloud: Processing of natural language commands and phrases, in addition to keyword verification and retraining.
- Offline: If the device is offline it will detect the keyword and capture telemetry that there is no internet connection at the time of the command. It will not be able to weed out false accepts since it cannot perform keyword verification.

-->

## <a name="getting-started"></a>Начало работы

- [Сборка Azure Перцепт DK](./quickstart-percept-dk-unboxing.md)
- [Завершение процесса установки Azure Перцепт DK](./quickstart-percept-dk-set-up.md)
- [Подключение устройства Azure Перцепт Audio к DevKit](./quickstart-percept-audio-setup.md)

### <a name="build-a-no-code-prototype"></a>Создание прототипа без кода

Создавайте [речевое решение без кода](./tutorial-no-code-speech.md) с помощью шаблонов голосового помощника по Azure перцепт для сферы здравоохранения, медицинского обслуживания, инвентаризации и автомобильных сценариев.

## <a name="additional-technical-information"></a>Дополнительные технические сведения

- [Спецификации Azure Percept Audio](./azure-percept-audio-datasheet.md)

## <a name="next-steps"></a>Дальнейшие действия

Закажите аудиоустройство Azure Перцепт в [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270).