---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Sortowanie niestandardowych danych stronicowanych (VB) | Microsoft Docs
author: rick-anderson
description: W poprzednim samouczku dowiesz się, jak zaimplementować niestandardowe stronicowanie podczas prezentowania danych na stronie sieci Web. W tym samouczku zobaczymy, jak zwiększyć poprzednią...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 934c7558d907611732ae6f04c553bc9e295c569b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78618833"
---
# <a name="sorting-custom-paged-data-vb"></a>Sortowanie niestandardowo stronicowanych danych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) lub [Pobierz plik PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> W poprzednim samouczku dowiesz się, jak zaimplementować niestandardowe stronicowanie podczas prezentowania danych na stronie sieci Web. W tym samouczku zobaczymy, jak zwiększyć poprzedni przykład, aby włączyć obsługę sortowania niestandardowych.

## <a name="introduction"></a>Wprowadzenie

W porównaniu do domyślnego stronicowania, niestandardowe stronicowanie może zwiększyć wydajność stronicowania za pomocą danych przez kilka zamówień, dzięki czemu niestandardowe stronicowanie ma wybór implementacji stronicowania podczas stronicowania w dużych ilościach danych. Implementacja stronicowania niestandardowego jest większa niż implementacja domyślnego stronicowania, ale szczególnie podczas dodawania sortowania do mieszanki. W tym samouczku podasz przykład z poprzednią, aby włączyć obsługę sortowania *i* niestandardowego stronicowania.

> [!NOTE]
> Ponieważ ten samouczek jest kompilowany na poprzednim, przed rozpoczęciem Poświęć chwilę na skopiowanie składni deklaracyjnej w ramach elementu `<asp:Content>` z poprzedniej strony sieci Web samouczka (`EfficientPaging.aspx`) i wkleić ją między `<asp:Content>` elementu na stronie `SortParameter.aspx`. Zapoznaj się z artykułem krok 1 [Dodawanie kontrolek walidacji do edycji i wstawiania interfejsów](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) samouczek, aby uzyskać bardziej szczegółową dyskusję na temat replikowania funkcji jednej strony ASP.NET do innej.

## <a name="step-1-reexamining-the-custom-paging-technique"></a>Krok 1: badanie niestandardowej techniki stronicowania

Aby niestandardowe stronicowanie działało prawidłowo, należy zaimplementować pewną technikę, która może efektywnie odfiltrować określony podzbiór rekordów z uwzględnieniem indeksu wierszy początkowych i parametrów maksymalnej liczby wierszy. Istnieje kilku technik, których można użyć do osiągnięcia tego celu. W poprzednim samouczku przeszukano to przy użyciu Microsoft SQL Server 2005 s nowej funkcji klasyfikacyjnej `ROW_NUMBER()`. W skrócie funkcja klasyfikacji `ROW_NUMBER()` przypisuje numer wiersza do każdego wiersza zwróconego przez zapytanie, które jest klasyfikowane według określonej kolejności sortowania. Następnie uzyskuje się odpowiedni podzbiór rekordów, zwracając określoną sekcję numerowanych wyników. Poniższe zapytanie ilustruje sposób użycia tej metody do zwrócenia tych produktów o numerach od 11 do 20 podczas klasyfikowania wyników uporządkowanych alfabetycznie przez `ProductName`:

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Ta technika działa dobrze w przypadku stronicowania przy użyciu określonej kolejności sortowania (`ProductName` posortowane alfabetycznie, w tym przypadku), ale zapytanie należy zmodyfikować, aby pokazać wyniki posortowane według innego wyrażenia sortowania. W idealnym przypadku powyższego zapytania można ponownie napisać, aby użyć parametru w klauzuli `OVER`, na przykład:

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Niestety, sparametryzowane klauzule `ORDER BY` są niedozwolone. Zamiast tego należy utworzyć procedurę składowaną, która akceptuje `@sortExpression` parametr wejściowy, ale używa jednego z następujących obejść:

- Napisz stałe zapytania dla każdego z wyrażeń sortowania, które mogą być używane; następnie użyj `IF/ELSE` instrukcji języka T-SQL, aby określić, które zapytanie należy wykonać.
- Użyj instrukcji `CASE`, aby dostarczyć dynamiczne wyrażenia `ORDER BY` na podstawie parametru wejściowego `@sortExpressio` n. Aby uzyskać więcej informacji, zobacz sekcję Używanie do dynamicznego sortowania wyników zapytania w [instrukcji dodatku SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml) .
- Utwórz odpowiednie zapytanie jako ciąg w procedurze składowanej, a następnie użyj [procedury składowanej `sp_executesql` system](https://msdn.microsoft.com/library/ms188001.aspx) do wykonania zapytania dynamicznego.

Każdy z tych obejść ma pewne wady. Pierwsza opcja nie jest możliwie najmniejsza, ponieważ wymaga utworzenia kwerendy dla każdego możliwego wyrażenia sortowania. W związku z tym, jeśli później zdecydujesz się dodać nowe, sortowane pola do widoku GridView, należy również wrócić i zaktualizować procedurę składowaną. Drugie podejście ma pewne subtleties, które wprowadzają problemy związane z wydajnością podczas sortowania według kolumn baz danych, które nie są ciągami, a także odczuwają te same problemy z konserwacją co pierwszy. A trzeci wybór, który używa dynamicznego języka SQL, stanowi wprowadzenie do ataku polegającego na iniekcji kodu SQL, jeśli osoba atakująca może wykonać procedurę przechowywaną w wybranych przez siebie wartości parametrów wejściowych.

Chociaż żadne z tych metod nie jest idealne, uważam, że trzecia opcja jest Najlepsza z trzech. Dzięki zastosowaniu dynamicznego języka SQL oferuje on poziom elastyczności, a pozostałe dwa nie. Ponadto atak iniekcji kodu SQL może być stosowany tylko wtedy, gdy osoba atakująca będzie mogła wykonać procedurę przechowywaną w wybranych przez siebie parametrach wejściowych. Ponieważ DAL korzysta z zapytań parametrycznych, ADO.NET będzie chronić te parametry, które są wysyłane do bazy danych za pośrednictwem architektury, co oznacza, że usterka ataku polegającego na iniekcji SQL występuje tylko wtedy, gdy osoba atakująca może bezpośrednio wykonać procedurę składowaną.

Aby zaimplementować tę funkcję, Utwórz nową procedurę składowaną w bazie danych Northwind o nazwie `GetProductsPagedAndSorted`. Ta procedura składowana powinna akceptować trzy parametry wejściowe: `@sortExpression`, parametr wejściowy typu `nvarchar(100`), który określa, jak wyniki mają być sortowane i są wstrzykiwane bezpośrednio po `ORDER BY` tekście w klauzuli `OVER`; i `@startRowIndex` i `@maximumRows`, te same dwa parametry wejściowe liczby całkowitej z procedury składowanej `GetProductsPaged` przeanalizowane w poprzednim samouczku. Utwórz procedurę składowaną `GetProductsPagedAndSorted` przy użyciu następującego skryptu:

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

Procedura składowana jest uruchamiana przez upewnienie się, że określono wartość parametru `@sortExpression`. W takim przypadku wyniki są klasyfikowane według `ProductID`. Następnie zostanie skonstruowane dynamiczne zapytanie SQL. Należy zauważyć, że dynamiczne zapytanie SQL różni się nieco od poprzednich zapytań użytych do pobrania wszystkich wierszy z tabeli Products. We wcześniejszych przykładach podano wszystkie skojarzone z nimi produkty kategorie s i dostawcy za pomocą podzapytania. Ta decyzja została wprowadzona z powrotem w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) i została wykonana zamiast używania `JOIN` s, ponieważ TableAdapter nie może automatycznie utworzyć skojarzonych metod INSERT, Update i DELETE dla takich zapytań. W `GetProductsPagedAndSorted` procedury składowanej należy jednak użyć `JOIN` s, aby wyniki były uporządkowane według nazw kategorii lub dostawców.

