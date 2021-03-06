---
title: Необходимые условия для Avere vFXT — Azure
description: Сведения о задачах, которые необходимо выполнить перед созданием кластера в Авере Вфкст для Azure, включая работу с подписками, квотами и конечными точками службы хранилища.
author: ekpgh
ms.service: avere-vfxt
ms.topic: how-to
ms.date: 01/21/2020
ms.author: rohogue
ms.openlocfilehash: d87f57873a4bb84b20df3da3880017d9ef2484a5
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96008418"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Подготовка к созданию Avere vFXT

В этой статье описываются необходимые задачи для создания кластера Авере Вфкст.

## <a name="create-a-new-subscription"></a>Создание подписки

Начните с создания подписки Azure. Используйте отдельную подписку для каждого проекта Авере Вфкст, чтобы упростить отслеживание и очистку расходов, а также убедиться, что на другие проекты не влияют квоты или регулирование ресурсов при подготовке рабочего процесса кластера.

Чтобы создать подписку Azure на портале Azure:

1. Перейдите в колонку [Подписки](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Нажмите кнопку **+ Добавить** вверху.
1. Войдите при появлении соответствующего запроса.
1. Выберите предложение и выполните действия, чтобы создать подписку.

## <a name="configure-subscription-owner-permissions"></a>Настройка разрешений владельца подписки

Кластер vFXT должен создать пользователь с разрешениями владельца подписки. Для создания кластера требуется, чтобы владелец принимал условия предоставления программного обеспечения и разрешил изменения в сети и ресурсах хранения.

Существуют некоторые обходные пути, позволяющие не владельцам создавать Авере Вфкст для кластера Azure. Эти сценарии подразумевают ограниченность ресурсов и назначение дополнительных ролей Azure создателю. Во всех этих случаях владелец подписки также должен заранее [принять условия использования программного обеспечения Авере вфкст](#accept-software-terms) .

| Сценарий | Ограничения | Роли доступа, необходимые для создания кластера Авере Вфкст |
|----------|--------|-------|
| Администратор группы ресурсов создает Вфкст | Виртуальная сеть, контроллер кластера и узлы кластера должны быть созданы в группе ресурсов. | [Администратор доступа пользователей](../role-based-access-control/built-in-roles.md#user-access-administrator) и роли [участника](../role-based-access-control/built-in-roles.md#contributor) , которые находятся в пределах целевой группы ресурсов. |
| Использование существующей внешней виртуальной сети | Контроллер кластера и узлы кластера создаются в группе ресурсов Вфкст, но используют существующую виртуальную сеть в другой группе ресурсов. | (1) [администратор доступа пользователей](../role-based-access-control/built-in-roles.md#user-access-administrator) и роли [участников](../role-based-access-control/built-in-roles.md#contributor) , областью действия которых является группа ресурсов вфкст; и (2) [участник виртуальных машин](../role-based-access-control/built-in-roles.md#virtual-machine-contributor), [администратор доступа пользователей](../role-based-access-control/built-in-roles.md#user-access-administrator)и роли [участников авере](../role-based-access-control/built-in-roles.md#avere-contributor) , областью действия которых является группа ресурсов виртуальной сети. |
| Настраиваемая роль для создателей кластера | Ограничения на размещение ресурсов отсутствуют. Этот метод дает значительные права, не являющиеся владельцами. | Владелец подписки создает настраиваемую роль Azure, как описано в [этой статье](avere-vfxt-non-owner.md). |

## <a name="quota-for-the-vfxt-cluster"></a>Квота для кластера vFXT

Убедитесь, что у вас достаточно квоты для следующих компонентов Azure. При необходимости [подайте запрос на увеличение квоты](../azure-portal/supportability/resource-manager-core-quotas-request.md).

> [!NOTE]
> Виртуальные машины и компоненты SSD, перечисленные здесь, предназначены для самого кластера vFXT. Помните, что вам также нужна квота для виртуальных машин и твердотельных накопителей, которые будут использоваться для вашей фермы вычислений.
>
> Убедитесь, что квота включена для региона, где планируется запустить рабочий процесс.

|Компонент Azure|Quota|
|----------|-----------|
|Виртуальные машины|3 или более E32s_v3 (по одному на узел кластера) |
|Хранилище SSD (цен. категория "Премиум")|200 ГБ пространства ОС плюс от 1 до 4 ТБ объема кэша на узел |
|Учетная запись хранения (необязательно) |Версия 2|
|Серверное хранилище данных (необязательно) |Один новый контейнер больших двоичных объектов в LRS |
<!-- this table also appears in the overview - update it there if updating here -->

## <a name="accept-software-terms"></a>Принятие условий программного обеспечения

> [!TIP]
> Пропустите этот шаг, если владелец подписки создаст кластер Авере Вфкст.

Во время создания кластера необходимо принять условия предоставления услуг для программного обеспечения Avere vFXT. Если вы не являетесь владельцем подписки, он должен принять условия заранее.

Этот шаг достаточно выполнить один раз для каждой подписки.

Чтобы принять условия программного обеспечения заранее:

1. Откройте Cloud Shell на портале Azure или перейдите по адресу <https://shell.azure.com>. Войдите с помощью идентификатора своей подписки.

   ```azurecli
    az login
    az account set --subscription <subscription ID>
   ```

1. Выполните эту команду, чтобы принять условия предоставления услуг и включить программный доступ для Avere vFXT для образа программного обеспечения в Azure:

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   ```

## <a name="create-a-storage-service-endpoint-in-your-virtual-network-if-needed"></a>Создание конечной точки службы хранилища в виртуальной сети (при необходимости)

[Конечная точка службы](../virtual-network/virtual-network-service-endpoints-overview.md) сохраняет локальный трафик BLOB-объектов Azure вместо его маршрутизации за пределы виртуальной сети. Рекомендуется использовать любой Авере Вфкст для кластера Azure, который использует большой двоичный объект Azure для хранения данных в серверной части.

При создании новой виртуальной сети при создании кластера конечная точка создается автоматически. Если вы предоставляете существующую виртуальную сеть, она должна иметь конечную точку службы хранилища Майкрософт, если вы хотите создать новый контейнер хранилища BLOB-объектов во время создания кластера.<!-- if there is no endpoint in that situation, the cluster creation will fail -->

> [!TIP]
>
>* Пропустите этот шаг, если вы создаете новую виртуальную сеть в ходе создания кластера.
>* Конечная точка не является обязательной, если хранилище BLOB-объектов не создается во время создания кластера. В этом случае можно создать конечную точку службы позже, если вы решили использовать большой двоичный объект Azure.

Создайте конечную точку службы хранилища из портал Azure.

1. На портале откройте список виртуальных сетей.
1. Выберите виртуальную сеть для кластера.
1. В меню слева щелкните **конечные точки службы** .
1. В верхней части страницы щелкните **Добавить**.
1. Выберите службу ``Microsoft.Storage`` .
1. Выберите подсеть кластера.
1. В нижней части страницы щелкните **Добавить**.

   ![Снимок экрана портала Azure с заметками для шагов создания конечной точки службы](media/avere-vfxt-service-endpoint.png)

## <a name="next-steps"></a>Следующие шаги

После выполнения этих предварительных требований можно создать кластер. Инструкции см. в статье [Развертывание кластера vFXT](avere-vfxt-deploy.md).