---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Zezwalanie tylko na niektóre znaki w polu tekstowym (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowy...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: 020f7bbe797a2c04f1ff97ea2056345028f700fb
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59407617"
---
# <a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="e1429-104">Zezwalanie tylko na niektóre znaki w polu tekstowym (C#)</span><span class="sxs-lookup"><span data-stu-id="e1429-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>

<span data-ttu-id="e1429-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e1429-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e1429-106">[Pobierz program Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e1429-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="e1429-107">Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e1429-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="e1429-108">Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowe znaki i podjęcie próby można przesłać formularza.</span><span class="sxs-lookup"><span data-stu-id="e1429-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="e1429-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="e1429-109">Overview</span></span>

<span data-ttu-id="e1429-110">Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e1429-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="e1429-111">Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowe znaki i podjęcie próby można przesłać formularza.</span><span class="sxs-lookup"><span data-stu-id="e1429-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="e1429-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="e1429-112">Steps</span></span>

<span data-ttu-id="e1429-113">ASP.NET AJAX Control Toolkit zawiera `FilteredTextBox` kontrolki, która rozszerza pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="e1429-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="e1429-114">Po uaktywnieniu tylko w określonym zestawie znaków mogą być wprowadzane w polu.</span><span class="sxs-lookup"><span data-stu-id="e1429-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="e1429-115">Aby to zrobić, najpierw należy w zwykły sposób ASP.NET AJAX `ScriptManager` która ładuje bibliotek JavaScript, które są również używane przez program ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="e1429-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="e1429-116">W efekcie potrzebujemy pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="e1429-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="e1429-117">Na koniec `FilteredTextBoxExtender` kontrolka zajmuje się ograniczenie znaków, użytkownik może wpisać.</span><span class="sxs-lookup"><span data-stu-id="e1429-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="e1429-118">Najpierw ustaw `TargetControlID` atrybutu `ID` z `TextBox` kontroli.</span><span class="sxs-lookup"><span data-stu-id="e1429-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="e1429-119">Następnie wybierz jedną z dostępnych `FilterType` wartości:</span><span class="sxs-lookup"><span data-stu-id="e1429-119">Then, choose one of the available `FilterType` values:</span></span>

- `Custom` <span data-ttu-id="e1429-120">Domyślnie; Musisz podać listę prawidłowe znaki</span><span class="sxs-lookup"><span data-stu-id="e1429-120">default; you have to provide a list of valid chars</span></span>
- `LowercaseLetters` <span data-ttu-id="e1429-121">małe litery</span><span class="sxs-lookup"><span data-stu-id="e1429-121">lowercase letters only</span></span>
- `Numbers` <span data-ttu-id="e1429-122">tylko cyfry</span><span class="sxs-lookup"><span data-stu-id="e1429-122">digits only</span></span>
- `UppercaseLetters` <span data-ttu-id="e1429-123">tylko wielkie litery</span><span class="sxs-lookup"><span data-stu-id="e1429-123">uppercase letters only</span></span>

<span data-ttu-id="e1429-124">Jeśli `Custom FilterType` jest używany, `ValidChars` właściwość musi być ustawione i podać listę znaków, które mogą być wpisana.</span><span class="sxs-lookup"><span data-stu-id="e1429-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="e1429-125">Przy okazji: Jeśli użytkownik próbuje wkleić tekst w polu tekstowym, zostaną usunięte wszystkie nieprawidłowe znaki.</span><span class="sxs-lookup"><span data-stu-id="e1429-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="e1429-126">Oto znaczniki dla `FilteredTextBoxExtender` formant, który dopuszcza tylko cyfr (coś, który także byłby możliwego `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="e1429-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="e1429-127">Uruchomić stronę i spróbuj wprowadzić literą, jeśli włączona JavaScript, nie będzie on działał; cyfry są jednak wyświetlane na stronie.</span><span class="sxs-lookup"><span data-stu-id="e1429-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="e1429-128">Jednak należy pamiętać, że ochrona `FilteredTextBox` zapewnia nie jest dowód punktor: Jeśli JavaScript jest włączony, wszelkie dane mogą być wprowadzane w polu tekstowym, więc trzeba użyć oznacza, że dodatkowe sprawdzenie poprawności, czyli ASP. Formanty sprawdzania poprawności w sieci.</span><span class="sxs-lookup"><span data-stu-id="e1429-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


[![O<span data-ttu-id="e1429-129">mogą być wprowadzane cyfry tylko do odczytu]</span><span class="sxs-lookup"><span data-stu-id="e1429-129">nly digits may be entered]</span></span>(allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

<span data-ttu-id="e1429-130">Mogą być wprowadzane tylko cyfry ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e1429-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e1429-131">Next</span><span class="sxs-lookup"><span data-stu-id="e1429-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
