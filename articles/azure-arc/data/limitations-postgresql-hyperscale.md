---
title: Ограничения PostgreSQL с поддержкой ARC в Azure
description: Ограничения PostgreSQL с поддержкой ARC в Azure
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: how-to
ms.openlocfilehash: b1a56c8acf1789690c01f1c16b7c37a237720e39
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100418241"
---
# <a name="limitations-of-azure-arc-enabled-postgresql-hyperscale"></a>Ограничения PostgreSQL с поддержкой ARC в Azure

В этой статье описываются ограничения PostgreSQL в службе "геомасштабирование Azure". 

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="backup-and-restore"></a>Резервное копирование и восстановление

- Восстановление на момент времени (например, восстановление на определенную дату и время) в одну и ту же группу серверов не поддерживается. При восстановлении на момент времени необходимо выполнить восстановление в другой группе серверов, которая была развернута перед восстановлением. После восстановления в новую группу серверов можно удалить группу серверов источника.
- Восстановление всего содержимого резервной копии (в отличие от восстановления до определенной точки во времени) в той же группе серверов поддерживается для PostgreSQL версии 12. PostgreSQL версии 11 не поддерживается в связи с ограничением механизма PostgreSQL с помощью временных шкал. Чтобы восстановить все содержимое резервной копии для группы серверов PostgreSQL версии 11, необходимо восстановить ее в другую группу серверов.


## <a name="databases"></a>Базы данных

Размещение более одной базы данных в группе серверов не поддерживается.


## <a name="security"></a>Безопасность

Управление пользователями и ролями не поддерживается. Пока же продолжайте использовать стандартного пользователя postgres.

## <a name="roles-and-responsibilities"></a>роли и обязанности.

Роли и обязанности корпорации Майкрософт и ее клиентов отличаются в разных службах Azure PaaS (платформа как услуга) и гибридных Azure (например, в службе "Дуга Azure" PostgreSQL "масштабирование"). 

### <a name="frequently-asked-questions"></a>Вопросы и ответы

В таблице ниже приведены ответы на часто задаваемые вопросы о ролях и обязанностях службы поддержки.

| Вопрос                      | Платформа Azure как услуга (PaaS) | Гибридные службы ARC в Azure |
|:----------------------------------|:------------------------------------:|:---------------------------:|
| Кто предоставляет инфраструктуру?  | Microsoft                          | Customer                  |
| Кто предоставляет программное обеспечение? *       | Microsoft                          | Microsoft                 |
| Кто выполняет операции? | Microsoft                          | Customer                  |
| Предоставляет ли корпорация Майкрософт соглашения об уровне обслуживания?      | Да                                | Нет                        |
| Кто оплачивает соглашения об уровне обслуживания? | Microsoft                          | Customer                  |

\* Службы Azure

__Почему корпорация Майкрософт не предоставляет соглашения об уровне обслуживания для гибридных служб ARC в Azure?__ Так как корпорация Майкрософт не владеет инфраструктурой и не работает. Клиенты.

## <a name="next-steps"></a>Следующие шаги

- **Попробуйте.** Быстро Начните работу с помощью [Azure Arc JumpStart](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) в службе Azure Kubernetes Service (AKS), AWS эластичной Kubernetes Service (ЕКС), Google Cloud Kubernetes Engine (гке) или на виртуальной машине Azure. 

- **Создайте свой собственный.** Чтобы создать кластер Kubernetes, выполните следующие действия. 
   1. [Установка клиентских средств](install-client-tools.md)
   2. [Создание контроллера данных ARC в Azure](create-data-controller.md)
   3. [Создание группы серверов масштабирования базы данных Azure для PostgreSQL в службе "Дуга Azure"](create-postgresql-hyperscale-server-group.md) 

- **Learn**
   - [Подробнее о службах данных с поддержкой Arc Azure](https://azure.microsoft.com/services/azure-arc/hybrid-data-services)
   - [Сведения о службе "Дуга Azure"](https://aka.ms/azurearc)
