---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Widoków ASP.NET MVC (VB) — omówienie | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Co to jest widok ASP.NET MVC i jak różni się on ze strony HTML? W tym samouczku Walther Autor: Stephen stanowi wprowadzenie do widoków i pokazuje, jak można t...'
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f02728ed248f29b09d654e509977ed43889cbb83
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65117433"
---
# <a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="6753f-104">Omówienie widoków ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="6753f-104">ASP.NET MVC Views Overview (VB)</span></span>

<span data-ttu-id="6753f-105">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="6753f-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="6753f-106">Co to jest widok ASP.NET MVC i jak różni się on ze strony HTML?</span><span class="sxs-lookup"><span data-stu-id="6753f-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="6753f-107">W tym samouczku Walther Autor: Stephen stanowi wprowadzenie do widoków i pokazuje, jak możesz korzystać z zalet danych widoku i pomocników HTML w widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>

<span data-ttu-id="6753f-108">Celem tego samouczka jest zapewnienie krótkie wprowadzenie do widoków ASP.NET MVC, wyświetlanie danych i pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="6753f-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="6753f-109">Do końca tego samouczka należy wiedzieć, jak tworzenie nowych widoków, przekazać dane z kontrolera do widoku i użyć pomocników HTML do generowania zawartości w widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="6753f-110">Objaśnienie widoków</span><span class="sxs-lookup"><span data-stu-id="6753f-110">Understanding Views</span></span>

<span data-ttu-id="6753f-111">W przeciwieństwie do programu ASP.NET lub Active Server Pages platformy ASP.NET MVC nie obejmuje wszystko, co odnosi się bezpośrednio do strony.</span><span class="sxs-lookup"><span data-stu-id="6753f-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="6753f-112">W aplikacji ASP.NET MVC nie istnieje strona na dysku, który odnosi się do ścieżki na adres URL, który można wpisać w pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6753f-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="6753f-113">Najbliższy rzeczą do strony w aplikacji ASP.NET MVC jest coś, co o nazwie *widoku*.</span><span class="sxs-lookup"><span data-stu-id="6753f-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="6753f-114">W aplikacji ASP.NET MVC przychodzących żądań przeglądarki są mapowane do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6753f-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="6753f-115">Akcja kontrolera może zwrócić widok.</span><span class="sxs-lookup"><span data-stu-id="6753f-115">A controller action might return a view.</span></span> <span data-ttu-id="6753f-116">Akcja kontrolera może jednak wykonać inny rodzaj akcje, takie jak przekierowywanie do kolejnej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6753f-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="6753f-117">Wyświetlanie listy 1 zawiera proste o nazwie HomeController kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6753f-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="6753f-118">HomeController udostępnia dwie akcje kontroler o nazwie indeks() i Details().</span><span class="sxs-lookup"><span data-stu-id="6753f-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="6753f-119">**Wyświetlanie listy 1 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="6753f-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="6753f-120">Możesz wywołać pierwszą akcję działania indeks(), wpisując następujący adres URL w pasku adresu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="6753f-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="6753f-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="6753f-121">/Home/Index</span></span>

<span data-ttu-id="6753f-122">Możesz wywołać drugiej akcji, akcja Details(), wpisując ten adres w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="6753f-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="6753f-123">/ Home/Details</span><span class="sxs-lookup"><span data-stu-id="6753f-123">/Home/Details</span></span>

<span data-ttu-id="6753f-124">Akcja indeks() zwraca widok.</span><span class="sxs-lookup"><span data-stu-id="6753f-124">The Index() action returns a view.</span></span> <span data-ttu-id="6753f-125">Większość czynności, które tworzysz zwróci widoków.</span><span class="sxs-lookup"><span data-stu-id="6753f-125">Most actions that you create will return views.</span></span> <span data-ttu-id="6753f-126">Jednak działanie może zwrócić inne typy wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="6753f-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="6753f-127">Na przykład akcja Details() zwraca RedirectToActionResult, który przekierowuje żądania przychodzące do działania indeks().</span><span class="sxs-lookup"><span data-stu-id="6753f-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="6753f-128">Akcja indeks() zawiera następujące jednego wiersza kodu:</span><span class="sxs-lookup"><span data-stu-id="6753f-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="6753f-129">View()</span><span class="sxs-lookup"><span data-stu-id="6753f-129">View()</span></span>

