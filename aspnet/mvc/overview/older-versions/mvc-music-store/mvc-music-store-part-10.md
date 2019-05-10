---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: Część 10. Końcowe aktualizacje nawigacji i projektu witryny, podsumowanie | Dokumentacja firmy Microsoft
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 10 obejmuje końcowe aktualizacje nawigacji i S...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65129718"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="a2800-104">Część 10. Końcowe aktualizacje nawigacji i projektu witryny, podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a2800-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>

<span data-ttu-id="a2800-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a2800-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a2800-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="a2800-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a2800-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="a2800-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a2800-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="a2800-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a2800-109">Część 10 obejmuje końcowe aktualizacje nawigacji i projektu witryny, podsumowanie.</span><span class="sxs-lookup"><span data-stu-id="a2800-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>

<span data-ttu-id="a2800-110">Najważniejsze funkcje została ukończona w naszej witrynie, ale wciąż istnieje pewne funkcje do dodania do nawigacji w witrynie, strony głównej i stronie przeglądanie Store.</span><span class="sxs-lookup"><span data-stu-id="a2800-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="a2800-111">Tworzenie zakupów podsumowania widoku częściowego koszyka</span><span class="sxs-lookup"><span data-stu-id="a2800-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="a2800-112">Chcemy udostępnić liczba elementów w koszyku użytkownika w całej lokacji.</span><span class="sxs-lookup"><span data-stu-id="a2800-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="a2800-113">Firma Microsoft można łatwo zaimplementować to przez utworzenie widoku częściowego, który zostanie dodany do naszych Site.master.</span><span class="sxs-lookup"><span data-stu-id="a2800-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="a2800-114">Jak pokazano wcześniej, kontroler ShoppingCart obejmuje CartSummary metody akcji, która zwraca widok częściowy:</span><span class="sxs-lookup"><span data-stu-id="a2800-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="a2800-115">Aby utworzyć widok częściowy CartSummary, kliknij prawym przyciskiem myszy w folderze Widoki/ShoppingCart i wybierz pozycję Dodaj widok.</span><span class="sxs-lookup"><span data-stu-id="a2800-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="a2800-116">Nazwa widoku CartSummary i zaznacz pole wyboru "Utwórz widok częściowy", jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a2800-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="a2800-117">Widok częściowy CartSummary jest bardzo proste — jest tylko łącze do widoku ShoppingCart indeksu, który pokazuje liczbę elementów w koszyku.</span><span class="sxs-lookup"><span data-stu-id="a2800-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="a2800-118">Kompletny kod dla CartSummary.cshtml jest następująca:</span><span class="sxs-lookup"><span data-stu-id="a2800-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="a2800-119">Firma Microsoft może zawierać widoku częściowego w dowolnej stronie w witrynie, łącznie z lokacji głównej, przy użyciu metody Html.RenderAction.</span><span class="sxs-lookup"><span data-stu-id="a2800-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="a2800-120">RenderAction wymaga od nas określić nazwę akcji ("CartSummary") i nazwy kontrolera ("ShoppingCart") jako poniżej.</span><span class="sxs-lookup"><span data-stu-id="a2800-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="a2800-121">Przed dodaniem to lokacja układu, zostaną również utworzone Menu gatunku umożliwia wykonywanie wszystkich naszych Site.master aktualizacji w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="a2800-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="a2800-122">Tworzenie widoku częściowego Menu gatunku</span><span class="sxs-lookup"><span data-stu-id="a2800-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="a2800-123">Firma Microsoft może ułatwić partii dla naszych użytkowników przejść w sklepie, dodając Menu gatunku, która zawiera listę wszystkich gatunki dostępne w naszym Sklepie.</span><span class="sxs-lookup"><span data-stu-id="a2800-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="a2800-124">Firma Microsoft będzie należy wykonać takie same kroki również utworzyć widok częściowy GenreMenu, a następnie możemy Dodaj oba te do gałęzi głównej witryny.</span><span class="sxs-lookup"><span data-stu-id="a2800-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="a2800-125">Najpierw dodaj następujące akcji kontrolera GenreMenu do StoreController:</span><span class="sxs-lookup"><span data-stu-id="a2800-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="a2800-126">Ta akcja zwraca listę gatunki, które będą wyświetlane przez widok częściowy, który zostanie następnie utworzymy.</span><span class="sxs-lookup"><span data-stu-id="a2800-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="a2800-127">*Uwaga: Dodano atrybut [ChildActionOnly] tej akcji kontrolera, co oznacza, że ma być uruchamiany tylko ta akcja ma być używany z widoku częściowego. Ten atrybut zapobiega wykonywana, przechodząc do /Store/GenreMenu tej akcji kontrolera. Nie jest to wymagane dla widoków częściowych, ale jest dobrym rozwiązaniem, ponieważ chcemy się upewnić, że nasze akcji kontrolera są używane jako Chcieliśmy. Możemy również zwróconego PartialView zamiast widoku, który umożliwia aparat widoku, nazywa się czy nie należy używać układu dla tego widoku jest uwzględniane w innych widokach.*</span><span class="sxs-lookup"><span data-stu-id="a2800-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="a2800-128">Kliknij prawym przyciskiem myszy na GenreMenu akcji kontrolera oraz tworzenia widoku częściowego o nazwie GenreMenu, który jest silnie typizowane przy użyciu klasy gatunku widoku danych, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a2800-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="a2800-129">Aktualizowanie kodu widoku GenreMenu widoku częściowego do wyświetlenia elementów przy użyciu nieuporządkowaną listę w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="a2800-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="a2800-130">Trwa aktualizowanie układu witryny do wyświetlenia naszych widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="a2800-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="a2800-131">Możemy dodać naszych widoki częściowe do układu witryny (/widoków/Shared/\_Layout.cshtml) przez wywołanie metody Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="a2800-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="a2800-132">Dodamy ich obu w, a także niektóre dodatkową marżę do wyświetlania, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="a2800-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="a2800-133">Teraz gdy możemy uruchomić aplikację, zobaczymy gatunku w obszarze nawigacji po lewej stronie i Podsumowanie koszyka u góry.</span><span class="sxs-lookup"><span data-stu-id="a2800-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="a2800-134">Aktualizuj stronę Przeglądaj Store</span><span class="sxs-lookup"><span data-stu-id="a2800-134">Update to the Store Browse page</span></span>

