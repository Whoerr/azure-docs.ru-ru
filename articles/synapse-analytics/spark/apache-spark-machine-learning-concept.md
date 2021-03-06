---
title: Машинное обучение с Apache Spark
description: В этой статье приводятся общие сведения о возможностях машинного обучения и обработки и анализа данных, доступных в Apache Spark в Azure синапсе Analytics.
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 11/13/2020
ms.author: midesa
ms.reviewer: jrasnick
ms.openlocfilehash: 0485f697b9360b0f2dfe94fdf07629978b5127c1
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98116931"
---
# <a name="machine-learning-with-apache-spark"></a>Машинное обучение с Apache Spark

Apache Spark в Azure синапсе Analytics обеспечивает машинное обучение с большими данными, предоставляя возможность получения ценных сведений из больших объемов структурированных, неструктурированных и быстро перемещаемых данных. 

В этом разделе содержится обзор и учебники по рабочим процессам машинного обучения, включая анализ исследовательских данных, проектирование признаков, обучение модели, оценку модели и развертывание.  

## <a name="synapse-runtime"></a>Среда выполнения синапсе 
Среда выполнения синапсе — это проверенная среда, оптимизированная для обработки и анализа данных и машинного обучения. Среда выполнения синапсе предлагает ряд популярных библиотек с открытым исходным кодом и сборки в Машинное обучение Azure SDK по умолчанию. Среда выполнения синапсе также включает множество внешних библиотек, включая PyTorch, Scikit-учиться, XGBoost и многое другое.

Дополнительные сведения о доступных библиотеках и соответствующих версиях см. в опубликованной [среде выполнения Azure синапсе Analytics](../spark/apache-spark-version-support.md).

## <a name="exploratory-data-analysis"></a>Анализ исследовательских данных
При использовании Apache Spark в Azure синапсе Analytics доступны различные встроенные параметры, которые помогут вам визуализировать данные, в том числе параметры Синапсеной записной книжки, доступ к популярным библиотекам с открытым исходным кодом, таким как Сеаборн и Matplotlib, а также интеграцию с синапсе SQL и Power BI.

Дополнительные сведения о возможностях визуализации данных и анализа данных см. в статье как [визуализировать данные с помощью записных книжек Azure синапсе](../spark/apache-spark-data-visualization.md).

## <a name="feature-engineering"></a>Проектирование признаков
По умолчанию среда выполнения синапсе включает набор библиотек, которые обычно используются для проектирования признаков. Для больших наборов данных можно использовать Spark SQL, MLlib и Коалас для проектирования признаков. Для небольших наборов данных сторонние библиотеки, такие как NumPy, Pandas и Scikit, также предоставляют полезные методы для этих сценариев.

## <a name="train-models"></a>Обучение моделей
Существует несколько вариантов обучения моделей машинного обучения с помощью Azure Spark в Azure синапсе Analytics: Apache Spark MLlib, Машинное обучение Azure и различные библиотеки с открытым исходным кодом. 

Дополнительные сведения о возможностях машинного обучения см. в статье [обучение моделей в Azure синапсе Analytics](../spark/apache-spark-machine-learning-training.md).

