---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Przegląd widoków ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Co to jest widok ASP.NET MVC i czym się różni od strony HTML? W tym samouczku Stephen Walther wprowadza do widoków i demonstruje, jak można t...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f02728ed248f29b09d654e509977ed43889cbb83
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541308"
---
# <a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="b2cdf-104">Omówienie widoków ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="b2cdf-104">ASP.NET MVC Views Overview (VB)</span></span>

<span data-ttu-id="b2cdf-105">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b2cdf-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b2cdf-106">Co to jest widok ASP.NET MVC i czym się różni od strony HTML?</span><span class="sxs-lookup"><span data-stu-id="b2cdf-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="b2cdf-107">W tym samouczku Stephen Walther wprowadza do widoków i demonstruje, jak można wykorzystać możliwości wyświetlania danych i pomocników HTML w widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>

<span data-ttu-id="b2cdf-108">Celem tego samouczka jest zapewnienie krótkiego wprowadzenia do ASP.NET widoków MVC, wyświetlania danych i pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="b2cdf-109">Na końcu tego samouczka należy zrozumieć, jak tworzyć nowe widoki, przekazywać dane z kontrolera do widoku i używać pomocników HTML do generowania zawartości w widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="b2cdf-110">Informacje o widokach</span><span class="sxs-lookup"><span data-stu-id="b2cdf-110">Understanding Views</span></span>

<span data-ttu-id="b2cdf-111">W przeciwieństwie do stron ASP.NET lub Active Server, ASP.NET MVC nie zawiera żadnych elementów, które bezpośrednio odnoszą się do strony.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="b2cdf-112">W aplikacji ASP.NET MVC nie istnieje strona na dysku odpowiadająca ścieżce w adresie URL, który można wpisać na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="b2cdf-113">Najbliższy element strony w aplikacji ASP.NET MVC to coś o nazwie *View*.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="b2cdf-114">W aplikacji ASP.NET MVC żądania przeglądarki przychodzącej są mapowane na akcje kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="b2cdf-115">Akcja kontrolera może zwrócić widok.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-115">A controller action might return a view.</span></span> <span data-ttu-id="b2cdf-116">Jednak akcja kontrolera może wykonać inny typ akcji, na przykład przekierowywanie do innej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="b2cdf-117">Lista 1 zawiera prosty kontroler o nazwie HomeController.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="b2cdf-118">HomeController uwidacznia dwie akcje kontrolera o nazwie index () i Details ().</span><span class="sxs-lookup"><span data-stu-id="b2cdf-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="b2cdf-119">**Lista 1 — HomeController. vb**</span><span class="sxs-lookup"><span data-stu-id="b2cdf-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="b2cdf-120">Możesz wywołać pierwszą akcję, indeks (), wpisując następujący adres URL na pasku adresu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="b2cdf-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="b2cdf-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="b2cdf-121">/Home/Index</span></span>

<span data-ttu-id="b2cdf-122">Możesz wywołać drugą akcję, szczegóły (), wpisując ten adres w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b2cdf-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="b2cdf-123">/Home/Details</span><span class="sxs-lookup"><span data-stu-id="b2cdf-123">/Home/Details</span></span>

<span data-ttu-id="b2cdf-124">Akcja index () zwraca widok.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-124">The Index() action returns a view.</span></span> <span data-ttu-id="b2cdf-125">Większość akcji, które tworzysz, będzie zwracać widoki.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-125">Most actions that you create will return views.</span></span> <span data-ttu-id="b2cdf-126">Jednak akcja może zwracać inne typy wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="b2cdf-127">Na przykład akcja szczegóły () zwraca RedirectToActionResult, która przekierowuje żądanie przychodzące do akcji index ().</span><span class="sxs-lookup"><span data-stu-id="b2cdf-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="b2cdf-128">Akcja index () zawiera następujący pojedynczy wiersz kodu:</span><span class="sxs-lookup"><span data-stu-id="b2cdf-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="b2cdf-129">View()</span><span class="sxs-lookup"><span data-stu-id="b2cdf-129">View()</span></span>

<span data-ttu-id="b2cdf-130">Ten wiersz kodu zwraca widok, który musi znajdować się w następującej ścieżce na serwerze sieci Web:</span><span class="sxs-lookup"><span data-stu-id="b2cdf-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="b2cdf-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="b2cdf-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="b2cdf-132">Ścieżka do widoku jest wywnioskowana na podstawie nazwy kontrolera i nazwy akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="b2cdf-133">Jeśli wolisz, możesz być jawne o widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="b2cdf-134">Następujący wiersz kodu zwraca widok o nazwie Fred:</span><span class="sxs-lookup"><span data-stu-id="b2cdf-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="b2cdf-135">Widok (Fred)</span><span class="sxs-lookup"><span data-stu-id="b2cdf-135">View( Fred )</span></span>

