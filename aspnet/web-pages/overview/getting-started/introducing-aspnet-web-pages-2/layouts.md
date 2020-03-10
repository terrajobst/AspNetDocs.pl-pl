---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Wprowadzenie do stron sieci Web ASP.NET — tworzenie spójnego układu | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak używać układów do tworzenia spójnego wyglądu stron w witrynie, która używa stron sieci Web ASP.NET. Przyjęto założenie, że ukończono...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526692"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="8886e-104">Wprowadzenie do stron sieci Web ASP.NET — tworzenie spójnego układu</span><span class="sxs-lookup"><span data-stu-id="8886e-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>

<span data-ttu-id="8886e-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8886e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8886e-106">W tym samouczku pokazano, jak używać *układów* do tworzenia spójnego wyglądu stron w witrynie, która używa stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8886e-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="8886e-107">Przyjęto założenie, że ukończono serię przez [usunięcie danych bazy danych na stronach sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="8886e-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="8886e-108">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="8886e-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8886e-109">Co to jest strona układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-109">What a layout page is.</span></span>
> - <span data-ttu-id="8886e-110">Jak połączyć strony układu z zawartością dynamiczną.</span><span class="sxs-lookup"><span data-stu-id="8886e-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="8886e-111">Jak przekazać wartości do strony układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-111">How to pass values to a layout page.</span></span>

## <a name="about-layouts"></a><span data-ttu-id="8886e-112">Układy — informacje</span><span class="sxs-lookup"><span data-stu-id="8886e-112">About Layouts</span></span>

<span data-ttu-id="8886e-113">Wszystkie utworzone przez siebie strony zostały ukończone, autonomiczne strony.</span><span class="sxs-lookup"><span data-stu-id="8886e-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="8886e-114">Wszystkie należą do tej samej lokacji, ale nie mają żadnych wspólnych elementów ani standardowego wyglądu.</span><span class="sxs-lookup"><span data-stu-id="8886e-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="8886e-115">Większość lokacji ma spójny wygląd i układ.</span><span class="sxs-lookup"><span data-stu-id="8886e-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="8886e-116">Na przykład jeśli przejdziesz do witryny [Microsoft.com/Web](https://www.microsoft.com/web/) i zobaczysz, że wszystkie strony są zgodne z ogólnym układem i motywem wizualnym:</span><span class="sxs-lookup"><span data-stu-id="8886e-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Strona witryny Microsoft.com/web przedstawiająca układ nagłówka, obszaru nawigacji, obszaru zawartości i stopki](layouts/_static/image1.png)

<span data-ttu-id="8886e-118">*Nieefektywnym* sposobem tworzenia tego układu będzie Definiowanie nagłówka, paska nawigacyjnego i stopki oddzielnie na każdej stronie.</span><span class="sxs-lookup"><span data-stu-id="8886e-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="8886e-119">Za każdym razem duplikujesz te same znaczniki.</span><span class="sxs-lookup"><span data-stu-id="8886e-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="8886e-120">Jeśli chcesz zmienić coś (na przykład zaktualizować stopkę), musisz oddzielnie zmienić każdą stronę.</span><span class="sxs-lookup"><span data-stu-id="8886e-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="8886e-121">Jest to miejsce, w którym są dostępne *strony układu* .</span><span class="sxs-lookup"><span data-stu-id="8886e-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="8886e-122">Na stronach sieci Web ASP.NET można zdefiniować stronę układu, która zawiera ogólny kontener dla stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="8886e-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="8886e-123">Na przykład strona układu może zawierać nagłówek, obszar nawigacji i stopkę.</span><span class="sxs-lookup"><span data-stu-id="8886e-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="8886e-124">Strona układu zawiera symbol zastępczy, w którym znajduje się główna Zawartość.</span><span class="sxs-lookup"><span data-stu-id="8886e-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="8886e-125">Następnie można zdefiniować pojedyncze strony zawartości zawierające znaczniki i kod tylko dla tej strony.</span><span class="sxs-lookup"><span data-stu-id="8886e-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="8886e-126">Strony zawartości nie muszą być kompletnymi stronami HTML; nie muszą oni nawet mieć elementu `<body>`.</span><span class="sxs-lookup"><span data-stu-id="8886e-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="8886e-127">Mają także wiersz kodu, który informuje ASP.NET, na której stronie układu ma być wyświetlana zawartość.</span><span class="sxs-lookup"><span data-stu-id="8886e-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="8886e-128">Oto ilustracja przedstawiająca sposób działania tej relacji:</span><span class="sxs-lookup"><span data-stu-id="8886e-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagram koncepcyjny pokazujący dwie strony zawartości i stronę układu, do których one pasują.](layouts/_static/image2.png)

