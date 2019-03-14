---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Dodawanie warstwy logiki biznesowej do projektu, który używa wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 4ba1830ce51f257e18880f752b249dbeb77f504e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067220"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="54b69-104">Dodawanie warstwy logiki biznesowej do projektu, który używa wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="54b69-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="54b69-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="54b69-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="54b69-106">W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="54b69-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="54b69-107">Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="54b69-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="54b69-108">Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="54b69-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="54b69-109">W tym samouczku przedstawiono sposób tworzenia powiązania modelu za pomocą warstwy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="54b69-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="54b69-110">Ustawi Członkowskie OnCallingDataMethods, aby określić, czy obiekt inny niż bieżąca strona jest używane do wywołania metody dotyczące danych.</span><span class="sxs-lookup"><span data-stu-id="54b69-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="54b69-111">Ten samouczek opiera się na projekt utworzony w [wcześniej](retrieving-data.md) części tej serii.</span><span class="sxs-lookup"><span data-stu-id="54b69-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="54b69-112">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="54b69-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="54b69-113">Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="54b69-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="54b69-114">Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="54b69-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="54b69-115">Będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="54b69-115">What you'll build</span></span>

<span data-ttu-id="54b69-116">Wiązanie modelu umożliwia umieść kod interakcji danych, w pliku związanym z kodem dla strony sieci web lub w klasie logiki oddzielne biznesowych.</span><span class="sxs-lookup"><span data-stu-id="54b69-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="54b69-117">Poprzednich samouczków wykazały, jak używać plików z kodem dla danych interakcję kodu.</span><span class="sxs-lookup"><span data-stu-id="54b69-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="54b69-118">Ta metoda działa w przypadku małych witryn, ale może to prowadzić do kodu powtórzeń i większa trudności podczas obsługi dużej witryny.</span><span class="sxs-lookup"><span data-stu-id="54b69-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="54b69-119">Może też być bardzo trudne, programowo testować kod, który znajduje się w kodzie plików, ponieważ nie istnieje żadne warstwę abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="54b69-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="54b69-120">Można scentralizować dane kod interakcji, można utworzyć warstwy logiki biznesowej, która zawiera całą logikę do interakcji z danymi.</span><span class="sxs-lookup"><span data-stu-id="54b69-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="54b69-121">Następnie możesz wywołać warstwę logiki biznesowej ze stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="54b69-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="54b69-122">W tym samouczku pokazano, jak przenieść cały kod, które zostały napisane w poprzednich samouczkach do warstwy logiki biznesowej, a następnie użyj tego kodu na stronach.</span><span class="sxs-lookup"><span data-stu-id="54b69-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="54b69-123">W ramach tego samouczka należy:</span><span class="sxs-lookup"><span data-stu-id="54b69-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="54b69-124">Przenieś kod z plików z kodem do warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="54b69-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="54b69-125">Zmień swoje formanty powiązane z danymi do wywołania metody warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="54b69-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="54b69-126">Tworzenie warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="54b69-126">Create business logic layer</span></span>

<span data-ttu-id="54b69-127">Teraz utworzysz klasę, która jest wywoływana ze stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="54b69-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="54b69-128">Metody tej klasy wyglądać podobnie do metody, które są używane w poprzednich samouczkach i zawierać atrybuty dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="54b69-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="54b69-129">Najpierw Dodaj nowy folder o nazwie **LOGIKI**.</span><span class="sxs-lookup"><span data-stu-id="54b69-129">First, add a new folder called **BLL**.</span></span>

