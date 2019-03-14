---
title: 'Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać stron Razor w programie ASP.NET Core. Dowiedz się, jak utworzyć model, generowanie kodu dla stron Razor, platformy Entity Framework Core i SQL Server na użytek dostęp do danych, dodać funkcje wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracje do aktualizacji modelu.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 81a2a76fc1cecc78b69226fe714d7c9272b04bf7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072455"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="e909c-104">Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e909c-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e909c-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e909c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e909c-106">To jest pierwszy samouczek serii.</span><span class="sxs-lookup"><span data-stu-id="e909c-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="e909c-107">[Seria](xref:tutorials/razor-pages/index) uczy podstaw tworzenia aplikacji platformy ASP.NET Core Razor strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="e909c-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="e909c-108">Na końcu serii będziesz mieć aplikację, która zarządza bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="e909c-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="e909c-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="e909c-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e909c-110">Tworzenie aplikacji internetowej stron Razor.</span><span class="sxs-lookup"><span data-stu-id="e909c-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="e909c-111">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="e909c-111">Run the app.</span></span>
> * <span data-ttu-id="e909c-112">Sprawdź pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="e909c-112">Examine the project files.</span></span>

<span data-ttu-id="e909c-113">Na końcu tego samouczka będziesz mieć działającą aplikację sieci web stron Razor, które utworzysz w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="e909c-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="e909c-115">Tworzenie aplikacji sieci web stron Razor</span><span class="sxs-lookup"><span data-stu-id="e909c-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e909c-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e909c-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e909c-117">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e909c-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="e909c-118">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e909c-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e909c-119">Nadaj projektowi nazwę **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e909c-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e909c-120">Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="e909c-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="e909c-122">Wybierz **platformy ASP.NET Core 2.2** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e909c-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="e909c-124">Utworzono następujący projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="e909c-124">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e909c-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e909c-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e909c-127">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e909c-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="e909c-128">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="e909c-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="e909c-129">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e909c-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="e909c-130">`dotnet new` Polecenie tworzy nowy projekt strony Razor w *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="e909c-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="e909c-131">`code` Polecenia otwiera *RazorPagesMovie* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e909c-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="e909c-132">Zostanie wyświetlone okno dialogowe z **"RazorPagesMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="e909c-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="e909c-133">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="e909c-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e909c-134">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e909c-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e909c-135">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e909c-135">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

<span data-ttu-id="e909c-136">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do utworzenia projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="e909c-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="e909c-137">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="e909c-137">Open the project</span></span>

<span data-ttu-id="e909c-138">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="e909c-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="e909c-139">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e909c-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e909c-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e909c-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e909c-141">Naciśnij klawisze Ctrl + F5, aby uruchomić bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="e909c-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="e909c-142">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e909c-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e909c-143">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e909c-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e909c-144">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e909c-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="e909c-145">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e909c-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e909c-146">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="e909c-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e909c-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e909c-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e909c-148">Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.</span><span class="sxs-lookup"><span data-stu-id="e909c-148">Press **Ctrl-F5** to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="e909c-149">Uruchamia programu Visual Studio Code [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e909c-149">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="e909c-150">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e909c-150">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e909c-151">To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="e909c-151">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="e909c-152">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e909c-152">Localhost only serves web requests from the local computer.</span></span>
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e909c-153">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e909c-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e909c-154">Wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e909c-154">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="e909c-155">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e909c-155">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

<!-- End of VS tabs -->

---

* <span data-ttu-id="e909c-156">Na stronie głównej aplikacji, wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="e909c-156">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="e909c-157">Ta aplikacja nie może śledzić informacje osobiste, ale szablonu projektu obejmuje funkcję zgody, w przypadku, gdy będą potrzebne do wykonania w Unii Europejskiej [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e909c-157">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="e909c-159">Na poniższej ilustracji przedstawiono aplikację po zgody śledzenia:</span><span class="sxs-lookup"><span data-stu-id="e909c-159">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a><span data-ttu-id="e909c-161">Przejrzyj pliki projektu</span><span class="sxs-lookup"><span data-stu-id="e909c-161">Examine the project files</span></span>

<span data-ttu-id="e909c-162">Poniżej przedstawiono omówienie folderów głównego projektu i plików, które będziesz pracować w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="e909c-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="e909c-163">Folder stron</span><span class="sxs-lookup"><span data-stu-id="e909c-163">Pages folder</span></span>

<span data-ttu-id="e909c-164">Zawiera stronami Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="e909c-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="e909c-165">Każda strona Razor jest parę plików:</span><span class="sxs-lookup"><span data-stu-id="e909c-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="e909c-166">A *.cshtml* pliku, który zawiera kod znaczników HTML za pomocą C# kodu przy użyciu składni Razor.</span><span class="sxs-lookup"><span data-stu-id="e909c-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="e909c-167">A *. cshtml.cs* pliku, który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="e909c-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="e909c-168">Pliki obsługi mają nazwy rozpoczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="e909c-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="e909c-169">Na przykład *_Layout.cshtml* plik konfiguruje elementy interfejsu użytkownika dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="e909c-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="e909c-170">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="e909c-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="e909c-171">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="e909c-171">For more information, see <xref:mvc/views/layout>.</span></span>


### <a name="wwwroot-folder"></a><span data-ttu-id="e909c-172">Wwwroot folder</span><span class="sxs-lookup"><span data-stu-id="e909c-172">wwwroot folder</span></span>

<span data-ttu-id="e909c-173">Zawiera pliki statyczne, takie jak pliki HTML, plików JavaScript i plików CSS.</span><span class="sxs-lookup"><span data-stu-id="e909c-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="e909c-174">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="e909c-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="e909c-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="e909c-175">appSettings.json</span></span>

<span data-ttu-id="e909c-176">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="e909c-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="e909c-177">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e909c-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="e909c-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="e909c-178">Program.cs</span></span>

<span data-ttu-id="e909c-179">Zawiera punkt wejścia programu.</span><span class="sxs-lookup"><span data-stu-id="e909c-179">Contains the entry point for the program.</span></span> <span data-ttu-id="e909c-180">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="e909c-180">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="e909c-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="e909c-181">Startup.cs</span></span>

<span data-ttu-id="e909c-182">Zawiera kod, który konfiguruje zachowania aplikacji, na przykład tego, czy wymaga zgody na pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="e909c-182">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="e909c-183">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e909c-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e909c-184">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="e909c-184">Next steps</span></span>

<span data-ttu-id="e909c-185">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="e909c-185">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e909c-186">Utworzona aplikacja internetowa ze stronami Razor.</span><span class="sxs-lookup"><span data-stu-id="e909c-186">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="e909c-187">Uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e909c-187">Ran the app.</span></span>
> * <span data-ttu-id="e909c-188">Zbadane plików projektu.</span><span class="sxs-lookup"><span data-stu-id="e909c-188">Examined the project files.</span></span>

<span data-ttu-id="e909c-189">Przejdź do następnego samouczka w serii:</span><span class="sxs-lookup"><span data-stu-id="e909c-189">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e909c-190">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="e909c-190">Add a model</span></span>](xref:tutorials/razor-pages/model)
