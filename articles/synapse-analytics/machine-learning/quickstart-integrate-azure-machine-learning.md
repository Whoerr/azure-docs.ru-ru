---
title: Краткое руководство. Связывание рабочей области машинного обучения Azure
description: Связывание рабочей области Synapse с рабочей областью Машинного обучения Azure
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: quickstart
ms.reviewer: jrasnick, garye
ms.date: 09/25/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 800cbf9b7a7fa415452f29b253347188c8917c52
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2021
ms.locfileid: "98219459"
---
# <a name="quickstart-create-a-new-azure-machine-learning-linked-service-in-synapse"></a>Краткое руководство. Создание новой связанной службы машинного обучения Azure в Synapse

В рамках этого краткого руководства вы свяжете рабочую область Azure Synapse Analytics с рабочей областью Машинного обучения Azure. Связывание этих рабочих областей позволяет использовать машинное обучение Azure из различных функций Synapse.

Например, такая связь с рабочей областью машинного обучения Azure обеспечивает следующие возможности.

- Запуск конвейеров машинного обучения Azure в составе конвейеров Synapse. Дополнительные сведения см. в разделе [выполнение конвейеров машинного обучения Azure](../../data-factory/transform-data-machine-learning-service.md).

- Наполнение своих данных прогнозами, применяя модель машинного обучения из реестра моделей машинного обучения Azure и выполняя оценку модели в пулах Synapse SQL. Дополнительные сведения см. в статье [Учебник. Мастер оценки моделей машинного обучения для пулов Synapse SQL](tutorial-sql-pool-model-scoring-wizard.md).

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/).
- [Рабочая область Synapse Analytics](../get-started-create-workspace.md) с учетной записью хранения ADLS 2-го поколения, настроенной в качестве хранилища по умолчанию. Необходимо быть **участником для данных BLOB-объектов хранилища** файловой системы ADLS 2-го поколения, с которой вы работаете.
- [Рабочая область машинного обучения Azure](../../machine-learning/how-to-manage-workspace.md)
- Для создания субъекта-службы и секрета, которые можно использовать для создания связанной службы, необходимы разрешения (или запрос от кого-либо, имеющего разрешения). Обратите внимание, что субъекту-службе необходимо назначить роль участника в рабочей области машинного обучения Azure.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портал Azure](https://portal.azure.com/)

## <a name="create-a-service-principal"></a>Создание субъекта-службы

На этом шаге будет создан новый субъект-служба. Если вы хотите использовать существующий субъект-службу, этот шаг можно пропустить.
1. Откройте портал Azure. 

1. Перейдите к **Azure Active Directory** -> **Регистрация приложений**

1. Щелкните **Новая регистрация**. Затем следуйте инструкциям пользовательского интерфейса, чтобы зарегистрировать новое приложение.

1. После регистрации приложения. Создайте секрет для приложения. Перейдите в **Ваше приложение** -> **Сертификат и секрет**. Щелкните **Добавить секрет клиента**, чтобы создать секрет. Обеспечьте безопасность секрета, чтобы использовать его позже.

   ![Создание секрета](media/quickstart-integrate-azure-machine-learning/quickstart-integrate-azure-machine-learning-createsp-00a.png)

1. Создайте субъект-службу для приложения. Перейдите в **Ваше приложение** -> **Обзор**, а затем щелкните **Создать субъект-службу**. В некоторых случаях этот субъект-служба создается автоматически.

   ![Создание субъекта-службы](media/quickstart-integrate-azure-machine-learning/quickstart-integrate-azure-machine-learning-createsp-00b.png)

1. Добавьте субъект-службу в качестве "участника" рабочей области машинного обучения Azure. Обратите внимание, что для этого потребуется владелец группы ресурсов, к которой принадлежит рабочая область машинного обучения Azure.

   ![Назначение роли участника](media/quickstart-integrate-azure-machine-learning/quickstart-integrate-azure-machine-learning-createsp-00c.png)

## <a name="create-a-linked-service"></a>Создание связанной службы

1. В рабочей области Synapse, в которой нужно создать новую связанную службу машинного обучения Azure, перейдите в раздел **Управление** -> **Связанная служба**, создайте связанную службу, указав ее тип как "Машинное обучение Azure".

   ![Создание связанной службы](media/quickstart-integrate-azure-machine-learning/quickstart-integrate-azure-machine-learning-create-linked-service-00a.png)

2. Заполните форму.

   - Идентификатор субъекта-службы: Это **идентификатор (клиента) приложения** для веб-приложения.
  
     > [!NOTE]
     > Это НЕ имя приложения. Этот идентификатор можно найти на обзорной странице приложения. Он должен выглядеть как длинная строка, похожая на "81707eac-ab38-406u-8f6c-10ce76a568d5".

   - Ключ субъекта-службы Секрет, созданный в предыдущем разделе.

3. Щелкните **Проверить подключение**, чтобы проверить правильность конфигурации. Если проверка подключения пройдена, щелкните **Сохранить**.

   Если проверка подключения не удалась, убедитесь, что идентификатор и секрет субъекта-службы указаны правильно, и повторите попытку.

## <a name="next-steps"></a>Дальнейшие действия

- [Учебник. Мастер оценки моделей машинного обучения — выделенный пул SQL](tutorial-sql-pool-model-scoring-wizard.md)
- [Возможности машинного обучения в Azure Synapse Analytics](what-is-machine-learning.md)