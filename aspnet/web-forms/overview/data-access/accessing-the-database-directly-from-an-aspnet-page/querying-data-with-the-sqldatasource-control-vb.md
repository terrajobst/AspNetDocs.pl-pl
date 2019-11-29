---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: Wykonywanie zapytań o dane za pomocą kontrolki kontrolki SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: W poprzednich samouczkach użyto kontrolki ObjectDataSource, aby całkowicie oddzielić warstwę prezentacji od warstwy dostępu do danych. Trwa Rozpoczynanie pracy z tym samouczekem...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 199ddb46e877c3a0937672d33241a240660684da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606067"
---
# <a name="querying-data-with-the-sqldatasource-control-vb"></a>Wykonywanie zapytań o dane przy użyciu kontrolki SqlDataSource (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) lub [Pobierz plik PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> W poprzednich samouczkach użyto kontrolki ObjectDataSource, aby całkowicie oddzielić warstwę prezentacji od warstwy dostępu do danych. Począwszy od tego samouczka, dowiesz się, w jaki sposób formant kontrolki SqlDataSource może być używany w przypadku prostych aplikacji, które nie wymagają takiego dokładnego oddzielenia informacji o prezentacjach i dostępie do danych.

## <a name="introduction"></a>Wprowadzenie

Wszystkie samouczki, które chcielibyśmy zbadać, do tej pory używały architektury warstwowej, która składa się z warstwy prezentacji, logiki biznesowej i dostępu do danych. Warstwa dostępu do danych (DAL) została spreparowana w pierwszym samouczku ([Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md)) i warstwy logiki biznesowej w drugim ([Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-vb.md)). Począwszy od [wyświetlanych danych za pomocą](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) samouczka elementu ObjectDataSource, przedstawiono sposób użycia nowej kontrolki objectdatasource ASP.NET 2,0 s do deklaratywnego interfejsu z architekturą z warstwy prezentacji.

We wszystkich samouczkach do pracy z danymi można także korzystać z danych, wstawiać, aktualizować i usuwać dane bazy danych bezpośrednio ze strony ASP.NET, pomijając architekturę. Spowoduje to umieszczenie określonych zapytań bazy danych i logiki biznesowej bezpośrednio na stronie sieci Web. W przypadku dostatecznie dużych lub złożonych aplikacji projektowanie, implementowanie i używanie architektury warstwowej jest istotnie ważne dla sukcesu, możliwość aktualizacji i utrzymania aplikacji. Jednak Tworzenie niezawodnej architektury może być niepotrzebne w przypadku tworzenia aplikacji, które nie są jeszcze proste.

ASP.NET 2,0 zapewnia pięć wbudowanych formantów źródła danych [kontrolki SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)i [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). Kontrolki SqlDataSource może służyć do uzyskiwania dostępu do danych i modyfikowania ich bezpośrednio z relacyjnej bazy danych, w tym Microsoft SQL Server, Microsoft Access, Oracle, MySQL i innych. W tym samouczku i następnych trzech przeanalizuje sposób pracy z kontrolką kontrolki SqlDataSource, eksplorowania zapytań i filtrowania danych bazy danych oraz jak używać kontrolki SqlDataSource do wstawiania, aktualizowania i usuwania danych.

