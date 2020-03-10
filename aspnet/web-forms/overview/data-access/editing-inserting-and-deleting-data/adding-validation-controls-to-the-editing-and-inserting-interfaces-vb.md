---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Dodawanie kontrolek weryfikacji do interfejsów edycji i wstawiania (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak łatwo można dodać kontrolki weryfikacji do EditItemTemplate i InsertItemTemplate kontrolki sieci Web danych, aby uzyskać więcej...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c5ad110ee0836f0a464b02a2b29254e2e06381e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78592828"
---
# <a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Dodawanie kontrolek walidacji do interfejsów edycji i wstawiania (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) lub [Pobierz plik PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> W tym samouczku dowiesz się, jak łatwo można dodać kontrolki weryfikacji do EditItemTemplate i InsertItemTemplate kontrolki sieci Web danych, aby zapewnić bardziej foolproof interfejs użytkownika.

## <a name="introduction"></a>Wprowadzenie

Kontrolki GridView i DetailsView w przykładach w ostatnich trzech samouczkach zostały złożone z BoundFields i CheckBoxFields (typy pól automatycznie dodawane przez program Visual Studio podczas wiązania widoku GridView lub DetailsView ze źródłem danych). kontrolowanie za pomocą tagu inteligentnego). Podczas edytowania wiersza w widoku GridView lub DetailsView te BoundFields, które nie są tylko do odczytu, są konwertowane na pola tekstowe, z których użytkownik końcowy może modyfikować istniejące dane. Podobnie podczas wstawiania nowego rekordu do kontrolki DetailsView, te BoundFields, których właściwość `InsertVisible` jest ustawiona na `True` (wartość domyślna) są renderowane jako puste pola tekstowe, w których użytkownik może podać wartości pól nowego rekordu. Podobnie CheckBoxFields, które są wyłączone w standardowym interfejsie tylko do odczytu, są konwertowane na pola wyboru włączone w interfejsie edytowanie i wstawianie.

Podczas gdy domyślne edytowanie i wstawianie interfejsów dla BoundField i CheckBoxField mogą być przydatne, interfejs nie ma żadnego sortowania walidacji. Jeśli użytkownik dokonuje błędu wpisu danych, na przykład pomijania pola `ProductName` lub wprowadzenia nieprawidłowej wartości dla `UnitsInStock` (na przykład-50), wyjątek zostanie zgłoszony z poziomu głębokości architektury aplikacji. Chociaż ten wyjątek można bezpiecznie obsłużyć, jak pokazano w [poprzednim samouczku](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), w idealnym przypadku edytowanie lub wstawianie interfejsu użytkownika obejmuje kontrolki sprawdzania poprawności, które uniemożliwiają użytkownikowi wprowadzanie takich nieprawidłowych danych w pierwszym miejscu.

Aby zapewnić dostosowaną edycję lub wstawianie interfejsu, należy zamienić BoundField lub CheckBoxField na TemplateField. Używanie TemplateField, który był tematem dyskusji w używanie TemplateField, [w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) i [przy użyciu używanie TemplateField w samouczkach kontrolek DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) , może składać się z wielu szablonów definiujących oddzielne interfejsy dla różnych stanów wiersza. `ItemTemplate` TemplateField jest używany do renderowania pól tylko do odczytu lub wierszy w kontrolkach DetailsView lub GridView, podczas gdy `EditItemTemplate` i `InsertItemTemplate` wskazują interfejsy do użycia w trybach edycji i wstawiania.

W tym samouczku dowiesz się, jak łatwo można dodać kontrolki weryfikacji do `EditItemTemplate` i `InsertItemTemplate`, aby zapewnić bardziej foolproof interfejs użytkownika. W tym samouczku przedstawiono przykład utworzony w ramach [badania zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) samouczka oraz rozszerza edytowanie i wstawianie interfejsów w celu uwzględnienia odpowiedniej weryfikacji.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deleting"></a>Krok 1. replikowanie przykładu z[badania zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

