---
title: Параметры сетевой маршрутизации
titleSuffix: Azure Storage
description: Параметр сетевая маршрутизация позволяет указать, как сетевой трафик направляется в вашу учетную запись от клиентов через Интернет.
services: storage
author: santoshc
ms.service: storage
ms.topic: conceptual
ms.date: 02/11/2021
ms.author: santoshc
ms.reviewer: normesta
ms.subservice: common
ms.custom: references_regions
ms.openlocfilehash: 6b6c90259c552895360281b393e15773c6e101e3
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101726943"
---
# <a name="network-routing-preference-for-azure-storage"></a>Параметры сетевой маршрутизации для службы хранилища Azure

Вы можете настроить [Параметры сетевой маршрутизации](../../virtual-network/routing-preference-overview.md) для учетной записи хранения Azure, чтобы указать, как сетевой трафик направляется в вашу учетную запись от клиентов через Интернет. По умолчанию трафик из Интернета направляется в общедоступную конечную точку вашей учетной записи хранения через [глобальную сеть Майкрософт](../../networking/microsoft-global-network.md). Служба хранилища Azure предоставляет дополнительные возможности для настройки способа маршрутизации трафика в учетную запись хранения.

Настройка предпочтительного варианта маршрутизации позволяет оптимизировать передачу трафика в соответствии со стоимостью или производительностью сети уровня "Премиум". При настройке предпочтительного варианта маршрутизации вы определяете, как по умолчанию будет направляться трафик в общедоступную конечную точку для вашей учетной записи хранения. Вы также можете опубликовать конечные точки учетной записи хранения для конкретного маршрута.

> [!NOTE]
> Эта функция не поддерживается в учетных записях хранения, настроенных для использования уровня производительности Premium или хранилища, избыточного в виде зоны (ZRS).

## <a name="microsoft-global-network-versus-internet-routing"></a>Маршрутизация через глобальную сеть Майкрософт или через Интернет

По умолчанию клиенты за пределами среды Azure обращаются к учетной записи хранения через глобальную сеть Майкрософт. Глобальная сеть Майкрософт оптимизирована для выбора пути с низкой задержкой, что обеспечивает высокую надежность и оптимальную производительность сети. Входящий и исходящий трафик направляется через ближайшую к клиенту точку присутствия. Стандартная конфигурация маршрутизации гарантирует, что входящий и исходящий трафик учетной записи хранения большую часть пути проходит через глобальную сеть Майкрософт, что повышает производительность.

Вы можете изменить для учетной записи хранения конфигурацию маршрутизации так, чтобы входящий и исходящий трафик направлялся к клиентам и от клиентов за пределами среды Azure через точку присутствия, которая находится ближе всего к учетной записи хранения. Такой маршрут минимизирует перемещение трафика по глобальной сети Майкрософт, передавая его транзитному поставщику услуг Интернета при первой же возможности. Использование этой конфигурации маршрутизации снижает затраты на сеть.

На следующей схеме показано, как проходит трафик между клиентом и учетной записью хранения при использовании этих двух предпочтительных вариантов маршрутизации:

![Обзор вариантов маршрутизации для службы хранилища Azure](media/network-routing-preference/routing-options-diagram.png)

Дополнительные сведения о предпочтениях маршрутизации в Azure см. в разделе [что такое маршрутизация предпочтений?](../../virtual-network/routing-preference-overview.md).

## <a name="routing-configuration"></a>Конфигурация маршрутизации

Пошаговое руководство, в котором показано, как настроить параметры маршрутизации и конечные точки для конкретных маршрутов, см. в статье [Настройка параметров сетевой маршрутизации для службы хранилища Azure](configure-network-routing-preference.md).

В качестве параметров маршрутизации по умолчанию для общедоступной конечной точки вашей учетной записи хранения можно выбрать между глобальной сетью и маршрутизацией в Интернете Майкрософт. Настройка предпочтительного варианта маршрутизации по умолчанию применяется ко всему трафику от клиентов за пределами сети Azure и влияет на конечные точки Azure Data Lake Storage 2-го поколения, хранилища BLOB-объектов, Файлов Azure и статических сайтов. Настройка предпочтительного варианта маршрутизации не поддерживается для очередей Azure и таблиц Azure.

