---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programowanie ASP.NET stron sieci Web (Razor) przy użyciu programu Visual Studio | Microsoft Docs
author: Rick-Anderson
description: W tym dodatku wyjaśniono, jak można użyć programu Visual Studio 2010 lub Visual Web Developer 2010 Express do programu ASP.NET Web Pages with the składnia Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633512"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="01c72-103">Programowanie ASP.NET stron sieci Web (Razor) przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01c72-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>

<span data-ttu-id="01c72-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="01c72-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="01c72-105">W tym artykule wyjaśniono, jak można użyć programu Visual Studio lub Visual Web Developer Express do programu program ASP.NET Web Pages (Razor) websites.</span><span class="sxs-lookup"><span data-stu-id="01c72-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="01c72-106">Zawartość</span><span class="sxs-lookup"><span data-stu-id="01c72-106">What you'll learn</span></span>
>
> - <span data-ttu-id="01c72-107">Co jest potrzebne do pracy z ASP.NET stronami sieci Web w używanej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c72-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="01c72-108">Jak dodać obsługę stron sieci Web ASP.NET do programu Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="01c72-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="01c72-109">Jak używać funkcji programu Visual Studio do pracy ze stronami ASP.NET Razor, w tym IntelliSense i debugerem.</span><span class="sxs-lookup"><span data-stu-id="01c72-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="01c72-110">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="01c72-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="01c72-111">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="01c72-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="01c72-112">Program Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="01c72-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="01c72-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="01c72-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="01c72-114">Ten samouczek działa również z ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 i WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="01c72-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>

