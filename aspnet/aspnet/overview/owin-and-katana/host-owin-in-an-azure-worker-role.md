---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: OWIN hosta w roli procesu roboczego platformy Azure | Microsoft Docs
author: MikeWasson
description: W tym samouczku przedstawiono sposób samodzielnego hostowania OWIN w roli procesu roboczego Microsoft Azure. Otwórz interfejs sieci Web dla platformy .NET (OWIN) definiuje abstrakcję między serwerem sieci Web platformy .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 59d2e0d549427093f8a2424b17af81169b78ef30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584617"
---
# <a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="b2c50-104">Hostowanie interfejsu OWIN w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="b2c50-104">Host OWIN in an Azure Worker Role</span></span>

<span data-ttu-id="b2c50-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b2c50-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="b2c50-106">W tym samouczku przedstawiono sposób samodzielnego hostowania OWIN w roli procesu roboczego Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c50-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
>
> <span data-ttu-id="b2c50-107">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (Owin) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET.</span><span class="sxs-lookup"><span data-stu-id="b2c50-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="b2c50-108">OWIN oddziela aplikację sieci Web od serwera, co sprawia, że OWIN jest idealnym rozwiązaniem do samodzielnego udostępniania aplikacji sieci Web we własnym procesie poza usługami IIS — na przykład w ramach roli proces roboczy platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c50-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="b2c50-109">W tym samouczku dowiesz się, jak samoobsługowo OWIN aplikacje w ramach roli procesu roboczego Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c50-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="b2c50-110">Aby dowiedzieć się więcej o rolach procesów roboczych, zobacz [modele wykonywania platformy Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="b2c50-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b2c50-111">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="b2c50-111">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="b2c50-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b2c50-112">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [<span data-ttu-id="b2c50-113">Zestaw Azure SDK dla programu .NET 2,3</span><span class="sxs-lookup"><span data-stu-id="b2c50-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="b2c50-114">Microsoft. Owin. SelfHost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="b2c50-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="b2c50-115">Tworzenie projektu Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="b2c50-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="b2c50-116">Uruchom program Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="b2c50-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="b2c50-117">Uprawnienia administratora są konieczne do lokalnego debugowania aplikacji przy użyciu emulatora obliczeń platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c50-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="b2c50-118">W menu **plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="b2c50-119">Z **zainstalowanych szablonów**, w obszarze C#Wizualizacja, kliknij pozycję **chmura** , a następnie kliknij pozycję **Usługa w chmurze platformy Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="b2c50-120">Nadaj projektowi nazwę "plik azureapp" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="b2c50-121">W **nowej usłudze w chmurze platformy Microsoft Azure** okno dialogowe, kliknij dwukrotnie **rolę proces roboczy**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="b2c50-122">Pozostaw nazwę domyślną ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="b2c50-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="b2c50-123">Ten krok powoduje dodanie roli procesu roboczego do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="b2c50-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="b2c50-124">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="b2c50-125">Utworzone rozwiązanie programu Visual Studio zawiera dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="b2c50-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="b2c50-126">&quot;plik azureapp&quot; definiuje role i konfigurację dla aplikacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c50-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="b2c50-127">&quot;WorkerRole1&quot; zawiera kod dla roli proces roboczy.</span><span class="sxs-lookup"><span data-stu-id="b2c50-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="b2c50-128">Ogólnie rzecz biorąc, aplikacja platformy Azure może zawierać wiele ról, chociaż w tym samouczku zostanie użyta jedna rola.</span><span class="sxs-lookup"><span data-stu-id="b2c50-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="b2c50-129">Dodawanie pakietów samoobsługi OWIN</span><span class="sxs-lookup"><span data-stu-id="b2c50-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="b2c50-130">W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-130">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="b2c50-131">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b2c50-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="b2c50-132">Dodawanie punktu końcowego HTTP</span><span class="sxs-lookup"><span data-stu-id="b2c50-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="b2c50-133">W Eksplorator rozwiązań rozwiń projekt plik azureapp.</span><span class="sxs-lookup"><span data-stu-id="b2c50-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="b2c50-134">Rozwiń węzeł role, kliknij prawym przyciskiem myszy pozycję WorkerRole1, a następnie wybierz pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="b2c50-135">Kliknij pozycję **punkty końcowe**, a następnie kliknij pozycję **Dodaj punkt końcowy**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="b2c50-136">Z listy rozwijanej **Protokół** wybierz pozycję "http".</span><span class="sxs-lookup"><span data-stu-id="b2c50-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="b2c50-137">W polu **port publiczny** i **port prywatny**wpisz 80.</span><span class="sxs-lookup"><span data-stu-id="b2c50-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="b2c50-138">Te numery portów mogą być różne.</span><span class="sxs-lookup"><span data-stu-id="b2c50-138">These port numbers can be different.</span></span> <span data-ttu-id="b2c50-139">Port publiczny jest używany przez klientów podczas wysyłania żądania do roli.</span><span class="sxs-lookup"><span data-stu-id="b2c50-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="b2c50-140">Utwórz klasę uruchomieniową OWIN</span><span class="sxs-lookup"><span data-stu-id="b2c50-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="b2c50-141">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz polecenie **Dodaj** **klasę** / , aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="b2c50-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="b2c50-142">Nadaj klasie nazwę `Startup`.</span><span class="sxs-lookup"><span data-stu-id="b2c50-142">Name the class `Startup`.</span></span>

