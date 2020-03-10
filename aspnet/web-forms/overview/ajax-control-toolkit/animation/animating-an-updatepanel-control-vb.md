---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animowanie kontrolki UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Dla zawartości...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d66dda923940a328c0757049c9d8bfa3b2d2b9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536338"
---
# <a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="9d555-104">Animowanie kontrolki UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="9d555-104">Animating an UpdatePanel Control (VB)</span></span>

<span data-ttu-id="9d555-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9d555-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9d555-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9d555-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="9d555-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="9d555-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9d555-108">W przypadku zawartości elementu UpdatePanel istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="9d555-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="9d555-109">W tym samouczku pokazano, jak skonfigurować takie animacje dla elementu UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="9d555-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>

## <a name="overview"></a><span data-ttu-id="9d555-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="9d555-110">Overview</span></span>

<span data-ttu-id="9d555-111">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="9d555-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9d555-112">W przypadku zawartości `UpdatePanel`istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="9d555-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="9d555-113">W tym samouczku pokazano, jak skonfigurować taką animację dla `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="9d555-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="9d555-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="9d555-114">Steps</span></span>

<span data-ttu-id="9d555-115">Pierwszym krokiem jest zwykle uwzględnienie `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:</span><span class="sxs-lookup"><span data-stu-id="9d555-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="9d555-116">Animacja w tym scenariuszu zostanie zastosowana do kontrolki sieci Web ASP.NET `Wizard` znajdującej się w `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="9d555-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="9d555-117">Trzy (dowolne) kroki zapewniają wystarczającą ilość opcji wyzwalających ogłaszanie zwrotne:</span><span class="sxs-lookup"><span data-stu-id="9d555-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="9d555-118">Znaczniki wymagane dla formantu `UpdatePanelAnimationExtender` są bardzo podobne do znaczników użytych dla `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="9d555-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="9d555-119">W `TargetControlID` atrybucie udostępniamy `ID` `UpdatePanel` do animacji; w kontrolce `UpdatePanelAnimationExtender` element `<Animations>` przechowuje znaczniki XML dla animacji.</span><span class="sxs-lookup"><span data-stu-id="9d555-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="9d555-120">Istnieje jednak jedna różnica: ilość zdarzeń i obsługi zdarzeń jest ograniczona w porównaniu do `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="9d555-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="9d555-121">W przypadku `UpdatePanels`istnieją tylko dwa z nich:</span><span class="sxs-lookup"><span data-stu-id="9d555-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="9d555-122">`<OnUpdated>`, gdy element UpdatePanel został zaktualizowany</span><span class="sxs-lookup"><span data-stu-id="9d555-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="9d555-123">`<OnUpdating>`, gdy element UpdatePanel rozpocznie aktualizowanie</span><span class="sxs-lookup"><span data-stu-id="9d555-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="9d555-124">W tym scenariuszu Nowa zawartość `UpdatePanel` (po odświeżeniu zwrotnego) zmienia się w programie.</span><span class="sxs-lookup"><span data-stu-id="9d555-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="9d555-125">Jest to wymagane oznaczenie:</span><span class="sxs-lookup"><span data-stu-id="9d555-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="9d555-126">Teraz za każdym razem, gdy w elemencie UpdatePanel zostanie przeprowadzone ogłaszanie zwrotne, Nowa zawartość panelu jest płynnie zmieniana.</span><span class="sxs-lookup"><span data-stu-id="9d555-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>

<span data-ttu-id="9d555-127">[![następnym kroku kreatora zostanie zanikanie](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9d555-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="9d555-128">Następnym krokiem kreatora jest zanikanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9d555-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d555-129">[Poprzednie](changing-an-animation-using-client-side-code-vb.md)
> [dalej](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9d555-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
