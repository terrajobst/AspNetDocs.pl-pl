---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Tworzenie interfejsów API RESTful za pomocą wzorca ASP.NET Web API — ASP.NET 4.x
author: rick-anderson
description: 'Ćwiczenia praktyczne: Użyj interfejsu API sieci Web w programie ASP.NET 4.x do tworzenia prostego interfejsu API REST dla aplikacji menedżera kontaktu.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 3ba7f2d186e6f0837a32f69f964cec19fe625953
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59391484"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="b0915-103">Tworzenie interfejsów API RESTful za pomocą wzorca ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b0915-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="b0915-104">Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b0915-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="b0915-105">Ćwiczenia praktyczne: Użyj interfejsu API sieci Web w programie ASP.NET 4.x do tworzenia prostego interfejsu API REST dla aplikacji menedżera kontaktu.</span><span class="sxs-lookup"><span data-stu-id="b0915-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="b0915-106">Zostanie też utworzony klienta do korzystania z interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b0915-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="b0915-107">W ostatnich latach stało się jasne, czy protokół HTTP nie jest po prostu potrzeby stron HTML.</span><span class="sxs-lookup"><span data-stu-id="b0915-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="b0915-108">Ponadto jest to zaawansowana platforma do tworzenia interfejsów API sieci Web, przy użyciu aspekty czasowniki (GET, POST i tak dalej) oraz kilka proste pojęcia — takich jak *identyfikatorów URI* i *nagłówki*.</span><span class="sxs-lookup"><span data-stu-id="b0915-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="b0915-109">Web API platformy ASP.NET to zestaw składników, które upraszczają Programowanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0915-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="b0915-110">Ponieważ jest on oparty na podstawie środowiska uruchomieniowego platformy ASP.NET MVC, interfejs API sieci Web automatycznie obsługuje szczegóły niskiego poziomu transportu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0915-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="b0915-111">W tym samym czasie internetowy interfejs API udostępnia naturalnie modelu programowania protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0915-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="b0915-112">W rzeczywistości ma jeden cel interfejsu API sieci Web *nie* abstrakcji natychmiast rzeczywistości HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0915-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="b0915-113">W rezultacie internetowy interfejs API jest elastyczny i łatwy sposób rozszerzyć.</span><span class="sxs-lookup"><span data-stu-id="b0915-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="b0915-114">Styl architektury REST okazał się efektywny sposób wykorzystać HTTP -, mimo że na pewno nie jest prawidłowa tylko metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0915-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="b0915-115">Skontaktuj się z pomocą Menedżer udostępni zgodne ze specyfikacją REST na wyświetlanie, dodawanie i usuwanie kontaktów, między innymi.</span><span class="sxs-lookup"><span data-stu-id="b0915-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="b0915-116">W tym laboratorium wymaga podstawową wiedzę na temat protokołu HTTP, REST, a przyjęto założenie, że masz podstawową wiedzę na temat pracy z kodu HTML, JavaScript i jQuery.</span><span class="sxs-lookup"><span data-stu-id="b0915-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="b0915-117">Witryny sieci Web ASP.NET jest dedykowany do interfejsu API sieci Web platformy ASP.NET framework na [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="b0915-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="b0915-118">Ta lokacja będzie w dalszym ciągu zapewnia najnowsze informacje, przykłady i informacje związane z internetowego interfejsu API, dlatego należy sprawdzić ją często czy chcesz delve głębiej do sztukę tworzenia niestandardowych interfejsów API sieci Web dostępne praktycznie dowolne urządzenie lub rozwoju Framework.</span><span class="sxs-lookup"><span data-stu-id="b0915-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="b0915-119">ASP.NET Web API, podobne do ASP.NET MVC 4, ma dużą elastyczność w zakresie oddzielenie warstwy usług z kontrolerów, co pozwala korzystać z kilku dostępnych platform wstrzykiwanie zależności dość proste.</span><span class="sxs-lookup"><span data-stu-id="b0915-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="b0915-120">W witrynie MSDN, który ilustruje sposób używania Ninject do wstrzykiwania zależności w projekcie interfejsu API sieci Web platformy ASP.NET, którą można pobrać go z znajduje się dobrej próbki [tutaj](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="b0915-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="b0915-121">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="b0915-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b0915-122">Cele</span><span class="sxs-lookup"><span data-stu-id="b0915-122">Objectives</span></span>

<span data-ttu-id="b0915-123">W tym praktyczne laboratorium dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="b0915-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="b0915-124">Implementowanie RESTful interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="b0915-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="b0915-125">Wywoływanie interfejsu API z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="b0915-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b0915-126">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b0915-126">Prerequisites</span></span>

<span data-ttu-id="b0915-127">Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="b0915-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="b0915-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek B](#AppendixB) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="b0915-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b0915-129">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="b0915-129">Setup</span></span>

<span data-ttu-id="b0915-130">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="b0915-130">**Installing Code Snippets**</span></span>

<span data-ttu-id="b0915-131">Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b0915-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b0915-132">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="b0915-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b0915-133">Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek A: Za pomocą fragmentów kodu](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b0915-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b0915-134">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="b0915-134">Exercises</span></span>

<span data-ttu-id="b0915-135">To ćwiczenie praktyczne obejmuje wykonywanie następujących:</span><span class="sxs-lookup"><span data-stu-id="b0915-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="b0915-136">Ćwiczenie 1: Utwórz tylko do odczytu internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="b0915-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="b0915-137">Ćwiczenie 2: Tworzenie interfejsu API sieci Web odczytu/zapisu</span><span class="sxs-lookup"><span data-stu-id="b0915-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="b0915-138">Ćwiczenie 3: Używanie interfejsu API sieci Web z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="b0915-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="b0915-139">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="b0915-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b0915-140">Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.</span><span class="sxs-lookup"><span data-stu-id="b0915-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b0915-141">Szacowany czas do ukończenia tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="b0915-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="b0915-142">Ćwiczenie 1: Utwórz tylko do odczytu internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="b0915-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="b0915-143">W tym ćwiczeniu zostanie implementować metody GET tylko do odczytu, skontaktuj się z Menedżera.</span><span class="sxs-lookup"><span data-stu-id="b0915-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="b0915-144">Zadanie 1. Tworzenie projektu interfejsu API</span><span class="sxs-lookup"><span data-stu-id="b0915-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="b0915-145">W tym zadaniu użyjesz nowe szablony projektów sieci web platformy ASP.NET do tworzenia aplikacji sieci web interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0915-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="b0915-146">Uruchom **Visual Studio 2012 Express for Web**, aby to zrobić, przejdź do **Start** i typ **VS Express for Web** naciśnij **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b0915-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="b0915-147">Z **pliku** menu, wybierz opcję **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="b0915-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="b0915-148">Wybierz **Visual C# | Web** projektu typu w widoku drzewa typ projektu, a następnie wybierz **aplikacji sieci Web programu ASP.NET MVC 4** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="b0915-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="b0915-149">Ustaw projekt **nazwa** do *ContactManager* i **Nazwa rozwiązania** do *rozpocząć*, następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0915-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="b0915-150">![Tworzenie nowego projektu ASP.NET MVC 4.0 sieci Web aplikacji](build-restful-apis-with-aspnet-web-api/_static/image1.png "tworzenia nowego projektu ASP.NET MVC 4.0 sieci Web aplikacji")</span><span class="sxs-lookup"><span data-stu-id="b0915-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="b0915-151">*Tworzenie nowego projektu ASP.NET MVC 4.0 sieci Web aplikacji*</span><span class="sxs-lookup"><span data-stu-id="b0915-151">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="b0915-152">W oknie dialogowym Typ projektu ASP.NET MVC 4, wybierz **interfejsu API sieci Web** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="b0915-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="b0915-153">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0915-153">Click **OK**.</span></span>

    <span data-ttu-id="b0915-154">![Określanie typu projektu interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "określający typ projektu interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="b0915-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="b0915-155">*Określanie typu projektu interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="b0915-155">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="b0915-156">Zadanie 2 — Tworzenie kontrolerów interfejsu API Contact Manager</span><span class="sxs-lookup"><span data-stu-id="b0915-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="b0915-157">W tym zadaniu utworzysz klasy kontrolera, w których będą znajdować się metody interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b0915-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="b0915-158">Usuń plik o nazwie **ValuesController.cs** w ramach **kontrolerów** folder z projektu.</span><span class="sxs-lookup"><span data-stu-id="b0915-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="b0915-159">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w projekcie i wybierz **Dodaj | Kontroler** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="b0915-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="b0915-160">![Dodawanie nowego kontrolera do projektu](build-restful-apis-with-aspnet-web-api/_static/image3.png "dodawania nowego kontrolera do projektu")</span><span class="sxs-lookup"><span data-stu-id="b0915-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="b0915-161">*Dodawanie nowego kontrolera do projektu*</span><span class="sxs-lookup"><span data-stu-id="b0915-161">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="b0915-162">W **Dodaj kontroler** wyświetlonym oknie dialogowym wybierz **pusty Kontroler interfejsu API** menu szablonu.</span><span class="sxs-lookup"><span data-stu-id="b0915-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="b0915-163">Nazwa klasy kontrolera **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="b0915-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="b0915-164">Następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="b0915-164">Then, click **Add.**</span></span>

    <span data-ttu-id="b0915-165">![Aby utworzyć nowy kontroler interfejsu API sieci Web za pomocą okna dialogowego Dodawanie kontrolera](build-restful-apis-with-aspnet-web-api/_static/image4.png "do utworzenia nowego kontrolera interfejsu API sieci Web za pomocą okna dialogowego Dodawanie kontrolera")</span><span class="sxs-lookup"><span data-stu-id="b0915-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="b0915-166">*Aby utworzyć nowy kontroler interfejsu API sieci Web za pomocą okna dialogowego Dodawanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="b0915-166">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="b0915-167">Dodaj następujący kod do **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="b0915-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="b0915-168">(Code Snippet — *laboratorium interfejsu API sieci Web - Ex01 - Get, metoda interfejsu API*)</span><span class="sxs-lookup"><span data-stu-id="b0915-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="b0915-169">Naciśnij klawisz **F5** debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="b0915-170">Domyślną stronę główną dla projektu interfejsu API sieci Web powinna zostać wyświetlona.</span><span class="sxs-lookup"><span data-stu-id="b0915-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="b0915-171">![Domyślną stronę główną w aplikacji ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "domyślną stronę główną w aplikacji interfejsu API sieci Web platformy ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="b0915-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="b0915-172">*Domyślna strona główna aplikacji interfejsu API sieci Web platformy ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="b0915-172">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="b0915-173">W oknie programu Internet Explorer, naciśnij klawisz **F12** klawisz, aby otworzyć **narzędzi deweloperskich** okna.</span><span class="sxs-lookup"><span data-stu-id="b0915-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="b0915-174">Kliknij przycisk **sieci** kartę, a następnie kliknij przycisk **Rozpocznij przechwytywanie** przycisk, aby rozpocząć, Przechwytywanie ruchu sieciowego do okna.</span><span class="sxs-lookup"><span data-stu-id="b0915-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="b0915-175">![Otwierając karty sieciowej i Inicjowanie sieci przechwytywania](build-restful-apis-with-aspnet-web-api/_static/image6.png "otwierając karty sieciowej i Inicjowanie sieci przechwytywania")</span><span class="sxs-lookup"><span data-stu-id="b0915-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="b0915-176">*Otwierając karty sieciowej i Inicjowanie sieciowej przechwytywania*</span><span class="sxs-lookup"><span data-stu-id="b0915-176">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="b0915-177">Dołącz adres URL w pasku adresu przeglądarki z **/api/contact** i naciśnij klawisz enter.</span><span class="sxs-lookup"><span data-stu-id="b0915-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="b0915-178">W oknie Przechwytywanie sieci są wyświetlane szczegóły przekazywania.</span><span class="sxs-lookup"><span data-stu-id="b0915-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="b0915-179">Należy zauważyć, że typ MIME odpowiedzi **application/json**.</span><span class="sxs-lookup"><span data-stu-id="b0915-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="b0915-180">W tym przykładzie pokazano, jak dane JSON są domyślnym formatem danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="b0915-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="b0915-181">![Wyświetlanie danych wyjściowych żądania interfejsu API sieci Web w widoku sieci](build-restful-apis-with-aspnet-web-api/_static/image7.png "wyświetlania danych wyjściowych żądania interfejsu API sieci Web w widoku sieci")</span><span class="sxs-lookup"><span data-stu-id="b0915-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="b0915-182">*Wyświetlanie danych wyjściowych żądania interfejsu API sieci Web w widoku sieci*</span><span class="sxs-lookup"><span data-stu-id="b0915-182">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0915-183">Zachowanie domyślne programu Internet Explorer 10 na tym etapie należy zadać, jeśli użytkownik chce zapisać lub otworzyć strumienia wynikające z wywołania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0915-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="b0915-184">Dane wyjściowe będą plik tekstowy zawierający wynik wywołania adresu URL interfejsu API sieci Web JSON.</span><span class="sxs-lookup"><span data-stu-id="b0915-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="b0915-185">Nie Anuluj okno dialogowe Aby móc obejrzeć zawartość odpowiedzi za pomocą okna narzędzi dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="b0915-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="b0915-186">Kliknij przycisk **przejdź do widoku szczegółowym** przycisk, aby wyświetlić więcej szczegółów na temat odpowiedzi to wywołanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b0915-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="b0915-187">![Przełącz na widok szczegółowy](build-restful-apis-with-aspnet-web-api/_static/image8.png "Przełącz do widoku szczegółów")</span><span class="sxs-lookup"><span data-stu-id="b0915-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="b0915-188">*Przełącz na widok szczegółowy*</span><span class="sxs-lookup"><span data-stu-id="b0915-188">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="b0915-189">Kliknij przycisk **treść odpowiedzi** kartę, aby wyświetlić rzeczywiste tekst JSON w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b0915-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="b0915-190">![Przeglądanie za pomocą pliku JSON tekst danych wyjściowych polecenia w Monitorze sieci](build-restful-apis-with-aspnet-web-api/_static/image9.png "przeglądanie za pomocą pliku JSON tekst danych wyjściowych polecenia w Monitorze sieci")</span><span class="sxs-lookup"><span data-stu-id="b0915-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="b0915-191">*Wyświetlanie tekstu dane wyjściowe JSON w Monitorze sieci*</span><span class="sxs-lookup"><span data-stu-id="b0915-191">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="b0915-192">Zadanie 3. tworzenie modeli skontaktuj się z pomocą i rozszerzyć nazwę Contactcontroller</span><span class="sxs-lookup"><span data-stu-id="b0915-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="b0915-193">W tym zadaniu utworzysz klasy kontrolera, w których będą znajdować się metody interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b0915-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="b0915-194">Kliknij prawym przyciskiem myszy **modeli** i wybierz polecenie **Dodaj | Klasa...**  z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="b0915-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="b0915-195">![Dodawanie nowego modelu do aplikacji sieci web](build-restful-apis-with-aspnet-web-api/_static/image10.png "dodanie nowego modelu aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="b0915-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="b0915-196">*Dodawanie nowego modelu do aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="b0915-196">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="b0915-197">W **Dodaj nowy element** okno dialogowe, Nazwa nowego pliku **Contact.cs** i kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="b0915-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="b0915-198">![Tworzenie nowego pliku klasy skontaktuj się z pomocą](build-restful-apis-with-aspnet-web-api/_static/image11.png "Tworzenie nowego pliku klasy kontaktu")</span><span class="sxs-lookup"><span data-stu-id="b0915-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="b0915-199">*Tworzenie nowego pliku klasy kontaktu*</span><span class="sxs-lookup"><span data-stu-id="b0915-199">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="b0915-200">Dodaj następujący wyróżniony kod do **skontaktuj się z pomocą** klasy.</span><span class="sxs-lookup"><span data-stu-id="b0915-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="b0915-201">(Code Snippet — *sieci Web interfejsu API laboratorium — Ex01 — skontaktuj się z klasy*)</span><span class="sxs-lookup"><span data-stu-id="b0915-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="b0915-202">W **ContactController** klasy, zaznacz wyraz **ciąg** w definicji metody **uzyskać** metody i wpisz wyraz *skontaktuj się z pomocą*.</span><span class="sxs-lookup"><span data-stu-id="b0915-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="b0915-203">Po słowo jest wpisany, wskaźnik pojawi się na początku słowo **skontaktuj się z pomocą**.</span><span class="sxs-lookup"><span data-stu-id="b0915-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="b0915-204">Albo naciśnij i przytrzymaj **Ctrl** klucz i naciśnij klawisz kropki (.) lub kliknij ikonę, aby otworzyć okno dialogowe pomocy w edytorze kodu, aby automatycznie wypełnić przy użyciu myszy **przy użyciu** dyrektywy dla modeli przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="b0915-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Przy użyciu technologii Intellisense pomocy dla deklaracji przestrzeni nazw](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="b0915-206">*Przy użyciu technologii Intellisense pomocy dla deklaracji przestrzeni nazw*</span><span class="sxs-lookup"><span data-stu-id="b0915-206">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="b0915-207">Zmodyfikuj kod **uzyskać** metodę, tak że zwraca tablicę, skontaktuj się z modelu wystąpień.</span><span class="sxs-lookup"><span data-stu-id="b0915-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="b0915-208">(Code Snippet — *laboratorium interfejsu API sieci Web — Ex01 — zwraca listę kontaktów*)</span><span class="sxs-lookup"><span data-stu-id="b0915-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="b0915-209">Naciśnij klawisz **F5** do debugowania aplikacji sieci web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b0915-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="b0915-210">Aby wyświetlić zmiany wprowadzone dane wyjściowe odpowiedzi interfejsu API, wykonaj następujące czynności.</span><span class="sxs-lookup"><span data-stu-id="b0915-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="b0915-211">Po otwarciu przeglądarki naciśnij **F12** Jeśli narzędzia programistyczne nie są jeszcze otwarte.</span><span class="sxs-lookup"><span data-stu-id="b0915-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="b0915-212">Kliknij przycisk **sieci** kartę.</span><span class="sxs-lookup"><span data-stu-id="b0915-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="b0915-213">Naciśnij klawisz **Rozpocznij przechwytywanie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b0915-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="b0915-214">Dodaj sufiks adresu URL **/api/contact** do adresu URL w pasku adresu i naciśnij klawisz **Enter** klucza.</span><span class="sxs-lookup"><span data-stu-id="b0915-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="b0915-215">Naciśnij klawisz **przejdź do widoku szczegółowym** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b0915-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="b0915-216">Wybierz **treść odpowiedzi** kartę. Powinien zostać wyświetlony ciąg JSON reprezentujący serializowane postaci tablicy, skontaktuj się z wystąpień.</span><span class="sxs-lookup"><span data-stu-id="b0915-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="b0915-217">![JSON serializowane dane wyjściowe złożonych wywołania metody interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializowane dane wyjściowe złożonych wywołania metody interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="b0915-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="b0915-218">*Dane wyjściowe Zserializowany do ciągu JSON złożonych wywołania metody interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="b0915-218">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="b0915-219">Zadanie 4 — wyodrębnianie funkcji do warstwy usług</span><span class="sxs-lookup"><span data-stu-id="b0915-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="b0915-220">To zadanie pokażemy, jak można wyodrębnić funkcji do warstwy usług, aby ułatwić deweloperom rozdzielić ich funkcjonalności usług z warstwy kontrolera, co pozwoli na możliwość ponownego wykorzystania usług, które rzeczywiście wykonują pracę.</span><span class="sxs-lookup"><span data-stu-id="b0915-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="b0915-221">Utwórz nowy folder w katalogu głównym rozwiązania i nadaj mu nazwę **usług**.</span><span class="sxs-lookup"><span data-stu-id="b0915-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="b0915-222">Aby to zrobić, kliknij prawym przyciskiem myszy **ContactManager** projektu, wybierz opcję **Dodaj** | **nowy Folder**, nadaj jej nazwę *usług*.</span><span class="sxs-lookup"><span data-stu-id="b0915-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="b0915-223">![Tworzenie folderu usługi](build-restful-apis-with-aspnet-web-api/_static/image14.png "folder Tworzenie usługi")</span><span class="sxs-lookup"><span data-stu-id="b0915-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="b0915-224">*Tworzenie folderu usługi*</span><span class="sxs-lookup"><span data-stu-id="b0915-224">*Creating Services folder*</span></span>
2. <span data-ttu-id="b0915-225">Kliknij prawym przyciskiem myszy **usług** i wybierz polecenie **Dodaj | Klasa...**  z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="b0915-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="b0915-226">![Dodawanie nowej klasy do folderu usługi](build-restful-apis-with-aspnet-web-api/_static/image15.png "Dodawanie nowej klasy do folderu usługi")</span><span class="sxs-lookup"><span data-stu-id="b0915-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="b0915-227">*Dodawanie nowej klasy do folderu usługi*</span><span class="sxs-lookup"><span data-stu-id="b0915-227">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="b0915-228">Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe, nadaj nowej klasie **ContactRepository** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b0915-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="b0915-229">![Tworzenie pliku klasy w celu uwzględnienia kodu dla warstwy usług skontaktuj się z repozytorium](build-restful-apis-with-aspnet-web-api/_static/image16.png "tworzenia plik klasy w celu uwzględnienia kodu dla warstwy usług skontaktuj się z repozytorium")</span><span class="sxs-lookup"><span data-stu-id="b0915-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="b0915-230">*Tworzenie pliku klasy w celu uwzględnienia kodu dla warstwy usług skontaktuj się z repozytorium*</span><span class="sxs-lookup"><span data-stu-id="b0915-230">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="b0915-231">Dodaj instrukcję using dyrektywę **ContactRepository.cs** plik, aby uwzględnić przestrzeń nazw modeli.</span><span class="sxs-lookup"><span data-stu-id="b0915-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="b0915-232">Dodaj następujący wyróżniony kod do **ContactRepository.cs** plik, aby zaimplementować metodę GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="b0915-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="b0915-233">(Code Snippet — *sieci Web interfejsu API laboratorium — Ex01 — skontaktuj się z repozytorium*)</span><span class="sxs-lookup"><span data-stu-id="b0915-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="b0915-234">Otwórz **ContactController.cs** plik, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="b0915-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="b0915-235">Dodaj następującą instrukcję using do sekcji deklaracji przestrzeni nazw w pliku.</span><span class="sxs-lookup"><span data-stu-id="b0915-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="b0915-236">Dodaj następujący wyróżniony kod do **ContactController.cs** klasy, aby dodać pole prywatne reprezentujący wystąpienie repozytorium, pozostała część klasy członkowie mogą wprowadzać użytkowania implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="b0915-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="b0915-237">(Code Snippet — *laboratorium interfejsu API — Ex01 — skontaktuj się z pomocą kontroler Web*)</span><span class="sxs-lookup"><span data-stu-id="b0915-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="b0915-238">Zmiana **uzyskać** metodę, tak że sprawia, że korzystanie z usługi skontaktuj się z repozytorium.</span><span class="sxs-lookup"><span data-stu-id="b0915-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="b0915-239">(Code Snippet — *laboratorium interfejsu API sieci Web — Ex01 — zwraca listę kontaktów za pośrednictwem repozytorium*)</span><span class="sxs-lookup"><span data-stu-id="b0915-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="b0915-240">Umieść punkt przerwania **ContactController**firmy **uzyskać** definicję metody.</span><span class="sxs-lookup"><span data-stu-id="b0915-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="b0915-241">![Dodawanie punktów przerwania, skontaktuj się z kontrolerem](build-restful-apis-with-aspnet-web-api/_static/image17.png "Dodawanie punktów przerwania, skontaktuj się z kontrolerem")</span><span class="sxs-lookup"><span data-stu-id="b0915-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="b0915-242">*Dodawanie punktów przerwania, skontaktuj się z kontrolerem.*</span><span class="sxs-lookup"><span data-stu-id="b0915-242">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="b0915-243">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="b0915-244">Po otwarciu przeglądarki, naciśnij klawisz **F12** można otworzyć narzędzia dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="b0915-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="b0915-245">Kliknij przycisk **sieci** kartę.</span><span class="sxs-lookup"><span data-stu-id="b0915-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="b0915-246">Kliknij przycisk **Rozpocznij przechwytywanie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b0915-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="b0915-247">Dołącz adres URL w pasku adresu z sufiksem **/api/contact** i naciśnij klawisz **Enter** załadować Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b0915-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="b0915-248">Program Visual Studio 2012 należy przerywaj raz **uzyskać** metoda rozpoczyna wykonywanie.</span><span class="sxs-lookup"><span data-stu-id="b0915-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="b0915-249">![Przerwanie wewnątrz metody Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "istotne wewnątrz metody Get")</span><span class="sxs-lookup"><span data-stu-id="b0915-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="b0915-250">*Przerwanie wewnątrz metody Get*</span><span class="sxs-lookup"><span data-stu-id="b0915-250">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="b0915-251">Naciśnij klawisz **F5** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="b0915-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="b0915-252">Wróć do przeglądarki Internet Explorer Jeśli nie jest już w trybie koncentracji uwagi.</span><span class="sxs-lookup"><span data-stu-id="b0915-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="b0915-253">Należy pamiętać, okno przechwytywania sieci.</span><span class="sxs-lookup"><span data-stu-id="b0915-253">Note the network capture window.</span></span>

    <span data-ttu-id="b0915-254">![Widok w programie Internet Explorer, wyświetlanie wyników wywołań interfejsu API sieci Web sieci](build-restful-apis-with-aspnet-web-api/_static/image19.png "sieci widok w programie Internet Explorer, wyświetlanie wyników wywołań interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="b0915-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="b0915-255">*Widok sieci w programie Internet Explorer, wyświetlanie wyników wywołań interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="b0915-255">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="b0915-256">Kliknij przycisk **przejdź do widoku szczegółowym** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b0915-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="b0915-257">Kliknij przycisk **treść odpowiedzi** kartę. Należy pamiętać, dane wyjściowe JSON z wywołania interfejsu API i jak reprezentuje dwa kontakty pobierane przez warstwę usług.</span><span class="sxs-lookup"><span data-stu-id="b0915-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="b0915-258">![Wyświetlanie danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia dla deweloperów](build-restful-apis-with-aspnet-web-api/_static/image20.png "wyświetlania danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia dla deweloperów")</span><span class="sxs-lookup"><span data-stu-id="b0915-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="b0915-259">*Wyświetlanie danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia dla deweloperów*</span><span class="sxs-lookup"><span data-stu-id="b0915-259">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="b0915-260">Ćwiczenie 2: Tworzenie interfejsu API sieci Web odczytu/zapisu</span><span class="sxs-lookup"><span data-stu-id="b0915-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="b0915-261">W tym ćwiczeniu zostaną zaimplementowane POST i PUT metody skontaktuj się z pomocą manager ją włączyć za pomocą funkcji edycji danych.</span><span class="sxs-lookup"><span data-stu-id="b0915-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="b0915-262">Zadanie 1 — otwierania projektu interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="b0915-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="b0915-263">W tym zadaniu przygotujesz się do zwiększenia projekt internetowy interfejs API utworzony w ćwiczeniu 1, dzięki czemu może akceptować dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b0915-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="b0915-264">Uruchom **Visual Studio 2012 Express for Web**, aby to zrobić, przejdź do **Start** i typ **VS Express for Web** naciśnij **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b0915-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="b0915-265">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex02-ReadWriteWebAPI/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="b0915-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="b0915-266">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="b0915-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b0915-267">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="b0915-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b0915-268">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b0915-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b0915-269">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="b0915-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b0915-270">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="b0915-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b0915-271">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="b0915-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b0915-272">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="b0915-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b0915-273">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="b0915-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b0915-274">Otwórz **Services/ContactRepository.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="b0915-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="b0915-275">Zadanie 2 — Dodawanie funkcji trwałości danych do implementacji skontaktuj się z repozytorium</span><span class="sxs-lookup"><span data-stu-id="b0915-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="b0915-276">W tym zadaniu zostanie rozszerzyć klasę ContactRepository projektu składnika Web API utworzony w ćwiczeniu 1 tak, aby go zostaną zachowane, a akceptują dane wejściowe użytkownika i nowe wystąpienia skontaktuj się z pomocą.</span><span class="sxs-lookup"><span data-stu-id="b0915-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="b0915-277">Dodaj następującą stałą na **ContactRepository** klasy do reprezentowania nazwy nazwę serwera sieci web pamięci podręcznej elementu klucza w dalszej części tego ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="b0915-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="b0915-278">Dodaj Konstruktor do **ContactRepository** zawierający poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="b0915-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="b0915-279">(Code Snippet — *sieci Web interfejsu API laboratorium — Ex02 — skontaktuj się z repozytorium Konstruktor*)</span><span class="sxs-lookup"><span data-stu-id="b0915-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="b0915-280">Zmodyfikuj kod **GetAllContacts** metody, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b0915-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="b0915-281">(Code Snippet — *laboratorium interfejsu API sieci Web - Ex02 - pobrania wszystkich kontaktów*)</span><span class="sxs-lookup"><span data-stu-id="b0915-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="b0915-282">W tym przykładzie jest dla celów demonstracyjnych i będzie używać pamięci podręcznej na serwerze sieci web jako nośnika magazynowania, tak, aby wartości będzie być dostępne dla wielu klientów jednocześnie, a nie za pomocą mechanizmu magazynowania sesji lub okres istnienia magazynu żądania.</span><span class="sxs-lookup"><span data-stu-id="b0915-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="b0915-283">Jeden wystarczą środki platformy Entity Framework, magazynu XML lub inne różne zamiast pamięci podręcznej na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="b0915-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="b0915-284">Zaimplementuj nową metodę o nazwie **SaveContact** do **ContactRepository** klasy, które wykonają tę pracę zapisywać kontaktu.</span><span class="sxs-lookup"><span data-stu-id="b0915-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="b0915-285">**SaveContact** metoda powinna podjąć jeden **skontaktuj się z pomocą** parametrów i zwrotu atrybut typu wartość logiczna. wartość oznaczający powodzenie lub niepowodzenie.</span><span class="sxs-lookup"><span data-stu-id="b0915-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="b0915-286">(Code Snippet — *sieci Web interfejsu API laboratorium — Ex02 — implementacja metody SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="b0915-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="b0915-287">Ćwiczenie 3: Używanie interfejsu API sieci Web z klienta HTML</span><span class="sxs-lookup"><span data-stu-id="b0915-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="b0915-288">W tym ćwiczeniu utworzysz klienta HTML do wywołania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0915-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="b0915-289">Ten klient ułatwi wymiany danych z interfejsu API sieci Web przy użyciu języka JavaScript i wyświetli wyniki w przeglądarce sieci web przy użyciu znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="b0915-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="b0915-290">Zadanie 1 — Modyfikowanie widoku indeksu do zapewnienia wyświetlanie kontaktów graficznego interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="b0915-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="b0915-291">W tym zadaniu należy zmodyfikować domyślny widok indeksu w aplikacji sieci web do obsługi wymagań wyświetlania listy istniejących kontaktów w przeglądarce HTML.</span><span class="sxs-lookup"><span data-stu-id="b0915-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="b0915-292">Otwórz **Visual Studio 2012 Express for Web** Jeśli nie jest już otwarty.</span><span class="sxs-lookup"><span data-stu-id="b0915-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="b0915-293">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex03-ConsumingWebAPI/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="b0915-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="b0915-294">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="b0915-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b0915-295">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="b0915-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b0915-296">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b0915-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b0915-297">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="b0915-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b0915-298">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="b0915-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b0915-299">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="b0915-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b0915-300">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="b0915-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b0915-301">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="b0915-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b0915-302">Otwórz **Index.cshtml** plik znajdujący się w **widoków domowych** folderu.</span><span class="sxs-lookup"><span data-stu-id="b0915-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="b0915-303">Zastąp kod HTML wewnątrz elementu div z identyfikatorem **treści** tak, aby wyglądał podobnie do poniższego kodu.</span><span class="sxs-lookup"><span data-stu-id="b0915-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="b0915-304">Dodaj następujący kod Javascript w dolnej części pliku do wykonania żądania HTTP do interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0915-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="b0915-305">Otwórz **ContactController.cs** plik, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="b0915-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="b0915-306">Umieść punkt przerwania na **uzyskać** metody **ContactController** klasy.</span><span class="sxs-lookup"><span data-stu-id="b0915-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="b0915-307">![Wprowadzenie do punktu przerwania na metody Get kontrolera interfejsu API](build-restful-apis-with-aspnet-web-api/_static/image21.png "wprowadzania punkt przerwania na metody Get kontrolera interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="b0915-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="b0915-308">*Wprowadzenie do punktu przerwania na metody Get kontrolera interfejsu API*</span><span class="sxs-lookup"><span data-stu-id="b0915-308">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="b0915-309">Naciśnij klawisz **F5** Aby uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="b0915-309">Press **F5** to run the project.</span></span> <span data-ttu-id="b0915-310">Przeglądarka zostanie załadowany dokumentu HTML.</span><span class="sxs-lookup"><span data-stu-id="b0915-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0915-311">Upewnij się, że po przejściu do adresu URL katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="b0915-312">Gdy strona ładuje się i wykonuje kod JavaScript, punkt przerwania zostanie osiągnięty i wykonywanie kodu zostanie wstrzymany w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="b0915-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="b0915-313">![Debugowanie do wywołań interfejsu API sieci Web programu VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "debugowania do wywołań interfejsu API sieci Web programu VS Express for Web")</span><span class="sxs-lookup"><span data-stu-id="b0915-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="b0915-314">*Debugowanie do wywołania interfejsu API sieci Web programu Visual Studio 2012 Express for Web*</span><span class="sxs-lookup"><span data-stu-id="b0915-314">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="b0915-315">Usuń punkt przerwania, a następnie naciśnij klawisz **F5** lub narzędzi debugowania **Kontynuuj** przycisk, aby kontynuować ładowania widoku w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b0915-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="b0915-316">Po ukończeniu wywołania interfejsu API sieci Web powinna być widoczna kontakty zwrócony z interfejsu API sieci Web, wywołaj wyświetlane jako elementy listy w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b0915-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="b0915-317">![Wyniki wywołania interfejsu API, wyświetlana w przeglądarce jako elementy listy](build-restful-apis-with-aspnet-web-api/_static/image23.png "wyniki wywołania interfejsu API, wyświetlana w przeglądarce jako elementy listy")</span><span class="sxs-lookup"><span data-stu-id="b0915-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="b0915-318">*Wyniki wywołania interfejsu API, wyświetlana w przeglądarce jako elementy listy*</span><span class="sxs-lookup"><span data-stu-id="b0915-318">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="b0915-319">Zatrzymaj debugowanie.</span><span class="sxs-lookup"><span data-stu-id="b0915-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="b0915-320">Zadanie 2 — Modyfikowanie widoku indeksu, aby zapewnić graficzny interfejs użytkownika pozwalający kontaktów</span><span class="sxs-lookup"><span data-stu-id="b0915-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="b0915-321">W ramach tego zadania będziesz zmodyfikuj widok indeksu w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="b0915-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="b0915-322">Formularz zostanie dodany do strony HTML, który będzie przechwytywać dane wejściowe użytkownika i wysyłać je do interfejsu API sieci Web, aby utworzyć nowy kontakt, a nowa metoda kontrolera interfejsu API sieci Web zostanie utworzone w celu zbierania daty z graficznego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b0915-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="b0915-323">Otwórz **ContactController.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="b0915-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="b0915-324">Dodaj nową metodę do klasy kontrolera, o nazwie **wpis** jak pokazano w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="b0915-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="b0915-325">(Code Snippet — *sieci Web interfejsu API laboratorium — Ex03 — metody Post*)</span><span class="sxs-lookup"><span data-stu-id="b0915-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="b0915-326">Otwórz **Index.cshtml** plik w programie Visual Studio, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="b0915-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="b0915-327">Dodaj poniższy kod HTML do pliku, zaraz po listę nieuporządkowaną, które zostały dodane w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="b0915-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="b0915-328">W elemencie skrypt w dolnej części dokumentu Dodaj następujący wyróżniony kod do obsługi zdarzeń kliknięcia przycisku, które będą wysyłania danych do interfejsu API sieci Web przy użyciu wywołania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b0915-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="b0915-329">W **ContactController.cs**, umieść punkt przerwania na **wpis** metody.</span><span class="sxs-lookup"><span data-stu-id="b0915-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="b0915-330">Naciśnij klawisz **F5** do uruchamiania aplikacji w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b0915-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="b0915-331">Po załadowaniu w przeglądarce stronę wpisz nazwę nowego kontaktu i identyfikator i kliknij przycisk **Zapisz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b0915-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="b0915-332">![Klient HTML dokument załadowaniu w przeglądarce](build-restful-apis-with-aspnet-web-api/_static/image24.png "dokument klienta HTML załadowaniu w przeglądarce")</span><span class="sxs-lookup"><span data-stu-id="b0915-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="b0915-333">*Dokument HTML klienta załadowaniu w przeglądarce*</span><span class="sxs-lookup"><span data-stu-id="b0915-333">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="b0915-334">Gdy podziały okna debugera w **wpis** metody take a przyjrzeć się właściwości **skontaktuj się z pomocą** parametru.</span><span class="sxs-lookup"><span data-stu-id="b0915-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="b0915-335">Wartość powinna być zgodna dane, które zostały wprowadzone w formularzu.</span><span class="sxs-lookup"><span data-stu-id="b0915-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="b0915-336">![Skontaktuj się z obiektu są wysyłane do interfejsu API sieci Web z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "Contact obiektu są wysyłane do interfejsu API sieci Web z klienta")</span><span class="sxs-lookup"><span data-stu-id="b0915-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="b0915-337">*Skontaktuj się z obiektu są wysyłane do interfejsu API sieci Web z klienta*</span><span class="sxs-lookup"><span data-stu-id="b0915-337">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="b0915-338">Krok za pośrednictwem metody w debugerze, dopóki **odpowiedzi** zmiennej została utworzona.</span><span class="sxs-lookup"><span data-stu-id="b0915-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="b0915-339">W trakcie kontroli w **lokalne** okna debugera, zobaczysz, że wszystkie właściwości ustawione.</span><span class="sxs-lookup"><span data-stu-id="b0915-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="b0915-340">![Odpowiedzi, po utworzeniu w debugerze](build-restful-apis-with-aspnet-web-api/_static/image26.png "odpowiedzi, po utworzeniu w debugerze")</span><span class="sxs-lookup"><span data-stu-id="b0915-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="b0915-341">*Odpowiedzi, po utworzeniu w debugerze*</span><span class="sxs-lookup"><span data-stu-id="b0915-341">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="b0915-342">Jeśli użytkownik naciśnie klawisz **F5** lub kliknij przycisk **Kontynuuj** w debugerze żądanie zostanie ukończone.</span><span class="sxs-lookup"><span data-stu-id="b0915-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="b0915-343">Kiedy przełączysz się do przeglądarki, dodano nowy kontakt do listy kontaktów przechowywane przez **ContactRepository** implementacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="b0915-344">![Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia skontaktuj się z pomocą](build-restful-apis-with-aspnet-web-api/_static/image27.png "przeglądarki odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu")</span><span class="sxs-lookup"><span data-stu-id="b0915-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="b0915-345">*Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu*</span><span class="sxs-lookup"><span data-stu-id="b0915-345">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="b0915-346">Ponadto można wdrożyć tę aplikację do platformy Azure następujące [dodatek C: Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="b0915-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b0915-347">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b0915-347">Summary</span></span>

<span data-ttu-id="b0915-348">Do nowej struktury Web API platformy ASP.NET i implementacji RESTful interfejsów API sieci Web przy użyciu framework została wprowadzona w tym laboratorium.</span><span class="sxs-lookup"><span data-stu-id="b0915-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="b0915-349">W tym miejscu możesz utworzyć nowe repozytorium, które ułatwia funkcji trwałości danych przy użyciu dowolnej liczby mechanizmów i Podłączanie usługi zamiast prostych podano w przykładzie w tym laboratorium.</span><span class="sxs-lookup"><span data-stu-id="b0915-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="b0915-350">Internetowy interfejs API obsługuje szereg dodatkowych funkcji, takich jak umożliwianie komunikację od klientów innych niż HTML napisane w dowolnym języku, który obsługuje HTTP i JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="b0915-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="b0915-351">Możliwość hostowania interfejsu Web API poza aplikacji sieci web typowe możliwe jest również, jak również jest możliwość tworzenia własnych formatów serializacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="b0915-352">Witryny sieci Web ASP.NET jest dedykowany do interfejsu API sieci Web platformy ASP.NET framework na [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="b0915-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="b0915-353">Ta lokacja będzie w dalszym ciągu zapewnia najnowsze informacje, przykłady i informacje związane z internetowego interfejsu API, dlatego należy sprawdzić ją często czy chcesz delve głębiej do sztukę tworzenia niestandardowych interfejsów API sieci Web dostępne praktycznie dowolne urządzenie lub rozwoju Framework.</span><span class="sxs-lookup"><span data-stu-id="b0915-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="b0915-354">Dodatek A: Za pomocą fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="b0915-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="b0915-355">Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="b0915-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b0915-356">Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="b0915-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b0915-357">![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](build-restful-apis-with-aspnet-web-api/_static/image28.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="b0915-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b0915-358">*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*</span><span class="sxs-lookup"><span data-stu-id="b0915-358">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="b0915-359">Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)</span><span class="sxs-lookup"><span data-stu-id="b0915-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="b0915-360">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="b0915-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b0915-361">Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="b0915-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b0915-362">Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.</span><span class="sxs-lookup"><span data-stu-id="b0915-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b0915-363">Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="b0915-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b0915-364">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="b0915-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="b0915-365">![Rozpocznij wpisywanie nazwy fragmentu kodu](build-restful-apis-with-aspnet-web-api/_static/image29.png "Rozpocznij wpisywanie nazwy fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="b0915-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="b0915-366">*Rozpocznij wpisywanie nazwy fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="b0915-366">*Start typing the snippet name*</span></span>

    <span data-ttu-id="b0915-367">![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](build-restful-apis-with-aspnet-web-api/_static/image30.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="b0915-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="b0915-368">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="b0915-368">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="b0915-369">![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](build-restful-apis-with-aspnet-web-api/_static/image31.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="b0915-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="b0915-370">*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*</span><span class="sxs-lookup"><span data-stu-id="b0915-370">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="b0915-371">Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)</span><span class="sxs-lookup"><span data-stu-id="b0915-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="b0915-372">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="b0915-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="b0915-373">Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.</span><span class="sxs-lookup"><span data-stu-id="b0915-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="b0915-374">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="b0915-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="b0915-375">![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](build-restful-apis-with-aspnet-web-api/_static/image32.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="b0915-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="b0915-376">*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="b0915-376">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="b0915-377">![Wybierz odpowiedni fragment z listy, klikając go](build-restful-apis-with-aspnet-web-api/_static/image33.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="b0915-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="b0915-378">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="b0915-378">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b0915-379">Dodatek B: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="b0915-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b0915-380">Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b0915-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b0915-381">Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="b0915-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b0915-382">Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b0915-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b0915-383">Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b0915-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b0915-384">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="b0915-384">Click on **Install Now**.</span></span> <span data-ttu-id="b0915-385">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="b0915-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b0915-386">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="b0915-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b0915-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b0915-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b0915-388">*Install Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b0915-388">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b0915-389">Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="b0915-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="b0915-391">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="b0915-391">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b0915-392">Zaczekaj, aż do zakończenia procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-392">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="b0915-394">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="b0915-394">*Installation progress*</span></span>
6. <span data-ttu-id="b0915-395">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="b0915-395">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="b0915-397">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="b0915-397">*Installation completed*</span></span>
7. <span data-ttu-id="b0915-398">Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0915-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b0915-399">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="b0915-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web tile](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="b0915-401">*VS Express for Web tile*</span><span class="sxs-lookup"><span data-stu-id="b0915-401">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b0915-402">Dodatek C: Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="b0915-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="b0915-403">Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w witrynie Azure Portal i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy udostępnianych przez platformę Azure.</span><span class="sxs-lookup"><span data-stu-id="b0915-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="b0915-404">Zadanie 1. Tworzenie nowej witryny sieci Web w witrynie Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b0915-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="b0915-405">Przejdź do [portalu zarządzania systemu Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="b0915-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0915-406">Za pomocą platformy Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu.</span><span class="sxs-lookup"><span data-stu-id="b0915-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="b0915-407">Możesz zarejestrować się [tutaj](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="b0915-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="b0915-408">![Zaloguj się do portalu usługi Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Zaloguj się do portalu usługi Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b0915-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="b0915-409">*Zaloguj się do portalu*</span><span class="sxs-lookup"><span data-stu-id="b0915-409">*Log on to Portal*</span></span>
2. <span data-ttu-id="b0915-410">Kliknij przycisk **New** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="b0915-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="b0915-411">![Tworzenie nowej witryny sieci Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="b0915-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="b0915-412">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="b0915-412">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="b0915-413">Kliknij przycisk **obliczenia** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b0915-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="b0915-414">Następnie wybierz pozycję **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="b0915-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="b0915-415">Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b0915-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0915-416">Platforma Azure to hosta dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi.</span><span class="sxs-lookup"><span data-stu-id="b0915-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="b0915-417">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web na platformie Azure z poza portalem.</span><span class="sxs-lookup"><span data-stu-id="b0915-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="b0915-418">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b0915-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="b0915-419">![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](build-restful-apis-with-aspnet-web-api/_static/image41.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="b0915-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="b0915-420">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="b0915-420">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="b0915-421">Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="b0915-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="b0915-422">Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="b0915-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="b0915-423">Sprawdź, czy działa nową witrynę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b0915-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="b0915-424">![Przejście do nowej witryny sieci web](build-restful-apis-with-aspnet-web-api/_static/image42.png "przejście do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="b0915-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="b0915-425">*Przejście do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="b0915-425">*Browsing to the new web site*</span></span>

    <span data-ttu-id="b0915-426">![Witryna sieci Web działa](build-restful-apis-with-aspnet-web-api/_static/image43.png "witryna sieci Web działa")</span><span class="sxs-lookup"><span data-stu-id="b0915-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="b0915-427">*Witryna sieci Web działa*</span><span class="sxs-lookup"><span data-stu-id="b0915-427">*Web site running*</span></span>
6. <span data-ttu-id="b0915-428">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="b0915-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="b0915-429">![Otwieranie strony zarządzania witryny sieci web](build-restful-apis-with-aspnet-web-api/_static/image44.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="b0915-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="b0915-430">*Otwieranie strony zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="b0915-430">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="b0915-431">W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="b0915-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0915-432">*Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web Azure dla poszczególnych metod włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="b0915-433">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="b0915-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="b0915-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="b0915-435">![Pobieranie witryny sieci web profil publikowania](build-restful-apis-with-aspnet-web-api/_static/image45.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="b0915-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="b0915-436">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="b0915-436">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="b0915-437">Pobierz plik profilu publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="b0915-438">Dodatkowo w tym ćwiczeniu zostanie zobaczysz, jak publikować aplikację sieci web na platformie Azure z programu Visual Studio za pomocą tego pliku.</span><span class="sxs-lookup"><span data-stu-id="b0915-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="b0915-439">![Zapisywanie pliku profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image46.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="b0915-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="b0915-440">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="b0915-440">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="b0915-441">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="b0915-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="b0915-442">Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="b0915-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="b0915-443">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="b0915-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="b0915-444">Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b0915-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="b0915-445">Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania platformy Azure w **baz danych Sql** | **serwerów** | **pulpitu nawigacyjnego serwera**.</span><span class="sxs-lookup"><span data-stu-id="b0915-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="b0915-446">Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="b0915-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="b0915-447">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne.</span><span class="sxs-lookup"><span data-stu-id="b0915-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="b0915-448">Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="b0915-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="b0915-449">![Pulpit nawigacyjny z serwera bazy danych SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="b0915-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="b0915-450">*Pulpit nawigacyjny z serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="b0915-450">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="b0915-451">W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="b0915-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="b0915-452">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="b0915-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Dodawanie adresu IP klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="b0915-454">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="b0915-454">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="b0915-455">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="b0915-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdź zmiany](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="b0915-457">*Potwierdź zmiany*</span><span class="sxs-lookup"><span data-stu-id="b0915-457">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b0915-458">Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="b0915-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="b0915-459">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b0915-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="b0915-460">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b0915-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="b0915-461">![Publikowanie aplikacji](build-restful-apis-with-aspnet-web-api/_static/image51.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="b0915-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="b0915-462">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="b0915-462">*Publishing the web site*</span></span>
2. <span data-ttu-id="b0915-463">Importuj profil publikowania, zapisaną w pierwszym zadaniem.</span><span class="sxs-lookup"><span data-stu-id="b0915-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="b0915-464">![Trwa importowanie profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image52.png "importowania profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="b0915-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="b0915-465">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="b0915-465">*Importing publish profile*</span></span>
3. <span data-ttu-id="b0915-466">Kliknij przycisk **sprawdzanie poprawności połączenia**.</span><span class="sxs-lookup"><span data-stu-id="b0915-466">Click **Validate Connection**.</span></span> <span data-ttu-id="b0915-467">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b0915-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0915-468">Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.</span><span class="sxs-lookup"><span data-stu-id="b0915-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="b0915-469">![Sprawdzanie poprawności połączenia](build-restful-apis-with-aspnet-web-api/_static/image53.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="b0915-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="b0915-470">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="b0915-470">*Validating connection*</span></span>
4. <span data-ttu-id="b0915-471">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="b0915-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="b0915-472">![Konfiguracja narzędzia Web deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="b0915-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="b0915-473">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="b0915-473">*Web deploy configuration*</span></span>
5. <span data-ttu-id="b0915-474">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b0915-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="b0915-475">W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="b0915-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="b0915-476">W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="b0915-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="b0915-477">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="b0915-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="b0915-478">Na przykład wpisz nazwę nowej bazy danych: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="b0915-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="b0915-479">![Konfigurowanie parametrów połączenia z docelowym](build-restful-apis-with-aspnet-web-api/_static/image55.png "Konfigurowanie parametrów połączenia z docelowym")</span><span class="sxs-lookup"><span data-stu-id="b0915-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="b0915-480">*Konfigurowanie parametrów połączenia z docelowym*</span><span class="sxs-lookup"><span data-stu-id="b0915-480">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="b0915-481">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0915-481">Then click **OK**.</span></span> <span data-ttu-id="b0915-482">Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.</span><span class="sxs-lookup"><span data-stu-id="b0915-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="b0915-483">![Tworzenie bazy danych](build-restful-apis-with-aspnet-web-api/_static/image56.png "Tworzenie parametrów bazy danych")</span><span class="sxs-lookup"><span data-stu-id="b0915-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="b0915-484">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="b0915-484">*Creating the database*</span></span>
7. <span data-ttu-id="b0915-485">Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne.</span><span class="sxs-lookup"><span data-stu-id="b0915-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="b0915-486">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="b0915-486">Then click **Next**.</span></span>

    <span data-ttu-id="b0915-487">![Ciąg połączenia wskazujący bazę danych SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "ciąg połączenia wskazujący bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="b0915-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="b0915-488">*Ciąg połączenia wskazujący bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="b0915-488">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="b0915-489">W **Podgląd** kliknij **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b0915-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="b0915-490">![Publikowanie aplikacji sieci web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="b0915-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="b0915-491">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="b0915-491">*Publishing the web application*</span></span>
9. <span data-ttu-id="b0915-492">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="b0915-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="b0915-493">![Aplikacja została opublikowana na platformie Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplikacja została opublikowana na platformie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b0915-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="b0915-494">*Aplikacja opublikowana na platformie Azure*</span><span class="sxs-lookup"><span data-stu-id="b0915-494">*Application published to Azure*</span></span>
