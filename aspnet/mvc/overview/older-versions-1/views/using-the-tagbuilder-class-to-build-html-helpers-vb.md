---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Używanie klasy TagBuilder do kompilowania pomocników HTML (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther wprowadza do przydatnej klasy narzędzi w strukturze ASP.NET MVC o nazwie klasy TagBuilder. Można użyć klasy TagBuilder, aby łatwo...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b0aa9816209cc326d3dea4b8dfb1b13cf697fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600003"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a><span data-ttu-id="70019-104">Używanie klasy TagBuilder do kompilowania pomocników HTML (VB)</span><span class="sxs-lookup"><span data-stu-id="70019-104">Using the TagBuilder Class to Build HTML Helpers (VB)</span></span>

<span data-ttu-id="70019-105">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="70019-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="70019-106">Stephen Walther wprowadza do przydatnej klasy narzędzi w strukturze ASP.NET MVC o nazwie klasy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="70019-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="70019-107">Można użyć klasy TagBuilder, aby łatwo tworzyć Tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-107">You can use the TagBuilder class to easily build HTML tags.</span></span>

<span data-ttu-id="70019-108">Struktura ASP.NET MVC zawiera użyteczną klasę narzędzi o nazwie Klasa TagBuilder, której można użyć podczas tworzenia pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="70019-109">Klasa TagBuilder, jako nazwa klasy sugeruje, pozwala łatwo tworzyć Tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="70019-110">W tym krótkim samouczku udostępniono Przegląd klasy TagBuilder i dowiesz się, jak używać tej klasy podczas kompilowania prostego pomocnika HTML, który renderuje Tagi &lt;IMG&gt; HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="70019-111">Przegląd klasy TagBuilder</span><span class="sxs-lookup"><span data-stu-id="70019-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="70019-112">Klasa TagBuilder jest zawarta w przestrzeni nazw System. Web. MVC.</span><span class="sxs-lookup"><span data-stu-id="70019-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="70019-113">Ma pięć metod:</span><span class="sxs-lookup"><span data-stu-id="70019-113">It has five methods:</span></span>

- <span data-ttu-id="70019-114">AddCssClass () — umożliwia dodanie nowego atrybutu *Class = ""* do tagu.</span><span class="sxs-lookup"><span data-stu-id="70019-114">AddCssClass() – Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="70019-115">GenerateId () — umożliwia dodanie atrybutu ID do znacznika.</span><span class="sxs-lookup"><span data-stu-id="70019-115">GenerateId() – Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="70019-116">Ta metoda automatycznie zastępuje okresy w identyfikatorze (domyślnie okresy są zamieniane na znaki podkreślenia)</span><span class="sxs-lookup"><span data-stu-id="70019-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="70019-117">Mergeattribute () — umożliwia dodanie atrybutów do tagu.</span><span class="sxs-lookup"><span data-stu-id="70019-117">MergeAttribute() – Enables you to add attributes to a tag.</span></span> <span data-ttu-id="70019-118">Istnieje wiele przeciążeń tej metody.</span><span class="sxs-lookup"><span data-stu-id="70019-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="70019-119">SetInnerText () — umożliwia ustawienie wewnętrznego tekstu znacznika.</span><span class="sxs-lookup"><span data-stu-id="70019-119">SetInnerText() – Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="70019-120">Tekst wewnętrzny jest automatycznie zakodowany w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="70019-121">ToString () — umożliwia renderowanie znacznika.</span><span class="sxs-lookup"><span data-stu-id="70019-121">ToString() – Enables you to render the tag.</span></span> <span data-ttu-id="70019-122">Możesz określić, czy chcesz utworzyć tag normalny, tag początkowy, tag końcowy lub tag samozamykający.</span><span class="sxs-lookup"><span data-stu-id="70019-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>

<span data-ttu-id="70019-123">Klasa TagBuilder ma cztery ważne właściwości:</span><span class="sxs-lookup"><span data-stu-id="70019-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="70019-124">Atrybuty — reprezentuje wszystkie atrybuty znacznika.</span><span class="sxs-lookup"><span data-stu-id="70019-124">Attributes – Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="70019-125">IdAttributeDotReplacement — reprezentuje znak używany przez metodę GenerateId () do zastępowania kropek (wartość domyślna to podkreślenie).</span><span class="sxs-lookup"><span data-stu-id="70019-125">IdAttributeDotReplacement – Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="70019-126">InnerHTML — reprezentuje wewnętrzną zawartość znacznika.</span><span class="sxs-lookup"><span data-stu-id="70019-126">InnerHTML – Represents the inner contents of the tag.</span></span> <span data-ttu-id="70019-127">Przypisanie ciągu do tej właściwości nie *powoduje* kodowania ciągu html.</span><span class="sxs-lookup"><span data-stu-id="70019-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="70019-128">TagName — reprezentuje nazwę znacznika.</span><span class="sxs-lookup"><span data-stu-id="70019-128">TagName – Represents the name of the tag.</span></span>

<span data-ttu-id="70019-129">Te metody i właściwości zawierają wszystkie podstawowe metody i właściwości, które należy wykonać, aby utworzyć tag HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="70019-130">Nie musisz naprawdę używać klasy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="70019-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="70019-131">Zamiast tego można użyć klasy StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="70019-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="70019-132">Jednak Klasa TagBuilder sprawia, że życie jest nieco prostsze.</span><span class="sxs-lookup"><span data-stu-id="70019-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="70019-133">Tworzenie pomocnika HTML obrazu</span><span class="sxs-lookup"><span data-stu-id="70019-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="70019-134">Podczas tworzenia wystąpienia klasy TagBuilder, należy przekazać nazwę tagu, który chcesz skompilować do konstruktora TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="70019-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="70019-135">Następnie można wywołać metody, takie jak AddCssClass i Mergeattribute (), aby zmodyfikować atrybuty znacznika.</span><span class="sxs-lookup"><span data-stu-id="70019-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="70019-136">Na koniec należy wywołać metodę ToString (), aby renderować tag.</span><span class="sxs-lookup"><span data-stu-id="70019-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="70019-137">Na przykład lista 1 zawiera pomocnika HTML obrazu.</span><span class="sxs-lookup"><span data-stu-id="70019-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="70019-138">Pomocnik obrazu jest implementowany wewnętrznie z TagBuilder, który reprezentuje tag&gt; HTML &lt;img.</span><span class="sxs-lookup"><span data-stu-id="70019-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="70019-139">**Lista 1 – Helpers\ImageHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="70019-139">**Listing 1 – Helpers\ImageHelper.vb**</span></span>

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

<span data-ttu-id="70019-140">Moduł na liście 1 zawiera dwie przeciążone metody o nazwie Image ().</span><span class="sxs-lookup"><span data-stu-id="70019-140">The module in Listing 1 contains two overloaded methods named Image().</span></span> <span data-ttu-id="70019-141">Po wywołaniu metody Image () można przekazać obiekt, który reprezentuje zestaw atrybutów HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="70019-142">Zwróć uwagę, jak Metoda TagBuilder. Mergeattribute () służy do dodawania pojedynczych atrybutów, takich jak atrybut src do TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="70019-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="70019-143">Należy zauważyć, że Ponadto Metoda TagBuilder. MergeAttributes () służy do dodawania kolekcji atrybutów do TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="70019-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="70019-144">Metoda MergeAttributes () akceptuje&lt;słownika,&gt; parametr obiektu.</span><span class="sxs-lookup"><span data-stu-id="70019-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="70019-145">Klasa RouteValueDictionary jest używana do konwersji obiektu reprezentującego kolekcję atrybutów do słownika&lt;ciągu,&gt;obiektu.</span><span class="sxs-lookup"><span data-stu-id="70019-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="70019-146">Po utworzeniu pomocnika obrazów można używać pomocnika w widokach ASP.NET MVC podobnie jak w przypadku innych pomocników w języku HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="70019-147">Widok w liście 2 używa pomocnika obrazu do wyświetlenia tego samego obrazu konsoli Xbox dwa razy (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="70019-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="70019-148">Pomocnik obrazu () jest wywoływana zarówno z, jak i bez kolekcji atrybutów HTML.</span><span class="sxs-lookup"><span data-stu-id="70019-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="70019-149">**Lista 2 — Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="70019-149">**Listing 2 – Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]

<span data-ttu-id="70019-150">[![okno dialogowe Nowy projekt](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="70019-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="70019-151">**Ilustracja 01**. Korzystanie z pomocnika obrazu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="70019-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span></span>

<span data-ttu-id="70019-152">Należy zauważyć, że należy zaimportować przestrzeń nazw skojarzoną z pomocnikiem obrazu w górnej części widoku index. aspx.</span><span class="sxs-lookup"><span data-stu-id="70019-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="70019-153">Pomocnik zostaje zaimportowany do następującej dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="70019-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

<span data-ttu-id="70019-154">W aplikacji Visual Basic domyślna przestrzeń nazw jest taka sama jak nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="70019-154">In a Visual Basic application, the default namespace is the same as the name of the application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="70019-155">[Poprzednie](creating-custom-html-helpers-vb.md)
> [dalej](creating-page-layouts-with-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="70019-155">[Previous](creating-custom-html-helpers-vb.md)
[Next](creating-page-layouts-with-view-master-pages-vb.md)</span></span>
