---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie za pomocą Entity Framework w aplikacji ASP.NET MVC | Microsoft Docs'
author: tdykstra
description: W tym samouczku dodasz funkcje sortowania, filtrowania i stronicowania na stronie indeksu **uczniów** . Możesz również utworzyć prostą stronę grupowania.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: d9fadc12aa83a8095f364cf39e5376243a7d0670
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810750"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="b355b-104">Samouczek: Dodawanie sortowania, filtrowania i stronicowania za pomocą Entity Framework w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b355b-104">Tutorial: Add sorting, filtering, and paging with the Entity Framework in an ASP.NET MVC application</span></span>

<span data-ttu-id="b355b-105">W [poprzednim samouczku](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)zaimplementowano zestaw stron sieci Web dla podstawowych operacji CRUD dla `Student` jednostek.</span><span class="sxs-lookup"><span data-stu-id="b355b-105">In the [previous tutorial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="b355b-106">W tym samouczku dodasz funkcje sortowania, filtrowania i stronicowania na stronie indeksu **uczniów** .</span><span class="sxs-lookup"><span data-stu-id="b355b-106">In this tutorial you add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="b355b-107">Możesz również utworzyć prostą stronę grupowania.</span><span class="sxs-lookup"><span data-stu-id="b355b-107">You also create a simple grouping page.</span></span>

<span data-ttu-id="b355b-108">Na poniższej ilustracji przedstawiono, jak będzie wyglądać strona po zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="b355b-108">The following image shows what the page will look like when you're done.</span></span> <span data-ttu-id="b355b-109">Nagłówki kolumn to linki, które użytkownik może kliknąć, aby posortować według tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="b355b-109">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="b355b-110">Wielokrotne kliknięcie nagłówka kolumny powoduje przełączenie między rosnącą i malejącą kolejnością sortowania.</span><span class="sxs-lookup"><span data-stu-id="b355b-110">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="b355b-112">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="b355b-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b355b-113">Dodaj linki sortowania kolumn</span><span class="sxs-lookup"><span data-stu-id="b355b-113">Add column sort links</span></span>
> * <span data-ttu-id="b355b-114">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="b355b-114">Add a Search box</span></span>
> * <span data-ttu-id="b355b-115">Dodaj stronicowanie</span><span class="sxs-lookup"><span data-stu-id="b355b-115">Add paging</span></span>
> * <span data-ttu-id="b355b-116">Tworzenie strony informacje</span><span class="sxs-lookup"><span data-stu-id="b355b-116">Create an About page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b355b-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b355b-117">Prerequisites</span></span>

* [<span data-ttu-id="b355b-118">Implementowanie podstawowych funkcji CRUD</span><span class="sxs-lookup"><span data-stu-id="b355b-118">Implementing Basic CRUD Functionality</span></span>](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a><span data-ttu-id="b355b-119">Dodaj linki sortowania kolumn</span><span class="sxs-lookup"><span data-stu-id="b355b-119">Add column sort links</span></span>

<span data-ttu-id="b355b-120">Aby dodać sortowanie na stronie indeksu ucznia, należy zmienić `Index` metodę `Student` kontrolera `Student` i dodać kod do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="b355b-120">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="b355b-121">Dodawanie funkcji sortowania do metody index</span><span class="sxs-lookup"><span data-stu-id="b355b-121">Add sorting functionality to the Index method</span></span>

- <span data-ttu-id="b355b-122">W *Controllers\StudentController.cs*Zastąp `Index` metodę następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b355b-122">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="b355b-123">Ten kod otrzymuje `sortOrder` parametr z ciągu zapytania w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="b355b-123">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="b355b-124">Wartość ciągu zapytania jest dostarczana przez ASP.NET MVC jako parametr do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="b355b-124">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="b355b-125">Parametr jest ciągiem "name" lub "date", opcjonalnie poprzedzonym znakiem podkreślenia i ciągiem "DESC" w celu określenia kolejności malejącej.</span><span class="sxs-lookup"><span data-stu-id="b355b-125">The parameter is a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="b355b-126">Domyślna kolejność sortowania to Ascending.</span><span class="sxs-lookup"><span data-stu-id="b355b-126">The default sort order is ascending.</span></span>

<span data-ttu-id="b355b-127">Podczas pierwszego żądania strony indeksu nie ma ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="b355b-127">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="b355b-128">Studenci są wyświetlani w porządku rosnącym według `LastName`, który jest wartością domyślną ustaloną przez przypadek przypadania `switch` w instrukcji.</span><span class="sxs-lookup"><span data-stu-id="b355b-128">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="b355b-129">Gdy użytkownik kliknie hiperlink nagłówka kolumny, odpowiednia `sortOrder` wartość jest podawana w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="b355b-129">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="b355b-130">Te dwie `ViewBag` zmienne są używane, aby widok mógł skonfigurować hiperłącza nagłówków kolumn z odpowiednimi wartościami ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="b355b-130">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="b355b-131">Są to "Trzyelementowy".</span><span class="sxs-lookup"><span data-stu-id="b355b-131">These are ternary statements.</span></span> <span data-ttu-id="b355b-132">Pierwszy z nich określa, że jeśli `sortOrder` parametr ma wartość null lub jest `ViewBag.NameSortParm` pusty, należy ustawić wartość "\_nazwa DESC"; w przeciwnym razie należy ustawić pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="b355b-132">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="b355b-133">Te dwie instrukcje umożliwiają widokowi ustawienie hiperłączy nagłówka kolumny w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b355b-133">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="b355b-134">Bieżący porządek sortowania</span><span class="sxs-lookup"><span data-stu-id="b355b-134">Current sort order</span></span> | <span data-ttu-id="b355b-135">Hiperłącze nazwisko</span><span class="sxs-lookup"><span data-stu-id="b355b-135">Last Name Hyperlink</span></span> | <span data-ttu-id="b355b-136">Hiperłącze daty</span><span class="sxs-lookup"><span data-stu-id="b355b-136">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b355b-137">Nazwisko — rosnąco</span><span class="sxs-lookup"><span data-stu-id="b355b-137">Last Name ascending</span></span> | <span data-ttu-id="b355b-138">descending</span><span class="sxs-lookup"><span data-stu-id="b355b-138">descending</span></span> | <span data-ttu-id="b355b-139">ascending</span><span class="sxs-lookup"><span data-stu-id="b355b-139">ascending</span></span> |
| <span data-ttu-id="b355b-140">Nazwisko malejąco</span><span class="sxs-lookup"><span data-stu-id="b355b-140">Last Name descending</span></span> | <span data-ttu-id="b355b-141">ascending</span><span class="sxs-lookup"><span data-stu-id="b355b-141">ascending</span></span> | <span data-ttu-id="b355b-142">ascending</span><span class="sxs-lookup"><span data-stu-id="b355b-142">ascending</span></span> |
| <span data-ttu-id="b355b-143">Data rosnąca</span><span class="sxs-lookup"><span data-stu-id="b355b-143">Date ascending</span></span> | <span data-ttu-id="b355b-144">ascending</span><span class="sxs-lookup"><span data-stu-id="b355b-144">ascending</span></span> | <span data-ttu-id="b355b-145">descending</span><span class="sxs-lookup"><span data-stu-id="b355b-145">descending</span></span> |
| <span data-ttu-id="b355b-146">Data malejąca</span><span class="sxs-lookup"><span data-stu-id="b355b-146">Date descending</span></span> | <span data-ttu-id="b355b-147">ascending</span><span class="sxs-lookup"><span data-stu-id="b355b-147">ascending</span></span> | <span data-ttu-id="b355b-148">ascending</span><span class="sxs-lookup"><span data-stu-id="b355b-148">ascending</span></span> |

<span data-ttu-id="b355b-149">Metoda używa [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) , aby określić kolumnę, według której ma zostać wykonane sortowanie.</span><span class="sxs-lookup"><span data-stu-id="b355b-149">The method uses [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) to specify the column to sort by.</span></span> <span data-ttu-id="b355b-150"><xref:System.Linq.IQueryable%601> Kod tworzy `switch` zmienną `switch` przed instrukcją `switch` , modyfikuje ją w instrukcji i wywołuje `ToList` metodę po instrukcji.</span><span class="sxs-lookup"><span data-stu-id="b355b-150">The code creates an <xref:System.Linq.IQueryable%601> variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="b355b-151">Podczas tworzenia i modyfikowania `IQueryable` zmiennych nie są wysyłane żadne zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b355b-151">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="b355b-152">Zapytanie nie jest wykonywane do momentu przekonwertowania `IQueryable` obiektu do kolekcji przez wywołanie metody, takiej jak. `ToList`</span><span class="sxs-lookup"><span data-stu-id="b355b-152">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="b355b-153">W związku z tym ten kod skutkuje pojedynczym zapytaniem, które nie `return View` jest wykonywane do momentu wykonania instrukcji.</span><span class="sxs-lookup"><span data-stu-id="b355b-153">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="b355b-154">Alternatywą dla pisania różnych instrukcji LINQ dla każdej kolejności sortowania jest możliwość dynamicznego tworzenia instrukcji LINQ.</span><span class="sxs-lookup"><span data-stu-id="b355b-154">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="b355b-155">Aby uzyskać informacje na temat dynamicznego LINQ, zobacz [dynamiczny LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span><span class="sxs-lookup"><span data-stu-id="b355b-155">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="b355b-156">Dodawanie hiperłączy nagłówka kolumny do widoku indeksu ucznia</span><span class="sxs-lookup"><span data-stu-id="b355b-156">Add column heading hyperlinks to the Student index view</span></span>

1. <span data-ttu-id="b355b-157">W *Views\Student\Index.cshtml*Zastąp `<tr>` elementy i `<th>` dla wiersza nagłówka wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="b355b-157">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   <span data-ttu-id="b355b-158">Ten kod używa informacji we `ViewBag` właściwościach do konfigurowania hiperłączy z odpowiednimi wartościami ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="b355b-158">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

2. <span data-ttu-id="b355b-159">Uruchom stronę i kliknij nagłówki kolumny **nazwisko** i **Data rejestracji** , aby sprawdzić, czy sortowanie działa.</span><span class="sxs-lookup"><span data-stu-id="b355b-159">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

   <span data-ttu-id="b355b-160">Po kliknięciu nagłówka nazwisko uczniowie będą wyświetlane w kolejności malejących nazwisk.</span><span class="sxs-lookup"><span data-stu-id="b355b-160">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

## <a name="add-a-search-box"></a><span data-ttu-id="b355b-161">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="b355b-161">Add a Search box</span></span>

<span data-ttu-id="b355b-162">Aby dodać filtrowanie do strony indeksu uczniów, należy dodać pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metodzie.</span><span class="sxs-lookup"><span data-stu-id="b355b-162">To add filtering to the Students index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="b355b-163">Pole tekstowe umożliwia wprowadzenie ciągu do wyszukania w polach Imię i nazwisko.</span><span class="sxs-lookup"><span data-stu-id="b355b-163">The text box lets you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="b355b-164">Dodawanie funkcji filtrowania do metody index</span><span class="sxs-lookup"><span data-stu-id="b355b-164">Add filtering functionality to the Index method</span></span>

- <span data-ttu-id="b355b-165">W *Controllers\StudentController.cs*Zastąp `Index` metodę następującym kodem (zmiany są wyróżnione):</span><span class="sxs-lookup"><span data-stu-id="b355b-165">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="b355b-166">Kod dodaje `searchString` parametr `Index` do metody.</span><span class="sxs-lookup"><span data-stu-id="b355b-166">The code adds a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="b355b-167">Wartość ciągu wyszukiwania jest odbierana z pola tekstowego, które zostanie dodane do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="b355b-167">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="b355b-168">Dodaje `where` również klauzulę do instrukcji LINQ, która wybiera tylko uczniów, których imię i nazwisko zawiera ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="b355b-168">It also adds a `where` clause to the LINQ statement that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="b355b-169">Instrukcja, która dodaje <xref:System.Linq.Queryable.Where%2A> klauzulę, jest wykonywana tylko wtedy, gdy istnieje wartość do wyszukania.</span><span class="sxs-lookup"><span data-stu-id="b355b-169">The statement that adds the <xref:System.Linq.Queryable.Where%2A> clause executes only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="b355b-170">W wielu przypadkach można wywołać tę samą metodę w ramach zestawu jednostek Entity Framework lub jako metodę rozszerzenia w kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b355b-170">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="b355b-171">Wyniki są zwykle takie same, ale w niektórych przypadkach mogą być różne.</span><span class="sxs-lookup"><span data-stu-id="b355b-171">The results are normally the same but in some cases may be different.</span></span>
>
> <span data-ttu-id="b355b-172">Na przykład .NET Framework implementacja `Contains` metody zwraca wszystkie wiersze w przypadku przekazania do niego pustego ciągu, ale dostawca Entity Framework dla SQL Server Compact 4,0 zwraca zero wierszy dla pustych ciągów.</span><span class="sxs-lookup"><span data-stu-id="b355b-172">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="b355b-173">W związku z tym kod w przykładzie (umieszczenie `Where` instrukcji `if` wewnątrz instrukcji) gwarantuje, że te same wyniki są uzyskiwane dla wszystkich wersji SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b355b-173">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="b355b-174">Ponadto .NET Framework implementacja `Contains` metody domyślnie wykonuje porównanie z uwzględnieniem wielkości liter, ale Entity Framework SQL Server dostawcy domyślnie wykonuje porównania bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="b355b-174">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="b355b-175">W związku z tym `ToUpper` , wywołanie metody w celu przeprowadzenia testu jawnie bez uwzględniania wielkości liter gwarantuje, że wyniki nie zmienią się później, aby użyć repozytorium, które `IEnumerable` zwróci kolekcję zamiast `IQueryable` obiektu.</span><span class="sxs-lookup"><span data-stu-id="b355b-175">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="b355b-176">(Po wywołaniu `Contains` metody `IEnumerable` w kolekcji jest pobierana .NET Framework implementacja. po wywołaniu dla `IQueryable` obiektu zostanie wykorzystana implementacja dostawcy bazy danych).</span><span class="sxs-lookup"><span data-stu-id="b355b-176">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
>
> <span data-ttu-id="b355b-177">Obsługa wartości null może być również różna dla różnych dostawców baz danych lub jeśli `IQueryable` używasz obiektu w porównaniu z, gdy korzystasz z `IEnumerable` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="b355b-177">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="b355b-178">Na przykład w niektórych scenariuszach `Where` warunek taki jak `table.Column != 0` może nie zwracać kolumn, które mają `null` jako wartość.</span><span class="sxs-lookup"><span data-stu-id="b355b-178">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="b355b-179">Domyślnie EF generuje dodatkowe operatory SQL, aby mieć równość między wartościami null, które działają w bazie danych, tak jak to działa w pamięci, ale można ustawić flagę [UseDatabaseNullSemantics](https://docs.microsoft.com/dotnet/api/system.data.entity.infrastructure.dbcontextconfiguration.usedatabasenullsemantics) w Ef6 lub wywołać metodę [UseRelationalNulls](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.infrastructure.relationaldbcontextoptionsbuilder-2.userelationalnulls) w EF Core do Skonfiguruj to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="b355b-179">By default, EF generates additional SQL operators to make equality between null values work in the database like it works in memory, but you can set the [UseDatabaseNullSemantics](https://docs.microsoft.com/dotnet/api/system.data.entity.infrastructure.dbcontextconfiguration.usedatabasenullsemantics) flag in EF6 or call the [UseRelationalNulls](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.infrastructure.relationaldbcontextoptionsbuilder-2.userelationalnulls) method in EF Core to configure this behavior.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="b355b-180">Dodawanie pola wyszukiwania do widoku indeksu ucznia</span><span class="sxs-lookup"><span data-stu-id="b355b-180">Add a search box to the Student index view</span></span>

1. <span data-ttu-id="b355b-181">W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed tagiem otwierającym `table` , aby utworzyć podpis, pole tekstowe i przycisk **wyszukiwania** .</span><span class="sxs-lookup"><span data-stu-id="b355b-181">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. <span data-ttu-id="b355b-182">Uruchom stronę, wprowadź ciąg wyszukiwania, a następnie kliknij przycisk **Wyszukaj** , aby sprawdzić, czy filtrowanie działa.</span><span class="sxs-lookup"><span data-stu-id="b355b-182">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

   <span data-ttu-id="b355b-183">Zwróć uwagę, że adres URL nie zawiera ciągu wyszukiwania "a", co oznacza, że jeśli utworzysz zakładkę na tej stronie, nie otrzymasz filtrowanej listy przy użyciu zakładki.</span><span class="sxs-lookup"><span data-stu-id="b355b-183">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="b355b-184">Dotyczy to również linków do sortowania kolumn, ponieważ sortuje całą listę.</span><span class="sxs-lookup"><span data-stu-id="b355b-184">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="b355b-185">Zmienisz przycisk **wyszukiwania** , aby używać ciągów zapytania dla kryteriów filtrowania w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="b355b-185">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging"></a><span data-ttu-id="b355b-186">Dodaj stronicowanie</span><span class="sxs-lookup"><span data-stu-id="b355b-186">Add paging</span></span>

<span data-ttu-id="b355b-187">Aby dodać stronicowanie do strony indeksu uczniów, Zacznij od zainstalowania pakietu NuGet **PagedList. MVC** .</span><span class="sxs-lookup"><span data-stu-id="b355b-187">To add paging to the Students index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="b355b-188">Następnie wprowadzisz dodatkowe zmiany w `Index` metodzie i dodasz linki stronicowania `Index` do widoku.</span><span class="sxs-lookup"><span data-stu-id="b355b-188">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="b355b-189">**PagedList. MVC** to jeden z wielu dobrych pakietów stronicowania i sortowania dla ASP.NET MVC, a jego użycie w tym miejscu jest przeznaczone tylko jako przykład, a nie jako zalecenia dotyczące innych opcji.</span><span class="sxs-lookup"><span data-stu-id="b355b-189">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span>

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="b355b-190">Zainstaluj pakiet NuGet PagedList. MVC</span><span class="sxs-lookup"><span data-stu-id="b355b-190">Install the PagedList.MVC NuGet package</span></span>

<span data-ttu-id="b355b-191">Pakiet NuGet **PagedList. MVC** automatycznie instaluje pakiet **PagedList** jako zależność.</span><span class="sxs-lookup"><span data-stu-id="b355b-191">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="b355b-192">Pakiet **PagedList** instaluje `PagedList` typ kolekcji i metody rozszerzające dla `IQueryable` i `IEnumerable` kolekcje.</span><span class="sxs-lookup"><span data-stu-id="b355b-192">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="b355b-193">Metody rozszerzające umożliwiają utworzenie pojedynczej strony danych `PagedList` w kolekcji `IQueryable` z lub `IEnumerable`, a `PagedList` kolekcja zawiera kilka właściwości i metod, które ułatwiają stronicowanie.</span><span class="sxs-lookup"><span data-stu-id="b355b-193">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="b355b-194">Pakiet **PagedList. MVC** instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.</span><span class="sxs-lookup"><span data-stu-id="b355b-194">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

1. <span data-ttu-id="b355b-195">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet** , a następnie **konsolę Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="b355b-195">From the **Tools** menu, select **NuGet Package Manager** and then **Package Manager Console**.</span></span>

2. <span data-ttu-id="b355b-196">W oknie **konsola Menedżera pakietów** upewnij się, że **źródło pakietu** to **NuGet.org** , a **Projekt domyślny** to **ContosoUniversity**, a następnie wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b355b-196">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

   ```text
   Install-Package PagedList.Mvc
   ```

3. <span data-ttu-id="b355b-197">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="b355b-197">Build the project.</span></span>

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="b355b-198">Dodawanie funkcji stronicowania do metody index</span><span class="sxs-lookup"><span data-stu-id="b355b-198">Add paging functionality to the Index method</span></span>

1. <span data-ttu-id="b355b-199">W *Controllers\StudentController.cs*Dodaj `using` instrukcję dla `PagedList` przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="b355b-199">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. <span data-ttu-id="b355b-200">Zastąp `Index` metodę następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b355b-200">Replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   <span data-ttu-id="b355b-201">Ten kod dodaje `page` parametr, bieżący parametr kolejności sortowania i bieżący parametr filtru do sygnatury metody:</span><span class="sxs-lookup"><span data-stu-id="b355b-201">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   <span data-ttu-id="b355b-202">Gdy strona jest wyświetlana po raz pierwszy, lub jeśli użytkownik nie kliknął linku stronicowania ani sortowania, wszystkie parametry mają wartość null.</span><span class="sxs-lookup"><span data-stu-id="b355b-202">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters are null.</span></span> <span data-ttu-id="b355b-203">Jeśli kliknięto łącze stronicowania, `page` zmienna zawiera numer strony do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="b355b-203">If a paging link is clicked, the `page` variable contains the page number to display.</span></span>

   <span data-ttu-id="b355b-204">`ViewBag` Właściwość zapewnia widok z bieżącą kolejnością sortowania, ponieważ ta wartość musi być uwzględniona w łączach stronicowania, aby kolejność sortowania była taka sama podczas stronicowania:</span><span class="sxs-lookup"><span data-stu-id="b355b-204">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   <span data-ttu-id="b355b-205">Inna Właściwość `ViewBag.CurrentFilter`,, zapewnia widok z bieżącym ciągiem filtru.</span><span class="sxs-lookup"><span data-stu-id="b355b-205">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="b355b-206">Ta wartość musi być uwzględniona w łączach stronicowania, aby zachować ustawienia filtru podczas stronicowania, i musi zostać przywrócone do pola tekstowego, gdy strona jest ponownie wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="b355b-206">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="b355b-207">Jeśli ciąg wyszukiwania zostanie zmieniony podczas stronicowania, należy zresetować stronę do 1, ponieważ nowy filtr może spowodować wyświetlenie różnych danych.</span><span class="sxs-lookup"><span data-stu-id="b355b-207">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="b355b-208">Ciąg wyszukiwania jest zmieniany, gdy wartość zostanie wprowadzona w polu tekstowym, a przycisk Prześlij zostanie naciśnięty.</span><span class="sxs-lookup"><span data-stu-id="b355b-208">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="b355b-209">W takim przypadku `searchString` parametr nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="b355b-209">In that case, the `searchString` parameter is not null.</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   <span data-ttu-id="b355b-210">Na końcu metody `ToPagedList` rozszerzenia Metoda w obiekcie Students `IQueryable` konwertuje zapytanie ucznia na jedną stronę uczniów w typie kolekcji, który obsługuje stronicowanie.</span><span class="sxs-lookup"><span data-stu-id="b355b-210">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="b355b-211">Ta pojedyncza strona studentów jest następnie przenoszona do widoku:</span><span class="sxs-lookup"><span data-stu-id="b355b-211">That single page of students is then passed to the view:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   <span data-ttu-id="b355b-212">`ToPagedList` Metoda przyjmuje numer strony.</span><span class="sxs-lookup"><span data-stu-id="b355b-212">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="b355b-213">Dwa znaki zapytania reprezentują [operator łączenia wartości null](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span><span class="sxs-lookup"><span data-stu-id="b355b-213">The two question marks represent the [null-coalescing operator](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span></span> <span data-ttu-id="b355b-214">Operator łączenia wartości null definiuje wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(page ?? 1)` oznacza zwrócenie `page` wartości, jeśli ma wartość, lub zwraca wartość 1, jeśli `page` jest równa null.</span><span class="sxs-lookup"><span data-stu-id="b355b-214">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="b355b-215">Dodawanie linków stronicowania do widoku indeksu ucznia</span><span class="sxs-lookup"><span data-stu-id="b355b-215">Add paging links to the Student index view</span></span>

1. <span data-ttu-id="b355b-216">W *Views\Student\Index.cshtml*Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="b355b-216">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="b355b-217">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="b355b-217">The changes are highlighted.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   <span data-ttu-id="b355b-218">Instrukcja w górnej części strony określa, że widok `PagedList` pobiera teraz obiekt zamiast `List` obiektu. `@model`</span><span class="sxs-lookup"><span data-stu-id="b355b-218">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

   <span data-ttu-id="b355b-219">`using` Instrukcja dla`PagedList.Mvc` daje dostęp do pomocnika MVC dla przycisków stronicowania.</span><span class="sxs-lookup"><span data-stu-id="b355b-219">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

   <span data-ttu-id="b355b-220">Kod używa przeciążenia elementu [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) , który umożliwia określenie [FormMethod. Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span><span class="sxs-lookup"><span data-stu-id="b355b-220">The code uses an overload of [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) that allows it to specify [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   <span data-ttu-id="b355b-221">Wartość domyślna [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) przesyła dane formularza z wpisem, co oznacza, że parametry są przesyłane w treści wiadomości HTTP, a nie w adresie URL jako ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="b355b-221">The default [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="b355b-222">Po określeniu protokołu HTTP GET dane formularza są przesyłane w adresie URL jako ciągi zapytań, co umożliwia użytkownikom tworzenie zakładek w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="b355b-222">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="b355b-223">[Wskazówki W3C dotyczące korzystania z protokołu HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) zalecają korzystanie z usługi Get, gdy akcja nie spowoduje aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="b355b-223">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

   <span data-ttu-id="b355b-224">Pole tekstowe jest inicjowane z bieżącym ciągiem wyszukiwania, więc po kliknięciu nowej strony można zobaczyć bieżący ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="b355b-224">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   <span data-ttu-id="b355b-225">Linki nagłówka kolumny używają ciągu zapytania, aby przekazać bieżący ciąg wyszukiwania do kontrolera, tak aby użytkownik mógł sortować wyniki filtrów:</span><span class="sxs-lookup"><span data-stu-id="b355b-225">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   <span data-ttu-id="b355b-226">Zostanie wyświetlona bieżąca strona i całkowita liczba stron.</span><span class="sxs-lookup"><span data-stu-id="b355b-226">The current page and total number of pages are displayed.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   <span data-ttu-id="b355b-227">Jeśli nie ma żadnych stron do wyświetlenia, wyświetlana jest wartość "Strona 0 z 0".</span><span class="sxs-lookup"><span data-stu-id="b355b-227">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="b355b-228">(W tym przypadku numer strony jest większy niż liczba stron, ponieważ `Model.PageNumber` jest równa 1 i `Model.PageCount` wynosi 0).</span><span class="sxs-lookup"><span data-stu-id="b355b-228">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

   <span data-ttu-id="b355b-229">Przyciski stronicowania są wyświetlane przez `PagedListPager` pomocnika:</span><span class="sxs-lookup"><span data-stu-id="b355b-229">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   <span data-ttu-id="b355b-230">`PagedListPager` Pomocnik zawiera kilka opcji, które można dostosować, w tym adresy URL i style.</span><span class="sxs-lookup"><span data-stu-id="b355b-230">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="b355b-231">Aby uzyskać więcej informacji, zobacz [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="b355b-231">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

2. <span data-ttu-id="b355b-232">Uruchom stronę.</span><span class="sxs-lookup"><span data-stu-id="b355b-232">Run the page.</span></span>

   <span data-ttu-id="b355b-233">Kliknij linki stronicowania w różnych kolejności sortowania, aby upewnić się, że stronicowanie działa.</span><span class="sxs-lookup"><span data-stu-id="b355b-233">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="b355b-234">Następnie wprowadź ciąg wyszukiwania i spróbuj ponownie wykonać stronicowanie, aby sprawdzić, czy stronicowanie działa prawidłowo z sortowaniem i filtrowaniem.</span><span class="sxs-lookup"><span data-stu-id="b355b-234">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page"></a><span data-ttu-id="b355b-235">Tworzenie strony informacje</span><span class="sxs-lookup"><span data-stu-id="b355b-235">Create an About page</span></span>

<span data-ttu-id="b355b-236">Na stronie informacje o witrynie sieci Web dla uniwersytetów firmy Contoso można wyświetlić liczbę studentów zarejestrowanych dla każdej daty rejestracji.</span><span class="sxs-lookup"><span data-stu-id="b355b-236">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="b355b-237">Wymaga to grupowania i prostych obliczeń dla grup.</span><span class="sxs-lookup"><span data-stu-id="b355b-237">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="b355b-238">Aby to osiągnąć, należy wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="b355b-238">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="b355b-239">Utwórz klasę modelu widoku dla danych, które należy przekazać do widoku.</span><span class="sxs-lookup"><span data-stu-id="b355b-239">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="b355b-240">`About` Zmodyfikuj metodę`Home` w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="b355b-240">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="b355b-241">`About` Zmodyfikuj widok.</span><span class="sxs-lookup"><span data-stu-id="b355b-241">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="b355b-242">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="b355b-242">Create the View Model</span></span>

<span data-ttu-id="b355b-243">Utwórz folder *modele widoków* w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="b355b-243">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="b355b-244">W tym folderze Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b355b-244">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="b355b-245">Modyfikowanie kontrolera macierzystego</span><span class="sxs-lookup"><span data-stu-id="b355b-245">Modify the Home Controller</span></span>

1. <span data-ttu-id="b355b-246">W *HomeController.cs*Dodaj następujące `using` instrukcje w górnej części pliku:</span><span class="sxs-lookup"><span data-stu-id="b355b-246">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. <span data-ttu-id="b355b-247">Dodaj zmienną klasy dla kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy:</span><span class="sxs-lookup"><span data-stu-id="b355b-247">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. <span data-ttu-id="b355b-248">Zastąp `About` metodę następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b355b-248">Replace the `About` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   <span data-ttu-id="b355b-249">Instrukcja LINQ grupuje jednostki studenta według daty rejestracji, oblicza liczbę jednostek w każdej grupie i zapisuje wyniki w kolekcji `EnrollmentDateGroup` obiektów modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="b355b-249">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

4. <span data-ttu-id="b355b-250">`Dispose` Dodaj metodę:</span><span class="sxs-lookup"><span data-stu-id="b355b-250">Add a `Dispose` method:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="b355b-251">Modyfikowanie widoku informacje</span><span class="sxs-lookup"><span data-stu-id="b355b-251">Modify the About View</span></span>

1. <span data-ttu-id="b355b-252">Zastąp kod w pliku *Views\Home\About.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b355b-252">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. <span data-ttu-id="b355b-253">Uruchom aplikację i kliknij łącze **informacje** .</span><span class="sxs-lookup"><span data-stu-id="b355b-253">Run the app and click the **About** link.</span></span>

   <span data-ttu-id="b355b-254">Liczba studentów dla każdej daty rejestracji zostanie wyświetlona w tabeli.</span><span class="sxs-lookup"><span data-stu-id="b355b-254">The count of students for each enrollment date displays in a table.</span></span>

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a><span data-ttu-id="b355b-256">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="b355b-256">Get the code</span></span>

[<span data-ttu-id="b355b-257">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="b355b-257">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="b355b-258">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b355b-258">Additional resources</span></span>

<span data-ttu-id="b355b-259">Linki do innych zasobów Entity Framework można znaleźć w temacie [ASP.NET Data Access — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="b355b-259">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b355b-260">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b355b-260">Next steps</span></span>

<span data-ttu-id="b355b-261">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="b355b-261">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b355b-262">Dodaj linki sortowania kolumn</span><span class="sxs-lookup"><span data-stu-id="b355b-262">Add column sort links</span></span>
> * <span data-ttu-id="b355b-263">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="b355b-263">Add a Search box</span></span>
> * <span data-ttu-id="b355b-264">Dodaj stronicowanie</span><span class="sxs-lookup"><span data-stu-id="b355b-264">Add paging</span></span>
> * <span data-ttu-id="b355b-265">Tworzenie strony informacje</span><span class="sxs-lookup"><span data-stu-id="b355b-265">Create an About page</span></span>

<span data-ttu-id="b355b-266">Przejdź do następnego artykułu, aby dowiedzieć się, jak korzystać z odporności połączeń i przechwycenia poleceń.</span><span class="sxs-lookup"><span data-stu-id="b355b-266">Advance to the next article to learn how to use connection resiliency and command interception.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b355b-267">Odporność połączeń i przechwycenie poleceń</span><span class="sxs-lookup"><span data-stu-id="b355b-267">Connection resiliency and command interception</span></span>](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
