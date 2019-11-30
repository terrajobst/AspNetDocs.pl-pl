---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulowanie właściwościami DropShadow z poziomu kodu klienta (VB) | Microsoft Docs
author: wenz
description: Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Właściwości tego rozszerzenia można także zmienić za pomocą JavaScrip klienta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574029"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="8cadf-104">Manipulowanie właściwościami kontrolki DropShadow z poziomu kodu klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="8cadf-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>

<span data-ttu-id="8cadf-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8cadf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8cadf-106">[Pobierz kod](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8cadf-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="8cadf-107">Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem.</span><span class="sxs-lookup"><span data-stu-id="8cadf-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="8cadf-108">Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="8cadf-108">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="8cadf-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8cadf-109">Overview</span></span>

<span data-ttu-id="8cadf-110">Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem.</span><span class="sxs-lookup"><span data-stu-id="8cadf-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="8cadf-111">Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="8cadf-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="8cadf-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="8cadf-112">Steps</span></span>

<span data-ttu-id="8cadf-113">Kod jest uruchamiany z panelem zawierającym kilka wierszy tekstu:</span><span class="sxs-lookup"><span data-stu-id="8cadf-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="8cadf-114">Skojarzona Klasa CSS daje panelowi całkiem kolor tła:</span><span class="sxs-lookup"><span data-stu-id="8cadf-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="8cadf-115">`DropShadowExtender` zostanie dodany, aby zwiększyć panel z efektem cienia, nieprzezroczystość ustawiona na 50%:</span><span class="sxs-lookup"><span data-stu-id="8cadf-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="8cadf-116">Następnie formant `ScriptManager` AJAX ASP.NET umożliwia działanie zestawu narzędzi sterowania:</span><span class="sxs-lookup"><span data-stu-id="8cadf-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="8cadf-117">Inny panel zawiera dwa linki JavaScript do ustawiania nieprzezroczystości cieniowania: łącze minus zmniejsza nieprzezroczystość w tle, a link Plus go zwiększa.</span><span class="sxs-lookup"><span data-stu-id="8cadf-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="8cadf-118">Funkcja JavaScript `changeOpacity()` musi najpierw znaleźć kontrolkę `DropShadowExtender` na stronie.</span><span class="sxs-lookup"><span data-stu-id="8cadf-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="8cadf-119">ASP.NET AJAX definiuje metodę `$find()` dla tego zadania.</span><span class="sxs-lookup"><span data-stu-id="8cadf-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="8cadf-120">Następnie Metoda `get_Opacity()` pobiera bieżącą nieprzezroczystość, Metoda `set_Opacity()` ustawia ją.</span><span class="sxs-lookup"><span data-stu-id="8cadf-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="8cadf-121">Kod JavaScript umieszcza bieżącą wartość nieprzezroczystości w `<label>` elemencie:</span><span class="sxs-lookup"><span data-stu-id="8cadf-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

<span data-ttu-id="8cadf-122">[![na stronie klienta zmieniono nieprzezroczystość](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8cadf-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="8cadf-123">Nieprzezroczystość jest zmieniana po stronie klienta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8cadf-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8cadf-124">Ubiegł</span><span class="sxs-lookup"><span data-stu-id="8cadf-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
