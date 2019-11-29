---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Zwijanie i rozszerzanie panelu z JavaScript (C#) | Microsoft Docs
author: wenz
description: Kontrolka CollapsiblePanel w zestawie narzędzi ASP.NET AJAX Control rozszerza panel i udostępnia ją z możliwością zwijania jej zawartości i rozszerzania jej...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: bed14d82394d28336493bec10e31ddb4d411a192
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599433"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="9a765-103">Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="9a765-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>

<span data-ttu-id="9a765-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9a765-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9a765-105">[Pobierz kod](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9a765-105">[Download Code](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="9a765-106">Kontrolka CollapsiblePanel w zestawie narzędzi ASP.NET AJAX Control rozszerza panel i udostępnia ją z możliwością zwijania jej zawartości i jej ponownego rozwinięcia.</span><span class="sxs-lookup"><span data-stu-id="9a765-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="9a765-107">Te dwie akcje mogą być również wywoływane z niestandardowego kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9a765-107">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="9a765-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="9a765-108">Overview</span></span>

<span data-ttu-id="9a765-109">Kontrolka CollapsiblePanel w zestawie narzędzi ASP.NET AJAX Control rozszerza panel i udostępnia ją z możliwością zwijania jej zawartości i jej ponownego rozwinięcia.</span><span class="sxs-lookup"><span data-stu-id="9a765-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="9a765-110">Te dwie akcje mogą być również wywoływane z niestandardowego kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9a765-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="9a765-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="9a765-111">Steps</span></span>

<span data-ttu-id="9a765-112">Najpierw utwórz nową stronę ASP.NET i Uwzględnij `ScriptManager` w obrębie jednego `<form>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9a765-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="9a765-113">Spowoduje to załadowanie biblioteki AJAX ASP.NET, która jest wymagana przez zestaw narzędzi sterowania:</span><span class="sxs-lookup"><span data-stu-id="9a765-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="9a765-114">Następnie utwórz panel z tekstem, tak aby można było zobaczyć efekt zwijania/rozwijania:</span><span class="sxs-lookup"><span data-stu-id="9a765-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="9a765-115">Jak widać, panel odwołuje się do klasy CSS, która jest wyświetlana w tym miejscu (i w istocie definiuje kolor tła i szerokość panelu):</span><span class="sxs-lookup"><span data-stu-id="9a765-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="9a765-116">Kontrolka `CollapsiblePanelExtender` wymaga atrybutu `TargetControlID`, dzięki czemu zestaw narzędzi wie, który panel ma zostać zwinięty lub rozwinięty na żądanie:</span><span class="sxs-lookup"><span data-stu-id="9a765-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="9a765-117">Niestety, rozszerzenie aktualnie nie uwidacznia określonego interfejsu API do zwijania lub rozszerzania panelu, ale niektóre nieudokumentowane metody.</span><span class="sxs-lookup"><span data-stu-id="9a765-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="9a765-118">Po pierwsze, Dodaj do strony trzy przyciski HTML, które będą wyzwalać kod JavaScript po stronie klienta, aby zwinąć lub rozwinąć zawartość panelu:</span><span class="sxs-lookup"><span data-stu-id="9a765-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="9a765-119">W kodzie JavaScript po stronie klienta (rozpoczętym z `<script type="text/javascript">`) Metoda `$find()` musi być używana w celu uzyskania dostępu do `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="9a765-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="9a765-120">`$find("cpe")` zwróci odwołanie do niego.</span><span class="sxs-lookup"><span data-stu-id="9a765-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="9a765-121">Z tego miejsca poszczególne metody rozwiązują zadanie.</span><span class="sxs-lookup"><span data-stu-id="9a765-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="9a765-122">Metoda otwierająca (rozszerzająca) Panel jest nazywana `_doOpen()`; Poniższy kod implementuje funkcję `doOpen()` wywołana po kliknięciu pierwszego przycisku:</span><span class="sxs-lookup"><span data-stu-id="9a765-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="9a765-123">Aby zamknąć lub zwinąć panel, należy wykonać metodę `_doClose()`.</span><span class="sxs-lookup"><span data-stu-id="9a765-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="9a765-124">Gdy użytkownik kliknie drugi przycisk, zostanie wywołany następujący kod JavaScript:</span><span class="sxs-lookup"><span data-stu-id="9a765-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="9a765-125">Trzeci przycisk przełącza stan panelu: od zwinięte do rozwinięte i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="9a765-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="9a765-126">`CollapsiblePanelExtender` uwidacznia metodę `toggle()`, która dokładnie to oznacza, że: odwraca stan panelu.</span><span class="sxs-lookup"><span data-stu-id="9a765-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="9a765-127">Istnieje również inne podejście (używane wewnętrznie przez metodę `toggle()`): Metoda `get_Collapsed()` `CollapsiblePanelExtender()` informuje nas o tym, czy panel jest zwinięty, czy nie.</span><span class="sxs-lookup"><span data-stu-id="9a765-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="9a765-128">W zależności od wartości zwracanej przez tę funkcję panel jest następnie rozwinięty (`_doOpen()` metodzie) lub zwinięty (`_doClose()`):</span><span class="sxs-lookup"><span data-stu-id="9a765-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

<span data-ttu-id="9a765-129">[![trzeci przycisk zmienia stan panelu: od zwinięte do rozwinięte i z powrotem](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9a765-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="9a765-130">Trzeci przycisk zmienia stan panelu: od zwiniętej do rozwiniętej i z powrotem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9a765-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9a765-131">Next</span><span class="sxs-lookup"><span data-stu-id="9a765-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
