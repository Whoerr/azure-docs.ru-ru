---
title: Сведения о том, как работает автоматическая миграция Azure Monitor классические оповещения
description: Узнайте, как работает процесс автоматической миграции.
ms.topic: conceptual
ms.date: 02/14/2021
ms.subservice: alerts
ms.openlocfilehash: 65409a1710a2b4c6b6d5a52c5129ec3e82dc7cc2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734865"
---
# <a name="understand-the-automatic-migration-process-for-your-classic-alert-rules"></a>Общие сведения о процессе автоматической миграции для классических правил генерации оповещений

Как [было объявлено ранее](monitoring-classic-retirement.md), классические оповещения в Azure Monitor исключаются для пользователей общедоступного облака, хотя и в ограниченном объеме использования до **31 мая 2021**. Классические оповещения для облачных служб Azure для государственных организаций и Azure для Китая, истечение **29 февраля 2024**.

[Средство миграции](alerts-using-migration-tool.md) доступно в портал Azure, чтобы клиенты могли активировать миграцию самостоятельно. В этой статье описывается процесс автоматической миграции в общедоступном облаке, который начнется после 31 мая 2021. Здесь также содержатся сведения о проблемах и решениях, с которыми вы можете столкнуться.

## <a name="important-things-to-note"></a>Важные моменты, которые следует отметить

Процесс миграции преобразует классические правила генерации оповещений в новые, эквивалентные правила генерации оповещений и создает группы действий. При подготовке необходимо учитывать следующие моменты.

- Форматы полезных данных уведомлений для новых правил генерации оповещений отличаются от полезных данных классических правил генерации оповещений, так как они поддерживают больше возможностей. При наличии классического правила генерации оповещений с приложениями логики, модулями Runbook или веб-перехватчиками они могут перестать работать надлежащим образом после миграции из-за различий в полезных данных. [Узнайте, как подготовиться к миграции](alerts-prepare-migration.md).

- Некоторые классические правила генерации оповещений нельзя перенести с помощью средства. [Узнайте, какие правила не могут быть перенесены, и что делать с ними](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

## <a name="what-will-happen-during-the-automatic-migration-process-in-public-cloud"></a>Что произойдет в процессе автоматической миграции в общедоступном облаке?

- Начиная с 31 мая 2021 г. Вы не сможете создавать новые классические правила генерации оповещений, а миграция классических предупреждений будет срабатывать в пакетах.
- Все классические правила генерации оповещений, которые отслеживают удаленные целевые ресурсы или [метрики, которые больше не поддерживаются](alerts-understand-migration.md#classic-alert-rules-on-deprecated-metrics) , считаются недействительными.
- Классические правила генерации оповещений, которые являются недопустимыми, будут удаляться позже 31 мая 2021.
- После начала миграции подписки она должна быть завершена в течение часа. Клиенты могут отслеживать состояние миграции на [средство миграции в Azure Monitor](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel).
- Владельцы подписки получат сообщение об успешном или неуспешном завершении миграции.

    > [!NOTE]
    > Если вы не хотите ждать начала процесса автоматической миграции, вы по-прежнему можете активировать миграцию с помощью средства миграции.

## <a name="what-if-the-automatic-migration-fails"></a>Что произойдет, если происходит сбой автоматической миграции?

В случае сбоя автоматической миграции владельцы подписки получат по электронной почте уведомление об этой неполадке. Вы можете использовать средство миграции в Azure Monitor, чтобы просмотреть все сведения о ней. Дополнительные сведения о проблемах, которые могут возникнуть во время миграции, см. в разделе [руководство по устранению неполадок](alerts-understand-migration.md#common-problems-and-remedies) .

  > [!NOTE]
  > Если требуется действие от клиентов, например временное отключение блокировки ресурса или изменение назначения политики, клиенты должны будут решить эти проблемы. Если проблемы не устранены, то успешное перемещение классических оповещений не гарантируется.

## <a name="next-steps"></a>Дальнейшие действия

- [Подготовка к переносу](alerts-prepare-migration.md)
- [Принцип работы средства миграции](alerts-understand-migration.md)