---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu z kontrolera | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 7c4aa34567ac4fb31d1ed874cf65986c4e779e66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540391"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="7773d-104">Uzyskiwanie dostępu do danych modelu za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="7773d-104">Accessing Your Model's Data from a Controller</span></span>

<span data-ttu-id="7773d-105">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7773d-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="7773d-106">Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7773d-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="7773d-107">Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.</span><span class="sxs-lookup"><span data-stu-id="7773d-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="7773d-108">W tej sekcji utworzysz nową klasę `MoviesController` i piszesz kod, który pobiera dane filmu i wyświetla go w przeglądarce przy użyciu szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="7773d-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="7773d-109">**Skompiluj aplikację** przed przejściem do następnego kroku.</span><span class="sxs-lookup"><span data-stu-id="7773d-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="7773d-110">Kliknij prawym przyciskiem myszy folder *controllers* i Utwórz nowy kontroler `MoviesController`.</span><span class="sxs-lookup"><span data-stu-id="7773d-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="7773d-111">Poniższe opcje nie będą wyświetlane do momentu skompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7773d-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="7773d-112">Wybierz następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="7773d-112">Select the following options:</span></span>

- <span data-ttu-id="7773d-113">Nazwa kontrolera: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="7773d-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="7773d-114">(Jest to ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="7773d-114">(This is the default.</span></span> <span data-ttu-id="7773d-115">).</span><span class="sxs-lookup"><span data-stu-id="7773d-115">)</span></span>
- <span data-ttu-id="7773d-116">Szablon: **kontroler MVC z akcjami odczytu/zapisu i widokami korzystającymi z Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="7773d-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="7773d-117">Model klasy: **Movie (MvcMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="7773d-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="7773d-118">Klasa kontekstu danych: **MovieDBContext (MvcMovie. models)** .</span><span class="sxs-lookup"><span data-stu-id="7773d-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="7773d-119">Widoki: **Razor (cshtml)** .</span><span class="sxs-lookup"><span data-stu-id="7773d-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="7773d-120">(Wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="7773d-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="7773d-122">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="7773d-122">Click **Add**.</span></span> <span data-ttu-id="7773d-123">Visual Studio Express tworzy następujące pliki i foldery:</span><span class="sxs-lookup"><span data-stu-id="7773d-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="7773d-124">Plik *MoviesController.cs* w folderze *controllers* programu Project.</span><span class="sxs-lookup"><span data-stu-id="7773d-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="7773d-125">Folder *filmów* w folderze *widoki* projektu.</span><span class="sxs-lookup"><span data-stu-id="7773d-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="7773d-126">*Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*i *index. cshtml* w nowym folderze *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="7773d-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="7773d-127">ASP.NET MVC 4 automatycznie utworzył metody i widoki akcji CRUD (Create, Read, Update i Delete) dla Ciebie (automatyczne tworzenie metod i widoków akcji CRUD jest znane jako rusztowania).</span><span class="sxs-lookup"><span data-stu-id="7773d-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="7773d-128">Masz teraz w pełni funkcjonalną aplikację sieci Web, która umożliwia tworzenie, wyświetlanie list, edytowanie i usuwanie wpisów filmów.</span><span class="sxs-lookup"><span data-stu-id="7773d-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="7773d-129">Uruchom aplikację i przejdź do kontrolera `Movies`, dołączając */Movies* do adresu URL na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7773d-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="7773d-130">Ponieważ aplikacja jest zależna od domyślnego routingu (zdefiniowanego w pliku *Global. asax* ), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowane do domyślnej metody akcji `Index` kontrolera `Movies`.</span><span class="sxs-lookup"><span data-stu-id="7773d-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="7773d-131">Innymi słowy, `http://localhost:xxxxx/Movies` żądania przeglądarki są skutecznie takie same, jak `http://localhost:xxxxx/Movies/Index`żądania przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7773d-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="7773d-132">Wynik jest pustą listą filmów, ponieważ nie został jeszcze dodany.</span><span class="sxs-lookup"><span data-stu-id="7773d-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="7773d-133">Tworzenie filmu</span><span class="sxs-lookup"><span data-stu-id="7773d-133">Creating a Movie</span></span>

<span data-ttu-id="7773d-134">Wybierz łącze **Utwórz nowy** .</span><span class="sxs-lookup"><span data-stu-id="7773d-134">Select the **Create New** link.</span></span> <span data-ttu-id="7773d-135">Wprowadź szczegóły dotyczące filmu, a następnie kliknij przycisk **Utwórz** .</span><span class="sxs-lookup"><span data-stu-id="7773d-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="7773d-136">Kliknięcie przycisku **Utwórz** powoduje opublikowanie formularza na serwerze, gdzie informacje o filmie są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7773d-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="7773d-137">Następnie nastąpi przekierowanie do adresu URL */Movies* , w którym można zobaczyć nowo utworzony film na liście.</span><span class="sxs-lookup"><span data-stu-id="7773d-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="7773d-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="7773d-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="7773d-139">Utwórz kilka dodatkowych wpisów filmu.</span><span class="sxs-lookup"><span data-stu-id="7773d-139">Create a couple more movie entries.</span></span> <span data-ttu-id="7773d-140">Wypróbuj linki **Edytuj**, **szczegóły**i **Usuń** , które są wszystkie funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="7773d-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="7773d-141">Badanie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="7773d-141">Examining the Generated Code</span></span>

<span data-ttu-id="7773d-142">Otwórz plik *Controllers\MoviesController.cs* i przejrzyj wygenerowaną metodę `Index`.</span><span class="sxs-lookup"><span data-stu-id="7773d-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="7773d-143">Poniżej przedstawiono część kontrolera filmu z metodą `Index`.</span><span class="sxs-lookup"><span data-stu-id="7773d-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="7773d-144">Poniższy wiersz z klasy `MoviesController` tworzy wystąpienie kontekstu bazy danych filmu, jak opisano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="7773d-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="7773d-145">Możesz użyć kontekstu bazy danych filmu, aby wysyłać zapytania, edytować i usuwać filmy.</span><span class="sxs-lookup"><span data-stu-id="7773d-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="7773d-146">Żądanie do kontrolera `Movies` zwraca wszystkie wpisy w tabeli `Movies` bazy danych filmów, a następnie przekazuje wyniki do widoku `Index`.</span><span class="sxs-lookup"><span data-stu-id="7773d-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="7773d-147">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="7773d-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="7773d-148">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do szablonu widoku przy użyciu obiektu `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="7773d-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="7773d-149">`ViewBag` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="7773d-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="7773d-150">ASP.NET MVC oferuje również możliwość przekazywania danych o jednoznacznie określonym typie lub obiektów do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="7773d-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="7773d-151">Takie silnie wpisane podejście umożliwia lepsze Sprawdzanie kodu w czasie kompilacji i bogatsze IntelliSense w edytorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7773d-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="7773d-152">Mechanizm tworzenia szkieletu w programie Visual Studio używa tego podejścia z klasą `MoviesController` i wyświetlania szablonów podczas tworzenia metod i widoków.</span><span class="sxs-lookup"><span data-stu-id="7773d-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="7773d-153">W pliku *Controllers\MoviesController.cs* Przejrzyj wygenerowaną metodę `Details`.</span><span class="sxs-lookup"><span data-stu-id="7773d-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="7773d-154">Poniżej przedstawiono część kontrolera filmu z metodą `Details`.</span><span class="sxs-lookup"><span data-stu-id="7773d-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="7773d-155">Jeśli `Movie` zostanie znaleziona, wystąpienie modelu `Movie` zostanie przesłane do widoku szczegółów.</span><span class="sxs-lookup"><span data-stu-id="7773d-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="7773d-156">Zapoznaj się z zawartością pliku *Views\Movies\Details.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="7773d-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="7773d-157">Dołączając instrukcję `@model` w górnej części pliku szablonu widoku, można określić typ obiektu, którego oczekuje widok.</span><span class="sxs-lookup"><span data-stu-id="7773d-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="7773d-158">Po utworzeniu kontrolera filmu program Visual Studio automatycznie dołączał następującą instrukcję `@model` w górnej części pliku *details. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7773d-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="7773d-159">Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="7773d-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7773d-160">Na przykład w szablonie *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i pomocników HTML [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) z silnie wpisanąm obiektem `Model`.</span><span class="sxs-lookup"><span data-stu-id="7773d-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="7773d-161">Metody Create i Edit i View templates również przekazują obiekt modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="7773d-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="7773d-162">Sprawdź szablon widoku *index. cshtml* i metodę `Index` w pliku *MoviesController.cs* .</span><span class="sxs-lookup"><span data-stu-id="7773d-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="7773d-163">Zwróć uwagę, jak kod tworzy obiekt [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) , gdy wywołuje metodę pomocnika `View` w metodzie `Index` akcji.</span><span class="sxs-lookup"><span data-stu-id="7773d-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="7773d-164">Następnie kod przekazuje tę `Movies` listę z kontrolera do widoku:</span><span class="sxs-lookup"><span data-stu-id="7773d-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="7773d-165">Po utworzeniu kontrolera filmu Visual Studio Express automatycznie dołączać następujące instrukcje `@model` w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7773d-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="7773d-166">Ta `@model` dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="7773d-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7773d-167">Na przykład w szablonie *index. cshtml* kod przechodzi przez film przez wykonanie instrukcji `foreach` na obiekcie `Model` o jednoznacznie określonym typie:</span><span class="sxs-lookup"><span data-stu-id="7773d-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="7773d-168">Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy obiekt `item` w pętli jest wpisywany jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7773d-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="7773d-169">W związku z innymi korzyściami oznacza to, że w edytorze kodu są dostępne sprawdzanie w czasie kompilacji kodu i pełna obsługa technologii IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="7773d-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="7773d-171">Praca z SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="7773d-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="7773d-172">Entity Framework Code First wykryła, że podane parametry połączenia z bazą danych wskazywały na `Movies` bazę danych, która jeszcze nie istnieje, więc Code First automatycznie utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7773d-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="7773d-173">Możesz sprawdzić, czy został on utworzony, przeglądając folder *danych\_aplikacji* .</span><span class="sxs-lookup"><span data-stu-id="7773d-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="7773d-174">Jeśli plik *wideo. mdf* nie jest widoczny, kliknij przycisk **Pokaż wszystkie pliki** na pasku narzędzi **Eksplorator rozwiązań** , kliknij przycisk **odśwież** , a następnie rozwiń folder *dane\_aplikacji* .</span><span class="sxs-lookup"><span data-stu-id="7773d-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="7773d-175">Kliknij dwukrotnie ikonę *filmy. mdf* , aby otworzyć **Eksploratora bazy danych**, a następnie rozwiń folder **tabele** , aby wyświetlić tabelę filmy.</span><span class="sxs-lookup"><span data-stu-id="7773d-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="7773d-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="7773d-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="7773d-177">Jeśli Eksplorator bazy danych nie jest wyświetlany, w menu **Narzędzia** wybierz pozycję **Połącz z bazą danych**, a następnie Anuluj okno dialogowe **Wybieranie źródła danych** .</span><span class="sxs-lookup"><span data-stu-id="7773d-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="7773d-178">Spowoduje to wymuszenie otwarcia Eksploratora bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7773d-178">This will force open the database explorer.</span></span>

> [!NOTE]
> <span data-ttu-id="7773d-179">Jeśli używasz programu pliku VWD lub Visual Studio 2010 i wystąpi błąd podobny do jednego z następujących:</span><span class="sxs-lookup"><span data-stu-id="7773d-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="7773d-180">Baza danych "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. Nie można otworzyć środowiska MDF, ponieważ jest w wersji 706.</span><span class="sxs-lookup"><span data-stu-id="7773d-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="7773d-181">Ten serwer obsługuje wersję 655 i wcześniejszą.</span><span class="sxs-lookup"><span data-stu-id="7773d-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="7773d-182">Ścieżka obniżania poziomu nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="7773d-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="7773d-183">&quot;wyjątek InvalidOperation został nieobsłużony przez kod użytkownika&quot; dostarczonego elementu SqlConnection nie określa katalogu początkowego.</span><span class="sxs-lookup"><span data-stu-id="7773d-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="7773d-184">Musisz zainstalować [SQL Server narzędzia danych](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) i [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="7773d-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="7773d-185">Sprawdź parametry połączenia `MovieDBContext` określone na poprzedniej stronie.</span><span class="sxs-lookup"><span data-stu-id="7773d-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>

<span data-ttu-id="7773d-186">Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz polecenie **Pokaż dane tabeli** , aby wyświetlić utworzone dane.</span><span class="sxs-lookup"><span data-stu-id="7773d-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="7773d-187">Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz pozycję **Otwórz definicję tabeli** , aby wyświetlić strukturę tabeli utworzoną przez Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="7773d-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="7773d-188">Zwróć uwagę, jak schemat tabeli `Movies` mapuje do utworzonej wcześniej klasy `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7773d-188">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="7773d-189">Entity Framework Code First automatycznie utworzył ten schemat na podstawie klasy `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7773d-189">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="7773d-190">Po zakończeniu zamknij połączenie, klikając prawym przyciskiem myszy pozycję *MovieDBContext* i wybierając pozycję **Zamknij połączenie**.</span><span class="sxs-lookup"><span data-stu-id="7773d-190">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="7773d-191">(Jeśli połączenie nie zostanie zamknięte, podczas następnego uruchomienia projektu może wystąpić błąd).</span><span class="sxs-lookup"><span data-stu-id="7773d-191">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

<span data-ttu-id="7773d-192">Teraz masz bazę danych i prostą stronę z listą, aby wyświetlić z niej zawartość.</span><span class="sxs-lookup"><span data-stu-id="7773d-192">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="7773d-193">W następnym samouczku sprawdzimy resztę kodu szkieletowego i dodamy metodę `SearchIndex` i widok `SearchIndex`, który umożliwi wyszukiwanie filmów w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7773d-193">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7773d-194">[Poprzednie](adding-a-model.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="7773d-194">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
