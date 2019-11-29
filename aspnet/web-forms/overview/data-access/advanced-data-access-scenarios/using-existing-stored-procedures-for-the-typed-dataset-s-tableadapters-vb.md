---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Używanie istniejących procedur składowanych dla TableAdapters określonego zestawu danych (VB) | Microsoft Docs
author: rick-anderson
description: W poprzednim samouczku dowiesz się, jak wygenerować nowe procedury składowane przy użyciu kreatora TableAdapter. W tym samouczku dowiesz się, jak to samo TableAdapter...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: e35c3d6a98516a07f6119e6cb9dbeb99bc28fe33
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74613558"
---
# <a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Korzystanie z istniejących procedur składowanych dla elementów TableAdapter typizowanego zestawu danych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) lub [Pobierz plik PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> W poprzednim samouczku dowiesz się, jak wygenerować nowe procedury składowane przy użyciu kreatora TableAdapter. W tym samouczku dowiesz się, jak ten sam Kreator TableAdapter może współdziałać z istniejącymi procedurami składowanymi. Dowiesz się również, jak ręcznie dodawać nowe procedury składowane do bazy danych.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) przedstawiono sposób, w jaki TableAdapters zestawu danych można skonfigurować do korzystania z procedur składowanych w celu uzyskania dostępu do danych, a nie ad hoc instrukcji SQL. W szczególności sprawdziłmy, jak Kreator TableAdapter ma automatycznie tworzyć te procedury składowane. W przypadku przenoszenia starszej aplikacji do ASP.NET 2,0 lub podczas kompilowania witryny sieci Web ASP.NET 2,0 z istniejącym modelem danych prawdopodobnie baza danych zawiera już wymagane przez nas procedury składowane. Alternatywnie można utworzyć procedury składowane ręcznie lub za pomocą narzędzia innego niż Kreator TableAdapter, który automatycznie generuje procedury składowane.

W tym samouczku powiesz się, jak skonfigurować TableAdapter do korzystania z istniejących procedur składowanych. Ponieważ baza danych Northwind zawiera tylko niewielki zestaw wbudowanych procedur składowanych, należy również zapoznać się z krokami, które trzeba wykonać, aby ręcznie dodać nowe procedury składowane do bazy danych za pomocą środowiska programu Visual Studio. Zacznij korzystać z aplikacji.

> [!NOTE]
> W przypadku [modyfikacji bazy danych zawijania w obrębie](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) samouczka transakcji dodaliśmy metody do TableAdapter w celu obsługi transakcji (`BeginTransaction`, `CommitTransaction`itd.). Alternatywnie można zarządzać transakcjami w całości w ramach procedury składowanej, która nie wymaga modyfikacji kodu warstwy dostępu do danych. W tym samouczku zapoznajemy polecenia języka T-SQL używane do wykonywania instrukcji składowanych procedury przechowywanej w zakresie transakcji.

## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Krok 1. Dodawanie procedur składowanych do bazy danych Northwind

Program Visual Studio ułatwia dodawanie nowych procedur składowanych do bazy danych programu. Zezwól usłudze s na dodanie nowej procedury składowanej do bazy danych Northwind, która zwraca wszystkie kolumny z tabeli `Products` dla tych, które mają określoną wartość `CategoryID`. W oknie Eksplorator serwera rozwiń bazę danych Northwind, aby wyświetlić jej foldery — diagramy bazy danych, tabele, widoki itd. Jak zostało to opisane w poprzednim samouczku, folder procedury składowane zawiera istniejące procedury składowane w bazie danych. Aby dodać nową procedurę składowaną, po prostu kliknij prawym przyciskiem myszy folder procedury składowane i wybierz opcję Dodaj nową procedurę składowaną z menu kontekstowego.

[![kliknij prawym przyciskiem myszy folder procedury składowane i Dodaj nową procedurę składowaną](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Rysunek 1**. Kliknij prawym przyciskiem myszy folder procedury składowane i Dodaj nową procedurę składowaną ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))

