---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamiczne dodawanie okienka zgodnie z opisem (VB) | Microsoft Docs
author: wenz
description: Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle zadeklarowane w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598323"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="024e1-104">Dynamiczne dodawanie okienka zgodnie z przepisami (VB)</span><span class="sxs-lookup"><span data-stu-id="024e1-104">Dynamically Adding An Accordion Pane (VB)</span></span>

<span data-ttu-id="024e1-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="024e1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="024e1-106">[Pobierz kod](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="024e1-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="024e1-107">Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz.</span><span class="sxs-lookup"><span data-stu-id="024e1-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="024e1-108">Panele są zwykle deklarowane na samej stronie, ale kod po stronie serwera może być używany do osiągnięcia tego samego wyniku.</span><span class="sxs-lookup"><span data-stu-id="024e1-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="overview"></a><span data-ttu-id="024e1-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="024e1-109">Overview</span></span>

<span data-ttu-id="024e1-110">Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz.</span><span class="sxs-lookup"><span data-stu-id="024e1-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="024e1-111">Panele są zwykle deklarowane na samej stronie, ale kod po stronie serwera może być używany do osiągnięcia tego samego wyniku.</span><span class="sxs-lookup"><span data-stu-id="024e1-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="024e1-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="024e1-112">Steps</span></span>

<span data-ttu-id="024e1-113">Kontrola wystawcy uwidacznia wszystkie ważne właściwości kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="024e1-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="024e1-114">W innych przypadkach Właściwość `Panes` udziela dostępu do kolekcji okienek, które składają się na siebie zgodnie z zasadami.</span><span class="sxs-lookup"><span data-stu-id="024e1-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="024e1-115">Każde okienko jest typu `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="024e1-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="024e1-116">W związku z tym nie można utworzyć takiego okienka:</span><span class="sxs-lookup"><span data-stu-id="024e1-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="024e1-117">Właściwość `HeaderContainer` `AccordionPane` zapewnia dostęp do kontrolek ASP.NET w sekcji nagłówka okienka; Właściwość `ContentContainer` `AccordionPane` jest taka sama dla sekcji zawartość okienka.</span><span class="sxs-lookup"><span data-stu-id="024e1-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="024e1-118">Dzięki temu kod ASP.NET może dodawać zawartość do okienek:</span><span class="sxs-lookup"><span data-stu-id="024e1-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="024e1-119">Na koniec należy dodać okienka (y) do `Panes` kolekcji:</span><span class="sxs-lookup"><span data-stu-id="024e1-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="024e1-120">Oto kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki przydzielenia:</span><span class="sxs-lookup"><span data-stu-id="024e1-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="024e1-121">Jedyny brakujący element to samo samo, co zależy od obecności kontrolki `ScriptManager` ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="024e1-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="024e1-122">Aby zakończyć ten przykład, dwie klasy CSS, do których odwołuje się kontrolka, udostępniają informacje o stylu dla przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="024e1-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

<span data-ttu-id="024e1-123">[![dane w ramach traktowania zostały dynamicznie dodane przez kod po stronie serwera](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="024e1-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="024e1-124">Dane w postawce zostały dynamicznie dodane przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="024e1-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="024e1-125">Wstecz</span><span class="sxs-lookup"><span data-stu-id="024e1-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
