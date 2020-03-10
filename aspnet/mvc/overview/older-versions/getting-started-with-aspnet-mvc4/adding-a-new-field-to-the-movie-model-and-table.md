---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Dodawanie nowego pola do modelu i tabeli filmów | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615088"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="c9637-104">Dodawanie nowego pola do modelu Movie i tabeli</span><span class="sxs-lookup"><span data-stu-id="c9637-104">Adding a New Field to the Movie Model and Table</span></span>

<span data-ttu-id="c9637-105">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c9637-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="c9637-106">Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c9637-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="c9637-107">Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.</span><span class="sxs-lookup"><span data-stu-id="c9637-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="c9637-108">W tej sekcji użyjesz migracje Code First platformy Entity Framework do migrowania niektórych zmian do klas modelu, aby zmiana została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="c9637-109">Domyślnie w przypadku automatycznego tworzenia bazy danych za pomocą Code First Entity Framework, tak jak wcześniej w tym samouczku, Code First dodaje tabelę do bazy danych, aby ułatwić śledzenie, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana.</span><span class="sxs-lookup"><span data-stu-id="c9637-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="c9637-110">Jeśli nie są zsynchronizowane, Entity Framework zgłosi błąd.</span><span class="sxs-lookup"><span data-stu-id="c9637-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="c9637-111">Ułatwia to Śledzenie problemów w czasie opracowywania, które można znaleźć w innym miejscu (poprzez zaciemnienie błędów) w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c9637-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="c9637-112">Konfigurowanie Migracje Code First do zmiany modelu</span><span class="sxs-lookup"><span data-stu-id="c9637-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="c9637-113">Jeśli używasz programu Visual Studio 2012, dwukrotnie kliknij plik *film wideo. mdf* w Eksplorator rozwiązań, aby otworzyć narzędzie Database.</span><span class="sxs-lookup"><span data-stu-id="c9637-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="c9637-114">Visual Studio Express dla sieci Web pokaże Eksplorator bazy danych, program Visual Studio 2012 będzie wyświetlał Eksplorator serwera.</span><span class="sxs-lookup"><span data-stu-id="c9637-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="c9637-115">Jeśli używasz programu Visual Studio 2010, użyj Eksplorator obiektów SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c9637-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="c9637-116">W narzędziu bazy danych (Eksplorator bazy danych, Eksplorator serwera lub Eksplorator obiektów SQL Server) kliknij prawym przyciskiem myszy pozycję `MovieDBContext` i wybierz pozycję Usuń, aby **usunąć** bazę danych filmów.</span><span class="sxs-lookup"><span data-stu-id="c9637-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="c9637-117">Przejdź wstecz do Eksplorator rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="c9637-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="c9637-118">Kliknij prawym przyciskiem myszy plik *wideo. mdf* i wybierz polecenie **Usuń** , aby usunąć bazę danych filmów.</span><span class="sxs-lookup"><span data-stu-id="c9637-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="c9637-119">Skompiluj aplikację, aby upewnić się, że nie występują żadne błędy.</span><span class="sxs-lookup"><span data-stu-id="c9637-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="c9637-120">W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Konsola menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="c9637-120">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Dodaj pakiet Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="c9637-122">W oknie **konsola Menedżera pakietów** w wierszu `PM>` wprowadź wartość "Enable-migrations-ContextTypeName MvcMovie. models. MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="c9637-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="c9637-123">Polecenie **enable-migrations** (pokazane powyżej) tworzy plik *Configuration.cs* w nowym folderze *migracji* .</span><span class="sxs-lookup"><span data-stu-id="c9637-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="c9637-124">Program Visual Studio otwiera plik *Configuration.cs* .</span><span class="sxs-lookup"><span data-stu-id="c9637-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="c9637-125">Zastąp metodę `Seed` w pliku *Configuration.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c9637-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="c9637-126">Kliknij prawym przyciskiem myszy czerwoną linię falistej w obszarze `Movie` i wybierz pozycję **Rozwiąż** **przy użyciu** **MvcMovie. models;**</span><span class="sxs-lookup"><span data-stu-id="c9637-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="c9637-127">Spowoduje to dodanie następującej instrukcji using:</span><span class="sxs-lookup"><span data-stu-id="c9637-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="c9637-128">Migracje Code First wywołuje metodę `Seed` po każdej migracji (czyli wywołującej **aktualizację bazy danych** w konsoli Menedżera pakietów), a ta metoda aktualizuje wiersze, które zostały już wstawione lub wstawia je, jeśli jeszcze nie istnieją.</span><span class="sxs-lookup"><span data-stu-id="c9637-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>

