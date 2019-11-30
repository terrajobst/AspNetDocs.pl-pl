---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Tworzenie niestandardowych pomocników HTML (VB) | Microsoft Docs
author: microsoft
description: Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC. Dzięki wykorzystaniu pomocnika HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: aaeadde258a2855343a5bfb1e5ee76000e04f6bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593877"
---
# <a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="51276-104">Tworzenie niestandardowych pomocników HTML (VB)</span><span class="sxs-lookup"><span data-stu-id="51276-104">Creating Custom HTML Helpers (VB)</span></span>

<span data-ttu-id="51276-105">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="51276-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="51276-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="51276-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="51276-107">Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC.</span><span class="sxs-lookup"><span data-stu-id="51276-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="51276-108">Korzystając z zalet pomocników HTML, można zmniejszyć liczbę żmudnymych tagów HTML, które należy wykonać, aby utworzyć standardową stronę HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="51276-109">Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC.</span><span class="sxs-lookup"><span data-stu-id="51276-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="51276-110">Korzystając z zalet pomocników HTML, można zmniejszyć liczbę żmudnymych tagów HTML, które należy wykonać, aby utworzyć standardową stronę HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="51276-111">W pierwszej części tego samouczka opisano niektóre z istniejących pomocników HTML dostępnych w strukturze ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="51276-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="51276-112">Następnie opisano dwie metody tworzenia niestandardowych pomocników HTML: wyjaśniono sposób tworzenia niestandardowych pomocników HTML przez utworzenie metody udostępnionej i utworzenie metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="51276-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="51276-113">Zrozumienie pomocników HTML</span><span class="sxs-lookup"><span data-stu-id="51276-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="51276-114">Pomocnik HTML to tylko Metoda zwracająca ciąg.</span><span class="sxs-lookup"><span data-stu-id="51276-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="51276-115">Ciąg może reprezentować dowolny typ zawartości.</span><span class="sxs-lookup"><span data-stu-id="51276-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="51276-116">Można na przykład użyć pomocników HTML do renderowania standardowych tagów HTML, takich jak tagi HTML `<input>` i `<img>`.</span><span class="sxs-lookup"><span data-stu-id="51276-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="51276-117">Można również użyć pomocników HTML do renderowania bardziej złożonej zawartości, takiej jak pasek kart lub tabela HTML danych bazy danych.</span><span class="sxs-lookup"><span data-stu-id="51276-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="51276-118">Struktura ASP.NET MVC obejmuje następujący zestaw standardowych pomocników HTML (to nie jest kompletna lista):</span><span class="sxs-lookup"><span data-stu-id="51276-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="51276-119">HTML. ActionLink ()</span><span class="sxs-lookup"><span data-stu-id="51276-119">Html.ActionLink()</span></span>
- <span data-ttu-id="51276-120">HTML. BeginForm ()</span><span class="sxs-lookup"><span data-stu-id="51276-120">Html.BeginForm()</span></span>
- <span data-ttu-id="51276-121">HTML. CheckBox ()</span><span class="sxs-lookup"><span data-stu-id="51276-121">Html.CheckBox()</span></span>
- <span data-ttu-id="51276-122">HTML. DropDownList ()</span><span class="sxs-lookup"><span data-stu-id="51276-122">Html.DropDownList()</span></span>
- <span data-ttu-id="51276-123">HTML. EndForm ()</span><span class="sxs-lookup"><span data-stu-id="51276-123">Html.EndForm()</span></span>
- <span data-ttu-id="51276-124">HTML. Hidden ()</span><span class="sxs-lookup"><span data-stu-id="51276-124">Html.Hidden()</span></span>
- <span data-ttu-id="51276-125">HTML. ListBox ()</span><span class="sxs-lookup"><span data-stu-id="51276-125">Html.ListBox()</span></span>
- <span data-ttu-id="51276-126">HTML. Password ()</span><span class="sxs-lookup"><span data-stu-id="51276-126">Html.Password()</span></span>
- <span data-ttu-id="51276-127">HTML. RadioButton ()</span><span class="sxs-lookup"><span data-stu-id="51276-127">Html.RadioButton()</span></span>
- <span data-ttu-id="51276-128">HTML. TextArea ()</span><span class="sxs-lookup"><span data-stu-id="51276-128">Html.TextArea()</span></span>
- <span data-ttu-id="51276-129">HTML. TextBox ()</span><span class="sxs-lookup"><span data-stu-id="51276-129">Html.TextBox()</span></span>

