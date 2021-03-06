---
title: Использование веб-перехватчиков для реагирования на действия реестра
description: Узнайте, как использовать веб-перехватчики для активации событий при выполнении действий отправки или вытягивания в репозиториях реестров.
ms.topic: article
ms.date: 05/24/2019
ms.openlocfilehash: 5374b58ba72727500294a173c26e9a131b29fe34
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101722251"
---
# <a name="using-azure-container-registry-webhooks"></a>Использование веб-перехватчиков реестра контейнеров Azure

В реестре контейнеров Azure хранятся частные образы контейнеров Docker, а также осуществляется управление ими (подобно тому, как в Docker Hub хранятся общедоступные образы Docker). Также он может размещать репозитории для [чартов Helm](container-registry-helm-repos.md) (предварительная версия), которые представляют собой формат упаковки для развертывания приложений в Kubernetes. Веб-перехватчики используются для активации событий при выполнении определенных действий в одном из репозиториев реестров. Веб-перехватчики могут реагировать на события на уровне реестра. Также их можно ориентировать на определенный тег репозитория. С помощью [геореплицированного](container-registry-geo-replication.md) реестра вы можете настроить веб-перехватчик так, чтобы он реагировал на события в определенной региональной реплике.

Конечная точка веб-перехватчика должна быть общедоступной из реестра. Вы можете настроить запросы веб-перехватчика реестра для проверки подлинности в защищенной конечной точке.

Дополнительные сведения о запросах веб-перехватчика см. в [справочнике по схеме веб-перехватчика реестра контейнеров Azure](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Предварительные требования

* Реестр контейнеров Azure. Создайте реестр контейнеров в своей подписке Azure. Это можно сделать на [портале Azure](container-registry-get-started-portal.md) или с помощью [Azure CLI](container-registry-get-started-azure-cli.md). [Уровни службы Реестра контейнеров Azure](container-registry-skus.md) имеют разные квоты для веб-перехватчиков.
* Интерфейс командной строки Docker (Docker CLI). Установите [подсистему Docker](https://docs.docker.com/engine/installation/), чтобы настроить локальный компьютер в качестве узла Docker и получить доступ к командам Docker CLI.

## <a name="create-webhook---azure-portal"></a>Создание веб-перехватчика на портале Azure

1. Войдите на [портал Azure](https://portal.azure.com).
1. Перейдите в реестр контейнеров, в котором нужно создать веб-перехватчик.
1. В разделе **Службы** выберите **Веб-перехватчики**.
1. Щелкните **Добавить** на панели инструментов веб-перехватчика.
1. Заполните форму *Создать веб-перехватчик* следующими данными:

| Значение | Описание |
|---|---|
| Имя веб-перехватчика | Имя, назначаемое веб-перехватчику. Оно может содержать только буквы и цифры (5–50 символов). |
| Расположение | Для [геореплицированного](container-registry-geo-replication.md) реестра укажите регион Azure с нужной репликой реестра. 
| URI службы | Универсальный код ресурса (URI) для отправки веб-перехватчиком уведомлений POST. |
| Настраиваемые заголовки | Заголовки, которые требуется передавать вместе с запросом POST. Они должны быть в формате "ключ: значение". |
| "Trigger actions" (Активирующие действия) | Действия, которые активируют веб-перехватчик. Поддерживаются такие действия, как отправка и удаление образа, отправка и удаление чарта Helm, а также помещение образа в карантин. Здесь можно выбрать одно или несколько действий, которые будут активировать этот веб-перехватчик. |
| Состояние | Состояние веб-перехватчика после его создания. По умолчанию он включен. |
| Область | Область действия веб-перехватчика. Если область не указана, учитываются все события в реестре. Можно указать определенный репозиторий или тег, используя формат "репозиторий:тег" или "репозиторий:*", чтобы учитывать все теги в одном репозитории. |

Пример формы для веб-перехватчика приведен ниже.

![Снимок экрана, показывающий создание веб-перехватчика записей контроля доступа U в портал Azure.](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook---azure-cli"></a>Создание веб-перехватчика с помощью Azure CLI

Чтобы создать веб-перехватчик с помощью Azure CLI, используйте команду [az acr webhook create](/cli/azure/acr/webhook#az-acr-webhook-create). Следующая команда создает веб-перехватчик для всех событий удаления образа в реестре *mycontainerregistry*:

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Проверка веб-перехватчика

### <a name="azure-portal"></a>Портал Azure

Прежде чем использовать веб-перехватчик, его можно проверить с помощью кнопки **Проверка связи**. После нажатия этой кнопки к указанной конечной точке отправляется общий запрос POST и ответ записывается в журнал. Используя функцию проверки связи, мы сможем проверить, правильно ли настроен веб-перехватчик.

1. Выберите веб-перехватчик, который требуется проверить.
2. В верхней части панели инструментов выберите **Проверка связи**.
3. Просмотрите ответ от конечной точки в столбце **СОСТОЯНИЕ HTTP**.

![Пользовательский интерфейс создания веб-перехватчика ACR на портале Azure](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

Чтобы протестировать веб-перехватчик ACR с помощью Azure CLI, используйте команду [az acr webhook ping](/cli/azure/acr/webhook#az-acr-webhook-ping).

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Чтобы просмотреть результаты, используйте команду [az acr webhook list-events](/cli/azure/acr/webhook).

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Удаление веб-перехватчика

### <a name="azure-portal"></a>Портал Azure

Любой веб-перехватчик можно удалить, выбрав его и нажав кнопку **Удалить** на портале Azure.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Дальнейшие действия

### <a name="webhook-schema-reference"></a>Справочник по схеме веб-перехватчика

Дополнительные сведения о формате и свойствах полезных данных событий JSON, генерируемых Реестром контейнеров Azure, см. в справочнике по схеме веб-перехватчика:

[Azure Container Registry webhook schema reference](container-registry-webhook-reference.md) (Справочник по схеме реестра контейнеров Azure)

### <a name="event-grid-events"></a>События Сетки событий

В дополнение к событиям веб-перехватчика собственного реестра, описанным в этой статье, Реестр контейнеров Azure может генерировать события для Сетки событий.

[Краткое руководство. Отправка событий из частного реестра контейнеров в службу "Сетка событий"](container-registry-event-grid-quickstart.md)
