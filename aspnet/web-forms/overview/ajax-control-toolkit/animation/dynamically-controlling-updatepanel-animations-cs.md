---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamiczne kontrolowanie animacji kontrolki UpdatePanel (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Dla zawartości...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 29c2673aa7b018cbe8c66eb72c256b69a2193a47
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65134053"
---
# <a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="92655-104">Dynamiczne kontrolowanie animacji kontrolki UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="92655-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>

<span data-ttu-id="92655-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="92655-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="92655-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="92655-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="92655-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="92655-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="92655-108">Dla zawartości UpdatePanel specjalne rozszerzenia istnieje już intensywnie korzystającej z framework animacji: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="92655-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="92655-109">Może również współdziałać, wraz z wyzwalaczy UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="92655-109">It can also work together with UpdatePanel triggers.</span></span>

## <a name="overview"></a><span data-ttu-id="92655-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="92655-110">Overview</span></span>

<span data-ttu-id="92655-111">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="92655-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="92655-112">Dla zawartości `UpdatePanel`, specjalne urządzenia extender istnieje która intensywnie korzysta z framework animacji: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="92655-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="92655-113">Może również współpracować wraz z `UpdatePanel` wyzwalaczy.</span><span class="sxs-lookup"><span data-stu-id="92655-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="92655-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="92655-114">Steps</span></span>

<span data-ttu-id="92655-115">Pierwszym krokiem jest jak zwykle obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:</span><span class="sxs-lookup"><span data-stu-id="92655-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="92655-116">Animacja w tym scenariuszu zostaną zastosowane do wyświetlenia bieżącej godziny.</span><span class="sxs-lookup"><span data-stu-id="92655-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="92655-117">Te informacje mogą być zapisywane w etykiecie, używając `Page_Load()` metodę, lub (dla uproszczenia należy) jest używany następujący kod wbudowany:</span><span class="sxs-lookup"><span data-stu-id="92655-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="92655-118">Ponadto jest tworzony przycisk do wyzwalania aktualizacji w czasie:</span><span class="sxs-lookup"><span data-stu-id="92655-118">Also, a button to trigger updating the time is created:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="92655-119">Następnie umieść ten kod `<ContentTemplate>` części `UpdatePanel` elementu.</span><span class="sxs-lookup"><span data-stu-id="92655-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="92655-120">Panel `UpdateMode` atrybutu musi być równa `"Conditional"`, ponieważ tylko wyzwalaczy może aktualizować zawartość panelu.</span><span class="sxs-lookup"><span data-stu-id="92655-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="92655-121">W `<Triggers>` części `UpdatePanel`, asynchronicznego zwrotu wyzwalacza jest tworzony i powiązane `Click` zdarzenia przycisku.</span><span class="sxs-lookup"><span data-stu-id="92655-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="92655-122">Dlatego, jeśli użytkownik kliknie przycisk, `UpdatePanel` jest odświeżane.</span><span class="sxs-lookup"><span data-stu-id="92655-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="92655-123">Oto znaczniki dla `UpdatePanel` sterowania:</span><span class="sxs-lookup"><span data-stu-id="92655-123">Here is the markup for the `UpdatePanel` control:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="92655-124">Na koniec `UpdatePanelAnimationExtender` muszą być skonfigurowane: Ustaw `TargetControlID` atrybutu ID panelu i zdefiniuj animacji w ramach urządzenia extender.</span><span class="sxs-lookup"><span data-stu-id="92655-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="92655-125">Rozjaśnianie sprawia, że sens, co powoduje utworzenie nieuprzywilejowany visual większym naciskiem na czas aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="92655-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="92655-126">Rozszerzenia znaczników może następnie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="92655-126">Your extender markup may then look like this:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="92655-127">Uruchom plik w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="92655-127">Run the file in the browser.</span></span> <span data-ttu-id="92655-128">Zawsze, gdy klikniesz przycisk, bieżący czas jest wyświetlany w panelu zawsze zanikania przy czasie trwania jednej sekundy.</span><span class="sxs-lookup"><span data-stu-id="92655-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>

<span data-ttu-id="92655-129">[![Stopniowe bieżący czas](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="92655-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="92655-130">Bieżący czas jest stopniowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="92655-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="92655-131">[Poprzednie](animating-an-updatepanel-control-cs.md)
> [dalej](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="92655-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