W oknie [badanie zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) samouczka została utworzona strona z nazwami i cenami produktów w edytowalnym widoku GridView. Ponadto strona zawiera element DetailsView, którego właściwość `DefaultMode` została ustawiona na `Insert`, a tym samym zawsze Renderuj w trybie wstawiania. Z tego widoku DetailsView użytkownik może wprowadzić nazwę i cenę nowego produktu, kliknąć pozycję Wstaw, a następnie dodać do systemu (patrz rysunek 1).

[![w poprzednim przykładzie użytkownicy mogą dodawać nowe produkty i edytować istniejące.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Rysunek 1**. w poprzednim przykładzie użytkownicy mogą dodawać nowe produkty i edytować istniejące. ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))

Naszym celem tego samouczka jest rozszerzenie widoku DetailsView i GridView w celu zapewnienia kontroli poprawności. W szczególności nasza logika walidacji będzie:

- Wymagaj, aby nazwa była podana podczas wstawiania lub edytowania produktu
- Wymagaj, aby cena była podawana podczas wstawiania rekordu; Podczas edytowania rekordu nadal będzie wymagana Cena, ale będzie ona używać logiki programowej w programie obsługi zdarzeń `RowUpdating` GridView, który jest już obecny w poprzednim samouczku
- Upewnij się, że wartość wprowadzona dla ceny jest prawidłowym formatem waluty

Zanim będziemy mogli zapoznać się z poprzednim przykładem w celu uwzględnienia walidacji, najpierw należy przeprowadzić replikację przykładu ze strony `DataModificationEvents.aspx` na stronie tego samouczka `UIValidation.aspx`. Aby to osiągnąć, należy skopiować zarówno znaczniki deklaratywne `DataModificationEvents.aspx` strony, jak i kod źródłowy. Najpierw należy skopiować znaczniki deklaratywne, wykonując następujące czynności:

1. Otwórz stronę `DataModificationEvents.aspx` w programie Visual Studio
2. Przejdź do znacznika deklaracyjnego strony (kliknij przycisk Źródło w dolnej części strony).
3. Skopiuj tekst w `<asp:Content>` i `</asp:Content>` tagów (wiersze od 3 do 44), jak pokazano na rysunku 2.

[![skopiować tekst w &lt;ASP: zawartość kontrolki&gt;](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Rysunek 2**. Skopiuj tekst w kontrolce `<asp:Content>` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))

1. Otwórz stronę `UIValidation.aspx`
2. Przejdź do znacznika deklaracyjnego strony
3. Wklej tekst w kontrolce `<asp:Content>`.

Aby skopiować kod źródłowy, Otwórz stronę `DataModificationEvents.aspx.vb` i skopiuj tylko tekst *w* klasie `EditInsertDelete_DataModificationEvents`. Skopiuj trzy procedury obsługi zdarzeń (`Page_Load`, `GridView1_RowUpdating`i `ObjectDataSource1_Inserting`), ale **nie** Kopiuj deklaracji klasy. Wklej *skopiowany tekst w* klasie `EditInsertDelete_UIValidation` w `UIValidation.aspx.vb`.

Po przeniesieniu zawartości i kodu z `DataModificationEvents.aspx` do `UIValidation.aspx`Poświęć chwilę na przetestowanie postępu w przeglądarce. Należy zobaczyć te same dane wyjściowe i korzystać z tych samych funkcji na każdej z tych dwóch stron (zapoznaj się z powrotem do rysunku 1, aby zrzut ekranu `DataModificationEvents.aspx` w działaniu).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Krok 2: konwertowanie BoundFields na używanie TemplateField

