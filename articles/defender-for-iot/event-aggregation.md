---
title: Классическое агрегирование событий модуля безопасности
description: Узнайте о защитнике для агрегирования событий IoT.
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
ms.date: 1/20/2021
ms.author: shhazam
ms.openlocfilehash: 0718c2637658e5519760a68f29c7a816b2aa61a1
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809224"
---
# <a name="security-module-classic-event-aggregation"></a>Классическое агрегирование событий модуля безопасности

Защитник для агентов безопасности IoT собирает данные и системные события с локального устройства и отправляет эти данные в облако Azure для обработки и анализа. Агент безопасности собирает различные типы событий устройств, включая новый процесс и новые события подключения. Как новый процесс, так и новые события подключения могут периодически встречаться на устройстве в течение секунды и, хотя это и важнее для обеспечения надежной и полной безопасности, количество сообщений, которые агенты безопасности могут быстро достигать или превысить квоту центра Интернета вещей и затраты на него. Однако эти события содержат очень ценные сведения о безопасности, которые крайне важны для защиты устройства.

Для уменьшения дополнительной квоты и затрат при сохранении защиты устройств защитник для агентов IoT выполняет статистическую обработку этих типов событий.

Агрегирование событий **включено по** умолчанию, и хотя это и не рекомендуется, можно вручную **Отключить** в любое время.

В настоящее время агрегирование доступно для следующих типов событий:

* процесскреате
* коннектионкреате
* Процесстерминате (только для Windows)

## <a name="how-does-event-aggregation-work"></a>Как работает агрегирование событий?

Если функция агрегирования событий **включена, защитник** для агентов IOT выполняет статистическую обработку событий в течение интервала времени или временного окна.
По истечении интервала времени агент отправляет Агрегированные события в облако Azure для дальнейшего анализа.
Агрегированные события хранятся в памяти до их отправки в облако Azure.

Чтобы уменьшить объем памяти агента, всякий раз, когда агент собирает идентичное событие, которое уже хранится в памяти, агент увеличивает число попаданий для этого конкретного события. При прохождении интервала времени агрегирования агент отправляет число попаданий каждого конкретного типа произошедшего события. Агрегирование событий — это просто агрегирование числа попаданий для каждого собранного типа события.

События считаются идентичными только при соблюдении следующих условий.

* События Процесскреате — при идентичности **командной строки**, **исполняемого файла**, **имени пользователя** и **UserID**
* События Коннектионкреате. когда **commandline**, **UserID**, **Direction**, **локальный адрес**, **Удаленный адрес**, **протокол** и **порт назначения** идентичны.
* События Процесстерминате — когда **исполняемый файл** и **состояние выхода** идентичны

### <a name="working-with-aggregated-events"></a>Работа с агрегированными событиями

Во время агрегирования свойства событий, которые не являются агрегированными, отбрасываются и отображаются в log Analytics со значением 0.

* Процесскреате Events- **ProcessID** и **парентпроцессид** имеют значение 0.
* Коннектионкреате Events- **ProcessID** и **Исходный порт** имеют значение 0

## <a name="event-aggregation-based-alerts"></a>Оповещения на основе агрегирования событий

После анализа защитник для Интернета вещей создает оповещения системы безопасности для подозрительных агрегированных событий. Оповещения, созданные из агрегированных событий, отображаются только один раз для каждого агрегированного события.

Время начала статистической обработки, время окончания и число попаданий для каждого события регистрируются в поле **екстрадетаилс** событий в log Analytics для использования во время расследования.

Каждое Агрегированное событие представляет 24-часовой период собранных оповещений. С помощью меню параметров событий в левом верхнем углу каждого события можно **Закрыть** каждое отдельное Агрегированное событие.

## <a name="event-aggregation-twin-configuration"></a>Настройка двойника агрегирования событий

Внесите изменения в конфигурацию "защитник" для агрегирования событий IoT в [объекте конфигурации агента](how-to-agent-configuration.md) двойника удостоверения модуля **азуреиотсекурити** .

| Имя конфигурации | Возможные значения | Сведения | Remarks |
|:-----------|:---------------|:--------|:--------|
| аггрегатионенабледпроцесскреате | boolean | Включить или отключить агрегирование событий для событий создания процесса |
| аггрегатионинтервалпроцесскреате | Строка TimeSpan ISO8601 | Интервал агрегирования для процесса создает события |
| аггрегатионенабледконнектионкреате | boolean| Включение и отключение агрегирования событий для событий создания соединения |
| аггрегатионинтервалконнектионкреате | Строка TimeSpan ISO8601 | Интервал агрегирования для соединения создает события |
| аггрегатионенабледпроцесстерминате | boolean | Включить или отключить агрегирование событий для событий завершения процесса | Только в Windows|
| аггрегатионинтервалпроцесстерминате | Строка TimeSpan ISO8601 | Интервал агрегирования для событий завершения процесса | Только в Windows|
|

## <a name="default-configurations-settings"></a>Параметры конфигурации по умолчанию

| Имя конфигурации | Значения по умолчанию |
|:-----------|:---------------|
| аггрегатионенабледпроцесскреате | Да |
| аггрегатионинтервалпроцесскреате | PT1H|
| аггрегатионенабледконнектионкреате | Да |
| аггрегатионинтервалконнектионкреате | PT1H|
| аггрегатионенабледпроцесстерминате | Да |
| аггрегатионинтервалпроцесстерминате | PT1H|
|

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы узнали о защитнике для агрегирования агента безопасности IoT и доступных параметрах конфигурации событий.

Чтобы продолжить знакомство с защитником для развертывания IoT, используйте следующие статьи:

- Общие сведения о [методах проверки подлинности агента безопасности](concept-security-agent-authentication-methods.md)
- Выбор и развертывание [агента безопасности](how-to-deploy-agent.md)
- Проверка [предварительных требований](quickstart-system-prerequisites.md) к защитнику для системы IOT
- Узнайте, как [включить защитник для службы IOT в центре Интернета вещей](quickstart-onboard-iot-hub.md)
- Дополнительные сведения о службе из [защитника для Интернета вещей: вопросы и ответы](resources-frequently-asked-questions.md)
