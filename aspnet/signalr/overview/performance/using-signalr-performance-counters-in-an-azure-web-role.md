---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Korzystanie z liczników wydajności sygnalizujących w roli sieci Web platformy Azure | Microsoft Docs
author: guardrex
description: Jak zainstalować liczniki wydajności sygnalizujące i korzystać z nich w roli sieci Web platformy Azure.
keywords: ASP. NET, sygnalizujący, licznik wydajności, rola sieci Web platformy Azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578982"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="6ad1d-104">Korzystanie z liczników wydajności sygnałów w roli sieci Web platformy Azure</span><span class="sxs-lookup"><span data-stu-id="6ad1d-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="6ad1d-105">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6ad1d-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="6ad1d-106">Liczniki wydajności sygnałów są używane do monitorowania wydajności aplikacji w roli sieci Web platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="6ad1d-107">Liczniki są przechwytywane przez Diagnostyka Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="6ad1d-108">Liczniki wydajności dla sygnałów są instalowane na platformie Azure przy użyciu programu *sygnalizującego. exe*, tego samego narzędzia, które jest używane w przypadku aplikacji autonomicznych lub lokalnych.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="6ad1d-109">Ponieważ Role platformy Azure są przejściowe, należy skonfigurować aplikację do instalowania i rejestrowania liczników wydajności sygnałów przy uruchamianiu.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ad1d-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6ad1d-110">Prerequisites</span></span>

