---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Niestandardowe formatowanie na podstawie danych (VB) | Microsoft Docs
author: rick-anderson
description: Dostosowanie formatu GridView, DetailsView lub FormView na podstawie danych powiązanych z nim można wykonać na wiele sposobów. W tym samouczku będziemy...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 268dc763ef6954903f721a3015daaf10bf9bccb1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74613050"
---
# <a name="custom-formatting-based-upon-data-vb"></a>Niestandardowe formatowanie na podstawie danych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) lub [Pobierz plik PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Dostosowanie formatu GridView, DetailsView lub FormView na podstawie danych powiązanych z nim można wykonać na wiele sposobów. W tym samouczku powiesz się, jak wykonać formatowanie powiązane z danymi za pomocą obsługi zdarzeń powiązanych i RowDataBound.

## <a name="introduction"></a>Wprowadzenie

Wygląd formantów GridView, DetailsView i FormView można dostosować za pomocą wyposażono właściwości związanych z stylem. Właściwości, takie jak `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`i `Height`, podyktują ogólny wygląd renderowanej kontrolki. Właściwości, w tym `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`i inne zezwalają na stosowanie tych samych ustawień stylu do określonych sekcji. Analogicznie, te ustawienia stylu mogą być stosowane na poziomie pola.

W wielu scenariuszach wymagania dotyczące formatowania są zależne od wartości wyświetlanych danych. Na przykład w celu wyszukania uwagi na produkty poza magazynem, raport zawierający informacje o produkcie może ustawić kolor tła na żółty dla tych produktów, których `UnitsInStock` i `UnitsOnOrder` pola są równe 0. Aby wyróżnić tańsze produkty, firma Microsoft może chcieć, że ceny tych produktów są droższe niż $75,00 w postaci pogrubionej czcionki.

Dostosowanie formatu GridView, DetailsView lub FormView na podstawie danych powiązanych z nim można wykonać na wiele sposobów. W tym samouczku zawarto informacje na temat wykonywania formatowania powiązanego z danymi za pomocą `DataBound` i `RowDataBound` obsługi zdarzeń. W następnym samouczku zapoznajemy alternatywne podejście.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Używanie procedury obsługi zdarzeń`DataBound`formantu DetailsView

Gdy dane są powiązane z DetailsView, albo z kontrolki źródła danych lub przez programowe przypisanie danych do właściwości `DataSource` kontrolki i wywołanie jej metody `DataBind()`, wykonywane są następujące sekwencje kroków:

1. Zdarzenie `DataBinding` formantu sieci Web danych.
2. Dane są powiązane z formantem sieci Web danych.
3. Zdarzenie `DataBound` formantu sieci Web danych.

Logikę niestandardową można wstrzyknąć bezpośrednio po krokach 1 i 3 za poorednictwem programu obsługi zdarzeń. Tworząc procedurę obsługi zdarzeń dla zdarzenia `DataBound`, możemy programistycznie określić dane, które zostały powiązane z kontrolką sieci Web danych i dostosować formatowanie odpowiednio do wymagań. Aby zilustrować ten sposób, Utwórz widok DetailsView, który będzie zawierać ogólne informacje o produkcie, ale `UnitPrice` wartość w postaci ***pogrubionej, kursywy,*** jeśli będzie ona przekroczyć $75,00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Krok 1. Wyświetlanie informacji o produkcie w widoku DetailsView

Otwórz stronę `CustomColors.aspx` w folderze `CustomFormatting`, przeciągnij kontrolkę DetailsView z przybornika do projektanta, ustaw jej wartość właściwości `ID` na `ExpensiveProductsPriceInBoldItalic`i powiąż ją z nową kontrolką ObjectDataSource, która wywołuje metodę `ProductsBLL` klasy `GetProducts()`. Szczegółowe kroki w celu osiągnięcia tego celu zostały pominięte w tym miejscu dla zwięzłości, ponieważ zostały one szczegółowo przejrzane w poprzednich samouczkach.

