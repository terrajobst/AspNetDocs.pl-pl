---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: Część 6. Tworzenie kontrolerów produktu i kolejności | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600029"
---
# <a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="337d2-102">Część 6. Tworzenie kontrolerów produktu i kolejności</span><span class="sxs-lookup"><span data-stu-id="337d2-102">Part 6: Creating Product and Order Controllers</span></span>

<span data-ttu-id="337d2-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="337d2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="337d2-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="337d2-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="337d2-105">Dodawanie kontrolera produktów</span><span class="sxs-lookup"><span data-stu-id="337d2-105">Add a Products Controller</span></span>

<span data-ttu-id="337d2-106">Kontroler administracyjny jest przeznaczony dla użytkowników, którzy mają uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="337d2-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="337d2-107">Z drugiej strony klienci mogą wyświetlać produkty, ale nie mogą ich tworzyć, aktualizować ani usuwać.</span><span class="sxs-lookup"><span data-stu-id="337d2-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="337d2-108">Można łatwo ograniczyć dostęp do metod post, PUT i DELETE, pozostawiając otwarte metody get.</span><span class="sxs-lookup"><span data-stu-id="337d2-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="337d2-109">Sprawdź dane, które są zwracane dla produktu:</span><span class="sxs-lookup"><span data-stu-id="337d2-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="337d2-110">Właściwość `ActualCost` nie powinna być widoczna dla klientów!</span><span class="sxs-lookup"><span data-stu-id="337d2-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="337d2-111">Rozwiązaniem jest zdefiniowanie *obiektu transferu danych* (DTO), który zawiera podzbiór właściwości, które powinny być widoczne dla klientów.</span><span class="sxs-lookup"><span data-stu-id="337d2-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="337d2-112">Do `ProductDTO` wystąpień będziemy używać wystąpień LINQ to Project `Product`.</span><span class="sxs-lookup"><span data-stu-id="337d2-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="337d2-113">Dodaj klasę o nazwie `ProductDTO` do folderu modele.</span><span class="sxs-lookup"><span data-stu-id="337d2-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="337d2-114">Teraz Dodaj kontroler.</span><span class="sxs-lookup"><span data-stu-id="337d2-114">Now add the controller.</span></span> <span data-ttu-id="337d2-115">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers.</span><span class="sxs-lookup"><span data-stu-id="337d2-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="337d2-116">Wybierz pozycję **Dodaj**, a następnie pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="337d2-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="337d2-117">W oknie dialogowym **Dodawanie kontrolera** nazwij kontroler &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="337d2-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="337d2-118">W obszarze **szablon**wybierz pozycję **pusty kontroler interfejsu API**.</span><span class="sxs-lookup"><span data-stu-id="337d2-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="337d2-119">Zastąp wszystko w pliku źródłowym następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="337d2-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="337d2-120">Kontroler nadal używa `OrdersContext` do wykonywania zapytań w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="337d2-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="337d2-121">Jednak zamiast zwracać `Product` wystąpienia bezpośrednio, wywołamy `MapProducts`, aby zaprojektować je na wystąpieniach `ProductDTO`:</span><span class="sxs-lookup"><span data-stu-id="337d2-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="337d2-122">Metoda `MapProducts` zwraca interfejs **IQueryable**, więc możemy złożyć wynik z innymi parametrami zapytania.</span><span class="sxs-lookup"><span data-stu-id="337d2-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="337d2-123">Można to zobaczyć w metodzie `GetProduct`, która dodaje klauzulę **WHERE** do zapytania:</span><span class="sxs-lookup"><span data-stu-id="337d2-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="337d2-124">Dodawanie kontrolera zamówień</span><span class="sxs-lookup"><span data-stu-id="337d2-124">Add an Orders Controller</span></span>

