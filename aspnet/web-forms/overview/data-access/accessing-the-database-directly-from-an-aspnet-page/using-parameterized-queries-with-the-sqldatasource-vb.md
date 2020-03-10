---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: Używanie zapytań parametrycznych z kontrolki SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku będziemy nadal korzystać z kontrolki kontrolki SqlDataSource i dowiedzieć się, jak definiować zapytania parametryczne. Parametry można określić zarówno w dekla...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 19b93ff6c0878ae6ed546d347cafef95fd2a01e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78552326"
---
# <a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>Używanie zapytań sparametryzowanych z kontrolką SqlDataSource (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) lub [Pobierz plik PDF](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> W tym samouczku będziemy nadal korzystać z kontrolki kontrolki SqlDataSource i dowiedzieć się, jak definiować zapytania parametryczne. Parametry można określać jednocześnie i programowo, a także pobierać z wielu lokalizacji, takich jak QueryString, stan sesji, inne kontrolki i inne.

## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku pokazano, jak za pomocą formantu kontrolki SqlDataSource pobrać dane bezpośrednio z bazy danych. Korzystając z Kreatora konfiguracji źródła danych, możemy wybrać bazę danych, a następnie: wybrać kolumny do zwrócenia z tabeli lub widoku; Wprowadź niestandardową instrukcję SQL; lub użyj procedury składowanej. Bez względu na to, czy wybierasz kolumny z tabeli czy widoku, czy wprowadzasz niestandardową instrukcję SQL, właściwość kontrolki SqlDataSource formantu `SelectCommand` s ma przypisaną powstałą instrukcję ad hoc SQL `SELECT` i jest to instrukcja `SELECT` wykonywana po wywołaniu metody kontrolki SqlDataSource s `Select()` (programowo lub automatycznie z formantu sieci Web danych).

Instrukcje SQL `SELECT` używane w poprzednich pokazach z samouczkiem s nie zostały `WHERE` klauzule. W instrukcji `SELECT` klauzula `WHERE` może służyć do ograniczania zwracanych wyników. Aby na przykład wyświetlić nazwy produktów, które są droższe niż $50,00, możemy użyć następującej kwerendy:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

Zazwyczaj wartości używane w klauzuli `WHERE` są określane przez niektóre źródła zewnętrzne, takie jak wartość QueryString, zmienna sesji lub dane wejściowe użytkownika z kontrolki sieci Web na stronie. W idealnym przypadku takie dane wejściowe są określane za pomocą *parametrów*. W przypadku Microsoft SQL Server parametry są oznaczane przy użyciu `@parameterName`, jak w:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

Kontrolki SqlDataSource obsługuje zapytania parametryczne, zarówno dla instrukcji `SELECT`, jak i `INSERT`, `UPDATE`i `DELETE` instrukcji. Ponadto wartości parametrów mogą być automatycznie ściągane z różnych źródeł, elementu QueryString, stanu sesji, formantów na stronie itd. lub można je przypisać programowo. W ramach tego samouczka zobaczymy, jak definiować sparametryzowane zapytania, a także jak określać wartości parametrów zarówno deklaratywnie, jak i programowo.

> [!NOTE]
> W poprzednim samouczku porównano element ObjectDataSource, który był narzędziem wybranym dla pierwszych 46 samouczków z kontrolki SqlDataSource, i notuje podobieństwa koncepcyjne. Te podobieństwa również przechodzą do parametrów. Parametry ObjectDataSource s zamapowane na parametry wejściowe dla metod w warstwie logiki biznesowej. Za pomocą kontrolki SqlDataSource parametry są definiowane bezpośrednio w zapytaniu SQL. Obie kontrolki mają kolekcje parametrów dla ich metod `Select()`, `Insert()`, `Update()`i `Delete()`, a obie te wartości parametrów są wypełniane ze wstępnie zdefiniowanych źródeł (wartości QueryString, zmienne sesji itd.) lub przypisane programowo.

## <a name="creating-a-parameterized-query"></a>Tworzenie zapytania parametrycznego

