---
title: Wstrzykiwanie zależności w programach obsługi wymagań w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wstawić programach obsługi wymagań autoryzacji do aplikacji ASP.NET Core przy użyciu iniekcji zależności.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067583"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="e63a0-103">Wstrzykiwanie zależności w programach obsługi wymagań w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e63a0-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="e63a0-104">[Programy obsługi autoryzacji musi być zarejestrowana](xref:security/authorization/policies#handler-registration) w kolekcji usługi podczas konfigurowania (przy użyciu [wstrzykiwanie zależności](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="e63a0-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="e63a0-105">Załóżmy, że masz repozytorium reguł, który chcesz ocenić wewnątrz procedury obsługi autoryzacji i tego repozytorium został zarejestrowany w kolekcji usługi.</span><span class="sxs-lookup"><span data-stu-id="e63a0-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="e63a0-106">Autoryzacja i rozpoznawania wstrzyknąć, do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e63a0-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="e63a0-107">Na przykład, jeśli chcesz użyć ASP. NET przez rejestrowanie infrastruktury, które chcesz wstawić `ILoggerFactory` do programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="e63a0-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="e63a0-108">Program obsługi może wyglądać jak:</span><span class="sxs-lookup"><span data-stu-id="e63a0-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="e63a0-109">Czy zarejestrować program obsługi przy użyciu `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="e63a0-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="e63a0-110">Wystąpienie zostanie obsługi można utworzyć podczas uruchamiania aplikacji i będzie DI wstrzyknąć zarejestrowaną `ILoggerFactory` do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e63a0-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="e63a0-111">Programy obsługi, które używają programu Entity Framework nie może zostać zarejestrowana jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="e63a0-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
