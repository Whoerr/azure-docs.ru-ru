---
title: Подключение устройства в Azure IoT Central | Документация Майкрософт
description: В этой статье описаны ключевые понятия, связанные с подключением устройства в Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 1/15/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.custom:
- amqp
- mqtt
- device-developer
ms.openlocfilehash: 4db7c9fdfd439e049ca76fec6f0e66bd4a37fffd
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101702714"
---
# <a name="get-connected-to-azure-iot-central"></a>Подключение к Azure IoT Central

*Эта статья относится к операторам и разработчикам устройств.*

В этой статье описывается, как устройства подключаются к приложению IoT Central Azure. Прежде чем устройство сможет обмениваться данными с IoT Central, оно должно:

- *Проверка подлинности*. При проверке подлинности в IoT Central приложении используется _маркер подписанного URL-адрес (SAS)_ или _сертификат X. 509_. Сертификаты X. 509 рекомендуется использовать в рабочих средах.
- *Зарегистрируйтесь*. Устройства должны быть зарегистрированы в приложении IoT Central. Вы можете просматривать зарегистрированные устройства на странице **устройства** в приложении.
- *Свяжите с шаблоном устройства*. В IoT Centralном приложении шаблоны устройств определяют пользовательский интерфейс, используемый операторами для просмотра подключенных устройств и управления ими.

IoT Central поддерживает следующие два сценария регистрации устройств:

