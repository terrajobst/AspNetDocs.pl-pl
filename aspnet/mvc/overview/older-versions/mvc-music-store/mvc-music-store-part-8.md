---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Część 8: koszyk z aktualizacjami AJAX | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 8 obejmuje koszyk z aktualizacjami AJAX.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539257"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="a8a68-104">Część 8. koszyk z aktualizacjami AJAX</span><span class="sxs-lookup"><span data-stu-id="a8a68-104">Part 8: Shopping Cart with Ajax Updates</span></span>

<span data-ttu-id="a8a68-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a8a68-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a8a68-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a8a68-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a8a68-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="a8a68-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a8a68-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="a8a68-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a8a68-109">Część 8 obejmuje koszyk z aktualizacjami AJAX.</span><span class="sxs-lookup"><span data-stu-id="a8a68-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>

<span data-ttu-id="a8a68-110">Zezwolimy użytkownikom na umieszczenie albumów w ich koszyku bez rejestrowania się, ale konieczne będzie zarejestrowanie się jako gościa w celu ukończenia wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="a8a68-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="a8a68-111">Proces zakupów i wyewidencjonowywania zostanie podzielony na dwa kontrolery: kontroler ShoppingCart, który umożliwia anonimowe Dodawanie elementów do koszyka, oraz kontroler wyewidencjonowania obsługujący proces wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="a8a68-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="a8a68-112">Zaczniemy od koszyka w tej sekcji, a następnie kompilować proces wyewidencjonowania w poniższej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a8a68-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="a8a68-113">Dodawanie klas modeli koszyk, zamówienie i OrderDetail</span><span class="sxs-lookup"><span data-stu-id="a8a68-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="a8a68-114">Nasze koszyki i procesy wyewidencjonowywania będą korzystać z nowych klas.</span><span class="sxs-lookup"><span data-stu-id="a8a68-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="a8a68-115">Kliknij prawym przyciskiem myszy folder modele i Dodaj klasę koszyka (Cart.cs) z poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="a8a68-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="a8a68-116">Ta klasa jest całkiem podobna do innych, z których korzystamy dotąd, z wyjątkiem atrybutu [Key] dla właściwości RecordId.</span><span class="sxs-lookup"><span data-stu-id="a8a68-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="a8a68-117">Nasze elementy koszyka będą miały identyfikator ciągu o nazwie CartID, aby umożliwić anonimowe zakupy, ale tabela zawiera klucz podstawowy Integer o nazwie RecordId.</span><span class="sxs-lookup"><span data-stu-id="a8a68-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="a8a68-118">Zgodnie z Konwencją, Entity Framework kod — najpierw oczekuje, że klucz podstawowy dla tabeli o nazwie Koszyk będzie miał wartość CartId lub identyfikator, ale możemy łatwo przesłonić ten element za pośrednictwem adnotacji lub kodu, jeśli chcemy.</span><span class="sxs-lookup"><span data-stu-id="a8a68-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="a8a68-119">Jest to przykład, w jaki sposób możemy używać prostych Konwencji w Entity Framework kodzie — najpierw w przypadku, gdy są one zgodne, ale nie są ograniczone przez nie.</span><span class="sxs-lookup"><span data-stu-id="a8a68-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="a8a68-120">Następnie Dodaj klasę Order (Order.cs) z poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="a8a68-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="a8a68-121">Ta klasa śledzi podsumowanie i informacje o dostarczaniu dla zamówienia.</span><span class="sxs-lookup"><span data-stu-id="a8a68-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="a8a68-122">**Nie będzie jeszcze kompilować**, ponieważ ma właściwość nawigacji OrderDetails, która zależy od klasy, która nie została jeszcze utworzona.</span><span class="sxs-lookup"><span data-stu-id="a8a68-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="a8a68-123">Naprawmy, że teraz dodając klasę o nazwie OrderDetail.cs, dodając następujący kod.</span><span class="sxs-lookup"><span data-stu-id="a8a68-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="a8a68-124">Wprowadzimy jedną ostatnią aktualizację do naszej klasy MusicStoreEntities, która będzie zawierać zestawy dbsets, które uwidaczniają te nowe klasy modelu, łącznie z&gt;wykonawcą&lt;Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="a8a68-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="a8a68-125">Zaktualizowana Klasa MusicStoreEntities zostanie wyświetlona poniżej.</span><span class="sxs-lookup"><span data-stu-id="a8a68-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="a8a68-126">Zarządzanie logiką biznesową koszyka zakupów</span><span class="sxs-lookup"><span data-stu-id="a8a68-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="a8a68-127">Następnie utworzymy klasę ShoppingCart w folderze models.</span><span class="sxs-lookup"><span data-stu-id="a8a68-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="a8a68-128">Model ShoppingCart obsługuje dostęp do danych do tabeli koszyka.</span><span class="sxs-lookup"><span data-stu-id="a8a68-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="a8a68-129">Ponadto będzie obsługiwał logikę biznesową w celu dodawania i usuwania elementów z koszyka.</span><span class="sxs-lookup"><span data-stu-id="a8a68-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="a8a68-130">Ponieważ nie chcemy wymagać od użytkowników rejestrowania się w celu dodania elementów do koszyka zakupów, przypiszemy użytkownikom tymczasowy unikatowy identyfikator (przy użyciu identyfikatora GUID lub unikatowego identyfikatora globalnego) podczas uzyskiwania dostępu do koszyka.</span><span class="sxs-lookup"><span data-stu-id="a8a68-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="a8a68-131">Ten identyfikator zostanie zapisany przy użyciu klasy sesji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a8a68-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="a8a68-132">*Uwaga: sesja ASP.NET jest wygodnym miejscem do przechowywania informacji specyficznych dla użytkownika, które wygasną po opuszczeniu witryny. W przypadku nieprawidłowego stanu sesji może to mieć wpływ na wydajność większych lokacji. nasze jasne użycie będzie dobrze działało do celów demonstracyjnych.*</span><span class="sxs-lookup"><span data-stu-id="a8a68-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="a8a68-133">Klasa ShoppingCart uwidacznia następujące metody:</span><span class="sxs-lookup"><span data-stu-id="a8a68-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="a8a68-134">**AddToCart** pobiera album jako parametr i dodaje go do koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a8a68-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="a8a68-135">Ponieważ tabela koszyka śledzi liczbę dla każdego albumu, zawiera logikę umożliwiającą utworzenie nowego wiersza w razie potrzeby lub po prostu zwiększenie ilości, jeśli użytkownik zamówił już jedną kopię albumu.</span><span class="sxs-lookup"><span data-stu-id="a8a68-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="a8a68-136">**RemoveFromCart** Pobiera identyfikator albumu i usuwa go z koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a8a68-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="a8a68-137">Jeśli użytkownik ma tylko jedną kopię albumu w swoim koszyku, wiersz zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="a8a68-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="a8a68-138">**EmptyCart** usuwa wszystkie elementy z koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a8a68-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="a8a68-139">**GetCartItems** pobiera listę CartItems do wyświetlania lub przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="a8a68-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="a8a68-140">**GetCount** pobiera łączną liczbę albumów, które użytkownik ma w swoim koszyku.</span><span class="sxs-lookup"><span data-stu-id="a8a68-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="a8a68-141">Wartość **gettotal** oblicza łączny koszt wszystkich elementów w koszyku.</span><span class="sxs-lookup"><span data-stu-id="a8a68-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="a8a68-142">**Porządek** jest konwertowany na zamówienie w ramach fazy wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="a8a68-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="a8a68-143">**Getkoszyk** to metoda statyczna, która umożliwia naszym kontrolerom uzyskanie obiektu koszyka.</span><span class="sxs-lookup"><span data-stu-id="a8a68-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="a8a68-144">Używa metody **GetCartId** , aby obsłużyć odczyt CartId z sesji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a8a68-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="a8a68-145">Metoda GetCartId wymaga HttpContextBase, aby mógł odczytać CartId użytkownika z sesji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a8a68-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="a8a68-146">Oto kompletna **Klasa ShoppingCart**:</span><span class="sxs-lookup"><span data-stu-id="a8a68-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="a8a68-147">Modele widoków</span><span class="sxs-lookup"><span data-stu-id="a8a68-147">ViewModels</span></span>

