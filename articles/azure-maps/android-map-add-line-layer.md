---
title: Добавление слоя линии в карты Android | Карты Microsoft Azure
description: Узнайте, как добавлять линии в карты. См. примеры использования пакет SDK для Android Azure Maps для добавления слоев линий к картам и для настройки линий с символами и градиентами цвета.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 3e68be79a4405af103512a9009187857a0d9af39
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681744"
---
# <a name="add-a-line-layer-to-the-map-android-sdk"></a>Добавление слоя линии на карту (пакет SDK для Android)

Слой линий можно использовать для отображения компонентов `LineString` и `MultiLineString` в виде путей или маршрутов на карте. Слой линий также можно использовать для отображения контура компонентов `Polygon` и `MultiPolygon`. Источник данных подключен к слою линий, чтобы предоставить ему данные для подготовки к просмотру.

> [!TIP]
> Слои линий по умолчанию отображают координаты многоугольников, а также линий в источнике данных. Чтобы ограничить слой таким образом, чтобы он отображал только функции LineString Geometry, установите `filter` параметр слоя в значение `eq(geometryType(), "LineString")` . Если вы хотите включить функции MultiLineString, установите `filter` параметр слоя в значение `any(eq(geometryType(), "LineString"), eq(geometryType(), "MultiLineString"))` .

## <a name="prerequisites"></a>Предварительные требования

Не забудьте выполнить действия, описанные в разделе [Краткое руководство. Создание документа приложения Android](quick-android-map.md) . Блоки кода в этой статье можно вставить в `onReady` обработчик событий Maps.

## <a name="add-a-line-layer"></a>Добавление слоя линий

Ниже приведен код, создающий линию. Добавьте строку в источник данных, а затем выводите ее на слой линии с помощью `LineLayer` класса.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a list of points.
List<Point> points = Arrays.asList(
    Point.fromLngLat(-73.972340, 40.743270),
    Point.fromLngLat(-74.004420, 40.756800));

//Create a LineString geometry and add it to the data source.
source.add(LineString.fromLngLats(points));

//Create a line layer and add it to the map.
LineLayer layer = new LineLayer(source,
    strokeColor("blue"),
    strokeWidth(5f)
);

map.layers.add(layer);
```

На следующем снимке экрана показан приведенный выше код для отрисовки линии в слое линий.

![Сопоставьте с линией, отображенной с помощью слоя линий](media/android-map-add-line-layer/android-line-layer.png)

## <a name="data-drive-line-style"></a>Тип линии диска данных

Следующий код создает две функции линии и добавляет значение предельной скорости как свойство в каждую строку. Слой линий использует выражение стиля дискового накопителя, в котором строки основаны на значении предельной скорости. Так как данные в строке накладываются вместе с дорогими, приведенный ниже код добавляет слой линии под слоем меток, чтобы можно было четко читать дорожные метки.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a line feature.
Feature feature = Feature.fromGeometry(
    LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.131821, 47.704033),
        Point.fromLngLat(-122.099919, 47.703678))));

//Add a property to the feature.
feature.addNumberProperty("speedLimitMph", 35);

//Add the feature to the data source.
source.add(feature);

//Create a second line feature.
Feature feature2 = Feature.fromGeometry(
    LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.126662, 47.708265),
        Point.fromLngLat(-122.126877, 47.703980))));

//Add a property to the second feature.
feature2.addNumberProperty("speedLimitMph", 15);

//Add the second feature to the data source.
source.add(feature2);

//Create a line layer and add it to the map.
LineLayer layer = new LineLayer(source,
    strokeColor(
        interpolate(
            linear(),
            get("speedLimitMph"),
            stop(0, color(Color.GREEN)),
            stop(30, color(Color.YELLOW)),
            stop(60, color(Color.RED))
        )
    ),
    strokeWidth(5f)
);

map.layers.add(layer, "labels");
```

На следующем снимке экрана показан приведенный выше код, отображающий две линии в слое линий с цветовым выражением, которые извлекаются из выражения стиля, основанного на данных, на основе свойства в функциях линии.

![Map с использованием линий с данными в стиле, подготовленных к просмотру на уровне линий](media/android-map-add-line-layer/android-line-layer-data-drive-style.png)

## <a name="add-symbols-along-a-line"></a>Добавление символов вдоль линии

В этом примере показано, как добавить значки стрелок вдоль линии на карте. При использовании слоя символов задайте `symbolPlacement` для параметра значение `SymbolPlacement.LINE` . Этот параметр позволяет отображать символы вдоль линии и вращать значки (0 градусов = вправо).

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Load a image of an arrow into the map image sprite and call it "arrow-icon".
map.images.add("arrow-icon", R.drawable.purple-arrow-right);

//Create and add a line to the data source.
source.add(LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.18822, 47.63208),
        Point.fromLngLat(-122.18204, 47.63196),
        Point.fromLngLat(-122.17243, 47.62976),
        Point.fromLngLat(-122.16419, 47.63023),
        Point.fromLngLat(-122.15852, 47.62942),
        Point.fromLngLat(-122.15183, 47.62988),
        Point.fromLngLat(-122.14256, 47.63451),
        Point.fromLngLat(-122.13483, 47.64041),
        Point.fromLngLat(-122.13466, 47.64422),
        Point.fromLngLat(-122.13844, 47.65440),
        Point.fromLngLat(-122.13277, 47.66515),
        Point.fromLngLat(-122.12779, 47.66712),
        Point.fromLngLat(-122.11595, 47.66712),
        Point.fromLngLat(-122.11063, 47.66735),
        Point.fromLngLat(-122.10668, 47.67035),
        Point.fromLngLat(-122.10565, 47.67498))));

//Create a line layer and add it to the map.
map.layers.add(new LineLayer(source,
    strokeColor("DarkOrchid"),
    strokeWidth(5f)
));

//Create a symbol layer and add it to the map.
map.layers.add(new SymbolLayer(source,
    //Space symbols out along line.
    symbolPlacement(SymbolPlacement.LINE),

    //Spread the symbols out 100 pixels apart.
    symbolSpacing(100f),
    
    //Use the arrow icon as the symbol.
    iconImage("arrow-icon"),

    //Allow icons to overlap so that they aren't hidden if they collide with other map elements.
    iconAllowOverlap(true),

    //Center the symbol icon.
    iconAnchor(AnchorType.CENTER),

    //Scale the icon size.
    iconSize(0.8f)
));
```

В этом примере приведенное ниже изображение было загружено в папку для рисования приложения.

| ![Изображение значка фиолетовой стрелки](media/android-map-add-line-layer/purple-arrow-right.png)|
|:-----------------------------------------------------------------------:|
|                                                  |

На следующем снимке экрана показан приведенный выше код, отображающий строку со значками стрелок.

![Сопоставьте линии с данными в стиле диска с линиями со стрелками, отображаемыми в слое линий](media/android-map-add-line-layer/android-symbols-along-line-path.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные примеры кода для добавления в карты см. в следующих статьях:

> [!div class="nextstepaction"]
> [Создание источника данных](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Использование стилистических выражений на основе данных](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Добавление слоя многоугольников](how-to-add-shapes-to-android-map.md)
