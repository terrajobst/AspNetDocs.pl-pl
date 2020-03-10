---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Część 3: Tworzenie kontrolera administratora | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556050"
---
# <a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="28a13-102">Część 3: Tworzenie kontrolera administratora</span><span class="sxs-lookup"><span data-stu-id="28a13-102">Part 3: Creating an Admin Controller</span></span>

<span data-ttu-id="28a13-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="28a13-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="28a13-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="28a13-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="28a13-105">Dodawanie kontrolera administratora</span><span class="sxs-lookup"><span data-stu-id="28a13-105">Add an Admin Controller</span></span>

<span data-ttu-id="28a13-106">W tej sekcji dodamy kontroler interfejsu API sieci Web, który obsługuje operacje CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie) dla produktów.</span><span class="sxs-lookup"><span data-stu-id="28a13-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="28a13-107">Kontroler będzie używać Entity Framework do komunikowania się z warstwą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="28a13-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="28a13-108">Tylko administratorzy będą mogli korzystać z tego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="28a13-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="28a13-109">Klienci będą uzyskiwać dostęp do produktów za pomocą innego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="28a13-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="28a13-110">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers.</span><span class="sxs-lookup"><span data-stu-id="28a13-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="28a13-111">Wybierz opcję **Dodaj** , a następnie pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="28a13-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="28a13-112">W oknie dialogowym **Dodawanie kontrolera** Nadaj nazwę kontrolerowi `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="28a13-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="28a13-113">W obszarze **szablon**wybierz pozycję &quot;kontroler interfejsu API z akcjami odczytu/zapisu, używając Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="28a13-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="28a13-114">W obszarze **Klasa modelu**wybierz pozycję "produkt (ProductStore. models)".</span><span class="sxs-lookup"><span data-stu-id="28a13-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="28a13-115">W obszarze **kontekst danych**wybierz opcję "&lt;nowym&gt;kontekstu danych".</span><span class="sxs-lookup"><span data-stu-id="28a13-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="28a13-116">Jeśli lista rozwijana **Klasa modelu** nie zawiera żadnych klas modelu, należy upewnić się, że projekt został skompilowany.</span><span class="sxs-lookup"><span data-stu-id="28a13-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="28a13-117">Entity Framework używa odbicia, dlatego wymaga skompilowanego zestawu.</span><span class="sxs-lookup"><span data-stu-id="28a13-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>

<span data-ttu-id="28a13-118">Wybranie opcji "&lt;nowego kontekstu danych&gt;" spowoduje otwarcie okna dialogowego **nowego kontekstu danych** .</span><span class="sxs-lookup"><span data-stu-id="28a13-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="28a13-119">Nazwij `ProductStore.Models.OrdersContext`kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="28a13-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="28a13-120">Kliknij przycisk **OK** , aby odrzucić okno dialogowe **nowego kontekstu danych** .</span><span class="sxs-lookup"><span data-stu-id="28a13-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="28a13-121">W oknie dialogowym **Dodawanie kontrolera** kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="28a13-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="28a13-122">Oto, co zostało dodane do projektu:</span><span class="sxs-lookup"><span data-stu-id="28a13-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="28a13-123">Klasa o nazwie `OrdersContext`, która pochodzi od **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="28a13-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="28a13-124">Ta klasa zapewnia Glue między modelami POCO i bazą danych.</span><span class="sxs-lookup"><span data-stu-id="28a13-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="28a13-125">Kontroler interfejsu API sieci Web o nazwie `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="28a13-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="28a13-126">Ten kontroler obsługuje operacje CRUD na wystąpieniach `Product`.</span><span class="sxs-lookup"><span data-stu-id="28a13-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="28a13-127">Używa klasy `OrdersContext` do komunikowania się z Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="28a13-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="28a13-128">Nowe parametry połączenia z bazą danych w pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="28a13-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="28a13-129">Otwórz plik OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="28a13-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="28a13-130">Zauważ, że Konstruktor Określa nazwę parametrów połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="28a13-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="28a13-131">Ta nazwa odwołuje się do parametrów połączenia, które zostały dodane do pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="28a13-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="28a13-132">Dodaj następujące właściwości do klasy `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="28a13-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="28a13-133">**Nieogólnymi** reprezentuje zestaw jednostek, które mogą być zapytania.</span><span class="sxs-lookup"><span data-stu-id="28a13-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="28a13-134">Oto kompletna lista dla klasy `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="28a13-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="28a13-135">Klasa `AdminController` definiuje pięć metod implementujących podstawowe funkcje CRUD.</span><span class="sxs-lookup"><span data-stu-id="28a13-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="28a13-136">Każda metoda odpowiada identyfikatorowi URI, który może być wywoływany przez klienta:</span><span class="sxs-lookup"><span data-stu-id="28a13-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="28a13-137">Controller — Metoda</span><span class="sxs-lookup"><span data-stu-id="28a13-137">Controller Method</span></span> | <span data-ttu-id="28a13-138">Opis</span><span class="sxs-lookup"><span data-stu-id="28a13-138">Description</span></span> | <span data-ttu-id="28a13-139">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="28a13-139">URI</span></span> | <span data-ttu-id="28a13-140">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="28a13-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="28a13-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="28a13-141">GetProducts</span></span> | <span data-ttu-id="28a13-142">Pobiera wszystkie produkty.</span><span class="sxs-lookup"><span data-stu-id="28a13-142">Gets all products.</span></span> | <span data-ttu-id="28a13-143">interfejsy API/produkty</span><span class="sxs-lookup"><span data-stu-id="28a13-143">api/products</span></span> | <span data-ttu-id="28a13-144">GET</span><span class="sxs-lookup"><span data-stu-id="28a13-144">GET</span></span> |
| <span data-ttu-id="28a13-145">Getproduct</span><span class="sxs-lookup"><span data-stu-id="28a13-145">GetProduct</span></span> | <span data-ttu-id="28a13-146">Znajduje produkt według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="28a13-146">Finds a product by ID.</span></span> | <span data-ttu-id="28a13-147">Interfejs API/produkty/*Identyfikator*</span><span class="sxs-lookup"><span data-stu-id="28a13-147">api/products/*id*</span></span> | <span data-ttu-id="28a13-148">GET</span><span class="sxs-lookup"><span data-stu-id="28a13-148">GET</span></span> |
| <span data-ttu-id="28a13-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="28a13-149">PutProduct</span></span> | <span data-ttu-id="28a13-150">Aktualizuje produkt.</span><span class="sxs-lookup"><span data-stu-id="28a13-150">Updates a product.</span></span> | <span data-ttu-id="28a13-151">Interfejs API/produkty/*Identyfikator*</span><span class="sxs-lookup"><span data-stu-id="28a13-151">api/products/*id*</span></span> | <span data-ttu-id="28a13-152">PUT</span><span class="sxs-lookup"><span data-stu-id="28a13-152">PUT</span></span> |
| <span data-ttu-id="28a13-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="28a13-153">PostProduct</span></span> | <span data-ttu-id="28a13-154">Tworzy nowy produkt.</span><span class="sxs-lookup"><span data-stu-id="28a13-154">Creates a new product.</span></span> | <span data-ttu-id="28a13-155">interfejsy API/produkty</span><span class="sxs-lookup"><span data-stu-id="28a13-155">api/products</span></span> | <span data-ttu-id="28a13-156">POST</span><span class="sxs-lookup"><span data-stu-id="28a13-156">POST</span></span> |
| <span data-ttu-id="28a13-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="28a13-157">DeleteProduct</span></span> | <span data-ttu-id="28a13-158">Usuwa produkt.</span><span class="sxs-lookup"><span data-stu-id="28a13-158">Deletes a product.</span></span> | <span data-ttu-id="28a13-159">Interfejs API/produkty/*Identyfikator*</span><span class="sxs-lookup"><span data-stu-id="28a13-159">api/products/*id*</span></span> | <span data-ttu-id="28a13-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="28a13-160">DELETE</span></span> |

<span data-ttu-id="28a13-161">Każda metoda wywołuje do `OrdersContext`, aby wykonać zapytanie względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="28a13-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="28a13-162">Metody modyfikujące wywołanie kolekcji (PUT, POST i DELETE) `db.SaveChanges`, aby zachować zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="28a13-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="28a13-163">Kontrolery są tworzone na żądanie HTTP, a następnie usuwane, dlatego należy zachować zmiany przed zwróceniem metody.</span><span class="sxs-lookup"><span data-stu-id="28a13-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="28a13-164">Dodawanie inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="28a13-164">Add a Database Initializer</span></span>

<span data-ttu-id="28a13-165">Entity Framework ma świetną funkcję, która umożliwia wypełnienie bazy danych przy uruchamianiu i automatyczne odtworzenie bazy danych przy każdym zmianie modeli.</span><span class="sxs-lookup"><span data-stu-id="28a13-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="28a13-166">Ta funkcja jest przydatna podczas opracowywania, ponieważ zawsze masz pewne dane testowe, nawet jeśli zmienisz modele.</span><span class="sxs-lookup"><span data-stu-id="28a13-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="28a13-167">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder modele i Utwórz nową klasę o nazwie `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="28a13-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="28a13-168">Wklej w następującej implementacji:</span><span class="sxs-lookup"><span data-stu-id="28a13-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="28a13-169">Przez dziedziczenie z klasy **DropCreateDatabaseIfModelChanges** , poinformujemy Entity Framework o porzucenia bazy danych przy każdej modyfikacji klas modelu.</span><span class="sxs-lookup"><span data-stu-id="28a13-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="28a13-170">Gdy Entity Framework tworzy (lub ponownie tworzy) bazę danych, wywołuje metodę **inicjatora** , aby wypełnić tabele.</span><span class="sxs-lookup"><span data-stu-id="28a13-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="28a13-171">Używamy metody **inicjatora** , aby dodać przykładowe produkty i przykładową kolejność.</span><span class="sxs-lookup"><span data-stu-id="28a13-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="28a13-172">Ta funkcja doskonale nadaje się do testowania, ale nie używa klasy **DropCreateDatabaseIfModelChanges** w środowisku produkcyjnym, ponieważ można utracić dane, jeśli ktoś zmieni klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="28a13-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="28a13-173">Następnie otwórz Global. asax i Dodaj następujący kod do **aplikacji\_Start** :</span><span class="sxs-lookup"><span data-stu-id="28a13-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="28a13-174">Wyślij żądanie do kontrolera</span><span class="sxs-lookup"><span data-stu-id="28a13-174">Send a Request to the Controller</span></span>

<span data-ttu-id="28a13-175">W tym momencie nie zapisano kodu klienta, ale można wywołać internetowy interfejs API przy użyciu przeglądarki sieci Web lub narzędzia debugowania HTTP, takiego jak [programu Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="28a13-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="28a13-176">W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowanie.</span><span class="sxs-lookup"><span data-stu-id="28a13-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="28a13-177">Przeglądarka sieci Web zostanie otwarta w `http://localhost:*portnum*/`, gdzie *portnum* jest numerem portu.</span><span class="sxs-lookup"><span data-stu-id="28a13-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="28a13-178">Wyślij żądanie HTTP do "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="28a13-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="28a13-179">Pierwsze żądanie może być wykonane wolno, ponieważ Entity Framework musi utworzyć i wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="28a13-179">The first request may be slow to complete, because Entity Framework needs to create and seed the database.</span></span> <span data-ttu-id="28a13-180">Odpowiedź powinna wyglądać podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="28a13-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="28a13-181">[Poprzednie](using-web-api-with-entity-framework-part-2.md)
> [dalej](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="28a13-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
