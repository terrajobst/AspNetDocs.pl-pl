---
uid: web-pages/overview/data/5-working-with-data
title: Wprowadzenie do pracy z bazą danych w witrynach ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale opisano, jak uzyskać dostęp do danych z bazy danych i wyświetlać je za pomocą stron sieci Web ASP.NET.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 45e988d037465e59ad352bb9444af2c69fd3cd70
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586535"
---
# <a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Wprowadzenie do pracy z bazą danych w witrynach ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano, jak używać narzędzi Microsoft WebMatrix do tworzenia bazy danych w witrynie sieci Web ASP.NET (Razor) i tworzenia stron, które umożliwiają wyświetlanie, Dodawanie, edytowanie i usuwanie danych.
> 
> **Dowiesz się:** 
> 
> - Jak utworzyć bazę danych.
> - Jak nawiązać połączenie z bazą danych.
> - Jak wyświetlać dane na stronie sieci Web.
> - Jak wstawiać, aktualizować i usuwać rekordy bazy danych.
> 
> Oto funkcje wprowadzone w artykule:
> 
> - Praca z bazą danych Microsoft SQL Server Compact Edition.
> - Praca z kwerendami SQL.
> - Klasa `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - WebMatrix 2
>   
> 
> Ten samouczek współpracuje również z programem WebMatrix 3. Możesz użyć ASP.NET Web Pages 3 i Visual Studio 2013 (lub Visual Studio Express 2013 dla sieci Web); jednak interfejs użytkownika będzie inny.

## <a name="introduction-to-databases"></a>Wprowadzenie do baz danych

Wyobraź sobie typową książkę adresową. Dla każdego wpisu w książce adresowej (czyli dla każdej osoby) masz kilka informacji, takich jak imię, nazwisko, adres, adres e-mail i numer telefonu.

Typowym sposobem na obrazowanie danych takich jak tabela z wierszami i kolumnami. W terminologii bazy danych każdy wiersz jest często określany jako rekord. Każda kolumna (czasami określana jako pola) zawiera wartość dla każdego typu danych: imię, nazwisko i tak dalej.

| **Identyfikator** | **Imię** | **Nazwisko** | **Ulica** | **Wiadomość e-mail** | **Połączenia** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 ewentualna szczytowa St 98031 SE | jim@contoso.com | 555 0100 |
| 2 | Planowa | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

W przypadku większości tabel bazy danych tabela musi mieć kolumnę, która zawiera unikatowy identyfikator, taki jak numer klienta, numer konta itd. Jest to nazywane *kluczem podstawowym*tabeli i służy do identyfikowania każdego wiersza w tabeli. W przykładzie kolumna ID jest kluczem podstawowym dla książki adresowej.

Dzięki tej podstawowej zrozumieniu baz danych można dowiedzieć się, jak utworzyć prostą bazę danych i wykonać operacje, takie jak dodawanie, modyfikowanie i usuwanie danych.

> [!TIP] 
> 
> **Relacyjne bazy danych**
> 
> Dane można przechowywać na wiele sposobów, w tym pliki tekstowe i arkusze kalkulacyjne. W przypadku większości używanych w firmie dane są przechowywane w relacyjnej bazie danych.
> 
> Ten artykuł nie działa bardzo głęboko w bazach danych. Przydatne może być jednak zrozumienie ich nieco. W relacyjnej bazie danych informacje są logicznie podzielone na oddzielne tabele. Na przykład baza danych dla szkoły może zawierać oddzielne tabele dla studentów i ofert klas. Oprogramowanie bazy danych (takie jak SQL Server) obsługuje zaawansowane polecenia, które umożliwiają dynamiczne Ustanawianie relacji między tabelami. Na przykład można użyć relacyjnej bazy danych do ustanowienia relacji logicznej między uczniami i klasami w celu utworzenia harmonogramu. Przechowywanie danych w oddzielnych tabelach zmniejsza złożoność struktury tabeli i zmniejsza potrzebę przechowywania nadmiarowych danych w tabelach.

## <a name="creating-a-database"></a>Tworzenie bazy danych

Ta procedura pokazuje, jak utworzyć bazę danych o nazwie SmallBakery przy użyciu narzędzia do projektowania bazy danych SQL Server Compact, które jest zawarte w programie WebMatrix. Chociaż można utworzyć bazę danych przy użyciu kodu, jest to bardziej typowe dla tworzenia baz danych i tabel bazy danych przy użyciu narzędzia do projektowania, takiego jak WebMatrix.

1. Uruchom program WebMatrix i na stronie Szybki start kliknij pozycję **Witryna z szablonu**.
2. Wybierz pozycję **pusta witryna**, a następnie w polu **Nazwa lokacji** wprowadź wartość "SmallBakery", a następnie kliknij przycisk **OK**. Witryna zostanie utworzona i wyświetlona w programie WebMatrix.
3. W lewym okienku kliknij obszar roboczy **bazy danych** .
4. Na wstążce kliknij pozycję **Nowa baza danych**. Zostanie utworzona pusta baza danych o tej samej nazwie co witryna.
5. W lewym okienku rozwiń węzeł **SmallBakery. sdf** , a następnie kliknij pozycję **tabele**.
6. Na wstążce kliknij pozycję **Nowa tabela**. WebMatrix otwiera projektanta tabel.

    ![Image](5-working-with-data/_static/image1.jpg)
7. Kliknij w kolumnie **Nazwa** , a następnie wprowadź &quot;identyfikator&quot;.
8. W kolumnie **Typ danych** wybierz pozycję **int**.
9. Ustawić **klucz podstawowy?** i **ma** wartość **tak**.

    Jak sugeruje nazwa, **klucz podstawowy** informuje bazę danych, że będzie ona kluczem podstawowym tabeli. **Czy tożsamość** instruuje bazę danych w celu automatycznego utworzenia numeru identyfikacyjnego dla każdego nowego rekordu i przypisania go do następnego sekwencyjnego numeru (rozpoczynając od 1).
10. Kliknij w następnym wierszu. Edytor uruchamia nową definicję kolumny.
11. W polu Nazwa wprowadź wartość &quot;nazwa&quot;.
12. W **polu Typ danych**wybierz &quot;nvarchar&quot; i Ustaw długość na 50. *WARIANCJA* części `nvarchar` informuje bazę danych, że dane dla tej kolumny będą ciągiem, którego rozmiar może się różnić od rekordu do rekordu. ( *N* prefiks reprezentuje *kraj*, wskazujący, że pole może zawierać dane znakowe, które reprezentują każdy system &#8212; alfabetu lub pisania, czyli pole przechowuje dane Unicode).
13. Dla opcji **Zezwalaj na wartości null ustaw wartość** **nie**. Spowoduje to wymuszenie, że kolumna *Nazwa* nie zostanie pozostawiona puste.
14. Przy użyciu tego samego procesu Utwórz kolumnę o nazwie *Description*. Ustaw **Typ danych** na "nvarchar" i 50 jako długość i ustaw **wartość null** na false.
15. Utwórz kolumnę o nazwie *Price*. Ustaw **Typ danych na "Money"** i ustaw wartość **null** na wartość false.
16. W polu u góry nazwij tabelę &quot;&quot;produktu.

    Gdy skończysz, definicja będzie wyglądać następująco:

    ![Image](5-working-with-data/_static/image2.png)
17. Naciśnij kombinację klawiszy Ctrl + S, aby zapisać tabelę.

## <a name="adding-data-to-the-database"></a>Dodawanie danych do bazy danych

Teraz można dodać do bazy danych przykładowe dane, które będą działały w dalszej części artykułu.

1. W lewym okienku rozwiń węzeł **SmallBakery. sdf** , a następnie kliknij pozycję **tabele**.
2. Kliknij prawym przyciskiem myszy tabelę Product, a następnie kliknij pozycję **dane**.
3. W okienku Edycja wprowadź następujące rekordy:

    | **Nazwa** | **Opis** | **Koszt** |
    | --- | --- | --- |
    | Chleb | Rozszerzania każdego dnia. | 2.99 |
    | Shortcake truskawki | Realizowane z użyciem truskawek organicznych z naszego ogrodu. | 9.99 |
    | Kołowy firmy Apple | Sekunda tylko dla wykresu kołowego MOM. | 12.99 |
    | Kołowy pecan | Jeśli lubisz Pecans, jest to możliwe. | 10.99 |
    | Kołowy cytrynowy | Z najlepszymi cytrynami na świecie. | 11.99 |
    | Babeczki | Twoje dzieci i dzieci będą się odkochać. | 7.99 |

    Należy pamiętać, że nie trzeba wprowadzać niczego dla kolumny *ID* . Po utworzeniu kolumny *ID* należy ustawić jej właściwość **Identity** na true, co spowoduje, że zostanie ona automatycznie wypełniona.

    Po zakończeniu wprowadzania danych Projektant tabel będzie wyglądać następująco:

    ![Image](5-working-with-data/_static/image3.jpg)
4. Zamknij kartę, która zawiera dane bazy danych.

## <a name="displaying-data-from-a-database"></a>Wyświetlanie danych z bazy danych

Po uzyskaniu bazy danych zawierającej dane można wyświetlić dane na stronie sieci Web ASP.NET. Aby wybrać wiersze tabeli do wyświetlenia, należy użyć instrukcji SQL, która jest poleceniem przekazywanym do bazy danych.

1. W lewym okienku kliknij obszar roboczy **pliki** .
2. W folderze głównym witryny sieci Web Utwórz nową stronę CSHTML o nazwie *ListProducts. cshtml*.
3. Zastąp istniejący znacznik następującym:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    W pierwszym bloku kodu otworzysz utworzony wcześniej plik *SmallBakery. sdf* (baza danych). Metoda `Database.Open` zakłada, że plik *. sdf* znajduje się w aplikacji witryny sieci Web *\_folderze danych* . (Należy zauważyć, że nie musisz określać rozszerzenia &#8212; *. sdf* w rzeczywistości, jeśli to zrobisz, Metoda `Open` nie będzie działała).

    > [!NOTE]
    > Folder *danych\_aplikacji* jest specjalnym folderem w ASP.NET, który jest używany do przechowywania plików danych. Aby uzyskać więcej informacji, zobacz [łączenie się z bazą danych](#SB_ConnectingToADatabase) w dalszej części tego artykułu.

    Następnie utworzysz żądanie przetworzenia zapytania do bazy danych przy użyciu następującej instrukcji SQL `Select`:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    W instrukcji `Product` identyfikuje tabelę do zapytania. Znak `*` określa, że zapytanie powinno zwrócić wszystkie kolumny z tabeli. (Można również wyświetlać pojedyncze kolumny, oddzielone przecinkami, jeśli chcesz zobaczyć tylko niektóre kolumny). Klauzula `Order By` wskazuje, jak w tym przypadku dane powinny &#8212; być sortowane według *nazwy* kolumny. Oznacza to, że dane są sortowane alfabetycznie na podstawie wartości kolumny *Nazwa* dla każdego wiersza.

    W treści strony, znacznik tworzy tabelę HTML, która będzie używana do wyświetlania danych. Wewnątrz elementu `<tbody>` należy użyć pętli `foreach`, aby osobno pobrać każdy wiersz danych zwrócony przez zapytanie. Dla każdego wiersza danych utworzysz wiersz tabeli HTML (`<tr>` element). Następnie utworzysz komórki tabeli HTML (elementy`<td>`) dla każdej kolumny. Za każdym razem, gdy przejdziesz przez pętlę, następnym dostępnym wierszem z bazy danych jest zmienna `row` (należy ją skonfigurować w instrukcji `foreach`). Aby uzyskać pojedynczą kolumnę z wiersza, można użyć `row.Name` lub `row.Description` lub niezależnie od tego, czy nazwa jest pożądaną kolumną.
4. Uruchom stronę w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Na stronie zostanie wyświetlona lista, taka jak następująca:

    ![Image](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL to język, który jest używany w większości relacyjnych baz danych do zarządzania danymi w bazie danych. Zawiera polecenia, które umożliwiają pobieranie danych i aktualizowanie ich oraz pozwalające tworzyć, modyfikować i zarządzać tabelami baz danych. Baza danych SQL różni się od języka programowania (takiego jak ten, który jest używany w programie WebMatrix) ze względu na to, że w programie SQL Server jest to pożądane, co jest potrzebne, a to zadanie bazy danych pozwala ustalić, jak pobrać dane lub wykonać zadanie. Oto przykłady niektórych poleceń SQL i ich działania:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Spowoduje to pobranie kolumn *identyfikatorów*, *nazw*i *cen* z rekordów w tabeli *Product* , jeśli wartość *ceny* jest większa niż 10 i zwraca wyniki w kolejności alfabetycznej na podstawie wartości kolumny *name* . To polecenie zwróci zestaw wyników, który zawiera rekordy spełniające kryteria, lub pusty zestaw, jeśli żadne rekordy nie są zgodne.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Spowoduje to wstawienie nowego rekordu do tabeli *Product* , ustawienie kolumny *name* na &quot;croissant&quot;, kolumnie *Opis* , aby &quot;&quot;płatne, a cena do 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> To polecenie usuwa rekordy z tabeli *Product* , której kolumna daty wygaśnięcia jest wcześniejsza niż 1 stycznia 2008. (Przyjęto założenie, że tabela *produktów* ma taką kolumnę, oczywiście). Data jest wprowadzana w formacie MM/DD/RRRR, ale należy ją wprowadzić w formacie używanym dla ustawień regionalnych.
> 
> Polecenia `Insert Into` i `Delete` nie zwracają zestawów wyników. Zamiast tego zwraca liczbę, która informuje o liczbie rekordów, na które miało wpływ polecenie.
> 
> W przypadku niektórych z tych operacji (takich jak wstawianie i usuwanie rekordów) proces żądający operacji musi mieć odpowiednie uprawnienia w bazie danych. Dlatego w przypadku produkcyjnych baz danych często konieczne jest podanie nazwy użytkownika i hasła podczas łączenia się z bazą danych.
> 
> Istnieją dziesiątki poleceń SQL, ale wszystkie są zgodne z wzorcem podobnym do tego. Za pomocą poleceń SQL można tworzyć tabele baz danych, liczyć liczbę rekordów w tabeli, obliczać ceny i wykonywać wiele innych operacji.

## <a name="inserting-data-in-a-database"></a>Wstawianie danych do bazy danych

W tej sekcji przedstawiono sposób tworzenia strony, która umożliwia użytkownikom dodawanie nowego produktu do tabeli bazy danych *produktów* . Po wstawieniu nowego rekordu produktu na stronie zostanie wyświetlona zaktualizowana tabela przy użyciu strony *ListProducts. cshtml* utworzonej w poprzedniej sekcji.

Strona zawiera sprawdzanie poprawności, aby upewnić się, że dane wprowadzane przez użytkownika są prawidłowe dla bazy danych. Na przykład kod na stronie gwarantuje, że wprowadzono wartość dla wszystkich wymaganych kolumn.

1. W witrynie sieci Web Utwórz nowy plik CSHTML o nazwie *InsertProducts. cshtml*.
2. Zastąp istniejący znacznik następującym:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Treść strony zawiera formularz HTML z trzema polami tekstowymi, które umożliwiają użytkownikom wprowadzanie nazwy, opisu i ceny. Gdy użytkownicy kliknieją przycisk **Wstaw** , kod w górnej części strony otwiera połączenie z bazą danych *SmallBakery. sdf* . Następnie uzyskasz wartości przesłane przez użytkownika przy użyciu obiektu `Request` i przypisanie tych wartości do zmiennych lokalnych.

    Aby sprawdzić, czy użytkownik wprowadził wartość dla każdej wymaganej kolumny, należy zarejestrować każdy element `<input>`, który ma zostać sprawdzony:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Pomocnik `Validation` sprawdza, czy istnieje wartość w każdym zarejestrowanym polu. Możesz sprawdzić, czy wszystkie pola przebiegły walidację, sprawdzając `Validation.IsValid()`, co zazwyczaj jest wykonywane przed przetworzeniem informacji uzyskanych od użytkownika:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (Operator `&&` oznacza i — ten test polega na tym, *że jest to przesłanie formularza, a wszystkie pola zostały pomyślnie zweryfikowane*.)

    Jeśli wszystkie zweryfikowane kolumny (nie były puste), możesz utworzyć instrukcję SQL, aby wstawić dane, a następnie wykonać ją w sposób przedstawiony dalej:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Aby można było wstawić wartości, należy uwzględnić symbole zastępcze parametrów (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Ze względów bezpieczeństwa zawsze przekazuj wartości do instrukcji SQL przy użyciu parametrów, jak widać w poprzednim przykładzie. Dzięki temu można sprawdzić poprawność danych użytkownika, a ponadto pomaga chronić przed próbami wysłania złośliwych poleceń do bazy danych (czasami nazywanych atakami polegającymi na iniekcji SQL).

    Aby wykonać zapytanie, należy użyć tej instrukcji, przekazując do niej zmienne zawierające wartości, które mają zostać zastąpione przez symbole zastępcze:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Po wykonaniu instrukcji `Insert Into` należy wysłać użytkownika na stronę, która zawiera listę produktów w tym wierszu:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Jeśli walidacja nie powiodła się, Pomiń Wstawianie. Zamiast tego masz na stronie pomocnika, który może wyświetlać skumulowane komunikaty o błędach (jeśli istnieją):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Zwróć uwagę, że blok stylu w znaczniku zawiera definicję klasy CSS o nazwie `.validation-summary-errors`. Jest to nazwa klasy CSS, która jest używana domyślnie dla elementu `<div>`, który zawiera błędy walidacji. W takim przypadku Klasa CSS określa, że błędy podsumowania walidacji są wyświetlane na czerwono i pogrubione, ale można zdefiniować klasę `.validation-summary-errors`, aby wyświetlić dowolne formatowanie, które lubisz.

### <a name="testing-the-insert-page"></a>Testowanie strony wstawiania

1. Wyświetl stronę w przeglądarce. Na stronie zostanie wyświetlony formularz podobny do przedstawionego na poniższej ilustracji.

    ![Image](5-working-with-data/_static/image5.jpg)
2. Wprowadź wartości dla wszystkich kolumn, ale upewnij się, że kolumna *Cena* nie jest pusta.
3. Kliknij przycisk **Wstaw**. Na stronie zostanie wyświetlony komunikat o błędzie, jak pokazano na poniższej ilustracji. (Nowy rekord nie został utworzony).

    ![Image](5-working-with-data/_static/image6.jpg)
4. Wypełnij formularz całkowicie, a następnie kliknij przycisk **Wstaw**. Tym razem zostanie wyświetlona strona *ListProducts. cshtml* i zostanie wyświetlony nowy rekord.

## <a name="updating-data-in-a-database"></a>Aktualizowanie danych w bazie danych

Po wprowadzeniu danych do tabeli może być konieczne jej zaktualizowanie. Ta procedura pokazuje, jak utworzyć dwie strony, które są podobne do tych, które zostały utworzone w celu wstawienia danych wcześniej. Na pierwszej stronie są wyświetlane produkty i użytkownicy wybierają je do zmiany. Druga strona pozwala użytkownikom w rzeczywistości wprowadzać zmiany i zapisywać je.

> [!NOTE] 
> 
> **Ważne** W produkcyjnej witrynie sieci Web zwykle ogranicza się, kto może wprowadzać zmiany w danych. Informacje o sposobie konfigurowania członkostwa i sposobach autoryzacji użytkowników do wykonywania zadań w witrynie można znaleźć w temacie [Dodawanie zabezpieczeń i członkostwa do witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).

1. W witrynie sieci Web Utwórz nowy plik CSHTML o nazwie *EditProducts. cshtml*.
2. Zastąp istniejący znacznik w pliku następującym:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Jedyną różnicą między tą stroną a stroną *ListProducts. cshtml* wcześniejszą jest to, że tabela HTML na tej stronie zawiera dodatkową kolumnę, która wyświetla link **Edytuj** . Kliknięcie tego linku spowoduje przejście do strony *UpdateProducts. cshtml* (którą utworzysz dalej), na której można edytować wybrany rekord.

    Przyjrzyj się kodowi, który tworzy link **edycji** :

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Spowoduje to utworzenie elementu HTML `<a>`, którego atrybut `href` jest ustawiany dynamicznie. Atrybut `href` określa stronę, która ma być wyświetlana, gdy użytkownik kliknie link. Przekazuje także wartość `Id` bieżącego wiersza do łącza. Po uruchomieniu strony Źródło strony może zawierać linki podobne do następujących:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Zwróć uwagę, że atrybut `href` jest ustawiony na `UpdateProducts/n`, gdzie *n* jest numerem produktu. Gdy użytkownik kliknie jedno z tych linków, otrzymany adres URL będzie wyglądać podobnie do poniższego:

    `http://localhost:18816/UpdateProducts/6`

    Innymi słowy, numer produktu do edycji zostanie przekazywać w adresie URL.
