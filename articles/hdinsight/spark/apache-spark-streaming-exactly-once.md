---
title: Потоковая передача Spark & обработки событий в Azure HDInsight ровно один раз
description: Как настроить потоковую передачу Apache Spark для обработки события только один раз.
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: how-to
ms.date: 11/15/2018
ms.openlocfilehash: 4ba7df665b24a3eba2cd185d85a17bd0ef456b0b
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98929666"
---
# <a name="create-apache-spark-streaming-jobs-with-exactly-once-event-processing"></a>Создание заданий потоковой передачи Apache Spark со строго однократной обработкой событий

Приложения потоковой обработки принимают различные подходы к обработке повторной обработки сообщений после сбоя в системе:

* По крайней мере один раз. Каждое сообщение гарантированно обрабатывается, однако оно может обрабатываться более одного раза.
* Не более одного раза. Каждое сообщение может быть обработано или нет. Если сообщение обрабатывается, оно обрабатывается только один раз.
* Только один раз. Каждое сообщение гарантированно обрабатывается только один раз.

В этой статье показано, как настроить приложение потоковой передачи Spark для обработки только один раз.

## <a name="exactly-once-semantics-with-apache-spark-streaming"></a>Семантика для обработки только один раз в приложении потоковой передачи Apache Spark

Сначала рассмотрим, как все системные точки отказа перезагружаются после возникновения проблемы и как избежать потери данных. В приложении потоковой передачи Spark есть:

* источник входных данных;
* один получатель или несколько, которые извлекают данные из источника входных данных;
* задачи, обрабатывающие данные;
* приемник выходных данных;
* процесс-драйвер, управляющий долго выполняющимся заданием.

Семантика обработки только один раз требует, чтобы никакие данные не терялись в какой-либо точке, а обработка сообщений перезапускалась, независимо от того, где произошел сбой.

### <a name="replayable-sources"></a>Воспроизводимые источники

Источник, из которого приложение потоковой передачи Spark считывает события, должен быть *воспроизводимым*. Это означает, что в случаях, когда сообщение было извлечено, но затем в системе произошел сбой и сообщение не было сохранено и обработано, источник должен снова предоставить то же сообщение.

