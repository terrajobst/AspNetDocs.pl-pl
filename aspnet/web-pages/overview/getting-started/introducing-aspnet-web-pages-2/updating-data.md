---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Wprowadzenie do stron sieci Web ASP.NET — aktualizowanie danych bazy danych | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak zaktualizować istniejący wpis bazy danych przy użyciu stron sieci Web ASP.NET (Razor). Przyjęto założenie, że seria została zakończona...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 8f8bcfb7d9d2416a2699776cadbdaae8e12415ba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574229"
---
# <a name="introducing-aspnet-web-pages---updating-database-data"></a>Wprowadzenie do stron sieci Web ASP.NET — aktualizowanie danych bazy danych

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym samouczku pokazano, jak zaktualizować istniejący wpis bazy danych przy użyciu stron sieci Web ASP.NET (Razor). Przyjęto założenie, że przeprowadzono serię, [wprowadzając dane przy użyciu formularzy przy użyciu stron sieci Web ASP.NET](entering-data.md).
> 
> Zawartość:
> 
> - Jak wybrać pojedynczy rekord w Pomocniku `WebGrid`.
> - Sposób odczytywania pojedynczego rekordu z bazy danych.
> - Jak załadować formularz z wartościami z rekordu bazy danych.
> - Jak zaktualizować istniejący rekord w bazie danych.
> - Jak przechowywać informacje na stronie bez wyświetlania go.
> - Jak używać ukrytego pola do przechowywania informacji.
>   
> 
> Omówione funkcje/technologie:
> 
> - Pomocnik `WebGrid`.
> - Polecenie SQL `Update`.
> - Metoda `Database.Execute`.
> - Ukryte pola (`<input type="hidden">`).

## <a name="what-youll-build"></a>Co będziesz kompilować

W poprzednim samouczku przedstawiono sposób dodawania rekordu do bazy danych. Tutaj znajdziesz informacje na temat wyświetlania rekordu do edycji. Na stronie *filmy* należy zaktualizować pomocnika `WebGrid` tak, aby wyświetlał link **Edytuj** obok każdego filmu:

![Wyświetlanie siatki WebGrid, w tym łącza "Edytuj" dla każdego filmu](updating-data/_static/image1.png)

Kliknięcie linku **edycji** spowoduje przejście do innej strony, gdzie informacje o filmie są już w postaci:

![Edytuj stronę filmu przedstawiającą film do edycji](updating-data/_static/image2.png)

Można zmienić dowolne wartości. Po przesłaniu zmian kod na stronie aktualizuje bazę danych i wraca do listy filmów.

Ta część procesu działa niemal dokładnie tak, jak strona *addmovie. cshtml* utworzona w poprzednim samouczku, dlatego większość tego samouczka będzie znana.

Istnieje kilka sposobów implementacji sposobu edytowania poszczególnych filmów. Pokazana metoda została wybrana, ponieważ jest łatwa do zaimplementowania i łatwego zrozumienia.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Dodawanie linku edycji do listy filmów

Aby rozpocząć, zaktualizujesz stronę *filmy* , tak aby każda lista filmów zawierała link **edycji** .

Otwórz plik *Films. cshtml* .

W treści strony Zmień znacznik `WebGrid`, dodając kolumnę. Oto zmodyfikowano znaczniki:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Nowa kolumna to:

[!code-html[Main](updating-data/samples/sample2.html)]

Punkt tej kolumny polega na wyświetleniu linku (`<a>` elementu), którego tekst mówi "Edit" (Edytuj). To, co mamy, aby utworzyć link, który wygląda jak poniżej, gdy zostanie uruchomiona Strona, z wartością `id` inną dla każdego filmu:

[!code-css[Main](updating-data/samples/sample3.css)]

Ten link spowoduje wywołanie strony o nazwie *EditMovie*i przekazanie `?id=7` ciągu zapytania do tej strony.

Składnia nowej kolumny może wyglądać nieskomplikowaną, ale tylko dlatego, że jest ona połączona z kilkoma elementami. Każdy pojedynczy element jest prosty. Jeśli koncentrujesz się tylko na `<a>` elemencie, zobaczysz następujący znacznik:

