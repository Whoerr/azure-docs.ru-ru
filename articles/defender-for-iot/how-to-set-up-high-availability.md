---
title: Настройка высокого уровня доступности
description: Увеличьте устойчивость защитника для развертывания IoT, установив локальное устройство консоли управления с высоким уровнем доступности. Развертывания с высоким уровнем доступности гарантируют непрерывный отчет управляемых датчиков в активную локальную консоль управления.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/07/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 8b647d5e97439db4805a7346a02db3f845dc9e37
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2021
ms.locfileid: "100523895"
---
# <a name="about-high-availability"></a>О высокой доступности

Увеличьте устойчивость защитника для развертывания IoT, установив локальное устройство консоли управления с высоким уровнем доступности. Развертывания с высоким уровнем доступности гарантируют непрерывный отчет управляемых датчиков в активную локальную консоль управления.

Это развертывание реализуется с помощью пары "локальная консоль управления", которая включает в себя первичное и дополнительное устройство.

## <a name="about-primary-and-secondary-communication"></a>Сведения о первичном и дополнительном взаимодействии

Если первичная и вторичная локальные консоли управления сопряжены:

- Сертификат СЛЛ локальной консоли управления применяется для создания безопасного подключения между основным и дополнительным устройствами. СЛЛ может быть самозаверяющим сертификатом, установленным по умолчанию, или сертификатом, установленным клиентом.

- Основные данные локальной консоли управления автоматически архивируются в дополнительную локальную консоль управления каждые 10 минут. Выполняется резервное копирование локальных конфигураций консоли управления и данных устройства. Файлы и журналы ПКАП не включаются в резервную копию. Резервное копирование и восстановление Пкапс и журналов можно выполнять вручную.

- Основная установка в консоли управления дублируется на сервере-получателе; Например, системные параметры. Если эти параметры обновляются на сервере-источнике, они также будут обновлены на сервере-получателе.

- До истечения срока действия лицензии на вторичную версию ее необходимо определить как основную, чтобы обновить лицензию.

## <a name="about-failover-and-failback"></a>Сведения об отработке отказа и восстановлении размещения

Если датчик не может подключиться к основной локальной консоли управления, он автоматически подключается к базе данных-получателю. Система будет поддерживаться как на первичной, так и во вторичной реплике, если менее половины датчиков взаимодействуют с базой данных-получателем. Вторичная база данных принимается, когда с ним взаимодействует более половины датчиков. Переход с основного сервера на сервер-получатель занимает примерно три минуты. В случае отработки отказа основная локальная консоль управления зависает. В этом случае можно войти на сервер-получатель, используя те же учетные данные для входа.

Во время отработки отказа датчики продолжают взаимодействовать с основным устройством. Если более половины управляемых датчиков завершили связь с базой данных-источником, то восстанавливается первичная реплика. Следующее сообщение появляется во вторичной консоли при восстановлении базы данных-источника.

:::image type="content" source="media/how-to-set-up-high-availability/secondary-console-message.png" alt-text="Снимок экрана сообщения, отображаемого во вторичной консоли при восстановлении базы данных-источника.":::

Снова войдите в основное устройство после перенаправления.

## <a name="high-availability-setup-overview"></a>Общие сведения о настройке высокого уровня доступности

Процедуры установки и настройки выполняются в четыре основных этапа:

1. Установите основное устройство консоли управления в локальной среде. 

2. Настройте основное устройство консоли управления в локальной среде. Например, параметры архивации по расписанию, параметры виртуальной ЛС. Дополнительные сведения см. в разделе Руководство пользователя по локальной консоли управления. Все параметры автоматически применяются к дополнительному устройству после связывания.

3. Установите дополнительное устройство локальной консоли управления. Дополнительные сведения см. [в разделе об установке защитника для IOT](how-to-install-software.md).