<span data-ttu-id="c9637-129">**Naciśnij kombinację klawiszy Ctrl-Shift-B, aby skompilować projekt.** (Poniższe kroki zakończą się niepowodzeniem, jeśli w tym momencie nie zostanie utworzona).</span><span class="sxs-lookup"><span data-stu-id="c9637-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="c9637-130">Następnym krokiem jest utworzenie klasy `DbMigration` dla migracji początkowej.</span><span class="sxs-lookup"><span data-stu-id="c9637-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="c9637-131">Ta migracja umożliwia utworzenie nowej bazy danych, dlatego plik *film. mdf* został usunięty w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="c9637-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="c9637-132">W oknie **konsola Menedżera pakietów** wprowadź polecenie "Add-Migration Initial" w celu utworzenia początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="c9637-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="c9637-133">Nazwa "początkowa" jest dowolna i jest używana do natworzenia nazwy utworzonego pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="c9637-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="c9637-134">Migracje Code First tworzy inny plik klasy w folderze *migracji* (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="c9637-135">Nazwa pliku migracji jest wstępnie naprawiona sygnaturą czasową, aby pomóc w określeniu kolejności.</span><span class="sxs-lookup"><span data-stu-id="c9637-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="c9637-136">Przeanalizuj plik *{DateStamp}\_Initial.cs* , zawierający instrukcje tworzenia tabeli filmów dla bazy danych filmu.</span><span class="sxs-lookup"><span data-stu-id="c9637-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="c9637-137">Po zaktualizowaniu bazy danych w poniższych instrukcjach ten plik *{DateStamp}\_Initial.cs* zostanie uruchomiony i zostanie utworzony schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="c9637-138">Następnie zostanie uruchomiona Metoda **inicjatora** , aby wypełnić bazę danych z danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="c9637-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="c9637-139">W **konsoli Menedżera pakietów**wprowadź polecenie "Update-Database", aby utworzyć bazę danych i uruchomić metodę **inicjatora** .</span><span class="sxs-lookup"><span data-stu-id="c9637-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="c9637-140">Jeśli zostanie wyświetlony komunikat o błędzie informujący o tym, że tabela już istnieje i nie można jej utworzyć, prawdopodobnie aplikacja została uruchomiona po usunięciu bazy danych programu i przed wykonaniem `update-database`.</span><span class="sxs-lookup"><span data-stu-id="c9637-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="c9637-141">W takim przypadku Usuń ponownie plik *film. mdf* i spróbuj ponownie wykonać `update-database` polecenie.</span><span class="sxs-lookup"><span data-stu-id="c9637-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="c9637-142">Jeśli nadal wystąpi błąd, Usuń folder migracji i zawartość, a następnie zacznij od instrukcji znajdujących się w górnej części tej strony (spowoduje to usunięcie pliku *film. mdf* , a następnie kontynuuj włączenie migracji).</span><span class="sxs-lookup"><span data-stu-id="c9637-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="c9637-143">Uruchom aplikację i przejdź do adresu URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="c9637-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="c9637-144">Dane inicjatora są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="c9637-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c9637-145">Dodawanie właściwości oceny do modelu filmu</span><span class="sxs-lookup"><span data-stu-id="c9637-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c9637-146">Zacznij od dodania nowej właściwości `Rating` do istniejącej klasy `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c9637-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="c9637-147">Otwórz plik *Models\Movie.cs* i dodaj Właściwość `Rating` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c9637-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="c9637-148">Kompletna Klasa `Movie` teraz wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="c9637-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="c9637-149">Kompilowanie aplikacji za pomocą polecenia **kompiluj** &gt;**Kompiluj film** menu lub naciskając klawisze CTRL-SHIFT-B.</span><span class="sxs-lookup"><span data-stu-id="c9637-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="c9637-150">Teraz, po zaktualizowaniu klasy `Model` należy również zaktualizować szablony widoków *\Views\Movies\Index.cshtml* i *\Views\Movies\Create.cshtml* w celu wyświetlenia nowej właściwości `Rating` w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c9637-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="c9637-151">Otwórz plik<em>\Views\Movies\Index.cshtml</em> i Dodaj nagłówek kolumny `<th>Rating</th>` tuż po kolumnie <strong>Price</strong> .</span><span class="sxs-lookup"><span data-stu-id="c9637-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="c9637-152">Następnie Dodaj `<td>` kolumnę blisko końca szablonu, aby renderować `@item.Rating` wartość.</span><span class="sxs-lookup"><span data-stu-id="c9637-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="c9637-153">Poniżej znajduje się opis zaktualizowanego szablonu widoku <em>index. cshtml</em> :</span><span class="sxs-lookup"><span data-stu-id="c9637-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="c9637-154">Następnie otwórz plik *\Views\Movies\Create.cshtml* i Dodaj następujący znacznik w górnej części formularza.</span><span class="sxs-lookup"><span data-stu-id="c9637-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="c9637-155">Spowoduje to renderowanie pola tekstowego, aby można było określić klasyfikację po utworzeniu nowego filmu.</span><span class="sxs-lookup"><span data-stu-id="c9637-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="c9637-156">Kod aplikacji został już zaktualizowany, aby obsługiwał nową właściwość `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c9637-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="c9637-157">Teraz uruchom aplikację i przejdź do adresu URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="c9637-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="c9637-158">Gdy to zrobisz, zobaczysz jeden z następujących błędów:</span><span class="sxs-lookup"><span data-stu-id="c9637-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="c9637-159">Ten błąd jest wyświetlany, ponieważ zaktualizowana Klasa modelu `Movie` w aplikacji jest inna niż schemat tabeli `Movie` istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="c9637-160">(Brak kolumny `Rating` w tabeli bazy danych).</span><span class="sxs-lookup"><span data-stu-id="c9637-160">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c9637-161">Istnieje kilka metod rozpoznawania błędu:</span><span class="sxs-lookup"><span data-stu-id="c9637-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="c9637-162">Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="c9637-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="c9637-163">To podejście jest bardzo wygodne podczas aktywnego programowania na testowej bazie danych. pozwala ona szybko rozwijać model i schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="c9637-164">Minusem, mimo że utracisz istniejące dane w bazie danych, więc *nie* chcesz używać tego podejścia w produkcyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="c9637-165">Użycie inicjatora do automatycznego umieszczania bazy danych z danymi testowymi jest często wydajnym sposobem na tworzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c9637-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="c9637-166">Aby uzyskać więcej informacji o Entity Framework inicjatorach baz danych, zobacz [ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)Dykstra.</span><span class="sxs-lookup"><span data-stu-id="c9637-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="c9637-167">Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu.</span><span class="sxs-lookup"><span data-stu-id="c9637-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c9637-168">Zaletą tego podejścia jest utrzymywanie danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c9637-169">Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="c9637-170">Użyj Migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9637-170">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="c9637-171">W tym samouczku będziemy używać Migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="c9637-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="c9637-172">Zaktualizuj metodę inicjatora, aby zapewnić wartość nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="c9637-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="c9637-173">Otwórz plik Migrations\Configuration.cs i Dodaj pole Rating do każdego obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="c9637-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="c9637-174">Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c9637-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="c9637-175">Polecenie `add-migration` informuje platformę migracji, aby przeanalizować bieżący model filmu z bieżącym schematem bazy danych filmu i utworzyć wymagany kod w celu przeprowadzenia migracji bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="c9637-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="c9637-176">AddRatingMig jest dowolny i jest używany do nazwy pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="c9637-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="c9637-177">Warto użyć zrozumiałej nazwy dla kroku migracji.</span><span class="sxs-lookup"><span data-stu-id="c9637-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="c9637-178">Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę pochodną `DbMigration`, a w metodzie `Up` można zobaczyć kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="c9637-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="c9637-179">Skompiluj rozwiązanie, a następnie wprowadź polecenie "Update-Database" w oknie **konsola Menedżera pakietów** .</span><span class="sxs-lookup"><span data-stu-id="c9637-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="c9637-180">Na poniższej ilustracji przedstawiono dane wyjściowe w oknie **konsola Menedżera pakietów** (Sygnatura daty oczekująca na AddRatingMig będzie inna).</span><span class="sxs-lookup"><span data-stu-id="c9637-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="c9637-181">Uruchom aplikację jeszcze raz i przejdź do adresu URL/Movies.</span><span class="sxs-lookup"><span data-stu-id="c9637-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="c9637-182">Możesz zobaczyć nowe pole oceny.</span><span class="sxs-lookup"><span data-stu-id="c9637-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="c9637-183">Kliknij link **Utwórz nowy** , aby dodać nowy film.</span><span class="sxs-lookup"><span data-stu-id="c9637-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="c9637-184">Należy pamiętać, że można dodać klasyfikację.</span><span class="sxs-lookup"><span data-stu-id="c9637-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="c9637-186">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="c9637-186">Click **Create**.</span></span> <span data-ttu-id="c9637-187">Nowy film, łącznie z klasyfikacją, znajduje się teraz na liście filmów:</span><span class="sxs-lookup"><span data-stu-id="c9637-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="c9637-189">Należy również dodać pole `Rating` do szablonów widoku Edytuj, szczegóły i SearchIndex.</span><span class="sxs-lookup"><span data-stu-id="c9637-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="c9637-190">W oknie **konsoli Menedżera pakietów** można ponownie wprowadzić polecenie "Update-Database" i nie zostaną wprowadzone żadne zmiany, ponieważ schemat jest zgodny z modelem.</span><span class="sxs-lookup"><span data-stu-id="c9637-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="c9637-191">W tej sekcji pokazano, jak można modyfikować obiekty modelu i zachować synchronizację bazy danych ze zmianami.</span><span class="sxs-lookup"><span data-stu-id="c9637-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="c9637-192">Poznasz również sposób wypełniania nowo utworzonej bazy danych z przykładowymi danymi, dzięki czemu można wypróbować scenariusze.</span><span class="sxs-lookup"><span data-stu-id="c9637-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="c9637-193">Następnie Przyjrzyjmy się sposobom dodawania bogatszej logiki walidacji do klas modelu i włączania niektórych reguł firmowych.</span><span class="sxs-lookup"><span data-stu-id="c9637-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c9637-194">[Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="c9637-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
