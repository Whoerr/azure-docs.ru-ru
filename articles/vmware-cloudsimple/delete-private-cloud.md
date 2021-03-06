---
title: Удаление решения VMware для Azure с помощью Клаудсимпле частного облака
description: Узнайте, как удалить частное облако Клаудсимпле. При удалении частного облака все кластеры будут удалены.
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 7db967955dc86db39db4dcb2b3a2baf8906efb20
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97896266"
---
# <a name="delete-a-cloudsimple-private-cloud"></a>Удаление частного облака Клаудсимпле

Клаудсимпле обеспечивает гибкость при удалении частного облака.  Частное облако состоит из одного или нескольких кластеров vSphere. Каждый кластер может иметь от 3 до 16 узлов. При удалении частного облака все кластеры будут удалены.

## <a name="before-you-begin"></a>Перед началом

При удалении частного облака удаляется все частное облако.  Все компоненты частного облака будут удалены.  Если вы хотите хранить данные, убедитесь, что вы заархивированы данные в локальное хранилище или в хранилище Azure.

Компоненты частного облака включают:

* Узлы Клаудсимпле
* Виртуальные машины
* Виртуальные ЛС и подсети
* Все пользовательские данные, хранящиеся в частном облаке
* Все вложения правил брандмауэра в виртуальную ЛС или подсеть

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="delete-a-private-cloud"></a>Удаление частного облака

1. [Доступ к порталу клаудсимпле](access-cloudsimple-portal.md).

2. Откройте страницу **ресурсы** .

3. Щелкните частное облако, которое необходимо удалить.

4. На странице Сводка нажмите кнопку **Удалить**.

    ![Удаление частного облака](media/delete-private-cloud.png)

5. На странице Подтверждение введите имя частного облака и нажмите кнопку **Удалить**. 

    ![Удаление частного облака — подтверждение](media/delete-private-cloud-confirm.png)

Частное облако помечено для удаления.  Процесс удаления начинается через три часа и удаляет частное облако.

> [!CAUTION]
> После удаления частного облака узлы должны быть удалены.  Измерение узлов будет продолжаться, пока узлы будут удалены из подписки.

## <a name="next-steps"></a>Дальнейшие действия

* [Удаление узлов](delete-nodes.md)
