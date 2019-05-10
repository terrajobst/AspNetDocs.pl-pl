---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (Visual Basic) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten dodatek zapewnia przegląd programowania, korzystając z wzorca ASP.NET Web pages w języku Visual Basic z użyciem składni Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65113090"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="b9e48-103">Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="b9e48-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>

<span data-ttu-id="b9e48-104">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b9e48-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b9e48-105">W tym artykule umożliwia przegląd programowania przy użyciu stron ASP.NET Web Pages przy użyciu składni Razor i Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="b9e48-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="b9e48-106">ASP.NET to technologia firmy Microsoft dotyczące uruchamiania dynamicznych stron sieci web na serwerach sieci web.</span><span class="sxs-lookup"><span data-stu-id="b9e48-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="b9e48-107">**Dowiesz się**:</span><span class="sxs-lookup"><span data-stu-id="b9e48-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="b9e48-108">Najważniejsze 8, porady dotyczące wprowadzenie do programowania stron sieci Web platformy ASP.NET używająca składni Razor programowania.</span><span class="sxs-lookup"><span data-stu-id="b9e48-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="b9e48-109">Podstawowe pojęcia programowania, które będą potrzebne.</span><span class="sxs-lookup"><span data-stu-id="b9e48-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="b9e48-110">Jaki kod serwera programu ASP.NET o składni Razor są wszystkie informacje.</span><span class="sxs-lookup"><span data-stu-id="b9e48-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="b9e48-111">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="b9e48-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="b9e48-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="b9e48-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="b9e48-113">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="b9e48-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="b9e48-114">Większość przykładów przy użyciu stron ASP.NET Web Pages o składni Razor używaj języka C#.</span><span class="sxs-lookup"><span data-stu-id="b9e48-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="b9e48-115">Ale języka Visual Basic obsługuje również składni Razor.</span><span class="sxs-lookup"><span data-stu-id="b9e48-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="b9e48-116">Programowanie na stronie sieci web platformy ASP.NET w języku Visual Basic, utworzyć stronę sieci web w taki sposób, przy użyciu *.vbhtml* rozszerzenie nazwy pliku, a następnie dodać kod języka Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="b9e48-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="b9e48-117">Ten artykuł zawiera omówienie pracy z języka Visual Basic i składnię tworzenia stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9e48-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e48-118">Domyślne szablony witryny sieci Web programu Microsoft WebMatrix (**dla piekarni**, **Galeria fotografii**, i **witryny początkowej**, itp.) są dostępne w wersjach C# i Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="b9e48-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="b9e48-119">Szablony programu Visual Basic, można zainstalować jako pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="b9e48-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="b9e48-120">Szablonów sieci Web są zainstalowane w folderze głównym lokacji w folderze o nazwie *Templates firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="b9e48-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="b9e48-121">Najważniejsze 8 porady dotyczące programowania</span><span class="sxs-lookup"><span data-stu-id="b9e48-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="b9e48-122">W tej sekcji przedstawiono kilka wskazówek, które bezwzględnie musisz wiedzieć, po rozpoczęciu pisania kodu serwera ASP.NET używająca składni Razor.</span><span class="sxs-lookup"><span data-stu-id="b9e48-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="b9e48-123">1. Możesz dodać kod do strony przy użyciu znaku @</span><span class="sxs-lookup"><span data-stu-id="b9e48-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="b9e48-124">`@` Znak rozpoczyna się w tekście wyrażeń, bloków pojedynczej instrukcji i bloków wielu instrukcji:</span><span class="sxs-lookup"><span data-stu-id="b9e48-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="b9e48-125">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-125">The result displayed in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="b9e48-127">**Kodowanie HTML**</span><span class="sxs-lookup"><span data-stu-id="b9e48-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="b9e48-128">Podczas wyświetlania zawartości na stronie za pomocą `@` znaków, jak w poprzednich przykładach, ASP.NET koduje jako HTML dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="b9e48-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="b9e48-129">Spowoduje to zastąpienie zastrzeżone znaki HTML (takie jak `<` i `>` i `&`) przy użyciu kodów, umożliwiające znaków, które mają być wyświetlane jako znaki na stronie sieci web, a interpretowany jako tagów HTML lub jednostki.</span><span class="sxs-lookup"><span data-stu-id="b9e48-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="b9e48-130">Zakodowany w formacie HTML, dane wyjściowe z kodu serwera mogą być wyświetlane nieprawidłowo i spowodować narażenie na zagrożenia bezpieczeństwa strony.</span><span class="sxs-lookup"><span data-stu-id="b9e48-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="b9e48-131">Jeśli dowiesz się, jak dane wyjściowe kod znaczników HTML, który renderuje tagi jako kod znaczników (na przykład `<p></p>` akapitu lub `<em></em>` aby wyróżnić tekst), zobacz sekcję [łączenie tekstu, znaczników i kodu w blokach kodu](#BM_CombiningTextMarkupAndCode) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="b9e48-132">Możesz dowiedzieć się więcej o kodowanie HTML w [Praca z formularzami HTML w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="b9e48-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="b9e48-133">2. Umieść bloki kodu przy użyciu kodu... Kod zakończenia</span><span class="sxs-lookup"><span data-stu-id="b9e48-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="b9e48-134">Blok kodu zawiera jeden lub więcej instrukcji kodu i znajduje się za pomocą słów kluczowych `Code` i `End Code`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="b9e48-135">Umieść otwierający `Code` — słowo kluczowe natychmiast po `@` znak &#8212; nie może zawierać odstępów między nimi.</span><span class="sxs-lookup"><span data-stu-id="b9e48-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="b9e48-136">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="b9e48-138">3. Wewnątrz bloku zakończenia każda instrukcja kodu za pomocą podział wiersza</span><span class="sxs-lookup"><span data-stu-id="b9e48-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="b9e48-139">W bloku kodu języka Visual Basic każda instrukcja kończy się podział wiersza.</span><span class="sxs-lookup"><span data-stu-id="b9e48-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="b9e48-140">(W dalszej części tego artykułu zobaczysz sposób opakować instrukcję kodu długa na wiele wierszy, jeśli to konieczne.)</span><span class="sxs-lookup"><span data-stu-id="b9e48-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="b9e48-141">4. W przypadku używania zmiennych do przechowywania wartości</span><span class="sxs-lookup"><span data-stu-id="b9e48-141">4. You use variables to store values</span></span>

