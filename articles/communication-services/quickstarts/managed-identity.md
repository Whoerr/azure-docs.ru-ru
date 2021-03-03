---
title: Использование управляемых удостоверений в службах связи
titleSuffix: An Azure Communication Services quickstart
description: Управляемые удостоверения позволяют авторизовать доступ к службам связи Azure из приложений, работающих на виртуальных машинах Azure, в приложениях функций и других ресурсах.
services: azure-communication-services
author: peiliu
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 2/24/2021
ms.author: peiliu
ms.reviewer: mikben
ms.openlocfilehash: 0d25e5dc97c700daf6655ecd270bfda469a9d353
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657660"
---
# <a name="use-managed-identities"></a>Использование управляемых удостоверений
Приступая к работе со службами связи Azure с помощью управляемых удостоверений. Удостоверения служб связи и клиентские библиотеки SMS поддерживают проверку подлинности Azure Active Directory (Azure AD) с помощью [управляемых удостоверений для ресурсов Azure](../../active-directory/managed-identities-azure-resources/overview.md).

В этом кратком руководстве показано, как авторизовать доступ к клиентским библиотекам удостоверений и SMS из среды Azure, которая поддерживает управляемые удостоверения. В нем также описывается тестирование кода в среде разработки.

## <a name="prerequisites"></a>Предварительные требования

 - Учетная запись Azure с активной подпиской. [Создать учетную запись бесплатно](https://azure.microsoft.com/free)
 - Активный ресурс Служб коммуникации и строка подключения. [Создайте ресурс Служб коммуникации.](./create-communication-resource.md?pivots=platform-azp&tabs=windows)

## <a name="setting-up"></a>Настройка

### <a name="enable-managed-identities-on-a-virtual-machine-or-app-service"></a>Включение управляемых удостоверений на виртуальной машине или в службе приложений

Управляемые удостоверения должны быть включены в ресурсах Azure, для которых выполняется авторизация. Сведения о включении управляемых удостоверений для ресурсов Azure см. в одной из следующих статей.

- [Портал Azure](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Шаблон Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Клиентские библиотеки Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)
- [Службы приложений](../../app-service/overview-managed-identity.md)

#### <a name="assign-azure-roles-with-the-azure-portal"></a>Назначение ролей Azure с помощью портал Azure

1. Перейдите на портал Azure.
1. Перейдите к ресурсу службы связи Azure.
1. Перейдите к меню управления доступом (IAM) — > + Add-> добавить назначение ролей.
1. Выберите роль участник (это единственная поддерживаемая роль).
1. Выберите "управляемое удостоверение, назначенное пользователем" (или "управляемое системой удостоверение"), а затем выберите нужное удостоверение. Сохраните выбранные элементы.

![Управляемая роль идентификации](media/managed-identity-assign-role.png)

#### <a name="assign-azure-roles-with-powershell"></a>Назначение ролей Azure с помощью PowerShell

Сведения о назначении ролей и разрешений с помощью PowerShell см. в статье [Добавление и удаление назначений ролей Azure с помощью Azure PowerShell](../../../articles/role-based-access-control/role-assignments-powershell.md)

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/managed-identity-net.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/managed-identity-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/managed-identity-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/managed-identity-python.md)]
::: zone-end