---
title: Интеграция карт VM Insights с Operations Manager | Документация Майкрософт
description: Служба VM Insights автоматически обнаруживает компоненты приложений в системах Windows и Linux и сопоставляет взаимодействие между службами. В этой статье рассматривается использование функции Map для автоматического создания схем распределенного приложения в Operations Manager.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/12/2019
ms.openlocfilehash: a39f40c2a284a743db258a49f36cb4f13c2a4d1c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101725498"
---
# <a name="integrate-system-center-operations-manager-with-vm-insights-map-feature"></a>Интеграция System Center Operations Manager с функцией карт VM Insights

В Application Insights можно просматривать обнаруженные компоненты приложений на виртуальных машинах Windows и Linux, работающих в Azure или в среде. Благодаря такой интеграции между функцией Map и System Center Operations Manager можно автоматически создавать схемы распределенных приложений в Operations Manager, основанные на динамических сопоставлениях зависимостей в VM Insights. В этой статье описывается настройка группы управления System Center Operations Manager для поддержки этой функции.

>[!NOTE]
>Если вы уже развернули Сопоставление служб, вы можете просматривать карты в Application Insights, включая дополнительные функции для мониторинга работоспособности и производительности виртуальных машин. Функция Map для VM Insights предназначена для замены автономного Сопоставление службного решения. Дополнительные сведения см. в разделе [Общие сведения о VM Insights](../vm/vminsights-overview.md).

## <a name="prerequisites"></a>Предварительные требования

* Группа управления System Center Operations Manager (2012 R2 или более поздней версии).
* Log Analytics рабочей области, настроенной для поддержки аналитики VM.
* Одна или несколько виртуальных машин Windows и Linux или физических компьютеров, которые отслеживаются Operations Manager и отправляют данные в рабочую область Log Analytics. Серверы Linux, передающие данные группе управления Operations Manager, необходимо настроить для прямого подключения к Azure Monitor. Дополнительные сведения см. в обзоре статьи [Получение данных журнала с помощью агента log Analytics](../agents/log-analytics-agent.md).
* Субъект-служба с доступом к подписке Azure, связанной с рабочей областью Log Analytics. Дополнительные сведения см. в руководстве по [созданию субъекта-службы](#create-a-service-principal).

## <a name="install-the-service-map-management-pack"></a>Установка пакета управления схемы услуги

Интеграция между Operations Manager и функцией Map включается путем импорта пакета управления Microsoft.SysТемцентер. ServiceMap (Microsoft.SysТемцентер. ServiceMap. MPB). Скачать пакет управления можно в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=55763). Этот набор содержит следующие пакеты управления:

* Microsoft.ServiceMap.Application.Views;
* Microsoft.System.Center.ServiceMap.Internal;
* Microsoft.System.Center.ServiceMap.Overrides;
* Microsoft.System.Center.ServiceMap.

## <a name="configure-integration"></a>Настройка интеграции

После установки пакета управления Сопоставление служб в разделе **Operations Management Suite** в области **администрирование** консоли управления Operations Manager появится новый узел **сопоставление служб**.

