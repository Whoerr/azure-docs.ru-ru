---
title: Общие сведения об агенте Connected Machine для Windows
description: В этой статье представлен подробный обзор доступного агента серверов с поддержкой Arc Azure, который поддерживает мониторинг виртуальных машин, размещенных в гибридных средах.
ms.date: 02/18/2021
ms.topic: conceptual
ms.openlocfilehash: ebd9412849b4a0b3081e892d7472e598ca6e8365
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101651099"
---
# <a name="overview-of-azure-arc-enabled-servers-agent"></a>Обзор агента серверов с поддержкой ARC в Azure

Агент подключенного компьютера с поддержкой Arc Azure позволяет управлять компьютерами Windows и Linux, размещенными за пределами Azure, в корпоративной сети или другом поставщике облачных служб. В этой статье предоставлен подробный обзор требований к агенту, системе и сети, а также различных методов развертывания.

>[!NOTE]
>Начиная с общего выпуска серверов с поддержкой дуги Azure в сентябре 2020, все предварительные версии агента подключенного компьютера Azure (агенты с версиями меньше 1,0) **устарели** до **2 февраля 2021**.  Этот период позволяет выполнить обновление до версии 1,0 или более поздней до выпуска предварительно выпущенных агентов, которые больше не смогут взаимодействовать со службой серверов с поддержкой ARC в Azure.

## <a name="agent-component-details"></a>Сведения о компоненте агента

Пакет агента подключенного компьютера Azure содержит несколько логических компонентов, Объединенных в пакеты.

* Служба метаданных гибридного экземпляра (ХИМДС) управляет подключением к Azure и удостоверением Azure подключенного компьютера.

* Гостевой агент настройки предоставляет In-Guestную политику и функциональность гостевой настройки, например оценку того, соответствует ли компьютер необходимым политикам.

    Обратите внимание на следующее поведение в [гостевой конфигурации](../../governance/policy/concepts/guest-configuration.md) политики Azure для отключенного компьютера:

    * Назначение политики гостевой конфигурации, предназначенное для отключенных компьютеров, не затрагивается.
    * Назначение гостя хранится локально в течение 14 дней. В течение 14-дневного периода, если агент подключенного компьютера повторно подключается к службе, назначения политик применяются заново.
    * Назначения удаляются через 14 дней и не переназначаются компьютеру после 14-дневного периода.

* Агент расширений управляет расширениями виртуальной машины, включая установку, удаление и обновление. Расширения загружаются из Azure и копируются в `%SystemDrive%\%ProgramFiles%\AzureConnectedMachineAgent\ExtensionService\downloads` папку в Windows, а для Linux — `/opt/GC_Ext/downloads` . В Windows расширение устанавливается по следующему пути `%SystemDrive%\Packages\Plugins\<extension>` , а в Linux расширение устанавливается в `/var/lib/waagent/<extension>` .

## <a name="instance-metadata"></a>Метаданные экземпляра

Метаданные о подключенном компьютере собираются после того, как агент подключенного компьютера регистрируется на серверах с поддержкой ARC. В частности:

* Имя, тип и версия операционной системы
* Имя компьютера
* полное доменное имя (FQDN) компьютера;
* версия агента подключенного компьютера.
* Active Directory и полное доменное имя DNS (FQDN)
* UUID (ИДЕНТИФИКАТОР BIOS)
* Период пульса агента подключенного компьютера
* версия агента подключенного компьютера.
* Открытый ключ для управляемого удостоверения
* Состояние соответствия политике и сведения (при использовании политик гостевой конфигурации политики Azure)

Агент из Azure запрашивает следующие сведения о метаданных:

* Расположение ресурса (регион)
* Virtual machine ID (Идентификатор виртуальной машины)
* Теги
* Сертификат управляемого удостоверения Azure Active Directory
* Назначения политик гостевой конфигурации
* Запросы расширения — установка, обновление и удаление.

## <a name="download-agents"></a>Скачивание агентов

