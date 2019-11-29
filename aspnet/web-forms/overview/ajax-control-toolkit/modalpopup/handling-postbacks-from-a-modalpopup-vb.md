---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Obsługa ogłaszania zwrotnego z kontrolki modalpopup (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Należy zwrócić szczególną uwagę, gdy pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: df0b71b3e336a0d230869623473bdac24b3dd07b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606627"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a>Obsługiwanie ogłaszania zwrotnego w kontrolce ModalPopup (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Należy zwrócić szczególną uwagę na to, że po utworzeniu ogłaszania zwrotnego z poziomu okna podręcznego.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Należy zwrócić szczególną uwagę na to, że po utworzeniu ogłaszania zwrotnego z poziomu okna podręcznego.

## <a name="steps"></a>Kroki

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Następnie Dodaj panel, który służy jako modalne menu podręczne. W tym miejscu użytkownik może wprowadzić nazwę i adres e-mail. Przycisk służy do zamykania okna podręcznego i zapisywania informacji. Należy zauważyć, że atrybut `OnClick` jest ustawiony tak, aby ogłaszać zwrotne po kliknięciu tego przycisku:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

Sama strona składa się z dwóch etykiet dla dokładnie tych samych informacji: Nazwa i adres e-mail. Przycisk służy do wyzwalania modalnego okienka podręcznego:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Aby wyświetlić okno podręczne, Dodaj kontrolkę `ModalPopupExtender`. Ustaw atrybut `PopupControlID` na identyfikator panelu i `TargetControlID` na identyfikator przycisku:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Teraz za każdym razem, gdy zostanie kliknięty przycisk `Save` w modalnym menu podręcznym, zostanie uruchomiona Metoda `SaveData()` po stronie serwera. W tym miejscu można zapisać wprowadzone dane w magazynie danych. W celu uproszczenia nowe dane są po prostu wyprowadzane na etykiecie:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Ponadto kontrolki TextBox w modalnym menu podręcznym powinny być wypełnione bieżącą nazwą i adresem e-mail. Jednak jest to konieczne tylko w przypadku braku ogłaszania zwrotnego. W przypadku ogłaszania zwrotnego funkcja stanu ASP.NET automatycznie wypełni pola tekstowe odpowiednimi wartościami.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

[![modalne menu podręczne powoduje odświeżenie](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

Modalne menu podręczne powoduje odświeżenie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-modalpopup-with-a-repeater-control-vb.md)
> [dalej](positioning-a-modalpopup-vb.md)
