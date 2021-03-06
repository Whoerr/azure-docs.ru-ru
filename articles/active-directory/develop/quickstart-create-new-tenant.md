---
title: Краткое руководство. Создание клиента Azure Active Directory
titleSuffix: Microsoft identity platform
description: Из этого краткого руководства вы узнаете, как создать клиент Azure Active Directory для использования в разрабатываемых приложениях, применяющих платформу удостоверений Майкрософт для проверки подлинности и авторизации.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: quickstart
ms.date: 02/15/2021
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40, fasttrack-edit
ms.openlocfilehash: cb4995c0c71d0bcebb3cdd8ec471a92b7820cb9d
ms.sourcegitcommit: b513b0becf878eb9a1554c26da53aa48d580bb22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100535167"
---
# <a name="quickstart-set-up-a-tenant"></a>Краткое руководство. Настройка клиента

Чтобы создавать приложения, в которых для управления удостоверениями и доступом используется платформа удостоверений Майкрософт, необходим доступ к *клиенту* Azure Active Directory (Azure AD). Именно в клиенте Azure AD вы регистрируете приложения и управляете ими, настраиваете их доступ к данным в Microsoft 365 и других веб-API и включаете такие функции, как условный доступ.

Клиент представляет организацию. Это выделенный экземпляр Azure AD, который организация или разработчик приложений получает при установлении отношений с Майкрософт. Эти отношения могут начаться, например, с регистрации в Azure, Microsoft Intune или Microsoft 365.

Каждый клиент Azure AD является отдельным и не связан с другими клиентами Azure AD. Он имеет собственное представление рабочих и учебных учетных записей, удостоверений потребителей (если это клиент Azure AD B2C) и регистрации приложений. Регистрация приложения в клиенте позволяет осуществлять проверку подлинности из учетных записей только в вашем клиенте или во всех клиентах.

## <a name="prerequisites"></a>Предварительные требования

Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) бесплатно.

## <a name="determining-the-environment-type"></a>Определение типа среды

Вы можете создать среду одного из двух типов. Выбор среды зависит исключительно от типов пользователей, для которых приложение будет выполнять проверку подлинности. 

В этом кратком руководстве рассматриваются два сценария в зависимости от типа приложения, которое вы хотите создать:

* рабочие и учебные учетные записи (Azure AD) или учетные записи Майкрософт (например, outlook.com и live.com);
* учетные записи социальных сетей и локальные учетные записи (Azure AD B2C).

## <a name="work-and-school-accounts-or-personal-microsoft-accounts"></a>Рабочие и учебные учетные записи или личные учетные записи Майкрософт

Чтобы создать среду для рабочих или учебных учетных записей либо личных учетных записей Майкрософт, вы можете использовать существующий клиент Azure AD или создать новый.
### <a name="use-an-existing-azure-ad-tenant"></a>Использование существующего клиента Azure AD

У многих разработчиков уже есть клиенты благодаря службам или подпискам, которые связаны с клиентами Azure AD (например, подписки Microsoft 365 или Azure).

Чтобы проверить клиент, сделайте следующее:

1. Войдите на <a href="https://portal.azure.com/" target="_blank">портал Azure</a>. Укажите учетную запись, которая будет использоваться для управления приложением.
1. Посмотрите в правый верхний угол. Если у вас есть клиент, вход будет выполнен автоматически. Имя клиента отобразится непосредственно под именем вашей учетной записи.
   * Если вы наведете указатель мыши на имя учетной записи, вы увидите свое имя, адрес электронной почты, идентификатор каталога или клиента (GUID) и домен.
   * Если ваша учетная запись связана с несколькими клиентами, выберите имя учетной записи, чтобы открыть меню, где можно переключаться между клиентами. У каждого клиента есть свой идентификатор.

> [!TIP]
> Чтобы найти идентификатор клиента, вы можете сделать следующее:
> * Наведите указатель мыши на имя вашей учетной записи, чтобы получить данные об идентификаторе каталога или клиента.
> * На портале Azure найдите и выберите **Azure Active Directory** > **Свойства** > **Идентификатор клиента**.

Если у вас нет клиента, связанного с вашей учетной записью, под именем вашей учетной записи отобразится идентификатор GUID. Вы не сможете выполнять такие действия, как регистрация приложений, пока не создадите клиент Azure AD.

### <a name="create-a-new-azure-ad-tenant"></a>Создание нового клиента Azure AD

Если у вас еще нет клиента Azure AD или вы хотите создать клиент для разработки, используйте [процедуру создания клиента в Azure AD](../fundamentals/active-directory-access-create-new-tenant.md). Или используйте [интерфейс создания каталога](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) на портале Azure. 

Чтобы создать клиент, нужно указать следующие сведения:

- **Название организации**
- **Исходный домен** — он будет частью *.onmicrosoft.com. Вы сможете настроить домен позже.
- **Страна или регион**

> [!NOTE]
> Имя вашего клиента должно содержать буквы и цифры. Использовать специальные символы запрещено. Длина имени не должна превышать 256 символов.

## <a name="social-and-local-accounts"></a>Учетные записи социальных сетей и локальные учетные записи

Чтобы приступить к созданию приложений, в вход которые выполняется с помощью учетных записей социальных сетей и локальных учетных записей, необходимо создать клиент Azure AD B2C. Сначала ознакомьтесь с процедурой [создания клиента Azure AD B2C](../../active-directory-b2c/tutorial-create-tenant.md).

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Зарегистрируйте приложение](quickstart-register-app.md), чтобы выполнить интеграцию с платформой удостоверений Майкрософт.