<span data-ttu-id="b2cdf-136">Po wykonaniu tego wiersza kodu jest zwracany widok z następującej ścieżki:</span><span class="sxs-lookup"><span data-stu-id="b2cdf-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="b2cdf-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="b2cdf-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b2cdf-138">Jeśli planujesz utworzyć testy jednostkowe dla aplikacji ASP.NET MVC, dobrym pomysłem jest jawne informacje o nazwach widoków.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="b2cdf-139">W ten sposób można utworzyć test jednostkowy, aby sprawdzić, czy oczekiwany widok został zwrócony przez akcję kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>

## <a name="adding-content-to-a-view"></a><span data-ttu-id="b2cdf-140">Dodawanie zawartości do widoku</span><span class="sxs-lookup"><span data-stu-id="b2cdf-140">Adding Content to a View</span></span>

<span data-ttu-id="b2cdf-141">Widok to standardowy dokument HTML (X), który może zawierać skrypty.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="b2cdf-142">Za pomocą skryptów można dodać zawartość dynamiczną do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="b2cdf-143">Na przykład widok na liście 2 wyświetla bieżącą datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="b2cdf-144">**Lista 2 — \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2cdf-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="b2cdf-145">Zwróć uwagę, że treść strony HTML na liście 2 zawiera następujący skrypt:</span><span class="sxs-lookup"><span data-stu-id="b2cdf-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="b2cdf-146">&lt;% Response. Write (DateTime. Now)%&gt;</span><span class="sxs-lookup"><span data-stu-id="b2cdf-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="b2cdf-147">Używasz ograniczników skryptów &lt;% i%&gt; do oznaczania początku i końca skryptu.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="b2cdf-148">Ten skrypt jest pisany w języku Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-148">This script is written in Visual basic.</span></span> <span data-ttu-id="b2cdf-149">Wyświetla bieżącą datę i godzinę, wywołując metodę Response. Write () w celu renderowania zawartości w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="b2cdf-150">Ograniczniki skryptu &lt;% i%&gt; mogą służyć do wykonywania jednej lub kilku instrukcji.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="b2cdf-151">Od momentu wywołania metody Response. Write (), firma Microsoft udostępnia skrót do wywołania metod Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="b2cdf-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="b2cdf-152">Widok w liście 3 używa ograniczników &lt;% = i%&gt; jako skrót do wywołania metody Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="b2cdf-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="b2cdf-153">**Lista 3 — Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2cdf-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="b2cdf-154">Aby wygenerować zawartość dynamiczną w widoku, można użyć dowolnego języka platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="b2cdf-155">Zwykle używa się Visual Basic .NET lub C# do pisania kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="b2cdf-156">Generowanie zawartości widoku przy użyciu pomocników HTML</span><span class="sxs-lookup"><span data-stu-id="b2cdf-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="b2cdf-157">Aby ułatwić Dodawanie zawartości do widoku, możesz skorzystać z czegoś " *pomocnik HTML*".</span><span class="sxs-lookup"><span data-stu-id="b2cdf-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="b2cdf-158">Pomocnik HTML, zazwyczaj jest metodą, która generuje ciąg.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="b2cdf-159">Pomocników HTML można użyć do wygenerowania standardowych elementów HTML, takich jak pola tekstowe, linki, listy rozwijane i pola listy.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="b2cdf-160">Na przykład widok z listy 4 wykorzystuje trzy pomocnicy HTML — BeginForm (), pole tekstowe () i hasło () — aby wygenerować formularz logowania (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="b2cdf-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="b2cdf-161">**Lista 4 — \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2cdf-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]

