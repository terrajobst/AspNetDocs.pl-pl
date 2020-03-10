---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Używanie klasy TagBuilder do kompilowania pomocników HTML (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther wprowadza do przydatnej klasy narzędzi w strukturze ASP.NET MVC o nazwie klasy TagBuilder. Można użyć klasy TagBuilder, aby łatwo...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: c8eaea9932a30c744b9a69861619ce9458b5a23a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600031"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="e045c-104">Używanie klasy TagBuilder do kompilowania pomocników HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="e045c-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>

<span data-ttu-id="e045c-105">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e045c-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e045c-106">Stephen Walther wprowadza do przydatnej klasy narzędzi w strukturze ASP.NET MVC o nazwie klasy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="e045c-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="e045c-107">Można użyć klasy TagBuilder, aby łatwo tworzyć Tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-107">You can use the TagBuilder class to easily build HTML tags.</span></span>

<span data-ttu-id="e045c-108">Struktura ASP.NET MVC zawiera użyteczną klasę narzędzi o nazwie Klasa TagBuilder, której można użyć podczas tworzenia pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="e045c-109">Klasa TagBuilder, jako nazwa klasy sugeruje, pozwala łatwo tworzyć Tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="e045c-110">W tym krótkim samouczku udostępniono Przegląd klasy TagBuilder i dowiesz się, jak używać tej klasy podczas kompilowania prostego pomocnika HTML, który renderuje Tagi &lt;IMG&gt; HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="e045c-111">Przegląd klasy TagBuilder</span><span class="sxs-lookup"><span data-stu-id="e045c-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="e045c-112">Klasa TagBuilder jest zawarta w przestrzeni nazw System. Web. MVC.</span><span class="sxs-lookup"><span data-stu-id="e045c-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="e045c-113">Ma pięć metod:</span><span class="sxs-lookup"><span data-stu-id="e045c-113">It has five methods:</span></span>

- <span data-ttu-id="e045c-114">AddCssClass () — umożliwia dodanie nowego atrybutu *Class = ""* do tagu.</span><span class="sxs-lookup"><span data-stu-id="e045c-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="e045c-115">GenerateId () — umożliwia dodanie atrybutu ID do znacznika.</span><span class="sxs-lookup"><span data-stu-id="e045c-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="e045c-116">Ta metoda automatycznie zastępuje okresy w identyfikatorze (domyślnie okresy są zamieniane na znaki podkreślenia)</span><span class="sxs-lookup"><span data-stu-id="e045c-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="e045c-117">Mergeattribute () — umożliwia dodanie atrybutów do tagu.</span><span class="sxs-lookup"><span data-stu-id="e045c-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="e045c-118">Istnieje wiele przeciążeń tej metody.</span><span class="sxs-lookup"><span data-stu-id="e045c-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="e045c-119">SetInnerText () — umożliwia ustawienie wewnętrznego tekstu znacznika.</span><span class="sxs-lookup"><span data-stu-id="e045c-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="e045c-120">Tekst wewnętrzny jest automatycznie zakodowany w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="e045c-121">ToString () — umożliwia renderowanie znacznika.</span><span class="sxs-lookup"><span data-stu-id="e045c-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="e045c-122">Możesz określić, czy chcesz utworzyć tag normalny, tag początkowy, tag końcowy lub tag samozamykający.</span><span class="sxs-lookup"><span data-stu-id="e045c-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>

<span data-ttu-id="e045c-123">Klasa TagBuilder ma cztery ważne właściwości:</span><span class="sxs-lookup"><span data-stu-id="e045c-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="e045c-124">Atrybuty — reprezentuje wszystkie atrybuty znacznika.</span><span class="sxs-lookup"><span data-stu-id="e045c-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="e045c-125">IdAttributeDotReplacement — reprezentuje znak używany przez metodę GenerateId () do zastępowania kropek (wartość domyślna to podkreślenie).</span><span class="sxs-lookup"><span data-stu-id="e045c-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="e045c-126">InnerHTML — reprezentuje wewnętrzną zawartość znacznika.</span><span class="sxs-lookup"><span data-stu-id="e045c-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="e045c-127">Przypisanie ciągu do tej właściwości nie *powoduje* kodowania ciągu html.</span><span class="sxs-lookup"><span data-stu-id="e045c-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="e045c-128">TagName-reprezentuje nazwę znacznika.</span><span class="sxs-lookup"><span data-stu-id="e045c-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="e045c-129">Te metody i właściwości zawierają wszystkie podstawowe metody i właściwości, które należy wykonać, aby utworzyć tag HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="e045c-130">Nie musisz naprawdę używać klasy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="e045c-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="e045c-131">Zamiast tego można użyć klasy StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="e045c-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="e045c-132">Jednak Klasa TagBuilder sprawia, że życie jest nieco prostsze.</span><span class="sxs-lookup"><span data-stu-id="e045c-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="e045c-133">Tworzenie pomocnika HTML obrazu</span><span class="sxs-lookup"><span data-stu-id="e045c-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="e045c-134">Podczas tworzenia wystąpienia klasy TagBuilder, należy przekazać nazwę tagu, który chcesz skompilować do konstruktora TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="e045c-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="e045c-135">Następnie można wywołać metody, takie jak AddCssClass i Mergeattribute (), aby zmodyfikować atrybuty znacznika.</span><span class="sxs-lookup"><span data-stu-id="e045c-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="e045c-136">Na koniec należy wywołać metodę ToString (), aby renderować tag.</span><span class="sxs-lookup"><span data-stu-id="e045c-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="e045c-137">Na przykład lista 1 zawiera pomocnika HTML obrazu.</span><span class="sxs-lookup"><span data-stu-id="e045c-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="e045c-138">Pomocnik obrazu jest implementowany wewnętrznie z TagBuilder, który reprezentuje tag&gt; HTML &lt;img.</span><span class="sxs-lookup"><span data-stu-id="e045c-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="e045c-139">**Lista 1 — Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="e045c-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="e045c-140">Klasa na liście 1 zawiera dwie statyczne metody przeciążone o nazwie Image.</span><span class="sxs-lookup"><span data-stu-id="e045c-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="e045c-141">Po wywołaniu metody Image () można przekazać obiekt, który reprezentuje zestaw atrybutów HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="e045c-142">Zwróć uwagę, jak Metoda TagBuilder. Mergeattribute () służy do dodawania pojedynczych atrybutów, takich jak atrybut src do TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="e045c-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="e045c-143">Należy zauważyć, że Ponadto Metoda TagBuilder. MergeAttributes () służy do dodawania kolekcji atrybutów do TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="e045c-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="e045c-144">Metoda MergeAttributes () akceptuje&lt;słownika,&gt; parametr obiektu.</span><span class="sxs-lookup"><span data-stu-id="e045c-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="e045c-145">Klasa RouteValueDictionary jest używana do konwersji obiektu reprezentującego kolekcję atrybutów do słownika&lt;ciągu,&gt;obiektu.</span><span class="sxs-lookup"><span data-stu-id="e045c-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="e045c-146">Po utworzeniu pomocnika obrazów można używać pomocnika w widokach ASP.NET MVC podobnie jak w przypadku innych pomocników w języku HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="e045c-147">Widok w liście 2 używa pomocnika obrazu do wyświetlenia tego samego obrazu konsoli Xbox dwa razy (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="e045c-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="e045c-148">Pomocnik obrazu () jest wywoływana zarówno z, jak i bez kolekcji atrybutów HTML.</span><span class="sxs-lookup"><span data-stu-id="e045c-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="e045c-149">**Lista 2 — Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="e045c-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]

<span data-ttu-id="e045c-150">[![okno dialogowe Nowy projekt](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e045c-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="e045c-151">**Ilustracja 01**. Korzystanie z pomocnika obrazu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e045c-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>

<span data-ttu-id="e045c-152">Należy zauważyć, że należy zaimportować przestrzeń nazw skojarzoną z pomocnikiem obrazu w górnej części widoku index. aspx.</span><span class="sxs-lookup"><span data-stu-id="e045c-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="e045c-153">Pomocnik zostaje zaimportowany do następującej dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="e045c-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> <span data-ttu-id="e045c-154">[Poprzednie](creating-custom-html-helpers-cs.md)
> [dalej](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e045c-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