<span data-ttu-id="a8a68-148">Nasz kontroler koszyka zakupów będzie musiał komunikować się ze swoimi widokami, które nie są prawidłowo mapowane do naszych obiektów modelu.</span><span class="sxs-lookup"><span data-stu-id="a8a68-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="a8a68-149">Nie chcemy modyfikować naszych modeli zgodnie z naszymi widokami. Klasy modelu powinny reprezentować naszą domenę, a nie interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a8a68-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="a8a68-150">Jednym z rozwiązań jest przekazanie informacji do naszych widoków przy użyciu klasy ViewBag, podobnie jak w przypadku listy rozwijanej Menedżera sklepu, ale przekazywanie wielu informacji za pośrednictwem ViewBag jest trudne do zarządzania.</span><span class="sxs-lookup"><span data-stu-id="a8a68-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="a8a68-151">Rozwiązaniem do tego jest użycie wzorca *ViewModel* .</span><span class="sxs-lookup"><span data-stu-id="a8a68-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="a8a68-152">W przypadku korzystania z tego wzorca tworzymy klasy o jednoznacznie określonym typie, które są zoptymalizowane pod kątem naszych scenariuszy widoku, i które uwidaczniają właściwości wartości dynamicznych/zawartości wymaganej przez nasze szablony widoków.</span><span class="sxs-lookup"><span data-stu-id="a8a68-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="a8a68-153">Nasze klasy kontrolera mogą następnie wypełnić i przekazać te klasy zoptymalizowane pod kątem widoku do naszego szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="a8a68-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="a8a68-154">Zapewnia to bezpieczeństwo typu, sprawdzanie czasu kompilacji i IntelliSense w edytorze w szablonach widoków.</span><span class="sxs-lookup"><span data-stu-id="a8a68-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="a8a68-155">Utworzymy dwa modele widoku do użycia w naszym kontrolerze koszyka: ShoppingCartViewModel będzie przechowywać zawartość koszyka zakupów użytkownika, a ShoppingCartRemoveViewModel będzie służyć do wyświetlania informacji o potwierdzeniach, gdy użytkownik usunie coś z koszyka.</span><span class="sxs-lookup"><span data-stu-id="a8a68-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="a8a68-156">Utwórzmy nowy folder modele widoków w katalogu głównym naszego projektu, aby zachować zorganizowane elementy.</span><span class="sxs-lookup"><span data-stu-id="a8a68-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="a8a68-157">Kliknij prawym przyciskiem myszy projekt, wybierz polecenie Dodaj/nowy folder.</span><span class="sxs-lookup"><span data-stu-id="a8a68-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="a8a68-158">Nazwij folder modele widoków.</span><span class="sxs-lookup"><span data-stu-id="a8a68-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="a8a68-159">Następnie Dodaj klasę ShoppingCartViewModel w folderze modele widoków.</span><span class="sxs-lookup"><span data-stu-id="a8a68-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="a8a68-160">Ma dwie właściwości: listę elementów koszyka oraz wartość dziesiętną, aby pomieścić całkowitą cenę dla wszystkich elementów w koszyku.</span><span class="sxs-lookup"><span data-stu-id="a8a68-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="a8a68-161">Teraz Dodaj ShoppingCartRemoveViewModel do folderu modele widoków, używając następujących czterech właściwości.</span><span class="sxs-lookup"><span data-stu-id="a8a68-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="a8a68-162">Kontroler koszyka zakupów</span><span class="sxs-lookup"><span data-stu-id="a8a68-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="a8a68-163">Kontroler koszyka zakupów ma trzy główne cele: Dodawanie elementów do koszyka, usuwanie elementów z koszyka i wyświetlanie elementów w koszyku.</span><span class="sxs-lookup"><span data-stu-id="a8a68-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="a8a68-164">Spowoduje to użycie trzech klas, które właśnie utworzyliśmy: ShoppingCartViewModel, ShoppingCartRemoveViewModel i ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="a8a68-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="a8a68-165">Podobnie jak w przypadku StoreController i StoreManagerController, dodamy pole do przechowywania wystąpienia MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="a8a68-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="a8a68-166">Dodaj nowy kontroler koszyka zakupów do projektu przy użyciu szablonu pustego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a8a68-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="a8a68-167">Oto kompletny kontroler ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="a8a68-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="a8a68-168">Akcje indeks i Dodaj kontroler powinny wyglądać bardzo dobrze.</span><span class="sxs-lookup"><span data-stu-id="a8a68-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="a8a68-169">Operacje kontrolera Remove i CartSummary obsługują dwa specjalne przypadki, które omówiono w poniższej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a8a68-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="a8a68-170">Aktualizacje AJAX z użyciem jQuery</span><span class="sxs-lookup"><span data-stu-id="a8a68-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="a8a68-171">Będziemy dalej tworzyć stronę indeksu koszyka zakupów, która jest silnie wpisana do ShoppingCartViewModel i używa szablonu widoku listy przy użyciu tej samej metody jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="a8a68-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="a8a68-172">Jednak zamiast używania pliku HTML. ActionLink do usuwania elementów z koszyka będziemy używać platformy jQuery do "podłączania" dla wszystkich linków w tym widoku, które mają klasę HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="a8a68-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="a8a68-173">Zamiast ogłaszać formularz, ten program obsługi zdarzeń kliknij po prostu wywołanie zwrotne AJAX do naszej akcji kontrolera RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="a8a68-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="a8a68-174">RemoveFromCart zwraca wynik Zserializowany w formacie JSON, który następnie jest analizowany przez wywołanie zwrotne jQuery i wykonuje cztery szybkie aktualizacje strony przy użyciu jQuery:</span><span class="sxs-lookup"><span data-stu-id="a8a68-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="a8a68-175">Usuwa z listy usunięty album</span><span class="sxs-lookup"><span data-stu-id="a8a68-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="a8a68-176">Aktualizuje liczbę koszyków w nagłówku</span><span class="sxs-lookup"><span data-stu-id="a8a68-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="a8a68-177">Wyświetla komunikat aktualizacji dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="a8a68-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="a8a68-178">Aktualizuje łączną cenę koszyka</span><span class="sxs-lookup"><span data-stu-id="a8a68-178">Updates the cart total price</span></span>

