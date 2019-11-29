---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Część 5: Tworzenie dynamicznego interfejsu użytkownika z odcinaniem. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599995"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="2bd87-102">Część 5. Tworzenie dynamicznego interfejsu użytkownika przy użyciu narzędzia separowania. js</span><span class="sxs-lookup"><span data-stu-id="2bd87-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="2bd87-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2bd87-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2bd87-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="2bd87-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="2bd87-105">Tworzenie dynamicznego interfejsu użytkownika z użyciem biblioteki Knockout.js</span><span class="sxs-lookup"><span data-stu-id="2bd87-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="2bd87-106">W tej sekcji będziemy używać odcinania. js do dodawania funkcji do widoku administratora.</span><span class="sxs-lookup"><span data-stu-id="2bd87-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="2bd87-107">[Separowanie. js](http://knockoutjs.com/) to biblioteka języka JavaScript, która ułatwia powiązanie formantów HTML z danymi.</span><span class="sxs-lookup"><span data-stu-id="2bd87-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="2bd87-108">Separowanie. js używa wzorca Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="2bd87-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="2bd87-109">*Model* jest reprezentacją po stronie serwera danych w domenie biznesowej (w naszym przypadku, produkty i zamówienia).</span><span class="sxs-lookup"><span data-stu-id="2bd87-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="2bd87-110">*Widok* jest warstwą prezentacji (html).</span><span class="sxs-lookup"><span data-stu-id="2bd87-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="2bd87-111">*Model widoku* to obiekt JavaScript, który zawiera dane modelu.</span><span class="sxs-lookup"><span data-stu-id="2bd87-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="2bd87-112">Model widoku jest abstrakcją kodu interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2bd87-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="2bd87-113">Nie ma informacji o reprezentacji HTML.</span><span class="sxs-lookup"><span data-stu-id="2bd87-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="2bd87-114">Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak "Lista elementów".</span><span class="sxs-lookup"><span data-stu-id="2bd87-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="2bd87-115">Widok jest powiązany z danymi z modelem widoku.</span><span class="sxs-lookup"><span data-stu-id="2bd87-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="2bd87-116">Aktualizacje widoku-model są automatycznie odzwierciedlane w widoku.</span><span class="sxs-lookup"><span data-stu-id="2bd87-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="2bd87-117">Model widoku również pobiera zdarzenia z widoku, takie jak kliknięcie przycisku i wykonuje operacje na modelu, takie jak tworzenie zamówienia.</span><span class="sxs-lookup"><span data-stu-id="2bd87-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="2bd87-118">Najpierw zdefiniujemy model widoku.</span><span class="sxs-lookup"><span data-stu-id="2bd87-118">First we'll define the view-model.</span></span> <span data-ttu-id="2bd87-119">Następnie powiążemy znacznik HTML z modelem widoku.</span><span class="sxs-lookup"><span data-stu-id="2bd87-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="2bd87-120">Dodaj następującą sekcję Razor do admin. cshtml:</span><span class="sxs-lookup"><span data-stu-id="2bd87-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="2bd87-121">Tę sekcję można dodać w dowolnym miejscu w pliku.</span><span class="sxs-lookup"><span data-stu-id="2bd87-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="2bd87-122">Gdy widok jest renderowany, sekcja pojawia się u dołu strony HTML bezpośrednio przed tagiem zamykającym &lt;/Body&gt;.</span><span class="sxs-lookup"><span data-stu-id="2bd87-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="2bd87-123">Cały skrypt dla tej strony zostanie umieszczony wewnątrz tagu skryptu wskazywanym przez komentarz:</span><span class="sxs-lookup"><span data-stu-id="2bd87-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="2bd87-124">Najpierw Zdefiniuj klasę widoku-model:</span><span class="sxs-lookup"><span data-stu-id="2bd87-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="2bd87-125">**ko. observableArray** jest specjalnym rodzajem obiektu w odróżnieniu, nazywanym *zauważalnością*.</span><span class="sxs-lookup"><span data-stu-id="2bd87-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="2bd87-126">Z [dokumentacji odcinania. js](http://knockoutjs.com/documentation/observables.html): zauważalny jest "obiekt JavaScript, który może powiadamiać subskrybentów o zmianach".</span><span class="sxs-lookup"><span data-stu-id="2bd87-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="2bd87-127">Gdy zawartość zauważalnej zmiany, widok zostanie automatycznie zaktualizowany do dopasowania.</span><span class="sxs-lookup"><span data-stu-id="2bd87-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="2bd87-128">Aby wypełnić tablicę `products`, Utwórz żądanie AJAX do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="2bd87-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="2bd87-129">Odwołaj, że został zapisany podstawowy identyfikator URI dla interfejsu API w worku widoku (zobacz [część 4](using-web-api-with-entity-framework-part-4.md) samouczka).</span><span class="sxs-lookup"><span data-stu-id="2bd87-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="2bd87-130">Następnie Dodaj funkcje do modelu widoku, aby tworzyć, aktualizować i usuwać produkty.</span><span class="sxs-lookup"><span data-stu-id="2bd87-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="2bd87-131">Te funkcje przesyłają wywołania AJAX do internetowego interfejsu API i używają wyników do aktualizowania modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="2bd87-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="2bd87-132">Teraz najważniejszym elementem: gdy DOM jest zapełniony, wywołaj funkcję **ko. applyBindings** i przekaż nowe wystąpienie `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="2bd87-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="2bd87-133">Metoda **ko. applyBindings** umożliwia aktywowanie odcinania i nacinanie modelu widoku do widoku.</span><span class="sxs-lookup"><span data-stu-id="2bd87-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="2bd87-134">Teraz, gdy mamy już model widoku, możemy utworzyć powiązania.</span><span class="sxs-lookup"><span data-stu-id="2bd87-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="2bd87-135">W pliku separowania. js można to zrobić, dodając atrybuty `data-bind` do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="2bd87-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="2bd87-136">Na przykład, aby powiązać listę HTML z tablicą, użyj powiązania `foreach`:</span><span class="sxs-lookup"><span data-stu-id="2bd87-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="2bd87-137">Powiązanie `foreach` wykonuje iterację przez tablicę i tworzy elementy podrzędne dla każdego obiektu w tablicy.</span><span class="sxs-lookup"><span data-stu-id="2bd87-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="2bd87-138">Powiązania elementów podrzędnych mogą odwoływać się do właściwości obiektów array.</span><span class="sxs-lookup"><span data-stu-id="2bd87-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="2bd87-139">Dodaj następujące powiązania do listy "aktualizacje produktów":</span><span class="sxs-lookup"><span data-stu-id="2bd87-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="2bd87-140">Element `<li>` występuje w zakresie powiązania **foreach** .</span><span class="sxs-lookup"><span data-stu-id="2bd87-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="2bd87-141">Oznacza to, że odcinanie będzie renderować element jeden raz dla każdego produktu w tablicy `products`.</span><span class="sxs-lookup"><span data-stu-id="2bd87-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="2bd87-142">Wszystkie powiązania w ramach elementu `<li>` odwołują się do tego wystąpienia produktu.</span><span class="sxs-lookup"><span data-stu-id="2bd87-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="2bd87-143">Na przykład `$data.Name` odwołuje się do właściwości `Name` produktu.</span><span class="sxs-lookup"><span data-stu-id="2bd87-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="2bd87-144">Aby ustawić wartości danych wejściowych tekstu, użyj powiązania `value`.</span><span class="sxs-lookup"><span data-stu-id="2bd87-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="2bd87-145">Przyciski są powiązane z funkcjami w widoku model, przy użyciu powiązania `click`.</span><span class="sxs-lookup"><span data-stu-id="2bd87-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="2bd87-146">Wystąpienie produktu jest przesyłane jako parametr do każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="2bd87-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="2bd87-147">Aby uzyskać więcej informacji, [Dokumentacja dotycząca odcinania. js](http://knockoutjs.com/documentation/observables.html) ma dobre opisy różnych powiązań.</span><span class="sxs-lookup"><span data-stu-id="2bd87-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="2bd87-148">Następnie Dodaj powiązanie dla zdarzenia **Prześlij** na formularzu Dodaj produkt:</span><span class="sxs-lookup"><span data-stu-id="2bd87-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="2bd87-149">To powiązanie wywołuje funkcję `create` w modelu widoku, aby utworzyć nowy produkt.</span><span class="sxs-lookup"><span data-stu-id="2bd87-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="2bd87-150">Oto pełen kod widoku administratora:</span><span class="sxs-lookup"><span data-stu-id="2bd87-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="2bd87-151">Uruchom aplikację, zaloguj się przy użyciu konta administratora, a następnie kliknij link "Administrator".</span><span class="sxs-lookup"><span data-stu-id="2bd87-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="2bd87-152">Powinna zostać wyświetlona lista produktów i będzie można tworzyć, aktualizować lub usuwać produkty.</span><span class="sxs-lookup"><span data-stu-id="2bd87-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2bd87-153">[Poprzednie](using-web-api-with-entity-framework-part-4.md)
> [dalej](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="2bd87-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
