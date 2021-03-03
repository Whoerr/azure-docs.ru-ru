---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: 61576de4a57d55ea9d1ea209c52df556f0069617
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750224"
---
| Свойство | Описание |
|:--- |:---|
|**identity/type** | Тип аутентификации, используемый для выполнения запроса. Например: `OAuth` ,, `Kerberos` `SAS Key` , `Account Key` или `Anonymous` |
|**identity/tokenHash**|Это поле зарезервировано для внутреннего использования. |
|**authorization/action** | Действие, назначенное запросу. |
|**authorization/roleAssignmentId** | Идентификатор назначения ролей. Например: `4e2521b7-13be-4363-aeda-111111111111`.|
|**authorization/roleDefinitionId** | Идентификатор определения роли. Например: `ba92f5b4-2d11-453d-a403-111111111111"`.|
|**principals/id** | Идентификатор субъекта безопасности. Например: `a4711f3a-254f-4cfb-8a2d-111111111111`.|
|**principals/type** | Тип субъекта безопасности. Например: `ServicePrincipal`. |
|**requester/appID** | Идентификатор приложения Open Authorization (OAuth), который используется в качестве инициатора запроса. <br> Например: `d3f7d5fe-e64a-4e4e-871d-333333333333`.|
|**requester/audience** | Аудитория OAuth запроса. Например: `https://storage.azure.com`. |
|**requester/objectId** | Идентификатор объекта OAuth инициатора запроса. В случае аутентификации Kerberos предоставляется идентификатор объекта пользователя, который ее прошел. Например: `0e0bf547-55e5-465c-91b7-2873712b249c`. |
|**requester/tenantId** | Идентификатор клиента OAuth удостоверения. Например: `72f988bf-86f1-41af-91ab-222222222222`.|
|**requester/tokenIssuer** | Издатель токенов OAuth. Например: `https://sts.windows.net/72f988bf-86f1-41af-91ab-222222222222/`.|
|**requester/upn** | Имя участника-пользователя (UPN) инициатора запроса. Например: `someone@contoso.com`. |
|**requester/userName** | Это поле зарезервировано для внутреннего использования.|
