---
author: baanders
description: файл включения для шагов обзор программы установки Azure Digital двойников
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: c1cfab8c9e68d9e6b0c3b84e8848645a50c24710
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560800"
---
Полная настройка нового экземпляра Digital двойников для Azure состоит из двух частей:
1. **Создание экземпляра**
2. **Настройка разрешений для доступа пользователей**. Пользователи Azure должны иметь роль *владельца данных Digital двойников* в экземпляре Azure Digital двойников, чтобы иметь возможность управлять им и данными. На этом шаге вы, как владелец или администратор подписки Azure, назначите эту роль пользователю, который будет управлять своим экземпляром Azure Digital двойников. Это может быть пользователь или кто-то другой в вашей организации.
 
>[!NOTE]
>Эти операции должны выполняться пользователем с возможностями управления ресурсами и доступом пользователей в подписке Azure. Хотя некоторые действия могут быть выполнены с более низкими разрешениями, для полной настройки пригодного к использованию экземпляра потребуется совместная работа пользователей с этими разрешениями. Дополнительные сведения см. в разделе необходимые [*разрешения*](#prerequisites-permission-requirements) ниже.