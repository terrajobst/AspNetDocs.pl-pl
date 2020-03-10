---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hostowanie ASP.NET Web API 2 w roli proces roboczy platformy Azure — ASP.NET 4. x
author: MikeWasson
description: 'Samouczek: hostowanie interfejsu API sieci Web ASP.NET w roli procesu roboczego platformy Azure przy użyciu OWIN do samoobsługowego udostępniania struktury internetowego interfejsu API.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556631"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="8e74d-103">Hostowanie ASP.NET Web API 2 w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="8e74d-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="8e74d-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8e74d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8e74d-105">W tym samouczku pokazano, jak hostować interfejs API sieci Web ASP.NET w roli procesu roboczego platformy Azure przy użyciu OWIN do samoobsługowego udostępniania struktury internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="8e74d-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="8e74d-106">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (Owin) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET.</span><span class="sxs-lookup"><span data-stu-id="8e74d-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="8e74d-107">OWIN oddziela aplikację sieci Web od serwera, co sprawia, że OWIN jest idealnym rozwiązaniem do samodzielnego udostępniania aplikacji sieci Web we własnym procesie poza usługami IIS — na przykład w ramach roli proces roboczy platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="8e74d-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="8e74d-108">W tym samouczku zostanie użyty pakiet Microsoft. Owin. host. odbiornika HttpListener, który udostępnia serwer HTTP, który będzie używany do samoobsługowego hostowania aplikacji OWIN.</span><span class="sxs-lookup"><span data-stu-id="8e74d-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8e74d-109">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="8e74d-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="8e74d-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8e74d-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="8e74d-111">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="8e74d-111">Web API 2</span></span>
> - [<span data-ttu-id="8e74d-112">Zestaw Azure SDK dla programu .NET 2,3</span><span class="sxs-lookup"><span data-stu-id="8e74d-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="8e74d-113">Tworzenie projektu Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8e74d-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="8e74d-114">Uruchom program Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="8e74d-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="8e74d-115">Uprawnienia administratora są konieczne do lokalnego debugowania aplikacji przy użyciu emulatora obliczeń platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="8e74d-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="8e74d-116">W menu **plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="8e74d-117">Z **zainstalowanych szablonów**, w obszarze C#Wizualizacja, kliknij pozycję **chmura** , a następnie kliknij pozycję **Usługa w chmurze platformy Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="8e74d-118">Nadaj projektowi nazwę "plik azureapp" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="8e74d-119">W **nowej usłudze w chmurze platformy Microsoft Azure** okno dialogowe, kliknij dwukrotnie **rolę proces roboczy**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="8e74d-120">Pozostaw nazwę domyślną ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="8e74d-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="8e74d-121">Ten krok powoduje dodanie roli procesu roboczego do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="8e74d-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="8e74d-122">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="8e74d-123">Utworzone rozwiązanie programu Visual Studio zawiera dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="8e74d-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="8e74d-124">&quot;plik azureapp&quot; definiuje role i konfigurację dla aplikacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="8e74d-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="8e74d-125">&quot;WorkerRole1&quot; zawiera kod dla roli proces roboczy.</span><span class="sxs-lookup"><span data-stu-id="8e74d-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="8e74d-126">Ogólnie rzecz biorąc, aplikacja platformy Azure może zawierać wiele ról, chociaż w tym samouczku zostanie użyta jedna rola.</span><span class="sxs-lookup"><span data-stu-id="8e74d-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="8e74d-127">Dodawanie interfejsu API sieci Web i pakietów OWIN</span><span class="sxs-lookup"><span data-stu-id="8e74d-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="8e74d-128">W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="8e74d-129">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8e74d-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="8e74d-130">Dodawanie punktu końcowego HTTP</span><span class="sxs-lookup"><span data-stu-id="8e74d-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="8e74d-131">W Eksplorator rozwiązań rozwiń projekt plik azureapp.</span><span class="sxs-lookup"><span data-stu-id="8e74d-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="8e74d-132">Rozwiń węzeł role, kliknij prawym przyciskiem myszy pozycję WorkerRole1, a następnie wybierz pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="8e74d-133">Kliknij pozycję **punkty końcowe**, a następnie kliknij pozycję **Dodaj punkt końcowy**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="8e74d-134">Z listy rozwijanej **Protokół** wybierz pozycję "http".</span><span class="sxs-lookup"><span data-stu-id="8e74d-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="8e74d-135">W polu **port publiczny** i **port prywatny**wpisz 80.</span><span class="sxs-lookup"><span data-stu-id="8e74d-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="8e74d-136">Te numery portów mogą być różne.</span><span class="sxs-lookup"><span data-stu-id="8e74d-136">These port numbers can be different.</span></span> <span data-ttu-id="8e74d-137">Port publiczny jest używany przez klientów podczas wysyłania żądania do roli.</span><span class="sxs-lookup"><span data-stu-id="8e74d-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="8e74d-138">Konfigurowanie interfejsu API sieci Web dla samego hosta</span><span class="sxs-lookup"><span data-stu-id="8e74d-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="8e74d-139">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz polecenie **Dodaj** **klasę** / , aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="8e74d-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="8e74d-140">Nadaj klasie nazwę `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8e74d-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="8e74d-141">Zastąp cały kod standardowy w tym pliku następującym:</span><span class="sxs-lookup"><span data-stu-id="8e74d-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="8e74d-142">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="8e74d-142">Add a Web API Controller</span></span>

<span data-ttu-id="8e74d-143">Następnie Dodaj klasę kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8e74d-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="8e74d-144">Kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz polecenie **Dodaj** **klasę** / .</span><span class="sxs-lookup"><span data-stu-id="8e74d-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="8e74d-145">Nadaj klasie nazwę TestController.</span><span class="sxs-lookup"><span data-stu-id="8e74d-145">Name the class TestController.</span></span> <span data-ttu-id="8e74d-146">Zastąp cały kod standardowy w tym pliku następującym:</span><span class="sxs-lookup"><span data-stu-id="8e74d-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="8e74d-147">Dla uproszczenia ten kontroler tylko definiuje dwie metody GET, które zwracają zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="8e74d-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="8e74d-148">Uruchom hosta OWIN</span><span class="sxs-lookup"><span data-stu-id="8e74d-148">Start the OWIN Host</span></span>

<span data-ttu-id="8e74d-149">Otwórz plik WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="8e74d-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="8e74d-150">Ta klasa definiuje kod, który jest uruchamiany po uruchomieniu i zatrzymaniu roli procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="8e74d-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="8e74d-151">Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="8e74d-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="8e74d-152">Dodaj element członkowski **IDisposable** do klasy `WorkerRole`:</span><span class="sxs-lookup"><span data-stu-id="8e74d-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="8e74d-153">W metodzie `OnStart` Dodaj następujący kod, aby uruchomić hosta:</span><span class="sxs-lookup"><span data-stu-id="8e74d-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="8e74d-154">Metoda **WEBAPP. Start** uruchamia hosta Owin.</span><span class="sxs-lookup"><span data-stu-id="8e74d-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="8e74d-155">Nazwa klasy `Startup` jest parametrem typu dla metody.</span><span class="sxs-lookup"><span data-stu-id="8e74d-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="8e74d-156">Zgodnie z Konwencją, Host wywoła metodę `Configure` tej klasy.</span><span class="sxs-lookup"><span data-stu-id="8e74d-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="8e74d-157">Przesłoń `OnStop`, aby usunąć wystąpienie *aplikacji\_* :</span><span class="sxs-lookup"><span data-stu-id="8e74d-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="8e74d-158">Oto kompletny kod dla WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="8e74d-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="8e74d-159">Skompiluj rozwiązanie i naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeń platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="8e74d-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="8e74d-160">W zależności od ustawień zapory może być konieczne zezwolenie emulatora przez zaporę.</span><span class="sxs-lookup"><span data-stu-id="8e74d-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="8e74d-161">Jeśli wystąpi wyjątek podobny do poniższego, zapoznaj się z [tym wpisem w blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) , aby zapoznać się z obejściem.</span><span class="sxs-lookup"><span data-stu-id="8e74d-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="8e74d-162">"Nie można załadować pliku lub zestawu" Microsoft. Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 "lub jednej z jego zależności.</span><span class="sxs-lookup"><span data-stu-id="8e74d-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="8e74d-163">Zlokalizowana definicja manifestu zestawu nie odpowiada odwołaniu do zestawu.</span><span class="sxs-lookup"><span data-stu-id="8e74d-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="8e74d-164">(Wyjątek od HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="8e74d-164">(Exception from HRESULT: 0x80131040)"</span></span>

<span data-ttu-id="8e74d-165">Emulator obliczeń przypisuje lokalny adres IP do punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="8e74d-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="8e74d-166">Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń.</span><span class="sxs-lookup"><span data-stu-id="8e74d-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="8e74d-167">Kliknij prawym przyciskiem myszy ikonę emulatora w obszarze powiadomień paska zadań, a następnie wybierz pozycję **Pokaż interfejs użytkownika emulatora obliczeń**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="8e74d-168">Znajdź adres IP w obszarze wdrożenia usługi, wdrożenie [ID], Szczegóły usługi.</span><span class="sxs-lookup"><span data-stu-id="8e74d-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="8e74d-169">Otwórz przeglądarkę internetową i przejdź do<em>adresu</em>http:///test/1, gdzie <em>Address</em> jest adresem IP przypisanym przez emulator obliczeń; na przykład `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="8e74d-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="8e74d-170">Powinna zostać wyświetlona odpowiedź z kontrolera internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8e74d-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="8e74d-171">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="8e74d-171">Deploy to Azure</span></span>

<span data-ttu-id="8e74d-172">W tym kroku musisz mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="8e74d-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="8e74d-173">Jeśli jeszcze tego nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut.</span><span class="sxs-lookup"><span data-stu-id="8e74d-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="8e74d-174">Aby uzyskać szczegółowe informacje, zobacz [Microsoft Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="8e74d-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="8e74d-175">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt plik azureapp.</span><span class="sxs-lookup"><span data-stu-id="8e74d-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="8e74d-176">Wybierz pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="8e74d-177">Jeśli nie zalogowano się na koncie platformy Azure, kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="8e74d-178">Po zalogowaniu wybierz subskrypcję i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="8e74d-179">Wprowadź nazwę usługi w chmurze i wybierz region.</span><span class="sxs-lookup"><span data-stu-id="8e74d-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="8e74d-180">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="8e74d-181">Kliknij przycisk **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="8e74d-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="8e74d-182">W oknie Dziennik aktywności platformy Azure zostanie wyświetlony postęp wdrażania.</span><span class="sxs-lookup"><span data-stu-id="8e74d-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="8e74d-183">Po wdrożeniu aplikacji przejdź do http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="8e74d-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="8e74d-184">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="8e74d-184">Additional Resources</span></span>

- [<span data-ttu-id="8e74d-185">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="8e74d-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="8e74d-186">Projekt Katana w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="8e74d-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
