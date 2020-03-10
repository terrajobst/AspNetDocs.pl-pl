---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: Przekazywanie danych w celu wyświetlenia stron wzorcowych (C#) | Microsoft Docs
author: microsoft
description: Celem tego samouczka jest wyjaśnienie, jak można przekazać dane z kontrolera do strony wzorcowej widoku. Analizujemy dwie strategie przekazywania danych do widoku m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1492175812b0a092cd1594a770e348efe9b4122b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600143"
---
# <a name="passing-data-to-view-master-pages-c"></a>Przekazywanie danych do stron wzorcowych widoku (C#)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> Celem tego samouczka jest wyjaśnienie, jak można przekazać dane z kontrolera do strony wzorcowej widoku. Analizujemy dwie strategie przekazywania danych do strony wzorcowej widoku. Najpierw omówiono proste rozwiązanie, które powoduje trudne do utrzymania działanie aplikacji. Następnie analizujemy znacznie lepsze rozwiązanie, które wymaga niewielkiej ilości pracy początkowej, ale spowoduje to znacznie bardziej konserwowaną aplikację.

## <a name="passing-data-to-view-master-pages"></a>Przekazywanie danych w celu wyświetlenia stron wzorcowych

Celem tego samouczka jest wyjaśnienie, jak można przekazać dane z kontrolera do strony wzorcowej widoku. Analizujemy dwie strategie przekazywania danych do strony wzorcowej widoku. Najpierw omówiono proste rozwiązanie, które powoduje trudne do utrzymania działanie aplikacji. Następnie analizujemy znacznie lepsze rozwiązanie, które wymaga niewielkiej ilości pracy początkowej, ale spowoduje to znacznie bardziej konserwowaną aplikację.

### <a name="the-problem"></a>Problem

Załóżmy, że tworzysz aplikację bazy danych filmów i chcesz wyświetlić listę kategorii filmów na każdej stronie w aplikacji (patrz rysunek 1). Wyobraź sobie, że lista kategorii filmów jest przechowywana w tabeli bazy danych. W takim przypadku warto pobrać kategorie z bazy danych i renderować listę kategorii filmów na stronie wzorcowej widoku.

[![wyświetlania kategorii filmów na stronie wzorcowej widoku](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**Ilustracja 01**. Wyświetlanie kategorii filmów na stronie wzorcowej widoku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](passing-data-to-view-master-pages-cs/_static/image3.png))

Wystąpił problem. Jak pobrać listę kategorii filmów na stronie wzorcowej? Zachęcamy do bezpośredniego wywoływania metod klas modelu na stronie wzorcowej. Innymi słowy, zachęcamy do dołączenia kodu do pobierania danych z bazy danych bezpośrednio na stronie wzorcowej. Jednak pominięcie kontrolerów MVC w celu uzyskania dostępu do bazy danych miałoby na celu naruszenie czystego rozdzielenia problemów, które są jedną z podstawowych korzyści związanych z tworzeniem aplikacji MVC.

W aplikacji MVC wszystkie interakcje między widokami MVC i modelem MVC mają być obsługiwane przez kontrolery MVC. Ta separacja problemów powoduje, że aplikacja jest bardziej konserwowana, dostosowywalna i weryfikowalne.

W aplikacji MVC wszystkie dane przesyłane do widoku — w tym strony wzorcowej widoku — powinny być przesyłane do widoku przez akcję kontrolera. Ponadto dane powinny być przesyłane przez skorzystanie z zalet wyświetlania danych. W pozostałej części tego samouczka badamy dwie metody przekazywania danych widoku do strony wzorcowej widoku.

### <a name="the-simple-solution"></a>Proste rozwiązanie

Zacznijmy od najprostszym rozwiązaniem, aby przekazywać dane widoku z kontrolera do strony wzorcowej widoku. Najprostszym rozwiązaniem jest przekazanie danych widoku dla strony wzorcowej w każdej akcji kontrolera.

Rozważmy kontroler z listą 1. Przedstawia dwie akcje o nazwie `Index()` i `Details()`. Metoda akcji `Index()` zwraca każdy film z tabeli bazy danych filmów. Metoda akcji `Details()` zwraca każdy film w określonej kategorii filmu.

**Lista 1 — `Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Zwróć uwagę, że zarówno akcje index (), jak i szczegóły () dodają dwa elementy do wyświetlania danych. Akcja index () umożliwia dodanie dwóch kluczy: kategorii i filmów. Klucz kategorii reprezentuje listę kategorii filmów wyświetlanych na stronie wzorcowej widoku. Klucz filmów reprezentuje listę filmów wyświetlanych na stronie widok indeksu.

Akcja szczegóły () dodaje również dwa klucze o nazwie kategorie i filmy. Klucz kategorie ponownie reprezentuje listę kategorii filmów wyświetlanych na stronie wzorca wyświetlania. Klawisz filmów reprezentuje listę filmów w określonej kategorii wyświetlanych na stronie widok szczegółów (patrz rysunek 2).

[![widoku szczegółów](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**Ilustracja 02**. widok szczegółów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](passing-data-to-view-master-pages-cs/_static/image6.png))

Widok indeksu znajduje się na liście 2. Po prostu wykonuje iterację na liście filmów reprezentowanych przez element filmów w widoku dane.

**Lista 2 — `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

Strona wzorca widoku znajduje się na liście 3. Strona wzorca widoku wykonuje iterację i renderuje wszystkie kategorie filmów reprezentowane przez element Categories z danych widoku.

**Lista 3 — `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Wszystkie dane są przesyłane do widoku oraz na stronie wzorca widoku za pomocą widoku danych. Jest to poprawny sposób przekazywania danych na stronę wzorcową.

Co się stało z tym rozwiązaniem? Problem polega na tym, że to rozwiązanie narusza zasadę SUCHą (Nie powtarzaj się). Każda akcja kontrolera i każdego z nich musi dodawać bardzo samą listę kategorii filmów do wyświetlania danych. Posiadanie duplikatu kodu w aplikacji sprawia, że aplikacja jest znacznie trudniejsza do utrzymania, dostosowania i modyfikacji.

### <a name="the-good-solution"></a>Dobre rozwiązanie

W tej sekcji analizujemy alternatywne i lepsze rozwiązanie umożliwiające przekazywanie danych z akcji kontrolera do strony wzorcowej widoku. Zamiast dodawać kategorie filmów dla strony wzorcowej w każdej akcji kontrolera i każdym z nich, dodamy kategorie filmów do danych widoku tylko raz. Wszystkie dane widoku używane przez stronę wzorcową widoku są dodawane do kontrolera aplikacji.

Klasa ApplicationController jest zawarta w liście 4.

**Lista 4 — `Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Istnieją trzy rzeczy, które należy zauważyć w przypadku kontrolera aplikacji na liście 4. Najpierw należy zauważyć, że klasa dziedziczy z klasy podstawowa system. Web. MVC. Controller. Kontroler aplikacji jest klasą kontrolera.

Następnie Zwróć uwagę, że klasa kontrolera aplikacji jest klasą abstrakcyjną. Klasa abstrakcyjna jest klasą, która musi być implementowana przez konkretną klasę. Ponieważ kontroler aplikacji jest klasą abstrakcyjną, nie można wywoływać żadnej metody zdefiniowanej w klasie bezpośrednio. W przypadku próby wywołania klasy aplikacji bezpośrednio zostanie wyświetlony komunikat o błędzie.

Po trzecie należy zauważyć, że kontroler aplikacji zawiera konstruktora, który dodaje listę kategorii filmów do wyświetlania danych. Każda klasa kontrolera, która dziedziczy z kontrolera aplikacji, automatycznie wywołuje Konstruktor aplikacji. Za każdym razem, gdy wywołasz dowolną akcję na dowolnym kontrolerze, który dziedziczy z kontrolera aplikacji, kategorie filmów są automatycznie uwzględniane w widoku danych.

Kontroler filmów na liście 5 dziedziczy z kontrolera aplikacji.

**Lista 5 — `Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

Kontroler filmów, podobnie jak kontroler macierzysty omówiony w poprzedniej sekcji, uwidacznia dwie metody działania o nazwie `Index()` i `Details()`. Zwróć uwagę, że lista kategorii filmów wyświetlanych na stronie wzorca widoku nie została dodana do wyświetlania danych w metodzie `Index()` lub `Details()`. Ponieważ kontroler filmów dziedziczy z kontrolera aplikacji, lista kategorii filmów jest dodawana do automatycznego wyświetlania danych.

Należy zauważyć, że to rozwiązanie do dodawania danych widoku dla strony wzorcowej widoku nie narusza zasady wyschnięcia (Nie powtarzaj się). Kod służący do dodawania listy kategorii filmów do wyświetlania danych znajduje się tylko w jednej lokalizacji: Konstruktor dla kontrolera aplikacji.

### <a name="summary"></a>Podsumowanie

W tym samouczku omówiono dwa podejścia do przekazywania danych widoku z kontrolera do strony wzorcowej widoku. Najpierw zbadamy prostą, ale trudno jest zachować podejście. W pierwszej sekcji omówiono sposób dodawania danych widoku dla strony wzorcowej widoku w każdej akcji kontrolera w aplikacji. Przyjęto, że było to złe podejście, ponieważ narusza zasadę SUCHą (Nie powtarzaj się).

Następnie zbadamy znacznie lepszą strategię dodawania danych wymaganych przez stronę wzorcową widoku do wyświetlania danych. Zamiast dodawać dane widoku w każdej akcji kontrolera, dodaliśmy dane widoku tylko raz w ramach kontrolera aplikacji. Dzięki temu można uniknąć duplikowania kodu podczas przekazywania danych do strony wzorcowej widoku w aplikacji ASP.NET MVC.

> [!div class="step-by-step"]
> [Poprzednie](creating-page-layouts-with-view-master-pages-cs.md)
> [dalej](asp-net-mvc-views-overview-vb.md)