<span data-ttu-id="6753f-130">Ten wiersz kodu zwraca widok, który musi znajdować się w następującej ścieżce na serwerze sieci web:</span><span class="sxs-lookup"><span data-stu-id="6753f-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="6753f-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="6753f-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="6753f-132">Ścieżka widoku wynika z nazwy kontrolera i nazwy akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6753f-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="6753f-133">Jeśli wolisz, może być wyraźnie widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="6753f-134">Następujący wiersz kodu zwraca widok o nazwie Fred:</span><span class="sxs-lookup"><span data-stu-id="6753f-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="6753f-135">Widok (Fred)</span><span class="sxs-lookup"><span data-stu-id="6753f-135">View( Fred )</span></span>

<span data-ttu-id="6753f-136">Po wykonaniu ten wiersz kodu widok jest zwracana z następującą ścieżkę:</span><span class="sxs-lookup"><span data-stu-id="6753f-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="6753f-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="6753f-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="6753f-138">Jeśli planujesz utworzenie testów jednostkowych dla aplikacji ASP.NET MVC jest dobry pomysł, aby była niejawna przy korzystaniu nazwy widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="6753f-139">W ten sposób można utworzyć test jednostkowy, aby sprawdzić, czy oczekiwany widok został zwrócony przez akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6753f-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>

## <a name="adding-content-to-a-view"></a><span data-ttu-id="6753f-140">Dodawanie zawartości do widoku</span><span class="sxs-lookup"><span data-stu-id="6753f-140">Adding Content to a View</span></span>

<span data-ttu-id="6753f-141">Widok jest standardowego (dokumentu HTML, który może zawierać skryptów X).</span><span class="sxs-lookup"><span data-stu-id="6753f-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="6753f-142">Skrypty umożliwia dodawanie zawartości dynamicznej do widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="6753f-143">Na przykład widok w ofercie 2 Wyświetla bieżącą datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="6753f-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="6753f-144">**Wyświetlanie listy 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="6753f-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="6753f-145">Zwróć uwagę, że treść strony HTML znajdującej się w ofercie 2 zawiera następujący skrypt:</span><span class="sxs-lookup"><span data-stu-id="6753f-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="6753f-146">&lt;% Response.Write(DateTime.Now)%&gt;</span><span class="sxs-lookup"><span data-stu-id="6753f-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="6753f-147">Możesz użyć ograniczników skryptu &lt;% i %&gt; do oznaczania początku i końcu skryptu.</span><span class="sxs-lookup"><span data-stu-id="6753f-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="6753f-148">Ten skrypt został napisany w języku Visual basic.</span><span class="sxs-lookup"><span data-stu-id="6753f-148">This script is written in Visual basic.</span></span> <span data-ttu-id="6753f-149">Wyświetla bieżącą datę i godzinę, wywołując metodę Response.Write() do renderowania zawartości do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6753f-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="6753f-150">Ograniczniki skryptu &lt;% i %&gt; można wykonać jedną lub więcej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="6753f-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="6753f-151">Ponieważ wywołujesz Response.Write() tak często, firma Microsoft umożliwia skrót do wywoływania metody Response.Write().</span><span class="sxs-lookup"><span data-stu-id="6753f-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="6753f-152">Wyświetl w ofercie 3 korzysta z ogranicznikami &lt;% = i %&gt; jako skrót do wywoływania Response.Write().</span><span class="sxs-lookup"><span data-stu-id="6753f-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="6753f-153">**Wyświetlanie listy 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="6753f-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="6753f-154">Można użyć dowolnego języka platformy .NET do generowania zawartości dynamicznej w widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="6753f-155">Zwykle ll możesz użyć Visual Basic .NET lub C# do pisania widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="6753f-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="6753f-156">Wyświetlanie zawartości przy użyciu pomocników HTML</span><span class="sxs-lookup"><span data-stu-id="6753f-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="6753f-157">Aby ułatwić dodawanie zawartości do widoku, możesz korzystać z zalet coś, co jest nazywane *pomocnika kodu HTML*.</span><span class="sxs-lookup"><span data-stu-id="6753f-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="6753f-158">Pomocnik kodu HTML, zazwyczaj jest metodą, która generuje ciąg.</span><span class="sxs-lookup"><span data-stu-id="6753f-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="6753f-159">Pomocników HTML można użyć do wygenerowania standardowych elementów HTML, takich jak pola tekstowe, linki, listy rozwijane i pola listy.</span><span class="sxs-lookup"><span data-stu-id="6753f-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="6753f-160">Na przykład widok w ofercie 4 wykorzystuje trzy pomocników HTML — pomocnicy BeginForm() TextBox() i Password() — do generowania nazwy logowania formularza (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="6753f-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="6753f-161">**Wyświetlanie listy 4 – \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="6753f-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]

