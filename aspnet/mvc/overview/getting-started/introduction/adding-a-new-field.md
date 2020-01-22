---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Dodawanie nowego pola | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: d79655bfadff83095bf4cb84445f5efaf44d6a89
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519079"
---
# <a name="adding-a-new-field"></a><span data-ttu-id="210ae-102">Dodanie nowego pola</span><span class="sxs-lookup"><span data-stu-id="210ae-102">Adding a New Field</span></span>

<span data-ttu-id="210ae-103">Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="210ae-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="210ae-104">W tej sekcji użyjesz migracje Code First platformy Entity Framework do migrowania niektórych zmian do klas modelu, aby zmiana została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="210ae-105">Domyślnie w przypadku automatycznego tworzenia bazy danych za pomocą Code First Entity Framework, tak jak wcześniej w tym samouczku, Code First dodaje tabelę do bazy danych, aby ułatwić śledzenie, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana.</span><span class="sxs-lookup"><span data-stu-id="210ae-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="210ae-106">Jeśli nie są zsynchronizowane, Entity Framework zgłosi błąd.</span><span class="sxs-lookup"><span data-stu-id="210ae-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="210ae-107">Ułatwia to Śledzenie problemów w czasie opracowywania, które można znaleźć w innym miejscu (poprzez zaciemnienie błędów) w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="210ae-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="210ae-108">Konfigurowanie Migracje Code First do zmiany modelu</span><span class="sxs-lookup"><span data-stu-id="210ae-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="210ae-109">Przejdź do Eksplorator rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="210ae-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="210ae-110">Kliknij prawym przyciskiem myszy plik *wideo. mdf* i wybierz polecenie **Usuń** , aby usunąć bazę danych filmów.</span><span class="sxs-lookup"><span data-stu-id="210ae-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="210ae-111">Jeśli nie widzisz pliku *wideo. mdf* , kliknij ikonę **Pokaż wszystkie pliki** poniżej w czerwonym konspekcie.</span><span class="sxs-lookup"><span data-stu-id="210ae-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="210ae-112">Skompiluj aplikację, aby upewnić się, że nie występują żadne błędy.</span><span class="sxs-lookup"><span data-stu-id="210ae-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="210ae-113">W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Konsola menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="210ae-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Dodaj pakiet Man](adding-a-new-field/_static/image2.png)

<span data-ttu-id="210ae-115">W oknie **konsola Menedżera pakietów** w wierszu `PM>` wprowadź</span><span class="sxs-lookup"><span data-stu-id="210ae-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="210ae-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="210ae-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="210ae-117">Polecenie **enable-migrations** (pokazane powyżej) tworzy plik *Configuration.cs* w nowym folderze *migracji* .</span><span class="sxs-lookup"><span data-stu-id="210ae-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="210ae-118">Program Visual Studio otwiera plik *Configuration.cs* .</span><span class="sxs-lookup"><span data-stu-id="210ae-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="210ae-119">Zastąp metodę `Seed` w pliku *Configuration.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="210ae-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="210ae-120">Umieść kursor na czerwono linii falistej w obszarze `Movie` i kliknij pozycję `Show Potential Fixes`, a następnie kliknij pozycję **using** **MvcMovie. models;**</span><span class="sxs-lookup"><span data-stu-id="210ae-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="210ae-121">Spowoduje to dodanie następującej instrukcji using:</span><span class="sxs-lookup"><span data-stu-id="210ae-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="210ae-122">Migracje Code First wywołuje metodę `Seed` po każdej migracji (czyli wywołującej **aktualizację bazy danych** w konsoli Menedżera pakietów), a ta metoda aktualizuje wiersze, które zostały już wstawione lub wstawia je, jeśli jeszcze nie istnieją.</span><span class="sxs-lookup"><span data-stu-id="210ae-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="210ae-123">Metoda [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) w poniższym kodzie wykonuje operację "upsert":</span><span class="sxs-lookup"><span data-stu-id="210ae-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="210ae-124">Ponieważ metoda [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) jest uruchamiana przy każdej migracji, nie można tylko wstawić danych, ponieważ wiersze, które próbujesz dodać, będą już znajdować się po pierwszej migracji, która tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="210ae-125">Operacja "[upsert](http://en.wikipedia.org/wiki/Upsert)" zapobiega błędom, które mogą wystąpić w przypadku próby wstawienia wiersza, który już istnieje, ale zastępuje wszelkie zmiany danych, które mogły zostać wprowadzone podczas testowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="210ae-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="210ae-126">Dane testowe w niektórych tabelach mogą nie być potrzebne: w niektórych przypadkach, gdy zmieniasz dane podczas testowania, jeśli chcesz, aby zmiany były nadal wykonywane po aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="210ae-127">W takim przypadku należy wykonać operację warunkowego wstawiania: Wstaw wiersz tylko wtedy, gdy jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="210ae-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="210ae-128">Pierwszy parametr przesłany do metody [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) określa właściwość, która ma zostać użyta do sprawdzenia, czy wiersz już istnieje.</span><span class="sxs-lookup"><span data-stu-id="210ae-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="210ae-129">Dla danych filmu testowego, które zapewniasz, właściwość `Title` może zostać użyta do tego celu, ponieważ każdy tytuł na liście jest unikatowy:</span><span class="sxs-lookup"><span data-stu-id="210ae-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="210ae-130">Ten kod zakłada, że tytuły są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="210ae-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="210ae-131">Jeśli ręcznie dodasz zduplikowany tytuł, podczas następnego przeprowadzenia migracji wystąpi Poniższy wyjątek.</span><span class="sxs-lookup"><span data-stu-id="210ae-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
> <span data-ttu-id="210ae-132">*Sekwencja zawiera więcej niż jeden element*</span><span class="sxs-lookup"><span data-stu-id="210ae-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="210ae-133">Aby uzyskać więcej informacji na temat metody [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) , zobacz temat zapoznaj [się z metodą dr 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).</span><span class="sxs-lookup"><span data-stu-id="210ae-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>

