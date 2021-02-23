---
title: Регистрация поставщика ресурсов для Решения Azure VMware
description: Инструкции по регистрации поставщика ресурсов для Решения Azure VMware.
ms.topic: include
ms.date: 02/17/2021
ms.openlocfilehash: 80010a232f80865b20c2e3d953dc1d9d22ece1c6
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2021
ms.locfileid: "100653184"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-create-private-cloud.md -->

Чтобы использовать Решение Azure VMware, необходимо сначала зарегистрировать поставщик ресурсов в подписке. Дополнительные сведения о поставщиках ресурсов см. в статье [Поставщики и типы ресурсов Azure](/azure/azure-resource-manager/management/resource-providers-and-types).

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
