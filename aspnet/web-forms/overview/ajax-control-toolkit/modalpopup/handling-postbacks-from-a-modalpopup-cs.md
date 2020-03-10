---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Obsługa ogłaszania zwrotnego z kontrolki modalpopupC#() | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Należy zwrócić szczególną uwagę, gdy pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554062"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="4a815-104">Obsługiwanie ogłaszania zwrotnego w kontrolce ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="4a815-104">Handling Postbacks from a ModalPopup (C#)</span></span>

<span data-ttu-id="4a815-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4a815-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4a815-106">[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4a815-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="4a815-107">Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4a815-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="4a815-108">Należy zwrócić szczególną uwagę na to, że po utworzeniu ogłaszania zwrotnego z poziomu okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="4a815-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="4a815-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4a815-109">Overview</span></span>

<span data-ttu-id="4a815-110">Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4a815-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="4a815-111">Należy zwrócić szczególną uwagę na to, że po utworzeniu ogłaszania zwrotnego z poziomu okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="4a815-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="4a815-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="4a815-112">Steps</span></span>

<span data-ttu-id="4a815-113">Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="4a815-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="4a815-114">Następnie Dodaj panel, który służy jako modalne menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="4a815-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="4a815-115">W tym miejscu użytkownik może wprowadzić nazwę i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="4a815-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="4a815-116">Przycisk służy do zamykania okna podręcznego i zapisywania informacji.</span><span class="sxs-lookup"><span data-stu-id="4a815-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="4a815-117">Należy zauważyć, że atrybut `OnClick` jest ustawiony tak, aby ogłaszać zwrotne po kliknięciu tego przycisku:</span><span class="sxs-lookup"><span data-stu-id="4a815-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="4a815-118">Sama strona składa się z dwóch etykiet dla dokładnie tych samych informacji: Nazwa i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="4a815-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="4a815-119">Przycisk służy do wyzwalania modalnego okienka podręcznego:</span><span class="sxs-lookup"><span data-stu-id="4a815-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="4a815-120">Aby wyświetlić okno podręczne, Dodaj kontrolkę `ModalPopupExtender`.</span><span class="sxs-lookup"><span data-stu-id="4a815-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="4a815-121">Ustaw atrybut `PopupControlID` na identyfikator panelu i `TargetControlID` na identyfikator przycisku:</span><span class="sxs-lookup"><span data-stu-id="4a815-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="4a815-122">Teraz za każdym razem, gdy zostanie kliknięty przycisk `Save` w modalnym menu podręcznym, zostanie uruchomiona Metoda `SaveData()` po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4a815-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="4a815-123">W tym miejscu można zapisać wprowadzone dane w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="4a815-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="4a815-124">W celu uproszczenia nowe dane są po prostu wyprowadzane na etykiecie:</span><span class="sxs-lookup"><span data-stu-id="4a815-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="4a815-125">Ponadto kontrolki TextBox w modalnym menu podręcznym powinny być wypełnione bieżącą nazwą i adresem e-mail.</span><span class="sxs-lookup"><span data-stu-id="4a815-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="4a815-126">Jednak jest to konieczne tylko w przypadku braku ogłaszania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="4a815-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="4a815-127">W przypadku ogłaszania zwrotnego funkcja stanu ASP.NET automatycznie wypełni pola tekstowe odpowiednimi wartościami.</span><span class="sxs-lookup"><span data-stu-id="4a815-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

<span data-ttu-id="4a815-128">[![modalne menu podręczne powoduje odświeżenie](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4a815-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="4a815-129">Modalne menu podręczne powoduje odświeżenie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4a815-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4a815-130">[Poprzednie](using-modalpopup-with-a-repeater-control-cs.md)
> [dalej](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4a815-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
