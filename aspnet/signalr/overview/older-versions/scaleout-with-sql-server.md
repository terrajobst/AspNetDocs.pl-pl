---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Skalowania sygnalizujący z SQL Server (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 8674b06a2a751c9a7b1c6c9b19782974d0602a62
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536471"
---
# <a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="1ff1d-102">SignalR — skalowanie w poziomie z użyciem programu SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="1ff1d-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>

<span data-ttu-id="1ff1d-103">według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1ff1d-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="1ff1d-104">W tym samouczku będziesz używać SQL Server do dystrybuowania komunikatów w aplikacji sygnalizującej wdrożonej w dwóch oddzielnych wystąpieniach usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="1ff1d-105">Możesz również uruchomić ten samouczek na pojedynczym komputerze testowym, ale aby uzyskać pełen efekt, należy wdrożyć aplikację sygnalizującą na co najmniej dwóch serwerach.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="1ff1d-106">Należy również zainstalować SQL Server na jednym z serwerów lub na osobnym dedykowanym serwerze.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="1ff1d-107">Innym rozwiązaniem jest uruchomienie samouczka przy użyciu maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="1ff1d-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1ff1d-108">Prerequisites</span></span>

<span data-ttu-id="1ff1d-109">Microsoft SQL Server 2005 lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="1ff1d-110">Plan dla programu jest obsługiwany zarówno przez program, jak i wersje systemu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="1ff1d-111">Nie obsługuje wersji SQL Server Compact ani Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="1ff1d-112">(Jeśli aplikacja jest hostowana na platformie Azure, rozważ użycie zamiast niej planu Service Bus.)</span><span class="sxs-lookup"><span data-stu-id="1ff1d-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="1ff1d-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="1ff1d-113">Overview</span></span>

<span data-ttu-id="1ff1d-114">Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="1ff1d-115">Utwórz nową pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-115">Create a new empty database.</span></span> <span data-ttu-id="1ff1d-116">Plan nie utworzy niezbędnych tabel w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="1ff1d-117">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1ff1d-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="1ff1d-118">Microsoft. AspNet. Signal</span><span class="sxs-lookup"><span data-stu-id="1ff1d-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="1ff1d-119">Microsoft. AspNet. Signaler. SqlServer</span><span class="sxs-lookup"><span data-stu-id="1ff1d-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="1ff1d-120">Tworzenie aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="1ff1d-121">Dodaj następujący kod do szablonu Global. asax, aby skonfigurować plan:</span><span class="sxs-lookup"><span data-stu-id="1ff1d-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="1ff1d-122">Konfigurowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="1ff1d-122">Configure the Database</span></span>

<span data-ttu-id="1ff1d-123">Zdecyduj, czy aplikacja będzie używać uwierzytelniania systemu Windows, czy SQL Server uwierzytelniania w celu uzyskania dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="1ff1d-124">Bez względu na to upewnij się, że użytkownik bazy danych ma uprawnienia do logowania, tworzenia schematów i tworzenia tabel.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="1ff1d-125">Utwórz nową bazę danych do użycia w celu utworzenia planu.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="1ff1d-126">Można nadać bazie danych dowolną nazwę.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-126">You can give the database any name.</span></span> <span data-ttu-id="1ff1d-127">Nie musisz tworzyć żadnych tabel w bazie danych; plan nie utworzy niezbędnych tabel.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="1ff1d-128">Włącz Service Broker</span><span class="sxs-lookup"><span data-stu-id="1ff1d-128">Enable Service Broker</span></span>

<span data-ttu-id="1ff1d-129">Zaleca się włączenie Service Broker dla bazy danych programu planowanie.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="1ff1d-130">Service Broker zapewnia natywną obsługę komunikatów i kolejkowania w SQL Server, co pozwala na efektywniejsze otrzymywanie aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="1ff1d-131">(Jednak plan nie działa również bez Service Broker).</span><span class="sxs-lookup"><span data-stu-id="1ff1d-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="1ff1d-132">Aby sprawdzić, czy Service Broker jest włączona, zbadaj kolumnę **is\_Broker\_Enabled** w widoku wykazu **sys. databases** .</span><span class="sxs-lookup"><span data-stu-id="1ff1d-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="1ff1d-133">Aby włączyć Service Broker, użyj następującego zapytania SQL:</span><span class="sxs-lookup"><span data-stu-id="1ff1d-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="1ff1d-134">Jeśli ta kwerenda wydaje się być zakleszczeniem, upewnij się, że żadne aplikacje nie są połączone z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="1ff1d-135">Jeśli włączono śledzenie, ślady będą również wskazywać, czy Service Broker jest włączona.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="1ff1d-136">Tworzenie aplikacji sygnalizującej</span><span class="sxs-lookup"><span data-stu-id="1ff1d-136">Create a SignalR Application</span></span>

<span data-ttu-id="1ff1d-137">Utwórz aplikację sygnalizującą, wykonując jeden z następujących samouczków:</span><span class="sxs-lookup"><span data-stu-id="1ff1d-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="1ff1d-138">Wprowadzenie z sygnałem</span><span class="sxs-lookup"><span data-stu-id="1ff1d-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="1ff1d-139">Wprowadzenie z sygnalizacyjnym i MVC 4</span><span class="sxs-lookup"><span data-stu-id="1ff1d-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="1ff1d-140">Następnie zmodyfikujemy aplikację czatu, aby obsługiwała skalowania z SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="1ff1d-141">Najpierw Dodaj pakiet NuGet programu Signaler do projektu.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="1ff1d-142">W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1ff1d-143">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1ff1d-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="1ff1d-144">Następnie otwórz plik Global. asax.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="1ff1d-145">Dodaj następujący kod do **aplikacji\_Start** :</span><span class="sxs-lookup"><span data-stu-id="1ff1d-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="1ff1d-146">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1ff1d-146">Deploy and Run the Application</span></span>

<span data-ttu-id="1ff1d-147">Przygotuj wystąpienia systemu Windows Server, aby wdrożyć aplikację sygnalizującą.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="1ff1d-148">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-148">Add the IIS role.</span></span> <span data-ttu-id="1ff1d-149">Uwzględnij funkcje "projektowanie aplikacji", w tym protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="1ff1d-150">Dołącz także usługę zarządzania (wymienioną w obszarze "narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="1ff1d-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="1ff1d-151">**Zainstaluj Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="1ff1d-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="1ff1d-152">Po uruchomieniu Menedżera usług IIS zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub [pobranie instalatora](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="1ff1d-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="1ff1d-153">W Instalatorze platformy Wyszukaj Web Deploy i zainstaluj Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="1ff1d-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="1ff1d-154">Sprawdź, czy usługa zarządzania siecią Web jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="1ff1d-155">Jeśli nie, uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-155">If not, start the service.</span></span> <span data-ttu-id="1ff1d-156">(Jeśli nie widzisz usługi zarządzania siecią Web na liście usług systemu Windows, upewnij się, że usługa zarządzania została zainstalowana po dodaniu roli usług IIS.)</span><span class="sxs-lookup"><span data-stu-id="1ff1d-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="1ff1d-157">Na koniec Otwórz port 8172 dla protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="1ff1d-158">Jest to port, którego używa narzędzie Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="1ff1d-159">Teraz możesz przystąpić do wdrażania projektu programu Visual Studio z komputera deweloperskiego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="1ff1d-160">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="1ff1d-161">Aby uzyskać bardziej szczegółową dokumentację dotyczącą wdrażania w sieci Web, zobacz [Web Deployment Content Map for Visual Studio i ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="1ff1d-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="1ff1d-162">Jeśli aplikacja zostanie wdrożona na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobaczyć, że każdy z nich odbiera komunikaty sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="1ff1d-163">(Oczywiście w środowisku produkcyjnym dwa serwery byłyby za modułem równoważenia obciążenia).</span><span class="sxs-lookup"><span data-stu-id="1ff1d-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="1ff1d-164">Po uruchomieniu aplikacji można zobaczyć, że sygnalizujący automatycznie utworzy tabele w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="1ff1d-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="1ff1d-165">Program sygnalizujący zarządza tabelami.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-165">SignalR manages the tables.</span></span> <span data-ttu-id="1ff1d-166">Dopóki aplikacja jest wdrożona, nie usuwaj wierszy, Modyfikuj tabelę i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="1ff1d-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
