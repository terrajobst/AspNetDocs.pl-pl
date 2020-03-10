---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Używanie interfejsu Web API 2 z Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web za pomocą zaplecza interfejsu API sieci Web ASP.NET. Samouczek używa Entity Framework 6 do rozmieszczenia danych...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622480"
---
# <a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="900d8-104">Używanie wzorca Web API 2 z programem Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="900d8-104">Using Web API 2 with Entity Framework 6</span></span>

[<span data-ttu-id="900d8-105">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="900d8-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="900d8-106">W tym samouczku przedstawiono podstawowe informacje na temat tworzenia aplikacji sieci Web za pomocą zaplecza interfejsu API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="900d8-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="900d8-107">Samouczek używa Entity Framework 6 dla warstwy danych i odcinania. js dla aplikacji JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="900d8-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="900d8-108">W tym samouczku pokazano również, jak wdrożyć aplikację w Web Apps Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="900d8-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="900d8-109">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="900d8-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="900d8-110">Internetowy interfejs API 2,1</span><span class="sxs-lookup"><span data-stu-id="900d8-110">Web API 2.1</span></span>
> - <span data-ttu-id="900d8-111">Visual Studio 2017 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="900d8-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="900d8-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="900d8-112">Entity Framework 6</span></span>
> - <span data-ttu-id="900d8-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="900d8-113">.NET 4.7</span></span>
> - <span data-ttu-id="900d8-114">[Odcinanie. js](http://knockoutjs.com/) 3,1</span><span class="sxs-lookup"><span data-stu-id="900d8-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="900d8-115">W tym samouczku jest używane ASP.NET Web API 2 z Entity Framework 6 do tworzenia aplikacji sieci Web, która manipuluje bazą danych zaplecza.</span><span class="sxs-lookup"><span data-stu-id="900d8-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="900d8-116">Oto zrzut ekranu przedstawiający aplikację, która zostanie utworzona.</span><span class="sxs-lookup"><span data-stu-id="900d8-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="900d8-117">Aplikacja używa projektu jednostronicowej aplikacji (SPA).</span><span class="sxs-lookup"><span data-stu-id="900d8-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="900d8-118">"Aplikacja jednostronicowa" to ogólny termin aplikacji sieci Web, który ładuje pojedynczą stronę HTML, a następnie automatycznie aktualizuje stronę, zamiast ładować nowe strony.</span><span class="sxs-lookup"><span data-stu-id="900d8-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="900d8-119">Po początkowym ładowaniu strony aplikacja komunikuje się z serwerem za pomocą żądań AJAX.</span><span class="sxs-lookup"><span data-stu-id="900d8-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="900d8-120">Żądania AJAX zwracają dane JSON, których aplikacja używa do aktualizowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="900d8-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="900d8-121">Technologia AJAX nie jest nowa, ale dzisiaj istnieją struktury języka JavaScript, które ułatwiają tworzenie i konserwowanie dużej, zaawansowanej aplikacji SPA.</span><span class="sxs-lookup"><span data-stu-id="900d8-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="900d8-122">W tym samouczku jest używany program [separowania. js](http://knockoutjs.com/), ale można użyć dowolnej struktury klienta języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="900d8-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="900d8-123">Oto główne bloki konstrukcyjne dla tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="900d8-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="900d8-124">ASP.NET MVC tworzy stronę HTML.</span><span class="sxs-lookup"><span data-stu-id="900d8-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="900d8-125">Interfejs API sieci Web ASP.NET obsługuje żądania AJAX i zwraca dane JSON.</span><span class="sxs-lookup"><span data-stu-id="900d8-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="900d8-126">Dane odcinania. js — wiążą elementy HTML z danymi JSON.</span><span class="sxs-lookup"><span data-stu-id="900d8-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="900d8-127">Entity Framework rozmowy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="900d8-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="900d8-128">Zobacz tę aplikację działającą na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="900d8-128">See this app running on Azure</span></span>

<span data-ttu-id="900d8-129">Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo?</span><span class="sxs-lookup"><span data-stu-id="900d8-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="900d8-130">Pełną wersję aplikacji można wdrożyć na koncie platformy Azure, wybierając poniższy przycisk.</span><span class="sxs-lookup"><span data-stu-id="900d8-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="900d8-131">Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="900d8-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="900d8-132">Jeśli nie masz jeszcze konta, możesz:</span><span class="sxs-lookup"><span data-stu-id="900d8-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="900d8-133">[Bezpłatnie Otwórz konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — Uzyskaj kredyty, których możesz użyć do wypróbowania płatnych usług platformy Azure, a nawet po ich użyciu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="900d8-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="900d8-134">[Aktywuj korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) — subskrypcja MSDN daje środki na korzystanie z płatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="900d8-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="900d8-135">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="900d8-135">Create the project</span></span>

<span data-ttu-id="900d8-136">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="900d8-136">Open Visual Studio.</span></span> <span data-ttu-id="900d8-137">Z menu **plik** wybierz pozycję **Nowy**, a następnie wybierz pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="900d8-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="900d8-138">(Lub wybierz pozycję **Nowy projekt** na stronie startowej).</span><span class="sxs-lookup"><span data-stu-id="900d8-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="900d8-139">W oknie dialogowym **Nowy projekt** wybierz pozycję **Sieć Web** w lewym okienku i **aplikację sieci Web ASP.NET (.NET Framework)** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="900d8-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="900d8-140">Nazwij projekt **BookService** i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="900d8-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="900d8-141">W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **internetowego interfejsu API** .</span><span class="sxs-lookup"><span data-stu-id="900d8-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

<span data-ttu-id="900d8-142">Wybierz przycisk **OK**, aby utworzyć projekt.</span><span class="sxs-lookup"><span data-stu-id="900d8-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="900d8-143">Konfigurowanie ustawień platformy Azure (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="900d8-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="900d8-144">Po utworzeniu projektu można wybrać opcję wdrożenia do Azure App Service Web Apps w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="900d8-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="900d8-145">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="900d8-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="900d8-146">W wyświetlonym oknie wybierz pozycję **Rozpocznij**.</span><span class="sxs-lookup"><span data-stu-id="900d8-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="900d8-147">Zostanie wyświetlone okno dialogowe **Wybieranie elementu docelowego publikowania** .</span><span class="sxs-lookup"><span data-stu-id="900d8-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="900d8-148">Wybierz pozycję **Utwórz profil**.</span><span class="sxs-lookup"><span data-stu-id="900d8-148">Select **Create Profile**.</span></span> <span data-ttu-id="900d8-149">Zostanie wyświetlone okno dialogowe **tworzenie App Service** .</span><span class="sxs-lookup"><span data-stu-id="900d8-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="900d8-150">Zaakceptuj wartości domyślne lub wprowadź różne wartości dla nazwy aplikacji, grupy zasobów, planu hostingu, subskrypcji platformy Azure i regionu geograficznego.</span><span class="sxs-lookup"><span data-stu-id="900d8-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="900d8-151">Wybierz pozycję **Utwórz bazę danych SQL**.</span><span class="sxs-lookup"><span data-stu-id="900d8-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="900d8-152">Zostanie wyświetlone okno dialogowe **konfigurowanie SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="900d8-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="900d8-153">Zaakceptuj wartości domyślne lub wprowadź różne wartości.</span><span class="sxs-lookup"><span data-stu-id="900d8-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="900d8-154">Wprowadź **nazwę użytkownika** i **hasło administratora** dla nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="900d8-154">Enter an **Administrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="900d8-155">Po zakończeniu wybierz **przycisk OK** .</span><span class="sxs-lookup"><span data-stu-id="900d8-155">Select **OK** when you're done.</span></span> <span data-ttu-id="900d8-156">Zostanie wyświetlona strona **tworzenie App Service** .</span><span class="sxs-lookup"><span data-stu-id="900d8-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="900d8-157">Wybierz pozycję **Utwórz** , aby utworzyć profil.</span><span class="sxs-lookup"><span data-stu-id="900d8-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="900d8-158">W prawym dolnym rogu pojawia się komunikat z informacją, że wdrożenie jest w toku.</span><span class="sxs-lookup"><span data-stu-id="900d8-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="900d8-159">Po krótkim czasie okno **publikowania** zostanie wyświetlone ponownie.</span><span class="sxs-lookup"><span data-stu-id="900d8-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="900d8-160">Profil utworzony w celu wdrożenia aplikacji jest teraz dostępny.</span><span class="sxs-lookup"><span data-stu-id="900d8-160">The profile you created to deploy the app is now available.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="900d8-161">Dalej</span><span class="sxs-lookup"><span data-stu-id="900d8-161">Next</span></span>](part-2.md)