Вы также можете опубликовать конечные точки учетной записи хранения для конкретного маршрута. При публикации конечных точек для конкретного маршрута служба хранилища Azure создает в учетной записи хранения новые общедоступные конечные точки, которые направляют трафик по нужному пути. Такая гибкость позволяет направлять трафик учетной записи хранения по конкретному маршруту, не меняя предпочтительный вариант маршрутизации по умолчанию.

Например, публикация конечной точки для конкретного маршрута в Интернете для "Сторажеаккаунта" приведет к публикации следующих конечных точек для учетной записи хранения:

| Служба хранилища        | Конечная точка для конкретного маршрута                                  |
| :--------------------- | :------------------------------------------------------- |
| Служба больших двоичных объектов           | `StorageAccountA-internetrouting.blob.core.windows.net`  |
| Data Lake Storage 2-го поколения | `StorageAccountA-internetrouting.dfs.core.windows.net`   |
| Служба файлов           | `StorageAccountA-internetrouting.file.core.windows.net`  |
| Статические веб-сайты        | `StorageAccountA-internetrouting.web.core.windows.net`   |

Если у вас есть хранилище RA-GRS (геоизбыточное с доступом на чтение) или RA-GZRS (геоизбыточное между зонами с доступом на чтение), публикация конечных точек для конкретного маршрута автоматически создает дополнительные конечные точки в дополнительном регионе с доступом на чтение.

| Служба хранилища        | Вторичная конечная точка для конкретного маршрута с доступом только для чтения                        |
| :--------------------- | :----------------------------------------------------------------- |
| Служба больших двоичных объектов           | `StorageAccountA-internetrouting-secondary.blob.core.windows.net`  |
| Data Lake Storage 2-го поколения | `StorageAccountA-internetrouting-secondary.dfs.core.windows.net`   |
| Служба файлов           | `StorageAccountA-internetrouting-secondary.file.core.windows.net`  |
| Статические веб-сайты        | `StorageAccountA-internetrouting-secondary.web.core.windows.net`   |

Строки подключения для опубликованных конечных точек для конкретного маршрута можно скопировать на [портале Azure](https://portal.azure.com). Эти строки подключения можно использовать для авторизации с общим ключом во всех существующих пакетах SDK и API службы хранилища Azure.

## <a name="regional-availability"></a>Доступность по регионам

Предпочтительный вариант маршрутизации для службы хранилища Azure доступен в следующих регионах:

- Центральная часть США 
- Центральная часть США (EUAP)
- Восточная часть США 
- восточная часть США 2
- восточная часть США 2 
- Восточная часть США 2 (EUAP)
- Центрально-южная часть США
- центрально-западная часть США
- западная часть США 
- западная часть США 2 
- Центральная Франция 
- Южная Франция 
- Северная Германия 
- Центрально-Западная Германия 
- Центрально-северная часть США
- Северная Европа 
- Восточная Норвегия; 
- Северная Швейцария
- Западная Швейцария
- южная часть Соединенного Королевства 
- западная часть Соединенного Королевства 
- Западная Европа 
- Центральная часть ОАЭ.
- Восточная Азия 
- Юго-Восточная Азия 
- Japan East 
- Западная Япония 
- Западная Индия
- Восточная Австралия 
- Юго-Восточная часть Австралии 

Следующие известные проблемы влияют на параметры маршрутизации для службы хранилища Azure.

- Запросы на доступ к конечной точке для конкретного маршрута, направляемого через глобальную сеть Майкрософт, возвращают ошибку HTTP 404 или что-то похожее. Маршрутизация через глобальную сеть Майкрософт работает нормально, только если она настроена как предпочтительный вариант по умолчанию для общедоступной конечной точки.

## <a name="pricing-and-billing"></a>Цены и выставление счетов

Сведения о ценах и выставлении счетов см. в разделе " **цены** " статьи [параметры маршрутизации](../../virtual-network/routing-preference-overview.md#pricing).

## <a name="next-steps"></a>Дальнейшие действия

- [Что такое предпочтение маршрутизации?](../../virtual-network/routing-preference-overview.md)
- [Настройка предпочтения маршрутизации для сети](configure-network-routing-preference.md)
- [Настройка брандмауэров службы хранилища Azure и виртуальных сетей](storage-network-security.md)
- [Рекомендации по обеспечению безопасности для хранилища BLOB-объектов](../blobs/security-recommendations.md)
