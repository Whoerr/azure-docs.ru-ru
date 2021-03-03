---
ms.openlocfilehash: 0fd97fe0eebb23130513409698b75d2ee7b98a6f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657667"
---
## <a name="add-managed-identity-to-your-communication-services-solution-js"></a>Добавление управляемого удостоверения в решение служб связи (JS)

### <a name="install-the-client-library-packages"></a>Установка пакетов клиентской библиотеки

```console
npm install @azure/communication-identity
npm install @azure/communication-common
npm install @azure/communication-sms
npm install @azure/identity
```

### <a name="use-the-client-library-packages"></a>Использование пакетов клиентских библиотек

Добавьте следующие `import` директивы в код для использования Azure Identity и клиентских библиотек службы хранилища Azure.

```typescript
import { DefaultAzureCredential } from "@azure/identity";
import { CommunicationIdentityClient } from "@azure/communication-identity";
import { SmsClient } from "@azure/communication-sms";
```

В приведенных ниже примерах используется [дефаултазурекредентиал](/javascript/api/azure.identity.defaultazurecredential). Эти учетные данные подходят для сред рабочей среды и разработки.

Сведения о регистрации приложения в среде разработки и настройке переменных среды см. в разделе [авторизация доступа с помощью управляемого удостоверения](../managed-identity-from-cli.md) .  

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Создание удостоверения и выдача маркера с управляемым удостоверением

В следующем примере кода показано, как создать объект клиента службы с управляемым удостоверением, а затем использовать клиент для выдаче маркера для нового пользователя:

```JavaScript
export async function createIdentityAndIssueToken(resourceEndpoint: string): Promise<CommunicationUserToken> {
     let credential = new DefaultAzureCredential();
     const client = new CommunicationIdentityClient(resourceEndpoint, credential);
     return await client.createUserWithToken(["chat"]);
}
```

### <a name="send-an-sms-with-managed-identity"></a>Отправка SMS с управляемым удостоверением

В следующем примере кода показано, как создать объект клиента службы с управляемым удостоверением, а затем использовать клиент для отправки SMS-сообщения:

```JavaScript
export async function sendSms(resourceEndpoint: string, fromNumber: any, toNumber: any, message: string) {
     let credential = new DefaultAzureCredential();
     const smsClient = new SmsClient(resourceEndpoint, credential);
     const sendRequest: SendRequest = { 
          from: fromNumber, 
          to: [toNumber], 
          message: message 
     };

     const response = await smsClient.send(
          sendRequest, 
          {} //Optional SendOptions
          );
}
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Сведения о проверке подлинности](../concepts/authentication.md)

Возможно, вы также захотите выполнить такие задачи:

- [Дополнительные сведения об управлении доступом на основе ролей в Azure](../../../../articles/role-based-access-control/index.yml)
- [Дополнительные сведения о библиотеке удостоверений Azure для JS](/javascript/api/overview/azure/identity-readme)
- [Создание маркеров доступа пользователей](../../quickstarts/access-tokens.md)
- [Отправка SMS](../../quickstarts/telephony-sms/send.md)
- [Дополнительные сведения об SMS](../../concepts/telephony-sms/concepts.md)

