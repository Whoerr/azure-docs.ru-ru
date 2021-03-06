---
title: Руководство по интеллектуальному управлению запасами на основе Интернета вещей | Документация Майкрософт
description: Руководство по шаблону приложения для интеллектуального управления запасами для IoT Central.
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
ms.date: 10/20/2019
ms.openlocfilehash: 3dbb6ca64451cb60d5a8ec67ecdc528865a4438c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101719089"
---
# <a name="tutorial-deploy-and-walk-through-a-smart-inventory-management-application-template"></a>Руководство по развертыванию шаблона приложения для интеллектуального управления запасами для IoT Central и его использованию

В этом руководстве показано, как приступить к работе, развернув шаблон приложения **для интеллектуального управления запасами** для IoT Central. Вы узнаете, как развернуть шаблон, который готов к использованию, и что можно будет сделать дальше.

Из этого руководства вы узнаете, как:

> [!div class="checklist"]
> * создать приложение для интеллектуального управления запасами; 
> * ознакомиться с этим приложением. 

## <a name="prerequisites"></a>Предварительные требования

* Для развертывания этого приложения не требуется выполнять какие-либо предварительные требования.
* Рекомендуется иметь подписку Azure, но вы можете поработать с приложением даже без нее.

## <a name="create-smart-inventory-management-application-template"></a>Создание приложения для интеллектуального управления запасами

Вы можете создать приложение, выполнив следующие действия.

1. Перейдите на веб-сайт диспетчера приложений в Azure IoT Central. Выберите **Build** (Сборка) в области навигации слева, а затем перейдите на вкладку **Retail** (Розничная торговля).

    :::image type="content" source="media/tutorial-iot-central-smart-inventory-management/iotc_retail_homepage.png" alt-text="Снимок экрана: выбор шаблона интеллектуального управления запасами managementapplication":::

2. Выберите вкладку **Розничная торговля** и щелкните **Создать приложение** в разделе **Интеллектуальное управление запасами**

3. Для **создания приложения** откроется форма нового приложения, которая будет заполнена запрошенными данными, как показано ниже.
    **Application name** (Имя приложения): можно использовать предложенное имя по умолчанию или ввести понятное имя приложения.
    **URL** (URL-адрес): можно использовать предложенный URL-адрес по умолчанию или ввести понятный уникальный URL-адрес. Если у вас уже есть подписка Azure, то рекомендуется использовать параметр по умолчанию. Вы можете начать с 7-дневной бесплатной пробной версии тарифного плана и выбрать преобразование в стандартный тарифный план в любое время до истечения срока действия бесплатной версии.
    **Billing Info** (Данные для выставления счетов): сведения о каталоге, подписке Azure и регионе необходимы для подготовки ресурсов.
    **Создание**. Выберите "Create" (Создать) в нижней части страницы, чтобы развернуть приложение.

    :::image type="content" source="media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_app_create.png" alt-text="Снимок экрана: создание приложения на основе шаблона интеллектуального управления запасами managementapplication":::

    :::image type="content" source="media/tutorial-iot-central-smart-inventory-management/smart-inventory-management-app-create-billinginfo.png" alt-text="Снимок экрана: варианты выставления счетов при создании приложения":::

## <a name="walk-through-the-application"></a>ознакомиться с этим приложением. 

### <a name="dashboard"></a>Панель мониторинга 

После успешного развертывания шаблона приложения панель мониторинга по умолчанию является порталом оператора интеллектуального управления запасами. Northwind Trader — это вымышленный поставщик интеллектуального управления запасами, управляющий хранилищем со сканерами BLE и магазином розничной торговли со сканерами RFID. На этой панели мониторинга вы увидите два шлюза, предоставляющих данные телеметрии о запасах вместе со связанными командами, заданиями и действиями, которые можно выполнить. Эта панель мониторинга предварительно настроена для демонстрации важных операций устройства интеллектуального управления запасами.
Панель мониторинга логически поделена для отображения операций управления устройством, связанных с двумя шлюзами. 
   * Хранилище развернуто со стационарным шлюзом BLE и сканерами BLE на палетах для отслеживания запасов на крупном объекте.
   * Магазин розничной торговли оснащен стационарным шлюзом RFID и сканерами RFID для каждой отдельной позиции для отслеживания запасов на складе магазина.
   * Просмотр расположения шлюзов, сведений о состоянии и связанных сведений 

