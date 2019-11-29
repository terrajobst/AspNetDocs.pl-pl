---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Uruchamianie modalnego okna podręcznego z kodu serwera (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak niektóre scenariusze wymagają, aby t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606602"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a>Uruchamianie modalnego okna podręcznego z kodu serwera (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak niektóre scenariusze wymagają, aby otwierając modalne menu podręczne zostało wyzwolone po stronie serwera.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Jednak niektóre scenariusze wymagają, aby otwierając modalne menu podręczne zostało wyzwolone po stronie serwera.

## <a name="steps"></a>Kroki

Pierwszym z nich jest wymagana kontrolka sieci Web ASP.NET Button, aby zademonstrować sposób działania kontrolki kontrolki modalpopup. Dodaj taki przycisk w formularzu &lt;&gt; elementu na nowej stronie:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Następnie potrzebujesz znacznika dla tworzonego okna podręcznego. Zdefiniuj ją jako kontrolkę `<asp:Panel>` i upewnij się, że zawiera ona kontrolkę Button. Kontrolka kontrolki modalpopup oferuje funkcje, które umożliwiają podjęcie takiego przycisku w celu zamknięcia okna podręcznego; w przeciwnym razie nie istnieje łatwy sposób, aby to umożliwić.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Następnie Dodaj kontrolkę kontrolki modalpopup z zestawu narzędzi ASP.NET AJAX do strony. Ustaw właściwości przycisku, który ładuje formant, przycisk, który go znika, i identyfikator rzeczywistego okna podręcznego.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Tak jak w przypadku wszystkich stron sieci Web opartych na ASP.NET AJAX; Menedżer skryptów jest wymagany do załadowania niezbędnych bibliotek JavaScript dla różnych przeglądarek docelowych:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Uruchom przykład w przeglądarce. Po kliknięciu przycisku zostanie wyświetlone modalne menu podręczne. Aby osiągnąć ten sam efekt przy użyciu kodu po stronie serwera, wymagany jest nowy przycisk:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Jak widać, kliknięcie przycisku spowoduje wygenerowanie ogłaszania zwrotnego i wykonanie `ServerButton_Click()` metody na serwerze. W tej metodzie funkcja języka JavaScript o nazwie `launchModal()` jest wykonywana jako dokładna, a funkcja JavaScript zostanie wykonana po załadowaniu strony:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Zadanie `launchModal()` ma wyświetlać kontrolki modalpopup. Funkcja `launchModal()` jest wykonywana po załadowaniu kompletnej strony HTML. W tej chwili jednak ASP.NET AJAX Framework nie został jeszcze w pełni załadowany. W związku z tym funkcja `launchModal()` jedynie ustawia zmienną, którą formant kontrolki modalpopup musi być pokazywany w dalszej części:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` funkcja języka JavaScript to specjalna funkcja, która jest wykonywana po całkowitym załadowaniu ASP.NET AJAX. W związku z tym dodamy kod do tej funkcji, aby pokazać formant kontrolki modalpopup, ale tylko wtedy, gdy `launchModal()` został wywołany przed:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

Funkcja `$find()` szuka nazwanego elementu na stronie i oczekuje identyfikatora po stronie serwera jako parametru. W związku z tym `$find("mpe")` zwraca reprezentację klienta formantu kontrolki modalpopup; jego `show()` Metoda umożliwia wyświetlenie okna podręcznego.

[![modalne okno podręczne pojawia się po kliknięciu jednego z przycisków](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Modalne okno podręczne pojawia się po kliknięciu jednego z przycisków ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](positioning-a-modalpopup-cs.md)
> [dalej](using-modalpopup-with-a-repeater-control-vb.md)
