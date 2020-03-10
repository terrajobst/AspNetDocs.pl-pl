---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Zmiana animacji przy użyciu kodu po stronie klienta (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacja może również...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cce0a5a901f71edd40eada59ac7eeba93222e2b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536310"
---
# <a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="89bdb-104">Zmienianie animacji przy użyciu kodu po stronie klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="89bdb-104">Changing an Animation Using Client-Side Code (VB)</span></span>

<span data-ttu-id="89bdb-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="89bdb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="89bdb-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="89bdb-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="89bdb-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="89bdb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="89bdb-108">Animację można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="89bdb-108">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="89bdb-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="89bdb-109">Overview</span></span>

<span data-ttu-id="89bdb-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="89bdb-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="89bdb-111">Animację można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="89bdb-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="89bdb-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="89bdb-112">Steps</span></span>

<span data-ttu-id="89bdb-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="89bdb-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="89bdb-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="89bdb-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="89bdb-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="89bdb-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="89bdb-116">Rzeczywista animacja jest uruchamiana przez przycisk HTML:</span><span class="sxs-lookup"><span data-stu-id="89bdb-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="89bdb-117">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="89bdb-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="89bdb-118">Należy zauważyć, że w kontrolce `AnimationExtender` nie ma węzła `<Animations>`.</span><span class="sxs-lookup"><span data-stu-id="89bdb-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="89bdb-119">Niestandardowy kod JavaScript służy do zapewnienia animacji, które mają być używane z kontrolką.</span><span class="sxs-lookup"><span data-stu-id="89bdb-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="89bdb-120">Podobnie jak w przypadku interfejsu API serwera `AnimationExtender`, nie istnieje łatwy sposób przypisywania jeszcze animacji do urządzenia Extender.</span><span class="sxs-lookup"><span data-stu-id="89bdb-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="89bdb-121">Jednak rozszerzenie udostępnia kilka metod odczytywania i zapisywania animacji zarejestrowanych w różnych zdarzeniach (`OnClick`, `OnLoad`itd.).</span><span class="sxs-lookup"><span data-stu-id="89bdb-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="89bdb-122">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="89bdb-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="89bdb-123">Format wartości zwracanej przez funkcje `get_*()` i Format argumentu dla funkcji `set_*()` jest ciągiem JSON, zapewniającym reprezentację obiektu w postaci tablicy XML.</span><span class="sxs-lookup"><span data-stu-id="89bdb-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="89bdb-124">Obecnie nie ma możliwości przekazania obiektu w, ale istnieje możliwość odczytania obiektu z danej animacji (`get_OnXXXBehavior()` metod).</span><span class="sxs-lookup"><span data-stu-id="89bdb-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="89bdb-125">Poniżej znajduje się ciąg JSON (bez cudzysłowów ograniczających i sformatowanych dobrze) reprezentujący animację wyzwalaną przez przycisk, ale animowanie panelu przez zmianę jego rozmiaru i zanikanie go w tym samym czasie:</span><span class="sxs-lookup"><span data-stu-id="89bdb-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="89bdb-126">Poniższy kod JavaScript przypisuje ten skrypt JSON do `OnClick` animacji bieżącego rozszerzenia i uruchamia go:</span><span class="sxs-lookup"><span data-stu-id="89bdb-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]

<span data-ttu-id="89bdb-127">[![animacje są uruchamiane natychmiast, bez kliknięcia przycisku myszy (i z bardzo małą adiustacją)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="89bdb-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="89bdb-128">Animacja zostanie uruchomiona natychmiast, bez kliknięcia przycisku myszy (i z bardzo małą adiustacją) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="89bdb-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="89bdb-129">[Poprzednie](executing-animations-using-client-side-code-vb.md)
> [dalej](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="89bdb-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