[!code-html[Main](updating-data/samples/sample4.html)]

Niektóre tła dotyczące sposobu działania siatki: Siatka wyświetla wiersze, jeden dla każdego rekordu bazy danych i wyświetla kolumny dla każdego pola w rekordzie bazy danych. Gdy jest konstruowany każdy wiersz siatki, obiekt `item` zawiera rekord bazy danych (element) dla tego wiersza. To rozmieszczenie umożliwia przejęcie kodu w celu uzyskania danych dla tego wiersza. Oto co widzisz tutaj: wyrażenie `item.ID` Pobiera wartość identyfikatora bieżącego elementu bazy danych. Możesz uzyskać dowolne wartości bazy danych (tytuł, gatunek lub rok) tak samo, jak przy użyciu `item.Title`, `item.Genre`lub `item.Year`.

Wyrażenie `"~/EditMovie?id=@item.ID` łączy trwale zakodowaną część docelowego adresu URL (`~/EditMovie?id=`) z tym dynamicznym IDENTYFIKATORem pochodnym. (W poprzednim samouczku wykorzystano operator `~`. jest to operator ASP.NET, który reprezentuje bieżący katalog główny witryny sieci Web).

Wynikiem tego jest to, że ta część znaczników w kolumnie po prostu generuje coś podobnie jak w przypadku następujących znaczników w czasie wykonywania:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalnie wartość `id` będzie różna dla każdego wiersza.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Tworzenie niestandardowego ekranu dla kolumny siatki

Wróć do kolumny siatka. Trzy kolumny w siatce zawierały tylko wartości danych (tytuł, gatunek i rok). Ten ekran został określony przez przekazanie nazwy kolumny bazy danych &mdash; na przykład `grid.Column("Title")`.

Ta nowa kolumna łącza **edycji** jest inna. Zamiast określać nazwę kolumny, przekazujesz parametr `format`. Ten parametr umożliwia zdefiniowanie znaczników, które pomocnik `WebGrid` będzie renderowany wraz z wartością `item`, aby wyświetlić dane kolumny jako pogrubione lub zielone albo w dowolnym formacie, który chcesz. Jeśli na przykład tytuł ma być pogrubiony, można utworzyć kolumnę podobną do tego przykładu:

[!code-html[Main](updating-data/samples/sample6.html)]

(Różne znaki `@` widoczne we właściwości `format` oznaczają przejście między adiustacją a wartością kodu).

Po uzyskaniu informacji o właściwości `format` łatwiej jest zrozumieć, w jaki sposób nowa kolumna **Edytuj** łącze:

[!code-html[Main](updating-data/samples/sample7.html)]

Kolumna zawiera *tylko* znaczniki, które renderuje łącze, oraz informacje (identyfikator) wyodrębnione z rekordu bazy danych dla wiersza.

> [!TIP]
> 
> **Parametry nazwane i parametry pozycyjne dla metody**
> 
> Wiele razy, gdy wywołano metodę i przeszedł do niej parametry, zostały po prostu wyświetlone wartości parametrów oddzielone przecinkami. Oto kilka przykładów:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Nie wspominamy problemu podczas pierwszego korzystania z tego kodu, ale w każdym przypadku przekazujesz parametry do metod w określonej kolejności &mdash; a mianowicie, kolejność, w której parametry są zdefiniowane w tej metodzie. W przypadku `db.Execute` i `Validation.RequireFields`, jeśli połączysz kolejność przekazanych wartości, otrzymasz komunikat o błędzie podczas uruchamiania strony lub co najmniej niektóre dziwne wyniki. Jasno trzeba znać kolejność przekazywania parametrów w. (W programie WebMatrix technologia IntelliSense może pomóc Ci w Poznaniu nazwy, typu i kolejności parametrów).
> 
> Alternatywnie, aby przekazywać wartości w kolejności, można użyć *nazwanych parametrów*. (Przekazywanie parametrów w kolejności jest znane jako użycie *parametrów pozycyjnych*). W przypadku parametrów nazwanych jawnie dołączana jest nazwa parametru podczas przekazywania jego wartości. W tych samouczkach użyto już parametrów nazwanych. Na przykład:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> i
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Parametry nazwane są przydatne w kilku sytuacjach, zwłaszcza w przypadku, gdy metoda przyjmuje wiele parametrów. Jeden jest, gdy chcesz przekazać tylko jeden lub dwa parametry, ale wartości, które mają zostać przekazane, nie należą do pierwszej pozycji na liście parametrów. Inna sytuacja polega na tym, że kod może być bardziej czytelny przez przekazanie parametrów w kolejności, która ma największe znaczenie.
> 
> Oczywiście, aby używać parametrów nazwanych, należy znać nazwy parametrów. Funkcja IntelliSense w technologii WebMatrix może *wyświetlić* nazwy, ale nie może ich obecnie wypełnić.

