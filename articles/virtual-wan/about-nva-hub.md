---
title: 'Виртуальная глобальная сеть Azure: сведения о сетевом виртуальном устройстве в концентраторе'
description: В этой статье вы узнаете о виртуальных сетевых устройствах в виртуальном концентраторе глобальной сети.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: scottnap
Customer intent: As someone with a networking background, I want to learn about Network Virtual Appliances in the Virtual WAN hub.
ms.openlocfilehash: 365ed60e73be9bb2098022fa767f4ae54b93c37c
ms.sourcegitcommit: e46f9981626751f129926a2dae327a729228216e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98028086"
---
# <a name="about-network-virtual-appliance-in-an-azure-virtual-wan-hub-preview"></a>Сведения о сетевом виртуальном устройстве в виртуальном концентраторе глобальной сети Azure (Предварительная версия)

Виртуальная глобальная сеть Azure работала с сетевыми партнерами для создания автоматизации, которая позволяет легко подключать свое клиентское оборудование (CPE) к VPN-шлюзу Azure в виртуальном концентраторе. Azure работает с выбранными сетевыми партнерами, чтобы клиенты могли развертывать сетевые виртуальные устройства сторонних производителей (NVA) непосредственно в виртуальном концентраторе. Это позволяет клиентам, желающим подключить свое АБОНЕНТское лицо к одной и той же торговой марке, NVA в виртуальном концентраторе, чтобы они могли воспользоваться преимуществами комплексных возможностей SD-WAN.

