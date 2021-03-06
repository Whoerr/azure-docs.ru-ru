---
title: Удаление подсети после удаления Управляемый экземпляр SQL
description: Узнайте, как удалить виртуальную сеть Azure после удаления Управляемый экземпляр Azure SQL.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.author: danil
ms.reviewer: douglas, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 496ff6c7ec39706a99bb40447b6443ca71f19e5e
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99093695"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-managed-instance"></a>Удаление подсети после удаления Управляемый экземпляр Azure SQL
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

В этой статье приведены рекомендации по удалению подсети вручную после удаления последнего Управляемый экземпляр Azure SQL, находящегося в нем.

Управляемые экземпляры SQL развертываются в [виртуальных кластерах](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture). Каждый виртуальный кластер связан с подсетью. Виртуальный кластер сохраняется в течение 12 часов после последнего удаления экземпляра, чтобы вы могли быстрее создавать управляемые экземпляры SQL в той же подсети. Плата за поддержание пустого виртуального кластера не взимается. В течение этого периода нельзя удалить подсеть, связанную с виртуальным кластером.

Если вы не хотите ждать 12 часов и предпочитаете удалить виртуальный кластер и его подсеть, это можно сделать вручную. Удалите виртуальный кластер вручную с помощью портал Azure или API виртуальных кластеров.

> [!IMPORTANT]
> - Чтобы удаление было успешным, виртуальный кластер не должен содержать управляемых SQL экземпляров. 
> - Удаление виртуального кластера — это длительная операция, содействующая примерно за 1,5 часов (см. раздел [операции управления SQL управляемый экземпляр](./sql-managed-instance-paas-overview.md#management-operations) для обновления времени удаления виртуального кластера). Виртуальный кластер по-прежнему будет отображаться на портале до завершения этого процесса.

## <a name="delete-a-virtual-cluster-from-the-azure-portal"></a>Удаление виртуального кластера из портал Azure

Чтобы удалить виртуальный кластер с помощью портал Azure, выполните поиск ресурсов виртуального кластера.

![Снимок экрана портал Azure с выделенным полем поиска](./media/virtual-cluster-delete/virtual-clusters-search.png)

Найдя виртуальный кластер, который необходимо удалить, выберите этот ресурс и нажмите кнопку **Удалить**. Вам будет предложено подтвердить удаление виртуального кластера.

![Снимок экрана панели мониторинга "портал Azure виртуальные кластеры" с выделенным параметром "Удалить"](./media/virtual-cluster-delete/virtual-clusters-delete.png)

В портал Azure уведомлениях будет показано подтверждение успешной отправки запроса на удаление виртуального кластера. Сама операция удаления будет выполняться в течение примерно 1,5 часов, в течение которых виртуальный кластер по-прежнему будет отображаться на портале. После завершения процесса виртуальный кластер станет невидимым, и связанная с ним подсеть будет выпущена для повторного использования.

> [!TIP]
> Если в виртуальном кластере нет управляемых экземпляров SQL, и вы не можете удалить виртуальный кластер, убедитесь в отсутствии выполняющегося развертывания экземпляра. Сюда входят запущенные и отмененные развертывания, которые еще выполняются. Это связано с тем, что эти операции будут по-прежнему использовать виртуальный кластер, заблокируя его после удаления. Просмотр на вкладке **развертывания** группы ресурсов, в которой развернут экземпляр, означает, что все выполняемые развертывания будут отображаться. В этом случае дождитесь завершения развертывания, удалите Управляемый экземпляр SQL, а затем удалите виртуальный кластер.

## <a name="delete-a-virtual-cluster-by-using-the-api"></a>Удаление виртуального кластера с помощью API

Чтобы удалить виртуальный кластер с помощью API, используйте параметры URI, указанные в [методе Delete виртуальных кластеров](/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Дальнейшие действия

- Общие сведения см. в статье [что такое Azure SQL управляемый экземпляр?](sql-managed-instance-paas-overview.md).
- Узнайте о [архитектуре подключения в управляемый экземпляр SQL](connectivity-architecture-overview.md).
- Узнайте, как [изменить существующую виртуальную сеть для управляемый экземпляр SQL](vnet-existing-add-subnet.md).
- Руководство по созданию виртуальной сети, созданию Управляемый экземпляр Azure SQL и восстановлению базы данных из резервной копии базы данных см. в статье [создание управляемый экземпляр SQL Azure (портал)](instance-create-quickstart.md).
- Сведения о проблемах с DNS см. [в статье Настройка настраиваемой службы DNS](custom-dns-configure.md).