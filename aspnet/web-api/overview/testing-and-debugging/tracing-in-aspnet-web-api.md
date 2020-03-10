---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Śledzenie w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Pokazuje, jak włączyć śledzenie w interfejsie API sieci Web ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598554"
---
# <a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="01bd8-103">Śledzenie w programie ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="01bd8-103">Tracing in ASP.NET Web API 2</span></span>

<span data-ttu-id="01bd8-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="01bd8-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="01bd8-105">Podczas próby debugowania aplikacji sieci Web nie ma znaczenia dla dobrego zestawu dzienników śledzenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="01bd8-106">W tym samouczku pokazano, jak włączyć śledzenie w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01bd8-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="01bd8-107">Za pomocą tej funkcji można śledzić działanie platformy internetowego interfejsu API przed wywołaniem kontrolera i po nim.</span><span class="sxs-lookup"><span data-stu-id="01bd8-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="01bd8-108">Można go również użyć do śledzenia własnego kodu.</span><span class="sxs-lookup"><span data-stu-id="01bd8-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="01bd8-109">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="01bd8-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="01bd8-110">[Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (działa również z programem visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="01bd8-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="01bd8-111">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="01bd8-111">Web API 2</span></span>
> - [<span data-ttu-id="01bd8-112">Microsoft. AspNet. WebApi. Tracing</span><span class="sxs-lookup"><span data-stu-id="01bd8-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="01bd8-113">Włącz śledzenie system. Diagnostics w interfejsie Web API</span><span class="sxs-lookup"><span data-stu-id="01bd8-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="01bd8-114">Najpierw utworzymy nowy projekt aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01bd8-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="01bd8-115">W programie Visual Studio w menu **plik** wybierz pozycję **Nowy** **projekt** > .</span><span class="sxs-lookup"><span data-stu-id="01bd8-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="01bd8-116">W obszarze **Szablony**, **Sieć Web**wybierz pozycję **aplikacja sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="01bd8-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="01bd8-117">Wybierz szablon projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01bd8-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="01bd8-118">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie polecenie **Zarządzanie pakietem**.</span><span class="sxs-lookup"><span data-stu-id="01bd8-118">From the **Tools** menu, select **NuGet Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="01bd8-119">W oknie Konsola Menedżera pakietów wpisz następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="01bd8-120">Pierwsze polecenie instaluje najnowszy pakiet śledzenia interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01bd8-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="01bd8-121">Aktualizuje również podstawowe pakiety interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01bd8-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="01bd8-122">Drugie polecenie aktualizuje pakiet WebApi. WebHost do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="01bd8-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="01bd8-123">Jeśli chcesz wskazać określoną wersję interfejsu API sieci Web, Użyj flagi-Version podczas instalacji pakietu śledzenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>

<span data-ttu-id="01bd8-124">Otwórz plik WebApiConfig.cs w folderze Start\_.</span><span class="sxs-lookup"><span data-stu-id="01bd8-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="01bd8-125">Dodaj następujący kod do metody **register** .</span><span class="sxs-lookup"><span data-stu-id="01bd8-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="01bd8-126">Ten kod dodaje klasę [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) do potoku interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01bd8-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="01bd8-127">Klasa **SystemDiagnosticsTraceWriter** zapisuje ślady do [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="01bd8-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="01bd8-128">Aby wyświetlić ślady, uruchom aplikację w debugerze.</span><span class="sxs-lookup"><span data-stu-id="01bd8-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="01bd8-129">W przeglądarce przejdź do strony `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="01bd8-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="01bd8-130">Instrukcje śledzenia są zapisywane w oknie danych wyjściowych w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01bd8-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="01bd8-131">(Z menu **Widok** wybierz pozycję **dane wyjściowe**).</span><span class="sxs-lookup"><span data-stu-id="01bd8-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="01bd8-132">Ponieważ **SystemDiagnosticsTraceWriter** zapisuje ślady do **System. Diagnostics. Trace**, można zarejestrować dodatkowe detektory śledzenia; na przykład, aby zapisać ślady w pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="01bd8-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="01bd8-133">Aby uzyskać więcej informacji na temat składników zapisywania śladów, zobacz temat [detektory śledzenia](https://msdn.microsoft.com/library/4y5y10s7.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="01bd8-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="01bd8-134">Konfigurowanie SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="01bd8-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="01bd8-135">Poniższy kod przedstawia sposób konfigurowania składnika zapisywania śledzenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="01bd8-136">Istnieją dwa ustawienia, które można kontrolować:</span><span class="sxs-lookup"><span data-stu-id="01bd8-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="01bd8-137">Isverbose: w przypadku wartości false każdy ślad zawiera minimalną ilość informacji.</span><span class="sxs-lookup"><span data-stu-id="01bd8-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="01bd8-138">Jeśli wartość jest równa true, ślady zawierają więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="01bd8-138">If true, traces include more information.</span></span>
- <span data-ttu-id="01bd8-139">MinimumLevel: ustawia minimalny poziom śledzenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="01bd8-140">Poziomy śledzenia, w kolejności, są debugowaniem, informacjami, ostrzeżeniem, błędem i krytycznym.</span><span class="sxs-lookup"><span data-stu-id="01bd8-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="01bd8-141">Dodawanie śladów do aplikacji internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="01bd8-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="01bd8-142">Dodanie składnika zapisywania śledzenia daje natychmiastowy dostęp do śladów utworzonych przez potok interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01bd8-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="01bd8-143">Możesz również użyć składnika zapisywania śledzenia do śledzenia własnego kodu:</span><span class="sxs-lookup"><span data-stu-id="01bd8-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="01bd8-144">Aby uzyskać składnik zapisywania śledzenia, wywołaj **HttpConfiguration. Services. GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="01bd8-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="01bd8-145">Z poziomu kontrolera ta metoda jest dostępna za pośrednictwem właściwości **ApiController. Configuration** .</span><span class="sxs-lookup"><span data-stu-id="01bd8-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="01bd8-146">Aby napisać ślad, można wywołać metodę **ITraceWriter. Trace** , ale Klasa [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) definiuje niektóre metody rozszerzające, które są bardziej przyjazne.</span><span class="sxs-lookup"><span data-stu-id="01bd8-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="01bd8-147">Na przykład pokazana powyżej Metoda **info** tworzy ślad z **informacjami**o poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="01bd8-148">Infrastruktura śledzenia interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="01bd8-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="01bd8-149">W tej sekcji opisano sposób pisania niestandardowego składnika zapisywania śledzenia dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="01bd8-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="01bd8-150">Pakiet Microsoft. AspNet. WebApi. Tracing został utworzony na podstawie bardziej ogólnej infrastruktury śledzenia w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01bd8-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="01bd8-151">Zamiast używać elementu Microsoft. AspNet. WebApi. Tracing, można również podłączyć inne biblioteki śledzenia/rejestrowania, takie jak [nLOG](http://nlog-project.org/) lub [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="01bd8-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/logging library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="01bd8-152">Aby zebrać ślady, zaimplementuj Interfejs **ITraceWriter** .</span><span class="sxs-lookup"><span data-stu-id="01bd8-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="01bd8-153">Oto prosty przykład:</span><span class="sxs-lookup"><span data-stu-id="01bd8-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="01bd8-154">Metoda **ITraceWriter. Trace** tworzy ślad.</span><span class="sxs-lookup"><span data-stu-id="01bd8-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="01bd8-155">Obiekt wywołujący określa kategorię i poziom śledzenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="01bd8-156">Kategoria może być dowolnym ciągiem zdefiniowanym przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="01bd8-156">The category can be any user-defined string.</span></span> <span data-ttu-id="01bd8-157">W ramach implementacji **śledzenia** należy wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="01bd8-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="01bd8-158">Utwórz nowy **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="01bd8-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="01bd8-159">Zainicjuj go przy użyciu żądania, kategorii i poziomu śledzenia, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="01bd8-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="01bd8-160">Te wartości są udostępniane przez obiekt wywołujący.</span><span class="sxs-lookup"><span data-stu-id="01bd8-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="01bd8-161">Wywołaj delegata *traceAction* .</span><span class="sxs-lookup"><span data-stu-id="01bd8-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="01bd8-162">Wewnątrz tego delegata oczekuje się, że obiekt wywołujący będzie wypełniał resztę **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="01bd8-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="01bd8-163">Napisz **TraceRecord**przy użyciu dowolnej techniki rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="01bd8-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="01bd8-164">W przykładzie pokazano po prostu wywołania do **System. Diagnostics. Trace**.</span><span class="sxs-lookup"><span data-stu-id="01bd8-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="01bd8-165">Ustawianie składnika zapisywania śledzenia</span><span class="sxs-lookup"><span data-stu-id="01bd8-165">Setting the Trace Writer</span></span>

<span data-ttu-id="01bd8-166">Aby włączyć śledzenie, należy skonfigurować internetowy interfejs API do korzystania z implementacji **ITraceWriter** .</span><span class="sxs-lookup"><span data-stu-id="01bd8-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="01bd8-167">Można to zrobić za pomocą obiektu **HttpConfiguration** , jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="01bd8-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="01bd8-168">Tylko jeden składnik zapisywania śledzenia może być aktywny.</span><span class="sxs-lookup"><span data-stu-id="01bd8-168">Only one trace writer can be active.</span></span> <span data-ttu-id="01bd8-169">Domyślnie interfejs API sieci Web ustawia &quot;No-op&quot; Tracer, który nic nie robi.</span><span class="sxs-lookup"><span data-stu-id="01bd8-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="01bd8-170">(&quot;No-op&quot; Tracer istnieje, tak że kod śledzenia nie musi sprawdzać, czy moduł zapisujący śledzenia ma **wartość null** przed zapisaniem śladu.)</span><span class="sxs-lookup"><span data-stu-id="01bd8-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="01bd8-171">Jak działa śledzenie interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="01bd8-171">How Web API Tracing Works</span></span>

<span data-ttu-id="01bd8-172">Śledzenie w interfejsie API sieci Web używa wzorca *elewacji* : gdy śledzenie jest włączone, interfejs API sieci Web otacza różne części potoku żądań z klasami, które wykonują wywołania śledzenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="01bd8-173">Na przykład podczas wybierania kontrolera potok używa interfejsu **IHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="01bd8-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="01bd8-174">Po włączeniu śledzenia potok wstawia klasę implementującą **IHttpControllerSelector** , ale wywołuje do rzeczywistej implementacji:</span><span class="sxs-lookup"><span data-stu-id="01bd8-174">With tracing enabled, the pipeline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Śledzenie interfejsu API sieci Web używa wzorca elewacji.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="01bd8-176">Korzyści wynikające z tego projektu obejmują:</span><span class="sxs-lookup"><span data-stu-id="01bd8-176">The benefits of this design include:</span></span>

- <span data-ttu-id="01bd8-177">Jeśli nie dodasz składnika zapisywania śladów, składniki śledzenia nie są tworzone i nie mają wpływu na wydajność.</span><span class="sxs-lookup"><span data-stu-id="01bd8-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="01bd8-178">Jeśli zastąpisz domyślne usługi, takie jak **IHttpControllerSelector** z własną implementacją niestandardową, nie będzie to miało wpływ na śledzenie, ponieważ śledzenie jest wykonywane przez obiekt otoki.</span><span class="sxs-lookup"><span data-stu-id="01bd8-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="01bd8-179">Możesz również zastąpić całą strukturę śledzenia interfejsu API sieci Web własnymi platformami niestandardowymi, zastępując domyślną usługę **ITraceManager** :</span><span class="sxs-lookup"><span data-stu-id="01bd8-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="01bd8-180">Zaimplementuj **ITraceManager. Initialize** , aby zainicjować system śledzenia.</span><span class="sxs-lookup"><span data-stu-id="01bd8-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="01bd8-181">Należy pamiętać, że spowoduje to zastąpienie *całej* struktury śledzenia, w tym wszystkich kodów śledzenia wbudowanych w interfejs API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01bd8-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
