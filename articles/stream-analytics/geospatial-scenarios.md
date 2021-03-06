---
title: Геоограждение и Геопространственное агрегирование с Azure Stream Analytics
description: В этой статье описывается, как использовать Azure Stream Analytics для геопространственной и геопространственных агрегатов.
author: krishna0815
ms.author: krishmam
ms.service: stream-analytics
ms.topic: how-to
ms.date: 04/02/2019
ms.openlocfilehash: 7f83fe3a8443ac1b9339a0ddb91ee95414eef437
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98016309"
---
# <a name="geofencing-and-geospatial-aggregation-scenarios-with-azure-stream-analytics"></a>Сценарии геозоны и геопространственной статистической обработки с Azure Stream Analytics

С помощью встроенных геопространственных функций можно использовать Azure Stream Analytics для создания приложений для таких сценариев, как управление препарком, совместное использование, подключенные автомобили и отслеживание ресурсов.

## <a name="geofencing"></a>Геозоны

Azure Stream Analytics поддерживает геозоны с низкой задержкой в реальном времени в облаке и в среде выполнения IoT Edge.

### <a name="geofencing-scenario"></a>Сценарий георазграничения

Производственная компания должна проконтролировать активы в своих зданиях. Они оснащены каждым устройством с GPS и хотят получать уведомления, если устройство покидает определенную область.

Ссылочные данные, используемые в этом примере, имеют сведения геозоны для зданий и устройств, разрешенных в каждом из зданий. Помните, что ссылочные данные могут быть как статическими, так и неизменными. Для этого сценария используются статические эталонные данные. Поток данных постоянно создает идентификатор устройства и его текущую точку.

### <a name="define-geofences-in-reference-data"></a>Определение геозаборов в ссылочных данных

Геозоны можно определить с помощью объекта геообъектно-JSON. Для заданий с совместимостью версии 1,2 и выше геозоны также могут быть определены с помощью хорошо известного текста (WKT) как `NVARCHAR(MAX)` . WKT — это стандартный Открытый геопространственный консорциум (OGC), который используется для представления пространственных данных в текстовом формате.

Встроенные геопространственные функции могут использовать определенные геограницы для определения того, входит ли элемент в определенный многоугольник геозоны или из него.

В следующей таблице приведен пример ссылочных данных геозоны, которые можно хранить в хранилище BLOB-объектов Azure или в таблице SQL Azure. Каждый сайт представлен геопространственной многоугольником, а каждое устройство связано с разрешенным ИДЕНТИФИКАТОРом сайта.

|Идентификатор|SiteName|Геозоны|алловеддевицеид|
|------|--------|--------|---------------|
|1|"Redmond, здание 41"|"POLYGON (-122.1337357922017 47.63782998329432,-122.13373042778369 47.637634793257305,-122.13346757130023 47.637642022530954,-122.13348902897235 47.637508280806806,-122.13361777500506 47.637508280806806,-122.13361241058703 47.63732393354484,-122.13265754417773 47.63730947490855,-122.13266290859576 47.637519124743164,-122.13302232460376 47.637515510097955,-122.13301696018573 47.63764925180358,-122.13272728161212 47.63764925180358,-122.13274873928424 47.63784082716388,-122.13373579220172 47.63782998329432))"|"B"|
|2|"Redmond, здание 40"|"POLYGON (-122.1336154507967 47.6366745947009,-122.13361008637867 47.636483015064535,-122.13349206918201 47.636479400347675,-122.13349743360004 47.63636372927573,-122.13372810357532 47.63636372927573,-122.13373346799335 47.63617576323771,-122.13263912671528 47.63616491902258,-122.13264985555134 47.63635649982525,-122.13304682248554 47.636367344000604,-122.13305218690357 47.63650831807564,-122.13276250832996 47.636497473929516,-122.13277323716602 47.63668543881025,-122.1336154507967 47.6366745947009))"|"A"|
|3|"Redmond, здание 22"|"POLYGON (-122.13611660248233 47.63758544698554,-122.13635263687564 47.6374083293018,-122.13622389084293 47.63733603619712,-122.13622389084293 47.63717699101473,-122.13581619507266 47.63692757827657,-122.13559625393344 47.637046862778135,-122.13569281345798 47.637144458985965,-122.13570890671207 47.637314348246214,-122.13611660248233 47.63758544698554))"|Ц|

### <a name="generate-alerts-with-geofence"></a>Создание оповещений с помощью геозоны

Устройства могут выдавать свои ИДЕНТИФИКАТОРы и расположения каждую минуту через поток с именем `DeviceStreamInput` . Следующая таблица представляет собой поток входных данных.

|DeviceID|Геоположение|
|--------|-----------|
|"A"|"POINT (-122.13292341559497 47.636318374032726)"|
|"B"|"POINT (-122.13338475554553 47.63743531308874)"|
|Ц|"POINT (-122.13354001095752 47.63627622505007)"|

Можно написать запрос, который присоединяет поток устройства к ссылочным данным геозоны и создает предупреждение каждый раз, когда устройство находится за пределами разрешенного здания.

```SQL
SELECT DeviceStreamInput.DeviceID, SiteReferenceInput.SiteID, SiteReferenceInput.SiteName 
INTO Output
FROM DeviceStreamInput 
JOIN SiteReferenceInput
ON st_within(DeviceStreamInput.GeoPosition, SiteReferenceInput.Geofence) = 0
WHERE DeviceStreamInput.DeviceID = SiteReferenceInput.AllowedDeviceID
```

На следующем рисунке представлены геограницы. Можно увидеть, где находятся устройства в соответствии с входными данными потока.

