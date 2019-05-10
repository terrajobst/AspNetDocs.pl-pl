---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: Wyświetlanie informacji podsumowania w stopce kontrolki GridView (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Informacje podsumowujące często jest wyświetlany w dolnej części raportu, w wierszu podsumowania. W kontrolce GridView może zawierać wiersz stopki, do którego komórek możemy żądania ściągnięcia...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 685917250247e6a36952de29404146d85af3c46d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65114989"
---
# <a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Wyświetlanie informacji podsumowania w stopce kontrolki GridView (C#)

przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) lub [Pobierz plik PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Informacje podsumowujące często jest wyświetlany w dolnej części raportu, w wierszu podsumowania. W kontrolce GridView mogą zawierać wiersz stopki, do którego komórek możemy programowe Wstawianie danych agregacji. W tym samouczku opisano sposób wyświetlania zagregowanych danych w tym wierszu stopki.

## <a name="introduction"></a>Wprowadzenie

Oprócz możliwości wyświetlania każdej cen produktów, jednostek w magazynie, jednostek w kolejności i której kolejność chcesz zmienić poziomy, użytkownik może również zainteresować się agregacji informacje, takie jak średnia cena łączna liczba jednostek w magazynie i tak dalej. Takie informacje podsumowujące często jest wyświetlany w dolnej części raportu, w wierszu podsumowania. W kontrolce GridView mogą zawierać wiersz stopki, do którego komórek możemy programowe Wstawianie danych agregacji.

To zadanie przedstawia nam trzy wyzwania:

1. Konfigurowanie kontrolki GridView, aby wyświetlić jego wiersz stopki
2. Określanie podsumowania danych; oznacza to jak możemy obliczeniowych średnia cena lub łączna liczba jednostek w magazynie?
3. Wstawianie danych podsumowania do komórek wiersza stopki

W tym samouczku opisano sposób przezwyciężyć te wyzwania. W szczególności utworzymy strona, która wyświetla listę kategorii z listy rozwijanej z wybranej kategorii produktów, wyświetlany w kontrolce GridView. Kontrolki GridView będzie zawierać wiersz stopki, który przedstawia średnią cenę i całkowita liczba jednostek w magazynie i na produkty z tej kategorii.

[![Informacje podsumowania jest wyświetlany w wierszu stopce kontrolki GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Rysunek 1**: Informacje podsumowania jest wyświetlany w wierszu stopce kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))

Ten samouczek, za pomocą kategorii produktów wzorzec/szczegół interfejsu, opiera się na koncepcji omówione wcześniej [wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczka. Jeśli jeszcze nie znasz już samouczkiem wcześniej, zrób to przed kontynuowaniem korzystania z niej.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Krok 1. Dodawanie kontrolki DropDownList kategorie i produktów GridView

Przed dotyczących osoby z podsumowania informacje dodane w stopce kontrolki GridView, najpierw po prostu utworzymy raport wzorzec/szczegół. Po pierwszym kroku została ukończona, omówimy jak dołączyć dane podsumowujące.

Zacznij od otwarcia `SummaryDataInFooter.aspx` stronie `CustomFormatting` folderu. Dodaj formant DropDownList i ustaw jego `ID` do `Categories`. Następnie kliknij łącze Wybierz źródło danych z kontrolki DropDownList tagów inteligentnych i zoptymalizowany pod kątem, aby dodać nowe kontrolki ObjectDataSource, o nazwie `CategoriesDataSource` wywołującej `CategoriesBLL` klasy `GetCategories()` metody.

[![Dodawanie nowego elementu ObjectDataSource, o nazwie CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Rysunek 2**: Dodaj nazwę nowej kontrolki ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))

[![Masz ObjectDataSource, wywołaj metodę GetCategories() klasy CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Rysunek 3**: Masz wywołania elementu ObjectDataSource `CategoriesBLL` klasy `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))

Po skonfigurowaniu kontrolki ObjectDataSource, Kreator wyświetli nam konfiguracji źródła danych DropDownList kreatora, w którym trzeba określić wartości pola danych, które powinny być wyświetlane, i który z nich powinien odpowiadać wartości metody DropDownList firmy `ListItem` s. Masz `CategoryName` wyświetlanemu polu i użyj `CategoryID` jako wartość.

[![Użyj CategoryName i pola CategoryID jako tekst i wartość ListItems, odpowiednio](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Rysunek 4**: Użyj `CategoryName` i `CategoryID` pól jako `Text` i `Value` dla `ListItem` s, odpowiednio ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))

W tym momencie mamy kontrolki DropDownList (`Categories`), wyświetla listę kategorii w systemie. Teraz należy dodać w kontrolce GridView, który zawiera listę tych produktów, które należą do wybranej kategorii. Zanim przejdziemy, jednak przyjrzeć zaznacz pole wyboru włączenia automatycznego ogłaszania zwrotnego w DropDownList tagu inteligentnego. Zgodnie z opisem w *wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList* samouczków, ustawiając DropDownList `AutoPostBack` właściwość `true` strony zostanie opublikowany ponownie każdorazowo DropDownList wartość zostanie zmieniona. Spowoduje to GridView zostanie odświeżona, wyświetlanie tych produktów w nowo wybranej kategorii. Jeśli `AutoPostBack` właściwość jest ustawiona na `false` (ustawienie domyślne), zmieniając kategorii nie powoduje odświeżenie strony i w związku z tym nie będzie aktualizacji listy produktów.

[![Zaznacz pole wyboru AutoPostBack Włącz w tagu inteligentnego DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Rysunek 5**: Zaznacz pole wyboru Włącz automatycznego ogłaszania zwrotnego w tagu inteligentnego DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))

Dodawanie kontrolki widoku siatki do strony w celu wyświetlania produktów dla wybranej kategorii. Ustaw GridView `ID` do `ProductsInCategory` i powiązać ją z nowego elementu ObjectDataSource, o nazwie `ProductsInCategoryDataSource`.

[![Dodawanie nowego elementu ObjectDataSource, o nazwie ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Rysunek 6**: Dodaj nazwę nowej kontrolki ObjectDataSource `ProductsInCategoryDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))

Skonfigurować kontrolki ObjectDataSource wywołuje `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody.

[![Masz ObjectDataSource, wywołaj metodę GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Rysunek 7**: Masz wywołania elementu ObjectDataSource `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))

