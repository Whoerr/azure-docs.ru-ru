---
title: Обмен сообщениями с помощью служебной шины Azure
description: Создание автоматизированных задач и рабочих процессов, отправляющих и получающих сообщения с помощью служебной шины Azure в Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: logicappspm, azla
ms.topic: conceptual
ms.date: 02/10/2021
tags: connectors
ms.openlocfilehash: 98d2ee8a85d25065c0021841a9b99a6d616a35d8
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100367424"
---
# <a name="exchange-messages-in-the-cloud-by-using-azure-logic-apps-and-azure-service-bus"></a>Обмен сообщениями в облаке с помощью Azure Logic Apps и служебной шины Azure

С помощью [Azure Logic Apps](../logic-apps/logic-apps-overview.md) и соединителя [служебной шины Azure](../service-bus-messaging/service-bus-messaging-overview.md) можно создавать автоматизированные задачи и рабочие процессы, которые перемещают данные, такие как заказы на продажу и покупку, журналы и перемещения запасов по приложениям для вашей организации. Соединитель не только отслеживает, отправляет сообщения и управляет ими, но также выполняет действия с очередями, сеансами, разделами, подписками и т. д., например:

* Отслеживает прибытие (автозавершение) или прием сообщений (с блокировкой при извлечении) в очередях, разделах и подписках на разделы.
* Отправляет сообщения.
* Создает и удаляет подписки на разделы.
* Управляет сообщениями в очередях и подписках на разделы, например, выполняет такие действия, как получение сообщения, получение отложенного сообщения, заполнение, отложение, прерывание и размещение недоставленных сообщений.
* Продлевает блокировку сообщений и сеансы в очередях и подписках на разделы.
* Закрывает сеансы в очередях и разделах.

Вы можете использовать триггеры, которые получают ответы из служебной шины и делают выходные данные доступными для других действий в приложениях логики. Кроме того, выходные данные действий служебной шины могут использоваться другими действиями. Если вы не знакомы с служебной шиной и Logic Apps, ознакомьтесь со [сведениями о](../service-bus-messaging/service-bus-messaging-overview.md) том, что такое служебная шина Azure? и [что такое Azure Logic Apps](../logic-apps/logic-apps-overview.md)?

[!INCLUDE [Warning about creating infinite loops](../../includes/connectors-infinite-loops.md)]

## <a name="prerequisites"></a>Предварительные требования

