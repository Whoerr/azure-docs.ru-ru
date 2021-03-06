---
title: Перенос Apache Storm Azure HDInsight 3,6 в HDInsight 4,0 Apache Spark
description: Различия и последовательность миграции для миграции Apache Storm рабочих нагрузок в потоковую передачу Spark или структурированную потоковую передачу Spark.
ms.service: hdinsight
ms.topic: how-to
ms.date: 01/16/2019
ms.openlocfilehash: aa57c01558cfdcf069b17fad9e86f7640553dcfd
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944787"
---
# <a name="migrate-azure-hdinsight-36-apache-storm-to-hdinsight-40-apache-spark"></a>Перенос Apache Storm Azure HDInsight 3,6 в HDInsight 4,0 Apache Spark

В этом документе описывается перенос рабочих нагрузок Apache Storm в HDInsight 3,6 в HDInsight 4,0. HDInsight 4,0 не поддерживает тип кластера Apache Storm и вам потребуется выполнить миграцию на другую платформу потоковых данных. Двумя подходящими параметрами являются Apache Spark потоковая передача и структурированная потоковая передача Spark. В этом документе описаны различия между этими платформами, а также рекомендации по переносу рабочих нагрузок Apache Storm.

## <a name="storm-migration-paths-in-hdinsight"></a>Пути миграции в HDInsight

Если вы хотите выполнить миграцию с Apache Storm в HDInsight 3,6, у вас есть несколько вариантов:

* Потоковая передача Spark в HDInsight 4,0
* Структурированная потоковая передача Spark в HDInsight 4,0
* Azure Stream Analytics

Этот документ содержит инструкции по переходу с Apache Storm на потоковую передачу Spark и структурированную потоковую передачу Spark.

> [!div class="mx-imgBorder"]
> ![Путь миграции в HDInsight](./media/migrate-storm-to-spark/storm-migration-path.png)

## <a name="comparison-between-apache-storm-and-spark-streaming-spark-structured-streaming"></a>Сравнение потоковой передачи данных Apache Storm и Spark, структурированная потоковая передача Spark

Apache Storm может обеспечить различные уровни гарантированной обработки сообщений. Например, приложение базового уровня может гарантировать обработку по крайней мере один раз, и [Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html) может гарантировать только однократную обработку. Потоковая передача Spark и структурированная потоковая передача Spark гарантируют, что любое входное событие обрабатывается ровно один раз, даже если происходит сбой узла. Множество имеет модель, которая обрабатывает каждое отдельное событие, а также можно использовать микропакетную модель с Trident. Потоковая передача Spark и структурированная потоковая передача Spark предоставляют модель обработки Micro-Batch.

