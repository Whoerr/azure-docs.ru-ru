---
title: Среда виртуальных рабочих столов Windows — Azure
description: Сведения об основных элементах среды виртуальных рабочих столов Windows, таких как пулы узлов и группы приложений.
author: Heidilohr
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 99a07dd1791b539ea44fcbab250aa9c227ee1705
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88002618"
---
# <a name="windows-virtual-desktop-environment"></a>Среда Виртуального рабочего стола Windows

>[!IMPORTANT]
>Это содержимое применимо к Виртуальному рабочему столу Windows с объектами Azure Resource Manager для Виртуального рабочего стола Windows. Если вы используете Виртуальный рабочий стол Windows (классический) без объектов Azure Resource Manager, ознакомьтесь с [этой статьей](./virtual-desktop-fall-2019/environment-setup-2019.md).

Виртуальный рабочий стол Windows — это служба, которая предоставляет пользователям простой и безопасный доступ к виртуализованным рабочим столам и приложениям RemoteApp. В этом разделе вы узнаете о общей структуре среды виртуальных рабочих столов Windows.

## <a name="host-pools"></a>Пулы узлов

Пул узлов — это коллекция виртуальных машин Azure, которые регистрируются в виртуальном рабочем столе Windows как узлы сеансов при запуске агента виртуальных рабочих столов Windows. Все виртуальные машины узла сеансов в пуле узлов должны быть источниками одного образа для единообразного взаимодействия с пользователем.

Пул узлов может иметь один из двух типов:

- Личное, где каждый узел сеансов назначается отдельным пользователям.
- В составе пула, где узлы сеансов могут принимать подключения от любого пользователя, имеющего разрешение на группу приложений в пуле узлов.

Вы можете задать дополнительные свойства в пуле узлов, чтобы изменить его поведение балансировки нагрузки, количество сеансов, которое может предпринять каждый узел сеансов, и то, что пользователь может сделать для сеансов узлов в пуле узлов при входе в сеансы виртуальных рабочих столов Windows. Вы управляете ресурсами, опубликованными для пользователей через группы приложений.

## <a name="app-groups"></a>Группы приложений

Группа приложений — это логическая группа приложений, установленных на узлах сеансов в пуле узлов. Группа приложений может иметь один из двух типов:

- RemoteApp, где пользователи получают доступ к удаленным приложениям RemoteApp, выбранным по отдельности, и публикуют их в группе приложений.
- Рабочий стол, где пользователи получают доступ к полному рабочему столу

По умолчанию группа приложений для настольных компьютеров (с именем "Группа приложений рабочей среды") создается автоматически при создании пула узлов. Вы можете удалить эту группу приложений в любое время. Однако нельзя создать другую группу настольных приложений в пуле узлов, пока существует группа приложений для настольных компьютеров. Чтобы опубликовать RemoteApps, необходимо создать группу приложений RemoteApp. Можно создать несколько групп приложений RemoteApp для размещения различных рабочих сценариев. Различные группы приложений RemoteApp также могут содержать перекрывающиеся приложения RemoteApp.

Чтобы опубликовать ресурсы для пользователей, их необходимо назначить группам приложений. При назначении пользователей группам приложений учитывайте следующие моменты.

- Пользователь может быть назначен как группе приложений рабочей группы, так и группе приложений RemoteApp в одном пуле узлов. Однако пользователи могут запускать только один тип группы приложений для каждого сеанса. Пользователи не могут одновременно запускать оба типа групп приложений в одном сеансе.
- Пользователь может быть назначен нескольким группам приложений в одном пуле узлов, и их веб-канал будет накоплен как для обеих групп приложений.

## <a name="workspaces"></a>Рабочие области

Рабочая область — это логическое группирование групп приложений в виртуальном рабочем столе Windows. Каждая группа приложений виртуальных рабочих столов Windows должна быть связана с рабочей областью, чтобы пользователи могли видеть опубликованные им удаленные приложения и настольные компьютеры.

## <a name="end-users"></a>конечные пользователи;

После назначения пользователей группам приложений они могут подключаться к развертыванию виртуальных рабочих столов Windows с любыми клиентами виртуальных рабочих столов Windows.

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте больше о делегированном доступе и о том, как назначать роли пользователям с [делегированным доступом в виртуальном рабочем столе Windows](delegated-access-virtual-desktop.md).

Сведения о настройке пула узлов виртуальных рабочих столов Windows см. в разделе [Создание пула узлов с портал Azure](create-host-pools-azure-marketplace.md).

Сведения о подключении к виртуальному рабочему столу Windows см. в одной из следующих статей:

- [Подключение с Windows 10 или Windows 7](connect-windows-7-10.md)
- [Подключение с помощью веб-браузера](connect-web.md)
- [Подключение к клиенту Android](connect-android.md)
- [Подключение с помощью клиента macOS](connect-macos.md)
- [Подключение с помощью клиента iOS](connect-ios.md)