<span data-ttu-id="b2c50-143">Zastąp cały kod standardowy następującymi:</span><span class="sxs-lookup"><span data-stu-id="b2c50-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="b2c50-144">Metoda rozszerzenia `UseWelcomePage` dodaje prostą stronę HTML do aplikacji, aby sprawdzić, czy witryna działa.</span><span class="sxs-lookup"><span data-stu-id="b2c50-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="b2c50-145">Uruchom hosta OWIN</span><span class="sxs-lookup"><span data-stu-id="b2c50-145">Start the OWIN Host</span></span>

<span data-ttu-id="b2c50-146">Otwórz plik WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="b2c50-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="b2c50-147">Ta klasa definiuje kod, który jest uruchamiany po uruchomieniu i zatrzymaniu roli procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="b2c50-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="b2c50-148">Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="b2c50-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="b2c50-149">Dodaj element członkowski **IDisposable** do klasy `WorkerRole`:</span><span class="sxs-lookup"><span data-stu-id="b2c50-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="b2c50-150">W metodzie `OnStart` Dodaj następujący kod, aby uruchomić hosta:</span><span class="sxs-lookup"><span data-stu-id="b2c50-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="b2c50-151">Metoda **WEBAPP. Start** uruchamia hosta Owin.</span><span class="sxs-lookup"><span data-stu-id="b2c50-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="b2c50-152">Nazwa klasy `Startup` jest parametrem typu dla metody.</span><span class="sxs-lookup"><span data-stu-id="b2c50-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="b2c50-153">Zgodnie z Konwencją, Host wywoła metodę `Configure` tej klasy.</span><span class="sxs-lookup"><span data-stu-id="b2c50-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="b2c50-154">Przesłoń `OnStop`, aby usunąć wystąpienie *aplikacji\_* :</span><span class="sxs-lookup"><span data-stu-id="b2c50-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="b2c50-155">Oto kompletny kod dla WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="b2c50-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="b2c50-156">Skompiluj rozwiązanie i naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeń platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c50-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="b2c50-157">W zależności od ustawień zapory może być konieczne zezwolenie emulatora przez zaporę.</span><span class="sxs-lookup"><span data-stu-id="b2c50-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="b2c50-158">Emulator obliczeń przypisuje lokalny adres IP do punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="b2c50-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="b2c50-159">Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń.</span><span class="sxs-lookup"><span data-stu-id="b2c50-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="b2c50-160">Kliknij prawym przyciskiem myszy ikonę emulatora w obszarze powiadomień paska zadań, a następnie wybierz pozycję **Pokaż interfejs użytkownika emulatora obliczeń**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="b2c50-161">Znajdź adres IP w obszarze wdrożenia usługi, wdrożenie [ID], Szczegóły usługi.</span><span class="sxs-lookup"><span data-stu-id="b2c50-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="b2c50-162">Otwórz przeglądarkę internetową i przejdź do adresu http:\/\/*Address*, gdzie *Address* to adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="b2c50-162">Open a web browser and navigate to http:\/\/*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="b2c50-163">Powinna zostać wyświetlona strona powitalna OWIN:</span><span class="sxs-lookup"><span data-stu-id="b2c50-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="b2c50-164">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="b2c50-164">Deploy to Azure</span></span>

<span data-ttu-id="b2c50-165">W tym kroku musisz mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c50-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="b2c50-166">Jeśli jeszcze tego nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut.</span><span class="sxs-lookup"><span data-stu-id="b2c50-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b2c50-167">Aby uzyskać szczegółowe informacje, zobacz [Microsoft Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="b2c50-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="b2c50-168">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt plik azureapp.</span><span class="sxs-lookup"><span data-stu-id="b2c50-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="b2c50-169">Wybierz pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="b2c50-170">Jeśli nie zalogowano się na koncie platformy Azure, kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="b2c50-171">Po zalogowaniu wybierz subskrypcję i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="b2c50-172">Wprowadź nazwę usługi w chmurze i wybierz region.</span><span class="sxs-lookup"><span data-stu-id="b2c50-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="b2c50-173">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="b2c50-174">Kliknij przycisk **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="b2c50-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="b2c50-175">W oknie Dziennik aktywności platformy Azure zostanie wyświetlony postęp wdrażania.</span><span class="sxs-lookup"><span data-stu-id="b2c50-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="b2c50-176">Po wdrożeniu aplikacji przejdź do `http://appname.cloudapp.net/`, gdzie *nazwa_aplikacji* to nazwa usługi w chmurze.</span><span class="sxs-lookup"><span data-stu-id="b2c50-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2c50-177">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="b2c50-177">Additional Resources</span></span>

- [<span data-ttu-id="b2c50-178">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="b2c50-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="b2c50-179">Projekt Katana w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="b2c50-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