Po powiązaniu elementu ObjectDataSource z DetailsView, poświęć chwilę, aby zmodyfikować listę pól. Wybrano usunięcie `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`i `Discontinued` BoundFields, a następnie zmieniono nazwę i ponownie sformatowano pozostałe BoundFields. Wyczyszczono również ustawienia `Width` i `Height`. Ponieważ w widoku DetailsView jest wyświetlany tylko jeden rekord, musimy włączyć stronicowanie, aby umożliwić użytkownikowi końcowemu wyświetlanie wszystkich produktów. Zrób to, zaznaczając pole wyboru Włącz stronicowanie w tagu inteligentnym DetailsView.

[![rysunek 1: zaznacz pole wyboru Włącz stronicowanie w tagu inteligentnym DetailsView](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Rysunek 1**. rysunek 1: zaznacz pole wyboru Włącz stronicowanie w tagu inteligentnym DetailsView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image3.png))

Po wprowadzeniu tych zmian znacznik DetailsView będzie:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Poświęć chwilę na przetestowanie tej strony w przeglądarce.

[![kontrolka DetailsView wyświetla jeden produkt w danym momencie](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Rysunek 2**. kontrolka DetailsView wyświetla jeden produkt jednocześnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image6.png))

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 2. programistyczne Określanie wartości danych w procedurze obsługi zdarzeń powiązanej z danymi

Aby wyświetlić cenę w postaci pogrubionej, kursywy dla tych produktów, których `UnitPrice` wartość przekracza $75,00, musi być w stanie programowo określić wartość `UnitPrice`. Dla widoku DetailsView można to osiągnąć w obsłudze zdarzeń `DataBound`. Aby utworzyć procedurę obsługi zdarzeń, kliknij pozycję DetailsView w projektancie, a następnie przejdź do okno Właściwości. Naciśnij klawisz F4, aby go przywrócić, jeśli nie jest widoczny, lub przejdź do menu Widok i wybierz opcję menu okno właściwości. Na okno Właściwości kliknij ikonę błyskawicy, aby wyświetlić listę zdarzeń DetailsView. Następnie kliknij dwukrotnie zdarzenie `DataBound` lub wpisz nazwę programu obsługi zdarzeń, który chcesz utworzyć.

![Utwórz procedurę obsługi zdarzeń dla zdarzenia powiązanego z danymi](custom-formatting-based-upon-data-vb/_static/image7.png)

**Rysunek 3**. Tworzenie programu obsługi zdarzeń dla zdarzenia `DataBound`

> [!NOTE]
> Możesz również utworzyć procedurę obsługi zdarzeń ze strony kodowej ASP.NET. W górnej części strony znajdują się dwie listy rozwijane. Wybierz obiekt z listy rozwijanej po lewej stronie i zdarzenie, dla którego chcesz utworzyć procedurę obsługi, z listy rozwijanej po prawej stronie, a program Visual Studio automatycznie utworzy odpowiednią procedurę obsługi zdarzeń.

Wykonanie tej czynności spowoduje automatyczne utworzenie programu obsługi zdarzeń i przejście do fragmentu kodu, w którym został dodany. W tym momencie zobaczysz:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Dostęp do danych w widoku DetailsView można uzyskać za pomocą właściwości `DataItem`. Odwołaj, że są one powiązane z naszymi kontrolkami, do silnie typu DataTable, która składa się z kolekcji instancji DataRow o jednoznacznie określonym typie. Gdy element DataTable jest powiązany z elementem DetailsView, pierwszy element DataRow w elemencie DataTable jest przypisany do właściwości `DataItem` DetailsView. Do właściwości `DataItem` jest przypisywany obiekt `DataRowView`. Możemy użyć właściwości `Row` `DataRowView`, aby uzyskać dostęp do podstawowego obiektu DataRow, który jest w rzeczywistości wystąpieniem `ProductsRow`. Po tym wystąpieniu `ProductsRow` możemy podjąć nasze decyzje przez sprawdzenie wartości właściwości obiektu.

