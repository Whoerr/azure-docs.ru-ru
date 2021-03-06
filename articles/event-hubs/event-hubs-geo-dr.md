---
title: Геоизбыточное аварийное восстановление — Центры событий Azure | Документация Майкрософт
description: Как использовать географические регионы для отработки отказа и аварийного восстановления в Центрах событий Azure.
ms.topic: article
ms.date: 02/10/2021
ms.openlocfilehash: 2fd13ac98e80aa67a2a3150e8406a0b0b1b08d13
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100390680"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Геоизбыточное аварийное восстановление в службе "Центры событий Azure" 

Устойчивость к неустойчивым простоям ресурсов обработки данных является обязательным требованием для многих предприятий и в некоторых случаях даже для отраслевых нормативов. 

Концентраторы событий Azure уже разбросают риск разрушительных сбоев отдельных компьютеров или даже полных стоек в кластерах, охватывающих несколько доменов сбоя в центре обработки данных, и реализуют прозрачное обнаружение сбоев и механизмы отработки отказа таким способом, чтобы служба продолжала работать на уровне обслуживания и обычно без заметных перерывов в работе таких сбоев. Если пространство имен концентраторов событий было создано с параметром включено для [зон доступности](../availability-zones/az-overview.md), то риск сбоя далее распределяется по трем физически разделенным средствам, а служба имеет достаточно резервных копий, чтобы быстро справиться с полной, катастрофической потерей всей ситуации. 

Модель кластера концентраторов событий Azure с поддержкой зон доступности обеспечивает устойчивость к сбоям оборудования и даже разрушительную утрату всех средств центра обработки данных. Тем не менее могут возникнуть случаи с грависом при значительном физическом уничтожении, и даже эти меры не могут быть достаточными для защиты. 

Функция географического аварийного восстановления концентраторов событий призвана упростить восстановление после аварии и отказаться от неисправного региона Azure и без необходимости изменять конфигурации приложений. Прекращение использования региона Azure обычно влечет за собой несколько служб, и эта функция в основном нацелена на сохранение целостности конфигурации составного приложения.  

Функция восстановления Geo-Disaster гарантирует, что вся конфигурация пространства имен (концентраторы событий, группы потребителей и параметры) постоянно реплицируется из основного пространства имен в дополнительное пространство имен, когда они сопоставлены, и позволяет инициировать отработку отказа только один раз из основной базы данных-получателя в любое время. При перемещении отработки отказа выбранное имя псевдонима для пространства имен будет повторно назначено дополнительному пространству имен, а затем нарушено связывание. Отработка отказа почти мгновенно инициируется. 

> [!IMPORTANT]
> Эта функция позволяет мгновенно непрерывно выполнять операции с той же конфигурацией, но не **реплицирует данные событий**. Если сбой привел к утрате всех зон, данные о событии, сохраняемые в основном концентраторе событий после отработки отказа, будут восстановлены, а исторические события могут быть получены из него после восстановления доступа. Для репликации данных событий и работы соответствующих пространств имен в конфигурациях "активный/активный" с учетом сбоев и аварий, не используйте этот набор функций географического аварийного восстановления, но следуйте [указаниям по репликации](event-hubs-federation-overview.md).  

## <a name="outages-and-disasters"></a>Сбои и аварийные ситуации

Важно отметить различия между "сбоями" и "авариями". **Сбой** — это временная недоступность Центров событий Azure. Он может повлиять на некоторые компоненты службы, такие как хранилище сообщений, или даже весь центр обработки данных. Однако после устранения проблемы Центры событий снова становятся доступны. Как правило, простой не приводит к утрате сообщений или других данных. Примером такого сбоя может быть сбой питания в центре обработки данных. Некоторые сбои являются всего лишь кратковременными потерями подключения из-за временных или сетевых проблем. 

*Авария* определяется как полная или долговременная потеря кластера Центров событий, региона Azure или центра обработки данных. Регион или центр обработки данных может снова стать доступным, но может и не стать или быть отключенным в течение нескольких часов или дней. Примеры аварий — пожар, наводнение или землетрясение. Длительное аварийное состояние может привести к потере некоторых сообщений, событий или других данных. Но в большинстве случаев потеря данных не происходит и сообщения можно восстановить сразу же после резервного копирования в центре обработки данных.

