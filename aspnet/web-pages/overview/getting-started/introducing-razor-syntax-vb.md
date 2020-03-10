---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Ten dodatek zawiera omówienie programowania za pomocą ASP.NET stron sieci Web w Visual Basic przy użyciu składnia Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526594"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="71f18-103">Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="71f18-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>

<span data-ttu-id="71f18-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="71f18-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="71f18-105">Ten artykuł zawiera omówienie programowania za pomocą ASP.NET stron sieci Web przy użyciu składnia Razor i Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="71f18-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="71f18-106">ASP.NET to technologia firmy Microsoft do uruchamiania dynamicznych stron sieci Web na serwerach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="71f18-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="71f18-107">Dowiesz **się**:</span><span class="sxs-lookup"><span data-stu-id="71f18-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="71f18-108">8 najważniejszych porad programistycznych dotyczących rozpoczynania pracy z programowaniem ASP.NET stron sieci Web przy użyciu programu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="71f18-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="71f18-109">Podstawowe pojęcia związane z programowaniem, które będą potrzebne.</span><span class="sxs-lookup"><span data-stu-id="71f18-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="71f18-110">Co to jest kod serwera ASP.NET i składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="71f18-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="71f18-111">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="71f18-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="71f18-112">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="71f18-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="71f18-113">Ten samouczek działa również z ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="71f18-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="71f18-114">Większość przykładów używania stron sieci Web ASP.NET z użyciem C#składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="71f18-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="71f18-115">Jednak składnia Razor obsługuje również Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="71f18-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="71f18-116">Aby program ASP.NET stronę sieci Web w Visual Basic, należy utworzyć stronę sieci Web z rozszerzeniem nazwy pliku *. vbhtml* , a następnie dodać kod Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="71f18-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="71f18-117">Ten artykuł zawiera omówienie pracy z Visual Basic językiem i składnią w celu tworzenia stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71f18-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="71f18-118">Domyślne szablony witryn sieci Web dla programu Microsoft WebMatrix (**piekarni**, **Photo Gallery**i **Starter site**itp.) są dostępne w C# systemach i Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="71f18-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="71f18-119">Szablony Visual Basic można zainstalować za pomocą programu jako pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="71f18-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="71f18-120">Szablony witryn sieci Web są instalowane w folderze głównym witryny w folderze o nazwie *Szablony firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="71f18-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="71f18-121">8 najważniejszych wskazówek dotyczących programowania</span><span class="sxs-lookup"><span data-stu-id="71f18-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="71f18-122">W tej sekcji przedstawiono kilka wskazówek, które należy znać podczas pisania kodu serwera ASP.NET przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="71f18-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="71f18-123">1. Dodaj kod do strony przy użyciu znaku @</span><span class="sxs-lookup"><span data-stu-id="71f18-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="71f18-124">Znak `@` uruchamia wyrażenia śródwierszowe, bloki pojedynczej instrukcji i bloki zawierające wiele instrukcji:</span><span class="sxs-lookup"><span data-stu-id="71f18-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="71f18-125">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-125">The result displayed in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="71f18-127">**Kodowanie HTML**</span><span class="sxs-lookup"><span data-stu-id="71f18-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="71f18-128">Gdy wyświetlasz zawartość na stronie przy użyciu znaku `@`, jak w powyższych przykładach, ASP.NET HTML — koduje dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="71f18-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="71f18-129">Zastępuje to zastrzeżone znaki HTML (takie jak `<` i `>` i `&`) kodami, które umożliwiają wyświetlanie znaków jako znaków na stronie sieci Web zamiast interpretowania ich jako tagów HTML lub jednostek.</span><span class="sxs-lookup"><span data-stu-id="71f18-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="71f18-130">Bez kodowania HTML dane wyjściowe z kodu serwera mogą nie być wyświetlane prawidłowo i mogą uwidaczniać zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="71f18-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="71f18-131">Jeśli celem jest wyprowadzanie kodu HTML, który renderuje Tagi jako znaczniki (na przykład `<p></p>` akapitu lub `<em></em>` w celu wyróżnienia tekstu), zobacz sekcję [łączenie tekstu, znaczników i kodu w blokach kodu w](#BM_CombiningTextMarkupAndCode) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="71f18-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="71f18-132">Więcej informacji na temat kodowania HTML można znaleźć w artykule [Praca z formularzami HTML w witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="71f18-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="71f18-133">2. bloki kodu zostały ujęte w kodzie... Kod końcowy</span><span class="sxs-lookup"><span data-stu-id="71f18-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="71f18-134">Blok kodu zawiera jedną lub więcej instrukcji kodu i jest ujęty w słowa kluczowe `Code` i `End Code`.</span><span class="sxs-lookup"><span data-stu-id="71f18-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="71f18-135">Umieść otwierający `Code` słowo kluczowe bezpośrednio po znaku &#8212; `@` nie może być odstęp między nimi.</span><span class="sxs-lookup"><span data-stu-id="71f18-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="71f18-136">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="71f18-138">3. wewnątrz bloku można zakończyć każdą instrukcję Code z podziałem wiersza</span><span class="sxs-lookup"><span data-stu-id="71f18-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="71f18-139">W bloku kodu Visual Basic każda instrukcja zostaje zakończona podziałem wiersza.</span><span class="sxs-lookup"><span data-stu-id="71f18-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="71f18-140">(W dalszej części artykułu zobaczysz sposób zawijania długiej instrukcji kodu do wielu wierszy w razie potrzeby).</span><span class="sxs-lookup"><span data-stu-id="71f18-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="71f18-141">4. Użyj zmiennych do przechowywania wartości</span><span class="sxs-lookup"><span data-stu-id="71f18-141">4. You use variables to store values</span></span>