Jak pokazano na rysunku 1, wybranie opcji Dodaj nową procedurę składowaną powoduje otwarcie okna skryptu w programie Visual Studio z konturem skryptu SQL wymaganego do utworzenia procedury składowanej. Jest to nasze zadanie do zamiąższowania tego skryptu i wykonania go, w którym procedura składowana zostanie dodana do bazy danych.

Wprowadź następujący skrypt:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Ten skrypt, gdy zostanie wykonany, doda nową procedurę składowaną do bazy danych Northwind o nazwie `Products_SelectByCategoryID`. Ta procedura składowana akceptuje pojedynczy parametr wejściowy (`@CategoryID`typu `int`) i zwraca wszystkie pola dla tych produktów ze zgodną `CategoryID` wartością.

Aby wykonać ten skrypt `CREATE PROCEDURE` i dodać procedurę przechowywaną do bazy danych, kliknij ikonę Zapisz na pasku narzędzi lub naciśnij klawisze CTRL + S. Po wykonaniu tej czynności folder procedury składowane zostanie odświeżony, pokazując nowo utworzoną procedurę składowaną. Ponadto skrypt w oknie zmieni subtlety z `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` na `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` dodaje do bazy danych nową procedurę składowaną, podczas gdy `ALTER PROCEDURE` aktualizuje istniejącą. Ponieważ początek skryptu został zmieniony na `ALTER PROCEDURE`, zmiana parametrów wejściowych procedur składowanych lub instrukcji SQL i kliknięcie ikony Zapisz spowoduje zaktualizowanie procedury składowanej przy użyciu tych zmian.

Rysunek 2 przedstawia program Visual Studio po zapisaniu procedury składowanej `Products_SelectByCategoryID`.

[![procedury składowanej Products_SelectByCategoryID została dodana do bazy danych](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Rysunek 2**. procedura składowana `Products_SelectByCategoryID` została dodana do bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))

## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Krok 2. Konfigurowanie TableAdapter do korzystania z istniejącej procedury składowanej

Po dodaniu `Products_SelectByCategoryID` procedury składowanej do bazy danych można skonfigurować warstwę dostępu do danych w taki sposób, aby korzystała z tej procedury składowanej w przypadku wywołania jednej z jej metod. W szczególności dodamy metodę `GetProductsByCategoryID(<_i22_>categoryID)<!--_i22_-->` do `ProductsTableAdapter` w zestawie danych z `NorthwindWithSprocs` wpisanym, który wywoła `Products_SelectByCategoryID`ą procedurę składowaną.

Zacznij od otworzenia zestawu danych `NorthwindWithSprocs`. Kliknij prawym przyciskiem myszy `ProductsTableAdapter` i wybierz polecenie Dodaj zapytanie, aby uruchomić Kreatora konfiguracji zapytania TableAdapter. W [powyższym samouczku](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) TableAdapter utworzyć nową procedurę składowaną dla nas. Jednak w tym samouczku chcemy obsłużyć nową metodę TableAdapter do istniejącej procedury składowanej `Products_SelectByCategoryID`. W związku z tym wybierz opcję Użyj istniejącej procedury składowanej z pierwszego kroku kreatora, a następnie kliknij przycisk Dalej.

[![wybrać opcję Użyj istniejącej procedury składowanej](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Rysunek 3**. Wybierz opcję Użyj istniejącej procedury składowanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))

Poniższy ekran zawiera listę rozwijaną z procedurami przechowywanymi w bazie danych. Wybranie procedury składowanej powoduje wyświetlenie listy parametrów wejściowych po lewej stronie i zwracanych pól danych (jeśli istnieją) po prawej stronie. Wybierz z listy procedurę składowaną `Products_SelectByCategoryID` i kliknij przycisk Dalej.

[![wybrać Products_SelectByCategoryID procedury składowanej](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Ilustracja 4**. wybranie `Products_SelectByCategoryID` procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))

