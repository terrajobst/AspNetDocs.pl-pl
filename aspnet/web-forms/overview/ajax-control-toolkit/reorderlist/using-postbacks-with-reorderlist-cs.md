---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Korzystanie z ogłaszania zwrotnegoC#z kontrolki reorderlist () | Microsoft Docs
author: wenz
description: Formant kontrolki reorderlist w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika. Za każdym razem, gdy lista zostanie zmieniona, a...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: f83201fc6fd458e730b6bb5ffee184d303b52e90
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611389"
---
# <a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="a23bf-104">Używanie ogłaszania zwrotnego z kontrolką ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="a23bf-104">Using Postbacks with ReorderList (C#)</span></span>

<span data-ttu-id="a23bf-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a23bf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a23bf-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a23bf-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="a23bf-107">Formant kontrolki reorderlist w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a23bf-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="a23bf-108">Za każdym razem, gdy lista zostanie zmieniona, ogłoszenie zwrotne informuje serwer o zmianie.</span><span class="sxs-lookup"><span data-stu-id="a23bf-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="overview"></a><span data-ttu-id="a23bf-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a23bf-109">Overview</span></span>

<span data-ttu-id="a23bf-110">Formant `ReorderList` w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a23bf-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="a23bf-111">Za każdym razem, gdy lista zostanie zmieniona, ogłoszenie zwrotne informuje serwer o zmianie.</span><span class="sxs-lookup"><span data-stu-id="a23bf-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="a23bf-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="a23bf-112">Steps</span></span>

<span data-ttu-id="a23bf-113">Istnieje kilka możliwych źródeł danych dla kontrolki `ReorderList`.</span><span class="sxs-lookup"><span data-stu-id="a23bf-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="a23bf-114">Jednym z nich jest użycie kontrolki `XmlDataSource`:</span><span class="sxs-lookup"><span data-stu-id="a23bf-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="a23bf-115">Aby powiązać ten plik XML z kontrolką `ReorderList` i włączyć ogłaszanie zwrotne, należy ustawić następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="a23bf-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="a23bf-116">`DataSourceID`: identyfikator źródła danych</span><span class="sxs-lookup"><span data-stu-id="a23bf-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="a23bf-117">`SortOrderField`: właściwość, według której ma zostać wykonane sortowanie</span><span class="sxs-lookup"><span data-stu-id="a23bf-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="a23bf-118">`AllowReorder`: czy zezwolić użytkownikowi na zmianę kolejności elementów listy</span><span class="sxs-lookup"><span data-stu-id="a23bf-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="a23bf-119">`PostBackOnReorder`: czy należy utworzyć ogłaszanie zwrotne za każdym razem, gdy lista zostanie przestawiona</span><span class="sxs-lookup"><span data-stu-id="a23bf-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="a23bf-120">Oto odpowiednie znaczniki dla kontrolki:</span><span class="sxs-lookup"><span data-stu-id="a23bf-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="a23bf-121">W kontrolce `ReorderList` określone dane ze źródła danych mogą być powiązane przy użyciu metody `Eval()`:</span><span class="sxs-lookup"><span data-stu-id="a23bf-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="a23bf-122">W dowolnym miejscu na stronie etykieta będzie zawierać informacje w przypadku wystąpienia ostatniej zmiany kolejności:</span><span class="sxs-lookup"><span data-stu-id="a23bf-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="a23bf-123">Etykieta jest wypełniana tekstem w kodzie po stronie serwera, z obsługą ogłaszania zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="a23bf-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="a23bf-124">Na koniec w celu aktywowania funkcji ASP.NET AJAX i zestawu narzędzi sterowania należy umieścić na stronie formant `ScriptManager`:</span><span class="sxs-lookup"><span data-stu-id="a23bf-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

<span data-ttu-id="a23bf-125">[![każda zmiana kolejności wyzwala ogłaszanie zwrotne](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a23bf-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="a23bf-126">Każda zmiana kolejności wyzwala ogłaszanie zwrotne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a23bf-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a23bf-127">Next</span><span class="sxs-lookup"><span data-stu-id="a23bf-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