Kreator konfiguracji źródła danych kontrolki SqlDataSource Control s oferuje trzy drogi do definiowania polecenia do wykonania w celu pobrania rekordów bazy danych:

- Przez wybranie kolumn z istniejącej tabeli lub widoku,
- Wprowadzając niestandardową instrukcję SQL lub
- Przez wybranie procedury składowanej

Podczas wybierania kolumn z istniejącej tabeli lub widoku, parametry dla klauzuli `WHERE` muszą być określone za pomocą okna dialogowego Dodaj klauzulę `WHERE`. W przypadku tworzenia niestandardowej instrukcji SQL można jednak wprowadzić parametry bezpośrednio w klauzuli `WHERE` (przy użyciu `@parameterName` do określenia poszczególnych parametrów). [Procedura składowana](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) składa się z co najmniej jednej instrukcji SQL, a te instrukcje mogą być sparametryzowane. Jednakże parametry używane w instrukcjach SQL muszą być przesyłane jako parametry wejściowe do procedury składowanej.

Ponieważ Tworzenie zapytania sparametryzowanego zależą od tego, jak kontrolki SqlDataSource `SelectCommand` jest określona, powiadom o wszystkich trzech podejściach. Aby rozpocząć, Otwórz stronę `ParameterizedQueries.aspx` w folderze `SqlDataSource`, przeciągnij kontrolkę kontrolki SqlDataSource z przybornika do projektanta i ustaw jej `ID` na `Products25BucksAndUnderDataSource`. Następnie kliknij link Konfiguruj źródło danych z tagu inteligentnego sterowanie s. Wybierz bazę danych, która ma zostać użyta (`NORTHWINDConnectionString`), a następnie kliknij przycisk Dalej.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Krok 1. Dodawanie klauzuli`WHERE`podczas wybierania kolumn z tabeli lub widoku

Po wybraniu danych do zwrócenia z bazy danych za pomocą kontrolki kontrolki SqlDataSource Kreator konfiguracji źródła danych umożliwia nam wybranie kolumn do zwrócenia z istniejącej tabeli lub widoku (patrz rysunek 1). Spowoduje to automatyczne utworzenie instrukcji SQL `SELECT`, która jest wysyłana do bazy danych po wywołaniu metody kontrolki SqlDataSource s `Select()`. Tak jak w poprzednim samouczku wybierz tabelę produkty z listy rozwijanej i Sprawdź kolumny `ProductID`, `ProductName`i `UnitPrice`.

[![wybrać kolumny do zwrócenia z tabeli lub widoku](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Rysunek 1**. Wybieranie kolumn do zwrócenia z tabeli lub widoku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))

Aby uwzględnić w instrukcji `SELECT` klauzulę `WHERE`, kliknij przycisk `WHERE`, który powoduje wyświetlenie okna dialogowego Dodawanie klauzuli `WHERE` (patrz rysunek 2). Aby dodać parametr w celu ograniczenia wyników zwracanych przez zapytanie `SELECT`, najpierw wybierz kolumnę, według której chcesz filtrować dane. Następnie wybierz operator, który ma być używany do filtrowania (=, &lt;, &lt;=, &gt;itd.). Na koniec wybierz źródło wartości parametru s, takie jak from QueryString lub Session State. Po skonfigurowaniu parametru kliknij przycisk Dodaj, aby uwzględnić go w kwerendzie `SELECT`.

Na potrzeby tego przykładu należy zwrócić te wyniki tylko wtedy, gdy wartość `UnitPrice` jest mniejsza lub równa $25,00. W związku z tym wybierz `UnitPrice` z listy rozwijanej kolumny i &lt;= z listy rozwijanej operator. W przypadku użycia zakodowanej wartości parametru (na przykład $25,00) lub, jeśli wartość parametru ma być określona programowo, wybierz opcję Brak z listy rozwijanej Źródło. Następnie wprowadź zakodowaną wartość parametru w polu tekstowym wartość 25,00 i Ukończ ten proces, klikając przycisk Dodaj.

[![ograniczyć wyniki zwracane z okna dialogowego Dodawanie klauzuli WHERE](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Rysunek 2**. Ograniczanie wyników zwróconych z okna dialogowego Dodaj klauzulę `WHERE` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))

