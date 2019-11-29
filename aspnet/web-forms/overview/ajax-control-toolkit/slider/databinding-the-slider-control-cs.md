---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Powiązanie kontrolki suwakaC#() | Microsoft Docs
author: wenz
description: Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Istnieje możliwość powiązania bieżącego positio...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598577"
---
# <a name="databinding-the-slider-control-c"></a><span data-ttu-id="4d660-104">Powiązanie danych kontrolki Slider (C#)</span><span class="sxs-lookup"><span data-stu-id="4d660-104">Databinding the Slider Control (C#)</span></span>

<span data-ttu-id="4d660-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4d660-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4d660-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4d660-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="4d660-107">Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="4d660-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="4d660-108">Można powiązać bieżącą pozycję suwaka z inną kontrolką ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4d660-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="overview"></a><span data-ttu-id="4d660-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4d660-109">Overview</span></span>

<span data-ttu-id="4d660-110">Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="4d660-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="4d660-111">Można powiązać bieżącą pozycję suwaka z inną kontrolką ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4d660-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="4d660-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="4d660-112">Steps</span></span>

<span data-ttu-id="4d660-113">Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="4d660-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="4d660-114">Następnie Dodaj dwie `TextBox` kontrolki do strony.</span><span class="sxs-lookup"><span data-stu-id="4d660-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="4d660-115">Jeden z nich zostanie przekształcony w suwak graficzny, a drugi będzie pomieścić położenie suwaka.</span><span class="sxs-lookup"><span data-stu-id="4d660-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="4d660-116">Następnym krokiem jest już ostatni krok.</span><span class="sxs-lookup"><span data-stu-id="4d660-116">The next step is already the final step.</span></span> <span data-ttu-id="4d660-117">Kontrolka `SliderExtender` z zestawu narzędzi ASP.NET AJAX Control tworzy suwak z pierwszego pola tekstowego i automatycznie aktualizuje drugie pole tekstowe po zmianie położenia suwaka.</span><span class="sxs-lookup"><span data-stu-id="4d660-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="4d660-118">Aby to działało, atrybut `TargetControlID` `SliderExtender`musi być ustawiony na identyfikator pierwszego pola tekstowego; atrybut `BoundControlID` musi być ustawiony na identyfikator drugiego pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="4d660-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="4d660-119">Jak widać w przeglądarce, powiązanie danych działa w obu kierunkach: wprowadzenie nowej wartości w polu tekstowym aktualizuje pozycję suwaka.</span><span class="sxs-lookup"><span data-stu-id="4d660-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="4d660-120">Jeśli nastąpi odczytanie drugiego pola tekstowego, możesz dodać słabą ochronę do pola tekstowego, aby utrudnić Użytkownikowi ręczne zaktualizowanie wartości w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="4d660-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>

<span data-ttu-id="4d660-121">[Suwak ![i pole tekstowe są zsynchronizowane](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4d660-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="4d660-122">Suwak i pole tekstowe są zsynchronizowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4d660-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4d660-123">[Poprzednie](using-the-slider-control-with-auto-postback-cs.md)
> [dalej](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4d660-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
