---
title: Inserimento di dipendenze nei gestori requisito in ASP.NET Core
author: rick-anderson
description: Informazioni su come inserire i gestori di requisiti di autorizzazione in un'app di ASP.NET Core mediante l'inserimento di dipendenza.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 4de7f0e49ade459968f8c30fbad76ce96a65815f
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="f92fc-103">Inserimento di dipendenze nei gestori requisito in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f92fc-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="f92fc-104">[I gestori di autorizzazione devono essere registrati](xref:security/authorization/policies#handler-registration) nella raccolta durante la configurazione del servizio (utilizzando [inserimento di dipendenze](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="f92fc-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="f92fc-105">Si supponga un repository di regole che si desidera valutare all'interno di un gestore di autorizzazione e il repository è stato registrato nella raccolta di servizio.</span><span class="sxs-lookup"><span data-stu-id="f92fc-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="f92fc-106">Autorizzazione risolveranno e inserire che nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="f92fc-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="f92fc-107">Ad esempio, se si desidera utilizzare le pagine ASP. Registrazione dell'infrastruttura che si desidera inserire NET `ILoggerFactory` nel gestore.</span><span class="sxs-lookup"><span data-stu-id="f92fc-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="f92fc-108">Un gestore di questo tipo potrebbe essere simile:</span><span class="sxs-lookup"><span data-stu-id="f92fc-108">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="f92fc-109">È necessario registrare il gestore con `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="f92fc-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="f92fc-110">Un'istanza dell'oggetto gestore essere creata all'avvio dell'applicazione e verrà eseguito DI inserire registrato `ILoggerFactory` del costruttore.</span><span class="sxs-lookup"><span data-stu-id="f92fc-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="f92fc-111">I gestori che utilizzano Entity Framework non devono essere registrati come singleton.</span><span class="sxs-lookup"><span data-stu-id="f92fc-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
