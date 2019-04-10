---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Dodawanie animacji do kontrolki (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ten samouczek pokazuje, jak...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e4c6bfe1884d3e066c7b27e07e3a069943793bdd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59392290"
---
# <a name="adding-animation-to-a-control-c"></a><span data-ttu-id="0ed2e-104">Dodawanie animacji do kontrolki (C#)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-104">Adding Animation to a Control (C#)</span></span>

<span data-ttu-id="0ed2e-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0ed2e-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="0ed2e-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="0ed2e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0ed2e-108">W tym samouczku pokazano, jak skonfigurować takie animacji.</span><span class="sxs-lookup"><span data-stu-id="0ed2e-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="0ed2e-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0ed2e-109">Overview</span></span>

<span data-ttu-id="0ed2e-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="0ed2e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0ed2e-111">W tym samouczku pokazano, jak skonfigurować takie animacji.</span><span class="sxs-lookup"><span data-stu-id="0ed2e-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="0ed2e-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="0ed2e-112">Steps</span></span>

<span data-ttu-id="0ed2e-113">Pierwszym krokiem jest jak zwykle obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:</span><span class="sxs-lookup"><span data-stu-id="0ed2e-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="0ed2e-114">Animacji w tym scenariuszu zostaną zastosowane do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0ed2e-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="0ed2e-115">Skojarzona klasa CSS w panelu definiuje kolor tła i szerokości:</span><span class="sxs-lookup"><span data-stu-id="0ed2e-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="0ed2e-116">Następnie up, potrzebujemy `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="0ed2e-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="0ed2e-117">Po podaniu `ID` i zwykle `runat="server"`, `TargetControlID` atrybutu musi być równa kontrolki animacji w naszym przypadku panelu:</span><span class="sxs-lookup"><span data-stu-id="0ed2e-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="0ed2e-118">Całe zastosowania animacji deklaratywne, przy użyciu składni XML, Niestety obecnie nie są w pełni obsługiwane przez funkcję IntelliSense programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ed2e-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="0ed2e-119">Węzeł główny jest `<Animations>;` w tym węźle kilka zdarzeń są dozwolone, które określają, kiedy animation(s) zakończeniu miejsce:</span><span class="sxs-lookup"><span data-stu-id="0ed2e-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- `OnClick` <span data-ttu-id="0ed2e-120">(kliknięcie myszą)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-120">(mouse click)</span></span>
- `OnHoverOut` <span data-ttu-id="0ed2e-121">(Jeśli kursora myszy poza formant)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-121">(when the mouse leaves a control)</span></span>
- `OnHoverOver` <span data-ttu-id="0ed2e-122">(gdy wskaźnika myszy nad formant, zatrzymanie `OnHoverOut` animacji)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-122">(when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- `OnLoad` <span data-ttu-id="0ed2e-123">(po stronie został załadowany)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-123">(when the page has been loaded)</span></span>
- `OnMouseOut` <span data-ttu-id="0ed2e-124">(Jeśli kursora myszy poza formant)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-124">(when the mouse leaves a control)</span></span>
- `OnMouseOver` <span data-ttu-id="0ed2e-125">(gdy wskaźnika myszy nad formant, nie zatrzymywanie `OnMouseOut` animacji)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-125">(when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="0ed2e-126">Struktura zawiera zestaw animacji, każdy z nich reprezentowany przez własną — element XML.</span><span class="sxs-lookup"><span data-stu-id="0ed2e-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="0ed2e-127">Oto zaznaczenia:</span><span class="sxs-lookup"><span data-stu-id="0ed2e-127">Here is a selection:</span></span>

- `<Color>` <span data-ttu-id="0ed2e-128">(zmiana koloru)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-128">(changing a color)</span></span>
- `<FadeIn>` <span data-ttu-id="0ed2e-129">(rozjaśnianie)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-129">(fading in)</span></span>
- `<FadeOut>` <span data-ttu-id="0ed2e-130">(wygaszanie)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-130">(fading out)</span></span>
- `<Property>` <span data-ttu-id="0ed2e-131">(zmiana właściwości kontrolki)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-131">(changing a control's property)</span></span>
- `<Pulse>` <span data-ttu-id="0ed2e-132">(pulsating)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-132">(pulsating)</span></span>
- `<Resize>` <span data-ttu-id="0ed2e-133">(zmiana rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-133">(changing the size)</span></span>
- `<Scale>` <span data-ttu-id="0ed2e-134">(proporcjonalnie zmiany rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="0ed2e-134">(proportionally changing the size)</span></span>

<span data-ttu-id="0ed2e-135">W tym przykładzie panelu są zanikanie. Animacja podejmują 1,5 s (`Duration` atrybut), wyświetlanie 24 ramek (kroki animację) na sekundę (`Fps` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="0ed2e-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="0ed2e-136">Oto kompletny kod znaczników dla `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="0ed2e-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="0ed2e-137">Po uruchomieniu tego skryptu, panel jest wyświetlany i stopniowo zmniejsza się w ciągu półtora sekund.</span><span class="sxs-lookup"><span data-stu-id="0ed2e-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


[![T<span data-ttu-id="0ed2e-138">wygaszanie panelu HE]</span><span class="sxs-lookup"><span data-stu-id="0ed2e-138">he panel is fading out]</span></span>(adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

<span data-ttu-id="0ed2e-139">Panel jest wygaszanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0ed2e-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0ed2e-140">Next</span><span class="sxs-lookup"><span data-stu-id="0ed2e-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
