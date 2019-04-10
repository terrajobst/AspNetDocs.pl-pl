---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym rozdziale umożliwia przegląd programowania przy użyciu stron ASP.NET Web Pages z użyciem składni Razor. ASP.NET to technologia firmy Microsoft dotyczące uruchamiania dynamicznych sieci Web...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 8237dc6b925ccefc5b411aebc8e7c399dcdc6746
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59407357"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="3deac-104">Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (C#)</span><span class="sxs-lookup"><span data-stu-id="3deac-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>

<span data-ttu-id="3deac-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3deac-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3deac-106">W tym artykule umożliwia przegląd programowania przy użyciu stron ASP.NET Web Pages z użyciem składni Razor.</span><span class="sxs-lookup"><span data-stu-id="3deac-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="3deac-107">ASP.NET to technologia firmy Microsoft dotyczące uruchamiania dynamicznych stron sieci web na serwerach sieci web.</span><span class="sxs-lookup"><span data-stu-id="3deac-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="3deac-108">To zespoły artykuły na temat korzystania z języka programowania C#.</span><span class="sxs-lookup"><span data-stu-id="3deac-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="3deac-109">**Dowiesz się**:</span><span class="sxs-lookup"><span data-stu-id="3deac-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="3deac-110">Najważniejsze 8, porady dotyczące wprowadzenie do programowania stron sieci Web platformy ASP.NET używająca składni Razor programowania.</span><span class="sxs-lookup"><span data-stu-id="3deac-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="3deac-111">Podstawowe pojęcia programowania, które będą potrzebne.</span><span class="sxs-lookup"><span data-stu-id="3deac-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="3deac-112">Jaki kod serwera programu ASP.NET o składni Razor są wszystkie informacje.</span><span class="sxs-lookup"><span data-stu-id="3deac-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="3deac-113">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="3deac-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="3deac-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3deac-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3deac-115">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="3deac-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="3deac-116">Najważniejsze 8 porady dotyczące programowania</span><span class="sxs-lookup"><span data-stu-id="3deac-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="3deac-117">W tej sekcji przedstawiono kilka wskazówek, które bezwzględnie musisz wiedzieć, po rozpoczęciu pisania kodu serwera ASP.NET używająca składni Razor.</span><span class="sxs-lookup"><span data-stu-id="3deac-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="3deac-118">Składnia Razor zależy od języka programowania C#, a to język, który jest używany w większości przypadków przy użyciu stron ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="3deac-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="3deac-119">Jednak składni Razor obsługuje również języka Visual Basic i wszystko, co widać, że możesz również wykonać w języku Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="3deac-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="3deac-120">Aby uzyskać więcej informacji, zobacz dodatku [języka Visual Basic i składni](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="3deac-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="3deac-121">Więcej informacji na temat większości z tych technik programowania można znaleźć w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="3deac-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="3deac-122">1. Możesz dodać kod do strony przy użyciu znaku @</span><span class="sxs-lookup"><span data-stu-id="3deac-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="3deac-123">`@` Znak rozpoczyna się w tekście wyrażeń, bloków pojedynczej instrukcji i bloków wielu instrukcji:</span><span class="sxs-lookup"><span data-stu-id="3deac-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="3deac-124">Jest to, te instrukcje wyglądać po uruchomieniu na stronie w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="3deac-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **<span data-ttu-id="3deac-126">Kodowanie HTML</span><span class="sxs-lookup"><span data-stu-id="3deac-126">HTML Encoding</span></span>**
> 
> <span data-ttu-id="3deac-127">Podczas wyświetlania zawartości na stronie za pomocą `@` znaków, jak w poprzednich przykładach, ASP.NET koduje jako HTML dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="3deac-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="3deac-128">Spowoduje to zastąpienie zastrzeżone znaki HTML (takie jak `<` i `>` i `&`) przy użyciu kodów, umożliwiające znaków, które mają być wyświetlane jako znaki na stronie sieci web, a interpretowany jako tagów HTML lub jednostki.</span><span class="sxs-lookup"><span data-stu-id="3deac-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="3deac-129">Zakodowany w formacie HTML, dane wyjściowe z kodu serwera mogą być wyświetlane nieprawidłowo i spowodować narażenie na zagrożenia bezpieczeństwa strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="3deac-130">Jeśli dowiesz się, jak dane wyjściowe kod znaczników HTML, który renderuje tagi jako kod znaczników (na przykład `<p></p>` akapitu lub `<em></em>` aby wyróżnić tekst), zobacz sekcję [łączenie tekstu, znaczników i kodu w blokach kodu](#BM_CombiningTextMarkupAndCode) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="3deac-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="3deac-131">Możesz dowiedzieć się więcej o kodowanie HTML w [Praca z formularzami](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="3deac-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="3deac-132">2. Bloki kodu są ująć w nawiasy klamrowe</span><span class="sxs-lookup"><span data-stu-id="3deac-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="3deac-133">A *blok kodu* zawiera jedną lub więcej instrukcji kodu i jest ujęty w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="3deac-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="3deac-134">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="3deac-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="3deac-136">3. Wewnątrz bloku zakończenia każda instrukcja kodu przy użyciu średnika</span><span class="sxs-lookup"><span data-stu-id="3deac-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="3deac-137">Wewnątrz bloku kodu każda instrukcja kompletny kod musi być zakończona średnikiem.</span><span class="sxs-lookup"><span data-stu-id="3deac-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="3deac-138">Wbudowane wyrażenia nie kończy się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="3deac-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="3deac-139">4. W przypadku używania zmiennych do przechowywania wartości</span><span class="sxs-lookup"><span data-stu-id="3deac-139">4. You use variables to store values</span></span>

<span data-ttu-id="3deac-140">Można przechowywać wartości w *zmiennej*, w tym ciągi, liczby i daty, itp. Możesz utworzyć nową zmienną za pomocą `var` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="3deac-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="3deac-141">Wartości zmiennych można wstawić bezpośrednio na stronie za pomocą `@`.</span><span class="sxs-lookup"><span data-stu-id="3deac-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="3deac-142">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="3deac-142">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="3deac-144">5. Ujmij wartości literału ciągu w znaki podwójnego cudzysłowu</span><span class="sxs-lookup"><span data-stu-id="3deac-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="3deac-145">A *ciąg* jest sekwencją znaków, które są traktowane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="3deac-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="3deac-146">Aby określić ciąg, ująć w znaki cudzysłowu:</span><span class="sxs-lookup"><span data-stu-id="3deac-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="3deac-147">Jeśli ciąg, który chcesz wyświetlić zawiera znak ukośnika odwrotnego ( `\` ) lub podwójny cudzysłów ( `"` ), użyj *verbatim literału ciągu* , jest poprzedzony znakiem `@` operatora.</span><span class="sxs-lookup"><span data-stu-id="3deac-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="3deac-148">(W języku C#, \ znak ma specjalne znaczenie, chyba że używasz verbatim literału ciągu.)</span><span class="sxs-lookup"><span data-stu-id="3deac-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="3deac-149">Aby osadzić podwójne znaki cudzysłowu, użyć literału ciągu verbatim i powtórz znaków cudzysłowu:</span><span class="sxs-lookup"><span data-stu-id="3deac-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="3deac-150">W tym miejscu jest wynikiem użycia obu tych przykładów na stronie:</span><span class="sxs-lookup"><span data-stu-id="3deac-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="3deac-152">Należy zauważyć, że `@` znak jest używany zarówno do oznaczenia verbatim literałów w języku C#, jak i do oznaczania kodu na stronach ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3deac-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="3deac-153">6. Kod jest uwzględniana wielkość liter</span><span class="sxs-lookup"><span data-stu-id="3deac-153">6. Code is case sensitive</span></span>

<span data-ttu-id="3deac-154">W języku C# słowa kluczowe (takie jak `var`, `true`, i `if`) i w nazwach zmiennych jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="3deac-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="3deac-155">Następujące wiersze kodu tworzą dwie różne zmienne, `lastName` i</span><span class="sxs-lookup"><span data-stu-id="3deac-155">The following lines of code create two different variables, `lastName` and</span></span> `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="3deac-156">Jeśli zadeklarujesz zmienną `var lastName = "Smith";` i jeśli zostanie podjęta próba odwoływać się do tej zmiennej na stronie jako `@LastName`, powoduje błąd, ponieważ `LastName` nie zostanie rozpoznane.</span><span class="sxs-lookup"><span data-stu-id="3deac-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="3deac-157">W języku Visual Basic słowa kluczowe i zmienne są *nie* z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="3deac-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="3deac-158">7. Obejmuje większość kodowania obiektów</span><span class="sxs-lookup"><span data-stu-id="3deac-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="3deac-159">*Obiektu* reprezentuje rzecz, którą można programować za pomocą &#8212; strony pola tekstowego, pliku, obraz, żądania sieci web, wiadomości e-mail, rekord klienta (wiersz bazy danych), itp. Obiekty mają właściwości, które opisują ich właściwości i może czytać lub zmieniać &#8212; ma obiekt pola tekstowego `Text` właściwości (między innymi), obiekt żądania ma `Url` właściwość, wiadomość e-mail ma `From` właściwości i obiekt klienta ma `FirstName` właściwości.</span><span class="sxs-lookup"><span data-stu-id="3deac-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="3deac-160">Obiektów ma także metody, które są &quot;zleceń&quot; mogą wykonywać.</span><span class="sxs-lookup"><span data-stu-id="3deac-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="3deac-161">Przykłady obejmują obiekt pliku `Save` metodę, obiekt obrazu `Rotate` metody i obiekt e-mail `Send` metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="3deac-162">Często będziesz pracować `Request` obiektów, które zapewnia informacje, takie jak wartości pól tekstowych (pola formularza) na stronie, jakiego rodzaju przeglądarki wysłał żądanie, adres URL strony, tożsamość użytkownika, itp. Poniższy przykład pokazuje, jak uzyskiwanie dostępu do właściwości `Request` obiektu i wywoływania `MapPath` metody `Request` obiektu, który zapewnia ścieżkę bezwzględną strony na serwerze:</span><span class="sxs-lookup"><span data-stu-id="3deac-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="3deac-163">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="3deac-163">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="3deac-165">8. Można napisać kod, który podejmuje decyzje</span><span class="sxs-lookup"><span data-stu-id="3deac-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="3deac-166">Kluczową funkcją dynamicznych stron sieci web jest, czy można określić, co należy zrobić, na podstawie warunków.</span><span class="sxs-lookup"><span data-stu-id="3deac-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="3deac-167">Jest najbardziej popularny sposób, w tym celu `if` — instrukcja (i opcjonalnie `else` instrukcji).</span><span class="sxs-lookup"><span data-stu-id="3deac-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="3deac-168">Wykonywanie instrukcji `if(IsPost)` jest skrót sposobem pisania `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="3deac-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="3deac-169">Wraz z `if` instrukcji, istnieje wiele sposobów, aby przetestować warunki, powtórz bloki kodu, i itd., które są opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="3deac-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="3deac-170">Wynik wyświetlany w przeglądarce (po kliknięciu przycisku **przesyłania**):</span><span class="sxs-lookup"><span data-stu-id="3deac-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="3deac-172">HTTP GET i POST metod i właściwości IsPost</span><span class="sxs-lookup"><span data-stu-id="3deac-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="3deac-173">Protokół używany dla stron sieci web (HTTP) obsługuje bardzo ograniczoną liczbę metod (poleceń), które są używane na wysyłanie żądań do serwera.</span><span class="sxs-lookup"><span data-stu-id="3deac-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="3deac-174">Dwie najbardziej typowe to GET, który służy do odczytu strony i WPIS, który jest używany do przesyłania strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="3deac-175">Ogólnie rzecz biorąc po raz pierwszy użytkownik zgłasza żądanie strony, strony jest przesyłane przy użyciu GET.</span><span class="sxs-lookup"><span data-stu-id="3deac-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="3deac-176">Jeśli użytkownik wypełnia formularz, a następnie kliknie przycisk przesyłania, przeglądarki wysyła żądanie POST do serwera.</span><span class="sxs-lookup"><span data-stu-id="3deac-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="3deac-177">W programowanie dla sieci web jest często grupowaniu można sprawdzić, czy strona jest wymagana jako GET lub POST, aby znać sposób przetwarzania tej strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="3deac-178">W składniku ASP.NET Web Pages można użyć `IsPost` właściwość, aby sprawdzić, czy żądanie GET lub POST.</span><span class="sxs-lookup"><span data-stu-id="3deac-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="3deac-179">Jeśli żądanie jest żądaniem POST `IsPost` właściwość zostanie zwrócona wartość PRAWDA, a można wykonywać takie czynności, takich jak odczyt wartości pola tekstowe w formularzu.</span><span class="sxs-lookup"><span data-stu-id="3deac-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="3deac-180">Wiele przykładów zobaczysz pokazują, jak można przetworzyć strony inaczej w zależności od wartości `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="3deac-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="3deac-181">Prosty przykład kodu</span><span class="sxs-lookup"><span data-stu-id="3deac-181">A Simple Code Example</span></span>

<span data-ttu-id="3deac-182">Ta procedura pokazuje, jak utworzyć stronę, która przedstawia podstawowe techniki programowania.</span><span class="sxs-lookup"><span data-stu-id="3deac-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="3deac-183">W tym przykładzie utworzysz stronę, która umożliwia użytkownikom wprowadź dwie liczby, a następnie dodanie ich i wyświetla wynik.</span><span class="sxs-lookup"><span data-stu-id="3deac-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="3deac-184">W edytorze, Utwórz nowy plik i nadaj mu nazwę *AddNumbers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3deac-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="3deac-185">Skopiuj następujący kod i znaczników do strony, zastępując wszystko już na stronie.</span><span class="sxs-lookup"><span data-stu-id="3deac-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="3deac-186">Oto kilka rzeczy, umożliwiające należy zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="3deac-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="3deac-187">`@` Znak rozpoczyna się pierwszego bloku kodu na stronie i poprzedza ono `totalMessage` zmiennej, która jest osadzony w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="3deac-188">Blok, w górnej części strony jest ujęty w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="3deac-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="3deac-189">W bloku u góry wszystkie wiersze kończyć średnikiem.</span><span class="sxs-lookup"><span data-stu-id="3deac-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="3deac-190">Zmienne `total`, `num1`, `num2`, i `totalMessage` przechowywać kilka liczb i ciąg.</span><span class="sxs-lookup"><span data-stu-id="3deac-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="3deac-191">Wartość literału ciągu przypisana do `totalMessage` zmiennej znajduje się w znaki podwójnego cudzysłowu.</span><span class="sxs-lookup"><span data-stu-id="3deac-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="3deac-192">Ponieważ kod jest rozróżniana wielkość liter, kiedy `totalMessage` zmienna jest używana w dolnej części strony, jego nazwa musi dokładnie pasować zmiennej u góry.</span><span class="sxs-lookup"><span data-stu-id="3deac-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="3deac-193">Wyrażenie `num1.AsInt() + num2.AsInt()` pokazuje, jak pracować z obiektami i metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="3deac-194">`AsInt` Metody dla każdej zmiennej konwertuje ciąg wprowadzanego przez użytkownika numer (liczba całkowita) tak, aby wykonywać operacje arytmetyczne na nim.</span><span class="sxs-lookup"><span data-stu-id="3deac-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="3deac-195">`<form>` Znacznik obejmuje `method="post"` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3deac-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="3deac-196">Określa to, że gdy użytkownik kliknie **Dodaj**, strony, które będą wysyłane do serwera przy użyciu metody POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3deac-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="3deac-197">Po przesłaniu strony `if(IsPost)` testu daje w wyniku wartość PRAWDA, a warunkowy kod działa, wyświetlanie wynikiem dodania liczb.</span><span class="sxs-lookup"><span data-stu-id="3deac-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="3deac-198">Zapisz stronę i uruchom go w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="3deac-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="3deac-199">(Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.) Wprowadź dwoma liczbami całkowitymi, a następnie kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="3deac-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="3deac-201">Podstawowe pojęcia związane z programowaniem</span><span class="sxs-lookup"><span data-stu-id="3deac-201">Basic Programming Concepts</span></span>

<span data-ttu-id="3deac-202">Ten artykuł zawiera omówienie programowania dla sieci web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3deac-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="3deac-203">Nie jest wyczerpująca badania, po prostu krótkiego przewodnika za pośrednictwem pojęcia dotyczące programowania, które będą używane najczęściej.</span><span class="sxs-lookup"><span data-stu-id="3deac-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="3deac-204">Nawet w takim przypadku obejmują prawie wszystko, które będą potrzebne, aby rozpocząć pracę przy użyciu stron ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="3deac-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="3deac-205">Ale pierwszym, nieco wiedzę techniczną.</span><span class="sxs-lookup"><span data-stu-id="3deac-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="3deac-206">Składnia Razor, kod serwera i programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3deac-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="3deac-207">Składnia razor jest prostą składnię programowania do osadzania kodu na serwerze, na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="3deac-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="3deac-208">Na stronie sieci web, która używa składni Razor, istnieją dwa rodzaje zawartości: Kod klienta zawartości i serwera.</span><span class="sxs-lookup"><span data-stu-id="3deac-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="3deac-209">Zawartość klienta jest rzeczy, których użyto do na stronach sieci web: Kod znaczników HTML (elementy), informacje dotyczące stylu, takich jak CSS, być może niektóre skrypt po stronie klienta takich jak JavaScript i zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="3deac-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="3deac-210">Składnia razor pozwala dodać kod serwera do tej zawartości klienta.</span><span class="sxs-lookup"><span data-stu-id="3deac-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="3deac-211">Jeśli na stronie znajduje się kod serwera, na serwerze działa ten kod najpierw przed wysłaniem strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3deac-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="3deac-212">Uruchamiając na serwerze, kod może wykonać zadania, które mogą być o wiele bardziej złożone, aby zrobić za pomocą samodzielnie, takich jak uzyskiwanie dostępu do serwera bazy danych zawartości klienta.</span><span class="sxs-lookup"><span data-stu-id="3deac-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="3deac-213">Co najważniejsze, kod serwera dynamicznie tworzyć zawartość klienta &#8212; można wygenerować kod znaczników HTML lub inną zawartość na bieżąco i wysłać go do przeglądarki, wraz z statyczny kod HTML, który może zawierać strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="3deac-214">Z perspektywy przeglądarki nie różni się od innych zawartości klienta jest zawartości klienta, który jest generowany przez kod serwera.</span><span class="sxs-lookup"><span data-stu-id="3deac-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="3deac-215">Jak możesz już możliwość przekonania się, kod serwera, który jest wymagany jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="3deac-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="3deac-216">Strony sieci web ASP.NET zawierających składnię Razor z rozszerzeniem pliku specjalne (*.cshtml* lub *.vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="3deac-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="3deac-217">Serwer rozpoznaje te rozszerzenia, uruchamia kod, który jest oznaczony przy użyciu składni Razor, a następnie wysyła strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3deac-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="3deac-218">Gdy program ASP.NET pasują do</span><span class="sxs-lookup"><span data-stu-id="3deac-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="3deac-219">Składnia razor jest oparty na technologii firmy Microsoft o nazwie ASP.NET, która z kolei zależy od Microsoft .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3deac-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="3deac-220">Środowiska.NET Framework to dużych, kompleksowe środowisko programowania firmy Microsoft do tworzenia praktycznie dowolnego typu aplikacji na komputerze.</span><span class="sxs-lookup"><span data-stu-id="3deac-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="3deac-221">ASP.NET jest częścią programu .NET Framework, który jest specjalnie przeznaczony do tworzenia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="3deac-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="3deac-222">Deweloperzy używanych platformy ASP.NET można tworzyć wiele witryn sieci Web z najwyższą ruchu i największą na świecie.</span><span class="sxs-lookup"><span data-stu-id="3deac-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="3deac-223">(Dowolnej chwili wyświetlić rozszerzenie nazwy pliku *.aspx* jako część adresu URL w witrynie, będziesz wiedzieć, że witryna została opracowana za pomocą programu ASP.NET.)</span><span class="sxs-lookup"><span data-stu-id="3deac-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="3deac-224">Składnia Razor zapewnia wszystkich możliwości platformy ASP.NET, ale przy użyciu uproszczoną skłądnią, która jest łatwiejsze dowiedzieć się, czy jesteś początkującym użytkownikiem, który udostępnia możesz zwiększyć produktywność gdy jesteś ekspertem.</span><span class="sxs-lookup"><span data-stu-id="3deac-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="3deac-225">Mimo że ta składnia jest łatwa w użyciu, jego relację rodziny na platformie ASP.NET i .NET Framework oznacza, że w miarę bardziej zaawansowanych witryn sieci Web trzeba możliwości większych struktur, dostępne dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="3deac-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **<span data-ttu-id="3deac-227">Klasy i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="3deac-227">Classes and Instances</span></span>**
> 
> <span data-ttu-id="3deac-228">Obiekty, które z kolei są oparte na pomysł klasy korzysta z kodu serwera ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3deac-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="3deac-229">Klasa jest definicja lub szablon dla obiektu.</span><span class="sxs-lookup"><span data-stu-id="3deac-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="3deac-230">Na przykład aplikacja może zawierać `Customer` klasę, która definiuje właściwości i metody, których potrzebuje dowolnego obiektu klienta.</span><span class="sxs-lookup"><span data-stu-id="3deac-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="3deac-231">Aplikacja musi pracować z informacjami o klientach rzeczywiste, tworzy wystąpienie (lub *tworzy*) obiektu klienta.</span><span class="sxs-lookup"><span data-stu-id="3deac-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="3deac-232">Poszczególnych klientów są osobne wystąpienie `Customer` klasy.</span><span class="sxs-lookup"><span data-stu-id="3deac-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="3deac-233">Każde wystąpienie obsługuje te same właściwości i metody, ale dla każdego wystąpienia wartości właściwości różnią się zwykle, ponieważ każdy obiekt klienta jest unikatowa.</span><span class="sxs-lookup"><span data-stu-id="3deac-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="3deac-234">W obiekcie jednego klienta `LastName` właściwość może być "Smith"; w innym obiekcie klienta, `LastName` właściwość może być "Kowalski".</span><span class="sxs-lookup"><span data-stu-id="3deac-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="3deac-235">Podobnie, dowolnej poszczególne strony sieci web w lokacji jest `Page` obiekt, który jest wystąpieniem `Page` klasy.</span><span class="sxs-lookup"><span data-stu-id="3deac-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="3deac-236">Przycisk na stronie jest `Button` obiekt, który jest wystąpieniem `Button` klasy i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="3deac-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="3deac-237">Każde wystąpienie ma własne charakterystyki, ale wszystkie one są oparte na został określony w definicji klasy obiektu.</span><span class="sxs-lookup"><span data-stu-id="3deac-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="3deac-238">Podstawowa składnia</span><span class="sxs-lookup"><span data-stu-id="3deac-238">Basic Syntax</span></span>

<span data-ttu-id="3deac-239">Wcześniej, jak działa przykład podstawowy sposób tworzenia stron ASP.NET Web Pages i jak możesz dodać kod serwera do kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="3deac-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="3deac-240">Tutaj dowiesz się podstaw pisania kodu serwera ASP.NET używająca składni Razor &#8212; czyli programowania reguły języka.</span><span class="sxs-lookup"><span data-stu-id="3deac-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="3deac-241">Jeśli masz doświadczenie w pracy z programowania (zwłaszcza, jeśli używano C, C++, C#, Visual Basic lub JavaScript), większość tutaj przeczytaj będą niczym nowym.</span><span class="sxs-lookup"><span data-stu-id="3deac-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="3deac-242">Prawdopodobnie należy zapoznać się tylko z jak kod serwera jest dodawany do znaczników w *.cshtml* plików.</span><span class="sxs-lookup"><span data-stu-id="3deac-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="3deac-243">Łączenie tekstu, znaczników i kodu w blokach kodu</span><span class="sxs-lookup"><span data-stu-id="3deac-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="3deac-244">W blokach kodu serwera często zachodzi potrzeba danych wyjściowych tekst lub znaczników (lub obie) do strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="3deac-245">Jeśli blok kodu serwera zawiera tekst nie jest kodem, który zamiast tego powinien być renderowany jako jest, ASP.NET musi być w stanie odróżnić go od kodu.</span><span class="sxs-lookup"><span data-stu-id="3deac-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="3deac-246">Istnieje kilka sposobów, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="3deac-246">There are several ways to do this.</span></span>

- <span data-ttu-id="3deac-247">Należy wpisać tekst w elemencie HTML, takich jak `<p></p>` lub `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="3deac-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="3deac-248">HTML element może zawierać tekstu, dodatkowe elementy HTML i wyrażenia w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="3deac-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="3deac-249">Gdy program ASP.NET widzi otwierający HTML tag (na przykład `<p>`), powoduje wyświetlenie, wszystko w tym elemencie, a jej zawartość jako do przeglądarki, rozpoznawanie kodu serwera wyrażenia, ponieważ przechodzi ona.</span><span class="sxs-lookup"><span data-stu-id="3deac-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="3deac-250">Użyj `@:` operatora lub `<text>` elementu.</span><span class="sxs-lookup"><span data-stu-id="3deac-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="3deac-251">`@:` Generuje pojedynczy wiersz zawartość zawierająca zwykły tekst lub niedopasowane tagów HTML; `<text>` element zawiera wiele wierszy w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="3deac-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="3deac-252">Te opcje są przydatne, jeśli nie chcesz renderować HTML element jako część danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="3deac-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="3deac-253">W danych wyjściowych wiele wierszy tekstu lub niedopasowane tagi HTML, można poprzedzić każdy wiersz z `@:`, lub może być częścią wiersza w `<text>` elementu.</span><span class="sxs-lookup"><span data-stu-id="3deac-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="3deac-254">Podobnie jak `@:` operatora`<text>` znaczniki są używane przez program ASP.NET do identyfikowania zawartości tekstowej i nigdy nie są renderowane w danych wyjściowych strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="3deac-255">Pierwszy przykład jest powtarzany w poprzednim przykładzie, ale używa jedna para `<text>` tagów, aby ująć tekstu do renderowania.</span><span class="sxs-lookup"><span data-stu-id="3deac-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="3deac-256">W drugim przykładzie `<text>` i `</text>` tagi należy ująć w trzy wiersze, które mają niektóre niedopasowane tagi HTML i uncontained tekst (`<br />`), wraz z kodu serwera i dopasowane tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="3deac-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="3deac-257">Ponownie, również może poprzedzać każdy wiersz za pomocą `@:` operator; albo sposób działania.</span><span class="sxs-lookup"><span data-stu-id="3deac-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3deac-258">Gdy tekstu wyjściowego przedstawione w tej sekcji &#8212; przy użyciu elementu HTML `@:` operatora lub `<text>` elementu &#8212; ASP.NET nie kodowanie HTML dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="3deac-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="3deac-259">(Jak wspomniano wcześniej, ASP.NET, kodowania danych wyjściowych wyrażeń kodu serwera i serwera bloków kodu, które są poprzedzone `@`, z wyjątkiem specjalnych przypadków wymienionych w tej sekcji.)</span><span class="sxs-lookup"><span data-stu-id="3deac-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="3deac-260">Białe znaki</span><span class="sxs-lookup"><span data-stu-id="3deac-260">Whitespace</span></span>

<span data-ttu-id="3deac-261">Dodatkowe spacje w instrukcji (i poza literału ciągu), nie mają wpływu na instrukcji:</span><span class="sxs-lookup"><span data-stu-id="3deac-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="3deac-262">Podział wiersza w instrukcji nie ma wpływu na instrukcji i może zawijać się instrukcje, aby zwiększyć czytelność.</span><span class="sxs-lookup"><span data-stu-id="3deac-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="3deac-263">Poniższe instrukcje są takie same:</span><span class="sxs-lookup"><span data-stu-id="3deac-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="3deac-264">Jednak nie można opakować linię trakcie literału ciągu.</span><span class="sxs-lookup"><span data-stu-id="3deac-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="3deac-265">Poniższy przykład nie działa:</span><span class="sxs-lookup"><span data-stu-id="3deac-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="3deac-266">Aby połączyć długi ciąg, który jest zawijany do wielu linii, takich jak kod powyżej, dostępne są dwie opcje.</span><span class="sxs-lookup"><span data-stu-id="3deac-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="3deac-267">Możesz użyć operatora łączenia (`+`), które zostaną wyświetlone w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="3deac-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="3deac-268">Można również użyć `@` znaku, aby utworzyć ciąg verbatim literału, jak przedstawiono wcześniej w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="3deac-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="3deac-269">Wielu liniach, można podzielić ciąg verbatim literałów:</span><span class="sxs-lookup"><span data-stu-id="3deac-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="3deac-270">Kod (i znaczników) komentarze</span><span class="sxs-lookup"><span data-stu-id="3deac-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="3deac-271">Komentarze umożliwiają pozostawienie notatki dla siebie i innych osób.</span><span class="sxs-lookup"><span data-stu-id="3deac-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="3deac-272">Umożliwiają także wyłączyć (*komentarz*) części kodu lub języka znaczników, nie chcesz uruchamiać przez użytkownika, które mają być zachowane w na stronie przez pewien czas.</span><span class="sxs-lookup"><span data-stu-id="3deac-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="3deac-273">Istnieje inny, komentowanie składnię Razor kod oraz kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="3deac-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="3deac-274">Podobnie jak w przypadku wszystkich kodu Razor, komentarz Razor są przetwarzane (i następnie usuwane) na serwerze przed wysłaniem strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3deac-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="3deac-275">W związku z tym ze składni Razor komentowania umożliwia umieść komentarze w kodzie (lub nawet do znaczników), widoczne podczas edycji pliku, ale użytkownicy nie widzą, nawet w przypadku źródła strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="3deac-276">Komentarze ASP.NET Razor start komentarz z `@*` i Zakończ ją za pomocą `*@`.</span><span class="sxs-lookup"><span data-stu-id="3deac-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="3deac-277">Komentarz może być w jednym lub wielu wierszy:</span><span class="sxs-lookup"><span data-stu-id="3deac-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="3deac-278">Poniżej przedstawiono komentarz w bloku kodu:</span><span class="sxs-lookup"><span data-stu-id="3deac-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="3deac-279">W tym miejscu jest tego samego bloku kodu, w wierszu kodu oznaczone jako komentarz, aby nie mogą być uruchamiane:</span><span class="sxs-lookup"><span data-stu-id="3deac-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="3deac-280">Wewnątrz bloku kodu zamiast przy użyciu składni komentarza Razor, można użyć komentowania składni języka programowania, którego używasz, takich jak C#:</span><span class="sxs-lookup"><span data-stu-id="3deac-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="3deac-281">W języku C# są poprzedzone Komentarze jednowierszowe `//` znaków i wielowierszowe komentarze zaczynają się od `/*` i kończyć się znakiem `*/`.</span><span class="sxs-lookup"><span data-stu-id="3deac-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="3deac-282">(Podobnie jak w przypadku komentarz Razor komentarze języka C# nie są uważane za do przeglądarki.)</span><span class="sxs-lookup"><span data-stu-id="3deac-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="3deac-283">W przypadku znaczników prawdopodobnie wiesz, można utworzyć komentarz HTML:</span><span class="sxs-lookup"><span data-stu-id="3deac-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="3deac-284">Komentarze HTML rozpoczynać `<!--` znaków oraz kończyć się `-->`.</span><span class="sxs-lookup"><span data-stu-id="3deac-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="3deac-285">Można użyć komentarze HTML otoczyć nie tylko tekst, ale również wszelkich znacznik HTML, który może chcesz zachować na stronie, ale nie ma być renderowany.</span><span class="sxs-lookup"><span data-stu-id="3deac-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="3deac-286">Ten komentarz HTML spowoduje ukrycie całą zawartość tagi i tekst, które zawierają:</span><span class="sxs-lookup"><span data-stu-id="3deac-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="3deac-287">W przeciwieństwie do komentarzy Razor, HTML komentarze *są* renderowania do strony i użytkownik może je wyświetlić, wyświetlając źródła strony.</span><span class="sxs-lookup"><span data-stu-id="3deac-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="3deac-288">Razor ma ograniczenia zagnieżdżone bloki konstrukcyjne C#.</span><span class="sxs-lookup"><span data-stu-id="3deac-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="3deac-289">Aby uzyskać więcej informacji, zobacz [o nazwie zmienne języka C# i zagnieżdżone bloki Generuj uszkodzony kod](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="3deac-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="3deac-290">Zmienne</span><span class="sxs-lookup"><span data-stu-id="3deac-290">Variables</span></span>

<span data-ttu-id="3deac-291">Zmienna jest nazwany obiekt, który służy do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="3deac-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="3deac-292">Zmienne możesz nazwać wszystko, ale nazwa musi rozpoczynać się od litery alfabetu i nie może zawierać spacji ani znaków zarezerwowanych.</span><span class="sxs-lookup"><span data-stu-id="3deac-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="3deac-293">Zmienne i typy danych</span><span class="sxs-lookup"><span data-stu-id="3deac-293">Variables and Data Types</span></span>

<span data-ttu-id="3deac-294">Zmienna może mieć na określony typ danych, która wskazuje, jakiego typu dane są przechowywane w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="3deac-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="3deac-295">Mogą mieć zmiennych ciągu, które przechowują wartości ciągu (takich jak &quot;Witaj, świecie&quot;), zmiennych całkowitych, które przechowują wartości liczby całkowitej (np. 3 lub 79) i zmienne daty, które przechowywanie wartości daty w różnych formatach (np. 4/12/2012 lub marca 2009 ).</span><span class="sxs-lookup"><span data-stu-id="3deac-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="3deac-296">Wiąże się wiele typów danych, w których można użyć.</span><span class="sxs-lookup"><span data-stu-id="3deac-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="3deac-297">Jednak zazwyczaj nie trzeba określać typu zmiennej.</span><span class="sxs-lookup"><span data-stu-id="3deac-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="3deac-298">W większości przypadków, platformy ASP.NET można ustalić typu, w oparciu sposobu korzystania z danych w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="3deac-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="3deac-299">(Czasami musisz określić typ; pojawi się przykłady którym ta zasada obowiązuje).</span><span class="sxs-lookup"><span data-stu-id="3deac-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="3deac-300">Możesz deklarować zmienną za pomocą `var` — słowo kluczowe (Jeśli nie chcesz określić typ) lub za pomocą nazwy typu:</span><span class="sxs-lookup"><span data-stu-id="3deac-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="3deac-301">Poniższy przykład przedstawia niektóre typowe zastosowania zmiennych na stronie sieci web:</span><span class="sxs-lookup"><span data-stu-id="3deac-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="3deac-302">Jeśli łączone poprzednich przykładach, na stronie widzisz taki komunikat wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="3deac-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="3deac-304">Konwertowanie i testowanie typów danych</span><span class="sxs-lookup"><span data-stu-id="3deac-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="3deac-305">Mimo że program ASP.NET zazwyczaj może automatycznie określić typu danych, czasami nie jest.</span><span class="sxs-lookup"><span data-stu-id="3deac-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="3deac-306">W związku z tym konieczne może być pomógł ASP.NET, wykonując jawnej konwersji.</span><span class="sxs-lookup"><span data-stu-id="3deac-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="3deac-307">Nawet jeśli nie masz konwersji typów, czasami warto sprawdzić, jakiego typu dane być może pracujesz z.</span><span class="sxs-lookup"><span data-stu-id="3deac-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="3deac-308">Najbardziej często zdarza się, trzeba przekonwertować ciąg do innego typu, takie jak liczbą całkowitą lub daty.</span><span class="sxs-lookup"><span data-stu-id="3deac-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="3deac-309">Poniższy przykład przedstawia typową sytuacją, w którym należy przekonwertować ciąg na liczbę.</span><span class="sxs-lookup"><span data-stu-id="3deac-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="3deac-310">Zgodnie z zasadą dane wejściowe użytkownika dostarczany jako ciągi.</span><span class="sxs-lookup"><span data-stu-id="3deac-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="3deac-311">Nawet jeśli po wyświetleniu monitu użytkowników podania numeru, a nawet wtedy, gdy zostały wprowadzone cyfr, podczas przesyłania danych wejściowych użytkownika, a następnie go odczytać w kodzie, dane są w formacie ciągu.</span><span class="sxs-lookup"><span data-stu-id="3deac-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="3deac-312">W związku z tym należy przekonwertować ciąg na liczbę.</span><span class="sxs-lookup"><span data-stu-id="3deac-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="3deac-313">W przykładzie Jeśli zostanie podjęta próba wykonania operacji arytmetycznych na wartościach, bez konwersji, następujący błąd wyników, ponieważ program ASP.NET nie można dodać dwa ciągi:</span><span class="sxs-lookup"><span data-stu-id="3deac-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

*<span data-ttu-id="3deac-314">Nie można niejawnie przekonwertować typu "string" do "int".</span><span class="sxs-lookup"><span data-stu-id="3deac-314">Cannot implicitly convert type 'string' to 'int'.</span></span>*

<span data-ttu-id="3deac-315">Aby przekonwertować wartości liczb całkowitych, należy wywołać `AsInt` metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="3deac-316">Jeśli konwersja się pomyślnie, następnie można dodać liczb.</span><span class="sxs-lookup"><span data-stu-id="3deac-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="3deac-317">Poniższa lista zawiera niektóre typowe metody konwersji i testowania dla zmiennych.</span><span class="sxs-lookup"><span data-stu-id="3deac-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <strong><span data-ttu-id="3deac-318">Metoda</span><span class="sxs-lookup"><span data-stu-id="3deac-318">Method</span></span></strong>
    :::column-end:::
    :::column:::
    <strong><span data-ttu-id="3deac-319">Opis</span><span class="sxs-lookup"><span data-stu-id="3deac-319">Description</span></span></strong>
    :::column-end:::
    :::column:::
    <strong><span data-ttu-id="3deac-320">Przykład</span><span class="sxs-lookup"><span data-stu-id="3deac-320">Example</span></span></strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="3deac-321">Konwertuje ciąg, który reprezentuje liczbę całkowitą z zakresu (np. "593") na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="3deac-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
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
    <span data-ttu-id="3deac-322">Konwertuje ciąg, takich jak &quot;true&quot; lub &quot;false&quot; na typ Boolean.</span><span class="sxs-lookup"><span data-stu-id="3deac-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
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
    <span data-ttu-id="3deac-323">Konwertuje ciąg, który ma wartość dziesiętną, takich jak &quot;1.3&quot; lub &quot;7.439&quot; na liczbę zmiennoprzecinkową.</span><span class="sxs-lookup"><span data-stu-id="3deac-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
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
    <span data-ttu-id="3deac-324">Konwertuje ciąg, który ma wartość dziesiętną, takich jak &quot;1.3&quot; lub &quot;7.439&quot; na liczbę dziesiętną.</span><span class="sxs-lookup"><span data-stu-id="3deac-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="3deac-325">(W programie ASP.NET: liczba dziesiętna jest bardziej precyzyjne niż liczba zmiennoprzecinkowa).</span><span class="sxs-lookup"><span data-stu-id="3deac-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
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
    <span data-ttu-id="3deac-326">Konwertuje ciąg reprezentujący wartość daty i godziny ASP.NET `DateTime` typu.</span><span class="sxs-lookup"><span data-stu-id="3deac-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
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
    <span data-ttu-id="3deac-327">Konwertuje ciąg inny typ danych.</span><span class="sxs-lookup"><span data-stu-id="3deac-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="3deac-328">Operatory</span><span class="sxs-lookup"><span data-stu-id="3deac-328">Operators</span></span>

<span data-ttu-id="3deac-329">Operator jest słowo kluczowe lub znak, który informuje ASP.NET, jakiego rodzaju polecenie do wykonania w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="3deac-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="3deac-330">W języku C# (i składni Razor, który opiera się na nim) obsługuje wiele operatorów, ale musisz rozpoznać kilka, aby rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="3deac-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="3deac-331">Poniższa tabela zawiera podsumowanie najbardziej typowych operatorów.</span><span class="sxs-lookup"><span data-stu-id="3deac-331">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
    <strong><span data-ttu-id="3deac-332">Operator</span><span class="sxs-lookup"><span data-stu-id="3deac-332">Operator</span></span></strong>
    :::column-end:::
    :::column:::
    <strong><span data-ttu-id="3deac-333">Opis</span><span class="sxs-lookup"><span data-stu-id="3deac-333">Description</span></span></strong>
    :::column-end:::
    :::column:::
    <strong><span data-ttu-id="3deac-334">Przykłady</span><span class="sxs-lookup"><span data-stu-id="3deac-334">Examples</span></span></strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    <span data-ttu-id="3deac-335">Operatory matematyczne używać w wyrażeniach liczbowych.</span><span class="sxs-lookup"><span data-stu-id="3deac-335">Math operators used in numerical expressions.</span></span>
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
    <span data-ttu-id="3deac-336">Przypisanie.</span><span class="sxs-lookup"><span data-stu-id="3deac-336">Assignment.</span></span> <span data-ttu-id="3deac-337">Przypisuje wartość po prawej stronie instrukcji do obiektu po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="3deac-337">Assigns the value on the right side of a statement to the object on the left side.</span></span>
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
    <span data-ttu-id="3deac-338">Równości.</span><span class="sxs-lookup"><span data-stu-id="3deac-338">Equality.</span></span> <span data-ttu-id="3deac-339">Zwraca `true` Jeśli wartości są równe.</span><span class="sxs-lookup"><span data-stu-id="3deac-339">Returns `true` if the values are equal.</span></span> <span data-ttu-id="3deac-340">(Należy zauważyć różnicę między `=` operatora i `==` operator.)</span><span class="sxs-lookup"><span data-stu-id="3deac-340">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
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
    <span data-ttu-id="3deac-341">Nierówności.</span><span class="sxs-lookup"><span data-stu-id="3deac-341">Inequality.</span></span> <span data-ttu-id="3deac-342">Zwraca `true` wartości nie są równe.</span><span class="sxs-lookup"><span data-stu-id="3deac-342">Returns `true` if the values are not equal.</span></span>
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
    <span data-ttu-id="3deac-343">Mniej-niż większą-niż, mniejsze niż lub równości i większa niż — lub równości.</span><span class="sxs-lookup"><span data-stu-id="3deac-343">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
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
    <span data-ttu-id="3deac-344">Łączenie, który jest używany do łączenia ciągów.</span><span class="sxs-lookup"><span data-stu-id="3deac-344">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="3deac-345">ASP.NET wie, że różnica między Ten operator i operator dodawania, na podstawie typu danych wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="3deac-345">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="3deac-346">Operatory inkrementacji i dekrementacji, które dodawania i odejmowania 1 (odpowiednio) ze zmiennej.</span><span class="sxs-lookup"><span data-stu-id="3deac-346">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
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
    <span data-ttu-id="3deac-347">Kropka.</span><span class="sxs-lookup"><span data-stu-id="3deac-347">Dot.</span></span> <span data-ttu-id="3deac-348">Używany do odróżnienia obiektów i ich właściwości i metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-348">Used to distinguish objects and their properties and methods.</span></span>
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
    <span data-ttu-id="3deac-349">Nawiasy.</span><span class="sxs-lookup"><span data-stu-id="3deac-349">Parentheses.</span></span> <span data-ttu-id="3deac-350">Używane do wyrażenia grupy oraz w celu przekazania parametrów do metod.</span><span class="sxs-lookup"><span data-stu-id="3deac-350">Used to group expressions and to pass parameters to methods.</span></span>
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
    <span data-ttu-id="3deac-351">Nawiasy kwadratowe.</span><span class="sxs-lookup"><span data-stu-id="3deac-351">Brackets.</span></span> <span data-ttu-id="3deac-352">Używane do uzyskiwania dostępu do wartości w tablice i kolekcje.</span><span class="sxs-lookup"><span data-stu-id="3deac-352">Used for accessing values in arrays or collections.</span></span>
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
    <span data-ttu-id="3deac-353">Nie.</span><span class="sxs-lookup"><span data-stu-id="3deac-353">Not.</span></span> <span data-ttu-id="3deac-354">Odwraca `true` wartość `false` i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="3deac-354">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="3deac-355">Zwykle jest używana jako sposób skrót do testowania `false` (oznacza to, aby nie `true`).</span><span class="sxs-lookup"><span data-stu-id="3deac-355">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&&` `||`
    :::column-end:::
    :::column:::
    <span data-ttu-id="3deac-356">Operator logiczny oraz i lub które są używane do łączenia ze sobą warunki.</span><span class="sxs-lookup"><span data-stu-id="3deac-356">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="3deac-357">Praca z pliku i ścieżki folderu, w kodzie</span><span class="sxs-lookup"><span data-stu-id="3deac-357">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="3deac-358">Będziesz często pracować ze ścieżkami plików i folderów w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3deac-358">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="3deac-359">Oto przykład strukturę folderu fizycznego dla witryny sieci Web mogą być wyświetlane na komputerze deweloperskim:</span><span class="sxs-lookup"><span data-stu-id="3deac-359">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="3deac-360">Poniżej przedstawiono niektóre podstawowe szczegóły dotyczące adresów URL i ścieżki:</span><span class="sxs-lookup"><span data-stu-id="3deac-360">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="3deac-361">Adres URL zaczyna się od jedną nazwę domeny (`http://www.example.com`) lub nazwy serwera (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="3deac-361">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="3deac-362">Adres URL odnosi się do ścieżki fizycznej na komputerze-hoście.</span><span class="sxs-lookup"><span data-stu-id="3deac-362">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="3deac-363">Na przykład `http://myserver` może odpowiadać folder *C:\websites\mywebsite* na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3deac-363">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="3deac-364">Ścieżka wirtualna jest skrótem do reprezentowania ścieżek w kodzie bez konieczności podać pełną ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="3deac-364">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="3deac-365">Obejmuje on część adresu URL, który następuje nazwa domeny lub serwera.</span><span class="sxs-lookup"><span data-stu-id="3deac-365">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="3deac-366">Gdy używasz ścieżki wirtualne, kod można przenieść do innej domeny lub serwer, bez konieczności aktualizowania ścieżki.</span><span class="sxs-lookup"><span data-stu-id="3deac-366">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="3deac-367">Oto przykład, aby lepiej zrozumieć różnice:</span><span class="sxs-lookup"><span data-stu-id="3deac-367">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="3deac-368">Pełny adres URL</span><span class="sxs-lookup"><span data-stu-id="3deac-368">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="3deac-369">Nazwa serwera</span><span class="sxs-lookup"><span data-stu-id="3deac-369">Server name</span></span> | *<span data-ttu-id="3deac-370">mycompanyserver</span><span class="sxs-lookup"><span data-stu-id="3deac-370">mycompanyserver</span></span>* |
| <span data-ttu-id="3deac-371">Ścieżka wirtualna</span><span class="sxs-lookup"><span data-stu-id="3deac-371">Virtual path</span></span> | *<span data-ttu-id="3deac-372">/HumanResources/CompanyPolicy.htm</span><span class="sxs-lookup"><span data-stu-id="3deac-372">/humanresources/CompanyPolicy.htm</span></span>* |
| <span data-ttu-id="3deac-373">Ścieżka fizyczna</span><span class="sxs-lookup"><span data-stu-id="3deac-373">Physical path</span></span> | *<span data-ttu-id="3deac-374">C:\mywebsites\humanresources\CompanyPolicy.htm</span><span class="sxs-lookup"><span data-stu-id="3deac-374">C:\mywebsites\humanresources\CompanyPolicy.htm</span></span>* |

<span data-ttu-id="3deac-375">Wirtualny katalog główny jest /, podobnie jak głównego C: dysku \.</span><span class="sxs-lookup"><span data-stu-id="3deac-375">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="3deac-376">(Folder wirtualny ścieżki zawsze używać ukośników.) Ścieżka wirtualna folderu nie musi mieć taką samą nazwę jak folder fizycznych; może być aliasem.</span><span class="sxs-lookup"><span data-stu-id="3deac-376">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="3deac-377">(Na serwerach produkcyjnych, ścieżka wirtualna rzadko odpowiada dokładnej ścieżki fizycznej.)</span><span class="sxs-lookup"><span data-stu-id="3deac-377">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="3deac-378">Podczas pracy z plikami i folderami w kodzie, czasami musisz odwoływać się do ścieżki fizycznej, a czasami ścieżką wirtualną, w zależności od tego, jakie obiekty pracujemy z użyciem.</span><span class="sxs-lookup"><span data-stu-id="3deac-378">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="3deac-379">ASP.NET udostępnia te narzędzia do pracy ze ścieżkami plików i folderów w kodzie: `Server.MapPath` metody i `~` operatora i `Href` metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-379">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="3deac-380">Konwertowanie ścieżki wirtualnej do fizycznego: Metoda Server.MapPath</span><span class="sxs-lookup"><span data-stu-id="3deac-380">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="3deac-381">`Server.MapPath` Metoda konwertuje ścieżki wirtualnej (np. */default.cshtml*) do ścieżki bezwzględnej fizycznych (takich jak *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="3deac-381">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="3deac-382">Możesz użyć tej metody w każdej chwili należy pełną ścieżkę fizyczną.</span><span class="sxs-lookup"><span data-stu-id="3deac-382">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="3deac-383">Typowym przykładem jest podczas odczytywania lub zapisywania pliku tekstowego lub pliku obrazu na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="3deac-383">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="3deac-384">Zazwyczaj nie wiesz, bezwzględną ścieżkę fizyczną witryny na serwerze lokacji hostingu, więc tej metody można przekonwertować ścieżkę wiadomo — ścieżka wirtualna — do odpowiedniej ścieżki na serwerze dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="3deac-384">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="3deac-385">Ścieżka wirtualna jest przekazywane do pliku lub folderu, do metody i zwraca ścieżkę fizyczną:</span><span class="sxs-lookup"><span data-stu-id="3deac-385">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="3deac-386">Odwoływanie się do wirtualnego katalogu głównego: ~ operatora i Href, metoda</span><span class="sxs-lookup"><span data-stu-id="3deac-386">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="3deac-387">W *.cshtml* lub *.vbhtml* pliku, możesz odwoływać się za pomocą ścieżka wirtualnego katalogu głównego `~` operatora.</span><span class="sxs-lookup"><span data-stu-id="3deac-387">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="3deac-388">Może to być bardzo przydatne, ponieważ możesz poruszać się strony w witrynie, a wszystkie łącza, które zawierają do innych stron nie będzie działać.</span><span class="sxs-lookup"><span data-stu-id="3deac-388">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="3deac-389">Jest to również przydatne w przypadku, gdy przeniesiesz kiedykolwiek witryny sieci Web w innej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="3deac-389">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="3deac-390">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="3deac-390">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="3deac-391">Jeśli witryna sieci Web `http://myserver/myapp`, Oto jak ASP.NET będzie traktować te ścieżki, po uruchomieniu na stronie:</span><span class="sxs-lookup"><span data-stu-id="3deac-391">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- `myImagesFolder`<span data-ttu-id="3deac-392">:</span><span class="sxs-lookup"><span data-stu-id="3deac-392">:</span></span> `http://myserver/myapp/images`
- `myStyleSheet` <span data-ttu-id="3deac-393">:</span><span class="sxs-lookup"><span data-stu-id="3deac-393">:</span></span> `http://myserver/myapp/styles/Stylesheet.css`

<span data-ttu-id="3deac-394">(Te ścieżki faktycznie nie będzie widoczna jako wartości zmiennej, ale ASP.NET traktują ścieżki, tak, jakby to, jakie były).</span><span class="sxs-lookup"><span data-stu-id="3deac-394">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="3deac-395">Możesz użyć `~` operator zarówno w kodzie serwera (jak powyżej), jak i znaczników w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3deac-395">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="3deac-396">W znaczniku, użyj `~` operatora do utworzenia ścieżki do zasobów, takich jak pliki obrazów, innych stron sieci web i plików CSS.</span><span class="sxs-lookup"><span data-stu-id="3deac-396">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="3deac-397">Po uruchomieniu strony ASP.NET przegląda strony (kodu i znaczników) i usuwa wszystkie `~` odwołania do danej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="3deac-397">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="3deac-398">Logika warunkowe i pętle</span><span class="sxs-lookup"><span data-stu-id="3deac-398">Conditional Logic and Loops</span></span>

<span data-ttu-id="3deac-399">Kod serwera ASP.NET umożliwia wykonywanie zadań na podstawie warunków i napisać kod, który powtarza instrukcji określoną liczbę razy (czyli kodu wykonywanego w pętli).</span><span class="sxs-lookup"><span data-stu-id="3deac-399">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="3deac-400">Warunki badania</span><span class="sxs-lookup"><span data-stu-id="3deac-400">Testing Conditions</span></span>

<span data-ttu-id="3deac-401">Aby przetestować warunku prostego, możesz użyć `if` instrukcję, która zwraca wartość PRAWDA lub FAŁSZ, oparte na test, należy określić:</span><span class="sxs-lookup"><span data-stu-id="3deac-401">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="3deac-402">`if` — Słowo kluczowe uruchamia blok.</span><span class="sxs-lookup"><span data-stu-id="3deac-402">The `if` keyword starts a block.</span></span> <span data-ttu-id="3deac-403">Test rzeczywisty (stan) w nawiasach i zwraca wartość PRAWDA lub FAŁSZ.</span><span class="sxs-lookup"><span data-stu-id="3deac-403">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="3deac-404">Instrukcje, które uruchomienia, jeśli test jest wartość true, są ujęte w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="3deac-404">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="3deac-405">`if` Może zawierać instrukcji `else` blok, który określa instrukcje do uruchomienia, jeśli warunek nie jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="3deac-405">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="3deac-406">Można dodać wiele warunków przy użyciu `else if` bloku:</span><span class="sxs-lookup"><span data-stu-id="3deac-406">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="3deac-407">W tym przykładzie, jeśli pierwszy warunek w instrukcji if bloku nie ma wartość true, `else if` warunek jest sprawdzany.</span><span class="sxs-lookup"><span data-stu-id="3deac-407">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="3deac-408">Jeśli ten warunek jest spełniony, instrukcje w `else if` bloku są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="3deac-408">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="3deac-409">Jeśli żaden z warunków nie jest spełniony, instrukcje w `else` bloku są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="3deac-409">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="3deac-410">Można dodać dowolną liczbę else if blokuje, a następnie Zamknij z `else` zablokować, jako &quot;cała reszta&quot; warunku.</span><span class="sxs-lookup"><span data-stu-id="3deac-410">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="3deac-411">Aby przetestować dużą liczbę warunków, należy użyć `switch` bloku:</span><span class="sxs-lookup"><span data-stu-id="3deac-411">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="3deac-412">Wartość do sprawdzenia jest w nawiasach (w tym przykładzie `weekday` zmiennej).</span><span class="sxs-lookup"><span data-stu-id="3deac-412">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="3deac-413">Każdy pojedynczy test używa `case` instrukcję, która kończy się wraz z dwukropkiem (:).</span><span class="sxs-lookup"><span data-stu-id="3deac-413">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="3deac-414">Jeśli wartość `case` instrukcji pasuje do wartości testu, wykonywany jest kod, w tym przypadku bloku.</span><span class="sxs-lookup"><span data-stu-id="3deac-414">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="3deac-415">Zamknij każdej instrukcji case z `break` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="3deac-415">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="3deac-416">(Jeśli zapomnisz o wstawieniu podziału w każdym `case` bloku kodu z następnej `case` instrukcji zostaną również uruchomione.) A `switch` blok często ma `default` instrukcję jako ostatni przypadku &quot;cała reszta&quot; opcja, która jest uruchamiana, gdy żaden z pozostałych przypadkach jest spełniony.</span><span class="sxs-lookup"><span data-stu-id="3deac-416">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="3deac-417">Wynik ostatnich dwóch bloków warunkowych, wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="3deac-417">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="3deac-419">Tworzenie pętli kodu</span><span class="sxs-lookup"><span data-stu-id="3deac-419">Looping Code</span></span>

<span data-ttu-id="3deac-420">Często muszą uruchomić te same instrukcje wielokrotnie.</span><span class="sxs-lookup"><span data-stu-id="3deac-420">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="3deac-421">W tym pętli są uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="3deac-421">You do this by looping.</span></span> <span data-ttu-id="3deac-422">Na przykład można często uruchamiane te same instrukcje dla każdego elementu w kolekcji danych.</span><span class="sxs-lookup"><span data-stu-id="3deac-422">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="3deac-423">Jeśli wiesz, dokładnie tak jak często chcesz pętli, możesz użyć `for` pętli.</span><span class="sxs-lookup"><span data-stu-id="3deac-423">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="3deac-424">Ten rodzaj pętli jest szczególnie przydatna podczas liczenia w lub inwentaryzacji:</span><span class="sxs-lookup"><span data-stu-id="3deac-424">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="3deac-425">Pętla zaczyna się od `for` — słowo kluczowe następują trzy instrukcje w nawiasach, każdy kończyć się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="3deac-425">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="3deac-426">Wewnątrz nawiasów w pierwszej instrukcji (`var i=10;`) tworzy licznika i inicjuje ją do 10.</span><span class="sxs-lookup"><span data-stu-id="3deac-426">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="3deac-427">Nie masz nazwy licznika `i` &#8212; można użyć dowolnej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="3deac-427">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="3deac-428">Gdy `for` kiedy pętla zostaje uruchomiona, licznik jest automatycznie zwiększany.</span><span class="sxs-lookup"><span data-stu-id="3deac-428">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="3deac-429">Druga instrukcja (`i < 21;`) określa warunek, o ile liczba.</span><span class="sxs-lookup"><span data-stu-id="3deac-429">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="3deac-430">W tym przypadku chcesz przejść do maksymalnie 20 (czyli kontynuować pracę podczas gdy ten licznik jest mniej niż 21).</span><span class="sxs-lookup"><span data-stu-id="3deac-430">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="3deac-431">Trzecia instrukcja (`i++` ) używa operator inkrementacji, który po prostu Określa, że licznik powinny mieć 1 dodawanych do niego przy każdym uruchomieniu pętli.</span><span class="sxs-lookup"><span data-stu-id="3deac-431">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="3deac-432">Wewnątrz nawiasów klamrowych jest kod, który będzie wykonywany dla każdej iteracji pętli.</span><span class="sxs-lookup"><span data-stu-id="3deac-432">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="3deac-433">Znaczniki, tworzy nowy akapit (`<p>` elementu) każdego czas i dodaje wiersz danych wyjściowych, wyświetlanie wartości `i` (licznik).</span><span class="sxs-lookup"><span data-stu-id="3deac-433">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="3deac-434">Po uruchomieniu tej strony, ten przykład tworzy 11 wiersza, wyświetlanie danych wyjściowych z tekstem w każdym wierszu, wskazującą liczbę elementów.</span><span class="sxs-lookup"><span data-stu-id="3deac-434">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="3deac-436">Jeśli pracujesz z kolekcji lub tablicy, są często używane `foreach` pętli.</span><span class="sxs-lookup"><span data-stu-id="3deac-436">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="3deac-437">Kolekcja to grupa podobnych obiektów i `foreach` pętli pozwala przeprowadzić zadanie dla każdego elementu w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3deac-437">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="3deac-438">Ten typ pętli jest wygodne w przypadku kolekcji, ponieważ w odróżnieniu od `for` pętli, nie trzeba ten licznik lub Ustaw limit.</span><span class="sxs-lookup"><span data-stu-id="3deac-438">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="3deac-439">Zamiast tego `foreach` kodu pętli po prostu obejmującego kolekcji dopiero po jej zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="3deac-439">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="3deac-440">Na przykład, poniższy kod zwraca elementy w `Request.ServerVariables` kolekcji, która jest obiektem, który zawiera informacje o serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="3deac-440">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="3deac-441">Używa ona `foreac` h pętli, aby wyświetlić nazwę każdego elementu przez utworzenie nowego `<li>` element na liście punktowanej we wcześniejszej HTML.</span><span class="sxs-lookup"><span data-stu-id="3deac-441">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="3deac-442">`foreach` — Słowo kluczowe, następuje nawiasy, o których należy zadeklarować zmienną, która reprezentuje pojedynczy element w kolekcji (w tym przykładzie `var item`), a następnie `in` — słowo kluczowe, następuje kolekcję ma pętli.</span><span class="sxs-lookup"><span data-stu-id="3deac-442">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="3deac-443">W treści `foreach` pętli, możesz uzyskać dostęp bieżącego elementu przy użyciu zmiennej zadeklarowanej wcześniej.</span><span class="sxs-lookup"><span data-stu-id="3deac-443">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="3deac-445">Aby utworzyć bardziej ogólnego przeznaczenia pętli, użyj `while` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="3deac-445">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="3deac-446">A `while` pętli zaczyna się od `while` — słowo kluczowe, następuje nawiasów której określić, jak długo będzie nadal pętli (tutaj, aby uzyskać tak długo, jak `countNum` jest mniejsza niż 50), następnie bloku powtórzeń.</span><span class="sxs-lookup"><span data-stu-id="3deac-446">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="3deac-447">Zazwyczaj Zwiększ pętli (dodać) lub dekrementacji (odjęcia od) zmiennej lub obiektu użytego do zliczania.</span><span class="sxs-lookup"><span data-stu-id="3deac-447">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="3deac-448">W tym przykładzie `+=` operator dodaje 1- `countNum` przy każdym uruchomieniu pętli.</span><span class="sxs-lookup"><span data-stu-id="3deac-448">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="3deac-449">(W celu dekrementacja zmiennej w pętlę, który odlicza, należy użyć operatora dekrementacyjnego `-=`).</span><span class="sxs-lookup"><span data-stu-id="3deac-449">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="3deac-450">Obiekty i kolekcje</span><span class="sxs-lookup"><span data-stu-id="3deac-450">Objects and Collections</span></span>

<span data-ttu-id="3deac-451">Prawie wszystko w witrynie internetowej platformy ASP.NET jest obiektem, w tym strona sieci web.</span><span class="sxs-lookup"><span data-stu-id="3deac-451">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="3deac-452">W tej sekcji omówiono niektóre ważne obiekty, które często będziesz pracować z w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3deac-452">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="3deac-453">Obiekty strony</span><span class="sxs-lookup"><span data-stu-id="3deac-453">Page Objects</span></span>

<span data-ttu-id="3deac-454">Najbardziej podstawowa obiektu w programie ASP.NET: jest to strona.</span><span class="sxs-lookup"><span data-stu-id="3deac-454">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="3deac-455">Możesz uzyskać dostęp właściwości obiektu strony bezpośrednio, bez dowolnych obiektów kwalifikujących.</span><span class="sxs-lookup"><span data-stu-id="3deac-455">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="3deac-456">Poniższy kod pobiera ścieżkę do pliku, przy użyciu `Request` obiekt strony:</span><span class="sxs-lookup"><span data-stu-id="3deac-456">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="3deac-457">Aby stał się jasne, możesz teraz odwołuje się do właściwości i metody dla bieżącego obiektu strony możesz opcjonalnie użyć słowa kluczowego `this` obiekt strony w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3deac-457">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="3deac-458">Poniżej przedstawiono w poprzednim przykładzie kodu za pomocą `this` dodane do reprezentowania strony:</span><span class="sxs-lookup"><span data-stu-id="3deac-458">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="3deac-459">Można użyć właściwości `Page` można uzyskać mnóstwo informacji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="3deac-459">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- `Request`<span data-ttu-id="3deac-460">.</span><span class="sxs-lookup"><span data-stu-id="3deac-460">.</span></span> <span data-ttu-id="3deac-461">Jak już przeczytane, to zbiór informacji o bieżącym żądaniu, w tym, jakiego rodzaju przeglądarki wysłał żądanie, adres URL strony, tożsamość użytkownika, itp.</span><span class="sxs-lookup"><span data-stu-id="3deac-461">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- `Response`<span data-ttu-id="3deac-462">.</span><span class="sxs-lookup"><span data-stu-id="3deac-462">.</span></span> <span data-ttu-id="3deac-463">Jest to zbiór informacji na temat odpowiedzi (strona), który będzie wysyłany do przeglądarki, gdy kod serwera zakończył działanie.</span><span class="sxs-lookup"><span data-stu-id="3deac-463">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="3deac-464">Na przykład można używasz tej właściwości można zapisać informacji do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3deac-464">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="3deac-465">Obiekty kolekcji (tablic i słowników)</span><span class="sxs-lookup"><span data-stu-id="3deac-465">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="3deac-466">A *kolekcji* jest grupą obiektów tego samego typu, na przykład zbiór `Customer` obiektów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3deac-466">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="3deac-467">Program ASP.NET zawiera wiele kolekcji wbudowanych, takie jak `Request.Files` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3deac-467">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="3deac-468">Często będziesz pracować z danymi w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3deac-468">You'll often work with data in collections.</span></span> <span data-ttu-id="3deac-469">Istnieją dwa typy kolekcji typowe *tablicy* i *słownika*.</span><span class="sxs-lookup"><span data-stu-id="3deac-469">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="3deac-470">Tablica jest przydatne, gdy ma być przechowywany w kolekcji podobnych elementów, ale nie chcesz utworzyć osobne zmienną do przechowywania każdego elementu:</span><span class="sxs-lookup"><span data-stu-id="3deac-470">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="3deac-471">W przypadku tablic, możesz zadeklarować na określony typ danych, takich jak `string`, `int`, lub `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="3deac-471">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="3deac-472">Aby wskazać, że zmienna może zawierać tablicę, Dodaj nawiasów do deklaracji (takie jak `string[]` lub `int[]`).</span><span class="sxs-lookup"><span data-stu-id="3deac-472">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="3deac-473">Możesz uzyskać dostęp elementów w tablicy przy użyciu ich pozycji (indeks) lub za pomocą `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="3deac-473">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="3deac-474">Indeksy tablicy są oparte na zerze &#8212; oznacza to, pierwszy element jest na pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="3deac-474">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="3deac-475">Można określić liczbę elementów w tablicy, uzyskując jego `Length` właściwości.</span><span class="sxs-lookup"><span data-stu-id="3deac-475">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="3deac-476">Aby uzyskać położenie konkretny element w tablicy (na przykład aby wyszukać), użyj `Array.IndexOf` metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-476">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="3deac-477">Można również wykonać elementów, takich jak wstecznego zawartość tablicy ( `Array.Reverse` metoda) i sortować zawartość ( `Array.Sort` metody).</span><span class="sxs-lookup"><span data-stu-id="3deac-477">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="3deac-478">Dane wyjściowe kodu tablicy ciąg wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="3deac-478">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="3deac-480">Słownik to kolekcja par klucz wartość, określane klucz (lub nazwę) można ustawić lub pobrać odpowiednią wartość:</span><span class="sxs-lookup"><span data-stu-id="3deac-480">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="3deac-481">Aby utworzyć słownik, należy użyć `new` — słowo kluczowe, aby wskazać, czy tworzysz nowy obiekt słownika.</span><span class="sxs-lookup"><span data-stu-id="3deac-481">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="3deac-482">Słownik można przypisać do zmiennej za pomocą `var` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="3deac-482">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="3deac-483">Możesz określać typy danych elementów w słowniku przy użyciu nawiasów ( `< >` ).</span><span class="sxs-lookup"><span data-stu-id="3deac-483">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="3deac-484">Na końcu deklaracji należy dodać parę nawiasów, ponieważ jest to rzeczywiście metodę, która tworzy nowy słownik.</span><span class="sxs-lookup"><span data-stu-id="3deac-484">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="3deac-485">Aby dodać elementy do słownika, możesz wywołać `Add` metody na zmienną słownika (`myScores` w tym przypadku), a następnie określ klucz i wartość.</span><span class="sxs-lookup"><span data-stu-id="3deac-485">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="3deac-486">Alternatywnie można nawiasami kwadratowymi klucza, a następnie wykonaj przypisanie proste, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3deac-486">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="3deac-487">Aby uzyskać wartość ze słownika, należy określić klucz w nawiasach:</span><span class="sxs-lookup"><span data-stu-id="3deac-487">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="3deac-488">Wywołanie metody z parametrami</span><span class="sxs-lookup"><span data-stu-id="3deac-488">Calling Methods with Parameters</span></span>

<span data-ttu-id="3deac-489">Podczas czytania we wcześniejszej części tego artykułu, obiekty, które program za pomocą może mieć metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-489">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="3deac-490">Na przykład `Database` obiekt może mieć `Database.Connect` metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-490">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="3deac-491">Wiele metod również mieć co najmniej jeden parametr.</span><span class="sxs-lookup"><span data-stu-id="3deac-491">Many methods also have one or more parameters.</span></span> <span data-ttu-id="3deac-492">A *parametru* jest wartością, który jest przekazywany do metody, aby włączyć metodę w celu zakończenia zadania.</span><span class="sxs-lookup"><span data-stu-id="3deac-492">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="3deac-493">Na przykład Przyjrzyj się deklarację dla `Request.MapPath` metody, która przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="3deac-493">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="3deac-494">(Wiersz został opakowany w celu poprawienia czytelności.</span><span class="sxs-lookup"><span data-stu-id="3deac-494">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="3deac-495">Należy pamiętać, umieścić podziały wierszy niemal dowolnym miejscu, z wyjątkiem wewnątrz ciągów, które są ujęte w znaki cudzysłowu.)</span><span class="sxs-lookup"><span data-stu-id="3deac-495">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="3deac-496">Ta metoda zwraca ścieżkę fizyczną na serwerze, który odnosi się do określonej ścieżki wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="3deac-496">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="3deac-497">Są trzy parametry dla metody `virtualPath`, `baseVirtualDir`, i `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="3deac-497">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="3deac-498">(Zwróć uwagę, że w deklaracji, parametry są wyświetlane z typami danych, który będzie mógł zaakceptować). Po wywołaniu tej metody należy podać wartości dla wszystkich trzech parametrów.</span><span class="sxs-lookup"><span data-stu-id="3deac-498">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="3deac-499">Składnia Razor daje dwie opcje do przekazywania parametrów do metody: *parametry pozycyjne* i *nazwanych parametrów*.</span><span class="sxs-lookup"><span data-stu-id="3deac-499">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="3deac-500">Aby wywołać metodę, przy użyciu parametry pozycyjne, parametry są przekazywane w kolejności strict, który jest określony w deklaracji metody.</span><span class="sxs-lookup"><span data-stu-id="3deac-500">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="3deac-501">(Będzie zazwyczaj wiesz to zamówienie, przeczytaj dokumentację dla metody.) Należy wykonać, kolejność i nie można pominąć dowolny z parametrów &#8212; Jeśli to konieczne, możesz przekazać pusty ciąg (`""`) lub `null` parametru pozycyjnych, które nie mają wartości.</span><span class="sxs-lookup"><span data-stu-id="3deac-501">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="3deac-502">W poniższym przykładzie założono, masz folder o nazwie *skrypty* w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3deac-502">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="3deac-503">Kod wywołuje `Request.MapPath` metody i przekazuje wartości dla trzech parametrów w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="3deac-503">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="3deac-504">Następnie wyświetla ścieżki wynikowej zamapowany.</span><span class="sxs-lookup"><span data-stu-id="3deac-504">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="3deac-505">Gdy metoda ma wiele parametrów, możesz przechowywać swój kod bardziej czytelny przy użyciu nazwanych parametrów.</span><span class="sxs-lookup"><span data-stu-id="3deac-505">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="3deac-506">Aby wywołać metodę, przy użyciu nazwanych parametrów, należy określić nazwę parametru, po której następuje dwukropek (:), a następnie wartość.</span><span class="sxs-lookup"><span data-stu-id="3deac-506">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="3deac-507">Zaletą nazwanych parametrów jest to, że można przekazać je w dowolnej kolejności, które mają.</span><span class="sxs-lookup"><span data-stu-id="3deac-507">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="3deac-508">(Wadą jest wywołanie metody nie jest najmniejszy).</span><span class="sxs-lookup"><span data-stu-id="3deac-508">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="3deac-509">Poniższy przykład wywołuje tę samą metodę jak powyżej, ale korzysta z nazwanych parametrów, aby podać wartości:</span><span class="sxs-lookup"><span data-stu-id="3deac-509">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="3deac-510">Jak widać, parametry są przekazywane w innej kolejności.</span><span class="sxs-lookup"><span data-stu-id="3deac-510">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="3deac-511">Jednak po uruchomieniu poprzedniego przykładu i w tym przykładzie zostanie zwrócona taką samą wartość.</span><span class="sxs-lookup"><span data-stu-id="3deac-511">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="3deac-512">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="3deac-512">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="3deac-513">Instrukcje try-Catch</span><span class="sxs-lookup"><span data-stu-id="3deac-513">Try-Catch Statements</span></span>

<span data-ttu-id="3deac-514">Często masz instrukcji w kodzie, który może zakończyć się niepowodzeniem ze względów poza Twoją kontrolą.</span><span class="sxs-lookup"><span data-stu-id="3deac-514">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="3deac-515">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3deac-515">For example:</span></span>

- <span data-ttu-id="3deac-516">Jeśli kod próbuje utworzyć lub uzyskać dostęp do pliku, mogą wystąpić wszelkiego rodzaju błędów.</span><span class="sxs-lookup"><span data-stu-id="3deac-516">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="3deac-517">Żądany plik nie istnieje, może być zablokowana, kod może nie mieć uprawnień i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="3deac-517">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="3deac-518">Podobnie jeśli kod próbuje zaktualizować rekordy w bazie danych, może to być problemy z uprawnieniami, mogą być opuszczane połączenia z bazą danych, dane w celu zaoszczędzenia może być nieprawidłowy i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="3deac-518">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="3deac-519">W sposób programowania, są nazywane tych sytuacji *wyjątki*.</span><span class="sxs-lookup"><span data-stu-id="3deac-519">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="3deac-520">Jeśli Twój kod napotka wyjątek, generuje (zgłasza) komunikat o błędzie przykład w najlepszym irytujących dla użytkowników:</span><span class="sxs-lookup"><span data-stu-id="3deac-520">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="3deac-522">W sytuacjach, w którym kod może wystąpić wyjątki i w celu uniknięcia komunikaty o błędach tego typu można użyć `try/catch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="3deac-522">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="3deac-523">W `try` instrukcji, uruchom kod, który jest sprawdzany.</span><span class="sxs-lookup"><span data-stu-id="3deac-523">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="3deac-524">W co najmniej jednej `catch` instrukcji, można wyszukać konkretnego błędów (określonych typów wyjątków), które mogły wystąpić.</span><span class="sxs-lookup"><span data-stu-id="3deac-524">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="3deac-525">Może zawierać tyle `catch` instrukcje, jak należy sprawdzić występowanie błędów, które są przewidywania.</span><span class="sxs-lookup"><span data-stu-id="3deac-525">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="3deac-526">Firma Microsoft zaleca, unikaj używania `Response.Redirect` method in Class metoda `try/catch` instrukcji, ponieważ może to spowodować wyjątek na stronie.</span><span class="sxs-lookup"><span data-stu-id="3deac-526">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="3deac-527">Poniższy przykład pokazuje stronę, która tworzy plik tekstowy na pierwsze żądanie, a następnie wyświetla przycisk, który umożliwia użytkownikowi, otwórz plik.</span><span class="sxs-lookup"><span data-stu-id="3deac-527">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="3deac-528">W przykładzie użyto celowo Zła nazwa pliku, tak aby spowoduje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3deac-528">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="3deac-529">Kod zawiera `catch` instrukcji dla dwóch możliwych wyjątków: `FileNotFoundException`, który występuje, jeśli nazwa pliku jest nieprawidłowa, i `DirectoryNotFoundException`, który występuje, gdy program ASP.NET nawet nie można odnaleźć folderu.</span><span class="sxs-lookup"><span data-stu-id="3deac-529">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="3deac-530">(Aby zobaczyć, jak działa, gdy wszystko będzie działać prawidłowo można komentarz do instrukcji w tym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="3deac-530">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="3deac-531">Jeśli Twój kod nie obsługuje wyjątek, widział takich jak poprzedniej zrzut ekranu przedstawiający stronę błędu.</span><span class="sxs-lookup"><span data-stu-id="3deac-531">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="3deac-532">Jednak `try/catch` sekcji uniemożliwia użytkownikowi widzisz błędy tych typów.</span><span class="sxs-lookup"><span data-stu-id="3deac-532">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="3deac-533">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3deac-533">Additional Resources</span></span>

**<span data-ttu-id="3deac-534">Programowanie za pomocą języka Visual Basic</span><span class="sxs-lookup"><span data-stu-id="3deac-534">Programming with Visual Basic</span></span>**


[<span data-ttu-id="3deac-535">Załącznik: Język Visual Basic i składnia</span><span class="sxs-lookup"><span data-stu-id="3deac-535">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


**<span data-ttu-id="3deac-536">Dokumentacja referencyjna</span><span class="sxs-lookup"><span data-stu-id="3deac-536">Reference Documentation</span></span>**


[<span data-ttu-id="3deac-537">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3deac-537">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="3deac-538">Język C#</span><span class="sxs-lookup"><span data-stu-id="3deac-538">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