![ASP.NET 2,0 obejmuje pięć wbudowanych kontrolek źródła danych](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Rysunek 1**: ASP.NET 2,0 zawiera pięć wbudowanych kontrolek źródła danych

## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Porównywanie elementu ObjectDataSource i kontrolki SqlDataSource

Koncepcyjnie, zarówno formanty ObjectDataSource, jak i kontrolki SqlDataSource są po prostu serwerami proxy do danych. Zgodnie z opisem w sekcji [Wyświetlanie danych za pomocą](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) samouczka ObjectDataSource, element ObjectDataSource ma właściwości wskazujące typ obiektu, który dostarcza dane, oraz metody, które mają być wywoływane, aby wybierać, wstawiać, aktualizować i usuwać dane z bazowego typu obiektu. Po skonfigurowaniu właściwości ObjectDataSource s, formant sieci Web danych, taki jak GridView, DetailsView lub DataList, można powiązać z kontrolką przy użyciu metod `Select()`, `Insert()`, `Delete()`i `Update()`, aby współistnieć z podstawową architekturą.

Kontrolki SqlDataSource zapewnia te same funkcje, ale działa w odniesieniu do relacyjnej bazy danych, a nie biblioteki obiektów. Przy użyciu kontrolki SqlDataSource musimy określić parametry połączenia z bazą danych oraz zapytania SQL ad hoc lub procedury składowane do wykonania, aby wstawiać, aktualizować, usuwać i pobierać dane. Metody kontrolki SqlDataSource s `Select()`, `Insert()`, `Update()`i `Delete()`, gdy są wywoływane, nawiązują połączenie z określoną bazą danych i generują odpowiednie zapytanie SQL. Jak przedstawiono na poniższym diagramie, te metody wykonują gruntą połączenie z bazą danych, wysyłając zapytanie i zwracają wyniki.

![Kontrolki SqlDataSource służy jako serwer proxy dla bazy danych](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Rysunek 2**. kontrolki SqlDataSource służy jako serwer proxy dla bazy danych

> [!NOTE]
> W tym samouczku będziemy skupić się na pobieraniu danych z bazy danych. W samouczku [Wstawianie, aktualizowanie i usuwanie danych za pomocą kontrolki kontrolki SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) zobaczymy, jak skonfigurować kontrolki SqlDataSource do obsługi wstawiania, aktualizowania i usuwania.

## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Kontrolki kontrolki SqlDataSource i AccessDataSource

Oprócz formantu kontrolki SqlDataSource, ASP.NET 2,0 zawiera również formant AccessDataSource. Te dwa różne kontrolki powodują, że wiele deweloperów prowadzi do ASP.NET 2,0, aby podejrzewać, że formant AccessDataSource jest przeznaczony do pracy wyłącznie z programem Microsoft Access z kontrolką kontrolki SqlDataSource zaprojektowaną do pracy wyłącznie z Microsoft SQL Server. Chociaż obiekt AccessDataSource został zaprojektowany z myślą o współpracy z programem Microsoft Access, formant kontrolki SqlDataSource współpracuje z *dowolną* relacyjną bazą danych, do której można uzyskać dostęp za pomocą platformy .NET. Obejmuje to wszystkie magazyny danych zgodne ze standardem OleDb lub ODBC, takie jak Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL i PostgreSQL, między wieloma innymi.

Jedyną różnicą między kontrolkami AccessDataSource i kontrolki SqlDataSource jest określenie, jak są określone informacje o połączeniu z bazą danych. Kontrolka AccessDataSource wymaga tylko ścieżki pliku do pliku bazy danych programu Access. Kontrolki SqlDataSource, z drugiej strony, wymaga kompletnych parametrów połączenia.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Krok 1. Tworzenie stron sieci Web kontrolki SqlDataSource

Przed rozpoczęciem eksplorowania sposobu pracy bezpośrednio z danymi bazy danych przy użyciu kontrolki kontrolki SqlDataSource poczekaj chwilę na utworzenie stron ASP.NET w naszym projekcie witryny sieci Web, które będą potrzebne w tym samouczku i następnych trzech. Zacznij od dodania nowego folderu o nazwie `SqlDataSource`. Następnie Dodaj następujące strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`

![Dodaj strony ASP.NET dla samouczków związanych z kontrolki SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Rysunek 3**. dodawanie stron ASP.NET dla samouczków związanych z kontrolki SqlDataSource

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `SqlDataSource` wyświetli samouczki w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. W związku z tym należy dodać tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na stronę widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Ilustracja 4**. Dodawanie `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))

Na koniec Dodaj te cztery strony jako wpisy do pliku `Web.sitemap`. Przede wszystkim Dodaj następujące znaczniki po dodaniu przycisków niestandardowych do `<siteMapNode>`DataList i wzmacniak:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy do edycji, wstawiania i usuwania samouczków.

