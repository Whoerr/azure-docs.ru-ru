---
title: Управление внешним доступом с помощью Azure Active Directory условного доступа
description: Использование Azure Active Directory политик условного доступа для защиты внешнего доступа к ресурсам.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64209a4d9ca200c69783a4ae317b38beef8ded89
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2021
ms.locfileid: "98222315"
---
# <a name="manage-external-access-with-conditional-access-policies"></a>Управление внешним доступом с помощью политик условного доступа 

[Условный доступ](../conditional-access/overview.md) — это средство, используемое Azure AD для объединения сигналов, применения политик и определения того, должен ли пользователь разрешить доступ к ресурсам. Подробные сведения о создании и использовании политик условного доступа (политики условного доступа) см. в статье [Планирование развертывания условного](../conditional-access/plan-conditional-access.md)доступа. 

![Схема сигналов и решений условного доступа](media/secure-external-access//7-conditional-access-signals.png)



В этой статье рассматривается применение политик условного доступа к внешним пользователям и предполагается, что у вас нет доступа к функциональным возможностям [управления](../governance/entitlement-management-overview.md) . Политики условного доступа можно использовать вместе с управлением назначениями.

Ранее в этом наборе документов был [создан план безопасности](3-secure-access-plan.md) , который был описан ниже.

* Приложения и ресурсы имеют одинаковые требования к безопасности и могут быть сгруппированы для доступа.

* Требования к входу для внешних пользователей.

Этот план будет использоваться для создания политик условного доступа для внешнего доступа. 

> [!IMPORTANT]
> Создайте несколько тестовых учетных записей внешних пользователей, чтобы можно было протестировать созданные политики перед их применением ко всем внешним пользователям.

## <a name="conditional-access-policies-for-external-access"></a>Политики условного доступа для внешнего доступа

Ниже приведены рекомендации по управлению внешним доступом с помощью политик условного доступа.

* Если вы не можете использовать подключенные Организации в управлении назначением, создайте группу безопасности Azure AD или группу Microsoft 365 для каждой партнерской организации, с которой вы работаете. Назначьте всем пользователям этого партнера группу. Эти группы можно использовать в политиках условного доступа.

* Создайте как можно меньше политик условного доступа. Для приложений с одинаковыми требованиями к доступу добавьте их в одну и ту же политику.  
‎ 
   > [!NOTE]
   > Политики условного доступа могут применяться к приложениям не более 250. Если более 250 приложений имеют одинаковые потребности в доступе, создайте дублирующиеся политики. Политика а будет применена к приложениям 1-250, политика б будет применяться к приложениям 251-500 и т. д.

* Четкое именование политик, характерных для внешнего доступа, с использованием соглашения об именах. Одно соглашение об именовании —*ExternalAccess_actiontaken_AppGroup*. Например ExternalAccess_Block_FinanceApps.

## <a name="block-all-external-users-from-resources"></a>Блокировать всех внешних пользователей из ресурсов

Вы можете блокировать доступ внешних пользователей к конкретным наборам ресурсов с помощью политик условного доступа. Определив набор ресурсов, к которым необходимо заблокировать доступ, создайте политику.

Создание политики, блокирующей доступ внешних пользователей к набору приложений:

1. Откройте **портал Azure**, выберите **Azure Active Directory**, выберите **Безопасность**, а затем выберите **Условный доступ**.

2. Выберите **создать политику** и введите **имя**, например ExternalAccess_Block_FinanceApps

3. Выберите **Пользователи и группы** s. На вкладке Включить выберите **выбрать пользователей и группы**, а затем выберите **все гости и внешние пользователи**. 

4. Выберите **исключить** и введите группы администраторов и любые учетные записи аварийного доступа (с непрозрачным разделением).

5. Выберите **облачные приложения или действия**, щелкните **выбрать приложения**, выберите все приложения, для которых требуется заблокировать внешний доступ, а затем нажмите кнопку **выбрать**.

6. Выберите **условия**, выберите **расположения**, в разделе Настройка выберите **Да**, а затем выберите **любое расположение**.

7. В разделе элементы управления доступом выберите **предоставить**, измените переключатель на **блокировать** и нажмите **кнопку Выбрать**.

8. Убедитесь, что для параметра включить политику задано значение **только отчет**, а затем нажмите кнопку **создать**.

## <a name="block-external-access-to-all-except-specific-external-users"></a>Блокировать внешний доступ ко всем, кроме отдельных внешних пользователей

Иногда требуется заблокировать внешних пользователей, кроме определенной группы. Например, может потребоваться заблокировать всех внешних пользователей, за исключением тех, кто работает с группой финансов из приложений финансового отдела. Выполните указанные ниже действия.

1. Создайте группу безопасности для хранения внешних пользователей, которые должны получить доступ к группе финансы.

2. Выполните шаги 1-3, чтобы заблокировать внешний доступ из ресурсов выше.

3. На шаге 4 Добавьте группу безопасности, которую вы хотите исключить из группы «финансы».

4. Выполните остальные шаги.

## <a name="implement-conditional-access"></a>Реализация условного доступа

Многие распространенные политики условного доступа документированы. См. следующие сведения, которые можно адаптировать для внешних пользователей.

* [Требовать многофакторную проверку подлинности для всех пользователей](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)

* [Условный доступ на основе рисков пользователей](../conditional-access/howto-conditional-access-policy-risk-user.md)

* [Требовать многофакторную проверку подлинности для доступа из ненадежных сетей](../conditional-access/untrusted-networks.md) 

* [Требовать условия использования](../conditional-access/terms-of-use.md)

## <a name="next-steps"></a>Дальнейшие действия

См. следующие статьи о защите внешнего доступа к ресурсам. Мы рекомендуем выполнить действия в указанном порядке.

1. [Определение требуемой системы безопасности для внешнего доступа](1-secure-access-posture.md)

2. [Обнаружение текущего состояния](2-secure-access-current-state.md)

3. [Создание плана управления](3-secure-access-plan.md)

4. [Использование групп для обеспечения безопасности](4-secure-access-groups.md)

5. [Переход на Azure AD B2B](5-secure-access-b2b.md)

6. [Безопасный доступ с помощью управления назначениями](6-secure-access-entitlement-managment.md)

7. [Безопасный доступ с помощью политик условного доступа](7-secure-access-conditional-access.md) (здесь)

8. [Безопасный доступ с метками чувствительности](8-secure-access-sensitivity-labels.md)

9. [Безопасный доступ к Microsoft Teams, OneDrive и SharePoint](9-secure-access-teams-sharepoint.md)
