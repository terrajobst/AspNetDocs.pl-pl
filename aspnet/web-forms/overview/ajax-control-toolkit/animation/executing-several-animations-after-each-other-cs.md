---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Wykonywanie kilku animacji między sobą (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchomienie serwera Sever...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: e378ac038796dbd44e89d099532b9e186dcf673f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614283"
---
# <a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="fdfbb-104">Wykonywanie kilku animacji jedna po drugiej (C#)</span><span class="sxs-lookup"><span data-stu-id="fdfbb-104">Executing Several Animations after Each Other (C#)</span></span>

<span data-ttu-id="fdfbb-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fdfbb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fdfbb-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fdfbb-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="fdfbb-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fdfbb-108">Umożliwia uruchamianie kilku animacji jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-108">It allows to run several animations one after the other.</span></span>

<span data-ttu-id="fdfbb-109">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fdfbb-110">Umożliwia uruchamianie kilku animacji jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="fdfbb-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="fdfbb-111">Steps</span></span>

<span data-ttu-id="fdfbb-112">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="fdfbb-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="fdfbb-113">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="fdfbb-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="fdfbb-114">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="fdfbb-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="fdfbb-115">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="fdfbb-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="fdfbb-116">W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="fdfbb-117">Ogólnie rzecz biorąc, `<OnLoad>` akceptuje tylko jedną animację.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="fdfbb-118">Struktura animacji umożliwia sprzęganie kilku animacji w jeden przy użyciu elementu `<Sequence>`.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="fdfbb-119">Wszystkie animacje w `<Sequence>` są wykonywane jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="fdfbb-120">Oto możliwe znaczniki dla kontrolki `AnimationExtender`, najpierw czynijąc panel, a następnie zmniejszając jego wysokość:</span><span class="sxs-lookup"><span data-stu-id="fdfbb-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="fdfbb-121">Po uruchomieniu tego skryptu panel najpierw uzyskuje szerszy rozmiar, a następnie mniejszy.</span><span class="sxs-lookup"><span data-stu-id="fdfbb-121">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="fdfbb-122">[![pierwsze zwiększenie szerokości](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fdfbb-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="fdfbb-123">Najpierw Zwiększ szerokość (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fdfbb-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>

<span data-ttu-id="fdfbb-124">[![następnie wysokość zostanie zmniejszona](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fdfbb-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="fdfbb-125">Następnie wysokość zostanie zmniejszona ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fdfbb-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fdfbb-126">[Poprzednie](executing-several-animations-at-the-same-time-cs.md)
> [dalej](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="fdfbb-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
