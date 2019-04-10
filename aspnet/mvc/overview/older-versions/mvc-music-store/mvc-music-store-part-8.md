---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: Część 8. Koszyk z aktualizacjami Ajax | Dokumentacja firmy Microsoft
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 8 obejmuje koszyk z aktualizacjami Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 2ba210d8c541c6c330dda74706470fa73a81474a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59379485"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="ffde6-104">Część 8. Koszyk z aktualizacjami AJAX</span><span class="sxs-lookup"><span data-stu-id="ffde6-104">Part 8: Shopping Cart with Ajax Updates</span></span>

<span data-ttu-id="ffde6-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ffde6-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ffde6-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="ffde6-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ffde6-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="ffde6-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="ffde6-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="ffde6-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ffde6-109">Część 8 obejmuje koszyk z aktualizacjami Ajax.</span><span class="sxs-lookup"><span data-stu-id="ffde6-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="ffde6-110">Firma Microsoft będzie Zezwalaj użytkownikom na umieszczenie albumów w ich koszyka bez rejestrowania, ale należy zarejestrować się jako goście zakończyć proces realizacji transakcji.</span><span class="sxs-lookup"><span data-stu-id="ffde6-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="ffde6-111">Proces zakupów i wyewidencjonowywania będą można podzielić na dwa kontrolery: kontroler ShoppingCart, co pozwala anonimowo Dodawanie elementów do koszyka i kontroler wyewidencjonowania, która obsługuje rozpoczęcie procesu realizowania zamówienia.</span><span class="sxs-lookup"><span data-stu-id="ffde6-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="ffde6-112">Firma Microsoft będzie rozpoczynać koszyka w tej sekcji, a następnie tworzenie rozpoczęcie procesu realizowania zamówienia w poniższej sekcji.</span><span class="sxs-lookup"><span data-stu-id="ffde6-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="ffde6-113">Dodawanie klasy modelu koszyka, kolejność i OrderDetail</span><span class="sxs-lookup"><span data-stu-id="ffde6-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="ffde6-114">Nasze procesów koszyk i wyewidencjonowania spowoduje, że korzystanie z niektórych nowych klas.</span><span class="sxs-lookup"><span data-stu-id="ffde6-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="ffde6-115">Kliknij prawym przyciskiem myszy folderu modeli i dodać klasę koszyka (Cart.cs) z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="ffde6-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="ffde6-116">Ta klasa jest bardzo podobne do innych osób, które była używana do tej pory, z wyjątkiem atrybutu [klucz] dla właściwości identyfikator rekordu.</span><span class="sxs-lookup"><span data-stu-id="ffde6-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="ffde6-117">Elementy naszego koszyka ma identyfikator ciągu o nazwie CartID, aby zezwolić na anonimowy zakupów, ale tabela zawiera klucz podstawowy liczbą całkowitą o nazwie identyfikator rekordu.</span><span class="sxs-lookup"><span data-stu-id="ffde6-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="ffde6-118">Zgodnie z Konwencją najpierw z programem Entity Framework kod oczekuje klucz podstawowy dla tabeli o nazwie koszyka będzie CartId lub Identyfikatora, że firma Microsoft można łatwo zastąpić, za pośrednictwem adnotacje lub kod Jeśli chcemy.</span><span class="sxs-lookup"><span data-stu-id="ffde6-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="ffde6-119">Jest to przykład jak możemy użyć konwencji proste w Entity Framework najpierw kod po ich własnych nam, ale firma Microsoft jest nie ograniczona przez nich Jeśli tak nie jest.</span><span class="sxs-lookup"><span data-stu-id="ffde6-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="ffde6-120">Następnie Dodaj klasę zamówienia (Order.cs) z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="ffde6-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="ffde6-121">Ta klasa śledzi informacje o kolejności Podsumowanie i dostarczania.</span><span class="sxs-lookup"><span data-stu-id="ffde6-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="ffde6-122">**Nie będzie jeszcze skompilować**, ponieważ ma ona właściwość nawigacji OrderDetails, która zależy od klasy, firma Microsoft nie został jeszcze utworzony.</span><span class="sxs-lookup"><span data-stu-id="ffde6-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="ffde6-123">Teraz ustalić, że teraz, dodając klasę o nazwie OrderDetail.cs, dodając następujący kod.</span><span class="sxs-lookup"><span data-stu-id="ffde6-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="ffde6-124">Wybierzemy jedną Ostatnia aktualizacja do klasy naszych MusicStoreEntities w taki sposób, aby uwzględnić DbSets, który udostępnić te nowe klasy modelu dotyczy to również DbSet&lt;wykonawcy&gt;.</span><span class="sxs-lookup"><span data-stu-id="ffde6-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="ffde6-125">Zaktualizowano klasa MusicStoreEntities jest wyświetlany jako poniżej.</span><span class="sxs-lookup"><span data-stu-id="ffde6-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="ffde6-126">Zarządzanie logikę biznesową koszyka</span><span class="sxs-lookup"><span data-stu-id="ffde6-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="ffde6-127">Następnie utworzymy klasy ShoppingCart w folderze modeli.</span><span class="sxs-lookup"><span data-stu-id="ffde6-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="ffde6-128">ShoppingCart model obsługuje dostęp do danych do tabeli koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="ffde6-129">Ponadto obsłuży logikę biznesową w celu dodawania i usuwania elementów z koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="ffde6-130">Ponieważ nie chcemy wymagać od użytkowników założyć konto tak dodać elementy do ich koszyka, przypiszemy użytkowników tymczasowe Unikatowy identyfikator (przy użyciu identyfikatora GUID lub Unikatowy identyfikator globalny) podczas uzyskiwania dostępu do koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="ffde6-131">Przechowujemy będzie ten identyfikator przy użyciu klasy sesji programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ffde6-131">We'll store this ID using the ASP.NET Session class.</span></span>

