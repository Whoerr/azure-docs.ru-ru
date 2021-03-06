---
title: Управление пользователями, которые исключены из политик условного доступа — Azure AD
description: Узнайте, как использовать проверки доступа Azure Active Directory для управления пользователями, которые были исключены из политик условного доступа
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 12/23/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 426e28048ae370919529ea710717a3a3867d999d
ms.sourcegitcommit: 6e2d37afd50ec5ee148f98f2325943bafb2f4993
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/23/2020
ms.locfileid: "97746258"
---
# <a name="use-azure-ad-access-reviews-to-manage-users-excluded-from-conditional-access-policies"></a>Использование проверок доступа Azure AD для управления пользователями, которые исключены из политик условного доступа

В идеальном мире все пользователи соблюдают политики доступа, чтобы обеспечить безопасный доступ к ресурсам организации. Но иногда в интересах бизнеса приходится делать исключения. В этой статье рассматриваются некоторые ситуации, для которых могут потребоваться такие исключения. Вы, как ИТ-администратор, можете управлять решением этой задачи, чтобы избежать упущений в исключениях из политик и предоставить аудиторам доказательства регулярной проверки этих исключений с помощью проверок доступа Azure Active Directory (Azure AD).

>[!NOTE]
> Для использования проверок доступа Azure AD требуется действительная платная лицензия Azure AD Premium P2, Enterprise Mobility + Security E5 или пробная лицензия. Дополнительные сведения см. в разделе [Выпуски Azure Active Directory](../fundamentals/active-directory-whatis.md).

## <a name="why-would-you-exclude-users-from-policies"></a>Зачем нужно исключать пользователей из политики?

Предположим, что вы, как администратор, с помощью [условного доступа Azure AD](../conditional-access/concept-conditional-access-policy-common.md) решили реализовать многофакторную проверку подлинности (MFA) и ограничить запросы на проверку подлинности только конкретными сетями или устройствами. Планируя развертывание этой системы, вы понимаете, что не все пользователи могут соблюсти эти требования. Например, некоторые ваши пользователи могут работать удаленно, не находясь во внутренней сети организации. Кроме того, вам может потребоваться обеспечить доступ пользователям, которые, ожидая замены своих устройств, подключаются к системе с других, неподдерживаемых устройств. Говоря коротко, для бизнеса необходимо, чтобы эти пользователи входили в систему и делали свою работу, и поэтому вам нужно исключить их из политик условного доступа.

Еще один пример. Вы можете использовать [именованные расположения](../conditional-access/location-condition.md) условного доступа, чтобы запретить доступ к клиенту пользователям из определенных стран или регионов.

![Именованные расположения в условном доступе](./media/conditional-access-exclusion/named-locations.png)

К сожалению, у некоторых пользователей по-прежнему может быть веская причина для входа из заблокированных стран или регионов. Например, пользователи могут находиться в рабочей командировке и им может требоваться доступ к корпоративным ресурсам. В этом случае в политике условного доступа, которая блокирует эти страны или регионы, можно использовать специальную облачную группу безопасности для пользователей, к которым эта политика не применяется. Пользователи, которым нужен доступ на время поездок, смогут сами добавлять себя в эту группу с помощью [самостоятельного управления группами AAD](../enterprise-users/groups-self-service-management.md).

Еще один пример: у вас может быть политика условного доступа, которая [блокирует устаревшие методы проверки подлинности для большинства пользователей](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/07/azure-ad-conditional-access-support-for-blocking-legacy-auth-is-in-public-preview/). Если же у вас есть пользователи, которым все-таки необходимо использовать старые методы проверки подлинности для доступа к ресурсам из Office 2010 или через клиенты IMAP, SMTP или POP, вы можете исключить этих пользователей из политики, которая блокирует старые методы проверки подлинности.

>[!NOTE]
>Корпорация Майкрософт настоятельно рекомендует блокировать в клиенте устаревшие протоколы, чтобы повысить уровень безопасности.