## <a name="creating-the-edit-page"></a>Tworzenie strony edytowania

Teraz możesz utworzyć stronę *EditMovie* . Po kliknięciu linku **edycji** użytkownicy będą mogli zakończyć na tej stronie.

Utwórz stronę o nazwie *EditMovie. cshtml* i Zastąp zawartość pliku następującym znacznikiem:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Znaczniki i kod są podobne do zawartości na stronie *addfilm* . Tekst przycisku Prześlij jest niewielką różnicą. Podobnie jak na stronie *addmovie* istnieje wywołanie `Html.ValidationSummary`, które spowoduje wyświetlenie błędów walidacji, jeśli istnieją. Tym razem trwa opuszczanie wywołań do `Validation.Message`, ponieważ błędy będą wyświetlane w podsumowaniu walidacji. Jak wspomniano w poprzednim samouczku, można użyć podsumowania walidacji i poszczególnych komunikatów o błędach w różnych kombinacjach.

Zwróć uwagę, że atrybut `method` elementu `<form>` jest ustawiony na `post`. Podobnie jak na stronie *addmovie. cshtml* , ta strona wprowadza zmiany w bazie danych. W związku z tym formularz powinien wykonać operację `POST`. (Aby uzyskać więcej informacji o różnicach między operacjami `GET` i `POST`, zobacz pasek boczny [bezpieczeństwa czasowników Get, post i http](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) w samouczku dotyczącym formularzy HTML.)