<span data-ttu-id="8886e-130">Ta interakcja jest łatwa do zrozumienia, gdy zobaczysz ją w działaniu.</span><span class="sxs-lookup"><span data-stu-id="8886e-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="8886e-131">W tym samouczku zmienisz strony filmów do korzystania z układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="8886e-132">Dodawanie strony układu</span><span class="sxs-lookup"><span data-stu-id="8886e-132">Adding a Layout Page</span></span>

<span data-ttu-id="8886e-133">Zacznij od utworzenia strony układu, która definiuje typowy układ strony z nagłówkiem, stopką i obszarem zawartości głównej.</span><span class="sxs-lookup"><span data-stu-id="8886e-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="8886e-134">W witrynie WebPagesMovies Dodaj stronę CSHTML o nazwie *\_Layout. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8886e-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="8886e-135">Znak podkreślenia wiodącego (`_`) jest znaczący.</span><span class="sxs-lookup"><span data-stu-id="8886e-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="8886e-136">Jeśli nazwa strony rozpoczyna się od znaku podkreślenia, ASP.NET nie będzie bezpośrednio wysyłać tej strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8886e-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="8886e-137">Ta konwencja umożliwia zdefiniowanie stron, które są wymagane dla danej witryny, ale nie ma możliwości bezpośredniego żądania.</span><span class="sxs-lookup"><span data-stu-id="8886e-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="8886e-138">Zastąp zawartość na stronie następującym:</span><span class="sxs-lookup"><span data-stu-id="8886e-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="8886e-139">Jak widać, ten znacznik jest tylko HTML, który używa elementów `<div>` do definiowania trzech sekcji na stronie oraz jednego elementu `<div>`, aby przechowywać trzy sekcje.</span><span class="sxs-lookup"><span data-stu-id="8886e-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="8886e-140">Stopka zawiera bit kodu Razor: `@DateTime.Now.Year`, który będzie renderować bieżący rok w tej lokalizacji na stronie.</span><span class="sxs-lookup"><span data-stu-id="8886e-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="8886e-141">Zwróć uwagę, że istnieje link do arkusza stylów o nazwie *filmy. css*.</span><span class="sxs-lookup"><span data-stu-id="8886e-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="8886e-142">Arkusz stylów to miejsce, w którym zostaną zdefiniowane szczegóły układu fizycznego elementów.</span><span class="sxs-lookup"><span data-stu-id="8886e-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="8886e-143">W tej chwili utworzysz ten element.</span><span class="sxs-lookup"><span data-stu-id="8886e-143">You'll create that in a moment.</span></span>

<span data-ttu-id="8886e-144">Jedyną nietypową funkcją na tej *\_stronie Layout. cshtml* jest wiersz `@Render.Body()`.</span><span class="sxs-lookup"><span data-stu-id="8886e-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="8886e-145">Jest to symbol zastępczy, w którym zostanie wystawiona zawartość, gdy ten układ zostanie scalony z inną stroną.</span><span class="sxs-lookup"><span data-stu-id="8886e-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="8886e-146">Dodawanie pliku. CSS</span><span class="sxs-lookup"><span data-stu-id="8886e-146">Adding a .css File</span></span>

