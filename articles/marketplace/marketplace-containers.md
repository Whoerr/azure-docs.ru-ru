---
title: Рекомендации по публикации для предложений контейнеров в Azure Marketplace
description: В этой статье описываются требования к публикации предложений контейнеров в Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 11/30/2020
ms.openlocfilehash: 83c575aa40b80d9a8e39263e89a5e7860c8f8774
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95741667"
---
# <a name="publishing-guide-for-azure-container-offers"></a>Рекомендации по публикации для предложений контейнеров Azure

Предложения контейнеров Azure помогут опубликовать образ контейнера в Azure Marketplace. В этом разделе вы ознакомитесь с требованиями для этого типа предложения.

Предложения контейнеров Azure — это предложения транзакций, которые развертываются и выставляются через Azure Marketplace. Параметр List, отображаемый пользователем, — «получить сейчас».

Используйте тип предложения контейнера Azure, если решение представляет собой образ контейнера DOCKER, настроенный как экземпляр контейнера Azure на основе Kubernetes.

> [!NOTE]
> Экземпляр контейнера Azure — это экземпляр DOCKER времени выполнения, который предоставляет самый быстрый и простой способ запуска контейнера в Azure без необходимости управлять виртуальными машинами и не применять службу более высокого уровня. Экземпляры контейнеров можно развертывать непосредственно в Azure или управлять службой Azure Kubernetes Services или ядром службы Kubernetes Azure.  

В настоящее время корпорация Майкрософт поддерживает свободное лицензирование и модель с использованием собственной лицензии (BYOL).

## <a name="container-offer-requirements"></a>Требования к контейнерному предложению

| Требование | Сведения |  
|:--- |:--- |  
| Выставление счетов и ценообразование | Поддерживается либо бесплатная, либо модель выставления счетов BYOL.<br><br> |  
| Образ, созданный из Dockerfile | Образы контейнеров должны быть основаны на спецификации образа DOCKER и построены на основе Dockerfile.<br> <br>Дополнительные сведения о создании образов DOCKER см. в разделе "использование" [справочника по Dockerfile](https://docs.docker.com/engine/reference/builder/#usage).<br><br> |  
| Размещение в репозитории реестра контейнеров Azure | Образы контейнеров должны размещаться в репозитории реестра контейнеров Azure.<br> <br>Дополнительные сведения о работе с реестром контейнеров Azure см. [в разделе Краткое руководство. создание закрытого реестра контейнеров с помощью портал Azure](../container-registry/container-registry-get-started-portal.md).<br><br> |  
| Добавление тегов изображений | Образы контейнеров должны содержать по крайней мере один тег (максимальное число тегов: 16).<br><br>Дополнительные сведения о добавлении тегов к изображению см `docker tag` . на странице сайта [документации DOCKER](https://docs.docker.com/engine/reference/commandline/tag) .<br><br> |  

## <a name="next-steps"></a>Следующие шаги

- Сведения о подготовке технических ресурсов для предложения контейнеров см. в статье [Создание технических ресурсов для контейнеров Azure](create-azure-container-technical-assets.md).

- Дополнительные сведения о создании предложения контейнеров Azure см. [в статье Создание предложения контейнера Azure в Azure Marketplace](create-azure-container-offer.md) .
