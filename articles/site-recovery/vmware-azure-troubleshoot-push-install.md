---
title: Устранение неполадок с принудительной установкой службы Mobility Service с Azure Site Recovery
description: Устранение ошибок установки служб Mobility Service при включении репликации для аварийного восстановления с Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 04/03/2020
ms.openlocfilehash: 62c8240a4d2e50aa3b584f322baf7d2ee217c6d3
ms.sourcegitcommit: 02b1179dff399c1aa3210b5b73bf805791d45ca2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98127878"
---
# <a name="troubleshoot-mobility-service-push-installation"></a>Устранение неполадок при принудительной установке службы Mobility Service

Установка службы Mobility Service является ключевым шагом для включения репликации. Успешность этого шага зависит от предварительных требований и работы с поддерживаемыми конфигурациями. Ниже перечислены наиболее распространенные сбои, которые могут возникнуть во время установки службы Mobility Service.

* [ошибки учетных данных или привилегий](#credentials-check-errorid-95107--95108);
* [Неудачные попытки входа.](#login-failures-errorid-95519-95520-95521-95522)
* [Ошибки подключения](#connectivity-failure-errorid-95117--97118).
* [ошибки совместного доступа к файлам и принтерам](#file-and-printer-sharing-services-check-errorid-95105--95106);
* [Сбои инструментарий управления Windows (WMI) (WMI)](#windows-management-instrumentation-wmi-configuration-check-error-code-95103)
* [Неподдерживаемые операционные системы](#unsupported-operating-systems)
* [Неподдерживаемые конфигурации загрузки](#unsupported-boot-disk-configurations-errorid-95309-95310-95311)
* [Сбои при установке службы теневого копирования томов (VSS)](#vss-installation-failures)
* [в конфигурации GRUB указано имя устройства вместо его UUID](#enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320);
* [Том диспетчера логических томов (LVM)](#lvm-support-from-920-version)
* [предупреждения о необходимости перезагрузки](#install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266).

При включении репликации Azure Site Recovery пытается установить агент службы Mobility Service на виртуальной машине (ВМ). В рамках этого процесса сервер конфигурации пытается подключиться к виртуальной машине и скопировать агент. Чтобы включить успешную установку, выполните пошаговое руководство по устранению неполадок.

## <a name="credentials-check-errorid-95107--95108"></a>Проверка учетных данных (код ошибки: 95107 и 95108)

Убедитесь, что учетная запись пользователя, выбранная во время включения репликации, является допустимой и точной. Для выполнения принудительной установки Azure Site Recovery требуется учетная запись **root** или пользователя с **правами администратора** . В противном случае принудительная установка будет заблокирована на исходном компьютере.

Для Windows (**ошибка 95107**) убедитесь, что учетная запись пользователя имеет административный доступ на исходном компьютере с локальной учетной записью или учетной записью домена. Если учетная запись домена не используется, необходимо отключить управление удаленным доступом пользователей на локальном компьютере.

* Чтобы вручную добавить раздел реестра, который отключает управление удаленным доступом пользователей:

  * `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`
  * Добавить новый `DWORD` : `LocalAccountTokenFilterPolicy`
  * Установите значение `1`

* Чтобы добавить раздел реестра, выполните следующую команду в командной строке:

  `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`

Для Linux (**ошибка 95108**) необходимо выбрать учетную запись **root** для успешной установки агента службы Mobility Service. Кроме того, должны быть запущена службы SSH протокол FTP (SFTP). Чтобы включить подсистему SFTP и проверку пароля в файле _sshd_config_ , выполните следующие действия.

1. Войдите в качестве **привилегированного** пользователя.
1. Перейдите в _файл каталог/etc/ssh/sshd_config_, найдите строку, которая начинается с `PasswordAuthentication` .
1. Раскомментируйте строку и измените значение на `yes` .
1. Найдите строку, которая начинается с `Subsystem` , и раскомментируйте строку.
1. Перезапустите службу `sshd` .

Если вы хотите изменить учетные данные выбранной учетной записи пользователя, выполните следующие [инструкции](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95517"></a>Сбой недостаточных прав доступа (код ошибки: 95517)

Когда пользователь, выбранный для установки агента Mobility Service, не имеет прав администратора, серверу конфигурации или серверу обработки масштабирования не будет разрешено копировать программное обеспечение агента Mobility Service на исходный компьютер. Эта ошибка является следствием отказа в доступе. Убедитесь, что учетной записи пользователя назначены права администратора.

Если вы хотите изменить учетные данные выбранной учетной записи пользователя, выполните следующие [инструкции](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95518"></a>Сбой недостаточных прав доступа (код ошибки: 95518)

При сбое доверительных отношений между основным доменом и рабочей станцией при попытке входа на исходный компьютер происходит сбой установки агента мобильности с кодом ошибки 95518. Убедитесь, что учетная запись пользователя, используемая для установки агента мобильности, имеет права администратора для входа через основной домен исходного компьютера.

Если вы хотите изменить учетные данные выбранной учетной записи пользователя, следуйте [этим инструкциям](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="login-failures-errorid-95519-95520-95521-95522"></a>Ошибки входа (код ошибки: 95519, 95520, 95521, 95522)

В этом разделе описываются учетные данные и сообщения об ошибках входа.

### <a name="credentials-of-the-user-account-have-been-disabled-errorid-95519"></a>Учетные данные учетной записи пользователя отключены (код ошибки: 95519)

Учетная запись пользователя, выбранная во время включения репликации, отключена. Чтобы включить учетную запись пользователя, обратитесь к [этой статье](/powershell/module/Microsoft.PowerShell.LocalAccounts/Enable-LocalUser) или выполните следующую команду, заменив _текстовое имя пользователя на_ фактическое.

`net user 'username' /active:yes`

### <a name="credentials-locked-out-because-of-multiple-failed-login-attempts-errorid-95520"></a>Учетные данные заблокированы из-за нескольких неудачных попыток входа (код ошибки: 95520)

Несколько неудачных повторных попыток при получении доступа к компьютеру приведут к блокировке учетной записи пользователя. Сбой может быть связан со следующими причинами:

* Учетные данные, указанные во время настройки конфигурации, неверны.
* Учетная запись пользователя, выбранная во время включения репликации, неверна.

Измените выбранные учетные данные, выполнив [эти инструкции](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) , и повторите операцию.

### <a name="logon-servers-arent-available-on-the-source-machine-errorid-95521"></a>Серверы входа недоступны на исходном компьютере (код ошибки: 95521)

Эта ошибка возникает, когда серверы входа недоступны на исходном компьютере. Если серверы входа недоступны, запросы на вход завершатся сбоем, и агент мобильности не может быть установлен. Для успешного входа убедитесь, что серверы входа доступны на исходном компьютере, и запустите службу Netlogon. Дополнительные сведения см. в статье [сценарии входа в Windows](/windows-server/security/windows-authentication/windows-logon-scenarios).

### <a name="logon-service-isnt-running-on-the-source-machine-errorid-95522"></a>Служба входа не запущена на исходном компьютере (код ошибки: 95522)

Служба входа не запущена на исходном компьютере и привела к сбою запроса на вход. Не удается установить агент мобильности. Чтобы устранить эту ошибку, используйте один из следующих методов запуска `Netlogon` службы на исходном компьютере:

* Чтобы запустить `Netlogon` службу из командной строки, выполните команду `net start Netlogon` .
* В диспетчере задач запустите `Netlogon` службу.

## <a name="connectivity-failure-errorid-95117--97118"></a>Сбой подключения (код ошибки: 95117 & 97118)

Сервер конфигурации и сервер обработки масштабирования пытаются подключиться к исходной виртуальной машине для установки агента Mobility Service. Эта ошибка возникает, когда исходный компьютер недоступен из-за проблем с сетевым подключением.

Чтобы устранить эту ошибку, сделайте следующее:

* Убедитесь, что учетная запись пользователя имеет административный доступ на исходном компьютере с локальной учетной записью или учетной записью домена. Если учетная запись домена не используется, необходимо отключить управление удаленным доступом пользователей на локальном компьютере.
  * Чтобы вручную добавить раздел реестра, который отключает управление удаленным доступом пользователей:
    * `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`
    * Добавить новый `DWORD` : `LocalAccountTokenFilterPolicy`
    * Установите значение `1`
  * Чтобы добавить раздел реестра, выполните следующую команду в командной строке:

    `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`

* Убедитесь, что вы можете проверить связь с исходным компьютером с сервера конфигурации. Если сервер обработки масштабирования был выбран во время включения репликации, убедитесь, что вы можете проверить связь с исходным компьютером с сервера обработки.

* Убедитесь, что на виртуальной машине включена служба общего доступа к файлам и принтерам. Проверьте действия, описанные [здесь](vmware-azure-troubleshoot-push-install.md#file-and-printer-sharing-services-check-errorid-95105--95106).

* Убедитесь, что на виртуальной машине включена служба WMI. Проверьте действия, описанные [здесь](vmware-azure-troubleshoot-push-install.md#windows-management-instrumentation-wmi-configuration-check-error-code-95103).

* Убедитесь, что общие сетевые папки на виртуальной машине доступны с сервера обработки. Проверьте действия, описанные [здесь](vmware-azure-troubleshoot-push-install.md#check-access-for-network-shared-folders-on-source-machine-errorid-9510595523).

* В командной строке компьютера исходного сервера используйте команду, `Telnet` чтобы проверить связь с сервером конфигурации или сервером обработки масштабирования по HTTPS-порту 135, как показано в следующей команде. Эта команда проверяет наличие проблем с сетевым подключением или блокирования портов брандмауэра.

  `telnet <CS/ scale-out PS IP address> <135>`

* Кроме того, для виртуальной машины Linux:
  * Проверьте, установлены ли последние пакеты OpenSSH, сервера OpenSSH и OpenSSL.
  * Убедитесь, что протокол Secure Shell (SSH) включен и работает через порт 22.
  * SFTP-службы должны работать. Чтобы включить подсистему SFTP и проверку пароля в файле _sshd_config_ , выполните следующие действия.

    1. Войдите в качестве **привилегированного** пользователя.
    1. Перейдите в файл _каталог/etc/ssh/sshd_config_ , найдите строку, которая начинается с `PasswordAuthentication` .
    1. Раскомментируйте строку и измените значение на `yes` .
    1. Найти строку, которая начинается с `Subsystem` , и раскомментировать строку
    1. Перезапустите службу `sshd` .

* Попытка подключения может быть неудачной, если по истечении определенного периода времени нет соответствующих ответов, или не удалось установить соединение из-за неудачного ответа подключенного узла.
* Возможно, это связано с подключением, сетью или проблемой, связанной с доменом. Это также может быть связано с устранением неполадок DNS-имен или нехваткой портов TCP. Проверьте, существуют ли такие проблемы в вашем домене.

## <a name="connectivity-failure-errorid-95523"></a>Сбой подключения (код ошибки: 95523)

Эта ошибка возникает, если сеть, в которой находится исходный компьютер, не найдена, может быть удалена или больше недоступна. Единственный способ устранить эту ошибку — убедиться, что сеть существует.

## <a name="check-access-for-network-shared-folders-on-source-machine-errorid-9510595523"></a>Проверка доступа для сетевых общих папок на исходном компьютере (код ошибки: 95105, 95523)

Убедитесь, что сетевые общие папки на виртуальной машине доступны удаленно с сервера обработки (PS) с использованием указанных учетных данных. Чтобы подтвердить доступ: 

1. Войдите на компьютер сервера обработки.
2. Откройте проводник. В адресной строке введите `\\<SOURCE-MACHINE-IP>\C$` и нажмите клавишу ВВОД.

    ![Открыть папку в PS](./media/vmware-azure-troubleshoot-push-install/open-folder-process-server.PNG)

3. Обозреватель файлов запросит учетные данные. Введите имя пользователя и пароль и нажмите кнопку ОК. <br><br/>

    ![Укажите учетные данные](./media/vmware-azure-troubleshoot-push-install/provide-credentials.PNG)

    >[!NOTE]
    > Если исходный компьютер присоединен к домену, укажите имя домена вместе с именем пользователя в виде `<domainName>\<username>` . Если исходный компьютер находится в Рабочей группе, укажите только имя пользователя.

4. В случае успешного подключения папки исходного компьютера будут видимы удаленно с сервера обработки.

    ![Видимые папки с исходного компьютера](./media/vmware-azure-troubleshoot-push-install/visible-folders-from-source.png)

Если подключение завершилось сбоем, проверьте, соблюдены ли все предварительные требования.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Проверка служб совместного доступа к файлам и принтерам (код ошибки: 95105 и 95106)

После проверки подключения проверьте, включена ли на виртуальной машине служба общего доступа к файлам и принтерам. Эти параметры необходимы для копирования агента мобильности на исходный компьютер.

Для **Windows 2008 R2 и более ранних версий**:

* Включение общего доступа к файлам и принтерам через брандмауэр Windows:
  1. Откройте **Панель управления**  >  **система и**  >  **Брандмауэр Windows** безопасность. В левой области выберите **Дополнительные параметры**  >  **правила для входящих подключений** в дереве консоли.
  1. Найдите правила "Общий доступ к файлам и принтерам (NB-Session-In)" и "Общий доступ к файлам и принтерам (SMB-In)".
  1. Щелкните правой кнопкой мыши на каждом правиле и нажмите **Включить правило**.

* Включение общего доступа к файлам с помощью групповая политика:
  1. Перейдите в раздел **Пуск**, введите `gpmc.msc` и выполните поиск.
  1. В области навигации откройте следующие папки: **Локальная политика компьютера**  >  **Конфигурация пользователя**  >  **Административные шаблоны**  >  **компоненты Windows**  >  **общий доступ к сети**.
  1. В области сведений дважды щелкните **Запретить пользователям представлять общий доступ к файлам в профиле**.

     Чтобы отключить параметр групповая политика и разрешить пользователю предоставлять общий доступ к файлам, выберите значение **отключено**.

  1. Нажмите кнопку **OK**, чтобы сохранить изменения.

  Дополнительные сведения см. в разделе [Включение или отключение общего доступа к файлам с помощью групповая политика](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

Чтобы включить общий доступ к файлам и принтерам для более поздних версий Windows или Linux, следуйте инструкциям в [статье Установка службы Mobility Service для аварийного восстановления виртуальных машин VMware и физических серверов](vmware-azure-install-mobility-service.md) .

## <a name="windows-management-instrumentation-wmi-configuration-check-error-code-95103"></a>Проверка конфигурации инструментарий управления Windows (WMI) (WMI) (код ошибки: 95103)

После проверки служб файлов и принтеров включите службу WMI для частных, открытых и доменных профилей в брандмауэре. Эти параметры необходимы для запуска удаленного выполнения на исходном компьютере.

Чтобы включить Инструментарий WMI, выполните следующие действия.

1. Перейдите в раздел Безопасность **панели управления**  >   и выберите **Брандмауэр Windows**.
1. Выберите **изменить параметры** , а затем перейдите на вкладку **исключения** .
1. В окне **исключения** установите флажок для Инструментарий управления Windows (WMI) (WMI), чтобы включить трафик WMI через брандмауэр.

Можно также включить трафик WMI через брандмауэр из командной строки с помощью следующей команды:

`netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`

Дополнительные сведения об устранении неполадок WMI:

* [Базовое тестирование WMI](https://techcommunity.microsoft.com/t5/ask-the-performance-team/bg-p/AskPerf)
* [Устранение неполадок WMI](/windows/win32/wmisdk/wmi-troubleshooting)
* [Устранение неполадок со сценариями WMI и службами WMI](/previous-versions/tn-archive/ff406382(v=msdn.10))

## <a name="unsupported-operating-systems"></a>Неподдерживаемые операционные системы

Другой распространенной причиной сбоя может быть неподдерживаемая операционная система. Используйте поддерживаемую операционную систему и версию ядра для успешной установки службы Mobility Service. Избегайте использования частных исправлений.

Список операционных систем и версий ядра, поддерживаемых Azure Site Recovery, см. в [документе Матрица поддержки](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="unsupported-boot-disk-configurations-errorid-95309-95310-95311"></a>Неподдерживаемые конфигурации загрузочного диска (код ошибки: 95309, 95310, 95311)

### <a name="boot-and-system-partitions--volumes-arent-the-same-disk-errorid-95309"></a>Загрузочные и системные разделы или тома не являются одним и тем же диском (код ошибки: 95309)

До версии 9,20 загрузочные и системные разделы или тома на разных дисках были неподдерживаемой конфигурацией. Начиная с [версии 9,20](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)эта конфигурация поддерживается.

### <a name="the-boot-disk-isnt-available-errorid-95310"></a>Загрузочный диск недоступен (код ошибки: 95310)

Невозможно защитить виртуальную машину без загрузочного диска. Загрузочный диск обеспечивает плавное восстановление виртуальной машины во время отработки отказа. Отсутствие загрузочного диска приводит к сбою при загрузке компьютера после отработки отказа. Убедитесь, что виртуальная машина содержит загрузочный диск, и повторите операцию. Кроме того, не поддерживается несколько загрузочных дисков на одном компьютере.

### <a name="multiple-boot-disks-present-on-the-source-machine-errorid-95311"></a>На исходном компьютере имеется несколько загрузочных дисков (код ошибки: 95311)

Виртуальная машина с несколькими загрузочными дисками не является [поддерживаемой конфигурацией](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage).

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>Системный раздел на нескольких дисках (код ошибки: 95313)

До версии 9,20 Установка корневого раздела или тома на нескольких дисках была неподдерживаемой конфигурацией. Начиная с [версии 9,20](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)эта конфигурация поддерживается.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320"></a>Не удалось включить защиту, так как имя устройства упоминается в конфигурации GRUB, а не UUID (код ошибки: 95320).

### <a name="possible-cause"></a>Возможные причины

Файлы конфигурации общего единого загрузчика (GRUB) (_/Boot/GRUB/menu.lst_, _/Бут/груб/груб.КФГ_, _/Boot/grub2/GRUB.cfg_ или _/etc/default/grub_) могут содержать значение для **корневого каталога** параметров и **возобновлять** его как фактические имена устройств вместо универсального уникального идентификатора (UUID). Site Recovery требует подход UUID, так как имена устройств могут изменяться в процессе перезагрузки виртуальной машины. Например, виртуальная машина может не перейти в режим «в сети» с тем же именем при отработке отказа, что приводит к проблемам.

Пример:

- Следующая строка находится в файле GRUB _/Boot/grub2/GRUB.cfg_:

  `linux /boot/vmlinuz-3.12.49-11-default root=/dev/sda2  ${extra_cmdline} resume=/dev/sda1 splash=silent quiet showopts`

- Следующая строка находится в файле GRUB _/Boot/GRUB/menu.lst_:

  `kernel /boot/vmlinuz-3.0.101-63-default root=/dev/sda2 resume=/dev/sda1 splash=silent crashkernel=256M-:128M showopts vga=0x314`

> [!NOTE]
> Строки GRUB содержат реальные имена устройств для **корневого** параметра и **возобновления** , а не UUID.

### <a name="how-to-fix"></a>Как исправить

Имена устройств должны быть заменены на соответствующий UUID.

1. Найдите UUID устройства, выполнив команду `blkid \<device name>` .

   Пример:

   ```shell
   blkid /dev/sda1
   /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
   blkid /dev/sda2
   /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```

1. Теперь замените имя устройства UUID в формате, например `root=UUID=\<UUID>` . Например, если мы заменяем имена устройств на UUID для параметра root и Resume, упомянутые в файлах _/Boot/grub2/GRUB.cfg_, _/Boot/grub2/GRUB.cfg_ или _/etc/default/grub_ , строки в файлах будут выглядеть так, как показано в следующей строке:

   `kernel /boot/vmlinuz-3.0.101-63-default root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4 resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b splash=silent crashkernel=256M-:128M showopts vga=0x314`

1. Перезапустите защиту.

## <a name="install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266"></a>Установка службы Mobility Service завершена с предупреждением о перезагрузке (код ошибки: 95265 & 95266)

Site Recovery служба Mobility Service имеет много компонентов, одна из которых называется драйвером фильтра. Драйвер фильтра загружается в системную память только во время перезагрузки системы. Исправления для драйверов фильтров можно реализовать только при загрузке нового драйвера фильтра во время перезагрузки системы.

> [!IMPORTANT]
> Это предупреждение, и существующая репликация будет работать даже после нового обновления агента. Вы можете перезагружать компьютер в любое время, чтобы получить преимущества нового драйвера фильтра, но если не выполнить перезагрузку, старый драйвер фильтра продолжит работать. Таким образом, после обновления без перезагрузки, за исключением драйвера фильтра, **преимущества других усовершенствований и исправлений в службе Mobility Service реализованы**. Рекомендуется не обязательно перезагружать компьютер после каждого обновления. Чтобы получить сведения о том, когда требуется перезагрузка, в разделе обновления службы в Azure Site Recovery Установите раздел [перезагрузить после обновления службы Mobility](service-updates-how-to.md#reboot-after-mobility-service-upgrade) Service.

> [!TIP]
>Рекомендации по планированию обновлений в течение периода обслуживания см. в статье [Поддержка последней версии операционной системы или ядра](service-updates-how-to.md#support-for-latest-operating-systemskernels) в обновлениях служб в Azure Site Recovery.

## <a name="lvm-support-from-920-version"></a>Поддержка диспетчера логических томов (LVM) начиная с версии 9.20

До версии 9,20 Диспетчер логических томов (LVM) поддерживался только для дисков данных. `/boot`Раздел должен находиться в разделе диска, а не в LVM томе.

Начиная с [версии 9,20](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery), поддерживается [диск операционной системы на LVM](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) .

## <a name="insufficient-space-errorid-95524"></a>Недостаточно места (код ошибки: 95524)

При копировании агента мобильности на исходный компьютер требуется не менее 100 МБ свободного места. Убедитесь, что на исходном компьютере имеется необходимый объем свободного пространства, и повторите операцию.

## <a name="low-system-resources"></a>Недостаточно системных ресурсов

Возможные идентификаторы ошибок для этой проблемы: 95572 и 95573. Эта проблема возникает, когда в системе недостаточно свободной памяти и не удается выделить память для установки службы Mobility Service. Убедитесь, что была освобождена достаточная память для продолжения установки и успешного завершения.

## <a name="vss-installation-failures"></a>Ошибки установки VSS

Установка службы теневого копирования томов (VSS) является частью установки агента мобильности. Эта служба используется в процессе создания точек восстановления, совместимых с приложениями. Ошибки во время установки VSS могут возникнуть по нескольким причинам. Чтобы найти точные ошибки, обратитесь к _к:\програмдата\асрсетуплогс\асрунифиедажентинсталлер.лог_. Некоторые распространенные ошибки и действия по их устранению выделены в следующем разделе.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>Ошибка VSS 2147023170 [0x800706BE] — код выхода 511

Чаще всего эта проблема возникает, когда антивирусная программа блокирует операции служб Azure Site Recovery.

Для разрешения этой проблемы:

1. Проверьте список [исключений папок из антивирусной программы](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).
1. Следуйте рекомендациям, опубликованным поставщиком антивирусных программ, чтобы разблокировать регистрацию библиотеки DLL в Windows.

### <a name="vss-error-7-0x7---exit-code-511"></a>Ошибка VSS 7 [0x7] — код выхода 511

Это ошибка времени выполнения, вызванная тем, что недостаточно памяти для установки VSS. Увеличьте место на диске для успешного завершения этой операции.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>Ошибка VSS 2147023824 [0x80070430] — код выхода 517

Эта ошибка возникает, когда служба поставщика VSS Azure Site Recovery [помечена для удаления](/previous-versions/ms838153(v=msdn.10)). Попробуйте установить VSS вручную на исходном компьютере, выполнив следующую команду:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>Ошибка VSS 2147023841 [0x8007041F] — код выхода 512

Эта ошибка возникает, если база данных службы поставщика VSS Azure Site Recovery [заблокирована](/previous-versions/ms833798(v=msdn.10)). Попробуйте установить VSS вручную на исходном компьютере, выполнив следующую команду из командной строки:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

В случае сбоя проверьте, не зависает ли антивирусная программа или другие службы в **начальном** состоянии. Процесс в **начальном** состоянии может сохранить блокировку служб базы данных. Это приводит к сбоям при установке поставщика VSS. Убедитесь, что служба не находится в **начальном** состоянии, и повторите операцию.

### <a name="vss-exit-code-806"></a>Код выхода VSS 806

Эта ошибка возникает, когда учетная запись пользователя, используемая для установки, не имеет разрешений на выполнение `CSScript` команды. Предоставьте учетной записи пользователя необходимые разрешения на выполнение скрипта и повторите операцию.

### <a name="other-vss-errors"></a>Другие ошибки VSS

Попробуйте установить службу поставщика VSS вручную на исходном компьютере, выполнив следующую команду из командной строки:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

## <a name="vss-error---0x8004e00f"></a>Ошибка VSS — 0x8004E00F

Эта ошибка обычно возникает во время установки агента Mobility Service из-за проблем в `DCOM` и `DCOM` находится в критическом состоянии.

Чтобы определить причину ошибки, используйте следующую процедуру.

### <a name="examine-the-installation-logs"></a>Изучите журналы установки

1. Откройте журнал установки, расположенный по адресу _к:\програмдата\асрсетуплогс\асрунифиедажентинсталлер.лог_.
2. Эта проблема обозначается наличием следующей ошибки:

    ```Output
    Unregistering the existing application...
    Create the catalogue object
    Get the collection of Applications

    ERROR:

    - Error code: -2147164145 [0x8004E00F]
    - Exit code: 802
    ```

Чтобы решить эту проблему, выполните указанные ниже действия.

Обратитесь к [группе разработчиков платформы Microsoft Windows](https://aka.ms/Windows_Support) , чтобы получить помощь по устранению проблемы DCOM.

После устранения проблемы DCOM переустановите Azure Site Recovery поставщика VSS вручную с помощью следующей команды из командной строки:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

Если согласованность приложений не является критической для требований к аварийному восстановлению, можно обойти установку поставщика VSS.

Чтобы обойти установку поставщика VSS Azure Site Recovery и вручную установить Azure Site Recovery поставщика VSS после установки:

1. Установите службу Mobility Service. Установка завершится ошибкой на шаге: **Настройка после установки**.
1. Чтобы обойти установку VSS, выполните следующие действия.
   1. Откройте каталог установки Azure Site Recovery Mobility Service, расположенный по адресу:

      _C:\Program Files (x86) \Microsoft Azure site Рековери\ажент_

   1. Измените сценарии установки поставщика VSS Azure Site Recovery _InMageVSSProvider_Install_ и _InMageVSSProvider_Uninstall. cmd_ , чтобы они всегда были выполнены, добавив следующие строки:

      ```plaintext
      rem @echo off
      setlocal
      exit /B 0
      ```

1. Установка агента мобильности вручную.
1. После завершения установки и перехода к следующему шагу **Настройте**, а затем удалите добавленные строки.
1. Чтобы установить поставщик VSS, откройте командную строку от имени администратора и выполните следующую команду:

   `"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

1. Убедитесь, что поставщик Azure Site Recovery VSS установлен как служба в службах Windows. Откройте консоль MMC службы компонентов, чтобы убедиться в том, что поставщик VSS указан.
1. Если установка поставщика VSS завершается неудачно, обратитесь в службу технической поддержки, чтобы устранить ошибки разрешений в криптографическим интерфейсе прикладного программирования (CAPI2).

## <a name="vss-provider-installation-fails-because-the-cluster-service-being-enabled-on-non-cluster-machine"></a>Установка поставщика VSS завершается сбоем, так как служба кластеров включена на компьютере, не являющемся кластером

Эта проблема приводит к сбою установки агента мобильности Azure Site Recovery во время установки поставщика Azure Site Recovery VSS. Сбой заключается в том `COM+` , что существует проблема, которая предотвращает установку поставщика VSS.

### <a name="to-identify-the-issue"></a>Для выяснения проблемы

В журнале, расположенном на сервере конфигурации в _к:\програмдата\асрсетуплогс\уплоадедлогс \<date-time> UA_InstallLogFile. log_ , можно найти следующее исключение:

```plaintext
COM+ was unable to talk to the Microsoft Distributed Transaction Coordinator (Exception from HRESULT: 0x8004E00F)
```

Чтобы решить эту проблему, выполните указанные ниже действия.

1. Убедитесь, что этот компьютер не является компьютером кластера и что компоненты кластера не используются.
1. Если компоненты не используются, удалите компоненты кластера с компьютера.

## <a name="drivers-are-missing-on-the-source-server"></a>На исходном сервере отсутствуют драйверы

Если установка агента мобильности завершается сбоем, проверьте журналы в разделе _к:\програмдата\асрсетуплогс_ , чтобы определить, отсутствуют ли в некоторых наборах элементов необходимые драйверы.

Чтобы решить эту проблему, выполните указанные ниже действия.

1. С помощью редактора реестра, например `regedit.msc` , откройте реестр.
1. Откройте `HKEY_LOCAL_MACHINE\SYSTEM` узел.
1. В `SYSTEM` узле выберите наборы элементов управления.
1. Откройте каждый набор элементов управления и убедитесь, что имеются следующие драйверы Windows:

   * Драйвер
   * VMBus
   * Storflt
   * Storvsc
   * Intelide

1. Переустановите все отсутствующие драйверы.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные [сведения](vmware-azure-tutorial.md) о настройке аварийного восстановления для виртуальных машин VMware.
