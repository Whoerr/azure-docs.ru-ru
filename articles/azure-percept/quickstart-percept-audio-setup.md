---
title: Начало работы с Azure Percept Audio
description: Сведения о том, как подключить устройство Azure Percept Audio к Azure Percept DK.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/18/2021
ms.custom: template-quickstart
ms.openlocfilehash: 575107859f56df742ab41a299269c250511022b3
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664418"
---
# <a name="azure-percept-audio-setup"></a>Настройка Azure Percept Audio

Azure Percept Audio поставляется полностью готовым к работе с Azure Percept DK. Никаких специальных настроек не требуется.

## <a name="prerequisites"></a>Предварительные требования

- Azure Percept DK (DevKit).
- Azure Percept Audio
- [Подписка Azure.](https://azure.microsoft.com/free/)
- [Выполненная настройка Azure Percept DK](./quickstart-percept-dk-set-up.md). Предполагается, что вы уже подключили DevKit к сети Wi-Fi, создали Центр Интернета вещей и подключили к нему DevKit.

## <a name="connecting-your-devices"></a>Подключение устройств

1. Подключите устройство Azure Percept Audio к плате-носителю Azure Percept DK с помощью предоставленного кабеля USB Micro Type-B — USB Type-A. Подключите разъем Micro Type-B этого кабеля к Audio SoM, а разъем Type-A — к плате-носителю Azure Percept DK.

1. Включите DevKit.

    - Индикатор L01 на Audio SoM загорится зеленым цветом, что подтверждает подачу питания на устройство.
    - Индикатор L02 будет мигать зеленым цветом, что означает процесс аутентификации Audio SoM.

1. Дождитесь завершения процесса проверки подлинности — это может занять до 3 минут.

1. Вы можете приступать к созданию прототипов, дождавшись любого из следующих событий:

    - Индикатор L01 погаснет, а L02 станет белым. Это означает, что аутентификация успешно завершена, а для DevKit еще не настроено ключевое слово.
    - Все три индикатора загорятся синим цветом. Это означает, что аутентификация успешно завершена, и для DevKit настроено ключевое слово.

    > [!NOTE]
    > Если DevKit не сможет выполнить проверку подлинности, обратитесь в службу поддержки.

## <a name="next-steps"></a>Дальнейшие действия

Создайте [решение по работе с речью без кода](./tutorial-no-code-speech.md).