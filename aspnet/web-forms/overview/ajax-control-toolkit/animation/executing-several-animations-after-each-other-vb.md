---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Wykonywanie kilku animacji między sobą (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchomienie serwera Sever...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 5034fe905299d4559b713dd841cb166886179c39
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598134"
---
# <a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="2cf97-104">Wykonywanie kilku animacji jedna po drugiej (VB)</span><span class="sxs-lookup"><span data-stu-id="2cf97-104">Executing Several Animations after Each Other (VB)</span></span>

<span data-ttu-id="2cf97-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2cf97-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2cf97-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2cf97-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="2cf97-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="2cf97-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2cf97-108">Umożliwia uruchamianie kilku animacji jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="2cf97-108">It allows to run several animations one after the other.</span></span>

## <a name="overview"></a><span data-ttu-id="2cf97-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2cf97-109">Overview</span></span>

<span data-ttu-id="2cf97-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="2cf97-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2cf97-111">Umożliwia uruchamianie kilku animacji jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="2cf97-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="2cf97-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="2cf97-112">Steps</span></span>

<span data-ttu-id="2cf97-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="2cf97-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="2cf97-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="2cf97-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="2cf97-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="2cf97-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="2cf97-116">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="2cf97-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="2cf97-117">W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="2cf97-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="2cf97-118">Ogólnie rzecz biorąc, `<OnLoad>` akceptuje tylko jedną animację.</span><span class="sxs-lookup"><span data-stu-id="2cf97-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="2cf97-119">Struktura animacji umożliwia sprzęganie kilku animacji w jeden przy użyciu elementu `<Sequence>`.</span><span class="sxs-lookup"><span data-stu-id="2cf97-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="2cf97-120">Wszystkie animacje w `<Sequence>` są wykonywane jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="2cf97-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="2cf97-121">Oto możliwe znaczniki dla kontrolki `AnimationExtender`, najpierw czynijąc panel, a następnie zmniejszając jego wysokość:</span><span class="sxs-lookup"><span data-stu-id="2cf97-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="2cf97-122">Po uruchomieniu tego skryptu panel najpierw uzyskuje szerszy rozmiar, a następnie mniejszy.</span><span class="sxs-lookup"><span data-stu-id="2cf97-122">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="2cf97-123">[![pierwsze zwiększenie szerokości](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2cf97-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="2cf97-124">Najpierw Zwiększ szerokość (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2cf97-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>

<span data-ttu-id="2cf97-125">[![następnie wysokość zostanie zmniejszona](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2cf97-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="2cf97-126">Następnie wysokość zostanie zmniejszona ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2cf97-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2cf97-127">[Poprzednie](executing-several-animations-at-the-same-time-vb.md)
> [dalej](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2cf97-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
