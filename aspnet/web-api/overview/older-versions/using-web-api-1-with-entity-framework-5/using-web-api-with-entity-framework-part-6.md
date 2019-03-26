---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: Część 6. Tworzenie kontrolerów produktów i zamówień | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 21cfbd0bf691ea033e9a5a873ab49c83507750d5
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58425967"
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="cc1bc-102">Część 6. Tworzenie kontrolerów produktów i zamówień</span><span class="sxs-lookup"><span data-stu-id="cc1bc-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="cc1bc-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cc1bc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cc1bc-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="cc1bc-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="cc1bc-105">Dodawanie kontrolera produktów</span><span class="sxs-lookup"><span data-stu-id="cc1bc-105">Add a Products Controller</span></span>

<span data-ttu-id="cc1bc-106">Kontrolera administratora jest przeznaczony dla użytkowników, którzy mają uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="cc1bc-107">Klientów, z drugiej strony, Wyświetl produkty, ale nie można utworzyć, zaktualizować lub je usunąć.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="cc1bc-108">Firma Microsoft może łatwo ograniczyć dostęp do metody Post, Put i Delete, przy równoczesnym pozostawieniu otwartego metody Get.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="cc1bc-109">Można jednak przyjrzeć się danym, która jest zwracana dla produktu:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="cc1bc-110">`ActualCost` Właściwości nie powinny być widoczne dla klientów!</span><span class="sxs-lookup"><span data-stu-id="cc1bc-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="cc1bc-111">Rozwiązanie polega na zdefiniowaniu *obiekt transferu danych* (DTO), który zawiera podzbiór właściwości, które powinny być widoczne dla klientów.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="cc1bc-112">Firma Microsoft użyje LINQ do projektu `Product` wystąpień do `ProductDTO` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="cc1bc-113">Dodaj klasę o nazwie `ProductDTO` do folderu modeli.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="cc1bc-114">Teraz Dodaj kontroler.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-114">Now add the controller.</span></span> <span data-ttu-id="cc1bc-115">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="cc1bc-116">Wybierz **Dodaj**, a następnie wybierz **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="cc1bc-117">W oknie dialogowym **Dodawanie kontrolera** nazwij kontrolera &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="cc1bc-118">W obszarze **szablonu**, wybierz opcję **kontrolera pusty interfejs API**.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="cc1bc-119">Zastąp całą zawartość pliku źródłowego, z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="cc1bc-120">Nadal używa kontrolera `OrdersContext` do wykonywania zapytań w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="cc1bc-121">Jednak zamiast zwracać `Product` wystąpień bezpośrednio, nazywamy `MapProducts` do projektu je na `ProductDTO` wystąpień:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="cc1bc-122">`MapProducts` Metoda zwraca **IQueryable**, dzięki czemu możemy narzędzia compose wyników za pomocą inne parametry zapytania.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="cc1bc-123">Widać to w `GetProduct` metody, która dodaje **gdzie** klauzuli kwerendy:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="cc1bc-124">Dodawanie kontrolera Orders</span><span class="sxs-lookup"><span data-stu-id="cc1bc-124">Add an Orders Controller</span></span>