*<span data-ttu-id="ffde6-132">Uwaga: Sesja programu ASP.NET jest wygodne miejsce do przechowywania informacji o użytkowniku, która wygaśnie po opuszczania witryny.</span><span class="sxs-lookup"><span data-stu-id="ffde6-132">Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site.</span></span> <span data-ttu-id="ffde6-133">Gdy nieuprawnione użycie stanu sesji mogą mieć wpływ na wydajność w witrynach większe, nasze światła użycia będzie działać również w celach demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="ffde6-133">While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.</span></span>*

<span data-ttu-id="ffde6-134">Klasa ShoppingCart udostępnia następujące metody:</span><span class="sxs-lookup"><span data-stu-id="ffde6-134">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="ffde6-135">**AddToCart** albumu jako parametr przyjmuje i dodaje go do koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ffde6-135">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="ffde6-136">Ponieważ w tabeli koszyka śledzi ilości dla każdego albumu, zawiera logikę w celu utworzenia nowego wiersza, jeśli to konieczne lub po prostu zwiększyć ilość, jeśli użytkownik ma już uporządkowane jedną kopię albumu.</span><span class="sxs-lookup"><span data-stu-id="ffde6-136">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="ffde6-137">**RemoveFromCart** przyjmuje identyfikator albumu i usuwa go z koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ffde6-137">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="ffde6-138">Jeśli użytkownik ma tylko jedną kopię album w ich koszyka, wiersza są usuwane.</span><span class="sxs-lookup"><span data-stu-id="ffde6-138">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="ffde6-139">**EmptyCart** spowoduje usunięcie wszystkich elementów z koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ffde6-139">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="ffde6-140">**GetCartItems** umożliwia pobranie listy CartItems w celu wyświetlania lub przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="ffde6-140">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="ffde6-141">**GetCount** pobiera całkowita liczba albumów użytkownika powoduje ich koszyk sklepowy.</span><span class="sxs-lookup"><span data-stu-id="ffde6-141">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="ffde6-142">**GetTotal** oblicza łączny koszt wszystkich elementów w koszyku.</span><span class="sxs-lookup"><span data-stu-id="ffde6-142">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="ffde6-143">**CreateOrder** konwertuje koszyka zamówienia w fazie wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="ffde6-143">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="ffde6-144">**GetCart** jest metoda statyczna, co pozwala naszym kontrolerów, aby uzyskać obiekt koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-144">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="ffde6-145">Używa ona **GetCartId** metody, aby obsłużyć odczyt CartId z sesji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ffde6-145">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="ffde6-146">Metoda GetCartId wymaga element HttpContextBase przeczytasz CartId użytkownika z sesji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ffde6-146">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="ffde6-147">Poniżej przedstawiono pełne **klasy ShoppingCart**:</span><span class="sxs-lookup"><span data-stu-id="ffde6-147">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="ffde6-148">Modele widoków</span><span class="sxs-lookup"><span data-stu-id="ffde6-148">ViewModels</span></span>

