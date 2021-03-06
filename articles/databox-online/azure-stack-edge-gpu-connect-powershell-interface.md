---
title: Подключение к устройству GPU Pro с помощью интерфейса Windows PowerShell и управление им Microsoft Azure Stack Документация Майкрософт
description: Описывает, как подключиться к графическому процессору (GPU) Pro с помощью интерфейса Windows PowerShell и управлять им Azure Stack.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 10/06/2020
ms.author: alkohli
ms.openlocfilehash: 27af230f8fa157f76865bd38a48c17640491d7db
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/27/2021
ms.locfileid: "98896195"
---
# <a name="manage-an-azure-stack-edge-pro-gpu-device-via-windows-powershell"></a>Управление устройством GPU Azure Stack с помощью Windows PowerShell

Azure Stack пограничных решений Pro позволяет обрабатывать данные и передавать их по сети в Azure. В этой статье описываются некоторые задачи по настройке и управлению для устройства на Azure Stack пограничной Pro. Для управления устройством можно использовать портал Azure, локальный веб-интерфейс или интерфейс Windows PowerShell.

В этой статье рассматриваются способы подключения к интерфейсу PowerShell устройства и задачи, которые можно выполнять с помощью этого интерфейса. 


## <a name="connect-to-the-powershell-interface"></a>Подключитесь к интерфейсу PowerShell.

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Создать пакет поддержки.

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

<!--## Upload certificate

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

You can also upload IoT Edge certificates to enable a secure connection between your IoT Edge device and the downstream devices that may connect to it. There are three IoT Edge certificates (*.pem* format) that you need to install:

- Root CA certificate or the owner CA
- Device CA certificate
- Device key certificate

The following example shows the usage of this cmdlet to install IoT Edge certificates:

```
Set-HcsCertificate -Scope IotEdge -RootCACertificateFilePath "\\hcfs\root-ca-cert.pem" -DeviceCertificateFilePath "\\hcfs\device-ca-cert.pem\" -DeviceKeyFilePath "\\hcfs\device-key-cert.pem" -Credential "username"
```
When you run this cmdlet, you will be prompted to provide the password for the network share.

For more information on certificates, go to [Azure IoT Edge certificates](../iot-edge/iot-edge-certs.md) or [Install certificates on a gateway](../iot-edge/how-to-create-transparent-gateway.md).-->

## <a name="view-device-information"></a>Просмотр сведений об устройстве
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="view-gpu-driver-information"></a>Просмотр сведений о драйвере GPU

Если на устройстве настроена роль вычислений, можно также получить сведения о драйвере GPU с помощью интерфейса PowerShell. 

