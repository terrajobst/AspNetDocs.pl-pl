---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Iteracja #6 — Korzystanie z projektowania opartego na testach (VB) | Microsoft Docs'
author: microsoft
description: W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: b166a1c6af29206d43558fa7de447c3f4da2ddfe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78608305"
---
# <a name="iteration-6--use-test-driven-development-vb"></a>Iteracja #6 — Korzystanie z projektowania opartego na testach (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji dodamy grupy kontaktów.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji Contact Management ASP.NET MVC (VB)

W tej serii samouczków tworzymy całą aplikację do zarządzania kontaktami od początku do końca. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazw, numerów telefonów i adresów e-mail — w celu uzyskania listy osób.

Aplikacja została utworzona przez wiele iteracji. W przypadku każdej iteracji stopniowo ulepszamy aplikację. Celem tej wielu iteracji jest umożliwienie zrozumienia przyczyny każdej zmiany.

- #1 iteracji — Utwórz aplikację. W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD).

- Iteracja #2 — Zwiększ wygląd aplikacji. W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również adresy e-mail i numery telefonów.

- Iteracja #4 — możliwość swobodnego łączenia aplikacji. W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład Refaktoryzacja naszej aplikacji używa wzorca repozytorium i wzorca iniekcji zależności.

- #5 iteracji — Utwórz testy jednostkowe. W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy klasy modelu danych i kompilujemy testy jednostkowe dla naszych kontrolerów i logiki walidacji.

- Iteracja #6 — Użyj programowania opartego na testach. W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji dodamy grupy kontaktów.

- Iteracja #7 — Dodawanie funkcji AJAX. W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Ta iteracja

W poprzedniej iteracji aplikacji Contact Manager utworzyliśmy testy jednostkowe w celu zapewnienia bezpieczeństwa sieci w naszym kodzie. Motywacja do tworzenia testów jednostkowych była bardziej odporna na zmianę kodu. W przypadku testów jednostkowych możemy Happily wprowadzić wszelkie zmiany w naszym kodzie i natychmiast wiedzieć, czy zostały naruszone istniejące funkcje.

W tej iteracji używamy testów jednostkowych do zupełnie różnych celów. W tej iteracji używamy testów jednostkowych jako części opłaty projektowej aplikacji zwanej *programowaniem opartym na testach*. W przypadku programowania opartego na testach należy najpierw napisać testy, a następnie napisać kod dla testów.

Dokładniej, podczas wdrażania opartego na testach, istnieją trzy kroki, które należy wykonać podczas tworzenia kodu (czerwony/zielony/Refaktoryzacja):

1. Napisz test jednostkowy, który zakończy się niepowodzeniem (czerwony)
2. Napisz kod, który przekazuje test jednostkowy (zielony)
3. Refaktoryzacja kodu (Refaktoryzacja)

Najpierw napiszesz test jednostkowy. Test jednostkowy powinien wyrazić swój zamiar w oczekiwaniu na zachowanie kodu. Podczas pierwszego tworzenia testu jednostkowego, test jednostkowy powinien kończyć się niepowodzeniem. Test powinien zakończyć się niepowodzeniem, ponieważ nie zapisano jeszcze kodu aplikacji spełniającego testy.

Następnie napisz wystarczy kod, aby test jednostkowy mógł zostać przekazany. Celem jest pisanie kodu w laziest, sloppiest i najszybszy możliwy sposób. Nie należy tracić czasu na architekturę aplikacji. Zamiast tego należy skoncentrować się na zapisaniu minimalnej ilości kodu niezbędnej do zaspokojenia zamiaru wyrażonej przez test jednostkowy.

Na koniec po zapisaniu wystarczającej ilości kodu można wykonać krok wstecz i rozważyć ogólną architekturę aplikacji. W tym kroku należy napisać ponownie (refaktoryzację) swój kod, wykorzystując zalety wzorców projektowych oprogramowania — takich jak wzorzec repozytorium — tak, aby kod był bardziej konserwowany. Możesz fearlessly ponownie napisać kod w tym kroku, ponieważ kod jest objęty testami jednostkowymi.