> [!div class="mx-imgBorder"]
> ![Снимок экрана: верхняя половина панели мониторинга интеллектуального управления запасами managementdashboard](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_dashboard1.png)

   * Вы можете легко отслеживать общее число шлюзов, активных и неизвестных радиожетонов.
   * Вы можете выполнять такие операции управления устройствами, как обновление встроенного ПО, отключение датчика, включение датчика, изменение порога датчика, изменение интервалов передачи телеметрии и обновление контрактов службы устройства.
   * Устройства шлюза могут выполнять управление запасами по запросу, осуществляя полное или добавочное сканирование.

> [!div class="mx-imgBorder"]
> ![Снимок экрана: нижняя половина панели мониторинга интеллектуального управления запасами managementdashboard](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_dashboard2.png)

## <a name="device-template"></a>Шаблон устройства
Перейдите на вкладку "Device templates" (Шаблоны устройств), и вы увидите модель возможностей шлюза. Модель возможностей основана на двух разных интерфейсах, **Gateway Telemetry & Property** (Телеметрия и свойства шлюза) и **Gateway Commands** (Команды шлюза).

**Gateway Telemetry & Property** (Телеметрия и свойства шлюза). Этот интерфейс представляет все данные телеметрии, связанные с датчиками, расположением и сведениями об устройстве, а также возможностями свойств двойника устройства, такими как пороги и интервалы обновления шлюзов.

> [!div class="mx-imgBorder"]
> ![Снимок экрана: шаблон устройства шлюза управления запасами в приложении](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_devicetemplate1.png)


**Gateway Commands** (Команды шлюза). Этот интерфейс организовывает все возможности команд шлюза.

> [!div class="mx-imgBorder"]
> ![Снимок экрана: интерфейс команд шлюза в шаблоне устройства шлюза управления запасами](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_devicetemplate2.png)

## <a name="rules"></a>Правила
Перейдите на вкладку "Rules" (Правила), чтобы просмотреть два правила в этом шаблоне приложения. Эти правила настроены для отправки уведомлений по электронной почте операторам для дальнейшего исследования.

**Gateway offline** (Шлюз не в сети): это правило активируется, если шлюз долго не передает данные в облако. Шлюз может не отвечать из-за низкого заряда аккумулятора, потери подключения или неработоспособности устройства.

**Unknown tags** (Неизвестные радиожетоны): очень важно отслеживать все радиожетоны RFID и BLE, связанные с ресурсом. Если шлюз обнаруживает слишком много неизвестных радиожетонов, это указывает на проблемы синхронизации, связанные с приложениями для работы с радиожетонами.

> [!div class="mx-imgBorder"]
> ![Снимок экрана: список правил в шаблоне интеллектуального управления запасами managementapplication](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_rules.png)

## <a name="jobs"></a>Задания
Перейдите на вкладку "Jobs" (Задания), чтобы увидеть пять заданий, входящих в состав этого шаблона приложения. Функцию заданий можно использовать для выполнения операций для всего решения. Здесь задания управления запасами используют команды устройства и возможности двойника для выполнения таких задач, как:
   * отключение сканеров для всех шлюзов;
   * изменение порогов телеметрии; 
   * проверка запасов по запросу во всем решении.

> [!div class="mx-imgBorder"]
> ![Снимок экрана: список заданий в шаблоне интеллектуального управления запасами managementapplication](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_jobs.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не собираетесь в дальнейшем использовать это приложение, удалите шаблон приложения, выбрав **Administration** (Администрирование)  > **Application settings** (Параметры приложения) и щелкнув **Delete** (Удалить).

> [!div class="mx-imgBorder"]
> ![Снимок экрана: удаление приложения после завершения работы с ним](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_cleanup.png)

## <a name="next-steps"></a>Дальнейшие действия

См. сведения об интеллектуальном управлении запасами:

> [!div class="nextstepaction"]
> [Понятия, связанные с интеллектуальным управлением запасами](./architecture-smart-inventory-management.md)
