---
title: Обзор настраиваемого нейронного голоса — служба речи
titleSuffix: Azure Cognitive Services
description: Настраиваемая нейронная речь — это функция преобразования текста в речь, позволяющая создавать один разновидность искусственного голоса для приложений, предоставляя собственные аудиоданные в качестве примера.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/01/2020
ms.author: trbye
ms.openlocfilehash: 36885e4673b83d1db7972f03c4a6309f766206c5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713105"
---
# <a name="what-is-custom-neural-voice"></a>Что такое настраиваемый нейронный счет?

Настраиваемая нейронная речь — это функция преобразования [текста в речь](./text-to-speech.md) (TTS), которая позволяет создавать односторонние специализированные искусственные голоса для ваших приложений, предоставляя в качестве примера собственные звуковые данные. Преобразование текста в речь осуществляется путем преобразования текста в искусственный речь с помощью модели машинного обучения, которая звучит как выбранный голос. С помощью [REST API](./rest-text-to-speech.md)вы можете разрешить приложениям говорить с [предварительно созданными голосовыми](./language-support.md#neural-voices) или [пользовательскими голосовыми](./how-to-custom-voice-prepare-data.md) моделями, разработанными с помощью пользовательской функции нейронных голосов. Настраиваемая нейронная речь основана на технологии нейронного TTS, которая создает естественный звуковой сигнал, который часто неразличимы при сравнении с человеком.
Реалистичная и естественная звуковая Голосовая передача настраиваемой нейронной речи может представлять торговые марки, персонифи машины и разрешать пользователям взаимодействовать с приложениями естественным образом.

> [!NOTE]
> Для настраиваемой функции нейронной речи требуется регистрация, и доступ к ней ограничивается в зависимости от соответствия требованиям и критериев использования корпорацией Майкрософт. Клиенты, желающие использовать эту функцию, должны регистрировать свои варианты использования с помощью [формы принимать](https://aka.ms/customneural).

## <a name="the-basics-of-custom-neural-voice"></a>Основы пользовательского нейронного голоса

Базовая технология нейронного TTS, используемая для настраиваемой нейронной речи, состоит из трех основных компонентов: анализатора текста, акустической модели нейронов и нейронного вокодер. Чтобы создать естественный искусственный поток из текста, текст сначала помещается в анализатор текста, который предоставляет выходные данные в виде последовательности phoneme. Phoneme — это основная единица звука, которая отличает одно слово от другого на определенном языке. Последовательность фонемы определяет произношение слов, предоставленных в тексте. 

Затем последовательность phoneme переходит в акустическую модель нейронов для прогнозирования акустических функций, определяющих речевые сигналы, такие как Тимбре, стиль речи, скорость, интонатионс и шаблоны нагрузки. И, наконец, нейронный вокодер преобразует акустические функции в звуковые волны, чтобы создать искусственный голос.

![Начальное изображение для пользовательского нейронного голоса.](./media/custom-voice/cnv-intro.png)

Для работы с голосовыми моделями нейронного TTS используются глубокие нейронные сети, основанные на записи образцов человеческого голоса. В этом [блоге](https://techcommunity.microsoft.com/t5/azure-ai/neural-text-to-speech-extends-support-to-15-more-languages-with/ba-p/1505911)мы расскажем, как алгоритм нейронного пробуждения работает с использованием современных моделей синтеза речи нейронных речевых функций. В блоге также объясняется, как можно адаптировать универсальную базовую модель с помощью менее чем 2 часов речевых данных (или менее 2 000 записанных фразы продолжительностью) от целевого докладчика и научиться говориться на голосовых выступающих. Сведения о том, как обучена нейронная вокодер, см. в записи [блога](https://techcommunity.microsoft.com/t5/azure-ai/azure-neural-tts-upgraded-with-hifinet-achieving-higher-audio/ba-p/1847860).

Используя возможности настраиваемой нейронной речи, можно адаптировать механизм нейронного TTS для лучшего соответствия вашим сценариям пользователя. Чтобы создать пользовательский нейронный голос, используйте [голосовую](https://speech.microsoft.com/customvoice) программу для передачи записанных аудио-и соответствующих сценариев, обучения модели и развертывания голоса в пользовательской конечной точке. В зависимости от варианта использования можно использовать пользовательский нейронный голос для преобразования текста в речь в режиме реального времени (например, использование в смарт-виртуальном помощнике) или создания звукового содержимого в автономном режиме (например, в звуковой книге или инструкциях в приложениях электронного обучения) с текстовым вводом, предоставленным пользователем. Это можно сделать с помощью [REST API](./rest-text-to-speech.md), [пакета SDK для распознавания речи](./get-started-text-to-speech.md?pivots=programming-language-csharp&tabs=script%2cwindowsinstall)или [веб-портала](https://speech.microsoft.com/audiocontentcreation).

## <a name="terms-and-definitions"></a>Термины и определения

| **Термин**      | **Определение**                                                                                                                                                                                                                                                                                                                                                                                       |
|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Модель голоса   | Модель преобразования текста в речь, которая может имитировать уникальные характеристики вокал целевого докладчика. *Голосовая модель* также называется *голосовым шрифтом* или *искусственным голосом*. Голосовая модель — это набор параметров в двоичном формате, который не читается человеком и не содержит звуковых записей. Она не может быть реконструирована для получения или создания звука человеческого голоса. |
| Актер озвучивания  | Лица или целевые динамики, голоса которых записываются и используются для создания голосовых моделей, предназначенных для звука, таких как голос.                                                                                                                                                                                                                                                   |
| Стандартный TTS  | Стандартный или традиционный метод TTS, который разделяет речевой язык на фонетические фрагменты, чтобы их можно было переключить и сопоставить с помощью классического программирования или статистических методов.                                                                                                                                                                                                    |
| Нейронный TTS    | Нейронный TTS используется для синтезирования речи с помощью глубоких нейронных сетей с «выясненным» способом объединения фонетических транскрипций в естественном речевом распознавании вместо использования процедурного программирования или статистических методов. В дополнение к записи целевого голоса, алгоритм нейронного TTS использует исходную библиотеку или базовую модель, созданную с голосовыми записями со многих разных динамиков.          |
| Данные для обучения | Пользовательский набор данных для обучения нейронных голоса, который включает звуковые записи о проходе голоса и связанные текстовые сообщения.                                                                                                                                                                                                                                                               |
| Описания       | Персонаж описывает, кто должен быть этим голосовым. Хорошая разработка персонажа сообщит о создании голоса, независимо от того, выбрана ли уже созданная пользовательская модель, или с самого начала путем приведения и записи нового голоса.                                                                                                |
| Скрипт        | Сценарий — это текстовый файл, содержащий фразы продолжительностью, которые должны быть обработаны вашим голосом. (Термин "*фразы продолжительностью*" охватывает полные предложения и более короткие фразы.)                                                                                                                                                                                                                               |

## <a name="responsible-use-of-ai"></a>Ответственное использование ИИ

Сведения о том, как использовать настраиваемый нейронный язык, см. в разделе [Примечание о прозрачности](/legal/cognitive-services/speech-service/custom-neural-voice/transparency-note-custom-neural-voice?context=/azure/cognitive-services/speech-service/context/context). Заметки о прозрачности корпорации Майкрософт призваны помочь вам понять, как работает наша технология искусственного интеллекта, а также о том, что владельцы системы могут повлиять на производительность и поведение системы, а также на важность думать о всей системе, включая технологию, людей и среду.

## <a name="next-steps"></a>Дальнейшие действия

* [Начало работы с набором средств "Пользовательский голос"](how-to-custom-voice.md)
* [Создание и использование пользовательской конечной точки голоса](how-to-custom-voice-create-voice.md)