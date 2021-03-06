---
title: Масштабирование и размещение Функций Azure
description: Узнайте, как выбрать между планом потребления и планом "функции Azure".
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.topic: conceptual
ms.date: 08/17/2020
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 490fa46deabc822e416705fe9bf9c5cdb58f8cd6
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936777"
---
# <a name="azure-functions-hosting-options"></a>Варианты размещения функций Azure

При создании приложения-функции в Azure необходимо выбрать план размещения для приложения. Для функций Azure доступны три основных плана размещения: [план потребления](consumption-plan.md), [план Premium](functions-premium-plan.md)и [выделенный план (служба приложений)](dedicated-plan.md). Все планы размещения общедоступны на виртуальных машинах Linux и Windows.

Выбранный план размещения определяет следующие варианты поведения:

* Как масштабируется приложение-функция.
* Ресурсы, доступные для каждого экземпляра приложения функции.
* Поддержка расширенных функций, таких как подключение к виртуальной сети Azure.

В этой статье приводится подробное сравнение различных планов размещения и размещения на основе Kubernetes.

## <a name="overview-of-plans"></a>Обзор планов

Ниже приведена сводка преимуществ трех основных планов размещения для функций.

| | |
| --- | --- |  
|**[План потребления](consumption-plan.md)**| Масштабировать автоматически и платить только за ресурсы вычислений только при выполнении функций.<br/><br/>В плане потребления экземпляры узла функций динамически добавляются и удаляются в зависимости от числа входящих событий.<br/><br/> ✔ План размещения по умолчанию.<br/>✔ Платите только при выполнении функций.<br/>✔ Масштабируется автоматически, даже в периоды высокой нагрузки.|  
|**[План "Премиум"](functions-premium-plan.md)**|Автоматически масштабируется на основе спроса с помощью предварительно подготовленных рабочих ролей, которые запускают приложения без задержки после бездействия, работают на более мощных экземплярах и подключаются к виртуальным сетям. <br/><br/>Рассмотрим план функций Azure уровня "Премиум" в следующих ситуациях. <br/><br/>✔, Что приложения функции работают постоянно или почти постоянно.<br/>✔ У вас большое количество небольших выполнений и высокий счет за использование, но в плане потребления не менее ГБ секунд.<br/>✔ Требуется больше параметров ЦП или памяти, чем предоставлено планом потребления.<br/>✔ Код должен выполняться дольше максимального времени выполнения, разрешенного в плане потребления.<br/>✔ Требуются функции, недоступные в плане потребления, например подключение к виртуальной сети.|  
|**[План ценовой категории "Выделенный"](dedicated-plan.md)** |Выполняйте функции в рамках плана службы приложений по стандартным [тарифам плана службы приложений](https://azure.microsoft.com/pricing/details/app-service/windows/).<br/><br/>Лучше всего подходит для долго выполняющихся сценариев, в которых [устойчивые функции](durable/durable-functions-overview.md) нельзя использовать. Рассмотрим план службы приложений в следующих ситуациях.<br/><br/>✔ Имеются недостаточные виртуальные машины, на которых уже запущены другие экземпляры службы приложений.<br/>✔ Требуется предоставить пользовательский образ для выполнения функций. <br/>✔ Требуется прогнозируемое масштабирование и затраты.|  

Таблицы сравнения в этой статье также включают следующие варианты размещения, которые обеспечивают максимальный уровень контроля и изоляции для выполнения приложений функций.  

| | |
| --- | --- |  
|**[ASE](dedicated-plan.md)** | Среда службы приложений (ASE) — это функция службы приложений, которая предоставляет полностью изолированную и выделенную среду для безопасного запуска приложений службы приложений в высоком масштабе.<br/><br/>Среды ASE подходят для рабочих нагрузок приложений, которым требуется: <br/><br/>✔ Очень высокая масштабируемость.<br/>✔ Полная изоляция вычислений и безопасный доступ к сети.<br/>✔ Высокой интенсивности использования памяти.|  
| **[Kubernetes](functions-kubernetes-keda.md)** | Kubernetes предоставляет полностью изолированную и выделенную среду на основе платформы Kubernetes.<br/><br/> Kubernetes подходит для рабочих нагрузок приложений, которым требуется: <br/>✔ Настраиваемые требования к оборудованию.<br/>✔ Изоляцию и безопасный доступ к сети.<br/>✔ Возможность запуска в гибридной или многооблачной среде.<br/>✔ Работать вместе с существующими приложениями и службами Kubernetes.|  

Остальные таблицы в этой статье сравнивают планы различных функций и поведения. Сравнение затрат между динамическими планами размещения (потребление и Premium) см. на [странице цен на функции Azure](https://azure.microsoft.com/pricing/details/functions/). Цены на различные параметры выделенного плана см. на [странице цен на службу приложений](https://azure.microsoft.com/pricing/details/app-service/windows/). 

## <a name="operating-systemruntime"></a>Операционная система или среда выполнения

В следующей таблице показана поддерживаемая поддержка операционной системы и языка среды выполнения для планов размещения.

| | Linux<sup>1</sup><br/>Только код | Windows<sup>2</sup><br/>Только код | Linux<sup>1, 3</sup><br/>Контейнер Docker |
| --- | --- | --- | --- |
| **[План потребления](consumption-plan.md)** | .NET Core<br/>Node.js<br/>Java<br/>Python | .NET Core<br/>Node.js<br/>Java<br/>PowerShell Core | Не поддерживается  |
| **[План "Премиум"](functions-premium-plan.md)** | .NET Core<br/>Node.js<br/>Java<br/>Python|.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core<br/>Python  | 
| **[План ценовой категории "Выделенный"](dedicated-plan.md)** | .NET Core<br/>Node.js<br/>Java<br/>Python|.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core<br/>Python |
| **[ASE](dedicated-plan.md)** | .NET Core<br/>Node.js<br/>Java<br/>Python |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core  |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core<br/>Python | 
| **[Kubernetes](functions-kubernetes-keda.md)** | Недоступно | Недоступно |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core<br/>Python |

<sup>1</sup> Linux является единственной поддерживаемой операционной системой для стека среды выполнения Python. <br/>
<sup>2</sup> операционная система Windows является единственной поддерживаемой операционной системой для стека среды выполнения PowerShell.<br/>
<sup>3</sup> Linux — единственная поддерживаемая операционная система для контейнеров DOCKER.<br/>

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="scale"></a>Масштабирование

В следующей таблице сравниваются режимы масштабирования различных планов размещения.

| | Горизонтальное увеличение масштаба | Максимальное число экземпляров |
| --- | --- | --- |
| **[План потребления](consumption-plan.md)** | [Управляется событиями](event-driven-scaling.md). Масштабирование выполняется автоматически, даже во время периодов высокой нагрузки. Инфраструктура функций Azure масштабирует ресурсы ЦП и памяти, добавляя дополнительные экземпляры узла функций в зависимости от числа входящих событий триггера. | 200 |
| **[План "Премиум"](functions-premium-plan.md)** | [Управляется событиями](event-driven-scaling.md). Масштабирование выполняется автоматически, даже во время периодов высокой нагрузки. Инфраструктура функций Azure масштабирует ресурсы ЦП и памяти, добавляя дополнительные экземпляры узла функций в зависимости от числа событий, на которые запускаются ее функции. |100|
| **[Выделенный план](dedicated-plan.md)**<sup>1</sup> | Ручная или автомасштабирование |10-20|
| **[ASE](dedicated-plan.md)**<sup>1</sup> | Ручная или автомасштабирование |100 |
| **[Kubernetes](functions-kubernetes-keda.md)**  | Автоматическое масштабирование на основе событий для кластеров Kubernetes с помощью [Кеда](https://keda.sh). | Зависит &nbsp; от &nbsp; кластера&nbsp;&nbsp;|

<sup>1</sup> для определенных ограничений для различных параметров плана службы приложений см. раздел [ограничения плана службы приложений](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits).

## <a name="cold-start-behavior"></a>Поведение холодного запуска

|    |    | 
| -- | -- |
| **[&nbsp;План потребления](consumption-plan.md)** | Приложения могут масштабироваться до нуля при простое, что означает, что некоторые запросы могут иметь дополнительную задержку при запуске.  План потребления имеет некоторые оптимизации для сокращения времени холодного запуска, включая получение от предварительно занятых функций-заполнителей, в которых уже запущены узлы функций и языковые процессы. |
| **[План "Премиум"](functions-premium-plan.md)** | Бессрочные экземпляры, чтобы избежать холодного запуска. |
| **[План ценовой категории "Выделенный"](dedicated-plan.md)** | При выполнении в выделенном плане узел функций может работать непрерывно, что означает, что холодный запуск на самом деле не является проблемой. |
| **[ASE](dedicated-plan.md)** | При выполнении в выделенном плане узел функций может работать непрерывно, что означает, что холодный запуск на самом деле не является проблемой. |
| **[Kubernetes](functions-kubernetes-keda.md)**  | В зависимости от конфигурации Кеда приложения можно настроить так, чтобы избежать холодного запуска. Если настроено масштабирование до нуля, то для новых событий может возникнуть холодный запуск. 

## <a name="service-limits"></a>Ограничения службы

[!INCLUDE [functions-limits](../../includes/functions-limits.md)]

## <a name="networking-features"></a>Функции сетей

[!INCLUDE [functions-networking-features](../../includes/functions-networking-features.md)]

## <a name="billing"></a>Выставление счетов

| | | 
| --- | --- |
| **[План потребления](consumption-plan.md)** | Платите только за время выполнения функций. Плата начисляется на основе числа операций выполнения, времени выполнения и объема использованной памяти. |
| **[План "Премиум"](functions-premium-plan.md)** | План Premium основан на количестве основных секунд и памяти, используемых в необходимых и предварительно занятых экземплярах. По крайней мере, один экземпляр на план должен постоянно оставаться в состоянии "тепло". Этот план обеспечивает наиболее прогнозируемые цены. |
| **[Выделенный план](dedicated-plan.md)* | Вы оплачиваете те же функции для приложений-функций в плане службы приложений, что и для других ресурсов службы приложений, таких как веб-приложения.|
| **[Среда службы приложений (ASE).](dedicated-plan.md)** | Существует фиксированная ежемесячная ставка для ASE, которая оплачивается для инфраструктуры и не изменяется размером ASE. Кроме того, на план службы приложений виртуальных ЦП. Все приложения, размещенные в среде ASE, относятся к ценовой категории (SKU) Isolated. |
| **[Kubernetes](functions-kubernetes-keda.md)**| Вы оплачиваете только расходы кластера Kubernetes; Дополнительная оплата за функции не взимается. Приложение-функция выполняется как Рабочая нагрузка приложения на основе кластера, как и обычное приложение. |

## <a name="next-steps"></a>Дальнейшие действия

+ [Технологии развертывания в функциях Azure](functions-deployment-technologies.md) 
+ [Руководство разработчиков Функций Azure](functions-reference.md)