---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamiczne kontrolowanie animacji UpdatePanelC#() | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Dla zawartości...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 183974564764aab9c0d8a4e577995f3c444bf2d3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606795"
---
# <a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="7ec58-104">Dynamiczne kontrolowanie animacji kontrolki UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="7ec58-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>

<span data-ttu-id="7ec58-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7ec58-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7ec58-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7ec58-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="7ec58-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="7ec58-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7ec58-108">W przypadku zawartości elementu UpdatePanel istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="7ec58-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="7ec58-109">Może również współdziałać z wyzwalaczami UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="7ec58-109">It can also work together with UpdatePanel triggers.</span></span>

## <a name="overview"></a><span data-ttu-id="7ec58-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7ec58-110">Overview</span></span>

<span data-ttu-id="7ec58-111">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="7ec58-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7ec58-112">W przypadku zawartości `UpdatePanel`istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="7ec58-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="7ec58-113">Może również współdziałać z wyzwalaczami `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="7ec58-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="7ec58-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="7ec58-114">Steps</span></span>

<span data-ttu-id="7ec58-115">Pierwszym krokiem jest zwykle uwzględnienie `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:</span><span class="sxs-lookup"><span data-stu-id="7ec58-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="7ec58-116">Animacja w tym scenariuszu zostanie zastosowana do wyświetlania bieżącego czasu.</span><span class="sxs-lookup"><span data-stu-id="7ec58-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="7ec58-117">Te informacje mogą być zapisywane w etykiecie przy użyciu metody `Page_Load()` lub (dla uproszczenia) użyto następującego kodu wbudowanego:</span><span class="sxs-lookup"><span data-stu-id="7ec58-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="7ec58-118">Ponadto zostanie utworzony przycisk służący do wyzwalania aktualizacji czasu:</span><span class="sxs-lookup"><span data-stu-id="7ec58-118">Also, a button to trigger updating the time is created:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="7ec58-119">Ten kod jest następnie umieszczany w sekcji `<ContentTemplate>` elementu `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="7ec58-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="7ec58-120">Atrybut `UpdateMode` panelu musi być ustawiony na `"Conditional"`, ponieważ tylko Wyzwalacze mogą zaktualizować zawartość panelu.</span><span class="sxs-lookup"><span data-stu-id="7ec58-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="7ec58-121">W `<Triggers>` sekcji `UpdatePanel`zostanie utworzony asynchroniczny wyzwalacz ogłaszania zwrotnego i powiązany z zdarzeniem `Click` przycisku.</span><span class="sxs-lookup"><span data-stu-id="7ec58-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="7ec58-122">W takim przypadku, jeśli użytkownik kliknie przycisk, `UpdatePanel` jest odświeżane.</span><span class="sxs-lookup"><span data-stu-id="7ec58-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="7ec58-123">Oto znaczniki dla kontrolki `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="7ec58-123">Here is the markup for the `UpdatePanel` control:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="7ec58-124">Na koniec należy skonfigurować `UpdatePanelAnimationExtender`: ustaw atrybut `TargetControlID` na identyfikator panelu i zdefiniuj animację w ramach rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="7ec58-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="7ec58-125">Zanikanie w systemie ma sens, co sprawia, że jest to bardzo atrakcyjny wizualny nacisk na zaktualizowany czas.</span><span class="sxs-lookup"><span data-stu-id="7ec58-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="7ec58-126">Znaczniki rozszerzające mogą następnie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="7ec58-126">Your extender markup may then look like this:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="7ec58-127">Uruchom plik w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="7ec58-127">Run the file in the browser.</span></span> <span data-ttu-id="7ec58-128">Za każdym razem, gdy klikniesz przycisk, bieżący czas jest pokazywany w panelu, zawsze zanika w czasie trwania jednej sekundy.</span><span class="sxs-lookup"><span data-stu-id="7ec58-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>

<span data-ttu-id="7ec58-129">[![bieżący czas zanika w](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7ec58-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="7ec58-130">Bieżąca godzina powoduje zanikanie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7ec58-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7ec58-131">[Poprzednie](animating-an-updatepanel-control-cs.md)
> [dalej](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7ec58-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
