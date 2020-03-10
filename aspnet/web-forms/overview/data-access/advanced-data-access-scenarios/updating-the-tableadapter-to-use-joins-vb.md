---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: Aktualizowanie TableAdapter do używania sprzężeń (VB) | Microsoft Docs
author: rick-anderson
description: Podczas pracy z bazą danych często żądają danych rozłożonych między wieloma tabelami. Aby pobrać dane z dwóch różnych tabel, możemy użyć jednej z nich...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c94baa99b126cdd24d69afc3d02bfe8b069419b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78532271"
---
# <a name="updating-the-tableadapter-to-use-joins-vb"></a>Aktualizowanie elementu TableAdapter w celu używania sprzężeń JOIN (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) lub [Pobierz plik PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Podczas pracy z bazą danych często żądają danych rozłożonych między wieloma tabelami. Aby pobrać dane z dwóch różnych tabel, można użyć skorelowanego podzapytania lub operacji JOIN. W tym samouczku porównano skorelowane podzapytania i składnię JOIN przed rozpoczęciem tworzenia TableAdapter zawierającego SPRZĘŻENIe w jego głównym zapytaniu.

## <a name="introduction"></a>Wprowadzenie

W przypadku relacyjnych baz danych dane, z którymi jesteśmy zainteresowani, są często rozłożone na wiele tabel. Na przykład podczas wyświetlania informacji o produkcie warto wyświetlić listę odpowiednich nazw kategorii i dostawców produktów. Tabela `Products` ma `CategoryID` i `SupplierID` wartości, ale rzeczywista nazwa kategorii i dostawcy znajdują się odpowiednio w tabelach `Categories` i `Suppliers`.

Aby pobrać informacje z innej powiązanej tabeli, można użyć *skorelowanych podkwerend* lub `JOIN`*s*. Skorelowane podzapytanie jest zagnieżdżoną kwerendą `SELECT`, która odwołuje się do kolumn w zewnętrznym zapytaniu. Na przykład w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) zostały użyte dwa skorelowane podzapytania w zapytaniu głównym `ProductsTableAdapter` s w celu zwrócenia kategorii i nazw dostawców dla każdego produktu. `JOIN` to konstrukcja SQL, która scala powiązane wiersze z dwóch różnych tabel. Do wyświetlania informacji o kategorii obok każdego produktu użyto `JOIN` w [danych zapytania za pomocą samouczka kontrolki kontrolki SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) .

