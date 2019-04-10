---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: Część 7. Tworzenie głównej strony | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 028631f8855e4d94bebb0e965de75c4025e22859
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59409268"
---
# <a name="part-7-creating-the-main-page"></a><span data-ttu-id="b7aff-102">Część 7. Tworzenie strony głównej</span><span class="sxs-lookup"><span data-stu-id="b7aff-102">Part 7: Creating the Main Page</span></span>

<span data-ttu-id="b7aff-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b7aff-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b7aff-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="b7aff-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="b7aff-105">Tworzenie strony głównej</span><span class="sxs-lookup"><span data-stu-id="b7aff-105">Creating the Main Page</span></span>

<span data-ttu-id="b7aff-106">W tej sekcji utworzysz strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b7aff-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="b7aff-107">Ta strona będzie bardziej skomplikowane niż strony administratora, co firma Microsoft będzie podejście w kilku krokach.</span><span class="sxs-lookup"><span data-stu-id="b7aff-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="b7aff-108">Po drodze zobaczysz niektóre bardziej zaawansowane techniki struktura Knockout.js.</span><span class="sxs-lookup"><span data-stu-id="b7aff-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="b7aff-109">Poniżej przedstawiono podstawowy układ strony:</span><span class="sxs-lookup"><span data-stu-id="b7aff-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="b7aff-110">"Produkty" zawiera szereg produktów.</span><span class="sxs-lookup"><span data-stu-id="b7aff-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="b7aff-111">"Koszyka" przechowywać tablicę produkty z ilości.</span><span class="sxs-lookup"><span data-stu-id="b7aff-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="b7aff-112">Klikając pozycję "Dodaj do koszyka" aktualizuje koszyka.</span><span class="sxs-lookup"><span data-stu-id="b7aff-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="b7aff-113">"Orders" zawiera tablicę identyfikatorów zamówień.</span><span class="sxs-lookup"><span data-stu-id="b7aff-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="b7aff-114">"Szczegóły" zawiera szczegóły zamówień, który jest tablicą elementów (produkty z ilościami)</span><span class="sxs-lookup"><span data-stu-id="b7aff-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="b7aff-115">Zaczniemy, definiując niektóre podstawowy układ w języku HTML, bez powiązania danych lub skryptu.</span><span class="sxs-lookup"><span data-stu-id="b7aff-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="b7aff-116">Otwórz plik Views/Home/Index.cshtml i Zastąp całą zawartość następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b7aff-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="b7aff-117">Następnie dodaj sekcję skrypty i utworzyć pusty model widoku:</span><span class="sxs-lookup"><span data-stu-id="b7aff-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="b7aff-118">Na podstawie projektu rysowane wcześniej, nasz model view potrzebuje dostrzegalne elementy produktów, koszyk, zamówień i szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="b7aff-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="b7aff-119">Dodaj następujące zmienne `AppViewModel` obiektu:</span><span class="sxs-lookup"><span data-stu-id="b7aff-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="b7aff-120">Użytkownicy mogą dodać elementy z listy produktów do koszyka i usuwanie elementów z koszyka.</span><span class="sxs-lookup"><span data-stu-id="b7aff-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="b7aff-121">Do hermetyzacji tych funkcji, utworzymy innej klasy model widoku, który reprezentuje produkt.</span><span class="sxs-lookup"><span data-stu-id="b7aff-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="b7aff-122">Dodaj następujący kod do `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="b7aff-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="b7aff-123">`ProductViewModel` Klasa zawiera dwie funkcje, które są używane do przenoszenia produktu do i z koszyka: `addItemToCart` dodaje jedną jednostkę produktu do koszyka, a `removeAllFromCart` spowoduje usunięcie wszystkich ilości produktu.</span><span class="sxs-lookup"><span data-stu-id="b7aff-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="b7aff-124">Użytkownicy mogą wybrać istniejące zamówienie i Pobierz szczegóły zamówienia.</span><span class="sxs-lookup"><span data-stu-id="b7aff-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="b7aff-125">Umieściliśmy tej funkcji do innego modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="b7aff-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="b7aff-126">`OrderDetailsViewModel` Jest inicjowany za pomocą zamówienia, a jej pobiera szczegóły zamówienia, wysyłając żądanie AJAX do serwera.</span><span class="sxs-lookup"><span data-stu-id="b7aff-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="b7aff-127">Zauważ również, `total` właściwość `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="b7aff-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="b7aff-128">Ta właściwość jest specjalnym rodzajem obserwowalnymi o nazwie [obliczane możliwość obserwowania](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="b7aff-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="b7aff-129">Jak wskazuje nazwa, obliczana możliwość obserwowania umożliwia powiązanie danych z obliczoną wartością&#8212;w tym przypadku całkowity koszt zamówienia.</span><span class="sxs-lookup"><span data-stu-id="b7aff-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="b7aff-130">Następnie dodaj te funkcje, aby `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="b7aff-130">Next, add these functions to `AppViewModel`:</span></span>

