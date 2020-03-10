---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Wybieranie jednej animacji z listy (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Platforma nie umożliwia także...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2bc203d694792716bbf4f7d7b8d152c589bf99b1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597973"
---
# <a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="ee968-104">Wybieranie jednej animacji z listy (C#)</span><span class="sxs-lookup"><span data-stu-id="ee968-104">Picking One Animation Out Of a List (C#)</span></span>

<span data-ttu-id="ee968-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ee968-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ee968-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee968-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="ee968-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ee968-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ee968-108">Struktura umożliwia również programistom wybranie jednej animacji z listy animacji, w zależności od oceny kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee968-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="ee968-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ee968-109">Overview</span></span>

<span data-ttu-id="ee968-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ee968-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ee968-111">Struktura umożliwia również programistom wybranie jednej animacji z listy animacji, w zależności od oceny kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee968-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="ee968-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="ee968-112">Steps</span></span>

<span data-ttu-id="ee968-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="ee968-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="ee968-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="ee968-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="ee968-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="ee968-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="ee968-116">Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="ee968-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="ee968-117">W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="ee968-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="ee968-118">Zamiast jednego ze zwykłych animacji, element `<Case>` jest dostępny do odtworzenia.</span><span class="sxs-lookup"><span data-stu-id="ee968-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="ee968-119">Wartość atrybutu SelectScript jest oceniana; zwracana wartość musi być wartością numeryczną.</span><span class="sxs-lookup"><span data-stu-id="ee968-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="ee968-120">W zależności od tego numeru jest wykonywana jedna z animacji w &lt;&gt; przypadku.</span><span class="sxs-lookup"><span data-stu-id="ee968-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="ee968-121">Na przykład jeśli SelectScript oblicza wartość 2, zestaw narzędzi sterowania uruchamia trzecią animację w &lt;&gt; przypadku (zliczanie zaczyna się od 0).</span><span class="sxs-lookup"><span data-stu-id="ee968-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="ee968-122">Następujące oznakowanie definiuje trzy animacje: zmienianie rozmiarów szerokości, zmianę rozmiarów i stopniowe Rozjaśnianie. Kod JavaScript (`Math.floor(3 * Math.random())`) następnie wybiera liczbę z zakresu od 0 do 2, aby jeden z trzech animacji był uruchamiany:</span><span class="sxs-lookup"><span data-stu-id="ee968-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]

<span data-ttu-id="ee968-123">[![jedną z możliwych trzech animacji: Panel jest szerszy](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ee968-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="ee968-124">Jedna z możliwych trzech animacji: Panel jest szerszy (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ee968-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ee968-125">[Poprzednie](animation-depending-on-a-condition-cs.md)
> [dalej](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ee968-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
