---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Wprowadzenie do stron sieci Web ASP.NET — wyświetlanie danych | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć bazę danych w programie WebMatrix oraz jak wyświetlać dane bazy danych na stronie w przypadku używania stron sieci Web ASP.NET (Razor). Zakłada, że y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 9e665ca8dd064c23a8b8bd3593014969d0c3da48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525222"
---
# <a name="introducing-aspnet-web-pages---displaying-data"></a>Wprowadzenie do stron sieci Web ASP.NET — wyświetlanie danych

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym samouczku pokazano, jak utworzyć bazę danych w programie WebMatrix oraz jak wyświetlać dane bazy danych na stronie w przypadku używania stron sieci Web ASP.NET (Razor). Przyjęto założenie, że seria została zakończona przez [wprowadzenie do programowania stron sieci Web ASP.NET](../introducing-razor-syntax-c.md).
> 
> Zawartość:
> 
> - Jak używać narzędzi WebMatrix do tworzenia bazy danych i tabel bazy danych.
> - Jak dodać dane do bazy danych przy użyciu narzędzi WebMatrix.
> - Jak wyświetlać dane z bazy danych na stronie.
> - Jak uruchamiać polecenia SQL na stronach sieci Web ASP.NET.
> - Jak dostosować pomocnika `WebGrid`, aby zmienić wyświetlanie danych i dodać stronicowanie i sortowanie.
>   
> 
> Omówione funkcje/technologie:
> 
> - Narzędzia WebMatrix Database.
> - pomocnik `WebGrid`.

## <a name="what-youll-build"></a>Co będziesz kompilować

W poprzednim samouczku wprowadzono do ASP.NET stron sieci Web (pliki *. cshtml* ), do podstaw składnia Razor i do pomocników. W tym samouczku rozpocznie się tworzenie rzeczywistej aplikacji sieci Web, która będzie używana w pozostałej części serii. Aplikacja to prosta aplikacja filmowa, która umożliwia wyświetlanie, Dodawanie, zmienianie i usuwanie informacji o filmach.

Gdy skończysz korzystać z tego samouczka, zobaczysz listę filmów, która wygląda podobnie do tej strony:

![Wyświetlacz WebGrid zawierający parametry ustawione na nazwy klas CSS](displaying-data/_static/image1.png)

Aby rozpocząć, musisz utworzyć bazę danych.

## <a name="a-very-brief-introduction-to-databases"></a>Krótkie wprowadzenie do baz danych

W tym samouczku przedstawiono tylko krótkie wprowadzenie do baz danych. W przypadku korzystania z bazy danych można pominąć tę krótką sekcję.

Baza danych zawiera co najmniej jedną tabelę zawierającą informacje &mdash; na przykład tabele dla klientów, zamówień i dostawców lub dla studentów, nauczycieli, klas i kategorii. Strukturalnie tabela bazy danych jest jak arkusz kalkulacyjny. Wyobraź sobie typową książkę adresową. Dla każdego wpisu w książce adresowej (czyli dla każdej osoby) masz kilka informacji, takich jak imię, nazwisko, adres, adres e-mail i numer telefonu.

![Przykładowa tabela bazy danych jako prosta siatka](displaying-data/_static/image2.png)

(Wiersze są czasami określane jako *rekordy*, a kolumny są czasami określane jako *pola*).

W przypadku większości tabel bazy danych tabela musi mieć kolumnę, która zawiera unikatową wartość, na przykład numer klienta, numer konta itd. Ta wartość jest określana jako *klucz podstawowy*tabeli i służy do identyfikowania każdego wiersza w tabeli. W przykładzie kolumna ID jest kluczem podstawowym dla książki adresowej pokazanej w poprzednim przykładzie.

Większość pracy wykonywanych w aplikacjach sieci Web polega na odczytywaniu informacji z bazy danych i wyświetlaniu jej na stronie. Często są również zbierane informacje od użytkowników i dodawane do bazy danych programu. można też modyfikować rekordy, które znajdują się już w bazie danych programu. (Wszystkie te operacje zostaną omówione w ramach tego zestawu samouczków).

Baza danych może być kompleksowo złożona i może wymagać specjalistycznej wiedzy. Na potrzeby tego samouczka należy zrozumieć tylko podstawowe koncepcje, które zostaną wyjaśnione po przejściu.

