---
title: Просмотр видеопотока RTSP в Azure Перцепт DK
description: Узнайте, как просмотреть видеопоток RTSP из SoM концепции в Azure Перцепт DK.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 20fb8495e17d4294351a50c3bc97436de9f03450
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101662988"
---
# <a name="view-your-azure-percept-dks-rtsp-video-stream"></a>Просмотр видеопотока RTSP в Azure Перцепт DK

Следуйте указаниям в этом разделе, чтобы просмотреть видеопоток RTSP из SoM концепции Azure Перцепт DK в Azure Перцепт Studio. В веб-потоке будет отображаться возможность получить представление о моделях AI, развернутых на устройстве.

## <a name="prerequisites"></a>Предварительные требования

- Azure Percept DK (DevKit).
- [Подписка Azure.](https://azure.microsoft.com/free/)
- [Выполненная настройка Azure Percept DK](./quickstart-percept-dk-set-up.md). Предполагается, что вы уже подключили DevKit к сети Wi-Fi, создали центр Интернета вещей и подключили к нему DevKit.

## <a name="view-the-rtsp-video-stream"></a>Просмотр видеопотока RTSP

1. Включите свой DevKit.

1. Перейдите в [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. В левой части страницы Обзор щелкните **устройства**.

    :::image type="content" source="./media/how-to-view-video-stream/overview-devices-inline.png" alt-text="Экран обзора Azure Percept Studio." lightbox="./media/how-to-view-video-stream/overview-devices.png":::

1. Выберите DevKit из списка.

    :::image type="content" source="./media/how-to-view-video-stream/select-device.png" alt-text="Экран обзора Azure Percept Studio.":::

1. Щелкните **Просмотр потока устройства**.

    :::image type="content" source="./media/how-to-view-video-stream/view-device-stream.png" alt-text="Экран обзора Azure Percept Studio.":::

    Откроется отдельная вкладка, на которой показан динамический веб-поток из SoM-элемента концепции Azure Перцепт DK.

    :::image type="content" source="./media/how-to-view-video-stream/webstream.png" alt-text="Экран обзора Azure Percept Studio.":::

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как просмотреть данные [телеметрии Azure ПЕРЦЕПТ DK](./how-to-view-telemetry.md).