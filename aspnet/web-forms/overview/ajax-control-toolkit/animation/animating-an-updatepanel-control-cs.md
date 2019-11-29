---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animowanie kontrolki UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Dla zawartości...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8875d750d57c5f4e362bdf461826130a881ab1d4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599935"
---
# <a name="animating-an-updatepanel-control-c"></a><span data-ttu-id="06100-104">Animowanie kontrolki UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="06100-104">Animating an UpdatePanel Control (C#)</span></span>

<span data-ttu-id="06100-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="06100-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="06100-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="06100-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span></span>

> <span data-ttu-id="06100-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="06100-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="06100-108">W przypadku zawartości elementu UpdatePanel istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="06100-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="06100-109">W tym samouczku pokazano, jak skonfigurować takie animacje dla elementu UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="06100-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>

## <a name="overview"></a><span data-ttu-id="06100-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="06100-110">Overview</span></span>

<span data-ttu-id="06100-111">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="06100-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="06100-112">W przypadku zawartości `UpdatePanel`istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="06100-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="06100-113">W tym samouczku pokazano, jak skonfigurować taką animację dla `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="06100-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="06100-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="06100-114">Steps</span></span>

<span data-ttu-id="06100-115">Pierwszym krokiem jest zwykle uwzględnienie `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:</span><span class="sxs-lookup"><span data-stu-id="06100-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

<span data-ttu-id="06100-116">Animacja w tym scenariuszu zostanie zastosowana do kontrolki sieci Web ASP.NET `Wizard` znajdującej się w `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="06100-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="06100-117">Trzy (dowolne) kroki zapewniają wystarczającą ilość opcji wyzwalających ogłaszanie zwrotne:</span><span class="sxs-lookup"><span data-stu-id="06100-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

<span data-ttu-id="06100-118">Znaczniki wymagane dla formantu `UpdatePanelAnimationExtender` są bardzo podobne do znaczników użytych dla `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="06100-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="06100-119">W `TargetControlID` atrybucie udostępniamy `ID` `UpdatePanel` do animacji; w kontrolce `UpdatePanelAnimationExtender` element `<Animations>` przechowuje znaczniki XML dla animacji.</span><span class="sxs-lookup"><span data-stu-id="06100-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="06100-120">Istnieje jednak jedna różnica: ilość zdarzeń i obsługi zdarzeń jest ograniczona w porównaniu do `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="06100-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="06100-121">W przypadku `UpdatePanels`istnieją tylko dwa z nich:</span><span class="sxs-lookup"><span data-stu-id="06100-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="06100-122">`<OnUpdated>`, gdy element UpdatePanel został zaktualizowany</span><span class="sxs-lookup"><span data-stu-id="06100-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="06100-123">`<OnUpdating>`, gdy element UpdatePanel rozpocznie aktualizowanie</span><span class="sxs-lookup"><span data-stu-id="06100-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="06100-124">W tym scenariuszu Nowa zawartość `UpdatePanel` (po odświeżeniu zwrotnego) zmienia się w programie.</span><span class="sxs-lookup"><span data-stu-id="06100-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="06100-125">Jest to wymagane oznaczenie:</span><span class="sxs-lookup"><span data-stu-id="06100-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

<span data-ttu-id="06100-126">Teraz za każdym razem, gdy w elemencie UpdatePanel zostanie przeprowadzone ogłaszanie zwrotne, Nowa zawartość panelu jest płynnie zmieniana.</span><span class="sxs-lookup"><span data-stu-id="06100-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>

<span data-ttu-id="06100-127">[![następnym kroku kreatora zostanie zanikanie](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06100-127">[![The next wizard step is fading in](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="06100-128">Następnym krokiem kreatora jest zanikanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-an-updatepanel-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="06100-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06100-129">[Poprzednie](changing-an-animation-using-client-side-code-cs.md)
> [dalej](dynamically-controlling-updatepanel-animations-cs.md)</span><span class="sxs-lookup"><span data-stu-id="06100-129">[Previous](changing-an-animation-using-client-side-code-cs.md)
[Next](dynamically-controlling-updatepanel-animations-cs.md)</span></span>
