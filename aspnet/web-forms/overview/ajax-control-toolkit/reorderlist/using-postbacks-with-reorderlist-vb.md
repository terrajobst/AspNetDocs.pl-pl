---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Korzystanie z ogłaszania zwrotnego z kontrolki reorderlist (VB) | Microsoft Docs
author: wenz
description: Formant kontrolki reorderlist w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika. Za każdym razem, gdy lista zostanie zmieniona, a...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d6075e40df2c32df6c0d801243eff98fa7790b2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611379"
---
# <a name="using-postbacks-with-reorderlist-vb"></a>Używanie ogłaszania zwrotnego z kontrolką ReorderList (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> Formant kontrolki reorderlist w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika. Za każdym razem, gdy lista zostanie zmieniona, ogłoszenie zwrotne informuje serwer o zmianie.

## <a name="overview"></a>Omówienie

Formant `ReorderList` w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika. Za każdym razem, gdy lista zostanie zmieniona, ogłoszenie zwrotne informuje serwer o zmianie.

## <a name="steps"></a>Kroki

Istnieje kilka możliwych źródeł danych dla kontrolki `ReorderList`. Jednym z nich jest użycie kontrolki `XmlDataSource`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Aby powiązać ten plik XML z kontrolką `ReorderList` i włączyć ogłaszanie zwrotne, należy ustawić następujące atrybuty:

- `DataSourceID`: identyfikator źródła danych
- `SortOrderField`: właściwość, według której ma zostać wykonane sortowanie
- `AllowReorder`: czy zezwolić użytkownikowi na zmianę kolejności elementów listy
- `PostBackOnReorder`: czy należy utworzyć ogłaszanie zwrotne za każdym razem, gdy lista zostanie przestawiona

Oto odpowiednie znaczniki dla kontrolki:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

W kontrolce `ReorderList` określone dane ze źródła danych mogą być powiązane przy użyciu metody `Eval()`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

W dowolnym miejscu na stronie etykieta będzie zawierać informacje w przypadku wystąpienia ostatniej zmiany kolejności:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Etykieta jest wypełniana tekstem w kodzie po stronie serwera, z obsługą ogłaszania zwrotnego:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Na koniec w celu aktywowania funkcji ASP.NET AJAX i zestawu narzędzi sterowania należy umieścić na stronie formant `ScriptManager`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]

[![każda zmiana kolejności wyzwala ogłaszanie zwrotne](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Każda zmiana kolejności wyzwala ogłaszanie zwrotne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](drag-and-drop-via-reorderlist-cs.md)
> [dalej](drag-and-drop-via-reorderlist-vb.md)
