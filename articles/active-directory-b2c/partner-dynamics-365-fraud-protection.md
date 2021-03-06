---
title: Руководство по настройке Azure Active Directory B2C с защитой мошенничества Microsoft Dynamics 365
titleSuffix: Azure AD B2C
description: Руководство по настройке Azure Active Directory B2C с защитой от мошенничества Microsoft Dynamics 365 для обнаружения рискованных и мошеннических учетных записей
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/10/2021
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: cf441108c9fd0ae87f265604f6f0706d92516746
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101646560"
---
# <a name="tutorial-configure-microsoft-dynamics-365-fraud-protection-with-azure-active-directory-b2c"></a>Учебник. Настройка защиты от мошенничества Microsoft Dynamics 365 с помощью Azure Active Directory B2C

В этом примере учебника мы представили рекомендации по интеграции [Microsoft Dynamics 365 мошенничества](/dynamics365/fraud-protection/overview) (ДФП) с Azure Active Directory (AD) B2C.

Microsoft ДФП предоставляет клиентам возможность оценить, является ли риск попыток создания новых учетных записей и попытки входа в экосистему клиента обманным. Оценка Microsoft ДФП может использоваться клиентом для блокировки или невозможности подозрительных попыток создания новых фиктивных учетных записей или для взлома существующих учетных записей. Защита учетной записи включает в себя искусственный интеллект, поддерживающий отпечатки устройств, интерфейсы API для оценки рисков в режиме реального времени, правила и список для оптимизации стратегии рисков в соответствии с бизнес-потребностями клиентов и системы показателей для отслеживания эффективности защиты от мошенничества и тенденций в экосистеме клиента.

В этом примере мы будем интегрировать функции защиты учетной записи Microsoft ДФП с Azure AD B2C пользовательским потоком. Служба будет видеть отпечатки пальца при каждом входе в систему или при регистрации, а также наблюдать за продолжением или присутствием подозрительного поведения. Azure AD B2C вызывает конечную точку принятия решения из Microsoft ДФП, которая возвращает результат на основе всех прошлых и представимых действий от идентифицированного пользователя, а также пользовательские правила, указанные в службе Microsoft ДФП. Azure AD B2C принимает решение об утверждении на основе этого результата и передает его обратно в Microsoft ДФП.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, вам потребуется:

- Подписка Azure. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).

- [Клиент Azure AD B2C](./tutorial-create-tenant.md). Клиент связан с вашей подпиской Azure.

