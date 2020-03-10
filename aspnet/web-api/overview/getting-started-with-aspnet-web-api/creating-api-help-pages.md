---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Tworzenie stron pomocy dla ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Ten samouczek z kodem pokazuje, jak utworzyć strony pomocy dla interfejsu API sieci Web ASP.NET w ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556876"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="26034-103">Tworzenie stron pomocy dla interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="26034-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="26034-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="26034-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="26034-105">Ten samouczek z kodem pokazuje, jak utworzyć strony pomocy dla interfejsu API sieci Web ASP.NET w ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="26034-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="26034-106">Podczas tworzenia interfejsu API sieci Web często warto utworzyć stronę pomocy, aby inni deweloperzy wiedzieli, jak wywołać interfejs API.</span><span class="sxs-lookup"><span data-stu-id="26034-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="26034-107">Wszystkie te dokumenty można utworzyć ręcznie, ale lepiej jest generować automatyczne generowanie tak dużo jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="26034-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="26034-108">Aby to zadanie było prostsze, interfejs API sieci Web ASP.NET udostępnia bibliotekę do autogeneracji stron pomocy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="26034-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="26034-109">Tworzenie stron pomocy interfejsu API</span><span class="sxs-lookup"><span data-stu-id="26034-109">Creating API Help Pages</span></span>

