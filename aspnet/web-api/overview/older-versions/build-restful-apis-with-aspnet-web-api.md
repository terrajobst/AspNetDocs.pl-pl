---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Tworzenie interfejsów API RESTful za pomocą ASP.NET Web API-ASP.NET 4. x
author: rick-anderson
description: 'Wskazówki dotyczące laboratorium: Użyj interfejsu API sieci Web w ASP.NET 4. x, aby utworzyć prosty interfejs API REST dla aplikacji menedżera kontaktów.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621815"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="c0baf-103">Tworzenie interfejsów API RESTful za pomocą interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c0baf-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="c0baf-104">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c0baf-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="c0baf-105">Wskazówki dotyczące laboratorium: Użyj interfejsu API sieci Web w ASP.NET 4. x, aby utworzyć prosty interfejs API REST dla aplikacji menedżera kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c0baf-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="c0baf-106">Zostanie również skompilowany klient, aby korzystał z interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c0baf-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="c0baf-107">W ostatnich latach stało się jasne, że protokół HTTP nie tylko służy do obsługi stron HTML.</span><span class="sxs-lookup"><span data-stu-id="c0baf-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="c0baf-108">Jest to również zaawansowana platforma służąca do tworzenia interfejsów API sieci Web przy użyciu kilku czasowników (GET, POST itd.) oraz kilku prostych koncepcji, takich jak *identyfikatory URI* i *nagłówki*.</span><span class="sxs-lookup"><span data-stu-id="c0baf-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="c0baf-109">Interfejs API sieci Web ASP.NET to zestaw składników, które upraszczają programowanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0baf-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="c0baf-110">Ponieważ jest zbudowany w oparciu o środowisko uruchomieniowe ASP.NET MVC, interfejs API sieci Web automatycznie obsługuje szczegóły transportu niskiego poziomu protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0baf-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="c0baf-111">W tym samym czasie interfejs API sieci Web naturalnie uwidacznia model programowania HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0baf-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="c0baf-112">W rzeczywistości jednym celem interfejsu API sieci Web jest *nieabstrakcyjna w rzeczywistości* wartość http.</span><span class="sxs-lookup"><span data-stu-id="c0baf-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="c0baf-113">W efekcie interfejs API sieci Web jest elastyczny i łatwy do rozbudowania.</span><span class="sxs-lookup"><span data-stu-id="c0baf-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="c0baf-114">Styl architektury REST okazał się skutecznym sposobem korzystania z protokołu HTTP, chociaż nie jest to jedyne prawidłowe podejście do protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0baf-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="c0baf-115">Program Contact Manager uwidoczni RESTful do wyświetlania, dodawania i usuwania kontaktów między innymi.</span><span class="sxs-lookup"><span data-stu-id="c0baf-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="c0baf-116">To laboratorium wymaga podstawowej znajomości protokołu HTTP, REST i zakłada, że masz podstawową praktyczną wiedzę na temat języków HTML, JavaScript i jQuery.</span><span class="sxs-lookup"><span data-stu-id="c0baf-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="c0baf-117">Witryna sieci Web ASP.NET ma obszar dedykowany struktury internetowego interfejsu API ASP.NET w [https://asp.net/web-api](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="c0baf-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="c0baf-118">Ta lokacja będzie w dalszym ciągu dostarczać najświeższe informacje, przykłady i wiadomości związane z interfejsem API sieci Web, dlatego należy sprawdzać je często, jeśli chcesz uzyskać więcej informacji na temat tworzenia niestandardowych interfejsów API sieci Web dostępnych dla praktycznie dowolnego urządzenia lub platformy programistycznej.</span><span class="sxs-lookup"><span data-stu-id="c0baf-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="c0baf-119">Interfejs API sieci Web ASP.NET, podobny do ASP.NET MVC 4, ma doskonałą elastyczność pod względem rozdzielania warstwy usług od kontrolerów, co pozwala na bardzo proste użycie kilku dostępnych platform iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="c0baf-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="c0baf-120">W witrynie MSDN znajduje się dobry przykład, który pokazuje, jak używać Ninject do iniekcji zależności w projekcie interfejsu API sieci Web ASP.NET, który można pobrać z tego [miejsca](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="c0baf-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="c0baf-121">Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c0baf-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c0baf-122">Cele</span><span class="sxs-lookup"><span data-stu-id="c0baf-122">Objectives</span></span>

<span data-ttu-id="c0baf-123">W tym ćwiczeniu dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="c0baf-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="c0baf-124">Implementowanie interfejsu API sieci Web RESTful</span><span class="sxs-lookup"><span data-stu-id="c0baf-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="c0baf-125">Wywoływanie interfejsu API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="c0baf-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c0baf-126">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c0baf-126">Prerequisites</span></span>

<span data-ttu-id="c0baf-127">Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:</span><span class="sxs-lookup"><span data-stu-id="c0baf-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="c0baf-128">[Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek B](#AppendixB) , aby uzyskać instrukcje dotyczące sposobu ich instalacji).</span><span class="sxs-lookup"><span data-stu-id="c0baf-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c0baf-129">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="c0baf-129">Setup</span></span>

<span data-ttu-id="c0baf-130">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="c0baf-130">**Installing Code Snippets**</span></span>

