---
title: Введение в управление версиями — Azure HDInsight
description: Узнайте, как работает управление версиями в Azure HDInsight.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: deshriva
ms.date: 02/08/2021
ms.openlocfilehash: c4dddeef5daf167eeef92532b61ed986861896e8
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746367"
---
# <a name="how-versioning-works-in-hdinsight"></a>Как работает управление версиями в HDInsight

Служба HDInsight имеет два основных компонента: поставщик ресурсов и Apache Hadoop компоненты, развернутые в кластере. 

## <a name="hdinsight-resource-provider"></a>Поставщик ресурсов HDInsight

Ресурсы в Azure становятся доступны поставщику ресурсов. Поставщик ресурсов HDInsight отвечает за создание, управление и удаление кластеров.

## <a name="hdinsight-images"></a>Образы HDInsight

HDInsight использует образы для размещения компонентов программного обеспечения с открытым исходным кодом (OSS), которые могут быть развернуты в кластере. Эти образы содержат базовую операционную систему Ubuntu и основные компоненты, такие как Spark, Hadoop, Kafka, HBase или Hive.

## <a name="versioning-in-hdinsight"></a>Управление версиями в HDInsight

Корпорация Майкрософт периодически обновляет образы и поставщик ресурсов HDInsight, добавляя новые улучшения и функции.

Новая версия HDInsight может быть создана при выполнении одного или нескольких из следующих условий.

- Основные изменения и обновления функций поставщика ресурсов HDInsight
- Основные выпуски компонентов OSS
- Основные изменения в операционной системе Ubuntu

Новая версия образа создается при выполнении одного или нескольких следующих условий.

- Основные или вспомогательные выпуски и обновления компонентов OSS
- Исправления или исправления для компонента в образе

## <a name="next-steps"></a>Дальнейшие действия

- [Компоненты и версии Apache в HDInsight](./hdinsight-component-versioning.md)