3. Wyświetl stronę w przeglądarce. Na stronie są wyświetlane dane w formacie podobnym do tego:

    ![Image](5-working-with-data/_static/image7.jpg)

    Następnie utworzysz stronę, która umożliwi użytkownikom rzeczywiste aktualizowanie danych. Strona aktualizacji zawiera weryfikację do zweryfikowania danych wprowadzonych przez użytkownika. Na przykład kod na stronie gwarantuje, że wprowadzono wartość dla wszystkich wymaganych kolumn.
4. W witrynie sieci Web Utwórz nowy plik CSHTML o nazwie *UpdateProducts. cshtml*.
5. Zastąp istniejący znacznik w pliku następującym.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Treść strony zawiera formularz HTML, w którym jest wyświetlany produkt i gdzie użytkownicy mogą go edytować. Aby można było wyświetlić produkt, należy użyć tej instrukcji SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Spowoduje to wybranie produktu, którego identyfikator pasuje do wartości, która została przeniesiona w parametrze `@0`. (Ponieważ *Identyfikator* jest kluczem podstawowym i dlatego musi być unikatowy, w ten sposób można wybrać tylko jeden rekord produktu). Aby uzyskać wartość identyfikatora do przekazania do tej instrukcji `Select`, można odczytać wartość, która jest przekazywany do strony w ramach adresu URL, przy użyciu następującej składni:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Aby faktycznie pobrać rekord produktu, należy użyć metody `QuerySingle`, która zwróci tylko jeden rekord:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Pojedynczy wiersz jest zwracany do zmiennej `row`. Możesz pobrać dane z każdej kolumny i przypisać je do zmiennych lokalnych, takich jak:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    W znacznikach formularza te wartości są wyświetlane automatycznie w poszczególnych polach tekstowych przy użyciu kodu osadzonego, takiego jak:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Ta część kodu wyświetla rekord produktu, który ma zostać zaktualizowany. Po wyświetleniu tego rekordu użytkownik może edytować pojedyncze kolumny.

    Gdy użytkownik prześle formularz, klikając przycisk **Aktualizuj** , zostanie uruchomiony kod w bloku `if(IsPost)`. Spowoduje to pobranie wartości użytkownika z obiektu `Request`, zapisanie wartości w zmiennych i sprawdzenie, czy każda kolumna została wypełniona. Jeśli walidacja kończy się powodzeniem, kod tworzy następującą instrukcję aktualizacji SQL:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    W instrukcji SQL `Update` należy określić każdą kolumnę do zaktualizowania i wartość, dla której ma zostać ustawiona. W tym kodzie wartości są określane przy użyciu zastępczych parametrów `@0`, `@1`, `@2`i tak dalej. (Jak wspomniano wcześniej, w przypadku zabezpieczeń należy zawsze przekazać wartości do instrukcji SQL przy użyciu parametrów).

    Po wywołaniu metody `db.Execute`, należy przekazać zmienne zawierające wartości w kolejności odpowiadającej parametrom w instrukcji SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Po wykonaniu instrukcji `Update` należy wywołać następującą metodę w celu przekierowania użytkownika z powrotem do strony edytowania:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Ten efekt polega na tym, że użytkownik widzi zaktualizowaną listę danych w bazie danych i może edytować inny produkt.
