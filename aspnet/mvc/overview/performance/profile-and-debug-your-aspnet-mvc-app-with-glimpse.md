---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilowanie i debugowanie aplikacji ASP.NET MVC przy użyciu możliwość wypróbowania innowacyjnego | Microsoft Docs
author: Rick-Anderson
description: Możliwość wypróbowania innowacyjnego to rozwijana i rosnąca rodzina pakietów NuGet typu open source, które udostępniają szczegółowe informacje o wydajności, debugowaniu i diagnostyki dla ASP.NET a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457664"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="51178-103">Profilowanie i debugowanie aplikacji ASP.NET MVC za pomocą pakietów Glimpse</span><span class="sxs-lookup"><span data-stu-id="51178-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>

<span data-ttu-id="51178-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="51178-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="51178-105">Możliwość wypróbowania innowacyjnego to rozwijana i rosnąca rodzina pakietów NuGet typu open source, które udostępniają szczegółowe informacje o wydajności, debugowaniu i diagnostyki dla aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51178-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="51178-106">Jest to bardzo proste Instalowanie, lekkie, niezwykle szybkie i wyświetlane kluczowe metryki wydajności w dolnej części każdej strony.</span><span class="sxs-lookup"><span data-stu-id="51178-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="51178-107">Pozwala to na przechodzenie do aplikacji w celu sprawdzenia, co dzieje się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="51178-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="51178-108">Usługa możliwość wypróbowania innowacyjnego zapewnia dużo cennych informacji, zalecamy korzystanie z nich w całym cyklu programistycznym, w tym w środowisku testowym platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="51178-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="51178-109">Podczas gdy [programu Fiddler](http://www.telerik.com/fiddler) i [Narzędzia deweloperskie F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) zapewniają widok po stronie klienta, możliwość wypróbowania innowacyjnego udostępnia szczegółowy widok z serwera.</span><span class="sxs-lookup"><span data-stu-id="51178-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="51178-110">Ten samouczek koncentruje się na korzystaniu z pakietów MVC i EF możliwość wypróbowania innowacyjnego ASP.NET, ale wiele innych pakietów jest dostępnych.</span><span class="sxs-lookup"><span data-stu-id="51178-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="51178-111">Gdzie to możliwe, połączymy się z odpowiednimi [dokumentami możliwość wypróbowania innowacyjnego](http://getglimpse.com/Docs/) , które ułatwiają konserwację.</span><span class="sxs-lookup"><span data-stu-id="51178-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="51178-112">Możliwość wypróbowania innowacyjnego to projekt open source, który może być również współautorem kodu źródłowego i dokumentów.</span><span class="sxs-lookup"><span data-stu-id="51178-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>

