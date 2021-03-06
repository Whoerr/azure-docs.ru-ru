---
title: Базовый план безопасности Azure для Azure Data Box
description: Базовый план безопасности Azure Data Box содержит практические руководства и ресурсы для реализации рекомендаций по безопасности, указанных в статье о производительности системы безопасности Azure.
author: msmbaldwin
ms.service: databox
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 0a05a4692bb4a09213532ed29b2501f6b8056857
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101721996"
---
# <a name="azure-security-baseline-for-azure-data-box"></a>Базовый план безопасности Azure для Azure Data Box

Этот базовый план безопасности применяет рекомендации из [теста безопасности Azure версии 1,0](../security/benchmarks/overview-v1.md) к Azure Data Box. Azure Security Benchmark содержит рекомендации по обеспечению безопасности облачных решений в Azure.
Содержимое группируются по **элементам управления безопасностью** , определенным в производительности системы безопасности Azure, и связанным рекомендациям, применимым к Azure Data Box. **Элементы управления** , неприменимые к Azure Data Box, были исключены.

 
Сведения о том, как Azure Data Box полностью сопоставлены с тестовым показателем безопасности Azure, см. в разделе [полное Azure Data Box файла сопоставления базовых показателей безопасности](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="logging-and-monitoring"></a>Ведение журналов и мониторинг

*Дополнительные сведения см. в статье [производительность системы безопасности Azure: ведение журнала и мониторинг](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1. Использование утвержденных источников синхронизации времени

**Руководство**. Корпорация Майкрософт поддерживает источник времени, используемый для ресурсов Azure, например, для меток времени в журналах. Azure Data Box время может быть смещено, если оно не подключено к сети, которая может получить доступ к NTP-серверу на сайте клиента.

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="22-configure-central-security-log-management"></a>2.2. Настройка централизованного управления журналами безопасности

**Рекомендации**. соответствующие каждому шагу в заказе Data Box, можно выполнить несколько действий по управлению доступом к заказу, аудиту событий, отслеживанию порядка и интерпретации различных формируемых журналов.

- [Общие сведения о мониторинге и ведении журнала событий для Azure Data Box](data-box-logs.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3. Включение журналов аудита для ресурсов Azure

**Рекомендации**. соответствующие каждому шагу в заказе Data Box, можно выполнить несколько действий по управлению доступом к заказу, аудиту событий, отслеживанию порядка и интерпретации различных формируемых журналов.

- [Общие сведения о мониторинге и ведении журнала событий для Azure Data Box](data-box-logs.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="26-monitor-and-review-logs"></a>2,6: мониторинг и просмотр журналов

**Рекомендации**. соответствующие каждому шагу в заказе Data Box, можно выполнить несколько действий по управлению доступом к заказу, аудиту событий, отслеживанию порядка и интерпретации различных формируемых журналов.

- [Общие сведения о мониторинге и ведении журнала событий для Azure Data Box](data-box-logs.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: Включение оповещений для аномальных действий

**Рекомендации**. соответствующие каждому шагу в заказе Data Box, можно выполнить несколько действий по управлению доступом к заказу, аудиту событий, отслеживанию порядка и интерпретации различных формируемых журналов.

- [Общие сведения о мониторинге и ведении журнала событий для Azure Data Box](data-box-logs.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="identity-and-access-control"></a>Идентификатор и управление доступом

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: идентификация и управление доступом](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1. Инвентаризация учетных записей администраторов

**Руководство**. поддержание инвентаризации учетных записей пользователей, имеющих административный доступ к вашей Azure Data Box. Вы можете использовать область управления доступом и удостоверениями (IAM) в портал Azure для подписки, чтобы настроить управление доступом на основе ролей Azure (Azure RBAC). Роли применяются к пользователям, группам, субъектам-службам и управляемым удостоверениям в Active Directory.

Вы можете контролировать, кто имеет доступ к вашему заказу при первом создании заказа. Настройте роли RBAC Azure в различных областях для управления доступом к Data Boxму заказу. Роль RBAC Azure определяет тип доступа — доступ для чтения и записи, доступный только для чтения и для чтения и записи в подмножестве операций.

- [Общие сведения о пользовательских ролях](../role-based-access-control/custom-roles.md)

- [Настройка Azure RBAC для книг](../sentinel/quickstart-get-visibility.md)

- [Узнайте, как настроить контроль доступа в заказе.](https://docs.microsoft.com/azure/databox/data-box-logs#set-up-access-control-on-the-order)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="32-change-default-passwords-where-applicable"></a>3.2. Изменение паролей по умолчанию, где применимо

**Руководство**. Azure Active Directory (Azure AD) не имеет концепции паролей по умолчанию. Другие ресурсы Azure, которым требуется пароль, принудительно запрашивают пароль с требованиями к сложности и минимальной длиной пароля, которая зависит от службы. Вы несете ответственность за сторонние приложения и службы Marketplace, которые могут использовать пароли по умолчанию.

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="33-use-dedicated-administrative-accounts"></a>3.3. Применение выделенных административных учетных записей

**Руководство**. Создайте стандартные операционные процедуры для использования выделенных административных учетных записей. Используйте учетную запись Центра безопасности Azure и управление доступом для мониторинга количества административных учетных записей.

Кроме того, чтобы отслеживать выделенные административные учетные записи, вы можете использовать рекомендации из Центра безопасности Azure или встроенные политики Azure, например следующие:

- Подписке должно быть назначено несколько владельцев.
- Устаревшие учетные записи с разрешениями владельца следует удалять из подписки.
- Внешние учетные записи с разрешениями владельца следует удалять из подписки.

- [Использование Центра безопасности Azure для мониторинга идентификации и доступа (предварительная версия)](../security-center/security-center-identity-access.md)

- [Использование Политики Azure](../governance/policy/tutorials/create-and-manage.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6. Использование выделенных компьютеров (рабочих станций с привилегированным доступом) для всех административных задач

**Руководство**. Использование рабочей станции привилегированного доступа (привилегированным доступом) с включенной многофакторной проверкой подлинности Azure AD для входа и настройки заказов на Azure Data Box.

- [Рабочие станции c привилегированным доступом](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Планирование развертывания на основе многофакторной проверки подлинности в Azure AD](../active-directory/authentication/howto-mfa-getstarted.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: журналы и оповещения о подозрительных действиях учетных записей администраторов

**Руководство**: неприменимо, Azure Data Box не имеет собственной учетной записи администратора. Доступ к нему осуществляется через портал Azure. Однако вы можете настроить подписку Azure для использования Azure Active Directory (Azure AD) управление привилегированными пользователями (PIM) для создания журналов и оповещений при возникновении подозрительных или ненадежных действий в среде.

Кроме того, используйте обнаружение рисков Azure AD для просмотра оповещений и отчетов об опасном поведении пользователя.

- [Развертывание Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Общие сведения об обнаружении рисков в Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8. Управление ресурсами Azure только из утвержденных расположений

**Руководство**. Используйте именованные расположения с условным доступом, чтобы разрешить доступ к порталу Azure только из конкретных логических групп диапазонов IP-адресов или стран и регионов.

- [Настройка именованных расположений в Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="39-use-azure-active-directory"></a>3.9. Использование Azure Active Directory

**Рекомендации**. Используйте Azure Active Directory (Azure AD) в качестве централизованной системы проверки подлинности и авторизации, где это возможно. Azure AD защищает данные с помощью надежного шифрования для хранимых и транзитных данных. Кроме того, в Azure AD используются salt-записи, хэши и безопасное хранение учетных данных пользователей.

- [Создание и настройка экземпляра Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10. Регулярная проверка и согласование доступа пользователей

**Руководство**. Azure Active Directory (Azure AD) предоставляет журналы для упрощения обнаружения устаревших учетных записей. Кроме того, используйте проверку доступа удостоверений Azure, чтобы эффективно управлять членством в группах, доступом к корпоративным приложениям и назначениями ролей. Доступ пользователей можно проверять на регулярной основе, чтобы только у авторизованных пользователей был постоянный доступ.

Для Data Box устройства это не поддерживается в режиме реального времени. Журналы можно просматривать в конце задания.

- [Общие сведения об отчетах Azure AD](/azure/active-directory/reports-monitoring/)

- [Использование проверок доступа для идентификации Azure](../active-directory/governance/access-reviews-overview.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: монитор пытается получить доступ к отключенным учетным данным

**Рекомендации**. Используйте Azure Active Directory (Azure AD) в качестве централизованной системы проверки подлинности и авторизации, где это возможно. Azure AD защищает данные с помощью надежного шифрования для хранимых и транзитных данных. Кроме того, в Azure AD используются salt-записи, хэши и безопасное хранение учетных данных пользователей.

У вас есть доступ к источникам журнала событий входа в Azure AD, аудита и риску, которые позволяют интегрироваться с Azure Sentinel или сторонними SIEM.

Этот процесс можно упростить, создав параметры диагностики для учетных записей пользователей Azure AD и отправив журналы аудита и журналы входа в рабочую область Log Analytics. Вы можете настроить нужные оповещения журнала в Log Analytics.

Azure Data Box журналы служб не записываются в Log Analytics рабочую область.

- [Как интегрировать журналы действий Azure в Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [Подключение к Azure Sentinel](../sentinel/quickstart-onboard.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: предупреждение об отклонении при входе в учетную запись

**Руководство**. сведения об отклонении режима входа учетной записи в плоскости управления (например, портал Azure) Azure Active Directory используйте функции защиты идентификации (Azure AD) и обнаружения рисков, чтобы настроить автоматические ответы на обнаруженные подозрительные действия, связанные с удостоверениями пользователей. Вы также можете включить данные в Azure Sentinel для дальнейшего изучения.

- [Как просмотреть рискованный вход в Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Как настроить и включить политики рисков с помощью защиты идентификации](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Подключение к Azure Sentinel](../sentinel/quickstart-onboard.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13. Предоставление корпорации Майкрософт доступа к соответствующим данным клиентов в рамках сценариев поддержки

**Руководство**: скоро будет доступно; Защищенное хранилище еще не поддерживается для Azure Data Box. 

- [Список поддерживаемых служб защищенного хранилища](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="data-protection"></a>Защита данных

*Дополнительные сведения см. в статье [Azure Security Benchmark: защита данных](../security/benchmarks/security-control-data-protection.md).*

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3. Мониторинг и блокирование несанкционированной передачи конфиденциальной информации

**Руководство**. Корпорация Майкрософт управляет базовой инфраструктурой для Azure Data Box и реализует четкие элементы управления для предотвращения потери или раскрытия данных клиента. Если Data Box находится на сайте клиента, следуйте рекомендациям по обеспечению защиты конфиденциальных данных.

- [Общие сведения о защите данных клиентов в Azure](../security/fundamentals/protection-customer-data.md)

**Ответственность**: Совмещаемая блокировка

**Мониторинг центра безопасности Azure**: нет

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4. Шифрование любой конфиденциальной информации при передаче

**Руководство**. Корпорация Майкрософт управляет базовой инфраструктурой для Azure Data Box и реализует четкие элементы управления для предотвращения потери или раскрытия данных клиента. Если Data Box находится на сайте клиента, следуйте рекомендациям по обеспечению защиты конфиденциальных данных.

- [Общие сведения о переносе данных в Azure Data Box](data-box-faq.md)

- [Общие сведения о безопасности Data Box](data-box-security.md)

**Ответственность**: Совмещаемая блокировка

**Мониторинг центра безопасности Azure**: нет

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6. Контроль доступа к ресурсам с помощью RBAC 

**Руководство**. Убедитесь, что у вас есть права владельца или участника подписки на создание Data Box заказа. Можно также определить Data Box читатель и Data Box роли участников на уровне ресурса.

- [Узнайте, как приступить к работе с Azure Data Box](data-box-quickstart-portal.md)

- [Сведения о настройке контроля доступа](https://docs.microsoft.com/azure/databox/data-box-logs#set-up-access-control-on-the-order)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8. Шифрование конфиденциальной информации при хранении

**Руководство**. Azure Data Box РЕАЛИЗУЕТ шифрование AES 256-bit для неактивных данных.

Azure Data Box реализует шифрование AES 256-bit для неактивных данных.

Кроме того, Azure Data Box защищает ключ разблокировки устройства (также известный как пароль устройства), который используется для блокировки устройства с помощью ключа шифрования. По умолчанию ключ разблокировки устройства для порядка Data Box шифруется с помощью управляемого корпорацией Майкрософт ключа. Для дополнительного контроля над ключом разблокировки устройства можно также предоставить ключ, управляемый клиентом. Управляемые клиентом ключи должны быть созданы и сохранены в Azure Key Vault.
 

- [Общие сведения о защите данных Data Box](data-box-security.md)

- [Использование управляемых клиентом ключей в Azure Key Vault для Azure Data Box](data-box-customer-managed-encryption-key-portal.md)

**Ответственность**: Совмещаемая блокировка

**Мониторинг центра безопасности Azure**: нет

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9. Включение в журнал и создание оповещений по изменениям критических ресурсов Azure

**Руководство**. Использование Azure Monitor с журналом действий Azure для создания оповещений о том, когда изменения выполняются для Azure Data Box, а также для других важных или связанных ресурсов.

- [Создание оповещений для событий журнала действий Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="inventory-and-asset-management"></a>Инвентаризация и управление ресурсами

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: Инвентаризация и управление активами](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: ограничьте возможность пользователей работать с Azure Resource Manager

**Руководство**. Настройте условный доступ Azure, чтобы ограничить возможность пользователей взаимодействовать с Azure Resource Manager путем настройки "Блокировать доступ" для приложения "Управление Microsoft Azure".

- [Как настроить условный доступ для блокировки доступа к Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="secure-configuration"></a>Настройка безопасности

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: безопасная конфигурация](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="711-manage-azure-secrets-securely"></a>7.11. Безопасное управление секретами Azure

**Руководство**. ключи, управляемые клиентом, должны быть созданы и сохранены в Azure Key Vault.

- [Использование управляемых клиентом ключей в Azure Key Vault для Azure Data Box](data-box-customer-managed-encryption-key-portal.md)

**Ответственность**: Совмещаемая блокировка

**Мониторинг центра безопасности Azure**: нет

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13. Устранение непреднамеренного раскрытия учетных данных

**Руководство**. Microsoft запускает средство проверки учетных данных на Data Boxном коде. Кроме того, корпорация Майкрософт также безопасно защищает учетные данные. Реализуйте средство проверки учетных данных для обнаружения учетных данных в коде. Сканер учетных данных также рекомендует перемещать обнаруженные учетные данные в более безопасные расположения, такие как Azure Key Vault.

- [Как настроить сканер учетных данных](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Ответственность**: Совмещаемая блокировка

**Мониторинг центра безопасности Azure**: нет

## <a name="malware-defense"></a>Защита от вредоносных программ

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: защита от вредоносных программ](../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2. Предварительная проверка файлов для отправки в ресурсы Azure, не являющиеся вычислительными

**Руководство**. антивредоносное по Майкрософт включено на базовом узле, поддерживающем службы Azure (например, Azure защищенное хранилище), но не выполняется в содержимом клиента. 

Вы отвечаете за предварительную проверку содержимого, которое передается в ресурсы Azure, не являющиеся ресурсами вычислений. Корпорация Майкрософт не может получить доступ к данным клиентов, поэтому не может проводить проверку содержимого клиентов от вредоносных программ от вашего имени.

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="data-recovery"></a>Восстановление данных

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: восстановление данных](../security/benchmarks/security-control-data-recovery.md).*

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: выполните полное резервное копирование системы и резервное копирование ключей, управляемых клиентом

**Руководство**. обязательно создайте резервные копии всех данных и ключа, управляемого клиентом. Data Box не делает никаких резервных копий.

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: Проверьте все резервные копии, включая управляемые клиентом ключи.

**Руководство**. перед удалением данных из локальной учетной записи необходимо убедиться, что все ваши данные находятся в службе хранилища Azure.

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: Обеспечьте защиту резервных копий и ключей, управляемых клиентом

**Руководство**. Убедитесь, что все резервные копии или ключи, управляемые клиентом, защищены в соответствии с рекомендациями.

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="incident-response"></a>реагирование на инциденты.

*Дополнительные сведения см. в статье [Azure Security Benchmark: реагирование на инциденты](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1. Создание руководства по реагированию на инциденты

**Руководство**. Создайте руководство по реагированию на инциденты для вашей организации. Убедитесь в том, что имеются письменные планы реагирования на инциденты, которые определяют все действия персонала, а также этапы обработки инцидентов и управления ими для проверки после инцидента. 

- [Руководство по созданию собственного процесса реагирования на инциденты безопасности](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Анатомия инцидента Центра Майкрософт по реагированию на угрозы](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Использование руководства по обработке инцидентов безопасности компьютера от NIST для создания собственного плана реагирования на инциденты](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2. Создание процедуры оценки инцидента и определения приоритетов

**Руководство**. Центр безопасности назначает каждому оповещению серьезность, которая поможет определить, какие предупреждения следует расследовать первыми. Серьезность основывается на том, насколько надежным является центр безопасности в поиске или аналитике, используемой для выдаче оповещения, а также об уровне достоверности, который был вредоносным намерением для действия, вызвавшего оповещение. 

Кроме того, четко помечайте подписки тегами (например, рабочие, нерабочие) и создайте систему именования, чтобы четко определить и классифицировать ресурсы Azure, особенно обрабатывающие конфиденциальные данные.  Вы несете ответственность за назначение приоритета оповещениям, требующим действий по исправлению, в зависимости от важности ресурсов Azure и среды, в которой произошел инцидент.

- [Оповещения безопасности в Центре безопасности Azure](../security-center/security-center-alerts-overview.md)

- [Создание и использование тегов](../azure-resource-manager/management/tag-resources.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="103-test-security-response-procedures"></a>10.3. Проверка процедур реагирования на угрозы

**Руководство**. Выполняйте упражнения, чтобы периодически тестировать возможности ваших систем реагировать на угрозы для защиты ресурсов Azure. Выявите слабые точки и пробелов и пересмотрите план по мере необходимости.

- [Публикация NIST — руководство по тестированию, обучению и выполнению программ для ИТ-планов и возможностей](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4. Предоставление контактных сведений и настройка уведомлений по инцидентам безопасности

**Руководство**. Корпорация Майкрософт будет использовать информацию об инциденте безопасности для связи с вами, если центр Microsoft Security Response Center (MSRC) обнаружит, что к вашим данным был получен незаконный или несанкционированный доступ. Проверьте инциденты после факта обращения, чтобы убедиться в том, что проблемы устранены.

- [Как задать контакт безопасности Центра безопасности Azure](../security-center/security-center-provide-security-contact-details.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5. Включение оповещений системы безопасности в систему реагирования на инциденты

**Руководство**. Экспортируйте оповещения и рекомендации Центра безопасности Azure с помощью функции непрерывного экспорта с целью выявления рисков для ресурсов Azure. Непрерывный экспорт позволяет экспортировать предупреждения и рекомендации как вручную, так и в постоянном, непрерывном режиме. Вы можете использовать соединитель данных Центра безопасности Azure для потоковой передачи оповещений в Azure Sentinel.

- [Настройка непрерывного экспорта данных](../security-center/continuous-export.md)

- [Как выполнить потоковую передачу оповещений в Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="106-automate-the-response-to-security-alerts"></a>10.6. Автоматизация реагирования на оповещения системы безопасности

**Руководство**. Использование функции автоматизации рабочих процессов в центре безопасности Azure для автоматического запуска ответов с помощью "Logic Apps" в оповещениях системы безопасности и рекомендациях по защите ресурсов Azure.

- [Как настроить автоматизацию рабочего процесса и Logic Apps](../security-center/workflow-automation.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="penetration-tests-and-red-team-exercises"></a>Тесты на проникновение и попытки нарушения безопасности "красной командой"

*Дополнительные сведения см. в статье [тесты производительности системы безопасности Azure: испытания на проникновение и команды красных команд](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1. Проведите регулярное тестирование на проникновение ресурсов Azure и обеспечьте исправление всех критических результатов безопасности.

**Руководство**. Корпорация Майкрософт выполняет проверку на уязвимости и проверку уязвимостей на Data Box устройствах. Вы можете выполнить собственное тестирование на проникновение и проверку уязвимостей. Если вы решили это сделать, следуйте правилам взаимодействия Майкрософт, чтобы убедиться, что тесты на проникновение не нарушают политики Майкрософт. Используйте стратегию и исполнение Microsoft, а затем протестировать проникновение в реальном времени для управляемой корпорацией Майкрософт облачной инфраструктуры, служб и приложений.

- [Правила взаимодействия при выполнении тестирования на проникновение](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Привлечение "красных команд для тестирования "Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Ответственность**: Совмещаемая блокировка

**Мониторинг центра безопасности Azure**: нет

## <a name="next-steps"></a>Следующие шаги

- См. [Обзор Azure Security Benchmark версии 2](/azure/security/benchmarks/overview)
- Дополнительные сведения о [базовой конфигурации безопасности Azure](/azure/security/benchmarks/security-baselines-overview).
