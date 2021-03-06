---
title: Виртуальные машины Azure серии NCv3
description: Спецификации виртуальных машин серии NCv3.
ms.subservice: hpc
author: vikancha-MSFT
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: d42b4eac09579dfa09cc07bfbbfffa1bbdaa65b5
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101670210"
---
# <a name="ncv3-series"></a>Серия NCv3

Виртуальные машины серии NCv3 созданы на базе GPU Tesla V100 от NVIDIA. Эти GPU могут увеличить производительность вычислений в 1,5 раза по сравнению с серией NCv2. Клиенты могут воспользоваться этими обновленными GPU для традиционных рабочих нагрузок HPC. Вы сможете реализовать такие сценарии, как пластовое моделирование, секвенирование ДНК, анализ белков, моделирование методом Монте-Карло и другие. Конфигурация NC24rs версии 3 обеспечивает низкую задержку и высокоскоростной сетевой интерфейс. Она оптимизирована для выполнения тесно взаимосвязанных параллельных вычислений Помимо графических процессоров, виртуальные машины серии NCv3 также работают на базе процессоров Intel Xeon Broadwell-2690 v4.

[Хранилище класса Premium](premium-storage-performance.md): поддерживается<br>
[Кэширование хранилища класса Premium](premium-storage-performance.md): поддерживается<br>
[Динамическая миграция](maintenance-and-updates.md): не поддерживается<br>
[Обновления с сохранением памяти](maintenance-and-updates.md): не поддерживается<br>
[Поддержка создания виртуальных машин](generation-2.md): поколение 1 и 2<br>
[Ускоренная сеть](../virtual-network/create-vm-accelerated-networking-cli.md): не поддерживается<br>
[Временные диски ОС](ephemeral-os-disks.md): не поддерживаются <br>
Нвлинкное Interconnect NVIDIA: не поддерживается<br>

> [!IMPORTANT]
> Для этой серии виртуальных машин квота виртуальных ЦП (Core) в вашей подписке изначально равна 0 в каждом регионе. [Запросите увеличение квоты виртуальных ЦП](../azure-portal/supportability/resource-manager-core-quotas-request.md) для этой серии в [доступном регионе](https://azure.microsoft.com/regions/services/). Эти номера SKU недоступны для пробной версии или подписки Azure для подписчиков Visual Studio. Ваш уровень подписки может не поддерживать выбор или развертывание этих номеров SKU. 
>

| Размер | vCPU | Память: ГиБ | Временное хранилище (SSD): ГиБ | Графический процессор | Память GPU: ГиБ | Максимальное число дисков данных | Максимальная пропускная способность дисков без кэширования: операций ввода-вывода в секунду / МБит/с | Максимальное число сетевых адаптеров |
|---|---|---|---|---|---|---|---|---|
| Standard_NC6s_v3    | 6  | 112 | 736  | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v3   | 12 | 224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v3   | 24 | 448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |
| Standard_NC24rs_v3* | 24 | 448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = 1 карта V100.

*С поддержкой RDMA

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Поддерживаемые операционные системы и драйверы

Чтобы воспользоваться преимуществами возможностей GPU виртуальных машин Azure серии N, необходимо установить драйверы GPU NVIDIA.

[Расширение драйвера GPU NVIDIA](./extensions/hpccompute-gpu-windows.md) устанавливает необходимые драйверы CUDA или GRID NVIDIA на виртуальную машину серии N. Для установки расширения и управления им можно использовать портал Azure или такие инструменты, как Azure PowerShell и шаблоны Azure Resource Manager. Сведения о поддерживаемых операционных системах и этапах развертывания см. в [документации по расширению драйвера GPU NVIDIA](./extensions/hpccompute-gpu-windows.md). Общие сведения о расширениях виртуальных машин см. в статье [Расширения и компоненты виртуальных машин Azure](./extensions/overview.md).

Если вы решили установить драйверы NVIDIA GPU вручную, см. раздел [Установка драйвера GPU серии n для Windows](./windows/n-series-driver-setup.md) или [Установка драйвера GPU для Linux](./linux/n-series-driver-setup.md) для поддерживаемых операционных систем, драйверов, установки и проверки.

## <a name="other-sizes"></a>Остальные размеры

- [Универсальные](sizes-general.md)
- [Оптимизированные для памяти](sizes-memory.md)
- [Оптимизированные для хранилища](sizes-storage.md)
- [Оптимизированные для GPU](sizes-gpu.md)
- [Для высокопроизводительных вычислений](sizes-hpc.md)
- [Предыдущие поколения](sizes-previous-gen.md)

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.