<span data-ttu-id="c0baf-131">Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0baf-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c0baf-132">Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c0baf-133">Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatek A: używanie fragmentów kodu](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0baf-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c0baf-134">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="c0baf-134">Exercises</span></span>

<span data-ttu-id="c0baf-135">To laboratorium praktyczne obejmuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c0baf-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="c0baf-136">Ćwiczenie 1. Tworzenie interfejsu API sieci Web tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="c0baf-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="c0baf-137">Ćwiczenie 2: Tworzenie internetowego interfejsu API do odczytu/zapisu</span><span class="sxs-lookup"><span data-stu-id="c0baf-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="c0baf-138">Ćwiczenie 3: korzystanie z internetowego interfejsu API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="c0baf-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="c0baf-139">Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="c0baf-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c0baf-140">Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="c0baf-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="c0baf-141">Szacowany czas wykonywania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="c0baf-142">Ćwiczenie 1. Tworzenie interfejsu API sieci Web tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="c0baf-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="c0baf-143">W tym ćwiczeniu zostaną zaimplementowane metody pobierania tylko do odczytu dla Menedżera kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c0baf-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="c0baf-144">Zadanie 1 — Tworzenie projektu interfejsu API</span><span class="sxs-lookup"><span data-stu-id="c0baf-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="c0baf-145">W tym zadaniu zostanie użyty nowy szablon projektu sieci Web ASP.NET do utworzenia aplikacji internetowej interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0baf-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="c0baf-146">Uruchom **program Visual Studio 2012 Express for Web**, w tym celu przejdź do **menu Start** , a następnie wpisz **vs Express for Web** następnie naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="c0baf-147">Z menu **plik** wybierz pozycję **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="c0baf-148">Wybierz **wizualizację C# | Typ projektu sieci Web** z widoku drzewa typ projektu, a następnie wybierz typ projektu **aplikacji sieci Web ASP.NET MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="c0baf-149">Ustaw **nazwę** projektu na *ContactName* i **nazwę rozwiązania** , aby *rozpocząć*, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="c0baf-150">![Tworzenie nowego projektu aplikacji sieci Web ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Tworzenie nowego projektu aplikacji sieci Web ASP.NET MVC 4,0")</span><span class="sxs-lookup"><span data-stu-id="c0baf-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="c0baf-151">*Tworzenie nowego projektu aplikacji sieci Web ASP.NET MVC 4,0*</span><span class="sxs-lookup"><span data-stu-id="c0baf-151">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="c0baf-152">W oknie dialogowym ASP.NET MVC 4 typu projektu wybierz typ projektu **interfejsu API sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="c0baf-153">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-153">Click **OK**.</span></span>

    <span data-ttu-id="c0baf-154">![Określanie typu projektu interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Określanie typu projektu interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="c0baf-155">*Określanie typu projektu interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-155">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="c0baf-156">Zadanie 2 — Tworzenie kontrolerów interfejsu API Menedżera kontaktów</span><span class="sxs-lookup"><span data-stu-id="c0baf-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="c0baf-157">W tym zadaniu utworzysz klasy kontrolera, w których będą znajdować się metody interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c0baf-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="c0baf-158">Usuń plik o nazwie **ValuesController.cs** w folderze **controllers** z projektu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="c0baf-159">Kliknij prawym przyciskiem myszy folder **controllers** w projekcie i wybierz polecenie **Dodaj | Z menu** kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="c0baf-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="c0baf-160">![Dodawanie nowego kontrolera do projektu](build-restful-apis-with-aspnet-web-api/_static/image3.png "Dodawanie nowego kontrolera do projektu")</span><span class="sxs-lookup"><span data-stu-id="c0baf-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="c0baf-161">*Dodawanie nowego kontrolera do projektu*</span><span class="sxs-lookup"><span data-stu-id="c0baf-161">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="c0baf-162">W wyświetlonym oknie dialogowym **Dodaj kontroler** wybierz pozycję **pusty kontroler interfejsu API** z menu szablon.</span><span class="sxs-lookup"><span data-stu-id="c0baf-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="c0baf-163">Nadaj nazwę klasy kontrolera **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="c0baf-164">Następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="c0baf-164">Then, click **Add.**</span></span>

    <span data-ttu-id="c0baf-165">![Przy użyciu okna dialogowego Dodawanie kontrolera można utworzyć nowy kontroler internetowego interfejsu API](build-restful-apis-with-aspnet-web-api/_static/image4.png "Przy użyciu okna dialogowego Dodawanie kontrolera można utworzyć nowy kontroler internetowego interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="c0baf-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="c0baf-166">*Przy użyciu okna dialogowego Dodawanie kontrolera można utworzyć nowy kontroler internetowego interfejsu API*</span><span class="sxs-lookup"><span data-stu-id="c0baf-166">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="c0baf-167">Dodaj następujący kod do **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="c0baf-168">(Fragment kodu- *Web API Lab-Ex01-Get API Metoda*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="c0baf-169">Naciśnij klawisz **F5**, aby debugować aplikację.</span><span class="sxs-lookup"><span data-stu-id="c0baf-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="c0baf-170">Powinna zostać wyświetlona domyślna Strona główna projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0baf-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="c0baf-171">![Domyślna strona główna aplikacji internetowego interfejsu API ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "Domyślna strona główna aplikacji internetowego interfejsu API ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="c0baf-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="c0baf-172">*Domyślna strona główna aplikacji internetowego interfejsu API ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="c0baf-172">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="c0baf-173">W oknie programu Internet Explorer naciśnij klawisz **F12** , aby otworzyć okno **Narzędzia deweloperskie** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="c0baf-174">Kliknij kartę **Sieć** , a następnie kliknij przycisk **Rozpocznij przechwytywanie** , aby rozpocząć przechwytywanie ruchu sieciowego do okna.</span><span class="sxs-lookup"><span data-stu-id="c0baf-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="c0baf-175">![Otwieranie karty sieć i Inicjowanie przechwytywania sieciowego](build-restful-apis-with-aspnet-web-api/_static/image6.png "Otwieranie karty sieć i Inicjowanie przechwytywania sieciowego")</span><span class="sxs-lookup"><span data-stu-id="c0baf-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="c0baf-176">*Otwieranie karty sieć i Inicjowanie przechwytywania sieciowego*</span><span class="sxs-lookup"><span data-stu-id="c0baf-176">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="c0baf-177">Dołącz adres URL na pasku adresu przeglądarki z **/API/Contact** i naciśnij klawisz ENTER.</span><span class="sxs-lookup"><span data-stu-id="c0baf-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="c0baf-178">Szczegóły przesyłania zostaną wyświetlone w oknie przechwytywanie sieci.</span><span class="sxs-lookup"><span data-stu-id="c0baf-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="c0baf-179">Należy pamiętać, że typ MIME odpowiedzi to **Application/JSON**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="c0baf-180">Pokazuje, jak domyślny format danych wyjściowych to JSON.</span><span class="sxs-lookup"><span data-stu-id="c0baf-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="c0baf-181">![Wyświetlanie danych wyjściowych żądania internetowego interfejsu API w widoku sieci](build-restful-apis-with-aspnet-web-api/_static/image7.png "Wyświetlanie danych wyjściowych żądania internetowego interfejsu API w widoku sieci")</span><span class="sxs-lookup"><span data-stu-id="c0baf-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="c0baf-182">*Wyświetlanie danych wyjściowych żądania internetowego interfejsu API w widoku sieci*</span><span class="sxs-lookup"><span data-stu-id="c0baf-182">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0baf-183">Domyślne zachowanie programu Internet Explorer 10 będzie miało na celu zaproszenie użytkownika o zapisanie lub otwarcie strumienia pochodzącego z wywołania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0baf-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="c0baf-184">Dane wyjściowe będą plikami tekstowymi zawierającymi wynik JSON wywołania adresu URL interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0baf-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="c0baf-185">Nie Anuluj okna dialogowego, aby zobaczyć zawartość odpowiedzi za pomocą okna narzędzia deweloperzy.</span><span class="sxs-lookup"><span data-stu-id="c0baf-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="c0baf-186">Kliknij przycisk **Przejdź do widoku szczegółowego** , aby zobaczyć więcej szczegółów na temat odpowiedzi tego wywołania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c0baf-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="c0baf-187">![Przełącz do widoku szczegółowego](build-restful-apis-with-aspnet-web-api/_static/image8.png "Przełącz do widoku szczegółów")</span><span class="sxs-lookup"><span data-stu-id="c0baf-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="c0baf-188">*Przełącz do widoku szczegółowego*</span><span class="sxs-lookup"><span data-stu-id="c0baf-188">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="c0baf-189">Kliknij kartę **treść odpowiedzi** , aby wyświetlić rzeczywisty tekst odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="c0baf-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="c0baf-190">![Wyświetlanie tekstu wyjściowego JSON w monitorze sieci](build-restful-apis-with-aspnet-web-api/_static/image9.png "Wyświetlanie tekstu wyjściowego JSON w monitorze sieci")</span><span class="sxs-lookup"><span data-stu-id="c0baf-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="c0baf-191">*Wyświetlanie tekstu wyjściowego JSON w monitorze sieci*</span><span class="sxs-lookup"><span data-stu-id="c0baf-191">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="c0baf-192">Zadanie 3 — Tworzenie modeli kontaktów i rozszerzanie kontrolera kontaktów</span><span class="sxs-lookup"><span data-stu-id="c0baf-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="c0baf-193">W tym zadaniu utworzysz klasy kontrolera, w których będą znajdować się metody interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c0baf-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="c0baf-194">Kliknij prawym przyciskiem myszy folder **modele** i wybierz polecenie **Dodaj | Klasa...** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="c0baf-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="c0baf-195">![Dodawanie nowego modelu do aplikacji sieci Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Dodawanie nowego modelu do aplikacji sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="c0baf-196">*Dodawanie nowego modelu do aplikacji sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-196">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="c0baf-197">W oknie dialogowym **Dodaj nowy element** Nazwij nowy plik **Contact.cs** a następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="c0baf-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="c0baf-198">![Tworzenie nowego pliku klasy kontaktu](build-restful-apis-with-aspnet-web-api/_static/image11.png "Tworzenie nowego pliku klasy kontaktu")</span><span class="sxs-lookup"><span data-stu-id="c0baf-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="c0baf-199">*Tworzenie nowego pliku klasy kontaktu*</span><span class="sxs-lookup"><span data-stu-id="c0baf-199">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="c0baf-200">Dodaj następujący wyróżniony kod do klasy **Contact** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="c0baf-201">(Fragment kodu- *Web API Lab-Ex01-Contact Class*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="c0baf-202">W klasie **ContactController** wybierz **ciąg** wyrazu w definicji metody metody **Get** , a następnie wpisz słowo *Contact*.</span><span class="sxs-lookup"><span data-stu-id="c0baf-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="c0baf-203">Po wpisaniu wyrazu w programie wskaźnik pojawi się na początku **kontaktu z**programem Word.</span><span class="sxs-lookup"><span data-stu-id="c0baf-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="c0baf-204">Przytrzymaj wciśnięty klawisz **Ctrl** i naciśnij klawisz kropki (.) lub kliknij ikonę przy użyciu myszy, aby otworzyć okno dialogowe pomocy w edytorze kodu, aby automatycznie wypełnić dyrektywę **using** dla przestrzeni nazw models.</span><span class="sxs-lookup"><span data-stu-id="c0baf-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Korzystanie z pomocy IntelliSense dla deklaracji przestrzeni nazw](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="c0baf-206">*Korzystanie z pomocy IntelliSense dla deklaracji przestrzeni nazw*</span><span class="sxs-lookup"><span data-stu-id="c0baf-206">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="c0baf-207">Zmodyfikuj kod metody **Get** , aby zwracała tablicę wystąpień modelu kontaktu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="c0baf-208">(Fragment kodu — *Web API Lab-Ex01 — zwracanie listy kontaktów*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="c0baf-209">Naciśnij klawisz **F5** , aby debugować aplikację sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c0baf-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="c0baf-210">Aby wyświetlić zmiany wprowadzone do danych wyjściowych odpowiedzi interfejsu API, wykonaj następujące czynności.</span><span class="sxs-lookup"><span data-stu-id="c0baf-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="c0baf-211">Po otwarciu przeglądarki naciśnij klawisz **F12** , jeśli narzędzia deweloperskie nie są jeszcze otwarte.</span><span class="sxs-lookup"><span data-stu-id="c0baf-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="c0baf-212">Kliknij kartę **Sieć** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="c0baf-213">Naciśnij przycisk **Rozpocznij przechwytywanie** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="c0baf-214">Dodaj sufiks adresu URL **/API/Contact** do adresu URL na pasku adresu i naciśnij klawisz **Enter** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="c0baf-215">Naciśnij przycisk **Przejdź do widoku szczegółowego** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="c0baf-216">Wybierz kartę **treść odpowiedzi** . Powinien zostać wyświetlony ciąg JSON reprezentujący serializowaną postać tablicy wystąpień kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c0baf-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="c0baf-217">![Serializowane dane wyjściowe JSON złożonego wywołania metody interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image13.png "Serializowane dane wyjściowe JSON złożonego wywołania metody interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="c0baf-218">*Serializowane dane wyjściowe JSON złożonego wywołania metody interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-218">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="c0baf-219">Zadanie 4 — wyodrębnianie funkcji do warstwy usług</span><span class="sxs-lookup"><span data-stu-id="c0baf-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="c0baf-220">To zadanie pokazuje, jak wyodrębnić funkcjonalność do warstwy usług, aby ułatwić deweloperom oddzielenie ich funkcjonalności usługi od warstwy kontrolera, co pozwala na wielokrotne wykorzystywanie usług, które faktycznie wykonują prace.</span><span class="sxs-lookup"><span data-stu-id="c0baf-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="c0baf-221">Utwórz nowy folder w katalogu głównym rozwiązania i nadaj mu nazwę **usługi**IT.</span><span class="sxs-lookup"><span data-stu-id="c0baf-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="c0baf-222">W tym celu kliknij prawym przyciskiem myszy pozycję **ContactManager** Project, wybierz pozycję **Dodaj** | **Nowy folder**, nadaj nazwę *usługom*IT.</span><span class="sxs-lookup"><span data-stu-id="c0baf-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="c0baf-223">![Tworzenie folderu usług](build-restful-apis-with-aspnet-web-api/_static/image14.png "Tworzenie folderu usług")</span><span class="sxs-lookup"><span data-stu-id="c0baf-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="c0baf-224">*Tworzenie folderu usług*</span><span class="sxs-lookup"><span data-stu-id="c0baf-224">*Creating Services folder*</span></span>
2. <span data-ttu-id="c0baf-225">Kliknij prawym przyciskiem myszy folder **usługi** i wybierz polecenie **Dodaj | Klasa...** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="c0baf-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="c0baf-226">![Dodawanie nowej klasy do folderu Services](build-restful-apis-with-aspnet-web-api/_static/image15.png "Dodawanie nowej klasy do folderu Services")</span><span class="sxs-lookup"><span data-stu-id="c0baf-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="c0baf-227">*Dodawanie nowej klasy do folderu Services*</span><span class="sxs-lookup"><span data-stu-id="c0baf-227">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="c0baf-228">Gdy pojawi się okno dialogowe **Dodaj nowy element** , nazwij nową klasę **ContactRepository** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="c0baf-229">![Tworzenie pliku klasy zawierającego kod dla warstwy usługi repozytorium kontaktów](build-restful-apis-with-aspnet-web-api/_static/image16.png "Tworzenie pliku klasy zawierającego kod dla warstwy usługi repozytorium kontaktów")</span><span class="sxs-lookup"><span data-stu-id="c0baf-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="c0baf-230">*Tworzenie pliku klasy zawierającego kod dla warstwy usługi repozytorium kontaktów*</span><span class="sxs-lookup"><span data-stu-id="c0baf-230">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="c0baf-231">Dodaj dyrektywę using do pliku **ContactRepository.cs** , aby uwzględnić przestrzeń nazw models.</span><span class="sxs-lookup"><span data-stu-id="c0baf-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="c0baf-232">Dodaj następujący wyróżniony kod do pliku **ContactRepository.cs** , aby zaimplementować metodę GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="c0baf-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="c0baf-233">(Fragment kodu — *Web API Lab-Ex01-contacting Repository*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="c0baf-234">Otwórz plik **ContactController.cs** , jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="c0baf-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="c0baf-235">Dodaj następującą instrukcję using do sekcji deklaracji przestrzeni nazw pliku.</span><span class="sxs-lookup"><span data-stu-id="c0baf-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="c0baf-236">Dodaj następujący wyróżniony kod do klasy **ContactController.cs** , aby dodać pole prywatne do reprezentowania wystąpienia repozytorium, dzięki czemu pozostałe elementy członkowskie klasy mogą korzystać z implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="c0baf-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="c0baf-237">(Fragment kodu — *Web API Lab-Ex01-Contact Controller*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="c0baf-238">Zmień metodę **Get** tak, aby korzystała z usługi repozytorium kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c0baf-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="c0baf-239">(Fragment kodu — *Web API Lab-Ex01 — zwraca listę kontaktów za pośrednictwem repozytorium*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="c0baf-240">Umieść punkt przerwania w definicji metody **Get** **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="c0baf-241">![Dodawanie punktów przerwania do kontrolera kontaktów](build-restful-apis-with-aspnet-web-api/_static/image17.png "Dodawanie punktów przerwania do kontrolera kontaktów")</span><span class="sxs-lookup"><span data-stu-id="c0baf-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="c0baf-242">*Dodawanie punktów przerwania do kontrolera kontaktów*</span><span class="sxs-lookup"><span data-stu-id="c0baf-242">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="c0baf-243">Naciśnij klawisz **F5**, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="c0baf-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="c0baf-244">Po otwarciu przeglądarki naciśnij klawisz **F12** , aby otworzyć narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="c0baf-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="c0baf-245">Kliknij kartę **Sieć** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="c0baf-246">Kliknij przycisk **Rozpocznij przechwytywanie** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="c0baf-247">Dołącz adres URL na pasku adresu przy użyciu sufiksu **/API/Contact** i naciśnij klawisz **Enter** , aby załadować kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c0baf-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="c0baf-248">Program Visual Studio 2012 powinien zostać przerwany po rozpoczęciu wykonywania metody **Get** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="c0baf-249">![Przerywanie w ramach metody get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Przerywanie w ramach metody get")</span><span class="sxs-lookup"><span data-stu-id="c0baf-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="c0baf-250">*Przerywanie w ramach metody get*</span><span class="sxs-lookup"><span data-stu-id="c0baf-250">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="c0baf-251">Naciśnij klawisz **F5**, aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="c0baf-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="c0baf-252">Wróć do programu Internet Explorer, jeśli nie jest jeszcze fokusem.</span><span class="sxs-lookup"><span data-stu-id="c0baf-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="c0baf-253">Zanotuj okno Przechwytywanie sieci.</span><span class="sxs-lookup"><span data-stu-id="c0baf-253">Note the network capture window.</span></span>

    <span data-ttu-id="c0baf-254">![Widok sieci w programie Internet Explorer przedstawiający wyniki wywołania interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Widok sieci w programie Internet Explorer przedstawiający wyniki wywołania interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="c0baf-255">*Widok sieci w programie Internet Explorer przedstawiający wyniki wywołania interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-255">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="c0baf-256">Kliknij przycisk **Przejdź do widoku szczegółowego** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="c0baf-257">Kliknij kartę **treść odpowiedzi** . Zanotuj dane wyjściowe JSON wywołania interfejsu API i sposób przedstawiający dwa kontakty pobrane przez warstwę usług.</span><span class="sxs-lookup"><span data-stu-id="c0baf-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="c0baf-258">![Wyświetlanie danych wyjściowych JSON z internetowego interfejsu API w oknie narzędzia deweloperskie](build-restful-apis-with-aspnet-web-api/_static/image20.png "Wyświetlanie danych wyjściowych JSON z internetowego interfejsu API w oknie narzędzia deweloperskie")</span><span class="sxs-lookup"><span data-stu-id="c0baf-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="c0baf-259">*Wyświetlanie danych wyjściowych JSON z internetowego interfejsu API w oknie narzędzia deweloperskie*</span><span class="sxs-lookup"><span data-stu-id="c0baf-259">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="c0baf-260">Ćwiczenie 2: Tworzenie internetowego interfejsu API do odczytu/zapisu</span><span class="sxs-lookup"><span data-stu-id="c0baf-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="c0baf-261">W tym ćwiczeniu zostaną zaimplementowane metody POST i PUT dla Menedżera kontaktów, aby umożliwić korzystanie z funkcji edytowania danych.</span><span class="sxs-lookup"><span data-stu-id="c0baf-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="c0baf-262">Zadanie 1 — Otwieranie projektu interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="c0baf-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="c0baf-263">W tym zadaniu zostanie przygotowana do udoskonalenia projektu internetowego interfejsu API utworzonego w ćwiczeniu 1, aby mógł akceptować dane wprowadzane przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c0baf-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="c0baf-264">Uruchom **program Visual Studio 2012 Express for Web**, w tym celu przejdź do **menu Start** , a następnie wpisz **vs Express for Web** następnie naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="c0baf-265">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex02-ReadWriteWebAPI/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="c0baf-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="c0baf-266">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="c0baf-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c0baf-267">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="c0baf-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c0baf-268">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c0baf-269">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="c0baf-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c0baf-270">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c0baf-271">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c0baf-272">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c0baf-273">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="c0baf-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="c0baf-274">Otwórz plik **Services/ContactRepository. cs** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="c0baf-275">Zadanie 2 — Dodawanie funkcji trwałości danych do implementacji repozytorium kontaktów</span><span class="sxs-lookup"><span data-stu-id="c0baf-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="c0baf-276">W tym zadaniu zostanie zaakceptowana Klasa ContactRepository projektu internetowego interfejsu API utworzona w ćwiczeniu 1, dzięki czemu może ona utrzymywać i akceptować dane wejściowe użytkownika i nowe wystąpienia kontaktów.</span><span class="sxs-lookup"><span data-stu-id="c0baf-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="c0baf-277">Dodaj następującą stałą do klasy **ContactRepository** , aby reprezentować nazwę klucza elementu pamięci podręcznej serwera sieci Web w dalszej części tego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="c0baf-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="c0baf-278">Dodaj konstruktora do **ContactRepository** zawierającego Poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="c0baf-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="c0baf-279">(Fragment kodu — *Web API Lab-Ex02-contacting Konstruktor repozytorium*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="c0baf-280">Zmodyfikuj kod metody **GetAllContacts** , jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="c0baf-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="c0baf-281">(Fragment kodu- *Web API Lab-Ex02-Get All Contacts*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="c0baf-282">Ten przykład jest przeznaczony dla celów demonstracyjnych i będzie używać pamięci podręcznej serwera sieci Web jako nośnika magazynu, dzięki czemu wartości będą dostępne jednocześnie dla wielu klientów, a nie przy użyciu mechanizmu magazynu sesji lub okresu istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="c0baf-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="c0baf-283">Jedna z nich może korzystać z Entity Framework, magazynu XML lub dowolnej innej odmiany zamiast pamięci podręcznej serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0baf-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="c0baf-284">Zaimplementuj nową metodę o nazwie **SaveContact** z klasą **ContactRepository** , aby wykonać zadania zapisywania kontaktu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="c0baf-285">Metoda **SaveContact** powinna przyjmować pojedynczy parametr **kontaktu** i zwracać wartość logiczną wskazującą powodzenie lub niepowodzenie.</span><span class="sxs-lookup"><span data-stu-id="c0baf-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="c0baf-286">(Fragment kodu — *Web API Lab-Ex02-implementująca metodę SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="c0baf-287">Ćwiczenie 3: korzystanie z internetowego interfejsu API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="c0baf-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="c0baf-288">W tym ćwiczeniu utworzysz klienta HTML, który wywoła internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="c0baf-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="c0baf-289">Ten klient ułatwi wymianę danych z interfejsem API sieci Web przy użyciu języka JavaScript i wyświetla wyniki w przeglądarce internetowej przy użyciu znacznika HTML.</span><span class="sxs-lookup"><span data-stu-id="c0baf-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="c0baf-290">Zadanie 1 — Modyfikowanie widoku indeksu w celu zapewnienia graficznego interfejsu użytkownika do wyświetlania kontaktów</span><span class="sxs-lookup"><span data-stu-id="c0baf-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="c0baf-291">W tym zadaniu zmodyfikujesz domyślny widok indeksu aplikacji sieci Web, który będzie obsługiwał wymaganie wyświetlania listy istniejących kontaktów w przeglądarce HTML.</span><span class="sxs-lookup"><span data-stu-id="c0baf-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="c0baf-292">Otwórz **program Visual Studio 2012 Express for Web** , jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="c0baf-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="c0baf-293">Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex03-ConsumingWebAPI/BEGIN/** folder.</span><span class="sxs-lookup"><span data-stu-id="c0baf-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="c0baf-294">W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="c0baf-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c0baf-295">Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="c0baf-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c0baf-296">W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c0baf-297">W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="c0baf-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c0baf-298">Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c0baf-299">Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c0baf-300">Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c0baf-301">To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="c0baf-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="c0baf-302">Otwórz plik **index. cshtml** znajdujący się w folderze **widoki/główne** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="c0baf-303">Zastąp kod HTML w elemencie DIV identyfikatorem **treści** , tak aby wyglądał jak poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="c0baf-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="c0baf-304">Dodaj następujący kod JavaScript w dolnej części pliku, aby wykonać żądanie HTTP do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c0baf-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="c0baf-305">Otwórz plik **ContactController.cs** , jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="c0baf-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="c0baf-306">Umieść punkt przerwania w metodzie **Get** klasy **ContactController** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="c0baf-307">![Umieszczanie punktu przerwania w metodzie GET kontrolera interfejsu API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Umieszczanie punktu przerwania w metodzie GET kontrolera interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="c0baf-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="c0baf-308">*Umieszczanie punktu przerwania w metodzie GET kontrolera interfejsu API*</span><span class="sxs-lookup"><span data-stu-id="c0baf-308">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="c0baf-309">Naciśnij klawisz **F5** , aby uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="c0baf-309">Press **F5** to run the project.</span></span> <span data-ttu-id="c0baf-310">Przeglądarka załaduje dokument HTML.</span><span class="sxs-lookup"><span data-stu-id="c0baf-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0baf-311">Upewnij się, że przeglądasz adres URL katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c0baf-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="c0baf-312">Po załadowaniu strony i wykonaniu kodu JavaScript zostanie osiągnięty punkt przerwania, a wykonywanie kodu zostanie wstrzymane na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="c0baf-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="c0baf-313">![Debugowanie do wywołań interfejsu API sieci Web przy użyciu VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugowanie do wywołań interfejsu API sieci Web przy użyciu VS Express for Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="c0baf-314">*Debugowanie do wywołania interfejsu API sieci Web za pomocą programu Visual Studio 2012 Express for Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-314">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="c0baf-315">Usuń punkt przerwania i naciśnij klawisz **F5** lub przycisk **Kontynuuj** debugowania paska narzędzi, aby kontynuować ładowanie widoku w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c0baf-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="c0baf-316">Po zakończeniu wywołania interfejsu API sieci Web powinny zostać wyświetlone kontakty zwrócone z wywołania interfejsu API sieci Web jako elementy listy w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c0baf-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="c0baf-317">![Wyniki wywołania interfejsu API wyświetlane w przeglądarce jako elementy listy](build-restful-apis-with-aspnet-web-api/_static/image23.png "Wyniki wywołania interfejsu API wyświetlane w przeglądarce jako elementy listy")</span><span class="sxs-lookup"><span data-stu-id="c0baf-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="c0baf-318">*Wyniki wywołania interfejsu API wyświetlane w przeglądarce jako elementy listy*</span><span class="sxs-lookup"><span data-stu-id="c0baf-318">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="c0baf-319">Zatrzymaj debugowanie.</span><span class="sxs-lookup"><span data-stu-id="c0baf-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="c0baf-320">Zadanie 2 — Modyfikowanie widoku indeksu w celu zapewnienia graficznego interfejsu użytkownika do tworzenia kontaktów</span><span class="sxs-lookup"><span data-stu-id="c0baf-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="c0baf-321">W tym zadaniu będziesz w dalszym ciągu modyfikować widok indeksu aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="c0baf-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="c0baf-322">Do strony HTML zostanie dodany formularz, który będzie przechwytywać dane wejściowe użytkownika i wysyłany do internetowego interfejsu API w celu utworzenia nowego kontaktu, a nowa metoda kontrolera internetowego interfejsu API zostanie utworzona w celu zebrania daty z graficznego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c0baf-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="c0baf-323">Otwórz plik **ContactController.cs** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="c0baf-324">Dodaj nową metodę do klasy kontrolera o nazwie **post** , jak pokazano w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="c0baf-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="c0baf-325">(Fragment kodu- *Web API Lab-Ex03-post-Metoda*)</span><span class="sxs-lookup"><span data-stu-id="c0baf-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="c0baf-326">Otwórz plik **index. cshtml** w programie Visual Studio, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="c0baf-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="c0baf-327">Dodaj poniższy kod HTML do pliku tuż po liście nieuporządkowanej, która została dodana w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="c0baf-328">W elemencie skryptu w dolnej części dokumentu Dodaj następujący wyróżniony kod, aby obsłużyć zdarzenia klikania przycisku, co spowoduje opublikowanie danych w interfejsie API sieci Web przy użyciu wywołania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="c0baf-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="c0baf-329">W **ContactController.cs**Umieść punkt przerwania w metodzie **post** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="c0baf-330">Naciśnij klawisz **F5** , aby uruchomić aplikację w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c0baf-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="c0baf-331">Po załadowaniu strony w przeglądarce wpisz nową nazwę kontaktu i identyfikator, a następnie kliknij przycisk **Zapisz** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="c0baf-332">![Dokument HTML klienta załadowany w przeglądarce](build-restful-apis-with-aspnet-web-api/_static/image24.png "Dokument HTML klienta załadowany w przeglądarce")</span><span class="sxs-lookup"><span data-stu-id="c0baf-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="c0baf-333">*Dokument HTML klienta załadowany w przeglądarce*</span><span class="sxs-lookup"><span data-stu-id="c0baf-333">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="c0baf-334">Gdy okno debugera jest zerwane w metodzie **post** , zapoznaj się z właściwościami parametru **Contact** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="c0baf-335">Wartości powinny być zgodne z danymi wprowadzonymi w formularzu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="c0baf-336">![Obiekt Contact wysyłany do internetowego interfejsu API z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "Obiekt Contact wysyłany do internetowego interfejsu API z klienta")</span><span class="sxs-lookup"><span data-stu-id="c0baf-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="c0baf-337">*Obiekt Contact wysyłany do internetowego interfejsu API z klienta*</span><span class="sxs-lookup"><span data-stu-id="c0baf-337">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="c0baf-338">Przechodzenie przez metodę w debugerze do momentu utworzenia zmiennej **odpowiedzi** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="c0baf-339">Po inspekcji w oknie **zmiennych lokalnych** w debugerze zobaczysz, że wszystkie właściwości zostały ustawione.</span><span class="sxs-lookup"><span data-stu-id="c0baf-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="c0baf-340">![Odpowiedź następująca po utworzeniu w debugerze](build-restful-apis-with-aspnet-web-api/_static/image26.png "Odpowiedź następująca po utworzeniu w debugerze")</span><span class="sxs-lookup"><span data-stu-id="c0baf-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="c0baf-341">*Odpowiedź następująca po utworzeniu w debugerze*</span><span class="sxs-lookup"><span data-stu-id="c0baf-341">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="c0baf-342">Jeśli naciśniesz klawisz **F5** lub klikniesz przycisk **Kontynuuj** w debugerze, żądanie zostanie ukończone.</span><span class="sxs-lookup"><span data-stu-id="c0baf-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="c0baf-343">Po ponownym przełączeniu do przeglądarki nowy kontakt został dodany do listy kontaktów przechowywanych przez implementację **ContactRepository** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="c0baf-344">![Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu](build-restful-apis-with-aspnet-web-api/_static/image27.png "Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu")</span><span class="sxs-lookup"><span data-stu-id="c0baf-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="c0baf-345">*Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu*</span><span class="sxs-lookup"><span data-stu-id="c0baf-345">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="c0baf-346">Ponadto możesz wdrożyć tę aplikację na platformie Azure, postępując zgodnie z [dodatkiem C: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="c0baf-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c0baf-347">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="c0baf-347">Summary</span></span>

<span data-ttu-id="c0baf-348">W tym laboratorium wprowadzono nową strukturę interfejsu API sieci Web ASP.NET oraz implementację interfejsów API sieci Web RESTful przy użyciu struktury.</span><span class="sxs-lookup"><span data-stu-id="c0baf-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="c0baf-349">W tym miejscu można utworzyć nowe repozytorium, które ułatwia trwałość danych przy użyciu dowolnej liczby mechanizmów i drutu, a nie prostego, dostarczonego jako przykład w tym laboratorium.</span><span class="sxs-lookup"><span data-stu-id="c0baf-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="c0baf-350">Interfejs API sieci Web obsługuje wiele dodatkowych funkcji, takich jak Włączanie komunikacji od klientów innych niż HTML pisanych w dowolnym języku, który obsługuje protokoły HTTP i JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="c0baf-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="c0baf-351">Możliwe jest również hostowanie internetowego interfejsu API poza typową aplikacją sieci Web, a także możliwość tworzenia własnych formatów serializacji.</span><span class="sxs-lookup"><span data-stu-id="c0baf-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="c0baf-352">Witryna sieci Web ASP.NET ma obszar dedykowany struktury internetowego interfejsu API ASP.NET w [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="c0baf-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="c0baf-353">Ta lokacja będzie w dalszym ciągu dostarczać najświeższe informacje, przykłady i wiadomości związane z interfejsem API sieci Web, dlatego należy sprawdzać je często, jeśli chcesz uzyskać więcej informacji na temat tworzenia niestandardowych interfejsów API sieci Web dostępnych dla praktycznie dowolnego urządzenia lub platformy programistycznej.</span><span class="sxs-lookup"><span data-stu-id="c0baf-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="c0baf-354">Dodatek A: używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="c0baf-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="c0baf-355">Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką.</span><span class="sxs-lookup"><span data-stu-id="c0baf-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c0baf-356">Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="c0baf-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c0baf-357">![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](build-restful-apis-with-aspnet-web-api/_static/image28.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="c0baf-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c0baf-358">*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="c0baf-358">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="c0baf-359">Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)</span><span class="sxs-lookup"><span data-stu-id="c0baf-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="c0baf-360">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="c0baf-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c0baf-361">Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).</span><span class="sxs-lookup"><span data-stu-id="c0baf-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c0baf-362">Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c0baf-363">Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).</span><span class="sxs-lookup"><span data-stu-id="c0baf-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c0baf-364">Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="c0baf-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="c0baf-365">![Zacznij wpisywać nazwę fragmentu kodu](build-restful-apis-with-aspnet-web-api/_static/image29.png "Zacznij wpisywać nazwę fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="c0baf-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="c0baf-366">*Zacznij wpisywać nazwę fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="c0baf-366">*Start typing the snippet name*</span></span>

    <span data-ttu-id="c0baf-367">![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](build-restful-apis-with-aspnet-web-api/_static/image30.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="c0baf-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="c0baf-368">*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="c0baf-368">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="c0baf-369">![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](build-restful-apis-with-aspnet-web-api/_static/image31.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")</span><span class="sxs-lookup"><span data-stu-id="c0baf-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="c0baf-370">*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*</span><span class="sxs-lookup"><span data-stu-id="c0baf-370">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="c0baf-371">Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)</span><span class="sxs-lookup"><span data-stu-id="c0baf-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="c0baf-372">Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="c0baf-373">Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="c0baf-374">Wybierz odpowiedni fragment kodu z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="c0baf-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="c0baf-375">![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](build-restful-apis-with-aspnet-web-api/_static/image32.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")</span><span class="sxs-lookup"><span data-stu-id="c0baf-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="c0baf-376">*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*</span><span class="sxs-lookup"><span data-stu-id="c0baf-376">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="c0baf-377">![Wybierz odpowiedni fragment kodu z listy, klikając go](build-restful-apis-with-aspnet-web-api/_static/image33.png "Wybierz odpowiedni fragment kodu z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="c0baf-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="c0baf-378">*Wybierz odpowiedni fragment kodu z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="c0baf-378">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c0baf-379">Dodatek B: Instalowanie Visual Studio Express 2012 dla sieci Web</span><span class="sxs-lookup"><span data-stu-id="c0baf-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c0baf-380">Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c0baf-381">Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.</span><span class="sxs-lookup"><span data-stu-id="c0baf-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c0baf-382">Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c0baf-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c0baf-383">Alternatywnie, jeśli wcześniej zainstalowano Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0baf-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c0baf-384">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-384">Click on **Install Now**.</span></span> <span data-ttu-id="c0baf-385">Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="c0baf-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c0baf-386">Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.</span><span class="sxs-lookup"><span data-stu-id="c0baf-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c0baf-387">![Zainstaluj Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Zainstaluj Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c0baf-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c0baf-388">*Zainstaluj Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c0baf-388">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c0baf-389">Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="c0baf-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie postanowień licencyjnych](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="c0baf-391">*Akceptowanie postanowień licencyjnych*</span><span class="sxs-lookup"><span data-stu-id="c0baf-391">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c0baf-392">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="c0baf-392">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="c0baf-394">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="c0baf-394">*Installation progress*</span></span>
6. <span data-ttu-id="c0baf-395">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-395">When the installation completes, click **Finish**.</span></span>

    ![Instalacja zakończona](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="c0baf-397">*Instalacja zakończona*</span><span class="sxs-lookup"><span data-stu-id="c0baf-397">*Installation completed*</span></span>
7. <span data-ttu-id="c0baf-398">Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0baf-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c0baf-399">Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Kafelek VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="c0baf-401">*Kafelek VS Express for Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-401">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c0baf-402">Dodatek C: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c0baf-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c0baf-403">W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web w witrynie Azure Portal i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy oferowanej przez platformę Azure.</span><span class="sxs-lookup"><span data-stu-id="c0baf-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="c0baf-404">Zadanie 1 — Tworzenie nowej witryny sieci Web w witrynie Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c0baf-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="c0baf-405">Przejdź do [usługi Azure Portal zarządzania](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="c0baf-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0baf-406">Dzięki platformie Azure możesz bezpłatnie hostować 10 ASP.NET witryn sieci Web, a następnie skalować je w miarę wzrostu ruchu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c0baf-407">Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c0baf-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c0baf-408">![Zaloguj się do systemu Windows Azure Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Zaloguj się do systemu Windows Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="c0baf-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="c0baf-409">*Zaloguj się do portalu*</span><span class="sxs-lookup"><span data-stu-id="c0baf-409">*Log on to Portal*</span></span>
2. <span data-ttu-id="c0baf-410">Na pasku poleceń kliknij pozycję **Nowy** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c0baf-411">![Tworzenie nowej witryny sieci Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "Tworzenie nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="c0baf-412">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-412">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="c0baf-413">Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c0baf-414">Następnie wybierz opcję **szybkie tworzenie** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="c0baf-415">Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0baf-416">Platforma Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią.</span><span class="sxs-lookup"><span data-stu-id="c0baf-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c0baf-417">Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web na platformie Azure spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="c0baf-418">Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0baf-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c0baf-419">![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](build-restful-apis-with-aspnet-web-api/_static/image41.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")</span><span class="sxs-lookup"><span data-stu-id="c0baf-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="c0baf-420">*Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*</span><span class="sxs-lookup"><span data-stu-id="c0baf-420">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="c0baf-421">Poczekaj na utworzenie nowej **witryny sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c0baf-422">Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c0baf-423">Sprawdź, czy nowa witryna sieci Web działa.</span><span class="sxs-lookup"><span data-stu-id="c0baf-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c0baf-424">![Przeglądanie w nowej witrynie sieci Web](build-restful-apis-with-aspnet-web-api/_static/image42.png "Przeglądanie w nowej witrynie sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="c0baf-425">*Przeglądanie w nowej witrynie sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-425">*Browsing to the new web site*</span></span>

    <span data-ttu-id="c0baf-426">![Uruchomiona witryna sieci Web](build-restful-apis-with-aspnet-web-api/_static/image43.png "Uruchomiona witryna sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="c0baf-427">*Uruchomiona witryna sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-427">*Web site running*</span></span>
6. <span data-ttu-id="c0baf-428">Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="c0baf-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c0baf-429">![Otwieranie stron zarządzania witryną sieci Web](build-restful-apis-with-aspnet-web-api/_static/image44.png "Otwieranie stron zarządzania witryną sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="c0baf-430">*Otwieranie stron zarządzania witryną sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-430">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="c0baf-431">Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .</span><span class="sxs-lookup"><span data-stu-id="c0baf-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0baf-432">*Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web na platformie Azure dla każdej z włączonych metod publikacji.</span><span class="sxs-lookup"><span data-stu-id="c0baf-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="c0baf-433">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="c0baf-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c0baf-434">**Firma Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów na potrzeby publikowania aplikacji sieci Web na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="c0baf-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="c0baf-435">![Pobieranie profilu publikowania witryny sieci Web](build-restful-apis-with-aspnet-web-api/_static/image45.png "Pobieranie profilu publikowania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="c0baf-436">*Pobieranie profilu publikowania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-436">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="c0baf-437">Pobierz plik profilu publikowania do znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="c0baf-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c0baf-438">W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web na platformie Azure z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0baf-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="c0baf-439">![Zapisywanie pliku profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image46.png "Zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="c0baf-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="c0baf-440">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="c0baf-440">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c0baf-441">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="c0baf-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c0baf-442">Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer.</span><span class="sxs-lookup"><span data-stu-id="c0baf-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c0baf-443">Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="c0baf-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c0baf-444">Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c0baf-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c0baf-445">Możesz wyświetlić SQL Database serwery z subskrypcji w portalu zarządzania Azure w obszarze **bazy danych SQL** | **serwery** | **pulpicie nawigacyjnym serwera**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c0baf-446">Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="c0baf-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c0baf-447">Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach.</span><span class="sxs-lookup"><span data-stu-id="c0baf-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c0baf-448">Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.</span><span class="sxs-lookup"><span data-stu-id="c0baf-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c0baf-449">![Pulpit nawigacyjny serwera SQL Database](build-restful-apis-with-aspnet-web-api/_static/image47.png "Pulpit nawigacyjny serwera SQL Database")</span><span class="sxs-lookup"><span data-stu-id="c0baf-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="c0baf-450">*Pulpit nawigacyjny serwera SQL Database*</span><span class="sxs-lookup"><span data-stu-id="c0baf-450">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="c0baf-451">W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera.</span><span class="sxs-lookup"><span data-stu-id="c0baf-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c0baf-452">W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](build-restful-apis-with-aspnet-web-api/_static/image48.png).</span><span class="sxs-lookup"><span data-stu-id="c0baf-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Dodawanie adresu IP klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="c0baf-454">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="c0baf-454">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="c0baf-455">Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="c0baf-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdź zmiany](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="c0baf-457">*Potwierdź zmiany*</span><span class="sxs-lookup"><span data-stu-id="c0baf-457">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c0baf-458">Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c0baf-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c0baf-459">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c0baf-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c0baf-460">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c0baf-461">![Publikowanie aplikacji](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publikowanie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="c0baf-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="c0baf-462">*Publikowanie witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-462">*Publishing the web site*</span></span>
2. <span data-ttu-id="c0baf-463">Zaimportuj profil publikowania zapisany w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="c0baf-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c0baf-464">![Importowanie profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="c0baf-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="c0baf-465">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="c0baf-465">*Importing publish profile*</span></span>
3. <span data-ttu-id="c0baf-466">Kliknij pozycję **Weryfikuj połączenie**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-466">Click **Validate Connection**.</span></span> <span data-ttu-id="c0baf-467">Po zakończeniu walidacji kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0baf-468">Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.</span><span class="sxs-lookup"><span data-stu-id="c0baf-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c0baf-469">![Weryfikowanie połączenia](build-restful-apis-with-aspnet-web-api/_static/image53.png "Weryfikowanie połączenia")</span><span class="sxs-lookup"><span data-stu-id="c0baf-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="c0baf-470">*Weryfikowanie połączenia*</span><span class="sxs-lookup"><span data-stu-id="c0baf-470">*Validating connection*</span></span>
4. <span data-ttu-id="c0baf-471">Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="c0baf-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c0baf-472">![Konfiguracja narzędzia Web Deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Konfiguracja narzędzia Web Deploy")</span><span class="sxs-lookup"><span data-stu-id="c0baf-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="c0baf-473">*Konfiguracja narzędzia Web Deploy*</span><span class="sxs-lookup"><span data-stu-id="c0baf-473">*Web deploy configuration*</span></span>
5. <span data-ttu-id="c0baf-474">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c0baf-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="c0baf-475">W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="c0baf-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="c0baf-476">W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="c0baf-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="c0baf-477">W polu **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="c0baf-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="c0baf-478">Wpisz nową nazwę bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="c0baf-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="c0baf-479">![Konfigurowanie parametrów połączenia docelowego](build-restful-apis-with-aspnet-web-api/_static/image55.png "Konfigurowanie parametrów połączenia docelowego")</span><span class="sxs-lookup"><span data-stu-id="c0baf-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="c0baf-480">*Konfigurowanie parametrów połączenia docelowego*</span><span class="sxs-lookup"><span data-stu-id="c0baf-480">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="c0baf-481">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-481">Then click **OK**.</span></span> <span data-ttu-id="c0baf-482">Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c0baf-483">![Tworzenie bazy danych](build-restful-apis-with-aspnet-web-api/_static/image56.png "Tworzenie ciągu bazy danych")</span><span class="sxs-lookup"><span data-stu-id="c0baf-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="c0baf-484">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="c0baf-484">*Creating the database*</span></span>
7. <span data-ttu-id="c0baf-485">Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie.</span><span class="sxs-lookup"><span data-stu-id="c0baf-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c0baf-486">Następnie kliknij przycisk **Next** (Dalej).</span><span class="sxs-lookup"><span data-stu-id="c0baf-486">Then click **Next**.</span></span>

    <span data-ttu-id="c0baf-487">![Parametry połączenia wskazujące SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Parametry połączenia wskazujące SQL Database")</span><span class="sxs-lookup"><span data-stu-id="c0baf-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="c0baf-488">*Parametry połączenia wskazujące SQL Database*</span><span class="sxs-lookup"><span data-stu-id="c0baf-488">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="c0baf-489">Na stronie **Podgląd** kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="c0baf-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c0baf-490">![Publikowanie aplikacji sieci Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publikowanie aplikacji sieci Web")</span><span class="sxs-lookup"><span data-stu-id="c0baf-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="c0baf-491">*Publikowanie aplikacji sieci Web*</span><span class="sxs-lookup"><span data-stu-id="c0baf-491">*Publishing the web application*</span></span>
9. <span data-ttu-id="c0baf-492">Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0baf-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="c0baf-493">![Aplikacja opublikowana w systemie Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Aplikacja opublikowana w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="c0baf-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="c0baf-494">*Aplikacja opublikowana na platformie Azure*</span><span class="sxs-lookup"><span data-stu-id="c0baf-494">*Application published to Azure*</span></span>
