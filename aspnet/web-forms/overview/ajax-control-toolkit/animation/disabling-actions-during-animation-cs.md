---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Wyłączanie akcji podczas animacji (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Obsługuje również akcję...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536268"
---
# <a name="disabling-actions-during-animation-c"></a><span data-ttu-id="e8bcc-104">Wyłączanie akcji podczas animacji (C#)</span><span class="sxs-lookup"><span data-stu-id="e8bcc-104">Disabling Actions during Animation (C#)</span></span>

<span data-ttu-id="e8bcc-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e8bcc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e8bcc-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e8bcc-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="e8bcc-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e8bcc-108">Obsługuje także akcje, takie jak kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="e8bcc-109">Jednak po kliknięciu przycisku myszy uruchamia animację, pożądane jest wyłączenie kliknięć myszą podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="e8bcc-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="e8bcc-110">Overview</span></span>

<span data-ttu-id="e8bcc-111">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e8bcc-112">Obsługuje także akcje, takie jak kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="e8bcc-113">Jednak po kliknięciu przycisku myszy uruchamia animację, pożądane jest wyłączenie kliknięć myszą podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="e8bcc-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="e8bcc-114">Steps</span></span>

<span data-ttu-id="e8bcc-115">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="e8bcc-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="e8bcc-116">Animacja zostanie zastosowana do przycisku HTML w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e8bcc-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="e8bcc-117">Należy zauważyć, że kontrolka HTML jest używana zamiast kontrolki sieci Web, ponieważ nie chcę, aby przycisk utworzył ogłaszanie zwrotne; po prostu uruchamia dla nas animację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="e8bcc-118">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="e8bcc-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="e8bcc-119">W węźle `<Animations>` `<OnClick>` jest prawym elementem, aby obsłużyć kliknięcie myszą.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="e8bcc-120">Jednak przycisk można kliknąć również podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="e8bcc-121">Element `<EnableAction>` może zadbać o to.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="e8bcc-122">Ustawienie `Enabled="false"` wyłącza przycisk w ramach animacji.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="e8bcc-123">Ponieważ korzystamy z kilku indywidualnych animacji (wyłączenie przycisku i rzeczywistych animacji), element `<Parallel>` jest wymagany do przyklejania pojedynczych animacji w jeden.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="e8bcc-124">Oto kompletny znacznik dla `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="e8bcc-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="e8bcc-125">Możliwe jest również ponowne włączenie do przycisku po animacji, przy użyciu następującego elementu XML na końcu listy:</span><span class="sxs-lookup"><span data-stu-id="e8bcc-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="e8bcc-126">Jednak w danym scenariuszu może być bezużyteczny, ponieważ przycisk znika i nie jest widoczny na końcu animacji.</span><span class="sxs-lookup"><span data-stu-id="e8bcc-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="e8bcc-127">[![przycisk jest wyłączony zaraz po uruchomieniu animacji](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e8bcc-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="e8bcc-128">Przycisk jest wyłączony zaraz po uruchomieniu animacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e8bcc-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e8bcc-129">[Poprzednie](animating-in-response-to-user-interaction-cs.md)
> [dalej](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e8bcc-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