<span data-ttu-id="51276-130">Rozważmy na przykład formularz z listą 1.</span><span class="sxs-lookup"><span data-stu-id="51276-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="51276-131">Ten formularz jest renderowany przy użyciu dwóch standardowych pomocników HTML (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="51276-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="51276-132">Ten formularz używa metod pomocniczych `Html.BeginForm()` i `Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="51276-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>

<span data-ttu-id="51276-133">[Strona ![renderowana z pomocnikami HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="51276-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="51276-134">**Ilustracja 01**: Strona renderowana za pomocą pomocników HTML ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="51276-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>

<span data-ttu-id="51276-135">**Lista 1 — `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="51276-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="51276-136">Metoda pomocnika `Html.BeginForm()` służy do tworzenia tagów otwierającego i zamykającego `<form>` HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="51276-137">Zauważ, że metoda `Html.BeginForm()` jest wywoływana w instrukcji using.</span><span class="sxs-lookup"><span data-stu-id="51276-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="51276-138">Instrukcja using gwarantuje, że tag `<form>` zostanie zamknięty na końcu bloku using.</span><span class="sxs-lookup"><span data-stu-id="51276-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="51276-139">Jeśli wolisz, zamiast tworzyć blok using, możesz wywołać metodę pomocnika html. EndForm (), aby zamknąć tag `<form>`.</span><span class="sxs-lookup"><span data-stu-id="51276-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="51276-140">Użyj niezależnie od tego, jak utworzysz otwierający i zamykający tag `<form>`, który jest najbardziej intuicyjny.</span><span class="sxs-lookup"><span data-stu-id="51276-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="51276-141">Metody pomocnika `Html.TextBox()` są używane na liście 1 w celu renderowania tagów `<input>` HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="51276-142">Jeśli wybierzesz opcję Wyświetl źródło w przeglądarce, zobaczysz źródło HTML na liście 2.</span><span class="sxs-lookup"><span data-stu-id="51276-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="51276-143">Należy zauważyć, że źródło zawiera standardowe Tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="51276-144">Zwróć uwagę, że pomocnik `Html.TextBox()`-HTML jest renderowany przy użyciu tagów `<%= %>` zamiast tagów `<% %>`.</span><span class="sxs-lookup"><span data-stu-id="51276-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="51276-145">Jeśli nie dołączysz znaku równości, nic nie zostanie renderowane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="51276-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="51276-146">Struktura ASP.NET MVC zawiera niewielki zestaw pomocników.</span><span class="sxs-lookup"><span data-stu-id="51276-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="51276-147">Najprawdopodobniej musisz zwiększyć strukturę MVC przy użyciu niestandardowych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="51276-148">W pozostałej części tego samouczka nauczysz się na dwie metody tworzenia niestandardowych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="51276-149">**Lista 2 — `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="51276-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="51276-150">Tworzenie pomocników HTML przy użyciu metod wspólnych</span><span class="sxs-lookup"><span data-stu-id="51276-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="51276-151">Najprostszym sposobem utworzenia nowego pomocnika HTML jest utworzenie metody udostępnionej, która zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="51276-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="51276-152">Załóżmy na przykład, że użytkownik zdecyduje się na utworzenie nowego pomocnika HTML, który renderuje tag HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="51276-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="51276-153">Aby renderować `<label>`, można użyć klasy z listy 2.</span><span class="sxs-lookup"><span data-stu-id="51276-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="51276-154">**Lista 2 — `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="51276-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="51276-155">Nie ma żadnych specjalnych informacji o klasie w liście 2.</span><span class="sxs-lookup"><span data-stu-id="51276-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="51276-156">Metoda `Label()` po prostu zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="51276-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="51276-157">Widok zmodyfikowany indeks w liście 3 używa `LabelHelper` do renderowania tagów `<label>` HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="51276-158">Należy zauważyć, że widok zawiera dyrektywę `<%@ imports %>`, która importuje przestrzeń nazw Application1. helps.</span><span class="sxs-lookup"><span data-stu-id="51276-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="51276-159">**Lista 2 — `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="51276-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="51276-160">Tworzenie pomocników HTML przy użyciu metod rozszerzających</span><span class="sxs-lookup"><span data-stu-id="51276-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="51276-161">Jeśli chcesz utworzyć pomocników HTML, które działają podobnie jak standardowe pomocniki HTML zawarte w środowisku ASP.NET MVC, należy utworzyć metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="51276-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="51276-162">Metody rozszerzające umożliwiają dodawanie nowych metod do istniejącej klasy.</span><span class="sxs-lookup"><span data-stu-id="51276-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="51276-163">Podczas tworzenia metody pomocnika HTML należy dodać nowe metody do klasy `HtmlHelper` reprezentowanej przez właściwość HTML widoku.</span><span class="sxs-lookup"><span data-stu-id="51276-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="51276-164">Moduł Visual Basic na liście 3 dodaje metodę rozszerzenia o nazwie `Label()` do klasy `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="51276-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="51276-165">Istnieje kilka rzeczy, które należy zauważyć w przypadku tego modułu.</span><span class="sxs-lookup"><span data-stu-id="51276-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="51276-166">Najpierw należy zauważyć, że moduł ma atrybut `<Extension()>`.</span><span class="sxs-lookup"><span data-stu-id="51276-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="51276-167">Aby można było użyć tego atrybutu, należy zaimportować przestrzeń nazw `System.Runtime.CompilerServices`</span><span class="sxs-lookup"><span data-stu-id="51276-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="51276-168">Następnie należy zauważyć, że pierwszy parametr metody `Label()` reprezentuje klasę `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="51276-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="51276-169">Pierwszy parametr metody rozszerzenia wskazuje klasę, którą rozszerza Metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="51276-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="51276-170">**Lista 3 — `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="51276-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="51276-171">Po utworzeniu metody rozszerzenia i pomyślnym skompilowaniu aplikacji Metoda rozszerzenia jest wyświetlana w Visual Studio IntelliSense, podobnie jak wszystkie inne metody klasy (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="51276-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="51276-172">Jedyną różnicą jest to, że metody rozszerzające są wyświetlane z symbolem specjalnym obok nich (ikona strzałki w dół).</span><span class="sxs-lookup"><span data-stu-id="51276-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="51276-173">[![przy użyciu metody rozszerzenia html. Label ()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="51276-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="51276-174">**Ilustracja 02**. użycie metody rozszerzenia html. Label () ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="51276-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>

<span data-ttu-id="51276-175">Zmodyfikowany widok indeksu w liście 4 używa metody rozszerzenia html. Label () w celu renderowania całej etykiety &lt;&gt; tagów.</span><span class="sxs-lookup"><span data-stu-id="51276-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="51276-176">**Lista 4 — `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="51276-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="51276-177">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="51276-177">Summary</span></span>

<span data-ttu-id="51276-178">W tym samouczku przedstawiono dwie metody tworzenia niestandardowych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="51276-179">Najpierw wiesz już, jak utworzyć niestandardowy pomocnik HTML `Label()`, tworząc metodę udostępnioną, która zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="51276-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="51276-180">Następnie dowiesz się, jak utworzyć niestandardową metodę pomocnika HTML `Label()`, tworząc metodę rozszerzenia klasy `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="51276-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="51276-181">W tym samouczku koncentrujemy się na tworzeniu bardzo prostej metody pomocnika HTML.</span><span class="sxs-lookup"><span data-stu-id="51276-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="51276-182">Należy pamiętać, że pomocnik HTML może być tak skomplikowany, jak chcesz.</span><span class="sxs-lookup"><span data-stu-id="51276-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="51276-183">Możesz tworzyć pomocników HTML, którzy renderują zawartość rozbudowaną, taką jak widoki drzewa, menu lub tabele danych bazy danych.</span><span class="sxs-lookup"><span data-stu-id="51276-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="51276-184">[Poprzednie](asp-net-mvc-views-overview-vb.md)
> [dalej](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="51276-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
