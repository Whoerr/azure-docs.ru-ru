---
title: Защита Microsoft 365 от локальных атак
description: Руководство по обеспечению локальной атаки не влияет на Microsoft 365.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/22/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9913132ca6ff3ce2840b55aa771f7bd9be2404ea
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101650980"
---
# <a name="protecting-microsoft-365-from-on-premises-attacks"></a>Защита Microsoft 365 от локальных атак

Многие клиенты соединяются с частными корпоративными сетями, чтобы Microsoft 365, чтобы воспользоваться преимуществами их пользователей, устройств и приложений. Однако эти частные сети могут быть скомпрометированы во многих хорошо документированных способах. Поскольку Microsoft 365 выступает в качестве нервной системы для многих организаций, важно защитить ее от скомпрометированной локальной инфраструктуры.

В этой статье показано, как настроить системы для защиты Microsoft 365 облачной среды от несанкционированного доступа в локальной среде. Основное внимание уделяется: 

- Параметры конфигурации клиента Azure Active Directory (Azure AD).
- Как клиенты Azure AD могут быть безопасно подключены к локальным системам.
- Компромиссы, необходимые для работы систем в целях защиты облачных систем от несанкционированного использования в локальной среде.

Мы настоятельно рекомендуем реализовать это руководство для защиты Microsoft 365 облачной среды.
> [!NOTE]
> Эта статья изначально опубликована в записи блога. Он был перемещен в текущее расположение для долговечности и обслуживания.
>
> Чтобы создать автономную версию этой статьи, используйте функции печати в браузере. Регулярно посещайте эту ссылку для установки обновлений.

## <a name="primary-threat-vectors-from-compromised-on-premises-environments"></a>Основные векторы угроз от скомпрометированных локальных сред


Microsoft 365 облачной среды — преимущества обширной инфраструктуры мониторинга и безопасности. Используя машинное обучение и человеческий интеллект, Microsoft 365 выглядит по всему миру. Он может быстро обнаружить атаки и настроить практически в режиме реального времени. 

В гибридных развертываниях, которые соединяют локальную инфраструктуру с Microsoft 365, многие организации делегируют доверие локальным компонентам для критически важных решений проверки подлинности и управления состоянием объектов каталога.
К сожалению, если локальная среда скомпрометирована, эти доверительные отношения становятся потенциальными злоумышленниками для компрометации Microsoft 365ной среды.

Двумя основными векторами угроз являются *отношения доверия федерации* и *Синхронизация учетных записей.* Оба вектора могут предоставить злоумышленнику административный доступ к вашему облаку.

* **Федеративные отношения доверия**, такие как проверка подлинности SAML, используются для проверки подлинности Microsoft 365 с помощью локальной инфраструктуры идентификации. Если сертификат для подписи маркера SAML скомпрометирован, Федерация позволяет всем пользователям, имеющим этот сертификат, олицетворять любого пользователя в облаке. *Мы рекомендуем отключить отношения доверия федерации для проверки подлинности, чтобы Microsoft 365 по возможности.*

* **Синхронизацию учетных записей** можно использовать для изменения привилегированных пользователей (включая их учетные данные) или групп с правами администратора в Microsoft 365. *Рекомендуется гарантировать, что синхронизированные объекты не будут иметь привилегий, кроме пользователя, в Microsoft 365,* напрямую или путем включения в доверенные роли или группы. Убедитесь, что эти объекты не имеют прямого или вложенного назначения в ролях или группах доверенного облака.

## <a name="protecting-microsoft-365-from-on-premises-compromise"></a>Защита Microsoft 365 от несанкционированного доступа в локальной среде


Для решения описанных выше векторов угроз рекомендуется соблюдать принципы, показанные на схеме ниже.

![Эталонная архитектура для защиты Microsoft 365.](media/protect-m365/protect-m365-principles.png)