Przyczyna abstained z używania `JOIN` s z TableAdapters wynika z faktu, że ograniczenia w Kreatorze TableAdapter s umożliwiają automatyczne generowanie odpowiednich instrukcji `INSERT`, `UPDATE`i `DELETE`. Dokładniej mówiąc, jeśli główne zapytanie TableAdapter s zawiera dowolne `JOIN` s, TableAdapter nie może utworzyć instrukcji SQL ad hoc ani procedur składowanych dla właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand`.

W tym samouczku zostaną krótko porównane i powiązane podzapytania, a `JOIN` s przed rozpoczęciem eksplorowania sposobu tworzenia TableAdapter zawierającego `JOIN` s w jego głównym zapytaniu.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Porównywanie i różnicowanie skorelowanych podzapytań oraz`JOIN` s

Odwołaj, że `ProductsTableAdapter` utworzone w pierwszym samouczku w `Northwind` DataSet używa skorelowanego podzapytań, aby przywrócić każde odpowiednie produkty i nazwę dostawcy. Poniżej przedstawiono zapytanie główne `ProductsTableAdapter` s.

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

Dwa skorelowane podzapytania — `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` i `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` — są `SELECT` zapytania, które zwracają pojedynczą wartość na produkt jako dodatkową kolumnę na liście kolumn zewnętrznej `SELECT` instrukcji s.

Alternatywnie można użyć `JOIN`, aby zwrócić każdy dostawca produktu i nazwę kategorii. Poniższe zapytanie zwraca te same dane wyjściowe co powyżej, ale używa `JOIN` s zamiast podkwerend:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

`JOIN` scala rekordy z jednej tabeli z rekordami z innej tabeli na podstawie niektórych kryteriów. Na przykład w powyższym zapytaniu `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` instruuje SQL Server, aby scalić każdy rekord produktu z rekordem kategorii, którego wartość `CategoryID` jest zgodna z wartością `CategoryID` produktu. Scalony wynik pozwala nam korzystać z odpowiednich pól kategorii dla każdego produktu (na przykład `CategoryName`).

> [!NOTE]
> `JOIN` s są często używane podczas wykonywania zapytań dotyczących danych z relacyjnych baz danych. Jeśli jesteś nowym użytkownikiem składni `JOIN` lub potrzebujesz pędzla w górę w celu użycia, I d zalecamy zapoznanie się z [samouczkiem dotyczącym sprzężenia SQL](http://www.w3schools.com/sql/sql_join.asp) w [szkołach W3](http://www.w3schools.com/). Jest [`JOIN`](https://msdn.microsoft.com/library/ms191517.aspx) to również, że odczytywanie to sekcje podstawowe i [podzapytania](https://msdn.microsoft.com/library/ms189575.aspx) dotyczące usługi [SQL Books Online](https://msdn.microsoft.com/library/ms130214.aspx).

Ze względu na to, że `JOIN` s i skorelowane podzapytania mogą być używane do pobierania powiązanych danych z innych tabel, wielu deweloperów pozostawiła swoje głowice i zastanawia się, które podejście ma być używane. Wszystkie Gurus języka SQL, do których odpowiedziano te same działania, tak samo, że nie jest to bardzo istotne dla wydajności, ponieważ SQL Server będzie generować bardzo identyczne plany wykonania. W tym celu należy użyć techniki, z którą ty i Twój zespół. Warto zwrócić uwagę na to, że po przeprowadzeniu tej porady eksperci natychmiast wyrażają preferencję `JOIN` s za pośrednictwem skorelowanych podzapytań.

Podczas kompilowania warstwy dostępu do danych przy użyciu wpisanych zestawów DataSet narzędzia działają lepiej podczas korzystania z podzapytań. W szczególności Kreator TableAdapter s nie generuje automatycznie odpowiednich instrukcji `INSERT`, `UPDATE`i `DELETE`, jeśli główne zapytanie zawiera wszystkie `JOIN` s, ale te instrukcje zostaną automatycznie wygenerowane w przypadku użycia skorelowanych podzapytań.

Aby zbadać to niedoskonałość, należy utworzyć tymczasowy zestaw danych o określonym typie w folderze `~/App_Code/DAL`. W Kreatorze konfiguracji TableAdapter wybierz, aby użyć instrukcji SQL ad hoc i wprowadź następujące zapytanie `SELECT` (patrz rysunek 1):

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]

[![wprowadzić głównej kwerendy zawierającej sprzężenia](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Rysunek 1**. Wprowadź główną kwerendę zawierającą `JOIN` s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))

Domyślnie TableAdapter automatycznie tworzy instrukcje `INSERT`, `UPDATE`i `DELETE` na podstawie zapytania głównego. Po kliknięciu przycisku Zaawansowane można zobaczyć, że ta funkcja jest włączona. Pomimo tego ustawienia TableAdapter nie będzie mógł utworzyć instrukcji `INSERT`, `UPDATE`i `DELETE`, ponieważ główne zapytanie zawiera `JOIN`.

![Wprowadź zapytanie główne zawierające sprzężenia](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Rysunek 2**. wprowadzanie głównego zapytania zawierającego `JOIN` s

Kliknij przycisk Zakończ, aby zakończyć pracę kreatora. W tym momencie Projektant zestawu danych s będzie zawierać pojedyncze TableAdapter z elementem DataTable z kolumnami dla każdego z pól zwróconych na liście kolumn `SELECT` zapytania s. Obejmuje to `CategoryName` i `SupplierName`, jak pokazano na rysunku 3.

![Element DataTable zawiera kolumnę dla każdego pola zwróconego na liście kolumn.](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Rysunek 3**. tabela DataTable zawiera kolumnę dla każdego pola zwróconego na liście kolumn

Gdy element DataTable ma odpowiednie kolumny, TableAdapter nie ma wartości dla właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand`. Aby to potwierdzić, kliknij TableAdapter w projektancie, a następnie przejdź do okno Właściwości. Zobaczysz, że właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand` są ustawione na wartość (brak).

[![właściwości InsertCommand, UpdateCommand i DeleteCommand są ustawione na (None)](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Rysunek 4**. właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand` są ustawione na (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))

