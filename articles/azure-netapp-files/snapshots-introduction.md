---
title: Как работают моментальные снимки Azure NetApp Files | Документация Майкрософт
description: Объясняется, как работают моментальные снимки Azure NetApp Files, включая способы создания моментальных снимков, способы восстановления моментальных снимков, использования моментальных снимков в параметрах репликации между регионами.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2021
ms.author: b-juche
ms.openlocfilehash: 526ef0af08833954aef4136716930cec0df40eea
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/06/2021
ms.locfileid: "99625253"
---
# <a name="how-azure-netapp-files-snapshots-work"></a>Принцип работы моментальных снимков Azure NetApp Files

В этой статье объясняется, как работают моментальные снимки Azure NetApp Files. Azure NetApp Filesная технология создания моментальных снимков обеспечивает стабильность, масштабируемость и быстроту восстановления, не влияя на производительность. Технология создания моментальных снимков Azure NetApp Files предоставляет основу для решения для защиты данных, включая восстановление отдельных файлов, восстановление томов и клоны, а также репликацию между регионами. 

Инструкции по использованию моментальных снимков томов см. в статье [Управление моментальными снимками с помощью Azure NetApp Files](azure-netapp-files-manage-snapshots.md). Рекомендации по управлению моментальными снимками в репликации между регионами см. в разделе [требования и рекомендации по использованию репликации между регионами](cross-region-replication-requirements-considerations.md).

## <a name="what-volume-snapshots-are"></a>Какие моментальные снимки тома  