Istnieje wiele korzyści wynikających z wdrażania opartego na testach. W pierwszej kolejności programowanie sterowane testami wymusza skoncentrowanie się na kodzie, który faktycznie musi być zapisany. Ze względu na to, że stale napisanie wystarczającej ilości kodu do przekazania określonego testu, nie można Wandering do chwastów i pisania ogromnych ilości kodu, które nigdy nie będą używane.

Druga metoda projektowania "test First" wymusza napisanie kodu z perspektywy użycia kodu. Innymi słowy, gdy jest to zgodne z programowaniem opartym na testach, nieustannie piszesz testy z perspektywy użytkownika. Z tego względu programowanie sterowane testami może spowodować przejrzyste i bardziej zrozumiałe interfejsy API.

Na koniec programowanie sterowane testami wymusza możliwość pisania testów jednostkowych w ramach normalnego procesu tworzenia aplikacji. Zgodnie z podejściem ostatecznym projektu testowanie jest zazwyczaj pierwszym krokiem, który przejdzie do okna. W przypadku praktycznego programowania opartego na testach, z drugiej strony, najprawdopodobniej będzie składa na temat pisania testów jednostkowych, ponieważ rozwój oparty na testach sprawia, że testy jednostkowe są centralne dla procesu tworzenia aplikacji.

> [!NOTE] 
> 
> Aby dowiedzieć się więcej na temat programowania opartego na testach, zalecamy zapoznanie się **z**artykułem Michael.

W tej iteracji dodamy nową funkcję do naszej aplikacji Contact Manager. Dodaliśmy obsługę grup kontaktów. Korzystając z grup kontaktów, można organizować kontakty w kategorie, takie jak grupy biznesowe i zaprzyjaźnione.

Dodamy tę nową funkcjonalność do naszej aplikacji, wykonując proces projektowania opartego na testach. Będziemy najpierw napisać nasze testy jednostkowe, a my zapiszemy cały nasz kod w oparciu o te testy.

## <a name="what-gets-tested"></a>Co zostanie przetestowane

Zgodnie z opisem w poprzedniej iteracji zazwyczaj nie są zapisywane testy jednostkowe dla logiki dostępu do danych ani logiki wyświetlania. Nie można napisać testów jednostkowych dla logiki dostępu do danych, ponieważ uzyskanie dostępu do bazy danych jest relatywną niską operacją. Nie napiszesz testów jednostkowych dla logiki wyświetlania, ponieważ dostęp do widoku wymaga nawirowania serwera sieci Web, który jest stosunkowo wolno działaniem. Nie należy pisać testu jednostkowego, chyba że test można wykonać zbyt szybko i ponownie

Ponieważ Programowanie oparte na testach jest sterowane przez testy jednostkowe, koncentrujemy się na pierwszym miejscu na pisaniu kontrolerów i logiki biznesowej. Unikamy dotknięcia bazy danych lub widoków. Nie zmodyfikujemy bazy danych ani nie utworzysz jej widoków do momentu zakończenia tego samouczka. Zaczynamy od tego, co można przetestować.

## <a name="creating-user-stories"></a>Tworzenie scenariuszy użytkownika