4. Свяжите основной и дополнительный локальные управляющие консоли управления, как описано [здесь](https://infrascale.secure.force.com/pkb/articles/Support_Article/How-to-access-your-Appliance-Management-Console). Для выполнения установки основная локальная консоль управления должна управлять по крайней мере двумя датчиками.

## <a name="high-availability-requirements"></a>Требования к высокой доступности

Убедитесь, что выполнены следующие требования к высокой доступности:

- Требования к сертификатам

- Требования к аппаратному и программному обеспечению

- Требования к сетевому доступу

### <a name="software-and-hardware-requirements"></a>Требования к аппаратному и программному обеспечению

- Первичная и вторичная локальные консоли управления должны работать с идентичными моделями оборудования и версиями программного обеспечения.

- Система высокой доступности может быть настроена защитником только для пользователей IoT с помощью средств CLI.

### <a name="network-access-requirements"></a>Требования к сетевому доступу

Необходимо убедиться, что политика безопасности Организации предоставляет доступ к следующим службам в основной и дополнительной консоли управления. Эти службы также обеспечивают подключение между датчиками и дополнительной локальной консолью управления:

|Port|Служба|Описание|
|----|-------|-----------|
|**443 или TCP**|HTTPS|Предоставляет доступ к локальной консоли управления через веб-консоль.|
|**22 или TCP**|SSH|Синхронизирует данные между основным и дополнительным устройствами локальной консоли управления.|
|**123 или UDP**|NTP| Синхронизация времени NTP в локальной консоли управления. Убедитесь, что активные и пассивные устройства определены с одним часовым поясом.|

## <a name="create-the-primary-and-secondary-pair"></a>Создание первичной и вторичной пар

Перед запуском процедуры убедитесь в том, что основные и вторичные устройства консоли управления подключены.  

### <a name="on-the-primary"></a>На основном

1. Войдите в CLI как защитник для пользователя IoT.

2. Выполните следующую команду на основном:

```azurecli-interactive
sudo cyberx-management-trusted-hosts-add -ip <Secondary IP>
```

>[!NOTE]
>В этом документе основная локальная консоль управления называется первичной, а агент называется дополнительным.

3. Введите IP-адрес дополнительного устройства в ```<Secondary ip>``` поле и нажмите клавишу ВВОД. Затем IP-адрес проверяется, и сертификат SSL загружается в основной. При вводе IP-адреса датчики также связываются с дополнительным устройством.

4. Выполните следующую команду на основном компьютере, чтобы убедиться, что сертификат установлен правильно:

```azurecli-interactive
sudo cyberx-management-trusted-hosts-apply
```

5. Выполните следующую команду на основном. **Не запускайте с помощью sudo.**

```azurecli-interactive
cyberx-management-deploy-ssh-key <Secondary IP>
```

Это позволяет подключиться между основным и дополнительным устройствами для резервного копирования и восстановления между ними.

6. Введите IP-адрес получателя и нажмите клавишу ВВОД.

### <a name="on-the-secondary"></a>На сервере-получателе

1. Войдите в CLI как защитник для пользователя IoT.

2. Выполните следующую команду на сервере-получателе. **Не запускать с помощью sudo**:

```azurecli-interactive
cyberx-management-deploy-ssh-key <Primary ip>
```

Это позволяет подключиться между основным и дополнительным устройствами в целях резервного копирования и восстановления между ними.

3. Введите IP-адрес первичной реплики и нажмите клавишу ВВОД.

### <a name="track-high-availability-activity"></a>Мониторинг действий высокой доступности

Основные журналы приложений можно экспортировать в службу поддержки "защитник для Интернета вещей" для решения любых проблем с высокой доступностью.  

Для доступа к основным журналам:

1. В окне **системные параметры** выберите **Экспорт** .

## <a name="update-the-on-premises-management-console-with-high-availability"></a>Обновление локальной консоли управления с высокой доступностью

Выполните обновление высокого уровня доступности в следующем порядке. Перед началом нового шага убедитесь, что каждый шаг завершен.

Для обновления с высоким уровнем доступности выполните следующие действия.

1. Обновите основную локальную консоль управления.

2. Обновите дополнительную локальную консоль управления.

3. Обновите датчики.

## <a name="next-steps"></a>Дальнейшие шаги

[Активация и настройка локальной консоли управления](how-to-activate-and-set-up-your-on-premises-management-console.md)
