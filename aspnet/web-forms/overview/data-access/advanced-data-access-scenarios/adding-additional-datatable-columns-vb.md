---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Dodawanie dodatkowych kolumn DataTable (VB) | Microsoft Docs
author: rick-anderson
description: W przypadku tworzenia typu zestawu danych przy użyciu kreatora TableAdapter odpowiadający mu element DataTable zawiera kolumny zwrócone przez zapytanie głównej bazy danych. Jednak...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 3a55f8bc4d3508387927ca81674073a001867de7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78534105"
---
# <a name="adding-additional-datatable-columns-vb"></a>Dodawanie dodatkowych kolumn tabeli DataTable (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) lub [Pobierz plik PDF](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> W przypadku tworzenia typu zestawu danych przy użyciu kreatora TableAdapter odpowiadający mu element DataTable zawiera kolumny zwrócone przez zapytanie głównej bazy danych. Zdarza się jednak, że element DataTable musi zawierać dodatkowe kolumny. W tym samouczku dowiesz się, dlaczego procedury składowane są zalecane, gdy potrzebne są dodatkowe kolumny tabeli DataTable.

## <a name="introduction"></a>Wprowadzenie

Podczas dodawania TableAdapter do określonego zestawu danych, odpowiadający schemat DataTable jest określany przez zapytanie główne TableAdapter s. Na przykład, jeśli zapytanie główne zwraca pola danych *a*, *b*i *c*, element DataTable będzie miał trzy odpowiadające im kolumny o nazwie *a*, *b*i *c*. Oprócz jego głównego zapytania TableAdapter może zawierać dodatkowe zapytania, które zwracają, ewentualnie, podzestaw danych na podstawie pewnego parametru. Przykładowo oprócz zapytania głównego `ProductsTableAdapter` s, które zwraca informacje o wszystkich produktach, zawiera również metody takie jak `GetProductsByCategoryID(categoryID)` i `GetProductByProductID(productID)`, które zwracają informacje o produkcie na podstawie dostarczonego parametru.

Model, w którym schemat elementu DataTable s odzwierciedla zapytanie główne TableAdapter s, działa prawidłowo, jeśli wszystkie metody TableAdapter s zwracają te same lub mniejsze pola danych niż określone w zapytaniu głównym. Jeśli metoda TableAdapter musi zwrócić dodatkowe pola danych, należy odpowiednio rozwinąć schemat DataTable. W obszarze [wzorzec/szczegóły przy użyciu listy punktowanej rekordów głównych z szczegółowym](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) samouczekem elementu DataList dodaliśmy metodę do `CategoriesTableAdapter`, która zwróciła `CategoryID`, `CategoryName`i `Description` pól danych zdefiniowanych w zapytaniu głównym i `NumberOfProducts`, dodatkowe pole danych, które zgłosiło liczbę produktów skojarzonych z każdą kategorią. Dodano ręcznie nową kolumnę do `CategoriesDataTable`, aby przechwycić wartość pola dane `NumberOfProducts` z tej nowej metody.

Zgodnie z opisem w samouczku [przekazywanie plików](../working-with-binary-files/uploading-files-vb.md) należy zwrócić szczególną uwagę na TableAdapters, które korzystają z instrukcji SQL ad hoc i zawierają metody, których pola danych nie pasują dokładnie do głównej kwerendy. Jeśli Kreator konfiguracji TableAdapter jest ponownie uruchamiany, zaktualizuje wszystkie metody TableAdapter, tak aby ich lista pól danych była zgodna z główną kwerendą. W związku z tym wszystkie metody z dostosowanymi listami kolumn zostaną przywrócone do głównej listy kolumn zapytania i nie zwróci oczekiwanych danych. Ten problem nie występuje w przypadku korzystania z procedur składowanych.

