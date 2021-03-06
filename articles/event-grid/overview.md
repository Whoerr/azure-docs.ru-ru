---
title: Что такое служба "Сетка событий Azure"?
description: Сведения об отправке данных событий из источника к обработчикам с помощью службы "Сетка событий Azure", а также о создании приложений на основе событий и интеграции со службами Azure.
ms.topic: overview
ms.date: 01/28/2021
ms.openlocfilehash: e53665c88c3860d37b3512c6498ab626b02a6400
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99055146"
---
# <a name="what-is-azure-event-grid"></a>Что такое служба "Сетка событий Azure"?

Служба "Сетка событий Azure" позволяет легко создавать приложения с архитектурой на основе событий. Сначала выберите ресурс Azure, на которой вы хотите подписаться, а затем укажите обработчик событий или конечную точку веб-перехватчика для отправки события. Служба "Сетка событий" обеспечивает встроенную поддержку событий, поступающих из таких служб Azure, как хранилища BLOB-объектов и группы ресурсов. Служба "Сетка событий" также поддерживает пользовательские события с использованием соответствующих разделов. 

Вы можете применять фильтры для маршрутизации определенных событий в различные конечные точки, использовать многоадресную рассылку в несколько конечных точек, а также проверять надежность доставки.

Сетка событий Azure развертывается для обеспечения максимальной доступности, предопределенным образом распределяясь между несколькими доменами сбоя в каждом регионе, а также между в зонами доступности (в регионах, которые их поддерживают). Список регионов, поддерживаемых службой "Сетка событий", см. в статье [Доступность продуктов по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all).

В статье представлен обзор службы "Сетка событий Azure". Чтобы приступить к использованию службы, см. раздел [Создание и перенаправление пользовательского события со службой "Сетка событий Azure"](custom-event-quickstart.md). 

:::image type="content" source="./media/overview/functional-model.png" alt-text="Модель источников и обработчиков в службе &quot;Сетка событий&quot;" lightbox="./media/overview/functional-model-big.png":::

На этом рисунке показано, как служба "Сетка событий" соединяет источники и обработчики. Здесь не предоставлен полный список поддерживаемых интеграций.

## <a name="event-sources"></a>Источники событий

В настоящее время отправку событий в службу "Сетка событий" поддерживают перечисленные ниже службы Azure. Для получения дополнительных сведений, перейдите по ссылке элемента из списка.

- [Конфигурация приложений Azure](event-schema-app-configuration.md)
- [Хранилище BLOB-объектов Azure](event-schema-blob-storage.md)
- [Службы коммуникации Azure](event-schema-communication-services.md) 
- [Реестр контейнеров Azure](event-schema-container-registry.md)
- [Центры событий Azure](event-schema-event-hubs.md)
- [Центр Интернета вещей Azure](event-schema-iot-hub.md)
- [Хранилище ключей Azure](event-schema-key-vault.md)
- [Машинное обучение Azure](event-schema-machine-learning.md)
- [Azure Maps](event-schema-azure-maps.md)
- [Службы мультимедиа Azure](event-schema-media-services.md)
- [группы ресурсов Azure](event-schema-resource-groups.md).
- [служебной шине Azure](event-schema-service-bus.md)
- [Azure SignalR](event-schema-azure-signalr.md)
- [Подписки Azure](event-schema-subscriptions.md)
- [Кэш Azure для Redis](event-schema-azure-cache.md)

## <a name="event-handlers"></a>Обработчики событий

Дополнительные сведения о возможностях каждого обработчика, а также статьи по этой теме см. в описании [обработчиков событий](event-handlers.md). В настоящее время обработку событий из службы "Сетка событий" поддерживают следующие службы Azure: 

