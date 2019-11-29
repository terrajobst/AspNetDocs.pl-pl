---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Wyzwalanie animacji w innej kontrolce (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Ogólnie uruchamianie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599638"
---
# <a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="f1225-104">Wyzwalanie animacji w innej kontrolce (C#)</span><span class="sxs-lookup"><span data-stu-id="f1225-104">Triggering an Animation in another Control (C#)</span></span>

<span data-ttu-id="f1225-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f1225-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f1225-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f1225-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="f1225-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="f1225-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f1225-108">Ogólnie uruchamianie animacji jest wyzwalane przez interakcję użytkownika z tą samą kontrolką.</span><span class="sxs-lookup"><span data-stu-id="f1225-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="f1225-109">Możliwe jest również współdziałanie z jednym formantem, a następnie przeprowadzenie animacji innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="f1225-109">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="overview"></a><span data-ttu-id="f1225-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="f1225-110">Overview</span></span>

<span data-ttu-id="f1225-111">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="f1225-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f1225-112">Ogólnie uruchamianie animacji jest wyzwalane przez interakcję użytkownika z tą samą kontrolką.</span><span class="sxs-lookup"><span data-stu-id="f1225-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="f1225-113">Możliwe jest również współdziałanie z jednym formantem, a następnie przeprowadzenie animacji innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="f1225-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="f1225-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="f1225-114">Steps</span></span>

<span data-ttu-id="f1225-115">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="f1225-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="f1225-116">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="f1225-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="f1225-117">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="f1225-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="f1225-118">Aby rozpocząć animację panelu, używany jest przycisk HTML.</span><span class="sxs-lookup"><span data-stu-id="f1225-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="f1225-119">Należy pamiętać, że `<input type="button" />` jest korzystne dla `<asp:Button />`, ponieważ nie ma potrzeby ogłaszania zwrotnego po kliknięciu tego przycisku przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f1225-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="f1225-120">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="f1225-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="f1225-121">Ważne jest, aby ustawić `TargetControlID` na identyfikator przycisku (element wyzwalający animację), a nie na identyfikator panelu (animowany element).</span><span class="sxs-lookup"><span data-stu-id="f1225-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="f1225-122">W węźle `<Animations>` Umieść animacje w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="f1225-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="f1225-123">Aby zmienić panel, a nie przycisk, należy ustawić atrybut `AnimationTarget` dla każdego elementu animacji w `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="f1225-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="f1225-124">Wartość `AnimationTarget` jest IDENTYFIKATORem panelu.</span><span class="sxs-lookup"><span data-stu-id="f1225-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="f1225-125">W ten sposób animacje są wykonywane z panelem, a nie z przyciskiem wyzwalającym.</span><span class="sxs-lookup"><span data-stu-id="f1225-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="f1225-126">Oto `AnimationExtender` znaczników w tym scenariuszu:</span><span class="sxs-lookup"><span data-stu-id="f1225-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="f1225-127">Zwróć uwagę na kolejność specjalną, w której pojawiają się pojedyncze animacje.</span><span class="sxs-lookup"><span data-stu-id="f1225-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="f1225-128">Po pierwsze, przycisk zostaje zdezaktywowany po uruchomieniu animacji.</span><span class="sxs-lookup"><span data-stu-id="f1225-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="f1225-129">Ponieważ nie ma `AnimationTarget` atrybutu w elemencie `<EnableAction>`, ta animacja jest stosowana do formantu źródłowego: przycisk.</span><span class="sxs-lookup"><span data-stu-id="f1225-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="f1225-130">Następne dwa kroki animacji są wykonywane równolegle (`<Parallel>` elementu).</span><span class="sxs-lookup"><span data-stu-id="f1225-130">The next two animation steps shall be carried out in parallel (`<Parallel>` element).</span></span> <span data-ttu-id="f1225-131">Oba mają atrybuty `AnimationTarget` ustawione na `"Panel1"`, a tym samym animowanie panelu, a nie przycisku.</span><span class="sxs-lookup"><span data-stu-id="f1225-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>

<span data-ttu-id="f1225-132">[![kliknięciu przycisku myszy spowoduje uruchomienie animacji panelu.](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f1225-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="f1225-133">Kliknięcie przycisku powoduje uruchomienie animacji panelu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f1225-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f1225-134">[Poprzednie](disabling-actions-during-animation-cs.md)
> [dalej](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f1225-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
