---
title: Именованные сущности для персональных данных
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/17/2021
ms.author: aahi
ms.openlocfilehash: 97167485dae155670f0eb83fc3ef9cb658952251
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750489"
---
> [!NOTE]
> Чтобы определить защищенную информацию о работоспособности (фи), используйте `domain=phi` параметр и версию модели `2020-04-01` или более поздней версии.
>
> Например: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.3/entities/recognition/pii?domain=phi&model-version=2021-01-15`
 
При отправке запросов в конечную точку возвращаются следующие категории сущностей `/v3.1-preview.3/entities/recognition/pii` .


| Категория   |  Описание                          |
|------------|-------------|
| [Person](#category-person)      |  Имена людей.  |
| [персонтипе](#category-persontype) | Типы заданий или роли, удерживаемые человеком. |
| [Номер телефона](#category-phonenumber) |Номера телефонов (только для телефонных номеров США и ЕС). |
| [Организация](#category-organization) |  Компании, группы, государственные органы и другие организации.  |
| [Адрес](#category-address) | Полные адреса электронной почты.  |
| [Электронная почта](#category-email) | Адреса электронной почты.   |
| [URL-адрес](#category-url) | URL-адреса веб-сайтов.  |
| [СМ](#category-ip) | Сетевые IP-адреса.  |
| [DateTime](#category-datetime) | Даты и время суток. | 
| [Количество](#category-quantity) | Числа и числовые величины.  |
| [Сведения об Azure](#azure-information) | Идентифицируемые сведения об Azure, например сведения о проверке подлинности.  |
| [Идентификация](#identification) | Финансовая и региональная идентификация.  |

### <a name="category-person"></a>Категория: Person

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        Человек

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Имена людей. Также возвращается с `domain=phi` .
      
    :::column-end:::
:::row-end:::

### <a name="category-persontype"></a>Категория: Персонтипе

Эта категория содержит следующую сущность:


:::row:::
    :::column span="":::
        **Сущность**

        персонтипе

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Типы заданий или роли, удерживаемые человеком.
      
    :::column-end:::
:::row-end:::

### <a name="category-phonenumber"></a>Категория: PhoneNumber

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        PhoneNumber

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Номера телефонов (только для телефонных номеров США и ЕС). Также возвращается с `domain=phi` .
      
    :::column-end:::
:::row-end:::


### <a name="category-organization"></a>Категория: Организация

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        План

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Компании, «неправительственные группы», «музыкальные зоны», «спорт», «государственные органы» и «общественные организации». Национальные и религионсы не включаются в этот тип сущности.
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Подкатегории

Сущность в этой категории может иметь следующие подкатегории.

:::row:::
    :::column span="":::
        **Подкатегория сущности**

        Медицина

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Медицинские компании и группы.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Обмен на фондовой бирже

    :::column-end:::
    :::column span="2":::

        Группы обмена фондовой биржи. 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Спорт

    :::column-end:::
    :::column span="2":::

        Организации, связанные с спортивными делами.
      
    :::column-end:::

:::row-end:::


### <a name="category-address"></a>Категория: адрес

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        Адрес

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Полный почтовый адрес.
      
    :::column-end:::
:::row-end:::

### <a name="category-email"></a>Категория: Электронная почта

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        Email

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Адреса электронной почты.
      
    :::column-end:::
:::row-end:::

### <a name="category-url"></a>Категория: URL-адрес

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        URL-адрес

    :::column-end:::
    :::column span="2":::
        **Сведения**

        URL-адреса веб-сайтов. 
      
    :::column-end:::
:::row-end:::

### <a name="category-ip"></a>Категория: IP-адрес

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        IP-адрес

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Сетевые IP-адреса. 
      
    :::column-end:::
:::row-end:::

### <a name="category-datetime"></a>Категория: DateTime

Эта категория содержит следующие сущности:

:::row:::
    :::column span="":::
        **Сущность**

        Дата и время

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Даты и время суток. 
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Подкатегории

Сущность в этой категории может иметь следующие подкатегории.

:::row:::
    :::column span="":::
        **Подкатегория сущности**

        Дата

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Календарные даты.
      
    :::column-end:::
:::row-end:::

### <a name="category-quantity"></a>Категория: количество

Эта категория содержит следующие сущности:

:::row:::
    :::column span="":::
        **Сущность**

        Количество

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Числа и числовые величины.
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Подкатегории

Сущность в этой категории может иметь следующие подкатегории.

:::row:::
    :::column span="":::
        **Подкатегория сущности**

        возраст;

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Устаревают.
      
    :::column-end:::
:::row-end:::

### <a name="azure-information"></a>Сведения об Azure

Эти категории сущностей включают в себя идентифицируемые сведения об Azure, включая сведения о проверке подлинности и строки подключения. Не возвращается с `domain=phi` параметром.

:::row:::
    :::column span="":::
        **Сущность**

        ключ проверки подлинности Azure DocumentDB; 

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Ключ авторизации для сервера Azure Cosmos DB.   
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        строка подключения Базы данных Azure IAAS и строка подключения Azure SQL;

    :::column-end:::
    :::column span="2":::

        Строка подключения для базы данных инфраструктуры Azure в виде службы (IaaS) и строки подключения SQL.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Строка подключения SQL Azure

    :::column-end:::
    :::column span="2":::

        Строка подключения для базы данных в базе данных SQL Azure.
      
    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        строка подключения Azure IoT;  

    :::column-end:::
    :::column span="2":::

        Строка подключения для Azure IoT. 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        пароль для публикации Azure;  

    :::column-end:::
    :::column span="2":::

        Пароль для параметров публикации Azure.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        строка подключения кэша Redis для Azure; 

    :::column-end:::
    :::column span="2":::

        Строка подключения для кэша Redis.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure SAS; 

    :::column-end:::
    :::column span="2":::

        Строка подключения для программного обеспечения Azure как услуги (SaaS).
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        строка подключения Служебной шины Azure;

    :::column-end:::
    :::column span="2":::

        Строка подключения для служебной шины Azure.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Ключ учетной записи хранения Azure 

    :::column-end:::
    :::column span="2":::

       Ключ учетной записи хранения Azure. 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        ключ учетной записи хранилища Azure (универсальный).

    :::column-end:::
    :::column span="2":::

       Универсальный ключ учетной записи для учетной записи хранения Azure.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        строка подключения SQL Server; 

    :::column-end:::
    :::column span="2":::

       Строка подключения для компьютера, на котором работает SQL Server.
      
    :::column-end:::
:::row-end:::

### <a name="identification"></a>Идентификация

[!INCLUDE [supported identification entities](./identification-entities.md)]