Функция географического аварийного восстановления Центров событий Azure — это решение для аварийного восстановления. Основные понятия и рабочий процесс, описанные в этой статье, относятся к сценариям восстановления после аварий, но не временных сбоев. Дополнительные сведения об аварийном восстановлении в Microsoft Azure см. в статье [Аварийное восстановление для приложений на платформе Azure](/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="basic-concepts-and-terms"></a>Основные понятия и термины

Функция аварийного восстановления реализует аварийное восстановление метаданных и основывается на первичном и вторичном пространстве имен для аварийного восстановления. 

Функция геоизбыточного аварийного восстановления доступна только для [номеров SKU ценовых категорий "Стандартный" и "Выделенный"](https://azure.microsoft.com/pricing/details/event-hubs/). Вам не нужно изменять какие-либо строки подключения, так как подключение выполняется через псевдоним.

В этом руководстве используются термины, представленные ниже.

-  *Псевдоним*. Имя настроенной конфигурации аварийного восстановления. Псевдоним предоставляет единую стабильную строку подключения полного доменного имени (FQDN). Приложения используют ее для подключения к пространству имен. 

-  *Основное или дополнительное пространство имен*. Пространство имен, соответствующее псевдониму. Основное пространство имен является "активным" и получает сообщения (это может быть существующее или новое пространство имен). Дополнительное пространство имен является "пассивным" и не получает сообщений. Метаданные между ними синхронизированы, поэтому они могут беспрепятственно принимать сообщения без каких-либо изменений кода или строки подключения приложения. Чтобы убедиться, что только активное пространство имен получает сообщения, необходимо использовать псевдоним.

    > [!IMPORTANT]
    > Для функции географического аварийного восстановления требуется, чтобы подписка и группа ресурсов совпадали с первичными и дополнительными пространствами имен. 
-  *Метаданные*. Сущности, такие как концентраторы событий и группы объектов-получателей, и их свойства службы, связанные с пространством имен. Только сущности и их параметры реплицируются автоматически. Сообщения и события не реплицируются. 

-  *Отработка отказа*. Процесс активации дополнительного пространства имен.

## <a name="supported-namespace-pairs"></a>Поддерживаемые пары пространств имен
Ниже приведены поддерживаемые сочетания основного и дополнительного пространств имен.  

| Основное пространство имен | Дополнительное пространство имен | Поддерживается | 
| ----------------- | -------------------- | ---------- |
| Standard | Standard | Да | 
| Standard | Выделенные | Да | 
| Выделенные | Выделенные | Да | 
| Выделенные | Standard | Нет | 

> [!NOTE]
> Невозможно связать пространства имен, размещенные в одном выделенном кластере. Можно связать пространства имен, размещенные в разных кластерах. 

## <a name="setup-and-failover-flow"></a>Настройка и поток отработки отказа

Здесь приведен обзор процесса отработки отказа и объясняется, как настроить начальную отработку отказа. 

![1][]

### <a name="setup"></a>Настройка

Сначала создается или используется существующее основное пространство имен и новое дополнительное пространство имен, а затем они связываются. Это связывание предоставит псевдоним, который можно использовать для подключения. Так как используется псевдоним, нет необходимости изменять строки подключения. Только новые пространства имен могут быть добавлены к вашему связыванию. 

1. Создайте основное пространство имен.
1. Создайте дополнительное пространство имен в подписке и группу ресурсов с основным пространством имен, но в другом регионе. Этот шаг является необязательным. Вы можете создать вторичное пространство имен при создании связывания на следующем шаге. 
1. В портал Azure перейдите к основному пространству имен.
1. Выберите **географическое восстановление** в меню слева и выберите **начать связывание** на панели инструментов. 

    :::image type="content" source="./media/event-hubs-geo-dr/primary-namspace-initiate-pairing-button.png" alt-text="Инициация связывания из основного пространства имен":::    
1. На странице **Инициация связывания** выполните следующие действия.
    1. Выберите существующее дополнительное пространство имен или создайте его в подписке, а также в группе ресурсов с основным пространством имен. В этом примере выбрано существующее пространство имен.  
    1. В поле **псевдоним** введите псевдоним для пары "географическое восстановление". 
    1. Затем выберите **Создать**. 

    :::image type="content" source="./media/event-hubs-geo-dr/initiate-pairing-page.png" alt-text="Выбор дополнительного пространства имен":::        
1. Должна отобразиться страница **псевдонима географического аварийного восстановления** . Кроме того, можно переходить к этой странице из основного пространства имен, выбрав **географическое восстановление** в меню слева.

    :::image type="content" source="./media/event-hubs-geo-dr/geo-dr-alias-page.png" alt-text="Страница псевдонима географического аварийного восстановления":::    
1. На странице **псевдоним географического аварийного восстановления** выберите **политики общего доступа** в меню слева для доступа к основной строке подключения для псевдонима. Используйте эту строку соединения вместо строки подключения к первичному/вторичному пространству имен напрямую. 
1. На этой странице **обзора** можно выполнить следующие действия. 
    1. Разбиение пары между основным и дополнительным пространствами имен. На панели инструментов нажмите кнопку **приостановить связывание** . 
    1. Вручную отработка отказа в дополнительное пространство имен. На панели инструментов выберите **отработка отказа** . 
    
        > [!WARNING]
        > При отработки отказа будет активировано дополнительное пространство имен и удалено основное пространство имен из Geo-Disaster связывания с восстановлением. Создайте другое пространство имен, чтобы создать пару географического аварийного восстановления. 

Наконец, следует добавить некоторые функции мониторинга, чтобы определить, необходимо ли выполнять отработку отказа. В большинстве случаев служба — это одна из частей большой экосистемы, поэтому автоматический переход на другой ресурс не всегда возможен, так как отработки отказа следует выполнять синхронно с оставшимися подсистемами или инфраструктурой.

### <a name="example"></a>Пример

Рассмотрим пример сценария с решением точки розничной продажи (POS), которое создает сообщения и события. Центры событий передают эти события решению сопоставления или переформатирования, которое затем переадресовывает сопоставленные данные в другую систему для дальнейшей обработки. В этот момент все эти системы могут размещаться в одном регионе Azure. Решение о том, когда и какая часть отработки отказа зависит от потока данных в вашей инфраструктуре. 

Отработку отказа можно автоматизировать с помощью систем мониторинга или пользовательских решений для мониторинга. Однако такая автоматизация требует дополнительного планирования и работы, что выходит за рамки данной статьи.

### <a name="failover-flow"></a>Поток отработки отказа

Если вы инициируете отработку отказа, необходимо выполнить два шага:

1. В случае другого сбоя необходимо иметь возможность повторной отработки отказа. Поэтому настройте другое пассивное пространство имен и обновите связывание. 

2. Извлеките сообщения из прежнего основного пространства имен, как только оно станет доступным. После этого используйте это пространство имен для регулярного обмена сообщениями вне настройки географического восстановления или удалите старое основное пространство имен.

> [!NOTE]
> Поддерживается только семантика переадресации сбоя. В этом сценарии выполняется отработка отказа, а затем повторное связывание с новым пространством имен. Восстановление размещения не поддерживается, к примеру, в кластере SQL. 

![2][]

## <a name="management"></a>Управление

Если допущена ошибка, например, во время первоначальной настройки связаны неправильные регионы, связь двух пространств имен можно разорвать в любое время. Если вы хотите использовать сопряженные пространства имен в качестве обычных пространств имен, удалите псевдоним.

## <a name="samples"></a>Примеры

В [примере из GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) показано, как настроить и инициировать отработку отказа. В примере демонстрируются следующие понятия:

- параметры, необходимые в Azure Active Directory для использования Azure Resource Manager с Центрами событий; 
- шаги, необходимые для выполнения примера кода; 
- отправка и получение из текущего основного пространства имен; 

## <a name="considerations"></a>Рекомендации

Обратите внимание на следующие моменты, которые следует учитывать:

1. Геоизбыточное аварийное восстановление Центров событий не реплицирует данные, поэтому вы не можете повторно использовать старое значение смещения основного концентратора событий в дополнительном концентраторе событий. Рекомендуется перезапустить приемник событий одним из следующих способов.

   - *EventPosition.FromStart()* — если вы хотите читать все данные в дополнительном концентраторе событий.
   - *EventPosition.FromEnd()* — если вы хотите читать все новые данные с момента подключения к дополнительному концентратору событий.
   - *EventPosition.FromEnqueuedTime(dateTime)* — если вы хотите читать все данные, полученные в дополнительном концентраторе событий, начиная с заданной даты и времени.

2. При планировании отработки отказа следует также учитывать фактор времени. Например, в случае потери подключения на более чем 15–20 минут можно запустить отработку отказа. 
 
3. Тот факт, что данные не реплицируются, означает, что текущие активные сеансы не реплицируются. Кроме того, обнаружение дубликатов и запланированные сообщения могут не работать. Новые сеансы, запланированные сообщения и новые дубликаты будут работать. 

4. Отработку отказа сложной распределенной инфраструктуры необходимо [протестировать](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) по крайней мере один раз. 

5. Синхронизация сущностей может занять некоторое время (примерно 50–100 сущностей в минуту).

## <a name="availability-zones"></a>зоны доступности; 

Номер SKU уровня "Стандартный" для Центров событий поддерживает функцию [Зоны доступности](../availability-zones/az-overview.md), предоставляя изолированные от сбоев расположения в регионе Azure. 

> [!NOTE]
> Поддержка Зон доступности Центров событий Azure ценовой категории "Стандартный" доступна только в [регионах Azure](../availability-zones/az-region.md), в которых представлены зоны доступности.

Вы можете включить эту функцию только в новых пространствах имен с помощью портала Azure. Центры событий не поддерживает перенос существующих пространств имен. Вы не можете отключить избыточность между зонами после ее включения в пространстве имен.

При использовании зон доступности метаданные и данные (события) реплицируются в центрах обработки данных в зоне доступности. 

![3][]

## <a name="private-endpoints"></a>Частные конечные точки
В этом разделе приводятся дополнительные рекомендации при использовании географического аварийного восстановления с пространствами имен, в которых используются частные конечные точки. Чтобы узнать больше об использовании частных конечных точек с Центрами событий, ознакомьтесь с разделом [Настройка частных конечных точек](private-link-service.md).

### <a name="new-pairings"></a>Новые пары
При попытке создать пару между основным пространством имен с частной конечной точкой и дополнительным пространством имен без частной конечной точки связывание завершится ошибкой. Связывание выполнится успешно, только если у основного и дополнительного пространств имен есть частные конечные точки. Рекомендуется использовать одинаковые конфигурации в основном и дополнительном пространствах имен, а также в виртуальных сетях, в которых создаются частные конечные точки.  

> [!NOTE]
> При попытке связать основное пространство имен с частной конечной точкой и дополнительное пространство имен проверяется только, существует ли частная конечная точка в дополнительном пространстве имен. Не проверяется, работает ли конечная точка и будет ли она работать после отработки отказа. Вы должны убедиться, что дополнительное пространство имен с частной конечной точкой будет корректно работать после отработки отказа.
>
> Чтобы проверить, совпадают конфигурации частных конечных точек в основном и дополнительном пространствах имен, отправьте запрос на чтение (например, [получение узла уведомлений](/rest/api/eventhub/get-event-hub)) в дополнительное пространство имен извне виртуальной сети и убедитесь, что из службы получено сообщение об ошибке.

### <a name="existing-pairings"></a>Существующие пары
Если связывание между основным и дополнительным пространством имен уже существует, создание частной конечной точки в основном пространстве имен завершится ошибкой. Чтобы устранить эту проблему, сначала создайте частную конечную точку в дополнительном пространстве имен, а затем — в основном.

> [!NOTE]
> Хотя мы допускаем доступ только для чтения к дополнительному пространству имен, обновление конфигураций частных конечных точек разрешено. 

### <a name="recommended-configuration"></a>Рекомендуемая конфигурация
При создании конфигурации аварийного восстановления для приложения и пространств имен Центров событий необходимо создать частные конечные точки для основного и дополнительного пространств имен Центров событий для виртуальных сетей, в которых размещаются как первичные, так и вторичные экземпляры приложения. 

Предположим, у вас есть две виртуальные сети (VNET-1, VNET-2) и основное и дополнительное пространства имен: EventHubs-Namespace1-Primary, EventHubs-Namespace2-Secondary. Сделайте следующее. 

- В EventHubs-Namespace1-Primary создайте две частные конечные точки, которые используют подсети VNET-1 и VNET-2
- В EventHubs-Namespace2-Secondary создайте две частные конечные точки, которые используют подсети VNET-1 и VNET-2 

![Частные конечные точки и виртуальные сети](./media/event-hubs-geo-dr/private-endpoints-virtual-networks.png)

Преимуществом этого подхода является то, что отработка отказа может происходить на уровне приложения независимо от пространства имен Центров событий. Рассмотрим следующие сценарии. 

**Отработка отказа только на уровне приложения.** Здесь приложение не будет существовать в VNET-1, но будет перемещено в VNET-2. Так как обе частные конечные точки настроены в VNET-1 и VNET-2 как для основного, так и для дополнительного пространства имен, приложение будет работать. 

**Отработка отказа только в пространстве имен Центров событий**. И здесь, так как обе частные конечные точки настроены в обеих виртуальных сетях как для основного, так и для дополнительного пространства имен, приложение будет работать. 

> [!NOTE]
> Рекомендации по геоизбыточному аварийному восстановлению виртуальной сети см. в статье [Виртуальная сеть — непрерывность бизнес-процессов](../virtual-network/virtual-network-disaster-recovery-guidance.md).
 
## <a name="next-steps"></a>Дальнейшие действия

* В разделе с [примером на сайте GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) рассматривается простой рабочий процесс, который создает геосвязь и инициирует отработку отказа для сценария аварийного восстановления.
* В [справочнике для REST API](/rest/api/eventhub/) описываются программные интерфейсы, позволяющие настроить географическое аварийное восстановление.

Дополнительные сведения о Центрах событий см. по следующим ссылкам:

- Приступая к работе с Центрами событий
    - [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
    - [Java](event-hubs-java-get-started-send.md)
    - [Python](event-hubs-python-get-started-send.md)
    - [JavaScript](event-hubs-java-get-started-send.md)
* [Часто задаваемые вопросы о Центрах событий](event-hubs-faq.md)
* [Примеры приложений, использующих Центры событий](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png
[3]: ./media/event-hubs-geo-dr/eh-az.png
