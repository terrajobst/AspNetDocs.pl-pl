---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Dodawanie metody Create i Tworzenie widoku | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 05a281720f76b107fe8d902ef60d5d2e72af3ef5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543660"
---
# <a name="adding-a-create-method-and-create-view"></a>Dodawanie metody Create i tworzenie widoku

przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych. Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.

W tej sekcji zamierzamy wdrożyć pomoc techniczną, aby umożliwić użytkownikom tworzenie nowych filmów w naszej bazie danych. Należy to zrobić, implementując akcję/Movies/Create URL.

Implementacja adresu URL/Movies/Create jest procesem dwuetapowym. Gdy użytkownik po raz pierwszy odwiedzi adres URL/Movies/Create, chcemy pokazać im formularz HTML, który może wypełnić, aby wprowadzić nowy film. Następnie, gdy użytkownik przesyła formularz i zapisuje dane z powrotem na serwerze, chcemy pobrać ogłoszoną zawartość i zapisać ją w bazie danych.

Te dwa kroki zostaną wdrożone w ramach dwóch metod Create () w ramach naszej klasy MoviesController. Jedna metoda wyświetli &lt;formularz&gt;, że użytkownik powinien wypełnić, aby utworzyć nowy film. Druga metoda obsłuży przetwarzanie danych ogłoszonych, gdy użytkownik prześle &lt;formularz&gt; z powrotem do serwera i zapisze nowy film w naszej bazie danych.

Poniżej znajduje się kod, który dodamy do naszej klasy MoviesController, aby zaimplementować ten:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Powyższy kod zawiera cały kod, który będzie potrzebny w naszym kontrolerze.

Teraz zaimplementujmy szablon tworzenia widoku, który będzie używany do wyświetlania formularza użytkownikowi. Zostaniemy kliknięty w pierwszej metodzie tworzenia i wybierz pozycję "Dodaj widok", aby utworzyć szablon widoku dla naszego formularza filmu.

Wybierzemy, że zamierzamy przekazać szablon widoku jako "film" jako swoją klasę danych widoku i wskazać, że chcemy "utworzyć szkielet" dla szablonu "Utwórz".

[![Dodaj widok](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Po kliknięciu przycisku Dodaj zostanie utworzony szablon widoku \Movies\Create.aspx. Ponieważ wybrano opcję "Utwórz" z listy rozwijanej "Wyświetl zawartość", w oknie dialogowym Dodawanie widoku automatycznie "szkieletowa" jest tworzona zawartość domyślna. Tworzenie szkieletu utworzyło formularz &lt;HTML&gt;, miejsce do sprawdzania poprawności komunikatów o błędach, a ponieważ szkielet ma informacje o filmach, utworzył etykietę i pola dla każdej właściwości naszej klasy.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Ponieważ nasza baza danych automatycznie zawiera identyfikator filmu, usuńmy te pola, które odwołują się do modelu. Identyfikator z naszego widoku tworzenia. Usuń wiersze w 7 po &lt;&gt;pól&lt;/Legend&gt;, ponieważ pokazują one pola identyfikatorów, których nie chcemy.

Utwórz teraz nowy film i dodaj go do bazy danych. W tym celu należy ponownie uruchomić aplikację i odwiedzić adres URL "/Movies" i kliknąć link "Utwórz", aby dodać nowy film.

[![tworzenie — Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Po kliknięciu przycisku Utwórz zostanie Wyksięgowany zwrot (za pośrednictwem protokołu HTTP POST) danych w tym formularzu do metody/Movies/Create, która została właśnie utworzona. Podobnie jak w przypadku, gdy system automatycznie przejął parametr "numTimes" i "name" z adresu URL i zamapować je do parametrów we wcześniejszej metodzie, system automatycznie przyjmuje pola formularza z wpisu i mapuje je do obiektu. W takim przypadku wartości z pól w formacie HTML, takich jak "ReleaseDate" i "title", zostaną automatycznie wprowadzone do poprawnych właściwości nowego wystąpienia filmu.

Przyjrzyjmy się drugiej metodzie tworzenia z naszego MoviesController. Zwróć uwagę, jak przyjmuje się, że obiekt "Movie" jako argument:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Ten obiekt filmu został następnie przekierowany do wersji [HttpPost] naszej metody tworzenia akcji i został zapisany w bazie danych, a następnie przekierować użytkownika z powrotem do metody akcji index (), która spowoduje wyświetlenie zapisanego wyniku na liście filmów:

[Lista filmów ![— Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Nie Sprawdzamy, czy filmy są poprawne, chociaż nie pozwalają na zapisanie filmu bez tytułu. Jest to dobre rozwiązanie, jeśli firma Microsoft poinformuje użytkownika, że zanim baza danych wygenerowała błąd. Zajmiemy się tym następnym, dodając obsługę walidacji do naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part5.md)
> [dalej](getting-started-with-mvc-part7.md)
