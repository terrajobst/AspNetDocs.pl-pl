---
uid: single-page-application/overview/templates/hottowel-template
title: Szablon gorąca ręczników | Microsoft Docs
author: madskristensen
description: Szablon HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075063"
---
# <a name="hot-towel-template"></a><span data-ttu-id="375a8-103">Szablon Hot Towel</span><span class="sxs-lookup"><span data-stu-id="375a8-103">Hot Towel template</span></span>

<span data-ttu-id="375a8-104">Autor [produktywność Madsa Kristensena](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="375a8-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="375a8-105">Szablon Hot ręczników MVC jest zapisywana przez Jan Papa</span><span class="sxs-lookup"><span data-stu-id="375a8-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="375a8-106">Wybierz wersję do pobrania:</span><span class="sxs-lookup"><span data-stu-id="375a8-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="375a8-107">Szablon ręczników MVC dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="375a8-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="375a8-108">Szablon Hot ręczników MVC dla Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="375a8-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="375a8-109">Gorąca ręczników: ponieważ nie chcesz przejść do SPA bez użycia</span><span class="sxs-lookup"><span data-stu-id="375a8-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>

<span data-ttu-id="375a8-110">Chcesz skompilować SPA, ale nie możesz zdecydować, gdzie zacząć?</span><span class="sxs-lookup"><span data-stu-id="375a8-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="375a8-111">Używaj gorącej ręczników i w ciągu kilku sekund będziesz mieć SPA i wszystkie narzędzia potrzebne do kompilowania na nim!</span><span class="sxs-lookup"><span data-stu-id="375a8-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="375a8-112">Gorąca ręczników tworzy doskonały punkt wyjścia do tworzenia aplikacji jednostronicowej (SPA) z ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="375a8-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="375a8-113">Poza ramką udostępniamy modularną strukturę kodu, nawigację w widoku, powiązanie danych, zaawansowane zarządzanie danymi oraz proste, ale elegancki styl.</span><span class="sxs-lookup"><span data-stu-id="375a8-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="375a8-114">Gorąca ręczników zapewnia wszystko, co jest potrzebne do utworzenia SPA, dzięki czemu możesz skupić się na aplikacji, a nie na instalacji wodociągowej.</span><span class="sxs-lookup"><span data-stu-id="375a8-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="375a8-115">Dowiedz się więcej na temat tworzenia SPA z [filmów wideo, samouczków i kursów Pluralsight firmy John Papa](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="375a8-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>

## <a name="application-structure"></a><span data-ttu-id="375a8-116">Struktura aplikacji</span><span class="sxs-lookup"><span data-stu-id="375a8-116">Application Structure</span></span>

<span data-ttu-id="375a8-117">Hot ręczników SPA udostępnia folder aplikacji, który zawiera pliki JavaScript i HTML, które definiują aplikację.</span><span class="sxs-lookup"><span data-stu-id="375a8-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="375a8-118">W folderze aplikacji:</span><span class="sxs-lookup"><span data-stu-id="375a8-118">Inside the App folder:</span></span>

- <span data-ttu-id="375a8-119">durandal</span><span class="sxs-lookup"><span data-stu-id="375a8-119">durandal</span></span>
- <span data-ttu-id="375a8-120">services</span><span class="sxs-lookup"><span data-stu-id="375a8-120">services</span></span>
- <span data-ttu-id="375a8-121">modele widoków</span><span class="sxs-lookup"><span data-stu-id="375a8-121">viewmodels</span></span>
- <span data-ttu-id="375a8-122">widoki</span><span class="sxs-lookup"><span data-stu-id="375a8-122">views</span></span>

<span data-ttu-id="375a8-123">Folder App zawiera kolekcję modułów.</span><span class="sxs-lookup"><span data-stu-id="375a8-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="375a8-124">Te moduły hermetyzują funkcjonalność i deklarują zależności w innych modułach.</span><span class="sxs-lookup"><span data-stu-id="375a8-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="375a8-125">Folder widoki zawiera kod HTML dla aplikacji, a folder modele widoków zawiera logikę prezentacji dla widoków (wspólny wzorzec MVVM).</span><span class="sxs-lookup"><span data-stu-id="375a8-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="375a8-126">Folder usługi doskonale nadaje się do przechowywania wszelkich typowych usług, których może potrzebować aplikacja, takich jak pobieranie danych HTTP lub interakcja z magazynem lokalnym.</span><span class="sxs-lookup"><span data-stu-id="375a8-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="375a8-127">Często wiele modele widoków umożliwia ponowne użycie kodu z modułów usług.</span><span class="sxs-lookup"><span data-stu-id="375a8-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="375a8-128">Struktura aplikacji po stronie serwera ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="375a8-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="375a8-129">Gorąca ręczników jest oparta na znanej i zaawansowanej strukturze ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="375a8-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="375a8-130">Rozpoczęcie\_aplikacji</span><span class="sxs-lookup"><span data-stu-id="375a8-130">App\_Start</span></span>
- <span data-ttu-id="375a8-131">Zawartość</span><span class="sxs-lookup"><span data-stu-id="375a8-131">Content</span></span>
- <span data-ttu-id="375a8-132">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="375a8-132">Controllers</span></span>
- <span data-ttu-id="375a8-133">Modele</span><span class="sxs-lookup"><span data-stu-id="375a8-133">Models</span></span>
- <span data-ttu-id="375a8-134">Scripts</span><span class="sxs-lookup"><span data-stu-id="375a8-134">Scripts</span></span>
- <span data-ttu-id="375a8-135">Widoki</span><span class="sxs-lookup"><span data-stu-id="375a8-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="375a8-136">Polecane biblioteki</span><span class="sxs-lookup"><span data-stu-id="375a8-136">Featured Libraries</span></span>

- <span data-ttu-id="375a8-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="375a8-137">ASP.NET MVC</span></span>
- <span data-ttu-id="375a8-138">Internetowy interfejs API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="375a8-138">ASP.NET Web API</span></span>
- <span data-ttu-id="375a8-139">ASP.NET optymalizacja sieci Web — dzielenie i minifikacja</span><span class="sxs-lookup"><span data-stu-id="375a8-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="375a8-140">[Breeze. js](http://Breezejs.com) — zaawansowane zarządzanie danymi</span><span class="sxs-lookup"><span data-stu-id="375a8-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="375a8-141">[Durandal. js](http://Durandaljs.com) — tworzenie nawigacji i wyświetlania</span><span class="sxs-lookup"><span data-stu-id="375a8-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="375a8-142">[Odcinanie. js](http://Knockoutjs.com) — powiązania danych</span><span class="sxs-lookup"><span data-stu-id="375a8-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="375a8-143">[Wymagaj. js](http://requirejs.org) — modularność z AMD i optymalizacją</span><span class="sxs-lookup"><span data-stu-id="375a8-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="375a8-144">Wyskakujące okienko [. js](http://jpapa.me/c7toastr) — komunikaty podręczne</span><span class="sxs-lookup"><span data-stu-id="375a8-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="375a8-145">Zaawansowane ustawianie stylów CSS w usłudze [Twitter](https://twitter.github.com/bootstrap/)</span><span class="sxs-lookup"><span data-stu-id="375a8-145">[Twitter Bootstrap](https://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="375a8-146">Instalowanie za pomocą szablonu programu Visual Studio 2012 gorąca ręczników SPA</span><span class="sxs-lookup"><span data-stu-id="375a8-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="375a8-147">Gorącą ręczników można zainstalować jako szablon programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="375a8-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="375a8-148">Po prostu kliknij pozycję `File` | `New Project` i wybierz pozycję `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="375a8-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="375a8-149">Następnie wybierz szablon "aplikacja jednostronicowa na gorąco ręczników" i uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="375a8-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="375a8-150">Instalowanie za pośrednictwem pakietu NuGet</span><span class="sxs-lookup"><span data-stu-id="375a8-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="375a8-151">Gorąca ręczników jest również pakietem NuGet, który rozszerza istniejący pusty projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="375a8-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="375a8-152">Po prostu zainstaluj program NuGet, a następnie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="375a8-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="375a8-153">Jak kompilować na gorącą ręczników?</span><span class="sxs-lookup"><span data-stu-id="375a8-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="375a8-154">Po prostu zacznij dodawać kod!</span><span class="sxs-lookup"><span data-stu-id="375a8-154">Simply start adding code!</span></span>

1. <span data-ttu-id="375a8-155">Dodaj swój własny kod po stronie serwera, najlepiej Entity Framework i WebAPI (który naprawdę ma Breeze. js)</span><span class="sxs-lookup"><span data-stu-id="375a8-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="375a8-156">Dodawanie widoków do folderu `App/views`</span><span class="sxs-lookup"><span data-stu-id="375a8-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="375a8-157">Dodawanie modele widoków do folderu `App/viewmodels`</span><span class="sxs-lookup"><span data-stu-id="375a8-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="375a8-158">Dodawanie powiązań HTML i odcinania danych do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="375a8-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="375a8-159">Aktualizowanie tras nawigacji w `shell.js`</span><span class="sxs-lookup"><span data-stu-id="375a8-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="375a8-160">Przewodnik po kodzie HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="375a8-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="375a8-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="375a8-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="375a8-162">index. cshtml to początkowa trasa i widok dla aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="375a8-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="375a8-163">Zawiera wszystkie standardowe Tagi meta, linki CSS i odwołania do języka JavaScript, których oczekujesz.</span><span class="sxs-lookup"><span data-stu-id="375a8-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="375a8-164">Treść zawiera pojedynczy `<div>`, w którym zostanie umieszczona cała zawartość (widoki), gdy zostanie ona zażądana.</span><span class="sxs-lookup"><span data-stu-id="375a8-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="375a8-165">`@Scripts.Render` używa programu Wymagaj. js do uruchomienia punktu wejścia dla kodu aplikacji, który znajduje się w pliku `main.js`.</span><span class="sxs-lookup"><span data-stu-id="375a8-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="375a8-166">Zostanie dostarczony ekran powitalny pokazujący, jak utworzyć ekran powitalny podczas ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="375a8-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="375a8-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="375a8-167">App/main.js</span></span>

<span data-ttu-id="375a8-168">Plik `main.js` zawiera kod, który zostanie uruchomiony zaraz po załadowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="375a8-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="375a8-169">Jest to miejsce, w którym chcesz zdefiniować trasy nawigacji, ustawić widoki uruchamiania i wykonać dowolne instalacji/uruchomienia, takie jak napełnianiu danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="375a8-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="375a8-170">Plik `main.js` definiuje kilka modułów Durandal, aby ułatwić rozpoczęcie uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="375a8-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="375a8-171">Instrukcja define pomaga rozpoznać zależności modułów, aby były dostępne dla funkcji.</span><span class="sxs-lookup"><span data-stu-id="375a8-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="375a8-172">Najpierw są włączone komunikaty debugowania, które wysyłają komunikaty o zdarzeniach wykonywanych przez aplikację w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="375a8-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="375a8-173">Kod App. Start mówi Durandal Framework, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="375a8-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="375a8-174">Konwencje są ustawiane tak, aby Durandal wie, że wszystkie widoki i modele widoków są zawarte w tych samych nazwanych folderach, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="375a8-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="375a8-175">Na koniec `app.setRoot` rozpoczyna ładowanie `shell` przy użyciu wstępnie zdefiniowanej animacji `entrance`.</span><span class="sxs-lookup"><span data-stu-id="375a8-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="375a8-176">Widoki</span><span class="sxs-lookup"><span data-stu-id="375a8-176">Views</span></span>

<span data-ttu-id="375a8-177">Widoki znajdują się w folderze `App/views`.</span><span class="sxs-lookup"><span data-stu-id="375a8-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="375a8-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="375a8-178">shell.html</span></span>

<span data-ttu-id="375a8-179">`shell.html` zawiera układ wzorca dla kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="375a8-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="375a8-180">Wszystkie inne widoki będą składać się gdzieś na stronie widoku `shell`.</span><span class="sxs-lookup"><span data-stu-id="375a8-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="375a8-181">Gorąca ręczników zapewnia `shell` z trzema takimi regionami: nagłówkiem, obszarem zawartości i stopką.</span><span class="sxs-lookup"><span data-stu-id="375a8-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="375a8-182">Każdy z tych regionów jest ładowany wraz z zawartością w innych widokach, gdy jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="375a8-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="375a8-183">`compose` powiązania dla nagłówka i stopki są twarde kodowane w gorącą ręczników, aby wskazywały odpowiednio `nav` i `footer` widoków.</span><span class="sxs-lookup"><span data-stu-id="375a8-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="375a8-184">Powiązanie redagowania dla sekcji `#content` jest powiązane z aktywnym elementem modułu `router`.</span><span class="sxs-lookup"><span data-stu-id="375a8-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="375a8-185">Innymi słowy po kliknięciu linku nawigacyjnego w tym obszarze jest ładowany odpowiedni widok.</span><span class="sxs-lookup"><span data-stu-id="375a8-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="375a8-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="375a8-186">nav.html</span></span>

<span data-ttu-id="375a8-187">`nav.html` zawiera linki nawigacji dla SPA.</span><span class="sxs-lookup"><span data-stu-id="375a8-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="375a8-188">Jest to miejsce, w którym można na przykład umieścić strukturę menu.</span><span class="sxs-lookup"><span data-stu-id="375a8-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="375a8-189">Często jest to powiązane z danymi (za pomocą odcinania) do modułu `router`, aby wyświetlić nawigację zdefiniowane w `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="375a8-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="375a8-190">Odcinanie szuka atrybutów powiązania danych i wiąże je z `shell` ViewModel, aby wyświetlić trasy nawigacji i wyświetlić element ProgressBar (przy użyciu Bootstrap usługi Twitter), jeśli moduł `router` jest w trakcie przechodzenia z jednego widoku do innego (zobacz `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="375a8-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="375a8-191">Home. html i details. html</span><span class="sxs-lookup"><span data-stu-id="375a8-191">home.html and details.html</span></span>

<span data-ttu-id="375a8-192">Te widoki zawierają kod HTML dla widoków niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="375a8-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="375a8-193">Po kliknięciu linku `home` w menu Widok `nav` zostanie umieszczony widok `home` w obszarze zawartość widoku `shell`.</span><span class="sxs-lookup"><span data-stu-id="375a8-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="375a8-194">Te widoki można rozszerzyć lub zastąpić własnymi widokami niestandardowymi.</span><span class="sxs-lookup"><span data-stu-id="375a8-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="375a8-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="375a8-195">footer.html</span></span>

<span data-ttu-id="375a8-196">`footer.html` zawiera kod HTML, który pojawia się w stopce w dolnej części widoku `shell`.</span><span class="sxs-lookup"><span data-stu-id="375a8-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="375a8-197">Modele widoków</span><span class="sxs-lookup"><span data-stu-id="375a8-197">ViewModels</span></span>

<span data-ttu-id="375a8-198">Modele widoków znajdują się w folderze `App/viewmodels`.</span><span class="sxs-lookup"><span data-stu-id="375a8-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="375a8-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="375a8-199">shell.js</span></span>

<span data-ttu-id="375a8-200">ViewModel `shell` zawiera właściwości i funkcje, które są powiązane z widokiem `shell`.</span><span class="sxs-lookup"><span data-stu-id="375a8-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="375a8-201">Często jest to miejsce, w którym znajdują się powiązania nawigacji menu (patrz logika `router.mapNav`).</span><span class="sxs-lookup"><span data-stu-id="375a8-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="375a8-202">Home. js i details. js</span><span class="sxs-lookup"><span data-stu-id="375a8-202">home.js and details.js</span></span>

<span data-ttu-id="375a8-203">Te modele widoków zawierają właściwości i funkcje, które są powiązane z widokiem `home`.</span><span class="sxs-lookup"><span data-stu-id="375a8-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="375a8-204">zawiera również logikę prezentacji dla widoku i jest przyklejana między danymi i widokiem.</span><span class="sxs-lookup"><span data-stu-id="375a8-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="375a8-205">Usługi</span><span class="sxs-lookup"><span data-stu-id="375a8-205">Services</span></span>

<span data-ttu-id="375a8-206">Usługi są dostępne w folderze aplikacji/usług.</span><span class="sxs-lookup"><span data-stu-id="375a8-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="375a8-207">W idealnym przypadku można umieścić w przyszłości usługi, takie jak moduł DataService, odpowiedzialne za pobieranie i publikowanie danych zdalnych.</span><span class="sxs-lookup"><span data-stu-id="375a8-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="375a8-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="375a8-208">logger.js</span></span>

<span data-ttu-id="375a8-209">Gorąca ręczników udostępnia moduł `logger` w folderze Services.</span><span class="sxs-lookup"><span data-stu-id="375a8-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="375a8-210">Moduł `logger` jest idealnym rozwiązaniem w przypadku rejestrowania komunikatów w konsoli programu i dla użytkownika w oknie wyskakujących okienek.</span><span class="sxs-lookup"><span data-stu-id="375a8-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
