---
title: Руководство. Передача данных в общие папки с помощью Azure Stack Edge Pro | Документация Майкрософт
description: Из этого руководства вы узнаете, как добавить общие папки на устройство Azure Stack Edge Pro и подключиться к ним, чтобы обеспечить передачу данных с этого устройства в Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 01/04/2021
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Azure Stack Edge Pro so I can use it to transfer data to Azure.
ms.openlocfilehash: afedbe739e955499f94d00a8ced940e7a4e9c27c
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2021
ms.locfileid: "97898699"
---
# <a name="tutorial-transfer-data-with-azure-stack-edge-pro"></a>Руководство по Передача данных с помощью Azure Stack Edge Pro

В этом руководстве описывается, как добавить общие папки на устройство Azure Stack Edge Pro и подключиться к ним. После добавления общих папок устройство Azure Stack Edge Pro может выполнять передачу данных в Azure.

Эта процедура может занять около 10 минут.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Добавление общей папки
> * Подключение к общей папке

 
## <a name="prerequisites"></a>Предварительные требования

Прежде чем добавлять общие папки в Azure Stack Edge Pro, убедитесь, что:

- Вы установили физическое устройство, как описано в статье [Установка Azure Stack Edge Pro](azure-stack-edge-deploy-install.md).

- Физическое устройство активировано, как описано в статье [Подключение, настройка и активация Azure Stack Edge Pro](azure-stack-edge-deploy-connect-setup-activate.md).


## <a name="add-a-share"></a>Добавление общей папки

Чтобы создать общие папки, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) выберите ресурс Azure Stack Edge и перейдите в раздел **Обзор**. Устройство должно быть подключено к сети. Выберите элемент **Шлюз для облачного хранилища**.

   ![Устройство, подключенное к сети](./media/azure-stack-edge-deploy-add-shares/device-online-1.png)

2. На панели команд для устройства выберите **+ Добавление общего файлового ресурса**.

   ![Добавление общей папки](./media/azure-stack-edge-deploy-add-shares/select-add-share-1.png)

3. На панели **Добавление общего файлового ресурса** выполните следующие действия.

    а. В поле **Имя** укажите уникальное имя для общей папки.  
    Имя общего ресурса может состоять только из цифр, дефисов и строчных букв. Оно должно содержать от 3 до 63 символов и начинаться с буквы или цифры. До и после дефиса нужно указать цифру или букву.
    
    b. Выберите **тип** для общей папки.  
    Вы можете выбрать тип **SMB** или **NFS** (тип SMB задан по умолчанию). SMB является стандартным для клиентов Windows, а NFS используется для клиентов Linux.  
    В зависимости от выбранного типа предоставления общего доступа (SMB или NFS) остальные параметры могут немного отличаться. 

    c. Предоставьте учетную запись хранения, где будет размещена общая папка. 

    > [!IMPORTANT]
    > Убедитесь, что требуемая учетная запись хранения Azure не имеет определенных политик неизменяемости, если она используется с устройством Azure Stack Edge Pro или Шлюза Data Box. См. сведения об [определении и администрировании политик неизменяемости для хранилища BLOB-объектов](../storage/blobs/storage-blob-immutability-policies-manage.md).
    
    d. В раскрывающемся списке **службы хранилища** выберите **Блочный BLOB-объект**, **Страничный BLOB-объект** или **Файлы**.  
    Тип выбранной службы зависит от формата данных, который вы хотите использовать в Azure. Так как мы хотим хранить данные в блочных BLOB-объектах, в этом примере мы выбрали **Блочный BLOB-объект**. Если вы выбрали **Страничный BLOB-объект**, убедитесь, что данные выровнены по размеру 512 байт. Например, VHDX всегда выравнивается по размеру 512 байт.

    д) Создайте контейнер BLOB-объектов или выберите существующий из раскрывающегося списка. Если вы создаете контейнер, укажите его имя. В случает отсутствия контейнера он создается в учетной записи хранения с уже только что созданным именем общей папки.
   
    е) В зависимости от типа предоставления общего доступа (SMB или NFS) выполните один из следующих шагов. 
     
    - **Общий ресурс SMB**. В разделе **Все привилегированные локальные пользователи** выберите **Создать** или **Использовать существующий**. При создании локального пользователя введите имя пользователя и пароль, а затем подтвердите пароль. Таким образом вы предоставите разрешения локальному пользователю. После начального назначения разрешений вы сможете изменять их в проводнике.

        Если установить флажок **Разрешить только операции чтения** для данных этой общей папки, можно указывать пользователей с правами только для чтения.

        ![Добавление общей папки SMB](./media/azure-stack-edge-deploy-add-shares/add-share-smb-1.png)
   
    - **Общий ресурс NFS**. Введите IP-адреса клиентов, которым разрешен доступ к общей папке.

        ![Добавление общей папки типа NFS](./media/azure-stack-edge-deploy-add-shares/add-share-nfs-1.png)
   
