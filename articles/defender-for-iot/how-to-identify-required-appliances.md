---
title: Определение необходимых устройств
description: Узнайте о оборудовании и виртуальных устройствах для сертифицированного защитника для датчиков Интернета вещей и локальной консоли управления.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 01/13/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 92bf066c9769cc4b2525923b9e18ed3c0e9c577a
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98937132"
---
# <a name="identify-required-appliances"></a>Определение необходимых устройств

Эта статья содержит сведения о сертифицированном защитнике для устройств датчика IoT. Защитник Форт IoT можно развернуть на физических и виртуальных устройствах.

Сюда входят сертифицированные *предварительно настроенные* устройства, на которых уже установлено программное обеспечение, а также ненастроенные сертифицированные устройства, на которых можно загрузить и установить необходимое программное обеспечение.

В статье также приводятся спецификации для локального консольного устройства управления. Локальная консоль управления недоступна в качестве предварительно настроенного устройства.

- Если вы хотите приобрести предварительно настроенный датчик, ознакомьтесь с моделями, доступными в разделе [устройства датчика](#sensor-appliances) , а затем выполните покупку.

- Если вы хотите приобрести собственное устройство, ознакомьтесь с моделями, доступными в разделе [устройства датчиков](#sensor-appliances) и в разделе [Дополнительные сертифицированные устройства](#additional-certified-appliances) . После получения устройства можно загрузить и установить программное обеспечение.

- Если вы хотите приобрести локальную консоль управления, ознакомьтесь с информацией в разделе [Локальная консоль управления](#on-premises-management-console-appliance) . После получения устройства можно загрузить и установить программное обеспечение.

После выполнения задач здесь можно установить программное обеспечение и настроить сеть.

## <a name="sensor-appliances"></a>Устройства датчиков

Защитник для Интернета вещей поддерживает как физические, так и виртуальные развертывания.

### <a name="physical-sensors"></a>Физические датчики

В этом разделе приводятся общие сведения о доступных моделях физических датчиков. Вы можете приобрести датчики с предварительно настроенным программным обеспечением или датчиками приобретения, которые не были предварительно настроены.

| Тип развертывания | Корпоративный | Enterprise | SMB |
|--|--|--|--|
| Образ — | :::image type="content" source="media/how-to-prepare-your-network/corporate-hpe-proliant-dl360-v2.png" alt-text="Модель корпоративного уровня."::: | :::image type="content" source="media/how-to-prepare-your-network/enterprise-and-smb-hpe-proliant-dl20-v2.png" alt-text="Модель корпоративного уровня."::: | :::image type="content" source="media/how-to-prepare-your-network/enterprise-and-smb-hpe-proliant-dl20-v2.png" alt-text="Модель на уровне SMB."::: |
| Модель | HPE ProLiant DL360 | HPE ProLiant DL20 | HPE ProLiant DL20 |
| Порты мониторинга | До 15 RJ45 или 8 OPT | До 8 RJ45 или 6 OPT | 4 RJ45 |
| Максимальная пропускная способность [1](#anchortext) | 3 ГБ в секунду | 1 ГБ в секунду | 200 МБ в секунду |
| Максимальное число защищенных устройств | 30 000 | 15 000 | 1000 |

Сведения о поставщике см. в разделе [спецификации устройств](#appliance-specifications) .

Сведения о предварительно настроенных датчиках: Корпорация Майкрософт сотрудничает со стрелкой для предоставления предварительно настроенных датчиков. Чтобы приобрести предварительно настроенный датчик, свяжитесь со стрелкой по следующему адресу: <hardware.sales@arrow.com>

Сведения о том, как присвоить собственное устройство: Ознакомьтесь с поддерживаемыми моделями, описанными здесь. После приобретения устройства перейдите в раздел **защитник для**  >  **сетевого датчика** Интернета вещей  >  **Установка** ISO для загрузки программного обеспечения.

:::image type="content" source="media/how-to-prepare-your-network/azure-defender-for-iot-sensor-download-software-screen.png" alt-text="Сетевой датчик ISO.":::

<a id="anchortext">1</a> емкость полосы пропускания может быть различной в зависимости от распределения протоколов.

### <a name="virtual-sensors"></a>Виртуальные датчики

В этом разделе приводятся общие сведения о доступных виртуальных датчиках.

| Тип развертывания | Корпоративный | Enterprise | SMB |
|--|--|--|--|
| Максимальная пропускная способность | 2,5 ГБ/с | 800 МБ/с | 160 МБ/с |
| Максимальное число защищенных устройств | 30 000 | 10 000 | 2500 |

## <a name="on-premises-management-console-appliance"></a>Локальное консольное устройство управления

Консоль управления доступна в качестве виртуального развертывания.

| Тип развертывания | Enterprise |
|--|--|
| Тип устройства | HPE DL20, VM |
| Число управляемых датчиков | 300 |

После получения локальной консоли управления перейдите к разделу **защитник для**  >  **локальной**  >  **установки ISO** консоли управления IOT, чтобы скачать ISO.

:::image type="content" source="media/how-to-prepare-your-network/azure-defender-for-iot-iso-download-screen.png" alt-text="Локальная консоль управления.":::

## <a name="appliance-specifications"></a>Спецификации устройства

В этом разделе описываются спецификации оборудования для следующих устройств:

- Корпоративное развертывание: HPE ProLiant DL360

- Корпоративное развертывание: HPE ProLiant DL20

- Развертывание SMB: HPE ProLiant DL20

- Спецификации виртуальных устройств

## <a name="corporate-deployment-hpe-proliant-dl360"></a>Корпоративное развертывание: HPE ProLiant DL360

| Компонент | Технические спецификации |
|--|--|
| Корпус | сервер 1U в стойке |
| Измерения | 42,9 x 43,46 x 70,7 (cm)/1.69 "x 17,11" x 27,83 "(в) |
| Вес | Макс. 16,27 кг (35,86 фунтов) |
| Процессор | Intel Xeon серебро 4215 R 3,2 ГГц, кэш 11M, 8В/16T, 130 W |
| Микросхем | Intel C621 |
| Память | 32 ГБ = 2 x 16-ГБ 2666MT/с DDR4 ECC УДИММ |
| Память | 6 x 1,2-ТБ SAS 12G Enterprise 10000 СФФ (2,5 в) в Hot-Plug жестком диске — RAID 5 |
| Сетевой контроллер | Встроенная: 2 x 1-ГБ Broadcom BCM5720<br>Встроенный адаптер LOM: Идрак Port Card 1 – GB Broadcom BCM5720<br><br>External: 1 x Intel ethernet I350 QP 1 – ГБ серверного адаптера, низкопрофильный профиль |
| Управление | HPE iLO Advanced |
| Доступ к устройству | Два задних USB-порта 3,0<br>Один интерфейсный USB 2,0<br>Один внутренний порт USB 3,0 |
| Мощный | 2 x HPE 500 с беспроблемным гнездом "горячая установка" с горячим подключением для горячей установки низкого Halogen |
| Поддержка стойки | HPE 1U Gen10 СФФ простая установка набора салазок |

### <a name="appliance-bom"></a>Спецификация устройства

| PN | Описание | Количество |
|--|--|--|
| P19766-B21 | HPE DL360 Gen10 8SFF NC CTO Server | 1 |
| P19766-B21 | Европа — Многоязычная локализация | 1 |
| P24479-L21 | Комплект Intel Xeon-S 4215 R FIO Kit для DL360 G10 | 1 |
| P24479-B21 | Комплект Intel Xeon-S 4215 R Kit для DL360 Gen10 | 1 |
| P00922-B21 | Интеллектуальный набор HPE 16 ГБ 2Rx8 PC4-2933Y-R | 2 |
| 872479-B21 | HPE 1,2-ТБ SAS 10000 СФФ SC DS | 6 |
| 811546-B21 | HPE 1-GbE 4-p BASE-T I350 адаптер | 1 |
| P02377-B21 | HPE Smart гибридный конденсатор w \_ 145 mm | 1 |
| 804331-B21 | Смарт-массив HPE P408i — контроллер SR Gen10 | 1 |
| 665240-B21 | HPE 1-р. 4-p ФЛР-T адаптер I350 | 1 |
| 871244-B21 | Комплект для вентиляторов HPE DL360 Gen10 высокой производительности | 1 |
| 865408-B21 | Комплект для источников питания HPE 500-W FS Plat Hot Plug-in LH | 2 |
| 512485-B21 | HPE iLO, подсистема 1 – поддержка по серверной лицензии 1 года | 1 |
| 874543-B21 | HPE 1U Gen10 СФФ простая установка набора салазок | 1 |

## <a name="enterprise-deployment-hpe-proliant-dl20"></a>Корпоративное развертывание: HPE ProLiant DL20

| Компонент | Технические спецификации |
|--|--|
| Корпус | сервер 1U в стойке |
| Измерения (высота x ширина x глубина) | 4,32 x 43,46 x 38,22 cm/1,70 x 17,11 x 15,05 дюйм |
| Вес | 7,9 кг/17.41 фунтов |
| Процессор | Intel Xeon E-2234, 3,6 ГГц, 4C/8T, 71 W |
| Микросхем | Intel C242 |
| Память | 2 x 16 ГБ двух рангов x8 DDR4-2666 |
| Память | 3 x 1 – ТБ SATA 6G заполнение нажатием клавиши 7,2 K СФФ (2,5 в) — RAID 5 с интеллектуальным массивом P408i — контроллер SR |
| Сетевой контроллер | Встроенная плата: 2 x 1 ГБ <br>Встроенная плата: порт iLO с картой 1 ГБ <br>External: 1 x HPE Ethernet 1-ГБ 4-Port 366FLR Adapter |
| Управление | HPE iLO Advanced |
| Доступ к устройству | Front: 1 x USB 3,0, порт службы USB iLO 1 x <br>Задняя: 2 x USB 3,0 <br>Внутренний: 1 x USB 3,0 |
| Мощный | Источники питания с двумя горячими подключениями 500 W |
| Поддержка стойки | Набор салазок для коротких трений HPE 1U |

### <a name="appliance-bom"></a>Спецификация устройства

| PN | Описание: верхний конец | Количество |
|--|--|--|
| P06963-B21 | HPE DL20 Gen10 4SFF CTO Server | 1 |
| P06963-B21 | HPE DL20 Gen10 4SFF CTO Server | 1 |
| P17104-L21 | HPE DL20 Gen10 E-2234 FIO Kit | 1 |
| 879507-B21 | HPE 16 ГБ 2Rx8 PC4-2666V-E СТНД Kit | 2 |
| 655710-B21 | HPE 1-ТБ SATA 7,2 K СФФ SC DS | 3 |
| P06667-B21 | HPE DL20 Gen10 x8x16 ФЛОМ, набор переходов | 1 |
| 665240-B21 | Адаптер 366FLR HPE Ethernet 1-ГБ 4-Port | 1 |
| 782961-B21 | ВысокоHPEный аккумулятор Smart Storage с 12-Вт | 1 |
| 869081-B21 | Интеллектуальный массив HPE P408i — набор SR G10 LH Controller | 1 |
| 865408-B21 | Комплект для источников питания HPE 500-W FS Plat Hot Plug-in LH | 2 |
| 512485-B21 | HPE iLO, подсистема 1 – поддержка по серверной лицензии 1 года | 1 |
| P06722-B21 | Набор HPE DL20 Gen10 FIO для включения RP | 1 |
| 775612-B21 | Набор салазок для коротких трений HPE 1U | 1 |

## <a name="smb-deployment-hpe-proliant-dl20"></a>Развертывание SMB: HPE ProLiant DL20

| Компонент | Технические спецификации |
|--|--|
| Корпус | сервер 1U в стойке |
| Измерения (высота x ширина x глубина) | 4,32 x 43,46 x 38,22 cm/1,70 x 17,11 x 15,05 дюйм |
| Вес | 7,88 кг/17.37 фунтов |
| Процессор | Intel Xeon E-2224, 3,4 ГГц, 4C, 71 W |
| Микросхем | Intel C242 |
| Память | 1 x 8 ГБ, двойной ранг x8 DDR4-2666 |
| Память | 2 x 1 ТБ SATA 6G заполнение нажатием клавиши 7,2 K СФФ (2,5 в) — RAID 1 с интеллектуальным массивом P208i-a |
| Сетевой контроллер | Встроенная плата: 2 x 1 ГБ <br>Встроенная плата: порт iLO с картой 1 ГБ <br>External: 1 x HPE Ethernet 1-ГБ 4-Port 366FLR Adapter |
| Управление | HPE iLO Advanced |
| Доступ к устройству | Front: 1 x USB 3,0, порт службы USB iLO 1 x <br>Задняя: 2 x USB 3,0 <br>Внутренний: 1 x USB 3,0 |
| Мощный | Источник питания с поддержкой горячего подключения 290 W |
| Поддержка стойки | Набор салазок для коротких трений HPE 1U |

### <a name="appliance-bom"></a>Спецификация устройства

| PN | Описание | Количество |
|--|--|--|
| P06961-B21 | HPE DL20 Gen10 НХП 2LFF CTO Server | 1 |
| P06961-B21 | HPE DL20 Gen10 НХП 2LFF CTO Server | 1 |
| P17102-L21 | HPE DL20 Gen10 E-2224 FIO Kit | 1 |
| 879505-B21 | HPE 8 ГБ 1Rx8 PC4-2666V-E СТНД Kit | 1 |
| 801882-B21 | HPE 1-ТБ SATA 7,2 K ЛФФ RW | 2 |
| P06667-B21 | HPE DL20 Gen10 x8x16 ФЛОМ, набор переходов | 1 |
| 665240-B21 | Адаптер 366FLR HPE Ethernet 1-ГБ 4-Port | 1 |
| 869079-B21 | Интеллектуальный массив HPE E208i — набор SR G10 LH Controller | 1 |
| P21649-B21 | HPE DL20 Gen10 Plat 290 W FIO комплект питания | 1 |
| P06683-B21 | HPE DL20 Gen10 M. 2 SATA/ЛФФ арок-комплект кабелей | 1 |
| 512485-B21 | HPE iLO, подсистема 1 – поддержка по серверной лицензии 1 года | 1 |
| 775612-B21 | Набор салазок для коротких трений HPE 1U | 1 |

## <a name="virtual-appliance-specifications"></a>Спецификации виртуальных устройств

### <a name="sensors"></a>Датчики

| Тип | Корпоративный | Enterprise | SMB |
|--|--|--|--|
| vCPU | 32 | 8 | 4 |
| Память | 32 ГБ | 32 ГБ | 8 ГБ |
| Память | 5,6 ТБ | 1,8 ТБ | 500 ГБ |

### <a name="on-premises-management-console-appliance"></a>Локальное консольное устройство управления

| Тип | Enterprise |
|--|--|
| Описание | Виртуальные устройства для корпоративных типов развертывания |
| vCPU | 8 |
| Память | 32 ГБ |
| Память | 1,8 ТБ |

Поддерживаемые низкоуровневые оболочки: VMware ESXi версии 5,0 и более поздней, Hyper-V

## <a name="additional-certified-appliances"></a>Дополнительные сертифицированные устройства

В этом разделе описаны дополнительные устройства, которые были сертифицированы корпорацией Майкрософт, но не предлагаются в качестве предварительно настроенных устройств.

| Тип развертывания | Enterprise |
|--|--|
| Образ — | :::image type="content" source="media/how-to-prepare-your-network/deployment-type-enterprise-for-azure-defender-for-iot-v2.png" alt-text="Тип развертывания Enterprise."::: |
| Модель | Dell PowerEdge R340 XL |
| Порты мониторинга | До девяти или шесть необязательностей |
| Максимальная пропускная способность [1](#anchortext2)| 1 ГБ/с |
| Максимальное число защищенных устройств | 10 000 |

<a id="anchortext2">Один</a> Пропускная способность может отличаться в зависимости от распределения протоколов.

После приобретения устройства перейдите в раздел **защитник для**  >  **сетевого датчика** Интернета вещей  >  **Установка** ISO для загрузки программного обеспечения.

:::image type="content" source="media/how-to-prepare-your-network/azure-defender-for-iot-sensor-download-software-screen.png" alt-text="Сетевой датчик ISO.":::

## <a name="enterprise-deployment-dell-poweredge-r340-xl"></a>Корпоративное развертывание: Dell PowerEdge R340 XL

| Компонент | Технические спецификации |
|--|--|
| Корпус | сервер 1U в стойке |
| Измерения | 42,8 x 434,0 x 596 (mm)/1,67 "x 17,09" x 23,5 "(в) |
| Вес | Максимальная 29,98 фунтов/13,6 кг |
| Процессор | Intel Xeon E-2144G 3,6 ГГц, 8 МБ кэш, 4C/8T, Turbo (71 W) |
| Микросхем | Intel C246 |
| Память | 32 ГБ = 2 x 16-ГБ 2666MT/с DDR4 ECC УДИММ |
| Память | 3 x 2 – ТБ 7,2 КБ RPM SATA 6-Гбит/с 512N 3,5-in Hot-Plug жесткий диск — RAID 5 |
| Сетевой контроллер | Встроенная: 2 x 1-ГБ Broadcom BCM5720<br>Встроенный адаптер LOM: Идрак Port Card 1 – GB Broadcom BCM5720 <br><br>External: 1 x Intel ethernet I350 QP 1 – ГБ серверного адаптера, низкопрофильный профиль |
| Управление | Идрак девять корпоративных |
| Доступ к устройству | Два задних USB-порта 3,0 <br> Один интерфейсный USB 3,0 |
| Мощный | Источники питания с двумя горячими подключениями 350 W |
| Поддержка стойки | Реадираилс II, скользящие салазки для инструментов, не применяя 4-процессорные стойки с квадратными или немногопоточными круговыми отверстиями, а также с помощью инструментария с поддержкой необязательных средств управления кабелями на 4-постах. |

## <a name="dell-r340-bom"></a>Спецификация Dell R340

:::image type="content" source="media/how-to-prepare-your-network/enterprise-deployment-for-azure-defender-for-iot-dell-r340-bom.png" alt-text="Спецификация Dell R340.":::

## <a name="next-steps"></a>Дальнейшие действия

[Об установке Azure Defender для IoT](how-to-install-software.md)

[Настройка сети с Azure Defender для Интернета вещей](how-to-set-up-your-network.md)
