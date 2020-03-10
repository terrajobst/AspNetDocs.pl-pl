---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Korzystanie z narzędzia Page Inspector dla programu Visual Studio 2012 w ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Page Inspector for Visual Studio 2012 to narzędzie deweloperskie dla sieci Web, które jest zintegrowane z przeglądarką. Zaznacz dowolny element w zintegrowanej przeglądarce i inspektor strony...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575923"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="b79a1-104">Korzystanie z narzędzia Page Inspector dla programu Visual Studio 2012 we wzorcu ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="b79a1-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>

<span data-ttu-id="b79a1-105">Autor Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="b79a1-105">by Tim Ammann</span></span>

> <span data-ttu-id="b79a1-106">Page Inspector for Visual Studio 2012 to narzędzie deweloperskie dla sieci Web, które jest zintegrowane z przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="b79a1-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="b79a1-107">Zaznacz dowolny element w zintegrowanej przeglądarce, a Inspektor strony natychmiast podświetla źródło i kod CSS elementu.</span><span class="sxs-lookup"><span data-stu-id="b79a1-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="b79a1-108">Możesz przeglądać dowolną stronę w aplikacji, szybko znajdować źródła renderowanego znacznika i korzystać z narzędzi przeglądarki bezpośrednio w środowisku programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b79a1-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="b79a1-109">W tym samouczku przedstawiono sposób włączania trybu inspekcji, a następnie szybkiego lokalizowania i edytowania reguł CSS i tekstu w projekcie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b79a1-109">This tutorial shows how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="b79a1-110">W tym samouczku jest używany projekt aplikacji formularzy sieci Web, ale można również użyć Inspektora stron dla projektów witryny sieci Web i aplikacji [MVC](https://go.microsoft.com/?linkid=9802002) .</span><span class="sxs-lookup"><span data-stu-id="b79a1-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="b79a1-111">Samouczek zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="b79a1-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="b79a1-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b79a1-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="b79a1-113">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="b79a1-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="b79a1-114">Korzystanie z Inspektora stron do wyświetlania aplikacji</span><span class="sxs-lookup"><span data-stu-id="b79a1-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="b79a1-115">Włącz tryb inspekcji</span><span class="sxs-lookup"><span data-stu-id="b79a1-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="b79a1-116">Używanie inspektora strony do wprowadzania zmian w znacznikach</span><span class="sxs-lookup"><span data-stu-id="b79a1-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="b79a1-117">Tryb inspekcji i okno HTML</span><span class="sxs-lookup"><span data-stu-id="b79a1-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="b79a1-118">Podgląd zmian CSS w oknie stylów</span><span class="sxs-lookup"><span data-stu-id="b79a1-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="b79a1-119">Autosynchronizacja CSS</span><span class="sxs-lookup"><span data-stu-id="b79a1-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="b79a1-120">Korzystanie z selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="b79a1-120">Using the CSS Color Picker</span></span>](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="b79a1-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b79a1-121">Prerequisites</span></span>

