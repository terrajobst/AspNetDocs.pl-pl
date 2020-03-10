---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Wykonywanie kilku animacji w tym samym czasie (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchomienie serwera Sever...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: fe71feccbcbc4ee8e9cdc09d6220de6a53dd2d2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598120"
---
# <a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="f68e1-104">Wykonywanie kilku animacji w tym samym czasie (C#)</span><span class="sxs-lookup"><span data-stu-id="f68e1-104">Executing Several Animations at The Same Time (C#)</span></span>

<span data-ttu-id="f68e1-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f68e1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f68e1-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f68e1-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="f68e1-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="f68e1-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f68e1-108">Umożliwia równoległe uruchamianie kilku animacji.</span><span class="sxs-lookup"><span data-stu-id="f68e1-108">It allows to run several animations in a parallel fashion.</span></span>

## <a name="overview"></a><span data-ttu-id="f68e1-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="f68e1-109">Overview</span></span>

<span data-ttu-id="f68e1-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="f68e1-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f68e1-111">Umożliwia równoległe uruchamianie kilku animacji.</span><span class="sxs-lookup"><span data-stu-id="f68e1-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="f68e1-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="f68e1-112">Steps</span></span>

<span data-ttu-id="f68e1-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="f68e1-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="f68e1-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="f68e1-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="f68e1-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="f68e1-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="f68e1-116">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="f68e1-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="f68e1-117">W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="f68e1-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="f68e1-118">Ogólnie rzecz biorąc, `<OnLoad>` akceptuje tylko jedną animację.</span><span class="sxs-lookup"><span data-stu-id="f68e1-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="f68e1-119">Struktura animacji umożliwia sprzęganie kilku animacji w jeden przy użyciu elementu `<Parallel>`.</span><span class="sxs-lookup"><span data-stu-id="f68e1-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="f68e1-120">Wszystkie animacje w `<Parallel>` są wykonywane w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="f68e1-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="f68e1-121">Oto możliwe znaczniki dla kontrolki `AnimationExtender`, Znikające i zmieniające rozmiar panelu w tym samym czasie:</span><span class="sxs-lookup"><span data-stu-id="f68e1-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="f68e1-122">W rzeczywistości: po uruchomieniu tego skryptu panel zostanie wyświetlony, a następnie zmienia rozmiar (więcej niż podróżuje jego szerokość i halving jego wysokość) i stopniowo znika.</span><span class="sxs-lookup"><span data-stu-id="f68e1-122">And indeed: when you run this script, the panel is displayed, then resizes (more than tripling its width and halving its height) and fades out at the same time.</span></span>

<span data-ttu-id="f68e1-123">[![panel jest znikający i zmienia rozmiar (łącznie z jego zawartością, dzięki czemu aparat renderowania przeglądarki)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f68e1-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="f68e1-124">Panel jest odtwarzany i zmienia rozmiar (w tym jego zawartość, Dzięki aparatowi renderowania przeglądarki) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f68e1-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f68e1-125">[Poprzednie](adding-animation-to-a-control-cs.md)
> [dalej](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f68e1-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