- Получите [подписку](https://dynamics.microsoft.com/pricing/#Sales)Microsoft ДФП. Можно также настроить [пробную версию клиента](https://dynamics.microsoft.com/ai/fraud-protection/signin/?RU=https%3A%2F%2Fdfp.microsoft.com%2Fsignin) .

## <a name="scenario-description"></a>Описание сценария

Интеграция Microsoft ДФП включает следующие компоненты:

- **Azure AD B2C клиент**: проверяет подлинность пользователя и действует как клиент Microsoft ДФП. Размещает скрипт отпечатка для сбора данных идентификации и диагностики каждого пользователя, выполняющего целевую политику. В дальнейшем блокируются или возникают проблемы с входом или регистрацией, если Microsoft ДФП находит подозрительные.

- **Пользовательская служба приложений**: веб-приложение, которое служит двум целям.

  - Служит для использования HTML-страниц в качестве пользовательского интерфейса платформы идентификации. Отвечают за внедрение скрипта отпечатка Microsoft Dynamics 365.

  - Контроллер API с конечными точками RESTFUL, которые соединяют Microsoft ДФП с Azure AD B2C. Обработка данных, структура и соблюдение требований безопасности обоих типов.

- **Служба отпечатка пальцев Microsoft ДФП**: динамически внедренный скрипт, который записывает данные телеметрии устройств и сведения о пользователе с самоподтверждением для создания уникально идентифицируемого отпечатка для пользователя, который будет использоваться позже в процессе принятия решений.

- **Конечные точки API Microsoft ДФП**: предоставляет результат принятия решения и принимает окончательное состояние, отражающее операцию, которую выполняет клиентское приложение. Azure AD B2C не взаимодействует с конечными точками напрямую из-за различных требований безопасности и полезных данных API, вместо этого использует службу приложений в качестве промежуточной.

Реализация показана на следующей схеме архитектуры.

![На рисунке показана схема архитектуры защиты от мошенничества Microsoft dynamics365](./media/partner-dynamics365-fraud-protection/microsoft-dynamics-365-fraud-protection-diagram.png)

|Шаг | Описание |
|:-----| :-----------|
| 1. | Пользователь поступает на страницу входа. Пользователи настроили регистрацию для создания новой учетной записи и ввода данных на странице. Azure AD B2C собирает атрибуты пользователя.
| 2. | Azure AD B2C вызывает API среднего уровня и передает атрибуты пользователя.
| 3. | API среднего уровня собирает атрибуты пользователя и преобразует их в формат, который может использовать Microsoft ДФП API. Затем после отправки его в Microsoft ДФП API.
| 4. | После того как Microsoft ДФП API потребляет информацию и обрабатывает ее, она возвращает результат в API среднего уровня.
| 5. | API среднего уровня обрабатывает сведения и отправляет в Azure AD B2C соответствующие сведения.
| 6. | Azure AD B2C получает сведения от API среднего уровня. Если отображается ответ на сбой, пользователю выдается сообщение об ошибке. Если отображается ответ об успешном выполнении, пользователь проходит проверку подлинности и записывается в каталог.

## <a name="set-up-the-solution"></a>Настройка решения

1. [Создание приложения Facebook](./identity-provider-facebook.md#create-a-facebook-application) , настроенного для Azure AD B2C Федерации.
2. [Добавьте секрет Facebook](./custom-policy-get-started.md#create-the-facebook-key) , созданный в качестве ключа политики инфраструктуры процедур идентификации.

## <a name="configure-your-application-under-microsoft-dfp"></a>Настройка приложения в Microsoft ДФП

[Настройте клиент Azure AD](/dynamics365/fraud-protection/integrate-real-time-api) для использования Microsoft ДФП.

## <a name="deploy-to-the-web-application"></a>Развертывание в веб-приложение

### <a name="implement-microsoft-dfp-service-fingerprinting"></a>Реализация отпечатка пальца службы Microsoft ДФП

[Отпечаток устройств Microsoft ДФП](/dynamics365/fraud-protection/device-fingerprinting) является обязательным для защиты учетных записей Microsoft ДФП.

>[!NOTE]
>Кроме Azure AD B2C страниц пользовательского интерфейса, клиент может также реализовать службу отпечатка пальца внутри кода приложения для более полного профилирования устройств. В этом примере не включена служба отпечатка пальца в коде приложения.

### <a name="deploy-the-azure-ad-b2c-api-code"></a>Развертывание кода API Azure AD B2C

Разверните [предоставленный код API](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Dynamics-Fraud-Protection/API) в службе Azure. Код можно [опубликовать из Visual Studio](/visualstudio/deployment/quickstart-deploy-to-azure?view=vs-2019).

Настройка CORS, добавление **разрешенного источника**`https://{your_tenant_name}.b2clogin.com`

>[!NOTE]
>Позже вам потребуется URL-адрес развернутой службы, чтобы настроить Azure AD с использованием требуемых параметров.

Дополнительные сведения см. в [документации по службе приложений](../app-service/app-service-web-tutorial-rest-api.md) .

### <a name="add-context-dependent-configuration-settings"></a>Добавление зависимых от контекста параметров конфигурации

Настройте параметры приложения в [службе приложений в Azure](../app-service/configure-common.md#configure-app-settings). Это позволяет безопасно настроить параметры без их возврата в репозиторий. Для API-интерфейса остальной необходимо указать следующие параметры:

| Параметры приложений | Источник | Примечания |
| :-------- | :------------| :-----------|
|Фраудпротектионсеттингс: InstanceId | Конфигурация Microsoft ДФП |     |
|Фраудпротектионсеттингс: Девицефинжерпринтингкустомерид | Идентификатор клиента для отпечатка оборудования Майкрософт |     |
| Фраудпротектионсеттингс: Апибасеурл |  Базовый URL-адрес на портале Microsoft ДФП   | Удалите "-int" для вызова производственного API.
|  Токенпровидерконфиг: ресурс | https://api.dfp.dynamics-int.com |   Удалите "-int" для вызова производственного API.  |
|   Токенпровидерконфиг: ClientId       |Идентификатор клиентского приложения Azure AD для защиты от мошенничества      |       |
| Токенпровидерконфиг: центр сертификации | https://login.microsoftonline.com/<directory_ID> | Ваш магазин для защиты от мошенничества центр клиента Azure AD |
| Токенпровидерконфиг: CertificateThumbprint * | Отпечаток сертификата, который будет использоваться для проверки подлинности в клиентском приложении Azure AD для вашего продавца |
| Токенпровидерконфиг: ClientSecret * | Секрет для клиентского приложения Azure AD для вашего продавца | Рекомендуется использовать диспетчер секретов |

* Установите только 1 из двух помеченных параметров в зависимости от того, выполняется ли проверка подлинности с помощью сертификата или секрета, например пароля.

## <a name="azure-ad-b2c-configuration"></a>Конфигурация Azure AD B2C

### <a name="replace-the-configuration-values"></a>Замена значений конфигурации

В предоставленных [пользовательских политиках](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Dynamics-Fraud-Protection/Policies)найдите следующие заполнители и замените их соответствующими значениями из своего экземпляра.

| Заполнитель | Заменить на | Примечания |
| :-------- | :------------| :-----------|
|{your_tenant_name} | Короткое имя вашего клиента |  "йоуртенант" из yourtenant.onmicrosoft.com   |
|{your_tenantId} | Идентификатор клиента клиента Azure AD B2C |  01234567-89ab-cdef-0123-456789abcdef   |
|  {your_tenant_IdentityExperienceFramework_appid}    |   Идентификатор приложения IdentityExperienceFramework, настроенного в клиенте Azure AD B2C    |  01234567-89ab-cdef-0123-456789abcdef   |
|  {your_tenant_ ProxyIdentityExperienceFramework _appid}     |  Идентификатор приложения ProxyIdentityExperienceFramework, настроенного в клиенте Azure AD B2C      |   01234567-89ab-cdef-0123-456789abcdef     |
|  {your_tenant_extensions_appid}   |  Идентификатор приложения для хранилища вашего клиента   |  01234567-89ab-cdef-0123-456789abcdef  |
|   {your_tenant_extensions_app_objectid}  | Идентификатор объекта приложения хранилища вашего клиента    | 01234567-89ab-cdef-0123-456789abcdef   |
|   {your_app_insights_instrumentation_key}  |   Ключ инструментирования экземпляра App Insights *  |   01234567-89ab-cdef-0123-456789abcdef |
|  {your_ui_base_url}   | Конечная точка в службе приложений, из которой обслуживаются ваши файлы пользовательского интерфейса    | https://yourapp.azurewebsites.net/B2CUI/GetUIPage   |
|   {your_app_service_url}  | URL-адрес службы приложений    |  https://yourapp.azurewebsites.net  |
|   {Ваш-Facebook-App-ID}  |  Идентификатор приложения Facebook, настроенного для Федерации Azure AD B2C   | 000000000000000   |
|  {Ваш-Facebook-App-Secret}   |  Имя ключа политики, который вы сохранили в качестве секрета приложения Facebook   | B2C_1A_FacebookAppSecret   |

* App Insights может находиться в другом клиенте. Это необязательный шаг. Удалите соответствующие TechnicalProfiles и Оречестратионстепс, если они не требуются.

### <a name="call-microsoft-dfp-label-api"></a>Вызов Microsoft ДФП Label API

Клиентам необходимо [реализовать метку API](/dynamics365/fraud-protection/integrate-ap-api). Дополнительные сведения см. в разделе [Microsoft ДФП API](https://apidocs.microsoft.com/services/dynamics365fraudprotection#/AccountProtection/v1.0) .

`URI: < API Endpoint >/v1.0/label/account/create/<userId>`

Значение userID должно быть таким же, как и в соответствующем значении Azure AD B2C конфигурации (ObjectID).

>[!NOTE]
>Добавьте уведомление о согласии на страницу коллекции атрибутов. Уведомлять о том, что данные телеметрии и удостоверений пользователей будут записаны для защиты учетной записи.

## <a name="configure-the-azure-ad-b2c-policy"></a>Настройка политики Azure AD B2C

1. Перейдите к [политике Azure AD B2C](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Dynamics-Fraud-Protection/Policies) в папке политики.

2. Чтобы скачать [LocalAccounts Starter Pack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/LocalAccounts) , следуйте этому [документу](./custom-policy-get-started.md?tabs=applications#custom-policy-starter-pack)

3. Настройте политику для клиента Azure AD B2C.

>[!NOTE]
>Обновите указанные политики, чтобы они были связаны с конкретным клиентом.

## <a name="test-the-user-flow"></a>Тестирование потока пользователя

1. Откройте клиент Azure AD B2C и в разделе политики выберите **инфраструктура процедур идентификации**.

2. Выберите ранее созданный **сигнупсигнин**.

3. Выберите **запустить поток пользователя** и выберите параметры:

   а. **Приложение**: выберите зарегистрированное приложение (пример — JWT)

   b. **URL-адрес ответа**: выберите **URL-адрес перенаправления**

   c. Выберите **Выполнить поток пользователя**.

4. Последовательно выберите процесс регистрации и создайте учетную запись.

5. Служба Microsoft ДФП будет вызываться во время потока, после создания атрибута пользователя. Если поток неполон, убедитесь, что пользователь не сохранен в каталоге.

>[!NOTE]
>Обновите правила непосредственно на портале Microsoft ДФП, если используется [механизм правил Microsoft ДФП](/dynamics365/fraud-protection/rules).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в следующих статьях:

- [Примеры Microsoft ДФП](https://github.com/Microsoft/Dynamics-365-Fraud-Protection-Samples)

- [Пользовательские политики в Azure AD B2C](./custom-policy-overview.md)

- [Приступая к работе с пользовательскими политиками в Azure AD B2C](./custom-policy-get-started.md?tabs=applications)
