---
title: Развертывание концепции модели искусственного интеллекта в Azure Перцепт DK
description: Узнайте, как развернуть модель искусственного интеллекта в Azure Перцепт DK из Azure Перцепт Studio.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 5ad263f5ae3b0b8e91de30b620002e33086608b4
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101662858"
---
# <a name="capture-images-for-a-vision-project"></a>Запись изображений для проекта визуального распознавания

Следуйте указаниям этого раздела, чтобы записать образы с помощью SoM концепции Azure Перцепт DK для существующего проекта концепции в Azure Перцепт Studio. Если вы еще не создали концепцию проекта, см. Руководство по [созданию непрограммных концепций](./tutorial-nocode-vision.md).

## <a name="prerequisites"></a>Предварительные требования

- Azure Percept DK (DevKit).
- [Подписка Azure.](https://azure.microsoft.com/free/)
- [Выполненная настройка Azure Percept DK](./quickstart-percept-dk-set-up.md). Предполагается, что вы уже подключили DevKit к сети Wi-Fi, создали центр Интернета вещей и подключили к нему DevKit.
- [Проект концепции без кода](./tutorial-nocode-vision.md)

## <a name="capture-images"></a>Образы записи

1. Включите свой DevKit.

1. Перейдите в [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. В левой части страницы Обзор щелкните **устройства**.

    :::image type="content" source="./media/how-to-capture-images/overview-devices-inline.png" alt-text="Экран обзора Azure Percept Studio." lightbox="./media/how-to-capture-images/overview-devices.png":::

1. Выберите DevKit из списка.

    :::image type="content" source="./media/how-to-capture-images/select-device.png" alt-text="Список устройств перцепт.":::

1. На странице устройства нажмите кнопку **захватить образы для проекта**.

    :::image type="content" source="./media/how-to-capture-images/capture-images.png" alt-text="Страница устройств перцепт с доступными действиями в списке.":::

1. В окне **Capture Image (запись образа** ) выполните следующие действия.

    1. В раскрывающемся меню **проекта** выберите проект концепции, для которого требуется получить образы.

    1. Нажмите кнопку **Просмотр потока устройства** , чтобы убедиться, что камера правильно размещена SoM концепции.

    1. Щелкните **взять фотографию** , чтобы записать изображение.

    1. Кроме того, установите флажок для параметра **Автоматическая запись образа** , чтобы настроить таймер для записи образа.

        1. Выберите предпочтительную скорость работы с образами в разделе **Частота захвата**.
        1. Выберите общее число образов, которые вы хотите получать в рамках **целевого объекта**.

    :::image type="content" source="./media/how-to-capture-images/take-photo.png" alt-text="Страница устройств перцепт с доступными действиями в списке.":::

Все изображения будут доступны в [пользовательское визуальное распознавание](https://www.customvision.ai/).

## <a name="next-steps"></a>Дальнейшие действия

[Протестируйте и переучить модель концепции без кода](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/test-your-model).