---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: Badanie zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku będziemy analizować dane przy użyciu zdarzeń występujących przed operacją INSERT, Update lub DELETE formantu sieci Web ASP.NET Data. W...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 35edb3cefc6fe23bb56e667c02d10dc7798f730d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633134"
---
# <a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>Badanie zdarzeń powiązanych ze wstawianiem, aktualizowaniem i usuwaniem (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe) lub [Pobierz plik PDF](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> W tym samouczku będziemy analizować dane przy użyciu zdarzeń występujących przed operacją INSERT, Update lub DELETE formantu sieci Web ASP.NET Data. Zobaczymy również, jak dostosować interfejs edycji, aby zaktualizować podzestaw pól produktu.

## <a name="introduction"></a>Wprowadzenie

W przypadku korzystania z wbudowanych funkcji wstawiania, edytowania lub usuwania elementów sterujących GridView, DetailsView lub FormView, wiele kroków wykonywanych, gdy użytkownik końcowy ukończy proces dodawania nowego rekordu lub aktualizowania lub usuwania istniejącego rekordu. Zgodnie z opisem w [poprzednim samouczku](an-overview-of-inserting-updating-and-deleting-data-vb.md), gdy wiersz jest edytowany w widoku GridView, przycisk Edytuj jest zamieniany na przyciski Update i Cancel i BoundFields się na pola tekstowe. Po zaktualizowaniu danych przez użytkownika końcowego i kliknięciu przycisku Aktualizuj na stronie ogłaszania zwrotnego wykonywane są następujące czynności:

1. W widoku GridView są wypełniane `UpdateParameters` elementu ObjectDataSource przy użyciu unikatowych pól identyfikacyjnych rekordu (za pośrednictwem `DataKeyNames` właściwości) wraz z wartościami wprowadzonymi przez użytkownika
2. Widok GridView wywołuje metodę `Update()` elementu ObjectDataSource, która z kolei wywołuje odpowiednią metodę w obiekcie źródłowym (`ProductsDAL.UpdateProduct`, w naszym poprzednim samouczku)
3. Dane podstawowe, które zawierają teraz zaktualizowane zmiany, są ponownie powiązane z elementem GridView

W ramach tej sekwencji kroków jest wyzwalana liczba zdarzeń, dzięki czemu możemy utworzyć programy obsługi zdarzeń w celu dodania logiki niestandardowej w razie potrzeby. Na przykład przed wykonaniem kroku 1 zdarzenie `RowUpdating` GridView zostanie wyzwolone. W tym momencie możemy anulować żądanie aktualizacji, jeśli wystąpił błąd sprawdzania poprawności. Po wywołaniu metody `Update()`, zdarzenie `Updating` elementu ObjectDataSource zostanie wyzwolone, co umożliwia dodanie lub dostosowanie wartości któregokolwiek z `UpdateParameters`. Po zakończeniu wykonywania metody obiektu bazowego elementu ObjectDataSource zostanie wywołane zdarzenie `Updated` elementu ObjectDataSource. Program obsługi zdarzeń dla zdarzenia `Updated` może sprawdzić szczegóły operacji aktualizacji, takie jak liczba wierszy, których dotyczył, oraz czy wystąpił wyjątek. Na koniec po kroku 2 zostanie wyzwolone zdarzenie `RowUpdated` GridView; Procedura obsługi zdarzeń dla tego zdarzenia może sprawdzać dodatkowe informacje o wykonywanej operacji aktualizacji.

Rysunek 1 przedstawia ten szereg zdarzeń i kroków podczas aktualizowania widoku GridView. Wzorzec zdarzenia na rysunku 1 nie jest unikatowy do aktualizacji za pomocą widoku GridView. Wstawianie, aktualizowanie lub usuwanie danych z obszaru GridView, DetailsView lub FormView ma taką samą sekwencję zdarzeń poprzedzających i po stronie, dla kontrolki sieci Web danych i elementu ObjectDataSource.

[![serię wyzwalania zdarzeń przed i po aktualizacji w przypadku aktualizowania danych w widoku GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**Rysunek 1**: szereg zdarzeń przed i po zdarzenia po aktualizacji danych w widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))

W tym samouczku sprawdzimy użycie tych zdarzeń, aby zwiększyć wbudowane funkcje wstawiania, aktualizowania i usuwania danych w kontrolkach sieci Web ASP.NET. Zobaczymy również, jak dostosować interfejs edycji, aby zaktualizować podzestaw pól produktu.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Krok 1: aktualizowanie pól`ProductName`i`UnitPrice`produktu

