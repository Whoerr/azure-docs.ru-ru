---
title: Подключение передней дверцы Azure к источнику с помощью частной ссылки
description: Узнайте, как подключить переднюю дверь Azure к источнику со службой Private Link с помощью портал Azure.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: tyao
ms.openlocfilehash: 3015a0560171b61aaab05e27b369d9ca531e81c1
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2021
ms.locfileid: "101099458"
---
# <a name="connect-azure-front-door-premium-to-your-origin-with-private-link"></a>Подключение передней дверцы Azure к источнику с помощью частной ссылки

В этой статье вы узнаете, как настроить номер SKU передней дверцы Azure для подключения к приложениям, размещенным в виртуальной сети, с помощью службы частной связи Azure.

## <a name="prerequisites"></a>Предварительные требования

* Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) бесплатно.
* Создайте службу [частной связи](../../private-link/create-private-link-service-portal.md) для исходных веб-серверов.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com).

## <a name="enable-private-endpoint-in-azure-front-door-service"></a>Включение частной конечной точки в службе "Передняя дверь" Azure

В этом разделе вы будете сопоставлять службу частной связи Azure с частной конечной точкой, созданной в частной сети "номер SKU передней дверцы Azure". 

1. В профиле "Премиум" на передней дверце Azure в разделе " *Параметры*" выберите **исходные группы**.

1. Выберите исходную группу, содержащую источник, для которого требуется включить закрытую ссылку.

1. Выберите **+ Добавить источник** , чтобы добавить новый источник, или выберите ранее созданный источник из списка. Затем установите флажок, чтобы **включить службу закрытых ссылок**.

    :::image type="content" source="../media/how-to-enable-private-link/front-door-private-endpoint-private-link.png" alt-text="Снимок экрана: включение закрытой ссылки в странице &quot;Добавление источника&quot;.":::

1. Для **выбора ресурса Azure** выберите **в моем каталоге**. Выберите или введите следующий параметр, чтобы настроить ресурс, к которому будет подключаться передняя дверь Azure Premium.
    
    | Параметр | Значение |
    | ------- | ----- |
    | Регион | Выберите область, которая является той же или ближайшей к источнику. |
    | Тип ресурса | Выберите **Microsoft.Network/privateLinkServices**. |
    | Ресурс | Выберите **myPrivateLinkService**. |
    | Целевой вспомогательный ресурс | Оставьте это поле пустым. |
    | Сообщение запроса | Настройте сообщение или выберите сообщение по умолчанию. |

## <a name="next-steps"></a>Next Steps

Сведения о [закрытой ссылке "Премиум" на передней дверце Azure](concept-private-link.md).
