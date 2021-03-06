---
title: Добавление ресурсов Azure в решение Интернета вещей
description: В этом кратком руководстве показано, как настроить комплексное решение Интернета вещей с помощью Azure Defender для Интернета вещей.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: Shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/25/2021
ms.author: shhazam
ms.openlocfilehash: 8912e8d66ae0cc1b5dba80ee9aabb0fbd288e3c6
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809040"
---
# <a name="quickstart-configure-your-azure-defender-for-iot-solution"></a>Краткое руководство. Настройка решения Azure Defender для Интернета вещей

В этой статье показано, как выполнить начальную настройку решения для защиты Интернета вещей с помощью Defender для Интернета вещей.

## <a name="what-is-defender-for-iot"></a>Что такое Defender для Интернета вещей?

Defender для Интернета вещей обеспечивает эффективную комплексную защиту решений Интернета вещей на базе Azure.

Defender для Интернета вещей позволяет отслеживать решение Интернета вещей на одной панели мониторинга, где отображаются все ваши устройства Интернета вещей, платформы Интернета вещей и серверные ресурсы в Azure.

После включения в Центре Интернета вещей Defender для Интернета вещей автоматически идентифицирует другие службы Azure, также подключенные к Центру Интернета вещей и связанные с решением Интернета вещей.

В дополнение к автоматическому обнаружению связей можно также выбрать другие группы ресурсов Azure, которые следует пометить как часть решения "Интернет вещей".

Выбранные параметры позволяют добавлять целые подписки, группы ресурсов или отдельные ресурсы.

После определения всех связей ресурсов Defender для Интернета вещей использует Defender, чтобы предоставлять рекомендации по безопасности и оповещения для этих ресурсов.

## <a name="add-azure-resources-to-your-iot-solution"></a>Добавление ресурсов Azure в решение Интернета вещей

Чтобы добавить новый ресурс в решение Интернета вещей, выполните следующие действия:

1. Откройте **Центр Интернета вещей** на портале Azure.

1. В разделе **Безопасность** выберите **Обзор**, затем **Параметры**, а затем щелкните **Отслеживаемые ресурсы**.

1. Щелкните **Изменить** и выберите отслеживаемые ресурсы, принадлежащие решению Интернета вещей.

1. Выберите **Добавить**.

Поздравляем! Вы добавили новую группу ресурсов в решение "Интернет вещей".

Как часть решения Интернета вещей Defender для Интернета вещей теперь отслеживает недавно добавленные группы ресурсов, а также отображает соответствующие рекомендации по безопасности и оповещения.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы узнать, как создавать модули безопасности, перейдите к следующей статье.

> [!div class="nextstepaction"]
> [Quickstart: Create an azureiotsecurity module twin](quickstart-create-security-twin.md) (Краткое руководство по созданию модуля двойника устройства azureiotsecurity)