Po dodaniu parametru kliknij przycisk OK, aby powrócić do Kreatora konfiguracji źródła danych. Instrukcja `SELECT` w dolnej części kreatora powinna teraz zawierać klauzulę `WHERE` z parametrem o nazwie `@UnitPrice`:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Jeśli określisz wiele warunków w klauzuli `WHERE` z okna dialogowego Dodaj klauzulę `WHERE`, Kreator przyłączy je za pomocą operatora `AND`. Jeśli konieczne jest uwzględnienie `OR` w klauzuli `WHERE` (na przykład `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), należy utworzyć instrukcję `SELECT` za pomocą niestandardowego ekranu instrukcji SQL.

Ukończ Konfigurowanie kontrolki SqlDataSource (kliknij przycisk Dalej, następnie Zakończ), a następnie sprawdź znaczniki deklaratywne kontrolki SqlDataSource. Znacznik zawiera teraz kolekcję `<SelectParameters>`, która sprawdza źródła parametrów w `SelectCommand`.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Po wywołaniu metody kontrolki SqlDataSource s `Select()` wartość parametru `UnitPrice` (25,00) zostanie zastosowana do parametru `@UnitPrice` w `SelectCommand` przed wysłaniem do bazy danych. Wynikiem jest to, że tylko produkty mniejsze niż lub równe $25,00 są zwracane z tabeli `Products`. Aby to potwierdzić, Dodaj widok GridView do strony, powiąż go z tym źródłem danych, a następnie Wyświetl stronę za pomocą przeglądarki. Należy zobaczyć tylko te produkty, które są mniejsze lub równe $25,00, jak pokazano na rysunku 3.

[![wyświetlane są tylko te produkty, które są mniejsze lub równe $25,00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Rysunek 3**. wyświetlane są tylko te produkty, które są mniejsze lub równe $25,00 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))

## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Krok 2. Dodawanie parametrów do niestandardowej instrukcji SQL

Podczas dodawania niestandardowej instrukcji SQL można wprowadzić klauzulę `WHERE` jawnie lub określić wartość w komórce filtru Konstruktor zapytań. Aby to zademonstrować, należy wyświetlić tylko te produkty w widoku GridView, których ceny są mniejsze niż określony próg. Zacznij od dodania pola tekstowego do strony `ParameterizedQueries.aspx`, aby zebrać tę wartość progową od użytkownika. Ustaw właściwość TextBox s `ID` na `MaxPrice`. Dodaj kontrolkę sieci Web przycisk i ustaw jej Właściwość `Text`, aby wyświetlić pasujące produkty.

Następnie przeciągnij widok GridView na stronę i z jego tagu inteligentnego wybierz, aby utworzyć nowy kontrolki SqlDataSource o nazwie `ProductsFilteredByPriceDataSource`. W Kreatorze konfiguracji źródła danych przejdź do ekranu Określ niestandardową instrukcję SQL lub procedurę składowaną (zobacz rysunek 4) i wprowadź następujące zapytanie:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Po wprowadzeniu zapytania (ręcznie lub za pomocą Konstruktor zapytań) kliknij przycisk Dalej.

[![zwracać tylko produkty mniejsze niż lub równe wartości parametru](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Rysunek 4**. Zwracanie tylko tych produktów, których wartość jest mniejsza lub równa wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))

Ponieważ zapytanie zawiera parametry, następnym ekranem w kreatorze zostanie wyświetlony komunikat z prośbą o źródło wartości parametrów. Wybierz kontrolkę z listy rozwijanej Źródło parametru i `MaxPrice` (pole tekstowe kontrolka `ID` wartość) z listy rozwijanej ControlID. Możesz również wprowadzić opcjonalną wartość domyślną, która ma być używana w przypadku, gdy użytkownik nie wprowadził tekstu do `MaxPrice` TextBox. W danym momencie nie należy wprowadzać wartości domyślnej.

[![właściwość Text pola tekstowego MaxPrice jest używana jako źródło parametru](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Rysunek 5**: Właściwość `MaxPrice` TextBox s `Text` jest używana jako źródło parametru ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))

Ukończ pracę Kreatora konfiguracji źródła danych, klikając przycisk Dalej, a następnie przycisk Zakończ. Znaczniki deklaratywne dla elementu GridView, TextBox, Button i kontrolki SqlDataSource są następujące:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Należy zauważyć, że parametr w sekcji kontrolki SqlDataSource `<SelectParameters>` jest `ControlParameter`, który zawiera dodatkowe właściwości, takie jak `ControlID` i `PropertyName`. Po wywołaniu metody kontrolki SqlDataSource `Select()` s `ControlParameter` ponosi wartość z określonej właściwości kontrolki sieci Web i przypisze ją do odpowiedniego parametru w `SelectCommand`. W tym przykładzie właściwość Text `MaxPrice` s jest używana jako wartość parametru `@MaxPrice`.

Poświęć minutę na wyświetlenie tej strony za pomocą przeglądarki. Podczas pierwszego odwiedzania strony lub gdy pole tekstowe `MaxPrice` nie ma wartości, w widoku GridView nie są wyświetlane żadne rekordy.

[![żadne rekordy nie są wyświetlane, gdy pole tekstowe MaxPrice jest puste](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Ilustracja 6**. nie są wyświetlane żadne rekordy, gdy pole tekstowe `MaxPrice` jest puste ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))

Nie są wyświetlane żadne produkty, ponieważ domyślnie pusty ciąg dla wartości parametru jest konwertowany na wartość `NULL` bazy danych. Ponieważ porównanie `[UnitPrice] <= NULL` ma zawsze wartość false, nie są zwracane żadne wyniki.

Wprowadź wartość w polu tekstowym, na przykład 5,00, a następnie kliknij przycisk Wyświetl pasujące produkty. Na stronie ogłaszania zwrotnego kontrolki SqlDataSource informuje widok GridView o zmianie jednego ze źródeł parametrów. W związku z tym, GridView ponownie wiąże się z kontrolki SqlDataSource, wyświetlając te produkty mniejsze lub równe $5,00.

[![są wyświetlane produkty mniejsze lub równe $5,00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Rysunek 7**. wyświetlane są produkty mniejsze niż lub równe $5,00 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))

## <a name="initially-displaying-all-products"></a>Początkowo wyświetlane są wszystkie produkty

Zamiast nie wyświetlać żadnych produktów podczas pierwszego ładowania strony, możemy chcieć wyświetlić *wszystkie* produkty. Jednym ze sposobów wyświetlania listy wszystkich produktów za każdym razem, gdy `MaxPrice` pole jest puste, jest ustawienie wartości domyślnej parametru s na wartość wyjątkowo o wysokiej wartości, na przykład 1000000, ponieważ jest mało prawdopodobne, że firma Northwind Traders będzie miała spis, którego cena jednostkowa przekracza $1 000 000. Jednak takie podejście jest shortsighted i może nie zadziałać w innych sytuacjach.

W poprzednich samouczkach — [Parametry deklaratywne](../basic-reporting/declarative-parameters-vb.md) i [wzorzec/szczegóły filtrowania przy użyciu DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) z podobnym problemem. Nasze rozwiązanie ma umieścić tę logikę w warstwie logiki biznesowej. LOGIKI biznesowej zbadał wartość przychodzącą i, jeśli była `NULL` lub określona wartość zastrzeżona, wywołanie zostało przekazane do metody DAL, która zwróciła wszystkie rekordy. Jeśli wartość przychodząca była normalną wartością filtrowania, nastąpiło wywołanie metody DAL, która wykonała instrukcję SQL, która używała sparametryzowanej klauzuli `WHERE` z podaną wartością.

Niestety, pomijamy architekturę przy użyciu kontrolki SqlDataSource. Zamiast tego musimy dostosować instrukcję SQL, aby inteligentnie odfiltrować wszystkie rekordy, jeśli parametr `@MaximumPrice` jest `NULL` lub pewnej wartości zastrzeżonej. Na potrzeby tego ćwiczenia poinformuj ich, że jeśli parametr `@MaximumPrice` jest równy `-1.0`, *wszystkie* rekordy zostaną zwrócone (`-1.0` działa jako wartość zastrzeżona, ponieważ żaden produkt nie może mieć ujemnej wartości `UnitPrice`). Aby to osiągnąć, można użyć następującej instrukcji SQL:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

Ta `WHERE` klauzula zwraca *wszystkie* rekordy, jeśli parametr `@MaximumPrice` jest równy `-1.0`. Jeśli wartość parametru nie jest `-1.0`, zwracane są tylko te produkty, których `UnitPrice` jest mniejsza niż lub równa wartości parametru `@MaximumPrice`. Ustawiając wartość domyślną parametru `@MaximumPrice` na `-1.0`, podczas pierwszego ładowania strony (lub za każdym razem, gdy pole tekstowe `MaxPrice` jest puste), `@MaximumPrice` będzie mieć wartość `-1.0` i zostaną wyświetlone wszystkie produkty.

[![teraz wszystkie produkty są wyświetlane, gdy pole tekstowe MaxPrice jest puste](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Ilustracja 8**. teraz wszystkie produkty są wyświetlane, gdy pole tekstowe `MaxPrice` jest puste ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))

Istnieje kilka sposobów zaewidencjonowania tego podejścia. Najpierw należy pamiętać, że typ danych parametru s jest wnioskowany przez użycie przez niego w zapytaniu SQL. Jeśli zmienisz klauzulę `WHERE` z `@MaximumPrice = -1.0` na `@MaximumPrice = -1`, środowisko uruchomieniowe traktuje parametr jako liczbę całkowitą. Jeśli następnie zostanie podjęta próba przypisania `MaxPrice` TextBox do wartości dziesiętnej (na przykład 5,00), wystąpi błąd, ponieważ nie można skonwertować 5,00 na liczbę całkowitą. Aby to naprawić, upewnij się, że używasz `@MaximumPrice = -1.0` w klauzuli `WHERE` lub jeszcze lepiej ustaw właściwość `Type` obiektu `ControlParameter` na wartość dziesiętną.

Po drugie, dodając `OR @MaximumPrice = -1.0` do klauzuli `WHERE`, aparat zapytań nie może użyć indeksu w `UnitPrice` (przy założeniu, że taki istnieje), co prowadzi do skanowania tabeli. Może to mieć wpływ na wydajność, jeśli w tabeli `Products` istnieje wystarczająco duża liczba rekordów. Lepszym rozwiązaniem jest przeniesienie tej logiki do procedury składowanej, w której instrukcja `IF` może wykonać zapytanie `SELECT` z tabeli `Products` bez klauzuli `WHERE`, gdy wszystkie rekordy muszą zostać zwrócone lub jeden z klauzul `WHERE` zawiera tylko kryteria `UnitPrice`, aby można było użyć indeksu.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Krok 3. Tworzenie i używanie sparametryzowanych procedur składowanych

Procedury składowane mogą zawierać zestaw parametrów wejściowych, które mogą być następnie używane w instrukcjach SQL zdefiniowanych w ramach procedury składowanej. Podczas konfigurowania kontrolki SqlDataSource do używania procedury składowanej, która akceptuje parametry wejściowe, te wartości parametrów można określić przy użyciu tych samych technik jak w przypadku instrukcji SQL ad hoc.

Aby zilustrować procedury składowane w kontrolki SqlDataSource, pozwól s utworzyć nową procedurę składowaną w bazie danych Northwind o nazwie `GetProductsByCategory`, która akceptuje parametr o nazwie `@CategoryID` i zwraca wszystkie kolumny produktów, których kolumna `CategoryID` pasuje do `@CategoryID`. Aby utworzyć procedurę przechowywaną, przejdź do Eksplorator serwera i przechodzenie do szczegółów `NORTHWND.MDF` bazie danych. (Jeśli nie widzisz Eksplorator serwera, przenieś je, przechodząc do menu Widok i wybierając opcję Eksplorator serwera).

W bazie danych `NORTHWND.MDF` kliknij prawym przyciskiem myszy folder procedury składowane, wybierz polecenie Dodaj nową procedurę składowaną, a następnie wprowadź następującą składnię:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Kliknij ikonę Zapisz (lub Ctrl + S), aby zapisać procedurę składowaną. Procedurę składowaną można przetestować, klikając ją prawym przyciskiem myszy w folderze procedury składowane, a następnie wybierając polecenie wykonaj. Spowoduje to wyświetlenie monitu o podanie parametrów procedury składowanej s (`@CategoryID`w tym wystąpieniu), po upływie którego wyniki będą wyświetlane w oknie danych wyjściowych.

[![procedury składowanej GetProductsByCategory podczas wykonywania z @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Rysunek 9**. `GetProductsByCategory` procedury składowanej w przypadku wykonywania z `@CategoryID` 1 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))

Niech s Użyj tej procedury składowanej, aby wyświetlić wszystkie produkty w kategorii napoje w widoku GridView. Dodaj nowy element GridView do strony i powiąż go z nowym kontrolki SqlDataSource o nazwie `BeverageProductsDataSource`. Przejdź do ekranu Określ niestandardową instrukcję SQL lub procedurę składowaną, wybierz przycisk radiowy procedury składowanej i wybierz procedurę składowaną `GetProductsByCategory` z listy rozwijanej.

[![wybierz procedurę składowaną GetProductsByCategory z listy rozwijanej](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Ilustracja 10**. Wybierz `GetProductsByCategory` procedurę składowaną z listy rozwijanej (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))

Ponieważ procedura składowana akceptuje parametr wejściowy (`@CategoryID`), kliknij przycisk Dalej monituje nas, aby określić źródło dla tej wartości parametru s. Napoje `CategoryID` są 1, więc pozostaw listę rozwijaną Źródło parametru w polu brak i wprowadź wartość 1 w polu tekstowym DefaultValue.

[![użyć zakodowanej wartości 1, aby zwrócić produkty z kategorii napoje](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Rysunek 11**. Użyj zakodowanej wartości 1, aby zwrócić produkty z kategorii napoje ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))

Jak pokazano poniżej, w przypadku używania procedury składowanej, właściwość kontrolki SqlDataSource `SelectCommand` s jest ustawiana na nazwę procedury składowanej, a [właściwość`SelectCommandType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) jest ustawiona na `StoredProcedure`, wskazując, że `SelectCommand` jest nazwą procedury składowanej, a nie instrukcją SQL ad hoc.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Przetestuj stronę w przeglądarce. Wyświetlane są tylko te produkty, które należą do kategorii napoje, chociaż *wszystkie* pola produktu są wyświetlane od momentu, gdy procedura składowana `GetProductsByCategory` zwraca wszystkie kolumny z tabeli `Products`. Możemy oczywiście ograniczyć lub dostosować pola wyświetlane w widoku GridView z okna dialogowego Edytowanie kolumn w widoku GridView.

