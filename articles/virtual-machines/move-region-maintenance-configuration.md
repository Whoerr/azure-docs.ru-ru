---
title: Перемещение конфигурации обслуживания в другой регион Azure
description: Узнайте, как переместить конфигурацию обслуживания виртуальной машины в другой регион Azure.
author: shants123
ms.service: virtual-machines
ms.topic: how-to
ms.tgt_pltfrm: vm
ms.date: 03/04/2020
ms.author: shants
ms.openlocfilehash: 91a6adecc9cf0db56fa4c433f388b05aa1bdef6a
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2021
ms.locfileid: "98202918"
---
# <a name="move-a-maintenance-control-configuration-to-another-region"></a>Перемещение конфигурации управления обслуживанием в другой регион

Выполните инструкции из этой статьи, чтобы переместить конфигурацию управления обслуживанием в другой регион Azure. Может потребоваться переместить конфигурацию по ряду причин. Например, чтобы воспользоваться преимуществами нового региона, можно развернуть компоненты или службы, доступные в определенном регионе, в соответствии с требованиями к внутренней политике и управлению, а также в ответ на планирование ресурсов.

[Управление обслуживанием](maintenance-control.md)с настраиваемыми конфигурациями обслуживания позволяет управлять применением обновлений платформы к виртуальным машинам и выделенным узлам Azure. Существует несколько сценариев перемещения управления обслуживанием в разных регионах:

- Чтобы переместить конфигурацию управления обслуживанием, но не ресурсы, связанные с конфигурацией, следуйте инструкциям в этой статье.
- Чтобы переместить ресурсы, связанные с конфигурацией обслуживания, но не самой конфигурацией, выполните следующие [инструкции](move-region-maintenance-configuration-resources.md).
- Чтобы переместить конфигурацию обслуживания и связанные с ней ресурсы, сначала следуйте инструкциям в этой статье. Затем выполните [эти инструкции](move-region-maintenance-configuration-resources.md).

## <a name="prerequisites"></a>Предварительные требования

Перед началом перемещения конфигурации управления обслуживанием выполните следующие действия.

- Конфигурации обслуживания связаны с виртуальными машинами Azure или выделенными узлами Azure. Прежде чем начать, убедитесь, что ресурсы виртуальной машины и узла существуют в новом регионе.
- Identify: 
    - Существующие конфигурации управления обслуживанием.
    - Группы ресурсов, в которых в настоящее время находятся существующие конфигурации. 
    - Группы ресурсов, к которым будут добавляться конфигурации после перемещения в новый регион. 
    - Ресурсы, связанные с конфигурацией обслуживания, которую необходимо переместить.
    - Убедитесь, что ресурсы в новом регионе совпадают с ресурсами, связанными с текущими конфигурациями обслуживания. Конфигурации могут иметь те же имена в новом регионе, что и в старом, но это не обязательно.

## <a name="prepare-and-move"></a>Подготовка и перемещение 

1. Извлеките все конфигурации обслуживания в каждой подписке. Выполните команду CLI [AZ Maintenance Configuration List](/cli/azure/ext/maintenance/maintenance/configuration#ext-maintenance-az-maintenance-configuration-list) , чтобы сделать это, заменив $SUBID на идентификатор подписки.

    ```
    az maintenance configuration list --subscription $subId --query "[*].{Name:name, Location:location, ResGroup:resourceGroup}" --output table
    ```
2. Проверьте список возвращенных таблиц записей конфигурации в подписке. Ниже приведен пример. Список будет содержать значения для конкретной среды.

    **имя**; | **Расположение** | **Группа ресурсов**
    --- | --- | ---
    Пропустить обслуживание | eastus2 | Настройка-группа ресурсов
    игнитедемоконфиг | eastus2 | Настройка-группа ресурсов
    Дефаултмаинтенанцеконфигуратион — eastus | eastus | конфигурация тестирования
    

3. Сохраните список для справки. При перемещении конфигураций можно убедиться, что все перемещено.
4. В качестве ссылки Сопоставьте каждую конфигурацию или группу ресурсов с новой группой ресурсов в новом регионе.
5. Создайте новые конфигурации обслуживания в новом регионе с помощью [PowerShell](../virtual-machines/maintenance-control-powershell.md#create-a-maintenance-configuration)или [CLI](../virtual-machines/maintenance-control-cli.md#create-a-maintenance-configuration).
6. Свяжите конфигурации с ресурсами в новом регионе с помощью [PowerShell](../virtual-machines/maintenance-control-powershell.md#assign-the-configuration)или [CLI](../virtual-machines/maintenance-control-cli.md#assign-the-configuration).


## <a name="verify-the-move"></a>Проверка перемещения

После перемещения конфигураций Сравните конфигурации и ресурсы в новом регионе с подготовленным списком таблиц.


## <a name="clean-up-source-resources"></a>Очистка исходных ресурсов

После перемещения рассмотрите возможность удаления перемещенных конфигураций обслуживания в исходном регионе, [PowerShell](../virtual-machines/maintenance-control-powershell.md#remove-a-maintenance-configuration)или [CLI](../virtual-machines/maintenance-control-cli.md#delete-a-maintenance-configuration).


## <a name="next-steps"></a>Дальнейшие действия

Если необходимо переместить ресурсы, связанные с конфигурациями обслуживания, следуйте [этим инструкциям](move-region-maintenance-configuration-resources.md) . 
