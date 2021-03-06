---
title: Интеграция Azure Stream Analytics с Машинное обучение Azure Studio (классическая модель)
description: В этой статье описывается, как быстро настроить простое Azure Stream Analytics задание, которое интегрирует Машинное обучение Azure Studio (классическая модель) с помощью определяемой пользователем функции.
author: krishna0815
ms.author: krishmam
ms.service: stream-analytics
ms.topic: how-to
ms.date: 08/12/2020
ms.custom: seodec18
ms.openlocfilehash: d7d74887b4b4e205645a45fa7e07768a1af73940
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98015306"
---
# <a name="do-sentiment-analysis-with-azure-stream-analytics-and-azure-machine-learning-studio-classic"></a>Анализ тональности с помощью Azure Stream Analytics и Машинное обучение Azure Studio (классическая модель)

В этой статье показано, как настроить простое задание Azure Stream Analytics, которое использует Машинное обучение Azure Studio (классическая модель) для анализа тональности. Модель тональности Analytics в студии (классическая) используется из Cortana Intelligence Gallery для анализа текстовых данных потоковой передачи и определения оценки тональности.

> [!TIP]
> Для повышения производительности и надежности настоятельно рекомендуется вместо UDF Студии машинного обучения Azure (классической) использовать [UDF машинного обучения Azure](machine-learning-udf.md).

Вы сможете применить сведения, полученные в рамках этой статьи, в следующих сценариях:

* при анализе тональности в режиме реального времени в потоковых данных Twitter;
* при анализе записей разговоров клиента со специалистами службы поддержки;
* при оценке комментариев на форумах, в блогах и видео;
* а также во многих других сценариях прогнозной оценки в реальном времени.

Задание Streaming Analytics, которое вы создаете, применяет модель анализа тональности как определяемую пользователем функцию к образцу текстовых данных из хранилища BLOB-объектов. Выходные данные (результат анализа тональности) записываются в то же хранилище BLOB-объектов в другом CSV-файле. 

## <a name="prerequisites"></a>Предварительные требования

Чтобы начать, у вас должны быть следующие компоненты:

* Активная подписка Azure.

* CSV-файл с некоторыми данными Twitter. Вы можете скачать пример файла из [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv)или создать собственный файл. В реальном сценарии вы получите данные напрямую из потока данных Twitter.

## <a name="create-a-storage-container-and-upload-the-csv-input-file"></a>Создание контейнера хранилища и передача входного CSV-файла

На этом шаге вы отправите CSV-файл в контейнер хранилища.

1. В портал Azure выберите **создать ресурс**  >  **хранилище**  >  **учетная запись хранения**.

2. Заполните вкладку " *основы* " следующими сведениями и оставьте значения по умолчанию для остальных полей:

   |Поле  |Значение  |
   |---------|---------|
   |Подписка|Выберите свою подписку.|
   |Группа ресурсов|Выберите группу ресурсов.|
   |Имя учетной записи хранения|Введите имя учетной записи хранения. Это имя должно быть уникальным в пределах Azure.|
   |Расположение|Выберите расположение. Все ресурсы должны использовать одно и то же расположение.|
   |Тип учетной записи|BlobStorage|

   ![указание сведений об учетной записи хранения](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account1.png)

3. Выберите **Review + Create** (Просмотреть и создать). Затем выберите **создать** , чтобы развернуть учетную запись хранения.

4. После завершения развертывания перейдите к своей учетной записи хранения. В колонке **Служба BLOB-объектов** щелкните элемент **Контейнеры**. Затем выберите **+ контейнер** , чтобы создать новый контейнер.

   ![Создание контейнера хранилища BLOB-объектов для входных данных](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account2.png)

5. Укажите имя контейнера и убедитесь, что для **общего уровня доступа** задано значение **частный**. Когда все будет готово, выберите **Создать**.

   ![указание сведений о контейнере больших двоичных объектов](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account3.png)

