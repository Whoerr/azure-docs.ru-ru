---
title: Создание устройства прозрачного шлюза с помощью Azure IoT Edge | Документация Майкрософт
description: Использование устройства Azure IoT Edge в качестве прозрачного шлюза, позволяющего обрабатывать информацию из подчиненных устройств
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/15/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 9ecb1c50fe99cc93417a37e892049e03585945a5
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100370433"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Настройка устройства IoT Edge в качестве прозрачного шлюза

В этой статье содержатся подробные инструкции по настройке устройства IoT Edge для работы в качестве прозрачного шлюза для взаимодействия других устройств с центром Интернета вещей. В этой статье используется термин *IOT Edge Gateway* для обозначения устройства IOT EDGE, настроенного в качестве прозрачного шлюза. Дополнительные сведения см. [в статье как можно использовать IOT Edge устройство в качестве шлюза](./iot-edge-as-gateway.md).

<!-- 1.1 -->
::: moniker range="iotedge-2018-06"

>[!NOTE]
>В данный момент:
>
> * устройства с поддержкой Edge не могут подключиться к шлюзам IoT Edge;
> * подчиненные устройства не поддерживают отправку файлов.

::: moniker-end

<!-- 1.2.0 -->
::: moniker range=">=iotedge-2020-11"

>[!NOTE]
>В данный момент:
>
> * подчиненные устройства не поддерживают отправку файлов.

::: moniker-end

Для настройки успешного подключения к прозрачному шлюзу необходимо выполнить три шага. В этой статье рассматривается первый шаг:

1. **Настройте устройство шлюза в качестве сервера, чтобы подчиненные устройства могли безопасно подключаться к нему. Настройте шлюз для приема сообщений от подчиненных устройств и направьте их в нужное место назначения.**
2. Создайте удостоверение устройства для подчиненного устройства, чтобы оно может проходить проверку подлинности в центре Интернета вещей. Настройте подчиненное устройство для отправки сообщений через устройство шлюза. Дополнительные сведения см. в статье [Проверка подлинности подчиненного устройства в центре Интернета вещей Azure](how-to-authenticate-downstream-device.md).
3. Подключите подчиненное устройство к устройству шлюза и начните отправлять сообщения. Эти действия см. в разделе [Подключение подчиненного устройства к шлюзу Azure IOT Edge](how-to-connect-downstream-device.md).

Чтобы устройство выработало в качестве шлюза, оно должно безопасно подключаться к своим подчиненным устройствам. Решение Azure IoT Edge позволяет настраивать безопасные подключения между устройствами с использованием инфраструктуры открытых ключей (PKI). В этом случае мы допускайте Подключение подчиненного устройства к IoT Edge устройству, выступающему в качестве прозрачного шлюза. Чтобы обеспечить приемлемую безопасность, подчиненное устройство должно подтвердить удостоверение устройства шлюза. Эта проверка удостоверения не позволяет устройствам подключаться к потенциально вредоносным шлюзам.

Роль подчиненного устройства может выполнять любое приложение или платформа с идентификатором, созданным с помощью облачной службы [Центра Интернета вещей Azure](../iot-hub/index.yml). Эти приложения часто используют [пакет SDK для устройств Azure IOT](../iot-hub/iot-hub-devguide-sdks.md). Подчиненное устройство может даже быть приложением, которое работает на IoT Edge устройстве шлюза. Однако устройство IoT Edge не может быть переIoT Edge шлюза.

Вы можете создать любую инфраструктуру сертификата, обеспечивающую доверие, необходимое для топологии "устройство — шлюз". В этой статье мы предполагаем ту же настройку сертификата, которую вы использовали для включения [безопасности ЦС x. 509](../iot-hub/iot-hub-x509ca-overview.md) в центре Интернета вещей, которая включает сертификат ЦС x. 509, связанный с конкретным центром Интернета вещей (КОРНЕВой ЦС центра Интернета вещей), ряд сертификатов, подписанных этим ЦС, и ЦС для устройства IOT Edge.

>[!NOTE]
>Термин *сертификат корневого ЦС* , используемый в этих статьях, относится к общедоступному открытому сертификату ЦЕПОЧКи PKI-сертификатов, а не к корневому центру сертификации для общедоступного центра сертификации. Во многих случаях это фактический открытый сертификат промежуточного ЦС.

