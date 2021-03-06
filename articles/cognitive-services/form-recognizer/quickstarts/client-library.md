---
title: Краткое руководство. Клиентская библиотеке или REST API Распознавателя документов
titleSuffix: Azure Cognitive Services
description: Узнайте, как с помощью клиентской библиотеки или REST API Распознавателя документов создать приложение для обработки форм, которое извлекает из пользовательских документов пары "ключ-значение" и табличные данные.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 01/29/2021
ms.author: pafarley
zone_pivot_groups: programming-languages-set-formre
ms.custom: devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
keywords: Обработка форм, автоматизированная обработка данных
ms.openlocfilehash: d468f40d9de7fd7efb4ac7d8021a667e0c6c31f6
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "99584614"
---
# <a name="quickstart-use-the-form-recognizer-client-library-or-rest-api"></a>Краткое руководство. Использование клиентской библиотеки или REST API Распознавателя документов

Начните работу с Распознавателем документов на используемом вами языке. Распознаватель документов Azure — это служба Cognitive Service, которая позволяет создавать автоматизированное программное обеспечение для обработки данных с помощью технологии машинного обучения. Эта служба идентифицирует и извлекает пары "ключ-значение", отметки выбора, табличные данные и другие сведения из документов с формами &mdash; она выводит структурированные данные, включая связи в исходном файле. С Распознавателем документов можно работать, используя REST API или пакет SDK. Выполните приведенные здесь действия, чтобы установить пакет SDK и протестировать пример кода для выполнения базовых задач. 

Распознаватель документов позволяет выполнять следующие задачи:

* [Анализ макета](#analyze-layout)
* [анализ счетов](#analyze-invoices);
* [обучение пользовательской модели](#train-a-custom-model);
* [анализ документов с помощью пользовательской модели](#analyze-forms-with-a-custom-model);
* [анализ квитанций](#analyze-receipts);
* [анализ визитных карточек](#analyze-business-cards);
* [управление пользовательскими моделями.](#manage-your-custom-models)

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](../includes/quickstarts/javascript-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-rest-api"

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end
