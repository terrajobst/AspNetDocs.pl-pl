---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Zezwalanie tylko na niektóre znaki w polu tekstowym (VB) | Microsoft Docs
author: wenz
description: Kontrolki walidacji ASP.NET mogą zapewnić, że w danych wejściowych użytkownika dozwolone są tylko pewne znaki. Jednak nadal nie jest możliwe wpisywanie przez użytkowników nieprawidłowych znaków...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 895708ebecc30c5f35e6ecd0349604bb777cbd93
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613513"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="06297-104">Zezwalanie tylko na niektóre znaki w polu tekstowym (VB)</span><span class="sxs-lookup"><span data-stu-id="06297-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>

<span data-ttu-id="06297-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="06297-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="06297-106">[Pobierz kod](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="06297-106">[Download Code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="06297-107">Kontrolki walidacji ASP.NET mogą zapewnić, że w danych wejściowych użytkownika dozwolone są tylko pewne znaki.</span><span class="sxs-lookup"><span data-stu-id="06297-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="06297-108">Mimo to nadal nie jest możliwe wpisywanie przez użytkowników nieprawidłowych znaków i próba przesłania formularza.</span><span class="sxs-lookup"><span data-stu-id="06297-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="overview"></a><span data-ttu-id="06297-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="06297-109">Overview</span></span>

<span data-ttu-id="06297-110">Kontrolki walidacji ASP.NET mogą zapewnić, że w danych wejściowych użytkownika dozwolone są tylko pewne znaki.</span><span class="sxs-lookup"><span data-stu-id="06297-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="06297-111">Mimo to nadal nie jest możliwe wpisywanie przez użytkowników nieprawidłowych znaków i próba przesłania formularza.</span><span class="sxs-lookup"><span data-stu-id="06297-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="06297-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="06297-112">Steps</span></span>

<span data-ttu-id="06297-113">Zestaw narzędzi ASP.NET AJAX Control Toolkit zawiera kontrolkę `FilteredTextBox`, która rozszerza pole tekstowe.</span><span class="sxs-lookup"><span data-stu-id="06297-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="06297-114">Po aktywowaniu tego pola może być wprowadzony tylko określony zestaw znaków.</span><span class="sxs-lookup"><span data-stu-id="06297-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="06297-115">Aby to działało, najpierw musimy być zwykle ASP.NET AJAX `ScriptManager`, która ładuje biblioteki JavaScript, które są również używane przez zestaw narzędzi ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="06297-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="06297-116">Następnie potrzebujemy pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="06297-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="06297-117">Na koniec kontrola `FilteredTextBoxExtender` ma na celu ograniczenie znaków, które użytkownik może wpisać.</span><span class="sxs-lookup"><span data-stu-id="06297-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="06297-118">Najpierw ustaw atrybut `TargetControlID` na `ID` kontrolki `TextBox`.</span><span class="sxs-lookup"><span data-stu-id="06297-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="06297-119">Następnie wybierz jedną z dostępnych wartości `FilterType`:</span><span class="sxs-lookup"><span data-stu-id="06297-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="06297-120">`Custom` domyślne; musisz podać listę prawidłowych znaków</span><span class="sxs-lookup"><span data-stu-id="06297-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="06297-121">tylko `LowercaseLetters` małe litery</span><span class="sxs-lookup"><span data-stu-id="06297-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="06297-122">tylko cyfry `Numbers`</span><span class="sxs-lookup"><span data-stu-id="06297-122">`Numbers` digits only</span></span>
- <span data-ttu-id="06297-123">tylko `UppercaseLetters` wielkie litery</span><span class="sxs-lookup"><span data-stu-id="06297-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="06297-124">Jeśli `Custom FilterType` jest używany, właściwość `ValidChars` musi być ustawiona i zawierać listę znaków, które mogą zostać wpisane.</span><span class="sxs-lookup"><span data-stu-id="06297-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="06297-125">Według sposobu: Jeśli spróbujesz wkleić tekst do pola tekstowego, wszystkie nieprawidłowe znaki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="06297-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="06297-126">Poniżej znajduje się znacznik kontrolki `FilteredTextBoxExtender`, który zezwala tylko na cyfry (co również było możliwe w przypadku `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="06297-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="06297-127">Uruchom stronę i spróbuj wprowadzić literę, jeśli język JavaScript jest włączony, nie będzie działać. cyfry są jednak wyświetlane na stronie.</span><span class="sxs-lookup"><span data-stu-id="06297-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="06297-128">Należy jednak pamiętać, że `FilteredTextBox` ochrony nie jest punktorem-testowym: Jeśli JavaScript jest włączony, wszystkie dane mogą być wprowadzane w polu tekstowym, więc należy użyć dodatkowej weryfikacji, np. ASP. Kontrolki walidacji netto.</span><span class="sxs-lookup"><span data-stu-id="06297-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>

<span data-ttu-id="06297-129">[można wprowadzać tylko ![cyfry](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06297-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="06297-130">Można wprowadzić tylko cyfry ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="06297-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="06297-131">Wstecz</span><span class="sxs-lookup"><span data-stu-id="06297-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