Следующие шаги описывают процесс создания сертификатов и их установки в нужных местах шлюза. Можно создать сертификаты с помощью любого компьютера и затем скопировать их на устройство IoT Edge.

## <a name="prerequisites"></a>Предварительные требования

Устройство Linux или Windows с установленным IoT Edge.

Если устройство не готово, вы можете создать его на виртуальной машине Azure. Выполните действия, описанные в разделе [развертывание первого модуля IOT EDGE на виртуальном устройстве Linux](quickstart-linux.md) , чтобы создать центр Интернета вещей, создать виртуальную машину и настроить среду выполнения IOT Edge. 

## <a name="set-up-the-device-ca-certificate"></a>Настройка сертификата ЦС устройства

На всех шлюзах IoT Edge должен быть установлен сертификат ЦС устройства. Управляющая программа IoT Edge безопасности использует сертификат ЦС устройства IoT Edge для подписи сертификата ЦС рабочей нагрузки, который, в свою очередь, подписывает сертификат сервера для центра IoT Edge. Шлюз представляет сертификат сервера для подчиненного устройства во время запуска подключения. Подчиненное устройство проверяет, является ли сертификат сервера частью цепочки сертификатов, которая объединяется с сертификатом корневого ЦС. Этот процесс позволяет подчиненному устройству подтвердить, что шлюз поступает из надежного источника. Дополнительные сведения см. в статье сведения [о том, как Azure IOT Edge использует сертификаты](iot-edge-certs.md).

![Установка сертификата шлюза](./media/how-to-create-transparent-gateway/gateway-setup.png)

Сертификат корневого ЦС и сертификат ЦС устройства (с закрытым ключом) должны присутствовать на устройстве шлюза IoT Edge и быть настроены в файле конфигурации IoT Edge config. YAML. Помните, что в этом случае *сертификат корневого ЦС* означает самый верхний центр сертификации для этого IOT Edge сценария. Сертификат ЦС устройства шлюза и сертификаты подчиненных устройств должны быть сведены к одному и тому же корневому сертификату ЦС.

>[!TIP]
>Процесс установки сертификата корневого ЦС и сертификата ЦС устройства на IoT Edge устройстве также более подробно описан в разделах [Управление сертификатами на IOT Edge устройстве](how-to-manage-device-certificates.md).

Готовые к работе следующие файлы:

* Корневой сертификат ЦС
* Сертификат ЦС устройства
* Закрытый ключ ЦС устройства

В рабочих сценариях эти файлы следует создавать с помощью собственного центра сертификации. Для сценариев разработки и тестирования можно использовать демонстрационные сертификаты.

1. Если вы используете демонстрационные сертификаты, воспользуйтесь инструкциями в статье [создание демонстрационных сертификатов, чтобы протестировать IOT Edge функции устройств](how-to-create-test-certificates.md) , чтобы создать файлы. На этой странице необходимо выполнить следующие действия.

   1. Для начала настройте скрипты для создания сертификатов на устройстве.
   2. Создайте сертификат корневого ЦС. В конце этих инструкций у вас будет файл сертификата корневого ЦС:
      * `<path>/certs/azure-iot-test-only.root.ca.cert.pem`.
   3. Создание IoT Edge сертификатов ЦС устройств. В конце этих инструкций у вас будет сертификат ЦС устройства и его закрытый ключ:
      * `<path>/certs/iot-edge-device-<cert name>-full-chain.cert.pem` и
      * `<path>/private/iot-edge-device-<cert name>.key.pem`

2. Если вы создали сертификаты на другом компьютере, скопируйте их на устройство IoT Edge.

3. На устройстве IoT Edge откройте файл конфигурации управляющей программы безопасности.
   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

4. Найдите раздел **Параметры сертификата** файла. Раскомментируйте четыре строки, начинающиеся с **Certificate:** , и предоставьте URI файлов трем файлам в качестве значений для следующих свойств:
   * **device_ca_cert**: сертификат ЦС устройства
   * **device_ca_pk**: закрытый ключ ЦС устройства
   * **trusted_ca_certs**: сертификат КОРНЕВого ЦС

   Убедитесь, что в поле **Сертификаты** нет предшествующих пробелов, а остальные строки имеют отступ в два пробела.

5. Сохраните файл и закройте его.

6. Перезапустите IoT Edge.
   * Windows: `Restart-Service iotedge`
   * Linux: `sudo systemctl restart iotedge`

