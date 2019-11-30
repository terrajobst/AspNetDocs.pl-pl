---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Omówienie edytowania i usuwania danych w elemencie DataList (C#) | Microsoft Docs
author: rick-anderson
description: Gdy element DataList nie ma wbudowanych funkcji edytowania i usuwania, w tym samouczku zobaczymy, jak utworzyć element DataList obsługujący edytowanie i usuwanie...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 481c9a14b1ebfe36ffcddd0237701bc04266e393
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74629534"
---
# <a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Omówienie edytowania i usuwania danych w elemencie DataList (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) lub [Pobierz plik PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Gdy element DataList nie ma wbudowanych funkcji edytowania i usuwania, w tym samouczku zobaczymy, jak utworzyć element DataList, który obsługuje edytowanie i usuwanie danych źródłowych.

## <a name="introduction"></a>Wprowadzenie

Samouczek dotyczący [wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) znajduje się w temacie jak wstawiać, aktualizować i usuwać dane przy użyciu architektury aplikacji, elementu ObjectDataSource oraz formantów GridView, DetailsView i FormView. Element ObjectDataSource i te trzy kontrolki sieci Web danych, implementując proste interfejsy modyfikacji danych, były przyciągane i polegające wyłącznie na oznaczeniu pola wyboru z tagu inteligentnego. Nie trzeba pisać kodu.

Niestety, element DataList nie ma wbudowanej edycji i usuwania funkcji, które są związane z formantem GridView. Ta brakująca funkcja jest zależna od faktu, że element DataList jest Relic z poprzedniej wersji ASP.NET, gdy kontrolki deklaracyjne źródła danych i strony modyfikujące dane bez kodu są niedostępne. Gdy element DataList w ASP.NET 2,0 nie oferuje takich samych możliwości modyfikacji danych, jak w widoku GridView, możemy użyć technik ASP.NET 1. x w celu uwzględnienia takich funkcji. To podejście wymaga pewnego rodzaju kodu, ale ponieważ zobaczymy w tym samouczku, element DataList ma pewne zdarzenia i właściwości w miejscu do pomocy w tym procesie.

W tym samouczku zobaczymy, jak utworzyć element DataList, który obsługuje edytowanie i usuwanie jego danych źródłowych. W przyszłości samouczki zapoznają się z bardziej zaawansowanymi scenariuszami edycji i usuwania, w tym sprawdzaniem poprawności pola wejściowego, bezpieczne obsługiwanie wyjątków wywoływanych z poziomu warstw dostępu do danych lub logiki biznesowej itd.

> [!NOTE]
> Podobnie jak w przypadku elementu DataList, formant wzmacniania nie ma funkcji pola do wstawiania, aktualizowania lub usuwania. Chociaż można dodać tę funkcję, Właściwość DataList zawiera właściwości i zdarzenia, które nie znajdują się w Wzmacniake, które upraszczają Dodawanie takich funkcji. W związku z tym ten samouczek i przyszłość, które wyglądają na edytowanie i usuwanie, będą skoncentrowane wyłącznie na DataList.

## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Krok 1. Tworzenie stron sieci Web do edycji i usuwania samouczków

Przed rozpoczęciem eksplorowania sposobu aktualizowania i usuwania danych z elementu DataList Poświęć chwilę na utworzenie stron ASP.NET w naszym projekcie witryny sieci Web, które będziemy potrzebować dla tego samouczka i następnych kilku. Zacznij od dodania nowego folderu o nazwie `EditDeleteDataList`. Następnie Dodaj następujące strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Dodaj strony ASP.NET dla samouczków](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Rysunek 1**. dodawanie stron ASP.NET dla samouczków

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `EditDeleteDataList` zawiera listę samouczków w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. W związku z tym należy dodać tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na stronę widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))

Na koniec Dodaj strony jako wpisy do pliku `Web.sitemap`. W szczególności Dodaj następujące znaczniki po raportach wzorzec/szczegóły z elementem DataList i Wzmacniaką `<siteMapNode>`:

[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy do edytowania i usuwania z samouczków elementu DataList.

![Mapa witryny zawiera teraz wpisy dotyczące edycji elementu DataList i usuwania samouczków](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Rysunek 3**. Mapa witryny zawiera teraz wpisy dotyczące edytowania i usuwania elementów DataList

## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Krok 2: badanie technik związanych z aktualizowaniem i usuwaniem danych

Edytowanie i usuwanie danych za pomocą widoku GridView jest tak proste, ponieważ, w obszarze okładek, w uzgodnionych działaniach GridView i ObjectDataSource. Zgodnie z opisem w temacie [badanie zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) samouczka po kliknięciu przycisku Aktualizuj wiersz s, GridView automatycznie przypisuje swoje pola, które używały dwukierunkowego wiązania danych do kolekcji `UpdateParameters` jej elementu ObjectDataSource, a następnie wywołuje tę metodę `Update()` ObjectDataSource s.

Niestety, DataList nie udostępnia żadnej z tych wbudowanych funkcji. Jest to nasza odpowiedzialność, aby upewnić się, że wartości użytkownika są przypisane do parametrów ObjectDataSource s i że metoda `Update()` jest wywoływana. Aby pomóc nam w tym Endeavor, Właściwość DataList udostępnia następujące właściwości i zdarzenia:

- **[Właściwość`DataKeyField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx) w** przypadku aktualizowania lub usuwania musi być w stanie jednoznacznie identyfikować każdy element w elemencie DataList. Ustaw tę właściwość na pole klucz podstawowy wyświetlanych danych. Wykonanie tej operacji spowoduje wypełnienie kolekcji DataList s [`DataKeys`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) określoną wartością `DataKeyField` dla każdego elementu DataList.
- **[Zdarzenie`EditCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)** wyzwalane, gdy kliknięto przycisk, element LinkButton lub kliknięto element imagebutton, którego właściwość `CommandName` ma wartość Edit (Edycja).
- **[Zdarzenie`CancelCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)** wyzwalane, gdy kliknięto przycisk, element LinkButton lub kliknięto element imagebutton, którego właściwość `CommandName` ma wartość Cancel.
- **[Zdarzenie`UpdateCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)** wyzwalane, gdy kliknięto przycisk, element LinkButton lub kliknięto element imagebutton, którego właściwość `CommandName` ma wartość Update.
- **[Zdarzenie`DeleteCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)** wyzwalane, gdy kliknięto przycisk, element LinkButton lub kliknięto element imagebutton, którego właściwość `CommandName` ma wartość DELETE.

Korzystając z tych właściwości i zdarzeń, istnieją cztery podejścia, za pomocą których można aktualizować i usuwać dane z elementu DataList:

1. **Przy użyciu technik ASP.NET 1. x** element DataList istniał przed ASP.NET 2,0 i objectdatasources i mógł całkowicie aktualizować i usuwać dane za pomocą metod programistycznych. Ta technika ditches całkowicie element ObjectDataSource i wymaga, aby dane były powiązane z elementem DataList bezpośrednio z warstwy logiki biznesowej, zarówno w przypadku pobierania danych do wyświetlenia, jak i podczas aktualizowania lub usuwania rekordu.
2. **Przy użyciu pojedynczej kontrolki ObjectDataSource na stronie do zaznaczania, aktualizowania i usuwania** , gdy element DataList nie ma niezależnych funkcji edytowania i usuwania elementów GridView, nie można dodać ich do wypróbujemy. W tym podejściu używamy elementu ObjectDataSource podobnie jak w przykładach GridView, ale należy utworzyć procedurę obsługi zdarzeń dla zdarzenia DataList s `UpdateCommand`, gdzie ustawiamy parametry ObjectDataSource s i wywołują metodę `Update()`.
3. **Przy użyciu formantu ObjectDataSource do zaznaczania, ale aktualizowania i usuwania bezpośrednio względem logiki biznesowej** przy użyciu opcji 2, musimy napisać bit kodu w zdarzeniu `UpdateCommand`, przypisując wartości parametrów i tak dalej. Zamiast tego możemy naklejić przy użyciu elementu ObjectDataSource do wybrania, ale aktualizować i usuwać wywołania bezpośrednio względem LOGIKI biznesowej (podobnie jak w przypadku opcji 1). W mojej opinii aktualizowanie danych przez bezpośrednie połączenie z LOGIKI BIZNESOWEJem prowadzi do bardziej czytelnego kodu niż przypisanie elementu ObjectDataSource s `UpdateParameters` i wywołanie jego metody `Update()`.
4. **Użycie deklaratywnych metod za pośrednictwem wielu elementów ObjectDataSource** wszystkie poprzednie trzy podejścia wymagają bitu kodu. Jeśli tak, nie używaj jak największej składni deklaratywnej, ostatnia opcja polega na dołączeniu wielu elementu ObjectDataSources na stronie. Pierwszy element ObjectDataSource pobiera dane z LOGIKI biznesowej i tworzy powiązanie go z elementu DataList. W przypadku aktualizacji zostanie dodany inny element ObjectDataSource, ale został dodany bezpośrednio w `EditItemTemplate`DataList. Aby włączyć obsługę usuwania, w `ItemTemplate`jest wymagany inny element ObjectDataSource. W tym podejściu te osadzone elementy ObjectDataSource s `ControlParameters`, aby deklaratywnie powiązać parametry ObjectDataSource s z kontrolkami wejściowymi użytkownika (zamiast konieczności ich programistycznego określania w programie obsługi zdarzeń typu DataList s `UpdateCommand`). Takie podejście nadal wymaga bitu kodu, który musi wywoływać osadzone `Update()` i `Delete()` polecenia, ale wymaga znacznie mniejszego niż z innymi trzema podejściami. Minusem tutaj polega na tym, że wiele elementów ObjectDataSource powoduje zapełnienie całej strony, pociągając się z ogólnej czytelności.

Jeśli wymuszono użycie jednego z tych metod, należy wybrać opcję 1, ponieważ zapewnia największą elastyczność i ponieważ początkowo została zaprojektowana w celu uwzględnienia tego wzorca. Element DataList został rozszerzony tak, aby działał z kontrolkami źródła danych ASP.NET 2,0, nie ma wszystkich punktów rozszerzalności ani funkcji kontrolek sieci Web danych oficjalnych ASP.NET 2,0 (GridView, DetailsView i FormView). W przypadku opcji od 2 do 4 nie są one jednakowe.

To i przyszłe samouczki edycji i usuwania będą używać elementu ObjectDataSource do pobierania danych w celu wyświetlenia i bezpośredniego wywołania do LOGIKI biznesowej w celu zaktualizowania i usunięcia danych (opcja 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Krok 3. Dodawanie elementu DataList i Konfigurowanie jego elementu ObjectDataSource

W tym samouczku utworzymy element DataList, który zawiera informacje o produkcie i dla każdego produktu, użytkownik może edytować nazwę i cenę oraz całkowicie usunąć produkt. W szczególności pobieramy rekordy do wyświetlania przy użyciu elementu ObjectDataSource, ale wykonują akcje aktualizacji i usuwania, które są połączone bezpośrednio z LOGIKI biznesowej. Przed zapoznanium się z zaimplementowaniem możliwości edytowania i usuwania do elementu DataList należy najpierw pobrać stronę, aby wyświetlić produkty w interfejsie tylko do odczytu. Ponieważ przebadamy te kroki w poprzednich samouczkach, przejdziemy do nich szybko.

Aby rozpocząć, Otwórz stronę `Basics.aspx` w folderze `EditDeleteDataList` i z widok Projekt Dodaj element DataList do strony. Następnie z tagu inteligentnego DataList s Utwórz nowy element ObjectDataSource. Ponieważ pracujemy z danymi produktu, skonfiguruj ją tak, aby korzystała z klasy `ProductsBLL`. Aby pobrać *wszystkie* produkty, wybierz metodę `GetProducts()` na karcie Wybierz.

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Ilustracja 4**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))

[![zwrócić informacje o produkcie przy użyciu metody getProducts ()](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Rysunek 5**. Zwracanie informacji o produkcie przy użyciu metody `GetProducts()` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))

Element DataList, taki jak GridView, nie jest przeznaczony do wstawiania nowych danych. w związku z tym wybierz opcję (brak) z listy rozwijanej na karcie Wstawianie. Dla kart UPDATE i DELETE można także wybrać opcję (brak), ponieważ aktualizacje i usunięcia będą wykonywane programowo za pomocą LOGIKI biznesowej.

[![upewnij się, że listy rozwijane na kartach Wstaw, Aktualizuj i Usuń są ustawione na (brak)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Ilustracja 6**. Upewnij się, że listy rozwijane na kartach Wstaw, Aktualizuj i Usuń są ustawione na (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))

Po skonfigurowaniu elementu ObjectDataSource kliknij przycisk Zakończ, zwracając do projektanta. Jak widać w poprzednich przykładach podczas kończenia konfiguracji elementów ObjectDataSource, program Visual Studio automatycznie tworzy `ItemTemplate` dla DropDownList, wyświetlając poszczególne pola danych. Zamień ten `ItemTemplate` na taki, który wyświetla tylko nazwę produktu i cenę. Ponadto ustaw właściwość `RepeatColumns` na 2.

> [!NOTE]
> Jak opisano w temacie *Omówienie wstawiania, aktualizowania i usuwania danych* w przypadku modyfikowania danych za pomocą elementu ObjectDataSource nasze architektura wymaga usunięcia właściwości `OldValuesParameterFormatString` z znacznika deklaracyjnego ObjectDataSource (lub zresetowania go do wartości domyślnej, `{0}`). W tym samouczku używamy tylko elementu ObjectDataSource do pobierania danych. W związku z tym nie trzeba modyfikować wartości właściwości "ObjectDataSource s `OldValuesParameterFormatString` (chociaż nie jest to konieczne.

Po zamianie domyślnego `ItemTemplate` DataList z dostosowanym jednym znacznikiem deklaratywnym na stronie powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Poświęć chwilę na wyświetlenie postępu w przeglądarce. Jak pokazano na rysunku nr 7, DataList wyświetla nazwę produktu i cenę jednostkową każdego produktu w dwóch kolumnach.

[![nazwy produktów i ceny są wyświetlane w dwóch kolumnach DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Rysunek 7**. nazwy produktów i ceny są wyświetlane w dwóch kolumnach DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))

> [!NOTE]
> Element DataList ma wiele właściwości, które są wymagane dla procesu aktualizowania i usuwania, a te wartości są przechowywane w stanie widoku. W związku z tym podczas kompilowania elementu DataList, który obsługuje edytowanie lub usuwanie danych, jest ważne, że stan widoku DataList s jest włączony.  
>   
> Czytnik uważni może odwoływać, że udało nam się wyłączyć stan widoku podczas tworzenia edytowalnych widoków GridView, DetailsViews i FormView. Wynika to z faktu, że kontrolki sieci Web ASP.NET 2,0 mogą zawierać *stan sterowania*, który jest utrwalany na ogłaszaniu zwrotnym, takich jak stan widoku, ale uznawany za istotny.

Wyłączenie stanu widoku w widoku GridView powoduje jedynie pominięcie prostych informacji o stanie, ale zachowuje stan kontroli (co obejmuje stan niezbędny do edycji i usuwania). DataList, która została utworzona w przedziale czasu ASP.NET 1. x, nie używa stanu kontroli i dlatego musi mieć włączony stan widoku. Aby uzyskać więcej informacji na temat przeznaczenie kontroli i różnice między Stanami widoku, zobacz stan [kontroli a stan widoku](https://msdn.microsoft.com/library/1whwt1k7.aspx) .

## <a name="step-4-adding-an-editing-user-interface"></a>Krok 4. Dodawanie interfejsu użytkownika edytowania

Formant GridView składa się z kolekcji pól (BoundFields, CheckBoxFields, używanie TemplateField itd.). Pola te mogą dostosować renderowane znaczniki w zależności od ich trybu. Na przykład, gdy w trybie tylko do odczytu, BoundField wyświetla wartość pola danych jako tekst; w trybie edycji renderuje formant sieci Web TextBox, którego właściwość `Text` ma przypisaną wartość pola dane.

Element DataList, z drugiej strony, renderuje swoje elementy przy użyciu szablonów. Elementy tylko do odczytu są renderowane przy użyciu `ItemTemplate`, a elementy w trybie edycji są renderowane za pośrednictwem `EditItemTemplate`. W tym momencie nasza DataList ma tylko `ItemTemplate`. Aby zapewnić obsługę funkcji edycji na poziomie elementu, należy dodać `EditItemTemplate`, która zawiera adiustację, która ma być wyświetlana dla elementu edytowalnego. W tym samouczku będziemy używać kontrolek sieci Web TextBox do edytowania nazwy produktu i ceny jednostkowej.

`EditItemTemplate` można utworzyć deklaratywnie lub za pomocą projektanta (poprzez wybranie opcji Edytuj szablony z taga inteligentnego DataList s). Aby użyć opcji Edytuj szablony, najpierw kliknij link Edytuj szablony w tagu inteligentnym, a następnie wybierz element `EditItemTemplate` z listy rozwijanej.

[![wybrać opcję pracy z elementem DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Ilustracja 8**. poprawność pracy z `EditItemTemplate`ą DataList s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))

Następnie wpisz nazwę produktu: i cenę: a następnie przeciągnij dwie kontrolki TextBox z przybornika do interfejsu `EditItemTemplate` w projektancie. Ustaw pola tekstowe `ID` właściwości na `ProductName` i `UnitPrice`.

[![dodać pola tekstowego dla nazwy produktu i ceny](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Ilustracja 9**. Dodawanie pola tekstowego dla nazwy produktu i ceny ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))

Musimy powiązać odpowiednie wartości pól danych produktu z właściwościami `Text` dwóch pól tekstowych. W polu texttags Tagi inteligentne kliknij łącze Edytuj dane powiązania, a następnie skojarz odpowiednie pole danych z właściwością `Text`, jak pokazano na rysunku 10.

> [!NOTE]
> Po powiązaniu pola danych `UnitPrice` do pola pole tekstowe ceny s `Text` można sformatować go jako wartość walutową (`{0:C}`), numer ogólny (`{0:N}`) lub pozostawić niesformatowany.

![Powiąż pola NazwaProduktu i CenaJednostkowa z właściwościami tekstu pól tekstowych](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Ilustracja 10**. powiązanie pól danych `ProductName` i `UnitPrice` z właściwościami `Text` pól tekstowych

Zwróć uwagę, jak okno dialogowe Edytuj powiązania danych na rysunku nr 10 *nie zawiera pola* wyboru powiązania dwukierunkowego, które jest obecne podczas edytowania elementu TemplateField w widoku GridView lub DetailsView lub szablonu w widoku FormView. Dwukierunkowa funkcja DataBinding umożliwia automatyczne przypisanie wartości wprowadzonej do wejściowej kontrolki sieci Web do odpowiedniego elementu ObjectDataSource s `InsertParameters` lub `UpdateParameters` podczas wstawiania lub aktualizowania danych. Element DataList nie obsługuje dwukierunkowego wiązania danych, ponieważ będzie on widoczny w dalszej części tego samouczka, po wprowadzeniu zmian przez użytkownika i przygotowaniu do jego zaktualizowania, musimy programowo uzyskać dostęp do tych pól tekstowych `Text` właściwości i przekazać ich wartości do odpowiedniej metody `UpdateProduct` w klasie `ProductsBLL`.

Na koniec należy dodać przyciski Aktualizuj i Anuluj do `EditItemTemplate`. Jak pokazano na [wzorcu/szczegółach przy użyciu listy punktowanej rekordów głównych z szczegółowym samouczkiem elementu DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) , gdy zostanie kliknięty przycisk, element LinkButton lub kliknięto element ImageButton, którego właściwość `CommandName` jest ustawiona z poziomu elementu wzmacniak lub DataList, zostanie wywołane zdarzenie wzmacniak lub datalist s `ItemCommand`. Dla elementu DataList, jeśli właściwość `CommandName` jest ustawiona na określoną wartość, również może zostać zgłoszone dodatkowe zdarzenie. Specjalne wartości właściwości `CommandName` obejmują między innymi:

- Anulowanie wywołuje zdarzenie `CancelCommand`
- Edycja wywołuje zdarzenie `EditCommand`
- Aktualizacja podnosi zdarzenie `UpdateCommand`

Należy pamiętać, że zdarzenia te są wywoływane *oprócz* zdarzenia `ItemCommand`.

Dodaj do kontrolek sieci Web `EditItemTemplate` dwa przyciski, dla których `CommandName` jest ustawiona na wartość Update, a pozostałe s ustawione na wartość Cancel. Po dodaniu tych dwóch kontrolek sieci Web Projektant powinien wyglądać podobnie do poniższego:

[![Dodaj przyciski Aktualizuj i Anuluj do EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Ilustracja 11**. Dodaj przyciski Aktualizuj i anuluj do `EditItemTemplate` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))

Z `EditItemTemplate` Dokończ znaczniki deklaratywne DataList, powinny wyglądać podobnie do następujących:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Krok 5. Dodawanie instalacji wodociągowej do trybu edycji

W tym momencie nasza DataList ma interfejs edytowania zdefiniowany za pośrednictwem jego `EditItemTemplate`; Jednak obecnie nie ma żadnego sposobu na odwiedzenie naszej strony przez użytkownika w celu wskazania, że chce on edytować informacje o produkcie. Musimy dodać przycisk Edytuj do każdego produktu, który zostanie kliknięty, renderuje ten element DataList w trybie edycji. Zacznij od dodania przycisku Edytuj do `ItemTemplate`za pomocą projektanta lub deklaratywnie. Należy upewnić się, że właściwość `CommandName` przycisku Edytuj ma być edytowana.

Po dodaniu tego przycisku edycji Poświęć chwilę na wyświetlenie strony za pomocą przeglądarki. W tym przypadku każda lista produktów powinna zawierać przycisk Edytuj.

[![Dodaj przyciski Aktualizuj i Anuluj do EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Ilustracja 12**. Dodaj przyciski Aktualizuj i anuluj do `EditItemTemplate` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))

Kliknięcie przycisku powoduje odświeżenie, ale *nie* spowoduje przełączenia listy produktów do trybu edycji. Aby można było edytować produkt, musimy:

1. Ustaw Właściwość DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) na indeks `DataListItem`, którego przycisk edycji został właśnie kliknięty.
2. Ponownie powiązać dane z właściwością DataList. Po ponownym renderowaniu elementu DataList `DataListItem`, którego `ItemIndex` odpowiada `EditItemIndex` DataList s, będzie renderowany przy użyciu `EditItemTemplate`.

Ponieważ zdarzenie `EditCommand` DataList s jest wyzwalane po kliknięciu przycisku Edytuj, Utwórz `EditCommand` obsługi zdarzeń przy użyciu następującego kodu:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

Procedura obsługi zdarzeń `EditCommand` jest przenoszona w obiekcie typu `DataListCommandEventArgs` jako drugi parametr wejściowy, który zawiera odwołanie do `DataListItem`, którego przycisk edycji został kliknięty (`e.Item`). Program obsługi zdarzeń najpierw ustawia listę DataList s `EditItemIndex` na `ItemIndex` `DataListItem` edytowalnej, a następnie ponownie wiąże dane z właściwością DataList, wywołując metodę `DataBind()` DataList s.

Po dodaniu tego programu obsługi zdarzeń ponownie odwiedź stronę w przeglądarce. Kliknięcie przycisku Edytuj spowoduje teraz edycję klikniętego produktu (patrz rysunek 13).

[![kliknięcie przycisku Edytuj sprawia, że produkt jest edytowalny](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Ilustracja 13**. kliknięcie przycisku Edytuj sprawia, że produkt jest edytowalny ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))

## <a name="step-6-saving-the-user-s-changes"></a>Krok 6. zapisanie zmian użytkownika s

Kliknięcie przycisku Edytuj aktualizację produktu lub Anuluj nie powoduje żadnych operacji w tym momencie. Aby dodać tę funkcję, należy utworzyć programy obsługi zdarzeń dla `UpdateCommand` i zdarzeń `CancelCommand` dla elementu DataList. Zacznij od utworzenia programu obsługi zdarzeń `CancelCommand`, który będzie wykonywany po kliknięciu przycisku "Edytuj produkt s", a jego zadaniem jest zwrócenie elementu DataList do jego stanu sprzed edycji.

Aby element DataList renderować wszystkie elementy w trybie tylko do odczytu, musimy:

1. Ustaw Właściwość DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) na indeks nieistniejącego `DataListItem` indeksu. `-1` jest bezpiecznym wyborem, ponieważ indeksy `DataListItem` zaczynają się w `0`.
2. Ponownie powiązać dane z właściwością DataList. Ponieważ `DataListItem` `ItemIndex` es odpowiada `EditItemIndex`DataList s, cała DataList będzie renderowana w trybie tylko do odczytu.

Te kroki można wykonać przy użyciu następującego kodu programu obsługi zdarzeń:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Za pomocą tego dodatku kliknięcie przycisku Anuluj spowoduje zwrócenie elementu DataList do jego stanu sprzed edycji.

Ostatnim programem obsługi zdarzeń, który musimy wykonać, jest program obsługi zdarzeń `UpdateCommand`. Ten program obsługi zdarzeń musi:

1. Programowe uzyskiwanie dostępu do nazwy i ceny produktu wprowadzonej przez użytkownika oraz `ProductID`edytowanego produktu.
2. Zainicjuj proces aktualizacji, wywołując odpowiednie przeciążenia `UpdateProduct` w klasie `ProductsBLL`.
3. Ustaw Właściwość DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) na indeks nieistniejącego `DataListItem` indeksu. `-1` jest bezpiecznym wyborem, ponieważ indeksy `DataListItem` zaczynają się w `0`.
4. Ponownie powiązać dane z właściwością DataList. Ponieważ `DataListItem` `ItemIndex` es odpowiada `EditItemIndex`DataList s, cała DataList będzie renderowana w trybie tylko do odczytu.

Kroki 1 i 2 są odpowiedzialne za zapisanie zmian użytkownika s; kroki 3 i 4 zwracają wartość DataList do stanu sprzed edycji po zapisaniu zmian i są identyczne z krokami wykonywanymi w programie obsługi zdarzeń `CancelCommand`.

Aby uzyskać zaktualizowaną nazwę i cenę produktu, musimy użyć metody `FindControl` do programistycznego odwoływania się do kontrolek sieci Web TextBox w `EditItemTemplate`. Należy również pobrać wartość `ProductID` edytowanego produktu. Po wstępnym powiązaniu elementu ObjectDataSource z elementem DataList program Visual Studio przypisał Właściwość DataList s `DataKeyField` do wartości klucza podstawowego ze źródła danych (`ProductID`). Tę wartość można następnie pobrać z kolekcji `DataKeys` DataList. Poświęć chwilę, aby upewnić się, że właściwość `DataKeyField` jest rzeczywiście ustawiona na `ProductID`.

Poniższy kod implementuje cztery kroki:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Program obsługi zdarzeń jest uruchamiany przez odczytanie w `ProductID` edytowanego produktu z kolekcji `DataKeys`. Następnie do dwóch pól tekstowych w `EditItemTemplate` są przywoływane odwołania i ich `Text` właściwości przechowywane w zmiennych lokalnych, `productNameValue` i `unitPriceValue`. Używamy metody `Decimal.Parse()`, aby odczytać wartość z pola tekstowego `UnitPrice`, dzięki czemu Jeśli wprowadzona wartość ma symbol waluty, nadal można ją poprawnie przekonwertować na wartość `Decimal`.

> [!NOTE]
> Wartości z `ProductName` i `UnitPrice` textpól są przypisywane tylko do zmiennych productNameValue i unitPriceValue, jeśli właściwości tekstu textOnly mają określoną wartość. W przeciwnym razie wartość `Nothing` jest używana dla zmiennych, które mają wpływ na aktualizację danych za pomocą bazy danych `NULL` wartość. Oznacza to, że nasz kod traktuje puste ciągi do wartości `NULL` bazy danych, co jest domyślnym zachowaniem interfejsu edycji w kontrolkach GridView, DetailsView i FormView.

Po przeczytaniu wartości, Metoda `ProductsBLL` klasy s `UpdateProduct` jest wywoływana, przekazując w nazwę produktu, cenę i `ProductID`. Procedura obsługi zdarzeń kończy się przez zwrócenie elementu DataList do jego stanu sprzed edycji przy użyciu dokładnie takiej samej logiki jak w programie obsługi zdarzeń `CancelCommand`.

Po ukończeniu obsługi zdarzeń `EditCommand`, `CancelCommand`i `UpdateCommand` osoba odwiedzająca może edytować nazwę i cenę produktu. Ilustracje 14-16 Pokaż ten przepływ pracy edycji w akcji.

[![podczas pierwszego odwiedzania strony wszystkie produkty są w trybie tylko do odczytu.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Ilustracja 14**. podczas pierwszego odwiedzania strony wszystkie produkty są w trybie tylko do odczytu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))

[![zaktualizować nazwy produktu lub ceny, kliknij przycisk Edytuj](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Ilustracja 15**. Aby zaktualizować nazwę produktu lub cenę, kliknij przycisk Edytuj ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))

[![po zmianie wartości kliknij przycisk Aktualizuj, aby powrócić do trybu tylko do odczytu.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Ilustracja 16**. po zmianie wartości kliknij przycisk Aktualizuj, aby powrócić do trybu tylko do odczytu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))

## <a name="step-7-adding-delete-capabilities"></a>Krok 7. Dodawanie możliwości usuwania

Kroki dodawania funkcji usuwania do elementu DataList są podobne do tych w przypadku dodawania możliwości edytowania. W krótkim czasie musimy dodać przycisk usuwania do `ItemTemplate`, który po kliknięciu:

1. Odczytuje w odpowiednim `ProductID` produktu za pośrednictwem kolekcji `DataKeys`.
2. Wykonuje operację usuwania, wywołując metodę `DeleteProduct` `ProductsBLL` Class.
3. Ponownie wiąże dane z właściwością DataList.

Niech Rozpocznij, dodając przycisk usuwania do `ItemTemplate`.

Po kliknięciu przycisk, którego `CommandName` ma wartość Edytuj, Aktualizuj lub Anuluj, zgłasza zdarzenie `ItemCommand` DataList s wraz z dodatkowym zdarzeniem (na przykład podczas korzystania z polecenia Edytuj zdarzenie `EditCommand` jest również zgłaszane). Analogicznie, dowolny przycisk, element LinkButton lub kliknięto element ImageButton na elemencie DataList, którego właściwość `CommandName` jest ustawiona na wartość Delete powoduje, że `DeleteCommand` zdarzenie do wyzwolenia (wraz z `ItemCommand`).

Dodaj przycisk usuwania obok przycisku Edytuj w `ItemTemplate`, ustawiając jego właściwość `CommandName` na wartość DELETE. Po dodaniu tego przycisku kontrolki "DataList" `ItemTemplate` składni deklaracyjnej powinna wyglądać następująco:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `DeleteCommand` DataList s przy użyciu następującego kodu:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Kliknięcie przycisku Usuń powoduje odświeżenie i wygenerowanie zdarzenia "`DeleteCommand` DataList s". W programie obsługi zdarzeń dostęp do klikniętej wartości `ProductID` jest uzyskiwany z kolekcji `DataKeys`. Następnie produkt zostanie usunięty przez wywołanie metody `DeleteProduct` `ProductsBLL` klasy s.

Po usunięciu produktu ważna jest konieczność ponownego powiązania danych z elementem DataList (`DataList1.DataBind()`). w przeciwnym razie element DataList będzie nadal pokazywał produkt, który został właśnie usunięty.

## <a name="summary"></a>Podsumowanie

Gdy element DataList nie ma punktu i klikniesz opcję Edytuj i Usuń pomoc techniczną przysługujący GridView, z krótkim bitem kodu, można rozszerzyć, aby uwzględnić te funkcje. W tym samouczku pokazano, jak utworzyć dwukolumnową listę produktów, które mogą zostać usunięte, a których nazwy i ceny można edytować. Dodawanie obsługi edycji i usuwania jest kwestią dołączenia odpowiednich kontrolek sieci Web w `ItemTemplate` i `EditItemTemplate`, tworzenia odpowiednich programów obsługi zdarzeń, odczytywania wartości wprowadzonych przez użytkownika i klucza podstawowego oraz do współdziałania z warstwą logiki biznesowej.

Mimo że dodaliśmy podstawowe możliwości edytowania i usuwania do elementu DataList, nie ma bardziej zaawansowanych funkcji. Na przykład nie istnieje walidacja pola wejściowego — Jeśli użytkownik wprowadzi cenę zbyt kosztowną, wyjątek zostanie wygenerowany przez `Decimal.Parse` przy próbie przekształcenia zbyt kosztownego w `Decimal`. Podobnie, jeśli wystąpi problem z aktualizacją danych w logice biznesowej lub warstwach dostępu do danych, użytkownik zostanie wyświetlony przy użyciu standardowego ekranu błędu. Bez żadnych potwierdzeń w przycisku Usuń, przypadkowe usunięcie produktu jest zbyt mało możliwe.

W przyszłych samouczkach zobaczymy, jak usprawnić Edytowanie środowiska użytkownika.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Zack Kowalski, Krzysztof Pespisa i Randy Schmidt. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](performing-batch-updates-cs.md)