W tym samouczku dowiesz się, jak zwiększyć schemat DataTable s w celu uwzględnienia dodatkowych kolumn. Ze względu na brittleness TableAdapter w przypadku korzystania z instrukcji SQL ad hoc w tym samouczku będziemy używać procedur składowanych. Aby uzyskać więcej informacji na temat konfigurowania TableAdapter w celu korzystania z procedur składowanych, zapoznaj się z tematem [Tworzenie nowych procedur składowanych dla wpisanych zestawów danych s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) i [używanie istniejących procedur składowanych dla wpisanych zestawów danych s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Krok 1. Dodawanie kolumny`PriceQuartile`do`ProductsDataTable`

W oknie *Tworzenie nowych procedur składowanych dla wpisanych danych TableAdapters* samouczek został utworzony zestaw danych o określonym typie o nazwie `NorthwindWithSprocs`. Ten zestaw danych zawiera obecnie dwa tabele: `ProductsDataTable` i `EmployeesDataTable`. `ProductsTableAdapter` ma trzy następujące metody:

- `GetProducts` — główne zapytanie, które zwraca wszystkie rekordy z tabeli `Products`
- `GetProductsByCategoryID(categoryID)` — zwraca wszystkie produkty z określonym *IDkategorii*.
- `GetProductByProductID(productID)` — zwraca konkretny produkt z określonym identyfikatorem *ProductID*.

Główne zapytanie i dwie dodatkowe metody zwracają ten sam zestaw pól danych, a mianowicie wszystkie kolumny z tabeli `Products`. Brak skorelowanych podkwerend lub `JOIN` s ściągające powiązane dane z tabel `Categories` lub `Suppliers`. W związku z tym `ProductsDataTable` ma odpowiadającą kolumnę dla każdego pola w tabeli `Products`.

Na potrzeby tego samouczka Dodaj metodę do `ProductsTableAdapter` o nazwie `GetProductsWithPriceQuartile`, która zwraca wszystkie produkty. Oprócz standardowych pól danych produktu, `GetProductsWithPriceQuartile` również zawiera pole danych `PriceQuartile`, które wskazuje, na podstawie którego kwartyl cena produktu jest równa. Na przykład te produkty, których ceny znajdują się w najtańszym 25%, będą miały `PriceQuartile` wartość 1, natomiast te, których ceny przypadają na dole 25%, będą mieć wartość 4. Zanim przejdziemy do utworzenia procedury składowanej w celu zwrócenia tych informacji, najpierw musimy zaktualizować `ProductsDataTable`, aby uwzględnić kolumnę do przechowywania `PriceQuartile` wyników, gdy zostanie użyta metoda `GetProductsWithPriceQuartile`.

Otwórz zestaw danych `NorthwindWithSprocs` i kliknij prawym przyciskiem myszy `ProductsDataTable`. Wybierz pozycję Dodaj z menu kontekstowego, a następnie wybierz kolumnę.

[![dodać nowej kolumny do ProductsDataTable](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Rysunek 1**. Dodawanie nowej kolumny do `ProductsDataTable` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image3.png))

Spowoduje to dodanie nowej kolumny do elementu DataTable o nazwie Kolumna1 typu `System.String`. Musimy zaktualizować tę nazwę kolumny s do PriceQuartile i jej typ `System.Int32`, ponieważ będzie ona używana do przechowywania liczby z zakresu od 1 do 4. Wybierz nowo dodaną kolumnę w `ProductsDataTable` i, w okno Właściwości, ustaw właściwość `Name` na PriceQuartile oraz Właściwość `DataType` na `System.Int32`.

[![ustawić nową nazwę kolumny s i właściwości DataType](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Rysunek 2**: ustaw nową kolumnę s `Name` i `DataType` właściwości ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image6.png))

Jak pokazano na rysunku 2, istnieją dodatkowe właściwości, które można ustawić, na przykład czy wartości w kolumnie muszą być unikatowe, jeśli kolumna jest kolumną typu autoprzyrost, niezależnie od tego, czy wartości `NULL` bazy danych są dozwolone i tak dalej. Pozostaw te wartości domyślne.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Krok 2. Tworzenie metody`GetProductsWithPriceQuartile`

Teraz, gdy `ProductsDataTable` zostało zaktualizowane w celu uwzględnienia kolumny `PriceQuartile`, jesteśmy gotowi do utworzenia metody `GetProductsWithPriceQuartile`. Zacznij od kliknięcia prawym przyciskiem myszy TableAdapter i wybierz polecenie Dodaj zapytanie z menu kontekstowego. Spowoduje to wyświetlenie Kreatora konfiguracji zapytania TableAdapter, który najpierw powiadamia nas o tym, czy chcemy użyć instrukcji SQL ad hoc, czy nowej lub istniejącej procedury składowanej. Ponieważ nie mamy jeszcze procedury składowanej, która zwraca dane kwartyl ceny, zezwól TableAdapter na utworzenie tej procedury składowanej. Wybierz opcję Utwórz nową procedurę składowaną, a następnie kliknij przycisk Dalej.

[![nakazać kreatorowi TableAdapter utworzenie procedury składowanej dla nas](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Rysunek 3**. nakazuje kreatorowi TableAdapter utworzenie procedury składowanej dla nas ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image9.png))

Na następnym ekranie przedstawionym na rysunku 4 Kreator pyta, jaki typ zapytania dodać. Ponieważ metoda `GetProductsWithPriceQuartile` zwróci wszystkie kolumny i rekordy z tabeli `Products`, zaznacz opcję Wybierz, która zwraca wiersze, a następnie kliknij przycisk Dalej.

[![zapytanie będzie instrukcją SELECT, która zwraca wiele wierszy](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Ilustracja 4**. zapytanie będzie `SELECT` instrukcją zwracającą wiele wierszy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image12.png))

Następnie zostanie wyświetlony monit o zapytanie `SELECT`. W Kreatorze wprowadź następujące zapytanie:

[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

Powyższe zapytanie używa SQL Server 2005 s nowej [funkcji`NTILE`](https://msdn.microsoft.com/library/ms175126.aspx) , aby podzielić wyniki do czterech grup, w których grupy są określane przez `UnitPrice` wartości sortowane w kolejności malejącej.

Niestety Konstruktor zapytań nie wie, jak przeanalizować słowa kluczowego `OVER` i zostanie wyświetlony błąd podczas analizowania powyższego zapytania. W związku z tym Wprowadź powyższe zapytanie bezpośrednio w polu tekstowym kreatora, nie używając Konstruktor zapytań.

> [!NOTE]
> Aby uzyskać więcej informacji na temat NTILE i SQL Server 2005 innych funkcji klasyfikacji, zobacz [zwracają rankingowe wyniki z Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) i [sekcji funkcje klasyfikacji](https://msdn.microsoft.com/library/ms189798.aspx) w [SQL Server 2005 Books Online](https://msdn.microsoft.com/library/ms189798.aspx).

Po wprowadzeniu kwerendy `SELECT` i kliknięciu przycisku Dalej Kreator poprosi nas o podanie nazwy procedury składowanej, która zostanie utworzona. Nazwij nową procedurę składowaną `Products_SelectWithPriceQuartile` a następnie kliknij przycisk Dalej.

[![nazwę procedury składowanej Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Rysunek 5**. Nazwij `Products_SelectWithPriceQuartile` procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image15.png))

Na koniec zostanie wyświetlony monit o podanie nazwy metod TableAdapter. Pozostaw zaznaczone pole wyboru Wypełnij DataTable i wróć do nich elementy CheckBox, a następnie nadaj im nazwę metody `FillWithPriceQuartile` i `GetProductsWithPriceQuartile`.

[![Nazwij metody TableAdapter s i kliknij przycisk Zakończ.](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Ilustracja 6**. Nazwij metody TableAdapter s i kliknij przycisk Zakończ ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image18.png))

Po określeniu zapytania `SELECT` i procedurze składowanej oraz metodach TableAdapter o nazwie kliknij przycisk Zakończ, aby zakończyć pracę kreatora. W tym momencie może pojawić się ostrzeżenie lub dwa z kreatora z informacją, że `OVER` konstrukcja lub instrukcja SQL nie jest obsługiwana. Te ostrzeżenia można zignorować.

Po zakończeniu działania kreatora TableAdapter powinien zawierać metody `FillWithPriceQuartile` i `GetProductsWithPriceQuartile`, a baza danych powinna zawierać procedurę przechowywaną o nazwie `Products_SelectWithPriceQuartile`. Poświęć chwilę, aby sprawdzić, czy TableAdapter rzeczywiście zawiera tę nową metodę i czy procedura składowana została prawidłowo dodana do bazy danych. Jeśli podczas sprawdzania bazy danych nie widzisz procedury składowanej, kliknij prawym przyciskiem myszy folder procedury składowane i wybierz polecenie Odśwież.

![Sprawdź, czy nowa metoda została dodana do TableAdapter](adding-additional-datatable-columns-vb/_static/image19.png)

**Rysunek 7**. Sprawdzanie, czy nowa metoda została dodana do TableAdapter

[![upewnij się, że baza danych zawiera Products_SelectWithPriceQuartile procedury składowanej](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Ilustracja 8**. Upewnij się, że baza danych zawiera procedurę składowaną `Products_SelectWithPriceQuartile` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image22.png))

> [!NOTE]
> Jedną z korzyści wynikających z używania procedur składowanych zamiast instrukcji SQL ad hoc jest to, że ponowne uruchomienie Kreatora konfiguracji TableAdapter nie spowoduje modyfikacji list kolumn procedury składowane. Sprawdź to, klikając prawym przyciskiem myszy TableAdapter, wybierając opcję Konfiguruj z menu kontekstowego, aby uruchomić kreatora, a następnie kliknąć przycisk Zakończ, aby zakończyć pracę. Następnie przejdź do bazy danych i zapoznaj się z `Products_SelectWithPriceQuartile` procedurą składowaną. Należy pamiętać, że lista kolumn nie została zmodyfikowana. Czy korzystamy z instrukcji SQL ad hoc, ponowne uruchomienie Kreatora konfiguracji TableAdapter spowodowałoby przywrócenie listy kolumn zapytania s w celu dopasowania jej do głównej listy kolumn zapytania, a tym samym usunięcie instrukcji NTILE z zapytania używanego przez metodę `GetProductsWithPriceQuartile`.

Gdy metoda `GetProductsWithPriceQuartile` warstwy dostępu do danych jest wywoływana, TableAdapter wykonuje `Products_SelectWithPriceQuartile` procedurę składowaną i dodaje wiersz do `ProductsDataTable` dla każdego zwróconego rekordu. Pola danych zwracane przez procedurę składowaną są mapowane na kolumny `ProductsDataTable` s. Ponieważ istnieje pole danych `PriceQuartile` zwrócone z procedury składowanej, jego wartość jest przypisana do kolumny `ProductsDataTable` s `PriceQuartile`.

Dla tych metod TableAdapter, których zapytania nie zwracają pola danych `PriceQuartile`, wartość `PriceQuartile` kolumna s jest wartością określoną przez `DefaultValue` właściwości. Jak pokazano na rysunku 2, ta wartość jest ustawiana na `DBNull`, domyślnie. Jeśli wolisz inną wartość domyślną, po prostu Ustaw odpowiednio Właściwość `DefaultValue`. Upewnij się, że wartość `DefaultValue` jest prawidłowa, przyjmując kolumnę s `DataType` (tj. `System.Int32` dla kolumny `PriceQuartile`).

W tym momencie wykonamy kroki niezbędne do dodania dodatkowej kolumny do elementu DataTable. Aby sprawdzić, czy ta dodatkowa kolumna działa zgodnie z oczekiwaniami, należy utworzyć stronę ASP.NET, która wyświetla wszystkie nazwy produktów, ceny i kwartyl cen. Przed wykonaniem tej czynności najpierw należy zaktualizować warstwę logiki biznesowej w celu uwzględnienia metody wywołującej metodę DAL `GetProductsWithPriceQuartile`. Będziemy aktualizować LOGIKI biznesowej dalej, w kroku 3, a następnie utworzyć stronę ASP.NET w kroku 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Krok 3. Rozszerzanie warstwy logiki biznesowej

Przed użyciem nowej metody `GetProductsWithPriceQuartile` z warstwy prezentacji należy najpierw dodać odpowiednią metodę do LOGIKI biznesowej. Otwórz plik klasy `ProductsBLLWithSprocs` i Dodaj następujący kod:

[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Podobnie jak w przypadku innych metod pobierania danych w `ProductsBLLWithSprocs`, Metoda `GetProductsWithPriceQuartile` po prostu wywołuje metodę `GetProductsWithPriceQuartile` odpowiadającą DAL i zwraca wyniki.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Krok 4. Wyświetlanie informacji o kwartyl cen na stronie sieci Web ASP.NET

Po dodaniu LOGIKI BIZNESOWEJu wszystko gotowe do utworzenia strony ASP.NET, która pokazuje KWARTYL cenowe dla każdego produktu. Otwórz stronę `AddingColumns.aspx` w folderze `AdvancedDAL` i przeciągnij widok GridView z przybornika do projektanta, ustawiając jego właściwość `ID` na `Products`. Z tagu inteligentnego GridView s powiąż go z nowym elementem ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj element ObjectDataSource, aby używał metody `GetProductsWithPriceQuartile` `ProductsBLLWithSprocs` Class. Ponieważ będzie to siatka tylko do odczytu, Ustaw listę rozwijaną na kartach Aktualizuj, Wstaw i Usuń na wartość (brak).

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLLWithSprocs](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Ilustracja 9**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLLWithSprocs` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image25.png))

[![pobrać informacji o produkcie z metody GetProductsWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Ilustracja 10**. Pobieranie informacji o produkcie z metody `GetProductsWithPriceQuartile` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image28.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio automatycznie doda element BoundField lub CheckBoxField do widoku GridView dla każdego z pól danych zwracanych przez metodę. Jedno z tych pól danych jest `PriceQuartile`, które jest kolumną dodaną do `ProductsDataTable` w kroku 1.

Edytuj pola GridView, usuwając wszystkie elementy oprócz `ProductName`, `UnitPrice`i `PriceQuartile` BoundFields. Skonfiguruj `UnitPrice` BoundField, aby sformatować jego wartość jako walutę i mieć odpowiednio `UnitPrice` i `PriceQuartile` BoundFields i wyrównane do prawej strony. Na koniec zaktualizuj odpowiednio pozostałe właściwości `HeaderText` BoundFields do produktu, ceny i kwartyl. Zaznacz również pole wyboru Włącz sortowanie w tagu inteligentnym GridView.

Po wprowadzeniu tych zmian znaczniki deklaratywne GridView i ObjectDataSource s powinny wyglądać następująco:

[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

Rysunek 11 przedstawia Tę stronę w przeglądarce. Należy pamiętać, że początkowo produkty są uporządkowane według ich cen w kolejności malejącej, przy czym każdy produkt ma przypisaną odpowiednią wartość `PriceQuartile`. Oczywiście te dane mogą być sortowane według innych kryteriów z wartością kolumny kwartyl, która nadal odzwierciedla klasyfikację produktu w odniesieniu do ceny (patrz rysunek 12).

[![produkty są uporządkowane według ich cen](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Ilustracja 11**. produkty są uporządkowane według ich cen ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image31.png))

[![produkty są uporządkowane według ich nazw](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Ilustracja 12**. produkty są uporządkowane według ich nazw ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image34.png))

> [!NOTE]
> Za pomocą kilku wierszy kodu możemy rozszerzyć widok GridView, aby kolorować wiersze produktu na podstawie ich wartości `PriceQuartile`. Możemy pokolorować te produkty w pierwszej kwartyl w kolorze zielonym, które w drugim kwartyl są żółte i tak dalej. Zachęcam do Poświęć chwilę na dodanie tej funkcji. Jeśli potrzebujesz odświeżacza na potrzeby formatowania widoku GridView, zapoznaj się z tematem [formatowanie niestandardowe na podstawie](../custom-formatting/custom-formatting-based-upon-data-vb.md) samouczka dotyczącego danych.

## <a name="an-alternative-approach---creating-another-tableadapter"></a>Alternatywne podejście — Tworzenie innego TableAdapter

Jak widać w tym samouczku, podczas dodawania metody do TableAdapter, która zwraca pola danych inne niż te, które są wpisane przez zapytanie główne, możemy dodać odpowiednie kolumny do tabeli DataTable. Takie podejście jednak również działa tylko wtedy, gdy istnieje niewielka liczba metod w TableAdapter, które zwracają różne pola danych i jeśli te alternatywne pola danych nie różnią się zbyt wiele od głównej kwerendy.

Zamiast dodawać kolumny do tabeli DataTable, można dodać kolejną TableAdapter do zestawu danych, który zawiera metody z pierwszego TableAdapter, które zwracają różne pola danych. W tym samouczku zamiast dodawania kolumny `PriceQuartile` do `ProductsDataTable` (gdzie jest używana tylko przez metodę `GetProductsWithPriceQuartile`), możemy dodać dodatkowe TableAdapter do zestawu danych o nazwie `ProductsWithPriceQuartileTableAdapter`, który użył procedury składowanej `Products_SelectWithPriceQuartile` jako głównej kwerendy. ASP.NET strony, które są konieczne do uzyskania informacji o produkcie, użyj `ProductsWithPriceQuartileTableAdapter`, podczas gdy te, które nie mogły nadal korzystać z `ProductsTableAdapter`.

Po dodaniu nowego TableAdapter, tabele danych pozostają untarnished i ich kolumny są precyzyjnie dublowane, zwracane przez ich metody TableAdapter s. Jednak dodatkowe TableAdapters mogą wprowadzać powtarzające się zadania i funkcje. Na przykład, jeśli te strony ASP.NET, które wyświetlają kolumnę `PriceQuartile`, również potrzebne do zapewnienia obsługi instrukcji INSERT, Update i DELETE, `ProductsWithPriceQuartileTableAdapter` muszą mieć prawidłowo skonfigurowane właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand`. Chociaż te właściwości spowodują odbicie `ProductsTableAdapter` s, ta konfiguracja wprowadza dodatkowy krok. Ponadto istnieją dwa sposoby aktualizowania, usuwania lub dodawania produktu do bazy danych — za pomocą klas `ProductsTableAdapter` i `ProductsWithPriceQuartileTableAdapter`.

Pobranie dla tego samouczka zawiera klasę `ProductsWithPriceQuartileTableAdapter` w `NorthwindWithSprocs` zestawie danych, który ilustruje to alternatywne podejście.

## <a name="summary"></a>Podsumowanie

W większości scenariuszy wszystkie metody w TableAdapter będą zwracać ten sam zestaw pól danych, ale istnieją sytuacje, w których dana metoda lub dwie mogą wymagać zwrócenia pola dodatkowego. Na przykład w przypadku [wzorca/szczegółów przy użyciu listy punktowanej rekordów głównych z szczegółowym](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) samouczekem elementu DataList dodaliśmy metodę do `CategoriesTableAdapter`, która oprócz pól danych głównego zapytania nie zwróciła pola `NumberOfProducts`, które zgłosiło liczbę produktów skojarzonych z każdą kategorią. W tym samouczku dodaliśmy metodę dodawania metody do `ProductsTableAdapter`, która zwróciła pole `PriceQuartile` jako uzupełnienie głównych pól danych zapytania. Aby przechwytywać dodatkowe pola danych zwracane przez metody TableAdapter, należy dodać odpowiednie kolumny do elementu DataTable.

Jeśli planujesz Ręczne dodawanie kolumn do elementu DataTable, zaleca się, aby TableAdapter używać procedur składowanych. Jeśli TableAdapter używa instrukcji ad-hoc SQL, za każdym razem, gdy Kreator konfiguracji TableAdapter zostanie uruchomiony wszystkie listy pól danych metody Przywróć do pól danych zwracanych przez zapytanie główne. Ten problem nie obejmuje procedur składowanych, dlatego zaleca się ich użycie w tym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Randy Schmidt, Jacky Goor, Bernadette Leigh i Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](updating-the-tableadapter-to-use-joins-vb.md)
> [dalej](working-with-computed-columns-vb.md)
