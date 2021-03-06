---
title: Масштабирование зарезервированных единиц мультимедиа (Мрус) CLI
description: В этом разделе показано, как использовать CLI для масштабирования обработки мультимедиа с использованием служб мультимедиа Azure.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/30/2020
ms.author: inhenkel
ms.openlocfilehash: 98e87cf9d1f46ddb8ee1d433bd0b0ba8806fac89
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2021
ms.locfileid: "101091921"
---
# <a name="how-to-scale-media-reserved-units"></a>Как масштабировать зарезервированные единицы мультимедиа

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

В этой статье показано, как масштабировать зарезервированные единицы мультимедиа (Мрсс) для ускорения кодирования.

## <a name="prerequisites"></a>Предварительные требования

[Создание учетной записи Служб мультимедиа](./create-account-howto.md).

Общие сведения о [зарезервированных единицах мультимедиа](concept-media-reserved-units.md).

## <a name="scale-media-reserved-units-with-cli"></a>Масштабирование зарезервированных единиц кодирования с помощью интерфейса командной строки

Выполните команду `mru`.

Следующая команда [az ams account mru](/cli/azure/ams/account/mru?view=azure-cli-latest) настраивает зарезервированные единицы мультимедиа в учетной записи amsaccount с использованием параметров **count** и **type**.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Выставление счетов

Вы платите за количество минут, в течение которых зарезервированные единицы мультимедиа подготавливаются в вашей учетной записи. Это происходит независимо от того, выполняются ли в вашей учетной записи какие-либо задания. Подробное описание см. в разделе часто задаваемых вопросов на странице [цен на службы мультимедиа](https://azure.microsoft.com/pricing/details/media-services/).   

## <a name="next-step"></a>Дальнейшие действия

[Анализ видео](analyze-videos-tutorial-with-api.md)

## <a name="see-also"></a>См. также

* [Квоты и ограничения](limits-quotas-constraints.md)