<span data-ttu-id="8886e-147">Preferowanym sposobem zdefiniowania rzeczywistego układu (czyli wyglądu) elementów na stronie jest użycie reguł kaskadowego arkusza stylów (CSS).</span><span class="sxs-lookup"><span data-stu-id="8886e-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="8886e-148">Utwórz plik *CSS* , który zawiera reguły dla nowego układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="8886e-149">W programie WebMatrix wybierz katalog główny witryny.</span><span class="sxs-lookup"><span data-stu-id="8886e-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="8886e-150">Następnie na karcie **pliki** wstążki kliknij strzałkę pod przyciskiem **Nowy** , a następnie kliknij pozycję **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="8886e-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Opcja "nowy folder" w obszarze nowy na Wstążce.](layouts/_static/image3.png)

<span data-ttu-id="8886e-152">Nazwij nowe *Style*folderów.</span><span class="sxs-lookup"><span data-stu-id="8886e-152">Name the new folder *Styles*.</span></span>

![Nazywanie nowego folderu "Style"](layouts/_static/image4.png)

<span data-ttu-id="8886e-154">W folderze nowe *Style* Utwórz plik o nazwie *filmy. css*.</span><span class="sxs-lookup"><span data-stu-id="8886e-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Tworzenie nowych filmów plik CSS](layouts/_static/image5.png)

<span data-ttu-id="8886e-156">Zastąp zawartość nowego pliku *. css* następującym:</span><span class="sxs-lookup"><span data-stu-id="8886e-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="8886e-157">Nie będziemy mówić więcej o tych regułach CSS, z wyjątkiem dwóch rzeczy.</span><span class="sxs-lookup"><span data-stu-id="8886e-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="8886e-158">Jest to, że oprócz ustawiania czcionek i rozmiarów, reguły używają pozycjonowania bezwzględnego do ustalenia lokalizacji nagłówka, stopki i głównego obszaru zawartości.</span><span class="sxs-lookup"><span data-stu-id="8886e-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="8886e-159">Jeśli dopiero zaczynasz pozycjonować w CSS, możesz przeczytać samouczek dotyczący [pozycjonowania CSS](http://www.w3schools.com/css/css_positioning.asp) w witrynie w3schools.</span><span class="sxs-lookup"><span data-stu-id="8886e-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="8886e-160">Druga należy zwrócić uwagę na to, że na dole zostały skopiowane reguły stylu, które zostały pierwotnie zdefiniowane w pliku *Films. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="8886e-161">Te reguły zostały użyte we [wprowadzeniu do wyświetlania danych przy użyciu samouczka ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) , aby uczynić pomocnika `WebGrid` renderować znaczniki dodawane do tabeli.</span><span class="sxs-lookup"><span data-stu-id="8886e-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="8886e-162">(Jeśli zamierzasz użyć pliku *. css* do definiowania definicji stylu, możesz również ustawić reguły stylu dla całej witryny).</span><span class="sxs-lookup"><span data-stu-id="8886e-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="8886e-163">Aktualizowanie pliku filmów do korzystania z układu</span><span class="sxs-lookup"><span data-stu-id="8886e-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="8886e-164">Teraz można zaktualizować istniejące pliki w lokacji, aby używały nowego układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="8886e-165">Otwórz plik *Films. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="8886e-166">W górnej części, jako pierwszy wiersz kodu, Dodaj następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="8886e-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="8886e-167">Strona zostanie teraz uruchomiona w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8886e-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="8886e-168">Ten wiersz kodu instruuje ASP.NET, że gdy strona *filmów* zostanie uruchomiona, należy ją scalić z plikiem *\_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="8886e-169">Ponieważ plik *Films. cshtml* używa teraz strony układu, można usunąć znaczniki ze strony *filmy. cshtml* , która jest traktowana przez plik *\_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="8886e-170">Wypełnij `<!DOCTYPE>`, `<html>`i `<body>` otwierając i zamykając Tagi.</span><span class="sxs-lookup"><span data-stu-id="8886e-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="8886e-171">Wykorzystaj cały `<head>` element i jego zawartość, w tym reguły stylu dla siatki, ponieważ teraz masz te reguły w pliku *. css* .</span><span class="sxs-lookup"><span data-stu-id="8886e-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="8886e-172">Gdy skończysz, Zmień istniejący element `<h1>` na element `<h2>`; masz już element `<h1>` na stronie układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="8886e-173">Zmień tekst `<h2>` na "filmy lists".</span><span class="sxs-lookup"><span data-stu-id="8886e-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="8886e-174">Zwykle nie trzeba wprowadzać tych zmian na stronie zawartości.</span><span class="sxs-lookup"><span data-stu-id="8886e-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="8886e-175">Po uruchomieniu witryny za pomocą strony układu utworzysz strony zawartości bez wszystkich tych elementów, które mają zaczynać się od.</span><span class="sxs-lookup"><span data-stu-id="8886e-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="8886e-176">W tym przypadku w tym przypadku konwertujesz autonomiczną stronę na jedną, która korzysta z układu, więc istnieje trochę oczyszczania.</span><span class="sxs-lookup"><span data-stu-id="8886e-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="8886e-177">Po zakończeniu strona *filmy. cshtml* będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="8886e-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="8886e-178">Testowanie układu</span><span class="sxs-lookup"><span data-stu-id="8886e-178">Testing the Layout</span></span>

