---
title: Навигация по версии IoT Edge и журнал — Azure IoT Edge
description: Узнайте о новых возможностях IoT Edge со сведениями о новых функциях и возможностях в последних выпусках.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/11/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9db51fe9298b7f3329d35df375d027046e1f272e
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100366155"
---
# <a name="azure-iot-edge-versions-and-release-notes"></a>Версии Azure IoT Edge и заметки о выпуске

Azure IoT Edge является продуктом, созданным на основе проекта IoT Edge с открытым исходным кодом, размещенного на сайте GitHub. Все новые выпуски становятся доступны в [проекте Azure IOT Edge](https://github.com/Azure/azure-iotedge). Публикации и отчеты об ошибках можно вносить в [проект IOT Edge с открытым исходным кодом](https://github.com/Azure/iotedge).

## <a name="documented-versions"></a>Документированные версии

Документация по IoT Edge на этом сайте доступна для двух разных версий продукта, чтобы можно было выбрать содержимое, применимое к среде IoT Edge. В настоящее время поддерживаются следующие две версии:

* **IoT Edge 1,1 (LTS)** — это первая версия долгосрочной поддержки (LTS) IOT Edge. Документация по этой версии охватывает все функции и возможности всех предыдущих версий до 1,1. Эта версия документации будет стабильной благодаря поддерживаемому времени существования версии 1,1 и не будет отражать новые функции, выпущенные в более поздних версиях. Выпуск 1,1 — это последняя общедоступная версия IoT Edge.
* **IoT Edge 1,2 (Предварительная версия)** содержит дополнительное содержимое для функций и возможностей, которые находятся в последнем предварительном выпуске, [1,2-RC1](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1)
  * Хотя IoT Edge 1,2 находится на этапе предварительной версии, необходимо установить версии-кандидат. Дополнительные сведения см. в разделе [Установка в автономном режиме или с определенной версией](how-to-install-iot-edge.md?tabs=linux#offline-or-specific-version-installation-optional).

Дополнительные сведения о выпусках IoT Edge см. в разделе [Azure IOT Edge Поддерживаемые системы](support.md).

## <a name="version-history"></a>Журнал версий

В этой таблице содержится журнал последних версий для IoT Edge выпусков пакетов, а также выделены обновления документации для каждой версии.

| Заметки о выпуске и активы | Type | Date | Ключевые моменты |
| ------------------------ | ---- | ---- | ---------- |
| [1.1](https://github.com/Azure/azure-iotedge/releases/tag/1.1.0) | Долгосрочная поддержка (LTS): | Февраль 2021 года | [Долгосрочный план поддержки и поддерживаемые обновления систем](support.md) |
| [1,2 — RC1](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1) | Предварительный просмотр | Ноябрь 2020 г. | [IoT Edge устройств за шлюзами](how-to-connect-downstream-iot-edge-device.md?view=iotedge-2020-11&preserve-view=true)<br>[Брокер IoT Edge MQTT](how-to-publish-subscribe.md?view=iotedge-2020-11&preserve-view=true) |
| [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10) | объем стабилен | Октябрь 2020 г. | [Прямой метод Уплоадсуппортбундле](how-to-retrieve-iot-edge-logs.md#upload-support-bundle-diagnostics)<br>[Передача метрик среды выполнения](how-to-access-built-in-metrics.md)<br>[Приоритет и срок жизни маршрута](module-composition.md#priority-and-time-to-live)<br>[Порядок запуска модуля](module-composition.md#configure-modules)<br>[Подготовка вручную для X. 509](how-to-register-device.md) |
| [1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) | объем стабилен | Март 2020 г. | [Автоматическая подготовка X. 509 с помощью DPS](how-to-auto-provision-x509-certs.md)<br>[Прямой метод Рестартмодуле](how-to-edgeagent-direct-method.md#restart-module)<br>[команда поддержки пакета](troubleshoot.md#gather-debug-information-with-support-bundle-command) |

## <a name="next-steps"></a>Следующие шаги

* [Просмотреть все выпуски Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases)
* [Создание и рассмотрение запросов функций на форуме отзывов](https://feedback.azure.com/forums/907045-azure-iot-edge)