<span data-ttu-id="337d2-125">Następnie Dodaj kontroler, który umożliwia użytkownikom tworzenie i wyświetlanie zamówień.</span><span class="sxs-lookup"><span data-stu-id="337d2-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="337d2-126">Zaczniemy od innego DTO.</span><span class="sxs-lookup"><span data-stu-id="337d2-126">We'll start with another DTO.</span></span> <span data-ttu-id="337d2-127">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder modele i Dodaj klasę o nazwie `OrderDTO` Użyj następującej implementacji:</span><span class="sxs-lookup"><span data-stu-id="337d2-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="337d2-128">Teraz Dodaj kontroler.</span><span class="sxs-lookup"><span data-stu-id="337d2-128">Now add the controller.</span></span> <span data-ttu-id="337d2-129">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers.</span><span class="sxs-lookup"><span data-stu-id="337d2-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="337d2-130">Wybierz pozycję **Dodaj**, a następnie pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="337d2-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="337d2-131">W oknie dialogowym **Dodawanie kontrolera** ustaw następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="337d2-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="337d2-132">W obszarze **nazwa kontrolera**wprowadź wartość "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="337d2-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="337d2-133">W obszarze **szablon**wybierz pozycję "kontroler interfejsu API z akcjami odczytu/zapisu, używając Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="337d2-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="337d2-134">W obszarze **Klasa modelu**wybierz&quot;&quot;zamówienie (ProductStore. models).</span><span class="sxs-lookup"><span data-stu-id="337d2-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="337d2-135">W obszarze **Klasa kontekstu danych**wybierz&quot;&quot;OrdersContext (ProductStore. models).</span><span class="sxs-lookup"><span data-stu-id="337d2-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="337d2-136">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="337d2-136">Click **Add**.</span></span> <span data-ttu-id="337d2-137">Spowoduje to dodanie pliku o nazwie OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="337d2-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="337d2-138">Następnie musimy zmodyfikować domyślną implementację kontrolera.</span><span class="sxs-lookup"><span data-stu-id="337d2-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="337d2-139">Najpierw usuń `PutOrder` i `DeleteOrder` metody.</span><span class="sxs-lookup"><span data-stu-id="337d2-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="337d2-140">W tym przykładzie klienci nie mogą modyfikować ani usuwać istniejących zamówień.</span><span class="sxs-lookup"><span data-stu-id="337d2-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="337d2-141">W rzeczywistej aplikacji potrzebna jest duża logika zaplecza, która będzie obsługiwać te przypadki.</span><span class="sxs-lookup"><span data-stu-id="337d2-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="337d2-142">(Na przykład czy zamówienie zostało już wysłane?)</span><span class="sxs-lookup"><span data-stu-id="337d2-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="337d2-143">Zmień metodę `GetOrders`, aby zwracała tylko zamówienia należące do użytkownika:</span><span class="sxs-lookup"><span data-stu-id="337d2-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="337d2-144">Zmień metodę `GetOrder` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="337d2-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="337d2-145">Poniżej przedstawiono zmiany wprowadzone w metodzie:</span><span class="sxs-lookup"><span data-stu-id="337d2-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="337d2-146">Wartość zwracana jest wystąpieniem `OrderDTO`, a nie `Order`.</span><span class="sxs-lookup"><span data-stu-id="337d2-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="337d2-147">Gdy wysyłamy zapytanie do bazy danych dla zamówienia, użyjemy metody [DBQuery. include](https://msdn.microsoft.com/library/gg696395) , aby pobrać powiązane `OrderDetail` i `Product` jednostki.</span><span class="sxs-lookup"><span data-stu-id="337d2-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="337d2-148">Wynik zostanie spłaszczony przy użyciu projekcji.</span><span class="sxs-lookup"><span data-stu-id="337d2-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="337d2-149">Odpowiedź HTTP będzie zawierać tablicę produktów z ilościami:</span><span class="sxs-lookup"><span data-stu-id="337d2-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="337d2-150">Ten format jest łatwiejszy w przypadku klientów korzystających z grafu oryginalnego obiektu, który zawiera zagnieżdżone jednostki (zamówienie, szczegóły i produkty).</span><span class="sxs-lookup"><span data-stu-id="337d2-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="337d2-151">Ostatnia metoda do uwzględnienia `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="337d2-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="337d2-152">Teraz ta metoda przyjmuje wystąpienie `Order`.</span><span class="sxs-lookup"><span data-stu-id="337d2-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="337d2-153">Należy jednak wziąć pod uwagę to, co się stanie, jeśli klient wyśle treść żądania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="337d2-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="337d2-154">Jest to dobrze skonstruowana kolejność, a Entity Framework będzie Happily Wstawianie go do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="337d2-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="337d2-155">Ale zawiera jednostkę produktu, która nie istniała wcześniej.</span><span class="sxs-lookup"><span data-stu-id="337d2-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="337d2-156">Klient właśnie utworzył nowy produkt w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="337d2-156">The client just created a new product in our database!</span></span> <span data-ttu-id="337d2-157">Jest to nieoczekiwane dla działu realizacji zamówień, gdy zobaczymy zamówienie dla Koala.</span><span class="sxs-lookup"><span data-stu-id="337d2-157">This will be a surprise to the order fulfillment department, when they see an order for koala bears.</span></span> <span data-ttu-id="337d2-158">Dobrym wymaganiem jest, aby zachować ostrożność w przypadku danych akceptowanych w żądaniu POST lub PUT.</span><span class="sxs-lookup"><span data-stu-id="337d2-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="337d2-159">Aby uniknąć tego problemu, Zmień metodę `PostOrder`, aby wykonać `OrderDTO` wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="337d2-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="337d2-160">Użyj `OrderDTO`, aby utworzyć `Order`.</span><span class="sxs-lookup"><span data-stu-id="337d2-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="337d2-161">Zwróć uwagę, że używamy właściwości `ProductID` i `Quantity` i zignorujemy wszelkie wartości, które klient wysłał jako nazwę produktu lub cenę.</span><span class="sxs-lookup"><span data-stu-id="337d2-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="337d2-162">Jeśli identyfikator produktu jest nieprawidłowy, nastąpi naruszenie ograniczenia klucza obcego w bazie danych, a wstawienie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="337d2-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="337d2-163">Oto pełna Metoda `PostOrder`:</span><span class="sxs-lookup"><span data-stu-id="337d2-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="337d2-164">Na koniec Dodaj atrybut **Autoryzuj** do kontrolera:</span><span class="sxs-lookup"><span data-stu-id="337d2-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="337d2-165">Teraz tylko zarejestrowani użytkownicy mogą tworzyć i wyświetlać zamówienia.</span><span class="sxs-lookup"><span data-stu-id="337d2-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="337d2-166">[Poprzednie](using-web-api-with-entity-framework-part-5.md)
> [dalej](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="337d2-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
