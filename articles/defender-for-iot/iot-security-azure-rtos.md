---
title: Обзор модуля безопасности для Azure RTO
description: Узнайте больше о модуле безопасности для поддержки и реализации Azure RTO в рамках защитника Azure для IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/14/2021
ms.author: shhazam
ms.openlocfilehash: 47b2b49a21a82be13c987b95c7c775f5014a532e
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809088"
---
# <a name="overview-defender-for-iot-security-module-for-azure-rtos-preview"></a>Обзор: модуль безопасности "защитник для Интернета вещей" для Azure RTO (Предварительная версия)

Модуль "защитник Azure для IoT Micro" предоставляет комплексное решение для обеспечения безопасности устройств, использующих Azure RTO. Она предоставляет сведения об общих угрозах и потенциально вредоносных действиях на устройствах операционной системы, работающих в режиме реального времени (RTO). Azure RTO теперь поставляется с встроенным модулем безопасности Azure IoT.

:::image type="content" source="./media/architecture/azure-rtos-security-monitoring.png" alt-text="Визуализация защитника Azure RTO для IoT.":::


Micro Module для Azure RTO предоставляет следующие возможности:

- Обнаружение вредоносных сетевых операций
- Пользовательское поведение устройства на основе оповещений задания базовых показателей
- Улучшенная безопасность устройств санации

## <a name="detect-malicious-network-activities"></a>Обнаружение вредоносных сетевых действий

Отслеживается входящая и исходящая сетевая активность каждого устройства. Поддерживаются протоколы TCP, UDP и ICMP для IPv4 и IPv6. Защитник для IoT проверяет каждое из этих сетевых операций на веб-канале анализа угроз Майкрософт. Веб-канал обновляется в режиме реального времени с миллионами уникальных индикаторов угроз, собранных по всему миру.

## <a name="device-behavior-baselining-based-on-custom-alerts"></a>Поведение устройства задания базовых показателей на основе пользовательских оповещений

Задания базовых показателей позволяет выполнять кластеризацию устройств в группах безопасности и определять ожидаемое поведение каждой группы. Так как устройства IoT обычно предназначены для работы в четко определенных и ограниченных сценариях, можно легко создать базовый уровень, определяющий ожидаемое поведение, с помощью набора параметров. Любое отклонение от базовых показателей активирует оповещение.

## <a name="improve-your-device-security-hygiene"></a>Улучшение безопасности устройств санации

С помощью рекомендуемого защитника инфраструктуры для Интернета вещей вы можете получить знания о проблемах в вашей среде, которые влияют на безопасность ваших устройств и могут повредить их. Слабая система безопасности IoT-Device Security может обеспечить успешную атаку, если она останется без изменений. Безопасность всегда измеряется самой слабой связью в любой организации.

## <a name="get-started-protecting-azure-rtos-devices"></a>Приступая к защите устройств Azure RTO

Модуль безопасности для Azure RTO предоставляется бесплатно для загрузки устройств. Служба "защитник для облачных служб Интернета вещей" доступна с 30-дневной пробной версией для каждой подписки Azure. Чтобы приступить к работе, скачайте [модуль безопасности для Azure RTO](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/defender-for-iot/iot-security-azure-rtos.md). 

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы узнали о модуле безопасности для Azure RTO. Дополнительные сведения о модуле безопасности и начале работы см. в следующих статьях:

- [Основные понятия модуля безопасности Azure RTO IoT](concept-rtos-security-module.md)
- [Краткое руководство. модуль безопасности Azure RTO IoT](quickstart-azure-rtos-security-module.md)
