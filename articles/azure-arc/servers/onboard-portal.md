---
title: Подключение гибридных компьютеров к Azure на портале Azure
description: Из этой статьи вы узнаете, как установить агент и подключить компьютеры к Azure с помощью серверов с поддержкой дуги Azure из портал Azure.
ms.date: 11/05/2020
ms.topic: conceptual
ms.openlocfilehash: 97962f7fd9816e398f017555d7043cf65db00ed8
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100584962"
---
# <a name="connect-hybrid-machines-to-azure-from-the-azure-portal"></a>Подключение гибридных компьютеров к Azure на портале Azure

Вы можете включить серверы с поддержкой ARC в Azure для одного или небольшого числа компьютеров Windows или Linux в вашей среде, выполнив набор действий вручную. Вы также можете использовать автоматизированный метод — запустить скрипт шаблона, который мы предоставляем. Этот скрипт автоматизирует скачивание и установку обоих агентов.

Этот метод требует наличия разрешений администратора на компьютере для установки и настройки агента. В Linux, используя корневую учетную запись, и в Windows вы являетесь членом группы локальных администраторов.

Прежде чем приступить к работе, ознакомьтесь с данными о [необходимых компонентах](agent-overview.md#prerequisites) и убедитесь, что подписка и ресурсы соответствуют требованиям. Сведения о поддерживаемых регионах и других связанных вопросах см. в разделе [Поддерживаемые регионы Azure](overview.md#supported-regions).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="generate-the-installation-script-from-the-azure-portal"></a>Создание скрипта установки на портале Azure

Скрипт для автоматизации скачивания и установки, а также для установки подключения к Azure Arc, доступен на портале Azure. Чтобы завершить процесс, выполните следующие действия:

1. В браузере перейдите на [портал Azure](https://portal.azure.com).

1. На странице **Серверы —Azure Arc** щелкните **Добавить** в левом верхнем углу.

1. На странице **Выбор метод** выберите плитку **Добавление серверов с помощью интерактивного скриптов**, а затем щелкните **Создать скрипт**.

1. На странице **Создать скрипт** выберите подписку и группу ресурсов, которые следует использовать для управления компьютером в Azure. Выберите расположение Azure, в котором будут храниться метаданные компьютера. Это расположение может совпадать или не совпадать с расположением группы ресурсов.

1. На странице **Необходимые условия** просмотрите сведения и нажмите кнопку **Далее: Сведения о ресурсе**.

1. На странице **Сведения о ресурсе** укажите следующее:

    1. В раскрывающемся списке **Группа ресурсов** выберите группу ресурсов, из которой будет осуществляться управление компьютером.
    1. В раскрывающемся списке **Регион** выберите регион Azure для хранения метаданных серверов.
    1. В раскрывающемся списке **Операционная система** выберите операционную систему, в которой будет выполняться скрипт.
    1. Если компьютер обменивается данными через прокси-сервер для подключения к Интернету, укажите IP-адрес прокси-сервера или имя и номер порта, которые будут использоваться компьютером для взаимодействия с прокси-сервером. Введите значение в формате `http://<proxyURL>:<proxyport>`.
    1. По завершении выберите **Next: Теги**.

1. На странице **Теги** проверьте **теги физического расположения**, заданные по умолчанию, а затем введите нужное значение или укажите один или несколько **настраиваемых тегов** в соответствии со своими стандартами.

1. По завершении выберите **Next: Download and run script** (Далее: скачивание и выполнение скрипта).

1. На вкладке **Download and run script** (Скачивание и выполнение скрипта) просмотрите сводные данные и щелкните **Скачать**. Если вам все еще нужно внести изменения, щелкните **Назад**.

## <a name="install-and-validate-the-agent-on-windows"></a>Установка и проверка агента в ОС Windows

### <a name="install-manually"></a>Установка вручную

Агент подключенного компьютера можно установить вручную, запустив пакет установщика Windows *AzureConnectedMachineAgent.msi*. Последнюю версию [пакета установщика Windows для агента Windows](https://aka.ms/AzureConnectedMachineAgent) можно скачать из Центра загрузки Майкрософт.

>[!NOTE]
>* Чтобы установить или удалить агент, необходимы разрешения *Администратора*.
>* Сначала необходимо скачать и скопировать пакет установщика в папку на целевом сервере или сделать это из общей сетевой папки. При запуске пакета установщика без настраивания каких-либо параметров активируется мастер установки, который можно использовать для установки агента в интерактивном режиме.

Если компьютеру необходимо взаимодействовать через прокси-сервер со службой, после установки агента необходимо выполнить команду, описанную в следующих шагах. Эта команда задает переменную системной среды прокси-сервера `https_proxy` .

Если вы не работали с параметрами командной строки для пакетов установщика Windows, ознакомьтесь со [стандартными параметрами командной строки Msiexec](/windows/win32/msi/standard-installer-command-line-options) и [параметрами командной строки Msiexec](/windows/win32/msi/command-line-options).

Например, запустите программу установки с параметром `/?`, чтобы ознакомиться с параметром справки и краткого справочника.

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /?
```

1. Чтобы автоматически установить агент и создать файл журнала установки в существующей папке `C:\Support\Logs`, выполните следующую команду.

    ```dos
    msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentsetup.log"
    ```

    Если не удается запустить агент после завершения установки, просмотрите подробные сведения об ошибке в журналах. Каталог журнала — *%програмдата%\азуреконнектедмачинеажент\лог*.

2. Если компьютеру требуется взаимодействие через прокси-сервер, чтобы задать переменную среды прокси-сервера, выполните следующую команду:

    ```powershell
    [Environment]::SetEnvironmentVariable("https_proxy", "http://{proxy-url}:{proxy-port}", "Machine")
    $env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
    # For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
    Restart-Service -Name himds
    ```

    >[!NOTE]
    >Агент не поддерживает настройку проверки подлинности прокси-сервера в этой предварительной версии.
    >

3. После установки агента необходимо настроить его для взаимодействия со службой Azure Arc, выполнив следующую команду:

    ```dos
    "%ProgramFiles%\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID"
    ```

### <a name="install-with-the-scripted-method"></a>Установка с помощью метода скрипта

1. Войдите на сервер.

1. Откройте командную строку PowerShell с повышенными привилегиями.

    >[!NOTE]
    >Сценарий поддерживает только запуск из 64-разрядной версии Windows PowerShell.
    >

1. Перейдите к обычной или общей папке, в которую скопировали скрипт, и выполните его на сервере, запустив `./OnboardingScript.ps1`.

Если не удается запустить агент после завершения установки, просмотрите подробные сведения об ошибке в журналах. Каталог журнала — *%програмдата%\азуреконнектедмачинеажент\лог*.

## <a name="install-and-validate-the-agent-on-linux"></a>Установка и проверка агента в Linux

Агент подключенного компьютера для ОС Linux предоставляется в пакете в предпочтительном формате для распространения (RPM или DEB). Он размещен в [репозитории пакетов](https://packages.microsoft.com/) Microsoft. [Пакет скриптов оболочки `Install_linux_azcmagent.sh`](https://aka.ms/azcmagent) выполняет следующие действия:

* настраивает на хост-компьютере скачивание пакета агента по адресу packages.microsoft.com.

* Устанавливает пакет поставщика гибридных ресурсов.

При необходимости можно указать в агенте сведения о прокси-сервере, включив параметр `--proxy "{proxy-url}:{proxy-port}"`.

Скрипт также содержит логику обнаружения поддерживаемых и неподдерживаемых распространений и проверяет разрешения, необходимые для установки.

В следующем примере выполняется скачивание агента и его установка:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent.
bash ~/Install_linux_azcmagent.sh
```

1. Чтобы скачать и установить агент и включить параметр `--proxy` для настройки взаимодействия агента через прокси-сервер, выполните следующие команды:

    ```bash
    # Download the installation package.
    wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

    # Install the connected machine agent.
    bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
    ```

2. После установки агента необходимо настроить его для взаимодействия со службой Azure Arc, выполнив следующую команду:

    ```bash
    azcmagent connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --cloud "cloudName"
    if [ $? = 0 ]; then echo "\033[33mTo view your onboarded server(s), navigate to https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.HybridCompute%2Fmachines\033[m"; fi
    ```

### <a name="install-with-the-scripted-method"></a>Установка с помощью метода скрипта

1. Войдите на сервер с учетной записью с корневым доступом.

1. Перейдите к обычной или общей папке, в которую скопировали скрипт, и выполните его на сервере, запустив `./OnboardingScript.sh`.

Если не удается запустить агент после завершения установки, просмотрите подробные сведения об ошибке в журналах. Каталог журнала — *var/opt/азкмажент/log*.

## <a name="verify-the-connection-with-azure-arc"></a>Проверка подключения с помощью Azure Arc

После установки агента и настройки его подключения к серверам с поддержкой Azure Arc перейдите на портал Azure, чтобы убедиться, что сервер подключен. Просмотрите свои компьютеры на [портале Azure](https://aka.ms/hybridmachineportal).

![Успешное соединение с сервером](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Дальнейшие действия

* Сведения об устранении неполадок можно найти в разделе [руководство по устранению неполадок подключенного компьютера](troubleshoot-agent-onboard.md).

* Узнайте, как управлять компьютером с помощью [Политики Azure](../../governance/policy/overview.md), например для [гостевой конфигурации](../../governance/policy/concepts/guest-configuration.md) виртуальной машины, проверять отправку отчетов с компьютера в ожидаемую рабочую область Log Analytics, включать мониторинг с помощью [Azure Monitor с виртуальными машинами](../../azure-monitor/vm/vminsights-enable-policy.md) и т. д.

* Узнайте больше об [агенте Log Analytics](../../azure-monitor/agents/log-analytics-agent.md). Агент Log Analytics для Windows и Linux необходим, если требуется получить данные мониторинга операционной системы и рабочей нагрузки, управлять ими с помощью модулей Runbook или функций автоматизации, таких как Управление обновлениями, или использовать другие службы Azure, такие как [Центр безопасности Azure](../../security-center/security-center-introduction.md).