1. [Подключитесь к интерфейсу PowerShell](#connect-to-the-powershell-interface).
2. Используйте `Get-HcsGpuNvidiaSmi` для получения сведений о драйвере GPU для вашего устройства.

    В следующем примере показано использование этого командлета:

    ```powershell
    Get-HcsGpuNvidiaSmi
    ```
    Запишите сведения о драйвере из примера выходных данных этого командлета.

    ```powershell    
    +-----------------------------------------------------------------------------+    
    | NVIDIA-SMI 440.64.00    Driver Version: 440.64.00    CUDA Version: 10.2     |    
    |-------------------------------+----------------------+----------------------+    
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |    
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |    
    |===============================+======================+======================|    
    |   0  Tesla T4            On   | 000029CE:00:00.0 Off |                    0 |    
    | N/A   60C    P0    29W /  70W |   1539MiB / 15109MiB |      0%      Default |    
    +-------------------------------+----------------------+----------------------+    
    |   1  Tesla T4           On  | 0000AD50:00:00.0 Off |                    0 |
    | N/A   58C    P0    29W /  70W |    330MiB / 15109MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    ```

## <a name="enable-multi-process-service-mps"></a>Включение многопроцессной службы (MPS)

Многопроцессная служба (MPS) на видеопроцессорах NVIDIA предоставляет механизм, в котором графические процессоры могут совместно использоваться несколькими заданиями, в которых каждому заданию выделяется некоторый процент ресурсов GPU. MPS — это предварительная версия функции на устройстве с Azure Stack ребра Pro GPU. Чтобы включить MPS на устройстве, выполните следующие действия.

1. Прежде чем начать, убедитесь, что 

    1. Вы настроили и [активировали устройство Azure Stack пограничной Pro](azure-stack-edge-gpu-deploy-activate.md) с помощью Azure Stackного ресурса pro/шлюз Data Box в Azure.
    1. Вы [настроили вычисление на этом устройстве в портал Azure](azure-stack-edge-deploy-configure-compute.md#configure-compute).
    
1. [Подключитесь к интерфейсу PowerShell](#connect-to-the-powershell-interface).
1. Используйте следующую команду, чтобы включить MPS на устройстве.

    ```powershell
    Start-HcsGpuMPS
    ```

## <a name="reset-your-device"></a>выполнить сброс устройства;

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>Получение журналов вычислений

Если на устройстве настроена роль вычислений, можно также получить журналы вычислений с помощью интерфейса PowerShell.

1. [Подключитесь к интерфейсу PowerShell](#connect-to-the-powershell-interface).
2. Используйте `Get-AzureDataBoxEdgeComputeRoleLogs` для получения журналов вычислений для устройства.

    В следующем примере показано использование этого командлета:

    ```powershell
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username" -FullLogCollection    
    ```

    Ниже приведено описание параметров, используемых для командлета.
    - `Path`: Укажите сетевой путь к общей папке, в которой нужно создать пакет журнала вычислений.
    - `Credential`: Укажите имя пользователя для сетевой папки. При выполнении этого командлета необходимо указать пароль для общей папки.
    - `FullLogCollection`: Этот параметр гарантирует, что пакет журнала будет содержать все журналы вычислений. По умолчанию пакет журналов содержит только подмножество журналов.


## <a name="change-kubernetes-pod-and-service-subnets"></a>Изменение Kubernetes Pod и подсетей службы

По умолчанию Kubernetes Azure Stack на пограничном устройстве использует подсети 172.27.0.0/16 и 172.28.0.0/16 для модуля Pod и службы соответственно. Если эти подсети уже используются в сети, можно выполнить `Set-HcsKubeClusterNetworkInfo` командлет, чтобы изменить эти подсети.

Вы хотите выполнить эту настройку перед настройкой вычислений на основе портал Azure так как кластер Kubernetes создается на этом шаге.

1. [Подключитесь к интерфейсу PowerShell устройства](#connect-to-the-powershell-interface).
1. В интерфейсе PowerShell устройства выполните:

    `Set-HcsKubeClusterNetworkInfo -PodSubnet <subnet details> -ServiceSubnet <subnet details>`

    Замените тем <subnet details> диапазоном подсети, который вы хотите использовать. 

1. После выполнения этой команды можно использовать `Get-HcsKubeClusterNetworkInfo` команду, чтобы убедиться, что подсети Pod и службы изменились.

Ниже приведен пример выходных данных для этой команды.

```powershell
[10.100.10.10]: PS>Set-HcsKubeClusterNetworkInfo -PodSubnet 10.96.0.1/16 -ServiceSubnet 10.97.0.1/16
[10.100.10.10]: PS>Get-HcsKubeClusterNetworkInfo

Id                                   PodSubnet    ServiceSubnet
--                                   ---------    -------------
6dbf23c3-f146-4d57-bdfc-76cad714cfd1 10.96.0.1/16 10.97.0.1/16
[10.100.10.10]: PS>
```


## <a name="debug-kubernetes-issues-related-to-iot-edge"></a>Отладка Kubernetes проблем, связанных с IoT Edge

<!--When the Kubernetes cluster is created, there are two system namespaces created: `iotedge` and `azure-arc`. --> 

<!--### Create config file for system namespace

To troubleshoot, first create the `config` file corresponding to the `iotedge` namespace with `aseuser`.

Run the `Get-HcsKubernetesUserConfig -AseUser` command and save the output as `config` file (no file extension). Save the file in the `.kube` folder of your user profile on the local machine.

Following is the sample output of the `Get-HcsKubernetesUserConfig` command.

```PowerShell
[10.100.10.10]: PS>Get-HcsKubernetesUserConfig -AseUser
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJd01EVXhNekl4TkRRME5sb1hEVE13TURVeE1USXhORFEwTmxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBS0M1CjlJbzRSU2hudG90QUdxdjNTYmRjOVd4UmJDYlRzWXU5S0RQeU9xanVoZE1UUE9PcmROOGNoa0x4NEFyZkZaU1AKZithUmhpdWZqSE56bWhucnkvZlprRGdqQzQzRmV5UHZzcTZXeVVDV0FEK2JBdi9wSkJDbkg2MldoWGNLZ1BVMApqU1k0ZkpXenNFbzBaREhoeUszSGN3MkxkbmdmaEpEanBQRFJBNkRWb2pIaktPb29OT1J1dURvUHpiOTg2dGhUCkZaQXJMZjRvZXRzTEk1ZzFYRTNzZzM1YVhyU0g3N2JPYVVsTGpYTzFYSnpFZlZWZ3BMWE5xR1ZqTXhBMVU2b1MKMXVJL0d1K1ArY
===========CUT=========================================CUT===================
    server: https://compute.myasegpu1.wdshcsso.com:6443
    name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: aseuser
    name: aseuser@kubernetes
current-context: aseuser@kubernetes
kind: Config
preferences: {}
users:
- name: aseuser
    user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMwRENDQWJpZ0F3SUJBZ0lJY1hOTXRPU2VwbG93RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TURBMU1UTXlNVFEwTkRaYUZ3MHlNVEExTVRNeU1UVXhNVEphTUJJeApFREFPQmdOVkJBTVRCMkZ6WlhWelpYSXdnZ0VpTUEwR0NTcUdTSWIzRFFFQkFRVUFBNElCRHdBd2dnRUtBb0lCCkFRRHVjQ1pKdm9qNFIrc0U3a1EyYmVjNEJkTXdpUEhmU2R2WnNDVVY0aTRRZGY1Yzd0dkE3OVRSZkRLQTY1d08Kd0h0QWdlK3lLK0hIQ1Qyd09RbWtNek1RNjZwVFEzUlE0eVdtRDZHR1cWZWMExBR1hFUUxWWHRuTUdGCi0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg==

[10.100.10.10]: PS>
```
-->

На устройстве Azure Stack пограничной Pro с настроенной ролью вычислений можно устранить неполадки и отслеживать устройство с помощью двух разных наборов команд.

- Использование `iotedge` команд. Эти команды доступны для основных операций с устройством.
- Использование `kubectl` команд. Эти команды доступны для обширного набора операций с устройством.

Чтобы выполнить любой из приведенных выше команд, необходимо [подключиться к интерфейсу PowerShell](#connect-to-the-powershell-interface).

### <a name="use-iotedge-commands"></a>Использование `iotedge` команд

Чтобы просмотреть список доступных команд, [подключитесь к интерфейсу PowerShell](#connect-to-the-powershell-interface) и используйте `iotedge` функцию.

```powershell
[10.100.10.10]: PS>iotedge -?                                                                                                                           
Usage: iotedge COMMAND

Commands:
   list
   logs
   restart

[10.100.10.10]: PS>
```

Следующая таблица содержит краткое описание команд, доступных для `iotedge` :

|.  |Описание |
|---------|---------|
|`list`     | Вывод списка модулей         |
|`logs`     | Получение журналов модуля        |
|`restart`     | Завершение и перезапуск модуля         |


Чтобы получить список всех модулей, выполняющихся на устройстве, используйте `iotedge list` команду.

Ниже приведен пример выходных данных этой команды. Эта команда выводит список всех модулей, связанную конфигурацию и внешние IP-адреса, связанные с модулями. Например, можно получить доступ к приложению " **сервер** приложений" в `https://10.128.44.244` . 


```powershell
[10.100.10.10]: PS>iotedge list

NAME                   STATUS  DESCRIPTION CONFIG                                             EXTERNAL-IP
----                   ------  ----------- ------                                             -----
gettingstartedwithgpus Running Up 10 days  mcr.microsoft.com/intelligentedge/solutions:latest
iotedged               Running Up 10 days  azureiotedge/azureiotedge-iotedged:0.1.0-beta10    <none>
edgehub                Running Up 10 days  mcr.microsoft.com/azureiotedge-hub:1.0             10.128.44.243
edgeagent              Running Up 10 days  azureiotedge/azureiotedge-agent:0.1.0-beta10
webserverapp           Running Up 10 days  nginx:stable                                       10.128.44.244

[10.100.10.10]: PS>
```


### <a name="use-kubectl-commands"></a>Использование команд kubectl

На устройстве Azure Stack пограничной Pro с настроенной ролью вычислений `kubectl` доступны все команды для мониторинга и устранения неполадок модулей. Чтобы просмотреть список доступных команд, выполните `kubectl --help` команду из командного окна.

```PowerShell
C:\Users\myuser>kubectl --help

kubectl controls the Kubernetes cluster manager.

Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/

Basic Commands (Beginner):
    create         Create a resource from a file or from stdin.
    expose         Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
    run            Run a particular image on the cluster
    set            Set specific features on objects
    run-container  Run a particular image on the cluster. This command is deprecated, use "run" instead
==============CUT=============CUT============CUT========================

Usage:
    kubectl [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).

C:\Users\myuser>
```

Полный список `kubectl` команд см. по адресу [ `kubectl` Памятка](https://kubernetes.io/docs/reference/kubectl/cheatsheet/).


#### <a name="to-get-ip-of-service-or-module-exposed-outside-of-kubernetes-cluster"></a>Получение IP-адреса службы или модуля, предоставляемых за пределами кластера Kubernetes

Чтобы получить IP-адрес службы балансировки нагрузки или модулей, предоставляемых за пределами Kubernetes, выполните следующую команду:

`kubectl get svc -n iotedge`

Ниже приведен пример выходных данных всех служб или модулей, которые доступны за пределами кластера Kubernetes. 


```powershell
[10.100.10.10]: PS>kubectl get svc -n iotedge
NAME           TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)                                       AGE
edgehub        LoadBalancer   10.103.52.225   10.128.44.243   443:31987/TCP,5671:32336/TCP,8883:30618/TCP   34h
iotedged       ClusterIP      10.107.236.20   <none>          35000/TCP,35001/TCP                           3d8h
webserverapp   LoadBalancer   10.105.186.35   10.128.44.244   8080:30976/TCP                                16h

[10.100.10.10]: PS>
```
IP-адрес в столбце External IP соответствует внешней конечной точке службы или модуля. Кроме того, [внешний IP-адрес можно получить на панели мониторинга Kubernetes](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md#get-ip-address-for-services-or-modules).


#### <a name="to-check-if-module-deployed-successfully"></a>Проверка успешного развертывания модуля

Модули вычислений — это контейнеры, в которых реализована бизнес-логика. Модуль Kubernetes может работать с несколькими контейнерами. 

Чтобы проверить, успешно ли развернуто модуль вычислений, подключитесь к интерфейсу PowerShell устройства.
Выполните `get pods` команду и проверьте, выполняется ли контейнер (соответствующий модулю вычислений).

Чтобы получить список всех модулей Pod, выполняющихся в определенном пространстве имен, выполните следующую команду:

`get pods -n <namespace>`

Чтобы проверить модули, развернутые с помощью IoT Edge, выполните следующую команду:

`get pods -n iotedge`

Ниже приведен пример выходных данных всех модулей Pod, выполняемых в `iotedge` пространстве имен.

```
[10.100.10.10]: PS>kubectl get pods -n iotedge
NAME                        READY   STATUS    RESTARTS   AGE
edgeagent-cf6d4ffd4-q5l2k   2/2     Running   0          20h
edgehub-8c9dc8788-2mvwv     2/2     Running   0          56m
filemove-66c49984b7-h8lxc   2/2     Running   0          56m
iotedged-675d7f4b5f-9nml4   1/1     Running   0          20h

[10.100.10.10]: PS>
```

**Состояние состояния означает,** что все модули в пространстве имен выполняются, а значение **Готово** указывает число контейнеров, развернутых в Pod. В приведенном выше примере выполняются все модули Pod, а также выполняется развертывание всех модулей, развернутых в каждом из модулей Pod. 

Чтобы проверить модули, развернутые с помощью дуги Azure, выполните следующую команду:

`get pods -n azure-arc`

Кроме того, вы можете [подключиться к панели мониторинга Kubernetes для просмотра IOT EDGE или развертываний Azure Arc](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md#view-module-status).


Чтобы получить более подробные выходные данные определенного модуля Pod для заданного пространства имен, можно выполнить следующую команду:

`kubectl describe pod <pod name> -n <namespace>` 

Пример выходных данных показан здесь.

```
[10.100.10.10]: PS>kubectl describe pod filemove-66c49984b7 -n iotedge
Name:           filemove-66c49984b7-h8lxc
Namespace:      iotedge
Priority:       0
Node:           k8s-1hwf613cl-1hwf613/10.139.218.12
Start Time:     Thu, 14 May 2020 12:46:28 -0700
Labels:         net.azure-devices.edge.deviceid=myasegpu-edge
                net.azure-devices.edge.hub=myasegpu2iothub.azure-devices.net
                net.azure-devices.edge.module=filemove
                pod-template-hash=66c49984b7
Annotations:    net.azure-devices.edge.original-moduleid: filemove
Status:         Running
IP:             172.17.75.81
IPs:            <none>
Controlled By:  ReplicaSet/filemove-66c49984b7
Containers:
    proxy:
    Container ID:   docker://fd7975ca78209a633a1f314631042a0892a833b7e942db2e7708b41f03e8daaf
    Image:          azureiotedge/azureiotedge-proxy:0.1.0-beta8
    Image ID:       docker://sha256:5efbf6238f13d24bab9a2b499e5e05bc0c33ab1587d6cf6f289cdbe7aa667563
    Port:           <none>
    Host Port:      <none>
    State:          Running
        Started:      Thu, 14 May 2020 12:46:30 -0700
    Ready:          True
    Restart Count:  0
    Environment:
        PROXY_LOG:  Debug
=============CUT===============================CUT===========================
Volumes:
    config-volume:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      iotedged-proxy-config
    Optional:  false
    trust-bundle-volume:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      iotedged-proxy-trust-bundle
    Optional:  false
    myasesmb1local:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  myasesmb1local
    ReadOnly:   false
    myasesmb1:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  myasesmb1
    ReadOnly:   false
    filemove-token-pzvw8:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  filemove-token-pzvw8
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                    node.kubernetes.io/unreachable:NoExecute for 300s
Events:          <none>


[10.100.10.10]: PS>
```

#### <a name="to-get-container-logs"></a>Получение журналов контейнеров

Чтобы получить журналы для модуля, выполните следующую команду в интерфейсе PowerShell устройства:

`kubectl logs <pod_name> -n <namespace> --all-containers` 

Поскольку `all-containers` флаг будет выгружать все журналы для всех контейнеров, хорошим способом просмотреть последние ошибки является использование параметра `--tail 10` .

Ниже приведен пример выходных данных. 

```
[10.100.10.10]: PS>kubectl logs filemove-66c49984b7-h8lxc -n iotedge --all-containers --tail 10
DEBUG 2020-05-14T20:40:42Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:40:44Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:40:44Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:40:44Z: loop process - 1 events, 0.000s
DEBUG 2020-05-14T20:40:44Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:42:12Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:42:14Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:42:14Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:42:14Z: loop process - 1 events, 0.000s
DEBUG 2020-05-14T20:42:14Z: loop process - 0 events, 0.000s
05/14/2020 19:46:44: Info: Opening module client connection.
05/14/2020 19:46:45: Info: Open done.
05/14/2020 19:46:45: Info: Initializing with input: /home/input, output: /home/output, protocol: Amqp.
05/14/2020 19:46:45: Info: IoT Hub module client initialized.

[10.100.10.10]: PS>
```

### <a name="change-memory-processor-limits-for-kubernetes-worker-node"></a>Изменение памяти, ограничение на число процессоров для рабочего узла Kubernetes

Чтобы изменить ограничения памяти или процессора для рабочего узла Kubernetes, выполните следующие действия.

1. [Подключитесь к интерфейсу PowerShell устройства](#connect-to-the-powershell-interface).
1. Чтобы получить текущие ресурсы для рабочего узла и параметров роли, выполните следующую команду:

    `Get-AzureDataBoxEdgeRole`

    Ниже приведен пример выходных данных. Обратите внимание на значения параметров `Name` и `Compute` в `Resources` разделе. `MemoryInBytes` и `ProcessorCount` запишите текущее значение объема памяти и процессоров для рабочего узла Kubernetes.  

    ```powershell
    [10.100.10.10]: PS>Get-AzureDataBoxEdgeRole
    ImageDetail                : Name:mcr.microsoft.com/azureiotedge-agent
                                 Tag:1.0
                                 PlatformType:Linux
    EdgeDeviceConnectionString :
    IotDeviceConnectionString  :
    HubHostName                : ase-srp-007.azure-devices.net
    IotDeviceId                : srp-007-storagegateway
    EdgeDeviceId               : srp-007-edge
    Version                    :
    Id                         : 6ebeff9f-84c5-49a7-890c-f5e05520a506
    Name                       : IotRole
    Type                       : IOT
    Resources                  : Compute:
                                 MemoryInBytes:34359738368
                                 ProcessorCount:12
                                 VMProfile:
    
                                 Storage:
                                 EndpointMap:
                                 EndpointId:c0721210-23c2-4d16-bca6-c80e171a0781
                                 TargetPath:mysmbedgecloudshare1
                                 Name:mysmbedgecloudshare1
                                 Protocol:SMB
    
                                 EndpointId:6557c3b6-d3c5-4f94-aaa0-6b7313ab5c74
                                 TargetPath:mysmbedgelocalshare
                                 Name:mysmbedgelocalshare
                                 Protocol:SMB
                                 RootFileSystemStorageSizeInBytes:0
    
    HostPlatform               : KubernetesCluster
    State                      : Created
    PlatformType               : Linux
    HostPlatformInstanceId     : 994632cb-853e-41c5-a9cd-05b36ddbb190
    IsHostPlatformOwner        : True
    IsCreated                  : True    
    [10.100.10.10]: PS>
    ```
    
1. Чтобы изменить значения памяти и процессоров для рабочего узла, выполните следующую команду:

    Set-AzureDataBoxEdgeRoleCompute-Name <Name value from the output of Get-AzureDataBoxEdgeRole> -Memory <Value in Bytes> -ProcessorCount <нет. ядер>

    Ниже приведен пример выходных данных. 
    
    ```powershell
    [10.100.10.10]: PS>Set-AzureDataBoxEdgeRoleCompute -Name IotRole -MemoryInBytes 32GB -ProcessorCount 16
    
    ImageDetail                : Name:mcr.microsoft.com/azureiotedge-agent
                                 Tag:1.0
                                 PlatformType:Linux
    
    EdgeDeviceConnectionString :
    IotDeviceConnectionString  :
    HubHostName                : ase-srp-007.azure-devices.net
    IotDeviceId                : srp-007-storagegateway
    EdgeDeviceId               : srp-007-edge
    Version                    :
    Id                         : 6ebeff9f-84c5-49a7-890c-f5e05520a506
    Name                       : IotRole
    Type                       : IOT
    Resources                  : Compute:
                                 MemoryInBytes:34359738368
                                 ProcessorCount:16
                                 VMProfile:
    
                                 Storage:
                                 EndpointMap:
                                 EndpointId:c0721210-23c2-4d16-bca6-c80e171a0781
                                 TargetPath:mysmbedgecloudshare1
                                 Name:mysmbedgecloudshare1
                                 Protocol:SMB
    
                                 EndpointId:6557c3b6-d3c5-4f94-aaa0-6b7313ab5c74
                                 TargetPath:mysmbedgelocalshare
                                 Name:mysmbedgelocalshare
                                 Protocol:SMB
    
                                 RootFileSystemStorageSizeInBytes:0
    
    HostPlatform               : KubernetesCluster
    State                      : Created
    PlatformType               : Linux
    HostPlatformInstanceId     : 994632cb-853e-41c5-a9cd-05b36ddbb190
    IsHostPlatformOwner        : True
    IsCreated                  : True
    
    [10.100.10.10]: PS>    
    ```

При изменении использования памяти и процессора следуйте приведенным ниже рекомендациям.

- Объем памяти по умолчанию составляет 25% от спецификации устройства.
- Число процессоров по умолчанию составляет 30% от спецификации устройства.
- При изменении значений количества памяти и процессоров рекомендуется изменять значения от 15% до 65% памяти устройства и числа процессоров. 
- Мы рекомендуем использовать верхний предел, равный 65%, чтобы иметь достаточно ресурсов для системных компонентов. 

## <a name="connect-to-bmc"></a>Подключение к BMC

Контроллер управления основной платой (BMC) используется для удаленного мониторинга и управления устройством. В этом разделе описываются командлеты, которые можно использовать для управления конфигурацией контроллера BMC. Перед запуском любого из этих командлетов [подключитесь к интерфейсу PowerShell устройства](#connect-to-the-powershell-interface).

- `Get-HcsNetBmcInterface`: Используйте этот командлет для получения свойств конфигурации сети контроллера BMC, например,,, `IPv4Address` `IPv4Gateway` `IPv4SubnetMask` `DhcpEnabled` . 
    
    Пример выходных данных:
    
    ```powershell
    [10.100.10.10]: PS>Get-HcsNetBmcInterface
    IPv4Address   IPv4Gateway IPv4SubnetMask DhcpEnabled
    -----------   ----------- -------------- -----------
    10.128.53.186 10.128.52.1 255.255.252.0        False
    [10.100.10.10]: PS>
    ```
- `Set-HcsNetBmcInterface`. Этот командлет можно использовать в следующих двух случаях.

    - Используйте командлет, чтобы включить или отключить конфигурацию DHCP для BMC с помощью соответствующего значения `UseDhcp` параметра. 

        ```powershell
        Set-HcsNetBmcInterface -UseDhcp $true
        ```

        Пример выходных данных: 

        ```powershell
        [10.100.10.10]: PS>Set-HcsNetBmcInterface -UseDhcp $true
        [10.100.10.10]: PS>Get-HcsNetBmcInterface
        IPv4Address IPv4Gateway IPv4SubnetMask DhcpEnabled
        ----------- ----------- -------------- -----------
        10.128.54.8 10.128.52.1 255.255.252.0         True
        [10.100.10.10]: PS>
        ```

    - Используйте этот командлет для настройки статической конфигурации контроллера BMC. Можно указать значения для параметров `IPv4Address` , `IPv4Gateway` и `IPv4SubnetMask` . 
    
        ```powershell
        Set-HcsNetBmcInterface -IPv4Address "<IPv4 address of the device>" -IPv4Gateway "<IPv4 address of the gateway>" -IPv4SubnetMask "<IPv4 address for the subnet mask>"
        ```        
        
        Пример выходных данных: 

        ```powershell
        [10.100.10.10]: PS>Set-HcsNetBmcInterface -IPv4Address 10.128.53.186 -IPv4Gateway 10.128.52.1 -IPv4SubnetMask 255.255.252.0
        [10.100.10.10]: PS>Get-HcsNetBmcInterface
        IPv4Address   IPv4Gateway IPv4SubnetMask DhcpEnabled
        -----------   ----------- -------------- -----------
        10.128.53.186 10.128.52.1 255.255.252.0        False
        [10.100.10.10]: PS>
        ```    

- `Set-HcsBmcPassword`: Используйте этот командлет, чтобы изменить пароль контроллера BMC для `EdgeUser` . Имя пользователя- `EdgeUser` -чувствительно к регистру.

    Пример выходных данных: 

    ```powershell
    [10.100.10.10]: PS> Set-HcsBmcPassword -NewPassword "Password1"
    [10.100.10.10]: PS>
    ```

## <a name="exit-the-remote-session"></a>Выход из удаленного сеанса

Чтобы выйти из удаленного сеанса PowerShell, закройте окно PowerShell.

## <a name="next-steps"></a>Дальнейшие действия

- Разверните [Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-prep.md) на портале Azure.