---
title: Поддержка локализации с помощью карт Microsoft Azure
description: Узнайте, какие регионы Azure Maps поддерживаются службами, такими как карты, поиск, маршрутизация, погода и инциденты трафика. Узнайте, как настроить параметр View.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 50e5d0721eb14d1fcdfad26aaf081bfa370e954e
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2020
ms.locfileid: "96904522"
---
# <a name="localization-support-in-azure-maps"></a>Поддержка локализации в Azure Maps

В Azure Maps реализована поддержка разных языков и представлений для разных стран и регионов. В этой статье перечисляются поддерживаемые языки и представления, используемые при работе с Azure Maps.


## <a name="azure-maps-supported-languages"></a>Поддерживаемые языки в службе "Карты Azure"

Службы Azure Maps локализованы на разные языки. В таблице ниже содержатся записи коды языков для каждой службы.  
  

| ID         | Имя                   |  Maps | Поиск | Маршрутизация | Weather | Инциденты трафика | Элементы управления картой JS |
|------------|------------------------|:-----:|:------:|:-------:|:--------:|:-----------------:|:--------------:|
| af-ZA      | Африкаанс              |       |    ✓   |    ✓    |         |                   |                |
| ar-SA      | Арабский                 |   ✓   |    ✓   |    ✓    |    ✓      |         ✓         |        ✓       |
| bn-BD      | Бенгальский (Бангладеш)    |       |       |         |     ✓    |                   |                |
| bn-IN      | Бенгальский (Индия)         |       |       |         |     ✓    |                   |                |
| bs-BA      | Боснийский                 |       |       |         |     ✓    |                   |                |
| eu-ES      | Баскский                 |       |    ✓   |         |         |                   |                |
| bg-BG      | Болгарский              |   ✓   |    ✓   |    ✓    |     ✓     |                   |        ✓       |
| ca-ES      | Каталонский                |       |    ✓   |         |    ✓      |                   |                |
| zh-HanS    | Китайский (упрощенное письмо)   |       |  zh-CN |         |     zh-CN   |                   |                |
| zh-HanT    | китайский (Гонконг, CАР)  |  |   |    |    zh-HK   |                   |           |
| zh-HanT    | Китайский (Тайвань)  | zh-TW |  zh-TW |  zh-TW  |    zh-TW   |                   |      zh-TW     |
| hr-HR      | Хорватский               |       |    ✓   |         |    ✓      |                   |                |
| cs-CZ      | Чешский                  |   ✓   |    ✓   |    ✓    |    ✓      |         ✓         |        ✓       |
| da-DK      | Датский                 |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| nl-BE      | Нидерландский (Бельгия)        |       |    ✓   |         |      ✓    |                   |                |
| nl-NL      | Нидерландский (Нидерланды)    |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| en-AU      | Английский (Австралия)    |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| en-NZ      | Английский (Новая Зеландия)  |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| en-GB      | Английский (Великобритания) |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| ru-RU      | Английский (США)          |   ✓   |    ✓   |    ✓    |      ✓    |         ✓         |        ✓       |
| et-EE      | Эстонский               |       |    ✓   |         |      ✓    |         ✓         |                |
| fil-PH     | Филиппинский               |       |       |         |     ✓    |                   |                |
| fi-FI      | Финский                |   ✓   |    ✓   |    ✓    |      ✓    |         ✓         |        ✓       |
| fr-FR      | Французский                 |   ✓   |    ✓   |    ✓    |      ✓    |         ✓         |        ✓       |
| fr-CA      | Французский (Канада)      |       |    ✓   |         |     ✓     |                   |                |
| gl-ES      | Галисийский               |       |    ✓   |         |         |                   |                |
| de-DE      | Немецкий                 |   ✓   |    ✓   |    ✓    |   ✓      |         ✓         |        ✓       |
| el-GR      | Греческий                  |   ✓   |    ✓   |    ✓    |    ✓     |         ✓         |        ✓       |
| gu-IN      | Гуджарати                |       |       |         |     ✓    |                   |                |
| he-IL      | Иврит                 |       |    ✓   |         |     ✓    |         ✓         |                |
| hi-IN      | Hindi                  |       |        |         |     ✓    |                   |                |
| hu-HU      | Венгерский              |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| is-IS      | Исландский              |       |       |         |     ✓    |                   |                |
| id-ID      | Индонезийский             |   ✓   |    ✓    |    ✓    |     ✓    |         ✓         |        ✓       |
| it-IT      | Итальянский                |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| ja-JP      | Японский               |       |        |         |     ✓    |                   |                |
| kn-IN      | Каннада                |       |       |         |     ✓    |                   |                |
| kk-KZ      | Казахский                 |       |    ✓   |         |     ✓    |                   |                |
| ko-KR      | Корейский                 |   ✓   |        |    ✓    |     ✓    |                   |        ✓       |
| es-419     | Испанский (Латинская Америка) |       |    ✓   |         |         |                   |                |
| lv-LV      | Латышский                |       |    ✓   |         |     ✓    |         ✓         |                |
| lt-LT      | Литовский             |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| mk-MK      | Macedonian             |       |       |         |     ✓    |                   |                |
| ms-MY      | Малайский (латиница)          |   ✓   |    ✓   |    ✓    |    ✓   |                   |        ✓       |
| mr-IN      | Маратхи                 |       |       |         |     ✓    |                   |                |
| nb-NO      | Норвежский (букмол)       |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| NGT        | Neutral Ground Truth — официальные языки для всех регионов в локальных сценариях, если он доступен. |   ✓     |        |         |       |        |      ✓          |
| NGT-Latn   | Neutral Ground Truth — экзонимы на латинском языке. Будет использоваться сценарий на латинском алфавите, если он доступен. |   ✓     |        |         |         |                |        ✓         |
| pl-PL      | Польский                 |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| pt-BR      | Португальский (Бразилия)    |   ✓   |    ✓   |    ✓    |      ✓   |                   |        ✓       |
| pt-PT      | Португальский (Португалия)  |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| pa-IN      | Панджаби                 |       |       |         |     ✓    |                   |                |
| ro-RO      | Румынский               |       |    ✓    |         |     ✓    |         ✓         |                |
| ru-RU      | Русский                |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| sr-Cyrl-RS | Сербский (кириллица)     |       |   sr-RS  |         |    sr-RS     |                   |                |
| sr-Latn-RS | Сербский (латиница)        |       |       |         |     sr-latn    |                   |                |
| sk-SK      | Словацкий             |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| sl-SL      | Словенский              |   ✓   |    ✓   |    ✓    |     ✓    |                   |        ✓       |
| es-ES      | Испанский                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| es-MX      | Испанский (Мексика)       |   ✓   |        |    ✓    |     ✓    |                   |        ✓       |
| sv-SE      | Шведский                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| ta-IN      | Тамильский (Индия)                 |       |       |         |     ✓    |                   |                |
| te-IN      | Телугу (Индия)                 |       |       |         |     ✓    |                   |                |
| th-TH      | Тайский                   |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| tr-TR      | Турецкий                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| uk-UA      | Украинский               |       |    ✓   |         |     ✓    |                   |                |
| ur-PK      | Урду                 |       |       |         |     ✓    |                   |                |
| uz-Latn-UZ | Узбекский                 |       |       |         |     ✓    |                   |                |
| vi-VN      | Вьетнамский             |       |    ✓   |         |      ✓    |                  |                |