W interfejsach edycji z poprzedniego samouczka *wszystkie* pola produktu, które nie były tylko do odczytu, musiały zostać uwzględnione. Jeśli firma Microsoft musiała usunąć pole z `QuantityPerUnit` GridView-aware — podczas aktualizowania danych formant sieci Web danych nie ustawi `QuantityPerUnit` wartości `UpdateParameters` elementu ObjectDataSource. Element ObjectDataSource następnie przekaże wartość `Nothing` do metody `UpdateProduct` Business Logic Layer (LOGIKI biznesowej), która spowodowałaby zmianę kolumny `QuantityPerUnit` edytowanego rekordu bazy danych na wartość `NULL`. Analogicznie, jeśli wymagane pole, takie jak `ProductName`, zostanie usunięte z interfejsu edycji, aktualizacja zakończy się niepowodzeniem z powodu "*kolumna" ProductName "nie zezwala na*wyjątek" null ". Przyczyną takiego zachowania jest fakt, że element ObjectDataSource został skonfigurowany tak, aby wywoływać metodę `UpdateProduct` klasy `ProductsBLL`, która oczekuje parametru wejściowego dla każdego pola produktu. W związku z tym, kolekcja `UpdateParameters` elementu ObjectDataSource zawiera parametr dla każdego parametru wejściowego metody.

Jeśli chcemy udostępnić formant sieci Web danych, który umożliwia użytkownikowi końcowemu aktualizowanie podzestawu pól, należy programowo ustawić brakujące wartości `UpdateParameters` w programie obsługi zdarzeń `Updating` elementu ObjectDataSource lub utworzyć i wywołać metodę LOGIKI biznesowej, która oczekuje tylko podzestawu pól. Zapoznajmy się z tym ostatnim podejściem.

W celu utworzenia strony, która wyświetla tylko `ProductName` i `UnitPrice` pól w edytowalnym widoku GridView. Ten interfejs edytowania widoku GridView zezwoli użytkownikowi na aktualizowanie dwóch wyświetlanych pól, `ProductName` i `UnitPrice`. Ponieważ ten interfejs do edycji zawiera tylko podzestaw pól produktu, musimy utworzyć element ObjectDataSource, który używa metody `UpdateProduct` istniejącej i ma programowe wartości pola produkt ustawione programowo w jego `Updating` obsługi zdarzeń lub musimy utworzyć nową metodę LOGIKI biznesowej, która oczekuje tylko podzbioru pól zdefiniowanych w widoku GridView. Na potrzeby tego samouczka użyjemy tej ostatniej opcji i utworzysz Przeciążenie metody `UpdateProduct`, która przyjmuje tylko trzy parametry wejściowe: `productName`, `unitPrice`i `productID`:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

Podobnie jak oryginalna Metoda `UpdateProduct`, to Przeciążenie rozpoczyna się od sprawdzenia, czy w bazie danych istnieje produkt z określoną `ProductID`. W przeciwnym razie zwraca `False`, wskazując, że żądanie zaktualizowania informacji o produkcie nie powiodło się. W przeciwnym razie Aktualizacja istniejącego rekordu produktu `ProductName` i `UnitPrice` pól odpowiednio i zostanie zatwierdzona przez wywołanie metody `Update()` TableAdapter, przekazując w wystąpieniu `ProductsRow`.

Dodanie do naszej klasy `ProductsBLL` jest gotowe do utworzenia uproszczonego interfejsu GridView. Otwórz `DataModificationEvents.aspx` w folderze `EditInsertDelete` i Dodaj widok GridView do strony. Utwórz nowy element ObjectDataSource i skonfiguruj go tak, aby używał klasy `ProductsBLL` z mapowaniem metody `Select()` do `GetProducts` i `Update()` mapowania metody do `UpdateProduct` przeciążenia, które pobiera tylko `productName`, `unitPrice`i `productID` parametry wejściowe. Rysunek 2 przedstawia Kreatora tworzenia źródła danych podczas mapowania metody `Update()` elementu ObjectDataSource na nowe Przeciążenie metody `UpdateProduct` klasy `ProductsBLL`.

[![zmapować metody Update () elementu ObjectDataSource na nowe Przeciążenie UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**Rysunek 2**. zamapuj metodę `Update()` elementu ObjectDataSource na nowe `UpdateProduct` Przeciążenie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))

