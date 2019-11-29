---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Wykonywanie animacji przy użyciu kodu po stronie klientaC#() | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Wykonanie animacji...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b6ba1553b9c8c51d5d6ae1679e53f9cc1d17b769
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599669"
---
# <a name="executing-animations-using-client-side-code-c"></a><span data-ttu-id="442d0-104">Wykonywanie animacji przy użyciu kodu po stronie klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="442d0-104">Executing Animations Using Client-Side Code (C#)</span></span>

<span data-ttu-id="442d0-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="442d0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="442d0-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="442d0-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span></span>

> <span data-ttu-id="442d0-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="442d0-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="442d0-108">Wykonanie animacji może być również wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="442d0-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="442d0-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="442d0-109">Overview</span></span>

<span data-ttu-id="442d0-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="442d0-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="442d0-111">Wykonanie animacji może być również wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="442d0-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="442d0-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="442d0-112">Steps</span></span>

<span data-ttu-id="442d0-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="442d0-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="442d0-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="442d0-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="442d0-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="442d0-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="442d0-116">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="442d0-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="442d0-117">W węźle `<Animations>` Użyj `<OnClick>`, aby uruchomić animacje, gdy użytkownik kliknie panel.</span><span class="sxs-lookup"><span data-stu-id="442d0-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="442d0-118">Dodaj dwie animacje do wykonania równolegle:</span><span class="sxs-lookup"><span data-stu-id="442d0-118">Add two animations to be executed in parallel:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

<span data-ttu-id="442d0-119">Ze względu na demonstrację ta animacja (i wszelkie inne animacje utworzone przy użyciu zestawu narzędzi sterowania) są wykonywane przy użyciu kodu JavaScript, gdy strona zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="442d0-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="442d0-120">Najpierw musimy uzyskać dostęp do kontrolki `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="442d0-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="442d0-121">Biblioteka ASP.NET AJAX zawiera funkcję `$find()` dla tego zadania:</span><span class="sxs-lookup"><span data-stu-id="442d0-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

<span data-ttu-id="442d0-122">Formant `AnimationExtender` uwidacznia bogaty interfejs API, w tym metody o nazwach identycznych z programami obsługi zdarzeń używanymi w znacznikach XML: `OnClick()`, `OnLoad()`i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="442d0-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="442d0-123">Na przykład wywołanie metody `OnClick()` wykonuje animację wewnątrz elementu `<OnClick>` kontrolki `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="442d0-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

<span data-ttu-id="442d0-124">Poniżej znajduje się kompletny kod JavaScript po stronie klienta, który emuluje kliknięcie panelu po całkowitym załadowaniu strony, należy zauważyć, że nazwa funkcji `pageLoad()` jest używana, która jest wywoływana przez ASP.NET AJAX po załadowaniu strony i wszystkich dołączonych bibliotek języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="442d0-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]

<span data-ttu-id="442d0-125">[![animacje są uruchamiane natychmiast, bez kliknięcia przycisku myszy](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="442d0-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="442d0-126">Animacja zostanie uruchomiona natychmiast, bez kliknięcia przycisku myszy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](executing-animations-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="442d0-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="442d0-127">[Poprzednie](modifying-animations-from-the-server-side-cs.md)
> [dalej](changing-an-animation-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="442d0-127">[Previous](modifying-animations-from-the-server-side-cs.md)
[Next](changing-an-animation-using-client-side-code-cs.md)</span></span>
