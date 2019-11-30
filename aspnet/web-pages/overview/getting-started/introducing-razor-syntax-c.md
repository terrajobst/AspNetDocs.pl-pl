---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składniC#Razor () | Microsoft Docs
author: Rick-Anderson
description: Ten rozdział zawiera omówienie programowania za pomocą ASP.NET stron sieci Web przy użyciu składnia Razor. ASP.NET to technologia firmy Microsoft do uruchamiania dynamicznego sieci Web PA...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2019
ms.locfileid: "74564883"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="8306b-104">Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składniC#Razor ()</span><span class="sxs-lookup"><span data-stu-id="8306b-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>

<span data-ttu-id="8306b-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8306b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8306b-106">Ten artykuł zawiera omówienie programowania za pomocą ASP.NET stron sieci Web przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="8306b-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="8306b-107">ASP.NET to technologia firmy Microsoft do uruchamiania dynamicznych stron sieci Web na serwerach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8306b-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="8306b-108">Ten artykuł koncentruje się na C# korzystaniu z języka programowania.</span><span class="sxs-lookup"><span data-stu-id="8306b-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="8306b-109">Dowiesz **się**:</span><span class="sxs-lookup"><span data-stu-id="8306b-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="8306b-110">8 najważniejszych porad programistycznych dotyczących rozpoczynania pracy z programowaniem ASP.NET stron sieci Web przy użyciu programu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="8306b-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="8306b-111">Podstawowe pojęcia związane z programowaniem, które będą potrzebne.</span><span class="sxs-lookup"><span data-stu-id="8306b-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="8306b-112">Co to jest kod serwera ASP.NET i składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="8306b-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="8306b-113">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="8306b-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="8306b-114">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="8306b-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="8306b-115">Ten samouczek działa również z ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="8306b-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="8306b-116">8 najważniejszych wskazówek dotyczących programowania</span><span class="sxs-lookup"><span data-stu-id="8306b-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="8306b-117">W tej sekcji przedstawiono kilka wskazówek, które należy znać podczas pisania kodu serwera ASP.NET przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="8306b-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="8306b-118">Składnia Razor jest oparta na języku C# programowania i jest to język, który jest najczęściej używany w przypadku stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8306b-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="8306b-119">Jednak składnia Razor obsługuje również język Visual Basic i wszystko, co widzisz, można również w Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="8306b-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="8306b-120">Aby uzyskać szczegółowe informacje, zobacz dodatek [Visual Basic języka i składni](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="8306b-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>

<span data-ttu-id="8306b-121">Więcej informacji o większości tych technik programowania można znaleźć w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="8306b-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="8306b-122">1. Dodaj kod do strony przy użyciu znaku @</span><span class="sxs-lookup"><span data-stu-id="8306b-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="8306b-123">Znak `@` uruchamia wyrażenia śródwierszowe, bloki pojedynczej instrukcji i bloki zawierające wiele instrukcji:</span><span class="sxs-lookup"><span data-stu-id="8306b-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="8306b-124">Te instrukcje wyglądają tak, jak w przypadku uruchamiania strony w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="8306b-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="8306b-126">**Kodowanie HTML**</span><span class="sxs-lookup"><span data-stu-id="8306b-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="8306b-127">Gdy wyświetlasz zawartość na stronie przy użyciu znaku `@`, jak w powyższych przykładach, ASP.NET HTML — koduje dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="8306b-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="8306b-128">Zastępuje to zastrzeżone znaki HTML (takie jak `<` i `>` i `&`) kodami, które umożliwiają wyświetlanie znaków jako znaków na stronie sieci Web zamiast interpretowania ich jako tagów HTML lub jednostek.</span><span class="sxs-lookup"><span data-stu-id="8306b-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="8306b-129">Bez kodowania HTML dane wyjściowe z kodu serwera mogą nie być wyświetlane prawidłowo i mogą uwidaczniać zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="8306b-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="8306b-130">Jeśli celem jest wyprowadzanie kodu HTML, który renderuje Tagi jako znaczniki (na przykład `<p></p>` akapitu lub `<em></em>` w celu wyróżnienia tekstu), zobacz sekcję [łączenie tekstu, znaczników i kodu w blokach kodu w](#BM_CombiningTextMarkupAndCode) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="8306b-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="8306b-131">Więcej informacji na temat kodowania HTML można znaleźć w artykule [Praca z formularzami](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="8306b-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="8306b-132">2. Umieść bloki kodu w nawiasach klamrowych</span><span class="sxs-lookup"><span data-stu-id="8306b-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="8306b-133">*Blok kodu* zawiera jedną lub więcej instrukcji kodu i jest ujęty w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="8306b-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="8306b-134">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="8306b-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="8306b-136">3. wewnątrz bloku każda instrukcja Code kończy się średnikiem</span><span class="sxs-lookup"><span data-stu-id="8306b-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="8306b-137">W bloku kodu Każda pełna instrukcja Code musi kończyć się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="8306b-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="8306b-138">Wyrażenia śródwierszowe nie kończą się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="8306b-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="8306b-139">4. Użyj zmiennych do przechowywania wartości</span><span class="sxs-lookup"><span data-stu-id="8306b-139">4. You use variables to store values</span></span>

<span data-ttu-id="8306b-140">Można przechowywać wartości w *zmiennej*, w tym ciągi, liczby i daty itp. Tworzysz nową zmienną za pomocą słowa kluczowego `var`.</span><span class="sxs-lookup"><span data-stu-id="8306b-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="8306b-141">Można wstawiać wartości zmiennych bezpośrednio na stronie przy użyciu `@`.</span><span class="sxs-lookup"><span data-stu-id="8306b-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="8306b-142">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="8306b-142">The result displayed in a browser:</span></span>

![Razor-IMG3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="8306b-144">5. w podwójnym cudzysłowie należy umieścić wartości ciągu literału</span><span class="sxs-lookup"><span data-stu-id="8306b-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="8306b-145">*Ciąg* jest sekwencją znaków, które są traktowane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="8306b-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="8306b-146">Aby określić ciąg, należy umieścić go w podwójnym cudzysłowie:</span><span class="sxs-lookup"><span data-stu-id="8306b-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="8306b-147">Jeśli ciąg, który ma być wyświetlany, zawiera znak ukośnika odwrotnego (`\`) lub podwójny cudzysłów (`"`), użyj *literału ciągu Verbatim* , który jest poprzedzony operatorem `@`.</span><span class="sxs-lookup"><span data-stu-id="8306b-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="8306b-148">(W C#, znak \ ma specjalne znaczenie, chyba że używasz literału ciągu Verbatim).</span><span class="sxs-lookup"><span data-stu-id="8306b-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="8306b-149">Aby osadzić podwójne cudzysłowy, użyj literału ciągu Verbatim i powtórz cudzysłowy:</span><span class="sxs-lookup"><span data-stu-id="8306b-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="8306b-150">Oto wynik użycia obu przykładów na stronie:</span><span class="sxs-lookup"><span data-stu-id="8306b-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="8306b-152">Należy zauważyć, że znak `@` jest używany zarówno do oznaczania literałów ciągu C# Verbatim w i do oznaczania kodu na stronach ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8306b-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>

### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="8306b-153">6. kod uwzględnia wielkość liter</span><span class="sxs-lookup"><span data-stu-id="8306b-153">6. Code is case sensitive</span></span>

<span data-ttu-id="8306b-154">W C#programie słowa kluczowe (takie jak `var`, `true`i `if`) i nazwy zmiennych uwzględniają wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="8306b-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="8306b-155">Poniższe wiersze kodu tworzą dwie różne zmienne, `lastName` i `LastName.`</span><span class="sxs-lookup"><span data-stu-id="8306b-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="8306b-156">W przypadku deklarowania zmiennej jako `var lastName = "Smith";` i próby odwołującej się do tej zmiennej na stronie jako `@LastName`, otrzymasz wartość `"Jones"` zamiast `"Smith"`.</span><span class="sxs-lookup"><span data-stu-id="8306b-156">If you declare a variable as `var lastName = "Smith";` and try to reference that variable in your page as `@LastName`, you would get the value `"Jones"` instead of `"Smith"`.</span></span>

> [!NOTE]
> <span data-ttu-id="8306b-157">W Visual Basic słowa kluczowe i zmienne *nie* uwzględniają wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="8306b-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>

### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="8306b-158">7. większość kodowania obejmuje obiekty</span><span class="sxs-lookup"><span data-stu-id="8306b-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="8306b-159">*Obiekt* reprezentuje element, który można zaprogramować ze &#8212; stroną, polem tekstowym, plikiem, obrazem, żądaniem sieci Web, wiadomością e-mail, rekordem klienta (wierszem bazy danych) itd. Obiekty mają właściwości opisujące ich cechy, które można odczytać lub zmienić &#8212; obiekt pola tekstowego ma właściwość `Text` (między innymi), obiekt żądania ma właściwość `Url`, wiadomość e-mail ma właściwość `From`, a obiekt klienta ma właściwość `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="8306b-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="8306b-160">Obiekty mają również metody, które są zleceniami &quot;,&quot; mogą być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="8306b-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="8306b-161">Przykłady obejmują metodę `Save` obiektu pliku, metodę `Rotate` obiektu obrazu oraz metodę `Send` obiektu poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="8306b-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="8306b-162">Często pracujesz z obiektem `Request`, który zawiera informacje, takie jak wartości pól tekstowych (pól formularza) na stronie, typ przeglądarki, która złożyła żądanie, adres URL strony, tożsamość użytkownika itd. Poniższy przykład pokazuje, jak uzyskać dostęp do właściwości obiektu `Request` i jak wywołać metodę `MapPath` obiektu `Request`, który zapewnia ścieżkę bezwzględną strony na serwerze:</span><span class="sxs-lookup"><span data-stu-id="8306b-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="8306b-163">Wynik wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="8306b-163">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="8306b-165">8. możesz napisać kod, który podejmuje decyzje</span><span class="sxs-lookup"><span data-stu-id="8306b-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="8306b-166">Kluczową cechą dynamicznych stron sieci Web jest określenie, jakie czynności należy wykonać na podstawie warunków.</span><span class="sxs-lookup"><span data-stu-id="8306b-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="8306b-167">Najbardziej typowym sposobem wykonania tej czynności jest wykonanie instrukcji `if` (i opcjonalnej instrukcji `else`).</span><span class="sxs-lookup"><span data-stu-id="8306b-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="8306b-168">Instrukcja `if(IsPost)` to skrócony sposób pisania `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="8306b-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="8306b-169">Wraz z `if` instrukcjami istnieją różne sposoby testowania warunków, powtarzania bloków kodu i tak dalej, które opisano w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="8306b-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="8306b-170">Wynik wyświetlany w przeglądarce (po kliknięciu przycisku **Prześlij**):</span><span class="sxs-lookup"><span data-stu-id="8306b-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="8306b-172">Metody GET i POST protokołu HTTP oraz Właściwość ispost</span><span class="sxs-lookup"><span data-stu-id="8306b-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="8306b-173">Protokół używany przez strony sieci Web (HTTP) obsługuje bardzo ograniczoną liczbę metod (czasowników), które są używane do żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="8306b-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="8306b-174">Dwa Najczęstsze są pobieranie, które są używane do odczytywania strony i wpisu, które są używane do przesyłania strony.</span><span class="sxs-lookup"><span data-stu-id="8306b-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="8306b-175">Na ogół podczas pierwszego żądania strony użytkownik żąda strony przy użyciu polecenia GET.</span><span class="sxs-lookup"><span data-stu-id="8306b-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="8306b-176">Jeśli użytkownik wypełni formularz, a następnie kliknie przycisk Prześlij, przeglądarka wyśle żądanie POST na serwer.</span><span class="sxs-lookup"><span data-stu-id="8306b-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="8306b-177">W programowaniu sieci Web często warto wiedzieć, czy strona jest żądana jako GET, czy jako wpis, aby poznać sposób przetwarzania strony.</span><span class="sxs-lookup"><span data-stu-id="8306b-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="8306b-178">Na stronach sieci Web ASP.NET można użyć właściwości `IsPost`, aby sprawdzić, czy żądanie jest GET lub POST.</span><span class="sxs-lookup"><span data-stu-id="8306b-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="8306b-179">Jeśli żądanie jest WPISem, właściwość `IsPost` zwróci wartość true i można wykonać czynności, takie jak odczytywanie wartości pól tekstowych w formularzu.</span><span class="sxs-lookup"><span data-stu-id="8306b-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="8306b-180">Wiele przykładów zobaczysz, jak przetwarzać stronę w różny sposób w zależności od wartości `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="8306b-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="8306b-181">Prosty przykład kodu</span><span class="sxs-lookup"><span data-stu-id="8306b-181">A Simple Code Example</span></span>

<span data-ttu-id="8306b-182">Ta procedura pokazuje, jak utworzyć stronę przedstawiającą podstawowe techniki programowania.</span><span class="sxs-lookup"><span data-stu-id="8306b-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="8306b-183">W tym przykładzie utworzysz stronę umożliwiającą użytkownikom wprowadzanie dwóch liczb, a następnie ich dodanie i wyświetlenie wyniku.</span><span class="sxs-lookup"><span data-stu-id="8306b-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="8306b-184">W edytorze Utwórz nowy plik i nadaj mu nazwę *AddNumbers. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8306b-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="8306b-185">Skopiuj poniższy kod i adiustację na stronie, zastępując wszystkie elementy znajdujące się już na stronie.</span><span class="sxs-lookup"><span data-stu-id="8306b-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="8306b-186">Oto kilka rzeczy, dla których warto zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="8306b-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="8306b-187">Znak `@` uruchamia pierwszy blok kodu na stronie i poprzedza zmienną `totalMessage`, która jest osadzona w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="8306b-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="8306b-188">Blok w górnej części strony jest ujęty w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="8306b-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="8306b-189">W bloku w górnej części wszystkie wiersze kończą się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="8306b-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="8306b-190">Zmienne `total`, `num1`, `num2`i `totalMessage` przechowują kilka liczb i ciąg.</span><span class="sxs-lookup"><span data-stu-id="8306b-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="8306b-191">Wartość ciągu literału przypisana do zmiennej `totalMessage` znajduje się w podwójnym cudzysłowie.</span><span class="sxs-lookup"><span data-stu-id="8306b-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="8306b-192">Ponieważ w kodzie jest rozróżniana wielkość liter, gdy zmienna `totalMessage` jest używana w dolnej części strony, jej nazwa musi być zgodna ze zmienną u góry.</span><span class="sxs-lookup"><span data-stu-id="8306b-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="8306b-193">Wyrażenie `num1.AsInt() + num2.AsInt()` pokazuje, jak korzystać z obiektów i metod.</span><span class="sxs-lookup"><span data-stu-id="8306b-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="8306b-194">Metoda `AsInt` dla każdej zmiennej konwertuje ciąg wprowadzony przez użytkownika na liczbę (liczbę całkowitą), dzięki czemu można wykonywać na nim operacje arytmetyczne.</span><span class="sxs-lookup"><span data-stu-id="8306b-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="8306b-195">Tag `<form>` zawiera atrybut `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="8306b-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="8306b-196">Oznacza to, że gdy użytkownik kliknie przycisk **Dodaj**, Strona zostanie wysłana na serwer przy użyciu metody POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8306b-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="8306b-197">Gdy strona zostanie przesłana, test `if(IsPost)` ma wartość true, a kod warunkowy zostanie uruchomiony, wyświetlając wynik dodawania liczb.</span><span class="sxs-lookup"><span data-stu-id="8306b-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="8306b-198">Zapisz stronę i uruchom ją w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8306b-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="8306b-199">(Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Wprowadź dwie liczby całkowite, a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="8306b-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="8306b-201">Podstawowe pojęcia związane z programowaniem</span><span class="sxs-lookup"><span data-stu-id="8306b-201">Basic Programming Concepts</span></span>

<span data-ttu-id="8306b-202">Ten artykuł zawiera omówienie programowania w sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8306b-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="8306b-203">Nie jest to wyczerpujące badanie, a jedynie krótki przewodnik po pojęciach związanych z programowaniem, których najczęściej używasz.</span><span class="sxs-lookup"><span data-stu-id="8306b-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="8306b-204">Nawet w ten sposób obejmuje niemal wszystko, czego potrzebujesz, aby rozpocząć pracę ze stronami sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8306b-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="8306b-205">Jednak pierwsze i małe tło techniczne.</span><span class="sxs-lookup"><span data-stu-id="8306b-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="8306b-206">Składnia Razor, kod serwera i ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8306b-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="8306b-207">Składnia Razor jest prostą składnią programowania dla osadzania kodu opartego na serwerze na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8306b-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="8306b-208">Na stronie sieci Web, która używa składnia Razor, istnieją dwa rodzaje zawartości: zawartość klienta i kod serwera.</span><span class="sxs-lookup"><span data-stu-id="8306b-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="8306b-209">Zawartość klienta to informacje, które są używane na stronach sieci Web: oznakowanie HTML (elementy), takie jak CSS, może być pewnym skryptem, takim jak JavaScript, i zwykłym tekstem.</span><span class="sxs-lookup"><span data-stu-id="8306b-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="8306b-210">Składnia Razor umożliwia dodanie kodu serwera do tej zawartości klienta.</span><span class="sxs-lookup"><span data-stu-id="8306b-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="8306b-211">Jeśli na stronie znajduje się kod serwera, serwer uruchamia najpierw ten kod przed wysłaniem strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8306b-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="8306b-212">Uruchamiając na serwerze, kod może wykonywać zadania, które mogą być znacznie bardziej skomplikowane do korzystania z samej zawartości klienta, takich jak uzyskiwanie dostępu do baz danych opartych na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8306b-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="8306b-213">Co najważniejsze, kod serwera może dynamicznie tworzyć zawartość &#8212; klienta, która może generować znaczniki HTML lub inną zawartość na bieżąco, a następnie wysyłać ją do przeglądarki wraz z dowolnym STATYCZNYM kodem HTML, który może zawierać strona.</span><span class="sxs-lookup"><span data-stu-id="8306b-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="8306b-214">W perspektywie przeglądarki zawartość klienta, która jest generowana przez kod serwera, nie jest inna niż jakakolwiek inna zawartość klienta.</span><span class="sxs-lookup"><span data-stu-id="8306b-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="8306b-215">Jak już widzisz, kod serwera, który jest wymagany, jest bardzo prosty.</span><span class="sxs-lookup"><span data-stu-id="8306b-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="8306b-216">ASP.NET strony sieci Web zawierające składnia Razor mają specjalne rozszerzenie pliku ( *. cshtml* lub *. vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="8306b-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="8306b-217">Serwer rozpoznaje te rozszerzenia, uruchamia kod, który jest oznaczony za pomocą składnia Razor, a następnie wysyła stronę do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8306b-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="8306b-218">Gdzie mieści się ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="8306b-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="8306b-219">Składnia Razor jest oparta na technologii firmy Microsoft o nazwie ASP.NET, która z kolei opiera się na platformie Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="8306b-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="8306b-220">The.NET Framework to obszerna, kompleksowa platforma programistyczna firmy Microsoft do tworzenia praktycznie dowolnego typu aplikacji komputerowej.</span><span class="sxs-lookup"><span data-stu-id="8306b-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="8306b-221">ASP.NET jest częścią .NET Framework, która jest przeznaczona specjalnie do tworzenia aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8306b-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="8306b-222">Deweloperzy korzystali z ASP.NET, aby tworzyć wiele witryn sieci Web o największych i najwyższym ruchu na świecie.</span><span class="sxs-lookup"><span data-stu-id="8306b-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="8306b-223">(W dowolnym momencie zobaczysz nazwę pliku Extension *. aspx* jako część adresu URL w witrynie, zobaczysz, że witryna została zapisywana przy użyciu ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="8306b-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="8306b-224">Składnia Razor zapewnia wszystkie możliwości ASP.NET, ale przy użyciu uproszczonej składni, która jest łatwiejsza do uczenia się, jeśli jesteś początkującym użytkownikiem, który zwiększa produktywność, jeśli jesteś ekspertem.</span><span class="sxs-lookup"><span data-stu-id="8306b-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="8306b-225">Mimo że ta składnia jest prosta do użycia, jej związek z rodziną ASP.NET i .NET Framework oznacza, że w miarę jak witryny sieci Web stają się bardziej zaawansowane, możesz uzyskać dostęp do większych platform.</span><span class="sxs-lookup"><span data-stu-id="8306b-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="8306b-227">**Klasy i wystąpienia**</span><span class="sxs-lookup"><span data-stu-id="8306b-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="8306b-228">Kod serwera ASP.NET korzysta z obiektów, które są oparte na koncepcji klas.</span><span class="sxs-lookup"><span data-stu-id="8306b-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="8306b-229">Klasa jest definicją lub szablonem obiektu.</span><span class="sxs-lookup"><span data-stu-id="8306b-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="8306b-230">Na przykład aplikacja może zawierać klasę `Customer`, która definiuje właściwości i metody wymagane przez każdy obiekt klienta.</span><span class="sxs-lookup"><span data-stu-id="8306b-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="8306b-231">Gdy aplikacja musi pracować z rzeczywistymi informacjami o kliencie, tworzy wystąpienie (lub tworzy *wystąpienia*) obiektu klient.</span><span class="sxs-lookup"><span data-stu-id="8306b-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="8306b-232">Każdy indywidualny klient jest osobnym wystąpieniem klasy `Customer`.</span><span class="sxs-lookup"><span data-stu-id="8306b-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="8306b-233">Każde wystąpienie obsługuje te same właściwości i metody, ale wartości właściwości dla każdego wystąpienia są zwykle różne, ponieważ każdy obiekt klienta jest unikatowy.</span><span class="sxs-lookup"><span data-stu-id="8306b-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="8306b-234">W jednym obiekcie klienta Właściwość `LastName` może mieć wartość "Smith"; w innym obiekcie klienta Właściwość `LastName` może mieć wartość "Nowak".</span><span class="sxs-lookup"><span data-stu-id="8306b-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="8306b-235">Podobnie każda indywidualna Strona sieci Web w swojej witrynie jest obiektem `Page`, który jest wystąpieniem klasy `Page`.</span><span class="sxs-lookup"><span data-stu-id="8306b-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="8306b-236">Przycisk na stronie jest obiektem `Button`, który jest wystąpieniem klasy `Button` i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="8306b-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="8306b-237">Każde wystąpienie ma własne cechy, ale wszystkie są oparte na tym, co jest określone w definicji klasy obiektu.</span><span class="sxs-lookup"><span data-stu-id="8306b-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>

## <a name="basic-syntax"></a><span data-ttu-id="8306b-238">Podstawowa składnia</span><span class="sxs-lookup"><span data-stu-id="8306b-238">Basic Syntax</span></span>

<span data-ttu-id="8306b-239">Wcześniej przedstawiono podstawowy przykład tworzenia strony sieci Web ASP.NET oraz sposób dodawania kodu serwerowego do znacznika HTML.</span><span class="sxs-lookup"><span data-stu-id="8306b-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="8306b-240">Tutaj znajdziesz podstawowe informacje na temat pisania kodu serwera ASP.NET przy użyciu składnia Razor &#8212; , które są regułami języka programowania.</span><span class="sxs-lookup"><span data-stu-id="8306b-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="8306b-241">Jeśli masz doświadczenie z programowaniem (zwłaszcza jeśli użyto języka C, C++, C#, Visual Basic lub JavaScript), większość czytanych tutaj informacji będzie znana.</span><span class="sxs-lookup"><span data-stu-id="8306b-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="8306b-242">Prawdopodobnie trzeba będzie zaznajomić się tylko z informacjami o sposobie dodawania kodu serwera do znaczników w plikach *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8306b-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="8306b-243">Łączenie tekstu, znaczników i kodu w blokach kodu</span><span class="sxs-lookup"><span data-stu-id="8306b-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="8306b-244">W blokach kodu serwera często chcesz na stronie wyprowadzać tekst lub znaczniki (lub obie).</span><span class="sxs-lookup"><span data-stu-id="8306b-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="8306b-245">Jeśli blok kodu serwera zawiera tekst, który nie jest kodem, a zamiast tego powinien być renderowany jako, ASP.NET musi mieć możliwość odróżnienia tego tekstu od kodu.</span><span class="sxs-lookup"><span data-stu-id="8306b-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="8306b-246">Istnieje kilka sposobów, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="8306b-246">There are several ways to do this.</span></span>

- <span data-ttu-id="8306b-247">Ujmij tekst w element HTML, taki jak `<p></p>` lub `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="8306b-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="8306b-248">Element HTML może zawierać tekst, dodatkowe elementy HTML i wyrażenia kodu serwera.</span><span class="sxs-lookup"><span data-stu-id="8306b-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="8306b-249">Gdy ASP.NET widzi tag otwierającego HTML (na przykład `<p>`), renderuje wszystko, w tym element i jego zawartość, w postaci, w jakiej jest przeglądarką, rozpoznawania wyrażeń kodu serwera w miarę ich przeszukiwania.</span><span class="sxs-lookup"><span data-stu-id="8306b-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="8306b-250">Użyj operatora `@:` lub elementu `<text>`.</span><span class="sxs-lookup"><span data-stu-id="8306b-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="8306b-251">`@:` wyprowadza pojedynczy wiersz zawartości zawierający zwykły tekst lub niedopasowane Tagi HTML; element `<text>` obejmuje wiele wierszy do danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="8306b-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="8306b-252">Te opcje są przydatne, gdy nie chcesz renderować elementu HTML jako części danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="8306b-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="8306b-253">Jeśli chcesz wypróbować wiele wierszy tekstu lub niedopasowane Tagi HTML, możesz poprzedzać każdy wiersz za pomocą `@:`lub umieścić wiersz w `<text>` elemencie.</span><span class="sxs-lookup"><span data-stu-id="8306b-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="8306b-254">Podobnie jak w przypadku operatora `@:`, Tagi`<text>` są używane przez ASP.NET do identyfikowania zawartości tekstowej i nigdy nie są renderowane w danych wyjściowych strony.</span><span class="sxs-lookup"><span data-stu-id="8306b-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="8306b-255">Pierwszy przykład powtarza poprzedni przykład, ale używa pojedynczej pary tagów `<text>`, aby umieścić tekst do renderowania.</span><span class="sxs-lookup"><span data-stu-id="8306b-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="8306b-256">W drugim przykładzie Tagi `<text>` i `</text>` zawierają trzy wiersze, z których wszystkie mają niezwierany tekst i niedopasowane Tagi HTML (`<br />`), a także kod serwera i dopasowane Tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="8306b-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="8306b-257">Ponownie można także poprzedzać każdy wiersz pojedynczym operatorem `@:`; w dowolny sposób działa.</span><span class="sxs-lookup"><span data-stu-id="8306b-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8306b-258">Podczas wyprowadzania tekstu, jak pokazano w tej &#8212; sekcji przy użyciu elementu HTML, operatora `@:` lub elementu &#8212; `<text>` ASP.NET nie jest kodowane w języku HTML.</span><span class="sxs-lookup"><span data-stu-id="8306b-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="8306b-259">(Jak wspomniano wcześniej, ASP.NET koduje dane wyjściowe wyrażeń kodów serwera i bloków kodu serwera, które są poprzedzone `@`, z wyjątkiem przypadków specjalnych zanotowanych w tej sekcji).</span><span class="sxs-lookup"><span data-stu-id="8306b-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="8306b-260">Odstępu</span><span class="sxs-lookup"><span data-stu-id="8306b-260">Whitespace</span></span>

<span data-ttu-id="8306b-261">Dodatkowe spacje w instrukcji (i poza literałem ciągu) nie wpływają na instrukcję:</span><span class="sxs-lookup"><span data-stu-id="8306b-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="8306b-262">Podział wiersza w instrukcji nie ma wpływu na instrukcję i można zawinąć instrukcje w celu zapewnienia czytelności.</span><span class="sxs-lookup"><span data-stu-id="8306b-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="8306b-263">Następujące instrukcje są takie same:</span><span class="sxs-lookup"><span data-stu-id="8306b-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="8306b-264">Nie można jednak otoczyć wiersza w środku literału ciągu.</span><span class="sxs-lookup"><span data-stu-id="8306b-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="8306b-265">Poniższy przykład nie działa:</span><span class="sxs-lookup"><span data-stu-id="8306b-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="8306b-266">Aby połączyć długi ciąg, który otacza wiele wierszy, takich jak powyższy kod, dostępne są dwie opcje.</span><span class="sxs-lookup"><span data-stu-id="8306b-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="8306b-267">Możesz użyć operatora łączenia (`+`), który będzie widoczny w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="8306b-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="8306b-268">Można również użyć znaku `@`, aby utworzyć Verbatim literał ciągu, jak zostało to opisane wcześniej w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="8306b-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="8306b-269">Literały ciągu Verbatim można przerwać w wierszach:</span><span class="sxs-lookup"><span data-stu-id="8306b-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="8306b-270">Komentarze kodu (i adiustacji)</span><span class="sxs-lookup"><span data-stu-id="8306b-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="8306b-271">Komentarze umożliwiają wystawianie notatek dla siebie lub dla innych osób.</span><span class="sxs-lookup"><span data-stu-id="8306b-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="8306b-272">Umożliwiają one także wyłączenie (*komentowanie*) sekcji kodu lub znaczników, które nie powinny być uruchamiane, ale które mają być przechowywane na stronie przez czas.</span><span class="sxs-lookup"><span data-stu-id="8306b-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="8306b-273">Istnieje inna składnia komentarzy dla kodu Razor i znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="8306b-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="8306b-274">Podobnie jak w przypadku wszystkich kodów Razor, komentarze Razor są przetwarzane (a następnie usuwane) na serwerze przed wysłaniem strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8306b-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="8306b-275">W związku z tym składnia komentarzy Razor pozwala umieścić komentarze w kodzie (lub nawet w znacznikach), które można zobaczyć podczas edytowania pliku, ale użytkownicy nie widzą nawet w źródle strony.</span><span class="sxs-lookup"><span data-stu-id="8306b-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="8306b-276">W przypadku komentarzy Razor ASP.NET należy uruchomić komentarz z `@*` i zakończyć z `*@`.</span><span class="sxs-lookup"><span data-stu-id="8306b-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="8306b-277">Komentarz może znajdować się w jednym wierszu lub wielu wierszach:</span><span class="sxs-lookup"><span data-stu-id="8306b-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="8306b-278">Oto komentarz w bloku kodu:</span><span class="sxs-lookup"><span data-stu-id="8306b-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="8306b-279">Poniżej znajduje się ten sam blok kodu z wierszem kodu oznaczonym jako komentarz, który nie zostanie uruchomiony:</span><span class="sxs-lookup"><span data-stu-id="8306b-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="8306b-280">W bloku kodu, jako alternatywa dla składni komentarzy Razor, można użyć składni komentarza używanego języka programowania, takiego jak C#:</span><span class="sxs-lookup"><span data-stu-id="8306b-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="8306b-281">W C#programie Komentarze jednowierszowe są poprzedzone `//` znakami, a Komentarze wielowierszowe zaczynają się od `/*` i kończyć `*/`.</span><span class="sxs-lookup"><span data-stu-id="8306b-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="8306b-282">(Podobnie jak w przypadku komentarzy C# Razor, komentarze nie są renderowane do przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="8306b-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="8306b-283">W przypadku znaczników, jak wiesz, możesz utworzyć komentarz HTML:</span><span class="sxs-lookup"><span data-stu-id="8306b-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="8306b-284">Komentarze HTML zaczynają się od `<!--` znaków i kończą się `-->`.</span><span class="sxs-lookup"><span data-stu-id="8306b-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="8306b-285">Możesz użyć komentarzy HTML do otaczania nietylko tekstu, ale również dowolnego znacznika HTML, który można zachować na stronie, ale nie powinien być renderowany.</span><span class="sxs-lookup"><span data-stu-id="8306b-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="8306b-286">Ten komentarz HTML spowoduje ukrycie całej zawartości tagów i zawartego w nim tekstu:</span><span class="sxs-lookup"><span data-stu-id="8306b-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="8306b-287">W przeciwieństwie do komentarzy Razor, komentarze HTML *są* renderowane na stronie, a użytkownik może je zobaczyć, wyświetlając Źródło strony.</span><span class="sxs-lookup"><span data-stu-id="8306b-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="8306b-288">Razor ma ograniczenia dotyczące zagnieżdżonych bloków C#.</span><span class="sxs-lookup"><span data-stu-id="8306b-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="8306b-289">Aby uzyskać więcej informacji [, C# Zobacz nazwane zmienne i zagnieżdżone bloki generują przerwany kod](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="8306b-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="8306b-290">Zmienne</span><span class="sxs-lookup"><span data-stu-id="8306b-290">Variables</span></span>

<span data-ttu-id="8306b-291">Zmienna to nazwany obiekt, który służy do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="8306b-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="8306b-292">Można nazwać zmienne, ale nazwa musi zaczynać się od litery i nie może zawierać spacji ani znaków zarezerwowanych.</span><span class="sxs-lookup"><span data-stu-id="8306b-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="8306b-293">Zmienne i typy danych</span><span class="sxs-lookup"><span data-stu-id="8306b-293">Variables and Data Types</span></span>

<span data-ttu-id="8306b-294">Zmienna może mieć określony typ danych, co wskazuje, jakiego rodzaju dane są przechowywane w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="8306b-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="8306b-295">Można mieć zmienne ciągów, które przechowują wartości ciągu (takie jak &quot;Hello World&quot;), zmienne typu Integer, które przechowują wartości całkowite (takie jak 3 lub 79) i zmienne dat, które przechowują wartości daty w różnych formatach (na przykład 4/12/2012 lub marzec 2009).</span><span class="sxs-lookup"><span data-stu-id="8306b-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="8306b-296">Istnieje wiele innych typów danych, których można użyć.</span><span class="sxs-lookup"><span data-stu-id="8306b-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="8306b-297">Zwykle nie trzeba określać typu dla zmiennej.</span><span class="sxs-lookup"><span data-stu-id="8306b-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="8306b-298">W większości przypadków ASP.NET może ustalić typ na podstawie sposobu użycia danych w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="8306b-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="8306b-299">(Czasami musisz określić typ; zobaczysz przykłady, w których to prawda).</span><span class="sxs-lookup"><span data-stu-id="8306b-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="8306b-300">Należy zadeklarować zmienną za pomocą słowa kluczowego `var` (jeśli nie chcesz określać typu) lub przy użyciu nazwy typu:</span><span class="sxs-lookup"><span data-stu-id="8306b-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="8306b-301">W poniższym przykładzie przedstawiono typowe zastosowania zmiennych na stronie sieci Web:</span><span class="sxs-lookup"><span data-stu-id="8306b-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="8306b-302">Jeśli połączysz poprzednie przykłady na stronie, zobaczysz ten komunikat w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="8306b-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="8306b-304">Konwertowanie i testowanie typów danych</span><span class="sxs-lookup"><span data-stu-id="8306b-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="8306b-305">Chociaż ASP.NET zazwyczaj zwykle określa typ danych, czasami nie jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="8306b-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="8306b-306">W związku z tym może być konieczne ASP.NET przez wykonanie jawnej konwersji.</span><span class="sxs-lookup"><span data-stu-id="8306b-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="8306b-307">Nawet jeśli nie musisz konwertować typów, czasami warto przetestować, aby zobaczyć, jakiego typu dane mogą pracować.</span><span class="sxs-lookup"><span data-stu-id="8306b-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="8306b-308">Najczęstszym przypadkiem jest to, że trzeba przekonwertować ciąg na inny typ, na przykład na liczbę całkowitą lub datę.</span><span class="sxs-lookup"><span data-stu-id="8306b-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="8306b-309">W poniższym przykładzie pokazano typowy przypadek, w którym należy przekonwertować ciąg na liczbę.</span><span class="sxs-lookup"><span data-stu-id="8306b-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="8306b-310">Zgodnie z regułą dane wprowadzane przez użytkownika są uznawane za ciągi.</span><span class="sxs-lookup"><span data-stu-id="8306b-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="8306b-311">Nawet w przypadku wyświetlenia monitu o wprowadzenie liczby, a nawet jeśli nie wprowadzono cyfry, gdy dane wejściowe użytkownika są przesyłane i odczytywane w kodzie, dane są w formacie ciągu.</span><span class="sxs-lookup"><span data-stu-id="8306b-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="8306b-312">W związku z tym należy przekonwertować ciąg na liczbę.</span><span class="sxs-lookup"><span data-stu-id="8306b-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="8306b-313">W przykładzie, jeśli spróbujesz wykonać operacje arytmetyczne na wartościach bez ich konwersji, następujące wyniki błędu, ponieważ ASP.NET nie mogą dodać dwóch ciągów:</span><span class="sxs-lookup"><span data-stu-id="8306b-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="8306b-314">*Nie można niejawnie przekonwertować typu "String" na "int".*</span><span class="sxs-lookup"><span data-stu-id="8306b-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="8306b-315">Aby przekonwertować wartości na liczby całkowite, należy wywołać metodę `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="8306b-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="8306b-316">Jeśli konwersja zakończy się pomyślnie, można dodać liczby.</span><span class="sxs-lookup"><span data-stu-id="8306b-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="8306b-317">W poniższej tabeli wymieniono niektóre typowe metody konwersji i testowania zmiennych.</span><span class="sxs-lookup"><span data-stu-id="8306b-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="8306b-318"><strong>Method</strong></span><span class="sxs-lookup"><span data-stu-id="8306b-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-319"><strong>Opis</strong></span><span class="sxs-lookup"><span data-stu-id="8306b-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-320"><strong>Przykład</strong></span><span class="sxs-lookup"><span data-stu-id="8306b-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-321">Konwertuje ciąg reprezentujący liczbę całkowitą (na przykład "593") na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="8306b-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-322">Konwertuje ciąg, taki jak &quot;true&quot; lub &quot;false&quot; na typ Boolean.</span><span class="sxs-lookup"><span data-stu-id="8306b-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-323">Konwertuje ciąg, który ma wartość dziesiętną, taką jak &quot;1,3&quot; lub &quot;7,439&quot; do liczby zmiennoprzecinkowej.</span><span class="sxs-lookup"><span data-stu-id="8306b-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-324">Konwertuje ciąg, który ma wartość dziesiętną, taką jak &quot;1,3&quot; lub &quot;7,439&quot; do liczby dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="8306b-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="8306b-325">(W ASP.NET liczba dziesiętna jest bardziej precyzyjna niż liczba zmiennoprzecinkowa).</span><span class="sxs-lookup"><span data-stu-id="8306b-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-326">Konwertuje ciąg, który reprezentuje wartość daty i godziny dla typu `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8306b-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-327">Konwertuje wszystkie inne typy danych na ciąg.</span><span class="sxs-lookup"><span data-stu-id="8306b-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="8306b-328">Operatory</span><span class="sxs-lookup"><span data-stu-id="8306b-328">Operators</span></span>

<span data-ttu-id="8306b-329">Operator jest słowem kluczowym lub znakiem, który informuje ASP.NET jakiego rodzaju polecenia wykonać w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="8306b-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="8306b-330">C# Język (i składnia Razor na jego podstawie) obsługuje wiele operatorów, ale wystarczy tylko rozpoznać kilka, aby rozpocząć.</span><span class="sxs-lookup"><span data-stu-id="8306b-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="8306b-331">Poniższa tabela zawiera podsumowanie najbardziej typowych operatorów.</span><span class="sxs-lookup"><span data-stu-id="8306b-331">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="8306b-332"><strong>Zakład</strong></span><span class="sxs-lookup"><span data-stu-id="8306b-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-333"><strong>Opis</strong></span><span class="sxs-lookup"><span data-stu-id="8306b-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-334"><strong>Przykłady</strong></span><span class="sxs-lookup"><span data-stu-id="8306b-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="8306b-335">`+` `-` `*` `/`</span><span class="sxs-lookup"><span data-stu-id="8306b-335">`+` `-` `*` `/`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-336">Operatory matematyczne używane w wyrażeniach liczbowych.</span><span class="sxs-lookup"><span data-stu-id="8306b-336">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-337">Przypisanie.</span><span class="sxs-lookup"><span data-stu-id="8306b-337">Assignment.</span></span> <span data-ttu-id="8306b-338">Przypisuje wartość po prawej stronie instrukcji do obiektu po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="8306b-338">Assigns the value on the right side of a statement to the object on the left side.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-339">Kryteri.</span><span class="sxs-lookup"><span data-stu-id="8306b-339">Equality.</span></span> <span data-ttu-id="8306b-340">Zwraca `true`, jeśli wartości są równe.</span><span class="sxs-lookup"><span data-stu-id="8306b-340">Returns `true` if the values are equal.</span></span> <span data-ttu-id="8306b-341">(Zauważ rozróżnienie między operatorem `=` i operatorem `==`).</span><span class="sxs-lookup"><span data-stu-id="8306b-341">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-342">Nierówności.</span><span class="sxs-lookup"><span data-stu-id="8306b-342">Inequality.</span></span> <span data-ttu-id="8306b-343">Zwraca `true`, jeśli wartości nie są równe.</span><span class="sxs-lookup"><span data-stu-id="8306b-343">Returns `true` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-344">Mniejsze niż, większe niż, mniejsze niż lub równe, i większe niż lub równe.</span><span class="sxs-lookup"><span data-stu-id="8306b-344">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-345">Łączenie, które jest używane do dołączania ciągów.</span><span class="sxs-lookup"><span data-stu-id="8306b-345">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="8306b-346">ASP.NET wie różnicę między tym operatorem a operatorem dodawania na podstawie typu danych wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="8306b-346">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="8306b-347">`+=``-=`</span><span class="sxs-lookup"><span data-stu-id="8306b-347">`+=` `-=`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-348">Operatory zwiększania i zmniejszania, które dodają i odejmujeją 1 (odpowiednio) od zmiennej.</span><span class="sxs-lookup"><span data-stu-id="8306b-348">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-349">Kropka.</span><span class="sxs-lookup"><span data-stu-id="8306b-349">Dot.</span></span> <span data-ttu-id="8306b-350">Używane do rozróżniania obiektów i ich właściwości i metod.</span><span class="sxs-lookup"><span data-stu-id="8306b-350">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-351">Nawiasów.</span><span class="sxs-lookup"><span data-stu-id="8306b-351">Parentheses.</span></span> <span data-ttu-id="8306b-352">Używane do grupowania wyrażeń i przekazywania parametrów do metod.</span><span class="sxs-lookup"><span data-stu-id="8306b-352">Used to group expressions and to pass parameters to methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-353">Nawias.</span><span class="sxs-lookup"><span data-stu-id="8306b-353">Brackets.</span></span> <span data-ttu-id="8306b-354">Służy do uzyskiwania dostępu do wartości w tablicach lub kolekcjach.</span><span class="sxs-lookup"><span data-stu-id="8306b-354">Used for accessing values in arrays or collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-355">Niemożliwe.</span><span class="sxs-lookup"><span data-stu-id="8306b-355">Not.</span></span> <span data-ttu-id="8306b-356">Odwraca `true` wartość do `false` i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="8306b-356">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="8306b-357">Zwykle używany jako skrócony sposób testowania dla `false` (to oznacza, że nie `true`).</span><span class="sxs-lookup"><span data-stu-id="8306b-357">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="8306b-358">`&&``||`</span><span class="sxs-lookup"><span data-stu-id="8306b-358">`&&` `||`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="8306b-359">Logiczne i i lub, które są używane do łączenia warunków.</span><span class="sxs-lookup"><span data-stu-id="8306b-359">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="8306b-360">Praca z ścieżkami plików i folderów w kodzie</span><span class="sxs-lookup"><span data-stu-id="8306b-360">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="8306b-361">Często pracujesz ze ścieżkami plików i folderów w kodzie.</span><span class="sxs-lookup"><span data-stu-id="8306b-361">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="8306b-362">Oto przykład struktury folderów fizycznych dla witryny sieci Web, która może pojawić się na komputerze deweloperskim:</span><span class="sxs-lookup"><span data-stu-id="8306b-362">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="8306b-363">Poniżej przedstawiono niektóre istotne informacje dotyczące adresów URL i ścieżek:</span><span class="sxs-lookup"><span data-stu-id="8306b-363">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="8306b-364">Adres URL rozpoczyna się od nazwy domeny (`http://www.example.com`) lub nazwy serwera (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="8306b-364">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="8306b-365">Adres URL odpowiada ścieżce fizycznej na komputerze-hoście.</span><span class="sxs-lookup"><span data-stu-id="8306b-365">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="8306b-366">Na przykład `http://myserver` mogą odpowiadać *C:\websites\mywebsite* folderu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8306b-366">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="8306b-367">Ścieżka wirtualna jest skrótem do reprezentowania ścieżek w kodzie bez konieczności określania pełnej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="8306b-367">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="8306b-368">Zawiera część adresu URL, która następuje po nazwie domeny lub serwera.</span><span class="sxs-lookup"><span data-stu-id="8306b-368">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="8306b-369">W przypadku używania ścieżek wirtualnych można przenieść kod do innej domeny lub serwera bez konieczności aktualizacji ścieżek.</span><span class="sxs-lookup"><span data-stu-id="8306b-369">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="8306b-370">Oto przykład, który pomoże zrozumieć różnice:</span><span class="sxs-lookup"><span data-stu-id="8306b-370">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="8306b-371">Pełny adres URL</span><span class="sxs-lookup"><span data-stu-id="8306b-371">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="8306b-372">Nazwa serwera</span><span class="sxs-lookup"><span data-stu-id="8306b-372">Server name</span></span> | <span data-ttu-id="8306b-373">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="8306b-373">*mycompanyserver*</span></span> |
| <span data-ttu-id="8306b-374">Ścieżka wirtualna</span><span class="sxs-lookup"><span data-stu-id="8306b-374">Virtual path</span></span> | <span data-ttu-id="8306b-375">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="8306b-375">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="8306b-376">Ścieżka fizyczna</span><span class="sxs-lookup"><span data-stu-id="8306b-376">Physical path</span></span> | <span data-ttu-id="8306b-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="8306b-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="8306b-378">Wirtualny katalog główny jest/, podobnie jak katalog główny dysku C:.</span><span class="sxs-lookup"><span data-stu-id="8306b-378">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="8306b-379">(Ścieżki folderów wirtualnych zawsze używają ukośników). Ścieżka wirtualna folderu nie musi mieć takiej samej nazwy jak folder fizyczny; może to być alias.</span><span class="sxs-lookup"><span data-stu-id="8306b-379">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="8306b-380">(Na serwerach produkcyjnych ścieżka wirtualna rzadko jest zgodna z dokładną ścieżką fizyczną).</span><span class="sxs-lookup"><span data-stu-id="8306b-380">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="8306b-381">Podczas pracy z plikami i folderami w kodzie czasami trzeba odwoływać się do ścieżki fizycznej i czasami ścieżki wirtualnej, w zależności od obiektów, z którymi pracujesz.</span><span class="sxs-lookup"><span data-stu-id="8306b-381">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="8306b-382">ASP.NET udostępnia następujące narzędzia do pracy z ścieżkami plików i folderów w kodzie: Metoda `Server.MapPath` i operator `~` i Metoda `Href`.</span><span class="sxs-lookup"><span data-stu-id="8306b-382">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="8306b-383">Konwertowanie wirtualnej na ścieżki fizyczne: Metoda Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="8306b-383">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="8306b-384">Metoda `Server.MapPath` Konwertuje ścieżkę wirtualną (na przykład */default.cshtml*) na bezwzględną ścieżkę fizyczną (na przykład *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="8306b-384">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="8306b-385">Tej metody należy używać w dowolnym momencie, gdy potrzebna jest kompletna ścieżka fizyczna.</span><span class="sxs-lookup"><span data-stu-id="8306b-385">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="8306b-386">Typowym przykładem jest odczytywanie lub zapisywanie pliku tekstowego lub pliku obrazu na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8306b-386">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="8306b-387">Zwykle nie jest znana absolutna ścieżka fizyczna witryny na serwerze lokacji hostingu, dlatego ta metoda może przekonwertować ścieżkę, którą znasz — ścieżkę wirtualną — do odpowiedniej ścieżki na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8306b-387">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="8306b-388">Ścieżka wirtualna do pliku lub folderu zostanie przekazana do metody i zwraca ścieżkę fizyczną:</span><span class="sxs-lookup"><span data-stu-id="8306b-388">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="8306b-389">Odwoływanie się do wirtualnego katalogu głównego: Metoda ~ operatora i href</span><span class="sxs-lookup"><span data-stu-id="8306b-389">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="8306b-390">W pliku *. cshtml* lub *. vbhtml* można odwoływać się do wirtualnej ścieżki katalogu głównego przy użyciu operatora `~`.</span><span class="sxs-lookup"><span data-stu-id="8306b-390">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="8306b-391">Jest to bardzo przydatne, ponieważ można przenieść strony w obrębie witryny, a wszystkie linki, które zawierają do innych stron, nie zostaną zerwane.</span><span class="sxs-lookup"><span data-stu-id="8306b-391">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="8306b-392">Jest on również przydatny w przypadku przenoszenia witryny sieci Web do innej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="8306b-392">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="8306b-393">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="8306b-393">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="8306b-394">Jeśli witryna sieci Web jest `http://myserver/myapp`, Oto, jak ASP.NET będzie traktować te ścieżki podczas uruchamiania strony:</span><span class="sxs-lookup"><span data-stu-id="8306b-394">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="8306b-395">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="8306b-395">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="8306b-396">`myStyleSheet`: `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="8306b-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="8306b-397">(Te ścieżki nie będą widoczne w rzeczywistości jako wartości zmiennej, ale ASP.NET będą traktować ścieżki tak, jakby były.)</span><span class="sxs-lookup"><span data-stu-id="8306b-397">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="8306b-398">Operatora `~` można użyć zarówno w kodzie serwera (jak powyżej), jak to:</span><span class="sxs-lookup"><span data-stu-id="8306b-398">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="8306b-399">W znacznikach można użyć operatora `~`, aby tworzyć ścieżki do zasobów, takich jak pliki obrazów, inne strony sieci Web i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="8306b-399">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="8306b-400">Po uruchomieniu strony ASP.NET przegląda stronę (kod i znacznik) i rozwiązuje wszystkie `~` odwołania do odpowiedniej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="8306b-400">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="8306b-401">Logika warunkowa i pętle</span><span class="sxs-lookup"><span data-stu-id="8306b-401">Conditional Logic and Loops</span></span>

<span data-ttu-id="8306b-402">Kod serwera ASP.NET umożliwia wykonywanie zadań w oparciu o warunki i pisanie kodu, który powtarza instrukcje w konkretnym czasie (czyli kodzie, który uruchamia pętlę).</span><span class="sxs-lookup"><span data-stu-id="8306b-402">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="8306b-403">Warunki testowania</span><span class="sxs-lookup"><span data-stu-id="8306b-403">Testing Conditions</span></span>

<span data-ttu-id="8306b-404">Aby przetestować prosty warunek, użyj instrukcji `if`, która zwraca wartość PRAWDA lub FAŁSZ w oparciu o określony test:</span><span class="sxs-lookup"><span data-stu-id="8306b-404">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="8306b-405">Słowo kluczowe `if` uruchamia blok.</span><span class="sxs-lookup"><span data-stu-id="8306b-405">The `if` keyword starts a block.</span></span> <span data-ttu-id="8306b-406">Rzeczywisty test (warunek) jest w nawiasach i zwraca wartość PRAWDA lub FAŁSZ.</span><span class="sxs-lookup"><span data-stu-id="8306b-406">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="8306b-407">Instrukcje, które są uruchamiane, jeśli test ma wartość true, są ujęte w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="8306b-407">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="8306b-408">Instrukcja `if` może zawierać blok `else`, który określa instrukcje do uruchomienia, jeśli warunek ma wartość false:</span><span class="sxs-lookup"><span data-stu-id="8306b-408">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="8306b-409">Można dodać wiele warunków przy użyciu bloku `else if`:</span><span class="sxs-lookup"><span data-stu-id="8306b-409">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="8306b-410">W tym przykładzie, jeśli pierwszy warunek w bloku if nie ma wartości true, sprawdzana jest `else if` warunek.</span><span class="sxs-lookup"><span data-stu-id="8306b-410">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="8306b-411">Jeśli ten warunek jest spełniony, instrukcje w bloku `else if` są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="8306b-411">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="8306b-412">Jeśli żaden z warunków nie zostanie spełniony, instrukcje w bloku `else` są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="8306b-412">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="8306b-413">Można dodać dowolną liczbę innych, jeśli bloki, a następnie zamknąć blok `else`, jak &quot;wszystko inne&quot; warunku.</span><span class="sxs-lookup"><span data-stu-id="8306b-413">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="8306b-414">Aby przetestować dużą liczbę warunków, użyj bloku `switch`:</span><span class="sxs-lookup"><span data-stu-id="8306b-414">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="8306b-415">Wartość do przetestowania jest w nawiasach (w przykładzie zmienna `weekday`).</span><span class="sxs-lookup"><span data-stu-id="8306b-415">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="8306b-416">Każdy test używa instrukcji `case` kończącej się dwukropkiem (:).</span><span class="sxs-lookup"><span data-stu-id="8306b-416">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="8306b-417">Jeśli wartość instrukcji `case` pasuje do wartości testowej, kod w tym bloku Case jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="8306b-417">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="8306b-418">Każdą instrukcję Case należy zamknąć za pomocą instrukcji `break`.</span><span class="sxs-lookup"><span data-stu-id="8306b-418">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="8306b-419">(Jeśli zapomnisz uwzględnić przerwy w każdym bloku `case`, kod z następnej `case` instrukcji zostanie również uruchomiony). Blok `switch` często ma instrukcję `default` jako ostatni przypadek dla &quot;wszystkiego inne&quot; opcji, która jest uruchamiana, jeśli żaden z innych przypadków nie ma wartości true.</span><span class="sxs-lookup"><span data-stu-id="8306b-419">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="8306b-420">Wynik ostatnich dwóch bloków warunkowych wyświetlanych w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="8306b-420">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="8306b-422">Kod zapętlenia</span><span class="sxs-lookup"><span data-stu-id="8306b-422">Looping Code</span></span>

<span data-ttu-id="8306b-423">Często trzeba wielokrotnie uruchamiać te same instrukcje.</span><span class="sxs-lookup"><span data-stu-id="8306b-423">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="8306b-424">Można to zrobić przez zapętlenie.</span><span class="sxs-lookup"><span data-stu-id="8306b-424">You do this by looping.</span></span> <span data-ttu-id="8306b-425">Na przykład często uruchamiasz te same instrukcje dla każdego elementu w kolekcji danych.</span><span class="sxs-lookup"><span data-stu-id="8306b-425">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="8306b-426">Jeśli wiesz dokładnie, ile razy chcesz wykonać pętlę, możesz użyć pętli `for`.</span><span class="sxs-lookup"><span data-stu-id="8306b-426">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="8306b-427">Ten rodzaj pętli jest szczególnie przydatny do zliczania w górę lub w dół:</span><span class="sxs-lookup"><span data-stu-id="8306b-427">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="8306b-428">Pętla zaczyna się od słowa kluczowego `for`, a następnie trzy instrukcje w nawiasach, każda zakończona średnikiem.</span><span class="sxs-lookup"><span data-stu-id="8306b-428">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="8306b-429">Wewnątrz nawiasów Pierwsza instrukcja (`var i=10;`) tworzy licznik i inicjuje go do 10.</span><span class="sxs-lookup"><span data-stu-id="8306b-429">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="8306b-430">Nie musisz nazwać licznika `i` &#8212; można użyć dowolnej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="8306b-430">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="8306b-431">Po uruchomieniu pętli `for` licznik jest automatycznie zwiększany.</span><span class="sxs-lookup"><span data-stu-id="8306b-431">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="8306b-432">Druga instrukcja (`i < 21;`) ustawia warunek, dla którego chcesz obliczyć liczbę.</span><span class="sxs-lookup"><span data-stu-id="8306b-432">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="8306b-433">W tym przypadku chcemy, aby przeszedł maksymalnie 20 (czyli kontynuować, gdy wartość licznika jest mniejsza niż 21).</span><span class="sxs-lookup"><span data-stu-id="8306b-433">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="8306b-434">Trzecia instrukcja (`i++`) używa operatora przyrostu, który po prostu określa, że licznik powinien mieć 1 dodany do niego przy każdym uruchomieniu pętli.</span><span class="sxs-lookup"><span data-stu-id="8306b-434">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="8306b-435">Wewnątrz nawiasów klamrowych jest kod, który będzie uruchamiany dla każdej iteracji pętli.</span><span class="sxs-lookup"><span data-stu-id="8306b-435">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="8306b-436">Znacznik powoduje utworzenie nowego akapitu (`<p>` elementu) za każdym razem i dodanie linii do danych wyjściowych, wyświetlając wartość `i` (licznik).</span><span class="sxs-lookup"><span data-stu-id="8306b-436">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="8306b-437">Po uruchomieniu tej strony przykład tworzy 11 wierszy wyświetlających dane wyjściowe, a tekst w każdym wierszu wskazujący numer elementu.</span><span class="sxs-lookup"><span data-stu-id="8306b-437">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="8306b-439">Jeśli pracujesz z kolekcją lub tablicą, często używasz pętli `foreach`.</span><span class="sxs-lookup"><span data-stu-id="8306b-439">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="8306b-440">Kolekcja jest grupą podobnych obiektów, a pętla `foreach` umożliwia wykonywanie zadania na każdym elemencie w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8306b-440">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="8306b-441">Pętla tego typu jest wygodna dla kolekcji, ponieważ w przeciwieństwie do pętli `for` nie trzeba zwiększać licznika ani ustawiać limitu.</span><span class="sxs-lookup"><span data-stu-id="8306b-441">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="8306b-442">Zamiast tego kod pętli `foreach` po prostu przechodzi przez kolekcję do momentu zakończenia.</span><span class="sxs-lookup"><span data-stu-id="8306b-442">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="8306b-443">Na przykład poniższy kod zwraca elementy w kolekcji `Request.ServerVariables`, która jest obiektem, który zawiera informacje o serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8306b-443">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="8306b-444">Używa pętli `foreac` h, aby wyświetlić nazwę każdego elementu przez utworzenie nowego elementu `<li>` na liście punktowanej HTML.</span><span class="sxs-lookup"><span data-stu-id="8306b-444">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="8306b-445">Po słowie kluczowym `foreach` następuje nawiasy, w którym deklarujesz zmienną reprezentującą pojedynczy element w kolekcji (w przykładzie `var item`), po której następuje słowo kluczowe `in`, a następnie kolekcja, w której chcesz umieścić pętlę.</span><span class="sxs-lookup"><span data-stu-id="8306b-445">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="8306b-446">W treści pętli `foreach` można uzyskać dostęp do bieżącego elementu przy użyciu zmiennej, która została zadeklarowana wcześniej.</span><span class="sxs-lookup"><span data-stu-id="8306b-446">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="8306b-448">Aby utworzyć więcej pętli ogólnego przeznaczenia, użyj instrukcji `while`:</span><span class="sxs-lookup"><span data-stu-id="8306b-448">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="8306b-449">Pętla `while` rozpoczyna się od słowa kluczowego `while`, a następnie nawiasów, w których można określić czas, w którym pętla jest kontynuowana (w tym przypadku, tak długo, jak `countNum` jest mniejsza niż 50), a następnie blok, który ma zostać powtórzony.</span><span class="sxs-lookup"><span data-stu-id="8306b-449">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="8306b-450">Pętle zwykle zwiększają się (Dodaj do) lub zmniejszają (odejmowanie od) zmiennej lub obiektu używanej do zliczania.</span><span class="sxs-lookup"><span data-stu-id="8306b-450">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="8306b-451">W przykładzie operator `+=` dodaje 1 do `countNum` za każdym razem, gdy pętla zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="8306b-451">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="8306b-452">(Aby zmniejszyć zmienną w pętli, która liczy w dół, użyj operatora zmniejszania `-=`).</span><span class="sxs-lookup"><span data-stu-id="8306b-452">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="8306b-453">Obiekty i kolekcje</span><span class="sxs-lookup"><span data-stu-id="8306b-453">Objects and Collections</span></span>

<span data-ttu-id="8306b-454">Niemal wszystko w witrynie ASP.NET Web to obiekt, w tym sama strona sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8306b-454">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="8306b-455">W tej sekcji omówiono niektóre ważne obiekty, które często działają w kodzie.</span><span class="sxs-lookup"><span data-stu-id="8306b-455">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="8306b-456">Obiekty strony</span><span class="sxs-lookup"><span data-stu-id="8306b-456">Page Objects</span></span>

<span data-ttu-id="8306b-457">Najbardziej podstawowym obiektem w ASP.NET jest strona.</span><span class="sxs-lookup"><span data-stu-id="8306b-457">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="8306b-458">Można uzyskać dostęp do właściwości obiektu strony bezpośrednio bez żadnego obiektu kwalifikującego.</span><span class="sxs-lookup"><span data-stu-id="8306b-458">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="8306b-459">Poniższy kod pobiera ścieżkę pliku strony przy użyciu obiektu `Request` strony:</span><span class="sxs-lookup"><span data-stu-id="8306b-459">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="8306b-460">Aby wyczyścić, że odwołujesz się do właściwości i metod na bieżącym obiekcie strony, możesz opcjonalnie użyć słowa kluczowego `this`, aby reprezentować obiekt strony w kodzie.</span><span class="sxs-lookup"><span data-stu-id="8306b-460">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="8306b-461">Oto poprzedni przykład kodu z `this` dodany do reprezentowania strony:</span><span class="sxs-lookup"><span data-stu-id="8306b-461">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="8306b-462">Właściwości obiektu `Page` można użyć, aby uzyskać wiele informacji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="8306b-462">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="8306b-463">`Request`.</span><span class="sxs-lookup"><span data-stu-id="8306b-463">`Request`.</span></span> <span data-ttu-id="8306b-464">Jak już widzisz, jest to zbiór informacji dotyczących bieżącego żądania, w tym typ przeglądarki, która złożyła żądanie, adres URL strony, tożsamość użytkownika itd.</span><span class="sxs-lookup"><span data-stu-id="8306b-464">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="8306b-465">`Response`.</span><span class="sxs-lookup"><span data-stu-id="8306b-465">`Response`.</span></span> <span data-ttu-id="8306b-466">Jest to zbiór informacji o odpowiedzi (stronie), które zostaną wysłane do przeglądarki po zakończeniu działania kodu serwera.</span><span class="sxs-lookup"><span data-stu-id="8306b-466">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="8306b-467">Na przykład można użyć tej właściwości do zapisu informacji w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8306b-467">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="8306b-468">Obiekty kolekcji (tablice i słowniki)</span><span class="sxs-lookup"><span data-stu-id="8306b-468">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="8306b-469">*Kolekcja* jest grupą obiektów tego samego typu, na przykład z kolekcją `Customer` obiektów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8306b-469">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="8306b-470">ASP.NET zawiera wiele wbudowanych kolekcji, takich jak kolekcja `Request.Files`.</span><span class="sxs-lookup"><span data-stu-id="8306b-470">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="8306b-471">Często pracujesz z danymi w kolekcjach.</span><span class="sxs-lookup"><span data-stu-id="8306b-471">You'll often work with data in collections.</span></span> <span data-ttu-id="8306b-472">Dwa popularne typy kolekcji to *Tablica* i *słownik*.</span><span class="sxs-lookup"><span data-stu-id="8306b-472">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="8306b-473">Tablica jest przydatna, gdy chcesz przechowywać kolekcję podobnych elementów, ale nie chcesz tworzyć oddzielnej zmiennej do przechowywania poszczególnych elementów:</span><span class="sxs-lookup"><span data-stu-id="8306b-473">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="8306b-474">Z tablicami deklaruje się konkretny typ danych, taki jak `string`, `int`lub `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="8306b-474">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="8306b-475">Aby wskazać, że zmienna może zawierać tablicę, należy dodać nawiasy do deklaracji (na przykład `string[]` lub `int[]`).</span><span class="sxs-lookup"><span data-stu-id="8306b-475">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="8306b-476">Dostęp do elementów w tablicy można uzyskać przy użyciu ich położenia (indeksu) lub instrukcji `foreach`.</span><span class="sxs-lookup"><span data-stu-id="8306b-476">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="8306b-477">Indeksy tablicy są oparte na &#8212; zero oznaczające, że pierwszy element znajduje się na pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="8306b-477">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="8306b-478">Możesz określić liczbę elementów w tablicy, pobierając jej Właściwość `Length`.</span><span class="sxs-lookup"><span data-stu-id="8306b-478">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="8306b-479">Aby uzyskać położenie określonego elementu w tablicy (aby przeszukać tablicę), użyj metody `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="8306b-479">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="8306b-480">Można również wykonywać operacje, takie jak odwracanie zawartości tablicy (Metoda `Array.Reverse`) lub sortowanie zawartości (Metoda `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="8306b-480">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="8306b-481">Dane wyjściowe kodu tablicy ciągów wyświetlanego w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="8306b-481">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="8306b-483">Słownik jest kolekcją par klucz/wartość, w których podano klucz (lub nazwę), aby ustawić lub pobrać odpowiednią wartość:</span><span class="sxs-lookup"><span data-stu-id="8306b-483">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="8306b-484">Aby utworzyć słownik, użyj słowa kluczowego `new`, aby wskazać, że tworzysz nowy obiekt słownika.</span><span class="sxs-lookup"><span data-stu-id="8306b-484">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="8306b-485">Można przypisać słownik do zmiennej za pomocą słowa kluczowego `var`.</span><span class="sxs-lookup"><span data-stu-id="8306b-485">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="8306b-486">Możesz wskazać typy danych elementów w słowniku przy użyciu nawiasów ostrych (`< >`).</span><span class="sxs-lookup"><span data-stu-id="8306b-486">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="8306b-487">Na końcu deklaracji należy dodać parę nawiasów, ponieważ jest to metoda, która tworzy nowy słownik.</span><span class="sxs-lookup"><span data-stu-id="8306b-487">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="8306b-488">Aby dodać elementy do słownika, można wywołać metodę `Add` zmiennej słownika (`myScores` w tym przypadku), a następnie określić klucz i wartość.</span><span class="sxs-lookup"><span data-stu-id="8306b-488">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="8306b-489">Alternatywnie możesz użyć nawiasów kwadratowych, aby wskazać klucz i wykonać proste przypisanie, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="8306b-489">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="8306b-490">Aby uzyskać wartość z słownika, należy określić klucz w nawiasach:</span><span class="sxs-lookup"><span data-stu-id="8306b-490">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="8306b-491">Wywoływanie metod z parametrami</span><span class="sxs-lookup"><span data-stu-id="8306b-491">Calling Methods with Parameters</span></span>

<span data-ttu-id="8306b-492">Podczas odczytywania wcześniej w tym artykule obiekty, z którymi program korzysta, mogą mieć metody.</span><span class="sxs-lookup"><span data-stu-id="8306b-492">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="8306b-493">Na przykład obiekt `Database` może mieć metodę `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="8306b-493">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="8306b-494">Wiele metod ma także jeden lub więcej parametrów.</span><span class="sxs-lookup"><span data-stu-id="8306b-494">Many methods also have one or more parameters.</span></span> <span data-ttu-id="8306b-495">*Parametr* jest wartością, którą można przekazać do metody, aby umożliwić wykonywanie jej zadania.</span><span class="sxs-lookup"><span data-stu-id="8306b-495">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="8306b-496">Na przykład Przyjrzyj się deklaracji dla metody `Request.MapPath`, która przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="8306b-496">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="8306b-497">(Wiersz został opakowany, aby zwiększyć jego czytelność.</span><span class="sxs-lookup"><span data-stu-id="8306b-497">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="8306b-498">Należy pamiętać, że podziały wierszy można umieścić niemal w dowolnym miejscu, z wyjątkiem wewnątrz ciągów, które są ujęte w cudzysłów.)</span><span class="sxs-lookup"><span data-stu-id="8306b-498">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="8306b-499">Ta metoda zwraca ścieżkę fizyczną na serwerze, który odpowiada określonej ścieżce wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="8306b-499">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="8306b-500">Trzy parametry dla metody to `virtualPath`, `baseVirtualDir`i `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="8306b-500">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="8306b-501">(Należy zauważyć, że w deklaracji parametry są wyświetlane z danymi o typach danych, które będą akceptowane). Po wywołaniu tej metody należy podać wartości dla wszystkich trzech parametrów.</span><span class="sxs-lookup"><span data-stu-id="8306b-501">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="8306b-502">Składnia Razor oferuje dwie opcje przekazywania parametrów do metody: *parametry pozycyjne* i *nazwane parametry*.</span><span class="sxs-lookup"><span data-stu-id="8306b-502">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="8306b-503">Aby wywołać metodę przy użyciu parametrów pozycyjnych, należy przekazać parametry w ścisłej kolejności, która jest określona w deklaracji metody.</span><span class="sxs-lookup"><span data-stu-id="8306b-503">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="8306b-504">(Zazwyczaj znana jest ta kolejność, odczytując dokumentację dla metody). Należy postępować zgodnie z kolejnością, a w razie potrzeby nie można &#8212; pominąć żadnego z parametrów, należy przekazać pusty ciąg (`""`) lub `null` dla parametru pozycyjnego, dla którego nie ma wartości.</span><span class="sxs-lookup"><span data-stu-id="8306b-504">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="8306b-505">W poniższym przykładzie przyjęto założenie, że w witrynie sieci Web znajduje się folder o nazwie *skrypty* .</span><span class="sxs-lookup"><span data-stu-id="8306b-505">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="8306b-506">Kod wywołuje metodę `Request.MapPath` i przekazuje wartości dla trzech parametrów w poprawnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="8306b-506">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="8306b-507">Następnie zostanie wyświetlona zamapowana ścieżka.</span><span class="sxs-lookup"><span data-stu-id="8306b-507">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="8306b-508">Gdy metoda ma wiele parametrów, można zachować kod bardziej czytelny przy użyciu nazwanych parametrów.</span><span class="sxs-lookup"><span data-stu-id="8306b-508">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="8306b-509">Aby wywołać metodę przy użyciu parametrów nazwanych, należy określić nazwę parametru, po którym następuje dwukropek (:), a następnie wartość.</span><span class="sxs-lookup"><span data-stu-id="8306b-509">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="8306b-510">Zaletą parametrów nazwanych jest możliwość przekazywania ich w dowolnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="8306b-510">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="8306b-511">(Wadą jest to, że wywołanie metody nie jest jako kompaktowe).</span><span class="sxs-lookup"><span data-stu-id="8306b-511">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="8306b-512">Poniższy przykład wywołuje tę samą metodę, jak powyżej, ale używa nazwanych parametrów do dostarczenia wartości:</span><span class="sxs-lookup"><span data-stu-id="8306b-512">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="8306b-513">Jak widać, parametry są przesyłane w innej kolejności.</span><span class="sxs-lookup"><span data-stu-id="8306b-513">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="8306b-514">Jeśli jednak zostanie uruchomiony poprzedni przykład i ten przykład zwróci tę samą wartość.</span><span class="sxs-lookup"><span data-stu-id="8306b-514">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="8306b-515">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="8306b-515">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="8306b-516">Instrukcje try-catch</span><span class="sxs-lookup"><span data-stu-id="8306b-516">Try-Catch Statements</span></span>

<span data-ttu-id="8306b-517">Często występują instrukcje w kodzie, które mogą zakończyć się niepowodzeniem z przyczyn poza formantem.</span><span class="sxs-lookup"><span data-stu-id="8306b-517">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="8306b-518">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8306b-518">For example:</span></span>

- <span data-ttu-id="8306b-519">Jeśli kod próbuje utworzyć plik lub uzyskać do niego dostęp, mogą wystąpić wszystkie rodzaje błędów.</span><span class="sxs-lookup"><span data-stu-id="8306b-519">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="8306b-520">Plik, który nie istnieje, może być zablokowany, kod może nie mieć uprawnień i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="8306b-520">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="8306b-521">Podobnie, jeśli kod próbuje zaktualizować rekordy w bazie danych, mogą wystąpić problemy z uprawnieniami, połączenie z bazą danych może być porzucone, dane do zapisania mogą być nieprawidłowe i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="8306b-521">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="8306b-522">W warunkach programowania te sytuacje nazywają się *wyjątkami*.</span><span class="sxs-lookup"><span data-stu-id="8306b-522">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="8306b-523">Jeśli Twój kod napotyka wyjątek, generuje (zgłasza) komunikat o błędzie, który jest najprawdopodobniej przez użytkownika:</span><span class="sxs-lookup"><span data-stu-id="8306b-523">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="8306b-525">W sytuacjach, w których kod może napotkać wyjątki i aby uniknąć komunikatów o błędach tego typu, można użyć instrukcji `try/catch`.</span><span class="sxs-lookup"><span data-stu-id="8306b-525">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="8306b-526">W instrukcji `try` uruchamiasz sprawdzany kod.</span><span class="sxs-lookup"><span data-stu-id="8306b-526">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="8306b-527">W co najmniej jednej instrukcji `catch` można wyszukać konkretne błędy (określone typy wyjątków), które mogły wystąpić.</span><span class="sxs-lookup"><span data-stu-id="8306b-527">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="8306b-528">Możesz dołączyć dowolną liczbę instrukcji `catch`, ile potrzebujesz, aby wyszukać błędy, które są przewidywane.</span><span class="sxs-lookup"><span data-stu-id="8306b-528">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="8306b-529">Zalecamy uniknięcie użycia metody `Response.Redirect` w instrukcjach `try/catch`, ponieważ może to spowodować wyjątek na stronie.</span><span class="sxs-lookup"><span data-stu-id="8306b-529">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="8306b-530">Poniższy przykład pokazuje stronę, która tworzy plik tekstowy przy pierwszym żądaniu, a następnie wyświetla przycisk, który umożliwia użytkownikowi otwarcie pliku.</span><span class="sxs-lookup"><span data-stu-id="8306b-530">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="8306b-531">Przykład celowo używa nieprawidłowej nazwy pliku, aby powodował wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8306b-531">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="8306b-532">Kod zawiera instrukcje `catch` dla dwóch możliwych wyjątków: `FileNotFoundException`, które występuje, jeśli nazwa pliku jest zła, i `DirectoryNotFoundException`, który występuje, jeśli ASP.NET nie może odnaleźć folderu.</span><span class="sxs-lookup"><span data-stu-id="8306b-532">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="8306b-533">(Możesz usunąć komentarz z instrukcji w przykładzie, aby zobaczyć, jak działa, gdy wszystko działa prawidłowo).</span><span class="sxs-lookup"><span data-stu-id="8306b-533">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="8306b-534">Jeśli kod nie obsłużył wyjątku, zobaczysz stronę błędu, jak poprzedni zrzut ekranu.</span><span class="sxs-lookup"><span data-stu-id="8306b-534">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="8306b-535">Jednak sekcja `try/catch` pozwala zapobiec wyświetlaniu tych typów błędów przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8306b-535">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="8306b-536">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="8306b-536">Additional Resources</span></span>

<span data-ttu-id="8306b-537">**Programowanie przy użyciu Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="8306b-537">**Programming with Visual Basic**</span></span>

[<span data-ttu-id="8306b-538">Dodatek: Visual Basic język i składnia</span><span class="sxs-lookup"><span data-stu-id="8306b-538">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)

<span data-ttu-id="8306b-539">**Dokumentacja referencyjna**</span><span class="sxs-lookup"><span data-stu-id="8306b-539">**Reference Documentation**</span></span>

[<span data-ttu-id="8306b-540">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8306b-540">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="8306b-541">C#Językowe</span><span class="sxs-lookup"><span data-stu-id="8306b-541">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
