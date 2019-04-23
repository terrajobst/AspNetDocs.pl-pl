---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hostowanie wzorca ASP.NET Web API 2 w roli procesu roboczego platformy Azure — ASP.NET 4.x
author: MikeWasson
description: 'Samouczek: Host sieci Web interfejsu API platformy ASP.NET w roli procesu roboczego platformy Azure, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: bfb23aafb814010e8651965dad91ca20a37fd786
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59404627"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="bbd0c-103">Hostowanie wzorca ASP.NET Web API 2 w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="bbd0c-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="bbd0c-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bbd0c-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bbd0c-105">Ten samouczek przedstawia hostowanie interfejsu API sieci Web platformy ASP.NET w roli procesu roboczego platformy Azure, za pomocą OWIN na potrzeby samodzielnego hostowania strukturę interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="bbd0c-106">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bbd0c-107">OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza programem IIS — na przykład w roli procesu roboczego platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="bbd0c-108">W tym samouczku użyjesz pakietu Microsoft.Owin.Host.HttpListener udostępniającej serwer HTTP używane na potrzeby samodzielnego hostowania aplikacji OWIN.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bbd0c-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="bbd0c-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="bbd0c-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bbd0c-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="bbd0c-111">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="bbd0c-111">Web API 2</span></span>
> - [<span data-ttu-id="bbd0c-112">Zestaw Azure SDK dla platformy .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="bbd0c-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="bbd0c-113">Tworzenie projektu platformy Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bbd0c-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="bbd0c-114">Uruchom program Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="bbd0c-115">Aby debugować aplikację lokalnie, za pomocą emulatora obliczeń platformy Azure potrzebne są uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="bbd0c-116">Na **pliku** menu, kliknij przycisk **New**, następnie kliknij przycisk **projektu**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="bbd0c-117">Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **chmury** a następnie kliknij przycisk **usłudze Windows Azure Cloud Services**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="bbd0c-118">Nadaj projektowi nazwę "AzureApp", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="bbd0c-119">W **nową usługę w chmurze Azure Windows** okno dialogowe, kliknij dwukrotnie **roli procesu roboczego**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="bbd0c-120">Pozostaw nazwę domyślną ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="bbd0c-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="bbd0c-121">Ten krok powoduje dodanie roli procesu roboczego do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="bbd0c-122">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="bbd0c-123">Rozwiązanie programu Visual Studio, który jest tworzony zawiera dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="bbd0c-124">&quot;AzureApp&quot; definiuje ról i konfiguracji dla aplikacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="bbd0c-125">&quot;WorkerRole1&quot; zawiera kod dla roli procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="bbd0c-126">Ogólnie rzecz biorąc aplikacją platformy Azure może zawierać wiele ról, chociaż w tym samouczku korzysta z jedną rolę.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="bbd0c-127">Dodawanie interfejsu API sieci Web i pakietów OWIN</span><span class="sxs-lookup"><span data-stu-id="bbd0c-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="bbd0c-128">Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet**, następnie kliknij przycisk **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="bbd0c-129">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="bbd0c-130">Dodaj punkt końcowy HTTP</span><span class="sxs-lookup"><span data-stu-id="bbd0c-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="bbd0c-131">W Eksploratorze rozwiązań rozwiń projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="bbd0c-132">Rozwiń węzeł ról, kliknij prawym przyciskiem myszy WorkerRole1, a następnie wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="bbd0c-133">Kliknij przycisk **punktów końcowych**, a następnie kliknij przycisk **Dodaj punkt końcowy**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="bbd0c-134">W **protokołu** listy rozwijanej wybierz opcję "http".</span><span class="sxs-lookup"><span data-stu-id="bbd0c-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="bbd0c-135">W **Port publiczny** i **Port prywatny**, wpisz 80.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="bbd0c-136">Te numery portów mogą być różne.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-136">These port numbers can be different.</span></span> <span data-ttu-id="bbd0c-137">Port publiczny jest o tym, co klienci używają wysłanie żądania do roli.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="bbd0c-138">Konfigurowanie internetowego interfejsu API dla hosta samodzielnego</span><span class="sxs-lookup"><span data-stu-id="bbd0c-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="bbd0c-139">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1, a następnie wybierz **Dodaj** / **klasy** Aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="bbd0c-140">Nazwa klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="bbd0c-141">Zastąp cały kod standardowy, w tym pliku następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="bbd0c-142">Dodaj Kontroler interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="bbd0c-142">Add a Web API Controller</span></span>

<span data-ttu-id="bbd0c-143">Następnie Dodaj klasę kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="bbd0c-144">Kliknij prawym przyciskiem myszy projekt WorkerRole1, a następnie wybierz pozycję **Dodaj** / **klasy**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="bbd0c-145">Nazwa klasy kontrolera testów.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-145">Name the class TestController.</span></span> <span data-ttu-id="bbd0c-146">Zastąp cały kod standardowy, w tym pliku następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="bbd0c-147">Dla uproszczenia tego kontrolera po prostu definiuje dwie metody GET, które zwracają zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="bbd0c-148">Uruchom hosta OWIN</span><span class="sxs-lookup"><span data-stu-id="bbd0c-148">Start the OWIN Host</span></span>

<span data-ttu-id="bbd0c-149">Otwórz plik WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="bbd0c-150">Ta klasa definiuje kod, który jest uruchamiany podczas uruchamiania i zatrzymywania roli procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="bbd0c-151">Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="bbd0c-152">Dodaj **interfejsu IDisposable** członka do `WorkerRole` klasy:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="bbd0c-153">W `OnStart` metody, Dodaj następujący kod, aby uruchomić hosta:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="bbd0c-154">**WebApp.Start** metoda uruchamia hosta OWIN.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="bbd0c-155">Nazwa `Startup` klasy jest parametrem typu w metodzie.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="bbd0c-156">Zgodnie z Konwencją, host będzie wywoływać `Configure` metody tej klasy.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="bbd0c-157">Zastąp `OnStop` do rozporządzania  *\_aplikacji* wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="bbd0c-158">Oto kompletny kod dla WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="bbd0c-159">Skompiluj rozwiązanie, a następnie naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeń systemu Azure.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="bbd0c-160">W zależności od ustawień zapory konieczne może być Zezwalaj na emulator przez zaporę.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="bbd0c-161">Jeśli wystąpi wyjątek, podobnie do poniższego, zobacz [ten wpis w blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) obejście tego problemu.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="bbd0c-162">"Nie można załadować pliku lub zestawu ' Microsoft.Owin, wersja = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" lub jednej z jego zależności.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="bbd0c-163">Definicja manifestu zestawu znajduje się niezgodna odwołanie do zestawu.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="bbd0c-164">(Wyjątek od HRESULT: 0x80131040)"</span><span class="sxs-lookup"><span data-stu-id="bbd0c-164">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="bbd0c-165">Emulator obliczeń przypisuje lokalny adres IP punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="bbd0c-166">Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="bbd0c-167">Kliknij prawym przyciskiem myszy ikonę emulatora w zadaniu paska w obszarze powiadomień, a następnie wybierz pozycję **Pokaż interfejs użytkownika emulatora obliczeń**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="bbd0c-168">Znajdowanie adresu IP, w ramach wdrożenia usług, wdrożenia [identyfikator], szczegóły usługi.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="bbd0c-169">Otwórz przeglądarkę internetową i przejdź do http://<em>adres</em>/test/1, gdzie <em>adres</em> jest adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="bbd0c-170">Powinny pojawić się odpowiedzi z kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="bbd0c-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="bbd0c-171">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="bbd0c-171">Deploy to Azure</span></span>

<span data-ttu-id="bbd0c-172">W tym kroku musisz mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="bbd0c-173">Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bbd0c-174">Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="bbd0c-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="bbd0c-175">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="bbd0c-176">Wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="bbd0c-177">Jeśli nie jest zalogowany do konta platformy Azure, kliknij przycisk **Sign In**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="bbd0c-178">Po użytkownik jest zalogowany, wybierz subskrypcję i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="bbd0c-179">Wprowadź nazwę usługi w chmurze, a następnie wybierz region.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="bbd0c-180">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="bbd0c-181">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="bbd0c-182">Okno Dziennik aktywności platformy Azure Pokazuje postęp wdrażania.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="bbd0c-183">Gdy aplikacja jest wdrożona, przejdź do http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="bbd0c-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="bbd0c-184">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bbd0c-184">Additional Resources</span></span>

- [<span data-ttu-id="bbd0c-185">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="bbd0c-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="bbd0c-186">Projektu Katana w usłudze GitHub</span><span class="sxs-lookup"><span data-stu-id="bbd0c-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
