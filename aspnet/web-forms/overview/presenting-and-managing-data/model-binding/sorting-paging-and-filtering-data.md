---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortowanie, stronicowanie i filtrowanie danych za pomocą powiązania modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548063"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="40b01-104">Sortowanie, stronicowanie i filtrowanie danych za pomocą powiązania modelu i formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="40b01-104">Sorting, paging, and filtering data with model binding and web forms</span></span>

<span data-ttu-id="40b01-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="40b01-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="40b01-106">Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="40b01-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="40b01-107">Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="40b01-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="40b01-108">Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="40b01-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="40b01-109">W tym samouczku pokazano, jak dodać sortowanie, stronicowanie i filtrowanie danych za pomocą powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="40b01-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="40b01-110">Ten samouczek jest oparty na projekcie utworzonym w pierwszej [części](retrieving-data.md) serii.</span><span class="sxs-lookup"><span data-stu-id="40b01-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="40b01-111">Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB.</span><span class="sxs-lookup"><span data-stu-id="40b01-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="40b01-112">Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="40b01-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="40b01-113">Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="40b01-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="40b01-114">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="40b01-114">What you'll build</span></span>

<span data-ttu-id="40b01-115">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="40b01-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="40b01-116">Włącz sortowanie i stronicowanie danych</span><span class="sxs-lookup"><span data-stu-id="40b01-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="40b01-117">Włącz filtrowanie danych na podstawie wyboru przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="40b01-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="40b01-118">Dodaj sortowanie</span><span class="sxs-lookup"><span data-stu-id="40b01-118">Add sorting</span></span>

<span data-ttu-id="40b01-119">Włączenie sortowania w widoku GridView jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="40b01-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="40b01-120">W pliku student. aspx po prostu ustaw **AllowSorting** na **true** w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="40b01-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="40b01-121">Nie trzeba ustawiać wartości **'sortexpression** dla każdej kolumny, ponieważ pole DataField jest automatycznie używane.</span><span class="sxs-lookup"><span data-stu-id="40b01-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="40b01-122">Widok GridView modyfikuje zapytanie, aby uwzględnić porządkowanie danych według wybranej wartości.</span><span class="sxs-lookup"><span data-stu-id="40b01-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="40b01-123">Wyróżniony kod poniżej pokazuje dodanie, które należy wykonać, aby włączyć sortowanie.</span><span class="sxs-lookup"><span data-stu-id="40b01-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="40b01-124">Uruchom aplikację sieci Web i przetestuj sortowanie rekordów uczniów według wartości w różnych kolumnach.</span><span class="sxs-lookup"><span data-stu-id="40b01-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Sortuj uczniów](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="40b01-126">Dodaj stronicowanie</span><span class="sxs-lookup"><span data-stu-id="40b01-126">Add paging</span></span>