<span data-ttu-id="b9e48-142">Można przechowywać wartości w *zmiennej*, w tym ciągi, liczby i daty, itp. Możesz utworzyć nową zmienną za pomocą `Dim` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="b9e48-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="b9e48-143">Wartości zmiennych można wstawić bezpośrednio na stronie za pomocą `@`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="b9e48-144">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="b9e48-146">5. Ujmij wartości literału ciągu w znaki podwójnego cudzysłowu</span><span class="sxs-lookup"><span data-stu-id="b9e48-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="b9e48-147">A *ciąg* jest sekwencją znaków, które są traktowane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="b9e48-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="b9e48-148">Aby określić ciąg, ująć w znaki cudzysłowu:</span><span class="sxs-lookup"><span data-stu-id="b9e48-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="b9e48-149">Aby osadzić podwójnego cudzysłowu wewnątrz wartości ciągu, Wstaw dwa znaki cudzysłowu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="b9e48-150">Znak cudzysłowu się jeden raz w danych wyjściowych strony, wprowadź go w formie `""` w cudzysłowie ciąg i się pojawić się dwa razy, wprowadź go jako `""""` w ramach ciągów w cudzysłowach.</span><span class="sxs-lookup"><span data-stu-id="b9e48-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="b9e48-151">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="b9e48-153">6. Kod języka Visual Basic nie jest uwzględniana wielkość liter</span><span class="sxs-lookup"><span data-stu-id="b9e48-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="b9e48-154">Język Visual Basic nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b9e48-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="b9e48-155">Programowanie słowa kluczowe (takie jak `Dim`, `If`, i `True`) i nazwy zmiennych (takich jak `myString`, lub `subTotal`) mogą być zapisywane w każdym przypadku.</span><span class="sxs-lookup"><span data-stu-id="b9e48-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="b9e48-156">Następujące wiersze kodu przypisać wartość do zmiennej `lastname` przy użyciu małymi literami nazwę, a następnie wyprowadzić wartość zmiennej strony za pomocą wielkie nazwy.</span><span class="sxs-lookup"><span data-stu-id="b9e48-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="b9e48-157">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="b9e48-159">7. Większość kodowania wymaga pracy z obiektami</span><span class="sxs-lookup"><span data-stu-id="b9e48-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="b9e48-160">Obiekt reprezentuje rzecz, którą można programować za pomocą &#8212; strony pola tekstowego, pliku, obraz, żądania sieci web, wiadomości e-mail, rekord klienta (wiersz bazy danych), itp. Obiekty mają właściwości, które opisują ich właściwości &#8212; ma obiekt pola tekstowego `Text` właściwości obiektu żądania ma `Url` właściwość, wiadomość e-mail ma `From` właściwość i obiekt klienta ma `FirstName` Właściwość.</span><span class="sxs-lookup"><span data-stu-id="b9e48-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="b9e48-161">Obiektów ma także metody, które są &quot;zleceń&quot; mogą wykonywać.</span><span class="sxs-lookup"><span data-stu-id="b9e48-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="b9e48-162">Przykłady obejmują obiekt pliku `Save` metodę, obiekt obrazu `Rotate` metody i obiekt e-mail `Send` metody.</span><span class="sxs-lookup"><span data-stu-id="b9e48-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="b9e48-163">Często będziesz pracować `Request` obiektu, który zapewnia informacje, takie jak wartości formularza pola na stronie (pola tekstowe itp.), jakiego rodzaju przeglądarki wysłał żądanie, adres URL strony, tożsamość użytkownika, itp. W tym przykładzie przedstawiono sposób uzyskiwania dostępu do właściwości `Request` obiektu i wywoływania `MapPath` metody `Request` obiektu, który zapewnia ścieżkę bezwzględną strony na serwerze:</span><span class="sxs-lookup"><span data-stu-id="b9e48-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="b9e48-164">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="b9e48-166">8. Można napisać kod, który podejmuje decyzje</span><span class="sxs-lookup"><span data-stu-id="b9e48-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="b9e48-167">Kluczową funkcją dynamicznych stron sieci web jest, czy można określić, co należy zrobić, na podstawie warunków.</span><span class="sxs-lookup"><span data-stu-id="b9e48-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="b9e48-168">Jest najbardziej popularny sposób, w tym celu `If` — instrukcja (i opcjonalnie `Else` instrukcji).</span><span class="sxs-lookup"><span data-stu-id="b9e48-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="b9e48-169">Wykonywanie instrukcji `If IsPost` jest skrót sposobem pisania `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="b9e48-170">Wraz z `If` instrukcji, istnieje wiele sposobów, aby przetestować warunki, powtórz bloki kodu, i itd., które są opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="b9e48-171">Wynik wyświetlany w przeglądarce (po kliknięciu przycisku **przesyłania**):</span><span class="sxs-lookup"><span data-stu-id="b9e48-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="b9e48-173">**HTTP GET i POST metod i właściwości IsPost**</span><span class="sxs-lookup"><span data-stu-id="b9e48-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="b9e48-174">Protokół używany dla stron sieci web (HTTP) obsługuje bardzo ograniczona liczba metod (&quot;zleceń&quot;) które są używane, aby wysyłać żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="b9e48-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="b9e48-175">Dwie najbardziej typowe to GET, który służy do odczytu strony i WPIS, który jest używany do przesyłania strony.</span><span class="sxs-lookup"><span data-stu-id="b9e48-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="b9e48-176">Ogólnie rzecz biorąc po raz pierwszy użytkownik zgłasza żądanie strony, strony jest przesyłane przy użyciu GET.</span><span class="sxs-lookup"><span data-stu-id="b9e48-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="b9e48-177">Jeśli użytkownik wypełnia formularz, a następnie klika przycisk **przesyłania**, przeglądarce wysyła żądanie POST do serwera.</span><span class="sxs-lookup"><span data-stu-id="b9e48-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="b9e48-178">W programowanie dla sieci web jest często grupowaniu można sprawdzić, czy strona jest wymagana jako GET lub POST, aby znać sposób przetwarzania tej strony.</span><span class="sxs-lookup"><span data-stu-id="b9e48-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="b9e48-179">W składniku ASP.NET Web Pages można użyć `IsPost` właściwość, aby sprawdzić, czy żądanie GET lub POST.</span><span class="sxs-lookup"><span data-stu-id="b9e48-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="b9e48-180">Jeśli żądanie jest żądaniem POST `IsPost` właściwość zostanie zwrócona wartość PRAWDA, a można wykonywać takie czynności, takich jak odczyt wartości pola tekstowe w formularzu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="b9e48-181">Wiele przykładów zobaczysz pokazują, jak można przetworzyć strony inaczej w zależności od wartości `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="b9e48-182">Prosty przykład kodu</span><span class="sxs-lookup"><span data-stu-id="b9e48-182">A Simple Code Example</span></span>