>[!NOTE]
>[Operations Management Suite был набор служб](../terminology.md#april-2018---retirement-of-operations-management-suite-brand) , включающих log Analytics, теперь является частью [Azure Monitor](../overview.md).

Чтобы настроить интеграцию с картой VM Insights, выполните следующие действия.

1. Чтобы открыть мастер настройки, щелкните **Добавить рабочую область** в области **Service Map Overview** (Обзор схемы услуги).  

    ![Область "Service Map Overview" (Обзор схемы услуги)](media/service-map-scom/scom-configuration.png)

2. В окне **Настройка подключения** введите имя или идентификатор клиента, идентификатор приложения (имя пользователя или clientID) и пароль субъекта-службы, а затем нажмите кнопку **Далее**. Дополнительные сведения см. в руководстве по созданию субъекта-службы.

    ![Окно "Настройка подключения"](media/service-map-scom/scom-config-spn.png)

3. В окне **Subscription Selection** (Выбор подписки) выберите подписку Azure, группу ресурсов Azure (которая содержит рабочую область Log Analytics) и саму рабочую область Log Analytics, а затем нажмите кнопку **Далее**.

    ![Настройка Operations Manager: рабочая область](media/service-map-scom/scom-config-workspace.png)

4. В окне **Machine Group Selection** (Выбор группы компьютеров) выберите группы компьютеров решения "Сопоставление служб", которые необходимо синхронизировать с Operations Manager. Нажмите кнопку **Add/Remove Machine Groups** (Добавить или удалить группы компьютеров), выберите группы из списка **доступных групп компьютеров** и нажмите кнопку **Добавить**.  Выбрав группы, нажмите кнопку **ОК** для завершения.

    ![Настройка групп компьютеров в Operations Manager](media/service-map-scom/scom-config-machine-groups.png)

5. В окне **Выбор сервера** Настройте группу серверов сопоставление служб с серверами, которые требуется синхронизировать между Operations Manager и функцией Map. Нажмите кнопку **Добавление и удаление серверов**.

    Для интеграции и успешного построения схемы распределенного приложения для сервера он должен:

   * Отслеживается Operations Manager
   * Настроено для передачи в рабочую область Log Analytics, настроенную с помощью аналитики ВМ.
   * входить в группу серверов решения "Сопоставление служб".

     ![Настройка Operations Manager: группа](media/service-map-scom/scom-config-group.png)

6. Необязательно. Выберите пул ресурсов "все серверы управления" для взаимодействия с Log Analytics, а затем нажмите кнопку " **Добавить рабочую область**".

    ![Снимок экрана: экран пула серверов в диалоговом окне "Добавление Microsoft Operations Management Suite рабочей области со всеми серверами управления — пул ресурсов".](media/service-map-scom/scom-config-pool.png)

    Для настройки и регистрации рабочей области Log Analytics может потребоваться около минуты. После настройки Operations Manager инициирует первую синхронизацию карт.

    ![Снимок экрана: экран завершения в Microsoft Operations Management Suite рабочей области "Добавить", подтверждающий Добавление рабочей области.](media/service-map-scom/scom-config-success.png)

## <a name="monitor-integration"></a>Мониторинг интеграции

После подключения рабочей области Log Analytics в области **мониторинг** консоли управления Operations Manager отображается новая папка сопоставление служб.

![Operations Manager: область "Мониторинг"](media/service-map-scom/scom-monitoring.png)

Папка "Сопоставление служб" имеет четыре узла:

* **Активные предупреждения**: содержит список всех активных оповещений о связи между Operations Manager и Azure Monitor.  

  >[!NOTE]
  >Эти предупреждения не Log Analytics предупреждения, синхронизированные с Operations Manager, они создаются в группе управления на основе рабочих процессов, определенных в пакете управления Сопоставление служб.

* **Серверы**: список наблюдаемых серверов, настроенных для синхронизации из функции карт VM Insights.

    ![Operations Manager: область "Серверы"](media/service-map-scom/scom-monitoring-servers.png)

* **Представления зависимостей группы компьютеров**: перечисляет все группы компьютеров, синхронизированные из функции Map. Вы можете щелкнуть любую группу, чтобы просмотреть распределенные диаграммы приложения.

    ![Снимок экрана с Сопоставление служб отображение диаграммы с изображениями для каждой группы компьютеров и линий, указывающих зависимости между ними.](media/service-map-scom/scom-group-dad.png)

* **Представления зависимостей сервера**: перечисляет все серверы, синхронизированные из функции Map. Вы можете щелкнуть любой сервер, чтобы просмотреть его схему распределенного приложения.

    ![Снимок экрана Сопоставление служб отображение диаграммы с изображениями для каждого сервера и линий, указывающих зависимости между ними.](media/service-map-scom/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Изменение или удаление рабочей области

Вы можете изменить или удалить настроенную рабочую область с помощью области **Service Map Overview** (Обзор схемы услуги) (область **Администрирование** > **Operations Management Suite** > **Схема услуги**).

> [!NOTE]
> [Набор Operations Management Suite представлял собой коллекцию служб](../terminology.md#april-2018---retirement-of-operations-management-suite-brand), среди которых была и служба Log Analytics. Теперь эта служба входит в состав [Azure Monitor](../overview.md).

В текущем выпуске можно настроить только одну рабочую область Log Analytics.

![Operations Manager: область изменения рабочей области](media/service-map-scom/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Настройка правил и переопределений

Правило, *Microsoft.Sysтемцентер. ServiceMapImport. Rule*, периодически извлекает сведения из функции карт VM Insights. Чтобы изменить интервал синхронизации, можно переопределить правило и изменить значение параметра **интервалминутес**.

![Operations Manager: окно свойств переопределений](media/service-map-scom/scom-overrides.png)

* **Enabled**: позволяет включить или отключить автоматическое обновление.
* **Интервалминутес**: указывает время между обновлениями. Значение по умолчанию — один час. Если вы хотите чаще синхронизировать карты, это значение можно изменить.
* **TimeoutSeconds**: указывает продолжительность времени до истечения времени ожидания запроса.
* **TimeWindowMinutes**: указывает временное окно для запроса данных. Значение по умолчанию — 60 минут, что является максимальным допустимым интервалом.

## <a name="known-issues-and-limitations"></a>Известные проблемы и ограничения

В текущей версии существуют следующие проблемы и ограничения:

* Можно подключиться только к одной рабочей области Log Analytics.
* Хотя пользователи могут вручную добавить серверы в группу серверов решения "Сопоставление служб" с помощью области **Разработка**, сопоставления для этих серверов синхронизируются не сразу. Они будут синхронизированы из функции карт VM Insights во время следующего цикла синхронизации.
* При внесении изменений в диаграммы распределенных приложений, созданные пакетом управления, эти изменения, скорее всего, будут перезаписаны при следующей синхронизации с VM Insights.

## <a name="create-a-service-principal"></a>Создание субъекта-службы

Официальная документация Azure по созданию субъекта-службы представлена в следующих статьях:

* [Использование Azure PowerShell для создания субъекта-службы и доступа к ресурсам](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Использование интерфейса командной строки Azure для создания субъекта-службы и доступа к ресурсам](/cli/azure/create-an-azure-service-principal-azure-cli)
* [Создание приложения Azure Active Directory и субъекта-службы с доступом к ресурсам с помощью портала](../../active-directory/develop/howto-create-service-principal-portal.md)

### <a name="suggestions"></a>Предложения

У вас есть свои отзывы о интеграции с функцией карт VM Insights или в этой документации? Посетите [страницу пользователя Voice](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), где можно предложить возможности или проголосовать за существующие предложения.

