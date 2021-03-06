---
title: Грубая перелокализация
description: Сведения об использовании грубой локализации для поиска ближайших привязок.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 01/28/2021
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.custom: devx-track-csharp
ms.openlocfilehash: fc04242e61c748d7ae52e61e40206ba338a1b6aa
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99071151"
---
# <a name="coarse-relocalization"></a>Простое уточнение расположения

Грубая Локализация — это функция, которая обеспечивает крупномасштабную локализацию, предоставляя приблизительный, но быстрый ответ на вопрос: *где мое устройство сейчас и какое содержимое следует наблюдать?* Ответ не является точным, а вместо этого находится в форме: *близко к этим привязкам; попробуйте найти один из них*.

Грубая локализация работает с помощью привязок тегов с различными считанными датчиками устройств, которые позже используются для быстрого выполнения запросов. В сценариях для общего использования данные датчика обычно являются позицией GPS (Глобальная система позиционирования) устройства. Если GPS недоступен или ненадежен (например, "крышки"), данные датчика состоят из точек доступа WiFi и маяков Bluetooth в диапазоне. Собранные данные датчика вносят вклад в хранение пространственного индекса, используемого пространственными привязками Azure, чтобы быстро определить, какие привязки находятся в близком к устройству.

## <a name="when-to-use-coarse-relocalization"></a>Когда следует использовать грубую локализацию

Если вы планируете работать с более чем 35 пространственными привязками в пространстве, превышающем судебный зал, скорее всего, вы получите преимущество от грубой перелокализации пространственного индексирования.

Быстрый поиск привязок, включенных в результате грубой локализации, предназначен для упрощения разработки приложений, поддерживающих коллекции мирового масштаба (скажем, миллионы геораспределенных географических точек). Сложность пространственного индексирования скрыта, что позволяет сосредоточиться на логике приложения. Вся работа привязок выполняется за кулисами с помощью пространственных привязок Azure.

## <a name="using-coarse-relocalization"></a>Использование грубой локализации

Типичный рабочий процесс для создания и запроса пространственных привязок Azure с грубой локализацией:
1.  Создайте и настройте поставщик отпечатков пальцев с датчика, чтобы собираются выбранные данные датчика.
2.  Запустите сеанс пространственной привязки Azure и создайте привязки. Так как отпечаток датчика включен, привязки на пространственной основе индексируются с помощью грубой локализации.
3.  Выполнение запросов к привязкам с помощью грубой локализации с использованием выделенных критериев поиска в сеансе пространственной привязки Azure.

Для настройки грубой локализации в приложении можно обратиться к соответствующему учебнику:
* [Грубая локализация в Unity](../how-tos/set-up-coarse-reloc-unity.md)
* [Грубая перелокализация в цели — C](../how-tos/set-up-coarse-reloc-objc.md)
* [Грубая перелокализация в SWIFT](../how-tos/set-up-coarse-reloc-swift.md)
* [Грубая локализация в Java](../how-tos/set-up-coarse-reloc-java.md)
* [Грубая локализация в C++/НДК](../how-tos/set-up-coarse-reloc-cpp-ndk.md)
* [Грубая локализация в C++/WinRT](../how-tos/set-up-coarse-reloc-cpp-winrt.md)

## <a name="sensors-and-platforms"></a>Датчики и платформы

### <a name="platform-availability"></a>Доступность платформы

Ниже перечислены типы данных датчика, которые можно отправить в службу привязки.

* Расположение GPS: Широта, Долгота, высота.
* Сила сигнала точек доступа WiFi в диапазоне.
* Сила сигнала сигналов маяков Bluetooth в диапазоне.

В таблице ниже сведены сведения о доступности датчиков на поддерживаемых платформах, а также предупреждения, относящиеся к конкретной платформе.

|                 | HoloLens | Android | iOS |
|-----------------|----------|---------|-----|
| **GPS**         | НЕТ<sup>1</sup>  | Да<sup>2</sup> | Да<sup>3</sup> |
| **Wi-Fi**        | Да<sup>4</sup> | Да<sup>5</sup> | NO  |
| **BLE маяки** | Да<sup>6</sup> | Да<sup>6</sup> | Да<sup>6</sup>|


