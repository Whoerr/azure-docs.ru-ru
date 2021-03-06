---
title: Проверка подлинности доступа к концентраторам событий Azure с помощью подписанных URL
description: В этой статье показано, как проверить подлинность доступа к ресурсам концентраторов событий с помощью подписанных URL.
ms.topic: conceptual
ms.date: 06/23/2020
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: ff141fc1bb681e2356a4471dfdc808d622fd76b5
ms.sourcegitcommit: 04297f0706b200af15d6d97bc6fc47788785950f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98986501"
---
# <a name="authenticate-access-to-event-hubs-resources-using-shared-access-signatures-sas"></a>Проверка подлинности доступа к ресурсам концентраторов событий с помощью подписанных URL-адресов (SAS)
Подписанный URL-адрес (SAS) обеспечивает детальный контроль над типом предоставляемого доступа для клиентов, имеющих подписанный общий доступ. Ниже приведены некоторые элементы управления, которые можно задать в SAS: 

- Интервал, по которому действительны SAS, включая время начала и окончания срока действия.
- Разрешения, предоставляемые с помощью SAS. Например, SAS для пространства имен концентраторов событий может предоставить разрешение на прослушивание, но не разрешение SEND.
- Только клиенты, предоставляющие действительные учетные данные, могут передать данные в концентратор событий.
- Клиент не может олицетворять другого клиента.
- Постороннему клиенту можно запретить отправку данных в концентратор событий.

В этой статье рассматривается проверка подлинности доступа к ресурсам концентраторов событий с помощью SAS. Дополнительные сведения о **предоставлении** доступа к ресурсам концентраторов событий с помощью SAS см. в [этой статье](authorize-access-shared-access-signature.md). 

> [!NOTE]
> Корпорация Майкрософт рекомендует использовать учетные данные Azure AD, если это возможно по соображениям безопасности, вместо использования подписанных URL-прав, что может быть более легко скомпрометировано. Хотя вы можете продолжать использовать подписанные URL-адресы (SAS), чтобы предоставить точный доступ к ресурсам концентраторов событий, Azure AD предлагает аналогичные возможности без необходимости управлять маркерами SAS или беспокоиться об отмене скомпрометированного SAS.
> 
> Дополнительные сведения об интеграции Azure AD в концентраторах событий Azure см. в статье [авторизация доступа к концентраторам событий с помощью Azure AD](authorize-access-azure-active-directory.md). 


## <a name="configuring-for-sas-authentication"></a>Настройка проверки подлинности SAS
Правило авторизации общего доступа EventHubs можно настроить в пространстве имен концентраторов событий или сущности (экземпляр концентратора событий или раздел Kafka в концентраторе событий). Настройка правила авторизации общего доступа для группы потребителей в настоящее время не поддерживается, но можно использовать правила, настроенные в пространстве имен или сущности, для безопасного доступа к группе потребителей. 

На следующем рисунке показано, как правила авторизации применяются к образцам сущностей. 

![Настройка правила авторизации](./media/authenticate-shared-access-signature/configure-sas-authorization-rule.png)

В этом примере в примере пространства имен концентраторов событий (Ексампленамеспаце) есть две сущности: EH1 и topic1. Правила авторизации определяются как на уровне сущности, так и на уровне пространства имен.  

Правила авторизации Манажеруленс, Сендруленс и Листенруленс применяются как к экземплярам концентратора событий EH1, так и к разделу T1. Правила авторизации Листенруле-EH и sendRule-EH применяются только к правилам авторизации экземпляра концентратора событий EH1 и Сендрулет применяются только к разделу topic1. 

При использовании правила авторизации Сендруленс клиентские приложения могут отправить сообщения как в EH1, так и в topic1. Когда используется правило авторизации Сендрулет, оно обеспечивает детализированный доступ только к topic1, поэтому клиентские приложения, использующие это правило для доступа, теперь не могут отправить EH1, а только в topic1.

