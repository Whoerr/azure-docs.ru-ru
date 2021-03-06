---
title: Настройка лабораторной работы для анализа больших данных с помощью служб лабораторий Azure | Документация Майкрософт
description: Узнайте, как настроить лабораторию для изучения анализа больших данных с помощью развертывания DOCKER платформы Hortonworks Data Platform (HDP).
author: nicolela
ms.topic: article
ms.date: 06/26/2020
ms.author: nicolela
ms.openlocfilehash: 5eb9cd00350c41645d4427e30a6f25a6c163358c
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "94659902"
---
# <a name="set-up-a-lab-for-big-data-analytics-using-docker-deployment-of-hortonworks-data-platform"></a>Настройка лаборатории для анализа больших данных с помощью развертывания DOCKER платформы HortonWorks Data Platform

В этой статье показано, как настроить лабораторию для изучения класса анализа больших данных.  С этим типом занятий студенты узнают, как работать с большими объемами данных и применять алгоритмы машинного и статистического обучения для получения аналитических данных.  Ключевым показателем для учащихся является обучение использованию средств анализа данных, таких как [программный пакет с открытым исходным кодом Apache Hadoop](https://hadoop.apache.org/) , который предоставляет средства для хранения больших данных, управления ими и их обработки.

В этом лабораторном занятии студенты будут использовать популярную коммерческую версию Hadoop, предоставляемую [Cloudera](https://www.cloudera.com/), именуемую [Hortonworks Data Platform (HDP)](https://www.cloudera.com/products/hdp.html).  В частности, учащиеся будут использовать [HDP песочницу 3.0.1](https://www.cloudera.com/tutorials/getting-started-with-hdp-sandbox/1.html) , которая представляет собой упрощенную, простую в использовании версию платформы, которая не имеет затрат и предназначена для обучения и экспериментов.  Хотя этот класс может использовать виртуальные машины Windows или Linux с развернутой песочницей HDP, в этой статье будет показано, как использовать Windows.

Еще один интересный аспект этой лабораторной работы заключается в том, что мы развернем песочницу HDP на виртуальных машинах лаборатории с помощью контейнеров [DOCKER](https://www.docker.com/) .  Каждый контейнер DOCKER предоставляет собственную изолированную среду для выполнения программных приложений внутри.  По сути, контейнеры DOCKER похожи на вложенные виртуальные машины и могут использоваться для простого развертывания и запуска широкого спектра программных приложений на основе образов контейнеров, предоставляемых в [DOCKER Hub](https://www.docker.com/products/docker-hub).  Сценарий развертывания Cloudera для песочницы HDP автоматически извлекает из DOCKER Hub [образ DOCKER HDP песочницы](https://hub.docker.com/r/hortonworks/sandbox-hdp) и выполняет два контейнера docker:
  - Песочница — HDP
  - Песочница — прокси

## <a name="lab-configuration"></a>Настройка лаборатории

Для настройки этой лабораторной работы требуется подписка Azure и учетная запись лаборатории. Если у вас нет подписки Azure, перед началом работы [создайте бесплатную учетную запись](https://azure.microsoft.com/free/). После получения подписки Azure можно создать новую учетную запись лаборатории в службах лаборатории Azure. Дополнительные сведения о создании учетной записи лаборатории см. в разделе [учебник по настройке учетной записи лаборатории](tutorial-setup-lab-account.md).  Можно также использовать существующую учетную запись лаборатории.

### <a name="lab-account-settings"></a>Параметры учетной записи лаборатории

Включите параметры, описанные в таблице ниже, для учетной записи лаборатории. Дополнительные сведения о том, как включить образы Marketplace, см. [в разделе Указание образов Marketplace, доступных авторам лабораторий](./specify-marketplace-images.md).

| Настройки учетной записи лаборатории | Instructions |
| ------------------- | ------------ |
|Образ Marketplace| Включите образ Windows 10 Pro для использования в учетной записи лаборатории.|

### <a name="lab-settings"></a>Параметры лаборатории

При настройке лаборатории для занятий используйте параметры, приведенные в таблице ниже.  Дополнительные сведения о создании лабораторной среды см. в разделе [Настройка лаборатории по созданию аудиторий](tutorial-setup-classroom-lab.md).

| Параметры лаборатории | Значение или инструкции |
| ------------ | ------------------ |
|Размер виртуальной машины| Средний (вложенная виртуализация). Этот размер виртуальной машины лучше всего подходит для реляционных баз данных, кэширования в памяти и аналитики.  Кроме того, этот размер поддерживает вложенную виртуализацию.|  
|Образ виртуальной машины| Windows 10 Pro|

> [!NOTE] 
> Необходимо использовать среднюю (вложенную виртуализацию), так как развертывание "песочницы" HDP с помощью DOCKER требует:
>   - Windows Hyper-V с вложенной виртуализацией
>   - Не менее 10 ГБ ОЗУ

## <a name="template-machine-configuration"></a>Конфигурация компьютера шаблона

Чтобы настроить компьютер шаблона, мы будем делать следующее:
- Установка Docker
- Развертывание песочницы HDP
- Использование PowerShell и Windows планировщик задач для автоматического запуска контейнеров DOCKER

### <a name="install-docker"></a>Установка Docker

Действия, описанные в этом разделе, основаны на [инструкциях Cloudera для развертывания с помощью контейнеров DOCKER](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html). 

Чтобы использовать контейнеры DOCKER, необходимо сначала установить DOCKER Desktop на виртуальной машине шаблона:

1. Выполните действия, описанные в [разделе Предварительные требования](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html#prerequisites) , чтобы установить [DOCKER для Windows](https://docs.docker.com/docker-for-windows/install/). 

    > [!IMPORTANT] 
    > Убедитесь, что параметр **использовать контейнеры Windows вместо конфигурации контейнеров Linux** не установлен.

1. Убедитесь, что включены **контейнеры Windows и компоненты Hyper-V** .
   ![Включение и отключение компонентов Windows](./media/class-type-big-data-analytics/windows-hyperv-features.png)

1. Выполните действия, описанные в разделе [память для Windows](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html#memory-for-windows) , чтобы настроить конфигурацию памяти DOCKER.

    > [!WARNING]
    > Если при установке DOCKER вы случайно установили флажок **использовать контейнеры Windows вместо контейнеров Linux** , параметры конфигурации памяти отображаться не будут.  Чтобы устранить эту проблему, можно переключиться на использование контейнеров Linux, [щелкнув значок DOCKER в области уведомлений Windows](https://docs.docker.com/docker-for-windows/#docker-settings-dialog). Когда откроется меню DOCKER Desktop, выберите **Переключиться в контейнеры Linux**.
 
### <a name="deploy-hdp-sandbox"></a>Развертывание песочницы HDP

В этом разделе вы развернете песочницу HDP, а затем получите доступ к песочнице HDP с помощью браузера.

1. Убедитесь, что вы установили [Git Bash](https://gitforwindows.org/) , как указано в [разделе "необходимые условия](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html#prerequisites) " руководства, так как это рекомендуется для выполнения дальнейших действий.

1. Используя [руководство по развертыванию и установке Cloudera для DOCKER](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html), выполните действия, описанные в следующих разделах:
   
   -    Развертывание песочницы HDP
   -    Проверка песочницы HDP

    > [!WARNING] 
    > При скачивании последнего ZIP-файла для HDP убедитесь, что файл Zip *не* сохраняется в каталоге, который содержит пробелы.

    > [!NOTE] 
    > Если при развертывании возникает исключение, уведомляющее о том, что **диск не был предоставлен в общий** доступ, необходимо предоставить общий доступ к диску C с помощью DOCKER, чтобы контейнеры Linux HDP могли получать доступ к локальным файлам Windows.  Чтобы устранить эту проблему, [щелкните значок DOCKER в области уведомлений Windows](https://docs.docker.com/docker-for-windows/#docker-settings-dialog) , чтобы открыть меню DOCKER Desktop и выбрать **Параметры**.  Когда откроется диалоговое окно **параметров DOCKER** , выберите **ресурсы > общий доступ к файлам** и проверьте диск **C** .  Затем можно повторить шаги для развертывания песочницы HDP.

1. После развертывания и запуска контейнеров DOCKER для песочницы HDP можно получить доступ к среде, запустив браузер и следуя инструкциям Cloudera для открытия [страницы приветствия "песочницы"](https://www.cloudera.com/tutorials/learning-the-ropes-of-the-hdp-sandbox.html#welcome-page) и запуска панели мониторинга HDP.

    > [!NOTE] 
    > В этих инструкциях предполагается, что сначала вы сопоставили локальный IP-адрес среды "песочницы" с sandbox-hdp.hortonworks.com в файле узла на виртуальной машине шаблона.  Если вы **не** выполняете это сопоставление, вы можете открыть страницу приветствия "песочницы", перейдя по адресу `http://localhost:8080` .

### <a name="automatically-start-docker-containers-when-students-log-in"></a>Автоматически запускать контейнеры DOCKER при входе учащихся

Чтобы обеспечить простоту использования для учащихся, мы будем использовать сценарий PowerShell, который автоматически:
  - Запускает контейнеры DOCKER "песочницы" HDP, когда учащийся запускается и подключается к виртуальной машине лаборатории.
  - Запускает браузер и переходит на страницу приветствия "песочницы".
Мы также будем использовать планировщик задач Windows для автоматического запуска этого скрипта при входе учащихся в свою виртуальную машину.
Чтобы настроить это, выполните следующие действия: [Создание скриптов для аналитики больших данных](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/BigDataAnalytics/).

## <a name="cost-estimate"></a>Оценка стоимости

Если вы хотите оценить стоимость этой лабораторной работы, можно использовать следующий пример.

Для класса из 25 учащихся с 20 часами запланированного времени обучения и 10 часами квоты для домашних заданий или назначений цена за лабораторию будет такой:
  - 25 учащихся * (20 + 10) ч * 55 модулей лаборатории * 0,01 долларов США в час = 412,50 долл. США

Дополнительные сведения о ценах см. в разделе [Цены на Службы лабораторий Azure](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="conclusion"></a>Заключение

В этой статье описаны шаги, необходимые для создания лаборатории для класса анализа больших данных, который использует платформу данных Hortonworks, развернутую с помощью DOCKER.  Программу установки для этого типа класса можно использовать для схожих классов аналитики данных.  Эта настройка также может быть применима к другим типам классов, использующих DOCKER для развертывания.

## <a name="next-steps"></a>Дальнейшие действия

Дальнейшие действия являются общими для настройки любой лаборатории.

- [Создание шаблона и управление им](how-to-create-manage-template.md)
- [Добавление пользователей](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Настройка квоты](how-to-configure-student-usage.md#set-quotas-for-users)
- [Настройка расписания](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Ссылки для регистрации электронной почты учащихся](how-to-configure-student-usage.md#send-invitations-to-users)