<span data-ttu-id="71f18-142">Można przechowywać wartości w *zmiennej*, w tym ciągi, liczby i daty itp. Tworzysz nową zmienną za pomocą słowa kluczowego `Dim`.</span><span class="sxs-lookup"><span data-stu-id="71f18-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="71f18-143">Można wstawiać wartości zmiennych bezpośrednio na stronie przy użyciu `@`.</span><span class="sxs-lookup"><span data-stu-id="71f18-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="71f18-144">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="71f18-146">5. w podwójnym cudzysłowie należy umieścić wartości ciągu literału</span><span class="sxs-lookup"><span data-stu-id="71f18-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="71f18-147">*Ciąg* jest sekwencją znaków, które są traktowane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="71f18-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="71f18-148">Aby określić ciąg, należy umieścić go w podwójnym cudzysłowie:</span><span class="sxs-lookup"><span data-stu-id="71f18-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="71f18-149">Aby osadzić znaki podwójnego cudzysłowu w ciągu wartości ciągu, Wstaw dwa znaki podwójnego cudzysłowu.</span><span class="sxs-lookup"><span data-stu-id="71f18-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="71f18-150">Jeśli chcesz, aby znak podwójnego cudzysłowu pojawił się raz w danych wyjściowych na stronie, wprowadź go jako `""` w ciągu ujętym w cudzysłów, a jeśli chcesz, aby pojawił się dwukrotnie, wprowadź go jako `""""` w ciągu ujętym w cudzysłów.</span><span class="sxs-lookup"><span data-stu-id="71f18-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="71f18-151">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="71f18-153">6. kod Visual Basic nie uwzględnia wielkości liter</span><span class="sxs-lookup"><span data-stu-id="71f18-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="71f18-154">W języku Visual Basic nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="71f18-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="71f18-155">W każdym przypadku można napisać słowa kluczowe programowania (takie jak `Dim`, `If`i `True`) oraz nazwy zmiennych (takie jak `myString`lub `subTotal`).</span><span class="sxs-lookup"><span data-stu-id="71f18-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="71f18-156">Następujące wiersze kodu przypisują wartość do zmiennej `lastname` przy użyciu nazwy z małymi literami, a następnie wyprowadza wartość zmiennej na stronę przy użyciu nazwy z wielką literą.</span><span class="sxs-lookup"><span data-stu-id="71f18-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="71f18-157">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="71f18-159">7. większość kodowania obejmuje pracę z obiektami</span><span class="sxs-lookup"><span data-stu-id="71f18-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="71f18-160">Obiekt reprezentuje element, który można zaprogramować ze &#8212; stroną, polem tekstowym, plikiem, obrazem, żądaniem sieci Web, wiadomością e-mail, rekordem klienta (wierszem bazy danych) itd. Obiekty mają właściwości opisujące ich właściwości &#8212; obiekt pola tekstowego ma właściwość `Text`, obiekt żądania ma właściwość `Url`, wiadomość e-mail ma właściwość `From`, a obiekt klienta ma właściwość `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="71f18-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="71f18-161">Obiekty mają również metody, które są zleceniami &quot;,&quot; mogą być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="71f18-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="71f18-162">Przykłady obejmują metodę `Save` obiektu pliku, metodę `Rotate` obiektu obrazu oraz metodę `Send` obiektu poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="71f18-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="71f18-163">Często pracujesz z obiektem `Request`, który zawiera informacje, takie jak wartości pól formularza na stronie (pola tekstowe itd.), typ przeglądarki, która złożyła żądanie, adres URL strony, tożsamość użytkownika itd. Ten przykład pokazuje, jak uzyskać dostęp do właściwości obiektu `Request` i jak wywołać metodę `MapPath` obiektu `Request`, która zapewnia ścieżkę bezwzględną strony na serwerze:</span><span class="sxs-lookup"><span data-stu-id="71f18-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="71f18-164">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="71f18-166">8. możesz napisać kod, który podejmuje decyzje</span><span class="sxs-lookup"><span data-stu-id="71f18-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="71f18-167">Kluczową cechą dynamicznych stron sieci Web jest określenie, jakie czynności należy wykonać na podstawie warunków.</span><span class="sxs-lookup"><span data-stu-id="71f18-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="71f18-168">Najbardziej typowym sposobem wykonania tej czynności jest wykonanie instrukcji `If` (i opcjonalnej instrukcji `Else`).</span><span class="sxs-lookup"><span data-stu-id="71f18-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="71f18-169">Instrukcja `If IsPost` to skrócony sposób pisania `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="71f18-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="71f18-170">Wraz z `If` instrukcjami istnieją różne sposoby testowania warunków, powtarzania bloków kodu i tak dalej, które opisano w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="71f18-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="71f18-171">Wynik wyświetlany w przeglądarce (po kliknięciu przycisku **Prześlij**):</span><span class="sxs-lookup"><span data-stu-id="71f18-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="71f18-173">**Metody GET i POST protokołu HTTP oraz Właściwość ispost**</span><span class="sxs-lookup"><span data-stu-id="71f18-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="71f18-174">Protokół używany przez strony sieci Web (HTTP) obsługuje bardzo ograniczoną liczbę metod (&quot;czasownik&quot;), które są używane do żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="71f18-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="71f18-175">Dwa Najczęstsze są pobieranie, które są używane do odczytywania strony i wpisu, które są używane do przesyłania strony.</span><span class="sxs-lookup"><span data-stu-id="71f18-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="71f18-176">Na ogół podczas pierwszego żądania strony użytkownik żąda strony przy użyciu polecenia GET.</span><span class="sxs-lookup"><span data-stu-id="71f18-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="71f18-177">Jeśli użytkownik wypełni formularz, a następnie kliknie przycisk **Prześlij**, przeglądarka wysyła żądanie post do serwera.</span><span class="sxs-lookup"><span data-stu-id="71f18-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="71f18-178">W programowaniu sieci Web często warto wiedzieć, czy strona jest żądana jako GET, czy jako wpis, aby poznać sposób przetwarzania strony.</span><span class="sxs-lookup"><span data-stu-id="71f18-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="71f18-179">Na stronach sieci Web ASP.NET można użyć właściwości `IsPost`, aby sprawdzić, czy żądanie jest GET lub POST.</span><span class="sxs-lookup"><span data-stu-id="71f18-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="71f18-180">Jeśli żądanie jest WPISem, właściwość `IsPost` zwróci wartość true i można wykonać czynności, takie jak odczytywanie wartości pól tekstowych w formularzu.</span><span class="sxs-lookup"><span data-stu-id="71f18-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="71f18-181">Wiele przykładów zobaczysz, jak przetwarzać stronę w różny sposób w zależności od wartości `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="71f18-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="71f18-182">Prosty przykład kodu</span><span class="sxs-lookup"><span data-stu-id="71f18-182">A Simple Code Example</span></span>

