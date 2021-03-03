---
ms.openlocfilehash: 9da0cbc3777359994fac3778dcc126c844754861
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657668"
---
## <a name="add-managed-identity-to-your-communication-services-solution"></a>Добавление управляемого удостоверения в решение служб связи

### <a name="install-the-client-library-packages"></a>Установка пакетов клиентской библиотеки

```console
pip install azure-identity
pip install azure-communication-identity
pip install azure-communication-sms
```

### <a name="use-the-client-library-packages"></a>Использование пакетов клиентских библиотек

Добавьте следующее `import` в код, чтобы использовать удостоверение Azure.

```python
from azure.identity import DefaultAzureCredential
```

В приведенных ниже примерах используется [дефаултазурекредентиал](/python/api/azure.identity.defaultazurecredential). Эти учетные данные подходят для сред рабочей среды и разработки.

Сведения о регистрации приложения в среде разработки и настройке переменных среды см. в разделе [авторизация доступа с помощью управляемого удостоверения](../managed-identity-from-cli.md) .

### <a name="create-an-identity-and-issue-a-token"></a>Создание удостоверения и выдача маркера

В следующем примере кода показано, как создать объект клиента службы с управляемым удостоверением, а затем использовать клиент для выдаче маркера для нового пользователя:

```python
import azure.communication.identity 

def create_identity_and_get_token(resource_endpoint):
     credential = DefaultAzureCredential()
     client = CommunicationIdentityClient(endpoint, credential)

     user = client.create_user()
     token_response = client.get_token(user, scopes=["voip"])
     
     return token_response
```

### <a name="send-an-sms-with-azure-managed-identity"></a>Отправка SMS с управляемым удостоверением Azure

В следующем примере кода показано, как создать объект клиента службы с управляемым удостоверением Azure, а затем использовать клиент для отправки SMS-сообщения:

```python
from azure.communication.sms import (
    PhoneNumberIdentifier,
    SendSmsOptions,
    SmsClient
)

def send_sms(resource_endpoint, from_phone_number, to_phone_number, message_content):
     credential = DefaultAzureCredential()
     sms_client = SmsClient(resource_endpoint, credential)

     sms_client.send(
          from_phone_number=PhoneNumberIdentitifier(from_phone_number),
          to_phone_numbers=[PhoneNumberIdentifier(to_phone_number)],
          message=message_content,
          send_sms_options=SendSmsOptions(enable_delivery_report=True))  # optional property
     )
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Сведения о проверке подлинности](../../concepts/authentication.md)

Возможно, вы также захотите выполнить такие задачи:

- [Дополнительные сведения об управлении доступом на основе ролей в Azure](../../../../articles/role-based-access-control/index.yml)
- [Дополнительные сведения о библиотеке удостоверений Azure для Python](/net/api/overview/azure/identity-readme)
- [Создание маркеров доступа пользователей](../../quickstarts/access-tokens.md)
- [Отправка SMS](../../quickstarts/telephony-sms/send.md)
- [Дополнительные сведения об SMS](../../concepts/telephony-sms/concepts.md)