6. Перейдите к только что созданному контейнеру и выберите **Отправить**. Отправьте загруженный ранее файл **sampleinput.csv** .

   ![Кнопка передачи для контейнера](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Добавление модели анализа тональности из коллекции Cortana Intelligence

Теперь, когда образец данных находится в большом двоичном объекте, вы можете включить модель анализа тональности в коллекции Cortana Intelligence.

1. Перейдите на страницу [модели прогнозной аналитики тональности](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) в коллекции Cortana Intelligence.  

2. Выберите **Открыть в студии (классическая модель)**.  
   
   ![Stream Analytics Машинное обучение Azure Studio (классическая модель), Open Studio (классическая модель)](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Выполните вход, чтобы перейти в рабочую область. Выберите расположение.

4. В нижней части страницы щелкните **запустить** . Процесс выполнения занимает около минуты.

   ![Запуск эксперимента в студии (классическая модель)](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. После успешного выполнения процесса выберите кнопку **Deploy Web Service** (Развернуть веб-службу) в нижней области страницы.

   ![Развертывание эксперимента в студии (классическая модель) в качестве веб-службы](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. Чтобы проверить готовность модели аналитики тональности к использованию, нажмите кнопку **проверить** . Введите текст "Мне нравится Майкрософт".

   ![Тестовый эксперимент в студии Studio (классическая модель)](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

   Если проверка завершится успешно, вы увидите результат, аналогичный следующему:

   ![Результаты тестирования в студии (классическая модель)](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. В столбце **приложения** выберите ссылку на **книгу Excel 2010 или более раннюю** , чтобы скачать книгу Excel. Книга содержит ключ API и URL-адрес, которые понадобятся вам позже для настройки задания Stream Analytics.

    ![Stream Analytics Машинное обучение Azure Studio (классическая модель), краткий обзор](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-studio-classic-model"></a>Создание задания Stream Analytics, в котором используется модель студии (классическая)

Теперь вы можете создать задание Stream Analytics, считывающее пример твитов из CSV-файла в хранилище BLOB-объектов.

### <a name="create-the-job"></a>Создание задания

Перейдите к [портал Azure](https://portal.azure.com) и создайте задание Stream Analytics. Если вы не знакомы с этим процессом, см. статью [создание Stream Analytics задания с помощью портал Azure](stream-analytics-quick-create-portal.md).

### <a name="configure-the-job-input"></a>Настройка входных данных для задания

Задание получает входные данные из CSV-файла, переданного ранее в хранилище BLOB-объектов.

1. Перейдите в задание Stream Analytics. В разделе **топология задания** выберите параметр **входные данные** . Выберите **добавить потоковый входное**  > **хранилище BLOB-объектов**.

2. Заполните сведения о **хранилище BLOB-объектов** следующими значениями:

   |Поле  |Значение  |
   |---------|---------|
   |Псевдоним входных данных|Введите имя для входа. Запомните этот псевдоним при написании запроса.|
   |Подписка|Выберите свою подписку.|
   |Учетная запись хранения|Выберите учетную запись хранения, созданную на предыдущем шаге.|
   |Контейнер|Выберите контейнер, созданный на предыдущем шаге.|
   |Формат сериализации событий|CSV|

3. Щелкните **Сохранить**.

### <a name="configure-the-job-output"></a>Настройка выходных данных для задания

Задание передает результаты в то же хранилище BLOB-объектов, в котором получает входные данные.

1. Перейдите в задание Stream Analytics. В разделе **топология задания** выберите параметр **выходные данные** . Выберите **Добавить**  >  **хранилище BLOB-объектов**.

2. Заполните форму **хранилища BLOB-объектов** следующими значениями:

   |Поле  |Значение  |
   |---------|---------|
   |Псевдоним входных данных|Введите имя для входа. Запомните этот псевдоним при написании запроса.|
   |Подписка|Выберите свою подписку.|
   |Учетная запись хранения|Выберите учетную запись хранения, созданную на предыдущем шаге.|
   |Контейнер|Выберите контейнер, созданный на предыдущем шаге.|
   |Формат сериализации событий|CSV|

3. Щелкните **Сохранить**.

### <a name="add-the-studio-classic-function"></a>Добавление функции Studio (классической)

Ранее вы опубликовали модель Studio (классической) в веб-службе. В нашем сценарии выполняемое задание Stream Analytics отправляет каждый пример твита из входных данных в веб-службу для анализа тональности. Веб-служба Studio (классическая) возвращает тональности ( `positive` , `neutral` или) и вероятность того, что `negative` твит будет положительным.

В этом разделе вы определите функцию в задании Stream Analysis. Эту функцию можно вызывать для отправки твита в веб-службу и получения ответа.

1. У вас должны быть URL-адрес веб-службы и ключ API в скачанной ранее книге Excel.

2. Перейдите в задание Stream Analytics. Затем выберите **функции**  >  **+ добавить** студию  >  **машинного обучения Azure** .

3. Заполните форму **функции машинное обучение Azure** следующими значениями:

   |Поле  |Значение  |
   |---------|---------|
   | Псевдоним функции | Используйте имя `sentiment` и выберите **указать параметры машинное обучение Azure функции вручную**, что дает возможность ввести URL-адрес и ключ.      |
   | URL-адрес| Вставьте URL-адрес веб-службы.|
   |Ключ | Вставьте ключ API. |

4. Щелкните **Save** (Сохранить).

### <a name="create-a-query-to-transform-the-data"></a>Создание запроса для преобразования данных

Stream Analytics использует декларативный запрос на основе SQL для проверки входных данных и их обработки. В этом разделе вы создадите запрос, который считывает каждый твит из входных данных, а затем вызывает функцию Studio (классическая) для выполнения анализа тональности. Затем запрос отправляет результат в приемник выходных данных (хранилище BLOB-объектов).

1. Вернитесь к обзору Stream Analyticsного задания.

2. В разделе **Топология задания** выберите **Запрос**.

3. Введите следующий запрос:

    ```SQL
    WITH sentiment AS (  
    SELECT text, sentiment1(text) as result 
    FROM <input>  
    )  

    SELECT text, result.[Score]  
    INTO <output>
    FROM sentiment  
    ```    

   Запрос вызывает функцию, созданную ранее ( `sentiment` ) для выполнения анализа тональности для каждого твита во входных данных.

4. Нажмите кнопку **сохранить** , чтобы сохранить запрос.

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Запуск задания Stream Analytics и просмотр выходных данных

Теперь вы можете запустить задание Stream Analytics.

### <a name="start-the-job"></a>Запустите задание

1. Вернитесь к обзору Stream Analyticsного задания.

2. Нажмите кнопку **начать** в верхней части страницы.

3. В поле **Запуск задания** выберите **Настраиваемый**, а затем выберите один день до передачи CSV-файла в хранилище BLOB-объектов. Когда все будет готово, нажмите кнопку **Запуск**.  

### <a name="check-the-output"></a>Просмотр выходных данных

1. Задание может выполняться несколько минут, прежде чем вы увидите выполнение этой операции в поле **Мониторинг**.

2. Если у вас есть средство, которое обычно используется для проверки содержимого хранилища BLOB-объектов, используйте это средство для проверки контейнера. Кроме того, на портале Azure выполните следующие действия:

      1. В портал Azure найдите учетную запись хранения и в учетной записи найдите контейнер. Вы увидите в нем два файла: файл, содержащий образец твитов, и CSV-файл, созданный заданием Stream Analytics.
      2. Щелкните правой кнопкой мыши созданный файл, а затем выберите **Скачать**.

3. Откройте созданный CSV-файл. Вы должны увидеть примерно следующее:  

   ![Stream Analytics Машинное обучение Azure Studio (классическая модель), представление CSV](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

### <a name="view-metrics"></a>Просмотр метрик

Вы также можете просматривать метрики, связанные с функциями Studio (классическая модель). Следующие метрики, связанные с функциями, отображаются в поле **мониторинг** в обзоре задания.

* **Запросы функций** указывают количество запросов, отправленных в веб-службу Studio (классическая).  
* **События функций** отображает количество событий в запросе. По умолчанию каждый запрос к веб-службе Studio (классической) содержит до 1 000 событий.

## <a name="next-steps"></a>Дальнейшие действия

* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Справочник по языку запросов Azure Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference)
* [Интеграция REST API и Машинное обучение Studio (классическая модель)](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [Справочник по API-интерфейсу REST управления Stream Analytics](/rest/api/streamanalytics/)