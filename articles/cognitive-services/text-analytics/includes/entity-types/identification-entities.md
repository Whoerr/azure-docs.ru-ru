---
title: Идентификация сущностей
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/17/2021
ms.author: aahi
ms.openlocfilehash: a376b050d79709885e3542d330bb6b1eea48d046
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750507"
---
### <a name="financial-account-identification"></a>Идентификация финансовой учетной записи

Эта категория сущностей включает в себя финансовую информацию и официальные формы идентификации.

#### <a name="category-aba-routing-number"></a>Категория: номер маршрутизации ABA

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        Номер маршрутизации ABA

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Номера транзитного маршрута ассоциации банка Америки (ABA).
      
    :::column-end:::
:::row-end:::

#### <a name="category-swift-code"></a>Категория: код SWIFT

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        Код SWIFT

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Коды SWIFT для сведений о инструкции по оплате.
      
    :::column-end:::
:::row-end:::

#### <a name="category-credit-card"></a>Категория: кредитная карта

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        Кредитная карта

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Номера кредитных карт. 
      
    :::column-end:::
:::row-end:::

#### <a name="category-international-banking-account-number-iban"></a>Категория: Международный номер банковского счета (IBAN) 

Эта категория содержит следующую сущность:

:::row:::
    :::column span="":::
        **Сущность**

        Кредитная карта

    :::column-end:::
    :::column span="2":::
        **Сведения**

        Коды IBAN для сведений о инструкции по оплате.
      
    :::column-end:::
:::row-end:::

### <a name="government-and-countryregion-specific-identification"></a>Идентификация правительства и страны или региона

> [!NOTE]
> Следующие сущности, относящиеся к финансовой и стране, не возвращаются с помощью `domain=phi` параметра:
> * Номера паспорта
> * Налоговые коды

Следующие сущности сгруппированы и перечислены по странам:

#### <a name="argentina"></a>Аргентина

:::row:::
    :::column span="":::
        **Сущность**

        Номер государственного удостоверения (дни)

    :::column-end:::
:::row-end:::


#### <a name="austria"></a>Австрия

:::row:::
    :::column span="":::
        **Сущность**

        Австрия Identity Card

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Налоговый идентификационный номер Австрии

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Номер налога на добавленную стоимость (VAT)

    :::column-end:::
:::row-end:::



#### <a name="australia"></a>Австралия

:::row:::
    :::column span="":::
        **Сущность**

        Номер банковского счета Австралии

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Австралийский бизнес-номер

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Номер компании Австралии

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Лицензия на драйвер Австралии  

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер медицинской учетной записи Австралии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер паспорта для Австралии

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Номер паспорта для Австралии

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Номер файла налога Австралии

    :::column-end:::

:::row-end:::


#### <a name="belgium"></a>Бельгия

:::row:::
    :::column span="":::
        **Сущность**

        Бельгийский национальный номер

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер налога на добавленную стоимость (VAT)

    :::column-end:::

:::row-end:::


#### <a name="brazil"></a>Бразилия 

:::row:::
    :::column span="":::
        **Сущность**

        Номер юридического лица в Бразилии (CNPJ)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер CPF в Бразилии

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Национальная ИДЕНТИФИКАЦИОНная карта Бразилии (RG)

    :::column-end:::
:::row-end:::

#### <a name="canada"></a>Канада

:::row:::
    :::column span="":::
        **Сущность**

        Номер банковского счета Канады

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Номер лицензии для драйвера Канады

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер службы работоспособности Канады

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер паспорта для Канады

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Личный идентификационный номер работоспособности (фин) для Канады

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Номер страхования социальных сетей (Канада)

    :::column-end:::
:::row-end:::

#### <a name="chile"></a>Чили 

:::row:::
    :::column span="":::
        **Сущность**

        Номер идентификационной карты для Чили

    :::column-end:::
:::row-end:::

#### <a name="china"></a>Китай

:::row:::
    :::column span="":::
        **Сущность**

        Номер резидентной идентификационной карточки (КНР) Китая

    :::column-end:::
:::row-end:::


#### <a name="european-union-eu"></a>Европейский союз (ЕС)

:::row:::
    :::column span="":::
        **Сущность**

        Номер дебетовой карты ЕС

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер лицензии для драйвера ЕС

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Национальный идентификационный номер ЕС

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер паспорта ЕС

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер социального страхования (SSN) ЕС или эквивалентный идентификатор

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Идентификационный номер налогоплательщика в ЕС

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Координаты GPS в ЕС

    :::column-end:::
:::row-end:::

#### <a name="france"></a>Франция

:::row:::
    :::column span="":::
        **Сущность**

        Номер лицензии для драйвера Франции

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер страховки работоспособности Франции

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Французская Национальная ИДЕНТИФИКАЦИОНная карта (CNI)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер паспорта Франции

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер социального страхования Франции (ИНСИ)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Налоговый идентификационный номер Франции (Нумéро SPI)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер налога на добавленную ценность Франции (НДС)

    :::column-end:::
:::row-end:::

#### <a name="germany"></a>Германия

:::row:::
    :::column span="":::
        **Сущность**

        Номер лицензии драйвера для Германии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер карточки удостоверения Германии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер паспорта для Германии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Идентификационный номер налогоплательщика для Германии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Налоговый номер добавленного значения в Германии

    :::column-end:::
:::row-end:::

#### <a name="hong-kong"></a>Гонконг, САР

:::row:::
    :::column span="":::
        **Сущность**

        Номер карточки удостоверения Гонконг (ХКИД)

    :::column-end:::