To zapytanie dynamiczne jest kompilowane przez łączenie części zapytania statycznego i parametrów `@sortExpression`, `@startRowIndex`i `@maximumRows`. Ponieważ `@startRowIndex` i `@maximumRows` są parametrami całkowitymi, muszą być konwertowane na nvarchars w celu poprawnego łączenia. Po skonstruowaniu tego dynamicznego zapytania SQL jest ono wykonywane za pośrednictwem `sp_executesql`.

Poświęć chwilę na przetestowanie tej procedury składowanej o różne wartości parametrów `@sortExpression`, `@startRowIndex`i `@maximumRows`. W Eksplorator serwera kliknij prawym przyciskiem myszy nazwę procedury składowanej i wybierz polecenie Execute (wykonaj). Spowoduje to wyświetlenie okna dialogowego uruchamianie procedury przechowywanej, w którym można wprowadzić parametry wejściowe (patrz rysunek 1). Aby posortować wyniki według nazwy kategorii, użyj CategoryName dla wartości parametru `@sortExpression`. Aby posortować według nazwy firmy dostawcy, użyj CompanyName. Po podania wartości parametrów kliknij przycisk OK. Wyniki są wyświetlane w oknie dane wyjściowe. Rysunek 2 przedstawia wyniki w przypadku powrotu produktów do rangi od 11 do 20 podczas porządkowania według `UnitPrice` w kolejności malejącej.

![Wypróbuj różne wartości dla procedury składowanej z trzema parametrami wejściowymi](sorting-custom-paged-data-vb/_static/image1.png)

**Rysunek 1**. Spróbuj użyć innych wartości dla procedury składowanej trzy parametry wejściowe

[![wyniki procedury składowanej są wyświetlane w Okno Dane wyjściowe](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Rysunek 2**. wyniki procedury składowanej s są wyświetlane w okno dane wyjściowe ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-custom-paged-data-vb/_static/image4.png))

> [!NOTE]
> Podczas klasyfikowania wyników według określonej `ORDER BY` kolumnie w klauzuli `OVER`, SQL Server muszą sortować wyniki. Jest to szybka operacja, jeśli istnieje klastrowany indeks względem kolumn, w których wyniki są uporządkowane według lub jeśli istnieje indeks obejmujący, ale może być bardziej kosztowny w innym przypadku. Aby zwiększyć wydajność dla dostatecznie dużych zapytań, należy rozważyć dodanie indeksu nieklastrowanego dla kolumny, według której mają być uporządkowane wyniki. Aby uzyskać więcej informacji, zapoznaj się z [funkcjami klasyfikacji i wydajnością w SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) .

## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Krok 2: rozszerzanie dostępu do danych i warstw logiki biznesowej

Przy tworzeniu procedury składowanej `GetProductsPagedAndSorted`, następnym krokiem jest zapewnienie środków do wykonania tej procedury składowanej za pośrednictwem architektury aplikacji. Pociąga to za sobą dodanie odpowiedniej metody do DAL i LOGIKI biznesowej. Zacznijmy od dodania metody do DAL. Otwórz `Northwind.xsd` Typ zestawu danych, kliknij prawym przyciskiem myszy `ProductsTableAdapter`i wybierz opcję Dodaj zapytanie z menu kontekstowego. Tak jak w poprzednim samouczku, chcemy skonfigurować tę nową metodę DAL, aby używać istniejącej procedury składowanej — `GetProductsPagedAndSorted`w tym przypadku. Zacznij od wskazywania, że nowa metoda TableAdapter ma używać istniejącej procedury składowanej.

![Wybierz użycie istniejącej procedury składowanej](sorting-custom-paged-data-vb/_static/image5.png)

**Rysunek 3**. wybór użycia istniejącej procedury składowanej

Aby określić procedurę przechowywaną do użycia, wybierz procedurę składowaną `GetProductsPagedAndSorted` z listy rozwijanej na następnym ekranie.