<span data-ttu-id="6753f-162">[![Okno dialogowe Nowy projekt](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6753f-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="6753f-163">**Rysunek 01**: Standardowa formularz logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6753f-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>

<span data-ttu-id="6753f-164">Wszystkie metody pomocników HTML są nazywane we właściwości Html widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="6753f-165">Na przykład przez wywołanie metody Html.TextBox() renderowanie pole tekstowe.</span><span class="sxs-lookup"><span data-stu-id="6753f-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="6753f-166">Zwróć uwagę, że używasz ograniczników skryptu &lt;% = i %&gt; podczas wywoływania Html.TextBox() i Html.Password() pomocników.</span><span class="sxs-lookup"><span data-stu-id="6753f-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="6753f-167">Pomocnicy te po prostu zwrócenia ciągu.</span><span class="sxs-lookup"><span data-stu-id="6753f-167">These helpers simply return a string.</span></span> <span data-ttu-id="6753f-168">Należy wywołać Response.Write() do wyświetlenia ciągu do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6753f-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="6753f-169">Użycie metody pomocnika kodu HTML jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="6753f-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="6753f-170">One ułatwiać pracę dzięki zmniejszeniu ilości kodu HTML i skryptu, który należy napisać.</span><span class="sxs-lookup"><span data-stu-id="6753f-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="6753f-171">Wyświetl listę 5 renderuje dokładnie tego samego formularza jako widok w ofercie 4 bez użycia pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="6753f-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="6753f-172">**Wyświetlanie listy 5 — \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="6753f-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="6753f-173">Istnieje również możliwość tworzenia własnych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="6753f-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="6753f-174">Na przykład można utworzyć metody pomocnika GridView(), który automatycznie wyświetla zestaw rekordów bazy danych w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="6753f-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="6753f-175">W tym samouczku dowiesz się w tym temacie **Tworzenie niestandardowych pomocników HTML**.</span><span class="sxs-lookup"><span data-stu-id="6753f-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="6753f-176">Za pomocą widoku danych, aby przekazać dane do widoku</span><span class="sxs-lookup"><span data-stu-id="6753f-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="6753f-177">Wyświetlanie danych służy do przekazywania danych za pomocą kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="6753f-178">Traktuj dane widoku, takie jak pakiet wysyłanych za pośrednictwem wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="6753f-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="6753f-179">Wszystkie dane przekazane za pomocą kontrolera do widoku musi zostać wysłany za pomocą tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="6753f-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="6753f-180">Na przykład kontroler w ofercie 6 dodaje komunikat do wyświetlania danych.</span><span class="sxs-lookup"><span data-stu-id="6753f-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="6753f-181">**Wyświetlanie listy 6 - ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="6753f-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="6753f-182">Kontroler ViewData właściwość reprezentuje kolekcję par nazw i wartości.</span><span class="sxs-lookup"><span data-stu-id="6753f-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="6753f-183">W ofercie 6 metoda indeks() dodaje element do widoku zbierania danych o nazwie message wartością Hello World!.</span><span class="sxs-lookup"><span data-stu-id="6753f-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="6753f-184">Gdy widok jest zwracany przez metodę indeks(), dane widoku są automatycznie przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="6753f-185">Wyświetl w ofercie 7 pobiera komunikat z danymi widoku i renderuje komunikat do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6753f-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="6753f-186">**Wyświetlanie listy 7 – \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="6753f-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="6753f-187">Należy zauważyć, że widok wykorzystuje metody pomocnika kodu HTML Html.Encode() podczas renderowania komunikatu.</span><span class="sxs-lookup"><span data-stu-id="6753f-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="6753f-188">Pomocnik kodu HTML Html.Encode() koduje znaki specjalne, takie jak &lt; i &gt; na znaki, które są bezpieczne wyświetlić na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="6753f-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="6753f-189">Zawsze, gdy renderowanie zawartości, który użytkownik prześle do witryny sieci Web należy zakodować zawartość, aby uniemożliwić ataki przez iniekcję kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6753f-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="6753f-190">(Ponieważ utworzyliśmy komunikat osoby w ProductController, firma Microsoft don t naprawdę potrzebne do zakodowania komunikatu.</span><span class="sxs-lookup"><span data-stu-id="6753f-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="6753f-191">Jednak jest dobry sposób zawsze wywołuj metody Html.Encode() podczas wyświetlania zawartości pobranej z wyświetlanie danych w widoku).</span><span class="sxs-lookup"><span data-stu-id="6753f-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="6753f-192">W ofercie 7 Firma Microsoft skorzystała z widoku danych komunikatu prosty ciąg znaków za pomocą kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="6753f-193">Również służy widok danych do przekazania innych typów danych, takich jak zbiór rekordów bazy danych, za pomocą kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="6753f-194">Na przykład jeśli chcesz wyświetlić zawartość tabeli Produkty bazy danych w widoku, a następnie przejdzie kolekcji bazy danych rejestruje w widoku danych.</span><span class="sxs-lookup"><span data-stu-id="6753f-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="6753f-195">Istnieje również opcja przekazywania silnie typizowanego widoku danych za pomocą kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="6753f-196">W tym samouczku dowiesz się w tym temacie **widoków i zrozumienia silnie Typizowane dane widoku**.</span><span class="sxs-lookup"><span data-stu-id="6753f-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="6753f-197">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="6753f-197">Summary</span></span>

<span data-ttu-id="6753f-198">W tym samouczku pod warunkiem krótkie wprowadzenie do widoków ASP.NET MVC, wyświetlanie danych i pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="6753f-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="6753f-199">W pierwszej sekcji przedstawiono sposób dodawania nowych widoków do projektu.</span><span class="sxs-lookup"><span data-stu-id="6753f-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="6753f-200">Wiesz, że należy dodać widok prawidłowego folderu w celu wywoływania go z określonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6753f-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="6753f-201">Następnie omówiono temat pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="6753f-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="6753f-202">Pokazaliśmy ci, jak pomocników HTML umożliwiają łatwe generowanie standardowych zawartość HTML.</span><span class="sxs-lookup"><span data-stu-id="6753f-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="6753f-203">Na koniec pokazaliśmy ci, jak korzystać z zalet danych widoku, aby przekazać dane z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="6753f-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6753f-204">[Poprzednie](passing-data-to-view-master-pages-cs.md)
> [dalej](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6753f-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
