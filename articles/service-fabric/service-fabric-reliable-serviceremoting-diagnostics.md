---
title: Диагностика и мониторинг Azure ServiceFabric
description: В этой статье описаны функции мониторинга производительности в среде выполнения Service Fabric Reliable ServiceRemoting, такие как генерируемые ею счетчики производительности.
author: suchiagicha
ms.topic: conceptual
ms.date: 06/29/2017
ms.author: pepogors
ms.openlocfilehash: 9c7d466d6e8fd36b4445966b92ee753becf96c64
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2021
ms.locfileid: "98791767"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Диагностика и мониторинг производительности в модели Reliable Service Remoting
Среда выполнения Reliable ServiceRemoting генерирует [счетчики производительности](/dotnet/api/system.diagnostics.performancecounter). Они содержат полезную информацию о работоспособности ServiceRemoting и помогают устранять неполадки и контролировать производительность.


## <a name="performance-counters"></a>Счетчики производительности
Среда выполнения Reliable ServiceRemoting определяет указанные ниже категории счетчиков производительности.

| Категория | Описание |
| --- | --- |
| Service Fabric Service |Счетчики, относящиеся к Azure Service Fabric Service Remoting, например счетчик среднего времени, затраченного на обработку запроса. |
| Service Fabric Service Method |Счетчики, относящиеся к методам, которые реализуются Service Fabric Remoting Service, например выявляющие частоту вызова того или иного метода службы. |

В каждой категории предусмотрен как минимум один счетчик.

Для сбора и просмотра данных счетчиков производительности можно использовать [системный монитор Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749249(v=ws.11)) , доступный по умолчанию в операционной системе Windows. [Диагностика Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) , в свою очередь, позволяет не только собирать данные счетчиков производительности, но и загружать их в таблицы Azure.

### <a name="performance-counter-instance-names"></a>Имена экземпляров счетчиков производительности
В кластере, содержащем большое количество служб или секций ServiceRemoting, находится большое количество экземпляров счетчиков производительности. Имя экземпляра счетчика производительности помогает определить конкретную секцию и метод службы (если применимо), с которыми связан этот экземпляр.