1. **Полностью изолируйте учетные записи администратора Microsoft 365.** Они должны быть:

    * В Azure AD.

     * Проверка подлинности с использованием многофакторной проверки подлинности.

     *  Защищено с помощью условного доступа Azure AD.

     *  Доступно только с рабочих станций, управляемых Azure.

    Эти учетные записи администратора являются учетными записями с ограниченным использованием. *Локальные учетные записи не должны иметь прав администратора в Microsoft 365.* 

    Дополнительные сведения см. в [обзоре ролей администратора Microsoft 365](/microsoft-365/admin/add-users/about-admin-roles?view=o365-worldwide). См. также [роли для Microsoft 365 в Azure AD](../roles/m365-workload-docs.md).

1. **Управление устройствами из Microsoft 365.** Используйте присоединение к Azure AD и управление мобильными устройствами (MDM) на основе облачных служб, чтобы исключить зависимости от инфраструктуры управления локальными устройствами. Эти зависимости могут нарушать Управление устройствами и безопасностью.

1. **Убедитесь, что локальная учетная запись не имеет повышенных привилегий для Microsoft 365.**
    Некоторые учетные записи обращаются к локальным приложениям, для которых требуется проверка подлинности NTLM, LDAP или Kerberos. Эти учетные записи должны находиться в локальной инфраструктуре идентификации Организации. Убедитесь, что эти учетные записи, включая учетные записи служб, не входят в привилегированные облачные роли или группы. Также убедитесь, что изменения в этих учетных записях не могут повлиять на целостность облачной среды. Привилегированное локальное программное обеспечение не может влиять на Microsoft 365 привилегированных учетных записей или ролей.

1. **Используйте проверку подлинности в облаке Azure AD** , чтобы исключить зависимости от локальных учетных данных. Всегда используйте строгую проверку подлинности, например Windows Hello, FIDO, Microsoft Authenticator или многофакторную проверку подлинности Azure AD.

## <a name="specific-security-recommendations"></a>Конкретные рекомендации по безопасности


В следующих разделах приводятся конкретные рекомендации по реализации описанных выше принципов.

### <a name="isolate-privileged-identities"></a>Изоляция привилегированных удостоверений


В Azure AD пользователи, имеющие привилегированные роли, такие как администраторы, являются корнем доверия для создания и управления остальной частью среды. Чтобы снизить последствия компрометации, реализуйте следующие методы.

* Используйте учетные записи только для облака для Azure AD и Microsoft 365 привилегированных ролей.

