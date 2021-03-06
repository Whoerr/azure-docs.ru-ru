---
title: Различия в проверке по поддерживаемым типам учетных записей | Службы
titleSuffix: Microsoft identity platform
description: Сведения о различиях в проверке различных свойств для различных поддерживаемых типов учетных записей при регистрации приложения на платформе Microsoft Identity.
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 07/21/2020
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.openlocfilehash: eea2e587a075d774a25f479ec61575a002b57f75
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98937818"
---
# <a name="validation-differences-by-supported-account-types-signinaudience"></a>Различия в проверке по поддерживаемым типам счетов (Сигнинаудиенце)

При регистрации приложения с помощью платформы идентификации Майкрософт для разработчиков вам будет предложено выбрать типы учетных записей, которые поддерживает ваше приложение. В объекте приложения и манифесте это свойство имеет значение `signInAudience` .

Возможны следующие варианты:

- **Азуреадмйорг**: учетные записи в каталоге Организации, где зарегистрировано приложение (один клиент).
- **Азуреадмултиплеоргс**: учетные записи в любом каталоге организации (с несколькими клиентами).
- **Азуреадандперсоналмикрософтаккаунт**: учетные записи в любом каталоге организации (с несколькими клиентами) и личных учетных записей Майкрософт (например, Skype, Xbox и Outlook.com).

Для зарегистрированных приложений можно найти значения для поддерживаемых типов учетных записей в разделе **Проверка подлинности** приложения. Его также можно найти в `signInAudience` свойстве в **манифесте**.

Значение, выбранное для этого свойства, влияет на другие свойства объекта приложения. В результате, если изменить это свойство, может потребоваться сначала изменить другие свойства.

Различия в проверке различных свойств для различных поддерживаемых типов учетных записей см. в следующей таблице.

| Свойство | `AzureADMyOrg` | `AzureADMultipleOrgs` | `AzureADandPersonalMicrosoftAccount` и `PersonalMicrosoftAccount` |
|--------------|---------------|----------------|----------------|
| URI идентификатора приложения ( `identifierURIs` )  | Должно быть уникальным в клиенте <br><br> поддерживаются схемы urn:// <br><br> Подстановочные знаки не поддерживаются <br><br> Строки запросов и фрагменты поддерживаются <br><br> Максимальная длина 255 символов <br><br> Нет ограничения * на число identifierURIs  | Оно должно быть глобально уникальным. <br><br> поддерживаются схемы urn:// <br><br> Подстановочные знаки не поддерживаются <br><br> Строки запросов и фрагменты поддерживаются <br><br> Максимальная длина 255 символов <br><br> Нет ограничения * на число identifierURIs | Оно должно быть глобально уникальным. <br><br> схемы urn://не поддерживаются <br><br> Подстановочные знаки, фрагменты и строки запросов не поддерживаются <br><br> Максимальная длина 120 символов <br><br> Максимум 50 identifierURIs |
| Сертификаты ( `keyCredentials` ) | Симметричный ключ подписи | Симметричный ключ подписи | Шифрование и асимметричный ключ подписи | 
| Секреты клиента ( `passwordCredentials` ) | Без ограничений * | Без ограничений * | Если Ливесдк включен: не более 2 секретов клиента | 
| URI перенаправления ( `replyURLs` ) | Дополнительные сведения см. в статьях [ограничения URL-адреса и ограничения для перенаправления](reply-url.md) . | | | 
| Разрешения API ( `requiredResourceAccess` ) | Без ограничений * | Без ограничений * | Максимум 50 ресурсов на приложение и 30 разрешений на ресурс (например, Microsoft Graph). Общее ограничение в 200 на приложение (разрешения по ресурсам x). | 
| Области, определяемые этим API ( `oauth2Permissions` ) | Максимальная длина имени области (120 символов) <br><br> Нет ограничения * в количестве определенных областей | Максимальная длина имени области (120 символов) <br><br> Нет ограничения * в количестве определенных областей |  Максимальная длина имени области (40 символов) <br><br> Определено не более 100 областей | 
| Полномочные клиентские приложения ( `preAuthorizedApplications` ) | Без ограничений * | Без ограничений * | Общее максимальное число 500 <br><br> Определено не более 100 клиентских приложений <br><br> Максимум 30 областей, определенных для каждого клиента | 
| appRoles | Поддерживается <br> Без ограничений * | Поддерживается <br> Без ограничений * | Не поддерживается | 
| URL-адрес интерфейса выхода на переднюю канал | https://localhost разрешено <br><br> `http` схема не разрешена <br><br> Максимальная длина 255 символов | https://localhost разрешено <br><br> `http` схема не разрешена <br><br> Максимальная длина 255 символов | <br><br> https://localhost разрешено, http://localhost сбой для MSA <br><br> Максимальная длина 255 символов <br><br> `http` схема не разрешена <br><br> Подстановочные знаки не поддерживаются | 

* Существует глобальное ограничение в 1000 элементов во всех свойствах коллекции в объекте App.

## <a name="next-steps"></a>Дальнейшие действия

- Сведения о [регистрации приложений](app-objects-and-service-principals.md).
- Сведения о [манифесте приложения](reference-app-manifest.md).
