---
title: Подробная конфигурация для размещенного тестового диска в Azure Marketplace
description: Объяснение действий по настройке размещенного тестового диска в коммерческом магазине
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: trkeya
ms.author: trkeya
ms.date: 11/06/2020
ms.openlocfilehash: 88779f67a2fa9b18f0177a1459b32c672343bb57
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96462946"
---
# <a name="detailed-configuration-for-hosted-test-drives"></a>Подробная конфигурация для размещенных тестовых дисков

В этой статье описывается настройка размещенного тестового диска для Dynamics 365 для служб взаимодействия с клиентами или Dynamics 365 для операций.

## <a name="configure-for-dynamics-365-customer-engagement"></a>Настройка для служб взаимодействия с клиентами Dynamics 365

1. Войдите в [Центр партнеров](https://partner.microsoft.com/).
2. Если вы не можете получить доступ к указанной выше ссылке, необходимо [Отправить запрос,](https://appsource.microsoft.com/partners/list-an-app) чтобы опубликовать приложение. После просмотра запроса вам будет предоставлен доступ для запуска процесса публикации.
3. Найдите существующее предложение **dynamics 365 для участия клиентов** или создайте новое предложение **Dynamics 365 для** взаимодействия с клиентами.
4. Установите флажок **включить тестовый диск** и выберите **тип тестового диска** (см. маркер ниже), а затем нажмите кнопку **сохранить**.

    [![Установите флажок "включить тестовый диск".](media/test-drive/enable-test-drive-check-box.png)](media/test-drive/enable-test-drive-check-box.png#lightbox)

    - **Тип тестового диска** — выберите **Microsoft Hosted (Dynamics 365 для участия клиентов & PowerApps)**. Это означает, что корпорация Майкрософт будет размещать и обслуживать службу, которая выполняет подготовку и отмену подготовки пользователей тестового диска.

5. Предоставьте Microsoft AppSource разрешение на предоставление и отмену подготовки пользователей тестовых дисков в клиенте с помощью [этих инструкций](./test-drive-azure-subscription-setup.md). На этом шаге вы создадите **идентификатор Azure AD App** и **Azure AD App ключевые** значения, указанные ниже.
6. Заполните эти поля на странице **технической конфигурации тестового диска** .

    [![Страница Техническая конфигурация тестового диска.](media/test-drive/technical-config-details.png)](media/test-drive/technical-config-details.png#lightbox)

    - Максимальное число одновременных **тестовых дисков** — количество параллельных пользователей, которые могут одновременно работать с активным тестовым устройством. Каждый пользователь будет использовать лицензию Dynamics, пока их тестовый накопитель активен, поэтому убедитесь, что у вас есть по крайней мере столько лицензий Dynamics, сколько доступно для пользователей тестового диска. Мы рекомендуем использовать от 3 до 5.
    - **Длительность тестового диска** — количество часов, в течение которых тестовый диск пользователя будет активным. По истечении этого времени пользователь будет отозван из вашего клиента. Мы рекомендуем 2-24 часов в зависимости от сложности приложения. Пользователь всегда может запросить другой тестовый диск, если он закончится неоднократно и вы снова хотите получить доступ к тестовому диску.
    - **URL-адрес экземпляра** — URL-адрес, на который будет отправлен пользователь тестового диска при запуске тестового диска. Обычно это URL-адрес экземпляра Dynamics 365, на котором установлено приложение и образец данных. Пример значения: `https://testdrive.crm.dynamics.com` .
    - **URL-адрес Web API экземпляра** — URL-адрес API для экземпляра Dynamics 365. Получите это значение, выполнив вход в экземпляр Microsoft Dynamics 365 и перейдя к **настройке**  >  **Customization**  >  **Developer Resources**  >  **веб-API экземпляра** ресурсов разработчика и скопировав адрес (URL-адрес). Пример значения:

        :::image type="content" source="./media/test-drive/sample-web-api-url.png" alt-text="Пример веб-API экземпляра.":::

    - **Имя роли** — имя пользовательской роли безопасности Dynamics 365, созданной для тестового диска, или можно использовать существующую роль. Для входа в экземпляр взаимодействия с клиентами новая роль должна обладать минимальными необходимыми привилегиями, добавленными в роль. Ознакомьтесь с [минимальными правами доступа, необходимыми для входа в Microsoft Dynamics 365](https://community.dynamics.com/crm/b/crminogic/archive/2016/11/24/minimum-privileges-required-to-login-microsoft-dynamics-365). Это роль, которая будет назначена пользователям во время тестового диска. Пример значения: `testdriverole` .
    
        > [!IMPORTANT]
        > Убедитесь, что проверка группы безопасности не добавлена. Это позволяет синхронизировать пользователя с экземпляром взаимодействия с клиентом.

    - **Идентификатор клиента Azure Active Directory** — идентификатор клиента Azure для экземпляра Dynamics 365. Чтобы получить это значение, войдите в портал Azure и перейдите к **Azure Active Directory**  >  **Свойства** и скопируйте идентификатор каталога. Пример значения: 172f988bf-86f1-41af-91ab-2d7cd01112341.
    - **Azure Active Directory имя клиента** — имя клиента Azure для экземпляра Dynamics 365. Используйте следующий формат: `<tenantname>.onmicrosoft.com`. Пример значения: `testdrive.onmicrosoft.com` .
    - **Azure Active Directory Application ID** — идентификатор приложения Azure Active Directory (AD), созданного на шаге 5. Пример значения: `53852862-a2ae-4e43-9461-faa49650a096` .
    - **Секрет клиента приложения Azure Active Directory** — секрет для приложения Azure AD, созданного на шаге 5. Пример значения: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=` .

7. Укажите сведения о списке Marketplace. Выберите **язык** , чтобы просмотреть дополнительные обязательные поля.

    [![Страница сведений о списке Marketplace.](media/test-drive/marketplace-listing-details.png)](media/test-drive/marketplace-listing-details.png#lightbox)

    - **Описание** — обзор тестового диска. Этот текст будет отображаться для пользователя во время подготовки тестового диска. Это поле поддерживает HTML, поэтому вы можете предоставить форматированное содержимое.
    - **Руководство пользователя** . Руководство пользователя PDF, помогающее протестировать пользователей, чтобы понять, как использовать приложение.
    - **Демонстрационное видео о тестовых** выпусках — видеоролик, демонстрирующий ваше приложение (необязательно).

## <a name="configure-for-dynamics-365-operations"></a>Настройка для операций Dynamics 365

2. Если вы не можете получить доступ к указанной выше ссылке, необходимо [Отправить запрос,](https://appsource.microsoft.com/partners/list-an-app) чтобы опубликовать приложение. После просмотра запроса вам будет предоставлен доступ для запуска процесса публикации.
3. Найдите существующее предложение **dynamics 365 для операций** или создайте новое предложение **Dynamics 365 for Operations** .
4. Установите флажок **включить тестовый диск** и выберите **тип тестового диска** (см. маркер ниже), а затем нажмите кнопку **сохранить**.

    [![Установите флажок "включить тестовый диск".](media/test-drive/enable-test-drive-check-box-operations.png)](media/test-drive/enable-test-drive-check-box-operations.png#lightbox)

    - **Тип тестового диска** — выберите **Dynamics 365 для параметра Operations** . Это означает, что корпорация Майкрософт будет размещать и обслуживать службу, которая выполняет подготовку и отмену подготовки пользователей тестового диска.

5. Предоставьте Microsoft AppSource разрешение на предоставление и отмену подготовки пользователей тестовых дисков в клиенте с помощью [этих инструкций](https://github.com/Microsoft/AppSource/blob/master/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md). На этом шаге вы создадите **идентификатор Azure AD App** и **Azure AD App ключевые** значения, указанные ниже.
6. Заполните эти поля на странице **технической конфигурации тестового диска** .

    [![Страница технической настройки Marketplace.](media/test-drive/technical-config-details.png)](media/test-drive/technical-config-details.png#lightbox)

    - Максимальное число одновременных **тестовых дисков** — количество параллельных пользователей, которые могут одновременно работать с активным тестовым устройством. Каждый пользователь будет использовать лицензию Dynamics, пока их тестовый накопитель активен, поэтому убедитесь, что у вас есть по крайней мере столько лицензий Dynamics, сколько доступно для пользователей тестового диска. Мы рекомендуем использовать от 3 до 5.
    - **Длительность тестового диска** — количество часов, в течение которых тестовый диск пользователя будет активным. По истечении этого времени пользователь будет отозван из вашего клиента. Мы рекомендуем 2-24 часов в зависимости от сложности приложения. Пользователь всегда может запросить другой тестовый диск, если он закончится неоднократно и вы снова хотите получить доступ к тестовому диску.
    - **URL-адрес экземпляра** — URL-адрес, на который будет отправлен пользователь тестового диска при запуске тестового диска. Обычно это URL-адрес экземпляра Dynamics 365, на котором установлено приложение и образец данных. Пример значения: `https://testdrive.crm.dynamics.com` .
    - **Идентификатор клиента Azure Active Directory** — идентификатор клиента Azure для экземпляра Dynamics 365. Чтобы получить это значение, войдите в портал Azure и перейдите к **Azure Active Directory**  >  **Свойства** и скопируйте идентификатор каталога. Пример значения: 172f988bf-86f1-41af-91ab-2d7cd01112341.
    - **Azure Active Directory имя клиента** — имя клиента Azure для экземпляра Dynamics 365. Используйте следующий формат: `<tenantname>.onmicrosoft.com`. Пример значения: `testdrive.onmicrosoft.com` .
    - **Azure Active Directory Application ID** — идентификатор приложения Azure Active Directory (AD), созданного на шаге 5. Пример значения: `53852862-a2ae-4e43-9461-faa49650a096` .
    - **Секрет клиента приложения Azure Active Directory** — секрет для приложения Azure AD, созданного на шаге 5. Пример значения: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=` .
    - **Пробная версия юридического лица** — предоставление юридического лица для назначения пробной версии. Вы можете создать новый объект при [создании или изменении юридического лица](/dynamicsax-2012/appuser-itpro/create-or-modify-a-legal-entity).
    - **Имя роли** — имя AOT (дерево объектов приложения) пользовательской роли безопасности Dynamics 365, созданной для тестового диска. Это роль, которая будет назначена пользователям во время тестового диска.

        :::image type="content" source="./media/test-drive/security-config.png" alt-text="Страница настройки безопасности.":::

7. Укажите сведения о списке Marketplace. Выберите **язык** , чтобы просмотреть дополнительные обязательные поля.

    [![Страница сведений о списке Marketplace.](media/test-drive/marketplace-listing-details.png)](media/test-drive/marketplace-listing-details.png#lightbox)

    - **Описание** — обзор тестового диска. Этот текст будет отображаться для пользователя во время подготовки тестового диска. Это поле поддерживает HTML, поэтому вы можете предоставить форматированное содержимое.
    - **Руководство пользователя** . Руководство пользователя PDF, помогающее протестировать пользователей, чтобы понять, как использовать приложение.
    - **Демонстрационное видео о тестовых** выпусках — видеоролик, демонстрирующий ваше приложение (необязательно).

<!--
## Next steps

- [Set up your Azure subscription](test-drive-azure-subscription-setup.md) -->