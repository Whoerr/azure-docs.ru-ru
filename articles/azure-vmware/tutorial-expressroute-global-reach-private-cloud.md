---
title: Учебник по настройке пиринга между локальной средой и частным облаком
description: Создание пиринга ExpressRoute Global Reach к частному облаку в решении Azure VMware.
ms.topic: tutorial
ms.date: 01/27/2021
ms.openlocfilehash: 04be94849f1cd78ff915ca271845b283c8cfc64c
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100558803"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Руководство по Настройка пиринга между локальной средой и частным облаком

ExpressRoute Global Reach подключает локальную среду к частному облаку решения Azure VMware. Подключение ExpressRoute Global Reach устанавливается между локальными средами и каналом ExpressRoute частного облака с имеющимся подключением ExpressRoute. 

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Использование портала Azure для включения пиринга Global Reach ExpressRoute между локальной средой и частным облаком.


## <a name="before-you-begin"></a>Перед началом

Перед включением подключения между двумя каналами ExpressRoute с помощью ExpressRoute Global Reach ознакомьтесь с документацией по [включению подключения в разных подписках Azure](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions).  


## <a name="prerequisites"></a>Предварительные требования

- Отдельный работоспособный канал ExpressRoute, используемый для подключения локальных сред к Azure.
- Убедитесь, что все шлюзы, включая службу поставщика ExpressRoute, поддерживают 4-байтовые номера автономной системы (ASN). Решение Azure VMware использует 4-байтовые общедоступные номера ASN для объявления маршрутов.


## <a name="create-an-expressroute-authorization-key-in-the-on-premises-expressroute-circuit"></a>Создание ключа авторизации ExpressRoute в локальном канале ExpressRoute

1. В колонке **Каналы ExpressRoute** в разделе "Параметры" выберите **Авторизации**.

2. Введите имя ключа авторизации и нажмите **Сохранить**.

    :::image type="content" source="media/expressroute-global-reach/start-request-auth-key-on-premises-expressroute.png" alt-text="Выбор элемента &quot;Авторизации&quot; и ввод имени для ключа авторизации.":::
  
     После создания новый ключ появится в списке ключей авторизации для канала.
 
 4. Запишите ключ авторизации и идентификатор ExpressRoute. Вы будете использовать их на следующем шаге для завершения настройки пиринга.
 
 
 ## <a name="peer-private-cloud-to-on-premises"></a>Пиринг между частным облаком и локальной средой

1. На странице **Обзор** частного облака в разделе "Управление" выберите **Подключение > ExpressRoute Global Reach > Добавить**.

    :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="В меню выберите &quot;Подключение&quot;, перейдите на вкладку &quot;ExpressRoute Global Reach&quot;, а затем выберите &quot;Добавить&quot;.":::

2. Введите идентификатор ExpressRoute и ключ авторизации, созданные с помощью инструкций из предыдущего раздела.

    :::image type="content" source="./media/expressroute-global-reach/on-premises-cloud-connections.png" alt-text="Ввод идентификатора ExpressRoute и ключа авторизации, а также выбор элемента &quot;Создать&quot;.":::

3. Нажмите кнопку **создания**. Новое подключение отобразится в списке локальных облачных подключений.  

>[!TIP]
>Вы можете удалить или отключить подключение в списке, выбрав элемент **Еще**.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Отключение или удаление локального подключения":::


## <a name="next-steps"></a>Дальнейшие действия

С помощь этого учебника вы узнали, как включить пиринг Global Reach ExpressRoute между локальной средой и частным облаком. 

Перейдите к следующему руководству, чтобы узнать, как развернуть и настроить решение VMware HCX для частного облака решения Azure VMware.

> [!div class="nextstepaction"]
> [Развертывание и настройка VMware HCX](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
