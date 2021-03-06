---
title: Решения Oracle на виртуальных машинах Azure | Документация Майкрософт
description: Узнайте о поддерживаемых конфигурациях и ограничениях образов виртуальных машин Oracle в Microsoft Azure.
author: dbakevlar
ms.service: virtual-machines
ms.subservice: oracle
ms.collection: linux
ms.topic: article
ms.date: 05/12/2020
ms.author: kegorman
ms.openlocfilehash: 2f34e0bb3c4abcf4efba807f95decd798bbc1f86
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101669067"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Образы ВМ Oracle в Microsoft Azure и их развертывание

Эта статья содержит информацию о решениях Oracle, созданных на основе образов виртуальных машин, опубликованных Oracle в Azure Marketplace. Если вы заинтересованы в решениях для кросс-облачных решений с помощью облачной инфраструктуры Oracle, см. статью [Решения для приложений Oracle, интегрирующие Microsoft Azure и облачной инфраструктуры Oracle](oracle-oci-overview.md).

Чтобы получить список доступных образов, выполните следующую команду:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

По начиная с июня 2020 доступны следующие образы:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
oracle-database-19-3    Oracle       oracle-db-19300         Oracle:oracle-database-19-3:oracle-db-19300:19.3.0           19.3.0
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.00                             6.10.00
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.0                                6.8.0
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190529                         6.8.20190529
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.0                                6.9.0
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190529                         6.9.20190529
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.0                                7.3.0
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190529                         7.3.20190529
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.1                                7.4.1
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190529                         7.4.20190529
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.1                                7.5.1
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.2                                7.5.2
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190529                         7.5.20190529
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.2                                7.6.2
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.3                                7.6.3
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.4                                7.6.4
Oracle-Linux            Oracle       77                      Oracle:Oracle-Linux:77:7.7.1                                 7.7.1
Oracle-Linux            Oracle       77                      Oracle:Oracle-Linux:77:7.7.2                                 7.7.2
Oracle-Linux            Oracle       77                      Oracle:Oracle-Linux:77:7.7.3                                 7.7.3
Oracle-Linux            Oracle       77                      Oracle:Oracle-Linux:77:7.7.4                                 7.7.4
Oracle-Linux            Oracle       77                      Oracle:Oracle-Linux:77:7.7.5                                 7.7.5
Oracle-Linux            Oracle       77-ci                   Oracle:Oracle-Linux:77-ci:7.7.01                             7.7.01
Oracle-Linux            Oracle       77-ci                   Oracle:Oracle-Linux:77-ci:7.7.02                             7.7.02
Oracle-Linux            Oracle       77-ci                   Oracle:Oracle-Linux:77-ci:7.7.03                             7.7.03
Oracle-Linux            Oracle       78                      Oracle:Oracle-Linux:78:7.8.01                                7.8.01
Oracle-Linux            Oracle       8                       Oracle:Oracle-Linux:8:8.0.2                                  8.0.2
Oracle-Linux            Oracle       8-ci                    Oracle:Oracle-Linux:8-ci:8.0.11                              8.0.11
Oracle-Linux            Oracle       81                      Oracle:Oracle-Linux:81:8.1.0                                 8.1.0
Oracle-Linux            Oracle       81                      Oracle:Oracle-Linux:81:8.1.2                                 8.1.2
Oracle-Linux            Oracle       81-ci                   Oracle:Oracle-Linux:81-ci:8.1.0                              8.1.0
Oracle-Linux            Oracle       81-gen2                 Oracle:Oracle-Linux:81-gen2:8.1.11                           8.1.11
Oracle-Linux            Oracle       ol77-ci-gen2            Oracle:Oracle-Linux:ol77-ci-gen2:7.7.1                       7.7.1
Oracle-Linux            Oracle       ol77-gen2               Oracle:Oracle-Linux:ol77-gen2:7.7.01                         7.7.01
Oracle-Linux            Oracle       ol77-gen2               Oracle:Oracle-Linux:ol77-gen2:7.7.02                         7.7.02
Oracle-Linux            Oracle       ol78-gen2               Oracle:Oracle-Linux:ol78-gen2:7.8.11                         7.8.11
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
weblogic-122130-jdk8u3  Oracle       owls-122130-8u131-ol73  Oracle:weblogic-122130-jdk8u131-ol73:owls-122130-8u131-ol7   1.1.6
weblogic-122130-jdk8u4  Oracle       owls-122130-8u131-ol74  Oracle:weblogic-122130-jdk8u131-ol74:owls-122130-8u131-ol7   1.1.1
weblogic-122140-jdk8u6  Oracle       owls-122140-8u251-ol76  Oracle:weblogic-122140-jdk8u251-ol76:owls-122140-8u251-ol7   1.1.1
weblogic-141100-jdk116  Oracle       owls-141100-11_07-ol76  Oracle:weblogic-141100-jdk11_07-ol76:owls-141100-11_07-ol7   1.1.1
weblogic-141100-jdk8u6  Oracle       owls-141100-8u251-ol76  Oracle:weblogic-141100-jdk8u251-ol76:owls-141100-8u251-ol7   1.1.1
```

Это образы с использованием собственной лицензии, а значит вы будете оплачивать только использование ресурсов вычислений и хранения, а также сетевых ресурсов, связанных с работой виртуальной машины.  Вы должны иметь соответствующую лицензию на использование программного обеспечения Oracle, а также соглашение о текущей поддержке. Oracle гарантирует перемещение лицензий из локальной среды в Azure. Ознакомьтесь с опубликованным примечанием [Oracle и Майкрософт](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) о перемещении лицензий.

Пользователи могут также разрабатывать свои решения на основе пользовательского образа, созданных с нуля в Azure, или передать пользовательский образ из своей локальной среды.

## <a name="oracle-database-vm-images"></a>Образы виртуальной машины базы данных Oracle

Oracle поддерживает выполнение выпусков базы данных Oracle Database 12.1 и выше Standard и Enterprise в Azure с использованием образов виртуальных машин на базе Oracle Linux.  Чтобы повысить производительность рабочих нагрузок базы данных Oracle в Azure, правильно выберите размер образа виртуальной машины и используйте управляемые диски SSD (цен. категория "Ультра" и "Премиум"). Сведения о том, как быстро настроить и запустить базу данных Oracle в Azure с помощью опубликованного образа виртуальной машины Oracle, см. в статье [Создание базы данных Oracle 12c на виртуальной машине Azure](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Варианты конфигурации подключенного диска

Подключенные диски используют службу хранилища больших двоичных объектов Azure. Теоретически каждый диск уровня "Стандартный" способен выполнять максимум около 500 операций ввода-вывода в секунду (IOPS). Наше предложение с дисками уровня "Премиум", которое предпочтительно использовать для высокопроизводительных рабочих нагрузок базы данных, может обеспечивать до 5000 операций ввода-вывода в секунду на диск. Вы можете использовать один диск, если это отвечает вашим требованиям к производительности. Тем не менее эффективность производительности операций ввода-вывода в секунду можно повысить, использовав несколько подключенных дисков, распределив между ними данные базы данных, а затем применив Oracle Automatic Storage Management (ASM). Дополнительные сведения об Oracle ASM см. в обзоре [Oracle Automatic Storage](https://www.oracle.com/technetwork/database/index-100339.html). Пример установки и настройки Oracle ASM на виртуальной машине Linux в Azure см. в статье [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) (Настройка Oracle ASM в виртуальной машине Linux в Azure).

### <a name="shared-storage-configuration-options"></a>Параметры конфигурации общего хранилища

Azure NetApp Files было разработано в соответствии с основными требованиями к запуску высокопроизводительных рабочих нагрузок, таких как базы данных в облаке, и предоставляет;

- Собственная общая служба хранилища NFS Azure для выполнения рабочих нагрузок Oracle с помощью встроенного машинного клиента NFS или Oracle dNFS
- Масштабируемые уровни производительности, отражающие реальные требования к объему операций ввода-вывода в секунду
- Минимальная задержка
- Высокая доступность, высокая устойчивость и управляемость в масштабе, обычно необходимые для критически важных промышленных рабочих нагрузок (например, SAP и Oracle)
- Быстрое и эффективное резервное копирование и восстановление для достижения наиболее агрессивных уровней обслуживания RTO и RPO

Это возможно, так как Azure NetApp Files основано на системах флеш-массивов NetApp® ONTAP®, работающих в среде центра обработки данных Azure — в качестве собственной службы Azure. Результатом является идеальная технология хранения базы данных, которую можно подготовить и использовать так же, как и другие варианты службы хранилища Azure. Дополнительные сведения о развертывании томов Azure NetApp Files NFS и доступе к ним см. в разделе [Документация по Azure NetApp Files](../../../azure-netapp-files/index.yml). Рекомендации по работе с базой данных Oracle на Azure NetApp Files см. в статье [Руководство Oracle по наилучшим практикам развертывания Azure с помощью Azure NetApp Files](https://www.netapp.com/us/media/tr-4780.pdf).

## <a name="licensing-oracle-database--software-on-azure"></a>Лицензирование базы данных и ПО Oracle в Azure

Microsoft Azure является полномочной облачной средой для запуска базы данных Oracle. Таблица коэффициентов Oracle Core неприменима при лицензировании баз данных Oracle в облаке. Вместо этого при использовании виртуальных машин с включенной технологией Hyper-Threading для баз данных выпуска Enterprise Edition подсчитайте два виртуальных ЦП соответствующими одной лицензии на процессор Oracle, если для нее включено создание потоков (как указано в документе политики). Подробные сведения о политике можно найти [здесь](http://www.oracle.com/us/corporate/pricing/cloud-licensing-070579.pdf).
Для баз данных Oracle обычно требуются более высокие объемы памяти и операций ввода-вывода. Поэтому для этих рабочих нагрузок рекомендуются [виртуальные машины, оптимизированные для операций в памяти](../../sizes-memory.md). Для дальнейшей оптимизации рабочих нагрузок [ограниченные базовые виртуальные ЦП](../../constrained-vcpu.md) рекомендуются для рабочих нагрузок баз данных Oracle, требующих большого объема памяти, хранилища и пропускной способности ввода-вывода, но большого числа ядер.

При переносе программного обеспечения и рабочих нагрузок Oracle из локальной среды в Microsoft Azure Oracle предоставляет возможность перемещения лицензий, как указано в разделе [часто задаваемых вопросов об Oracle в Azure](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Рекомендации по высокой доступности и аварийному восстановлению

При использовании базы данных Oracle в Azure вы отвечаете за реализацию решения, обеспечивающего высокую доступность и аварийное восстановление во избежание простоев.

Высокую доступность и аварийное восстановление в Oracle Database Enterprise Edition (без использования решений RAC) можно реализовать в Azure с помощью [Data Guard, Active Data Guard](https://www.oracle.com/database/technologies/high-availability/dataguard.html) или [Oracle Golden Gate](https://www.oracle.com/technetwork/middleware/goldengate), разместив две базы данных на двух отдельных виртуальных машинах. Обе виртуальные машины должны находиться в одной [виртуальной сети](https://azure.microsoft.com/documentation/services/virtual-network/), чтобы они могли обращаться друг к другу через постоянный частный IP-адрес.  Кроме того, рекомендуется размещать виртуальные машины в одной группе доступности, так как это позволяет Azure помещать их в отдельные домены сбоя и домены обновления. Если требуется геоизбыточность, выполняйте репликацию этих двух баз данных между двумя разными регионами и подключать оба экземпляра через VPN-шлюз.

Руководство [Реализация Oracle Data Guard на виртуальной машине Azure под управлением Linux](configure-oracle-dataguard.md) описывает процедуру базовой установки в Azure.  

При использовании Oracle Data Guard высокую доступность можно обеспечить, разместив базу данных источника в одной виртуальной машине и базу данных получателя (резервную) в другой виртуальной машине и настроив одностороннюю репликацию между ними. В результате можно получить доступ на чтение к копии базы данных. При использовании Oracle GoldenGate можно настроить двунаправленную репликацию между двумя базами данных. Дополнительные сведения о настройке высокой доступности для баз данных с помощью этих инструментов см. в документации по [Active Data Guard](https://www.oracle.com/database/technologies/high-availability/dataguard.html) и [GoldenGate](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) на веб-сайте Oracle. Если вам требуется получить доступ на чтение и запись к копии базы данных, ознакомьтесь с документом [Oracle Active Data Guard](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

Руководство [Реализация Oracle GoldenGate на виртуальной машине Azure под управлением Linux](configure-oracle-golden-gate.md) описывает процедуру базовой установки в Azure.

Кроме наличия решения, разработанного в Azure, с высоким уровнем доступности и возможностью аварийного восстановления, вам необходима стратегия архивации для восстановления базы данных. В руководстве [Создание резервных копий и восстановление базы данных Oracle Database](./oracle-overview.md) описаны основные процедуры создания согласованной резервной копии.

## <a name="support-for-jd-edwards"></a>Поддержка JD Edwards

Согласно примечанию по поддержке Oracle [Doc ID 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4), JD Edwards EnterpriseOne версии 9.2 и выше поддерживает **любое открытое облачное предложение**, которое соответствует определенным `Minimum Technical Requirements` (минимальным техническим требованиям).  Необходимо создать пользовательские образы, соответствующие минимальным техническим требованиям к операционной системе и к совместимости с программными приложениями.

## <a name="oracle-weblogic-server-virtual-machine-offers"></a>Предложения виртуальных машин Oracle WebLogic Server

Oracle и Майкрософт совместно работают над тем, чтобы подключить сервер веб-логики к Azure Marketplace в виде коллекции предложений приложений Azure.  Эти предложения описаны в статье [Приложения Azure с серверов веб-логики Oracle](oracle-weblogic.md).

### <a name="oracle-weblogic-server-virtual-machine-images"></a>Образы виртуальных машин Oracle WebLogic Server

- **Кластеризация поддерживается только в выпуске Enterprise Edition.** Ваша лицензия дает право использовать кластеризацию WebLogic только при использовании выпуска Oracle WebLogic Server Enterprise Edition. Не используйте кластеризацию с выпуском Oracle WebLogic Server Standard Edition.
- **Не поддерживается многоадресная рассылка по UDP.** Azure поддерживает одноадресную рассылку по UDP, но не поддерживает ни многоадресную, ни широковещательную рассылку. Oracle WebLogic Server может использовать возможности одноадресной рассылки по UDP в Azure. Чтобы получить наилучшие результаты при использовании одноадресной рассылки по UDP, мы советуем не изменять размер кластера WebLogic и не размещать более 10 управляемых серверов.
- **Oracle WebLogic Server ожидает использования одинаковых общих и частных портов для доступа к каналу T3 (например, при использовании Enterprise JavaBeans).** Рассмотрим многоуровневый сценарий, при котором приложение уровня служб (EJB) выполняется в кластере Oracle WebLogic Server, состоящем из двух или более виртуальных машин в виртуальной сети с именем *SLWLS*. Клиентский уровень расположен в другой подсети в рамках одной виртуальной сети, выполняющей простую программу Java, которая пытается вызвать EJB на уровне служб. Так как необходимо сбалансировать нагрузку для уровня служб, нужно создать общедоступную конечную точку с балансировкой нагрузки для виртуальных машин в кластере Oracle WebLogic Server. Если заданный частный порт отличается от общедоступного порта (например, 7006:7008), появится следующая ошибка:

```bash
   [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

   Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]
