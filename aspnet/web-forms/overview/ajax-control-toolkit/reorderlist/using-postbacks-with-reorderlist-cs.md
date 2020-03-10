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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627233"
---
# <a name="using-postbacks-with-reorderlist-c"></a>Używanie ogłaszania zwrotnego z kontrolką ReorderList (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> Formant kontrolki reorderlist w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika. Za każdym razem, gdy lista zostanie zmieniona, ogłoszenie zwrotne informuje serwer o zmianie.

## <a name="overview"></a>Omówienie

Formant `ReorderList` w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika. Za każdym razem, gdy lista zostanie zmieniona, ogłoszenie zwrotne informuje serwer o zmianie.

## <a name="steps"></a>Kroki

Istnieje kilka możliwych źródeł danych dla kontrolki `ReorderList`. Jednym z nich jest użycie kontrolki `XmlDataSource`:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Aby powiązać ten plik XML z kontrolką `ReorderList` i włączyć ogłaszanie zwrotne, należy ustawić następujące atrybuty:

- `DataSourceID`: identyfikator źródła danych
- `SortOrderField`: właściwość, według której ma zostać wykonane sortowanie
- `AllowReorder`: czy zezwolić użytkownikowi na zmianę kolejności elementów listy
- `PostBackOnReorder`: czy należy utworzyć ogłaszanie zwrotne za każdym razem, gdy lista zostanie przestawiona

Oto odpowiednie znaczniki dla kontrolki:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

W kontrolce `ReorderList` określone dane ze źródła danych mogą być powiązane przy użyciu metody `Eval()`:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

W dowolnym miejscu na stronie etykieta będzie zawierać informacje w przypadku wystąpienia ostatniej zmiany kolejności:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Etykieta jest wypełniana tekstem w kodzie po stronie serwera, z obsługą ogłaszania zwrotnego:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Na koniec w celu aktywowania funkcji ASP.NET AJAX i zestawu narzędzi sterowania należy umieścić na stronie formant `ScriptManager`:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

[![każda zmiana kolejności wyzwala ogłaszanie zwrotne](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Każda zmiana kolejności wyzwala ogłaszanie zwrotne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Dalej](drag-and-drop-via-reorderlist-cs.md)
