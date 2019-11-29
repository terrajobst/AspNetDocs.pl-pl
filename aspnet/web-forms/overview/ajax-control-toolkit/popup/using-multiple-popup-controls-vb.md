---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Używanie wielu kontrolek popup (VB) | Microsoft Docs
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Istnieje również możliwość użycia m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611603"
---
# <a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="7265d-104">Używanie wielu kontrolek Popup (VB)</span><span class="sxs-lookup"><span data-stu-id="7265d-104">Using Multiple Popup Controls (VB)</span></span>

<span data-ttu-id="7265d-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7265d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7265d-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7265d-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="7265d-107">Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="7265d-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7265d-108">Istnieje również możliwość użycia więcej niż jednej kontrolki popup na jednej stronie.</span><span class="sxs-lookup"><span data-stu-id="7265d-108">It is also possible to use more than one popup control on one page.</span></span>

## <a name="overview"></a><span data-ttu-id="7265d-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7265d-109">Overview</span></span>

<span data-ttu-id="7265d-110">Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="7265d-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7265d-111">Istnieje również możliwość użycia więcej niż jednej kontrolki popup na jednej stronie.</span><span class="sxs-lookup"><span data-stu-id="7265d-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="7265d-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="7265d-112">Steps</span></span>

<span data-ttu-id="7265d-113">Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="7265d-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="7265d-114">Następnie Dodaj panel, który służy jako okno podręczne.</span><span class="sxs-lookup"><span data-stu-id="7265d-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="7265d-115">W bieżącym scenariuszu panel zawiera kontrolkę `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="7265d-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="7265d-116">Aby uniknąć odświeżenia stron spowodowanych przez ogłaszanie zwrotne w kalendarzu, panel jest umieszczany w kontrolce `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="7265d-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="7265d-117">Strona zawiera również dwa pola tekstowe.</span><span class="sxs-lookup"><span data-stu-id="7265d-117">The page also contains two text boxes.</span></span> <span data-ttu-id="7265d-118">Dla każdego pola tekstowego okno podręczne zostanie wyświetlone po aktywowaniu pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="7265d-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="7265d-119">Teraz możesz rozłożyć wszystkie dwa pola tekstowe z `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="7265d-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="7265d-120">Atrybut `TargetControlID` zawiera identyfikator formantu powiązanego z przedłużaczem.</span><span class="sxs-lookup"><span data-stu-id="7265d-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="7265d-121">Atrybut `PopupControlID` zawiera identyfikator panelu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="7265d-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="7265d-122">W takim przypadku obydwa elementy wykraczają na ten sam panel, ale również są dostępne różne panele.</span><span class="sxs-lookup"><span data-stu-id="7265d-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="7265d-123">Teraz za każdym razem, gdy klikniesz wewnątrz pola tekstowego, Kalendarz zostanie wyświetlony poniżej pola, co umożliwi wybranie daty.</span><span class="sxs-lookup"><span data-stu-id="7265d-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="7265d-124">(Pobranie wybranej daty z powrotem do pól tekstowych zostanie omówione w innym samouczku).</span><span class="sxs-lookup"><span data-stu-id="7265d-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>

<span data-ttu-id="7265d-125">[![kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7265d-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="7265d-126">Kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7265d-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7265d-127">[Poprzednie](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [dalej](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7265d-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
