---
ms.openlocfilehash: e098ad497b13b4add583de017885cdbed952a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57069647"
---
<!-- This include not used by windows version -->
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="fac0e-101">Dodawanie nowego pola do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="fac0e-101">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="fac0e-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fac0e-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fac0e-103">W tym samouczku spowoduje dodanie nowego pola do `Movies` tabeli.</span><span class="sxs-lookup"><span data-stu-id="fac0e-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="fac0e-104">Utworzymy porzucenia bazy danych i utworzyć nową w przypadku zmiany schematu (Dodawanie nowego pola).</span><span class="sxs-lookup"><span data-stu-id="fac0e-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="fac0e-105">Ten przepływ pracy działa również na wczesnym etapie projektowania, gdy nie ma żadnych danych produkcyjnych perserve.</span><span class="sxs-lookup"><span data-stu-id="fac0e-105">This workflow works well early in development when we don't have any production data to perserve.</span></span>

<span data-ttu-id="fac0e-106">Gdy Twoja aplikacja jest wdrożona i mieć dane potrzebne do perserve, nie można porzucić użytkownika bazy danych, gdy zajdzie potrzeba zmiany schematu.</span><span class="sxs-lookup"><span data-stu-id="fac0e-106">Once your app is deployed and you have data that you need to perserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="fac0e-107">Entity Framework [migracje Code First](/ef/core/get-started/aspnetcore/new-db) można aktualizować schematu i migracji bazy danych bez utraty danych.</span><span class="sxs-lookup"><span data-stu-id="fac0e-107">Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="fac0e-108">Migracje jest popularnych funkcji podczas przy użyciu programu SQL Server, ale SQLlite nie obsługuje wielu operacji migracji schematu więc tylko bardzo prosty sposób migracji są możliwe.</span><span class="sxs-lookup"><span data-stu-id="fac0e-108">Migrations is a popular feature when using SQL Server, but SQLlite doesn't support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="fac0e-109">Zobacz [ograniczenia SQLite](/ef/core/providers/sqlite/limitations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="fac0e-109">See [SQLite Limitations](/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="fac0e-110">Dodawanie właściwości klasyfikacji do modelu Movie</span><span class="sxs-lookup"><span data-stu-id="fac0e-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="fac0e-111">Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="fac0e-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

<span data-ttu-id="fac0e-112">Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania listy dozwolonych adresów, dzięki tej nowej właściwości zostaną dołączone.</span><span class="sxs-lookup"><span data-stu-id="fac0e-112">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="fac0e-113">W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="fac0e-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="fac0e-114">Możesz również należy zaktualizować szablonów widoków w celu wyświetlania, tworzenia i edytowania nowy `Rating` właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="fac0e-114">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="fac0e-115">Edytuj */Views/Movies/Index.cshtml* pliku i Dodaj `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="fac0e-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="fac0e-116">Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="fac0e-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="fac0e-117">Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazy danych, aby uwzględnić nowe pole.</span><span class="sxs-lookup"><span data-stu-id="fac0e-117">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="fac0e-118">Jeśli uruchamiasz go teraz, uzyskasz następujące `SqliteException`:</span><span class="sxs-lookup"><span data-stu-id="fac0e-118">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="fac0e-119">Widzisz ten błąd, ponieważ zaktualizowane klasy modelu Movie różni się od schematu tabeli filmu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fac0e-119">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="fac0e-120">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="fac0e-120">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="fac0e-121">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="fac0e-121">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="fac0e-122">Upuść bazę danych i mieć automatycznie ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu w programie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fac0e-122">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="fac0e-123">W przypadku tej metody, tracisz istniejących danych w bazie danych, więc nie możesz tego zrobić za pomocą produkcyjnej bazy danych!</span><span class="sxs-lookup"><span data-stu-id="fac0e-123">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="fac0e-124">Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fac0e-124">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="fac0e-125">Ręcznie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="fac0e-125">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="fac0e-126">Zaletą tego podejścia jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="fac0e-126">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="fac0e-127">Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.</span><span class="sxs-lookup"><span data-stu-id="fac0e-127">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="fac0e-128">Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="fac0e-128">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="fac0e-129">W tym samouczku będziemy Porzuć i ponownie utworzyć bazę danych, po zmianie schematu.</span><span class="sxs-lookup"><span data-stu-id="fac0e-129">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="fac0e-130">Uruchom następujące polecenie w terminalu, aby porzucić bazy danych:</span><span class="sxs-lookup"><span data-stu-id="fac0e-130">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="fac0e-131">Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="fac0e-131">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="fac0e-132">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="fac0e-132">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="fac0e-133">Dodaj `Rating` pole `Edit`, `Details`, i `Delete` widoku.</span><span class="sxs-lookup"><span data-stu-id="fac0e-133">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="fac0e-134">Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="fac0e-134">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="fac0e-135">Szablony.</span><span class="sxs-lookup"><span data-stu-id="fac0e-135">templates.</span></span>