<sup>1</sup> внешнее устройство GPS может быть связано с HoloLens. Если вы пожелаете использовать HoloLens с помощью средства регистрации GPS, обратитесь в [службу поддержки](../spatial-anchor-support.md) .<br/>
<sup>2</sup> поддерживается через API [ЛОКАТИОНМАНАЖЕР][3] (как GPS, так и Network)<br/>
<sup>3</sup> поддерживается через API-интерфейсы [кллокатионманажер][4]<br/>
<sup>4</sup> поддерживается с частотой приблизительно одного сканирования каждые 3 секунды <br/>
<sup>5</sup> начиная с уровня API 28, сканирование Wi-Fi регулируется до 4 вызовов каждые 2 минуты. С Android 10 регулирование можно отключить в меню "Параметры разработчика". Дополнительные сведения см. в [документации по Android][5].<br/>
<sup>6</sup> с ограничениями на [еддистоне][1] и [ибеакон][2]

### <a name="which-sensor-to-enable"></a>Выбор датчика для включения

Выбранный датчик зависит от разрабатываемого приложения и платформы.
На следующей схеме показана отправная точка, в которой можно включить сочетание датчиков в зависимости от сценария локализации.

![Схема выбора включенных датчиков](media/coarse-relocalization-enabling-sensors.png)

Следующие разделы содержат более подробные сведения о преимуществах и ограничениях для каждого типа датчика.

### <a name="gps"></a>GPS

GPS — это вариант для одностороннего сценария.
При использовании GPS в приложении следует помнить, что в нем используются следующие средства чтения:

* Асинхронная и низкая частота (менее 1 Гц).
* ненадежный/шум (для среднего стандартного отклонения в 7-м).

Как правило, как ОС устройства, так и пространственные привязки Azure будут выполнять некоторую фильтрацию и экстраполяцию на необработанном сигнале GPS, пытаясь устранить эти проблемы. Для этой дополнительной обработки требуется время для конвергенции, поэтому для получения наилучших результатов необходимо выполнить следующие действия.

* Создание одного поставщика отпечатков пальцев с датчика как можно раньше в приложении
* обеспечение активности поставщика отпечатков пальцев датчика между несколькими сеансами
* Совместное использование поставщика отпечатков пальцев с датчика между несколькими сеансами

Устройства GPS класса "потребитель" обычно неточны. Исследование, [занденбержен и барбеау (2011)][6] , сообщает медианную точность мобильных телефонов с помощью GPS (A-GPS), которая составляет около 7 метров. это довольно большое значение. Чтобы учитывать эти ошибки измерения, служба рассматривает привязки как распределения вероятностей в пространстве GPS. Таким образом, привязка теперь является областью пространства, которая, вероятнее всего, (то есть более 95% достоверности) содержит свою истинную, неизвестную точку GPS.

Та же причина применяется при выполнении запросов с помощью GPS. Устройство представляется в виде другой геопространственной области достоверности в отношении истинной, неизвестной координаты GPS. Обнаружение ближайших привязок преобразуется в простой поиск привязок с областями достоверности, *достаточно близкой* к региону достоверности устройства, как показано на рисунке ниже:

![Выбор кандидатов привязки с помощью GPS](media/coarse-reloc-gps-separation-distance.png)

### <a name="wifi"></a>WiFi

В HoloLens и Android сила сигнала Wi-Fi может быть хорошим вариантом для включения грубой локализации.
Его преимущество заключается в потенциальной доступности точек доступа WiFi (например, в приложениях Office Spaces или центрах) без дополнительной настройки.

> [!NOTE]
> iOS не предоставляет API для чтения силы сигнала Wi-Fi, поэтому его нельзя использовать для грубой локализации с поддержкой WiFi.

При использовании Wi-Fi в приложении помните о том, что для оборудования обычно используются следующие данные:

* Асинхронная и низкая частота (менее 0,1 Гц).
* потенциально регулируется на уровне ОС.
* ненадежный/шум (в среднем 3-dBm стандартное отклонение).

Пространственные привязки Azure попытаются создать отфильтрованную карту мощности сигнала Wi-Fi во время сеанса, пытаясь устранить эти проблемы. Для получения лучших результатов следует попытаться:

* Создайте сеанс, прежде чем поместить первую привязку.
* Обеспечьте активность сеанса в течение всего времени (то есть создайте все привязки и запрос в одном сеансе).