Aby dodać kontrolki weryfikacji do interfejsów edycji i wstawiania, BoundFields używany przez kontrolki DetailsView i GridView muszą zostać przekonwertowane na używanie TemplateField. Aby to osiągnąć, kliknij odpowiednio linki Edytuj kolumny i edytuj pola w tagach inteligentnych GridView i DetailsView. Zaznacz wszystkie BoundFields i kliknij link "Konwertuj to pole na TemplateField".

[![przekonwertować wszystkie BoundFieldsy widoku DetailsView i GridView na używanie TemplateField](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Rysunek 3**. Przekonwertuj wszystkie BoundFieldsy widoku DetailsView i GridView na używanie TemplateField ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))

Konwersja BoundField na TemplateField za pomocą okna dialogowego pola generuje TemplateField, który wykazuje te same tylko do odczytu, edycji i wstawiania interfejsów jako samego BoundField. Poniższe znaczniki przedstawiają składnię deklaratywną dla pola `ProductName` w widoku DetailsView po przekształceniu jej w TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Należy zauważyć, że te TemplateField miały trzy szablony, które są automatycznie tworzone `ItemTemplate`, `EditItemTemplate`i `InsertItemTemplate`. `ItemTemplate` Wyświetla pojedynczą wartość pola danych (`ProductName`) przy użyciu kontrolki sieci Web etykieta, podczas gdy `EditItemTemplate` i `InsertItemTemplate` przedstawiają wartość pola danych w formancie sieci Web TextBox, który kojarzy pole danych z właściwością `Text`ą pola tekstowego przy użyciu wielokierunkowego wiązania z danymi. Ponieważ korzystamy tylko z widoku DetailsView na tej stronie do wstawiania, możesz usunąć `ItemTemplate` i `EditItemTemplate` z dwóch używanie TemplateField, chociaż nie ma żadnych szkód w opuszczaniu tych danych.

