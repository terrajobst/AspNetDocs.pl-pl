---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Tworzenie wzajemnie wykluczających sięC#pól wyboru () | Microsoft Docs
author: wenz
description: 'Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe. Istnieje zwrot, chociaż: wybrano jeden przycisk radiowy w grupie,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606501"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="19909-104">Tworzenie wzajemnie wykluczających się pól wyboru (C#)</span><span class="sxs-lookup"><span data-stu-id="19909-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>

<span data-ttu-id="19909-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="19909-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="19909-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="19909-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="19909-107">Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe.</span><span class="sxs-lookup"><span data-stu-id="19909-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="19909-108">Istnieje zwrot, chociaż: po wybraniu jednego przycisku radiowego w grupie nie można usunąć zaznaczenia wszystkich przycisków radiowych.</span><span class="sxs-lookup"><span data-stu-id="19909-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="19909-109">Pola wyboru można zaznaczać w dowolnym momencie, ale nie wykluczają się wzajemnie.</span><span class="sxs-lookup"><span data-stu-id="19909-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="19909-110">Ten samouczek zawiera najlepsze podejścia: pola wyboru, które wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="19909-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="19909-111">Omówienie</span><span class="sxs-lookup"><span data-stu-id="19909-111">Overview</span></span>

<span data-ttu-id="19909-112">Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe.</span><span class="sxs-lookup"><span data-stu-id="19909-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="19909-113">Istnieje zwrot, chociaż: po wybraniu jednego przycisku radiowego w grupie nie można usunąć zaznaczenia wszystkich przycisków radiowych.</span><span class="sxs-lookup"><span data-stu-id="19909-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="19909-114">Pola wyboru można zaznaczać w dowolnym momencie, ale nie wykluczają się wzajemnie.</span><span class="sxs-lookup"><span data-stu-id="19909-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="19909-115">Ten samouczek zawiera najlepsze podejścia: pola wyboru, które wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="19909-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="19909-116">Kroki</span><span class="sxs-lookup"><span data-stu-id="19909-116">Steps</span></span>

<span data-ttu-id="19909-117">Zestaw narzędzi ASP.NET AJAX Control Toolkit zawiera rozszerzenie MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="19909-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="19909-118">Dzięki temu programiści mogą przypisywać dowolne pola wyboru do nazwy grupy (`Key` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="19909-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="19909-119">Ze wszystkich pól wyboru w tej samej grupie można wybrać tylko jedną z nich.</span><span class="sxs-lookup"><span data-stu-id="19909-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="19909-120">Zacznijmy od umieszczenia dwóch pól wyboru na nowej stronie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="19909-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="19909-121">Może być więcej, ale dwa z nich wystarczą do zademonstrowania zasady:</span><span class="sxs-lookup"><span data-stu-id="19909-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="19909-122">Dla obu pól wyboru formant MutuallyExclusiveCheckBoxExtender musi być umieszczony na stronie.</span><span class="sxs-lookup"><span data-stu-id="19909-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="19909-123">Oba atrybuty klucza muszą mieć tę samą wartość, podobnie jak atrybuty wartości elementów przycisku radiowego HTML muszą być identyczne, aby można było zauważyć, do której grupy należą.</span><span class="sxs-lookup"><span data-stu-id="19909-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="19909-124">Właściwość TargetControlID elementu Extender wskazuje identyfikator pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="19909-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="19909-125">Na koniec Uwzględnij ASP.NET AJAX `ScriptManager`, który jest wymagany przez wszystkie elementy zestawu narzędzi ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="19909-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="19909-126">Zapisz i Uruchom stronę: można zaznaczyć i usunąć zaznaczenie obu pól wyboru, jednak w żadnym momencie nie można sprawdzić obu pól wyboru.</span><span class="sxs-lookup"><span data-stu-id="19909-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="19909-127">[![można sprawdzić tylko jedno pole wyboru w czasie](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="19909-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="19909-128">Tylko jedno pole wyboru może być zaznaczone naraz ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="19909-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="19909-129">Next</span><span class="sxs-lookup"><span data-stu-id="19909-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
