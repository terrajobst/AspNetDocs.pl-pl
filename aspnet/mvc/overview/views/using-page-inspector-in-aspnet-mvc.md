---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Korzystanie z narzędzia Page Inspector w ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Narzędzie Page Inspector w programie Visual Studio 2012 jest narzędziem do programowania w sieci Web za pomocą zintegrowanej przeglądarki. Zaznacz dowolny element w zintegrowanej przeglądarce i inspektor strony i...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538018"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="fd4a7-104">Korzystanie z narzędzia Page Inspector we wzorcu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="fd4a7-104">Using Page Inspector in ASP.NET MVC</span></span>

<span data-ttu-id="fd4a7-105">Autor Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="fd4a7-105">by Tim Ammann</span></span>

> <span data-ttu-id="fd4a7-106">Narzędzie Page Inspector w programie Visual Studio 2012 jest narzędziem do programowania w sieci Web za pomocą zintegrowanej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="fd4a7-107">Zaznacz dowolny element w zintegrowanej przeglądarce, a Inspektor strony natychmiast podświetla źródło i kod CSS elementu.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="fd4a7-108">Możesz przeglądać dowolny widok MVC, szybko znajdować źródła renderowanego znacznika i korzystać z narzędzi przeglądarki bezpośrednio w środowisku programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="fd4a7-109">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="fd4a7-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="fd4a7-110">W tym samouczku pokazano, jak włączyć tryb inspekcji, a następnie szybko zlokalizować i edytować znaczniki i CSS w projekcie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="fd4a7-111">Samouczek używa projektu MVC, ale można również użyć Inspektora stron do [formularzy sieci Web](https://go.microsoft.com/?linkid=9802001) i innych aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="fd4a7-112">Samouczek zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="fd4a7-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="fd4a7-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fd4a7-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="fd4a7-114">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="fd4a7-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="fd4a7-115">Użyj Inspektora stron, aby przejść do widoku</span><span class="sxs-lookup"><span data-stu-id="fd4a7-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="fd4a7-116">Włącz tryb inspekcji</span><span class="sxs-lookup"><span data-stu-id="fd4a7-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="fd4a7-117">Używanie inspektora strony do wprowadzania zmian w znacznikach</span><span class="sxs-lookup"><span data-stu-id="fd4a7-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="fd4a7-118">Tryb inspekcji i okno HTML</span><span class="sxs-lookup"><span data-stu-id="fd4a7-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="fd4a7-119">Podgląd zmian CSS w oknie stylów</span><span class="sxs-lookup"><span data-stu-id="fd4a7-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="fd4a7-120">Autosynchronizacja CSS</span><span class="sxs-lookup"><span data-stu-id="fd4a7-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="fd4a7-121">Korzystanie z selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="fd4a7-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="fd4a7-122">Mapowanie dynamicznych elementów strony na JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd4a7-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="fd4a7-123">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fd4a7-123">Prerequisites</span></span>

