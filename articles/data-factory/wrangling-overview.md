---
title: Структурирование данных в фабрике данных Azure
description: Обзор структурирование данных в фабрике данных Azure
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/19/2021
ms.openlocfilehash: f922e7a2755a6e26a0d9f93f2668753e2f4dad5a
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98738175"
---
# <a name="what-is-data-wrangling"></a>Что такое структурирование данных?

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Организациям необходимо иметь возможность исследовать важные бизнес-данные для подготовки данных и структурирование, чтобы обеспечить точный анализ сложных данных, которые по-своему возрастают каждый день. Подготовка данных необходима, чтобы организации могли использовать данные в различных бизнес-процессах и сократить время на ценность.

Фабрика данных позволяет итеративно подготовить данные без кода в масштабе облака с помощью Power Query. Фабрика данных интегрируется с [Power Query Online](/power-query/) и делает функции Power Query M доступными в качестве действий конвейера.

Фабрика данных преобразует M, созданный Power Query интерактивным гибридным редактором, в код Spark для выполнения масштабирования в облаке путем перевода M в потоки данных фабрики данных Azure. Данные структурирование с Power Query и потоками данных особенно полезны для инженеров по работе с данными или "интеграторов данных".

> [!NOTE]
> Действие Power Query в фабрике данных Azure в настоящее время доступно в общедоступной предварительной версии

## <a name="use-cases"></a>Варианты использования

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4MFkW]

### <a name="fast-interactive-data-exploration-and-preparation"></a>Быстрое исследование и подготовка данных в интерактивном режиме

Несколько инженеров по данным и интеграторов данных, содействующих с данными, могут интерактивно исследовать и подготавливать наборы в масштабе облака. С увеличением объема и скорости данных в озера данных пользователям требуется эффективный способ просмотра и подготовки наборов данных. Например, может потребоваться создать набор данных, содержащий все демографические данные о клиентах для новых клиентов с 2017. Вы не сопоставляется с известным целевым объектом. Вы изучаете, структурирование и готовитесь наборы данных, чтобы удовлетворить требованиям, прежде чем публиковать их в Lake. Структурирование часто используется для менее формальных сценариев аналитики. Наборы данных подготовив можно использовать для выполнения преобразований и операций машинного обучения.

### <a name="code-free-agile-data-preparation"></a>Подготовка гибких данных без поддержки кода

Интеграторы данных совышают 60% времени, затрачиваемого на поиск и подготовку данных. Они стремятся сделать это с помощью кода, чтобы повысить эксплуатационную производительность. Разрешив интеграторам данных сообразировать, формировать и публиковать данные с помощью известных средств, таких как Power Query Online, масштабируемым способом, радикально повышая их производительность. Структурирование в фабрике данных Azure позволяет использовать привычный интерактивный редактор гибридных веб-приложений Power Query, позволяющий интеграторам данных сорешать быстро исправлять ошибки, стандартизировать данные и создавать высококачественные данные для поддержки бизнес-решений.

### <a name="data-validation-and-exploration"></a>Проверка данных и исследование

Визуальная проверка данных в целях удаления выбросов, аномалий и их согласования с формой для быстрой аналитики.

## <a name="supported-sources"></a>Поддерживаемые источники

| Соединитель | Формат данных | Authentication type (Тип проверки подлинности) |
| -- | -- | --|
| [Хранилище BLOB-объектов Azure](connector-azure-blob-storage.md) | CSV, Parquet | Ключ учетной записи |
| [Azure Data Lake Storage 1-го поколения](connector-azure-data-lake-store.md) | CSV | Субъект-служба |
| [Azure Data Lake Storage 2-го поколения](connector-azure-data-lake-storage.md) | CSV, Parquet | Ключ учетной записи, субъект-служба |
| [База данных SQL Azure](connector-azure-sql-database.md) | - | Проверка подлинности SQL |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md) | - | Проверка подлинности SQL |

## <a name="the-mashup-editor"></a>Редактор гибридных приложений

При создании Power Queryного действия все исходные наборы данных становятся запросами набора DataSet и помещаются в папку **адфресаурце** . По умолчанию Усеркуери будет указывать на первый запрос набора данных. Все преобразования должны выполняться в Усеркуери, так как изменения в запросах к набору данных не поддерживаются и не сохраняются. Переименование, Добавление и удаление запросов в настоящее время не поддерживается.

![Структурирование](media/wrangling-data-flow/editor.png)

В настоящее время не все функции Power Query M поддерживаются для структурирование данных, несмотря на то, что они доступны во время разработки. При построении действий Power Query вам будет предложено следующее сообщение об ошибке, если функция не поддерживается:

`The wrangling data flow is invalid. Expression.Error: The transformation logic isn't supported. Please try a simpler expression`

Дополнительные сведения о поддерживаемых преобразованиях см. в разделе [Data структурирование functions](wrangling-functions.md).

## <a name="next-steps"></a>Следующие шаги

Узнайте, как [создать структурирование данных Power Query в гибридном](wrangling-tutorial.md)процессе.