#### <a name="service-fabric-service-category"></a>Категория Service Fabric Service
Экземпляры счетчиков в категории `Service Fabric Service`имеют имена в следующем формате:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* — это строковое представление идентификатора секции Service Fabric, с которым связан экземпляр счетчика производительности. ИДЕНТИФИКАТОРом секции является GUID, и его строковое представление создается с помощью [`Guid.ToString`](/dotnet/api/system.guid.tostring#System_Guid_ToString_System_String_) метода с описателем формата "D".

*ServiceReplicaOrInstanceId* — это строковое представление идентификатора реплики или экземпляра Service Fabric, c которой связан экземпляр счетчика производительности.

*ServiceRuntimeInternalID* — это строковое представление 64-разрядного целого числа. Оно создается средой выполнения Fabric Service для внутреннего использования. В имя экземпляра счетчика производительности оно включается для того, чтобы обеспечить его уникальность и избежать конфликтов с именами других экземпляров счетчиков производительности. Пользователям не следует пытаться интерпретировать эту часть имени экземпляра счетчика производительности.

Пример имени экземпляра счетчика, относящегося к категории `Service Fabric Service` :

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

В предыдущем примере `2740af29-78aa-44bc-a20b-7e60fb783264` — это строковое представление идентификатора секции Service Fabric, `635650083799324046` — строковое представление идентификатора реплики или экземпляра, а `5008379932`— 64-разрядный идентификатор, создаваемый средой выполнения для внутреннего использования.

#### <a name="service-fabric-service-method-category"></a>Категория Service Fabric Service Method
Экземпляры счетчиков в категории `Service Fabric Service Method`имеют имена в следующем формате:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* — это имя метода службы, с которым связан экземпляр счетчика производительности. Формат имени метода определяется на основе логики в среде выполнения Fabric Service, которая органично сочетает удобочитаемость с ограничениями Windows на максимальную длину имен экземпляров счетчиков производительности.

*ServiceRuntimeMethodId* — это строковое представление 32-разрядного целого числа. Оно создается средой выполнения Fabric Service для внутреннего использования. В имя экземпляра счетчика производительности оно включается для того, чтобы обеспечить его уникальность и избежать конфликтов с именами других экземпляров счетчиков производительности. Пользователям не следует пытаться интерпретировать эту часть имени экземпляра счетчика производительности.

*ServiceFabricPartitionID* — это строковое представление идентификатора секции Service Fabric, с которым связан экземпляр счетчика производительности. ИДЕНТИФИКАТОРом секции является GUID, и его строковое представление создается с помощью [`Guid.ToString`](/dotnet/api/system.guid.tostring#System_Guid_ToString_System_String_) метода с описателем формата "D".

*ServiceReplicaOrInstanceId* — это строковое представление идентификатора реплики или экземпляра Service Fabric, c которой связан экземпляр счетчика производительности.

*ServiceRuntimeInternalID* — это строковое представление 64-разрядного целого числа. Оно создается средой выполнения Fabric Service для внутреннего использования. В имя экземпляра счетчика производительности оно включается для того, чтобы обеспечить его уникальность и избежать конфликтов с именами других экземпляров счетчиков производительности. Пользователям не следует пытаться интерпретировать эту часть имени экземпляра счетчика производительности.

Пример имени экземпляра счетчика, относящегося к категории `Service Fabric Service Method` :

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

В предыдущем примере `ivoicemailboxservice.leavemessageasync` — это имя метода, `2` — 32-разрядный идентификатор, создаваемый для внутреннего использования средой выполнения, `89383d32-e57e-4a9b-a6ad-57c6792aa521` — строковое представление идентификатора секции Service Fabric, `635650083804480486` — строковое представление идентификатора реплики или экземпляра Service Fabric, а `5008380` — 64-разрядный идентификатор, создаваемый для внутреннего использования средой выполнения.

## <a name="list-of-performance-counters"></a>Список счетчиков производительности
### <a name="service-method-performance-counters"></a>Счетчики производительности метода службы

Среда выполнения Reliable Service публикует следующие счетчики производительности, связанные с выполнением методов службы.

| Имя категории | Имя счетчика | Описание |
| --- | --- | --- |
| Service Fabric Service Method |Вызовов в секунду |Количество вызовов метода службы в секунду |
| Service Fabric Service Method |Среднее время вызова (мс) |Время, затраченное на выполнение метода службы, в миллисекундах |
| Service Fabric Service Method |Исключений в секунду |Количество исключений, порожденных методом службы, в секунду |

### <a name="service-request-processing-performance-counters"></a>Счетчики производительности обработки запросов службы
Когда клиент вызывает метод через объект прокси службы, это приводит к отправке по сети сообщения запроса в Remoting Service. Служба обрабатывает сообщение запроса и отправляет ответ обратно клиенту. Среда выполнения Reliable ServiceRemoting публикует следующие счетчики производительности, связанные с обработкой запросов службы.

| Имя категории | Имя счетчика | Описание |
| --- | --- | --- |
| Service Fabric Service |Число невыполненных запросов |Число запросов, обрабатываемых в службе |
| Service Fabric Service |Среднее время запроса (мс) |Время обработки запроса службой (мс) |
| Service Fabric Service |Среднее время десериализации запроса (мс) |Время десериализации сообщения запроса службы при получении его службой (мс) |
| Service Fabric Service |Среднее время сериализации ответа (мс) |Время сериализации ответного сообщения службы в службе до отправки ответа клиенту (мс) |

## <a name="next-steps"></a>Дальнейшие действия
* [Образец кода](https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0)
* [Поставщики EventSource в PerfView](/archive/blogs/vancem/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource)
