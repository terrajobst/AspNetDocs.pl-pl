---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Pozycjonowanie elementu kontrolki modalpopup (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak formant nie oferuje...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: fb79a08a339588ed8adc4b4236911819ea9286b4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598958"
---
# <a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="3aff1-104">Pozycjonowanie kontrolki ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="3aff1-104">Positioning a ModalPopup (VB)</span></span>

<span data-ttu-id="3aff1-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3aff1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3aff1-106">[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3aff1-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="3aff1-107">Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3aff1-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="3aff1-108">Jednak formant nie oferuje wbudowanej funkcji do pozycjonowania okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="3aff1-108">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="3aff1-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="3aff1-109">Overview</span></span>

<span data-ttu-id="3aff1-110">Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3aff1-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="3aff1-111">Jednak formant nie oferuje wbudowanej funkcji do pozycjonowania okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="3aff1-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="3aff1-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="3aff1-112">Steps</span></span>

<span data-ttu-id="3aff1-113">W celu aktywowania funkcji ASP.NET AJAX i zestawu narzędzi sterowania `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="3aff1-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="3aff1-114">Kontrolka musi być umieszczona w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="3aff1-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="3aff1-115">Następnie Dodaj panel, który służy jako modalne menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="3aff1-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="3aff1-116">Przycisk jest używany do zamykania okna podręcznego:</span><span class="sxs-lookup"><span data-stu-id="3aff1-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="3aff1-117">Gdy zostanie wyświetlone okno podręczne, będzie ono umieszczane w określonym miejscu na stronie.</span><span class="sxs-lookup"><span data-stu-id="3aff1-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="3aff1-118">W przypadku tego zadania zostanie utworzona funkcja JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3aff1-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="3aff1-119">Najpierw próbuje uzyskać dostęp do panelu.</span><span class="sxs-lookup"><span data-stu-id="3aff1-119">It first tries to access the panel.</span></span> <span data-ttu-id="3aff1-120">Jeśli to się powiedzie, pozycja panelu jest ustawiana za pomocą CSS i JavaScript (Zmień położenie okna podręcznego w programie).</span><span class="sxs-lookup"><span data-stu-id="3aff1-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="3aff1-121">Jednak formant `ModalPopupExtender` próbuje również umieścić okno podręczne.</span><span class="sxs-lookup"><span data-stu-id="3aff1-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="3aff1-122">W związku z tym kod JavaScript wielokrotnie umieszcza okno podręczne, co dziesiąte części sekundy.</span><span class="sxs-lookup"><span data-stu-id="3aff1-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="3aff1-123">Jak widać, wartość zwracana `setTimeout()` Metoda JavaScript jest zapisywana w zmiennej globalnej.</span><span class="sxs-lookup"><span data-stu-id="3aff1-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="3aff1-124">Pozwala to zatrzymać powtarzające się pozycjonowanie okna podręcznego na żądanie przy użyciu metody `clearTimeout()`:</span><span class="sxs-lookup"><span data-stu-id="3aff1-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="3aff1-125">Teraz wszystko, co pozostało do zrobienia, ma mieć możliwość wywołania tych funkcji w przeglądarce w miarę potrzeb.</span><span class="sxs-lookup"><span data-stu-id="3aff1-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="3aff1-126">Funkcja `movePanel()` JavaScript musi być wywoływana, gdy kliknięto przycisk, który wyzwala panel:</span><span class="sxs-lookup"><span data-stu-id="3aff1-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="3aff1-127">A funkcja `stopMoving()` jest odtwarzana po zamknięciu okna podręcznego, może być wyzwalane w kontrolce `ModalPopupExtender`:</span><span class="sxs-lookup"><span data-stu-id="3aff1-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]

<span data-ttu-id="3aff1-128">[![modalny podręczny jest wyświetlany w wyznaczeniu pozycji](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3aff1-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="3aff1-129">Modalne menu podręczne pojawia się w wydzielonym miejscu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3aff1-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3aff1-130">Ubiegł</span><span class="sxs-lookup"><span data-stu-id="3aff1-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