<span data-ttu-id="b2cdf-162">[![okno dialogowe Nowy projekt](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b2cdf-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="b2cdf-163">**Ilustracja 01**: standardowy formularz logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b2cdf-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>

<span data-ttu-id="b2cdf-164">Wszystkie metody pomocników HTML są wywoływane we właściwości HTML widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="b2cdf-165">Na przykład, możesz renderować pole tekstowe przez wywołanie metody html. TextBox ().</span><span class="sxs-lookup"><span data-stu-id="b2cdf-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="b2cdf-166">Zwróć uwagę, że używasz ograniczników skryptów &lt;% = i%&gt; podczas wywoływania zarówno pomocników html. TextBox (), jak i HTML. Password ().</span><span class="sxs-lookup"><span data-stu-id="b2cdf-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="b2cdf-167">Ci pomocnicy po prostu zwracają ciąg.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-167">These helpers simply return a string.</span></span> <span data-ttu-id="b2cdf-168">Musisz wywołać metodę Response. Write () w celu renderowania ciągu do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="b2cdf-169">Korzystanie z metod pomocników HTML jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="b2cdf-170">Zwiększają one życie, zmniejszając ilość kodu HTML i skryptów, które należy napisać.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="b2cdf-171">Widok na liście 5 renderuje dokładnie ten sam formularz co widok na liście 4 bez użycia pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="b2cdf-172">**Lista 5 — \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2cdf-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="b2cdf-173">Dostępna jest również opcja tworzenia własnych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="b2cdf-174">Na przykład można utworzyć metodę pomocnika GridView (), która wyświetla zestaw rekordów bazy danych w tabeli HTML automatycznie.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="b2cdf-175">W samouczku opisano **Tworzenie niestandardowych pomocników HTML**.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="b2cdf-176">Używanie danych widoku do przekazywania danych do widoku</span><span class="sxs-lookup"><span data-stu-id="b2cdf-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="b2cdf-177">Użyj widoku dane, aby przekazać dane z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="b2cdf-178">Pomyśl o wyświetlaniu danych, takich jak pakiet wysyłany za pomocą poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="b2cdf-179">Wszystkie dane przekazywane z kontrolera do widoku muszą być wysyłane przy użyciu tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="b2cdf-180">Na przykład kontroler na liście 6 dodaje komunikat, aby wyświetlić dane.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="b2cdf-181">**Lista 6 — ProductController. vb**</span><span class="sxs-lookup"><span data-stu-id="b2cdf-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="b2cdf-182">Właściwość ViewData kontrolera reprezentuje kolekcję par nazw i wartości.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="b2cdf-183">W liście 6 Metoda index () dodaje element do kolekcji danych widoku o nazwie Message o wartości Hello world!.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="b2cdf-184">Gdy widok jest zwracany przez metodę index (), dane widoku są automatycznie przesyłane do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="b2cdf-185">Widok z listy 7 pobiera komunikat z danych widoku i renderuje komunikat do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="b2cdf-186">**Lista 7 — \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2cdf-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="b2cdf-187">Należy zauważyć, że widok wykorzystuje metodę pomocnika html. Encode () podczas renderowania komunikatu.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="b2cdf-188">Pomocnik html. Encode () koduje znaki specjalne, takie jak &lt; i &gt;, do znaków, które są bezpieczne do wyświetlenia na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="b2cdf-189">Zawsze podczas renderowania zawartości przesyłanej przez użytkownika do witryny sieci Web należy zakodować zawartość, aby zapobiec atakom z użyciem kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="b2cdf-190">(Ponieważ został utworzony komunikat wypróbujemy w ProductController, my chcemy zakodować komunikat.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="b2cdf-191">Jednak dobrym wykonywaćem jest zawsze wywoływanie metody html. Encode () podczas wyświetlania zawartości pobranej z widoku danych w widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="b2cdf-192">W przypadku wystawiania 7 korzystamy z funkcji Wyświetl dane w celu przekazania prostego komunikatu ciągu z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="b2cdf-193">Można także użyć widoku dane do przekazania innych typów danych, takich jak kolekcja rekordów bazy danych, z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="b2cdf-194">Na przykład, jeśli chcesz wyświetlić zawartość tabeli bazy danych produktów w widoku, należy przekazać kolekcję rekordów bazy danych w widoku dane.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="b2cdf-195">Dostępna jest również opcja przekazywania danych o jednoznacznie określonym typie z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="b2cdf-196">Ten temat jest omawiany w samouczku **Omówienie jednoznacznie wpisanych danych i widoków widoku**.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="b2cdf-197">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b2cdf-197">Summary</span></span>

<span data-ttu-id="b2cdf-198">Ten samouczek zawiera krótkie wprowadzenie do ASP.NET widoków MVC, wyświetlania danych i pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="b2cdf-199">W pierwszej sekcji przedstawiono sposób dodawania nowych widoków do projektu.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="b2cdf-200">Wiesz, że musisz dodać widok do odpowiedniego folderu, aby wywołać go z określonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="b2cdf-201">Następnie omówiono temat pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="b2cdf-202">Wiesz, jak pomocniki HTML umożliwiają łatwe generowanie standardowej zawartości HTML.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="b2cdf-203">Na koniec wiesz już, jak korzystać z danych widoku do przekazywania danych z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2cdf-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2cdf-204">[Poprzednie](passing-data-to-view-master-pages-cs.md)
> [dalej](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b2cdf-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
