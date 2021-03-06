---
title: Создание оценки AVS с помощью Azure "миграция сервера" | Документация Майкрософт
description: Описание создания оценки AVS с помощью средства Azure Migrate Server для оценки
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 06/26/2020
ms.openlocfilehash: e386db1ee2042d75a31d4a9de2a5174e904c6b5c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101732978"
---
# <a name="create-an-azure-vmware-solution-avs-assessment"></a>Создание оценки решения Azure VMware (AVS)

В этой статье описывается, как создать оценку решения Azure VMware (AVS) для локальных виртуальных машин VMware с помощью службы "миграция Azure": Оценка серверов.

Служба " [Миграция Azure](migrate-services-overview.md) " помогает выполнить миграцию в Azure. "Миграция Azure" — это центральный узел, с помощью которого можно отслеживать обнаружение, оценку и перенос локальной инфраструктуры, приложений и данных в Azure. Центр обеспечивает инструменты Azure для оценки и миграции, а также предложения сторонних независимых поставщиков программного обеспечения (ISV).

## <a name="before-you-start"></a>Прежде чем начать

- Убедитесь, что вы [создали](./create-manage-projects.md) проект "миграция Azure".
- Если вы уже создали проект, убедитесь, что вы [добавили](how-to-assess.md) средство Azure Migrate: Server для оценки серверов.
- Чтобы создать оценку, необходимо настроить устройство "миграция Azure" для [VMware](how-to-set-up-appliance-vmware.md), которое обнаруживает локальные компьютеры и отправляет метаданные и данные производительности в службу "миграция Azure": Оценка сервера. [Подробнее.](migrate-appliance.md)
- Кроме того, можно [импортировать метаданные сервера](./tutorial-discover-import.md) в формате значений с разделителями-запятыми (CSV).


## <a name="azure-vmware-solution-avs-assessment-overview"></a>Обзор оценки решения Azure VMware (AVS)

Существует два типа оценки, которые можно создать с помощью средства "Оценка серверов" службы "Миграция Azure".

**Тип оценки** | **Сведения**
--- | --- 
**Azure** | Оценки для переноса локальных серверов на виртуальные машины Azure. <br/><br/> Вы можете оценить локальные [виртуальные машины VMware](how-to-set-up-appliance-vmware.md), [виртуальные машины Hyper-V](how-to-set-up-appliance-hyper-v.md)и [физические серверы](how-to-set-up-appliance-physical.md) для миграции в Azure с помощью этого типа оценки. Дополнительные [сведения](concepts-assessment-calculation.md)
**Решение Azure VMware (AVS)** | Оценки для переноса локальных серверов в [Решение Azure VMware (AVS)](../azure-vmware/introduction.md). <br/><br/> С помощью этого типа оценки вы можете оценить возможность миграции на Решение Azure VMware (AVS) для локальных [виртуальных машин VMware](how-to-set-up-appliance-vmware.md).[Подробнее](concepts-azure-vmware-solution-assessment-calculation.md)

> [!NOTE]
> Оценку решения Azure VMware (AVS) можно создать только для виртуальных машин VMware.


Существует два типа критериев изменения размера, которые можно использовать для создания оценок решения Azure VMware (AVS).

**Оценка** | **Сведения** | **Data**
--- | --- | ---
**На основе производительности** | Оценки на основе собранных данных о производительности локальных виртуальных машин. | **Рекомендуемый размер узла**: на основе данных об использовании ЦП и памяти вместе с типом узла, типом хранилища и параметром ФТТ, выбранным для оценки.
**Как в локальной среде** | Оценки на основе определения размера в локальной среде. | **Рекомендуемый размер узла**: на основе размера локальной виртуальной машины, а также типа узла, типа хранилища и параметра ФТТ, выбранного для оценки.


## <a name="run-an-azure-vmware-solution-avs-assessment"></a>Запуск оценки решения Azure VMware (AVS)