### <a name="sparkml-and-mllib"></a>SparkM и MLlib
Возможности распределенного вычисления в памяти Spark отлично подходят для итеративных алгоритмов, используемых в машинном обучении и графовых вычислениях. ```spark.ml``` предоставляет универсальный набор интерфейсов API высокого уровня, помогающих пользователям создавать и настраивать конвейеры машинного обучения. Дополнительные сведения о ```spark.ml``` см. в [Apache Sparkном руководством по программированию для машинного обучения](https://spark.apache.org/docs/1.2.2/ml-guide.html).

### <a name="azure-machine-learning-automated-ml"></a>Машинное обучение Azure автоматизированного ML
[Машинное обучение Azure автоматизированного ML](../../machine-learning/concept-automated-ml.md) (автоматизированное машинное обучение) помогает автоматизировать процесс разработки моделей машинного обучения. С его помощью специалисты по обработке и анализу данных могут создавать модели машинного обучения с высокой масштабируемостью, эффективностью и производительностью, сохраняя при этом качество модели. Компоненты для запуска Машинное обучение Azure автоматизированного пакета SDK для машинного обучения создаются непосредственно в среде выполнения синапсе.

### <a name="open-source-libraries"></a>Библиотеки с открытым исходным кодом
Каждый пул Apache Spark в Azure синапсе Analytics поставляется с набором предварительно загруженных и популярных библиотек машинного обучения.  Ниже перечислены некоторые из соответствующих библиотек машинного обучения, включенных по умолчанию.

- [Scikit —](https://scikit-learn.org/stable/index.html) это одна из самых популярных библиотек машинного обучения с одним узлом для классических алгоритмов ml. Scikit-Learning поддерживает большинство защищенных и неконтролируемых алгоритмов обучения, которые также можно использовать для интеллектуального анализа данных и анализа данных.
  
- [XGBoost](https://xgboost.readthedocs.io/en/latest/) — это популярная библиотека машинного обучения, которая содержит оптимизированные алгоритмы для учебных деревьев решений и случайных лесов. 
  
- [PyTorch](https://pytorch.org/)  &  [Tensorflow](https://www.tensorflow.org/) — это мощные библиотеки глубокого обучения Python. В пуле Apache Spark в Azure синапсе Analytics эти библиотеки можно использовать для создания моделей с одним компьютером, установив количество исполнителей в пуле равным нулю. Несмотря на то, что Apache Spark не работает под этой конфигурацией, это простой и экономичный способ создания моделей с одним компьютером.

## <a name="track-model-development"></a>Отслеживание разработки модели
[Млфлов](https://www.mlflow.org/) — это библиотека с открытым исходным кодом для управления жизненным циклом экспериментов машинного обучения. Отслеживание Млфлов — это компонент Млфлов, который регистрирует и отслеживает метрики и артефакты обучающего запуска. Дополнительные сведения об использовании отслеживания Млфлов в Azure синапсе Analytics и Машинное обучение Azure см. в этом руководстве по [использованию млфлов](../../machine-learning/how-to-use-mlflow.md).

## <a name="model-scoring"></a>Оценка модели
Оценка модели или выработка — это этап, в котором модель используется для создания прогнозов. Для оценки модели с помощью SparkML или MLLib можно использовать собственные методы Spark для выполнения определения непосредственно в кадре данных Spark. Для других библиотек с открытым исходным кодом и типов моделей можно также создать набор UDF Spark для горизонтального масштабирования в больших наборах данных. Для небольших наборов данных можно также использовать собственные методы определения модели, предоставляемые библиотекой.

## <a name="register-and-serve-models"></a>Регистрация и обслуживание моделей
Регистрация модели позволяет хранить, отменять и записывать метаданные о моделях в рабочей области. После завершения обучения модели можно зарегистрировать модель в [реестре машинное обучение Azure Model](../../machine-learning/concept-model-management-and-deployment.md#register-package-and-deploy-models-from-anywhere). После регистрации можно также использовать модели ONNX, чтобы [расширить данные,](../machine-learning/tutorial-sql-pool-model-scoring-wizard.md) хранящиеся в выделенных пулах SQL.

## <a name="next-steps"></a>Дальнейшие действия
Чтобы приступить к работе с машинным обучением в Azure синапсе Analytics, ознакомьтесь со следующими учебниками:
- [Анализ данных с помощью записных книжек синапсе Azure](../spark/apache-spark-data-visualization-tutorial.md)

- [Обучение модели машинного обучения с помощью автоматизированного ML](../spark/apache-spark-azure-machine-learning-tutorial.md)

- [Обучение модели машинного обучения с помощью Apache Spark MLlib](../spark/apache-spark-machine-learning-mllib-notebook.md)
