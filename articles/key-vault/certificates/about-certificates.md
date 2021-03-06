---
title: Сведения о сертификатах Azure Key Vault — Azure Key Vault
description: Общие сведения об интерфейсе REST и сертификатах Azure Key Vault.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: overview
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 66f077028b9f9f7a7644a318d4447eeaaab19e98
ms.sourcegitcommit: 03c0a713f602e671b278f5a6101c54c75d87658d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2020
ms.locfileid: "94919936"
---
# <a name="about-azure-key-vault-certificates"></a>Сведения о сертификатах Azure Key Vault

Поддержка сертификатов хранилища ключей не только позволяет управлять сертификатами x509, но и предоставляет следующие возможности.  

-   Владелец сертификата может создать сертификат при создании хранилища ключей или импорте существующего сертификата. Это применимо как для самозаверяющих сертификатов, так и для сертификатов, созданных центром сертификации.
-   Владелец сертификата хранилища ключей может безопасно хранить сертификаты X509 и управлять ими без необходимости использовать закрытый ключ.  
-   Владелец сертификата может создать политику, которая определяет для хранилища ключей параметры управления жизненным циклом сертификата.  
-   Владелец сертификата может предоставить контактные данные для получения уведомлений о таких событиях жизненного цикла, как истечение срока действия и обновление сертификата.  
-   Автоматическое обновление с возможностью выбора издателей сертификатов хранилища ключей — партнерских поставщиков сертификатов X509 и центров сертификации.

>[!Note]
>Также разрешено использовать сертификаты, которые предоставляют поставщики и центры, не являющиеся партнерами. Но в таком случае функция автоматического обновления недоступна.

## <a name="composition-of-a-certificate"></a>Создание сертификатов

Когда создается сертификат Key Vault, адресуемые ключ и секрет также создаются с тем же именем. Ключ Key Vault разрешает операции с ключами, а секрет Key Vault позволяет извлечь значение сертификата в виде секрета. Сертификат Key Vault также содержит открытые метаданные сертификата x509.  

Идентификатор и версия сертификатов аналогичны этим объектам у ключей и секретов. Определенная версия адресуемого ключа и секрета, созданная с помощью версии сертификата Key Vault, доступна в ответе сертификата Key Vault.
 
![Сертификаты — составные объекты](../media/azure-key-vault.png)

## <a name="exportable-or-non-exportable-key"></a>Доступный или недоступный для экспорта ключ

Созданный сертификат Key Vault можно извлечь из адресуемого секрета с помощью закрытого ключа в формате PFX или PEM. Политика, используемая для создания сертификата, указывает, что ключ доступен для экспорта. Если политика указывает, что ключ недоступен для экспорта, тогда закрытый ключ не будет частью значения при извлечении в качестве секрета.  

Адресуемый ключ становится более релевантным с недоступными для экспорта сертификатами Key Vault. Операции с адресуемыми ключами Key Vault сопоставляются из поля *keyusage* политики сертификата Key Vault, используемой для его создания.  