Пакет агента Azure Connected Machine для Windows и Linux можно скачать из указанных ниже расположений.

* [Пакет установщика Windows для агента Windows](https://aka.ms/AzureConnectedMachineAgent) из Центра загрузки Майкрософт.

* Агент Linux распределяется через [репозиторий пакетов](https://packages.microsoft.com/) Microsoft в предпочтительном для распределения пакетном формате (RPM или DEB).

Агент Azure Connected Machine для Windows и Linux можно обновить до последней версии вручную или автоматически в зависимости от требований. Дополнительные сведения см. [здесь](manage-agent.md).

## <a name="prerequisites"></a>Предварительные требования

### <a name="supported-environments"></a>Поддерживаемые среды

Серверы с поддержкой Arc поддерживают установку агента подключенного компьютера на любой физический сервер и виртуальную машину, размещенные *за пределами* Azure. Сюда входят виртуальные машины, работающие на таких платформах, как VMware, Azure Stack ХЦИ и другие облачные среды. Серверы с поддержкой Arc не поддерживают установку агента на виртуальных машинах, работающих в Azure, или на виртуальных машинах, работающих в центре Azure Stack или Azure Stack, так как они уже смоделированы как виртуальные машины Azure.

### <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

Агент Azure Connected Machine официально поддерживают следующие версии операционных систем Windows и Linux:

- Windows Server 2008 R2, Windows Server 2012 R2 и более поздние версии (включая Server Core)
- Ubuntu 16,04 и 18,04 LTS (x64)
- CentOS Linux 7 (x64);
- SUSE Linux Enterprise Server (SLES) 15 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64)
- Amazon Linux 2 (x64)
- Oracle Linux 7

> [!WARNING]
> В имени узла Linux или компьютера Windows нельзя использовать зарезервированные слова или товарные знаки. В противном случае, при попытке зарегистрировать подключенный компьютер в Azure произойдет ошибка. Список зарезервированных слов см. в статье [Ошибки имен зарезервированных ресурсов](../../azure-resource-manager/templates/error-reserved-resource-name.md).

### <a name="required-permissions"></a>Необходимые разрешения

* Чтобы подключить компьютеры, необходимо быть членом роли **Подключение компьютеров, подключенных к Azure**.

* Для чтения, изменения и удаления компьютера вы являетесь членом роли **администратора ресурсов на компьютере, подключенном к Azure** . 

### <a name="azure-subscription-and-service-limits"></a>Ограничения подписки и служб Azure

Перед настройкой компьютеров с серверами с поддержкой Arc Azure проверьте Azure Resource Manager [ограничения подписки](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) и [ограничения группы ресурсов](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) , чтобы спланировать число подключенных компьютеров.

Серверы с поддержкой Arc Azure поддерживают до 5 000 экземпляров компьютеров в группе ресурсов.

### <a name="transport-layer-security-12-protocol"></a>Протокол безопасности транспортного уровня 1,2

Чтобы обеспечить безопасность данных, передаваемых в Azure, настоятельно рекомендуем настроить для компьютера использование протокола TLS версии 1.2. Более старые версии протоколов TLS/SSL оказались уязвимы. Хотя они все еще используются для обеспечения обратной совместимости, применять их **не рекомендуется**.

|Платформа или язык | Поддержка | Дополнительные сведения |
| --- | --- | --- |
|Linux | Как правило, дистрибутивы Linux для поддержки протокола TLS 1.2 используют [OpenSSL](https://www.openssl.org). | Убедитесь, что ваша версия OpenSSL поддерживается, проверив [журнал изменений OpenSSL](https://www.openssl.org/news/changelog.html).|
| Windows Server 2012 R2 и более поздних версий; | Поддерживается и включена по умолчанию. | Убедитесь, что вы все еще используете [параметры по умолчанию](/windows-server/security/tls/tls-registry-settings).|

### <a name="networking-configuration"></a>Конфигурация сети

Агент Connected Machine для Linux и Windows обменивается исходящими данными со службой Azure Arc через TCP-порт 443. Если компьютер подключается через брандмауэр или прокси-сервер для взаимодействия через Интернет, ознакомьтесь со следующими сведениями о требованиях к конфигурации сети.

> [!NOTE]
> Серверы с поддержкой Arc не поддерживают использование [шлюза log Analytics](../../azure-monitor/agents/gateway.md) в качестве прокси-сервера для агента подключенного компьютера.
>

Если исходящее подключение ограничено брандмауэром или прокси-сервером, убедитесь, что указанные ниже URL-адреса не заблокированы. Если разрешить агенту взаимодействовать со службой только диапазоны IP-адресов или имена доменов, необходимо разрешить доступ к следующим тегам службы и URL-адресам.

Теги служб:

* AzureActiveDirectory
* AzureTrafficManager.
* AzureResourceManager
* азуреарЦинфраструктуре

URL-адреса:

| Ресурс агента | Описание |
|---------|---------|
|`management.azure.com`|Azure Resource Manager|
|`login.windows.net`|Azure Active Directory|
|`login.microsoftonline.com`|Azure Active Directory|
|`dc.services.visualstudio.com`|Application Insights|
|`*.guestconfiguration.azure.com` |Гостевая конфигурация|
|`*.his.arc.azure.com`|Служба гибридной идентификации|
|`www.office.com`|Office 365|

Для предварительных версий агентов (версии 0,11 и ниже) также требуется доступ к следующим URL-адресам:

| Ресурс агента | Описание |
|---------|---------|
|`agentserviceapi.azure-automation.net`|Гостевая конфигурация|
|`*-agentservice-prod-1.azure-automation.net`|Гостевая конфигурация|

Список IP-адресов для каждого тега или региона службы см. в файле JSON [Azure IP Ranges and Service Tags – Public Cloud](https://www.microsoft.com/download/details.aspx?id=56519) (Диапазоны IP-адресов и теги служб Azure — общедоступное облако). Корпорация Майкрософт публикует еженедельные обновления, содержащие каждую службу Azure и диапазоны IP-адресов, которые она использует. Эта информация в JSON-файле является текущим списком точек во времени для диапазонов IP-адресов, соответствующих каждому тегу службы. IP-адреса могут быть изменены. Если для настройки брандмауэра требуются диапазоны IP-адресов, то для предоставления доступа ко всем службам Azure следует использовать тег службы **AzureCloud**. Не отключайте мониторинг безопасности или проверку URL-адресов, но предоставьте такие же разрешения, как для интернет-трафика.

Дополнительные сведения см. в [обзоре тегов служб](../../virtual-network/service-tags-overview.md).

### <a name="register-azure-resource-providers"></a>Регистрация поставщиков ресурсов Azure

Использование серверов с поддержкой Azure Arc зависит от следующих поставщиков ресурсов Azure в подписке:

* **Microsoft.HybridCompute**
* **Microsoft.GuestConfiguration**

Если поставщики не зарегистрированы, их можно зарегистрировать с помощью таких команд:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

Azure CLI:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

Вы также можете зарегистрировать поставщики ресурсов с помощью портала Azure, выполнив действия, описанные в [этом разделе](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

## <a name="installation-and-configuration"></a>Установка и настройка

Подключение компьютеров в гибридной среде напрямую к Azure может осуществляться разными способами в зависимости от требований. В следующей таблице описан каждый метод. Эти данные помогут вам определить, какой из методов самый подходящий для вашей организации.

> [!IMPORTANT]
> Агент подключенного компьютера не может быть установлен на виртуальной машине Azure Windows. При попытке установки обнаруживает это и выполняет откат.

| Метод | Описание |
|--------|-------------|
| Интерактивно | Вручную установите агент на одном или нескольких компьютерах, выполнив действия, описанные в статье [Краткое руководство. Подключение компьютеров к Azure с помощью Azure Arc для серверов на портале](onboard-portal.md).<br> На портале Azure можно создать сценарий и выполнить его на компьютере, чтобы автоматизировать шаги установки и настройки агента.|
| В большом масштабе | Установите и настройте агент для нескольких машин, выполнив указания в статье [Краткое руководство. Подключение компьютеров к Azure с помощью Azure Arc для серверов в PowerShell](onboard-service-principal.md).<br> Этот метод создает субъект-службу для подключения компьютеров в неинтерактивном режиме.|
| В большом масштабе | Установите и настройте агент для нескольких машин с помощью метода, описанного в [этой статье](onboard-dsc.md).<br> Этот метод предполагает использование субъект-службы для подключения компьютеров в неинтерактивном режиме с помощью службы настройки требуемого состояния (DSC) PowerShell. |

## <a name="connected-machine-agent-technical-overview"></a>Технический обзор агента подключенного компьютера

### <a name="windows-agent-installation-details"></a>Сведения об установке агента для Windows

Агент Connected Machine для Windows можно установить одним из следующих трех способов:

* Дважды щелкните файл `AzureConnectedMachineAgent.msi`.
* Вручную, запустив пакет установщика Windows `AzureConnectedMachineAgent.msi` из командной оболочки.
* Из сеанса PowerShell с помощью сценариев.

После установки агента подключенного компьютера для Windows применяются следующие изменения конфигурации на уровне системы.

* Во время установки создаются указанные ниже папки установки.

    |Папка |Описание |
    |-------|------------|
    |%ProgramFiles%\AzureConnectedMachineAgent |Путь установки по умолчанию, содержащий вспомогательные файлы агента.|
    |%ProgramData%\AzureConnectedMachineAgent |Содержит файлы конфигурации агента.|
    |%ProgramData%\AzureConnectedMachineAgent\Tokens |Содержит полученные маркеры.|
    |%ProgramData%\AzureConnectedMachineAgent\Config |Содержит файл конфигурации агента `agentconfig.json` с записями сведений о регистрации в службе.|
    |%програмфилес%\аркконнектедмачинеажент\екстенсионсервице\гк | Путь установки, содержащий файлы гостевого агента конфигурации. |
    |%ProgramData%\GuestConfig |Содержит политики (примененные) из Azure.|
    |%програмфилес%\азуреконнектедмачинеажент\екстенсионсервице\довнлоадс | Расширения загружаются из Azure и копируются здесь.|

* Во время установки агента на целевом компьютере создаются указанные ниже службы Windows.

    |Имя службы |Отображаемое имя |Имя процесса |Описание |
    |-------------|-------------|-------------|------------|
    |himds |Гибридная служба метаданных экземпляров Azure |himds |Эта служба реализует службу метаданных экземпляров Azure (IMDS) для управления подключением к Azure и удостоверением Azure подключенного компьютера.|
    |гкарксервице |Служба Arc настройки гостевой службы |gc_service |Отслеживает конфигурацию требуемого состояния компьютера.|
    |ExtensionService |Служба расширения гостевой настройки | gc_service |Устанавливает необходимые расширения, предназначенные для компьютера.|

* Во время установки агента создаются указанные ниже переменные среды.

    |Имя |Значение по умолчанию |Описание |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Существует несколько файлов журналов, доступных для устранения неполадок. Они описаны в следующей таблице.

    |Журнал |Описание |
    |----|------------|
    |%ProgramData%\AzureConnectedMachineAgent\Log\himds.log |Записывает сведения о службе агентов (ХИМДС) и взаимодействии с Azure.|
    |%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log |Содержит выходные данные команд инструмента azcmagent при использовании аргумента подробного протоколирования (-v).|
    |%ProgramData%\GuestConfig\gc_agent_logs\gc_agent.log |Записывает сведения о действиях службы DSC,<br> в частности, подключение между службой ХИМДС и политикой Azure.|
    |%ProgramData%\GuestConfig\gc_agent_logs\gc_agent_telemetry.txt |Записывает сведения о телеметрии службы DSC и подробном протоколировании.|
    |%Програмдата%\гуестконфиг\ ext_mgr_logs|Записывает сведения о компоненте агента расширения.|
    |%Програмдата%\гуестконфиг\ extension_logs\<Extension>|Записывает сведения об установленном расширении.|

* Создается локальная группа безопасности **приложений расширения гибридного агента**.

* Во время удаления агента указанные ниже артефакты не удаляются.

    * %програмдата%\азуреконнектедмачинеажент\лог
    * %ProgramData%\AzureConnectedMachineAgent и подкаталоги
    * %ProgramData%\GuestConfig

### <a name="linux-agent-installation-details"></a>Сведения об установке агента для Linux

Агент подключенного компьютера для ОС Linux предоставляется в пакете в предпочтительном формате для распространения (RPM или DEB). Он размещен в [репозитории пакетов](https://packages.microsoft.com/) Microsoft. Агент устанавливается и настраивается с помощью пакета сценариев оболочки [Install_linux_azcmagent.sh](https://aka.ms/azcmagent).

После установки агента подключенного компьютера для Linux применяются следующие изменения конфигурации на уровне системы.

* Во время установки создаются указанные ниже папки установки.

    |Папка |Описание |
    |-------|------------|
    |/var/opt/azcmagent/ |Путь установки по умолчанию, содержащий вспомогательные файлы агента.|
    |/opt/azcmagent/ |
    |/ОПТ/GC_Ext | Путь установки, содержащий файлы гостевого агента конфигурации.|
    |/opt/DSC/ |
    |/var/opt/azcmagent/tokens |Содержит полученные маркеры.|
    |/var/lib/GuestConfig |Содержит политики (примененные) из Azure.|
    |/ОПТ/GC_Ext/downloads|Расширения загружаются из Azure и копируются здесь.|

* Во время установки агента на целевом компьютере создаются указанные ниже управляющие программы.

    |Имя службы |Отображаемое имя |Имя процесса |Описание |
    |-------------|-------------|-------------|------------|
    |himdsd.service |Служба агента подключенных компьютеров Azure |himds |Эта служба реализует службу метаданных экземпляров Azure (IMDS) для управления подключением к Azure и удостоверением Azure подключенного компьютера.|
    |гкад. сервце |Служба "Дуга" GC |gc_linux_service |Отслеживает конфигурацию требуемого состояния компьютера. |
    |екстд. Service |Служба расширения |gc_linux_service | Устанавливает необходимые расширения, предназначенные для компьютера.|

* Существует несколько файлов журналов, доступных для устранения неполадок. Они описаны в следующей таблице.

    |Журнал |Описание |
    |----|------------|
    |/var/opt/azcmagent/log/himds.log |Записывает сведения о службе агентов (ХИМДС) и взаимодействии с Azure.|
    |/var/opt/azcmagent/log/azcmagent.log |Содержит выходные данные команд инструмента azcmagent при использовании аргумента подробного протоколирования (-v).|
    |/opt/logs/dsc.log |Записывает сведения о действиях службы DSC,<br> в частности о взаимодействии между службой himds и Политикой Azure.|
    |/opt/logs/dsc.telemetry.txt |Записывает сведения о телеметрии службы DSC и подробном протоколировании.|
    |/Вар/либ/гуестконфиг/ext_mgr_logs |Записывает сведения о компоненте агента расширения.|
    |/Вар/либ/гуестконфиг/extension_logs|Записывает сведения об установленном расширении.|

* Во время установки агента создаются указанные ниже переменные среды. В `/lib/systemd/system.conf.d/azcmagent.conf` задаются указанные ниже переменные.

    |Имя |Значение по умолчанию |Описание |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Во время удаления агента указанные ниже артефакты не удаляются.

    * /var/opt/azcmagent
    * /opt/logs

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы начать оценку серверов с поддержкой Arc Azure, следуйте указаниям в статье [подключение гибридных компьютеров к Azure из портал Azure](onboard-portal.md).

* Сведения об устранении неполадок можно найти в разделе [руководство по устранению неполадок подключенного компьютера](troubleshoot-agent-onboard.md).
