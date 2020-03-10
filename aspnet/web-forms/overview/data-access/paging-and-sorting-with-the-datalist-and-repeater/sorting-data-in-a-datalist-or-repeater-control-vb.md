---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: Sortowanie danych w kontrolce DataList lub Wzmacniake (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak uwzględnić obsługę sortowania w elemencie DataList i Wzmacniake, a także jak utworzyć element DataList lub wzmacniak, którego dane mogą...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 81e07bec8569b9ee987dfaa84dec9eec95a2692f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576287"
---
# <a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Sortowanie danych w kontrolce DataList lub Repeater (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) lub [Pobierz plik PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> W tym samouczku dowiesz się, jak uwzględnić obsługę sortowania w elemencie DataList i Wzmacniake, a także jak utworzyć element DataList lub wzmacniak, którego dane mogą być stronicowane i posortowane.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](paging-report-data-in-a-datalist-or-repeater-control-vb.md) sprawdziłmy, jak dodać obsługę stronicowania do elementu DataList. Utworzyliśmy nową metodę w klasie `ProductsBLL` (`GetProductsAsPagedDataSource`), która zwróciła obiekt `PagedDataSource`. Gdy jest to powiązane z elementem DataList lub wzmacniak, element DataList lub wzmacniak wyświetli tylko żądaną stronę danych. Ta technika jest podobna do tego, co jest używane wewnętrznie przez kontrolki GridView, DetailsView i FormView, aby zapewnić wbudowane funkcje stronicowania.

Oprócz oferowania obsługi stronicowania, widok GridView obejmuje również obsługę sortowania w polu. Żadna z elementów DataList i wzmacniak nie zapewnia wbudowanej funkcji sortowania; jednak sortowanie funkcji można dodać przy użyciu bitu kodu. W tym samouczku dowiesz się, jak uwzględnić obsługę sortowania w elemencie DataList i Wzmacniake, a także jak utworzyć element DataList lub wzmacniak, którego dane mogą być stronicowane i posortowane.

## <a name="a-review-of-sorting"></a>Przegląd sortowania

Jak opisano w samouczku dotyczącego [stronicowania i sortowania danych raportu](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , kontrolka GridView udostępnia pomoc techniczną do sortowania pól. Każde pole GridView może mieć skojarzone `SortExpression`, które wskazuje pole danych, według którego mają być sortowane dane. Gdy właściwość GridView `AllowSorting` jest ustawiona na `true`, każde pole GridView, które ma `SortExpression` wartość właściwości, ma swój nagłówek renderowany jako element LinkButton. Gdy użytkownik kliknie konkretny nagłówek pola GridView, następuje odświeżenie, a dane są sortowane według klikniętego pola s `SortExpression`.

Formant GridView ma również właściwość `SortExpression`, która przechowuje `SortExpression` pola GridView, według którego są sortowane dane. Ponadto Właściwość `SortDirection` wskazuje, czy dane mają być sortowane w kolejności rosnącej czy malejącej (Jeśli użytkownik kliknie dwa razy łącze nagłówka pola GridView), kolejność sortowania jest przełączana.

Gdy widok GridView jest powiązany z jego kontrolą źródła danych, `SortExpression` i `SortDirection` właściwości do kontroli źródła danych. Formant źródła danych pobiera dane, a następnie sortuje je według podanych `SortExpression` i `SortDirection` właściwości. Po posortowaniu danych formant źródła danych zwraca go do widoku GridView.

Aby replikować tę funkcję za pomocą kontrolek DataList lub wzmacniak, musimy:

- Tworzenie interfejsu sortowania
- Zapamiętaj pole danych, według którego ma zostać wykonane sortowanie, i czy ma być sortowane w kolejności rosnącej czy malejącej
- Poinstruuj element ObjectDataSource, aby posortować dane według określonego pola danych

Te trzy zadania zostaną przedstawione w krokach 3 i 4. Poniżej opisano, jak uwzględnić obsługę stronicowania i sortowania w elemencie DataList lub Wzmacniake.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Krok 2. Wyświetlanie produktów w Wzmacniake

Przed zainstalowaniem dowolnych funkcji związanych z sortowaniem Zacznij od poinformowania o produktach w kontrolce wzmacniania. Zacznij od otwarcia strony `Sorting.aspx` w folderze `PagingSortingDataListRepeater`. Dodaj kontrolkę wzmacniak do strony sieci Web, ustawiając jej Właściwość `ID` na `SortableProducts`. W tagu inteligentnym wzmacniak Utwórz nowy element ObjectDataSource o nazwie `ProductsDataSource` i skonfiguruj go tak, aby pobierał dane z metody `GetProducts()` klasy `ProductsBLL`. Wybierz opcję (brak) z listy rozwijanej na kartach Wstawianie, aktualizowanie i usuwanie.

[![utworzyć elementu ObjectDataSource i skonfigurować go tak, aby korzystał z metody GetProductsAsPagedDataSource ()](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Rysunek 1**. Utwórz element ObjectDataSource i skonfiguruj go tak, aby korzystał z metody `GetProductsAsPagedDataSource()` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))

[![ustawić listy rozwijane na kartach Aktualizuj, Wstaw i Usuń do (brak).](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Rysunek 2**. Ustawianie list rozwijanych w kartach Aktualizuj, Wstaw i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))

W przeciwieństwie do obiektu DataList, program Visual Studio nie tworzy automatycznie `ItemTemplate` dla formantu wzmacniak po powiązaniu go ze źródłem danych. Ponadto należy dodać tę `ItemTemplate` deklaratywnie, ponieważ tag inteligentny kontrolka nie ma opcji Edytuj szablony znalezionej w elemencie DataList s. Niech s użyje tego samego `ItemTemplate` z poprzedniego samouczka, w którym wyświetlana jest nazwa produktu, dostawca i Kategoria.

Po dodaniu `ItemTemplate`u znaczniki deklaratywne i ObjectDataSource s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

Rysunek 3 przedstawia Tę stronę, gdy jest wyświetlany za pomocą przeglądarki.

[![zostanie wyświetlona nazwa każdego produktu, dostawcy i kategorii](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Rysunek 3**. zostanie wyświetlona nazwa każdego produktu, dostawcy i kategorii ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Krok 3. nakazuje elementowi ObjectDataSource sortowanie danych

Aby posortować dane wyświetlane w Wzmacniake, musimy poinformować element ObjectDataSource wyrażenia Sort, według którego mają być sortowane dane. Przed pobraniem danych przez element ObjectDataSource zostanie najpierw wyzwolone jego [zdarzenie`Selecting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), które pozwala nam określić wyrażenie sortowania. Procedura obsługi zdarzeń `Selecting` jest przenoszona do obiektu typu [`ObjectDataSourceSelectingEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), który ma właściwość o nazwie [`Arguments`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). Klasa `DataSourceSelectArguments` została zaprojektowana w celu przekazywania żądań związanych z danymi od konsumenta danych do kontroli źródła danych i zawiera [właściwość`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Aby przekazać informacje o sortowaniu ze strony ASP.NET do elementu ObjectDataSource, Utwórz procedurę obsługi zdarzeń dla zdarzenia `Selecting` i użyj następującego kodu:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

Wartość *'sortexpression* powinna mieć przypisaną nazwę pola danych, według którego będą sortowane dane (na przykład ProductName). Nie ma żadnej właściwości związanej z kierunkiem sortowania, dlatego jeśli chcesz sortować dane w kolejności malejącej, Dołącz wartość DESC ciągu do wartości *'sortexpression* (np. ProductName).

Spróbuj wykonać inne zakodowane wartości *'sortexpression* i przetestuj wyniki w przeglądarce. Jak pokazano na rysunku 4, jeśli jako *'sortexpression*jest używany Opis programu ProductName, produkty są sortowane według nazwy w odwrotnej kolejności alfabetycznej.

[![produkty są sortowane według nazwy w odwrotnej kolejności alfabetycznej](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Ilustracja 4**. produkty są sortowane według nazwy w odwrotnej kolejności alfabetycznej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Krok 4. Tworzenie interfejsu sortowania i zapamiętywanie wyrażenia sortowania i kierunku

Włączenie obsługi sortowania w widoku GridView spowoduje przekonwertowanie każdego tekstu nagłówka pola sortowania w element LinkButton, który po kliknięciu sortuje odpowiednio dane. Taki interfejs sortowania ma sens dla widoku GridView, gdzie jego dane są starannie ułożone w kolumnach. Jednak dla kontrolek DataList i wzmacniak jest wymagany inny interfejs sortowania. Typowy interfejs sortowania dla listy danych (w przeciwieństwie do siatki danych) jest listą rozwijaną, która dostarcza pola, według których dane mogą być sortowane. Zezwól na wdrożenie takiego interfejsu dla tego samouczka.

Dodaj kontrolkę sieci Web DropDownList powyżej wzmacniaka `SortableProducts` i ustaw jej Właściwość `ID` na `SortBy`. Na okno Właściwości kliknij wielokropek we właściwości `Items`, aby wyświetlić Edytor kolekcji ListItem. Dodaj `ListItem` s, aby posortować dane według pól `ProductName`, `CategoryName`i `SupplierName`. Dodaj również `ListItem`, aby posortować produkty według ich nazwy w odwrotnej kolejności alfabetycznej.

Właściwości `ListItem` `Text` można ustawić na dowolną wartość (taką jak nazwa), ale właściwości `Value` muszą być ustawione na nazwę pola danych (na przykład ProductName). Aby posortować wyniki w kolejności malejącej, Dołącz wartość DESC ciągu do nazwy pola danych, na przykład ProductName DESC.

![Dodaj element ListItem dla każdego z pól danych do sortowania](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Rysunek 5**. Dodawanie `ListItem` dla każdego z pól danych do sortowania

Na koniec Dodaj kontrolkę sieci Web przycisk po prawej stronie DropDownList. Ustaw jej `ID` na `RefreshRepeater` i jej Właściwość `Text` do odświeżenia.

Po utworzeniu `ListItem` s i dodaniem przycisku Odśwież składni deklaracyjnej DropDownList i Button powinna wyglądać podobnie do poniższego:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Po ukończeniu sortowania DropDownList należy najpierw zaktualizować procedurę obsługi zdarzeń elementu ObjectDataSource s `Selecting`, tak aby używała wybranej właściwości `SortBy``ListItem` s `Value` w przeciwieństwie do zakodowanego wyrażenia sortowania.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

W tym momencie podczas pierwszego odwiedzania strony produkty będą początkowo sortowane według pola danych `ProductName`, ponieważ `SortBy` `ListItem` wybrane domyślnie (patrz rysunek 6). Wybranie innej opcji sortowania, takiej jak kategoria i kliknięcie przycisku Odśwież spowoduje wystąpienie zwrotne i ponowne sortowanie danych według nazwy kategorii, jak pokazano na rysunku 7.

[![produkty są początkowo sortowane według ich nazwy](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Ilustracja 6**. produkty są początkowo sortowane według ich nazwy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))

[![produkty są teraz sortowane według kategorii](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Rysunek 7**. produkty są teraz sortowane według kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))

> [!NOTE]
> Kliknięcie przycisku Odśwież powoduje, że dane zostaną automatycznie posortowane, ponieważ stan widoku wzmacniak został wyłączony, co powoduje, że wzmacniak ponownie powiąże się ze źródłem danych na każdym odświeżeniu. Jeśli pozostawiono stan widoku wzmacniak s włączony, zmiana listy rozwijanej sortowania nie będzie miała wpływu na kolejność sortowania. Aby rozwiązać ten wyjątek, należy utworzyć procedurę obsługi zdarzeń dla zdarzenia `Click` przycisku Odśwież i ponownie powiązać wzmacniak z jego źródłem danych (wywołując metodę `DataBind()`a.

## <a name="remembering-the-sort-expression-and-direction"></a>Zapamiętywanie wyrażenia sortowania i kierunku

Podczas tworzenia elementu DataList lub wzmacniak do sortowania na stronie, w której mogą wystąpić odświeżenia odnoszące się do sortowania, należy pamiętać, że wyrażenie sortowania i kierunek są zapamiętywane na stronach ogłaszania zwrotnego. Załóżmy na przykład, że zaktualizowaliśmy wzmacniak w tym samouczku, aby dodać przycisk Usuń dla każdego produktu. Gdy użytkownik kliknie przycisk Usuń, należy uruchomić kod, aby usunąć wybrany produkt, a następnie ponownie powiązać dane z Wzmacniakem. Jeśli szczegóły sortowania nie są utrwalane w odniesieniu do ogłaszania zwrotnego, dane wyświetlane na ekranie zostaną przywrócone do oryginalnej kolejności sortowania.

W tym samouczku DropDownList niejawnie zapisuje wyrażenie sortowania i kierunek w stanie widoku dla nas. Jeśli korzystamy z innego interfejsu sortowania o jeden z, powiedzmy, LinkButtons, który dostarczył różne opcje sortowania, mamy zadbać o zapamiętanie kolejności sortowania na stronach ogłaszania zwrotnego. Można to osiągnąć przez przechowywanie parametrów sortowania w stanie widoku strony, przez uwzględnienie parametru Sort w ciągu QueryString lub innej metody trwałości stanu.

W przyszłości przykłady w tym samouczku dowiesz się, jak utrwalać szczegóły sortowania w stanie widoku strony.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Krok 5. Dodawanie obsługi sortowania do elementu DataList używającego domyślnego stronicowania

W [poprzednim samouczku](paging-report-data-in-a-datalist-or-repeater-control-vb.md) sprawdziłmy, jak zaimplementować domyślne stronicowanie za pomocą elementu DataList. Pozwól s zwiększyć ten poprzedni przykład, aby uwzględnić możliwość sortowania danych stronicowanych. Zacznij od otwarcia stron `SortingWithDefaultPaging.aspx` i `Paging.aspx` w folderze `PagingSortingDataListRepeater`. Na stronie `Paging.aspx` kliknij przycisk Source (Źródło), aby wyświetlić znaczniki deklaratywne strony. Skopiuj zaznaczony tekst (patrz rysunek 8) i wklej go do deklaratywnego znacznika `SortingWithDefaultPaging.aspx` między tagami `<asp:Content>`.

[![przeprowadzić replikację deklaratywnego znacznika w &lt;ASP: Content&gt; tagów z pliku stronicowania. aspx do SortingWithDefaultPaging. aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Ilustracja 8**. replikowanie znaczników deklaratywnych w `<asp:Content>` tagów z `Paging.aspx` do `SortingWithDefaultPaging.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))

Po skopiowaniu oznakowania deklaracyjnego Skopiuj metody i właściwości w klasie `Paging.aspx`ej z kodem związanym ze stroną do klasy związanej z kodem dla `SortingWithDefaultPaging.aspx`. Następnie Poświęć chwilę na wyświetlenie strony `SortingWithDefaultPaging.aspx` w przeglądarce. Powinien on mieć taką samą funkcjonalność i wygląd jak `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Rozszerzanie ProductsBLL w celu uwzględnienia domyślnej metody stronicowania i sortowania

W poprzednim samouczku utworzyliśmy metodę `GetProductsAsPagedDataSource(pageIndex, pageSize)` w klasie `ProductsBLL`, która zwróciła obiekt `PagedDataSource`. Ten obiekt `PagedDataSource` został wypełniony *wszystkimi* produktami (za pośrednictwem metody `GetProducts()` logiki biznesowej s), ale w przypadku powiązana z kolumną DataList są wyświetlane tylko te rekordy, które odpowiadają określonym parametrom wejściowym *pageIndex* i *PageSize* .

Wcześniej w tym samouczku dodaliśmy obsługę sortowania, określając wyrażenie sortowania z programu obsługi zdarzeń `Selecting` ObjectDataSource s. Jest to dobre rozwiązanie, gdy element ObjectDataSource zwraca obiekt, który może być posortowany, jak `ProductsDataTable` zwracany przez metodę `GetProducts()`. Jednak obiekt `PagedDataSource` zwrócony przez metodę `GetProductsAsPagedDataSource` nie obsługuje sortowania jego wewnętrznego źródła danych. Zamiast tego należy posortować wyniki zwrócone przez metodę `GetProducts()` *przed* umieszczeniem jej w `PagedDataSource`.

Aby to osiągnąć, Utwórz nową metodę w klasie `ProductsBLL`, `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Aby posortować `ProductsDataTable` zwrócone przez metodę `GetProducts()`, należy określić właściwość `Sort` jej domyślnego `DataTableView`:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Metoda `GetProductsSortedAsPagedDataSource` różni się nieco od metody `GetProductsAsPagedDataSource` utworzonej w poprzednim samouczku. W szczególności `GetProductsSortedAsPagedDataSource` akceptuje dodatkowy parametr wejściowy `sortExpression` i przypisuje tę wartość do właściwości `Sort` `ProductDataTable` s `DefaultView`. Kilka wierszy kodu później, `DefaultView``ProductDataTable` s jest przypisany do `PagedDataSource` obiektu DataSource.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Wywoływanie metody GetProductsSortedAsPagedDataSource i określanie wartości parametru wejściowego 'Sortexpression

Po ukończeniu metody `GetProductsSortedAsPagedDataSource` następnym krokiem jest podanie wartości tego parametru. Element ObjectDataSource w `SortingWithDefaultPaging.aspx` jest obecnie skonfigurowany do wywoływania metody `GetProductsAsPagedDataSource` i przekazuje dwa parametry wejściowe za pomocą dwóch `QueryStringParameters`, które są określone w kolekcji `SelectParameters`. Te dwa `QueryStringParameters` wskazują, że źródło dla `GetProductsAsPagedDataSource` metod *popageindex* i *PageSize* pochodzi z pól QueryString `pageIndex` i `pageSize`.

Zaktualizuj Właściwość `SelectMethod` ObjectDataSource s tak, aby wywołuje nową metodę `GetProductsSortedAsPagedDataSource`. Następnie Dodaj nowe `QueryStringParameter` tak, aby parametr wejściowy *'sortexpression* był dostępny z poziomu pola QueryString `sortExpression`. Ustaw `DefaultValue` `QueryStringParameter` s na ProductName.

Po wprowadzeniu tych zmian znaczniki deklaratywne ObjectDataSource powinny wyglądać następująco:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

W tym momencie strona `SortingWithDefaultPaging.aspx` sortuje wyniki alfabetycznie według nazwy produktu (zobacz rysunek 9). Jest to spowodowane tym, że wartość ProductName jest domyślnie przenoszona jako parametr `GetProductsSortedAsPagedDataSource` *'sortexpression* metody s.

[Domyślnie ![wyniki są sortowane według NazwaProduktu](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Rysunek 9**: Domyślnie wyniki są sortowane według `ProductName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))

Jeśli ręcznie dodasz `sortExpression` QueryString pola, takie jak `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` wyniki zostaną posortowane według określonego `sortExpression`. Jednak ten `sortExpression` parametr nie jest uwzględniony w ciągu QueryString podczas przechodzenia do innej strony danych. W rzeczywistości klikaj przyciski Dalej lub Ostatnia strona Wróć do `Paging.aspx`! Ponadto nie ma obecnie interfejsu sortowania. Jedyny sposób, w jaki użytkownik może zmienić kolejność sortowania danych stronicowanych jest przez bezpośrednie manipulowanie QueryString.

## <a name="creating-the-sorting-interface"></a>Tworzenie interfejsu sortowania

Najpierw należy zaktualizować metodę `RedirectUser`, aby wysłać użytkownika do `SortingWithDefaultPaging.aspx` (zamiast `Paging.aspx`) i dołączyć wartość `sortExpression` do ciągu QueryString. Należy również dodać właściwość `SortExpression` tylko do odczytu na poziomie strony. Ta właściwość, podobna do właściwości `PageIndex` i `PageSize` utworzonych w poprzednim samouczku, zwraca wartość pola `sortExpression` QueryString, jeśli istnieje, a wartość domyślna (ProductName) w przeciwnym razie.

Obecnie Metoda `RedirectUser` akceptuje tylko jeden parametr wejściowy indeksu strony do wyświetlenia. Mogą jednak wystąpić sytuacje, w których chcemy przekierować użytkownika do określonej strony danych przy użyciu wyrażenia sortowania innego niż określone w ciągu kwerendy. W tej chwili utworzymy interfejs sortowania dla tej strony, który będzie zawierać serię kontrolek sieci Web przycisków do sortowania danych według określonej kolumny. Po kliknięciu jednego z tych przycisków chcemy przekierować użytkownika w celu przekierowania w odpowiednie wartości wyrażenia sortowania. Aby zapewnić tę funkcję, należy utworzyć dwie wersje metody `RedirectUser`. Pierwsza z nich powinna akceptować tylko indeks strony do wyświetlenia, podczas gdy druga akceptuje indeks strony i wyrażenie sortowania.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

W pierwszym przykładzie w tym samouczku utworzyliśmy interfejs sortowania przy użyciu DropDownList. Na potrzeby tego przykładu należy użyć trzech kontrolek sieci Web, umieszczonych powyżej elementu DataList, do sortowania według `ProductName`, jeden dla `CategoryName`i jeden dla `SupplierName`. Dodaj kontrolki sieci Web z trzema przyciskami, ustawiając odpowiednio `ID` i właściwości `Text`:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń `Click` dla każdej z nich. Procedury obsługi zdarzeń powinny wywołać metodę `RedirectUser`, zwracając użytkownika do pierwszej strony przy użyciu odpowiedniego wyrażenia sortowania.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Podczas pierwszego odwiedzania strony dane są sortowane według nazwy produktu alfabetycznie (zapoznaj się z powrotem do rysunku 9). Kliknij przycisk Dalej, aby przejść do drugiej strony danych, a następnie kliknij przycisk Sortuj według kategorii. Spowoduje to zwrócenie do pierwszej strony danych posortowanej według nazwy kategorii (patrz rysunek 10). Podobnie klikając przycisk Sortuj według dostawcy sortuje dane według dostawcy, rozpoczynając od pierwszej strony danych. Wybór sortowania jest zapamiętany, ponieważ dane są stronicowane. Rysunek 11 przedstawia stronę po sortowaniu według kategorii, a następnie przejściu do trzynastej strony danych.

[![produkty są sortowane według kategorii](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Ilustracja 10**. produkty są sortowane według kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))

[![wyrażenie sortowania jest zapamiętywane podczas stronicowania danych](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Ilustracja 11**. wyrażenie sortowania jest zapamiętywane podczas stronicowania danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))

## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Krok 6. niestandardowe stronicowanie za pomocą rekordów w Wzmacniake

Przykład elementu DataList zbadany w kroku 5 stron za pośrednictwem jego danych przy użyciu niewydajnej domyślnej techniki stronicowania. Podczas stronicowania przy użyciu wystarczająco dużych ilości danych jest konieczne użycie stronicowania niestandardowego. Do [wydajnego stronicowania za pomocą dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) i [sortowania niestandardowych](../paging-and-sorting/sorting-custom-paged-data-vb.md) samouczków danych z stronicowania zbadamy różnice między domyślnym i niestandardowym stronicowaniem i UTWORZONYMI metodami w logiki biznesowej na potrzeby wykorzystywania niestandardowego stronicowania i sortowania niestandardowych danych stron. W szczególności w tych dwóch wcześniejszych samouczkach dodaliśmy następujące trzy metody do klasy `ProductsBLL`:

- `GetProductsPaged(startRowIndex, maximumRows)` zwraca określony podzbiór rekordów, zaczynając od *StartRowIndex* i nie przekracza *MaximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` zwraca określony podzbiór rekordów posortowanych według określonego parametru wejściowego *'sortexpression* .
- `TotalNumberOfProducts()` zawiera łączną liczbę rekordów w tabeli `Products` Database.

Te metody mogą służyć do wydajnej strony i sortowania danych przy użyciu elementu DataList lub formantu wzmacniak. Aby to zilustrować, Zacznij od utworzenia kontrolki wzmacniak z obsługą niestandardowego stronicowania. następnie dodamy funkcje sortowania.

Otwórz stronę `SortingWithCustomPaging.aspx` w folderze `PagingSortingDataListRepeater` i Dodaj wzmacniak do strony, ustawiając jej Właściwość `ID` na `Products`. W tagu inteligentnym wzmacniak Utwórz nowy element ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj ją, aby wybrać jej dane z metody `GetProductsPaged` `ProductsBLL` Class.

[![skonfigurować element ObjectDataSource do używania metody GetProductsPaged klasy ProductsBLL](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Rysunek 12**. Konfigurowanie elementu ObjectDataSource do używania metody `GetProductsPaged` `ProductsBLL` klasy s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))

Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak), a następnie kliknij przycisk Dalej. Kreator konfiguracji źródła danych wyświetla teraz monit dotyczący źródeł `GetProductsPaged` metod *StartRowIndex* i *MaximumRows* Input Parameters. W rzeczywistości te parametry wejściowe są ignorowane. Zamiast tego wartości *StartRowIndex* i *MaximumRows* będą przesyłane za pomocą właściwości `Arguments` w programie obsługi zdarzeń `Selecting` ObjectDataSource s, podobnie jak w przypadku określenia *'sortexpression* w tym samouczku pierwszej demonstracyjnej. W związku z tym pozostaw listę rozwijaną źródła parametrów w Kreatorze ustawionym na brak.

[![pozostawić źródła parametrów ustawione na wartość None](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Ilustracja 13**. pozostawienie źródeł parametrów ustawionymi na None ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))

> [!NOTE]
> *Nie* ustawiaj właściwości `EnablePaging` ObjectDataSource s na `true`. Spowoduje to, że element ObjectDataSource automatycznie uwzględni własne parametry *StartRowIndex* i *MaximumRows* do listy istniejących parametrów `SelectMethod` s. Właściwość `EnablePaging` jest przydatna podczas wiązania niestandardowych danych stronicowanych do kontrolki GridView, DetailsView lub FormView, ponieważ te kontrolki oczekują określonego zachowania z elementu ObjectDataSource, który jest dostępny tylko wtedy, gdy właściwość `EnablePaging` jest `true`. Ponieważ trzeba ręcznie dodać obsługę stronicowania dla elementu DataList i wzmacniak, pozostaw tę właściwość ustawioną na `false` (wartość domyślna), ponieważ będziemy tworzenie w wymaganych funkcjach bezpośrednio na naszej stronie ASP.NET.

Na koniec Zdefiniuj `ItemTemplate`ka, aby wyświetlić nazwę produktu, kategorię i dostawcę. Po wprowadzeniu tych zmian, składnia deklaracyjne i ObjectDataSource s powinna wyglądać podobnie do poniższego:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Poświęć chwilę na odwiedzenie strony za pomocą przeglądarki i Zauważ, że żadne rekordy nie są zwracane. Jest to spowodowane tym, że już teraz podasz wartości parametrów *StartRowIndex* i *MaximumRows* ; w związku z tym wartości 0 są przesyłane w obu. Aby określić te wartości, należy utworzyć procedurę obsługi zdarzeń dla zdarzenia ObjectDataSource s `Selecting` i ustawić te wartości parametrów programowo na odpowiednio kodowane wartości 0 i 5:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Po tej zmianie Strona, która jest wyświetlana w przeglądarce, pokazuje pierwsze pięć produktów.

[![wyświetlane są pięć pierwszych rekordów](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Ilustracja 14**. wyświetlane są pięć pierwszych rekordów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))

> [!NOTE]
> Produkty wymienione na rysunku 14 mają być sortowane według nazwy produktu, ponieważ procedura składowana `GetProductsPaged`, która wykonuje wydajne niestandardowe zapytanie stronicowania, porządkuje wyniki według `ProductName`.

Aby umożliwić użytkownikowi przechodzenie między stronami, musimy śledzić indeks wierszy początkowych i maksymalną liczbę wierszy oraz zapamiętać te wartości na stronach ogłaszania zwrotnego. W domyślnym przykładzie stronicowania użyto pól QueryString, aby zachować te wartości. na potrzeby tego pokazu poinformuj te informacje w stanie widoku strony. Utwórz następujące dwie właściwości:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

Następnie zaktualizuj kod w programie obsługi zdarzeń Selecting, tak aby używał właściwości `StartRowIndex` i `MaximumRows` zamiast zakodowanych wartości 0 i 5:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

W tym momencie nasza strona nadal zawiera tylko pięć pierwszych rekordów. Jednak przy użyciu tych właściwości będziemy gotowi się do utworzenia naszego interfejsu stronicowania.

## <a name="adding-the-paging-interface"></a>Dodawanie interfejsu stronicowania

Zezwól na użycie tego samego pierwszego, poprzedniego, następnego, ostatniego interfejsu stronicowania używanego w domyślnym przykładzie stronicowania, w tym kontrolki sieci Web etykieta, która wyświetla informacje o przeglądanej stronie i liczbie istniejących stron. Dodaj cztery przyciski kontrolek sieci Web i etykietę poniżej wzmacniania.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

Następnie utwórz obsługę zdarzeń `Click` dla czterech przycisków. Po kliknięciu jednego z tych przycisków musimy zaktualizować `StartRowIndex` i ponownie powiązać dane z Wzmacniakem. Kod dla pierwszych, poprzednich i następnych przycisków jest mały, ale dla ostatniego przycisku jak określamy indeks początku wiersza dla ostatniej strony danych? Aby obliczyć ten indeks, a także można określić, czy następny i ostatni przycisk powinien być włączony, musimy wiedzieć, ile rekordów w sumie jest stronicowanych. Możemy to określić, wywołując metodę `ProductsBLL` klasy s `TotalNumberOfProducts()`. Niech s utworzy właściwość tylko do odczytu, na poziomie strony o nazwie `TotalRowCount`, która zwraca wyniki metody `TotalNumberOfProducts()`:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

Za pomocą tej właściwości możemy teraz określić indeks ostatniego wiersza początku strony. W związku z tym liczba całkowita wyniku `TotalRowCount` minus 1 podzielona przez `MaximumRows`, pomnożona przez `MaximumRows`. Teraz można napisać obsługę zdarzeń `Click` dla czterech przycisków interfejsu stronicowania:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Na koniec musimy wyłączyć pierwsze i poprzednie przyciski w interfejsie stronicowania podczas wyświetlania pierwszej strony danych i następnych i ostatnich przycisków podczas wyświetlania ostatniej strony. Aby to osiągnąć, Dodaj następujący kod do programu obsługi zdarzeń `Selecting` ObjectDataSource s:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Po dodaniu tych `Click` obsługi zdarzeń i kodzie do włączania lub wyłączania elementów interfejsu stronicowania na podstawie bieżącego indeksu wierszy początkowych Przetestuj stronę w przeglądarce. Jak pokazano na rysunku 15, podczas pierwszego odwiedzania strony pierwszy i Poprzedni przycisk zostaną wyłączone. Kliknięcie przycisku Dalej powoduje wyświetlenie drugiej strony danych, a następnie kliknięcie pozycji Ostatnia wyświetla końcową stronę (zobacz ilustracje 16 i 17). Podczas wyświetlania ostatniej strony danych, zarówno następny, jak i ostatni przycisk są wyłączone.

[![poprzednie i ostatnie przyciski są wyłączone podczas wyświetlania pierwszej strony produktów](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Ilustracja 15**. przyciski poprzednie i ostatnie są wyłączone podczas wyświetlania pierwszej strony produktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))

[![zostanie wyświetlona druga strona produktów](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Ilustracja 16**. zostanie wyświetlona druga strona produktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))

[![kliknięciu pozycji ostatni wyświetla końcową stronę danych](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Ilustracja 17**. kliknięcie pozycji Ostatnia wyświetla końcową stronę danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))

## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Krok 7. dołączenie obsługi sortowania z niestandardowym Wzmacniakem stronicowanym

Po zaimplementowaniu niestandardowego stronicowania jest ono gotowe do uwzględnienia obsługi sortowania. Metoda `ProductsBLL` klasy s `GetProductsPagedAndSorted` ma takie same parametry wejściowe *StartRowIndex* i *MaximumRows* jak `GetProductsPaged`, ale zezwala na dodatkowy parametr wejściowy *'sortexpression* . Aby użyć metody `GetProductsPagedAndSorted` z `SortingWithCustomPaging.aspx`, musimy wykonać następujące czynności:

1. Zmień właściwość "ObjectDataSource s `SelectMethod` z `GetProductsPaged` na `GetProductsPagedAndSorted`.
2. Dodaj obiekt *'sortexpression* `Parameter` do kolekcji ObjectDataSource s `SelectParameters`.
3. Utwórz prywatną Właściwość `SortExpression` na poziomie strony, która będzie utrzymywać swoją wartość na stronach ogłaszania zwrotnego przez stan widoku strony.
4. Zaktualizuj procedurę obsługi zdarzeń `Selecting` ObjectDataSource s, aby przypisać parametr ObjectDataSource s *'sortexpression* wartość właściwości `SortExpression` na poziomie strony.
5. Utwórz interfejs sortowania.

Zacznij od zaktualizowania właściwości `SelectMethod` ObjectDataSource s i dodania `Parameter` *'sortexpression* . Upewnij się, że właściwość *'sortexpression* `Parameter` s `Type` jest ustawiona na `String`. Po wykonaniu tych pierwszych dwóch zadań znacznik deklaratywny programu ObjectDataSource powinien wyglądać następująco:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

Następnie potrzebujemy właściwości `SortExpression` na poziomie strony, której wartość jest serializowana do stanu widoku. Jeśli nie ustawiono żadnej wartości wyrażenia sortowania, użyj opcji ProductName jako domyślnego:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Zanim element ObjectDataSource wywoła metodę `GetProductsPagedAndSorted`, musimy ustawić `Parameter` *'sortexpression* na wartość właściwości `SortExpression`. W obsłudze zdarzeń `Selecting` Dodaj następujący wiersz kodu:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

To wszystko, co ma na celu zaimplementowanie interfejsu sortowania. Podobnie jak w ostatnim przykładzie, niech s zaimplementowano interfejs sortowania przy użyciu trzech kontrolek sieci Web, które umożliwiają użytkownikowi sortowanie wyników według nazwy produktu, kategorii lub dostawcy.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Utwórz obsługę zdarzeń `Click` dla tych trzech kontrolek przycisków. W obsłudze zdarzeń Zresetuj `StartRowIndex` do 0, ustaw `SortExpression` na odpowiednią wartość i ponownie Powiąż dane z Wzmacniaką:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

Wszystko, co wszystko jest gotowe! Chociaż wprowadzono kilka kroków, aby uzyskać niestandardowe stronicowanie i zaimplementowane sortowanie, kroki były bardzo podobne do tych, które są wymagane do domyślnego stronicowania. Ilustracja 18 pokazuje produkty podczas wyświetlania ostatniej strony danych w posortowaniu według kategorii.

[zostanie wyświetlona ![ostatniej stronie danych posortowanej według kategorii.](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Ilustracja 18**. wyświetlana jest Ostatnia strona danych posortowana według kategorii (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))

> [!NOTE]
> W poprzednich przykładach podczas sortowania według dostawcy dostawcy użyto jako wyrażenia sortowania. Jednak w przypadku implementacji stronicowania niestandardowego musimy użyć elementu NazwaFirmy. Wynika to z faktu, że procedura składowana odpowiedzialna za implementowanie niestandardowego stronicowania `GetProductsPagedAndSorted` przekazuje wyrażenie sortowania do słowa kluczowego `ROW_NUMBER()`, słowo kluczowe `ROW_NUMBER()` wymaga rzeczywistej nazwy kolumny, a nie aliasu. W związku z tym należy użyć `CompanyName` (nazwę kolumny w tabeli `Suppliers`), a nie aliasu użytego w kwerendzie `SELECT` (`SupplierName`) dla wyrażenia Sort.

## <a name="summary"></a>Podsumowanie

Ani element DataList, ani wzmacniak nie oferują wbudowanej pomocy w zakresie sortowania, ale z niestandardowym interfejsem sortowania, można dodać tę funkcję. Podczas implementowania sortowania, ale nie stronicowania, wyrażenie sortowania można określić za pomocą obiektu `DataSourceSelectArguments` przekazaną do metody `Select` ObjectDataSource s. Ta właściwość `SortExpression` obiektu `DataSourceSelectArguments` można przypisać w programie obsługi zdarzeń `Selecting` ObjectDataSource s.

Aby dodać funkcje sortowania do elementu DataList lub wzmacniak, który już zapewnia obsługę stronicowania, najprostszym podejściem jest dostosowanie warstwy logiki biznesowej w celu uwzględnienia metody, która akceptuje wyrażenie sortowania. Te informacje można następnie przekazywać przez parametr w elemencie ObjectDataSource s `SelectParameters`.

Ten samouczek wykonuje nasze badanie stronicowania i sortowania za pomocą kontrolek DataList i wzmacniak. Nasz następnym i ostatni samouczek zapoznaj się z tematem Dodawanie kontrolek sieci Web przycisków do szablonów DataList i wzmacniak w celu zapewnienia pewnych niestandardowych, inicjowanych przez użytkownika funkcji dla poszczególnych elementów.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnego klienta dla tego samouczka był David suru. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Wstecz](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
