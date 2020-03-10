---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Dodawanie warstwy logiki biznesowej do projektu używającego powiązań modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634835"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="958d1-104">Dodawanie warstwy logiki biznesowej do projektu używającego powiązań modelu i formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="958d1-104">Adding business logic layer to a project that uses model binding and web forms</span></span>

<span data-ttu-id="958d1-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="958d1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="958d1-106">Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="958d1-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="958d1-107">Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="958d1-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="958d1-108">Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="958d1-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="958d1-109">W tym samouczku pokazano, jak używać powiązania modelu z warstwą logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="958d1-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="958d1-110">Należy ustawić składową OnCallingDataMethods, aby określić, że obiekt inny niż bieżąca strona jest używany do wywoływania metod danych.</span><span class="sxs-lookup"><span data-stu-id="958d1-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="958d1-111">Ten samouczek jest oparty na projekcie utworzonym w [poprzednich](retrieving-data.md) częściach serii.</span><span class="sxs-lookup"><span data-stu-id="958d1-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="958d1-112">Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB.</span><span class="sxs-lookup"><span data-stu-id="958d1-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="958d1-113">Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="958d1-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="958d1-114">Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="958d1-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="958d1-115">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="958d1-115">What you'll build</span></span>

<span data-ttu-id="958d1-116">Powiązanie modelu umożliwia umieszczenie kodu interakcji z danymi w pliku związanym z kodem dla strony sieci Web lub w oddzielnej klasie logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="958d1-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="958d1-117">W poprzednich samouczkach pokazano, jak używać plików związanych z kodem dla kodu interakcji z danymi.</span><span class="sxs-lookup"><span data-stu-id="958d1-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="958d1-118">Ta metoda działa w przypadku małych lokacji, ale może prowadzić do powtarzania kodu i większego trudności podczas konserwacji dużej lokacji.</span><span class="sxs-lookup"><span data-stu-id="958d1-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="958d1-119">Może być również bardzo trudne do programistycznego testowania kodu, który znajduje się w pliku znajdującym się w kodzie, ponieważ nie ma warstwy abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="958d1-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="958d1-120">Aby scentralizować kod interakcji z danymi, można utworzyć warstwę logiki biznesowej, która zawiera całą logikę interakcji z danymi.</span><span class="sxs-lookup"><span data-stu-id="958d1-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="958d1-121">Następnie Wywołaj warstwę logiki biznesowej ze stron sieci Web.</span><span class="sxs-lookup"><span data-stu-id="958d1-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="958d1-122">W tym samouczku pokazano, jak przenieść cały kod zapisany w poprzednich samouczkach do warstwy logiki biznesowej, a następnie użyć tego kodu ze stron.</span><span class="sxs-lookup"><span data-stu-id="958d1-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="958d1-123">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="958d1-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="958d1-124">Przenoszenie kodu z plików związanych z kodem do warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="958d1-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="958d1-125">Zmień kontrolki powiązane z danymi, aby wywoływać metody z warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="958d1-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="958d1-126">Tworzenie warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="958d1-126">Create business logic layer</span></span>

<span data-ttu-id="958d1-127">Teraz utworzysz klasę, która jest wywoływana ze stron sieci Web.</span><span class="sxs-lookup"><span data-stu-id="958d1-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="958d1-128">Metody w tej klasie wyglądają podobnie jak metody używane w poprzednich samouczkach i zawierają atrybuty dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="958d1-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="958d1-129">Najpierw Dodaj nowy folder o nazwie **logiki biznesowej**.</span><span class="sxs-lookup"><span data-stu-id="958d1-129">First, add a new folder called **BLL**.</span></span>

