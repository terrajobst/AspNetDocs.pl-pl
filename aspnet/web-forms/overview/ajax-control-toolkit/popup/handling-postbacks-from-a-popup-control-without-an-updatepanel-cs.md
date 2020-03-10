---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Obsługa ogłaszania zwrotnego w kontrolce popup bez elementuC#UpdatePanel () | Microsoft Docs
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Gdy ogłaszanie zwrotne odbywa się w funkcji su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612743"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="2cdd6-104">Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="2cdd6-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>

<span data-ttu-id="2cdd6-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2cdd6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2cdd6-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2cdd6-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="2cdd6-107">Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="2cdd6-108">Gdy ogłaszanie zwrotne odbywa się w takim panelu, a na stronie jest dostępnych kilka paneli, trudno ustalić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="overview"></a><span data-ttu-id="2cdd6-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2cdd6-109">Overview</span></span>

<span data-ttu-id="2cdd6-110">Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="2cdd6-111">Gdy ogłaszanie zwrotne odbywa się w takim panelu, a na stronie jest dostępnych kilka paneli, trudno ustalić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="2cdd6-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="2cdd6-112">Steps</span></span>

<span data-ttu-id="2cdd6-113">W przypadku korzystania z `PopupControl` z ogłaszaniem zwrotnym, ale bez konieczności `UpdatePanel` na stronie, zestaw narzędzi kontroli nie oferuje metody określania, który element klienta wyzwolił okno podręczne, które z kolei spowodowało odświeżenie.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="2cdd6-114">Jednak niewielka lewę oferuje obejście tego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="2cdd6-115">Najpierw jest to podstawowa konfiguracja: dwa pola tekstowe, które wyzwalają to samo menu podręczne, kalendarz.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="2cdd6-116">Dwa `PopupControlExtenders` przełączenia pól tekstowych i podręcznych.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="2cdd6-117">Podstawowym pomysłem jest dodanie ukrytego pola formularza w &lt;`form`&gt; elementu, który zawiera pole tekstowe, które uruchomiło menu podręczne:</span><span class="sxs-lookup"><span data-stu-id="2cdd6-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="2cdd6-118">Po załadowaniu strony kod JavaScript dodaje procedurę obsługi zdarzeń do obu pól tekstowych: zawsze, gdy użytkownik kliknie pole tekstowe, jego nazwa jest zapisywana w ukrytym polu formularza:</span><span class="sxs-lookup"><span data-stu-id="2cdd6-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="2cdd6-119">W kodzie po stronie serwera wartość pola ukryte musi być odczytana.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="2cdd6-120">Ponieważ ukryte pola formularzy są proste do manipulowania, wymagana jest dozwolonych podejście do zweryfikowania wartości ukrytej.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="2cdd6-121">Po zidentyfikowaniu poprawnego pola tekstowego Data w kalendarzu zostanie zapisywana.</span><span class="sxs-lookup"><span data-stu-id="2cdd6-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

<span data-ttu-id="2cdd6-122">[![kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2cdd6-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="2cdd6-123">Kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2cdd6-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>

<span data-ttu-id="2cdd6-124">[![kliknięcie daty spowoduje jej umieszczenie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2cdd6-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="2cdd6-125">Kliknięcie daty spowoduje jej umieszczenie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2cdd6-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2cdd6-126">[Poprzednie](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [dalej](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2cdd6-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
