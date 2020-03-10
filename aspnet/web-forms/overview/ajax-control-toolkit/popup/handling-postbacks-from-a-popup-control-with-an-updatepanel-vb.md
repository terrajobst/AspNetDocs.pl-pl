---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Obsługa ogłaszania zwrotnego w kontrolce popup przy użyciu elementu UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Należy zwrócić szczególną uwagę...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: dd045ae56696c7944df98cf805ba812fde1bb4ff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612897"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="7460b-104">Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="7460b-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>

<span data-ttu-id="7460b-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7460b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7460b-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7460b-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="7460b-107">Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="7460b-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7460b-108">Należy zwrócić szczególną uwagę, gdy ogłaszanie zwrotne odbywa się w tym menu podręcznym.</span><span class="sxs-lookup"><span data-stu-id="7460b-108">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="overview"></a><span data-ttu-id="7460b-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7460b-109">Overview</span></span>

<span data-ttu-id="7460b-110">Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="7460b-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7460b-111">Należy zwrócić szczególną uwagę, gdy ogłaszanie zwrotne odbywa się w tym menu podręcznym.</span><span class="sxs-lookup"><span data-stu-id="7460b-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="7460b-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="7460b-112">Steps</span></span>

<span data-ttu-id="7460b-113">W przypadku korzystania z `PopupControl` z ogłaszaniem zwrotnym `UpdatePanel` może uniemożliwić odświeżenie strony spowodowane przez ogłaszanie zwrotne.</span><span class="sxs-lookup"><span data-stu-id="7460b-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="7460b-114">Następujące znaczniki definiują kilka ważnych elementów:</span><span class="sxs-lookup"><span data-stu-id="7460b-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="7460b-115">Kontrolka `ScriptManager`, tak aby zestaw narzędzi ASP.NET AJAX Control działał</span><span class="sxs-lookup"><span data-stu-id="7460b-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="7460b-116">Dwie `TextBox` kontrolki, które będą wyzwalać menu podręczne</span><span class="sxs-lookup"><span data-stu-id="7460b-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="7460b-117">Formant `Panel`, który będzie używany jako podręczny</span><span class="sxs-lookup"><span data-stu-id="7460b-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="7460b-118">W panelu formant `Calendar` jest osadzony w kontrolce `UpdatePanel`</span><span class="sxs-lookup"><span data-stu-id="7460b-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="7460b-119">Dwie `PopupControlExtender` kontrolki, które przypisują panel do pól tekstowych</span><span class="sxs-lookup"><span data-stu-id="7460b-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="7460b-120">Należy zauważyć, że atrybut `OnSelectionChanged` kontrolki `Calendar` jest ustawiony.</span><span class="sxs-lookup"><span data-stu-id="7460b-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="7460b-121">Tak więc, gdy użytkownik wybierze datę w kalendarzu, następuje ogłaszanie zwrotne i zostanie uruchomiona metoda po stronie serwera `c1_SelectionChanged()`.</span><span class="sxs-lookup"><span data-stu-id="7460b-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="7460b-122">W ramach tej metody bieżąca data musi być pobrana i zapisywana z powrotem do pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="7460b-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="7460b-123">Składnia dla programu jest następująca: najpierw należy wygenerować obiekt serwera proxy dla `PopupControlExtender` na stronie.</span><span class="sxs-lookup"><span data-stu-id="7460b-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="7460b-124">Zestaw narzędzi ASP.NET AJAX Control Toolkit oferuje metodę `GetProxyForCurrentPopup()`.</span><span class="sxs-lookup"><span data-stu-id="7460b-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="7460b-125">Obiekt zwracany przez tę metodę obsługuje metodę `Commit()`, która wysyła wartość z powrotem do kontrolki, która wyzwoliła wyskakujące okienko, a nie kontrolka, która wyzwoliła wywołanie metody!).</span><span class="sxs-lookup"><span data-stu-id="7460b-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="7460b-126">Poniższy kod przedstawia wybraną datę jako argument metody `Commit()`, powodując, że kod zapiszą wybraną datę z powrotem do pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="7460b-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="7460b-127">Teraz za każdym razem, gdy klikniesz datę kalendarzową, wybrana data zostanie wyświetlona w skojarzonym polu tekstowym i zostanie utworzona kontrolka selektora daty, która jest obecnie dostępna w wielu witrynach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7460b-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>

<span data-ttu-id="7460b-128">[![kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7460b-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="7460b-129">Kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7460b-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>

<span data-ttu-id="7460b-130">[![kliknięcie daty spowoduje jej umieszczenie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7460b-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="7460b-131">Kliknięcie daty spowoduje jej umieszczenie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7460b-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7460b-132">[Poprzednie](using-multiple-popup-controls-vb.md)
> [dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7460b-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
