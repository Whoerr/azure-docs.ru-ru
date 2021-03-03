---
ms.openlocfilehash: 2c816f005dea452c3ffa2889aa7d2742a5762759
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657675"
---
## <a name="add-managed-identity-to-your-communication-services-solution-net"></a>Добавление управляемого удостоверения в решение служб связи (.NET)

### <a name="install-the-client-library-packages"></a>Установка пакетов клиентской библиотеки

```console
dotnet add package Azure.Communication.Identity
dotnet add package Azure.Communication.Sms
dotnet add package Azure.Identity
```

### <a name="use-the-client-library-packages"></a>Использование пакетов клиентских библиотек

Добавьте следующие `using` директивы в код для использования Azure Identity и клиентских библиотек службы хранилища Azure.

```csharp
using Azure.Identity;
using Azure.Communication.Identity;
using Azure.Communication.Sms;
```

В приведенных ниже примерах используется [дефаултазурекредентиал](/dotnet/api/azure.identity.defaultazurecredential). Эти учетные данные подходят для сред рабочей среды и разработки.

`AZURE_CLIENT_SECRET`, `AZURE_CLIENT_ID` а `AZURE_TENANT_ID` переменные среды необходимы для создания `DefaultAzureCredential` объекта. Сведения о создании зарегистрированного приложения в среде разработки и настройке переменных среды см. в разделе [авторизация доступа с помощью управляемого удостоверения](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Создание удостоверения и выдача маркера с управляемым удостоверением

В следующем примере кода показано, как создать объект клиента службы с токенами Azure Active Directory.

Затем используйте клиент, чтобы выдать маркер для нового пользователя:

```csharp
     public async Task<Response<CommunicationUserToken>> CreateIdentityAndGetTokenAsync(Uri resourceEdnpoint)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          String resourceEndpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          var client = new CommunicationIdentityClient(resourceEndpoint, credential);
          var identityResponse = await client.CreateUserAsync();

          var tokenResponse = await client.GetTokenAsync(identity, scopes: new [] { CommunicationTokenScope.VoIP });

          return tokenResponse;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Отправка SMS с управляемым удостоверением

В следующем примере кода показано, как создать объект клиента службы SMS с управляемым удостоверением, а затем использовать клиент для отправки SMS-сообщения:

```csharp
     public async Task SendSms(Uri resourceEndpoint, PhoneNumber from, PhoneNumber to, string message)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          String resourceEndpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          SmsClient smsClient = new SmsClient(resourceEndpoint, credential);
          smsClient.Send(
               from: from,
               to: to,
               message: message,
               new SendSmsOptions { EnableDeliveryReport = true } // optional
          );
     }
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Сведения о проверке подлинности](../concepts/authentication.md)

Возможно, вы также захотите выполнить такие задачи:

- [Дополнительные сведения об управлении доступом на основе ролей в Azure](../../../../articles/role-based-access-control/index.yml)
- [Дополнительные сведения о библиотеке удостоверений Azure для .NET](/dotnet/api/overview/azure/identity-readme)
- [Создание маркеров доступа пользователей](../../quickstarts/access-tokens.md)
- [Отправка SMS](../telephony-sms/send.md)
- [Дополнительные сведения об SMS](../../concepts/telephony-sms/concepts.md)
