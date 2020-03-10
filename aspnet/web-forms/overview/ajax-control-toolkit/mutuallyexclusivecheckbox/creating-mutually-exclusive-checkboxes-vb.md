---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Tworzenie wzajemnie wykluczających się pól wyboru (VB) | Microsoft Docs
author: wenz
description: 'Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe. Istnieje zwrot, chociaż: wybrano jeden przycisk radiowy w grupie,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554013"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="1cf14-104">Tworzenie wzajemnie wykluczających się pól wyboru (VB)</span><span class="sxs-lookup"><span data-stu-id="1cf14-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>

<span data-ttu-id="1cf14-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1cf14-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1cf14-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1cf14-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="1cf14-107">Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe.</span><span class="sxs-lookup"><span data-stu-id="1cf14-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="1cf14-108">Istnieje zwrot, chociaż: po wybraniu jednego przycisku radiowego w grupie nie można usunąć zaznaczenia wszystkich przycisków radiowych.</span><span class="sxs-lookup"><span data-stu-id="1cf14-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="1cf14-109">Pola wyboru można zaznaczać w dowolnym momencie, ale nie wykluczają się wzajemnie.</span><span class="sxs-lookup"><span data-stu-id="1cf14-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="1cf14-110">Ten samouczek zawiera najlepsze podejścia: pola wyboru, które wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="1cf14-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="1cf14-111">Omówienie</span><span class="sxs-lookup"><span data-stu-id="1cf14-111">Overview</span></span>

<span data-ttu-id="1cf14-112">Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe.</span><span class="sxs-lookup"><span data-stu-id="1cf14-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="1cf14-113">Istnieje zwrot, chociaż: po wybraniu jednego przycisku radiowego w grupie nie można usunąć zaznaczenia wszystkich przycisków radiowych.</span><span class="sxs-lookup"><span data-stu-id="1cf14-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="1cf14-114">Pola wyboru można zaznaczać w dowolnym momencie, ale nie wykluczają się wzajemnie.</span><span class="sxs-lookup"><span data-stu-id="1cf14-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="1cf14-115">Ten samouczek zawiera najlepsze podejścia: pola wyboru, które wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="1cf14-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="1cf14-116">Kroki</span><span class="sxs-lookup"><span data-stu-id="1cf14-116">Steps</span></span>

<span data-ttu-id="1cf14-117">Zestaw narzędzi ASP.NET AJAX Control Toolkit zawiera rozszerzenie MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="1cf14-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="1cf14-118">Dzięki temu programiści mogą przypisywać dowolne pola wyboru do nazwy grupy (`Key` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="1cf14-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="1cf14-119">Ze wszystkich pól wyboru w tej samej grupie można wybrać tylko jedną z nich.</span><span class="sxs-lookup"><span data-stu-id="1cf14-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="1cf14-120">Zacznijmy od umieszczenia dwóch pól wyboru na nowej stronie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1cf14-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="1cf14-121">Może być więcej, ale dwa z nich wystarczą do zademonstrowania zasady:</span><span class="sxs-lookup"><span data-stu-id="1cf14-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="1cf14-122">Dla obu pól wyboru formant MutuallyExclusiveCheckBoxExtender musi być umieszczony na stronie.</span><span class="sxs-lookup"><span data-stu-id="1cf14-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="1cf14-123">Oba atrybuty klucza muszą mieć tę samą wartość, podobnie jak atrybuty wartości elementów przycisku radiowego HTML muszą być identyczne, aby można było zauważyć, do której grupy należą.</span><span class="sxs-lookup"><span data-stu-id="1cf14-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="1cf14-124">Właściwość TargetControlID elementu Extender wskazuje identyfikator pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="1cf14-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="1cf14-125">Na koniec Uwzględnij ASP.NET AJAX `ScriptManager`, który jest wymagany przez wszystkie elementy zestawu narzędzi ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="1cf14-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="1cf14-126">Zapisz i Uruchom stronę: można zaznaczyć i usunąć zaznaczenie obu pól wyboru, jednak w żadnym momencie nie można sprawdzić obu pól wyboru.</span><span class="sxs-lookup"><span data-stu-id="1cf14-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="1cf14-127">[![można sprawdzić tylko jedno pole wyboru w czasie](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1cf14-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="1cf14-128">Tylko jedno pole wyboru może być zaznaczone naraz ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1cf14-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1cf14-129">Wstecz</span><span class="sxs-lookup"><span data-stu-id="1cf14-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