<span data-ttu-id="40b01-127">Włączenie stronicowania jest również bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="40b01-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="40b01-128">W widoku GridView ustaw właściwość **Właściwość AllowPaging** na **wartość true** i ustaw właściwość **PageSize** na liczbę rekordów, które mają być wyświetlane na każdej stronie.</span><span class="sxs-lookup"><span data-stu-id="40b01-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="40b01-129">W tym samouczku można ustawić wartość 4.</span><span class="sxs-lookup"><span data-stu-id="40b01-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="40b01-130">Uruchom aplikację sieci Web i zwróć uwagę, że teraz rekordy są podzielone na wiele stron, które nie mają więcej niż 4 rekordów wyświetlanych na jednej stronie.</span><span class="sxs-lookup"><span data-stu-id="40b01-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Dodaj stronicowanie](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="40b01-132">Opóźnione wykonywanie zapytań poprawia wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40b01-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="40b01-133">Zamiast pobierać cały zestaw danych, GridView modyfikuje zapytanie w celu pobrania tylko rekordów dla bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="40b01-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="40b01-134">Filtruj rekordy według wyboru użytkownika</span><span class="sxs-lookup"><span data-stu-id="40b01-134">Filter records by user selection</span></span>

<span data-ttu-id="40b01-135">Powiązanie modelu dodaje kilka atrybutów, które umożliwiają określenie sposobu ustawiania wartości parametru w metodzie powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="40b01-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="40b01-136">Te atrybuty znajdują się w przestrzeni nazw **System. Web. ModelBinding** .</span><span class="sxs-lookup"><span data-stu-id="40b01-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="40b01-137">Dostępne są następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="40b01-137">They include:</span></span>

- <span data-ttu-id="40b01-138">Kontrolka</span><span class="sxs-lookup"><span data-stu-id="40b01-138">Control</span></span>
- <span data-ttu-id="40b01-139">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="40b01-139">Cookie</span></span>
- <span data-ttu-id="40b01-140">Formularz</span><span class="sxs-lookup"><span data-stu-id="40b01-140">Form</span></span>
- <span data-ttu-id="40b01-141">Profil</span><span class="sxs-lookup"><span data-stu-id="40b01-141">Profile</span></span>
- <span data-ttu-id="40b01-142">Ciąg zapytania</span><span class="sxs-lookup"><span data-stu-id="40b01-142">QueryString</span></span>
- <span data-ttu-id="40b01-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="40b01-143">RouteData</span></span>
- <span data-ttu-id="40b01-144">Sesja</span><span class="sxs-lookup"><span data-stu-id="40b01-144">Session</span></span>
- <span data-ttu-id="40b01-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="40b01-145">UserProfile</span></span>
- <span data-ttu-id="40b01-146">Stan widoku</span><span class="sxs-lookup"><span data-stu-id="40b01-146">ViewState</span></span>

<span data-ttu-id="40b01-147">W tym samouczku zostanie użyta wartość kontrolki do filtrowania, które rekordy są wyświetlane w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="40b01-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="40b01-148">Dodasz atrybut **Control** do utworzonej wcześniej metody zapytania.</span><span class="sxs-lookup"><span data-stu-id="40b01-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="40b01-149">W [kolejnym](using-query-string-values-to-retrieve-data.md) samouczku zastosujesz atrybut **QueryString** do parametru, aby określić, że wartość parametru pochodzi z wartości ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="40b01-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="40b01-150">Najpierw, powyżej podsumowania walidacji, dodać listę rozwijaną umożliwiającą filtrowanie, które studenci są pokazywane.</span><span class="sxs-lookup"><span data-stu-id="40b01-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="40b01-151">W pliku związanym z kodem zmodyfikuj metodę select, aby otrzymać wartość z kontrolki, i Ustaw nazwę parametru na nazwę kontrolki, która zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="40b01-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="40b01-152">Należy dodać instrukcję **using** dla przestrzeni nazw **System. Web. ModelBinding** , aby rozpoznać atrybut Control.</span><span class="sxs-lookup"><span data-stu-id="40b01-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="40b01-153">Poniższy kod pokazuje, że metoda SELECT została przeprowadzona ponownie w celu filtrowania zwracanych danych na podstawie wartości listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="40b01-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="40b01-154">Dodanie atrybutu kontrolki przed parametrem określa, że wartość tego parametru pochodzi z kontrolki o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="40b01-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="40b01-155">Uruchom aplikację sieci Web i wybierz różne wartości z listy rozwijanej, aby przefiltrować listę studentów.</span><span class="sxs-lookup"><span data-stu-id="40b01-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Filtruj uczniów](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="40b01-157">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="40b01-157">Conclusion</span></span>

<span data-ttu-id="40b01-158">W tym samouczku włączono sortowanie i stronicowanie danych.</span><span class="sxs-lookup"><span data-stu-id="40b01-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="40b01-159">Można również włączyć filtrowanie danych przez wartość kontrolki.</span><span class="sxs-lookup"><span data-stu-id="40b01-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="40b01-160">W następnym [samouczku](integrating-jquery-ui.md) zostanie wzbogacenie interfejsu użytkownika przez integrację WIDŻETU interfejsu użytkownika jQuery z szablonem danych dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="40b01-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40b01-161">[Poprzednie](updating-deleting-and-creating-data.md)
> [dalej](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="40b01-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
