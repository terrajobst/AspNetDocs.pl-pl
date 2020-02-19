---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu z kontrolera | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5d882d765133d32d3acdba9ffb5d43b69119a273
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457235"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="fe20d-102">Uzyskiwanie dostępu do danych modelu za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="fe20d-102">Accessing Your Model's Data from a Controller</span></span>

<span data-ttu-id="fe20d-103">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe20d-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="fe20d-104">W tej sekcji utworzysz nową klasę `MoviesController` i piszesz kod, który pobiera dane filmu i wyświetla go w przeglądarce przy użyciu szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="fe20d-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="fe20d-105">**Skompiluj aplikację** przed przejściem do następnego kroku.</span><span class="sxs-lookup"><span data-stu-id="fe20d-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="fe20d-106">Jeśli aplikacja nie zostanie utworzona, wystąpi błąd podczas dodawania kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fe20d-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="fe20d-107">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder *controllers* , a następnie kliknij polecenie **Dodaj**, a następnie pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="fe20d-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="fe20d-108">W oknie dialogowym **Dodawanie szkieletu** kliknij pozycję **kontroler MVC 5 z widokami, używając Entity Framework**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fe20d-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="fe20d-109">Wybierz pozycję **film (MvcMovie. models)** dla klasy model.</span><span class="sxs-lookup"><span data-stu-id="fe20d-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="fe20d-110">Wybierz pozycję **MovieDBContext (MvcMovie. models)** dla klasy kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="fe20d-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="fe20d-111">Dla nazwy kontrolera wprowadź **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="fe20d-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="fe20d-112">Na poniższej ilustracji przedstawiono okno dialogowe ukończone.</span><span class="sxs-lookup"><span data-stu-id="fe20d-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="fe20d-113">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="fe20d-113">Click **Add**.</span></span> <span data-ttu-id="fe20d-114">(Jeśli wystąpi błąd, prawdopodobnie nie skompilowano aplikacji przed rozpoczęciem dodawania kontrolera). Program Visual Studio tworzy następujące pliki i foldery:</span><span class="sxs-lookup"><span data-stu-id="fe20d-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="fe20d-115">Plik *MoviesController.cs* w folderze *controllers* .</span><span class="sxs-lookup"><span data-stu-id="fe20d-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="fe20d-116">Folder *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="fe20d-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="fe20d-117">*Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*i *index. cshtml* w nowym folderze *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="fe20d-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="fe20d-118">Program Visual Studio automatycznie utworzył metody i widoki akcji [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenie, odczytywanie, aktualizowanie i usuwanie) dla użytkownika (automatyczne tworzenie metod akcji CRUD i widoków jest znane jako rusztowania).</span><span class="sxs-lookup"><span data-stu-id="fe20d-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="fe20d-119">Masz teraz w pełni funkcjonalną aplikację sieci Web, która umożliwia tworzenie, wyświetlanie list, edytowanie i usuwanie wpisów filmów.</span><span class="sxs-lookup"><span data-stu-id="fe20d-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="fe20d-120">Uruchom aplikację i kliknij link do **filmu MVC** (lub przejdź do kontrolera `Movies`, dołączając */Movies* do adresu URL na pasku adresu przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="fe20d-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="fe20d-121">Ponieważ aplikacja jest zależna od domyślnego routingu (zdefiniowanego w *aplikacji\_pliku Start\RouteConfig.cs* ), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowane do domyślnej metody akcji `Index` kontrolera `Movies`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="fe20d-122">Innymi słowy, `http://localhost:xxxxx/Movies` żądania przeglądarki są skutecznie takie same, jak `http://localhost:xxxxx/Movies/Index`żądania przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="fe20d-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="fe20d-123">Wynik jest pustą listą filmów, ponieważ nie został jeszcze dodany.</span><span class="sxs-lookup"><span data-stu-id="fe20d-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="fe20d-124">Tworzenie filmu</span><span class="sxs-lookup"><span data-stu-id="fe20d-124">Creating a Movie</span></span>

