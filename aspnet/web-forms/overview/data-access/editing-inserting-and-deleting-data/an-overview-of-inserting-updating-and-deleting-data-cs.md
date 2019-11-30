---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Omówienie wstawiania, aktualizowania i usuwania danych (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak zmapować metody Insert (), Update () i Delete () klasy ObjectDataSource na metody klas LOGIKI biznesowej, a także jak konfigu...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e26b8e841f86272a158b0c09b62ab2790d01d191
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74571385"
---
# <a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Omówienie wstawiania, aktualizowania i usuwania danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) lub [Pobierz plik PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> W tym samouczku dowiesz się, jak zmapować metody Insert (), Update () i Delete () klasy ObjectDataSource na metody klas LOGIKI biznesowej, a także jak skonfigurować kontrolki GridView, DetailsView i FormView w celu zapewnienia możliwości modyfikacji danych.

## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku samouczków sprawdzono sposób wyświetlania danych na stronie ASP.NET przy użyciu kontrolek GridView, DetailsView i FormView. Te kontrolki działają po prostu z danymi dostarczonymi do nich. Zwykle te kontrolki uzyskują dostęp do danych za pomocą kontrolki źródła danych, takiej jak element ObjectDataSource. Zaobserwowano, jak element ObjectDataSource działa jako serwer proxy między stroną ASP.NET i danymi źródłowymi. Gdy GridView musi wyświetlać dane, wywołuje metodę `Select()` elementu ObjectDataSource, która z kolei wywołuje metodę z naszej warstwy logiki biznesowej (LOGIKI biznesowej), która wywołuje metodę w odpowiedniej warstwie dostępu do danych (DAL) TableAdapter, która z kolei wysyła zapytanie `SELECT` do bazy danych Northwind.

Odwołaj się, że po utworzeniu TableAdapters w DAL w [pierwszym samouczku](../introduction/creating-a-data-access-layer-cs.md)program Visual Studio automatycznie dodaliśmy metody wstawiania, aktualizowania i usuwania danych z tabeli źródłowej bazy danych. Ponadto przy [tworzeniu warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-cs.md) zaprojektowano metody w logiki biznesowej, które wywołały metody dal modyfikacje danych.

Oprócz metody `Select()` element ObjectDataSource ma także metody `Insert()`, `Update()`i `Delete()`. Podobnie jak w przypadku metody `Select()`, te trzy metody można zamapować na metody w obiekcie źródłowym. Gdy jest skonfigurowany do wstawiania, aktualizowania lub usuwania danych, formanty GridView, DetailsView i FormView zapewniają interfejs użytkownika służący do modyfikowania danych źródłowych. Ten interfejs użytkownika wywołuje metody `Insert()`, `Update()`i `Delete()` elementu ObjectDataSource, który następnie wywołuje metody skojarzone z obiektem źródłowym (patrz rysunek 1).

[![metody Insert (), Update () i Delete () obiektu ObjectDataSource jako serwer proxy do LOGIKI biznesowej](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Rysunek 1**: metody `Insert()`, `Update()`i `Delete()` elementu ObjectDataSource są używane jako serwer proxy w logiki biznesowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))

W tym samouczku opisano sposób mapowania metod `Insert()`, `Update()`i `Delete()` elementu ObjectDataSource na metody klas w LOGIKI biznesowej, a także sposób konfigurowania formantów GridView, DetailsView i FormView w celu zapewnienia możliwości modyfikacji danych.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Krok 1. Tworzenie stron sieci Web wstawiania, aktualizacji i usuwania samouczków

Przed rozpoczęciem eksplorowania sposobu wstawiania, aktualizowania i usuwania danych spróbuj przystąpić do tworzenia stron ASP.NET w naszym projekcie witryny sieci Web, które będziemy potrzebować do tego samouczka i następnych kilku z nich. Zacznij od dodania nowego folderu o nazwie `EditInsertDelete`. Następnie Dodaj następujące strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Dodaj strony ASP.NET dla samouczków związanych z modyfikacją danych](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Rysunek 2**. dodawanie stron ASP.NET dla samouczków związanych z modyfikacją danych

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `EditInsertDelete` wyświetli samouczki w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. W związku z tym Dodaj tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Rysunek 3**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))

Na koniec Dodaj strony jako wpisy do pliku `Web.sitemap`. W tym celu należy dodać następujące znaczniki po dostosowaniu `<siteMapNode>`formatowania:

[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy do edycji, wstawiania i usuwania samouczków.

![Mapa witryny zawiera teraz wpisy umożliwiające edytowanie, wstawianie i usuwanie samouczków](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Rysunek 4**. Mapa witryny zawiera teraz wpisy do samouczków dotyczących edycji, wstawiania i usuwania

## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Krok 2. Dodawanie i Konfigurowanie kontrolki ObjectDataSource

Ponieważ dane GridView, DetailsView i FormView różnią się w zależności od możliwości i układu modyfikacji danych, należy dokładnie przeanalizować każdy z nich. Zamiast korzystać z każdej kontrolki korzystającej z własnego elementu ObjectDataSource, należy jednak po prostu utworzyć pojedynczy element ObjectDataSource, który może być współużytkowany przez wszystkie trzy przykłady formantów.

Otwórz stronę `Basics.aspx`, przeciągnij element ObjectDataSource z przybornika do projektanta, a następnie kliknij link Konfiguruj źródło danych z tagu inteligentnego. Ponieważ `ProductsBLL` jest jedyną klasą LOGIKI biznesowej, która umożliwia edytowanie, wstawianie i usuwanie metod, skonfiguruj element ObjectDataSource do korzystania z tej klasy.

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Rysunek 5**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))

Na następnym ekranie możemy określić, jakie metody klasy `ProductsBLL` są mapowane na `Select()`, `Insert()`, `Update()`i `Delete()` elementu ObjectDataSource, wybierając odpowiednią kartę i wybierając metodę z listy rozwijanej. Rysunek 6, który powinien wyglądać znajomo teraz, mapuje metodę `Select()` elementu ObjectDataSource na metodę `GetProducts()` klasy `ProductsBLL`. Metody `Insert()`, `Update()`i `Delete()` można skonfigurować, wybierając odpowiednią kartę z listy wzdłuż góry.

