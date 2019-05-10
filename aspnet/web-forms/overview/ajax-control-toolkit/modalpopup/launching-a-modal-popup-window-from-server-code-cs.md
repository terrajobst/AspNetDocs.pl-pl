---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Uruchamianie modalnego okna podręcznego z kodu serwera (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Jednak niektóre scenariusze wymagają tego t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cc2d9c7a571a8f76e9d935784810280c348b6bb8
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132630"
---
# <a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="0939a-104">Uruchamianie modalnego okna podręcznego z kodu serwera (C#)</span><span class="sxs-lookup"><span data-stu-id="0939a-104">Launching a Modal Popup Window from Server Code (C#)</span></span>

<span data-ttu-id="0939a-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0939a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0939a-106">[Pobierz program Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0939a-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="0939a-107">Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0939a-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="0939a-108">Jednak niektóre scenariusze wymagają, że otwarcie modalnego okna podręcznego jest wyzwalane po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0939a-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="overview"></a><span data-ttu-id="0939a-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0939a-109">Overview</span></span>

<span data-ttu-id="0939a-110">Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0939a-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="0939a-111">Jednak niektóre scenariusze wymagają, że otwarcie modalnego okna podręcznego jest wyzwalane po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0939a-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="0939a-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="0939a-112">Steps</span></span>

<span data-ttu-id="0939a-113">Przede wszystkim kontrolkę przycisku ASP.NET sieci web jest wymagany, aby zademonstrować, jak działa Kontrola ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="0939a-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="0939a-114">Dodawanie przycisku w ramach &lt;formularza&gt; element na nowej stronie:</span><span class="sxs-lookup"><span data-stu-id="0939a-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="0939a-115">Następnie należy znaczników wyskakującego okienka, który chcesz utworzyć.</span><span class="sxs-lookup"><span data-stu-id="0939a-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="0939a-116">Zdefiniuj go w formie `<asp:Panel>` kontroli i upewnij się, że zawiera on kontrolkę przycisku.</span><span class="sxs-lookup"><span data-stu-id="0939a-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="0939a-117">Kontrolki ModalPopup oferuje funkcje, aby takie przycisk Zamknij okno podręczne; w przeciwnym razie jest łatwym sposobem Niech dopasowywane.</span><span class="sxs-lookup"><span data-stu-id="0939a-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="0939a-118">Następnie dodaj kontrolki ModalPopup z zestawu narzędzi AJAX programu ASP.NET do strony.</span><span class="sxs-lookup"><span data-stu-id="0939a-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="0939a-119">Ustaw właściwości dla przycisku, który ładuje formant, przycisk, który sprawia, że zniknąć i identyfikator rzeczywiste okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="0939a-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="0939a-120">Podobnie jak w przypadku wszystkich stron sieci web, w oparciu o ASP.NET AJAX; Menedżer skryptu jest wymagane do załadowania wymaganych bibliotek JavaScript dla przeglądarek inny element docelowy:</span><span class="sxs-lookup"><span data-stu-id="0939a-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="0939a-121">Uruchom przykład w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="0939a-121">Run the example in the browser.</span></span> <span data-ttu-id="0939a-122">Po kliknięciu przycisku zostanie wyświetlone modalnego okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="0939a-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="0939a-123">Aby osiągnąć ten sam efekt przy użyciu kodu po stronie serwera, wymagane jest nowy przycisk:</span><span class="sxs-lookup"><span data-stu-id="0939a-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="0939a-124">Jak widać, kliknij przycisk generuje odświeżenie strony i wykonuje `ServerButton_Click()` metody na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0939a-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="0939a-125">W przypadku tej metody jest wywoływana funkcja języka JavaScript `launchModal()` jest wykonywana funkcja języka JavaScript się dokładnie, zostaną wykonane po załadowaniu strony:</span><span class="sxs-lookup"><span data-stu-id="0939a-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="0939a-126">Zadaniem `launchModal()` jest wyświetlanie ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="0939a-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="0939a-127">`launchModal()` Funkcja jest wykonywana po załadowaniu pełne strony HTML.</span><span class="sxs-lookup"><span data-stu-id="0939a-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="0939a-128">W tej chwili jednak platformę ASP.NET AJAX nie został w pełni załadowany jeszcze.</span><span class="sxs-lookup"><span data-stu-id="0939a-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="0939a-129">W związku z tym `launchModal()` funkcja po prostu ustawia zmienną, która kontrolki ModalPopup musi zostać pokazany w późniejszym czasie na:</span><span class="sxs-lookup"><span data-stu-id="0939a-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="0939a-130">`pageLoad()` Funkcji języka JavaScript jest funkcją specjalne, która pobiera wykonywane po całkowitym załadowaniu ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="0939a-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="0939a-131">Dlatego dodamy kod tę funkcję, aby wyświetlić kontrolki ModalPopup, ale tylko wtedy, gdy `launchModal()` została wywołana przed:</span><span class="sxs-lookup"><span data-stu-id="0939a-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="0939a-132">`$find()` Funkcji szuka nazwanego elementu na stronie i oczekuje, że identyfikator po stronie serwera, jako parametr.</span><span class="sxs-lookup"><span data-stu-id="0939a-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="0939a-133">W związku z tym `$find("mpe")` zwraca reprezentację klienta kontrolki ModalPopup; jej `show()` metoda umożliwia wyskakujące okienko wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="0939a-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>

<span data-ttu-id="0939a-134">[![Modalnego okna podręcznego, pojawia się, gdy kliknięto opcję przycisków](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0939a-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="0939a-135">Modalnego okna podręcznego, pojawia się, gdy jeden z przycisków kliknięciu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0939a-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0939a-136">Next</span><span class="sxs-lookup"><span data-stu-id="0939a-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
