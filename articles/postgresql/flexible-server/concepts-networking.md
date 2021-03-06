---
title: 'Обзор сети: база данных Azure для PostgreSQL — гибкий сервер'
description: Сведения о вариантах подключения и работы в сети в варианте гибкого развертывания сервера для базы данных Azure для PostgreSQL
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/21/2021
ms.openlocfilehash: a6f049670a6860bbc195b92458945d1a53029b4f
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101732808"
---
# <a name="networking-overview---azure-database-for-postgresql---flexible-server"></a>Обзор сети: база данных Azure для PostgreSQL — гибкий сервер

> [!IMPORTANT]
> Гибкий сервер Базы данных Azure для PostgreSQL предоставляется в режиме предварительной версии

В этой статье описываются концепции подключения и сети для базы данных Azure для PostgreSQL-гибкого сервера. 

## <a name="choosing-a-networking-option"></a>Выбор параметра сети
У вас есть два варианта работы с сетью для базы данных Azure для PostgreSQL-гибкого сервера. Вы можете выбрать **частный доступ (интеграция с виртуальной сетью)** или **общий доступ (разрешенные IP-адреса)** . При создании сервера необходимо выбрать один из вариантов. 

> [!NOTE]
> Невозможно изменить параметр сети после создания сервера. 

* **Частный доступ (интеграция с виртуальной сетью)** . Вы можете развернуть гибкий сервер в [виртуальной сети Azure](../../virtual-network/virtual-networks-overview.md). Виртуальные сети Azure используют частное и безопасное сетевое подключение. Это позволит ресурсам в виртуальной сети взаимодействовать через частные IP-адреса.

   Выберите вариант интеграции с виртуальной сетью, если вам нужны следующие возможности.
   * Подключение из ресурсов Azure в той же виртуальной сети к гибкому серверу с помощью частных IP-адресов.
   * Использование VPN или ExpressRoute для подключения к гибкому серверу со стороны ресурсов, не относящихся к Azure.
   * У гибкого сервера нет общедоступной конечной точки

* **Общий доступ (разрешенные IP-адреса)** — доступ к гибкому серверу осуществляется через общедоступную конечную точку. Общедоступная конечная точка — это общедоступный DNS-адрес. Фраза "разрешенные IP-адреса" относится к диапазону IP-адресов, который вы выбрали для предоставления разрешения на доступ к серверу. Эти разрешения называются **правилами брандмауэра**. 

   Выберите общедоступный метод доступа, если вам нужны следующие возможности:
   * Подключение из ресурсов Azure, которые не поддерживают виртуальные сети
   * Подключение из ресурсов за пределами Azure, которые не подключены с помощью VPN или ExpressRoute 
   * У гибкого сервера есть общедоступная конечная точка