Zgodnie z wcześniejszym samouczkiem `value` atrybuty pól tekstowych są ustawiane za pomocą kodu Razor w celu ich wstępnego załadowania. Tym razem używamy zmiennych, takich jak `title` i `genre` dla tego zadania zamiast `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Tak jak wcześniej, ten znacznik spowoduje wstępne załadowanie wartości pola tekstowego przy użyciu wartości filmu. Zobaczysz tutaj, dlaczego warto używać zmiennych tej godziny zamiast używać obiektu `Request`.

Na tej stronie znajduje się również element `<input type="hidden">`. Ten element przechowuje identyfikator filmu bez udostępniania go na stronie. Identyfikator jest początkowo przekazywać do strony przy użyciu wartości ciągu zapytania (`?id=7` lub podobnej w adresie URL). Przez umieszczenie wartości identyfikatora w ukrytym polu, można upewnić się, że jest ona dostępna podczas przesyłania formularza, nawet jeśli nie masz już dostępu do oryginalnego adresu URL, z którym została wywołana strona.

W przeciwieństwie do strony *addmovie* , kod dla strony *EditMovie* ma dwie odrębne funkcje. Pierwsza funkcja polega na tym, że gdy strona jest wyświetlana po raz pierwszy (i *tylko* wtedy), kod pobiera identyfikator filmu z ciągu zapytania. Następnie kod używa identyfikatora w celu odczytania odpowiedniego filmu z bazy danych i wyświetlenia (wstępnego ładowania) w polach tekstowych.

Druga funkcja polega na tym, że gdy użytkownik kliknie przycisk **Prześlij zmiany** , kod musi odczytać wartości pól tekstowych i sprawdzić ich poprawność. Kod musi również zaktualizować element bazy danych o nowe wartości. Ta technika przypomina Dodawanie rekordu, jak pokazano w *addfilm*.

## <a name="adding-code-to-read-a-single-movie"></a>Dodawanie kodu w celu odczytania pojedynczego filmu

Aby wykonać pierwszą funkcję, Dodaj ten kod w górnej części strony:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Większość tego kodu znajduje się wewnątrz bloku, który zaczyna się `if(!IsPost)`. Operator `!` oznacza "not", więc wyrażenie oznacza, że *to żądanie nie jest przesłaniem po* *raz pierwszy*, czyli pośrednim sposobem wymawiania tego żądania, gdy ta strona została uruchomiona. Jak wspomniano wcześniej, ten kod powinien być uruchamiany *tylko* podczas pierwszego uruchomienia strony. Jeśli kod nie został ujęty w `if(!IsPost)`, będzie uruchamiany za każdym razem, gdy zostanie wywołana Strona, niezależnie od tego, czy po raz pierwszy lub w odpowiedzi na kliknięcie przycisku.

Należy zauważyć, że kod zawiera blok `else` tym razem. Jak wspomniano, gdy wprowadziliśmy `if` bloki, Czasami chcesz uruchomić alternatywny kod, jeśli testowany warunek nie jest spełniony. Dzieje się tak w tym miejscu. Jeśli warunek zostanie spełniony (oznacza to, że jeśli identyfikator przekazywany do strony jest prawidłowy), odczytasz wiersz z bazy danych. Jeśli jednak warunek nie zostanie spełniony, zostanie uruchomiony blok `else` i kod ustawi komunikat o błędzie.

## <a name="validating-a-value-passed-to-the-page"></a>Sprawdzanie poprawności wartości przesłanej do strony

Kod używa `Request.QueryString["id"]`, aby uzyskać identyfikator, który jest przesyłany do strony. Kod sprawdza, czy wartość jest faktycznie przenoszona dla tego identyfikatora. Jeśli nie przekazano żadnej wartości, kod ustawia błąd walidacji.

Ten kod przedstawia inny sposób sprawdzania poprawności informacji. W poprzednim samouczku pracowałeś z pomocnika `Validation`. Zarejestrowano pola do walidacji, a ASP.NET automatycznie przeprowadził weryfikację i wyświetlane błędy przy użyciu `Html.ValidationMessage` i `Html.ValidationSummary`. W tym przypadku nie jest to jednak naprawdę Weryfikowanie danych wejściowych użytkownika. Zamiast tego jest sprawdzana wartość, która została przeniesiona na stronę z innego miejsca. Pomocnik `Validation` nie wykonuje tej czynności.

W związku z tym sprawdzasz wartość samodzielnie, testując ją z `if(!Request.QueryString["ID"].IsEmpty()`). Jeśli wystąpi problem, można wyświetlić błąd przy użyciu `Html.ValidationSummary`, jak w przypadku pomocnika `Validation`. W tym celu należy wywołać `Validation.AddFormError` i przekazać mu komunikat do wyświetlenia. `Validation.AddFormError` to wbudowana Metoda, która umożliwia Definiowanie niestandardowych komunikatów, które są dostępne w systemie weryfikacji, które są już znane. (W dalszej części tego samouczka dowiesz się, jak przeprowadzić proces weryfikacji nieco bardziej niezawodny).

Po upewnieniu się, że istnieje identyfikator filmu, kod odczytuje bazę danych, szukając tylko pojedynczego elementu bazy danych. (Prawdopodobnie zauważono ogólny wzorzec dla operacji bazy danych: Otwórz bazę danych, zdefiniuj instrukcję SQL i uruchom instrukcję). Tym razem instrukcja SQL `Select` zawiera `WHERE ID = @0`. Ponieważ identyfikator jest unikatowy, można zwrócić tylko jeden rekord.

Zapytanie jest wykonywane przy użyciu `db.QuerySingle` (nie `db.Query`, jak używane dla listy filmowej), a kod umieszcza wynik w zmiennej `row`. Nazwa `row` jest dowolnie. Możesz nadać nazwy zmiennym. Zmienne zainicjowane u góry są wypełniane informacjami o filmie, dzięki czemu te wartości mogą być wyświetlane w polach tekstowych.

## <a name="testing-the-edit-page-so-far"></a>Testowanie strony edycji (do tej pory)

Jeśli chcesz przetestować stronę, Uruchom teraz stronę *filmów* i kliknij link **Edytuj** obok dowolnego filmu. Zostanie wyświetlona strona *EditMovie* z informacjami podanymi dla wybranego filmu:

![Edytuj stronę filmu przedstawiającą film do edycji](updating-data/_static/image3.png)

Zwróć uwagę, że adres URL strony zawiera coś takiego jak `?id=10` (lub inną liczbę). Do tej pory sprawdzono, że linki **edycji** na stronie *filmu* działają, że strona odczytuje identyfikator z ciągu zapytania, a zapytanie bazy danych mające na celu uzyskanie pojedynczego rekordu filmu działa.

Możesz zmienić informacje o filmie, ale nic się nie dzieje po kliknięciu przycisku **Prześlij zmiany**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Dodawanie kodu w celu zaktualizowania filmu ze zmianami użytkownika

W pliku *EditMovie. cshtml* , aby zaimplementować drugą funkcję (zapisanie zmian), Dodaj następujący kod tuż wewnątrz zamykającego nawiasu klamrowego bloku `@`. (Jeśli nie masz pewności, gdzie należy umieścić kod, możesz zapoznać się z [pełną listą kodu dla strony Edytuj film](#Complete_Page_Listing_for_EditMovie) , która pojawia się na końcu tego samouczka).

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Ponownie znaczniki i kod są podobne do kodu w *addfilm*. Kod znajduje się w bloku `if(IsPost)`, ponieważ ten kod jest uruchamiany tylko wtedy, gdy użytkownik kliknie przycisk **Prześlij zmiany** &mdash;, gdy (i tylko wtedy, gdy) formularz został opublikowany. W tym przypadku nie używasz testu, takiego jak `if(IsPost && Validation.IsValid())`— to znaczy, że nie łączysz obu testów za pomocą i. Na tej stronie należy najpierw określić, czy jest przesyłany formularz (`if(IsPost)`), a następnie zarejestrować pola do walidacji. Następnie można testować wyniki walidacji (`if(Validation.IsValid()`). Przepływ jest nieco inny niż na stronie *addmovie. cshtml* , ale efekt jest taki sam.

Wartości pól tekstowych są uzyskiwane przy użyciu `Request.Form["title"]` i podobnego kodu dla innych elementów `<input>`. Zauważ, że w tym czasie kod pobiera identyfikator filmu z ukrytego pola (`<input type="hidden">`). Gdy strona jest uruchamiana po raz pierwszy, kod uzyskał identyfikator z ciągu zapytania. Otrzymujesz wartość z ukrytego pola, aby upewnić się, że otrzymujesz identyfikator filmu, który był pierwotnie wyświetlany, na wypadek gdyby ciąg zapytania był w jakiś sposób modyfikowany od momentu.

Istotna różnica między kodem *addmovie* a tym kodem polega na tym, że w tym kodzie używasz instrukcji SQL `Update`, a nie instrukcji `Insert Into`. W poniższym przykładzie przedstawiono składnię instrukcji SQL `Update`:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Można określić dowolne kolumny w dowolnej kolejności i nie trzeba aktualizować każdej kolumny w trakcie operacji `Update`. (Nie można zaktualizować samego identyfikatora, ponieważ spowodowałoby to zapisanie rekordu jako nowego rekordu i niedozwolone dla operacji `Update`).

> [!NOTE] 
> 
> **Ważne** Klauzula `Where` o IDENTYFIKATORze jest bardzo ważna, ponieważ jest to sposób, w jaki baza danych wie, który rekord bazy danych ma zostać zaktualizowany. Jeśli pozostawiłeś klauzulę `Where`, baza danych zaktualizuje *każdy* rekord w bazie danych. W większości przypadków jest to awaria.

W kodzie wartości do zaktualizowania są przesyłane do instrukcji SQL przy użyciu symboli zastępczych. Aby powtórzyć powyższe czynności: ze względów bezpieczeństwa Użyj symboli zastępczych *tylko* do przekazania wartości do instrukcji SQL.

Gdy kod używa `db.Execute` do uruchomienia instrukcji `Update`, przekierowuje do strony z listą, gdzie można zobaczyć zmiany.

> [!TIP] 
> 
> **Różne instrukcje SQL, różne metody**
> 
> Być może zauważono, że używasz nieco różnych metod do uruchamiania różnych instrukcji SQL. Aby uruchomić kwerendę `Select`, która potencjalnie zwraca wiele rekordów, należy użyć metody `Query`. Aby uruchomić zapytanie `Select`, które wie, że zwrócisz tylko jeden element bazy danych, należy użyć metody `QuerySingle`. Aby uruchomić polecenia, które wprowadzają zmiany, ale nie zwracają elementów bazy danych, należy użyć metody `Execute`.
> 
> Musisz mieć różne metody, ponieważ każdy z nich zwraca różne wyniki, ponieważ wystąpiły już różnice między `Query` i `QuerySingle`. (Metoda `Execute` faktycznie zwraca wartość, &mdash; mianowicie, liczbę wierszy bazy danych, na które miało wpływ polecenie &mdash; ale zostało to dotąd zignorowane.)
> 
> Oczywiście Metoda `Query` może zwrócić tylko jeden wiersz bazy danych. Jednak ASP.NET zawsze traktuje wyniki metody `Query` jako kolekcje. Nawet jeśli metoda zwraca tylko jeden wiersz, należy wyodrębnić ten pojedynczy wiersz z kolekcji. Z tego względu, w sytuacjach, gdy *wiesz* , że będziesz otrzymywać tylko jeden wiersz, jest to nieco bardziej wygodne do użycia `QuerySingle`.
> 
> Istnieje kilka innych metod wykonywania określonych typów operacji bazy danych. Listę metod bazy danych można znaleźć w [szybkim Kompendium interfejsu API stron sieci Web ASP.NET](../../api-reference/asp-net-web-pages-api-reference.md#Data).

## <a name="making-validation-for-the-id-more-robust"></a>Sprawdzanie poprawności identyfikatora w sposób bardziej niezawodny

Przy pierwszym uruchomieniu strony otrzymujesz identyfikator filmu z ciągu zapytania, aby można było pobrać ten film z bazy danych. Upewnij się, że faktycznie była dostępna wartość, dla której wyszukiwany jest ten kod:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Ten kod został użyty do upewnienia się, że jeśli użytkownik przejdzie do strony *EditMovies* bez wcześniejszego wybrania filmu na stronie *filmy* , na stronie zostanie wyświetlony przyjazny dla użytkownika komunikat o błędzie. (W przeciwnym razie użytkownicy zobaczą komunikat o błędzie, który prawdopodobnie go zamylić).

Jednak sprawdzanie poprawności nie jest bardzo niezawodne. Ta strona może być również wywoływana z następującymi błędami:

- Identyfikator nie jest liczbą. Na przykład strona może być wywoływana przy użyciu adresu URL, takiego jak `http://localhost:nnnnn/EditMovie?id=abc`.
- Identyfikator jest liczbą, ale odwołuje się do filmu, który nie istnieje (na przykład `http://localhost:nnnnn/EditMovie?id=100934`).

Jeśli chcesz wiedzieć się z błędami, które wynikają z tych adresów URL, Uruchom stronę *filmy* . Wybierz film do edycji, a następnie Zmień adres URL strony *EditMovie* na adres URL, który zawiera identyfikator alfabetyczny lub identyfikator nieistniejącego filmu.

Co należy zrobić? Pierwsza poprawka ma na celu upewnienie się, że nie tylko jest to identyfikator przesłany do strony, ale identyfikator jest liczbą całkowitą. Zmień kod dla testu `!IsPost` tak, aby wyglądał następująco:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Do testu `IsEmpty` został dodany drugi warunek połączony z `&&` (koniunkcja logiczna i):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Zapamiętaj, że możesz zapamiętać o samouczku [programowanie stron sieci Web ASP.NET](../introducing-razor-syntax-c.md) , które metody, takie jak `AsBool` `AsInt` konwersji ciągu znaków na inne typy danych. Metoda `IsInt` (i inne, takie jak `IsBool` i `IsDateTime`) są podobne. Jednak testuje tylko, czy *można* przekonwertować ciąg, bez faktycznego wykonania konwersji. W tym miejscu mówiąc, *czy wartość ciągu zapytania może być konwertowana na liczbę całkowitą...*

Innym potencjalnym problemem jest wyszukiwanie filmu, który nie istnieje. Kod do pobrania filmu wygląda podobnie do tego kodu:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Jeśli przekażesz wartość `movieId` do metody `QuerySingle`, która nie odpowiada rzeczywistemu filmowi, nic nie zostanie zwrócone i instrukcje, które obserwują (na przykład `title=row.Title`) powodują błędy.

Ponownie jest prosta poprawka. Jeśli metoda `db.QuerySingle` nie zwróci żadnych wyników, zmienna `row` będzie równa null. Aby sprawdzić, czy zmienna `row` ma wartość null, przed podjęciem próby pobrania z niej wartości. Poniższy kod dodaje blok `if` wokół instrukcji, które pobierają wartości z obiektu `row`:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Dzięki tym dwóm dodatkowym testom weryfikacyjnym strona otrzymuje więcej punktorów. Pełen kod dla gałęzi `!IsPost` teraz wygląda następująco:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Zanotujemy, że to zadanie jest dobrym użyciem dla bloku `else`. Jeśli testy nie zachodzą, bloki `else` ustawiają komunikaty o błędach.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Dodawanie linku do powrotu do strony filmów

Ostateczne i przydatne szczegóły to dodanie łącza z powrotem do strony *filmów* . W zwykłym przepływie zdarzeń użytkownicy uruchomili się na stronie *filmów* i klikają link **edycji** . Spowoduje to przełączenie ich do strony *EditMovie* , na której można edytować film, a następnie kliknąć przycisk. Gdy kod przetworzy zmianę, przekierowuje ponownie do strony *filmów* .

Ale

- Użytkownik może zrezygnować z zmian.
- Użytkownik mógł uzyskać dostęp do tej strony bez uprzedniego kliknięcia linku **edycji** na stronie *filmów* .

W obu przypadkach chcesz ułatwić użytkownikom powrót do listy głównej. Jest to Łatwa poprawka &mdash; Dodaj następujące znaczniki tuż po tagu zamykającym `</form>` w znaczniku:

[!code-html[Main](updating-data/samples/sample19.html)]

Ten znacznik używa tej samej składni dla elementu `<a>`, który był widoczny w innym miejscu. Adres URL zawiera `~` oznaczający "katalog główny witryny sieci Web".

## <a name="testing-the-movie-update-process"></a>Testowanie procesu aktualizacji filmu

Teraz można testować. Uruchom stronę *filmy* , a następnie kliknij pozycję **Edytuj** obok filmu. Gdy zostanie wyświetlona strona *EditMovie* , wprowadź zmiany w filmie i kliknij pozycję **Prześlij zmiany**. Gdy zostanie wyświetlona lista filmów, upewnij się, że zmiany są wyświetlane.

Aby upewnić się, że walidacja działa, kliknij pozycję **Edytuj** dla innego filmu. Po przeniesieniu do strony *EditMovie* wyczyść pole **gatunek** (lub pole **rok** lub oba), a następnie spróbuj przesłać zmiany. Zobaczysz błąd, zgodnie z oczekiwaniami:

![Edytuj stronę filmu przedstawiającą błędy walidacji](updating-data/_static/image4.png)

Kliknij link **Wróć do listy filmów** , aby porzucić zmiany i powrócić do strony *filmów* .

## <a name="coming-up-next"></a>Przyszłe przejście

W następnym samouczku zobaczysz, jak usunąć rekord filmu.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Ukończ listę dla strony filmu (Zaktualizowano za pomocą linków edycji)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Kompletna lista stron do edycji strony filmu

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Dodatkowe materiały

- [Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrukcja SQL Update](http://www.w3schools.com/sql/sql_update.asp) w witrynie w3schools

> [!div class="step-by-step"]
> [Poprzednie](entering-data.md)
> [dalej](deleting-data.md)
