---
ms.openlocfilehash: 24726fba7f431f701b264a988a8de1b67d41d8a2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073259"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="17c23-101">Dodawanie wyszukiwania do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="17c23-101">Add search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="17c23-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17c23-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="17c23-103">W tej sekcji możesz dodać możliwości wyszukiwania do `Index` metody akcji, która umożliwia wyszukiwanie filmów przez *gatunku* lub *nazwa*.</span><span class="sxs-lookup"><span data-stu-id="17c23-103">In this section you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="17c23-104">Aktualizacja `Index` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="17c23-104">Update the `Index` method with the following code:</span></span>
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

<span data-ttu-id="17c23-105">W pierwszym wierszu `Index` tworzy metody akcji [LINQ](/dotnet/standard/using-linq) zapytanie, aby wybrać filmów:</span><span class="sxs-lookup"><span data-stu-id="17c23-105">The first line of the `Index` action method creates a [LINQ](/dotnet/standard/using-linq) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="17c23-106">Zapytanie jest *tylko* zdefiniowane w tym momencie, ma ona **nie** zostały uruchomione dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="17c23-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="17c23-107">Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania na wartość ciągu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="17c23-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

<span data-ttu-id="17c23-108">`s => s.Title.Contains()` Powyższy kod jest [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="17c23-108">The `s => s.Title.Contains()` code above is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="17c23-109">Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/standard/using-linq) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](/dotnet/api/system.linq.enumerable.where) metody lub `Contains` (używane w powyższym kodzie).</span><span class="sxs-lookup"><span data-stu-id="17c23-109">Lambdas are used in method-based [LINQ](/dotnet/standard/using-linq) queries as arguments to standard query operator methods such as the [Where](/dotnet/api/system.linq.enumerable.where) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="17c23-110">Zapytania LINQ nie są wykonywane, gdy są one definiowane lub ich modyfikacji przez wywołanie metody, takie jak `Where`, `Contains` lub `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="17c23-110">LINQ queries are not executed when they're defined or when they're modified by calling a method such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="17c23-111">Przeciwnie wykonanie zapytania jest odroczone.</span><span class="sxs-lookup"><span data-stu-id="17c23-111">Rather, query execution is deferred.</span></span>  <span data-ttu-id="17c23-112">Oznacza to, że wyniku obliczenia wyrażenia zostanie opóźnione, dopóki wartość zrealizowane faktycznie jest powtarzana lub `ToListAsync` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="17c23-112">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="17c23-113">Aby uzyskać więcej informacji na temat wykonywania zapytań z opóźnieniem, zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="17c23-113">For more information about deferred query execution, see [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span></span>

<span data-ttu-id="17c23-114">Uwaga: [Zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda jest uruchomiona w bazie danych nie jest w kodzie języka c# pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="17c23-114">Note: The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="17c23-115">Rozróżnianie wielkości liter w zapytaniu, zależy od bazy danych i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="17c23-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="17c23-116">W programie SQL Server [zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="17c23-116">On SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="17c23-117">W SQLlite przy użyciu sortowania domyślnego, jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="17c23-117">In SQLlite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="17c23-118">Przejdź do adresu `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="17c23-118">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="17c23-119">Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17c23-119">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="17c23-120">Wyświetlane są filtrowane filmów.</span><span class="sxs-lookup"><span data-stu-id="17c23-120">The filtered movies are displayed.</span></span>

![Widok indeksu](~/tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="17c23-122">Jeśli zmienisz podpis `Index` metoda będzie miała parametru o nazwie `id`, `id` parametr będzie odpowiadał opcjonalnego `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="17c23-122">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
