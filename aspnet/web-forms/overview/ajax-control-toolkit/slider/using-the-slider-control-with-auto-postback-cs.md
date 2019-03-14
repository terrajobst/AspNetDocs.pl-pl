---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Używanie kontrolki Slider z automatycznym ogłaszaniem zwrotnym (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Jest możliwe automatycznie Księguj suwaka...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f776d609099c398b4937710487ba51a5efb1876
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068711"
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="9fc0e-104">Używanie kontrolki Slider z automatycznym ogłaszaniem zwrotnym (C#)</span><span class="sxs-lookup"><span data-stu-id="9fc0e-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="9fc0e-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9fc0e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9fc0e-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9fc0e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="9fc0e-107">Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="9fc0e-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="9fc0e-108">Istnieje możliwość zmiany autopostback suwaka po jego wartość.</span><span class="sxs-lookup"><span data-stu-id="9fc0e-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="9fc0e-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="9fc0e-109">Overview</span></span>

<span data-ttu-id="9fc0e-110">Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="9fc0e-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="9fc0e-111">Istnieje możliwość zmiany autopostback suwaka po jego wartość.</span><span class="sxs-lookup"><span data-stu-id="9fc0e-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="9fc0e-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="9fc0e-112">Steps</span></span>

<span data-ttu-id="9fc0e-113">Aby wprowadzić suwak automatycznie zwrotu na zmianę, obu polach tekstowych muszą atrybut `AutoPostBack="true"`: Pole tekstowe, które staną się suwak i pola tekstowego, który przechowuje położenie suwaka.</span><span class="sxs-lookup"><span data-stu-id="9fc0e-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="9fc0e-114">W tym miejscu jest wymagane w tym:</span><span class="sxs-lookup"><span data-stu-id="9fc0e-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="9fc0e-115">`SliderExtender` Formantu z ASP.NET AJAX Control Toolkit przypisuje funkcji suwaka do dwóch pól tekstowych:</span><span class="sxs-lookup"><span data-stu-id="9fc0e-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="9fc0e-116">Element label dodatkowe posłuży później poinformowania użytkownika o odświeżenie strony:</span><span class="sxs-lookup"><span data-stu-id="9fc0e-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="9fc0e-117">Na koniec `ScriptManager` kontroli ASP.NET AJAX ładuje wymagany język JavaScript dla kontrolki zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="9fc0e-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="9fc0e-118">Teraz wrócili, publikuje suwaka po stronie serwera to zdarzenie może być przechwycony i podjęto jakieś działanie:</span><span class="sxs-lookup"><span data-stu-id="9fc0e-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="9fc0e-119">[![Przesunięcie suwaka wyzwala ogłaszania zwrotnego](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9fc0e-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="9fc0e-120">Przesunięcie suwaka wyzwala ogłaszania zwrotnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9fc0e-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="9fc0e-121">[![Później Data zmiany są zapisywane w etykiecie](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9fc0e-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="9fc0e-122">Później, Data zmiany są zapisywane w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9fc0e-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9fc0e-123">Next</span><span class="sxs-lookup"><span data-stu-id="9fc0e-123">Next</span></span>](databinding-the-slider-control-cs.md)