Poniższy kod ilustruje sposób ustalania, czy wartość `UnitPrice` powiązana z formantem DetailsView jest większa niż $75,00:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Ponieważ `UnitPrice` może mieć wartość `NULL` w bazie danych, najpierw sprawdzimy, czy przed uzyskaniem dostępu do właściwości `UnitPrice` `ProductsRow`nie będziemy omawiać wartości `NULL`. Ta kontrola jest ważna, ponieważ jeśli próbujemy uzyskać dostęp do właściwości `UnitPrice`, gdy ma ona `NULL` wartość, obiekt `ProductsRow` zgłosi [wyjątek StrongTypingException](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).

## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Krok 3. formatowanie wartości CenaJednostkowa w widoku DetailsView

W tym momencie możemy określić, czy wartość `UnitPrice` powiązana z elementem DetailsView ma wartość przekraczającą $75,00, ale jeszcze więcej zapoznaj się z tym, jak programowo dostosować formatowanie widoku DetailsView. Aby zmodyfikować formatowanie całego wiersza w widoku DetailsView, program programowo uzyskuje dostęp do wiersza przy użyciu `DetailsViewID.Rows(index)`; Aby zmodyfikować określoną komórkę, użyj `DetailsViewID.Rows(index).Cells(index)`. Gdy mamy odwołanie do wiersza lub komórki, możemy dostosować jego wygląd, ustawiając jego właściwości powiązane z stylem.

Programowe uzyskiwanie dostępu do wiersza wymaga, aby znać indeks wiersza, który zaczyna się od 0. Wiersz `UnitPrice` jest piątym wierszem w widoku DetailsView, dając go indeks 4 i programistycznie dostępny przy użyciu `ExpensiveProductsPriceInBoldItalic.Rows(4)`. W tym momencie zawartość całego wiersza może być wyświetlana w postaci pogrubionej, kursywy przy użyciu następującego kodu:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Jednak spowoduje to *zarówno* etykietę (cenę), jak i wartość kursywy. Jeśli chcemy wprowadzić wartość pogrubioną i kursywą, musimy zastosować to formatowanie do drugiej komórki w wierszu, co można wykonać przy użyciu następujących elementów:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Ze względu na to, że nasze samouczki wykorzystują arkusze stylów, aby zachować czyste rozdzielenie między renderowanymi informacjami dotyczącymi znaczników i stylów, zamiast ustawiania określonych właściwości stylu, jak pokazano powyżej, zamiast tego użyj klasy CSS. Otwórz arkusz stylów `Styles.css` i Dodaj nową klasę CSS o nazwie `ExpensivePriceEmphasis` z następującą definicją:

