---
title: Развертывание концепции модели искусственного интеллекта в Azure Перцепт DK
description: Узнайте, как развернуть модель искусственного интеллекта в Azure Перцепт DK из Azure Перцепт Studio.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 6e1ed39edfd3c395fbc3e4d26a4aa358d48a1d5b
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663079"
---
# <a name="deploy-a-vision-ai-model-to-your-azure-percept-dk"></a>Развертывание концепции модели искусственного интеллекта в Azure Перцепт DK

Следуйте этому руководству, чтобы развернуть модель искусственного интеллекта в Azure Перцепт DK из Azure Перцепт Studio.

## <a name="prerequisites"></a>Предварительные требования

- Azure Percept DK (DevKit).
- [Подписка Azure.](https://azure.microsoft.com/free/)
- [Выполненная настройка Azure Percept DK](./quickstart-percept-dk-set-up.md). Предполагается, что вы уже подключили DevKit к сети Wi-Fi, создали центр Интернета вещей и подключили к нему DevKit.

## <a name="model-deployment"></a>Развертывание модели

1. Включите свой DevKit.

1. Перейдите в [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. В левой части страницы Обзор щелкните **устройства**.

    :::image type="content" source="./media/how-to-deploy-model/overview-devices-inline.png" alt-text="Экран обзора Azure Percept Studio." lightbox="./media/how-to-deploy-model/overview-devices.png":::

1. Выберите DevKit из списка.

    :::image type="content" source="./media/how-to-deploy-model/select-device.png" alt-text="Список устройств перцепт.":::

1. На следующей странице щелкните **развернуть образец модели** , если хотите развернуть одну из моделей предварительно обученных образцов. Если вы хотите развернуть существующее [пользовательское решение без кода](./tutorial-nocode-vision.md), щелкните **развернуть проект пользовательское визуальное распознавание**.

    :::image type="content" source="./media/how-to-deploy-model/deploy-model.png" alt-text="Список устройств перцепт.":::

1. Если вы решили развернуть решение без концепции кода, выберите проект и предпочтительную итерацию модели и нажмите кнопку **развернуть**.

1. Если вы выбрали развертывание образца модели, выберите модель и щелкните **развернуть на устройстве**.

    :::image type="content" source="./media/how-to-deploy-model/select-sample-model.png" alt-text="Список устройств перцепт.":::

1. После успешного развертывания модели вы получите сообщение о состоянии в правом верхнем углу экрана. Чтобы просмотреть сведения о модели в действии, щелкните ссылку **просмотреть поток** в сообщении о состоянии, чтобы просмотреть видеопоток RTSP из SoM концепции DevKit.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как просмотреть данные [телеметрии Azure ПЕРЦЕПТ DK](how-to-view-telemetry.md).