- *Автоматическая регистрация*. Устройство регистрируется автоматически при первом подключении. Этот сценарий позволяет поставщикам вычислительной техники выполнять массовое изготовление устройств, которые могут подключаться без предварительной регистрации. Изготовитель оборудования создает подходящие учетные данные устройства и настраивает устройства в фабрике. При необходимости можно потребовать от оператора утверждения устройства перед отправкой данных. Этот сценарий требует настройки _регистрации группы_ X. 509 или SAS в приложении.
- *Регистрация вручную*. Операторы регистрируют отдельные устройства на странице " **устройства** " или [импортируют CSV-файл](howto-manage-devices.md#import-devices) для блочной регистрации устройств. В этом сценарии можно использовать регистрацию _групп_ x. 509 или SAS, а также _отдельную регистрацию_ x. 509 или SAS.

Устройства, подключающиеся к IoT Central, должны соответствовать *соглашениям Самонастраивающийся IOT*. Одно из этих соглашений заключается в том, что устройство должно отправить _идентификатор модели_ модели устройства, которую он реализует при подключении. Идентификатор модели позволяет приложению IoT Central связать устройство с правильным шаблоном устройства.

IoT Central использует [службу подготовки устройств для центра Интернета вещей Azure (DPS)](../../iot-dps/about-iot-dps.md) для управления процессом подключения. Сначала устройство подключается к конечной точке DPS для получения сведений, необходимых для подключения к приложению. На внутреннем уровне приложение IoT Central использует центр Интернета вещей для управления подключением устройства. DPS позволяет следующее:

- Использовать IoT Central для поддержки адаптации и подключения устройств в большом масштабе.
- Создавать учетные данные устройства и настраивать устройства в автономном режиме без необходимости их регистрации в пользовательском интерфейсе IoT Central.
- Использовать собственные идентификаторы устройств для регистрации устройств в IoT Central. Использование собственных идентификаторов устройств упрощает интеграцию с существующими системами операционных отделов организации.
- Использование единой процедуры подключения устройств к IoT Central.

В этой статье описаны следующие шаги подключения устройства.

- [Регистрация группы X. 509](#x509-group-enrollment)
- [Регистрация группы SAS](#sas-group-enrollment)
- [Отдельная регистрация](#individual-enrollment)
- [Регистрация устройств](#device-registration)
- [Связывание устройства с шаблоном устройства](#associate-a-device-with-a-device-template)

## <a name="x509-group-enrollment"></a>Регистрация группы X. 509

В рабочей среде рекомендуемым механизмом проверки подлинности устройств для IoT Central является использование сертификатов X. 509. Дополнительные сведения см. в статье [Device Authentication using X.509 CA Certificates](../../iot-hub/iot-hub-x509ca-overview.md) (Аутентификация устройств с помощью сертификатов ЦС X.509).

Чтобы подключить устройство с сертификатом X. 509 к приложению, выполните следующие действия.

1. Создайте *группу регистрации* , использующую тип аттестации **Certificates (X. 509)** .
1. Добавьте и проверьте промежуточный или корневой сертификат X. 509 в группе регистрации.
1. Создайте конечный сертификат из корневого или промежуточного сертификата в группе регистрации. Отправка конечного сертификата с устройства при подключении к приложению.

Дополнительные сведения см. в статье [Подключение устройств с помощью сертификатов X. 509](how-to-connect-devices-x509.md) .

### <a name="for-testing-purposes-only"></a>Только для целей тестирования

Только для тестирования можно использовать следующие служебные программы для создания корневого, промежуточного сертификата и сертификатов устройств.

- [Средства для пакета SDK для устройства подготовки устройств Azure IOT](https://github.com/Azure/azure-iot-sdk-node/blob/master/provisioning/tools/readme.md): коллекция Node.jsных инструментов, которые можно использовать для создания и проверки сертификатов и ключей X. 509.
- Если вы используете устройство DevKit, это [средство командной строки](https://aka.ms/iotcentral-docs-dicetool) создает сертификат ЦС, который можно добавить в приложение IOT Central для проверки сертификатов.
- [Управление тестовыми сертификатами ЦС для примеров и учебников](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md): коллекция скриптов PowerShell и bash:
  - Создайте цепочку сертификатов.
  - Сохраните сертификаты в виде CER – файлов, чтобы отправить их в приложение IoT Central.
  - Используйте код проверки из приложения IoT Central, чтобы создать сертификат проверки.
  - Создайте конечные сертификаты для устройств, используя идентификаторы устройств в качестве параметра для средства.

## <a name="sas-group-enrollment"></a>Регистрация группы SAS

Чтобы подключить устройство с ключом SAS устройства к приложению, выполните следующие действия.

1. Создайте *группу регистрации* , использующую тип аттестации подписанного URL **-адрес (SAS)** .
1. Скопируйте группу первичного или вторичного ключа из группы регистрации.
1. Используйте Azure CLI, чтобы создать ключ устройства из ключа группы:

    ```azurecli
    az iot central device compute-device-key --primary-key <enrollment group primary key> --device-id <device ID>
    ```

1. Используйте сгенерированный ключ устройства, когда устройство подключается к приложению IoT Central.

## <a name="individual-enrollment"></a>Отдельная регистрация

Клиенты, подключающиеся к устройствам, у которых есть свои учетные данные для проверки подлинности, используют индивидуальные регистрации. Отдельная регистрация — это запись для одного устройства, к которому разрешено подключение. Отдельные регистрации могут использовать конечные сертификаты X. 509 или маркеры SAS (от физического или виртуального доверенного платформенного модуля) в качестве механизмов аттестации. Код устройства может содержать только буквы, цифры и символ `-`. Дополнительные сведения см. в статье [Индивидуальная регистрация в DP](../../iot-dps/concepts-service.md#individual-enrollment).

> [!NOTE]
> При создании отдельной регистрации для устройства оно имеет приоритет над параметрами регистрации группы по умолчанию в приложении IoT Central.

### <a name="create-individual-enrollments"></a>Создание индивидуальных регистраций

IoT Central поддерживает следующие механизмы аттестации для индивидуальных регистраций:

- **Аттестация симметричных ключей:** Аттестация симметричных ключей — это простой подход к проверке подлинности устройства с помощью экземпляра DPS. Чтобы создать отдельную регистрацию, которая использует симметричные ключи, откройте страницу **подключения устройства** , выберите в качестве метода подключения и **подписанный URL-адрес (SAS)** **в качестве механизма** . Введите первичные и вторичные ключи в кодировке Base64 и сохраните изменения. Для подключения устройства используйте **область идентификатора**, **идентификатор устройства** и первичный или вторичный ключ.

    > [!TIP]
    > Для тестирования можно использовать **OpenSSL** для создания ключей в кодировке Base64: `openssl rand -base64 64`

- **Сертификаты X. 509:** Чтобы создать отдельную регистрацию с помощью сертификатов X. 509, откройте страницу **Подключение устройства** , в качестве метода подключения выберите вариант **отдельная регистрация** , а в качестве механизма — **Certificates (X. 509)** . Сертификаты устройств, используемые с отдельной записью регистрации, имеют требование, что для издателя и субъекта CN задано значение идентификатора устройства.

    > [!TIP]
    > Для тестирования можно использовать [средства для пакета SDK устройства подготовки устройств Azure IOT для Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools) создания самозаверяющего сертификата. `node create_test_cert.js device "mytestdevice"`

- **Аттестация доверенный платформенный модуль (TPM) (TPM):** [TPM](../../iot-dps/concepts-tpm-attestation.md) — это тип аппаратного модуля безопасности. Использование TPM является одним из наиболее безопасных способов подключения устройства. В этой статье предполагается, что вы используете дискретное встроенное по или встроенный доверенный платформенный модуль. Эмуляторы программного обеспечения хорошо подходят для создания прототипов или тестирования, но они не обеспечивают такой же уровень безопасности, как дискретные, встроенные или встроенные. Не используйте программные модули в рабочей среде. Чтобы создать отдельную регистрацию, использующую доверенный платформенный модуль, откройте страницу **Подключение устройства** , выберите вариант **отдельная регистрация** в качестве метода подключения и **доверенный платформенный модуль** в качестве механизма. Введите ключ подтверждения TPM и сохраните сведения о подключении устройства.

## <a name="device-registration"></a>Регистрация устройства

Прежде чем устройство сможет подключиться к IoT Central приложению, оно должно быть зарегистрировано в приложении:

- Устройства могут автоматически регистрироваться при первом подключении. Чтобы использовать этот параметр, необходимо использовать регистрацию групп регистрации или [групп SAS](#sas-group-enrollment)с помощью [X. 509](#x509-group-enrollment) .
- Оператор может импортировать CSV-файл для выполнения групповой регистрации списка устройств в приложении.
- Оператор может вручную зарегистрировать отдельное устройство на странице **устройства** в приложении.

IoT Central позволяет изготовителям оборудования выполнять массовую регистрацию устройств, которые могут регистрироваться автоматически. Изготовитель оборудования создает подходящие учетные данные устройства и настраивает устройства в фабрике. Когда клиент включает устройство в первый раз, он подключается к DPS, который затем автоматически подключает устройство к правильному IoT Central приложению. При необходимости можно потребовать от оператора утверждения устройства перед отправкой данных в приложение.

> [!TIP]
> На странице " **администрирование > подключение устройства** " параметр **автоматического утверждения** определяет, должен ли оператор вручную утвердить устройство, прежде чем он сможет начать отправку данных.

### <a name="automatically-register-devices-that-use-x509-certificates"></a>Автоматическая регистрация устройств, использующих сертификаты X. 509

1. Создайте конечные сертификаты для устройств, используя корневой или промежуточный сертификат, добавленный в [группу регистрации X. 509](#x509-group-enrollment). Используйте идентификаторы устройств в качестве `CNAME` в конечных сертификатах. Код устройства может содержать только буквы, цифры и символ `-`.

1. Как OEM, Flash каждое устройство имеет идентификатор устройства, созданный конечный сертификат X. 509 и значение **области идентификатора** приложения. Код устройства также должен отправить идентификатор модели модели устройства, которую он реализует.

1. При переключении на устройство оно сначала подключается к DP для получения сведений о подключении к IoT Central.

1. Устройство использует сведения из DPS для подключения к приложению IoT Central и регистрации в нем.

Приложение IoT Central использует идентификатор модели, отправленный устройством, чтобы [связать зарегистрированное устройство с шаблоном устройства](#associate-a-device-with-a-device-template).

### <a name="automatically-register-devices-that-use-sas-tokens"></a>Автоматическая регистрация устройств, использующих маркеры SAS

1. Скопируйте первичный ключ группы из группы регистрации **SAS-IOT-Devices** :

    :::image type="content" source="media/concepts-get-connected/group-primary-key.png" alt-text="Первичный ключ группы из группы регистрации SAS-IoT-Devices":::

1. Используйте `az iot central device compute-device-key` команду, чтобы создать ключи SAS устройства. Используйте групповой первичный ключ из предыдущего шага. ИДЕНТИФИКАТОР устройства может содержать буквы, цифры и `-` символ:

    ```azurecli
    az iot central device compute-device-key --primary-key <enrollment group primary key> --device-id <device ID>
    ```

1. Как OEM, Flash каждое устройство с ИДЕНТИФИКАТОРом устройства, созданный ключ SAS устройства и значение **области идентификатора** приложения. Код устройства также должен отправить идентификатор модели модели устройства, которую он реализует.

1. При переключении на устройство оно сначала подключается к DP для получения сведений о регистрации IoT Central.

1. Устройство использует сведения из DPS для подключения к приложению IoT Central и регистрации в нем.

Приложение IoT Central использует идентификатор модели, отправленный устройством, чтобы [связать зарегистрированное устройство с шаблоном устройства](#associate-a-device-with-a-device-template).

### <a name="bulk-register-devices-in-advance"></a>Предварительная регистрация устройств

Чтобы зарегистрировать большое количество устройств в IoT Central приложении, используйте CSV-файл для [импорта идентификаторов устройств и устройств](howto-manage-devices.md#import-devices).

Если устройства используют токены SAS для проверки подлинности, [экспортируйте CSV-файл из приложения IOT Central](howto-manage-devices.md#export-devices). Экспортированный CSV-файл содержит идентификаторы устройств и ключи SAS.

Если устройства используют сертификаты X. 509 для проверки подлинности, создайте конечные сертификаты X. 509 для устройств с помощью корневого или промежуточного сертификата, отправленного в группу регистрации X. 509. Используйте идентификаторы устройств, импортированные `CNAME` в качестве значений в конечных сертификатах.

Устройства должны использовать значение **области идентификатора** для приложения и отправить идентификатор модели при подключении.

> [!TIP]
> Значение **области идентификаторов** можно найти в окне **Администрирование > подключение устройства**.

### <a name="register-a-single-device-in-advance"></a>Регистрация одного устройства заранее

Этот подход удобен при экспериментах с IoT Central или тестированием устройств. На странице **устройства** выберите **+ создать** , чтобы зарегистрировать отдельное устройство. Вы можете использовать ключи SAS подключения устройства для подключения устройства к IoT Centralному приложению. Скопируйте _ключ SAS устройства_ из сведений о подключении для зарегистрированного устройства:

![Ключи SAS для отдельного устройства](./media/concepts-get-connected/single-device-sas.png)

## <a name="associate-a-device-with-a-device-template"></a>Связывание устройства с шаблоном устройства

IoT Central автоматически связывает устройство с шаблоном устройства при подключении устройства. Устройство отправляет [идентификатор модели](../../iot-pnp/iot-plug-and-play-glossary.md#model-id) при подключении. IoT Central использует идентификатор модели для идентификации шаблона устройства для конкретной модели устройства. Процесс обнаружения выполняется следующим образом.

1. Если шаблон устройства уже опубликован в IoT Central приложении, устройство связывается с шаблоном устройства.
1. Если шаблон устройства еще не опубликован в IoT Central приложении, IoT Central ищет модель устройства в [репозитории общедоступной модели](https://github.com/Azure/iot-plugandplay-models). Если IoT Central находит модель, она использует ее для создания базового шаблона устройства.
1. Если IoT Central не находит модель в репозитории общедоступной модели, устройство помечается как **несвязанное**. Оператор может создать шаблон устройства для устройства, а затем перенести несвязанное устройство в новый шаблон устройства.

На следующем снимке экрана показано, как просмотреть идентификатор модели для шаблона устройства в IoT Central. В шаблоне устройства выберите компонент, а затем щелкните **Просмотр удостоверения**:

:::image type="content" source="media/concepts-get-connected/model-id.png" alt-text="Снимок экрана, показывающий идентификатор модели в шаблоне устройства термостата.":::

[Модель термостата](https://github.com/Azure/iot-plugandplay-models/blob/main/dtmi/com/example/thermostat-1.json) можно просмотреть в репозитории общедоступной модели. Определение идентификатора модели выглядит следующим образом:

```json
"@id": "dtmi:com:example:Thermostat;1"
```

## <a name="device-status-values"></a>Значения состояния устройства

Когда реальное устройство подключается к IoT Centralному приложению, его состояние устройства изменяется следующим образом:

1. Состояние устройства **регистрируется** первыми. Это состояние означает, что устройство создано в IoT Central и имеет идентификатор устройства. Устройство регистрируется в следующих случаях:
    - На странице **устройства** будет добавлено новое реальное устройство.
    - Набор устройств добавляется с помощью **импорта** на странице **устройства** .

1. Состояние устройства изменяется на **подготовлено** , когда устройство, подключенное к IOT Centralному приложению с действительными учетными данными, завершает этап подготовки. На этом шаге устройство использует службу DPS для автоматического получения строки подключения из центра Интернета вещей, используемого приложением IoT Central. Теперь устройство может подключиться к IoT Central и начать отправку данных.

1. Оператор может блокировать устройство. Если устройство заблокировано, оно не может отправить данные в приложение IoT Central. Заблокированные устройства имеют состояние **заблокировано**. Оператор должен сбросить устройство, прежде чем оно сможет возобновить отправку данных. Когда оператор разблокирует устройство, состояние возвращается к предыдущему значению, **зарегистрированному** или **подготовленному**.

1. Если состояние устройства **ожидает утверждения**, это означает, что параметр **автоматического утверждения** отключен. Оператор должен явно утвердить устройство перед отправкой данных. Устройства, не зарегистрированные вручную на странице " **устройства** ", но подключенные с действительными учетными данными, будут иметь состояние устройства **ожидание утверждения**. Операторы могут утверждать эти устройства на странице **устройства** с помощью кнопки **утвердить** .

1. Если состояние устройства не **связано**, это означает, что устройство, подключающегося к IOT Central, не имеет связанного шаблона устройства. Эта ситуация обычно возникает в следующих случаях.

    - Набор устройств добавляется с помощью **импорта** на странице **устройства** без указания шаблона устройства.
    - Устройство было зарегистрировано вручную на странице **устройства** без указания шаблона устройства. После этого устройство подключается к действительным учетным данным.  

    Оператор может связать устройство с шаблоном устройства на странице **устройства** с помощью кнопки **Миграция** .

## <a name="best-practices"></a>Рекомендации

В этих рекомендациях показано, как реализовать устройства, чтобы воспользоваться преимуществами встроенного аварийного восстановления и автоматического масштабирования в IoT Central.

В следующем списке показан поток высокого уровня, когда устройство подключается к IoT Central:

1. Используйте службу DPS для подготовки устройства и получения строки подключения устройства.

1. Используйте строку подключения для подключения внутренней конечной точки центра Интернета вещей IoT Central. Отправка данных в приложение IoT Central и получение данных из него.

1. Если устройство получает ошибки подключения, то в зависимости от типа ошибки повторите попытку подключения или повторно выполните инициализацию устройства.

### <a name="use-dps-to-provision-the-device"></a>Использование DPS для подготовки устройства

Для подготовки устройства с помощью DPS используйте идентификатор области, учетные данные и идентификатор устройства из приложения IoT Central. Дополнительные сведения о типах учетных данных см. в разделе Регистрация [группы X. 509](#x509-group-enrollment) и [регистрация группы SAS](#sas-group-enrollment). Дополнительные сведения об идентификаторах устройств см. в разделе [Регистрация устройств](#device-registration).

При успешном выполнении служба DPS возвращает строку подключения, которую устройство может использовать для подключения к приложению IoT Central. Сведения об устранении ошибок подготовки см. в разделе [Проверка состояния подготовки устройства](troubleshoot-connection.md#check-the-provisioning-status-of-your-device).

Устройство может кэшировать строку подключения, которая будет использоваться для последующих соединений. Однако устройство должно быть подготовлено к [обработке ошибок подключения](#handle-connection-failures).

### <a name="connect-to-iot-central"></a>Подключение к IoT Central

Используйте строку подключения для подключения внутренней конечной точки центра Интернета вещей IoT Central. Подключение позволяет отправлять данные телеметрии в приложение IoT Central, синхронизировать значения свойств с приложением IoT Central и отвечать на команды, отправленные приложением IoT Central.

### <a name="handle-connection-failures"></a>Обработку ошибок соединения

В целях масштабирования или аварийного восстановления IoT Central может обновить свой базовый центр Интернета вещей. Чтобы поддерживать подключение, код устройства должен обрабатывать определенные ошибки подключения, устанавливая подключение к новой конечной точке центра Интернета вещей.

Если устройство получает одну из следующих ошибок при подключении, для получения новой строки подключения необходимо повторить шаг подготовки с помощью DPS. Эти ошибки означают, что строка подключения, используемая устройством, больше не действительна:

- Недоступная конечная точка центра Интернета вещей.
- Токен безопасности с истекшим сроком действия.
- Устройство отключено в центре Интернета вещей.

Если устройство получает следующие ошибки при подключении, для повторного подключения следует использовать стратегию отката. Эти ошибки означают, что строка подключения, используемая устройством, по-прежнему действительна, но временные условия останавливают подключение устройства:

- Устройство с заблокированным оператором.
- Из службы произошла внутренняя ошибка 500.

Дополнительные сведения о кодах ошибок устройств см. в разделе [Устранение неполадок подключения устройств](troubleshoot-connection.md).

## <a name="sdk-support"></a>Поддержка пакетов SDK

Пакеты SDK для устройств Azure предлагают самый простой способ реализации кода устройства. Доступны следующие пакеты SDK для устройств:

- [Пакет SDK Azure IoT для C](https://github.com/azure/azure-iot-sdk-c)
- [Пакет SDK Azure IoT для Python](https://github.com/azure/azure-iot-sdk-python)
- [Пакет SDK Azure IoT для Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Пакет SDK Azure IoT для Java](https://github.com/azure/azure-iot-sdk-java)
- [Пакет SDK Azure IoT для .NET](https://github.com/azure/azure-iot-sdk-csharp)

### <a name="sdk-features-and-iot-hub-connectivity"></a>Функции пакета SDK и возможность подключения Центра Интернета вещей

Для всего обмена данными устройства с Центром Интернета вещей используются следующие варианты подключения Центра Интернета вещей:

- [Передача сообщений с устройства в облако](../../iot-hub/iot-hub-devguide-messages-d2c.md)
- [Передача сообщений из облака на устройство](../../iot-hub/iot-hub-devguide-messages-c2d.md)
- [Двойники устройства](../../iot-hub/iot-hub-devguide-device-twins.md)

В следующей таблице содержатся сведения о том, каким образом возможности устройства Azure IoT Central сопоставляются с возможностями Центра Интернета вещей.

| Azure IoT Central | Центр Интернета вещей Azure |
| ----------- | ------- |
| Телеметрия | Передача сообщений с устройства в облако |
| Автономные команды | Передача сообщений из облака на устройство |
| Свойство | Сообщаемые свойства двойника устройства |
| Свойство (доступно для записи) | Требуемые и передаваемые свойства двойника устройства |
| Get-Help | Прямые методы |

### <a name="protocols"></a>Протоколы

Пакеты SDK для устройства поддерживают следующие сетевые протоколы для подключения к Центру Интернета вещей.

- MQTT
- AMQP
- HTTPS

Дополнительные сведения об этих разных протоколах и рекомендации по их выбору см. в статье [Справочник — выбор протокола связи](../../iot-hub/iot-hub-devguide-protocols.md).

Если устройство не может использовать поддерживаемые протоколы, используйте Azure IoT Edge для преобразования протокола. IoT Edge поддерживает другие пограничные сценарии для разгрузки обработки из приложения IoT Central Azure.

## <a name="security"></a>Безопасность

Все данные, передаваемые между устройствами и Azure IoT Central, зашифрованы. Центр Интернета вещей выполняет проверку подлинности каждого запроса с устройства, которое подключается к любой конечной точке Центра Интернета вещей, обращенной к устройству. Чтобы избежать обмена учетными данными по сети, для проверки подлинности устройство использует подписанные маркеры. Дополнительные сведения см. в разделе [Управление доступом к центру Интернета вещей](../../iot-hub/iot-hub-devguide-security.md).

## <a name="next-steps"></a>Дальнейшие действия

Если вы являетесь разработчиком устройства, вот некоторые из предлагаемых дальнейших действий:

- Ознакомьтесь с примером кода, в котором показано, как использовать маркеры SAS в [руководстве по созданию и подключению клиентского приложения к приложению IOT Central Azure.](tutorial-connect-device.md)
- Узнайте, как [подключать устройства с помощью сертификатов X. 509, используя пакет SDK для устройств Node.js для IOT Central приложения](how-to-connect-devices-x509.md) .
- Узнайте, как [отслеживать подключение устройств с помощью Azure CLI](./howto-monitor-devices-azure-cli.md)
- Узнайте, как [определить новый тип устройств IOT в приложении IOT Central Azure](./howto-set-up-template.md) .
- Узнайте об [устройствах Azure IOT EDGE и Azure IOT Central](./concepts-iot-edge.md)
