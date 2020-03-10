---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Wprowadzenie do stron sieci Web ASP.NET — usuwanie danych bazy danych | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono sposób usuwania poszczególnych wpisów bazy danych. Przyjęto założenie, że została ukończona seria przez zaktualizowanie danych bazy danych w sieci Web ASP.NET...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629039"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a>Wprowadzenie do stron sieci Web ASP.NET — usuwanie danych bazy danych

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym samouczku przedstawiono sposób usuwania poszczególnych wpisów bazy danych. Przyjęto założenie, że wykonano serię przez [zaktualizowanie danych bazy danych na stronach sieci Web ASP.NET](updating-data.md).
> 
> Zawartość:
> 
> - Jak wybrać pojedynczy rekord z listy rekordów.
> - Jak usunąć pojedynczy rekord z bazy danych.
> - Jak sprawdzić, czy określony przycisk został kliknięty w formularzu.
>   
> 
> Omówione funkcje/technologie:
> 
> - Pomocnik `WebGrid`.
> - Polecenie SQL `Delete`.
> - Metoda `Database.Execute` do uruchamiania polecenia SQL `Delete`.

## <a name="what-youll-build"></a>Co będziesz kompilować

W poprzednim samouczku przedstawiono sposób aktualizowania istniejącego rekordu bazy danych. Ten samouczek jest podobny, z tą różnicą, że zamiast aktualizacji rekordu, zostanie usunięty. Procesy są znacznie takie same, z tą różnicą, że usuwanie jest prostsze, więc ten samouczek będzie krótki.

Na stronie *filmy* należy zaktualizować pomocnika `WebGrid` tak, aby wyświetlał link **usuwania** obok każdego filmu, który zostanie dołączony do dodanego wcześniej linku **edycji** .

![Strona filmów z linkiem usuwania dla każdego filmu](deleting-data/_static/image1.png)

Podobnie jak w przypadku edycji, kliknięcie linku **usuwania** spowoduje przejście do innej strony, gdzie informacje o filmie są już w postaci:

![Usuń stronę filmu z wyświetlonym filmem](deleting-data/_static/image2.png)

Następnie możesz kliknąć przycisk, aby trwale usunąć rekord.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Dodawanie linku usuwania do listy filmów

Zacznij od dodania linku **usuwania** do pomocnika `WebGrid`. Ten link jest podobny do linku **edycji** , który został dodany w poprzednim samouczku.

Otwórz plik *Films. cshtml* .

Zmień `WebGrid` znaczników w treści strony, dodając kolumnę. Oto zmodyfikowano znaczniki:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Nowa kolumna to:

[!code-html[Main](deleting-data/samples/sample2.html)]

W sposób skonfigurowania siatki, kolumna **Edycja** jest lewej strony siatki, a kolumna **Usuń** jest przysunięta do prawej strony. (Istnieje przecinek po `Year` kolumnie teraz, jeśli nie zauważysz, że.) Nie ma żadnych specjalnych informacji o tym, gdzie znajdują się te kolumny linków, i można je łatwo umieścić obok siebie. W takim przypadku są one oddzielone, aby utrudnić ich przetworzenie.

![Strona filmów z linkami Edytuj i szczegóły oznaczona jako widoczna, aby pokazać, że nie są obok siebie](deleting-data/_static/image3.png)

Nowa kolumna zawiera link (`<a>` element), którego tekst mówi "Delete". Obiekt docelowy linku (jego atrybut `href`) to kod, który ostatecznie jest rozpoznawany jako taki, jak ten adres URL, z wartością `id` inną dla każdego filmu:

[!code-css[Main](deleting-data/samples/sample3.css)]

Ten link spowoduje wywołanie strony o nazwie *DeleteMovie* i przekazanie jej identyfikatora wybranego filmu.

Ten samouczek nie zawiera szczegółowych informacji o sposobie konstruowania tego linku, ponieważ jest prawie identyczny z linkiem **edycji** z poprzedniego samouczka ([Aktualizowanie danych bazy danych na stronach sieci Web ASP.NET](updating-data.md)).

## <a name="creating-the-delete-page"></a>Tworzenie strony usuwania

