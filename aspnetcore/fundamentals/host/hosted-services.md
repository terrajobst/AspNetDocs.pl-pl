---
title: Zadania w tle z usług hostowanych w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: d10a335429752c1a52c1b3619adecc41725a819a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067670"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="0856f-103">Zadania w tle z usług hostowanych w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0856f-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="0856f-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0856f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0856f-105">W programie ASP.NET Core, można zaimplementować jako zadania w tle *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="0856f-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="0856f-106">Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="0856f-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="0856f-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="0856f-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="0856f-108">Zadanie w tle wykonywana przez czasomierz.</span><span class="sxs-lookup"><span data-stu-id="0856f-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="0856f-109">Hostowana usługa, która aktywuje [zakresu usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="0856f-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="0856f-110">Usługi o określonym zakresie służy iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="0856f-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="0856f-111">Umieszczonych w kolejce zadania w tle wykonywane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="0856f-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="0856f-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0856f-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0856f-113">Przykładowa aplikacja znajduje się w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="0856f-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="0856f-114">Sieci Web hosta &ndash; hosta sieci Web jest przydatne w przypadku hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="0856f-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="0856f-115">Kod przykładu przedstawiony w tym temacie pochodzą od hosta sieci Web wersję przykładu.</span><span class="sxs-lookup"><span data-stu-id="0856f-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="0856f-116">Aby uzyskać więcej informacji, zobacz [hosta sieci Web](xref:fundamentals/host/web-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="0856f-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="0856f-117">Ogólny hosta &ndash; ogólnego Host jest nowego w programie ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0856f-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="0856f-118">Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="0856f-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="0856f-119">Package</span><span class="sxs-lookup"><span data-stu-id="0856f-119">Package</span></span>

<span data-ttu-id="0856f-120">Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) pakietu.</span><span class="sxs-lookup"><span data-stu-id="0856f-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="0856f-121">Interfejs pomocą interfejsu IHostedService</span><span class="sxs-lookup"><span data-stu-id="0856f-121">IHostedService interface</span></span>

<span data-ttu-id="0856f-122">Implementowanie usług hostowanych <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="0856f-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="0856f-123">Interfejs definiuje dwie metody dla obiektów, które są zarządzane przez hosta:</span><span class="sxs-lookup"><span data-stu-id="0856f-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="0856f-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę, aby uruchomić zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="0856f-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="0856f-125">Korzystając z [hosta sieci Web](xref:fundamentals/host/web-host), `StartAsync` jest wywoływana po uruchomieniu serwera i [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="0856f-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="0856f-126">Korzystając z [ogólnego hosta](xref:fundamentals/host/generic-host), `StartAsync` jest wywoływana przed `ApplicationStarted` zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="0856f-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="0856f-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host działa łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="0856f-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="0856f-128">`StopAsync` zawiera logikę, aby zakończyć zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="0856f-128">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="0856f-129">Implementowanie <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) usuwania niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="0856f-129">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="0856f-130">Token anulowania ma domyślny pięciu drugi limit czasu do wskazania, że proces zamykania nie powinny już być bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="0856f-130">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="0856f-131">Jeśli zażądano anulowania w tokenie:</span><span class="sxs-lookup"><span data-stu-id="0856f-131">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="0856f-132">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja powinna zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="0856f-132">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="0856f-133">Wszystkie metody o nazwie w `StopAsync` powinna zwrócić natychmiast.</span><span class="sxs-lookup"><span data-stu-id="0856f-133">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="0856f-134">Jednak zadania nie są porzucona po żądania anulowania&mdash;obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="0856f-134">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="0856f-135">Jeśli aplikacja zostanie wyłączony nieoczekiwanie (na przykład aplikacji proces zakończy się niepowodzeniem), `StopAsync` nie może być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0856f-135">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="0856f-136">W związku z tym, wszystkie metody o nazwie lub operacje przeprowadzane w `StopAsync` nie mogą wystąpić.</span><span class="sxs-lookup"><span data-stu-id="0856f-136">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="0856f-137">Aby rozszerzyć domyślna pięć drugi zamykania wartość limitu czasu, należy ustawić:</span><span class="sxs-lookup"><span data-stu-id="0856f-137">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="0856f-138"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> Korzystając z ogólnego hosta.</span><span class="sxs-lookup"><span data-stu-id="0856f-138"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="0856f-139">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="0856f-139">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="0856f-140">Zamknięcie ustawienie limitu czasu hosta konfiguracji przy użyciu hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0856f-140">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="0856f-141">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="0856f-141">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="0856f-142">Usługa hostowana jest aktywowany po przy uruchamianiu aplikacji i zostanie poprawnie zamknięte przy zamykaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0856f-142">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="0856f-143">Jeśli podczas wykonywania zadania w tle, zostanie zgłoszony błąd `Dispose` powinna być wywoływana nawet wtedy, gdy `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0856f-143">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="0856f-144">Zadania w tle przekroczono limit czasu</span><span class="sxs-lookup"><span data-stu-id="0856f-144">Timed background tasks</span></span>

<span data-ttu-id="0856f-145">Zadanie w tle czasu sprawia, że użycie [System.Threading.Timer](xref:System.Threading.Timer) klasy.</span><span class="sxs-lookup"><span data-stu-id="0856f-145">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="0856f-146">Czasomierz wyzwala zadanie `DoWork` metody.</span><span class="sxs-lookup"><span data-stu-id="0856f-146">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="0856f-147">Czasomierz jest wyłączona na `StopAsync` i usunięty po usunięciu kontenera usług na `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="0856f-147">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="0856f-148">Usługa jest zarejestrowana w `Startup.ConfigureServices` z `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="0856f-148">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="0856f-149">Korzystanie z usługi o określonym zakresie w zadanie w tle</span><span class="sxs-lookup"><span data-stu-id="0856f-149">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="0856f-150">Aby użyć [zakresu usług](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `IHostedService`, utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="0856f-150">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="0856f-151">Zakres nie jest domyślnie tworzone dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="0856f-151">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="0856f-152">Usługa zadań w tle o określonym zakresie zawiera logikę zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="0856f-152">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="0856f-153">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> są wstrzykiwane do usługi:</span><span class="sxs-lookup"><span data-stu-id="0856f-153">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="0856f-154">Usługa hostowana tworzy zakres rozpoznawanie usługi zadania tła o określonym zakresie wywołać jej `DoWork` metody:</span><span class="sxs-lookup"><span data-stu-id="0856f-154">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="0856f-155">Usługi są zarejestrowane w usłudze `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0856f-155">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0856f-156">`IHostedService` Implementacja jest zarejestrowane w usłudze `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="0856f-156">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="0856f-157">Zadania w tle umieszczonych w kolejce</span><span class="sxs-lookup"><span data-stu-id="0856f-157">Queued background tasks</span></span>

<span data-ttu-id="0856f-158">Kolejki zadań tła opiera się na platformie .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowane być wbudowane dla platformy ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="0856f-158">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="0856f-159">W `QueueHostedService`, w kolejce zadania w tle są usuwane z kolejki i są stosowane jako <xref:Microsoft.Extensions.Hosting.BackgroundService>, czyli klasę bazową dla implementacji działa długo `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="0856f-159">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="0856f-160">Usługi są zarejestrowane w usłudze `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0856f-160">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0856f-161">`IHostedService` Implementacja jest zarejestrowane w usłudze `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="0856f-161">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="0856f-162">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="0856f-162">In the Index page model class:</span></span>

* <span data-ttu-id="0856f-163">`IBackgroundTaskQueue` Wprowadzony do konstruktora i ma przypisaną do `Queue`.</span><span class="sxs-lookup"><span data-stu-id="0856f-163">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="0856f-164"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> Wprowadzony i ma przypisaną do `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="0856f-164">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="0856f-165">Fabryka jest używany do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, który jest używany do tworzenia usług w obrębie zakresu.</span><span class="sxs-lookup"><span data-stu-id="0856f-165">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="0856f-166">Zakres jest utworzone w celu korzystania z aplikacji `AppDbContext` ( [zakresu usługi](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów bazy danych `IBackgroundTaskQueue` (usługi singleton).</span><span class="sxs-lookup"><span data-stu-id="0856f-166">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0856f-167">Gdy **Dodaj zadanie** przycisk jest zaznaczony na stronie indeksu `OnPostAddTask` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="0856f-167">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="0856f-168">`QueueBackgroundWorkItem` jest wywoływana, aby umieścić w kolejce elementu roboczego:</span><span class="sxs-lookup"><span data-stu-id="0856f-168">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="0856f-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0856f-169">Additional resources</span></span>

* [<span data-ttu-id="0856f-170">Implementowanie zadań w tle w mikrousługach za pomocą interfejsu IHostedService i klasa BackgroundService</span><span class="sxs-lookup"><span data-stu-id="0856f-170">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
