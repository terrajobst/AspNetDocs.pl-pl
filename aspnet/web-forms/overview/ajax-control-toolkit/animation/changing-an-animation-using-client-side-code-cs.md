---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Zmiana animacji przy użyciu kodu po stronie klienta (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacja może również...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 84fc2d6646b89cfabb2193cdfca59462d6d7ef16
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606981"
---
# <a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="406bc-104">Zmienianie animacji przy użyciu kodu po stronie klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="406bc-104">Changing an Animation Using Client-Side Code (C#)</span></span>

<span data-ttu-id="406bc-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="406bc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="406bc-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="406bc-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="406bc-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="406bc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="406bc-108">Animację można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="406bc-108">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="406bc-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="406bc-109">Overview</span></span>

<span data-ttu-id="406bc-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="406bc-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="406bc-111">Animację można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="406bc-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="406bc-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="406bc-112">Steps</span></span>

<span data-ttu-id="406bc-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="406bc-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="406bc-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="406bc-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="406bc-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="406bc-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="406bc-116">Rzeczywista animacja jest uruchamiana przez przycisk HTML:</span><span class="sxs-lookup"><span data-stu-id="406bc-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="406bc-117">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="406bc-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="406bc-118">Należy zauważyć, że w kontrolce `AnimationExtender` nie ma węzła `<Animations>`.</span><span class="sxs-lookup"><span data-stu-id="406bc-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="406bc-119">Niestandardowy kod JavaScript służy do zapewnienia animacji, które mają być używane z kontrolką.</span><span class="sxs-lookup"><span data-stu-id="406bc-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="406bc-120">Podobnie jak w przypadku interfejsu API serwera `AnimationExtender`, nie istnieje łatwy sposób przypisywania jeszcze animacji do urządzenia Extender.</span><span class="sxs-lookup"><span data-stu-id="406bc-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="406bc-121">Jednak rozszerzenie udostępnia kilka metod odczytywania i zapisywania animacji zarejestrowanych w różnych zdarzeniach (`OnClick`, `OnLoad`itd.).</span><span class="sxs-lookup"><span data-stu-id="406bc-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="406bc-122">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="406bc-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="406bc-123">Format wartości zwracanej przez funkcje `get_*()` i Format argumentu dla funkcji `set_*()` jest ciągiem JSON, zapewniającym reprezentację obiektu w postaci tablicy XML.</span><span class="sxs-lookup"><span data-stu-id="406bc-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="406bc-124">Obecnie nie ma możliwości przekazania obiektu w, ale istnieje możliwość odczytania obiektu z danej animacji (`get_OnXXXBehavior()` metod).</span><span class="sxs-lookup"><span data-stu-id="406bc-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="406bc-125">Poniżej znajduje się ciąg JSON (bez cudzysłowów ograniczających i sformatowanych dobrze) reprezentujący animację wyzwalaną przez przycisk, ale animowanie panelu przez zmianę jego rozmiaru i zanikanie go w tym samym czasie:</span><span class="sxs-lookup"><span data-stu-id="406bc-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="406bc-126">Poniższy kod JavaScript przypisuje ten skrypt JSON do `OnClick` animacji bieżącego rozszerzenia i uruchamia go:</span><span class="sxs-lookup"><span data-stu-id="406bc-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

<span data-ttu-id="406bc-127">[![animacje są uruchamiane natychmiast, bez kliknięcia przycisku myszy (i z bardzo małą adiustacją)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="406bc-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="406bc-128">Animacja zostanie uruchomiona natychmiast, bez kliknięcia przycisku myszy (i z bardzo małą adiustacją) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="406bc-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="406bc-129">[Poprzednie](executing-animations-using-client-side-code-cs.md)
> [dalej](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="406bc-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
