---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2020
ms.author: trbye
ms.openlocfilehash: dc569050b78a5797808f2e2e000019ba516ba22e
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/23/2020
ms.locfileid: "97739248"
---
**Интерактивный**
- Предназначен для сценариев команд и управления.
- Имеет значение времени ожидания сегментации X.
- В конце одного распознанного utterance служба останавливает обработку звука из идентификатора запроса и завершает свою работу. Соединение не закрыто.
- Максимальный предел для распознавания — 20s.
- Типичным вызовом метода углекислого вызова является `RecognizeOnceAsync` .

**Разговора**
- Предназначен для более длительного распознавания.
- Имеет значение времени ожидания сегментации Y. (Y! = X)
- Будет обрабатывать несколько полных фразы продолжительностью, не завершая свою работу.
- Завершится слишком мало тишины.
- При необходимости она продолжит использовать новый идентификатор запроса и воспроизводит звук.
- Служба будет принудительно отключена через 10 минут при распознавании речи.
- Воспроизведет повторное подключение и воспроизведение неподтвержденного звука.
- Вызывается в углекислом с `StartContinuousRecognition` .

**Диктофона**
- Позволяет пользователям указывать знаки препинания, нажимая их.
- Вызывается в объекте углекислого, указывая `EnableDictation` для `SpeechConfig` объекта независимо от вызова API, который начинает распознавание.
- <sup>Один кластер</sup> с `speech.fragment` несредним уровнем выводит сообщения для промежуточных результатов, три сообщения, возвращаемые стороной<sup>удаленных рабочих столов</sup> `speech.hypothesis` .