<span data-ttu-id="01c72-115">Można programować ASP.NET składnia Razor stron sieci Web za pomocą programu WebMatrix lub wielu innych edytorów kodu.</span><span class="sxs-lookup"><span data-stu-id="01c72-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="01c72-116">Możesz również użyć Microsoft Visual Studio, który jest w pełni funkcjonalnym zintegrowanym środowiskiem programistycznym (IDE), który oferuje zaawansowany zestaw narzędzi do tworzenia wielu typów aplikacji (a nie tylko witryn sieci Web).</span><span class="sxs-lookup"><span data-stu-id="01c72-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="01c72-117">Aby współpracować ze stronami ASP.NET Razor, możesz użyć [programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="01c72-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="01c72-118">Dwie szczególnie przydatne funkcje, które program Visual Studio oferuje do programowania przy użyciu stron sieci Web ASP.NET Razor:</span><span class="sxs-lookup"><span data-stu-id="01c72-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="01c72-119">Funkcja *IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="01c72-119">*IntelliSense*.</span></span> <span data-ttu-id="01c72-120">Funkcja IntelliSense wbudowana w program Visual Studio jest bardziej kompleksowa niż technologia IntelliSense w programie WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="01c72-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="01c72-121">*Debuger*.</span><span class="sxs-lookup"><span data-stu-id="01c72-121">*Debugger*.</span></span> <span data-ttu-id="01c72-122">Debuger umożliwia rozwiązywanie problemów z kodem przez zatrzymywanie programu, gdy jest on uruchomiony, badanie zmiennych i przechodzenie do kolejnych wierszy kodu.</span><span class="sxs-lookup"><span data-stu-id="01c72-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="01c72-123">Korzystanie z programu Visual Studio z różnymi wersjami stron sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="01c72-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="01c72-124">Aby opracowywać aplikacje sieci Web ASP.NET w programie Visual Studio 2017, zainstaluj obciążenie narzędzia **ASP.NET i programowanie dla sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="01c72-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="01c72-125">Program Visual Studio 2012 i Visual Studio 2013 obejmują obsługę stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01c72-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="01c72-126">(Pakiety wymagane w celu obsługi stron sieci Web ASP.NET są instalowane podczas instalowania programu Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="01c72-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="01c72-127">Program Visual Studio 2010 nie obejmuje domyślnie obsługi stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01c72-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="01c72-128">Aby korzystać ze stron sieci Web ASP.NET w programie Visual Studio 2010, należy zainstalować pakiet ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="01c72-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="01c72-129">Aby uzyskać ASP.NET Web Pages 2, zainstaluj ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="01c72-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="01c72-130">Poniższa tabela zawiera podsumowanie obsługi stron sieci Web ASP.NET w różnych wersjach programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c72-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="01c72-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="01c72-131">Visual Studio 2010</span></span> | <span data-ttu-id="01c72-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="01c72-132">Visual Studio 2012</span></span> | <span data-ttu-id="01c72-133">Program Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="01c72-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="01c72-134">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="01c72-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="01c72-135">Zainstaluj ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="01c72-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="01c72-136">Uwzględnione</span><span class="sxs-lookup"><span data-stu-id="01c72-136">(Included)</span></span> | <span data-ttu-id="01c72-137">Uwzględnione</span><span class="sxs-lookup"><span data-stu-id="01c72-137">(Included)</span></span> |
| <span data-ttu-id="01c72-138">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="01c72-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="01c72-139">Aktualizacja programu ASP.NET Web Pages 3 za poorednictwem narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="01c72-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="01c72-140">Uwzględnione</span><span class="sxs-lookup"><span data-stu-id="01c72-140">(Included)</span></span> |

<span data-ttu-id="01c72-141">Aby współpracować z programem Visual Studio 2010, zobacz [Instalowanie obsługi stron sieci Web ASP.NET w programie Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="01c72-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="01c72-142">Uruchamianie programu Visual Studio z programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="01c72-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="01c72-143">Jeśli rozpoczęto projekt w programie WebMatrix i chcesz przełączyć się do programu Visual Studio, Program WebMatrix udostępnia przycisk umożliwiający łatwe otwieranie projektu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c72-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="01c72-144">Aby ten przycisk został włączony, na komputerze musi być zainstalowany program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c72-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="01c72-145">Na poniższej ilustracji przedstawiono przycisk w programie WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="01c72-145">The following image shows the button in WebMatrix.</span></span>

![Uruchom program Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="01c72-147">Po kliknięciu przycisku projekt zostanie otwarty w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c72-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="01c72-148">Możesz przełączać się między programem WebMatrix i programem Visual Studio bez żadnych problemów.</span><span class="sxs-lookup"><span data-stu-id="01c72-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="01c72-149">Użytkownik zostanie powiadomiony, czy jakiekolwiek pliki uległy zmianie w innym środowisku i muszą zostać ponownie załadowane w celu uzyskania najnowszych zmian.</span><span class="sxs-lookup"><span data-stu-id="01c72-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="01c72-150">Tworzenie witryny ASP.NET Razor w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01c72-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="01c72-151">Aby utworzyć witrynę sieci Web ASP.NET Razor w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="01c72-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="01c72-152">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c72-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="01c72-153">W menu **plik** kliknij pozycję **Nowa witryna sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="01c72-153">In the **File** menu, click **New Web Site**.</span></span>

    ![Utwórz nową witrynę sieci Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="01c72-155">W **nowej witrynie sieci Web** okno dialogowe, wybierz język, który ma być używany C# (Visual lub Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="01c72-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="01c72-156">Wybierz szablon **witryny sieci Web ASP.NET (Razor)** .</span><span class="sxs-lookup"><span data-stu-id="01c72-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![Witryna Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="01c72-158">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="01c72-158">Click **OK**.</span></span>

<span data-ttu-id="01c72-159">Nowy projekt istnieje i zawiera kilka domyślnych stron sieci Web, które ułatwiają rozpoczęcie pracy.</span><span class="sxs-lookup"><span data-stu-id="01c72-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="01c72-160">Korzystanie z IntelliSense</span><span class="sxs-lookup"><span data-stu-id="01c72-160">Using IntelliSense</span></span>

<span data-ttu-id="01c72-161">Po utworzeniu lokacji można zobaczyć, jak działa technologia IntelliSense w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c72-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="01c72-162">W właśnie utworzonej witrynie sieci Web otwórz stronę *default. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="01c72-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="01c72-163">Po `<h3>` tagów na stronie wpisz `@ServerInfo.` (łącznie z kropką).</span><span class="sxs-lookup"><span data-stu-id="01c72-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="01c72-164">Zauważ, że technologia IntelliSense wyświetla dostępne metody pomocnika `ServerInfo` na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="01c72-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![Technologia](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="01c72-166">Wybierz z listy metodę `GetHtml`, a następnie naciśnij klawisz ENTER.</span><span class="sxs-lookup"><span data-stu-id="01c72-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="01c72-167">Funkcja IntelliSense automatycznie wypełnia metodę.</span><span class="sxs-lookup"><span data-stu-id="01c72-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="01c72-168">(Podobnie jak w przypadku dowolnej C#metody w, należy dodać `()` znaków po metodzie). Ukończony kod metody `GetHtml` wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="01c72-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="01c72-169">Naciśnij klawisze CTRL + F5, aby uruchomić stronę.</span><span class="sxs-lookup"><span data-stu-id="01c72-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="01c72-170">Oto jak wygląda strona, gdy jest wyświetlana w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="01c72-170">This is what the page looks like when displayed in a browser:</span></span>

    ![Strona domyślna w przeglądarce](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="01c72-172">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="01c72-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="01c72-173">Korzystanie z debugera</span><span class="sxs-lookup"><span data-stu-id="01c72-173">Using the Debugger</span></span>

1. <span data-ttu-id="01c72-174">W górnej części strony *default. cshtml* po wierszu rozpoczynającym się od `Page.Title`Dodaj następujący wiersz kodu:</span><span class="sxs-lookup"><span data-stu-id="01c72-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="01c72-175">Na szarym marginesie edytora z lewej strony kodu kliknij przycisk Dalej w tym nowym wierszu, aby dodać *punkt przerwania*.</span><span class="sxs-lookup"><span data-stu-id="01c72-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="01c72-176">Punkt przerwania to znacznik informujący debugera o zatrzymaniu działania programu w tym momencie, aby zobaczyć, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="01c72-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Ustaw punkt przerwania](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="01c72-178">Usuń wywołanie metody `ServerInfo.GetHtml` i Dodaj wywołanie do zmiennej `@myTime` w jej miejscu.</span><span class="sxs-lookup"><span data-stu-id="01c72-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="01c72-179">To wywołanie wyświetla bieżącą wartość czasu zwracaną przez nowy wiersz kodu.</span><span class="sxs-lookup"><span data-stu-id="01c72-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="01c72-180">Naciśnij klawisz F5, aby uruchomić stronę w debugerze.</span><span class="sxs-lookup"><span data-stu-id="01c72-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="01c72-181">Strona zostanie zatrzymana w ustawionym punkcie przerwania.</span><span class="sxs-lookup"><span data-stu-id="01c72-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="01c72-182">Na poniższej ilustracji pokazano, jak wygląda strona w edytorze z punktem przerwania (żółtym).</span><span class="sxs-lookup"><span data-stu-id="01c72-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![Debuguj punkt przerwania](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="01c72-184">Na pasku narzędzi debugowania kliknij przycisk **Wkrocz** (lub naciśnij klawisz F11), aby uruchomić następny wiersz kodu.</span><span class="sxs-lookup"><span data-stu-id="01c72-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="01c72-185">Za każdym razem, gdy klikniesz ten przycisk, przechodzenie do następnego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="01c72-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Przycisk Wkrocz do](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="01c72-187">Sprawdź wartość zmiennej `myTime`, trzymając wskaźnik myszy nad nią lub sprawdzając wartości wyświetlane w oknach **zmiennych lokalnych** i **stosu wywołań** .</span><span class="sxs-lookup"><span data-stu-id="01c72-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="01c72-188">Program Visual Studio Wyświetla wartość zmiennej.</span><span class="sxs-lookup"><span data-stu-id="01c72-188">Visual Studio display the value of the variable.</span></span>

    ![Pokaż wartość czasu](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="01c72-190">Gdy skończysz badanie zmiennej i przechodzenie przez kod, naciśnij klawisz F5, aby kontynuować uruchamianie strony bez zatrzymywania w każdym wierszu.</span><span class="sxs-lookup"><span data-stu-id="01c72-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="01c72-191">Gdy skończysz przechodzenie przez cały kod, przeglądarka wyświetli stronę.</span><span class="sxs-lookup"><span data-stu-id="01c72-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="01c72-192">Aby dowiedzieć się więcej o debugerze i sposobach debugowania kodu w programie Visual Studio, zobacz [Przewodnik: debugowanie stron sieci Web w programie Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="01c72-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="01c72-193">Korzystanie z Razor w projektach ASP.NET MVC w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01c72-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="01c72-194">Składnia Razor jest również szeroko używany w projektach ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="01c72-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="01c72-195">MVC to zaawansowany, oparty na wzorcach sposób tworzenia dynamicznych witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01c72-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="01c72-196">Jeśli witryna ASP.NET Web Pages będzie trudna do utrzymania, warto rozważyć jej konwersję do aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="01c72-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="01c72-197">Przykład tworzenia aplikacji MVC można znaleźć w [wprowadzenie z ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="01c72-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="01c72-198">Instalowanie obsługi stron sieci Web ASP.NET w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="01c72-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="01c72-199">W tej sekcji przedstawiono sposób instalowania programu Visual Web Developer Express 2010 i narzędzi ASP.NET Web Pages for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01c72-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="01c72-200">Jeśli nie masz jeszcze Instalatora platformy sieci Web, Pobierz go z następującego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="01c72-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="01c72-201">Uruchom Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="01c72-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="01c72-202">Kliknij kartę **produkty** .</span><span class="sxs-lookup"><span data-stu-id="01c72-202">Click the **Products** tab.</span></span>

    ![Karta produkty Instalatora WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="01c72-204">Wyszukaj **ASP.NET MVC 4** (dla ASP.NET Web Pages 2), a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="01c72-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="01c72-205">Te produkty zawierają narzędzia Visual Studio Tools do tworzenia witryn sieci Web ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="01c72-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opcje instalacji Instalatora WebPI](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="01c72-207">Kliknij przycisk **Instaluj** , aby zakończyć instalację.</span><span class="sxs-lookup"><span data-stu-id="01c72-207">Click **Install** to complete the installation.</span></span>
