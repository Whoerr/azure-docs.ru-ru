---
title: Рекомендации по обеспечению непрерывности бизнес-процессов AKS и аварийному восстановлению
description: Узнайте о рекомендациях оператора кластера для достижения максимального времени бесперебойной работы приложений, обеспечения высокого уровня доступности и подготовки к аварийному восстановлению в службе Azure Kubernetes Service (AKS).
services: container-service
author: lastcoolnameleft
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: thfalgou
ms.custom: fasttrack-edit
ms.openlocfilehash: 3ff8406a3634fa946ab8ce7aca694bbc57d556a5
ms.sourcegitcommit: 8f0803d3336d8c47654e119f1edd747180fe67aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2021
ms.locfileid: "97976407"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Рекомендации по обеспечению непрерывности бизнес-процессов и аварийного восстановления в Службе Azure Kubernetes (AKS)

При управлении кластерами в Службе Azure Kubernetes (AKS) время бесперебойной работы приложений является важным аспектом. По умолчанию AKS обеспечивает высокий уровень доступности с помощью нескольких узлов в [масштабируемом наборе виртуальных машин (VMSS)](../virtual-machine-scale-sets/overview.md). Но эти несколько узлов не защищают систему от сбоя региона. Чтобы максимально увеличить время работы, планируйте непрерывность бизнес-процессов и подготовьте аварийное восстановление.

Эта статья посвящена планированию непрерывности бизнес-процессов и аварийного восстановления в AKS. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Спланируйте кластеры AKS в нескольких регионах.
> * Маршрутизация трафика между несколькими кластерами с помощью диспетчера трафика Azure.
> * Используйте георепликацию для реестров образа контейнера.
> * Планируйте состояние приложения в нескольких кластерах.
> * Репликация хранилища в несколько регионов.

## <a name="plan-for-multiregion-deployment"></a>Планирование развертывания в многорегионовой среде

**Рекомендации**. при развертывании нескольких кластеров AKS выберите регионы, в которых доступен AKS, и используйте парные регионы.

Кластер AKS развертывается в одном регионе. Чтобы защитить систему от сбоя региона, разверните приложение в нескольких кластерах AKS в разных регионах. При планировании развертывания кластера AKS учитывайте следующее.

