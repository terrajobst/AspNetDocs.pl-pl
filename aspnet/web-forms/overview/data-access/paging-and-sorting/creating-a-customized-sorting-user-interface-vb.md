---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: Tworzenie dostosowanego interfejsu użytkownika sortowania (VB) | Microsoft Docs
author: rick-anderson
description: Podczas wyświetlania długiej listy posortowanych danych może być bardzo przydatna do grupowania powiązanych danych przez wprowadzenie wierszy separatora. W tym samouczku zobaczymy, jak u...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 66127630560141cd795beb15f525a7fba85f3993
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607343"
---
# <a name="creating-a-customized-sorting-user-interface-vb"></a>Tworzenie dostosowanego interfejsu użytkownika sortowania (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) lub [Pobierz plik PDF](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Podczas wyświetlania długiej listy posortowanych danych może być bardzo przydatna do grupowania powiązanych danych przez wprowadzenie wierszy separatora. W tym samouczku dowiesz się, jak utworzyć taki interfejs użytkownika do sortowania.

## <a name="introduction"></a>Wprowadzenie

W przypadku wyświetlania długiej listy posortowanych danych w przypadku, gdy w sortowanej kolumnie znajduje się tylko kilku różnych wartości, użytkownik końcowy może się przekonać, że rozpoznać, gdzie dokładnie występują granice różnic. Na przykład w bazie danych znajdują się 81 produkty, ale dostępne są tylko dziewięć różnych kategorii (osiem unikatowych kategorii i opcja `NULL`). Rozważmy przypadek przypadku użytkownika, który interesuje badanie produktów, które podlegają kategorii owoce morza. Na stronie, która wyświetla listę *wszystkich* produktów w jednym widoku GridView, użytkownik może zdecydować, aby posortować wyniki według kategorii, które będą grupować razem wszystkie produkty z owoców. Po posortowaniu według kategorii użytkownik musi przeglądać listę, gdzie są organizowane i kończyć produkty pogrupowane. Ponieważ wyniki są uporządkowane alfabetycznie według nazwy kategorii znalezienie produktów morza nie jest trudne, ale nadal wymaga ścisłego skanowania listy elementów w siatce.

Aby wyróżnić granice między posortowanymi grupami, wiele witryn sieci Web korzysta z interfejsu użytkownika, który dodaje separator między tymi grupami. Separatory takie jak pokazane na rysunku 1 umożliwiają użytkownikowi szybsze znalezienie określonej grupy i zidentyfikowanie jej granic, a także ustalenie, jakie odrębne grupy istnieją w danych.

[![każda grupa kategorii jest jasno zidentyfikowana](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Rysunek 1**. Każda grupa kategorii jest jasno zidentyfikowana ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-vb/_static/image3.png))

W tym samouczku dowiesz się, jak utworzyć taki interfejs użytkownika do sortowania.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Krok 1. Tworzenie standardowego, sortowanie GridView

Przed zapoznawaniem się, jak rozszerzyć widok GridView w celu zapewnienia ulepszonego interfejsu sortowania, pozwól najpierw utworzyć standardowy, do sortowania GridView, który wyświetla listę produktów. Zacznij od otwarcia strony `CustomSortingUI.aspx` w folderze `PagingAndSorting`. Dodaj widok GridView do strony, ustaw jej Właściwość `ID` na `ProductList`i powiąż ją z nowym elementem ObjectDataSource. Skonfiguruj element ObjectDataSource, aby używał metody `ProductsBLL` klasy s `GetProducts()` do wybierania rekordów.

Następnie skonfiguruj widok GridView, tak aby zawierał tylko `ProductName`, `CategoryName`, `SupplierName`i `UnitPrice` BoundFields oraz wycofane CheckBoxField. Na koniec Skonfiguruj widok GridView, aby obsługiwał sortowanie, zaznaczając pole wyboru Włącz sortowanie w tagu inteligentnym "GridView s" (lub ustawiając właściwość `AllowSorting` na `true`). Po wprowadzeniu tych dodatków do strony `CustomSortingUI.aspx`, znaczniki deklaratywne powinny wyglądać podobnie do następujących:

[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Poświęć chwilę na przejrzenie tego postępu w przeglądarce. Rysunek 2 przedstawia widok GridView do sortowania, gdy jego dane są sortowane według kategorii w kolejności alfabetycznej.

[![dane GridView do sortowania są uporządkowane według kategorii](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Rysunek 2**. sortowanie danych w widoku GridView jest uporządkowane według kategorii ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-customized-sorting-user-interface-vb/_static/image6.png))

## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Krok 2. Eksplorowanie technik dodawania wierszy separatora

Dzięki ogólnemu sortowaniu w widoku GridView wszystko to jest możliwe, aby dodać wiersze separatora w widoku GridView przed każdą unikatową grupą sortowania. Ale jak można wprowadzić takie wiersze do widoku GridView? Zasadniczo należy wykonać iterację w wierszach GridView, określić, gdzie występują różnice między wartościami w sortowanej kolumnie, a następnie dodać odpowiedni wiersz separatora. W przypadku wystąpienia tego problemu wydaje się, że rozwiązanie należy do programu obsługi zdarzeń `RowDataBound` GridView. Zgodnie z opisem w temacie [formatowanie niestandardowe oparte](../custom-formatting/custom-formatting-based-upon-data-vb.md) na samouczku danych ten program obsługi zdarzeń jest często używany podczas stosowania formatowania na poziomie wiersza na podstawie danych wiersza. Jednak program obsługi zdarzeń `RowDataBound` nie jest tym rozwiązaniem, ponieważ nie można dodać wierszy do widoku GridView programowo z tego programu obsługi zdarzeń. Kolekcja `Rows` GridView, w rzeczywistości, jest tylko do odczytu.

Aby dodać dodatkowe wiersze do widoku GridView, mamy trzy opcje:

- Dodaj te wiersze separatora metadanych do rzeczywistych danych, które są powiązane z elementem GridView
- Po powiązaniu widoku GridView z danymi Dodaj kolejne wystąpienia `TableRow` do kolekcji formantów GridView
- Utwórz niestandardową kontrolkę serwera, która rozszerza formant GridView i zastępuje te metody, które są odpowiedzialne za konstruowanie struktury GridView

Tworzenie niestandardowej kontrolki serwerowej byłoby najlepszym rozwiązaniem, jeśli ta funkcja była wymagana na wielu stronach sieci Web lub w kilku witrynach internetowych. Jednak może to pociągnąć za sobą trochę kodu i gruntownie poszukiwanie głębokości wewnętrznych działań w widoku GridView. W związku z tym nie należy uwzględniać tej opcji w tym samouczku.

Pozostałe dwie opcje dodają wiersze separatora do rzeczywistych danych, które są powiązane z widokiem GridView i manipulowania kolekcją formantów GridView, po jej zastosowaniu

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Dodawanie wierszy do danych powiązanych z elementem GridView

Gdy widok GridView jest powiązany ze źródłem danych, tworzy `GridViewRow` dla każdego rekordu zwróconego przez źródło danych. W związku z tym możemy wstrzyknąć niezbędne wiersze separatora, dodając rekordy separatora do źródła danych przed powiązaniem go z GridView. Rysunek 3 ilustruje tę koncepcję.

![Jedna technika obejmuje Dodawanie wierszy separatora do źródła danych](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Rysunek 3**. jedna technika obejmuje dodanie wierszy separatora do źródła danych

Używam rekordów separatorów terminów w cudzysłowie, ponieważ nie ma specjalnego rekordu separatora; Zamiast tego musimy mieć dowolną flagę, że konkretny rekord w źródle danych służy jako separator, a nie normalny wiersz danych. Nasze przykłady umożliwiają ponowne wiązanie wystąpienia `ProductsDataTable` w widoku GridView, który składa się z `ProductRows`. Możemy oflagować rekord jako wiersz separatora przez ustawienie jego właściwości `CategoryID` na `-1` (ponieważ taka wartość nie może istnieć normalnie).

Aby użyć tej techniki, należy wykonać następujące czynności:

1. Programowe pobieranie danych w celu utworzenia powiązania z elementem GridView (wystąpienie `ProductsDataTable`)
2. Sortuj dane na podstawie `SortExpression` GridView i właściwości `SortDirection`
3. Wykonaj iterację `ProductsRows` w `ProductsDataTable`, szukając miejsc, w których znajdują się różnice w sortowanej kolumnie.
4. W każdej granicy grupy wstrzyknąć rekord separatora `ProductsRow` wystąpienia do elementu DataTable, który ma `CategoryID` z ustawionym na `-1` (lub każde oznaczenie zostało ustalone, aby oznaczyć rekord jako rekord separatora).
5. Po wprowadzeniu wierszy separatora program programowo wiąże dane z GridView

Oprócz tych pięciu kroków Ponadto należy zapewnić obsługę zdarzeń dla zdarzenia `RowDataBound` GridView. W tym miejscu należy sprawdzić wszystkie `DataRow` i określić, czy był to wiersz separatora, po którym `-1`zostało `CategoryID` ustawienie. Jeśli tak, to mamy d prawdopodobnie chcieć dostosować formatowanie lub tekst wyświetlany w komórkach.

Użycie tej techniki do wstrzykiwania granic grup sortowania wymaga nieco więcej pracy niż opisano powyżej, co jest potrzebne do zapewnienia obsługi zdarzeń dla zdarzenia GridView `Sorting` i śledzenia wartości `SortExpression` i `SortDirection`.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulowanie kolekcją formantów GridView

Zamiast przesyłać komunikaty do danych przed powiązaniem ich z GridView, można dodać wiersze separatora *po* powiązaniu danych z elementem GridView. Proces powiązania danych kompiluje hierarchię formantów GridView, która w rzeczywistości jest po prostu `Table` wystąpieniem składającym się z kolekcji wierszy, z których każdy składa się z kolekcji komórek. W odniesieniu do kolekcji "GridView" w elemencie WebControl zawiera obiekt `Table` w jego elemencie głównym, `GridViewRow` (który pochodzi z klasy `TableRow`) dla każdego rekordu w `DataSource` powiązanego z elementem GridView i `TableCell` obiektu w każdym `GridViewRow` wystąpienia dla każdego pola danych w `DataSource`.

Aby dodać wiersze separatora między każdą grupą sortowania, można bezpośrednio manipulować tą hierarchią formantów po jej utworzeniu. Mamy pewność, że hierarchia formantów GridView s została utworzona po raz ostatni przez czas renderowania strony. W związku z tym takie podejście zastępuje metodę `Page` klasy `Render` s, w którym zostanie zaktualizowana Ostatnia hierarchia kontrolki GridView s w celu uwzględnienia wymaganych wierszy separatora. Na rysunku 4 przedstawiono ten proces.

[![technikę alternatywną manipuluje hierarchią formantów GridView](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Rysunek 4**. Metoda alternatywna manipuluje hierarchią formantów GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-vb/_static/image10.png))

W tym samouczku użyjemy tego ostatniego podejścia do dostosowania środowiska użytkownika do sortowania.

> [!NOTE]
> Kod I Wyrażam w tym samouczku jest oparty na przykładzie podanym w wpisie na blogu [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s, [odtwarzając bit z grupowaniem sortowania GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).

## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Krok 3. Dodawanie wierszy separatora do hierarchii formantów GridView

Ponieważ chcemy dodać wiersze separatora do hierarchii formantów GridView, po utworzeniu hierarchii kontroli i utworzeniu jej po raz ostatni na tej stronie, chcemy wykonać to dodawanie na końcu cyklu życia strony, ale przed rzeczywistym formantem GridView c Hierarchia ontrola została renderowana w kodzie HTML. Ostatnim możliwym punktem, w którym możemy to zrobić, jest zdarzenie `Page` klasy s `Render`, które możemy przesłonić w naszej klasie związanej z kodem przy użyciu następującej sygnatury metody:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Gdy metoda pierwotna `Render` klasy `Page` jest wywoływana `base.Render(writer)` każda kontrolka na stronie będzie renderowana, generując znaczniki na podstawie ich hierarchii formantów. W związku z tym należy zastanowić się, że obie metody wywołują `base.Render(writer)`, tak że strona jest renderowana i manipuluje hierarchią formantów GridView, przed wywołaniem `base.Render(writer)`, aby wiersze separatorów zostały dodane do hierarchii formantów GridView.

Aby wstrzyknąć nagłówki grup sortowania, najpierw musimy upewnić się, że użytkownik zażądał posortowania danych. Domyślnie zawartość GridView nie jest sortowana i w związku z tym nie trzeba wprowadzać żadnych nagłówków sortowania grup.

> [!NOTE]
> Jeśli chcesz, aby GridView był sortowany według określonej kolumny podczas pierwszego ładowania strony, wywołaj metodę `Sort` GridView na pierwszej stronie odwiedzaj (ale nie na kolejnych ogłaszania zwrotnego). Aby to osiągnąć, należy dodać to wywołanie w obsłudze zdarzeń `Page_Load` w ramach `if (!Page.IsPostBack)` warunkowego. Zapoznaj się z powrotem z informacjami o samouczku dotyczącym [stronicowania i sortowania danych raportu](paging-and-sorting-report-data-vb.md) , aby uzyskać więcej informacji na temat metody `Sort`.

Przy założeniu, że dane zostały posortowane, następne zadanie polega na określeniu kolumny, według której dane zostały posortowane, a następnie do skanowania wierszy szukających różnic w tych wartościach kolumn. Poniższy kod gwarantuje, że dane zostały posortowane i znajduje kolumnę, według której zostały posortowane dane:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Jeśli widok GridView nie został jeszcze posortowany, właściwość GridView `SortExpression` nie zostanie ustawiona. W związku z tym chcemy tylko dodać wiersze separatora, jeśli ta właściwość ma pewną wartość. Jeśli tak, następnym musimy określić indeks kolumny, według której zostały posortowane dane. Jest to realizowane przez zapętlenie w kolekcji `Columns` GridView i wyszukiwanie kolumny, której Właściwość `SortExpression` jest równa właściwości GridView w `SortExpression`. Oprócz indeksu kolumn s ponosimy również właściwość `HeaderText`, która jest używana podczas wyświetlania wierszy separatora.

W przypadku indeksu kolumny, według której są sortowane dane, ostatnim krokiem jest Wyliczenie wierszy widoku GridView. Dla każdego wiersza należy określić, czy posortowana wartość kolumny s różni się od posortowanej wartości kolumny s z poprzedniego wiersza s. Jeśli tak, musimy wstrzyknąć nowe wystąpienie `GridViewRow` do hierarchii formantów. Jest to realizowane przy użyciu następującego kodu:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Ten kod jest uruchamiany przez programowe odwołujące się do `Table` obiektu znajdującego się w katalogu głównym hierarchii formantów GridView i tworząc zmienną ciągu o nazwie `lastValue`. `lastValue` służy do porównywania bieżącego wiersza s posortowanej kolumny z poprzednim wierszem wartości. Następnie zostanie wyliczona kolekcja `Rows` GridView i dla każdego wiersza wartość posortowanej kolumny jest przechowywana w zmiennej `currentValue`.

> [!NOTE]
> Aby określić wartość konkretnej kolumny posortowanej wiersz s, użyj właściwości `Text` komórki. To dobrze sprawdza się w przypadku BoundFields, ale nie będzie działać zgodnie z potrzebami używanie TemplateField, CheckBoxFields i tak dalej. Wkrótce dowiesz się, jak szybko rozróżnić pola GridView.

Zmienne `currentValue` i `lastValue` są porównywane. Jeśli różnią się one, należy dodać nowy wiersz separatora do hierarchii formantów. W tym celu należy określić indeks `GridViewRow` w kolekcji `Table` obiektów `Rows`, tworząc nowe wystąpienia `GridViewRow` i `TableCell`, a następnie dodając `TableCell` i `GridViewRow` do hierarchii formantów.

Należy zauważyć, że wiersz separatora s Lone `TableCell` jest sformatowany w taki sposób, że obejmuje całą szerokość GridView, jest formatowana przy użyciu `SortHeaderRowStyle`j klasy CSS i ma właściwość `Text` w taki sposób, aby pokazywała zarówno nazwę grupy sortowania (na przykład kategorię), jak i wartość grupy (na przykład napoje). Na koniec `lastValue` jest aktualizowana do wartości `currentValue`.

Klasa CSS używana do formatowania wiersza nagłówka grupy sortowania `SortHeaderRowStyle` musi być określona w pliku `Styles.css`. Korzystaj z dowolnych ustawień stylów, które można odwoływać do Ciebie. Zostały użyte następujące elementy:

[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

W bieżącym kodzie interfejs sortowania dodaje nagłówki grup sortowania podczas sortowania według dowolnego BoundFieldu (zobacz rysunek 5, który pokazuje zrzut ekranu podczas sortowania według dostawcy). Jednak podczas sortowania według dowolnego innego typu pola (na przykład CheckBoxField lub TemplateField) nagłówki grup sortowania są Nowhere (zobacz rysunek 6).

[![interfejs sortowania zawiera nagłówki grup sortowania podczas sortowania według BoundFields](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Rysunek 5**. interfejs sortowania zawiera nagłówki grup sortowania podczas sortowania według BoundFields ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-vb/_static/image13.png))

[![brak nagłówków grupy sortowania podczas sortowania CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Ilustracja 6**. brak nagłówków grupy sortowania podczas sortowania CheckBoxField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-vb/_static/image16.png))

Brak nagłówków grupy sortowania podczas sortowania przez CheckBoxField jest to spowodowane tym, że kod używa obecnie tylko właściwości `TableCell` s `Text` do określenia wartości kolumny posortowanej dla każdego wiersza. W przypadku CheckBoxFields Właściwość `Text` `TableCell` jest pustym ciągiem; Zamiast tego wartość jest dostępna za pomocą kontrolki sieci Web CheckBox, która znajduje się w kolekcji `TableCell` s `Controls`.

Aby obsłużyć typy pól inne niż BoundFields, musimy rozszerzyć kod, do którego przypisano zmienną `currentValue`, aby sprawdzić obecność pola wyboru w kolekcji `TableCell` s `Controls`. Zamiast używać `currentValue = gvr.Cells(sortColumnIndex).Text`, Zastąp ten kod następującym:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Ten kod sprawdza posortowaną kolumnę `TableCell` dla bieżącego wiersza, aby określić, czy w kolekcji `Controls` znajdują się jakiekolwiek kontrolki. Jeśli istnieją, a pierwszy formant to pole wyboru, zmienna `currentValue` jest ustawiona na tak lub nie, w zależności od Właściwości CheckBox `Checked`. W przeciwnym razie wartość jest pobierana z właściwości `Text` `TableCell`. Tę logikę można zreplikować, aby obsłużyć sortowanie dla wszystkich używanie TemplateField, które mogą istnieć w widoku GridView.

Za pomocą powyższego kodu nagłówki grup sortowania są teraz obecne podczas sortowania przez wycofane CheckBoxField (patrz rysunek 7).

[![nagłówki grup sortowania są teraz obecne podczas sortowania CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Rysunek 7**. nagłówki grup sortowania są teraz obecne podczas sortowania CheckBoxField (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-vb/_static/image19.png))

> [!NOTE]
> Jeśli masz produkty mające `NULL` wartości bazy danych dla pól `CategoryID`, `SupplierID`lub `UnitPrice`, te wartości będą wyświetlane jako puste ciągi w widoku GridView domyślnie, co oznacza, że tekst wiersza separatora dla tych produktów z wartościami `NULL` będzie odczytywany jak Kategoria: (to znaczy, że nie ma nazwy po kategorii: podobnej do kategorii: napoje). Jeśli chcesz wyświetlić wartość w tym miejscu, możesz ustawić [Właściwość BoundFields`NullDisplayText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) na tekst, który ma być wyświetlany, lub dodać instrukcję warunkową do metody Render podczas przypisywania `currentValue` do właściwości `Text` wiersza separatora.

## <a name="summary"></a>Podsumowanie

Widok GridView nie zawiera wielu wbudowanych opcji dostosowywania interfejsu sortowania. Jednak przy użyciu bitu kodu niskiego poziomu można dostosować hierarchię formantów GridView, aby utworzyć bardziej dostosowany interfejs. W tym samouczku dowiesz się, jak dodać wiersz separatora grupy sortowania dla widoku GridView do sortowania, który łatwiej identyfikuje odrębne grupy i te grupy. Aby zapoznać się z dodatkowymi przykładami niestandardowych interfejsów sortowania, zapoznaj się z [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [a ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) .

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Ubiegł](sorting-custom-paged-data-vb.md)
