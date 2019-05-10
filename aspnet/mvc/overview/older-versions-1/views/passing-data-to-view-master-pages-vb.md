---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Przekazywanie danych do stron wzorcowych widoku (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Celem tego samouczka jest wyjaśniają, jak przekazać dane za pomocą kontrolera widoku strony wzorcowej. Sprawdzamy pomocy dwóch strategii przekazywania danych do widoku m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: ebf670fc0d8cf2cfd7df01d07d4119122b61a6a1
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130419"
---
# <a name="passing-data-to-view-master-pages-vb"></a>Przekazywanie danych do stron wzorcowych widoku (VB)

przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> Celem tego samouczka jest wyjaśniają, jak przekazać dane za pomocą kontrolera widoku strony wzorcowej. Sprawdzamy pomocy dwóch strategii przekazywania danych do strony wzorcowej widoku. Po pierwsze omówimy prostemu rozwiązaniu, powstałego w aplikacji, która jest trudne w utrzymaniu. Następnie omówiony dużo lepszym rozwiązaniem, która wymaga nieco więcej pracy początkowej, ale wyniki w aplikacji będzie znacznie łatwiejszy w utrzymaniu.

## <a name="passing-data-to-view-master-pages"></a>Przekazywanie danych do stron wzorcowych widoku

Celem tego samouczka jest wyjaśniają, jak przekazać dane za pomocą kontrolera widoku strony wzorcowej. Sprawdzamy pomocy dwóch strategii przekazywania danych do strony wzorcowej widoku. Po pierwsze omówimy prostemu rozwiązaniu, powstałego w aplikacji, która jest trudne w utrzymaniu. Następnie omówiony dużo lepszym rozwiązaniem, która wymaga nieco więcej pracy początkowej, ale wyniki w aplikacji będzie znacznie łatwiejszy w utrzymaniu.

### <a name="the-problem"></a>Ten Problem

Wyobraź sobie, że tworzysz aplikacji bazy danych filmów i mają być wyświetlane na liście kategorii filmu na każdej stronie w aplikacji (patrz rysunek 1). Wyobraź sobie, co więcej, że na liście kategorii filmu są przechowywane w tabeli bazy danych. W takiej sytuacji sensowne będzie do pobrania kategorii z bazy danych i renderowania na liście kategorii filmu w obrębie strony wzorcowej widoku.

[![Wyświetlanie kategorii filmu w widoku strony wzorcowej](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Rysunek 01**: Wyświetlanie kategorii filmu w widoku strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](passing-data-to-view-master-pages-vb/_static/image3.png))

Poniżej przedstawiono ten problem. Sposób pobierania listy kategorii filmu na stronie głównej? Jest to wydawać się bezpośrednio wywoływać metod w klasach modeli strony wzorcowej. Innymi słowy jest kuszące Dołączanie kodu do pobierania danych z bazy danych po prawej stronie na stronie głównej. Jednak Twoja kontrolerów MVC, dostęp do bazy danych z pominięciem naruszyłoby czyste rozdzielenie problemów, które jest jednym z podstawowych zalet kompilowania aplikacji MVC.

W aplikacji MVC chcesz, aby wszystkie interakcje między widoków MVC i modelu MVC, które mają być obsługiwane przez Twoje kontrolerów MVC. Powoduje to separacji w obsłudze, dostosowywalne i testowaniu aplikacji.

W aplikacji MVC wszystkie dane przekazywane do widoku — w tym widoku strony wzorcowej — powinien być przekazywany do widoku przez akcji kontrolera. Ponadto danych powinien być przekazywany przez wykorzystanie danych widoku. W pozostałej części tego samouczka I Sprawdź dwie metody przekazywania danych widoku strony wzorcowej widoku.

### <a name="the-simple-solution"></a>Proste rozwiązanie

Zacznijmy z Najprostszym rozwiązaniem do przekazywania wyświetlanie danych za pomocą kontrolera widoku strony wzorcowej. Jest najprostszym rozwiązaniu do przekazania danych widoku dla strony wzorcowej w każdej akcji kontrolera.

Należy wziąć pod uwagę kontrolera w ofercie 1. Udostępnia ona dwie akcje o nazwie `Index()` i `Details()`. `Index()` Metoda akcji zwraca każdego filmu w tabeli bazy danych filmów. `Details()` Metoda akcji zwraca każdego filmu w kategorii konkretnego filmu.

**1 — Lista `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Należy zauważyć, że zarówno `Index()` i `Details()` akcje dodania dwóch elementów, aby wyświetlić dane. `Index()` Akcja go dodaje dwa klucze: kategorie i filmy. Klucz kategorie reprezentuje listę kategorii filmu wyświetlane przez strony wzorcowej widoku. Klucz filmy reprezentuje listę filmów wyświetlane przez indeks strony widoku.

`Details()` Akcja go dodaje również dwa klucze o nazwie kategorie i filmy. Klucz kategorie reprezentuje jeszcze raz na liście kategorii filmu wyświetlane przez strony wzorcowej widoku. Klucz filmy reprezentuje listę filmów w określonej kategorii wyświetlanych przez strony widoku szczegółów (patrz rysunek 2).

[![Widok szczegółów](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Rysunek 02**: Widok szczegółów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](passing-data-to-view-master-pages-vb/_static/image6.png))

Widok indeksu są zawarte w ofercie 2. Iteruje po prostu listy filmów reprezentowanego przez element filmów w widoku danych.

**2 — Lista `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

Strony wzorcowej widoku znajduje się w ofercie 3. Strony wzorcowej widoku dokonuje iteracji i renderuje wszystkie kategorie filmu, reprezentowane przez element kategorii z danymi widoku.

**3 — lista `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Wszystkie dane jest przekazywane do widoku oraz widoku strony wzorcowej widoku danych. To prawidłowy sposób, aby przekazać dane do strony wzorcowej.

W związku czym jest problem za pomocą tego rozwiązania? Problem polega na tym, że to rozwiązanie narusza zasadę PRÓBNEGO (nie należy powtórzyć samodzielnie). Każdej akcji kontrolera należy dodać bardzo ta sama lista kategorii filmu, aby wyświetlić dane. Posiadanie kodu zduplikowanego w aplikacji sprawia, że aplikacja dużo bardziej skomplikowane, obsługa, dostosowania i modyfikowania.

### <a name="the-good-solution"></a>Dobrym rozwiązaniem

W tej sekcji omówiony rozwiązanie alternatywne i lepsze, do przekazywania danych z akcji kontrolera widoku strony wzorcowej. Zamiast opcji dodawania kategorii filmu dla strony wzorcowej w każdej akcji kontrolera, możemy dodać film kategorie wyświetlanie danych tylko raz. Wszystkie dane widoku, używane przez strony wzorcowej widoku jest dodawany do kontrolera aplikacji.

Klasa ApplicationController znajduje się w ofercie 4.

Klasa ApplicationController znajduje się w ofercie 4.

**4 — lista `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Istnieją trzy czynności, które użytkownik zauważy o kontrolera aplikacji w ofercie 4. Najpierw zwróć uwagę, że klasa dziedziczy z klasy bazowej System.Web.Mvc.Controller. Kontroler aplikacji jest klasa kontrolera.

Po drugie Zauważ, że klasa kontrolera aplikacji klasy MustInherit. MustInherit — klasa to klasa, która musi być implementowana przez klasy konkretnej. Ponieważ MustInherit — Klasa kontrolera aplikacji, nie może nie wywoływać wszelkie metody zdefiniowane w klasie bezpośrednio. Jeśli użytkownik podejmie próbę bezpośrednio wywołać klasy aplikacji następnie otrzymasz komunikat o błędzie nie można odnaleźć zasobu.

Po trzecie Zwróć uwagę, że kontroler aplikacji zawiera konstruktora, który dodaje listę kategorii filmu, aby wyświetlić dane. Każda klasa kontrolera, który dziedziczy z kontrolera aplikacji, które automatycznie wywołuje konstruktora kontrolera aplikacji. Zawsze, gdy chcesz wywołać dowolną akcję na żadnym kontrolerze, który dziedziczy z kontrolera aplikacji, kategorie filmu znajduje się w widoku danych automatycznie.

Kontroler filmów w ofercie 5 dziedziczy kontrolera aplikacji.

**5 — lista `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Kontroler filmy, podobnie jak kontrolera głównego opisanych w poprzedniej sekcji udostępnia dwie metody akcji, o nazwie `Index()` i `Details()`. Należy zauważyć, że na liście kategorii filmu wyświetlane przez strony wzorcowej widoku nie jest dodawane do wyświetlania danych w jednym `Index()` lub `Details()` metody. Ponieważ kontroler filmy dziedziczy kontroler aplikacji, na liście kategorii film jest dodawany do wyświetlania danych automatycznie.

Zwróć uwagę, to rozwiązanie do dodawania danych widoku dla strony wzorcowej widoku naruszają zasady PRÓBNEGO (nie należy powtórzyć samodzielnie). Kod do dodawania listy kategorii film do wyświetlania danych znajduje się w lokalizacji tylko jeden: konstruktor dla kontrolera aplikacji.

### <a name="summary"></a>Podsumowanie

W tym samouczku Omówiliśmy dwa podejścia do przekazywania danych Widok z kontrolera do widoku strony wzorcowej. Po pierwsze zbadaliśmy prosta, ale trudne w utrzymaniu podejście. W pierwszej sekcji omówiono, jak dodać dane widoku dla strony wzorcowej widoku w każdej kontrolera akcji w aplikacji. Firma Microsoft stwierdzono, było to zły podejście ponieważ narusza zasadę PRÓBNEGO (nie należy powtórzyć samodzielnie).

Następnie zbadaliśmy znacznie lepszą strategię Dodawanie danych do wyświetlenia danych wymagany przez strony wzorcowej widoku. Zamiast opcji dodawania danych widoku w każdej akcji kontrolera, dodaliśmy dane widoku, tylko jeden raz w ramach kontrolera aplikacji. Dzięki temu można uniknąć kodu zduplikowanego przy przekazywaniu danych do strony wzorcowej widoku w aplikacji ASP.NET MVC.

> [!div class="step-by-step"]
> [Poprzednie](creating-page-layouts-with-view-master-pages-vb.md)
