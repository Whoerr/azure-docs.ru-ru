---
title: Возможности Azure для передачи крупных наборов данных при низкой или отсутствующей пропускной способности сети | Документация Майкрософт
description: Узнайте, как выбрать решение Azure для передачи данных в среде с ограниченной или отсутствующей пропускной способностью сети, если планируется передача крупных наборов данных.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 9b83ba106b35a0a3abd035e85f60c4c39bbadd3b
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2021
ms.locfileid: "98704646"
---
# <a name="data-transfer-for-large-datasets-with-low-or-no-network-bandwidth"></a>Передача данных для больших наборов данных с низкой или отсутствующей пропускной способностью сети
 
В этой статье представлен обзор решений для передачи данных в среде с ограниченной или отсутствующей пропускной способностью сети, если планируется передача крупных наборов данных. Также здесь описаны рекомендуемые варианты передачи данных и приведена матрица ключевых функций для указанного сценария.

Чтобы узнать больше обо всех доступных параметрах передачи данных, ознакомьтесь со статьей [Choose an Azure solution for data transfer](storage-choose-data-transfer-solution.md) (Выбор решения Azure для передачи данных).

## <a name="offline-transfer-or-network-transfer"></a>Автономная передача или передача по сети

Большие наборы данных означают, что у вас есть несколько терабайт или петабайт данных. У вас ограниченная или отсутствующая пропускная способность сети, ваша сеть медленна или ненадежна. Кроме того:

- Ваши поставщики услуг Интернета предлагают высокие тарифы на передачу данных по сети.
- политики безопасности или организации запрещают исходящие подключения при работе с конфиденциальными данными.

Во всех указанных выше случаях для выполнения однократной пакетной передачи данных используется физическое устройство. Выберите одно из устройств (Диск Data Box, Data Box, Data Box Heavy), которые предоставляются корпорацией Майкрософт, или выполните импорт и экспорт с использованием собственных дисков.

Чтобы проверить, является ли физическое устройство правильным вариантом, используйте следующую таблицу. В ней указаны прогнозы по времени передачи данных по сетям с разной пропускной способностью (при условии загрузки на 90 %). Если прогноз для передачи по сети окажется слишком медленным, следует использовать физическое устройство.  

![Передача по сети или вне сети](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

## <a name="recommended-options"></a>Рекомендуемые варианты

Варианты, доступные в этом сценарии, — это устройства для автономной передачи Azure Data Box или служба импорта и экспорта Azure.

- **Семейство Azure Data Box для передачи вне сети.** Устройства Data Box, предоставляемые корпорацией Майкрософт, удобны для перемещения больших объемов данных в Azure при наличии ограничений по времени, доступности сети или стоимости. Копируйте локальные данные с помощью специальных средств, таких как Robocopy. В зависимости от размера данных, предназначенных для передачи, можно выбрать Диск Data Box, Data Box или Data Box Heavy.
- **Импорт и экспорт Azure.** Используйте службу "Импорт и экспорт Azure" для безопасного переноса больших объемов данных на своих дисках в хранилище BLOB-объектов Azure и службу файлов Azure. Кроме того, эту службу можно использовать, чтобы переносить данные из хранилища BLOB-объектов Azure на диски и передавать на локальные сайты.

## <a name="comparison-of-key-capabilities"></a>Сравнение ключевых возможностей

В следующей таблице перечислены различия в ключевых возможностях.

|                                     |    Диск Data Box      |    Data Box                                      |    Data Box Heavy              |    Импорт и экспорт                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    **Размер данных**                    |    До 35 ТБ                 |    До 80 ТБ на устройство                       |    До 800 ТБ на устройство               |    Переменная                            |
|    **Data type**                    |    Большие двоичные объекты Azure                  |    Большие двоичные объекты Azure<br>Файлы Azure                    |    Большие двоичные объекты Azure<br>Файлы Azure            |    Большие двоичные объекты Azure<br>Файлы Azure          |
|    **Форм-фактор**                  |    Не более 5 SSD в одном заказе             |    1 x 23 кг одно устройство в заказе размером с настольный компьютер    |    1 X ~227 кг большое устройство в заказе    |    До 10 жестких дисков или твердотельных накопителей в одном заказе        |
|    **Время начальной настройки**           |    Низкий <br>(15 мин)            |    От низкой до умеренной сложности <br> (менее 30 мин)               |    Средняя<br>(1–2 ч)               |    От умеренной до высокой сложности<br>(переменная) |
|    **Отправка данных в Azure**           |    Да                          |    Да                                           |    Да                                   |    Да                                 |
|    **Экспорт данных из Azure**       |    Нет                           |    Нет                                            |    Нет                                    |    Да                                 |
|    **Шифрование**                   |    AES, 128-битное                  |    AES, 256-битное                                   |    AES, 256-битное                           |    AES, 128-битное                         |
|    **Оборудование**                     |     Предоставляется корпорацией Майкрософт          |    Предоставляется корпорацией Майкрософт                            |    Предоставляется корпорацией Майкрософт                    |    Предоставляется пользователем                   |
|    **Сетевой интерфейс**            |    USB 3.1 или SATA                 |    RJ 45, SFP+                                   |    RJ45, QSFP+                           |    SATA II или SATA III                    |
|    **Интеграция партнеров**          |    Некотор.                         |    [Высокая](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureExpressPod)                                          |    [Высокая](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureExpressPod)                                  |    Некотор.                                |
|    **Отправка**                     |    Организуется корпорацией Майкрософт            |    Организуется корпорацией Майкрософт                             |    Организуется корпорацией Майкрософт                     |    Организуется пользователем                    |
| **Используется при перемещении данных**     |В пределах коммерческого региона|В пределах коммерческого региона|В пределах коммерческого региона|Между любыми географическими регионами, например из США в ЕС|
|    **Цены**                      |    [Цены](https://azure.microsoft.com/pricing/details/databox/disk/)                    |   [Цены](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Цены](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Цены](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


## <a name="next-steps"></a>Следующие шаги

- Практические руководства по:

    - [Перенесите данные с диск Data Box](../../databox/data-box-disk-quickstart-portal.md).
    - [Перенесите данные с Data Box](../../databox/data-box-quickstart-portal.md).
    - [Перенесите данные с помощью импорта и экспорта](../../import-export/storage-import-export-data-to-blobs.md).