[![, że element ObjectDataSource zwróci wszystkie produkty](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Ilustracja 6**. czy element ObjectDataSource zwraca wszystkie produkty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))

Rysunki 7, 8 i 9 przedstawiają karty UPDATE, INSERT i DELETE elementu ObjectDataSource. Skonfiguruj te karty tak, aby metody `Insert()`, `Update()`i `Delete()` wywoływały odpowiednio metody `UpdateProduct`, `AddProduct`i `DeleteProduct` klasy `ProductsBLL`.

[![zmapować metody Update () elementu ObjectDataSource na metodę UpdateProduct klasy ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Rysunek 7**. zamapuj metodę `Update()` elementu ObjectDataSource na metodę `UpdateProduct` klasy `ProductBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))

[![zmapować metody Insert () elementu ObjectDataSource na metodę addproduct klasy ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Ilustracja 8**. zamapuj metodę `Insert()` elementu ObjectDataSource na metodę Add `Product` klasy `ProductBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))

[![zmapować metody Delete () elementu ObjectDataSource na metodę DeleteProduct klasy ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Ilustracja 9**. zamapuj metodę `Delete()` elementu ObjectDataSource na metodę `DeleteProduct` klasy `ProductBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))

Być może zauważono, że listy rozwijane na kartach aktualizacja, wstawianie i usuwanie mają już wybrane te metody. Jest to poświęcenie naszym użyciu `DataObjectMethodAttribute`, które zdobi metody `ProductsBLL`. Na przykład Metoda DeleteProduct ma następujący podpis:

[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

Atrybut `DataObjectMethodAttribute` wskazuje przeznaczenie każdej metody do wybierania, wstawiania, aktualizowania lub usuwania oraz niezależnie od tego, czy jest to wartość domyślna. Jeśli pominięto te atrybuty podczas tworzenia klas LOGIKI biznesowej, należy ręcznie wybrać metody z kart UPDATE, INSERT i DELETE.

Po upewnieniu się, że odpowiednie metody `ProductsBLL` są mapowane na metody `Insert()`, `Update()`i `Delete()` elementu ObjectDataSource, kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

## <a name="examining-the-objectdatasources-markup"></a>Badanie znaczników elementu ObjectDataSource

Po skonfigurowaniu elementu ObjectDataSource za pomocą jego kreatora przejdź do widoku źródła, aby sprawdzić wygenerowane deklaratywne znaczniki. Tag `<asp:ObjectDataSource>` określa obiekt źródłowy i metody do wywołania. Ponadto istnieją `DeleteParameters`, `UpdateParameters`i `InsertParameters`, które mapują do parametrów wejściowych dla `AddProduct`klasy `ProductsBLL`, `UpdateProduct`i `DeleteProduct` metod:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

Element ObjectDataSource zawiera parametr dla każdego z parametrów wejściowych dla skojarzonych z nimi metod, podobnie jak lista `SelectParameter` s jest obecna, gdy element ObjectDataSource jest skonfigurowany do wywołania metody Select, która oczekuje parametru wejściowego (takiego jak `GetProductsByCategoryID(categoryID)`). Ponieważ wkrótce zobaczymy wartości dla tych `DeleteParameters`, `UpdateParameters`i `InsertParameters` są ustawiane automatycznie przez GridView, DetailsView i FormView przed wywołaniem metody `Insert()`, `Update()`lub `Delete()` elementu ObjectDataSource. Te wartości można również skonfigurować programowo w razie konieczności, ponieważ będziemy omawiać w przyszłym samouczku.

Jednym efektem ubocznym korzystania z kreatora w celu skonfigurowania elementu ObjectDataSource jest to, że program Visual Studio ustawia [Właściwość OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) na `original_{0}`. Ta wartość właściwości służy do uwzględniania oryginalnych wartości edytowanych danych i jest przydatna w dwóch scenariuszach:

- Jeśli podczas edytowania rekordu, użytkownicy mogą zmienić wartość klucza podstawowego. W takim przypadku należy podać zarówno nową wartość klucza podstawowego, jak i pierwotną wartość klucza podstawowego, aby można było znaleźć rekord z oryginalną wartością klucza podstawowego i zaktualizować jego wartość.
- W przypadku korzystania z optymistycznej współbieżności. Optymistyczna współbieżność to technika, która zapewnia, że dwóch równoczesnych użytkowników nie zastąpią zmian i jest tematem dla przyszłego samouczka.

Właściwość `OldValuesParameterFormatString` wskazuje nazwę parametrów wejściowych w metodach aktualizacji i usuwania obiektu źródłowego dla oryginalnych wartości. W celu zbadania optymistycznej współbieżności będziemy omawiać tę właściwość i jej cel bardziej szczegółowo. Teraz jest to możliwe, ponieważ nasze metody LOGIKI biznesowej nie oczekują oryginalnych wartości i dlatego ważne jest, aby usunąć tę właściwość. Pozostawienie właściwości `OldValuesParameterFormatString` ustawionej na wartość inną niż domyślna (`{0}`) spowoduje błąd, gdy kontrolka sieci Web danych próbuje wywołać metody `Update()` lub `Delete()` elementu ObjectDataSource, ponieważ element ObjectDataSource podejmie próbę przekazania zarówno `UpdateParameters`, jak i `DeleteParameters`, jak i parametrów oryginalnych wartości.

Jeśli nie jest to poważny jasne w tym miejscu, nie martw się, sprawdzimy tę właściwość i narzędzie w przyszłym samouczku. Na razie należy tylko usunąć tę deklarację właściwości wyłącznie ze składni deklaracyjnej lub ustawić wartość domyślną ({0}).

> [!NOTE]
> Jeśli po prostu wyczyścisz wartość właściwości `OldValuesParameterFormatString` z okno Właściwości w widok Projekt, właściwość będzie nadal istniała w składni deklaracyjnej, ale zostanie ustawiona jako pusty ciąg. Niestety, nadal będzie to powodowało ten sam problem opisany powyżej. W związku z tym Usuń właściwość całkowicie ze składni deklaratywnej lub z okno Właściwości ustaw wartość domyślną, `{0}`.

## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Krok 3. Dodawanie kontrolki sieci Web danych i Konfigurowanie jej do modyfikacji danych

Gdy element ObjectDataSource zostanie dodany do strony i skonfigurowany, będzie można dodać do strony kontrolki sieci Web danych, aby wyświetlić dane i udostępnić użytkownikom końcowym możliwość ich zmodyfikowania. Zobaczysz oddzielnie widok GridView, DetailsView i FormView, ponieważ te kontrolki sieci Web danych różnią się w zależności od możliwości modyfikacji danych i konfiguracji.

Jak widać w pozostałej części tego artykułu, dodanie bardzo podstawowej edycji, wstawiania i usuwania pomocy technicznej za pomocą kontrolek GridView, DetailsView i FormView jest naprawdę tak proste jak sprawdzanie kilku pól wyboru. W świecie istnieje wiele przypadków subtletiesych i brzegowych, które udostępniają takie funkcje, aby nie tylko wskazywały i klikać. W tym samouczku omówiono jednak jedynie uproszczony możliwości modyfikacji danych. W przyszłości samouczki zapoznają się z problemami, które będą Undoubtedly w świecie rzeczywistym.

## <a name="deleting-data-from-the-gridview"></a>Usuwanie danych z widoku GridView

Zacznij od przeciągnięcia widoku GridView z przybornika do projektanta. Następnie powiąż element ObjectDataSource z GridView, wybierając go z listy rozwijanej w tagu inteligentnym GridView. W tym momencie znaczniki deklaratywne GridView będą:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Powiązanie widoku GridView z elementem ObjectDataSource za pomocą jego tagu inteligentnego ma dwie zalety:

- BoundFields i CheckBoxFields są tworzone automatycznie dla każdego z pól zwracanych przez element ObjectDataSource. Ponadto właściwości BoundField i CheckBoxField są ustawiane na podstawie metadanych pola źródłowego. Na przykład pola `ProductID`, `CategoryName`i `SupplierName` są oznaczone jako tylko do odczytu w `ProductsDataTable` i w związku z tym nie można ich aktualizować podczas edytowania. Aby to umożliwić, te [właściwości tylko do odczytu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) BoundFields są ustawione na `true`.
- [Właściwość DataKeyNames ustawionej](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) jest przypisywana do pól klucza podstawowego obiektu źródłowego. Jest to istotne w przypadku używania widoku GridView do edytowania lub usuwania danych, ponieważ ta właściwość wskazuje pole (lub zestaw pól), które jednoznacznie identyfikuje każdy rekord. Aby uzyskać więcej informacji na temat właściwości `DataKeyNames`, zapoznaj się z powrotem z [danymi głównymi/szczegółowymi przy użyciu wieloelementowego widoku głównego GridView zawierającego szczegóły](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczka kontrolką detailview.

Widok GridView można powiązać z elementem ObjectDataSource za pomocą składni okno Właściwości lub deklaracyjnej, więc wymaga to ręcznego dodania odpowiednich BoundField i `DataKeyNames` znaczników.

Formant GridView zawiera wbudowaną obsługę edytowania i usuwania na poziomie wiersza. Skonfigurowanie widoku GridView do obsługi usuwania powoduje dodanie kolumny usuwania przycisków. Gdy użytkownik końcowy kliknie przycisk Usuń dla danego wiersza, wystąpi odświeżenie, a w widoku GridView wykonywane są następujące czynności:

1. `DeleteParameters` wartości elementu ObjectDataSource są przypisane
2. Metoda `Delete()` elementu ObjectDataSource jest wywoływana, usuwanie określonego rekordu
3. Element GridView ponownie wiąże się z elementem ObjectDataSource, wywołując jego metodę `Select()`

Wartości przypisane do `DeleteParameters` to wartości pól `DataKeyNames` dla wiersza, dla którego kliknięto przycisk Usuń. W związku z tym ważne jest, aby Właściwość `DataKeyNames` GridView była prawidłowo ustawiona. W przeciwnym razie `DeleteParameters` zostanie przypisana `null` wartość w kroku 1, co z kolei nie spowoduje żadnych usuniętych rekordów w kroku 2.

> [!NOTE]
> Kolekcja `DataKeys` jest przechowywana w stanie formantu GridView s, co oznacza, że wartości `DataKeys` będą zapamiętywane przez ogłaszanie zwrotne, nawet jeśli stan widoku GridView został wyłączony. Jest to jednak bardzo ważne, aby stan widoku pozostał włączony dla widoków GridView, które obsługują edytowanie lub usuwanie (zachowanie domyślne). Jeśli ustawisz właściwość GridView s `EnableViewState` na `false`, zachowanie edytowania i usuwania będzie działało dla jednego użytkownika, ale jeśli istnieją współbieżne użytkownicy usuwający dane, istnieje możliwość, że mogą przypadkowo usunąć lub edytować rekordy, które nie zostały zamierzone. Zobacz mój wpis w blogu, [Ostrzeżenie: problem współbieżności z ASP.NET 2,0 gridviews/DetailsView/FormView obsługujący edytowanie i/lub usuwanie stanu widoku](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), aby uzyskać więcej informacji.

To samo ostrzeżenie ma zastosowanie również do widoku DetailsViews i FormView.

Aby dodać możliwości usuwania do widoku GridView, po prostu przejdź do jego tagu inteligentnego i zaznacz pole wyboru Włącz usuwanie.

![Zaznacz pole wyboru Włącz usuwanie](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Ilustracja 10**. Zaznacz pole wyboru Włącz usuwanie

Zaznaczenie pola wyboru Włącz usuwanie z tagu inteligentnego powoduje dodanie CommandField do widoku GridView. CommandField renderuje kolumnę w widoku GridView z przyciskami służącymi do wykonywania co najmniej jednego z następujących zadań: Zaznaczanie rekordu, edytowanie rekordu i usuwanie rekordu. Wcześniej znaleźliśmy CommandField w działaniu z zaznaczaniem rekordów w bazie danych [master/detail przy użyciu wybieranego wzorca GridView ze szczegółowymi informacjami kontrolką detailview](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczka.

CommandField zawiera wiele właściwości `ShowXButton`, które wskazują, która seria przycisków jest wyświetlana w CommandField. Zaznaczając pole wyboru Włącz usuwanie CommandField, którego właściwość `ShowDeleteButton` została `true` została dodana do kolekcji kolumn GridView.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

W tym momencie uważamy, że jest to konieczne, dodając do widoku GridView obsługę usuwania. Jak pokazano na rysunku 11, podczas odwiedzania tej strony za pomocą przeglądarki jest obecna kolumna przycisków usuwania.

[![CommandField dodaje kolumnę przycisków usuwania](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Ilustracja 11**. CommandField dodaje kolumnę przycisków usuwania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))

Jeśli ten samouczek został już zebrany od podstaw, podczas testowania tej strony kliknięcie przycisku Usuń spowoduje wystąpienie wyjątku. Kontynuuj odczytywanie, aby dowiedzieć się, dlaczego te wyjątki zostały zgłoszone i jak je rozwiązać.

> [!NOTE]
> Jeśli korzystasz z narzędzia pobierania dołączonego do tego samouczka, te problemy zostały już uwzględnione. Jednak zapoznaj się z informacjami podanymi poniżej, aby pomóc w zidentyfikowaniu problemów, które mogą wystąpić, i odpowiednich obejściach.

Jeśli podczas próby usunięcia produktu wystąpi wyjątek, którego komunikat jest podobny do "ObjectDataSource1" elementu "*ObjectDataSource", nie można znaleźć niegenerycznej metody "DeleteProduct", która ma parametry: ProductID, oryginalny\_ProductID*, "prawdopodobnie zapomniano usunąć Właściwość `OldValuesParameterFormatString` z elementu ObjectDataSource. Po określeniu właściwości `OldValuesParameterFormatString` element ObjectDataSource próbuje przekazać oba `productID` i `original_ProductID` parametry wejściowe do metody `DeleteProduct`. `DeleteProduct`jednak akceptuje tylko jeden parametr wejściowy, dlatego wyjątek. Usunięcie właściwości `OldValuesParameterFormatString` (lub ustawienie jej do `{0}`) powoduje, że element ObjectDataSource nie zostanie podjęta próba przekazania oryginalnego parametru wejściowego.

[![upewnij się, że Właściwość OldValuesParameterFormatString została wyczyszczona](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Ilustracja 12**. Upewnij się, że właściwość `OldValuesParameterFormatString` została wyczyszczona ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))

Nawet jeśli usunięto Właściwość `OldValuesParameterFormatString`, nadal wystąpi wyjątek podczas próby usunięcia produktu z komunikatem: "*instrukcja DELETE spowodowała konflikt z ograniczeniem odwołania" obcy\_Order\_szczegóły\_produkty "* ." Baza danych Northwind zawiera ograniczenie klucza obcego między tabelami `Order Details` i `Products`, co oznacza, że produkt nie może zostać usunięty z systemu, jeśli istnieje co najmniej jeden rekord w tabeli `Order Details`. Ponieważ każdy produkt w bazie danych Northwind zawiera co najmniej jeden rekord w `Order Details`, nie można usunąć żadnych produktów do momentu, gdy najpierw usuniesz powiązane z nim rekordy szczegółów zamówienia.

[![ograniczenie klucza obcego uniemożliwia usunięcie produktów](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Ilustracja 13**. ograniczenie klucza obcego uniemożliwia usunięcie produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))

W naszym samouczku usuniesz teraz wszystkie rekordy z tabeli `Order Details`. W rzeczywistej aplikacji musimy:

- Inny ekran, aby zarządzać informacjami o szczegółach zamówienia
- Rozszerzanie metody `DeleteProduct` w celu uwzględnienia logiki w celu usunięcia szczegółów zamówienia określonego produktu
- Zmodyfikuj zapytanie SQL używane przez TableAdapter w celu uwzględnienia usunięcia szczegółów zamówienia określonego produktu

Po prostu usuń wszystkie rekordy z tabeli `Order Details`, aby obejść ograniczenie klucza obcego. Przejdź do Eksplorator serwera w programie Visual Studio, kliknij prawym przyciskiem myszy węzeł `NORTHWND.MDF`, a następnie wybierz polecenie nowe zapytanie. Następnie w oknie zapytania uruchom następującą instrukcję SQL: `DELETE FROM [Order Details]`

[![usunąć wszystkie rekordy z tabeli Order Details](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Ilustracja 14**. Usuwanie wszystkich rekordów z tabeli `Order Details` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))

Po wyczyszczeniu tabeli `Order Details` kliknięcie przycisku Usuń spowoduje usunięcie produktu bez błędu. Jeśli kliknięcie przycisku Usuń nie powoduje usunięcia produktu, należy sprawdzić, czy właściwość `DataKeyNames` GridView jest ustawiona na pole klucza podstawowego (`ProductID`).

> [!NOTE]
> Po kliknięciu przycisku Usuń ogłaszane zwrotne i rekord zostanie usunięty. Może to być niebezpieczne, ponieważ łatwo można przypadkowo kliknąć przycisk Usuń z niewłaściwym wierszem. W przyszłym samouczku zobaczymy, jak dodać potwierdzenie po stronie klienta podczas usuwania rekordu.

## <a name="editing-data-with-the-gridview"></a>Edytowanie danych za pomocą widoku GridView

Oprócz usuwania, formant GridView udostępnia również wbudowaną obsługę edycji na poziomie wiersza. Skonfigurowanie widoku GridView do obsługi edycji powoduje dodanie kolumny edycji przycisków. Z punktu widzenia użytkownika końcowego kliknięcie przycisku Edytuj w wierszu powoduje, że ten wiersz można edytować, przełączając komórki do pól tekstowych zawierających istniejące wartości i zastępując przycisk Edytuj za pomocą przycisków Aktualizuj i Anuluj. Po wprowadzeniu żądanych zmian użytkownik końcowy może kliknąć przycisk Aktualizuj, aby zatwierdzić zmiany, lub przycisk Anuluj, aby je odrzucić. W obu przypadkach po kliknięciu przycisku Aktualizuj lub Anuluj polecenie GridView powraca do stanu sprzed edycji.

W naszej perspektywie jako deweloper strony, gdy użytkownik końcowy kliknie przycisk Edytuj dla konkretnego wiersza, odświeżenie zostanie zakończone, a w widoku GridView wykonywane są następujące czynności:

1. Właściwość `EditItemIndex` GridView zostanie przypisana do indeksu wiersza, którego przycisk edycji został kliknięty
2. Element GridView ponownie wiąże się z elementem ObjectDataSource, wywołując jego metodę `Select()`
3. Indeks wiersza, który pasuje do `EditItemIndex` jest renderowany w trybie edycji. W tym trybie przycisk Edytuj jest zamieniany na przyciski Update i Cancel i BoundFields, których właściwości `ReadOnly` są fałszywe (domyślnie) są renderowane jako kontrolki sieci Web TextBox, których właściwości `Text` są przypisane do wartości pól danych.

W tym momencie Adiustacja jest zwracana do przeglądarki, co umożliwia użytkownikowi końcowemu wprowadzanie wszelkich zmian do danych wiersza. Gdy użytkownik kliknie przycisk Aktualizuj, nastąpi odświeżenie, a w obszarze GridView wykonywane są następujące czynności:

1. Wartości `UpdateParameters` elementu ObjectDataSource są przypisywane wartości wprowadzone przez użytkownika końcowego do interfejsu edycji GridView
2. Metoda `Update()` elementu ObjectDataSource jest wywoływana, aktualizowanie określonego rekordu
3. Element GridView ponownie wiąże się z elementem ObjectDataSource, wywołując jego metodę `Select()`

Wartości klucza podstawowego przypisane do `UpdateParameters` w kroku 1 pochodzą z wartości określonych we właściwości `DataKeyNames`, natomiast wartości klucza niepodstawowe pochodzą z tekstu w kontrolkach sieci Web w postaci pola tekstowego edytowanego wiersza. Podobnie jak w przypadku usuwania, ważne jest, aby Właściwość `DataKeyNames` GridView była prawidłowo ustawiona. W przeciwnym razie `UpdateParameters` wartość klucza podstawowego zostanie przypisana `null` wartość w kroku 1, co z kolei nie spowoduje żadnych zaktualizowanych rekordów w kroku 2.

Funkcję edytowania można aktywować, zaznaczając pole wyboru Włącz edycję w tagu inteligentnym GridView.

![Zaznacz pole wyboru Włącz edycję](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Ilustracja 15**. Zaznacz pole wyboru Włącz edytowanie

Zaznaczenie pola wyboru Włącz edycję spowoduje dodanie CommandField (w razie potrzeby) i ustawienie jego właściwości `ShowEditButton` na `true`. Jak wydaliśmy wcześniej, CommandField zawiera kilka `ShowXButton` właściwości, które wskazują, która seria przycisków jest wyświetlana w CommandField. Zaznaczenie pola wyboru Włącz edycję powoduje dodanie właściwości `ShowEditButton` do istniejącej CommandField:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

To wszystko, co jest konieczne do dodania obsługi edycji podstawowe. Jako Figure16 pokazuje, że interfejs edytowania jest raczej surowy, każdy BoundField, którego właściwość `ReadOnly` jest ustawiona na `false` (wartość domyślna) jest renderowany jako pole tekstowe. Obejmuje to pola, takie jak `CategoryID` i `SupplierID`, które są kluczami do innych tabel.

[![kliknięciu przycisku Edytuj Chai, wyświetla wiersz w trybie edycji](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Rysunek 16**: kliknięcie przycisku Edytuj Chai s wyświetla wiersz w trybie edycji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))

Oprócz monitowania użytkowników o bezpośrednie edytowanie wartości kluczy obcych, interfejs interfejsu edycji nie jest w następujący sposób:

- Jeśli użytkownik wprowadzi `CategoryID` lub `SupplierID`, które nie istnieją w bazie danych, `UPDATE` narusza ograniczenia klucza obcego, powodując wystąpienie wyjątku.
- Interfejs edycji nie zawiera żadnych walidacji. Jeśli nie podano wymaganej wartości (takiej jak `ProductName`), lub wprowadź wartość ciągu, w której oczekiwana jest wartość liczbowa (na przykład wprowadzenie "zbyt dużo!") do `UnitPrice` pole tekstowe) zostanie zgłoszony wyjątek. W przyszłym samouczku przedstawiono sposób dodawania kontrolek weryfikacji do interfejsu użytkownika do edycji.
- Obecnie *wszystkie* pola produktu, które nie są tylko do odczytu, muszą być uwzględnione w widoku GridView. Jeśli firma Microsoft musiała usunąć pole z widoku GridView, powiedz `UnitPrice`, gdy aktualizujesz dane, w widoku GridView nie ustawił `UnitPrice` `UpdateParameters` wartości, co spowoduje zmianę `UnitPrice` rekordu bazy danych na wartość `NULL`. Podobnie, jeśli wymagane pole, takie jak `ProductName`, zostanie usunięte z widoku GridView, aktualizacja zakończy się niepowodzeniem z tą samą "*kolumną" ProductName "nie zezwala*na powyższy wyjątek null".
- Formatowanie interfejsu edycji pozostawia potrzebną dużo. `UnitPrice` jest pokazywana z czterema miejscami dziesiętnymi. W idealnym przypadku wartości `CategoryID` i `SupplierID` będą zawierać kontrolek DropDownList, które zawierają listę kategorii i dostawców w systemie.

Są to wszystkie niedociągnięcia, z którymi będziemy korzystać na bieżąco, ale zostaną rozkierowane w przyszłości samouczków.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Wstawianie, edytowanie i usuwanie danych za pomocą widoku DetailsView

Jak widać we wcześniejszych samouczkach, formant DetailsView wyświetla jeden rekord jednocześnie i, podobnie jak GridView, umożliwia edytowanie i usuwanie aktualnie wyświetlanego rekordu. Zarówno środowisko użytkownika końcowego z edytowaniem, jak i usuwaniem elementów z widoku DetailsView, a przepływ pracy ze strony ASP.NET jest taki sam jak w widoku GridView. Gdzie Widok DetailsView różni się od GridView, zapewnia również wbudowaną obsługę wstawiania.

Aby zademonstrować możliwości modyfikacji danych w widoku GridView, Zacznij od dodania widoku DetailsView do strony `Basics.aspx` nad istniejącym elementem GridView i powiąż go z istniejącym elementem ObjectDataSource za pomocą tagu inteligentnego DetailsView. Następnie wyczyść właściwości `Height` i `Width` widoku DetailsView, a następnie zaznacz opcję Włącz stronicowanie z tagu inteligentnego. Aby włączyć obsługę edycji, wstawiania i usuwania, po prostu zaznacz pola wyboru Włącz edytowanie, Włącz wstawianie i Włącz usuwanie w tagu inteligentnym.

![Skonfiguruj DetailsView, aby obsługiwał edytowanie, wstawianie i usuwanie](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Ilustracja 17**. Konfigurowanie widoku DetailsView do obsługi edycji, wstawiania i usuwania

Podobnie jak w przypadku widoku GridView Dodawanie, wstawianie lub usuwanie pomocy technicznej dodaje CommandField do widoku DetailsView, jak pokazano w następującej składni deklaracyjnej:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Należy pamiętać, że dla widoku DetailsView CommandField domyślnie pojawia się na końcu kolekcji Columns. Ponieważ pola DetailsView są renderowane jako wiersze, CommandField pojawia się jako wiersz z przyciskami Wstaw, Edytuj i Usuń w dolnej części widoku DetailsView.

[![skonfigurować DetailsView, aby obsługiwał edytowanie, wstawianie i usuwanie](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Ilustracja 18**. Konfigurowanie widoku DetailsView do obsługi edycji, wstawiania i usuwania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))

Kliknięcie przycisku Usuń powoduje uruchomienie tej samej sekwencji zdarzeń, co w widoku GridView: ogłaszanie zwrotne; a następnie DetailsView wypełnia `DeleteParameters` elementu ObjectDataSource na podstawie wartości `DataKeyNames`; i wykonane z wywołaniem metody `Delete()` elementu ObjectDataSource, która faktycznie usuwa produkt z bazy danych. Edytowanie w widoku DetailsView działa również tak samo jak w widoku GridView.

Dla użytkownika końcowego zostanie wyświetlony nowy przycisk, który po kliknięciu spowoduje renderowanie widoku DetailsView w trybie wstawiania. Za pomocą opcji "Wstaw tryb" nowy przycisk jest zastępowany przyciskami Wstaw i Anuluj i tylko te BoundFields, których właściwość `InsertVisible` jest ustawiona na `true` (wartość domyślna) są wyświetlane. Te pola danych zidentyfikowane jako pola autoprzyrostu, takie jak `ProductID`, mają [Właściwość InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) ustawioną na `false` podczas wiązania widoku DetailsView ze źródłem danych za pomocą tagu inteligentnego.

W przypadku powiązania źródła danych z elementem DetailsView za pomocą tagu inteligentnego program Visual Studio ustawia właściwość `InsertVisible` na `false` tylko dla pól o autoprzyrostie. Pola tylko do odczytu, takie jak `CategoryName` i `SupplierName`, będą wyświetlane w interfejsie użytkownika "Wstaw tryb", chyba że ich Właściwość `InsertVisible` jest jawnie ustawiona na `false`. Poświęć chwilę, aby ustawić te dwa pola `InsertVisible` właściwości na `false`, za pomocą składni deklaracyjnej DetailsView lub łącza Edytuj pola w tagu inteligentnym. Rysunek 19 przedstawia Ustawianie właściwości `InsertVisible` do `false` przez kliknięcie linku Edytuj pola.

[![Northwind Traders teraz oferuje Acme herbata](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Ilustracja 19**. firma Northwind Traders oferuje teraz Acme herbata ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))

Po ustawieniu właściwości `InsertVisible` Wyświetl stronę `Basics.aspx` w przeglądarce i kliknij przycisk Nowy. Ilustracja 20 przedstawia DetailsView, gdy dodajesz nowe napoje, herbata (Acme) do naszej linii produktów.

[![Northwind Traders teraz oferuje Acme herbata](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Ilustracja 20**. firma Northwind Traders oferuje teraz Acme herbata ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))

Po wprowadzeniu szczegółów dotyczących herbaty i kliknięciu przycisku Wstaw zostanie wygrane ogłaszanie zwrotne, a nowy rekord zostanie dodany do tabeli bazy danych `Products`. Ponieważ ten DetailsView wyświetla listę produktów w kolejności, w których istnieją w tabeli bazy danych, musimy być stroną ostatniego produktu w celu wyświetlenia nowego produktu.

[![szczegóły herbata](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Ilustracja 21**. szczegóły dotyczące herbata ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))

> [!NOTE]
> [Właściwość CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) elementu DetailsView wskazuje wyświetlany interfejs i może być jedną z następujących wartości: `Edit`, `Insert`lub `ReadOnly`. [Właściwość DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) wskazuje tryb, do którego funkcja DetailsView wraca po zakończeniu edycji lub wstawiania i jest przydatna do wyświetlania widoku DetailsView, który jest trwale w trybie edycji lub wstawiania.

Punkt i kliknij pozycję Wstawianie i edytowanie możliwości widoku DetailsView z tych samych ograniczeń co GridView: użytkownik musi wprowadzić istniejące `CategoryID` i `SupplierID` wartości za pomocą pola tekstowego; Interfejs nie ma żadnej logiki walidacji; wszystkie pola produktu, które nie zezwalają na wartości `NULL` lub nie mają wartości domyślnej określonej na poziomie bazy danych, muszą być zawarte w interfejsie wstawiania i tak dalej.

Techniki, które sprawdzimy, aby rozszerzać i rozszerzać interfejs edytowania GridView w przyszłości, można zastosować do edycji i wstawiania interfejsów w kontrolce DetailsView.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Używanie widoku FormView dla bardziej elastycznego interfejsu użytkownika modyfikującego dane

Widok FormView oferuje wbudowaną obsługę wstawiania, edytowania i usuwania danych, ale ponieważ używa szablonów zamiast pól, które nie są dodawane do BoundFields lub CommandField używanych przez kontrolki GridView i DetailsView do dostarczania danych Interfejs modyfikacji. Zamiast tego interfejs ten służy do kontroli sieci Web służących do zbierania danych wejściowych użytkownika podczas dodawania nowego elementu lub edytowania istniejącego, a także do odpowiednich szablonów przy użyciu przycisków nowe, Edytuj, Usuń, Wstaw, Aktualizuj i Anuluj. Na szczęście program Visual Studio automatycznie utworzy interfejs niezbędny podczas wiązania widoku FormView ze źródłem danych za pomocą listy rozwijanej w tagu inteligentnym.

Aby zilustrować te techniki, Zacznij od dodania widoku FormView do strony `Basics.aspx` i, w tagu inteligentnym FormView, powiązać go z elementem ObjectDataSource już utworzonym. Spowoduje to wygenerowanie `EditItemTemplate`, `InsertItemTemplate`i `ItemTemplate` dla FormView z kontrolkami sieci Web w postaci pola tekstowego służących do zbierania kontrolek sieci Web danych wejściowych i przycisków na potrzeby nowych, Edytuj, Usuń, Wstaw, Aktualizuj i Anuluj. Ponadto Właściwość `DataKeyNames` FormView jest ustawiona na pole klucza podstawowego (`ProductID`) obiektu zwróconego przez element ObjectDataSource. Na koniec zaznacz opcję Włącz stronicowanie w tagu inteligentnym FormView.

Poniżej przedstawiono znaczniki deklaratywne dla `ItemTemplate` FormView po powiązaniu FormView z elementem ObjectDataSource. Domyślnie każde pole produkt niebędące wartością logiczną jest powiązane z właściwością `Text` kontrolki sieci Web etykiety, natomiast każde pole wartości logicznej (`Discontinued`) jest powiązane z właściwością `Checked` wyłączonej kontrolki sieci Web CheckBox. Aby przyciski nowe, Edytuj i Usuń powodowały wyzwalanie niektórych zachowań FormView po kliknięciu, konieczne jest, aby ich wartości `CommandName` były odpowiednio ustawiane na `New`, `Edit`i `Delete`.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

Ilustracja 22 przedstawia `ItemTemplate` w widoku FormView oglądanym za pomocą przeglądarki. Każde pole produkt jest wyświetlane z przyciskami nowe, Edytuj i Usuń u dołu.

[![defaut FormView ItemTemplate wyświetla listę każdego pola produktu wraz z przyciskami nowe, Edytuj i Usuń](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Ilustracja 22**: defaut `ItemTemplate` FormView wyświetla listę każdego pola produktu wraz z przyciskami nowe, Edytuj i Usuń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))

Podobnie jak w przypadku widoku GridView i DetailsView, kliknięcie przycisku Usuń lub dowolnego przycisku, element LinkButton lub kliknięto element ImageButton, którego właściwość `CommandName` jest ustawiona na wartość Delete powoduje ogłoszenie zwrotne, wypełnia `DeleteParameters` elementu ObjectDataSource na podstawie wartości `DataKeyNames` FormView i wywołuje metodę `Delete()` elementu ObjectDataSource.

Gdy przycisk Edytuj zostanie kliknięty, a dane zostaną powiązane z `EditItemTemplate`, który jest odpowiedzialny za renderowanie interfejsu edycji. Ten interfejs zawiera kontrolki sieci Web służące do edytowania danych wraz z przyciskami Aktualizuj i Anuluj. Domyślny `EditItemTemplate` generowany przez program Visual Studio zawiera etykietę dla wszystkich pól autoprzyrostu (`ProductID`), pole tekstowe dla każdego pola wartości innej niż logiczna oraz pole wyboru dla każdego pola wartości logicznej. To zachowanie jest bardzo podobne do wygenerowanej automatycznie BoundFields w kontrolkach GridView i DetailsView.

> [!NOTE]
> Jeden mały problem z autogeneracjam FormView `EditItemTemplate` to renderuje kontrolki sieci Web typu TextBox dla tych pól, które są tylko do odczytu, na przykład `CategoryName` i `SupplierName`. Wkrótce zobaczymy, jak to zrobić.

Kontrolki TextBox w `EditItemTemplate` mają swoją właściwość `Text` powiązaną z wartością odpowiadającego im pola danych przy użyciu *dwukierunkowego wiązania z danymi*. Dwukierunkowa wartość DataBinding, wskazywana przez `<%# Bind("dataField") %>`, wykonuje powiązania między danymi podczas wiązania danych z szablonem i podczas wypełniania parametrów elementu ObjectDataSource do wstawiania lub edytowania rekordów. Oznacza to, że po kliknięciu przez użytkownika przycisku Edytuj w `ItemTemplate`Metoda `Bind()` Zwraca określoną wartość pola danych. Po wprowadzeniu zmian przez użytkownika i kliknięciu przycisku Aktualizuj wartości, które odpowiadają polom danych określonym przy użyciu `Bind()` są stosowane do `UpdateParameters`elementu ObjectDataSource. Alternatywnie jednokierunkowa DataBinding, oznaczona przez `<%# Eval("dataField") %>`, pobiera tylko wartości pól danych podczas wiązania danych z szablonem i *nie zwraca wartości* wprowadzonych przez użytkownika do parametrów źródła danych na stronie ogłaszania zwrotnego.

Poniższe znaczniki deklaratywne pokazują `EditItemTemplate`FormView. Należy zauważyć, że metoda `Bind()` jest używana w składni DataBinding w tym miejscu, a kontrolki sieci Web Aktualizuj i Anuluj mają odpowiednio ustawione właściwości `CommandName`.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Nasze `EditItemTemplate`, w tym momencie, spowodują zgłoszenie wyjątku, jeśli spróbujemy go użyć. Problem polega na tym, że pola `CategoryName` i `SupplierName` są renderowane jako kontrolki sieci Web TextBox w `EditItemTemplate`. Musimy zmienić te pola tekstowe, aby etykietować lub usuwać je całkowicie. Po prostu usuń je całkowicie z `EditItemTemplate`.

Rysunek 23 pokazuje FormView w przeglądarce po kliknięciu przycisku Edytuj dla Chai. Należy zauważyć, że pola `SupplierName` i `CategoryName` wyświetlane w `ItemTemplate` nie są już obecne, ponieważ po prostu zostały usunięte z `EditItemTemplate`. Po kliknięciu przycisku Aktualizuj widok FormView przechodzi przez tę samą sekwencję kroków jak kontrolki GridView i DetailsView.

[![domyślnie EditItemTemplate pokazuje każde pole edytowanego produktu jako pole tekstowe lub pole wyboru](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Ilustracja 23**. domyślnie `EditItemTemplate` pokazuje każde pole edytowanego produktu jako pole tekstowe lub CheckBox ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))

Gdy klikniesz przycisk Wstaw, `ItemTemplate`e ogłaszania zwrotnego. Jednak żadne dane nie są powiązane z formantem FormView, ponieważ jest dodawany nowy rekord. Interfejs `InsertItemTemplate` zawiera kontrolki sieci Web służące do dodawania nowego rekordu wraz z przyciskami Wstaw i Anuluj. Domyślny `InsertItemTemplate` generowany przez program Visual Studio zawiera pole tekstowe dla każdego pola wartości innej niż logiczna i pole wyboru dla każdego pola wartości logicznej, podobnie jak automatycznie wygenerowany interfejs `EditItemTemplate`. Kontrolki TextBox mają swoją właściwość `Text` powiązaną z wartością odpowiadającego im pola danych, korzystając z dwukierunkowego wiązania z danymi.

Poniższe znaczniki deklaratywne pokazują `InsertItemTemplate`FormView. Należy zauważyć, że metoda `Bind()` jest używana w składni DataBinding w tym miejscu, a kontrolki sieci Web przycisk INSERT i Cancel mają odpowiednio ustawione właściwości `CommandName`.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Istnieje subtlety z automatyczne generowaniem `InsertItemTemplate`w widoku FormView. W odniesieniu do kontrolek sieci Web TextBox są tworzone nawet dla tych pól, które są tylko do odczytu, takich jak `CategoryName` i `SupplierName`. Podobnie jak w przypadku `EditItemTemplate`należy usunąć te pola tekstowe z `InsertItemTemplate`.

Rysunek 24 przedstawia formant FormView w przeglądarce podczas dodawania nowego produktu, kawy xyz. Należy zauważyć, że pola `SupplierName` i `CategoryName` wyświetlane w `ItemTemplate` nie są już obecne, ponieważ zostały po prostu usunięte. Po kliknięciu przycisku Wstaw kontrolka FormView przechodzi przez tę samą sekwencję kroków co formant DetailsView, dodając nowy rekord do tabeli `Products`. Rysunek 25 przedstawia szczegóły produktu kawy xyz w widoku FormView po jego wstawieniu.

[![InsertItemTemplate wymusza Wstawianie interfejsu przez FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Ilustracja 24**. `InsertItemTemplate` określa interfejs wstawiania w widoku FormView (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))

[![szczegóły dotyczące nowego produktu, kawy XYZ, są wyświetlane w widoku FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Ilustracja 25**. szczegóły nowego produktu, kawy XYZ, są wyświetlane w widoku FormView (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))

Oddzielając interfejsy tylko do odczytu, edycji i wstawiania do trzech oddzielnych szablonów, FormView pozwala na dokładniejszy stopień kontroli nad interfejsami niż w widoku DetailsView i GridView.

> [!NOTE]
> Podobnie jak w widoku DetailsView Właściwość `CurrentMode` FormView wskazuje interfejs wyświetlany, a jego właściwość `DefaultMode` wskazuje tryb, do którego formant FormView wraca po zakończeniu edycji lub wstawiania.

## <a name="summary"></a>Podsumowanie

W tym samouczku oceniono podstawowe informacje dotyczące wstawiania, edytowania i usuwania danych przy użyciu widoku GridView, DetailsView i FormView. Wszystkie trzy z tych kontrolek zapewniają pewien poziom wbudowanych funkcji modyfikacji danych, które mogą być używane bez konieczności pisania jednego wiersza kodu na stronie ASP.NET, dzięki formantom sieci Web danych i elementowi ObjectDataSource. Jednak proste metody i techniki klikania renderują stosunkowo Frail i algorytmiey interfejs użytkownika modyfikacji danych. Aby zapewnić weryfikację, wstrzyknąć wartości programistyczne, bezpiecznie obsłużyć wyjątki, dostosować interfejs użytkownika i tak dalej, musimy zależeć od bevy technik, które zostaną omówione w kolejnych samouczkach.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
