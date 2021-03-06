---
title: Планы обслуживания и квоты для Azure Веснного облака
description: Сведения о квотах служб и планах обслуживания для Azure Веснного облака
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: b02ccb3acb4546e08e7d58159ab9d85bca2d0eed
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101711881"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Квоты и планы обслуживания для Azure Веснного облака

**Эта статья применима к:** ✔️ Java ✔️ C#

Все службы Azure устанавливают ограничения по умолчанию и квоты для ресурсов и компонентов.   Azure Веснного облака предлагает две ценовые категории: "базовый" и "Стандартный". Ограничения для обоих уровней будут подробно описаны в этой статье.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Уровни облачных служб Azure весны и ограничения

| Ресурс | Область | Basic | Standard
------- | ------- | -------
vCPU | на экземпляр приложения | 1 | 4
Память | на экземпляр приложения | 2 ГБ | 8 ГБ
Экземпляры облачной службы Azure весны | для каждого региона на подписку | 10 | 10
Всего экземпляров приложений | на экземпляр облачной службы Azure весны | 25 | 500
Личные домены | на экземпляр облачной службы Azure весны | 0 | 25 
Постоянные тома | на экземпляр облачной службы Azure весны | 1 ГБ/приложение x 10 приложений | 50 ГБ/приложение x 10 приложений

> [!TIP]
> Ограничения, перечисленные для каждого экземпляра службы, применяются для приложений и развертываний в любом состоянии, включая остановленное состояние. Удалите приложения или развертывания, которые не используются.

## <a name="next-steps"></a>Дальнейшие действия

Некоторые ограничения по умолчанию можно увеличить. Если для установки требуется увеличение, [Создайте запрос в службу поддержки](../azure-portal/supportability/how-to-create-azure-support-request.md).
