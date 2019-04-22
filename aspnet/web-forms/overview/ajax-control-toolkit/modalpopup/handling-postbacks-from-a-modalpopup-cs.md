---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Obsługa ogłaszania zwrotnego w kontrolce ModalPopup (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Specjalne należy zachować ostrożność podczas pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 54dd3bae21e661e0b17cab6a71f0df33b6712bcd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59388559"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="5d880-104">Obsługiwanie ogłaszania zwrotnego w kontrolce ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="5d880-104">Handling Postbacks from a ModalPopup (C#)</span></span>

<span data-ttu-id="5d880-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5d880-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5d880-106">[Pobierz program Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5d880-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="5d880-107">Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="5d880-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="5d880-108">Specjalne należy uważać podczas tworzenia ogłaszania zwrotnego z w ramach menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5d880-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="5d880-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5d880-109">Overview</span></span>

<span data-ttu-id="5d880-110">Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="5d880-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="5d880-111">Specjalne należy uważać podczas tworzenia ogłaszania zwrotnego z w ramach menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5d880-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="5d880-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="5d880-112">Steps</span></span>

<span data-ttu-id="5d880-113">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="5d880-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="5d880-114">Następnie dodaj panel, który służy jako modalnego okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="5d880-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="5d880-115">Użytkownik może wprowadzić nazwę i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="5d880-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="5d880-116">Przycisk służy do Zamknij okno podręczne i Zapisz informacje.</span><span class="sxs-lookup"><span data-stu-id="5d880-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="5d880-117">Należy pamiętać, że `OnClick` atrybut jest ustawiony tak, aby ogłaszania zwrotnego występuje po kliknięciu tego przycisku:</span><span class="sxs-lookup"><span data-stu-id="5d880-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="5d880-118">Sama strona składa się z dwóch etykiet dla dokładnie te same informacje: Nazwa i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="5d880-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="5d880-119">Przycisk służy do wyzwalania modalnego okna podręcznego:</span><span class="sxs-lookup"><span data-stu-id="5d880-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="5d880-120">Aby było wyświetlane wyskakujące okienko, Dodaj `ModalPopupExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="5d880-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="5d880-121">Ustaw `PopupControlID` atrybutu ID panelu i `TargetControlID` identyfikatorowi przycisku:</span><span class="sxs-lookup"><span data-stu-id="5d880-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="5d880-122">Teraz zawsze, gdy `Save` w ramach modalnego okna podręcznego przycisku, po stronie serwera `SaveData()` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="5d880-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="5d880-123">Można zapisać wprowadzonych danych w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="5d880-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="5d880-124">Dla uproszczenia nowe dane po prostu znajdują się dane wyjściowe w etykiecie:</span><span class="sxs-lookup"><span data-stu-id="5d880-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="5d880-125">Ponadto kontrolki pola tekstowego, w ramach modalnego okna podręcznego powinny być widoczne bieżącą nazwę i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="5d880-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="5d880-126">Jednak jest to konieczne tylko po wystąpieniu nie zwrotu.</span><span class="sxs-lookup"><span data-stu-id="5d880-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="5d880-127">W przypadku zwrot funkcji viewstate programu ASP.NET spowoduje automatyczne wypełnienie pól tekstowych odpowiednimi wartościami.</span><span class="sxs-lookup"><span data-stu-id="5d880-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="5d880-128">[![Modalne okno podręczne powoduje odświeżenie strony](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5d880-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="5d880-129">Modalne okno podręczne powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5d880-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5d880-130">[Poprzednie](using-modalpopup-with-a-repeater-control-cs.md)
> [dalej](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5d880-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