<span data-ttu-id="210ae-134">**Naciśnij kombinację klawiszy Ctrl-Shift-B, aby skompilować projekt.** (Poniższe kroki zakończą się niepowodzeniem, jeśli w tym momencie nie zostanie utworzona).</span><span class="sxs-lookup"><span data-stu-id="210ae-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="210ae-135">Następnym krokiem jest utworzenie klasy `DbMigration` dla migracji początkowej.</span><span class="sxs-lookup"><span data-stu-id="210ae-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="210ae-136">Ta migracja tworzy nową bazę danych, dlatego plik *film. mdf* został usunięty w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="210ae-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="210ae-137">W oknie **konsola Menedżera pakietów** wprowadź polecenie `add-migration Initial`, aby utworzyć początkową migrację.</span><span class="sxs-lookup"><span data-stu-id="210ae-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="210ae-138">Nazwa "początkowa" jest dowolna i jest używana do natworzenia nazwy utworzonego pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="210ae-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="210ae-139">Migracje Code First tworzy inny plik klasy w folderze *migracji* (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="210ae-140">Nazwa pliku migracji jest wstępnie naprawiona sygnaturą czasową, aby pomóc w określeniu kolejności.</span><span class="sxs-lookup"><span data-stu-id="210ae-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="210ae-141">Przeanalizuj plik *{DateStamp}\_Initial.cs* , zawierający instrukcje tworzenia tabeli `Movies` dla bazy danych filmu.</span><span class="sxs-lookup"><span data-stu-id="210ae-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="210ae-142">Po zaktualizowaniu bazy danych w poniższych instrukcjach ten plik *{DateStamp}\_Initial.cs* zostanie uruchomiony i zostanie utworzony schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="210ae-143">Następnie zostanie uruchomiona Metoda **inicjatora** , aby wypełnić bazę danych z danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="210ae-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="210ae-144">W **konsoli Menedżera pakietów**wprowadź polecenie `update-database`, aby utworzyć bazę danych i uruchomić metodę `Seed`.</span><span class="sxs-lookup"><span data-stu-id="210ae-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="210ae-145">Jeśli zostanie wyświetlony komunikat o błędzie informujący o tym, że tabela już istnieje i nie można jej utworzyć, prawdopodobnie aplikacja została uruchomiona po usunięciu bazy danych programu i przed wykonaniem `update-database`.</span><span class="sxs-lookup"><span data-stu-id="210ae-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="210ae-146">W takim przypadku Usuń ponownie plik *film. mdf* i spróbuj ponownie wykonać `update-database` polecenie.</span><span class="sxs-lookup"><span data-stu-id="210ae-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="210ae-147">Jeśli nadal wystąpi błąd, Usuń folder migracji i zawartość, a następnie zacznij od instrukcji znajdujących się w górnej części tej strony (spowoduje to usunięcie pliku *film. mdf* , a następnie kontynuuj włączenie migracji).</span><span class="sxs-lookup"><span data-stu-id="210ae-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="210ae-148">Jeśli nadal pojawia się błąd, Otwórz Eksplorator obiektów SQL Server i Usuń bazę danych z listy.</span><span class="sxs-lookup"><span data-stu-id="210ae-148">If you still get an error, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="210ae-149">Uruchom aplikację i przejdź do adresu URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="210ae-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="210ae-150">Dane inicjatora są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="210ae-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="210ae-151">Dodawanie właściwości oceny do modelu filmu</span><span class="sxs-lookup"><span data-stu-id="210ae-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="210ae-152">Zacznij od dodania nowej właściwości `Rating` do istniejącej klasy `Movie`.</span><span class="sxs-lookup"><span data-stu-id="210ae-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="210ae-153">Otwórz plik *Models\Movie.cs* i dodaj Właściwość `Rating` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="210ae-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="210ae-154">Kompletna Klasa `Movie` teraz wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="210ae-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="210ae-155">Kompiluj aplikację (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="210ae-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="210ae-156">Ponieważ dodano nowe pole do klasy `Movie`, należy również zaktualizować *białe listy* powiązań, aby ta nowa właściwość została uwzględniona.</span><span class="sxs-lookup"><span data-stu-id="210ae-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="210ae-157">Zaktualizuj `bind` atrybutu dla `Create` i `Edit` metod akcji, aby uwzględnić Właściwość `Rating`:</span><span class="sxs-lookup"><span data-stu-id="210ae-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="210ae-158">Należy również zaktualizować szablony widoków, aby wyświetlić, utworzyć i edytować nową właściwość `Rating` w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="210ae-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="210ae-159">Otwórz plik *\Views\Movies\Index.cshtml* i Dodaj nagłówek kolumny `<th>Rating</th>` tuż po kolumnie **Price** .</span><span class="sxs-lookup"><span data-stu-id="210ae-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="210ae-160">Następnie Dodaj `<td>` kolumnę blisko końca szablonu, aby renderować `@item.Rating` wartość.</span><span class="sxs-lookup"><span data-stu-id="210ae-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="210ae-161">Poniżej znajduje się opis zaktualizowanego szablonu widoku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="210ae-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="210ae-162">Następnie otwórz plik *\Views\Movies\Create.cshtml* i dodaj pole `Rating` z następującym wyróżnionym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="210ae-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighted markup.</span></span> <span data-ttu-id="210ae-163">Spowoduje to renderowanie pola tekstowego, aby można było określić klasyfikację po utworzeniu nowego filmu.</span><span class="sxs-lookup"><span data-stu-id="210ae-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="210ae-164">Kod aplikacji został już zaktualizowany, aby obsługiwał nową właściwość `Rating`.</span><span class="sxs-lookup"><span data-stu-id="210ae-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="210ae-165">Uruchom aplikację i przejdź do adresu URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="210ae-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="210ae-166">Gdy to zrobisz, zobaczysz jeden z następujących błędów:</span><span class="sxs-lookup"><span data-stu-id="210ae-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="210ae-167">Model z kopią zapasową kontekstu "MovieDBContext" został zmieniony od czasu utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="210ae-168">Rozważ użycie Migracje Code First do zaktualizowania bazy danych (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="210ae-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="210ae-169">Ten błąd jest wyświetlany, ponieważ zaktualizowana Klasa modelu `Movie` w aplikacji jest inna niż schemat tabeli `Movie` istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="210ae-170">(Brak kolumny `Rating` w tabeli bazy danych).</span><span class="sxs-lookup"><span data-stu-id="210ae-170">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="210ae-171">Istnieje kilka metod rozpoznawania błędu:</span><span class="sxs-lookup"><span data-stu-id="210ae-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="210ae-172">Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="210ae-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="210ae-173">To podejście jest bardzo wygodne w cyklu programowania, gdy przeprowadzasz aktywne Programowanie na testowej bazie danych. pozwala ona szybko rozwijać model i schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="210ae-174">Minusem, mimo że utracisz istniejące dane w bazie danych, więc *nie* chcesz używać tego podejścia w produkcyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="210ae-175">Użycie inicjatora do automatycznego umieszczania bazy danych z danymi testowymi jest często wydajnym sposobem na tworzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="210ae-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="210ae-176">Aby uzyskać więcej informacji na temat Entity Framework inicjatorów bazy danych, zobacz [samouczek ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="210ae-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="210ae-177">Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu.</span><span class="sxs-lookup"><span data-stu-id="210ae-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="210ae-178">Zaletą tego podejścia jest utrzymywanie danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="210ae-179">Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="210ae-180">Użyj Migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-180">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="210ae-181">W tym samouczku będziemy używać Migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="210ae-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="210ae-182">Zaktualizuj metodę inicjatora, aby zapewnić wartość nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="210ae-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="210ae-183">Otwórz plik Migrations\Configuration.cs i Dodaj pole Rating do każdego obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="210ae-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="210ae-184">Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="210ae-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="210ae-185">Polecenie `add-migration` informuje platformę migracji, aby przeanalizować bieżący model filmu z bieżącym schematem bazy danych filmu i utworzyć wymagany kod w celu przeprowadzenia migracji bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="210ae-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="210ae-186">*Klasyfikacja* nazw jest dowolna i jest używana do nazwy pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="210ae-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="210ae-187">Warto użyć zrozumiałej nazwy dla kroku migracji.</span><span class="sxs-lookup"><span data-stu-id="210ae-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="210ae-188">Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę pochodną `DbMigration`, a w metodzie `Up` można zobaczyć kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="210ae-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="210ae-189">Skompiluj rozwiązanie, a następnie wprowadź `update-database` polecenie w oknie **konsola Menedżera pakietów** .</span><span class="sxs-lookup"><span data-stu-id="210ae-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="210ae-190">Na poniższej ilustracji przedstawiono dane wyjściowe w oknie **konsola Menedżera pakietów** ( *Klasyfikacja* sygnatury czasowej w toku będzie inna).</span><span class="sxs-lookup"><span data-stu-id="210ae-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="210ae-191">Uruchom aplikację jeszcze raz i przejdź do adresu URL/Movies.</span><span class="sxs-lookup"><span data-stu-id="210ae-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="210ae-192">Możesz zobaczyć nowe pole oceny.</span><span class="sxs-lookup"><span data-stu-id="210ae-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="210ae-193">Kliknij link **Utwórz nowy** , aby dodać nowy film.</span><span class="sxs-lookup"><span data-stu-id="210ae-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="210ae-194">Należy pamiętać, że można dodać klasyfikację.</span><span class="sxs-lookup"><span data-stu-id="210ae-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="210ae-196">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="210ae-196">Click **Create**.</span></span> <span data-ttu-id="210ae-197">Nowy film, łącznie z klasyfikacją, znajduje się teraz na liście filmów:</span><span class="sxs-lookup"><span data-stu-id="210ae-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="210ae-199">Teraz, gdy projekt korzysta z migracji, nie trzeba porzucić bazy danych podczas dodawania nowego pola lub w inny sposób aktualizacji schematu.</span><span class="sxs-lookup"><span data-stu-id="210ae-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="210ae-200">W następnej sekcji wprowadzimy więcej zmian schematu i użyjesz migracji w celu zaktualizowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="210ae-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="210ae-201">Należy również dodać pole `Rating` do szablonów Edytuj, szczegóły i Usuń.</span><span class="sxs-lookup"><span data-stu-id="210ae-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="210ae-202">W oknie **konsoli Menedżera pakietów** można ponownie wprowadzić polecenie "Update-Database" i nie będzie można uruchomić kodu migracji, ponieważ schemat jest zgodny z modelem.</span><span class="sxs-lookup"><span data-stu-id="210ae-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="210ae-203">Jednak uruchomienie polecenie "Update-Database" spowoduje ponowne uruchomienie `Seed`ej metody, a w przypadku zmiany dowolnego z danych inicjatora zmiany zostaną utracone, ponieważ metoda `Seed` upserts dane.</span><span class="sxs-lookup"><span data-stu-id="210ae-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="210ae-204">Więcej informacji na temat metody `Seed` można znaleźć w popularnym [samouczku ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)Dykstra.</span><span class="sxs-lookup"><span data-stu-id="210ae-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="210ae-205">W tej sekcji pokazano, jak można modyfikować obiekty modelu i zachować synchronizację bazy danych ze zmianami.</span><span class="sxs-lookup"><span data-stu-id="210ae-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="210ae-206">Poznasz również sposób wypełniania nowo utworzonej bazy danych z przykładowymi danymi, dzięki czemu można wypróbować scenariusze.</span><span class="sxs-lookup"><span data-stu-id="210ae-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="210ae-207">To właśnie krótkie wprowadzenie do Code First, zobacz [tworzenie Entity Framework modelu danych dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) w celu uzyskania pełniejszego samouczka dotyczącego tematu.</span><span class="sxs-lookup"><span data-stu-id="210ae-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="210ae-208">Następnie Przyjrzyjmy się sposobom dodawania bogatszej logiki walidacji do klas modelu i włączania niektórych reguł firmowych.</span><span class="sxs-lookup"><span data-stu-id="210ae-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="210ae-209">[Poprzedni](adding-search.md)
> [Następny](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="210ae-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