<span data-ttu-id="8886e-179">Teraz można zobaczyć, jak wygląda układ.</span><span class="sxs-lookup"><span data-stu-id="8886e-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="8886e-180">W programie WebMatrix kliknij prawym przyciskiem myszy stronę *filmy. cshtml* i wybierz polecenie **Uruchom w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="8886e-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="8886e-181">Gdy przeglądarka wyświetli stronę, będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="8886e-181">When the browser displays the page, it looks like this page:</span></span>

![Strona filmów renderowana przy użyciu układu](layouts/_static/image6.png)

<span data-ttu-id="8886e-183">ASP.NET scalono zawartość strony filmy. cshtml na stronie *\_Layout. cshtml* , w której znajduje się `RenderBody` Metoda.</span><span class="sxs-lookup"><span data-stu-id="8886e-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="8886e-184">I oczywiście strona *\_Layout. cshtml* odwołuje się do pliku *. css* , który definiuje wygląd strony.</span><span class="sxs-lookup"><span data-stu-id="8886e-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="8886e-185">Aktualizowanie strony addmovie do korzystania z układu</span><span class="sxs-lookup"><span data-stu-id="8886e-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="8886e-186">Rzeczywista korzyść dotycząca układów polega na tym, że można używać ich dla wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="8886e-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="8886e-187">Otwórz stronę *addmovie. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="8886e-188">Można pamiętać, że na stronie *addmovie. cshtml* na początku znajdują się pewne reguły CSS umożliwiające Definiowanie wyglądu komunikatów o błędach walidacji.</span><span class="sxs-lookup"><span data-stu-id="8886e-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="8886e-189">Ponieważ masz teraz plik *CSS* witryny, możesz przenieść te reguły do pliku *. css* .</span><span class="sxs-lookup"><span data-stu-id="8886e-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="8886e-190">Usuń je z pliku *addmovie. cshtml* i Dodaj je do dolnej części pliku *film. css* .</span><span class="sxs-lookup"><span data-stu-id="8886e-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="8886e-191">Przenosisz następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="8886e-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="8886e-192">Teraz należy wprowadzić takie same zmiany w pliku *addmovie. cshtml* dla *filmów. cshtml* — Dodaj `Layout="~/_Layout.cshtml;` i Usuń znacznik HTML, który jest teraz nadmiarowy.</span><span class="sxs-lookup"><span data-stu-id="8886e-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="8886e-193">Zmień element `<h1>`, aby `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="8886e-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="8886e-194">Gdy skończysz, strona będzie wyglądać podobnie do tego przykładu:</span><span class="sxs-lookup"><span data-stu-id="8886e-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="8886e-195">Uruchom stronę.</span><span class="sxs-lookup"><span data-stu-id="8886e-195">Run the page.</span></span> <span data-ttu-id="8886e-196">Teraz wygląda na to ilustracja:</span><span class="sxs-lookup"><span data-stu-id="8886e-196">Now it looks like this illustration:</span></span>

