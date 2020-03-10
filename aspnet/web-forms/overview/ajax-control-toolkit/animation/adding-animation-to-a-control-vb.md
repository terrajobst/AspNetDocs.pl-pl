---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Dodawanie animacji do kontrolki (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W tym samouczku pokazano, jak...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598302"
---
# <a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="30927-104">Dodawanie animacji do kontrolki (VB)</span><span class="sxs-lookup"><span data-stu-id="30927-104">Adding Animation to a Control (VB)</span></span>

<span data-ttu-id="30927-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="30927-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="30927-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="30927-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="30927-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="30927-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="30927-108">W tym samouczku pokazano, jak skonfigurować taką animację.</span><span class="sxs-lookup"><span data-stu-id="30927-108">This tutorial shows how to set up such an animation.</span></span>

## <a name="overview"></a><span data-ttu-id="30927-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="30927-109">Overview</span></span>

<span data-ttu-id="30927-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="30927-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="30927-111">W tym samouczku pokazano, jak skonfigurować taką animację.</span><span class="sxs-lookup"><span data-stu-id="30927-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="30927-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="30927-112">Steps</span></span>

<span data-ttu-id="30927-113">Pierwszym krokiem jest zwykle uwzględnienie `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:</span><span class="sxs-lookup"><span data-stu-id="30927-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="30927-114">Animacja w tym scenariuszu zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="30927-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="30927-115">Skojarzona Klasa CSS panelu definiuje kolor tła i Szerokość:</span><span class="sxs-lookup"><span data-stu-id="30927-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="30927-116">Następnie potrzebujemy `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="30927-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="30927-117">Po podania `ID` i zwykłych `runat="server"`, atrybut `TargetControlID` musi być ustawiony na kontrolkę do animacji w naszym przypadku, panel:</span><span class="sxs-lookup"><span data-stu-id="30927-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="30927-118">Cała animacja jest stosowana deklaratywnie, przy użyciu składni XML, niestety obecnie nie jest w pełni obsługiwana przez funkcję IntelliSense programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30927-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="30927-119">Węzeł główny jest `<Animations>;` w tym węźle, ale jest dozwolonych kilka zdarzeń, które określają, kiedy wykonane są następujące animacje:</span><span class="sxs-lookup"><span data-stu-id="30927-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="30927-120">`OnClick` (kliknięcie myszą)</span><span class="sxs-lookup"><span data-stu-id="30927-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="30927-121">`OnHoverOut` (gdy mysz opuści formant)</span><span class="sxs-lookup"><span data-stu-id="30927-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="30927-122">`OnHoverOver` (gdy wskaźnik myszy jest przesuwany nad kontrolką, zatrzymywanie animacji `OnHoverOut`)</span><span class="sxs-lookup"><span data-stu-id="30927-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="30927-123">`OnLoad` (gdy strona została załadowana)</span><span class="sxs-lookup"><span data-stu-id="30927-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="30927-124">`OnMouseOut` (gdy mysz opuści formant)</span><span class="sxs-lookup"><span data-stu-id="30927-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="30927-125">`OnMouseOver` (gdy wskaźnik myszy znajduje się nad kontrolką, nie zatrzyma animacji `OnMouseOut`)</span><span class="sxs-lookup"><span data-stu-id="30927-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="30927-126">Struktura zawiera zestaw animacji, z których każdy jest reprezentowany przez własny element XML.</span><span class="sxs-lookup"><span data-stu-id="30927-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="30927-127">Oto wybór:</span><span class="sxs-lookup"><span data-stu-id="30927-127">Here is a selection:</span></span>

- <span data-ttu-id="30927-128">`<Color>` (zmiana koloru)</span><span class="sxs-lookup"><span data-stu-id="30927-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="30927-129">`<FadeIn>` (Zanikanie w)</span><span class="sxs-lookup"><span data-stu-id="30927-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="30927-130">`<FadeOut>` (Zanikanie)</span><span class="sxs-lookup"><span data-stu-id="30927-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="30927-131">`<Property>` (zmiana właściwości kontrolki)</span><span class="sxs-lookup"><span data-stu-id="30927-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="30927-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="30927-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="30927-133">`<Resize>` (zmiana rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="30927-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="30927-134">`<Scale>` (proporcjonalnie do zmiany rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="30927-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="30927-135">W tym przykładzie panel stopniowo rozjaśnia się. Animacja przyjmuje 1,5 sekund (`Duration` atrybutu), wyświetlając 24 klatki (kroki animacji) na sekundę (atrybut`Fps`).</span><span class="sxs-lookup"><span data-stu-id="30927-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="30927-136">Oto kompletny znacznik dla kontrolki `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="30927-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="30927-137">Po uruchomieniu tego skryptu panel zostanie wyświetlony i zanika w ciągu jednej i pół sekundy.</span><span class="sxs-lookup"><span data-stu-id="30927-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>

<span data-ttu-id="30927-138">[![panel jest znikający](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="30927-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="30927-139">Panel powoduje zanikanie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="30927-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="30927-140">[Poprzednie](dynamically-controlling-updatepanel-animations-cs.md)
> [dalej](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="30927-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
