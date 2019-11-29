---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Obsługa ogłaszania zwrotnego z kontrolki modalpopupC#() | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Należy zwrócić szczególną uwagę, gdy pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599102"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a>Obsługiwanie ogłaszania zwrotnego w kontrolce ModalPopup (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Należy zwrócić szczególną uwagę na to, że po utworzeniu ogłaszania zwrotnego z poziomu okna podręcznego.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Należy zwrócić szczególną uwagę na to, że po utworzeniu ogłaszania zwrotnego z poziomu okna podręcznego.

## <a name="steps"></a>Kroki

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Następnie Dodaj panel, który służy jako modalne menu podręczne. W tym miejscu użytkownik może wprowadzić nazwę i adres e-mail. Przycisk służy do zamykania okna podręcznego i zapisywania informacji. Należy zauważyć, że atrybut `OnClick` jest ustawiony tak, aby ogłaszać zwrotne po kliknięciu tego przycisku:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

Sama strona składa się z dwóch etykiet dla dokładnie tych samych informacji: Nazwa i adres e-mail. Przycisk służy do wyzwalania modalnego okienka podręcznego:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Aby wyświetlić okno podręczne, Dodaj kontrolkę `ModalPopupExtender`. Ustaw atrybut `PopupControlID` na identyfikator panelu i `TargetControlID` na identyfikator przycisku:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Teraz za każdym razem, gdy zostanie kliknięty przycisk `Save` w modalnym menu podręcznym, zostanie uruchomiona Metoda `SaveData()` po stronie serwera. W tym miejscu można zapisać wprowadzone dane w magazynie danych. W celu uproszczenia nowe dane są po prostu wyprowadzane na etykiecie:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Ponadto kontrolki TextBox w modalnym menu podręcznym powinny być wypełnione bieżącą nazwą i adresem e-mail. Jednak jest to konieczne tylko w przypadku braku ogłaszania zwrotnego. W przypadku ogłaszania zwrotnego funkcja stanu ASP.NET automatycznie wypełni pola tekstowe odpowiednimi wartościami.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

[![modalne menu podręczne powoduje odświeżenie](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Modalne menu podręczne powoduje odświeżenie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-modalpopup-with-a-repeater-control-cs.md)
> [dalej](positioning-a-modalpopup-cs.md)
