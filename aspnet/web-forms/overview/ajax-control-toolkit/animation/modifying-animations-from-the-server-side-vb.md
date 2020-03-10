---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modyfikowanie animacji po stronie serwera (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą również...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ebc311d1a931ad611d9556799c94440d41a9cf49
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598001"
---
# <a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="24e90-104">Modyfikowanie animacji po stronie serwera (VB)</span><span class="sxs-lookup"><span data-stu-id="24e90-104">Modifying Animations From The Server Side (VB)</span></span>

<span data-ttu-id="24e90-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="24e90-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="24e90-106">[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="24e90-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="24e90-107">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="24e90-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="24e90-108">Animacje można także zmienić po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="24e90-108">The animations may also be changed on the server-side</span></span>

## <a name="overview"></a><span data-ttu-id="24e90-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="24e90-109">Overview</span></span>

<span data-ttu-id="24e90-110">Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="24e90-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="24e90-111">Animacje można także zmienić po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="24e90-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="24e90-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="24e90-112">Steps</span></span>

<span data-ttu-id="24e90-113">Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="24e90-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="24e90-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="24e90-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="24e90-115">W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="24e90-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="24e90-116">Pozostała część kodu jest uruchamiana po stronie serwera i nie używa znaczników; Zamiast tego używa kodu do utworzenia formantu `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="24e90-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="24e90-117">Jednak zestaw narzędzi kontroli nie zapewnia obecnie dostępu do interfejsu API w celu utworzenia pojedynczych animacji.</span><span class="sxs-lookup"><span data-stu-id="24e90-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="24e90-118">Można jednak ustawić właściwość animacje `AnimationExtender`na ciąg zawierający znacznik XML, używany podczas bardziej przypisywania animacji.</span><span class="sxs-lookup"><span data-stu-id="24e90-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="24e90-119">W celu utworzenia pliku XML, który nie może zawierać elementu `<Animations>`, można użyć obsługi XML .NET Framework lub, jak w poniższym kodzie, wystarczy podać ciąg:</span><span class="sxs-lookup"><span data-stu-id="24e90-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="24e90-120">Na koniec Dodaj formant `AnimationExtender` do bieżącej strony w obrębie elementu `<form runat="server">`, aby upewnić się, że animacja jest dołączona i uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="24e90-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]

<span data-ttu-id="24e90-121">[![animacja jest tworzona przy użyciu kodu/VB po C#stronie serwera](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="24e90-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="24e90-122">Animacja jest tworzona przy użyciu kodu/VB po C#stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="24e90-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="24e90-123">[Poprzednie](triggering-an-animation-in-another-control-vb.md)
> [dalej](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="24e90-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
