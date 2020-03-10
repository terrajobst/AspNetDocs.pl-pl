---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Obsługa ogłaszania zwrotnego z kontrolki modalpopup (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Należy zwrócić szczególną uwagę, gdy pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: df0b71b3e336a0d230869623473bdac24b3dd07b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621528"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="b42a8-104">Obsługiwanie ogłaszania zwrotnego w kontrolce ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="b42a8-104">Handling Postbacks from a ModalPopup (VB)</span></span>

<span data-ttu-id="b42a8-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b42a8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b42a8-106">[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b42a8-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="b42a8-107">Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="b42a8-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b42a8-108">Należy zwrócić szczególną uwagę na to, że po utworzeniu ogłaszania zwrotnego z poziomu okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="b42a8-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="b42a8-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b42a8-109">Overview</span></span>

<span data-ttu-id="b42a8-110">Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="b42a8-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b42a8-111">Należy zwrócić szczególną uwagę na to, że po utworzeniu ogłaszania zwrotnego z poziomu okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="b42a8-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="b42a8-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="b42a8-112">Steps</span></span>

<span data-ttu-id="b42a8-113">Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="b42a8-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="b42a8-114">Następnie Dodaj panel, który służy jako modalne menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="b42a8-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="b42a8-115">W tym miejscu użytkownik może wprowadzić nazwę i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b42a8-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="b42a8-116">Przycisk służy do zamykania okna podręcznego i zapisywania informacji.</span><span class="sxs-lookup"><span data-stu-id="b42a8-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="b42a8-117">Należy zauważyć, że atrybut `OnClick` jest ustawiony tak, aby ogłaszać zwrotne po kliknięciu tego przycisku:</span><span class="sxs-lookup"><span data-stu-id="b42a8-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="b42a8-118">Sama strona składa się z dwóch etykiet dla dokładnie tych samych informacji: Nazwa i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b42a8-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="b42a8-119">Przycisk służy do wyzwalania modalnego okienka podręcznego:</span><span class="sxs-lookup"><span data-stu-id="b42a8-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="b42a8-120">Aby wyświetlić okno podręczne, Dodaj kontrolkę `ModalPopupExtender`.</span><span class="sxs-lookup"><span data-stu-id="b42a8-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="b42a8-121">Ustaw atrybut `PopupControlID` na identyfikator panelu i `TargetControlID` na identyfikator przycisku:</span><span class="sxs-lookup"><span data-stu-id="b42a8-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="b42a8-122">Teraz za każdym razem, gdy zostanie kliknięty przycisk `Save` w modalnym menu podręcznym, zostanie uruchomiona Metoda `SaveData()` po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="b42a8-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="b42a8-123">W tym miejscu można zapisać wprowadzone dane w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="b42a8-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="b42a8-124">W celu uproszczenia nowe dane są po prostu wyprowadzane na etykiecie:</span><span class="sxs-lookup"><span data-stu-id="b42a8-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="b42a8-125">Ponadto kontrolki TextBox w modalnym menu podręcznym powinny być wypełnione bieżącą nazwą i adresem e-mail.</span><span class="sxs-lookup"><span data-stu-id="b42a8-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="b42a8-126">Jednak jest to konieczne tylko w przypadku braku ogłaszania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="b42a8-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="b42a8-127">W przypadku ogłaszania zwrotnego funkcja stanu ASP.NET automatycznie wypełni pola tekstowe odpowiednimi wartościami.</span><span class="sxs-lookup"><span data-stu-id="b42a8-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

<span data-ttu-id="b42a8-128">[![modalne menu podręczne powoduje odświeżenie](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b42a8-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="b42a8-129">Modalne menu podręczne powoduje odświeżenie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b42a8-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b42a8-130">[Poprzednie](using-modalpopup-with-a-repeater-control-vb.md)
> [dalej](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b42a8-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
