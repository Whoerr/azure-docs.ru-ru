---
title: Руководство. Обучение модели с помощью автоматизированного машинного обучения
description: В этом руководстве показано, как без написания кода обучить модель машинного обучения в Azure Synapse Analytics.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: aaf0aab2ef600b269b9b47182aeb096ca13c7a87
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98943521"
---
# <a name="tutorial-train-a-machine-learning-model-without-code"></a>Руководство. Обучение модели машинного обучения без написания кода

Вы можете обогатить данные в таблицах Spark с помощью новых моделей машинного обучения, обученных с использованием [автоматизированного машинного обучения](../../machine-learning/concept-automated-ml.md). Пользователь Azure Synapse Analytics может выбрать таблицу Spark в рабочей области, чтобы применить ее в качестве обучающего набора данных для создания моделей машинного обучения без необходимости писать код.

Из этого руководства вы узнаете, как обучать модели машинного обучения с помощью Synapse Studio без написания кода. Synapse Studio — это компонент Azure Synapse Analytics. 

Вы воспользуетесь функцией автоматизированного машинного обучения в Машинном обучении Azure, и при этом вам не нужно будет писать код. Тип модели, которую вам предстоит обучить, зависит от решаемой проблемы.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись, прежде чем начинать работу](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Предварительные требования

- [Рабочая область Azure Synapse Analytics.](../get-started-create-workspace.md) Убедитесь, что она включает учетную запись хранения Azure Data Lake Storage 2-го поколения, настроенную в качестве хранилища по умолчанию. При работе с файловой системой Data Lake Storage 2-го поколения убедитесь, что вы имеете права *участника данных Хранилища BLOB-объектов*.
- Пул Apache Spark в рабочей области Azure Synapse Analytics. Подробные сведения см. в статье [Краткое руководство. Создание выделенного пула SQL с помощью Synapse Studio](../quickstart-create-sql-pool-studio.md).
- Связанная служба Машинного обучения Azure в рабочей области Azure Synapse Analytics. Подробные сведения см. в статье [Краткое руководство. Создание связанной службы "Машинное обучение Azure" в Azure Synapse Analytics](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

## <a name="create-a-spark-table-for-the-training-dataset"></a>Создание таблицы Spark для обучающего набора данных

Для работы с этим руководством вам потребуется таблица Spark. Она создается следующей записной книжкой:

1. Скачайте записную книжку [Create-Spark-Table-NYCTaxi-Data.ipynb](https://go.microsoft.com/fwlink/?linkid=2149229).

1. Импортируйте эту записную книжку в Synapse Studio.
![Снимок экрана: Azure Synapse Analytics с выделенным пунктом "Импорт".](media/tutorial-automl-wizard/tutorial-automl-wizard-00a.png)

1. Выберите нужный пул Spark и щелкните **Запустить все**, чтобы получить данные о такси Нью-Йорка из открытого набора данных и сохранить их в базе данных Spark по умолчанию.
![Снимок экрана: Azure Synapse Analytics с выделенным элементом "Выполнить все" и базой данных Spark.](media/tutorial-automl-wizard/tutorial-automl-wizard-00b.png)

1. Когда выполнение этой записной книжки завершится, в базе данных Spark по умолчанию появится новая таблица Spark. В разделе **Данные** найдите таблицу с именем **nyc_taxi**.
![Снимок экрана: вкладка "Данные" Azure Synapse Analytics с выделенной новой таблицей.](media/tutorial-automl-wizard/tutorial-automl-wizard-00c.png)

## <a name="open-the-automated-machine-learning-wizard"></a>Открытые мастера автоматизированного машинного обучения

Чтобы открыть мастер, сделайте следующее:

1. Щелкните правой кнопкой мыши таблицу Spark, созданную на предыдущем шаге. Затем выберите **Машинное обучение** > **Enrich with new model** (Дополнить с помощью новой модели).
![Снимок экрана: таблица Spark с выделенными элементами "Машинное обучение" и "Дополнить".](media/tutorial-automl-wizard/tutorial-automl-wizard-00d.png)

1. Укажите сведения о конфигурации для создания запуска эксперимента автоматизированного машинного обучения в Машинном обучении Azure. При этом будет выполнено обучение нескольких моделей, из которых лучшая по результатам успешного выполнения будет внесена в реестр моделей Машинного обучения Azure.

   ![Снимок экрана: спецификации конфигурации для обучения модели машинного обучения.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00a.png)

    - **Рабочая область Машинного обучения Azure.** Рабочая область Машинного обучения Azure необходима для создания автоматического выполнения эксперимента машинного обучения. Вам также нужно связать рабочую область Azure Synapse Analytics с рабочей областью Машинного обучения Azure, используя [связанную службу](quickstart-integrate-azure-machine-learning.md). Когда все предварительные условия будут соблюдены, вы сможете указать рабочую область Машинного обучения Azure для этого автоматического запуска.

    - **Имя эксперимента.** Укажите имя эксперимента. При подготовке автоматического выполнения машинного обучения нужно указать имя эксперимента. Сведения о выполнении эксперимента сохраняются в рабочей области Машинного обучения Azure. Процедура создаст новый эксперимент по умолчанию и предложит для него имя, но вы можете также указать имя существующего эксперимента.

    - **Имя лучшей модели.** Укажите имя модели с лучшими результатами выполнения автоматического выполнения. Эта модель будет сохранена с указанным именем в реестре моделей Машинного обучения Azure по завершении выполнения. Автоматическое выполнение машинного обучения создает множество моделей машинного обучения. Эти модели будут сравниваться друг с другом по основной метрике, которую вы выберете на более позднем шаге, и из них будет выбрана наилучшая модель.

    - **Целевой столбец.** Это тот элемент, который модель будет учиться прогнозировать. Выберите столбец, для которого вы хотите создать прогноз. В этом руководстве в качестве целевого используется числовой столбец `fareAmount`.

    - **Пул Spark.** Укажите пул Spark для автоматического запуска эксперимента. Все вычисления выполняются в указанном пуле.

    - **Сведения о конфигурации Spark.** Помимо пула Spark, вы можете указать необязательные сведения о конфигурации сеанса.

1. Выберите **Continue** (Продолжить).

## <a name="choose-a-task-type"></a>Выбор типа задачи

Выберите тип модели машинного обучения для эксперимента в зависимости от того, на какой вопрос вы пытаетесь ответить. Так как `fareAmount` является целевым столбцом с числовым значением, мы рекомендуем выбрать вариант **Регрессия**. Затем выберите **Continue** (Продолжить).

![Снимок экрана: страница "Дополнение с использованием новой модели" с выделенным вариантом "Регрессия".](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00b.png)

## <a name="additional-configurations"></a>Дополнительные конфигурации

Если в предыдущем разделе выбрать вариант **Регрессия** или **Классификация** в качестве типа модели, будут доступны следующие конфигурации:

- **Основная метрика.** Укажите метрику, используемую для оценки работы модели. Эта метрика будет использоваться для сравнения нескольких моделей, созданных в ходе автоматического запуска, и выбора самой точной из них.

- **Training job time (hours)** (Время задания обучения (в часах)). Укажите максимальное время (в часах) для выполнения эксперимента и обучения моделей. Обратите внимание, что здесь можно указать значения меньше 1 (например, **0,5**).

- **Max concurrent iterations** (Максимальное число одновременных итераций). Выберите максимальное количество итераций, которые могут выполняться параллельно.

- **Совместимость с моделью ONNX.** Если этот параметр включен, модели, обученные автоматическим машинным обучением, преобразуются в формат ONNX. Это особенно важно, если вы хотите использовать модель для оценки в пулах SQL Azure Synapse Analytics.

Эти параметры имеют значения по умолчанию, которые можно настраивать.
![Снимок экрана: дополнительные конфигурации для настройки модели регрессии.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00c.png)

После завершения настройки можно запустить автоматическое выполнение. Можно выбрать вариант **Создать выполнение**, который запускает выполнение непосредственно без написания кода. Если вы предпочитаете использовать код, выберите **Открыть в записной книжке**. Этот вариант позволяет просмотреть код, который создает запуск, и выполнить записную книжку.

>[!NOTE]
>Если вы выбрали **Прогнозирование временных рядов** в качестве типа модели в предыдущем разделе, вам потребуется задать дополнительные настройки. Также режим прогнозирования не поддерживает совместимость с моделью ONNX.

### <a name="create-a-run-directly"></a>Непосредственное создание запуска

Чтобы непосредственно запустить автоматическое выполнение машинного обучения, выберите **Запустить выполнение**. Появится уведомление о запуске выполнения. После этого появится еще одно уведомление об успешном выполнении. Вы также можете проверить состояние в Машинном обучении Azure, щелкнув ссылку в уведомлении.
![Снимок экрана: уведомление об успешном выполнении.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00d.png)

### <a name="create-a-run-with-a-notebook"></a>Создание запуска с помощью записной книжки

Выберите **Открыть в записной книжке**, чтобы создать записную книжку. Затем выберите **Выполнить все**. Этот вариант позволяет добавить параметры для запуска автоматизированного машинного обучения.

![Снимок экрана: записная книжка с выделенным элементом "Запустить все".](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00e.png)

Когда выполнение будет успешно отправлено, в выходных данных записной книжки появится ссылка на запуск эксперимента в рабочей области Машинного обучения Azure. Щелкните эту ссылку, чтобы отслеживать автоматическое выполнение в Машинном обучении Azure.
![Снимок экрана: Azure Synapse Analytics с выделенной ссылкой.](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00f.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Руководство по Использование мастера оценки моделей машинного обучения (предварительная версия) для выделенных пулов SQL](tutorial-sql-pool-model-scoring-wizard.md)
- [Краткое руководство. Создание связанной службы "Машинное обучение Azure" в Azure Synapse Analytics](quickstart-integrate-azure-machine-learning.md)
- [Возможности Машинного обучения в Azure Synapse Analytics](what-is-machine-learning.md)