<span data-ttu-id="ffde6-149">Nasze kontroler koszyka zakupów należy do komunikowania się niektórych złożone informacje do jego widoków, który nie jest mapowany nie pozostawia żadnych śladów naszych obiekty modelu.</span><span class="sxs-lookup"><span data-stu-id="ffde6-149">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="ffde6-150">Nie chcemy zmodyfikować naszych modeli do potrzeb naszych widoków; Klasy modeli powinny reprezentować naszej domenie nie interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ffde6-150">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="ffde6-151">Jedno rozwiązanie będzie mógł przekazać informacje do naszych widoków przy użyciu klasy obiekt ViewBag postępowanie z informacje listy rozwijanej Menedżera Store, ale przekazywanie dużej ilości informacji za pomocą elementów ViewBag pobiera trudny do zarządzania.</span><span class="sxs-lookup"><span data-stu-id="ffde6-151">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="ffde6-152">To rozwiązanie jest użycie *ViewModel* wzorca.</span><span class="sxs-lookup"><span data-stu-id="ffde6-152">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="ffde6-153">Korzystając z tego wzorca, możemy utworzyć silnie typizowanych klas, które są zoptymalizowane pod kątem scenariuszy określonego widoku, a które udostępnianie właściwości dla zawartości dynamicznej, wartości/wymagane przez naszych szablonów widoku.</span><span class="sxs-lookup"><span data-stu-id="ffde6-153">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="ffde6-154">Naszych zajęć kontrolera można wypełnić i przekazać te zoptymalizowane pod kątem widoku klasy do naszych Wyświetl szablon do użycia.</span><span class="sxs-lookup"><span data-stu-id="ffde6-154">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="ffde6-155">Dzięki temu, bezpieczeństwo typów, sprawdzanie w czasie kompilacji i Edytor technologii IntelliSense w szablonach widoku.</span><span class="sxs-lookup"><span data-stu-id="ffde6-155">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="ffde6-156">Utworzymy dwa modele widok do użycia w naszym kontroler koszyka: ShoppingCartViewModel będzie przechowywać zawartość koszyka użytkownika i ShoppingCartRemoveViewModel będzie służyć do wyświetlania informacji potwierdzenie, gdy użytkownik usunie coś z ich koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-156">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="ffde6-157">Utwórz nowy folder modele widoków w folderze głównym nasz projekt, aby zachować zorganizowane.</span><span class="sxs-lookup"><span data-stu-id="ffde6-157">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="ffde6-158">Kliknij prawym przyciskiem myszy projekt, wybierz opcję Dodaj / nowy Folder.</span><span class="sxs-lookup"><span data-stu-id="ffde6-158">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="ffde6-159">Nazwa folderu modele widoków.</span><span class="sxs-lookup"><span data-stu-id="ffde6-159">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="ffde6-160">Następnie Dodaj klasę ShoppingCartViewModel w folderze modele widoków.</span><span class="sxs-lookup"><span data-stu-id="ffde6-160">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="ffde6-161">Posiada dwie właściwości: lista elementów z koszyka, a wartość dziesiętną, aby pomieścić całkowita cena dla wszystkich elementów w koszyku.</span><span class="sxs-lookup"><span data-stu-id="ffde6-161">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="ffde6-162">Teraz Dodaj ShoppingCartRemoveViewModel do folderu modele widoków z następującymi właściwościami cztery.</span><span class="sxs-lookup"><span data-stu-id="ffde6-162">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="ffde6-163">Kontroler koszyka</span><span class="sxs-lookup"><span data-stu-id="ffde6-163">The Shopping Cart Controller</span></span>

<span data-ttu-id="ffde6-164">Kontroler koszyka ma trzy główne funkcje: Dodawanie elementów do koszyka, usunięcie elementów z koszyka i wyświetlania elementów w koszyku.</span><span class="sxs-lookup"><span data-stu-id="ffde6-164">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="ffde6-165">Użyj trzech klas, firma Microsoft wprowadzi właśnie utworzony: ShoppingCartViewModel ShoppingCartRemoveViewModel i ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="ffde6-165">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="ffde6-166">Tak jak w przypadku StoreController i StoreManagerController dodasz pole do przechowywania wystąpienie MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="ffde6-166">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="ffde6-167">Dodaj nowy kontroler koszyka do projektu przy użyciu szablonu pusty kontroler.</span><span class="sxs-lookup"><span data-stu-id="ffde6-167">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="ffde6-168">Oto kompletny kontrolera ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="ffde6-168">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="ffde6-169">Akcje indeksu i Dodaj kontroler powinien wyglądać znajomo.</span><span class="sxs-lookup"><span data-stu-id="ffde6-169">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="ffde6-170">Akcji kontrolera Remove i CartSummary obsługiwać dwa przypadki specjalne, które omówimy w poniższej sekcji.</span><span class="sxs-lookup"><span data-stu-id="ffde6-170">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="ffde6-171">Aktualizacje interfejsu AJAX przy użyciu jQuery</span><span class="sxs-lookup"><span data-stu-id="ffde6-171">Ajax Updates with jQuery</span></span>

