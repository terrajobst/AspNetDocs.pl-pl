---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Część 10: ostateczne aktualizacje nawigacji i projektu witryny, wniosek | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 10 obejmuje ostateczne aktualizacje nawigacji i...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539369"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="55d23-104">Część 10. Końcowe aktualizacje nawigacji i projektu witryny, podsumowanie</span><span class="sxs-lookup"><span data-stu-id="55d23-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>

<span data-ttu-id="55d23-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="55d23-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="55d23-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="55d23-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="55d23-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="55d23-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="55d23-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="55d23-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="55d23-109">Część 10 obejmuje ostateczne aktualizacje nawigacji i projektu witryny, wnioski.</span><span class="sxs-lookup"><span data-stu-id="55d23-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>

<span data-ttu-id="55d23-110">Wszystkie najważniejsze funkcje naszej witryny zostały wykonane, ale nadal mamy kilka funkcji do dodania do nawigacji witryny, strony głównej i strony przeglądania sklepu.</span><span class="sxs-lookup"><span data-stu-id="55d23-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="55d23-111">Tworzenie widoku częściowego podsumowania koszyka zakupów</span><span class="sxs-lookup"><span data-stu-id="55d23-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="55d23-112">Chcemy uwidocznić liczbę elementów w koszyku użytkownika w całej lokacji.</span><span class="sxs-lookup"><span data-stu-id="55d23-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="55d23-113">Można to łatwo zaimplementować, tworząc widok częściowy, który jest dodawany do naszej witryny. Master.</span><span class="sxs-lookup"><span data-stu-id="55d23-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="55d23-114">Jak pokazano wcześniej, kontroler ShoppingCart zawiera metodę akcji CartSummary, która zwraca widok częściowy:</span><span class="sxs-lookup"><span data-stu-id="55d23-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="55d23-115">Aby utworzyć widok częściowy CartSummary, kliknij prawym przyciskiem myszy folder widoki/ShoppingCart i wybierz polecenie Dodaj widok.</span><span class="sxs-lookup"><span data-stu-id="55d23-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="55d23-116">Nazwij widok CartSummary i zaznacz pole wyboru "Utwórz widok częściowy", jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="55d23-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="55d23-117">Widok częściowy CartSummary jest naprawdę prosty — jest to tylko link do widoku indeksu ShoppingCart, który pokazuje liczbę elementów w koszyku.</span><span class="sxs-lookup"><span data-stu-id="55d23-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="55d23-118">Pełny kod dla CartSummary. cshtml jest następujący:</span><span class="sxs-lookup"><span data-stu-id="55d23-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="55d23-119">W każdej stronie w witrynie można uwzględnić widok częściowy, w tym wzorzec witryny, przy użyciu metody html. RenderAction.</span><span class="sxs-lookup"><span data-stu-id="55d23-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="55d23-120">RenderAction wymaga, aby określić nazwę akcji ("CartSummary") i nazwę kontrolera ("ShoppingCart") w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="55d23-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="55d23-121">Przed dodaniem tego do układu lokacji utworzymy również menu gatunek, dzięki czemu będziemy mogli jednocześnie wprowadzić wszystkie nasze witryny. Master.</span><span class="sxs-lookup"><span data-stu-id="55d23-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="55d23-122">Tworzenie widoku częściowego menu gatunku</span><span class="sxs-lookup"><span data-stu-id="55d23-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="55d23-123">Możemy ułatwić naszym użytkownikom nawigowanie w sklepie przez dodanie menu gatunek, które zawiera listę wszystkich gatunków dostępnych w naszym sklepie.</span><span class="sxs-lookup"><span data-stu-id="55d23-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="55d23-124">Należy wykonać te same kroki, aby utworzyć widok częściowy GenreMenu, a następnie dodać go zarówno do wzorca lokacji.</span><span class="sxs-lookup"><span data-stu-id="55d23-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="55d23-125">Najpierw Dodaj następującą akcję kontrolera GenreMenu do StoreController:</span><span class="sxs-lookup"><span data-stu-id="55d23-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="55d23-126">Ta akcja zwraca listę gatunków, które będą wyświetlane przez widok częściowy, który zostanie utworzony dalej.</span><span class="sxs-lookup"><span data-stu-id="55d23-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="55d23-127">*Uwaga: dodaliśmy atrybut [ChildActionOnly] do tej akcji kontrolera, co oznacza, że ta akcja ma być używana tylko z widoku częściowego. Ten atrybut uniemożliwi wykonanie akcji kontrolera przez przechodzenie do/Store/GenreMenu. Nie jest to wymagane w przypadku widoków częściowych, ale jest to dobre rozwiązanie, ponieważ chcemy upewnić się, że nasze działania kontrolera są używane zgodnie z oczekiwaniami. Zwracamy również PartialView, a nie widok, dzięki czemu aparat widoku wie, że nie powinien korzystać z układu dla tego widoku, ponieważ jest uwzględniany w innych widokach.*</span><span class="sxs-lookup"><span data-stu-id="55d23-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="55d23-128">Kliknij prawym przyciskiem myszy akcję kontrolera GenreMenu i Utwórz widok częściowy o nazwie GenreMenu, który jednoznacznie wpisano przy użyciu klasy danych widoku gatunku, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="55d23-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="55d23-129">Zaktualizuj kod widoku częściowego widoku GenreMenu, aby wyświetlić elementy przy użyciu listy nieuporządkowanej w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="55d23-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="55d23-130">Aktualizowanie układu witryny w celu wyświetlenia naszych częściowych widoków</span><span class="sxs-lookup"><span data-stu-id="55d23-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="55d23-131">Możemy dodać nasze częściowe widoki do układu lokacji (/Views/Shared/\_Layout. cshtml), wywołując plik HTML. RenderAction ().</span><span class="sxs-lookup"><span data-stu-id="55d23-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="55d23-132">Dodamy je zarówno w, jak i kilka dodatkowych znaczników, aby je wyświetlić, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="55d23-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="55d23-133">Teraz po uruchomieniu aplikacji zobaczymy gatunek w lewym obszarze nawigacji i podsumowanie koszyka u góry.</span><span class="sxs-lookup"><span data-stu-id="55d23-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="55d23-134">Aktualizacja na stronie przeglądania sklepu</span><span class="sxs-lookup"><span data-stu-id="55d23-134">Update to the Store Browse page</span></span>

