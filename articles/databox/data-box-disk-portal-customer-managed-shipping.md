---
title: Самоуправляемая доставка Диска Microsoft Azure Data Box | Документация Майкрософт
description: Описание процесса самоуправляемой доставки Дисков Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 02/02/2021
ms.author: alkohli
ms.openlocfilehash: f512b4415f4a83e779a8f9bf790ba2806e3b05c5
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526336"
---
# <a name="use-self-managed-shipping-for-azure-data-box-disk-in-the-azure-portal"></a>Использование функции самоуправляемой доставки для Дисков Azure Data Box на портале Azure

В этой статье описываются действия, выполняемые в процессе самоуправляемой доставки Диска Azure Data Box (заказ, вывоз и сдача). Вы можете управлять Диском Azure Data Box через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Самоуправляемая доставка доступна как один из вариантов при [заказе Диска Azure Data Box](data-box-disk-deploy-ordered.md). Самоуправляемая доставка доступна только в следующих регионах:

* US (США)
* Соединенное Королевство
* Западная Европа
* Австралия
* Япония
* Сингапур
* Южная Корея
* ЮАР
* Индия (Предварительная версия)

## <a name="use-self-managed-shipping"></a>Использование самоуправляемой доставки

При размещении заказа на Диск Data Box можно выбрать вариант самоуправляемой доставки.

1. В заказе на Диск Azure Data Box в разделе **Контактные сведения** выберите **+ Добавить адрес доставки**.

   ![Снимок экрана: Мастер заказов, отображающий шаг контактных данных с вызываемым параметром добавить адрес доставки.](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. При выборе типа доставки выберите вариант **Самоуправляемая доставка**. Этот вариант доступен только в том случае, если вы находитесь в одном из поддерживаемых регионов, указанных в разделе "Предварительные требования".

3. После предоставления адреса доставки необходимо проверить его и выполнить заказ.

   ![Снимок экрана диалогового окна добавления адреса доставки с параметрами доставки и добавления адреса доставки.](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. После подготовки устройства и получения уведомления по электронной почте можно запланировать отправку. В заказе на Диск Azure Data Box перейдите в раздел **Обзор** и выберите **Запланировать вывоз**.

   ![Заказ Диска Data Box для вывоза](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-01b.png)

5. Следуйте инструкциям в разделе **Планирование вывоза для Azure**. Прежде чем вы получите код авторизации, вам необходимо будет отправить сообщение на адрес [adbops@microsoft.com](mailto:adbops@microsoft.com), чтобы запланировать вывоз устройств из регионального центра обработки данных.

   ![Расписание отправки](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-02c.png)

6. После того как вы запланировали отправку устройств, вы можете просмотреть код авторизации в **расписании отправки для Azure**.

   ![Снимок экрана: диалоговое окно "Выбор расписания для Azure" с вызываемым текстовым полем "код авторизации для отправки".](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-01b.png)

   Запишите этот код авторизации.

   Согласно требованиям безопасности, во время планирования выбора необходимо предоставить имя пользователя, который будет поставляться на выбор.

   Кроме того, необходимо предоставить сведения о том, кто будет переходить в центр обработки данных для выбора. Вы или точка контакта должны нести утвержденный правительственный идентификатор фотографии, который будет проверяться в центре обработки данных.

   Пользователь, который настраивает устройство, также должен иметь код авторизации. Код авторизации уникален для подбора или отбрасывания и проверяется в центре обработки данных.

7. После выбора устройства в центре обработки данных заказ автоматически переходит в состояние " **выбрано** ".

   ![Принято курьером](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-ready-disk-01b.png)

8. Получив Диск Data Box, скопируйте на него необходимые данные в локальной среде. После копирования данных подготовьте Диск Data Box к отправке.

   Когда вы завершите копирование данных, обратитесь к операциям, чтобы запланировать встречу для отбрасывания. Необходимо предоставить общий доступ к сведениям о пользователе, поступающем в центр обработки данных, чтобы удалить диски. Кроме того, в центре обработки данных при сдаче проверят код авторизации. Код авторизации для удаления можно найти в портал Azure в разделе **Расписание раскрытия**.

   > [!NOTE]
   > Не отправляйте код авторизации по электронной почте. Это необходимо проверить только в центре обработки данных во время удаления.

9. Когда вы получите встречу для отбрасывания, заказ должен быть **готов к получению в состоянии центра обработки данных Azure** в портал Azure.

   ![Снимок экрана: диалоговое окно "Добавление адреса доставки" с именем "доставка с использованием вариантов выхода" и "добавить адрес доставки".](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-dropoff-02b.png)

10. После проверки идентификатора и кода авторизации и удаления устройства в центре обработки данных необходимо **получить** состояние заказа.

    ![Сдача завершена](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-received-01a.png)

11. После сдачи устройства копирование данных будет продолжено. После завершения копирования выполнение заказа завершено.

## <a name="next-steps"></a>Дальнейшие действия

* [Краткое руководство. Развертывание Диска Azure Data Box с помощью портала Azure](data-box-disk-quickstart-portal.md)
