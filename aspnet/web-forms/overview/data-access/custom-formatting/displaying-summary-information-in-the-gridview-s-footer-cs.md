---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: Wyświetlanie informacji podsumowania w stopce GridView (C#) | Microsoft Docs
author: rick-anderson
description: Informacje podsumowujące są często wyświetlane u dołu raportu w wierszu podsumowania. Formant GridView może zawierać wiersz stopki, do którego są dostępne komórki...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: b258a2bdeaea8da4e9c5c5d8043b167d94e1e817
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617005"
---
# <a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Wyświetlanie informacji podsumowania w stopce kontrolki GridView (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) lub [Pobierz plik PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Informacje podsumowujące są często wyświetlane u dołu raportu w wierszu podsumowania. Formant GridView może zawierać wiersz stopki, do którego można programistycznie wstrzyknąć dane zagregowane. W tym samouczku zobaczymy, jak wyświetlić zagregowane dane w tym wierszu stopki.

## <a name="introduction"></a>Wprowadzenie

Oprócz wyświetlania poszczególnych cen produktów, jednostek w magazynie, jednostek w kolejności oraz poziomów zmiany kolejności, użytkownik może również zainteresować zagregowane informacje, takie jak średnia cena, Łączna liczba jednostek w magazynie i tak dalej. Takie informacje podsumowujące są często wyświetlane u dołu raportu w wierszu podsumowania. Formant GridView może zawierać wiersz stopki, do którego można programistycznie wstrzyknąć dane zagregowane.

To zadanie przedstawia nam trzy wyzwania:

1. Konfigurowanie widoku GridView, aby wyświetlić jego wiersz stopki
2. Określanie danych podsumowujących; Co to jest, w jaki sposób obliczamy średnią cenę lub łączną liczbę jednostek w magazynie?
3. Wprowadzanie danych podsumowujących do odpowiednich komórek w wierszu stopki

W tym samouczku dowiesz się, jak przezwyciężyć te wyzwania. W celu utworzenia strony, która wyświetla listę kategorii na liście rozwijanej z produktami z wybranej kategorii wyświetlanymi w widoku GridView. Widok GridView będzie zawierać wiersz stopki, który pokazuje średnią cenę i łączną liczbę jednostek w magazynie oraz kolejność produktów w tej kategorii.

[Informacje podsumowujące ![są wyświetlane w wierszu stopki GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Rysunek 1**. informacje podsumowujące są wyświetlane w wierszu stopki GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))

W tym samouczku, wraz z jego kategorią, interfejsem głównym/szczegółowym, kompiluje koncepcje omówione w wcześniejszym [przefiltrowaniu wzorzec/szczegóły przy użyciu](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczka DropDownList. Jeśli jeszcze nie pracujesz nad wcześniejszym samouczkiem, zrób to przed kontynuowaniem tego działania.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Krok 1. Dodawanie kategorii DropDownList i produktów GridView

Przed odnoszącą się do wypróbujemy z dodawaniem informacji podsumowujących do stopki GridView, najpierw wystarczy skompilować Raport główny/szczegółowy. Po wykonaniu tego pierwszego kroku zawarto informacje na temat sposobu dołączania danych podsumowania.

Zacznij od otwarcia strony `SummaryDataInFooter.aspx` w folderze `CustomFormatting`. Dodaj kontrolkę DropDownList i ustaw jej `ID`, aby `Categories`. Następnie kliknij link wybierz źródło danych z tagu inteligentnego DropDownList i wybierz polecenie Dodaj nowy element ObjectDataSource o nazwie `CategoriesDataSource`, który wywołuje metodę `GetCategories()` klasy `CategoriesBLL`.

[![dodać nowego elementu ObjectDataSource o nazwie CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Rysunek 2**. Dodawanie nowego elementu ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))

[![, że element ObjectDataSource wywołuje metodę GetCategories () klasy CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Rysunek 3**. Element ObjectDataSource wywołuje metodę `GetCategories()` klasy `CategoriesBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))

Po skonfigurowaniu elementu ObjectDataSource Kreator zwraca nam do Kreatora konfiguracji źródła danych DropDownList, z którego musimy określić wartość pola danych, która powinna być wyświetlana i która powinna odpowiadać wartości `ListItem` s DropDownList. Pole `CategoryName` wyświetlane i użyj `CategoryID` jako wartości.

[![użyć pól CategoryName i IDkategorii jako tekstu i wartości dla elementu ListItem odpowiednio](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Rysunek 4**. użyj pól `CategoryName` i `CategoryID` jako `Text` i `Value` odpowiednio dla `ListItem` s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))

W tym momencie mamy DropDownList (`Categories`), który zawiera listę kategorii w systemie. Teraz musimy dodać widok GridView, który zawiera listę produktów należących do wybranej kategorii. Przed wykonaniem tej czynności Poświęć chwilę na zaznaczenie pola wyboru Włącz autoogłaszanie w tagu inteligentnym DropDownList. Zgodnie z opisem w *przefiltrowaniu wzorzec/szczegóły przy użyciu* samouczka DropDownList, ustawiając właściwość `AutoPostBack` DropDownList na `true` strona zostanie zaksięgowana za każdym razem, gdy wartość DropDownList zostanie zmieniona. Spowoduje to odświeżenie widoku GridView, pokazując te produkty dla nowo wybranej kategorii. Jeśli właściwość `AutoPostBack` jest ustawiona na wartość `false` (domyślnie), zmiana kategorii nie spowoduje odświeżenia i w związku z tym nie spowoduje zaktualizowania wymienionych produktów.

[![zaznacz pole wyboru Włącz autoogłaszanie w tagu inteligentnym DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Rysunek 5**. Zaznacz pole wyboru Włącz autoogłaszanie w tagu inteligentnym DropDownList ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))

Dodaj kontrolkę GridView do strony, aby wyświetlić produkty dla wybranej kategorii. Ustaw `ID` GridView, aby `ProductsInCategory` i powiązać go z nowym elementem ObjectDataSource o nazwie `ProductsInCategoryDataSource`.

[![dodać nowego elementu ObjectDataSource o nazwie ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Ilustracja 6**. Dodawanie nowego elementu ObjectDataSource o nazwie `ProductsInCategoryDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))

Skonfiguruj element ObjectDataSource, aby wywołuje metodę `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL`.

[![, że element ObjectDataSource wywołuje metodę GetProductsByCategoryID (IDKategorii)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Rysunek 7**. Element ObjectDataSource wywołuje metodę `GetProductsByCategoryID(categoryID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))

Ponieważ metoda `GetProductsByCategoryID(categoryID)` przyjmuje parametr wejściowy, w ostatnim kroku kreatora możemy określić źródło wartości parametru. Aby wyświetlić te produkty z wybranej kategorii, należy pobrać parametr z `Categories` DropDownList.

[![pobrać wartości parametru IDKategorii z wybranych kategorii DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Ilustracja 8**. Pobieranie wartości parametru *`categoryID`* z wybranych kategorii DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))

Po zakończeniu działania kreatora widok GridView będzie miał BoundField dla każdej właściwości produktu. Wyczyśćmy te BoundFields tak, aby były wyświetlane tylko `ProductName`, `UnitPrice`, `UnitsInStock`i `UnitsOnOrder` BoundFields. Możesz dodać dowolne ustawienia na poziomie pola do pozostałej BoundFields (na przykład formatowania `UnitPrice` jako waluty). Po wprowadzeniu tych zmian znaczniki deklaratywne GridView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

W tym momencie mamy w pełni funkcjonalny raport wzorzec/szczegóły, który zawiera nazwę, cenę jednostkową, jednostki w magazynie i jednostki na potrzeby tych produktów, które należą do wybranej kategorii.

[![pobrać wartości parametru IDKategorii z wybranych kategorii DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Ilustracja 9**. Pobieranie wartości parametru *`categoryID`* z wybranych kategorii DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))

## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Krok 2. Wyświetlanie stopki w widoku GridView

Kontrolka GridView może wyświetlać zarówno nagłówek, jak i wiersz stopki. Wiersze te są wyświetlane w zależności od wartości właściwości `ShowHeader` i `ShowFooter`, z `ShowHeader` domyślną `true` i `ShowFooter` `false`. Aby dołączyć stopkę w GridView, wystarczy ustawić jej Właściwość `ShowFooter` na `true`.

[![ustawić wartość true dla właściwości ShowFooter GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Ilustracja 10**. ustaw właściwość `ShowFooter` GridView na `true` (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))

Wiersz stopki zawiera komórkę dla każdego z pól zdefiniowanych w widoku GridView; Jednak te komórki są domyślnie puste. Poświęć chwilę na wyświetlenie postępu w przeglądarce. Gdy właściwość `ShowFooter` jest teraz ustawiona na `true`, GridView zawiera pusty wiersz stopki.

[![GridView zawiera teraz wiersz stopki](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Ilustracja 11**. widok GridView zawiera teraz wiersz stopki ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))

Wiersz stopki na rysunku 11 nie jest wyrzucany, ponieważ ma białe tło. Utwórzmy `FooterStyle` klasy CSS w `Styles.css`, która określa ciemne czerwone tło, a następnie skonfiguruj `GridView.skin` plik skórki w motywie `DataWebControls`, aby przypisać tę klasę CSS do właściwości `FooterStyle`, `CssClass`. Jeśli potrzebujesz pędzla nad karnacjami i motywami, zapoznaj się z artykułem [Wyświetlanie danych za pomocą](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) samouczka elementu ObjectDataSource.

Zacznij od dodania następującej klasy CSS do `Styles.css`:

[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

Klasa CSS `FooterStyle` jest podobna do klasy `HeaderStyle`, chociaż kolor tła `HeaderStyle`jest ciemniejszy, a jego tekst jest wyświetlany pogrubioną czcionką. Ponadto tekst w stopce jest wyrównany do prawej, a tekst nagłówka jest wyśrodkowany.

Następnie, aby skojarzyć tę klasę CSS z stopką każdego GridView, Otwórz plik `GridView.skin` w motywie `DataWebControls` i ustaw właściwość `CssClass` `FooterStyle`. Następnie znacznik pliku powinien wyglądać następująco:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Jak pokazano na poniższym zrzucie ekranu, ta zmiana sprawia, że stopnie jest bardziej jasne.

[![wiersz stopki GridView ma teraz kolor tła Reddish](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Ilustracja 12**. wiersz stopki GridView ma teraz kolor tła Reddish ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))

## <a name="step-3-computing-the-summary-data"></a>Krok 3. przetwarzanie danych podsumowania

Gdy zostanie wyświetlona Stopka GridView, następnym wyzwaniem skierowanym do nas jest sposób obliczania danych podsumowania. Istnieją dwa sposoby obliczenia tych informacji agregowanych:

1. Za pomocą zapytania SQL możemy wydać dodatkowe zapytanie do bazy danych w celu obliczenia danych podsumowania dla określonej kategorii. SQL zawiera wiele funkcji agregujących wraz z klauzulą `GROUP BY`, aby określić dane, za pomocą których dane mają być podsumowywane. Następujące zapytanie SQL spowoduje przywrócenie wymaganych informacji:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Oczywiście nie chcesz wystawić tego zapytania bezpośrednio ze strony `SummaryDataInFooter.aspx`, ale zamiast tworzyć metodę w `ProductsTableAdapter` i `ProductsBLL`.
2. Oblicz te informacje w miarę ich dodawania do widoku GridView, jak zostało to omówione w temacie [formatowanie niestandardowe oparte](custom-formatting-based-upon-data-cs.md) na samouczku danych, program obsługi zdarzeń `RowDataBound` GridView jest uruchamiany raz dla każdego wiersza dodawanego do widoku GridView po jego powiązaniu z danymi. Tworząc procedurę obsługi zdarzeń dla tego zdarzenia, możemy zachować całkowitą sumę wartości, które chcemy agregować. Po powiązaniu ostatniego wiersza danych z elementem GridView jest to suma i informacje konieczne do obliczenia średniej.

Zwykle stosujemy drugie podejście w miarę zapisywania w bazie danych i nakładu pracy wymaganego do zaimplementowania funkcji podsumowujących w warstwie dostępu do danych i w warstwie logiki biznesowej, ale w obu przypadkach byłoby to wystarczające. W tym samouczku użyjemy drugiej opcji i śledzisz sumę uruchamiania przy użyciu programu obsługi zdarzeń `RowDataBound`.

Utwórz procedurę obsługi zdarzeń `RowDataBound` dla widoku GridView, wybierając widok GridView w projektancie, klikając ikonę pioruna z okno Właściwości, a następnie klikając dwukrotnie zdarzenie `RowDataBound`. Spowoduje to utworzenie nowego programu obsługi zdarzeń o nazwie `ProductsInCategory_RowDataBound` w klasie `SummaryDataInFooter.aspx`j powiązanej z kodem.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Aby zachować łączną sumę, należy zdefiniować zmienne poza zakresem programu obsługi zdarzeń. Utwórz następujące cztery zmienne na poziomie strony:

- `_totalUnitPrice`typu `decimal`
- `_totalNonNullUnitPriceCount`typu `int`
- `_totalUnitsInStock`typu `int`
- `_totalUnitsOnOrder`typu `int`

Następnie napisz kod, aby zwiększyć te trzy zmienne dla każdego wiersza danych napotkanego w obsłudze zdarzeń `RowDataBound`.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

Procedura obsługi zdarzeń `RowDataBound` rozpoczyna się od zagwarantowania, że będziemy omawiać element DataRow. Po jego ustanowieniu wystąpienie `Northwind.ProductsRow`, które zostało właśnie powiązane z obiektem `GridViewRow` w `e.Row`, jest przechowywane w zmiennej `product`. Następnie uruchamianie łącznej liczby zmiennych jest zwiększane przez odpowiednie wartości bieżącego produktu (przy założeniu, że nie zawierają one `NULL` wartość). Śledzimy zarówno aktualną `UnitPrice`, jak i liczbę rekordów, które nie`NULL` `UnitPrice`, ponieważ średnia cena jest ilorazem tych dwóch liczb.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Krok 4. Wyświetlanie danych podsumowania w stopce

W przypadku sumowania danych podsumowania ostatni krok to wyświetlenie go w wierszu stopki GridView. To zadanie można także wykonać programowo za pomocą procedury obsługi zdarzeń `RowDataBound`. Odwołaj się, że program obsługi zdarzeń `RowDataBound` wyzwolony dla *każdego* wiersza, który jest powiązany z elementem GridView, włącznie z wierszem stopki. W związku z tym możemy rozszerzyć procedurę obsługi zdarzeń, aby wyświetlić dane w wierszu stopki przy użyciu następującego kodu:

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Ponieważ wiersz stopki jest dodawany do widoku GridView po dodaniu wszystkich wierszy danych, można mieć pewność, że przez wszystko gotowe do wyświetlenia danych podsumowania w stopce zostanie ukończona suma uruchomionych obliczeń. Ostatnim krokiem jest ustawienie tych wartości w komórkach stopki.

Aby wyświetlić tekst w danej komórce stopki, użyj `e.Row.Cells[index].Text = value`, gdzie indeksowanie `Cells` zaczyna się od 0. Poniższy kod oblicza średnią cenę (łączną cenę podzieloną przez liczbę produktów) i wyświetla ją wraz z łączną liczbą jednostek w magazynie i jednostkami w odpowiedniej stopce w komórkach GridView.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Rysunek 13 przedstawia raport po dodaniu tego kodu. Zwróć uwagę, jak `ToString("c")` powoduje, że średnie informacje podsumowujące cenę mają być sformatowane jako waluta.

[![wiersz stopki GridView ma teraz kolor tła Reddish](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Ilustracja 13**. wiersz stopki GridView ma teraz kolor tła Reddish ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))

## <a name="summary"></a>Podsumowanie

Wyświetlanie danych podsumowujących jest typowym wymaganiem raportu, a formant GridView ułatwia uwzględnianie takich informacji w jego wierszu stopki. Wiersz stopki jest wyświetlany, gdy właściwość `ShowFooter` GridView jest ustawiona na `true` i można programowo ustawić tekst w jej komórkach za pomocą programu obsługi zdarzeń `RowDataBound`. Obliczanie danych podsumowujących można wykonać przez ponowne przetworzenie zapytania do bazy danych lub użycie kodu w klasie ASP.NET strony powiązanej z kodem, aby programowo obliczyć dane podsumowujące.

W tym samouczku zakończymy badanie niestandardowego formatowania za pomocą formantów GridView, DetailsView i FormView. Nasz następny samouczek rozpoczyna pracę nad wstawianiem, aktualizowaniem i usuwaniem danych przy użyciu tych samych kontrolek.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](using-the-formview-s-templates-cs.md)
> [dalej](custom-formatting-based-upon-data-vb.md)
