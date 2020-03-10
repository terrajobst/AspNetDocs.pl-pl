---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Część 9: Rejestracja i wyewidencjonowywanie | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 9 obejmuje rejestrację i wyewidencjonowanie.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559536"
---
# <a name="part-9-registration-and-checkout"></a><span data-ttu-id="d2c54-104">Część 9. Rejestracja i finalizacja zakupu</span><span class="sxs-lookup"><span data-stu-id="d2c54-104">Part 9: Registration and Checkout</span></span>

<span data-ttu-id="d2c54-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="d2c54-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="d2c54-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d2c54-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="d2c54-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="d2c54-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="d2c54-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="d2c54-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="d2c54-109">Część 9 obejmuje rejestrację i wyewidencjonowanie.</span><span class="sxs-lookup"><span data-stu-id="d2c54-109">Part 9 covers Registration and Checkout.</span></span>

<span data-ttu-id="d2c54-110">W tej sekcji zostanie utworzona CheckoutController, która będzie zbierać informacje o adresie i płatnościach klientów.</span><span class="sxs-lookup"><span data-stu-id="d2c54-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="d2c54-111">Wymagamy od użytkowników zarejestrowania się w naszej witrynie przed wyewidencjonowaniem, więc ten kontroler będzie wymagał autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="d2c54-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="d2c54-112">Użytkownicy przejdą do procesu wyewidencjonowania w koszyku, klikając przycisk "Wyewidencjonuj".</span><span class="sxs-lookup"><span data-stu-id="d2c54-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="d2c54-113">Jeśli użytkownik nie jest zalogowany, zostanie wyświetlony monit o podanie.</span><span class="sxs-lookup"><span data-stu-id="d2c54-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="d2c54-114">Po pomyślnym zalogowaniu użytkownika zostanie wyświetlony widok adres i płatność.</span><span class="sxs-lookup"><span data-stu-id="d2c54-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="d2c54-115">Po wypełnieniu formularza i przesłaniu zamówienia zostanie wyświetlony ekran potwierdzenie zamówienia.</span><span class="sxs-lookup"><span data-stu-id="d2c54-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="d2c54-116">Próba wyświetlenia nieistniejącej kolejności lub zamówienia, które nie należy do Ciebie, spowoduje wyświetlenie widoku błędów.</span><span class="sxs-lookup"><span data-stu-id="d2c54-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="d2c54-117">Migrowanie koszyka</span><span class="sxs-lookup"><span data-stu-id="d2c54-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="d2c54-118">Gdy proces zakupów jest anonimowy, gdy użytkownik kliknie przycisk wyewidencjonowania, będzie wymagane zarejestrowanie i zalogowanie się.</span><span class="sxs-lookup"><span data-stu-id="d2c54-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="d2c54-119">Użytkownicy będą oczekiwać, że firma Microsoft będzie obsługiwać informacje o koszyku zakupów między wizytami, więc należy skojarzyć informacje o koszyku z użytkownikiem po zakończeniu rejestracji lub zalogowaniu się.</span><span class="sxs-lookup"><span data-stu-id="d2c54-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="d2c54-120">Jest to bardzo proste, ponieważ Klasa ShoppingCart ma już metodę, która spowoduje skojarzenie wszystkich elementów w bieżącym koszyku z nazwą użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d2c54-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="d2c54-121">Po ukończeniu rejestracji lub zalogowania użytkownik będzie musiał wywołać tę metodę.</span><span class="sxs-lookup"><span data-stu-id="d2c54-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="d2c54-122">Otwórz klasę **elementu AccountController** , która została dodana podczas konfigurowania członkostwa i autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="d2c54-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="d2c54-123">Dodaj instrukcję using odwołującą się do MvcMusicStore. Models, a następnie Dodaj następującą metodę MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="d2c54-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="d2c54-124">Następnie zmodyfikuj akcję post logowania w celu wywołania MigrateShoppingCart po sprawdzeniu poprawności użytkownika, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="d2c54-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="d2c54-125">Wprowadź tę samą zmianę w akcji rejestracji po pomyślnym utworzeniu konta użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d2c54-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="d2c54-126">To teraz — anonimowy koszyk do zakupów zostanie automatycznie przeniesiony na konto użytkownika po pomyślnej rejestracji lub zalogowaniu się.</span><span class="sxs-lookup"><span data-stu-id="d2c54-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="d2c54-127">Tworzenie CheckoutController</span><span class="sxs-lookup"><span data-stu-id="d2c54-127">Creating the CheckoutController</span></span>

