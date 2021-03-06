---
title: 'Azure ExpressRoute: асимметричная маршрутизация'
description: В этой статье рассматриваются проблемы, которые могут возникнуть при асимметричной маршрутизации в сети, когда есть несколько подключений к целевому расположению.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: 0713c52c7f07db93d9ae9084062ef2c3b25a9074
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/16/2020
ms.locfileid: "97560514"
---
# <a name="asymmetric-routing-with-multiple-network-paths"></a>Асимметричная маршрутизация с использованием нескольких сетевых путей
В этой статье объясняется, как сетевой трафик может принимать разные пути, если между сетевым источником и назначением доступно несколько маршрутов.

Есть два понятия, которые необходимо знать, чтобы понять асимметричную маршрутизацию. Первый — это воздействие нескольких сетевых путей. Другой — это то, как устройства, например, сохраняют состояние брандмауэра. Устройства такого типа называются устройствами с отслеживанием состояния. При объединении этих двух факторов можно создать сценарий, в котором сетевой трафик будет удален устройством с отслеживанием состояния.  Трафик отбрасывается, так как он не обнаружил, что трафик был получен из самого себя.

## <a name="multiple-network-paths"></a>Несколько сетевых путей
Если корпоративная сеть имеет только одну ссылку на Интернет через поставщика услуг Интернета, весь входящий и исходящий трафик Интернета проходит по одному и тому же пути. Обычно компании приобретают несколько цепей для создания избыточных путей, чтобы повысить бесперебойную работу сети. Такой тип конфигурации позволяет трафику исключать одну ссылку на Интернет и возвращать по другой ссылке. Этот сценарий обычно называют асимметричной маршрутизацией. В случае асимметричной маршрутизации возвращаемый сетевой трафик принимает другой путь от исходного потока.