* [Служба автоматизации Azure](handler-webhooks.md#azure-automation)
* [Функции Azure](handler-functions.md)
* [Центры событий](handler-event-hubs.md)
* [Гибридные подключения Relay](handler-relay-hybrid-connections.md)
* [Logic Apps](handler-webhooks.md#logic-apps)
* [Power Automate (ранее — Microsoft Flow)](https://preview.flow.microsoft.com/connectors/shared_azureeventgrid/azure-event-grid/)
* [Служебная шина](handler-service-bus.md)
* [Хранилище очередей](handler-storage-queues.md)
* [Веб-перехватчики](handler-webhooks.md)

## <a name="concepts"></a>Основные понятия

Есть пять основных понятий, с которыми нужно ознакомиться перед началом работы со службой "Сетка событий Azure":

* **События** — это то, что произошло.
* **Источники событий** — это расположения, в которых произошли события.
* **Разделы** — это конечная точка, в которую издатель отправляет событие.
* **Подписки на события** — это конечная точка или встроенный механизм для маршрутизации событий, иногда в несколько обработчиков. Кроме того, подписки используются обработчиками для интеллектуальной фильтрации входящих событий.
* **Обработчики событий** — это приложения или службы, реагирующие на события.

Дополнительные сведения об этих понятиях см. в разделе [Concepts in Azure Event Grid](concepts.md) (Основные понятия службы "Сетка событий Azure").

## <a name="capabilities"></a>Возможности

Вот несколько ключевых преимуществ службы "Сетка событий Azure":

* **Простота** — интерактивная поддержка целевых событий в ресурсе Azure для любого обработчика событий или конечной точки.
* **Расширенная фильтрация** — фильтрация по типам или пути публикации событий, гарантирующая, что обработчики получают только релевантные события.
* **Размноженная отправка** — подписывайтесь на несколько конечных точек для одного и того же события, чтобы копии событий отправлялись в любое требуемое число расположений.
* **Надежность** — выполнение повторных попыток в течение 24 часов с экспоненциальной задержкой гарантирует доставку событий.
* **Оплата за событие** — вы платите только за те ресурсы службы "Сетка событий", которые используете.
* **Высокая пропускная способность** — в службе "Сетка событий"можно создавать рабочие нагрузки большого объема.
* **Встроенные события** — быстрое возобновление работы благодаря встроенным событиям с определяемыми ресурсами.
* **Пользовательские события** — используйте службу "Сетка событий" для маршрутизации и фильтрации пользовательских событий, а также обеспечения их надежной доставки в приложение.

Сравнение службы "Сетка событий", Центров событий и служебной шины см. в статье [Choose between Azure services that deliver messages](compare-messaging-services.md) (Выбор между службами Azure, которые доставляют сообщения).

## <a name="what-can-i-do-with-event-grid"></a>Что можно сделать с помощью службы "Сетка событий"?

Служба "Сетка событий Azure" предоставляет несколько функций, которые значительно расширяют возможности бессерверных операций, автоматизации операций и [интеграции](https://azure.com/integration): 

### <a name="serverless-application-architectures"></a>Архитектура бессерверных приложений

![Архитектура бессерверных приложений](./media/overview/serverless_web_app.png)

Служба "Сетка событий Azure" связывает источники данных и обработчики событий. Например, можно использовать службу "Сетка событий" для активации бессерверной функции, которая анализирует изображения при добавлении в контейнер хранилища BLOB-объектов. 

### <a name="ops-automation"></a>Автоматизация операций

![Автоматизация операций](./media/overview/Ops_automation.png)

Служба "Сетка событий Azure" ускоряет автоматизацию процессов и упрощает применение политик. Например, служба "Сетка событий" может уведомлять службу автоматизации Azure о создании виртуальной машины или базы данных в Базе данных Azure SQL. Эти события можно использовать для автоматической проверки соответствия конфигураций служб, отправки метаданных в рабочие средства, присвоения тегов виртуальным машинам или регистрации рабочих элементов.

### <a name="application-integration"></a>Интеграция приложений

![Интеграция приложений с Azure](./media/overview/app_integration.png)

Служба "Сетка событий Azure" связывает приложения с другими службами. Например, вы можете создавать пользовательские разделы для отправки событий приложения в службу "Сетка событий", используя преимущества надежной доставки, расширенных возможностей маршрутизации и непосредственной интеграции с Azure. Кроме того, службу "Сетка событий Azure" можно использовать с Logic Apps для обработки данных в любой точке мира без необходимости писать код. 

## <a name="how-much-does-event-grid-cost"></a>Сколько стоит служба "Сетка событий"?

Для службы "Сетка событий Azure" применяется модель оплаты за событие, поэтому счет выставляются только за те ресурсы, которые вы используете. Первые 100 000 операций в месяц не оплачиваются. Операциями считаются входящие события, попытки доставки подписок, вызовы управления и фильтрация по суффиксу субъекта. Дополнительные сведения см. на [странице с ценами](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Дальнейшие действия

* [Перенаправление событий хранилища BLOB-объектов](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)  
  Реагирование на события хранилища BLOB-объектов с помощью службы "Сетка событий".
* [Создание и перенаправление пользовательского события со службой "Сетка событий Azure"](custom-event-quickstart.md)  
  Перейдите сразу к делу. Отправляйте пользовательские события в любую конечную точку, воспользовавшись инструкциями в кратком руководстве по службе "Сетка событий Azure".
* [Отслеживание изменений виртуальной машины с помощью Azure Logic Apps и службы "Сетка событий Azure"](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  Руководство по созданию приложения с помощью Logic Apps для реагирования на события, отправляемые службой "Сетка событий".
* [Потоковая передача больших данных в хранилище данных](event-grid-event-hubs-integration.md)  
  Руководство, в котором служба "Функции Azure" используется для потоковой передачи данных из Центров событий в Azure Synapse Analytics.
* [Справочник по REST API службы "Сетка событий Azure"](/rest/api/eventgrid)  
  Содержит справочные материалы по управлению подписками на события, маршрутизацией и фильтрацией.