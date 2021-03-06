---
title: Двойное шифрование инфраструктуры — база данных Azure для PostgreSQL
description: Узнайте, как использовать двойное шифрование инфраструктуры, чтобы добавить второй уровень шифрования с ключами, управляемыми службой.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 6/30/2020
ms.openlocfilehash: 83635b732318a4ada76d1d71c1ce419cae8b35e9
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98630152"
---
# <a name="azure-database-for-postgresql-infrastructure-double-encryption"></a>Двойное шифрование базы данных Azure для инфраструктуры PostgreSQL

База данных Azure для PostgreSQL использует [Шифрование хранилища неактивных данных](concepts-security.md#at-rest) для данных с помощью управляемых ключей Майкрософт. Данные, включая резервные копии, шифруются на диске, и это шифрование всегда включено и не может быть отключено. Шифрование использует проверенный криптографический модуль FIPS 140-2 и шифр AES 256-bit для шифрования службы хранилища Azure.

При двойном шифровании инфраструктуры добавляется второй уровень шифрования с помощью управляемых службой ключей. Он использует проверенный криптографический модуль FIPS 140-2, но с другим алгоритмом шифрования. Это обеспечивает дополнительный уровень защиты для неактивных данных. Ключ, используемый в двойном шифровании инфраструктуры, также управляется службой "база данных Azure для PostgreSQL". Двойное шифрование инфраструктуры не включено по умолчанию, так как дополнительный уровень шифрования может повлиять на производительность.

> [!NOTE]
> Эта функция поддерживается только для ценовых категорий "общего назначения" и "оптимизировано для памяти" в базе данных Azure для PostgreSQL.

При шифровании уровня инфраструктуры преимущество реализуется на уровне, близком к устройству хранения или сетевому беспроводному подключению. База данных Azure для PostgreSQL реализует два уровня шифрования с помощью управляемых службой ключей. Несмотря на то, что технически все еще находится в слое служб, очень близко к оборудованию, в котором хранятся неактивных данные. При необходимости шифрование неактивных данных можно включить с помощью [управляемого клиентом ключа](concepts-data-encryption-postgresql.md) для подготовленного сервера PostgreSQL.  

Реализация на уровнях инфраструктуры также поддерживает разнообразие ключей. Инфраструктура должна быть осведомлена о различных кластерах компьютеров и сетей. Таким образом, для снижения радиуса атак с инфраструктурой и различных сбоев оборудования и сети используются разные ключи. 

> [!NOTE]
> Использование двойного шифрования инфраструктуры приведет к снижению производительности сервера базы данных Azure для PostgreSQL из-за дополнительного процесса шифрования.

## <a name="benefits"></a>Преимущества

Двойное шифрование инфраструктуры для базы данных Azure для PostgreSQL предоставляет следующие преимущества.

1. **Дополнительное разнообразие реализаций криптографии** . Запланированное перемещение на аппаратное шифрование будет дополнительно изменить реализации, предоставляя аппаратную реализацию в дополнение к реализации на основе программного обеспечения.
2. **Ошибки реализации** . два уровня шифрования на уровне инфраструктуры защищают от ошибок кэширования или управления памятью на более высоких уровнях, предоставляя данные в виде открытого текста. Кроме того, эти два уровня также гарантируют ошибки в реализации шифрования в целом.

Их сочетание обеспечивает надежную защиту от распространенных угроз и слабых мест, используемых для атаки на криптографию.

## <a name="supported-scenarios-with-infrastructure-double-encryption"></a>Поддерживаемые сценарии с двойным шифрованием инфраструктуры

Возможности шифрования, предоставляемые базой данных Azure для PostgreSQL, можно использовать вместе. Ниже приведена сводка различных сценариев, которые можно использовать.

|  ##   | Шифрование по умолчанию | Двойное шифрование инфраструктуры | Шифрование данных с помощью управляемых клиентом ключей  |
|:------|:------------------:|:--------------------------------:|:--------------------------------------------:|
| 1     | *Да*              | *Нет*                             | *Нет*                                         |
| 2     | *Да*              | *Да*                            | *Нет*                                         |
| 3     | *Да*              | *Нет*                             | *Да*                                        |
| 4     | *Да*              | *Да*                            | *Да*                                        |
|       |                    |                                  |                                              |

> [!Important]
> - Сценарий 2 и 4 влияют на производительность сервера базы данных Azure для PostgreSQL из-за дополнительного уровня шифрования инфраструктуры.
> - Настройка двойного шифрования инфраструктуры для базы данных Azure для PostgreSQL разрешена только во время создания сервера. После подготовки сервера вы не сможете изменить шифрование хранилища. Тем не менее вы по-прежнему можете включить шифрование данных с помощью управляемых клиентом ключей для сервера, созданного с помощью двойного шифрования/без использования инфраструктуры.

## <a name="limitations"></a>Ограничения

Для базы данных Azure для PostgreSQL поддержка двойного шифрования инфраструктуры с помощью управляемого службой ключа имеет следующие ограничения.

* Поддержка этой функции ограничена **общего назначения** и **оптимизированными для памяти** ценовыми категориями.
* Эта функция поддерживается только в регионах и серверах, поддерживающих хранилище объемом до 16 ТБ. Список регионов Azure, поддерживающих хранилище объемом до 16 ТБ, см. в [документации по хранилищу](concepts-pricing-tiers.md#storage).

    > [!NOTE]
    > - Все **новые** серверы PostgreSQL, созданные в перечисленных выше регионах, также поддерживают шифрование данных с помощью ключей клиента. В этом случае серверы, созданные с помощью восстановления на момент времени (PITR) или реплики чтения, не соответствуют "новым".
    > - Чтобы проверить, поддерживает ли подготовленный сервер до 16 ТБ, перейдите в колонку ценовая категория на портале и убедитесь, что ползунок хранилища можно переместить до 16 ТБ. Если вы можете переместить ползунок только до 4 ТБ, сервер может не поддерживать шифрование с помощью управляемых клиентом ключей. Однако данные шифруются с помощью ключей, управляемых службой, в любое время. AskAzureDBforPostgreSQL@service.microsoft.comЕсли у вас возникнут вопросы, свяжитесь с вами.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как [настроить двойное шифрование инфраструктуры для базы данных Azure для PostgreSQL](howto-double-encryption.md).
