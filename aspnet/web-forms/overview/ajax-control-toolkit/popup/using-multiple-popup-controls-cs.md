---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Używanie wielu kontrolek Popup (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Istnieje również możliwość użycia m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d13fbfdb8d2fe66c5ff036060b9289017f79d14
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59421540"
---
# <a name="using-multiple-popup-controls-c"></a><span data-ttu-id="0d211-104">Używanie wielu kontrolek Popup (C#)</span><span class="sxs-lookup"><span data-stu-id="0d211-104">Using Multiple Popup Controls (C#)</span></span>

<span data-ttu-id="0d211-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0d211-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0d211-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0d211-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="0d211-107">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="0d211-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="0d211-108">Istnieje również możliwość użycia więcej niż jeden formant okna podręcznego na jednej stronie.</span><span class="sxs-lookup"><span data-stu-id="0d211-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="0d211-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0d211-109">Overview</span></span>

<span data-ttu-id="0d211-110">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="0d211-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="0d211-111">Istnieje również możliwość użycia więcej niż jeden formant okna podręcznego na jednej stronie.</span><span class="sxs-lookup"><span data-stu-id="0d211-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="0d211-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="0d211-112">Steps</span></span>

<span data-ttu-id="0d211-113">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="0d211-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="0d211-114">Następnie dodaj panel, który służy jako menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="0d211-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="0d211-115">W tym scenariuszu zawiera panelu `Calendar` kontroli.</span><span class="sxs-lookup"><span data-stu-id="0d211-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="0d211-116">Aby uniknąć odświeżenie strony, spowodowane ogłaszania zwrotnego kalendarza, panel jest umieszczany w ramach `UpdatePanel` sterowania:</span><span class="sxs-lookup"><span data-stu-id="0d211-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="0d211-117">Strona zawiera również dwóch pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="0d211-117">The page also contains two text boxes.</span></span> <span data-ttu-id="0d211-118">Dla każdego pola tekstowego podręcznego kalendarza są wyświetlane po aktywowaniu pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="0d211-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="0d211-119">Teraz rozszerzone każdego z dwóch pól tekstowych z `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="0d211-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="0d211-120">`TargetControlID` Atrybut zawiera identyfikator kontrolki powiązane z urządzenia extender.</span><span class="sxs-lookup"><span data-stu-id="0d211-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="0d211-121">`PopupControlID` Atrybut zawiera identyfikator panel menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="0d211-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="0d211-122">W takim przypadku zarówno rozszerzeń Pokaż panel ten sam, ale różne zespoły są również możliwe.</span><span class="sxs-lookup"><span data-stu-id="0d211-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="0d211-123">Zawsze, gdy klikniesz pozycję w polu tekstowym, kalendarz pojawi się poniżej pola, umożliwiając wybranie daty.</span><span class="sxs-lookup"><span data-stu-id="0d211-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="0d211-124">(Powrót wybranej daty w polach tekstowych zostały omówione w samouczku różnych.)</span><span class="sxs-lookup"><span data-stu-id="0d211-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


[![T<span data-ttu-id="0d211-125">jego kalendarz jest wyświetlany, gdy użytkownik klika polecenie w polu tekstowym]</span><span class="sxs-lookup"><span data-stu-id="0d211-125">he Calendar appears when the user clicks into the textbox]</span></span>(using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

<span data-ttu-id="0d211-126">Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0d211-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0d211-127">Next</span><span class="sxs-lookup"><span data-stu-id="0d211-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