- [<span data-ttu-id="51178-113">Instalowanie możliwość wypróbowania innowacyjnego</span><span class="sxs-lookup"><span data-stu-id="51178-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="51178-114">Włącz możliwość wypróbowania innowacyjnego dla hosta lokalnego</span><span class="sxs-lookup"><span data-stu-id="51178-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="51178-115">Karta oś czasu</span><span class="sxs-lookup"><span data-stu-id="51178-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="51178-116">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="51178-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="51178-117">Trasy</span><span class="sxs-lookup"><span data-stu-id="51178-117">Routes</span></span>](#route)
- [<span data-ttu-id="51178-118">Korzystanie z programu możliwość wypróbowania innowacyjnego na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="51178-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="51178-119">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="51178-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="51178-120">Instalowanie możliwość wypróbowania innowacyjnego</span><span class="sxs-lookup"><span data-stu-id="51178-120">Installing Glimpse</span></span>

<span data-ttu-id="51178-121">Program możliwość wypróbowania innowacyjnego można zainstalować z konsoli Menedżera pakietów NuGet lub z konsoli **Zarządzanie pakietami NuGet** .</span><span class="sxs-lookup"><span data-stu-id="51178-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="51178-122">W tej wersji demonstracyjnej zainstaluję pakiety Mvc5 i EF6:</span><span class="sxs-lookup"><span data-stu-id="51178-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Zainstaluj możliwość wypróbowania innowacyjnego z poziomu narzędzia NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="51178-124">Wyszukaj *możliwość wypróbowania innowacyjnego. Ef*</span><span class="sxs-lookup"><span data-stu-id="51178-124">Search for *Glimpse.EF*</span></span>

![Możliwość wypróbowania innowacyjnego. EF z poziomu instalacji NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="51178-126">Wybierając **zainstalowane pakiety**, można zobaczyć zainstalowane moduły zależne możliwość wypróbowania innowacyjnego:</span><span class="sxs-lookup"><span data-stu-id="51178-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Zainstalowano pakiety możliwość wypróbowania innowacyjnego z poziomu okno dialogowe](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="51178-128">Następujące polecenia instalują moduły możliwość wypróbowania innowacyjnego MVC5 i EF6 z konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="51178-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="51178-129">Włącz możliwość wypróbowania innowacyjnego dla hosta lokalnego</span><span class="sxs-lookup"><span data-stu-id="51178-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="51178-130">Przejdź do http://localhost:&lt&gt;;p wartość/Glimpse.axd, a następnie kliknij przycisk <strong>Włącz możliwość wypróbowania innowacyjnego on</strong> .</span><span class="sxs-lookup"><span data-stu-id="51178-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Strona możliwość wypróbowania innowacyjnego AXD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="51178-132">Jeśli masz wyświetlany pasek ulubionych, możesz przeciągnąć i upuścić przyciski możliwość wypróbowania innowacyjnego i dodać je jako Bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="51178-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Program IE z możliwość wypróbowania innowacyjnego Bookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="51178-134">Teraz możesz nawigować po swojej aplikacji, a w dolnej części strony zostanie wyświetlony **ekran** (HUD).</span><span class="sxs-lookup"><span data-stu-id="51178-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Strona menedżera kontaktów z HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="51178-136">Na [stronie możliwość WYPRÓBOWANIA innowacyjnego HUD](http://getglimpse.com/Docs/Heads-up-Display) szczegółowo przedstawiono informacje o chronometrażu.</span><span class="sxs-lookup"><span data-stu-id="51178-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="51178-137">Niezauważalne dane wydajności wyświetlane przez HUD mogą natychmiast powiadomić użytkownika o problemie — przed przejściem do cyklu testowego.</span><span class="sxs-lookup"><span data-stu-id="51178-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="51178-138">Kliknięcie&quot; &quot;g w prawym dolnym rogu spowoduje wyświetlenie panelu możliwość wypróbowania innowacyjnego:</span><span class="sxs-lookup"><span data-stu-id="51178-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Panel możliwość wypróbowania innowacyjnego](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="51178-140">Na powyższym obrazie zostanie wybrana [karta wykonywanie](http://getglimpse.com/Docs/Execution-Tab) , która wyświetla szczegóły czasu akcji i filtrów w potoku.</span><span class="sxs-lookup"><span data-stu-id="51178-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="51178-141">[Zegar filtru Zatrzymaj czujkę](http://www.nuget.org/packages/StopWatch/) można zobaczyć na stronie etap 6 potoku.</span><span class="sxs-lookup"><span data-stu-id="51178-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="51178-142">Gdy mój czasomierz wagi uproszczonej może zapewnić przydatne dane dotyczące profilu/czasu, nie wszystkie czasu poświęcają na autoryzację i renderowanie widoku.</span><span class="sxs-lookup"><span data-stu-id="51178-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="51178-143">Informacje o moim czasomierzu można odczytać w [profilu i w czasie, gdy aplikacja ASP.NET MVC jest w całości na platformie Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="51178-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="51178-144">Strona [karty](http://getglimpse.com/Docs/Tabs) zawiera linki do szczegółowych informacji na poszczególnych kartach.</span><span class="sxs-lookup"><span data-stu-id="51178-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="51178-145">Karta oś czasu</span><span class="sxs-lookup"><span data-stu-id="51178-145">The Timeline tab</span></span>

<span data-ttu-id="51178-146">Po zmodyfikowaniu pozostałego [samouczka Dr 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) z następującej zmiany kodu do kontrolera instruktorów:</span><span class="sxs-lookup"><span data-stu-id="51178-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="51178-147">Powyższy kod umożliwia przekazywanie danych w ciągu zapytania (`eager`) w celu kontrolowania eager lub jawnego ładowania dane.</span><span class="sxs-lookup"><span data-stu-id="51178-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="51178-148">Na poniższym obrazie jest używane jawne ładowanie, a strona chronometrażu przedstawia każdą rejestrację załadowana w `Index`ej metodzie działania:</span><span class="sxs-lookup"><span data-stu-id="51178-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![jawne ładowanie](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="51178-150">W poniższym kodzie określono eager i każda Rejestracja jest pobierana po wywołaniu widoku `Index`:</span><span class="sxs-lookup"><span data-stu-id="51178-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager jest określony](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="51178-152">Możesz umieścić wskaźnik myszy nad segmentem czasu, aby uzyskać szczegółowe informacje o chronometrażu:</span><span class="sxs-lookup"><span data-stu-id="51178-152">You can hover over a time segment to get detailed timing information:</span></span>

![Aktywuj wskaźnik, aby wyświetlić szczegółowy chronometraż](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="51178-154">Powiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="51178-154">Model Binding</span></span>

<span data-ttu-id="51178-155">[Karta powiązanie modelu](http://getglimpse.com/Docs/Model-Binding-Tab) zawiera mnóstwo informacji, które pomogą zrozumieć, w jaki sposób zmienne formularzy są powiązane i dlaczego niektóre nie są związane z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="51178-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="51178-156">Na poniższej ilustracji przedstawiono **?**</span><span class="sxs-lookup"><span data-stu-id="51178-156">The image below shows the **?**</span></span> <span data-ttu-id="51178-157">ikona, którą można kliknąć, aby wyświetlić stronę pomocy możliwość wypróbowania innowacyjnego dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="51178-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Widok powiązania modelu możliwość wypróbowania innowacyjnego](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="51178-159">Trasy</span><span class="sxs-lookup"><span data-stu-id="51178-159">Routes</span></span>

 <span data-ttu-id="51178-160">Karta trasy możliwość wypróbowania innowacyjnego może pomóc w debugowaniu i zrozumieniu routingu.</span><span class="sxs-lookup"><span data-stu-id="51178-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="51178-161">Na poniższej ilustracji wybrano trasę produktu (i jest ona wyświetlana w kolorze zielonym, Konwencja możliwość wypróbowania innowacyjnego).</span><span class="sxs-lookup"><span data-stu-id="51178-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="51178-162">![wybraną nazwę produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) ograniczenia trasy, obszary i tokeny danych są również wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="51178-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="51178-163">Aby uzyskać więcej informacji, zobacz [trasy możliwość wypróbowania innowacyjnego](http://getglimpse.com/Docs/Routes-Tab) i [Routing atrybutów w ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) .</span><span class="sxs-lookup"><span data-stu-id="51178-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="51178-164">Korzystanie z programu możliwość wypróbowania innowacyjnego na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="51178-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="51178-165">Domyślne zasady zabezpieczeń możliwość wypróbowania innowacyjnego umożliwiają wyświetlanie danych możliwość wypróbowania innowacyjnego z hosta lokalnego.</span><span class="sxs-lookup"><span data-stu-id="51178-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="51178-166">Te zasady zabezpieczeń można zmienić, aby można było wyświetlać te dane na serwerze zdalnym (na przykład w aplikacji sieci Web na platformie Azure).</span><span class="sxs-lookup"><span data-stu-id="51178-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="51178-167">W przypadku środowisk testowych na platformie Azure Dodaj wyróżniony znacznik do dolnej części pliku *Web. config* , aby włączyć możliwość wypróbowania innowacyjnego:</span><span class="sxs-lookup"><span data-stu-id="51178-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.config* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="51178-168">W przypadku tej zmiany każdy użytkownik może zobaczyć dane możliwość wypróbowania innowacyjnego w zdalnej witrynie.</span><span class="sxs-lookup"><span data-stu-id="51178-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="51178-169">Rozważ dodanie znacznika powyżej do profilu publikowania, aby został wdrożony tylko podczas korzystania z tego profilu publikowania (na przykład profilu testu platformy Azure). Aby ograniczyć możliwość wypróbowania innowacyjnego dane, dodamy rolę `canViewGlimpseData` i zezwolisz użytkownikom z tej roli na wyświetlanie danych możliwość wypróbowania innowacyjnego.</span><span class="sxs-lookup"><span data-stu-id="51178-169">Consider adding the markup above to a publish profile so it's only deployed an applied when you use that publish profile (for example, your Azure test profile.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="51178-170">Usuń Komentarze z pliku *GlimpseSecurityPolicy.cs* i Zmień wywołanie [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) z `Administrator` na rolę `canViewGlimpseData`:</span><span class="sxs-lookup"><span data-stu-id="51178-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="51178-171">Zabezpieczenia — bogate dane udostępniane przez możliwość wypróbowania innowacyjnego mogą uwidaczniać bezpieczeństwo aplikacji.</span><span class="sxs-lookup"><span data-stu-id="51178-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="51178-172">Firma Microsoft nie wykonała inspekcji zabezpieczeń możliwość wypróbowania innowacyjnego do użycia w aplikacjach do produkcji.</span><span class="sxs-lookup"><span data-stu-id="51178-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>

<span data-ttu-id="51178-173">Aby uzyskać informacje na temat dodawania ról, zobacz moje [wdrożenie bezpiecznego ASP.NET MVC 5 aplikacji sieci Web z członkostwem, uwierzytelnianiem OAuth i SQL Database w samouczku platformy Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .</span><span class="sxs-lookup"><span data-stu-id="51178-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="51178-174">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="51178-174">Additional Resources</span></span>

- [<span data-ttu-id="51178-175">Wdrażanie bezpiecznej aplikacji ASP.NET MVC 5 z członkostwem, uwierzytelnianiem OAuth i SQL Database na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="51178-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="51178-176">[Konfiguracja możliwość wypróbowania innowacyjnego](http://getglimpse.com/Docs/Configuration) — Strona doc na temat konfigurowania kart, zasad środowiska uruchomieniowego, rejestrowania i nie tylko.</span><span class="sxs-lookup"><span data-stu-id="51178-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
