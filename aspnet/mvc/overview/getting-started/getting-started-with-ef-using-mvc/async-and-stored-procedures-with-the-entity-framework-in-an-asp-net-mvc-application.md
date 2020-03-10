---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: używanie procedur asynchronicznych i składowanych z Dr w aplikacji ASP.NET MVC'
description: W tym samouczku pokazano, jak zaimplementować model programowania asynchronicznego i dowiedzieć się, jak korzystać z procedur składowanych.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583441"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="41c55-103">Samouczek: używanie procedur asynchronicznych i składowanych z Dr w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="41c55-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="41c55-104">We wcześniejszych samouczkach pokazano, jak odczytywać i aktualizować dane przy użyciu modelu programowania synchronicznego.</span><span class="sxs-lookup"><span data-stu-id="41c55-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="41c55-105">W tym samouczku przedstawiono sposób implementacji asynchronicznego modelu programowania.</span><span class="sxs-lookup"><span data-stu-id="41c55-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="41c55-106">Kod asynchroniczny może poprawić wydajność aplikacji, ponieważ ułatwia ona korzystanie z zasobów serwera.</span><span class="sxs-lookup"><span data-stu-id="41c55-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="41c55-107">W tym samouczku pokazano również, jak używać procedur składowanych do operacji INSERT, Update i DELETE w jednostce.</span><span class="sxs-lookup"><span data-stu-id="41c55-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="41c55-108">Na koniec należy ponownie wdrożyć aplikację na platformie Azure wraz ze wszystkimi zmianami bazy danych, które zostały zaimplementowane od czasu pierwszego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="41c55-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="41c55-109">Na poniższych ilustracjach przedstawiono niektóre ze stron, z którymi będziesz korzystać.</span><span class="sxs-lookup"><span data-stu-id="41c55-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Strona działy](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Utwórz dział](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="41c55-112">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="41c55-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41c55-113">Informacje o kodzie asynchronicznym</span><span class="sxs-lookup"><span data-stu-id="41c55-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="41c55-114">Tworzenie kontrolera działu</span><span class="sxs-lookup"><span data-stu-id="41c55-114">Create a Department controller</span></span>
> * <span data-ttu-id="41c55-115">Korzystanie z procedur składowanych</span><span class="sxs-lookup"><span data-stu-id="41c55-115">Use stored procedures</span></span>
> * <span data-ttu-id="41c55-116">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="41c55-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41c55-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="41c55-117">Prerequisites</span></span>

* [<span data-ttu-id="41c55-118">Aktualizowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="41c55-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="41c55-119">Dlaczego warto używać kodu asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="41c55-119">Why use asynchronous code</span></span>

<span data-ttu-id="41c55-120">Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte.</span><span class="sxs-lookup"><span data-stu-id="41c55-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="41c55-121">Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane.</span><span class="sxs-lookup"><span data-stu-id="41c55-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="41c55-122">Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć.</span><span class="sxs-lookup"><span data-stu-id="41c55-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="41c55-123">Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań.</span><span class="sxs-lookup"><span data-stu-id="41c55-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="41c55-124">W efekcie kod asynchroniczny umożliwia efektywniejsze korzystanie z zasobów serwera, a serwer jest włączony do obsługi większej ilości ruchu bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="41c55-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="41c55-125">We wcześniejszych wersjach programu .NET, pisanie i testowanie kodu asynchronicznego jest skomplikowane, podatne na błędy i trudne do debugowania.</span><span class="sxs-lookup"><span data-stu-id="41c55-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="41c55-126">W przypadku programu .NET 4,5, pisania, testowania i debugowania kodu asynchronicznego jest to znacznie prostsze, dlatego należy zwykle pisać kod asynchroniczny, chyba że masz powód.</span><span class="sxs-lookup"><span data-stu-id="41c55-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="41c55-127">Kod asynchroniczny wprowadza niewielką ilość narzutów, ale w przypadku niskiego natężenia ruchu zwiększenie wydajności jest mało znaczące</span><span class="sxs-lookup"><span data-stu-id="41c55-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="41c55-128">Aby uzyskać więcej informacji o programowaniu asynchronicznym, zobacz [Korzystanie z asynchronicznej obsługi programu .NET 4.5, aby uniknąć blokowania wywołań](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="41c55-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="41c55-129">Utwórz kontroler działu</span><span class="sxs-lookup"><span data-stu-id="41c55-129">Create Department controller</span></span>

<span data-ttu-id="41c55-130">Utwórz kontroler działu w taki sam sposób, jak w przypadku wcześniejszych kontrolerów, z wyjątkiem tego, zaznacz pole wyboru **Użyj akcji kontrolera asynchronicznego** .</span><span class="sxs-lookup"><span data-stu-id="41c55-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="41c55-131">W poniższych podświetlech pokazano, co zostało dodane do kodu synchronicznego dla metody `Index`, aby uczynić ją asynchroniczną:</span><span class="sxs-lookup"><span data-stu-id="41c55-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="41c55-132">Zostały zastosowane cztery zmiany, aby włączyć asynchroniczne wykonywanie zapytania bazy danych Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="41c55-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="41c55-133">Metoda jest oznaczona za pomocą słowa kluczowego `async`, co informuje kompilator, aby wygenerował wywołania zwrotne dla części treści metody i automatycznie utworzyć obiekt `Task<ActionResult>`, który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="41c55-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="41c55-134">Typ zwracany został zmieniony z `ActionResult` na `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="41c55-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="41c55-135">Typ `Task<T>` reprezentuje bieżącą współpracę z wynikiem typu `T`.</span><span class="sxs-lookup"><span data-stu-id="41c55-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="41c55-136">Słowo kluczowe `await` zostało zastosowane do wywołania usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="41c55-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="41c55-137">Gdy kompilator widzi to słowo kluczowe, w tle dzieli metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="41c55-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="41c55-138">Pierwsza część jest zakończona operacją, która jest uruchamiana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="41c55-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="41c55-139">Druga część jest umieszczana w metodzie wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="41c55-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="41c55-140">Wywołano asynchroniczną wersję metody rozszerzenia `ToList`.</span><span class="sxs-lookup"><span data-stu-id="41c55-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="41c55-141">Dlaczego instrukcja `departments.ToList` została zmodyfikowana, ale nie instrukcją `departments = db.Departments`?</span><span class="sxs-lookup"><span data-stu-id="41c55-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="41c55-142">Przyczyną jest to, że tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="41c55-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="41c55-143">Instrukcja `departments = db.Departments` konfiguruje zapytanie, ale zapytanie nie jest wykonywane do momentu wywołania metody `ToList`.</span><span class="sxs-lookup"><span data-stu-id="41c55-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="41c55-144">W związku z tym tylko Metoda `ToList` jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="41c55-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="41c55-145">W metodzie `Details` i `HttpGet` `Edit` i `Delete` Metoda `Find` jest taka, która powoduje wysłanie zapytania do bazy danych, dzięki czemu jest to metoda, która jest wykonywana asynchronicznie:</span><span class="sxs-lookup"><span data-stu-id="41c55-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="41c55-146">W metodach `Create`, `HttpPost Edit`i `DeleteConfirmed` jest to wywołanie metody `SaveChanges`, które powoduje wykonanie polecenia, nie instrukcje, takie jak `db.Departments.Add(department)`, które powodują, że tylko jednostki w pamięci mają być modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="41c55-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="41c55-147">Otwórz *Views\Department\Index.cshtml*i Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="41c55-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="41c55-148">Ten kod zmienia tytuł z indeksu na działy, przenosi nazwę administratora z prawej strony i udostępnia pełną nazwę administratora.</span><span class="sxs-lookup"><span data-stu-id="41c55-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="41c55-149">W widokach tworzenie, usuwanie, szczegóły i edycja Zmień podpis pola `InstructorID` na "Administrator" tak samo jak w przypadku zmiany pola Nazwa działu na "Wydział" w widokach kursu.</span><span class="sxs-lookup"><span data-stu-id="41c55-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="41c55-150">W widokach tworzenie i edytowanie Użyj następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="41c55-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="41c55-151">W widokach Usuń i szczegóły Użyj następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="41c55-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="41c55-152">Uruchom aplikację, a następnie kliknij kartę **działy** .</span><span class="sxs-lookup"><span data-stu-id="41c55-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="41c55-153">Wszystko działa tak samo jak w przypadku innych kontrolerów, ale w tym kontrolerze wszystkie zapytania SQL są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="41c55-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="41c55-154">Niektóre kwestie, o których należy wiedzieć w przypadku używania programowania asynchronicznego z Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="41c55-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="41c55-155">Kod asynchroniczny nie jest bezpieczny wątkowo.</span><span class="sxs-lookup"><span data-stu-id="41c55-155">The async code is not thread safe.</span></span> <span data-ttu-id="41c55-156">Innymi słowy, innymi słowy, nie należy próbować wykonać wielu operacji równolegle przy użyciu tego samego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="41c55-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="41c55-157">Jeśli chcesz wykorzystać zalety wydajności w kodzie asynchronicznym, upewnij się, że wszystkie używane pakiety biblioteki (na przykład na potrzeby stronicowania) używają również metody asynchronicznej, jeśli wywołują wszelkie Entity Framework metod, które powodują wysłanie zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="41c55-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="41c55-158">Korzystanie z procedur składowanych</span><span class="sxs-lookup"><span data-stu-id="41c55-158">Use stored procedures</span></span>

<span data-ttu-id="41c55-159">Niektórzy deweloperzy i przetwarzający wolą używać procedur składowanych do uzyskiwania dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="41c55-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="41c55-160">We wcześniejszych wersjach Entity Framework można pobrać dane przy użyciu procedury składowanej przez [wykonanie nieprzetworzonego zapytania SQL](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ale nie można wydać instrukcji EF używania procedur składowanych dla operacji aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="41c55-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="41c55-161">W programie Dr 6 można łatwo skonfigurować Code First do korzystania z procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="41c55-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="41c55-162">W *DAL\SchoolContext.cs*Dodaj wyróżniony kod do metody `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="41c55-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="41c55-163">Ten kod instruuje Entity Framework, aby używać procedur składowanych do operacji INSERT, Update i DELETE w jednostce `Department`.</span><span class="sxs-lookup"><span data-stu-id="41c55-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="41c55-164">W konsoli Zarządzanie pakietami wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="41c55-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="41c55-165">Otwórz *migracje\\&lt;sygnatury czasowej&gt;\_DepartmentSP.cs* , aby wyświetlić kod w metodzie `Up`, która tworzy procedury składowane INSERT, Update i DELETE:</span><span class="sxs-lookup"><span data-stu-id="41c55-165">Open *Migrations\\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="41c55-166">W konsoli Zarządzanie pakietami wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="41c55-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="41c55-167">Uruchom aplikację w trybie debugowania, kliknij kartę **działy** , a następnie kliknij przycisk **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="41c55-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="41c55-168">Wprowadź dane dla nowego działu, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="41c55-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="41c55-169">W programie Visual Studio zapoznaj się z dziennikami w oknie **danych wyjściowych** , aby zobaczyć, że procedura składowana została użyta do wstawienia nowego wiersza działu.</span><span class="sxs-lookup"><span data-stu-id="41c55-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![Wstawianie działu SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="41c55-171">Code First tworzy nazwy domyślnej procedury przechowywanej.</span><span class="sxs-lookup"><span data-stu-id="41c55-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="41c55-172">W przypadku korzystania z istniejącej bazy danych może być konieczne dostosowanie nazw procedur składowanych, aby można było używać procedur składowanych zdefiniowanych już w bazie danych programu.</span><span class="sxs-lookup"><span data-stu-id="41c55-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="41c55-173">Aby uzyskać informacje o tym, jak to zrobić, zobacz [Entity Framework Code First Wstawianie/aktualizowanie/usuwanie procedur składowanych](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="41c55-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="41c55-174">Jeśli chcesz dostosować wygenerowane procedury składowane, możesz edytować kod szkieletowy dla metody `Up` migracji, która tworzy procedurę składowaną.</span><span class="sxs-lookup"><span data-stu-id="41c55-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="41c55-175">W ten sposób zmiany zostaną odzwierciedlone za każdym razem, gdy migracja zostanie uruchomiona i zostanie zastosowana do produkcyjnej bazy danych, gdy migracja zostanie uruchomiona automatycznie w środowisku produkcyjnym po wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="41c55-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="41c55-176">Jeśli chcesz zmienić istniejącą procedurę składowaną, która została utworzona w poprzedniej migracji, możesz użyć polecenia Add-Migration do wygenerowania pustej migracji, a następnie ręcznie napisać kod, który wywołuje metodę [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .</span><span class="sxs-lookup"><span data-stu-id="41c55-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="41c55-177">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="41c55-177">Deploy to Azure</span></span>

<span data-ttu-id="41c55-178">Ta sekcja wymaga wykonania opcjonalnej sekcji **wdrażanie aplikacji do platformy Azure** w samouczku [migracja i wdrażanie](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tej serii.</span><span class="sxs-lookup"><span data-stu-id="41c55-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="41c55-179">W przypadku migracji błędów, które zostały rozwiązane przez usunięcie bazy danych w projekcie lokalnym, Pomiń tę sekcję.</span><span class="sxs-lookup"><span data-stu-id="41c55-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="41c55-180">W programie Visual Studio kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="41c55-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="41c55-181">Kliknij przycisk **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="41c55-181">Click **Publish**.</span></span>

    <span data-ttu-id="41c55-182">Program Visual Studio wdroży aplikację na platformie Azure, a aplikacja zostanie otwarta w domyślnej przeglądarce działającej na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="41c55-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="41c55-183">Przetestuj aplikację, aby upewnić się, że działa.</span><span class="sxs-lookup"><span data-stu-id="41c55-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="41c55-184">Przy pierwszym uruchomieniu strony, która uzyskuje dostęp do bazy danych, Entity Framework uruchamia wszystkie migracje `Up` metody wymagane w celu przeprowadzenia Aktualności bazy danych z bieżącym modelem danych.</span><span class="sxs-lookup"><span data-stu-id="41c55-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="41c55-185">Teraz możesz użyć wszystkich stron sieci Web, które zostały dodane od czasu ostatniego wdrożenia, łącznie ze stronami działu, które zostały dodane w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="41c55-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="41c55-186">Uzyskiwanie kodu</span><span class="sxs-lookup"><span data-stu-id="41c55-186">Get the code</span></span>

[<span data-ttu-id="41c55-187">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="41c55-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="41c55-188">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="41c55-188">Additional resources</span></span>

<span data-ttu-id="41c55-189">Linki do innych zasobów Entity Framework można znaleźć w [zasobach zalecanych przez dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="41c55-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="41c55-190">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="41c55-190">Next steps</span></span>

<span data-ttu-id="41c55-191">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="41c55-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41c55-192">Informacje o kodzie asynchronicznym</span><span class="sxs-lookup"><span data-stu-id="41c55-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="41c55-193">Utworzono kontroler działu</span><span class="sxs-lookup"><span data-stu-id="41c55-193">Created a Department controller</span></span>
> * <span data-ttu-id="41c55-194">Używane procedury składowane</span><span class="sxs-lookup"><span data-stu-id="41c55-194">Used stored procedures</span></span>
> * <span data-ttu-id="41c55-195">Wdrożone na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="41c55-195">Deployed to Azure</span></span>

<span data-ttu-id="41c55-196">Przejdź do następnego artykułu, aby dowiedzieć się, jak obsłużyć konflikty, gdy wielu użytkowników aktualizuje tę samą jednostkę w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="41c55-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="41c55-197">Obsługa współbieżności</span><span class="sxs-lookup"><span data-stu-id="41c55-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
