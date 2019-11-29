---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Uruchamianie modalnego okna podręcznego z koduC#serwera () | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak niektóre scenariusze wymagają, aby t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: fec0ce2cdd24333f65201301718440e1a09d930e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599043"
---
# <a name="launching-a-modal-popup-window-from-server-code-c"></a>Uruchamianie modalnego okna podręcznego z kodu serwera (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak niektóre scenariusze wymagają, aby otwierając modalne menu podręczne zostało wyzwolone po stronie serwera.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak niektóre scenariusze wymagają, aby otwierając modalne menu podręczne zostało wyzwolone po stronie serwera.

## <a name="steps"></a>Kroki

Pierwszym z nich jest wymagana kontrolka sieci Web ASP.NET Button, aby zademonstrować sposób działania kontrolki kontrolki modalpopup. Dodaj taki przycisk w formularzu &lt;&gt; elementu na nowej stronie:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

Następnie potrzebujesz znacznika dla tworzonego okna podręcznego. Zdefiniuj ją jako kontrolkę `<asp:Panel>` i upewnij się, że zawiera ona kontrolkę Button. Kontrolka kontrolki modalpopup oferuje funkcje, które umożliwiają podjęcie takiego przycisku w celu zamknięcia okna podręcznego; w przeciwnym razie nie istnieje łatwy sposób, aby to umożliwić.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

Następnie Dodaj kontrolkę kontrolki modalpopup z zestawu narzędzi ASP.NET AJAX do strony. Ustaw właściwości przycisku, który ładuje formant, przycisk, który go znika, i identyfikator rzeczywistego okna podręcznego.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Tak jak w przypadku wszystkich stron sieci Web opartych na ASP.NET AJAX; Menedżer skryptów jest wymagany do załadowania niezbędnych bibliotek JavaScript dla różnych przeglądarek docelowych:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Uruchom przykład w przeglądarce. Po kliknięciu przycisku zostanie wyświetlone modalne menu podręczne. Aby osiągnąć ten sam efekt przy użyciu kodu po stronie serwera, wymagany jest nowy przycisk:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Jak widać, kliknięcie przycisku spowoduje wygenerowanie ogłaszania zwrotnego i wykonanie `ServerButton_Click()` metody na serwerze. W tej metodzie funkcja języka JavaScript o nazwie `launchModal()` jest wykonywana jako dokładna, a funkcja JavaScript zostanie wykonana po załadowaniu strony:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

Zadanie `launchModal()` ma wyświetlać kontrolki modalpopup. Funkcja `launchModal()` jest wykonywana po załadowaniu kompletnej strony HTML. W tej chwili jednak ASP.NET AJAX Framework nie został jeszcze w pełni załadowany. W związku z tym funkcja `launchModal()` jedynie ustawia zmienną, którą formant kontrolki modalpopup musi być pokazywany w dalszej części:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

`pageLoad()` funkcja języka JavaScript to specjalna funkcja, która jest wykonywana po całkowitym załadowaniu ASP.NET AJAX. W związku z tym dodamy kod do tej funkcji, aby pokazać formant kontrolki modalpopup, ale tylko wtedy, gdy `launchModal()` został wywołany przed:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

Funkcja `$find()` szuka nazwanego elementu na stronie i oczekuje identyfikatora po stronie serwera jako parametru. W związku z tym `$find("mpe")` zwraca reprezentację klienta formantu kontrolki modalpopup; jego `show()` Metoda umożliwia wyświetlenie okna podręcznego.

[![modalne okno podręczne pojawia się po kliknięciu jednego z przycisków](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Modalne okno podręczne pojawia się po kliknięciu jednego z przycisków ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-modalpopup-with-a-repeater-control-cs.md)
