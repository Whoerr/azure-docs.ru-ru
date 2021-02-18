---
title: Подключение к API служб мультимедиа Azure v3 — Node.js
description: В этой статье показано, как подключиться к API служб мультимедиа v3 с помощью Node.js.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/17/2021
ms.author: inhenkel
ms.custom: devx-track-js
ms.openlocfilehash: 9628e46281e267bd1c1fd8277a3a975bc338c6dc
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2021
ms.locfileid: "101096050"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>Подключение к API служб мультимедиа v3 — Node.js

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

В этой статье показано, как подключиться к пакету SDK для служб мультимедиа Azure версии 3 node.js с помощью метода входа субъекта-службы.

## <a name="prerequisites"></a>Предварительные требования

- Установите [Node.js](https://nodejs.org/en/download/).
- Установите [typescript](https://www.typescriptlang.org/download).
- [Создание учетной записи Служб мультимедиа](./create-account-howto.md). Обязательно запомните имя группы ресурсов и имя учетной записи Служб мультимедиа.

> [!IMPORTANT]
> Ознакомьтесь с [соглашениями об именовании](media-services-apis-overview.md#naming-conventions) служб мультимедиа Azure, чтобы ознакомиться с важными ограничениями именования сущностей. 

## <a name="reference-documentation-for-azurearm-mediaservices"></a>Справочная документация по @Azure/arm-mediaservices
- [Справочная документация по модулям служб мультимедиа Azure для Node.js](https://docs.microsoft.com/javascript/api/overview/azure/media-services?view=azure-node-latest)

## <a name="more-developer-documentation-for-nodejs-on-azure"></a>Дополнительная документация для разработчиков Node.js в Azure
- [Azure для Node.js разработчиков & JavaScript](https://docs.microsoft.com/azure/developer/javascript/?view=azure-node-latest)
- [Исходный код служб мультимедиа в @azure/azure-sdk-for-js репозитории центра Git](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/mediaservices/arm-mediaservices)
- [Документация по пакету Azure для разработчиков Node.js](https://docs.microsoft.com/javascript/api/overview/azure/?view=azure-node-latest)

## <a name="install-the-packages"></a>Установка пакетов

1. Создайте package.jsв файле с помощью любимого редактора.
1. Откройте файл и вставьте следующий код:

   Обязательно получите последнюю версию [пакета SDK для AzureMediaServices для JavaScript](https://www.npmjs.com/package/@azure/arm-mediaservices).

```json
{
  "name": "media-services-node-sample",
  "version": "0.1.0",
  "description": "",
  "main": "./index.ts",
  "dependencies": {
    "@azure/arm-mediaservices": "^8.0.0",
    "@azure/abort-controller": "^1.0.2",
    "@azure/ms-rest-nodeauth": "^3.0.6",
    "@azure/storage-blob": "^12.4.0",
  }
}
```

Должны быть указаны следующие пакеты:

|Пакет|Описание|
|---|---|
|`@azure/arm-mediaservices`|Пакет SDK служб мультимедиа Azure. <br/>Чтобы убедиться, что используется последний пакет служб мультимедиа Azure, установите флажок [NPM Install @azure/arm-mediaservices ](https://www.npmjs.com/package/@azure/arm-mediaservices).|
|`@azure/ms-rest-nodeauth` | Требуется для проверки подлинности AAD с помощью субъекта-службы или управляемого удостоверения|
|`@azure/storage-blob`|Пакет SDK для службы хранилища. Используется при отправке файлов в активы.|
|`@azure/ms-rest-js`| Используется для входа в систему.|
|`@azure/storage-blob` | Используется для отправки и скачивания файлов в ресурсы в службах мультимедиа Azure для кодирования.|
|`@azure/abort-controller`| Используется вместе с клиентом хранилища для длительного времени ожидания выполнения операций скачивания|


Вы можете выполнить следующую команду, чтобы убедиться, что используется последний пакет:

### <a name="install-azurearm-mediaservices"></a>установить @azure/arm-mediaservices;
```
npm install @azure/arm-mediaservices
```

### <a name="install-azurems-rest-nodeauth"></a>установить @azure/ms-rest-nodeauth;

Установите минимальную версию " @azure/ms-rest-nodeauth ": "^ 3.0.0".

```
npm install @azure/ms-rest-nodeauth@"^3.0.0"
```

## <a name="connect-to-nodejs-client-using-typescript"></a>Подключение к Node.js клиенту с помощью TypeScript

1. Создайте файл TypeScript. TS с помощью любимого редактора.
1. Откройте файл и вставьте в него следующий код.
1. Создайте файл. env и заполните сведения из портал Azure. См. раздел [API доступа](./access-api-howto.md).

### <a name="sample-env-file"></a>Пример файла Sample. env
```
# copy the content of this file to a file named ".env". It should be stored at the root of the repo.
# The values can be obtained from the API Access page for your Media Services account in the portal.
AZURE_CLIENT_ID=""
AZURE_CLIENT_SECRET= ""
AZURE_TENANT_ID= ""

# Change this to match your AAD Tenant domain name. 
AAD_TENANT_DOMAIN = "microsoft.onmicrosoft.com"

# Set this to your Media Services Account name, resource group it is contained in, and location
AZURE_MEDIA_ACCOUNT_NAME = ""
AZURE_LOCATION= ""
AZURE_RESOURCE_GROUP= ""

# Set this to your Azure Subscription ID
AZURE_SUBSCRIPTION_ID= ""

# You must change these if you are using Gov Cloud, China, or other non standard cloud regions
AZURE_ARM_AUDIENCE= "https://management.core.windows.net"
AZURE_ARM_ENDPOINT="https://management.azure.com"

# DRM Testing
DRM_SYMMETRIC_KEY="add random base 64 encoded string here"
```

## <a name="typescript---hello-world---list-assets"></a>Typescript-Hello World — список ресурсов
В этом примере показано, как подключиться к клиенту служб мультимедиа с помощью субъекта-службы и вывести список ресурсов в учетной записи. Если вы используете новую учетную запись, список будет пустым. Чтобы просмотреть результаты, можно отправить на портал несколько ресурсов.

```ts
import * as msRestNodeAuth from "@azure/ms-rest-nodeauth";
import { AzureMediaServices } from '@azure/arm-mediaservices';
import { AzureMediaServicesOptions } from "@azure/arm-mediaservices/esm/models";
// Load the .env file if it exists
import * as dotenv from "dotenv";
dotenv.config();

export async function main() {
  // Copy the samples.env file and rename it to .env first, then populate it's values with the values obtained 
  // from your Media Services account's API Access page in the Azure portal.
  const clientId = process.env.AZURE_CLIENT_ID as string;
  const secret = process.env.AZURE_CLIENT_SECRET as string;
  const tenantDomain = process.env.AAD_TENANT_DOMAIN as string;
  const subscriptionId = process.env.AZURE_SUBSCRIPTION_ID as string;
  const resourceGroup = process.env.AZURE_RESOURCE_GROUP as string;
  const accountName = process.env.AZURE_MEDIA_ACCOUNT_NAME as string;

  let clientOptions: AzureMediaServicesOptions = {
    longRunningOperationRetryTimeout: 5, // set the time out for retries to 5 seconds
    noRetryPolicy: false // use the default retry policy.
  }

  const creds = await msRestNodeAuth.loginWithServicePrincipalSecret(clientId, secret, tenantDomain);
  const mediaClient = new AzureMediaServices(creds, subscriptionId, clientOptions);

  // List Assets in Account
  console.log("Listing Assets Names in account:")
  var assets = await mediaClient.assets.list(resourceGroup, accountName);

  assets.forEach(asset => {
    console.log(asset.name);
  });

  if (assets.odatanextLink) {
    console.log("There are more than 1000 assets in this account, use the assets.listNext() method to continue listing more assets if needed")
    console.log("For example:  assets = await mediaClient.assets.listNext(assets.odatanextLink)");
  }
}

main().catch((err) => {
  console.error("Error running sample:", err.message);
});
```

## <a name="run-the-sample-application-helloworld-listassets"></a>Запуск примера приложения HelloWorld-ListAssets

Клонирование репозитория для Node.js примеров

```git
git clone https://github.com/Azure-Samples/media-services-v3-node-tutorials.git
```

Измените каталог на папку AMSv3Samples
```bash
cd AMSv3Samples
```

Установите пакеты, используемые в packages.js, на
```
npm install 
```

Перейдите в папку HelloWorld-ListAssets
```bash
cd HelloWorld-ListAssets
```

Запустите Visual Studio Code из папки AMSv3Samples Это необходимо для запуска из папки, в которой находятся папка ". vscode" и tsconfig.jsдля файлов
```dotnetcli
cd ..
code .
```

Откройте папку для HelloWorld-Листассетс и откройте файл index. TS в редакторе Visual Studio Code.
В файле index. TS нажмите клавишу F5, чтобы запустить отладчик. Если у вас есть активы, которые уже находятся в учетной записи, должен отобразиться список ресурсов. Если учетная запись пуста, вы увидите пустой список.  Загрузите несколько ресурсов на портале, чтобы просмотреть результаты.

## <a name="more-samples"></a>Другие примеры

В [репозитории](https://github.com/Azure-Samples/media-services-v3-node-tutorials) доступны следующие примеры.

|Имя проекта|Вариант использования|
|---|---|
|Динамическое/index. TS| Пример простой потоковой передачи в реальном времени. **Предупреждение**. Убедитесь, что все ресурсы очищены и больше не взимается на портале при использовании Live.|
|Стреамфилессампле/index. TS| Базовый пример передачи локального файла или кодировки из исходного URL-адреса. В примере показано, как использовать пакет SDK для службы хранилища для загрузки содержимого и демонстрируется потоковая передача в проигрыватель. |
|Стреамфилесвисдрмсампле/index. TS| Демонстрирует кодирование и потоковую передачу с помощью Widevine и PlayReady DRM |
|Видеоиндексерсампле/index. TS| Пример использования предустановок видео и анализатора аудио для создания метаданных и ценной информации из видео-или звукового файла |

## <a name="see-also"></a>См. также раздел

- [Основные понятия служб мультимедиа Azure](concepts-overview.md)
- [Установка NPM @azure/arm-mediaservices](https://www.npmjs.com/package/@azure/arm-mediaservices)
- [Azure для Node.js разработчиков & JavaScript](https://docs.microsoft.com/azure/developer/javascript/?view=azure-node-latest)
- [Исходный код служб мультимедиа в @azure/azure-sdk-for-js репозитории](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/mediaservices/arm-mediaservices)

## <a name="next-steps"></a>Следующие шаги

Изучите справочную документацию [Node.js](/javascript/api/overview/azure/mediaservices/management) Служб мультимедиа и ознакомьтесь с [примерами](https://github.com/Azure-Samples/media-services-v3-node-tutorials), показывающими как использовать API Служб мультимедиа с Node.js.
