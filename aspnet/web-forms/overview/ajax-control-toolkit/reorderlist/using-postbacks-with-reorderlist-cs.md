---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Używanie ogłaszania zwrotnego z kontrolką ReorderList (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Zawsze, gdy lista jest zmieniana, zamówienia zakupu...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6f8f74b74080104980e1db866d695fe7c6d9d5fc
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393356"
---
# <a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="75013-104">Używanie ogłaszania zwrotnego z kontrolką ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="75013-104">Using Postbacks with ReorderList (C#)</span></span>

<span data-ttu-id="75013-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="75013-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="75013-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="75013-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="75013-107">Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania.</span><span class="sxs-lookup"><span data-stu-id="75013-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="75013-108">Zawsze, gdy lista jest zmieniana, odświeżenie strony informuje serwer zmiany.</span><span class="sxs-lookup"><span data-stu-id="75013-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="75013-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="75013-109">Overview</span></span>

<span data-ttu-id="75013-110">`ReorderList` Formantu w zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania.</span><span class="sxs-lookup"><span data-stu-id="75013-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="75013-111">Zawsze, gdy lista jest zmieniana, odświeżenie strony informuje serwer zmiany.</span><span class="sxs-lookup"><span data-stu-id="75013-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="75013-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="75013-112">Steps</span></span>

<span data-ttu-id="75013-113">Istnieje kilka źródeł danych dla `ReorderList` kontroli.</span><span class="sxs-lookup"><span data-stu-id="75013-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="75013-114">Jeden jest użycie `XmlDataSource` sterowania:</span><span class="sxs-lookup"><span data-stu-id="75013-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="75013-115">Aby powiązać plik XML do `ReorderList` musi być ustawiona kontroli i włączenie ogłaszania zwrotnego, następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="75013-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- `DataSourceID`<span data-ttu-id="75013-116">: Identyfikator źródła danych</span><span class="sxs-lookup"><span data-stu-id="75013-116">: The ID of the data source</span></span>
- `SortOrderField`<span data-ttu-id="75013-117">: Właściwości, aby posortować według</span><span class="sxs-lookup"><span data-stu-id="75013-117">: The property to sort by</span></span>
- `AllowReorder`<span data-ttu-id="75013-118">: Czy zezwalać na użytkownika zmienić kolejność elementów listy</span><span class="sxs-lookup"><span data-stu-id="75013-118">: Whether to allow the user to reorder the list elements</span></span>
- `PostBackOnReorder`<span data-ttu-id="75013-119">: Czy chcesz utworzyć ogłaszania zwrotnego w każdym przypadku, gdy lista jest grupowany</span><span class="sxs-lookup"><span data-stu-id="75013-119">: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="75013-120">Oto odpowiednie kodu znaczników kontrolki:</span><span class="sxs-lookup"><span data-stu-id="75013-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="75013-121">W ramach `ReorderList` kontrolki, określonych danych ze źródła danych można powiązać, przy użyciu `Eval()` metody:</span><span class="sxs-lookup"><span data-stu-id="75013-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="75013-122">W dowolnym położeniu na stronie etykiety będzie wyświetlał informacje po ostatniej zmiany kolejności wystąpił:</span><span class="sxs-lookup"><span data-stu-id="75013-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="75013-123">Ta etykieta jest wypełniany tekst w kodzie po stronie serwera, obsługa ogłaszania zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="75013-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="75013-124">Na koniec, aby aktywować tę funkcję ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się na stronie:</span><span class="sxs-lookup"><span data-stu-id="75013-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![E<span data-ttu-id="75013-125">Zmiana kolejności ach wyzwala ogłaszania zwrotnego]</span><span class="sxs-lookup"><span data-stu-id="75013-125">ach reordering triggers a postback]</span></span>(using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

<span data-ttu-id="75013-126">Każda zmiana kolejności wyzwala ogłaszania zwrotnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="75013-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="75013-127">Next</span><span class="sxs-lookup"><span data-stu-id="75013-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