Ponieważ GridView nie obsługuje wbudowanej funkcji wstawiania w widoku DetailsView, przekonwertowanie pola `ProductName` GridView na TemplateField powoduje tylko `ItemTemplate` i `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Klikając "Konwertuj to pole na TemplateField", program Visual Studio utworzył TemplateField, którego szablony naśladuje interfejs użytkownika przekonwertowanego BoundField. Możesz to sprawdzić, odwiedzając tę stronę za pomocą przeglądarki. Wygląd i zachowanie używanie TemplateField są takie same, jak w przypadku użycia BoundFields.

> [!NOTE]
> W razie potrzeby możesz dostosować interfejsy edycji w szablonach. Na przykład możemy chcieć, aby pole tekstowe w `UnitPrice` używanie TemplateField renderowane jako mniejsze pole tekstowe niż `ProductName` pole tekstowe. Aby to osiągnąć, można ustawić właściwość `Columns` pola tekstowego na odpowiednią wartość lub podać absolutną szerokość za pomocą właściwości `Width`. W następnym samouczku zobaczymy, jak całkowicie dostosować interfejs edycji, zastępując pole tekstowe alternatywnym formantem sieci Web wprowadzania danych.

## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Krok 3. Dodawanie kontrolek weryfikacji do`EditItemTemplate` GridView

Podczas konstruowania formularzy wprowadzania danych ważne jest, aby użytkownicy wprowadzali wszystkie wymagane pola, a wszystkie podane dane wejściowe są prawidłowymi, właściwie sformatowanymi wartościami. Aby zapewnić, że dane wejściowe użytkownika są prawidłowe, ASP.NET zapewnia pięć wbudowanych kontrolek walidacji, które są przeznaczone do użycia w celu sprawdzenia wartości pojedynczej kontrolki wejściowej:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) gwarantuje, że została podana wartość
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) sprawdza wartość pod kątem innej wartości kontrolki sieci Web lub wartości stałej lub gwarantuje, że format wartości jest dozwolony dla określonego typu danych
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) gwarantuje, że wartość znajduje się w zakresie wartości
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) sprawdza poprawność wartości względem [wyrażenia regularnego](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) sprawdza poprawność wartości względem niestandardowej, zdefiniowanej przez użytkownika metody

Aby uzyskać więcej informacji na temat tych pięciu kontrolek, zapoznaj się z [sekcją kontrolki walidacji](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) [samouczków szybkiego startu ASP.NET](https://asp.net/QuickStart/aspnet/).

W naszym samouczku będziemy musieli użyć elementu RequiredFieldValidator zarówno w widoku `ProductName` DetailsView, jak i GridView, a RequiredFieldValidator w TemplateField `UnitPrice` DetailsView. Ponadto należy dodać CompareValidator do obu formantów "`UnitPrice` używanie TemplateField, które zapewniają, że wprowadzona cena ma wartość większą lub równą 0 i jest prezentowana w prawidłowym formacie waluty.

> [!NOTE]
> Chociaż ASP.NET 1. x miała te same pięć kontrolek weryfikacyjnych, ASP.NET 2,0 dodaliśmy kilka ulepszeń, główne dwa z nich obsługują skrypty po stronie klienta dla przeglądarek innych niż Internet Explorer i możliwość partycjonowania formantów walidacji na stronie grupy walidacji. Aby uzyskać więcej informacji na temat nowych funkcji kontroli walidacji w 2,0, zapoznaj się z artykułem dotyczącym wypełniania [kontrolek weryfikacji w ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Zacznijmy od dodania wymaganych kontrolek weryfikacji do `EditItemTemplate` s w używanie TemplateField GridView. Aby to osiągnąć, kliknij link Edytuj szablony w tagu inteligentnym GridView, aby wyświetlić interfejs edycji szablonu. W tym miejscu możesz wybrać szablon do edycji z listy rozwijanej. Ponieważ chcemy rozszerzyć interfejs edycji, musimy dodać kontrolki weryfikacji do `ProductName` i `UnitPrice``EditItemTemplate` s.

[![musimy zwiększyć EditItemTemplates ProductName i CenaJednostkowa](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Rysunek 4**. musimy zwiększyć `ProductName` i `UnitPrice``EditItemTemplate` s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))

W `EditItemTemplate``ProductName` Dodaj RequiredFieldValidator, przeciągając go z przybornika do interfejsu edycji szablonu, umieszczając po elemencie TextBox.

[![dodać RequiredFieldValidator do EditItemTemplate ProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Rysunek 5**. Dodawanie RequiredFieldValidator do `EditItemTemplate` `ProductName` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))

Wszystkie kontrolki walidacji działają, sprawdzając poprawność danych wejściowych pojedynczej kontrolki sieci Web ASP.NET. W związku z tym musimy wskazać, że RequiredFieldValidator dodaliśmy powinien sprawdzić poprawność względem pola tekstowego w `EditItemTemplate`; jest to realizowane przez ustawienie [Właściwości ControlToValidate obiektu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) kontrolki walidacji na `ID` odpowiedniej kontrolki sieci Web. Pole tekstowe ma obecnie nondescript `ID` `TextBox1`, ale zmienimy je na bardziej odpowiednie. Kliknij pole tekstowe w szablonie, a następnie w okno Właściwości zmień `ID` `TextBox1` na `EditProductName`.

[![zmienić identyfikatora pola tekstowego na EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Ilustracja 6**. zmiana `ID` pola tekstowego na `EditProductName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))

Następnie ustaw właściwość `ControlToValidate` RequiredFieldValidator na `EditProductName`. Na koniec Ustaw [Właściwość ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) na "należy podać nazwę produktu" i [właściwość Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) na "\*". Wartość właściwości `Text`, jeśli podana, to tekst wyświetlany przez kontrolkę walidacji w przypadku niepowodzenia walidacji. Wartość właściwości `ErrorMessage`, która jest wymagana, jest używana przez formant podsumowania walidacji; Jeśli `Text` wartość właściwości zostanie pominięta, wartość właściwości `ErrorMessage` jest również tekstem wyświetlanym przez kontrolkę walidacji dla nieprawidłowych danych wejściowych.

