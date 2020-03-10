---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Część 7: Tworzenie strony głównej | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598680"
---
# <a name="part-7-creating-the-main-page"></a><span data-ttu-id="a20f4-102">Część 7: Tworzenie strony głównej</span><span class="sxs-lookup"><span data-stu-id="a20f4-102">Part 7: Creating the Main Page</span></span>

<span data-ttu-id="a20f4-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a20f4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a20f4-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="a20f4-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="a20f4-105">Tworzenie strony głównej</span><span class="sxs-lookup"><span data-stu-id="a20f4-105">Creating the Main Page</span></span>

<span data-ttu-id="a20f4-106">W tej sekcji utworzysz główną stronę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a20f4-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="a20f4-107">Ta strona będzie bardziej złożona niż strona administracyjna, dlatego będziemy rozmieścić ją w kilku krokach.</span><span class="sxs-lookup"><span data-stu-id="a20f4-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="a20f4-108">W ten sposób zobaczysz bardziej zaawansowane techniki odcinania. js.</span><span class="sxs-lookup"><span data-stu-id="a20f4-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="a20f4-109">Oto podstawowy układ strony:</span><span class="sxs-lookup"><span data-stu-id="a20f4-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="a20f4-110">"Produkty" przechowuje tablicę produktów.</span><span class="sxs-lookup"><span data-stu-id="a20f4-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="a20f4-111">"Koszyk" zawiera tablicę produktów z ilościami.</span><span class="sxs-lookup"><span data-stu-id="a20f4-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="a20f4-112">Kliknięcie pozycji "Dodaj do koszyka" powoduje aktualizację koszyka.</span><span class="sxs-lookup"><span data-stu-id="a20f4-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="a20f4-113">"Zamówienia" przechowuje tablicę identyfikatorów zamówień.</span><span class="sxs-lookup"><span data-stu-id="a20f4-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="a20f4-114">"Szczegóły" zawiera szczegółowe informacje o zamówieniach, czyli tablicę elementów (produkty z ilościami)</span><span class="sxs-lookup"><span data-stu-id="a20f4-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="a20f4-115">Zaczniemy od definiowania podstawowego układu w języku HTML, bez powiązania danych ani skryptu.</span><span class="sxs-lookup"><span data-stu-id="a20f4-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="a20f4-116">Otwórz widok plików/home/index. cshtml i Zastąp całą zawartość następującym:</span><span class="sxs-lookup"><span data-stu-id="a20f4-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="a20f4-117">Następnie Dodaj sekcję skrypty i Utwórz pusty model widoku:</span><span class="sxs-lookup"><span data-stu-id="a20f4-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="a20f4-118">W oparciu o przedstawiony wcześniej projekt nasz model widoku wymaga observables dla produktów, koszyków, zamówień i szczegółów.</span><span class="sxs-lookup"><span data-stu-id="a20f4-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="a20f4-119">Dodaj następujące zmienne do obiektu `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="a20f4-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="a20f4-120">Użytkownicy mogą dodawać elementy z listy produktów do koszyka i usuwać elementy z koszyka.</span><span class="sxs-lookup"><span data-stu-id="a20f4-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="a20f4-121">Aby hermetyzować te funkcje, utworzymy kolejną klasę View-model, która reprezentuje produkt.</span><span class="sxs-lookup"><span data-stu-id="a20f4-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="a20f4-122">Dodaj następujący kod do pliku `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="a20f4-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="a20f4-123">Klasa `ProductViewModel` zawiera dwie funkcje, które są używane do przenoszenia produktu do i z koszyka: `addItemToCart` dodaje jedną jednostkę produktu do koszyka, a `removeAllFromCart` usuwa wszystkie ilości produktu.</span><span class="sxs-lookup"><span data-stu-id="a20f4-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="a20f4-124">Użytkownicy mogą wybrać istniejące zamówienie i uzyskać szczegółowe informacje o zamówieniu.</span><span class="sxs-lookup"><span data-stu-id="a20f4-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="a20f4-125">Ta funkcja zostanie hermetyzowana do innego widoku — model:</span><span class="sxs-lookup"><span data-stu-id="a20f4-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="a20f4-126">`OrderDetailsViewModel` jest inicjowana z kolejnością i pobiera szczegóły zamówienia przez wysłanie żądania AJAX do serwera.</span><span class="sxs-lookup"><span data-stu-id="a20f4-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="a20f4-127">Zauważ również, że właściwość `total` w `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="a20f4-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="a20f4-128">Ta właściwość jest specjalnym rodzajem dostrzegalnych elementów o nazwie [obliczone zauważalne](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="a20f4-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="a20f4-129">Jak nazywa się, obliczone zauważalne umożliwia powiązanie danych z wartością&#8212;obliczaną w tym przypadku, łączny koszt zamówienia.</span><span class="sxs-lookup"><span data-stu-id="a20f4-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="a20f4-130">Następnie Dodaj następujące funkcje do `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="a20f4-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="a20f4-131">`resetCart` usuwa wszystkie elementy z koszyka.</span><span class="sxs-lookup"><span data-stu-id="a20f4-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="a20f4-132">`getDetails` pobiera szczegóły dla zamówienia (przez wypchnięcie nowej `OrderDetailsViewModel` na listę `details`).</span><span class="sxs-lookup"><span data-stu-id="a20f4-132">`getDetails` gets the details for an order (by pushing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="a20f4-133">`createOrder` tworzy nowe zamówienie i opróżnia koszyk.</span><span class="sxs-lookup"><span data-stu-id="a20f4-133">`createOrder` creates a new order and empties the cart.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="a20f4-134">Na koniec zainicjuj model widoku, wykonując żądania AJAX dotyczące produktów i zamówień:</span><span class="sxs-lookup"><span data-stu-id="a20f4-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="a20f4-135">Jest to bardzo dużo kodu, ale tworzymymy ją krok po kroku, więc miejmy nadzieję projekt jest przejrzysty.</span><span class="sxs-lookup"><span data-stu-id="a20f4-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="a20f4-136">Teraz możemy dodać do kodu HTML pewne powiązania odcinania. js.</span><span class="sxs-lookup"><span data-stu-id="a20f4-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="a20f4-137">**Produkty**</span><span class="sxs-lookup"><span data-stu-id="a20f4-137">**Products**</span></span>

<span data-ttu-id="a20f4-138">Oto powiązania z listą produktów:</span><span class="sxs-lookup"><span data-stu-id="a20f4-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="a20f4-139">To iteruje tablicę produkty i wyświetla nazwę i cenę.</span><span class="sxs-lookup"><span data-stu-id="a20f4-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="a20f4-140">Przycisk "Dodaj do zamówienia" jest widoczny tylko wtedy, gdy użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="a20f4-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="a20f4-141">Przycisk "Dodaj do zamówienia" wywołuje `addItemToCart` w wystąpieniu `ProductViewModel` dla produktu.</span><span class="sxs-lookup"><span data-stu-id="a20f4-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="a20f4-142">Przedstawia to całkiem funkcję odcinania. js: gdy model widoku zawiera inne modele widoku, można zastosować powiązania z modelem wewnętrznym.</span><span class="sxs-lookup"><span data-stu-id="a20f4-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="a20f4-143">W tym przykładzie powiązania w `foreach` są stosowane do każdego wystąpienia `ProductViewModel`.</span><span class="sxs-lookup"><span data-stu-id="a20f4-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="a20f4-144">To podejście jest znacznie bardziej przejrzyste niż umieszczenie wszystkich funkcji w jednym modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="a20f4-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="a20f4-145">**Koszyk**</span><span class="sxs-lookup"><span data-stu-id="a20f4-145">**Cart**</span></span>

<span data-ttu-id="a20f4-146">Oto powiązania dla koszyka:</span><span class="sxs-lookup"><span data-stu-id="a20f4-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="a20f4-147">To iteruje tablicę koszyka i wyświetla nazwę, cenę i liczbę.</span><span class="sxs-lookup"><span data-stu-id="a20f4-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="a20f4-148">Zwróć uwagę, że łącze "Usuń" i przycisk "Utwórz zamówienie" są powiązane z funkcjami widoku-model.</span><span class="sxs-lookup"><span data-stu-id="a20f4-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="a20f4-149">**Zamówienie**</span><span class="sxs-lookup"><span data-stu-id="a20f4-149">**Orders**</span></span>

<span data-ttu-id="a20f4-150">Oto powiązania z listą Orders (zamówienia):</span><span class="sxs-lookup"><span data-stu-id="a20f4-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="a20f4-151">Powoduje to iterację zamówień i pokazuje identyfikator zamówienia.</span><span class="sxs-lookup"><span data-stu-id="a20f4-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="a20f4-152">Zdarzenie kliknięcia w łączu jest powiązane z funkcją `getDetails`.</span><span class="sxs-lookup"><span data-stu-id="a20f4-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="a20f4-153">**Szczegóły zamówienia**</span><span class="sxs-lookup"><span data-stu-id="a20f4-153">**Order Details**</span></span>

<span data-ttu-id="a20f4-154">Oto powiązania dla szczegółów zamówienia:</span><span class="sxs-lookup"><span data-stu-id="a20f4-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="a20f4-155">To iteruje elementy w kolejności i wyświetla produkt, cenę i ilość.</span><span class="sxs-lookup"><span data-stu-id="a20f4-155">This iterates over the items in the order and displays the product, price, and quantity.</span></span> <span data-ttu-id="a20f4-156">Otaczający blok DIV jest widoczny tylko wtedy, gdy tablica szczegółów zawiera jeden lub więcej elementów.</span><span class="sxs-lookup"><span data-stu-id="a20f4-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a20f4-157">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a20f4-157">Conclusion</span></span>

<span data-ttu-id="a20f4-158">W tym samouczku utworzysz aplikację, która używa Entity Framework do komunikowania się z bazą danych, i ASP.NET interfejs API sieci Web, aby zapewnić publiczny interfejs na podstawie warstwy danych.</span><span class="sxs-lookup"><span data-stu-id="a20f4-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="a20f4-159">Korzystamy z ASP.NET MVC 4 do renderowania stron HTML oraz odcinania. js Plus jQuery, aby zapewnić dynamiczne interakcje bez ponownego ładowania stron.</span><span class="sxs-lookup"><span data-stu-id="a20f4-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="a20f4-160">Dodatkowe zasoby:</span><span class="sxs-lookup"><span data-stu-id="a20f4-160">Additional resources:</span></span>

- [<span data-ttu-id="a20f4-161">Mapa zawartości dostępu do danych ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a20f4-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="a20f4-162">Centrum deweloperów Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a20f4-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="a20f4-163">Wstecz</span><span class="sxs-lookup"><span data-stu-id="a20f4-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
