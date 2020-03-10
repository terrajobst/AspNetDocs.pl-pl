---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Stronicowanie danych raportu w kontrolce DataList lub wzmacniak (VB) | Microsoft Docs
author: rick-anderson
description: Chociaż żadna z elementów DataList ani wzmacniak nie oferuje automatycznej obsługi stronicowania ani sortowania, w tym samouczku pokazano, jak dodać obsługę stronicowania do elementu DataList lub wzmacniak,...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c65ca1f263e41748d99323dbdf1c28fdd077246
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78620443"
---
# <a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Stronicowanie danych raportu w kontrolce DataList lub Repeater (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) lub [Pobierz plik PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Chociaż żadna z elementów DataList ani wzmacniak nie oferuje automatycznej obsługi stronicowania ani sortowania, w tym samouczku pokazano, jak dodać obsługę stronicowania do elementu DataList lub wzmacniak, który umożliwia znacznie bardziej elastyczne interfejsy stronicowania i wyświetlania danych.

## <a name="introduction"></a>Wprowadzenie

Stronicowanie i sortowanie to dwie bardzo popularne funkcje podczas wyświetlania danych w aplikacji online. Na przykład podczas wyszukiwania książek ASP.NET w księgarni online mogą istnieć setki takich książek, ale Raport z listą wyników wyszukiwania dotyczy tylko dziesięciu odpowiedników na stronę. Ponadto wyniki mogą być sortowane według tytułu, ceny, liczby stron, nazwy autora itd. Zgodnie z opisem w samouczku dotyczącego [stronicowania i sortowania danych raportu](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , wszystkie kontrolki GridView, DetailsView i FormView zapewniają wbudowaną obsługę stronicowania, którą można włączyć na osi pola wyboru. Widok GridView obejmuje również obsługę sortowania.

Niestety, żadna z elementów DataList ani wzmacniak nie oferuje automatycznej obsługi stronicowania ani sortowania. W tym samouczku sprawdzimy, jak dodać obsługę stronicowania do elementu DataList lub wzmacniak. Konieczne jest ręczne utworzenie interfejsu stronicowania, wyświetlenie odpowiedniej strony rekordów i zapamiętanie przeglądanej strony na stronach ogłaszania zwrotnego. Chociaż to zajmie więcej czasu i kodu niż w widoku GridView, DetailsView lub FormView, element DataList i wzmacniak umożliwiają znacznie bardziej elastyczne interfejsy stronicowania i wyświetlania danych.

> [!NOTE]
> Ten samouczek koncentruje się wyłącznie na stronicowaniu. W następnym samouczku będziemy zachęcamy do dodawania funkcji sortowania.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1. Dodawanie stron sieci Web samouczka stronicowania i sortowania

Przed rozpoczęciem pracy z tym samouczkiem Poświęć chwilę na dodanie ASP.NET stron, których potrzebujemy do tego samouczka i kolejnego. Zacznij od utworzenia nowego folderu w projekcie o nazwie `PagingSortingDataListRepeater`. Następnie Dodaj następujące pięć stron ASP.NET do tego folderu, z których wszystkie są skonfigurowane do korzystania z strony wzorcowej `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`

![Utwórz folder PagingSortingDataListRepeater i Dodaj samouczek ASP.NET stron](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Rysunek 1**. tworzenie folderu `PagingSortingDataListRepeater` i dodawanie stron samouczka ASP.NET

Następnie otwórz stronę `Default.aspx` i przeciągnij kontrolkę użytkownika `SectionLevelTutorialListing.ascx` z folderu `UserControls` na powierzchnię projektu. Ta kontrolka użytkownika, utworzona na [stronach wzorcowych i](../introduction/master-pages-and-site-navigation-vb.md) w samouczku nawigacji po witrynie, wylicza mapę witryny i wyświetla te samouczki w bieżącej sekcji na liście punktowanej.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))

Aby Lista punktowana wyświetlała wyświetlane przez siebie samouczki i sortowania, należy dodać je do mapy witryny. Otwórz plik `Web.sitemap` i Dodaj następujące znaczniki po edycji i usunięciu za pomocą znacznika węzła mapy witryny DataList:

[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]

![Aktualizowanie mapy witryny w celu uwzględnienia nowych stron ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Rysunek 3**. aktualizowanie mapy witryny w celu uwzględnienia nowych stron ASP.NET

## <a name="a-review-of-paging"></a>Przegląd stronicowania

W poprzednich samouczkach przedstawiono sposób wyświetlania stron przez dane w kontrolkach GridView, DetailsView i FormView. Te trzy kontrolki oferują prostą postać stronicowania o nazwie *domyślne stronicowanie* , które można zaimplementować, po prostu zaznaczając opcję Włącz stronicowanie w tagu inteligentnym sterowania. Za pomocą domyślnego stronicowania, za każdym razem, gdy strona danych jest żądana na pierwszej stronie lub gdy użytkownik nawiguje do innej strony danych, formant GridView, DetailsView lub FormView ponownie żąda *wszystkich* danych z elementu ObjectDataSource. Następnie wykreśla określony zestaw rekordów, aby wyświetlić dane z żądanym indeksem strony oraz liczbę rekordów do wyświetlenia na stronie. Szczegółowo omówiono domyślne stronicowanie w samouczku dotyczącego [stronicowania i sortowania danych raportu](../paging-and-sorting/paging-and-sorting-report-data-vb.md) .

Ponieważ domyślne stronicowanie powoduje ponowne zażądanie wszystkich rekordów dla każdej strony, nie jest to rozwiązanie praktyczne podczas stronicowania przy użyciu wystarczająco dużych ilości danych. Załóżmy na przykład, że stronicowanie odbywa się za pomocą rekordów 50 000 o rozmiarze 10. Za każdym razem, gdy użytkownik przechodzi do nowej strony, wszystkie rekordy 50 000 muszą zostać pobrane z bazy danych, nawet jeśli są wyświetlane tylko dziesięć z nich.

*Niestandardowe stronicowanie* rozwiązuje problemy związane z wydajnością domyślnego stronicowania, ponieważ jest to możliwe tylko do wyświetlania dokładnego podzestawu rekordów, które mają być wyświetlane na żądana stronie. Podczas implementowania niestandardowego stronicowania należy napisać zapytanie SQL, które będzie efektywnie zwracać tylko poprawny zestaw rekordów. Znaleźliśmy, jak utworzyć takie zapytanie przy użyciu SQL Server 2005 s nowe [`ROW_NUMBER()` słowo kluczowe](http://www.4guysfromrolla.com/webtech/010406-1.shtml) z powrotem w [wydajnej stronicowaniu za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) — samouczek.

Aby zaimplementować domyślne stronicowanie w kontrolkach DataList lub Wzmacniake, możemy użyć [klasy`PagedDataSource`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) jako otoka wokół `ProductsDataTable`, którego zawartość jest stronicowana. Klasa `PagedDataSource` ma właściwość `DataSource`, która może być przypisana do dowolnego wyliczalnego obiektu i [`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) i [`CurrentPageIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) właściwości, które wskazują, ile rekordów ma być pokazywanych na stronie i na bieżącym indeksie strony. Po ustawieniu tych właściwości `PagedDataSource` mogą być używane jako źródło danych dowolnego formantu sieci Web danych. `PagedDataSource`, po wyliczeniu, zwróci tylko odpowiedni podzbiór rekordów jego wewnętrznego `DataSource` na podstawie właściwości `PageSize` i `CurrentPageIndex`. Rysunek 4 przedstawia funkcjonalność klasy `PagedDataSource`.

![PagedDataSource zawija wyliczalny obiekt z interfejsem stronicowania](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Rysunek 4**. `PagedDataSource` zawija obiekt wyliczalny z interfejsem stronicowania

Obiekt `PagedDataSource` można utworzyć i skonfigurować bezpośrednio z poziomu warstwy logiki biznesowej i powiązana z elementem DataList lub wzmacniak za pośrednictwem elementu ObjectDataSource lub można go utworzyć i skonfigurować bezpośrednio w klasie z kodem ASP.NET Page s. Jeśli używane jest drugie podejście, musimy forgo przy użyciu elementu ObjectDataSource i zamiast tego powiązać dane stronicowane z właściwością DataList lub wzmacniak programowo.

Obiekt `PagedDataSource` ma również właściwości do obsługi stronicowania niestandardowego. Można jednak obejść przy użyciu `PagedDataSource` niestandardowego stronicowania, ponieważ mamy już metody LOGIKI biznesowej w klasie `ProductsBLL` zaprojektowane dla niestandardowego stronicowania, które zwraca precyzyjne rekordy do wyświetlenia.

W tym samouczku Przyjrzyjmy się implementacji domyślnego stronicowania w elemencie DataList przez dodanie nowej metody do klasy `ProductsBLL`, która zwraca odpowiednio skonfigurowany obiekt `PagedDataSource`. W następnym samouczku zobaczymy, jak używać stronicowania niestandardowego.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Krok 2. Dodawanie domyślnej metody stronicowania w warstwie logiki biznesowej

Obecnie Klasa `ProductsBLL` ma metodę zwracającą wszystkie informacje o produkcie `GetProducts()` i jeden do zwrócenia określonego podzestawu produktów w `GetProductsPaged(startRowIndex, maximumRows)`indeksie początkowym. Przy użyciu domyślnego stronicowania formanty GridView, DetailsView i FormView wykorzystują metodę `GetProducts()` do pobierania wszystkich produktów, a następnie używają `PagedDataSource` wewnętrznie do wyświetlania tylko poprawnego podzestawu rekordów. Aby replikować tę funkcję za pomocą kontrolek DataList i wzmacniak, możemy utworzyć nową metodę w LOGIKI biznesowej, która naśladuje to zachowanie.

Dodaj metodę do klasy `ProductsBLL` o nazwie `GetProductsAsPagedDataSource`, która przyjmuje dwa parametry wejściowe w postaci liczby całkowitej:

- `pageIndex` indeks strony do wyświetlenia, indeksowanie na zero i
- `pageSize` liczbę rekordów do wyświetlenia na stronie.

`GetProductsAsPagedDataSource` uruchamia się, pobierając *wszystkie* rekordy z `GetProducts()`. Następnie tworzy obiekt `PagedDataSource`, ustawiając jego właściwości `CurrentPageIndex` i `PageSize` na wartości przetworzonych `pageIndex` i `pageSize` parametrów. Metoda kończy przez zwróceniem skonfigurowanego `PagedDataSource`:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Krok 3. Wyświetlanie informacji o produkcie w elemencie DataList przy użyciu domyślnego stronicowania

Za pomocą metody `GetProductsAsPagedDataSource` dodanej do klasy `ProductsBLL` możemy teraz utworzyć element DataList lub wzmacniak, który zapewnia domyślne stronicowanie. Aby rozpocząć, Otwórz stronę `Paging.aspx` w folderze `PagingSortingDataListRepeater` i przeciągnij element DataList z przybornika do projektanta, ustawiając Właściwość DataList s `ID` na `ProductsDefaultPaging`. W tagu inteligentnym DataList s Utwórz nowy element ObjectDataSource o nazwie `ProductsDefaultPagingDataSource` i skonfiguruj go tak, aby pobierał dane przy użyciu metody `GetProductsAsPagedDataSource`.

[![utworzyć elementu ObjectDataSource i skonfigurować go tak, aby korzystał z metody GetProductsAsPagedDataSource ()](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Rysunek 5**. Utwórz element ObjectDataSource i skonfiguruj go tak, aby korzystał z metody `()` `GetProductsAsPagedDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak).

[![ustawić listy rozwijane na kartach Aktualizuj, Wstaw i Usuń do (brak).](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Ilustracja 6**. Ustawianie list rozwijanych w kartach Aktualizuj, Wstaw i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

Ponieważ metoda `GetProductsAsPagedDataSource` oczekuje dwóch parametrów wejściowych, Kreator monituje nas o źródło tych wartości parametrów.

Wartości indeksu strony i rozmiaru strony muszą być zapamiętywane na stronach ogłaszania zwrotnego. Mogą być przechowywane w stanie widoku, utrwalone w ciągu QueryString, przechowywane w zmiennych sesji lub zapamiętane przy użyciu innej techniki. Na potrzeby tego samouczka użyjemy ciągu QueryString, który pozwala na tworzenie zakładek określonej strony danych.

W szczególności należy odpowiednio użyć pól QueryString i pageSize dla parametrów `pageIndex` i `pageSize` (patrz rysunek 7). Poświęć chwilę na ustawienie wartości domyślnych dla tych parametrów, ponieważ wartości QueryString nie będą wyświetlane, gdy użytkownik po raz pierwszy odwiedzi tę stronę. Dla `pageIndex`ustaw wartość domyślną na 0 (co spowoduje wyświetlenie pierwszej strony danych), a wartość domyślna `pageSize` s na 4.

[![użyć ciągu QueryString jako źródła parametrów pageIndex i pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Rysunek 7**. Użyj ciągu QueryString jako źródła parametrów `pageIndex` i `pageSize` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))

Po skonfigurowaniu elementu ObjectDataSource program Visual Studio automatycznie tworzy `ItemTemplate` dla elementu DataList. Dostosuj `ItemTemplate` tak, aby były wyświetlane tylko nazwy produktu, kategorii i dostawcy. Ustaw również Właściwość DataList s `RepeatColumns` na wartość 2, jej `Width` na 100%, a jej `ItemStyle` s `Width` do 50%. Te ustawienia szerokości zapewniają równe odstępy dla dwóch kolumn.

Po wprowadzeniu tych zmian znaczniki DataList i ObjectDataSource s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Ponieważ nie wykonujemy żadnej funkcji aktualizacji ani usuwania w tym samouczku, możesz wyłączyć stan widoku DataList s, aby zmniejszyć rozmiar renderowanej strony.

Po wstępnym odwiedzeniu tej strony za pomocą przeglądarki nie podano `pageIndex` ani `pageSize` parametrów QueryString. W związku z tym są używane wartości domyślne 0 i 4. Jak pokazano na rysunku 8, spowoduje to wyświetlenie elementu DataList zawierającego pierwsze cztery produkty.

[![wymienione są pierwsze cztery produkty](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Ilustracja 8**. wymienione są pierwsze cztery produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))

Bez interfejsu stronicowania nie jest obecnie proste, aby użytkownik mógł przejść do drugiej strony danych. Utworzymy interfejs stronicowania w kroku 4. Dla tej pory stronicowanie można wykonać tylko przez bezpośrednie określenie kryteriów stronicowania w ciągu QueryString. Na przykład aby wyświetlić drugą stronę, Zmień adres URL na pasku adresu przeglądarki z `Paging.aspx` na `Paging.aspx?pageIndex=2` i naciśnij klawisz ENTER. Powoduje to wyświetlenie drugiej strony danych (patrz rysunek 9).

[![zostanie wyświetlona druga strona danych](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Ilustracja 9**. wyświetlana jest druga strona danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))

## <a name="step-4-creating-the-paging-interface"></a>Krok 4. Tworzenie interfejsu stronicowania

Istnieją różne interfejsy stronicowania, które mogą być implementowane. Kontrolki GridView, DetailsView i FormView udostępniają cztery różne interfejsy, spośród których można wybierać:

- **Następnie Poprzedni** użytkownicy mogą przenieść jedną stronę jednocześnie do następnej lub poprzedniej.
- Przycisk **Dalej, poprzedni, pierwszy, ostatni,** oprócz przycisków dalej i wstecz, ten interfejs zawiera pierwsze i ostatnie przyciski do przechodzenia do pierwszej lub bardzo ostatniej strony.
- Wartość **liczbowa** wyświetla listę numerów stron w interfejsie stronicowania, co umożliwia użytkownikowi szybkie przechodzenie do określonej strony.
- **Liczbowa, pierwsza, Ostatnia** oprócz liczbowych numerów stron, zawiera przyciski do przechodzenia do pierwszej lub bardzo ostatniej strony.

Dla elementu DataList i wzmacniak są odpowiedzialni za decydowanie o interfejsie stronicowania i jego wdrożeniu. Obejmuje to utworzenie wymaganych kontrolek sieci Web na stronie i wyświetlenie strony żądana, gdy zostanie kliknięty określony przycisk interfejsu stronicowania. Ponadto może być konieczne wyłączenie pewnych kontrolek interfejsu stronicowania. Na przykład podczas przeglądania pierwszej strony danych przy użyciu przycisków dalej, poprzedni, pierwszy i ostatni, zarówno pierwszy, jak i poprzedni zostaną wyłączone.

Na potrzeby tego samouczka Użyj instrukcji use Next, Previous, First i Last. Dodaj cztery kontrolki sieci Web przycisków na stronie i ustaw ich `ID` s na `FirstPage`, `PrevPage`, `NextPage`i `LastPage`. Ustaw właściwości `Text`, aby &lt;&lt; pierwsze, &lt; poprzedni, następny &gt;i ostatni &gt;&gt;.

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń `Click` dla każdego z tych przycisków. W chwili dodamy kod niezbędny do wyświetlenia wybranej strony.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Zapamiętywanie łącznej liczby rekordów na stronie

Niezależnie od wybranego interfejsu stronicowania, musimy obliczyć i zapamiętać łączną liczbę rekordów, które są stronicowane. Łączna liczba wierszy (w połączeniu z rozmiarem strony) określa, ile łącznych stron danych jest stronicowanych przez, co określa, jakie kontrolki interfejsu stronicowania są dodawane lub są włączone. W następnym, poprzednim, pierwszym, ostatnim interfejsie, który tworzysz, liczba stron jest używana na dwa sposoby:

- Aby określić, czy jest wyświetlana Ostatnia strona, w takim przypadku przyciski Następny i ostatni są wyłączone.
- Jeśli użytkownik kliknie ostatni przycisk, musimy przewhiskić je do ostatniej strony, której indeks jest mniejszy niż liczba stron.

Liczba stron jest obliczana jako wartość limitu całkowitej liczby wierszy podzielona przez rozmiar strony. Na przykład jeśli dane są stronicowane za pomocą 79 rekordów z czterema rekordami na stronie, liczba stron wynosi 20 (górny limit 79/4). Jeśli korzystamy z interfejsu stronicowania numerycznego, te informacje informuje nas, jak wiele przycisków stron numerycznych ma być wyświetlanych; Jeśli interfejs stronicowania zawiera przyciski Dalej lub ostatnie, liczba stron jest używana do określenia, kiedy należy wyłączyć następne lub ostatnie przyciski.

Jeśli interfejs stronicowania zawiera ostatni przycisk, należy pamiętać, że całkowita liczba rekordów, które są stronicowane w ramach ogłaszania zwrotnego, tak, aby po kliknięciu ostatniego przycisku można było określić ostatni indeks strony. Aby to ułatwić, należy utworzyć właściwość `TotalRowCount` w klasie z kodem "ASP.NET Page s", która zachowuje swoją wartość w celu wyświetlenia stanu:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Oprócz `TotalRowCount`Poświęć minutę na utworzenie właściwości na poziomie strony tylko do odczytu, aby ułatwić uzyskiwanie dostępu do indeksu strony, rozmiaru strony i liczby stron:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Określanie łącznej liczby rekordów na stronie

Obiekt `PagedDataSource` zwrócony przez metodę `Select()` ObjectDataSource s znajduje się w obrębie *wszystkich* rekordów produktu, nawet jeśli tylko podzbiór są wyświetlane na liście DataList. [Właściwość`Count`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) `PagedDataSource` s zwraca tylko liczbę elementów, które będą wyświetlane na liście DataList; [właściwość`DataSourceCount`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) zwraca łączną liczbę elementów w `PagedDataSource`. W związku z tym musimy przypisać ASP.NET strony `TotalRowCount` wartość właściwości `DataSourceCount` `PagedDataSource` s.

Aby to osiągnąć, Utwórz procedurę obsługi zdarzeń dla zdarzenia `Selected` elementu ObjectDataSource s. W programie obsługi zdarzeń `Selected` mamy dostęp do wartości zwracanej przez metodę `Select()` ObjectDataSource s w tym przypadku `PagedDataSource`.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Wyświetlanie żądanej strony danych

Gdy użytkownik kliknie jeden z przycisków w interfejsie stronicowania, musi wyświetlić żądaną stronę danych. Ponieważ parametry stronicowania są określone za pośrednictwem ciągu QueryString, aby wyświetlić żądaną stronę danych, użyj `Response.Redirect(url)`, aby przeglądarka użytkownika zażądała ponownego żądania strony `Paging.aspx` z odpowiednimi parametrami stronicowania. Na przykład aby wyświetlić drugą stronę danych, należy przekierować użytkownika do `Paging.aspx?pageIndex=1`.

Aby to ułatwić, należy utworzyć metodę `RedirectUser(sendUserToPageIndex)`, która przekierowuje użytkownika do `Paging.aspx?pageIndex=sendUserToPageIndex`. Następnie Wywołaj tę metodę z czterema przyciskami `Click` obsługi zdarzeń. W programie obsługi zdarzeń `Click` `FirstPage` Wywołaj `RedirectUser(0)`, aby wysłać je do pierwszej strony; w programie obsługi zdarzeń `Click` `PrevPage` Użyj `PageIndex - 1` jako indeksu strony. i tak dalej.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Po zakończeniu obsługi zdarzeń `Click` rekordy DataList s mogą być stronicowane przez kliknięcie przycisków. Poświęć chwilę, aby ją wypróbować!

## <a name="disabling-paging-interface-controls"></a>Wyłączanie kontrolek interfejsu stronicowania

Obecnie wszystkie cztery przyciski są włączane niezależnie od wyświetlanej strony. Jednak chcemy wyłączyć przyciski pierwszy i poprzedni przy pokazywaniu pierwszej strony danych oraz przyciski Następny i ostatni podczas wyświetlania ostatniej strony. Obiekt `PagedDataSource` zwrócony przez metodę `Select()` ObjectDataSource s ma właściwości [`IsFirstPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) i [`IsLastPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , które możemy sprawdzić, aby określić, czy oglądamy pierwszą lub ostatnią stronę danych.

Dodaj następujący element do programu obsługi zdarzeń `Selected` ObjectDataSource s:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

W przypadku tego dodania przyciski pierwszy i poprzedni zostaną wyłączone podczas wyświetlania pierwszej strony, podczas gdy podczas wyświetlania ostatniej strony zostaną wyłączone przyciski Następny i ostatni.

Niech s uzupełnia interfejs stronicowania, informując użytkownika o tym, co strona aktualnie przegląda i ile z nich łączą się. Dodaj kontrolkę sieci Web etykieta do strony i ustaw jej Właściwość `ID` na `CurrentPageNumber`. Ustaw jej Właściwość `Text` w procedurze obsługi zdarzeń w wybranym elemencie ObjectDataSource s tak, aby zawierała bieżącą stronę wyświetlaną (`PageIndex + 1`) i łączną liczbę stron (`PageCount`).

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

Rysunek 10 pokazuje `Paging.aspx` podczas pierwszej wizyty. Ponieważ ciąg QueryString jest pusty, domyślnie wyświetlana jest wartość domyślna elementu DataList pokazująca pierwsze cztery produkty; przyciski pierwszy i poprzedni są wyłączone. Kliknięcie przycisku Dalej wyświetla następne cztery rekordy (patrz rysunek 11). przyciski pierwszy i poprzedni są teraz włączone.

[![zostanie wyświetlona pierwsza strona danych](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Ilustracja 10**. wyświetlana jest pierwsza strona danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))

[![zostanie wyświetlona druga strona danych](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Ilustracja 11**. wyświetlana jest druga strona danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))

> [!NOTE]
> Interfejs stronicowania można dodatkowo zwiększyć, umożliwiając użytkownikowi określenie, ile stron ma być wyświetlanych na stronie. Na przykład DropDownList można dodać listę opcji rozmiaru strony, takich jak 5, 10, 25, 50 i wszystkie. Po wybraniu rozmiaru strony użytkownik powinien zostać przekierowany z powrotem do `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Pozostawiam to ulepszenie jako ćwiczenie dla czytnika.

## <a name="using-custom-paging"></a>Używanie niestandardowego stronicowania

Strony DataList za pośrednictwem swoich danych przy użyciu niewydajnej domyślnej techniki stronicowania. Podczas stronicowania przy użyciu wystarczająco dużych ilości danych jest konieczne użycie stronicowania niestandardowego. Chociaż szczegóły implementacji różnią się nieznacznie, koncepcje związane z implementacją niestandardowego stronicowania w elemencie DataList są takie same jak w przypadku domyślnego stronicowania. Przy użyciu niestandardowego stronicowania Użyj metody `ProductBLL` klasy s `GetProductsPaged` (zamiast `GetProductsAsPagedDataSource`). Jak opisano w [efektywnej stronicowaniu za pomocą dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) , `GetProductsPaged` należy przekazywać indeks początkowych wierszy i maksymalną liczbę wierszy do zwrócenia. Te parametry można utrzymywać za pomocą ciągu QueryString podobnie jak parametry `pageIndex` i `pageSize` używane w domyślnym stronicowaniu.

Ponieważ nie ma żadnych `PagedDataSource` z stronicowaniem niestandardowym, należy użyć alternatywnych technik do określenia łącznej liczby rekordów, które są wyświetlane w tym miejscu, oraz od tego, czy będziemy ponownie wyświetlać pierwszą lub ostatnią stronę danych. Metoda `TotalNumberOfProducts()` w klasie `ProductsBLL` zwraca łączną liczbę produktów, które są stronicowane. Aby określić, czy pierwsza strona danych jest wyświetlana, sprawdź, czy jest wyświetlany indeks wierszy początkowych, jeśli jest równa zero, a następnie jest wyświetlana pierwsza strona. Ostatnia strona jest wyświetlana, jeśli indeks wierszy początkowych i Maksymalna liczba wierszy do zwrócenia jest większa lub równa łącznej liczbie rekordów, które są wyświetlane na stronie.

W następnym samouczku dowiesz się, jak wdrażać niestandardowe stronicowanie w bardziej szczegółowy sposób.

## <a name="summary"></a>Podsumowanie

Chociaż ani element DataList, ani wzmacniak nie oferuje funkcji obsługi stronicowania Box dostępnej w kontrolkach GridView, DetailsView i FormView, można dodawać takie funkcje z minimalnym nakładem pracy. Najprostszym sposobem implementacji stronicowania domyślnego jest Zawijanie całego zestawu produktów w ramach `PagedDataSource`, a następnie powiązanie `PagedDataSource` z elementem DataList lub Wzmacniake. W tym samouczku dodaliśmy metodę `GetProductsAsPagedDataSource` do klasy `ProductsBLL` w celu zwrócenia `PagedDataSource`. Klasa `ProductsBLL` zawiera już metody, które są zbędne dla niestandardowego `GetProductsPaged` stronicowania i `TotalNumberOfProducts`.

Wraz z pobraniem precyzyjnego zestawu rekordów do wyświetlania dla niestandardowego stronicowania lub wszystkich rekordów w `PagedDataSource` dla domyślnego stronicowania, należy również ręcznie dodać interfejs stronicowania. W tym samouczku utworzyliśmy następny, poprzedni, pierwszy, ostatni interfejs z czterema kontrolkami sieci Web. Dodatkowo dodano kontrolkę etykieta wyświetlającą bieżący numer strony i całkowitą liczbę stron.

W następnym samouczku zobaczymy, jak dodać obsługę sortowania do elementu DataList i wzmacniak. Zobaczymy również, jak utworzyć element DataList, który może być zarówno stronicowane, jak i posortowany (z przykładami przy użyciu domyślnego i niestandardowego stronicowania).

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Liz Shulok, Krzysztof Pespisa i Bernadette Leigh. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [dalej](sorting-data-in-a-datalist-or-repeater-control-vb.md)