Ponieważ `GetProductsByCategoryID(categoryID)` metoda przyjmuje parametr wejściowy w ostatnim kroku kreatora można określić źródło wartości parametru. Aby wyświetlić te produkty z wybranej kategorii, ma parametr pobierane z `Categories` DropDownList.

[![Uzyskiwanie categoryID wartość parametru metody DropDownList wybrane kategorie](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Rysunek 8**: Pobierz *`categoryID`* wartość parametru metody DropDownList wybrane kategorie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))

Po zakończeniu pracy kreatora widoku GridView mają elementu BoundField dla każdej właściwości produktu. Umożliwia czyszczenie, aby tylko te BoundFields `ProductName`, `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` BoundFields są wyświetlane. Możesz dodać wszelkie ustawienia na poziomie pola do pozostałych BoundFields (takie jak formatowanie `UnitPrice` jako walutę). Po wprowadzeniu tych zmian, oznaczeniu deklaracyjnym GridView powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

W tym momencie mamy pełnej funkcjonalności raportu wzorzec/szczegół, który zawiera nazwę, cenę jednostkową, jednostek w magazynie i jednostek w kolejności dla tych produktów, które należą do wybranej kategorii.

[![Uzyskiwanie categoryID wartość parametru metody DropDownList wybrane kategorie](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Rysunek 9**: Pobierz *`categoryID`* wartość parametru metody DropDownList wybrane kategorie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))

## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Krok 2. Wyświetlanie stopki w widoku GridView

W kontrolce GridView można wyświetlić nagłówek i stopka wiersza. Te wiersze są wyświetlane w zależności od wartości `ShowHeader` i `ShowFooter` właściwości, za pomocą `ShowHeader` przyjęty `true` i `ShowFooter` do `false`. Aby dołączyć stopce kontrolki GridView po prostu ustaw jego `ShowFooter` właściwość `true`.

[![Wartość true właściwości ShowFooter GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Na rysunku nr 10**: Ustaw GridView `ShowFooter` właściwości `true` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))

