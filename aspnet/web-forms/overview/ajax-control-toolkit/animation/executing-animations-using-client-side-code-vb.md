---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Wykonywanie animacji przy użyciu kodu po stronie klienta (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Wykonanie animacji...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598162"
---
# <a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="6e3cd-104">Wykonywanie animacji przy użyciu kodu po stronie klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="6e3cd-104">Executing Animations Using Client-Side Code (VB)</span></span>

<span data-ttu-id="6e3cd-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6e3cd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6e3cd-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6e3cd-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="6e3cd-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6e3cd-108">Wykonanie animacji może być również wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="6e3cd-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6e3cd-109">Overview</span></span>

<span data-ttu-id="6e3cd-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6e3cd-111">Wykonanie animacji może być również wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="6e3cd-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="6e3cd-112">Steps</span></span>

<span data-ttu-id="6e3cd-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="6e3cd-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="6e3cd-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="6e3cd-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="6e3cd-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="6e3cd-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="6e3cd-116">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="6e3cd-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="6e3cd-117">W węźle `<Animations>` Użyj `<OnClick>`, aby uruchomić animacje, gdy użytkownik kliknie panel.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="6e3cd-118">Dodaj dwie animacje do wykonania równolegle:</span><span class="sxs-lookup"><span data-stu-id="6e3cd-118">Add two animations to be executed in parallel:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="6e3cd-119">Ze względu na demonstrację ta animacja (i wszelkie inne animacje utworzone przy użyciu zestawu narzędzi sterowania) są wykonywane przy użyciu kodu JavaScript, gdy strona zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="6e3cd-120">Najpierw musimy uzyskać dostęp do kontrolki `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="6e3cd-121">Biblioteka ASP.NET AJAX zawiera funkcję `$find()` dla tego zadania:</span><span class="sxs-lookup"><span data-stu-id="6e3cd-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="6e3cd-122">Formant `AnimationExtender` uwidacznia bogaty interfejs API, w tym metody o nazwach identycznych z programami obsługi zdarzeń używanymi w znacznikach XML: `OnClick()`, `OnLoad()`i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="6e3cd-123">Na przykład wywołanie metody `OnClick()` wykonuje animację wewnątrz elementu `<OnClick>` kontrolki `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="6e3cd-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="6e3cd-124">Poniżej znajduje się kompletny kod JavaScript po stronie klienta, który emuluje kliknięcie panelu po całkowitym załadowaniu strony, należy zauważyć, że nazwa funkcji `pageLoad()` jest używana, która jest wywoływana przez ASP.NET AJAX po załadowaniu strony i wszystkich dołączonych bibliotek języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6e3cd-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

<span data-ttu-id="6e3cd-125">[![animacje są uruchamiane natychmiast, bez kliknięcia przycisku myszy](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6e3cd-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="6e3cd-126">Animacja zostanie uruchomiona natychmiast, bez kliknięcia przycisku myszy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6e3cd-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6e3cd-127">[Poprzednie](modifying-animations-from-the-server-side-vb.md)
> [dalej](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6e3cd-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
