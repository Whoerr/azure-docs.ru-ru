---
title: Структуры данных служб Mobility Services (Предварительная версия) в картах Microsoft Azure
description: Сведения о том, как данные организованы в области Metro в Azure Maps служб Mobility Services (Предварительная версия). Узнайте, какие поля хранят сведения о прекращении общей транзитной передачи и строках.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 8ffedc18ba331733723a6293756b60b733cc32cf
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2020
ms.locfileid: "96904726"
---
# <a name="data-structures-in-azure-maps-mobility-services-preview"></a>Структуры данных в службах Azure Maps Mobility Services (Предварительная версия) 

> [!IMPORTANT]
> Службы Mobility Azure Maps в настоящее время доступны в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



В этой статье описывается понятие области Metro в [Azure Maps служб Mobility Services](/rest/api/maps/mobility). Мы обсудим некоторые из общих полей, возвращаемых при запросе этой службы о прекращении передачи и строках. Мы рекомендуем ознакомиться с этой статьей перед началом разработки с помощью API-интерфейсов служб Mobility Services.

## <a name="metro-area"></a>Область Metro

Данные служб Mobility Services (Предварительная версия) группируются по поддерживаемым областям метро. Области Metro не следуют границам города. В области Metro может содержаться несколько городов, заполненный город и окружающие города. На самом деле, страна или регион могут быть одной областью метро. 

`metroID`— Это идентификатор области метро, который можно использовать для вызова [API получения сведений о области Metro](/rest/api/maps/mobility/getmetroareainfopreview). Используйте Azure Maps "" Get Metro "API для запроса типов транзитного склада, транзитных агентств, активных оповещений и дополнительных сведений о выбранном Metro. Вы также можете запросить поддерживаемые области Metro и Метроидс. Идентификаторы областей Metro могут изменяться.

**Метроид:** 522   **Имя:** Сиэтл-Tacoma-Бельвью

![Сиэтл-Metro-Area](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Идентификаторы завершения

На транзитные остановки могут ссылаться два типа идентификаторов: идентификатор [гфтс](http://gtfs.org/) и идентификатор остановки Azure Maps. Идентификатор ГФТС называется Стопкэй, а Azure Maps ИДЕНТИФИКАТОРом останавливает называется Стопид. При частом обращении к транзиту рекомендуется использовать идентификатор остановки Azure Maps. Стопид является более стабильным и, скорее всего, останется тем же, пока существует физическая точка. Идентификатор ГТФС останавливает обновляться чаще. Например, можно обновить идентификатор ГТФС в запросе поставщика ГТФС или при освобождении новой версии ГТФС. Несмотря на то, что физическая точка прекращения не имела изменений, идентификатор завершения ГТФС может измениться.

Чтобы начать, можно запросить находящиеся в ближайшем транзите пути, используя [функцию получения ближайших API транзитного пути](/rest/api/maps/mobility/getnearbytransitpreview).

## <a name="line-groups-and-lines"></a>Группы линий и линии

Службы Mobility Service (Предварительная версия) используют параллельную модель данных для линий и групп строк. Эта модель используется для повышения эффективности изменений, наследуемых от [гтфс](http://gtfs.org/) маршрутов и данных о поездках.


### <a name="line-groups"></a>Группы строк

Группа строк — это сущность, которая объединяет все строки, логически входящие в одну и ту же группу. Как правило, группа строк содержит две строки: одну от точки A к B, а другая — от точки B до A. Обе строки будут принадлежать одному открытому агентству транспорта и имеют одинаковый номер строки. Однако могут возникнуть ситуации, в которых группа строк содержит более двух строк или только одну строку внутри нее.


### <a name="lines"></a>Линии

Как упоминалось выше, каждая группа строк состоит из набора строк. Каждая группа строк состоит из двух строк, и каждая строка описывает направление.  Однако бывают случаи, в которых дополнительные строки образуют группу строк. Например, есть строка, которая иногда рассматривает определенное окружение, а иногда нет. В обоих случаях он работает под тем же номером строки. Кроме того, группа строк может состоять из одной строки. Круглая линия с одним направлением — это группа «водо» с одной линией.

Для начала можно запросить группы строк с помощью [API получения транзитной строки](/rest/api/maps/mobility/gettransitlineinfopreview).


## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как запросить транзитные данные с помощью служб Mobility Services (Предварительная версия):

> [!div class="nextstepaction"]
> [Запрос передачи данных](how-to-request-transit-data.md)

Узнайте, как запрашивать данные в режиме реального времени с помощью служб Mobility Services (Предварительная версия):

> [!div class="nextstepaction"]
> [Как запросить данные в режиме реального времени](how-to-request-real-time-data.md)

Ознакомьтесь с документацией по API Azure Maps Mobility Services (Предварительная версия)

> [!div class="nextstepaction"]
> [Документация по API служб Mobility Services](/rest/api/maps/mobility)