Teraz można utworzyć stronę, która będzie elementem docelowym linku **usuwania** w siatce.

> [!NOTE] 
> 
> **Ważne** Technika pierwszego wyboru rekordu do usunięcia, a następnie użycie osobnej strony i przycisku w celu potwierdzenia, że proces jest niezwykle istotny dla bezpieczeństwa. Po przeczytaniu w poprzednich samouczkach, wprowadzanie *wszelkich* zmian w witrynie sieci Web powinno odbywać się *zawsze* przy użyciu formularza &mdash;, czyli przy użyciu operacji post protokołu HTTP. Jeśli można zmienić witrynę po prostu przez kliknięcie linku (czyli przy użyciu operacji GET), użytkownicy mogą tworzyć proste żądania do witryny i usuwać dane. Nawet przeszukiwarka wyszukiwarki, która indeksuje witrynę, może przypadkowo usunąć dane po prostu przez następujące linki.
> 
> Gdy aplikacja pozwala użytkownikom na zmianę rekordu, należy zaprezentować rekord użytkownikowi do edycji. Może jednak być skłonny do pominięcia tego kroku w celu usunięcia rekordu. Nie pomijaj tego kroku, chociaż. (Przydatne jest również, aby użytkownicy mogli zobaczyć rekord i potwierdzić, że usuwają one rekord, którego zamierzą).
> 
> W kolejnym zestawie samouczków zobaczysz, jak dodać funkcje logowania, aby użytkownik musiał się zalogować przed usunięciem rekordu.

Utwórz stronę o nazwie *DeleteMovie. cshtml* i Zastąp zawartość pliku następującym znacznikiem:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Znaczniki te są podobne do stron *EditMovie* , z tą różnicą, że zamiast używania pól tekstowych (`<input type="text">`), znaczniki zawierają `<span>` elementy. Nie ma nic tutaj do edycji. Wystarczy wyświetlić szczegóły filmu, aby użytkownicy mogli upewnić się, że usuwa właściwy film.

Znacznik zawiera już link umożliwiający powrót użytkownika do strony z listą filmów.

Podobnie jak na stronie *EditMovie* , identyfikator wybranego filmu jest przechowywany w ukrytym polu. (Jest ona przenoszona do strony w pierwszym miejscu jako wartość ciągu zapytania). Istnieje `Html.ValidationSummary` wywołanie, które spowoduje wyświetlenie błędów walidacji. W takim przypadku przyczyną błędu może być to, że żaden z identyfikatorów filmów nie został przesłany do strony lub identyfikator filmu jest nieprawidłowy. Taka sytuacja może wystąpić, jeśli ktoś uruchomił Tę stronę bez uprzedniego wybrania filmu na stronie *filmów* .

Podpis przycisku to **Usuń film**, a jego atrybut name jest ustawiony na `buttonDelete`. Atrybut `name` zostanie użyty w kodzie do zidentyfikowania przycisku, który przesłał formularz.

Musisz napisać kod do 1) odczytać szczegóły filmu, gdy strona jest wyświetlana po raz pierwszy i 2) w rzeczywistości usunie film, gdy użytkownik kliknie przycisk.

## <a name="adding-code-to-read-a-single-movie"></a>Dodawanie kodu w celu odczytania pojedynczego filmu

W górnej części strony *DeleteMovie. cshtml* Dodaj następujący blok kodu:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Ten znacznik jest taki sam jak odpowiedni kod na stronie *EditMovie* . Pobiera identyfikator filmu z ciągu zapytania i używa identyfikatora do odczytywania rekordu z bazy danych. Kod zawiera test weryfikacyjny (`IsInt()` i `row != null`), aby upewnić się, że identyfikator filmu jest prawidłowy.

Należy pamiętać, że ten kod powinien zostać uruchomiony tylko podczas pierwszego uruchomienia strony. Nie ma potrzeby ponownego odczytywania rekordu filmu z bazy danych, gdy użytkownik kliknie przycisk **Usuń film** . W związku z tym kod odczytywania filmu znajduje się w teście, który brzmi `if(!IsPost)` &mdash; to, *Jeśli żądanie nie jest operacją post (Przesyłanie formularza)* .

