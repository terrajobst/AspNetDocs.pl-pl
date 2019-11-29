---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Pozycjonowanie elementu kontrolki modalpopupC#() | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak formant nie oferuje...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8034f5aeb5a1a80f1ea8cbc9d638f3dfb1a38706
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599005"
---
# <a name="positioning-a-modalpopup-c"></a>Pozycjonowanie kontrolki ModalPopup (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak formant nie oferuje wbudowanej funkcji do pozycjonowania okna podręcznego.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak formant nie oferuje wbudowanej funkcji do pozycjonowania okna podręcznego.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji ASP.NET AJAX i zestawu narzędzi sterowania `ScriptManager`. Kontrolka musi być umieszczona w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Następnie Dodaj panel, który służy jako modalne menu podręczne. Przycisk jest używany do zamykania okna podręcznego:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Gdy zostanie wyświetlone okno podręczne, będzie ono umieszczane w określonym miejscu na stronie. W przypadku tego zadania zostanie utworzona funkcja JavaScript po stronie klienta. Najpierw próbuje uzyskać dostęp do panelu. Jeśli to się powiedzie, pozycja panelu jest ustawiana za pomocą CSS i JavaScript (Zmień położenie okna podręcznego w programie). Jednak formant `ModalPopupExtender` próbuje również umieścić okno podręczne. W związku z tym kod JavaScript wielokrotnie umieszcza okno podręczne, co dziesiąte części sekundy.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Jak widać, wartość zwracana `setTimeout()` Metoda JavaScript jest zapisywana w zmiennej globalnej. Pozwala to zatrzymać powtarzające się pozycjonowanie okna podręcznego na żądanie przy użyciu metody `clearTimeout()`:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Teraz wszystko, co pozostało do zrobienia, ma mieć możliwość wywołania tych funkcji w przeglądarce w miarę potrzeb. Funkcja `movePanel()` JavaScript musi być wywoływana, gdy kliknięto przycisk, który wyzwala panel:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

A funkcja `stopMoving()` jest odtwarzana po zamknięciu okna podręcznego, może być wyzwalane w kontrolce `ModalPopupExtender`:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]

[![modalny podręczny jest wyświetlany w wyznaczeniu pozycji](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Modalne menu podręczne pojawia się w wydzielonym miejscu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](handling-postbacks-from-a-modalpopup-cs.md)
> [dalej](launching-a-modal-popup-window-from-server-code-vb.md)
