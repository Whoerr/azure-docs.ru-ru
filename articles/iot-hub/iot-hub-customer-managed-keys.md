---
title: Шифрование данных в центре Интернета вещей Azure с помощью ключей, управляемых клиентом | Документация Майкрософт
description: Шифрование неактивных данных с помощью управляемых клиентом ключей для центра Интернета вещей
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/17/2020
ms.author: asrastog
ms.openlocfilehash: 352da24b36124ff0446a81c1ecbc584da545bb16
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2020
ms.locfileid: "92142196"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>Шифрование неактивных данных с помощью управляемых клиентом ключей для центра Интернета вещей

Центр Интернета вещей поддерживает шифрование неактивных данных с помощью управляемых клиентом ключей (CMK), также называемого собственным ключом (BYOK). Центр Интернета вещей Azure обеспечивает шифрование неактивных и транзитных данных, так как они записываются в наши центры обработки данных и расшифровываются для вас при доступе к нему. По умолчанию центр Интернета вещей использует ключи, управляемые корпорацией Майкрософт, для шифрования неактивных данных. С помощью CMK можно получить другой уровень шифрования поверх шифрования по умолчанию и можно выбрать шифрование неактивных данных с помощью ключа шифрования ключа, управляемого с помощью [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Это обеспечивает гибкость при создании, повороте, отключении и отмене элементов управления доступом. Если для центра Интернета вещей настроен BYOK, мы также предоставляем двойное шифрование, которое обеспечивает второй уровень защиты, позволяя управлять ключом шифрования с помощью Azure Key Vault.

Для этой возможности требуется создать центр Интернета вещей (уровень "базовый" или "Стандартный"). Чтобы испытать эту возможность, свяжитесь с нами через [службу поддержки Майкрософт](https://azure.microsoft.com/support/create-ticket/). Поделитесь своим названием организации и ИДЕНТИФИКАТОРом подписки при обращении в службу поддержки Майкрософт.


## <a name="next-steps"></a>Дальнейшие действия

* [Дополнительные сведения о центре Интернета вещей](./about-iot-hub.md)

* [Дополнительные сведения о Azure Key Vault](../key-vault/general/overview.md)