```

   Это вызвано тем, что при любой установке удаленного доступа к каналу T3 Oracle WebLogic Server ожидает использования одинаковых портов балансировки нагрузки и портов управляемых серверов WebLogic. В предыдущем примере клиент обращается к порту 7006 (порт балансировки нагрузки), а управляемый сервер прослушивает порт 7008 (частный порт). Это ограничение применяется только для доступа к каналу T3, а не к HTTP.

   Чтобы избежать этой проблемы, используйте одно из следующих решений:

- Используйте частные и общие порты с одинаковыми номерами для конечных точек с балансировкой нагрузки, выделенных для получения доступа к каналу T3.
- Добавьте следующий параметр виртуальной машины Java при запуске Oracle WebLogic Server:

```bash
   -Dweblogic.rjvm.enableprotocolswitch=true
```

Связанные сведения доступны в статье базы знаний **860340.1** по адресу: <https://support.oracle.com>.

- **Ограничения для динамической кластеризации и балансировки нагрузки.** Предположим, что вы хотите использовать динамический кластер в Oracle WebLogic Server и сделать его доступным через единственную общедоступную конечную точку с балансировкой нагрузки в Azure. Для этого нужно использовать порт с определенным номером для каждого из управляемых серверов (номер порта не должен динамически назначаться из диапазона). Количество запускаемых управляемых серверов не должно превышать количество виртуальных машин, которые отслеживает администратор. То есть в каждой виртуальной машине не более одного управляемого сервера. Если количество серверов Oracle WebLogic в вашей конфигурации превышает количество виртуальных машин (то есть если несколько экземпляров Oracle WebLogic Server совместно используют одну и ту же виртуальную машину), то невозможно привязать к заданному номеру порта более одного экземпляра серверов Oracle WebLogic. Остальные экземпляры на этой виртуальной машине привязать не удастся. Остальные на этой виртуальной машине завершаются сбоем.

   Если настроить сервер администратора таким образом, чтобы управляемым серверам автоматически назначались уникальные номера портов, то будет невозможна балансировка нагрузки, так как Azure не поддерживает сопоставление одного общедоступного порта с несколькими частными портами, которое потребовалось бы для этой конфигурации.
- **Несколько экземпляров сервера Oracle Weblogic Server на виртуальной машине.** В зависимости от требований вашего развертывания следует рассмотреть запуск нескольких экземпляров Oracle WebLogic Server на одной и той же виртуальной машине при условии, что ее размер достаточно большой. Например, на виртуальной машине среднего размера с двумя ядрами можно запустить два экземпляра Oracle WebLogic Server. Обратите внимание, что мы все равно не рекомендуем использовать в архитектуре единые точки отказа, как может произойти при выполнении нескольких экземпляров Oracle WebLogic Server на одной виртуальной машине. Лучше использовать не менее двух виртуальных машин, на каждой из которых может выполняться несколько экземпляров Oracle WebLogic Server. При этом все экземпляры Oracle WebLogic Server могут быть частью одного и того же кластера. Тем не менее обратите внимание, что на данный момент невозможно использовать Azure для балансировки нагрузки конечных точек, предоставляемых такими развертываниями Oracle WebLogic Server в пределах одной и той же виртуальной машины, так как подсистема балансировки нагрузки Azure требует, чтобы серверы с балансировкой нагрузки были распределены между отдельными виртуальными машинами.

## <a name="oracle-jdk-virtual-machine-images"></a>Образы виртуальных машин JDK для Oracle

- **Последние обновления JDK 6 и 7.** Хотя мы советуем использовать последнюю общедоступную поддерживаемую версию Java (на данный момент это Java 8), в Azure также доступны образы JDK 6 и 7. Они предназначены для устаревших приложений, которые еще невозможно обновить до версии JDK 8. Хотя обновления для предыдущих образов JDK могут быть уже недоступными, благодаря сотрудничеству корпорации Майкрософт и компании Oracle образы JDK 6 и 7, предоставляемые Azure, будут содержать более свежее частное обновление, которое обычно предоставляется только избранной группе клиентов, поддерживаемых Oracle. Новые версии образов JDK, а также обновленные версии JDK 6 и 7 будут доступны со временем.

   Комплект JDK, доступный в этих образах JDK 6 и 7, а также виртуальные машины и образы, полученные из них, можно использовать только в Azure.
- **64-разрядный JDK.** Образы виртуальных машин Oracle WebLogic Server и Oracle JDK, предоставляемые Azure, содержат 64-разрядные версии Windows Server и JDK.

## <a name="next-steps"></a>Дальнейшие действия

Вы узнали о текущих решениях Oracle на основе образов виртуальных машин в Microsoft Azure. Теперь вы можете приступить к развертыванию своей первой базы данных Oracle в Azure.

> [!div class="nextstepaction"]
> [Создание соединителя базы данных Oracle в Azure](oracle-database-quick-create.md)
