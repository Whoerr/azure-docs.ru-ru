---
title: Об интеграции Splunk
titleSuffix: Azure Defender for IoT
description: Чтобы устранить отсутствие видимости в области безопасности и устойчивости сетей, защитник для IoT разработал защитник для приложений для мониторинга угроз IoT, промышленному и ICS для Splunk, собственную интеграцию между защитником для Интернета вещей и Splunk, которая обеспечивает единый подход к ИТ и безопасности.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/4/2021
ms.topic: article
ms.service: azure
ms.openlocfilehash: 91d877d644b4b5ca7231f5f81f9163a0fd3cbe25
ms.sourcegitcommit: 6628bce68a5a99f451417a115be4b21d49878bb2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2021
ms.locfileid: "98558571"
---
# <a name="defender-for-iot-and-ics-threat-monitoring-application-for-splunk"></a>Защитник для приложения для мониторинга угроз IoT и ICS для Splunk

Защитник для Интернета вещей уменьшает риски промышленному, ICS и СКАДА с помощью запатентованных механизмов самообучения, поддерживающих ICS, которые предоставляют мгновенное представление об устройствах ICS, уязвимостях и угрозах менее чем за час образа и не полагаются на агенты, правила или подписи, специализированные навыки или знания о среде.

Чтобы устранить отсутствие видимости в области безопасности и устойчивости сетей, защитник для IoT разработал защитник для приложений для мониторинга угроз IoT, промышленному и ICS для Splunk, собственную интеграцию между защитником для Интернета вещей и Splunk, которая обеспечивает единый подход к ИТ и безопасности.

> [!Note]
> Ссылки на Циберкс см. в разделе защитник Azure для IoT.

## <a name="about-the-splunk-application"></a>О приложении Splunk

Приложение предоставляет аналитики SOC с многомерным отображением специализированных протоколов и устройств промышленному, развернутых в промышленных средах, а также анализа поведения с учетом ICS для быстрого обнаружения подозрительных или аномальных поведений. Приложение также обеспечивает отклики ИТ и инцидентов в рамках одного корпоративного SOC. Это важный эволюции, учитывая текущее схождение ИТ и для поддержки новых инициатив промышленному, таких как Интеллектуальные компьютеры и логика аналитики в реальном времени.

Приложение Splunk можно установить локально или запустить в облаке. Интеграция с защитником IoT поддерживает оба развертывания.

## <a name="about-the-integration"></a>Сведения об интеграции

Интеграция защитника для Интернета вещей и Splunk с помощью собственного приложения позволяет пользователям:

- Сократите время, необходимое для промышленных и критически важных организаций, чтобы обнаруживать, изучать и исдействовать угрозы кибератак.

- Получите аналитические сведения о рисках в реальном времени.

- Сопоставьте защитник для оповещений IoT с репозиториями анализа угроз Splunk Enterprise Security.

- Мониторинг и реагирование с одной панели на прозрачность.

