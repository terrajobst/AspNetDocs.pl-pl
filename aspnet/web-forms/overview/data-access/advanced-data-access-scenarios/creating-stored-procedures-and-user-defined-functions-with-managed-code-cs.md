---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Tworzenie procedur składowanych i funkcji zdefiniowanych przez użytkownika z kodem zarządzanym (C#) | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 integruje się z środowiskiem uruchomieniowym języka wspólnego .NET, aby umożliwić deweloperom tworzenie obiektów bazy danych za pomocą kodu zarządzanego. Ten samouczek...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: c6aec9ca70fe3ab568b3d17fea6bfd56671edc03
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78533538"
---
# <a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Tworzenie procedur składowanych i funkcji zdefiniowanych przez użytkownika z kodem zarządzanym (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) lub [Pobierz plik PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 integruje się z środowiskiem uruchomieniowym języka wspólnego .NET, aby umożliwić deweloperom tworzenie obiektów bazy danych za pomocą kodu zarządzanego. W tym samouczku pokazano, jak utworzyć zarządzane procedury składowane i zarządzane funkcje zdefiniowane przez użytkownika przy C# użyciu Visual Basic lub kodu. Zobaczymy również, jak te wersje programu Visual Studio umożliwiają debugowanie takich obiektów zarządzanych baz danych.

## <a name="introduction"></a>Wprowadzenie

Bazy danych, takie jak Microsoft s SQL Server 2005, wykorzystują [Transact-Structured Query Language (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) do wstawiania, modyfikowania i pobierania danych. Większość systemów baz danych zawiera konstrukcje do grupowania serii instrukcji SQL, które można następnie wykonać jako pojedynczą jednostkę wielokrotnego użytku. Procedury składowane są jednym z przykładów. Inna to *funkcja zdefiniowana przez użytkownika*(UDF), konstrukcja, która dokładniej sprawdzi się w kroku 9.

Na jego rdzeń program SQL został zaprojektowany do pracy z zestawami danych. Instrukcje `SELECT`, `UPDATE`i `DELETE` dotyczą wszystkich rekordów w odpowiedniej tabeli i są ograniczone tylko przez ich klauzule `WHERE`. Istnieje jednak wiele funkcji języka zaprojektowanych do pracy z jednym rekordem jednocześnie i do manipulowania danymi skalarnymi. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) pozwala na zapętlenie zestawu rekordów w czasie. Funkcje manipulowania ciągami, takie jak `LEFT`, `CHARINDEX`i `PATINDEX` pracy z danymi skalarnymi. SQL zawiera również instrukcje przepływu sterowania, takie jak `IF` i `WHILE`.

Przed Microsoft SQL Server 2005, procedury składowane i UDF mogą być zdefiniowane tylko jako kolekcja instrukcji T-SQL. SQL Server 2005, jednak został zaprojektowany w celu zapewnienia integracji ze [środowiskiem uruchomieniowym języka wspólnego (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), który jest używany przez wszystkie zestawy .NET. W związku z tym procedury składowane i UDF w bazie danych SQL Server 2005 można utworzyć za pomocą kodu zarządzanego. Oznacza to, że można utworzyć procedurę składowaną lub UDF jako metodę w C# klasie. Dzięki temu te procedury składowane i UDF mogą korzystać z funkcji w .NET Framework i z własnych klas niestandardowych.

W tym samouczku sprawdzimy, jak tworzyć zarządzane procedury składowane i funkcje zdefiniowane przez użytkownika oraz jak integrować je z naszą bazą danych Northwind. Zacznij korzystać z aplikacji.

> [!NOTE]
> Obiekty zarządzanej bazy danych oferują pewne korzyści w porównaniu z ich odpowiednikami SQL. Najważniejsze zalety to zaawansowanie i znajomość języka oraz możliwość ponownego użycia istniejącego kodu i logiki. Mimo że obiekty zarządzanej bazy danych mogą być mniej wydajne podczas pracy z zestawami danych, które nie obejmują logiki proceduralnej. Dokładniejsze Omówienie zalet korzystania z kodu zarządzanego i języka T-SQL można znaleźć w sekcji [zalety korzystania z kodu zarządzanego do tworzenia obiektów bazy danych](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).

## <a name="step-1-moving-the-northwind-database-out-ofapp_data"></a>Krok 1. przeniesienie bazy danych Northwind z`App_Data`

We wszystkich naszych samouczkach wykorzystano plik bazy danych Microsoft SQL Server 2005 Express Edition w folderze `App_Data` aplikacji sieci Web. Umieszczenie bazy danych w `App_Data` uproszczone dystrybuowanie i uruchamianie tych samouczków, ponieważ wszystkie pliki znajdują się w jednym katalogu i nie wymagały dodatkowych kroków konfiguracyjnych do przetestowania samouczka.

Na potrzeby tego samouczka należy jednak przenieść bazę danych Northwind do `App_Data` i jawnie zarejestrować ją w wystąpieniu bazy danych SQL Server 2005 Express Edition. Chociaż możemy wykonać kroki tego samouczka z bazą danych w folderze `App_Data`, wiele kroków jest znacznie prostsze przez jawne zarejestrowanie bazy danych z wystąpieniem bazy danych SQL Server 2005 Express Edition.

Do pobrania w tym samouczku znajdują się dwa pliki bazy danych — `NORTHWND.MDF` i `NORTHWND_log.LDF` — umieszczane w folderze o nazwie `DataFiles`. Jeśli korzystasz z własnych implementacji samouczków, Zamknij program Visual Studio i Przenieś `NORTHWND.MDF` i `NORTHWND_log.LDF` plików z folderu witryny sieci Web `App_Data` do folderu poza witryną sieci Web. Po przeniesieniu plików bazy danych do innego folderu musimy zarejestrować bazę danych Northwind z wystąpieniem bazy danych SQL Server 2005 Express Edition. Można to zrobić z poziomu SQL Server Management Studio. Jeśli na komputerze zainstalowano wersję nierolniczą SQL Server 2005, na pewno zainstalowano już Management Studio. Jeśli masz tylko SQL Server 2005 Express Edition na komputerze, poświęć chwilę na pobranie i zainstalowanie [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Uruchom SQL Server Management Studio. Jak pokazano na rysunku 1, Management Studio rozpocznie się z pytaniem o serwer, z którym ma zostać nawiązane połączenie. Wprowadź localhost\SQLExpress w polu Nazwa serwera, wybierz pozycję Uwierzytelnianie systemu Windows na liście rozwijanej uwierzytelnianie, a następnie kliknij przycisk Połącz.

![Nawiązywanie połączenia z odpowiednim wystąpieniem bazy danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Rysunek 1**. Nawiązywanie połączenia z odpowiednim wystąpieniem bazy danych

Po zakończeniu połączenia w oknie Eksplorator obiektów zostaną wystawione informacje dotyczące wystąpienia bazy danych SQL Server 2005 Express Edition, w tym jego baz danych, informacje o zabezpieczeniach, opcje zarządzania itd.

Musimy dołączyć bazę danych Northwind do folderu `DataFiles` (lub wszędzie tam, gdzie można je przenieść) do wystąpienia bazy danych SQL Server 2005 Express Edition. Kliknij prawym przyciskiem myszy folder Databases, a następnie wybierz opcję Dołącz z menu kontekstowego. Spowoduje to wyświetlenie okna dialogowego dołączanie baz danych. Kliknij przycisk Dodaj, przejdź do odpowiedniego pliku `NORTHWND.MDF` i kliknij przycisk OK. W tym momencie ekran powinien wyglądać podobnie do rysunku 2.

[![połączyć się z odpowiednim wystąpieniem bazy danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**Rysunek 2**. Nawiązywanie połączenia z odpowiednim wystąpieniem bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))

> [!NOTE]
> Podczas nawiązywania połączenia z wystąpieniem SQL Server 2005 Express Edition za pomocą Management Studio okno dialogowe Dołączanie baz danych nie pozwala na przechodzenie do szczegółów katalogów profilów użytkowników, takich jak Moje dokumenty. W związku z tym upewnij się, że pliki `NORTHWND.MDF` i `NORTHWND_log.LDF` są umieszczane w katalogu profilów nienależących do użytkownika.

Kliknij przycisk OK, aby dołączyć bazę danych. Zostanie zamknięte okno dialogowe Dołączanie baz danych, a Eksplorator obiektów powinna teraz wyświetlić listę bezpośrednio dołączonej bazy danych. Prawdopodobnie baza danych Northwind ma nazwę, taką jak `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Zmień nazwę bazy danych na Northwind, klikając ją prawym przyciskiem myszy i wybierając polecenie Zmień nazwę.

![Zmień nazwę bazy danych na Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Rysunek 3**. zmiana nazwy bazy danych na Northwind

## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Krok 2. Tworzenie nowego rozwiązania i projektu SQL Server w programie Visual Studio

Aby utworzyć zarządzane procedury składowane lub UDF w SQL Server 2005, należy napisać procedurę składowaną i logikę C# UDF jako kod w klasie. Po zapisaniu kodu będziemy musieli skompilować tę klasę do zestawu (pliku `.dll`), zarejestrować zestaw z bazą danych SQL Server, a następnie utworzyć procedurę składowaną lub obiekt UDF w bazie danych, która wskazuje odpowiednią metodę w zestawie. Wszystkie te kroki można wykonać ręcznie. Możemy utworzyć kod w dowolnym edytorze tekstów, skompilować go z wiersza polecenia przy użyciu C# kompilatora ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), zarejestrować go w bazie danych przy użyciu polecenia [`CREATE ASSEMBLY`](https://msdn.microsoft.com/library/ms189524.aspx) lub z Management Studio, a następnie dodać procedurę składowaną lub metodę UDF przy użyciu podobnych metod. Na szczęście wersje Professional i Team Systems programu Visual Studio zawierają SQL Server typ projektu, który automatyzuje te zadania. W tym samouczku opisano, jak za pomocą typu projektu SQL Server utworzyć zarządzaną procedurę składowaną i funkcję UDF.

> [!NOTE]
> W przypadku korzystania z programu Visual Web Developer lub Standard Edition programu Visual Studio należy zamiast tego użyć podejścia ręcznego. Krok 13 zawiera szczegółowe instrukcje dotyczące ręcznego wykonywania tych czynności. Zachęcam do odczytu kroków od 2 do 12 przed przeczytaniem kroku 13, ponieważ te kroki zawierają ważne instrukcje dotyczące konfiguracji SQL Server, które należy zastosować niezależnie od używanej wersji programu Visual Studio.

Zacznij od otwarcia programu Visual Studio. Z menu plik wybierz polecenie Nowy projekt, aby wyświetlić okno dialogowe Nowy projekt (zobacz rysunek 4). Przejdź do szczegółów typu projektu bazy danych, a następnie z szablonów wymienionych po prawej stronie wybierz opcję utworzenia nowego projektu SQL Server. Wybrano opcję nazywania tego projektu `ManagedDatabaseConstructs` i umieszczenia go w rozwiązaniu o nazwie `Tutorial75`.

[![utworzyć nowy projekt SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Ilustracja 4**. Tworzenie nowego projektu SQL Server ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))

Kliknij przycisk OK w oknie dialogowym Nowy projekt, aby utworzyć rozwiązanie i projekt SQL Server.

Projekt SQL Server jest powiązany z określoną bazą danych. W związku z tym po utworzeniu nowego projektu SQL Server natychmiast poprosimy o podanie tych informacji. Rysunek 5 zawiera okno dialogowe nowe odwołanie do bazy danych, które zostało wypełnione, aby wskazywało bazę danych Northwind zarejestrowaną w wystąpieniu bazy danych SQL Server 2005 Express Edition z powrotem w kroku 1.

![Skojarz projekt SQL Server z bazą danych Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Rysunek 5**. kojarzenie projektu SQL Server z bazą danych Northwind

Aby debugować zarządzane procedury składowane i UDF, które zostaną utworzone w ramach tego projektu, musimy włączyć obsługę debugowania SQL/CLR dla tego połączenia. Za każdym razem, gdy kojarzy projekt SQL Server z nową bazą danych (jak pokazano na rysunku 5), program Visual Studio monituje nas, jeśli chcemy włączyć debugowanie SQL/CLR dla połączenia (zobacz rysunek 6). kliknij przycisk Tak.

![Włącz debugowanie SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Ilustracja 6**. Włączanie debugowania SQL/CLR

W tym momencie nowy projekt SQL Server został dodany do rozwiązania. Zawiera folder o nazwie `Test Scripts` z plikiem o nazwie `Test.sql`, który jest używany do debugowania obiektów zarządzanej bazy danych utworzonych w projekcie. Zobaczmy debugowanie w kroku 12.

Teraz możemy dodawać nowe zarządzane procedury składowane i UDF do tego projektu, ale zanim wyślemy do niej ofertę istniejącej aplikacji sieci Web w rozwiązaniu. Z menu plik wybierz opcję Dodaj, a następnie wybierz istniejącą witrynę sieci Web. Przejdź do odpowiedniego folderu witryny sieci Web i kliknij przycisk OK. Jak pokazano na rysunku 7, spowoduje to zaktualizowanie rozwiązania w taki sposób, aby obejmowało dwa projekty: witrynę internetową i SQL Server projekt `ManagedDatabaseConstructs`.

![Eksplorator rozwiązań obejmuje teraz dwa projekty](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Rysunek 7**. Eksplorator rozwiązań obejmuje teraz dwa projekty

Wartość `NORTHWNDConnectionString` w `Web.config` obecnie odwołuje się do `NORTHWND.MDF` pliku w folderze `App_Data`. Ponieważ ta baza danych została usunięta z `App_Data` i jawnie zarejestrowana w wystąpieniu bazy danych SQL Server 2005 Express Edition, musimy odpowiednio zaktualizować wartość `NORTHWNDConnectionString`. Otwórz plik `Web.config` w witrynie sieci Web i zmień wartość `NORTHWNDConnectionString`, tak aby parametry połączenia były odczytywane: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Po tej zmianie sekcja `<connectionStrings>` w `Web.config` powinna wyglądać podobnie do następującej:

[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Zgodnie z opisem w [poprzednim samouczku](debugging-stored-procedures-cs.md)podczas debugowania obiektu SQL Server z aplikacji klienckiej, takiej jak witryna sieci Web ASP.NET, musimy wyłączyć buforowanie połączeń. Pokazane powyżej parametry połączenia powodują wyłączenie puli połączeń (`Pooling=false`). Jeśli nie planujesz debugowania zarządzanymi procedurami składowanymi i UDF z witryny sieci Web ASP.NET, Włącz buforowanie połączeń.

## <a name="step-3-creating-a-managed-stored-procedure"></a>Krok 3. Tworzenie zarządzanej procedury składowanej

Aby dodać zarządzaną procedurę składowaną do bazy danych Northwind, najpierw musimy utworzyć procedurę przechowywaną jako metodę w projekcie SQL Server. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu `ManagedDatabaseConstructs`, a następnie wybierz polecenie Dodaj nowy element. Spowoduje to wyświetlenie okna dialogowego Dodawanie nowego elementu zawierającego listę typów obiektów zarządzanych baz danych, które można dodać do projektu. Jak pokazano na rysunku 8, obejmuje procedury składowane i funkcje zdefiniowane przez użytkownika, między innymi.

Zacznij od dodania procedury składowanej, która po prostu zwraca wszystkie produkty, które zostały wycofane. Nazwij nowy plik procedury składowanej `GetDiscontinuedProducts.cs`.

[![dodać nową procedurę składowaną o nazwie GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Ilustracja 8**. Dodawanie nowej procedury składowanej o nazwie `GetDiscontinuedProducts.cs` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))

Spowoduje to utworzenie nowego C# pliku klasy z następującą zawartością:

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Należy zauważyć, że procedura składowana jest zaimplementowana jako metoda `static` w pliku klasy `partial` o nazwie `StoredProcedures`. Ponadto Metoda `GetDiscontinuedProducts` ma `SqlProcedure attribute`, która oznacza metodę jako procedurę składowaną.

Poniższy kod tworzy obiekt `SqlCommand` i ustawia jego `CommandText` na kwerendę `SELECT`, która zwraca wszystkie kolumny z tabeli `Products` dla produktów, których `Discontinued` pole ma wartość 1. Następnie wykonuje polecenie i wysyła wyniki z powrotem do aplikacji klienckiej. Dodaj ten kod do metody `GetDiscontinuedProducts`.

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Wszystkie obiekty zarządzanej bazy danych mają dostęp do [obiektu`SqlContext`](https://msdn.microsoft.com/library/ms131108.aspx) , który reprezentuje kontekst obiektu wywołującego. `SqlContext` zapewnia dostęp do [obiektu`SqlPipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) za pośrednictwem jego [Właściwości`Pipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Ten obiekt `SqlPipe` służy do promowania informacji między bazą danych SQL Server i aplikacją wywołującą. Jak nazywa się to, [metoda`ExecuteAndSend`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) wykonuje przekazaną `SqlCommand` obiektu i wysyła wyniki z powrotem do aplikacji klienckiej.

> [!NOTE]
> Obiekty zarządzanej bazy danych najlepiej nadają się do procedur składowanych i UDF, które używają logiki proceduralnej, a nie logiki opartej na zestawie. Logika proceduralna obejmuje pracę z zestawami danych w poszczególnych wierszach lub w pracy z danymi skalarnymi. Metoda `GetDiscontinuedProducts`, która właśnie została utworzona, nie obejmuje jednak logiki proceduralnej. W związku z tym najlepszym rozwiązaniem jest wdrożenie procedury składowanej T-SQL. Jest ona zaimplementowana jako zarządzana procedura składowana w celu przedstawienia kroków niezbędnych do utworzenia i wdrożenia zarządzanych procedur składowanych.

## <a name="step-4-deploying-the-managed-stored-procedure"></a>Krok 4. wdrażanie zarządzanej procedury składowanej

Po ukończeniu tego kodu jesteśmy gotowi do wdrożenia go w bazie danych Northwind. Wdrożenie projektu SQL Server kompiluje kod w zestawie, rejestruje zestaw z bazą danych i tworzy odpowiednie obiekty w bazie danych, łącząc je z odpowiednimi metodami w zestawie. Dokładny zestaw zadań wykonywanych przez opcję Wdróż jest bardziej precyzyjnie wpisany w kroku 13. Kliknij prawym przyciskiem myszy nazwę projektu `ManagedDatabaseConstructs` w Eksplorator rozwiązań i wybierz opcję Wdróż. Jednak wdrożenie nie powiedzie się z powodu następującego błędu: nieprawidłowa składnia w sąsiedztwie "EXTERNAL". Może być konieczne ustawienie poziomu zgodności bieżącej bazy danych na wyższą wartość, aby włączyć tę funkcję. Zapoznaj się z pomocą techniczną `sp_dbcmptlevel`procedury przechowywanej.

Ten komunikat o błędzie występuje podczas próby zarejestrowania zestawu przy użyciu bazy danych Northwind. Aby zarejestrować zestaw z bazą danych SQL Server 2005, należy ustawić poziom zgodności bazy danych na 90. Domyślnie nowe bazy danych SQL Server 2005 mają poziom zgodności równy 90. Jednak bazy danych utworzone przy użyciu Microsoft SQL Server 2000 mają domyślny poziom zgodności równy 80. Ponieważ baza danych Northwind była początkowo bazą danych Microsoft SQL Server 2000, jej poziom zgodności jest obecnie ustawiony na 80 i dlatego należy go zwiększyć do 90 w celu zarejestrowania obiektów zarządzanych baz danych.

Aby zaktualizować poziom zgodności bazy danych, Otwórz nowe okno zapytania w Management Studio i wprowadź:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Kliknij ikonę Execute (wykonaj) na pasku narzędzi, aby uruchomić powyższe zapytanie.

[![zaktualizować poziomu zgodności bazy danych Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Ilustracja 9**. aktualizowanie poziomu zgodności bazy danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))

Po zaktualizowaniu poziomu zgodności, wdróż ponownie projekt SQL Server. Ta godzina wdrożenia powinna zakończyć się bez błędu.

Wróć do SQL Server Management Studio, kliknij prawym przyciskiem myszy bazę danych Northwind w Eksplorator obiektów, a następnie wybierz polecenie Odśwież. Następnie przejdź do szczegółów folderu programowanie, a następnie rozwiń folder zestawy. Jak pokazano na rysunku 10, baza danych Northwind zawiera teraz zestaw wygenerowany przez projekt `ManagedDatabaseConstructs`.

![Zestaw ManagedDatabaseConstructs jest teraz zarejestrowany w bazie danych Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Ilustracja 10**. zestaw `ManagedDatabaseConstructs` jest teraz zarejestrowany w bazie danych Northwind

Rozwiń również folder procedury składowane. Zostanie wyświetlona procedura składowana o nazwie `GetDiscontinuedProducts`. Ta procedura składowana została utworzona przez proces wdrażania i wskazuje na metodę `GetDiscontinuedProducts` w zestawie `ManagedDatabaseConstructs`. W przypadku wykonywania procedury składowanej `GetDiscontinuedProducts`, z kolei wykonuje metodę `GetDiscontinuedProducts`. Ponieważ jest to zarządzana procedura składowana, nie można jej edytować za pomocą Management Studio (w związku z tym ikona kłódki obok nazwy procedury składowanej).

![Procedura składowana GetDiscontinuedProducts jest wymieniona w folderze procedury składowane](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Ilustracja 11**. procedura składowana `GetDiscontinuedProducts` jest wymieniona w folderze procedury składowane

Nadal trzeba przezwyciężyć, zanim będziemy mogli wywoływać zarządzane procedury składowane: baza danych jest skonfigurowana tak, aby uniemożliwiać wykonywanie kodu zarządzanego. Sprawdź to, otwierając nowe okno zapytania i wykonując `GetDiscontinuedProducts` procedury składowanej. Zostanie wyświetlony następujący komunikat o błędzie: wykonywanie kodu użytkownika w .NET Framework jest wyłączone. Włącz opcję konfiguracji z włączoną funkcją CLR.

Aby zbadać informacje o konfiguracji bazy danych Northwind, wprowadź i wykonaj polecenie `exec sp_configure` w oknie zapytania. Wskazuje to, że ustawienie CLR enabled ma obecnie wartość 0.

[![ustawienie Enabled CLR ma obecnie wartość 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Rysunek 12**: ustawienie Enabled CLR ma obecnie wartość 0 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))

Należy zauważyć, że każde ustawienie konfiguracji na rysunku 12 ma cztery wartości na liście: wartości minimalne i maksymalne oraz wartości konfiguracyjne i uruchomienia. Aby zaktualizować wartość konfiguracji dla ustawienia Enabled CLR, wykonaj następujące polecenie:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

Po ponownym uruchomieniu `exec sp_configure` zostanie wyświetlona informacja, że powyższa instrukcja zaktualizował wartość konfiguracyjną ustawienia Enabled CLR na 1, ale wartość Run jest nadal ustawiona na 0. Aby ta zmiana konfiguracji zaczęła mieć wpływ, musimy wykonać [`RECONFIGURE` polecenie](https://msdn.microsoft.com/library/ms176069.aspx), które ustawi wartość Run na bieżącą wartość konfiguracji. Po prostu wprowadź `RECONFIGURE` w oknie zapytania, a następnie kliknij ikonę Execute (wykonaj) na pasku narzędzi. W przypadku uruchomienia `exec sp_configure` teraz powinna zostać wyświetlona wartość 1 dla ustawienia konfiguracja i uruchomienia środowiska CLR.

Po zakończeniu konfiguracji z włączoną obsługą środowiska CLR jesteśmy gotowi do uruchomienia zarządzanej `GetDiscontinuedProducts` procedury składowanej. W oknie zapytania wprowadź i wykonaj polecenie `exec` `GetDiscontinuedProducts`. Wywołanie procedury składowanej powoduje, że odpowiedni zarządzany kod w metodzie `GetDiscontinuedProducts` będzie wykonywany. Ten kod generuje zapytanie `SELECT` zwracające wszystkie produkty, które są wycofane i zwraca te dane do aplikacji wywołującej, która jest SQL Server Management Studio w tym wystąpieniu. Management Studio otrzymuje te wyniki i wyświetla je w oknie wyników.

[![procedura składowana GetDiscontinuedProducts zwraca wszystkie wycofane produkty](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Ilustracja 13**. procedura składowana `GetDiscontinuedProducts` zwraca wszystkie wycofane produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))

## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Krok 5. Tworzenie zarządzanych procedur składowanych akceptujących parametry wejściowe

Wiele zapytań i procedur składowanych utworzonych w ramach tych samouczków ma używane *Parametry*. Na przykład w przypadku [tworzenia nowych procedur składowanych dla wpisanych zestawów danych TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczek został utworzony w procedurze składowanej o nazwie `GetProductsByCategoryID`, która zaakceptowała parametr wejściowy o nazwie `@CategoryID`. Procedura składowana zwraca wszystkie produkty, których `CategoryID` pole pasuje do wartości podanego `@CategoryID` parametru.

Aby utworzyć zarządzaną procedurę składowaną, która akceptuje parametry wejściowe, wystarczy określić te parametry w definicji metody s. Aby to zilustrować, należy dodać kolejną zarządzaną procedurę składowaną do projektu `ManagedDatabaseConstructs` o nazwie `GetProductsWithPriceLessThan`. Ta zarządzana procedura składowana przyjmie parametr wejściowy określający cenę i zwróci wszystkie produkty, których `UnitPrice` pole jest mniejsze niż wartość parametru s.

Aby dodać nową procedurę składowaną do projektu, kliknij prawym przyciskiem myszy nazwę projektu `ManagedDatabaseConstructs` i wybierz opcję dodania nowej procedury składowanej. Nazwij plik `GetProductsWithPriceLessThan.cs`. Jak opisano w kroku 3, spowoduje to utworzenie nowego C# pliku klasy z metodą o nazwie `GetProductsWithPriceLessThan` umieszczoną w klasie `partial` `StoredProcedures`.

Zaktualizuj definicję `GetProductsWithPriceLessThan` metody s tak, aby akceptowała [`SqlMoney`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) parametr wejściowy o nazwie `price` i napisać kod do wykonania i zwrócić wyniki zapytania:

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan` i definicja metody s ściśle przypominają definicję i kod metody `GetDiscontinuedProducts` utworzonej w kroku 3. Jedyne różnice polegają na tym, że metoda `GetProductsWithPriceLessThan` akceptuje jako parametr wejściowy (`price`), zapytanie `SqlCommand` s zawiera parametr (`@MaxPrice`), a parametr zostanie dodany do kolekcji `SqlCommand` s `Parameters` jest i przypisana wartość zmiennej `price`.

Po dodaniu tego kodu ponownie Wdróż projekt SQL Server. Następnie wróć do SQL Server Management Studio i Odśwież folder procedury składowane. Powinien zostać wyświetlony nowy wpis `GetProductsWithPriceLessThan`. W oknie zapytania wprowadź i wykonaj polecenie `exec GetProductsWithPriceLessThan 25`, w którym zostaną wyświetlone wszystkie produkty mniejsze niż $25, jak pokazano na rysunku 14.

[Wyświetlane są ![produkty poniżej $25](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Ilustracja 14**. wyświetlane są produkty poniżej $25 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))

## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Krok 6. wywoływanie zarządzanej procedury składowanej z warstwy dostępu do danych

W tym momencie dodaliśmy `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzane procedury składowane do projektu `ManagedDatabaseConstructs` i zarejestrowali je za pomocą bazy danych Northwind SQL Server Database. Te zarządzane procedury składowane są również wywoływane z SQL Server Management Studio (zobacz rysunek s 13 i 14). Aby nasze aplikacje ASP.NET korzystały z tych zarządzanych procedur składowanych, należy jednak dodać je do warstw dostępu do danych i logiki biznesowej w architekturze. W tym kroku dodamy dwie nowe metody do `ProductsTableAdapter` w określonym `NorthwindWithSprocs` zestawie danych, który początkowo został utworzony w [nowych procedurach składowanych dla wpisanych TableAdapters zestawu danych](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) . W kroku 7 dodamy odpowiednie metody do LOGIKI biznesowej.

Otwórz zestaw danych z `NorthwindWithSprocs` określonym w programie Visual Studio i zacznij od dodania nowej metody do `ProductsTableAdapter` o nazwie `GetDiscontinuedProducts`. Aby dodać nową metodę do TableAdapter, kliknij prawym przyciskiem myszy nazwę TableAdapter s w Projektancie i wybierz opcję Dodaj zapytanie z menu kontekstowego.

> [!NOTE]
> Ponieważ baza danych Northwind została przeniesiona z folderu `App_Data` do wystąpienia bazy danych SQL Server 2005 Express Edition, należy koniecznie zaktualizować odpowiednie parametry połączenia w pliku Web. config w celu odzwierciedlenia tej zmiany. W kroku 2 omówiono aktualizowanie wartości `NORTHWNDConnectionString` w `Web.config`. Jeśli nie pamiętasz tej aktualizacji, zostanie wyświetlony komunikat o błędzie nie można dodać zapytania. Nie można odnaleźć `NORTHWNDConnectionString` połączenia dla obiektu `Web.config` w oknie dialogowym podczas próby dodania nowej metody do TableAdapter. Aby rozwiązać ten problem, kliknij przycisk OK, a następnie przejdź do `Web.config` i zaktualizuj wartość `NORTHWNDConnectionString` zgodnie z opisem w sekcji Krok 2. Następnie spróbuj ponownie dodać metodę do TableAdapter. Tym razem należy to zrobić bez błędu.

Dodanie nowej metody powoduje uruchomienie Kreatora konfiguracji zapytania TableAdapter, którego użyto wielokrotnie w poprzednich samouczkach. Pierwszy krok prosi nas o określenie, w jaki sposób element TableAdapter powinien uzyskiwać dostęp do bazy danych: za pośrednictwem instrukcji SQL ad hoc lub za pośrednictwem nowej lub istniejącej procedury składowanej. Ponieważ już utworzono i zarejestrowano `GetDiscontinuedProducts` zarządzaną procedurę przechowywaną w bazie danych, wybierz opcję Użyj istniejącej procedury składowanej, a następnie kliknij przycisk Dalej.

[![wybrać opcję Użyj istniejącej procedury składowanej](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Ilustracja 15**. Wybierz opcję Użyj istniejącej procedury składowanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))

Na następnym ekranie zostanie wyświetlony komunikat z prośbą o procedurę składowaną, która zostanie wywołana przez metodę. Wybierz z listy rozwijanej `GetDiscontinuedProducts` zarządzaną procedurę składowaną, a następnie kliknij przycisk Dalej.

[![wybrać zarządzaną procedurę składowaną GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**Rysunek 16**. Wybierz zarządzaną `GetDiscontinuedProducts` procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))

Zostanie wyświetlony monit o określenie, czy procedura składowana zwraca wiersze, pojedynczą wartość, czy nic. Ponieważ `GetDiscontinuedProducts` zwraca zestaw wycofanych wierszy produktu, wybierz pierwszą opcję (dane tabelaryczne), a następnie kliknij przycisk Dalej.

[![wybrać opcji dane tabelaryczne](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Ilustracja 17**. Wybierz opcję danych tabelarycznych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))

Końcowy ekran kreatora pozwala nam określić używane wzorce dostępu do danych oraz nazwy metod wynikowych. Pozostaw zaznaczone pola wyboru i nazwij metody `FillByDiscontinued` i `GetDiscontinuedProducts`. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![nazwy metod FillByDiscontinued i GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**Ilustracja 18**. nazwij metody `FillByDiscontinued` i `GetDiscontinuedProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))

Powtórz te kroki, aby utworzyć metody o nazwie `FillByPriceLessThan` i `GetProductsWithPriceLessThan` w `ProductsTableAdapter` dla procedury składowanej `GetProductsWithPriceLessThan` Managed.

Rysunek 19 przedstawia zrzut ekranu projektanta obiektów DataSet po dodaniu metod do `ProductsTableAdapter` dla `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzanych procedur składowanych.

[![ProductsTableAdapter zawiera nowe metody dodane w tym kroku](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**Ilustracja 19**. `ProductsTableAdapter` obejmuje nowe metody dodane w tym kroku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))

## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Krok 7. Dodawanie odpowiednich metod do warstwy logiki biznesowej

Po zaktualizowaniu warstwy dostępu do danych w celu uwzględnienia metod wywoływania zarządzanych procedur składowanych, które zostały dodane w krokach 4 i 5, musimy dodać odpowiednie metody do warstwy logiki biznesowej. Dodaj następujące dwie metody do klasy `ProductsBLLWithSprocs`:

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Obie metody po prostu wywołują odpowiednie metody DAL i zwracają wystąpienie `ProductsDataTable`. Znaczniki `DataObjectMethodAttribute` powyżej każda metoda powoduje, że te metody zostaną uwzględnione na liście rozwijanej na karcie Wybierz w Kreatorze konfiguracji źródła danych programu ObjectDataSource s.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Krok 8: wywoływanie zarządzanych procedur składowanych z warstwy prezentacji

Dzięki warstwom logiki biznesowej i dostępu do danych dowiesz się, jak włączyć obsługę wywoływania `GetDiscontinuedProducts` i `GetProductsWithPriceLessThan` zarządzane procedury składowane, teraz można wyświetlić te procedury składowane za pomocą strony ASP.NET.

Otwórz stronę `ManagedFunctionsAndSprocs.aspx` w folderze `AdvancedDAL` i z przybornika przeciągnij widok GridView do projektanta. Ustaw właściwość GridView s `ID` na `DiscontinuedProducts` i, z jej tagu inteligentnego, powiąż ją z nowym elementem ObjectDataSource o nazwie `DiscontinuedProductsDataSource`. Skonfiguruj element ObjectDataSource, aby ściągał swoje dane z metody `GetDiscontinuedProducts` `ProductsBLLWithSprocs` Class.

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Ilustracja 20**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLLWithSprocs` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))

[![wybierz z listy rozwijanej metodę GetDiscontinuedProducts na karcie Wybieranie](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Ilustracja 21**. wybierz metodę `GetDiscontinuedProducts` z listy rozwijanej na karcie Wybieranie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))

Ponieważ ta siatka zostanie użyta do wyświetlania informacji o produkcie, Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak), a następnie kliknij przycisk Zakończ.

Po zakończeniu działania kreatora program Visual Studio automatycznie doda BoundField lub CheckBoxField dla każdego pola danych w `ProductsDataTable`. Poświęć chwilę, aby usunąć wszystkie te pola z wyjątkiem `ProductName` i `Discontinued`, w tym momencie znaczniki deklaratywne GridView i ObjectDataSource s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Poświęć chwilę na wyświetlenie tej strony za pomocą przeglądarki. Po odwiedzeniu strony element ObjectDataSource wywołuje metodę `GetDiscontinuedProducts` `ProductsBLLWithSprocs` Class. Jak zostało to opisane w kroku 7, ta metoda wywołuje do DAL `ProductsDataTable` s metody `GetDiscontinuedProducts` klasy s, która wywołuje `GetDiscontinuedProducts` procedurę składowaną. Ta procedura składowana jest zarządzaną procedurą składowaną i wykonuje kod utworzony w kroku 3, zwracając wycofane produkty.

Wyniki zwrócone przez zarządzaną procedurę składowaną są pakowane do `ProductsDataTable` przez DAL, a następnie zwracane do LOGIKI biznesowej, który następnie zwraca je do warstwy prezentacji, gdzie są one powiązane z elementem GridView i są wyświetlane. Zgodnie z oczekiwaniami siatka zawiera listę produktów, które zostały wycofane.

[![produkty, które zostały wycofane, są wymienione](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**Ilustracja 22**. wycofane produkty są wymienione na liście ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))

Aby zapewnić dalsze rozwiązanie, Dodaj do strony pole tekstowe i inny element GridView. Czy w tym widoku GridView są wyświetlane produkty mniejsze niż wartość wprowadzona w polu tekstowym przez wywołanie metody `GetProductsWithPriceLessThan` `ProductsBLLWithSprocs` klasy s.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Krok 9. Tworzenie i wywoływanie UDF T-SQL

Funkcje zdefiniowane przez użytkownika, lub UDF, to obiekty bazy danych ściśle naśladuje semantykę funkcji w językach programowania. Podobnie jak w przypadku C#funkcji w, UDF może zawierać zmienną liczbę parametrów wejściowych i zwracać wartość określonego typu. System UDF może zwracać dane skalarne — ciąg, liczbę całkowitą i tak dalej lub dane tabelaryczne. Zezwól na szybkie przeszukiwanie obu typów UDF, rozpoczynając od wartości UDF, która zwraca typ danych skalarnych.

Poniższy format UDF oblicza szacowaną wartość spisu dla określonego produktu. Robi to przez podejmowanie trzech parametrów wejściowych — wartości `UnitPrice`, `UnitsInStock`i `Discontinued` dla określonego produktu — i zwraca wartość typu `money`. Oblicza szacowaną wartość spisu, mnożąc `UnitPrice` przez `UnitsInStock`. W przypadku niewycofanych elementów ta wartość jest mniejsza.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

Po dodaniu tej funkcji UDF do bazy danych można ją znaleźć za pomocą Management Studio, rozszerzając folder programowalny, a następnie funkcje, a następnie funkcje skalarne Value. Można go użyć w zapytaniu `SELECT`, takich jak:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

Dodano `udf_ComputeInventoryValue` UDF do bazy danych Northwind; Rysunek 23 pokazuje dane wyjściowe powyższego `SELECT` zapytania podczas przeglądania za pomocą Management Studio. Należy również zauważyć, że UDF jest wymieniony w folderze funkcje skalarne wartości w Eksplorator obiektów.

[![wszystkie wartości spisu produktów produktu są wymienione na liście](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**Ilustracja 23**: wszystkie wartości spisu produktów są wyświetlane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))

UDF może również zwracać dane tabelaryczne. Można na przykład utworzyć ciąg UDF, który zwraca produkty należące do określonej kategorii:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID` UDF akceptuje parametr wejściowy `@CategoryID` i zwraca wyniki określonego zapytania `SELECT`. Po utworzeniu tego elementu UDF można odwoływać się w klauzuli `FROM` (lub `JOIN`) zapytania `SELECT`. Poniższy przykład zwróci wartości `ProductID`, `ProductName`i `CategoryID` dla każdego z napojów.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

Dodano `udf_GetProductsByCategoryID` UDF do bazy danych Northwind; Rysunek 24 przedstawia dane wyjściowe powyższego `SELECT` zapytania podczas przeglądania za pomocą Management Studio. UDF, które zwracają dane tabelaryczne, można znaleźć w folderze funkcji tabeli Eksplorator obiektów s.

[![identyfikator ProductID, NazwaProduktu i IDKategorii są wyświetlane dla każdego napoju](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Ilustracja 24**. `ProductID`, `ProductName`i `CategoryID` są wyświetlane dla każdego napoju ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia i używania UDF, zapoznaj [się z wprowadzeniem do funkcji zdefiniowanych przez użytkownika](http://www.sqlteam.com/item.asp?ItemID=1955). Sprawdź również [zalety i wady funkcji zdefiniowanych przez użytkownika](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).

## <a name="step-10-creating-a-managed-udf"></a>Krok 10. Tworzenie zarządzanego elementu UDF

`udf_ComputeInventoryValue` i `udf_GetProductsByCategoryID` UDF utworzone w powyższych przykładach to obiekty bazy danych T-SQL. SQL Server 2005 obsługuje również zarządzane UDF, które można dodać do projektu `ManagedDatabaseConstructs`, podobnie jak zarządzane procedury składowane z kroków 3 i 5. Na potrzeby tego kroku, należy zaimplementować `udf_ComputeInventoryValue` UDF w kodzie zarządzanym.

Aby dodać zarządzany system UDF do projektu `ManagedDatabaseConstructs`, kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy element. Wybierz szablon zdefiniowany przez użytkownika w oknie dialogowym Dodaj nowy element i nazwij nowy plik UDF `udf_ComputeInventoryValue_Managed.cs`.

[![dodać nowego zarządzanego elementu UDF do projektu ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Ilustracja 25**. Dodawanie nowego zarządzanego elementu UDF do projektu `ManagedDatabaseConstructs` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))

Zdefiniowany przez użytkownika szablon funkcji tworzy klasę `partial` o nazwie `UserDefinedFunctions` z metodą, której nazwa jest taka sama jak nazwa pliku klasy (`udf_ComputeInventoryValue_Managed`w tym wystąpieniu). Ta metoda jest dekoracyjna przy użyciu [atrybutu`SqlFunction`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), który oznacza metodę jako ZARZĄDZANY system UDF.

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

Metoda `udf_ComputeInventoryValue` obecnie zwraca [obiekt`SqlString`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) i nie akceptuje żadnych parametrów wejściowych. Musimy zaktualizować definicję metody, aby akceptowała trzy parametry wejściowe — `UnitPrice`, `UnitsInStock`i `Discontinued`-i zwraca obiekt `SqlMoney`. Logika obliczania wartości spisu jest taka sama jak w `udf_ComputeInventoryValue` UDF w języku T-SQL.

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Należy pamiętać, że parametry wejściowe metody UDF s mają odpowiednie typy SQL: `SqlMoney` dla `UnitPrice` pole, [`SqlInt16`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) dla `UnitsInStock`i [`SqlBoolean`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) dla `Discontinued`. Te typy danych odzwierciedlają typy zdefiniowane w tabeli `Products`: kolumna `UnitPrice` jest typu `money`, kolumna `UnitsInStock` typu `smallint`oraz kolumna `Discontinued` typu `bit`.

Kod zaczyna się od utworzenia wystąpienia `SqlMoney` o nazwie `inventoryValue`, do którego przypisano wartość 0. Tabela `Products` zezwala na wartości `NULL` bazy danych w kolumnach `UnitsInPrice` i `UnitsInStock`. W związku z tym musi najpierw sprawdzić, czy te wartości zawierają `NULL` s, które można wykonać za pomocą właściwości `SqlMoney` obiektu s [`IsNull`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Jeśli oba `UnitPrice` i `UnitsInStock` zawierają wartości nie`NULL`, firma Microsoft obliczy `inventoryValue` jako iloczyn dwóch. Następnie, jeśli `Discontinued` ma wartość true, wartość jest mniejsza.

> [!NOTE]
> Obiekt `SqlMoney` umożliwia tylko dwa wystąpienia `SqlMoney` do przemnożenia jednocześnie. Nie zezwala na pomnożenie wystąpienia `SqlMoney` przez literał liczby zmiennoprzecinkowej. W związku z tym do połowy `inventoryValue` mnożymy ją przez nowe wystąpienie `SqlMoney` o wartości 0,5.

## <a name="step-11-deploying-the-managed-udf"></a>Krok 11. wdrażanie zarządzanego formatu UDF

Po utworzeniu zarządzanego formatu UDF wszystko jest gotowe do wdrożenia w bazie danych Northwind. W kroku 4 obiekty zarządzane w projekcie SQL Server są wdrażane przez kliknięcie prawym przyciskiem myszy nazwy projektu w Eksplorator rozwiązań i wybranie opcji Wdróż z menu kontekstowego.

Po wdrożeniu projektu Wróć do SQL Server Management Studio i Odśwież folder funkcje o wartościach skalarnych. Powinny teraz być widoczne dwa wpisy:

- `dbo.udf_ComputeInventoryValue` — UDF języka T-SQL utworzony w kroku 9 i
- `dbo.udf ComputeInventoryValue_Managed` — zarządzany system UDF utworzony w kroku 10, który został właśnie wdrożony.

Aby przetestować zarządzany system UDF, wykonaj następujące zapytanie w Management Studio:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

To polecenie używa zarządzanej `udf ComputeInventoryValue_Managed` UDF zamiast UDF `udf_ComputeInventoryValue` języka T-SQL, ale dane wyjściowe są takie same. Zapoznaj się z powrotem z rysunkiem 23, aby wyświetlić zrzut ekranu przedstawiający dane wyjściowe w formacie UDF s.

## <a name="step-12-debugging-the-managed-database-objects"></a>Krok 12. debugowanie obiektów zarządzanej bazy danych

W samouczku [procedury składowane debugowania](debugging-stored-procedures-cs.md) omówiono trzy opcje debugowania SQL Server za pośrednictwem programu Visual Studio: bezpośrednie debugowanie bazy danych, debugowanie aplikacji i debugowanie z SQL Server projekcie. Obiekty zarządzanej bazy danych nie mogą być debugowane za pośrednictwem bezpośredniego debugowania bazy danych, ale mogą być debugowane z aplikacji klienckiej i bezpośrednio z projektu SQL Server. Aby debugowanie działało, jednak baza danych SQL Server 2005 musi zezwalać na debugowanie SQL/CLR. Odwołaj się, że podczas pierwszego tworzenia projektu `ManagedDatabaseConstructs` Visual Studio prosi nas o włączenie debugowania SQL/CLR (patrz rysunek 6 w kroku 2). To ustawienie można zmodyfikować, klikając prawym przyciskiem myszy bazę danych w oknie Eksplorator serwera.

![Upewnij się, że baza danych umożliwia debugowanie SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Ilustracja 26**. Upewnij się, że baza danych umożliwia debugowanie SQL/CLR

Załóżmy, że chcemy debugować procedurę składowaną zarządzaną przez `GetProductsWithPriceLessThan`. Możemy zacząć od ustawienia punktu przerwania w kodzie metody `GetProductsWithPriceLessThan`.

[![ustawić punktu przerwania w metodzie GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Ilustracja 27**. Ustawianie punktu przerwania w metodzie `GetProductsWithPriceLessThan` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))

Pozwól, aby najpierw debugować obiekty zarządzanej bazy danych z projektu SQL Server. Ponieważ nasze rozwiązanie obejmuje dwa projekty — `ManagedDatabaseConstructs` SQL Server projekcie wraz z naszą witryną sieci Web — w celu debugowania z projektu SQL Server należy nakazać programowi Visual Studio uruchomienie `ManagedDatabaseConstructs` SQL Server projektu, gdy rozpocznie się debugowanie. Kliknij prawym przyciskiem myszy projekt `ManagedDatabaseConstructs` w Eksplorator rozwiązań i wybierz opcję Ustaw jako projekt startowy z menu kontekstowego.

Gdy projekt `ManagedDatabaseConstructs` jest uruchamiany z debugera, wykonuje instrukcje SQL w pliku `Test.sql`, który znajduje się w folderze `Test Scripts`. Na przykład w celu przetestowania `GetProductsWithPriceLessThan` zarządzanej procedury składowanej Zastąp istniejącą `Test.sql` zawartość pliku następującą instrukcją, która wywołuje `GetProductsWithPriceLessThan`ą zarządzaną procedurę przechowywaną w `@CategoryID` wartość 14,95:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Po wprowadzeniu powyższego skryptu do `Test.sql`, Rozpocznij debugowanie, przechodząc do menu Debuguj i wybierając polecenie Rozpocznij debugowanie lub naciskając klawisz F5 lub zieloną ikonę odtwarzania na pasku narzędzi. Spowoduje to skompilowanie projektów w ramach rozwiązania, wdrożenie obiektów zarządzanej bazy danych w bazie danych Northwind, a następnie wykonanie skryptu `Test.sql`. W tym momencie punkt przerwania zostanie trafiony i możemy przejść przez metodę `GetProductsWithPriceLessThan`, przeanalizować wartości parametrów wejściowych i tak dalej.

[![został trafiony punkt przerwania w metodzie GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Ilustracja 28**. trafienie punktu przerwania w metodzie `GetProductsWithPriceLessThan` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))

Aby obiekt bazy danych SQL był debugowany za pomocą aplikacji klienckiej, należy bezwzględnie skonfigurować bazę danych do obsługi debugowania aplikacji. Kliknij prawym przyciskiem myszy bazę danych w Eksplorator serwera i upewnij się, że opcja Debugowanie aplikacji jest zaznaczona. Ponadto należy skonfigurować aplikację ASP.NET do integracji z debugerem SQL i wyłączyć buforowanie połączeń. Te kroki zostały szczegółowo omówione w kroku 2 samouczka [procedury składowane debugowania](debugging-stored-procedures-cs.md) .

Po skonfigurowaniu aplikacji ASP.NET i bazy danych ustaw witrynę ASP.NET jako projekt startowy, a następnie Rozpocznij debugowanie. Jeśli zostanie wyświetlona strona, która wywołuje jeden z zarządzanych obiektów, które mają punkt przerwania, aplikacja zostanie zatrzymana, a kontrola zostanie przełączona do debugera, gdzie można przejść przez kod, jak pokazano na rysunku 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Krok 13. ręczne Kompilowanie i wdrażanie obiektów zarządzanych baz danych

Projekty SQL Server ułatwiają tworzenie, kompilowanie i wdrażanie obiektów zarządzanych baz danych. Niestety, SQL Server projekty są dostępne tylko w wersjach Professional i Team Systems programu Visual Studio. Jeśli używasz programu Visual Web Developer lub Standard Edition programu Visual Studio i chcesz korzystać z obiektów zarządzanej bazy danych, musisz je ręcznie utworzyć i wdrożyć. Obejmuje to cztery kroki:

1. Utwórz plik zawierający kod źródłowy obiektu zarządzanej bazy danych,
2. Kompiluj obiekt do zestawu,
3. Zarejestruj zestaw z bazą danych SQL Server 2005 i
4. Utwórz obiekt bazy danych w SQL Server, który wskazuje odpowiednią metodę w zestawie.

Aby zilustrować te zadania, należy utworzyć nową zarządzaną procedurę składowaną, która zwraca te produkty, których `UnitPrice` jest większa niż określona wartość. Utwórz nowy plik na komputerze o nazwie `GetProductsWithPriceGreaterThan.cs` i wprowadź następujący kod do pliku (możesz użyć programu Visual Studio, Notatnika lub dowolnego edytora tekstów):

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Ten kod jest niemal identyczny jak w przypadku metody `GetProductsWithPriceLessThan` utworzonej w kroku 5. Jedyne różnice to nazwy metod, klauzula `WHERE` i nazwa parametru użyta w zapytaniu. Z powrotem w metodzie `GetProductsWithPriceLessThan`, klauzula `WHERE` Read: `WHERE UnitPrice < @MaxPrice`. Tutaj, w `GetProductsWithPriceGreaterThan`używamy: `WHERE UnitPrice > @MinPrice`.

Teraz musimy skompilować tę klasę do zestawu. W wierszu polecenia przejdź do katalogu, w którym zapisano plik `GetProductsWithPriceGreaterThan.cs` i Użyj C# kompilatora (`csc.exe`) do skompilowania pliku klasy w zestawie:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Jeśli folder zawierający `csc.exe` nie znajduje się w `PATH`system s, należy w pełni odwołać się do jego ścieżki, `%WINDOWS%\Microsoft.NET\Framework\version\`w następujący sposób:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]

[![Kompiluj GetProductsWithPriceGreaterThan.cs do zestawu](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Ilustracja 29**. kompilowanie `GetProductsWithPriceGreaterThan.cs` do zestawu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))

Flaga `/t` określa, że plik C# klasy powinien być kompilowany do biblioteki DLL (a nie pliku wykonywalnego). Flaga `/out` określa nazwę tworzonego zestawu.

> [!NOTE]
> Zamiast kompilowania pliku klasy `GetProductsWithPriceGreaterThan.cs` z poziomu wiersza polecenia możesz użyć [programu Visual C# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/) lub utworzyć oddzielny projekt biblioteki klas w programie Visual Studio Standard Edition. Polecenie S Ren Jacob Lauritsen, które ma taki projekt Visual C# Express Edition, zawiera kod dla `GetProductsWithPriceGreaterThan` procedury składowanej oraz dwie zarządzane procedury składowane i UDF utworzone w krokach 3, 5 i 10. Program s Ren s zawiera również polecenia T-SQL, które są konieczne do dodania odpowiednich obiektów bazy danych.

Gdy kod jest kompilowany do zestawu, jest gotowy do zarejestrowania zestawu w bazie danych SQL Server 2005. Można to zrobić za pomocą T-SQL, używając polecenia `CREATE ASSEMBLY`lub SQL Server Management Studio. Zezwól na korzystanie z Management Studio.

W obszarze Management Studio rozwiń folder programowanie w bazie danych Northwind. Jednym z jego podfolderów jest zestaw. Aby ręcznie dodać nowy zestaw do bazy danych, kliknij prawym przyciskiem myszy folder Zestawy i wybierz polecenie Nowy zestaw z menu kontekstowego. Spowoduje to wyświetlenie okna dialogowego Nowy zestaw (zobacz rysunek 30). Kliknij przycisk Przeglądaj, wybierz zestaw `ManuallyCreatedDBObjects.dll`, który właśnie został skompilowany, a następnie kliknij przycisk OK, aby dodać zestaw do bazy danych. Nie powinien być widoczny zestaw `ManuallyCreatedDBObjects.dll` w Eksplorator obiektów.

[![dodać zestawu ManuallyCreatedDBObjects. dll do bazy danych](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**Ilustracja 30**. dodawanie zestawu `ManuallyCreatedDBObjects.dll` do bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))

![ManuallyCreatedDBObjects. dll jest wymieniony w Eksplorator obiektów](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Rysunek 31**. `ManuallyCreatedDBObjects.dll` znajduje się na liście Eksplorator obiektów

Po dodaniu zestawu do bazy danych Northwind nie mamy jeszcze skojarzyć procedury składowanej z metodą `GetProductsWithPriceGreaterThan` w zestawie. Aby to zrobić, Otwórz nowe okno zapytania i wykonaj następujący skrypt:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

Spowoduje to utworzenie nowej procedury składowanej w bazie danych Northwind o nazwie `GetProductsWithPriceGreaterThan` i skojarzenie jej z zarządzaną metodą `GetProductsWithPriceGreaterThan` (która znajduje się w klasie `StoredProcedures`, która znajduje się w `ManuallyCreatedDBObjects`zestawu).

Po wykonaniu powyższego skryptu Odśwież folder procedury składowane w Eksplorator obiektów. Powinna zostać wyświetlona nowa pozycja procedury składowanej — `GetProductsWithPriceGreaterThan` — która ma obok niej ikonę blokady. Aby przetestować tę procedurę składowaną, wprowadź i wykonaj następujący skrypt w oknie zapytania:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Jak pokazano na rysunku 32, powyższe polecenie wyświetla informacje dla tych produktów o `UnitPrice` większym niż $24,95.

[![ManuallyCreatedDBObjects. dll znajduje się na liście Eksplorator obiektów](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Ilustracja 32**: `ManuallyCreatedDBObjects.dll` znajduje się na liście Eksplorator obiektów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))

## <a name="summary"></a>Podsumowanie

Microsoft SQL Server 2005 zapewnia integrację ze środowiskiem uruchomieniowym języka wspólnego (CLR), która umożliwia tworzenie obiektów bazy danych za pomocą kodu zarządzanego. Wcześniej te obiekty bazy danych można utworzyć tylko przy użyciu języka T-SQL, ale teraz można utworzyć te obiekty przy użyciu języków programowania .NET C#, takich jak. W tym samouczku zostały utworzone dwie zarządzane procedury składowane i zarządzana funkcja zdefiniowana przez użytkownika.

Program Visual Studio s SQL Server typ projektu ułatwia tworzenie, kompilowanie i wdrażanie obiektów zarządzanych baz danych. Ponadto oferuje zaawansowaną obsługę debugowania. Jednak typy projektów SQL Server są dostępne tylko w wersjach Professional i Team Systems programu Visual Studio. W przypadku korzystania z programu Visual Web Developer lub Standard Edition programu Visual Studio, kroki tworzenia, kompilowania i wdrażania należy przeprowadzić ręcznie, jak pokazano w kroku 13.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Zalety i wady funkcji zdefiniowanych przez użytkownika](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Tworzenie obiektów SQL Server 2005 w kodzie zarządzanym](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Tworzenie wyzwalaczy przy użyciu kodu zarządzanego w SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Instrukcje: Tworzenie i uruchamianie procedury składowanej SQL Server CLR](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Instrukcje: Tworzenie i uruchamianie CLR SQL Server funkcji zdefiniowanej przez użytkownika](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Instrukcje: Edytowanie skryptu `Test.sql` do uruchamiania obiektów SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Wprowadzenie do funkcji zdefiniowanych przez użytkownika](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Kod zarządzany i SQL Server 2005 (wideo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Dokumentacja języka Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Przewodnik: Tworzenie procedury składowanej w kodzie zarządzanym](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Osoba dokonująca przeglądu potencjalnego klienta dla tego samouczka była Jacob Lauritsen. Oprócz przejrzenia tego artykułu polecenie S Ren spowodowało również utworzenie projektu programu C# Visual Express Edition zawartego w tym artykule s Download do ręcznego kompilowania zarządzanych obiektów bazy danych. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](debugging-stored-procedures-cs.md)
> [dalej](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
