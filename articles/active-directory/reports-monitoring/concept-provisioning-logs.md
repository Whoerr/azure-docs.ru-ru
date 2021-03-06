---
title: Общие сведения о подготовке журналов в портал Azure (Предварительная версия) | Документация Майкрософт
description: Ознакомьтесь с введением в подготовку отчетов по журналам в Azure Active Directory с помощью портал Azure.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 1/29/2021
ms.author: markvi
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 284b554581534a8493225fba0b70b074fb7dd982
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101651943"
---
# <a name="overview-of-provisioning-logs-in-the-azure-portal-preview"></a>Общие сведения о подготовке журналов в портал Azure (Предварительная версия)

Архитектура создания отчетов в Azure Active Directory (Azure AD) состоит из следующих компонентов:

- Действие: 
    - **Входы**: сведения об использовании управляемых приложений и действиях входа пользователей.
    - [Журналы аудита](concept-audit-logs.md). сведения о действиях системы для управления пользователями и группами, управляемыми приложениями и действиями с каталогами.
    - **Журналы подготовки**: системные действия для пользователей, групп и ролей, подготовленных службой подготовки Azure AD. 

- Безопасность: 
    - **Рискованные входы**. [рискованный вход](../identity-protection/overview-identity-protection.md) является индикатором для попытки входа, которая могла быть выполнена пользователем, который не является законным владельцем учетной записи пользователя.
    - **Пользователи, помеченные для риска**: [рискованный пользователь](../identity-protection/overview-identity-protection.md) является индикатором для учетной записи пользователя, которая могла быть скомпрометирована.

В этом разделе приводятся общие сведения о журналах подготовки. Журналы содержат ответы на такие вопросы: 

* Какие группы были успешно созданы в ServiceNow?
* Какие пользователи были успешно удалены из Adobe?
* Какие пользователи из Workday были успешно созданы в Active Directory? 

## <a name="prerequisites"></a>Предварительные требования

Эти пользователи могут получать доступ к данным в журналах подготовки:

