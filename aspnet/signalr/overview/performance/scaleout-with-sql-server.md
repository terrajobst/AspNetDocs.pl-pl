---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Skalowania sygnalizujący z SQL Server | Microsoft Docs
author: bradygaster
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizującego w wersji 2 poprzednie wersje tego tematu, aby uzyskać informacje o wcześniejszych wersjach programu...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579185"
---
# <a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="88ca4-103">SignalR — skalowanie w poziomie z użyciem programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="88ca4-103">SignalR Scaleout with SQL Server</span></span>

<span data-ttu-id="88ca4-104">według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="88ca4-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="88ca4-105">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="88ca4-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="88ca4-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="88ca4-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="88ca4-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="88ca4-107">.NET 4.5</span></span>
> - <span data-ttu-id="88ca4-108">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="88ca4-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="88ca4-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="88ca4-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="88ca4-110">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="88ca4-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="88ca4-111">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="88ca4-111">Questions and comments</span></span>
>
> <span data-ttu-id="88ca4-112">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="88ca4-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="88ca4-113">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="88ca4-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="88ca4-114">W tym samouczku będziesz używać SQL Server do dystrybuowania komunikatów w aplikacji sygnalizującej wdrożonej w dwóch oddzielnych wystąpieniach usług IIS.</span><span class="sxs-lookup"><span data-stu-id="88ca4-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="88ca4-115">Możesz również uruchomić ten samouczek na pojedynczym komputerze testowym, ale aby uzyskać pełen efekt, należy wdrożyć aplikację sygnalizującą na co najmniej dwóch serwerach.</span><span class="sxs-lookup"><span data-stu-id="88ca4-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="88ca4-116">Należy również zainstalować SQL Server na jednym z serwerów lub na osobnym dedykowanym serwerze.</span><span class="sxs-lookup"><span data-stu-id="88ca4-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="88ca4-117">Innym rozwiązaniem jest uruchomienie samouczka przy użyciu maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="88ca4-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="88ca4-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="88ca4-118">Prerequisites</span></span>

<span data-ttu-id="88ca4-119">Microsoft SQL Server 2005 lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="88ca4-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="88ca4-120">Plan dla programu jest obsługiwany zarówno przez program, jak i wersje systemu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="88ca4-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="88ca4-121">Nie obsługuje wersji SQL Server Compact ani Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="88ca4-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="88ca4-122">(Jeśli aplikacja jest hostowana na platformie Azure, rozważ użycie zamiast niej planu Service Bus.)</span><span class="sxs-lookup"><span data-stu-id="88ca4-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="88ca4-123">Omówienie</span><span class="sxs-lookup"><span data-stu-id="88ca4-123">Overview</span></span>

