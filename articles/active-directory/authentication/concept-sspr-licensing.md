---
title: 'Azure Active Directory: лицензирование самостоятельного сброса пароля'
description: Сведения о разнице Azure Active Directory требования к лицензированию самостоятельного сброса пароля
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f8f270f740951b95b0d4c00cbcf9b08e20a8e0c
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2020
ms.locfileid: "96743730"
---
# <a name="licensing-requirements-for-azure-active-directory-self-service-password-reset"></a>Требования к лицензированию для самостоятельного сброса пароля Azure Active Directory

Чтобы снизить количество обращений в службу поддержки и потери производительности, когда пользователь не может войти на устройство или в приложение, учетные записи пользователей в Azure Active Directory (Azure AD) можно включить для самостоятельного сброса пароля (SSPR). Функции, составляющие SSPR, включают изменение пароля, сброс, разблокировку и обратную запись в локальный каталог. Базовые функции SSPR доступны в Microsoft 365 бизнес Standard или выше и все Azure AD Premium SKU без каких бы то ни было затрат.

В этой статье описаны различные способы лицензирования и использования самостоятельного сброса пароля. Конкретные сведения о ценах и выставлении счетов см. на [странице цен на Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Сравнение выпусков и функций

SSPR лицензируется для каждого пользователя. Чтобы обеспечить соответствие требованиям, организациям необходимо назначить соответствующую лицензию своим пользователям.

В следующей таблице описаны различные сценарии SSPR для изменения, сброса или обратной записи паролей, а также номера SKU, которые предоставляют эту функцию.

| Компонент | Azure AD уровня "Бесплатный" | Microsoft 365 бизнес стандартный; | Microsoft 365 бизнес премиум. | Azure AD Premium (P1 или P2) |
| --- |:---:|:---:|:---:|:---:|
| **Изменение пароля пользователя только в облаке**<br />Когда пользователь в Azure AD знает свой пароль и хочет изменить его на что-то новое. | ● | ● | ● | ● |
| **Сброс пароля пользователя только в облаке**<br />Когда пользователь в Azure AD забыл свой пароль и его необходимо сбросить. | | ● | ● | ● |
| **Изменение или сброс пароля гибридного пользователя с помощью локальной обратной записи**<br />Когда пользователь в Azure AD, синхронизированный из локального каталога, использует Azure AD Connect хочет изменить или сбросить пароль, а также записать новый пароль обратно в локальную среду. | | | ● | ● |

> [!WARNING]
> Автономные Microsoft 365 планы лицензирования "базовый" и "Стандартный" не поддерживают SSPR с обратной записью в локальной среде. Для функции обратной записи в локальной среде требуется Azure AD Premium P1, Premium P2 или Microsoft 365 бизнес Premium.

Дополнительные сведения о лицензировании, включая затраты, см. на следующих страницах:

* [Цены на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Функции и возможности Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)
* [Microsoft 365 для бизнеса](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="enable-group-or-user-based-licensing"></a>Включение группового и пользовательского лицензирования

Azure AD поддерживает лицензирование на основе групп. Это позволяет администраторам назначать лицензии сразу группе пользователей, вместо назначения их по одной. Дополнительные сведения см. в разделе [Назначение лицензий группе пользователей в Azure Active Directory](../enterprise-users/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Некоторые службы Майкрософт доступны не во всех расположениях. Прежде чем можно будет назначить лицензию пользователю, администратор должен указать для пользователя свойство " **место использования** ". Назначение лицензий можно выполнить в разделе " **User**  >  **Profile**  >  **Параметры** профиля пользователя" в портал Azure. *Если лицензии назначаются группам, все пользователи, для которых не указано расположение, наследуют расположение каталога.*

## <a name="next-steps"></a>Дальнейшие действия

Чтобы приступить к работе с SSPR, выполните инструкции из следующего руководства.

> [!div class="nextstepaction"]
> [Учебник. Включение самостоятельного сброса пароля (SSPR)](tutorial-enable-sspr.md)