![Mapa witryny zawiera teraz wpisy dla samouczków kontrolki SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Rysunek 5**. Mapa witryny zawiera teraz wpisy dotyczące samouczków kontrolki SqlDataSource

## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Krok 2. Dodawanie i Konfigurowanie kontrolki kontrolki SqlDataSource

Aby rozpocząć, Otwórz stronę `Querying.aspx` w folderze `SqlDataSource` i przejdź do widok Projekt. Przeciągnij kontrolkę kontrolki SqlDataSource z przybornika do projektanta i ustaw jej `ID`, aby `ProductsDataSource`. Podobnie jak w przypadku elementu ObjectDataSource, kontrolki SqlDataSource nie produkuje żadnych renderowanych danych wyjściowych, dlatego pojawia się jako szare pole na powierzchni projektowej. Aby skonfigurować kontrolki SqlDataSource, kliknij link Konfiguruj źródło danych z tagu inteligentnego kontrolki SqlDataSource s.

![Kliknij link Konfiguruj źródło danych z tagu inteligentnego kontrolki SqlDataSource s](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Ilustracja 6**. Kliknij link Konfiguruj źródło danych z tagu inteligentnego kontrolki SqlDataSource s

Spowoduje to wyświetlenie Kreatora konfiguracji źródła danych kontrolki SqlDataSource sterowania. Mimo że kroki kreatora s różnią się od kontrolki ObjectDataSource, cel końcowy jest taki sam, aby zapewnić szczegóły dotyczące pobierania, wstawiania, aktualizowania i usuwania danych za pośrednictwem źródła danych. W przypadku kontrolki SqlDataSource to pociąga za sobą określenie źródłowej bazy danych, która będzie używana i dostarczająca instrukcje SQL ad hoc lub procedury składowane.

Pierwszy krok kreatora poprosi nas o bazę danych. Lista rozwijana zawiera te bazy danych znajdujące się w folderze `App_Data` aplikacji sieci Web, a także te, które zostały dodane do węzła połączenia danych w Eksplorator serwera. Ponieważ już dodaliśmy parametry połączenia dla bazy danych `NORTHWIND.MDF` w folderze `App_Data` do naszego pliku `Web.config` projektu, lista rozwijana zawiera odwołanie do tych parametrów połączenia, `NORTHWINDConnectionString`. Wybierz ten element z listy rozwijanej i kliknij przycisk Dalej.

![Wybierz NORTHWINDConnectionString z listy rozwijanej](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Rysunek 7**. Wybierz `NORTHWINDConnectionString` z listy rozwijanej

Po wybraniu bazy danych Kreator pyta o zapytanie, aby zwracało dane. Możemy określić kolumny tabeli lub widoku do zwrócenia lub można wprowadzić niestandardową instrukcję SQL lub określić procedurę przechowywaną. Można przełączać się między tym wyborem za pośrednictwem Określ niestandardową instrukcję SQL lub procedurę składowaną oraz określać kolumny z poziomu tabeli lub przycisków radiowych widok.

> [!NOTE]
> Dla pierwszego przykładu Użyj opcji Określ kolumny z tabeli lub widoku. Powrócimy do kreatora w dalszej części tego samouczka i eksploruje opcję Określ niestandardową instrukcję SQL lub procedurę składowaną.

Na rysunku nr 8 przedstawiono ekran Konfiguruj instrukcję, gdy jest zaznaczone pole wyboru Określ kolumny z tabeli lub widoku. Lista rozwijana zawiera zestaw tabel i widoków w bazie danych Northwind z wybraną kolumną tabeli lub widoku wyświetlaną na liście pól wyboru poniżej. Na potrzeby tego przykładu należy zwrócić kolumny `ProductID`, `ProductName`i `UnitPrice` z tabeli `Products`. Jak pokazano na rysunku 8, po dokonaniu wyboru Kreator wyświetli wyniki `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`instrukcji SQL.

![Zwracanie danych z tabeli Products](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Ilustracja 8**. Zwracanie danych z tabeli `Products`

Po skonfigurowaniu kreatora do zwracania kolumn `ProductID`, `ProductName`i `UnitPrice` z tabeli `Products` kliknij przycisk Dalej. Ten końcowy ekran umożliwia sprawdzenie wyników zapytania skonfigurowanego w poprzednim kroku. Kliknięcie przycisku Testuj zapytanie wykonuje skonfigurowaną instrukcję `SELECT` i wyświetla wyniki w siatce.

![Kliknij przycisk Testuj zapytanie, aby przejrzeć wybrane zapytanie](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Ilustracja 9**. kliknij przycisk Testuj zapytanie, aby przejrzeć zapytanie `SELECT`

Aby zakończyć pracę kreatora, kliknij przycisk Zakończ.

Podobnie jak w przypadku elementu ObjectDataSource, Kreator kontrolki SqlDataSource s tylko przypisuje wartości do właściwości kontrolki, a mianowicie [`ConnectionString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) i [`SelectCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) właściwości. Po zakończeniu działania kreatora znacznik deklaratywny kontrolki SqlDataSource kontrolki powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

Właściwość `ConnectionString` zawiera informacje na temat łączenia się z bazą danych. Do tej właściwości można przypisać kompletną, zakodowaną wartość parametrów połączenia lub wskazać parametry połączenia w `Web.config`. Aby odwołać się do wartości parametrów połączenia w pliku Web. config, użyj składni `<%$ expressionPrefix:expressionValue %>`. Zazwyczaj *ExpressionPrefix* jest connectionStrings, a *expressionValue* jest nazwą ciągu connect w [sekcji`<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx)`Web.config`. Jednak składnia może służyć do odwoływania się do `<appSettings>` elementów lub zawartości z plików zasobów. Aby uzyskać więcej informacji na temat tej składni, zobacz [ASP.NET wyrażeń](https://msdn.microsoft.com/library/d5bd1tad.aspx) .

Właściwość `SelectCommand` określa instrukcję SQL ad hoc lub procedurę przechowywaną do wykonania w celu zwrócenia danych.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Krok 3. Dodawanie kontrolki sieci Web danych i powiązywanie jej z kontrolki SqlDataSource

Po skonfigurowaniu kontrolki SqlDataSource można go powiązać z kontrolką sieci Web danych, taką jak GridView lub DetailsView. W tym samouczku poinformujemy o tym, że dane są wyświetlane w widoku GridView. Z przybornika przeciągnij widok GridView na stronę i powiąż go z `ProductsDataSource` kontrolki SqlDataSource, wybierając źródło danych z listy rozwijanej w tagu inteligentnym GridView.

[![dodać widoku GridView i powiązać go z kontrolką kontrolki SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Ilustracja 10**. Dodawanie widoku GridView i powiązywanie go z kontrolką kontrolki SqlDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))

Po wybraniu kontrolki kontrolki SqlDataSource z listy rozwijanej w tagu inteligentnym GridView, program Visual Studio automatycznie doda element BoundField lub CheckBoxField do widoku GridView dla każdej z kolumn zwracanych przez formant źródła danych. Ponieważ kontrolki SqlDataSource zwraca trzy kolumny bazy danych `ProductID`, `ProductName`i `UnitPrice` istnieją trzy pola w widoku GridView.

Poświęć chwilę na skonfigurowanie trzech BoundFields. Zmień właściwość `HeaderText` pola `ProductName` na nazwę produktu i `UnitPrice` pole s na Price. Sformatuj również pole `UnitPrice` jako walutę. Po wprowadzeniu tych zmian znaczniki deklaratywne GridView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Odwiedź Tę stronę za pomocą przeglądarki. Jak pokazano na rysunku 11, w widoku GridView są wyświetlane wszystkie wartości `ProductID`produktu, `ProductName`i `UnitPrice`.

[![w widoku GridView są wyświetlane wartości IDProduktu, ProductName i CenaJednostkowa każdego produktu](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Ilustracja 11**. widok GridView przedstawia każdy produkt `ProductID`, `ProductName`i `UnitPrice` wartości (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))

Po odwiedzeniu strony GridView wywołuje metodę `Select()` kontroli źródła danych. W przypadku używania kontrolki ObjectDataSource, nazywana `ProductsBLL` klasą `GetProducts()` Metoda. Kontrolki SqlDataSource jednak Metoda `Select()` nawiązuje połączenie z określoną bazą danych i wystawia `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`w tym przykładzie). Kontrolki SqlDataSource zwraca wyniki, które następnie jest wyliczany przez GridView, tworząc wiersz w widoku GridView dla każdego zwróconego rekordu bazy danych.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Wbudowane funkcje formantu sieci Web danych i formant kontrolki SqlDataSource

Ogólnie rzecz biorąc, funkcje związane z kontrolkami sieci Web danych, sortowanie, edytowanie, usuwanie, wstawianie i tak dalej są specyficzne dla formantu sieci Web danych i nie są zależne od używanej kontroli źródła danych. Oznacza to, że widok GridView może korzystać z wbudowanego stronicowania, sortowania, edytowania i usuwania, niezależnie od tego, czy jest on powiązany z elementem ObjectDataSource lub kontrolki SqlDataSource. Jednak niektóre funkcje kontroli sieci Web są poufne dla używanej kontroli źródła danych lub konfiguracji kontroli źródła danych.

Na przykład w [efektywnej stronicowaniu za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) opisano, jak domyślnie logika stronicowania dla kontrolek sieci Web danych naively zwraca *wszystkie* rekordy z bazowego źródła danych, a następnie wyświetla tylko odpowiedni podzbiór rekordów o bieżącym indeksie strony oraz liczbę rekordów, które mają być wyświetlane na stronie. Ten model jest wysoce wydajny podczas stronicowania w wystarczająco dużych zestawach wyników. Na szczęście element ObjectDataSource można skonfigurować w taki sposób, aby obsługiwał niestandardowe stronicowanie, które zwraca tylko dokładny podzestaw rekordów do wyświetlenia. Formant kontrolki SqlDataSource nie ma jednak właściwości do implementowania stronicowania niestandardowego.

Inny subtlety z stronicowaniem i sortowaniem powstaje w kontrolki SqlDataSource. Domyślnie dane zwracane z kontrolki SqlDataSource mogą być stronicowane lub sortowane za pośrednictwem widoku GridView. Aby to zademonstrować, zaznacz opcje Włącz stronicowanie i Włącz sortowanie w tagów inteligentnych GridView s w `Querying.aspx` i sprawdź, czy działa ono zgodnie z oczekiwaniami.

Sortowanie i stronicowanie działa, ponieważ kontrolki SqlDataSource pobiera dane bazy danych do swobodnie określonego zestawu danych. Całkowita liczba rekordów zwracanych przez zapytanie, które są istotnym aspektem implementowania stronicowania, można ustalić na podstawie zestawu danych. Ponadto wyniki zestawu danych s mogą być sortowane za pomocą elementu DataView. Te funkcje są automatycznie używane przez kontrolki SqlDataSource, gdy żądanie GridView ma stronicowane lub posortowane dane.

Kontrolki SqlDataSource można skonfigurować w taki sposób, aby zwracał element DataReader zamiast zestawu danych przez zmianę jego [właściwości`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) z `DataSet` (wartość domyślna) na `DataReader`. Użycie elementu DataReader może być preferowane w sytuacjach, gdy przekazuje wyniki kontrolki SqlDataSource do istniejącego kodu, który oczekuje elementu DataReader. Ponadto ponieważ obiekty odczytujące są znacznie prostsze niż zestawy danych, oferują lepszą wydajność. Jeśli wprowadzisz tę zmianę, jednak kontrolka sieci Web danych nie będzie mogła być sortowana ani stronicowa, ponieważ kontrolki SqlDataSource nie może ustalić, ile rekordów jest zwracanych przez zapytanie, ani nie ma żadnych technik do sortowania zwracanych danych.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Krok 4. Używanie niestandardowej instrukcji SQL lub procedury składowanej

Podczas konfigurowania formantu kontrolki SqlDataSource, zapytanie używane do zwracania danych można określić w jednym z dwóch metod jako niestandardowa instrukcja SQL lub procedura składowana lub jako kolumny z istniejącej tabeli lub widoku. W kroku 2 sprawdzono wybór kolumn z tabeli `Products`. Zezwól na korzystanie z niestandardowej instrukcji SQL.

Dodaj kolejną kontrolkę GridView do strony `Querying.aspx` i wybierz opcję utworzenia nowego źródła danych z listy rozwijanej w tagu inteligentnym. Następnie wskaż, że dane zostaną pobrane z bazy danych. spowoduje to utworzenie nowej kontrolki kontrolki SqlDataSource. Nadaj nazwę formantowi `ProductsWithCategoryInfoDataSource`.

![Utwórz nową kontrolkę kontrolki SqlDataSource o nazwie ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Ilustracja 12**. Tworzenie nowej kontrolki kontrolki SqlDataSource o nazwie `ProductsWithCategoryInfoDataSource`

Na następnym ekranie zostanie wyświetlony monit o określenie bazy danych. Po powrocie na rysunek 7 wybierz `NORTHWINDConnectionString` z listy rozwijanej i kliknij przycisk Dalej. Na ekranie Konfigurowanie instrukcji SELECT wybierz przycisk radiowy Określ niestandardową instrukcję SQL lub procedurę składowaną, a następnie kliknij przycisk Dalej. Spowoduje to wyświetlenie ekranu Zdefiniuj niestandardowe instrukcje lub procedury składowane, które oferuje karty z etykietą SELECT, UPDATE, INSERT i DELETE. Na każdej karcie można wprowadzić niestandardową instrukcję SQL do pola tekstowego lub wybrać procedurę przechowywaną z listy rozwijanej. W tym samouczku przejdziemy do wprowadzenia niestandardowej instrukcji SQL. następny samouczek zawiera przykład, który używa procedury składowanej.

![Wprowadź niestandardową instrukcję SQL lub wybierz procedurę składowaną](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Ilustracja 13**. wprowadzanie NIESTANDARDOWEJ instrukcji SQL lub wybieranie procedury składowanej

Niestandardowe instrukcje SQL można wprowadzać ręcznie do pola tekstowego lub można je utworzyć graficznie, klikając przycisk Konstruktor zapytań. Z poziomu Konstruktor zapytań lub pola tekstowego, użyj następującego zapytania, aby zwrócić wartości `ProductID` i `ProductName` z tabeli `Products` przy użyciu `JOIN` do pobrania `CategoryName` produktu z tabeli `Categories`:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]

![Zapytanie można utworzyć graficznie przy użyciu Konstruktor zapytań](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Ilustracja 14**. można utworzyć graficznie zapytanie przy użyciu Konstruktor zapytań

Po określeniu zapytania kliknij przycisk Dalej, aby przejoć do ekranu test zapytania. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora kontrolki SqlDataSource.

Po zakończeniu działania kreatora w widoku GridView zostaną dodane trzy BoundFields, które wyświetlają kolumny `ProductID`, `ProductName`i `CategoryName` zwrócone z zapytania i w wyniku następujące znaczniki deklaracyjne:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]

[![GridView pokazuje każdy identyfikator produktu, nazwę i nazwę powiązanej kategorii](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Ilustracja 15**. widok GridView pokazuje każdy identyfikator produktu, nazwę i skojarzoną nazwę kategorii ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazano, jak wykonywać zapytania i wyświetlać dane przy użyciu formantu kontrolki SqlDataSource. Podobnie jak w przypadku elementu ObjectDataSource, kontrolki SqlDataSource służy jako serwer proxy, zapewniając deklaratywne podejście do uzyskiwania dostępu do danych. Jego właściwości określają bazę danych, z którą ma zostać nawiązane połączenie, oraz kwerendę programu SQL `SELECT` do wykonania; można je określić za pomocą okno Właściwości lub za pomocą Kreatora konfiguracji źródła danych.

Przykłady zapytań `SELECT`, które zostały zbadane w tym samouczku, zwracają wszystkie rekordy z określonego zapytania. Kontrolka kontrolki SqlDataSource, jednak może zawierać klauzulę `WHERE` z parametrami, których wartości są przypisywane programowo lub są automatycznie ściągane z określonego źródła. Sprawdzimy, jak tworzyć i używać zapytań parametrycznych w następnym samouczku.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Uzyskiwanie dostępu do danych relacyjnej bazy danych](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Kontrolki SqlDataSource — informacje o formancie](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Samouczki szybkiego startu ASP.NET: formant kontrolki SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Plik Web. config `<connectionStrings>` element](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Odwołanie do parametrów połączenia z bazą danych](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Susan Connery, Bernadette Leigh i David suru. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [dalej](using-parameterized-queries-with-the-sqldatasource-vb.md)
