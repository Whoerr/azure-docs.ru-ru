---
title: Оптимизация затрат на Хранилище дисков Azure с помощью резервирований
description: Узнайте о приобретении Хранилище дисков Azure резервированиях, чтобы снизить затраты на управляемые диски SSD уровня "Премиум".
author: roygara
ms.author: rogarana
ms.date: 01/30/2020
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: db6575894904e6ced2d4be48fec5961f5b8b8a54
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98602630"
---
# <a name="reduce-costs-with-azure-disks-reservation"></a>Сокращение расходов с помощью резервирования дисков Azure

Сэкономьте на Хранилище дисков Azure использовании с зарезервированной емкостью. Хранилище дисков Azure резервирования в сочетании с Azure Reserved Virtual Machine Instances позволяет снизить общую стоимость виртуальной машины. Скидка на резервирование автоматически применяется к соответствующим дискам в выбранной области резервирования. Из-за этого автоматического приложения вам не нужно назначать резервирование для управляемого диска, чтобы получить скидки.

Скидки применяются каждый час в зависимости от использования диска. Неиспользованные зарезервированные ресурсы не переносятся. Скидки на резервирование Хранилище дисков Azure не применяются к неуправляемым дискам, Ultra Disks или страничному BLOB-объекту.

## <a name="determine-your-storage-needs"></a>Определение потребностей в хранении

Прежде чем приобретать резервирование, определите потребности в хранении. В настоящее время резервирования Хранилище дисков Azure доступны только для выбранных номеров SKU SSD Azure Premium. SKU твердотельного накопителя уровня "Премиум" определяет размер и производительность диска.

При определении потребностей в хранении не думайте о том, что диски основаны только на емкости. Например, вы не можете зарезервировать диск P40 и использовать его для оплаты двух небольших P30 дисков. При покупке резервирования вы только закупаете резервирование только для общего числа дисков на номер SKU.

Резервирование диска устанавливается на номер SKU диска. В результате потребление резервирования зависит от единицы номера SKU диска, а не от указанного размера.

Например, предположим, что вы резервируете один диск P40, который имеет 2 Тиб подготовленной емкости хранилища. Также предположим, что вы выделили только два диска P30. Резервирование P40 в этом случае не учитывает потребление P30 и оплачивается тариф по мере использования на дисках P30.
<br/>
<br/>
[!INCLUDE [disk-storage-premium-ssd-sizes](../../includes/disk-storage-premium-ssd-sizes.md)]

## <a name="purchase-considerations"></a>Вопросы, связанные с приобретением

При рассмотрении приобретения резервирования дисков рекомендуется принять во внимание следующие рекомендации.

- Проанализируйте сведения об использовании, чтобы определить, какие резервирования следует приобрести. Убедитесь, что вы следите за использованием в номерах дисков, а не подготовленных или используемых дисковых мощностях.
- Изучите резервирование дисков и резервирование виртуальных машин. Мы настоятельно рекомендуем использовать резервирование как для виртуальной машины, так и для использования диска в целях максимальной экономии. Вы можете начать с определения правильного резервирования виртуальной машины, а затем оценить резервирование диска. Как правило, у вас есть Стандартная конфигурация для каждой рабочей нагрузки. Например, сервер SQL Server может иметь два диска данных P40 и один диск операционной системы P30.
  
  Этот тип шаблона помогает определить зарезервированный объем, который вы можете приобрести. Этот подход позволяет упростить процесс оценки и обеспечить согласованный план для виртуальной машины и дисков. План содержит такие рекомендации, как подписки или регионы.

## <a name="purchase-restrictions"></a>Ограничения на приобретение

В настоящее время скидки на резервирование недоступны для следующих:

- Неуправляемые диски или страничные BLOB-объекты.
- Стандартные твердотельные накопители или стандартные жесткие диски (HDD).
- Номера SKU SSD (цен. категория "Премиум") меньше, чем P30: P1, P2, P3, P4, P6, P10, P15 и P20 SSD.
- Диски в Azure для государственных организаций, Azure для Германии или в области китайского региона Azure.

В редких обстоятельствах Azure ограничивает покупку новых резервирований подмножеством номеров SKU дисков из-за нехватки емкости в регионе.

## <a name="buy-a-disk-reservation"></a>Приобретение резервирования диска

