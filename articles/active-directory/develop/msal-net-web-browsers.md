---
title: Использование веб-браузеров (MSAL.NET) |Azure
titleSuffix: Microsoft identity platform
description: Ознакомьтесь с конкретными рекомендациями по использованию Xamarin Android с библиотекой проверки подлинности Майкрософт для .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/18/2020
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 23ba50a6eca1e398b9d459153b84719909f2ecac
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "99583745"
---
# <a name="using-web-browsers-msalnet"></a>Использование веб-браузеров (MSAL.NET)

Для интерактивной проверки подлинности нужны веб-браузеры. По умолчанию MSAL.NET поддерживает [системный веб-браузер](#system-web-browser-on-xamarinios-xamarinandroid) в Xamarin.iOS и Xamarin.Android. Но [вы также можете включить встроенный веб-браузер](#enable-embedded-webviews-on-ios-and-android) в зависимости от ваших требований (пользовательский интерфейс, потребность в едином входе, безопасность) в приложениях [Xamarin.iOS](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinios) и [Xamarin.Android](#detecting-the-presence-of-custom-tabs-on-xamarinandroid). Кроме того, можно даже [выбирать веб-браузер динамически](#detecting-the-presence-of-custom-tabs-on-xamarinandroid) в зависимости от наличия Chrome или браузера, поддерживающего настраиваемые вкладки Chrome в Android. MSAL.NET поддерживает только системный браузер в классических приложениях .NET Core.

## <a name="web-browsers-in-msalnet"></a>Веб-браузеры в MSAL.NET

### <a name="interaction-happens-in-a-web-browser"></a>Взаимодействие происходит в веб-браузере

Важно понимать, что при получении токена в интерактивном режиме содержимое диалогового окна предоставляется не библиотекой, а службой токенов безопасности (STS). Конечная точка проверки подлинности возвращает код HTML и JavaScript для управления взаимодействием, отображаемым в веб-браузере или веб-элементе управления. Предоставление разрешения службе токенов безопасности на обработку взаимодействия HTML имеет много преимуществ:

- Пароль (если он был введен) никогда не хранится ни приложением, ни библиотекой проверки подлинности.
- Позволяет настроить перенаправление на других поставщиков удостоверений. Например, для входа в рабочую, учебную или личную учетную запись можно использовать MSAL, а в учетную запись социальной сети — Azure AD B2C.
- Позволяет службе токенов безопасности управлять условным доступом, например, путем выполнения пользователем [многофакторной проверки подлинности (MFA)](../authentication/concept-mfa-howitworks.md) на этапе проверки подлинности (ввод ПИН-кода Windows Hello, ответ на вызов по телефону или использование приложения проверки подлинности на телефоне). Если требуемая многофакторная проверка подлинности еще не настроена, пользователь может настроить ее в том же диалоговом окне.  Пользователь вводит номер мобильного телефона, после по инструкциям он устанавливает приложение проверки подлинности и сканирует QR-тег для добавления учетной записи. Такое взаимодействие под руководством сервера очень удобно.
- Позволяет пользователю изменить свой пароль в этом же диалоговом окне, если срок действия пароля истек (при этом для старого пароля и нового пароля предоставляются дополнительные поля).
- Дает возможность использовать фирменную символику клиента или приложения (изображения) под контролем администратора клиента Azure AD или владельца приложения.
- Позволяет пользователям предоставить приложению доступ к ресурсам и областям от своего имени сразу после проверки подлинности.

### <a name="embedded-vs-system-web-ui"></a>Встроенный и системный веб-интерфейс

MSAL.NET — это библиотека для нескольких платформ. Она включает код, зависящий от платформы, для размещения окна браузера в элементе управления пользовательского интерфейса. Например, для классической платформы .NET она использует WinForms, для Xamarin — собственные мобильные элементы управления и т. д. Этот элемент управления называется встроенным (`embedded`) веб-интерфейсом. Кроме того, MSAL.NET также может запускать системный браузер.

Как правило, рекомендуется использовать платформу по умолчанию, и обычно это системный браузер. Системный браузер лучше запоминает пользователей, выполнивших вход ранее. Чтобы изменить это поведение, используйте `WithUseEmbeddedWebView(bool)`

### <a name="at-a-glance"></a>Вкратце

| FRAMEWORK        | Внедренный | Система | По умолчанию |
| ------------- |-------------| -----| ----- |
| Классическая платформа .NET     | Да | Да^ | Внедренный |
| .NET Core     | Нет | Да^ | Система |
| .NET Standard | Нет | Да^ | Система |
| UWP | Да | Нет | Внедренный |
| Xamarin.Android | Да | Да  | Система |
| Xamarin.iOS | Да | Да  | Система |
| Xamarin.Mac| Да | Нет | Внедренный |

^ Требуется URI перенаправления http://localhost

## <a name="system-web-browser-on-xamarinios-xamarinandroid"></a>Системный веб-браузер в Xamarin.iOS, Xamarin.Android

По умолчанию MSAL.NET поддерживает системный веб-браузер в Xamarin.iOS, Xamarin.Android и .NET Core. Для всех платформ, предоставляющих пользовательский интерфейс (к таким не относится .NET Core), диалоговое окно предоставляется библиотекой, позволяющей внедрить элемент управления веб-браузера. Библиотека MSAL.NET также использует встроенное веб-представление для классических приложений .NET и WAB для платформы UWP. При этом по умолчанию она использует **системный веб-браузер** для приложений Xamarin iOS и Xamarin Android. В iOS она даже позволяет выбрать веб-представление в зависимости от версии операционной системы (iOS 12, iOS 11, более ранние версии).

Использование системного браузера дает значительное преимущество за счет совместного использования состояния единого входа с другими приложениями и веб-приложениями без потребности в брокере (корпоративном портале или Authenticator). По умолчанию в MSAL.NET для платформ Xamarin iOS и Xamarin Android использовался системный браузер, поскольку на этих платформах системный веб-браузер занимает весь экран, а процесс взаимодействия с пользователем лучше. Системное веб-представление неотличимо от диалогового окна. Но в iOS пользователю может потребоваться предоставить согласие на вызов приложения в браузере, что может быть неудобно.

## <a name="system-browser-experience-on-net-core"></a>Взаимодействие с системным браузером в .NET Core

В .NET Core библиотека MSAL.NET запускает системный браузер как отдельный процесс. MSAL.NET не может контролировать этот браузер. Но когда пользователь завершит проверку подлинности, веб-страница будет перенаправлена таким образом, что MSAL.NET сможет перехватить универсальный код ресурса (URI).

Вы также можете настроить приложения, написанные для классической платформы .NET, чтобы использовать этот браузер. Для этого укажите следующее:

```csharp
await pca.AcquireTokenInteractive(s_scopes)
         .WithUseEmbeddedWebView(false)
```

MSAL.NET не может определить, переключается ли пользователь на другое приложение или просто закрывает браузер. Для приложений, использующих этот прием, рекомендуется определить время ожидания (с помощью `CancellationToken`). Рекомендуется задать время ожидания не менее нескольких минут, чтобы учесть случаи, когда пользователю предлагается изменить пароль или выполнить многофакторную проверку подлинности.

### <a name="how-to-use-the-default-os-browser"></a>Использование браузера ОС по умолчанию

Библиотека MSAL.NET должна прослушивать `http://localhost:port` и перехватывать код, который AAD отправляет при прохождении пользователем проверки подлинности (подробные сведения см. в статье о [коде авторизации](v2-oauth2-auth-code-flow.md)).

Чтобы включить системный браузер, сделайте следующее:

1. Во время регистрации приложения укажите `http://localhost` в качестве URI перенаправления (в настоящее время не поддерживается B2C)
2. При создании PublicClientApplication укажите этот URI перенаправления:

```csharp
IPublicClientApplication pca = PublicClientApplicationBuilder
                            .Create("<CLIENT_ID>")
                             // or use a known port if you wish "http://localhost:1234"
                            .WithRedirectUri("http://localhost")  
                            .Build();
```

> [!Note]
> При настройке `http://localhost` библиотека MSAL.NET самостоятельно найдет случайный открытый порт и будет его использовать.

### <a name="linux-and-mac"></a>Linux и Mac

В ОС Linux MSAL.NET открывает браузер ОС по умолчанию с помощью средства xdg-open. Чтобы устранить неполадки, запустите средство с помощью терминала, например `xdg-open "https://www.bing.com"`. На компьютерах Mac браузер открывается путем вызова `open <url>`.

### <a name="customizing-the-experience"></a>Настройка взаимодействия с пользователем

> [!NOTE]
> Настройка доступна в MSAL.NET 4.1.0 и более поздних версиях.

MSAL.NET может ответить HTTP-сообщением при получении токена или в случае ошибки. Можно отобразить HTML-сообщение или перенаправить на определенный URL-адрес:

```csharp
var options = new SystemWebViewOptions() 
{
    HtmlMessageError = "<p> An error occured: {0}. Details {1}</p>",
    BrowserRedirectSuccess = new Uri("https://www.microsoft.com");
}

await pca.AcquireTokenInteractive(s_scopes)
         .WithUseEmbeddedWebView(false)
         .WithSystemWebViewOptions(options)
         .ExecuteAsync();
```

### <a name="opening-a-specific-browser-experimental"></a>Открытие конкретного браузера (экспериментальная функция)

Вы можете настроить способ, с помощью которого MSAL.NET открывает браузер. Например, вместо использования любого браузера по умолчанию можно принудительно открыть конкретный браузер:

```csharp
var options = new SystemWebViewOptions() 
{
    OpenBrowserAsync = SystemWebViewOptions.OpenWithEdgeBrowserAsync
}
```

### <a name="uwp-doesnt-use-the-system-webview"></a>UWP не использует System WebView

Однако для классических приложений запуск System WebView приводит к менее удобному взаимодействию с пользователем, так как пользователь видит браузер, в котором у него уже могут быть открытые вкладки. После проверки подлинности пользователям отображается страница с просьбой закрыть окно. Если пользователь будет действовать невнимательно, то может закрыть весь процесс (в том числе другие вкладки, которые не связаны с проверкой подлинности). При использовании системного браузера на компьютере также нужно будет открыть локальные порты и прослушивать их, что может потребовать дополнительных разрешений для приложения. Возможно, разработчику, пользователю или администратору, не подойдет это требование.

## <a name="enable-embedded-webviews-on-ios-and-android"></a>Включение встроенных веб-представлений на iOS и Android

Вы также можете включить встроенные веб-представления в приложениях Xamarin.iOS и Xamarin.Android. Начиная с MSAL.NET 2.0.0 (предварительная версия), MSAL.NET также поддерживает использование варианта со **встроенным** веб-представление. ADAL.NET поддерживает только встроенное веб-представление.

Как разработчик, использующий MSAL.NET для Xamarin, вы можете использовать либо встроенные веб-представления, либо системные браузеры. Выбор зависит от того, какие аспекты взаимодействия с пользователем и безопасности нужно обеспечить.

MSAL.NET пока еще не поддерживает брокеры Android и iOS. Поэтому для обеспечения единого входа (SSO) обозреватель системы может быть лучшим вариантом. Обеспечение поддержки брокеров при использовании встроенного веб-браузера находится в списке запланированных функций для будущих версий MSAL.NET.

### <a name="differences-between-embedded-webview-and-system-browser"></a>Различия между встроенным веб-представлением и системным браузером
Между встроенным веб-представлением и системным браузером в MSAL.NET существуют некоторые визуальные различия.

**Интерактивный вход с помощью MSAL.NET и встроенного веб-представления:**

![встроенный](media/msal-net-web-browsers/embedded-webview.png)

**Интерактивный вход с помощью MSAL.NET и системного браузера:**

![Системный браузер](media/msal-net-web-browsers/system-browser.png)

### <a name="developer-options"></a>Возможности для разработчика

Как разработчик, использующий MSAL.NET, вы можете выбрать один из вариантов отображения интерактивного диалогового окна от службы токенов безопасности:

- **Системный браузер.** По умолчанию в библиотеке задано использование системного браузера. Если вы используете Android, ознакомьтесь со сведениями о [системных браузерах](msal-net-system-browser-android-considerations.md), чтобы узнать, какие браузеры поддерживают проверку подлинности. При использовании системного браузера в Android рекомендуется использовать для устройства браузер, поддерживающий настраиваемые вкладки Chrome.  В противном случае при проверке подлинности может произойти сбой.
- **Встроенное веб-представление.** Для использования только встроенного веб-представления в MSAL.NET построитель параметров `AcquireTokenInteractively` содержит метод `WithUseEmbeddedWebView()`.

    iOS

    ```csharp
    AuthenticationResult authResult;
    authResult = app.AcquireTokenInteractively(scopes)
                    .WithUseEmbeddedWebView(useEmbeddedWebview)
                    .ExecuteAsync();
    ```

    Android:

    ```csharp
    authResult = app.AcquireTokenInteractively(scopes)
                .WithParentActivityOrWindow(activity)
                .WithUseEmbeddedWebView(useEmbeddedWebview)
                .ExecuteAsync();
    ```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinios"></a>Выбор между встроенным веб-браузером или системным браузером в Xamarin.iOS

В приложении iOS в `AppDelegate.cs` можно инициализировать для параметра `ParentWindow` значение `null`. Это окно не используется в iOS.

```csharp
App.ParentWindow = null; // no UI parent on iOS
```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid"></a>Выбор между встроенным веб-браузером или системным браузером в Xamarin.Android

В приложении Android в `MainActivity.cs` можно задать родительское действие, чтобы результат проверки подлинности был возвращен к нему:

```csharp
 App.ParentWindow = this;
```

Затем в `MainPage.xaml.cs` укажите следующее:

```csharp
authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(true)
                      .ExecuteAsync();
```

#### <a name="detecting-the-presence-of-custom-tabs-on-xamarinandroid"></a>Определение наличия настраиваемых вкладок в Xamarin.Android

Если вы хотите использовать системный веб-браузер для включения единого входа с приложениями, работающими в браузере, но вам важен процесс взаимодействия с пользователями на устройствах Android без браузера с поддержкой настраиваемых вкладок, можно выбрать вызов метода `IsSystemWebViewAvailable()` в `IPublicClientApplication`. Этот метод возвращает `true`, если PackageManager обнаруживает настраиваемые вкладки и `false`, если они не обнаружены на устройстве.

В зависимости от возвращаемого этим методом значения и ваших требований можете принять решение:

- Можно вернуть пользователю настраиваемое сообщение об ошибке. Пример: "Установите Chrome, чтобы приступить к проверке подлинности".
- Можно вернуться к варианту со встроенным веб-представлением и запустить пользовательский интерфейс в виде встроенного веб-представления.

В приведенном ниже коде показан вариант со встроенным веб-представлением:

```csharp
bool useSystemBrowser = app.IsSystemWebviewAvailable();

authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(!useSystemBrowser)
                      .ExecuteAsync();
```

#### <a name="net-core-doesnt-support-interactive-authentication-with-an-embedded-browser"></a>.NET Core не поддерживает интерактивную проверку подлинности с помощью встроенного браузера

Для .NET Core получение токенов в интерактивном режиме доступно только через системный веб-браузер, а не с помощью встроенных веб-представлений. Действительно, .NET Core пока не предоставляет пользовательский интерфейс.
Если вы хотите настроить взаимодействие пользователя с системным веб-браузером, можете реализовать интерфейс [IWithCustomUI](scenario-desktop-acquire-token.md#withcustomwebui) и даже предоставить собственный браузер.