1. На странице **Серверы** > **Серверы Windows и Linux** щелкните **Оценка и миграция серверов**.

   ![Расположение кнопки "Оценка и миграция серверов"](./media/tutorial-assess-vmware-azure-vmware-solution/assess.png)

1. В **Azure Migrate: Server Assessment** (Миграция Azure: оценка сервера) щелкните **Оценка**.

1. В окне **Оценка**  >  **типа оценки** серверов выберите **решение VMware для Azure (AVS)**.

1. В **источнике обнаружения**:

    - Если вы обнаружили компьютеры с помощью устройства, выберите **Компьютеры, обнаруженные с устройства Миграции Azure**.
    - Если вы обнаружили компьютеры с помощью импортированного CSV-файла, выберите **Импортируемые компьютеры**. 
    
1. Щелкните **Изменить**, чтобы просмотреть свойства оценки.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/assess-servers.png" alt-text="Страница выбора параметров оценки":::
 

1. В разделе **Свойства оценки** > **Свойства целевого объекта**:

    - В поле **Целевое расположение** выберите регион Azure, в который необходимо выполнить миграцию.
       - Рекомендации по размеру и стоимости основаны на указанном вами расположении.
   - Для параметра **Тип хранилища** по умолчанию используется значение **vSAN**. Это тип хранилища по умолчанию для частного облака AVS.
   - **Зарезервированные экземпляры** в настоящее время не поддерживаются для узлов AVS.
1. В поле **Размер виртуальной машины**:
    - Для параметра **Тип узла** по умолчанию используется значение **AV36**. Миграция Azure рекомендует узел из узлов, необходимых для миграции виртуальных машин в AVS.
    - В раскрывающемся списке **Параметр FTT, уровень RAID** выберите допустимое число сбоев и сочетание RAID.  Выбранный параметр FTT в сочетании с требованиями локального диска виртуальной машины определяет общий размер хранилища vSAN, необходимого в AVS.
    - В поле **Превышение лимита для ЦП** укажите отношение количества виртуальных ядер, связанных с одним физическим ядром, в узле AVS. Превышение лимита более чем 4:1 может привести к снижению производительности, но может использоваться для рабочих нагрузок типа веб-сервера.
    - В поле **Коэффициент перефиксирования памяти** укажите отношение объема памяти при фиксации в кластере. Значение 1 соответствует 100% использования памяти, 0,5, например, 50%, а 2 будет использовать 200% доступной памяти. Можно добавлять значения от 0,5 до 10 до одного десятичного разряда.
    - В поле Коэффициент дедупликации **и сжатия** укажите предполагаемый коэффициент дедупликации и сжатия для рабочих нагрузок. Фактическое значение можно получить из локальной конфигурации vSAN или хранилища. это может зависеть от рабочей нагрузки. Значение 3 означает 3 раза, поэтому для дискового пространства емкостью 300 ГБ будет использоваться только память. Значение 1 означает отсутствие дедупликации или сжатия. Можно добавлять значения от 1 до 10 до одного десятичного знака.
1. В разделе **Размер узла**: 
    - В поле **Условия изменения размера** выберите, следует ли основывать оценку на статических метаданных или на данных, основанных на производительности. При использовании данных о производительности:
        - В поле **Журнал производительности** укажите период, данные за который должны стать основой для оценки.
        - В поле **Использование процентиля** укажите значение процентиля, которое будет использоваться для примера производительности. 
    - В поле **Фактор комфорта** укажите буфер, который вы хотите использовать во время оценки. Он учитывается, например, для сезонного использования и малого количества записей в журнале с потенциальным повышением в будущем. Например, если вы используете фактор комфорта "2":
    
        **Компонент** | **Эффективное использование** | **Добавление фактора комфорта (2.0)**
        --- | --- | ---
        Ядра | 2  | 4
        Память | 8 ГБ | 16 Гб  