Вы можете приобрести Хранилище дисков Azure резервирования с помощью [портал Azure](https://portal.azure.com/). Вы можете платить за резервирование перед или с ежемесячными платежами. Дополнительные сведения о приобретении с ежемесячными платежами см. [в разделе покупка резервирования с ежемесячными платежами](../cost-management-billing/reservations/prepare-buy-reservation.md#buy-reservations-with-monthly-payments).

Чтобы приобрести зарезервированную емкость, выполните следующие действия.

1. Перейдите на панель « [Покупка резервирования](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Browse_AddCommand) » в портал Azure.

1. Выберите **управляемые диски Azure** , чтобы приобрести резервирование.

    ![Панель для резервирования покупок](media/disks-reserved-capacity/disks-reserved-purchase-reservation.png) 

1. Укажите необходимые значения, описанные в следующей таблице.

   |Элемент  |Описание  |
   |---------|---------|
   |**Область**   |  Сколько подписок может использовать преимущества выставления счетов, связанные с резервированием. Это значение также указывает, как резервирование применяется к конкретным подпискам. <br/><br/> Если выбран **общий доступ**, скидка резервирования применяется к емкости хранилища Azure в каждой подписке в контексте выставления счетов. Контекст выставления счетов зависит от способа регистрации в Azure. Для клиентов с соглашением Enterprise общая область действует в рамках регистрации и включает в себя все подписки, заданные при регистрации. Для клиентов с оплатой по мере использования общая область включает все индивидуальные подписки с тарифами оплаты по мере использования, созданными администратором учетной записи.  <br/><br/>  При выборе **одной подписки** скидка на резервирование применяется к емкости хранилища Azure в выбранной подписке. <br/><br/> Если выбрать **одну группу ресурсов**, скидка резервирования будет применена к емкости хранилища Azure в выбранной подписке и выбранной группе ресурсов этой подписки. <br/><br/> Вы можете изменить область резервирования после приобретения резервирования.  |
   |**Подписка**  | Подписка, используемая для оплаты резервирования службы хранилища Azure. Метод оплаты в выбранной подписке используется для зарядки затрат. Подписка должна иметь один из следующих типов:<br/><ul><li> Соглашение Enterprise (номера предложений MS-AZR-0017P и MS-AZR-0148P). Для корпоративной подписки плата вычитается из предварительной оплаты Azure (ранее называемой денежным обязательством) или выставляется за превышение.</li><br/><li>Отдельная подписка с оплатой по мере использования (номера предложений MS-AZR-0003P и MS-AZR-0023P). Для отдельной подписки с тарифами с оплатой по мере использования плата взимается по методу оплаты кредитной карты или счета-фактуры в подписке.</li></ul>    |
   | **диски;** | Номер SKU, который вы хотите создать. |
   | **Регион** | Регион, в котором действует резервирование. |
   | **Периодичность выставления счетов** | Частота выставления счетов за резервирование. Параметры включают **ежемесячные** **и** предварительные. |

    ![Область для выбора продукта, который требуется purchase.png](media/disks-reserved-capacity/premium-ssd-reserved-purchase-selection.png)

1. После указания значений для резервирования портал Azure отображает затраты. На портале также отображается процент скидки по выставлению счетов с оплатой по мере использования. Нажмите кнопку **Далее** , чтобы перейти на панель **резервирования покупки** .

1. На панели **резервирования покупки** можно присвоить имя резервированию и выбрать общее количество резервирований, которое вы хотите сделать. Число резервирований сопоставляется с количеством дисков. Например, если вы хотите зарезервировать сотни дисков, введите значение **Quantity** **100**.

1. Ознакомьтесь с общей стоимостью резервирования.

    ![Панель «Покупка резервирования»](media/disks-reserved-capacity/premium-ssd-reserved-selecting-sku-total-purchase.png)

После приобретения резервирования оно автоматически применяется к любым существующим Хранилище дисков ресурсам, которые соответствуют условиям резервирования. Если вы еще не создали ресурсы Хранилище дисков, резервирование применяется при каждом создании ресурса, удовлетворяющего условиям резервирования. В любом случае срок резервирования начинается сразу после успешной покупки.

## <a name="cancel-exchange-or-refund-reservations"></a>Отмена, обмен резервирования, возмещение средств за резервирование

Вы можете отменять, обмениваться и возмещать в определенные ограничения резервирования. Дополнительные сведения см. в статье [Self-service exchanges and refunds for Azure Reservations](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md) (Самостоятельное выполнение обмена и возмещения средств для резервирований Azure).

## <a name="expiration-of-a-reservation"></a>Срок действия резервирования

По истечении срока хранения любая Хранилище дисков Azureная емкость, используемая при резервировании, оплачивается по тарифу оплаты по мере использования. Резервирования не продлеваются автоматически.

Вы получите по электронной почте уведомление через 30 дней до истечения срока действия резервирования и снова на дату окончания срока действия. Чтобы продолжить использовать преимущества, которые предоставляет резервирование, обновите его позже даты окончания срока действия.

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами

Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Дальнейшие действия

- [Основные сведения о резервировании в Azure](../cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Узнайте, как применяется скидка на резервирование для Хранилище дисков Azure](../cost-management-billing/reservations/understand-disk-reservations.md)