6. Zapisz stronę.
7. Uruchom stronę *EditProducts. cshtml* (nie stronę aktualizacji), a następnie kliknij przycisk **Edytuj** , aby wybrać produkt do edycji. Zostanie wyświetlona strona *UpdateProducts. cshtml* z wybranym rekordem.

    ![Image](5-working-with-data/_static/image8.jpg)
8. Wprowadź zmianę i kliknij przycisk **Aktualizuj**. Lista produktów zostanie ponownie pokazana ze zaktualizowanymi danymi.

## <a name="deleting-data-in-a-database"></a>Usuwanie danych w bazie danych

W tej sekcji pokazano, jak umożliwić użytkownikom usuwanie produktu z tabeli bazy danych *produktów* . Przykład składa się z dwóch stron. Na pierwszej stronie użytkownicy wybierają rekord do usunięcia. Rekord, który ma zostać usunięty, jest następnie wyświetlany na drugiej stronie, dzięki czemu można potwierdzić, że chcą usunąć rekord.

> [!NOTE] 
> 
> **Ważne** W produkcyjnej witrynie sieci Web zwykle ogranicza się, kto może wprowadzać zmiany w danych. Informacje o sposobie konfigurowania członkostwa i sposobach autoryzacji użytkownika do wykonywania zadań w witrynie można znaleźć w temacie [Dodawanie zabezpieczeń i członkostwa do witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).

