---
uid: visual-studio/overview/2013/using-browser-link
title: Korzystanie z linku przeglądarki w Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622907"
---
# <a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="a846e-102">Korzystanie z linku przeglądarki w Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a846e-102">Using Browser Link in Visual Studio 2013</span></span>

<span data-ttu-id="a846e-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a846e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a846e-104">Link do przeglądarki to nowa funkcja w Visual Studio 2013, która tworzy kanał komunikacyjny między środowiskiem deweloperskim i jedną lub wieloma przeglądarkami sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a846e-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="a846e-105">Możesz użyć linku przeglądarki, aby odświeżyć aplikację sieci Web w kilku przeglądarkach jednocześnie, co jest przydatne w przypadku testowania między przeglądarkami.</span><span class="sxs-lookup"><span data-stu-id="a846e-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="a846e-106">Odświeżanie przeglądarki</span><span class="sxs-lookup"><span data-stu-id="a846e-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="a846e-107">Wyświetlanie pulpitu nawigacyjnego linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="a846e-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="a846e-108">Włączanie linku przeglądarki dla statycznych plików HTML</span><span class="sxs-lookup"><span data-stu-id="a846e-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="a846e-109">Wyłączanie linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="a846e-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="a846e-110">Jak to działa?</span><span class="sxs-lookup"><span data-stu-id="a846e-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="a846e-111">Odświeżanie przeglądarki</span><span class="sxs-lookup"><span data-stu-id="a846e-111">Browser Refresh</span></span>

<span data-ttu-id="a846e-112">Po odświeżeniu przeglądarki możesz odświeżyć wiele przeglądarek, które są połączone z programem Visual Studio za pomocą linku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a846e-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="a846e-113">Aby użyć funkcji odświeżania przeglądarki, należy najpierw utworzyć aplikację ASP.NET przy użyciu dowolnego szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="a846e-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="a846e-114">Debuguj aplikację, naciskając klawisz F5 lub klikając ikonę strzałki na pasku narzędzi:</span><span class="sxs-lookup"><span data-stu-id="a846e-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="a846e-115">Możesz również użyć listy rozwijanej, aby wybrać określoną przeglądarkę do debugowania.</span><span class="sxs-lookup"><span data-stu-id="a846e-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="a846e-116">Aby debugować z wieloma przeglądarkami, wybierz pozycję **Przeglądaj za pomocą**.</span><span class="sxs-lookup"><span data-stu-id="a846e-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="a846e-117">W oknie dialogowym **Przeglądaj przy użyciu** naciśnij i przytrzymaj klawisz CTRL, aby wybrać więcej niż jedną przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="a846e-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="a846e-118">Kliknij przycisk **Przeglądaj** , aby debugować wybrane przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a846e-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="a846e-119">Łącze przeglądarki działa również w przypadku uruchamiania przeglądarki spoza programu Visual Studio i przejścia do adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a846e-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="a846e-120">Formanty łączy przeglądarki znajdują się na liście rozwijanej z ikoną okrągłą strzałką.</span><span class="sxs-lookup"><span data-stu-id="a846e-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="a846e-121">Ikona strzałki to przycisk **Odśwież** .</span><span class="sxs-lookup"><span data-stu-id="a846e-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="a846e-122">Aby sprawdzić, które przeglądarki są połączone, umieść kursor myszy nad przyciskiem **odświeżania** podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="a846e-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="a846e-123">Połączone przeglądarki są wyświetlane w oknie etykietki narzędzi.</span><span class="sxs-lookup"><span data-stu-id="a846e-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="a846e-124">Aby odświeżyć połączone przeglądarki, kliknij przycisk **Odśwież** lub naciśnij klawisze CTRL + ALT + ENTER.</span><span class="sxs-lookup"><span data-stu-id="a846e-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="a846e-125">Na przykład poniższy zrzut ekranu przedstawia projekt ASP.NET, który został utworzony przy użyciu szablonu projektu MVC 5.</span><span class="sxs-lookup"><span data-stu-id="a846e-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="a846e-126">Możesz zobaczyć, że aplikacja działa w dwóch przeglądarkach u góry.</span><span class="sxs-lookup"><span data-stu-id="a846e-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="a846e-127">W dolnej części projektu jest otwarty w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a846e-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="a846e-128">W programie Visual Studio zmieniono nagłówek &lt;H1&gt; dla strony głównej:</span><span class="sxs-lookup"><span data-stu-id="a846e-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="a846e-129">Po kliknięciu przycisku **Odśwież** zmiany pojawiły się w obu oknach przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="a846e-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="a846e-130">**Uwagi**</span><span class="sxs-lookup"><span data-stu-id="a846e-130">**Notes**</span></span>

