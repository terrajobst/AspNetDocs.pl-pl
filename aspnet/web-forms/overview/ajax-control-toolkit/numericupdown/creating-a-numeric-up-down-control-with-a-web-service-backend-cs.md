---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Tworzenie numerycznej kontrolki up/down z zapleczem usługi sieciC#Web () | Microsoft Docs
author: wenz
description: Zamiast zezwalać użytkownikowi na wpisywanie wartości w polu wyboru, numeryczna kontrolka w górę/w dół (która istnieje w systemie Windows i innych systemach operacyjnych) może okazać się bardziej c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627352"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Tworzenie kontrolki Numeric Up/Down z zapleczem usługi internetowej (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Zamiast zezwalać użytkownikowi na wpisywanie wartości w polu wyboru, kontrolka w górę/w dół (która istnieje w systemie Windows i innych systemach operacyjnych) może być bardziej wygodna. Domyślnie formant NumericUpDown zawsze zwiększa lub zmniejsza wartość o 1, ale usługa sieci Web zapewnia większą elastyczność.

## <a name="overview"></a>Omówienie

Zamiast zezwalać użytkownikowi na wpisywanie wartości w polu wyboru, kontrolka w górę/w dół (która istnieje w systemie Windows i innych systemach operacyjnych) może być bardziej wygodna. Domyślnie formant `NumericUpDown` zawsze zwiększa lub zmniejsza wartość o 1, ale usługa sieci Web zapewnia większą elastyczność.

## <a name="steps"></a>Kroki

Zestaw narzędzi ASP.NET AJAX Control Toolkit zawiera rozszerzenie `NumericUpDown`, które automatycznie dodaje dwa przyciski do pola tekstowego: jeden w celu zwiększenia jego wartości, jeden do jego zmniejszenia. Jednak kontrolka obsługuje również wywołanie usługi sieci Web (lub wywołanie metody strony). Za każdym razem, gdy zostanie kliknięty przycisk w górę lub w dół, kod JavaScript nawiązuje połączenie z serwerem sieci Web i wykonuje metodę. Podpis metody jest następujący:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

`current` argument jest bieżącą wartością w polu tekstowym; atrybut `tag` to dodatkowe dane kontekstowe, które można ustawić jako właściwość rozszerzenia `NumericUpDown` (ale nie jest to wymagane).

W tym przykładzie numeryczna kontrolka up/down zezwala tylko na wartości, które są następujące: 1, 2, 4, 8, 16, 32, 64 itd. W związku z tym Metoda wykonywana, gdy użytkownik chce zwiększyć wartość, musi być podwójnie stara wartość; Druga metoda musi dzielić wartość o dwa. Oto kompletna usługa sieci Web:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Na koniec Utwórz nową stronę ASP.NET. Jak zwykle potrzebna jest kontrolka `ScriptManager`, kontrolka `TextBox` i kontrolka `NumericUpDownExtender`. Dla tej ostatniej należy podać informacje o usłudze sieci Web:

- Nazwa `ServiceDownMethod` lub metoda strony sieci Web w dół
- `ServiceDownPath` ścieżkę do usługi sieci Web przy użyciu metody usługi w dół. Pomiń, jeśli używasz metody strony
- `ServiceUpMethod` nazwę metody sieci Web lub metody strony
- `ServiceUpPath` ścieżkę do usługi sieci Web przy użyciu metody usługi up; Pomiń, jeśli używasz metody strony

Oto kompletna Adiustacja strony:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Jeśli uruchomisz stronę, Zauważ, że wartość w polu tekstowym zawsze podwaja się po kliknięciu górnego przycisku i jest wyświetlana po kliknięciu dolnego przycisku.

[![wyświetlane są tylko liczby, które są potęgą 2](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Wyświetlane są tylko liczby, które są potęgą 2 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Dalej](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