- <span data-ttu-id="fd4a7-124">[Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="fd4a7-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="fd4a7-125">Aby uzyskać najnowszą wersję inspektora stron, użyj [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) w celu zainstalowania zestawu Windows Azure SDK dla programu .NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="fd4a7-126">Narzędzia Page Inspector są powiązane z Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="fd4a7-127">Najnowsza wersja to 1,3.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-127">The latest version is 1.3.</span></span> <span data-ttu-id="fd4a7-128">Aby sprawdzić posiadaną wersję, uruchom program Visual Studio i wybierz pozycję **Informacje o Microsoft Visual Studio** z menu **Pomoc** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="fd4a7-129">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="fd4a7-129">Create a Web Application</span></span>

<span data-ttu-id="fd4a7-130">Najpierw Utwórz aplikację sieci Web, która będzie używana w programie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="fd4a7-131">W programie Visual Studio wybierz kolejno pozycje **plik** &gt; **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="fd4a7-132">Po lewej stronie rozwiń pozycję **C#Wizualizacja**, wybierz pozycję **Sieć Web**, a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nowa aplikacja ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="fd4a7-134">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-134">Click **OK**.</span></span>

<span data-ttu-id="fd4a7-135">W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **aplikacja internetowa**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="fd4a7-136">Pozostaw **Razor** jako domyślny aparat widoku.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-136">Leave **Razor** as the default view engine.</span></span>

![Nowy projekt ASP.NET MVC — aplikacja internetowa](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="fd4a7-138">Aplikacja zostanie otwarta w widoku **źródła** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-138">The application opens in **Source** view.</span></span>

![Nowa aplikacja ASP.NET MVC w widoku źródła](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="fd4a7-140">Teraz, gdy masz aplikację do współpracy z programem, możesz użyć Inspektora stron do jego sprawdzenia i modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="fd4a7-141">Użyj Inspektora stron, aby przejść do widoku</span><span class="sxs-lookup"><span data-stu-id="fd4a7-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="fd4a7-142">W programie Visual Studio 2012 można kliknąć prawym przyciskiem myszy dowolny widok w projekcie, wybrać pozycję **Widok w Inspektorze strony**, a Inspektor strony wystawia trasę i wyświetlić stronę.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="fd4a7-143">W **Eksplorator rozwiązań**rozwiń folder **widoki** , a następnie folder **macierzysty** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="fd4a7-144">Kliknij prawym przyciskiem myszy plik index. cshtml i wybierz polecenie **Widok w Inspektorze strony**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Wyświetl index. cshtml w Inspektorze strony](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="fd4a7-146">Domyślnie Inspektor strony jest zadokowany jako okno po lewej stronie środowiska programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="fd4a7-147">Jeśli wolisz, możesz zadokować ją w innym miejscu lub oddokować okno.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="fd4a7-148">Zobacz [How to: porządkowanie i Dokowanie okien](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd4a7-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="fd4a7-149">Górne okienko okna inspektora stron pokazuje bieżącą stronę w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="fd4a7-150">W dolnym okienku zostanie wyświetlona strona znaczników HTML wraz z niektórymi kartami, które umożliwiają badanie różnych aspektów strony.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="fd4a7-151">Dolne okienko jest podobne do [Narzędzia deweloperskie F12](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Aplikacja ASP.NET MVC w Inspektorze strony](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="fd4a7-153">W tym samouczku zostaną użyte karty **HTML** i **Style** umożliwiające szybkie nawigowanie i wprowadzanie zmian w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="fd4a7-154">Tryb EnableInspection</span><span class="sxs-lookup"><span data-stu-id="fd4a7-154">EnableInspection Mode</span></span>

<span data-ttu-id="fd4a7-155">Aby umieścić inspektora stron w trybie inspekcji, kliknij przycisk **Zbadaj** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="fd4a7-156">W trybie inspekcji, gdy przetrzymasz wskaźnik myszy na dowolnej części renderowanej strony, odpowiednie oznakowanie źródłowe lub kod jest wyróżniony.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Przełącz tryb inspekcji](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="fd4a7-158">Teraz Przenieś wskaźnik myszy na różne części strony w Inspektorze strony.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="fd4a7-159">Jak to zrobisz, wskaźnik myszy zmieni się na duży znak plus, a element poniżej zostanie wyróżniony:</span><span class="sxs-lookup"><span data-stu-id="fd4a7-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Przesuwanie nad blokiem DIV. Content-otoka](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="fd4a7-161">Podczas przesuwania wskaźnika myszy program Visual Studio podświetla odpowiednie składnia Razor w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="fd4a7-162">Jeśli element HTML pochodzi z innego pliku źródłowego, program Visual Studio automatycznie otworzy ten plik.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="fd4a7-163">W Inspektorze strony karta **HTML** zawiera kod HTML, który został wygenerowany na podstawie składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="fd4a7-164">Podczas przesuwania wskaźnika myszy elementy HTML są wyróżniane.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="fd4a7-165">Na karcie **Style** są wyświetlane reguły CSS dla elementu.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="fd4a7-166">Używanie inspektora strony do wprowadzania zmian w znacznikach</span><span class="sxs-lookup"><span data-stu-id="fd4a7-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="fd4a7-167">Inspektor strony umożliwia znalezienie znaczników, których lokalizacja może nie być oczywista.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="fd4a7-168">Następnie można zmodyfikować znaczniki i zobaczyć wyniki zmian.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="fd4a7-169">Aby to sprawdzić, kliknij pozycję **Inspekcja** , a następnie przewiń w dół strony w oknie Inspektora strony.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="fd4a7-170">Po przesunięciu wskaźnika myszy do obszaru stopka, inspektor strony otwiera plik \_Layout. cshtml i podświetla sekcję strony układu, która została wybrana.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="fd4a7-171">Jak widać, stopka jest zdefiniowana w pliku układu, a nie samego widoku.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Stop](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="fd4a7-173">Teraz Przenieś wskaźnik myszy nad wiersz z informacją o prawach autorskich <a id="a"> </a>.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="fd4a7-174">Na stronie \_Layout. cshtml zostanie wyróżniony odpowiedni wiersz.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Wyróżniono wiersz Copyright](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="fd4a7-176">Dodaj tekst na końcu wiersza w pliku \_Layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="fd4a7-177">&lt;p&gt;&amp;Copy; @DateTime.Now.Year — moja aplikacja ASP.NET MVC Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="fd4a7-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="fd4a7-178">Teraz naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij pasek aktualizacji, aby wyświetlić wyniki w oknie przeglądarki stron inspektora.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Moja aplikacja ASP.NET Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="fd4a7-180">Być może wiesz, że stopka zdefiniowana w indeksie index. cshtml, ale wyłączono ją w \_Layout. cshtml, a Inspektor strony go wyszukał.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="fd4a7-181">Tryb inspekcji i okno HTML</span><span class="sxs-lookup"><span data-stu-id="fd4a7-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="fd4a7-182">Następnie będziesz mieć szybki podgląd okna HTML i sposobu, w jaki mapuje elementy.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="fd4a7-183">Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="fd4a7-184">Kliknij górną część strony, w której znajduje się tekst "Twoje logo".</span><span class="sxs-lookup"><span data-stu-id="fd4a7-184">Click the top part of the page that says "Your logo here".</span></span> <span data-ttu-id="fd4a7-185">Badasz konkretny element bardziej szczegółowo, więc wyświetlanie w oknie przeglądarki nie zmienia się po przesunięciu wskaźnika myszy.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="fd4a7-186">Teraz Przenieś wskaźnik myszy do okna **HTML** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="fd4a7-187">Podczas przesuwania wskaźnika myszy, inspektor strony zawiera element w oknie **HTML** i podświetla odpowiadający element w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Okno HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="fd4a7-189">Jak wcześniej, inspektor strony otwiera plik \_Layout. cshtml w karcie tymczasowej. Kliknij kartę tymczasową \_układ. cshtml, a odpowiednie adiustacje zostaną wyróżnione w sekcji&gt; nagłówka &lt;:</span><span class="sxs-lookup"><span data-stu-id="fd4a7-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Wyróżnione adiustacje](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="fd4a7-191">Podgląd zmian CSS w oknie stylów</span><span class="sxs-lookup"><span data-stu-id="fd4a7-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="fd4a7-192">Następnie użyjesz okna **stylów** inspektora strony do podglądu zmian w CSS.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="fd4a7-193">Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="fd4a7-194">W oknie przeglądarki stron inspektora przesuń wskaźnik myszy nad sekcję "Strona główna", aż zostanie wyświetlona etykieta **DIV. Content-otoka** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Przesuwanie nad blokiem DIV. Content-otoka](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="fd4a7-196">Kliknij jeden z sekcji DIV. Content-otoka, a następnie przesuń wskaźnik myszy do okna **Style** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="fd4a7-197">W oknie **Style** są wyświetlane wszystkie reguły CSS dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-197">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="fd4a7-198">Przewiń w dół, aby znaleźć selektor klasy. Polecane. Content-otoka.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="fd4a7-199">Teraz wyczyść pole wyboru dla właściwości Background-Color.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-199">Now clear the checkbox for the background-color property.</span></span>

![Wyczyść kolor tła](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="fd4a7-201">Zwróć uwagę, jak natychmiast przeglądanie zmian w oknie przeglądarki stron.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="fd4a7-202">Zaznacz pole wyboru ponownie, a następnie kliknij dwukrotnie wartość właściwości i zmień ją na czerwony.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="fd4a7-203">Zmiana zostanie wyświetlona natychmiast:</span><span class="sxs-lookup"><span data-stu-id="fd4a7-203">The change shows immediately:</span></span>

![Czerwony kolor tła](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="fd4a7-205">Okno **Style** ułatwia testowanie i Podgląd zmian CSS przed zatwierdzeniem zmian w arkuszu stylów.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="fd4a7-206">Autosynchronizacja CSS</span><span class="sxs-lookup"><span data-stu-id="fd4a7-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="fd4a7-207">Ta funkcja wymaga wersji 1,3 inspektora strony.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-207">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="fd4a7-208">Funkcja autosynchronizacji CSS umożliwia bezpośrednie edytowanie pliku CSS i natychmiastowe wyświetlanie zmian w przeglądarce stron.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="fd4a7-209">Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="fd4a7-210">W przeglądarce inspektora stron przesuń wskaźnik myszy nad sekcję "Strona główna" do momentu, gdy zostanie wyświetlona etykieta **DIV. Content-otoka** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="fd4a7-211">Kliknij raz, aby zaznaczyć ten element.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-211">Click once to select this element.</span></span>

<span data-ttu-id="fd4a7-212">W oknie **Style** są wyświetlane wszystkie reguły CSS dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-212">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="fd4a7-213">Przewiń w dół, aby znaleźć selektor klasy. Polecane. Content-otoka.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="fd4a7-214">Kliknij pozycję ". Polecane. Content-otoka".</span><span class="sxs-lookup"><span data-stu-id="fd4a7-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="fd4a7-215">Inspektor strony otwiera plik CSS, który definiuje ten styl (site. CSS) i podświetla odpowiedni styl CSS.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="fd4a7-216">Teraz zmień wartość dla `background-color` na "Red".</span><span class="sxs-lookup"><span data-stu-id="fd4a7-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="fd4a7-217">Zmiana zostanie wyświetlona natychmiast w przeglądarce inspektora stron.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="fd4a7-218">Korzystanie z selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="fd4a7-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="fd4a7-219">Edytor CSS w programie Visual Studio 2012 ma selektor kolorów, dzięki czemu można łatwo wybierać i wstawiać kolory.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="fd4a7-220">Selektor kolorów zawiera standardową paletę kolorów, obsługuje standardowe nazwy kolorów, kody skrótów, kolory RGB, RGBA, HSL i HSLA, a także zachowuje listę kolorów używanych ostatnio w dokumencie.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="fd4a7-221">W poprzedniej sekcji zmieniono wartość właściwości `background-color`.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="fd4a7-222">Aby wywołać selektor kolorów, umieść punkt wstawiania po nazwie właściwości i wpisz **#** lub **RGB (** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Pasek selektora kolorów CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="fd4a7-224">Kliknij kolor, aby go zaznaczyć, lub naciśnij klawisz Strzałka w dół, a następnie użyj klawiszy strzałek w lewo i w prawo, aby przechodzenie między kolorami.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="fd4a7-225">Podczas odwiedzania koloru wyświetlana jest odpowiednia wartość szesnastkowa:</span><span class="sxs-lookup"><span data-stu-id="fd4a7-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![Podgląd — wartość właściwości koloru w tle](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="fd4a7-227">Jeśli pasek koloru nie ma dokładnego koloru, możesz użyć selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="fd4a7-228">Aby go otworzyć, kliknij podwójny cudzysłów ostrokątny na prawym końcu paska kolorów lub naciśnij klawisz Strzałka w dół jeden raz lub dwukrotnie na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selektor kolorów CSS — wyskakujące okienko](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="fd4a7-230">Kliknij kolor z pionowego paska po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="fd4a7-231">Spowoduje to wyświetlenie gradientu dla tego koloru w oknie głównym.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="fd4a7-232">Wybierz kolor bezpośrednio z paska pionowego, naciskając klawisz ENTER, lub kliknij dowolny punkt w oknie głównym, aby wybrać większą precyzję.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="fd4a7-233">Jeśli na ekranie komputera istnieje kolor, który ma być używany (nie musi znajdować się w interfejsie użytkownika programu Visual Studio), możesz przechwycić jego wartość za pomocą narzędzia Kroplomierz w prawym dolnym rogu.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="fd4a7-234">Możesz również zmienić nieprzezroczystość koloru, przesuwając suwak w dolnej części selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="fd4a7-235">Spowoduje to zmianę wartości koloru na wartości RGBA, ponieważ format RGBA może reprezentować nieprzezroczystość.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="fd4a7-236">Po wybraniu koloru naciśnij klawisz ENTER, a następnie wpisz średnik, aby zakończyć wprowadzanie koloru tła w pliku *site. css* .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="fd4a7-237">Pasek aktualizacji inspektora stron</span><span class="sxs-lookup"><span data-stu-id="fd4a7-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="fd4a7-238">Inspektor strony natychmiast wykrywa zmiany w pliku *site. css* i wyświetla alert na pasku aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Pasek aktualizacji](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="fd4a7-240">Aby zapisać wszystkie pliki i odświeżyć przeglądarkę inspektora stron, naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij pasek aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="fd4a7-241">Zmiana w kolorze wyróżnienia zostanie wyświetlona w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="fd4a7-242">Mapowanie dynamicznych elementów strony na JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd4a7-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="fd4a7-243">W nowoczesnych aplikacjach sieci Web elementy na stronie są często generowane dynamicznie przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="fd4a7-244">Oznacza to, że nie istnieje statyczny znacznik (HTML lub Razor) odpowiadający tym elementom strony.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="fd4a7-245">W wersji 1,3, inspektor strony może teraz mapować elementy, które zostały dynamicznie dodane do strony z powrotem do odpowiedniego kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="fd4a7-246">Aby zademonstrować tę funkcję, użyjemy [szablonu aplikacji jednostronicowej (Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="fd4a7-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fd4a7-247">Szablon SPA wymaga aktualizacji [ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>

<span data-ttu-id="fd4a7-248">W programie Visual Studio wybierz kolejno pozycje **plik** &gt; **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="fd4a7-249">Po lewej stronie rozwiń pozycję **C#Wizualizacja**, wybierz pozycję **Sieć Web**, a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="fd4a7-250">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-250">Click **OK**.</span></span>

<span data-ttu-id="fd4a7-251">W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **aplikacja jednostronicowa**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="fd4a7-252">W Eksplorator rozwiązań rozwiń folder **widoki** , a następnie folder **macierzysty** .</span><span class="sxs-lookup"><span data-stu-id="fd4a7-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="fd4a7-253">Kliknij prawym przyciskiem myszy plik index. cshtml i wybierz polecenie **Widok w Inspektorze strony**.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="fd4a7-254">Pierwszy element, który jest wyświetlany w przeglądarce stron, jest stroną logowania.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="fd4a7-255">Kliknij pozycję "Utwórz konto" i Utwórz nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="fd4a7-256">Po zarejestrowaniu aplikacja zostanie zarejestrowana w usłudze i zostanie utworzona lista czynności do wykonania z niektórymi Przykładowymi elementami.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="fd4a7-257">Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="fd4a7-258">W przeglądarce inspektora stron kliknij jeden z elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="fd4a7-259">Należy zauważyć, że zamiast wyróżnienia w kolorze niebieskim element jest wyróżniony w pomarańczowym, z "JS" obok nazwy elementu.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="fd4a7-260">Oznacza to, że element został utworzony dynamicznie za pomocą skryptu.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="fd4a7-261">Ponadto do karty **stosu wywołań** pojawia się pomarańczowe podkreolenie. Oznacza to, że okienko **stosu wywołań** zawiera więcej informacji na temat elementu.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="fd4a7-262">Kliknij kartę **stos wywołań** . W okienku **stos wywołań** jest wyświetlany stos wywołań dla wywołania JavaScript, które utworzyło element.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="fd4a7-263">Wywołania bibliotek zewnętrznych, takich jak jQuery, są zwijane, dzięki czemu można łatwo zobaczyć wywołania skryptu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="fd4a7-264">Aby wyświetlić pełny stos, w tym wywołania bibliotek zewnętrznych, można rozwinąć węzły z etykietą "biblioteki zewnętrzne":</span><span class="sxs-lookup"><span data-stu-id="fd4a7-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="fd4a7-265">Po kliknięciu elementu w stosie wywołań program Visual Studio otwiera plik kodu i podświetla odpowiedni skrypt.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="fd4a7-266">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="fd4a7-266">See Also</span></span>

<span data-ttu-id="fd4a7-267">[Wprowadzenie do ASP.NET MVC 4 za pomocą programu Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.NET Web)</span><span class="sxs-lookup"><span data-stu-id="fd4a7-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="fd4a7-268">[Wprowadzenie do Inspektora stron](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (wideo Channel 9)</span><span class="sxs-lookup"><span data-stu-id="fd4a7-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="fd4a7-269">[Komunikaty o błędach inspektora stron](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="fd4a7-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