* Развертывание [устройств привилегированного доступа](/security/compass/privileged-access-devices#device-roles-and-profiles) для привилегированного доступа для управления Microsoft 365 и Azure AD.

*  Развертывайте [Azure AD privileged Identity Management](../privileged-identity-management/pim-configure.md) (PIM) для JIT-доступа ко всем учетным записям, имеющим привилегированные роли. Требовать строгую проверку подлинности для активации ролей.

* Предоставьте административные роли, которые обеспечивают [минимальную привилегию, необходимую для необходимых задач](../roles/delegate-by-task.md).

* Чтобы обеспечить широкие возможности назначения ролей, включающие делегирование и несколько ролей одновременно, рассмотрите возможность использования групп безопасности Azure AD или групп Microsoft 365. Эти группы совместно называются *облачными группами*. Также [включите управление доступом на основе ролей](../roles/groups-assign-role.md). Вы можете использовать [административные единицы](../roles/administrative-units.md) , чтобы ограничить область ролей частью Организации.

* Развертывание [учетных записей аварийного доступа](../roles/security-emergency-access.md). *Не* используйте локальные хранилища паролей для хранения учетных данных.

Дополнительные сведения см. в разделе [Защита привилегированного доступа](/security/compass/overview). См. также [рекомендации по безопасному доступу для администраторов в Azure AD](../roles/security-planning.md).

### <a name="use-cloud-authentication"></a>Использовать проверку подлинности в облаке 

Учетные данные являются основным вектором атак. Чтобы сделать учетные данные более безопасными, реализуйте следующие методы.

* [Развертывание проверки подлинности с паролем](../authentication/howto-authentication-passwordless-deployment.md). При развертывании учетных данных, не имеющих пароля, сократите использование паролей максимально возможным. Эти учетные данные управляются и проверяются изначально в облаке. Выберите один из следующих методов проверки подлинности:

   * [Windows Hello для бизнеса](/windows/security/identity-protection/hello-for-business/passwordless-strategy)

   * [Приложение Microsoft Authenticator](../authentication/howto-authentication-passwordless-phone.md)

   * [Ключи безопасности FIDO2](../authentication/howto-authentication-passwordless-security-key-windows.md)

* [Развертывание многофакторной проверки подлинности](../authentication/howto-mfa-getstarted.md). Подготавливает [несколько надежных учетных данных с помощью многофакторной проверки подлинности Azure AD](../fundamentals/resilience-in-credentials.md). Таким образом, для доступа к облачным ресурсам потребуется учетные данные, управляемые в Azure AD, а также локальный пароль, который можно изменять. Дополнительные сведения см. в статье [Создание отказоустойчивой стратегии управления доступом с помощью Azure AD](./resilience-overview.md).

### <a name="limitations-and-tradeoffs"></a>Ограничения и компромиссы

* Для управления паролями гибридной учетной записи требуются Гибридные компоненты, такие как агенты защиты паролей и агенты обратной записи паролей. Если ваша локальная инфраструктура скомпрометирована, злоумышленники могут управлять компьютерами, на которых находятся эти агенты. Эта уязвимость не будет нарушать вашу облачную инфраструктуру. Но облачные учетные записи не защищают эти компоненты от несанкционированного доступа в локальной среде.

*  Локальные учетные записи, синхронизированные из Active Directory, помечены для бессрочных истечения срока действия в Azure AD. Этот параметр обычно уменьшается в локальной Active Directory параметры пароля. Однако если ваш локальный экземпляр Active Directory скомпрометирован и синхронизация отключена, необходимо установить параметр [енфорцеклаудпассвордполицифорпассвордсинцедусерс](../hybrid/how-to-connect-password-hash-synchronization.md) для принудительного изменения пароля.

## <a name="provision-user-access-from-the-cloud"></a>Предоставление пользователям доступа из облака

*Подготовка* подразумевает создание учетных записей пользователей и групп в приложениях или поставщиках удостоверений.

![Схема архитектуры подготовки.](media/protect-m365/protect-m365-provision.png)

Мы рекомендуем использовать следующие методы подготовки:

* **Подготовка из облачных приложений отдела кадров к Azure AD**. Эта подготовка позволяет изолировать компьютер в локальной среде, не нарушая цикл приостановки работы и выхода из облачных приложений отдела кадров в Azure AD.

* **Облачные приложения**. по возможности развертывайте [подготовку приложений Azure AD](../app-provisioning/user-provisioning.md) в отличие от локальных решений подготовки. Этот метод защищает некоторые приложения SaaS от вредоносных профилей злоумышленников в локальных средах, не подверженных атакам. 

* **Внешние удостоверения**. Используйте [службу совместной работы Azure AD B2B](../external-identities/what-is-b2b.md).
    Этот метод сокращает зависимость от локальных учетных записей для внешней совместной работы с партнерами, клиентами и поставщиками. Тщательно оцените любую прямую федерацию с другими поставщиками удостоверений. Мы рекомендуем ограничить учетные записи гостя B2B следующими способами.

   *  Ограничьте гостевой доступ для обзора групп и других свойств в каталоге. Используйте параметры внешней совместной работы, чтобы запретить гостям возможность чтения групп, которые они не входят в группу. 

    *   Блокировка доступа к портал Azure. Вы можете делать нередко необходимые исключения.  Создайте политику условного доступа, включающую всех гостей и внешних пользователей. Затем [реализуйте политику для блокирования доступа](../../role-based-access-control/conditional-access-azure-management.md). 

* **Отключенные леса**. Используйте [подготовку облачных служб Azure AD](../cloud-sync/what-is-cloud-sync.md). Этот метод позволяет подключаться к отключенным лесам, устраняя необходимость установления подключения между лесами или отношения доверия, что может расширить воздействие локального нарушения. 
 
### <a name="limitations-and-tradeoffs"></a>Ограничения и компромиссы

При использовании для инициализации гибридных учетных записей система Azure-AD-from-Cloud-HR использует локальную синхронизацию для завершения потока данных из Active Directory в Azure AD. Если синхронизация прервана, новые записи о сотрудниках будут недоступны в Azure AD.

## <a name="use-cloud-groups-for-collaboration-and-access"></a>Использование облачных групп для совместной работы и доступа

Облачные группы позволяют отделить совместную работу и доступ из локальной инфраструктуры.

* **Совместная** работа. используйте группы Microsoft 365 и Microsoft Teams для современной совместной работы. Списание локальных списков рассылки и [Обновление списков рассылки до Microsoft 365 групп в Outlook](/office365/admin/manage/upgrade-distribution-lists?view=o365-worldwide).

* **Доступ**. Используйте группы безопасности Azure AD или группы Microsoft 365, чтобы авторизовать доступ к приложениям в Azure AD.
* **Лицензирование office 365**. использование группового лицензирования для предоставления Office 365 с помощью групп, предназначенных только для облака. Этот метод разделяет Управление членством в группе из локальной инфраструктуры.

Владельцы групп, используемых для доступа, должны считаться привилегированными удостоверениями, чтобы не перенаправление членство в локальной системе безопасности.
Перенаправление будет включать в себя прямую манипуляции с членством в локальной группе или манипуляцией с локальными атрибутами, которые могут повлиять на динамическое членство в группе Microsoft 365.

## <a name="manage-devices-from-the-cloud"></a>Управление устройствами из облака


Используйте возможности Azure AD для безопасного управления устройствами.

-   **Использование рабочих станций Windows 10**: [развертывание присоединенных к Azure AD](../devices/azureadjoin-plan.md) устройств с помощью политик MDM. Включите [Windows автопилот](/mem/autopilot/windows-autopilot) для полностью автоматизированного процесса подготовки.

    -   Используйте нерекомендуемые компьютеры, на которых выполняется Windows 8.1 и более ранних версий.

    -   Не развертывайте компьютеры серверных ОС в качестве рабочих станций.

    -   Используйте [Microsoft Intune](https://www.microsoft.com/microsoft-365/enterprise-mobility-security/microsoft-intune) в качестве источника полномочий для всех рабочих нагрузок по управлению устройствами.

-   [**Развертывание устройств привилегированного доступа**](/security/compass/privileged-access-devices#device-roles-and-profiles). Используйте привилегированный доступ для управления Microsoft 365 и Azure AD.

## <a name="workloads-applications-and-resources"></a>Рабочие нагрузки, приложения и ресурсы 

-   **Локальные системы единого входа (SSO)** 

    Не рекомендуется использовать локальную инфраструктуру управления интеграцией и веб-доступом. Настройка приложений для использования Azure AD.  

-   **SaaS и бизнес-приложения, поддерживающие современные протоколы проверки подлинности** 

    [Используйте Azure AD для единого входа](../manage-apps/what-is-single-sign-on.md). Чем больше приложений вы настраиваете для использования Azure AD для проверки подлинности, тем меньше риск в локальной системе безопасности.


* **Устаревшие приложения** 

   * Вы можете включить проверку подлинности, авторизацию и удаленный доступ к устаревшим приложениям, которые не поддерживают современную проверку подлинности. Используйте [AD application proxy Azure](../manage-apps/application-proxy.md). Их также можно включить с помощью решения контроллера доставки приложений в сети или приложения, используя [безопасные интеграции с партнером по гибридному доступу](../manage-apps/secure-hybrid-access.md).   

   * Выберите поставщика VPN, который поддерживает современную проверку подлинности. Интеграция проверки подлинности с Azure AD. В случае нарушения безопасности в локальной среде вы можете отключить или заблокировать доступ с помощью Azure AD, отключив VPN.

*  **Серверы приложений и рабочих нагрузок**

   * Приложения или ресурсы, необходимые для переноса серверов в инфраструктуру Azure как услуга (IaaS). Используйте [доменные службы Azure AD](../../active-directory-domain-services/overview.md) (Azure AD DS) для отделения доверия и зависимости от локальных экземпляров Active Directory. Для этого убедитесь, что виртуальные сети, используемые для Azure AD DS, не подключены к корпоративным сетям.

   * Следуйте указаниям по [многоуровневой настройке учетных данных](/security/compass/privileged-access-access-model#ADATM_BM). Серверы приложений обычно считаются ресурсами уровня 1.

## <a name="conditional-access-policies"></a>Политики условного доступа

Используйте условный доступ Azure AD для интерпретации сигналов и их использования для принятия решений о проверке подлинности. Дополнительные сведения см. в разделе [план развертывания условного доступа](../conditional-access/plan-conditional-access.md).

* Используйте условный доступ для [блокировки устаревших протоколов проверки подлинности](../conditional-access/howto-conditional-access-policy-block-legacy.md) везде, где это возможно. Кроме того, отключите устаревшие протоколы проверки подлинности на уровне приложения, используя конфигурацию приложения.

   Дополнительные сведения см. в разделе [устаревшие протоколы проверки подлинности](../fundamentals/auth-sync-overview.md). Или ознакомьтесь с подробными сведениями о [Exchange Online](/exchange/clients-and-mobile-in-exchange-online/disable-basic-authentication-in-exchange-online#how-basic-authentication-works-in-exchange-online) и [SharePoint Online](/powershell/module/sharepoint-online/set-spotenant?view=sharepoint-ps).

* Реализуйте Рекомендуемые [конфигурации удостоверений и доступа к устройствам](/microsoft-365/security/office-365-security/identity-access-policies?view=o365-worldwide).

* Если вы используете версию Azure AD, которая не включает условный доступ, убедитесь, что вы используете [Параметры безопасности Azure AD по умолчанию](../fundamentals/concept-fundamentals-security-defaults.md).

   Дополнительные сведения о лицензировании функций Azure AD см. в статье о [ценах на Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="monitor"></a>Монитор 

После настройки среды для защиты Microsoft 365 от локального нарушения безопасности [проследите за работой среды заранее](../reports-monitoring/overview-monitoring.md) .
### <a name="scenarios-to-monitor"></a>Сценарии для мониторинга

В дополнение к любым сценариям, относящимся к вашей организации, отслеживайте следующие ключевые сценарии. Например, вы должны заранее отслеживать доступ к важнейшим бизнес-приложениям и ресурсам.

* **Подозрительные действия** 

    Отслеживайте все [события риска Azure AD](../identity-protection/overview-identity-protection.md#risk-detection-and-remediation) на предмет подозрительных действий. [Защита идентификации Azure AD](../identity-protection/overview-identity-protection.md) изначально интегрирована с центром безопасности Azure.

    Определите сетевые [расположения](../reports-monitoring/quickstart-configure-named-locations.md) , чтобы избежать обнаружения помех в сигналах на основе расположения. 
*  **Предупреждения анализа поведения пользователей и сущностей (УЕБА)** 

    Используйте УЕБА для получения ценной информации об обнаружении аномалий.
    * Microsoft Cloud App Security (МКАС) предоставляет [уеба в облаке](/cloud-app-security/tutorial-ueba).

    * [Локальную уеба можно интегрировать из Azure Advanced Threat protection (ATP)](/defender-for-identity/install-step2). МКАС считывает сигналы из защита идентификации Azure AD. 

* **Действия с учетными записями аварийного доступа** 

    Отслеживайте любой доступ, использующий [учетные записи аварийного доступа](../roles/security-emergency-access.md). Создание оповещений для исследований. Этот мониторинг должен включать: 

   * Входы в систему. 

   * Управление учетными данными. 

   * Любые обновления в отношении членства в группах. 

   * Назначения приложений. 
* **Действие привилегированной роли**

    Настройка и проверка [предупреждений системы безопасности, созданных Azure AD privileged Identity Management (PIM)](../privileged-identity-management/pim-how-to-configure-security-alerts.md?tabs=new#security-alerts).
    Отслеживание прямого назначения привилегированных ролей за пределами PIM за счет создания предупреждений при непосредственном назначении пользователя.

* **Конфигурации на уровне клиента Azure AD**

    Любые изменения в конфигурациях на уровне клиента должны создавать предупреждения в системе. Эти изменения включают, но не ограничиваются:

  *  Обновленные личные домены.  

  * Azure AD B2B изменится на алловлистс и адресов.

  * Azure AD B2B изменится на разрешенных поставщиков удостоверений (поставщиков удостоверений SAML с помощью прямой Федерации или социальных учетных записей).  

  * Изменение политики условного доступа или риска. 

* **Объекты приложения и субъекта-службы в Azure Active Directory (Azure AD)**
    
   * Новые приложения или субъекты-службы, которые могут требовать применения политик условного доступа. 

   * Учетные данные добавлены в субъекты-службы.
   * Действие согласия приложения. 

* **Пользовательские роли**
   * Обновляет определения пользовательских ролей. 

   * Вновь созданные пользовательские роли. 

### <a name="log-management"></a>Управление журналами

Определение хранилища журналов и стратегии хранения, проектирования и реализации для обеспечения единообразия набора средств. Например, можно рассмотреть системы безопасности и управления событиями (SIEM), такие как Azure Sentinel, общие запросы, исследование и судебные модули playbookы.

* **Журналы Azure AD**: прием созданных журналов и сигналов путем согласования рекомендаций по таким параметрам, как диагностика, хранение журналов и SIEM приема. 

    Стратегия журнала должна включать следующие журналы Azure AD:
   * Действия при входе 

   * Журналы аудита 

   * События риска 

    Azure AD предоставляет [Azure Monitor интеграцию](../reports-monitoring/concept-activity-logs-azure-monitor.md) журналов действий входа и журналов аудита. События риска могут быть приняты через [API Microsoft Graph](/graph/api/resources/identityriskevent). Вы можете выполнять [потоковую передачу журналов Azure AD в журналы Azure Monitor](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md).

* **Журналы безопасности операционной системы гибридной инфраструктуры**. все журналы ОС для инфраструктуры гибридной идентификации должны быть архивированы и тщательно отслеживаться как система уровня 0 из-за особенностей области поверхности. Включите следующие элементы: 

   *  Azure AD Connect. Для мониторинга синхронизации удостоверений необходимо развернуть [Azure AD Connect Health](../hybrid/whatis-azure-ad-connect.md) .

   *  Агенты прокси приложения 


   * Агенты обратной записи паролей 

   * Компьютеры шлюза защиты паролей  

   * Серверы политики сети (НПСС) с расширением RADIUS для многофакторной проверки подлинности Azure AD 

## <a name="next-steps"></a>Дальнейшие действия
* [Создание отказоустойчивости в управлении удостоверениями и доступом с помощью Azure AD](resilience-overview.md)

* [Безопасный внешний доступ к ресурсам](secure-external-access-resources.md) 
* [Интеграция всех приложений с Azure AD](five-steps-to-full-application-integration-with-azure-ad.md)