W przypadku wdrażania opartego na testach zawsze zaczyna się od pisania testu. To natychmiast wywołuje pytanie: jak określić, jaki test ma być wpisany najpierw? Aby odpowiedzieć na to pytanie, należy napisać zestaw [*historii użytkownika*](http://en.wikipedia.org/wiki/User_stories).

Scenariusz użycia to bardzo krótkie (zwykle jedno zdanie) opis wymagania dotyczącego oprogramowania. Powinien być nietechnicznym opisem wymagania zarejestrowanego z perspektywy użytkownika.

W tym miejscu znajduje się zestaw scenariuszy użytkowników opisujących funkcje wymagane przez nową funkcję grupy kontaktów:

1. Użytkownik może wyświetlić listę grup kontaktów.
2. Użytkownik może utworzyć nową grupę kontaktów.
3. Użytkownik może usunąć istniejącą grupę kontaktów.
4. Użytkownik może wybrać grupę kontaktów podczas tworzenia nowego kontaktu.
5. Użytkownik może wybrać grupę kontaktów podczas edytowania istniejącej osoby kontaktowej.
6. W widoku indeks zostanie wyświetlona lista grup kontaktów.
7. Gdy użytkownik kliknie grupę kontaktów, zostanie wyświetlona lista pasujących kontaktów.

Należy zauważyć, że ta lista przypadków użycia jest w pełni zrozumiała dla klienta. Nie ma wzmianki o szczegółach implementacji technicznej.

Podczas tworzenia aplikacji zestaw scenariuszy użycia może stać się bardziej rafinowany. Możesz przerwać historię użytkownika w wielu wątkach (wymaganiach). Na przykład można zdecydować, że utworzenie nowej grupy kontaktów powinno wiązać się z walidacją. Przesyłanie grupy kontaktów bez nazwy powinno zwracać błąd walidacji.

Po utworzeniu listy historii użytkownika możesz napisać pierwszy test jednostkowy. Rozpocznie się tworzenie testów jednostkowych na potrzeby wyświetlania listy grup kontaktów.

## <a name="listing-contact-groups"></a>Wyświetlanie listy grup kontaktów

Naszym pierwszym scenariuszem użycia jest możliwość wyświetlania listy grup kontaktów przez użytkownika. Musimy przedstawić ten scenariusz z testem.

Utwórz nowy test jednostkowy, klikając prawym przyciskiem myszy folder controllers w projekcie ContactManager. Tests, wybierając pozycję **Dodaj, nowy test**i wybierając szablon **testu jednostkowego** (patrz rysunek 1). Nazwij nowy test jednostkowy GroupControllerTest. vb i kliknij przycisk **OK** .

[![Dodaj test jednostkowy GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Ilustracja 01**. Dodawanie testu jednostkowego GroupControllerTest ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image2.png))

Nasz pierwszy test jednostkowy znajduje się na liście 1. Ten test sprawdza, czy metoda index () kontrolera grupy zwraca zestaw grup. Test weryfikuje, czy kolekcja grup jest zwracana w danych widoku.

**Lista 1 — Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Gdy po raz pierwszy wpiszesz kod w liście 1 w programie Visual Studio, uzyskasz wiele czerwonych linii falistej. Nie utworzono klas GroupController ani Group.

W tym momencie możemy nawet utworzyć naszą aplikację, aby można było wykonać nasze pierwsze testy jednostkowe. To wszystko. Jest to liczone jako niepowodzenie testu. W związku z tym mamy teraz uprawnienia do rozpoczęcia pisania kodu aplikacji. Musimy napisać kod wystarczający do wykonania testu.

Klasa kontrolera grupy znajdująca się na liście 2 zawiera minimalny kod, który jest wymagany do przekazania testu jednostkowego. Akcja index () zwraca statycznie zakodowaną listę grup (Klasa grupy jest zdefiniowana w liście 3).

**Lista 2 — Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Lista 3 — Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Po dodaniu klas GroupController i Group do projektu nasz pierwszy test jednostkowy zostanie zakończony pomyślnie (patrz rysunek 2). Wprowadziliśmy minimalną ilość pracy wymaganą do przekazania testu. Jest to czasochłonne.

[![powodzenie!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Ilustracja 02**: sukces! ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image4.png))

## <a name="creating-contact-groups"></a>Tworzenie grup kontaktów

Teraz możemy przejść do drugiego scenariusza użytkownika. Musimy mieć możliwość tworzenia nowych grup kontaktów. Musimy wyrazić ten zamiar w teście.

Test na liście 4 weryfikuje, że wywołanie metody Create () z nową grupą dodaje grupę do listy grup zwracanych przez metodę index (). Innymi słowy, jeśli tworzę nową grupę, chcę mieć możliwość przywrócenia nowej grupy z listy grup zwracanych przez metodę index ().

**Lista 4 — Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Test w liście 4 wywołuje metodę Create () kontrolera grupy z nową grupą kontaktów. Następnie test weryfikuje, że wywołanie metody index () kontrolera grupy zwraca nową grupę w widoku dane.

Zmodyfikowany kontroler grupy na liście 5 zawiera minimum zmiany wymagane do przekazania nowego testu.

**Lista 5 — Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Kontroler grupy na liście 5 ma nową akcję Create (). Ta akcja dodaje grupę do kolekcji grup. Zwróć uwagę, że akcja index () została zmodyfikowana w celu zwrócenia zawartości kolekcji grup.

Po ponownym uruchomieniu test jednostkowy potrzebuje minimalnej ilości pracy wymaganej przez program. Po wprowadzeniu tych zmian do kontrolera grupy wszystkie testy jednostkowe zostaną zakończone pomyślnie.

## <a name="adding-validation"></a>Dodawanie walidacji

Ten wymóg nie został jawnie określony w scenariuszu użytkownika. Jednak jest uzasadnione wymaganie, aby grupa miała nazwę. W przeciwnym razie organizowanie kontaktów do grup nie będzie bardzo przydatne.

Lista 6 zawiera nowy test, który wyraża ten zamiar. Ten test sprawdza, czy próba utworzenia grupy bez podawania nazwy skutkuje komunikatem o błędzie walidacji w stanie modelu.

**Lista 6 — Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Aby można było spełnić ten test, musimy dodać właściwość Name do naszej klasy grupy (patrz lista 7). Ponadto musimy dodać mały bit logiki walidacji do naszej akcji Create () kontrolera grupy (zobacz Lista 8).

**Lista 7 — Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Lista 8-Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Zauważ, że akcja Utwórz kontroler grupy () zawiera teraz zarówno walidację, jak i logikę bazy danych. Obecnie baza danych używana przez kontroler grupy nie obejmuje niczego więcej niż kolekcja w pamięci.

## <a name="time-to-refactor"></a>Czas do refaktoryzacji

Trzeci krok w czerwono/zielony/refaktoryzacja jest częścią refaktoryzacji. W tym momencie musimy wrócić do naszego kodu i zastanowić się, w jaki sposób możemy remyśleć nasze aplikacje w celu usprawnienia jej projektowania. Etap refaktoryzacji jest etapem, na którym myślimy o najlepszym sposobie wdrażania zasad i wzorców projektowania oprogramowania.

Możemy zmodyfikować nasz kod w dowolny sposób, który zdecyduje się na ulepszenie projektu kodu. Mamy bezpieczeństwo testów jednostkowych, które uniemożliwiają nam nieprzerwane działanie istniejących funkcji.

Teraz nasz kontroler grupy to komunikat z perspektywą dobrego projektu oprogramowania. Kontroler grupy zawiera tangledy komunikat sprawdzania poprawności i kodu dostępu do danych. Aby uniknąć naruszenia jednej zasady odpowiedzialności, należy rozdzielić te zagadnienia na różne klasy.

Nasze Refaktoryzacja Klasa kontrolera grupy znajduje się na liście 9. Kontroler został zmodyfikowany w celu użycia warstwy usługi ContactManager. Jest to ta sama warstwa usługi, która jest używana z kontrolerem Contact.

Lista 10 zawiera nowe metody dodane do warstwy usługi ContactManager do obsługi sprawdzania poprawności, wyświetlania i tworzenia grup. Interfejs IContactManagerService został zaktualizowany w celu uwzględnienia nowych metod.

Lista 11 zawiera nową klasę FakeContactManagerRepository, która implementuje interfejs IContactManagerRepository. W przeciwieństwie do klasy EntityContactManagerRepository, która również implementuje interfejs IContactManagerRepository, nasza nowa klasa FakeContactManagerRepository nie komunikuje się z bazą danych. Klasa FakeContactManagerRepository używa kolekcji w pamięci jako serwera proxy dla bazy danych. Użyjemy tej klasy w naszych testach jednostkowych jako fałszywej warstwie repozytorium.

**Lista 9-Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Lista 10-Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Lista 11 — Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]

