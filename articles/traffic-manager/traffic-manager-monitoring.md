---
title: Мониторинг конечных точек диспетчера трафика Azure | Документация Майкрософт
description: Из этой статьи вы узнаете, каким образом благодаря мониторингу и автоматической отработке отказа конечных точек в диспетчере трафика пользователи Azure могут развертывать высокодоступные приложения.
services: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/22/2021
ms.author: duau
ms.openlocfilehash: 455eeb83ef5a9608c077de24b8d3d4722d26a822
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98742715"
---
# <a name="traffic-manager-endpoint-monitoring"></a>Мониторинг конечных точек в диспетчере трафика

Диспетчер трафика Azure включает в себя встроенные возможности мониторинга и автоматическую отработку отказа конечных точек. Благодаря этому можно получать высокодоступные приложения, которые устойчивы к сбоям конечной точки, в том числе к сбоям региона Azure.

## <a name="configure-endpoint-monitoring"></a>Настройка мониторинга конечных точек

Чтобы настроить мониторинг конечных точек, необходимо указать следующие параметры в профиле диспетчера трафика.

* **Протокол**. Выберите протокол HTTP, HTTPS или TCP, который диспетчер трафика будет использовать при проверке работоспособности конечной точки. Мониторинг HTTPS не проверяет допустимость сертификата TLS/SSL — он только проверяет наличие сертификата.
* **Порт**. Выберите порт, используемый для запроса.
* **Path**. Этот параметр конфигурации является допустимым только для протоколов HTTP и HTTPS, для которых требуется указание параметра пути. Если указать этот параметр для протокола мониторинга TCP, вы получите ошибку. Для протокола HTTP и HTTPS введите относительный путь и имя веб-страницы или файла, к которому обращается средство мониторинга. В качестве относительного пути можно указать косую черту (/). Это значение указывает, что файл находится в корневом каталоге (по умолчанию).
* **Параметры настраиваемого заголовка**. Этот параметр конфигурации помогает добавлять определенные заголовки HTTP к проверкам работоспособности, которые диспетчер трафика отправляет конечным точкам в профиле. Пользовательские заголовки можно задать на уровне профиля, чтобы они применялись для всех конечных точек в этом профиле, или на уровне конечной точки, чтобы они применялись только для этой конечной точки. Пользовательские заголовки можно использовать для проверки работоспособности конечных точек в среде с несколькими клиентами. Таким образом, его можно правильно направить в назначение, указав заголовок узла. Этот параметр можно также использовать, добавив уникальные заголовки, которые могут использоваться для определения запросов HTTP(S) диспетчера трафика и на основе этого обрабатываться по-разному. Можно указать до восьми пар "заголовок: значение", разделенных запятыми. Например, "Header1: Значение1, header2: value2". 
* **Ожидаемые диапазоны кода состояния**. Этот параметр позволяет указать несколько диапазонов успешного выполнения кода в формате 200-299, 301-301. Если эти коды состояния получены как ответ от конечной точки после завершения проверки работоспособности, диспетчер трафика помечает эти конечные точки как работоспособные. Можно указать не более восьми диапазонов кодов состояний. Этот параметр применим только для протокола HTTP и HTTPS, а также для всех конечных точек. Этот параметр находится на уровне профиля диспетчера трафика, и по умолчанию значение 200 определяется как код состояния успеха.
* **Интервал** проверки. Это значение указывает, как часто проверяется работоспособность конечной точки с помощью агента проверки диспетчера трафика. Вы можете указать здесь два значения: 30 секунд (обычная проверка) и 10 секунд (быстрая проверка). Если не указать значения, профиль задаст значение по умолчанию 30 секунд. Посетите страницу [цен диспетчера трафика](https://azure.microsoft.com/pricing/details/traffic-manager), чтобы узнать больше о ценах на быструю проверку.
* **Допустимое количество сбоев**. Это значение указывает количество сбоев, которое допускает агент проверки диспетчера трафика. По достижении этого количества конечная точка будет помечена как неработоспособная. Значение должно быть в диапазоне от 0 до 9. Значение 0 означает, что при первом сбое конечная точка будет помечена как неработоспособная. Если значение не задано, используется значение по умолчанию 3.
* **Время ожидания пробы**. Это свойство указывает период времени, в течение которого агент проверки диспетчера трафика должен ожидать, прежде чем проанализировать проверку работоспособности до конечной точки. Если для интервала проверки задано значение 30 секунд, тогда вы можете задать значение времени ожидания 5–10 секунд. Если значение не задано, используется значение по умолчанию 10 секунд. Если для интервала проверки задано значение 10 секунд, тогда вы можете задать значение времени ожидания 5–9 секунд. Если значение времени ожидания не задано, используется значение по умолчанию 9 секунд.

    ![Мониторинг конечных точек в диспетчере трафика](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

    **Рис.1. Мониторинг конечных точек в диспетчере трафика**

## <a name="how-endpoint-monitoring-works"></a>Как выполняется мониторинг конечных точек

Если для протокола мониторинга задано значение HTTP или HTTPS, агент проверки диспетчера трафика выполняет запрос GET к конечной точке, используя указанный протокол, порт и относительный путь. Конечная точка считается работоспособной, если агент зондирования получает ответ 200-ОК или любой из ответов, настроенных в **ожидаемых \* диапазонах кода состояния**. Если ответ имеет другое значение или в течение времени ожидания не получен ответ, агент проверки диспетчера трафика будет повторен в соответствии с настройкой допустимого количества неудачных попыток. Если этот параметр имеет значение 0, повторные попытки не выполняются. Конечная точка помечается как неработоспособная, если число последовательных сбоев превышает значение, заданное параметром число неудачных попыток.

Если протокол мониторинга имеет значение TCP, агент проверки диспетчера трафика создает запрос TCP-соединения, используя указанный порт. Если конечная точка отвечает на запрос с ответом на установление соединения, проверка работоспособности помечается как успешная. Агент проверки диспетчера трафика сбрасывает TCP-подключение. В случаях, когда ответ имеет другое значение или в течение времени ожидания не получен ответ, агент проверки диспетчера трафика выполняет повторную попытку в соответствии с настройкой допустимого количества неудачных попыток. Если этот параметр имеет значение 0, повторные попытки не выполняются. Если число последовательных сбоев превышает значение, указанное в параметре допустимого числа сбоев, тогда эта конечная точка помечается как неработоспособная.

Во всех случаях диспетчер трафика проверяет из нескольких расположений. Последовательный сбой определяет, что происходит в каждом регионе. Вот почему конечные точки получают пробы работоспособности от диспетчера трафика с более высокой частотой, чем параметр, используемый для интервала проверки.

>[!NOTE]
>На стороне конечной точки для протоколов мониторинга HTTP и HTTPS распространенной практикой является реализация пользовательской страницы в приложении, например /health.aspx. Используя этот путь для мониторинга, можно выполнить проверки для конкретного приложения, такие как проверка счетчиков производительности или проверка доступности базы данных. На основании этих пользовательских проверок страница возвращает соответствующий код состояния HTTP.

Все конечные точки в профиле диспетчера трафика имеют общие параметры мониторинга. Если необходимо использовать разные параметры мониторинга для разных конечных точек, можно создать [вложенные профили диспетчера трафика](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Состояние конечной точки и профиля

Можно включать и отключать профили и конечные точки диспетчера трафика. Однако изменение состояния конечной точки также может произойти из-за автоматизированных параметров и процессов диспетчера трафика.

### <a name="endpoint-status"></a>Состояние конечной точки

Можно включить или отключить определенную конечную точку. Это не повлияет на базовую службу, которая может остаться работоспособной. Изменение состояния конечной точки определяет ее доступность в профиле диспетчера трафика. Если состояние конечной точки — "отключено", диспетчер трафика не проверяет его работоспособность, а конечная точка не включается в ответ DNS.

### <a name="profile-status"></a>Состояние профиля

С помощью параметра "состояние профиля" можно включить или отключить определенный профиль. Если состояние конечной точки влияет на одну конечную точку, то состояние профиля влияет на весь профиль, включая все конечные точки. При отключении профиля конечные точки не проверяются на работоспособность, а конечные точки не включаются в ответ DNS. Для запроса DNS возвращается код ответа [NXDOMAIN](https://tools.ietf.org/html/rfc2308).

### <a name="endpoint-monitor-status"></a>Отслеживаемое состояние конечной точки

Отслеживаемое состояние конечной точки — это параметр, создаваемый диспетчером трафика, который отражает состояние конечной точки. Этот параметр нельзя изменить вручную. Отслеживаемое состояние конечной точки представляет собой сочетание отслеживания и настроенного состояния конечной точки. В следующей таблице показаны возможные значения отслеживаемого состояния конечной точки:

| Состояние профиля | Состояние конечной точки | Отслеживаемое состояние конечной точки | Примечания |
| --- | --- | --- | --- |
| Выключено |Активировано |Неактивно |Профиль отключен. Хотя конечная точка остается в состоянии "Включено", состояние профиля ("Отключено") имеет более высокий приоритет. Конечные точки в отключенных профилях не отслеживаются. Для запроса DNS возвращается код ответа NXDOMAIN. |
| &lt;любой&gt; |Выключено |Выключено |Конечная точка отключена. Отключенные конечные точки не отслеживаются. Конечная точка не включена в ответы DNS, так как она не получает трафик. |
| Активировано |Активировано |Миграция по сети |Конечная точка отслеживается и работоспособна. Он включается в ответы DNS и может принимать трафик. |
| Активировано |Активировано |Деградация |Проверки работоспособности мониторинга конечной точки завершаются ошибкой. Конечная точка не входит в ответы DNS и не получает трафик. <br>Исключением является, если все конечные точки имеют пониженную производительность. В этом случае все они считаются возвращаемыми в ответе на запрос.</br>|
| Активировано |Активировано |Проверка конечной точки |Конечная точка отслеживается, но результаты первой проверки еще не получены. CheckingEndpoint — это временное состояние, которое обычно возникает сразу же после добавления конечной точки или ее включения в профиле. Конечная точка в таком состоянии включается в ответы DNS и может получать трафик. |
| Активировано |Активировано |Остановлена |Веб-приложение, на которое указывает конечная точка, не работает. Проверьте параметры веб-приложения. Это состояние также может произойти, если конечная точка имеет тип "вложенная конечная точка", а дочерний профиль отключен или неактивен. <br>Конечная точка с остановленным состоянием не отслеживается. Он не включается в ответы DNS и не получает трафик. Исключением является, если все конечные точки имеют пониженную производительность. В этом случае все они будут рассматриваться как возвращаемые в ответе на запрос.</br>|

Дополнительные сведения о вычислении отслеживаемого состояния конечной точки для вложенных конечных точек см. в статье [Вложенные профили диспетчера трафика](traffic-manager-nested-profiles.md).

>[!NOTE]
> Состояние монитора "Конечная точка остановлена" может возникнуть в службе приложений, если веб-приложение запущено на уровне, отличном от "Стандартный" или выше. Дополнительные сведения см. в статье [Управление трафиком службы приложений Azure с помощью диспетчера трафика Azure](../app-service/web-sites-traffic-manager.md).

### <a name="profile-monitor-status"></a>Состояние монитора профиля

Отслеживаемое состояние профиля — это сочетание настроенного состояния профиля и отслеживаемых состояний для всех конечных точек. Возможные значения описаны в следующей таблице:

| Состояние профиля (согласно настройкам) | Отслеживаемое состояние конечной точки | Состояние монитора профиля | Примечания |
| --- | --- | --- | --- |
| Отключен |&lt;любой&gt; или профиль без определенных конечных точек. |Отключен |Профиль отключен. |
| Включено |По крайней мере одна конечная точка находится в состоянии пониженной функциональности. |Деградация |Просмотрите значения состояния отдельных конечных точек, чтобы определить, какие конечные точки необходимо рассмотреть. |
| Включено |По крайней мере одна конечная точка находится в сети. Отсутствуют конечные точки в состоянии пониженной функциональности. |Миграция по сети |Служба принимает трафик. В дальнейших действиях нет необходимости. |
| Включено |По крайней мере одна конечная точка находится в состоянии проверки конечной точки. Нет конечных точек, находящихся в сети или в состоянии пониженной функциональности. |Проверка конечных точек |Это переходное состояние возникает при создании или включении профиля. Работоспособность конечной точки проверяется в первый раз. |
| Включено |Все конечные точки профиля отключены или остановлены, либо в профиле не определены конечные точки. |Неактивно |Конечные точки не активны, но профиль включен. |

## <a name="endpoint-failover-and-recovery"></a>Отработка отказа и восстановление конечной точки

Диспетчер трафика периодически проверяет работоспособность всех конечных точек, включая неработоспособные конечные точки. Он обнаруживает, когда конечная точка восстановила работоспособность, и возвращает ее в работу.

Конечная точка считается неработоспособной при возникновении любого из следующих событий:

- Если используется протокол мониторинга HTTP или HTTPS:
    - Ответ, не связанный с 200, или ответ, который не включает диапазон состояния, указанный в поле **Ожидаемые диапазоны кода состояния** получить полученное значение. (Включая другой код 2xx или перенаправление 301/302).
- Если используется протокол мониторинга TCP: 
    - Ответ, отличный от ACK или SYN-ACK, получается в ответ на запрос SYN, отправленный диспетчером трафика для попытки установки соединения.
- Превышено время ожидания. 
- Любые другие проблемы с подключением, из-за которых невозможно получить доступ к конечной точке.

Дополнительную информацию об устранении неполадок, вызвавших сбой при проверке, см. в разделе [Устранение неполадок с состоянием пониженной функциональности в диспетчере трафика Azure](traffic-manager-troubleshooting-degraded.md). 

Временная шкала на следующем рисунке представляет собой подробное описание процесса мониторинга конечной точки диспетчера трафика, имеющего следующие параметры.

* Протокол мониторинга — HTTP.
* Интервал проверки составляет 30 секунд.
* Количество допустимых сбоев — 3.
* Значение времени ожидания — 10 секунд.
* Срок жизни DNS-сервера составляет 30 секунд.

![Последовательность действий при отработке отказа и восстановлении после сбоя конечной точки диспетчера трафика](./media/traffic-manager-monitoring/timeline.png)

**Рис.2. Последовательность отработки отказа и восстановления конечной точки диспетчера трафика**

1. **Получить**. Для каждой конечной точки система мониторинга диспетчера трафика выполняет запрос GET по пути, указанному в параметрах мониторинга.
2. **200 ОК или пользовательский диапазон кода указанные параметры мониторинга профиля диспетчера трафика**. Система мониторинга принимает ошибку HTTP 200 OK или код состояния в диапазоне, указанном в параметрах мониторинга, возвращаемых в течение 10 секунд. Получив этот ответ, система распознает облачную службу как доступную.
3. **30 секунд между проверками**. Проверка работоспособности конечной точки выполняется каждые 30 секунд.
4. **Служба недоступна**. Служба становится недоступной. Диспетчер трафика не знает, пока не будет выполнена следующая проверка работоспособности.
5. **Попытки получить доступ к пути мониторинга**. Система мониторинга выполняет запрос GET, но не получает ответ в течение периода ожидания, равного 10 секундам. Затем выполняется еще три попытки с интервалом в 30 секунд. Если одна из попыток оказывается успешной, число попыток сбрасывается.
6. **Устанавливается состояние пониженной функциональности**. После четырех последовательных неудачных попыток система мониторинга помечает недоступную конечную точку как находящуюся в состоянии пониженной функциональности.
7. **Трафик направляется на другие конечные точки**. DNS-серверы диспетчера трафика обновляются, и диспетчер трафика больше не возвращает эту конечную точку в ответ на запросы DNS. Новые подключения направляются к другим, доступным конечным точкам. Однако рекурсивные DNS-серверы и DNS-клиенты могут по-прежнему содержать в кэше предыдущие ответы DNS с этой конечной точкой. Клиенты продолжат использовать конечную точку, пока не истечет срок действия кэша DNS. По истечении срока действия этого кэша клиенты создадут новые запросы DNS и будут направлены к другим конечным точкам. Длительность кэширования задается с помощью параметра срока жизни в профиле диспетчера трафика. Например, это может быть 30 секунд.
8. **Проверки работоспособности продолжаются**. Диспетчер трафика продолжает проверять работоспособность конечной точки, пока она находится в состоянии пониженной функциональности. Диспетчер трафика обнаруживает, когда конечная точка восстановила работоспособность.
9. **Служба возвращается в сеть**. Служба становится доступной. Конечная точка сохраняет состояние пониженной функциональности в диспетчере трафика до тех пор, пока система мониторинга не выполнит следующую проверку работоспособности.
10. **Возобновляется направление трафика к службе**. Диспетчер трафика отправляет запрос GET и получает ответ о состоянии "200 OK". Служба вернулась в работоспособное состояние. Серверы имен диспетчера трафика снова обновляются и начинают передавать DNS-имя службы в ответах DNS. Трафик возвращается в конечную точку в виде кэшированных ответов DNS, которые возвращают другие конечные точки, а также по окончании срока действия существующих подключений к другим конечным точкам.

    > [!NOTE]
    > Так как диспетчер трафика работает на уровне DNS, он не может влиять на существующие подключения к какой-либо конечной точке. При маршрутизации трафика между конечными точками (путем изменения параметров профиля либо во время отработки отказа или восстановления размещения) диспетчер трафика направляет новые подключения к доступным конечным точкам. Однако другие конечные точки могут по-прежнему получать трафик через имеющиеся подключения, пока эти сеансы не будут завершены. Чтобы трафик не проходил через имеющиеся подключения, для приложений следует ограничить продолжительность сеанса в каждой конечной точке.

## <a name="traffic-routing-methods"></a>Методы маршрутизации трафика

Когда конечная точка имеет состояние пониженной функциональности, она больше не возвращается в ответ на запросы DNS. Вместо этого выбирается и возвращается альтернативная конечная точка. Метод маршрутизации трафика, заданный в профиле, определяет, как выбирается альтернативная конечная точка.

* **Приоритет**. Конечные точки формируют список по приоритетам. Всегда возвращается первая доступная конечная точка в списке. Если она переходит в состояние пониженной функциональности, то возвращается следующая доступная конечная точка.
* **Взвешивание**. Все доступные конечные точки выбираются случайным образом в зависимости от назначенных весов и веса других доступных конечных точек.
* **Производительность**. Возвращается конечная точка, ближайшая к конечному пользователю. В случае недоступности этой конечной точки диспетчер трафика перемещает трафик на конечные точки в следующем ближайшем регионе Azure. С помощью [вложенных профилей диспетчера трафика](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region) можно настроить альтернативные планы отработки отказа для повышения производительности маршрутизации.
* **Географическое** положение. Возвращается конечная точка, сопоставленная для обработки географического расположения на основе IP-адреса запроса. Если эта конечная точка недоступна, для отработки отказа не будет выбрана другая конечная точка, так как географическое расположение может быть сопоставлено только с одной конечной точкой в профиле. (Дополнительные сведения см. в [разделе часто задаваемые вопросы](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). В качестве оптимального варианта при использовании географической маршрутизации мы рекомендуем клиентам использовать вложенные профили диспетчера трафика с несколькими конечными точками профиля.
* **MultiValue**. Возвращается множество конечных точек, сопоставленных с IPv4- или IPv6-адресами. При получении запроса для этого профиля возвращаются работоспособные конечные точки на основе **максимального числа записей в** указанном значении ответа. Количество ответов по умолчанию равно двум конечным точкам.
* **Подсеть**. Возвращается конечная точка, сопоставленная с набором диапазонов IP-адресов. При получении запроса с этого IP-адреса возвращаемая конечная точка, сопоставленная с этим IP-адресом. 

Дополнительные сведения см. в разделе [Методы маршрутизации трафика диспетчером трафика](traffic-manager-routing-methods.md).

> [!NOTE]
> Единственное исключение для нормального поведения маршрутизации трафика возникает, когда все конечные точки находятся в состоянии пониженной функциональности. В этом случае диспетчер трафика по возможности *воспринимает конечные точки в состоянии пониженной функциональности как работающие и находящиеся в сети*. Такое поведение предпочтительнее альтернативного варианта — вообще не возвращать конечную точку в ответе DNS. Отключенные и остановленные конечные точки не отслеживаются, таким образом, они не считаются допустимыми для трафика.
>
> Это условие обычно вызвано неправильной конфигурацией службы, например:
>
> * проверки работоспособности диспетчера трафика блокируются списком управления доступом [ACL];
> * неправильная конфигурация порта или протокола мониторинга в профиле диспетчера трафика.
>
> В результате такого поведения, даже если проверки работоспособности диспетчера трафика настроены неправильно, с точки зрения маршрутизации трафика может показаться, что диспетчер трафика *работает правильно*. Кроме того, в этом случае при сбое конечной точки не происходит отработка отказа, что негативно влияет на общую доступность приложения. Поэтому важно убедиться, что профиль находится в сети, а не в состоянии пониженной функциональности. Состояние "В сети" указывает на то, что проверки работоспособности диспетчера трафика выполняются надлежащим образом.

Дополнительную информацию об устранении неполадок, вызвавших сбой при проверке работоспособности, см. в разделе [Устранение неполадок с состоянием пониженной функциональности в диспетчере трафика Azure](traffic-manager-troubleshooting-degraded.md).

## <a name="faqs"></a>Частые вопросы

* [Устойчив ли диспетчер трафика к сбоям в регионе Azure?](./traffic-manager-faqs.md#is-traffic-manager-resilient-to-azure-region-failures)

* [Каким образом выбор расположения группы ресурсов влияет на диспетчер трафика?](./traffic-manager-faqs.md#how-does-the-choice-of-resource-group-location-affect-traffic-manager)

* [Как определить текущую работоспособность каждой конечной точки?](./traffic-manager-faqs.md#how-do-i-determine-the-current-health-of-each-endpoint)

* [Можно ли отслеживать конечные точки HTTPS?](./traffic-manager-faqs.md#can-i-monitor-https-endpoints)

* [Необходимо использовать IP-адрес или имя DNS при добавлении конечной точки?](./traffic-manager-faqs.md#do-i-use-an-ip-address-or-a-dns-name-when-adding-an-endpoint)

* [Какие типы IP-адресов можно использовать при добавлении конечной точки?](./traffic-manager-faqs.md#what-types-of-ip-addresses-can-i-use-when-adding-an-endpoint)

* [Можно ли использовать разные типы адресов конечной точки в одном профиле?](./traffic-manager-faqs.md#can-i-use-different-endpoint-addressing-types-within-a-single-profile)

* [Что происходит, когда тип записи входящего запроса отличается от типа записи, связанного с типом адреса конечных точек?](./traffic-manager-faqs.md#what-happens-when-an-incoming-querys-record-type-is-different-from-the-record-type-associated-with-the-addressing-type-of-the-endpoints)

* [Можно ли использовать профиль с конечными точками с адресами IPv4/IPv6 во вложенном профиле?](./traffic-manager-faqs.md#can-i-use-a-profile-with-ipv4--ipv6-addressed-endpoints-in-a-nested-profile)

* [Вы остановили конечную точку веб-приложения в своем профиле диспетчера трафика, но не получаете никакого трафика даже после его перезапуска. Как это исправить?](./traffic-manager-faqs.md#i-stopped-an-web-application-endpoint-in-my-traffic-manager-profile-but-i-am-not-receiving-any-traffic-even-after-i-restarted-it-how-can-i-fix-this)

* [Можно ли использовать диспетчер трафика, даже если у моего приложения нет поддержки HTTP или HTTPS?](./traffic-manager-faqs.md#can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https)

* [Какие стоит рассмотреть ответы от конечной точки при использовании протокола ТСР как протокола мониторинга?](./traffic-manager-faqs.md#what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring)

* [Насколько быстро диспетчер трафика перемещает пользователей из неработоспособной конечной точки?](./traffic-manager-faqs.md#how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint)

* [Как я могу указать в профиле другие параметры мониторинга для других конечных точек?](./traffic-manager-faqs.md#how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile)

* [Как можно назначить заголовки HTTP для проверок работоспособности моих конечных точек диспетчером трафика?](./traffic-manager-faqs.md#how-can-i-assign-http-headers-to-the-traffic-manager-health-checks-to-my-endpoints)

* [Какой заголовок узла используется для проверки работоспособности конечной точки?](./traffic-manager-faqs.md#what-host-header-do-endpoint-health-checks-use)

* [Что такое IP-адресов, используемые при проверке работоспособности?](./traffic-manager-faqs.md#what-are-the-ip-addresses-from-which-the-health-checks-originate)

* [Сколько проверок работоспособности конечной точки выполняет диспетчер трафика?](./traffic-manager-faqs.md#how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager)

* [Каким образом я получу уведомление о том, что одна из моих конечных точек вышла из строя?](./traffic-manager-faqs.md#how-can-i-get-notified-if-one-of-my-endpoints-goes-down)

## <a name="next-steps"></a>Следующие шаги

Узнайте о том, [как работает диспетчер трафика](traffic-manager-how-it-works.md)

Узнайте больше о [методах маршрутизации трафика](traffic-manager-routing-methods.md) , поддерживаемых в диспетчере трафика.

Узнайте, как [создать профиль диспетчера трафика](traffic-manager-manage-profiles.md)

[устранении неполадок с состоянием пониженной функциональности](traffic-manager-troubleshooting-degraded.md) конечной точки диспетчера трафика.