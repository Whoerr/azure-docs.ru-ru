---
title: Основные понятия — обновления и обновление частного облака
description: Сведения об основных процессах обновления и функциях в решении VMware для Azure.
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: d93453cbf6ad744844a04cd298cc18ad181cc0b0
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100634998"
---
# <a name="azure-vmware-solution-private-cloud-updates-and-upgrades"></a>Обновление и обновление частного облака решения VMware для Azure

Одним из преимуществ частных облаков решений VMware для Azure является платформа. Обслуживание включает автоматические обновления пакета с проверенным программным обеспечением VMware, чтобы обеспечить использование последней версии программного обеспечения частного облака решения VMware для Azure.

В частности, в частном облаке решения Azure VMware есть:

- Выделенные узлы серверов без операционной системы, подготовленные с помощью VMware ESXi гипервизора 
- сервер vCenter для управления ESXi и vSAN 
- VMware НСКС-T программно определяемая сеть для виртуальных машин рабочей нагрузки vSphere  
- Хранилище данных VMware vSAN для виртуальных машин рабочей нагрузки vSphere  
- ХККС VMware для мобильной рабочей нагрузки  

Частное облако решения Azure VMware также включает ресурсы в ундерлай Azure, необходимые для подключения и для эксплуатации частного облака. Решение Azure VMware постоянно отслеживает работоспособность компонентов ундерлай и VMware. Когда решение Azure VMware обнаруживает сбой, оно выполняет действия по исправлению неисправных компонентов. 

## <a name="what-components-get-updated"></a>Какие компоненты будут обновлены?   

Решение Azure VMware обновляет следующие компоненты VMware: 

- vCenter Server и ESXi, работающие на узлах серверов без операционной системы 
- vSAN 
- НСКС-T 

Решение VMware для Azure также обновляет программное обеспечение в ундерлай, например драйверы, программное обеспечение для сетевых коммутаторов и встроенное по на узлах без операционной системы. 

## <a name="types-of-updates"></a>Типы обновлений

Решение VMware для Azure применяет следующие типы обновлений для компонентов VMware:

- Исправления: исправления безопасности и исправления ошибок, выпущенные VMware. 
- Обновления: дополнительные обновления версии одного или нескольких компонентов VMware. 
- Обновления: основные обновления версии одного или нескольких компонентов VMware.

Вы получите уведомления до и после применения исправлений к частным облакам. Мы также будем работать с вами, чтобы запланировать период обслуживания перед применением обновлений или обновлений в частном облаке. 

## <a name="vmware-appliance-backup"></a>Резервное копирование устройств VMware 

Решение Azure VMware также выполняет резервное копирование конфигурации следующих компонентов VMware:

- с сервером vCenter 
- Диспетчер НСКС-T 

В случае сбоя решение Azure VMware может восстановить эти компоненты из резервной копии конфигурации. 

Дополнительные сведения о версиях программного обеспечения VMware см. в [статье концепция частных облаков и кластеров](concepts-private-clouds-clusters.md) и [часто задаваемые вопросы](faq.yml).

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали об основных процессах обновления и функциях в решении Azure VMware, вы можете узнать о следующих возможностях:

- [Как создать частное облако](tutorial-create-private-cloud.md).
- [Как включить ресурс решения Azure VMware](enable-azure-vmware-solution.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
