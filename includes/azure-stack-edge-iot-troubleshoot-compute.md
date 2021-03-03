---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 02/05/2021
ms.openlocfilehash: ad981264a99bd48e27f745a789ebe857b7f17d80
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750661"
---
Используйте ответы времени выполнения агента IoT Edge, чтобы устранить ошибки, связанные с вычислением. Ниже перечислены возможные ответы:

* 200 – OK
* 400 — конфигурация развертывания имеет неправильный формат или недопустима.
* 417-у устройства нет набора конфигураций развертывания.
* 412 — версия схемы в конфигурации развертывания недопустима.
* 406 — устройство IoT Edge работает в автономном режиме или не отправляет отчеты о состоянии.
* 500 — в среде выполнения IoT Edge произошла ошибка.

Дополнительные сведения см. в разделе [агент IOT Edge](../articles/iot-edge/iot-edge-runtime.md?preserve-view=true&view=iotedge-2018-06#iot-edge-agent).

Следующая ошибка связана со службой IoT Edge на Azure Stack пограничных Pro.<!--/ Data Box Gateway--> возможностями, доступными на любом устройстве.

### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>Модули вычислений имеют неизвестное состояние и не могут использоваться

#### <a name="error-description"></a>Описание ошибки

Все модули на устройстве отображают неизвестное состояние и не могут использоваться. Неизвестное состояние сохраняется при перезагрузке.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Предлагаемое решение

Удалите IoT Edgeную службу, а затем повторно разверните модули. Дополнительные сведения см. в разделе [Remove IOT Edge Service](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#remove-iot-edge-service).