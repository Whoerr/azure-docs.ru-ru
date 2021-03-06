---
title: Часто задаваемые вопросы о служебной шине Azure | Документация Майкрософт
description: В этой статье содержатся ответы на некоторые часто задаваемые вопросы о служебной шине Azure.
ms.topic: article
ms.date: 01/20/2021
ms.openlocfilehash: 3a96cf94ca4a7edd115f12b3e2eded11a5894e04
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2021
ms.locfileid: "98693412"
---
# <a name="azure-service-bus---frequently-asked-questions-faq"></a>Служебная шина Azure — часто задаваемые вопросы

В этой статье обсуждаются некоторые часто задаваемые вопросы о служебной шине Microsoft Azure. Вы также можете посетить страницу [Часто задаваемые вопросы о поддержке Azure](https://azure.microsoft.com/support/faq/), чтобы получить общие сведения о ценах и поддержке Azure.


## <a name="general-questions-about-azure-service-bus"></a>Общие вопросы о служебной шине Azure
### <a name="what-is-azure-service-bus"></a>Что такое служебная шина Azure?
[Служебная шина Azure](service-bus-messaging-overview.md) — это облачная платформа асинхронного обмена сообщениями, которая позволяет передавать данные между несвязанными системами. Корпорация Майкрософт предлагает эту функцию как услугу. Это означает, что вам не нужно размещать собственное оборудование для его использования.

### <a name="what-is-a-service-bus-namespace"></a>Что такое пространство имен служебной шины?
[Пространство имен](service-bus-create-namespace-portal.md) предоставляет контейнер области для адресации ресурсов служебной шины в приложении. Создание пространства имен является первым и обязательным шагом перед началом работы со служебной шиной.

### <a name="what-is-an-azure-service-bus-queue"></a>Что такое очередь служебной шины Azure?
[Очередь служебной шины](service-bus-queues-topics-subscriptions.md) — это сущность, в которой сохраняются сообщения. Очереди целесообразно использовать при наличии нескольких приложений или нескольких частей распределенного приложения, которые должны взаимодействовать друг с другом. Очередь подобна центру распределения, куда поступают несколько продуктов (сообщений), которые затем отправляются из этого расположения.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Что такое разделы и подписки служебной шины Azure?
Раздел можно представить в виде очереди. При использовании нескольких подписок он становится расширенной моделью обмена сообщениями. По сути, это инструмент связи по принципу "один ко многим". В рамках этой модели публикации и подписки (*pub/sub*) сообщение может быть отправлено из приложения в раздел с несколькими подписками, а затем получено несколькими приложениями.

### <a name="what-is-a-partitioned-entity"></a>Что такое секционированная сущность?
Обычная очередь или раздел обрабатываются одним брокером сообщений и сохраняются в одном хранилище сообщений. Поддерживается только в уровнях обмена сообщениями "базовый" и "Стандартный", [секционированной очереди или разделе](service-bus-partitioning.md) обрабатывается несколькими брокерами сообщений и хранится в нескольких хранилищах сообщений. Это означает, что общая пропускная способность секционированной очереди или раздела больше не ограничивается производительностью одного брокера сообщений или хранилища сообщений. Кроме того, временный сбой хранилища сообщений не приводит к недоступности секционированной очереди или раздела.

При использовании секционированных сущностей порядок не гарантируется. Если раздел недоступен, вы по-прежнему можете отправлять и получать сообщения из других секций.

 Секционированные сущности больше не поддерживаются в [номере SKU уровня "Премиум"](service-bus-premium-messaging.md). 

### <a name="where-does-azure-service-bus-store-data"></a><a name="in-region-data-residency"></a>Где служебная шина Azure хранит данные?
Стандартная категория служебной шины Azure использует базу данных SQL Azure для внутреннего уровня хранилища. Для всех регионов, кроме Южная Бразилия и Юго-Восточной Азии, резервная копия базы данных размещается в другом регионе (обычно в парном регионе Azure). Для регионов Южной и Юго-Восточной Азии резервные копии баз данных хранятся в том же регионе, что и требования к местонахождение данных для этих регионов.

На уровне Premium служебная шина Azure сохраняет метаданные и данные в выбранных регионах. При настройке географического аварийного восстановления для пространства имен уровня "Премиум" служебной шины Azure метаданные копируются в выбранный дополнительный регион.


### <a name="what-ports-do-i-need-to-open-on-the-firewall"></a>Какие порты нужно открыть в брандмауэре? 
Для отправки и получения сообщений можно использовать следующие протоколы со служебной шиной Azure.

- Расширенный протокол управления очередью сообщений 1,0 (AMQP)
- Протокол HTTP 1,1 с TLS (HTTPS)

В следующей таблице приведены исходящие TCP-порты, которые необходимо открыть для использования этих протоколов для взаимодействия с служебной шиной Azure.

| Протокол | Port | Сведения | 
| -------- | ----- | ------- | 
| AMQP | 5671 | AMQP с TLS. См. [Руководство по использованию протокола AMQP](service-bus-amqp-protocol-guide.md). | 
| HTTPS | 443 | Этот порт используется для HTTP/REST API и для AMQP-over-WebSockets. |

Порт HTTPS обычно требуется для исходящей связи, если AMQP используется через порт 5671, поскольку несколько операций управления, выполняемых клиентскими пакетами SDK, и получение маркеров из Azure Active Directory (при использовании) выполняются по протоколу HTTPS. 

Официальный пакет SDK для Azure обычно использует протокол AMQP для отправки и получения сообщений из служебной шины. 

[!INCLUDE [service-bus-websockets-options](../../includes/service-bus-websockets-options.md)]

Старый пакет WindowsAzure. ServiceBus для платформа .NET Framework имеет возможность использовать устаревший протокол обмена сообщениями служебной шины (SBMP), также называемый "NetMessaging". Этот протокол использует TCP-порты 9350-9354. Режим по умолчанию для этого пакета — автоматическое определение доступности этих портов для связи и переключение на WebSockets с TLS через порт 443, если это не так. Вы можете переопределить этот параметр и принудительно включить этот режим, задав для `Https` параметра [задать ConnectivityMode](/dotnet/api/microsoft.servicebus.connectivitymode) значение [`ServiceBusEnvironment.SystemConnectivity`](/dotnet/api/microsoft.servicebus.servicebusenvironment.systemconnectivity) , которое применяется глобально к приложению.

### <a name="what-ip-addresses-do-i-need-to-add-to-allow-list"></a>Какие IP-адреса нужно добавить в список разрешений?
Чтобы найти IP-адреса, которые нужно добавить в список разрешений для подключений, выполните следующие действия.

1. В командной строке выполните следующую команду: 

    ```
    nslookup <YourNamespaceName>.servicebus.windows.net
    ```
2. Запишите IP-адрес, возвращенный в `Non-authoritative answer`. 

При использовании **избыточности зоны** для пространства имен необходимо выполнить несколько дополнительных действий. 

1. Сначала следует запустить nslookup в пространстве имен.

    ```
    nslookup <yournamespace>.servicebus.windows.net
    ```
2. Запишите имя в разделе **не заслуживающий доверия ответ**, который имеет один из следующих форматов: 

    ```
    <name>-s1.cloudapp.net
    <name>-s2.cloudapp.net
    <name>-s3.cloudapp.net
    ```
3. Выполните команду nslookup для каждого из них с суффиксами s1, s2 и s3, чтобы получить IP-адреса всех трех экземпляров, работающих в трех зонах доступности. 

    > [!NOTE]
    > IP-адрес, возвращенный `nslookup` командой, не является статическим IP-адресом. Однако он остается постоянным до тех пор, пока базовое развертывание не будет удалено или перемещено в другой кластер.

### <a name="where-can-i-find-the-ip-address-of-the-client-sendingreceiving-messages-tofrom-a-namespace"></a>Где можно найти IP-адрес клиента, отправляющего и принимающего сообщения в пространство имен? 
Мы не будем регистрировать IP-адреса клиентов, отправляющих или получающих сообщения в пространство имен. Повторное создание ключей для того, чтобы все существующие клиенты не могли пройти проверку подлинности и проверить параметры [управления доступом на основе ролей Azure (Azure RBAC)](authenticate-application.md#azure-built-in-roles-for-azure-service-bus), чтобы разрешить доступ к пространству имен только пользователям или приложениям. 

Если вы используете пространство имен уровня " **премиум** ", используйте [IP-фильтрацию](service-bus-ip-filtering.md), [конечные точки службы виртуальной сети](service-bus-service-endpoints.md)и [частные конечные точки](private-link-service.md) , чтобы ограничить доступ к пространству имен. 

## <a name="best-practices"></a>Рекомендации
### <a name="what-are-some-azure-service-bus-best-practices"></a>Рекомендации по работе со служебной шиной Azure
См. [рекомендации по повышению производительности с помощью обмена сообщениями через служебную шину][Best practices for performance improvements using Service Bus]. В этой статье описывается, как оптимизировать производительность обмена сообщениями.

### <a name="what-should-i-know-before-creating-entities"></a>Что необходимо знать перед созданием сущностей?
Приведенные ниже свойства очереди и раздела являются неизменяемыми. Учитывайте это ограничение при подготовке сущностей, так как эти свойства нельзя изменить, не создавая новую заменяющую сущность.

* Секционирование
* Сеансы
* Обнаружение дубликатов
* экспресс-сущность.

## <a name="pricing"></a>Цены
В этом разделе содержатся ответы на некоторые часто задаваемые вопросы о ценах на служебную шину.

В [этой статье](https://azure.microsoft.com/pricing/details/service-bus/) объясняются единицы измерения, на основе которых выставляются счета за использование служебной шины. Подробные сведения о ценах смотрите [здесь](https://azure.microsoft.com/pricing/details/service-bus/).

Вы также можете посетить страницу [Часто задаваемые вопросы о поддержке Azure](https://azure.microsoft.com/support/faq/), чтобы получить общие сведения о ценах на Azure. 

### <a name="how-do-you-charge-for-service-bus"></a>Как выставляется цена на служебную шину?
Все сведения о ценах на использование служебной шины приведены на [странице с ценами][Pricing overview]. Помимо указанной платы, также взимается плата за связанные операции передачи данных, исходящих из центра обработки данных, в котором подготавливается ваше приложение.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-isnt"></a>Какое использование служебной шины считается передачей данных? Что не так?
Любая передача данных в пределах заданного региона Azure выполняется бесплатно, как и любая входящая передача данных. Передача данных за пределы региона оплачивается как исходящий трафик. Соответствующие тарифы указаны [здесь](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Взимает ли служебная шина плату за использование хранилища?
Нет. Служебная шина не взимается за хранилище. Однако существует квота, ограничивающая максимальный объем данных, который может быть сохранен в каждой очереди или разделе. См. следующий раздел часто задаваемых вопросов.

### <a name="i-have-a-service-bus-standard-namespace-why-do-i-see-charges-under-resource-group-system"></a>У меня есть стандартное пространство имен служебной шины. Почему я вижу оплату в группе ресурсов "$system"?
Служебная шина Azure недавно обновила компоненты выставления счетов. Из-за этого изменения, если у вас есть стандартное пространство имен служебной шины, вы можете увидеть элементы строк для ресурса "/Subscriptions/<azure_subscription_id>/resourceGroups/$system/Провидерс/Микрософт.сервицебус/намеспацес/$system" в группе ресурсов "$system".

Эти расходы представляют базовую плату за подписку Azure, в которой подготовлено стандартное пространство имен служебной шины. 

Важно отметить, что эти расходы не являются новыми, т. е. они существовали в предыдущей модели выставления счетов. Единственное изменение заключается в том, что они теперь перечислены в разделе "$system". Это делается из-за ограничений в новой системе выставления счетов, которая группирует расходы на уровень подписки, не привязанные к конкретному ресурсу, под ИДЕНТИФИКАТОРом ресурса "$system".

## <a name="quotas"></a>Квоты

Список ограничений и квот на служебную шину доступен в этом [обзоре квот служебной шины][Quotas overview].

### <a name="how-to-handle-messages-of-size--1-mb"></a>Как обрабатывать сообщения, длина которых превышает 1 МБ?
Службы сообщений служебной шины (очереди, темы и подписки) позволяют приложениям отправлять сообщения размером до 256 КБ (ценовая категория "Стандартный") или до 1 МБ (ценовая категория "Премиум"). Если вы работаете с сообщениями размером больше 1 МБ, используйте шаблон проверки утверждений, описанный в [этой записи блога](https://www.serverless360.com/blog/deal-with-large-service-bus-messages-using-claim-check-pattern).

## <a name="troubleshooting"></a>Устранение неполадок
### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>Почему я не могу создать пространство имен после удаления его из другой подписки? 
При удалении пространства имен из подписки подождите 4 часа, прежде чем создать его заново с тем же именем в другой подписке. Иначе можно получить сообщение об ошибке `Namespace already exists`. 

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Какие исключения порождаются интерфейсами API служебной шины Azure? Какие действия можно предпринять в отношении этих исключений?
Список возможных исключений служебной шины приведен в разделе [Общие сведения об исключениях][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Что такое подписанный URL-адрес? На каких языках можно создавать подписи?
Подписи общего доступа представляют собой механизм проверки подлинности на основе безопасных хэшей SHA-256 или URI. Сведения о создании собственных подписей в Node.js, PHP, Java, Python и C# см. в статье о [подписанных][Shared Access Signatures] URL.

## <a name="subscription-and-namespace-management"></a>Управление подпиской и пространством имен
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Как перенести пространство имен в другую подписку Azure?

Вы можете переместить пространство имен из одной подписки Azure в другую, используя [портал Azure](https://portal.azure.com) или выполняя команды PowerShell. Для выполнения операции пространство имен уже должно быть активным. Пользователь, который выполняет команды, должен обладать правами администратора как в исходной, так и в целевой подписках.

#### <a name="portal"></a>Портал

Чтобы использовать портал Azure для переноса пространств имен служебной шины в другую подписку, следуйте [этим инструкциям](../azure-resource-manager/management/move-resource-group-and-subscription.md#use-the-portal). 

#### <a name="powershell"></a>PowerShell

Следующая последовательность команд PowerShell перемещает пространство имен из одной подписки Azure в другую. Для выполнения этой операции пространство имен уже должно быть активным, а пользователь, выполняющий команды PowerShell, должен обладать правами администратора в исходной и целевой подписках.

```powershell
# Create a new resource group in target subscription
Select-AzSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```
## <a name="is-it-possible-to-disable-tls-10-or-11-on-service-bus-namespaces"></a>Можно ли отключить TLS 1,0 или 1,1 в пространствах имен служебной шины?
Нет. Невозможно отключить TLS 1,0 или 1,1 в пространствах имен служебной шины. В клиентских приложениях, подключающихся к служебной шине, используйте TLS 1,2 или более позднюю версию. Дополнительные сведения см. [в статье обеспечение использования TLS 1,2 с помощью служебной шины Azure — технического сообщества Майкрософт](https://techcommunity.microsoft.com/t5/messaging-on-azure/enforcing-tls-1-2-use-with-azure-service-bus/ba-p/370912).

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о служебной шине см. в следующих статьях:

* [Introducing Azure Service Bus Premium Messaging](https://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/) (Общие сведения об обмене сообщениями через служебную шину Azure уровня "Премиум") (запись блога)
* [Introducing Azure Service Bus Premium Messaging](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging) (Общие сведения об обмене сообщениями через служебную шину Azure уровня "Премиум") (Channel9)
* [Обзор служебной шины](service-bus-messaging-overview.md)
* [Начало работы с очередями служебной шины](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