<span data-ttu-id="55d23-135">Strona przeglądania sklepu jest funkcjonalna, ale nie jest bardzo dobra.</span><span class="sxs-lookup"><span data-stu-id="55d23-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="55d23-136">Możemy zaktualizować stronę, aby wyświetlić albumy w lepszym układzie przez zaktualizowanie kodu widoku (znalezionego w/Views/Store/Browse.cshtml) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="55d23-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="55d23-137">Tutaj korzystamy z adresu URL. akcja, a nie HTML. ActionLink, aby można było zastosować formatowanie specjalne do linku zawierającego kompozycję albumu.</span><span class="sxs-lookup"><span data-stu-id="55d23-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="55d23-138">*Uwaga: na potrzeby tych albumów jest wyświetlana ogólna okładka albumu. Te informacje są przechowywane w bazie danych programu i można je edytować za pomocą Menedżera sklepu. Witamy w dodawaniu własnej kompozycji.*</span><span class="sxs-lookup"><span data-stu-id="55d23-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="55d23-139">Teraz po przejściu do gatunku zobaczysz albumy wyświetlane w siatce z kompozycją albumu.</span><span class="sxs-lookup"><span data-stu-id="55d23-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="55d23-140">Aktualizowanie strony głównej w celu wyświetlenia najważniejszych albumów do sprzedaży</span><span class="sxs-lookup"><span data-stu-id="55d23-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="55d23-141">Chcemy skorzystać z najpopularniejszych albumów sprzedaży na stronie głównej, aby zwiększyć sprzedaż.</span><span class="sxs-lookup"><span data-stu-id="55d23-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="55d23-142">Wprowadzimy pewne aktualizacje naszych HomeController, aby obsłużyć to i dodać również do kilku dodatkowych grafik.</span><span class="sxs-lookup"><span data-stu-id="55d23-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="55d23-143">Najpierw dodamy do naszej klasy albumu właściwość nawigacji, dzięki czemu EntityFramework wie, że są one skojarzone.</span><span class="sxs-lookup"><span data-stu-id="55d23-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="55d23-144">Ostatnie kilka wierszy naszej klasy **albumu** powinno teraz wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="55d23-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="55d23-145">*Uwaga: wymaga to dodania instrukcji using w celu przyłączenia przestrzeni nazw System. Collections. Generic.*</span><span class="sxs-lookup"><span data-stu-id="55d23-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="55d23-146">Najpierw dodamy pole storeDB oraz instrukcje MvcMusicStore. models using, jak w przypadku innych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="55d23-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="55d23-147">Następnie dodamy następującą metodę do HomeController, która wysyła zapytanie do naszej bazy danych w celu znalezienia najważniejszych albumów sprzedaży zgodnie z OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="55d23-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="55d23-148">Jest to Metoda prywatna, ponieważ nie chcemy, aby była ona dostępna jako akcja kontrolera.</span><span class="sxs-lookup"><span data-stu-id="55d23-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="55d23-149">Firma Microsoft umieszcza ją w HomeController dla uproszczenia, ale zachęcamy do przenoszenia logiki biznesowej do oddzielnych klas usługi.</span><span class="sxs-lookup"><span data-stu-id="55d23-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="55d23-150">W tym miejscu możemy zaktualizować akcję kontrolera indeksu, aby wykonać zapytanie o 5 najważniejszych albumów do sprzedaży i zwrócić je do widoku.</span><span class="sxs-lookup"><span data-stu-id="55d23-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="55d23-151">Pełny kod dla zaktualizowanego HomeController jest przedstawiony poniżej.</span><span class="sxs-lookup"><span data-stu-id="55d23-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="55d23-152">Na koniec należy zaktualizować nasz widok indeksu macierzystego, aby można było wyświetlić listę albumów, aktualizując Typ modelu i dodając listę albumów na dole.</span><span class="sxs-lookup"><span data-stu-id="55d23-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="55d23-153">Będziemy mogli również dodać do strony nagłówek i sekcję promocyjną.</span><span class="sxs-lookup"><span data-stu-id="55d23-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="55d23-154">Teraz po uruchomieniu aplikacji zobaczymy naszą zaktualizowaną stronę główną z najpopularniejszymi albumami do sprzedaży i naszym komunikatem promocyjnym.</span><span class="sxs-lookup"><span data-stu-id="55d23-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="55d23-155">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="55d23-155">Conclusion</span></span>

<span data-ttu-id="55d23-156">Wiemy, że ASP.NET MVC ułatwia tworzenie zaawansowanej witryny sieci Web z dostępem do baz danych, członkostwem, AJAX itp.</span><span class="sxs-lookup"><span data-stu-id="55d23-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="55d23-157">szybko.</span><span class="sxs-lookup"><span data-stu-id="55d23-157">pretty quickly.</span></span> <span data-ttu-id="55d23-158">Miejmy nadzieję ten samouczek zawiera narzędzia potrzebne do rozpoczęcia tworzenia własnych aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="55d23-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="55d23-159">Wstecz</span><span class="sxs-lookup"><span data-stu-id="55d23-159">Previous</span></span>](mvc-music-store-part-9.md)