Ponieważ w naszym przykładzie początkowo potrzebna jest możliwość edytowania danych, ale nie do wstawiania ani usuwania rekordów, poświęć chwilę, aby jawnie wskazać, że `Insert()` i metody `Delete()` elementu ObjectDataSource nie powinny być mapowane do żadnej z metod klasy `ProductsBLL` przez przechodzenie do kart Wstawianie i usuwanie i wybieranie (brak) z listy rozwijanej.

[![wybierz (brak) z listy rozwijanej dla kart Wstawianie i usuwanie](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**Rysunek 3**. Wybierz pozycję (brak) z listy rozwijanej dla kart Wstawianie i usuwanie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))

Po ukończeniu tego kreatora zaznacz pole wyboru Włącz edycję w tagu inteligentnym GridView.

Po zakończeniu działania Kreatora tworzenia źródła danych i powiązania tego widoku GridView program Visual Studio utworzył składnię deklaratywną dla obu formantów. Przejdź do widoku źródła, aby sprawdzić deklaratywne znaczniki elementu ObjectDataSource, które pokazano poniżej:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

Ponieważ nie istnieją żadne mapowania dla `Insert()` i metod `Delete()` elementu ObjectDataSource, nie ma `InsertParameters` ani `DeleteParameters` sekcji. Ponadto ponieważ metoda `Update()` jest mapowana na Przeciążenie metody `UpdateProduct`, która akceptuje tylko trzy parametry wejściowe, sekcja `UpdateParameters` ma tylko trzy wystąpienia `Parameter`.

Zauważ, że właściwość `OldValuesParameterFormatString` elementu ObjectDataSource ma wartość `original_{0}`. Ta właściwość jest ustawiana automatycznie przez program Visual Studio w przypadku korzystania z Kreatora konfiguracji źródła danych. Jednak ponieważ nasze metody LOGIKI biznesowej nie oczekują, że oryginalna wartość `ProductID` do przekazania, usuń przypisanie tej właściwości całkowicie ze składni deklaracyjnej elementu ObjectDataSource.

> [!NOTE]
> Jeśli po prostu wyczyścisz wartość właściwości `OldValuesParameterFormatString` z okno Właściwości w widok Projekt, właściwość będzie nadal istnieć w składni deklaracyjnej, ale zostanie ona ustawiona na pusty ciąg. Usuń właściwość całkowicie ze składni deklaracyjnej lub z okno Właściwości ustaw wartość domyślną, `{0}`.

Chociaż element ObjectDataSource ma tylko `UpdateParameters` dla nazwy produktu, ceny i identyfikatora, Visual Studio dodał element BoundField lub CheckBoxField w widoku GridView dla każdego pola produktu.

[![GridView zawiera BoundField lub CheckBoxField dla każdego z pól produktu](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**Ilustracja 4**. element GridView zawiera BoundField lub CheckBoxField dla każdego z pól produktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))

Gdy użytkownik końcowy edytuje produkt i klika przycisk Aktualizuj, w widoku GridView są wyliczane pola, które nie są tylko do odczytu. Następnie ustawia wartość odpowiadającego parametru w kolekcji `UpdateParameters` elementu ObjectDataSource na wartość wprowadzoną przez użytkownika. Jeśli nie ma odpowiedniego parametru, GridView dodaje jeden do kolekcji. W związku z tym, jeśli widok GridView zawiera BoundFields i CheckBoxFields dla wszystkich pól produktu, element ObjectDataSource zakończy wywoływanie `UpdateProduct` przeciążenia, które przyjmuje wszystkie te parametry, pomimo faktu, że deklaratywne znaczniki elementu ObjectDataSource określają tylko trzy parametry wejściowe (patrz rysunek 5). Analogicznie, jeśli w widoku GridView istnieje kilka kombinacji pól nieprzeznaczonych tylko do odczytu, które nie odpowiadają parametrom wejściowym przeciążenia `UpdateProduct`, podczas próby zaktualizowania zostanie zgłoszony wyjątek.

[![GridView doda parametry do kolekcji UpdateParameters elementu ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**Rysunek 5**. widok GridView doda parametry do kolekcji `UpdateParameters` elementu ObjectDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))