В Azure и Центры событий Azure, и [Apache Kafka](https://kafka.apache.org/) в HDInsight предоставляют воспроизводимые источники. Еще одним примером воспроизводимого источника является отказоустойчивая файловая система, такая как [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html), большие двоичные объекты службы хранилища Azure или Azure Data Lake Storage, где все данные хранятся неограниченно долго и могут быть повторно считаны в полном объеме.

### <a name="reliable-receivers"></a>Надежные приемники

В приложении потоковой передачи Spark такие источники, как Центры событий и Kafka, имеют *надежные приемники*, где каждый получатель отслеживает прогресс чтения из источника. Надежные получатели сохраняют свое состояние в отказоустойчивом хранилище (в [Apache ZooKeeper](https://zookeeper.apache.org/) или в контрольных точках потоковой передачи Spark, записанных в HDFS). Если такие приемники завершаются сбоем и позднее перезагружаются, они могут продолжить выполнение с места прерывания.

### <a name="use-the-write-ahead-log"></a>Использование упреждающего протоколирования

Потоковая передача Spark поддерживает использование упреждающего протоколирования, в котором каждое событие сначала записывается в каталог контрольных точек Spark в отказоустойчивом хранилище, а затем сохраняется в устойчивом распределенном наборе данных (RDD). В Azure отказоустойчивым хранилищем является служба хранилища Azure или Data Lake Storage на базе HDFS. В приложении потоковой передачи Spark можно включить упреждающее протоколирование для всех получателей, задав для параметра конфигурации `spark.streaming.receiver.writeAheadLog.enable` значение `true`. Упреждающее протоколирование обеспечивает отказоустойчивость в случае ошибок драйвера и исполнителя.

Для рабочих ролей, выполняющих задачи с данными событий, каждый набор данных RDD по определению реплицируется и распределяется между несколькими рабочими ролями. Если выполнение задачи завершилось сбоем из-за сбоя рабочей роли, то задача будет перезапущена на другом рабочем процессе с репликой данных события, поэтому событие не теряется.

### <a name="use-checkpoints-for-drivers"></a>Использование контрольных точек для драйверов

Драйверы заданий должны быть перезапускаемыми. Если происходит сбой драйвера, выполняющего приложение потоковой передачи Spark, вместе с ним отключаются все запущенные приемники, задачи и любые RDD, хранящие данные о событиях. В этом случае необходимо иметь возможность сохранить ход выполнения задания, чтобы возобновить его позже. Это достигается путем периодического назначения контрольных точек с использованием направленного ациклического графа (DAG) потока DStream и сохранения соответствующего состояния в отказоустойчивое хранилище. Метаданные DAG включают в себя конфигурацию, используемую для создания приложения потоковой передачи, операции, которые определяют приложение, и пакеты, которые поставлены в очередь, но еще не выполнены. Эти метаданные позволяют перезапустить завершившийся со сбоем драйвер на основе сведений о контрольных точках. При перезапуске драйвера запускаются новые приемники, которые самостоятельно восстанавливают данные о событиях в RDD из журнала упреждающего протоколирования.

Контрольные точки в приложении потоковой передачи Spark включаются в два этапа.

1. В объекте StreamingContext настройте путь к хранилищу контрольных точек:

    ```Scala
    val ssc = new StreamingContext(spark, Seconds(1))
    ssc.checkpoint("/path/to/checkpoints")
    ```

    В HDInsight эти контрольные точки нужно сохранить в хранилище по умолчанию, присоединенном к кластеру (службе хранилища Azure или Azure Data Lake Storage).

2. Затем укажите интервал создания контрольных точек (в секундах) в потоке DStream. Через определенный промежуток данные состояния, полученные из входного события, сохраняются в хранилище. Сохраненные данные состояния могут уменьшить количество вычислений, необходимых при восстановлении состояния из исходного события.

    ```Scala
    val lines = ssc.socketTextStream("hostname", 9999)
    lines.checkpoint(30)
    ssc.start()
    ssc.awaitTermination()
    ```

### <a name="use-idempotent-sinks"></a>Использование идемпотентных приемников

Приемник назначения, в который записываются результаты задания, должен иметь возможность обрабатывать ситуацию, когда она получает один и тот же результат более одного раза. Приемник должен определять такие повторяющиеся результаты и игнорировать их. *Идемпотентный* приемник может вызываться несколько раз с теми же данными без изменения состояния.

Идемпотентные приемники создаются путем реализации логики, которая сначала проверяет наличие входящего результата в хранилище данных. Если результат уже существует, запись должна выглядеть успешно с точки зрения задания Spark, но на самом деле хранилище данных игнорирует дубликаты данных. Если результат не существует, приемник должен вставить этот новый результат в хранилище.

Например, можно использовать хранимую процедуру с Базой данных SQL Azure, которая вставляет события в таблицу. Эта хранимая процедура сначала выполняет поиск событий по ключевым полям и вставляет запись в таблицу только в том случае, если соответствующее событие не найдено.

Можно также использовать секционированную файловую систему, например большие двоичные объекты службы хранилища Azure или хранилище Azure Data Lake Storage. В этом случае логике приемника не требуется проверять наличие файла. Если файл, представляющий событие, существует, он просто перезаписывается теми же данными. В противном случае по вычисленному пути создается новый файл.

## <a name="next-steps"></a>Дальнейшие действия

* [Общие сведения о потоковой передаче Apache Spark](apache-spark-streaming-overview.md)
* [Создание высокодоступных заданий потоковой передачи Apache Spark в Apache Hadoop YARN](apache-spark-streaming-high-availability.md)