1. W witrynie sieci Web Utwórz nowy plik CSHTML o nazwie *ListProductsForDelete. cshtml*.
2. Zastąp istniejący znacznik następującym:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Ta strona jest podobna do strony *EditProducts. cshtml* ze starszej wersji. Jednak zamiast wyświetlania linku **edycji** dla każdego produktu wyświetla link **Usuń** . Łącze **Usuń** jest tworzone przy użyciu następującego kodu osadzonego w znaczniku:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Spowoduje to utworzenie adresu URL, który wygląda tak, gdy użytkownik kliknie link:

    `http://<server>/DeleteProduct/4`

    Adres URL wywołuje stronę o nazwie *DeleteProduct. cshtml* (którą utworzysz dalej) i przekazuje jej identyfikator produktu do usunięcia (w tym przypadku 4).
3. Zapisz plik, pozostawiając go otwartym.
4. Utwórz inny plik CHTML o nazwie *DeleteProduct. cshtml*. Zastąp istniejącą zawartość następującym:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Ta strona jest wywoływana przez *ListProductsForDelete. cshtml* i pozwala użytkownikom potwierdzić, że chcą usunąć produkt. Aby wyświetlić listę produktów do usunięcia, należy uzyskać identyfikator produktu do usunięcia z adresu URL przy użyciu następującego kodu:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Na stronie zostanie wyświetlony monit o kliknięcie przycisku w celu usunięcia rekordu. Jest to ważna miara zabezpieczeń: w przypadku wykonywania poufnych operacji w witrynie sieci Web, takich jak aktualizowanie lub usuwanie danych, należy zawsze wykonać te operacje przy użyciu operacji POST, a nie operacji pobierania. Jeśli lokacja jest skonfigurowana tak, aby można było wykonać operację usuwania przy użyciu operacji GET, każdy może przekazać adres URL, taki jak `http://<server>/DeleteProduct/4`, i usunąć dowolne z bazy danych. Dodając potwierdzenie i kodowanie strony, aby można było je usunąć tylko przy użyciu wpisu POST, należy dodać miarę zabezpieczeń do lokacji.

    Rzeczywista operacja usuwania jest wykonywana przy użyciu poniższego kodu, który najpierw potwierdza, że jest to operacja post i że identyfikator nie jest pusty:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Kod uruchamia instrukcję SQL, która usuwa określony rekord, a następnie przekierowuje użytkownika z powrotem do strony z listą.