Modyfikowanie interfejsu IContactManagerRepository wymaga użycia, aby zaimplementować metody Create () i wyświetlanie grup () w klasie EntityContactManagerRepository. Laziest i najszybszy sposób to dodanie metod zastępczych, które wyglądają następująco:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]

Na koniec te zmiany w projekcie naszej aplikacji wymagają wprowadzenia pewnych zmian w naszych testach jednostkowych. Teraz podczas przeprowadzania testów jednostkowych musimy używać FakeContactManagerRepository. Zaktualizowana Klasa GroupControllerTest znajduje się na liście 12.

**Lista 12 — Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Po ponownym wprowadzeniu wszystkich tych zmian wszystkie testy jednostkowe zostaną zakończone pomyślnie. Cały cykl czerwony/zielony/Refaktoryzacja został ukończony. Wprowadziliśmy pierwsze dwa historie użytkownika. Firma Microsoft obsługuje teraz testy jednostkowe dla wymagań wyrażonych w wątkach użytkownika. Implementacja pozostałej części historii użytkownika obejmuje powtarzanie tego samego cyklu czerwony/zielony/refaktoryzacji.

## <a name="modifying-our-database"></a>Modyfikowanie naszej bazy danych

Niestety, mimo że wszystkie wymagania podane przez nasze testy jednostkowe zostały spełnione, nasze prace nie są wykonywane. Nadal musimy zmodyfikować naszą bazę danych.

