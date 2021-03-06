---
title: Устранение неполадок виртуального рабочего стола Windows удаленный рабочий стол Client — Azure
description: Сведения об устранении проблем при настройке клиентских подключений в среде клиента Виртуального рабочего стола Windows.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 08/11/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 097c97d16cf62793d03ac42662267e0553383bc1
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2021
ms.locfileid: "98539624"
---
# <a name="troubleshoot-the-remote-desktop-client"></a>Устранение неполадок клиента удаленный рабочий стол

В этой статье описаны распространенные проблемы с клиентом удаленный рабочий стол и способы их устранения.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>удаленный рабочий стол клиент для Windows 7 или Windows 10 перестает отвечать на запросы или не может быть открыт

Начиная с версии 1.2.790 можно сбрасывать данные пользователя на странице About или с помощью команды.

Используйте следующую команду, чтобы удалить данные пользователя, восстановить параметры по умолчанию и отменить подписывание для всех рабочих областей.

```cmd
msrdcw.exe /reset [/f]
```

Если вы используете более раннюю версию клиента удаленный рабочий стол, рекомендуется удалить и переустановить клиент.

## <a name="web-client-wont-open"></a>Веб-клиент не открывается

Сначала проверьте подключение к Интернету, открыв другой веб-сайт в браузере. Например, [www.Bing.com](https://www.bing.com).

Используйте **nslookup** , чтобы подтвердить, что DNS может разрешить полное доменное имя:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Попробуйте подключиться с помощью другого клиента, например удаленный рабочий стол клиента для Windows 7 или Windows 10, и проверьте, можно ли открыть веб-клиент.

### <a name="cant-open-other-websites-while-connected-to-the-web-client"></a>Не удается открыть другие веб-сайты при подключении к веб-клиенту

Если не удается открыть другие веб-сайты при подключении к веб-клиенту, возможны проблемы с сетевым подключением или сбой сети. Рекомендуется обратиться в службу поддержки сети.

### <a name="nslookup-cant-resolve-the-name"></a>Nslookup не удается разрешить имя

Если nslookup не удается разрешить имя, возможны проблемы с сетевым подключением или сбой сети. Рекомендуется обратиться в службу поддержки сети.

### <a name="your-client-cant-connect-but-other-clients-on-your-network-can-connect"></a>Клиент не может подключиться, но можно подключиться к другим клиентам в сети

Если браузер начинает работу или перестает работать при использовании веб-клиента, следуйте приведенным ниже инструкциям, чтобы устранить неполадки.

1. Перезапустите браузер.
2. Очистите файлы cookie браузера. См. раздел [Удаление файлов cookie в Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Очистите кэш браузера. См. раздел [Очистка кэша браузера для браузера](https://binged.it/2RKyfdU).
4. Откройте браузер в частном режиме.

## <a name="client-doesnt-show-my-resources"></a>Клиент не отображает мои ресурсы

Сначала проверьте учетную запись Azure Active Directory, которую вы используете. Если вы уже вошли, используя другую учетную запись Azure Active Directory, отличную от той, которую вы хотите использовать для Виртуального рабочего стола Windows, необходимо выйти из системы или использовать вкладку браузера в режиме конфиденциального просмотра.

Если вы используете Windows Virtual Desktop (классическая модель), используйте ссылку на веб-клиент в [этой статье](./virtual-desktop-fall-2019/connect-web-2019.md) для подключения к ресурсам.

Если это не сработает, убедитесь, что группа приложений связана с рабочей областью.

## <a name="web-client-stops-responding-or-disconnects"></a>Веб-клиент перестает отвечать или отключается

Попробуйте подключиться с помощью другого браузера или клиента.

### <a name="other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Другие браузеры и клиенты также неисправен или не могут открыться

Если проблемы продолжают возникать даже после переключения браузеров, проблема может быть не связана с браузером, но с вашей сетью. Рекомендуется обратиться в службу поддержки сети.

## <a name="web-client-keeps-prompting-for-credentials"></a>Веб-клиент сохраняет запросы учетных данных

Если веб-клиент сохраняет запрос на ввод учетных данных, выполните следующие инструкции:

1. Убедитесь, что URL-адрес клиента указан правильно.
2. Убедитесь, что используемые учетные данные предназначены для среды виртуальных рабочих столов Windows, привязанной к URL-адресу.
3. Очистите файлы cookie браузера. Дополнительные сведения см. в разделе [Удаление файлов cookie в Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Очистите кэш браузера. Дополнительные сведения см. [в разделе Очистка кэша браузера для браузера](https://binged.it/2RKyfdU).
5. Откройте браузер в частном режиме.

## <a name="windows-client-blocks-windows-virtual-desktop-classic-feed"></a>Клиент Windows блокирует канал виртуальных рабочих столов Windows (классическая модель)

Если веб-канал клиента Windows не отображает приложения для виртуальных рабочих столов Windows (классические), следуйте приведенным ниже инструкциям.

1. Проверьте, содержит ли политика условного доступа Идентификаторы приложений, связанные с Windows Virtual Desktop (классическая модель).
2. Проверьте, блокирует ли политика условного доступа все доступ, кроме идентификаторов приложений Windows Virtual Desktop (классические). В этом случае необходимо добавить идентификатор приложения **9cdead84-a844-4324-93f2-b2e6bb768d07** в политику, чтобы разрешить клиенту обнаруживать каналы.

Если в списке не удается найти идентификатор приложения 9cdead84-a844-4324-93f2-b2e6bb768d07, необходимо зарегистрировать поставщик ресурсов виртуальных рабочих столов Windows. Выполните приведенные ниже действия, чтобы зарегистрировать поставщика ресурсов.

1. Войдите на портал Azure.
2. Перейдите к **подписке**, а затем выберите свою подписку.
3. В меню в левой части страницы выберите **поставщик ресурсов**.
4. Найдите и выберите **Microsoft. десктопвиртуализатион**, а затем нажмите кнопку **Повторная регистрация**.

## <a name="next-steps"></a>Дальнейшие действия

- Общие сведения об устранении неполадок с Виртуальным рабочим столом Windows и о путях эскалации см. в статье [Общие сведения об устранении неисправностей, отзывы и поддержка](troubleshoot-set-up-overview.md).
- Инструкции по устранению неполадок при создании среды Виртуального рабочего стола Windows и пула узлов в среде Виртуального рабочего стола Windows см. в [этой статье](troubleshoot-set-up-issues.md).
- Инструкции по устранению неполадок при настройке виртуальной машины через Виртуальный рабочий стол Windows см. в статье [Конфигурация виртуальной машины узла сеанса](troubleshoot-vm-configuration.md).
- Сведения об устранении неполадок, связанных с агентом виртуальных рабочих столов Windows или подключением к сеансу, см. в статье [Устранение распространенных неполадок агента виртуальных рабочих столов Windows](troubleshoot-agent.md).
- Инструкции по устранению неполадок при использовании PowerShell через Виртуальный рабочий стол Windows см. в статье [Виртуальный рабочий стол Windows — PowerShell](troubleshoot-powershell.md).
- Сведения об устранении неполадок см. в статье [Tutorial: Troubleshoot Resource Manager template deployments](../azure-resource-manager/templates/template-tutorial-troubleshoot.md) (Руководство. Устранение неполадок развертывания шаблонов Resource Manager)