Aby upewnić się, że element ObjectDataSource wywołuje Przeciążenie `UpdateProduct`, które przyjmuje tylko nazwę produktu, cenę i identyfikator, musimy ograniczyć widok GridView, aby zawierał pola edytowalne tylko dla `ProductName` i `UnitPrice`. Można to osiągnąć, usuwając inne BoundFields i CheckBoxFields, ustawiając te inne pola `ReadOnly` właściwość na `True`lub przez kilka kombinacji obu. Na potrzeby tego samouczka po prostu usuniemy wszystkie pola GridView, z wyjątkiem `ProductName` i `UnitPrice` BoundFields, po którym znaczniki deklaratywne GridView będą wyglądać następująco:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

Mimo że Przeciążenie `UpdateProduct` oczekuje trzech parametrów wejściowych, mamy tylko dwa BoundFields w naszym widoku GridView. Jest to spowodowane tym, że parametr wejściowy `productID` jest wartością klucza podstawowego i przekazaną przez wartość właściwości `DataKeyNames` edytowanego wiersza.

Nasz widok GridView, wraz z przeciążeniem `UpdateProduct`, umożliwia użytkownikowi edytowanie samej nazwy i ceny produktu bez utraty jakichkolwiek innych pól produktu.

[![interfejs pozwala edytować tylko nazwę produktu i cenę](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**Ilustracja 6**. interfejs pozwala edytować tylko nazwę produktu i cenę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))

