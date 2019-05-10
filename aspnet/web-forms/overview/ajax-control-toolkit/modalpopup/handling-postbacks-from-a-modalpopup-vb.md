---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Obsługa ogłaszania zwrotnego w kontrolce ModalPopup (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Specjalne należy zachować ostrożność podczas pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 3c1951e1ae4f97982d1263dfa9dc29454f7ce55a
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132674"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="5558a-104">Obsługiwanie ogłaszania zwrotnego w kontrolce ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="5558a-104">Handling Postbacks from a ModalPopup (VB)</span></span>

<span data-ttu-id="5558a-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5558a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5558a-106">[Pobierz program Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5558a-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="5558a-107">Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="5558a-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="5558a-108">Specjalne należy uważać podczas tworzenia ogłaszania zwrotnego z w ramach menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5558a-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="5558a-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5558a-109">Overview</span></span>

<span data-ttu-id="5558a-110">Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="5558a-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="5558a-111">Specjalne należy uważać podczas tworzenia ogłaszania zwrotnego z w ramach menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5558a-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="5558a-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="5558a-112">Steps</span></span>

<span data-ttu-id="5558a-113">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="5558a-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="5558a-114">Następnie dodaj panel, który służy jako modalnego okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5558a-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="5558a-115">Użytkownik może wprowadzić nazwę i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="5558a-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="5558a-116">Przycisk służy do Zamknij okno podręczne i Zapisz informacje.</span><span class="sxs-lookup"><span data-stu-id="5558a-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="5558a-117">Należy pamiętać, że `OnClick` atrybut jest ustawiony tak, aby ogłaszania zwrotnego występuje po kliknięciu tego przycisku:</span><span class="sxs-lookup"><span data-stu-id="5558a-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="5558a-118">Sama strona składa się z dwóch etykiet dla dokładnie te same informacje: Nazwa i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="5558a-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="5558a-119">Przycisk służy do wyzwalania modalnego okna podręcznego:</span><span class="sxs-lookup"><span data-stu-id="5558a-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="5558a-120">Aby było wyświetlane wyskakujące okienko, Dodaj `ModalPopupExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="5558a-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="5558a-121">Ustaw `PopupControlID` atrybutu ID panelu i `TargetControlID` identyfikatorowi przycisku:</span><span class="sxs-lookup"><span data-stu-id="5558a-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="5558a-122">Teraz zawsze, gdy `Save` w ramach modalnego okna podręcznego przycisku, po stronie serwera `SaveData()` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="5558a-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="5558a-123">Można zapisać wprowadzonych danych w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="5558a-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="5558a-124">Dla uproszczenia nowe dane po prostu znajdują się dane wyjściowe w etykiecie:</span><span class="sxs-lookup"><span data-stu-id="5558a-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="5558a-125">Ponadto kontrolki pola tekstowego, w ramach modalnego okna podręcznego powinny być widoczne bieżącą nazwę i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="5558a-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="5558a-126">Jednak jest to konieczne tylko po wystąpieniu nie zwrotu.</span><span class="sxs-lookup"><span data-stu-id="5558a-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="5558a-127">W przypadku zwrot funkcji viewstate programu ASP.NET spowoduje automatyczne wypełnienie pól tekstowych odpowiednimi wartościami.</span><span class="sxs-lookup"><span data-stu-id="5558a-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

<span data-ttu-id="5558a-128">[![Modalne okno podręczne powoduje odświeżenie strony](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5558a-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="5558a-129">Modalne okno podręczne powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5558a-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5558a-130">[Poprzednie](using-modalpopup-with-a-repeater-control-vb.md)
> [dalej](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5558a-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
