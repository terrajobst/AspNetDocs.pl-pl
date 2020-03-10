---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Korzystając z Entity Framework 4,0 i formantu ObjectDataSource, część 3: sortowanie i filtrowanie | Microsoft Docs'
author: tdykstra
description: Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631671"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="1463c-104">Używanie Entity Framework 4,0 i formantu ObjectDataSource, część 3: sortowanie i filtrowanie</span><span class="sxs-lookup"><span data-stu-id="1463c-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>

<span data-ttu-id="1463c-105">Autor [Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1463c-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="1463c-106">Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków [Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) .</span><span class="sxs-lookup"><span data-stu-id="1463c-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="1463c-107">Jeśli wcześniej samouczki nie zostały wykonane, jako punkt wyjścia dla tego samouczka możesz [pobrać utworzoną aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) .</span><span class="sxs-lookup"><span data-stu-id="1463c-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="1463c-108">Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) utworzoną przez kompletną serię samouczków.</span><span class="sxs-lookup"><span data-stu-id="1463c-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="1463c-109">Jeśli masz pytania dotyczące samouczków, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="1463c-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>

<span data-ttu-id="1463c-110">W poprzednim samouczku zaimplementowano wzorzec repozytorium w wielowarstwowej aplikacji sieci Web, która używa Entity Framework i kontrolki `ObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="1463c-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="1463c-111">W tym samouczku pokazano, jak wykonywać sortowanie i filtrowanie oraz obsłużyć scenariusze wzorca-szczegóły.</span><span class="sxs-lookup"><span data-stu-id="1463c-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="1463c-112">Na stronie *działS. aspx* zostaną dodane następujące ulepszenia:</span><span class="sxs-lookup"><span data-stu-id="1463c-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="1463c-113">Pole tekstowe umożliwiające użytkownikom wybieranie działów według nazwy.</span><span class="sxs-lookup"><span data-stu-id="1463c-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="1463c-114">Lista kursów dla każdego działu, który jest wyświetlany w siatce.</span><span class="sxs-lookup"><span data-stu-id="1463c-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="1463c-115">Możliwość sortowania przez kliknięcie nagłówków kolumn.</span><span class="sxs-lookup"><span data-stu-id="1463c-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="1463c-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1463c-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="1463c-117">Dodawanie możliwości sortowania kolumn GridView</span><span class="sxs-lookup"><span data-stu-id="1463c-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="1463c-118">Otwórz stronę *działS. aspx* i dodaj atrybut `SortParameterName="sortExpression"` do kontrolki `ObjectDataSource` o nazwie `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="1463c-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="1463c-119">(W dalszej części utworzysz metodę `GetDepartments`, która przyjmuje parametr o nazwie `sortExpression`.) Znaczniki dla otwierającego tagu kontrolki są teraz podobne do poniższego przykładu.</span><span class="sxs-lookup"><span data-stu-id="1463c-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="1463c-120">Dodaj atrybut `AllowSorting="true"` do tagu otwierającego kontrolki `GridView`.</span><span class="sxs-lookup"><span data-stu-id="1463c-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="1463c-121">Znaczniki dla otwierającego tagu kontrolki są teraz podobne do poniższego przykładu.</span><span class="sxs-lookup"><span data-stu-id="1463c-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="1463c-122">W *Departments.aspx.cs*Ustaw domyślną kolejność sortowania, wywołując metodę `Sort` formantu `GridView` z metody `Page_Load`:</span><span class="sxs-lookup"><span data-stu-id="1463c-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="1463c-123">Można dodać kod, który sortuje lub filtruje w klasie logiki biznesowej lub klasy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="1463c-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="1463c-124">Jeśli zostanie to zrobione w klasie logiki biznesowej, zadania sortowania lub filtrowania będą wykonywane po pobraniu danych z bazy danych, ponieważ Klasa logiki biznesowej pracuje z obiektem `IEnumerable` zwróconym przez repozytorium.</span><span class="sxs-lookup"><span data-stu-id="1463c-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="1463c-125">W przypadku dodania sortowania i filtrowania kodu w klasie repozytorium, gdy zostanie to wykonane przed przekonwertowaniem wyrażenia LINQ lub zapytania obiektu na obiekt `IEnumerable`, polecenia zostaną przesłane do bazy danych w celu przetworzenia, co jest zwykle bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="1463c-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="1463c-126">W tym samouczku zaimplementowano sortowanie i filtrowanie w taki sposób, który powoduje, że przetwarzanie zostanie wykonane przez bazę danych — czyli w repozytorium.</span><span class="sxs-lookup"><span data-stu-id="1463c-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="1463c-127">Aby dodać funkcję sortowania, należy dodać nową metodę do interfejsów repozytorium i klas repozytorium oraz do klasy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="1463c-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="1463c-128">W pliku *ISchoolRepository.cs* Dodaj nową metodę `GetDepartments`, która przyjmuje `sortExpression` parametr, który będzie używany do sortowania listy zwracanych wydziałów:</span><span class="sxs-lookup"><span data-stu-id="1463c-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="1463c-129">Parametr `sortExpression` określa kolumnę, według której ma zostać wykonane sortowanie, oraz kierunek sortowania.</span><span class="sxs-lookup"><span data-stu-id="1463c-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="1463c-130">Dodaj kod dla nowej metody do pliku *SchoolRepository.cs* :</span><span class="sxs-lookup"><span data-stu-id="1463c-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="1463c-131">Zmień istniejącą metodę `GetDepartments` bez parametrów, aby wywołać nową metodę:</span><span class="sxs-lookup"><span data-stu-id="1463c-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="1463c-132">W projekcie testowym Dodaj następującą nową metodę do *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="1463c-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="1463c-133">Jeśli chcesz utworzyć wszystkie testy jednostkowe, które od tej metody zwracają posortowaną listę, należy posortować listę przed jej zwróceniem.</span><span class="sxs-lookup"><span data-stu-id="1463c-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="1463c-134">Nie będziesz tworzyć testów takich jak w tym samouczku, dlatego metoda może po prostu zwrócić niesortowaną listę działów.</span><span class="sxs-lookup"><span data-stu-id="1463c-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="1463c-135">W pliku *SchoolBL.cs* Dodaj następującą nową metodę do klasy logiki biznesowej:</span><span class="sxs-lookup"><span data-stu-id="1463c-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="1463c-136">Ten kod przekazuje parametr sortowania do metody repozytorium.</span><span class="sxs-lookup"><span data-stu-id="1463c-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="1463c-137">Uruchom stronę *działS. aspx* .</span><span class="sxs-lookup"><span data-stu-id="1463c-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="1463c-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1463c-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="1463c-139">Teraz możesz kliknąć dowolny nagłówek kolumny, aby posortować według tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="1463c-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="1463c-140">Jeśli kolumna jest już posortowana, kliknięcie nagłówka powoduje odwrócenie kierunku sortowania.</span><span class="sxs-lookup"><span data-stu-id="1463c-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="1463c-141">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="1463c-141">Adding a Search Box</span></span>