<span data-ttu-id="88ca4-124">Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.</span><span class="sxs-lookup"><span data-stu-id="88ca4-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="88ca4-125">Utwórz nową pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="88ca4-125">Create a new empty database.</span></span> <span data-ttu-id="88ca4-126">Plan nie utworzy niezbędnych tabel w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="88ca4-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="88ca4-127">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="88ca4-127">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="88ca4-128">Microsoft. AspNet. Signal</span><span class="sxs-lookup"><span data-stu-id="88ca4-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="88ca4-129">Microsoft. AspNet. Signaler. SqlServer</span><span class="sxs-lookup"><span data-stu-id="88ca4-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="88ca4-130">Tworzenie aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="88ca4-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="88ca4-131">Dodaj następujący kod do Startup.cs w celu skonfigurowania planu:</span><span class="sxs-lookup"><span data-stu-id="88ca4-131">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="88ca4-132">Ten kod konfiguruje plan z wartościami domyślnymi dla [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="88ca4-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="88ca4-133">Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajność sygnałów: metryki skalowania](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="88ca4-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

## <a name="configure-the-database"></a><span data-ttu-id="88ca4-134">Konfigurowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="88ca4-134">Configure the Database</span></span>

<span data-ttu-id="88ca4-135">Zdecyduj, czy aplikacja będzie używać uwierzytelniania systemu Windows, czy SQL Server uwierzytelniania w celu uzyskania dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="88ca4-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="88ca4-136">Bez względu na to upewnij się, że użytkownik bazy danych ma uprawnienia do logowania, tworzenia schematów i tworzenia tabel.</span><span class="sxs-lookup"><span data-stu-id="88ca4-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="88ca4-137">Utwórz nową bazę danych do użycia w celu utworzenia planu.</span><span class="sxs-lookup"><span data-stu-id="88ca4-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="88ca4-138">Można nadać bazie danych dowolną nazwę.</span><span class="sxs-lookup"><span data-stu-id="88ca4-138">You can give the database any name.</span></span> <span data-ttu-id="88ca4-139">Nie musisz tworzyć żadnych tabel w bazie danych; plan nie utworzy niezbędnych tabel.</span><span class="sxs-lookup"><span data-stu-id="88ca4-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="88ca4-140">Włącz Service Broker</span><span class="sxs-lookup"><span data-stu-id="88ca4-140">Enable Service Broker</span></span>

<span data-ttu-id="88ca4-141">Zaleca się włączenie Service Broker dla bazy danych programu planowanie.</span><span class="sxs-lookup"><span data-stu-id="88ca4-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="88ca4-142">Service Broker zapewnia natywną obsługę komunikatów i kolejkowania w SQL Server, co pozwala na efektywniejsze otrzymywanie aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="88ca4-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="88ca4-143">(Jednak plan nie działa również bez Service Broker).</span><span class="sxs-lookup"><span data-stu-id="88ca4-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="88ca4-144">Aby sprawdzić, czy Service Broker jest włączona, zbadaj kolumnę **is\_Broker\_Enabled** w widoku wykazu **sys. databases** .</span><span class="sxs-lookup"><span data-stu-id="88ca4-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="88ca4-145">Aby włączyć Service Broker, użyj następującego zapytania SQL:</span><span class="sxs-lookup"><span data-stu-id="88ca4-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="88ca4-146">Jeśli ta kwerenda wydaje się być zakleszczeniem, upewnij się, że żadne aplikacje nie są połączone z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="88ca4-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="88ca4-147">Jeśli włączono śledzenie, ślady będą również wskazywać, czy Service Broker jest włączona.</span><span class="sxs-lookup"><span data-stu-id="88ca4-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="88ca4-148">Tworzenie aplikacji sygnalizującej</span><span class="sxs-lookup"><span data-stu-id="88ca4-148">Create a SignalR Application</span></span>

<span data-ttu-id="88ca4-149">Utwórz aplikację sygnalizującą, wykonując jeden z następujących samouczków:</span><span class="sxs-lookup"><span data-stu-id="88ca4-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="88ca4-150">Wprowadzenie z sygnałem 2,0</span><span class="sxs-lookup"><span data-stu-id="88ca4-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="88ca4-151">Wprowadzenie z sygnałami 2,0 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="88ca4-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="88ca4-152">Następnie zmodyfikujemy aplikację czatu, aby obsługiwała skalowania z SQL Server.</span><span class="sxs-lookup"><span data-stu-id="88ca4-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="88ca4-153">Najpierw Dodaj pakiet NuGet programu Signaler do projektu.</span><span class="sxs-lookup"><span data-stu-id="88ca4-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="88ca4-154">W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="88ca4-154">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="88ca4-155">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="88ca4-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="88ca4-156">Następnie otwórz plik Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="88ca4-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="88ca4-157">Dodaj następujący kod do metody **Configure** :</span><span class="sxs-lookup"><span data-stu-id="88ca4-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="88ca4-158">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="88ca4-158">Deploy and Run the Application</span></span>

<span data-ttu-id="88ca4-159">Przygotuj wystąpienia systemu Windows Server, aby wdrożyć aplikację sygnalizującą.</span><span class="sxs-lookup"><span data-stu-id="88ca4-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="88ca4-160">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="88ca4-160">Add the IIS role.</span></span> <span data-ttu-id="88ca4-161">Uwzględnij funkcje "projektowanie aplikacji", w tym protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="88ca4-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="88ca4-162">Dołącz także usługę zarządzania (wymienioną w obszarze "narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="88ca4-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="88ca4-163">**Zainstaluj Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="88ca4-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="88ca4-164">Po uruchomieniu Menedżera usług IIS zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub [pobranie instalatora](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="88ca4-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="88ca4-165">W Instalatorze platformy Wyszukaj Web Deploy i zainstaluj Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="88ca4-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="88ca4-166">Sprawdź, czy usługa zarządzania siecią Web jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="88ca4-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="88ca4-167">Jeśli nie, uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="88ca4-167">If not, start the service.</span></span> <span data-ttu-id="88ca4-168">(Jeśli nie widzisz usługi zarządzania siecią Web na liście usług systemu Windows, upewnij się, że usługa zarządzania została zainstalowana po dodaniu roli usług IIS.)</span><span class="sxs-lookup"><span data-stu-id="88ca4-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="88ca4-169">Na koniec Otwórz port 8172 dla protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="88ca4-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="88ca4-170">Jest to port, którego używa narzędzie Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="88ca4-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="88ca4-171">Teraz możesz przystąpić do wdrażania projektu programu Visual Studio z komputera deweloperskiego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="88ca4-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="88ca4-172">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="88ca4-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="88ca4-173">Aby uzyskać bardziej szczegółową dokumentację dotyczącą wdrażania w sieci Web, zobacz [Web Deployment Content Map for Visual Studio i ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="88ca4-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="88ca4-174">Jeśli aplikacja zostanie wdrożona na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobaczyć, że każdy z nich odbiera komunikaty sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="88ca4-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="88ca4-175">(Oczywiście w środowisku produkcyjnym dwa serwery byłyby za modułem równoważenia obciążenia).</span><span class="sxs-lookup"><span data-stu-id="88ca4-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="88ca4-176">Po uruchomieniu aplikacji można zobaczyć, że sygnalizujący automatycznie utworzy tabele w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="88ca4-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="88ca4-177">Program sygnalizujący zarządza tabelami.</span><span class="sxs-lookup"><span data-stu-id="88ca4-177">SignalR manages the tables.</span></span> <span data-ttu-id="88ca4-178">Dopóki aplikacja jest wdrożona, nie usuwaj wierszy, Modyfikuj tabelę i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="88ca4-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
