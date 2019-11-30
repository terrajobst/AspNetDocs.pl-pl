---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Samouczek: samodzielne hostowanie sygnałów | Microsoft Docs'
author: bradygaster
description: W tym samouczku pokazano, jak utworzyć serwer z własnym sygnałem 2 i jak połączyć się z nim za pomocą klienta JavaScript. Wersje oprogramowania używane w samouczku V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578568"
---
# <a name="tutorial-signalr-self-host"></a><span data-ttu-id="7722e-104">Samouczek: samodzielne hostowanie usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="7722e-104">Tutorial: SignalR Self-Host</span></span>

<span data-ttu-id="7722e-105">[Fletcher Patryka](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7722e-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[<span data-ttu-id="7722e-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="7722e-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="7722e-107">W tym samouczku pokazano, jak utworzyć serwer z własnym sygnałem 2 i jak połączyć się z nim za pomocą klienta JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7722e-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7722e-108">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="7722e-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="7722e-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7722e-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7722e-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7722e-110">.NET 4.5</span></span>
> - <span data-ttu-id="7722e-111">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="7722e-111">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="7722e-112">Korzystanie z programu Visual Studio 2012 z tym samouczkiem</span><span class="sxs-lookup"><span data-stu-id="7722e-112">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="7722e-113">Aby użyć programu Visual Studio 2012 z tym samouczkiem, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7722e-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="7722e-114">Zaktualizuj [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="7722e-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="7722e-115">Zainstaluj [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="7722e-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="7722e-116">W Instalatorze platformy sieci Web Wyszukaj i zainstaluj **ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="7722e-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="7722e-117">Spowoduje to zainstalowanie szablonów programu Visual Studio dla klas sygnalizujących, takich jak **Hub**.</span><span class="sxs-lookup"><span data-stu-id="7722e-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="7722e-118">Niektóre szablony (takie jak **Klasa startowa Owin**) nie będą dostępne; w takim przypadku zamiast tego należy użyć pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="7722e-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="7722e-119">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="7722e-119">Questions and comments</span></span>
>
> <span data-ttu-id="7722e-120">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="7722e-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7722e-121">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="7722e-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="7722e-122">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7722e-122">Overview</span></span>

<span data-ttu-id="7722e-123">Serwer sygnalizujący jest zwykle hostowany w aplikacji ASP.NET w usługach IIS, ale może być również własnym hostem (na przykład w aplikacji konsoli lub usługi systemu Windows) przy użyciu biblioteki samoobsługowej.</span><span class="sxs-lookup"><span data-stu-id="7722e-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="7722e-124">Ta biblioteka, podobnie jak w przypadku wszystkich sygnałów 2, jest oparta na OWIN ([Otwórz interfejs sieci Web dla platformy .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="7722e-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="7722e-125">OWIN definiuje abstrakcję między serwerami sieci Web .NET i aplikacjami sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7722e-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="7722e-126">OWIN oddziela aplikację sieci Web od serwera, co sprawia, że OWIN jest idealnym rozwiązaniem do samodzielnego udostępniania aplikacji sieci Web we własnym procesie poza programem IIS.</span><span class="sxs-lookup"><span data-stu-id="7722e-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="7722e-127">Przyczyny braku hostingu w usługach IIS obejmują:</span><span class="sxs-lookup"><span data-stu-id="7722e-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="7722e-128">Środowiska, w których usługi IIS są niedostępne lub niepożądane, takie jak istniejąca farma serwerów bez usług IIS.</span><span class="sxs-lookup"><span data-stu-id="7722e-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="7722e-129">Należy unikać narzutu na wydajność usług IIS.</span><span class="sxs-lookup"><span data-stu-id="7722e-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="7722e-130">Funkcja sygnalizująca jest dodawana do istniejącej aplikacji działającej w ramach usługi systemu Windows, roli procesu roboczego platformy Azure lub innego procesu.</span><span class="sxs-lookup"><span data-stu-id="7722e-130">SignalR functionality is to be added to an existing application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="7722e-131">Jeśli rozwiązanie jest opracowywane jako własne hosty ze względu na wydajność, zaleca się również testowanie aplikacji hostowanej w usługach IIS w celu określenia korzyści z wydajności.</span><span class="sxs-lookup"><span data-stu-id="7722e-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="7722e-132">Ten samouczek zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="7722e-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="7722e-133">Tworzenie serwera</span><span class="sxs-lookup"><span data-stu-id="7722e-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="7722e-134">Uzyskiwanie dostępu do serwera za pomocą klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="7722e-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="7722e-135">Tworzenie serwera</span><span class="sxs-lookup"><span data-stu-id="7722e-135">Creating the server</span></span>

<span data-ttu-id="7722e-136">W tym samouczku utworzysz serwer, który jest hostowany w aplikacji konsolowej, ale serwer może być hostowany w dowolnym procesie, takim jak usługa systemu Windows lub rola proces roboczy platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="7722e-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="7722e-137">Przykładowy kod służący do hostowania serwera sygnalizującego w usłudze systemu Windows znajduje się [w temacie sygnalizujący hosting samoobsługowy w usłudze systemu Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="7722e-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="7722e-138">Otwórz Visual Studio 2013 z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="7722e-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="7722e-139">Wybierz **plik**, **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="7722e-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="7722e-140">Wybierz pozycję **Windows** w **węźle C# Wizualizacja** w okienku **Szablony** , a następnie wybierz szablon **Aplikacja konsolowa** .</span><span class="sxs-lookup"><span data-stu-id="7722e-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="7722e-141">Nadaj nowemu projektowi nazwę "SignalRSelfHost" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7722e-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="7722e-142">Otwórz konsolę Menedżera pakietów NuGet, wybierając kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="7722e-142">Open the NuGet package manager console by selecting **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="7722e-143">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7722e-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="7722e-144">To polecenie dodaje biblioteki samoobsługowe do projektu sygnalizujące 2.</span><span class="sxs-lookup"><span data-stu-id="7722e-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="7722e-145">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7722e-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="7722e-146">To polecenie dodaje bibliotekę Microsoft. Owin. CORS do projektu.</span><span class="sxs-lookup"><span data-stu-id="7722e-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="7722e-147">Ta biblioteka będzie używana do obsługi wielu domen, która jest wymagana w przypadku aplikacji, które obsługują sygnalizację hosta i klienta strony sieci Web w różnych domenach.</span><span class="sxs-lookup"><span data-stu-id="7722e-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="7722e-148">Ponieważ będziesz hostować serwer sygnalizujący i klienta sieci Web na różnych portach, oznacza to, że między domenami musi być włączona komunikacja między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="7722e-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="7722e-149">Zastąp zawartość Program.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="7722e-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="7722e-150">Powyższy kod zawiera trzy klasy:</span><span class="sxs-lookup"><span data-stu-id="7722e-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="7722e-151">**Program**, w tym Metoda **Main** definiująca ścieżkę podstawową wykonania.</span><span class="sxs-lookup"><span data-stu-id="7722e-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="7722e-152">W tej metodzie aplikacja sieci Web typu **Startup** jest uruchamiana z określonym adresem URL (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="7722e-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="7722e-153">Jeśli w punkcie końcowym wymagane są zabezpieczenia, można zaimplementować protokół SSL.</span><span class="sxs-lookup"><span data-stu-id="7722e-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="7722e-154">Aby uzyskać więcej informacji [, zobacz jak: Konfigurowanie portu z certyfikatem SSL](https://msdn.microsoft.com/library/ms733791.aspx) .</span><span class="sxs-lookup"><span data-stu-id="7722e-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="7722e-155">**Uruchamianie**, Klasa zawierająca konfigurację dla serwera sygnalizującego (jedyną konfiguracją używaną w tym samouczku to wywołanie `UseCors`) i wywołanie `MapSignalR`, które tworzy trasy dla wszystkich obiektów Hub w projekcie.</span><span class="sxs-lookup"><span data-stu-id="7722e-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="7722e-156">**MyHub**, Klasa centrum sygnalizująca, którą aplikacja będzie dostarczać klientom.</span><span class="sxs-lookup"><span data-stu-id="7722e-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="7722e-157">Ta klasa ma jedną metodę, **wysyłając**, którą klienci będą dzwonić w celu rozgłaszania komunikatów do wszystkich innych podłączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="7722e-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="7722e-158">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7722e-158">Compile and run the application.</span></span> <span data-ttu-id="7722e-159">Adres, na którym uruchomiony jest serwer powinien być wyświetlany w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="7722e-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="7722e-160">Jeśli wykonanie nie powiedzie się z wyjątkiem `System.Reflection.TargetInvocationException was unhandled`, konieczne będzie ponowne uruchomienie programu Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="7722e-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="7722e-161">Przed przejściem do następnej sekcji Zatrzymaj aplikację.</span><span class="sxs-lookup"><span data-stu-id="7722e-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="7722e-162">Uzyskiwanie dostępu do serwera za pomocą klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="7722e-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="7722e-163">W tej sekcji użyjesz tego samego klienta JavaScript z [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="7722e-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="7722e-164">Po wprowadzeniu tylko jednej modyfikacji klient będzie mógł jawnie zdefiniować adres URL centrum.</span><span class="sxs-lookup"><span data-stu-id="7722e-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="7722e-165">W przypadku aplikacji samoobsługowych serwer może nie mieć takiego samego adresu jak adres URL połączenia (z powodu zwrotnych serwerów proxy i modułów równoważenia obciążenia), dlatego należy jawnie zdefiniować adres URL.</span><span class="sxs-lookup"><span data-stu-id="7722e-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="7722e-166">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj**, **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="7722e-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="7722e-167">Wybierz węzeł **Sieć Web** , a następnie wybierz szablon **aplikacja sieci Web ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="7722e-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="7722e-168">Nadaj projektowi nazwę "JavascriptClient" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7722e-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="7722e-169">Wybierz **pusty** szablon i pozostaw pozostałe opcje, które nie zostały zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="7722e-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="7722e-170">Wybierz pozycję **Utwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="7722e-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="7722e-171">W konsoli Menedżera pakietów wybierz projekt "JavascriptClient" z listy rozwijanej **Projekt domyślny** i wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7722e-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="7722e-172">To polecenie powoduje zainstalowanie bibliotek sygnalizujących i JQuery, które będą potrzebne w kliencie programu.</span><span class="sxs-lookup"><span data-stu-id="7722e-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="7722e-173">Kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj**, **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="7722e-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="7722e-174">Wybierz węzeł **Sieć Web** , a następnie wybierz pozycję Strona HTML.</span><span class="sxs-lookup"><span data-stu-id="7722e-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="7722e-175">Nadaj stronie nazwę **default. html**.</span><span class="sxs-lookup"><span data-stu-id="7722e-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="7722e-176">Zamień zawartość nowej strony HTML na następujący kod.</span><span class="sxs-lookup"><span data-stu-id="7722e-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="7722e-177">Sprawdź, czy odwołania do skryptów są zgodne ze skryptami w folderze skryptów projektu.</span><span class="sxs-lookup"><span data-stu-id="7722e-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="7722e-178">Poniższy kod (wyróżniony w powyższym przykładzie kodu) jest dodatkiem klienta używanym w samouczku pobieranie z gwiazdką (oprócz uaktualnienia kodu do wersji 2 beta).</span><span class="sxs-lookup"><span data-stu-id="7722e-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="7722e-179">Ten wiersz kodu jawnie ustawia podstawowy adres URL połączenia dla sygnalizującego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7722e-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="7722e-180">Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Ustaw projekty startowe..** .. Wybierz przycisk radiowy **wiele projektów startowych** , a następnie ustaw **akcję** **uruchamiania**obu projektów.</span><span class="sxs-lookup"><span data-stu-id="7722e-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="7722e-181">Kliknij prawym przyciskiem myszy pozycję "default. html" i wybierz pozycję **Ustaw jako stronę początkową**.</span><span class="sxs-lookup"><span data-stu-id="7722e-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="7722e-182">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7722e-182">Run the application.</span></span> <span data-ttu-id="7722e-183">Zostanie uruchomiony serwer i strona.</span><span class="sxs-lookup"><span data-stu-id="7722e-183">The server and page will launch.</span></span> <span data-ttu-id="7722e-184">Może być konieczne ponowne załadowanie strony sieci Web (lub wybranie opcji **Kontynuuj** w debugerze), jeśli strona zostanie załadowana przed uruchomieniem serwera.</span><span class="sxs-lookup"><span data-stu-id="7722e-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="7722e-185">W przeglądarce Podaj nazwę użytkownika po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="7722e-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="7722e-186">Skopiuj adres URL strony na inną kartę lub okno przeglądarki i podaj inną nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7722e-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="7722e-187">Będzie można wysyłać wiadomości z jednego okienka przeglądarki do drugiego, jak w samouczku Wprowadzenie.</span><span class="sxs-lookup"><span data-stu-id="7722e-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
