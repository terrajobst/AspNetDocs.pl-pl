---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Przy użyciu kontrolki suwaka z funkcją autoogłaszania zwrotnego (C#) | Microsoft Docs
author: wenz
description: Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Możliwe jest, aby suwak był Autopost...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 785d62108667fddac42994344cde265e82aca8f4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598391"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="3318e-104">Używanie kontrolki suwaka z funkcją autoogłaszania zwrotnego (C#)</span><span class="sxs-lookup"><span data-stu-id="3318e-104">Using the Slider Control With Auto-Postback (C#)</span></span>

<span data-ttu-id="3318e-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3318e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3318e-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3318e-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="3318e-107">Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="3318e-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="3318e-108">Po zmianie wartości na suwaku jest możliwe przeprowadzenie autoogłaszania.</span><span class="sxs-lookup"><span data-stu-id="3318e-108">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="overview"></a><span data-ttu-id="3318e-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="3318e-109">Overview</span></span>

<span data-ttu-id="3318e-110">Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="3318e-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="3318e-111">Po zmianie wartości na suwaku jest możliwe przeprowadzenie autoogłaszania.</span><span class="sxs-lookup"><span data-stu-id="3318e-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="3318e-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="3318e-112">Steps</span></span>

<span data-ttu-id="3318e-113">Aby suwak automatycznie ogłaszał zwrotnie po zmianie, oba pola tekstowe muszą mieć atrybut `AutoPostBack="true"`: pole tekstowe, które zostanie przesunięte do samego suwaka, oraz pole tekstowe, które zawiera położenie suwaka.</span><span class="sxs-lookup"><span data-stu-id="3318e-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="3318e-114">Oto wymagane znaczniki dla tego:</span><span class="sxs-lookup"><span data-stu-id="3318e-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="3318e-115">Formant `SliderExtender` z zestawu narzędzi ASP.NET AJAX Control przypisuje funkcje suwaka do dwóch pól tekstowych:</span><span class="sxs-lookup"><span data-stu-id="3318e-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="3318e-116">Dodatkowy element etykiety będzie później używany do informowania użytkownika o ogłaszaniu zwrotnym:</span><span class="sxs-lookup"><span data-stu-id="3318e-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="3318e-117">Na koniec kontrola `ScriptManager` ASP.NET AJAX ładuje kod JavaScript wymagany do działania zestawu narzędzi sterowania:</span><span class="sxs-lookup"><span data-stu-id="3318e-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="3318e-118">Teraz suwak jest ogłaszany ponownie; po stronie serwera to zdarzenie może być przechwytywane i podejmowane działania:</span><span class="sxs-lookup"><span data-stu-id="3318e-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

<span data-ttu-id="3318e-119">[![Przesuwanie suwaka wyzwala ogłaszanie zwrotne](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3318e-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="3318e-120">Przesuwanie suwaka wyzwala ogłaszanie zwrotne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3318e-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>

<span data-ttu-id="3318e-121">[![później, Data tej zmiany jest zapisywana w etykiecie](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="3318e-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="3318e-122">Następnie Data tej zmiany jest zapisywana w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="3318e-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3318e-123">Next</span><span class="sxs-lookup"><span data-stu-id="3318e-123">Next</span></span>](databinding-the-slider-control-cs.md)
