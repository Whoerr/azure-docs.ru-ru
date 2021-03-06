---
title: Группа безопасности сети — как она работает
titlesuffix: Azure Virtual Network
description: Узнайте, как группы безопасности сети помогают фильтровать сетевой трафик между ресурсами Azure.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/24/2020
ms.author: kumud
ms.reviewer: kumud
ms.openlocfilehash: 9510c4b0940a0a03ae9a232c3329817468aaf358
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537904"
---
# <a name="how-network-security-groups-filter-network-traffic"></a>Фильтрация сетевого трафика с группами безопасности сети
<a name="network-security-groups"></a>

Группу безопасности сети Azure можно использовать для фильтрации сетевого трафика в ресурсах Azure и из них в виртуальной сети Azure. Группа безопасности сети содержит [правила безопасности](./network-security-groups-overview.md#security-rules) , разрешающие или запрещающие входящий сетевой трафик, или исходящий сетевой трафик из нескольких типов ресурсов Azure. Для каждого правила можно указать источник и назначение, порт и протокол.

В виртуальную сеть Azure можно развернуть ресурсы из нескольких служб Azure. Полный список см. в разделе [Службы, которые можно развернуть в виртуальной сети](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Вы можете связать не более одной группы безопасности сети с каждой [подсетью](virtual-network-manage-subnet.md#change-subnet-settings) виртуальной сети и [сетевым интерфейсом](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group) на виртуальной машине. Одну и ту же группу безопасности сети можно связать с любым выбранным количеством подсетей или сетевых интерфейсов.

На следующем рисунке показаны различные сценарии развертывания групп безопасности сети для разрешения трафика, поступающего из Интернета и обратно через TCP-порт 80:

![NSG-processing](./media/network-security-group-how-it-works/network-security-group-interaction.png)

Чтобы понять, как Azure обрабатывает входящие и исходящие правила для групп безопасности сети, см. предыдущий рисунок, а также следующий текст.

## <a name="inbound-traffic"></a>Входящий трафик

Для входящего трафика Azure сначала обрабатывает правила в группе безопасности сети, связанной с подсетью, если таковая имеется, а затем правила в группе безопасности сети, связанной с сетевым интерфейсом, если таковой имеется.

- **VM1**: обрабатываются правила безопасности в *NSG1*, так как она связана с *Subnet1* и *VM1* в *Subnet1*. Если вы не создали правило, разрешающее входящий трафик через порт 80, трафик будет запрещен правилом безопасности по умолчанию [DenyAllInbound](./network-security-groups-overview.md#denyallinbound) и никогда не будет вычислен группой *NSG2*, так как *NSG2* связана с сетевым интерфейсом. Если в группе *NSG1* есть правило безопасности, разрешающее трафик через порт 80, трафик будет обрабатываться группой *NSG2*. Чтобы разрешить трафик к виртуальной машине через порт 80, в *NSG1* и *NSG2* должно быть правило, разрешающее трафик из Интернета через порт 80.
- **VM2**: обрабатываются правила в *NSG1*, потому что *VM2* также существует в *Subnet1*. Так как у *VM2* нет группы безопасности сети, связанной с ее сетевым интерфейсом, она получает весь трафик, разрешенный *NSG1*, или не получает трафик, запрещенный *NSG1*. Если группа безопасности сети связана с подсетью, трафик разрешен или запрещен для всех ресурсов в одной и той же подсети.
- **VM3**: так как с *Subnet2* не связана ни одна группа безопасности сети, трафик разрешен в подсеть и обрабатывается *NSG2*, потому что *NSG2* связана с сетевым интерфейсом, подключенным к *VM3*.
- **VM4**: трафик разрешен в *VM4,*, так как группа безопасности сети не связана с *Subnet3* или сетевым интерфейсом в виртуальной машине. Весь трафик пропускается через подсеть и сетевой интерфейс, если с ними не связана группа безопасности сети.

## <a name="outbound-traffic"></a>Исходящий трафик

Для исходящего трафика Azure сначала обрабатывает правила в группе безопасности сети, связанной с сетевым интерфейсом, если таковой имеется, а затем правила в группе безопасности сети, связанной с подсетью, если таковая имеется.

- **VM1**: обрабатываются правила безопасности в *NSG2*. Если вы не создали правило безопасности, которое запрещает исходящий трафик в Интернет через порт 80, трафик разрешается правилом безопасности по умолчанию [AllowInternetOutbound](./network-security-groups-overview.md#allowinternetoutbound) в *NSG1* и *NSG2*. Если в *NSG2* есть правило безопасности, которое запрещает трафик через порт 80, трафик запрещен и никогда не вычисляется *NSG1*. Чтобы запретить исходящий трафик через порт 80 на виртуальной машине, у одной из группы безопасности сети или у обеих должно быть правило, которое запрещает трафик, поступающий в Интернет через порт 80.
- **VM2**: весь трафик отправляется через этот сетевой интерфейс к подсети, так как с сетевым интерфейсом, подключенным к *VM2*, не связана группа безопасности сети. Обрабатываются правила в *NSG1*.
- **VM3**: если в *NSG2* есть правило безопасности, которое запрещает трафик через порт 80, трафик запрещается. Если в *NSG2* есть правило безопасности, разрешающее трафик через порт 80, входящий трафик, поступающий в Интернет, разрешается через порт 80, так как группа безопасности сети не связана с *Subnet2*.
- **VM4**: весь трафик разрешен из *VM4*, так как группа безопасности сети не связана с сетевым интерфейсом, подключенным к виртуальной машине или к *Subnet3*.


## <a name="intra-subnet-traffic"></a>Intra-Subnet трафик

Важно отметить, что правила безопасности в NSG, связанном с подсетью, могут повлиять на подключение между виртуальными машинами. Например, если правило Добавлено в *NSG1* , которое запрещает весь входящий и исходящий трафик, то *VM1* и *VM2* больше не смогут взаимодействовать друг с другом. Чтобы разрешить это, необходимо добавить другое правило. 

Все правила, применяемые к сетевому интерфейсу, можно просмотреть в списке [действующих правил безопасности](virtual-network-network-interface.md#view-effective-security-rules) сетевого интерфейса. Кроме того, вы можете воспользоваться функцией [Проверка IP-потока](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) в службе "Наблюдатель за сетями Azure", чтобы определить, разрешен ли обмен данными с сетевым интерфейсом. Проверка IP-потока позволяет узнать, разрешен или запрещен обмен данными, а также правило безопасности сети, разрешающее или запрещающее трафик.

> [!NOTE]
> Группы безопасности сети связаны с подсетями или с виртуальными машинами и облачными службами, развернутыми в классической модели развертывания, а также с подсетями или сетевыми интерфейсами в модели развертывания диспетчер ресурсов. Дополнительные сведения о моделях развертывания Azure см. в статье [Развертывание с помощью Azure Resource Manager и классическое развертывание: сведения о моделях развертывания и состоянии ресурсов](../azure-resource-manager/management/deployment-models.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

> [!TIP]
> Если у вас нет конкретной причины, рекомендуется связать группу безопасности сети с подсетью или сетевым интерфейсом, но не с обеими причинами. Так как правила в группе безопасности сети, связанной с подсетью, могут конфликтовать с правилами в группе безопасности сети, связанной с сетевым интерфейсом, могут возникнуть непредвиденные проблемы с обменом данными, требующие устранения неполадок.

## <a name="next-steps"></a>Следующие шаги

* Дополнительные сведения о том, какие ресурсы Azure можно развернуть в виртуальной сети и какие группы безопасности сети можно связать с ними, см. в статье [Интеграция виртуальной сети для служб Azure](virtual-network-for-azure-services.md).
* Если вы еще не создавали группу безопасности сети, вы можете пройти краткое [руководство](tutorial-filter-network-traffic.md), чтобы получить такой опыт создания.
* Если вы ознакомлены с группами безопасности сети и вам нужно управлять ими, см. сведения в статье [Создание, изменение и удаление группы безопасности сети](manage-network-security-group.md). 
* Если возникают проблемы с обменом данными и вам необходимо устранить неполадки с группами безопасности сети, см. сведения в статье [Диагностика проблемы с фильтрацией трафика на виртуальной машине](diagnose-network-traffic-filter-problem.md). 
* Узнайте, как включить [журналы потоков для групп безопасности сети](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) для анализа сетевого трафика в ресурсы, имеющие связанную группу безопасности сети.