<span data-ttu-id="71f18-183">Ta procedura pokazuje, jak utworzyć stronę przedstawiającą podstawowe techniki programowania.</span><span class="sxs-lookup"><span data-stu-id="71f18-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="71f18-184">W tym przykładzie utworzysz stronę umożliwiającą użytkownikom wprowadzanie dwóch liczb, a następnie ich dodanie i wyświetlenie wyniku.</span><span class="sxs-lookup"><span data-stu-id="71f18-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="71f18-185">W edytorze Utwórz nowy plik i nadaj mu nazwę *AddNumbers. vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="71f18-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="71f18-186">Skopiuj poniższy kod i adiustację na stronie, zastępując wszystkie elementy znajdujące się już na stronie.</span><span class="sxs-lookup"><span data-stu-id="71f18-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="71f18-187">Oto kilka rzeczy, dla których warto zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="71f18-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="71f18-188">Znak `@` uruchamia pierwszy blok kodu na stronie i poprzedza zmienną `totalMessage` osadzoną w dolnej części.</span><span class="sxs-lookup"><span data-stu-id="71f18-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="71f18-189">Blok w górnej części strony jest ujęty w `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="71f18-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="71f18-190">Zmienne `total`, `num1`, `num2`i `totalMessage` przechowują kilka liczb i ciąg.</span><span class="sxs-lookup"><span data-stu-id="71f18-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="71f18-191">Wartość ciągu literału przypisana do zmiennej `totalMessage` znajduje się w podwójnym cudzysłowie.</span><span class="sxs-lookup"><span data-stu-id="71f18-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="71f18-192">Ponieważ w kodzie Visual Basic nie jest rozróżniana wielkość liter, gdy zmienna `totalMessage` jest używana w dolnej części strony, jej nazwa musi być zgodna z pisownią deklaracji zmiennej w górnej części strony.</span><span class="sxs-lookup"><span data-stu-id="71f18-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="71f18-193">Wielkość liter nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="71f18-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="71f18-194">Wyrażenie `num1.AsInt()` + `num2.AsInt()` pokazuje, jak korzystać z obiektów i metod.</span><span class="sxs-lookup"><span data-stu-id="71f18-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="71f18-195">Metoda `AsInt` dla każdej zmiennej konwertuje ciąg wprowadzony przez użytkownika na liczbę całkowitą (liczbę całkowitą), którą można dodać.</span><span class="sxs-lookup"><span data-stu-id="71f18-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="71f18-196">Tag `<form>` zawiera atrybut `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="71f18-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="71f18-197">Oznacza to, że gdy użytkownik kliknie przycisk **Dodaj**, Strona zostanie wysłana na serwer przy użyciu metody POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="71f18-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="71f18-198">Gdy strona zostanie przesłana, kod `If IsPost` ma wartość true, a kod warunkowy zostanie uruchomiony, wyświetlając wynik dodawania liczb.</span><span class="sxs-lookup"><span data-stu-id="71f18-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="71f18-199">Zapisz stronę i uruchom ją w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="71f18-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="71f18-200">(Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Wprowadź dwie liczby całkowite, a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="71f18-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="71f18-202">Visual Basic język i składnia</span><span class="sxs-lookup"><span data-stu-id="71f18-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="71f18-203">Wcześniej przedstawiono podstawowy przykład tworzenia strony sieci Web ASP.NET oraz sposób dodawania kodu serwerowego do znacznika HTML.</span><span class="sxs-lookup"><span data-stu-id="71f18-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="71f18-204">W tym artykule poznasz podstawy używania Visual Basic do pisania kodu serwera ASP.NET przy użyciu składnia Razor &#8212; , które są regułami języka programowania.</span><span class="sxs-lookup"><span data-stu-id="71f18-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="71f18-205">Jeśli masz doświadczenie z programowaniem (zwłaszcza jeśli użyto języka C, C++, C#, Visual Basic lub JavaScript), większość czytanych tutaj informacji będzie znana.</span><span class="sxs-lookup"><span data-stu-id="71f18-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="71f18-206">Prawdopodobnie trzeba będzie zaznajomić się tylko z informacjami o sposobie dodawania kodu WebMatrix do znaczników w plikach *. vbhtml* .</span><span class="sxs-lookup"><span data-stu-id="71f18-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a><span data-ttu-id="71f18-207">Łączenie tekstu, znaczników i kodu w blokach kodu</span><span class="sxs-lookup"><span data-stu-id="71f18-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="71f18-208">W blokach kodu serwera często chcesz, aby tekst i adiustacje były wyprowadzane na stronie.</span><span class="sxs-lookup"><span data-stu-id="71f18-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="71f18-209">Jeśli blok kodu serwera zawiera tekst, który nie jest kodem, a zamiast tego powinien być renderowany jako, ASP.NET musi mieć możliwość odróżnienia tego tekstu od kodu.</span><span class="sxs-lookup"><span data-stu-id="71f18-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="71f18-210">Istnieje kilka sposobów, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="71f18-210">There are several ways to do this.</span></span>

- <span data-ttu-id="71f18-211">Ujmij tekst w element bloku HTML, taki jak `<p></p>` lub `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="71f18-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="71f18-212">Element HTML może zawierać tekst, dodatkowe elementy HTML i wyrażenia kodu serwera.</span><span class="sxs-lookup"><span data-stu-id="71f18-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="71f18-213">Gdy ASP.NET widzi tag otwierającego HTML (na przykład `<p>`), renderuje wszystko element i jego zawartość jako plik przeglądarki (i rozwiązuje wyrażenie kodu serwera).</span><span class="sxs-lookup"><span data-stu-id="71f18-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="71f18-214">Użyj operatora `@:` lub elementu `<text>`.</span><span class="sxs-lookup"><span data-stu-id="71f18-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="71f18-215">`@:` wyprowadza pojedynczy wiersz zawartości zawierający zwykły tekst lub niedopasowane Tagi HTML; element `<text>` obejmuje wiele wierszy do danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="71f18-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="71f18-216">Te opcje są przydatne, gdy nie chcesz renderować elementu HTML jako części danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="71f18-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="71f18-217">Poniższy przykład powtarza poprzedni przykład, ale używa pojedynczej pary tagów `<text>`, aby umieścić tekst do renderowania.</span><span class="sxs-lookup"><span data-stu-id="71f18-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="71f18-218">W poniższym przykładzie Tagi `<text>` i `</text>` zawierają trzy wiersze, z których wszystkie mają niezwierany tekst i niedopasowane Tagi HTML (`<br />`), a także kod serwera i dopasowane Tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="71f18-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="71f18-219">Ponownie można także poprzedzać każdy wiersz pojedynczym operatorem `@:`; w dowolny sposób działa.</span><span class="sxs-lookup"><span data-stu-id="71f18-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="71f18-220">Podczas wyprowadzania tekstu, jak pokazano w tej &#8212; sekcji przy użyciu elementu HTML, operatora `@:` lub elementu &#8212; `<text>` ASP.NET nie jest kodowane w języku HTML.</span><span class="sxs-lookup"><span data-stu-id="71f18-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="71f18-221">(Jak wspomniano wcześniej, ASP.NET koduje dane wyjściowe wyrażeń kodów serwera i bloków kodu serwera, które są poprzedzone `@`, z wyjątkiem przypadków specjalnych zanotowanych w tej sekcji).</span><span class="sxs-lookup"><span data-stu-id="71f18-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="71f18-222">Odstępu</span><span class="sxs-lookup"><span data-stu-id="71f18-222">Whitespace</span></span>

<span data-ttu-id="71f18-223">Dodatkowe spacje w instrukcji (i poza literałem ciągu) nie wpływają na instrukcję:</span><span class="sxs-lookup"><span data-stu-id="71f18-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="71f18-224">Przerywanie długich instrukcji w wielu wierszach</span><span class="sxs-lookup"><span data-stu-id="71f18-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="71f18-225">Można przerwać wykonywanie instrukcji Long Code w wielu wierszach przy użyciu znaku podkreślenia `_` (który w Visual Basic jest nazywany *znakiem kontynuacji*) po każdym wierszu kodu.</span><span class="sxs-lookup"><span data-stu-id="71f18-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="71f18-226">Aby przerwać instrukcję do następnego wiersza, na końcu wiersza Dodaj spację, a następnie znak kontynuacji.</span><span class="sxs-lookup"><span data-stu-id="71f18-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="71f18-227">Kontynuuj wykonywanie instrukcji w następnym wierszu.</span><span class="sxs-lookup"><span data-stu-id="71f18-227">Continue the statement on the next line.</span></span> <span data-ttu-id="71f18-228">Można zawijać instrukcje na tyle wierszy, ile potrzebujesz, aby zwiększyć czytelność.</span><span class="sxs-lookup"><span data-stu-id="71f18-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="71f18-229">Następujące instrukcje są takie same:</span><span class="sxs-lookup"><span data-stu-id="71f18-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="71f18-230">Nie można jednak otoczyć wiersza w środku literału ciągu.</span><span class="sxs-lookup"><span data-stu-id="71f18-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="71f18-231">Poniższy przykład nie działa:</span><span class="sxs-lookup"><span data-stu-id="71f18-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="71f18-232">Aby połączyć długi ciąg, który otacza wiele wierszy, takich jak powyższy kod, należy użyć *operatora łączenia* (`&`), który będzie widoczny w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="71f18-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="71f18-233">Komentarze do kodu</span><span class="sxs-lookup"><span data-stu-id="71f18-233">Code comments</span></span>

<span data-ttu-id="71f18-234">Komentarze umożliwiają wystawianie notatek dla siebie lub dla innych osób.</span><span class="sxs-lookup"><span data-stu-id="71f18-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="71f18-235">Komentarze składnia Razor są poprzedzone `@*` i kończyć się `*@`.</span><span class="sxs-lookup"><span data-stu-id="71f18-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="71f18-236">W blokach kodu można użyć komentarzy składnia Razor lub użyć zwykłego znaku Visual Basic komentarza, który jest pojedynczym cudzysłowem (`'`) poprzedzonym każdym wierszem.</span><span class="sxs-lookup"><span data-stu-id="71f18-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="71f18-237">Zmienne</span><span class="sxs-lookup"><span data-stu-id="71f18-237">Variables</span></span>

<span data-ttu-id="71f18-238">Zmienna to nazwany obiekt, który służy do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="71f18-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="71f18-239">Można nazwać zmienne, ale nazwa musi zaczynać się od litery i nie może zawierać spacji ani znaków zarezerwowanych.</span><span class="sxs-lookup"><span data-stu-id="71f18-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="71f18-240">W Visual Basic, jak pokazano wcześniej, przypadek liter w nazwie zmiennej nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="71f18-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="71f18-241">Zmienne i typy danych</span><span class="sxs-lookup"><span data-stu-id="71f18-241">Variables and data types</span></span>

<span data-ttu-id="71f18-242">Zmienna może mieć określony typ danych, co wskazuje, jakiego rodzaju dane są przechowywane w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="71f18-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="71f18-243">Można mieć zmienne ciągów, które przechowują wartości ciągu (takie jak &quot;Hello World&quot;), zmienne typu Integer, które przechowują wartości całkowite (takie jak 3 lub 79) i zmienne dat, które przechowują wartości daty w różnych formatach (na przykład 4/12/2012 lub marzec 2009).</span><span class="sxs-lookup"><span data-stu-id="71f18-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="71f18-244">Istnieje wiele innych typów danych, których można użyć.</span><span class="sxs-lookup"><span data-stu-id="71f18-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="71f18-245">Nie trzeba jednak określać typu dla zmiennej.</span><span class="sxs-lookup"><span data-stu-id="71f18-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="71f18-246">W większości przypadków ASP.NET może ustalić typ na podstawie sposobu użycia danych w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="71f18-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="71f18-247">(Czasami musisz określić typ; zobaczysz przykłady, w których to prawda).</span><span class="sxs-lookup"><span data-stu-id="71f18-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="71f18-248">Aby zadeklarować zmienną bez określenia typu, użyj `Dim` i nazwy zmiennej (na przykład `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="71f18-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="71f18-249">Aby zadeklarować zmienną typu, użyj `Dim` plusa nazwy zmiennej, a następnie `As` a następnie nazwę typu (na przykład `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="71f18-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="71f18-250">Poniższy przykład pokazuje niektóre wyrażenia wbudowane, które używają zmiennych na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="71f18-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="71f18-251">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="71f18-253">Konwertowanie i testowanie typów danych</span><span class="sxs-lookup"><span data-stu-id="71f18-253">Converting and testing data types</span></span>

<span data-ttu-id="71f18-254">Chociaż ASP.NET zazwyczaj zwykle określa typ danych, czasami nie jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="71f18-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="71f18-255">W związku z tym może być konieczne ASP.NET przez wykonanie jawnej konwersji.</span><span class="sxs-lookup"><span data-stu-id="71f18-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="71f18-256">Nawet jeśli nie musisz konwertować typów, czasami warto przetestować, aby zobaczyć, jakiego typu dane mogą pracować.</span><span class="sxs-lookup"><span data-stu-id="71f18-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="71f18-257">Najczęstszym przypadkiem jest to, że trzeba przekonwertować ciąg na inny typ, na przykład na liczbę całkowitą lub datę.</span><span class="sxs-lookup"><span data-stu-id="71f18-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="71f18-258">W poniższym przykładzie pokazano typowy przypadek, w którym należy przekonwertować ciąg na liczbę.</span><span class="sxs-lookup"><span data-stu-id="71f18-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="71f18-259">Zgodnie z regułą dane wprowadzane przez użytkownika są uznawane za ciągi.</span><span class="sxs-lookup"><span data-stu-id="71f18-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="71f18-260">Nawet jeśli użytkownik otrzyma monit o wprowadzenie numeru, a nawet jeśli podano cyfrę, gdy dane wejściowe użytkownika są przesyłane i odczytywane w kodzie, dane są w formacie ciągu.</span><span class="sxs-lookup"><span data-stu-id="71f18-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="71f18-261">W związku z tym należy przekonwertować ciąg na liczbę.</span><span class="sxs-lookup"><span data-stu-id="71f18-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="71f18-262">W przykładzie, jeśli spróbujesz wykonać operacje arytmetyczne na wartościach bez ich konwersji, następujące wyniki błędu, ponieważ ASP.NET nie mogą dodać dwóch ciągów:</span><span class="sxs-lookup"><span data-stu-id="71f18-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="71f18-263">Aby przekonwertować wartości na liczby całkowite, należy wywołać metodę `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="71f18-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="71f18-264">Jeśli konwersja zakończy się pomyślnie, można dodać liczby.</span><span class="sxs-lookup"><span data-stu-id="71f18-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="71f18-265">W poniższej tabeli wymieniono niektóre typowe metody konwersji i testowania zmiennych.</span><span class="sxs-lookup"><span data-stu-id="71f18-265">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="71f18-266"><strong>Metoda</strong></span><span class="sxs-lookup"><span data-stu-id="71f18-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-267"><strong>Opis</strong></span><span class="sxs-lookup"><span data-stu-id="71f18-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-268"><strong>Przykład</strong></span><span class="sxs-lookup"><span data-stu-id="71f18-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-269">Konwertuje ciąg, który reprezentuje liczbę całkowitą (na przykład &quot;593&quot;) do liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="71f18-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-270">Konwertuje ciąg, taki jak &quot;true&quot; lub &quot;false&quot; na typ Boolean.</span><span class="sxs-lookup"><span data-stu-id="71f18-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-271">Konwertuje ciąg, który ma wartość dziesiętną, taką jak &quot;1,3&quot; lub &quot;7,439&quot; do liczby zmiennoprzecinkowej.</span><span class="sxs-lookup"><span data-stu-id="71f18-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-272">Konwertuje ciąg, który ma wartość dziesiętną, taką jak &quot;1,3&quot; lub &quot;7,439&quot; do liczby dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="71f18-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="71f18-273">(W ASP.NET liczba dziesiętna jest bardziej precyzyjna niż liczba zmiennoprzecinkowa).</span><span class="sxs-lookup"><span data-stu-id="71f18-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-274">Konwertuje ciąg, który reprezentuje wartość daty i godziny dla typu `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71f18-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-275">Konwertuje wszystkie inne typy danych na ciąg.</span><span class="sxs-lookup"><span data-stu-id="71f18-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="71f18-276">Operatory</span><span class="sxs-lookup"><span data-stu-id="71f18-276">Operators</span></span>

<span data-ttu-id="71f18-277">Operator jest słowem kluczowym lub znakiem, który informuje ASP.NET jakiego rodzaju polecenia wykonać w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="71f18-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="71f18-278">Visual Basic obsługuje wiele operatorów, ale wystarczy tylko rozpoznać kilka, aby rozpocząć opracowywanie stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71f18-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="71f18-279">Poniższa tabela zawiera podsumowanie najbardziej typowych operatorów.</span><span class="sxs-lookup"><span data-stu-id="71f18-279">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="71f18-280"><strong>Zakład</strong></span><span class="sxs-lookup"><span data-stu-id="71f18-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-281"><strong>Opis</strong></span><span class="sxs-lookup"><span data-stu-id="71f18-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-282"><strong>Przykłady</strong></span><span class="sxs-lookup"><span data-stu-id="71f18-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-283">Operatory matematyczne używane w wyrażeniach liczbowych.</span><span class="sxs-lookup"><span data-stu-id="71f18-283">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-284">Przypisanie i równość.</span><span class="sxs-lookup"><span data-stu-id="71f18-284">Assignment and equality.</span></span> <span data-ttu-id="71f18-285">W zależności od kontekstu przypisuje wartość po prawej stronie instrukcji do obiektu po lewej stronie lub sprawdza wartości pod kątem równości.</span><span class="sxs-lookup"><span data-stu-id="71f18-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-286">Nierówności.</span><span class="sxs-lookup"><span data-stu-id="71f18-286">Inequality.</span></span> <span data-ttu-id="71f18-287">Zwraca `True`, jeśli wartości nie są równe.</span><span class="sxs-lookup"><span data-stu-id="71f18-287">Returns `True` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-288">Mniejsze niż, większe niż, mniejsze niż lub równe i większe niż lub równe.</span><span class="sxs-lookup"><span data-stu-id="71f18-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-289">Łączenie, które jest używane do dołączania ciągów.</span><span class="sxs-lookup"><span data-stu-id="71f18-289">Concatenation, which is used to join strings.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-290">Operatory zwiększania i zmniejszania, które dodają i odejmujeją 1 (odpowiednio) od zmiennej.</span><span class="sxs-lookup"><span data-stu-id="71f18-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-291">Kropka.</span><span class="sxs-lookup"><span data-stu-id="71f18-291">Dot.</span></span> <span data-ttu-id="71f18-292">Używane do rozróżniania obiektów i ich właściwości i metod.</span><span class="sxs-lookup"><span data-stu-id="71f18-292">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-293">Nawiasów.</span><span class="sxs-lookup"><span data-stu-id="71f18-293">Parentheses.</span></span> <span data-ttu-id="71f18-294">Służy do grupowania wyrażeń, przekazywania parametrów do metod i uzyskiwania dostępu do elementów członkowskich tablic i kolekcji.</span><span class="sxs-lookup"><span data-stu-id="71f18-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-295">Niemożliwe.</span><span class="sxs-lookup"><span data-stu-id="71f18-295">Not.</span></span> <span data-ttu-id="71f18-296">Odwraca wartość true na false i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="71f18-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="71f18-297">Zwykle używany jako skrócony sposób testowania dla `False` (to oznacza, że nie `True`).</span><span class="sxs-lookup"><span data-stu-id="71f18-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        <span data-ttu-id="71f18-298">Logiczne i i lub, które są używane do łączenia warunków.</span><span class="sxs-lookup"><span data-stu-id="71f18-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="71f18-299">Praca z ścieżkami plików i folderów w kodzie</span><span class="sxs-lookup"><span data-stu-id="71f18-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="71f18-300">Często pracujesz ze ścieżkami plików i folderów w kodzie.</span><span class="sxs-lookup"><span data-stu-id="71f18-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="71f18-301">Oto przykład struktury folderów fizycznych dla witryny sieci Web, która może pojawić się na komputerze deweloperskim:</span><span class="sxs-lookup"><span data-stu-id="71f18-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="71f18-302">Poniżej przedstawiono niektóre istotne informacje dotyczące adresów URL i ścieżek:</span><span class="sxs-lookup"><span data-stu-id="71f18-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="71f18-303">Adres URL rozpoczyna się od nazwy domeny (`http://www.example.com`) lub nazwy serwera (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="71f18-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="71f18-304">Adres URL odpowiada ścieżce fizycznej na komputerze-hoście.</span><span class="sxs-lookup"><span data-stu-id="71f18-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="71f18-305">Na przykład `http://myserver` mogą odpowiadać *C:\websites\mywebsite* folderu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="71f18-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="71f18-306">Ścieżka wirtualna jest skrótem do reprezentowania ścieżek w kodzie bez konieczności określania pełnej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="71f18-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="71f18-307">Zawiera część adresu URL, która następuje po nazwie domeny lub serwera.</span><span class="sxs-lookup"><span data-stu-id="71f18-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="71f18-308">W przypadku używania ścieżek wirtualnych można przenieść kod do innej domeny lub serwera bez konieczności aktualizacji ścieżek.</span><span class="sxs-lookup"><span data-stu-id="71f18-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="71f18-309">Oto przykład, który pomoże zrozumieć różnice:</span><span class="sxs-lookup"><span data-stu-id="71f18-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="71f18-310">Pełny adres URL</span><span class="sxs-lookup"><span data-stu-id="71f18-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="71f18-311">Nazwa serwera</span><span class="sxs-lookup"><span data-stu-id="71f18-311">Server name</span></span> | <span data-ttu-id="71f18-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="71f18-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="71f18-313">Ścieżka wirtualna</span><span class="sxs-lookup"><span data-stu-id="71f18-313">Virtual path</span></span> | <span data-ttu-id="71f18-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="71f18-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="71f18-315">Ścieżka fizyczna</span><span class="sxs-lookup"><span data-stu-id="71f18-315">Physical path</span></span> | <span data-ttu-id="71f18-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="71f18-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="71f18-317">Wirtualny katalog główny jest/, podobnie jak katalog główny dysku C:.</span><span class="sxs-lookup"><span data-stu-id="71f18-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="71f18-318">(Ścieżki folderów wirtualnych zawsze używają ukośników). Ścieżka wirtualna folderu nie musi mieć takiej samej nazwy jak folder fizyczny; może to być alias.</span><span class="sxs-lookup"><span data-stu-id="71f18-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="71f18-319">(Na serwerach produkcyjnych ścieżka wirtualna rzadko jest zgodna z dokładną ścieżką fizyczną).</span><span class="sxs-lookup"><span data-stu-id="71f18-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="71f18-320">Podczas pracy z plikami i folderami w kodzie czasami trzeba odwoływać się do ścieżki fizycznej i czasami ścieżki wirtualnej, w zależności od obiektów, z którymi pracujesz.</span><span class="sxs-lookup"><span data-stu-id="71f18-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="71f18-321">ASP.NET udostępnia następujące narzędzia do pracy z ścieżkami plików i folderów w kodzie: Metoda `Server.MapPath` i operator `~` i Metoda `Href`.</span><span class="sxs-lookup"><span data-stu-id="71f18-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="71f18-322">Konwertowanie wirtualnej na ścieżki fizyczne: Metoda Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="71f18-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="71f18-323">Metoda `Server.MapPath` Konwertuje ścieżkę wirtualną (na przykład */default.cshtml*) na bezwzględną ścieżkę fizyczną (na przykład *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="71f18-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="71f18-324">Tej metody należy używać w dowolnym momencie, gdy potrzebna jest kompletna ścieżka fizyczna.</span><span class="sxs-lookup"><span data-stu-id="71f18-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="71f18-325">Typowym przykładem jest odczytywanie lub zapisywanie pliku tekstowego lub pliku obrazu na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="71f18-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="71f18-326">Zwykle nie jest znana absolutna ścieżka fizyczna witryny na serwerze lokacji hostingu, dlatego ta metoda może przekonwertować ścieżkę, którą znasz — ścieżkę wirtualną — do odpowiedniej ścieżki na serwerze.</span><span class="sxs-lookup"><span data-stu-id="71f18-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="71f18-327">Ścieżka wirtualna do pliku lub folderu zostanie przekazana do metody i zwraca ścieżkę fizyczną:</span><span class="sxs-lookup"><span data-stu-id="71f18-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="71f18-328">Odwoływanie się do wirtualnego katalogu głównego: Metoda ~ operatora i href</span><span class="sxs-lookup"><span data-stu-id="71f18-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="71f18-329">W pliku *. cshtml* lub *. vbhtml* można odwoływać się do wirtualnej ścieżki katalogu głównego przy użyciu operatora `~`.</span><span class="sxs-lookup"><span data-stu-id="71f18-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="71f18-330">Jest to bardzo przydatne, ponieważ można przenieść strony w obrębie witryny, a wszystkie linki, które zawierają do innych stron, nie zostaną zerwane.</span><span class="sxs-lookup"><span data-stu-id="71f18-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="71f18-331">Jest on również przydatny w przypadku przenoszenia witryny sieci Web do innej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="71f18-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="71f18-332">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="71f18-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="71f18-333">Jeśli witryna sieci Web jest `http://myserver/myapp`, Oto, jak ASP.NET będzie traktować te ścieżki podczas uruchamiania strony:</span><span class="sxs-lookup"><span data-stu-id="71f18-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="71f18-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="71f18-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="71f18-335">`myStyleSheet`: `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="71f18-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="71f18-336">(Te ścieżki nie będą widoczne w rzeczywistości jako wartości zmiennej, ale ASP.NET będą traktować ścieżki tak, jakby były.)</span><span class="sxs-lookup"><span data-stu-id="71f18-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="71f18-337">Operatora `~` można użyć zarówno w kodzie serwera (jak powyżej), jak to:</span><span class="sxs-lookup"><span data-stu-id="71f18-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="71f18-338">W znacznikach można użyć operatora `~`, aby tworzyć ścieżki do zasobów, takich jak pliki obrazów, inne strony sieci Web i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="71f18-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="71f18-339">Po uruchomieniu strony ASP.NET przegląda stronę (kod i znacznik) i rozwiązuje wszystkie `~` odwołania do odpowiedniej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="71f18-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="71f18-340">Logika warunkowa i pętle</span><span class="sxs-lookup"><span data-stu-id="71f18-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="71f18-341">Kod serwera ASP.NET umożliwia wykonywanie zadań w oparciu o warunki i pisanie kodu, który powtarza instrukcje, która pociąga za siebie określoną liczbę razy, kod, który uruchamia pętlę.</span><span class="sxs-lookup"><span data-stu-id="71f18-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="71f18-342">Warunki testowania</span><span class="sxs-lookup"><span data-stu-id="71f18-342">Testing conditions</span></span>

<span data-ttu-id="71f18-343">Aby przetestować prosty warunek, użyj instrukcji `If...Then`, która zwraca `True` lub `False` w oparciu o określony test:</span><span class="sxs-lookup"><span data-stu-id="71f18-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="71f18-344">Słowo kluczowe `If` uruchamia blok.</span><span class="sxs-lookup"><span data-stu-id="71f18-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="71f18-345">Rzeczywisty test (warunek) następuje po słowie kluczowym `If` i zwraca wartość true lub false.</span><span class="sxs-lookup"><span data-stu-id="71f18-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="71f18-346">Instrukcja `If` zostanie zakończona z `Then`.</span><span class="sxs-lookup"><span data-stu-id="71f18-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="71f18-347">Instrukcje, które będą uruchamiane, jeśli test ma wartość true, są ujęte w `If` i `End If`.</span><span class="sxs-lookup"><span data-stu-id="71f18-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="71f18-348">Instrukcja `If` może zawierać blok `Else`, który określa instrukcje do uruchomienia, jeśli warunek ma wartość false:</span><span class="sxs-lookup"><span data-stu-id="71f18-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="71f18-349">Jeśli instrukcja `If` uruchamia blok kodu, nie trzeba używać zwykłych instrukcji `Code...End Code` w celu uwzględnienia bloków.</span><span class="sxs-lookup"><span data-stu-id="71f18-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="71f18-350">Możesz po prostu dodać `@` do bloku i będzie on działać.</span><span class="sxs-lookup"><span data-stu-id="71f18-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="71f18-351">Takie podejście współdziała z `If`, a także innymi Visual Basic programistycznymi słowami kluczowymi, które są umieszczane w blokach kodu, takich jak `For`, `For Each`, `Do While`itd.</span><span class="sxs-lookup"><span data-stu-id="71f18-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="71f18-352">Można dodać wiele warunków przy użyciu jednego lub kilku bloków `ElseIf`:</span><span class="sxs-lookup"><span data-stu-id="71f18-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="71f18-353">W tym przykładzie, jeśli pierwszy warunek w bloku `If` nie ma wartości true, sprawdzana jest `ElseIf` warunek.</span><span class="sxs-lookup"><span data-stu-id="71f18-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="71f18-354">Jeśli ten warunek jest spełniony, instrukcje w bloku `ElseIf` są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="71f18-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="71f18-355">Jeśli żaden z warunków nie zostanie spełniony, instrukcje w bloku `Else` są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="71f18-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="71f18-356">Można dodać dowolną liczbę bloków `ElseIf`, a następnie zamknąć z blokiem `Else`, podobnie jak w przypadku &quot;wszystko inne&quot; warunek.</span><span class="sxs-lookup"><span data-stu-id="71f18-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="71f18-357">Aby przetestować dużą liczbę warunków, użyj bloku `Select Case`:</span><span class="sxs-lookup"><span data-stu-id="71f18-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="71f18-358">Wartość do przetestowania jest w nawiasach (w przykładzie zmienna Weekday).</span><span class="sxs-lookup"><span data-stu-id="71f18-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="71f18-359">Każdy test używa instrukcji `Case`, która wyświetla wartość.</span><span class="sxs-lookup"><span data-stu-id="71f18-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="71f18-360">Jeśli wartość instrukcji `Case` pasuje do wartości testowej, wykonywany jest kod w tym `Case` bloku.</span><span class="sxs-lookup"><span data-stu-id="71f18-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="71f18-361">Wynik ostatnich dwóch bloków warunkowych wyświetlanych w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="71f18-363">Kod zapętlenia</span><span class="sxs-lookup"><span data-stu-id="71f18-363">Looping code</span></span>

<span data-ttu-id="71f18-364">Często trzeba wielokrotnie uruchamiać te same instrukcje.</span><span class="sxs-lookup"><span data-stu-id="71f18-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="71f18-365">Można to zrobić przez zapętlenie.</span><span class="sxs-lookup"><span data-stu-id="71f18-365">You do this by looping.</span></span> <span data-ttu-id="71f18-366">Na przykład często uruchamiasz te same instrukcje dla każdego elementu w kolekcji danych.</span><span class="sxs-lookup"><span data-stu-id="71f18-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="71f18-367">Jeśli wiesz dokładnie, ile razy chcesz wykonać pętlę, możesz użyć pętli `For`.</span><span class="sxs-lookup"><span data-stu-id="71f18-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="71f18-368">Ten rodzaj pętli jest szczególnie przydatny do zliczania w górę lub w dół:</span><span class="sxs-lookup"><span data-stu-id="71f18-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="71f18-369">Pętla zaczyna się od słowa kluczowego `For`, a następnie trzech elementów:</span><span class="sxs-lookup"><span data-stu-id="71f18-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="71f18-370">Bezpośrednio po instrukcji `For` deklaruje zmienną licznika (nie trzeba używać `Dim`), a następnie wskazuje zakres, jak w `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="71f18-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="71f18-371">Oznacza to, że zmienna `i` rozpocznie zliczanie w wysokości 10 i kontynuuje do momentu osiągnięcia 20 (włącznie).</span><span class="sxs-lookup"><span data-stu-id="71f18-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="71f18-372">Między instrukcjami `For` i `Next` jest zawartość bloku.</span><span class="sxs-lookup"><span data-stu-id="71f18-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="71f18-373">Może zawierać co najmniej jedną instrukcję Code, która jest wykonywana z każdą pętlą.</span><span class="sxs-lookup"><span data-stu-id="71f18-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="71f18-374">Instrukcja `Next i` zatrzymuje pętlę.</span><span class="sxs-lookup"><span data-stu-id="71f18-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="71f18-375">Zwiększa licznik i uruchamia następną iterację pętli.</span><span class="sxs-lookup"><span data-stu-id="71f18-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="71f18-376">Wiersz kodu między `For` i `Next` wierszy zawiera kod, który jest uruchamiany dla każdej iteracji pętli.</span><span class="sxs-lookup"><span data-stu-id="71f18-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="71f18-377">Znacznik tworzy nowy akapit (`<p>` elementu) za każdym razem i dodaje linię do danych wyjściowych, wyświetlając wartość i (licznik).</span><span class="sxs-lookup"><span data-stu-id="71f18-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="71f18-378">Po uruchomieniu tej strony przykład tworzy 11 wierszy wyświetlających dane wyjściowe, a tekst w każdym wierszu wskazujący numer elementu.</span><span class="sxs-lookup"><span data-stu-id="71f18-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="71f18-380">Jeśli pracujesz z kolekcją lub tablicą, często używasz pętli `For Each`.</span><span class="sxs-lookup"><span data-stu-id="71f18-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="71f18-381">Kolekcja jest grupą podobnych obiektów, a pętla `For Each` umożliwia wykonywanie zadania na każdym elemencie w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="71f18-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="71f18-382">Pętla tego typu jest wygodna dla kolekcji, ponieważ w przeciwieństwie do pętli `For` nie trzeba zwiększać licznika ani ustawiać limitu.</span><span class="sxs-lookup"><span data-stu-id="71f18-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="71f18-383">Zamiast tego kod pętli `For Each` po prostu przechodzi przez kolekcję do momentu zakończenia.</span><span class="sxs-lookup"><span data-stu-id="71f18-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="71f18-384">Ten przykład zwraca elementy z kolekcji `Request.ServerVariables` (która zawiera informacje o serwerze sieci Web).</span><span class="sxs-lookup"><span data-stu-id="71f18-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="71f18-385">Używa pętli `For Each`, aby wyświetlić nazwę każdego elementu przez utworzenie nowego elementu `<li>` na liście punktowanej HTML.</span><span class="sxs-lookup"><span data-stu-id="71f18-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="71f18-386">Po słowie kluczowym `For Each` następuje zmienna, która reprezentuje pojedynczy element w kolekcji (w przykładzie `myItem`), po którym następuje słowo kluczowe `In`, a następnie kolekcja, w której ma się znajdować pętla.</span><span class="sxs-lookup"><span data-stu-id="71f18-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="71f18-387">W treści pętli `For Each` można uzyskać dostęp do bieżącego elementu przy użyciu zmiennej, która została zadeklarowana wcześniej.</span><span class="sxs-lookup"><span data-stu-id="71f18-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="71f18-389">Aby utworzyć więcej pętli ogólnego przeznaczenia, użyj instrukcji `Do While`:</span><span class="sxs-lookup"><span data-stu-id="71f18-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="71f18-390">Ta pętla rozpoczyna się od słowa kluczowego `Do While`, po którym następuje warunek, po którym następuje blok, który ma zostać powtórzony.</span><span class="sxs-lookup"><span data-stu-id="71f18-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="71f18-391">Pętle zwykle zwiększają się (Dodaj do) lub zmniejszają (odejmowanie od) zmiennej lub obiektu używanej do zliczania.</span><span class="sxs-lookup"><span data-stu-id="71f18-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="71f18-392">W przykładzie operator `+=` dodaje 1 do wartości zmiennej za każdym razem, gdy pętla zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="71f18-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="71f18-393">(Aby zmniejszyć zmienną w pętli, która liczy w dół, użyj operatora zmniejszania `-=`).</span><span class="sxs-lookup"><span data-stu-id="71f18-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="71f18-394">Obiekty i kolekcje</span><span class="sxs-lookup"><span data-stu-id="71f18-394">Objects and Collections</span></span>

<span data-ttu-id="71f18-395">Niemal wszystko w witrynie ASP.NET Web to obiekt, w tym sama strona sieci Web.</span><span class="sxs-lookup"><span data-stu-id="71f18-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="71f18-396">W tej sekcji omówiono niektóre ważne obiekty, które często działają w kodzie.</span><span class="sxs-lookup"><span data-stu-id="71f18-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="71f18-397">Obiekty strony</span><span class="sxs-lookup"><span data-stu-id="71f18-397">Page objects</span></span>

<span data-ttu-id="71f18-398">Najbardziej podstawowym obiektem w ASP.NET jest strona.</span><span class="sxs-lookup"><span data-stu-id="71f18-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="71f18-399">Można uzyskać dostęp do właściwości obiektu strony bezpośrednio bez żadnego obiektu kwalifikującego.</span><span class="sxs-lookup"><span data-stu-id="71f18-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="71f18-400">Poniższy kod pobiera ścieżkę pliku strony przy użyciu obiektu `Request` strony:</span><span class="sxs-lookup"><span data-stu-id="71f18-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="71f18-401">Właściwości obiektu `Page` można użyć, aby uzyskać wiele informacji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="71f18-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="71f18-402">`Request`.</span><span class="sxs-lookup"><span data-stu-id="71f18-402">`Request`.</span></span> <span data-ttu-id="71f18-403">Jak już widzisz, jest to zbiór informacji dotyczących bieżącego żądania, w tym typ przeglądarki, która złożyła żądanie, adres URL strony, tożsamość użytkownika itd.</span><span class="sxs-lookup"><span data-stu-id="71f18-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="71f18-404">`Response`.</span><span class="sxs-lookup"><span data-stu-id="71f18-404">`Response`.</span></span> <span data-ttu-id="71f18-405">Jest to zbiór informacji o odpowiedzi (stronie), które zostaną wysłane do przeglądarki po zakończeniu działania kodu serwera.</span><span class="sxs-lookup"><span data-stu-id="71f18-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="71f18-406">Na przykład można użyć tej właściwości do zapisu informacji w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="71f18-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="71f18-407">Obiekty kolekcji (tablice i słowniki)</span><span class="sxs-lookup"><span data-stu-id="71f18-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="71f18-408">Kolekcja jest grupą obiektów tego samego typu, na przykład z kolekcją `Customer` obiektów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="71f18-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="71f18-409">ASP.NET zawiera wiele wbudowanych kolekcji, takich jak kolekcja `Request.Files`.</span><span class="sxs-lookup"><span data-stu-id="71f18-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="71f18-410">Często pracujesz z danymi w kolekcjach.</span><span class="sxs-lookup"><span data-stu-id="71f18-410">You'll often work with data in collections.</span></span> <span data-ttu-id="71f18-411">Dwa popularne typy kolekcji to *Tablica* i *słownik*.</span><span class="sxs-lookup"><span data-stu-id="71f18-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="71f18-412">Tablica jest przydatna, gdy chcesz przechowywać kolekcję podobnych elementów, ale nie chcesz tworzyć oddzielnej zmiennej do przechowywania poszczególnych elementów:</span><span class="sxs-lookup"><span data-stu-id="71f18-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="71f18-413">Z tablicami deklaruje się konkretny typ danych, taki jak `String`, `Integer`lub `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="71f18-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="71f18-414">Aby wskazać, że zmienna może zawierać tablicę, należy dodać nawiasy do nazwy zmiennej w deklaracji (takiej jak `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="71f18-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="71f18-415">Dostęp do elementów w tablicy można uzyskać przy użyciu ich położenia (indeksu) lub instrukcji `For Each`.</span><span class="sxs-lookup"><span data-stu-id="71f18-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="71f18-416">Indeksy tablicy są oparte na &#8212; zero oznaczające, że pierwszy element znajduje się na pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="71f18-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="71f18-417">Możesz określić liczbę elementów w tablicy, pobierając jej Właściwość `Length`.</span><span class="sxs-lookup"><span data-stu-id="71f18-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="71f18-418">Aby uzyskać położenie określonego elementu w tablicy (to oznacza, aby przeszukać tablicę), użyj metody `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="71f18-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="71f18-419">Można również wykonywać operacje, takie jak odwracanie zawartości tablicy (Metoda `Array.Reverse`) lub sortowanie zawartości (Metoda `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="71f18-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="71f18-420">Dane wyjściowe kodu tablicy ciągów wyświetlanego w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="71f18-420">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="71f18-422">Słownik jest kolekcją par klucz/wartość, w których podano klucz (lub nazwę), aby ustawić lub pobrać odpowiednią wartość:</span><span class="sxs-lookup"><span data-stu-id="71f18-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="71f18-423">Aby utworzyć słownik, użyj słowa kluczowego `New`, aby wskazać, że tworzysz nowy obiekt `Dictionary`.</span><span class="sxs-lookup"><span data-stu-id="71f18-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="71f18-424">Można przypisać słownik do zmiennej za pomocą słowa kluczowego `Dim`.</span><span class="sxs-lookup"><span data-stu-id="71f18-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="71f18-425">Możesz wskazać typy danych elementów w słowniku przy użyciu nawiasów (`( )`).</span><span class="sxs-lookup"><span data-stu-id="71f18-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="71f18-426">Na końcu deklaracji należy dodać kolejną parę nawiasów, ponieważ jest to metoda, która tworzy nowy słownik.</span><span class="sxs-lookup"><span data-stu-id="71f18-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="71f18-427">Aby dodać elementy do słownika, można wywołać metodę `Add` zmiennej słownika (`myScores` w tym przypadku), a następnie określić klucz i wartość.</span><span class="sxs-lookup"><span data-stu-id="71f18-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="71f18-428">Alternatywnie można użyć nawiasów, aby wskazać klucz i wykonać proste przypisanie, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="71f18-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="71f18-429">Aby uzyskać wartość ze słownika, należy określić klucz w nawiasach:</span><span class="sxs-lookup"><span data-stu-id="71f18-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="71f18-430">Wywoływanie metod z parametrami</span><span class="sxs-lookup"><span data-stu-id="71f18-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="71f18-431">Jak przedstawiono wcześniej w tym artykule, obiekty, z którymi program ma metody.</span><span class="sxs-lookup"><span data-stu-id="71f18-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="71f18-432">Na przykład obiekt `Database` może mieć metodę `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="71f18-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="71f18-433">Wiele metod ma także jeden lub więcej parametrów.</span><span class="sxs-lookup"><span data-stu-id="71f18-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="71f18-434">*Parametr* jest wartością, którą można przekazać do metody, aby umożliwić wykonywanie jej zadania.</span><span class="sxs-lookup"><span data-stu-id="71f18-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="71f18-435">Na przykład Przyjrzyj się deklaracji dla metody `Request.MapPath`, która przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="71f18-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="71f18-436">Ta metoda zwraca ścieżkę fizyczną na serwerze, który odpowiada określonej ścieżce wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="71f18-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="71f18-437">Trzy parametry dla metody to `virtualPath`, `baseVirtualDir`i `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="71f18-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="71f18-438">(Należy zauważyć, że w deklaracji parametry są wyświetlane z danymi o typach danych, które będą akceptowane). Po wywołaniu tej metody należy podać wartości dla wszystkich trzech parametrów.</span><span class="sxs-lookup"><span data-stu-id="71f18-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="71f18-439">Gdy używasz Visual Basic z składnia Razor, masz dwie opcje przekazywania parametrów do metody: *parametry pozycyjne* lub *nazwane parametry*.</span><span class="sxs-lookup"><span data-stu-id="71f18-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="71f18-440">Aby wywołać metodę przy użyciu parametrów pozycyjnych, należy przekazać parametry w ścisłej kolejności, która jest określona w deklaracji metody.</span><span class="sxs-lookup"><span data-stu-id="71f18-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="71f18-441">(Zazwyczaj znana jest ta kolejność, odczytując dokumentację dla metody). Należy postępować zgodnie z kolejnością, a w razie potrzeby nie można &#8212; pominąć żadnego z parametrów, należy przekazać pusty ciąg (`""`) lub null dla parametru pozycyjnego, dla którego nie ma wartości.</span><span class="sxs-lookup"><span data-stu-id="71f18-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="71f18-442">W poniższym przykładzie przyjęto założenie, że w witrynie sieci Web znajduje się folder o nazwie *skrypty* .</span><span class="sxs-lookup"><span data-stu-id="71f18-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="71f18-443">Kod wywołuje metodę `Request.MapPath` i przekazuje wartości dla trzech parametrów w poprawnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="71f18-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="71f18-444">Następnie zostanie wyświetlona zamapowana ścieżka.</span><span class="sxs-lookup"><span data-stu-id="71f18-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="71f18-445">Jeśli istnieje wiele parametrów dla metody, można zachować przejrzystość kodu i bardziej czytelność przy użyciu nazwanych parametrów.</span><span class="sxs-lookup"><span data-stu-id="71f18-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="71f18-446">Aby wywołać metodę przy użyciu nazwanych parametrów, należy określić nazwę parametru, po którym następuje `:=` a następnie podać wartość.</span><span class="sxs-lookup"><span data-stu-id="71f18-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="71f18-447">Zaletą parametrów nazwanych jest możliwość dodawania ich w dowolnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="71f18-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="71f18-448">(Wadą jest to, że wywołanie metody nie jest jako kompaktowe).</span><span class="sxs-lookup"><span data-stu-id="71f18-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="71f18-449">Poniższy przykład wywołuje tę samą metodę, jak powyżej, ale używa nazwanych parametrów do dostarczenia wartości:</span><span class="sxs-lookup"><span data-stu-id="71f18-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="71f18-450">Jak widać, parametry są przesyłane w innej kolejności.</span><span class="sxs-lookup"><span data-stu-id="71f18-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="71f18-451">Jeśli jednak zostanie uruchomiony poprzedni przykład i ten przykład zwróci tę samą wartość.</span><span class="sxs-lookup"><span data-stu-id="71f18-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="71f18-452">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="71f18-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="71f18-453">Instrukcje try-catch</span><span class="sxs-lookup"><span data-stu-id="71f18-453">Try-Catch statements</span></span>

<span data-ttu-id="71f18-454">Często występują instrukcje w kodzie, które mogą zakończyć się niepowodzeniem z przyczyn poza formantem.</span><span class="sxs-lookup"><span data-stu-id="71f18-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="71f18-455">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="71f18-455">For example:</span></span>

- <span data-ttu-id="71f18-456">Jeśli kod próbuje otworzyć, utworzyć, odczytać lub zapisać plik, mogą wystąpić wszystkie rodzaje błędów.</span><span class="sxs-lookup"><span data-stu-id="71f18-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="71f18-457">Plik, który nie istnieje, może być zablokowany, kod może nie mieć uprawnień i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="71f18-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="71f18-458">Podobnie, jeśli kod próbuje zaktualizować rekordy w bazie danych, mogą wystąpić problemy z uprawnieniami, połączenie z bazą danych może być porzucone, dane do zapisania mogą być nieprawidłowe i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="71f18-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="71f18-459">W warunkach programowania te sytuacje nazywają się *wyjątkami*.</span><span class="sxs-lookup"><span data-stu-id="71f18-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="71f18-460">Jeśli Twój kod napotyka wyjątek, generuje (zgłasza) komunikat o błędzie, który jest, co jest najlepszym rozwiązaniem dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="71f18-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="71f18-462">W sytuacjach, w których kod może napotkać wyjątki i aby uniknąć komunikatów o błędach tego typu, można użyć instrukcji `Try/Catch`.</span><span class="sxs-lookup"><span data-stu-id="71f18-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="71f18-463">W instrukcji `Try` uruchamiasz sprawdzany kod.</span><span class="sxs-lookup"><span data-stu-id="71f18-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="71f18-464">W co najmniej jednej instrukcji `Catch` można wyszukać konkretne błędy (określone typy wyjątków), które mogły wystąpić.</span><span class="sxs-lookup"><span data-stu-id="71f18-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="71f18-465">Możesz dołączyć dowolną liczbę instrukcji `Catch`, ile potrzebujesz, aby wyszukać błędy, które są przewidywane.</span><span class="sxs-lookup"><span data-stu-id="71f18-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="71f18-466">Zalecamy uniknięcie użycia metody `Response.Redirect` w instrukcjach `Try/Catch`, ponieważ może to spowodować wyjątek na stronie.</span><span class="sxs-lookup"><span data-stu-id="71f18-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="71f18-467">Poniższy przykład pokazuje stronę, która tworzy plik tekstowy przy pierwszym żądaniu, a następnie wyświetla przycisk, który umożliwia użytkownikowi otwarcie pliku.</span><span class="sxs-lookup"><span data-stu-id="71f18-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="71f18-468">Przykład celowo używa nieprawidłowej nazwy pliku, aby powodował wyjątek.</span><span class="sxs-lookup"><span data-stu-id="71f18-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="71f18-469">Kod zawiera instrukcje `Catch` dla dwóch możliwych wyjątków: `FileNotFoundException`, które występuje, jeśli nazwa pliku jest zła, i `DirectoryNotFoundException`, który występuje, jeśli ASP.NET nie może odnaleźć folderu.</span><span class="sxs-lookup"><span data-stu-id="71f18-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="71f18-470">(Możesz usunąć komentarz z instrukcji w przykładzie, aby zobaczyć, jak działa, gdy wszystko działa prawidłowo).</span><span class="sxs-lookup"><span data-stu-id="71f18-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="71f18-471">Jeśli kod nie obsłużył wyjątku, zobaczysz stronę błędu, jak poprzedni zrzut ekranu.</span><span class="sxs-lookup"><span data-stu-id="71f18-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="71f18-472">Jednak sekcja `Try/Catch` pozwala zapobiec wyświetlaniu tych typów błędów przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="71f18-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="71f18-473">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="71f18-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="71f18-474">Dokumentacja referencyjna</span><span class="sxs-lookup"><span data-stu-id="71f18-474">Reference Documentation</span></span>

- [<span data-ttu-id="71f18-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="71f18-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="71f18-476">Język Visual Basic</span><span class="sxs-lookup"><span data-stu-id="71f18-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
