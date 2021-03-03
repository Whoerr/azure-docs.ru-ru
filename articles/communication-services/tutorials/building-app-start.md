---
title: Руководство. Подготовка веб-приложения для Служб коммуникации Azure (Node.js)
titleSuffix: An Azure Communication Services tutorial
description: Узнайте, как создать базовое веб-приложение, которое поддерживает Службы коммуникации Azure.
author: nmurav
services: azure-communication-services
ms.author: nmurav
ms.date: 01/03/2012
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: d682524ae3ff5b82233a69959a309a7495e30bed
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658069"
---
# <a name="tutorial-prepare-a-web-app-for-azure-communication-services-nodejs"></a>Руководство. Подготовка веб-приложения для Служб коммуникации Azure (Node.js)

Службы коммуникации Azure позволяют добавлять в приложения средства коммуникации в реальном времени. В этом руководстве показано, как настроить веб-приложение, которое поддерживает Службы коммуникации Azure. Это вводное руководство для разработчиков, желающих ознакомиться с возможностями коммуникации в реальном времени.

Завершив работу с этим руководством, вы получите базовое веб-приложение с настроенными клиентскими библиотеками Служб коммуникации Azure, на основе которого вы сможете создать собственное решение для коммуникации в реальном времени.

Вы можете посетить [страницу Служб коммуникации Azure](https://github.com/Azure/communication) на сайте GitHub, чтобы оставить свой отзыв.

В этом руководстве вы узнаете, как:
> [!div class="checklist"]
> * Настройка среды разработки
> * настройка локального сервера;
> * добавление пакетов Служб коммуникации Azure на веб-сайт;
> * публикация веб-сайта в статических веб-сайтах Azure.

## <a name="prerequisites"></a>Предварительные требования

- Учетная запись Azure с активной подпиской. Дополнительные сведения см. на странице [Создайте бесплатную учетную запись Azure уже сегодня](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Обратите внимание, что бесплатная учетная запись предусматривает предоставление 200 долл. США в качестве денег на счете в Azure, что позволяет испытать в работе любое сочетание служб.
- [Visual Studio Code](https://code.visualstudio.com/). Мы будем использовать это решение для редактирования кода в локальной среде разработки.
- [webpack](https://webpack.js.org/). Это решение будет использоваться для создания пакета кода и его размещения в локальной среде.
- [Node.js](https://nodejs.org/en/). Это решение будет использоваться для установки зависимостей и управления ими, например для клиентских библиотек и webpack Служб коммуникации Azure.
- [nvm и npm](/windows/nodejs/setup-on-windows) для управления версиями.
- [Расширение службы хранилища Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) для Visual Studio Code. Это расширение требуется для публикации приложения в службе хранилища Azure. [Подробнее о размещении статических веб-сайтов в службе хранилища Azure](../../storage/blobs/storage-blob-static-website.md)
- [Расширение Службы приложений Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Это расширение, как и предыдущее, позволяет развертывать веб-сайты, но с дополнительной возможностью настройки полностью управляемой среды непрерывной поставки и непрерывной интеграции (CI/CD).
- [Расширение Функций Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) для создания собственных бессерверных приложений. Например, вы можете разместить в Функциях Azure приложение проверки подлинности.
- Активный ресурс Служб коммуникации и строка подключения. [Создайте ресурс Служб коммуникации.](../quickstarts/create-communication-resource.md)
- Маркер доступа пользователя. Инструкции по работе вы найдете в [кратком руководстве по маркерам доступа](../quickstarts/access-tokens.md?pivots=programming-language-javascript) и [руководстве по использованию доверенной службы](./trusted-service-tutorial.md).


## <a name="configure-your-development-environment"></a>Настройка среды разработки

Локальная среда разработки будет иметь следующую конфигурацию:

:::image type="content" source="./media/step-one-pic-one.png" alt-text="Архитектура среды разработки":::


### <a name="install-nodejs-nvm-and-npm"></a>Установка Node.js, а также nvm и npm

Мы будем использовать Node.js для скачивания и установки зависимостей, требуемых для работы приложения на стороне клиента. Мы применим это решение для создания статических файлов, которые затем разместим в Azure, поэтому вам можно не беспокоиться о настройке на стороне сервера.

Разработчики Windows могут воспользоваться [этим руководством по NodeJS](/windows/nodejs/setup-on-windows), чтобы настроить Node, nvm и npm.

При работе с этим руководством использовалась версия LTS 12.20.0. После установки nvm выполните следующую команду PowerShell, чтобы развернуть нужную версию:

```PowerShell
nvm list available
nvm install 12.20.0
nvm use 12.20.0
```

:::image type="content" source="./media/step-one-pic-two.png" alt-text="Применение nvm для развертывания Node.js":::

### <a name="configure-visual-studio-code"></a>Настройка Visual Studio Code

Вы можете скачать [Visual Studio Code](https://code.visualstudio.com/) для любой из [поддерживаемых платформ](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

### <a name="create-a-workspace-for-your-azure-communication-services-projects"></a>Создание рабочей области для проектов Служб коммуникации Azure

Создайте новую папку для хранения файлов проекта, например: `C:\Users\Documents\ACS\CallingApp`. В Visual Studio Code щелкните "Файл" и "Добавить папку в рабочую область", а затем добавьте созданную папку в рабочую область.

:::image type="content" source="./media/step-one-pic-three.png" alt-text="Создание новой рабочей области":::

В Visual Studio Code на панели слева откройте обозреватель. Здесь вы увидите созданную папку CallingApp в рабочей области Untitled.

:::image type="content" source="./media/step-one-pic-four.png" alt-text="Обозреватель":::

Вы можете указать для рабочей области любое имя. Чтобы проверить версию Node.js, щелкните правой кнопкой мыши папку CallingApp и выберите "Открыть в интегрированном терминале".

:::image type="content" source="./media/step-one-pic-five.png" alt-text="Открытие терминала":::

В окне терминала введите следующую команду, чтобы проверить версию Node.js, которую мы установили на предыдущем шаге:

```JavaScript
node --version
```

:::image type="content" source="./media/step-one-pic-six.png" alt-text="Проверка версии Node.js":::

### <a name="install-azure-extensions-for-visual-studio-code"></a>Установка расширений Azure для Visual Studio Code

Установите [расширение службы хранилища Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) из Visual Studio Marketplace или прямо в Visual Studio Code, выбрав "Представление" > "Расширения" > "Служба хранилища Azure".

:::image type="content" source="./media/step-one-pic-seven.png" alt-text="Установка расширения службы хранилища Azure, шаг 1":::

Выполните эти же действия для расширений [Функций Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) и [Службы приложений Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).


## <a name="set-up-a-local-webserver"></a>Настройка локального сервера

### <a name="create-a-new-npm-package"></a>Создание проекта npm

В окне терминала перейдите в папку рабочей области и введите следующую команду:

``` console
npm init -y
```

Эта команда инициализирует новый пакет npm и добавляет `package.json` в корневую папку проекта.

:::image type="content" source="./media/step-one-pic-eight.png" alt-text="JSON-файл пакета":::

Дополнительную документацию по команде npm init можно найти [здесь](https://docs.npmjs.com/cli/v6/commands/npm-init).

### <a name="install-webpack"></a>Установка webpack

[webpack](https://webpack.js.org/) позволяет упаковывать код в статические файлы, которые можно развернуть в Azure. Это решение включает собственный сервер разработки, который мы настроим для работы с примером вызова.

В терминале введите следующую команду, чтобы установить webpack:

``` Console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```

При работе с этим руководством использовались указанные выше версии. Указание `-dev` сообщает диспетчеру пакетов, что эта зависимость предназначена для разработки и не должна включаться в код, развертываемый в Azure.

Вы увидите, что в файл `package.json` в раздел devDependencies добавлены два новых пакета. Эти пакеты будут установлены в каталог `./CallingApp/node_modules/`.

:::image type="content" source="./media/step-one-pic-ten.png" alt-text="Конфигурация webpack":::

### <a name="configure-the-development-server"></a>Настройка сервера разработки

При запуске в браузере статического приложения (например, файла `index.html`) используется протокол `file://`. Чтобы модули npm работали правильно, нужно использовать протокол HTTP, настроив webpack в качестве локального сервера разработки.

Мы создадим две конфигурации: одну для разработки и одну для рабочей среды. При подготовке для рабочей среды файлы минифицируются, то есть из них удаляются все неиспользуемые пробелы и символы. Это полезно для рабочих сценариев, в которых нужно обеспечить минимальную задержку или замаскировать код.

Средство `webpack-merge` мы применим для работы с [разными файлами конфигурации для webpack](https://webpack.js.org/guides/production/).

Сначала давайте настроим среду разработки. Прежде всего нужно установить `webpack merge`. В окне терминала выполните следующую команду:

```Console
npm install --save-dev webpack-merge
```

Вы увидите, что в файл `package.json` в раздел devDependencies добавлена еще одна зависимость.

На следующем шаге нужно создать новый файл `webpack.common.js` и добавить в него следующий код:

```JavaScript
const path = require('path');
module.exports ={
    entry: './app.js',
    output: {
        filename:'app.js',
        path: path.resolve(__dirname, 'dist'),
    }
}
```

Затем мы добавим еще два файла, по одному для каждой конфигурации:

* webpack.dev.js
* webpack.prod.js

На следующем шаге необходимо изменить файл `webpack.dev.js`. Добавьте в него следующий код:

```JavaScript
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
    mode: 'development',
    devtool: 'inline-source-map',
});
```
В этой конфигурации общие параметры импортируются из `webpack.common.js`, затем два файла объединяются, устанавливается режим разработки и SourceMap определяется как inline-source-map.

Режим разработки означает, что webpack не будет минифицировать файлы и создавать оптимизированные рабочие файлы. Подробную документацию по режимам webpack можно найти [здесь](https://webpack.js.org/configuration/mode/).

Параметры SourceMap перечислены [здесь](https://webpack.js.org/configuration/devtool/#root). Настройка SourceMap упрощает отладку в браузере.

:::image type="content" source="./media/step-one-pic-11.png" alt-text="Настройка webpack":::

Чтобы запустить сервер разработки, откройте адрес `package.json` и добавьте следующий код в раздел scripts:

```JavaScript
    "build:dev": "webpack-dev-server --config webpack.dev.js"
```

Теперь этот файл будет выглядеть так:

```JavaScript
{
  "name": "CallingApp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server --config webpack.dev.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3"
  }
}
```

Вы добавили команду, которую можно использовать из npm.

:::image type="content" source="./media/step-one-pic-12.png" alt-text="Изменение package.json":::

### <a name="testing-the-development-server"></a>Тестирование сервера разработки

 Создайте в Visual Studio Code три файла в папке проекта:

* `index.html`
* `app.js`
* `app.css` (необязательно; позволяет настроить стиль приложения).

Вставьте в `index.html` следующий код:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My first ACS application</title>
    <link rel="stylesheet" href="./app.css"/>
    <script src="./app.js" defer></script>
</head>
<body>
    <h1>Hello from ACS!</h1>
</body>
</html>
```
:::image type="content" source="./media/step-one-pic-13.png" alt-text="HTML-файл":::

Добавьте в `app.js` следующий код:

```JavaScript
alert('Hello world alert!');
console.log('Hello world console!');
```
Добавьте в `app.css` следующий код:

```CSS
html {
    font-family: sans-serif;
  }
```
Не забудьте сохранить! Несохраненный файл обозначается белыми точками рядом с именами файлов в проводнике.

 :::image type="content" source="./media/step-one-pic-14.png" alt-text="Файл app.js с кодом JavaScript":::

Открыв эту страницу, вы увидите сообщение с оповещением и сообщение в консоли браузера.

:::image type="content" source="./media/step-one-pic-15.png" alt-text="Файл app.css":::

Выполните в терминале следующую команду, чтобы протестировать конфигурацию разработки:

```Console
npm run build:dev
```

Консоль отобразит сообщение о том, какой у работающего сервера адрес. По умолчанию это `http://localhost:8080`. Команду build:dev мы ранее добавили в `package.json`.

 :::image type="content" source="./media/step-one-pic-16.png" alt-text="Запуск сервера разработки":::

 Перейдите в браузере по адресу сервера и убедитесь, что открывается ранее настроенные страница и оповещение.

  :::image type="content" source="./media/step-one-pic-17.png" alt-text="HTML-страница":::


Пока сервер работает, вы можете внести в код любые изменения, после чего сервер и HTML-страница будут автоматически перезагружены.

Затем перейдите к файлу `app.js` в Visual Studio Code и удалите `alert('Hello world alert!');`. Сохраните этот файл и убедитесь, что оповещение исчезнет из браузера.

Чтобы остановить сервер, выполните `Ctrl+C` в терминале. Чтобы запустить сервер в любое время, выполните `npm run build:dev`.

## <a name="add-the-azure-communication-services-packages"></a>Добавление пакетов Служб коммуникации Azure

Используйте команду `npm install`, чтобы установить клиентскую библиотеку Служб коммуникации для реализации вызовов на JavaScript.

```Console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

Это действие добавит общие и вызывающие пакеты службы коммуникации Azure в качестве зависимостей основного пакета. Вы увидите,что в файл `package.json` добавлены два новых пакета. Дополнительные сведения о команде `npm install` см. [здесь](https://docs.npmjs.com/cli/v6/commands/npm-install).

:::image type="content" source="./media/step-one-pic-nine.png" alt-text="Установка пакетов Служб коммуникации Azure":::

Эти пакеты предоставляются командой разработчиков Служб коммуникации Azure и содержат библиотеки проверки подлинности и вызова. Команда --save сообщает, что приложение зависит от этих пакетов в рабочей среде, а значит их нужно включить в `dependencies` файла `package.json`. Теперь при сборке приложения для рабочей среды указанные пакеты будут включаться в рабочий код.


## <a name="publish-your-website-to-azure-static-websites"></a>Публикация веб-сайта в статических веб-сайтах Azure

### <a name="create-a-configuration-for-production-deployment"></a>Создание конфигурации для развертывания в рабочей среде

Добавьте в `webpack.prod.js` следующий код:

```JavaScript
const { merge } = require('webpack-merge');
 const common = require('./webpack.common.js');

 module.exports = merge(common, {
   mode: 'production',
 });
 ```

Учтите, что эта конфигурация будет объединена с файлом webpack.common.js (где мы указали входной файл и расположение для сохранения результатов) и приведет к включению режима рабочей среды.

В классе `package.json` добавьте следующий код.

```JavaScript
"build:prod": "webpack --config webpack.prod.js"
```

Файл должен выглядеть следующим образом.

```JavaScript
{
  "name": "CallingApp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server --config webpack.dev.js",
    "build:prod": "webpack --config webpack.prod.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@azure/communication-calling": "^1.0.0-beta.6",
    "@azure/communication-common": "^1.0.0"
  },
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3",
    "webpack-merge": "^5.7.3"
  }
}
```

 :::image type="content" source="./media/step-one-pic-20.png" alt-text="Настроенные файлы":::


В окне терминала выполните следующую команду:

```Console
npm run build:prod
```

Эта команда создаст папку `dist` с готовым к работе статическим файлом `app.js`.

 :::image type="content" source="./media/step-one-pic-21.png" alt-text="Рабочая сборка":::


### <a name="deploy-your-app-to-azure-storage"></a>Развертывание приложения в службе хранилища Azure

Скопируйте `index.html` и `app.css` в папку `dist`.

В папке `dist` создайте файл с именем `404.html`. Скопируйте в этот файл следующий код разметки:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="./app.css"/>
    <title>Document</title>
</head>
<body>
    <h1>The page does not exists.</h1>
</body>
</html>
```

Сохраните файл, нажав клавиши CTRL+S.

Щелкните его правой кнопкой мыши и выберите вариант развертывания в статическом веб-сайте через службу хранилища Azure.

:::image type="content" source="./media/step-one-pic-22.png" alt-text="Начало развертывания в Azure":::

В поле `Select subscription` выберите "Войти в Azure" или "Создать бесплатную учетную запись Azure", если вы еще не создали подписку.

:::image type="content" source="./media/step-one-pic-23.png" alt-text="Вход в Azure":::

Выберите `Create new Storage Account` > `Advanced`:

 :::image type="content" source="./media/step-one-pic-24.png" alt-text="Создание группы учетных записей хранения":::

 Укажите имя группы для службы хранения:

 :::image type="content" source="./media/step-one-pic-25.png" alt-text="Добавление имени учетной записи":::

Если нужно, создайте новую группу ресурсов:

  :::image type="content" source="./media/step-one-pic-26.png" alt-text="Создание новой группы":::

  Ответьте "Да" на предложение включить размещение статических веб-сайтов.

  :::image type="content" source="./media/step-one-pic-27.png" alt-text="Включение размещения статических веб-сайтов":::

В поле "Введите имя документа индекса" подтвердите имя файла по умолчанию, так как мы создали файл `index.html`.

Введите значение `404.html` в поле "Введите путь к документу с сообщением об ошибке 404".

Выберите расположение приложения. Это расположение определяет, какой обработчик мультимедиа будет в будущем использоваться вызывающим приложением при групповых вызовах.

Службы коммуникации Azure выбирают обработчик мультимедиа в зависимости от расположения приложения.

:::image type="content" source="./media/step-one-pic-28.png" alt-text="Выбор расположения":::

Дождитесь, пока завершится создание ресурса и веб-сайта.

Щелкните "Перейти к веб-сайту":

:::image type="content" source="./media/step-one-pic-29.png" alt-text="Развертывание завершено":::

С помощью средств разработки в браузере вы можете проверить исходный код и просмотреть файл, подготовленный для рабочей среды.

:::image type="content" source="./media/step-one-pic-30.png" alt-text="Веб-сайт":::

Перейдите на [портал Azure](https://portal.azure.com/#home), выберите созданную группу ресурсов и созданное приложение, затем перейдите к `Settings` > `Static website`. Здесь вы увидите, что включены статические веб-сайты, и увидите адреса основной конечной точки, документов для главного файла и для ошибок.

:::image type="content" source="./media/step-one-pic-31.png" alt-text="Выбор статического веб-сайта":::

В разделе "Служба BLOB-объектов" выберите "Контейнеры", и вы увидите два созданных контейнера: один для журналов ($logs) и другой для содержимого веб-сайта ($web).

:::image type="content" source="./media/step-one-pic-32.png" alt-text="Конфигурация контейнера":::

Если вы откроете `$web`, вы увидите файлы, ранее созданные в Visual Studio и развернутые в Azure.

:::image type="content" source="./media/step-one-pic-33.png" alt-text="Развертывание":::

Приложение можно в любой момент повторно развернуть из Visual Studio Code.

Теперь вы готовы к созданию первого веб-приложения Служб коммуникации Azure.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Добавление функции голосового вызова в приложение](../quickstarts/voice-video-calling/getting-started-with-calling.md)

Возможно, вы также захотите выполнить такие задачи:

- [Добавление чата в приложение](../quickstarts/chat/get-started.md)
- [Создание маркеров доступа пользователей](../quickstarts/access-tokens.md)
- [Сведения об архитектуре клиент-сервер](../concepts/client-and-server-architecture.md)
- [Сведения о проверке подлинности](../concepts/authentication.md)