- `resetCart` <span data-ttu-id="b7aff-131">Usuwa wszystkie elementy z koszyka.</span><span class="sxs-lookup"><span data-stu-id="b7aff-131">removes all items from the cart.</span></span>
- `getDetails` <span data-ttu-id="b7aff-132">pobiera szczegóły zamówienia (wypychając nową `OrderDetailsViewModel` na `details` listy).</span><span class="sxs-lookup"><span data-stu-id="b7aff-132">gets the details for an order (by pushing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- `createOrder` <span data-ttu-id="b7aff-133">Tworzy nowe zamówienie i usuwa zawartość koszyka.</span><span class="sxs-lookup"><span data-stu-id="b7aff-133">creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="b7aff-134">Na koniec można zainicjować modelu widoku, wprowadzając wysyłanie żądań AJAX dla produktów i zamówień:</span><span class="sxs-lookup"><span data-stu-id="b7aff-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="b7aff-135">Dobrze, to duża ilość kodu, ale dostosowaliśmy ją się krok po kroku, więc miejmy nadzieję projektu jest wyczyszczone.</span><span class="sxs-lookup"><span data-stu-id="b7aff-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="b7aff-136">Teraz możemy dodać niektóre wiązania struktura Knockout.js w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="b7aff-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

**<span data-ttu-id="b7aff-137">Produkty</span><span class="sxs-lookup"><span data-stu-id="b7aff-137">Products</span></span>**

<span data-ttu-id="b7aff-138">Poniżej przedstawiono powiązania dla listy produktów:</span><span class="sxs-lookup"><span data-stu-id="b7aff-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="b7aff-139">Wykonuje iterację przez tablicę produktów i wyświetla nazwę i ceny.</span><span class="sxs-lookup"><span data-stu-id="b7aff-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="b7aff-140">Przycisk "Dodaj do Order" jest widoczny tylko wtedy, gdy użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="b7aff-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="b7aff-141">Wywołuje przycisk "Dodaj do Order" `addItemToCart` na `ProductViewModel` wystąpienia produktu.</span><span class="sxs-lookup"><span data-stu-id="b7aff-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="b7aff-142">W tym przykładzie pokazano, jak funkcja dobre rozwiązanie z użyciem Knockout.js: Jeśli model widoku zawiera innych modeli widoków, można zastosować powiązania do wewnętrznego modelu.</span><span class="sxs-lookup"><span data-stu-id="b7aff-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="b7aff-143">W tym przykładzie powiązania w ramach `foreach` są stosowane do wszystkich `ProductViewModel` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="b7aff-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="b7aff-144">To podejście jest znacznie bardziej przejrzyste niż umieszczenie wszystkich funkcji w pojedynczy model widoku.</span><span class="sxs-lookup"><span data-stu-id="b7aff-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

**<span data-ttu-id="b7aff-145">Koszyk</span><span class="sxs-lookup"><span data-stu-id="b7aff-145">Cart</span></span>**

<span data-ttu-id="b7aff-146">Poniżej przedstawiono wiązania koszyka:</span><span class="sxs-lookup"><span data-stu-id="b7aff-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="b7aff-147">Wykonuje iterację przez tablicę koszyka i wyświetla nazwę, ceny i ilości.</span><span class="sxs-lookup"><span data-stu-id="b7aff-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="b7aff-148">Należy pamiętać o tym, czy link "Usuń" oraz przycisk "Utwórz zamówienie" są powiązane z funkcji model widoku.</span><span class="sxs-lookup"><span data-stu-id="b7aff-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

**<span data-ttu-id="b7aff-149">Zamówienia</span><span class="sxs-lookup"><span data-stu-id="b7aff-149">Orders</span></span>**

<span data-ttu-id="b7aff-150">Poniżej przedstawiono powiązania dla listy zamówień:</span><span class="sxs-lookup"><span data-stu-id="b7aff-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="b7aff-151">To wykonuje iterację na zamówień i zawiera identyfikator zamówienia.</span><span class="sxs-lookup"><span data-stu-id="b7aff-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="b7aff-152">Zdarzenie kliknięcia łącze jest powiązany z `getDetails` funkcji.</span><span class="sxs-lookup"><span data-stu-id="b7aff-152">The click event on the link is bound to the `getDetails` function.</span></span>

**<span data-ttu-id="b7aff-153">Szczegóły zamówienia</span><span class="sxs-lookup"><span data-stu-id="b7aff-153">Order Details</span></span>**

<span data-ttu-id="b7aff-154">Poniżej przedstawiono wiązania szczegóły zamówienia:</span><span class="sxs-lookup"><span data-stu-id="b7aff-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="b7aff-155">Iteruje przez elementy w kolejności i wyświetla produktu, ceny i ilości.</span><span class="sxs-lookup"><span data-stu-id="b7aff-155">This iterates over the items in the order and displays the product, price, and quantity.</span></span> <span data-ttu-id="b7aff-156">Div otaczającego jest widoczne tylko wtedy, gdy tablica szczegółowe informacje zawiera jeden lub więcej elementów.</span><span class="sxs-lookup"><span data-stu-id="b7aff-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b7aff-157">Wniosek</span><span class="sxs-lookup"><span data-stu-id="b7aff-157">Conclusion</span></span>

<span data-ttu-id="b7aff-158">W tym samouczku utworzono aplikację, która korzysta z programu Entity Framework do komunikacji z bazą danych i Web API platformy ASP.NET, aby udostępnić interfejs publicznie dostępnych na podstawie warstwy danych.</span><span class="sxs-lookup"><span data-stu-id="b7aff-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="b7aff-159">ASP.NET MVC 4 są używane do renderowania stron HTML i struktura Knockout.js oraz jQuery zapewniają dynamiczne interakcje bez ładuje ponownie stronę.</span><span class="sxs-lookup"><span data-stu-id="b7aff-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="b7aff-160">Dodatkowe zasoby:</span><span class="sxs-lookup"><span data-stu-id="b7aff-160">Additional resources:</span></span>

- [<span data-ttu-id="b7aff-161">Mapa zawartości dostępu do danych w programie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b7aff-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="b7aff-162">Centrum deweloperów programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b7aff-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="b7aff-163">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="b7aff-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
