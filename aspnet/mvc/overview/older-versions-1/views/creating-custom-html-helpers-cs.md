---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Tworzenie niestandardowych pomocników HTML (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC. Dzięki wykorzystaniu pomocnika kodu HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 23741d7974713102e6ccb46ced5d62ec202505e8
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400857"
---
# <a name="creating-custom-html-helpers-c"></a><span data-ttu-id="20f09-104">Tworzenie niestandardowych pomocników HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="20f09-104">Creating Custom HTML Helpers (C#)</span></span>

<span data-ttu-id="20f09-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="20f09-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="20f09-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="20f09-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="20f09-107">Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="20f09-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="20f09-108">Dzięki wykorzystaniu pomocników HTML, można zmniejszyć ilość tedious wpisywania tagów HTML, czy należy wykonać, aby utworzyć standardowej strony HTML.</span><span class="sxs-lookup"><span data-stu-id="20f09-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="20f09-109">Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="20f09-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="20f09-110">Dzięki wykorzystaniu pomocników HTML, można zmniejszyć ilość tedious wpisywania tagów HTML, czy należy wykonać, aby utworzyć standardowej strony HTML.</span><span class="sxs-lookup"><span data-stu-id="20f09-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="20f09-111">W pierwszej części tego samouczka I opisano niektóre istniejące pomocników HTML dołączone do struktury ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="20f09-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="20f09-112">Następnie I opisano tworzenie niestandardowych pomocników HTML na dwa sposoby: Opisano sposób tworzenia niestandardowych pomocników HTML, tworząc metody statycznej i tworząc metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="20f09-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="20f09-113">Opis pomocników HTML</span><span class="sxs-lookup"><span data-stu-id="20f09-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="20f09-114">Pomocnik kodu HTML jest po prostu metody, która zwraca wartość typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="20f09-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="20f09-115">Ciąg może reprezentować dowolnego typu zawartości.</span><span class="sxs-lookup"><span data-stu-id="20f09-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="20f09-116">Na przykład, można użyć pomocników HTML do renderowania standardowych znaczników HTML, takich jak HTML `<input>` i `<img>` tagów.</span><span class="sxs-lookup"><span data-stu-id="20f09-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="20f09-117">Możesz również użyć pomocników HTML do renderowania zawartości bardziej złożonych, takie jak pasek kart lub tabeli HTML danych bazy danych.</span><span class="sxs-lookup"><span data-stu-id="20f09-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="20f09-118">Platforma ASP.NET MVC zawiera następujący zestaw standardowych pomocników HTML (nie jest to pełna lista):</span><span class="sxs-lookup"><span data-stu-id="20f09-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="20f09-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="20f09-119">Html.ActionLink()</span></span>
- <span data-ttu-id="20f09-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="20f09-120">Html.BeginForm()</span></span>
- <span data-ttu-id="20f09-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="20f09-121">Html.CheckBox()</span></span>
- <span data-ttu-id="20f09-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="20f09-122">Html.DropDownList()</span></span>
- <span data-ttu-id="20f09-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="20f09-123">Html.EndForm()</span></span>
- <span data-ttu-id="20f09-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="20f09-124">Html.Hidden()</span></span>
- <span data-ttu-id="20f09-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="20f09-125">Html.ListBox()</span></span>
- <span data-ttu-id="20f09-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="20f09-126">Html.Password()</span></span>
- <span data-ttu-id="20f09-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="20f09-127">Html.RadioButton()</span></span>
- <span data-ttu-id="20f09-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="20f09-128">Html.TextArea()</span></span>
- <span data-ttu-id="20f09-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="20f09-129">Html.TextBox()</span></span>

<span data-ttu-id="20f09-130">Rozważmy na przykład formularz w ofercie 1.</span><span class="sxs-lookup"><span data-stu-id="20f09-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="20f09-131">Ta forma jest renderowany przy pomocy dwóch standardowa pomocników HTML (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="20f09-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="20f09-132">Ten formularz używa `Html.BeginForm()` i `Html.TextBox()` metody pomocnika do renderowania prostego formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="20f09-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


[![P<span data-ttu-id="20f09-133">wiek, renderowane przy użyciu pomocników HTML]</span><span class="sxs-lookup"><span data-stu-id="20f09-133">age rendered with HTML Helpers]</span></span>(creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

<span data-ttu-id="20f09-134">**Rysunek 01**: Strony renderowane przy użyciu pomocników HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="20f09-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


**<span data-ttu-id="20f09-135">1 — Lista</span><span class="sxs-lookup"><span data-stu-id="20f09-135">Listing 1 –</span></span> `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="20f09-136">Metoda pomocnika Html.BeginForm() służy do utworzenia pliku HTML otwierający i zamykający `<form>` tagów.</span><span class="sxs-lookup"><span data-stu-id="20f09-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="20f09-137">Należy zauważyć, że `Html.BeginForm()` metoda jest wywoływana w obrębie za pomocą instrukcji.</span><span class="sxs-lookup"><span data-stu-id="20f09-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="20f09-138">Za pomocą instrukcji zapewnia, że `<form>` tag zostanie zamknięty na końcu używając bloku.</span><span class="sxs-lookup"><span data-stu-id="20f09-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="20f09-139">Jeśli wolisz, zamiast tworzyć za pomocą bloku, można wywołać metodę pomocnika Html.EndForm(), aby zamknąć `<form>` tagu.</span><span class="sxs-lookup"><span data-stu-id="20f09-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="20f09-140">Użyj jednego z tych podejście do tworzenia otwierający i zamykający `<form>` tag, który wydaje się być najbardziej intuicyjnym do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="20f09-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="20f09-141">`Html.TextBox()` Metody pomocnika służą do renderowania elementów HTML w ofercie 1 `<input>` tagów.</span><span class="sxs-lookup"><span data-stu-id="20f09-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="20f09-142">Jeśli wybierzesz Wyświetl źródło w przeglądarce zobaczysz do źródła HTML w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="20f09-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="20f09-143">Należy zauważyć, że źródłowa zawiera standardowych znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="20f09-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="20f09-144">Należy zauważyć, że `Html.TextBox()`— HTML pomocnika jest renderowany przy użyciu `<%= %>` tagów zamiast `<% %>` tagów.</span><span class="sxs-lookup"><span data-stu-id="20f09-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="20f09-145">Jeśli nie dołączysz znaku równości, następnie nic nie pobiera renderowane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="20f09-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="20f09-146">Platforma ASP.NET MVC zawiera niewielki zestaw pomocników.</span><span class="sxs-lookup"><span data-stu-id="20f09-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="20f09-147">Prawdopodobnie należy rozszerzyć platformę MVC za pomocą niestandardowych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="20f09-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="20f09-148">W pozostałej części tego samouczka dowiesz się, tworzenie niestandardowych pomocników HTML na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="20f09-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

**<span data-ttu-id="20f09-149">2 — Lista</span><span class="sxs-lookup"><span data-stu-id="20f09-149">Listing 2 –</span></span> `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="20f09-150">Tworzenie pomocników HTML za pomocą metod statycznych</span><span class="sxs-lookup"><span data-stu-id="20f09-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="20f09-151">Najprostszym sposobem utworzenia nowego pomocnika kodu HTML jest Tworzenie statycznej metody, która zwraca wartość typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="20f09-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="20f09-152">Wyobraź sobie, na przykład zdecydujesz utworzyć nowego pomocnika HTML, który powoduje wyświetlenie kodu HTML `<label>` tagu.</span><span class="sxs-lookup"><span data-stu-id="20f09-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="20f09-153">Można użyć klasy w ofercie 2 do renderowania `<label>` .</span><span class="sxs-lookup"><span data-stu-id="20f09-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

**<span data-ttu-id="20f09-154">2 — Lista</span><span class="sxs-lookup"><span data-stu-id="20f09-154">Listing 2 –</span></span> `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="20f09-155">Nie ma nic specjalnego informacje o klasie w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="20f09-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="20f09-156">`Label()` Metoda po prostu zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="20f09-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="20f09-157">Używa zmodyfikowanego widoku indeksu w ofercie 3 `LabelHelper` do renderowania elementów HTML `<label>` tagów.</span><span class="sxs-lookup"><span data-stu-id="20f09-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="20f09-158">Należy zauważyć, że widok zawiera `<%@ imports %>` dyrektywę, który importuje `Application1.Helpers` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="20f09-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

**<span data-ttu-id="20f09-159">2 — Lista</span><span class="sxs-lookup"><span data-stu-id="20f09-159">Listing 2 –</span></span> `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="20f09-160">Tworzenie pomocników HTML przy użyciu metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="20f09-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="20f09-161">Jeśli chcesz tworzyć pomocników HTML, które działają podobnie, takich jak standardowy pomocników HTML zawarte w platformę ASP.NET MVC, a następnie należy utworzyć metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="20f09-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="20f09-162">Metody rozszerzające umożliwiają dodawanie nowych metod do istniejącej klasy.</span><span class="sxs-lookup"><span data-stu-id="20f09-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="20f09-163">Podczas tworzenia metodę pomocnika kodu HTML, należy dodać nowe metody do klasy HtmlHelper, reprezentowane przez właściwości Html widoku.</span><span class="sxs-lookup"><span data-stu-id="20f09-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="20f09-164">Ta klasa w ofercie 3 dodaje metodę rozszerzenia, aby `HtmlHelper` klasę o nazwie `Label()`.</span><span class="sxs-lookup"><span data-stu-id="20f09-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="20f09-165">Istnieje kilka rzeczy, które użytkownik zauważy o tej klasie.</span><span class="sxs-lookup"><span data-stu-id="20f09-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="20f09-166">Najpierw zwróć uwagę, że klasa jest klasą statyczną.</span><span class="sxs-lookup"><span data-stu-id="20f09-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="20f09-167">Należy zdefiniować metodę rozszerzającą o klasie statycznej.</span><span class="sxs-lookup"><span data-stu-id="20f09-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="20f09-168">Po drugie, zwróć uwagę, że pierwszy parametr `Label()` metody jest poprzedzone słowem kluczowym `this`.</span><span class="sxs-lookup"><span data-stu-id="20f09-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="20f09-169">Pierwszy parametr metody rozszerzenia wskazuje klasę, która rozszerza metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="20f09-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

**<span data-ttu-id="20f09-170">3 — lista</span><span class="sxs-lookup"><span data-stu-id="20f09-170">Listing 3 –</span></span> `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="20f09-171">Po utworzeniu metodę rozszerzenia, a następnie skompilować aplikację pomyślnym, metoda rozszerzenia pojawia się w Visual Studio technologii Intellisense, podobnie jak wszystkie inne metody klasy (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="20f09-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="20f09-172">Jedyną różnicą jest to rozszerzenie, które metody są wyświetlane z symbolem specjalne obok nich (ikona strzałki w dół).</span><span class="sxs-lookup"><span data-stu-id="20f09-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


[![U<span data-ttu-id="20f09-173">SING — metoda rozszerzenia Html.Label()]</span><span class="sxs-lookup"><span data-stu-id="20f09-173">sing the Html.Label() extension method]</span></span>(creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

<span data-ttu-id="20f09-174">**Rysunek 02**: Przy użyciu metody rozszerzenia Html.Label() ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="20f09-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="20f09-175">Zmodyfikowany widok indeksu w ofercie 4 używa Html.Label() metody rozszerzenia do renderowania, wszystkie jego `<label>` tagów.</span><span class="sxs-lookup"><span data-stu-id="20f09-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

**<span data-ttu-id="20f09-176">4 — lista</span><span class="sxs-lookup"><span data-stu-id="20f09-176">Listing 4 –</span></span> `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="20f09-177">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="20f09-177">Summary</span></span>

<span data-ttu-id="20f09-178">W tym samouczku przedstawiono tworzenie niestandardowych pomocników HTML na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="20f09-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="20f09-179">Po pierwsze, wiesz, jak utworzyć niestandardową `Label()` pomocnika kodu HTML, tworząc statycznej metody, która zwraca wartość typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="20f09-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="20f09-180">Następnie pokazano, jak utworzyć niestandardową `Label()` metody pomocnika kodu HTML, tworząc metodę rozszerzającą o `HtmlHelper` klasy.</span><span class="sxs-lookup"><span data-stu-id="20f09-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="20f09-181">W tym samouczku I koncentruje się na tworzeniu bardzo prostą metodę pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="20f09-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="20f09-182">Należy pamiętać, że pomocnika kodu HTML, może być tak skomplikowane, jak chcesz.</span><span class="sxs-lookup"><span data-stu-id="20f09-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="20f09-183">Można tworzyć pomocników HTML, renderowanie sformatowanej zawartości, takich jak widoki drzewa i menu tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="20f09-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="20f09-184">[Poprzednie](asp-net-mvc-views-overview-cs.md)
> [dalej](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="20f09-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