* [**Доступность региона AKS**](./quotas-skus-regions.md#region-availability). Выберите регионы, близкие к пользователям. AKS постоянно развертывается в новых регионах.
* [**Парные регионы Azure**](../best-practices-availability-paired-regions.md). для географической области выберите два региона, которые связаны друг с другом. Парные регионы координируют обновления платформы и назначают приоритеты при необходимости.
* **Доступность службы**. Определите, должны ли парные регионы быть "горячий", "горячий", "горячий" или "горячий" или "теплый". Вы хотите одновременно запустить оба региона, где один регион *готов* начать обслуживание трафика? Или вы хотите, чтобы у одного региона было время, чтобы подготовиться к обработке трафика?

Доступность региона AKS и парные регионы являются совместным фактором. Развертывайте кластеры AKS в парах регионах, для которых настроено совместное управление аварийным восстановлением при региональных сбоях. Например, AKS доступны в регионах Восточная часть США и Западная часть США. Эти регионы являются парными. Выберите эти два региона при создании стратегии AKS BC/DR.

При развертывании приложения добавьте еще один шаг к конвейеру CI/CD для развертывания в этих нескольких кластерах AKS. Если вы не обновляете конвейеры развертывания, приложения могут быть развернуты только в одном из регионов и кластеров AKS. Клиентский трафик, направляемый в дополнительный регион, не получит последние обновления кода.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Использование диспетчера трафика Azure для маршрутизации трафика

**Рекомендация**. диспетчер трафика Azure может направлять клиентов в ближайший кластер AKS и экземпляр приложения. Чтобы обеспечить оптимальную производительность и избыточность, направьте весь трафик приложения через диспетчер трафика, прежде чем он перейдет в кластер AKS.

Если у вас несколько кластеров AKS в разных регионах, используйте диспетчер трафика для управления передачей трафика в приложения, выполняемые в каждом кластере. [Диспетчер трафика Azure](../traffic-manager/index.yml) — это подсистема балансировки нагрузки на основе DNS, которая может распределять сетевой трафик между регионами. Используйте диспетчер трафика для маршрутизации пользователей на основе времени отклика кластера или по географическому расположению.

![AKS с диспетчером трафика](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Клиенты, имеющие один кластер AKS, обычно подключаются к IP-или DNS-имени данного приложения. При развертывании в многокластерной среде клиенты должны подключаться к DNS-имени диспетчера трафика, которое указывает на службы в каждом кластере AKS. Определите эти службы с помощью конечных точек диспетчера трафика. Каждая конечная точка — это *IP-адрес балансировщика нагрузки службы*. Используйте эту конфигурацию, чтобы направить сетевой трафик из конечной точки диспетчера трафика в одном регионе в конечную точку в другом регионе.

![Географическая маршрутизация через диспетчер трафика](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Диспетчер трафика выполняет поиск DNS и возвращает наиболее подходящую конечную точку пользователя. Вложенные профили могут определять приоритет основного расположения. Например, пользователи обычно подключаются к ближайшему географическому региону. Если в этом регионе возникает проблема, диспетчер трафика направляет пользователей в дополнительный регион. Такой подход гарантирует, что клиенты смогут подключаться к экземпляру приложения, даже если их ближайший географический регион недоступен.

Сведения о настройке конечных точек и маршрутизации см. в статье [Настройка метода географической маршрутизации трафика с помощью диспетчера трафика](../traffic-manager/traffic-manager-configure-geographic-routing-method.md).

### <a name="application-routing-with-azure-front-door-service"></a>Маршрутизация приложений с помощью службы "Передняя дверь" Azure

Использование протокола разбиения на части по протоколу TCP, [Служба "Передняя дверь" Azure](../frontdoor/front-door-overview.md) обеспечивает непрерывное подключение конечных пользователей к ближайшему POP передней дверцы (точка присутствия). Дополнительные возможности службы "Передняя дверца Azure" включают в себя завершение TLS, пользовательский домен, брандмауэр веб-приложения, переписывание URL-адресов и сходство сеансов. Оцените требования к трафику для приложения, чтобы выбрать оптимальное решение.

### <a name="interconnect-regions-with-global-virtual-network-peering"></a>Области Interconnect с глобальным пирингом виртуальной сети

Если кластеры должны взаимодействовать друг с другом, подключение между виртуальными сетями может осуществляться через [пиринг виртуальных сетей](../virtual-network/virtual-network-peering-overview.md). Эта технология соединяет виртуальные сети друг с другом, обеспечивая высокую пропускную способность в магистральной сети Майкрософт, даже в разных географических регионах.

Предварительные требования для пиринга между виртуальными сетями, в которых работают кластеры AKS, — это использование стандартных Load Balancer в кластере AKS, чтобы службы Kubernetes были доступны через пиринг виртуальных сетей.

## <a name="enable-geo-replication-for-container-images"></a>Включение георепликации для образов контейнеров

**Рекомендации**. Храните образы контейнеров в реестре контейнеров Azure и Георепликация реестра в каждом регионе AKS.

Чтобы развернуть и выполнить приложения в AKS, вам нужен определенный способ хранить и извлекать образы контейнеров. Реестр контейнеров интегрируется с AKS, поэтому он может безопасно хранить образы контейнеров или Helm диаграммы. Реестр контейнеров поддерживает репликацию с несколькими хозяевами для автоматической репликации образов в регионы Azure по всему миру. 

Чтобы повысить производительность и доступность, используйте георепликацию реестра контейнеров, чтобы создать реестр в каждом регионе, где имеется кластер AKS. Затем каждый кластер AKS извлекает образы контейнеров из локального реестра контейнеров в том же регионе:

![Георепликация реестра контейнеров для образов контейнеров](media/operator-best-practices-bc-dr/acr-geo-replication.png)

При использовании георепликации реестра контейнеров для извлечения образов из одного и того же региона результаты будут:

* **Быстрее**: вы извлекаюте изображения из высокоскоростных сетевых подключений с низкой задержкой в одном регионе Azure.
* **Более надежный**: Если регион недоступен, кластер AKS извлекает образы из доступного реестра контейнеров.
* **Дешевле**. между центрами обработки данных не взимается сетевая исходящий трафик.

Георепликация — это функция реестров контейнеров SKU *класса Premium* . Сведения о настройке георепликации см. в статье [Георепликация реестра контейнеров](../container-registry/container-registry-geo-replication.md).

## <a name="remove-service-state-from-inside-containers"></a>Удаление состояния службы из контейнеров

**Рекомендации**. где это возможно, не храните состояние службы в контейнере. Вместо этого используйте платформу Azure как службу (PaaS), которая поддерживает репликацию по регионам.

*Состояние службы* относится к данным в памяти или на диске, которые требуются службе для работы. Сюда входят структуры данных и переменные-члены, которые считываются и записываются службой. В зависимости от архитектуры службы состояние может также включать файлы или другие ресурсы, хранящиеся на диске. Например, состояние может включать файлы, которые база данных использует для хранения данных и журналов транзакций.

Состояние может быть либо внешним, либо совместно размещенным, с кодом, управляющим состоянием. Как правило, вы выносить состояние с помощью базы данных или другого хранилища данных, работающего на разных компьютерах по сети или использующих необработанный процесс на том же компьютере.

Контейнеры и микрослужбы наиболее устойчивы, когда процессы, выполняющиеся внутри них, не сохраняются в состоянии. Так как приложения почти всегда содержат определенное состояние, используйте решение PaaS, например Azure Cosmos DB, базу данных Azure для PostgreSQL, базу данных Azure для MySQL или базу данных SQL Azure.

Сведения о создании переносимых приложений см. в следующих рекомендациях.

* [Методология 12-факторного приложения](https://12factor.net/)
* [Выполнение веб-приложения в нескольких регионах Azure](/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Создание плана миграции хранилища

**Рекомендации**. Если вы используете службу хранилища Azure, подготовьте и проверьте, как перенести хранилище из основного региона в регион резервного копирования.

Приложения могут использовать службу хранилища Azure для своих данных. Так как приложения распределены между несколькими кластерами AKS в разных регионах, необходимо синхронизировать хранилище. Ниже приведены два распространенных способа репликации хранилища.

* Асинхронная репликация на основе инфраструктуры
* асинхронная репликация на основе приложений;

### <a name="infrastructure-based-asynchronous-replication"></a>Асинхронная репликация на основе инфраструктуры

Приложениям может требоваться постоянное хранилище даже после удаления Pod. В Kubernetes можно использовать постоянные тома для сохранения хранилища данных. Постоянные тома подключены к виртуальной машине узла, а затем доступны для модулей Pod. Постоянные тома следуют модулям Pod, даже если они перемещаются на другой узел в пределах одного кластера.

Используемая стратегия репликации зависит от решения хранилища. Общие решения для хранения данных, такие как [Gluster](https://docs.gluster.org/en/latest/Administrator-Guide/Geo-Replication/), [ЦЕФ](https://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [рук](https://rook.io/docs/rook/v1.2/ceph-disaster-recovery.html)и [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) , предоставляют собственные рекомендации по аварийному восстановлению и репликации.

Типичной стратегией является предоставление общей точки хранения, где приложения могут записывать свои данные. Затем эти данные реплицируются в другие регионы и предоставляются для локального доступа.

![Асинхронная репликация на основе инфраструктуры](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

При использовании управляемых дисков Azure существует несколько параметров, которые можно использовать для управления репликацией и аварийным восстановлением. [Велеро в Azure][velero] и [Кастен][kasten] являются резервными копиями собственных решений для Kubernetes, но не поддерживаются.

### <a name="application-based-asynchronous-replication"></a>асинхронная репликация на основе приложений;

В настоящее время Kubernetes не предоставляет собственной реализации для асинхронной репликации на основе приложений. Поскольку контейнеры и Kubernetes слабо связаны, должен работать любой традиционный подход к приложениям или языку. Как правило, приложения реплицируют запросы к хранилищу, которые затем записываются в базовое хранилище данных каждого кластера.

![асинхронная репликация на основе приложений;](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Дальнейшие действия

В этой статье рассматриваются вопросы обеспечения непрерывности бизнес-процессов и аварийного восстановления для кластеров AKS. Дополнительные сведения об операциях кластера в AKS см. в следующих статьях о рекомендациях:

* [Мультитенантность и изоляция кластеров][aks-best-practices-cluster-isolation]
* [Основные функции планировщика Kubernetes][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md

[velero]: https://github.com/vmware-tanzu/velero-plugin-for-microsoft-azure/blob/master/README.md
[kasten]: https://www.kasten.io/