<span data-ttu-id="b9e48-183">Ta procedura pokazuje, jak utworzyć stronę, która przedstawia podstawowe techniki programowania.</span><span class="sxs-lookup"><span data-stu-id="b9e48-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="b9e48-184">W tym przykładzie utworzysz stronę, która umożliwia użytkownikom wprowadź dwie liczby, a następnie dodanie ich i wyświetla wynik.</span><span class="sxs-lookup"><span data-stu-id="b9e48-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="b9e48-185">W edytorze, Utwórz nowy plik i nadaj mu nazwę *AddNumbers.vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="b9e48-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="b9e48-186">Skopiuj następujący kod i znaczników do strony, zastępując wszystko już na stronie.</span><span class="sxs-lookup"><span data-stu-id="b9e48-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="b9e48-187">Oto kilka rzeczy, umożliwiające należy zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="b9e48-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="b9e48-188">`@` Znak rozpoczyna się pierwszego bloku kodu na stronie i poprzedza ono `totalMessage` zmiennej osadzone w dolnej części.</span><span class="sxs-lookup"><span data-stu-id="b9e48-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="b9e48-189">Blok, w górnej części strony jest ujęty w `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="b9e48-190">Zmienne `total`, `num1`, `num2`, i `totalMessage` przechowywać kilka liczb i ciąg.</span><span class="sxs-lookup"><span data-stu-id="b9e48-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="b9e48-191">Wartość literału ciągu przypisana do `totalMessage` zmiennej znajduje się w znaki podwójnego cudzysłowu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="b9e48-192">Ponieważ kod języka Visual Basic nie jest uwzględniana wielkość liter, kiedy `totalMessage` zmienna jest używana w dolnej części strony, jego nazwa tylko musi być zgodna pisownię deklaracja zmiennej w górnej części strony.</span><span class="sxs-lookup"><span data-stu-id="b9e48-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="b9e48-193">Wielkość liter w wyrazie nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="b9e48-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="b9e48-194">Wyrażenie `num1.AsInt()`  +  `num2.AsInt()` pokazuje, jak pracować z obiektami i metody.</span><span class="sxs-lookup"><span data-stu-id="b9e48-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="b9e48-195">`AsInt` Metody dla każdej zmiennej konwertuje ciąg wprowadzanego przez użytkownika do liczb całkowitych (liczba całkowita), który można dodać.</span><span class="sxs-lookup"><span data-stu-id="b9e48-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="b9e48-196">`<form>` Znacznik obejmuje `method="post"` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="b9e48-197">Określa to, że gdy użytkownik kliknie **Dodaj**, strony, które będą wysyłane do serwera przy użyciu metody POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9e48-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="b9e48-198">Po przesłaniu strony, kod `If IsPost` daje w wyniku wartość PRAWDA, a warunkowy kod działa, wyświetlanie wynikiem dodania liczb.</span><span class="sxs-lookup"><span data-stu-id="b9e48-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="b9e48-199">Zapisz stronę i uruchom go w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b9e48-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="b9e48-200">(Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.) Wprowadź dwoma liczbami całkowitymi, a następnie kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b9e48-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="b9e48-202">Język Visual Basic i składnia</span><span class="sxs-lookup"><span data-stu-id="b9e48-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="b9e48-203">Wcześniej był wyświetlany podstawowy przykład sposobu tworzenia strony sieci web ASP.NET i jak możesz dodać kod serwera do kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="b9e48-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="b9e48-204">Tutaj dowiesz się podstawowe informacje dotyczące pisania kodu serwera ASP.NET używająca składni Razor przy użyciu języka Visual Basic &#8212; czyli programowania reguły języka.</span><span class="sxs-lookup"><span data-stu-id="b9e48-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="b9e48-205">Jeśli masz doświadczenie w pracy z programowania (zwłaszcza, jeśli używano C, C++, C#, Visual Basic lub JavaScript), większość tutaj przeczytaj będą niczym nowym.</span><span class="sxs-lookup"><span data-stu-id="b9e48-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="b9e48-206">Prawdopodobnie należy zapoznać się tylko z jak kod w programie WebMatrix jest dodawany do znaczników w *.vbhtml* plików.</span><span class="sxs-lookup"><span data-stu-id="b9e48-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="b9e48-207">Łączenie tekstu, znaczników i kodu w blokach kodu</span><span class="sxs-lookup"><span data-stu-id="b9e48-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="b9e48-208">W blokach kodu serwera należy często danych wyjściowych tekst i znacznik do strony.</span><span class="sxs-lookup"><span data-stu-id="b9e48-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="b9e48-209">Jeśli blok kodu serwera zawiera tekst nie jest kodem, który zamiast tego powinien być renderowany jako jest, ASP.NET musi być w stanie odróżnić go od kodu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="b9e48-210">Istnieje kilka sposobów, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="b9e48-210">There are several ways to do this.</span></span>

- <span data-ttu-id="b9e48-211">Należy wpisać tekst w elemencie bloku HTML, takich jak `<p></p>` lub `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="b9e48-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="b9e48-212">HTML element może zawierać tekstu, dodatkowe elementy HTML i wyrażenia w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="b9e48-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="b9e48-213">Gdy program ASP.NET widzi otwierający HTML tag (na przykład `<p>`), wszystko, czego renderowania elementu a jej zawartość jako przeglądarki (i rozwiązuje wyrażeń kodu serwera).</span><span class="sxs-lookup"><span data-stu-id="b9e48-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="b9e48-214">Użyj `@:` operatora lub `<text>` elementu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="b9e48-215">`@:` Generuje pojedynczy wiersz zawartość zawierająca zwykły tekst lub niedopasowane tagów HTML; `<text>` element zawiera wiele wierszy w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="b9e48-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="b9e48-216">Te opcje są przydatne, jeśli nie chcesz renderować HTML element jako część danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="b9e48-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="b9e48-217">W poniższym przykładzie jest powtarzany w poprzednim przykładzie, ale używa jedna para `<text>` tagów, aby ująć tekstu do renderowania.</span><span class="sxs-lookup"><span data-stu-id="b9e48-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="b9e48-218">W poniższym przykładzie `<text>` i `</text>` tagi należy ująć w trzy wiersze, które mają niektóre niedopasowane tagi HTML i uncontained tekst (`<br />`), wraz z kodu serwera i dopasowane tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="b9e48-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="b9e48-219">Ponownie, również może poprzedzać każdy wiersz za pomocą `@:` operator; albo sposób działania.</span><span class="sxs-lookup"><span data-stu-id="b9e48-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="b9e48-220">Gdy tekstu wyjściowego przedstawione w tej sekcji &#8212; przy użyciu elementu HTML `@:` operatora lub `<text>` elementu &#8212; ASP.NET nie kodowanie HTML dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="b9e48-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="b9e48-221">(Jak wspomniano wcześniej, ASP.NET, kodowania danych wyjściowych wyrażeń kodu serwera i serwera bloków kodu, które są poprzedzone `@`, z wyjątkiem specjalnych przypadków wymienionych w tej sekcji.)</span><span class="sxs-lookup"><span data-stu-id="b9e48-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="b9e48-222">Białe znaki</span><span class="sxs-lookup"><span data-stu-id="b9e48-222">Whitespace</span></span>

<span data-ttu-id="b9e48-223">Dodatkowe spacje w instrukcji (i poza literału ciągu), nie mają wpływu na instrukcji:</span><span class="sxs-lookup"><span data-stu-id="b9e48-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="b9e48-224">Podzielenie długie instrukcje na wiele wierszy</span><span class="sxs-lookup"><span data-stu-id="b9e48-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="b9e48-225">Instrukcja długie kodu można podzielić wiele wierszy za pomocą znaku podkreślenia `_` (w języku Visual Basic nazywana *znak kontynuacji*) po każdym wierszu kodu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="b9e48-226">Aby przerwać instrukcję do następnego wiersza, na końcu wiersza dodać spację, a następnie znak kontynuacji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="b9e48-227">Continue — instrukcja w następnym wierszu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-227">Continue the statement on the next line.</span></span> <span data-ttu-id="b9e48-228">Może zawijać się instrukcje, na których trzeba poprawić czytelność dowolną liczbę wierszy.</span><span class="sxs-lookup"><span data-stu-id="b9e48-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="b9e48-229">Poniższe instrukcje są takie same:</span><span class="sxs-lookup"><span data-stu-id="b9e48-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="b9e48-230">Jednak nie można opakować linię trakcie literału ciągu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="b9e48-231">Poniższy przykład nie działa:</span><span class="sxs-lookup"><span data-stu-id="b9e48-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="b9e48-232">Aby połączyć długi ciąg, który jest zawijany do wielu linii, takich jak kod powyżej, czy należy użyć *operatora łączenia* (`&`), które zostaną wyświetlone w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="b9e48-233">Komentarze w kodzie</span><span class="sxs-lookup"><span data-stu-id="b9e48-233">Code comments</span></span>

<span data-ttu-id="b9e48-234">Komentarze umożliwiają pozostawienie notatki dla siebie i innych osób.</span><span class="sxs-lookup"><span data-stu-id="b9e48-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="b9e48-235">Komentarze składni razor mają prefiks `@*` i kończyć się znakiem `*@`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="b9e48-236">Wewnątrz bloków kodu można użyć komentarze składni Razor, lub można użyć zwykły znak komentarz języka Visual Basic, który jest pojedynczy cudzysłów (`'`) prefiks do każdego wiersza.</span><span class="sxs-lookup"><span data-stu-id="b9e48-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="b9e48-237">Zmienne</span><span class="sxs-lookup"><span data-stu-id="b9e48-237">Variables</span></span>