<span data-ttu-id="1463c-142">W tej sekcji dodasz pole tekstowe wyszukiwania, połączysz je z kontrolką `ObjectDataSource` przy użyciu parametru kontrolki i dodamy metodę do klasy logiki biznesowej w celu obsługi filtrowania.</span><span class="sxs-lookup"><span data-stu-id="1463c-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="1463c-143">Otwórz stronę *działS. aspx* i Dodaj następujące znaczniki między nagłówkiem i pierwszą kontrolką `ObjectDataSource`:</span><span class="sxs-lookup"><span data-stu-id="1463c-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="1463c-144">W kontrolce `ObjectDataSource` o nazwie `DepartmentsObjectDataSource`wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1463c-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="1463c-145">Dodaj element `SelectParameters` dla parametru o nazwie `nameSearchString`, który pobiera wartość wprowadzoną w kontrolce `SearchTextBox`.</span><span class="sxs-lookup"><span data-stu-id="1463c-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="1463c-146">Zmień wartość atrybutu `SelectMethod` na `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="1463c-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="1463c-147">(Ta metoda zostanie utworzona później).</span><span class="sxs-lookup"><span data-stu-id="1463c-147">(You'll create this method later.)</span></span>

<span data-ttu-id="1463c-148">Znaczniki dla kontrolki `ObjectDataSource` są teraz podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="1463c-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="1463c-149">W *ISchoolRepository.cs*dodaj metodę `GetDepartmentsByName`, która przyjmuje zarówno parametry `sortExpression`, jak i `nameSearchString`:</span><span class="sxs-lookup"><span data-stu-id="1463c-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="1463c-150">W *SchoolRepository.cs*Dodaj następującą nową metodę:</span><span class="sxs-lookup"><span data-stu-id="1463c-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="1463c-151">Ten kod używa metody `Where`, aby wybrać elementy, które zawierają ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="1463c-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="1463c-152">Jeśli ciąg wyszukiwania jest pusty, zostaną zaznaczone wszystkie rekordy.</span><span class="sxs-lookup"><span data-stu-id="1463c-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="1463c-153">Należy pamiętać, że po określeniu wywołań metod w jednej instrukcji, takiej jak (`Include`, a następnie `OrderBy`, a następnie `Where`), Metoda `Where` musi być zawsze Ostatnia.</span><span class="sxs-lookup"><span data-stu-id="1463c-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="1463c-154">Zmień istniejącą metodę `GetDepartments`, która przyjmuje parametr `sortExpression`, aby wywołać nową metodę:</span><span class="sxs-lookup"><span data-stu-id="1463c-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="1463c-155">W *MockSchoolRepository.cs* w projekcie testowym Dodaj następującą nową metodę:</span><span class="sxs-lookup"><span data-stu-id="1463c-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="1463c-156">W *SchoolBL.cs*Dodaj następującą nową metodę:</span><span class="sxs-lookup"><span data-stu-id="1463c-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="1463c-157">Uruchom stronę *działu. aspx* , a następnie wprowadź ciąg wyszukiwania, aby upewnić się, że logika wyboru działa.</span><span class="sxs-lookup"><span data-stu-id="1463c-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="1463c-158">Pozostaw pole tekstowe puste i spróbuj wykonać wyszukiwanie, aby upewnić się, że wszystkie rekordy są zwracane.</span><span class="sxs-lookup"><span data-stu-id="1463c-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="1463c-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1463c-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="1463c-160">Dodawanie kolumny szczegółów dla każdego wiersza siatki</span><span class="sxs-lookup"><span data-stu-id="1463c-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="1463c-161">Następnie chcesz zobaczyć wszystkie kursy dla każdego działu wyświetlanego w prawej komórce siatki.</span><span class="sxs-lookup"><span data-stu-id="1463c-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="1463c-162">W tym celu należy użyć zagnieżdżonej kontrolki `GridView` i powiązać ją z danymi z `Courses` właściwości nawigacji jednostki `Department`.</span><span class="sxs-lookup"><span data-stu-id="1463c-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="1463c-163">Otwórz *działy. aspx* i w znaczniku dla kontrolki `GridView` Określ procedurę obsługi dla zdarzenia `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="1463c-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="1463c-164">Znaczniki dla otwierającego tagu kontrolki są teraz podobne do poniższego przykładu.</span><span class="sxs-lookup"><span data-stu-id="1463c-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="1463c-165">Dodaj nowy element `TemplateField` po polu szablonu `Administrator`:</span><span class="sxs-lookup"><span data-stu-id="1463c-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="1463c-166">Ten znacznik tworzy zagnieżdżony formant `GridView`, który pokazuje numer kursu i tytuł listy kursów.</span><span class="sxs-lookup"><span data-stu-id="1463c-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="1463c-167">Nie określa źródła danych, ponieważ będzie on wiązać się z kodem w obsłudze `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="1463c-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="1463c-168">Otwórz *Departments.aspx.cs* i Dodaj następujący program obsługi dla zdarzenia `RowDataBound`:</span><span class="sxs-lookup"><span data-stu-id="1463c-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="1463c-169">Ten kod pobiera `Department` jednostkę z argumentów zdarzenia, konwertuje właściwość nawigacji `Courses` do kolekcji `List`, a następnie datawiąże zagnieżdżoną `GridView` z kolekcją.</span><span class="sxs-lookup"><span data-stu-id="1463c-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="1463c-170">Otwórz plik *SchoolRepository.cs* i określ eager do załadowania dla właściwości nawigacji `Courses` przez wywołanie metody `Include` w zapytaniu obiektu utworzonym w metodzie `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="1463c-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="1463c-171">Instrukcja `return` w metodzie `GetDepartmentsByName` jest teraz podobna do poniższego przykładu.</span><span class="sxs-lookup"><span data-stu-id="1463c-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="1463c-172">Uruchom stronę.</span><span class="sxs-lookup"><span data-stu-id="1463c-172">Run the page.</span></span> <span data-ttu-id="1463c-173">Oprócz możliwości sortowania i filtrowania, które zostały dodane wcześniej, formant GridView pokazuje teraz zagnieżdżone szczegóły kursu dla każdego działu.</span><span class="sxs-lookup"><span data-stu-id="1463c-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="1463c-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1463c-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="1463c-175">Spowoduje to zakończenie wprowadzania do sortowania, filtrowania i szczegółowych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="1463c-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="1463c-176">W następnym samouczku zobaczysz, jak obsłużyć współbieżność.</span><span class="sxs-lookup"><span data-stu-id="1463c-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1463c-177">[Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="1463c-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
