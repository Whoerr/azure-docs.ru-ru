---
title: Миграция локального NAS в Синхронизация файлов Azure
description: Узнайте, как перенести файлы из локального сетевого хранилища (NAS) в гибридное облачное развертывание с помощью Синхронизация файлов Azure и файловых ресурсов Azure.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 03/19/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 2d531edeeae9e0dd7e392cae66d9e4d41c68dfa2
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/27/2021
ms.locfileid: "98882269"
---
# <a name="migrate-from-network-attached-storage-nas-to-a-hybrid-cloud-deployment-with-azure-file-sync"></a>Миграция из подключенного к сети хранилища (NAS) в гибридное облачное развертывание с Синхронизация файлов Azure

Синхронизация файлов Azure работает в расположениях с прямым подключением к хранилищу (DAS) и не поддерживает синхронизацию с расположениями сетевого хранилища (NAS).
В этом случае выполняется миграция необходимых файлов, и в этой статье описывается планирование и выполнение такой миграции.

## <a name="migration-goals"></a>Цели миграции

Целью является перемещение общих папок на устройстве NAS на сервер Windows. Затем используйте Синхронизация файлов Azure для гибридного облачного развертывания. Эту миграцию необходимо выполнить таким образом, чтобы гарантировать целостность рабочих данных, а также доступность во время миграции. Последний требует, чтобы время простоя не превышало минимального, так что оно может поместиться в или только немного больше, чем обычные периоды обслуживания.

## <a name="migration-overview"></a>Общие сведения о переносе

Как упоминалось в [статье Обзор миграции](storage-files-migration-overview.md)файлов Azure, важно использовать правильное средство копирования и подход. Устройство NAS предоставляет общие ресурсы SMB непосредственно в локальной сети. Средство Robocopy, встроенное в Windows Server, является лучшим способом перемещения файлов в этом сценарии миграции.