Моментальный снимок Azure NetApp Files — это образ файловой системы на момент времени (том). Он может выступать в качестве идеального оперативного резервного копирования. С помощью моментального снимка можно [создать новый том](azure-netapp-files-manage-snapshots.md#restore-a-snapshot-to-a-new-volume), [восстановить файл](azure-netapp-files-manage-snapshots.md#restore-a-file-from-a-snapshot-using-a-client)или [восстановить том](azure-netapp-files-manage-snapshots.md#revert-a-volume-using-snapshot-revert). Для обеспечения согласованности приложений в конкретных данных приложения, хранящихся на Azure NetApp Files томах, могут потребоваться дополнительные действия.  

Моментальные снимки с низкими затратами становятся возможными благодаря уникальным возможностям технологии виртуализации томов, которая является частью Azure NetApp Files. Как и база данных, этот уровень использует указатели на реальные блоки данных на диске. Но, в отличие от базы данных, она не перезаписывает существующие блоки. Он записывает обновленные данные в новый блок и изменяет указатель. Моментальный снимок Azure NetApp Files просто манипулирует блочными указателями, создавая «замороженную» представление тома, доступное только для чтения, которое позволяет приложениям обращаться к более старым версиям файлов и иерархиям каталогов без специального программирования. Фактические блоки данных не копируются. Таким образом, моментальные снимки эффективны в течение времени, необходимого для их создания; они практически мгновенно, независимо от размера тома. Моментальные снимки также эффективны в пространстве для хранения данных. Они сами не занимают пробелов, и сохраняются только разностные блоки между моментальными снимками и активным томом. 

Основные понятия иллюстрируются на следующих диаграммах:  

![Схемы, отображающие ключевые понятия моментальных снимков](../media/azure-netapp-files/snapshot-concepts.png)

На схемах создается моментальный снимок на рис. 1a. На рис. 1b измененные данные записываются в *новый блок* , и указатель обновляется. Но указатель моментального снимка по-прежнему указывает на *ранее записанный блок*, что дает вам активное и историческое представление данных. Другой моментальный снимок показан на рис. 1c. Теперь у вас есть доступ к трем поколениям данных (динамические данные, моментальный снимок 2 и моментальный снимок 1 в порядке возраста), не требуя места в томе, которое потребовалось для трех полных копий. 

Моментальный снимок принимает только копию метаданных тома (*иноде Table*). Создание займет всего несколько секунд, независимо от размера тома, используемой емкости или уровня активности тома. Поэтому создание моментального снимка тома 100-Тиб выполняется так же, как и создание моментального снимка тома 100 гиб. После создания моментального снимка изменения в файлах данных отражаются в активной версии файлов в нормальном режиме.

В то же время блоки данных, на которые указывает моментальный снимок, остаются стабильными и неизменяемыми. Из-за характера "перенаправление при записи" Azure NetApp Files томов моментальный снимок не влияет на производительность, а сам по себе не потребляет пробелы. Можно хранить до 255 моментальных снимков на том с течением времени, все из которых доступны в виде доступных только для чтения и интернет-версий данных, используя как небольшую емкость, чем число измененных блоков между каждым снимком. Измененные блоки хранятся в активном томе. Блоки, на которые указывают ссылки в моментальных снимках, сохраняются в томе только для чтения, чтобы их можно было перенацелить только в том случае, если все указатели (в активном Томе и моментальных снимках) были удалены. Таким образом, использование тома со временем увеличится с помощью новых блоков данных или (измененных) блоков данных, сохраненных в моментальных снимках.

 На следующей схеме показаны моментальные снимки тома и использованное место с течением времени: 

![Диаграмма, на которой показаны моментальные снимки тома и использованное место с течением времени](../media/azure-netapp-files/snapshots-used-space-over-time.png)

Поскольку моментальный снимок тома записывает только изменения блоков с момента последнего моментального снимка, он предоставляет следующие основные преимущества:

* Моментальные снимки ***эффективно*** хранятся в хранилище.   
    Моментальные снимки потребляют минимальный объем хранилища, поскольку не копируют блоки данных всего тома. Два моментальных снимка, созданные в последовательности, отличаются только блоками, добавленными или измененными в интервале времени между ними. Это пошаговое поведение ограничивает потребление ресурсов емкости хранилища. Многие альтернативные реализации моментальных снимков используют тома хранилища, равные активной файловой системе, что вызывает требования к емкости хранилища. В зависимости от частоты изменения *уровня блоков на уровне* каждого приложения Azure NetApp Files моментальные снимки будут потреблять больше или меньше емкости, но только в измененных данных. Средний объем использования моментального снимка составляет только 1-5% от объема используемой емкости тома для многих томов приложений или до 20-30% для таких томов, как SAP HANA тома баз данных. Не забудьте [отслеживать использование тома и моментального снимка](azure-netapp-files-metrics.md#volumes) для потребления емкости моментальных снимков относительно количества созданных и обслуживаемых моментальных снимков.   

* Моментальные снимки ***быстро создаются, реплицируются, восстанавливаются или копируются***.   
    Создание, репликация, восстановление или клонирование моментального снимка занимает всего несколько секунд независимо от размера тома и уровня действий. Моментальный снимок тома можно создать [по требованию](azure-netapp-files-manage-snapshots.md#create-an-on-demand-snapshot-for-a-volume). Можно также использовать [политики моментальных снимков](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) , чтобы указать, когда Azure NetApp files должны автоматически создавать моментальные снимки и сколько моментальных снимков будет оставаться в томе.  Согласованность приложений может быть достигнута путем оркестрации моментальных снимков на уровне приложения, например с помощью [средства азакснап](azacsnap-introduction.md) для SAP HANA.

* Моментальные снимки не влияют на ***производительность*** тома.   
    Из-за особенностей технологии подкладки «перенаправление при записи» хранение или сохранение Azure NetApp Files моментальных снимков не влияет на производительность даже при работе с большими объемами данных. Удаление моментального снимка также не влияет на производительность в большинстве случаев. 

* Моментальные снимки обеспечивают ***масштабируемость*** , так как их можно создавать часто и хранить много.   
    Azure NetApp Filesные тома поддерживают до 255 моментальных снимков. Возможность хранения большого количества незначительных, часто создаваемых моментальных снимков повышает вероятность успешного восстановления требуемой версии данных.

* Моментальные снимки предоставляют ***видимость пользователя** _ и _ * возможности _восстановления файлов_* *.   
Высокая производительность, масштабируемость и стабильность Azure NetApp Files технологии моментальных снимков означает, что она обеспечивает идеальную оперативную архивацию для восстановления, управляемого пользователем. Моментальные снимки можно сделать доступными для пользователя в целях восстановления файлов, каталогов или томов. Дополнительные решения позволяют копировать резервные копии в автономное хранилище или выполнять [репликацию между регионами](cross-region-replication-introduction.md) для целей хранения или аварийного восстановления.

## <a name="ways-to-create-snapshots"></a>Способы создания моментальных снимков   

Для создания и обслуживания моментальных снимков можно использовать несколько методов.

* Вручную (по запросу) с помощью:   
    * Средства [портал Azure](azure-netapp-files-manage-snapshots.md#create-an-on-demand-snapshot-for-a-volume), [REST API](/rest/api/netapp/snapshots), [Azure CLI](/cli/azure/netappfiles/snapshot)или [PowerShell](/powershell/module/az.netappfiles/new-aznetappfilessnapshot)
    * Скрипты (см. [примеры](azure-netapp-files-solution-architectures.md#sap-tech-community-and-blog-posts))

* Автоматически с помощью:
    * Политики моментальных снимков с помощью средств [портал Azure](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies), [REST API](/rest/api/netapp/snapshotpolicies), [Azure CLI](/cli/azure/netappfiles/snapshot/policy)или [PowerShell](/powershell/module/az.netappfiles/new-aznetappfilessnapshotpolicy)
    * Средства создания моментальных снимков с одинаковыми приложениями, например [азакснап](azacsnap-introduction.md)

## <a name="how-volumes-and-snapshots-are-replicated-cross-region-for-dr"></a>Репликация томов и моментальных снимков между регионами для аварийного восстановления  

Azure NetApp Files поддерживает [репликацию по нескольким регионам](cross-region-replication-introduction.md) для аварийного восстановления (Dr). Azure NetApp Files репликация между регионами использует технологию SnapMirror. Только измененные блоки отправляются по сети в сжатом и эффективном формате. После инициирования межрегионной репликации между томами все содержимое тома (то есть собственно сохраненные блоки данных) переносится только один раз. Эта операция называется *перемещением по базовому плану*. После первоначальной передачи передаются только измененные блоки (как захваченные в моментальных снимках). Результатом является асинхронная реплика 1:1 исходного тома, включая все моментальные снимки. Это поведение соответствует полному и инкрементному механизму репликации. Эта технология позволяет сократить объем данных, необходимых для репликации в регионах, таким образом экономя расходы на передачу данных. Кроме того, сокращается время репликации. Можно достичь меньшей цели точки восстановления (RPO), так как больше моментальных снимков можно создавать и переносить чаще с ограниченной передачей данных. Кроме того, она требует использования механизмов репликации на основе узла, избегая стоимости лицензий на виртуальную машину и программное обеспечение.

На следующей схеме показан трафик моментальных снимков в сценариях репликации между регионами. 

![Диаграмма, показывающая трафик моментальных снимков в сценариях репликации между регионами](../media/azure-netapp-files/snapshot-traffic-cross-region-replication.png)

## <a name="ways-to-restore-data-from-snapshots"></a>Способы восстановления данных из моментальных снимков  

Технология создания моментальных снимков Azure NetApp Files значительно повышает частоту и надежность резервного копирования. Это приводит к минимальным затратам на производительность и может быть безопасно создано на активном томе. Моментальные снимки Azure NetApp Files обеспечивают почти мгновенное, безопасное и необязательное восстановление, управляемое пользователем. В этом разделе описываются различные способы доступа к данным и их восстановления из Azure NetApp Files моментальных снимков.

### <a name="restoring-files-or-directories-from-snapshots"></a>Восстановление файлов или каталогов из моментальных снимков 

Если [видимость пути к снимку](azure-netapp-files-manage-snapshots.md#edit-the-hide-snapshot-path-option) не задана `hidden` , пользователи могут напрямую обращаться к моментальным снимкам для восстановления после случайного удаления, повреждения или изменения данных. Безопасность файлов и каталогов сохраняются в моментальном снимке, а моментальные снимки — только для чтения. Таким образом, восстановление является безопасным и простым. 

На следующей схеме показан доступ к моментальному снимку файла или каталога. 

![Схема, показывающая доступ к файлу или каталогу для моментального снимка](../media/azure-netapp-files/snapshot-file-directory-access.png)

На схеме моментальный снимок 1 потребляет только разностные блоки между активным томом и моментом создания моментального снимка. Но при доступе к моментальному снимку через путь моментального снимка тома данные будут *отображаться* так, как если бы это был полный объем тома во время создания моментального снимка. При доступе к папкам моментальных снимков пользователи могут самостоятельно восстанавливать данные путем копирования файлов и каталогов из моментального снимка по выбору.

Аналогично, моментальные снимки в целевых томах репликации на разных регионах можно получить только для чтения при восстановлении данных в регионе АВАРИЙного восстановления.  

На следующей схеме показан доступ к моментальным снимкам в сценариях репликации между регионами. 

![Схема, показывающая доступ к моментальным снимкам при репликации между регионами](../media/azure-netapp-files/snapshot-access-cross-region-replication.png)

См. раздел [Восстановление файла из моментального снимка с помощью клиента](azure-netapp-files-manage-snapshots.md#restore-a-file-from-a-snapshot-using-a-client) о восстановлении отдельных файлов или каталогов из моментальных снимков.

### <a name="restoring-cloning-a-snapshot-to-a-new-volume"></a>Восстановление (клонирование) моментального снимка на новый том

Моментальные снимки Azure NetApp Files можно восстановить на отдельный, независимый том. Эта операция почти мгновенно, независимо от размера тома и потребляемой емкости. Вновь созданный том почти немедленно доступен для доступа, а фактические блоки данных тома и моментальных снимков копируются. В зависимости от размера и емкости тома этот процесс может занять значительное время, в течение которого родительский том и моментальный снимок не могут быть удалены. Однако после первоначального создания том уже доступен, а процесс копирования выполняется в фоновом режиме. Эта возможность позволяет быстро создавать тома для восстановления данных или клонирования томов для тестирования и разработки. По природе процесса копирования данных потребление пула емкости хранилища удваивается после завершения восстановления, и новый том будет показывать полную активную емкость исходного моментального снимка. После завершения этого процесса том будет независимым и несвязанным с исходным томом, а исходные тома и моментальный снимок можно будет управлять или удалять независимо от нового тома.

На следующей схеме показан новый том, созданный при восстановлении моментального снимка (клонирование).   

![Схема, на которой показан новый том, созданный путем восстановления моментального снимка](../media/azure-netapp-files/snapshot-restore-clone-new-volume.png)

Одни и те же операции можно выполнять с реплицированными моментальными снимками на томе аварийного восстановления (DR). Любой моментальный снимок можно восстановить на новый том, даже если репликация между регионами остается активной или выполняющейся. Эта возможность позволяет без нарушений производительности создавать среды тестирования и разработки в регионе аварийного восстановления, используя данные, в то время как реплицированные тома будут использоваться только в целях аварийного восстановления. Этот вариант использования позволяет изолировать тест и разработку от рабочей среды, устраняя потенциальные последствия в рабочих средах.  

На следующей схеме показано восстановление тома (клонирование) с помощью моментального снимка целевого тома аварийного восстановления при выполнении репликации между регионами.  

![Схема, показывающая восстановление тома с помощью моментального снимка целевого тома аварийного восстановления](../media/azure-netapp-files/snapshot-restore-clone-target-volume.png)

См. раздел [Восстановление моментального снимка в новый том](azure-netapp-files-manage-snapshots.md#restore-a-snapshot-to-a-new-volume) о операциях восстановления тома.

### <a name="restoring-reverting-a-snapshot-in-place"></a>Восстановление (возврат) моментального снимка на месте

В некоторых случаях, поскольку новый том будет потреблять емкость хранилища, создание нового тома из моментального снимка может быть ненужным или несоответствующим. Для быстрого восстановления после повреждения данных (например, для повреждения базы данных или атак с помощью программы-атаки) может оказаться более подходящим восстановление моментального снимка в самом томе. Эту операцию можно выполнить с помощью функции отката Azure NetApp Filesного моментального снимка. Эта функция позволяет быстро вернуть том в состояние, в котором он находился, когда был сделан определенный моментальный снимок. В большинстве случаев возврат тома выполняется гораздо быстрее, чем восстановление отдельных файлов из моментального снимка в активную файловую систему, особенно в больших Тиб томах. 

Восстановление моментального снимка тома практически мгновенно и занимает всего несколько секунд, даже для самых крупных томов. Метаданные активного тома (*иноде Table*) заменяются метаданными моментального снимка с момента создания моментального снимка, что приводит к откату тома к определенному моменту времени. Не нужно копировать блоки данных, чтобы изменения вступили в силу. Таким образом, больше свободного пространства, чем восстановление моментального снимка на новый том. 

На следующей схеме показан том, что тома возвращаются к более раннему снимку:  

![Схема, показывающая, что том возвращается к более раннему снимку](../media/azure-netapp-files/snapshot-volume-revert.png)

> [!IMPORTANT]
> Данные активных файловых систем, которые были записаны, и моментальные снимки, созданные после создания выбранного снимка, будут потеряны. Операция отката моментального снимка заменит все данные в целевом томе данными из выбранного моментального снимка. При выборе моментального снимка необходимо обратить внимание на содержимое моментального снимка и дату создания. Отменить операцию отмены изменений моментального снимка нельзя.  

Сведения об использовании этой функции см. [в разделе Отмена изменений тома с помощью моментального снимка](azure-netapp-files-manage-snapshots.md#revert-a-volume-using-snapshot-revert) .

## <a name="how-snapshots-are-deleted"></a>Удаление моментальных снимков 

Моментальные снимки используют емкость хранилища. Таким образом, они обычно не хранятся в течение неограниченного времени. Для защиты, хранения и восстановления данных несколько моментальных снимков (созданных в различные моменты времени) обычно хранятся в сети в течение определенной длительности в зависимости от требований к точке восстановления RPO, RTO и сроков обслуживания. Однако более старые моментальные снимки часто не обязательно должны храниться в службе хранилища. для освобождения места их необходимо удалить. Любой моментальный снимок можно удалить (не обязательно в порядке создания) администратором в любое время. 

> [!IMPORTANT]
> Невозможно отменить операцию удаления моментальных снимков. Необходимо хранить автономные копии тома для защиты и хранения данных. 

При удалении моментального снимка все указатели из этого снимка на существующие блоки данных будут удалены. Если блок данных не имеет больше указателей, указывающих на него (активный том или другие моментальные снимки в томе), блок данных возвращается в свободное место тома для будущего использования. Таким образом, удаление моментальных снимков обычно освобождает больше емкости тома, чем удаление данных из активного тома, так как блоки данных часто фиксируются в ранее созданных моментальных снимках. 

На следующей схеме показано воздействие на использование хранилища для удаления моментального снимка 3 из тома.  

![Схема, показывающая воздействие на потребление хранилища при удалении моментального снимка](../media/azure-netapp-files/snapshot-delete-storage-consumption.png)

Не забудьте [отслеживать потребление томов и снимков](azure-netapp-files-metrics.md#volumes) и понять, как взаимодействуют приложения, активные тома и моментальные снимки. 

См. раздел [удаление моментальных снимков](azure-netapp-files-manage-snapshots.md#delete-snapshots) об управлении удалением моментальных снимков. См. раздел [Управление политиками моментальных снимков](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) о том, как автоматизировать этот процесс.

## <a name="next-steps"></a>Дальнейшие действия

* [Управление моментальными снимками с помощью Azure NetApp Files](azure-netapp-files-manage-snapshots.md)
* [Мониторинг метрик томов и моментальных снимков](azure-netapp-files-metrics.md#volumes)
* [Устранение неполадок с политиками моментальных снимков](troubleshoot-snapshot-policies.md)
* [Ограничения ресурсов для службы Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Видео о Azure NetApp Filesных моментальных снимках 101](https://www.youtube.com/watch?v=uxbTXhtXCkw)
* [Общие сведения о снимке Azure NetApp Files](https://anfcommunity.com/2021/01/31/azure-netapp-files-snapshot-overview/)



