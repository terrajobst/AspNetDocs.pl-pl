---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Używanie ogłaszania zwrotnego z kontrolką ReorderList (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Zawsze, gdy lista jest zmieniana, zamówienia zakupu...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e2124327a36c94db8bafbdf91f767068ac7e834
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57066242"
---
<a name="using-postbacks-with-reorderlist-vb"></a>Używanie ogłaszania zwrotnego z kontrolką ReorderList (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Zawsze, gdy lista jest zmieniana, odświeżenie strony informuje serwer zmiany.


## <a name="overview"></a>Omówienie

`ReorderList` Formantu w zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Zawsze, gdy lista jest zmieniana, odświeżenie strony informuje serwer zmiany.

## <a name="steps"></a>Kroki

Istnieje kilka źródeł danych dla `ReorderList` kontroli. Jeden jest użycie `XmlDataSource` sterowania:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Aby powiązać plik XML do `ReorderList` musi być ustawiona kontroli i włączenie ogłaszania zwrotnego, następujące atrybuty:

- `DataSourceID`: Identyfikator źródła danych
- `SortOrderField`: Właściwości, aby posortować według
- `AllowReorder`: Czy zezwalać na użytkownika zmienić kolejność elementów listy
- `PostBackOnReorder`: Czy chcesz utworzyć ogłaszania zwrotnego w każdym przypadku, gdy lista jest grupowany

Oto odpowiednie kodu znaczników kontrolki:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

W ramach `ReorderList` kontrolki, określonych danych ze źródła danych można powiązać, przy użyciu `Eval()` metody:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

W dowolnym położeniu na stronie etykiety będzie wyświetlał informacje po ostatniej zmiany kolejności wystąpił:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Ta etykieta jest wypełniany tekst w kodzie po stronie serwera, obsługa ogłaszania zwrotnego:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Na koniec, aby aktywować tę funkcję ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się na stronie:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Każda zmiana kolejności wyzwala ogłaszania zwrotnego](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Każda zmiana kolejności wyzwala ogłaszania zwrotnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](drag-and-drop-via-reorderlist-cs.md)
> [dalej](drag-and-drop-via-reorderlist-vb.md)