Wiersz stopki ma komórki dla każdego z pól zdefiniowanych w kontrolce GridView; Jednak te komórki są domyślnie puste. Poświęć chwilę, aby wyświetlić postępach w przeglądarce. Za pomocą `ShowFooter` teraz właściwością `true`, widoku GridView zawiera stopkę pusty wiersz.

[![Teraz GridView zawiera wiersz stopki](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Rysunek 11**: Kontrolki GridView zawiera teraz wiersz stopki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))

Wiersz stopki rysunek 11 nie Wyróżnij, ponieważ zawiera ona białe tło. Utwórzmy `FooterStyle` klasy CSS w `Styles.css` , określa ciemnym tle czerwony, a następnie skonfiguruj `GridView.skin` plik Skin w `DataWebControls` motywu, aby przypisać tej klasy CSS do GridView `FooterStyle`firmy `CssClass` właściwości. Jeśli zachodzi potrzeba projektujesz skórek i motywów, odwołaj się do [wyświetlanie danych za pomocą kontrolki ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) samouczka.

Rozpocznij, dodając następujące klasy CSS do `Styles.css`:

[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

`FooterStyle` Klasy CSS jest podobna we styl `HeaderStyle` klasy, mimo że `HeaderStyle`firmy kolor tła subtlety ciemniejszego i jego tekstu zostaną wyświetlone pogrubioną czcionką. Co więcej tekst w stopce kontrolki jest wyrównany do prawej natomiast skupia się tekst nagłówka.

Następnie, aby skojarzyć ten klasę CSS z stopce kontrolki GridView, co, otwórz `GridView.skin` w pliku `DataWebControls` motywu i ustaw `FooterStyle`firmy `CssClass` właściwości. Po dodaniu tego kodu znaczników pliku powinno wyglądać:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Jak zrzucie ekranu poniżej przedstawiono, ta zmiana powoduje stopki wyraźnie więcej.

[![Kolor tła czerwonawego ma teraz wiersz stopce kontrolki GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Rysunek 12**: Wiersz stopki GridView ma teraz czerwonawego kolor tła ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))

## <a name="step-3-computing-the-summary-data"></a>Krok 3. Przetwarzanie danych podsumowania

Za pomocą stopce kontrolki GridView wyświetlane dalej wyzwanie, połączonego z nami przedstawiono sposób obliczenia dane podsumowujące. Istnieją dwa sposoby obliczeń tych agregacji informacji:

1. Za pomocą kwerendy SQL firma Microsoft może wydać dodatkowych kwerend w bazie danych do obliczenia dane podsumowania dla określonej kategorii. SQL obejmuje pewną liczbę funkcji agregujących, wraz z `GROUP BY` klauzuli, aby określić dane, w którym można podsumować dane. Następujące zapytanie SQL będzie przywrócić potrzebnych informacji:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Oczywiście nie byłoby dobrze wydać to zapytanie bezpośrednio z `SummaryDataInFooter.aspx` strony, ale raczej, tworząc metody w `ProductsTableAdapter` i `ProductsBLL`.
2. Obliczenia te informacje, ponieważ ona jest dodawany do kontrolki GridView zgodnie z opisem w [niestandardowe formatowanie oparte na danych](custom-formatting-based-upon-data-cs.md) samouczku, widoku GridView `RowDataBound` program obsługi zdarzeń jest uruchamiana raz dla każdego wiersza dodawane do kontrolki GridView po jego zostały z danymi. Tworząc program obsługi zdarzeń dla tego zdarzenia nam zapewnić działającego całkowitej wartości chcemy, aby agregować. Po ostatnim wierszu danych została powiązana z GridView mamy sum oraz informacjami potrzebnymi do obliczenia średniej.

I zazwyczaj używają drugiej metody, zapisuje podróż do bazy danych i pracy potrzeba do implementacji funkcji Podsumowanie warstwy dostępu do danych i warstwy logiki biznesowej, ale każda z tych metod może wystarczyć. W tym samouczku możemy użyć drugiej opcji i śledzenie Suma przy użyciu `RowDataBound` programu obsługi zdarzeń.

Tworzenie `RowDataBound` program obsługi zdarzeń dla widoku GridView wybranie kontrolki GridView w projektancie, klikając ikonę pioruna w oknie właściwości i dwukrotne kliknięcie `RowDataBound` zdarzeń. Spowoduje to utworzenie nowego programu obsługi zdarzeń o nazwie `ProductsInCategory_RowDataBound` w `SummaryDataInFooter.aspx` strony osobna klasa kodu.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Aby sumy należy zdefiniować zmienne poza zakresem obsługi zdarzeń. Utwórz cztery następujące zmienne na poziomie strony:

- `_totalUnitPrice`, typu `decimal`
- `_totalNonNullUnitPriceCount`, typu `int`
- `_totalUnitsInStock`, typu `int`
- `_totalUnitsOnOrder`, typu `int`

Następnie napisać kod, aby zwiększyć te trzy zmienne, dla każdego wiersza danych podczas `RowDataBound` programu obsługi zdarzeń.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

`RowDataBound` Program obsługi zdarzeń, który rozpoczyna się poprzez zapewnienie, że firma Microsoft jest zajmujących się DataRow. Gdy zostanie który zostało nawiązane, `Northwind.ProductsRow` wystąpienia, która właśnie została powiązana z `GridViewRow` obiektu `e.Row` jest przechowywana w zmiennej `product`. Następny, uruchomione łączna liczba zmiennych są zwiększane o bieżącego produktu odpowiadające im wartości (przy założeniu, że nie zawierają bazy danych `NULL` wartości). Firma Microsoft zachować informacje o jednocześnie działające `UnitPrice` łącznej liczby przypadków i liczba non -`NULL` `UnitPrice` rejestruje ponieważ średnia cena jest równy ilorazowi tych dwóch liczb.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Krok 4. Wyświetlanie danych podsumowania w stopce

Za pomocą dane podsumowania sumowane ostatnim krokiem jest do wyświetlenia go w wierszu stopce kontrolki GridView. To zadanie, można osiągnąć programowo za pośrednictwem `RowDataBound` programu obsługi zdarzeń. Pamiętamy `RowDataBound` obsługi zdarzenia generowane dla *co* wiersza, który jest powiązany z GridView, w tym wierszu stopki. W związku z tym możemy rozszerzyć nasze programu obsługi zdarzeń, aby wyświetlić dane w wierszu stopki, używając następującego kodu:

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Ponieważ wiersz stopki jest dodawana do kontrolki GridView po wszystkich wierszy danych zostały dodane, firma Microsoft może być pewność, że przez czas jesteśmy gotowi wyświetlić dane podsumowania w stopce, który ukończy Suma obliczeń. Ostatnim krokiem jest następnie ustaw te wartości w komórkach stopki.

Aby wyświetlić tekst w komórce stopki określonego, użyj `e.Row.Cells[index].Text = value`, gdzie `Cells` indeksowanie rozpoczyna się od 0. Poniższy kod oblicza średnią cenę (całkowita cena dzielona przez liczbę produktów) i wyświetla go wraz z całkowitą liczbą jednostek w magazynie i jednostki w kolejności w komórkach odpowiednie stopce kontrolki GridView.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Rysunek 13 zawiera raport, po dodaniu tego kodu. Uwaga jak `ToString("c")` powoduje, że średnia cena informacje podsumowujące do być sformatowane następująco walutę.

[![Kolor tła czerwonawego ma teraz wiersz stopce kontrolki GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Rysunek 13**: Wiersz stopki GridView ma teraz czerwonawego kolor tła ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))

## <a name="summary"></a>Podsumowanie

Wyświetlanie podsumowania danych jest typowym wymogiem raportów i kontrolki GridView ułatwia obejmują takie informacje w jego wierszu stopki. Wiersz stopka jest wyświetlany podczas GridView `ShowFooter` właściwość jest ustawiona na `true` i może zawierać tekstu w komórkach ustawić programowo za pomocą `RowDataBound` programu obsługi zdarzeń. Przetwarzanie danych podsumowania albo można przeprowadzić przez ponownie zapytanie do bazy danych lub przy użyciu kodu w klasie CodeBehind strony ASP.NET, można programowo obliczyć dane podsumowujące.

W tym samouczku zawiera niestandardowe formatowanie przy użyciu kontrolki GridView DetailsView i FormView nasze badania. Naszego następnego samouczka dotyczącego naszych badań Wstawianie, aktualizowanie i usuwanie danych przy użyciu tych tych samych kontrolek.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](using-the-formview-s-templates-cs.md)
> [dalej](custom-formatting-based-upon-data-vb.md)