:::row-end:::

#### <a name="hungary"></a>Венгрия

:::row:::
    :::column span="":::
        **Сущность**

        Личный идентификационный номер венгерского

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Идентификационный номер венгерского налога

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Налоговый номер добавленного значения Венгрии

    :::column-end:::
:::row-end:::

#### <a name="india"></a>Индия

:::row:::
    :::column span="":::
        **Сущность**

        Номер постоянной учетной записи Индии (PAN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер уникальной идентификации (Аадхаар) Индии

    :::column-end:::
:::row-end:::


#### <a name="indonesia"></a>Индонезия

:::row:::
    :::column span="":::
        **Сущность**

        Номер карточки удостоверения Индонезия (КТП)

    :::column-end:::
:::row-end:::

#### <a name="ireland"></a>Ирландия

:::row:::
    :::column span="":::
        **Сущность**

        Номер личной общедоступной службы (PPS)

    :::column-end:::
:::row-end:::

#### <a name="israel"></a>Израиль

:::row:::
    :::column span="":::
        **Сущность**

        Национальный идентификатор Израиля

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер банковского счета Израиля

    :::column-end:::
:::row-end:::

#### <a name="italy"></a>Италия

:::row:::
    :::column span="":::
        **Сущность**

        Идентификатор лицензии драйвера Италии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Финансовый код Италии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Налоговый номер добавленного значения Италии

    :::column-end:::
:::row-end:::


#### <a name="japan"></a>Япония

:::row:::
    :::column span="":::
        **Сущность**

        Номер банковского счета для Японии

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Номер лицензии для драйвера Японии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Япония "Моя цифра" (личное)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Япония "Моя цифра" (корпоративный)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер резидентной регистрации для Японии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер карты для Японии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер социального страхования для Японии (SIN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер паспорта для Японии

    :::column-end:::
:::row-end:::

#### <a name="luxembourg"></a>Люксембург

:::row:::
    :::column span="":::
        **Сущность**

        Идентификационный номер Люксембург (естественное лицо)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Идентификационный номер Люксембург (неестественные лица)

    :::column-end:::
:::row-end:::

#### <a name="malta"></a>Мальта

:::row:::
    :::column span="":::
        **Сущность**

        Номер карты идентификации Мальта

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Идентификационный номер налогоплательщика Мальта

    :::column-end:::
:::row-end:::


#### <a name="new-zealand"></a>Новая Зеландия

:::row:::
    :::column span="":::
        **Сущность**

        Номер банковского счета Новой Зеландии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер лицензии для драйвера Новой Зеландии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер дохода для Новой Зеландии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Министерство номера работоспособности Новой Зеландии

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Номер социального Велфареа Новой Зеландии

    :::column-end:::
:::row-end:::


#### <a name="philippines"></a>Филиппины

:::row:::
    :::column span="":::
        **Сущность**

        Идентификатор единой унифицированной многозначной идентификации Филиппины

    :::column-end:::
:::row-end:::

#### <a name="portugal"></a>Португалия 

:::row:::
    :::column span="":::
        **Сущность**

        Номер карточки гражданского по Португалия

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Идентификационный номер налогоплательщика (Португалия)

    :::column-end:::
:::row-end:::

#### <a name="singapore"></a>Сингапур

:::row:::
    :::column span="":::
        **Сущность**

        (НРИК) номер национальной регистрационной карты (Сингапур)

    :::column-end:::
:::row-end:::


#### <a name="south-africa"></a>ЮАР

:::row:::
    :::column span="":::
        **Сущность**

        Идентификационный номер Южной Африки

    :::column-end:::
:::row-end:::


#### <a name="south-korea"></a>Южная Корея

:::row:::
    :::column span="":::
        **Сущность**

        Номер резидентной регистрации Южной Кореи

    :::column-end:::
:::row-end:::

#### <a name="spain"></a>Испания

:::row:::
    :::column span="":::
        **Сущность**

        Испания дни

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер социального страхования (SSN) Испания

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Налоговый идентификационный номер Испании

    :::column-end:::
:::row-end:::
 
#### <a name="switzerland"></a>Швейцария

:::row:::
    :::column span="":::
        **Сущность**

        Швейцарский номер социального страхования АХВ

    :::column-end:::
:::row-end:::


#### <a name="taiwan"></a>Тайвань 

:::row:::
    :::column span="":::
        **Сущность**

        Тайваньская национальная ИДЕНТИФИКАЦИя

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Сертификат в формате Тайваня (ARC/ТАРК)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Номер паспорта для Тайваня

    :::column-end:::
:::row-end:::

#### <a name="united-kingdom"></a>Соединенное Королевство

:::row:::
    :::column span="":::
        **Сущность**

        Великобритании Номер лицензии драйвера

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Великобритании Номер рулона предпочтений избирателей

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Великобритании Номер национального служба работоспособности (отделения)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Великобритании Номер национальной страховки (Нино)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Великобритании или номер паспорта США

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

       Великобритании Уникальный номер ссылки налогоплательщика

    :::column-end:::

:::row-end:::


#### <a name="united-states"></a>США

:::row:::
    :::column span="":::
        **Сущность**

        Номер социального страхования США (SSN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Номер лицензии для драйвера США

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       США или Великобритания Номер паспорта

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Индивидуальный идентификационный номер налогоплательщика (ИТИН) США

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Номер агентства для принудительного применения наркотиков США (ДЕА)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Номер банковского счета США

    :::column-end:::
:::row-end:::