## <a name="adding-code-to-delete-the-selected-movie"></a>Dodawanie kodu w celu usunięcia wybranego filmu

Aby usunąć film, gdy użytkownik kliknie przycisk, Dodaj następujący kod tuż wewnątrz zamykającego nawiasu klamrowego bloku `@`:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Ten kod jest podobny do kodu do aktualizowania istniejącego rekordu, ale łatwiejszy. Kod zasadniczo uruchamia instrukcję SQL `Delete`.

 Podobnie jak na stronie *EditMovie* , kod znajduje się w bloku `if(IsPost)`. Tym razem `if()` warunku jest nieco bardziej skomplikowany: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Poniżej przedstawiono dwa warunki. Pierwszy polega na tym, że strona jest przesyłana, jak widać przed &mdash; `if(IsPost)`.

Drugi warunek jest `!Request["buttonDelete"].IsEmpty()`, co oznacza, że żądanie ma obiekt o nazwie `buttonDelete`. Jest to pośredni sposób testowania, który przycisk przesłał formularz. Jeśli formularz zawiera wiele przycisków przesyłania, w żądaniu zostanie wyświetlona tylko nazwa klikniętego przycisku. W związku z tym, logicznie, jeśli nazwa określonego przycisku pojawia się w żądaniu &mdash; lub zgodnie z opisem w kodzie, jeśli ten przycisk nie jest pusty &mdash; jest to przycisk, który przesłał formularz.

Operator `&&` oznacza "i" (logiczny i). W związku z tym cały `if` warunek to...

*To żądanie jest ogłoszeniem (nie jest żądaniem pierwszego uruchomienia)*  
  
 AND  
  
Przycisk `buttonDelete`*był przyciskiem, który przesłał formularz.*

Ten formularz (w rzeczywistości ta strona) zawiera tylko jeden przycisk, dlatego dodatkowy test dla `buttonDelete` nie jest technicznie wymagany. Nadal można wykonać operację, która spowoduje trwałe usunięcie danych. Tak więc należy się upewnić, że wykonywanie operacji jest możliwe tylko wtedy, gdy użytkownik jawnie zażądał tego. Załóżmy na przykład, że ta strona została rozszerzona później i dodano do niej inne przyciski. Nawet kod usuwający film zostanie uruchomiony tylko wtedy, gdy kliknięto przycisk `buttonDelete`.

Podobnie jak na stronie *EditMovie* , otrzymujesz identyfikator z ukrytego pola, a następnie uruchamiasz polecenie SQL. Składnia instrukcji `Delete` jest następująca:

`DELETE FROM table WHERE ID = value`

Należy uwzględnić klauzulę `WHERE` i identyfikator. Jeśli opuścisz klauzulę WHERE, *wszystkie rekordy w tabeli zostaną usunięte*. Jak widać, przekaż wartość identyfikatora do polecenia SQL przy użyciu symbolu zastępczego.

## <a name="testing-the-movie-delete-process"></a>Testowanie procesu usuwania filmu

Teraz można testować. Uruchom stronę *filmy* , a następnie kliknij pozycję **Usuń** obok filmu. Gdy zostanie wyświetlona strona *DeleteMovie* , kliknij przycisk **Usuń film**.

![Usuń stronę filmu z wyróżnionym przyciskiem Usuń film](deleting-data/_static/image4.png)

Po kliknięciu przycisku kod usuwa filmy i wraca do listy filmów. Można wyszukać usunięty film i potwierdzić, że został on usunięty.

## <a name="coming-up-next"></a>Przyszłe przejście

W następnym samouczku przedstawiono sposób nadawania wszystkim stronom w witrynie typowego wyglądu i układu.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Ukończ listę na stronie filmu (Zaktualizowano za pomocą linków usuwania)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Ukończ listę dla strony DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Dodatkowe materiały

- [Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor](../introducing-razor-syntax-c.md)
- [Instrukcja DELETE SQL](http://www.w3schools.com/sql/sql_delete.asp) w witrynie w3schools

> [!div class="step-by-step"]
> [Poprzednie](updating-data.md)
> [dalej](layouts.md)