Barracuda сети и Cisco Systems являются первыми партнерами по предоставлению NVA, который можно развернуть непосредственно в виртуальном концентраторе глобальной сети.  Документацию по продукту [Barracuda КЛАУДЖЕН WAN](https://www.barracuda.com/products/cloudgenwan) и [Cisco Cloud для нескольких облаков](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/cloudonramp/ios-xe-17/cloud-onramp-book-xe/cloud-onramp-multi-cloud.html#Cisco_Concept.dita_c61e0e7a-fff8-4080-afee-47b81e8df701) см. здесь. Azure работает с дополнительными партнерами, поэтому вы должны увидеть другие предложения.

> [!NOTE]
> Только предложения NVA, доступные для развертывания в виртуальном концентраторе глобальной сети, можно развернуть в виртуальном концентраторе глобальной сети. Они не могут быть развернуты в произвольной виртуальной сети в Azure.

## <a name="how-does-it-work"></a><a name="how"></a>Как это работает?

NVA, доступные для развертывания непосредственно в виртуальном концентраторе глобальной сети Azure, разработаны специально для использования в виртуальном концентраторе. Предложение NVA публикуется в Azure Marketplace как управляемое приложение, а клиенты могут развернуть предложение непосредственно из Azure Marketplace или развернуть предложение из виртуального концентратора с помощью портал Azure.

:::image type="content" source="./media/about-nva-hub/high-level-process.png" alt-text="Общие сведения о процессе":::

Предложение NVA для каждого партнера будет иметь немного другие возможности и функции в зависимости от требований к развертыванию. Однако существует несколько вещей, которые являются общими для всех предложений партнеров для NVA в виртуальном концентраторе глобальной сети.

* Взаимодействие управляемого приложения, предлагаемое в Azure Marketplace.
* Емкость и выставление счетов на основе единицы инфраструктуры NVA.
* Метрики работоспособности, выходящие через Azure Monitor.

### <a name="managed-application"></a><a name="managed"></a>Управляемое приложение

Все предложения NVA, доступные для развертывания в виртуальном концентраторе глобальной сети, будут иметь **управляемое приложение** , доступное в Azure Marketplace. Управляемые приложения позволяют партнерам выполнять следующие действия:

* Создайте пользовательский интерфейс развертывания для своих NVA.
* Предоставьте специализированный шаблон диспетчер ресурсов, который позволит им создавать NVA непосредственно в виртуальном концентраторе глобальной сети.
* Выставление счетов за лицензирование программного обеспечения напрямую или с помощью Azure Marketplace.
* Предоставление пользовательских свойств и показателей ресурсов.

Партнеры NVA могут создавать различные ресурсы в зависимости от их развертывания, лицензирования конфигурации и потребностей управления. Когда клиент создает NVA в виртуальном концентраторе глобальной сети, как и все управляемые приложения, в подписке будут созданы две группы ресурсов.

* **Группа ресурсов клиента** — это будет содержать заполнитель приложения для управляемого приложения. Партнеры могут использовать его для предоставления любых свойств клиентов, которые они выбирают здесь.
* **Управляемая группа ресурсов** . клиенты не могут настраивать или изменять ресурсы в этой группе ресурсов напрямую, так как это управляется издателем управляемого приложения. Эта группа ресурсов будет содержать ресурс **нетворквиртуалапплианцес** .

:::image type="content" source="./media/about-nva-hub/managed-app.png" alt-text="Группы ресурсов управляемых приложений":::

### <a name="nva-infrastructure-units"></a><a name="units"></a>Единицы инфраструктуры NVA

При создании NVA в виртуальном концентраторе глобальной сети необходимо выбрать число единиц инфраструктуры NVA, с которыми вы хотите развернуть. **Единица инфраструктуры NVA** — это единица совокупного объема пропускной способности для NVA в виртуальном КОНЦЕНТРАТОРЕ глобальной сети. **Единица инфраструктуры NVA** аналогична [единице масштабирования](pricing-concepts.md#scale-unit) VPN с точки зрения того, как вы думаете о емкости и размере.

* 1 NVAная единица инфраструктуры представляет 500 Мбит/с для всех подключений сайтов филиалов, поступающих в эту NVA, по цене на $0.25/час.
* Azure поддерживает от 1-80 единиц инфраструктуры NVA для данного развертывания виртуального концентратора NVA.
* Каждый партнер может предложить разные пакеты единиц инфраструктуры NVA, которые являются подмножеством всех поддерживаемых конфигураций модулей инфраструктуры NVA.

Как и в случае с единицами масштабирования VPN, если выбрать *1 NVA инфраструктуры = 500 Мбит/* с, это означает, что будут созданы два экземпляра для избыточности, каждая из которых имеет максимальную пропускную способность 500 Мбит/с. Например, если у вас пять ветвей, каждая из которых использует 10 Мбит/с, то на начальном узле потребуется пропускная способность в 50 Мбит/с. Планирование агрегатной емкости NVA должно выполняться после оценки емкости, необходимой для поддержки количества ветвей в центре.

## <a name="network-virtual-appliance-configuration-process"></a><a name="configuration"></a>Процесс настройки виртуального сетевого устройства

Партнеры работали над тем, чтобы предоставить опыт, который автоматически настраивает NVA в рамках процесса развертывания. После подготовки NVA в виртуальном концентраторе любая дополнительная конфигурация, которая может потребоваться для NVA, должна быть выполнена через портал NVA Partners или управляющее приложение. Прямой доступ к NVA недоступен.

## <a name="site-and-connection-resources-with-nvas"></a><a name="resources"></a>Ресурсы сайта и подключения с помощью NVA

В отличие от конфигураций VPN-шлюзов Azure вам не нужно создавать ресурсы **сайта** , ресурсы **подключения типа "сеть —** сеть" или ресурсы **подключения типа "точка — сеть** " для подключения сайтов филиалов к NVA в виртуальном концентраторе глобальной сети. Управление ими осуществляется через партнера NVA.

Вам по-прежнему нужно создавать подключения между виртуальными сетями для подключения виртуального концентратора глобальной сети к виртуальной сети Azure.

## <a name="supported-regions"></a><a name="regions"></a>Поддерживаемые регионы

NVA в виртуальном концентраторе доступна для предварительной версии в следующих регионах:

|Геополитический регион | Регионы Azure|
|---|---|
| Северная Америка| Западная часть США, Юго-Центральный регион США, восток США 2   |
| Южная Америка | Южная Бразилия |
| Европа | Западная Европа, южная часть Соединенного Королевства|
|  Ближний Восток | Северная часть ОАЭ; |
| Азия | Восточная Япония |
| Австралия | Восточная Австралия |

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ

### <a name="i-am-a-network-appliance-partner-and-want-to-get-our-nva-in-the-hub--can-i-join-this-partner-program"></a>Я являюсь партнером по сетевым устройствам и хочу получить наши NVA в центре.  Можно ли присоединиться к этой партнерской программе?

К сожалению, в настоящее время у нас нет возможности использовать новые предложения для партнеров. Ознакомьтесь с нами в ноябре!

### <a name="can-i-deploy-any-nva-from-azure-marketplace-into-the-virtual-wan-hub"></a>Можно ли развернуть любой NVA из Azure Marketplace в виртуальном концентраторе глобальной сети?

В настоящее время для развертывания в виртуальном концентраторе глобальной сети можно развернуть только [Barracuda КЛАУДЖЕН WAN](https://aka.ms/BarracudaMarketPlaceOffer) и [Приложение Cisco Cloud вван](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/cisco.cisco_cloud_vwan_app?tab=Overview) .

### <a name="what-is-the-cost-of-the-nva"></a>Какова стоимость NVA?

Необходимо приобрести лицензию для NVA от поставщика NVA.  Сведения о лицензии Barracuda Клауджен WAN NVA from Barracuda см. на [странице Barracuda WAN](https://www.barracuda.com/products/cloudgenwan). В настоящее время Cisco предлагает только модель лицензирования BYOL (с собственным лицензированием), которая должна быть приобретена непосредственно компанией Cisco. Кроме того, вам будет взиматься плата за использование единиц инфраструктуры NVA, которые вы используете, и любых других используемых ресурсов. Дополнительные сведения см. в статье [Основные понятия ценообразования](pricing-concepts.md).

### <a name="can-i-deploy-an-nva-to-a-basic-hub"></a>Можно ли развернуть NVA на базовом концентраторе?

Нет. Если требуется развернуть NVA, необходимо использовать стандартный концентратор.

### <a name="can-i-deploy-an-nva-into-a-secure-hub"></a>Можно ли развернуть NVA в безопасном концентраторе?

Да. Barracuda Клауджен WAN можно развернуть в концентраторе с помощью брандмауэра Azure.

### <a name="can-i-connect-any-cpe-device-in-my-branch-office-to-barracuda-cloudgen-wan-nva-in-the-hub"></a>Можно ли подключить любое устройство CPE в филиале, чтобы Barracuda Клауджен WAN NVA в концентраторе?

Нет. Barracuda Клауджен WAN совместима только с устройствами с Barracuda CPE. Дополнительные сведения о требованиях к глобальной сети Клауджен см. в разделе [Barracuda КЛАУДЖЕН WAN Page](https://www.barracuda.com/products/cloudgenwan). Для Cisco существует несколько устройств с SD-WAN, которые являются совместимыми. Дополнительные сведения о совместимости КПЕС см. в статье [Cloud документации по для нескольких облаков](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/cloudonramp/ios-xe-17/cloud-onramp-book-xe/cloud-onramp-multi-cloud.html#Cisco_Concept.dita_c61e0e7a-fff8-4080-afee-47b81e8df701) .

### <a name="what-routing-scenarios-are-supported-with-nva-in-the-hub"></a>Какие сценарии маршрутизации поддерживаются с NVA в центре?

Все сценарии маршрутизации, поддерживаемые виртуальной глобальной сетью, поддерживаются с NVA в концентраторе.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о виртуальной глобальной сети см. в статье [Обзор виртуальной глобальной сети](virtual-wan-about.md) .
