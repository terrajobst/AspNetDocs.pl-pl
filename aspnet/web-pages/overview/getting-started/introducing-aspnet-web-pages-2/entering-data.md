---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Wprowadzenie do stron sieci Web ASP.NET — wprowadzanie danych bazy danych przy użyciu formularzy | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć formularz wprowadzania, a następnie wprowadzić dane uzyskane z formularza do tabeli bazy danych w przypadku używania stron sieci Web ASP.NET (...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b9354a7b97a7df9020a681f709e16a92650cfcf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624538"
---
# <a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Wprowadzenie do stron sieci Web ASP.NET — wprowadzanie danych bazy danych za pomocą formularzy

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym samouczku pokazano, jak utworzyć formularz wprowadzania, a następnie wprowadzić dane uzyskane z formularza do tabeli bazy danych w przypadku używania stron sieci Web ASP.NET (Razor). Przyjęto założenie, że serie zostały wykonane za pomocą [podstaw formularzy HTML na stronach sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Zawartość:
> 
> - Więcej informacji o sposobie przetwarzania formularzy wprowadzania.
> - Dodawanie (wstawianie) danych w bazie danych.
> - Jak upewnić się, że użytkownicy wprowadzili wymaganą wartość w formularzu (jak sprawdzić poprawność danych wejściowych użytkownika).
> - Jak wyświetlić błędy walidacji.
> - Jak przejść do innej strony z bieżącej strony.
>   
> 
> Omówione funkcje/technologie:
> 
> - Metoda `Database.Execute`.
> - Instrukcja SQL `Insert Into`
> - Pomocnik `Validation`.
> - Metoda `Response.Redirect`.

## <a name="what-youll-build"></a>Co będziesz kompilować

W samouczku, który pokazano, jak utworzyć bazę danych, wprowadzono dane bazy danych, edytując bazę danych bezpośrednio w programie WebMatrix, pracując w obszarze roboczym **bazy danych** . W większości aplikacji to nie jest praktyczny sposób umieszczania danych w bazie danych, chociaż. W tym samouczku utworzysz interfejs oparty na sieci Web, który pozwala na wprowadzanie danych i zapisywanie ich w bazie danych.

Utworzysz stronę umożliwiającą wprowadzanie nowych filmów. Strona będzie zawierać formularz wprowadzania zawierający pola (pola tekstowe), w których można wprowadzić tytuł filmu, gatunek i rok. Strona będzie wyglądać następująco:

![Strona "Dodaj film" w przeglądarce](entering-data/_static/image1.png)

Pola tekstowe będą `<input>` elementy HTML, które będą wyglądać jak w tym znaczniku:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Tworzenie podstawowego formularza wprowadzania

Utwórz stronę o nazwie *addmovie. cshtml*.

Zastąp zawartość pliku następującym znacznikiem. Zastąp wszystko; Wkrótce dodasz blok kodu u góry.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Ten przykład pokazuje typowy kod HTML na potrzeby tworzenia formularza. Używa `<input>` elementów dla pól tekstowych i dla przycisku Prześlij. Podpisy dla pól tekstowych są tworzone przy użyciu standardowych elementów `<label>`. Elementy `<fieldset>` i `<legend>` umieszczają w postaci całkiem dobre pole.

Zwróć uwagę na to, że na tej stronie element `<form>` używa `post` jako wartości atrybutu `method`. W poprzednim samouczku utworzono formularz, który używa metody `get`. Jest to poprawne, ponieważ mimo że formularz przesłał wartości do serwera, żądanie nie wprowadza żadnych zmian. Wszystkie dane były pobierane na różne sposoby. Jednak na tej *stronie wprowadzisz* zmiany — będziesz dodawać nowe rekordy bazy danych. W związku z tym formularz powinien używać metody `post`. (Aby uzyskać więcej informacji o różnicach między operacjami `GET` i `POST`, zobacz pasek boczny[bezpieczeństwa czasowników Get, post i http](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) w poprzednim samouczku.)

Zwróć uwagę, że każde pole tekstowe ma `name` element (`title`, `genre`, `year`). Jak widać w poprzednim samouczku, te nazwy są ważne, ponieważ trzeba mieć te nazwy, aby można było później uzyskać dane wejściowe użytkownika. Możesz użyć dowolnych nazw. Warto używać znaczących nazw, które ułatwiają zapamiętywanie danych, z którymi pracujesz.

Atrybut `value` każdego elementu `<input>` zawiera bit kodu Razor (na przykład `Request.Form["title"]`). W poprzednim samouczku poznasz wersję tej lewie, aby zachować wartość wprowadzoną w polu tekstowym (jeśli istnieje) po przesłaniu formularza.

## <a name="getting-the-form-values"></a>Pobieranie wartości formularza

Następnie Dodaj kod, który przetwarza formularz. W obszarze konspekt należy wykonać następujące czynności:

1. Sprawdź, czy strona jest ogłaszana (została przesłana). Chcesz, aby kod był uruchamiany tylko wtedy, gdy użytkownik kliknął przycisk, a nie podczas pierwszego uruchomienia strony.
2. Pobierz wartości wprowadzone przez użytkownika do pól tekstowych. W tym przypadku, ponieważ formularz używa zlecenia `POST`, otrzymujesz wartości formularza z kolekcji `Request.Form`.
3. Wstaw wartości jako nowy rekord w tabeli bazy danych *filmów* .

W górnej części pliku Dodaj następujący kod:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Pierwsze kilka wierszy tworzy zmienne (`title`, `genre`i `year`), aby przechowywać wartości z pól tekstowych. Wiersz `if(IsPost)` upewnij się, że zmienne są ustawiane *tylko* wtedy, gdy użytkownik kliknie przycisk **Dodaj film** , czyli po opublikowaniu formularza.

Jak pokazano w poprzednim samouczku, uzyskasz wartość pola tekstowego przy użyciu wyrażenia, takiego jak `Request.Form["name"]`, gdzie *name* jest nazwą elementu `<input>`.

Nazwy zmiennych (`title`, `genre`i `year`) są dowolne. Podobnie jak w przypadku nazw przypisanych do `<input>` elementów, możesz je wywoływać w dowolny sposób. (Nazwy zmiennych nie muszą odpowiadać atrybutom nazwy elementów `<input>` w formularzu). Ale podobnie jak w przypadku elementów `<input>`, dobrym pomysłem jest użycie nazw zmiennych, które odzwierciedlają zawarte w nich dane. Gdy piszesz kod, spójne nazwy ułatwiają zapamiętywanie danych, z którymi pracujesz.

## <a name="adding-data-to-the-database"></a>Dodawanie danych do bazy danych

W bloku kodu, który właśnie został dodany, tuż *wewnątrz* zamykającego nawiasu klamrowego (`}`) bloku `if` (nie tylko wewnątrz bloku kodu), Dodaj następujący kod:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Ten przykład przypomina kod użyty w poprzednim samouczku do pobierania i wyświetlania danych. Wiersz rozpoczynający się od `db =` otwiera bazę danych, jak poprzednio, a następny wiersz definiuje instrukcję SQL ponownie, jak wcześniej. Jednak ten czas definiuje instrukcję SQL `Insert Into`. Poniższy przykład przedstawia ogólną składnię instrukcji `Insert Into`:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Innymi słowy, należy określić tabelę do wstawienia, a następnie wyświetlić listę kolumn do wstawienia, a następnie wyświetlić listę wartości do wstawienia. (Jak wspomniano wcześniej, w języku SQL nie jest rozróżniana wielkość liter, ale niektóre osoby mają wielkie słowa kluczowe, aby ułatwić odczytywanie polecenia).

Kolumny, które są wstawiane, są już wymienione w poleceniu — `(Title, Genre, Year)`. Interesująca część polega na tym, jak otrzymujesz wartości z pól tekstowych do `VALUES` części polecenia. Zamiast wartości rzeczywistych widoczne są `@0`, `@1`i `@2`, które są symbolami zastępczymi kursów. Po uruchomieniu polecenia (w wierszu `db.Execute`) przekazywane są wartości z pól tekstowych.

**Ważne!** Należy pamiętać, że jedynym sposobem, w jaki należy uwzględnić dane wprowadzone w trybie online przez użytkownika w instrukcji SQL, jest użycie symboli zastępczych, jak widać tutaj (`VALUES(@0, @1, @2)`). Jeśli połączysz dane wprowadzane przez użytkownika z instrukcją SQL, możesz otworzyć siebie do ataku polegającego na iniekcji SQL, jak wyjaśniono w [formularzach podstawowe informacje na stronach sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581) (poprzedni samouczek).

Nadal wewnątrz bloku `if`, Dodaj następujący wiersz po wierszu `db.Execute`:

[!code-css[Main](entering-data/samples/sample4.css)]

Po wstawieniu nowego filmu do bazy danych ten wiersz przeskakuje użytkownika (przekierowania) do strony *filmy* , aby zobaczyć właśnie wprowadzony film. Operator `~` oznacza "katalog główny witryny sieci Web". (Operator `~` działa tylko na stronach ASP.NET, a nie w języku HTML Generally).

Pełny blok kodu wygląda podobnie do tego przykładu:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Testowanie polecenia INSERT (dotąd)

Nie zostało to jeszcze zrobione, ale teraz jest to dobry czas na przetestowanie.

W widoku drzewa plików w programie WebMatrix kliknij prawym przyciskiem myszy stronę *addmovie. cshtml* , a następnie kliknij polecenie **Uruchom w przeglądarce**.

![Strona "Dodaj film" w przeglądarce](entering-data/_static/image2.png)

(Jeśli w przeglądarce zostanie zainstalowana inna strona, upewnij się, że adres URL jest `http://localhost:nnnnn/AddMovie`), gdzie *nnnnn* jest numerem portu, którego używasz.)

Czy pojawiły się strona błędu? Jeśli tak, przeczytaj ją uważnie i upewnij się, że kod wygląda dokładnie na wymienionym wcześniej.

Wprowadź film w formularzu &mdash; na przykład użyj "Obywatel Kane", "dramat" i "1941". (Lub niezależnie od siebie). Następnie kliknij pozycję **Dodaj film**.

Jeśli wszystko przebiegnie prawidłowo, nastąpi przekierowanie do strony *filmów* . Upewnij się, że Twój nowy film znajduje się na liście.

![Strona filmów pokazująca nowo dodany film](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Sprawdzanie poprawności danych wejściowych użytkownika

Wróć do strony *addmovie* lub uruchom ją ponownie. Wprowadź inny film, ale tym razem wprowadź tylko tytuł, &mdash; na przykład wprowadź "Singin' w deszczu". Następnie kliknij pozycję **Dodaj film**.

Nastąpi ponowne przekierowanie na stronę *filmów* . Nowy film można znaleźć, ale jest on niekompletny.

![Strona filmów pokazująca nowy film, w którym brakuje niektórych wartości](entering-data/_static/image4.png)

Po utworzeniu tabeli *filmów* jawnie określono, że żadne z pól nie może mieć wartości null. Tutaj masz formularz wprowadzania nowych filmów i opuszczasz pola. To jest błąd.

W takim przypadku baza danych w rzeczywistości nie *podnosi lub nie*zgłasza błędu. Nie podano gatunku lub roku, dlatego kod na stronie *addmovie* traktuje te wartości jako *ciągi puste*. Gdy uruchomiono polecenie SQL `Insert Into`, pola gatunek i rok nie miały przydatnych danych, ale nie mają wartości null.

Oczywiście nie chcesz zezwalać użytkownikom na wprowadzanie informacji o połowach pustych do bazy danych. Rozwiązaniem jest zweryfikowanie danych wejściowych użytkownika. Początkowo Walidacja będzie mieć pewność, że użytkownik wprowadził wartość dla wszystkich pól (oznacza to, że żaden z nich nie zawiera pustego ciągu).

> [!TIP]
> 
> **Ciągi o wartości null i puste**
> 
> W programowaniu istnieje rozróżnienie między różnymi pojęciemi "brak wartości". Ogólnie rzecz biorąc, wartość jest *równa null* , jeśli nigdy nie została ustawiona lub nie została zainicjowana w jakikolwiek sposób. Natomiast zmienna, która oczekuje, że dane znakowe (ciągi) można ustawić na *pusty ciąg*. W takim przypadku wartość nie jest równa null; jest on właśnie jawnie ustawiony na ciąg znaków, którego długość wynosi zero. Te dwie instrukcje przedstawiają różnicę:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Jest nieco bardziej skomplikowany niż ten, ale ważnym punktem jest to, że `null` reprezentuje sortowanie stanu nieokreślony.
> 
> Teraz ważne jest, aby dokładnie zrozumieć, kiedy wartość jest równa null i kiedy jest tylko pustym ciągiem. W kodzie strony *addmovie* uzyskasz wartości pól tekstowych przy użyciu `Request.Form["title"]` i tak dalej. Gdy strona jest uruchamiana po raz pierwszy (przed kliknięciem przycisku), wartość `Request.Form["title"]` ma wartość null. Ale podczas przesyłania formularza `Request.Form["title"]` Pobiera wartość pola tekstowego `title`. Nie jest to oczywiste, ale puste pole tekstowe nie ma wartości null; po prostu zawiera pusty ciąg. Tak więc, gdy kod jest uruchamiany w odpowiedzi na kliknięcie przycisku, `Request.Form["title"]` zawiera pusty ciąg.
> 
> Dlaczego to odrębne znaczenie? Po utworzeniu tabeli *filmów* jawnie określono, że żadne z pól nie może mieć wartości null. Ale w tym miejscu masz formularz wprowadzania nowych filmów i opuszczasz pola puste. Przed próbą zapisania nowych filmów, które nie zawierały wartości dla gatunku lub roku, należy oczekiwać, że baza danych ma zaistnieć skargę. Ale jest to punkt &mdash; nawet wtedy, gdy te pola tekstowe zostaną pozostawione puste, wartości nie będą puste. są to puste ciągi. W efekcie można zapisać nowe filmy w bazie danych z tymi kolumnami puste &mdash; ale nie wartości null! wartości &mdash;. W związku z tym trzeba upewnić się, że użytkownicy nie przesyłają pustego ciągu, co można zrobić, sprawdzając dane wejściowe użytkownika.

### <a name="the-validation-helper"></a>Pomocnik walidacji

ASP.NET Web Pages zawiera pomocnika &mdash; `Validation` pomocnik &mdash;, za pomocą którego można upewnić się, że użytkownicy wprowadzają dane spełniające Twoje wymagania. Pomocnik `Validation` jest jednym z pomocników wbudowanych w ASP.NET stron sieci Web, więc nie trzeba instalować jej jako pakietu przy użyciu narzędzia NuGet, w jaki sposób zainstalowano pomocnika skonfigurowali Gravatar w poprzednim samouczku.

Aby sprawdzić poprawność danych wejściowych użytkownika, wykonaj następujące czynności:

- Użyj kodu, aby określić, że wartości w polach tekstowych na stronie mają być wymagane.
- Należy umieścić test w kodzie, aby informacje o filmie były dodawane do bazy danych tylko wtedy, gdy wszystko jest poprawnie zweryfikowane.
- Dodaj kod do znacznika, aby wyświetlić komunikaty o błędach.

W bloku kodu na stronie *Addmovie* na początku przed deklaracjami zmiennych Dodaj następujący kod:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Należy wywołać `Validation.RequireField` raz dla każdego pola (`<input>` elementu), gdzie ma być wymagany wpis. Możesz również dodać niestandardowy komunikat o błędzie dla każdego wywołania, tak jak tutaj. (Zmieniamy komunikaty po prostu, aby pokazać, że możesz umieścić coś w tym miejscu).

Jeśli wystąpił problem, chcesz uniemożliwić wstawianie nowych informacji o filmie do bazy danych programu. W bloku `if(IsPost)` Użyj `&&` (koniunkcja logiczna i), aby dodać kolejny warunek, który testuje `Validation.IsValid()`. Gdy wszystko będzie gotowe, cały blok `if(IsPost)` będzie wyglądać następująco:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Jeśli wystąpi błąd walidacji w dowolnym z pól, które zostały zarejestrowane przy użyciu pomocnika `Validation`, Metoda `Validation.IsValid` zwróci wartość false. W takim przypadku żaden kod w tym bloku nie zostanie uruchomiony, więc żadne nieprawidłowe wpisy filmu nie zostaną wstawione do bazy danych. I oczywiście nie przekierowywać Cię do strony *filmów* .

Pełny blok kodu, w tym kod sprawdzania poprawności, wygląda teraz następująco:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Wyświetlanie błędów walidacji

Ostatnim krokiem jest wyświetlenie wszystkich komunikatów o błędach. Można wyświetlić poszczególne komunikaty dla każdego błędu walidacji lub można wyświetlić podsumowanie albo oba te błędy. W tym samouczku wykonasz oba czynności, aby zobaczyć, jak to działa.

Obok każdego elementu `<input>`, który jest weryfikowany, wywołaj metodę `Html.ValidationMessage` i przekaż ją do nazwy elementu `<input>`, którego dotyczy Walidacja. Należy umieścić metodę `Html.ValidationMessage`, w której ma pojawić się komunikat o błędzie. Po uruchomieniu strony `Html.ValidationMessage` Metoda renderuje `<span>` elementu, w którym zostanie wystawiony błąd walidacji. (Jeśli nie ma żadnego błędu, element `<span>` jest renderowany, ale nie zawiera tekstu.)

Zmień adiustację na stronie tak, aby zawierała metodę `Html.ValidationMessage` dla każdego z trzech elementów `<input>` na stronie, jak w tym przykładzie:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Aby zobaczyć, jak działa podsumowanie, Dodaj również następujące znaczniki i kod po elemencie `<h1>Add a Movie</h1>` na stronie:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Domyślnie metoda `Html.ValidationSummary` wyświetla wszystkie komunikaty weryfikacyjne na liście (element `<ul>` znajdujący się wewnątrz elementu `<div>`). Podobnie jak w przypadku metody `Html.ValidationMessage`, znaczniki dla podsumowania walidacji są zawsze renderowane; Jeśli nie ma żadnych błędów, nie są renderowane żadne elementy listy.

Podsumowanie może być alternatywnym sposobem wyświetlania komunikatów weryfikacyjnych zamiast przy użyciu metody `Html.ValidationMessage` do wyświetlania każdego błędu związanego z polem. Można też użyć podsumowania i szczegółów. Można też użyć metody `Html.ValidationSummary`, aby wyświetlić błąd ogólny, a następnie użyć pojedynczych wywołań `Html.ValidationMessage`, aby wyświetlić szczegóły.

Pełna strona wygląda teraz następująco:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

Gotowe. Możesz teraz przetestować stronę, dodając film, ale pozostawiając co najmniej jedno pole. Gdy to zrobisz, zostanie wyświetlony następujący komunikat o błędzie:

![Dodawanie strony filmu zawierającej komunikaty o błędach walidacji](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Określanie stylu komunikatów o błędach walidacji

Zobaczysz, że są wyświetlane komunikaty o błędach, ale nie są one naprawdę bardzo dobrze. Istnieje prosty sposób na styl komunikatów o błędach.

Aby w stylu poszczególnych komunikatów o błędach, które są wyświetlane przez `Html.ValidationMessage`, Utwórz klasę stylów CSS o nazwie `field-validation-error`. Aby zdefiniować poszukiwanie podsumowania walidacji, Utwórz klasę stylów CSS o nazwie `validation-summary-errors`.

Aby zobaczyć, jak działa ta technika, Dodaj element `<style>` w sekcji `<head>` strony. Następnie zdefiniuj klasy stylów o nazwie `field-validation-error` i `validation-summary-errors`, które zawierają następujące reguły:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Zazwyczaj prawdopodobnie umieścisz informacje o stylu w oddzielnym pliku *. css* , ale dla uproszczenia możesz umieścić je na stronie. (W dalszej części tego samouczka przeniesiesz reguły CSS do oddzielnego pliku *. css* ).

W przypadku błędu walidacji metoda `Html.ValidationMessage` renderuje element `<span>`, który zawiera `class="field-validation-error"`. Dodając definicję stylu dla tej klasy, można skonfigurować wygląd wiadomości. W przypadku wystąpienia błędów Metoda `ValidationSummary` podobnie dynamicznie renderuje atrybut `class="validation-summary-errors"`.

Ponownie uruchom stronę i zanotuj kilka pól. Błędy są teraz bardziej zauważalne. (W rzeczywistości są one overdone, ale właśnie pokazują, co można zrobić).

![Dodawanie strony filmu zawierającej błędy walidacji, które zostały przypisane do stylu](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Dodawanie linku do strony filmów

Jednym z etapów końcowych jest wygodne przechodzenie do strony *addmovie* z oryginalnej listy filmów.

Otwórz ponownie stronę *filmy* . Po tagu `</div>` zamykającego, który następuje po Pomocniku `WebGrid`, Dodaj następujący znacznik:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Zgodnie z oczekiwaniami ASP.NET interpretuje operator `~` jako element główny witryny sieci Web. Nie musisz używać operatora `~`; Możesz użyć znacznika `<a href="./AddMovie">Add a movie</a>` lub innego sposobu definiowania ścieżki zrozumiałej dla kodu HTML. Jednak operator `~` jest dobrym podejściem, gdy tworzysz linki dla stron Razor, ponieważ lokacja jest bardziej elastyczna — Jeśli przenosisz bieżącą stronę do podfolderu, link nadal przejdzie do strony *addmovie* . (Należy pamiętać, że operator `~` działa tylko na stronach *. cshtml* . ASP.NET rozumie, ale nie jest to standardowy kod HTML.

Gdy skończysz, Uruchom stronę *filmy* . Będzie wyglądać podobnie do tej strony:

![Strona filmów z linkiem do strony "Dodawanie filmów"](entering-data/_static/image7.png)

Kliknij link **Dodaj film** , aby upewnić się, że przechodzi do strony *addmovie* .

## <a name="coming-up-next"></a>Przyszłe przejście

W następnym samouczku dowiesz się, jak umożliwić użytkownikom edycję danych znajdujących się już w bazie danych.

## <a name="complete-listing-for-addmovie-page"></a>Pełna lista dla strony addmovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Dodatkowe materiały

- [Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Instrukcja INSERT języka SQL](http://www.w3schools.com/sql/sql_insert.asp) w witrynie w3schools
- [Sprawdzanie poprawności danych wejściowych użytkownika w witrynach stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002). Więcej informacji na temat pracy z pomocnikiem `Validation`.

> [!div class="step-by-step"]
> [Poprzednie](form-basics.md)
> [dalej](updating-data.md)
