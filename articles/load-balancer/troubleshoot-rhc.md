---
title: Устранение неполадок Azure Load Balancer работоспособности ресурсов, внешних и внутренних проблем доступности
description: Используйте доступные метрики для диагностики Load Balancer (цен. категория "Стандартный") Azure с пониженной производительностью или недоступности.
services: load-balancer
documentationcenter: na
author: erichrt
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2020
ms.author: errobin
ms.openlocfilehash: 3acaaba86c9a546a0bd45b5386287908168d50d0
ms.sourcegitcommit: 19ffdad48bc4caca8f93c3b067d1cf29234fef47
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97955626"
---
# <a name="troubleshoot-resource-health-and-inbound-availability-issues"></a>Устранение неполадок работоспособности ресурсов и входящих проблем доступности 

Эта статья представляет собой руководство по исследованию проблем, влияющих на доступность внешнего IP-адреса и серверных ресурсов балансировщика нагрузки. 

Проверка Работоспособность ресурсов (RHC) для Load Balancer используется для определения работоспособности балансировщика нагрузки. Он анализирует метрику доступности пути данных в течение **2-минутного** интервала, чтобы определить, доступны ли конечные точки балансировки нагрузки, а также сочетания IP-интерфейсов и интерфейсных портов с правилами балансировки нагрузки.

В приведенной ниже таблице описывается логика RHC, используемая для определения состояния работоспособности балансировщика нагрузки.

| Состояние работоспособности ресурсов | Описание |
| --- | --- |
| Доступно | Ресурс "Стандартный балансировщик нагрузки" исправен и доступен. |
| Деградация | В вашей стандартной подсистеме балансировки нагрузки события, инициируемые платформой или пользователем, влияют на производительность. Метрика доступности пути к данным сообщила, что состояние работоспособности в течение как минимум двух минут ниже 90 %, но больше 25 %. Вы получите умеренное воздействие на производительность. 
| Рекомендации недоступны | Ресурс "Стандартный балансировщик нагрузки" неисправен. Метрика доступности пути к данным сообщила о снижении уровня работоспособности 25% по крайней мере на две минуты. Вы получите значительное влияние на производительность или недостаток доступности для входящего подключения. Возможно, события пользователя или платформы вызывают недоступность. |
| Неизвестно | Состояние работоспособности ресурсов для ресурса балансировщика нагрузки уровня "Стандартный" еще не обновлено или не получило сведений о доступности пути данных за последние 10 минут. Это временное состояние. Правильное состояние будет отображено сразу после получения данных. |


## <a name="about-the-metrics-well-use"></a>О метриках, которые мы будем использовать
Используются две метрики: *доступность пути к данным* и *состояние проверки работоспособности* . Важно понимать их смысл, чтобы получить правильные аналитические сведения. 

## <a name="data-path-availability"></a>Доступность пути к данным
Метрика доступности пути к данным создается с помощью TCP-проверки связи каждые 25 секунд на всех интерфейсных портах, для которых настроены правила балансировки нагрузки и входящего трафика NAT. Затем эта проверка связи TCP будет направляться на любой работоспособный (проверенный) серверный экземпляр. Если служба получает ответ на запрос проверки связи, она считается успешной и сумма метрики будет повторяться один раз, если она не будет выполнена. Счетчик этой метрики составляет 1/100 общих TCP-проверок связи за период выборки. Поэтому мы хотим рассмотреть среднее значение, в котором будет показана средняя сумма/количество за период времени. Метрика доступности пути к данным, агрегированная по среднему, позволяет получить процент успешных попыток TCP-проверок на интерфейсных IP: порт для каждого правила балансировки нагрузки и входящего трафика NAT.

## <a name="health-probe-status"></a>Состояние проверки работоспособности
Метрика состояния проверки работоспособности формируется командой ping для протокола, определенного в зонде работоспособности. Эта проверка связи отправляется каждому экземпляру во внутреннем пуле и порту, определенным в зонде работоспособности. Для проверок HTTP и HTTPS успешная проверка связи требует наличия ответа HTTP 200 OK, в то время как при использовании проверки TCP любой ответ считается успешным. Последовательные успешные или неудачные попытки каждой проверки определяют, является ли внутренний экземпляр работоспособным и может принимать трафик для правил балансировки нагрузки, которым назначен внутренний пул. Аналогично доступности пути к данным мы используем среднее агрегирование, которое указывает среднее количество успешных и общих проверок связи в течение интервала выборки. Это значение состояния пробы работоспособности обозначает работоспособность серверной части в изоляции от балансировщика нагрузки путем проверки серверных экземпляров без отправки трафика через интерфейс.

>[!IMPORTANT]
>Состояние пробы работоспособности выдается на основе одной минуты. Это может привести к незначительным колебаниям в постоянном значении в противном случае. Например, если имеется два экземпляра серверной части, одна из которых проверена и одна проверена, служба проверки работоспособности может собрать 7 образцов для работоспособного экземпляра и 6 для неработоспособного экземпляра. Это приведет к появлению ранее стабильного значения 50, отображаемого в виде 46,15 в течение одного минуты. 