- Этап 1. [Определение необходимого количества файловых ресурсов Azure](#phase-1-identify-how-many-azure-file-shares-you-need)
- Этап 2. [предоставление подходящего локального сервера Windows Server](#phase-2-provision-a-suitable-windows-server-on-premises)
- Этап 3. [развертывание облачного ресурса синхронизация файлов Azure](#phase-3-deploy-the-azure-file-sync-cloud-resource)
- Этап 4. [развертывание ресурсов службы хранилища Azure](#phase-4-deploy-azure-storage-resources)
- Этап 5. [развертывание агента Синхронизация файлов Azure](#phase-5-deploy-the-azure-file-sync-agent)
- Этап 6. [настройка синхронизация файлов Azure в Windows Server](#phase-6-configure-azure-file-sync-on-the-windows-server)
- Этап 7. [Robocopy](#phase-7-robocopy)
- Этап 8. [вырезание пользователя](#phase-8-user-cut-over)

## <a name="phase-1-identify-how-many-azure-file-shares-you-need"></a>Этап 1. Определение необходимого количества файловых ресурсов Azure

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="phase-2-provision-a-suitable-windows-server-on-premises"></a>Этап 2. предоставление подходящего локального сервера Windows Server

* Создайте Windows Server 2019 — как минимум 2012 R2 — как виртуальную машину или физический сервер. Также поддерживается кластер с отработкой отказа Windows Server.
* Подготавливаете или добавляйте непосредственно подключенное хранилище (DAS по сравнению с NAS, которое не поддерживается).

    Объем предоставляемого хранилища может быть меньше, чем вы используете в настоящее время на устройстве NAS, если вы используете службу синхронизации файлов Azure для распределения по [уровням облака](storage-sync-cloud-tiering.md) .
    Однако при копировании файлов из большего пространства NAS на более короткий том Windows Server на более позднем этапе необходимо будет работать с пакетами:

    1. Перемещение набора файлов, помещая их на диск
    2. Разрешить синхронизацию файлов и участие в облаке по уровням облака
    3. Если на томе создано больше свободного пространства, перейдите к следующему пакету файлов. 
    
    Этот подход к пакетной обработке можно избежать путем подготовки эквивалентного пространства на сервере Windows Server, занимаемого вашими файлами на устройстве NAS. Рассмотрите возможность дедупликации в NAS/Windows. Если вы не хотите окончательно зафиксировать этот большой объем хранилища на Windows Server, можно уменьшить размер тома после миграции и перед настройкой политик уровня облака. Это создает меньший локальный кэш файловых ресурсов Azure.

Конфигурация ресурсов (вычислений и ОЗУ) развертываемого Windows Server зависит главным образом от количества элементов (файлов и папок), которые будут синхронизироваться. При наличии каких-либо проблем рекомендуется использовать более высокую конфигурацию производительности.

[Узнайте, как масштабировать Windows Server на основе количества элементов (файлов и папок), которые необходимо синхронизировать.](storage-sync-files-planning.md#recommended-system-resources)

> [!NOTE]
> Ранее связанная статья представляет собой таблицу с диапазоном памяти сервера (ОЗУ). Можно ориентироваться на меньшее число для сервера, но предполагается, что начальная синхронизация может занять значительно больше времени.

## <a name="phase-3-deploy-the-azure-file-sync-cloud-resource"></a>Этап 3. Развертывание облачного ресурса Синхронизация файлов Azure

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

## <a name="phase-4-deploy-azure-storage-resources"></a>Этап 4. Развертывание ресурсов службы хранилища Azure

На этом этапе просмотрите таблицу сопоставления из фазы 1 и используйте ее для подсчета правильного числа учетных записей хранения Azure и файловых ресурсов в них.

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

## <a name="phase-5-deploy-the-azure-file-sync-agent"></a>Этап 5. Развертывание агента Синхронизация файлов Azure

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

## <a name="phase-6-configure-azure-file-sync-on-the-windows-server"></a>Этап 6. Настройка Синхронизация файлов Azure в Windows Server

Зарегистрированный локальный сервер Windows должен быть готов к работе и подключен к Интернету для этого процесса.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!IMPORTANT]
> Распределение по уровням в облаке — это функция AFS, которая позволяет локальному серверу иметь меньше ресурсов хранилища, чем хранится в облаке, но имеет доступное полное пространство имен. Локально интересные данные также кэшируются локально для повышения производительности доступа. Распределение по уровням облака — это необязательная функция для Синхронизация файлов Azure "конечная точка сервера".

> [!WARNING]
> При подготовке меньшего объема хранилища на томах Windows Server, чем в данных, используемых на устройстве NAS, распределение по уровням облака является обязательным. Если вы не включаете распределение по уровням в облаке, сервер не будет освобождать место для хранения всех файлов. Задайте политику распределения, временно предназначенную для миграции, на 99% свободного места на томе. После завершения миграции обязательно вернитесь к параметрам уровня облака и задайте более долгосрочный полезный уровень.

Повторите шаги создания группы синхронизации и добавления соответствующей серверной папки в качестве конечной точки сервера для всех общих файловых ресурсов Azure и расположений серверов, которые необходимо настроить для синхронизации.

После создания всех конечных точек сервера Синхронизация будет работать. Вы можете создать тестовый файл и увидеть, как он синхронизируется из расположения на сервере с подключенным общим файловым ресурсом Azure (как описано в облачной конечной точке в группе синхронизации).

Оба расположения, папки сервера и файловые ресурсы Azure в противном случае пусты и ожидают данные в любом расположении. На следующем шаге будет начато копирование файлов в Windows Server для Синхронизация файлов Azure для их перемещения в облако. Если вы включили распределение по уровням в облаке, сервер начнет работать с файлами уровня, если вы исчерпали емкость локальных томов.

## <a name="phase-7-robocopy"></a>Этап 7. Robocopy

Базовый подход к миграции — это средство Robocopy от устройства NAS до Windows Server, а также Синхронизация файлов Azure к общим файловым ресурсам Azure.

Запустите первую локальную копию в целевой папке Windows Server:

* Найдите первое расположение на устройстве NAS.
* Найдите соответствующую папку на сервере Windows, на которой уже настроен Синхронизация файлов Azure.
* Запуск копирования с помощью средства Robocopy

Следующая команда Robocopy скопирует файлы из хранилища NAS в целевую папку Windows Server. Windows Server будет синхронизировать его с общими файловыми ресурсами Azure. 

Если вы подготовили меньше места на сервере Windows Server, чем файлы, заданные на устройстве NAS, вы настроили распределение по уровням облака. По мере заполнения локального тома Windows Server распределение по [уровням облака](storage-sync-cloud-tiering.md) будет запущено в, а файлы уровня, которые были успешно синхронизированы. Распределение по уровням облака создаст достаточно места для продолжения копирования с устройства NAS. Распределение по уровням в облаке выполняется один раз в час, чтобы увидеть, что синхронизировано, и освободить место на диске, чтобы получить доступ к свободному объему тома 99%.
Это возможно, так как средство Robocopy перемещает файлы быстрее, чем можно синхронизировать в облако и на уровне локально, что приводит к нехватке места на локальном диске. Команда Robocopy завершится ошибкой. Рекомендуется работать с общими ресурсами в последовательности, которая не позволяет это сделать. Например, не следует запускать задания Robocopy для всех общих ресурсов одновременно или только перемещать общие ресурсы, которые соответствуют текущему объему свободного пространства на сервере Windows Server, чтобы упомянуть несколько раз.

```console
Robocopy /MT:32 /UNILOG:<file name> /TEE /B /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Фон:

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      Позволяет использовать средство Robocopy для работы с многопоточными потоками. Значение по умолчанию — 8, максимальное — 128.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /УНИЛОГ:\<file name\>
   :::column-end:::
   :::column span="1":::
      Выводит состояние в файл журнала как Юникод (перезаписывает существующий журнал).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /ти
   :::column-end:::
   :::column span="1":::
      Вывод в окно консоли. Используется в сочетании с выходными данными в файл журнала.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /B
   :::column-end:::
   :::column span="1":::
      Запускает Robocopy в том же режиме, что и приложение резервного копирования. Это позволяет Robocopy перемещать файлы, к которым у текущего пользователя нет разрешений.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /мир
   :::column-end:::
   :::column span="1":::
      Позволяет выполнять эту команду Robocopy несколько раз, последовательно применяя к одному и тому же целевому объекту или назначению. Он определяет, что было скопировано ранее, и опускает его. Будут обработаны только изменения, добавления и *удаления*, произошедшие с момента последнего запуска. Если команда не выполнялась ранее, ничего не пропускается. Флаг */мир* является отличным вариантом для исходных расположений, которые по-прежнему используются и изменяются.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY: копифлаг [s]
   :::column-end:::
   :::column span="1":::
      достоверность копии файла (по умолчанию —/COPY: DAT), флаги копирования: D = данные, A = атрибуты, T = метки времени, S = безопасность = NTFS ACL, O = сведения о владельце, U = сведения аудита
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /копялл
   :::column-end:::
   :::column span="1":::
      Копировать все сведения о файле (эквивалентно/COPY: ДАТСАУ)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /ДКОПИ: копифлаг [s]
   :::column-end:::
   :::column span="1":::
      точность для копии каталогов (по умолчанию/ДКОПИ: DA), флаги копирования: D = данные, A = Attributes, T = метки времени
   :::column-end:::
:::row-end:::

## <a name="phase-8-user-cut-over"></a>Этап 8. Вырезание пользователя

При первом запуске команды Robocopy пользователи и приложения по-прежнему получают доступ к файлам на NAS и потенциально могут изменить их. Возможно, средство Robocopy обработало каталог, переходит к следующему, а затем пользователь в исходном расположении (NAS) добавляет, изменяет или удаляет файл, который сейчас не будет обрабатываться в текущем запуске средства Robocopy. Это ожидаемое поведение.

Первый запуск заключается в перемещении большого объема данных на сервер Windows Server и в облако с помощью Синхронизация файлов Azure. Эта первая копия может занять много времени, в зависимости от следующих:

* пропускная способность загрузки
* пропускная способность передачи
* скорость локальной сети и количество оптимальных потоков Robocopy.
* количество элементов (файлов и папок), которые должны быть обработаны средством Robocopy и Синхронизация файлов Azure

После завершения начального запуска выполните команду еще раз.

Второй раз он будет выполняться быстрее, так как ему нужно только передать изменения, произошедшие с момента последнего запуска. Во время этого второго запуска все еще могут накапливаться новые изменения.

Повторите эту процедуру, пока не будет удовлетворено, что время, затрачиваемое на выполнение Robocopy для определенного расположения, находится в приемлемом окне для простоя.

Если вы считаете допустимое время простоя и готовы перевести расположение NAS в автономный режим: чтобы перевести пользователя в режим "вне сети", вы можете изменить списки управления доступом на корневом ресурсе, чтобы пользователи больше не могли получить доступ к расположению, или выполнить другие действия, которые не позволяют изменить содержимое в этой папке на NAS.

Выполнить один последний цикл Robocopy. Он выберет все изменения, которые могли быть пропущены.
Время выполнения последнего шага зависит от скорости сканирования Robocopy. Вы можете оценить время (которое равно времени простоя), измеряя длительность предыдущего запуска.

Создайте общую папку в папке Windows Server и, возможно, настройте развертывание DFS-N, чтобы она указывала на нее. Не забудьте установить те же разрешения на уровне общего ресурса, что и в общем ресурсе SMB NAS. Если у вас есть присоединенный к домену NAS корпоративного класса, идентификаторы безопасности пользователя будут автоматически сопоставляться по мере того, как они существуют в Active Directory и Robocopy копирует файлы и метаданные с полной точностью. Если вы использовали локальных пользователей на NAS, необходимо повторно создать этих пользователей в качестве локальных пользователей Windows Server и сопоставлять существующие идентификаторы безопасности, которые средство Robocopy переместит на сервер Windows Server с идентификаторами безопасности новых локальных пользователей Windows Server.

Вы завершили миграцию общей папки или группы общих папок в общий корень или том. (В зависимости от вашего сопоставления из фазы 1)

Можно попытаться выполнить несколько из этих копий параллельно. Рекомендуется обрабатывать по одной общей папке Azure за раз.

> [!WARNING]
> После перемещения всех данных с сервера NAS на сервер Windows Server и завершения миграции: Вернитесь на ***все** _ группы синхронизации в портал Azure и измените значение процента свободного места на уровне облака на более подходящее для использования кэша, скажем, 20%. 

Политика свободного места тома уровня облака действует на уровне тома с потенциальной синхронизацией нескольких конечных точек сервера. Если вы забыли скорректировать свободное пространство даже на одной конечной точке сервера, то Sync продолжит применять наиболее ограниченное правило и попытается сохранить 99% свободного места на диске, что делает локальный кэш не так, как вы можете ожидать. Если это не ваша цель, чтобы иметь только пространство имен для тома, содержащего редко используемые архивные данные, и вы резервируете остальную часть дискового пространства для другого сценария.

## <a name="troubleshoot"></a>Диагностика

Наиболее вероятной причиной проблемы, с которой можно столкнуться, является то, что команда Robocopy завершается с параметром _ "том Full" * на стороне Windows Server. Распределение по уровням в облаке действует один раз в час, чтобы эвакуировать содержимое с локального диска Windows Server, синхронизированного. Его целью является достижение 99% свободного места на томе.

Разрешите выполнение синхронизации и распределение по уровням облака, освободите место на диске. Это можно заметить в проводнике на Windows Server.

При наличии достаточной емкости Windows Server повторное выполнение команды устранит проблему. При возникновении этой ситуации ничего не происходит, и вы можете легко продвигаться вперед. Неудобство выполнения команды является единственным следствием.

Чтобы устранить неполадки Синхронизация файлов Azure, просмотрите ссылку в следующем разделе.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о файловых ресурсах Azure и Синхронизация файлов Azure см. здесь. Следующие статьи содержат сведения о дополнительных параметрах, рекомендациях, а также о решении по устранению неполадок. В этих статьях содержатся ссылки на [документацию по общей папке Azure](storage-files-introduction.md) .

* [Общие сведения о AFS](./storage-sync-files-planning.md)
* [Рекомендации по развертыванию AFS](./storage-how-to-create-file-share.md)
* [Устранение неполадок с AFS](storage-sync-files-troubleshoot.md)