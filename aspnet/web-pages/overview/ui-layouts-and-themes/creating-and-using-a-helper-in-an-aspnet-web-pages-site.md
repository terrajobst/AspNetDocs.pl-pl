---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Tworzenie i używanie pomocnika w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano sposób tworzenia pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). Pomocnik to składnik wielokrotnego użytku, który zawiera kod i znaczniki do wydajności...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563512"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="f13fb-104">Tworzenie i używanie pomocnika w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="f13fb-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="f13fb-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f13fb-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f13fb-106">W tym artykule opisano sposób tworzenia pomocnika w witrynie internetowej ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="f13fb-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="f13fb-107">*Pomocnik* to składnik wielokrotnego użytku, który zawiera kod i znaczniki umożliwiające wykonanie zadania, które może być żmudnym lub złożone.</span><span class="sxs-lookup"><span data-stu-id="f13fb-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="f13fb-108">**Dowiesz się:**</span><span class="sxs-lookup"><span data-stu-id="f13fb-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="f13fb-109">Jak utworzyć i używać prostego pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f13fb-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="f13fb-110">Są to funkcje ASP.NET wprowadzone w artykule:</span><span class="sxs-lookup"><span data-stu-id="f13fb-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="f13fb-111">Składnia `@helper`.</span><span class="sxs-lookup"><span data-stu-id="f13fb-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f13fb-112">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="f13fb-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f13fb-113">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f13fb-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f13fb-114">Ten samouczek działa również z ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="f13fb-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="f13fb-115">Przegląd pomocników</span><span class="sxs-lookup"><span data-stu-id="f13fb-115">Overview of Helpers</span></span>

<span data-ttu-id="f13fb-116">Jeśli musisz wykonać te same zadania na różnych stronach w witrynie, możesz użyć pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f13fb-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="f13fb-117">Strony sieci Web ASP.NET zawierają kilka pomocników i istnieje wiele więcej, które można pobrać i zainstalować.</span><span class="sxs-lookup"><span data-stu-id="f13fb-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="f13fb-118">(Lista wbudowanych pomocników na stronach sieci Web ASP.NET jest wymieniona w [szybkim Kompendium interfejsu API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907)). Jeśli żaden z istniejących pomocników nie spełnia Twoich potrzeb, możesz utworzyć własnego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f13fb-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="f13fb-119">Pomocnik umożliwia użycie typowego bloku kodu na wielu stronach.</span><span class="sxs-lookup"><span data-stu-id="f13fb-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="f13fb-120">Załóżmy, że na stronie często chcesz utworzyć element notatki, który jest ustawiany poza normalnymi akapitami.</span><span class="sxs-lookup"><span data-stu-id="f13fb-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="f13fb-121">Być może notatka jest tworzona jako element `<div>`, który jest stylem jako pole z obramowaniem.</span><span class="sxs-lookup"><span data-stu-id="f13fb-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="f13fb-122">Zamiast dodawać te same znaczniki do strony za każdym razem, gdy chcesz wyświetlić notatkę, możesz spakować znacznik jako pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f13fb-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="f13fb-123">Następnie możesz wstawić notatkę z jednym wierszem kodu wszędzie tam, gdzie jest to potrzebne.</span><span class="sxs-lookup"><span data-stu-id="f13fb-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="f13fb-124">Użycie pomocnika takiego jak sprawia, że kod na każdej stronie jest prostszy i łatwiejszy do odczytania.</span><span class="sxs-lookup"><span data-stu-id="f13fb-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="f13fb-125">Ułatwia ona również obsługę witryny, ponieważ w przypadku konieczności zmiany wyglądu notatek można zmienić adiustację w jednym miejscu.</span><span class="sxs-lookup"><span data-stu-id="f13fb-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="f13fb-126">Tworzenie pomocnika</span><span class="sxs-lookup"><span data-stu-id="f13fb-126">Creating a Helper</span></span>

