---
title: Краткое руководство. Реализация взаимодействия в Teams с помощью Служб коммуникации Azure
titleSuffix: An Azure Communication Services quickstart
description: Из этого краткого руководства вы узнаете, как присоединиться к собранию в Teams с помощью пакета SDK вызовов для Служб коммуникации Azure.
author: matthewrobertson
ms.author: chpalm
ms.date: 10/10/2020
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-plat-web-ios-android
ms.openlocfilehash: 6da700fdd8149a7fff92ed0edef2015e354eca05
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660086"
---
# <a name="quickstart-join-your-calling-app-to-a-teams-meeting"></a>Краткое руководство. Подключение приложения для звонков к собранию в Teams

> [!IMPORTANT]
> Чтобы включить или отключить [взаимодействие арендаторов в Teams](../concepts/teams-interop.md), заполните [эту форму](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u).

Приступите к работе со Службами коммуникации Azure, подключив решение для вызовов к Microsoft Teams с помощью клиентской библиотеки JavaScript.

::: zone pivot="platform-web"
[!INCLUDE [Calling with JavaScript](./includes/teams-interop-javascript.md)]
::: zone-end

::: zone pivot="platform-android"
[!INCLUDE [Calling with Android](./includes/teams-interop-android.md)]
::: zone-end

::: zone pivot="platform-ios"
[!INCLUDE [Calling with iOS](./includes/teams-interop-ios.md)]
::: zone-end

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите очистить и удалить подписку на Службы коммуникации, вы можете удалить ресурс или группу ресурсов. При этом удаляются все ресурсы, связанные с ней. См. сведения об [очистке ресурсов](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в следующих статьях:

- Ознакомьтесь с нашим [главным примером функции вызовов](../../samples/calling-hero-sample.md).
- Изучите [возможности клиентской библиотеки для вызовов](./calling-client-samples.md).
- Узнайте больше о [принципе работы функции вызовов](../../concepts/voice-video-calling/about-call-types.md).
