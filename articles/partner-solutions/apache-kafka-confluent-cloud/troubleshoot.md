---
title: Устранение неполадок Apache Kafka для несамостоятельного облака — решения партнеров Azure
description: В этой статье приводятся сведения об устранении неполадок и часто задаваемых вопросах (FAQ) о свободном облаке в Azure.
ms.service: partner-services
ms.topic: conceptual
ms.date: 02/18/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: b1e4b06fcbecf11d7d5f58a583fe3bd6643d99ec
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101709404"
---
# <a name="troubleshooting-apache-kafka-for-confluent-cloud-solutions"></a>Устранение неполадок Apache Kafka, связанных с неfluent облачными решениями

Этот документ содержит сведения об устранении неполадок решений, использующих Apache Kafka для работы в облаке.

Если вы не нашли ответ или не можете устранить проблему, [Создайте запрос через портал Azure](manage.md#get-support) или обратитесь в [службу поддержки Fluent](https://support.confluent.io).

## <a name="cant-find-offer-in-the-marketplace"></a>Не удается найти предложение в Marketplace

Чтобы найти предложение в Azure Marketplace, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com) выберите **Создать ресурс**.
1. Выполните поиск _Apache Kafka в облаке с fluentм_.
1. Выберите плитку приложения.

Если предложение не отображается, обратитесь в [службу поддержки Fluent](https://support.confluent.io). Идентификатор клиента Azure Active Directory должен быть в списке разрешенных клиентов. Чтобы узнать, как найти идентификатор клиента, см. статью [как найти идентификатор клиента Azure Active Directory](../../active-directory/fundamentals/active-directory-how-to-find-tenant.md).

## <a name="purchase-errors"></a>Ошибки покупки

* Не удается выполнить покупку, так как допустимая кредитная карта не подключена к подписке Azure или метод оплаты не связан с подпиской.

  Используйте другую подписку Azure. Или добавьте или обновите кредитную карту или метод оплаты для подписки. Дополнительные сведения см. [в разделе Обновление метода кредита и оплаты](../../cost-management-billing/manage/change-credit-card.md).

* Подписка EA не допускает покупки в Marketplace.

  Используйте другую подписку. Или проверьте, включена ли подписка EA для покупки в Marketplace. Дополнительные сведения см. в статье [Включение покупок в Marketplace](../../cost-management-billing/manage/ea-azure-marketplace.md#enabling-azure-marketplace-purchases). Если эти параметры не устраняют проблему, обратитесь в [службу поддержки Fluent](https://support.confluent.io).

## <a name="conflict-error"></a>конфликт

Если вы ранее зарегистрировались для работы в облаке, необходимо использовать новый адрес электронной почты, чтобы создать еще один ресурс облачной организации. При использовании ранее зарегистрированного адреса электронной почты возникнет ошибка **конфликта** . Повторите регистрацию, но на этот раз с новым адресом электронной почты.

## <a name="deploymentfailed-error"></a>DeploymentFailed, ошибка

Если вы получаете ошибку **DeploymentFailed** , проверьте состояние подписки Azure. Убедитесь, что она не приостановлена и у вас нет проблем с выставлением счетов.

## <a name="resource-isnt-displayed"></a>Ресурс не отображается

Если **Обзор** или **Удаление** Blade-столбцов для облака, которые находятся в области Fluent, не отображаются на портале, попробуйте обновить страницу. Эта ошибка может быть временной проблемой с порталом. Если это не поможет, обратитесь в [службу поддержки Fluent](https://support.confluent.io).

Если облачный ресурс Fluent не найден в списке **ресурсов** Azure, обратитесь в [службу поддержки Fluent](https://support.confluent.io).

## <a name="resource-creation-takes-long-time"></a>Создание ресурса занимает много времени

Если для завершения процесса развертывания требуется более трех часов, обратитесь в службу поддержки.

Если развертывание завершается сбоем, а состояние облачного ресурса находится в состоянии `Failed` , удалите ресурс. После удаления повторите попытку, чтобы создать ресурс.

## <a name="offer-plan-doesnt-load"></a>План предложения не загружается

Эта ошибка может быть временной проблемой портал Azure. Попробуйте развернуть предложение еще раз.

## <a name="unable-to-delete"></a>Не удалось удалить файл

Если не удается удалить ресурсы, убедитесь, что у вас есть разрешения на удаление ресурса. Необходимо иметь разрешение на использование действий Microsoft. Fluent/*/DELETE. Дополнительные сведения о просмотре разрешений см. [в разделе Список назначений ролей Azure с помощью портал Azure](../../role-based-access-control/role-assignments-list-portal.md).

Если у вас есть необходимые разрешения, но по-прежнему не удается удалить ресурс, обратитесь в [службу поддержки Fluent](https://support.confluent.io). Это условие может быть связано с политикой хранения, которой не владеет. Поддержка недостаточной поддержки может удалить организацию и адрес электронной почты.

## <a name="unable-to-use-single-sign-on"></a>Не удается использовать единый вход

Если единый вход не работает на портале SaaS для облака, убедитесь, что вы используете правильный Azure Active Directory электронной почте. Кроме того, необходимо предоставить разрешение на доступ к порталу SaaS (по отношению к облачному программному обеспечению). Дополнительные сведения см. в [руководстве по единому входу](manage.md#single-sign-on).

Если проблема не исчезнет, обратитесь в [службу поддержки Fluent](https://support.confluent.io).

## <a name="next-steps"></a>Дальнейшие действия

Сведения об [управлении экземпляром](manage.md) Apache Kafka для использования в облаке.
