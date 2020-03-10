---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Wprowadzenie z OWIN i Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584673"
---
# <a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="bf891-102">Wprowadzenie do korzystania z interfejsu OWIN i projektu Katana</span><span class="sxs-lookup"><span data-stu-id="bf891-102">Getting Started with OWIN and Katana</span></span>

<span data-ttu-id="bf891-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bf891-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bf891-104">[Otwórz interfejs sieci Web dla platformy .NET (Owin)](http://owin.org/) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET.</span><span class="sxs-lookup"><span data-stu-id="bf891-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bf891-105">Łącząc serwer sieci Web z aplikacji, OWIN ułatwia tworzenie oprogramowania pośredniczącego na potrzeby programowania w sieci Web dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="bf891-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="bf891-106">Ponadto OWIN ułatwiają przenoszenie aplikacji sieci Web na inne hosty&#8212;na przykład samodzielnego udostępniania w usłudze systemu Windows lub w innym procesie.</span><span class="sxs-lookup"><span data-stu-id="bf891-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="bf891-107">OWIN to specyfikacja będąca własnością Wspólnoty, a nie implementacja.</span><span class="sxs-lookup"><span data-stu-id="bf891-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="bf891-108">Projekt Katana to zestaw składników OWIN typu open source opracowanych przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bf891-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="bf891-109">Ogólne omówienie programów OWIN i Katana można znaleźć w temacie [Omówienie projektu Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="bf891-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="bf891-110">W tym artykule przeskoczę do kodu, aby rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="bf891-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="bf891-111">W tym samouczku jest używana [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ale można również użyć programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="bf891-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="bf891-112">Niektóre z tych kroków różnią się w programie Visual Studio 2012, który należy zauważyć poniżej.</span><span class="sxs-lookup"><span data-stu-id="bf891-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="bf891-113">OWIN hosta w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="bf891-113">Host OWIN in IIS</span></span>

<span data-ttu-id="bf891-114">W tej sekcji będziemy hostować OWIN w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="bf891-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="bf891-115">Ta opcja zapewnia elastyczność i możliwość tworzenia potoku OWIN wraz z zestawem dojrzałych funkcji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="bf891-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="bf891-116">Przy użyciu tej opcji aplikacja OWIN jest uruchamiana w potoku żądania ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf891-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="bf891-117">Najpierw utwórz nowy projekt aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf891-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="bf891-118">(W programie Visual Studio 2012 użyj pustego typu projektu aplikacji sieci Web ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="bf891-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="bf891-119">W oknie dialogowym **Nowy projekt ASP.NET** wybierz **pusty** szablon.</span><span class="sxs-lookup"><span data-stu-id="bf891-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="bf891-120">Dodawanie pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="bf891-120">Add NuGet Packages</span></span>

<span data-ttu-id="bf891-121">Następnie Dodaj wymagane pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="bf891-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="bf891-122">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="bf891-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bf891-123">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bf891-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="bf891-124">Dodaj klasę uruchomieniową</span><span class="sxs-lookup"><span data-stu-id="bf891-124">Add a Startup Class</span></span>

<span data-ttu-id="bf891-125">Następnie Dodaj klasę uruchomieniową OWIN.</span><span class="sxs-lookup"><span data-stu-id="bf891-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="bf891-126">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj**, a następnie wybierz pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="bf891-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="bf891-127">W oknie dialogowym **Dodaj nowy element** wybierz pozycję **Klasa startowa Owin**.</span><span class="sxs-lookup"><span data-stu-id="bf891-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="bf891-128">Aby uzyskać więcej informacji na temat konfigurowania klasy startowej, zobacz [Owin Starting Class Detection](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="bf891-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="bf891-129">Dodaj następujący kod do metody `Startup1.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="bf891-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="bf891-130">Ten kod dodaje proste oprogramowanie pośredniczące do potoku OWIN, zaimplementowane jako funkcja, która odbiera wystąpienie **Microsoft. Owin. IOwinContext** .</span><span class="sxs-lookup"><span data-stu-id="bf891-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="bf891-131">Gdy serwer odbiera żądanie HTTP, potok OWIN wywołuje oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="bf891-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="bf891-132">Oprogramowanie pośredniczące ustawia typ zawartości odpowiedzi i zapisuje treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bf891-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="bf891-133">Szablon klasy uruchamiania OWIN jest dostępny w Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bf891-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="bf891-134">Jeśli używasz programu Visual Studio 2012, wystarczy dodać nową pustą klasę o nazwie `Startup1`i wkleić ją w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="bf891-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="bf891-135">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="bf891-135">Run the Application</span></span>

<span data-ttu-id="bf891-136">Naciśnij klawisz F5, aby rozpocząć debugowanie.</span><span class="sxs-lookup"><span data-stu-id="bf891-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="bf891-137">Program Visual Studio otworzy okno przeglądarki, aby `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="bf891-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="bf891-138">Strona powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="bf891-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="bf891-139">Samoobsługowe OWIN w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="bf891-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="bf891-140">Można łatwo przekonwertować tę aplikację z hostingu usług IIS na własne hosty w procesie niestandardowym.</span><span class="sxs-lookup"><span data-stu-id="bf891-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="bf891-141">W przypadku hostingu usług IIS program IIS działa zarówno jako serwer HTTP, jak i proces obsługujący usługę.</span><span class="sxs-lookup"><span data-stu-id="bf891-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="bf891-142">W przypadku samoobsługi aplikacja tworzy proces i używa klasy **odbiornika HttpListener** jako serwera http.</span><span class="sxs-lookup"><span data-stu-id="bf891-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="bf891-143">W programie Visual Studio Utwórz nową aplikację konsolową.</span><span class="sxs-lookup"><span data-stu-id="bf891-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="bf891-144">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bf891-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="bf891-145">Dodaj do projektu klasę `Startup1` z części 1 tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="bf891-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="bf891-146">Nie trzeba modyfikować tej klasy.</span><span class="sxs-lookup"><span data-stu-id="bf891-146">You don't need to modify this class.</span></span>

<span data-ttu-id="bf891-147">Zaimplementuj metodę `Main` aplikacji w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="bf891-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="bf891-148">Po uruchomieniu aplikacji konsolowej serwer uruchamia nasłuchiwanie `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="bf891-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="bf891-149">Jeśli przejdziesz do tego adresu w przeglądarce internetowej, zostanie wyświetlona strona "Hello World".</span><span class="sxs-lookup"><span data-stu-id="bf891-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="bf891-150">Dodaj diagnostykę OWIN</span><span class="sxs-lookup"><span data-stu-id="bf891-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="bf891-151">Pakiet Microsoft. Owin. Diagnostics zawiera oprogramowanie pośredniczące, które przechwytuje Nieobsłużone wyjątki i wyświetla stronę HTML z informacjami o błędzie.</span><span class="sxs-lookup"><span data-stu-id="bf891-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="bf891-152">Ta strona działa podobnie jak strona błędu ASP.NET, która jest czasami nazywana "[żółtym ekranem zgonu](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="bf891-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="bf891-153">Podobnie jak w przypadku YSOD, strona błędu Katana jest przydatna podczas opracowywania, ale dobrym rozwiązaniem jest wyłączenie jej w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="bf891-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="bf891-154">Aby zainstalować pakiet diagnostyczny w projekcie, wpisz następujące polecenie w oknie Konsola Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="bf891-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="bf891-155">Zmień kod w metodzie `Startup1.Configuration` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bf891-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="bf891-156">Teraz użyj kombinacji klawiszy CTRL + F5, aby uruchomić aplikację bez debugowania, aby program Visual Studio nie przerywał tego wyjątku.</span><span class="sxs-lookup"><span data-stu-id="bf891-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="bf891-157">Aplikacja zachowuje się tak samo jak wcześniej, dopóki nie przejdziesz do `http://localhost/fail`, w którym aplikacja zgłosi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="bf891-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="bf891-158">Strona błędu oprogramowanie pośredniczące przechwytuje wyjątek i wyświetla stronę HTML z informacjami o błędzie.</span><span class="sxs-lookup"><span data-stu-id="bf891-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="bf891-159">Możesz kliknąć karty, aby zobaczyć zmienne środowiskowe, ciąg zapytania, pliki cookie, nagłówek żądania i OWIN.</span><span class="sxs-lookup"><span data-stu-id="bf891-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="bf891-160">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bf891-160">Next Steps</span></span>

- [<span data-ttu-id="bf891-161">Wykrywanie klasy początkowej OWIN</span><span class="sxs-lookup"><span data-stu-id="bf891-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="bf891-162">Korzystanie z OWIN do samoobsługowego hostowania interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bf891-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="bf891-163">Korzystanie z OWIN do obsługi sygnałów z dowolnego hosta</span><span class="sxs-lookup"><span data-stu-id="bf891-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
