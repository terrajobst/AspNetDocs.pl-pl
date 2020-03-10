---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Używanie wartości ciągu zapytania do filtrowania danych przy użyciu powiązania modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639098"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="1165b-104">Używanie wartości ciągu zapytania do filtrowania danych przy użyciu powiązania modelu i formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="1165b-104">Using query string values to filter data with model binding and web forms</span></span>

<span data-ttu-id="1165b-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1165b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1165b-106">Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1165b-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="1165b-107">Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="1165b-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="1165b-108">Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="1165b-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="1165b-109">W tym samouczku pokazano, jak przekazać wartość w ciągu zapytania i użyć tej wartości do pobierania danych za pomocą powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="1165b-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="1165b-110">Ten samouczek jest oparty na projekcie utworzonym w [poprzednich](retrieving-data.md) częściach serii.</span><span class="sxs-lookup"><span data-stu-id="1165b-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="1165b-111">Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB.</span><span class="sxs-lookup"><span data-stu-id="1165b-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="1165b-112">Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1165b-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="1165b-113">Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="1165b-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="1165b-114">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="1165b-114">What you'll build</span></span>

<span data-ttu-id="1165b-115">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="1165b-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="1165b-116">Dodaj nową stronę, aby wyświetlić zarejestrowane kursy dla ucznia</span><span class="sxs-lookup"><span data-stu-id="1165b-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="1165b-117">Pobierz zarejestrowane kursy dla wybranego ucznia na podstawie wartości w ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="1165b-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="1165b-118">Dodaj hiperlink z wartością ciągu zapytania z widoku siatki do nowej strony</span><span class="sxs-lookup"><span data-stu-id="1165b-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="1165b-119">Kroki opisane w tym samouczku są stosunkowo podobne do tego, co zostało zrobione w poprzednim [samouczku](sorting-paging-and-filtering-data.md) , aby odfiltrować wyświetlanych uczniów w oparciu o wybór użytkownika na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="1165b-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="1165b-120">W tym samouczku użyto atrybutu **Control** w metodzie SELECT, aby określić, że wartość parametru pochodzi z formantu.</span><span class="sxs-lookup"><span data-stu-id="1165b-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="1165b-121">W tym samouczku użyjesz atrybutu **QueryString** w metodzie SELECT, aby określić, że wartość parametru pochodzi z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="1165b-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="1165b-122">Dodaj nową stronę do wyświetlania kursów ucznia</span><span class="sxs-lookup"><span data-stu-id="1165b-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="1165b-123">Dodaj nowy formularz sieci Web, który używa strony głównej witryny. Master, a następnie nazwij **kursy**strony.</span><span class="sxs-lookup"><span data-stu-id="1165b-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="1165b-124">W pliku **kursy. aspx** Dodaj widok siatki, aby wyświetlić kursy dla wybranego ucznia.</span><span class="sxs-lookup"><span data-stu-id="1165b-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="1165b-125">Zdefiniuj metodę select</span><span class="sxs-lookup"><span data-stu-id="1165b-125">Define the select method</span></span>

<span data-ttu-id="1165b-126">W **courses.aspx.cs**zostanie dodana Metoda SELECT o nazwie określonej we właściwości **SelectMethod** widoku siatki.</span><span class="sxs-lookup"><span data-stu-id="1165b-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="1165b-127">W tej metodzie zdefiniujesz zapytanie do pobierania kursów studenta i określisz, że parametr pochodzi z wartości ciągu zapytania o tej samej nazwie co parametr.</span><span class="sxs-lookup"><span data-stu-id="1165b-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="1165b-128">Najpierw należy dodać następujące instrukcje **using** .</span><span class="sxs-lookup"><span data-stu-id="1165b-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="1165b-129">Następnie Dodaj następujący kod do Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="1165b-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="1165b-130">Atrybut QueryString oznacza, że wartość ciągu zapytania o nazwie StudentID jest automatycznie przypisywana do parametru w tej metodzie.</span><span class="sxs-lookup"><span data-stu-id="1165b-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="1165b-131">Dodaj hiperlink z wartością ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="1165b-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="1165b-132">W widoku siatki w programie Students. aspx dodasz pole Hyperlink, które łączy się ze swoją nową stroną kursów.</span><span class="sxs-lookup"><span data-stu-id="1165b-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="1165b-133">Hiperłącze będzie zawierać wartość ciągu zapytania z identyfikatorem studenta.</span><span class="sxs-lookup"><span data-stu-id="1165b-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="1165b-134">W polu Students. aspx Dodaj następujące pole do kolumn w widoku siatki tuż poniżej pola dla łącznej liczby kredytów.</span><span class="sxs-lookup"><span data-stu-id="1165b-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="1165b-135">Uruchom aplikację i zwróć uwagę, że widok siatki zawiera teraz link kursy.</span><span class="sxs-lookup"><span data-stu-id="1165b-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Dodaj hiperlink](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="1165b-137">Po kliknięciu jednego z tych linków zobaczysz zarejestrowane kursy tego ucznia.</span><span class="sxs-lookup"><span data-stu-id="1165b-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Pokaż kursy](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="1165b-139">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="1165b-139">Conclusion</span></span>

<span data-ttu-id="1165b-140">W tym samouczku Dodano łącze z wartością ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="1165b-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="1165b-141">Użyto tej wartości ciągu zapytania dla wartości parametru w metodzie SELECT.</span><span class="sxs-lookup"><span data-stu-id="1165b-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="1165b-142">W następnym [samouczku](adding-business-logic-layer.md)kod jest przenoszony z plików związanych z kodem do warstwy logiki biznesowej i warstwy dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="1165b-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1165b-143">[Poprzednie](integrating-jquery-ui.md)
> [dalej](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="1165b-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