## <a name="azure-maps-supported-views"></a>Представления, поддерживаемые в Azure Maps

> [!Note]
> 1 августа 2019 года службы Azure Maps выпущены в следующих странах и регионах:
>  * Аргентина
>  * Индия
>  * Марокко
>  * Пакистан
>
> С 1 августа 2019 года параметр **View** (Представление) будет определять полученное содержимое карты для перечисленных выше новых регионов и стран. В Azure Maps параметр **View** (Представление), который также называется параметром региона пользователя, представляет собой двухбуквенный код страны по стандарту ISO-3166. Он позволяет отображать в службах Azure Maps правильные карты для указанной страны или региона с учетом геополитически спорного содержимого, включая границы и метки на карте. 

Обязательно настройте параметр **View** (Представление) нужным образом в REST API и пакетах SDK, которые используются вашими службами.
  

### <a name="rest-apis"></a>Интерфейсы API-интерфейсов RESTful
  
Убедитесь, что для параметра View (Представление) задано требуемое значение. Параметр View (Представление) указывает, какой набор геополитически спорного содержимого отображают службы Azure Maps. 

Этот параметр влияет на следующие службы REST в Azure Maps:
    
 * Get Map Tile
 * Get Map Image 
 * Get Search Fuzzy
 * Get Search POI
 * Get Search POI Category
 * Get Search Nearby
 * Get Search Address
 * Get Search Address Structured
 * Get Search Address Reverse
 * Get Search Address Reverse Cross Street
 * Post Search Inside Geometry
 * Пакет разноски адреса поиска
 * Обратный пакет для обратного поиска адреса
 * Post Search Along Route
 * Нечеткий пакет после поиска

 