<span data-ttu-id="ffde6-172">Następnie utworzymy strony indeksu koszyka zakupów, zdecydowanie jest wpisane w ShoppingCartViewModel, która używa tego szablonu w widoku listy przy użyciu tej samej metody przed.</span><span class="sxs-lookup"><span data-stu-id="ffde6-172">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="ffde6-173">Jednak zamiast Html.ActionLink do usuwanie elementów z koszyka, użyjemy jQuery do "Podłączanie" Zdarzenie kliknięcia dla wszystkich łączy w tym widoku, które mają klas kodu HTML biznesowych — właścicieli.</span><span class="sxs-lookup"><span data-stu-id="ffde6-173">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="ffde6-174">Zamiast Publikowanie formularza, ta procedura obsługi zdarzeń kliknij właśnie wprowadzi wywołania zwrotnego AJAX naszej RemoveFromCart akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ffde6-174">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="ffde6-175">RemoveFromCart zwraca wynik Zserializowany do ciągu JSON, które naszym wywołania zwrotnego jQuery następnie analizuje i wykonuje czterech szybkich aktualizacji do strony, przy użyciu jQuery:</span><span class="sxs-lookup"><span data-stu-id="ffde6-175">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="ffde6-176">Usuwa albumu usuniętych z listy</span><span class="sxs-lookup"><span data-stu-id="ffde6-176">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="ffde6-177">Aktualizuje liczba koszyka w nagłówku</span><span class="sxs-lookup"><span data-stu-id="ffde6-177">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="ffde6-178">Wyświetla komunikat o aktualizacji dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="ffde6-178">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="ffde6-179">Aktualizuje cena łączna koszyka</span><span class="sxs-lookup"><span data-stu-id="ffde6-179">Updates the cart total price</span></span>

<span data-ttu-id="ffde6-180">Ponieważ scenariusz Usuń jest obsługiwane przez wywołanie zwrotne Ajax w widoku indeksu, nie potrzebujemy dodatkowy widok RemoveFromCart akcji.</span><span class="sxs-lookup"><span data-stu-id="ffde6-180">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="ffde6-181">Oto kompletny kod dla widoku /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="ffde6-181">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="ffde6-182">Aby przetestować tę możliwość, musimy mieć możliwość dodawania elementów do naszego koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-182">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="ffde6-183">Zaktualizujemy naszych **szczegóły Store** widok ma zawierać przycisk "Dodaj do koszyka".</span><span class="sxs-lookup"><span data-stu-id="ffde6-183">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="ffde6-184">Są nam na tym, firma Microsoft może obejmować niektóre spośród albumu dodatkowe informacje, które dodaliśmy od czasu ostatniej aktualizacji firma w tym widoku: Gatunku wykonawcy, ceny i albumów.</span><span class="sxs-lookup"><span data-stu-id="ffde6-184">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="ffde6-185">Zaktualizowany kod widoku szczegółów Store pojawi się, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ffde6-185">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="ffde6-186">Teraz możemy kliknij za pośrednictwem sklepu i przetestować, dodając i usuwając ze zdjęciami do i z naszego koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-186">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="ffde6-187">Uruchom aplikację, a następnie przejdź do indeksu Store.</span><span class="sxs-lookup"><span data-stu-id="ffde6-187">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="ffde6-188">Następnie kliknij na określonego rodzaju, aby wyświetlić listę albumów.</span><span class="sxs-lookup"><span data-stu-id="ffde6-188">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="ffde6-189">Teraz kliknięcie tytuł pokazuje nasz zaktualizowane widoku Szczegóły albumu, w tym przycisk "Dodaj do koszyka".</span><span class="sxs-lookup"><span data-stu-id="ffde6-189">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="ffde6-190">Kliknięcie przycisku "Dodaj do koszyka" przedstawiono naszych koszyk sklepowy indeksu za pomocą lista podsumowania koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="ffde6-190">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="ffde6-191">Po załadowaniu usługi koszyka, możesz kliknąć Usuń z koszyka łącza w celu wyświetlenia aktualizacji interfejsu Ajax do koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-191">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="ffde6-192">Utworzyliśmy się działającego koszyk, co pozwala niezarejestrowanym użytkownikom dodać elementy do ich koszyka.</span><span class="sxs-lookup"><span data-stu-id="ffde6-192">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="ffde6-193">W poniższej sekcji firma Microsoft będzie zezwolić im na rejestrowanie i ukończenie procesu realizowania zamówienia.</span><span class="sxs-lookup"><span data-stu-id="ffde6-193">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="ffde6-194">[Poprzednie](mvc-music-store-part-7.md)
> [dalej](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="ffde6-194">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
