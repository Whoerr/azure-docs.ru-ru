---
title: Настройка пользовательского доменного имени для самостоятельно размещенного шлюза управления API Azure | Документация Майкрософт
description: В этом разделе описаны действия по настройке пользовательского доменного имени для самостоятельно размещенного шлюза управления API Azure.
services: api-management
documentationcenter: ''
author: vladvino
ms.service: api-management
ms.topic: article
ms.date: 03/31/2020
ms.author: apimpm
ms.openlocfilehash: d52bf87b74ae9b1770ed5092738fd05eb9f54fde
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2021
ms.locfileid: "99491036"
---
# <a name="configure-a-custom-domain-name-for-a-self-hosted-gateway"></a>Настройка пользовательского доменного имени для самостоятельно размещенного шлюза

При подготовке [самостоятельно размещенного шлюза управления API Azure](self-hosted-gateway-overview.md)ему не назначается имя узла, и он должен ссылаться по его IP-адресу. В этой статье показано, как сопоставлять существующее пользовательское DNS-имя (также называемое именем узла) с локальным шлюзом.

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить действия, описанные в этой статье, необходимо следующее.

-   Активная подписка Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   Экземпляр управления API. Дополнительные сведения см. в статье о [создании экземпляра управления API Azure](get-started-create-service-instance.md).
- Самостоятельно размещенный шлюз. Дополнительные сведения см. [в разделе как подготавливать самостоятельный шлюз](api-management-howto-provision-self-hosted-gateway.md) .
-   Пользовательское доменное имя, которое принадлежит вам или вашей организации. В этом разделе не приводятся инструкции по получению пользовательского доменного имени.
-   Запись DNS, размещенная на DNS-сервере, который сопоставляет имя личного домена с IP-адресом самостоятельно размещенного шлюза. В этом разделе не приводятся инструкции по размещению записи DNS.
-   Необходимо иметь действующий сертификат с открытым и закрытым ключами (PFX-файл). Субъект или альтернативное имя субъекта (SAN) должны соответствовать доменному имени (это позволяет экземпляру управления API безопасно предоставлять URL-адреса по протоколу TLS).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-custom-domain-certificate-to-your-api-management-service"></a>Добавление пользовательского сертификата домена в службу управления API

Добавьте пользовательский сертификат домена (. PFX) в экземпляре управления API или ссылку на сертификат, хранящийся в Azure Key Vault. Выполните действия в разделе [безопасные серверные службы с использованием проверки подлинности сертификата клиента в службе управления API Azure](api-management-howto-mutual-certificates.md).

> [!NOTE]
> Мы рекомендуем использовать сертификат хранилища ключей для домена автономного шлюза.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name-for-your-self-hosted-gateway"></a>Используйте портал Azure, чтобы задать пользовательское доменное имя для самостоятельно размещенного шлюза.

1. Выберите **шлюзы** в разделе **развертывание и инфраструктура**.
2. Выберите самостоятельно размещенный шлюз, для которого нужно настроить доменное имя.
3. В разделе **Параметры** выберите **имена узлов** .
4. Выберите **+ Добавить**.
5. Введите имя ресурса для имени узла в поле **имя** .
6. Введите доменное имя в поле **HostName** .
7. Выберите сертификат из раскрывающегося списка **сертификат** .
8. Установите флажок **согласовать сертификат клиента** , если любой из API, предоставляемых через этот шлюз, использует проверку подлинности на основе сертификата клиента.
    > [!WARNING]
    > Этот параметр является общим для всех доменных имен, настроенных для шлюза.
9. Нажмите кнопку **Добавить** , чтобы назначить имя личного домена выбранному автономному шлюзу.

## <a name="next-steps"></a>Дальнейшие действия

[Повышение категории и масштабирование службы](upgrade-and-scale.md)