Musimy utworzyć nową tabelę bazy danych grup. Wykonaj następujące kroki:

1. W oknie Eksplorator serwera kliknij prawym przyciskiem myszy folder tabele i wybierz opcję menu **Dodaj nową tabelę**.
2. Wprowadź dwie kolumny opisane poniżej w Projektancie tabel.
3. Oznacz kolumnę ID jako kolumnę klucza podstawowego i tożsamości.
4. Zapisz nową tabelę z nazwami grup, klikając ikonę dyskietki.

<a id="0.12_table01"></a>

| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Identyfikator | int | Fałsz |
| Name (Nazwa) | nvarchar (50) | Fałsz |

Następnie musimy usunąć wszystkie dane z tabeli kontaktów (w przeciwnym razie nie będzie można utworzyć relacji między tabelami kontakty i grupy). Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy tabelę kontakty i wybierz opcję menu **Pokaż dane tabeli**.
2. Usuń wszystkie wiersze.

Następnie musimy zdefiniować relację między tabelą bazy danych grup a istniejącą tabelą bazy danych kontaktów. Wykonaj następujące kroki:

1. Kliknij dwukrotnie tabelę kontakty w oknie Eksplorator serwera, aby otworzyć projektanta tabel.
2. Dodaj nową kolumnę Integer do tabeli Contacts o nazwie GroupId.
3. Kliknij przycisk relacja, aby otworzyć okno dialogowe relacje klucza obcego (patrz rysunek 3).
4. Kliknij przycisk Dodaj.
5. Kliknij przycisk wielokropka, który pojawia się obok przycisku Specyfikacja tabeli i kolumny.
6. W oknie dialogowym tabele i kolumny wybierz pozycję grupy jako tabelę klucza podstawowego i identyfikator jako kolumnę klucza podstawowego. Wybierz kontakty jako tabelę klucza obcego i identyfikator GroupId jako kolumnę klucza obcego (zobacz rysunek 4). Kliknij przycisk OK.
7. W obszarze **Specyfikacja INSERT i Update**wybierz wartość **Kaskada** dla **reguły usuwania**.
8. Kliknij przycisk Zamknij, aby zamknąć okno dialogowe relacje klucza obcego.
9. Kliknij przycisk Zapisz, aby zapisać zmiany w tabeli kontakty.

[![tworzenia relacji tabeli bazy danych](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Ilustracja 03**: Tworzenie relacji tabeli bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image6.png))

[![Określanie relacji między tabelami](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Ilustracja 04**: Określanie relacji tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image8.png))

### <a name="updating-our-data-model"></a>Aktualizowanie naszego modelu danych

Następnie musimy zaktualizować nasz model danych, aby reprezentować nową tabelę bazy danych. Wykonaj następujące kroki:

1. Kliknij dwukrotnie plik ContactManagerModel. edmx w folderze models, aby otworzyć Entity Designer.
2. Kliknij prawym przyciskiem myszy powierzchnię projektanta i wybierz opcję menu **Aktualizuj model z bazy danych**.
3. W Kreatorze aktualizacji wybierz tabelę grupy i kliknij przycisk Zakończ (patrz rysunek 5).
4. Kliknij prawym przyciskiem myszy jednostkę grupy i wybierz opcję menu **Zmień nazwę**. Zmień nazwę jednostki *grupy* na *grupę* (pojedyncza).
5. Kliknij prawym przyciskiem myszy właściwość nawigacji grupy, która pojawia się u dołu jednostki kontakt. Zmień nazwę właściwości nawigacji *grup* na *grupę* (pojedyncza).