Aby obejść ten brak, możemy ręcznie dostarczyć instrukcje i parametry SQL dla `InsertCommand`, `UpdateCommand`i `DeleteCommand` właściwości za pośrednictwem okno Właściwości. Alternatywnie można rozpocząć od skonfigurowania zapytania głównego TableAdapter s, aby *nie* zawierały żadnych `JOIN` s. Dzięki temu instrukcje `INSERT`, `UPDATE`i `DELETE` będą generowane automatycznie dla nas. Po zakończeniu działania kreatora można ręcznie zaktualizować TableAdapter `SelectCommand` z okno Właściwości, aby zawierał składnię `JOIN`.

Chociaż to podejście działa, jest bardzo kruchy w przypadku korzystania z zapytań SQL ad hoc, ponieważ za każdym razem, gdy zapytanie główne TableAdapter s zostało ponownie skonfigurowane za pośrednictwem kreatora, automatycznie generowane instrukcje `INSERT`, `UPDATE`i `DELETE`. Oznacza to, że wszystkie wprowadzone później modyfikacje zostaną utracone, jeśli zostanie kliknięty prawym przyciskiem myszy w TableAdapter, wybierz pozycję Konfiguruj z menu kontekstowego, a następnie ponownie Ukończ pracę kreatora.

Brittleness z TableAdapter s `INSERT`generowanym automatycznie, `UPDATE`i `DELETE` są na szczęście, ograniczone do instrukcji SQL ad hoc. Jeśli TableAdapter korzysta z procedur składowanych, można dostosować `SelectCommand`, `InsertCommand`, `UpdateCommand`lub `DeleteCommand` procedury składowane, a następnie ponownie uruchomić Kreatora konfiguracji TableAdapter bez konieczności obawowania, że procedury składowane zostaną zmodyfikowane.

W ciągu następnych kilku kroków utworzymy TableAdapter, która początkowo korzysta z głównego zapytania, które pomija wszystkie `JOIN` s, aby odpowiednie procedury składowane INSERT, Update i DELETE będą generowane automatycznie. Następnie zaktualizujemy `SelectCommand` tak, aby używała `JOIN`, która zwraca dodatkowe kolumny z powiązanych tabel. Na koniec utworzymy odpowiednią klasę warstwy logiki biznesowej i wykażemy użycie TableAdapter na stronie sieci Web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Krok 1. Tworzenie TableAdapter przy użyciu uproszczonego zapytania głównego

W tym samouczku dodamy TableAdapter i silnie wpisaną wartość DataTable dla tabeli `Employees` w zestawie danych `NorthwindWithSprocs`. Tabela `Employees` zawiera pole `ReportsTo`, które określa `EmployeeID` menedżera pracowników. Na przykład pracownik Anne Dodsworth ma `ReportTo` wartość 5, czyli `EmployeeID` z Steven Buchanan. W związku z tym Anne raporty do Steven, jego Menedżera. Wraz z raportowaniem każdej wartości `ReportsTo` pracownika może być również konieczne pobranie nazwy swojego menedżera. Można to zrobić przy użyciu `JOIN`. Jednak przy pierwszym tworzeniu TableAdapter przy użyciu `JOIN` uniemożliwia kreatorowi automatyczne generowanie odpowiednich funkcji INSERT, Update i DELETE. W związku z tym zaczniemy od utworzenia elementu TableAdapter, którego zapytanie główne nie zawiera żadnych `JOIN` s. Następnie w kroku 2 będziemy aktualizować główną procedurę przechowywaną zapytania, aby pobrać nazwę Menedżera za pośrednictwem `JOIN`.

Zacznij od otwarcia `NorthwindWithSprocs` zestawu danych w folderze `~/App_Code/DAL`. Kliknij prawym przyciskiem myszy projektanta, wybierz opcję Dodaj z menu kontekstowego i wybierz element menu TableAdapter. Spowoduje to uruchomienie Kreatora konfiguracji TableAdapter. Zgodnie z ilustracją 5 przedstawia Kreator tworzy nowe procedury składowane i klika dalej. Aby uzyskać odświeżacz dotyczący tworzenia nowych procedur składowanych z poziomu kreatora TableAdapter s, zapoznaj się z tematem [Tworzenie nowych procedur składowanych dla określonego typu zestawu danych s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

[![wybrać opcję Utwórz nowe procedury składowane](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Rysunek 5**. Wybierz opcję Utwórz nowe procedury składowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))

Użyj następującej instrukcji `SELECT` dla głównego zapytania TableAdapter s:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Ponieważ to zapytanie nie zawiera żadnych `JOIN` s, Kreator TableAdapter automatycznie utworzy procedury składowane z odpowiednimi `INSERT`, `UPDATE`i `DELETE` instrukcjami, jak również procedurę przechowywaną do wykonywania zapytania głównego.

Poniższy krok pozwala nam nazwać procedury składowane TableAdapter. Użyj nazw `Employees_Select`, `Employees_Insert`, `Employees_Update`i `Employees_Delete`, jak pokazano na rysunku 6.

[Nazwa ![procedury składowane TableAdapter](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Ilustracja 6**. Nazwij procedury składowane TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))

Ostatni krok monituje nas o nazwę metod TableAdapter. Użyj `Fill` i `GetEmployees` jako nazw metod. Pamiętaj również, aby pozostawić zaznaczone pole wyboru Utwórz metody do wysyłania aktualizacji bezpośrednio do bazy danych (GenerateDBDirectMethods).

[![Nazwij metody TableAdapter i GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Rysunek 7**. Nazwij metody TableAdapter s `Fill` i `GetEmployees` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))

Po zakończeniu działania kreatora Poświęć chwilę na przeanalizowanie procedur składowanych w bazie danych. Powinny być widoczne cztery nowe: `Employees_Select`, `Employees_Insert`, `Employees_Update`i `Employees_Delete`. Następnie sprawdź `EmployeesDataTable` i `EmployeesTableAdapter` utworzone. Element DataTable zawiera kolumnę dla każdego pola zwróconego przez zapytanie główne. Kliknij TableAdapter, a następnie przejdź do okno Właściwości. Zobaczysz, że właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand` są poprawnie skonfigurowane do wywoływania odpowiednich procedur składowanych.

[![TableAdapter obejmuje możliwości wstawiania, aktualizowania i usuwania](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Ilustracja 8**. TableAdapter zawiera możliwości INSERT, Update i Delete ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))

Przy użyciu automatycznie utworzonych i `InsertCommand``UpdateCommand`, aktualizacji i właściwości przechowywanych procedury składowane INSERT, Update i `DeleteCommand` są gotowe do dostosowania procedury składowanej `SelectCommand` s w celu zwrócenia dodatkowych informacji na temat każdego z menedżerów pracowników. W szczególnych przypadkach należy zaktualizować `Employees_Select` procedury składowanej, aby użyć `JOIN` i zwrócić wartości `FirstName` i `LastName` Menedżera. Po zaktualizowaniu procedury składowanej należy zaktualizować element DataTable, aby zawierał te dodatkowe kolumny. Te dwa zadania zostaną przedstawione w krokach 2 i 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Krok 2. Dostosowywanie procedury składowanej w celu uwzględnienia`JOIN`

Zacznij od przechodzenia do Eksplorator serwera, przechodzenie do szczegółów folderu procedur składowanych Northwind Database i otwieranie `Employees_Select` procedury składowanej. Jeśli nie widzisz tej procedury składowanej, kliknij prawym przyciskiem myszy folder procedury składowane i wybierz polecenie Odśwież. Zaktualizuj procedurę składowaną, tak aby używała `LEFT JOIN` do zwrócenia najpierw Menedżera i nazwisko:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Po zaktualizowaniu instrukcji `SELECT` Zapisz zmiany, przechodząc do menu plik i wybierając pozycję Zapisz `Employees_Select`. Alternatywnie możesz kliknąć ikonę Zapisz na pasku narzędzi lub nacisnąć klawisze CTRL + S. Po zapisaniu zmian kliknij prawym przyciskiem myszy `Employees_Select` procedury składowanej w Eksplorator serwera i wybierz polecenie Execute (wykonaj). Spowoduje to uruchomienie procedury składowanej i wyświetlenie jej wyników w oknie danych wyjściowych (zobacz rysunek 9).

[![wyniki procedur składowanych są wyświetlane w Okno Dane wyjściowe](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Ilustracja 9**. wyniki procedur składowanych są wyświetlane w okno dane wyjściowe ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))

## <a name="step-3-updating-the-datatable-s-columns"></a>Krok 3. aktualizowanie kolumn DataTable s

W tym momencie `Employees_Select` procedura składowana zwraca `ManagerFirstName` i `ManagerLastName` wartości, ale w `EmployeesDataTable` brakuje tych kolumn. Te brakujące kolumny można dodać do elementu DataTable na jeden z dwóch sposobów:

- **Ręcznie** — kliknij prawym przyciskiem myszy element DataTable w Projektancie obiektów DataSet, a następnie w menu Dodaj wybierz pozycję kolumna. Następnie można nazwać kolumnę i odpowiednio ustawić jej właściwości.
- **Automatycznie** — Kreator konfiguracji TableAdapter zaktualizuje kolumny DataTable s w celu odzwierciedlenia pól zwracanych przez `SelectCommand` procedury składowanej. W przypadku korzystania z instrukcji SQL ad hoc kreator usunie również właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand`, ponieważ `SelectCommand` zawiera teraz `JOIN`. Jednak w przypadku korzystania z procedur składowanych te właściwości poleceń pozostają nienaruszone.

Dodaliśmy ręczne dodawaj kolumny DataTable w poprzednich samouczkach, w tym [wzorzec/szczegóły przy użyciu listy punktowanej rekordów głównych z szczegółami DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) i [przekazywania plików](../working-with-binary-files/uploading-files-vb.md), a my poznamy ten proces ponownie w następnym samouczku. Jednak w tym samouczku użyjemy automatycznej metody za pomocą Kreatora konfiguracji TableAdapter.

Zacznij od kliknięcia prawym przyciskiem myszy `EmployeesTableAdapter` i wybrania opcji Konfiguruj z menu kontekstowego. Spowoduje to wyświetlenie Kreatora konfiguracji TableAdapter, w którym są wyświetlane procedury składowane używane do wybierania, wstawiania, aktualizowania i usuwania oraz ich wartości zwracane i parametry (jeśli istnieją). Ten Kreator zawiera rysunek 10. W tym miejscu możemy zobaczyć, że `Employees_Select` procedura składowana zwróci teraz pola `ManagerFirstName` i `ManagerLastName`.

[![Kreator wyświetla listę zaktualizowanych kolumn dla procedury składowanej Employees_Select](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Ilustracja 10**. Kreator wyświetla zaktualizowaną listę kolumn dla `Employees_Select` procedury składowanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))

Aby zakończyć pracę kreatora, kliknij przycisk Zakończ. Po powrocie do projektanta obiektów DataSet `EmployeesDataTable` obejmuje dwie dodatkowe kolumny: `ManagerFirstName` i `ManagerLastName`.

[![EmployeesDataTable zawiera dwie nowe kolumny](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Ilustracja 11**. `EmployeesDataTable` zawiera dwie nowe kolumny ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))

Aby zilustrować, że uaktualniona `Employees_Select` procedura składowana obowiązuje i że możliwości wstawiania, aktualizowania i usuwania TableAdapter są nadal funkcjonalne, Utwórz stronę sieci Web, która umożliwia użytkownikom wyświetlanie i usuwanie pracowników. Przed utworzeniem takiej strony musimy jednak najpierw utworzyć nową klasę w warstwie logiki biznesowej do pracy z pracownikami z zestawu danych `NorthwindWithSprocs`. W kroku 4 utworzymy klasę `EmployeesBLLWithSprocs`. W kroku 5 będziemy używać tej klasy ze strony ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Krok 4. Implementowanie warstwy logiki biznesowej

Utwórz nowy plik klasy w folderze `~/App_Code/BLL` o nazwie `EmployeesBLLWithSprocs.vb`. Ta klasa naśladuje semantykę istniejącej klasy `EmployeesBLL`, tylko ten nowy z nich zapewnia mniejszą liczbę metod i używa `NorthwindWithSprocs` zestawu danych (zamiast zestawu danych `Northwind`). Dodaj następujący kod do klasy `EmployeesBLLWithSprocs`.

[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

Właściwość `Adapter` `EmployeesBLLWithSprocs` klasy s zwraca wystąpienie `NorthwindWithSprocs` `EmployeesTableAdapter`zestawu danych. Jest on używany przez metody `GetEmployees` i `DeleteEmployee` klasy. Metoda `GetEmployees` wywołuje `EmployeesTableAdapter` `GetEmployees` odpowiadającą metodzie, która wywołuje `Employees_Select` procedurę składowaną i wypełnia wyniki w `EmployeeDataTable`. Metoda `DeleteEmployee` podobnie wywołuje metodę `EmployeesTableAdapter` s `Delete`, która wywołuje `Employees_Delete` procedurę składowaną.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Krok 5. Praca z danymi w warstwie prezentacji

Po zakończeniu `EmployeesBLLWithSprocs`j klasy wszystko gotowe do pracy z danymi pracownika za pomocą strony ASP.NET. Otwórz stronę `JOINs.aspx` w folderze `AdvancedDAL` i przeciągnij widok GridView z przybornika do projektanta, ustawiając jego właściwość `ID` na `Employees`. Następnie w tagu inteligentnym GridView należy powiązać siatkę z nową kontrolką ObjectDataSource o nazwie `EmployeesDataSource`.

Skonfiguruj element ObjectDataSource do używania klasy `EmployeesBLLWithSprocs` i z kart wybierz i Usuń upewnij się, że na listach rozwijanych są wybrane metody `GetEmployees` i `DeleteEmployee`. Kliknij przycisk Zakończ, aby zakończyć konfigurację elementu ObjectDataSource s.

[![skonfigurować element ObjectDataSource do używania klasy EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Ilustracja 12**. Konfigurowanie elementu ObjectDataSource do używania klasy `EmployeesBLLWithSprocs` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))

[![, aby element ObjectDataSource używał metod GetEmployees i DeleteEmployee](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Ilustracja 13**. aby element ObjectDataSource używał metod `GetEmployees` i `DeleteEmployee` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))

Program Visual Studio doda BoundField do widoku GridView dla każdej z kolumn `EmployeesDataTable` s. Usuń wszystkie te BoundFields z wyjątkiem `Title`, `LastName`, `FirstName`, `ManagerFirstName`i `ManagerLastName`, a następnie zmień nazwę `HeaderText` właściwości dla ostatnich czterech BoundFields na nazwisko, imię, nazwisko, imię i nazwisko, a Menedżer s odpowiednio nazwisko.

Aby umożliwić użytkownikom usuwanie pracowników z tej strony, musimy wykonać dwie rzeczy. Najpierw poinstruuj widok GridView, aby udostępnić możliwości usuwania, sprawdzając opcję Włącz usuwanie z jej tagu inteligentnego. Następnie zmień właściwość `OldValuesParameterFormatString` ObjectDataSource s z wartości ustawionej przez kreatora ObjectDataSource (`original_{0}`) na wartość domyślną (`{0}`). Po wprowadzeniu tych zmian, znaczniki deklaratywne GridView i ObjectDataSource s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Przetestuj stronę, odwiedzając ją za pomocą przeglądarki. Jak pokazano na rysunku 14, na stronie zostanie wyświetlona lista każdego pracownika i jego nazwa kierownika (przy założeniu, że ma ona jeden).

[![SPRZĘŻENIa w procedurze składowanej Employees_Select zwraca nazwę Menedżera](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Ilustracja 14**. `JOIN` w procedurze składowanej `Employees_Select` zwraca nazwę Menedżera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))

Kliknięcie przycisku Usuń powoduje uruchomienie usuwania przepływu pracy, który culminates wykonywanie procedury składowanej `Employees_Delete`. Jednak próba `DELETE` instrukcji w procedurze składowanej kończy się niepowodzeniem z powodu naruszenia ograniczenia klucza obcego (patrz rysunek 15). W każdym przypadku każdy pracownik ma jeden lub więcej rekordów w tabeli `Orders`, co powoduje niepowodzenie usuwania.

[![usunięcie pracownika, który ma odpowiednie zamówienia, spowoduje naruszenie ograniczenia klucza obcego](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Ilustracja 15**. Usuwanie pracownika z odpowiednimi zamówieniami skutkuje naruszeniem ograniczenia klucza obcego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))

Aby zezwolić na usunięcie pracownika, możesz:

- Zaktualizuj ograniczenie klucza obcego do usuwania kaskadowego,
- Ręcznie usuń rekordy z tabeli `Orders` dla pracowników, które chcesz usunąć.
- Zaktualizuj procedurę składowaną `Employees_Delete`, aby najpierw usunąć powiązane rekordy z tabeli `Orders` przed usunięciem rekordu `Employees`. Omawiana technika została omówiona w [istniejących procedurach składowanych w samouczku TableAdapters zestawu danych](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

Jestem tym ćwiczeniem dla czytnika.

## <a name="summary"></a>Podsumowanie

Podczas pracy z relacyjnymi bazami danych często zapytania pobierają dane z wielu powiązanych tabel. Skorelowane podzapytania i `JOIN` s zapewniają dwie różne techniki uzyskiwania dostępu do danych z powiązanych tabel w zapytaniu. W poprzednich samouczkach najczęściej używano skorelowanych podkwerend, ponieważ TableAdapter nie może automatycznie generować instrukcji `INSERT`, `UPDATE`i `DELETE` dla zapytań obejmujących `JOIN` s. Te wartości można podać ręcznie, podczas korzystania z instrukcji SQL ad hoc wszystkie dostosowania zostaną nadpisywane po zakończeniu działania Kreatora konfiguracji TableAdapter.

Na szczęście TableAdapters utworzone przy użyciu procedur składowanych nie pogorszą się od tych samych brittleness, co te utworzone przy użyciu instrukcji SQL ad hoc. W związku z tym jest możliwe utworzenie elementu TableAdapter, którego główne zapytanie używa `JOIN` podczas korzystania z procedur składowanych. W tym samouczku przedstawiono sposób tworzenia takich TableAdapter. Uruchomiono za pomocą zapytania `SELECT` `JOIN`-less dla zapytania głównego TableAdapter s, aby odpowiednie procedury składowane INSERT, Update i DELETE byłyby tworzone na podstawie autostartu. Po zakończeniu konfiguracji początkowej TableAdapter s podaliśmy procedurę składowaną `SelectCommand`, aby użyć `JOIN` i ponownie uruchomiła Kreatora konfiguracji TableAdapter w celu zaktualizowania kolumn `EmployeesDataTable` s.

Ponowne uruchomienie Kreatora konfiguracji programu TableAdapter automatycznie zaktualizowała kolumny `EmployeesDataTable`, aby odzwierciedlały pola danych zwracane przez `Employees_Select` procedurę składowaną. Alternatywnie, możemy dodać te kolumny ręcznie do elementu DataTable. Będziemy eksplorować Ręczne dodawanie kolumn do tabeli DataTable w następnym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Hilton Geisenow, David suru i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [dalej](adding-additional-datatable-columns-vb.md)