![Dodaj folder](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="958d1-131">W folderze LOGIKI biznesowej Utwórz nową klasę o nazwie **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="958d1-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="958d1-132">Będzie zawierać wszystkie operacje na danych, które pierwotnie znajdowały się w plikach związanych z kodem.</span><span class="sxs-lookup"><span data-stu-id="958d1-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="958d1-133">Metody są prawie takie same jak metody w pliku związanym z kodem, ale zawierają pewne zmiany.</span><span class="sxs-lookup"><span data-stu-id="958d1-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="958d1-134">Najważniejszym zmianą w notatce jest to, że kod nie jest już wykonywany z poziomu instancji klasy **Page** .</span><span class="sxs-lookup"><span data-stu-id="958d1-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="958d1-135">Klasa Page zawiera metodę **TryUpdateModel** i Właściwość **ModelState** .</span><span class="sxs-lookup"><span data-stu-id="958d1-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="958d1-136">Gdy ten kod jest przenoszony do warstwy logiki biznesowej, nie ma już wystąpienia klasy Page do wywołania tych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="958d1-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="958d1-137">Aby obejść ten problem, należy dodać parametr **ModelMethodContext** do dowolnej metody, która uzyskuje dostęp do TryUpdateModel lub ModelState.</span><span class="sxs-lookup"><span data-stu-id="958d1-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="958d1-138">Ten parametr ModelMethodContext służy do wywoływania TryUpdateModel lub pobierania ModelState.</span><span class="sxs-lookup"><span data-stu-id="958d1-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="958d1-139">Nie trzeba zmieniać żadnych elementów na stronie sieci Web, aby uwzględnić ten nowy parametr.</span><span class="sxs-lookup"><span data-stu-id="958d1-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="958d1-140">Zastąp kod w SchoolBL.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="958d1-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="958d1-141">Popraw istniejące strony, aby pobrać dane z warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="958d1-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="958d1-142">Na koniec przekonwertujesz strony uczniowie. aspx, addstudents. aspx i kursy. aspx z używania zapytań w pliku związanym z kodem, aby użyć warstwy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="958d1-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="958d1-143">W plikach związanych z kodem dla studentów, addstudenta i kursów Usuń lub Skomentuj następujące metody zapytania:</span><span class="sxs-lookup"><span data-stu-id="958d1-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="958d1-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="958d1-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="958d1-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="958d1-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="958d1-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="958d1-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="958d1-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="958d1-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="958d1-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="958d1-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="958d1-149">W pliku związanym z kodem nie powinien już znajdować się kod, który odnosi się do operacji na danych.</span><span class="sxs-lookup"><span data-stu-id="958d1-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="958d1-150">Program obsługi zdarzeń **OnCallingDataMethods** umożliwia określenie obiektu, który ma być używany w metodach danych.</span><span class="sxs-lookup"><span data-stu-id="958d1-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="958d1-151">W uczniów. aspx Dodaj wartość dla tego programu obsługi zdarzeń i Zmień nazwy metod danych na nazwy metod w klasie logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="958d1-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="958d1-152">W pliku związanym z kodem dla uczniów. aspx Zdefiniuj program obsługi zdarzeń dla zdarzenia CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="958d1-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="958d1-153">W tym obsłudze zdarzeń należy określić klasę logiki biznesowej dla operacji na danych.</span><span class="sxs-lookup"><span data-stu-id="958d1-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="958d1-154">W polu addstudent. aspx wprowadź podobne zmiany.</span><span class="sxs-lookup"><span data-stu-id="958d1-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="958d1-155">W polu kursy. aspx Zmień podobne zmiany.</span><span class="sxs-lookup"><span data-stu-id="958d1-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="958d1-156">Uruchom aplikację i zwróć uwagę, że wszystkie strony działają tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="958d1-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="958d1-157">Logika walidacji działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="958d1-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="958d1-158">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="958d1-158">Conclusion</span></span>

<span data-ttu-id="958d1-159">W tym samouczku utworzysz swoją aplikację, aby użyć warstwy dostępu do danych i warstwy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="958d1-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="958d1-160">Określono, że kontrolki danych używają obiektu, który nie jest bieżącą stroną dla operacji na danych.</span><span class="sxs-lookup"><span data-stu-id="958d1-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="958d1-161">Wstecz</span><span class="sxs-lookup"><span data-stu-id="958d1-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
