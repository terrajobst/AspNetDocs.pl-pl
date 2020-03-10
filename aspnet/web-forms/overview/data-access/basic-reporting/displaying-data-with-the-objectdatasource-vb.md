---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Wyświetlanie danych za pomocą elementu ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Ten samouczek przegląda kontrolkę ObjectDataSource przy użyciu tej kontrolki. można powiązać dane pobierane z LOGIKI biznesowej utworzonych w poprzednim samouczku bez HAVI...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 754188352cbfb08e610027f5b7890a32bd88ae26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597049"
---
# <a name="displaying-data-with-the-objectdatasource-vb"></a>Wyświetlanie danych za pomocą kontrolki ObjectDataSource (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) lub [Pobierz plik PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Ten samouczek przegląda kontrolkę ObjectDataSource przy użyciu tej kontrolki. można powiązać dane pobierane z LOGIKI biznesowej utworzonych w poprzednim samouczku bez konieczności pisania wiersza kodu.

## <a name="introduction"></a>Wprowadzenie

Dzięki naszej architekturze aplikacji i układowi strony witryny sieci Web jesteśmy gotowi do rozpoczęcia eksplorowania sposobu wykonywania różnych typowych zadań związanych z danymi i raportowaniem. W poprzednich samouczkach zaobserwowano, jak programowo powiązać dane z DAL i LOGIKI biznesowej do kontrolki sieci Web danych na stronie ASP.NET. Ta składnia przypisuje Właściwość `DataSource` formantu sieci Web danych do danych do wyświetlenia, a następnie wywołująca metodę `DataBind()` formantu była wzorcem używanym w aplikacjach ASP.NET 1. x i mogą być nadal używane w aplikacjach 2,0. Jednak nowe kontrolki źródła danych w programie ASP.NET 2.0 oferują deklaratywny sposób pracy z danymi. Za pomocą tych kontrolek można powiązać dane pobierane z LOGIKI biznesowej utworzonych w poprzednim samouczku bez konieczności pisania wiersza kodu.

ASP.NET 2,0 jest dostarczany z pięcioma wbudowanymi formantami źródła danych [kontrolki SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)i [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) , chociaż można utworzyć własne [niestandardowe kontrolki źródła danych](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), jeśli jest to konieczne. Ponieważ opracowano architekturę naszej aplikacji samouczka, użyjemy elementu ObjectDataSource względem naszych klas LOGIKI biznesowej.