Na następnym ekranie poprosimy nas o rodzaj danych zwracanych przez procedurę składowaną i odpowiedź w tym miejscu określa typ zwracany przez metodę TableAdapter s. Jeśli na przykład wskażemy, że dane tabelaryczne są zwracane, metoda zwróci wystąpienie `ProductsDataTable` wypełnione rekordami zwróconymi przez procedurę składowaną. Z drugiej strony, jeśli wskazujemy, że ta procedura składowana zwraca pojedynczą wartość, TableAdapter zwróci `Object`, do której przypisano wartość w pierwszej kolumnie pierwszego rekordu zwróconego przez procedurę składowaną.

Ponieważ `Products_SelectByCategoryID` procedura składowana zwraca wszystkie produkty należące do określonej kategorii, wybierz pierwszą odpowiedź — dane tabelaryczne — i kliknij przycisk Dalej.

[![wskazać, że procedura składowana zwraca dane tabelaryczne](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Rysunek 5**. wskazanie, że procedura składowana zwraca dane tabelaryczne ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))

Wszystko to, które pozostało, wskazuje, jakie wzorce metody zastosować, a następnie nazwy tych metod. Pozostaw zaznaczone pole wyboru Wypełnij DataTable i wróć do opcji DataTable, ale Zmień nazwy metod na `FillByCategoryID` i `GetProductsByCategoryID`. Następnie kliknij przycisk Dalej, aby przejrzeć podsumowanie zadań wykonywanych przez kreatora. Jeśli wszystko wygląda poprawnie, kliknij przycisk Zakończ.

[![nazwy metod FillByCategoryID i GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Ilustracja 6**. nazwij metody `FillByCategoryID` i `GetProductsByCategoryID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

> [!NOTE]
> Metody TableAdapter, które właśnie utworzyliśmy, `FillByCategoryID` i `GetProductsByCategoryID`, oczekują parametru wejściowego typu `Integer`. Ta wartość parametru wejściowego jest przenoszona do procedury składowanej za pośrednictwem jej `@CategoryID` parametru. W przypadku modyfikowania parametrów `Products_SelectByCategory` procedury składowanej należy również zaktualizować parametry dla tych metod TableAdapter. Jak opisano w [poprzednim samouczku](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), można to zrobić na jeden z dwóch sposobów: przez ręczne dodanie lub usunięcie parametrów z kolekcji Parameters lub przez ponownie uruchomienie Kreatora TableAdapter.

## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Krok 3. Dodawanie metody`GetProductsByCategoryID(categoryID)`do LOGIKI biznesowej

Po zakończeniu `GetProductsByCategoryID`j metody DAL następnym krokiem jest zapewnienie dostępu do tej metody w warstwie logiki biznesowej. Otwórz plik klasy `ProductsBLLWithSprocs` i Dodaj następującą metodę:

[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Ta metoda LOGIKI biznesowej po prostu zwraca `ProductsDataTable` zwracaną z metody `GetProductsByCategoryID` `ProductsTableAdapter` s. Atrybut `DataObjectMethodAttribute` zawiera metadane używane przez Kreatora konfiguracji źródła danych programu ObjectDataSource s. W szczególności ta metoda zostanie wyświetlona na liście rozwijanej wybierz kartę s.

## <a name="step-4-displaying-products-by-category"></a>Krok 4. Wyświetlanie produktów według kategorii

Aby przetestować nowo dodaną `Products_SelectByCategoryID` procedurę składowaną oraz odpowiednie metody DAL i LOGIKI biznesowej, Zezwól na tworzenie strony ASP.NET, która zawiera DropDownList i GridView. DropDownList wyświetli wszystkie kategorie w bazie danych, podczas gdy GridView zobaczy produkty należące do wybranej kategorii.

> [!NOTE]
> Utworzyliśmy interfejsy wzorzec/szczegóły przy użyciu kontrolek DropDownList w poprzednich samouczkach. Aby uzyskać bardziej szczegółowe informacje na temat implementowania takiego raportu wzorzec/szczegóły, zobacz [filtrowanie wzorców/szczegółów za pomocą](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) samouczka DropDownList.

Otwórz stronę `ExistingSprocs.aspx` w folderze `AdvancedDAL` i przeciągnij DropDownList z przybornika do projektanta. Ustaw właściwość DropDownList s `ID` na `Categories` i jej Właściwość `AutoPostBack` na `True`. Następnie z poziomu tagu inteligentnego Powiąż DropDownList z nowym elementem ObjectDataSource o nazwie `CategoriesDataSource`. Skonfiguruj element ObjectDataSource w taki sposób, aby pobierał dane z metody `GetCategories` `CategoriesBLL` Class. Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak).

[![pobrać danych z metody GetCategories klasy CategoriesBLL](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Rysunek 7**. Pobieranie danych z metody `GetCategories` `CategoriesBLL` Class ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))

[![ustawić listy rozwijane na kartach Aktualizuj, Wstaw i Usuń do (brak).](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Ilustracja 8**. Ustawianie list rozwijanych w kartach Aktualizuj, Wstaw i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))

Po ukończeniu działania kreatora ObjectDataSourcea Skonfiguruj DropDownList do wyświetlania pola danych `CategoryName` i użyj pola `CategoryID` jako `Value` dla każdego `ListItem`u.

W tym momencie znaczniki deklaracyjne DropDownList i ObjectDataSource s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Następnie przeciągnij widok GridView do projektanta, umieszczając go poniżej DropDownList. Ustaw `ID` GridView s na `ProductsByCategory` i, z jego tagu inteligentnego, powiąż go z nowym elementem ObjectDataSource o nazwie `ProductsByCategoryDataSource`. Skonfiguruj `ProductsByCategoryDataSource` element ObjectDataSource, aby używał klasy `ProductsBLLWithSprocs`, która pobiera swoje dane przy użyciu metody `GetProductsByCategoryID(categoryID)`. Ponieważ ten element GridView będzie używany tylko do wyświetlania danych, Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak) i kliknij przycisk Dalej.

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Ilustracja 9**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLLWithSprocs` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))

