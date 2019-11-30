---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Dostosowywanie indeksu Z DropShadow (C#) | Microsoft Docs
author: wenz
description: Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Jednak ten cień czasami powoduje konflikt z innymi kontrolkami, dla insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574301"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="bdf92-104">Dostosowywanie indeksu Z kontrolki DropShadow (C#)</span><span class="sxs-lookup"><span data-stu-id="bdf92-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>

<span data-ttu-id="bdf92-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bdf92-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bdf92-106">[Pobierz kod](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bdf92-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="bdf92-107">Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem.</span><span class="sxs-lookup"><span data-stu-id="bdf92-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="bdf92-108">Jednak ten cień czasami powoduje konflikt z innymi kontrolkami, na przykład formant menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bdf92-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="bdf92-109">Gdy pojawi się pozycja menu, zostanie wyświetlona w tle.</span><span class="sxs-lookup"><span data-stu-id="bdf92-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="overview"></a><span data-ttu-id="bdf92-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="bdf92-110">Overview</span></span>

<span data-ttu-id="bdf92-111">Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem.</span><span class="sxs-lookup"><span data-stu-id="bdf92-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="bdf92-112">Jednak ten cień czasami powoduje konflikt z innymi kontrolkami, na przykład formant menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bdf92-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="bdf92-113">Gdy pojawi się pozycja menu, zostanie wyświetlona w tle.</span><span class="sxs-lookup"><span data-stu-id="bdf92-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="bdf92-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="bdf92-114">Steps</span></span>

<span data-ttu-id="bdf92-115">Kod rozpoczyna się od samego panelu, zawierający wystarczająco dużo tekstu, aby panel zawiera wystarczającą ilość tekstu, aby efekt był widoczny:</span><span class="sxs-lookup"><span data-stu-id="bdf92-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="bdf92-116">Inny panel jest umieszczany bezpośrednio przed panelem `panelShadow`.</span><span class="sxs-lookup"><span data-stu-id="bdf92-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="bdf92-117">Zawiera menu z orientacją poziomą, tak aby wpisy menu były widoczne (lub zamiast: w obszarze) panelu `dropShadow`):</span><span class="sxs-lookup"><span data-stu-id="bdf92-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="bdf92-118">Następnie `DropShadowExtender` zostanie dodany w celu rozwinięcia panelu `panelShadow` z efektem cienia:</span><span class="sxs-lookup"><span data-stu-id="bdf92-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="bdf92-119">Na koniec formant `ScriptManager` AJAX ASP.NET umożliwia działanie zestawu narzędzi sterowania:</span><span class="sxs-lookup"><span data-stu-id="bdf92-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="bdf92-120">Po uruchomieniu tego skryptu pozycje menu są wyświetlane poniżej panelu.</span><span class="sxs-lookup"><span data-stu-id="bdf92-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="bdf92-121">Jednak menu używa klasy CSS `panel`, gdzie wystarczy zdefiniować dwie rzeczy, aby elementy były wyświetlane przed drugim panelem:</span><span class="sxs-lookup"><span data-stu-id="bdf92-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="bdf92-122">Pozycjonowanie względne</span><span class="sxs-lookup"><span data-stu-id="bdf92-122">Relative positioning</span></span>
- <span data-ttu-id="bdf92-123">Pozytywny indeks z</span><span class="sxs-lookup"><span data-stu-id="bdf92-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="bdf92-124">Następnie formant `DropShadowExtender` nie powoduje konfliktu z kontrolką menu.</span><span class="sxs-lookup"><span data-stu-id="bdf92-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>

<span data-ttu-id="bdf92-125">[![przed: element menu nie jest widoczny](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bdf92-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="bdf92-126">Przed: element menu nie jest widoczny ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bdf92-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>

<span data-ttu-id="bdf92-127">[![po: pojawia się pozycja menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="bdf92-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="bdf92-128">Po: pojawia się pozycja menu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="bdf92-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bdf92-129">Next</span><span class="sxs-lookup"><span data-stu-id="bdf92-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