## <a name="why-are-exclusions-challenging"></a>Почему исключения вызывают трудности?

В Azure AD вы можете указать, что политика условного доступа действует только для определенного набора пользователей. Кроме того, вы можете настроить исключения, указав в них нужные роли Azure AD, отдельных пользователей или пользователей-гостей. Важно помнить, что при наличии исключений невозможно обязать таких пользователей соблюдать требования политики. Если исключения настраиваются с помощью списка пользователей или устаревших локальных групп безопасности, вы сможете лишь ограниченно контролировать эти исключения. В результате имеем следующее:

- Пользователи могут не знать, что они находятся в категории исключенных.

- Пользователи могут присоединиться к группе безопасности, чтобы обойти политику.

- Исключенные пользователи могли подпадать под условия исключения раньше, но теперь уже не подпадают под эти условия.

Часто при первой настройке исключения имеется небольшой список пользователей, которым нужно обходить эту политику. Но с течением времени в число исключенных попадает все больше и больше пользователей и размер этого списка растет. В какой-то момент следует просмотреть этот список и проверить, все ли включенные в него пользователи действительно заслуживают исключения. С технической точки зрения управление списком исключений не вызывает особых трудностей. Но кто принимает деловые решения и как обеспечить возможность аудита? Если же вы настроите исключения с помощью группы Azure AD, к ним можно будет применять проверки доступа в качестве средства компенсирующего контроля для повышения видимости и сокращения числа исключенных пользователей.

## <a name="how-to-create-an-exclusion-group-in-a-conditional-access-policy"></a>Создание группы исключения в политике условного доступа

Чтобы создать группу Azure AD и политику условного доступа, которая не будет применяться к этой группе, сделайте следующее.

### <a name="create-an-exclusion-group"></a>Создание группы исключения

1. Войдите на портал Azure.

2. В области навигации слева щелкните **Azure Active Directory** и **Группы**.

3. В меню сверху щелкните **Новая группа**, чтобы открыть панель группы.

4. В списке **Тип группы** выберите **Безопасность**. Введите имя и описание.

5. Для параметра **Членство** обязательно укажите тип **Назначено**.

6. Выберите пользователей, которые должны входить в эту группу исключения, и щелкните **Создать**.

![Панель новой группы в Azure Active Directory](./media/conditional-access-exclusion/new-group.png)

### <a name="create-a-conditional-access-policy-that-excludes-the-group"></a>Создание политики условного доступа, которая не распространяется на группу

Теперь вы можете создать политику условного доступа, которая распространяется на эту группу исключения.

1. В области навигации слева щелкните **Azure Active Directory**, затем щелкните **Условный доступ**, чтобы открыть колонку **Политики**.

2. Щелкните **Новая политика**, чтобы открыть панель **Создание**.

3. Задайте имя.

4. В разделе "Назначения" выберите **Пользователи и группы**.

5. На вкладке **Включить** выберите **Все пользователи**.

6. На вкладке **Исключить** установите флажок **Пользователи и группы** и щелкните **Выбрать исключенных пользователей**.

7. Выберите ранее созданную группу исключения.

   > [!NOTE] 
   > Мы рекомендуем на время тестирования внести исключить из политики хотя бы одну учетную запись администратора, чтобы случайно не заблокировать весь доступ к клиенту.

8. Настройте остальные параметры политики условного доступа в соответствии с требованиями организации.

![Выбор панели исключенных пользователей в условном доступе](./media/conditional-access-exclusion/select-excluded-users.png)
  
Давайте рассмотрим два примера, в которых проверка доступа поможет в управлении исключениями из политик условного доступа.

## <a name="example-1-access-review-for-users-accessing-from-blocked-countriesregions"></a>Пример 1. Проверка доступа для пользователей, которым нужен доступ из заблокированных стран или регионов