<span data-ttu-id="a2800-135">Na stronie przeglądania Store będzie działać, ale nie wygląda bardzo dobra.</span><span class="sxs-lookup"><span data-stu-id="a2800-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="a2800-136">Firma Microsoft może aktualizować strony Aby pokazać albumów w lepszego układu, aktualizując widok kodu (znajdujący się w /Views/Store/Browse.cshtml) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a2800-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="a2800-137">W tym miejscu wprowadzamy użytkowania Url.Action zamiast Html.ActionLink, dzięki czemu można zastosować specjalne formatowanie łącza do uwzględnienia kompozycji albumu.</span><span class="sxs-lookup"><span data-stu-id="a2800-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="a2800-138">*Uwaga: Firma Microsoft są wyświetlane okładki albumu ogólny, dla tych albumów. Te informacje są przechowywane w bazie danych i można edytować za pomocą Menedżera Store. Zachęcamy do dodania własnej kompozycji.*</span><span class="sxs-lookup"><span data-stu-id="a2800-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="a2800-139">Teraz gdy możemy przejść do określonego rodzaju, zobaczymy albumów wyświetlane w siatce z w kompozycją albumu.</span><span class="sxs-lookup"><span data-stu-id="a2800-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="a2800-140">Aktualizowanie stronę główną, aby pokazać albumów sprzedaży w górnej</span><span class="sxs-lookup"><span data-stu-id="a2800-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="a2800-141">Chcemy są wyposażone w naszym najlepiej sprzedających się ze zdjęciami, na stronie głównej, aby zwiększyć sprzedaż.</span><span class="sxs-lookup"><span data-stu-id="a2800-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="a2800-142">Wybierzemy niektórych aktualizacji do naszych HomeController obsługiwały, a następnie dodaj niektóre dodatkowe grafiki.</span><span class="sxs-lookup"><span data-stu-id="a2800-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="a2800-143">Najpierw dodamy właściwość nawigacji do klasy Nasze albumu tak, aby EntityFramework wie, że są one skojarzone.</span><span class="sxs-lookup"><span data-stu-id="a2800-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="a2800-144">Kilka ostatnich wierszy z naszych **albumu** klasy powinna teraz wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="a2800-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="a2800-145">*Uwaga: Wymaga to dodanie przy użyciu instrukcji w przestrzeni nazw System.Collections.Generic.*</span><span class="sxs-lookup"><span data-stu-id="a2800-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="a2800-146">Najpierw dodamy, pole storeDB i MvcMusicStore.Models przy użyciu instrukcji, tak jak w naszych innych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="a2800-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="a2800-147">Następnie dodamy następującą metodę do HomeController, który sprawdza naszej bazie danych, odnaleźć najważniejsze albumów sprzedaży według OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="a2800-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="a2800-148">Jest to metoda prywatna, ponieważ nie chcemy udostępnić go jako akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a2800-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="a2800-149">Firma Microsoft jest uwzględniania go w HomeController dla uproszczenia, ale zaleca się przeniesienie logikę biznesową do klasy osobną usługą zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="a2800-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="a2800-150">Mając to na miejscu możemy zaktualizować akcji kontrolera indeks zapytania 5 najlepiej sprzedających się ze zdjęciami i przywrócić je do widoku.</span><span class="sxs-lookup"><span data-stu-id="a2800-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="a2800-151">Kompletny kod dla zaktualizowanego HomeController to, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a2800-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="a2800-152">Na koniec musimy aktualizacja naszych widoku Home indeks, tak, aby go wyświetlić listę albumów aktualizowanie typu modelu i dodając listy albumu do dołu.</span><span class="sxs-lookup"><span data-stu-id="a2800-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="a2800-153">Firma Microsoft będzie okazji można również dodać nagłówek i sekcja promocji do strony.</span><span class="sxs-lookup"><span data-stu-id="a2800-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="a2800-154">Teraz gdy możemy uruchomić aplikację, zobaczymy zaktualizowane strony głównej z najważniejszych sprzedaży ze zdjęciami i naszej promocyjne wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a2800-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="a2800-155">Wniosek</span><span class="sxs-lookup"><span data-stu-id="a2800-155">Conclusion</span></span>

<span data-ttu-id="a2800-156">Zobaczyliśmy, że ASP.NET MVC można łatwo utworzyć zaawansowane witryny sieci Web z dostępu do bazy danych, członkostwo w technologii AJAX, itp.</span><span class="sxs-lookup"><span data-stu-id="a2800-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="a2800-157">bardzo szybko.</span><span class="sxs-lookup"><span data-stu-id="a2800-157">pretty quickly.</span></span> <span data-ttu-id="a2800-158">Miejmy nadzieję w tym samouczku przyznał Ci narzędzia, których potrzebujesz do rozpoczęcia tworzenia własnych platformy ASP.NET MVC aplikacji!</span><span class="sxs-lookup"><span data-stu-id="a2800-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a2800-159">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="a2800-159">Previous</span></span>](mvc-music-store-part-9.md)
