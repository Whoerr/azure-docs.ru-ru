---
title: Краткое руководство. Приостановка и возобновление вычислений в выделенном пуле SQL с помощью портала Azure
description: Используйте портал Azure, чтобы приостановить вычисления для выделенного пула SQL и сэкономить. Возобновите вычисления, когда вы готовы к использованию хранилища данных.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 11/23/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 567d5ae8b9d480003486362a7aa0ef69f9740d52
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98120297"
---
# <a name="quickstart-pause-and-resume-compute-in-dedicated-sql-pool-via-the-azure-portal"></a>Краткое руководство. Приостановка и возобновление вычислений в выделенном пуле SQL с помощью портала Azure

Для приостановки и возобновления работы вычислительных ресурсов выделенного пула SQL можно использовать портал Azure. Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Перед началом

Используйте инструкции из статьи [Краткое руководство. Создание выделенного пула SQL с помощью портала Azure](../quickstart-create-sql-pool-portal.md), чтобы создать выделенный пул SQL **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>Приостановка работы вычислительных ресурсов

Для сокращения затрат можно приостанавливать и возобновлять работу вычислительных ресурсов по требованию. Например, если база данных не будет использоваться ночью и по выходным, ее работу можно приостанавливать на это время и возобновлять днем.
 
>[!NOTE]
>Когда база данных приостановлена, оплата за вычислительные ресурсы не взимается. Тем не менее плата за хранение по-прежнему будет взиматься. 

Чтобы приостановить работу выделенного пула SQL, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Перейдите на страницу **Выделенный пул SQL**, чтобы открыть пул SQL. 
3. В поле **Состояние** должно отображаться значение **В сети**.

    ![Вычисления в сети](././media/pause-and-resume-compute-portal/compute-online.png)

4. Чтобы приостановить выделенный пул SQL, нажмите на кнопку **Пауза**. 
5. Отобразится запрос подтверждения операции. Нажмите кнопку **Да**.
6. Подождите несколько секунд, и вы увидите, что значение параметра **Состояние** изменилось на **Приостановка**.

    ![Снимок экрана: портал Azure, пример хранилища данных, для параметра "Состояние" отображается значение "Приостановка".](./media/pause-and-resume-compute-portal/pausing.png)

7. После завершения операции приостановки пул пребывает в состоянии **Приостановлен** и доступен переключатель **Возобновить**.
8. Теперь вычислительные ресурсы для выделенного пула SQL пребывают вне сети. Вы не будете платить за вычисления, пока работа службы не будет возобновлена.

    ![Вычисления вне сети](././media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Возобновление работы вычислительных ресурсов

Чтобы возобновить работу выделенного пула SQL, сделайте следующее:

1. Перейдите на страницу **Выделенный пул SQL**, чтобы открыть пул SQL.
3. На странице **mySampleDataWarehouse** обратите внимание на то, что параметр **Состояние** имеет значение **Приостановлено**.

    ![Вычисления вне сети](././media/pause-and-resume-compute-portal/compute-offline.png)

1. Чтобы возобновить вычисления для пула SQL, нажмите кнопку **Возобновить**. 
1. Отобразится запрос подтверждения запуска. Нажмите кнопку **Да**.
1. Обратите внимание на то, что значение параметра **Состояние** изменилось на **Возобновление**.

    ![Снимок экрана: портал Azure, пример хранилища данных, выделена кнопка "Запуск", а для параметра "Состояние" отображается значение "Возобновление".](./media/pause-and-resume-compute-portal/resuming.png)

1. После возвращения в сеть пул SQL пребывает в состоянии **В сети** и доступен переключатель **Пауза**.
1. Вычислительные ресурсы для пула SQL теперь находятся в сети, и вы можете использовать службу. Плата за вычисления будет взиматься.

    ![Вычисления в сети](././media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Плата взимается за единицы хранилища данных и данные, хранящиеся в выделенном пуле SQL. Плата за вычислительные ресурсы и ресурсы хранилища взимается отдельно. 

- Если вы хотите сохранить данные в хранилище, приостановите вычисления.
- Если вы хотите исключить будущие расходы, то можете удалить выделенный пул SQL. 

Выполните следующие действия, чтобы очистить ресурсы по необходимости.

1. Войдите на [портал Azure](https://portal.azure.com) и выберите выделенный пул SQL.

    ![Очистка ресурсов](./media/pause-and-resume-compute-portal/clean-up-resources.png)

1. Чтобы приостановить работу вычислительных ресурсов, нажмите кнопку **Приостановить**. 

1. Чтобы удалить выделенный пул SQL во избежание дальнейших платежей за вычисления или хранение, нажмите кнопку **Удалить**.



## <a name="next-steps"></a>Дальнейшие действия

Вы приостановили и возобновили вычисления для выделенного пула SQL. Перейдите к следующей статье, чтобы узнать больше о том, как [загрузить данные в выделенный пул SQL](./load-data-from-azure-blob-storage-using-copy.md). Дополнительные сведения об управлении возможностями вычислений см. в статье [Управление вычислениями](sql-data-warehouse-manage-compute-overview.md).