---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Rozszerzenie panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go kontrolki CollapsiblePanel w ASP.NET AJAX Control Toolkit...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 1509d25583b7e4826a4581d373bed417dacaccdf
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132890"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="49279-103">Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="49279-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>

<span data-ttu-id="49279-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="49279-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="49279-105">[Pobierz program Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="49279-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="49279-106">Kontrolka CollapsiblePanel w ASP.NET AJAX Control Toolkit rozszerza panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go ponownie.</span><span class="sxs-lookup"><span data-stu-id="49279-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="49279-107">Te dwie akcje mogą być też wywoływane z niestandardowego kodu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="49279-107">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="49279-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="49279-108">Overview</span></span>

<span data-ttu-id="49279-109">Kontrolka CollapsiblePanel w ASP.NET AJAX Control Toolkit rozszerza panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go ponownie.</span><span class="sxs-lookup"><span data-stu-id="49279-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="49279-110">Te dwie akcje mogą być też wywoływane z niestandardowego kodu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="49279-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="49279-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="49279-111">Steps</span></span>

<span data-ttu-id="49279-112">Po pierwsze, tworzenie nowej strony programu ASP.NET i zawierają `ScriptManager` w ramach jednego `<form>` elementu.</span><span class="sxs-lookup"><span data-stu-id="49279-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="49279-113">Spowoduje to załadowanie biblioteki ASP.NET AJAX, która jest wymagana przez zestaw narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="49279-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="49279-114">Następnie należy utworzyć panel zawierający tekst tak, aby były widoczne Zwiń/Rozwiń wpływu:</span><span class="sxs-lookup"><span data-stu-id="49279-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="49279-115">Jak widać, w panelu odwołuje się do klasy CSS, która jest wyświetlana w tym miejscu (zasadniczo Określa kolor tła i szerokość panelu):</span><span class="sxs-lookup"><span data-stu-id="49279-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="49279-116">`CollapsiblePanelExtender` Formant wymaga `TargetControlID` atrybutu, tak aby wie zestawu narzędzi, które panelu, aby zwinąć lub rozwinąć na żądanie:</span><span class="sxs-lookup"><span data-stu-id="49279-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="49279-117">Niestety urządzenia extender aktualnie nie ujawnia konkretnego interfejsu API dla zwijanie i rozwijanie panelu, ale niektóre metody nieudokumentowane będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="49279-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="49279-118">Po pierwsze Dodaj trzy przyciski HTML do strony, który następnie wyzwoli JavaScript po stronie klienta, aby zwinąć lub rozwinąć panelu zawartości:</span><span class="sxs-lookup"><span data-stu-id="49279-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="49279-119">W przypadku kodu JavaScript po stronie klienta (wprowadzenie `<script type="text/javascript">`), `$find()` metoda musi zostać użyte do dostępu `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="49279-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="49279-120">`$find("cpe")` Zwraca odwołanie do niej.</span><span class="sxs-lookup"><span data-stu-id="49279-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="49279-121">Z tego miejsca na określonych metod rozwiąże wykonywanego zadania.</span><span class="sxs-lookup"><span data-stu-id="49279-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="49279-122">Metoda otwierania (Rozszerzanie) nosi nazwę panelu `_doOpen()`; poniższy kod implementuje `doOpen()` funkcja wywoływana, gdy kliknięto przycisk pierwszy:</span><span class="sxs-lookup"><span data-stu-id="49279-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="49279-123">Zamykanie lub zwijanie panelu `_doClose()` metody musi być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="49279-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="49279-124">Zatem gdy użytkownik kliknie drugi przycisk, następujący kod JavaScript jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="49279-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="49279-125">Trzeci przycisk Włącza/wyłącza stan panelu: od zwinięte do rozwinięta i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="49279-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="49279-126">`CollapsiblePanelExtender` Udostępnia `toggle()` metodę, która dokładnie tak: odwraca stan panelu.</span><span class="sxs-lookup"><span data-stu-id="49279-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="49279-127">Jednak istnieje również inna metoda (który wewnętrznie jest używany przez `toggle()` metoda): `get_Collapsed()` Metody `CollapsiblePanelExtender()` informuje NAS, czy panel jest zwinięty.</span><span class="sxs-lookup"><span data-stu-id="49279-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="49279-128">W zależności od wartość zwracaną z tej funkcji jest albo rozwinięte panelu (`_doOpen()` metoda) lub zwinięte (`_doClose()`) metody:</span><span class="sxs-lookup"><span data-stu-id="49279-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

<span data-ttu-id="49279-129">[![Trzeci przycisk zmienia stan panelu: od zwinięte rozwinięte i Wstecz](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="49279-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="49279-130">Trzeci przycisk zmienia stan panelu: od zwinięte rozwinięte i Wstecz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="49279-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="49279-131">Next</span><span class="sxs-lookup"><span data-stu-id="49279-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