> [!NOTE]
> Zgodnie z opisem w poprzednim samouczku ważne jest, aby włączyć stan widoku GridView (zachowanie domyślne). Jeśli ustawisz właściwość GridView s `EnableViewState` na `false`, zostanie uruchomione ryzyko przypadkowego usunięcia lub edycji rekordów przez współbieżnych użytkowników. Zobacz [Ostrzeżenie: problem współbieżności z elementem ASP.NET 2,0 gridviews/DetailsView/FormView, który obsługuje edytowanie i/lub usuwanie stanu widoku,](http://scottonwriting.net/sowblog/posts/10054.aspx) Aby uzyskać więcej informacji.

## <a name="improving-theunitpriceformatting"></a>Ulepszanie formatowania`UnitPrice`

Chociaż przykład GridView przedstawiony na rysunku 6 działa, pole `UnitPrice` nie jest w ogóle formatowane, co powoduje wyświetlenie cennika, który nie ma żadnych symboli walutowych i ma cztery miejsca dziesiętne. Aby zastosować formatowanie waluty dla nieedytowalnych wierszy, po prostu ustaw właściwość `DataFormatString` `UnitPrice` BoundField na `{0:c}`, a jej Właściwość `HtmlEncode` na `False`.

[![ustawić odpowiednio właściwości DataFormatString i HtmlEncode elementu CenaJednostkowa](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**Rysunek 7**: ustaw odpowiednio `DataFormatString` `UnitPrice`i właściwości `HtmlEncode` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))

W przypadku tej zmiany wiersze, które nie są edytowalne, są formatowane jako waluta. jednak edytowany wiersz nadal wyświetla wartość bez symbolu waluty i z czterema miejscami dziesiętnymi.

[![wiersze, które nie są edytowalne, są teraz sformatowane jako wartości walutowe](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**Ilustracja 8**. wiersze, które nie są edytowalne, są teraz sformatowane jako wartości walutowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))

Instrukcje formatowania określone we właściwości `DataFormatString` mogą być stosowane do interfejsu edycji przez ustawienie właściwości `ApplyFormatInEditMode` BoundField na `True` (wartość domyślna to `False`). Poświęć chwilę na ustawienie tej właściwości na `True`.

[![ustawić dla właściwości ApplyFormatInEditMode elementu CenaJednostkowa BoundField wartość true](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**Rysunek 9**. ustaw właściwość `ApplyFormatInEditMode` `UnitPrice` BoundField na `True` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))

W przypadku tej zmiany wartość `UnitPrice` wyświetlana w edytowanym wierszu jest również formatowana jako waluta.

[![wartość CenaJednostkowa edytowanego wiersza jest teraz sformatowana jako waluta](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**Rysunek 10**: wartość `UnitPrice` edytowanego wiersza jest teraz sformatowana jako waluta (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))

Jednak Aktualizacja produktu z symbolem waluty w polu tekstowym, takim jak $19,00 zgłasza `FormatException`. Gdy GridView próbuje przypisać wartości dostarczone przez użytkownika do kolekcji `UpdateParameters` elementu ObjectDataSource, nie można przekonwertować ciągu `UnitPrice` "$19,00" na `Decimal` wymagane przez parametr (patrz rysunek 11). Aby rozwiązać ten wyjątek, możemy utworzyć procedurę obsługi zdarzeń dla zdarzenia `RowUpdating` GridView i analizować `UnitPrice` dostarczone przez użytkownika jako `Decimal`sformatowane w walucie.

Zdarzenie `RowUpdating` GridView akceptuje jako drugi parametr obiekt typu [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), który zawiera słownik `NewValues` jako jedną z właściwości, która zawiera wartości dostarczone przez użytkownika, które są gotowe do przypisania do kolekcji `UpdateParameters` elementu ObjectDataSource. Można zastąpić istniejącą wartość `UnitPrice` w kolekcji `NewValues` wartością dziesiętną przeanalizowanej przy użyciu formatu waluty z następującymi wierszami kodu w programie obsługi zdarzeń `RowUpdating`:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

Jeśli użytkownik podał wartość `UnitPrice` (taką jak "$19,00"), ta wartość jest zastępowana wartością dziesiętną obliczoną przez [ułamek dziesiętny. Przeanalizuj](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx)analizę wartości jako walutę. Spowoduje to poprawne przeanalizowanie wartości dziesiętnej w przypadku wszelkich symboli walut, przecinków, cyfr dziesiętnych itd. i użycie [wyliczenia wyliczenia NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) w przestrzeni nazw [System. globalizacji](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) .

Rysunek 11 przedstawia problem spowodowany przez symbole walutowe w `UnitPrice`dostarczone przez użytkownika, a także sposób, w jaki można wykorzystać procedurę obsługi zdarzeń `RowUpdating` GridView, aby prawidłowo przeanalizować takie dane wejściowe.

[![wartość CenaJednostkowa edytowanego wiersza jest teraz sformatowana jako waluta](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**Rysunek 11**: wartość `UnitPrice` edytowanego wiersza jest teraz sformatowana jako waluta (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))

## <a name="step-2-prohibitingnull-unitprices"></a>Krok 2. Zabroń`NULL UnitPrices`

W czasie, gdy baza danych jest skonfigurowana tak, aby zezwalała na `NULL` wartości w kolumnie `UnitPrice` tabeli `Products`, możemy chcieć uniemożliwić użytkownikom odwiedzającym tę konkretną stronę Określanie `NULL` `UnitPrice` wartości. Oznacza to, że jeśli użytkownik nie będzie mógł wprowadzić wartości `UnitPrice` podczas edytowania wiersza produktu, zamiast zapisywać wyniki w bazie danych, chcemy wyświetlić komunikat informujący użytkownika, że na tej stronie wszystkie edytowane produkty muszą mieć określoną cenę.

Obiekt `GridViewUpdateEventArgs` przeszedł do programu obsługi zdarzeń `RowUpdating` GridView zawiera właściwość `Cancel`, która w przypadku wybrania wartości `True`kończy proces aktualizacji. Rozwińmy procedurę obsługi zdarzeń `RowUpdating`, aby ustawić `e.Cancel` `True` i wyświetlić komunikat wyjaśniający, dlaczego wartość `UnitPrice` w kolekcji `NewValues` ma wartość `Nothing`.

Zacznij od dodania kontrolki sieci Web etykieta do strony o nazwie `MustProvideUnitPriceMessage`. Ta kontrolka etykiety będzie wyświetlana, jeśli użytkownik nie będzie mógł określić wartości `UnitPrice` podczas aktualizowania produktu. Ustaw właściwość `Text` etykiety na "musisz podać cenę produktu". Utworzono również nową klasę CSS w `Styles.css` o nazwie `Warning` z następującą definicją:

[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

Na koniec ustaw właściwość `CssClass` etykiety na `Warning`. W tym momencie Projektant powinien pokazać komunikat ostrzegawczy w kolorze czerwonym, pogrubionym, kursywą, o bardzo dużym rozmiarze czcionki powyżej widoku GridView, jak pokazano na rysunku 12.

[![dodano etykietę powyżej widoku GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**Rysunek 12**. etykieta została dodana powyżej widoku GridView (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))

Domyślnie etykieta powinna być ukryta, więc ustaw jej Właściwość `Visible` na `False` w programie obsługi zdarzeń `Page_Load`:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

Jeśli użytkownik spróbuje zaktualizować produkt bez określania `UnitPrice`, chce anulować aktualizację i wyświetlić etykietę ostrzeżenia. Rozszerzanie programu obsługi zdarzeń `RowUpdating` GridView w następujący sposób:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

Jeśli użytkownik próbuje zapisać produkt bez określenia ceny, aktualizacja zostanie anulowana i zostanie wyświetlony komunikat pomocny. Podczas gdy baza danych (i logika biznesowa) zezwala na `NULL` `UnitPrice` s, ta strona ASP.NETa nie.

[![użytkownik nie może opuścić wartości pola CenaJednostkowa](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**Ilustracja 13**. użytkownik nie może opuścić `UnitPrice` pustego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))

Do tej pory zaobserwowano, jak użyć zdarzenia `RowUpdating` GridView, aby programowo zmienić wartości parametrów przypisanych do kolekcji `UpdateParameters` elementu ObjectDataSource oraz jak całkowicie anulować proces aktualizacji. Te koncepcje przenoszą się do kontrolek DetailsView i FormView, a także stosują się do wstawiania i usuwania.

Te zadania można także wykonać na poziomie elementu ObjectDataSource przy użyciu obsługi zdarzeń dla `Inserting`, `Updating`i `Deleting` zdarzeń. Te zdarzenia są wyzwalane przed wywołaniem skojarzonej metody obiektu bazowego i zapewnienia ostatecznej szansy do zmodyfikowania kolekcji parametrów wejściowych lub anulowania operacji. Procedury obsługi zdarzeń dla tych trzech zdarzeń są przesyłane do obiektu typu [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , który ma dwie właściwości zainteresowania:

- [Anuluj](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), co spowoduje, że jeśli zostanie ustawiona na `True`, anuluje wykonywane operacje
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), który jest kolekcją `InsertParameters`, `UpdateParameters`lub `DeleteParameters`, w zależności od tego, czy program obsługi zdarzeń jest przeznaczony dla zdarzenia `Inserting`, `Updating`lub `Deleting`

Aby zilustrować pracę z wartościami parametrów na poziomie elementu ObjectDataSource, na naszej stronie zostanie umieszczony element DetailsView, który umożliwi użytkownikom dodawanie nowego produktu. Ten DetailsView zostanie użyty w celu udostępnienia interfejsu umożliwiającego szybkie dodanie nowego produktu do bazy danych programu. Aby zachować spójny interfejs użytkownika podczas dodawania nowego produktu, Zezwól użytkownikowi na wprowadzanie wartości tylko dla `ProductName` i `UnitPrice` pól. Domyślnie te wartości, które nie są podane w interfejsie wstawiania w widoku DetailsView, zostaną ustawione na wartość `NULL` bazy danych. Można jednak użyć zdarzenia `Inserting` elementu ObjectDataSource, aby wprowadzić różne wartości domyślne, ponieważ wkrótce zobaczymy.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Krok 3. dostarczenie interfejsu umożliwiającego dodanie nowych produktów

Przeciągnij widok DetailsView z przybornika do projektanta nad GridView, wyczyść `Height` i `Width` właściwości i powiąż go z elementem ObjectDataSource już znajdującym się na stronie. Spowoduje to dodanie elementu BoundField lub CheckBoxField dla każdego z pól produktu. Ponieważ chcemy używać tego widoku DetailsView do dodawania nowych produktów, musimy sprawdzić opcję Włącz wstawianie z tagu inteligentnego. nie istnieje jednak taka opcja, ponieważ metoda `Insert()` elementu ObjectDataSource nie jest zamapowana na metodę w klasie `ProductsBLL` (odwołując się do ustawienia tego mapowania na (brak) podczas konfigurowania źródła danych, zobacz rysunek 3).

Aby skonfigurować element ObjectDataSource, wybierz łącze Konfiguruj źródło danych z taga inteligentnego, uruchamiając Kreatora. Pierwszy ekran umożliwia zmianę podstawowego obiektu, z którym jest powiązany element ObjectDataSource; Pozostaw wartość ustawioną na `ProductsBLL`. Na następnym ekranie są wyświetlane mapowania z metod elementu ObjectDataSource do obiektu źródłowego. Mimo że jawnie wskazano, że metody `Insert()` i `Delete()` nie powinny być mapowane do żadnej metody, jeśli przejdziesz do kart Wstawianie i usuwanie zobaczysz, że jest tam mapowanie. Wynika to z faktu, że `ProductsBLL``AddProduct` i `DeleteProduct` metody używają atrybutu `DataObjectMethodAttribute`, aby wskazać, że są to domyślne metody odpowiednio dla `Insert()` i `Delete()`. W związku z tym Kreator ObjectDataSource wybiera te za każdym razem, gdy zostanie uruchomiony Kreator, chyba że określono inną wartość.

Pozostaw metodę `Insert()` wskazującą metodę `AddProduct`, ale ponownie ustaw listę rozwijaną karty Usuń na (brak).

[![ustawić listę rozwijaną karty Wstaw na metodę addproduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**Ilustracja 14**. Ustawianie listy rozwijanej karty Wstaw na metodę `AddProduct` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))

[![ustawić listę rozwijaną karty Usuń na (brak)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**Ilustracja 15**. Ustaw listę rozwijaną karty Usuń na (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))

Po wprowadzeniu tych zmian składnia deklaracyjnego elementu ObjectDataSource zostanie rozwinięta w celu uwzględnienia kolekcji `InsertParameters`, jak pokazano poniżej:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

Ponowne uruchomienie Kreatora dodało `OldValuesParameterFormatString` właściwości. Poświęć chwilę na wyczyszczenie tej właściwości, ustawiając ją na wartość domyślną (`{0}`) lub usuwając ją całkowicie ze składni deklaracyjnej.

Za pomocą elementu ObjectDataSource dostarczającego możliwości wstawiania, tag inteligentny DetailsView będzie teraz zawierać pole wyboru Włącz wstawianie; Wróć do projektanta i zaznacz tę opcję. Następnie dostosowanie w górę widoku DetailsView, aby miał tylko dwa BoundFields-`ProductName` i `UnitPrice`-i CommandField. W tym momencie składnia deklaratywna DetailsView powinna wyglądać następująco:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

Na rysunku 16 przedstawiono Tę stronę w przeglądarce w tym momencie. Jak widać, w widoku DetailsView wyświetlana jest nazwa i cena pierwszego produktu (Chai). Jednak jest to interfejs wstawiania, który umożliwia użytkownikowi szybkie dodawanie nowego produktu do bazy danych programu.

[![DetailsView jest obecnie renderowany w trybie tylko do odczytu](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**Ilustracja 16**. widok DetailsView jest obecnie renderowany w trybie tylko do odczytu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))

Aby można było wyświetlić widok DetailsView w jego trybie wstawiania, należy ustawić właściwość `DefaultMode` na `Inserting`. Renderuje to DetailsView w trybie wstawiania podczas pierwszej wizyty i utrzymuje po wstawieniu nowego rekordu. Jak pokazano na rysunku 17, taka DetailsView udostępnia szybki interfejs do dodawania nowego rekordu.

[![DetailsView udostępnia interfejs umożliwiający szybkie dodanie nowego produktu](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**Ilustracja 17**. widok DetailsView udostępnia interfejs umożliwiający szybkie dodanie nowego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))

Gdy użytkownik wprowadzi nazwę produktu i cenę (na przykład "wody XYZ" i 1,99, jak na rysunku 17) i klika pozycję Wstaw, ogłaszanie zwrotne zostanie rozpoczęte i rozpocznie się Wstawianie przepływu pracy, culminating w nowym rekordzie produktu do bazy danych. Widok DetailsView zachowuje swój interfejs, a GridView jest automatycznie ponownie powiązane ze źródłem danych w celu uwzględnienia nowego produktu, jak pokazano na rysunku 18.

![Produkt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**Ilustracja 18**. produkt "woda XYZ" została dodana do bazy danych

Chociaż widok GridView na rysunku 18 nie pokazuje go, pola produktu, które nie są w interfejsie DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`i tak dalej, są przypisane `NULL` wartości bazy danych. Można to sprawdzić, wykonując następujące czynności:

1. Przejdź do Eksplorator serwera w programie Visual Studio
2. Rozszerzanie węzła bazy danych `NORTHWND.MDF`
3. Kliknij prawym przyciskiem myszy węzeł tabeli bazy danych `Products`
4. Wybierz pozycję Pokaż dane tabeli

Spowoduje to wyświetlenie listy wszystkich rekordów w tabeli `Products`. Jak pokazano na rysunku 19, wszystkie kolumny nowego produktu inne niż `ProductID`, `ProductName`i `UnitPrice` mają `NULL` wartości.

[![pola produktu, które nie zostały podane w widoku DetailsView, są przypisane wartości NULL](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**Ilustracja 19**. pola produktu, które nie zostały podane w widoku DetailsView, są przypisane do wartości `NULL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))

Firma Microsoft może chcieć podać wartość domyślną inną niż `NULL` dla co najmniej jednej z tych wartości kolumny, ponieważ `NULL` nie jest najlepszą opcją domyślną lub ponieważ sama kolumna bazy danych nie zezwala na `NULL` s. Aby to osiągnąć, można programowo ustawić wartości parametrów kolekcji `InputParameters` DetailsView. To przypisanie można wykonać w procedurze obsługi zdarzeń dla zdarzenia `ItemInserting` DetailsView lub zdarzenia `Inserting` elementu ObjectDataSource. Ze względu na to, że już korzystamy z zdarzeń przed i na poziomie na poziomie formantu sieci Web danych, poszukajmy w tym czasie za pomocą zdarzeń elementu ObjectDataSource.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Krok 4. Przypisywanie wartości do`CategoryID`i`SupplierID`parametrów

Na potrzeby tego samouczka Wyobraź sobie, że dla naszej aplikacji podczas dodawania nowego produktu za pomocą tego interfejsu należy przypisać `CategoryID` i `SupplierID` wartość 1. Jak wspomniano wcześniej, element ObjectDataSource ma parę zdarzeń przed i na poziomie, które są wyzwalane podczas procesu modyfikowania danych. Po wywołaniu metody `Insert()` element ObjectDataSource najpierw wywołuje swoje zdarzenie `Inserting`, a następnie wywołuje metodę, do której zamapowana została Metoda `Insert()` i na koniec wywołuje zdarzenie `Inserted`. Program obsługi zdarzeń `Inserting` zapewnia nam jedną ostatnią szansę na dostosowanie parametrów wejściowych lub anulowanie operacji.

> [!NOTE]
> W rzeczywistej aplikacji prawdopodobnie chcesz zezwolić użytkownikowi na określenie kategorii i dostawcy albo wybrać dla nich tę wartość na podstawie niektórych kryteriów lub logiki biznesowej (zamiast odślepej próby wybrania identyfikatora 1). Niezależnie od tego przykład ilustruje sposób programowo ustawić wartość parametru wejściowego z zdarzenia wstępnego elementu ObjectDataSource.

Poświęć chwilę, aby utworzyć procedurę obsługi zdarzeń dla zdarzenia `Inserting` elementu ObjectDataSource. Zwróć uwagę, że drugi parametr wejściowy programu obsługi zdarzeń jest obiektem typu `ObjectDataSourceMethodEventArgs`, który ma właściwość do uzyskiwania dostępu do kolekcji parametrów (`InputParameters`) i właściwość, aby anulować operację (`Cancel`).

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

W tym momencie Właściwość `InputParameters` zawiera kolekcję `InsertParameters` elementu ObjectDataSource z wartościami przypisanymi z widoku DetailsView. Aby zmienić wartość jednego z tych parametrów, wystarczy użyć: `e.InputParameters("paramName") = value`. W związku z tym, aby ustawić `CategoryID` i `SupplierID` na wartości 1, Dostosuj procedurę obsługi zdarzeń `Inserting` w taki sposób, aby wyglądała następująco:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

Tym razem, gdy dodajesz nowy produkt (na przykład do wykrycia Acme), `CategoryID` i `SupplierID` kolumny nowego produktu są ustawione na 1 (Zobacz Rysunek 20).

[![nowe produkty mają teraz wartości IDKategorii i IDDostawcy ustawione na 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**Ilustracja 20**. nowe produkty mają teraz `CategoryID` i `SupplierID` wartości ustawione na 1 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))

## <a name="summary"></a>Podsumowanie

Podczas edytowania, wstawiania i usuwania, zarówno kontrolka sieci Web danych, jak i element ObjectDataSource postępują przez wiele zdarzeń przed i na poziomie. W tym samouczku zbadamy zdarzenia na poziomie wstępnym i pokazano, jak za ich pomocą dostosować parametry wejściowe lub anulować operację modyfikacji danych całkowicie zarówno z poziomu formantu sieci Web danych, jak i z zdarzeń elementu ObjectDataSource. W następnym samouczku przedstawiono tworzenie i używanie programów obsługi zdarzeń dla zdarzeń na poziomie.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Jackie Goor i Liz Shulok. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [dalej](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
