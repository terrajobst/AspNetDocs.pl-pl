---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Uruchamianie modalnego okna podręcznego z koduC#serwera () | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak niektóre scenariusze wymagają, aby t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: fec0ce2cdd24333f65201301718440e1a09d930e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613296"
---
# <a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="8df85-104">Uruchamianie modalnego okna podręcznego z kodu serwera (C#)</span><span class="sxs-lookup"><span data-stu-id="8df85-104">Launching a Modal Popup Window from Server Code (C#)</span></span>

<span data-ttu-id="8df85-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8df85-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8df85-106">[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8df85-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="8df85-107">Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8df85-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="8df85-108">Jednak niektóre scenariusze wymagają, aby otwierając modalne menu podręczne zostało wyzwolone po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="8df85-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="overview"></a><span data-ttu-id="8df85-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8df85-109">Overview</span></span>

<span data-ttu-id="8df85-110">Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8df85-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="8df85-111">Jednak niektóre scenariusze wymagają, aby otwierając modalne menu podręczne zostało wyzwolone po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="8df85-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="8df85-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="8df85-112">Steps</span></span>

<span data-ttu-id="8df85-113">Pierwszym z nich jest wymagana kontrolka sieci Web ASP.NET Button, aby zademonstrować sposób działania kontrolki kontrolki modalpopup.</span><span class="sxs-lookup"><span data-stu-id="8df85-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="8df85-114">Dodaj taki przycisk w formularzu &lt;&gt; elementu na nowej stronie:</span><span class="sxs-lookup"><span data-stu-id="8df85-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="8df85-115">Następnie potrzebujesz znacznika dla tworzonego okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="8df85-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="8df85-116">Zdefiniuj ją jako kontrolkę `<asp:Panel>` i upewnij się, że zawiera ona kontrolkę Button.</span><span class="sxs-lookup"><span data-stu-id="8df85-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="8df85-117">Kontrolka kontrolki modalpopup oferuje funkcje, które umożliwiają podjęcie takiego przycisku w celu zamknięcia okna podręcznego; w przeciwnym razie nie istnieje łatwy sposób, aby to umożliwić.</span><span class="sxs-lookup"><span data-stu-id="8df85-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="8df85-118">Następnie Dodaj kontrolkę kontrolki modalpopup z zestawu narzędzi ASP.NET AJAX do strony.</span><span class="sxs-lookup"><span data-stu-id="8df85-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="8df85-119">Ustaw właściwości przycisku, który ładuje formant, przycisk, który go znika, i identyfikator rzeczywistego okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="8df85-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="8df85-120">Tak jak w przypadku wszystkich stron sieci Web opartych na ASP.NET AJAX; Menedżer skryptów jest wymagany do załadowania niezbędnych bibliotek JavaScript dla różnych przeglądarek docelowych:</span><span class="sxs-lookup"><span data-stu-id="8df85-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="8df85-121">Uruchom przykład w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8df85-121">Run the example in the browser.</span></span> <span data-ttu-id="8df85-122">Po kliknięciu przycisku zostanie wyświetlone modalne menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="8df85-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="8df85-123">Aby osiągnąć ten sam efekt przy użyciu kodu po stronie serwera, wymagany jest nowy przycisk:</span><span class="sxs-lookup"><span data-stu-id="8df85-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="8df85-124">Jak widać, kliknięcie przycisku spowoduje wygenerowanie ogłaszania zwrotnego i wykonanie `ServerButton_Click()` metody na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8df85-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="8df85-125">W tej metodzie funkcja języka JavaScript o nazwie `launchModal()` jest wykonywana jako dokładna, a funkcja JavaScript zostanie wykonana po załadowaniu strony:</span><span class="sxs-lookup"><span data-stu-id="8df85-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="8df85-126">Zadanie `launchModal()` ma wyświetlać kontrolki modalpopup.</span><span class="sxs-lookup"><span data-stu-id="8df85-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="8df85-127">Funkcja `launchModal()` jest wykonywana po załadowaniu kompletnej strony HTML.</span><span class="sxs-lookup"><span data-stu-id="8df85-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="8df85-128">W tej chwili jednak ASP.NET AJAX Framework nie został jeszcze w pełni załadowany.</span><span class="sxs-lookup"><span data-stu-id="8df85-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="8df85-129">W związku z tym funkcja `launchModal()` jedynie ustawia zmienną, którą formant kontrolki modalpopup musi być pokazywany w dalszej części:</span><span class="sxs-lookup"><span data-stu-id="8df85-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="8df85-130">`pageLoad()` funkcja języka JavaScript to specjalna funkcja, która jest wykonywana po całkowitym załadowaniu ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="8df85-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="8df85-131">W związku z tym dodamy kod do tej funkcji, aby pokazać formant kontrolki modalpopup, ale tylko wtedy, gdy `launchModal()` został wywołany przed:</span><span class="sxs-lookup"><span data-stu-id="8df85-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="8df85-132">Funkcja `$find()` szuka nazwanego elementu na stronie i oczekuje identyfikatora po stronie serwera jako parametru.</span><span class="sxs-lookup"><span data-stu-id="8df85-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="8df85-133">W związku z tym `$find("mpe")` zwraca reprezentację klienta formantu kontrolki modalpopup; jego `show()` Metoda umożliwia wyświetlenie okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="8df85-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>

<span data-ttu-id="8df85-134">[![modalne okno podręczne pojawia się po kliknięciu jednego z przycisków](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8df85-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="8df85-135">Modalne okno podręczne pojawia się po kliknięciu jednego z przycisków ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8df85-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8df85-136">Dalej</span><span class="sxs-lookup"><span data-stu-id="8df85-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