- <span data-ttu-id="a846e-131">Aby włączyć łącze przeglądarki, ustaw `debug=true` w elemencie [&lt;kompilacja&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) w pliku Web. config projektu.</span><span class="sxs-lookup"><span data-stu-id="a846e-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="a846e-132">Aplikacja musi być uruchomiona na hoście lokalnym.</span><span class="sxs-lookup"><span data-stu-id="a846e-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="a846e-133">Aplikacja musi być ukierunkowana na platformę .NET 4,0 lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="a846e-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="a846e-134">Wyświetlanie pulpitu nawigacyjnego linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="a846e-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="a846e-135">Pulpit nawigacyjny linku przeglądarki pokazuje informacje o połączeniach z przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="a846e-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="a846e-136">Aby wyświetlić pulpit nawigacyjny, wybierz menu rozwijane łącze przeglądarki (mała strzałka obok przycisku **odświeżania** ).</span><span class="sxs-lookup"><span data-stu-id="a846e-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="a846e-137">Następnie kliknij pozycję **pulpit nawigacyjny link do przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="a846e-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="a846e-138">Pulpit nawigacyjny wyświetla listę podłączonych przeglądarek i adres URL, do którego przejdzie Każda przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="a846e-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="a846e-139">Sekcja **wymagania wstępne** zawiera wszystkie kroki niezbędne do włączenia linku przeglądarki dla tego projektu.</span><span class="sxs-lookup"><span data-stu-id="a846e-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="a846e-140">Na przykład poniższy zrzut ekranu przedstawia projekt, w którym w pliku Web. config jest ustawiony parametr "debug" na wartość false.</span><span class="sxs-lookup"><span data-stu-id="a846e-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="a846e-141">Włączanie linku przeglądarki dla statycznych plików HTML</span><span class="sxs-lookup"><span data-stu-id="a846e-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="a846e-142">Aby włączyć łącze przeglądarki dla statycznych plików HTML, Dodaj następujące elementy do pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="a846e-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="a846e-143">Ze względu na wydajność Usuń to ustawienie przy publikowaniu projektu.</span><span class="sxs-lookup"><span data-stu-id="a846e-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="a846e-144">Wyłączanie linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="a846e-144">Disabling Browser Link</span></span>

<span data-ttu-id="a846e-145">Link przeglądarki jest domyślnie włączony.</span><span class="sxs-lookup"><span data-stu-id="a846e-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="a846e-146">Istnieje kilka sposobów jego wyłączenia:</span><span class="sxs-lookup"><span data-stu-id="a846e-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="a846e-147">W menu rozwijanym łącze przeglądarki usuń zaznaczenie pola wyboru **Włącz łącze przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="a846e-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="a846e-148">W pliku Web. config Dodaj klucz o nazwie "vs: EnableBrowserLink" o wartości "false" w sekcji appSettings.</span><span class="sxs-lookup"><span data-stu-id="a846e-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="a846e-149">W pliku Web. config Ustaw debugowanie na false.</span><span class="sxs-lookup"><span data-stu-id="a846e-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="a846e-150">Jak to działa?</span><span class="sxs-lookup"><span data-stu-id="a846e-150">How Does It Work?</span></span>

<span data-ttu-id="a846e-151">Link przeglądarki używa programu [sygnalizującego](../../../signalr/index.md) do utworzenia kanału komunikacyjnego między programem Visual Studio i przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="a846e-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="a846e-152">Gdy łącze przeglądarki jest włączone, program Visual Studio działa jako serwer sygnalizujący, z którym może się łączyć wielu klientów (przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="a846e-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="a846e-153">Link przeglądarki rejestruje także moduł HTTP z ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a846e-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="a846e-154">Ten moduł wprowadza specjalne odwołania &lt;skryptu&gt; do każdego żądania strony z serwera.</span><span class="sxs-lookup"><span data-stu-id="a846e-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="a846e-155">Odwołania do skryptów można zobaczyć, wybierając pozycję "Wyświetl Źródło" w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="a846e-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="a846e-156">Pliki źródłowe nie są modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="a846e-156">Your source files are not modified.</span></span> <span data-ttu-id="a846e-157">Moduł HTTP wprowadza odwołania do skryptów dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="a846e-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="a846e-158">Ponieważ kod po stronie przeglądarki jest językiem JavaScript, działa on we wszystkich przeglądarkach, które [obsługuje program sygnalizujący](../../../signalr/overview/getting-started/supported-platforms.md), bez konieczności używania wtyczki przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a846e-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
