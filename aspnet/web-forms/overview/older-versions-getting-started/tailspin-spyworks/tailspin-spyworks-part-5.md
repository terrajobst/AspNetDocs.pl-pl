---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Część 5: logika biznesowa | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 5 dodaje logikę biznesową.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630306"
---
# <a name="part-5-business-logic"></a><span data-ttu-id="f46cb-104">Część 5. Logika biznesowa</span><span class="sxs-lookup"><span data-stu-id="f46cb-104">Part 5: Business Logic</span></span>

<span data-ttu-id="f46cb-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="f46cb-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="f46cb-106">Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f46cb-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="f46cb-107">W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.</span><span class="sxs-lookup"><span data-stu-id="f46cb-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="f46cb-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="f46cb-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="f46cb-109">Część 5 dodaje logikę biznesową.</span><span class="sxs-lookup"><span data-stu-id="f46cb-109">Part 5 adds some business logic.</span></span>

## <a id="_Toc260221671"></a><span data-ttu-id="f46cb-110">Dodawanie jakiejś logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="f46cb-110">Adding Some Business Logic</span></span>

<span data-ttu-id="f46cb-111">Chcemy, aby nasze środowisko zakupów było dostępne za każdym razem, gdy ktoś odwiedzi naszą witrynę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f46cb-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="f46cb-112">Osoby odwiedzające będą mogły przeglądać i dodawać elementy do koszyka, nawet jeśli nie są zarejestrowane lub zarejestrowani.</span><span class="sxs-lookup"><span data-stu-id="f46cb-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="f46cb-113">Gdy użytkownicy są gotowi do wyewidencjonowania, otrzymają możliwość uwierzytelnienia, a jeśli nie są jeszcze członkami, będą mogli utworzyć konto.</span><span class="sxs-lookup"><span data-stu-id="f46cb-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="f46cb-114">Oznacza to, że konieczne będzie wdrożenie logiki w celu przekonwertowania koszyka ze stanu anonimowego na stan zarejestrowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f46cb-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="f46cb-115">Utwórzmy katalog o nazwie "classes", a następnie kliknij prawym przyciskiem myszy folder i Utwórz nowy plik "Class" o nazwie MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="f46cb-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="f46cb-116">Jak wspomniano wcześniej, rozszerzamy klasę, która implementuje stronę MyShoppingCart. aspx i będziemy używać. Zaawansowana konstrukcja sieciowa "klasy częściowej".</span><span class="sxs-lookup"><span data-stu-id="f46cb-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="f46cb-117">Wygenerowane wywołanie naszego pliku MyShoppingCart.aspx.cf wygląda następująco.</span><span class="sxs-lookup"><span data-stu-id="f46cb-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="f46cb-118">Zwróć uwagę na użycie słowa kluczowego "częściowe".</span><span class="sxs-lookup"><span data-stu-id="f46cb-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="f46cb-119">Właśnie wygenerowany plik klasy wygląda następująco.</span><span class="sxs-lookup"><span data-stu-id="f46cb-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="f46cb-120">Będziemy scalać nasze implementacje, dodając słowo kluczowe części do tego pliku.</span><span class="sxs-lookup"><span data-stu-id="f46cb-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="f46cb-121">Nasz nowy plik klasy będzie teraz wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="f46cb-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="f46cb-122">Pierwszą metodą dodawaną do naszej klasy jest metoda "AddItem".</span><span class="sxs-lookup"><span data-stu-id="f46cb-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="f46cb-123">Jest to metoda, która ostatecznie będzie wywoływana, gdy użytkownik kliknie linki "Dodaj do grafiki" na liście produktów i na stronach szczegółów produktu.</span><span class="sxs-lookup"><span data-stu-id="f46cb-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="f46cb-124">Dołącz następujące elementy do instrukcji using w górnej części strony.</span><span class="sxs-lookup"><span data-stu-id="f46cb-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="f46cb-125">I Dodaj tę metodę do klasy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="f46cb-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="f46cb-126">Używamy LINQ to Entities, aby sprawdzić, czy element znajduje się już w koszyku.</span><span class="sxs-lookup"><span data-stu-id="f46cb-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="f46cb-127">W takim przypadku aktualizujemy ilość zamówienia elementu, w przeciwnym razie utworzymy nowy wpis dla wybranego elementu</span><span class="sxs-lookup"><span data-stu-id="f46cb-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="f46cb-128">Aby wywołać tę metodę, zaimplementujemy stronę AddToCart. aspx, która nie tylko jest klasą tej metody, ale zamiast tego zostanie wyświetlona bieżąca zakupy a = koszyk po dodaniu elementu.</span><span class="sxs-lookup"><span data-stu-id="f46cb-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="f46cb-129">Kliknij prawym przyciskiem myszy nazwę rozwiązania w Eksploratorze rozwiązań i Dodaj nową stronę o nazwie AddToCart. aspx, która została wcześniej ukończona.</span><span class="sxs-lookup"><span data-stu-id="f46cb-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="f46cb-130">Chociaż możemy użyć tej strony do wyświetlania wyników pośrednich, takich jak problemy ze stanem niskiego stanu zasobów itp., Strona nie będzie w rzeczywistości renderowana, ale zamiast tego wywołaj logikę "Dodaj" i Przekieruj.</span><span class="sxs-lookup"><span data-stu-id="f46cb-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="f46cb-131">Aby to osiągnąć, dodamy następujący kod do strony\_zdarzenie ładowania.</span><span class="sxs-lookup"><span data-stu-id="f46cb-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="f46cb-132">Należy zauważyć, że pobieramy produkt do dodania do koszyka z parametru QueryString i wywołując metodę addItem klasy.</span><span class="sxs-lookup"><span data-stu-id="f46cb-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="f46cb-133">Przy założeniu, że żadne błędy nie wystąpią, kontrola jest przenoszona do strony SHoppingCart. aspx, która zostanie w pełni zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="f46cb-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="f46cb-134">Jeśli wystąpi błąd, wystąpił wyjątek.</span><span class="sxs-lookup"><span data-stu-id="f46cb-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="f46cb-135">Obecnie nie została jeszcze zaimplementowana globalna procedura obsługi błędów, więc ten wyjątek zostałby nieobsłużony przez naszą aplikację, ale wkrótce nastąpi rozwiązanie tego problemu.</span><span class="sxs-lookup"><span data-stu-id="f46cb-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="f46cb-136">Należy również pamiętać, że użycie instrukcji Debug. Fail () (dostępne za pośrednictwem `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="f46cb-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="f46cb-137">Czy aplikacja działa w debugerze, ta metoda wyświetli szczegółowe okno dialogowe z informacjami o stanie aplikacji wraz z określonym przez nas komunikatem o błędzie.</span><span class="sxs-lookup"><span data-stu-id="f46cb-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="f46cb-138">Podczas wykonywania w środowisku produkcyjnym instrukcja Debug. Fail () jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="f46cb-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="f46cb-139">Należy zwrócić uwagę na kod powyżej wywołania metody z naszych nazw klas koszyka zakupów "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="f46cb-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="f46cb-140">Dodaj kod, aby zaimplementować metodę w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f46cb-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="f46cb-141">Należy zauważyć, że dodano również przyciski aktualizacji i wyewidencjonowania oraz etykietę, w której można wyświetlić koszyk "łącznie".</span><span class="sxs-lookup"><span data-stu-id="f46cb-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="f46cb-142">Teraz możemy dodać elementy do naszego koszyka zakupów, ale nie wprowadziliśmy logiki, aby wyświetlić koszyk po dodaniu produktu.</span><span class="sxs-lookup"><span data-stu-id="f46cb-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="f46cb-143">Dlatego na stronie MyShoppingCart. aspx zostanie dodana kontrolka EntityDataSource i kontrolka GridVire w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f46cb-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="f46cb-144">Zadzwoń do formularza w projektancie, aby można było dwukrotnie kliknąć przycisk Aktualizuj koszyk i wygenerować procedurę obsługi zdarzeń kliknięcia określoną w deklaracji w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="f46cb-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="f46cb-145">Zaimplementujemy szczegóły później, ale pozwoli to nam na skompilowanie i uruchomienie naszej aplikacji bez błędów.</span><span class="sxs-lookup"><span data-stu-id="f46cb-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="f46cb-146">Gdy uruchamiasz aplikację i dodasz element do koszyka zakupów, zobaczysz.</span><span class="sxs-lookup"><span data-stu-id="f46cb-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="f46cb-147">Zwróć uwagę na to, że od "domyślnego" wyświetlania siatki są one stosowane przez implementację trzech kolumn niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="f46cb-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="f46cb-148">Pierwszym z nich jest edytowalne pole "Bound" dla danej ilości:</span><span class="sxs-lookup"><span data-stu-id="f46cb-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="f46cb-149">Następna jest kolumna "obliczeniowa", w której wyświetlana jest suma elementu wiersza (element jest kosztem do uporządkowania):</span><span class="sxs-lookup"><span data-stu-id="f46cb-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="f46cb-150">Na koniec mamy kolumnę niestandardową, która zawiera kontrolkę CheckBox, która będzie używana przez użytkownika w celu wskazania, że element powinien zostać usunięty z wykresu zakupów.</span><span class="sxs-lookup"><span data-stu-id="f46cb-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="f46cb-151">Jak widać, wiersz sumy zamówienia jest pusty, dlatego dodamy kilka logiki w celu obliczenia sumy zamówienia.</span><span class="sxs-lookup"><span data-stu-id="f46cb-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="f46cb-152">Najpierw zaimplementujmy metodę "gettotal" dla naszej klasy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="f46cb-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="f46cb-153">W pliku MyShoppingCart.cs Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="f46cb-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="f46cb-154">Następnie na stronie\_Załaduj procedurę obsługi zdarzeń możemy wywołać metodę gettotal.</span><span class="sxs-lookup"><span data-stu-id="f46cb-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="f46cb-155">W tym samym czasie dodamy test, aby sprawdzić, czy koszyk jest pusty i odpowiednio dostosować wyświetlanie.</span><span class="sxs-lookup"><span data-stu-id="f46cb-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="f46cb-156">Jeśli koszyk jest pusty, możemy to zrobić:</span><span class="sxs-lookup"><span data-stu-id="f46cb-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="f46cb-157">A jeśli nie, zobaczymy nasze podsumowanie.</span><span class="sxs-lookup"><span data-stu-id="f46cb-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="f46cb-158">Jednak ta strona nie została jeszcze ukończona.</span><span class="sxs-lookup"><span data-stu-id="f46cb-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="f46cb-159">Potrzebujemy dodatkowej logiki, aby ponownie obliczyć koszyk, usuwając elementy oznaczone do usunięcia i określając nowe wartości ilości, które mogły zostać zmienione w siatce przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f46cb-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="f46cb-160">Umożliwia dodanie metody "RemoveItem" do naszej klasy koszyka zakupów w MyShoppingCart.cs, aby obsłużyć przypadek, gdy użytkownik oznaczy element do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="f46cb-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="f46cb-161">Teraz przejdźmy do usługi AD, aby obsłużyć okoliczności, gdy użytkownik po prostu zmieni jakość do uporządkowania w GridView.</span><span class="sxs-lookup"><span data-stu-id="f46cb-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="f46cb-162">Korzystając z podstawowych funkcji usuwania i aktualizowania w miejscu, możemy wdrożyć logikę, która rzeczywiście aktualizuje koszyk w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f46cb-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="f46cb-163">(In MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="f46cb-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="f46cb-164">Należy zauważyć, że ta metoda oczekuje dwóch parametrów.</span><span class="sxs-lookup"><span data-stu-id="f46cb-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="f46cb-165">Jednym z nich jest identyfikator koszyka zakupów, a drugi to tablica obiektów typu zdefiniowanego przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f46cb-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="f46cb-166">Aby zminimalizować zależność naszej logiki od specyficznych dla interfejsu użytkownika, definiujemy strukturę danych, za pomocą której możemy przekazać elementy koszyka zakupów do naszego kodu bez konieczności bezpośredniego dostępu do formantu GridView.</span><span class="sxs-lookup"><span data-stu-id="f46cb-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="f46cb-167">W naszym pliku MyShoppingCart.aspx.cs możemy użyć tej struktury na naszym przycisku aktualizacji kliknij program obsługi zdarzeń w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f46cb-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="f46cb-168">Pamiętaj, że oprócz aktualizowania koszyka będziemy również aktualizować sumę koszyka.</span><span class="sxs-lookup"><span data-stu-id="f46cb-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="f46cb-169">Należy pamiętać o szczególnym znaczeniu dla tego wiersza kodu:</span><span class="sxs-lookup"><span data-stu-id="f46cb-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="f46cb-170">GetValues () to specjalna funkcja pomocnicza, która zostanie zaimplementowana w MyShoppingCart.aspx.cs w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f46cb-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="f46cb-171">Zapewnia to czysty sposób uzyskiwania dostępu do wartości elementów powiązanych w naszym formancie GridView.</span><span class="sxs-lookup"><span data-stu-id="f46cb-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="f46cb-172">Ponieważ nasz formant CheckBox "Usuń element" nie jest powiązany, będziemy uzyskiwać do niego dostęp za pośrednictwem metody FindControl ().</span><span class="sxs-lookup"><span data-stu-id="f46cb-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="f46cb-173">Na tym etapie opracowywania projektu jesteśmy gotowi do wdrożenia procesu wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="f46cb-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="f46cb-174">Przed wykonaniem tej czynności Użyj programu Visual Studio do wygenerowania bazy danych członkostwa i dodania użytkownika do repozytorium członkostwa.</span><span class="sxs-lookup"><span data-stu-id="f46cb-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f46cb-175">[Poprzednie](tailspin-spyworks-part-4.md)
> [dalej](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="f46cb-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