<span data-ttu-id="d2c54-128">Kliknij prawym przyciskiem myszy folder controllers i Dodaj nowy kontroler do projektu o nazwie CheckoutController przy użyciu szablonu pustego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d2c54-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="d2c54-129">Najpierw Dodaj atrybut Autoryzuj powyżej deklaracji klasy kontrolera, aby wymagać od użytkowników rejestrowania przed wyewidencjonowaniem:</span><span class="sxs-lookup"><span data-stu-id="d2c54-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="d2c54-130">*Uwaga: jest to podobne do wprowadzonej wcześniej zmiany w StoreManagerController, ale w takim przypadku atrybut Autoryzuj wymaga, aby użytkownik miał rolę administratora. W kontrolerze wyewidencjonowania wymagamy, aby użytkownik był zalogowany, ale nie musi być administratorem.*</span><span class="sxs-lookup"><span data-stu-id="d2c54-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="d2c54-131">Ze względu na prostotę nie będziemy omawiać informacji o płatnościach w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="d2c54-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="d2c54-132">Zamiast tego zezwalamy użytkownikom na wyewidencjonowywanie przy użyciu kodu promocyjnego.</span><span class="sxs-lookup"><span data-stu-id="d2c54-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="d2c54-133">Ten kod promocyjny będzie przechowywany przy użyciu stałej o nazwie PromoCode.</span><span class="sxs-lookup"><span data-stu-id="d2c54-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="d2c54-134">Podobnie jak w StoreController, zadeklarujemy pole, aby pomieścić wystąpienie klasy MusicStoreEntities o nazwie storeDB.</span><span class="sxs-lookup"><span data-stu-id="d2c54-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="d2c54-135">Aby można było korzystać z klasy MusicStoreEntities, konieczne będzie dodanie instrukcji using dla przestrzeni nazw MvcMusicStore. models.</span><span class="sxs-lookup"><span data-stu-id="d2c54-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="d2c54-136">Poniżej znajduje się Górna część naszego kontrolera wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="d2c54-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="d2c54-137">CheckoutController będą miały następujące akcje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="d2c54-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="d2c54-138">**AddressAndPayment (metoda Get)** wyświetli formularz umożliwiający użytkownikowi wprowadzanie informacji.</span><span class="sxs-lookup"><span data-stu-id="d2c54-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="d2c54-139">**AddressAndPayment (Metoda post)** sprawdzi dane wejściowe i przetworzy zamówienie.</span><span class="sxs-lookup"><span data-stu-id="d2c54-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="d2c54-140">**Zakończenie** zostanie wyświetlone po pomyślnym zakończeniu procesu wyewidencjonowania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d2c54-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="d2c54-141">Ten widok będzie zawierać numer zamówienia użytkownika w postaci potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="d2c54-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="d2c54-142">Najpierw Zmień nazwę akcji kontrolera indeksu (która została wygenerowana podczas tworzenia kontrolera) do AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="d2c54-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="d2c54-143">Ta akcja kontrolera po prostu wyświetla formularz wyewidencjonowania, dlatego nie wymaga żadnych informacji o modelu.</span><span class="sxs-lookup"><span data-stu-id="d2c54-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="d2c54-144">Nasza metoda POST AddressAndPayment będzie zgodna z tym samym wzorcem, który został użyty w StoreManagerController: spróbuje zaakceptować przesłane formularz i zakończyć zamówienie i ponownie wyświetlić formularz, jeśli zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="d2c54-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="d2c54-145">Po sprawdzeniu poprawności formularza spełnia wymagania dotyczące weryfikacji dla zamówienia, wartość formularza PromoCode zostanie sprawdzona bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="d2c54-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="d2c54-146">Przy założeniu, że wszystko jest poprawne, będziemy zapisywać zaktualizowane informacje z kolejnością, poinstruować obiekt ShoppingCart, aby dokończył proces zamówienia, i przekierować do kompletnej akcji.</span><span class="sxs-lookup"><span data-stu-id="d2c54-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="d2c54-147">Po pomyślnym zakończeniu procesu wyewidencjonowania użytkownicy zostaną przekierowani do akcji Ukończ kontroler.</span><span class="sxs-lookup"><span data-stu-id="d2c54-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="d2c54-148">Ta akcja spowoduje wykonanie prostej kontroli w celu sprawdzenia, czy zamówienie rzeczywiście należy do zalogowanego użytkownika przed pokazywaniem numeru zamówienia jako potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="d2c54-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="d2c54-149">*Uwaga: widok błędów został automatycznie utworzony dla nas w folderze/Views/Shared po rozpoczęciu projektu.*</span><span class="sxs-lookup"><span data-stu-id="d2c54-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="d2c54-150">Kompletny kod CheckoutController jest następujący:</span><span class="sxs-lookup"><span data-stu-id="d2c54-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="d2c54-151">Dodawanie widoku AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="d2c54-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="d2c54-152">Teraz Utwórzmy widok AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="d2c54-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="d2c54-153">Kliknij prawym przyciskiem myszy jedną z akcji kontrolera AddressAndPayment i Dodaj widok o nazwie AddressAndPayment, który jednoznacznie wpisano jako zamówienie i używa szablonu edycji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d2c54-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="d2c54-154">Ten widok umożliwia korzystanie z dwóch technik, które oglądamy podczas tworzenia widoku StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="d2c54-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="d2c54-155">Użyjemy html. EditorForModel () do wyświetlania pól formularza dla modelu zamówienia</span><span class="sxs-lookup"><span data-stu-id="d2c54-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="d2c54-156">Będziemy korzystać z reguł walidacji przy użyciu klasy Order z atrybutami walidacji</span><span class="sxs-lookup"><span data-stu-id="d2c54-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="d2c54-157">Zaczniemy od zaktualizowania kodu formularza do używania HTML. EditorForModel (), a następnie dodatkowego pola tekstowego dla kodu promocyjnego.</span><span class="sxs-lookup"><span data-stu-id="d2c54-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="d2c54-158">Poniżej przedstawiono pełen kod widoku AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="d2c54-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="d2c54-159">Definiowanie reguł walidacji dla zamówienia</span><span class="sxs-lookup"><span data-stu-id="d2c54-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="d2c54-160">Teraz, gdy nasz widok jest skonfigurowany, skonfigurujemy reguły sprawdzania poprawności dla modelu zamówienia, tak jak wcześniej dla modelu albumu.</span><span class="sxs-lookup"><span data-stu-id="d2c54-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="d2c54-161">Kliknij prawym przyciskiem myszy folder modele i Dodaj klasę o nazwie Order.</span><span class="sxs-lookup"><span data-stu-id="d2c54-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="d2c54-162">Oprócz atrybutów weryfikacji użytych wcześniej dla albumu należy również użyć wyrażenia regularnego do zweryfikowania adresu e-mail użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d2c54-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="d2c54-163">Próba przesłania formularza z brakującymi lub nieprawidłowymi informacjami spowoduje wyświetlenie komunikatu o błędzie przy użyciu walidacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d2c54-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="d2c54-164">Tak, większość pracy w procesie wyewidencjonowania została wykonana. Mamy tylko kilka szanse i zakończy się.</span><span class="sxs-lookup"><span data-stu-id="d2c54-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="d2c54-165">Musimy dodać dwa proste widoki i będziemy musieli zadbać o przekazanie informacji o koszyku podczas procesu logowania.</span><span class="sxs-lookup"><span data-stu-id="d2c54-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="d2c54-166">Dodawanie widoku ukończenia wyewidencjonowania</span><span class="sxs-lookup"><span data-stu-id="d2c54-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="d2c54-167">Widok Kończenie wyewidencjonowywania jest bardzo prosty, ponieważ wymaga tylko wyświetlenia identyfikatora zamówienia.</span><span class="sxs-lookup"><span data-stu-id="d2c54-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="d2c54-168">Kliknij prawym przyciskiem myszy akcję Ukończ kontroler i Dodaj widok o nazwie Complete, który jest silnie określony jako int.</span><span class="sxs-lookup"><span data-stu-id="d2c54-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="d2c54-169">Teraz będziemy aktualizować kod widoku w celu wyświetlenia identyfikatora zamówienia, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d2c54-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="d2c54-170">Aktualizowanie widoku błędów</span><span class="sxs-lookup"><span data-stu-id="d2c54-170">Updating The Error view</span></span>

<span data-ttu-id="d2c54-171">Szablon domyślny zawiera widok błędów w folderze widoki udostępnione, dzięki czemu można go ponownie używać w innym miejscu w witrynie.</span><span class="sxs-lookup"><span data-stu-id="d2c54-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="d2c54-172">Ten widok błędów zawiera bardzo prosty błąd i nie używa naszego układu witryny, dlatego zaktualizujemy go.</span><span class="sxs-lookup"><span data-stu-id="d2c54-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="d2c54-173">Ponieważ jest to ogólna strona błędu, zawartość jest bardzo prosta.</span><span class="sxs-lookup"><span data-stu-id="d2c54-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="d2c54-174">Jeśli użytkownik chce ponownie spróbować wykonać tę akcję, zostanie uwzględniony komunikat i link do przechodzenia do poprzedniej strony w historii.</span><span class="sxs-lookup"><span data-stu-id="d2c54-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> <span data-ttu-id="d2c54-175">[Poprzednie](mvc-music-store-part-8.md)
> [dalej](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="d2c54-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