<span data-ttu-id="fe20d-125">Wybierz łącze **Utwórz nowy** .</span><span class="sxs-lookup"><span data-stu-id="fe20d-125">Select the **Create New** link.</span></span> <span data-ttu-id="fe20d-126">Wprowadź szczegóły dotyczące filmu, a następnie kliknij przycisk **Utwórz** .</span><span class="sxs-lookup"><span data-stu-id="fe20d-126">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="fe20d-127">W polu cena może nie być możliwe wprowadzanie przecinków dziesiętnych ani przecinków.</span><span class="sxs-lookup"><span data-stu-id="fe20d-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="fe20d-128">Aby zapewnić obsługę walidacji jQuery dla ustawień regionalnych innych niż angielskie, które używają przecinka (&quot;,&quot;) dla punktu dziesiętnego i formatów daty innych niż angielski, należy dołączyć plik *globalizacjs. js* i określone *kultury/globalizacja pliku kultury. js* (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) i JavaScript, aby użyć `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="fe20d-129">Pokażę, jak to zrobić w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="fe20d-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="fe20d-130">Na razie po prostu wprowadź wartości całkowite, takie jak 10.</span><span class="sxs-lookup"><span data-stu-id="fe20d-130">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="fe20d-131">Kliknięcie przycisku **Utwórz** powoduje opublikowanie formularza na serwerze, gdzie informacje o filmie są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fe20d-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="fe20d-132">Następnie nastąpi przekierowanie do adresu URL */Movies* , w którym można zobaczyć nowo utworzony film na liście.</span><span class="sxs-lookup"><span data-stu-id="fe20d-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="fe20d-133">Utwórz kilka dodatkowych wpisów filmu.</span><span class="sxs-lookup"><span data-stu-id="fe20d-133">Create a couple more movie entries.</span></span> <span data-ttu-id="fe20d-134">Wypróbuj linki **Edytuj**, **szczegóły**i **Usuń** , które są wszystkie funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="fe20d-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="fe20d-135">Badanie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="fe20d-135">Examining the Generated Code</span></span>