![Strona "Dodawanie filmów" renderowana przy użyciu układu](layouts/_static/image7.png)

<span data-ttu-id="8886e-198">Chcesz wprowadzić podobne zmiany na stronach w witrynie — *EditMovie. cshtml* i *DeleteMovie. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8886e-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="8886e-199">Jednak przed rozpoczęciem możesz wprowadzić kolejną zmianę w układzie, która sprawia, że jest nieco bardziej elastyczna.</span><span class="sxs-lookup"><span data-stu-id="8886e-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="8886e-200">Przekazywanie informacji o tytule do strony układu</span><span class="sxs-lookup"><span data-stu-id="8886e-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="8886e-201">Utworzona strona *\_Layout. cshtml* ma element `<title>`, który jest ustawiony na "Moja witryna filmów".</span><span class="sxs-lookup"><span data-stu-id="8886e-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="8886e-202">Większość przeglądarek wyświetla zawartość tego elementu jako tekst na karcie:</span><span class="sxs-lookup"><span data-stu-id="8886e-202">Most browsers display the content of this element as the text on a tab:</span></span>

![&lt;tytuł strony&gt; element wyświetlany na karcie przeglądarki](layouts/_static/image8.png)

<span data-ttu-id="8886e-204">Informacje o tym tytule są ogólne.</span><span class="sxs-lookup"><span data-stu-id="8886e-204">This title information is generic.</span></span> <span data-ttu-id="8886e-205">Załóżmy, że tekst tytułu ma być bardziej szczegółowy dla bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="8886e-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="8886e-206">(Tekst tytułu jest również używany przez aparaty wyszukiwania, aby określić, do czego służy strona). Można przekazać informacje ze strony zawartości, takiej jak *filmy. cshtml* lub *addmovie. cshtml* , do strony układ, a następnie użyć tych informacji do dostosowania działania renderowania strony układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="8886e-207">Otwórz ponownie stronę *filmy. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="8886e-208">W kodzie u góry Dodaj następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="8886e-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="8886e-209">Obiekt `Page` jest dostępny na wszystkich stronach *. cshtml* i jest przeznaczony do tego celu, a mianowicie do udostępniania informacji między stroną a jej układem.</span><span class="sxs-lookup"><span data-stu-id="8886e-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="8886e-210">Otwórz stronę *\_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-210">Open the *\_Layout.cshtml* page.</span></span> <span data-ttu-id="8886e-211">Zmień element `<title>` tak, aby wyglądał następująco:</span><span class="sxs-lookup"><span data-stu-id="8886e-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="8886e-212">Ten kod renderuje wszystko, co znajduje się we właściwości `Page.Title` bezpośrednio w tej lokalizacji na stronie.</span><span class="sxs-lookup"><span data-stu-id="8886e-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="8886e-213">Uruchom stronę *filmy. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="8886e-214">Tym razem karta przeglądarki pokazuje przekazaną wartość `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="8886e-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Karta przeglądarki pokazująca tytuł utworzony dynamicznie](layouts/_static/image9.png)

<span data-ttu-id="8886e-216">Jeśli chcesz, Wyświetl źródło strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8886e-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="8886e-217">Można zobaczyć, że element `<title>` jest renderowany jako `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="8886e-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="8886e-218">**Obiekt strony**</span><span class="sxs-lookup"><span data-stu-id="8886e-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="8886e-219">Przydatną funkcją `Page` jest to, że jest to obiekt dynamiczny — Właściwość `Title` nie jest nazwą stałą lub zastrzeżoną.</span><span class="sxs-lookup"><span data-stu-id="8886e-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="8886e-220">Możesz użyć *dowolnej* nazwy dla wartości `Page` obiektu.</span><span class="sxs-lookup"><span data-stu-id="8886e-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="8886e-221">Na przykład można łatwo przekazywać tytuł przy użyciu właściwości o nazwie `Page.CurrentName` lub `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="8886e-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="8886e-222">Jedynym ograniczeniem jest to, że nazwa musi być zgodna z normalnymi regułami dla właściwości, które można nazwać.</span><span class="sxs-lookup"><span data-stu-id="8886e-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="8886e-223">(Na przykład nazwa nie może zawierać spacji).</span><span class="sxs-lookup"><span data-stu-id="8886e-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="8886e-224">Za pomocą obiektu `Page` można przekazać dowolną liczbę wartości.</span><span class="sxs-lookup"><span data-stu-id="8886e-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="8886e-225">Jeśli chcesz przekazać informacje o filmie do strony układu, możesz przekazać wartości, używając takich elementów jak `Page.MovieTitle` i `Page.Genre` i `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="8886e-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="8886e-226">(Lub wszystkie inne nazwy, które zostały stworzone w celu przechowywania informacji). Jedyne wymaganie — co jest prawdopodobnie oczywiste — należy używać tych samych nazw na stronie zawartości i strony układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="8886e-227">Informacje przekazywane przy użyciu obiektu `Page` nie są ograniczone do tekstu wyświetlanego na stronie układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="8886e-228">Można przekazać wartość do strony układ, a następnie kod na stronie układu może użyć wartości, aby zdecydować, czy ma być wyświetlana sekcja strony, co plik *CSS* ma być używany i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="8886e-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="8886e-229">Wartości przekazywane w obiekcie `Page` są podobne do innych wartości, które są używane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="8886e-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="8886e-230">Tylko te wartości pochodzą ze strony zawartości i są przesyłane do strony układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>

