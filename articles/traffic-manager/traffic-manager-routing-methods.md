---
title: Методы маршрутизации трафика средствами диспетчера трафика Azure
description: Эта статья поможет вам разобраться с различными методами маршрутизации трафика, используемыми диспетчером трафика.
services: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2021
ms.author: duau
ms.openlocfilehash: eeebded128f636ecba2e4e0dab1bc2f0632aaa6a
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98730978"
---
# <a name="traffic-manager-routing-methods"></a>Методы маршрутизации диспетчера трафика

Диспетчер трафика Azure поддерживает шесть методов маршрутизации трафика, которые определяют правила маршрутизации сетевого трафика в разные конечные точки службы. Для любого профиля диспетчер трафика применяет связанный с ним метод маршрутизации трафика к каждому полученному запросу DNS. Метод маршрутизации трафика определяет, какая конечная точка будет возвращена в ответе DNS.

В диспетчере трафика доступны следующие методы маршрутизации трафика:

* **[Приоритет](#priority-traffic-routing-method).** выберите **приоритетную** маршрутизацию, если требуется создать основную конечную точку службы для всего трафика. Можно указать несколько конечных точек резервного копирования в случае недоступности основной или одной из конечных точек резервного копирования.
* **[Взвешенный](#weighted).** выберите **взвешенную** маршрутизацию, если необходимо распределить трафик по набору конечных точек в зависимости от их веса. Задайте одинаковый вес для равномерного распределения по всем конечным точкам.
* **[Производительность](#performance).** выберите маршрутизацию **производительности** , если у вас есть конечные точки в разных географических расположениях и вы хотите, чтобы конечные пользователи использовали "ближайшую" конечную точку для наименьшей задержки сети.
* **[Географическая](#geographic).** выберите **географическая** маршрутизация для направления пользователей к конкретным конечным точкам (Azure, external или Nested) в зависимости от того, откуда исходят запросы DNS от географически. Этот метод маршрутизации позволяет обеспечить соответствие таким сценариям, как независимости данных, локализация содержимого & взаимодействия с пользователем и измерения трафика из разных регионов.
* **[Многозначный](#multivalue):** выберите **Многозначный** для профилей диспетчера трафика, которые могут иметь конечные точки только в виде IPv4- или IPv6-адресов. При получении запроса для этого профиля возвращаются все работоспособные конечные точки.
* **[Подсеть](#subnet).** выберите метод маршрутизации трафика **подсети** , чтобы сопоставлять наборы диапазонов IP-адресов конечных пользователей с определенной конечной точкой. При получении запроса возвращаемая конечная точка будет сопоставлена с этим исходным IP-адресом запроса. 


Все профили диспетчера трафика имеют наблюдение за работоспособностью и автоматический переход на другой ресурс конечных точек. Дополнительные сведения см. в статье [Мониторинг и отработка отказов конечной точки диспетчера трафика](traffic-manager-monitoring.md). В профиле диспетчера трафика можно настроить только один метод маршрутизации трафика. В любое время для профиля можно выбрать другой метод маршрутизации трафика. Изменения будут применены в течение минуты без простоев. Методы маршрутизации трафика можно объединять с помощью вложенных профилей диспетчера трафика. Профили вложенности позволяют применять сложные конфигурации маршрутизации трафика, соответствующие потребностям крупных и сложных приложений. Дополнительную информацию см. в статье [Вложенные профили диспетчера трафика](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Метод маршрутизации трафика по приоритету

Часто Организация хочет обеспечить надежность своих служб. Для этого они развертывают одну или несколько служб резервного копирования на случай, если их первичный переход не выполняется. Метод маршрутизации трафика "По приоритету" позволяет клиентам Azure легко реализовать эту схему отработки отказа.

![Диспетчер трафика Azure: маршрутизация трафика по приоритету](media/traffic-manager-routing-methods/priority.png)

В этом профиле диспетчера трафика создается упорядоченный по приоритету список конечных точек службы. По умолчанию диспетчер трафика направляет весь трафик в первичную конечную точку (с наивысшим приоритетом). Если первичная конечная точка недоступна, диспетчер трафика направляет трафик на вторую конечную точку. В ситуации, когда первичная и вторичная конечные точки недоступны, трафик переходит к третьему и т. д. Доступность конечной точки определяется по указанному для нее состоянию (включена или отключена) и данным мониторинга конечных точек.

### <a name="configuring-endpoints"></a>Настройка конечных точек

В Azure Resource Manager можно явно указать приоритет для каждой конечной точки, изменяя значение свойства priority. Допускаются значения в диапазоне от 1 до 1000. Более низкое значение представляет более высокий приоритет. Конечные точки не могут совместно использовать значения приоритета. Это необязательное свойство. Если оно не указано, для конечной точки задается приоритет по умолчанию в соответствии с ее расположением в профиле.

## <a name="weighted-traffic-routing-method"></a><a name = "weighted"></a>Метод маршрутизации трафика со взвешиванием
Маршрутизация трафика по методу взвешивания позволяет распределять трафик равномерно или в соответствии с предустановленными весовыми коэффициентами.

![Диспетчер трафика Azure: маршрутизации трафика по методу взвешивания](media/traffic-manager-routing-methods/weighted.png)

При использовании метода взвешивания при маршрутизации трафика каждой конечной точке в профиле диспетчера трафика присваивается определенный весовой коэффициент. Это целое число в диапазоне от 1 до 1000. Этот параметр является необязательным. Если значение не указано, диспетчер трафика использует коэффициент 1 по умолчанию. Чем выше коэффициент, тем выше приоритет.

Для каждого полученного запроса службы доменных имен (DNS) диспетчер трафика случайным образом выбирает конечную точку из числа доступных. Вероятность выбора конечной точки определяется весовыми коэффициентами всех доступных конечных точек. Если для всех конечных точек используются одинаковые коэффициенты, трафик будет распределяться между ними равномерно. Если задать больший (или меньший) вес для определенных конечных точек, они будут чаще (или реже) возвращаться в ответах DNS.

Метод взвешивания позволяет реализовать некоторые полезные сценарии.

* Постепенное обновление приложений: при наличии в процентах трафика, передаваемого в новую конечную точку, постепенно увеличивается объем трафика до 100%.
* Миграция приложений в Azure: создайте профиль, который включает конечные точки, расположенные в среде Azure и за ее пределами. Настройте весовые коэффициенты так, чтобы приоритет отдавался новым конечным точкам.
* Облачная оптимизация для большей емкости: позволяет быстро развернуть локальное развертывание в облаке, поместив его за профилем диспетчера трафика. Если вам требуется дополнительная емкость в облаке, добавьте конечные точки и укажите, какая часть трафика направляется на ту или иную конечную точку.

Весовые коэффициенты можно настроить с помощью портал Azure, Azure PowerShell, интерфейса командной строки или API-интерфейсов RESTFUL.

Важно помнить, что ответы DNS кэшируются клиентами. Они также кэшируются рекурсивными DNS-серверами, которые клиенты используют для разрешения имен DNS. Это кэширование может оказывать воздействие на взвешенные распределения трафика. При большом числе клиентов и рекурсивных DNS-серверов распределение трафика работает ожидаемым образом. Но если клиентов и (или) рекурсивных DNS-серверов мало, кэширование может ощутимо исказить распределение трафика.

Типичные примеры такой ситуации:

* среды для разработки и тестирования;
* обмен данных между приложениями;
* приложения, предназначенные для узкого круга пользователей, которые используют общую инфраструктуру рекурсивной DNS-службы (например, сотрудники компании, подключенные через общий прокси-сервер).

Такое воздействие кэширования DNS характерно для всех систем маршрутизации трафика на основе DNS, а не только для диспетчера трафика. В некоторых случаях может помочь явная очистка кэша DNS. Если это не сработает, может оказаться более подходящим альтернативный способ маршрутизации трафика.

## <a name="performance-traffic-routing-method"></a><a name = "performance"></a>Метод маршрутизации трафика для повышения производительности

Развертывание конечных точек в двух или более расположениях по всему миру может повысить скорость реагирования приложений. С помощью метода маршрутизации трафика "производительность" можно направить трафик в ближайшее расположение.

![Диспетчер трафика Azure: маршрутизация трафика по производительности](media/traffic-manager-routing-methods/performance.png)

Ближайшая конечная точка не обязательно является ближайшей, измеряемой по географическому расстоянию. Вместо этого при определении расстояния для маршрутизации трафика по производительности используются характеристики задержки сети. Диспетчер трафика ведет собственную таблицу задержек Интернета, в которой сохраняет время кругового пути между диапазонами IP-адресов и центрами обработки данных Azure.

Для каждого входящего запроса DNS он находит в таблице задержек Интернета строки, соответствующие IP-адресу клиента. Затем диспетчер трафика выбирает доступную конечную точку в центре обработки данных Azure с наименьшей задержкой для этого диапазона IP-адресов. Затем диспетчер трафика возвращает эту конечную точку в ответе DNS.

Как объясняется, [как работает диспетчер](traffic-manager-how-it-works.md)трафика, диспетчер трафика не получает запросы DNS непосредственно от клиентов. Вместо этого запросы DNS берутся из рекурсивной службы DNS, для использования которой настроены клиенты. Таким образом, IP-адрес, используемый для определения ближайшей конечной точки, не является IP-адресом клиента, но является IP-адресом рекурсивной службы DNS. Этот IP-адрес является хорошим прокси-сервером для клиента.


Чтобы учесть изменения в глобальной сети Интернет и добавление новых регионов Azure, диспетчер трафика регулярно обновляет используемую таблицу задержек Интернета. Но производительность приложения зависит от распределения и колебаний загрузки в сети Интернет на определенный момент времени. Трафик производительности — маршрутизация не отслеживает нагрузку на данную конечную точку службы. Если конечная точка становится недоступной, диспетчер трафика не будет включать его в ответы на запросы DNS.


Примечания:

* Если профиль содержит несколько конечных точек, которые входят в один регион Azure, диспетчер трафика распределяет трафик равномерно между всеми доступными конечными точками в этом регионе. Если вы предпочитаете распределение трафика в пределах региона, используйте [вложенные профили диспетчера трафика](traffic-manager-nested-profiles.md).
* Если работоспособность всех задействованных конечных точек в ближайших регионах Azure снижена, диспетчер трафика перенаправляет трафик к конечным точкам в следующем ближайшем регионе Azure. Если вы хотите определить свою логику отработки сбоя, используйте [вложенные профили диспетчера трафика](traffic-manager-nested-profiles.md).
* Чтобы использовать маршрутизацию трафика по производительности между внешними или вложенными конечными точками, нужно вручную указать их расположение. Выберите ближайший к вашему развертыванию регион Azure. Именно эти расположения отслеживаются в таблице задержек Интернета.
* Алгоритм выбора конечной точки детерминирован. Повторные запросы DNS от одного клиента будут направляться к той же конечной точке. Обычно клиенты используют разные рекурсивные серверы DNS, когда путешествуют. Возможно, в этом случае клиент будет перенаправлен к другой конечной точке. Кроме того, на маршрутизацию могут влиять изменения в таблице задержек Интернета. Именно поэтому метод маршрутизации трафика производительности не гарантирует, что клиент всегда перенаправляется к той же конечной точке.
* При изменении таблицы задержек Интернета вы можете заметить, что некоторые клиенты переключились на другую конечную точку. Это изменение маршрутизации более точно основано на текущих данных задержки. Данные обновления необходимы, чтобы обеспечить точность маршрутизации трафика для повышения производительности, так как Интернет постоянно развивается.

## <a name="geographic-traffic-routing-method"></a><a name = "geographic"></a>Метод географической маршрутизации трафика

Профили диспетчера трафика можно настроить для использования метода географической маршрутизации, чтобы пользователи направлялись на конкретные конечные точки (Azure, внешние или вложенные), основанные на географическом расположении, откуда поступил запрос DNS. Этот метод маршрутизации позволяет обеспечить соответствие требованиям независимости данных, локализации содержимого & взаимодействия с пользователем и измерения трафика из разных регионов.
Если в профиле настроена географическая маршрутизация, то каждой конечной точке, связанной с профилем, необходимо назначить набор географических регионов. Ниже приведены возможные уровни детализации географических регионов. 
- Мир — любой регион.
- Группа регионов — например, Африка, Ближний Восток, Австралия и Тихоокеанский регион и т. д. 
- Страна или регион — например, Ирландия, Перу, Гонконг, САР и т. д. 
- Область, штат, провинция — например, Калифорния (США), Квинсленд (Австралия), Альберта (Канада) и т. д. (Обратите внимание: этот уровень детализации поддерживается только для штатов и провинций в Австралии, Канаде и США.)

Если конечной точке назначен регион или набор регионов, любые запросы из этих регионов маршрутизируются только на эту конечную точку. Диспетчер трафика использует исходный IP-адрес запроса DNS для определения региона, из которого пользователь выполняет запрос. Обычно в качестве IP-адреса локального сопоставителя DNS, который делает запрос для пользователя.  

![Диспетчер трафика Azure: метод географической маршрутизации трафика](./media/traffic-manager-routing-methods/geographic.png)

Диспетчер трафика считывает исходный IP-адрес запроса DNS и определяет, из какого географического региона он является. Затем он просматривает наличие конечной точки, которой сопоставлен этот географический регион. Этот поиск начинается с самого низкого уровня детализации (штата или Республики, где он поддерживается, в другом месте на уровне страны или региона) и проходит до самого высокого уровня, который является **мировым**. Первое совпадение, найденное с помощью этого обхода, выбирается в качестве конечной точки, возвращаемой в ответе на запрос. Если это конечная точка вложенного типа, то будет возвращена конечная точка в соответствующем дочернем профиле, в зависимости от его метода маршрутизации. В такой ситуации возможно следующее.

- Если используется географическая маршрутизация, то географический регион может быть сопоставлен только с одной конечной точкой в профиле диспетчера трафика. Это ограничение обеспечивает детерминированную маршрутизацию пользователей, а клиенты могут включать сценарии, для которых требуются однозначные географические границы.
- Если регион пользователя указан в двух разных конечных точках, диспетчер трафика выбирает конечную точку с наименьшей гранулярностью. Диспетчер трафика не учитывает запросы маршрутизации из этого региона в другую конечную точку. Рассмотрим пример профиля с географической маршрутизацией и двумя конечными точками: Endpoint1 и Endpoint2. Endpoint1 настроена для приема трафика из Ирландии, а Endpoint2 — для приема трафика из Европы. Если запрос исходит из Ирландии, он всегда перенаправляется в Endpoint1.
- Так как регион может быть сопоставлен только с одной конечной точкой, диспетчер трафика возвращает ответ о том, является ли конечная точка работоспособной или нет.

    >[!IMPORTANT]
    >Клиентам настоятельно рекомендуется использовать метод географической маршрутизации с конечными точками вложенного типа, у которых имеются дочерние профили, содержащие по крайней мере по две конечные точки.
- Если найдена соответствующая конечная точка, которая **остановлена**, то диспетчер трафика вернет ответ NODATA. В этом случае поиски не продолжаются вверх по иерархии географических регионов. Это также относится к конечным точкам вложенного типа, когда дочерний профиль **остановлен** или **отключен**.
- Если конечная точка **отключена**, она не участвует в процессе сопоставления с регионом. Это также относится к конечным точкам вложенного типа, когда конечная точка **отключена**.
- Если поступает запрос из географического региона, сопоставления с которым в данном профиле нет, то диспетчер трафика возвращает ответ NODATA. Поэтому мы настоятельно рекомендуем использовать географическую маршрутизацию с одной конечной точкой. В идеале — вложенный тип с двумя конечными точками в дочернем профиле, которому назначена область **мира** . Эта конфигурация также гарантирует, что все IP-адреса, которые не сопоставляются с регионом, будут обработаны.

Как объясняется, [как работает диспетчер](traffic-manager-how-it-works.md)трафика, диспетчер трафика не получает запросы DNS непосредственно от клиентов. Запросы DNS берутся из рекурсивной службы DNS, для использования которой настроены клиенты. Именно поэтому IP-адрес, используемый для определения региона, не является IP-адресом клиента, а IP-адресом рекурсивной службы DNS. Этот IP-адрес является хорошим прокси-сервером для клиента.

### <a name="faqs"></a>Частые вопросы

* [В каких случаях следует использовать географическую маршрутизацию?](./traffic-manager-faqs.md#what-are-some-use-cases-where-geographic-routing-is-useful)

* [Как решить, какой метод использовать — метод маршрутизации трафика по производительности или метод географической маршрутизации?](./traffic-manager-faqs.md#how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method)

* [Какие регионы диспетчер трафика поддерживает для географической маршрутизации?](./traffic-manager-faqs.md#what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing)

* [Как диспетчер трафика определяет, откуда пользователь обращается к приложению?](./traffic-manager-faqs.md#how-does-traffic-manager-determine-where-a-user-is-querying-from)

* [Есть ли гарантия, что диспетчер трафика правильно определит точное географическое расположение пользователя во всех случаях?](./traffic-manager-faqs.md#is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case)

* [Должна ли конечная точка быть физически размещена в том же регионе, что и конфигурация для географической маршрутизации?](./traffic-manager-faqs.md#does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing)

* [Можно ли назначить географические регионы конечным точкам в профиле, не настроенном для географической маршрутизации?](./traffic-manager-faqs.md#can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing)

* [Почему, когда я пытаюсь изменить метод маршрутизации существующего профиля на географический, возникает ошибка?](./traffic-manager-faqs.md#why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic)

* [Почему клиентам настоятельно рекомендуется создавать в профиле с географической маршрутизацией вложенные профили вместо конечных точек?](./traffic-manager-faqs.md#why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled)

* [Существуют ли ограничения на версии API, которые поддерживает этот тип маршрутизации?](./traffic-manager-faqs.md#are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type)

## <a name="multivalue-traffic-routing-method"></a><a name = "multivalue"></a>Метод маршрутизации трафика "Многозначный"
Метод маршрутизации трафика **Многозначный** позволяет получить несколько работоспособных конечных точек в одном ответе на запрос DNS. Эта конфигурация позволяет вызывающему объекту выполнять повторные попытки на стороне клиента с другими конечными точками в случае, если возвращенная конечная точка не отвечает. Эта модель может повысить доступность службы и сократить задержки, связанные с новым запросом DNS на получение работоспособной конечной точки. Метод маршрутизации "Многозначный" работает только в том случае, если все конечные точки внешние и являются IPv4- или IPv6-адресами. При получении запроса для этого профиля все работоспособные конечные точки возвращаются на основе настраиваемого максимального числа записей в ответе.

### <a name="faqs"></a>Частые вопросы

* [В каких случаях следует использовать маршрутизацию с несколькими значениями?](./traffic-manager-faqs.md#what-are-some-use-cases-where-multivalue-routing-is-useful)

* [Сколько конечных точек возвращается, если используется маршрутизация с несколькими значениями?](./traffic-manager-faqs.md#how-many-endpoints-are-returned-when-multivalue-routing-is-used)

* [Я буду получать одинаковый набор конечных точек при использовании маршрутизации с несколькими значениями?](./traffic-manager-faqs.md#will-i-get-the-same-set-of-endpoints-when-multivalue-routing-is-used)

## <a name="subnet-traffic-routing-method"></a><a name = "subnet"></a>Метод маршрутизации трафика "Подсеть"
Метод маршрутизации трафика **подсети** позволяет сопоставлять набор ДИАПАЗОНов IP-адресов конечных пользователей с конкретными конечными точками в профиле. Если диспетчер трафика получает запрос DNS для этого профиля, он будет проверять исходный IP-адрес этого запроса. Затем он определит конечную точку, с которой она сопоставлена, и возвратит эту конечную точку в ответе на запрос. В большинстве случаев исходный IP-адрес — это сопоставитель DNS, используемый вызывающим объектом.

IP-адрес, который необходимо сопоставить с конечной точкой, можно указать в виде диапазонов CIDR (например, 1.2.3.0/24) или в качестве диапазона адресов (например, 1.2.3.4-5.6.7.8). Диапазоны IP-адресов, связанные с конечной точкой, должны быть уникальными в пределах этого профиля. Диапазон адресов не может иметь перекрытие с набором IP-адресов другой конечной точки в том же профиле.
При определении конечной точки без диапазона адресов, которая используется в качестве резервной и принимает трафик из оставшихся подсетей. Если резервная конечная точка отсутствует, диспетчер трафика отправляет ответ NODATA для всех неопределенных диапазонов. Настоятельно рекомендуется определить резервную конечную точку, чтобы обеспечить указание всех возможных диапазонов IP-адресов в конечных точках.

Маршрутизацию подсети можно использовать для предоставления различных возможностей для пользователей, подключающихся из определенного пространства IP-адресов. Например, можно сделать так, чтобы все запросы из корпоративного офиса направлялись в другую конечную точку. Этот метод маршрутизации особенно полезен, если вы пытаетесь протестировать только внутреннюю версию приложения. Другой сценарий — если вы хотите указать другие возможности для пользователей, подключающихся из конкретного поставщика услуг Интернета (например, заблокировать пользователей данного поставщика услуг Интернета).

### <a name="faqs"></a>Частые вопросы

* [В каких случаях следует использовать маршрутизацию подсети?](./traffic-manager-faqs.md#what-are-some-use-cases-where-subnet-routing-is-useful)

* [Как диспетчер трафика узнает IP-адрес конечного пользователя?](./traffic-manager-faqs.md#how-does-traffic-manager-know-the-ip-address-of-the-end-user)

* [Как можно указать IP-адреса при использовании маршрутизации подсети?](./traffic-manager-faqs.md#how-can-i-specify-ip-addresses-when-using-subnet-routing)

* [Как указать резервную конечную точку при использовании маршрутизации подсети?](./traffic-manager-faqs.md#how-can-i-specify-a-fallback-endpoint-when-using-subnet-routing)

* [Что произойдет, если конечная точка отключена в профиле типа маршрутизации подсети?](./traffic-manager-faqs.md#what-happens-if-an-endpoint-is-disabled-in-a-subnet-routing-type-profile)


## <a name="next-steps"></a>Следующие шаги

Узнайте, как разрабатывать приложения с высоким уровнем доступности с помощью [мониторинга конечных точек диспетчера трафика](traffic-manager-monitoring.md)