<span data-ttu-id="f13fb-127">Ta procedura pokazuje, jak utworzyć pomocnika, który tworzy notatkę, tak jak to opisano.</span><span class="sxs-lookup"><span data-stu-id="f13fb-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="f13fb-128">Jest to prosty przykład, ale pomocnik niestandardowy może zawierać dowolny kod znacznika i ASP.NET, którego potrzebujesz.</span><span class="sxs-lookup"><span data-stu-id="f13fb-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="f13fb-129">W folderze głównym witryny sieci Web Utwórz folder o nazwie *App\_Code*.</span><span class="sxs-lookup"><span data-stu-id="f13fb-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="f13fb-130">Jest to zarezerwowana nazwa folderu w ASP.NET, w którym można umieścić kod dla składników takich jak pomocniki.</span><span class="sxs-lookup"><span data-stu-id="f13fb-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="f13fb-131">W folderze *\_aplikacji* Utwórz nowy plik *. cshtml* i nazwij go *pomocnik. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f13fb-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="f13fb-132">Zastąp istniejącą zawartość następującym:</span><span class="sxs-lookup"><span data-stu-id="f13fb-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="f13fb-133">Kod używa składni `@helper` do deklarowania nowego pomocnika o nazwie `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="f13fb-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="f13fb-134">Ten konkretny pomocnik umożliwia przekazywanie parametru o nazwie `content`, który może zawierać kombinację tekstu i znaczników.</span><span class="sxs-lookup"><span data-stu-id="f13fb-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="f13fb-135">Pomocnik wstawia ciąg do treści notatki przy użyciu zmiennej `@content`.</span><span class="sxs-lookup"><span data-stu-id="f13fb-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="f13fb-136">Zwróć uwagę, że plik ma nazwę *pomocnicy. cshtml*, ale pomocnik ma nazwę `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="f13fb-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="f13fb-137">Można umieścić wiele pomocników niestandardowych w pojedynczym pliku.</span><span class="sxs-lookup"><span data-stu-id="f13fb-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="f13fb-138">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="f13fb-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="f13fb-139">Korzystanie z pomocnika na stronie</span><span class="sxs-lookup"><span data-stu-id="f13fb-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="f13fb-140">W folderze głównym Utwórz nowy pusty plik o nazwie *TestHelper. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f13fb-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="f13fb-141">Dodaj następujący kod do pliku:</span><span class="sxs-lookup"><span data-stu-id="f13fb-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="f13fb-142">Aby wywołać utworzoną przez Ciebie pomocnika, użyj `@`, a następnie nazwę pliku, gdzie pomocnika to, kropka, a następnie nazwę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f13fb-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="f13fb-143">(Jeśli w folderze *\_aplikacji* znajduje się wiele folderów, można użyć składni `@FolderName.FileName.HelperName` do wywołania pomocnika na dowolnym zagnieżdżonym poziomie folderów).</span><span class="sxs-lookup"><span data-stu-id="f13fb-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="f13fb-144">Tekst dodany w cudzysłowie w nawiasach jest tekstem, który pomocnik będzie wyświetlał jako część notatki na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f13fb-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="f13fb-145">Zapisz stronę i uruchom ją w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f13fb-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="f13fb-146">Pomocnik generuje element notatki z prawej strony, gdzie wywołano pomocnika: między dwa akapity.</span><span class="sxs-lookup"><span data-stu-id="f13fb-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Zrzut ekranu przedstawiający stronę w przeglądarce oraz sposób wygenerowania znaczników przez pomocnika, które umieszczają pole wokół określonego tekstu.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="f13fb-148">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="f13fb-148">Additional Resources</span></span>

<span data-ttu-id="f13fb-149">[Menu poziome jako pomocnik Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="f13fb-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="f13fb-150">Ten wpis w blogu według Jan Pope pokazuje, jak utworzyć poziome menu jako pomocnika przy użyciu znaczników, CSS i kodu.</span><span class="sxs-lookup"><span data-stu-id="f13fb-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="f13fb-151">Korzystanie [z HTML5 w ASP.NET stronach sieci Web pomocników dla WebMatrix i ASP.NET MvC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="f13fb-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="f13fb-152">Ten wpis w blogu Abraham sam pokazuje pomocnika, który renderuje element HTML5 `Canvas`.</span><span class="sxs-lookup"><span data-stu-id="f13fb-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="f13fb-153">[Różnica między @Helpers i @Functions w programie WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="f13fb-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="f13fb-154">Ten wpis w blogu według Jan solance zawiera opis składni `@helper` i składni `@function` i kiedy należy używać każdego z nich.</span><span class="sxs-lookup"><span data-stu-id="f13fb-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
