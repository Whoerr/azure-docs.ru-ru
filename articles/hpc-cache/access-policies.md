---
title: Использование политик доступа в кэше Azure HPC
description: Создание и применение пользовательских политик доступа для ограничения клиентского доступа к целевым объектам хранилища в кэше Azure HPC
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 12/28/2020
ms.author: v-erkel
ms.openlocfilehash: 795b194eb7cd31e633128c22ddffe808b32e07da
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/29/2020
ms.locfileid: "97802424"
---
# <a name="use-client-access-policies"></a>Использование политик клиентского доступа

В этой статье объясняется, как создать и применить настраиваемые политики клиентского доступа для целевых объектов хранилища.

Политики клиентского доступа определяют, как клиенты могут подключаться к экспортам целевого объекта хранилища. Вы можете управлять такими объектами, как root squash и доступом на чтение и запись на узле клиента или на уровне сети.

Политики доступа применяются к пути к пространству имен. Это означает, что можно использовать разные политики доступа для двух различных операций экспорта в системе хранения NFS.

Эта функция предназначена для рабочих процессов, где необходимо контролировать, как разные группы клиентов обращаются к целевым объектам хранилища.

Если вам не требуется детальный контроль доступа к целевому хранилищу, можно использовать политику по умолчанию или настроить политику по умолчанию с дополнительными правилами.

## <a name="create-a-client-access-policy"></a>Создание политики клиентского доступа

Страница **политики клиентского доступа** на портал Azure используется для создания политик и управления ими. <!-- is there AZ CLI for this? -->

[![снимок экрана со страницей политик клиентского доступа. Определено несколько политик, и некоторые из них развернуты для отображения их правил](media/policies-overview.png)](media/policies-overview.png#lightbox)

Каждая политика состоит из правил. Правила применяются к узлам в порядке от наименьшей области (узла) до самой крупной (по умолчанию). При этом применяется первое правило, соответствующее правилам, и более поздние правила игнорируются.

Чтобы создать новую политику доступа, нажмите кнопку **+ Добавить политику доступа** в верхней части списка. Присвойте новой политике доступа имя и введите по крайней мере одно правило.

![снимок экрана: изменение колонки "политики доступа" с несколькими введенными правилами. Нажмите кнопку ОК, чтобы сохранить правило.](media/add-policy.png)

В оставшейся части этого раздела объясняются значения, которые можно использовать в правилах.

### <a name="scope"></a>Область

Термин область и фильтр адресов работают вместе, чтобы определить, на какие клиенты влияет это правило.

Используйте их, чтобы указать, применяется ли правило к отдельному клиенту (узлу), диапазону IP-адресов (сеть) или всем клиентам (по умолчанию).

Выберите подходящее значение **области** для правила:

* **Узел** — правило применяется к отдельному клиенту.
* **Сеть** — правило применяется к клиентам в диапазоне IP-адресов.
* **По умолчанию** — правило применяется ко всем клиентам.

Правила в политике оцениваются в указанном порядке. После того как запрос на подключение клиента соответствует одному правилу, остальные игнорируются.

### <a name="address-filter"></a>Фильтр адресов

Значение **фильтра адресов** указывает, какие клиенты соответствуют правилу.

Если для области задано **Размещение**, в фильтре можно указать только один IP-адрес. Для параметра область **по умолчанию** нельзя ввести IP-адреса в поле **фильтр адреса** , так как область по умолчанию соответствует всем клиентам.

Укажите IP-адрес или диапазон адресов для этого правила. Используйте нотацию CIDR (пример: 0.1.0.0/16), чтобы указать диапазон адресов.

### <a name="access-level"></a>Уровень доступа

Укажите, какие привилегии следует предоставить клиентам, соответствующим области и фильтру.

Параметры доступны **для чтения и записи**, **только для чтения** или **без доступа**.

### <a name="suid"></a>SUID

Установите флажок **SUID** , чтобы разрешить файлам в хранилище задавать идентификаторы пользователей при доступе.

SUID обычно используется для временного увеличения привилегий пользователя, чтобы пользователь мог выполнить задачу, связанную с этим файлом.

### <a name="submount-access"></a>Доступ к подподключению

Установите этот флажок, чтобы разрешить указанным клиентам напрямую подключать подкаталоги экспорта.

### <a name="root-squash"></a>Корневой Squash

Выберите, следует ли задавать корневые Squash для клиентов, соответствующих этому правилу.

Это значение позволяет разрешить корневую Squash на уровне экспорта хранилища. Вы также можете [задать корневую Squash на уровне кэша](configuration.md#configure-root-squash).

Если включить корневой Squash, необходимо также задать для анонимного идентификатора значение одного из следующих параметров:

* **-2** (никто)
* **65534** (никто)
* **-1** (без доступа)
* **65535** (нет доступа)
* **0** (непривилегированный корень)

## <a name="update-access-policies"></a>Обновление политик доступа

Политики доступа можно изменить или удалить из таблицы на странице **политики клиентского доступа** .

Щелкните имя политики, чтобы открыть его для редактирования.

Чтобы удалить политику, установите флажок рядом с ее именем в списке, а затем нажмите кнопку **Удалить** в верхней части списка. Невозможно удалить политику с именем "default".

> [!NOTE]
> Нельзя удалить используемую политику доступа. Удалите политику из всех путей к пространству имен, содержащих ее, прежде чем пытаться удалить ее.

## <a name="next-steps"></a>Дальнейшие действия

* Примените политики доступа в путях к пространствам имен для целевых объектов хранилища. Дополнительные сведения см. в статье [Настройка агрегированного пространства имен](add-namespace-paths.md) .