* <span data-ttu-id="6ad1d-111">Visual Studio 2015 lub [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="6ad1d-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="6ad1d-112">[Microsoft Azure SDK dla programu Visual Studio](https://azure.microsoft.com/downloads/) **Uwaga: Uruchom ponownie komputer po zainstalowaniu zestawu SDK.**</span><span class="sxs-lookup"><span data-stu-id="6ad1d-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="6ad1d-113">Subskrypcja Microsoft Azure: Aby utworzyć bezpłatne konto wersji próbnej platformy Azure, zobacz [bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6ad1d-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="6ad1d-114">Tworzenie aplikacji roli sieci Web platformy Azure, która uwidacznia liczniki wydajności sygnałów</span><span class="sxs-lookup"><span data-stu-id="6ad1d-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="6ad1d-115">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="6ad1d-116">W programie Visual Studio wybierz pozycje **Plik** > **Nowy** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="6ad1d-117">W oknie dialogowym **Nowy projekt** wybierz kategorię **Visual C#**  > **Cloud** z lewej strony, a następnie wybierz szablon **usługi w chmurze platformy Azure** .</span><span class="sxs-lookup"><span data-stu-id="6ad1d-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="6ad1d-118">Nadaj aplikacji nazwę **SignalRPerfCounters** i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nowa aplikacja w chmurze](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="6ad1d-120">Jeśli nie widzisz kategorii szablonów w **chmurze** lub szablonu **usługi w chmurze Azure** , musisz zainstalować obciążenie Programowanie na **platformie azure** dla programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="6ad1d-121">Wybierz łącze **otwórz Instalator programu Visual Studio** w lewej dolnej części okna dialogowego **Nowy projekt** , aby otworzyć Instalator programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="6ad1d-122">Wybierz obciążenie **Programowanie na platformie Azure** , a następnie wybierz pozycję **Modyfikuj** , aby rozpocząć instalowanie obciążenia.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Obciążenie Programowanie na platformie Azure w Instalator programu Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="6ad1d-124">W oknie dialogowym **nowa Microsoft Azure usługa w chmurze** wybierz pozycję **rola sieci Web ASP.NET** i wybierz przycisk >, aby dodać rolę do projektu.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="6ad1d-125">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-125">Select **OK**.</span></span>

   ![Dodaj rolę sieci Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="6ad1d-127">W oknie dialogowym **Nowa aplikacja sieci Web ASP.NET — WebRole1** wybierz szablon **MVC** , a następnie wybierz przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Dodawanie MVC i interfejsu API sieci Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="6ad1d-129">W **Eksplorator rozwiązań**Otwórz plik *Diagnostics. Wadcfgx* w obszarze **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Eksplorator rozwiązań Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="6ad1d-131">Zastąp zawartość pliku następującą konfiguracją i Zapisz plik:</span><span class="sxs-lookup"><span data-stu-id="6ad1d-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="6ad1d-132">Otwórz **konsolę Menedżera pakietów** z **narzędzi** > **Menedżera pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="6ad1d-133">Wprowadź następujące polecenia, aby zainstalować najnowszą wersję programu sygnalizującego i pakiet narzędzi sygnalizujących:</span><span class="sxs-lookup"><span data-stu-id="6ad1d-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="6ad1d-134">Skonfiguruj aplikację, aby zainstalować liczniki wydajności sygnalizujące w wystąpieniu roli podczas jego uruchamiania lub odtwarzania.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="6ad1d-135">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **WebRole1** i wybierz pozycję **Dodaj** > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="6ad1d-136">Nazwij nowy folder *startowy*.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-136">Name the new folder *Startup*.</span></span>

   ![Dodaj folder startowy](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="6ad1d-138">Skopiuj plik *sygnalizującer. exe* (dodany z pakietem **Microsoft. ASPNET. signaler.** /SignalRPerfCounters/Packages/Microsoft.ASPNET.SignalR.utils.) z folderu projektu \<>\<wersja >/Tools do folderu *Start* utworzonego w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="6ad1d-139">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *startowy* i wybierz polecenie **Dodaj** > **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="6ad1d-140">W wyświetlonym oknie dialogowym wybierz pozycję *sygnalizujący. exe* i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Dodaj program signaler. exe do projektu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="6ad1d-142">Kliknij prawym przyciskiem myszy utworzony folder *startowy* .</span><span class="sxs-lookup"><span data-stu-id="6ad1d-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="6ad1d-143">Wybierz pozycję **dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="6ad1d-144">Wybierz węzeł **Ogólne** , wybierz pozycję **plik tekstowy**i Nadaj nowemu elementowi *SignalRPerfCounterInstall. cmd*.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="6ad1d-145">Ten plik polecenia spowoduje zainstalowanie liczników wydajności sygnalizujących w roli sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Utwórz plik wsadowy instalacji licznika wydajności sygnalizujący](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="6ad1d-147">Gdy program Visual Studio tworzy plik *SignalRPerfCounterInstall. cmd* , zostanie automatycznie otwarty w oknie głównym.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="6ad1d-148">Zastąp zawartość pliku następującym skryptem, a następnie Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="6ad1d-149">Ten skrypt wykonuje program *sygnalizując. exe*, który dodaje liczniki wydajności sygnalizujące do wystąpienia roli.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="6ad1d-150">Wybierz plik *sygnalizującer. exe* w **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="6ad1d-151">We **właściwościach**pliku ustaw opcję **Kopiuj do katalogu wyjściowego** na **zawsze Kopiuj**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Ustaw wartość Kopiuj do katalogu wyjściowego, aby zawsze była kopiowana](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="6ad1d-153">Powtórz poprzedni krok dla pliku *SignalRPerfCounterInstall. cmd* .</span><span class="sxs-lookup"><span data-stu-id="6ad1d-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

16. <span data-ttu-id="6ad1d-154">Kliknij prawym przyciskiem myszy plik *SignalRPerfCounterInstall. cmd* i wybierz polecenie **Otwórz za pomocą**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="6ad1d-155">W wyświetlonym oknie dialogowym wybierz pozycję **Edytor binarny** , a następnie wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Otwórz za pomocą edytora binarnego](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="6ad1d-157">W edytorze binarnym zaznacz wszystkie wiodące bajty w pliku i usuń je.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="6ad1d-158">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-158">Save and close the file.</span></span>

    ![Usuń wiodące bajty](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="6ad1d-160">Otwórz plik *ServiceDefinition. csdef* i Dodaj zadanie uruchamiania, które powoduje wykonanie pliku *SignalrPerfCounterInstall. cmd* podczas uruchamiania usługi:</span><span class="sxs-lookup"><span data-stu-id="6ad1d-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="6ad1d-161">Otwórz `Views/Shared/_Layout.cshtml` i Usuń skrypt pakietu jQuery z końca pliku.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="6ad1d-162">Dodaj klienta JavaScript, który stale wywołuje metodę `increment` na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="6ad1d-163">Otwórz `Views/Home/Index.cshtml` i Zastąp zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6ad1d-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="6ad1d-164">Utwórz nowy folder w projekcie **WebRole1** o nazwie *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="6ad1d-165">Kliknij prawym przyciskiem myszy folder *Hubs* w **Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="6ad1d-166">W oknie dialogowym **Dodaj nowy element** wybierz kategorię moduł **sygnalizujący** > **sieci Web** , a następnie wybierz szablon elementu **Klasa centrum sygnałów (v2)** .</span><span class="sxs-lookup"><span data-stu-id="6ad1d-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="6ad1d-167">Nazwij nowe centrum *MyHub.cs* i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Dodawanie klasy centrum sygnałów do folderu Hubs w oknie dialogowym Dodaj nowy element](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="6ad1d-169">*MyHub.cs* zostanie automatycznie otwarta w oknie głównym.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="6ad1d-170">Zastąp zawartość następującym kodem, a następnie Zapisz i zamknij plik:</span><span class="sxs-lookup"><span data-stu-id="6ad1d-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="6ad1d-171">*[Plik. exe](signalr-connection-density-testing-with-crank.md)* jest narzędziem do testowania gęstości połączenia z bazą kodu sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="6ad1d-172">Ponieważ podpory wymaga połączenia trwałego, należy dodać je do lokacji do użycia podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="6ad1d-173">Dodaj nowy folder do projektu **WebRole1** o nazwie *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="6ad1d-174">Kliknij prawym przyciskiem myszy ten folder i wybierz polecenie **Dodaj** **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="6ad1d-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6ad1d-175">Nazwij nowy plik klasy *MyPersistentConnections.cs* a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="6ad1d-176">Program Visual Studio otworzy plik *MyPersistentConnections.cs* w oknie głównym.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="6ad1d-177">Zastąp zawartość następującym kodem, a następnie Zapisz i zamknij plik:</span><span class="sxs-lookup"><span data-stu-id="6ad1d-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="6ad1d-178">Przy użyciu klasy `Startup` obiekty sygnalizujące są uruchamiane po rozpoczęciu OWIN.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="6ad1d-179">Otwórz lub Utwórz *Startup.cs* i Zastąp zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6ad1d-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="6ad1d-180">W powyższym kodzie atrybut `OwinStartup` oznacza tę klasę do uruchomienia OWIN.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="6ad1d-181">Metoda `Configuration` uruchamia sygnał.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="6ad1d-182">Przetestuj aplikację w Emulator Microsoft Azure, naciskając klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ad1d-183">Jeśli napotkasz **FileLoadException** o **MapSignalR**, Zmień przekierowania powiązań w *pliku Web. config* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="6ad1d-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="6ad1d-184">Odczekaj około jednej minuty.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-184">Wait about one minute.</span></span> <span data-ttu-id="6ad1d-185">Otwórz okno narzędzia Cloud Explorer w programie Visual Studio (**wyświetl** > **Cloud Explorer**) i rozwiń `(Local)/Storage Accounts/(Development)/Tables`ścieżki.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="6ad1d-186">Kliknij dwukrotnie pozycję **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="6ad1d-187">W danych tabeli powinny być widoczne liczniki sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="6ad1d-188">Jeśli nie widzisz tabeli, może być konieczne ponowne wprowadzenie poświadczeń usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="6ad1d-189">Może być konieczne wybranie przycisku **Odśwież** w celu wyświetlenia tabeli w programie **Cloud Explorer** lub wybranie przycisku **Odśwież** w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Wybieranie tabeli liczników wydajności funkcji wad w programie Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Wyświetlanie liczników zebranych w tabeli liczników wydajności funkcji wad](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="6ad1d-192">Aby przetestować aplikację w chmurze, zaktualizuj plik **ServiceConfiguration. Cloud. cscfg** i ustaw `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` na prawidłowe parametry połączenia z kontem usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="6ad1d-193">Wdróż aplikację w subskrypcji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="6ad1d-194">Aby uzyskać szczegółowe informacje na temat sposobu wdrażania aplikacji na platformie Azure, zobacz [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="6ad1d-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="6ad1d-195">Zaczekaj kilka minut.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-195">Wait a few minutes.</span></span> <span data-ttu-id="6ad1d-196">W programie **Cloud Explorer**Znajdź konto magazynu skonfigurowane powyżej i Znajdź w nim tabelę `WADPerformanceCountersTable`.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="6ad1d-197">W danych tabeli powinny być widoczne liczniki sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="6ad1d-198">Jeśli nie widzisz tabeli, może być konieczne ponowne wprowadzenie poświadczeń usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="6ad1d-199">Może być konieczne wybranie przycisku **Odśwież** w celu wyświetlenia tabeli w programie **Cloud Explorer** lub wybranie przycisku **Odśwież** w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="6ad1d-200">Specjalne poświęcenie na [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) dla oryginalnej zawartości używanej w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="6ad1d-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