[![wszystkie napoje są wyświetlane](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Ilustracja 12**. wyświetlane są wszystkie napoje ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))

## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Krok 4. programowe wywoływanie instrukcji kontrolki SqlDataSource s`Select()`

Przykłady, które zostały zaobserwowane w poprzednim samouczku i tym samouczku, mają do tej pory powiązane kontrolki kontrolki SqlDataSource bezpośrednio z elementem GridView. Dane kontrolne kontrolki SqlDataSource są jednak dostępne do programistycznego i wyliczane w kodzie. Może to być szczególnie przydatne, gdy trzeba wykonać zapytanie o dane, aby je sprawdzić, ale nie trzeba go wyświetlić. Zamiast zapisywać cały standardowy kod ADO.NET w celu nawiązania połączenia z bazą danych, określić polecenie i pobrać wyniki, możesz pozwolić, aby kontrolki SqlDataSource obsłużyć ten kod monotonous.

Aby w sposób programistyczny zilustrować pracę z danymi kontrolki SqlDataSource, Załóżmy, że szef przybrał żądanie utworzenia strony sieci Web, która wyświetla nazwę losowo wybranej kategorii i powiązane z nią produkty. Oznacza to, że gdy użytkownik odwiedzi tę stronę, chcemy losowo wybrać kategorię z tabeli `Categories`, wyświetlić nazwę kategorii, a następnie wyświetlić listę produktów należących do tej kategorii.