![Сеть с несколькими путями](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Хотя асимметричная маршрутизация обычно происходит при переходе в Интернет. Это также происходит при введении сочетания нескольких путей. Первый пример — при наличии Интернет-пути и частного пути, который переходит к тому же назначению. Второй пример — при наличии нескольких закрытых путей, которые также будут находиться в одном месте назначения.

Каждый маршрутизатор вдоль пути между источником и назначением будет вычислять лучший путь для достижения места назначения. Маршрутизатор определяет наилучший путь на основе двух основных факторов:

* Маршрутизация между внешними сетями реализуется с помощью протокола маршрутизации BGP. BGP принимает объявления от соседей и запускает их в несколько этапов, чтобы определить оптимальный путь к целевому расположению. Оптимальный путь сохраняется в таблице маршрутизации.
* На пути маршрутизации влияет длина маски подсети, связанной с маршрутом. Если маршрутизатор получает несколько объявлений для одного и того же IP-адреса, маршрутизатор выберет путь с более длинной маской подсети, так как он считается более конкретным маршрутом.

## <a name="stateful-devices"></a>Устройства с отслеживанием состояния
Маршрутизаторы анализируют IP-заголовок пакета в рамках задач маршрутизации. Некоторые устройства анализируют пакет более подробно. Как правило, эти устройства выглядят на уровне 4 — протокол управления передачей (TCP) или UDP, или даже заголовки уровня 7 (уровень приложения). Это либо устройства для обеспечения безопасности, либо устройства для оптимизации пропускной способности. 

Распространенным примером устройства с отслеживанием состояния является брандмауэр. Брандмауэр разрешает или отклоняет пакеты для передачи через его интерфейсы на основе различных критериев. Эти критерии включают, но не ограничиваются протоколом, портом TCP/UDP и заголовками URL-адресов. Этот уровень проверки пакетов может положить интенсивную нагрузку на устройство. 

Для повышения производительности брандмауэр проверяет первый пакет из потока. Если он позволяет пакету проходить через свои интерфейсы, он сохраняет информацию о потоке в своей таблице состояний. Все исходящие пакеты, связанные с этим потоком, затем разрешаются в зависимости от первоначального определения. Пакет, который является частью существующего потока, может поступать в брандмауэр, от которого он не был получен. Так как она не имеет предыдущих сведений о состоянии первоначального потока, брандмауэр удаляет пакет.

## <a name="asymmetric-routing-with-expressroute"></a>Асимметричная маршрутизация с помощью ExpressRoute
При подключении к Майкрософт через Azure ExpressRoute в сеть вносятся следующие изменения.

* Существует несколько подключений к Майкрософт. Одним из ссылок является существующее подключение к Интернету, а другое — через подключение ExpressRoute. Определенный трафик, предназначенный для корпорации Майкрософт, может проходить через Интернет-подключение, но возвращать подключение ExpressRoute. Это также может произойти, если трафик проходит через ExpressRoute, но возвращается по пути Интернета.
* Вы получили более конкретные IP-адреса из канала ExpressRoute. Таким образом, когда трафик из сети передается в корпорацию Майкрософт для служб, предлагаемых через ExpressRoute, маршрутизаторы всегда предпочитают подключение ExpressRoute.

Чтобы понять, как эти два изменения повлияли на сеть, рассмотрим некоторые сценарии. Например, у вас есть канал в Интернете и все службы Майкрософт используются через Интернет. Трафик из сети в корпорацию Майкрософт проходит по одному и тому же каналу Интернета и проходит через брандмауэр. Брандмауэр записывает поток, когда видит первый пакет. Все принимаемые пакеты этого диалога разрешены, так как поток существует в таблице состояний.

![Асимметричная маршрутизация с помощью ExpressRoute](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)

Затем вы выводите канал ExpressRoute для использования служб, предлагаемых корпорацией Майкрософт через ExpressRoute. Все остальные службы корпорации Майкрософт используются через Интернет. Вы развертываете отдельный брандмауэр на границе, подключенном к подключению ExpressRoute. Корпорация Майкрософт объявляет более конкретные префиксы сети через ExpressRoute для определенных служб. Инфраструктура маршрутизации выберет ExpressRoute в качестве предпочтительного пути для этих префиксов. 

Если вы не объявите общедоступные IP-адреса в корпорацию Майкрософт через ExpressRoute. Корпорация Майкрософт будет обмениваться данными с общедоступными IP-адресами через Интернет. Трафик, отправляемый из сети в корпорацию Майкрософт, использует подключение ExpressRoute, но возвращаемый трафик от Майкрософт будет использовать путь в Интернете. Когда брандмауэр на вашем крае видит ответный пакет для потока, о котором он не знает, это приведет к удалению этих пакетов.

Если вы решили объявить один и тот же пул преобразования сетевых адресов (NAT) для ExpressRoute и для Интернета. Вы увидите похожие проблемы с клиентами в вашей сети на частных IP-адресах. Запросы к службам, например Центр обновления Windows, будут проходить через Интернет, так как IP-адреса для этих служб не объявляются через ExpressRoute. Однако возвращаемый трафик будет возвращаться через ExpressRoute. Так как корпорация Майкрософт получила IP-адрес с той же маской подсети из Интернета и ExpressRoute, предпочтительным путем всегда является ExpressRoute. Если брандмауэр или другое устройство с отслеживанием состояния в сети, к которому подключено подключение ExpressRoute, не имеет предыдущих сведений о потоке, то эти пакеты будут удалены.

## <a name="asymmetric-routing-solutions"></a>Решения при асимметричной маршрутизации
Существует два варианта решения проблемы асимметричной маршрутизации. Первый — через маршрутизацию, второй — с помощью NAT на основе источника (SNAT).

### <a name="routing"></a>Маршрутизация
Убедитесь, что общедоступные IP-адреса объявлены в соответствующих связях глобальной сети (WAN). Например, если вы хотите использовать Интернет для трафика проверки подлинности и ExpressRoute для почтового трафика. Не объявляйте общедоступные IP-адреса службы федерации Active Directory (AD FS) (AD FS) через ExpressRoute. Кроме того, не предоставляйте локальный AD FS сервер IP-адресам, которые маршрутизатор получает через ExpressRoute. Маршруты, полученные через ExpressRoute, являются более определенными, поэтому ExpressRoute будет предпочтительным путем для трафика проверки подлинности, направляемого в Майкрософт. Если вы не можете обратить внимание на то, как происходит маршрутизация в сети, могут возникнуть проблемы с асимметричной маршрутизацией.

Если вы хотите использовать ExpressRoute для проверки подлинности, убедитесь, что вы объявили AD FS общедоступные IP-адреса через ExpressRoute без NAT. При такой настройке трафик, поступающий от корпорации Майкрософт, переходит на локальный сервер AD FS, который переходит по ExpressRoute. Трафик, возвращаемый из сети в корпорацию Майкрософт, будет использовать ExpressRoute, так как это предпочтительный маршрут через Интернет.

### <a name="source-based-nat"></a>NAT на основе источника
Другим способом решения проблемы с асимметричной маршрутизацией является использование SNAT. Например, вы решили не объявлять общедоступный IP-адрес локального сервера SMTP через ExpressRoute. Вместо этого вы планируете использовать Интернет для такого типа обмена данными. Запрос, исходящий от корпорации Майкрософт и переходящий на локальный SMTP-сервер, проходит через Интернет. С помощью SNAT входящий запрос перенаправляется на внутренний IP-адрес. Трафик, возвращаемый с SMTP-сервера, передается на пограничные брандмауэр (который используется для NAT), а не через ExpressRoute. В результате возвращаемый трафик будет принимать путь в Интернете.

![Конфигурация сети во время SNAT](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="asymmetric-routing-detection"></a>Обнаружение асимметричной маршрутизации
Лучшим подтверждением того, что сетевой трафик проходит по ожидаемому пути, является трассировка маршрута. Если вы ожидаете передачи трафика с локального SMTP-сервера в корпорацию Майкрософт для получения пути в Интернете, ожидаемый Traceroute — от SMTP-сервера до Microsoft 365. Результат подтверждает, что трафик действительно выходит из сети в Интернет, а не в ExpressRoute.

