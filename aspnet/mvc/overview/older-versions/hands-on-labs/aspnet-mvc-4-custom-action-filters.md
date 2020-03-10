---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtry akcji niestandardowych ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC udostępnia filtry akcji do wykonywania logiki filtrowania przed wywołaniem lub po wywołaniu metody akcji. Filtry akcji są atrybutami niestandardowymi określona...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579696"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="58c4e-104">ASP.NET MVC 4 — niestandardowe filtry akcji</span><span class="sxs-lookup"><span data-stu-id="58c4e-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="58c4e-105">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="58c4e-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="58c4e-106">Pobierz zestaw szkoleniowy dla sieci Web Camp</span><span class="sxs-lookup"><span data-stu-id="58c4e-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="58c4e-107">ASP.NET MVC udostępnia filtry akcji do wykonywania logiki filtrowania przed wywołaniem lub po wywołaniu metody akcji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="58c4e-108">Filtry akcji są atrybutami niestandardowymi, które zapewniają deklaratywne środki do dodawania działań przed akcją i po akcji do metod akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="58c4e-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="58c4e-109">W tym ćwiczeniu można utworzyć niestandardowy atrybut filtru akcji w rozwiązaniu MvcMusicStore w celu przechwycenia żądań kontrolera i zarejestrowania działania lokacji w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58c4e-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="58c4e-110">Można dodać filtr rejestrowania przez iniekcję do dowolnego kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="58c4e-111">Na koniec zostanie wyświetlony widok dziennika, w którym zostanie wyświetlona lista odwiedzających.</span><span class="sxs-lookup"><span data-stu-id="58c4e-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="58c4e-112">W ramach tego ćwiczenia praktycznego założono, że masz podstawową wiedzę na temat **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="58c4e-113">Jeśli wcześniej nie korzystasz z **ASP.NET MVC** , zalecamy przechodzenie przez **ASP.NETe podstawowe wskazówki dotyczące MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="58c4e-114">Wszystkie przykładowe kod i fragmenty kodu są dołączone do zestawu szkoleniowego dla sieci Web Camp, dostępnego w [wersji Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="58c4e-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="58c4e-115">Projekt specyficzny dla tego laboratorium jest dostępny w [niestandardowych filtrach akcji ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="58c4e-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="58c4e-116">Cele</span><span class="sxs-lookup"><span data-stu-id="58c4e-116">Objectives</span></span>

<span data-ttu-id="58c4e-117">W tym ćwiczeniu dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="58c4e-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="58c4e-118">Utwórz atrybut niestandardowego filtru akcji, aby zwiększyć możliwości filtrowania</span><span class="sxs-lookup"><span data-stu-id="58c4e-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="58c4e-119">Stosowanie niestandardowego atrybutu filtru przez iniekcję do określonego poziomu</span><span class="sxs-lookup"><span data-stu-id="58c4e-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="58c4e-120">Globalne rejestrowanie filtrów akcji niestandardowych</span><span class="sxs-lookup"><span data-stu-id="58c4e-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="58c4e-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="58c4e-121">Prerequisites</span></span>

<span data-ttu-id="58c4e-122">Aby ukończyć to laboratorium, musisz mieć następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="58c4e-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="58c4e-123">[Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="58c4e-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="58c4e-124">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="58c4e-124">Setup</span></span>

<span data-ttu-id="58c4e-125">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="58c4e-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="58c4e-126">Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58c4e-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="58c4e-127">Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="58c4e-128">Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku C: Using fragmenty kodu](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="58c4e-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="58c4e-129">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="58c4e-129">Exercises</span></span>

<span data-ttu-id="58c4e-130">To laboratorium praktyczne obejmuje następujące ćwiczenia:</span><span class="sxs-lookup"><span data-stu-id="58c4e-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="58c4e-131">Ćwiczenie 1: akcje rejestrowania</span><span class="sxs-lookup"><span data-stu-id="58c4e-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="58c4e-132">Ćwiczenie 2: Zarządzanie wieloma filtrami akcji</span><span class="sxs-lookup"><span data-stu-id="58c4e-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="58c4e-133">Szacowany czas wykonywania tego laboratorium: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="58c4e-134">Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="58c4e-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="58c4e-135">Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="58c4e-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="58c4e-136">Ćwiczenie 1: akcje rejestrowania</span><span class="sxs-lookup"><span data-stu-id="58c4e-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="58c4e-137">W tym ćwiczeniu dowiesz się, jak utworzyć niestandardowy filtr dziennika akcji przy użyciu dostawców filtrów ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="58c4e-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="58c4e-138">W tym celu należy zastosować filtr rejestrowania do witryny MusicStore, która będzie rejestrować wszystkie działania na wybranych kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="58c4e-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="58c4e-139">Filtr rozszerzy **ActionFilterAttributeClass** i zastąpi metodę **OnActionExecuting** , aby przechwycić każde żądanie, a następnie wykonać akcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="58c4e-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="58c4e-140">Informacje kontekstowe dotyczące żądań HTTP, wykonywania metod, wyników i parametrów zostaną dostarczone przez klasę ASP.NET MVC **ActionExecutingContext** **.**</span><span class="sxs-lookup"><span data-stu-id="58c4e-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="58c4e-141">ASP.NET MVC 4 ma także domyślnych dostawców filtrów, których można użyć bez tworzenia filtru niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="58c4e-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="58c4e-142">ASP.NET MVC 4 udostępnia następujące typy filtrów:</span><span class="sxs-lookup"><span data-stu-id="58c4e-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="58c4e-143">Filtr **autoryzacji** , który podejmuje decyzje dotyczące zabezpieczeń, aby wykonać metodę akcji, taką jak uwierzytelnianie lub Walidacja właściwości żądania.</span><span class="sxs-lookup"><span data-stu-id="58c4e-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="58c4e-144">Filtr **akcji** , który otacza wykonywanie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="58c4e-145">Ten filtr może wykonywać dodatkowe przetwarzanie, takie jak dostarczanie dodatkowych danych do metody akcji, badanie wartości zwracanej lub anulowanie wykonywania metody akcji</span><span class="sxs-lookup"><span data-stu-id="58c4e-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="58c4e-146">Filtr **wyników** , który otacza wykonywanie obiektu ActionResult.</span><span class="sxs-lookup"><span data-stu-id="58c4e-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="58c4e-147">Ten filtr może wykonać dodatkowe przetwarzanie wyniku, na przykład modyfikując odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="58c4e-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="58c4e-148">Filtr **wyjątku** , który jest wykonywany, jeśli wystąpił nieobsługiwany wyjątek w metodzie działania, rozpoczynając od filtrów autoryzacji i kończąc na wykonaniu wyniku.</span><span class="sxs-lookup"><span data-stu-id="58c4e-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="58c4e-149">Filtry wyjątków mogą służyć do zadań takich jak rejestrowanie lub wyświetlanie strony błędu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="58c4e-150">Więcej informacji o dostawcach filtrów można znaleźć w tym łączu MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="58c4e-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="58c4e-151">Informacje o funkcji rejestrowania aplikacji ze sklepu MVC Music</span><span class="sxs-lookup"><span data-stu-id="58c4e-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="58c4e-152">To rozwiązanie do przechowywania utworów muzycznych ma nową tabelę modelu danych do rejestrowania witryn, **ActionLog**z następującymi polami: nazwa kontrolera, który odebrał żądanie, o nazwie Action, IP klienta i sygnatura czasowa.</span><span class="sxs-lookup"><span data-stu-id="58c4e-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="58c4e-153">![Model danych. Tabela ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Model danych. Tabela ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="58c4e-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="58c4e-154">*Model danych — tabela ActionLog*</span><span class="sxs-lookup"><span data-stu-id="58c4e-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="58c4e-155">Rozwiązanie udostępnia widok ASP.NET MVC dla dziennika akcji, który można znaleźć w **MvcMusicStores/views/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="58c4e-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="58c4e-156">![Widok dziennika akcji](aspnet-mvc-4-custom-action-filters/_static/image2.png "Widok dziennika akcji")</span><span class="sxs-lookup"><span data-stu-id="58c4e-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="58c4e-157">*Widok dziennika akcji*</span><span class="sxs-lookup"><span data-stu-id="58c4e-157">*Action Log view*</span></span>

<span data-ttu-id="58c4e-158">W przypadku tej struktury cała służbowa będzie skoncentrowana na żądaniu na kontrolerze i przeprowadzeniu rejestrowania przy użyciu filtrowania niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="58c4e-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="58c4e-159">Zadanie 1 — Tworzenie niestandardowego filtru do przechwytywania żądania kontrolera</span><span class="sxs-lookup"><span data-stu-id="58c4e-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="58c4e-160">W tym zadaniu utworzysz klasę niestandardowego atrybutu filtru, która będzie zawierać logikę rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="58c4e-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="58c4e-161">W tym celu zostanie rozszerzona Klasa ASP.NET MVC **ActionFilterAttribute** i zaimplementowano Interfejs **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="58c4e-162">**ActionFilterAttribute** jest klasą bazową dla wszystkich filtrów atrybutów.</span><span class="sxs-lookup"><span data-stu-id="58c4e-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="58c4e-163">Udostępnia następujące metody, aby wykonać określoną logikę po wykonaniu akcji kontrolera i przed nią:</span><span class="sxs-lookup"><span data-stu-id="58c4e-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="58c4e-164">**OnActionExecuting**(ActionExecutingContext filterContext): tuż przed wywołaniem metody Action.</span><span class="sxs-lookup"><span data-stu-id="58c4e-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="58c4e-165">**OnActionExecuted**(ActionExecutedContext filterContext): po wywołaniu metody akcji i przed wykonaniem wyniku (przed renderowaniem widoku).</span><span class="sxs-lookup"><span data-stu-id="58c4e-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="58c4e-166">**OnResultExecuting**(ResultExecutingContext filterContext): tuż przed wykonaniem wyniku (przed renderowaniem widoku).</span><span class="sxs-lookup"><span data-stu-id="58c4e-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="58c4e-167">**OnResultExecuted**(ResultExecutedContext filterContext): po wykonaniu wyniku (po wyrenderowaniu widoku).</span><span class="sxs-lookup"><span data-stu-id="58c4e-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="58c4e-168">Zastępowanie którejkolwiek z tych metod w klasie pochodnej, można wykonać własny kod filtrowania.</span><span class="sxs-lookup"><span data-stu-id="58c4e-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>

1. <span data-ttu-id="58c4e-169">Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **\Source\Ex01-LoggingActions\Begin** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="58c4e-170">Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="58c4e-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="58c4e-171">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="58c4e-172">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="58c4e-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="58c4e-173">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="58c4e-174">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="58c4e-175">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="58c4e-176">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="58c4e-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="58c4e-177">Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="58c4e-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="58c4e-178">Dodaj nową C# klasę do folderu **filters** i nadaj jej nazwę *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="58c4e-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="58c4e-179">W tym folderze będą przechowywane wszystkie filtry niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="58c4e-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="58c4e-180">Otwórz **CustomActionFilter.cs** i Dodaj odwołanie do przestrzeni nazw **System. Web. MVC** i **MvcMusicStore. models** :</span><span class="sxs-lookup"><span data-stu-id="58c4e-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="58c4e-181">(Fragment kodu — *niestandardowe filtry akcji ASP.NET MVC 4-Ex1-CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="58c4e-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="58c4e-182">Dziedzicz klasę **CustomActionFilter** z **ActionFilterAttribute** , a następnie ustaw klasę **CustomActionFilter** implementującą interfejs **IActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="58c4e-183">Utwórz klasę **CustomActionFilter** , zastępując metodę **OnActionExecuting** i Dodaj wymaganą logikę w celu zarejestrowania wykonywania filtru.</span><span class="sxs-lookup"><span data-stu-id="58c4e-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="58c4e-184">W tym celu Dodaj następujący wyróżniony kod w klasie **CustomActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="58c4e-185">(Fragment kodu — *niestandardowe filtry akcji ASP.NET MVC 4-Ex1-LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="58c4e-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="58c4e-186">Metoda **OnActionExecuting** używa **Entity Framework** , aby dodać nowy rejestr ActionLog.</span><span class="sxs-lookup"><span data-stu-id="58c4e-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="58c4e-187">Tworzy i wypełnia nowe wystąpienie jednostki z informacjami kontekstu z **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="58c4e-188">Więcej informacji na temat klasy **ControllerContext** można znaleźć w [witrynie MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="58c4e-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="58c4e-189">Zadanie 2 — wprowadzanie interceptora kodu do klasy kontrolera magazynu</span><span class="sxs-lookup"><span data-stu-id="58c4e-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="58c4e-190">W tym zadaniu zostanie dodany filtr niestandardowy przez wstrzyknięcie go do wszystkich klas kontrolera i akcji kontrolerów, które będą rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="58c4e-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="58c4e-191">Na potrzeby tego ćwiczenia Klasa kontrolera magazynu będzie miała dziennik.</span><span class="sxs-lookup"><span data-stu-id="58c4e-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="58c4e-192">Metoda **OnActionExecuting** z niestandardowego filtru **ActionLogFilterAttribute** jest uruchamiana po wywołaniu wstrzykniętego elementu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="58c4e-193">Istnieje również możliwość przechwycenia konkretnej metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="58c4e-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="58c4e-194">Otwórz **StoreController** w **MvcMusicStore\Controllers** i Dodaj odwołanie do przestrzeni nazw **filters** :</span><span class="sxs-lookup"><span data-stu-id="58c4e-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="58c4e-195">Wstrzyknąć filtr niestandardowy **CustomActionFilter** do klasy **StoreController** przez dodanie atrybutu **[CustomActionFilter]** przed deklaracją klasy.</span><span class="sxs-lookup"><span data-stu-id="58c4e-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="58c4e-196">Gdy filtr jest wstrzykiwany do klasy kontrolera, wszystkie jego działania są również wstrzykiwane.</span><span class="sxs-lookup"><span data-stu-id="58c4e-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="58c4e-197">Jeśli chcesz zastosować filtr tylko dla zestawu akcji, musisz wstrzyknąć **[CustomActionFilter]** do każdego z nich:</span><span class="sxs-lookup"><span data-stu-id="58c4e-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="58c4e-198">Zadanie 3 — Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="58c4e-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="58c4e-199">W tym zadaniu zostanie przetestowane, że filtr rejestrowania działa.</span><span class="sxs-lookup"><span data-stu-id="58c4e-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="58c4e-200">Uruchomisz aplikację i przejdziesz do sklepu, a następnie sprawdzisz zarejestrowane działania.</span><span class="sxs-lookup"><span data-stu-id="58c4e-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="58c4e-201">Naciśnij klawisz **F5**, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="58c4e-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="58c4e-202">Przejdź do **/ActionLog** , aby wyświetlić stan początkowy widoku dziennika:</span><span class="sxs-lookup"><span data-stu-id="58c4e-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="58c4e-203">![Stan śledzenia dzienników przed aktywnością strony](aspnet-mvc-4-custom-action-filters/_static/image3.png "Stan śledzenia dzienników przed aktywnością strony")</span><span class="sxs-lookup"><span data-stu-id="58c4e-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="58c4e-204">*Stan śledzenia dzienników przed aktywnością strony*</span><span class="sxs-lookup"><span data-stu-id="58c4e-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="58c4e-205">Domyślnie zawsze będzie wyświetlany jeden element, który jest generowany podczas pobierania istniejących gatunków menu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="58c4e-206">W celach prostoty czyścimy tabelę **ActionLog** za każdym razem, gdy aplikacja zostanie uruchomiona, aby wyświetlić tylko dzienniki poszczególnych zadań.</span><span class="sxs-lookup"><span data-stu-id="58c4e-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="58c4e-207">Aby zapisać dziennik historyczny dla wszystkich akcji wykonywanych w ramach kontrolera magazynu, może być konieczne usunięcie następującego kodu z **sesji\_Metoda Start** (w klasie **Global. asax** ).</span><span class="sxs-lookup"><span data-stu-id="58c4e-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="58c4e-208">Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="58c4e-209">Przejdź do **/ActionLog** i jeśli dziennik jest pusty, naciśnij klawisz **F5** , aby odświeżyć stronę.</span><span class="sxs-lookup"><span data-stu-id="58c4e-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="58c4e-210">Sprawdź, czy Twoje wizyty zostały śledzone:</span><span class="sxs-lookup"><span data-stu-id="58c4e-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="58c4e-211">![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image4.png "Dziennik akcji z zarejestrowanym działaniem")</span><span class="sxs-lookup"><span data-stu-id="58c4e-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58c4e-212">*Dziennik akcji z zarejestrowanym działaniem*</span><span class="sxs-lookup"><span data-stu-id="58c4e-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="58c4e-213">Ćwiczenie 2: Zarządzanie wieloma filtrami akcji</span><span class="sxs-lookup"><span data-stu-id="58c4e-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="58c4e-214">W tym ćwiczeniu zostanie dodany drugi filtr akcji niestandardowej do klasy StoreController i zostanie określona kolejność wykonywania obu filtrów.</span><span class="sxs-lookup"><span data-stu-id="58c4e-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="58c4e-215">Następnie zaktualizujesz kod w celu zarejestrowania globalnie filtru.</span><span class="sxs-lookup"><span data-stu-id="58c4e-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="58c4e-216">Istnieją różne opcje, które należy wziąć pod uwagę podczas definiowania kolejności wykonywania filtrów.</span><span class="sxs-lookup"><span data-stu-id="58c4e-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="58c4e-217">Na przykład Właściwość Order i zakres filters:</span><span class="sxs-lookup"><span data-stu-id="58c4e-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="58c4e-218">Można zdefiniować **zakres** dla każdego z filtrów, na przykład można określić zakres wszystkich filtrów akcji do uruchomienia w ramach **zakresu kontrolera**i wszystkie filtry autoryzacji do uruchomienia w **zakresie globalnym**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="58c4e-219">Zakresy mają zdefiniowane zamówienie wykonania.</span><span class="sxs-lookup"><span data-stu-id="58c4e-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="58c4e-220">Ponadto każdy filtr akcji ma Właściwość Order, która jest używana do określenia kolejności wykonywania w zakresie filtru.</span><span class="sxs-lookup"><span data-stu-id="58c4e-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="58c4e-221">Aby uzyskać więcej informacji na temat kolejności wykonywania filtrów akcji niestandardowych, odwiedź ten artykuł w witrynie MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="58c4e-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="58c4e-222">Zadanie 1. Tworzenie nowego niestandardowego filtru akcji</span><span class="sxs-lookup"><span data-stu-id="58c4e-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="58c4e-223">W tym zadaniu utworzysz nowy niestandardowy filtr akcji, aby wstrzyknąć do klasy StoreController, jak zarządzać kolejnością wykonywania filtrów.</span><span class="sxs-lookup"><span data-stu-id="58c4e-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="58c4e-224">Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **\Source\Ex02-ManagingMultipleActionFilters\Begin** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="58c4e-225">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="58c4e-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="58c4e-226">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="58c4e-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="58c4e-227">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="58c4e-228">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="58c4e-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="58c4e-229">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="58c4e-230">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="58c4e-231">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="58c4e-232">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="58c4e-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="58c4e-233">Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="58c4e-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="58c4e-234">Dodaj nową C# klasę do folderu **filters** i nadaj jej nazwę *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="58c4e-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="58c4e-235">Otwórz **MyNewCustomActionFilter.cs** i Dodaj odwołanie do przestrzeni nazw **System. Web. MVC** i **MvcMusicStore. models** :</span><span class="sxs-lookup"><span data-stu-id="58c4e-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="58c4e-236">(Fragment kodu — *niestandardowe filtry akcji ASP.NET MVC 4-Ex2-MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="58c4e-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="58c4e-237">Zastąp domyślną deklarację klasy poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="58c4e-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="58c4e-238">(Fragment kodu — *niestandardowe filtry akcji ASP.NET MVC 4-Ex2-MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="58c4e-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="58c4e-239">Ten filtr akcji niestandardowej jest prawie taki sam jak ten, który został utworzony w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="58c4e-240">Główną różnicą jest to, że ma *&quot;zarejestrowany przez atrybut&quot;* aktualizowany przy użyciu tej nowej klasy, aby zidentyfikować filtr zarejestrowany w dzienniku.</span><span class="sxs-lookup"><span data-stu-id="58c4e-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify which filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="58c4e-241">Zadanie 2: wprowadzanie nowego przechwycenia kodu do klasy StoreController</span><span class="sxs-lookup"><span data-stu-id="58c4e-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="58c4e-242">W tym zadaniu dodasz nowy filtr niestandardowy do klasy StoreController i uruchomisz rozwiązanie, aby sprawdzić, jak oba filtry współpracują ze sobą.</span><span class="sxs-lookup"><span data-stu-id="58c4e-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="58c4e-243">Otwórz klasę **StoreController** znajdującą się w **MvcMusicStore\Controllers** i wstrzyknąć nowy filtr niestandardowy **MyNewCustomActionFilter** do klasy **StoreController** , jak pokazano w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="58c4e-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="58c4e-244">Teraz uruchom aplikację, aby zobaczyć, jak działają te dwa niestandardowe filtry akcji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="58c4e-245">Aby to zrobić, naciśnij klawisz **F5** i poczekaj na uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="58c4e-246">Przejdź do **/ActionLog** , aby wyświetlić stan początkowy widoku dziennika.</span><span class="sxs-lookup"><span data-stu-id="58c4e-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="58c4e-247">![Stan śledzenia dzienników przed aktywnością strony](aspnet-mvc-4-custom-action-filters/_static/image5.png "Stan śledzenia dzienników przed aktywnością strony")</span><span class="sxs-lookup"><span data-stu-id="58c4e-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="58c4e-248">*Stan śledzenia dzienników przed aktywnością strony*</span><span class="sxs-lookup"><span data-stu-id="58c4e-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="58c4e-249">Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="58c4e-250">Sprawdź ten czas; wizyty zostały śledzone dwa razy: jeden raz dla każdego z filtrów akcji niestandardowych, które zostały dodane w klasie **StorageController** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="58c4e-251">![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image6.png "Dziennik akcji z zarejestrowanym działaniem")</span><span class="sxs-lookup"><span data-stu-id="58c4e-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58c4e-252">*Dziennik akcji z zarejestrowanym działaniem*</span><span class="sxs-lookup"><span data-stu-id="58c4e-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="58c4e-253">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="58c4e-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="58c4e-254">Zadanie 3. Zarządzanie porządkowaniem filtrów</span><span class="sxs-lookup"><span data-stu-id="58c4e-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="58c4e-255">W tym zadaniu dowiesz się, jak zarządzać kolejnością wykonywania filtrów za pomocą właściwości Order.</span><span class="sxs-lookup"><span data-stu-id="58c4e-255">In this task, you will learn how to manage the filters' execution order by using the Order property.</span></span>

1. <span data-ttu-id="58c4e-256">Otwórz klasę **StoreController** znajdującą się w **MvcMusicStore\Controllers** i określ Właściwość **Order** w obu filtrach, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="58c4e-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="58c4e-257">Teraz sprawdź, w jaki sposób filtry są wykonywane, w zależności od wartości właściwości Order.</span><span class="sxs-lookup"><span data-stu-id="58c4e-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="58c4e-258">Zobaczysz, że filtr o najmniejszej wartości kolejności (**CustomActionFilter**) to pierwszy, który jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="58c4e-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="58c4e-259">Naciśnij klawisz **F5** i poczekaj na uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="58c4e-260">Przejdź do **/ActionLog** , aby wyświetlić stan początkowy widoku dziennika.</span><span class="sxs-lookup"><span data-stu-id="58c4e-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="58c4e-261">![Stan śledzenia dzienników przed aktywnością strony](aspnet-mvc-4-custom-action-filters/_static/image7.png "Stan śledzenia dzienników przed aktywnością strony")</span><span class="sxs-lookup"><span data-stu-id="58c4e-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="58c4e-262">*Stan śledzenia dzienników przed aktywnością strony*</span><span class="sxs-lookup"><span data-stu-id="58c4e-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="58c4e-263">Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="58c4e-264">Sprawdź, czy w tym czasie wizyty zostały śledzone uporządkowane według wartości kolejności filtrów: **CustomActionFilter** Logs '.</span><span class="sxs-lookup"><span data-stu-id="58c4e-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="58c4e-265">![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image8.png "Dziennik akcji z zarejestrowanym działaniem")</span><span class="sxs-lookup"><span data-stu-id="58c4e-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58c4e-266">*Dziennik akcji z zarejestrowanym działaniem*</span><span class="sxs-lookup"><span data-stu-id="58c4e-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="58c4e-267">Teraz zaktualizujesz wartość kolejności filtrów i sprawdź, jak zmienia się kolejność rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="58c4e-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="58c4e-268">W klasie **StoreController** zaktualizuj wartość kolejności filtrów, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="58c4e-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="58c4e-269">Uruchom aplikację ponownie, naciskając klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="58c4e-270">Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="58c4e-271">Sprawdź, czy w tym czasie najpierw pojawiają się dzienniki utworzone za pomocą filtru **MyNewCustomActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="58c4e-272">![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image9.png "Dziennik akcji z zarejestrowanym działaniem")</span><span class="sxs-lookup"><span data-stu-id="58c4e-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58c4e-273">*Dziennik akcji z zarejestrowanym działaniem*</span><span class="sxs-lookup"><span data-stu-id="58c4e-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="58c4e-274">Zadanie 4. rejestrowanie filtrów globalnie</span><span class="sxs-lookup"><span data-stu-id="58c4e-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="58c4e-275">W tym zadaniu aktualizujesz rozwiązanie w celu zarejestrowania nowego filtru (**MyNewCustomActionFilter**) jako filtru globalnego.</span><span class="sxs-lookup"><span data-stu-id="58c4e-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="58c4e-276">W ten sposób zostanie wyzwolone przez wszystkie akcje wykonywane w aplikacji, a nie tylko w StoreController, jak w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-276">By doing this, it will be triggered by all the actions performed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="58c4e-277">W klasie **StoreController** Usuń atrybut **[MyNewCustomActionFilter]** i Właściwość Order z **[CustomActionFilter]** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="58c4e-278">Powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="58c4e-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="58c4e-279">Otwórz plik **Global. asax** i znajdź **aplikację\_Start** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="58c4e-280">Należy zauważyć, że za każdym razem, gdy aplikacja uruchamia ją, rejestruje filtry globalne przez wywołanie metody **RegisterGlobalFilters** w klasie **FilterConfig** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="58c4e-281">![Rejestrowanie filtrów globalnych w Global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Rejestrowanie filtrów globalnych w Global. asax")</span><span class="sxs-lookup"><span data-stu-id="58c4e-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="58c4e-282">*Rejestrowanie filtrów globalnych w Global. asax*</span><span class="sxs-lookup"><span data-stu-id="58c4e-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="58c4e-283">Otwórz plik **FilterConfig.cs** w folderze **Start\_** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="58c4e-284">Dodaj odwołanie do programu System. Web. MVC; Korzystanie z MvcMusicStore. filters; obszaru.</span><span class="sxs-lookup"><span data-stu-id="58c4e-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="58c4e-285">Aktualizowanie metody **RegisterGlobalFilters** Dodawanie filtru niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="58c4e-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="58c4e-286">Aby to zrobić, Dodaj wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="58c4e-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="58c4e-287">Uruchom aplikację, naciskając klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="58c4e-288">Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="58c4e-289">Sprawdź, czy teraz **[MyNewCustomActionFilter]** jest wstrzykiwany w HomeController i ActionLogController.</span><span class="sxs-lookup"><span data-stu-id="58c4e-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="58c4e-290">![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image11.png "Dziennik akcji z zarejestrowanym działaniem")</span><span class="sxs-lookup"><span data-stu-id="58c4e-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="58c4e-291">*Dziennik akcji z zarejestrowanym działaniem globalnym*</span><span class="sxs-lookup"><span data-stu-id="58c4e-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="58c4e-292">Ponadto tę aplikację można wdrożyć w witrynach sieci Web systemu Windows Azure, które zostały opisane w [dodatku B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="58c4e-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="58c4e-293">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="58c4e-293">Summary</span></span>

<span data-ttu-id="58c4e-294">Dzięki zakończeniu tego praktycznego laboratorium wiesz już, jak zwiększyć filtr akcji do wykonywania akcji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="58c4e-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="58c4e-295">Dowiesz się również, jak wstrzyknąć dowolny filtr do kontrolerów strony.</span><span class="sxs-lookup"><span data-stu-id="58c4e-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="58c4e-296">Zostały użyte następujące pojęcia:</span><span class="sxs-lookup"><span data-stu-id="58c4e-296">The following concepts were used:</span></span>

- <span data-ttu-id="58c4e-297">Jak utworzyć niestandardowe filtry akcji przy użyciu klasy ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="58c4e-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="58c4e-298">Jak wstrzyknąć filtry do kontrolerów ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="58c4e-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="58c4e-299">Jak zarządzać kolejnością filtrowania przy użyciu właściwości Order</span><span class="sxs-lookup"><span data-stu-id="58c4e-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="58c4e-300">Jak zarejestrować filtry globalnie</span><span class="sxs-lookup"><span data-stu-id="58c4e-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="58c4e-301">Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web</span><span class="sxs-lookup"><span data-stu-id="58c4e-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="58c4e-302">Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="58c4e-303">Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.</span><span class="sxs-lookup"><span data-stu-id="58c4e-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="58c4e-304">Przejdź do obszaru [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169) (Ustawienia — Integracje i usługi).</span><span class="sxs-lookup"><span data-stu-id="58c4e-304">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="58c4e-305">Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="58c4e-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="58c4e-306">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-306">Click on **Install Now**.</span></span> <span data-ttu-id="58c4e-307">Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="58c4e-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="58c4e-308">Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.</span><span class="sxs-lookup"><span data-stu-id="58c4e-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="58c4e-309">![Zainstaluj Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Zainstaluj Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="58c4e-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="58c4e-310">*Zainstaluj Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="58c4e-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="58c4e-311">Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="58c4e-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="58c4e-313">*Akceptowanie postanowień licencyjnych*</span><span class="sxs-lookup"><span data-stu-id="58c4e-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="58c4e-314">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-314">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="58c4e-316">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="58c4e-316">*Installation progress*</span></span>
6. <span data-ttu-id="58c4e-317">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-317">When the installation completes, click **Finish**.</span></span>

    ![Instalacja zakończona](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="58c4e-319">*Instalacja zakończona*</span><span class="sxs-lookup"><span data-stu-id="58c4e-319">*Installation completed*</span></span>
7. <span data-ttu-id="58c4e-320">Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="58c4e-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="58c4e-321">Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Kafelek VS Express for Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="58c4e-323">*Kafelek VS Express for Web*</span><span class="sxs-lookup"><span data-stu-id="58c4e-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="58c4e-324">Dodatek B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="58c4e-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="58c4e-325">W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web na podstawie portal zarządzania systemu Windows Azure i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy dostępnej w systemie Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="58c4e-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="58c4e-326">Zadanie 1 — Tworzenie nowej witryny sieci Web z poziomu portalu systemu Windows Azure</span><span class="sxs-lookup"><span data-stu-id="58c4e-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="58c4e-327">Przejdź do [Portal zarządzania systemu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="58c4e-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="58c4e-328">System Windows Azure umożliwia bezpłatne hostowanie witryn sieci Web 10 ASP.NET, a następnie skalowanie ich w miarę wzrostu ruchu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="58c4e-329">Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="58c4e-329">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="58c4e-330">![Zaloguj się do systemu Windows Azure Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Zaloguj się do systemu Windows Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="58c4e-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="58c4e-331">*Zaloguj się do usługi Windows Azure portal zarządzania*</span><span class="sxs-lookup"><span data-stu-id="58c4e-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="58c4e-332">Na pasku poleceń kliknij pozycję **Nowy** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="58c4e-333">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "Tworzenie nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="58c4e-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="58c4e-334">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="58c4e-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="58c4e-335">Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="58c4e-336">Następnie wybierz opcję **szybkie tworzenie** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="58c4e-337">Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="58c4e-338">Witryna sieci Web systemu Windows Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią.</span><span class="sxs-lookup"><span data-stu-id="58c4e-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="58c4e-339">Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web w witrynie sieci Web systemu Windows Azure spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="58c4e-340">Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58c4e-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="58c4e-341">![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](aspnet-mvc-4-custom-action-filters/_static/image19.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")</span><span class="sxs-lookup"><span data-stu-id="58c4e-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="58c4e-342">*Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*</span><span class="sxs-lookup"><span data-stu-id="58c4e-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="58c4e-343">Poczekaj na utworzenie nowej **witryny sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="58c4e-344">Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="58c4e-345">Sprawdź, czy nowa witryna sieci Web działa.</span><span class="sxs-lookup"><span data-stu-id="58c4e-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="58c4e-346">![Przeglądanie w nowej witrynie sieci Web](aspnet-mvc-4-custom-action-filters/_static/image20.png "Przeglądanie w nowej witrynie sieci Web")</span><span class="sxs-lookup"><span data-stu-id="58c4e-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="58c4e-347">*Przeglądanie w nowej witrynie sieci Web*</span><span class="sxs-lookup"><span data-stu-id="58c4e-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="58c4e-348">![Uruchomiona witryna sieci Web](aspnet-mvc-4-custom-action-filters/_static/image21.png "Uruchomiona witryna sieci Web")</span><span class="sxs-lookup"><span data-stu-id="58c4e-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="58c4e-349">*Uruchomiona witryna sieci Web*</span><span class="sxs-lookup"><span data-stu-id="58c4e-349">*Web site running*</span></span>
6. <span data-ttu-id="58c4e-350">Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="58c4e-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="58c4e-351">![Otwieranie stron zarządzania witryną sieci Web](aspnet-mvc-4-custom-action-filters/_static/image22.png "Otwieranie stron zarządzania witryną sieci Web")</span><span class="sxs-lookup"><span data-stu-id="58c4e-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="58c4e-352">*Otwieranie stron zarządzania witryną sieci Web*</span><span class="sxs-lookup"><span data-stu-id="58c4e-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="58c4e-353">Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .</span><span class="sxs-lookup"><span data-stu-id="58c4e-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="58c4e-354">*Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web w witrynie sieci Web systemu Windows Azure dla każdej z włączonych metod publikacji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="58c4e-355">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="58c4e-356">**Program Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów do publikowania aplikacji sieci Web w usłudze Microsoft Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="58c4e-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="58c4e-357">![Pobieranie profilu publikowania witryny sieci Web](aspnet-mvc-4-custom-action-filters/_static/image23.png "Pobieranie profilu publikowania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="58c4e-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="58c4e-358">*Pobieranie profilu publikowania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="58c4e-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="58c4e-359">Pobierz plik profilu publikowania do znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="58c4e-360">W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web w witrynach sieci Web systemu Windows Azure z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58c4e-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="58c4e-361">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image24.png "Zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="58c4e-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="58c4e-362">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="58c4e-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="58c4e-363">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="58c4e-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="58c4e-364">Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer.</span><span class="sxs-lookup"><span data-stu-id="58c4e-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="58c4e-365">Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="58c4e-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="58c4e-366">Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database.</span><span class="sxs-lookup"><span data-stu-id="58c4e-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="58c4e-367">Serwery SQL Database z subskrypcji można wyświetlić w portalu zarządzania systemu Windows Azure w obszarze **bazy danych SQL** | **serwery** | **pulpitu nawigacyjnego serwera**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="58c4e-368">Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="58c4e-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="58c4e-369">Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach.</span><span class="sxs-lookup"><span data-stu-id="58c4e-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="58c4e-370">Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.</span><span class="sxs-lookup"><span data-stu-id="58c4e-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="58c4e-371">![Pulpit nawigacyjny serwera SQL Database](aspnet-mvc-4-custom-action-filters/_static/image25.png "Pulpit nawigacyjny serwera SQL Database")</span><span class="sxs-lookup"><span data-stu-id="58c4e-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="58c4e-372">*Pulpit nawigacyjny serwera SQL Database*</span><span class="sxs-lookup"><span data-stu-id="58c4e-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="58c4e-373">W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera.</span><span class="sxs-lookup"><span data-stu-id="58c4e-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="58c4e-374">W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](aspnet-mvc-4-custom-action-filters/_static/image26.png).</span><span class="sxs-lookup"><span data-stu-id="58c4e-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="58c4e-376">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="58c4e-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="58c4e-377">Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="58c4e-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdź zmiany](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="58c4e-379">*Potwierdź zmiany*</span><span class="sxs-lookup"><span data-stu-id="58c4e-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="58c4e-380">Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="58c4e-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="58c4e-381">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="58c4e-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="58c4e-382">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="58c4e-383">![Publikowanie aplikacji](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publikowanie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="58c4e-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="58c4e-384">*Publikowanie witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="58c4e-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="58c4e-385">Zaimportuj profil publikowania zapisany w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="58c4e-386">![Importowanie profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="58c4e-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="58c4e-387">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="58c4e-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="58c4e-388">Kliknij pozycję **Weryfikuj połączenie**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-388">Click **Validate Connection**.</span></span> <span data-ttu-id="58c4e-389">Po zakończeniu walidacji kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="58c4e-390">Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.</span><span class="sxs-lookup"><span data-stu-id="58c4e-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="58c4e-391">![Weryfikowanie połączenia](aspnet-mvc-4-custom-action-filters/_static/image31.png "Weryfikowanie połączenia")</span><span class="sxs-lookup"><span data-stu-id="58c4e-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="58c4e-392">*Weryfikowanie połączenia*</span><span class="sxs-lookup"><span data-stu-id="58c4e-392">*Validating connection*</span></span>
4. <span data-ttu-id="58c4e-393">Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="58c4e-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="58c4e-394">![Konfiguracja narzędzia Web Deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Konfiguracja narzędzia Web Deploy")</span><span class="sxs-lookup"><span data-stu-id="58c4e-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="58c4e-395">*Konfiguracja narzędzia Web Deploy*</span><span class="sxs-lookup"><span data-stu-id="58c4e-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="58c4e-396">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="58c4e-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="58c4e-397">W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="58c4e-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="58c4e-398">W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="58c4e-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="58c4e-399">W polu **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="58c4e-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="58c4e-400">Wpisz nową nazwę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58c4e-400">Type a new database name.</span></span>

     <span data-ttu-id="58c4e-401">![Konfigurowanie parametrów połączenia docelowego](aspnet-mvc-4-custom-action-filters/_static/image33.png "Konfigurowanie parametrów połączenia docelowego")</span><span class="sxs-lookup"><span data-stu-id="58c4e-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="58c4e-402">*Konfigurowanie parametrów połączenia docelowego*</span><span class="sxs-lookup"><span data-stu-id="58c4e-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="58c4e-403">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-403">Then click **OK**.</span></span> <span data-ttu-id="58c4e-404">Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="58c4e-405">![Tworzenie bazy danych](aspnet-mvc-4-custom-action-filters/_static/image34.png "Tworzenie ciągu bazy danych")</span><span class="sxs-lookup"><span data-stu-id="58c4e-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="58c4e-406">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="58c4e-406">*Creating the database*</span></span>
7. <span data-ttu-id="58c4e-407">Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie.</span><span class="sxs-lookup"><span data-stu-id="58c4e-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="58c4e-408">Następnie kliknij przycisk **Next** (Dalej).</span><span class="sxs-lookup"><span data-stu-id="58c4e-408">Then click **Next**.</span></span>

    <span data-ttu-id="58c4e-409">![Parametry połączenia wskazujące SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Parametry połączenia wskazujące SQL Database")</span><span class="sxs-lookup"><span data-stu-id="58c4e-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="58c4e-410">*Parametry połączenia wskazujące SQL Database*</span><span class="sxs-lookup"><span data-stu-id="58c4e-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="58c4e-411">Na stronie **Podgląd** kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="58c4e-412">![Publikowanie aplikacji sieci Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publikowanie aplikacji sieci Web")</span><span class="sxs-lookup"><span data-stu-id="58c4e-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="58c4e-413">*Publikowanie aplikacji sieci Web*</span><span class="sxs-lookup"><span data-stu-id="58c4e-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="58c4e-414">Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.</span><span class="sxs-lookup"><span data-stu-id="58c4e-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="58c4e-415">Dodatek C: używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="58c4e-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="58c4e-416">Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką.</span><span class="sxs-lookup"><span data-stu-id="58c4e-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="58c4e-417">Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="58c4e-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="58c4e-418">![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-custom-action-filters/_static/image37.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="58c4e-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="58c4e-419">*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="58c4e-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="58c4e-420">***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="58c4e-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="58c4e-421">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="58c4e-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="58c4e-422">Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).</span><span class="sxs-lookup"><span data-stu-id="58c4e-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="58c4e-423">Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="58c4e-424">Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).</span><span class="sxs-lookup"><span data-stu-id="58c4e-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="58c4e-425">Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="58c4e-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="58c4e-426">![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-custom-action-filters/_static/image38.png "Zacznij wpisywać nazwę fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="58c4e-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="58c4e-427">*Zacznij wpisywać nazwę fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="58c4e-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="58c4e-428">![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-custom-action-filters/_static/image39.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="58c4e-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="58c4e-429">*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="58c4e-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="58c4e-430">![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-custom-action-filters/_static/image40.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")</span><span class="sxs-lookup"><span data-stu-id="58c4e-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="58c4e-431">*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*</span><span class="sxs-lookup"><span data-stu-id="58c4e-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="58c4e-432">***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno.</span><span class="sxs-lookup"><span data-stu-id="58c4e-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="58c4e-433">Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="58c4e-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="58c4e-434">Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.</span><span class="sxs-lookup"><span data-stu-id="58c4e-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="58c4e-435">Wybierz odpowiedni fragment kodu z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="58c4e-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="58c4e-436">![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-custom-action-filters/_static/image41.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")</span><span class="sxs-lookup"><span data-stu-id="58c4e-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="58c4e-437">*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*</span><span class="sxs-lookup"><span data-stu-id="58c4e-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="58c4e-438">![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-custom-action-filters/_static/image42.png "Wybierz odpowiedni fragment kodu z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="58c4e-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="58c4e-439">*Wybierz odpowiedni fragment kodu z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="58c4e-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