[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Następnie w programie obsługi zdarzeń `DataBound` ustaw właściwość `CssClass` komórki na `ExpensivePriceEmphasis`. Poniższy kod pokazuje program obsługi zdarzeń `DataBound` w całości:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Podczas wyświetlania Chai, które zawierają koszty mniejsze niż $75,00, Cena jest wyświetlana w normalnej czcionce (zobacz rysunek 4). Jednak podczas wyświetlania Mishi Kobe niku, który ma cenę $97,00, Cena jest wyświetlana w postaci pogrubionej, kursywy (patrz rysunek 5).

[Ceny ![mniejsze niż $75,00 są wyświetlane w normalnej czcionce](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Ilustracja 4**. ceny mniejsze niż $75,00 są wyświetlane w normalnej czcionce ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image10.png))

[Ceny ![ych kosztownych produktów są wyświetlane w postaci pogrubionej, kursywy](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Rysunek 5**. ceny kosztownych produktów są wyświetlane w postaci pogrubionej, kursywy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image13.png))

## <a name="using-the-formview-controlsdataboundevent-handler"></a>Korzystanie z programu obsługi zdarzeń`DataBound`kontrolki FormView

Kroki określania danych bazowych powiązanych z elementem FormView są takie same jak w przypadku tworzenia zdarzeń `DataBound` w widoku DetailsView, rzutowania właściwości `DataItem` na odpowiedni typ obiektu powiązanego z kontrolką i określić, jak można wykonać operację. Widok FormView i DetailsView różnią się jednak w sposobie aktualizowania wyglądu interfejsu użytkownika.

Formant FormView nie zawiera żadnych BoundFields i dlatego nie ma kolekcji `Rows`. Zamiast tego, FormView składa się z szablonów, które mogą zawierać kombinację statycznych HTML, kontrolek sieci Web i składni wiązania danych. Dostosowanie stylu FormView zazwyczaj obejmuje dostosowanie stylu co najmniej jednej kontrolki sieci Web w ramach szablonów FormView.

Aby to zilustrować, użyjmy FormView, aby wyświetlić listę produktów, takich jak w poprzednim przykładzie, ale ten czas będzie wyświetlał tylko nazwę produktu i jednostki w magazynie z jednostkami w magazynie w kolorze czerwonym, jeśli jest ona mniejsza lub równa 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Krok 4. Wyświetlanie informacji o produkcie w widoku FormView

Dodaj element FormView do strony `CustomColors.aspx` poniżej widoku DetailsView i ustaw jej Właściwość `ID` na `LowStockedProductsInRed`. Powiąż element FormView z kontrolką elementu ObjectDataSource utworzonego w poprzednim kroku. Spowoduje to utworzenie `ItemTemplate`, `EditItemTemplate`i `InsertItemTemplate` dla widoku FormView. Usuń `EditItemTemplate` i `InsertItemTemplate` i Uprość `ItemTemplate` w celu uwzględnienia tylko wartości `ProductName` i `UnitsInStock`, z których każdy we własnych odpowiednio nazwanych kontrolkach etykiet. Podobnie jak w przypadku widoku DetailsView z wcześniejszego przykładu, zaznacz pole wyboru Włącz stronicowanie w tagu inteligentnym FormView.

Po wprowadzeniu zmian znacznik FormView powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Należy pamiętać, że `ItemTemplate` zawiera:

- **Statyczny kod HTML** tekst "Product:" i "jednostki w magazynie:" wraz z `<br />` i `<b>` elementów.
- **Formant sieci Web kontroluje** dwie kontrolki etykiet, `ProductNameLabel` i `UnitsInStockLabel`.
- **Składnia wiązania** danych jest składnią `<%# Bind("ProductName") %>` i `<%# Bind("UnitsInStock") %>`, która przypisuje wartości z tych pól do właściwości `Text` formantów etykiet.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 5. programistyczne Określanie wartości danych w programie obsługi zdarzeń powiązanych z danymi

Po zakończeniu adiustacji FormView następnym krokiem jest programowe ustalenie, czy wartość `UnitsInStock` jest mniejsza niż lub równa 10. Jest to realizowane w taki sam sposób, jak w widoku DetailsView. Zacznij od utworzenia obsługi zdarzeń dla zdarzenia `DataBound` FormView.

![Tworzenie procedury obsługi zdarzeń powiązanej z danymi](custom-formatting-based-upon-data-vb/_static/image14.png)

**Ilustracja 6**. Tworzenie procedury obsługi zdarzeń `DataBound`

W obsłudze zdarzeń Właściwość `DataItem` FormView jest rzutowana na wystąpienie `ProductsRow` i określa, czy wartość `UnitsInPrice` jest taka, że musi ona być wyświetlana w czerwoną czcionką.

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Krok 6. formatowanie kontrolki etykiety UnitsInStockLabel w ItemTemplate FormView

Ostatnim krokiem jest sformatowanie wyświetlanej wartości `UnitsInStock` w czerwonej czcionce, jeśli wartość wynosi 10 lub mniej. Aby to osiągnąć, musimy programowo uzyskać dostęp do kontrolki `UnitsInStockLabel` w `ItemTemplate` i ustawić jej właściwości stylu tak, aby tekst był wyświetlany na czerwono. Aby uzyskać dostęp do formantu sieci Web w szablonie, użyj metody `FindControl("controlID")` w następujący sposób:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

Naszym Przykładem chcemy uzyskać dostęp do kontrolki etykiety, której wartość `ID` jest `UnitsInStockLabel`, dlatego będziemy używać następujących metod:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Gdy firma Microsoft udostępnia programowe odwołanie do formantu sieci Web, w razie potrzeby można zmodyfikować jego właściwości powiązane z stylem. Tak jak w przypadku wcześniejszego przykładu została utworzona Klasa CSS w `Styles.css` o nazwie `LowUnitsInStockEmphasis`. Aby zastosować ten styl do kontrolki sieci Web etykieta, Ustaw odpowiednio jej Właściwość `CssClass`.

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> Składnia służąca do formatowania szablonu programowo uzyskującego dostęp do formantu sieci Web przy użyciu `FindControl("controlID")` a następnie ustawiając jego właściwości powiązane z stylem, można również użyć [Używanie TemplateField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) w kontrolkach DetailsView lub GridView. Będziemy analizować używanie TemplateField w naszym następnym samouczku.

Rysunki 7 przedstawiają FormView podczas wyświetlania produktu, którego `UnitsInStock` wartość jest większa niż 10, natomiast produkt na rysunku 8 ma swoją wartość mniejszą niż 10.

[![produktów z dostatecznie dużymi jednostkami w magazynie, nie jest stosowane formatowanie niestandardowe](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Rysunek 7**. w przypadku produktów z dostatecznie dużymi jednostkami w magazynie, nie jest stosowane formatowanie niestandardowe ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image17.png))

[![jednostki w numerze zapasów są wyświetlane na czerwono dla tych produktów o wartościach 10 lub mniej](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Rysunek 8**: jednostki w numerze zapasów są pokazane na czerwono dla tych produktów o wartościach 10 lub mniej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image20.png))

## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formatowanie za pomocą zdarzenia`RowDataBound`GridView

Wcześniej sprawdzono sekwencję kroków wykonywanych przez formant DetailsView i FormView. Przyjrzyjmy się tym krokom ponownie w celu odświeżenia.

1. Zdarzenie `DataBinding` formantu sieci Web danych.
2. Dane są powiązane z formantem sieci Web danych.
3. Zdarzenie `DataBound` formantu sieci Web danych.

Te trzy proste kroki są wystarczające dla widoku DetailsView i FormView, ponieważ wyświetlają tylko jeden rekord. Dla widoku GridView, w którym są wyświetlane *wszystkie* powiązane z nim rekordy (nie tylko pierwszy), krok 2 to nieco więcej.

W kroku 2 widok GridView wylicza źródło danych i dla każdego rekordu tworzy wystąpienie `GridViewRow` i wiąże bieżący rekord z nim. Dla każdej `GridViewRow` dodanej do widoku GridView są zgłaszane dwa zdarzenia:

- **`RowCreated`** uruchamiany po utworzeniu `GridViewRow`
- **`RowDataBound`** uruchamiany po powiązaniu bieżącego rekordu z `GridViewRow`.

W przypadku widoku GridView szczegółowe dane są bardziej szczegółowo opisane w następującej kolejności kroków:

1. Zdarzenie `DataBinding` GridView zostanie wyzwolone.
2. Dane są powiązane z elementem GridView.   
  
   Dla każdego rekordu w źródle danych 

    1. Tworzenie obiektu `GridViewRow`
    2. Wyzwalanie zdarzenia `RowCreated`
    3. Powiąż rekord z `GridViewRow`
    4. Wyzwalanie zdarzenia `RowDataBound`
    5. Dodawanie `GridViewRow` do kolekcji `Rows`
3. Zdarzenie `DataBound` GridView zostanie wyzwolone.

Aby dostosować format poszczególnych rekordów GridView, należy utworzyć procedurę obsługi zdarzeń dla zdarzenia `RowDataBound`. Aby to zilustrować, dodajmy element GridView do strony `CustomColors.aspx`, która zawiera nazwę, kategorię i cenę każdego produktu, a następnie wyróżnij te produkty, których cena jest mniejsza niż $10,00 i żółtym kolorem tła.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Krok 7. Wyświetlanie informacji o produkcie w widoku GridView

Dodaj widok GridView poniżej FormView z poprzedniego przykładu i ustaw jego właściwość `ID` na `HighlightCheapProducts`. Ponieważ mamy już element ObjectDataSource, który zwraca wszystkie produkty na stronie, powiąż widok GridView z tym. Na koniec Edytuj BoundFields GridView, aby uwzględnić tylko nazwy produktów, kategorie i ceny. Po edycji znacznik GridView powinien wyglądać następująco:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Rysunek 9 przedstawia postęp w tym punkcie, gdy jest wyświetlany za pomocą przeglądarki.

[![GridView wyświetla nazwę, kategorię i cenę każdego produktu](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Ilustracja 9**. w widoku GridView wyświetlana jest nazwa, Kategoria i cena każdego produktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image23.png))

## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Krok 8. programistyczne Określanie wartości danych w programie obsługi zdarzeń RowDataBound

Gdy `ProductsDataTable` jest powiązany z elementem GridView, jego wystąpienia `ProductsRow` są wyliczane i dla każdego `ProductsRow` zostanie utworzony `GridViewRow`. Właściwość `DataItem` `GridViewRow`jest przypisywana do określonego `ProductRow`, po której zostanie zgłoszony program obsługi zdarzeń `RowDataBound` elementu GridView. Aby określić `UnitPrice` wartość dla każdego produktu powiązanego z elementem GridView, należy utworzyć procedurę obsługi zdarzeń dla zdarzenia `RowDataBound` GridView. W tym obsłudze zdarzeń możemy sprawdzić `UnitPrice` wartość dla bieżącego `GridViewRow` i wprowadzić decyzję o formatowaniu dla tego wiersza.

Ten program obsługi zdarzeń można utworzyć, korzystając z tej samej serii kroków jak w przypadku widoku FormView i DetailsView.

![Utwórz procedurę obsługi zdarzeń dla zdarzenia RowDataBound GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Ilustracja 10**. Tworzenie obsługi zdarzeń dla zdarzenia `RowDataBound` GridView

Utworzenie programu obsługi zdarzeń w ten sposób spowoduje automatyczne dodanie następującego kodu do fragmentu kodu strony ASP.NET:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Gdy zdarzenie `RowDataBound` zostanie wyzwolone, program obsługi zdarzeń jest przenoszona jako drugi parametr obiektu typu `GridViewRowEventArgs`, który ma właściwość o nazwie `Row`. Ta właściwość zwraca odwołanie do `GridViewRow`, która była tylko powiązana z danymi. Aby uzyskać dostęp do wystąpienia `ProductsRow` powiązanego z `GridViewRow` używamy właściwości `DataItem`, takiej jak:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Podczas pracy z programem obsługi zdarzeń `RowDataBound` należy pamiętać, że widok GridView składa się z różnych typów wierszy i że to zdarzenie jest wywoływane dla *wszystkich* typów wierszy. Typ `GridViewRow`może być określony przez jego właściwość `RowType` i może mieć jedną z możliwych wartości:

- `DataRow` wiersz, który jest powiązany z rekordem z `DataSource` GridView
- `EmptyDataRow` wyświetlany wiersz, gdy `DataSource` GridView jest pusty
- `Footer` wiersz stopki; wyświetlany, jeśli właściwość `ShowFooter` GridView jest ustawiona na `True`
- `Header` wiersza nagłówka; wyświetlany, jeśli właściwość ShowHeader widoku GridView ma wartość `True` (wartość domyślna)
- `Pager` dla elementu GridView implementującego stronicowanie, wiersz, w którym jest wyświetlany interfejs stronicowania
- `Separator` nie jest używany dla widoku GridView, ale jest używany przez właściwości `RowType` dla kontrolek DataList i wzmacniak, dwie kontrolki sieci Web danych, które będziemy omówić w przyszłych samouczkach

Ponieważ `EmptyDataRow`, `Header`, `Footer`i `Pager` wierszy nie są skojarzone z rekordem `DataSource`, zawsze będą mieć wartość `Nothing` dla właściwości `DataItem`. Z tego powodu przed podjęciem próby pracy z bieżącą właściwością `DataItem` `GridViewRow`należy najpierw upewnić się, że prowadzimy Cię przez nas `DataRow`. Można to osiągnąć, sprawdzając Właściwość `RowType` `GridViewRow`, tak jak:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Krok 9: wyróżnianie wiersza żółty, gdy wartość CenaJednostkowa jest mniejsza niż $10,00

Ostatnim krokiem jest programowe wyróżnianie całego `GridViewRow`, jeśli wartość `UnitPrice` dla tego wiersza jest mniejsza niż $10,00. Składnia służąca do uzyskiwania dostępu do wierszy lub komórek GridView jest taka sama jak w przypadku `GridViewID.Rows(index)` DetailsView, aby uzyskać dostęp do całego wiersza, `GridViewID.Rows(index).Cells(index)`, aby uzyskać dostęp do określonej komórki. Jednak gdy program obsługi zdarzeń `RowDataBound` wyzwala, `GridViewRow` powiązana z danymi nie została jeszcze dodana do kolekcji `Rows` GridView. W związku z tym nie można uzyskać dostępu do bieżącego wystąpienia `GridViewRow` z programu obsługi zdarzeń `RowDataBound` przy użyciu kolekcji wierszy.

Zamiast `GridViewID.Rows(index)`można odwoływać się do bieżącego wystąpienia `GridViewRow` w programie obsługi zdarzeń `RowDataBound` przy użyciu `e.Row`. Oznacza to, że w celu wyróżnienia bieżącego wystąpienia `GridViewRow` z programu obsługi zdarzeń `RowDataBound`, z którego korzystamy:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Zamiast ustawiać właściwość `BackColor` `GridViewRow`bezpośrednio, Zacznijmy od używania klas CSS. Utworzyliśmy klasę CSS o nazwie `AffordablePriceEmphasis`, która ustawia kolor tła na żółty. Zakończono procedurę obsługi zdarzeń `RowDataBound`:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]

[![najbardziej atrakcyjne produkty są wyróżnione jako żółte](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Ilustracja 11**. najbardziej atrakcyjne produkty są wyróżnione jako żółte ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image27.png))

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób formatowania widoku GridView, DetailsView i FormView na podstawie danych powiązanych z kontrolką. W tym celu została utworzona procedura obsługi zdarzeń dla zdarzeń `DataBound` lub `RowDataBound`, w przypadku których dane podstawowe zostały zbadane wraz ze zmianą formatowania, w razie potrzeby. Aby uzyskać dostęp do danych powiązanych z formantem DetailsView lub FormView, użyjemy właściwości `DataItem` w programie obsługi zdarzeń `DataBound`; dla widoku GridView Każda właściwość `DataItem` wystąpienia `GridViewRow` zawiera dane powiązane z tym wierszem, który jest dostępny w programie obsługi zdarzeń `RowDataBound`.

Składnia służąca do programistycznego dostosowywania formatowania kontrolki sieci Web danych zależy od kontrolki sieci Web i sposobu wyświetlania danych. W przypadku kontrolek DetailsView i GridView można uzyskać dostęp do wierszy i komórek za pomocą indeksu porządkowego. Dla widoku FormView, który używa szablonów, Metoda `FindControl("controlID")` jest często używana do lokalizowania formantu sieci Web z poziomu szablonu.

W następnym samouczku dowiesz się, jak używać szablonów z widokami GridView i DetailsView. Dodatkowo zobaczymy kolejną technikę dostosowywania formatowania w oparciu o dane bazowe.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka zostali E.R. Gilmore, Dennis Patterson i Dan Jagers. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [dalej](using-templatefields-in-the-gridview-control-vb.md)
