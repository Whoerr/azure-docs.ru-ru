---
title: Руководство по настройке NetSuite OneWorld для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Сведения о том, как настроить единый вход Azure Active Directory в NetSuite OneWorld.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 294870d3448886b9cea573a0e79b3ac436941f89
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2021
ms.locfileid: "98696495"
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Руководство по настройке NetSuite для автоматической подготовки пользователей

Цель этого руководства — показать, как в NetSuite OneWorld и Azure AD настроить автоматическую подготовку и отзыв учетных записей пользователей из Azure AD в NetSuite.

> [!WARNING]
> Эта интегрированная возможность подготовки прекратит действовать в феврале 2020 года из-за изменения интерфейсов API NetSuite, используемых корпорацией Майкрософт для подготовки пользователей в NetSuite. В результате функции подготовки приложения NetSuite в коллекции корпоративных приложений Azure Active Directory вскоре будут удалены. На функции единого входа приложения это не повлияет. Корпорация Майкрософт совместно с NetSuite работает над новой модернизированной интегрируемой возможностью подготовки, но дата ее реализации пока неизвестна.

## <a name="prerequisites"></a>Предварительные требования

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

*   клиент Azure Active Directory;
*   Подписка NetSuite OneWorld. Обратите внимание, что автоматическую подготовку пользователей сейчас поддерживает только NetSuite OneWorld.
*   Учетная запись пользователя в Netsuite с разрешениями администратора.
*   Для интеграции с Azure AD требуется освобождение от двухфакторной проверки подлинности. Чтобы запросить такое освобождение, обратитесь в службу поддержки NetSuite.

## <a name="assigning-users-to-netsuite-oneworld"></a>Назначение пользователей в NetSuite OneWorld

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки учетной записи синхронизированы будут только те пользователи и группы, которые были "назначены" приложению в Azure AD.

Перед настройкой и включением службы подготовки необходимо решить, какие пользователи или группы в Azure AD представляют пользователей, которым требуется доступ к приложению NetSuite. Когда данное решение будет принято, можно будет назначить этих пользователей приложению NetSuite, следуя указаниям ниже.

[Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-netsuite-oneworld"></a>Важные советы по назначению пользователей в NetSuite OneWorld

*   Рекомендуется назначить одного пользователя Azure AD в NetSuite для тестирования конфигурации подготовки. Дополнительные пользователи и/или группы можно назначить позднее.

*   При назначении пользователя NetSuite необходимо выбрать допустимую роль пользователя. Роль "Доступ по умолчанию" не работает для подготовки.

## <a name="enable-user-provisioning"></a>Включение подготовки пользователей

В этом разделе описывается подключение вашего каталога Azure AD к API подготовки учетных записей пользователей в NetSuite и настройка службы подготовки для создания, обновления и отключения назначенных учетных записей пользователей в NetSuite на основе назначения пользователей и групп в Azure AD.

> [!TIP] 
> Для NetSuite можно также включить единый вход на основе SAML. Для этого следуйте инструкциям на [портале Azure](https://portal.azure.com). Единый вход можно настроить независимо от автоматической подготовки, хотя эти две возможности дополняют друг друга.

### <a name="to-configure-user-account-provisioning"></a>Чтобы настроить подготовку учетных записей пользователей:

В этом разделе показано, как включить подготовку учетных записей пользователей Active Directory для NetSuite.

1. На [портале Azure](https://portal.azure.com) перейдите в раздел **Azure Active Directory > Корпоративные приложения > Все приложения**.

1. Если в NetSuite уже настроен единый вход, найдите свой экземпляр NetSuite с помощью поля поиска. В противном случае щелкните **Добавить** и выполните поиск **NetSuite** в коллекции приложений. Выберите NetSuite в результатах поиска и добавьте в список приложений.

1. Выберите экземпляр NetSuite, а затем перейдите на вкладку **Подготовка**.

1. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически). 

    ![Снимок экрана страницы "NetSuite — Подготовка" со значением "Авто" для параметра "Режим подготовки" и другими значениями, которые можно задать.](./media/netsuite-provisioning-tutorial/provisioning.png)

1. В разделе **Учетные данные администратора** укажите следующие параметры конфигурации.
   
    a. В текстовом поле **Имя пользователя администратора** введите имя учетной записи NetSuite, которой на сайте NetSuite.com назначен профиль **System Administrator** (Системный администратор).
   
    b. В текстовом поле **Пароль администратора** введите пароль для этой учетной записи.
      
1. На портале Azure щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к приложению NetSuite.

1. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок.

1. Нажмите кнопку **Сохранить**.

1. В разделе "Сопоставления" выберите **Синхронизировать пользователей Azure Active Directory с NetSuite**.

1. В разделе **Сопоставления атрибутов** просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD в NetSuite. Обратите внимание, что атрибуты, выбранные как свойства в разделе **Сопоставление**, используются для сопоставления учетных записей пользователей в NetSuite для операций обновления. Нажмите кнопку "Сохранить", чтобы подтвердить все изменения.

1. Чтобы включить службу подготовки Azure AD для NetSuite, измените значение параметра **Состояние подготовки** на **Включено** в разделе "Параметры".

1. Нажмите кнопку **Сохранить**.

После этого будет запущена начальная синхронизация всех пользователей и групп, назначенных для NetSuite в разделе "Пользователи и группы". Обратите внимание, что начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **Сведения о синхронизации** можно отслеживать ход синхронизации и с помощью ссылок просматривать журналы действий по подготовке, в которых описаны все действия, выполняемые службой подготовки для приложения NetSuite.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Настройка единого входа](netsuite-tutorial.md)