![Dodaj folder](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="54b69-131">W folderze LOGIKI, Utwórz nową klasę o nazwie **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="54b69-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="54b69-132">Będzie zawierać wszystkie operacje danych, które pierwotnie znajdowały się w plikach związanym z kodem.</span><span class="sxs-lookup"><span data-stu-id="54b69-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="54b69-133">Te metody są prawie takie same jak metody w pliku związanym z kodem, ale będzie zawierać pewne zmiany.</span><span class="sxs-lookup"><span data-stu-id="54b69-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="54b69-134">Najważniejsze zmiany, należy pamiętać, jest już wykonujesz kodu z w ramach wystąpienia **strony** klasy.</span><span class="sxs-lookup"><span data-stu-id="54b69-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="54b69-135">Zawiera klasy strony **TryUpdateModel** metody i **ModelState** właściwości.</span><span class="sxs-lookup"><span data-stu-id="54b69-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="54b69-136">Kiedy ten kod jest przenoszony do warstwy logiki biznesowej, masz już wystąpienie klasy strony, aby wywołać te elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="54b69-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="54b69-137">Aby obejść ten problem, należy dodać **ModelMethodContext** parametru do dowolnej metody, która uzyskuje dostęp do TryUpdateModel lub ModelState.</span><span class="sxs-lookup"><span data-stu-id="54b69-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="54b69-138">Użyjesz tego parametru ModelMethodContext można wywołać TryUpdateModel lub pobrać element ModelState.</span><span class="sxs-lookup"><span data-stu-id="54b69-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="54b69-139">Nie trzeba wprowadzić na stronie sieci web, aby uwzględnić ten nowy parametr zmiany.</span><span class="sxs-lookup"><span data-stu-id="54b69-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="54b69-140">Zastąp kod w SchoolBL.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="54b69-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="54b69-141">Popraw istniejących stron do pobierania danych z warstwy logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="54b69-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="54b69-142">Na koniec przekonwertuje stron Students.aspx AddStudent.aspx i Courses.aspx korzystania z zapytania w pliku związanym z kodem za pomocą warstwy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="54b69-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="54b69-143">Pliki związane z kodem dla uczniów, AddStudent i kursów usunąć lub komentarz następujące metody zapytania:</span><span class="sxs-lookup"><span data-stu-id="54b69-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="54b69-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="54b69-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="54b69-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="54b69-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="54b69-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="54b69-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="54b69-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="54b69-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="54b69-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="54b69-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="54b69-149">Możesz teraz powinien mieć żadnego kodu w pliku związanym z kodem, które odnoszą się do operacji na danych.</span><span class="sxs-lookup"><span data-stu-id="54b69-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="54b69-150">**OnCallingDataMethods** programu obsługi zdarzeń można określić obiekt ma być używany dla metody dotyczące danych.</span><span class="sxs-lookup"><span data-stu-id="54b69-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="54b69-151">W Students.aspx Dodaj wartość dla tego programu obsługi zdarzeń i zmieniać nazwy metod danych do nazw metod w klasie logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="54b69-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="54b69-152">W pliku związanym z kodem Students.aspx należy zdefiniować program obsługi zdarzeń dla zdarzenia CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="54b69-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="54b69-153">Ta procedura obsługi zdarzeń służy do określenia klasy logiki biznesowej dla operacji na danych.</span><span class="sxs-lookup"><span data-stu-id="54b69-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="54b69-154">W AddStudent.aspx wprowadzić zmiany podobne.</span><span class="sxs-lookup"><span data-stu-id="54b69-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="54b69-155">W Courses.aspx wprowadzić zmiany podobne.</span><span class="sxs-lookup"><span data-stu-id="54b69-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="54b69-156">Uruchom aplikację i zwróć uwagę, wszystkie strony działać zgodnie z ich były wcześniej.</span><span class="sxs-lookup"><span data-stu-id="54b69-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="54b69-157">Logika sprawdzania poprawności działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="54b69-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="54b69-158">Wniosek</span><span class="sxs-lookup"><span data-stu-id="54b69-158">Conclusion</span></span>

<span data-ttu-id="54b69-159">W tym samouczku ponownie strukturę aplikacji przy użyciu warstwy dostępu do danych i warstwy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="54b69-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="54b69-160">Określono, że formanty danych użyć obiektu, który nie jest stroną bieżące operacje na danych.</span><span class="sxs-lookup"><span data-stu-id="54b69-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="54b69-161">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="54b69-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
