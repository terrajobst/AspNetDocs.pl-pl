---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Wzorzec/Szczegóły przy użyciu listy punktowanej rekordów głównych z szczegółami DataList (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku będziemy kompresować raport wzorzec/szczegóły dwóch stron z poprzedniego samouczka do jednej strony, pokazując listę punktowaną nazw kategorii w t...
ms.author: riande
ms.date: 10/17/2006
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 81d72c666925e89729464e7ea696bde8d323e277
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74641726"
---
# <a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Formularz rekord główny/szczegóły korzystający z listy punktowanej rekordów głównych z kontrolką DataList szczegółów (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) lub [Pobierz plik PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> W tym samouczku będziemy kompresować raport wzorzec/szczegóły dwóch stron z poprzedniego samouczka do pojedynczej strony, pokazując listę punktowaną z nazwami kategorii po lewej stronie ekranu i produkty wybranej kategorii z prawej strony ekranu.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](master-detail-filtering-acess-two-pages-datalist-vb.md) pokazano, jak rozdzielić Raport główny/szczegółowy na dwie strony. Na stronie wzorcowej użyto kontrolki wzmacniak do renderowania listy punktowanej kategorii. Każda nazwa kategorii była hiperłączem, które po kliknięciu spowoduje przełączenie go do strony szczegółów, gdzie dwie kolumny DataList wskazują te produkty należące do wybranej kategorii.

W tym samouczku będziemy kompresować samouczek dwustronicowy do jednej strony, pokazując listę punktowaną nazw kategorii po lewej stronie ekranu z każdą nazwą kategorii renderowaną jako element LinkButton. Kliknięcie jednej z nazw kategorii LinkButtons wywołuje ogłaszanie zwrotne i wiąże wybrane produkty kategorii s z dwiema kolumnami DataList po prawej stronie ekranu. Oprócz wyświetlania każdej kategorii, wzmacniak po lewej stronie pokazuje, ile łącznych produktów istnieje dla danej kategorii (patrz rysunek 1).

[![nazwa kategorii i całkowita liczba produktów są wyświetlane po lewej stronie](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Rysunek 1**: Nazwa kategorii i łączna liczba produktów są wyświetlane po lewej stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))

## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Krok 1. Wyświetlanie Wzmacniakka w lewej części ekranu

W tym samouczku musimy znajdować się lista punktowana kategorii z lewej strony wybranych produktów kategorii. Zawartość na stronie sieci Web można umieścić przy użyciu standardowych tagów akapitu w języku HTML, spacji rozmieszczonych, `<table>` s itd. lub za pośrednictwem technik kaskadowych arkuszy stylów (CSS). We wszystkich naszych samouczkach użyto technik CSS do pozycjonowania. Po skompilowaniu interfejsu użytkownika nawigacji na naszej stronie wzorcowej na [stronach wzorcowych i](../introduction/master-pages-and-site-navigation-vb.md) w samouczku nawigacji witryny użyto *pozycjonowania bezwzględnego*, wskazując dokładne przesunięcie pikseli dla listy nawigacji i głównej zawartości. Alternatywnie można użyć CSS do pozycjonowania jednego elementu w prawo lub w lewo od drugiego za pomocą wartości *zmiennoprzecinkowych*. Lista punktowana kategorii pojawia się po lewej stronie wybranych produktów kategorii, przez przepływający wzmacniak z lewej strony elementu DataList

Otwórz stronę `CategoriesAndProducts.aspx` z folderu `DataListRepeaterFiltering` i Dodaj do strony jako wzmacniak i element DataList. Ustaw `ID` wzmacniak `Categories` i elementu DataList s, aby `CategoryProducts`. Przejdź do widoku źródła i umieść kontrolki wzmacniak i DataList we własnym `<div>` elementów. Oznacza to, że najpierw należy umieścić wzmacniak w obrębie elementu `<div>`, a następnie element DataList we własnym elemencie `<div>` bezpośrednio po Wzmacniake. Twoje znaczniki powinny wyglądać podobnie do następujących:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Aby przestawić wzmacniak z lewej strony DataList, musimy użyć atrybutu stylu CSS `float`, takiego jak:

[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

`float: left;` powoduje przepływanie pierwszego elementu `<div>` z lewej strony drugiego. Ustawienia `width` i `padding-right` wskazują pierwsze `<div>` s `width` i ilość dopełnienia między `<div>` zawartości elementu a jego prawego marginesu. Aby uzyskać więcej informacji na temat elementów zmiennoprzecinkowych w CSS, zapoznaj się z [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Zamiast określać ustawienie stylu bezpośrednio przy użyciu pierwszego `<p>` atrybutu `style` elementu, zezwól zamiast tego utworzyć nową klasę CSS w `Styles.css` o nazwie `FloatLeft`:

[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Następnie możemy zastąpić `<div>` z `<div class="FloatLeft">`.

Po dodaniu klasy CSS i skonfigurowaniu znaczników na stronie `CategoriesAndProducts.aspx` przejdź do projektanta. Powinieneś zobaczyć, że wzmacniak jest przepływany po lewej stronie elementu DataList (mimo że teraz po prostu pojawiają się szare pola, ponieważ jeszcze nie skonfigurowano ich źródeł danych lub szablonów).

[![wzmacniak jest przestawiony z lewej strony elementu DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Rysunek 2**. wzmacniak jest przepływany po lewej stronie elementu DataList ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))

## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Krok 2. Określanie liczby produktów dla każdej kategorii

Po zakończeniu adiustacji z Wzmacniaką i elementem DataList s, wszystko gotowe do powiązania danych kategorii z formantem wzmacniania. Jednak jako lista punktowana kategorii na rysunku 1 pokazuje, oprócz nazw kategorii s, należy również wyświetlić liczbę produktów skojarzonych z kategorią. Aby uzyskać dostęp do tych informacji, możemy:

- **Należy określić te informacje z klasy ASP.NET strony powiązanej z kodem.** Mając określoną *`categoryID`* , możemy określić liczbę skojarzonych produktów, wywołując metodę `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)`. Ta metoda zwraca obiekt `ProductsDataTable`, którego właściwość `Count` wskazuje, ile `ProductsRow` s istnieje, czyli liczbę produktów dla określonego *`categoryID`* . Możemy utworzyć procedurę obsługi zdarzeń `ItemDataBound` dla wzmacniak, która dla każdej kategorii powiązanej z wzmacniak wywoła metodę `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` i zawiera jej liczbę w danych wyjściowych.
- **Zaktualizuj `CategoriesDataTable` w określonym zestawie danych, aby dołączyć `NumberOfProducts` kolumnę.** Następnie możemy zaktualizować metodę `GetCategories()` w `CategoriesDataTable`, aby uwzględnić te informacje lub, Alternatywnie, pozostawić `GetCategories()` jako-is i utworzyć nową metodę `CategoriesDataTable` o nazwie `GetCategoriesAndNumberOfProducts()`.

Poznanie obu tych technik. Pierwsze podejście jest prostsze do wdrożenia, ponieważ nie trzeba aktualizować warstwy dostępu do danych. jednak wymaga więcej komunikacji z bazą danych. Wywołanie metody `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)` w obsłudze zdarzeń `ItemDataBound` powoduje dodanie dodatkowego wywołania bazy danych dla każdej kategorii wyświetlanej w Wzmacniake. W tej metodzie istnieją *n* + 1 wywołania bazy danych, gdzie *N* to liczba kategorii wyświetlanych w wzmacniake. Drugim podejściem jest zwrócenie liczby produktów z informacjami o każdej kategorii z metody `CategoriesBLL` Class s `GetCategories()` (lub `GetCategoriesAndNumberOfProducts()`), co powoduje tylko jedną podróż do bazy danych.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Określanie liczby produktów w programie obsługi zdarzeń ItemDataBound

Określenie liczby produktów dla każdej kategorii w obsłudze zdarzeń `ItemDataBound` wzmacniak nie wymaga żadnych modyfikacji istniejącej warstwy dostępu do danych. Wszystkie modyfikacje można wprowadzać bezpośrednio na stronie `CategoriesAndProducts.aspx`. Zacznij od dodania nowego elementu ObjectDataSource o nazwie `CategoriesDataSource` za pośrednictwem tagu inteligentnego "wzmacniak s". Następnie skonfiguruj `CategoriesDataSource` element ObjectDataSource, tak aby pobierał dane z metody `CategoriesBLL` klasy s `GetCategories()`.

[![skonfigurować element ObjectDataSource do używania metody GetCategories () klasy CategoriesBLL](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Rysunek 3**. Konfigurowanie elementu ObjectDataSource do używania metody `GetCategories()` `CategoriesBLL` Class (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))

Każdy element w Wzmacniake `Categories` musi być kliknięty i, po kliknięciu, spowodować, że `CategoryProducts` DataList wyświetla te produkty dla wybranej kategorii. Można to zrobić, tworząc każdą kategorię hiperłącze, łącząc się z powrotem z tą samą stroną (`CategoriesAndProducts.aspx`), ale przekazując `CategoryID` za pomocą ciągu QueryString, podobnie jak pokazano w poprzednim samouczku. Zaletą tego podejścia jest to, że strona wyświetlająca konkretne produkty kategorii nie może być oznaczona zakładką i indeksowana przez wyszukiwarkę.

Alternatywnie możemy wprowadzić każdą kategorię a element LinkButton, która jest podejściem do użycia w tym samouczku. Element LinkButton renderuje w przeglądarce User s jako hiperłącze, ale po kliknięciu, wywołuje ogłaszanie zwrotne; na stronie ogłaszania zwrotnego element ObjectDataSource s musi zostać odświeżony, aby wyświetlić te produkty należące do wybranej kategorii. Na potrzeby tego samouczka użycie hiperłącza sprawia, że jest to element LinkButton; mogą jednak wystąpić inne scenariusze, w których użycie element LinkButton jest bardziej korzystne. Mimo że podejście Hyperlink byłoby idealne dla tego przykładu, poczekaj, aż zostanie zbadane przy użyciu element LinkButton. Jak zobaczymy, korzystanie z element LinkButton wprowadza pewne wyzwania, które w przeciwnym razie nie wystąpią z hiperłączem. W związku z tym przy użyciu element LinkButton w tym samouczku zostaną wyróżnione te wyzwania i pomoc dotycząca rozwiązań dla tych scenariuszy, w których firma Microsoft może chcieć użyć element LinkButton zamiast hiperlinku.

> [!NOTE]
> Zaleca się powtórzenie tego samouczka przy użyciu kontrolki HyperLink lub `<a>` elementu zamiast element LinkButton.

W poniższym znaczniku przedstawiono składnię deklaratywną dla wzmacniak i element ObjectDataSource. Należy zauważyć, że szablony wzmacniak renderuje listę punktowaną z każdym elementem jako element LinkButton:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> W tym samouczku wzmacniak musi mieć włączony stan widoku (należy zwrócić uwagę na pominięcie `EnableViewState="False"` ze składni deklaracyjnej. W kroku 3 utworzysz procedurę obsługi zdarzeń dla zdarzenia `ItemCommand`, w którym będziemy aktualizować kolekcję elementów ObjectDataSource s `SelectParameters`. Niemniej, gdy stan widoku jest wyłączony, nie będzie on wyzwalany `ItemCommand`. Aby uzyskać więcej informacji na temat sytuacji, w której stan widoku musi być włączony, aby można było uruchomić `ItemCommand`, zobacz temat [Stumper pytania ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) i [jego rozwiązanie](http://scottonwriting.net/sowBlog/posts/1268.aspx) .

Element LinkButton z wartością właściwości `ID` `ViewCategory` nie ma ustawionej `Text` właściwości. Jeśli udało Ci się wyświetlić nazwę kategorii, ustawimy właściwość Text w sposób deklaratywny za pomocą składni wiązania danych, na przykład:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Jednak chcemy wyświetlić zarówno nazwę kategorii *, jak i* liczbę produktów należących do tej kategorii. Te informacje można pobrać z programu obsługi zdarzeń `ItemDataBound` wzmacniak, wykonując wywołanie do metody `ProductBLL` klasy s `GetCategoriesByProductID(categoryID)` i określając, ile rekordów jest zwracanych w wynikowym `ProductsDataTable`, jak ilustruje poniższy kod:

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Firma Microsoft rozpoczyna pracę, upewniając się, że pracujemy nad elementem danych (który `ItemType` jest `Item` lub `AlternatingItem`), a następnie odwołuje się do wystąpienia `CategoriesRow`, które zostało właśnie powiązane z bieżącym `RepeaterItem`. Następnie określimy liczbę produktów dla tej kategorii, tworząc wystąpienie klasy `ProductsBLL`, wywołując metodę `GetCategoriesByProductID(categoryID)` i określając liczbę rekordów zwracanych przy użyciu właściwości `Count`. Wreszcie `ViewCategory` element LinkButton w ItemTemplate jest odwołaniem i jego właściwość `Text` jest ustawiona na *CategoryName* (*NumberOfProductsInCategory*), gdzie *NumberOfProductsInCategory* jest sformatowana jako liczba z zerowymi miejscami dziesiętnymi.

> [!NOTE]
> Alternatywnie możemy dodać *funkcję formatowania* do klasy ASP.NET strony z kodem, która akceptuje `CategoryName` i `CategoryID` wartości, i zwraca `CategoryName` połączony z liczbą produktów w kategorii (zgodnie z wywołaniem metody `GetCategoriesByProductID(categoryID)`). Wyniki takiej funkcji formatowania można deklaratywnie przypisać do właściwości text element LinkButton s zastępującej potrzebę obsługi zdarzeń `ItemDataBound`. Aby uzyskać więcej informacji na temat korzystania z funkcji formatowania, zapoznaj się z tematem [using używanie TemplateField w formancie GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) lub [formatowaniem DataList oraz wzmacniak na podstawie](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) samouczków dotyczących danych.

Po dodaniu tego programu obsługi zdarzeń Poświęć chwilę na przetestowanie strony za pomocą przeglądarki. Zwróć uwagę na to, w jaki sposób każda kategoria jest wymieniona na liście punktowanej, wyświetlając kategorię s i liczbę produktów skojarzonych z kategorią (patrz rysunek 4).

[zostanie wyświetlona ![każdej kategorii i liczbie produktów](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Rysunek 4**. wyświetlana jest Nazwa każdej kategorii i liczba produktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))

## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aktualizowanie`CategoriesDataTable`i`CategoriesTableAdapter`w celu uwzględnienia liczby produktów dla każdej kategorii

Zamiast określania liczby produktów dla każdej kategorii jako s powiązanej z Wzmacniaką, możemy usprawnić ten proces przez dostosowanie `CategoriesDataTable` i `CategoriesTableAdapter` w warstwie dostępu do danych w celu dołączenia tych informacji natywnie. Aby to osiągnąć, należy dodać nową kolumnę do `CategoriesDataTable`, aby pomieścić liczbę skojarzonych produktów. Aby dodać nową kolumnę do elementu DataTable, Otwórz wybrany zestaw danych (`App_Code\DAL\Northwind.xsd`), kliknij prawym przyciskiem myszy element DataTable do zmodyfikowania, a następnie wybierz polecenie Dodaj/kolumna. Dodaj nową kolumnę do `CategoriesDataTable` (patrz rysunek 5).

[![dodać nowej kolumny do CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Ilustracja 5**. Dodawanie nowej kolumny do `CategoriesDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))

Spowoduje to dodanie nowej kolumny o nazwie `Column1`, którą można zmienić, wpisując inną nazwę. Zmień nazwę tej nowej kolumny na `NumberOfProducts`. Następnie należy skonfigurować właściwości kolumny s. Kliknij nową kolumnę i przejdź do okno Właściwości. Zmień Właściwość Column s `DataType` z `System.String` na `System.Int32` i ustaw właściwość `ReadOnly` na `True`, jak pokazano na rysunku 6.

![Ustaw właściwości DataType i ReadOnly nowej kolumny](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Ilustracja 6**. ustawianie właściwości `DataType` i `ReadOnly` nowej kolumny

Gdy `CategoriesDataTable` ma teraz kolumnę `NumberOfProducts`, jej wartość nie jest ustawiana przez żadną z odpowiednich zapytań TableAdapter s. Możemy zaktualizować metodę `GetCategories()`, aby zwracała te informacje, jeśli chcesz, aby te informacje były zwracane za każdym razem, gdy są pobierane informacje o kategorii. Jeśli jednak potrzebujemy tylko, aby dowiedzieć się więcej o liczbie skojarzonych produktów dla kategorii w rzadkich wystąpieniach (na przykład tylko w tym samouczku), możemy pozostawić `GetCategories()` jako-is i utworzyć nową metodę zwracającą te informacje. Zezwól na użycie tego ostatniego podejścia, tworząc nową metodę o nazwie `GetCategoriesAndNumberOfProducts()`.

Aby dodać tę nową metodę `GetCategoriesAndNumberOfProducts()`, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` i wybierz polecenie nowe zapytanie. Spowoduje to wyświetlenie Kreatora konfiguracji zapytania TableAdapter, którego użyto wiele razy w poprzednich samouczkach. W przypadku tej metody Uruchom kreatora, wskazując, że zapytanie używa instrukcji SQL ad hoc, która zwraca wiersze.

[![utworzyć metody przy użyciu instrukcji SQL ad hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Rysunek 7**. Tworzenie metody przy użyciu instrukcji SQL ad hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))

[![instrukcja SQL zwraca wiersze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Ilustracja 8**. Instrukcja SQL zwraca wiersze ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))

Na następnym ekranie kreatora zostanie wyświetlony komunikat z prośbą o użycie zapytania. Aby zwrócić każdą kategorię `CategoryID`, `CategoryName`i `Description` pola wraz z liczbą produktów skojarzonych z kategorią, należy użyć następującej instrukcji `SELECT`:

[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]

[![określić zapytania do użycia](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Ilustracja 9**. Określanie zapytania do użycia ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))

Należy zauważyć, że podzapytanie, które oblicza liczbę produktów skojarzonych z kategorią, jest aliasem jako `NumberOfProducts`. Takie dopasowanie nazewnictwa powoduje, że wartość zwracana przez to podzapytanie ma być skojarzona z kolumną `CategoriesDataTable` s `NumberOfProducts`.

Po wprowadzeniu tego zapytania, ostatnim krokiem jest wybranie nazwy nowej metody. Użyj `FillWithNumberOfProducts` i `GetCategoriesAndNumberOfProducts` do wypełnienia elementu DataTable i zwrócenia odpowiednio wzorców DataTable.

[Nazwa ![nowe metody TableAdapter s FillWithNumberOfProducts i GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Ilustracja 10**. Nazwij nowe metody TableAdapter `FillWithNumberOfProducts` i `GetCategoriesAndNumberOfProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))

W tym momencie Warstwa dostępu do danych została rozszerzona w celu uwzględnienia liczby produktów na kategorię. Ponieważ cała nasza warstwa prezentacji kieruje wszystkie wywołania do DAL za pomocą oddzielnej warstwy logiki biznesowej, należy dodać odpowiednią metodę `GetCategoriesAndNumberOfProducts` do klasy `CategoriesBLL`:

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Po ukończeniu DAL i LOGIKI biznesowej wszystko gotowe do powiązania tych danych z Wzmacniaką `Categories` w `CategoriesAndProducts.aspx`. Jeśli został już utworzony element ObjectDataSource dla wzmacniak z określania liczby produktów w sekcji obsługi zdarzeń `ItemDataBound`, Usuń ten element ObjectDataSource i Usuń ustawienie właściwości wzmacniak-`DataSourceID`; Usuń także zdarzenie `ItemDataBound` wzmacniak z obsługi zdarzeń, usuwając składnię `Handles Categories.OnItemDataBound` w klasie z kodem ASP.NET.

Z powrotem w pierwotnym stanie, Dodaj nowy element ObjectDataSource o nazwie `CategoriesDataSource` za pośrednictwem tagu inteligentnego wzmacniak. Skonfiguruj element ObjectDataSource do używania klasy `CategoriesBLL`, ale zamiast używać metody `GetCategories()`, użyj `GetCategoriesAndNumberOfProducts()` zamiast tego (patrz rysunek 11).

[![skonfigurować element ObjectDataSource do korzystania z metody GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Ilustracja 11**. Konfigurowanie elementu ObjectDataSource do używania metody `GetCategoriesAndNumberOfProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))

Następnie zaktualizuj `ItemTemplate` tak, aby właściwość element LinkButton s `Text` została deklaratywnie przypisana przy użyciu składni DataBinding i zawiera pola danych `CategoryName` i `NumberOfProducts`. Kompletny, deklaratywny znacznik dla wzmacniak i `CategoriesDataSource` ObjectDataSource:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

Dane wyjściowe renderowane przez aktualizację DAL w celu uwzględnienia kolumny `NumberOfProducts` są takie same jak przy użyciu metody obsługi zdarzeń `ItemDataBound` (zapoznaj się z powrotem do rysunku 4, aby wyświetlić zrzut ekranu przedstawiający kod kategorii i liczbę produktów).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Krok 3. Wyświetlanie wybranych produktów kategorii s

W tym momencie mamy `Categories` wzmacniak wyświetlający listę kategorii wraz z liczbą produktów w każdej kategorii. Wzmacniak używa elementu element LinkButton dla każdej kategorii, która po kliknięciu powoduje odświeżenie. w tym momencie musimy wyświetlić te produkty dla wybranej kategorii na `CategoryProducts` DataList.

Jedną z wyzwań skierowanych do nas jest sposób wyświetlania tylko tych produktów dla wybranej kategorii. W obszarze [wzorzec/szczegóły przy użyciu dostępnego do wyboru wzorca GridView z szczegółowym](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) samouczkiem DetailsView znaleźliśmy, w jaki sposób utworzyć widok GridView, którego wiersze mogą być zaznaczone, a szczegóły wybranego wiersza s są wyświetlane w widoku DetailsView na tej samej stronie. Element GridView. ObjectDataSource s zwrócił informacje o wszystkich produktach korzystających z metody `ProductsBLL` s `GetProducts()`, podczas gdy element ObjectDataSource s w widoku DetailsView pobrał informacje o wybranym produkcie przy użyciu metody `GetProductsByProductID(productID)`. Wartość parametru *`productID`* podano deklaratywnie, kojarząc ją z wartością właściwości GridView s `SelectedValue`. Niestety, wzmacniak nie ma właściwości `SelectedValue` i nie może stanowić źródła parametru.

> [!NOTE]
> Jest to jedno z wyzwań, które pojawiają się podczas korzystania z element LinkButton w Wzmacniake. Czy użyto hiperłącza do przekazania `CategoryID` za pomocą ciągu QueryString zamiast tego, możemy użyć tego pola QueryString jako źródła dla wartości parametru s.

Zanim będziemy obawiać się o braku `SelectedValue` właściwości dla Wzmacniakka, należy najpierw popowiązać element DataList z elementem ObjectDataSource i określić jego `ItemTemplate`.

W tagu inteligentnym DataList s Dodaj nowy element ObjectDataSource o nazwie `CategoryProductsDataSource` i skonfiguruj go tak, aby korzystał z metody `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL`. Ponieważ element DataList w tym samouczku oferuje interfejs tylko do odczytu, możesz bezpłatnie ustawić listę rozwijaną na kartach Wstawianie, aktualizowanie i usuwanie do (brak).

[![skonfigurować element ObjectDataSource do używania metody ProductsBLL Class s GetProductsByCategoryID (IDKategorii)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Ilustracja 12**. Skonfiguruj element ObjectDataSource, aby używał metody `GetProductsByCategoryID(categoryID)` `ProductsBLL` klasy s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))

Ponieważ metoda `GetProductsByCategoryID(categoryID)` oczekuje parametru wejściowego ( *`categoryID`* ), Kreator konfiguracji źródła danych pozwala nam określić źródło parametru s. Kategorie zostały wymienione w widoku GridView lub DataList, ale ustawimy listę rozwijaną Źródło parametru na kontrolkę i ControlID do `ID` formantu sieci Web danych. Jednakże, ponieważ wzmacniak nie ma właściwości `SelectedValue`, nie można jej użyć jako źródła parametru. W przypadku sprawdzenia, że lista rozwijana ControlID zawiera tylko jedną kontrolkę `ID``CategoryProducts`, `ID` elementu DataList.

Na razie Ustaw listę rozwijaną Źródło parametru na wartość Brak. Będziemy w stanie programowo przypisywać tę wartość parametru, gdy element LinkButton kategorii zostanie kliknięty w Wzmacniake.

[![nie określać źródła parametrów dla parametru IDKategorii](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Ilustracja 13**. nie określaj źródła parametrów dla parametru *`categoryID`* ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio automatycznie generuje `ItemTemplate`DataList s. Zastąp to domyślne `ItemTemplate` szablonem użytym w poprzednim samouczku; Ponadto ustaw Właściwość DataList s `RepeatColumns` na 2. Po wprowadzeniu tych zmian znaczniki deklaratywne dla elementu DataList i skojarzony z nim element ObjectDataSource powinny wyglądać następująco:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Obecnie parametr *`categoryID`* `CategoryProductsDataSource` ObjectDataSource nie jest nigdy ustawiony, więc żadne produkty nie są wyświetlane podczas wyświetlania strony. Ta wartość parametru musi być ustawiona na podstawie `CategoryID` klikniętej kategorii w Wzmacniake. W ten sposób wprowadzono dwa wyzwania: najpierw, jak określić, kiedy element LinkButton się w `ItemTemplate` Wzmacniake. i sekundę, jak możemy określić `CategoryID` odpowiedniej kategorii, której kliknięto element LinkButton?

Element LinkButton, podobnie jak przyciski i kontrolki kliknięto element ImageButton, mają zdarzenie `Click` i [zdarzenie`Command`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). Zdarzenie `Click` zostało zaprojektowane w taki sposób, aby po kliknięciu element LinkButton. Czasami jednak oprócz tego, że element LinkButton został kliknięty, konieczne jest również przekazanie pewnych dodatkowych informacji do programu obsługi zdarzeń. W takim przypadku element LinkButton [`CommandName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) i właściwości [`CommandArgument`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) można przypisać do tych dodatkowych informacji. Następnie po kliknięciu element LinkButton zostanie wyzwolone jego zdarzenie `Command` (zamiast zdarzenia `Click`), a program obsługi zdarzeń przekazał wartości `CommandName` i `CommandArgument` właściwości.

Gdy zdarzenie `Command` jest wywoływane z poziomu szablonu w Wzmacniake, wyzwolone zostało [zdarzenie`ItemCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) wzmacniak i zostanie przeniesiona `CommandName` i `CommandArgument` wartości klikniętego element LinkButton (lub kliknięto element ImageButton). W związku z tym, aby określić, kiedy kliknięto kategorię element LinkButton w Wzmacniake, musimy wykonać następujące czynności:

1. Ustaw właściwość `CommandName` element LinkButton w `ItemTemplate` wzmacniaka na inną wartość (chcę użyć ListProducts). Ustawiając tę wartość `CommandName`, zdarzenie element LinkButton s `Command` wyzwalane po kliknięciu element LinkButton.
2. Ustaw właściwość element LinkButton s `CommandArgument` na wartość bieżącego elementu `CategoryID`.
3. Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemCommand` wzmacniak. W programie obsługi zdarzeń ustaw parametr `CategoryProductsDataSource` ObjectDataSource s `CategoryID` na wartość przekazaną `CommandArgument`.

Następujące `ItemTemplate` znaczniki dla wzmacniania kategorii implementują kroki 1 i 2. Zwróć uwagę na to, w jaki sposób wartość `CommandArgument` jest przypisywana do `CategoryID` elementu danych przy użyciu składni DataBinding:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Za każdym razem, gdy tworzony jest program obsługi zdarzeń `ItemCommand`, należy zawsze sprawdzić wartość przychodzącej `CommandName`, ponieważ *każde* zdarzenie `Command` wywołane przez *dowolny* przycisk, element LinkButton lub kliknięto element ImageButton w ramach tego wzmacniak spowoduje wyzwolenie zdarzenia `ItemCommand`. Obecnie mamy teraz tylko jeden taki element LinkButton, w przyszłości (lub inny deweloper w naszym zespole) może dodać dodatkowe kontrolki sieci Web przycisków do wzmacniak, który po kliknięciu wywołuje ten sam `ItemCommand` obsługę zdarzeń. W związku z tym najlepszym rozwiązaniem jest upewnienie się, że jest sprawdzana Właściwość `CommandName` i kontynuuje pracę tylko z logiką programistyczną, jeśli jest ona zgodna z oczekiwaną wartością.

Po upewnieniu się, że wartość `CommandName` przekazanie jest równa ListProducts, program obsługi zdarzeń przypisze parametr `CategoryProductsDataSource` ObjectDataSource s `CategoryID` do wartości przekazaną `CommandArgument`. Ta modyfikacja `SelectParameters` ObjectDataSource s automatycznie powoduje, że element DataList może ponownie powiązać siebie ze źródłem danych, pokazując produkty dla nowo wybranej kategorii.

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Dzięki tym dodatkom nasz samouczek jest gotowy! Poświęć chwilę na przetestowanie go w przeglądarce. Ilustracja 14 przedstawia ekran podczas pierwszego odwiedzania strony. Ponieważ kategoria jest jeszcze zaznaczona, nie są wyświetlane żadne produkty. Kliknięcie kategorii, takiej jak produkt, wyświetla te produkty w kategorii produkt w widoku dwóch kolumn (patrz rysunek 15).

[![podczas pierwszego odwiedzania strony nie są wyświetlane żadne produkty](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Ilustracja 14**. podczas pierwszego odwiedzania strony nie są wyświetlane żadne produkty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))

[![kliknięciu kategorii producent wyświetla listę pasujących produktów po prawej stronie](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Ilustracja 15**. kliknięcie kategorii produkcji powoduje wyświetlenie listy pasujących produktów z prawej strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))

## <a name="summary"></a>Podsumowanie

Jak wspomniano w tym samouczku, a poprzedni, główny/szczegółowy raport można rozłożyć na dwie strony lub skonsolidować na jednym z nich. Jednak wyświetlanie raportu wzorzec/szczegóły na pojedynczej stronie zawiera pewne wyzwania dotyczące sposobu układu wzorca i rekordów szczegółowych na stronie. We *wzorcu/szczegółach przy użyciu selektora w widoku szczegółowym, w którym szczegółowo* przedstawiono szczegółowe informacje, są wyświetlane nad rekordami głównymi. w tym samouczku użyto technik CSS, aby rekordy główne były przepływające z lewej strony szczegółów.

Wraz z wyświetlaniem raportów Master/Details mogliśmy poznać, jak pobrać liczbę produktów skojarzonych z każdą kategorią, a także jak wykonać logikę po stronie serwera, gdy element LinkButton (lub Button lub kliknięto element ImageButton) jest kliknięty w obrębie Wzmacniakka.

Ten samouczek wykonuje nasze badanie raportów master/detail za pomocą elementu DataList i wzmacniak. Nasz następny zestaw samouczków pokazuje, jak dodać możliwości edytowania i usuwania do formantu DataList.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) samouczek dotyczący elementów zmiennoprzecinkowych CSS z CSS
- [CSS pozycjonowanie](http://www.brainjar.com/css/positioning/) więcej informacji na temat pozycjonowania elementów za pomocą CSS
- [Układanie zawartości za pomocą kodu HTML](http://www.w3schools.com/html/html_layout.asp) przy użyciu `<table>` s i innych elementów HTML do pozycjonowania

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Zack Nowak. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Ubiegł](master-detail-filtering-acess-two-pages-datalist-vb.md)