<span data-ttu-id="b9e48-238">Zmienna jest nazwany obiekt, który służy do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="b9e48-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="b9e48-239">Zmienne możesz nazwać wszystko, ale nazwa musi rozpoczynać się od litery alfabetu i nie może zawierać spacji ani znaków zarezerwowanych.</span><span class="sxs-lookup"><span data-stu-id="b9e48-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="b9e48-240">W języku Visual Basic, jak przedstawiono wcześniej, wielkość liter w nazwie zmiennej nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="b9e48-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="b9e48-241">Zmienne i typy danych</span><span class="sxs-lookup"><span data-stu-id="b9e48-241">Variables and data types</span></span>

<span data-ttu-id="b9e48-242">Zmienna może mieć na określony typ danych, która wskazuje, jakiego typu dane są przechowywane w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="b9e48-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="b9e48-243">Mogą mieć zmiennych ciągu, które przechowują wartości ciągu (takich jak &quot;Witaj, świecie&quot;), zmiennych całkowitych, które przechowują wartości liczby całkowitej (np. 3 lub 79) i zmienne daty, które przechowywanie wartości daty w różnych formatach (np. 4/12/2012 lub marca 2009 ).</span><span class="sxs-lookup"><span data-stu-id="b9e48-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="b9e48-244">Wiąże się wiele typów danych, w których można użyć.</span><span class="sxs-lookup"><span data-stu-id="b9e48-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="b9e48-245">Jednakże nie trzeba określać typu zmiennej.</span><span class="sxs-lookup"><span data-stu-id="b9e48-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="b9e48-246">W większości przypadków platformy ASP.NET można ustalić typu, w oparciu sposobu korzystania z danych w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="b9e48-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="b9e48-247">(Czasami musisz określić typ; pojawi się przykłady którym ta zasada obowiązuje).</span><span class="sxs-lookup"><span data-stu-id="b9e48-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="b9e48-248">Aby zadeklarować zmienną bez określania typu, należy użyć `Dim` oraz nazwę zmiennej (na przykład `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="b9e48-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="b9e48-249">Aby zadeklarować zmienną typu, należy użyć `Dim` oraz nazwę zmiennej, a następnie `As` i następnie nazwę typu (na przykład `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="b9e48-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="b9e48-250">Poniższy przykład przedstawia niektóre wbudowane wyrażeń zmiennych na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="b9e48-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="b9e48-251">Wynik jest wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="b9e48-253">Konwertowanie i testowanie typów danych</span><span class="sxs-lookup"><span data-stu-id="b9e48-253">Converting and testing data types</span></span>

<span data-ttu-id="b9e48-254">Mimo że program ASP.NET zazwyczaj może automatycznie określić typu danych, czasami nie jest.</span><span class="sxs-lookup"><span data-stu-id="b9e48-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="b9e48-255">W związku z tym konieczne może być pomógł ASP.NET, wykonując jawnej konwersji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="b9e48-256">Nawet jeśli nie masz konwersji typów, czasami warto sprawdzić, jakiego typu dane być może pracujesz z.</span><span class="sxs-lookup"><span data-stu-id="b9e48-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="b9e48-257">Najbardziej często zdarza się, trzeba przekonwertować ciąg do innego typu, takie jak liczbą całkowitą lub daty.</span><span class="sxs-lookup"><span data-stu-id="b9e48-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="b9e48-258">Poniższy przykład przedstawia typową sytuacją, w którym należy przekonwertować ciąg na liczbę.</span><span class="sxs-lookup"><span data-stu-id="b9e48-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="b9e48-259">Zgodnie z zasadą dane wejściowe użytkownika dostarczany jako ciągi.</span><span class="sxs-lookup"><span data-stu-id="b9e48-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="b9e48-260">Nawet jeśli została monituje użytkownika o wprowadzenie numeru, a nawet wtedy, gdy zostały wprowadzone cyfr, podczas przesyłania danych wejściowych użytkownika, a następnie go odczytać w kodzie, dane są w formacie ciągu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="b9e48-261">W związku z tym należy przekonwertować ciąg na liczbę.</span><span class="sxs-lookup"><span data-stu-id="b9e48-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="b9e48-262">W przykładzie Jeśli zostanie podjęta próba wykonania operacji arytmetycznych na wartościach, bez konwersji, następujący błąd wyników, ponieważ program ASP.NET nie można dodać dwa ciągi:</span><span class="sxs-lookup"><span data-stu-id="b9e48-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="b9e48-263">Aby przekonwertować wartości liczb całkowitych, należy wywołać `AsInt` metody.</span><span class="sxs-lookup"><span data-stu-id="b9e48-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="b9e48-264">Jeśli konwersja się pomyślnie, następnie można dodać liczb.</span><span class="sxs-lookup"><span data-stu-id="b9e48-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="b9e48-265">Poniższa lista zawiera niektóre typowe metody konwersji i testowania dla zmiennych.</span><span class="sxs-lookup"><span data-stu-id="b9e48-265">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <strong>Method</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Example</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converts a string that represents a whole number (like &quot;593&quot;) to an integer.
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
        Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.
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
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.
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
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number. (In ASP.NET, a decimal number is more precise than a floating-point number.)
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
        Converts a string that represents a date and time value to the ASP.NET `DateTime` type.
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
        Converts any other data type to a string.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="b9e48-266">Operatory</span><span class="sxs-lookup"><span data-stu-id="b9e48-266">Operators</span></span>

<span data-ttu-id="b9e48-267">Operator jest słowo kluczowe lub znak, który informuje ASP.NET, jakiego rodzaju polecenie do wykonania w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-267">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="b9e48-268">Visual Basic obsługuje wiele operatorów, ale musisz rozpoznać kilka, aby rozpocząć wdrażanie stron ASP.NET web pages.</span><span class="sxs-lookup"><span data-stu-id="b9e48-268">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="b9e48-269">Poniższa tabela zawiera podsumowanie najbardziej typowych operatorów.</span><span class="sxs-lookup"><span data-stu-id="b9e48-269">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Examples</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Math operators used in numerical expressions.
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
        Assignment and equality. Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.
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
        Inequality. Returns `True` if the values are not equal.
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
        Less than, greater than, less than or equal, and greater than or equal.
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
        Concatenation, which is used to join strings.
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
        The increment and decrement operators, which add and subtract 1 (respectively) from a variable.
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
        Dot. Used to distinguish objects and their properties and methods.
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
        Parentheses. Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.
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
        Not. Reverses a true value to false and vice versa. Typically used as a shorthand way to test for `False` (that is, for not `True`).
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
        Logical AND and OR, which are used to link conditions together.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="b9e48-270">Praca z pliku i ścieżki folderu, w kodzie</span><span class="sxs-lookup"><span data-stu-id="b9e48-270">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="b9e48-271">Będziesz często pracować ze ścieżkami plików i folderów w kodzie.</span><span class="sxs-lookup"><span data-stu-id="b9e48-271">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="b9e48-272">Oto przykład strukturę folderu fizycznego dla witryny sieci Web mogą być wyświetlane na komputerze deweloperskim:</span><span class="sxs-lookup"><span data-stu-id="b9e48-272">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="b9e48-273">Poniżej przedstawiono niektóre podstawowe szczegóły dotyczące adresów URL i ścieżki:</span><span class="sxs-lookup"><span data-stu-id="b9e48-273">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="b9e48-274">Adres URL zaczyna się od jedną nazwę domeny (`http://www.example.com`) lub nazwy serwera (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="b9e48-274">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="b9e48-275">Adres URL odnosi się do ścieżki fizycznej na komputerze-hoście.</span><span class="sxs-lookup"><span data-stu-id="b9e48-275">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="b9e48-276">Na przykład `http://myserver` może odpowiadać folder *C:\websites\mywebsite* na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b9e48-276">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="b9e48-277">Ścieżka wirtualna jest skrótem do reprezentowania ścieżek w kodzie bez konieczności podać pełną ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="b9e48-277">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="b9e48-278">Obejmuje on część adresu URL, który następuje nazwa domeny lub serwera.</span><span class="sxs-lookup"><span data-stu-id="b9e48-278">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="b9e48-279">Gdy używasz ścieżki wirtualne, kod można przenieść do innej domeny lub serwer, bez konieczności aktualizowania ścieżki.</span><span class="sxs-lookup"><span data-stu-id="b9e48-279">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="b9e48-280">Oto przykład, aby lepiej zrozumieć różnice:</span><span class="sxs-lookup"><span data-stu-id="b9e48-280">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="b9e48-281">Pełny adres URL</span><span class="sxs-lookup"><span data-stu-id="b9e48-281">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="b9e48-282">Nazwa serwera</span><span class="sxs-lookup"><span data-stu-id="b9e48-282">Server name</span></span> | <span data-ttu-id="b9e48-283">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="b9e48-283">*mycompanyserver*</span></span> |
| <span data-ttu-id="b9e48-284">Ścieżka wirtualna</span><span class="sxs-lookup"><span data-stu-id="b9e48-284">Virtual path</span></span> | <span data-ttu-id="b9e48-285">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="b9e48-285">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="b9e48-286">Ścieżka fizyczna</span><span class="sxs-lookup"><span data-stu-id="b9e48-286">Physical path</span></span> | <span data-ttu-id="b9e48-287">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="b9e48-287">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="b9e48-288">Wirtualny katalog główny jest /, podobnie jak głównego C: dysku \.</span><span class="sxs-lookup"><span data-stu-id="b9e48-288">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="b9e48-289">(Folder wirtualny ścieżki zawsze używać ukośników.) Ścieżka wirtualna folderu nie musi mieć taką samą nazwę jak folder fizycznych; może być aliasem.</span><span class="sxs-lookup"><span data-stu-id="b9e48-289">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="b9e48-290">(Na serwerach produkcyjnych, ścieżka wirtualna rzadko odpowiada dokładnej ścieżki fizycznej.)</span><span class="sxs-lookup"><span data-stu-id="b9e48-290">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="b9e48-291">Podczas pracy z plikami i folderami w kodzie, czasami musisz odwoływać się do ścieżki fizycznej, a czasami ścieżką wirtualną, w zależności od tego, jakie obiekty pracujemy z użyciem.</span><span class="sxs-lookup"><span data-stu-id="b9e48-291">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="b9e48-292">ASP.NET udostępnia te narzędzia do pracy ze ścieżkami plików i folderów w kodzie: `Server.MapPath` metody i `~` operatora i `Href` metody.</span><span class="sxs-lookup"><span data-stu-id="b9e48-292">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="b9e48-293">Konwertowanie ścieżki wirtualnej do fizycznego: Metoda Server.MapPath</span><span class="sxs-lookup"><span data-stu-id="b9e48-293">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="b9e48-294">`Server.MapPath` Metoda konwertuje ścieżki wirtualnej (np. */default.cshtml*) do ścieżki bezwzględnej fizycznych (takich jak *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b9e48-294">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="b9e48-295">Możesz użyć tej metody w każdej chwili należy pełną ścieżkę fizyczną.</span><span class="sxs-lookup"><span data-stu-id="b9e48-295">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="b9e48-296">Typowym przykładem jest podczas odczytywania lub zapisywania pliku tekstowego lub pliku obrazu na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="b9e48-296">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="b9e48-297">Zazwyczaj nie wiesz, bezwzględną ścieżkę fizyczną witryny na serwerze lokacji hostingu, więc tej metody można przekonwertować ścieżkę wiadomo — ścieżka wirtualna — do odpowiedniej ścieżki na serwerze dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="b9e48-297">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="b9e48-298">Ścieżka wirtualna jest przekazywane do pliku lub folderu, do metody i zwraca ścieżkę fizyczną:</span><span class="sxs-lookup"><span data-stu-id="b9e48-298">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="b9e48-299">Odwoływanie się do wirtualnego katalogu głównego: ~ operatora i Href, metoda</span><span class="sxs-lookup"><span data-stu-id="b9e48-299">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="b9e48-300">W *.cshtml* lub *.vbhtml* pliku, możesz odwoływać się za pomocą ścieżka wirtualnego katalogu głównego `~` operatora.</span><span class="sxs-lookup"><span data-stu-id="b9e48-300">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="b9e48-301">Może to być bardzo przydatne, ponieważ możesz poruszać się strony w witrynie, a wszystkie łącza, które zawierają do innych stron nie będzie działać.</span><span class="sxs-lookup"><span data-stu-id="b9e48-301">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="b9e48-302">Jest to również przydatne w przypadku, gdy przeniesiesz kiedykolwiek witryny sieci Web w innej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-302">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="b9e48-303">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="b9e48-303">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="b9e48-304">Jeśli witryna sieci Web `http://myserver/myapp`, Oto jak ASP.NET będzie traktować te ścieżki, po uruchomieniu na stronie:</span><span class="sxs-lookup"><span data-stu-id="b9e48-304">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="b9e48-305">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="b9e48-305">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="b9e48-306">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="b9e48-306">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="b9e48-307">(Te ścieżki faktycznie nie będzie widoczna jako wartości zmiennej, ale ASP.NET traktują ścieżki, tak, jakby to, jakie były).</span><span class="sxs-lookup"><span data-stu-id="b9e48-307">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="b9e48-308">Możesz użyć `~` operator zarówno w kodzie serwera (jak powyżej), jak i znaczników w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b9e48-308">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="b9e48-309">W znaczniku, użyj `~` operatora do utworzenia ścieżki do zasobów, takich jak pliki obrazów, innych stron sieci web i plików CSS.</span><span class="sxs-lookup"><span data-stu-id="b9e48-309">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="b9e48-310">Po uruchomieniu strony ASP.NET przegląda strony (kodu i znaczników) i usuwa wszystkie `~` odwołania do danej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="b9e48-310">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="b9e48-311">Logika warunkowe i pętle</span><span class="sxs-lookup"><span data-stu-id="b9e48-311">Conditional Logic and Loops</span></span>

<span data-ttu-id="b9e48-312">Kod serwera ASP.NET umożliwia wykonywanie zadań na podstawie warunków i zapisu kod powtarzany instrukcji określonej liczby razy oznacza to, że kod uruchamiany pętli).</span><span class="sxs-lookup"><span data-stu-id="b9e48-312">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="b9e48-313">Warunki badania</span><span class="sxs-lookup"><span data-stu-id="b9e48-313">Testing conditions</span></span>

<span data-ttu-id="b9e48-314">Aby przetestować warunku prostego, możesz użyć `If...Then` instrukcję, która zwraca `True` lub `False` na podstawie testu, można określić:</span><span class="sxs-lookup"><span data-stu-id="b9e48-314">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="b9e48-315">`If` — Słowo kluczowe uruchamia blok.</span><span class="sxs-lookup"><span data-stu-id="b9e48-315">The `If` keyword starts a block.</span></span> <span data-ttu-id="b9e48-316">Test rzeczywisty (stan) jest zgodna `If` — słowo kluczowe i zwraca wartość PRAWDA lub FAŁSZ.</span><span class="sxs-lookup"><span data-stu-id="b9e48-316">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="b9e48-317">`If` Instrukcja kończy się `Then`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-317">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="b9e48-318">Znajdują się wewnątrz instrukcji, które zostanie uruchomione, gdy test jest true `If` i `End If`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-318">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="b9e48-319">`If` Może zawierać instrukcji `Else` blok, który określa instrukcje do uruchomienia, jeśli warunek nie jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="b9e48-319">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="b9e48-320">Jeśli `If` instrukcji rozpoczyna blok kodu, nie trzeba stosować normalną `Code...End Code` instrukcji, aby zawierała bloki.</span><span class="sxs-lookup"><span data-stu-id="b9e48-320">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="b9e48-321">Można dodać `@` do bloku, i będzie ona działać.</span><span class="sxs-lookup"><span data-stu-id="b9e48-321">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="b9e48-322">Ta metoda działa z `If` oraz innych programowania słów kluczowych, które następują bloków kodu, w tym Visual Basic `For`, `For Each`, `Do While`itp.</span><span class="sxs-lookup"><span data-stu-id="b9e48-322">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="b9e48-323">Można dodać wiele warunków przy użyciu co najmniej jeden `ElseIf` bloków:</span><span class="sxs-lookup"><span data-stu-id="b9e48-323">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="b9e48-324">W tym przykładzie, jeśli pierwszy warunek w `If` bloku nie ma wartość true, `ElseIf` warunek jest sprawdzany.</span><span class="sxs-lookup"><span data-stu-id="b9e48-324">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="b9e48-325">Jeśli ten warunek jest spełniony, instrukcje w `ElseIf` bloku są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="b9e48-325">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="b9e48-326">Jeśli żaden z warunków nie jest spełniony, instrukcje w `Else` bloku są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="b9e48-326">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="b9e48-327">Można dodać dowolną liczbę `ElseIf` blokuje, a następnie Zamknij z `Else` zablokować, jako &quot;cała reszta&quot; warunku.</span><span class="sxs-lookup"><span data-stu-id="b9e48-327">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="b9e48-328">Aby przetestować dużą liczbę warunków, należy użyć `Select Case` bloku:</span><span class="sxs-lookup"><span data-stu-id="b9e48-328">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="b9e48-329">Wartość do sprawdzenia jest w nawiasach (w przykładzie zmienna dzień tygodnia).</span><span class="sxs-lookup"><span data-stu-id="b9e48-329">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="b9e48-330">Każdy pojedynczy test używa `Case` instrukcję, która zawiera listę wartości.</span><span class="sxs-lookup"><span data-stu-id="b9e48-330">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="b9e48-331">Jeśli wartość `Case` instrukcji pasuje do wartości testu, kod w tym `Case` blok jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="b9e48-331">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="b9e48-332">Wynik ostatnich dwóch bloków warunkowych, wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-332">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="b9e48-334">Tworzenie pętli kodu</span><span class="sxs-lookup"><span data-stu-id="b9e48-334">Looping code</span></span>

<span data-ttu-id="b9e48-335">Często muszą uruchomić te same instrukcje wielokrotnie.</span><span class="sxs-lookup"><span data-stu-id="b9e48-335">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="b9e48-336">W tym pętli są uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="b9e48-336">You do this by looping.</span></span> <span data-ttu-id="b9e48-337">Na przykład można często uruchamiane te same instrukcje dla każdego elementu w kolekcji danych.</span><span class="sxs-lookup"><span data-stu-id="b9e48-337">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="b9e48-338">Jeśli wiesz, dokładnie tak jak często chcesz pętli, możesz użyć `For` pętli.</span><span class="sxs-lookup"><span data-stu-id="b9e48-338">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="b9e48-339">Ten rodzaj pętli jest szczególnie przydatna podczas liczenia w lub inwentaryzacji:</span><span class="sxs-lookup"><span data-stu-id="b9e48-339">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="b9e48-340">Pętla zaczyna się od `For` — słowo kluczowe następują trzy elementy:</span><span class="sxs-lookup"><span data-stu-id="b9e48-340">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="b9e48-341">Natychmiast po `For` instrukcji, Zadeklaruj zmienną licznika (nie trzeba stosować `Dim`), a następnie zaznaczyć zakres, podobnie jak w `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-341">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="b9e48-342">Oznacza to, że zmienna `i` uruchomi zliczania, 10 i kontynuować, dopóki nie osiągnie 20 (włącznie).</span><span class="sxs-lookup"><span data-stu-id="b9e48-342">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="b9e48-343">Między `For` i `Next` instrukcji znajduje się odpowiednia zawartość bloku.</span><span class="sxs-lookup"><span data-stu-id="b9e48-343">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="b9e48-344">To może zawierać jedną lub więcej kodu instrukcji, które są wykonywane w każdej iteracji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-344">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="b9e48-345">`Next i` Instrukcja kończy pętli.</span><span class="sxs-lookup"><span data-stu-id="b9e48-345">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="b9e48-346">On zwiększa licznik i uruchamia następnej iteracji pętli.</span><span class="sxs-lookup"><span data-stu-id="b9e48-346">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="b9e48-347">W wierszu kodu między `For` i `Next` wierszy zawiera kod, który jest wykonywany dla każdej iteracji pętli.</span><span class="sxs-lookup"><span data-stu-id="b9e48-347">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="b9e48-348">Znaczniki, tworzy nowy akapit (`<p>` elementu) każdego czas i dodaje wiersz danych wyjściowych, wyświetlanie wartości i (licznik).</span><span class="sxs-lookup"><span data-stu-id="b9e48-348">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="b9e48-349">Po uruchomieniu tej strony, ten przykład tworzy 11 wiersza, wyświetlanie danych wyjściowych z tekstem w każdym wierszu, wskazującą liczbę elementów.</span><span class="sxs-lookup"><span data-stu-id="b9e48-349">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="b9e48-351">Jeśli pracujesz z kolekcji lub tablicy, są często używane `For Each` pętli.</span><span class="sxs-lookup"><span data-stu-id="b9e48-351">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="b9e48-352">Kolekcja to grupa podobnych obiektów i `For Each` pętli pozwala przeprowadzić zadanie dla każdego elementu w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-352">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="b9e48-353">Ten typ pętli jest wygodne w przypadku kolekcji, ponieważ w odróżnieniu od `For` pętli, nie trzeba ten licznik lub Ustaw limit.</span><span class="sxs-lookup"><span data-stu-id="b9e48-353">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="b9e48-354">Zamiast tego `For Each` kodu pętli po prostu obejmującego kolekcji dopiero po jej zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-354">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="b9e48-355">W tym przykładzie zwraca elementy w `Request.ServerVariables` kolekcję (która zawiera informacje o serwerze sieci web).</span><span class="sxs-lookup"><span data-stu-id="b9e48-355">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="b9e48-356">Używa ona `For Each` pętli, aby wyświetlić nazwę każdego elementu przez utworzenie nowego `<li>` element na liście punktowanej we wcześniejszej HTML.</span><span class="sxs-lookup"><span data-stu-id="b9e48-356">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="b9e48-357">`For Each` — Słowo kluczowe następuje zmienną, która reprezentuje pojedynczy element w kolekcji (w tym przykładzie `myItem`), a następnie `In` — słowo kluczowe, następuje kolekcję ma pętli.</span><span class="sxs-lookup"><span data-stu-id="b9e48-357">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="b9e48-358">W treści `For Each` pętli, możesz uzyskać dostęp bieżącego elementu przy użyciu zmiennej zadeklarowanej wcześniej.</span><span class="sxs-lookup"><span data-stu-id="b9e48-358">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="b9e48-360">Aby utworzyć bardziej ogólnego przeznaczenia pętli, użyj `Do While` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="b9e48-360">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="b9e48-361">Ta pętla zaczyna się od `Do While` — słowo kluczowe warunku, a następnie bloku, aby powtórzyć.</span><span class="sxs-lookup"><span data-stu-id="b9e48-361">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="b9e48-362">Zazwyczaj Zwiększ pętli (dodać) lub dekrementacji (odjęcia od) zmiennej lub obiektu użytego do zliczania.</span><span class="sxs-lookup"><span data-stu-id="b9e48-362">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="b9e48-363">W tym przykładzie `+=` operator dodaje 1 wartość zmiennej przy każdym uruchomieniu pętli.</span><span class="sxs-lookup"><span data-stu-id="b9e48-363">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="b9e48-364">(W celu dekrementacja zmiennej w pętlę, który odlicza, należy użyć operatora dekrementacyjnego `-=`.)</span><span class="sxs-lookup"><span data-stu-id="b9e48-364">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="b9e48-365">Obiekty i kolekcje</span><span class="sxs-lookup"><span data-stu-id="b9e48-365">Objects and Collections</span></span>

<span data-ttu-id="b9e48-366">Prawie wszystko w witrynie internetowej platformy ASP.NET jest obiektem, w tym strona sieci web.</span><span class="sxs-lookup"><span data-stu-id="b9e48-366">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="b9e48-367">W tej sekcji omówiono niektóre ważne obiekty, które często będziesz pracować z w kodzie.</span><span class="sxs-lookup"><span data-stu-id="b9e48-367">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="b9e48-368">Obiekty strony</span><span class="sxs-lookup"><span data-stu-id="b9e48-368">Page objects</span></span>

<span data-ttu-id="b9e48-369">Najbardziej podstawowa obiektu w programie ASP.NET: jest to strona.</span><span class="sxs-lookup"><span data-stu-id="b9e48-369">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="b9e48-370">Możesz uzyskać dostęp właściwości obiektu strony bezpośrednio, bez dowolnych obiektów kwalifikujących.</span><span class="sxs-lookup"><span data-stu-id="b9e48-370">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="b9e48-371">Poniższy kod pobiera ścieżkę do pliku, przy użyciu `Request` obiekt strony:</span><span class="sxs-lookup"><span data-stu-id="b9e48-371">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="b9e48-372">Można użyć właściwości `Page` można uzyskać mnóstwo informacji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="b9e48-372">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="b9e48-373">`Request`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-373">`Request`.</span></span> <span data-ttu-id="b9e48-374">Jak już przeczytane, to zbiór informacji o bieżącym żądaniu, w tym, jakiego rodzaju przeglądarki wysłał żądanie, adres URL strony, tożsamość użytkownika, itp.</span><span class="sxs-lookup"><span data-stu-id="b9e48-374">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="b9e48-375">`Response`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-375">`Response`.</span></span> <span data-ttu-id="b9e48-376">Jest to zbiór informacji na temat odpowiedzi (strona), który będzie wysyłany do przeglądarki, gdy kod serwera zakończył działanie.</span><span class="sxs-lookup"><span data-stu-id="b9e48-376">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="b9e48-377">Na przykład można używasz tej właściwości można zapisać informacji do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b9e48-377">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="b9e48-378">Obiekty kolekcji (tablic i słowników)</span><span class="sxs-lookup"><span data-stu-id="b9e48-378">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="b9e48-379">Kolekcja to grupa obiektów tego samego typu, na przykład zbiór `Customer` obiektów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b9e48-379">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="b9e48-380">Program ASP.NET zawiera wiele kolekcji wbudowanych, takie jak `Request.Files` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-380">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="b9e48-381">Często będziesz pracować z danymi w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-381">You'll often work with data in collections.</span></span> <span data-ttu-id="b9e48-382">Istnieją dwa typy kolekcji typowe *tablicy* i *słownika*.</span><span class="sxs-lookup"><span data-stu-id="b9e48-382">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="b9e48-383">Tablica jest przydatne, gdy ma być przechowywany w kolekcji podobnych elementów, ale nie chcesz utworzyć osobne zmienną do przechowywania każdego elementu:</span><span class="sxs-lookup"><span data-stu-id="b9e48-383">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="b9e48-384">W przypadku tablic, możesz zadeklarować na określony typ danych, takich jak `String`, `Integer`, lub `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-384">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="b9e48-385">Aby wskazać, że zmienna może zawierać tablicę, Dodaj nawiasy do nazwy zmiennej w deklaracji (takie jak `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="b9e48-385">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="b9e48-386">Możesz uzyskać dostęp elementów w tablicy przy użyciu ich pozycji (indeks) lub za pomocą `For Each` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-386">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="b9e48-387">Indeksy tablicy są oparte na zerze &#8212; oznacza to, pierwszy element jest na pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="b9e48-387">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="b9e48-388">Można określić liczbę elementów w tablicy, uzyskując jego `Length` właściwości.</span><span class="sxs-lookup"><span data-stu-id="b9e48-388">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="b9e48-389">Można pobrać pozycja konkretny element w tablicy (oznacza to, aby wyszukać), użyj `Array.IndexOf` metody.</span><span class="sxs-lookup"><span data-stu-id="b9e48-389">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="b9e48-390">Można również wykonać elementów, takich jak wstecznego zawartość tablicy ( `Array.Reverse` metoda) i sortować zawartość ( `Array.Sort` metody).</span><span class="sxs-lookup"><span data-stu-id="b9e48-390">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="b9e48-391">Dane wyjściowe kodu tablicy ciąg wyświetlany w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b9e48-391">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="b9e48-393">Słownik to kolekcja par klucz wartość, określane klucz (lub nazwę) można ustawić lub pobrać odpowiednią wartość:</span><span class="sxs-lookup"><span data-stu-id="b9e48-393">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="b9e48-394">Aby utworzyć słownik, należy użyć `New` — słowo kluczowe, aby wskazać, czy tworzysz nową `Dictionary` obiektu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-394">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="b9e48-395">Słownik można przypisać do zmiennej za pomocą `Dim` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="b9e48-395">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="b9e48-396">Możesz określać typy danych elementów w słowniku przy użyciu nawiasów ( `( )` ).</span><span class="sxs-lookup"><span data-stu-id="b9e48-396">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="b9e48-397">Na końcu deklaracji należy dodać kolejną parę nawiasów, ponieważ jest to rzeczywiście metodę, która tworzy nowy słownik.</span><span class="sxs-lookup"><span data-stu-id="b9e48-397">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="b9e48-398">Aby dodać elementy do słownika, możesz wywołać `Add` metody na zmienną słownika (`myScores` w tym przypadku), a następnie określ klucz i wartość.</span><span class="sxs-lookup"><span data-stu-id="b9e48-398">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="b9e48-399">Alternatywnie można użyć nawiasów do klucza, a następnie wykonaj przypisanie proste, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="b9e48-399">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="b9e48-400">Aby uzyskać wartość ze słownika, należy określić klucz w nawiasach:</span><span class="sxs-lookup"><span data-stu-id="b9e48-400">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="b9e48-401">Wywołanie metody z parametrami</span><span class="sxs-lookup"><span data-stu-id="b9e48-401">Calling Methods with Parameters</span></span>

<span data-ttu-id="b9e48-402">Jak przedstawiono wcześniej w tym artykule obiektów, które program za pomocą mają metody.</span><span class="sxs-lookup"><span data-stu-id="b9e48-402">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="b9e48-403">Na przykład `Database` obiekt może mieć `Database.Connect` metody.</span><span class="sxs-lookup"><span data-stu-id="b9e48-403">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="b9e48-404">Wiele metod również mieć co najmniej jeden parametr.</span><span class="sxs-lookup"><span data-stu-id="b9e48-404">Many methods also have one or more parameters.</span></span> <span data-ttu-id="b9e48-405">A *parametru* jest wartością, który jest przekazywany do metody, aby włączyć metodę w celu zakończenia zadania.</span><span class="sxs-lookup"><span data-stu-id="b9e48-405">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="b9e48-406">Na przykład Przyjrzyj się deklarację dla `Request.MapPath` metody, która przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="b9e48-406">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="b9e48-407">Ta metoda zwraca ścieżkę fizyczną na serwerze, który odnosi się do określonej ścieżki wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="b9e48-407">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="b9e48-408">Są trzy parametry dla metody `virtualPath`, `baseVirtualDir`, i `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="b9e48-408">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="b9e48-409">(Zwróć uwagę, że w deklaracji, parametry są wyświetlane z typami danych, który będzie mógł zaakceptować). Po wywołaniu tej metody należy podać wartości dla wszystkich trzech parametrów.</span><span class="sxs-lookup"><span data-stu-id="b9e48-409">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="b9e48-410">Podczas korzystania z języka Visual Basic z użyciem składni Razor, masz dwie opcje do przekazywania parametrów do metody: *parametry pozycyjne* lub *nazwanych parametrów*.</span><span class="sxs-lookup"><span data-stu-id="b9e48-410">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="b9e48-411">Aby wywołać metodę, przy użyciu parametry pozycyjne, parametry są przekazywane w kolejności strict, który jest określony w deklaracji metody.</span><span class="sxs-lookup"><span data-stu-id="b9e48-411">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="b9e48-412">(Będzie zazwyczaj wiesz to zamówienie, przeczytaj dokumentację dla metody.) Należy wykonać, kolejność i nie można pominąć dowolny z parametrów &#8212; Jeśli to konieczne, możesz przekazać pusty ciąg (`""`) lub wartość null parametru pozycyjnych, które nie mają wartości.</span><span class="sxs-lookup"><span data-stu-id="b9e48-412">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="b9e48-413">W poniższym przykładzie założono, masz folder o nazwie *skrypty* w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b9e48-413">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="b9e48-414">Kod wywołuje `Request.MapPath` metody i przekazuje wartości dla trzech parametrów w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="b9e48-414">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="b9e48-415">Następnie wyświetla ścieżki wynikowej zamapowany.</span><span class="sxs-lookup"><span data-stu-id="b9e48-415">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="b9e48-416">Gdy istnieje wiele parametrów dla metody, można zachować kod bardziej przejrzyste i bardziej czytelny przy użyciu nazwanych parametrów.</span><span class="sxs-lookup"><span data-stu-id="b9e48-416">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="b9e48-417">Aby wywołać metodę, przy użyciu nazwanych parametrów, należy określić nazwę parametru, a następnie `:=` , a następnie wprowadź wartość.</span><span class="sxs-lookup"><span data-stu-id="b9e48-417">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="b9e48-418">Zaletą nazwanych parametrów jest, możesz je dodać w dowolnej kolejności, które chcesz.</span><span class="sxs-lookup"><span data-stu-id="b9e48-418">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="b9e48-419">(Wadą jest wywołanie metody nie jest najmniejszy).</span><span class="sxs-lookup"><span data-stu-id="b9e48-419">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="b9e48-420">Poniższy przykład wywołuje tę samą metodę jak powyżej, ale korzysta z nazwanych parametrów, aby podać wartości:</span><span class="sxs-lookup"><span data-stu-id="b9e48-420">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="b9e48-421">Jak widać, parametry są przekazywane w innej kolejności.</span><span class="sxs-lookup"><span data-stu-id="b9e48-421">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="b9e48-422">Jednak po uruchomieniu poprzedniego przykładu i w tym przykładzie zostanie zwrócona taką samą wartość.</span><span class="sxs-lookup"><span data-stu-id="b9e48-422">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="b9e48-423">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="b9e48-423">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="b9e48-424">Instrukcje try-Catch</span><span class="sxs-lookup"><span data-stu-id="b9e48-424">Try-Catch statements</span></span>

<span data-ttu-id="b9e48-425">Często masz instrukcji w kodzie, który może zakończyć się niepowodzeniem ze względów poza Twoją kontrolą.</span><span class="sxs-lookup"><span data-stu-id="b9e48-425">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="b9e48-426">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9e48-426">For example:</span></span>

- <span data-ttu-id="b9e48-427">Jeśli kod próbuje otworzyć, tworzenia, odczytu lub zapisu pliku, mogą wystąpić wszelkiego rodzaju błędów.</span><span class="sxs-lookup"><span data-stu-id="b9e48-427">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="b9e48-428">Żądany plik nie istnieje, może być zablokowana, kod może nie mieć uprawnień i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="b9e48-428">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="b9e48-429">Podobnie jeśli kod próbuje zaktualizować rekordy w bazie danych, może to być problemy z uprawnieniami, mogą być opuszczane połączenia z bazą danych, dane w celu zaoszczędzenia może być nieprawidłowy i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="b9e48-429">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="b9e48-430">W sposób programowania, są nazywane tych sytuacji *wyjątki*.</span><span class="sxs-lookup"><span data-stu-id="b9e48-430">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="b9e48-431">Jeśli Twój kod napotka wyjątek, generuje (zgłasza) komunikatu o błędzie oznacza to, co najlepiej irytujących dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="b9e48-431">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="b9e48-433">W sytuacjach, w którym kod może wystąpić wyjątki i w celu uniknięcia komunikaty o błędach tego typu można użyć `Try/Catch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="b9e48-433">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="b9e48-434">W `Try` instrukcji, uruchom kod, który jest sprawdzany.</span><span class="sxs-lookup"><span data-stu-id="b9e48-434">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="b9e48-435">W co najmniej jednej `Catch` instrukcji, można wyszukać konkretnego błędów (określonych typów wyjątków), które mogły wystąpić.</span><span class="sxs-lookup"><span data-stu-id="b9e48-435">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="b9e48-436">Może zawierać tyle `Catch` instrukcje, jak należy sprawdzić występowanie błędów, które są przewidywania.</span><span class="sxs-lookup"><span data-stu-id="b9e48-436">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e48-437">Firma Microsoft zaleca, unikaj używania `Response.Redirect` method in Class metoda `Try/Catch` instrukcji, ponieważ może to spowodować wyjątek na stronie.</span><span class="sxs-lookup"><span data-stu-id="b9e48-437">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="b9e48-438">Poniższy przykład pokazuje stronę, która tworzy plik tekstowy na pierwsze żądanie, a następnie wyświetla przycisk, który umożliwia użytkownikowi, otwórz plik.</span><span class="sxs-lookup"><span data-stu-id="b9e48-438">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="b9e48-439">W przykładzie użyto celowo Zła nazwa pliku, tak aby spowoduje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="b9e48-439">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="b9e48-440">Kod zawiera `Catch` instrukcji dla dwóch możliwych wyjątków: `FileNotFoundException`, który występuje, jeśli nazwa pliku jest nieprawidłowa, i `DirectoryNotFoundException`, który występuje, gdy program ASP.NET nawet nie można odnaleźć folderu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-440">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="b9e48-441">(Aby zobaczyć, jak działa, gdy wszystko będzie działać prawidłowo można komentarz do instrukcji w tym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="b9e48-441">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="b9e48-442">Jeśli Twój kod nie obsługuje wyjątek, widział takich jak poprzedniej zrzut ekranu przedstawiający stronę błędu.</span><span class="sxs-lookup"><span data-stu-id="b9e48-442">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="b9e48-443">Jednak `Try/Catch` sekcji uniemożliwia użytkownikowi widzisz błędy tych typów.</span><span class="sxs-lookup"><span data-stu-id="b9e48-443">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="b9e48-444">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b9e48-444">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="b9e48-445">Dokumentacja referencyjna</span><span class="sxs-lookup"><span data-stu-id="b9e48-445">Reference Documentation</span></span>

- [<span data-ttu-id="b9e48-446">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b9e48-446">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="b9e48-447">Język Visual Basic</span><span class="sxs-lookup"><span data-stu-id="b9e48-447">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
