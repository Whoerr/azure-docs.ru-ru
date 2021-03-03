---
title: Обзор Azure Percept DK
description: Дополнительные сведения об Azure Перцепт DK
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 4fd0a7cb575a109d1393527b48de3fa4e3446167
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663471"
---
# <a name="azure-percept-dk-overview"></a>Обзор Azure Percept DK

Azure Перцепт DK — это пограничная пакет разработки для искусственного интеллекта, предназначенный для разработки концепций искусственного интеллекта. В сочетании с [Azure Перцепт Studio](./overview-azure-percept-studio.md)это мощная, но простая в использовании платформа для создания пограничных решений ИСКУССТВЕНного интеллекта для широкого спектра концепций приложений искусственного интеллекта. Его можно приобрести в [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270).

:::image type="content" source="./media/overview-azure-percept-dk/dk-image.png" alt-text="Эскиз.":::

## <a name="key-features"></a>Основные возможности

- **Возможность запуска AI на границе**. Встроенное аппаратное ускорение позволяет выполнять модели искусственного интеллекта без подключения к облаку.
- **Встроенная в аппаратное обеспечение безопасности доверенного корня**. Дополнительные сведения см. в этом обзоре [системы безопасности Azure перцепт](./overview-percept-security.md) .
- **Простая интеграция с [Azure перцепт Studio](./overview-azure-percept-studio.md)** и другими службами Azure. Например, центр Интернета вещей Azure, Azure Cognitive Services и [аналитика видео в реальном времени](https://docs.microsoft.com/azure/media-services/live-video-analytics-edge/overview)
- **Поддержка основных платформ искусственного интеллекта**. Например ONNX и TensorFlow.
- **Интеграция с системой салазок 80/20**. Упрощение создания прототипов в рабочих средах. Дополнительные сведения об [интеграции 8/20](./overview-8020-integration.md).

## <a name="hardware-components"></a>Компоненты оборудования

- Плата перевозчика Azure Перцепт DK
    - Процессор НКСП iMX8m
    - Доверенный платформенный модуль (TPM) (TPM) версии 2,0
    - Подключение WiFi и Bluetooth
    - Просмотреть полный [лист данных](./azure-percept-dk-datasheet.md)
- Система концепции Azure Перцепт в модуле (SoM)
    - Вычислительный блок Intel Мовидиус множество X (MA2085) (ВПУ)
    - Датчик камеры RGB с возможностью добавления второго
    - Просмотреть полный [лист данных](./azure-percept-vision-datasheet.md)

## <a name="get-started-with-the-azure-percept-dk"></a>Начало работы с Azure Перцепт DK

- Заполните эти краткие руководства
    - [Распаковка и сборка Azure Перцепт DK](./quickstart-percept-dk-unboxing.md)
    - [Настройка Azure Перцепт DK и выполнение первой концепции модели искусственного интеллекта](./quickstart-percept-dk-set-up.md)
- Начните создавать эксперименты с помощью этих учебников
    - [Создание решения компьютерного зрения в Azure Percept Studio без необходимости писать код](./tutorial-nocode-vision.md)
    - [Создание речевого помощника в Azure Перцепт Studio](./tutorial-no-code-speech.md)

## <a name="next-steps"></a>Дальнейшие действия

Закажите Azure Перцепт DK в [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270).