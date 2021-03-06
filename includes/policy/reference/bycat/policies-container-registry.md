---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/09/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 21a501d65966d5a99cc368b71094072825a9fe3b
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100100372"
---
|Имя<br /><sub>(портал Azure)</sub> |Описание |Действие |Версия<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Реестры контейнеров должны шифроваться с помощью ключа, управляемого клиентом](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Используйте ключи, управляемые клиентом, для управления шифрованием неактивного содержимого реестров. По умолчанию неактивные данные шифруются с помощью ключей, управляемых службой, но для соблюдения нормативных требований обычно требуются управляемые клиентом ключи. Управляемые клиентом ключи позволяют шифровать данные с помощью ключа Azure Key Vault, создателем и владельцем которого являетесь вы. Вы полностью контролируете жизненный цикл ключа, включая его смену и управление им. Подробная информация собрана на странице [https://aka.ms/acr/CMK](https://aka.ms/acr/CMK). |Audit, Deny, Disabled |[1.1.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json) |
|[В реестрах контейнеров не должен быть разрешен неограниченный сетевой доступ](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |По умолчанию реестры контейнеров Azure принимают подключения через Интернет от узлов в любой сети. Чтобы защитить свои реестры от потенциальных угроз, разрешите доступ только с определенных общедоступных IP-адресов или диапазонов адресов. Если для реестра нет правила IP-адресов или брандмауэра либо настроенной виртуальной сети, он отобразится как неработоспособный ресурс. См. дополнительные сведения о сетевых правилах Реестра контейнеров: [https://aka.ms/acr/portal/public-network](https://aka.ms/acr/portal/public-network) и [https://aka.ms/acr/vnet](https://aka.ms/acr/vnet). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json) |
|[Реестры контейнеров должны использовать приватный канал](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe8eef0a8-67cf-4eb4-9386-14b0e78733d4) |Приватный канал Azure позволяет подключить виртуальную сеть к службам Azure без общедоступного IP-адреса в исходном или целевом расположении. Платформа приватного канала обрабатывает подключение между потребителем и службами через магистральную сеть Azure. Сопоставление частных конечных точек с реестрами контейнеров, а не всей службой также обеспечивает защиту от утечки данных. См. дополнительные сведения: [https://aka.ms/acr/private-link](https://aka.ms/acr/private-link). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_PrivateEndpointEnabled_Audit.json) |