## <a name="creating-a-database"></a>Tworzenie bazy danych

Program WebMatrix zawiera narzędzia, które ułatwiają tworzenie bazy danych i tworzenie tabel w bazie danych programu. (Struktura bazy danych jest określana jako *schemat*bazy danych). Dla tego zestawu samouczków utworzysz bazę danych zawierającą tylko jedną tabelę w &mdash; filmach.

Otwórz program WebMatrix, jeśli jeszcze tego nie zrobiono, a następnie otwórz witrynę WebPagesMovies utworzoną w poprzednim samouczku.

W lewym okienku kliknij obszar roboczy **baza danych** .

![Karta obszaru roboczego bazy danych WebMatrix](displaying-data/_static/image3.png)

Wstążka zmienia się w celu wyświetlenia zadań związanych z bazą danych. Na wstążce kliknij pozycję **Nowa baza danych**.

![Przycisk "Nowa baza danych" na Wstążce WebMatrix](displaying-data/_static/image4.png)

WebMatrix tworzy bazę danych SQL Server CE (plik *. sdf* ) o takiej samej nazwie, co witryna &mdash; *WebPagesMovies. sdf*. (W tym miejscu nie można wykonać tej czynności, ale można zmienić nazwę pliku na dowolną zawartość, o ile ma rozszerzenie *. sdf* ).

## <a name="creating-a-table"></a>Tworzenie tabeli

Na wstążce kliknij pozycję **Nowa tabela**. WebMatrix otwiera projektanta tabel na nowej karcie. (Jeśli opcja Nowa tabela jest niedostępna, upewnij się, że nowa baza danych została wybrana w widoku drzewa po lewej stronie).

![Przycisk "Nowa tabela" na Wstążce WebMatrix](displaying-data/_static/image5.png)

W polu tekstowym u góry (w którym znajduje się znak wodny "Wprowadź nazwę tabeli"), wprowadź "filmy".

![Wprowadzanie nazwy tabeli w projektancie bazy danych WebMatrix](displaying-data/_static/image6.png)

W okienku pod nazwą tabeli można definiować poszczególne kolumny. W przypadku tabeli *filmów* w tym samouczku utworzysz tylko kilka kolumn: *Identyfikator*, *tytuł*, *gatunek*i *rok*.

W polu **Nazwa** wprowadź wartość "ID". Wprowadzenie wartości w tym miejscu aktywuje wszystkie kontrolki dla nowej kolumny.

Tab na listę **Typ danych** i wybierz **int**. Ta wartość określa, że kolumna identyfikatora będzie zawierać dane całkowite (liczbowe).

> [!NOTE]
> Nie wywołajemy go więcej (wiele), ale możesz użyć standardowych gestów klawiatury systemu Windows, aby przejść do tej siatki. Na przykład można nawiązać tabulację między polami, po prostu zacznij pisać, aby wybrać element na liście i tak dalej.

Tab poza polem **wartości domyślnej** (to oznacza, pozostaw puste). Na karcie **klucz podstawowy** pole wyboru i wybierz ją. Ta opcja instruuje bazę danych, że kolumna *Identyfikator* będzie zawierać dane, które identyfikują poszczególne wiersze. (Oznacza to, że każdy wiersz będzie miał unikatową wartość w kolumnie ID, która może być używana do znajdowania tego wiersza).

Wybierz opcję **tożsamość** . Ta opcja instruuje bazę danych, że powinna automatycznie generować następny numer sekwencyjny dla każdego nowego wiersza. (Opcja **jest tożsamość** działa tylko wtedy, gdy wybrano również opcję " **jest kluczem podstawowym** ").

Kliknij w następnym wierszu siatki lub dwukrotnie naciśnij klawisz Tab, aby zakończyć bieżący wiersz. Każdy gest zapisuje bieżący wiersz i uruchamia kolejny. Zwróć uwagę, że kolumna **wartość domyślna** ma teraz **wartość null**. (Wartość domyślna to wartość domyślna, dlatego należy mówić).

Po zakończeniu definiowania nowej kolumny **identyfikatora** Projektant będzie wyglądać podobnie do poniższej ilustracji:

