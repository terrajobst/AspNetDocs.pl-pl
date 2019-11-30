---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulowanie właściwościami DropShadow z poziomu koduC#klienta () | Microsoft Docs
author: wenz
description: Dostosowywanie interfejsu edycji elementu DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574089"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="d6969-103">Manipulowanie właściwościami kontrolki DropShadow z poziomu kodu klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="d6969-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>

<span data-ttu-id="d6969-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d6969-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d6969-105">[Pobierz kod](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d6969-105">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="d6969-106">Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem.</span><span class="sxs-lookup"><span data-stu-id="d6969-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d6969-107">Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="d6969-107">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="d6969-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d6969-108">Overview</span></span>

<span data-ttu-id="d6969-109">Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem.</span><span class="sxs-lookup"><span data-stu-id="d6969-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d6969-110">Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="d6969-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="d6969-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="d6969-111">Steps</span></span>

<span data-ttu-id="d6969-112">Kod jest uruchamiany z panelem zawierającym kilka wierszy tekstu:</span><span class="sxs-lookup"><span data-stu-id="d6969-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="d6969-113">Skojarzona Klasa CSS daje panelowi całkiem kolor tła:</span><span class="sxs-lookup"><span data-stu-id="d6969-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="d6969-114">`DropShadowExtender` zostanie dodany, aby zwiększyć panel z efektem cienia, nieprzezroczystość ustawiona na 50%:</span><span class="sxs-lookup"><span data-stu-id="d6969-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="d6969-115">Następnie formant `ScriptManager` AJAX ASP.NET umożliwia działanie zestawu narzędzi sterowania:</span><span class="sxs-lookup"><span data-stu-id="d6969-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="d6969-116">Inny panel zawiera dwa linki JavaScript do ustawiania nieprzezroczystości cieniowania: łącze minus zmniejsza nieprzezroczystość w tle, a link Plus go zwiększa.</span><span class="sxs-lookup"><span data-stu-id="d6969-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="d6969-117">Funkcja JavaScript `changeOpacity()` musi najpierw znaleźć kontrolkę `DropShadowExtender` na stronie.</span><span class="sxs-lookup"><span data-stu-id="d6969-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="d6969-118">ASP.NET AJAX definiuje metodę `$find()` dla tego zadania.</span><span class="sxs-lookup"><span data-stu-id="d6969-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="d6969-119">Następnie Metoda `get_Opacity()` pobiera bieżącą nieprzezroczystość, Metoda `set_Opacity()` ustawia ją.</span><span class="sxs-lookup"><span data-stu-id="d6969-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="d6969-120">Kod JavaScript umieszcza bieżącą wartość nieprzezroczystości w `<label>` elemencie:</span><span class="sxs-lookup"><span data-stu-id="d6969-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

<span data-ttu-id="d6969-121">[![na stronie klienta zmieniono nieprzezroczystość](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d6969-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="d6969-122">Nieprzezroczystość jest zmieniana po stronie klienta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d6969-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d6969-123">[Poprzednie](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [dalej](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d6969-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