* Учетная запись и подписка Azure. Если у вас еще нет подписки Azure, [зарегистрируйтесь для получения бесплатной учетной записи Azure](https://azure.microsoft.com/free/).

* Пространство имен служебной шины и сущность обмена сообщениями, например очередь. Эти элементы и приложение логики должны использовать одну и ту же подписку Azure. Если у вас нет этих элементов, узнайте, как [создавать пространство имен служебной шины и очередь](../service-bus-messaging/service-bus-create-namespace-portal.md).

* Базовые знания [создания приложений логики](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Приложение логики, в котором используется пространство имен служебной шины и сущность обмена сообщениями. Приложение логики и служебная шина должны использовать одну и ту же подписку Azure. Чтобы запустить рабочий процесс с помощью триггера служебной шины, [создайте пустое приложение логики](../logic-apps/quickstart-create-first-logic-app-workflow.md). Чтобы использовать действие служебной шины в рабочем процессе, запустите приложение логики с помощью другого триггера, например [триггера повторения](../connectors/connectors-native-recurrence.md).

<a name="permissions-connection-string"></a>

## <a name="check-permissions"></a>Проверьте разрешения.

Убедитесь, что у приложения логики есть разрешения для доступа к пространству имен служебной шины.

1. В [портал Azure](https://portal.azure.com)Войдите с помощью учетной записи Azure.

1. Перейдите к *пространству имен* служебной шины. На странице пространства имен в разделе **Параметры** выберите **Политики общего доступа**. В разделе **Утверждения** убедитесь, что у вас есть разрешения на **управление** для этого пространства имен.

   ![Управление разрешениями для пространства имен служебной шины](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

1. Получите строку подключения для пространства имен служебной шины. Эта строка необходима при предоставлении сведений о подключении в приложении логики.

   1. На панели **политики общего доступа** выберите **RootManageSharedAccessKey**.

   1. Нажмите кнопку Копировать рядом с основной строкой подключения. Сохраните строку подключения для дальнейшего использования.

      ![Копирование строки подключения к пространству имен служебной шины](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Чтобы проверить, относится ли строка подключения к пространству имен служебной шины или сущности обмена сообщениями, например очереди, найдите в ней параметр `EntityPath`. Если в строке подключения есть такой параметр, значит она относится к определенной сущности и ее нельзя использовать в приложении логики.

## <a name="add-service-bus-trigger"></a>Добавить триггер служебной шины

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Войдите в [портал Azure](https://portal.azure.com)и откройте пустое приложение логики в конструкторе приложений логики.

1. В поле поиска на портале введите `azure service bus` . В появившемся списке триггеры выберите нужный триггер.

   Например, чтобы активировать приложение логики при отправке нового элемента в очередь служебной шины, выберите **время получения сообщения в триггере очереди (автозавершение)** .

   ![Выбор триггера служебной шины](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   Ниже приведены некоторые рекомендации по использованию триггера служебной шины.

   * Все триггеры служебной шины являются триггерами *длительного опроса* . Это описание означает, что при срабатывании триггера триггер обрабатывает все сообщения, а затем ожидает 30 секунд для отображения дополнительных сообщений в очереди или подписке раздела. Если на протяжении 30 секунд новые сообщения не поступают, триггер больше не срабатывает. В противном случае триггер продолжит считывать сообщения, пока очередь или подписка раздела не опустеет. Следующий опрос триггера основывается на интервале повторения, установленном в свойствах триггера.

   * Некоторые триггеры, например, **когда одно или несколько сообщений поступают в очередь (автозавершение)** , могут возвращать одно или несколько сообщений. Когда эти триггеры срабатывают, они возвращают значение между одним и количеством сообщений, указанных в свойстве **максимального числа сообщений** триггера.

     > [!NOTE]
     > Триггер автоматического завершения автоматически завершает сообщение, но завершение происходит только при следующем вызове служебной шины. Такое поведение может повлиять на структуру приложения логики. Например, Избегайте изменения параллелизма в триггере автозавершения, так как это изменение может привести к дублированию сообщений, если приложение логики переходит в регулируемое состояние. При изменении элемента управления параллелизмом создаются следующие условия: регулируемые триггеры пропускаются с помощью `WorkflowRunInProgress` кода, операция завершения не выполняется, а выполнение следующего триггера происходит после интервала опроса. Необходимо установить длительность блокировки служебной шины в значение, превышающее интервал опроса. Однако, несмотря на этот параметр, сообщение по-прежнему может не завершиться, если приложение логики остается в регулируемом состоянии при следующем интервале опроса.

   * Если [включить параметр параллелизма](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency) для триггера служебной шины, значение по умолчанию для этого `maximumWaitingRuns` свойства равно 10. В зависимости от значения длительности блокировки сущности служебной шины и длительности выполнения экземпляра приложения логики это значение по умолчанию может быть слишком большим и может вызвать исключение "Блокировка потеряно". Чтобы найти оптимальное значение для вашего сценария, начните тестирование со значением 1 или 2 для `maximumWaitingRuns` Свойства. Чтобы изменить значение параметра Максимальное число запусков в ожидании, см. раздел [изменение числа запусков с ожиданием](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs).

1. Если ваш триггер подключается к пространству имен служебной шины в первый раз, выполните следующие действия, когда конструктор приложений логики запросит сведения о подключении.

   1. Укажите имя для подключения и выберите нужное пространство имен служебной шины.

      ![Снимок экрана, показывающий имя подключения и выбор пространства имен служебной шины](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-trigger-1.png)

      Чтобы вместо этого ввести строку подключения вручную, выберите **вручную ввести сведения о подключении**. Если у вас нет строки подключения, узнайте, [как ее найти](#permissions-connection-string).

   1. Выберите политику служебной шины и нажмите кнопку **создать**.

      ![Снимок экрана, на котором показано, как выбрать политику служебной шины](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-trigger-2.png)

   1. Выберите нужную сущность обмена сообщениями, например очередь или раздел. В этом примере выберите свою очередь служебной шины.
   
      ![Снимок экрана, на котором показано, как выбрать очередь служебной шины](./media/connectors-create-api-azure-service-bus/service-bus-select-queue-trigger.png)

1. Укажите необходимые сведения для выбранного триггера. Чтобы добавить другие доступные свойства к действию, откройте список **Добавить новый параметр** и выберите нужные свойства.

   Для триггера этого примера выберите интервал опроса и частоту проверки очереди.

   ![Снимок экрана, на котором показан параметр интервала опроса триггера служебной шины](./media/connectors-create-api-azure-service-bus/service-bus-trigger-details.png)

   Дополнительные сведения о доступных триггерах и свойствах см. на [странице справочника](/connectors/servicebus/)по соединителю.

1. Продолжайте создание приложения логики, добавляя необходимые действия.

   Например, можно добавить действие, которое отправляет сообщение электронной почты при поступлении нового сообщения. Когда триггер проверяет вашу очередь и находит новое сообщение, ваше приложение логики запускает для него выбранные действия.

## <a name="add-service-bus-action"></a>Добавить действие служебной шины

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Откройте приложение логики в конструкторе приложений логики на [портале Azure](https://portal.azure.com).

1. На шаге, где нужно добавить действие, выберите **новый шаг**.

   Или, чтобы добавить действие между шагами, наведите указатель мыши на стрелку между этими шагами. Выберите отображаемый знак плюса ( **+** ) и щелкните **Добавить действие**.

1. В разделе **Выберите действие**, введите в поле поиска `azure service bus`. В появившемся списке действий выберите нужное действие. 

   В этом примере выберите действие **Отправить сообщение** .

   ![Снимок экрана, на котором показано, как выбрать действие служебной шины](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png) 

1. Если ваше действие подключается к пространству имен служебной шины в первый раз, выполните следующие действия, когда конструктор приложений логики запросит сведения о подключении.

   1. Укажите имя для подключения и выберите нужное пространство имен служебной шины.

      ![Снимок экрана, показывающий имя подключения и выбор пространства имен служебной шины](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-action-1.png)

      Чтобы вместо этого ввести строку подключения вручную, выберите **вручную ввести сведения о подключении**. Если у вас нет строки подключения, узнайте, [как ее найти](#permissions-connection-string).

   1. Выберите политику служебной шины и нажмите кнопку **создать**.

      ![Снимок экрана, на котором показано, как выбрать политику служебной шины и нажать кнопку "создать"](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-action-2.png)

   1. Выберите нужную сущность обмена сообщениями, например очередь или раздел. В этом примере выберите свою очередь служебной шины.

      ![Снимок экрана, на котором показано, как выбрать очередь служебной шины](./media/connectors-create-api-azure-service-bus/service-bus-select-queue-action.png)

1. Укажите необходимые сведения для выбранного действия. Чтобы добавить другие доступные свойства к действию, откройте список **Добавить новый параметр** и выберите нужные свойства.

   Например, выберите свойства **содержимое** и **тип содержимого** , чтобы добавить их в действие. Затем укажите содержимое сообщения, которое необходимо отправить.

   ![Снимок экрана, показывающий тип содержимого сообщения и сведения](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)

   Дополнительные сведения о доступных действиях и их свойствах см. на [странице справочника](/connectors/servicebus/)по соединителю.

1. Продолжайте создание приложения логики, добавляя любые другие необходимые действия.

   Например, можно добавить действие, которое отправляет электронную почту, чтобы подтвердить, что сообщение было отправлено.

1. Сохраните приложение логики. На панели инструментов конструктора щелкните **Сохранить**.

<a name="sequential-convoy"></a>

## <a name="send-correlated-messages-in-order"></a>Отправка коррелированных сообщений по порядку

При необходимости отправки связанных сообщений в определенном порядке можно использовать [ *последовательный шаблон сопровождение*](/azure/architecture/patterns/sequential-convoy) с помощью [соединителя служебной шины Azure](../connectors/connectors-create-api-servicebus.md). Коррелированные сообщения имеют свойство, определяющее связь между этими сообщениями, например идентификатор [сеанса](../service-bus-messaging/message-sessions.md) в служебной шине.

При создании приложения логики можно выбрать **коррелированную поочередную доставку с помощью шаблона сеансов служебной шины** , который реализует последовательный шаблон сопровождение. Дополнительные сведения см. [в разделе Отправка связанных сообщений по порядку](../logic-apps/send-related-messages-sequential-convoy.md).

## <a name="delays-in-updates-to-your-logic-app-taking-effect"></a>Задержки в обновлениях приложения логики, которые вступают в действие

Если интервал опроса триггера служебной шины мал, например 10 секунд, обновления для приложения логики могут не действовать в течение 10 минут. Чтобы обойти эту проблему, можно отключить приложение логики, внести изменения, а затем снова включить приложение логики.

<a name="connector-reference"></a>

## <a name="connector-reference"></a>Справочник по соединителям

С помощью служебной шины соединитель служебной шины может сохранять до 1 500 уникальных сеансов за раз в кэше соединителя на каждую [сущность обмена сообщениями служебной шины, например подписку или раздел](../service-bus-messaging/service-bus-queues-topics-subscriptions.md). Если число сеансов превышает это ограничение, старые сеансы удаляются из кэша. Дополнительные сведения см. в разделе [сеансы сообщений](../service-bus-messaging/message-sessions.md).

Для получения других технических сведений о триггерах, действиях и ограничениях, которые описаны в описании Swagger соединителя, ознакомьтесь со [страницей справочника по соединителям](/connectors/servicebus/). Дополнительные сведения об обмене сообщениями через служебную шину Azure см. в статье [что такое служебная шина Azure](../service-bus-messaging/service-bus-messaging-overview.md)?

## <a name="next-steps"></a>Дальнейшие действия

* См. дополнительные сведения о других [соединителях Logic Apps](../connectors/apis-list.md).