![Создание геозаборов](./media/geospatial-scenarios/building-geofences.png)

Устройство "C" находится в ИДЕНТИФИКАТОРе сборки 2, что запрещено в соответствии с эталонными данными. Это устройство должно находиться в здании с ИДЕНТИФИКАТОРом 3. При выполнении этого задания будет создано предупреждение для этого конкретного нарушения.

### <a name="site-with-multiple-allowed-devices"></a>Сайт с несколькими разрешенными устройствами

Если сайт допускает использование нескольких устройств, можно определить массив идентификаторов устройств, `AllowedDeviceID` а в предложении можно использовать функцию User-Defined, `WHERE` чтобы проверить, соответствует ли идентификатор устройства потока идентификатору устройства в этом списке. Дополнительные сведения см. в руководстве по [JavaScript UDF](stream-analytics-javascript-user-defined-functions.md) для облачных заданий и в учебнике по [языку UDF C#](stream-analytics-edge-csharp-udf.md) для заданий пограничных задач.

## <a name="geospatial-aggregation"></a>Геопространственное агрегирование

Azure Stream Analytics поддерживает Геопространственное геопространственные вычисления в реальном времени в облаке и в среде выполнения IoT Edge.

### <a name="geospatial-aggregation-scenario"></a>Сценарий геопространственной статистической обработки

Компании-CAB требуется создать приложение в режиме реального времени, чтобы попытаться воспользоваться своими драйверами CAB-файлов для работы с областями городов, на которых в настоящее время наблюдается более высокая потребность.

Компания хранит логические регионы города как ссылочные данные. Каждый регион определяется RegionID, RegionName и геозоны.

### <a name="define-the-geofences"></a>Определение геозаборов

В следующей таблице приведен пример ссылочных данных геозоны, которые можно хранить в хранилище BLOB-объектов Azure или в таблице SQL Azure. Каждый регион представлен геопространственной многоугольником, который используется для сопоставления с запросами, поступающими из потоковых данных.

Эти многоугольники предназначены только для справки и не представляют фактическое логическое или физическое разделение города.

|RegionID|RegionName|Геозоны|
|--------|----------|--------|
|1|Локаль|"POLYGON (-74.00279525078275 40.72833625216264,-74.00547745979765 40.721929158663244,-74.00125029839018 40.71893680218994,-73.9957785919998 40.72521409075776,-73.9972377137039 40.72557184584898,-74.00279525078275 40.72833625216264))"|
|2|"Чинатовн"|"МНОГОУГОЛЬНИК ((-73.99712367114876 40.71281582267133,-73.9901070123658 40.71336881907936,-73.99023575839851 40.71452359088633,-73.98976368961189 40.71554823078944,-73.99551434573982 40.717337246783735,-73.99480624255989 40.718491949759304,-73.99652285632942 40.719109951574,-73.99776740131233 40.7168005470334,-73.99903340396736 40.71727219249899,-74.00193018970344 40.71938642421256,-74.00409741458748 40.71688186545551,-74.00051398334358 40.71517415773184,-74.0004281526551 40.714377212470005,-73.99849696216438 40.713450141693166))"|
|3|"Трибека"|"POLYGON (-74.01091641815208 40.72583120006787,-74.01338405044578 40.71436586362705,-74.01370591552757 40.713617702123415,-74.00862044723533 40.711308107057235,-74.00194711120628 40.7194238654018,-74.01091641815208 40.72583120006787))"|

### <a name="aggregate-data-over-a-window-of-time"></a>Агрегирование данных за период времени

В следующей таблице содержатся потоковые данные "переданных".

|UserID|фромлокатион|ToLocation|трипрекуестедтиме|
|------|------------|----------|-----------------|
|"A"|"POINT (-74.00726861389182 40.71610611981975)"|"POINT (-73.98615095917779 40.703107386025835)"|"2019-03-12T07:00:00Z"|
|"B"|"POINT (-74.00249841021645 40.723827238895666)"|"POINT (-74.01160699942085 40.71378884930115)"|"2019-03-12T07:01:00Z"|
|Ц|"POINT (-73.99680120565864 40.716439898624024)"|"POINT (-73.98289663412544 40.72582343969828)"|"2019-03-12T07:02:00Z"|
|"D"|"POINT (-74.00741090068288 40.71615626755086)"|"POINT (-73.97999843120539 40.73477895807408)"|"2019-03-12T07:03:00Z"|

Следующий запрос соединяет поток устройства с эталонными данными геозоны и вычисляет количество запросов на регион в течение 15 минут каждую минуту.

```SQL
SELECT count(*) as NumberOfRequests, RegionsRefDataInput.RegionName 
FROM UserRequestStreamDataInput
JOIN RegionsRefDataInput 
ON st_within(UserRequestStreamDataInput.FromLocation, RegionsRefDataInput.Geofence) = 1
GROUP BY RegionsRefDataInput.RegionName, hoppingwindow(minute, 1, 15)
```

Этот запрос выводит число запросов, каждую минуту за последние 15 минут по каждому региону в городе. Эти сведения можно легко отобразить Power BI панели мониторинга или транслировать во все драйверы как текстовые сообщения SMS путем интеграции со службами, такими как функции Azure.

На рисунке ниже показаны выходные данные запроса для Power BI панели мониторинга. 

![Вывод результатов на панели мониторинга Power BI](./media/geospatial-scenarios/power-bi-output.png)


## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о геопространственных функциях Azure Stream Analytics](stream-analytics-geospatial-functions.md)
* [Геопространственные функции (Azure Stream Analytics)](/stream-analytics-query/geospatial-functions)