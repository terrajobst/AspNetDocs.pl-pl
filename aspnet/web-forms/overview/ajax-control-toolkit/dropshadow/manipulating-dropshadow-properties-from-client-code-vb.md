---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulowanie właściwościami DropShadow z poziomu kodu klienta (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia. Właściwości tego rozszerzenia można także zmienić przy użyciu klienta JavaScrip...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c44a1e95564c668f017f6116f3e62652e87eeac
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65116949"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="5c77e-104">Manipulowanie właściwościami kontrolki DropShadow z poziomu kodu klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="5c77e-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>

<span data-ttu-id="5c77e-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5c77e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5c77e-106">[Pobierz program Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5c77e-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="5c77e-107">Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia.</span><span class="sxs-lookup"><span data-stu-id="5c77e-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="5c77e-108">Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="5c77e-108">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="5c77e-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5c77e-109">Overview</span></span>

<span data-ttu-id="5c77e-110">Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia.</span><span class="sxs-lookup"><span data-stu-id="5c77e-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="5c77e-111">Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="5c77e-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="5c77e-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="5c77e-112">Steps</span></span>

<span data-ttu-id="5c77e-113">Panel, zawierający kilka wierszy tekstu zaczyna się kod:</span><span class="sxs-lookup"><span data-stu-id="5c77e-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="5c77e-114">Skojarzona klasa CSS zapewnia panelu Kolor tła nieuprzywilejowany:</span><span class="sxs-lookup"><span data-stu-id="5c77e-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="5c77e-115">`DropShadowExtender` Jest dodawany do rozszerzenia panelu z efektem cienia, nieprzezroczystość równa 50%:</span><span class="sxs-lookup"><span data-stu-id="5c77e-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="5c77e-116">Następnie ASP.NET AJAX `ScriptManager` control umożliwia kontrolki zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="5c77e-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="5c77e-117">Kolejny panel zawiera dwa linki JavaScript do ustawiania Nieprzezroczystość cienia: minus link zmniejsza jego nieprzezroczystości, oraz link jej zwiększenie.</span><span class="sxs-lookup"><span data-stu-id="5c77e-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="5c77e-118">Funkcja języka JavaScript `changeOpacity()` następnie musi najpierw odnaleźć `DropShadowExtender` formantu na stronie.</span><span class="sxs-lookup"><span data-stu-id="5c77e-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="5c77e-119">Definiuje ASP.NET AJAX `$find()` metody do dokładnie tego zadania.</span><span class="sxs-lookup"><span data-stu-id="5c77e-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="5c77e-120">Następnie `get_Opacity()` metoda pobiera bieżący nieprzezroczystość `set_Opacity()` metody ustawia ją.</span><span class="sxs-lookup"><span data-stu-id="5c77e-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="5c77e-121">Następnie kod JavaScript umieszcza bieżącą wartość nieprzezroczystości w `<label>` elementu:</span><span class="sxs-lookup"><span data-stu-id="5c77e-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

<span data-ttu-id="5c77e-122">[![Nieprzezroczystość zostanie zmieniony po stronie klienta](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5c77e-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="5c77e-123">Nieprzezroczystość zostanie zmieniony po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5c77e-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5c77e-124">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="5c77e-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
