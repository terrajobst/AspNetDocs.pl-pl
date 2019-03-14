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
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Zadania w tle z usług hostowanych w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W programie ASP.NET Core, można zaimplementować jako zadania w tle *usługi hostowane*. Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu. Ten temat zawiera trzy przykłady usługi hostowanej:

* Zadanie w tle wykonywana przez czasomierz.
* Hostowana usługa, która aktywuje [zakresu usługi](xref:fundamentals/dependency-injection#service-lifetimes). Usługi o określonym zakresie służy iniekcji zależności.
* Umieszczonych w kolejce zadania w tle wykonywane sekwencyjnie.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja znajduje się w dwóch wersjach:

* Sieci Web hosta &ndash; hosta sieci Web jest przydatne w przypadku hostowania aplikacji sieci web. Kod przykładu przedstawiony w tym temacie pochodzą od hosta sieci Web wersję przykładu. Aby uzyskać więcej informacji, zobacz [hosta sieci Web](xref:fundamentals/host/web-host) tematu.
* Ogólny hosta &ndash; ogólnego Host jest nowego w programie ASP.NET Core 2.1. Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host) tematu.

## <a name="package"></a>Package

Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) pakietu.

## <a name="ihostedservice-interface"></a>Interfejs pomocą interfejsu IHostedService

Implementowanie usług hostowanych <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu. Interfejs definiuje dwie metody dla obiektów, które są zarządzane przez hosta:

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę, aby uruchomić zadanie w tle. Korzystając z [hosta sieci Web](xref:fundamentals/host/web-host), `StartAsync` jest wywoływana po uruchomieniu serwera i [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) zostanie wywołany. Korzystając z [ogólnego hosta](xref:fundamentals/host/generic-host), `StartAsync` jest wywoływana przed `ApplicationStarted` zostanie wywołany.

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host działa łagodne zamykanie. `StopAsync` zawiera logikę, aby zakończyć zadanie w tle. Implementowanie <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) usuwania niezarządzanych zasobów.

  Token anulowania ma domyślny pięciu drugi limit czasu do wskazania, że proces zamykania nie powinny już być bezpieczne. Jeśli zażądano anulowania w tokenie:

  * Wszystkie pozostałe operacje w tle, które wykonuje aplikacja powinna zostało przerwane.
  * Wszystkie metody o nazwie w `StopAsync` powinna zwrócić natychmiast.

  Jednak zadania nie są porzucona po żądania anulowania&mdash;obiekt wywołujący czeka na ukończenie wszystkich zadań.

  Jeśli aplikacja zostanie wyłączony nieoczekiwanie (na przykład aplikacji proces zakończy się niepowodzeniem), `StopAsync` nie może być wywoływana. W związku z tym, wszystkie metody o nazwie lub operacje przeprowadzane w `StopAsync` nie mogą wystąpić.

  Aby rozszerzyć domyślna pięć drugi zamykania wartość limitu czasu, należy ustawić:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> Korzystając z ogólnego hosta. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Zamknięcie ustawienie limitu czasu hosta konfiguracji przy użyciu hosta sieci Web. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.

Usługa hostowana jest aktywowany po przy uruchamianiu aplikacji i zostanie poprawnie zamknięte przy zamykaniu aplikacji. Jeśli podczas wykonywania zadania w tle, zostanie zgłoszony błąd `Dispose` powinna być wywoływana nawet wtedy, gdy `StopAsync` nie jest wywoływana.

## <a name="timed-background-tasks"></a>Zadania w tle przekroczono limit czasu

Zadanie w tle czasu sprawia, że użycie [System.Threading.Timer](xref:System.Threading.Timer) klasy. Czasomierz wyzwala zadanie `DoWork` metody. Czasomierz jest wyłączona na `StopAsync` i usunięty po usunięciu kontenera usług na `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Usługa jest zarejestrowana w `Startup.ConfigureServices` z `AddHostedService` — metoda rozszerzenia:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Korzystanie z usługi o określonym zakresie w zadanie w tle

Aby użyć [zakresu usług](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `IHostedService`, utworzyć zakres. Zakres nie jest domyślnie tworzone dla usługi hostowanej.

Usługa zadań w tle o określonym zakresie zawiera logikę zadanie w tle. W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> są wstrzykiwane do usługi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Usługa hostowana tworzy zakres rozpoznawanie usługi zadania tła o określonym zakresie wywołać jej `DoWork` metody:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Usługi są zarejestrowane w usłudze `Startup.ConfigureServices`. `IHostedService` Implementacja jest zarejestrowane w usłudze `AddHostedService` — metoda rozszerzenia:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Zadania w tle umieszczonych w kolejce

Kolejki zadań tła opiera się na platformie .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowane być wbudowane dla platformy ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

W `QueueHostedService`, w kolejce zadania w tle są usuwane z kolejki i są stosowane jako <xref:Microsoft.Extensions.Hosting.BackgroundService>, czyli klasę bazową dla implementacji działa długo `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Usługi są zarejestrowane w usłudze `Startup.ConfigureServices`. `IHostedService` Implementacja jest zarejestrowane w usłudze `AddHostedService` — metoda rozszerzenia:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

W klasie modelu strony indeksu:

* `IBackgroundTaskQueue` Wprowadzony do konstruktora i ma przypisaną do `Queue`.
* <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> Wprowadzony i ma przypisaną do `_serviceScopeFactory`. Fabryka jest używany do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, który jest używany do tworzenia usług w obrębie zakresu. Zakres jest utworzone w celu korzystania z aplikacji `AppDbContext` ( [zakresu usługi](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów bazy danych `IBackgroundTaskQueue` (usługi singleton).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Gdy **Dodaj zadanie** przycisk jest zaznaczony na stronie indeksu `OnPostAddTask` metoda jest wykonywana. `QueueBackgroundWorkItem` jest wywoływana, aby umieścić w kolejce elementu roboczego:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Implementowanie zadań w tle w mikrousługach za pomocą interfejsu IHostedService i klasa BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