4. Щелкните **Создать**, чтобы создать общую папку.
    
    Вы получите уведомление о том, что общая папка создается. После создания общей папки с указанными параметрами плитка **Общие папки** обновится и в ней появится новая общая папка.
    

## <a name="connect-to-the-share"></a>Подключение к общей папке

Теперь можно подключиться к общим папкам, созданным на предыдущем шаге. В зависимости от выбранного типа общей папки (SMB или NFS) шаги могут отличаться.

### <a name="connect-to-an-smb-share"></a>Подключение к общей папке типа SMB

На клиентском компьютере Windows Server, подключенном к устройству Azure Stack Edge Pro, подключитесь к общей папке SMB, введя следующие команды:


1. В командной строке введите следующую команду:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

2. Введите пароль общей папки, когда он будет запрошен.  
   Здесь приведен пример выходных данных этой команды.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192)
    (c) 2017 Microsoft Corporation. All rights reserved.
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60':
    The command completed successfully.
    
    C: \Users\DataBoxEdgeUser>
    ```   


3. На клавиатуре одновременно нажмите клавиши Windows + R.

4. В окне **выполнения** введите `\\<device IP address>`, а затем нажмите кнопку **ОК**.  
   Откроется проводник, и вы увидите общие папки. В проводнике для просмотра содержимого дважды щелкните общую папку.
 
    ![Подключение к общей папке типа SMB](./media/azure-stack-edge-deploy-add-shares/connect-to-share2.png)

    Данные записываются в эти общие папки по мере их создания, а устройство отправляет эти данные в облако.

### <a name="connect-to-an-nfs-share"></a>Подключение к общей папке типа NFS

На клиентском компьютере Linux, подключенном к устройству Azure Stack Edge Pro, выполните следующие действия:

1. Убедитесь, что на этом клиенте установлен клиент NFSv4. Используйте следующую команду для установки NFS-клиента:

   `sudo apt-get install nfs-common`

    Дополнительные сведения см. в разделе об [установке клиента NFSv4](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client).

2. После установки NFS-клиента используйте следующую команду, чтобы подключить общую папку NFS, созданную на устройстве Azure Stack Edge Pro:

   `sudo mount -t nfs -o sec=sys,resvport <device IP>:/<NFS shares on device> /home/username/<Folder on local Linux computer>`

    > [!IMPORTANT]
    > Использование параметра `sync` при подключении общих папок ускоряет передачу больших файлов.
    > Прежде чем подключать общие папки, создайте каталоги, которые будут действовать как точки подключения на локальном компьютере. Эти каталоги не должны содержать никаких файлов или вложенных папок.

    В следующем примере показано, как выполнить подключение к папке на устройстве Azure Stack Edge Pro через NFS. `10.10.10.60` — IP-адрес устройства. Общая папка `mylinuxshare2` подключена на виртуальной машине ubuntuVM. `/home/databoxubuntuhost/edge` представляет собой точку подключения общей папки.

    `sudo mount -t nfs -o sec=sys,resvport 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> В этом выпуске следует учитывать такие нюансы:
> - Не поддерживается переименование файлов, созданных в общих папках. 
> - Удаление файлов из общих папок не приводит к удалению записи в учетной записи хранения.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве были рассмотрены такие темы относительно Azure Stack Edge Pro:

> [!div class="checklist"]
> * Добавление общей папки
> * Подключение к общей папке.

Чтобы узнать о преобразовании данных с помощью Azure Stack Edge Pro, перейдите к следующему руководству:

> [!div class="nextstepaction"]
> [Преобразование данных с помощью Azure Stack Edge Pro](./azure-stack-edge-deploy-configure-compute.md)