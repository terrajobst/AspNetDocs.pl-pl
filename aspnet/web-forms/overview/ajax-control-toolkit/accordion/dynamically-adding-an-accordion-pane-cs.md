---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Dynamiczne dodawanie okienka zgodnie z (C#) | Microsoft Docs
author: wenz
description: Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle zadeklarowane w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 2834f56bd77c412923f4a8f382e670727f70eae4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614465"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="e481a-104">Dynamiczne dodawanie okienka zgodnie z przepisami (C#)</span><span class="sxs-lookup"><span data-stu-id="e481a-104">Dynamically Adding An Accordion Pane (C#)</span></span>

<span data-ttu-id="e481a-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e481a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e481a-106">[Pobierz kod](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e481a-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="e481a-107">Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz.</span><span class="sxs-lookup"><span data-stu-id="e481a-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="e481a-108">Panele są zwykle deklarowane na samej stronie, ale kod po stronie serwera może być używany do osiągnięcia tego samego wyniku.</span><span class="sxs-lookup"><span data-stu-id="e481a-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="overview"></a><span data-ttu-id="e481a-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="e481a-109">Overview</span></span>

<span data-ttu-id="e481a-110">Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz.</span><span class="sxs-lookup"><span data-stu-id="e481a-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="e481a-111">Panele są zwykle deklarowane na samej stronie, ale kod po stronie serwera może być używany do osiągnięcia tego samego wyniku.</span><span class="sxs-lookup"><span data-stu-id="e481a-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="e481a-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="e481a-112">Steps</span></span>

<span data-ttu-id="e481a-113">Kontrola wystawcy uwidacznia wszystkie ważne właściwości kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="e481a-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="e481a-114">W innych przypadkach Właściwość `Panes` udziela dostępu do kolekcji okienek, które składają się na siebie zgodnie z zasadami.</span><span class="sxs-lookup"><span data-stu-id="e481a-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="e481a-115">Każde okienko jest typu `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="e481a-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="e481a-116">W związku z tym nie można utworzyć takiego okienka:</span><span class="sxs-lookup"><span data-stu-id="e481a-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="e481a-117">Właściwość `HeaderContainer` `AccordionPane` zapewnia dostęp do kontrolek ASP.NET w sekcji nagłówka okienka; Właściwość `ContentContainer` `AccordionPane` jest taka sama dla sekcji zawartość okienka.</span><span class="sxs-lookup"><span data-stu-id="e481a-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="e481a-118">Dzięki temu kod ASP.NET może dodawać zawartość do okienek:</span><span class="sxs-lookup"><span data-stu-id="e481a-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="e481a-119">Na koniec należy dodać okienka (y) do `Panes` kolekcji:</span><span class="sxs-lookup"><span data-stu-id="e481a-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="e481a-120">Oto kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki przydzielenia:</span><span class="sxs-lookup"><span data-stu-id="e481a-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="e481a-121">Jedyny brakujący element to samo samo, co zależy od obecności kontrolki `ScriptManager` ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="e481a-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="e481a-122">Aby zakończyć ten przykład, dwie klasy CSS, do których odwołuje się kontrolka, udostępniają informacje o stylu dla przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="e481a-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

<span data-ttu-id="e481a-123">[![dane w ramach traktowania zostały dynamicznie dodane przez kod po stronie serwera](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e481a-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="e481a-124">Dane w postawce zostały dynamicznie dodane przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e481a-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e481a-125">[Poprzednie](databinding-to-an-accordion-cs.md)
> [dalej](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e481a-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
