---
title: Создание контроллера данных в Azure Data Studio
description: Создание контроллера данных в Azure Data Studio
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 12/09/2020
ms.topic: how-to
ms.openlocfilehash: 2b35abf7f318d4db6166dd48c4f7bd35aaf4d98a
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101644414"
---
# <a name="create-data-controller-in-azure-data-studio"></a>Создание контроллера данных в Azure Data Studio

Контроллер данных можно создать с помощью Azure Data Studio с помощью мастера развертывания и записных книжек.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Предварительные требования

- Вам потребуется доступ к кластеру Kubernetes и настроить файл kubeconfig, чтобы он указывал на кластер Kubernetes, в который требуется выполнить развертывание.
- Необходимо [установить клиентские средства](install-client-tools.md) , в том числе **Azure Data Studio** расширения Azure Data Studio, называемые " **дуга" Azure** и **[!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]** .
- Необходимо войти в Azure в Azure Data Studio.  Для этого введите сочетание клавиш CTRL/Command + SHIFT + P, чтобы открыть окно текст команды и ввести **Azure**.  Выберите **Azure: вход**.   На панели щелкните значок + в правом верхнем углу, чтобы добавить учетную запись Azure.

## <a name="use-the-deployment-wizard-to-create-azure-arc-data-controller"></a>Использование мастера развертывания для создания контроллера данных ARC в Azure

Выполните следующие действия, чтобы создать контроллер данных ARC в Azure с помощью мастера развертывания.

1. В Azure Data Studio щелкните вкладку Connections (подключения) в области навигации слева.
2. Нажмите кнопку **...** в верхней части панели подключения и выберите **Создать развертывание...**
3. В мастере создания развертывания выберите **контроллер данных ARC в Azure**, а затем нажмите кнопку **выбрать** внизу.
4. Убедитесь, что необходимые средства доступны и соответствуют требуемым версиям. **Нажмите кнопку «Далее»**.
5. Используйте файл kubeconfig по умолчанию или выберите другой.  Щелкните **Далее**.
6. Выберите контекст кластера Kubernetes. Щелкните **Далее**.
7. Выберите профиль конфигурации развертывания в зависимости от целевого кластера Kubernetes. **Нажмите кнопку «Далее»**.
8. Если вы используете платформу контейнеров Azure Red Hat OpenShift или Red Hat OpenShift, примените ограничения контекста безопасности. Следуйте инструкциям в разделе [применение ограничения контекста безопасности для служб данных, включенных в службу "Дуга Azure" на OpenShift](how-to-apply-security-context-constraint.md).

   >[!IMPORTANT]
   >На платформе контейнеров Azure Red Hat OpenShift или Red Hat OpenShift необходимо применить ограничение контекста безопасности перед созданием контроллера данных.

1. Выберите нужную подписку и группу ресурсов.
1. Выберите расположение Azure.
   
   Расположение Azure, выбранное здесь, — это расположение в Azure, где будут храниться *метаданные* об контроллере данных и управляемых ими экземплярах баз данных. Контроллер данных и экземпляры базы данных будут фактически кревтед в кластере Kubernetes везде, где это возможно.

10. Выберите подходящий режим подключения. Дополнительные сведения о [режимах подключения](./connectivity.md). **Нажмите кнопку «Далее»**.

    Если выбран режим прямого подключения, требуются учетные данные субъекта-службы, как описано в разделе [Создание субъекта-службы](upload-metrics-and-logs-to-azure-monitor.md#create-service-principal).

11. Введите имя контроллера данных и пространства имен, в котором будет создан контроллер данных.

    Контроллер данных и имя пространства имен будут использоваться для создания настраиваемого ресурса в кластере Kubernetes, чтобы они должны были соответствовать [соглашениям об именовании Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#names).
    
    Если пространство имен уже существует, оно будет использоваться, если пространство имен еще не содержит другие объекты Kubernetes-Pod и т. д.  Если пространство имен не существует, будет предпринята попытка создать пространство имен.  Для создания пространства имен в кластере Kubernetes требуются права администратора кластера Kubernetes.  Если у вас нет прав администратора кластера Kubernetes, попросите администратора кластера Kubernetes выполнить первые несколько шагов в разделе [Создание контроллера данных с помощью Kubernetes-собственных средств](./create-data-controller-using-kubernetes-native-tools.md) , которые должны быть выполнены администратором Kubernetes перед завершением работы мастера.


12. Выберите класс хранения, в котором будет развернут контроллер данных. 
13.  Введите имя пользователя и пароль и подтвердите пароль для учетной записи администратора контроллера данных. Щелкните **Далее**.

14. Проверьте конфигурацию развертывания.
15. Щелкните **развертывание** , чтобы развернуть требуемую конфигурацию или **сценарий в записной книжке** , чтобы ознакомиться с инструкциями по развертыванию или внести необходимые изменения, например имена классов хранения или типы служб. Щелкните **запустить все** в верхней части записной книжки.

## <a name="monitoring-the-creation-status"></a>Наблюдение за состоянием создания

Создание контроллера займет несколько минут. Вы можете отслеживать ход выполнения в другом окне терминала с помощью следующих команд:

> [!NOTE]
>  В примерах команд ниже предполагается, что вы создали контроллер данных и пространство имен Kubernetes с именем "Arc".  Если вы использовали другое имя пространства имен или контроллера данных, вы можете заменить "Arc" своим именем.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

Вы также можете проверить состояние создания любого конкретного модуля, выполнив команду, как показано ниже.  Это особенно полезно для устранения любых проблем.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Устранение неполадок при создании

Если у вас возникли роняли с созданием, ознакомьтесь с [руководством по устранению неполадок](troubleshoot-guide.md).