<span data-ttu-id="a8a68-179">Ze względu na to, że scenariusz usuwania jest obsługiwany przez wywołanie zwrotne AJAX w widoku indeksu, nie jest potrzebny dodatkowy widok dla akcji RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="a8a68-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="a8a68-180">Oto kompletny kod widoku/ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="a8a68-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="a8a68-181">Aby przetestować ten limit, musimy móc dodać elementy do naszego koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="a8a68-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="a8a68-182">Zaktualizujemy nasz widok **szczegółów sklepu** , aby uwzględnić przycisk "Dodaj do koszyka".</span><span class="sxs-lookup"><span data-stu-id="a8a68-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="a8a68-183">Na bieżąco możemy uwzględnić niektóre informacje dodatkowe dotyczące albumu, które zostały dodane od czasu ostatniej aktualizacji tego widoku: gatunek, wykonawca, Cena i album.</span><span class="sxs-lookup"><span data-stu-id="a8a68-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="a8a68-184">Zostanie wyświetlony zaktualizowany kod widoku szczegółów sklepu, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a8a68-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="a8a68-185">Teraz możemy klikać w sklepie i testować Dodawanie i usuwanie albumów do i z naszego koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="a8a68-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="a8a68-186">Uruchom aplikację i przejdź do indeksu magazynu.</span><span class="sxs-lookup"><span data-stu-id="a8a68-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="a8a68-187">Następnie kliknij gatunek, aby wyświetlić listę albumów.</span><span class="sxs-lookup"><span data-stu-id="a8a68-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="a8a68-188">Kliknięcie tytułu albumu zawiera teraz nasz zaktualizowany widok szczegółów albumu, w tym przycisk "Dodaj do koszyka".</span><span class="sxs-lookup"><span data-stu-id="a8a68-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="a8a68-189">Kliknięcie przycisku "Dodaj do koszyka" pokazuje widok indeksu koszyka zakupów z listą podsumowania koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="a8a68-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="a8a68-190">Po załadowaniu koszyka zakupów możesz kliknąć link Usuń z koszyka, aby zobaczyć aktualizację AJAX do koszyka.</span><span class="sxs-lookup"><span data-stu-id="a8a68-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="a8a68-191">Utworzyliśmy roboczy koszyk, który umożliwia niezarejestrowanim użytkownikom dodawanie elementów do ich koszyka.</span><span class="sxs-lookup"><span data-stu-id="a8a68-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="a8a68-192">W poniższej sekcji zezwolimy im na zarejestrowanie i zakończenie procesu wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="a8a68-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a8a68-193">[Poprzednie](mvc-music-store-part-7.md)
> [dalej](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="a8a68-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
