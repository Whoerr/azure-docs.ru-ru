---
title: Регистрация поставщика ресурсов для Решения Azure VMware
description: Инструкции по регистрации поставщика ресурсов для Решения Azure VMware.
ms.topic: include
ms.date: 02/17/2021
ms.openlocfilehash: cd4a6f3003945973f0fe5367eb198823595a412e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750270"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-create-private-cloud.md -->

Чтобы использовать Решение Azure VMware, необходимо сначала зарегистрировать поставщик ресурсов в подписке. Дополнительные сведения о поставщиках ресурсов см. в статье [Поставщики и типы ресурсов Azure](../../azure-resource-manager/management/resource-providers-and-types.md).

### <a name="azure-cli"></a>Azure CLI 

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

### <a name="azure-portal"></a>Портал Azure
 
1. Войдите на [портал Azure](https://portal.azure.com).

1. В меню портала Azure выберите **Все службы**.

1. В поле **Все службы** введите **подписка**, а затем выберите **Подписки**.

1. Выберите подписку из списка подписок для просмотра.

1. Выберите **Поставщики ресурсов** и введите **Microsoft.AVS** в поле поиска. 
 
1. Если поставщик ресурсов не зарегистрирован, выберите **Зарегистрировать**.