|  |Storm |Потоковая передача Spark | Структурированная потоковая передача Spark|
|---|---|---|---|
|**Гарантия обработки событий**|По крайней мере один раз <br> Ровно один раз (Trident) |[Только один раз](https://spark.apache.org/docs/latest/streaming-programming-guide.html)|[Только один раз](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)|
|**Обработка модели**|В режиме реального времени <br> Micro Batch (Trident) |Micro Batch |Micro Batch |
|**Поддержка времени событий**|[Да](https://storm.apache.org/releases/2.0.0/Windowing.html)|Нет|[Да](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)|
|**Языки**|Java и т. д.|Scala, Java, Python|Python, R, Scala, Java, SQL|

### <a name="spark-streaming-vs-spark-structured-streaming"></a>Потоковая передача Spark и структурированной потоковой передачи Spark

Структурированная потоковая передача Spark заменяет потоковую передачу Spark (DStreams). Структурированная потоковая передача будет продолжать получать улучшения и обслуживание, тогда как DStreams будет работать только в режиме обслуживания. **Примечание. для выделения этой точки требуются ссылки**. Структурированная потоковая передача не имеет столько функций, сколько DStreams для источников и приемников, которые он поддерживает, поэтому необходимо оценить требования, чтобы выбрать соответствующий параметр обработки потока Spark.

## <a name="streaming-single-event-processing-vs-micro-batch-processing"></a>Обработка потоковой передачи (одно событие) и обработка Micro-Batch

Множество предоставляет модель, которая обрабатывает каждое отдельное событие. Это означает, что все входящие записи будут обрабатываться сразу после их прибытия. Приложениям потоковой передачи Spark необходимо подождать долю секунды, чтобы собрать каждый микропакет событий, прежде чем отправить пакет на обработку. В свою очередь, приложение на основе событий обрабатывает каждое событие сразу же. Задержка при потоковой передаче Spark, как правило, не превышает несколько секунд. Преимущества подхода с использованием микропакетов заключаются в более эффективной обработке данных и более простых статистических вычислениях.

> [!div class="mx-imgBorder"]
> ![Потоковая передача и микропакетная обработка](./media/migrate-storm-to-spark/streaming-and-micro-batch-processing.png)

## <a name="storm-architecture-and-components"></a>Архитектура и компоненты

Топологии Storm состоят из нескольких компонентов, которые расположены в виде направленного ациклического графа (DAG). На указанной ниже схеме показаны потоки данных между компонентами. Каждый компонент использует один или несколько потоков данных и может генерировать дополнительные потоки.

|Компонент |Описание |
|---|---|
|Spout|Переносит данные в топологию. Они генерируют один или несколько потоков в топологии.|
|Молни|Потребляет потоки, выдаваемые из порождений или других «винтов». Элементы bolt могут дополнительно генерировать потоки в топологии. Они также отвечают за запись данных во внешние службы или хранилище, например HDFS, Kafka или HBase.|

> [!div class="mx-imgBorder"]
> ![взаимодействие компонентов, составляющих множество](./media/migrate-storm-to-spark/apache-storm-components.png)

Множество состоит из следующих трех управляющих программ, которые сохраняют работоспособность кластера.

|Управляющая программа |Описание |
|---|---|
|Nimbus;|Как и в Hadoop JobTracker, он отвечает за распространение кода вокруг кластера и назначение задач компьютерам и мониторинг сбоев.|
|Zookeeper|Используется для координации кластеров.|
|Supervisor;|Прослушивает работу, назначенную компьютеру, и запускает и останавливает рабочие процессы на основе директив из Nimbus. Каждый рабочий процесс выполняет подмножество топологии. Логика приложения пользователя (выкрутка и молния) выполняется здесь.|

> [!div class="mx-imgBorder"]
> ![управляющие программы Nimbus, Zookeeper и супервизора](./media/migrate-storm-to-spark/nimbus-zookeeper-supervisor.png)

## <a name="spark-streaming-architecture-and-components"></a>Архитектура и компоненты потоковой передачи Spark

Ниже описано, как компоненты работают совместно в потоковой передаче Spark (DStreams) и структурированной потоковой передаче Spark.

* При запуске потоковой передачи Spark драйвер запускает задачу в исполнителе.
* Исполнитель получает поток из источника потоковых данных.
* Когда исполнитель получает потоки данных, он разделяет поток на блоки и сохраняет их в памяти.
* Блоки данных реплицируются в другие исполнители.
* Обработанные данные затем сохраняются в целевом хранилище данных.

> [!div class="mx-imgBorder"]
> ![путь потоковой передачи Spark к выходу](./media/migrate-storm-to-spark/spark-streaming-to-output.png)

## <a name="spark-streaming-dstream-workflow"></a>Рабочий процесс потоковой передачи Spark (DStream)

По окончании каждого интервала пакетов создается новый набор RDD, содержащий все данные из этого интервала. Непрерывные наборы RDD собираются в DStream. Например, если интервал пакетов составляет одну секунду, ваш поток DStream каждую секунду выдает пакет, содержащий один набор RDD со всеми данными, полученными на протяжении этой секунды. При обработке потока DStream событие температуры появляется в одном из этих пакетов. Приложение потоковой передачи Spark обрабатывает пакеты, содержащие события, и в конечном счете работает с данными, хранящимися в каждом наборе RDD.

> [!div class="mx-imgBorder"]
> ![пакеты обработки потоковой передачи Spark](./media/migrate-storm-to-spark/spark-streaming-batches.png)

Дополнительные сведения о различных преобразованиях, доступных для потоковой передачи Spark, см. [в разделе Transforms on DStreams](https://spark.apache.org/docs/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="spark-structured-streaming"></a>Структурированная потоковая передача Spark

Структурированная потоковая передача Spark представляет поток данных в виде таблицы, которая не привязана по глубине. Таблица продолжит расти по мере поступления новых данных. Эта входная таблица непрерывно обрабатывается долго выполняющимся запросом, а результаты отправляются в выходную таблицу.

В структурированной потоковой передаче данные передаются в систему и сразу же поступают во входную таблицу. Вы можете написать запросы с помощью API кадров и наборов данных для выполнения операций в этой входной таблице.

Выходные данные запроса дают *таблицу результатов*, которая содержит результаты запроса. Вы можете рисовать данные из таблицы результатов для внешнего хранилища данных, такого как реляционная база.

С помощью интервала триггера задается расписание для обработки данных во входной таблице. По умолчанию интервал триггера равен нулю, поэтому структурированная потоковая передача пытается обрабатывать данные по мере их поступления. На практике это означает, что как только структурированная потоковая передача завершает обработку выполнения предыдущего запроса, она запускает следующую обработку новых полученных данных. Можно настроить запуск триггера с интервалом, что позволит обрабатывать данные потоковой передачи в пакетах на основе времени.

> [!div class="mx-imgBorder"]
> ![обработка данных в структурированной потоковой передаче](./media/migrate-storm-to-spark/structured-streaming-data-processing.png)

> [!div class="mx-imgBorder"]
> ![модель программирования для структурированной потоковой передачи](./media/migrate-storm-to-spark/structured-streaming-model.png)

## <a name="general-migration-flow"></a>Общий поток миграции

Рекомендуемый поток переноса от, от, до Spark, предполагает следующую начальную архитектуру:

* Kafka используется в качестве источника потоковых данных
* Kafka и в одной виртуальной сети развернуты
* Данные, обрабатываемые с помощью, записываются в приемник данных, например в службу хранилища Azure или Azure Data Lake Storage 2-го поколения.

    > [!div class="mx-imgBorder"]
    > ![Схема предполагаемой текущей среды](./media/migrate-storm-to-spark/presumed-current-environment.png)

Чтобы перенести приложение из числа в один из API потоковой передачи Spark, выполните следующие действия.

1. **Разверните новый кластер.** Разверните новый кластер HDInsight 4,0 Spark в той же виртуальной сети и разверните приложение потоковой передачи Spark или приложения структурированной потоковой передачи Spark на нем и тщательно протестируйте его.

    > [!div class="mx-imgBorder"]
    > ![новое развертывание Spark в HDInsight](./media/migrate-storm-to-spark/new-spark-deployment.png)

1. **Перестает потребляться в старом кластере.** В существующем выходе, закройте использование данных из источника потоковых данных и дождитесь завершения записи данных в целевой приемник.

    > [!div class="mx-imgBorder"]
    > ![запретить использование текущего кластера](./media/migrate-storm-to-spark/stop-consuming-current-cluster.png)

1. **Начните потреблять в новом кластере Spark.** Запустите потоковую передачу данных из вновь развернутого кластера HDInsight 4,0 Spark. В настоящее время процесс принимается за счет использования последнего смещения Kafka.

    > [!div class="mx-imgBorder"]
    > ![начать использование нового кластера](./media/migrate-storm-to-spark/start-consuming-new-cluster.png)

1. **При необходимости удалите старый кластер.** После завершения и правильной работы коммутатора при необходимости удалите старый кластер HDInsight 3,6.

    > [!div class="mx-imgBorder"]
    > ![удалять старые кластеры HDInsight по мере необходимости](./media/migrate-storm-to-spark/remove-old-clusters1.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о структуризации, потоковой передаче Spark и структурированной потоковой передаче Spark см. в следующих документах:

* [Overview of Apache Spark Streaming](../spark/apache-spark-streaming-overview.md) (Общие сведения о потоковой передаче Apache Spark)
* [Overview of Apache Spark Structured Streaming](../spark/apache-spark-structured-streaming-overview.md) (Обзор структурированной потоковой передачи Apache Spark)