Aby to osiągnąć, potrzebujemy dwóch kontrolki SqlDataSource formantów, aby pobrać losowo kategorię z tabeli `Categories` i drugą, aby uzyskać produkty kategorii. Utworzymy kontrolki SqlDataSource, który pobiera losowy rekord kategorii w tym kroku. Krok 5 analizuje kontrolki SqlDataSource, który pobiera produkty kategorii.

Zacznij od dodania kontrolki SqlDataSource do `ParameterizedQueries.aspx` i ustaw `ID` do `RandomCategoryDataSource`. Skonfiguruj ją tak, aby korzystała z następującej kwerendy SQL:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` zwraca rekordy posortowane w kolejności losowej (zobacz [używanie `NEWID()` do losowego sortowania rekordów](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` zwraca pierwszy rekord z zestawu wyników. To zapytanie zwraca wartości kolumn `CategoryID` i `CategoryName` z pojedynczej, losowo wybranej kategorii.

Aby wyświetlić wartość `CategoryName` kategorii, Dodaj kontrolkę sieci Web etykieta do strony, ustaw jej Właściwość `ID` na `CategoryNameLabel`i wyczyść jej Właściwość `Text`. Aby programowo pobrać dane z formantu kontrolki SqlDataSource, musimy wywołać jego metodę `Select()`. [Metoda`Select()`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) oczekuje jednego parametru wejściowego typu [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), który określa sposób, w jaki dane powinny być wysyłane przed zwróceniem. Może to obejmować instrukcje sortowania i filtrowania danych oraz używane przez kontrolki sieci Web danych podczas sortowania lub stronicowania danych z formantu kontrolki SqlDataSource. W naszym przykładzie nie potrzebujemy, aby dane były modyfikowane przed zwróceniem i w związku z tym zostaną przekazane do obiektu `DataSourceSelectArguments.Empty`.

