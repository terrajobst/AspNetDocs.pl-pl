---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Tworzenie numerycznej kontrolki up/down z zapleczem usługi sieci Web (VB) | Microsoft Docs
author: wenz
description: Zamiast zezwalać użytkownikowi na wpisywanie wartości w polu wyboru, numeryczna kontrolka w górę/w dół (która istnieje w systemie Windows i innych systemach operacyjnych) może okazać się bardziej c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bf6e1b27180589d39e308de62b5be1f47fa8fe2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606374"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="86dd9-103">Tworzenie kontrolki Numeric Up/Down z zapleczem usługi internetowej (VB)</span><span class="sxs-lookup"><span data-stu-id="86dd9-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>

<span data-ttu-id="86dd9-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="86dd9-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="86dd9-105">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="86dd9-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="86dd9-106">Zamiast zezwalać użytkownikowi na wpisywanie wartości w polu wyboru, kontrolka w górę/w dół (która istnieje w systemie Windows i innych systemach operacyjnych) może być bardziej wygodna.</span><span class="sxs-lookup"><span data-stu-id="86dd9-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="86dd9-107">Domyślnie formant NumericUpDown zawsze zwiększa lub zmniejsza wartość o 1, ale usługa sieci Web zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="86dd9-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="86dd9-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="86dd9-108">Overview</span></span>

<span data-ttu-id="86dd9-109">Zamiast zezwalać użytkownikowi na wpisywanie wartości w polu wyboru, kontrolka w górę/w dół (która istnieje w systemie Windows i innych systemach operacyjnych) może być bardziej wygodna.</span><span class="sxs-lookup"><span data-stu-id="86dd9-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="86dd9-110">Domyślnie formant `NumericUpDown` zawsze zwiększa lub zmniejsza wartość o 1, ale usługa sieci Web zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="86dd9-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="86dd9-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="86dd9-111">Steps</span></span>

<span data-ttu-id="86dd9-112">Zestaw narzędzi ASP.NET AJAX Control Toolkit zawiera rozszerzenie `NumericUpDown`, które automatycznie dodaje dwa przyciski do pola tekstowego: jeden w celu zwiększenia jego wartości, jeden do jego zmniejszenia.</span><span class="sxs-lookup"><span data-stu-id="86dd9-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="86dd9-113">Jednak kontrolka obsługuje również wywołanie usługi sieci Web (lub wywołanie metody strony).</span><span class="sxs-lookup"><span data-stu-id="86dd9-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="86dd9-114">Za każdym razem, gdy zostanie kliknięty przycisk w górę lub w dół, kod JavaScript nawiązuje połączenie z serwerem sieci Web i wykonuje metodę.</span><span class="sxs-lookup"><span data-stu-id="86dd9-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="86dd9-115">Podpis metody jest następujący:</span><span class="sxs-lookup"><span data-stu-id="86dd9-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="86dd9-116">`current` argument jest bieżącą wartością w polu tekstowym; atrybut `tag` to dodatkowe dane kontekstowe, które można ustawić jako właściwość rozszerzenia `NumericUpDown` (ale nie jest to wymagane).</span><span class="sxs-lookup"><span data-stu-id="86dd9-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="86dd9-117">W tym przykładzie numeryczna kontrolka up/down zezwala tylko na wartości, które są następujące: 1, 2, 4, 8, 16, 32, 64 itd.</span><span class="sxs-lookup"><span data-stu-id="86dd9-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="86dd9-118">W związku z tym Metoda wykonywana, gdy użytkownik chce zwiększyć wartość, musi być podwójnie stara wartość; Druga metoda musi dzielić wartość o dwa.</span><span class="sxs-lookup"><span data-stu-id="86dd9-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="86dd9-119">Oto kompletna usługa sieci Web:</span><span class="sxs-lookup"><span data-stu-id="86dd9-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="86dd9-120">Na koniec Utwórz nową stronę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="86dd9-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="86dd9-121">Jak zwykle potrzebna jest kontrolka `ScriptManager`, kontrolka `TextBox` i kontrolka `NumericUpDownExtender`.</span><span class="sxs-lookup"><span data-stu-id="86dd9-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="86dd9-122">Dla tej ostatniej należy podać informacje o usłudze sieci Web:</span><span class="sxs-lookup"><span data-stu-id="86dd9-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="86dd9-123">Nazwa `ServiceDownMethod` lub metoda strony sieci Web w dół</span><span class="sxs-lookup"><span data-stu-id="86dd9-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="86dd9-124">`ServiceDownPath` ścieżkę do usługi sieci Web przy użyciu metody usługi w dół. Pomiń, jeśli używasz metody strony</span><span class="sxs-lookup"><span data-stu-id="86dd9-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="86dd9-125">`ServiceUpMethod` nazwę metody sieci Web lub metody strony</span><span class="sxs-lookup"><span data-stu-id="86dd9-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="86dd9-126">`ServiceUpPath` ścieżkę do usługi sieci Web przy użyciu metody usługi up; Pomiń, jeśli używasz metody strony</span><span class="sxs-lookup"><span data-stu-id="86dd9-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="86dd9-127">Oto kompletna Adiustacja strony:</span><span class="sxs-lookup"><span data-stu-id="86dd9-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="86dd9-128">Jeśli uruchomisz stronę, Zauważ, że wartość w polu tekstowym zawsze podwaja się po kliknięciu górnego przycisku i jest wyświetlana po kliknięciu dolnego przycisku.</span><span class="sxs-lookup"><span data-stu-id="86dd9-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>

<span data-ttu-id="86dd9-129">[![wyświetlane są tylko liczby, które są potęgą 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="86dd9-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="86dd9-130">Wyświetlane są tylko liczby, które są potęgą 2 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="86dd9-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="86dd9-131">Ubiegł</span><span class="sxs-lookup"><span data-stu-id="86dd9-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