## <a name="diagnose-degraded-and-unavailable-load-balancers"></a>Диагностика неработоспособных и недоступных подсистем балансировки нагрузки
Как описано в [статье о работоспособности ресурсов](load-balancer-standard-diagnostics.md#resource-health-status), подсистема балансировки нагрузки с пониженной производительностью — это одна из следующих: от 25% до 90% доступности пути к данным, а недоступная подсистема балансировки нагрузки — это один с менее чем на 25% доступности пути к данным в течение двух минут. Эти же действия можно выполнить, чтобы исследовать ошибку, которая отображается в состоянии проверки работоспособности или оповещения о доступности пути к данным, которые вы настроили. Мы рассмотрим случай, когда мы проверили работоспособность ресурсов и обнаружили, что подсистема балансировки нагрузки недоступна с доступностью данных в 0% — Наша служба не работает.

Сначала мы перейдем к представлению подробной информации о метриках в колонке "аналитика балансировщика нагрузки". Это можно сделать через колонку ресурсов балансировщика нагрузки или ссылку в сообщении о работоспособности ресурса.  Затем перейдите на вкладку доступность внешнего интерфейса и серверной части и просмотрите 30-минутное окно периода времени, когда произошло пониженное или недоступное состояние. Если мы видим, что доступ к данным по пути данных был равен 0%, мы понимаем, что проблема заключается в предотвращении трафика для всех правил балансировки нагрузки и входящего трафика NAT и может увидеть, как долго это воздействие последней. 

Далее нужно найти метрику состояния проверки работоспособности, чтобы определить, недоступен ли наш путь к данным, так как у нас нет работоспособных серверных экземпляров для обслуживания трафика. Если у нас есть по крайней мере один работоспособный серверный экземпляр для всех правил балансировки нагрузки и входящего трафика, мы понимаем, что это не наша конфигурация, из-за которой наши пути к данным недоступны. Этот сценарий указывает на проблему с платформой Azure, и, в редких случаях, не волнуйтесь, если вы нашли их в качестве автоматического оповещения, отправляемого нашей команде для быстрого устранения всех проблем с платформой.

## <a name="diagnose-health-probe-failures"></a>Диагностика сбоев проверки работоспособности
Предположим, что мы проверяем состояние проверки работоспособности и обнаружили, что все экземпляры отображаются как неработоспособные. В этом поиске объясняется, почему наш путь к данным недоступен, так как трафик вообще не найдется. Затем следует просмотреть следующий контрольный список, чтобы исправила ошибки конфигурации:
* Проверьте использование ЦП для ресурсов, чтобы проверить, действительно ли ваши экземпляры работоспособны
  * Это можно проверить 
* При использовании проверки HTTP или HTTPS, если приложение работоспособно и отвечает
  * Проверка работоспособности осуществляется путем прямого доступа к приложениям через частный IP-адрес или общедоступный IP-адрес уровня экземпляра, связанный с экземпляром серверной части.
* Ознакомьтесь с группами безопасности сети, применяемыми к нашим внутренним ресурсам. Убедитесь, что правила с более высоким приоритетом отсутствуют, чем Алловазурелоадбаланцеринбаунд, которые блокируют проверку работоспособности.
  * Это можно сделать, перейдя в колонку сети серверных виртуальных машин или масштабируемых наборов виртуальных машин.
  * Если вы обнаружите эту ошибку NSG, переместите существующее правило разрешающего или создайте новое правило с высоким приоритетом, чтобы разрешить AzureLoadBalancer трафик.
* Проверьте вашу ОС. Убедитесь, что виртуальные машины прослушивают порт пробы и просматривают правила брандмауэра ОС, чтобы убедиться, что они не блокируют трафик пробы, исходящий с IP-адреса 168.63.129.16
  * Вы можете проверить порты прослушивания, выполнив команду netstat-a командной строки Windows или netstat-l в терминале Linux.
* Не размещайте виртуальную машину брандмауэра NVA во внутреннем пуле балансировщика нагрузки, используйте [определяемые пользователем маршруты](../virtual-network/virtual-networks-udr-overview.md#user-defined) для маршрутизации трафика к серверным экземплярам через брандмауэр.
* Убедитесь, что вы используете правильный протокол, если вы используете HTTP для проверки порта, ожидающего приложения, отличного от HTTP, проверка завершится ошибкой.

Если вы проверили этот контрольный список и намерены обнаружить сбои проверки работоспособности, возможны проблемы с платформой, влияющие на службу проб для ваших экземпляров. В этом случае для быстрого устранения всех проблем с платформой в нашей группе будет отправлено автоматическое оповещение Azure.

## <a name="next-steps"></a>Дальнейшие действия

* [Дополнительные сведения о пробе работоспособности Azure Load Balancer](load-balancer-custom-probe-overview.md)
* [Дополнительные сведения о метриках Azure Load Balancer](load-balancer-standard-diagnostics.md)
