---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animacja w zależności od warunku (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Czy animacja jest...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607010"
---
# <a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="ce02c-104">Animacja w zależności od warunku (VB)</span><span class="sxs-lookup"><span data-stu-id="ce02c-104">Animation Depending On a Condition (VB)</span></span>

<span data-ttu-id="ce02c-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ce02c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ce02c-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ce02c-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="ce02c-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ce02c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce02c-108">Czy animacja jest uruchamiana, czy nie może również zależeć od warunku w postaci niektórych kodów JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ce02c-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="ce02c-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ce02c-109">Overview</span></span>

<span data-ttu-id="ce02c-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ce02c-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce02c-111">Czy animacja jest uruchamiana, czy nie może również zależeć od warunku w postaci niektórych kodów JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ce02c-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="ce02c-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="ce02c-112">Steps</span></span>

<span data-ttu-id="ce02c-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="ce02c-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="ce02c-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="ce02c-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="ce02c-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="ce02c-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="ce02c-116">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="ce02c-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="ce02c-117">W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="ce02c-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="ce02c-118">Zamiast jednego ze zwykłych animacji, element `<Condition>` jest dostępny do odtworzenia.</span><span class="sxs-lookup"><span data-stu-id="ce02c-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="ce02c-119">Kod JavaScript podany jako wartość atrybutu `ConditionScript` jest wykonywany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ce02c-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="ce02c-120">Jeśli wartość jest równa true, animacja jest wykonywana, w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="ce02c-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="ce02c-121">Poniższe znaczniki zawierają dwie animacje, z których każdy jest wykonywany w 50% przypadków losowo.</span><span class="sxs-lookup"><span data-stu-id="ce02c-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="ce02c-122">Ponieważ w `<OnLoad>`może istnieć tylko jedna animacja, dwa `<Condition>` animacje są sprzężone ze sobą przy użyciu elementu `<Sequence>`:</span><span class="sxs-lookup"><span data-stu-id="ce02c-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="ce02c-123">Należy zauważyć, że znak mniejszości (`<`) w atrybucie `ConditionScript` musi mieć wartość ucieczki ().</span><span class="sxs-lookup"><span data-stu-id="ce02c-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="ce02c-124">Po uruchomieniu tego skryptu nie są wykonywane żadne animacje ani jeden z dwóch elementów lub.</span><span class="sxs-lookup"><span data-stu-id="ce02c-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>

<span data-ttu-id="ce02c-125">[![panel jest znikający bez zmiany rozmiarów, więc drugie uruchomienie animacji spowoduje, że pierwszy z nich nie](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce02c-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="ce02c-126">Panel jest odtwarzany bez zmiany rozmiaru, więc drugie uruchomienie animacji, pierwsze, nie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ce02c-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ce02c-127">[Poprzednie](executing-several-animations-after-each-other-vb.md)
> [dalej](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ce02c-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