[![pobrać danych z metody GetProductsByCategoryID (IDKategorii)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Ilustracja 10**. Pobieranie danych z metody `GetProductsByCategoryID(categoryID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))

Metoda wybrana na karcie Wybierz oczekuje parametru, więc ostatni krok kreatora poprosi nas o Źródło parametru s. Ustaw listę rozwijaną Źródło parametru na kontrolkę i wybierz formant `Categories` z listy rozwijanej ControlID. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![używać kategorii DropDownList jako źródła parametru IDKategorii](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Ilustracja 11**. Użyj `Categories` DropDownList jako źródła parametru `categoryID` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

Po zakończeniu działania kreatora ObjectDataSource Visual Studio doda BoundFields oraz CheckBoxField dla każdego z pól danych produktu. Możesz dowolnie dostosowywać te pola zgodnie z potrzebami.

Odwiedź stronę za pomocą przeglądarki. Podczas odwiedzania strony wybrano kategorię napoje i odpowiadające im produkty wymienione w siatce. Zmiana listy rozwijanej na kategorię alternatywną, jak pokazano na rysunku 12, powoduje odświeżenie i ponowne załadowanie siatki z produktami z nowo wybranej kategorii.

[![są wyświetlane produkty z kategorii produkcji](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Rysunek 12**. wyświetlane są produkty z kategorii produkcja ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))

## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Krok 5. Zawijanie instrukcji procedury składowanej w zakresie transakcji

W przypadku [modyfikacji bazy danych opakowującego w ramach transakcji](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) omawiamy techniki wykonywania serii instrukcji modyfikacji bazy danych w zakresie transakcji. Odwołaj, że modyfikacje wykonywane w ramach parasola transakcji kończą się powodzeniem lub wszystkie niepowodzeniem, gwarantując niepodzielność. Metody korzystania z transakcji obejmują:

- Korzystając z klas w przestrzeni nazw `System.Transactions`,
- Aby Warstwa dostępu do danych korzystała z klas ADO.NET, takich jak `SqlTransaction`, i
- Dodawanie poleceń transakcji T-SQL bezpośrednio w procedurze składowanej

*Modyfikacje zawijania bazy danych w ramach* samouczka transakcji używały klas ADO.NET w dal. W pozostałej części tego samouczka pokazano, jak zarządzać transakcjami przy użyciu poleceń T-SQL w ramach procedury składowanej.

Trzy kluczowe polecenia SQL do ręcznego uruchamiania, zatwierdzania i wycofywania transakcji są odpowiednio `BEGIN TRANSACTION`, `COMMIT TRANSACTION`i `ROLLBACK TRANSACTION`. Podobnie jak w przypadku metody ADO.NET, w przypadku korzystania z transakcji z poziomu procedury składowanej musimy zastosować następujący wzorzec:

1. Wskaż początek transakcji.
2. Wykonaj instrukcje SQL, które składają się na transakcję.
3. Jeśli wystąpi błąd w jednej z instrukcji w kroku 2, wycofaj tę transakcję.
4. Jeśli wszystkie instrukcje z kroku 2 zakończą pracę bez błędu, Zatwierdź transakcję.

Ten wzorzec można zaimplementować w składni języka T-SQL przy użyciu następującego szablonu:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Szablon zaczyna się od zdefiniowania bloku `TRY...CATCH`, a konstrukcja New to SQL Server 2005. Podobnie jak w przypadku bloków `Try...Catch` w Visual Basic, blok `TRY...CATCH` SQL wykonuje instrukcje w bloku `TRY`. Jeśli jakakolwiek instrukcja zgłasza błąd, formant zostanie natychmiast przeniesiony do bloku `CATCH`.

Jeśli nie występują żadne błędy wykonywania instrukcji SQL, które korzeń transakcji, instrukcja `COMMIT TRANSACTION` zatwierdza zmiany i kończy transakcję. Jeśli jednak jedna z instrukcji powoduje błąd, `ROLLBACK TRANSACTION` w bloku `CATCH` zwraca bazę danych do jej stanu przed rozpoczęciem transakcji. Procedura składowana wywołuje również błąd przy użyciu [polecenia RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), które powoduje, że `SqlException` zostać zgłoszone w aplikacji.

> [!NOTE]
> Ponieważ blok `TRY...CATCH` jest nowy do SQL Server 2005, powyższy szablon nie będzie działał, jeśli używasz starszych wersji programu Microsoft SQL Server. Jeśli nie korzystasz z SQL Server 2005, zapoznaj się z tematem [Zarządzanie transakcjami w SQL Server procedurach składowanych](http://www.4guysfromrolla.com/webtech/080305-1.shtml) szablonu, który będzie współpracować z innymi wersjami SQL Server.

Poszukaj w konkretnym przykładzie. Istnieje ograniczenie klucza obcego między tabelami `Categories` i `Products`, co oznacza, że każde pole `CategoryID` w tabeli `Products` musi mapować do wartości `CategoryID` w tabeli `Categories`. Każda akcja, która spowodowałaby naruszenie tego ograniczenia, taka jak próba usunięcia kategorii, która zawiera skojarzone produkty, powoduje naruszenie ograniczenia klucza obcego. Aby to sprawdzić, ponownie odwiedź przykład aktualizacji i usuwania istniejących danych binarnych w sekcji Praca z danymi binarnymi (`~/BinaryData/UpdatingAndDeleting.aspx`). Ta strona zawiera listę wszystkich kategorii w systemie wraz z przyciskami Edytuj i Usuń (Zobacz Rysunek 13), ale w przypadku próby usunięcia kategorii zawierającej powiązane produkty, takie jak napoje — usunięcie nie powiedzie się z powodu naruszenia ograniczenia klucza obcego (patrz rysunek 14).

[![każda kategoria jest wyświetlana w widoku GridView z przyciskami Edytuj i Usuń](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Ilustracja 13**. Każda kategoria jest wyświetlana w widoku GridView z przyciskami Edytuj i Usuń ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))

[![nie można usunąć kategorii, która ma istniejące produkty](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Ilustracja 14**. nie można usunąć kategorii zawierającej istniejące produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))

Wyobraź sobie, że chcemy zezwolić na usuwanie kategorii bez względu na to, czy mają one skojarzone produkty. Jeśli kategoria z produktami zostanie usunięta, Załóżmy, że chcemy również usunąć istniejące produkty (mimo że inne opcje byłyby po prostu ustawiać jej produkty `CategoryID` wartości `NULL`). Tę funkcję można zaimplementować za pomocą reguł kaskadowych ograniczenia klucza obcego. Alternatywnie możemy utworzyć procedurę składowaną, która akceptuje `@CategoryID` parametr wejściowy i, po wywołaniu, jawnie usuwa wszystkie skojarzone produkty, a następnie określoną kategorię.

Pierwsza próba wykonania tej procedury składowanej może wyglądać następująco:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

W związku z tym ostatecznie usuniesz skojarzone produkty i kategorię, nie robi to w ramach parasola transakcji. Załóżmy, że istnieją inne ograniczenia klucza obcego dotyczące `Categories`, które mogłyby uniemożliwić usunięcie określonej `@CategoryID` wartości. Problem polega na tym, że w takim przypadku wszystkie produkty zostaną usunięte przed podjęciem próby usunięcia kategorii. Wynikiem działania netto jest to, że w przypadku takiej kategorii ta procedura składowana usunie wszystkie swoje produkty, gdy Kategoria pozostała, ponieważ nadal ma powiązane rekordy w innej tabeli.

Jeśli procedura składowana została opakowana w ramach zakresu transakcji, usunięcie z tabeli `Products` zostanie cofnięte w wyniku nieudanego usunięcia w `Categories`. Poniższy skrypt procedury składowanej używa transakcji, aby zapewnić niepodzielność między dwiema `DELETE` instrukcjami:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Poświęć chwilę na dodanie procedury składowanej `Categories_Delete` do bazy danych Northwind. Zapoznaj się z artykułem krok 1, aby uzyskać instrukcje dotyczące dodawania procedur składowanych do bazy danych programu.

## <a name="step-6-updating-thecategoriestableadapter"></a>Krok 6. aktualizowanie`CategoriesTableAdapter`

Po dodaniu `Categories_Delete` procedury składowanej do bazy danych DAL jest obecnie skonfigurowana do wykonywania usuwania przy użyciu instrukcji SQL ad hoc. Musimy zaktualizować `CategoriesTableAdapter` i poinstruować, aby zamiast tego użyć procedury składowanej `Categories_Delete`.

> [!NOTE]
> Wcześniej w tym samouczku pracowałem z zestawem danych `NorthwindWithSprocs`. Jednak ten zestaw danych ma tylko jedną jednostkę, `ProductsDataTable`i musimy współpracować z kategoriami. W związku z tym w pozostałej części tego samouczka, gdy porozmawiamy o warstwie dostępu do danych I m odniesieniu do `Northwind` zestawu danych, ten, który został najpierw utworzony w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) .

Otwórz zestaw danych Northwind, wybierz `CategoriesTableAdapter`i przejdź do okno Właściwości. Okno Właściwości wyświetla listę `InsertCommand`, `UpdateCommand`, `DeleteCommand`i `SelectCommand` używanych przez TableAdapter, a także informacje o nazwie i połączeniu. Rozwiń Właściwość `DeleteCommand`, aby wyświetlić jej szczegóły. Jak pokazano na rysunku 15, właściwość `CommandType` `DeleteCommand` s ma wartość text, która instruuje ją do wysłania tekstu z właściwości `CommandText` jako zapytania ad-hoc.

![Wybierz CategoriesTableAdapter w projektancie, aby wyświetlić jego właściwości w oknie właściwości](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Ilustracja 15**. Wybierz `CategoriesTableAdapter` w projektancie, aby wyświetlić jego właściwości w oknie właściwości

Aby zmienić te ustawienia, wybierz tekst (DeleteCommand) w okno Właściwości i wybierz pozycję (nowy) z listy rozwijanej. Spowoduje to wyczyszczenie ustawień właściwości `CommandText`, `CommandType`i `Parameters`. Następnie ustaw właściwość `CommandType` na `StoredProcedure`, a następnie wpisz nazwę procedury składowanej dla `CommandText` (`dbo.Categories_Delete`). Jeśli Pamiętaj o wprowadzeniu właściwości w tej kolejności — najpierw `CommandType` a następnie `CommandText` — Visual Studio automatycznie wypełni kolekcję parametrów. Jeśli nie wprowadzisz tych właściwości w tej kolejności, konieczne będzie ręczne dodanie parametrów za pomocą edytora kolekcji parametrów. W obu przypadkach ostrożnie klikać wielokropek we właściwości Parameters, aby wyświetlić Edytor kolekcji parametrów w celu sprawdzenia, czy wprowadzono poprawne zmiany ustawień parametrów (zobacz rysunek 16). Jeśli nie widzisz żadnych parametrów w oknie dialogowym, Dodaj `@CategoryID` parametr ręcznie (nie musisz dodawać `@RETURN_VALUE` parametru).

![Upewnij się, że ustawienia parametrów są poprawne](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Ilustracja 16**. Upewnij się, że ustawienia parametrów są poprawne

Po aktualizacji DAL usunięcie kategorii spowoduje automatyczne usunięcie wszystkich skojarzonych z nią produktów i wykonanie tej czynności w ramach parasola transakcji. Aby to sprawdzić, Wróć do strony aktualizowanie i usuwanie istniejących danych binarnych, a następnie kliknij przycisk Usuń dla jednej z kategorii. Jednym kliknięciem myszy, Kategoria i wszystkie skojarzone produkty zostaną usunięte.

> [!NOTE]
> Przed przetestowaniem `Categories_Delete` procedury składowanej, która spowoduje usunięcie wielu produktów wraz z wybraną kategorią, warto ostrożnie utworzyć kopię zapasową bazy danych. Jeśli używasz bazy danych `NORTHWND.MDF` w `App_Data`, po prostu Zamknij program Visual Studio i skopiuj pliki MDF i LDF w `App_Data` do innego folderu. Po przetestowaniu funkcji można przywrócić bazę danych, zamykając program Visual Studio i zastępując bieżące pliki MDF i LDF w `App_Data` przy użyciu kopii zapasowych.

## <a name="summary"></a>Podsumowanie

Mimo że Kreator TableAdapter s automatycznie generuje procedury składowane, istnieją sytuacje, w których firma Microsoft może już utworzyć te procedury składowane lub chcieć utworzyć je ręcznie lub przy użyciu innych narzędzi. Aby obsłużyć takie scenariusze, TableAdapter można również skonfigurować tak, aby wskazywał istniejącą procedurę składowaną. W tym samouczku pokazano, jak ręcznie dodać procedury składowane do bazy danych za pomocą środowiska programu Visual Studio i jak obsłużyć metody TableAdapter s do tych procedur składowanych. Zbadamy również polecenia i wzorzec skryptu T-SQL służący do uruchamiania, zatwierdzania i wycofywania transakcji z poziomu procedury składowanej.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Hilton Geisenow, S Ren Jacob Lauritsen i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [dalej](updating-the-tableadapter-to-use-joins-vb.md)