* Владельцы приложений (журналы для собственных приложений)
* Пользователи в роли "администратор безопасности", "читатель отчетов", "агент безопасности", "Администратор приложения" и "Администратор облачных приложений"
* Пользователи в пользовательской роли с [разрешением провисионинглогс](../roles/custom-enterprise-app-permissions.md#full-list-of-permissions)
* Глобальные администраторы


Для просмотра отчета о действиях подготовки к клиенту должна быть связана лицензия Azure AD Premium. Сведения об обновлении выпуска Azure AD см. в статье [Приступая к работе с Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md). 


## <a name="ways-of-interacting-with-the-provisioning-logs"></a>Способы взаимодействия с журналами подготовки 
Клиенты могут взаимодействовать с журналами подготовки четырьмя способами:

- Доступ к журналам из портал Azure, как описано в следующем разделе.
- Потоковая передача журналов подготовки в [Azure Monitor](../app-provisioning/application-provisioning-log-analytics.md). Этот метод обеспечивает расширенное хранение данных и создание пользовательских панелей мониторинга, предупреждений и запросов.
- Запрос [Microsoft Graph API](/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta) для журналов подготовки.
- Загрузка журналов подготовки в формате CSV или JSON.

## <a name="access-the-logs-from-the-azure-portal"></a>Доступ к журналам из портал Azure
Чтобы получить доступ к журналам подготовки, выберите **журналы подготовки** в разделе **мониторинг** панели **Azure Active Directory** в [портал Azure](https://portal.azure.com). Для отображения некоторых записей подготовки на портале может потребоваться до двух часов.

![Снимок экрана, на котором показаны элементы для доступа к журналам подготовки.](./media/concept-provisioning-logs/access-provisioning-logs.png "Подготовка журналов")


Журнал подготовки имеет представление списка по умолчанию, которое показывает:

- Удостоверение
- Действие
- Исходная система
- Целевая система
- Состояние
- Дата


![Снимок экрана, показывающий столбцы по умолчанию в журнале подготовки.](./media/concept-provisioning-logs/default-columns.png "Столбцы по умолчанию")

Можно настроить представление списка, выбрав **столбцы** на панели инструментов.

![Снимок экрана, на котором показана кнопка для настройки столбцов.](./media/concept-provisioning-logs/column-chooser.png "Выбор столбца")

Эта область позволяет отображать дополнительные поля или удалять уже отображаемые поля.

![Снимок экрана, на котором показаны доступные столбцы с несколькими выбранными.](./media/concept-provisioning-logs/available-columns.png "Доступные столбцы")

Чтобы получить более подробную информацию, выберите элемент в списке.

![Снимок экрана, на котором отображаются подробные сведения.](./media/concept-provisioning-logs/steps.png "Filter")


## <a name="filter-provisioning-activities"></a>Фильтрация действий по подготовке

Вы можете отфильтровать данные подготовки. Некоторые значения фильтров заполняются динамически на основе клиента. Если, например, у вас нет событий "создать" в клиенте, параметр "создать фильтр" не будет **создан** .

В представлении по умолчанию можно выбрать следующие фильтры:

- **Удостоверение**
- **Дата**
- **Состояние**
- **Действие**


![Снимок экрана, на котором показаны значения фильтра.](./media/concept-provisioning-logs/default-filter.png "Filter")

Фильтр **удостоверений** позволяет указать имя или удостоверение, которое вас интересуют. Это может быть пользователь, группа, роль или другой объект. 

Можно выполнить поиск по имени или ИДЕНТИФИКАТОРу объекта. Идентификатор зависит от сценария. Например, при подготовке объекта из Azure AD в Salesforce идентификатор источника — это идентификатор объекта пользователя в Azure AD. Идентификатор целевого объекта — это идентификатор пользователя в Salesforce. При подготовке от Workday к Active Directory ИДЕНТИФИКАТОРом источника является идентификатор работника рабочего дня Workday. 

> [!NOTE]
> Имя пользователя может не всегда присутствовать в столбце **идентификаторов** . Всегда будет один идентификатор. 


Фильтр **дат** позволяет определить интервал времени для возвращаемых данных. Возможны следующие значения:

- 1 месяц
- 7 дней
- 30 дней
- 24 часа
- Настраиваемый интервал

При выборе настраиваемого временного интервала можно настроить начальную и конечную даты.

В фильтре **Состояние** можете выбрать:

- **все**
- **Успешно**
- **Сбой**
- **Пропущено**

Фильтр **действий** позволяет отфильтровать следующие действия:

- **Создание** 
- **Обновление**
- **Удаление**
- **Отключено**
- **Другое**

В дополнение к фильтрам представления по умолчанию можно задать следующие фильтры.

![Снимок экрана, на котором показаны поля, которые можно добавить в качестве фильтров.](./media/concept-provisioning-logs/add-filter.png "Выбор поля")

- **ИД задания**: уникальный идентификатор задания связан с каждым приложением, для которого вы включили подготовку.   

- **Идентификатор цикла**: идентификатор цикла однозначно определяет цикл подготовки. Вы можете поделиться этим ИДЕНТИФИКАТОРом с помощью поддержки продукта, чтобы найти цикл, в котором произошло это событие.

- **Идентификатор изменения**: идентификатор изменения является уникальным идентификатором для события подготовки. Вы можете поделиться этим ИДЕНТИФИКАТОРом с помощью поддержки продукта, чтобы найти событие подготовки.   

- **Исходная система**. Вы можете указать, откуда будет подготавливается удостоверение. Например, при подготовке объекта из Azure AD в ServiceNow исходная система — это Azure AD. 

- **Целевая система**: можно указать, куда будет подготовлено удостоверение. Например, при подготовке объекта из Azure AD в ServiceNow используется целевая система ServiceNow. 

- **Приложение**. можно отобразить только записи приложений с отображаемым именем, содержащим определенную строку.

## <a name="provisioning-details"></a>Сведения о подготовке 

При выборе элемента в представлении списка подготовки вы получите дополнительные сведения об этом элементе. Подробные сведения группируются по следующим вкладкам.

![Снимок экрана, на котором показаны четыре вкладки, содержащие сведения о подготовке.](./media/concept-provisioning-logs/provisioning-tabs.png "Вкладки")

- **Шаги**: описывает шаги, предпринимаемые для инициализации объекта. Подготовка объекта может состоять из четырех шагов:
  
  1. Импортируйте объект.
  1. Определить, находится ли объект в области.
  1. Сопоставление объекта между источником и целевым объектом.
  1. Подготавливает объект (создание, обновление, удаление или отключение).

  ![На снимке экрана показаны этапы подготовки на вкладке шаги.](./media/concept-provisioning-logs/steps.png "Filter")

- **Устранение неполадок & рекомендации**: содержит код и причину ошибки. Сведения об ошибке доступны, только если происходит сбой.

- **Измененные свойства**: показывает старое и новое значения. Если старое значение отсутствует, этот столбец пуст.

- **Сводка**: предоставляет общие сведения о том, что случилось и идентификаторы для объекта в исходной и целевой системах.

## <a name="download-logs-as-csv-or-json"></a>Скачать журналы в формате CSV или JSON

Вы можете скачать журналы подготовки для последующего использования, перейдя к журналам в портал Azure и выбрав **загрузить**. Файл будет отфильтрован на основе выбранных критериев фильтра. Сделайте фильтры как можно точными, чтобы уменьшить размер и время загрузки. 

Загружаемый файл CSV содержит три файла:

* **Провисионинглогс**: скачивает все журналы, за исключением шагов подготовки и измененных свойств.
* **ProvisioningLogs_ProvisioningSteps**: содержит шаги подготовки и идентификатор изменения. Идентификатор изменения можно использовать для объединения события с другими двумя файлами.
* **ProvisioningLogs_ModifiedProperties**: содержит атрибуты, которые были изменены, и идентификатор изменения. Идентификатор изменения можно использовать для объединения события с другими двумя файлами.

#### <a name="open-the-json-file"></a>Открыть JSON файл
Чтобы открыть JSON-файл, используйте текстовый редактор, например [код Microsoft Visual Studio](https://aka.ms/vscode). Visual Studio Code упрощает чтение файла, предоставляя синтаксис выделения. Файл JSON также можно открыть с помощью браузеров в нередактируемом формате, таком как [Microsoft ребро](https://aka.ms/msedge). 

#### <a name="prettify-the-json-file"></a>Переработаны JSON файл
Файл JSON загружается в формате минифицированные, чтобы уменьшить размер загружаемого файла. Этот формат может сделать полезные данные трудными для чтения. Ознакомьтесь с двумя вариантами переработаны файла:

- [Для форматирования JSON используйте Visual Studio Code](https://code.visualstudio.com/docs/languages/json#_formatting).

- Используйте PowerShell для форматирования JSON. Этот скрипт выводит JSON в формате, содержащем символы табуляции и пробелы: 

  ` $JSONContent = Get-Content -Path "<PATH TO THE PROVISIONING LOGS FILE>" | ConvertFrom-JSON`

  `$JSONContent | ConvertTo-Json > <PATH TO OUTPUT THE JSON FILE>`

#### <a name="parse-the-json-file"></a>Анализ JSON файла

Ниже приведены примеры команд для работы с JSON с файлом с помощью PowerShell. Вы можете использовать любой язык программирования, с которым вам удобно работать.  

Сначала [прочтите файл JSON](/powershell/module/microsoft.powershell.utility/convertfrom-json?view=powershell-7.1) , выполнив следующую команду:

` $JSONContent = Get-Content -Path "<PATH TO THE PROVISIONING LOGS FILE>" | ConvertFrom-JSON`

Теперь данные можно проанализировать в соответствии с вашим сценарием. Вот несколько примеров. 

- Выводить все идентификаторы заданий в JSON file:

  `foreach ($provitem in $JSONContent) { $provitem.jobId }`

- Вывод всех идентификаторов изменений для событий, для которых было создано действие "создать":

  `foreach ($provitem in $JSONContent) { `
  `   if ($provItem.action -eq 'Create') {`
  `       $provitem.changeId `
  `   }`
  `}`

## <a name="what-you-should-know"></a>Необходимая информация

Ниже приведены некоторые советы и рекомендации по подготовке отчетов.

- Портал Azure сохраняет данные о подготовке в течение 30 дней, если у вас выпуск Premium и 7 дней, если у вас есть бесплатный выпуск. Журналы подготовки можно опубликовать в [log Analytics](../app-provisioning/application-provisioning-log-analytics.md) для хранения за пределами 30 дней. 

- Атрибут идентификатора изменения можно использовать в качестве уникального идентификатора. Это полезно, например, при взаимодействии с поддержкой продукта.

- Вы можете увидеть пропущенные события для пользователей, которые не находятся в области. Это ожидается, особенно если для области синхронизации задано значение все пользователи и группы. Служба будет оценивать все объекты в клиенте, даже те, которые выходят за пределы области. 

- Журналы подготовки сейчас недоступны в облаке для государственных организаций. Если вы не можете получить доступ к журналам подготовки, используйте журналы аудита в качестве временного решения. 

- Журналы подготовки не отображают импорты ролей (применяется к AWS, Salesforce и Zendesk). Журналы для импорта ролей можно найти в журналах аудита. 

## <a name="error-codes"></a>Коды ошибок

Используйте следующую таблицу, чтобы лучше понять, как устранять ошибки, найденные в журналах подготовки. Для всех отсутствующих кодов ошибок Предоставьте отзыв, используя ссылку в нижней части этой страницы. 

|Код ошибки|Описание|
|---|---|
|Конфликт, Ентриконфликт|Исправьте конфликтующие значения атрибутов в Azure AD или в приложении. Или проверьте соответствующую конфигурацию атрибута, если конфликтующая учетная запись пользователя должна быть сопоставлена и передаваться. Дополнительные сведения о настройке соответствующих атрибутов см. в [документации](../app-provisioning/customize-application-attributes.md) .|
|TooManyRequests|Целевое приложение отклонило попытку обновления пользователя из-за перегрузки и получения слишком большого количества запросов. Нет ничего делать. Эта попыток будет автоматически прекращена. Корпорация Майкрософт также уведомила об этой ошибке.|
|InternalServerError |Целевое приложение вернуло непредвиденную ошибку. Проблема службы с целевым приложением может препятствовать работе. Эта попытки будет автоматически прекращена через 40 минут.|
|ИнсуффиЦиентригхтс, Месодноталловед, Нотпермиттед, несанкционированный| Проверка подлинности Azure AD выполнена с целевым приложением, но не авторизована для выполнения обновления. Ознакомьтесь со всеми инструкциями, предоставленными целевым приложением, а также с [руководством](../saas-apps/tutorial-list.md)по соответствующему приложению.|
|унпроцессаблинтити|Целевое приложение вернуло непредвиденный ответ. Конфигурация целевого приложения может быть неправильной, или проблема службы с целевым приложением может препятствовать работе.|
|вебексцептионпротоколеррор |Произошла ошибка протокола HTTP при подключении к целевому приложению. Ничего делать не нужно. Эта попытки будет автоматически прекращена через 40 минут.|
|инвалиданчор|Пользователь, который ранее был создан или сопоставлен с помощью службы подготовки, больше не существует. Убедитесь, что пользователь существует. Чтобы принудительно выполнить новое сопоставление всех пользователей, используйте API Microsoft Graph, чтобы [перезапустить задание](/graph/api/synchronization-synchronizationjob-restart?tabs=http&view=graph-rest-beta). <br><br>Перезапуск подготовки приведет к запуску начального цикла, выполнение которого может занять некоторое время. При перезапуске подготовки также удаляется кэш, используемый службой подготовки. Это означает, что все пользователи и группы в клиенте должны повторно оцениваться, а некоторые события подготовки могут быть удалены.|
|NotImplemented | Целевое приложение вернуло непредвиденный ответ. Конфигурация приложения может быть неправильной, или проблема службы с целевым приложением может препятствовать работе. Ознакомьтесь со всеми инструкциями, предоставленными целевым приложением, а также с [руководством](../saas-apps/tutorial-list.md)по соответствующему приложению. |
|Мандаторифиелдсмиссинг, Миссингвалуес |Не удалось создать пользователя, так как отсутствуют необходимые значения. Исправьте отсутствующие значения атрибутов в исходной записи или проверьте конфигурацию соответствующего атрибута, чтобы убедиться, что требуемые поля не пропущены. Дополнительные [сведения](../app-provisioning/customize-application-attributes.md) о настройке соответствующих атрибутов.|
|счемааттрибутенотфаунд |Не удалось выполнить операцию, так как был указан атрибут, не существующий в целевом приложении. Ознакомьтесь с [документацией](../app-provisioning/customize-application-attributes.md) по настройке атрибутов и убедитесь, что конфигурация правильная.|
|InternalError |Произошла внутренняя ошибка службы в службе подготовки Azure AD. Ничего делать не нужно. Эта попытка будет автоматически повторена через 40 минут.|
|InvalidDomain |Не удалось выполнить операцию, так как значение атрибута содержит недопустимое имя домена. Обновите доменное имя пользователя или добавьте его в список разрешенных в целевом приложении. |
|Время ожидания |Не удалось выполнить операцию, так как целевое приложение заняло слишком много времени для ответа. Ничего делать не нужно. Эта попытка будет автоматически повторена через 40 минут.|
|лиценселимитексцеедед|Не удалось создать пользователя в целевом приложении из-за отсутствия доступных лицензий для этого пользователя. Приобретение дополнительных лицензий для целевого приложения. Или проверьте назначения пользователей и конфигурацию сопоставления атрибутов, чтобы убедиться, что им назначены правильные атрибуты.|
|дупликатетаржетентриес  |Не удалось выполнить операцию, так как найдено более одного пользователя в целевом приложении с настроенными атрибутами сопоставления. Удалите дублирующего пользователя из целевого приложения или [перенастройте сопоставления атрибутов](../app-provisioning/customize-application-attributes.md).|
|дупликатесаурцеентриес | Не удалось выполнить операцию, так как обнаружено несколько пользователей с настроенными соответствующими атрибутами. Удалите повторяющегося пользователя или [перенастройте сопоставления атрибутов](../app-provisioning/customize-application-attributes.md).|
|импортскиппед | При оценке каждого пользователя система пытается импортировать пользователя из исходной системы. Эта ошибка обычно возникает, когда у импортируемого пользователя отсутствует совпадающее свойство, определенное в сопоставлении атрибутов. Если в объекте пользователя отсутствует значение для атрибута сопоставления, система не может оценить изменения области, сопоставления или экспорта. Обратите внимание, что эта ошибка не указывает на то, что пользователь находится в области, так как вы еще не оценили области для пользователя.|
|ентрисинчронизатионскиппед | Служба подготовки успешно запросила исходную систему и определила пользователя. Никаких дополнительных действий пользователя не было выполнено, и они были пропущены. Возможно, пользователь был вне области действия или пользователь уже существовал в целевой системе, но дальнейшие изменения не требуются.|
|системфоркроссдомаинидентитиманажементмултиплинтриесинреспонсе| Запрос GET для получения пользователя или группы получил нескольких пользователей или группы в ответе. Система должна получить в ответе только одного пользователя или группу. [Например](../app-provisioning/use-scim-to-provision-users-and-groups.md#get-group), если вы выполняете запрос GET для получения группы и предоставляете фильтр для исключения членов, а система для конечных точек с междоменным управлением удостоверениями (scim) возвращает элементы, вы получаете эту ошибку.|

## <a name="next-steps"></a>Дальнейшие действия

* [Проверка состояния подготовки пользователей](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md)
* [Проблема при настройке подготовки пользователей для приложения из коллекции Azure AD](../app-provisioning/application-provisioning-config-problem.md)
* [API Graph для журналов подготовки](/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta)