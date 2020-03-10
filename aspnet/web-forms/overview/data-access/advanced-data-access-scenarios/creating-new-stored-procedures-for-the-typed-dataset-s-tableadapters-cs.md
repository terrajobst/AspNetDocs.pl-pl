---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Tworzenie nowych procedur składowanych dla TableAdapters określonego zestawu danych (C#) | Microsoft Docs
author: rick-anderson
description: We wcześniejszych samouczkach utworzyliśmy instrukcje SQL w naszym kodzie i przekazały instrukcje do bazy danych, która ma zostać wykonana. Alternatywnym podejściem jest użycie metody s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: db0d83a0fd1f1f175001d20844b298be0cf7e1cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78533713"
---
# <a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Tworzenie nowych procedur składowanych dla elementów TableAdapter typizowanego zestawu danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) lub [Pobierz plik PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> We wcześniejszych samouczkach utworzyliśmy instrukcje SQL w naszym kodzie i przekazały instrukcje do bazy danych, która ma zostać wykonana. Alternatywnym podejściem jest użycie procedur składowanych, w których instrukcje SQL są wstępnie zdefiniowane w bazie danych. W tym samouczku dowiesz się, jak za pomocą Kreatora TableAdapter wygenerować nowe procedury składowane.

## <a name="introduction"></a>Wprowadzenie

Warstwa dostępu do danych (DAL) dla tych samouczków używa wpisanych zestawów danych. Zgodnie z opisem w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) , typy zestawów danych składają się z jednoznacznie wpisanych tabel DataTables i TableAdapters. Tabele danych reprezentują jednostki logiczne w systemie, podczas gdy interfejs TableAdapters z podstawową bazą danych umożliwia wykonywanie zadań dostępu do danych. Obejmuje to Wypełnianie tabel danych danymi, wykonywanie zapytań, które zwracają dane skalarne oraz wstawianie, aktualizowanie i usuwanie rekordów z bazy danych.

Polecenia SQL wykonywane przez TableAdapters mogą być instrukcją SQL ad hoc, taką jak `SELECT columnList FROM TableName`lub procedury składowane. TableAdapters w naszej architekturze używają ad hoc instrukcji SQL. Wielu deweloperów i administratorów bazy danych preferują jednak procedury składowane za pośrednictwem instrukcji SQL ad hoc w zakresie bezpieczeństwa, utrzymania i możliwość aktualizacji. Inne ardently preferują ad hoc instrukcje SQL, aby zapewnić ich elastyczność. W ramach własnych pracy preferuję procedury składowane za pośrednictwem instrukcji SQL ad hoc, ale wybrano używanie instrukcji SQL ad hoc w celu uproszczenia wcześniejszych samouczków.

Podczas definiowania TableAdapter lub dodawania nowych metod, Kreator TableAdapter s sprawia, że jest równie proste, aby można było tworzyć nowe procedury składowane lub używać istniejących procedur przechowywanych, jak w przypadku używania instrukcji SQL ad hoc. W tym samouczku sprawdzimy, jak Kreator TableAdapter s automatycznie generuje procedury składowane. W następnym samouczku powiesz się, jak skonfigurować metody TableAdapter s do używania istniejących lub ręcznie tworzonych procedur składowanych.

> [!NOTE]
> Zobacz wpis w blogu z programu Rob Howard [nie używaj jeszcze procedur przechowywanych?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) , a [Frans Bouma](https://weblogs.asp.net/fbouma/) s [procedury składowane wpisów w blogu są nieprawidłowe, M Kay?,](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) aby zapoznać się z livelyą debatą dotyczącą informatyków i wad procedur przechowywanych oraz SQL ad hoc.

## <a name="stored-procedure-basics"></a>Podstawy procedury składowanej

Funkcje są wspólne dla wszystkich języków programowania. Funkcja jest kolekcją instrukcji, które są wykonywane, gdy wywoływana jest funkcja. Funkcje mogą akceptować parametry wejściowe i opcjonalnie zwracać wartość. *[Procedury składowane](http://en.wikipedia.org/wiki/Stored_procedure)* to konstrukcje baz danych, które współużytkują wiele podobieństw z funkcjami w językach programowania. Procedura składowana składa się z zestawu instrukcji T-SQL, które są wykonywane po wywołaniu procedury składowanej. Procedura składowana może przyjmować zero do wielu parametrów wejściowych i może zwracać wartości skalarne, parametry wyjściowe lub, najczęściej, zestawy wyników z zapytań `SELECT`.

> [!NOTE]
> Procedury składowane są często określane jako sprocs lub SPs.

Procedury składowane są tworzone przy użyciu instrukcji języka t-SQL [`CREATE PROCEDURE`](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) . Na przykład poniższy skrypt T-SQL tworzy procedurę przechowywaną o nazwie `GetProductsByCategoryID`, która przyjmuje jeden parametr o nazwie `@CategoryID` i zwraca pola `ProductID`, `ProductName`, `UnitPrice`i `Discontinued` tych kolumn w tabeli `Products`, które mają zgodną `CategoryID` wartość:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Po utworzeniu tej procedury składowanej można ją wywołać przy użyciu następującej składni:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> W następnym samouczku będziemy analizować procedury składowane za pomocą środowiska IDE programu Visual Studio. Jednak w tym samouczku będziemy mogli automatycznie generować procedury składowane przez kreatora TableAdapter.

Oprócz zwykłego zwracania danych procedury składowane są często używane do wykonywania wielu poleceń bazy danych w zakresie pojedynczej transakcji. Procedura składowana o nazwie `DeleteCategory`, na przykład, może przyjmować parametr `@CategoryID` i wykonywać dwie `DELETE`e instrukcje: pierwszy, jeden, aby usunąć powiązane produkty i drugi raz usunąć określoną kategorię. Wielokrotne instrukcje w ramach procedury składowanej *nie* są automatycznie zawijane w ramach transakcji. Należy wydać dodatkowe polecenia T-SQL, aby upewnić się, że w procedurze składowanej wiele poleceń jest traktowanych jako operacja niepodzielna. Zobaczymy sposób zawijania poleceń procedury składowanej w ramach zakresu transakcji w następnym samouczku.

W przypadku korzystania z procedur składowanych w ramach architektury, metody warstwy dostępu do danych wywołują konkretną procedurę składowaną zamiast wydawania instrukcji SQL ad hoc. Umożliwia to scentralizowanie lokalizacji instrukcji SQL wykonanych (w bazie danych), a nie ich zdefiniowanie w ramach architektury aplikacji. Ten raczej scentralizowany ułatwia znajdowanie, analizowanie i dostrajanie zapytań oraz zapewnia znaczną czytelność obrazu, w jaki sposób i jak jest używana baza danych.

Aby uzyskać więcej informacji na temat podstawowych procedur przechowywanych, zapoznaj się z artykułami w dalszej części dotyczącej odczytu na końcu tego samouczka.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Krok 1. Tworzenie stron sieci Web zaawansowanych scenariuszy warstwy dostępu do danych

Przed rozpoczęciem dyskusji na temat tworzenia DAL za pomocą procedur składowanych Poświęć chwilę na utworzenie stron ASP.NET w naszym projekcie witryny sieci Web, które będą potrzebne dla tego i kilku następnych samouczków. Zacznij od dodania nowego folderu o nazwie `AdvancedDAL`. Następnie Dodaj następujące strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`

![Dodaj strony ASP.NET dla samouczków zaawansowanej warstwy dostępu do danych](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Rysunek 1**. Dodawanie ASP.NET stron dla samouczków zaawansowanej warstwy dostępu do danych

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `AdvancedDAL` wyświetli samouczki w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. W związku z tym należy dodać tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na stronę widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))

Na koniec Dodaj te strony jako wpisy do pliku `Web.sitemap`. W tym celu należy dodać następujące znaczniki po pracy z danymi wsadowymi `<siteMapNode>`:

[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy dla samouczków zaawansowane scenariusze DAL.

![Mapa witryny zawiera teraz wpisy dla samouczków zaawansowanego scenariusza DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Rysunek 3**. Mapa witryny zawiera teraz wpisy dla samouczków zaawansowanych dal scenariuszy

## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Krok 2. Konfigurowanie TableAdapter w celu utworzenia nowych procedur składowanych

Aby zademonstrować Tworzenie warstwy dostępu do danych, która używa procedur składowanych zamiast instrukcji SQL ad hoc, pozwól, aby utworzyć nowy typ zestawu danych w folderze `~/App_Code/DAL` o nazwie `NorthwindWithSprocs.xsd`. Ze względu na szczegółowe instrukcje tego procesu w poprzednich samouczkach przejdziemy szybko w tym miejscu. W przypadku zablokowania lub potrzeby dalszych instrukcji krok po kroku dotyczących tworzenia i konfigurowania określonego zestawu danych zapoznaj się z artykułem [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) .

Dodaj nowy zestaw danych do projektu, klikając prawym przyciskiem myszy folder `DAL`, wybierając pozycję Dodaj nowy element i wybierając szablon zestawu danych, jak pokazano na rysunku 4.

[![dodać do projektu nowy typ zestawu danych o nazwie NorthwindWithSprocs. xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Ilustracja 4**. Dodawanie nowego typu zestawu danych do projektu o nazwie `NorthwindWithSprocs.xsd` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))

Spowoduje to utworzenie nowego typu zestawu danych, otwarcie jego projektanta, utworzenie nowego TableAdapter i uruchomienie Kreatora konfiguracji TableAdapter. W pierwszym kroku Kreatora konfiguracji TableAdapter zostanie wyświetlony monit z pytaniem o wybranie bazy danych, z którą chcesz współpracować. Parametry połączenia z bazą danych Northwind powinny znajdować się na liście rozwijanej. Wybierz tę opcję i kliknij przycisk Dalej.

Na tym następnym ekranie możemy wybrać sposób, w jaki TableAdapter powinien uzyskiwać dostęp do bazy danych. W poprzednich samouczkach wybrano pierwszą opcję, użyj instrukcji SQL. Na potrzeby tego samouczka wybierz drugą opcję, Utwórz nowe procedury składowane, a następnie kliknij przycisk Dalej.

[![nakazuje TableAdapter utworzyć nowe procedury składowane](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Rysunek 5**. poinstruuj TableAdapter, aby utworzył nowe procedury składowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))

Podobnie jak w przypadku korzystania z instrukcji SQL ad hoc, w poniższym kroku zaleca się dostarczenie instrukcji `SELECT` dla głównego zapytania TableAdapter s. Zamiast używać instrukcji `SELECT` wprowadzonej w tym miejscu do bezpośredniego wykonywania zapytania ad hoc, Kreator TableAdapter s utworzy procedurę przechowywaną, która zawiera to `SELECT` zapytanie.

Użyj następującego `SELECT` kwerendy dla tego TableAdapter:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

[![wprowadź zapytanie SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Ilustracja 6**. wprowadzanie zapytania `SELECT` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))

> [!NOTE]
> Zapytanie powyżej różni się nieco od głównej kwerendy `ProductsTableAdapter` w zestawie danych z `Northwind` typem. Odwołaj, że `ProductsTableAdapter` w określonym `Northwind` zestawie danych zawiera dwa skorelowane podzapytania, aby przywrócić nazwę kategorii i nazwę firmy dla każdej kategorii i dostawcy produktów. W nadchodzącym TableAdapter samouczku [do korzystania z sprzężeń](updating-the-tableadapter-to-use-joins-cs.md) dowiesz się, jak dodać te powiązane dane do TableAdapter.

Poświęć chwilę, aby kliknąć przycisk Opcje zaawansowane. W tym miejscu możemy określić, czy Kreator powinien również generować instrukcje INSERT, Update i DELETE dla TableAdapter, czy ma być używana Optymistyczna współbieżność, czy tabela danych powinna być odświeżana po wstawieniu i aktualizacji. Opcja Generuj instrukcje INSERT, Update i DELETE jest domyślnie zaznaczona. Pozostaw to pole wyboru. Na potrzeby tego samouczka Pozostaw zaznaczenie pola wyboru Użyj optymistycznych opcji współbieżności.

Gdy procedury składowane tworzone automatycznie przez kreatora TableAdapter pojawiają się, że opcja Odśwież tabelę danych jest ignorowana. Bez względu na to, czy to pole wyboru jest zaznaczone, wyniki operacji INSERT i Update składowane pobierają wstawiony lub tylko zaktualizowany rekord, ponieważ zobaczymy w kroku 3.

![Pozostaw zaznaczoną opcję Generuj instrukcje INSERT, Update i DELETE](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Rysunek 7**. Pozostaw zaznaczoną opcję Generuj instrukcje INSERT, Update i DELETE

> [!NOTE]
> Jeśli zaznaczona jest opcja Użyj optymistycznej współbieżności, Kreator doda dodatkowe warunki do klauzuli `WHERE`, która uniemożliwi aktualizowanie danych w przypadku wprowadzenia zmian w innych polach. Zapoznaj się z artykułem [wdrażanie optymistycznego samouczka współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) , aby uzyskać więcej informacji na temat używania wbudowanej funkcji jednokrotnej kontroli współbieżności TableAdapter.

Po wprowadzeniu kwerendy `SELECT` i upewnieniu się, że opcja Generuj instrukcje INSERT, Update i DELETE jest zaznaczona, kliknij przycisk Dalej. Na tym następnym ekranie pokazano na rysunku 8, monitując o nazwy procedur składowanych tworzonych przez kreatora do wybierania, wstawiania, aktualizowania i usuwania danych. Zmień te nazwy procedur składowanych na `Products_Select`, `Products_Insert`, `Products_Update`i `Products_Delete`.

[![zmienić nazwy procedur składowanych](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Ilustracja 8**. zmiana nazwy procedur składowanych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))

Aby wyświetlić Kreatora TableAdapter w języku T-SQL, zostanie użyty do utworzenia czterech procedur składowanych, kliknij przycisk Podgląd skryptu SQL. W oknie dialogowym Podgląd skryptu SQL możesz zapisać skrypt do pliku lub skopiować go do Schowka.

![Podgląd skryptu SQL używanego do wygenerowania procedur składowanych](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Ilustracja 9**. Podgląd skryptu SQL używanego do generowania procedur składowanych

Po napisaniu nazw procedur składowanych kliknij przycisk Dalej, aby nazwać odpowiednie metody TableAdapter. Podobnie jak w przypadku korzystania z instrukcji SQL ad hoc, możemy utworzyć metody, które wypełniają istniejące DataTable lub zwracają nowe. Możemy także określić, czy TableAdapter powinien zawierać wzorzec DB-Direct do wstawiania, aktualizowania i usuwania rekordów. Pozostaw wszystkie trzy pola wyboru zaznaczone, ale Zmień nazwę zwracanej metody DataTable na `GetProducts` (jak pokazano na rysunku 10).

[![Nazwij metody Fill i getProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Ilustracja 10**. nazwij metody `Fill` i `GetProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))

Kliknij przycisk Dalej, aby zobaczyć podsumowanie kroków wykonywanych przez kreatora. Ukończ pracę kreatora, klikając przycisk Zakończ. Po zakończeniu działania kreatora nastąpi powrót do projektanta zestawu danych, który powinien teraz zawierać `ProductsDataTable`.

[![Projektant obiektów DataSet pokazuje nowo dodane ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Ilustracja 11**. Projektant zestawów danych pokazuje nowo dodane `ProductsDataTable` (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))

## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Krok 3: badanie nowo utworzonych procedur składowanych

Kreator TableAdapter używany w kroku 2 automatycznie utworzył procedury składowane do wybierania, wstawiania, aktualizowania i usuwania danych. Te procedury składowane można wyświetlać lub modyfikować za pomocą programu Visual Studio, przechodząc do Eksplorator serwera i przechodzenie do szczegółów w folderze procedury składowane bazy danych. Jak pokazano na rysunku 12, baza danych Northwind zawiera cztery nowe procedury składowane: `Products_Delete`, `Products_Insert`, `Products_Select`i `Products_Update`.

![Cztery procedury składowane utworzone w kroku 2 znajdują się w folderze procedury składowane bazy danych](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Ilustracja 12**. cztery procedury składowane utworzone w kroku 2 znajdują się w folderze procedury składowane bazy danych

> [!NOTE]
> Jeśli nie widzisz Eksplorator serwera, przejdź do menu Widok i wybierz opcję Eksplorator serwera. Jeśli nie widzisz procedur składowanych związanych z produktem dodanych z kroku 2, spróbuj kliknąć prawym przyciskiem myszy folder procedury składowane i wybierz polecenie Odśwież.

Aby wyświetlić lub zmodyfikować procedurę składowaną, kliknij dwukrotnie jej nazwę w Eksplorator serwera lub, kliknij prawym przyciskiem myszy procedurę składowaną i wybierz polecenie Otwórz. Rysunek 13 zawiera `Products_Delete` procedury przechowywanej, po otwarciu.

[![procedury składowane mogą być otwierane i modyfikowane w programie Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Ilustracja 13**. procedury składowane mogą być otwierane i modyfikowane w programie Visual Studio ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))

Zawartość procedur składowanych `Products_Delete` i `Products_Select` jest dość prosta. `Products_Insert` i `Products_Update` procedury składowane, z drugiej strony, gwarantują bliższą kontrolę, ponieważ wykonują instrukcję `SELECT` po instrukcji `INSERT` i `UPDATE`. Na przykład poniższy kod SQL tworzy `Products_Insert` procedury składowanej:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Procedura składowana akceptuje jako parametry wejściowe `Products` kolumny, które zostały zwrócone przez zapytanie `SELECT` określone w Kreatorze TableAdapter s i te wartości są używane w instrukcji `INSERT`. Zgodnie z instrukcją `INSERT` zapytanie `SELECT` jest używane do zwrócenia wartości `Products` kolumny (łącznie z `ProductID`) nowo dodanego rekordu. Ta funkcja odświeżania jest przydatna w przypadku dodawania nowego rekordu przy użyciu wzorca aktualizacji wsadowych, ponieważ automatycznie aktualizuje nowo dodane wystąpienia `ProductRow` `ProductID` właściwości z wartościami automatycznie zwiększanymi przez bazę danych.

Poniższy kod ilustruje tę funkcję. Zawiera `ProductsTableAdapter` i `ProductsDataTable` utworzone dla zestawu danych `NorthwindWithSprocs` określonego typu. Nowy produkt zostanie dodany do bazy danych przez utworzenie wystąpienia `ProductsRow`, dostarczenie jego wartości i wywołanie metody TableAdapter s `Update`, przekazując w `ProductsDataTable`. Wewnętrznie metoda TableAdapter `Update` wylicza `ProductsRow` wystąpień w zadanej tabeli DataTable (w tym przykładzie jest tylko jeden — dodaliśmy) i wykonuje odpowiednie polecenie INSERT, Update lub DELETE. W takim przypadku wykonywana jest `Products_Insert` procedury składowanej, która dodaje nowy rekord do tabeli `Products` i zwraca szczegóły nowo dodanego rekordu. Wartość `ProductID` `ProductsRow` wystąpienia s zostanie zaktualizowana. Po ukończeniu metody `Update` dostęp do nowo dodanego rekordu `ProductID` można uzyskać za pomocą właściwości `ProductsRow` `ProductID`.

[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

Procedura składowana `Products_Update` podobnie zawiera instrukcję `SELECT` po instrukcji `UPDATE`.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Należy zauważyć, że ta procedura składowana zawiera dwa parametry wejściowe dla `ProductID`: `@Original_ProductID` i `@ProductID`. Ta funkcja umożliwia obsługę scenariuszy, w których można zmienić klucz podstawowy. Na przykład w bazie danych pracownika każdy rekord pracownika może używać numeru ubezpieczenia społecznego pracownika jako klucza podstawowego. Aby zmienić istniejący numer ubezpieczenia społecznego pracownika, należy podać zarówno nowy numer ubezpieczenia społecznego, jak i oryginał. W przypadku tabeli `Products` taka funkcja nie jest wymagana, ponieważ kolumna `ProductID` jest kolumną `IDENTITY` i nigdy nie powinna być zmieniana. W rzeczywistości instrukcja `UPDATE` w `Products_Update` procedurze składowanej nie obejmuje kolumny `ProductID` na liście kolumn. Tak więc, podczas gdy `@Original_ProductID` jest używany w klauzuli `WHERE` instrukcji `UPDATE` s, jest zbędny dla tabeli `Products` i może być zastąpiony przez parametr `@ProductID`. Podczas modyfikowania parametrów procedury składowanej należy pamiętać, że metody TableAdapter, które używają tej procedury składowanej, są również aktualizowane.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Krok 4. modyfikowanie parametrów procedury składowanej s i aktualizowanie TableAdapter

Ponieważ parametr `@Original_ProductID` jest zbędny, należy usunąć go z `Products_Update` procedury składowanej. Otwórz `Products_Update` procedury składowanej, Usuń parametr `@Original_ProductID` i w klauzuli `WHERE` instrukcji `UPDATE` Zmień nazwę parametru używaną z `@Original_ProductID` na `@ProductID`. Po wprowadzeniu tych zmian, język T-SQL w ramach procedury składowanej powinien wyglądać następująco:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Aby zapisać te zmiany w bazie danych, kliknij ikonę Zapisz na pasku narzędzi lub naciśnij klawisze CTRL + S. W tym momencie `Products_Update` procedury składowanej nie oczekuje `@Original_ProductID` parametru wejściowego, ale TableAdapter jest skonfigurowany do przekazywania takiego parametru. Można zobaczyć parametry, które TableAdapter wyślą do procedury składowanej `Products_Update` przez wybranie TableAdapter w Projektancie obiektów DataSet, przechodzenie do okno Właściwości i kliknięcie wielokropka w kolekcji `UpdateCommand` `Parameters`. Spowoduje to wyświetlenie okna dialogowego Edytor kolekcji parametrów pokazanego na rysunku 14.

![Edytor kolekcji parametrów zawiera listę parametrów użytych do procedury składowanej Products_Update](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Ilustracja 14**. Edytor kolekcji parametrów zawiera listę parametrów użytych do procedury składowanej `Products_Update`

Ten parametr można usunąć z tego miejsca, zaznaczając parametr `@Original_ProductID` z listy członków i klikając przycisk Usuń.

Alternatywnie można odświeżyć parametry używane dla wszystkich metod, klikając prawym przyciskiem myszy TableAdapter w projektancie, a następnie wybierając pozycję Konfiguruj. Spowoduje to wyświetlenie Kreatora konfiguracji usługi TableAdapter, który zawiera listę procedur przechowywanych używanych do wybierania, wstawiania, aktualizowania i usuwania, a także parametry, które powinny zostać odebrane przez procedury składowane. Po kliknięciu listy rozwijanej aktualizacja można zobaczyć, że `Products_Update` procedury składowane oczekują parametrów wejściowych, które nie zawierają już `@Original_ProductID` (patrz rysunek 15). Po prostu kliknij przycisk Zakończ, aby automatycznie zaktualizować kolekcję parametrów używaną przez TableAdapter.

[![można użyć Kreatora konfiguracji TableAdapter s, aby odświeżyć kolekcje parametrów metod](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Ilustracja 15**. Możesz również użyć Kreatora konfiguracji TableAdapter s, aby odświeżyć kolekcje parametrów metod ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))

## <a name="step-5-adding-additional-tableadapter-methods"></a>Krok 5. Dodawanie dodatkowych metod TableAdapter

Jak pokazano w kroku 2, podczas tworzenia nowego TableAdapter łatwo jest automatycznie generować odpowiednie procedury składowane. To samo jest prawdziwe przy dodawaniu dodatkowych metod do TableAdapter. Aby to zilustrować, pozwól s dodać metodę `GetProductByProductID(productID)` do `ProductsTableAdapter` utworzonego w kroku 2. Ta metoda przyjmuje jako dane wejściowe `ProductID` wartość i zwróci szczegóły dotyczące określonego produktu.

Zacznij od kliknięcia prawym przyciskiem myszy TableAdapter i wybierz polecenie Dodaj zapytanie z menu kontekstowego.

![Dodaj nowe zapytanie do TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Ilustracja 16**. Dodawanie nowego zapytania do TableAdapter

Spowoduje to uruchomienie Kreatora konfiguracji zapytania TableAdapter, który najpierw wyświetli komunikat o tym, jak TableAdapter powinien uzyskać dostęp do bazy danych. Aby utworzyć nową procedurę składowaną, wybierz opcję Utwórz nową procedurę składowaną, a następnie kliknij przycisk Dalej.

[![wybrać opcję Utwórz nową procedurę składowaną](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Ilustracja 17**. Wybierz opcję Utwórz nową procedurę składowaną ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))

Na następnym ekranie poprosimy nas o zidentyfikowanie typu zapytania do wykonania, czy zwróci zestaw wierszy, czy jedną wartość skalarną, czy też wykonuje instrukcję `UPDATE`, `INSERT`lub `DELETE`. Ponieważ metoda `GetProductByProductID(productID)` zwróci wiersz, pozostaw wybraną opcję, która zwraca wiersz, i kliknij przycisk Dalej.

[![wybierz opcję Wybierz, która zwraca wiersz](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Ilustracja 18**. Wybierz opcję Wybierz, która zwraca wiersz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))

Na następnym ekranie zostanie wyświetlona kwerenda główna TableAdapter, która po prostu wyświetla nazwę procedury składowanej (`dbo.Products_Select`). Zastąp nazwę procedury składowanej następującą instrukcją `SELECT`, która zwraca wszystkie pola produktu dla określonego produktu:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]

[![zastąpić nazwy procedury składowanej kwerendą SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Ilustracja 19**. Zastąp nazwę procedury składowanej `SELECT` zapytania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))

Na następnym ekranie zostanie wyświetlony monit o podanie nazwy procedury składowanej, która zostanie utworzona. Wprowadź nazwę `Products_SelectByProductID` a następnie kliknij przycisk Dalej.

[![nazwę nowej procedury składowanej Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Ilustracja 20**. Nazwa nowej procedury składowanej `Products_SelectByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))

Ostatni krok kreatora pozwala zmienić nazwy metod, które wygenerowały, a także wskazać, czy użyć wypełnienia wzorca DataTable, zwrócić wzorzec DataTable lub oba te elementy. W przypadku tej metody pozostaw zaznaczone obie opcje, ale Zmień nazwy metod na `FillByProductID` i `GetProductByProductID`. Kliknij przycisk Dalej, aby wyświetlić podsumowanie kroków wykonywanych przez kreatora, a następnie kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![zmienić nazwy metod TableAdapter s na FillByProductID i GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Ilustracja 21**. zmiana nazw metod TableAdapter `FillByProductID` i `GetProductByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))

Po zakończeniu działania kreatora TableAdapter ma nową metodę, `GetProductByProductID(productID)` to, gdy zostanie wywołane, wykona procedurę składowaną `Products_SelectByProductID`, która została właśnie utworzona. Poświęć chwilę na wyświetlenie tej nowej procedury składowanej z Eksplorator serwera, przechodząc do folderu procedury składowane i otwierając `Products_SelectByProductID` (jeśli go nie widzisz, kliknij prawym przyciskiem myszy folder procedury składowane i wybierz polecenie Odśwież).

Należy zauważyć, że `SelectByProductID` procedura składowana przyjmuje `@ProductID` jako parametr wejściowy i wykonuje instrukcję `SELECT` wprowadzoną w kreatorze.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Krok 6. Tworzenie klasy warstwy logiki biznesowej

W całej serii samouczków dążymy do utrzymania architektury warstwowej, w której warstwa prezentacji wykonał wszystkie wywołania do warstwy logiki biznesowej (LOGIKI biznesowej). Aby móc przestrzegać tej decyzji projektowej, najpierw musimy utworzyć klasę LOGIKI biznesowej dla nowego typu zestawu danych, zanim będzie można uzyskać dostęp do danych produktu z warstwy prezentacji.

Utwórz nowy plik klasy o nazwie `ProductsBLLWithSprocs.cs` w folderze `~/App_Code/BLL` i Dodaj do niego następujący kod:

[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Ta klasa naśladuje semantykę klasy `ProductsBLL` z wcześniejszych samouczków, ale używa obiektów `ProductsTableAdapter` i `ProductsDataTable` z zestawu danych `NorthwindWithSprocs`. Na przykład, zamiast mieć instrukcję `using NorthwindTableAdapters` na początku pliku klasy jako `ProductsBLL`, Klasa `ProductsBLLWithSprocs` używa `using NorthwindWithSprocsTableAdapters`. Podobnie obiekty `ProductsDataTable` i `ProductsRow` używane w tej klasie są poprzedzone `NorthwindWithSprocs` przestrzeni nazw. Klasa `ProductsBLLWithSprocs` zapewnia dwie metody dostępu do danych, `GetProducts` i `GetProductByProductID`oraz metody dodawania, aktualizowania i usuwania pojedynczego wystąpienia produktu.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Krok 7. Praca z zestawem danych`NorthwindWithSprocs`z warstwy prezentacji

W tym momencie utworzyliśmy DAL, który używa procedur składowanych w celu uzyskania dostępu do podstawowych danych bazy danych i ich modyfikacji. Utworzyliśmy również podstawowe LOGIKI biznesowej z metodami pobierania wszystkich produktów lub określonego produktu oraz metodami dodawania, aktualizowania i usuwania produktów. Aby zaokrąglić ten samouczek, należy utworzyć stronę ASP.NET, która używa klasy LOGIKI biznesowej `ProductsBLLWithSprocs` do wyświetlania, aktualizowania i usuwania rekordów.

Otwórz stronę `NewSprocs.aspx` w folderze `AdvancedDAL` i przeciągnij widok GridView z przybornika do projektanta, nadając mu nazwę `Products`. W tagu inteligentnym GridView wybierz, aby powiązać go z nowym elementem ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj element ObjectDataSource do używania klasy `ProductsBLLWithSprocs`, jak pokazano na rysunku 22.

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Ilustracja 22**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLLWithSprocs` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))

Lista rozwijana na karcie Wybierz ma dwie opcje, `GetProducts` i `GetProductByProductID`. Ponieważ chcemy wyświetlić wszystkie produkty w widoku GridView, wybierz metodę `GetProducts`. Listy rozwijane na kartach UPDATE, INSERT i DELETE mają tylko jedną metodę. Upewnij się, że każda z tych list rozwijanych ma wybraną odpowiednią metodę, a następnie kliknij przycisk Zakończ.

Po zakończeniu pracy Kreatora ObjectDataSource program Visual Studio doda BoundFields i CheckBoxField do widoku GridView dla pól danych produktu. Włącz wbudowaną edycję i usuwanie funkcji GridView, zaznaczając opcje Włącz edytowanie i Włącz usuwanie dostępne w tagu inteligentnym.

[![Strona zawiera widok GridView z włączoną obsługą edycji i usuwania](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Ilustracja 23**. Strona zawiera widok GridView z włączoną obsługą edycji i usuwania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))

Jak opisano w poprzednich samouczkach, po zakończeniu działania kreatora ObjectDataSource s program Visual Studio ustawi właściwość `OldValuesParameterFormatString` na oryginalny\_{0}. Należy przywrócić wartość domyślną {0}, aby funkcje modyfikacji danych działały prawidłowo, podając parametry oczekiwane przez metody w naszym LOGIKI biznesowej. W związku z tym należy ustawić właściwość `OldValuesParameterFormatString` na {0} lub usunąć Właściwość całkowicie ze składni deklaracyjnej.

Po zakończeniu działania Kreatora konfiguracji źródła danych, włączenia edytowania i usuwania pomocy w widoku GridView i zwróceniem właściwości "ObjectDataSource s `OldValuesParameterFormatString` do wartości domyślnej, znaczniki deklaratywne strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

W tym momencie możemy uporządkowanego widok GridView przez dostosowanie interfejsu edycji do uwzględnienia walidacji, gdy kolumny `CategoryID` i `SupplierID` są renderowane jako kontrolek DropDownList i tak dalej. Możemy również dodać potwierdzenie po stronie klienta do przycisku Usuń i zachęcamy do podzielenia czasu na wdrożenie tych ulepszeń. Ze względu na to, że te tematy zostały omówione w poprzednich samouczkach, nie będą jednak omawiane w tym miejscu.

Bez względu na to, czy ulepszasz widok GridView, przetestuj podstawowe funkcje strony w przeglądarce. Jak pokazano na rysunku 24, Strona zawiera listę produktów w widoku GridView, które udostępniają możliwości edytowania i usuwania poszczególnych wierszy.

[![produkty można wyświetlać, edytować i usuwać z widoku GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Ilustracja 24**. produkty można wyświetlać, edytować i usuwać z widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))

## <a name="summary"></a>Podsumowanie

TableAdapters w określonym zestawie danych może uzyskać dostęp do danych z bazy danych przy użyciu instrukcji SQL ad hoc lub procedur składowanych. Podczas pracy z procedurami składowanymi można użyć istniejących procedur składowanych lub Kreatora TableAdapter można utworzyć nowe procedury składowane na podstawie zapytania `SELECT`owego. W tym samouczku przedstawiono sposób, w jaki procedury składowane są automatycznie tworzone dla nas.

Podczas gdy procedury składowane generowane automatycznie pomagają zaoszczędzić czas, istnieją pewne przypadki, w których procedura składowana utworzona przez kreatora jest niewyrównana i została utworzona we własnym zakresie. Przykładem jest `Products_Update` procedury składowanej, która oczekuje zarówno `@Original_ProductID`, jak i `@ProductID` parametrów wejściowych, mimo że parametr `@Original_ProductID` był zbędny.

W wielu scenariuszach procedury składowane mogły już zostać utworzone lub mogą chcieć skompilować je ręcznie, aby mieć dokładniejszy stopień kontroli nad poleceniami procedury składowanej. W obu przypadkach chcemy poinstruować TableAdapter, aby używał istniejących procedur przechowywanych dla swoich metod. W następnym samouczku Dowiedz się, jak to zrobić.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Tworzenie i obsługa procedur składowanych](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Pobieranie danych skalarnych z procedury składowanej](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Podstawowe informacje SQL Server procedury składowanej](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procedury składowane: przegląd](http://www.sqlteam.com/item.asp?ItemID=563)
- [Pisanie procedury składowanej](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Hilton Geisenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Dalej](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