1. В разделе **Цены**:
    - В раскрывающемся списке **Предложение** указывается [предложение Azure](https://azure.microsoft.com/support/legal/offer-details/), в котором вы зарегистрированы. Средство оценки серверов оценит стоимость этого предложения.
    - В поле **Валюта** выберите валюту выставления счетов для своей учетной записи.
    - В поле **Скидка (%)** добавьте все скидки, относящиеся к подписке и предоставляемые в рамках предложения Azure. Значение по умолчанию — 0 %.

1. При внесении изменений нажмите кнопку **Сохранить**.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/avs-view-all.png" alt-text="Свойства оценки":::

1. В поле **Оценка серверов** щелкните **Далее**.

1. В разделе **Выбор компьютеров для оценки** > **Имя оценки** укажите имя для оценки. 
 
1. В разделе **Создание или выбор группы** выберите **Создать** и укажите имя группы. 
    
    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/assess-group.png" alt-text="Добавление виртуальных машин в группу":::
 
1. Выберите устройство, а также виртуальные машины, которые необходимо добавить в группу. Затем нажмите кнопку **Далее**.

1. На странице **Review + create assessment** (Проверка и создание оценки) проверьте сведения об оценке и щелкните **Создать оценку**, чтобы создать группу и запустить оценку.

    > [!NOTE]
    > Если вы хотите выполнить оценку на основе производительности, рекомендуется подождать по крайней мере один день после запуска обнаружения, прежде чем создавать оценку. Благодаря этому вы сможете получить данные о производительности с большей достоверностью. В идеале после запуска обнаружения следует дождаться заданной длительности производительности (день/неделя/месяц), что позволит получить оценку с высокой достоверностью.


## <a name="review-an-azure-vmware-solution-avs-assessment"></a>Обзор оценки решения Azure VMware (AVS)

Оценка решения Azure VMware (AVS) описывает:

- **Готовность решения VMware для Azure (AVS)**: указывает, подходят ли локальные виртуальные машины для миграции в решение VMware для Azure (AVS).
- **Количество узлов AVS**: предполагаемое количество узлов AVS, необходимых для запуска виртуальных машин.
- **Использование на УЗЛАХ AVS**: планируемое использование ЦП, памяти и хранилища на всех узлах.
    - Использование включает в себя рефакторинг в следующих перезаголовках управления кластерами, например vCenter Server, НСКС Manager (крупный), НСКС ребро, если ХККС развертывается также диспетчер ХККС и IX, использующий ~ 44vCPU (11 ЦП), 75 ГБ ОЗУ и 722GB хранилища, до сжатия и дедупликации.
- **Ежемесячная оценка затрат**: приблизительные ежемесячные затраты на все узлы решения Azure VMware (AVS), на которых выполняются локальные виртуальные машины.


### <a name="view-an-assessment"></a>Просмотр оценки

1. В разделе **Цели миграции** >  **Серверы** щелкните **Оценки** в области **Azure Migrate: Server Assessment** (Миграция Azure: оценка сервера).

2. В разделе **Оценки** щелкните оценку, чтобы открыть сведения о ней.

    :::image type="content" source="./media/how-to-create-avs-assessment/avs-assessment-summary.png" alt-text="Сводка оценки AVS":::

### <a name="review-azure-vmware-solution-avs-readiness"></a>Проверка готовности решения VMware для Azure (AVS)

1. В окне готовность к работе с **Azure** проверьте, готовы ли виртуальные машины к миграции в AVS.

2. Проверьте состояние виртуальной машины:
    - **Готово** к установке AVS. компьютер можно перенести как есть в Azure (AVS) без каких-либо изменений. Он будет запущен в AVS с полной поддержкой AVS.
    - **Готовность к работе**. возможно, у виртуальной машины есть проблемы совместимости с текущей версией vSphere, а также требуются средства VMware и другие параметры, прежде чем можно будет достичь полной функциональности виртуальной машины в AVS.
    - **Не готово для AVS**: виртуальная машина не запустится в AVS. Например, если локальная виртуальная машина VMware имеет подключенное внешнее устройство, например компакт-диск, операция VMotion завершится ошибкой (если используется VMware VMotion).
    - **Неизвестная готовность**: службе миграции Azure не удалось определить готовность компьютера из-за недостатка метаданных, собранных из локальной среды.

3. Ознакомьтесь с предлагаемым средством.
    - **VMware хккс или Enterprise**: для виртуальных машин VMware — это рекомендуемое средство миграции, которое позволяет перенести локальную рабочую нагрузку в частное облако решения Azure VMware (AVS). [Подробнее](../azure-vmware/tutorial-deploy-vmware-hcx.md)
    - **Неизвестно**: для виртуальных машин, импортированных с помощью CSV-файла, инструмент миграции по умолчанию неизвестен. Но для виртуальных машин VMware мы рекомендуем использовать решение VMware Hybrid Cloud Extension (HCX). 

4. Щелкните состояние **готовности AVS** . Вы можете просмотреть сведения о готовности виртуальной машины и сведения о самой виртуальной машине, включая вычисления, хранилище и параметры сети.

### <a name="review-cost-details"></a>Просмотр сведений о затратах

В этом представлении отображаются оценочные затраты на выполнение виртуальных машин в решении Azure VMware (AVS).

1. Просмотрите общую сумму ежемесячных расходов. Затраты объединяются для всех виртуальных машин в оцениваемой группе. 

    - Оценка затрат зависит от количества узлов AVS, необходимых для учета требований к ресурсам всех виртуальных машин в целом.
    - Так как ценообразование для решения Azure VMware (AVS) относится к каждому узлу, Общая стоимость не имеет стоимости вычислений и распределения затрат на хранение.
    - Оценка затрат предназначена для запуска локальных виртуальных машин в AVS. Оценка AVS не учитывает затраты на PaaS и SaaS.
    
2. Вы можете просмотреть примерную стоимость хранилища в месяц. В этом представлении показаны совокупные затраты на хранение для оцениваемой группы, распределенные по дискам хранения разных типов.

3. Вы можете воспользоваться детализацией для просмотра сведений об определенных виртуальных машинах.


### <a name="review-confidence-rating"></a>Просмотр рейтинга достоверности

При запуске оценок на основе производительности для них назначается оценка достоверности.

![Оценка достоверности](./media/how-to-create-assessment/confidence-rating.png)

- Можно получить оценку от 1 звезды (самая низкая) до 5 звезд (самая высокая).
- Оценка достоверности помогает определить надежность рекомендаций по выбору размера, предоставленных оценкой.
- Оценка достоверности назначается на основе доступности точек данных, необходимых для вычисления оценки.
- Для изменения размера на основе производительности для оценки AVS в оценке сервера требуются данные об использовании ЦП и памяти виртуальной машины. Следующие данные о производительности собираются, но не используются в рекомендациях по размерам для оценки AVS:
  - Данные дискового ввода-вывода и пропускной способности для каждого диска, подключенного к виртуальной машине.
  - Сетевой ввод-вывод для управления размером на основе производительности для каждого сетевого адаптера, подключенного к виртуальной машине.

Ниже приведены оценки достоверности.

**Доступность точки данных** | **Оценка достоверности**
--- | ---
0–20 % | 1 звезда
21–40 % | 2 звезды
41–60 % | 3 звезды
61–80 % | 4 звезды
81–100 % | 5 звезд

Дополнительные [сведения](concepts-azure-vmware-solution-assessment-calculation.md) о данных производительности 


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как использовать [сопоставление зависимостей](how-to-create-group-machine-dependencies.md) для создания групп с высокой степенью достоверности.
- [Узнайте больше](concepts-azure-vmware-solution-assessment-calculation.md) о том, как рассчитываются оценки AVS.