Metoda `Select()` zwraca obiekt implementujący `IEnumerable`. Dokładny zwracany typ zależy od wartości właściwości kontrolki SqlDataSource sterowania [`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Zgodnie z opisem w poprzednim samouczku tę właściwość można ustawić na wartość `DataSet` lub `DataReader`. Jeśli zostanie ustawiona na `DataSet`, Metoda `Select()` zwróci obiekt [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) ; Jeśli zostanie ustawiona na `DataReader`, zwraca obiekt implementujący [`IDataReader`](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Ponieważ `RandomCategoryDataSource` kontrolki SqlDataSource ma właściwość `DataSourceMode` ustawioną na `DataSet` (wartość domyślna), będziemy pracować z obiektem DataView.

Poniższy kod ilustruje sposób pobierania rekordów z `RandomCategoryDataSource` kontrolki SqlDataSource jako widok DataView, jak również odczytywanie wartości kolumny `CategoryName` z pierwszego wiersza DataView:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` zwraca pierwszy `DataRowView` w elemencie DataView. `randomCategoryView(0)("CategoryName")` zwraca wartość kolumny `CategoryName` w tym pierwszym wierszu. Należy zauważyć, że widok DataView jest luźno określony. Aby odwołać się do określonej wartości kolumny, musimy przekazać nazwę kolumny jako ciąg (w tym przypadku CategoryName). Rysunek 13 przedstawia komunikat wyświetlany w `CategoryNameLabel` podczas wyświetlania strony. Oczywiście rzeczywista nazwa kategorii jest losowo wybierana przez `RandomCategoryDataSource` kontrolki SqlDataSource na każdej odwiedziny na stronie (w tym zwroty ogłoszeń).

[![zostanie wyświetlona nazwa losowo wybranej kategorii s](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Ilustracja 13**. wyświetlana jest losowo wybrana nazwa kategorii (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))

> [!NOTE]
> Jeśli właściwość `DataSourceMode` sterowania kontrolki SqlDataSource została ustawiona na `DataReader`, wartość zwracana z metody `Select()` byłaby wymagana do rzutowania na `IDataReader`. Aby odczytać wartość kolumny `CategoryName` z pierwszego wiersza, używamy kodu, takiego jak:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

Po wybraniu kategorii kontrolki SqlDataSource losowo, możemy dodać widok GridView, który wyświetla listę produktów kategorii.

> [!NOTE]
> Zamiast używać kontrolki sieci Web etykieta do wyświetlania nazwy kategorii, możemy dodać do strony element FormView lub DetailsView, a następnie powiązać go z kontrolki SqlDataSource. Jednak korzystając z etykiety, można dowiedzieć się, jak programowo wywołać instrukcję kontrolki SqlDataSource s `Select()` i pracy z danymi uzyskanymi w kodzie.

## <a name="step-5-assigning-parameter-values-programmatically"></a>Krok 5. programowe Przypisywanie wartości parametrów

We wszystkich przykładach, które zaobserwowano do tej pory w tym samouczku, użyto zakodowanej wartości parametru lub jednej z jednego ze wstępnie zdefiniowanych źródeł parametrów (wartości QueryString, kontrolki sieci Web na stronie itd.). Jednak parametry kontrolki SqlDataSource Control s można także skonfigurować programowo. Aby ukończyć bieżący przykład, potrzebujemy elementu kontrolki SqlDataSource, który zwraca wszystkie produkty należące do określonej kategorii. Ta kontrolki SqlDataSource będzie miała parametr `CategoryID`, którego wartość musi być ustawiona na podstawie `CategoryID` wartości kolumny zwróconej przez `RandomCategoryDataSource` kontrolki SqlDataSource w obsłudze zdarzeń `Page_Load`.

Zacznij od dodania widoku GridView do strony i powiąż go z nowym kontrolki SqlDataSource o nazwie `ProductsByCategoryDataSource`. Podobnie jak w kroku 3, skonfiguruj kontrolki SqlDataSource tak, aby wywoła procedurę składowaną `GetProductsByCategory`. Pozostaw listę rozwijaną Źródło parametru ustawioną na wartość None, ale nie wprowadzaj wartości domyślnej, ponieważ ta wartość domyślna zostanie ustawiona programowo.

[![nie określać wartości źródłowej lub domyślnej parametru](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Ilustracja 14**. nie określaj wartości źródłowej lub domyślnej parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))

Po zakończeniu działania kreatora kontrolki SqlDataSource, powstające deklaratywne znaczniki powinny wyglądać podobnie do następujących:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

`DefaultValue` parametru `CategoryID` można programowo przypisać do programu obsługi zdarzeń `Page_Load`:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Dzięki temu, Strona zawiera widok GridView, który pokazuje produkty skojarzone z losowo wybraną kategorią.

[![nie określać wartości źródłowej lub domyślnej parametru](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Ilustracja 15**. nie określaj wartości źródłowej lub domyślnej parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))

## <a name="summary"></a>Podsumowanie

Kontrolki SqlDataSource umożliwia deweloperom stron Definiowanie zapytań parametrycznych, których wartości parametrów mogą być zakodowane na stałe, pobierane ze wstępnie zdefiniowanych źródeł parametrów lub przypisane programowo. W tym samouczku przedstawiono sposób tworzenia zapytania parametrycznego w Kreatorze konfiguracji źródła danych dla zapytań SQL ad hoc i procedur składowanych. Przeglądamy również przy użyciu zakodowanych źródeł parametrów, kontrolki sieci Web jako źródła parametrów i programowo określając wartość parametru.

Podobnie jak w przypadku elementu ObjectDataSource, kontrolki SqlDataSource udostępnia również możliwości modyfikacji własnych danych. W następnym samouczku przedstawiono sposób definiowania instrukcji `INSERT`, `UPDATE`i `DELETE` z kontrolki SqlDataSource. Po dodaniu tych instrukcji możemy użyć wbudowanego wstawiania, edytowania i usuwania funkcji, które są związane z kontrolkami GridView, DetailsView i FormView.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Scott Clyde, Randell Schmidt i Krzysztof Pespisa. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](querying-data-with-the-sqldatasource-control-vb.md)
> [dalej](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
