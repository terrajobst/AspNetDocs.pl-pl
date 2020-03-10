---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Praca z kolumnami obliczanymi (C#) | Microsoft Docs
author: rick-anderson
description: Podczas tworzenia tabeli bazy danych Microsoft SQL Server umożliwia zdefiniowanie kolumny obliczanej, której wartość jest obliczana na podstawie wyrażenia, które zwykle referen...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: ad6a96f2721510c2478f707c8eed018ae797f27a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78531795"
---
# <a name="working-with-computed-columns-c"></a>Praca z kolumnami obliczanymi (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) lub [Pobierz plik PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Podczas tworzenia tabeli bazy danych Microsoft SQL Server umożliwia zdefiniowanie kolumny obliczanej, której wartość jest obliczana na podstawie wyrażenia, które zwykle odwołuje się do innych wartości w tym samym rekordzie bazy danych. Takie wartości są tylko do odczytu w bazie danych, co wymaga specjalnych zagadnień podczas pracy z TableAdapters. W tym samouczku dowiesz się, jak sprostać wyzwaniom związanym z kolumnami obliczanymi.

## <a name="introduction"></a>Wprowadzenie

Microsoft SQL Server umożliwia używanie *[kolumn obliczanych](https://msdn.microsoft.com/library/ms191250.aspx)* , które są kolumnami, których wartości są obliczane na podstawie wyrażenia, które zwykle odwołuje się do wartości z innych kolumn w tej samej tabeli. Przykładowo model danych śledzenia czasu może mieć tabelę o nazwie `ServiceLog` z kolumnami, w tym `ServicePerformed`, `EmployeeID`, `Rate`i `Duration`, między innymi. Chociaż kwota należna za każdy element usługi (jest to stawka pomnożona przez czas trwania), może być obliczana za pomocą strony sieci Web lub innego interfejsu programistycznego, ale może być przydatna do uwzględnienia kolumny w tabeli `ServiceLog` o nazwie `AmountDue`, która zgłosiła te informacje. Tę kolumnę można utworzyć jako normalną kolumnę, ale należy ją zaktualizować w dowolnym momencie, gdy wartości kolumn `Rate` lub `Duration` zostaną zmienione. Lepszym rozwiązaniem byłoby, aby kolumna `AmountDue` była kolumną obliczaną przy użyciu wyrażenia `Rate * Duration`. Wykonanie tej operacji spowodowałoby SQL Server automatyczne obliczenie `AmountDue` wartości kolumny za każdym razem, gdy odwołuje się do zapytania.

Ponieważ obliczona wartość kolumny s jest określana na podstawie wyrażenia, takie kolumny są tylko do odczytu i w związku z tym nie mogą mieć przypisanych do nich wartości w instrukcji `INSERT` lub `UPDATE`. Jednak gdy kolumny obliczane są częścią zapytania głównego dla TableAdapter, który używa instrukcji SQL ad hoc, są one automatycznie uwzględniane w `INSERT` automatycznie generowanych i `UPDATE`. W związku z tym TableAdapter s `INSERT` i `UPDATE` zapytania oraz `InsertCommand` i `UpdateCommand` właściwości muszą zostać zaktualizowane, aby usunąć odwołania do dowolnych kolumn obliczanych.

Jednym z wyzwań używania kolumn obliczanych z TableAdapter, które używają ad hoc instrukcji SQL, jest to, że TableAdapter s `INSERT` i `UPDATE` zapytania są automatycznie generowane ponownie za każdym razem, gdy Kreator konfiguracji TableAdapter zostanie ukończony. W związku z tym kolumny obliczane ręcznie usunięte z `INSERT` i `UPDATE` zapytania zostaną wyświetlone ponownie po ponownym uruchomieniu kreatora. Chociaż TableAdapters korzystające z procedur składowanych nie poniosły z tego brittleness, mają własne osobliwości, które będziemy zająć w kroku 3.

W tym samouczku dodamy kolumnę obliczaną do tabeli `Suppliers` w bazie danych Northwind, a następnie utworzysz odpowiednie TableAdapter do pracy z tą tabelą i jej kolumną obliczaną. Nasze TableAdapter będą używać procedur składowanych zamiast instrukcji SQL ad hoc, dzięki czemu nasze dostosowania nie zostaną utracone po użyciu Kreatora konfiguracji TableAdapter.

Zacznij korzystać z aplikacji.

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Krok 1. Dodawanie kolumny obliczanej do tabeli`Suppliers`

Baza danych Northwind nie ma żadnych kolumn obliczanych, więc konieczne będzie dodanie jednego wypróbujemy. W tym samouczku Dodaj kolumnę obliczaną do tabeli `Suppliers` o nazwie `FullContactName`, która zwraca nazwę kontaktu, tytuł i firmę, dla których działają, w następującym formacie: `ContactName` (`ContactTitle`, `CompanyName`). Ta kolumna obliczana może być używana w raportach w przypadku wyświetlania informacji o dostawcach.

Rozpocznij od otworzenia definicji tabeli `Suppliers`, klikając prawym przyciskiem myszy tabelę `Suppliers` w Eksplorator serwera i wybierając polecenie Otwórz definicję tabeli z menu kontekstowego. Spowoduje to wyświetlenie kolumn tabeli i ich właściwości, takich jak typ danych, niezależnie od tego, czy zezwalają one na `NULL` s i tak dalej. Aby dodać kolumnę obliczaną, Zacznij od wpisania nazwy kolumny do definicji tabeli. Następnie wprowadź wyrażenie do pola tekstowego (Formula) w sekcji Specyfikacja kolumn obliczanych w kolumnie okno Właściwości (patrz rysunek 1). Nadaj nazwę kolumnie obliczanej `FullContactName` i użyj następującego wyrażenia:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Należy zauważyć, że ciągi mogą być połączone w języku SQL przy użyciu operatora `+`. Instrukcji `CASE` można użyć jak warunku w tradycyjnym języku programowania. W powyższym wyrażeniu instrukcja `CASE` może zostać odczytana jako: Jeśli `ContactTitle` nie `NULL` następnie dane wyjściowe `ContactTitle` wartości połączone z przecinkiem, w przeciwnym razie nic nie emituj. Aby uzyskać więcej informacji na temat użyteczności instrukcji `CASE`, zobacz [moc instrukcji SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Zamiast korzystać z instrukcji `CASE` w tym miejscu, możemy użyć również `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) zwraca *checkExpression* , jeśli nie ma wartości null, w przeciwnym razie zwraca *replacementValue*. Mimo że `ISNULL` lub `CASE` będą działać w tym wystąpieniu, istnieje więcej intricateych scenariuszy, w których elastyczność instrukcji `CASE` nie można dopasować do `ISNULL`.

Po dodaniu tej kolumny obliczanej ekran powinien wyglądać jak zrzut ekranu na rysunku 1.

[![dodać kolumnę obliczaną o nazwie FullContactName do tabeli dostawcy](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Rysunek 1**. Dodawanie kolumny obliczanej o nazwie `FullContactName` do tabeli `Suppliers` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](working-with-computed-columns-cs/_static/image3.png))

Po nadaniu nazwy kolumnie obliczanej i wprowadzeniu jej wyrażenia Zapisz zmiany w tabeli, klikając ikonę Zapisz na pasku narzędzi, naciskając klawisze CTRL + S lub wybierając polecenie Zapisz `Suppliers`.

Zapisanie tabeli powinno spowodować odświeżenie Eksplorator serwera, łącznie z dodaną kolumną z listy kolumn `Suppliers` tabeli s. Ponadto wyrażenie wprowadzone do pola tekstowego (Formula) automatycznie dostosowuje się do równoważnego wyrażenia, które dzieli zbędne odstępy, otacza nazwy kolumn nawiasami (`[]`) i zawiera nawiasy, aby dokładniej pokazać kolejność operacji:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Aby uzyskać więcej informacji na temat kolumn obliczanych w Microsoft SQL Server, zapoznaj się z [dokumentacją techniczną](https://msdn.microsoft.com/library/ms191250.aspx). Zapoznaj się również z [tematem: Określanie kolumn obliczanych](https://msdn.microsoft.com/library/ms188300.aspx) dla przewodnika krok po kroku tworzenia kolumn obliczanych.

> [!NOTE]
> Domyślnie kolumny obliczane nie są fizycznie przechowywane w tabeli, ale zamiast tego są ponownie obliczane za każdym razem, gdy odwołują się do zapytania. Sprawdzając, czy jest to utrwalone pole wyboru, można wydać SQL Server, aby fizycznie przechowywać kolumnę obliczaną w tabeli. Dzięki temu można utworzyć indeks w kolumnie obliczanej, co może zwiększyć wydajność zapytań, które używają wartości kolumny obliczanej w ich klauzulach `WHERE`. Aby uzyskać więcej informacji, zobacz [Tworzenie indeksów w kolumnach obliczanych](https://msdn.microsoft.com/library/ms189292.aspx) .

## <a name="step-2-viewing-the-computed-column-s-values"></a>Krok 2. Wyświetlanie wartości kolumn obliczanych

Zanim zaczniemy pracę nad warstwą dostępu do danych, poświęć chwilę, aby wyświetlić wartości `FullContactName`. W Eksplorator serwera kliknij prawym przyciskiem myszy nazwę tabeli `Suppliers` i wybierz polecenie nowe zapytanie z menu kontekstowego. Spowoduje to wyświetlenie okna zapytania z prośbą o wybranie tabel do uwzględnienia w zapytaniu. Dodaj tabelę `Suppliers` i kliknij przycisk Zamknij. Następnie sprawdź kolumny `CompanyName`, `ContactName`, `ContactTitle`i `FullContactName` z tabeli dostawcy. Na koniec kliknij ikonę czerwony wykrzyknika na pasku narzędzi, aby wykonać zapytanie i wyświetlić wyniki.

Jak pokazano na rysunku 2, wyniki zawierają `FullContactName`, które wyświetlają kolumny `CompanyName`, `ContactName`i `ContactTitle` przy użyciu formatu ldquo;`ContactName` (`ContactTitle`, `CompanyName`).

[![FullContactName używa formatu ContactName (ContactTitle, NazwaFirmy)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Rysunek 2**. `FullContactName` używa formatu `ContactName` (`ContactTitle`, `CompanyName`) (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](working-with-computed-columns-cs/_static/image6.png))

## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Krok 3. Dodawanie`SuppliersTableAdapter`do warstwy dostępu do danych

Aby móc korzystać z informacji o dostawcy w naszej aplikacji, musisz najpierw utworzyć TableAdapter i DataTable w naszym DAL. W idealnym przypadku można to zrobić przy użyciu tych samych prostych kroków, które zostały sprawdzone w poprzednich samouczkach. Jednak praca z kolumnami obliczanymi wprowadza kilka wrinklesów, które są charakterystyczne dla dyskusji.

Jeśli używasz TableAdapter, który używa instrukcji SQL ad hoc, możesz po prostu dołączyć kolumnę obliczaną w głównym zapytaniu TableAdapter s za pośrednictwem Kreatora konfiguracji TableAdapter. Jednak spowoduje to automatyczne wygenerowanie instrukcji `INSERT` i `UPDATE`, które zawierają kolumnę obliczaną. Jeśli podjęto próbę wykonania jednej z tych metod, `SqlException` z komunikatem nie można zmodyfikować kolumny " *ColumnName* ", ponieważ jest to kolumna obliczana albo wynik operatora Union zostanie wygenerowany. Chociaż instrukcje `INSERT` i `UPDATE` można korygować ręcznie za pomocą właściwości TableAdapter s `InsertCommand` i `UpdateCommand`, te dostosowania zostaną utracone po każdym ponownym uruchomieniu Kreatora konfiguracji TableAdapter.

Ze względu na brittleness TableAdapters, który używa instrukcji SQL ad hoc, zaleca się korzystanie z procedur składowanych podczas pracy z kolumnami obliczanymi. Jeśli używasz istniejących procedur składowanych, po prostu Skonfiguruj TableAdapter zgodnie z opisem w temacie [using Existing procedurs for the datasetd zestaw danych s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) . W przypadku tworzenia przez kreatora TableAdapter procedur składowanych należy jednak najpierw pominąć wszystkie kolumny obliczane z głównego zapytania. Jeśli w zapytaniu głównym dołączysz kolumnę obliczaną, Kreator konfiguracji TableAdapter wyświetli powiadomienie po zakończeniu, że nie może utworzyć odpowiednich procedur składowanych. Krótko mówiąc, najpierw musimy skonfigurować TableAdapter przy użyciu kwerendy głównej z obliczanymi kolumnami, a następnie ręcznie zaktualizować odpowiednią procedurę składowaną oraz `SelectCommand` TableAdapter s w celu uwzględnienia kolumny obliczanej. Takie podejście jest podobne do tej, która jest używana *w`JOIN`* [TableAdapter](updating-the-tableadapter-to-use-joins-cs.md) .

W tym samouczku dodamy do usługi s nowe TableAdapter i automatycznie utworzysz procedury składowane. W związku z tym musimy najpierw pominąć `FullContactName` kolumnę obliczaną z głównego zapytania.

Zacznij od otwarcia `NorthwindWithSprocs` zestawu danych w folderze `~/App_Code/DAL`. Kliknij prawym przyciskiem myszy w projektancie, a następnie z menu kontekstowego wybierz polecenie, aby dodać nowy TableAdapter. Spowoduje to uruchomienie Kreatora konfiguracji TableAdapter. Określ bazę danych, z której mają być przeszukiwane dane (`NORTHWNDConnectionString` from `Web.config`), a następnie kliknij przycisk Dalej. Ponieważ nie zostały jeszcze utworzone żadne procedury składowane służące do wykonywania zapytań lub modyfikowania tabeli `Suppliers`, wybierz opcję Utwórz nowe procedury składowane, aby Kreator utworzy je dla nas, i kliknij przycisk Dalej.

[![wybrać opcję Utwórz nowe procedury składowane](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Rysunek 3**. Wybierz opcję Utwórz nowe procedury składowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](working-with-computed-columns-cs/_static/image9.png))

W następnym kroku zostanie wyświetlony komunikat z prośbą o zapytanie główne. Wprowadź następujące zapytanie, które zwraca kolumny `SupplierID`, `CompanyName`, `ContactName`i `ContactTitle` dla każdego dostawcy. Zwróć uwagę, że to zapytanie celowo całkowicie pomija kolumnę obliczaną (`FullContactName`); będziemy aktualizować odpowiednią procedurę przechowywaną, aby uwzględnić tę kolumnę w kroku 4.

[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Po wprowadzeniu głównego zapytania i kliknięciu przycisku Dalej Kreator umożliwi nam nadawanie nazw wygenerowanym przez niego procedur składowanych. Nazwij te procedury składowane `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`i `Suppliers_Delete`, jak pokazano na rysunku 4.

[![dostosować nazwy automatycznie generowanych procedur składowanych](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Ilustracja 4**. Dostosowywanie nazw automatycznie generowanych procedur składowanych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-cs/_static/image12.png))

Następny krok kreatora umożliwia nawiązanie nazwy metod TableAdapter i określenie wzorców używanych do uzyskiwania dostępu do danych i ich aktualizowania. Pozostaw wszystkie trzy pola wyboru zaznaczone, ale Zmień nazwę metody `GetData` na `GetSuppliers`. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![zmienić nazwy metody GetData na getdostawcs](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Rysunek 5**. zmiana nazwy metody `GetData` na `GetSuppliers` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-cs/_static/image15.png))

Po kliknięciu przycisku Zakończ Kreator utworzy cztery procedury składowane i doda TableAdapter i odpowiednią wartość DataTable do określonego zestawu danych.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Krok 4. uwzględnienie kolumny obliczanej w kwerendzie głównej TableAdapter s

Teraz należy zaktualizować TableAdapter i DataTable utworzone w kroku 3, aby uwzględnić kolumnę obliczaną `FullContactName`. Obejmuje to dwie czynności:

1. Aktualizowanie `Suppliers_Select` procedury składowanej w celu zwrócenia `FullContactName` kolumny obliczanej i
2. Aktualizowanie elementu DataTable w celu uwzględnienia odpowiedniej kolumny `FullContactName`.

Zacznij od przejścia do Eksplorator serwera i przechodzenia do szczegółów w folderze procedury składowane. Otwórz `Suppliers_Select` procedury składowanej i zaktualizuj zapytanie `SELECT`, aby zawierało `FullContactName` kolumnę obliczaną:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Zapisz zmiany w procedurze składowanej, klikając ikonę Zapisz na pasku narzędzi, naciskając klawisze CTRL + S lub wybierając opcję Zapisz `Suppliers_Select` z menu plik.

Następnie wróć do projektanta obiektów DataSet, kliknij prawym przyciskiem myszy `SuppliersTableAdapter`i wybierz polecenie Konfiguruj z menu kontekstowego. Należy pamiętać, że kolumna `Suppliers_Select` zawiera teraz kolumnę `FullContactName` w kolekcji kolumn danych.

[![uruchomić Kreatora konfiguracji TableAdapter s w celu zaktualizowania kolumn DataTable s](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Ilustracja 6**. uruchomienie Kreatora konfiguracji TableAdapter s w celu zaktualizowania kolumn DataTable s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](working-with-computed-columns-cs/_static/image18.png))

Kliknij przycisk Zakończ, aby zakończyć pracę kreatora. Spowoduje to automatyczne dodanie odpowiedniej kolumny do `SuppliersDataTable`. Kreator TableAdapter jest wystarczająco inteligentny, aby wykryć, że kolumna `FullContactName` jest kolumną obliczaną i w związku z tym tylko do odczytu. W związku z tym ustawia Właściwość Column s `ReadOnly` na `true`. Aby to sprawdzić, wybierz kolumnę z `SuppliersDataTable` a następnie przejdź do okno Właściwości (patrz rysunek 7). Należy pamiętać, że `FullContactName` kolumn `DataType` i `MaxLength` właściwości są również odpowiednio ustawiane.

[![kolumna FullContactName jest oznaczona jako tylko do odczytu](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Rysunek 7**: kolumna `FullContactName` jest oznaczona jako tylko do odczytu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](working-with-computed-columns-cs/_static/image21.png))

## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Krok 5. Dodawanie metody`GetSupplierBySupplierID`do TableAdapter

W tym samouczku utworzymy stronę ASP.NET, która będzie wyświetlać dostawców w siatce z aktualizowalną aktualizacją. W poprzednich samouczkach Zaktualizowaliśmy pojedynczy rekord z warstwy logiki biznesowej, pobierając ten konkretny rekord z DAL jako element DataTable o jednoznacznie określonym typie, aktualizując jego właściwości, a następnie wysyłając zaktualizowany element DataTable z powrotem do DAL w celu propagowania zmian do Baza danych programu. Aby wykonać ten pierwszy krok — pobranie zaktualizowanego rekordu z DAL — musimy najpierw dodać metodę `GetSupplierBySupplierID(supplierID)` do DAL.

Kliknij prawym przyciskiem myszy `SuppliersTableAdapter` w projekcie zestawu danych i wybierz opcję Dodaj zapytanie z menu kontekstowego. Tak jak w kroku 3, niech Kreator wygeneruje nową procedurę składowaną dla nas, wybierając opcję Utwórz nową procedurę składowaną (zapoznaj się z powrotem do rysunku 3 dla zrzutu ekranu tego kroku kreatora). Ponieważ ta metoda zwróci rekord z wieloma kolumnami, wskaż, że chcemy użyć zapytania SQL, które zwraca wiersze, a następnie kliknij przycisk Dalej.

[![wybierz opcję Wybierz, która zwraca wiersze](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Ilustracja 8**. Wybierz opcję Wybierz, która zwraca wiersze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-cs/_static/image24.png))

W następnym kroku zostanie wyświetlony komunikat z prośbą o zapytanie, które ma być używane dla tej metody. Wprowadź następujące polecenie, które zwraca te same pola danych co główne zapytanie, ale dla określonego dostawcy.

[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

Na następnym ekranie poprosimy nas o podanie nazwy procedury przechowywanej, która zostanie wygenerowana automatycznie. Nadaj nazwę tej procedurze składowanej `Suppliers_SelectBySupplierID` a następnie kliknij przycisk Dalej.

[![nazwę procedury składowanej Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Ilustracja 9**. nazwa `Suppliers_SelectBySupplierID` procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-cs/_static/image27.png))

Na koniec Kreator poprosi nas o wzorce dostępu do danych i nazwy metod, które mają być używane dla TableAdapter. Pozostaw zaznaczone pole wyboru, ale Zmień nazwę `FillBy` i `GetDataBy` metody na odpowiednio `FillBySupplierID` i `GetSupplierBySupplierID`.

[Nazwa ![TableAdapter FillBySupplierID i GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Ilustracja 10**. Nazwij metody TableAdapter `FillBySupplierID` i `GetSupplierBySupplierID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-cs/_static/image30.png))

Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

## <a name="step-6-creating-the-business-logic-layer"></a>Krok 6. Tworzenie warstwy logiki biznesowej

Przed utworzeniem strony ASP.NET, która używa kolumny obliczanej utworzonej w kroku 1, najpierw musimy dodać odpowiednie metody w LOGIKI biznesowej. Nasza strona ASP.NET, która zostanie utworzona w kroku 7, umożliwi użytkownikom wyświetlanie i edytowanie dostawców. W związku z tym potrzebujemy naszej LOGIKI biznesowej, aby zapewnić wszystkim dostawcom i innym podmiotom, aby zaktualizować określonego dostawcę.

Utwórz nowy plik klasy o nazwie `SuppliersBLLWithSprocs` w folderze `~/App_Code/BLL` i Dodaj następujący kod:

[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Podobnie jak w przypadku innych klas LOGIKI biznesowej, `SuppliersBLLWithSprocs` ma właściwość `protected` `Adapter`, która zwraca wystąpienie klasy `SuppliersTableAdapter` oraz dwie metody `public`: `GetSuppliers` i `UpdateSupplier`. Metoda `GetSuppliers` wywołuje i zwraca `SuppliersDataTable` zwracany przez odpowiednią metodę `GetSupplier` w warstwie dostępu do danych. Metoda `UpdateSupplier` pobiera informacje o konkretnym dostawcy, które są aktualizowane za pośrednictwem wywołania metody `GetSupplierBySupplierID(supplierID)` DAL s. Następnie aktualizuje `CategoryName`, `ContactName`i `ContactTitle` właściwości i zatwierdza te zmiany w bazie danych, wywołując metodę dostępu do danych `Update` metodzie, przekazując do zmodyfikowanego obiektu `SuppliersRow`.

> [!NOTE]
> Z wyjątkiem `SupplierID` i `CompanyName`wszystkie kolumny w tabeli dostawcy zezwalają na wartości `NULL`. W związku z tym, jeśli `contactName` `contactTitle` lub parametry, które zostały przesłane, są `null` musimy ustawić odpowiednie `ContactName` i `ContactTitle` do wartości bazy danych `NULL` odpowiednio przy użyciu metod `SetContactNameNull` i `SetContactTitleNull`.

## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Krok 7. Praca z kolumną obliczaną z warstwy prezentacji

Dzięki kolumnie obliczanej dodawanej do tabeli `Suppliers`, a DAL i LOGIKI biznesowej odpowiednio zaktualizowane, jesteśmy gotowi do skompilowania strony ASP.NET, która współpracuje z `FullContactName` kolumną obliczaną. Zacznij od otworzenia strony `ComputedColumns.aspx` w folderze `AdvancedDAL` i przeciągnij widok GridView z przybornika na projektanta. Ustaw właściwość GridView s `ID` na `Suppliers` i, z jej tagu inteligentnego, powiąż ją z nowym elementem ObjectDataSource o nazwie `SuppliersDataSource`. Skonfiguruj element ObjectDataSource, aby używał klasy `SuppliersBLLWithSprocs`, która została dodana z powrotem w kroku 6, a następnie kliknij przycisk Dalej.

[![skonfigurować element ObjectDataSource do używania klasy SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Ilustracja 11**. Konfigurowanie elementu ObjectDataSource do używania klasy `SuppliersBLLWithSprocs` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](working-with-computed-columns-cs/_static/image33.png))

W klasie `SuppliersBLLWithSprocs` są zdefiniowane tylko dwie metody: `GetSuppliers` i `UpdateSupplier`. Upewnij się, że te dwie metody są odpowiednio określone na kartach wybierz i Aktualizuj, a następnie kliknij przycisk Zakończ, aby zakończyć konfigurację elementu ObjectDataSource.

Po zakończeniu pracy Kreatora konfiguracji źródła danych program Visual Studio doda BoundField dla każdego zwróconego pola danych. Usuń `SupplierID` BoundField i zmień właściwości `HeaderText` `CompanyName`, `ContactName`, `ContactTitle`i `FullContactName` BoundFields na nazwę firmy, kontakt, tytuł i pełną nazwę kontaktu. W tagu inteligentnym zaznacz pole wyboru Włącz edycję, aby włączyć wbudowane funkcje edycji GridView.

Oprócz dodawania BoundFields do widoku GridView, ukończenie kreatora źródła danych powoduje również, że program Visual Studio ustawił Właściwość ObjectDataSource s `OldValuesParameterFormatString` na oryginalny\_{0}. Przywróć wartość domyślną, {0}.

Po wprowadzeniu tych zmian do elementów GridView i ObjectDataSource, ich deklaratywne znaczniki powinny wyglądać podobnie do następujących:

[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

Następnie odwiedź tę stronę za pomocą przeglądarki. Jak pokazano na rysunku 12, każdy dostawca jest wyświetlany w siatce zawierającej kolumnę `FullContactName`, której wartość jest po prostu połączeniem innych trzech kolumn sformatowanych jako `ContactName` (`ContactTitle`, `CompanyName`).

[![każdy dostawca znajduje się na liście w siatce](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Rysunek 12**: każdy dostawca jest wymieniony w siatce ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](working-with-computed-columns-cs/_static/image36.png))

Kliknięcie przycisku Edytuj dla danego dostawcy powoduje odświeżenie i odwzorowanie tego wiersza w interfejsie edycji (Zobacz Rysunek 13). Pierwsze trzy kolumny renderują w domyślnym interfejsie edycji — kontrolka TextBox, której Właściwość `Text` jest ustawiona na wartość pola dane. Kolumna `FullContactName`, jednak pozostanie jako tekst. Gdy BoundFields zostały dodane do widoku GridView po zakończeniu pracy Kreatora konfiguracji źródła danych, właściwość `FullContactName` BoundField s `ReadOnly` została ustawiona na `true`, ponieważ odpowiadająca kolumna `FullContactName` w `SuppliersDataTable` ma właściwość `ReadOnly` ustawioną na `true`. Jak wskazano w kroku 4, właściwość `FullContactName` s `ReadOnly` została ustawiona na `true`, ponieważ TableAdapter wykrył, że kolumna była kolumną obliczaną.

[![nie można edytować kolumny FullContactName](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Ilustracja 13**. kolumna `FullContactName` nie jest edytowalna ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](working-with-computed-columns-cs/_static/image39.png))

Przejdź dalej i zaktualizuj wartość jednej lub kilku edytowalnych kolumn, a następnie kliknij przycisk Aktualizuj. Zwróć uwagę, jak wartość `FullContactName` s jest automatycznie aktualizowana w celu odzwierciedlenia zmiany.

> [!NOTE]
> W widoku GridView są obecnie stosowane BoundFields do pól edytowalnych, co spowodowało domyślny interfejs edycji. Ponieważ pole `CompanyName` jest wymagane, powinno być konwertowane na TemplateField, które zawiera RequiredFieldValidator. Jestem to ćwiczenie dla interesującego czytnika. Aby uzyskać instrukcje krok po kroku dotyczące konwertowania BoundField na TemplateField i dodawania kontrolek sprawdzania poprawności, zapoznaj się z tematem [Dodawanie kontrolek weryfikacji do okna Edycja i wstawianie interfejsów](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) .

## <a name="summary"></a>Podsumowanie

Podczas definiowania schematu dla tabeli Microsoft SQL Server umożliwia uwzględnienie kolumn obliczanych. Są to kolumny, których wartości są obliczane na podstawie wyrażenia, które zwykle odwołuje się do wartości z innych kolumn w tym samym rekordzie. Ponieważ wartości kolumn obliczanych opierają się na wyrażeniu, są one tylko do odczytu i nie można przypisać wartości w instrukcji `INSERT` lub `UPDATE`. W ten sposób wprowadzono wyzwania podczas korzystania z kolumny obliczanej w głównym zapytaniu TableAdapter, które próbuje automatycznie generować odpowiednie instrukcje `INSERT`, `UPDATE`i `DELETE`.

W tym samouczku omówiono techniki obejścia wyzwań powodowanych przez kolumny obliczane. W szczególności zostały użyte procedury składowane w naszym TableAdapter, aby przezwyciężyć brittleness nieodłączne w TableAdapters, które używają ad hoc instrukcji SQL. Gdy Kreator TableAdapter tworzy nowe procedury składowane, ważne jest, aby najpierw pominąć wszystkie kolumny obliczane, ponieważ ich obecność uniemożliwia wygenerowanie procedur składowanych modyfikacji danych. Po wstępnym skonfigurowaniu TableAdapter `SelectCommand` procedury składowanej można ponownie dołączyć do dowolnych kolumn obliczanych.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Hilton Geisenow i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](adding-additional-datatable-columns-cs.md)
> [dalej](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
