---
title: Устранение неполадок для Datadog — решения партнеров Azure
description: В этой статье содержатся сведения об устранении неполадок Datadog в Azure.
ms.service: partner-services
ms.topic: conceptual
ms.date: 02/19/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: a8bb28892fe42215876b5cc8771ae73c7d2aab7f
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747116"
---
# <a name="troubleshooting-datadog-on-azure"></a>Устранение неполадок Datadog в Azure

Этот документ содержит сведения об устранении неполадок решений, использующих Datadog.

## <a name="unable-to-create-datadog-resource"></a>Не удалось создать ресурс Datadog

Чтобы настроить интеграцию с Azure Datadog, необходимо иметь права **владельца** на подписку Azure. Перед началом установки убедитесь, что у вас есть соответствующий доступ.

## <a name="single-sign-on-errors"></a>Ошибки единого входа

**Не удалось сохранить параметры единого входа** . Эта ошибка возникает в другом корпоративном приложении, использующем идентификатор SAML Datadog. Чтобы узнать, какое приложение использует его, выберите **изменить** в разделе Базовая конфигурация SAML.

Чтобы устранить эту проблему, отключите другое приложение или используйте другое приложение в качестве корпоративного приложения для настройки единого входа SAML с помощью Datadog. Если вы решили использовать другое приложение, убедитесь, что приложение имеет [необходимые параметры](create.md#configure-single-sign-on).

**Приложение не отображается на странице параметров единого входа** . сначала найдите идентификатор приложения. Если результат не отображается, проверьте параметры SAML приложения. В сетке отображаются только приложения с правильными параметрами SAML. 

URL-адрес идентификатора должен иметь значение `https://us3.datadoghq.com/account/saml/metadata.xml` .

URL-адрес ответа должен иметь значение `https://us3.datadoghq.com/account/saml/assertion` .

На следующем рисунке показаны правильные значения.
  
:::image type="content" source="media/troubleshoot/troubleshooting.png" alt-text="Проверьте параметры SAML для приложения Datadog в AAD." border="true":::

**Приглашенным гостевым пользователям, которые приглашаются в клиент, не удается получить доступ к единому входу** . у некоторых пользователей есть два адреса электронной почты в портал Azure. Как правило, одним адресом электронной почты является имя участника-пользователя (UPN), а другой — альтернативный адрес электронной почты.

При приглашении гостевого пользователя используйте UPN домашнего клиента. С помощью имени участника-пользователя вы сохраняете адрес электронной почты в процессе единого входа. Имя участника-пользователя можно найти, выполнив поиск адреса электронной почты в правом верхнем углу портал Azure пользователя.
  
## <a name="logs-not-being-emitted"></a>Журналы не выдаются

Журналы Datadog могут выдаваться только ресурсам, перечисленным в категории журнала ресурсов Azure Monitor. Чтобы проверить, отправляет ли ресурс журналы в Datadog, перейдите к параметру диагностики Azure для конкретного ресурса. Убедитесь в наличии параметра диагностики Datadog.

:::image type="content" source="media/troubleshoot/diagnostic-setting.png" alt-text="Параметр диагностики Datadog в ресурсе Azure" border="true":::

## <a name="metrics-not-being-emitted"></a>Метрики не выдаются

Ресурсу Datadog назначается роль **читателя мониторинга** в соответствующей подписке Azure. Эта роль позволяет ресурсу Datadog получать метрики и отсылать Эти метрики в Datadog.

Чтобы убедиться, что ресурс имеет правильное назначение ролей, откройте портал Azure и выберите подписку. В левой области выберите **Управление доступом (IAM)**. Найдите имя ресурса Datadog. Убедитесь, что ресурс Datadog имеет назначение роли **читателя мониторинга** , как показано ниже.

:::image type="content" source="media/troubleshoot/datadog-role-assignment.png" alt-text="Назначение роли Datadog в подписке Azure" border="true":::

## <a name="datadog-agent-installation-fails"></a>Сбой установки агента Datadog

Интеграция Azure Datadog предоставляет возможность установки агента Datadog на виртуальной машине или в службе приложений. Для настройки агента Datadog используется ключ API, выбранный в качестве **ключа по умолчанию** на экране ключи API. Если ключ по умолчанию не выбран, установка агента Datadog завершится ошибкой.

Если в агенте Datadog был настроен неверный ключ, перейдите на экран ключи API и измените **ключ по умолчанию**. Необходимо удалить агент Datadog и переустановить его, чтобы настроить виртуальную машину с новыми ключами API.

## <a name="next-steps"></a>Дальнейшие действия

Сведения об [управлении экземпляром](manage.md) Datadog.