![ASP.NET 2,0 obejmuje pięć wbudowanych kontrolek źródła danych](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Rysunek 1**: ASP.NET 2,0 zawiera pięć wbudowanych kontrolek źródła danych

Element ObjectDataSource służy jako serwer proxy do pracy z innym obiektem. Aby skonfigurować element ObjectDataSource, należy określić ten obiekt źródłowy i sposób mapowania metod na `Select`, `Insert`, `Update`i `Delete` elementu ObjectDataSource. Po określeniu tego obiektu bazowego i jego metodach zamapowanych na element ObjectDataSource można powiązać ten element ObjectDataSource z kontrolką sieci Web danych. ASP.NET dostarcza z wieloma kontrolkami sieci Web danych, w tym GridView, DetailsView, RadioButtonList i DropDownList, między innymi. Podczas cyklu życia strony kontrolka sieci Web danych może potrzebować dostępu do danych, które są związane z tym, co zostanie wykonane przez wywołanie metody `Select` elementu ObjectDataSource; Jeśli formant sieci Web danych obsługuje wstawianie, aktualizowanie lub usuwanie, wywołania mogą być wykonywane na `Insert`metodach, `Update`lub `Delete` elementu ObjectDataSource. Te wywołania są następnie kierowane przez element ObjectDataSource do odpowiednich metod obiektu źródłowego, jak pokazano na poniższym diagramie.

[![element ObjectDataSource służy jako serwer proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Rysunek 2**. Element ObjectDataSource służy jako serwer proxy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image4.png))

Chociaż element ObjectDataSource może służyć do wywoływania metod wstawiania, aktualizowania lub usuwania danych, należy skupić się na zwracaniu danych. w przyszłości samouczki zostaną omówione przy użyciu kontrolek typu ObjectDataSource i Data sieci Web, które modyfikują dane.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Krok 1. Dodawanie i Konfigurowanie kontrolki ObjectDataSource

Aby rozpocząć, Otwórz stronę `SimpleDisplay.aspx` w folderze `BasicReporting`, przejdź do widok Projekt, a następnie przeciągnij kontrolkę ObjectDataSource z przybornika na powierzchnię projektu strony. Element ObjectDataSource pojawia się jako szare pole na powierzchni projektowej, ponieważ nie produkuje żadnego znacznika; po prostu uzyskuje dostęp do danych przez wywołanie metody z określonego obiektu. Dane zwrócone przez element ObjectDataSource mogą być wyświetlane przez formant sieci Web danych, taki jak GridView, DetailsView, FormView i tak dalej.

> [!NOTE]
> Alternatywnie możesz najpierw dodać formant sieci Web danych do strony, a następnie z jej tagu inteligentnego wybrać opcję &lt;nowe źródło danych&gt; z listy rozwijanej.

Aby określić obiekt źródłowy elementu ObjectDataSource i sposób, w jaki metody tego obiektu są mapowane na element ObjectDataSource, kliknij link Konfiguruj źródło danych z tagu inteligentnego elementu ObjectDataSource.

[![kliknij link Konfiguruj źródło danych z tagu inteligentnego](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Rysunek 3**. Kliknij link Konfiguruj źródło danych z tagu inteligentnego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image7.png))

Spowoduje to wyświetlenie Kreatora konfiguracji źródła danych. Najpierw należy określić obiekt, z którym ma współpracować element ObjectDataSource. Jeśli zaznaczone jest pole wyboru "Pokaż tylko składniki danych", na liście rozwijanej na tym ekranie są wyświetlane tylko te obiekty, które zostały uzupełnione atrybutem `DataObject`. Obecnie nasza lista zawiera TableAdapters w określonym zestawie danych i klasy LOGIKI biznesowej utworzone w poprzednim samouczku. Jeśli nie pamiętasz dodać atrybutu `DataObject` do klas warstwy logiki biznesowej, nie będą one widoczne na tej liście. W takim przypadku Usuń zaznaczenie pola wyboru "Pokaż tylko składniki danych", aby wyświetlić wszystkie obiekty, które powinny zawierać klasy LOGIKI biznesowej (wraz z innymi klasami w określonym zestawie danych, tabelami, danymi i tak dalej).

Na pierwszym ekranie wybierz z listy rozwijanej klasę `ProductsBLL` i kliknij przycisk Dalej.

[![określić obiektu do użycia z kontrolką ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Ilustracja 4**. Określanie obiektu do użycia z kontrolką ObjectDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image10.png))

Na następnym ekranie kreatora zostanie wyświetlony komunikat z prośbą o wybranie metody, którą element ObjectDataSource powinien wywołać. Lista rozwijana zawiera te metody, które zwracają dane w obiekcie wybranym z poprzedniego ekranu. Tutaj widzimy `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`i `GetProductsBySupplierID`. Wybierz z listy rozwijanej metodę `GetProducts` i kliknij przycisk Zakończ (Jeśli dodano `DataObjectMethodAttribute` do metod `ProductBLL`, jak pokazano w poprzednim samouczku, ta opcja zostanie wybrana domyślnie).

[![wybrać metodę zwracania danych z karty Wybieranie](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Rysunek 5**. Wybierz metodę zwracania danych z karty wybierz ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image13.png))

## <a name="configure-the-objectdatasource-manually"></a>Ręczne konfigurowanie elementu ObjectDataSource

Kreator konfiguracji źródła danych elementu ObjectDataSource umożliwia szybkie określenie obiektu, który używa, i kojarzenie metod wywoływanych przez obiekt. Można jednak skonfigurować element ObjectDataSource za pomocą jego właściwości, za pomocą okno Właściwości lub bezpośrednio w znacznikach deklaratywnych. Po prostu ustaw właściwość `TypeName` na typ obiektu źródłowego, który ma być używany, i `SelectMethod` do metody do wywołania podczas pobierania danych.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Nawet jeśli wolisz Kreatora konfigurowania źródła danych, może się okazać, że trzeba ręcznie skonfigurować element ObjectDataSource, ponieważ Kreator wyświetla tylko klasy utworzone przez dewelopera. Jeśli chcesz powiązać element ObjectDataSource z klasą w .NET Framework, takich jak [Klasa Membership](https://msdn.microsoft.com/library/system.web.security.membership.aspx), aby uzyskać dostęp do informacji o koncie użytkownika lub [Klasa katalogu](https://msdn.microsoft.com/library/system.io.directory.aspx) do pracy z informacjami o systemie plików, musisz ręcznie ustawić właściwości elementu ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Krok 2. Dodawanie kontrolki sieci Web danych i powiązywanie jej z elementem ObjectDataSource

Gdy element ObjectDataSource zostanie dodany do strony i skonfigurowany, będzie można dodać do strony kontrolki sieci Web danych, aby wyświetlić dane zwrócone przez metodę `Select` elementu ObjectDataSource. Każda kontrolka sieci Web danych może być powiązana z elementem ObjectDataSource; Przyjrzyjmy się wyświetlaniu danych elementu ObjectDataSource w widoku GridView, DetailsView i FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Powiązywanie widoku GridView z elementem ObjectDataSource

Dodaj kontrolkę GridView z przybornika do powierzchni projektowej `SimpleDisplay.aspx`. W tagu inteligentnym GridView Wybierz kontrolkę ObjectDataSource dodaną w kroku 1. Spowoduje to automatyczne utworzenie BoundField w widoku GridView dla każdej właściwości zwróconej przez dane z metody `Select` elementu ObjectDataSource (tj. właściwości zdefiniowanych przez produkty DataTable).

[![element GridView został dodany do strony i powiązany z elementem ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Ilustracja 6**. widok GridView został dodany do strony i powiązany z elementem ObjectDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image16.png))

Następnie można dostosować, zmienić rozmieszczenie lub usunąć BoundFields GridView, klikając opcję Edytuj kolumny w tagu inteligentnym.

[![zarządzać BoundFieldsą widoku GridView za pomocą okna dialogowego Edytowanie kolumn](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Rysunek 7**. Zarządzanie BoundFieldsami w widoku GridView za pomocą okna dialogowego Edytowanie kolumn ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image19.png))

Poświęć chwilę, aby zmodyfikować BoundFields GridView, usuwając `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`i `ReorderLevel` BoundFields. Po prostu wybierz pozycję BoundField z listy w lewym dolnym rogu, a następnie kliknij przycisk Usuń (czerwony symbol X), aby je usunąć. Następnie Zmień rozmieszczenie BoundFields tak, aby `CategoryName` i `SupplierName` BoundFields poprzedzać `UnitPrice` BoundField, wybierając te BoundFields i klikając strzałkę w górę. Ustaw właściwości `HeaderText` pozostałej BoundFields na odpowiednio `Products`, `Category`, `Supplier`i `Price`. Następnie `Price` BoundField sformatowana jako waluta, ustawiając właściwość `HtmlEncode` BoundField na false i jej Właściwość `DataFormatString` na `{0:c}`. Na koniec Wyrównaj `Price` do prawej `Discontinued` i pola wyboru w centrum za pośrednictwem właściwości `ItemStyle`/`HorizontalAlign`.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

[![BoundFields GridView został dostosowany](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Ilustracja 8**. BoundFields GridView został dostosowany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image22.png))

## <a name="using-themes-for-a-consistent-look"></a>Używanie motywów dla spójnego wyglądu

Te samouczki dążą do usuwania wszelkich ustawień stylu na poziomie kontroli zamiast używania kaskadowych arkuszy stylów zdefiniowanych w pliku zewnętrznym, gdy jest to możliwe. Plik `Styles.css` zawiera klasy CSS `DataWebControlStyle`, `HeaderStyle`, `RowStyle`i `AlternatingRowStyle`, które powinny być używane do dyktowania wyglądu formantów sieci Web danych używanych w tych samouczkach. Aby to osiągnąć, można ustawić właściwość `CssClass` widoku GridView na `DataWebControlStyle`, a jej właściwości `HeaderStyle`, `RowStyle`i `AlternatingRowStyle` `CssClass` odpowiednio.

Jeśli ustawimy te `CssClass` właściwości w formancie sieci Web, musimy pamiętać, aby jawnie ustawić te wartości właściwości dla każdej kontrolki sieci Web, która została dodana do naszych samouczków. Bardziej zarządzanym podejściem jest zdefiniowanie domyślnych właściwości związanych z CSS dla kontrolek GridView, DetailsView i FormView przy użyciu motywu. Motyw to zbiór ustawień właściwości, obrazów i klas CSS na poziomie formantu, które mogą być stosowane do stron w witrynie w celu wymuszenia wspólnego wyglądu i działania.

Nasz motyw nie będzie zawierać żadnych obrazów ani plików CSS (arkusz stylów zostanie pozostawiony `Styles.css` tak, jak jest zdefiniowany w folderze głównym aplikacji sieci Web), ale będzie zawierać dwie karnacje. Karnacja to plik, który definiuje domyślne właściwości kontrolki sieci Web. W odniesieniu do kontrolek GridView i DetailsView zawieramy plik skórki, wskazujący domyślne właściwości dotyczące `CssClass`.

Zacznij od dodania nowego pliku skórki do projektu o nazwie `GridView.skin`, klikając prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierając polecenie Dodaj nowy element.

[![dodać pliku skórki o nazwie GridView. skórka](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Ilustracja 9**. Dodawanie pliku skórki o nazwie `GridView.skin` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image25.png))

Pliki skórki muszą być umieszczone w motywie, które znajdują się w folderze `App_Themes`. Ponieważ nie mamy jeszcze takiego folderu, program Visual Studio nie będzie mógł go utworzyć dla nas podczas dodawania pierwszej karnacji. Kliknij przycisk tak, aby utworzyć folder `App_Theme` i umieścić w nim nowy plik `GridView.skin`.

[![Pozwól programowi Visual Studio utworzyć folder App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Ilustracja 10**. Pozwól programowi Visual Studio utworzyć folder `App_Theme` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image28.png))

Spowoduje to utworzenie nowego motywu w folderze `App_Themes` o nazwie GridView z `GridView.skin`pliku skórki.

![Motyw GridView został dodany do folderu App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Ilustracja 11**. motyw GridView został dodany do folderu `App_Theme`

Zmień nazwę motywu GridView na DataWebControls (kliknij prawym przyciskiem myszy folder GridView w folderze `App_Theme` i wybierz polecenie Zmień nazwę). Następnie wprowadź następujące znaczniki do pliku `GridView.skin`:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Definiuje ona domyślne właściwości dla `CssClass`właściwości dla każdego elementu GridView na każdej stronie korzystającej z motywu DataWebControls. Dodajmy kolejną skórę dla widoku DetailsView, kontrolki sieci Web danych, której będziemy używać wkrótce. Dodaj nową skórkę do motywu DataWebControls o nazwie `DetailsView.skin` i Dodaj następujące znaczniki:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Po zdefiniowaniu naszego motywu ostatni krok polega na zastosowaniu motywu do naszej strony ASP.NET. Motyw można zastosować w odniesieniu do strony lub dla wszystkich stron w witrynie sieci Web. Użyjemy tego motywu dla wszystkich stron w witrynie sieci Web. Aby to osiągnąć, Dodaj następujące znaczniki do sekcji `<system.web>` `Web.config`:

[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

To wszystko! Ustawienie `styleSheetTheme` wskazuje, że właściwości określone w motywie *nie* powinny przesłaniać właściwości określonych na poziomie formantu. Aby określić, że ustawienia motywu powinny Trump ustawienia kontroli, Użyj atrybutu `theme` zamiast `styleSheetTheme`; Niestety, ustawienia motywów nie są wyświetlane w programie Visual Studio widok Projekt. Więcej informacji na temat motywów i skórek można znaleźć w tematach [ASP.NET motywy i karnacje](https://msdn.microsoft.com/library/ykzx33wh.aspx) oraz [Style po stronie serwera za pomocą motywów](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) . Zobacz [How to: Apply ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) Themes, aby uzyskać więcej informacji na temat konfigurowania strony do korzystania z motywu.

[![GridView wyświetla nazwę produktu, kategorię, dostawcę, cenę i wycofane informacje](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Ilustracja 12**. w widoku GridView wyświetlana jest nazwa produktu, Kategoria, dostawca, Cena i wycofane informacje ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image32.png))

## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Wyświetlanie jednego rekordu w czasie w widoku DetailsView

Widok GridView wyświetla jeden wiersz dla każdego rekordu zwróconego przez kontrolkę źródła danych, z którą jest powiązany. Czasami jednak może być konieczne wyświetlenie pojedynczego rekordu lub tylko jednego rekordu w danym momencie. [Formant DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) oferuje tę funkcję, renderowanie jako `<table>` HTML z dwiema kolumnami i jednym wierszem dla każdej kolumny lub właściwości powiązanej z kontrolką. Widok DetailsView można traktować jako GridView z pojedynczym rekordem obróconym o 90 stopni.

Zacznij od dodania kontrolki DetailsView *nad* elementem GridView w `SimpleDisplay.aspx`. Następnie powiąż go z tą samą kontrolką ObjectDataSource co widok GridView. Podobnie jak w przypadku widoku GridView, BoundField zostanie dodana do widoku DetailsView dla każdej właściwości w obiekcie zwracanym przez metodę `Select` elementu ObjectDataSource. Jedyną różnicą jest to, że BoundFields DetailsView są rozłożone w poziomie, a nie w pionie.

[![dodać widoku DetailsView do strony i powiązać ją z elementem ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Ilustracja 13**. Dodaj widok DetailsView do strony i powiąż go z elementem ObjectDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image35.png))

Podobnie jak w widoku GridView, BoundFields można dostosować, aby zapewnić bardziej dostosowany sposób wyświetlania danych zwracanych przez element ObjectDataSource. Rysunek 14 przedstawia widok DetailsView, gdy jego właściwości BoundFields i `CssClass` zostały skonfigurowane tak, aby były podobne do przykładu GridView.

[![widoku DetailsView pokazuje pojedynczy rekord](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Ilustracja 14**. widok DetailsView pokazuje pojedynczy rekord ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image38.png))

Należy pamiętać, że w widoku DetailsView jest wyświetlany tylko pierwszy rekord zwrócony przez źródło danych. Aby umożliwić użytkownikowi jednoczesne przechodzenie między wszystkimi rekordami, należy włączyć stronicowanie w widoku DetailsView. Aby to zrobić, Wróć do programu Visual Studio i zaznacz pole wyboru Włącz stronicowanie w tagu inteligentnym DetailsView.

[![Włącz stronicowanie w kontrolce DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Ilustracja 15**. Włączanie stronicowania w kontrolce DetailsView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image41.png))

[![ze stronicowaniem włączonym, widok DetailsView umożliwia użytkownikowi wyświetlanie dowolnych produktów](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Ilustracja 16**. po włączeniu stronicowania widok DetailsView umożliwia użytkownikowi wyświetlanie dowolnego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image44.png))

Dowiesz się więcej o stronicowaniu w przyszłych samouczkach.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Bardziej elastyczny układ wyświetlania jednego rekordu w danym momencie

Widok DetailsView jest bardzo sztywny w sposobie wyświetlania każdego rekordu zwróconego z elementu ObjectDataSource. Firma Microsoft może chcieć bardziej elastyczne przeglądanie danych. Na przykład zamiast wyświetlania nazwy produktu, kategorii, dostawcy, ceny i nieprawidłowych informacji każdego w osobnym wierszu możemy chcieć pokazać nazwę produktu i cenę w `<h4>` nagłówku, a informacje o kategorii i dostawcy są wyświetlane poniżej nazwy i ceny przy użyciu mniejszego rozmiaru czcionki. Firma Microsoft może nie uważać, aby wyświetlić nazwy właściwości (produkt, Kategoria i tak dalej) obok wartości.

[Kontrolka FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) zapewnia ten poziom dostosowywania. Zamiast używać pól (takich jak GridView i DetailsView), FormView korzysta z szablonów, które umożliwiają mieszanie kombinacji kontrolek sieci Web, statycznego kodu HTML i [składni wiązania z danymi](http://www.15seconds.com/issue/040630.htm). Jeśli znasz formant wzmacniak od ASP.NET 1. x, Możesz pomyśleć o FormView jako wzmacniak do wyświetlania pojedynczego rekordu.

Dodaj kontrolkę FormView do powierzchni projektowej strony `SimpleDisplay.aspx`. Początkowo formant FormView jest wyświetlany w postaci szarego bloku, informując nas, że musimy podać co najmniej `ItemTemplate`formantu.

[![FormView musi zawierać element ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Ilustracja 17**. formant FormView musi zawierać `ItemTemplate` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image47.png))

Można powiązać FormView bezpośrednio z kontrolką źródła danych za pomocą tagu inteligentnego FormView, co spowoduje automatyczne utworzenie domyślnego `ItemTemplate` (wraz z `EditItemTemplate` i `InsertItemTemplate`, jeśli ustawiono właściwości `InsertMethod` i `UpdateMethod` formantu ObjectDataSource). Jednak w tym przykładzie powiążemy dane z formantem FormView i ręcznie określisz jego `ItemTemplate`. Zacznij od ustawienia właściwości `DataSourceID` FormView do `ID` formantu ObjectDataSource, `ObjectDataSource1`. Następnie utwórz `ItemTemplate` w taki sposób, aby wyświetlał nazwę i cenę produktu w elemencie `<h4>`, a nazwy kategorii i spedytora poniżej, które mają mniejszy rozmiar czcionki.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]

[![pierwszy produkt (Chai) jest wyświetlany w formacie niestandardowym](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Ilustracja 18**. pierwszy produkt (Chai) jest wyświetlany w formacie niestandardowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image50.png))

`<%# Eval(propertyName) %>` jest składnią wiązania danych. Metoda `Eval` zwraca wartość określonej właściwości dla bieżącego obiektu, który jest powiązany z kontrolką FormView. Zapoznaj się z artykułem [uproszczonej i rozszerzonej składni powiązań danych w programie ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm) , aby uzyskać więcej informacji na temat elementów i wykroczeń wiązania z danymi.

Podobnie jak w widoku DetailsView, FormView pokazuje tylko pierwszy rekord zwrócony z elementu ObjectDataSource. Można włączyć stronicowanie w widoku FormView, aby umożliwić odwiedzającym przechodzenie przez produkty po jednej naraz.

## <a name="summary"></a>Podsumowanie

Uzyskiwanie dostępu do danych z warstwy logiki biznesowej i wyświetlanie ich z niej nie jest możliwe bez konieczności pisania wiersza kodu dzięki formantowi ObjectDataSourcea ASP.NET 2.0. Element ObjectDataSource wywołuje określoną metodę klasy i zwraca wyniki. Te wyniki można wyświetlić w kontrolce sieci Web danych, która jest powiązana z elementem ObjectDataSource. W tym samouczku pokazano, jak powiązać kontrolki GridView, DetailsView i FormView z elementami ObjectDataSource.

Do tej pory widzimy, jak używać elementu ObjectDataSource do wywołania metody bez parametrów, ale co zrobić, jeśli chcemy wywołać metodę, która oczekuje parametrów wejściowych, takich jak `GetProductsByCategoryID(categoryID)`klasy `ProductBLL`? W celu wywołania metody, która oczekuje co najmniej jednego parametru, należy skonfigurować element ObjectDataSource, aby określić wartości tych parametrów. Zobaczymy, jak to zrobić w [następnym samouczku](declarative-parameters-vb.md).

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Utwórz własne kontrolki źródła danych](https://msdn.microsoft.com/library/ms364049.aspx)
- [Przykłady widoku GridView dla ASP.NET 2,0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Uproszczona i rozszerzona składnia powiązań danych w ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm)
- [Motywy w programie ASP.NET 2,0](http://www.odetocode.com/Articles/423.aspx)
- [Style po stronie serwera przy użyciu motywów](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Jak zastosować motywy ASP.NET programistycznie](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [dalej](declarative-parameters-vb.md)