![Projektant baz danych WebMatrix po zdefiniowaniu kolumny ID dla tabeli filmów](displaying-data/_static/image7.png)

Aby utworzyć następną kolumnę, kliknij w polu w kolumnie **Nazwa** . Wprowadź "title" dla kolumny, a następnie wybierz pozycję **nvarchar** dla wartości **Typ danych** . Część "var" elementu **nvarchar** instruuje bazę danych, że dane dla tej kolumny będą ciągiem, którego rozmiar może się różnić od rekordu do rekordu. (Prefiks "n" reprezentuje "National", co oznacza, że pole może zawierać dane znakowe dla każdego systemu alfabetu lub pisania — to znaczy, że pole przechowuje dane Unicode).

Po wybraniu opcji **nvarchar**, pojawi się inne pole, w którym można wprowadzić maksymalną długość pola. Wprowadź 50, w założeniu, że żaden tytuł filmu, z którym będziesz korzystać z tego samouczka, będzie dłuższy niż 50 znaków.

Pomiń **wartość domyślną** i usuń zaznaczenie opcji **Zezwalaj na wartości null** . Nie chcesz, aby baza danych zezwalała na wprowadzanie jakichkolwiek filmów do bazy danych, która nie ma tytułu.

Gdy skończysz i przejdziesz do następnego wiersza, Projektant wygląda jak na poniższej ilustracji:

![Projektant baz danych WebMatrix po zdefiniowaniu kolumny title dla tabeli filmów](displaying-data/_static/image8.png)

Powtórz te kroki, aby utworzyć kolumnę o nazwie "gatunek" z wyjątkiem długości, ustaw ją na wartość zaledwie 30.

Utwórz kolejną kolumnę o nazwie "Year". Dla typu danych wybierz opcję **nchar** (nie **nvarchar**) i Ustaw długość na 4. Na rok jest używana 4-cyfrowa liczba, taka jak "1995" lub "2010", więc nie jest wymagana kolumna o zmiennym rozmiarze.

Oto, jak wygląda gotowy projekt:

![Projektant baz danych WebMatrix po zdefiniowaniu wszystkich pól dla tabeli filmów](displaying-data/_static/image9.png)

Naciśnij klawisze CTRL + S lub kliknij przycisk **Zapisz** na pasku narzędzi Szybki dostęp. Zamknij projektanta baz danych, zamykając kartę.

## <a name="adding-some-example-data"></a>Dodawanie przykładowych danych

W dalszej części tej serii samouczków utworzysz stronę umożliwiającą wprowadzanie nowych filmów w formularzu. Na razie można jednak dodać przykładowe dane, które można następnie wyświetlić na stronie.

W obszarze roboczym **baza danych** w programie WebMatrix Zwróć uwagę, że istnieje drzewo pokazujące utworzony wcześniej plik *. sdf* . Otwórz węzeł dla nowego pliku *. sdf* , a następnie otwórz węzeł **tabele** .

![Obszar roboczy bazy danych WebMatrix z otwartym drzewem w tabeli filmów](displaying-data/_static/image10.png)

Kliknij prawym przyciskiem myszy węzeł **filmy** , a następnie wybierz polecenie **dane**. WebMatrix otwiera siatkę, w której można wprowadzać dane dla tabeli *filmów* :

![Siatka wejścia bazy danych w programie WebMatrix (pusta)](displaying-data/_static/image11.png)

Kliknij kolumnę **tytuł** i wprowadź wartość "Kiedy Harry MET Sally". Przejdź do kolumny **gatunek** (możesz użyć klawisza Tab) i wprowadź ciąg "Romantic komedia". Przejdź do kolumny **Year** i wprowadź wartość "1989":

![Siatka wejścia bazy danych w programie WebMatrix z jednym rekordem](displaying-data/_static/image12.png)

Naciśnij klawisz ENTER, a Program WebMatrix zapisuje nowy film. Zwróć uwagę, że kolumna **ID** została wypełniona.

![Siatka wejścia bazy danych w programie WebMatrix z jednym rekordem i automatycznie generowanym IDENTYFIKATORem](displaying-data/_static/image13.png)

Wprowadź inny film (na przykład "znikł z wiatrem", "dramat", "1939"). Kolumna Identyfikator jest wypełniana ponownie:

![Siatka wejścia bazy danych w programie WebMatrix z dwoma rekordami i automatycznie generowanymi identyfikatorami](displaying-data/_static/image14.png)

Wprowadź trzeci film (na przykład "Ghostbusters", "komedia"). W ramach eksperymentu pozostaw pustą kolumnę **Year** , a następnie naciśnij klawisz ENTER. Ponieważ wyznaczono opcję **Zezwalaj na wartości null** , w bazie danych zostanie wyświetlony komunikat o błędzie:

![Wyświetlany jest błąd "nieprawidłowe dane", jeśli wymagana wartość kolumny pozostaje pusta](displaying-data/_static/image15.png)

Kliknij przycisk **OK** , aby wrócić i naprawić wpis (rok dla "Ghostbusters" to 1984), a następnie naciśnij klawisz ENTER.

Wypełnij kilka filmów do momentu, gdy nie masz 8 lub tak. (Wprowadzenie 8 ułatwia pracę z stronicowaniem później. Ale jeśli jest to zbyt wiele, wprowadź tylko kilka. Rzeczywiste dane nie są istotne.

![Siatka wejścia bazy danych w programie WebMatrix z dowolnego rekordu](displaying-data/_static/image16.png)

Jeśli wprowadzono wszystkie filmy bez błędów, wartości identyfikatorów są sekwencyjne. Jeśli podjęto próbę zapisania niekompletnego rekordu filmu, numery IDENTYFIKACYJNe mogą nie być sekwencyjne. Jeśli tak, to jest dobry. Liczby nie mają własnych znaczenia i jedyną istotną kwestią jest to, że są one unikatowe w tabeli *filmów* .

Zamknij kartę, która zawiera projektanta baz danych.

Teraz można włączyć wyświetlanie tych danych na stronie sieci Web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Wyświetlanie danych na stronie przy użyciu pomocnika WebGrid

Aby wyświetlić dane na stronie, użyj pomocnika `WebGrid`. Ten pomocnik tworzy ekran w siatce lub tabeli (wiersze i kolumny). Jak widać, będziesz mieć możliwość dostosowania siatki z formatowaniem i innymi funkcjami.

Aby uruchomić siatkę, należy napisać kilka wierszy kodu. Te kilka wierszy będzie działać jako rodzaj wzorca dla niemal wszystkich danych, które można wykonać w tym samouczku.

> [!NOTE]
> Istnieje wiele opcji wyświetlania danych na stronie; pomocnik `WebGrid` jest tylko jeden. Wybieramy go dla tego samouczka, ponieważ jest to najprostszy sposób na wyświetlanie danych i ponieważ jest on rozsądnie elastyczny. W następnym zestawie samouczków zobaczysz, jak używać bardziej "ręcznego" sposobu pracy z danymi na stronie, co zapewnia większą kontrolę nad sposobem wyświetlania danych.

W lewym okienku w programie WebMatrix kliknij obszar roboczy **pliki** .

Utworzona nowa baza danych znajduje się w folderze *danych\_aplikacji* . Jeśli folder nie istnieje, WebMatrix utworzył go dla nowej bazy danych. (Folder mógł istnieć, jeśli wcześniej zainstalowano pomocników).

W widoku drzewa wybierz katalog główny witryny sieci Web. Musisz wybrać katalog główny witryny sieci Web; w przeciwnym razie nowy plik może zostać dodany do folderu danych\_aplikacji.

Na wstążce kliknij pozycję **Nowy**. W polu **Wybierz typ pliku** wybierz pozycję **cshtml**.

W polu **Nazwa** wpisz nową stronę "filmy. cshtml":

![Okno dialogowe "Wybieranie typu pliku" pokazujące stronę "filmy"](displaying-data/_static/image17.png)

Kliknij przycisk **OK**. W programie WebMatrix zostanie otwarty nowy plik z niektórymi elementami szkieletu. Najpierw napiszesz kod, aby pobrać dane z bazy danych. Następnie dodasz znaczniki do strony, aby faktycznie wyświetlały dane.

### <a name="writing-the-data-query-code"></a>Pisanie kodu zapytania o dane

W górnej części strony między znakami `@{` i `}` wprowadź następujący kod. (Upewnij się, że wprowadzasz ten kod między otwierającym i zamykającym nawiasem klamrowym).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

Pierwszy wiersz otwiera utworzoną wcześniej bazę danych, która jest zawsze pierwszym krokiem przed wykonaniem operacji w bazie danych. Poznasz nazwę metody `Database.Open` bazy danych do otwarcia. Zwróć uwagę, że w nazwie nie jest uwzględniony *. sdf.* Metoda `Open` zakłada, że szuka pliku *. sdf* (czyli *WebPagesMovies. sdf*) i że plik *. sdf* znajduje się w folderze *\_danych aplikacji* . (Wcześniej zauważono, że folder *danych\_aplikacji* jest zarezerwowany. ten scenariusz jest jednym z miejsc, w których ASP.NET tworzy założeń o tej nazwie.)

Gdy baza danych zostanie otwarta, odwołanie do niej jest umieszczane w zmiennej o nazwie `db`. (Może to być nazwane wszystko). Zmienna `db` to sposób, w jaki będziesz kończyć pracę z bazą danych.

Drugi wiersz w rzeczywistości pobiera dane bazy danych za pomocą metody `Query`. Zwróć uwagę, jak działa ten kod: zmienna `db` ma odwołanie do otwartej bazy danych i wywołujemy metodę `Query` przy użyciu zmiennej `db` (`db.Query`).

Sama kwerenda jest instrukcją SQL `Select`. (Aby uzyskać informacje na temat programu SQL Server w niewielkim stopniu, zobacz wyjaśnienie później). W instrukcji `Movies` identyfikuje tabelę do zapytania. Znak `*` określa, że zapytanie powinno zwrócić wszystkie kolumny z tabeli. (Można również wystawić pojedyncze kolumny, oddzielone przecinkami).

Wyniki zapytania, jeśli istnieją, są zwracane i udostępniane w zmiennej `selectedData`. Ponownie zmienna może mieć nazwę wszystko.

Na koniec trzeci wiersz informuje ASP.NET o konieczności użycia wystąpienia pomocnika `WebGrid`. Można utworzyć (utworzyć*wystąpienie*) obiekt pomocnika za pomocą słowa kluczowego `new` i przekazać go do wyników zapytania za pomocą zmiennej `selectedData`. Nowy obiekt `WebGrid`, wraz z wynikami zapytania bazy danych, jest udostępniany w zmiennej `grid`. Konieczne będzie chwilowe wyświetlenie danych na stronie.

Na tym etapie baza danych została otwarta i masz wybrane dane i przygotowano pomocnika `WebGrid` z tymi danymi. Następnie należy utworzyć adiustację na stronie.

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL to język, który jest używany w większości relacyjnych baz danych do zarządzania danymi w bazie danych. Zawiera polecenia, które umożliwiają pobieranie danych i aktualizowanie ich oraz pozwalające tworzyć, modyfikować i zarządzać danymi w tabelach bazy danych. Język SQL różni się od języka programowania (na C#przykład). Dzięki programowi SQL możesz poinformowanie bazy danych o tym, co chcesz zrobić, i jest to zadanie bazy danych, aby ustalić, jak pobrać dane lub wykonać zadanie. Oto przykłady niektórych poleceń SQL i ich działania:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Pierwsza instrukcja `Select` pobiera wszystkie kolumny (określone przez `*`) z tabeli *filmów* .
> 
> Druga instrukcja `Select` Pobiera kolumny ID, Name i Price z rekordów w tabeli *Product* , których wartość kolumny cena jest większa niż 10. Polecenie zwraca wyniki w kolejności alfabetycznej na podstawie wartości kolumny Name. Jeśli żadne rekordy nie pasują do kryteriów cen, polecenie zwraca pusty zestaw.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> To polecenie wstawia nowy rekord do tabeli *Product* , ustawiając dla kolumny Name wartość "croissant", kolumnę Description na wartość "płatne" i cenę do 1,99.
> 
> Należy zauważyć, że podczas określania wartości innej niż numeryczna wartość jest ujęta w znaki pojedynczego cudzysłowu (nie podwójne cudzysłowy, jak w C#przypadku). Te znaki cudzysłowu są używane wokół wartości tekstowych lub dat, ale nie liczby.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> To polecenie usuwa rekordy z tabeli *Product* , której kolumna daty wygaśnięcia jest wcześniejsza niż 1 stycznia 2008. (Polecenie zakłada, że tabela *produktów* ma taką kolumnę, oczywiście). Data jest wprowadzana w formacie MM/DD/RRRR, ale należy ją wprowadzić w formacie używanym dla ustawień regionalnych.
> 
> Polecenia `Insert` i `Delete` nie zwracają zestawów wyników. Zamiast tego zwraca liczbę, która informuje o liczbie rekordów, na które miało wpływ polecenie.
> 
> W przypadku niektórych z tych operacji (takich jak wstawianie i usuwanie rekordów) proces żądający operacji musi mieć odpowiednie uprawnienia w bazie danych. Dlatego w przypadku produkcyjnych baz danych często konieczne jest podanie nazwy użytkownika i hasła podczas łączenia się z bazą danych.
> 
> Istnieją dziesiątki poleceń SQL, ale wszystkie są zgodne ze wzorcem, takim jak polecenia widoczne w tym miejscu. Za pomocą poleceń SQL można tworzyć tabele baz danych, liczyć liczbę rekordów w tabeli, obliczać ceny i wykonywać wiele innych operacji.

### <a name="adding-markup-to-display-the-data"></a>Dodawanie znaczników w celu wyświetlenia danych

Wewnątrz elementu `<head>` Ustaw zawartość elementu `<title>` na "filmy":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Wewnątrz elementu `<body>` strony Dodaj następujące elementy:

[!code-html[Main](displaying-data/samples/sample3.html)]

Gotowe. Zmienna `grid` jest wartością utworzoną podczas tworzenia obiektu `WebGrid` w kodzie wcześniejszym.

W widoku drzewa WebMatrix kliknij prawym przyciskiem myszy stronę i wybierz polecenie **Uruchom w przeglądarce**. Zobaczysz coś podobnej do tej strony:

![Domyślne dane wyjściowe pomocnika WebGrid z tabeli filmów](displaying-data/_static/image18.png)

Kliknij link nagłówka kolumny, aby posortować według tej kolumny. Można sortować według kliknięcia nagłówka jest funkcją wbudowaną pomocnika **WebGrid** .

Metoda `GetHtml`, która sugeruje jego nazwę, generuje znacznik, który wyświetla dane. Domyślnie metoda `GetHtml` generuje element HTML `<table>`. (Jeśli chcesz, możesz sprawdzić renderowanie, przeglądając Źródło strony w przeglądarce).

## <a name="modifying-the-look-of-the-grid"></a>Modyfikowanie wyglądu siatki

Korzystanie z pomocnika `WebGrid`, podobnie jak wiesz, jest proste, ale wyświetlane wyniki są zwykłe. Pomocnik `WebGrid` ma wszystkie opcje, które umożliwiają kontrolowanie sposobu wyświetlania danych. W tym samouczku jest zbyt wiele, aby eksplorować ten samouczek, ale ta sekcja zawiera pomysł dotyczący niektórych z tych opcji. Kilka dodatkowych opcji zostanie uwzględnionych w kolejnych samouczkach w tej serii.

### <a name="specifying-individual-columns-to-display"></a>Określanie pojedynczych kolumn do wyświetlenia

Aby rozpocząć, można określić, że mają być wyświetlane tylko niektóre kolumny. Domyślnie, jak widać, Siatka pokazuje wszystkie cztery kolumny z tabeli *filmów* .

W pliku *Films. cshtml* Zastąp znacznik `@grid.GetHtml()`, który właśnie został dodany:

[!code-css[Main](displaying-data/samples/sample4.css)]

Aby poinformować pomocnika, które kolumny mają być wyświetlane, należy podać parametr `columns` dla metody `GetHtml` i przekazać kolekcję kolumn. W kolekcji należy określić każdą kolumnę do uwzględnienia. Należy określić pojedynczą kolumnę, która ma być wyświetlana przez uwzględnienie obiektu `grid.Column` i przekazać nazwę wybranej kolumny danych. (Te kolumny muszą być zawarte w wynikach zapytania SQL — pomocnik `WebGrid` nie może wyświetlić kolumn, które nie zostały zwrócone przez zapytanie.)

Uruchom ponownie stronę *filmy. cshtml* w przeglądarce, a tym razem wyświetli ekran podobny do następującego (Zauważ, że nie jest wyświetlana żadna kolumna identyfikatora):

![Wyświetlanie siatki w sieci WebGrid pokazujące tylko wybrane kolumny](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Zmiana wyglądu siatki

Istnieje całkiem kilka dodatkowych opcji wyświetlania kolumn, a niektóre z nich zostaną omówione w kolejnych samouczkach w tym zestawie. W tej sekcji opisano sposób, w jaki można stylować siatkę jako całość.

Wewnątrz sekcji `<head>` strony tuż przed tagiem zamykającym `</head>` Dodaj następujący element `<style>`:

[!code-css[Main](displaying-data/samples/sample5.css)]

Ten znacznik CSS definiuje klasy o nazwie `grid`, `head`i tak dalej. Można również umieścić te definicje stylów w osobnym pliku *CSS* i połączyć go ze stroną. (W rzeczywistości wykonasz tę czynność w dalszej części tego samouczka). Jednak aby ułatwić Ci ten samouczek, znajdują się one na tej samej stronie, na której są wyświetlane dane.

Teraz możesz uzyskać pomocnika `WebGrid`, aby używać tych klas stylów. Pomocnik ma wiele właściwości (na przykład `tableStyle`) tylko w tym celu — przypisujesz do nich nazwę klasy stylu CSS, a nazwa klasy jest renderowana jako część znaczników, które są renderowane przez pomocnika.

Zmień znacznik `grid.GetHtml` tak, aby wyglądał teraz tak, jak w tym kodzie:

[!code-css[Main](displaying-data/samples/sample6.css)]

Nowością jest dodanie `tableStyle`, `headerStyle`i `alternatingRowStyle` parametrów do metody `GetHtml`. Te parametry zostały ustawione na nazwy stylów CSS, które zostały dodane do momentu temu.

Uruchom stronę, a w tym momencie zobaczysz siatkę, która wygląda mniej więcej tak, jak wcześniej:

![Wyświetlacz WebGrid zawierający parametry ustawione na nazwy klas CSS](displaying-data/_static/image20.png)

Aby zobaczyć, jak Wygenerowano metodę `GetHtml`, możesz przyjrzeć się źródłu strony w przeglądarce. Nie będziemy tutaj podawać szczegółów, ale ważnym punktem jest to, że przez określenie parametrów takich jak `tableStyle`, Siatka generuje Tagi HTML w następujący sposób:

`<table class="grid">`

Tag `<table>` ma dodany atrybut `class`, który odwołuje się do jednej z wcześniej dodanych reguł CSS. Ten kod przedstawia podstawowy wzorzec &mdash; różne parametry metody `GetHtml` umożliwiają odwoływanie się do klas CSS, które następnie generują metody wraz ze znacznikiem. Co robisz z klasami CSS.

## <a name="adding-paging"></a>Dodawanie stronicowania

Jako ostatnie zadanie dla tego samouczka dodasz stronicowanie do siatki. Teraz nie ma problemu, aby wyświetlić wszystkie filmy jednocześnie. Ale jeśli dodano setki filmów, wyświetlenie strony będzie długie.

W kodzie strony Zmień wiersz, który tworzy obiekt `WebGrid`, na następujący kod:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Jedyną różnicą od przed jest dodanie parametru `rowsPerPage`, który jest ustawiony na 3.

Uruchom stronę. Siatka wyświetla 3 wiersze jednocześnie oraz linki nawigacyjne, które umożliwiają przechodzenie do stron w bazie danych:

![Wyświetlanie siatki WebGrid z stronicowaniem](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Przyszłe przejście

W następnym samouczku dowiesz się, jak używać składni Razor i C# Code do uzyskiwania danych wejściowych użytkownika w formularzu. Dodasz pole wyszukiwania do strony filmy, aby można było znaleźć filmy według tytułu lub gatunku.

## <a name="complete-listing-for-movies-page"></a>Pełna lista dla filmów

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Dodatkowe materiały

- [Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Poprzednie](intro-to-web-pages-programming.md)
> [dalej](form-basics.md)