<span data-ttu-id="cc1bc-125">Następnie dodaj kontroler, który pozwala użytkownikom tworzyć i wyświetlać zamówienia.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="cc1bc-126">Rozpoczniemy od innego DTO.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-126">We'll start with another DTO.</span></span> <span data-ttu-id="cc1bc-127">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli, a następnie Dodaj klasę o nazwie `OrderDTO` Użyj następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="cc1bc-128">Teraz Dodaj kontroler.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-128">Now add the controller.</span></span> <span data-ttu-id="cc1bc-129">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="cc1bc-130">Wybierz **Dodaj**, a następnie wybierz **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="cc1bc-131">W **Dodaj kontroler** okna dialogowego, ustaw następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="cc1bc-132">W obszarze **nazwy kontrolera**, wprowadź "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="cc1bc-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="cc1bc-133">W obszarze **szablonu**, wybierz opcję "Kontroler interfejsu API z akcjami odczytu/zapisu, używający narzędzia Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="cc1bc-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="cc1bc-134">W obszarze **klasa modelu**, wybierz opcję &quot;zamówienia (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="cc1bc-135">W obszarze **klasa kontekstu danych**, wybierz opcję &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="cc1bc-136">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-136">Click **Add**.</span></span> <span data-ttu-id="cc1bc-137">Spowoduje to dodanie pliku o nazwie OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="cc1bc-138">Następnie należy zmodyfikować domyślną implementację elementu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="cc1bc-139">Najpierw usuń `PutOrder` i `DeleteOrder` metody.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="cc1bc-140">W tym przykładzie klienci nie mogą modyfikować ani usuwać istniejące zamówienia.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="cc1bc-141">W rzeczywistej aplikacji będziesz potrzebować mnóstwo logiki zaplecza w takich sytuacjach.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="cc1bc-142">(Na przykład został już wysłaniu zamówienia?)</span><span class="sxs-lookup"><span data-stu-id="cc1bc-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="cc1bc-143">Zmiana `GetOrders` metodę, aby zwrócić tylko zamówienia, które należą do użytkownika:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="cc1bc-144">Zmiana `GetOrder` metody w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="cc1bc-145">Poniżej przedstawiono zmiany wprowadzone do metody:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="cc1bc-146">Wartość zwracana jest `OrderDTO` wystąpienia, zamiast `Order`.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="cc1bc-147">Firma Microsoft zapytania bazy danych dla zamówienia, używamy [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) — w celu pobrania powiązane `OrderDetail` i `Product` jednostek.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="cc1bc-148">Firma Microsoft spłaszczanie wynik za pomocą projekcji.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="cc1bc-149">Odpowiedź HTTP będzie zawierać tablicę produkty z ilości:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="cc1bc-150">Ten format jest łatwiejsze w przypadku klientów z niż oryginalny wykresu obiektu, który zawiera zagnieżdżony jednostki (kolejność, szczegóły i produktów).</span><span class="sxs-lookup"><span data-stu-id="cc1bc-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="cc1bc-151">Last — metoda wziąć pod uwagę jej `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="cc1bc-152">W tej chwili, ta metoda przyjmuje `Order` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="cc1bc-153">Jednak należy wziąć pod uwagę, co się stanie, jeśli klient wysyła treści żądania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="cc1bc-154">Jest to dobrze kolejności i Entity Framework trafem korzysta wstawi je do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="cc1bc-155">Ale zawiera jednostkę produktu, która wcześniej nie istniał.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="cc1bc-156">Klient właśnie utworzony nowy produkt w naszej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="cc1bc-156">The client just created a new product in our database!</span></span> <span data-ttu-id="cc1bc-157">Będzie to Zaskoczenie do działu realizacji zamówienia, po zgłoszeniu zamówienie koala polarnych.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-157">This will be a surprise to the order fulfillment department, when they see an order for koala bears.</span></span> <span data-ttu-id="cc1bc-158">Wnioski, należy zachować ostrożność bardzo dane, które akceptują żądania POST lub PUT.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="cc1bc-159">Aby uniknąć tego problemu, należy zmienić `PostOrder` metodę, aby wykonać `OrderDTO` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="cc1bc-160">Użyj `OrderDTO` utworzyć `Order`.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="cc1bc-161">Należy zauważyć, że używamy `ProductID` i `Quantity` właściwości, a firma Microsoft Ignoruj dowolnych wartości, które klient wysyłał dla nazwy produktu i cenę.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="cc1bc-162">Jeśli identyfikator produktu nie jest prawidłowy, spowodują naruszenie, ograniczenie klucza obcego w bazie danych, a następnie Wstaw zakończy się niepowodzeniem, zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="cc1bc-163">Poniżej przedstawiono pełne `PostOrder` metody:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="cc1bc-164">Na koniec należy dodać **Autoryzuj** atrybutu do kontrolera:</span><span class="sxs-lookup"><span data-stu-id="cc1bc-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="cc1bc-165">Teraz tylko zarejestrowani użytkownicy można utworzyć lub wyświetlić zamówienia.</span><span class="sxs-lookup"><span data-stu-id="cc1bc-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc1bc-166">[Poprzednie](using-web-api-with-entity-framework-part-5.md)
> [dalej](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="cc1bc-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
