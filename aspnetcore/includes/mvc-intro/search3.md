---
ms.openlocfilehash: ba0d709d86227fa81eca9c9c1c6706018cc19f8d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074105"
---
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="4948d-101">Teraz gdy prześlesz wyszukiwania, adres URL zawiera ciąg zapytania wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="4948d-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="4948d-102">Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.</span><span class="sxs-lookup"><span data-stu-id="4948d-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Okno przeglądarki, przedstawiające Ciągwyszukiwania = ghost w adresie Url i filmów, zwrócone, Ghostbusters i Ghostbusters 2, zawierają ghost programu word](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="4948d-104">Następujący kod przedstawia zmiany `form` tag:</span><span class="sxs-lookup"><span data-stu-id="4948d-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="4948d-105">Dodawanie wyszukiwania według gatunku</span><span class="sxs-lookup"><span data-stu-id="4948d-105">Adding Search by genre</span></span>

<span data-ttu-id="4948d-106">Dodaj następujący kod `MovieGenreViewModel` klasy *modeli* folderu:</span><span class="sxs-lookup"><span data-stu-id="4948d-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="4948d-107">Model widoku gatunku filmu będzie zawierać:</span><span class="sxs-lookup"><span data-stu-id="4948d-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="4948d-108">Lista filmów.</span><span class="sxs-lookup"><span data-stu-id="4948d-108">A list of movies.</span></span>
   * <span data-ttu-id="4948d-109">A `SelectList` zawierającego listę gatunki.</span><span class="sxs-lookup"><span data-stu-id="4948d-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="4948d-110">Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.</span><span class="sxs-lookup"><span data-stu-id="4948d-110">This allows the user to select a genre from the list.</span></span>
   * <span data-ttu-id="4948d-111">`MovieGenre`, zawierającą wybrane gatunku.</span><span class="sxs-lookup"><span data-stu-id="4948d-111">`MovieGenre`, which contains the selected genre.</span></span>
   * <span data-ttu-id="4948d-112">`SearchString`, który zawiera tekst, użytkownicy wprowadzają w polu tekstowym wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="4948d-112">`SearchString`, which contains the text users enter in the search text box.</span></span>

<span data-ttu-id="4948d-113">Zastąp `Index` method in Class metoda `MoviesController.cs` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4948d-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="4948d-114">Poniższy kod jest `LINQ` zapytania, który pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4948d-114">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="4948d-115">`SelectList` Gatunków jest tworzona przy wyświetlaniu distinct gatunki (nie chcemy naszej listy wyboru mają zduplikowane gatunki).</span><span class="sxs-lookup"><span data-stu-id="4948d-115">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

<span data-ttu-id="4948d-116">Gdy użytkownik wyszukuje element, do wartości wyszukiwania są przechowywane w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="4948d-116">When the user searches for the item, the search value is retained in the search box.</span></span> <span data-ttu-id="4948d-117">Aby zachować wartości wyszukiwania, należy wypełnić `SearchString` właściwość o wartości wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="4948d-117">To retain the search value,  populate the `SearchString` property with the search value.</span></span> <span data-ttu-id="4948d-118">Wyszukaj wartość `searchString` parametr `Index` akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4948d-118">The search value is the `searchString` parameter for the `Index` controller action.</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="4948d-119">Dodawanie wyszukiwania według gatunku do widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="4948d-119">Adding search by genre to the Index view</span></span>

<span data-ttu-id="4948d-120">Aktualizacja `Index.cshtml` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4948d-120">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="4948d-121">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="4948d-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
<span data-ttu-id="4948d-122">W poprzednim kodzie `DisplayNameFor` sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="4948d-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="4948d-123">Ponieważ wyrażenie lambda jest kontrolowane zamiast oceniane, nie otrzymasz naruszenie zasad dostępu podczas `model`, `model.Movies`, lub `model.Movies[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="4948d-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="4948d-124">Kiedy jest obliczane wyrażenie lambda (na przykład `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="4948d-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="4948d-125">Testowanie aplikacji przez wyszukiwanie według gatunku, tytuł filmu i obu.</span><span class="sxs-lookup"><span data-stu-id="4948d-125">Test the app by searching by genre, by movie title, and by both.</span></span>