Если вы решили использовать частный доступ или параметр общего доступа, применяются следующие характеристики.
* Подключения с разрешенных IP-адресов должны пройти проверку подлинности на сервере PostgreSQL с действительными учетными данными
* [Шифрование подключения](#tls-and-ssl) доступно для сетевого трафика.
* Сервер имеет полное доменное имя (FQDN). Для свойства HostName в строках подключения рекомендуется использовать полное доменное имя вместо IP-адреса.
* Оба параметра управляют доступом на уровне сервера, а не на уровне базы данных или таблицы. Для управления доступом к базам данных, таблицам и другим объектам можно использовать свойства ролей PostgreSQL.


## <a name="private-access-vnet-integration"></a>Закрытый доступ (интеграция виртуальной сети)
Частный доступ с интеграцией виртуальных сетей (vnet) обеспечивает частное и безопасное взаимодействие для гибкого сервера PostgreSQL.

### <a name="virtual-network-concepts"></a>Основные понятия виртуальной сети
Ниже приведены некоторые основные понятия, с которыми следует ознакомиться при использовании виртуальных сетей с PostgreSQL гибкими серверами.

* **Виртуальная сеть** . Виртуальная сеть Azure содержит пространство частных IP-адресов, настроенное для использования. Дополнительные сведения о виртуальных сетях Azure см. в [обзоре виртуальной сети Azure](../../virtual-network/virtual-networks-overview.md) .

    Виртуальная сеть должна находиться в том же регионе Azure, что и гибкий сервер.


* **Делегированная подсеть** — виртуальная сеть содержит подсети (допуски). Подсети позволяют сегментировать виртуальную сеть в небольшие адресные пространства. Ресурсы Azure развертываются в определенные подсети в виртуальной сети. 

   Гибкий сервер PostgreSQL должен находиться в подсети, **делегированной** только для гибкого использования сервера PostgreSQL. Это делегирование означает, что только гибкие серверы базы данных Azure для PostgreSQL могут использовать эту подсеть. Другие типы ресурсов Azure не могут быть делегированы подсети. Вы делегируйте подсеть, назначив ее свойство делегирования как Microsoft. Дбфорпостгрескл/Флексиблесерверс.

   Добавьте `Microsoft.Storage` в конечную точку службы подсеть, делегированную к гибким серверам. 

* **Группы безопасности сети (NSG)** Правила безопасности в группах безопасности сети позволяют фильтровать тип сетевого трафика, который может подаваться в подсетях виртуальной сети и сетевых интерфейсах, а также из них. Дополнительные сведения [см. в обзоре группы безопасности сети](../../virtual-network/network-security-groups-overview.md) .


### <a name="unsupported-virtual-network-scenarios"></a>Неподдерживаемые сценарии виртуальной сети
* Общедоступная конечная точка (или общедоступный IP-адрес или DNS) — гибкий сервер, развернутый в виртуальной сети, не может иметь общедоступную
* После развертывания гибкого сервера в виртуальной сети и подсети его нельзя переместить в другую виртуальную сеть или подсеть. Невозможно переместить виртуальную сеть в другую группу ресурсов или подписку.
* Размер подсети (адресные пространства) невозможно увеличить после существования ресурсов в подсети
* Пиринг виртуальных сетей между регионами не поддерживается

Узнайте, как создать гибкий сервер с частным доступом (интеграция с виртуальной сетью) в [портал Azure](how-to-manage-virtual-network-portal.md) или [в Azure CLI](how-to-manage-virtual-network-cli.md).

> [!NOTE]
> При использовании пользовательского DNS-сервера необходимо использовать сервер пересылки DNS для разрешения полного доменного имени базы данных Azure для PostgreSQL-гибкого сервера. Дополнительные сведения см. в разделе [разрешение имен, которое использует собственный DNS-сервер](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) .

## <a name="public-access-allowed-ip-addresses"></a>Открытый доступ (разрешенные IP-адреса)
Характеристики общедоступного метода доступа включают:
* Только IP-адреса, которые вы разрешаете, имеют разрешение на доступ к гибкому серверу PostgreSQL. По умолчанию IP-адреса запрещены. Вы можете добавить IP-адреса во время создания сервера или позже.
* Сервер PostgreSQL имеет публично разрешаемое DNS-имя
* Ваш гибкий сервер не входит в одну из виртуальных сетей Azure
* Сетевой трафик к серверу и с него не проходит через частную сеть. Трафик использует общие пути Интернета.

### <a name="firewall-rules"></a>Правила брандмауэра
Предоставление разрешения на IP-адрес называется правилом брандмауэра. Если попытки подключения поступают с IP-адреса, который вы не разрешили, то в исходном клиенте будет отображаться сообщение об ошибке.

Узнайте, как создать гибкий сервер с общедоступным (разрешенным IP-адресом) в [портал Azure](how-to-manage-firewall-portal.md) или [Azure CLI](how-to-manage-firewall-cli.md).


### <a name="allowing-all-azure-ip-addresses"></a>Разрешение всех IP-адресов Azure
Если фиксированный исходящий IP-адрес недоступен для службы Azure, можно рассмотреть возможность включения подключений из всех IP-адресов центра обработки данных Azure.

> [!IMPORTANT]
> Параметр **Разрешить общий доступ из служб и ресурсов Azure в Azure** позволяет настроить брандмауэр для разрешения всех подключений из Azure, включая подключения из подписок других клиентов. При выборе этого параметра убедитесь, что используемое имя для входа и разрешения пользователя предоставляют доступ только авторизованным пользователям.

### <a name="troubleshooting-public-access-issues"></a>Устранение общих проблем доступа
Если доступ к службе сервера базы данных Microsoft Azure для PostgreSQL не работает должным образом, необходимо учитывать следующее:

* **Изменения в списке разрешенных адресов еще не вступили в силу.** До вступления в силу изменений конфигурации брандмауэра сервера базы данных Azure для PostgreSQL может присутствовать задержка до пяти минут.

* **Ошибка проверки подлинности:** Если у пользователя нет разрешений на сервере базы данных Azure для PostgreSQL или используемый пароль неверен, подключение к серверу базы данных Azure для PostgreSQL будет отклонено. Создание параметра брандмауэра предоставляет клиентам возможность попытаться подключиться к серверу. Каждый клиент по-прежнему должен предоставлять необходимые учетные данные безопасности.

* **Динамический IP-адрес клиента:** При наличии подключения к Интернету с динамическим IP-адресом и возникновении проблем с брандмауэром можно попробовать одно из следующих решений.

   * Попросите поставщика услуг Интернета (ISP) назначить диапазон IP-адресов тем клиентским компьютерам, с которых осуществляется доступ к серверу базы данных Azure для PostgreSQL, а затем добавьте диапазон IP-адресов в качестве правила брандмауэра.
   * Получите статические IP-адреса для клиентских компьютеров, а затем добавьте статические IP-адреса как правила брандмауэра.

* **Правило брандмауэра недоступно для формата IPv6:** Правила брандмауэра должны иметь формат IPv4. Если указать правила брандмауэра в формате IPv6, отобразится ошибка проверки.

## <a name="hostname"></a>Hostname (Имя узла)
Независимо от выбранного варианта работы в сети, при подключении к гибкому серверу рекомендуется всегда использовать полное доменное имя в качестве имени узла. IP-адрес сервера не обязательно должен быть статическим. Использование полного доменного имени поможет избежать внесения изменений в строку подключения. 

Пример
* Такую `hostname = servername.postgres.database.azure.com`
* По возможности старайтесь не использовать `hostname = 10.0.0.4` (частный адрес) или `hostname = 40.2.45.67` (общедоступный адрес)


## <a name="tls-and-ssl"></a>TLS и SSL
База данных Azure для PostgreSQL — гибкий сервер поддерживает подключение клиентских приложений к службе PostgreSQL с помощью протокола TLS. TLS — это стандартный промышленный протокол, обеспечивающий шифрование сетевых подключений между сервером базы данных и клиентскими приложениями. TLS — это обновленный протокол SSL (SSL).

База данных Azure для PostgreSQL — гибкий сервер поддерживает только зашифрованные подключения с использованием протокола TLS. Все входящие подключения с TLS 1,0 и TLS 1,1 будут отклонены. 

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте, как создать гибкий сервер с **частным доступом (интеграция с виртуальной сетью)** в [портал Azure](how-to-manage-virtual-network-portal.md) или [в Azure CLI](how-to-manage-virtual-network-cli.md).
* Узнайте, как создать гибкий сервер с общедоступным **(разрешенным IP-адресом)** в [портал Azure](how-to-manage-firewall-portal.md) или [Azure CLI](how-to-manage-firewall-cli.md).