[![aktualizowania modelu Entity Framework z bazy danych](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Ilustracja 05**: aktualizowanie modelu Entity Framework z bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image10.png))

Po wykonaniu tych kroków model danych będzie reprezentował tabele kontakty i grupy. W Entity Designer powinny być wyświetlane obie jednostki (zobacz rysunek 6).

[![Entity Designer wyświetlania grupy i kontaktu](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Ilustracja 06**. Entity Designer wyświetlania grupy i kontaktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image12.png))

### <a name="creating-our-repository-classes"></a>Tworzenie naszych klas repozytorium

Następnie musimy zaimplementować nasze klasy repozytorium. W trakcie tej iteracji dodaliśmy kilka nowych metod do interfejsu IContactManagerRepository podczas pisania kodu, aby spełnić nasze testy jednostkowe. Końcowa wersja interfejsu IContactManagerRepository jest zawarta w liście 14.

**Lista 14-Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

W rzeczywistości nie zaimplementowano żadnej z metod związanych z pracą z grupami kontaktów w naszej rzeczywistej klasie EntityContactManagerRepository. Obecnie Klasa EntityContactManagerRepository ma metody zastępcze dla każdej metody grupy kontaktów wymienionej w interfejsie IContactManagerRepository. Na przykład Metoda wyświetlanie grup () aktualnie wygląda następująco:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Metody zastępcze umożliwiały nam Kompilowanie naszej aplikacji i przekazywanie testów jednostkowych. Jednak teraz czas na faktyczne wdrożenie tych metod. Końcowa wersja klasy EntityContactManagerRepository jest zawarta w liście 13.

**Lista 13-Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Tworzenie widoków

ASP.NET aplikację MVC przy użyciu domyślnego aparatu widoku ASP.NET. Dlatego nie należy tworzyć widoków w odpowiedzi na konkretny test jednostkowy. Jednak ze względu na to, że aplikacja będzie bezużyteczny bez widoków, możemy zakończyć tę iterację bez tworzenia i modyfikowania widoków zawartych w aplikacji Contact Manager.

Musimy utworzyć następujące nowe widoki do zarządzania grupami kontaktów (patrz rysunek 7):

- Views\Group\Index.aspx — wyświetla listę grup kontaktów
- Views\Group\Delete.aspx — wyświetla formularz potwierdzenia usuwania grupy kontaktów

[![widoku indeksu grupy](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Ilustracja 07**: widok indeksu grupy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image14.png))

Musimy zmodyfikować następujące istniejące widoki, aby zawierały grupy kontaktów:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Zmodyfikowane widoki można wyświetlić, przeglądając aplikację Visual Studio, która obejmuje ten samouczek. Na przykład rysunek 8 ilustruje widok indeksu kontaktów.

[![widoku indeksu kontaktów](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Ilustracja 08**: widok indeksu kontaktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image16.png))

## <a name="summary"></a>Podsumowanie

W tej iteracji Dodaliśmy nową funkcję do naszej aplikacji do zarządzania kontaktami, postępując zgodnie z metodologią projektowania aplikacji programistycznych opartych na testach. Rozpoczęto od utworzenia zestawu scenariuszy użycia. Utworzyliśmy zestaw testów jednostkowych odpowiadających wymaganiom wyrażonym przez historie użytkownika. Wreszcie zapisałem wystarczy kod wystarczający do spełnienia wymagań wyrażonych przez testy jednostkowe.

Po zapisaniu wystarczającej ilości kodu w celu spełnienia wymagań wyrażonych przez testy jednostkowe Zaktualizowaliśmy bazę danych i widoki. Dodaliśmy nową tabelę grup do bazy danych i Zaktualizowano nasz model danych Entity Framework. Utworzono również i zmodyfikowano zestaw widoków.

W następnej iteracji — Ostatnia iteracja — ponownie napiszemy naszą aplikację, aby skorzystać z technologii AJAX. Dzięki wykorzystaniu technologii AJAX poprawimy czas odpowiedzi i wydajność aplikacji Contact Manager.

> [!div class="step-by-step"]
> [Poprzednie](iteration-5-create-unit-tests-vb.md)
> [dalej](iteration-7-add-ajax-functionality-vb.md)
