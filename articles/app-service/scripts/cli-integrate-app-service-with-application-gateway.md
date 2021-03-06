---
title: Пример скрипта Azure CLI — интеграция Службы приложений со Шлюзом приложений | Документация Майкрософт
description: Пример скрипта Azure CLI — интеграция Службы приложений со Шлюзом приложений
services: appservice
documentationcenter: appservice
author: madsd
manager: ccompy
editor: ''
tags: azure-service-management
ms.assetid: 6c16d6f8-3c08-4a59-858e-684a2ceccb5f
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/09/2019
ms.author: madsd
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 3820e7bf00f99a846dd2be0edeaf4248e0dfd8ad
ms.sourcegitcommit: 273c04022b0145aeab68eb6695b99944ac923465
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2020
ms.locfileid: "97006093"
---
# <a name="integrate-app-service-with-application-gateway-using-cli"></a>Интеграция Службы приложений со Шлюзом приложений с использованием CLI

Этот пример скрипта создает веб-приложение Службы приложений Azure, виртуальную сеть Azure и Шлюз приложений. Затем он разрешает для веб-приложения входящий трафик только из подсети Шлюза приложений.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Для работы с этим руководством требуется Azure CLI версии 2.0.74 или более поздней. Если вы используете Azure Cloud Shell, последняя версия уже установлена.

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/integrate-with-app-gateway/integrate-with-app-gateway.sh "Integrate with Application Gateway")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, приложения Службы приложений, базы данных Cosmos DB и всех связанных ресурсов этот сценарий использует указанные ниже команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [`az group create`](/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [`az network vnet create`](/cli/azure/network/vnet#az-network-vnet-create) | Создает виртуальную сеть. |
| [`az network public-ip create`](/cli/azure/network/public-ip#az-network-public-ip-create) | Создает общедоступный IP-адрес. |
| [`az network public-ip show`](/cli/azure/network/public-ip#az-network-public-ip-show) | Показывает подробные сведения об общедоступном IP-адресе. |
| [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create) | Создает план службы приложений. |
| [`az webapp create`](/cli/azure/webapp#az-webapp-create) | Создает веб-приложение Службы приложений. |
| [`az webapp show`](/cli/azure/webapp#az-webapp-show) | Показывает подробные сведения о веб-приложении Службы приложений. |
| [`az webapp config access-restriction add`](/cli/azure/webapp/config/access-restriction#az-webapp-config-access-restriction-add) | Добавляет ограничения доступа к веб-приложению Службы приложений. |
| [`az network application-gateway create`](/cli/azure/network/application-gateway#az-network-application-gateway-create) | Создает Шлюз приложений. |
| [`az network application-gateway http-settings update`](/cli/azure/network/application-gateway/http-settings#az-network-application-gateway-http-settings-update) | Обновляет параметры HTTP Шлюза приложений. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры скриптов Azure CLI для службы приложений см. в [документации по службе приложений Azure](../samples-cli.md).
