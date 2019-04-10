---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Szczególną uwagę należy zwrócić...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: dff813f0f3e4da26a32fd6305e476d24484e0e7c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59415092"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="5aefd-104">Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="5aefd-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>

<span data-ttu-id="5aefd-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5aefd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5aefd-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5aefd-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="5aefd-107">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="5aefd-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="5aefd-108">Szczególną uwagę należy podjąć w przypadku ogłaszania zwrotnego w obrębie okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5aefd-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="5aefd-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5aefd-109">Overview</span></span>

<span data-ttu-id="5aefd-110">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="5aefd-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="5aefd-111">Szczególną uwagę należy podjąć w przypadku ogłaszania zwrotnego w obrębie okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5aefd-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="5aefd-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="5aefd-112">Steps</span></span>

<span data-ttu-id="5aefd-113">Korzystając z `PopupControl` z zwrotu `UpdatePanel` może uniemożliwić odświeżania strony spowodowane zwrotu.</span><span class="sxs-lookup"><span data-stu-id="5aefd-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="5aefd-114">Następujące znaczniki definiuje kilka ważnych elementów:</span><span class="sxs-lookup"><span data-stu-id="5aefd-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="5aefd-115">A `ScriptManager` kontrolki ASP.NET AJAX Control Toolkit działa</span><span class="sxs-lookup"><span data-stu-id="5aefd-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="5aefd-116">Dwa `TextBox` formanty, które wyzwolą oba okna podręcznego</span><span class="sxs-lookup"><span data-stu-id="5aefd-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="5aefd-117">A `Panel` formant, który będzie służyć jako menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5aefd-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="5aefd-118">W panelu `Calendar` kontroli jest osadzony w ramach `UpdatePanel` kontroli</span><span class="sxs-lookup"><span data-stu-id="5aefd-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="5aefd-119">Dwa `PopupControlExtender` mechanizmy przypisać panelu do pól tekstowych</span><span class="sxs-lookup"><span data-stu-id="5aefd-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="5aefd-120">Należy pamiętać, że `OnSelectionChanged` atrybutu `Calendar` kontrolki jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="5aefd-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="5aefd-121">Tak, gdy użytkownik wybierze datę w kalendarzu, występuje odświeżenie strony, a także metoda po stronie serwera `c1_SelectionChanged()` jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="5aefd-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="5aefd-122">W ramach tej metody należy pobrać i zapisywane z powrotem do pola tekstowego bieżącą datę.</span><span class="sxs-lookup"><span data-stu-id="5aefd-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="5aefd-123">Składnia, jest następujący: Po pierwsze, serwer proxy dla obiektu `PopupControlExtender` na stronie musi zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="5aefd-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="5aefd-124">ASP.NET AJAX Control Toolkit oferuje `GetProxyForCurrentPopup()` metody.</span><span class="sxs-lookup"><span data-stu-id="5aefd-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="5aefd-125">Obiekt, Metoda ta zwraca obsługuje `Commit()` metodę, która wysyła wartość do formantu, który wyzwolił menu podręczne (nie formant który wyzwolił wywołanie metody!).</span><span class="sxs-lookup"><span data-stu-id="5aefd-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="5aefd-126">Poniższy kod zawiera wybranej daty jako argument dla `Commit()` metody, powodując kod w celu zapisania wybranej daty z powrotem do pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="5aefd-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="5aefd-127">Teraz po każdym kliknięciu datę w kalendarzu wybrana data pojawia się w polu tekstowym skojarzone tworzenie kontrolki selektora daty, które aktualnie znajdują się na wiele witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5aefd-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


[![T<span data-ttu-id="5aefd-128">jego kalendarz jest wyświetlany, gdy użytkownik klika polecenie w polu tekstowym]</span><span class="sxs-lookup"><span data-stu-id="5aefd-128">he Calendar appears when the user clicks into the textbox]</span></span>(handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

<span data-ttu-id="5aefd-129">Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5aefd-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


[![C<span data-ttu-id="5aefd-130">licking w dniu umieszcza go w polu tekstowym]</span><span class="sxs-lookup"><span data-stu-id="5aefd-130">licking on a date puts it in the textbox]</span></span>(handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

<span data-ttu-id="5aefd-131">Klikając pozycję na wartość typu date umieszcza go w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5aefd-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5aefd-132">[Poprzednie](using-multiple-popup-controls-cs.md)
> [dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5aefd-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