<span data-ttu-id="8886e-231">Otwórz stronę *addmovie. cshtml* i Dodaj wiersz na początku kodu, który zawiera tytuł dla strony *addmovie. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="8886e-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="8886e-232">Uruchom stronę *addmovie. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8886e-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="8886e-233">Zobaczysz nowy tytuł:</span><span class="sxs-lookup"><span data-stu-id="8886e-233">You see the new title there:</span></span>

![Karta przeglądarki pokazująca, że tytuł "Dodaj filmy" został utworzony dynamicznie](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="8886e-235">Aktualizowanie pozostałych stron w celu korzystania z układu</span><span class="sxs-lookup"><span data-stu-id="8886e-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="8886e-236">Teraz możesz zakończyć pozostałe strony w swojej lokacji, tak aby korzystały z nowego układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="8886e-237">Otwórz *EditMovie. cshtml* i *DeleteMovie. cshtml* z kolei i wprowadź te same zmiany w każdym z nich.</span><span class="sxs-lookup"><span data-stu-id="8886e-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="8886e-238">Dodaj wiersz kodu, który łączy się ze stroną układu:</span><span class="sxs-lookup"><span data-stu-id="8886e-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="8886e-239">Dodaj wiersz, aby ustawić tytuł strony:</span><span class="sxs-lookup"><span data-stu-id="8886e-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="8886e-240">lub:</span><span class="sxs-lookup"><span data-stu-id="8886e-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="8886e-241">Usuń wszystkie nadmiarowe znaczniki HTML — w zasadzie pozostaw tylko bity znajdujące się wewnątrz elementu `<body>` (plus blok kodu u góry).</span><span class="sxs-lookup"><span data-stu-id="8886e-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="8886e-242">Zmień element `<h1>` na element `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="8886e-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="8886e-243">Po wprowadzeniu tych zmian przetestuj każdy z nich i upewnij się, że jest on prawidłowo wyświetlany i że tytuł jest poprawny.</span><span class="sxs-lookup"><span data-stu-id="8886e-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="8886e-244">Częściowe pomysły dotyczące stron układu</span><span class="sxs-lookup"><span data-stu-id="8886e-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="8886e-245">W tym samouczku utworzono stronę *\_Layout. cshtml* i użyto metody `RenderBody`, aby scalić zawartość z innej strony.</span><span class="sxs-lookup"><span data-stu-id="8886e-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="8886e-246">Jest to podstawowy wzorzec służący do używania układów na stronach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8886e-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="8886e-247">Strony układu mają dodatkowe funkcje, które nie zostały tutaj omówione.</span><span class="sxs-lookup"><span data-stu-id="8886e-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="8886e-248">Na przykład można zagnieżdżać strony układu — jedną stronę układu można z kolei zmienić.</span><span class="sxs-lookup"><span data-stu-id="8886e-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="8886e-249">Układy zagnieżdżone mogą być przydatne, jeśli pracujesz z podsekcjami lokacji, które wymagają różnych układów.</span><span class="sxs-lookup"><span data-stu-id="8886e-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="8886e-250">Możesz również użyć dodatkowych metod (na przykład `RenderSection`), aby skonfigurować nazwane sekcje na stronie układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="8886e-251">Kombinacja stron układu i plików *CSS* jest zaawansowana.</span><span class="sxs-lookup"><span data-stu-id="8886e-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="8886e-252">Jak widać w następnej serii samouczków, w programie WebMatrix można utworzyć witrynę na podstawie *szablonu*, który udostępnia lokację ze wstępnie utworzoną funkcją.</span><span class="sxs-lookup"><span data-stu-id="8886e-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="8886e-253">Szablony ułatwiają używanie stron układu i CSS do tworzenia witryn, które wyglądają doskonale i mają funkcje takie jak menu.</span><span class="sxs-lookup"><span data-stu-id="8886e-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="8886e-254">Oto zrzut ekranu strony głównej z witryny na podstawie szablonu, który pokazuje funkcje korzystające ze stron układu i CSS:</span><span class="sxs-lookup"><span data-stu-id="8886e-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Układ utworzony przez szablon witryny WebMatrix pokazujący nagłówek, obszar nawigacji, obszar zawartości, sekcję opcjonalną i linki logowania](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="8886e-256">Ukończ listę dla strony filmu (Zaktualizowano do korzystania ze strony układu)</span><span class="sxs-lookup"><span data-stu-id="8886e-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="8886e-257">Kompletna lista stron na potrzeby dodawania stron filmu (zaktualizowanych na potrzeby układu)</span><span class="sxs-lookup"><span data-stu-id="8886e-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="8886e-258">Pełna lista stron do usuwania filmów (zaktualizowanych dla układu)</span><span class="sxs-lookup"><span data-stu-id="8886e-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="8886e-259">Kompletna lista stron do edycji strony filmu (zaktualizowana do układu)</span><span class="sxs-lookup"><span data-stu-id="8886e-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="8886e-260">Przyszłe przejście</span><span class="sxs-lookup"><span data-stu-id="8886e-260">Coming Up Next</span></span>

<span data-ttu-id="8886e-261">W następnym samouczku dowiesz się, jak opublikować witrynę w Internecie, aby wszyscy mogli ją zobaczyć.</span><span class="sxs-lookup"><span data-stu-id="8886e-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8886e-262">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="8886e-262">Additional Resources</span></span>

- <span data-ttu-id="8886e-263">[Tworzenie spójnego wyglądu](https://go.microsoft.com/fwlink/?LinkID=202891) — artykułu, który zawiera bardziej szczegółowe informacje na temat pracy z układami.</span><span class="sxs-lookup"><span data-stu-id="8886e-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="8886e-264">Opisano w nim również, jak przekazać wartość do strony układu, która pokazuje lub ukrywa część zawartości.</span><span class="sxs-lookup"><span data-stu-id="8886e-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="8886e-265">[Zagnieżdżone strony układu z](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) tablicą Razor — na przykład, jak zagnieżdżać strony układu.</span><span class="sxs-lookup"><span data-stu-id="8886e-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="8886e-266">(Obejmuje pobieranie stron).</span><span class="sxs-lookup"><span data-stu-id="8886e-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8886e-267">[Poprzednie](deleting-data.md)
> [dalej](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="8886e-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