### <a name="sdks"></a>Пакеты SDK

Обязательно укажите нужное значение для параметра **View** (Представление) и всегда используйте последнюю версию веб-пакета SDK и пакета SDK для Android. Этот параметр влияет на следующие пакеты SDK:

 * Веб-пакет SDK Azure Maps
 * Пакет SDK для Android в Azure Maps

По умолчанию параметр View (Представление) имеет значение **Unified** (Унифицированное), даже если это не определено в запросе. Определите расположение пользователя. На его основе задайте правильное значение параметра **View** (Представление). Также вы можете присвоить параметру View (Представление) значение Auto (Автоматически), чтобы возвращать данные карт с учетом IP-адреса запроса.  Параметр **View** (Представление) в Azure Maps должен использоваться в соответствии с применимым законодательством, включая любые законы, регламентирующие использование карт для страны или региона, где предоставляются карты, изображения и другое содержимое сторонних поставщиков, к которым вы можете получить доступ через службы Azure Maps.


В таблице ниже перечисленные поддерживаемые представления.

| Представление         | Описание                            |  Maps | Поиск | Элементы управления картой JS |
|--------------|----------------------------------------|:-----:|:------:|:--------------:|
| AE           | Объединенные Арабские Эмираты (арабское представление)    |   ✓   |        |     ✓          |
| AR           | Аргентина (аргентинское представление)           |   ✓   |    ✓   |     ✓          |
| BH           | Бахрейн (арабское представление)                 |   ✓   |        |     ✓          |
| IN           | Индия (индийское представление)                    |   ✓   |   ✓     |     ✓          |
| IQ           | Ирак (арабское представление)                    |   ✓   |        |     ✓          |
| JO           | Иордан (арабское представление)                  |   ✓   |        |     ✓          |
| KW           | Кувейт (арабское представление)                  |   ✓   |        |     ✓          |
| LB           | Ливан (арабское представление)                 |   ✓   |        |     ✓          |
| MA           | Марокко (марокканское представление)                |   ✓   |   ✓     |     ✓          |
| OM           | Оман (арабское представление)                    |   ✓   |        |     ✓          |
| PK           | Пакистан (пакистанское представление)              |   ✓   |    ✓    |     ✓          |
| PS           | Палестинская Автономия (арабское представление)    |   ✓   |        |     ✓          |
| QA           | Катар (арабское представление)                   |   ✓   |        |     ✓          |
| SA           | Саудовская Аравия (арабское представление)            |   ✓   |        |     ✓          |
| SY           | Сирия (арабское представление)                   |   ✓   |        |     ✓          |
| YE           | Йемен (арабское представление)                   |   ✓   |        |     ✓          |
| Auto         | Данные карт возвращаются с учетом IP-адреса запроса.|   ✓   |    ✓   |     ✓          |
| Unified      | Унифицированное представление (прочие)                  |   ✓   |   ✓     |     ✓          |
