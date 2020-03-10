---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Dodawanie walidacji do modelu | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543646"
---
# <a name="adding-validation-to-the-model"></a>Dodawanie walidacji do modelu

przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych. Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.

W tej sekcji zamierzamy wdrożyć pomoc techniczną, aby umożliwić weryfikację danych wejściowych w naszej aplikacji. Upewnij się, że zawartość bazy danych jest zawsze poprawna, i zapewnij użytkownikom końcowym pomocne komunikaty o błędach, a następnie wprowadź nieprawidłowe dane filmu. Zaczniemy od dodania niewielkiej logiki walidacji do klasy filmów.

Kliknij prawym przyciskiem myszy folder model i wybierz polecenie Dodaj klasę. Nazwij film klasy.

Po utworzeniu modelu obiektu filmu wcześniej środowisko IDE utworzyło klasę filmu. W rzeczywistości część klasy filmu może znajdować się w jednym pliku i części w innym. Ta nazwa jest nazywana klasą częściową. Zajmiemy się rozszerzaniem klasy filmu z innego pliku.

Utworzymy częściową klasę filmową, która wskazuje na "kolega klasy" z pewnymi atrybutami, które umożliwią wskazówkom walidacji w systemie. Oznaczę tytuł i cenę zgodnie z wymaganiami, a także zapewnią, że cena należy do określonego zakresu. Kliknij prawym przyciskiem myszy folder modele i wybierz polecenie Dodaj klasę. Nazwij film klasy, a następnie kliknij przycisk OK. Oto jak wygląda częściowa Klasa filmu.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Uruchom ponownie aplikację i spróbuj wprowadzić film o cenie powyżej 100. Po przesłaniu formularza wystąpi błąd. Ten błąd jest przechwytywany po stronie serwera i występuje po opublikowaniu formularza. Zwróć uwagę, jak wbudowane pomocniki HTML ASP.NET MVC były wystarczająco inteligentne, aby wyświetlić komunikat o błędzie i zachować wartości dla nas w elementach TextBox:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Jest to doskonałe rozwiązanie, ale warto poinstruować użytkownika po stronie klienta natychmiast, zanim serwer zostanie uwzględniony.

Włączmy kilka weryfikacji po stronie klienta przy użyciu języka JavaScript.

## <a name="adding-client-side-validation"></a>Dodawanie weryfikacji po stronie klienta

Ponieważ Klasa filmu ma już pewne atrybuty walidacji, wystarczy dodać kilka plików JavaScript do naszego szablonu Create. aspx, a następnie dodać wiersz kodu, aby włączyć weryfikację po stronie klienta.

Z poziomu programu pliku VWD przejdź do naszego folderu Viewss/Movie, a następnie otwórz pozycję Create. aspx.

Otwórz folder skryptów w Eksplorator rozwiązań i przeciągnij następujące trzy skrypty do wewnątrz tagu &lt;&gt;.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Te pliki skryptów mają być wyświetlane w tej kolejności.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Ponadto Dodaj ten pojedynczy wiersz powyżej pliku HTML. BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Oto kod wyświetlany w środowisku IDE.

[![filmów — Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Uruchom aplikację i odwiedź witrynę/Movies/Create ponownie, a następnie kliknij pozycję Utwórz bez wprowadzania żadnych danych. Komunikaty o błędach pojawiają się natychmiast bez strony Flash, która jest skojarzona z wysyłaniem danych z powrotem do serwera programu. Jest to spowodowane tym, że ASP.NET MVC sprawdza teraz dane wejściowe na kliencie (przy użyciu języka JavaScript) i na serwerze.

[![tworzenie — Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Wygląda to dobrze! Dodajmy teraz jedną dodatkową kolumnę do bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part6.md)
> [dalej](getting-started-with-mvc-part8.md)
