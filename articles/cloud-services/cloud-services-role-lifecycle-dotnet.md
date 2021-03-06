---
title: Обработку событий жизненного цикла облачной службы (классическая модель) | Документация Майкрософт
description: Узнайте, как использовать методы жизненного цикла роли облачной службы в .NET, в том числе RoleEntryPoint, который предоставляет методы для реагирования на события жизненного цикла.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: b5aa4bd061647f63ebcc70109f0ba21b39e814cc
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98741338"
---
# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>Адаптируйте жизненный цикл веб-роли или рабочей роли в .NET

> [!IMPORTANT]
> [Облачные службы Azure (Расширенная поддержка)](../cloud-services-extended-support/overview.md) — это новая модель развертывания на основе Azure Resource Manager для продукта облачных служб Azure.После этого изменения облачные службы Azure, работающие в модели развертывания на основе Service Manager Azure, были переименованы как облачные службы (классические), и все новые развертывания должны использовать [облачные службы (Расширенная поддержка)](../cloud-services-extended-support/overview.md).

Создавая рабочую роль, вы расширяете класс [RoleEntryPoint](/previous-versions/azure/reference/ee758619(v=azure.100)) , который предоставляет вам методы переопределения, которые позволяют реагировать на события жизненного цикла. Этот класс необязателен для веб-ролей, поэтому его следует использовать для реагирования на события жизненного цикла.

## <a name="extend-the-roleentrypoint-class"></a>Расширение класса RoleEntryPoint
Класс [RoleEntryPoint](/previous-versions/azure/reference/ee758619(v=azure.100)) включает в себя методы, вызываемые Azure при **запуске**, **выполнении** и **остановке** рабочей или веб-роли. Для управления ролями инициализации, последовательности завершения работы или потока выполнения при необходимости можно переопределить эти методы. 

При расширении класса **RoleEntryPoint** следует помнить о поведении следующих методов.

* Метод [OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) возвращает логическое значение, поэтому в этом методе можно вернуть **false** .
  
   Если код возвращает **false**, роль процесса немедленно завершается без запуска процессов завершения имеющихся последовательностей. В большинстве случаев нужно избегать возвращения значения **false** из метода **OnStart**.
* Любое непойманное исключение из перегрузки метода **RoleEntryPoint** считается необработанным исключением.
  
   Если исключение возникает в одном из методов жизненного цикла, Azure генерирует событие [UnhandledException](/dotnet/api/system.appdomain.unhandledexception) и процесс завершается. После перевода вашей роли в автономный режим Azure перезапустит ее. При возникновении необработанного исключения не вызывается событие [Stopping](/previous-versions/azure/reference/ee758136(v=azure.100)) и метод **OnStop** .

Если ваша роль не запускается или зацикливается между состояниями инициализации, занятости и остановки, возможно ваш код вызывает необработанное исключение в одном из событий жизненного цикла при каждом перезапуске роли. В этом случае используйте событие [UnhandledException](/dotnet/api/system.appdomain.unhandledexception) для определения причины исключения и его обработки. Кроме того, ваша роль могла возвращаться из метода [Run](/previous-versions/azure/reference/ee772746(v=azure.100)) , что приводит к ее перезапуску. Узнать больше о состоянии развертывания можно в разделе [Распространенные проблемы, которые вызывают повторное использование ролей](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [!NOTE]
> Если для разработки приложения вы используете **средства Azure для Microsoft Visual Studio**, то шаблоны проекта роли автоматически расширяют класс **RoleEntryPoint** в файлах *WebRole.cs* и *WorkerRole.cs*.
> 
> 

## <a name="onstart-method"></a>Метод OnStart
Метод **OnStart** вызывается при запуске экземпляра роли в Azure. Во время выполнения кода OnStart, экземпляр роли помечается как **Busy** и подсистема балансировки нагрузки не будет направлять к нему внешний трафик. Можно переопределить этот метод для выполнения инициализации, например для реализации обработчиков событий и запуска [диагностики Azure](cloud-services-how-to-monitor.md).

Если **OnStart** возвращает значение **true**, то экземпляр инициализирован успешно и Azure вызывает метод **RoleEntryPoint.Run**. Если **OnStart** возвращает значение **false**, роль немедленно завершается без выполнения запланированных выключений.

В следующем примере кода показано, как переопределить метод **OnStart**. Этот метод настраивает и запускает монитор диагностики при запуске экземпляра роли и устанавливает передачу данных журналов в учетную запись хранилища:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>Метод OnStop
Метод **OnStop** выполняется после выключения экземпляра роли и до завершения процесса. Можно переопределить этот метод с целью выполнения кода, необходимого для экземпляра роли и чистого завершения работы приложения.

> [!IMPORTANT]
> Код, исполняемый в методе **OnStop**, имеет ограниченное время на свое выполнение, если он вызван не после действия пользователя. По истечению данного времени процесс завершается, поэтому необходимо убедиться в том, что код в методе **OnStop** выполнятся быстро и будет допустимо, если он не выполнится до конца. Метод **OnStop** вызывается после отправки события **Stopping**.
> 
> 

## <a name="run-method"></a>Метод Run
Можно переопределить метод **Run** для реализации длительно выполняемого потока для экземпляра роли.

Переопределение метода **Run** не обязательно. Реализация по умолчанию запускает постоянно спящий поток. В случае переопределения метода **Run** код должен выполняться бесконечно. Если метод **Run** возвращает управление, то роль автоматически перезапускается; другими словами, Azure отправляет событие **Stopping** и вызывает метод **OnStop**, чтобы выполнить последовательность завершения работы до выключения роли.

### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Реализация методов жизненного цикла ASP.NET для веб-роли
Для управления жизненным циклом роли можно использовать методы ASP.NET в дополнение к тем, которые предоставляет класс **RoleEntryPoint**. Это может быть полезно для обеспечения совместимости при переносе существующего приложения ASP.NET в Azure. Методы жизненного цикла ASP.NET вызываются из методов **RoleEntryPoint**. Метод **Application\_Start** вызывается по завершении вызова метода **RoleEntryPoint.OnStart**. Метод **Application\_End** вызывается перед вызовом метода **RoleEntryPoint.OnStop**.

## <a name="next-steps"></a>Следующие шаги
Узнайте, как [создать пакет облачной службы](cloud-services-model-and-package.md).




