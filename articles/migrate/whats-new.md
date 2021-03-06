---
title: Новые возможности в службе "Миграция Azure"
description: Узнайте о новых и недавних обновлениях в службе "Миграция Azure".
ms.topic: overview
author: anvar-ms
ms.author: anvar
ms.manager: bsiva
ms.date: 04/19/2020
ms.custom: mvc
ms.openlocfilehash: cc207522048e3f0491e3aa54f2ae21d2e7e63bc2
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99834041"
---
# <a name="whats-new-in-azure-migrate"></a>Новые возможности в службе "Миграция Azure"

С помощью [службы "Миграция Azure"](migrate-services-overview.md) можно обнаруживать, оценивать и переносить локальные серверы, приложения и данные в облако Microsoft Azure. В этой статье описаны новые выпуски и функции службы "Миграция Azure".

## <a name="update-january-2021"></a>Обновление (январь 2021 г.)
-  Миграция Azure Средство миграции сервера теперь позволяет переносить виртуальные машины VMware, физические серверы и виртуальные машины из других облаков в виртуальные машины Azure с помощью дисков, зашифрованных управляемыми клиентом ключами с помощью шифрования на стороне сервера.

## <a name="update-december-2020"></a>Обновление (декабрь 2020 г.)
- Служба "Миграция Azure" теперь автоматически устанавливает агент виртуальных машин Azure на виртуальных машинах VMware, когда они переносятся в Azure с помощью метода миграции VMware без агента.
- Теперь на портале Azure можно перенести виртуальные машины VMware на виртуальные машины Azure с дисками, зашифрованными на стороне сервера (SSE) с помощью ключей, управляемых клиентом (CMK). Для такой миграции используется средство "Миграция сервера" службы "Миграция Azure" (репликация без агента).

## <a name="update-september-2020"></a>Обновление (сентябрь 2020 г.)
- Добавлена поддержка переноса серверов в зоны доступности.
- Теперь также поддерживается перенос виртуальных машин на основе UEFI и физических серверов на виртуальные машины Azure 2-го поколения. В этом выпуске средство миграции серверов службы "Миграция Azure" не будет выполнять преобразование виртуальной машины 2-го поколения в виртуальную машину 1-го поколения во время миграции.
- Доступна новая панель мониторинга оценок Power BI в службе "Миграция Azure", которая поможет вам сравнивать расходы с применением разных параметров оценок. Эта панель мониторинга включает служебную программу PowerShell, которая автоматически создает оценки, подключающиеся к панели мониторинга Power BI. [Подробнее.](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/assessment-utility)
- Анализ зависимостей (без агента) теперь можно выполнять параллельно на 1000 виртуальных машинах.
- Анализ зависимостей (без агента) теперь можно включить или отключить в большом масштабе с помощью скриптов PowerShell. [Подробнее.](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/dependencies-at-scale)
- Доступна визуализация сетевых подключений в Power BI с помощью данных, собранных с использованием анализа зависимостей (без агента). [Подробнее.](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/dependencies-at-scale)
- Теперь можно переносить виртуальные машины VMware с диском данных размером до 32 ТБ с помощью метода миграции VMware без агента, поддерживаемом в средстве "Миграция сервера" службы "Миграция Azure".

## <a name="update-august-2020"></a>Обновление (август 2020 г.)

- Улучшен процесс адаптации, в ходе которого создаваемый на портале ключ проекта Миграции Azure используется для завершения регистрации устройства.
- Включена поддержка загрузки файлов OVA/VHD или скриптов установщика с портала для настройки устройств VMware и Hyper-V соответственно.
- Обновлен диспетчер конфигурации устройств с улучшенным интерфейсом пользователя.
- Включена поддержка нескольких учетных данных для обнаружения виртуальных машин Hyper-V.

## <a name="update-july-2020"></a>Обновление (июль 2020 г.)

