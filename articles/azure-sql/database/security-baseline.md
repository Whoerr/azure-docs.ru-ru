---
title: Базовый план безопасности Azure для базы данных SQL Azure
description: Базовый план безопасности базы данных SQL Azure содержит рекомендации и ресурсы для реализации рекомендаций по безопасности, указанных в статье о производительности системы безопасности Azure.
author: msmbaldwin
ms.service: sql-database
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 87072ecc4bff054d64c3d8576f821e725959ea7e
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657797"
---
# <a name="azure-security-baseline-for-azure-sql-database"></a>Базовый план безопасности Azure для базы данных SQL Azure

Этот базовый план безопасности применяет рекомендации из [тестового показателя безопасности Azure версии 1,0](../../security/benchmarks/overview.md) к базе данных SQL Azure. Azure Security Benchmark содержит рекомендации по обеспечению безопасности облачных решений в Azure. Содержимое группируются по **элементам управления безопасностью** , определенным в производительности системы безопасности Azure, и связанным рекомендациям, применимым к базе данных SQL Azure. **Элементы управления** , неприменимые к базе данных SQL Azure, были исключены.

Сведения о том, как полностью сопоставить базу данных SQL Azure с тестовым показателем безопасности Azure, см. в [полном файле сопоставления базовых показателей безопасности базы данных SQL Azure](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Безопасность сети

*Дополнительные сведения см. в статье [Azure Security Benchmark: безопасность сети](../../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: защита ресурсов Azure в виртуальных сетях

**Руководство**. Вы можете включить функцию "Частная связь Azure", чтобы разрешить доступ к службам Azure PaaS (например, к базе данных SQL) и размещенным в Azure клиентам или партнерским службам через частную конечную точку в виртуальной сети. Трафик между виртуальной сетью и службой проходит через магистральную сеть Майкрософт, что позволяет избежать рисков общедоступного Интернета. 

Чтобы трафик поступал в базу данных SQL Azure, используйте теги служб SQL, которые разрешают исходящий трафик через механизм групп безопасности сети.

Правила виртуальной сети позволяют базе данных SQL Azure принимать только подключения, отправленные из выбранных подсетей в виртуальной сети.

- [Настройка Приватного канала для Базы данных SQL Azure](/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database)

- [Использование конечных точек службы и правил виртуальной сети для серверов баз данных](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 1.1](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1,2. Мониторинг и запись конфигурации и трафика виртуальных сетей, подсетей и сетевых интерфейсов

**Руководство**. Используйте центр безопасности Azure и исправьте рекомендации по защите сети для подсети, в которой развернут сервер базы данных SQL Azure. 

Для виртуальных машин Azure, которые будут подключаться к экземпляру сервера базы данных SQL Azure, включите журналы потоков группы безопасности сети (NSG) для группы безопасности сети, защищающие эти виртуальные машины, и отправьте журналы в учетную запись хранения Azure для аудита трафика. 

Вы также можете отправить журналы потоков для группы безопасности сети в рабочую область Log Analytics и использовать аналитику трафика для получения ценных сведений о потоке трафика в облаке Azure. Некоторые преимущества Аналитики трафика — это возможность визуализировать сетевые активности и определять горячие участки, выявлять угрозы безопасности, анализировать шаблоны потоков трафика и выявлять некорректные сетевые настройки.

- [Как включить журналы потоков NSG](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Общие сведения о безопасности сети, предоставляемой центром безопасности Azure](../../security-center/security-center-network-recommendations.md)

- [Как включать и использовать Аналитику трафика](../../network-watcher/traffic-analytics.md)

- [Общие сведения о безопасности сети, предоставляемой центром безопасности Azure](../../security-center/security-center-network-recommendations.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: запретите обмен данными с известными вредоносными IP-адресами

**Руководство**. Включение стандарта защиты от атак DDoS в виртуальных сетях, связанных с экземплярами SQL Server, для защиты от атак типа "отказ в обслуживании". Используйте интегрированную аналитику угроз Центра безопасности Azure, чтобы запретить обмен данными с известными вредоносными или неиспользуемыми IP-адресами Интернета.

- [Настройка защиты от атак DDoS](/azure/virtual-network/manage-ddos-protection)

- [Общие сведения об интегрированной аналитике угроз в Центре безопасности Azure](/azure/security-center/security-center-alerts-data-services)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="15-record-network-packets"></a>1,5: запись сетевых пакетов

**Руководство**. для виртуальных машин Azure, которые будут подключаться к экземпляру базы данных SQL Azure, включите журналы потоков для группы безопасности сети (NSG) для группы безопасности сети, защищающие эти виртуальные машины, и отправьте журналы в учетную запись хранения Azure для аудита трафика. Если требуется для изучения аномальных действий, включите запись пакетов наблюдателя за сетями.

- [Как включить журналы потоков NSG](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Как включить Наблюдатель за сетями](../../network-watcher/network-watcher-create.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: развертывание системы обнаружения вторжений на основе сети и предотвращения вторжения (ИДЕНТИФИКАТОРы и IP-адреса)

**Руководство**. Включение расширенной защиты от угроз (ATP) для базы данных SQL Azure.  Пользователи получают оповещения о подозрительных действиях с базами данных, потенциальных уязвимостях, атаках путем внедрения кода SQL и аномальных закономерностей в доступе к базам данных и шаблонам запросов. Кроме того, с помощью центра безопасности Azure вы можете интегрировать оповещения в службу Advanced Threat protection. 

- [Общие сведения и использование расширенной защиты от угроз для базы данных SQL Azure](/azure/sql-database/sql-database-threat-detection-overview)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8. Уменьшение сложности и дополнительных затрат на администрирование в правилах безопасности сети

**Руководство**. Использование тегов службы виртуальной сети для определения элементов управления доступом к сети для групп безопасности сети или брандмауэра Azure. Теги служб можно использовать вместо определенных IP-адресов при создании правил безопасности. Указав имя тега службы (например, ApiManagement) в соответствующем исходном поле или поле назначения правила, можно разрешить или запретить трафик для соответствующей службы. Корпорация Майкрософт управляет префиксами адресов, входящих в тег службы, и автоматически обновляет этот тег при изменении адресов.

При использовании конечных точек службы для базы данных SQL Azure требуется исходящий трафик к общедоступным IP-адресам базы данных SQL Azure. чтобы разрешить подключение, необходимо открыть группы безопасности сети (группы безопасности сети) для прав доступа к базе данных SQL Azure. Это можно сделать с помощью тегов службы NSG для базы данных SQL Azure.

- [Общие сведения о тегах служб с конечными точками службы для базы данных SQL Azure](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview#limitations)

- [Общие сведения и использование тегов служб](../../virtual-network/service-tags-overview.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9. Поддержание стандартных конфигураций безопасности для сетевых устройств

**Руководство**. Определение и реализация конфигураций безопасности сети для экземпляров сервера базы данных SQL Azure с помощью политики Azure. Вы можете использовать пространство имен Microsoft. SQL для определения пользовательских политик или использовать любое из встроенных определений политик, предназначенных для защиты сети сервера базы данных SQL Azure. Пример применимой встроенной политики безопасности сети для сервера базы данных SQL Azure: "SQL Server должен использовать конечную точку службы виртуальной сети".

 

Используйте схемы Azure для упрощения крупномасштабных развертываний Azure с помощью ключевых артефактов среды пакетов, таких как шаблоны управления ресурсами Azure, управление доступом на основе ролей Azure (Azure RBAC) и политики, в одном определении схемы. Простое применение схемы к новым подпискам и средам, а также управление точной настройкой и управление с помощью версионирования.

- [Настройка Политики Azure и управление ею](../../governance/policy/tutorials/create-and-manage.md)

- [Создание схемы Azure](../../governance/blueprints/create-blueprint-portal.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="110-document-traffic-configuration-rules"></a>1.10. Документация по правилам конфигурации трафика

**Руководство**. Использование тегов для групп безопасности сети (NSG) и других ресурсов, относящихся к сетевой безопасности и потоку трафика. Для отдельных правил NSG используйте поле "Описание", чтобы указать бизнес-потребности и длительность (и т. д.) любых правил, которые разрешают трафик в сеть и из нее.

Используйте любое из встроенных определений политик Azure, связанных с тегами, например "требовать тег и его значение", чтобы убедиться, что все ресурсы созданы с помощью тегов и уведомлять вас о существующих ресурсах без тегов.

Вы можете использовать Azure PowerShell или Azure CLI для поиска или выполнения действий с ресурсами на основе их тегов.

- [Создание и использование тегов](/azure/azure-resource-manager/resource-group-using-tags)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11. Использование автоматизированных средств для мониторинга конфигураций сетевых ресурсов и обнаружения изменений

**Руководство**. Использование журнала действий Azure для мониторинга конфигураций сетевых ресурсов и обнаружения изменений сетевых ресурсов, связанных с экземплярами сервера базы данных SQL Azure. Создавайте оповещения в Azure Monitor, которые будут запускаться при изменении критических сетевых ресурсов.

- [Как просматривать и извлекать события журнала действий Azure](/azure/azure-monitor/platform/activity-log-view)

- [Как создать оповещения в службе Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="logging-and-monitoring"></a>Ведение журналов и мониторинг

*Дополнительные сведения см. в статье [производительность системы безопасности Azure: ведение журнала и мониторинг](../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2. Настройка централизованного управления журналами безопасности

**Руководство**. Включение аудита для базы данных SQL Azure для мониторинга событий базы данных и записи их в журнал аудита в учетной записи хранения Azure, log Analytics рабочей области или концентраторах событий.

Кроме того, вы можете передавать данные телеметрии диагностики SQL Azure в Аналитика SQL Azure, облачное решение, которое отслеживает производительность баз данных SQL Azure и управляемых экземпляров SQL Azure в масштабе и нескольких подписках. Решение поможет собирать и визуализировать показатели производительности Базы данных SQL Azure, а также обладает встроенными интеллектуальными возможностями для устранения неполадок производительности.

- [Настройка аудита для базы данных SQL Azure](/azure/sql-database/sql-database-auditing)

- [Как получить журналы и метрики платформы с помощью Azure Monitor](/azure/sql-database/sql-database-metrics-diag-logging)

- [Потоковая передача диагностики в Аналитика SQL Azure](/azure/sql-database/sql-database-metrics-diag-logging#stream-into-azure-sql-analytics)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3. Включение журналов аудита для ресурсов Azure

**Руководство**. Включите аудит на экземпляре сервера базы данных SQL Azure и выберите место хранения журналов аудита (служба хранилища Azure, log Analytics или концентратор событий).

- [Включение аудита для SQL Server Azure](/azure/sql-database/sql-database-auditing)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 2.3](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-2-3.md)]

### <a name="25-configure-security-log-storage-retention"></a>2.5. Настройка хранения журнала безопасности

**Руководство**. при хранении журналов базы данных SQL Azure в log Analytics рабочей области задайте срок хранения журнала согласно нормативным требованиям Организации.

- [Настройка параметров хранения журнала](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 2.5](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-2-5.md)]

### <a name="26-monitor-and-review-logs"></a>2,6: мониторинг и просмотр журналов

**Руководство**. анализ и мониторинг журналов для аномальных поведений и регулярная проверка результатов. Используйте расширенную защиту от угроз в центре безопасности Azure для оповещения о необычных действиях, связанных с экземпляром базы данных SQL Azure. Кроме того, можно настроить оповещения на основе значений метрик или записей журнала действий Azure, связанных с экземплярами базы данных SQL Azure.

- [Общие сведения о расширенной защите от угроз и предупреждениях для Azure SQL Server](/azure/sql-database/sql-database-threat-detection-overview)

- [Настройка пользовательских предупреждений для базы данных SQL Azure](alerts-insights-configure-portal.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: Включение оповещений для аномальных действий

**Руководство**. Использование Advanced Threat Protection в центре безопасности Azure для баз данных SQL Azure для мониторинга и оповещения о аномальных действиях. Включите защитник Azure для SQL для баз данных SQL. Защитник Azure для SQL включает функции обнаружения и классификации конфиденциальных данных, отображая и устранения потенциальных уязвимостей баз данных, а также обнаружения аномальных действий, которые могут указывать на угрозу для базы данных.

- [Общие сведения о расширенной защите от угроз и предупреждениях для базы данных SQL Azure](/azure/sql-database/sql-database-threat-detection-overview)

- [Как включить защитник Azure для SQL для базы данных SQL Azure](azure-defender-for-sql.md)

- [Управление оповещениями в центре безопасности Azure](../../security-center/security-center-managing-and-responding-alerts.md)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 2.7](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-2-7.md)]

## <a name="identity-and-access-control"></a>Идентификатор и управление доступом

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: идентификация и управление доступом](../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1. Инвентаризация учетных записей администраторов

**Руководство**. Azure Active Directory (Azure AD) содержит встроенные роли, которые должны быть явно назначены и доступны для запросов. Используйте модуль Azure AD PowerShell для выполнения специальных запросов для обнаружения учетных записей, входящих в группы администраторов.

- [Как получить роль каталога в Azure AD с помощью PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Как получить членов роли каталога в Azure AD с помощью PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="32-change-default-passwords-where-applicable"></a>3.2. Изменение паролей по умолчанию, где применимо

**Руководство**. Azure Active Directory (Azure AD) не имеет концепции паролей по умолчанию. При подготовке экземпляра базы данных SQL Azure рекомендуется выбрать интеграцию аутентификации с Azure AD.

- [Настройка аутентификации Azure AD и управление ею с помощью Azure SQL](/azure/sql-database/azure-sql/database/authentication-aad-configure)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="33-use-dedicated-administrative-accounts"></a>3.3. Применение выделенных административных учетных записей

**Руководство**. Создание политик и процедур, связанных с использованием выделенных административных учетных записей. Используйте учетную запись Центра безопасности Azure и управление доступом для мониторинга количества административных учетных записей.

- [Общие сведения об удостоверениях и доступе центра безопасности Azure](../../security-center/security-center-identity-access.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: используйте многофакторную проверку подлинности для всех Azure Active Directory доступа

**Руководство**. Включение многофакторной проверки подлинности в Azure Active Directory (Azure AD) и следование рекомендациям по управлению удостоверениями и доступом в центре безопасности Azure.

- [Как включить многофакторную проверку подлинности в Azure](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Мониторинг идентификации и доступа в Центре безопасности Azure](../../security-center/security-center-identity-access.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3,6: используйте защищенные рабочие станции, управляемые Azure, для административных задач

**Руководство**. Использование рабочей станции привилегированного доступа (привилегированным доступом) с многофакторной проверкой подлинности, настроенной для входа и настройки ресурсов Azure.

- [Использование рабочих станций с привилегированным доступом](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Как включить многофакторную проверку подлинности в Azure](../../active-directory/authentication/howto-mfa-getstarted.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: журналы и оповещения о подозрительных действиях учетных записей администраторов

**Руководство**. Использование отчетов по безопасности Azure Active Directory (Azure AD) для создания журналов и оповещений при возникновении подозрительных или ненадежных действий в среде.

Используйте расширенную защиту от угроз для базы данных SQL Azure, чтобы обнаружить аномальные действия, указывающие на необычные и потенциально опасные попытки доступа к базам данных или их использования.

- [Как определить пользователей Azure AD, помеченных для события риска](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Как отслеживать активность удостоверений и доступа пользователей в центре безопасности Azure](../../security-center/security-center-identity-access.md)

- [Обзор расширенной защиты от угроз и потенциальных оповещений](https://docs.microsoft.com/azure/azure-sql/database/threat-detection-overview#alerts)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8. Управление ресурсами Azure только из утвержденных расположений

**Руководство**. Используйте условный доступ с именованными расположениями, чтобы разрешить доступ к порталу и управлению ресурсами Azure только из конкретных логических групп диапазонов IP-адресов или стран и регионов.

- [Настройка именованных расположений в Azure](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="39-use-azure-active-directory"></a>3.9. Использование Azure Active Directory

**Руководство**. создание администратора Azure Active Directory (Azure AD) для экземпляров сервера базы данных SQL Azure.

- [Настройка аутентификации Azure AD и управление ею с помощью Azure SQL](authentication-aad-configure.md)

- [Создание и настройка экземпляра Azure AD](../../active-directory-domain-services/tutorial-create-instance.md)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 3.9](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-3-9.md)]

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10. Регулярная проверка и согласование доступа пользователей

**Руководство**. Azure Active Directory (Azure AD) предоставляет журналы для облегчения поиска устаревших учетных записей. Кроме того, используйте проверку доступа удостоверений Azure, чтобы эффективно управлять членством в группах, доступом к корпоративным приложениям и назначениями ролей. Доступ пользователей можно регулярно проверять, чтобы убедиться, что доступ к ним имеют только нужные пользователи.

- [Использование проверок доступа для идентификации Azure](../../active-directory/governance/access-reviews-overview.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: монитор пытается получить доступ к отключенным учетным данным

**Руководство**. Настройка проверки подлинности Azure Active Directory (Azure AD) с помощью Azure SQL и создание параметров диагностики для учетных записей пользователей Azure AD с последующим отправкой журналов аудита и журналов входа в рабочую область log Analytics. Настройте нужные оповещения в Log Analytics рабочей области.

- [Настройка аутентификации Azure AD и управление ею с помощью Azure SQL](authentication-aad-configure.md)

- [Как интегрировать журналы действий Azure в Azure Monitor](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: предупреждение об отклонении при входе в учетную запись

**Руководство**. Использование Azure Active Directory (Azure AD) для защиты идентификации и выявления рисков для настройки автоматических ответов на обнаруженные подозрительные действия, связанные с удостоверениями пользователей. Кроме того, вы можете принимать данные в метку Azure для дальнейшего изучения.

- [Просмотр входа в систему с рисками Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Как настроить и включить политики рисков с помощью защиты идентификации](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13. Предоставление корпорации Майкрософт доступа к соответствующим данным клиентов в рамках сценариев поддержки

**Руководство**. в сценариях поддержки, в которых корпорации Майкрософт требуется доступ к данным клиентов, защищенное хранилище Azure предоставляет пользователям интерфейс для просмотра и утверждения или отклонения запросов на доступ к данным клиентов.

- [Общие сведения о защищенное хранилище](../../security/fundamentals/customer-lockbox-overview.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="data-protection"></a>Защита данных

*Дополнительные сведения см. в статье [Azure Security Benchmark: защита данных](../../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1. Инвентаризация конфиденциальных данных

**Руководство**. Используйте теги для пометки ресурсов Azure, в которых хранятся или обрабатываются конфиденциальные данные.

- [Создание и использование тегов](/azure/azure-resource-manager/resource-group-using-tags)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 4.1](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-4-1.md)]

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2. Изолирование систем, хранящих или обрабатывающих конфиденциальные данные

**Руководство**. Реализуйте отдельные подписки и группы управления для разработки, тестирования и производства. Ресурсы должны быть разделены виртуальной сетью или подсетью, помечены соответствующим образом и защищены в брандмауэре NSG или Azure. Ресурсы, которые хранят или обрабатывают конфиденциальные данные, должны быть изолированы. Использовать частную ссылку; развертывание SQL Server Azure в виртуальной сети и подключение к частным конечным точкам в частном порядке.

- [Создание дополнительных подписок Azure](/azure/billing/billing-create-subscription)

- [Создание групп управления](/azure/governance/management-groups/create)

- [Создание и использование тегов](/azure/azure-resource-manager/resource-group-using-tags)

- [Настройка Приватного канала для Базы данных SQL Azure](/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3. Мониторинг и блокирование несанкционированной передачи конфиденциальной информации

**Руководство**. для баз данных в базе данных SQL Azure хранение или обработка конфиденциальной информации пометьте базу данных и связанные ресурсы как конфиденциальные с помощью тегов. Настройте закрытую связь в сочетании с тегами службы групп безопасности сети в экземплярах базы данных SQL Azure, чтобы предотвратить утечка конфиденциальной информации.

Для базовой платформы, управляемой корпорацией Майкрософт, корпорация Майкрософт считает все содержимое клиента конфиденциальным и предпринимает все возможные усилия для защиты клиентов от потери данных и раскрытия информации. Чтобы обеспечить безопасность данных клиентов в Azure, корпорация Майкрософт реализовала и поддерживает набор надежных элементов управления и возможностей защиты данных.

- [Настройка частных ссылок и группы безопасности сети для предотвращения утечка данных в экземплярах базы данных SQL Azure](/azure/sql-database/sql-database-private-endpoint-overview)

- [Общие сведения о защите данных клиентов в Azure](../../security/fundamentals/protection-customer-data.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5. Использование средства активного обнаружения для распознавания конфиденциальных данных

**Руководство**. Использование функции обнаружения и классификации данных в базе данных SQL Azure. Обнаружение и классификация данных предоставляет расширенные возможности, встроенные в базу данных SQL Azure, для обнаружения, классификации и пометки &amp; защиты конфиденциальных данных в базах данных.

- [Как использовать обнаружение и классификацию данных для Azure SQL Server](/azure/sql-database/sql-database-data-discovery-and-classification)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 4.5](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-4-5.md)]

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6. Использование управления доступом на основе ролей для управления доступом к ресурсам

**Руководство**. Использование Azure Active Directory (Azure AD) для проверки подлинности и управления доступом к экземплярам базы данных SQL Azure.

- [Интеграция SQL Server Azure с Azure AD для проверки подлинности](/azure/sql-database/sql-database-aad-authentication)

- [Управление доступом в Azure SQL Server](/azure/sql-database/sql-database-control-access)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8. Шифрование конфиденциальной информации при хранении

**Руководство**. прозрачное шифрование данных (TDE) помогает защитить базу данных SQL Azure, управляемый экземпляр SQL Azure и хранилище данных Azure от угроз вредоносной автономной активности путем шифрования неактивных данных. Выполняется шифрование и расшифровка базы данных, связанных резервных копий и неактивных файлов журналов транзакций в реальном времени без необходимости изменения приложения. По умолчанию TDE включается для всех вновь развернутых баз данных SQL Server и SQL Управляемый экземпляр. Ключ шифрования TDE может управляться как корпорацией Майкрософт, так и клиентом.

- [Управление прозрачным шифрованием данных и использование собственных ключей шифрования](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal#manage-transparent-data-encryption)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 4.8](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-4-8.md)]

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9. Включение в журнал и создание оповещений по изменениям критических ресурсов Azure

**Руководство**. Использование Azure Monitor с журналом действий Azure для создания оповещений о том, когда изменения выполняются в рабочих экземплярах базы данных SQL Azure и других критических или связанных ресурсах.

- [Создание оповещений для событий журнала действий Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="vulnerability-management"></a>Управление уязвимостями

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: Управление уязвимостью](../../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1. Выполнение автоматизированных средства анализа уязвимостей

**Рекомендации**. Включите защитник Azure для SQL для базы данных SQL Azure и следуйте рекомендациям центра безопасности Azure по выполнению оценки уязвимостей на серверах Azure SQL.

- [Как запустить оценку уязвимостей в базе данных SQL Azure](/azure/sql-database/sql-vulnerability-assessment)

- [Как включить защитник Azure для SQL](azure-defender-for-sql.md)

- [Реализация рекомендаций по оценке уязвимостей в центре безопасности Azure](/azure/security-center/security-center-vulnerability-assessment-recommendations)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 5.1](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-5-1.md)]

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4. Сравнение проверок смежных уязвимостей

**Руководство**. Включение периодических повторяющихся проверок для экземпляров базы данных SQL Azure; будет настроена Оценка уязвимостей для автоматического выполнения проверки базы данных в течение недели. Сводка результатов отправляется ​​на указанный адрес электронной почты. Сравните результаты, чтобы убедиться, что уязвимости исправлены.

- [Экспорт отчета об оценке уязвимостей в центре безопасности Azure](/azure/sql-database/sql-vulnerability-assessment#implementing-vulnerability-assessment)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5. Использование процесса оценки рисков для определения приоритета в устранении обнаруженных уязвимостей

**Руководство**. Используйте оценки рисков по умолчанию (Оценка безопасности), предоставляемые центром безопасности Azure.

- [Оценка безопасности центра безопасности Azure](/azure/security-center/security-center-secure-score)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 5.5](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-5-5.md)]

## <a name="inventory-and-asset-management"></a>Инвентаризация и управление ресурсами

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: Инвентаризация и управление активами](../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1. Использование автоматизированного решения для обнаружения ресурсов

**Руководство**. Использование графа ресурсов Azure для запроса и обнаружения всех ресурсов (включая экземпляры SQL Server Azure) в ваших подписках. Убедитесь, что у вас есть соответствующие разрешения (на чтение) в клиенте и вы можете перечислить все подписки Azure, а также ресурсы в ваших подписках.

Хотя классические ресурсы Azure можно обнаружить через Resource Graph, настоятельно рекомендуется в дальнейшем создавать и использовать ресурсы Azure Resource Manager.

- [Как создавать запросы с помощью Azure Resource Graph](../../governance/resource-graph/first-query-portal.md)

- [Как просматривать подписки Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Общие сведения об Azure RBAC](../../role-based-access-control/overview.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="62-maintain-asset-metadata"></a>6.2. Ведение метаданных активов

**Руководство**. Применяйте к ресурсам Azure теги, чтобы логически классифицировать их на основе метаданных.

- [Как создавать и использовать теги](/azure/azure-resource-manager/resource-group-using-tags)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="63-delete-unauthorized-azure-resources"></a>6.3. Удаление неавторизованных ресурсов Azure

**Руководство**. Использование тегов, групп управления и отдельных подписок (при необходимости) для Организации и мониторинга ресурсов. Регулярно сверяйте ресурсы, чтобы своевременно удалять неавторизованные ресурсы из подписки.

- [Создание дополнительных подписок Azure](/azure/billing/billing-create-subscription)

- [Создание групп управления](/azure/governance/management-groups/create)

- [Создание и использование тегов](/azure/azure-resource-manager/resource-group-using-tags)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5. Отслеживание неутвержденных ресурсов Azure

**Рекомендации**. Используйте политику Azure, чтобы ограничить тип ресурсов, которые могут быть созданы в подписках клиентов, используя следующие встроенные определения политик:

- Недопустимые типы ресурсов

- Допустимые типы ресурсов

Используйте граф ресурсов Azure для запроса или обнаружения ресурсов в ваших подписках. Убедитесь в том, что все ресурсы Azure, представленные в среде, утверждены.

- [Настройка Политики Azure и управление ею](../../governance/policy/tutorials/create-and-manage.md)

- [Как создавать запросы с помощью Azure Resource Graph](../../governance/resource-graph/first-query-portal.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="69-use-only-approved-azure-services"></a>6.9. Использование только утвержденных служб Azure

**Руководство**. Использование политики Azure для размещения ограничений на тип ресурсов, которые могут быть созданы в подписках клиента с помощью следующих встроенных определений политик:
- Недопустимые типы ресурсов
- Допустимые типы ресурсов

Используйте граф ресурсов Azure для запроса или обнаружения ресурсов в ваших подписках. Убедитесь в том, что все ресурсы Azure, представленные в среде, утверждены.

- [Настройка Политики Azure и управление ею](../../governance/policy/tutorials/create-and-manage.md)

- [Как отказаться от определенного типа ресурса с помощью Политики Azure](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: ограничьте возможность пользователей работать с Azure Resource Manager

**Руководство**. Используйте условный доступ Azure, чтобы ограничить возможность пользователей взаимодействовать с Azure Resource Manager путем настройки "Блокировать доступ" для приложения "Управление Microsoft Azure".

- [Как настроить условный доступ для блокировки доступа к Azure Resource Manager](../../role-based-access-control/conditional-access-azure-management.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="secure-configuration"></a>Настройка безопасности

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: безопасная конфигурация](../../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1. Установка безопасных конфигураций для всех ресурсов Azure

**Руководство**. Используйте рекомендации по использованию политики Azure или центра безопасности Azure для серверов и баз данных SQL Azure, чтобы поддерживать конфигурации безопасности для всех ресурсов Azure.

- [Настройка Политики Azure и управление ею](../../governance/policy/tutorials/create-and-manage.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3. Сохранение безопасных конфигураций для ресурсов Azure

**Рекомендации**. Используйте Политику Azure [отказывать] и [развернуть, если не существует], чтобы обеспечить безопасность параметров в ресурсах Azure.

- [Настройка Политики Azure и управление ею](../../governance/policy/tutorials/create-and-manage.md)

- [Сведения о действии Политик Azure](../../governance/policy/concepts/effects.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5. Безопасное хранение конфигурации ресурсов Azure

**Руководство**. Если вы используете пользовательские определения политики Azure, используйте Azure DevOps или Azure Repos для безопасного хранения кода и управления им.

- [Как хранить код в Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Документация по Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: развертывание средств управления конфигурацией для ресурсов Azure

**Руководство**. Использование псевдонимов политик Azure в пространстве имен Microsoft. SQL для создания настраиваемых политик для оповещения, аудита и принудительного применения конфигураций системы. Кроме того, разрабатывайте процесс и конвейер для управления исключениями политик.

- [Настройка Политики Azure и управление ею](../../governance/policy/tutorials/create-and-manage.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9. Реализация автоматического мониторинга конфигурации для ресурсов Azure

**Руководство**. Использование центра безопасности Azure для выполнения проверок базовых показателей для серверов и баз данных SQL Azure.

- [Как исправить рекомендации в центре безопасности Azure](/azure/security-center/security-center-sql-service-recommendations)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="711-manage-azure-secrets-securely"></a>7.11. Безопасное управление секретами Azure

**Руководство**. Использование Azure Key Vault для хранения ключей шифрования для прозрачное шифрование данных базы данных SQL Azure (TDE).

- [Как защитить конфиденциальные данные, хранящиеся в SQL Server Azure, и хранить ключи шифрования в Azure Key Vault](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="712-manage-identities-securely-and-automatically"></a>7.12. Безопасное и автоматическое управление удостоверениями

**Руководство**. Использование управляемых удостоверений для предоставления служб Azure с автоматически управляемым удостоверением в Azure Active Directory (Azure AD). Управляемые удостоверения позволяют проходить проверку подлинности в любой службе, поддерживающей проверку подлинности Azure AD, включая Azure Key Vault без каких бы то ни было учетных данных в коде.

- [Руководство по Использование назначаемого системой управляемого удостоверения на виртуальной машине Windows для доступа к SQL Azure](../../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql.md)

- [Настройка управляемых удостоверений](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13. Устранение непреднамеренного раскрытия учетных данных

**Руководство**. Реализация средства проверки учетных данных для указания учетных данных в коде. Сканер учетных данных также рекомендует перемещать обнаруженные учетные данные в более безопасные расположения, такие как Azure Key Vault. 

- [Как настроить сканер учетных данных](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="malware-defense"></a>Защита от вредоносных программ

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: защита от вредоносных программ](../../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2. Предварительная проверка файлов для отправки в ресурсы Azure, не являющиеся вычислительными

**Руководство**. Защита от вредоносных программ Майкрософт включена на базовом узле, поддерживающем службы Azure (например, в службе приложений Azure), но не выполняется в содержимом клиента.

Предварительное сканирование любого содержимого, отправляемого в Нерасчетные ресурсы Azure, например служба приложений, Data Lake Storage, хранилище BLOB-объектов, SQL Server Azure и т. д. Корпорация Майкрософт не может получить доступ к данным в этих экземплярах.

- [Сведения о антивредоносном по Майкрософт для облачных служб и виртуальных машин Azure](../../security/fundamentals/antimalware.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="data-recovery"></a>Восстановление данных

*Дополнительные сведения см. в статье о [производительности системы безопасности Azure: восстановление данных](../../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: Обеспечьте регулярное автоматическое резервное копирование

**Рекомендации**. чтобы защитить бизнес от потери данных, база данных SQL Azure автоматически создает полные резервные копии баз данных еженедельно, разностные резервные копии каждые 12 часов, а резервные копии журналов транзакций — каждые 5-10 минут. Резервные копии хранятся в хранилище RA-GRS по крайней мере за 7 дней для всех уровней служб. Все уровни служб, за исключением поддержка Basic настраиваемого срока хранения резервных копий для восстановления до точки во времени, до 35 дней.

Для удовлетворения различных требований соответствия можно выбрать разные периоды хранения для еженедельных, ежемесячных и (или) ежегодных резервных копий. Использование хранилища зависит от частоты резервного копирования и периода хранения.

- [Общие сведения о резервном копировании и непрерывности бизнес-процессов с помощью Azure SQL Server](/azure/sql-database/sql-database-business-continuity)

**Ответственность**: Совмещаемая блокировка

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 9.1](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-9-1.md)]

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: выполните полное резервное копирование системы и резервное копирование ключей, управляемых клиентом

**Руководство**. база данных SQL Azure автоматически создает резервные копии базы данных, которые хранятся в интервале от 7 до 35 дней, и использует геоизбыточное хранилище с доступом на чтение Azure (RA-GRS), чтобы гарантировать их сохранность, даже если центр обработки данных недоступен. Эти резервные копии создаются автоматически. При необходимости включите долгосрочные геоизбыточные резервные копии для базы данных SQL Azure.

Если для прозрачное шифрование данных используются управляемые клиентом ключи, убедитесь, что резервное копирование ключей выполняется.

- [Общие сведения о резервном копировании в Azure SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups?tabs=single-database)

- [Как создать резервную копию ключей хранилища ключей в Azure](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Ответственность**: Customer

**Мониторинг в центре безопасности Azure. производительность** [системы безопасности Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) — это инициатива политики по умолчанию для центра безопасности, которая является основой для [рекомендаций центра безопасности](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Определения политик Azure, связанные с этим элементом управления, автоматически включаются центром безопасности. Для оповещений, связанных с этим элементом управления, может потребоваться план [защитника Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) для связанных служб.

**Встроенные определения политики Azure — Microsoft. SQL**:

[!INCLUDE [Resource Policy for Microsoft.Sql 9.2](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-9-2.md)]

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: Проверьте все резервные копии, включая управляемые клиентом ключи.

**Руководство**. обеспечение возможности периодически выполнять восстановление данных в Azure Backup. При необходимости протестируйте содержимое для восстановления изолированной виртуальной ЛС. Проверка восстановления резервных копий ключей, управляемых клиентом.

- [Как восстановить резервную копию ключей хранилища ключей в Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

- [Как восстановить резервные копии базы данных SQL Azure с помощью восстановления до точки во времени](/azure/sql-database/sql-database-recovery-using-backups#point-in-time-restore)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: Обеспечьте защиту резервных копий и ключей, управляемых клиентом

**Руководство**. Включение обратимого удаления в Azure Key Vault для защиты ключей от случайного или вредоносного удаления.

- [Включение обратимого удаления в Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="incident-response"></a>реагирование на инциденты.

*Дополнительные сведения см. в статье [Azure Security Benchmark: реагирование на инциденты](../../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1. Создание руководства по реагированию на инциденты

**Руководство**. Убедитесь, что существуют планы реагирования на инциденты, определяющие роли персонала, а также этапы обработки инцидентов и управления ими.

- [Как настроить автоматизацию рабочих процессов в Центре безопасности Azure](../../security-center/security-center-planning-and-operations-guide.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2. Создание процедуры оценки инцидента и определения приоритетов

**Руководство**. Центр безопасности назначает серьезность предупреждениям, чтобы помочь вам определить порядок, в котором вы задаете каждое оповещение, чтобы при компрометации ресурса вы могли сразу перейти к нему. Серьезность основывается на том, насколько надежным является центр безопасности в поиске или аналитике, используемой для выдаче оповещения, а также об уровне достоверности, который был вредоносным намерением для действия, вызвавшего оповещение.

- [Оповещения безопасности в Центре безопасности Azure](../../security-center/security-center-alerts-overview.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="103-test-security-response-procedures"></a>10.3. Проверка процедур реагирования на угрозы

**Рекомендации**. Выполните упражнения, чтобы периодически протестировать возможности ваших систем реагировать на угрозы. Выявите слабые точки и пробелов и пересмотрите план по мере необходимости.

- [Вы можете обратиться к публикации NIST: руководство по тестированию, обучению и упражнениям в плане ИТ-планов и возможностей.](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4. Предоставление контактных сведений и настройка уведомлений по инцидентам безопасности

**Руководство**. контактные данные инцидентов безопасности будут использоваться корпорацией Майкрософт для связи с вами, если Microsoft Security Response Center (MSRC) обнаруживает, что доступ к данным был получен незаконные или неавторизованной стороной.

- [Как задать контакт безопасности Центра безопасности Azure](../../security-center/security-center-provide-security-contact-details.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5. Включение оповещений системы безопасности в систему реагирования на инциденты

**Рекомендации**. Экспортируйте оповещения и рекомендации центра безопасности Azure с помощью функции непрерывного экспорта. Непрерывный экспорт позволяет экспортировать предупреждения и рекомендации как вручную, так и в постоянном, непрерывном режиме. Вы можете использовать соединитель данных Центра безопасности Azure для потоковой передачи оповещений в Azure Sentinel.

- [Настройка непрерывного экспорта данных](../../security-center/continuous-export.md)

- [Как выполнить потоковую передачу оповещений в Azure Sentinel](../../sentinel/connect-azure-security-center.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

### <a name="106-automate-the-response-to-security-alerts"></a>10.6. Автоматизация реагирования на оповещения системы безопасности

**Руководство**. Используйте функцию автоматизации рабочих процессов в Центре безопасности Azure для автоматического запуска реагирования с помощью Logic Apps в оповещениях и рекомендациях системы безопасности.

- [Как настроить автоматизацию рабочего процесса и Logic Apps](../../security-center/workflow-automation.md)

**Ответственность**: Customer

**Мониторинг центра безопасности Azure**: нет

## <a name="penetration-tests-and-red-team-exercises"></a>Тесты на проникновение и попытки нарушения безопасности "красной командой"

*Дополнительные сведения см. в статье [тесты производительности системы безопасности Azure: испытания на проникновение и команды красных команд](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1. Проведите регулярное тестирование на проникновение ресурсов Azure и обеспечьте исправление всех критических результатов безопасности.

**Рекомендации**. Следуйте правилам тестирования уязвимости Microsoft Cloud, чтобы убедиться, что тесты на проникновение не нарушают политики Майкрософт. Используйте стратегию Майкрософт и рекомендации "красных команд", а затем выполните тест на проникновение в режиме реального времени для управляемых корпорацией Майкрософт облачной инфраструктуры, служб и приложений. 

- [Правила взаимодействия при выполнении тестирования на проникновение](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Привлечение "красных команд для тестирования "Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Ответственность**: Совмещаемая блокировка

**Мониторинг центра безопасности Azure**: нет

## <a name="next-steps"></a>Следующие шаги

- См. [Обзор Azure Security Benchmark версии 2](/azure/security/benchmarks/overview)
- Дополнительные сведения о [базовой конфигурации безопасности Azure](/azure/security/benchmarks/security-baselines-overview).