- <span data-ttu-id="b79a1-122">[Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="b79a1-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="b79a1-123">Aby uzyskać najnowszą wersję inspektora stron, Zainstaluj zestaw Azure SDK dla programu .NET 2,0 przy użyciu [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) .</span><span class="sxs-lookup"><span data-stu-id="b79a1-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="b79a1-124">Narzędzia Page Inspector są powiązane z Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="b79a1-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="b79a1-125">Najnowsza wersja to 1,3.</span><span class="sxs-lookup"><span data-stu-id="b79a1-125">The latest version is 1.3.</span></span> <span data-ttu-id="b79a1-126">Aby sprawdzić posiadaną wersję, uruchom program Visual Studio i wybierz pozycję **Informacje o Microsoft Visual Studio** z menu **Pomoc** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="b79a1-127">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="b79a1-127">Create a Web Application</span></span>

<span data-ttu-id="b79a1-128">Najpierw utworzysz aplikację sieci Web, która będzie używana w programie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b79a1-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="b79a1-129">W programie Visual Studio wybierz kolejno pozycje **plik** &gt; **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="b79a1-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="b79a1-130">Po lewej stronie rozwiń pozycję **C#Wizualizacja**, wybierz pozycję **Sieć Web**, a następnie wybierz pozycję **aplikacja formularzy sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="b79a1-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nowa aplikacja formularzy sieci Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="b79a1-132">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b79a1-132">Click **OK**.</span></span>

<span data-ttu-id="b79a1-133">Aplikacja zostanie otwarta w widoku **źródła** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-133">The application opens in **Source** view.</span></span>

![Nowa aplikacja formularzy sieci Web w widoku źródła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="b79a1-135">Teraz, gdy masz aplikację do współpracy z programem, możesz użyć Inspektora stron do jego sprawdzenia i modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="b79a1-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="b79a1-136">Korzystanie z Inspektora stron do wyświetlania aplikacji</span><span class="sxs-lookup"><span data-stu-id="b79a1-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="b79a1-137">Następnie zobaczysz aplikację za pomocą Inspektora stron.</span><span class="sxs-lookup"><span data-stu-id="b79a1-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="b79a1-138">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Widok w Inspektorze strony**.</span><span class="sxs-lookup"><span data-stu-id="b79a1-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Wyświetl w Inspektorze strony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="b79a1-140">Domyślnie, gdy inspektor strony jest uruchamiany po raz pierwszy, jest zadokowany jako wąskie okno po lewej stronie środowiska programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b79a1-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="b79a1-141">Pozostaw to pole zadokowane po lewej stronie i ustaw dla niego szerokość, która jest odpowiednia dla Ciebie, lub Zadokuj ją w jednym z obszarów narzędzi na górze, na dole lub w prawo:</span><span class="sxs-lookup"><span data-stu-id="b79a1-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Położenie dokowania inspektora stron](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="b79a1-143">Jeśli oddokusz okno Inspektora stron, możesz je umieścić poza programem Visual Studio, a nawet na drugim monitorze, jeśli go masz.</span><span class="sxs-lookup"><span data-stu-id="b79a1-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="b79a1-144">Jednak aby można było ALT + TAB między inspektorem strony a programem Visual Studio, gdy okno Inspektora strony jest odzadokowane, przejdź do pozycji **narzędzia** &gt; **opcje** &gt; **środowisko** &gt; **karty i okna** **, a**następnie usuń zaznaczenie pola wyboru nazwanego okna **narzędziowego zawsze na wierzchu okna głównego**:</span><span class="sxs-lookup"><span data-stu-id="b79a1-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Wyczyść pole wyboru przepływającego okna narzędzi, aby ALT + TAB między programem Visual Studio i niezadokowanym oknem inspektora strony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="b79a1-146">Górne okienko okna inspektora stron pokazuje bieżącą stronę w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b79a1-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="b79a1-147">W dolnym okienku zostanie wyświetlona strona w znaczniku HTML po lewej stronie, a niektóre karty po prawej stronie umożliwiają sprawdzenie różnych aspektów strony.</span><span class="sxs-lookup"><span data-stu-id="b79a1-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="b79a1-148">Dolne okienko jest podobne do [Narzędzia deweloperskie F12](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="b79a1-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="b79a1-149">(Jednak w przeciwieństwie do narzędzi deweloperskich możesz użyć Inspektora stron bezpośrednio w programie Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="b79a1-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![{1&gt;Inspektor strony&lt;1}](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="b79a1-151">W tym samouczku użyjesz okienka przeglądarki stron oraz kart **HTML** i **Style** , które ułatwią Ci szybkie nawigowanie i wprowadzanie zmian w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79a1-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="b79a1-152">Włącz tryb inspekcji</span><span class="sxs-lookup"><span data-stu-id="b79a1-152">Enable Inspection Mode</span></span>

<span data-ttu-id="b79a1-153">Następnie zobaczysz, jak działa tryb inspekcji inspektora strony.</span><span class="sxs-lookup"><span data-stu-id="b79a1-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="b79a1-154">W oknie Inspektora stron kliknij przycisk **Zbadaj** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Zbadaj element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="b79a1-156">Aby wyświetlić tryb inspekcji w akcji, Przenieś wskaźnik myszy na różne części strony w oknie przeglądarki strony.</span><span class="sxs-lookup"><span data-stu-id="b79a1-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="b79a1-157">Jak to zrobisz, wskaźnik myszy zmieni się na duży znak plus, a element poniżej zostanie wyróżniony:</span><span class="sxs-lookup"><span data-stu-id="b79a1-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Przesuwanie nad blokiem DIV. Content-otoka](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="b79a1-159">Podczas przesuwania wskaźnika myszy należy zauważyć, że</span><span class="sxs-lookup"><span data-stu-id="b79a1-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="b79a1-160">Zawartość w widoku **źródła** zmienia się, aby wyświetlić znaczniki odpowiadające wybranemu elementowi na stronie.</span><span class="sxs-lookup"><span data-stu-id="b79a1-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="b79a1-161">Odpowiednie znaczniki są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="b79a1-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="b79a1-162">Jeśli źródło znajduje się w innym pliku, plik zostanie otwarty w widoku źródła z wyróżnionym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="b79a1-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="b79a1-163">Znaczniki wyświetlane na karcie **HTML** w Inspektorze strony również zmieniają się, aby odpowiadały wybranemu elementowi na stronie.</span><span class="sxs-lookup"><span data-stu-id="b79a1-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="b79a1-164">Na karcie **HTML** odpowiednie znaczniki są opisane w opisie.</span><span class="sxs-lookup"><span data-stu-id="b79a1-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="b79a1-165">Na karcie **Style** są wyświetlane reguły CSS odpowiednie dla bieżącego zaznaczenia.</span><span class="sxs-lookup"><span data-stu-id="b79a1-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="b79a1-166">Używanie inspektora strony do wprowadzania zmian w znacznikach</span><span class="sxs-lookup"><span data-stu-id="b79a1-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="b79a1-167">Teraz zobaczysz, jak można użyć Inspektora strony do znajdowania i wprowadzania zmian w znacznikach lub tekstach, których lokalizacja może nie być od razu oczywista.</span><span class="sxs-lookup"><span data-stu-id="b79a1-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="b79a1-168">Umieść Inspektor strony w trybie inspekcji, a następnie przewiń w dół strony głównej.</span><span class="sxs-lookup"><span data-stu-id="b79a1-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="b79a1-169">Gdy tylko wprowadzisz obszar stopki, inspektor strony otworzy plik *. Master* w widoku **źródła** na karcie tymczasowej z prawej strony innych kart i podświetla wybraną sekcję na stronie wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="b79a1-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="b79a1-170">Pokazuje, w jaki sposób Inspektor strony może znaleźć i wyświetlić zawartość na stronie, która może faktycznie pochodzić z innego pliku niż ten, który został pierwotnie otwarty.</span><span class="sxs-lookup"><span data-stu-id="b79a1-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Najważniejsze stopki w trybie inspekcji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="b79a1-172">W oknie przeglądarki stron inspektora przesuń wskaźnik myszy nad wiersz z informacją o prawach <a id="a"> </a>autorskich.</span><span class="sxs-lookup"><span data-stu-id="b79a1-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="b79a1-173">Na stronie *site. Master* zostanie wyróżniony odpowiedni wiersz.</span><span class="sxs-lookup"><span data-stu-id="b79a1-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Wyróżniono wiersz Copyright](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="b79a1-175">Dodaj tekst na końcu wiersza w pliku *site. Master* .</span><span class="sxs-lookup"><span data-stu-id="b79a1-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="b79a1-176">&lt;p&gt;&amp;Copy; &lt;%: DateTime. Now. Year%&gt;-my ASP.NET Application Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="b79a1-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="b79a1-177">Teraz naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij pasek aktualizacji, aby wyświetlić wyniki w oknie przeglądarki stron inspektora.</span><span class="sxs-lookup"><span data-stu-id="b79a1-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Moja aplikacja ASP.NET Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="b79a1-179">Być może wiesz, że stopka znajdowała się na stronie *default. aspx* , ale wyłączono ją na stronie układu głównego, a Inspektor strony go wyszukał.</span><span class="sxs-lookup"><span data-stu-id="b79a1-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="b79a1-180">Tryb inspekcji i okno HTML</span><span class="sxs-lookup"><span data-stu-id="b79a1-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="b79a1-181">Następnie będziesz mieć szybki podgląd okna HTML i sposobu, w jaki mapuje elementy.</span><span class="sxs-lookup"><span data-stu-id="b79a1-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="b79a1-182">Umieść Inspektor strony w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="b79a1-182">Put Page Inspector in Inspection Mode.</span></span>

![Zbadaj element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="b79a1-184">Kliknij górną część strony, w której znajduje się tekst "Twoje logo".</span><span class="sxs-lookup"><span data-stu-id="b79a1-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="b79a1-185">Badasz konkretny element bardziej szczegółowo, więc wyświetlanie w oknie przeglądarki nie zmienia się po przesunięciu wskaźnika myszy.</span><span class="sxs-lookup"><span data-stu-id="b79a1-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="b79a1-186">Teraz Przenieś wskaźnik myszy do okna **HTML** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="b79a1-187">Podczas przesuwania wskaźnika myszy, inspektor strony zawiera element w oknie **HTML** i podświetla odpowiadający element w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b79a1-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Okno HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="b79a1-189">Jak wcześniej, inspektor strony otworzy plik *site. Master* dla Ciebie na karcie tymczasowej. Kliknij kartę site. Master, a odpowiednie znaczniki są wyróżnione w sekcji &lt;nagłówek&gt;:</span><span class="sxs-lookup"><span data-stu-id="b79a1-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Wyróżnione adiustacje](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="b79a1-191">Podgląd zmian CSS w oknie stylów</span><span class="sxs-lookup"><span data-stu-id="b79a1-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="b79a1-192">Następnie zobaczysz, jak można użyć okna **stylów** inspektora strony do podglądu zmian w CSS.</span><span class="sxs-lookup"><span data-stu-id="b79a1-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="b79a1-193">Kliknij przycisk **Sprawdź** , aby umieścić inspektora strony w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="b79a1-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b79a1-194">W oknie przeglądarki stron inspektora przesuń wskaźnik myszy nad sekcję "Strona główna", aż zostanie wyświetlona etykieta **DIV. Content-otoka** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Przesuwanie nad elementami](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="b79a1-196">Kliknij jeden z sekcji DIV. Content-otoka, a następnie przesuń wskaźnik myszy do okna **Style** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="b79a1-197">W selektorze klas. Polecane. Content-otoka Usuń zaznaczenie pola wyboru dla właściwości Background-Color.</span><span class="sxs-lookup"><span data-stu-id="b79a1-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Wyczyść kolor tła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="b79a1-199">Zwróć uwagę, jak natychmiast przeglądanie zmian w oknie przeglądarki stron.</span><span class="sxs-lookup"><span data-stu-id="b79a1-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="b79a1-200">Zaznacz pole wyboru ponownie, a następnie kliknij dwukrotnie wartość właściwości i zmień ją na `red`.</span><span class="sxs-lookup"><span data-stu-id="b79a1-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="b79a1-201">Zmiana zostanie wyświetlona natychmiast:</span><span class="sxs-lookup"><span data-stu-id="b79a1-201">The change shows immediately:</span></span>

![Czerwony kolor tła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="b79a1-203">Okno **Style** ułatwia testowanie i Podgląd zmian CSS przed zatwierdzeniem zmian w arkuszu stylów.</span><span class="sxs-lookup"><span data-stu-id="b79a1-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="b79a1-204">Autosynchronizacja CSS</span><span class="sxs-lookup"><span data-stu-id="b79a1-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="b79a1-205">Ta funkcja wymaga wersji 1,3 inspektora strony.</span><span class="sxs-lookup"><span data-stu-id="b79a1-205">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="b79a1-206">Funkcja autosynchronizacji CSS umożliwia bezpośrednie edytowanie pliku CSS i natychmiastowe wyświetlanie zmian w przeglądarce stron.</span><span class="sxs-lookup"><span data-stu-id="b79a1-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="b79a1-207">Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="b79a1-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b79a1-208">W przeglądarce inspektora stron przesuń wskaźnik myszy nad sekcję "Strona główna" do momentu, gdy zostanie wyświetlona etykieta **DIV. Content-otoka** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="b79a1-209">Kliknij raz, aby zaznaczyć ten element.</span><span class="sxs-lookup"><span data-stu-id="b79a1-209">Click once to select this element.</span></span>

<span data-ttu-id="b79a1-210">W oknie **Style** są wyświetlane wszystkie reguły CSS dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="b79a1-210">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="b79a1-211">Przewiń w dół, aby znaleźć selektor klasy. Polecane. Content-otoka.</span><span class="sxs-lookup"><span data-stu-id="b79a1-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="b79a1-212">Kliknij pozycję ". Polecane. Content-otoka".</span><span class="sxs-lookup"><span data-stu-id="b79a1-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="b79a1-213">Inspektor strony otwiera plik CSS, który definiuje ten styl (site. CSS) i podświetla odpowiedni styl CSS.</span><span class="sxs-lookup"><span data-stu-id="b79a1-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Plik CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="b79a1-215">Teraz zmień wartość dla `background-color` na "Red".</span><span class="sxs-lookup"><span data-stu-id="b79a1-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="b79a1-216">Zmiana zostanie wyświetlona natychmiast w przeglądarce inspektora stron.</span><span class="sxs-lookup"><span data-stu-id="b79a1-216">The change appears immediately in the Page Inspector browser.</span></span>

![Przeglądarka stron inspektora](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="b79a1-218">Korzystanie z selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="b79a1-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="b79a1-219">Następnie dowiesz się, jak za pomocą Inspektora stron szybko znajdować i zmieniać arkusz CSS dla wyróżnionego tekstu w aplikacji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="b79a1-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="b79a1-220">W tym przykładzie postanowisz, że nie jest to niebieskie wyróżnianie i chcesz zmienić kolor na inny.</span><span class="sxs-lookup"><span data-stu-id="b79a1-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="b79a1-221">Kliknij przycisk **Zbadaj** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-221">Click the **Inspect** button.</span></span>

![Zbadaj element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="b79a1-223">W oknie przeglądarki stron inspektora przesuń wskaźnik myszy nad wyróżniony tekst "wideo, samouczki i przykłady", aby pojawił się etykieta CSS "Mark".</span><span class="sxs-lookup"><span data-stu-id="b79a1-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Przesuwanie nad elementem znacznika](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="b79a1-225">Kliknij tekst, aby go zaznaczyć.</span><span class="sxs-lookup"><span data-stu-id="b79a1-225">Click the text to select it.</span></span> <span data-ttu-id="b79a1-226">Odpowiedni selektor znacznika CSS pojawia się u dołu okna **Style** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Oznacz selektor w oknie stylów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="b79a1-228">Kliknij selektor znacznika.</span><span class="sxs-lookup"><span data-stu-id="b79a1-228">Click the mark selector.</span></span> <span data-ttu-id="b79a1-229">Spowoduje to otwarcie pliku *. css* dla aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b79a1-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="b79a1-230">Kliknij kartę site. CSS, a odpowiadająca jej CSS selektora zostanie wyróżniona:</span><span class="sxs-lookup"><span data-stu-id="b79a1-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Oznacz selektor w arkuszu stylów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="b79a1-232">Zaznacz i Usuń wiersz z właściwością Background-Color.</span><span class="sxs-lookup"><span data-stu-id="b79a1-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="b79a1-233">Teraz będziesz używać nowego selektora kolorów CSS programu Visual Studio 2012, aby wybrać nowy kolor dla właściwości tło **znacznika** .</span><span class="sxs-lookup"><span data-stu-id="b79a1-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="b79a1-234">Korzystanie z selektora kolorów CSS programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b79a1-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="b79a1-235">Edytor CSS w programie Visual Studio 2012 ma selektor kolorów, dzięki czemu można łatwo wybierać i wstawiać kolory.</span><span class="sxs-lookup"><span data-stu-id="b79a1-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="b79a1-236">Ma prosty pasek koloru i selektor "podręczny", który oferuje dokładniejszą kontrolę.</span><span class="sxs-lookup"><span data-stu-id="b79a1-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="b79a1-237">Selektor kolorów zawiera standardową paletę kolorów, obsługuje standardowe nazwy kolorów, kody skrótów, kolory RGB, RGBA, HSL i HSLA, a także zachowuje listę kolorów używanych ostatnio w dokumencie.</span><span class="sxs-lookup"><span data-stu-id="b79a1-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="b79a1-238">W wierszu, w którym zainstalowano Właściwość Background-Color, wpisz "BC" i naciśnij klawisz Strzałka w dół.</span><span class="sxs-lookup"><span data-stu-id="b79a1-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="b79a1-239">Po wpisaniu pierwszego znaku każdego wyrazu w właściwości rozdzielanej łącznikami, takiej jak "Background-Color", IntelliSense filtruje listę, aby wyświetlić tylko pasujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="b79a1-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Przefiltrowane wartości IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="b79a1-241">Teraz wpisz dwukropek.</span><span class="sxs-lookup"><span data-stu-id="b79a1-241">Now type a colon.</span></span> <span data-ttu-id="b79a1-242">Gdy to zrobisz, zostanie wstawiona pełna nazwa właściwości Background.</span><span class="sxs-lookup"><span data-stu-id="b79a1-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="b79a1-243">Typ **#** lub **RGB (** i zostanie wyświetlony pasek selektora kolorów:</span><span class="sxs-lookup"><span data-stu-id="b79a1-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Pasek selektora kolorów CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="b79a1-245">Aby zobaczyć, jak działa pasek selektora kolorów, kliknij jego kolory za pomocą wskaźnika myszy lub naciśnij klawisz Strzałka w dół, a następnie użyj klawiszy strzałek w lewo i w prawo, aby przechodzenie między kolorami.</span><span class="sxs-lookup"><span data-stu-id="b79a1-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="b79a1-246">Podczas odwiedzania koloru wyświetlana jest odpowiednia wartość właściwości Background-Color:</span><span class="sxs-lookup"><span data-stu-id="b79a1-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![Podgląd — wartość właściwości koloru w tle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="b79a1-248">W tym momencie można nacisnąć klawisz ENTER, aby wybrać wartość, a następnie średnik (;) Aby ukończyć wprowadzanie wpisów CSS.</span><span class="sxs-lookup"><span data-stu-id="b79a1-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="b79a1-249">Na razie przejdź do następnej sekcji, aby zobaczyć, jak działa selektor kolorów.</span><span class="sxs-lookup"><span data-stu-id="b79a1-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="b79a1-250">Korzystanie z okna podręcznego selektora kolorów</span><span class="sxs-lookup"><span data-stu-id="b79a1-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="b79a1-251">Gdy pasek koloru nie ma dokładnego koloru, którego szukasz, możesz użyć selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="b79a1-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="b79a1-252">Aby go otworzyć, kliknij podwójny cudzysłów ostrokątny na prawym końcu paska kolorów lub naciśnij klawisz Strzałka w dół jeden raz lub dwukrotnie na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="b79a1-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selektor kolorów CSS — wyskakujące okienko](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="b79a1-254">Kliknij kolor z pionowego paska po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="b79a1-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="b79a1-255">Spowoduje to wyświetlenie gradientu dla tego koloru w oknie głównym.</span><span class="sxs-lookup"><span data-stu-id="b79a1-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="b79a1-256">Wybierz kolor bezpośrednio z paska pionowego, naciskając klawisz ENTER, lub kliknij dowolny punkt w oknie głównym, aby wybrać większą precyzję.</span><span class="sxs-lookup"><span data-stu-id="b79a1-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="b79a1-257">Jeśli na ekranie komputera istnieje kolor, który ma być używany (nie musi znajdować się w interfejsie użytkownika programu Visual Studio), możesz przechwycić jego wartość za pomocą narzędzia Kroplomierz w prawym dolnym rogu.</span><span class="sxs-lookup"><span data-stu-id="b79a1-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="b79a1-258">Możesz również zmienić nieprzezroczystość koloru, przesuwając suwak w dolnej części selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="b79a1-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="b79a1-259">Spowoduje to zmianę wartości koloru na wartości RGBA, ponieważ format RGBA może reprezentować nieprzezroczystość.</span><span class="sxs-lookup"><span data-stu-id="b79a1-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="b79a1-260">Po wybraniu koloru naciśnij klawisz ENTER, a następnie wpisz średnik, aby zakończyć wprowadzanie koloru tła w pliku *site. css* .</span><span class="sxs-lookup"><span data-stu-id="b79a1-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="b79a1-261">Pasek aktualizacji inspektora stron</span><span class="sxs-lookup"><span data-stu-id="b79a1-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="b79a1-262">Inspektor strony natychmiast wykrywa zmiany w pliku *site. css* (lub w dowolnym pliku w aplikacji) i wyświetla alert na pasku aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="b79a1-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Pasek aktualizacji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="b79a1-264">Aby zapisać wszystkie pliki i odświeżyć przeglądarkę inspektora stron, naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij pasek aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="b79a1-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="b79a1-265">Zmiana w kolorze wyróżnienia zostanie wyświetlona w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b79a1-265">The change in the highlight color appears in the browser:</span></span>

![Zmieniono kolor wyróżnienia](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="b79a1-267">Należy zauważyć, że wygodnie odświeżono przeglądarkę inspektora stron bezpośrednio z poziomu środowiska programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b79a1-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="b79a1-268">Korzystanie z narzędzia Page Inspector zamiast zewnętrznej przeglądarki pozwala pozostać w edytorze podczas opracowywania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b79a1-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="b79a1-269">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="b79a1-269">See Also</span></span>

<span data-ttu-id="b79a1-270">[Wprowadzenie do Inspektora stron](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (wideo Channel 9)</span><span class="sxs-lookup"><span data-stu-id="b79a1-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="b79a1-271">[Komunikaty o błędach inspektora stron](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="b79a1-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
