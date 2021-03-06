---
title: Краткое руководство. Настройка изоляции рабочих нагрузок на портале
description: Использование портала Azure для настройки изоляции рабочих нагрузок для выделенного пула SQL.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 05/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 5773691852ddc723ec84503edee37c678bbbcfd9
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2021
ms.locfileid: "98677510"
---
# <a name="quickstart-configure-dedicated-sql-pool-workload-isolation-using-a-workload-group-in-the-azure-portal"></a>Краткое руководство. Настройка изоляции рабочих нагрузок выделенного пула SQL с помощью группы рабочей нагрузки на портале Azure

В этом кратком руководстве показано, как настроить [изоляцию рабочих нагрузок](sql-data-warehouse-workload-isolation.md), создав группу рабочей нагрузки для резервирования ресурсов.  Мы создадим группу рабочей нагрузки для загрузки данных с именем `DataLoads`. Группа рабочей нагрузки будет резервировать 20 % системных ресурсов.  Изоляция загрузки данных на уровне 20 % обеспечит доступность ресурсов для этих операций в соответствии с действующими соглашениями об уровне обслуживания.  После создания группы рабочей нагрузки [создайте классификатор рабочих нагрузок](quickstart-create-a-workload-classifier-portal.md) для назначения запросов этой группе рабочей нагрузки.


Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.


## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

> [!NOTE]
> Создание экземпляра выделенного пула SQL в Azure Synapse Analytics может повлечь дополнительные расходы.  Дополнительные сведения см. на странице [цен на Azure Synapse Analytics](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="prerequisites"></a>Предварительные требования

В этом кратком руководстве предполагается, что у вас уже есть экземпляр выделенного пула SQL в Synapse SQL и права доступа CONTROL DATABASE. Если такой экземпляр необходимо создать, выполните действия из статьи [Краткое руководство. Создание выделенного пула SQL с помощью портала Azure](../quickstart-create-sql-pool-portal.md) для создания хранилища данных **mySampleDataWarehouse**.

>[!IMPORTANT] 
>Для настройки управления рабочими нагрузками выделенный пул SQL должен быть подключенным к сети. 

## <a name="configure-workload-isolation"></a>Настройка изоляции рабочей нагрузки

Вы можете изолировать и зарезервировать ресурсы выделенного пула SQL для конкретных рабочих нагрузок, создав группы рабочей нагрузки.  См. сведения о том, как группы рабочей нагрузки помогают управлять рабочей нагрузкой, в руководстве по [изоляции рабочих нагрузок](sql-data-warehouse-workload-isolation.md).  В кратком руководстве по [созданию хранилища данных SQL Azure на портале Azure и отправке запросов к этому хранилищу данных](create-data-warehouse-portal.md) показано, как создать хранилище **mySampleDataWarehouse** и инициализировать его со значением DW100c. Следующие действия создают группу рабочей нагрузки в **mySampleDataWarehouse**.

Чтобы создать группу рабочей нагрузки с изоляцией на уровне 20 %, выполните следующие действия.
1.  Перейдите на страницу **mySampleDataWarehouse** выделенного пула SQL.
1.  Выберите раздел **Управление рабочими нагрузками**.
1.  Щелкните **Создать группу рабочей нагрузки**.
1.  Выберите **Пользовательский**.

    ![Элемент управления "Пользовательский"](./media/quickstart-configure-workload-isolation-portal/create-wg.png)

6.  Введите значение `DataLoads` в поле **Группа рабочей нагрузки**.
7.  Введите значение `20` в поле **Минимальный процент ресурсов**.
8.  Введите значение `5` в поле **Минимальный процент ресурсов на запрос**.
9.  Введите значение `100` в поле **Ограничение ресурсов (%)** .
10. Введите **Сохранить**.

   ![Щелкните Сохранить](./media/quickstart-configure-workload-isolation-portal/configure-wg.png)

При создании группы рабочей нагрузки на портале отображается уведомление.  Ресурсы группы рабочей нагрузки отображаются на диаграммах под заданными значениями.

   ![Завершение настройки](./media/quickstart-configure-workload-isolation-portal/display-wg.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Чтобы удалить созданный с помощью этого руководства классификатор рабочих нагрузок `DataLoads`, выполните следующие действия.
1. Щелкните **`...`** справа от строки, определяющей группу рабочей нагрузки `DataLoads`.
2. Щелкните **Удалить группу рабочей нагрузки**.
3. Щелкните **Да** при появлении запроса на подтверждение удаления группы рабочей нагрузки.
4. Щелкните **Save**(Сохранить).

   ![Удаление](./media/quickstart-configure-workload-isolation-portal/delete-wg.png)



Плата взимается за единицы хранилища данных и данные, которые содержатся в нем. Плата за вычислительные ресурсы и ресурсы хранилища взимается отдельно.

- Если вы хотите сохранить данные в хранилище, то можете приостановить работу вычислительных ресурсов, когда не используете хранилище данных. При приостановке вычислений плата взимается только за хранение данных. Когда вы будете готовы работать с данными, возобновите вычисление.
- Если вы хотите исключить будущие расходы, то можете удалить хранилище данных.

Выполните следующие действия, чтобы очистить ресурсы.

1. Войдите на [портал Azure](https://portal.azure.com) и выберите выделенный пул SQL.

    ![Очистка ресурсов](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Чтобы приостановить вычисление, нажмите кнопку **Пауза**. Если работа хранилища данных приостановлена, вы увидите кнопку **Запуск**.  Чтобы возобновить вычисление, нажмите кнопку **Пуск**.

3. Чтобы удалить хранилище данных во избежание дальнейших платежей за вычисления или хранение, нажмите кнопку **Удалить**.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы использовать группу рабочей нагрузки `DataLoads`, необходимо создать [классификатор рабочих нагрузок](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) для направления запросов к группе рабочей нагрузки.  См. сведения о [создании классификатора рабочих нагрузок](quickstart-create-a-workload-classifier-portal.md) для `DataLoads`.

## <a name="see-also"></a>См. также раздел
См. сведения о [мониторинге рабочих нагрузок для управления рабочими нагрузками](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