<span data-ttu-id="26034-110">Zainstaluj [aktualizację ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="26034-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="26034-111">Ta aktualizacja integruje strony pomocy z szablonem projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="26034-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="26034-112">Następnie utwórz nowy projekt ASP.NET MVC 4 i wybierz szablon projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="26034-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="26034-113">Szablon projektu tworzy przykładowy kontroler interfejsu API o nazwie `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="26034-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="26034-114">Szablon tworzy również strony pomocy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="26034-114">The template also creates the API help pages.</span></span> <span data-ttu-id="26034-115">Wszystkie pliki kodu dla strony pomocy są umieszczane w folderze obszarów projektu.</span><span class="sxs-lookup"><span data-stu-id="26034-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="26034-116">Po uruchomieniu aplikacji Strona główna zawiera link do strony pomocy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="26034-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="26034-117">Ze strony głównej ścieżką względną jest/help.</span><span class="sxs-lookup"><span data-stu-id="26034-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="26034-118">Ten link umożliwia przełączenie do strony podsumowania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="26034-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="26034-119">Widok MVC dla tej strony jest zdefiniowany w obszarze Areas/HelpPage/Viewss/help/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="26034-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="26034-120">Możesz edytować Tę stronę, aby modyfikować układ, wprowadzenie, tytuł, style i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="26034-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="26034-121">Główna część strony jest tabelą interfejsów API pogrupowanych według kontrolera.</span><span class="sxs-lookup"><span data-stu-id="26034-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="26034-122">Wpisy tabeli są generowane dynamicznie przy użyciu interfejsu **IApiExplorer** .</span><span class="sxs-lookup"><span data-stu-id="26034-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="26034-123">(Dowiesz się więcej o tym interfejsie później). W przypadku dodania nowego kontrolera interfejsu API tabela zostanie automatycznie zaktualizowana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="26034-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="26034-124">Kolumna "API" zawiera listę metod HTTP i względnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="26034-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="26034-125">Kolumna "Description" zawiera dokumentację dla każdego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="26034-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="26034-126">Początkowo dokumentacja jest tylko tekstem zastępczym.</span><span class="sxs-lookup"><span data-stu-id="26034-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="26034-127">W następnej sekcji pokażemy, jak dodać dokumentację z komentarzy do kodu XML.</span><span class="sxs-lookup"><span data-stu-id="26034-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="26034-128">Każdy interfejs API ma link do strony z bardziej szczegółowymi informacjami, w tym przykładowe treści żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="26034-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="26034-129">Dodawanie stron pomocy do istniejącego projektu</span><span class="sxs-lookup"><span data-stu-id="26034-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="26034-130">Możesz dodawać strony pomocy do istniejącego projektu interfejsu API sieci Web za pomocą Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="26034-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="26034-131">Ta opcja jest przydatna, rozpoczynając od innego szablonu projektu niż szablon "Web API".</span><span class="sxs-lookup"><span data-stu-id="26034-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="26034-132">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="26034-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="26034-133">W oknie [konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) wpisz jedno z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="26034-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="26034-134">Dla **C#** aplikacji: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="26034-134">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="26034-135">W przypadku aplikacji **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="26034-135">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="26034-136">Istnieją dwa pakiety, jeden dla C# i jeden dla Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="26034-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="26034-137">Upewnij się, że używasz tego, który jest zgodny z projektem.</span><span class="sxs-lookup"><span data-stu-id="26034-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="26034-138">To polecenie instaluje niezbędne zestawy i dodaje widoki MVC dla stron pomocy (znajdujących się w folderze Areas/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="26034-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="26034-139">Musisz ręcznie dodać link do strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="26034-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="26034-140">Identyfikator URI to/help.</span><span class="sxs-lookup"><span data-stu-id="26034-140">The URI is /Help.</span></span> <span data-ttu-id="26034-141">Aby utworzyć łącze w widoku Razor, Dodaj następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="26034-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="26034-142">Upewnij się również, że zarejestrowano obszary.</span><span class="sxs-lookup"><span data-stu-id="26034-142">Also, make sure to register areas.</span></span> <span data-ttu-id="26034-143">W pliku Global. asax Dodaj następujący kod do **aplikacji\_Start** , jeśli jeszcze nie istnieje:</span><span class="sxs-lookup"><span data-stu-id="26034-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="26034-144">Dodawanie dokumentacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="26034-144">Adding API Documentation</span></span>

<span data-ttu-id="26034-145">Domyślnie strony pomocy mają ciągi zastępcze dla dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="26034-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="26034-146">Możesz użyć [komentarzy dokumentacji XML](https://msdn.microsoft.com/library/b2s063f7.aspx) , aby utworzyć dokumentację.</span><span class="sxs-lookup"><span data-stu-id="26034-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="26034-147">Aby włączyć tę funkcję, Otwórz obszary pliku/HelpPage/App\_Start/HelpPageConfig. cs i Usuń komentarz z następującego wiersza:</span><span class="sxs-lookup"><span data-stu-id="26034-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="26034-148">Teraz Włącz dokumentację XML.</span><span class="sxs-lookup"><span data-stu-id="26034-148">Now enable XML documentation.</span></span> <span data-ttu-id="26034-149">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="26034-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="26034-150">Wybierz stronę **kompilacja** .</span><span class="sxs-lookup"><span data-stu-id="26034-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="26034-151">W obszarze **dane wyjściowe**Sprawdź **plik dokumentacji XML**.</span><span class="sxs-lookup"><span data-stu-id="26034-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="26034-152">W polu edycji wpisz "App\_Data/XmlDocument. xml".</span><span class="sxs-lookup"><span data-stu-id="26034-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="26034-153">Następnie otwórz kod dla `ValuesController` kontrolera interfejsu API, który jest zdefiniowany w/Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="26034-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="26034-154">Dodaj komentarze dokumentacji do metod kontrolera.</span><span class="sxs-lookup"><span data-stu-id="26034-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="26034-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26034-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="26034-156">Porada: Jeśli umieścisz karetkę nad linią powyżej metody i wpiszesz trzy ukośniki, program Visual Studio automatycznie wstawi elementy XML.</span><span class="sxs-lookup"><span data-stu-id="26034-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="26034-157">Następnie można wypełnić puste.</span><span class="sxs-lookup"><span data-stu-id="26034-157">Then you can fill in the blanks.</span></span>

<span data-ttu-id="26034-158">Teraz Skompiluj i ponownie uruchom aplikację, a następnie przejdź do stron pomocy.</span><span class="sxs-lookup"><span data-stu-id="26034-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="26034-159">Ciągi dokumentacji powinny znajdować się w tabeli interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="26034-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="26034-160">Strona pomoc odczytuje ciągi z pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="26034-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="26034-161">(Podczas wdrażania aplikacji upewnij się, że plik XML został wdrożony).</span><span class="sxs-lookup"><span data-stu-id="26034-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="26034-162">Pod okapem</span><span class="sxs-lookup"><span data-stu-id="26034-162">Under the Hood</span></span>

<span data-ttu-id="26034-163">Strony pomocy są zbudowane na podstawie klasy **ApiExplorer** , która jest częścią struktury internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="26034-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="26034-164">Klasa **ApiExplorer** dostarcza surowy materiał do tworzenia strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="26034-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="26034-165">Dla każdego interfejsu API **ApiExplorer** zawiera **ApiDescription** , który opisuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="26034-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="26034-166">W tym celu interfejs "API" jest definiowany jako kombinacja metody HTTP i względnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="26034-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="26034-167">Na przykład Oto kilka różnych interfejsów API:</span><span class="sxs-lookup"><span data-stu-id="26034-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="26034-168">Pobierz/api/Products</span><span class="sxs-lookup"><span data-stu-id="26034-168">GET /api/Products</span></span>
- <span data-ttu-id="26034-169">Pobierz/api/Products/{id}</span><span class="sxs-lookup"><span data-stu-id="26034-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="26034-170">Opublikuj/api/Products</span><span class="sxs-lookup"><span data-stu-id="26034-170">POST /api/Products</span></span>

<span data-ttu-id="26034-171">Jeśli akcja kontrolera obsługuje wiele metod HTTP, **ApiExplorer** traktuje każdą metodę jako odrębny interfejs API.</span><span class="sxs-lookup"><span data-stu-id="26034-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="26034-172">Aby ukryć interfejs API z **ApiExplorer**, Dodaj atrybut **ApiExplorerSettings** do akcji i ustaw *IgnoreApi* na true.</span><span class="sxs-lookup"><span data-stu-id="26034-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="26034-173">Możesz również dodać ten atrybut do kontrolera, aby wykluczyć cały kontroler.</span><span class="sxs-lookup"><span data-stu-id="26034-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="26034-174">Klasa ApiExplorer pobiera ciągi dokumentacji z interfejsu **IDocumentationProvider** .</span><span class="sxs-lookup"><span data-stu-id="26034-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="26034-175">Jak pokazano wcześniej, biblioteka stron pomocy zawiera **IDocumentationProvider** , który pobiera dokumentację z CIĄGÓW dokumentacji XML.</span><span class="sxs-lookup"><span data-stu-id="26034-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="26034-176">Kod znajduje się w/Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="26034-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="26034-177">Możesz uzyskać dokumentację z innego źródła, pisząc własne **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="26034-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="26034-178">Aby je połączyć, wywołaj metodę rozszerzenia **SetDocumentationProvider** , która została zdefiniowana w **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="26034-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="26034-179">**ApiExplorer** automatycznie wywołuje interfejs **IDocumentationProvider** w celu pobrania ciągów dokumentacji dla każdego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="26034-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="26034-180">Przechowuje je we właściwości **Dokumentacja** obiektów **ApiDescription** i **ApiParameterDescription** .</span><span class="sxs-lookup"><span data-stu-id="26034-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="26034-181">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="26034-181">Next Steps</span></span>

<span data-ttu-id="26034-182">Nie masz ograniczeń do stron pomocy widocznych tutaj.</span><span class="sxs-lookup"><span data-stu-id="26034-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="26034-183">W rzeczywistości **ApiExplorer** nie jest ograniczony do tworzenia stron pomocy.</span><span class="sxs-lookup"><span data-stu-id="26034-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="26034-184">Yao Huang, który zapisał kilka doskonałych wpisów w blogu, aby dowiedzieć się, jak to zrobić:</span><span class="sxs-lookup"><span data-stu-id="26034-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="26034-185">Dodawanie prostego klienta testowego do strony pomocy interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="26034-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="26034-186">Tworzenie strony pomocy interfejsu API sieci Web ASP.NET w usługach samoobsługowych</span><span class="sxs-lookup"><span data-stu-id="26034-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="26034-187">Generowanie w czasie projektowania strony pomocy (lub klienta) dla interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="26034-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="26034-188">Zaawansowane dostosowania stron pomocy</span><span class="sxs-lookup"><span data-stu-id="26034-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
