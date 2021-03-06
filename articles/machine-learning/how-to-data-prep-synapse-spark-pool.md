---
title: Подготовка данных с помощью пулов Spark (Предварительная версия)
titleSuffix: Azure Machine Learning
description: Узнайте, как подключить пулы Spark для подготовки данных с помощью Azure синапсе и Машинное обучение Azure
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: nibaccam
ms.reviewer: nibaccam
ms.date: 03/02/2021
ms.custom: how-to, devx-track-python, data4ml
ms.openlocfilehash: 87e03b6aee122c5a26d4388ca8b570aa6cdf7b55
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101662889"
---
# <a name="attach-synapse-spark-pools-for-data-preparation-with-azure-synapse-preview"></a>Подключение пулов синапсе Spark к подготовке данных с помощью Azure синапсе (Предварительная версия)

Из этой статьи вы узнаете, как присоединить и запустить пул Apache Spark, поддерживаемый [Azure синапсе](/synapse-analytics/overview-what-is.md) для подготовки данных. 

>[!IMPORTANT]
> Машинное обучение Azure и интеграция Azure синапсе находятся на этапе предварительной версии. Возможности, представленные в этой статье, `azureml-synapse` применяют пакет, который содержит функции [экспериментальной](/python/api/overview/azure/ml/?preserve-view=true&view=azure-ml-py#stable-vs-experimental) предварительной версии, которые могут изменяться в любое время.

## <a name="azure-machine-learning-and-azure-synapse-integration-preview"></a>Машинное обучение Azure и интеграция Azure синапсе (Предварительная версия)

Интеграция Azure синапсе с Машинное обучение Azure (Предварительная версия) позволяет подключать пул Apache Spark, поддерживаемый Azure синапсе для интерактивного исследования и подготовки данных. Благодаря этой интеграции вы можете использовать выделенные ресурсы для подготовки данных в масштабе, все они находятся в той же записной книжке Python, которая используется для обучения моделей машинного обучения.

## <a name="prerequisites"></a>Предварительные требования

* [Создайте рабочую область Машинного обучения Azure](how-to-manage-workspace.md?tabs=python).

* [Создайте рабочую область синапсе в портал Azure](../synapse-analytics/quickstart-create-workspace.md).

* [Создание пула Apache Spark с помощью портал Azure, веб-средств или синапсе Studio](../synapse-analytics/quickstart-create-apache-spark-pool-portal.md)

* [Установите пакет SDK для машинное обучение Azure Python](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py) , включающий `azureml-synapse` пакет (Предварительная версия). 
    * Его также можно установить самостоятельно, но он совместим только с пакетами SDK 1,20 или более поздней версии. 
        ```python
        pip install azureml-synapse
        ```

## <a name="link-machine-learning-workspace-and-synapse-assets"></a>Связывание рабочей области машинного обучения и ресурсов синапсе

Прежде чем подключить пул синапсе Spark для подготовки данных, Рабочая область Машинное обучение Azure должна быть связана с рабочей областью синапсе для Azure. 

Вы можете связать рабочую область ML и рабочую область синапсе с помощью [пакета SDK для Python](#link-sdk) или [машинное обучение Azure Studio](#link-studio). 

> [!IMPORTANT]
> Для успешного связывания с рабочей областью синапсе необходимо иметь роль **владельца** рабочей области синапсе. Проверьте доступ в [портал Azure](https://ms.portal.azure.com/).
>
> Если вы не являетесь **владельцем** рабочей области синапсе, но хотите использовать существующую связанную службу, см. статью [Получение существующей связанной службы](#get-an-existing-linked-service).


<a name="link-sdk"></a>
### <a name="link-workspaces-with-the-python-sdk"></a>Связывание рабочих областей с пакетом SDK для Python

В следующем коде [`LinkedService`](/python/api/azureml-core/azureml.core.linked_service.linkedservice?preserve-view=true&view=azure-ml-py) классы и используются [`SynapseWorkspaceLinkedServiceConfiguration`](/python/api/azureml-core/azureml.core.linked_service.synapseworkspacelinkedserviceconfiguration?preserve-view=true&view=azure-ml-py) в, 

* Свяжите рабочую область машинного обучения `ws` с рабочей областью Azure синапсе. 
* Зарегистрируйте рабочую область синапсе с Машинное обучение Azure в качестве связанной службы.

``` python
import datetime  
from azureml.core import Workspace, LinkedService, SynapseWorkspaceLinkedServiceConfiguration

# Azure Machine Learning workspace
ws = Workspace.from_config()

#link configuration 
synapse_link_config = SynapseWorkspaceLinkedServiceConfiguration(
    subscription_id=ws.subscription_id,
    resource_group= 'your resource group',
    name='mySynapseWorkspaceName')

# Link workspaces and register Synapse workspace in Azure Machine Learning
linked_service = LinkedService.register(workspace = ws,              
                                            name = 'synapselink1',    
                                            linked_service_config = synapse_link_config)
```
> [!IMPORTANT] 
> Управляемое удостоверение `system_assigned_identity_principal_id` создается для каждой связанной службы. Перед началом сеанса синапсе для этого управляемого удостоверения необходимо предоставить роль **администратора синапсе Apache Spark** рабочей области синапсе. [Назначьте управляемому удостоверению роль администратора синапсе Apache Spark в синапсе Studio](../synapse-analytics/security/how-to-manage-synapse-rbac-role-assignments.md).
>
> Чтобы найти `system_assigned_identity_principal_id` конкретную связанную службу, используйте `LinkedService.get('<your-mlworkspace-name>', '<linked-service-name>')` .

<a name="link-studio"></a>
### <a name="link-workspaces-via-studio"></a>Связывание рабочих областей с помощью студии

Свяжите рабочую область машинного обучения и рабочую область синапсе с помощью Машинное обучение Azure Studio, выполнив следующие действия. 

1. Войдите в [машинное обучение Azure Studio](https://ml.azure.com/).
1. Выберите **связанные службы** в разделе **Управление** левой панели.
1. Выберите **добавить интеграцию**.
1. В форме " **связать рабочую область** " заполните поля

   |Поле| Описание    
   |---|---
   |Имя| Укажите имя для связанной службы. Это имя будет использоваться для ссылки на эту конкретную связанную службу.
   |имя подписки; | Выберите имя подписки, связанное с рабочей областью машинного обучения. 
   |Рабочая область синапсе | Выберите рабочую область синапсе, с которой необходимо установить связь. 
   
1. Нажмите кнопку **Далее** , чтобы открыть форму **Выбор пулов Spark (необязательно)** . В этой форме вы выберите пул синапсе Spark для присоединения к рабочей области.

1. Нажмите кнопку **Далее** , чтобы открыть форму **проверки** и проверить выбранные параметры. 
1. Выберите **создать** , чтобы завершить процесс создания связанной службы.

## <a name="get-an-existing-linked-service"></a>Получение существующей связанной службы

Для получения и использования существующей связанной службы требуются разрешения **пользователя или участника** рабочей области синапсе.

В этом примере из рабочей области извлекается существующая связанная служба `synapselink1` `ws` с [`get()`](/python/api/azureml-core/azureml.core.linkedservice?preserve-view=true&view=azure-ml-py#get-workspace--name-) методом.
```python
linked_service = LinkedService.get(ws, 'synapselink1')
```

### <a name="manage-linked-services"></a>Управление связанными службами

Чтобы удалить связь с рабочими областями, используйте `unregister()` метод.

``` python
linked_service.unregister()
```

Просмотрите все связанные службы, связанные с рабочей областью машинного обучения. 

```python
LinkedService.list(ws)
```
 
## <a name="attach-synapse-spark-pool-as-a-compute"></a>Подключение пула синапсе Spark в качестве вычислений

После связывания рабочих областей Подключите пул синапсе Spark в качестве выделенного ресурса вычислений для задач подготовки данных. 

Пулы синапсе Spark можно подключать с помощью,
* Студия машинного обучения Azure.
* [Шаблоны Azure Resource Manager (ARM)](https://github.com/Azure/azure-quickstart-templates/blob/master/101-machine-learning-linkedservice-create/azuredeploy.json)
* Пакет SDK для Python 

Выполните следующие действия, чтобы подключить пул синапсе Spark с помощью студии. 

1. Войдите в [машинное обучение Azure Studio](https://ml.azure.com/).
1. Выберите **связанные службы** в разделе **Управление** левой панели.
1. Выберите рабочую область синапсе.
1. Выберите **подключенные пулы Spark** в верхнем левом углу. 
1. Выберите **Подключить**. 
1. Выберите из списка Пул синапсе Spark и укажите имя.  
    1. В этом списке указаны доступные пулы синапсе Spark, которые можно присоединить к вычислению. 
    1. Сведения о создании нового пула Spark синапсе см. [в статье Создание пула Apache Spark в синапсе Studio](../synapse-analytics/quickstart-create-apache-spark-pool-portal.md) .
1. Выберите пункт **присоединить выбранное**. 


Вы также можете использовать **пакет SDK для Python** , чтобы подключить пул синапсе Spark. 

Следующий код 
1. Настраивает Синапсекомпуте с помощью,

   1. LinkedService, `linked_service` созданный или полученный на предыдущем шаге. 
   1. Тип целевого объекта вычислений, который необходимо присоединить, `SynapseSpark`
   1. Имя пула Spark синапсе. Это должно совпадать с существующим пулом Apache Spark, который находится в рабочей области синапсе.
   
1. Создает ComputeTarget машинного обучения, передавая, 
   1. Рабочая область машинного обучения, которую вы хотите использовать, `ws`
   1. Имя, которое вы хотите использовать для вычислений в рабочей области машинного обучения. 
   1. Attach_configuration, указанный при настройке Синапсекомпуте.
       1. Вызов ComputeTarget. Attach () выполняется асинхронно, поэтому образец блокируется до завершения вызова.

```python
from azureml.core.compute import SynapseCompute, ComputeTarget

attach_config = SynapseCompute.attach_configuration(linked_service, #Linked synapse workspace alias
                                                    type='SynapseSpark', #Type of assets to attach
                                                    pool_name="<Synapse Spark pool name>") #Name of Synapse spark pool 

synapse_compute = ComputeTarget.attach(workspace= ws,                
                                       name='<Synapse Spark pool alias in Azure ML>', 
                                       attach_configuration=attach_config
                                      )

synapse_compute.wait_for_completion()
```

Убедитесь, что пул синапсе Spark подключен.

```python
ws.compute_targets['Synapse Spark pool alias']
```

## <a name="launch-synapse-spark-pool-for-data-preparation-tasks"></a>Запуск пула Spark синапсе для задач подготовки данных

Вы можете указать [машинное обучение Azure среду](concept-environments.md) , которая будет использоваться во время сеанса синапсе. Вступят в силу будут только зависимости Conda, указанные в среде. Образ DOCKER не поддерживается.

>[!WARNING]
>  Зависимости Python, указанные в зависимости Conda окружения, не поддерживаются в пулах Spark синапсе. В настоящее время поддерживаются только фиксированные версии Python. Проверьте версию Python, включив  `sys.version_info` в скрипт.

Следующий код создает среду, `myenv` которая устанавливает `azureml-core` версию 1.20.0 и `numpy` версию 1.17.0 до начала сеанса. Затем эту среду можно включить в инструкцию синапсе Session `start` .

```python

from azureml.core import Workspace, Environment

# creates environment with numpy and azureml-core dependencies
ws = Workspace.from_config()
env = Environment(name="myenv")
env.python.conda_dependencies.add_pip_package("azureml-core==1.20.0")
env.python.conda_dependencies.add_conda_package("numpy==1.17.0")
env.register(workspace=ws)
```

Чтобы начать подготовку данных с помощью пула Spark синапсе, укажите имя пула синапсе Spark и укажите идентификатор подписки, группу ресурсов рабочей области машинного обучения, имя рабочей области машинного обучения и среду, которая будет использоваться во время сеанса синапсе. 

> [!IMPORTANT]
> Чтобы продолжить использование пула синапсе Spark, необходимо указать, какой ресурс вычислений использовать в задачах подготовки данных с помощью `%synapse` для отдельных строк кода и `%%synapse` для нескольких строк. 

```python
%synapse start -c SynapseSparkPoolAlias -s AzureMLworkspaceSubscriptionID -r AzureMLworkspaceResourceGroupName -w AzureMLworkspaceName -e myenv
```

После запуска сеанса можно проверить метаданные сеанса.

```python
%synapse meta
```

## <a name="load-data-from-storage"></a>Загрузка данных из хранилища

После запуска сеанса синапсе Spark прочтите данные, которые вы хотите подготовить. Загрузка данных поддерживается для хранилища BLOB-объектов Azure и Azure Data Lake Storage поколений 1 и 2.

Существует два способа загрузки данных из следующих служб хранилища: 

* Непосредственная загрузка данных из хранилища с помощью пути к распределенным файлам Hadoop (HDFS).

* Чтение данных из существующего [машинное обучение Azure набора данных](how-to-create-register-datasets.md).

Для доступа к этим службам хранилища требуются разрешения **читателя данных BLOB-объекта хранилища** . Если вы планируете записывать данные обратно в эти службы хранилища, необходимы разрешения **участника данных BLOB-объекта хранилища** . [Дополнительные сведения о разрешениях и ролях хранилища](../storage/common/storage-auth-aad-rbac-portal.md#azure-roles-for-blobs-and-queues).

### <a name="load-data-with-hadoop-distributed-files-system-hdfs-path"></a>Загрузка данных с помощью пути к распределенным файлам Hadoop (HDFS)

Для загрузки и чтения данных из хранилища с соответствующим путем HDFS необходимо иметь доступ к учетным данным для проверки подлинности. Эти учетные данные различаются в зависимости от типа хранилища.  

В следующем коде показано, как считывать данные из **хранилища BLOB-объектов Azure** в кадр данных Spark с помощью токена подписанного URL-адрес (SAS) или ключа доступа. 

```python
%%synapse

# setup access key or SAS token
sc._jsc.hadoopConfiguration().set("fs.azure.account.key.<storage account name>.blob.core.windows.net", "<access key>")
sc._jsc.hadoopConfiguration().set("fs.azure.sas.<container name>.<storage account name>.blob.core.windows.net", "sas token")

# read from blob 
df = spark.read.option("header", "true").csv("wasbs://demo@dprepdata.blob.core.windows.net/Titanic.csv")
```

В следующем коде показано, как считывать данные из **Azure Data Lake Storage поколения 1 (ADLS Gen 1)** с учетными данными субъекта-службы. 

```python

# setup service principal which has access of the data
sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.access.token.provider.type","ClientCredential")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.client.id", "<client id>")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.credential", "<client secret>")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.refresh.url",
https://login.microsoftonline.com/<tenant id>/oauth2/token)

df = spark.read.csv("adl://<storage account name>.azuredatalakestore.net/<path>")

```

В следующем коде показано, как считывать данные из **Azure Data Lake Storage поколения 2 (ADLS Gen 2)** с учетными данными субъекта-службы. 

```python
# setup service principal which has access of the data
sc._jsc.hadoopConfiguration().set("fs.azure.account.auth.type.<storage account name>.dfs.core.windows.net","OAuth")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth.provider.type.<storage account name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.id.<storage account name>.dfs.core.windows.net", "<client id>")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.secret.<storage account name>.dfs.core.windows.net", "<client secret>")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.endpoint.<storage account name>.dfs.core.windows.net",
https://login.microsoftonline.com/<tenant id>/oauth2/token)


df = spark.read.csv("abfss://<container name>@<storage account>.dfs.core.windows.net/<path>")

```

### <a name="read-in-data-from-registered-datasets"></a>Считывает данные из зарегистрированных наборов данных

Вы также можете получить имеющийся зарегистрированный набор данных в рабочей области и выполнить для него подготовку данных, преобразовав ее в таблицу данных Spark.  

В следующем примере возвращается зарегистрированный Табулардатасет, `blob_dset` , который ссылается на файлы в хранилище BLOB-объектов, и преобразуется в таблицу данных Spark. При преобразовании наборов данных в таблицу данных Spark можно использовать `pyspark` библиотеки для изучения и подготовки.  

``` python

%%synapse
from azureml.core import Workspace, Dataset

dset = Dataset.get_by_name(ws, "blob_dset")
spark_df = dset.to_spark_dataframe()
```

## <a name="perform-data-preparation-tasks"></a>Выполнение задач подготовки данных

После извлечения и изучения данных можно выполнять задачи подготовки данных.

Следующий код разворачивается по примеру HDFS в предыдущем разделе и фильтрует данные в таблице данных Spark, `df` на основе столбца и групп **выжившую»** , которые перечислены по **возрасту** .

```python
%%synapse
from pyspark.sql.functions import col, desc

df.filter(col('Survived') == 1).groupBy('Age').count().orderBy(desc('count')).show(10)

df.show()

```

## <a name="save-data-to-storage-and-stop-spark-session"></a>Сохранение данных в хранилище и завершение сеанса Spark

После завершения изучения и подготовки данных храните подготовленные данные для последующего использования в учетной записи хранения в Azure.

В следующем примере подготовленные данные записываются обратно в хранилище BLOB-объектов Azure и перезаписывают исходный `Titanic.csv` файл в `training_data` каталоге. Для обратной записи в хранилище требуются разрешения **участника данных BLOB-объекта хранилища** . [Дополнительные сведения о разрешениях и ролях хранилища](../storage/common/storage-auth-aad-rbac-portal.md#azure-roles-for-blobs-and-queues).

```python
%% synapse

df.write.format("csv").mode("overwrite").save("wasbs://demo@dprepdata.blob.core.windows.net/training_data/Titanic.csv")
```

Завершив подготовку данных и сохранив подготовленные данные в хранилище, завершите работу с пулом синапсе Spark с помощью следующей команды.

```python
%synapse stop
```

## <a name="create-dataset-to-represent-prepared-data"></a>Создание набора данных для представления подготовленных данных

Когда вы будете готовы использовать подготовленные данные для обучения модели, подключитесь к хранилищу с помощью [хранилища данных машинное обучение Azure](how-to-access-data.md)и укажите, какие файлы нужно использовать с [набором машинное обучение Azure](how-to-create-register-datasets.md).

Следующий пример кода:

* Предполагается, что хранилище данных, которое подключается к службе хранилища, в которой были сохранены подготовленные данные, уже создано.  
* Возвращает существующее хранилище данных `mydatastore` из рабочей области `ws` с помощью метода Get ().
* Создает [филедатасет](how-to-create-register-datasets.md#filedataset), `train_ds` который ссылается на подготовленные файлы данных, расположенные в `training_data` каталоге в `mydatastore` .  
* Создает переменную `input1` , которую можно использовать позднее, чтобы сделать файлы данных `train_ds` набора данных доступными для целевого объекта вычислений.

```python
from azureml.core import Datastore, Dataset

datastore = Datastore.get(ws, datastore_name='mydatastore')

datastore_paths = [(datastore, '/training_data/')]
train_ds = Dataset.File.from_files(path=datastore_paths, validate=True)
input1 = train_ds.as_mount()

```

## <a name="example-notebook"></a>Пример записной книжки

Подробный пример кода для подготовки данных и обучения модели из одной записной книжки с помощью Azure синапсе и Машинное обучение Azure см. в этой [сквозной записной книжке](../synapse-analytics/overview-what-is.md) .

## <a name="next-steps"></a>Дальнейшие действия

* [Обучение модели](how-to-set-up-training-targets.md).
* [Обучение с помощью набора данных Машинное обучение Azure](how-to-train-with-datasets.md)
* [Создайте набор данных машинного обучения Azure](how-to-create-register-datasets.md).

