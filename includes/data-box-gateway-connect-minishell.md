---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: c0ba6aee0e23dc0c68348c59c8ca8acdcd606435
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/27/2021
ms.locfileid: "98901155"
---
В зависимости от операционной системы клиента, процедуры удаленного подключения к устройству отличаются.

### <a name="remotely-connect-from-a-windows-client"></a>Удаленное подключение из клиента Windows

Прежде чем начать, убедитесь, что клиент Windows работает под управлением Windows PowerShell 5,0 или более поздней версии.

Выполните следующие действия, чтобы удаленно подключиться из клиента Windows.

1. Запустите сеанс Windows PowerShell от имени администратора.
2. Убедитесь, что на вашем клиенте запущена служба служба удаленного управления Windows. В командной строке введите:

    `winrm quickconfig`

3. Назначьте переменную IP-адресу устройства.

    $ip = "<device_ip>"

    Замените `<device_ip>` IP-адресом вашего устройства.

4. Чтобы добавить IP-адрес устройства в список доверенных узлов клиента, введите следующую команду:

    `Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force`

5. Запустите сеанс Windows PowerShell на устройстве:

    `Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell`

6. При появлении запроса введите пароль. Используйте тот же пароль, который используется для входа в локальный веб-интерфейс. Локальный пароль пользовательского веб-интерфейса по умолчанию — *password1*. При успешном подключении к устройству с помощью удаленной PowerShell вы увидите следующий пример выходных данных:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $ip = "10.100.10.10"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [10.100.10.10]: PS>
    ```

### <a name="remotely-connect-from-a-linux-client"></a>Удаленное подключение из клиента Linux

На клиенте Linux, который будет использоваться для подключения:

- [Установите последнюю версию PowerShell Core для Linux](/powershell/scripting/install/installing-powershell-core-on-linux?preserve-view=true&view=powershell-6) из GitHub, чтобы получить возможность удаленного взаимодействия SSH. 
- [Установите только `gss-ntlmssp` пакет из модуля NTLM](https://github.com/Microsoft/omi/blob/master/Unix/doc/setup-ntlm-omi.md). Для клиентов Ubuntu используйте следующую команду:
    - `sudo apt-get install gss-ntlmssp`

Дополнительные сведения см. в подразделах [удаленное взаимодействие PowerShell по протоколу SSH](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core?preserve-view=true&view=powershell-6).

Выполните следующие действия, чтобы удаленно подключиться из клиента NFS.

1. Чтобы открыть сеанс PowerShell, введите:

    `sudo pwsh`
 
2. Для подключения с помощью удаленного клиента введите:

    `Enter-PSSession -ComputerName $ip -Authentication Negotiate -ConfigurationName Minishell -Credential ~\EdgeUser`

    При появлении запроса введите пароль, используемый для входа на устройство.
 
> [!NOTE]
> Эта процедура не работает в macOS.