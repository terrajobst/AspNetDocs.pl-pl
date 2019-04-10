---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Jeśli odświeżenie strony występuje w su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c46f00c42f9b06d0224bcae03f51be8a73099974
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59409346"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="ddcb5-104">Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="ddcb5-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>

<span data-ttu-id="ddcb5-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ddcb5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ddcb5-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ddcb5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="ddcb5-107">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="ddcb5-108">Podczas ogłaszania zwrotnego odbywa się w taki zespół wiąże się z kilku panelach na stronie trudno jest określić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="ddcb5-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ddcb5-109">Overview</span></span>

<span data-ttu-id="ddcb5-110">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="ddcb5-111">Podczas ogłaszania zwrotnego odbywa się w taki zespół wiąże się z kilku panelach na stronie trudno jest określić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="ddcb5-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="ddcb5-112">Steps</span></span>

<span data-ttu-id="ddcb5-113">Korzystając z `PopupControl` odświeżenie strony, ale bez konieczności `UpdatePanel` na stronie Toolkit formantu nie oferuje możliwość określenia, który element klienta wyzwolił okna podręcznego, który z kolei spowodował zwrotu.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="ddcb5-114">Jednak małych lewy dostarcza obejście dla tego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="ddcb5-115">Po pierwsze, w tym miejscu jest konfiguracja podstawowa: dwóch pól tekstowych, które wyzwolić tego samego okna podręcznego kalendarza.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="ddcb5-116">Dwa `PopupControlExtenders` łączą ze sobą pola tekstowe i menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="ddcb5-117">Podstawowa koncepcja jest dodanie ukryte pole formularza w &lt; `form` &gt; element, który posiada pola tekstowego, która została uruchomiona, okno podręczne:</span><span class="sxs-lookup"><span data-stu-id="ddcb5-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="ddcb5-118">Po załadowaniu strony kodu w języku JavaScript dodaje procedurę obsługi zdarzeń do obu polach tekstowych: Zawsze, gdy użytkownik kliknie pole tekstowe, jego nazwa zostanie zapisane ukryte pole formularza:</span><span class="sxs-lookup"><span data-stu-id="ddcb5-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="ddcb5-119">W kodzie po stronie serwera można odczytać wartości pola ukrytego.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="ddcb5-120">Ponieważ ukryte pola są proste do manipulowania, lista dozwolonych sposobem sprawdzenia ukryte wartości jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="ddcb5-121">Po zidentyfikowaniu pole tekstowe poprawną datę z kalendarza są zapisywane do niego.</span><span class="sxs-lookup"><span data-stu-id="ddcb5-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![T<span data-ttu-id="ddcb5-122">jego kalendarz jest wyświetlany, gdy użytkownik klika polecenie w polu tekstowym]</span><span class="sxs-lookup"><span data-stu-id="ddcb5-122">he Calendar appears when the user clicks into the textbox]</span></span>(handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

<span data-ttu-id="ddcb5-123">Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ddcb5-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


[![C<span data-ttu-id="ddcb5-124">licking w dniu umieszcza go w polu tekstowym]</span><span class="sxs-lookup"><span data-stu-id="ddcb5-124">licking on a date puts it in the textbox]</span></span>(handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

<span data-ttu-id="ddcb5-125">Klikając pozycję na wartość typu date umieszcza go w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ddcb5-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ddcb5-126">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="ddcb5-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