### <a name="bluetooth-beacons"></a>Маяки Bluetooth
<a name="beaconsDetails"></a>

Тщательное развертывание маяков Bluetooth является хорошим решением для крупномасштабных сценариев перелокализации больших масштабов, где GPS отсутствует или неточно. Он также является единственным методом, поддерживаемым на всех трех платформах.

Маяки обычно являются универсальными устройствами, где все, включая UUID и MAC-адреса, можно настроить. Пространственные привязки Azure предполагают уникальную идентификацию маркеров UUID. Невозможность гарантировать такую уникальность скорее всего приведет к неверным результатам. Для получения лучших результатов следует:

* Присвойте вашим маякам уникальные идентификаторы UUID.
* разверните их таким образом, чтобы обеспечить единообразное последовательное пространство, и так, чтобы по крайней мере 3 маяка были доступны из любой точки в пространстве.
* Передача списка уникальных идентификаторов UUID маяка поставщику отпечатков пальцев с датчика

На радиосигналы, такие как Bluetooth, влияют препятствия и могут мешать другим радиосигналам. По этим причинам может быть трудно угадать, является ли ваша сфера равномерной. Чтобы обеспечить лучшее взаимодействие с клиентами, рекомендуется вручную протестировать покрытие маяков. Это можно сделать путем прохода по модулю с устройствами-кандидатами и приложением, которое показывает Bluetooth в диапазоне. При тестировании покрытия убедитесь, что вы можете связаться по крайней мере с тремя маяками из любой стратегической позиции вашего пространства. Настройка слишком большого количества маяков может привести к увеличению количества помех между ними и не обязательно повысит точность перелокализации.

Если в пространстве нет препятствий, то у маркеров Bluetooth обычно есть 80 метров.
Это означает, что для пробела, не имеющего больших препятствий, один из них может развернуть маяки для шаблона сетки каждые 40 метров.

Затронутый батарея затронет результаты отрицательно, поэтому следите за периодическим наблюдением за развертыванием с низкими или неработающими батареями.

Пространственные привязки Azure будут отслеживанием только маяков Bluetooth, которые находятся в списке идентификаторов UUID с известным сходством маркеров. Вредоносные маяки, запрограммированные на наличие ИДЕНТИФИКАТОРов, которые разрешено использовать в списке, могут негативно повлиять на качество службы. По этой причине вы получите лучшие результаты в проверенных пространствах, где можно управлять развертыванием.

### <a name="sensors-accuracy"></a>Точность датчиков

Точность сигнала GPS, как при создании привязки, так и во время запросов, сильно влияет на набор возвращаемых привязок. В отличие от них, запросы, основанные на WiFi/маяках, будут рассматривать все привязки, имеющие по крайней мере одну точку доступа или Маяк с запросом. В этом смысле результат запроса, основанного на Wi-Fi/маяках, в основном определяется физическим диапазоном точек доступа, маркеров маяка и препятствий на окружающей среде.
В таблице ниже вычисляется ожидаемое пространство поиска для каждого типа датчика:

| Sensor      | Область поиска радиус пространства (примерно) | Сведения |
|-------------|:-------:|---------|
| GPS         | 20 м – 30 м | Определяется неуверенностью GPS среди других факторов. Сообщаемые числа оцениваются по срединной точности GPS для мобильных телефонов с помощью A-GPS, т. е. 7 метров. |
| WiFi        | 50 м-100 м | Определяется диапазоном точек беспроводного доступа. Зависит от частоты, силы передатчика, физических препятствий, помех и т. д. |
| BLE маяки |  70 м | Определяется диапазоном сигналов маяка. Зависит от частоты, силы передачи, физических препятствий, помех и т. д. |

<!-- Reference links in article -->
[1]: https://developers.google.com/beacons/eddystone
[2]: https://developer.apple.com/ibeacon/
[3]: https://developer.android.com/reference/android/location/LocationManager
[4]: https://developer.apple.com/documentation/corelocation/cllocationmanager?language=objc
[5]: https://developer.android.com/guide/topics/connectivity/wifi-scan
[6]: https://www.cambridge.org/core/journals/journal-of-navigation/article/positional-accuracy-of-assisted-gps-data-from-highsensitivity-gpsenabled-mobile-phones/E1EE20CD1A301C537BEE8EC66766B0A9
