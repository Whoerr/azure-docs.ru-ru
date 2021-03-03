---
title: Программа проверки Kubernetes с поддержкой ARC в Azure
services: azure-arc
ms.service: azure-arc
ms.date: 03/02/2021
ms.topic: article
author: shashankbarsin
ms.author: shasb
description: Описывает программу проверки дуги для дистрибутивов Kubernetes
keywords: Kubernetes, Arc, Azure, K8s, проверка
ms.openlocfilehash: ad87fe3f11c97524595a517b17bb52fa1b771c9d
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101662829"
---
# <a name="azure-arc-validation-program"></a>Программа проверки Azure Arc

Kubernetes с поддержкой Azure Arc работает со всеми кластерами Kubernetes, сертифицированными для Cloud Native Computing Foundation. Группа по работе с Azure Arc также работала с основными отраслевыми поставщиками Kubernetes, которые позволяют проверять, включена Kubernetesя ARC в Azure с дистрибутивами Kubernetes. Будущие основные и дополнительные версии дистрибутивов Kubernetes, выпущенные этими поставщиками, будут проверены на совместимость с Kubernetes с поддержкой ARC.

## <a name="validated-distributions"></a>Проверенные дистрибутивы

Следующие предоставленные корпорацией Майкрософт дистрибутивы Kubernetes и поставщики инфраструктуры успешно прошли тесты соответствия для Kubernetes с включенной службой ARC.

| Поставщик распространения и инфраструктуры | Версия |
| ---------------------------------------- | ------- |
| Поставщик API кластера в Azure            | Версия выпуска: [0.4.12](https://github.com/kubernetes-sigs/cluster-api-provider-azure/releases/tag/v0.4.12); Версия Kubernetes: [1.18.2](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.2) |
| AKS в Azure Stack HCI                   | Версия выпуска: [декабрь 2020 обновление](https://github.com/Azure/aks-hci/releases/tag/AKS-HCI-2012); Версия Kubernetes: [1.18.8](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.8) |

Следующие поставщики и соответствующие Kubernetes распределения успешно прошли проверки соответствия для Kubernetes с поддержкой дуги Azure:

| Имя поставщика | Имя распространения | Версия |
| ------------ | ----------------- | ------- |
| RedHat       | [Платформа контейнеров OpenShift](https://www.openshift.com/products/container-platform) | [4.6](https://docs.openshift.com/container-platform/4.6/release_notes/ocp-4-6-release-notes.html) |
| VMware       | [Сетка танзу Kubernetes](https://tanzu.vmware.com/kubernetes-grid) | Версия Kubernetes: v 1.17.5 |
| Canonical    | [Kubernetes с чудо](https://ubuntu.com/kubernetes) | [1,19](https://ubuntu.com/kubernetes/docs/1.19/components) |
| ранчер      | [Подсистема Kubernetes Ранчер](https://rancher.com/products/rke/) | Версия рке CLI: [v 1.2.4](https://github.com/rancher/rke/releases/tag/v1.2.4); Kubernetes версии: [1.19.6](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.6)), [1.18.14](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.14)), [1.17.16](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.16))  |
| нутаникс      | [карбон](https://www.nutanix.com/products/karbon)    | Версия выпуска: [2,0](https://www.nutanix.com/blog/introducing-nutanix-karbon-2-kubernetes-simplicity-upgraded); Версия Kubernetes: [1.17.0](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.0) |

Группа Arc Azure также выполнила проверку соответствия и проверила Kubernetes сценарии с поддержкой дуги Azure на следующих поставщиках общедоступного облака:

| Имя поставщика общедоступного облака | Имя распространения | Версия |
| -------------------------- | ----------------- | ------- |
| Amazon Web Services        | Служба эластичных Kubernetes (ЕКС) | v 1.18.9  |
| Google Cloud Platform      | Google Kubernetes Engine (GKE) | v 1.17.15 |

## <a name="scenarios-validated"></a>Проверенные сценарии

Проверка соответствия, выполняемая как часть Kubernetes с включенной проверкой данных в службе "Дуга Azure", охватывает следующие сценарии:

1. Подключение кластеров Kubernetes к службе "Дуга Azure": 
    * Разверните диаграмму Kubernetes Agent Helm в кластере с включенной службой ARC.
    * Настройте сертификат управляемого удостоверения системы (MSI) в кластере.
    * Агенты отправляют метаданные кластера в Azure.

2. Конфигурация: 
    * Создайте конфигурацию на основе ресурса Kubernetes с включенной службой "Дуга Azure".
    * [Flux](https://docs.fluxcd.io/), необходимый для настройки рабочего процесса гитопс, развернут в кластере.
    * Flux извлекает манифесты и Helm диаграммы из демонстрационного репозитория Git и развертывает их в кластере.

## <a name="next-steps"></a>Дальнейшие действия

* [Подключение кластера к Azure Arc](./connect-cluster.md)
* [Создание конфигураций в кластере Kubernetes с включенной Arc](./use-gitops-connected-cluster.md)
* [Использование политики Azure для применения конфигураций в масштабе](./use-azure-policy.md)