Типы пар ключей, поддерживаемые для сертификатов

 - Поддерживаемые типы ключей: RSA, RSA-HSM, EC, EC-HSM и т. д. (см. [здесь](/rest/api/keyvault/createcertificate/createcertificate#jsonwebkeytype)) Для экспорта доступны только RSA и EC. Ключи HSM нельзя экспортировать.

|Тип ключа|Сведения|Безопасность|
|--|--|--|
|**RSA**| Ключ RSA, защищенный программным обеспечением.|FIPS 140-2 уровня 1|
|**RSA-HSM**| Ключ RSA, защищенный модулем HSM (только SKU "Премиум")|HSM FIPS 140-2 уровня 2|
|**EC**| Ключ на основе эллиптической кривой, защищенный программным обеспечением|FIPS 140-2 уровня 1|
|**EC-HSM**| Ключ с эллиптической кривой, защищенный модулем HSM (только SKU "Премиум")|HSM FIPS 140-2 уровня 2|
|||

## <a name="certificate-attributes-and-tags"></a>Атрибуты и теги сертификата

Помимо метаданных сертификата, адресуемых ключа и секрета, сертификат Key Vault также содержит атрибуты и теги.  

### <a name="attributes"></a>Атрибуты

Атрибуты сертификатов зеркально отражают атрибуты адресуемых ключей и секретов, созданные вместе с сертификатом Key Vault.  

Сертификат Key Vault имеет следующие атрибуты:  

-   *enabled*. Необязательный атрибут с логическим значением, по умолчанию — **true**. Этот атрибут можно задать для указания возможности извлекать данные сертификата в виде секрета или использовать их в качестве ключа. Он используется совместно с *nbf* и *exp* при возникновении операции между *nbf* и *exp*. Она будет разрешена, только если атрибут enabled имеет значение true. Операции вне окна *nbf* и *exp* автоматически запрещаются.  

Есть дополнительные атрибуты только для чтения, которые включены в ответ:

-   *Создано*. Указывает, когда была создана эта версия сертификата. Значение в формате IntDate.  
-   *Обновлено*. Указывает, когда была обновлена эта версия сертификата. Значение в формате IntDate.  
-   *exp*. Содержит значение даты истечения срока действия сертификата x509. Значение в формате IntDate.  
-   *nbf*. Содержит значение даты сертификата x509. Значение в формате IntDate.  

> [!Note] 
> Если срок действия сертификата в Key Vault истекает, адресуемый ключ и секрет становятся неработоспособными.  

### <a name="tags"></a>Теги

 Клиент указал словарь пар значений и ключей, аналогичный тегам в ключах и секретах.  

 > [!Note]
> Теги могут быть прочитаны вызывающим объектом, если он имеет разрешения *list* или *get* на такие типы объектов: ключи, секреты или сертификаты.

## <a name="certificate-policy"></a>Политика сертификата

Политика сертификата содержит сведения о способах создания сертификатов Key Vault и об управлении их жизненным циклом. При импорте сертификата с закрытым ключом в хранилище ключей создается политика по умолчанию путем чтения сертификата x509.  

При создании сертификата Key Vault с нуля необходимо предоставить политику для Key Vault. Политика указывает, как создать эту или следующую версию сертификата Key Vault. После создания политики при последующих операциях создания не требуется создавать следующие версии сертификатов. Есть только один экземпляр политики для всех версий сертификата Key Vault.  

В целом политика сертификата содержит приведенные ниже элементы (их определения можно найти [здесь](/powershell/module/az.keyvault/set-azkeyvaultcertificatepolicy?view=azps-4.4.0)).  

-   Свойства сертификата X509 Содержит имя субъекта, альтернативные имена субъекта и другие свойства, используемые для создания запроса на сертификат X509.  
-   Свойства ключей. Это поля типа ключа, длины ключа, доступности для экспорта и повторного использования ключа. Эти поля инструктируют хранилище ключей о том, как создать ключ. 
     - Поддерживаемые типы ключей: RSA, RSA-HSM, EC, EC-HSM, oct (перечислены [здесь](/rest/api/keyvault/createcertificate/createcertificate#jsonwebkeytype)). 
-   Свойства секрета. Это такие свойства, как тип содержимого адресуемого секрета для создания значения секрета, а также для извлечения сертификата в качестве секрета.  
-   Действия на протяжении времени существования. Это соответствующие действия для сертификата Key Vault. Каждое такое действие содержит:  

     - Триггер. Указывается с помощью дней до истечения срока действия или процента времени существования.  

     - Действие. Указывается тип действия: *emailContacts* или *autoRenew*.  

-   Издатель. Параметры издателя сертификата, которые используются для выдачи сертификата x509.  
-   Атрибуты политики. Атрибуты, связанные с политикой.  

### <a name="x509-to-key-vault-usage-mapping"></a>Сопоставление использования параметров X509 и операций Key Vault

В следующей таблице представлено сопоставление политики использования ключа x509 с эффективными операциями с ключами, созданными как часть создания сертификата Key Vault.

|**Параметры использования ключей X509**|**Операции с ключами Key Vault**|**Поведение по умолчанию**|
|----------|--------|--------|
|DataEncipherment|шифрование, расшифровка| Недоступно |
|DecipherOnly|расшифровка| Недоступно  |
|DigitalSignature|подпись, проверка| Key Vault по умолчанию не имеет определения использования при создании сертификата | 
|EncipherOnly|encrypt| Недоступно |
|KeyCertSign|подпись, проверка|Недоступно|
|KeyEncipherment|упаковка и разворачивание ключа| Key Vault по умолчанию не имеет определения использования при создании сертификата | 
|NonRepudiation|подпись, проверка| Недоступно |
|crlsign|подпись, проверка| Недоступно |

## <a name="certificate-issuer"></a>Издатель сертификата

Объект сертификата Key Vault содержит конфигурацию, используемую для обмена данными с выбранным поставщиком издателей сертификатов для заказа сертификатов x509.  

-   Key Vault сотрудничает со следующими поставщиками издателей для TLS/SSL-сертификатов:

|**Имя поставщика**|**Расположения**|
|----------|--------|
|DigiCert;|Поддерживается во всех расположениях службы хранилища ключей в общедоступном облаке и в Azure для государственных организаций|
|GlobalSign;|Поддерживается во всех расположениях службы хранилища ключей в общедоступном облаке и в Azure для государственных организаций|

Перед созданием издателя сертификата в Key Vault необходимо выполнить приведенные ниже обязательные шаги 1 и 2.  

1. Подключение к поставщикам центра сертификации (ЦС).  

    -   Администратор организации должен подключить свою компанию (например, Contoso) хотя бы к одному поставщику ЦС.  

2. Администратор создает учетные данные инициатора запроса, чтобы зарегистрировать (и обновить) TLS/SSL-сертификаты в Key Vault.  

    -   Предоставляет конфигурацию, используемую для создания объекта издателя поставщика в хранилище ключей.  

Дополнительные сведения о создании объектов издателя на портале сертификатов см. в [записи блога по сертификатам Key Vault](/archive/blogs/kv/manage-certificates-via-azure-key-vault).  

Key Vault позволяет создать несколько объектов издателей с различной конфигурацией поставщиков издателей. После создания объекта издателя на его имя можно ссылаться в одной или нескольких политиках сертификата. Ссылка на объект издателя указывает, чтобы Key Vault использовал указанную в этом объекте конфигурацию при запросе сертификата x509 из поставщика ЦС во время создания или обновления сертификата.  

Объекты издателя создаются в хранилище и могут использоваться только с сертификатами в том же хранилище.  

## <a name="certificate-contacts"></a>Контакты сертификатов

Контакты сертификатов содержат контактную информацию для отправки уведомлений, активируемых событиями времени существования сертификата. Информация о контактах совместно используется всеми сертификатами в хранилище ключей. Уведомление отправляется всем контактам, указанным для события, связанного с любым сертификатом в хранилище ключей. Сведения о том, как указать контактные данные ответственного лица по вопросам о сертификатах, см. [здесь](overview-renew-certificate.md#steps-to-set-certificate-notifications).  

## <a name="certificate-access-control"></a>Контроль доступа к сертификату

 Контролем доступа к сертификатам управляет Key Vault. Его предоставляет Key Vault, которое выступает в роли контейнера для этих сертификатов. Существует политика управления доступом к сертификатам, отличная от политики управления доступом к ключам и секретам в том же Key Vault. Пользователи могут создать одно или несколько хранилищ для хранения сертификатов и должны поддерживать соответствующую сценарию сегментацию и управление сертификатами.  Дополнительные сведения об управлении доступом к сертификатам см. [здесь](certificate-access-control.md).

## <a name="next-steps"></a>Дальнейшие действия

- [Сведения о Key Vault](../general/overview.md)
- [Сведения о ключах, секретах и сертификатах](../general/about-keys-secrets-certificates.md)
- [Сведения о ключах](../keys/about-keys.md)
- [О секретах](../secrets/about-secrets.md)
- [Аутентификация, запросы и ответы](../general/authentication-requests-and-responses.md)
- [Руководство разработчика Azure Key Vault](../general/developers-guide.md)