![Użyj procedury składowanej GetProductsPagedAndSorted](sorting-custom-paged-data-vb/_static/image6.png)

**Rysunek 4**. Używanie procedury składowanej GetProductsPagedAndSorted

Ta procedura składowana zwraca zestaw rekordów jako wyniki, więc na następnym ekranie wskaże, że zwraca dane tabelaryczne.

![Wskaż, że procedura składowana zwraca dane tabelaryczne](sorting-custom-paged-data-vb/_static/image7.png)

**Rysunek 5**. wskazanie, że procedura składowana zwraca dane tabelaryczne

Na koniec Utwórz metody DAL, które korzystają zarówno z wypełniania elementu DataTable, jak i zwracają wzorce tabeli DataTable, odpowiednio nadające się do metod `FillPagedAndSorted` i `GetProductsPagedAndSorted`.

![Wybierz nazwy metod](sorting-custom-paged-data-vb/_static/image8.png)

**Ilustracja 6**. Wybieranie nazw metod

Teraz, gdy DALmy, dodaliśmymy do LOGIKI biznesowej. Otwórz plik klasy `ProductsBLL` i Dodaj nową metodę `GetProductsPagedAndSorted`. Ta metoda musi przyjmować trzy parametry wejściowe `sortExpression`, `startRowIndex`i `maximumRows` i powinna po prostu wywołać metodę `GetProductsPagedAndSorted` DAL s, na przykład:

[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Krok 3. Konfigurowanie elementu ObjectDataSource do przekazania w parametrze 'Sortexpression

Po rozszerzeniu DAL i LOGIKI biznesowej w celu uwzględnienia metod, które wykorzystują procedurę składowaną `GetProductsPagedAndSorted`, to wszystko to, aby skonfigurować element ObjectDataSource na stronie `SortParameter.aspx`, aby użyć nowej metody LOGIKI biznesowej i przekazać parametr `SortExpression` na podstawie kolumny, według której użytkownik zażądał sortowania wyników.

Zacznij od zmiany `SelectMethod` ObjectDataSource s z `GetProductsPaged` na `GetProductsPagedAndSorted`. Można to zrobić za pomocą Kreatora konfiguracji źródła danych, z okno Właściwości lub bezpośrednio za pomocą składni deklaracyjnej. Następnie musimy podać wartość [właściwości`SortParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)ObjectDataSource s. Jeśli ta właściwość jest ustawiona, element ObjectDataSource próbuje przekazać właściwość GridView `SortExpression` w `SelectMethod`. W szczególności element ObjectDataSource szuka parametru wejściowego, którego nazwa jest równa wartości właściwości `SortParameterName`. Ponieważ LOGIKI biznesowej s Metoda `GetProductsPagedAndSorted` ma parametr wejściowy wyrażenia sortowania o nazwie `sortExpression`, ustaw właściwość ObjectDataSource s `SortExpression` na 'Sortexpression.

Po wprowadzeniu tych dwóch zmian składnia deklaracyjnego elementu ObjectDataSource powinna wyglądać podobnie do poniższego:

[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Tak jak w poprzednim samouczku, upewnij się, że element ObjectDataSource *nie zawiera parametrów* wejściowych 'Sortexpression, StartRowIndex lub MaximumRows w swojej kolekcji SelectParameters.

Aby włączyć sortowanie w widoku GridView, po prostu zaznacz pole wyboru Włącz sortowanie w tagu "GridView s", które ustawia właściwość GridView s `AllowSorting` na `true` i powoduje, że tekst nagłówka dla każdej kolumny będzie renderowany jako element LinkButton. Po kliknięciu przez użytkownika końcowego jednego z nagłówków LinkButtons, ogłaszania zwrotnego i następujących krokach wykonywanych:

1. Widok GridView aktualizuje swoją [właściwość`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) do wartości `SortExpression` pola, którego łącze nagłówka zostało kliknięte
2. Element ObjectDataSource wywołuje metodę LOGIKI biznesowej `GetProductsPagedAndSorted`, przekazując właściwość GridView s `SortExpression` jako wartość parametru wejściowego metody s `sortExpression` (wraz z odpowiednimi `startRowIndex` i `maximumRows` wartościami parametru wejściowego)
3. LOGIKI biznesowej wywołuje metodę DAL s `GetProductsPagedAndSorted`
4. DAL wykonuje procedurę składowaną `GetProductsPagedAndSorted`, przekazując parametr `@sortExpression` (wraz z wartościami parametrów wejściowych `@startRowIndex` i `@maximumRows`)
5. Procedura składowana zwraca odpowiedni podzestaw danych do LOGIKI biznesowej, który zwraca go do elementu ObjectDataSource; te dane są następnie powiązane z elementem GridView, renderowane w formacie HTML i wysyłane do użytkownika końcowego

Rysunek 7 przedstawia pierwszą stronę wyników posortowaną według `UnitPrice` w kolejności rosnącej.

[![wyniki są sortowane według ceny jednostkowej](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Rysunek 7**. wyniki są sortowane według wartości CenaJednostkowa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-custom-paged-data-vb/_static/image11.png))

Bieżąca implementacja może poprawnie sortować wyniki według nazwy produktu, nazwy kategorii, ilości na jednostkę i ceny jednostkowej, a próby uporządkowania wyników według nazwy dostawcy są wynikiem wyjątku czasu wykonywania (patrz rysunek 8).

![Podjęto próbę posortowania wyników przez dostawcę w następującym wyjątku czasu wykonywania](sorting-custom-paged-data-vb/_static/image12.png)

**Ilustracja 8**. próba sortowania wyników przez dostawcę w wyniku następującego wyjątku środowiska uruchomieniowego

Ten wyjątek występuje, ponieważ `SortExpression` w widoku GridView s `SupplierName` BoundField jest ustawiony na `SupplierName`. Jednak nazwa dostawcy w tabeli `Suppliers` jest w rzeczywistości wywoływana `CompanyName` nazwa kolumny została aliasem jako `SupplierName`. Jednak klauzula `OVER` używana przez funkcję `ROW_NUMBER()` nie może używać aliasu i musi używać rzeczywistej nazwy kolumny. W związku z tym Zmień `SupplierName` BoundField s `SortExpression` od NazwaDostawcy na NazwaFirmy (patrz rysunek 9). Jak pokazano na rysunku 10, po wprowadzeniu tej zmiany wyniki mogą być sortowane przez dostawcę.

![Zmień wartość NazwaDostawcy BoundField s 'Sortexpression na NazwaFirmy](sorting-custom-paged-data-vb/_static/image13.png)

**Rysunek 9**. zmiana dostawcy BoundField s 'Sortexpression na NazwaFirmy

[![wyniki mogą teraz być posortowane według dostawcy](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Ilustracja 10**. wyniki mogą teraz być sortowane według dostawcy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-custom-paged-data-vb/_static/image16.png))

## <a name="summary"></a>Podsumowanie

Implementacja stronicowania niestandardowego, która została sprawdzona w poprzednim samouczku, wymagała, aby kolejność sortowania wyników była określona w czasie projektowania. W skrócie, oznacza to, że implementacja stronicowania niestandardowego nie została zaimplementowana w tym samym czasie, zapewnia możliwości sortowania. W tym samouczku overcame to ograniczenie przez rozszerzenie procedury składowanej od pierwszego do uwzględnienia `@sortExpression` parametru wejściowego, w którym można sortować wyniki.

Po utworzeniu tej procedury składowanej i utworzeniu nowych metod w DAL i LOGIKI biznesowej można zaimplementować widok GridView, który oferuje sortowanie i niestandardowe stronicowanie przez skonfigurowanie elementu ObjectDataSource do przekazania właściwości Current `SortExpression` w widoku GridView do `SelectMethod`LOGIKI biznesowej.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Carlos Santos. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](efficiently-paging-through-large-amounts-of-data-vb.md)
> [dalej](creating-a-customized-sorting-user-interface-vb.md)
