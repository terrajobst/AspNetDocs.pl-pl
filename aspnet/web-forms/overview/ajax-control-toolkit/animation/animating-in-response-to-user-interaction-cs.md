---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animowanie w odpowiedzi na interakcję użytkownikaC#() | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą być gwiazdką...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: d04fa680d0cd4f7fb54521ac6fbb47a2cf9a83cf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614374"
---
# <a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="451f7-104">Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (C#)</span><span class="sxs-lookup"><span data-stu-id="451f7-104">Animating in Response To User Interaction (C#)</span></span>

<span data-ttu-id="451f7-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="451f7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="451f7-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="451f7-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="451f7-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="451f7-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="451f7-108">Animacje mogą być uruchamiane automatycznie lub mogą być wyzwalane przez interakcję z użytkownikiem, np. klikając myszą.</span><span class="sxs-lookup"><span data-stu-id="451f7-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="overview"></a><span data-ttu-id="451f7-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="451f7-109">Overview</span></span>

<span data-ttu-id="451f7-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="451f7-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="451f7-111">Animacje mogą być uruchamiane automatycznie lub mogą być wyzwalane przez interakcję z użytkownikiem, np. klikając myszą.</span><span class="sxs-lookup"><span data-stu-id="451f7-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="451f7-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="451f7-112">Steps</span></span>

<span data-ttu-id="451f7-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="451f7-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="451f7-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="451f7-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="451f7-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="451f7-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="451f7-116">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="451f7-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="451f7-117">W węźle `<Animations>` istnieje pięć sposobów uruchamiania animacji przez interakcję z użytkownikiem (brak elementu jest `<OnLoad>`, który jest wykonywany po całkowitym załadowaniu całej strony):</span><span class="sxs-lookup"><span data-stu-id="451f7-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="451f7-118">`<OnClick>` (kliknięcie kontrolki myszy)</span><span class="sxs-lookup"><span data-stu-id="451f7-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="451f7-119">`<OnHoverOut>` (mysz opuszcza formant)</span><span class="sxs-lookup"><span data-stu-id="451f7-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="451f7-120">`<OnHoverOver>` (przesuwanie myszą nad kontrolką, zatrzymywanie animacji `<OnHoverOut>`)</span><span class="sxs-lookup"><span data-stu-id="451f7-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="451f7-121">`<OnMouseOut>` (mysz opuszcza kontrolkę)</span><span class="sxs-lookup"><span data-stu-id="451f7-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="451f7-122">`<OnMouseOver>` (przesuwanie myszą nad kontrolką, nie zatrzymywanie animacji `<OnMouseOut>`)</span><span class="sxs-lookup"><span data-stu-id="451f7-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="451f7-123">W tym scenariuszu jest używana `<OnClick>`.</span><span class="sxs-lookup"><span data-stu-id="451f7-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="451f7-124">Gdy użytkownik kliknie panel, zmieniany jest rozmiar i zanika w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="451f7-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]

<span data-ttu-id="451f7-125">[![kliknięciu myszą uruchamia animację](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="451f7-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="451f7-126">Kliknięcie przycisku powoduje uruchomienie animacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="451f7-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="451f7-127">[Poprzednie](picking-one-animation-out-of-a-list-cs.md)
> [dalej](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="451f7-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
