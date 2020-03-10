---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Tworzenie kontrolki oceny (C#) | Microsoft Docs
author: wenz
description: Wiele witryn sieci Web — od handlu elektronicznego do witryn społecznościowych oferuje użytkownikom możliwość oceniania artykułów lub przedmiotów. Zwykle wymaga to pewnego nakładu na kodowanie, ale mamy...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: c0bf793406e378321f54f0460031c526a0b41a02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612344"
---
# <a name="creating-a-rating-control-c"></a>Tworzenie kontrolki Rating (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> Wiele witryn sieci Web — od handlu elektronicznego do witryn społecznościowych oferuje użytkownikom możliwość oceniania artykułów lub przedmiotów. Zwykle wymaga to pewnego wysiłku związanego z kodowaniem, ale mamy do dyspozycji zestaw narzędzi kontroli.

## <a name="overview"></a>Omówienie

Wiele witryn sieci Web — od handlu elektronicznego do witryn społecznościowych oferuje użytkownikom możliwość oceniania artykułów lub przedmiotów. Zwykle wymaga to pewnego wysiłku związanego z kodowaniem, ale mamy do dyspozycji zestaw narzędzi kontroli.

## <a name="steps"></a>Kroki

Najpierw wystarczy (co najmniej) dwa rodzaje obrazów: jeden dla wypełnionego elementu oceny i jeden dla pustego elementu oceny. Element oceny jest zwykle gwiazdką lub uśmiechem. W tym scenariuszu znajdziesz trzy pliki, buźki. png i Empty. png i smiley-done. png w ramach pobierania kodu źródłowego dla tego samouczka.

Następnie utwórz nowy plik ASP.NET i zacznij od dodania do niego formantu `ScriptManager`:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

Następnie Dodaj formant `Rating` z zestawu narzędzi ASP.NET AJAX Control Toolkit. Następujące atrybuty muszą być ustawione na potrzeby tego przykładu:

- `CurrentRating` klasyfikacji początkowej, która ma zostać użyta
- `MaxRating` maksymalną klasyfikację
- `EmptyStarCssClass` klasy CSS, która ma być używana, gdy element klasyfikacji (gwiazdka) jest pusty
- `FilledStarCssClass` klasy CSS, która ma być używana w przypadku wypełnienia elementu klasyfikacji (gwiazdka)
- `StarCssClass` klasy CSS do użycia dla widocznego statu
- `WaitingStarCssClass` klasy CSS, która ma być używana podczas wysyłania do serwera klasyfikacji gwiazdek

A oto znacznik, który tworzy kontrolkę klasyfikacji z pięcioma elementami (uśmiechy), dla których żadna wartość nie jest początkowo wypełniana:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

Trzy klasy CSS, do których istnieją odwołania, teraz muszą wyświetlać odpowiednie pliki obrazów, co jest łatwe do użycia w CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Upewnij się, że podajesz szerokość i wysokość trzech obrazów, w przeciwnym razie może wyglądać bit popełniliśmy up.

Na koniec wynik oceny powinien zostać wyświetlony użytkownikowi (lub, co najmniej zapisany w bazie danych). Dodaj etykietę dla danych wyjściowych wiadomości tekstowej i przycisk Prześlij, aby opublikować formularz oceny na serwerze:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

W kodzie po stronie serwera, uzyskaj dostęp do kontrolki oceny za pośrednictwem jej `ID` a następnie uzyskaj dostęp do jej właściwości `CurrentRating`, która jest liczbą wybranych elementów rankingu, w naszym przykładzie wartość z przedziału od 0 do 5.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Zapisz stronę i Załaduj ją do przeglądarki. Po umieszczeniu wskaźnika myszy nad elementami klasyfikacji (początkowo puste) występuje efekt JavaScript: Klasyfikacja zmienia się. Po kliknięciu zestawu gwiazdek bieżąca klasyfikacja zostanie zachowana. Na koniec po przesłaniu formularza kod po stronie serwera wyprowadza wybraną klasyfikację.

[![tworzenia systemu klasyfikacji z minimalnym kodem](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Tworzenie systemu klasyfikacji z minimalnym kodem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Dalej](creating-a-rating-control-vb.md)