## <a name="generate-a-shared-access-signature-token"></a>Создание маркера подписанного URL-адреса 
Каждый клиент, который имеет доступ к имени правила авторизации и одному из его ключей подписи, может создать маркер SAS. Маркер создается путем составления строки в следующем формате:

- `se`  — Срок действия маркера истекает мгновенно. Целочисленное отражение секунд, начиная с эпохи 00:00:00 UTC в 1 января 1970 (эпоха UNIX) по истечении срока действия маркера
- `skn` — Имя правила авторизации, которое является именем ключа SAS.
- `sr` — URI ресурса, к которому осуществляется доступ.
- `sig` Образец.

Строка сигнатуры — это хэш SHA-256, вычисленный на основе URI ресурса (область, как описано в предыдущем разделе), и строковое представление срока действия маркера, разделенное символом CRLF.

Вычисление хэша напоминает следующий псевдокод и возвращает 256-битное (32-байтное) значение хэша. 

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Маркер содержит нехэшируемые значения, что позволяет получателю повторно вычислить хэш с теми же параметрами и проверить, что издатель использует допустимый ключ подписи.

URI ресурса — это полный URI ресурса служебной шины, к которому запрашивается доступ. Например, http:// <namespace> . servicebus.Windows.NET/ <entityPath> или, `sb://<namespace>.servicebus.windows.net/<entityPath>;` `http://contoso.servicebus.windows.net/eventhubs/eh1` .

URI должен быть закодирован с помощью знака процента.

Правило авторизации общего доступа, используемое для подписи, необходимо настроить для сущности, указанной данным URI или одной из ее иерархических родительских сущностей. Например, в предыдущем примере это — `http://contoso.servicebus.windows.net/eventhubs/eh1` или `http://contoso.servicebus.windows.net`.

Маркер SAS действителен для всех ресурсов с префиксом, <resourceURI> используемым в строке подписи.