- Миграция VMware без агента теперь поддерживает параллельную репликацию 300 виртуальных машин на vCenter.

## <a name="update-june-2020"></a>Обновление (июнь 2020)

- Теперь поддерживаются оценки для переноса локальных виртуальных машин VMware в [Решение Azure VMware (AVS)](./concepts-azure-vmware-solution-assessment-calculation.md). [Дополнительные сведения](how-to-create-azure-vmware-solution-assessment.md)
- Поддержка нескольких учетных данных в модуле для обнаружения физических серверов.
- Поддержка возможности входа в Azure из модуля для клиента, в котором настроено ограничение клиента.


## <a name="update-april-2020"></a>Обновление (апрель 2020 г.)

Миграция Azure поддерживает развертывания в Azure для государственных организаций. 

- Вы можете обнаруживать и оценивать виртуальные машины VMware и Hyper-V, а также физические серверы.
- Вы можете переносить в Azure виртуальные машины VMware и Hyper-V, а также физические серверы.
- Для виртуальных машин VMware поддерживается миграция без агентов или на основе агентов. [Подробнее](server-migrate-overview.md).
- Просмотрите [список поддерживаемых регионов](migrate-support-matrix.md#supported-geographies-azure-government) для Azure для государственных организаций.
- [Анализ зависимостей на основе агента](concepts-dependency-visualization.md#agent-based-analysis) не поддерживается в Azure для государственных организаций.
- В Azure для государственных организаций поддерживаются такие функции в предварительной версии, как [анализ зависимостей без агента](concepts-dependency-visualization.md#agentless-analysis) и [обнаружение приложений](how-to-discover-applications.md).


## <a name="update-march-2020"></a>Обновление (март 2020 г.)

Теперь для настройки [устройства службы "Миграция Azure"](migrate-appliance.md) доступна установка на основе скриптов:

- Установка на основе сценария является альтернативой установки устройства OVA (VMware)/VHD (Hyper-V).
- Она предоставляет скрипт установщика PowerShell, который можно использовать для настройки устройства для VMware или Hyper-V на существующем компьютере под управлением Windows Server 2016.

## <a name="update-november-2019"></a>Обновление (ноябрь 2019 г.)

В службу "Миграция Azure" добавлено несколько новых возможностей.

- **Оценка физического сервера**. Помимо миграции физических серверов, теперь также поддерживается оценка локальных физических серверов.
- **Оценка на основе импорта**. Теперь поддерживается оценка компьютеров с использованием метаданных и данных о производительности, указанных в CSV-файле.
- **Обнаружение приложений**. Служба "Миграция Azure" теперь поддерживает обнаружение приложений, ролей и компонентов на уровне приложений с помощью устройства службы "Миграция Azure". Сейчас эта возможность поддерживается только для виртуальных машин VMware и ограничена только обнаружением (в настоящее время оценка не поддерживается). [Дополнительные сведения](how-to-discover-applications.md)
- **Визуализация зависимостей без агента**. Вам больше не нужно отдельно устанавливать агенты для визуализации зависимостей. Теперь поддерживается как безагентный вариант, так и вариант на основе агента.
- **Виртуальный рабочий стол**. используйте средства независимого поставщика программного обеспечения для оценки и миграции локальной инфраструктуры виртуальных рабочих столов (VDI) в Виртуальный рабочий стол Windows в Azure.
- **Веб-приложение**. Помощник по миграции Службы приложений Azure, используемый для оценки и миграции веб-приложений, теперь интегрирован в службу "Миграция Azure".

В службу "Миграция Azure" добавлены новые средства оценки и миграции.

- **Rackware**: используется для миграции в облако.
- **Movere**: предоставляет возможности оценки.

[Узнайте больше](migrate-services-overview.md) об оценке и миграции в службе "Миграция Azure" с помощью средств и предложений от независимых поставщиков программного обеспечения.

## <a name="azure-migrate-current-version"></a>Текущая версия службы "Миграция Azure"

Текущая версия службы "Миграция Azure" (выпущенная в июле 2019 г.) предоставляет ряд новых функций.

- **Единая платформа для миграции**: Теперь служба "Миграция Azure" предоставляет единый портал для централизации, управления и отслеживания процесса миграции в Azure с улучшенным потоком развертывания и возможностями портала.
- **Инструменты оценки и миграции**. Служба "Миграция Azure" предоставляет собственные инструменты и интегрируется с другими службами Azure, а также с инструментами независимого поставщика программного обеспечения (ISV). [Дополнительные сведения](migrate-services-overview.md#isv-integration) об интеграции ISV.
- **Оценка службы "Миграция Azure"** . С помощью инструмента оценки сервера службы "Миграция Azure" можно оценивать виртуальные машины VMware и Hyper-V на предмет миграции в Azure. Также можно оценить миграцию, используя другие службы Azure, а также инструменты ISV.
- **Миграция службы "Миграция Azure"** : С помощью средства миграции сервера службы "Миграция Azure" можно перенести локальные виртуальные машины VMware и Hyper-V в Azure, а также физические серверы, другие виртуализированные серверы и виртуальные частные/общедоступные облачные виртуальные машины. Кроме того, вы можете перенести их в Azure с помощью инструментов ISV.
- **Устройство службы "Миграция Azure"** : Служба "Миграция Azure" развертывает упрощенное устройство для обнаружения и оценки локальных виртуальных машин VMware и Hyper-V.
    - Это устройство используется при оценивании и миграции сервера службы "Миграция Azure" без агентов.
    - Метаданные сервера и данные производительности в рамках оценки и миграции можно обнаруживать с помощью непрерывной работы устройства.  
- **Миграция виртуальной машины VMware**:  Миграция сервера службы "Миграция Azure" предоставляет несколько методов для миграции локальных виртуальных машин VMware в Azure.  Миграция без агента с использованием устройства службы "Миграция Azure" и миграция с агентом, при которой используется устройство репликации и развертывается агент в каждой виртуальной машине, которую требуется перенести. [Дополнительные сведения](server-migrate-overview.md)
 - **Оценка и миграция базы данных**. С помощью службы "Миграция Azure" можно оценить локальные базы данных для миграции в Azure, используя помощника по миграции баз данных Azure. Базы данных можно перенести с помощью сервера миграции базы данных Azure.
- **Перенос веб-приложений**. Вы можете оценивать веб-приложения, используя общедоступный URL-адрес конечной точки в службе приложений Azure. Для переноса внутренних приложений .NET можно загрузить и запустить помощника по миграции службы приложений.
- **Data Box**. Импортируйте большие объемы автономных данных в Azure с помощью Azure Data Box в службе "Миграция Azure".

## <a name="azure-migrate-previous-version"></a>Предыдущая версия службы "Миграция Azure"

Если вы используете предыдущую версию службы "Миграция Azure" (поддерживалась только оценка локальных виртуальных машин VMware), теперь следует использовать текущую версию. В предыдущей версии вы больше не можете создавать новые проекты службы "Миграция Azure" или выполнять новые обнаружения. Вы по-прежнему можете получить доступ к существующим проектам. Перейдите на портал Azure, выберите **Все службы** и выполните поиск по запросу **Миграция Azure**. В уведомлении службы "Миграция Azure" есть уведомление и ссылка для доступа к старым проектам службы.



## <a name="next-steps"></a>Дальнейшие действия

- [Дополнительные сведения](https://azure.microsoft.com/pricing/details/azure-migrate/) о ценах на службу "Миграция Azure".
- [Просмотрите часто задаваемые вопросы](resources-faq.md) о службе "Миграция Azure".
- Перейдите к другим руководствам, чтобы оценить [виртуальные машины VMware](./tutorial-assess-vmware-azure-vm.md) и [виртуальные машины Hyper-V](tutorial-assess-hyper-v.md).