<span data-ttu-id="fe20d-136">Otwórz plik *Controllers\MoviesController.cs* i przejrzyj wygenerowaną metodę `Index`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="fe20d-137">Poniżej przedstawiono część kontrolera filmu z metodą `Index`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="fe20d-138">Żądanie do kontrolera `Movies` zwraca wszystkie wpisy w tabeli `Movies`, a następnie przekazuje wyniki do widoku `Index`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="fe20d-139">Poniższy wiersz z klasy `MoviesController` tworzy wystąpienie kontekstu bazy danych filmu, jak opisano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="fe20d-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="fe20d-140">Możesz użyć kontekstu bazy danych filmu, aby wysyłać zapytania, edytować i usuwać filmy.</span><span class="sxs-lookup"><span data-stu-id="fe20d-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="fe20d-141">Modele silnie wpisane i @model słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="fe20d-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="fe20d-142">Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do szablonu widoku przy użyciu obiektu `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="fe20d-143">`ViewBag` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="fe20d-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="fe20d-144">MVC oferuje również możliwość przekazywania obiektów z *silną* typem do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="fe20d-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="fe20d-145">Takie silnie wpisane podejście umożliwia lepsze Sprawdzanie kodu w czasie kompilacji i bogatsze [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) w edytorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe20d-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="fe20d-146">Mechanizm tworzenia szkieletu w programie Visual Studio użył tego podejścia (czyli przekazywania *silnego* typu modelu) z klasą `MoviesController` i wyświetlania szablonów podczas tworzenia metod i widoków.</span><span class="sxs-lookup"><span data-stu-id="fe20d-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="fe20d-147">W pliku *Controllers\MoviesController.cs* Przejrzyj wygenerowaną metodę `Details`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="fe20d-148">Poniżej przedstawiono metodę `Details`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="fe20d-149">Parametr `id` jest zwykle przenoszona jako dane trasy, na przykład `http://localhost:1234/movies/details/1` ustawi kontroler na kontroler filmu, akcję `details` i `id` na 1.</span><span class="sxs-lookup"><span data-stu-id="fe20d-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="fe20d-150">Można również przekazać identyfikator z ciągiem zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fe20d-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="fe20d-151">Jeśli `Movie` zostanie znaleziona, wystąpienie modelu `Movie` zostanie przesłane do widoku `Details`:</span><span class="sxs-lookup"><span data-stu-id="fe20d-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="fe20d-152">Zapoznaj się z zawartością pliku *Views\Movies\Details.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="fe20d-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="fe20d-153">Dołączając instrukcję `@model` w górnej części pliku szablonu widoku, można określić typ obiektu, którego oczekuje widok.</span><span class="sxs-lookup"><span data-stu-id="fe20d-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="fe20d-154">Po utworzeniu kontrolera filmu program Visual Studio automatycznie dołączał następującą instrukcję `@model` w górnej części pliku *details. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="fe20d-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="fe20d-155">Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="fe20d-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="fe20d-156">Na przykład w szablonie *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i pomocników HTML [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) z silnie wpisanąm obiektem `Model`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="fe20d-157">Metody `Create` i `Edit` i szablony widoków również przekazują obiekt modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="fe20d-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="fe20d-158">Sprawdź szablon widoku *index. cshtml* i metodę `Index` w pliku *MoviesController.cs* .</span><span class="sxs-lookup"><span data-stu-id="fe20d-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="fe20d-159">Zwróć uwagę, jak kod tworzy obiekt [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) , gdy wywołuje metodę pomocnika `View` w metodzie `Index` akcji.</span><span class="sxs-lookup"><span data-stu-id="fe20d-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="fe20d-160">Następnie kod przekazuje tę `Movies` listę z metody akcji `Index` do widoku:</span><span class="sxs-lookup"><span data-stu-id="fe20d-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="fe20d-161">Po utworzeniu kontrolera filmu program Visual Studio automatycznie dołączał następującą instrukcję `@model` w górnej części pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="fe20d-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="fe20d-162">Ta `@model` dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony.</span><span class="sxs-lookup"><span data-stu-id="fe20d-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="fe20d-163">Na przykład w szablonie *index. cshtml* kod przechodzi przez film przez wykonanie instrukcji `foreach` na obiekcie `Model` o jednoznacznie określonym typie:</span><span class="sxs-lookup"><span data-stu-id="fe20d-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="fe20d-164">Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy obiekt `item` w pętli jest wpisywany jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="fe20d-165">W związku z innymi korzyściami oznacza to, że w edytorze kodu są dostępne sprawdzanie w czasie kompilacji kodu i pełna obsługa technologii IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="fe20d-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="fe20d-167">Praca z SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="fe20d-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="fe20d-168">Entity Framework Code First wykryła, że podane parametry połączenia z bazą danych wskazywały na `Movies` bazę danych, która jeszcze nie istnieje, więc Code First automatycznie utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="fe20d-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="fe20d-169">Możesz sprawdzić, czy został on utworzony, przeglądając folder *danych\_aplikacji* .</span><span class="sxs-lookup"><span data-stu-id="fe20d-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="fe20d-170">Jeśli plik *wideo. mdf* nie jest widoczny, kliknij przycisk **Pokaż wszystkie pliki** na pasku narzędzi **Eksplorator rozwiązań** , kliknij przycisk **odśwież** , a następnie rozwiń folder *dane\_aplikacji* .</span><span class="sxs-lookup"><span data-stu-id="fe20d-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="fe20d-171">Kliknij dwukrotnie ikonę *filmy. mdf* , aby otworzyć **Eksploratora serwera**, a następnie rozwiń folder **tabele** , aby wyświetlić tabelę filmy.</span><span class="sxs-lookup"><span data-stu-id="fe20d-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="fe20d-172">Zanotuj ikonę klucza obok pozycji ID.</span><span class="sxs-lookup"><span data-stu-id="fe20d-172">Note the key icon next to ID.</span></span> <span data-ttu-id="fe20d-173">Domyślnie EF ustawi właściwość o nazwie ID klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="fe20d-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="fe20d-174">Aby uzyskać więcej informacji na temat EF i MVC, zobacz Doskonały samouczek Dykstra na [MVC i EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="fe20d-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="fe20d-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="fe20d-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="fe20d-176">Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz polecenie **Pokaż dane tabeli** , aby wyświetlić utworzone dane.</span><span class="sxs-lookup"><span data-stu-id="fe20d-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="fe20d-177">Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz pozycję **Otwórz definicję tabeli** , aby wyświetlić strukturę tabeli utworzoną przez Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="fe20d-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="fe20d-178">Zwróć uwagę, jak schemat tabeli `Movies` mapuje do utworzonej wcześniej klasy `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="fe20d-179">Entity Framework Code First automatycznie utworzył ten schemat na podstawie klasy `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fe20d-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="fe20d-180">Po zakończeniu zamknij połączenie, klikając prawym przyciskiem myszy pozycję *MovieDBContext* i wybierając pozycję **Zamknij połączenie**.</span><span class="sxs-lookup"><span data-stu-id="fe20d-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="fe20d-181">(Jeśli połączenie nie zostanie zamknięte, podczas następnego uruchomienia projektu może wystąpić błąd).</span><span class="sxs-lookup"><span data-stu-id="fe20d-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

<span data-ttu-id="fe20d-182">Masz teraz bazę danych i strony do wyświetlania, edytowania, aktualizowania i usuwania danych.</span><span class="sxs-lookup"><span data-stu-id="fe20d-182">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="fe20d-183">W następnym samouczku sprawdzimy resztę kodu szkieletowego i dodamy metodę `SearchIndex` i widok `SearchIndex`, który umożliwi wyszukiwanie filmów w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fe20d-183">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="fe20d-184">Aby uzyskać więcej informacji na temat używania Entity Framework z MVC, zobacz [tworzenie Entity Framework modelu danych dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="fe20d-184">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fe20d-185">[Poprzednie](creating-a-connection-string.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="fe20d-185">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