> [!NOTE]
> Маркер доступа создается для концентраторов событий с помощью политики общего доступа. Дополнительные сведения см. в разделе [Политика авторизации общего доступа](authorize-access-shared-access-signature.md#shared-access-authorization-policies).

### <a name="generating-a-signaturetoken-from-a-policy"></a>Создание подписи (маркера) из политики 
В следующем разделе показано, как создать маркер SAS с помощью политик подписанного URL-доступа.

#### <a name="nodejs"></a>Node.js

```javascript
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
```

#### <a name="java"></a>Java

```java
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```
#### <a name="php"></a>PHP

```php
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
    $targetUri = strtolower(rawurlencode(strtolower($uri))); 
    $expires = time();  
    $expiresInMins = 60; 
    $week = 60*60*24*7;
    $expires = $expires + $week; 
    $toSign = $targetUri . "\n" . $expires; 
    $signature = rawurlencode(base64_encode(hash_hmac('sha256',             
     $toSign, $sasKeyValue, TRUE))); 
    
    $token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
    return $token; 
}
```

#### <a name="c"></a>C#

```csharp
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="authenticating-event-hubs-publishers-with-sas"></a>Проверка подлинности издателей концентраторов событий с SAS 
Издатель событий определяет виртуальную конечную точку для концентратора событий. Издатель может использоваться только для отправки сообщений в концентратор событий и не для получения сообщений.

Как правило, концентратор событий использует один издатель на клиент. Все сообщения, отправленные любому из издателей концентратора событий, добавляются в очередь этого концентратора событий. Издатели обеспечивают детальный контроль доступа.

Каждому клиенту Центров событий назначается уникальный маркер, который передается в клиент. Маркеры создаются таким, что каждый уникальный маркер предоставляет доступ к другому уникальному издателю. Клиент, владеющий маркером, может отсылаться только одному издателю и не иметь другого издателя. Если несколько клиентов совместно используют один и тот же маркер, каждый из них совместно использует издателя.

Всем токенам назначаются ключи SAS. Как правило, все маркеры должны быть подписаны тем же ключом. Клиенты не знают о ключе, что не позащищает клиентов от маркеров производства. Клиенты работают с теми же маркерами до истечения срока их действия.

Например, чтобы определить правила авторизации до отправки или публикации в концентраторы событий, необходимо определить правило авторизации отправки. Это можно сделать на уровне пространства имен или предоставить более детализированную область для конкретной сущности (экземпляр концентратора событий или раздел). Клиент или приложение, область действия которого имеет такой детализированный доступ, называется издателем концентраторов событий. Для этого выполните следующие действия.

1. Создайте ключ SAS для сущности, которую необходимо опубликовать, чтобы назначить ей область **отправки** . Дополнительные сведения см. в разделе [политики авторизации общего доступа](authorize-access-shared-access-signature.md#shared-access-authorization-policies).
2. Создайте маркер SAS с временем окончания срока действия для конкретного издателя с помощью ключа, созданного в шаг 1.

    ```csharp
    var sasToken = SharedAccessSignatureTokenProvider.GetPublisherSharedAccessSignature(
                new Uri("Service-Bus-URI"),
                "eventub-name",
                "publisher-name",
                "sas-key-name",
                "sas-key",
                TimeSpan.FromMinutes(30));
    ```
3. Предоставьте маркер клиенту издателя, который может отправить только сущности и издателю, к которым маркер предоставляет доступ.

    После истечения срока действия маркера клиент теряет доступ к отправке и публикации в сущности. 


> [!NOTE]
> Хотя это не рекомендуется, для устройств можно использовать маркеры, которые предоставляют доступ к концентратору событий или пространству имен. Любое устройство, содержащее этот маркер, может передавать сообщения непосредственно в этот концентратор событий. Кроме того, устройство не может быть добавлен от отправки в этот концентратор событий.
> 
> Рекомендуется всегда предоставлять определенные и детализированные области.

> [!IMPORTANT]
> После создания маркеров каждому клиенту присваивается собственный уникальный маркер.
>
> Когда клиент отправляет данные в концентратор событий, он помечает свой запрос маркером. Во избежание перехвата и кражи маркера злоумышленником связь между клиентом и концентратором событий должна выполняться по зашифрованному каналу.
> 
> Если маркер украден злоумышленником, злоумышленник может действовать от имени клиента, маркер которого был украден. Добавление издатель, готовит его к использованию, пока не получит новый маркер, использующий другого издателя.


## <a name="authenticating-event-hubs-consumers-with-sas"></a>Проверка подлинности потребителей концентраторов событий с SAS 
Для проверки подлинности серверных приложений, которые используют данные, формируемые производителями концентраторов событий, для аутентификации на основе токенов концентраторов событий требуется, чтобы клиенты имели права **управления** или **прослушиваемые** привилегии, назначенные его пространству имен концентраторов событий или экземпляру концентратора событий или разделу. Данные потребляются из концентраторов событий с помощью групп потребителей. Хотя политика SAS обеспечивает детализированную область, эта область определяется только на уровне сущности, а не на уровне потребителя. Это означает, что привилегии, определенные на уровне пространства имен или на уровне экземпляра или раздела концентратора событий, будут применяться к группам потребителей этой сущности.

## <a name="next-steps"></a>Дальнейшие действия
См. следующие статьи:

- [Авторизация с помощью SAS](authenticate-shared-access-signature.md)
- [Авторизация с помощью управления доступом на основе ролей Azure (Azure RBAC)](authenticate-shared-access-signature.md)
- [Дополнительные сведения о концентраторах событий](event-hubs-about.md)

См. следующие статьи:

- [Проверка подлинности запросов к концентраторам событий Azure из приложения с помощью Azure Active Directory](authenticate-application.md)
- [Проверка подлинности управляемого удостоверения с Azure Active Directory для доступа к ресурсам концентраторов событий](authenticate-managed-identity.md)
- [Авторизация доступа к ресурсам концентраторов событий с помощью Azure Active Directory](authorize-access-azure-active-directory.md)
- [Авторизация доступа к ресурсам концентраторов событий с помощью подписанных URL](authorize-access-shared-access-signature.md)
