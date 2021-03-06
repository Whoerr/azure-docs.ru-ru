---
title: Подключение к конечным точкам REST из Azure Logic Apps
description: Выполняйте мониторинг конечных точек REST в автоматизированных задачах, процессах и рабочих процессах с помощью Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/01/2019
tags: connectors
ms.openlocfilehash: 810aaae9634a7de8d07b6d49edd0c6c2eda96754
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101730887"
---
# <a name="call-rest-endpoints-by-using-azure-logic-apps"></a>Вызов конечных точек REST с помощью Azure Logic Apps

С помощью [Azure Logic Apps](../logic-apps/logic-apps-overview.md) и встроенного соединителя HTTP + Swagger можно автоматизировать рабочие процессы, которые регулярно вызывают какую-либо конечную точку REST через [файл Swagger](https://swagger.io), создавая приложения логики. Триггер и действие HTTP + Swagger работают так же, как [триггер и действие HTTP](connectors-native-http.md), но обеспечивают более широкие возможности при работе с конструктором приложений логики, предоставляя структуру API и выходные данные, описанные файлом Swagger. Для внедрения опрашивающего триггера используйте шаблон опроса, описанный в разделе [Создание пользовательских API для вызова других API, служб и систем из приложений логики](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас еще нет подписки Azure, [зарегистрируйтесь для получения бесплатной учетной записи Azure](https://azure.microsoft.com/free/).

* URL-адрес файла Swagger (не OpenAPI), который описывает целевую конечную точку REST

  Как правило, чтобы соединитель мог работать, конечная точка REST должна соответствовать следующим условиям.

  * Файл Swagger должен размещаться в ресурсе с общедоступным URL-адресом HTTPS.
  
  * Файл Swagger должен содержать `operationID` операцию for each в определении. В противном случае соединитель показывает только последнюю операцию в файле Swagger. 

  * В файле Swagger должен быть включен [общий доступ к ресурсам независимо от источника (CORS)](/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services).

  Если файл Swagger не размещен или не соответствует требованиям безопасности и независимости от источника, можно [отправить файл Swagger в контейнер больших двоичных объектов в учетной записи хранения Azure](#host-swagger) и включить CORS в этой учетной записи хранения, чтобы можно было ссылаться на этот файл.

  В примерах этого раздела используется [API распознавания лиц Cognitive Services](../cognitive-services/face/overview.md), для которого требуется [учетная запись Cognitive Services и ключ доступа](../cognitive-services/cognitive-services-apis-create-account.md).

* Базовые знания о [создании приложений логики](../logic-apps/quickstart-create-first-logic-app-workflow.md). Если вы не знакомы с приложениями логики, ознакомьтесь со статьей [Что такое Azure Logic Apps](../logic-apps/logic-apps-overview.md).

* Приложение логики, из которого вы хотите вызвать целевую конечную точку. Чтобы начать работу с триггером HTTP + Swagger, [создайте пустое приложение логики](../logic-apps/quickstart-create-first-logic-app-workflow.md). Чтобы использовать действие HTTP + Swagger, запустите свое приложение логики с любым триггером. В этом примере на первом шаге используется триггер HTTP + Swagger.

## <a name="add-an-http--swagger-trigger"></a>Добавление триггера HTTP + Swagger

Этот встроенный триггер отправляет HTTP-запрос на URL-адрес файла Swagger, который описывает REST API, и возвращает ответ с содержимым этого файла.

1. Войдите на [портал Azure](https://portal.azure.com). Откройте свое пустое приложение логики в конструкторе приложений логики.

1. В конструкторе в поле поиска введите "swagger" в качестве фильтра. В списке **Триггеры** выберите триггер **HTTP + Swagger**.

   ![Выбор триггера HTTP + Swagger](./media/connectors-native-http-swagger/select-http-swagger-trigger.png)

1. В поле **URL-АДРЕС КОНЕЧНОЙ ТОЧКИ SWAGGER** введите URL-адрес файла Swagger и нажмите кнопку **Далее**.

   В этом примере для [API распознавания лиц Cognitive Services](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) используется URL-адрес Swagger, расположенный в регионе "Западная часть США":

   `https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/export?DocumentFormat=Swagger&ApiName=Face%20API%20-%20V1.0`

   ![Снимок экрана, показывающий конструктор приложений логики с триггером "H T T + Swagger" и свойством "конечная точка Swagger U R L", для которого задано значение U R L.](./media/connectors-native-http-swagger/http-swagger-trigger-parameters.png)

1. Когда в конструкторе появятся операции, описанные в файле Swagger, выберите операцию, которую хотите использовать.

   ![Снимок экрана, на котором показан конструктор приложений логики с триггером "H T T + Swagger" и список, отображающий операции Swagger.](./media/connectors-native-http-swagger/http-swagger-trigger-operations.png)

1. Укажите значения параметров триггера в зависимости от выбранной операции, которую вы хотите включить в вызов конечной точки. Настройте периодичность, с которой триггер должен вызывать целевую конечную точку.

   В этом примере триггер переименовывается в "HTTP + Swagger Trigger: Face - Detect", чтобы этот шаг имел более понятное имя.

   ![Снимок экрана, показывающий конструктор приложений логики с триггером "H T T + Swagger", который отображает операцию распознавания лиц.](./media/connectors-native-http-swagger/http-swagger-trigger-operation-details.png)

1. Чтобы добавить другие доступные параметры, откройте список **Добавить новый параметр** и выберите нужные параметры.

   Дополнительные сведения о типах проверки подлинности, доступных для HTTP + Swagger, см. в разделе [Добавление проверки подлинности в исходящие вызовы](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. Продолжите создание рабочего процесса приложения логики с действиями, выполняемыми при срабатывании триггера.

1. Закончив, не забудьте сохранить свое приложение логики. На панели инструментов конструктора щелкните **Сохранить**.

## <a name="add-an-http--swagger-action"></a>Добавление действия триггера HTTP + Swagger

Это встроенное действие отправляет HTTP-запрос на URL-адрес файла Swagger, который описывает REST API, и возвращает ответ с содержимым этого файла.

1. Войдите на [портал Azure](https://portal.azure.com). Создание приложения логики в Конструкторе приложений логики.

1. В шаге, где вы хотите добавить действие триггера HTTP + Swagger, выберите **Создать шаг**.

   Чтобы добавить действие между шагами, переместите указатель на стрелку между шагами. Нажмите появившийся знак "плюс" ( **+** ), а затем выберите **Добавить действие**.

1. В конструкторе в поле поиска введите "swagger" в качестве фильтра. В списке **Действия** выберите действие **HTTP + Swagger**.

    ![Выбор действия "HTTP + Swagger"](./media/connectors-native-http-swagger/select-http-swagger-action.png)

1. В поле **URL-АДРЕС КОНЕЧНОЙ ТОЧКИ SWAGGER** введите URL-адрес файла Swagger и нажмите кнопку **Далее**.

   В этом примере для [API распознавания лиц Cognitive Services](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) используется URL-адрес Swagger, расположенный в регионе "Западная часть США":

   `https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/export?DocumentFormat=Swagger&ApiName=Face%20API%20-%20V1.0`

   ![Ввод URL-адреса для конечной точки Swagger](./media/connectors-native-http-swagger/http-swagger-action-parameters.png)

1. Когда в конструкторе появятся операции, описанные в файле Swagger, выберите операцию, которую хотите использовать.

   ![Операции в файле Swagger](./media/connectors-native-http-swagger/http-swagger-action-operations.png)

1. Укажите значения для параметров действия в зависимости от выбранной операции, которую вы хотите включить в вызов конечной точки.

   В этом примере нет параметров; в нем действие переименовывается в "HTTP + Swagger Action: Face - Identify", чтобы этот шаг имел более понятное имя.

   ![Сведения об операции](./media/connectors-native-http-swagger/http-swagger-action-operation-details.png)

1. Чтобы добавить другие доступные параметры, откройте список **Добавить новый параметр** и выберите нужные параметры.

   Дополнительные сведения о типах проверки подлинности, доступных для HTTP + Swagger, см. в разделе [Добавление проверки подлинности в исходящие вызовы](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. Закончив, не забудьте сохранить свое приложение логики. На панели инструментов конструктора щелкните **Сохранить**.

<a name="host-swagger"></a>

## <a name="host-swagger-in-azure-storage"></a>Размещение Swagger в службе хранилища Azure

Если файл Swagger не размещен или не соответствует требованиям безопасности и независимости от источника, можно отправить этот файл в контейнер больших двоичных объектов в учетной записи хранения Azure и включить CORS в этой учетной записи хранения, чтобы можно было ссылаться на этот файл. Для создания, настройки и сохранения файлов Swagger в службе хранилища Azure необходимо выполнить следующие действия.

1. [Создайте учетную запись хранения Azure](../storage/common/storage-account-create.md).

1. Теперь включите CORS для большого двоичного объекта. В меню вашей учетной записи хранения выберите **CORS**. На вкладке **Служба BLOB-объектов** укажите эти значения, а затем нажмите **Сохранить**.

   | Свойство | Значение |
   |----------|-------|
   | **Допустимые источники** | `*` |
   | **Допустимые методы** | `GET`, `HEAD`, `PUT` |
   | **Допустимые заголовки** | `*` |
   | **Предоставляемые заголовки** | `*` |
   | **Максимальный возраст** (в секундах) | `200` |
   |||

   Хотя в этом примере используется [портал Azure](https://portal.azure.com), вы можете воспользоваться другим инструментом, таким как [Обозреватель службы хранилища Azure](https://storageexplorer.com/), или автоматически настроить этот параметр с помощью [скрипта PowerShell](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) из этого примера.

1. [Создайте контейнер BLOB объектов](../storage/blobs/storage-quickstart-blobs-portal.md). В панели **Обзор** контейнера выберите **Изменить уровень доступа**. В списке **Общедоступный уровень доступа** выберите **BLOB-объект (анонимный доступ на чтение только для BLOB-объектов)** и нажмите кнопку **ОК**.

1. [Отправьте файл Swagger в контейнер BLOB-объектов](../storage/blobs/storage-quickstart-blobs-portal.md#upload-a-block-blob) с помощью [портала Azure](https://portal.azure.com) или [Обозревателя службы хранилища Azure](https://storageexplorer.com/).

1. Чтобы ссылаться на файл в контейнере BLOB-объектов, получите URL-адрес HTTPS, соответствующий этому формату с учетом регистра, из Обозревателя службы хранилища Azure:

   `https://<storage-account-name>.blob.core.windows.net/<blob-container-name>/<complete-swagger-file-name>?<query-parameters>`

## <a name="connector-reference"></a>Справочник по соединителям

Ниже приведены дополнительные сведения о выходных данных триггера или действия HTTP + Swagger. Вызов HTTP + Swagger возвращает следующие сведения:

| Имя свойства | Тип | Описание |
|---------------|------|-------------|
| Заголовки | объект | Заголовки из запроса |
| текст | объект | Объект JSON | Объект с содержимым текста из запроса |
| Код состояния | INT | Код состояния из запроса |
|||

| Код состояния | Описание |
|-------------|-------------|
| 200 | OK |
| 202 | Принято |
| 400 | Недопустимый запрос |
| 401 | Не авторизовано |
| 403 | Запрещено |
| 404 | Не найдено |
| 500 | Внутренняя ошибка сервера. Произошла неизвестная ошибка. |
|||

## <a name="next-steps"></a>Дальнейшие действия

* См. дополнительные сведения о других [соединителях Logic Apps](../connectors/apis-list.md).