[:::image type="content" source="media/integration-splunk/splunk-mainpage-v2.png" alt-text="Главная страница средства Splunk.":::](media/integration-splunk/splunk-mainpage-v2.png#lightbox)

:::image type="content" source="media/integration-splunk/alerts.png" alt-text="Страница &quot;оповещения&quot; в Splunk.":::

Приложение позволяет администраторам Splunk анализировать оповещения, отправляемые защитником для Интернета вещей, и отслеживать все развертывание системы безопасности, включая следующие сведения:

- Какой из пяти модулей аналитики обнаружил предупреждение.

- Протокол, создавший оповещение.

- Какой защитник для датчика Интернета вещей создал оповещение.

- Степень серьезности предупреждения.

- Источник и назначение связи.

## <a name="requirements"></a>Требования

### <a name="version-requirements"></a>Требования к версиям

Ниже приведены требования к следующим версиям.

- Защитник для IoT версии 2,4 и выше.

- Splunkbase версии 11 и выше.

- Splunk Enterprise версии 7,2 и выше.
  
## <a name="download-the-application"></a>Скачивание приложения

Скачайте *приложение мониторинга угроз ЦИБЕРКС ICS для приложения Splunk* из [Splunkbase](https://splunkbase.splunk.com/app/4313/).

## <a name="splunk-permission-requirements"></a>Требования к разрешениям Splunk

Требуется следующее разрешение Splunk:

- Любой пользователь с разрешениями роли пользователя *"Администратор"* .

## <a name="send-defender-for-iot-alerts-to-splunk"></a>Отправка защитника для оповещений IoT в Splunk

Защитник для оповещений IoT предоставляет сведения о широком спектре событий безопасности, в том числе:

- Отклонения от выученной базовой сетевой активности.

- Обнаружения вредоносных программ.

- Обнаружения на основе подозрительных рабочих изменений.

- Сетевые аномалии.

- Отклонения протоколов от спецификаций протокола.

:::image type="content" source="media/integration-splunk/address-scan.png" alt-text="Экран обнаружение.":::

Вы можете настроить защитник для Интернета вещей, чтобы отправлять оповещения на сервер Splunk, где сведения о предупреждениях отображаются на панели мониторинга Splunk Enterprise.

:::image type="content" source="media/integration-splunk/alerts-and-details.png" alt-text="Просмотрите все оповещения и сведения о них.":::

На сервер Splunk отправляются следующие сведения об оповещении.

- Дата и время оповещения.

- Защитник для механизма IoT, который обнаружил событие: нарушение протокола, нарушение политики, вредоносное по, аномалию или операционное ядро.

- Заголовок оповещения.

- Предупреждающее сообщение.

- Серьезность предупреждения: предупреждение, дополнительный, основной или критический.

- Имя исходного устройства.

- IP-адрес исходного устройства.

- Имя целевого устройства.

- IP-адрес целевого устройства.

- Защитник для IP-адреса платформы IoT (узел).

- Имя защитника для устройства платформы IoT (тип источника).

Ниже приведен пример выходных данных:

| Time | Событие |
|--|--|
| 7/23/15<br />9:28:31.000 PM | **Оповещение защитника для платформы IOT**: устройство остановлено с помощью команды ","<br /><br />**Тип**: нарушение эксплуатации <br /><br />**Серьезность**: Основная <br /><br />**Имя источника**: my_device1 <br /><br />**Исходный IP-адрес**: 192.168.110.131 <br /><br />**Имя назначения**: my_device2<br /><br /> **Конечный IP-адрес**: 10.140.33.238 <br /><br />**Сообщение**: сетевое устройство было остановлено с помощью команды "остановить". Это устройство не будет действовать до отправки команды запуска. 192.168.110.131 остановлена 10.140.33.238 (устройство Siemens S7) с помощью команды "перестали".<br /><br />**Узел**: 192.168.90.43 <br /><br />**SourceType**: Sensor_Agent |

## <a name="define-alert-forwarding-rules"></a>Определение правил пересылки предупреждений

Используйте защитник для *правил переадресации* IOT, чтобы отправлять сведения об оповещениях на серверы Splunk.

Доступны параметры для настройки правил генерации оповещений на основе:

- Обнаружены определенные протоколы.

- Серьезность события.

- Защитник для механизма IoT, который обнаруживает события.

Чтобы создать правило пересылки, выполните следующие действия.

1. В левой области консоли управления датчика или локальной среды выберите **перенаправление.**

    :::image type="content" source="media/integration-splunk/forwarding.png" alt-text="Нажмите синюю кнопку Создать оповещение о пересылке.":::

1. Выберите **создать правила пересылки**. В окне **Создание правила перенаправления** определите параметры правила.

    :::image type="content" source="media/integration-splunk/forwarding-rule.png" alt-text="Создайте правила для правила пересылки.":::

    | Параметр | Описание |
    |--|--|
    | **Имя** | Имя правила пересылки. |
    | **Выберите серьезность** | Минимальный уровень безопасности для пересылки. Например, если выбран параметр дополнительная, незначительные предупреждения и предупреждения выше этого уровня серьезности будут перенаправлены. |
    | **Протоколы** | По умолчанию выбраны все протоколы. Чтобы выбрать конкретный протокол, выберите **конкретный** и выберите протокол, к которому применяется это правило. |
    | **Двигатели** | По умолчанию все механизмы безопасности задействованы. Чтобы выбрать конкретный модуль безопасности, к которому применяется это правило, выберите **конкретное** и выберите подсистему. |
    | **Системные уведомления** | Состояние датчика прямого подключения к сети или в автономном режиме. Этот параметр доступен только в том случае, если вы выполнили вход в Центральный диспетчер. |                                            |

1. Чтобы настроить для центра Интернета вещей отправку сведений о ресурсах в Splunk, выберите **действие**, а затем выберите **отправить на сервер Splunk**.

1. Введите следующие параметры Splunk.

    :::image type="content" source="media/integration-splunk/parameters.png" alt-text="Параметры Splunk, которые следует ввести на этом экране.":::

    | Параметр | Описание |
    |--|--|
    | **Узел** | Адрес сервера Splunk |
    | **порт**. | 8089 |
    | **Имя пользователя** | Имя пользователя сервера Splunk |
    | **Пароль** | Пароль сервера Splunk |

1. Нажмите **Отправить**

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как [пересылать сведения о предупреждениях](how-to-forward-alert-information-to-partners.md).
