---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Obsługa ogłaszania zwrotnego w kontrolce popup bez elementu UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Gdy ogłaszanie zwrotne odbywa się w funkcji su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: aaecf77c1d25f2c99ef4e9948d79fc01b837169b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553985"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="d8ad0-104">Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="d8ad0-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>

<span data-ttu-id="d8ad0-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d8ad0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d8ad0-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d8ad0-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="d8ad0-107">Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="d8ad0-108">Gdy ogłaszanie zwrotne odbywa się w takim panelu, a na stronie jest dostępnych kilka paneli, trudno ustalić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="overview"></a><span data-ttu-id="d8ad0-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d8ad0-109">Overview</span></span>

<span data-ttu-id="d8ad0-110">Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="d8ad0-111">Gdy ogłaszanie zwrotne odbywa się w takim panelu, a na stronie jest dostępnych kilka paneli, trudno ustalić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="d8ad0-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="d8ad0-112">Steps</span></span>

<span data-ttu-id="d8ad0-113">W przypadku korzystania z `PopupControl` z ogłaszaniem zwrotnym, ale bez konieczności `UpdatePanel` na stronie, zestaw narzędzi kontroli nie oferuje metody określania, który element klienta wyzwolił okno podręczne, które z kolei spowodowało odświeżenie.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="d8ad0-114">Jednak niewielka lewę oferuje obejście tego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="d8ad0-115">Najpierw jest to podstawowa konfiguracja: dwa pola tekstowe, które wyzwalają to samo menu podręczne, kalendarz.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="d8ad0-116">Dwa `PopupControlExtenders` przełączenia pól tekstowych i podręcznych.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="d8ad0-117">Podstawowym pomysłem jest dodanie ukrytego pola formularza w &lt;`form`&gt; elementu, który zawiera pole tekstowe, które uruchomiło menu podręczne:</span><span class="sxs-lookup"><span data-stu-id="d8ad0-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="d8ad0-118">Po załadowaniu strony kod JavaScript dodaje procedurę obsługi zdarzeń do obu pól tekstowych: zawsze, gdy użytkownik kliknie pole tekstowe, jego nazwa jest zapisywana w ukrytym polu formularza:</span><span class="sxs-lookup"><span data-stu-id="d8ad0-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="d8ad0-119">W kodzie po stronie serwera wartość pola ukryte musi być odczytana.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="d8ad0-120">Ponieważ ukryte pola formularzy są proste do manipulowania, wymagana jest dozwolonych podejście do zweryfikowania wartości ukrytej.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="d8ad0-121">Po zidentyfikowaniu poprawnego pola tekstowego Data w kalendarzu zostanie zapisywana.</span><span class="sxs-lookup"><span data-stu-id="d8ad0-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]

<span data-ttu-id="d8ad0-122">[![kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d8ad0-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="d8ad0-123">Kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d8ad0-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>

<span data-ttu-id="d8ad0-124">[![kliknięcie daty spowoduje jej umieszczenie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d8ad0-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="d8ad0-125">Kliknięcie daty spowoduje jej umieszczenie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d8ad0-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d8ad0-126">Wstecz</span><span class="sxs-lookup"><span data-stu-id="d8ad0-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