## <a name="deploy-edgehub-and-route-messages"></a>Развертывание edgeHub и маршрутизация сообщений

Подчиненные устройства отправляют данные телеметрии и сообщения на устройство шлюза, где модуль IoT Edge концентратора отвечает за маршрутизацию информации в другие модули или в центр Интернета вещей. Чтобы подготовить устройство шлюза для этой функции, убедитесь, что:

* Модуль IoT Edge концентратора развертывается на устройстве.

  При первой установке IoT Edge на устройстве автоматически запускается только один системный модуль: агент IoT Edge. После создания первого развертывания для устройства также запускается второй системный модуль, концентратор IoT Edge. Если модуль **edgeHub** не работает на вашем устройстве, создайте развертывание для своего устройства.

* В модуле IoT Edge концентратора настроены маршруты для обработки входящих сообщений от подчиненных устройств.

  Устройство шлюза должно иметь маршрут на месте для обработки сообщений от подчиненных устройств, иначе эти сообщения не будут обрабатываться. Сообщения можно отправить в модули на устройстве шлюза или непосредственно в центр Интернета вещей.

Чтобы развернуть модуль IoT Edge концентратора и настроить его с помощью маршрутов для обработки входящих сообщений от подчиненных устройств, выполните следующие действия.

1. Найдите нужный Центр Интернета вещей на портале Azure.

2. Перейдите к **IoT Edge** и выберите устройство IoT Edge, которое нужно использовать в качестве шлюза.

3. Выберите **задать модули**.

4. На странице **модули** можно добавить любые модули, которые требуется развернуть на устройстве шлюза. В рамках этой статьи мы посвящены настройке и развертыванию модуля edgeHub, который не нужно задавать явно на этой странице.

5. Нажмите кнопку **Далее: Маршруты**.

6. На странице **маршруты** убедитесь, что имеется маршрут для работы с сообщениями, поступающими от подчиненных устройств. Пример:

   * Маршрут, отправляющий в центр Интернета вещей все сообщения, как из модуля, так и из подчиненного устройства.
       * **Имя**: `allMessagesToHub`
       * **Значение**: `FROM /messages/* INTO $upstream`

   * Маршрут, отправляющий все сообщения со всех подчиненных устройств в центр Интернета вещей:
      * **Имя**: `allDownstreamToHub`
      * **Значение**: `FROM /messages/* WHERE NOT IS_DEFINED ($connectionModuleId) INTO $upstream`

      Этот маршрут работает, поскольку, в отличие от сообщений от IoT Edge модулей, с сообщениями от подчиненных устройств не связан идентификатор модуля. Использование предложения **WHERE** маршрута позволяет отфильтровать все сообщения с этим системным свойством.

      Дополнительные сведения о маршрутизации сообщений см. в разделе [Объявление маршрутов](./module-composition.md#declare-routes).

7. После создания маршрута или маршрутов выберите **проверить и создать**.

8. На странице **Проверка и создание** нажмите кнопку **Создать**.

## <a name="open-ports-on-gateway-device"></a>Открытие портов на устройстве шлюза

Для стандартных устройств IoT Edge не требуется выполнять какие-либо входящие подключения, так как все связи с центром Интернета вещей выполняются через исходящие подключения. Устройства шлюза отличаются, так как они должны принимать сообщения со своих нижестоящих устройств. Если брандмауэр находится между подчиненными устройствами и устройством шлюза, связь также должна быть возможна через брандмауэр.

Чтобы сценарий шлюза работал, по крайней мере один из поддерживаемых протоколов концентратора IoT Edge должен быть открыт для входящего трафика от подчиненных устройств. Поддерживаемые протоколы: MQTT, AMQP, HTTPS, MQTT через WebSockets и AMQP через WebSockets.

| Порт | Протокол |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT + WS <br> AMQP + WS |

## <a name="next-steps"></a>Следующие шаги

Теперь, когда IoT Edge устройство настроено в качестве прозрачного шлюза, необходимо настроить подчиненные устройства, чтобы они доверяли шлюзу и отправляли в него сообщения. Продолжите [проверку подлинности подчиненного устройства в центре Интернета вещей Azure](how-to-authenticate-downstream-device.md) , чтобы выполнить следующие действия в разделе Настройка сценария прозрачного шлюза.