Предположим, у вас есть политика условного доступа, которая блокирует доступ из определенных стран или регионов. В этой политике также настроена группа исключений. Для членов этой группы мы рекомендуем применить следующую проверку доступа.

> [!NOTE] 
> Для создания проверки доступа нужна роль глобального администратора или пользователя-администратора.

1. Проверка будет проводиться каждую неделю.

2. Время завершения не устанавливается, чтобы группа исключения всегда содержала самые актуальные сведения.

3. В область действия этой проверки включаются все члены этой группы.

4. Каждый пользователь должен самостоятельно подтвердить, что ему все еще требуется доступ из заблокированных стран или регионов, а значит, и членство в этой группе.

5. Если пользователь не отвечает на запрос проверки, он автоматически удаляется из группы и теряет доступ к клиенту во время поездок в указанные страны или регионы.

6. Включите уведомления по электронной почте, чтобы пользователи получали информацию о начале и завершении проверки доступа.

    ![Создание панели проверки доступа для примера 1](./media/conditional-access-exclusion/create-access-review-1.png)

## <a name="example-2-access-review-for-users-accessing-with-legacy-authentication"></a>Пример 2. Проверка доступа для пользователей, использующих устаревшие методы аутентификации

Предположим, вы настроили политику условного доступа, которая блокирует доступ для пользователей со старыми версиями клиента и старыми методами проверки подлинности, и в этой политике имеется группа, на которую не распространяется действие политики. Для членов этой группы мы рекомендуем применить следующую проверку доступа.

1. Эта проверка должна выполняться регулярно.

2. Проверка применяется к каждому члену группы.

3. Можно настроить ее так, чтобы рецензентами проверки являлись руководители бизнес-подразделений.

4. Автоматическое применение результатов позволяет автоматически удалять пользователей, для которых не подтверждена необходимость продолжать использование устаревших методов проверки подлинности.

5. Также будет полезно включить рекомендации, чтобы рецензентам больших групп было легче принимать решения.

6. Включите уведомления по электронной почте, чтобы пользователи получали информацию о начале и завершении проверки доступа.

    ![Создание панели проверки доступа для примера 2](./media/conditional-access-exclusion/create-access-review-2.png)

>[!IMPORTANT] 
>Если у вас много групп исключений, а значит, есть необходимость создавать много проверок доступа, вы можете выполнять эти действия программным способом через API-интерфейс в бета-версии конечной точки Microsoft Graph. Чтобы воспользоваться этими возможностями, изучите [документацию по API проверки доступа AAD](/graph/api/resources/accessreviews-root?view=graph-rest-beta) и [примеры проверок доступа для AAD через Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/td-p/236096).

## <a name="access-review-results-and-audit-logs"></a>Результаты проверки доступа и журналы аудита

Теперь у вас готовы все компоненты: группа, политика условного доступа и проверка доступа. Можно выполнять проверки и отслеживать их результаты.

1. На портале Azure откройте колонку **Проверки доступа**.

2. Откройте элемент управления и программу, которые вы создали для управления группой исключений.

3. Щелкните **Результаты**, чтобы увидеть списки подтвержденных и удаленных пользователей.

    ![Результаты проверок доступа показывают, какие пользователи были утверждены](./media/conditional-access-exclusion/access-reviews-results.png)

4. Теперь щелкните **Журналы аудита**, чтобы увидеть список выполненных в ходе этой проверки действий.

    ![Действия в журнале аудита проверки доступа](./media/conditional-access-exclusion/access-reviews-audit-logs.png)

Вы как ИТ-администратор должны знать, что иногда невозможно избежать создания групп исключений для политик. Однако проверки доступа Azure AD существенно упростят обслуживание таких групп, их регулярную проверку руководителями компании или самими пользователями, а также аудит изменений.

## <a name="next-steps"></a>Дальнейшие действия

- [Создание проверки доступа групп или приложений](create-access-review.md)
- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)