5. Uruchom *ListProductsForDelete. cshtml* w przeglądarce.

    ![Image](5-working-with-data/_static/image9.jpg)
6. Kliknij link **Usuń** dla jednego z produktów. Zostanie wyświetlona strona *DeleteProduct. cshtml* , aby potwierdzić, że chcesz usunąć ten rekord.
7. Kliknij przycisk **Usuń** . Rekord produktu zostanie usunięty, a strona zostanie odświeżona przy użyciu zaktualizowanej listy produktów.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Łączenie z bazą danych
> 
> Możesz połączyć się z bazą danych na dwa sposoby. Pierwszym jest użycie metody `Database.Open` i określenie nazwy pliku bazy danych (mniej niż rozszerzenie *. sdf* ):
> 
> `var db = Database.Open("SmallBakery");`
> 
> Metoda `Open` zakłada, że. plik *SDF* znajduje się w folderze *danych\_aplikacji* witryny sieci Web. Ten folder został zaprojektowany specjalnie do przechowywania danych. Na przykład ma odpowiednie uprawnienia do zezwalania witrynie sieci Web na odczytywanie i zapisywanie danych, a jako środek zabezpieczeń, WebMatrix nie zezwala na dostęp do plików z tego folderu.
> 
> Drugi sposób polega na użyciu parametrów połączenia. Parametry połączenia zawierają informacje o sposobie łączenia się z bazą danych. Może to obejmować ścieżkę pliku lub może zawierać nazwę bazy danych SQL Server na serwerze lokalnym lub zdalnym oraz nazwę użytkownika i hasło, aby połączyć się z tym serwerem. (Jeśli przechowujesz dane w centralnej zarządzanej wersji SQL Server, na przykład w lokacji dostawcy hostingu, zawsze możesz użyć parametrów połączenia do określenia informacji o połączeniu z bazą danych).
> 
> W programie WebMatrix parametry połączenia są zwykle przechowywane w pliku XML o nazwie *Web. config*. Jak nazywa się to, można użyć pliku *Web. config* w katalogu głównym witryny sieci Web do przechowywania informacji o konfiguracji lokacji, w tym wszystkich parametrów połączenia, które może wymagać witryna. Przykład parametrów połączenia w pliku *Web. config* może wyglądać następująco:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> W tym przykładzie parametry połączenia wskazują bazę danych w wystąpieniu SQL Server, które działa na serwerze, a nie (w przeciwieństwie do lokalnego pliku *. sdf* ). Należy zastąpić odpowiednie nazwy dla `myServer` i `myDatabase`i określić SQL Server wartości logowania dla `username` i `password`. (Wartości nazwy użytkownika i hasła nie muszą być takie same jak poświadczenia systemu Windows lub wartości, które dostawca hostingu udzielił Ci do logowania się do swoich serwerów. Należy skontaktować się z administratorem, aby uzyskać dokładne wartości, które są potrzebne.
> 
> Metoda `Database.Open` jest elastyczna, ponieważ umożliwia przekazywanie nazwy pliku Database *. sdf* lub nazwy parametrów połączenia, które są przechowywane w pliku *Web. config* . Poniższy przykład pokazuje, jak nawiązać połączenie z bazą danych przy użyciu parametrów połączenia przedstawionych w poprzednim przykładzie:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Zgodnie z zanotowaną metodą `Database.Open` można przekazać nazwę bazy danych lub parametry połączenia, które będą używane. Jest to bardzo przydatne podczas wdrażania (publikowania) witryny sieci Web. Podczas tworzenia i testowania lokacji można użyć pliku *. sdf* w folderze *danych\_aplikacji* . Następnie po przeniesieniu lokacji na serwer produkcyjny możesz użyć parametrów połączenia w pliku *Web. config* , który ma taką samą nazwę jak plik *. sdf* , ale wskazuje bazę danych &#8212; dostawcy hostingu bez konieczności zmiany kodu.
> 
> Na koniec, jeśli chcesz bezpośrednio korzystać z parametrów połączenia, możesz wywołać metodę `Database.OpenConnectionString` i przekazać do niej rzeczywiste parametry połączenia zamiast tylko nazwy jednego z nich w pliku *Web. config* . Może to być przydatne w sytuacjach, gdy z jakiegoś powodu nie masz dostępu do parametrów połączenia (lub wartości w nim, takich jak nazwa pliku *. sdf* ), dopóki strona nie zostanie uruchomiona. Jednak w przypadku większości scenariuszy można użyć `Database.Open` zgodnie z opisem w tym artykule.

## <a name="additional-resources"></a>Dodatkowe materiały

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Łączenie się z bazą danych SQL Server lub MySQL w programie WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Weryfikacja danych wejściowych użytkownika w witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
