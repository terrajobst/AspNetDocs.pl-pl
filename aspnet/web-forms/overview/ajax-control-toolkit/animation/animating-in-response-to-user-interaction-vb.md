---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można w formie gwiazdek...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: c38160ffa9965384cf4eae2ebda52bd62b766bba
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59396242"
---
# <a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="be7b4-104">Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (VB)</span><span class="sxs-lookup"><span data-stu-id="be7b4-104">Animating in Response To User Interaction (VB)</span></span>

<span data-ttu-id="be7b4-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="be7b4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="be7b4-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="be7b4-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="be7b4-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="be7b4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="be7b4-108">Animacji można uruchomić automatycznie lub mogą być powodowane przez interakcję użytkownika, np. przez kliknięcie myszą.</span><span class="sxs-lookup"><span data-stu-id="be7b4-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="be7b4-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="be7b4-109">Overview</span></span>

<span data-ttu-id="be7b4-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="be7b4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="be7b4-111">Animacji można uruchomić automatycznie lub mogą być powodowane przez interakcję użytkownika, np. przez kliknięcie myszą.</span><span class="sxs-lookup"><span data-stu-id="be7b4-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="be7b4-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="be7b4-112">Steps</span></span>

<span data-ttu-id="be7b4-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="be7b4-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="be7b4-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="be7b4-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="be7b4-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="be7b4-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="be7b4-116">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="be7b4-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="be7b4-117">W ramach `<Animations>` węzła, istnieje pięć sposobów rozpoczęcia animacji za pośrednictwem interakcji z użytkownikiem (Brak elementu jest `<OnLoad>` które jest wykonywane po całkowitym załadowaniu całej strony):</span><span class="sxs-lookup"><span data-stu-id="be7b4-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- `<OnClick>` <span data-ttu-id="be7b4-118">(kliknięcie myszą kontrolki)</span><span class="sxs-lookup"><span data-stu-id="be7b4-118">(mouse click on the control)</span></span>
- `<OnHoverOut>` <span data-ttu-id="be7b4-119">(kursora myszy poza formant)</span><span class="sxs-lookup"><span data-stu-id="be7b4-119">(mouse leaves the control)</span></span>
- `<OnHoverOver>` <span data-ttu-id="be7b4-120">(wskaźnika myszy nad formant zatrzymywanie `<OnHoverOut>` animacji)</span><span class="sxs-lookup"><span data-stu-id="be7b4-120">(mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- `<OnMouseOut>` <span data-ttu-id="be7b4-121">(kursora myszy poza formant)</span><span class="sxs-lookup"><span data-stu-id="be7b4-121">(mouse leaves a control)</span></span>
- `<OnMouseOver>` <span data-ttu-id="be7b4-122">(wskaźnika myszy nad formant nie zatrzymywanie `<OnMouseOut>` animacji)</span><span class="sxs-lookup"><span data-stu-id="be7b4-122">(mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="be7b4-123">W tym scenariuszu `<OnClick>` jest używany.</span><span class="sxs-lookup"><span data-stu-id="be7b4-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="be7b4-124">Gdy użytkownik kliknie na panelu, rozmiaru i stopniowo zmniejsza się w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="be7b4-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![A <span data-ttu-id="be7b4-125">Kliknięcie myszą powoduje uruchomienie animacji]</span><span class="sxs-lookup"><span data-stu-id="be7b4-125">mouse click starts the animation]</span></span>(animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

<span data-ttu-id="be7b4-126">Kliknięcie myszą powoduje uruchomienie animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="be7b4-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="be7b4-127">[Poprzednie](picking-one-animation-out-of-a-list-vb.md)
> [dalej](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="be7b4-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