Po ustawieniu tych trzech właściwości RequiredFieldValidator ekran powinien wyglądać podobnie do rysunku 7.

[![ustawić właściwości ControlToValidate obiektu, ErrorMessage i Text elementu RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Rysunek 7**. ustawianie właściwości `ControlToValidate`, `ErrorMessage`i `Text` dla RequiredFieldValidator (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))

Po dodaniu RequiredFieldValidator do `EditItemTemplate``ProductName` wszystko to, co ma na celu dodanie niezbędnej walidacji `UnitPrice` `EditItemTemplate`. Ze względu na to, że na tej stronie `UnitPrice` jest opcjonalne podczas edytowania rekordu, nie musimy dodawać RequiredFieldValidator. Należy jednak dodać CompareValidator, aby upewnić się, że `UnitPrice`, jeśli jest podany, jest poprawnie sformatowana jako waluta i jest większa lub równa 0.

Przed dodaniem CompareValidator do `EditItemTemplate``UnitPrice` należy najpierw zmienić identyfikator kontrolki sieci Web TextBox z `TextBox2` na `EditUnitPrice`. Po wprowadzeniu tej zmiany Dodaj CompareValidator, ustawiając jej Właściwość `ControlToValidate` na `EditUnitPrice`, jej Właściwość `ErrorMessage` na wartość "Cena musi być większa niż lub równa zero i nie może zawierać symbolu waluty" i jego właściwości `Text` na "\*".

Aby wskazać, że wartość `UnitPrice` musi być większa lub równa 0, ustaw [Właściwość operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) CompareValidator na `GreaterThanEqual`, jej [Właściwość ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) na wartość "0", a jej [Właściwość Type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) na `Currency`. W następującej składni deklaracyjnej przedstawiono `EditItemTemplate` `UnitPrice` TemplateField po wprowadzeniu tych zmian:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Po wprowadzeniu tych zmian Otwórz stronę w przeglądarce. Jeśli spróbujesz pominąć nazwę lub wprowadzić nieprawidłową wartość ceny podczas edytowania produktu, obok pola tekstowego pojawia się gwiazdka. Jak pokazano na rysunku 8, wartość ceny, która zawiera symbol waluty, na przykład $19,95, jest uznawana za nieprawidłową. CompareValidator `Currency` `Type` umożliwia używanie separatorów cyfr (takich jak przecinki lub kropki, w zależności od ustawień kultury) i wiodącego znaku plus lub minus, ale *nie zezwala na* symbol waluty. Takie zachowanie może Perplex użytkowników jako interfejs edycji obecnie renderuje `UnitPrice` przy użyciu formatu waluty.

> [!NOTE]
> Odwołaj się to w przypadku *zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem* samouczka ustawimy Właściwość `DataFormatString` BoundField na `{0:c}`, aby sformatować ją jako walutę. Ponadto ustawimy Właściwość `ApplyFormatInEditMode` na wartość true, co spowoduje, że interfejs edycyjny w programie GridView sformatuje `UnitPrice` jako walutę. Podczas konwertowania BoundField na TemplateField, program Visual Studio zauważał te ustawienia i sformatował Właściwość `Text` TextBox jako walutę przy użyciu składni DataBinding `<%# Bind("UnitPrice", "{0:c}") %>`.

[![gwiazdki pojawia się obok pól tekstowych z nieprawidłowymi danymi wejściowymi](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Ilustracja 8**. obok pól tekstowych pojawia się gwiazdka z nieprawidłowymi danymi wejściowymi ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))

Gdy walidacja działa tak, jak to jest, użytkownik musi ręcznie usunąć symbol waluty podczas edytowania rekordu, co nie jest akceptowalne. W celu rozwiązania tego problemu dostępne są trzy opcje:

1. Skonfiguruj `EditItemTemplate` tak, aby wartość `UnitPrice` nie była sformatowana jako waluta.
2. Zezwalaj użytkownikowi na wprowadzanie symbolu waluty przez usunięcie CompareValidator i zastąpienie go RegularExpressionValidator, który prawidłowo sprawdza prawidłowo sformatowaną wartość waluty. Problem polega na tym, że wyrażenie regularne służące do sprawdzania poprawności wartości walutowej nie jest dość i wymagałoby pisania kodu, jeśli chcemy uwzględnić ustawienia kultury.
3. Całkowicie Usuń kontrolę weryfikacji i Polegaj na logice walidacji po stronie serwera w programie obsługi zdarzeń `RowUpdating` GridView.

Przejdźmy do opcji #1 dla tego ćwiczenia. Obecnie `UnitPrice` jest sformatowana jako waluta ze względu na wyrażenie wiązania danych dla pola tekstowego w `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Zmień instrukcję bind na `Bind("UnitPrice", "{0:n2}")`, która formatuje wynik jako liczbę z dwiema cyframi precyzji. Można to zrobić bezpośrednio poprzez składnię deklaratywną lub klikając łącze Edytuj powiązania danych z `EditUnitPrice` TextBox w `EditItemTemplate` `UnitPrice` TemplateField (zobacz ilustracje 9 i 10).

[![kliknij link Edytuj powiązania danych w polu tekstowym](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Ilustracja 9**. Kliknij link Edytuj powiązania danych w polu tekstowym (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))

[![określić specyfikator formatu w instrukcji bind](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Ilustracja 10**. Określanie specyfikatora formatu w instrukcji `Bind` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))

W przypadku tej zmiany sformatowana cena w interfejsie edycji obejmuje przecinki jako separator grupy i kropkę jako separator dziesiętny, ale pozostawia symbol waluty.

> [!NOTE]
> `EditItemTemplate` `UnitPrice` nie zawiera elementu RequiredFieldValidator, co pozwala na rozpoczęcie procesu ogłaszania zwrotnego i logiki aktualizowania. Niemniej jednak program obsługi zdarzeń `RowUpdating` skopiowany za pomocą funkcji *badania zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem* samouczka zawiera kontrolę programistyczną, która zapewnia, że `UnitPrice`. Możesz usunąć tę logikę, pozostawić ją w niezmienionej postaci lub dodać RequiredFieldValidator do `EditItemTemplate``UnitPrice`.

## <a name="step-4-summarizing-data-entry-problems"></a>Krok 4. podsumowanie problemów z wprowadzaniem danych

Oprócz pięciu kontrolek weryfikacyjnych ASP.NET zawiera [kontrolkę podsumowania walidacji](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), która wyświetla `ErrorMessage` s tych kontrolek sprawdzania poprawności, które wykryły nieprawidłowe dane. Te dane podsumowujące mogą być wyświetlane jako tekst na stronie sieci Web lub przez modalne, po stronie klienta. Ulepszamy ten samouczek, aby dołączyć element MessageBox po stronie klienta podsumowujący wszelkie problemy z walidacją.

Aby to osiągnąć, przeciągnij formant podsumowania walidacji z przybornika do projektanta. Lokalizacja kontroli poprawności nie jest w rzeczywistości kwestią, ponieważ skonfigurujemy ją tak, aby wyświetlała tylko podsumowanie jako element MessageBox. Po dodaniu kontrolki ustaw jej [Właściwość ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) na wartość `False` i jej [właściwość ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) na `True`. W tym przypadku wszelkie błędy sprawdzania poprawności są sumowane w elemencie MessageBox po stronie klienta.

[![błędy walidacji są sumowane w elemencie MessageBox po stronie klienta](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Ilustracja 11**. błędy walidacji są sumowane w elemencie MessageBox po stronie klienta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))

## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Krok 5. Dodawanie kontrolek weryfikacji do`InsertItemTemplate` DetailsView

Wszystkie te, które pozostają na potrzeby tego samouczka, to dodawanie kontrolek weryfikacji do interfejsu wstawiania w widoku DetailsView. Proces dodawania kontrolek weryfikacji do szablonów DetailsView jest identyczny z tym, co zostało zbadane w kroku 3. w związku z tym będziemy Breeze przez zadanie w tym kroku. Tak jak w przypadku `EditItemTemplate`u GridView, zachęcamy do zmiany nazwy `ID` s pól textnondescript `TextBox1` i `TextBox2` `InsertProductName` i `InsertUnitPrice`.

Dodaj RequiredFieldValidator do `InsertItemTemplate``ProductName`. Ustaw `ControlToValidate` na `ID` pola tekstowego w szablonie, jego właściwość `Text` na "\*" i jego właściwość `ErrorMessage` na "musisz podać nazwę produktu".

Ponieważ `UnitPrice` jest wymagana dla tej strony podczas dodawania nowego rekordu, Dodaj element RequiredFieldValidator do `InsertItemTemplate``UnitPrice`, odpowiednio ustawiając jego właściwości `ControlToValidate`, `Text`i `ErrorMessage`. Na koniec Dodaj również CompareValidator do `InsertItemTemplate` `UnitPrice`, konfigurując `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`i `ValueToCompare` właściwości, podobnie jak w przypadku `UnitPrice`CompareValidator w `EditItemTemplate`GridView.

Po dodaniu tych kontrolek weryfikacyjnych nowy produkt nie może zostać dodany do systemu, jeśli jego nazwa nie została podana lub jego cena jest liczbą ujemną lub niesformatowaną.

[![logiki walidacji została dodana do interfejsu wstawiania w widoku DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Ilustracja 12**. Logika walidacji została dodana do interfejsu wstawiania w widoku DetailsView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))

## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Krok 6. partycjonowanie kontrolek weryfikacji do grup walidacji

Nasza strona zawiera dwa logicznie różne zestawy kontrolek weryfikacji: te, które są zgodne z interfejsem edycyjnym GridView oraz te, które odpowiadają wstawianiu interfejsu w widoku DetailsView. Domyślnie, gdy następuje ogłaszanie zwrotne *wszystkie* kontrolki walidacji na stronie są sprawdzane. Jednak podczas edytowania rekordu nie chcemy, aby kontrolki sprawdzania poprawności interfejsu w widoku DetailsView były weryfikowane. Na rysunku 13 przedstawiono bieżące dylematem, gdy użytkownik edytuje produkt przy użyciu idealnie dozwolonych wartości, a kliknięcie przycisku Aktualizuj powoduje błąd walidacji, ponieważ nazwy i wartości cen w interfejsie wstawiania są puste.

[![zaktualizowanie produktu powoduje, że kontrolki walidacji interfejsu wstawiania są uruchamiane](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Ilustracja 13**. Aktualizacja produktu powoduje, że kontrolki sprawdzania poprawności interfejsu wstawiania są uruchamiane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))

Kontrolki walidacji w ASP.NET 2,0 mogą być partycjonowane w grupach walidacji za pomocą ich właściwości `ValidationGroup`. Aby skojarzyć zestaw kontrolek weryfikacji w grupie, po prostu ustaw ich Właściwość `ValidationGroup` na taką samą wartość. W naszym samouczku Ustaw `ValidationGroup` właściwości kontrolek walidacji w używanie TemplateField GridView na `EditValidationControls` i `ValidationGroup` właściwości używanie TemplateField widoku DetailsView do `InsertValidationControls`. Te zmiany można wykonać bezpośrednio w znacznikach deklaratywnych lub za pośrednictwem okno Właściwości w przypadku korzystania z interfejsu szablonu edycji w projektancie.

Oprócz kontrolek walidacji kontrolki powiązane z przyciskiem i przyciskiem w ASP.NET 2,0 zawierają również właściwość `ValidationGroup`. Walidacje grupy sprawdzania poprawności są sprawdzane pod kątem poprawności tylko wtedy, gdy proces ogłaszania zwrotnego jest wywoływany przez przycisk, który ma takie samo `ValidationGroup` ustawienie właściwości. Na przykład aby można było wyzwolić przycisk wstawiania w widoku DetailsView `InsertValidationControls` grupę sprawdzania poprawności, należy ustawić właściwość `ValidationGroup` CommandField na `InsertValidationControls` (zobacz rysunek 14). Ponadto ustaw właściwość CommandField's `ValidationGroup` widoku GridView na `EditValidationControls`.

[![ustawić właściwość CommandField's walidacji widoku DetailsView na InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Ilustracja 14**. Ustaw właściwość CommandField's `ValidationGroup` widoku DetailsView na `InsertValidationControls` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))

Po wprowadzeniu tych zmian elementy używanie TemplateField i CommandFields widoku GridView powinny wyglądać podobnie do następujących:

Używanie TemplateField i CommandField w widoku DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField i używanie TemplateField GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

W tym momencie kontrolki walidacji specyficzne dla edycji są wyzwalane tylko wtedy, gdy kliknięto przycisk aktualizacji GridView, a kontrolki walidacji dotyczące wstawiania są wyzwalane tylko wtedy, gdy kliknięto przycisk wstawiania w widoku DetailsView, rozpuszczenie problemu wyróżnionego na rysunku 13. Jednak w przypadku zmiany podsumowania walidacji formant nie jest już wyświetlany podczas wprowadzania nieprawidłowych danych. Kontrolka podsumowania walidacji zawiera również właściwość `ValidationGroup` i zawiera tylko podsumowanie informacji o tych kontrolkach sprawdzania poprawności w swojej grupie walidacji. W związku z tym muszą mieć dwie kontrolki walidacji na tej stronie, jedną dla grupy walidacji `InsertValidationControls` i jeden dla `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Dzięki temu samouczek został ukończony!

## <a name="summary"></a>Podsumowanie

Chociaż BoundFields może zapewnić interfejs wstawiania i edytowania, interfejs nie jest dostosowywany. Często chcemy dodać kontrolki weryfikacji do interfejsu edycji i wstawiania, aby upewnić się, że użytkownik wprowadzi wymagane dane wejściowe w dozwolonym formacie. Aby to osiągnąć, należy przekonwertować BoundFields na używanie TemplateField i dodać kontrolki weryfikacji do odpowiednich szablonów. W tym samouczku przedłużono przykład z *badania zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem* samouczka, Dodawanie kontrolek weryfikacji do interfejsu wstawiania w widoku DetailsView oraz do interfejsu edycji GridView. Ponadto pokazano, jak wyświetlać podsumowanie informacji o walidacji za pomocą kontrolki podsumowania walidacji oraz jak podzielić kontrolki walidacji na stronie na różne grupy sprawdzania poprawności.

Jak zostało to opisane w tym samouczku, używanie TemplateField zezwolić na rozszerzanie interfejsów edycji i wstawiania, aby obejmowały kontrolki walidacji. Używanie TemplateField można także rozszerzyć w celu uwzględnienia dodatkowych kontrolek sieci Web, co umożliwia zamienienie pola tekstowego przez bardziej odpowiedni formant sieci Web. W naszym następnym samouczku zobaczymy, jak zastąpić formant TextBox za pomocą kontrolki DropDownList powiązanej z danymi, która jest idealnym rozwiązaniem podczas edytowania klucza obcego (takiego jak `CategoryID` lub `SupplierID` w tabeli `Products`).

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Liz Shulok i Zack Kowalski. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [dalej](customizing-the-data-modification-interface-vb.md)
