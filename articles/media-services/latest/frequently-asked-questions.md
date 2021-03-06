---
title: Часто задаваемые вопросы о Службах мультимедиа Azure версии 3
description: В этой статье приводятся ответы на часто задаваемые вопросы о службах мультимедиа Azure v3.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: a452a056ff2bdbad5d2e461716ee1a56d36c8523
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/27/2021
ms.locfileid: "98897566"
---
# <a name="media-services-v3-frequently-asked-questions"></a>Часто задаваемые вопросы о службах мультимедиа v3

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

В этой статье приводятся ответы на часто задаваемые вопросы о службах мультимедиа Azure v3.

## <a name="general"></a>Общие сведения

### <a name="what-are-the-azure-portal-limitations-for-media-services-v3"></a>Каковы ограничения портал Azure для служб мультимедиа v3?

Вы можете использовать [портал Azure](https://portal.azure.com/) для управления динамическими событиями v3, просмотреть v3 Assets and Jobs, получить сведения о доступе к API-интерфейсам, шифровать содержимое. <br/>Для всех других задач управления (например, для управления преобразованиями и заданиями или анализа содержимого v3) используйте [REST API](/rest/api/media/accountfilters), [CLI](/cli/azure/ams)или один из поддерживаемых [пакетов SDK](media-services-apis-overview.md#sdks).

Если ваше видео ранее было отправлено в учетную запись Служб мультимедиа с помощью API Служб мультимедиа версии 3 или если содержимое было создано на основе выходных данных потоковой передачи, на портале Azure не будут отображаться кнопки **Кодировать**, **Анализировать** или **Зашифровать**. Для выполнения этих задач используйте API Служб мультимедиа версии 3.  

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>Какие роли Azure могут выполнять действия с ресурсами служб мультимедиа Azure? 

[Учетные записи служб мультимедиа см. в статье Управление доступом на основе ролей Azure (Azure RBAC)](rbac-overview.md).

### <a name="how-do-i-stream-to-apple-ios-devices"></a>Разделы справки поток на устройства Apple iOS?

Убедитесь, что у вас есть **(Format = m3u8-AAPL)** в конце пути (после **/manifest** части URL-адреса), чтобы сообщить серверу-источнику потоковой передачи о необходимости вернуть содержимое HTTP Live Streaming (HLS) для использования на устройствах Apple iOS Native. Дополнительные сведения см. в разделе [Доставка содержимого](dynamic-packaging-overview.md).

### <a name="how-do-i-configure-media-reserved-units"></a>Как настроить зарезервированные единицы мультимедиа?

Для заданий анализа аудио и анализа видео, активируемых индексатором служб мультимедиа V3 или Video, рекомендуется подготавливать учетную запись с 10 зарезервированными единицами мультимедиа S3 (Мрус). Если вам требуется более 10 S3 Мрус, откройте запрос в службу поддержки с помощью [портал Azure](https://portal.azure.com/).

Дополнительные сведения см. в разделе [масштабирование обработки мультимедиа](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>Что такое рекомендуемый метод для обработки видео?

[Преобразования](/rest/api/media/transforms) можно использовать для настройки общих задач кодирования или анализа видеоматериалов. Каждое преобразование описывает набор инструкций (или рабочий процесс задач) для обработки видео- и аудиофайлов. [Задание](/rest/api/media/jobs) — это фактический запрос к службам мультимедиа для применения преобразования к входному видео или аудио содержимому. После создания преобразования можно отправлять задания с помощью API служб мультимедиа или любого из опубликованных пакетов SDK. Дополнительные сведения см. в статье [Преобразования и задания](transforms-jobs-concept.md).

### <a name="i-uploaded-encoded-and-published-a-video-why-wont-the-video-play-when-i-try-to-stream-it"></a>Видео отправлено, закодировано и опубликовано. Почему видео не воспроизводится при попытке его потоковой передачи?

Одной из наиболее распространенных причин является отсутствие конечной точки потоковой передачи, из которой выполняется попытка воспроизведения в состоянии выполнения.

### <a name="how-does-pagination-work"></a>Как работает разбиение на страницы?

При использовании разбиения на страницы следует всегда использовать следующую ссылку для перечисления коллекции и не зависеть от конкретного размера страницы. Дополнительные сведения и примеры см. в [этой статье о фильтрации, упорядочении и разбиении на страницы](entities-overview.md).

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>Какие функции пока недоступны в службах мультимедиа Azure v3?

Дополнительные сведения см. [в разделе руководств по миграции](migrate-v-2-v-3-migration-introduction.md).

### <a name="what-is-the-process-of-moving-a-media-services-account-between-subscriptions"></a>В чем заключается процесс перемещения учетной записи служб мультимедиа между подписками?  

Дополнительные сведения см. [в статье Перемещение учетной записи служб мультимедиа между подписками](media-services-account-concept.md).

## <a name="live-streaming"></a>Потоковая передача в реальном времени 

### <a name="how-do-i-stop-the-live-stream-after-the-broadcast-is-done"></a>Разделы справки завершить поток в реальном времени после вещания?

Его можно подходить на стороне клиента или на стороне сервера.

#### <a name="client-side"></a>На стороне клиента

Веб-приложение должно запрашивать пользователя, если требуется завершить вещание по мере закрытия браузера. Это событие браузера, которое может быть обработано веб-приложением.

#### <a name="server-side"></a>На сервере

Вы можете отслеживать события в реальном времени, подписавшись на события службы "Сетка событий Azure". Дополнительные сведения см. в описании [схемы событий EventGrid](media-services-event-schemas.md#live-event-types).

Вы можете сделать одно из двух:

* [Подпишитесь](reacting-to-media-services-events.md) на события [Microsoft. Media. лививентенкодердисконнектед](media-services-event-schemas.md#liveeventencoderdisconnected) на уровне потока и следите за тем, чтобы в течение длительного времени не поступали повторного подключения для завершения и удаления интерактивного события.
* [Подпишитесь](reacting-to-media-services-events.md) на события [пульса](media-services-event-schemas.md#liveeventingestheartbeat) уровня Track. Если все дорожки имеют скорость передачи, равную 0, или последняя отметка времени больше не растет, можно безопасно завершить интерактивное событие. События пульса появляются каждые 20 секунд для каждой записи, поэтому это может быть немного подробным.

###  <a name="how-do-i-insert-breaksvideos-and-image-slates-during-a-live-stream"></a>Разделы справки вставлять разрывы, видео и изображения во время потока в реальном времени?

Функция кодирования в реальном времени в Службах мультимедиа версии 3 не поддерживает вставку видео или баннеров во время передачи динамического потока. 

Можно использовать [локальный динамический кодировщик](recommended-on-premises-live-encoders.md), чтобы переключать исходное видео. Многие приложения предоставляют возможность переключения источников, в том числе телепотоковых Wirecast, Switch Studio (в iOS) и OBS Studio (бесплатное приложение).

## <a name="content-protection"></a>Защита содержимого

### <a name="should-i-use-aes-128-clear-key-encryption-or-a-drm-system"></a>Следует ли использовать шифрование AES-128 с открытым ключом или систему DRM?

Клиенты часто интересуются, следует ли им использовать шифрование AES или систему DRM. Основное различие между двумя системами заключается в том, что при шифровании AES ключ содержимого передается клиенту по протоколу TLS, чтобы ключ был зашифрован при передаче, но без какого-либо дополнительного шифрования ("в очистке"). В результате ключ, используемый для расшифровки содержимого, доступен клиентскому проигрывателю, и его можно просмотреть в сетевой трассировке на клиенте в виде обычного текста. Шифрование незащищенного ключа AES-128 подходит для использования в случаях, когда зритель является доверенным лицом (например, шифрование корпоративного видео, распространяемого внутри компании, для просмотра сотрудниками).

Системы DRM, такие как PlayReady, Widevine и FairPlay, обеспечивают дополнительный уровень шифрования ключа, который используется для расшифровки содержимого, по сравнению с ключом AES-128 Clear. Ключ содержимого шифруется в ключе, защищенном средой выполнения DRM, помимо шифрования на транспортном уровне, обеспечиваемом TLS. Кроме того, расшифровка выполняется в защищенной среде на уровне операционной системы, где злоумышленнику труднее атаковать. Рекомендуется использовать DRM для случаев, когда средство просмотра может быть не доверенным лицом, и вам нужен самый высокий уровень безопасности.

### <a name="how-do-i-show-a-video-to-only-users-who-have-a-specific-permission-without-using-azure-ad"></a>Разделы справки показывать видео только пользователям с указанным разрешением без использования Azure AD?

Вам не нужно использовать какой-либо конкретный поставщик маркеров, например Azure Active Directory (Azure AD). Вы можете создать собственный поставщик [JWT](https://jwt.io/) (так называемый Служба маркеров безопасности или STS) с помощью шифрования асимметричного ключа. В пользовательской STS можно добавлять заявки на основе бизнес-логики.

Убедитесь, что издатель, аудитория и утверждения полностью совпадают между тем, что находится в JWT, и `ContentKeyPolicyRestriction` значением, используемым в `ContentKeyPolicy` .

Дополнительные сведения см. в статье [Защита содержимого с помощью динамического шифрования служб мультимедиа](content-protection-overview.md).

### <a name="how-and-where-did-i-get-a-jwt-token-before-using-it-to-request-a-license-or-key"></a>Как и где я получил маркер JWT, прежде чем использовать его для запроса лицензии или ключа?

Для рабочей среды требуется служба маркеров безопасности (то есть веб-служба), которая выдает маркер JWT при запросе HTTPS. Для тестирования можно использовать код, показанный в `GetTokenAsync` методе, определенном в [Program.CS](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).

Проигрыватель выполняет запрос, после того как пользователь прошел проверку подлинности, STS для такого маркера и назначает его в качестве значения маркера. Можно использовать [API решения "Проигрыватель мультимедиа Azure"](https://amp.azure.net/libs/amp/latest/docs/).

Пример запуска STS с симметричным или асимметричным ключом см. в разделе [средство JWT](https://aka.ms/jwt). Пример проигрывателя на основе Проигрыватель мультимедиа Azure с помощью маркера JWT см. в разделе [средство тестирования мультимедиа Azure](https://aka.ms/amtest). (Чтобы увидеть входные данные маркера, разверните ссылку **player_settings** .)

### <a name="how-do-i-authorize-requests-to-stream-videos-with-aes-encryption"></a>Разделы справки авторизовать запросы для потоковой передачи видео с шифрованием AES?

Правильный подход заключается в использовании службы маркеров безопасности. В службе STS, в зависимости от профиля пользователя, добавьте различные утверждения (например, "привилегированный пользователь", "базовый пользователь", "Бесплатная пробная версия пользователя"). С различными утверждениями в JWT пользователь может видеть разное содержимое. Для другого содержимого или ресурсов `ContentKeyPolicyRestriction` будет иметь соответствующее `RequiredClaims` значение.

Используйте API служб мультимедиа Azure для настройки доставки лицензий и ключей и шифрования ресурсов (как показано в [этом примере](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

Дополнительные сведения см. в разделе:

- [Обзор системы защиты содержимого](content-protection-overview.md)
- [Проектирование системы для защиты содержимого с несколькими подсистемами DRM и управлением доступом](design-multi-drm-system-with-access-control.md)

### <a name="should-i-use-http-or-https"></a>Следует ли использовать протокол HTTP или HTTPS?
Приложение проигрывателя ASP.NET MVC, которое мы разработали, должно поддерживать следующее:

* Проверку подлинности пользователя через Azure AD, который должен находиться в разделе HTTPS.
* Обмен маркерами JWT между клиентом и Azure AD, который находится в разделе HTTPS.
* Приобретение лицензий DRM клиентом, который должен находиться в разделе HTTPS, если доставка лицензий осуществляется Службами мультимедиа. Набор продуктов PlayReady не требует HTTPS для доставки лицензий. Если ваш сервер лицензирования PlayReady находится за пределами Служб мультимедиа, можно использовать HTTP или HTTPS.

Приложение проигрывателя ASP.NET использует рекомендуемый протокол HTTPS, поэтому проигрыватель находится на странице в разделе HTTPS. Однако предпочтительно использовать протокол HTTP для потоковой передачи, поэтому необходимо учитывать эти проблемы со смешанным содержимым:

* Браузер запрещает смешанное содержимое. Но подключаемые модули, такие как Silverlight и подключаемый модуль OSMF для сглаживания и ТИРЕ, разрешают это. Смешанное содержимое является проблемой безопасности из-за угрозы возможности внедрять вредоносные сценарии JavaScript, которые могут поставить под угрозу данные клиента. Браузеры по умолчанию блокируют эту возможность. На данный момент единственным способом обойти это на стороне сервера (источника) — разрешить все домены (и HTTPS, и HTTP). Это, скорее всего, тоже не самая лучшая идея.
* Не следует использовать смешанное содержимое. Приложение проигрывателя и Проигрыватель мультимедиа должны использовать HTTP или HTTPS. При воспроизведении смешанного содержимого для специалистов по Silverlight необходимо очистить предупреждение о смешанном содержимом. Техническая поддержка Flash обрабатывает смешанное содержимое без предупреждения о смешанном содержимом.
* Если конечная точка потоковой передачи была создана до августа 2014 г., она не будет поддерживать протокол HTTPS. В этом случае создайте и используйте новую конечную точку потоковой передачи для использования протокола HTTPS.

### <a name="what-about-live-streaming"></a>Как насчет динамической потоковой передачи?

Для защиты потоковой передачи в службах мультимедиа можно использовать точно такой же дизайн и реализацию, рассматривая ресурс, связанный с программой, как ресурс VOD. Чтобы обеспечить защиту динамического содержимого с помощью нескольких DRM, примените ту же настройку и обработку к ресурсу, как если бы он был ресурсом VOD, прежде чем связать ресурс с выходом в реальном времени.

### <a name="what-about-license-servers-outside-media-services"></a>Как насчет серверов лицензирования за пределами служб мультимедиа?

Часто клиенты вкладываются в ферму серверов лицензирования либо в собственном центре обработки данных, либо в одном из них, размещенных поставщиками служб DRM. Система защиты содержимого Служб мультимедиа дает возможность работы в гибридном режиме. Содержимое может размещаться и динамически защищаться в службах мультимедиа, а лицензии DRM доставляются серверами за пределами служб мультимедиа. В этом случае обратите внимание на следующие изменения:

* Служба маркеров безопасности должна выдавать маркеры, которые являются допустимыми и могут быть проверены на ферме серверов лицензий. Например, серверы лицензий Widevine, предоставляемые Axinom, требуют определенного маркера JWT, который содержит сообщение о правах. Необходимо иметь STS для выдаче такого JWT. 
* Настройка службы доставки лицензий в Службах мультимедиа больше не требуется. При настройке необходимо указать URL-адреса для приобретения лицензий (для PlayReady, Widevine и FairPlay) `ContentKeyPolicy` .

> [!NOTE]
> Widevine — это служба, предоставляемая Google, которая подчиняется условиям обслуживания и политики конфиденциальности Google.

## <a name="media-services-v2-vs-v3"></a>Службы мультимедиа версии 2 и v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Могу ли я использовать портал Azure для управления ресурсами версии 3?

В настоящее время [портал Azure](https://portal.azure.com/) можно использовать для:

* Управление [событиями Live](live-events-outputs-concept.md) в службах мультимедиа v3. 
* Просмотр [ресурсов](assets-concept.md)(не управляемых) v3. 
* [Получение сведений о доступе к API](./access-api-howto.md). 

Для всех других задач управления (например, [преобразований, заданий](transforms-jobs-concept.md) и [защиты содержимого](content-protection-overview.md)) используйте [REST API](/rest/api/media/), [Azure CLI](/cli/azure/ams)или один из поддерживаемых [пакетов SDK](media-services-apis-overview.md#sdks).

### <a name="is-there-an-assetfile-concept-in-v3"></a>Существует элемент AssetFile в версии 3?

`AssetFile`Концепция удалена из API служб мультимедиа для разделения служб мультимедиа от зависимости пакета SDK хранилища. Теперь служба хранилища Azure, а не службы мультимедиа, хранит информацию, относящуюся к пакету SDK хранилища. 

Дополнительные сведения см. в разделе [Руководство по миграции из версии 2 в версию 3 Служб мультимедиа](migrate-v-2-v-3-migration-introduction.md).

### <a name="where-did-client-side-storage-encryption-go"></a>Где происходит шифрование хранилища на стороне клиента?

Теперь рекомендуется использовать шифрование хранилища на стороне сервера (включено по умолчанию). Дополнительные сведения см. [в статье шифрование службы хранилища Azure для неактивных данных](../../storage/common/storage-service-encryption.md).

## <a name="offline-streaming"></a>Автономная потоковая передача

### <a name="fairplay-streaming-for-ios"></a>Потоковая передача FairPlay для iOS

Следующие часто задаваемые вопросы содержат помощь с устранением неполадок в автономной потоковой передаче FairPlay для iOS.

#### <a name="why-does-only-audio-play-but-not-video-during-offline-mode"></a>Почему в автономном режиме воспроизводится только звук, но нет видео?

Такое поведение, по-видимому, связано с дизайном примера приложения. При наличии альтернативной звуковой дорожки (например, для HLS) во время работы в автономном режиме как для iOS 10, так и для iOS 11 по умолчанию используется альтернативная звуковая дорожка. Чтобы компенсировать это поведение в автономном режиме, удалите из потока альтернативную звуковую дорожку. Чтобы сделать это в службах мультимедиа, добавьте фильтр динамического манифеста **Audio-Only = false**. Иными словами, URL-адрес HLS заканчивается на **. ISM/MANIFEST (формат = m3u8-AAPL, Audio-Only = false)**. 

#### <a name="why-does-it-still-play-audio-only-without-video-during-offline-mode-after-i-add-audio-onlyfalse"></a>Почему после добавления audio-only=false в автономном режиме по-прежнему воспроизводится аудио без видео?

В зависимости от структуры ключа кэша для сети доставки содержимого, содержимое может кэшироваться. Очистите кэш.

#### <a name="is-fps-offline-mode-supported-on-ios-11-in-addition-to-ios-10"></a>Поддерживается ли автономный режим кадров в iOS 11 в дополнение к iOS 10?

Да. Автономный режим FPS поддерживается в iOS 10 и в iOS 11.

#### <a name="why-cant-i-find-the-document-offline-playback-with-fairplay-streaming-and-http-live-streaming-in-the-fps-server-sdk"></a>Почему я не могу найти документ автономного воспроизведения с помощью потоковой передачи FairPlay и HTTP Live Streaming в пакете SDK сервера FPS?

Начиная с 4-й версии пакета SDK сервера FPS, этот документ был объединен с руководством по потоковому программированию FairPlay.

#### <a name="what-is-the-downloadedoffline-file-structure-on-ios-devices"></a>Какова структура скачанного или автономного файла на устройствах iOS?

Структура скачанного файла на устройстве iOS выглядит как на следующем снимке экрана. В папке `_keys` хранятся скачанные лицензии FPS, один файл хранилища для каждого узла службы лицензий. В папке `.movpkg` хранится аудио- и видеосодержимое. 

Первая папка с именем, заканчивающееся на тире, за которым следует число, содержит видеоматериал. Числовое значение — это Пиковая пропускная способность видеороликов. Вторая папка с именем, заканчивающимся тире, за которым следует 0, содержит аудио. Третья папка `Data` содержит главный список воспроизведения содержимого в кадре/с. Наконец, boot.xml предоставляет полное описание содержимого папки `.movpkg`. 

![Автономная структура файлов для примера приложения FairPlay iOS](media/offline-fairplay-for-ios/offline-fairplay-file-structure.png)

Ниже приведен пример файла boot.xml.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
  <Version>1.0</Version>
  <HLSMoviePackageType>PersistedStore</HLSMoviePackageType>
  <Streams>
    <Stream ID="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" Path="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(127000)/Manifest(aac_eng_2_127,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
    <Stream ID="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" Path="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(161000)/Manifest(video,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
  </Streams>
  <MasterPlaylist>
    <NetworkURL>https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/manifest(format=m3u8-aapl,audio-only=false)</NetworkURL>
  </MasterPlaylist>
  <DataItems Directory="Data">
    <DataItem>
      <ID>CB50F631-8227-477A-BCEC-365BBF12BCC0</ID>
      <Category>Playlist</Category>
      <Name>master.m3u8</Name>
      <DataPath>Playlist-master.m3u8-CB50F631-8227-477A-BCEC-365BBF12BCC0.data</DataPath>
      <Role>Master</Role>
    </DataItem>
  </DataItems>
</HLSMoviePackage>
```

### <a name="widevine-streaming-for-android"></a>Потоковая передача Widevine для Android

#### <a name="how-can-i-deliver-persistent-licenses-offline-enabled-for-some-clientsusers-and-non-persistent-licenses-offline-disabled-for-others-do-i-have-to-duplicate-the-content-and-use-separate-content-keys"></a>Как можно доставить постоянные лицензии (автономно включены) для некоторых клиентов и пользователей и непостоянных лицензий (автономный режим отключен) для других? Нужно ли дублировать содержимое и использовать отдельные ключи содержимого?

Так как службы мультимедиа V3 позволяют ресурсу иметь несколько `StreamingLocator` экземпляров, можно использовать:

* Один `ContentKeyPolicy` экземпляр с `license_type = "persistent"` , `ContentKeyPolicyRestriction` с утверждением `"persistent"` и `StreamingLocator` .
* Другой `ContentKeyPolicy` экземпляр с `license_type="nonpersistent"` , `ContentKeyPolicyRestriction` с утверждением в `"nonpersistent` "и его `StreamingLocator` .
* Два `StreamingLocator` экземпляра с разными `ContentKey` значениями.

В зависимости от бизнес-логики пользовательской службы маркеров безопасности публикуются разные заявки в маркере JWT. С маркером возможно получить только соответствующие лицензии и воспроизводить только соответствующие URL-адреса.

#### <a name="what-is-the-mapping-between-the-widevine-and-media-services-drm-security-levels"></a>Что такое сопоставление между уровнями безопасности DRM служб Widevine и Media Services?

В статье "Общие сведения об архитектуре DRM Widevine" для Google определены три уровня безопасности. Однако в [документации по службам мультимедиа Azure в шаблоне лицензии Widevine](widevine-license-template-overview.md) описываются пять уровней безопасности (требования к надежности клиента для воспроизведения). В этом разделе объясняется, как сопоставляются уровни безопасности.

Оба набора уровней безопасности определяются с помощью Google Widevine. Разница заключается в уровне использования: архитектура или API. В API Widevine используется пять уровней безопасности. `content_key_specs`Объект, который содержит `security_level` , десериализуется и передается в службу глобальной доставки Widevine службой лицензирования Widevine служб мультимедиа Azure. В следующей таблице показано сопоставление между двумя наборами уровней безопасности.

| **Уровни безопасности, определенные в архитектуре Widevine** |**Уровни безопасности, используемые в API Widevine**|
|---|---| 
| **Уровень безопасности 1**. Вся обработка, шифрование и управление содержимым выполняются в доверенной среде выполнения (tee). В некоторых моделях реализации обработка безопасности может выполняться в разных микросхемах.|**security_level = 5**: шифрование, декодирование и вся обработка носителя (сжатые и несжатые) должны быть обработаны в аппаратно-Tee.<br/><br/>**security_level = 4**: шифрование и декодирование содержимого должно выполняться в аппаратно-TEEной конфигурации.|
**Уровень безопасности 2**. шифрование (но не обработка видео) выполняется в Tee. Расшифрованные буферы возвращаются в домен приложения и обрабатываются через отдельное оборудование или программное обеспечение. Однако на уровне 2 криптографические данные по-прежнему обрабатываются только в пределах TEE.| **security_level = 3**: материал ключа и операции шифрования должны выполняться в аппаратно-TEEной конфигурации. |
| **Уровень безопасности 3**: на устройстве нет Tee. Для защиты криптографических данных и расшифрованного содержимого в операционной системе узла можно предпринять соответствующие меры. Реализация уровня 3 может также включать в себя аппаратный механизм шифрования, но это расширяет только производительность, а не безопасность. | **security_level = 2**: требуется шифрование программного обеспечения и скрытый декодер.<br/><br/>**security_level = 1**: требуется шифрование на основе программного обеспечения.|

#### <a name="why-does-content-download-take-so-long"></a>Почему для загрузки содержимого требуется так много времени?

Увеличить скорости загрузки можно двумя способами.

* Включите сеть доставки содержимого, чтобы пользователи, скорее всего, нажимаюлись на это вместо конечной точки исходной или потоковой передачи для загрузки содержимого. Если пользователь нажимает конечную точку потоковой передачи, каждый сегмент HLS или фрагмент ШТРИХа динамически упаковывается и шифруется. Несмотря на то, что эта задержка выполняется в миллисекундах для каждого сегмента или фрагмента, при наличии часового видео накопленная задержка может быть большой, что приводит к более длительному скачиванию.
* Предоставьте пользователям возможность выборочно загружать слои качества видео и звуковые дорожки, а не все содержимое. Для автономного режима нет никакой точки в скачивании всех уровней качества. Это достигается двумя способами.

  * Управляемый клиентом: Приложение проигрывателя автоматически выбирает или выбирает пользователя, уровень качества видео и звуковые дорожки для загрузки.
  * Управляемая службой. с помощью функции динамического манифеста в службах мультимедиа Azure можно создать (глобальный) фильтр, ограничивающий список воспроизведения HLS или ШТРИХовых MPD на один слой качества видео и выбранные звуковые дорожки. Затем URL-адрес загрузки, представленный пользователям, будет включать этот фильтр.

## <a name="next-steps"></a>Дальнейшие действия

[Что такое Службы мультимедиа Azure версии 3?](media-services-overview.md)