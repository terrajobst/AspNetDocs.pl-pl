---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Dodawanie kontrolek weryfikacji do interfejsu edycji elementu DataList (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak łatwo można dodać kontrolki walidacji do EditItemTemplatea elementu DataList, aby zapewnić większą foolproof edycji int użytkownika...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: f952a7bb95e956a2ad935f8bdef5c3efa7437ecb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78594522"
---
# <a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Dodawanie kontrolek walidacji do interfejsu edycji kontrolki DataList (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) lub [Pobierz plik PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> W tym samouczku dowiesz się, jak łatwo można dodać kontrolki weryfikacji do EditItemTemplatea elementu DataList, aby zapewnić bardziej foolproofy interfejs użytkownika.

## <a name="introduction"></a>Wprowadzenie

W tym przypadku w samouczkach do edycji elementu DataList te interfejsy edytowania danych nie zawierały żadnych funkcji weryfikacji aktywnego użytkownika, nawet jeśli nieprawidłowe dane wejściowe użytkownika, takie jak brakująca Nazwa produktu lub cena ujemna, powodują wyjątek. W [poprzednim samouczku](handling-bll-and-dal-level-exceptions-vb.md) sprawdziłmy, jak dodać kod obsługi wyjątków do procedury obsługi zdarzeń `UpdateCommand` DataList, aby przechwycić i bezpiecznie wyświetlić informacje o wszelkich zgłoszonych wyjątkach. W idealnym przypadku interfejs edytowania obejmuje kontrolki sprawdzania poprawności, które uniemożliwiają użytkownikowi wprowadzanie takich nieprawidłowych danych w pierwszym miejscu.

W tym samouczku dowiesz się, jak łatwo można dodać kontrolki walidacji do `EditItemTemplate` DataList, aby zapewnić bardziej foolproofą edycję interfejsu użytkownika. W tym samouczku przedstawiono przykład utworzony w poprzednim samouczku i rozszerza interfejs edycji w celu uwzględnienia odpowiedniej weryfikacji.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptions"></a>Krok 1. replikowanie przykładu z[obsługi wyjątków logiki biznesowej i dal](handling-bll-and-dal-level-exceptions-vb.md)

W samouczku [Obsługa wyjątków logiki biznesowej-i dal](handling-bll-and-dal-level-exceptions-vb.md) została utworzona strona z nazwami i cenami produktów w dwukolumnowej, edytowalnej nazwie DataList. Naszym celem tego samouczka jest rozszerzenie interfejsu edycji DataList s w celu uwzględnienia formantów walidacji. W szczególności nasza logika walidacji będzie:

- Wymagaj podania nazwy produktu
- Upewnij się, że wartość wprowadzona dla ceny jest prawidłowym formatem waluty
- Upewnij się, że wartość wprowadzona dla ceny jest większa lub równa zero, ponieważ ujemna `UnitPrice` wartość jest niedozwolona

Zanim będziemy mogli zapoznać się z poprzednim przykładem w celu uwzględnienia weryfikacji, najpierw musimy replikować przykład ze strony `ErrorHandling.aspx` w folderze `EditDeleteDataList` na stronie tego samouczka `UIValidation.aspx`. Aby to osiągnąć, należy skopiować zarówno znacznik deklaratywny `ErrorHandling.aspx` strony, jak i jego kod źródłowy. Najpierw należy skopiować znaczniki deklaratywne, wykonując następujące czynności:

1. Otwórz stronę `ErrorHandling.aspx` w programie Visual Studio
2. Przejdź do znacznika deklaracyjnego strony (kliknij przycisk Źródło w dolnej części strony).
3. Skopiuj tekst w `<asp:Content>` i `</asp:Content>` tagów (wiersze od 3 do 32), jak pokazano na rysunku 1.

[![skopiować tekst w &lt;ASP: zawartość kontrolki&gt;](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Rysunek 2**. Skopiuj tekst w kontrolce `<asp:Content>` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))

1. Otwórz stronę `UIValidation.aspx`
2. Przejdź do znacznika deklaracyjnego strony
3. Wklej tekst w kontrolce `<asp:Content>`.

Aby skopiować kod źródłowy, Otwórz stronę `ErrorHandling.aspx.vb` i skopiuj tylko tekst *w* klasie `EditDeleteDataList_ErrorHandling`. Skopiuj trzy procedury obsługi zdarzeń (`Products_EditCommand`, `Products_CancelCommand`i `Products_UpdateCommand`) wraz z metodą `DisplayExceptionDetails`, ale **nie** Kopiuj deklaracji klasy ani instrukcji `using`. Wklej *skopiowany tekst w* klasie `EditDeleteDataList_UIValidation` w `UIValidation.aspx.vb`.

Po przeniesieniu zawartości i kodu z `ErrorHandling.aspx` do `UIValidation.aspx`Poświęć chwilę na przetestowanie stron w przeglądarce. Te same dane wyjściowe powinny być widoczne dla każdej z tych dwóch stron (patrz rysunek 2).

[![Strona UIValidation. aspx naśladuje funkcjonalność w ErrorHandling. aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Rysunek 2**. Strona `UIValidation.aspx` naśladuje funkcjonalność w `ErrorHandling.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))

## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Krok 2. Dodawanie kontrolek walidacji do elementu DataList s EditItemTemplate

Podczas konstruowania formularzy wprowadzania danych ważne jest, aby użytkownicy wprowadzali wszystkie wymagane pola, a wszystkie podane dane wejściowe są prawidłowymi, właściwie sformatowanymi wartościami. Aby zapewnić, że dane wejściowe użytkownika są prawidłowe, ASP.NET zapewnia pięć wbudowanych kontrolek weryfikacji, które są przeznaczone do sprawdzania poprawności wartości pojedynczej wejściowej kontrolki sieci Web:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) gwarantuje, że została podana wartość
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) sprawdza wartość pod kątem innej wartości kontrolki sieci Web lub wartości stałej lub gwarantuje, że format wartości s jest dozwolony dla określonego typu danych
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) gwarantuje, że wartość znajduje się w zakresie wartości
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) sprawdza poprawność wartości względem [wyrażenia regularnego](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) sprawdza poprawność wartości względem niestandardowej, zdefiniowanej przez użytkownika metody

Aby uzyskać więcej informacji na temat tych pięciu kontrolek, zapoznaj się z artykułem [Dodawanie kontrolek walidacji do edycji i wstawiania interfejsów](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) lub zapoznaj się z [sekcją kontroli walidacji](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) [samouczków szybkiego startu ASP.NET](https://quickstarts.asp.net).

W naszym samouczku będziemy musieli użyć elementu RequiredFieldValidator, aby upewnić się, że podano wartość dla nazwy produktu i CompareValidator, aby zapewnić, że wprowadzona cena ma wartość większą lub równą 0 i jest prezentowana w prawidłowym formacie waluty.

> [!NOTE]
> Chociaż ASP.NET 1. x miała te same pięć kontrolek weryfikacyjnych, ASP.NET 2,0 dodaliśmy kilka ulepszeń, główne dwa z nich obsługują skrypty po stronie klienta dla przeglądarek oprócz programu Internet Explorer i możliwość partycjonowania formantów walidacji na stronie do grupy walidacji. Aby uzyskać więcej informacji na temat nowych funkcji kontroli walidacji w 2,0, zapoznaj się z artykułem dotyczącym wypełniania [kontrolek weryfikacji w ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Aby rozpocząć, Dodaj wymagane kontrolki weryfikacji do `EditItemTemplate`DataList. To zadanie można wykonać za pomocą projektanta, klikając łącze Edytuj szablony z taga inteligentnego DataList s lub stosując składnię deklaratywną. Pozwól na przechodzenie przez proces przy użyciu opcji Edytuj szablony z widok Projekt. Po wybraniu edycji `EditItemTemplate`DataList, Dodaj element RequiredFieldValidator, przeciągając go z przybornika do interfejsu edycji szablonu, umieszczając go po `ProductName` polu tekstowym.

[![dodać RequiredFieldValidator do EditItemTemplate po polu tekstowym ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Rysunek 3**. Dodawanie RequiredFieldValidator do `EditItemTemplate After` `ProductName` pole tekstowe (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))

Wszystkie kontrolki walidacji działają, sprawdzając poprawność danych wejściowych pojedynczej kontrolki sieci Web ASP.NET. W związku z tym musimy wskazać, że dodaliśmy RequiredFieldValidator powinna sprawdzić poprawność względem pola tekstowego `ProductName`; w tym celu należy ustawić [właściwość`ControlToValidate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) kontrolki walidacji na `ID` odpowiedniej kontrolce sieci Web (`ProductName`w tym wystąpieniu). Następnie ustaw [właściwość`ErrorMessage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) na wartość należy podać nazwę produktu i [Właściwość`Text`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) , aby \*. Wartość właściwości `Text`, jeśli podana, to tekst wyświetlany przez kontrolkę walidacji w przypadku niepowodzenia walidacji. Wartość właściwości `ErrorMessage`, która jest wymagana, jest używana przez formant podsumowania walidacji; w przypadku pominięcia wartości właściwości `Text` Właściwość `ErrorMessage` zostanie wyświetlona przez kontrolkę walidacji dla nieprawidłowych danych wejściowych.

Po ustawieniu tych trzech właściwości RequiredFieldValidator ekran powinien wyglądać podobnie do rysunku 4.

[![ustawić właściwości RequiredFieldValidator s ControlToValidate obiektu, ErrorMessage i text](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Rysunek 4**. Ustawianie właściwości RequiredFieldValidator `ControlToValidate`, `ErrorMessage`i `Text` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))

Po dodaniu RequiredFieldValidator do `EditItemTemplate`wszystko to, co ma na celu dodanie niezbędnej weryfikacji dla pola tekstowego cena produktu. Ponieważ `UnitPrice` jest opcjonalne podczas edytowania rekordu, nie musimy dodawać RequiredFieldValidator. Należy jednak dodać CompareValidator, aby upewnić się, że `UnitPrice`, jeśli jest podany, jest poprawnie sformatowana jako waluta i jest większa lub równa 0.

Dodaj CompareValidator do `EditItemTemplate` i ustaw jej Właściwość `ControlToValidate` na `UnitPrice`, jej Właściwość `ErrorMessage` na wartość Price musi być większa lub równa zero i nie może zawierać symbolu waluty oraz jego właściwości `Text`, aby \*. Aby wskazać, że wartość `UnitPrice` musi być większa lub równa 0, ustaw właściwość CompareValidator s [`Operator`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) na `GreaterThanEqual`, jej [Właściwość`ValueToCompare`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) na wartość 0, a jej [Właściwość`Type`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) na `Currency`.

Po dodaniu tych dwóch kontrolek walidacji, składnia deklaracyjne s `EditItemTemplate` s powinna wyglądać podobnie do poniższego:

[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Po wprowadzeniu tych zmian Otwórz stronę w przeglądarce. Jeśli spróbujesz pominąć nazwę lub wprowadzić nieprawidłową wartość ceny podczas edytowania produktu, obok pola tekstowego pojawia się gwiazdka. Jak pokazano na rysunku 5, wartość ceny, która zawiera symbol waluty, na przykład $19,95, jest uznawana za nieprawidłową. CompareValidator s `Currency` `Type` umożliwia używanie separatorów cyfr (takich jak przecinki lub kropki, w zależności od ustawień kultury) i wiodącego znaku plus lub minus, ale *nie zezwala na* symbol waluty. Takie zachowanie może Perplex użytkowników jako interfejs edycji obecnie renderuje `UnitPrice` przy użyciu formatu waluty.

[![gwiazdki pojawia się obok pól tekstowych z nieprawidłowymi danymi wejściowymi](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Ilustracja 5**. obok pól tekstowych pojawia się gwiazdka z nieprawidłowymi danymi wejściowymi ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))

Gdy walidacja działa tak, jak to jest, użytkownik musi ręcznie usunąć symbol waluty podczas edytowania rekordu, co nie jest akceptowalne. Ponadto jeśli w interfejsie edycyjnym są nieprawidłowe dane wejściowe, a przyciski Aktualizuj i Anuluj nie zostaną wywołane, program wywoła ogłaszanie zwrotne. Najlepiej, gdy przycisk Anuluj zwróci wartość DataList do stanu sprzed edycji, niezależnie od ważności danych wejściowych użytkownika. Ponadto musimy upewnić się, że dane strony są prawidłowe przed zaktualizowaniem informacji o produkcie w programie obsługi zdarzeń DataList s `UpdateCommand`, ponieważ sprawdzanie poprawności logiki po stronie klienta może zostać pominięte przez użytkowników, których przeglądarki nie obsługują języka JavaScript lub których obsługa została wyłączona.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Usuwanie symbolu waluty z pola tekstowego CenaJednostkowa EditItemTemplate s

W przypadku używania CompareValidator s `Currency``Type`sprawdzanie poprawności nie może zawierać żadnych symboli walut. Obecność takich symboli powoduje, że CompareValidator oznacza, że dane wejściowe są nieprawidłowe. Jednak nasz interfejs edycji zawiera obecnie symbol waluty w `UnitPrice` TextBox, co oznacza, że użytkownik musi jawnie usunąć symbol waluty przed zapisaniem zmian. Aby rozwiązać ten sposób, mamy trzy opcje:

1. Skonfiguruj `EditItemTemplate` tak, aby wartość TextBox `UnitPrice` nie była sformatowana jako waluta.
2. Zezwalaj użytkownikowi na wprowadzanie symbolu waluty przez usunięcie CompareValidator i zastąpienie go RegularExpressionValidator, który sprawdza prawidłowo sformatowaną wartość waluty. Wezwanie w tym miejscu polega na tym, że wyrażenie regularne służące do sprawdzania poprawności wartości walutowej nie jest tak proste jak CompareValidator i wymagało pisania kodu, jeśli chcemy uwzględnić ustawienia kultury.
3. Całkowicie Usuń kontrolkę walidacji i zależą od niestandardowej logiki walidacji po stronie serwera w programie obsługi zdarzeń `RowUpdating` GridView.

Zezwól na pracę z opcją 1 dla tego samouczka. Obecnie `UnitPrice` jest sformatowana jako wartość walutowa ze względu na wyrażenie wiązania danych dla pola tekstowego w `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Zmień instrukcję `Eval` na `Eval("UnitPrice", "{0:n2}")`, która formatuje wynik jako liczbę z dwiema cyframi precyzji. Można to zrobić bezpośrednio poprzez składnię deklaratywną lub klikając łącze Edytuj powiązania danych z `UnitPrice` TextBox w `EditItemTemplate`DataList.

W przypadku tej zmiany sformatowana cena w interfejsie edycji obejmuje przecinki jako separator grupy i kropkę jako separator dziesiętny, ale pozostawia symbol waluty.

> [!NOTE]
> W przypadku usuwania formatu waluty z interfejsu edytowalnego warto ułatwić umieszczenie symbolu waluty jako tekstu spoza pola tekstowego. Służy to jako Wskazówka dla użytkownika, że nie muszą podawać symbolu waluty.

## <a name="fixing-the-cancel-button"></a>Naprawianie przycisku Anuluj

Domyślnie formanty sieci Web walidacji emitują kod JavaScript, aby przeprowadzić walidację po stronie klienta. Gdy zostanie kliknięty przycisk, element LinkButton lub kliknięto element ImageButton, kontrolki walidacji na stronie są sprawdzane po stronie klienta przed wykonaniem ogłaszania zwrotnego. Jeśli istnieją jakieś nieprawidłowe dane, ogłaszanie zwrotne zostanie anulowane. W przypadku niektórych przycisków ważność danych może być nieistotna. w takim przypadku unieważnienie ogłaszania zwrotnego zostało anulowane z powodu nieprawidłowych danych.

Przycisk Anuluj to przykład. Załóżmy, że użytkownik wprowadza nieprawidłowe dane, takie jak pominięcie nazwy produktu, a następnie decydowanie o tym, że nie chcesz, aby zapisał produkt po wszystko i trafi przycisk Anuluj. Obecnie przycisk Anuluj wyzwala kontrolki walidacji na stronie, w której raport nie ma nazwy produktu i uniemożliwia ogłaszanie zwrotne. Użytkownik musi wpisać jakiś tekst w polu tekstowym `ProductName` po prostu, aby anulować proces edycji.

Na szczęście przycisk, element LinkButton i kliknięto element ImageButton mają [właściwość`CausesValidation`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) , która może wskazywać, czy kliknięcie przycisku powinno inicjować logikę walidacji (domyślnie `True`). Ustaw właściwość `CausesValidation` przycisku Anuluj na `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Upewnienie się, że dane wejściowe są prawidłowe w programie obsługi zdarzeń elementu UpdateCommand

Ze względu na skrypt po stronie klienta emitowany przez kontrolki walidacji, jeśli użytkownik wprowadzi nieprawidłowe dane wejściowe, kontrolki walidacji anulują wszelkie ogłaszania zwrotne zainicjowane przez przycisk, element LinkButton lub kliknięto element ImageButton, których właściwości `CausesValidation` są `True` (wartość domyślna). Jeśli jednak użytkownik zostanie odwiedzany przy użyciu przeglądarki antiquated lub jednej z nich, której obsługa języka JavaScript została wyłączona, sprawdzanie poprawności po stronie klienta nie zostanie wykonane.

Wszystkie kontrolki walidacji ASP.NET powtarzają swoją logikę weryfikacji natychmiast po odświeżeniu i raportują ogólną ważność danych wejściowych stron za pośrednictwem [właściwości`Page.IsValid`](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Jednak przepływ strony nie zostanie przerwany ani zatrzymany w sposób oparty na wartości `Page.IsValid`. Deweloperzy mogą upewnić się, że właściwość `Page.IsValid` ma wartość `True` przed kontynuowaniem kodu, który przyjmuje prawidłowe dane wejściowe.

Jeśli użytkownik ma wyłączony kod JavaScript, odwiedza naszą stronę, edytuje produkt, wprowadza wartość ceny zbyt kosztowną i klika przycisk Aktualizuj, po stronie klienta zostanie pominięte, a proces ogłaszania zwrotnego zostanie przesunięty. W przypadku ogłaszania zwrotnego ASP.NET stronę `UpdateCommand` s programu obsługi zdarzeń i występuje wyjątek podczas próby przeanalizowania zbyt kosztownych `Decimal`. Ze względu na to, że mamy obsługę wyjątków, taki wyjątek będzie obsłużony w sposób płynny, ale możemy zapobiec poślizgu nieprawidłowych danych w pierwszym miejscu przez wykonanie procedury obsługi zdarzeń `UpdateCommand`, jeśli `Page.IsValid` ma wartość `True`.

Dodaj następujący kod na początku procedury obsługi zdarzeń `UpdateCommand` bezpośrednio przed blokiem `Try`:

[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Dzięki temu produkt będzie podejmować próby aktualizacji tylko wtedy, gdy przesłane dane są prawidłowe. Większość użytkowników nie będzie w stanie ogłaszać nieprawidłowych danych z powodu weryfikacji skryptów po stronie klienta, ale użytkownicy, których przeglądarki nie obsługują języka JavaScript lub których obsługa języka JavaScript została wyłączona, mogą ominąć sprawdzanie po stronie klienta i przesłać nieprawidłowe dane.

> [!NOTE]
> Czytnik uważni będzie się odwoływać, gdy aktualizujesz dane przy użyciu widoku GridView, nie musimy jawnie sprawdzać właściwości `Page.IsValid` w klasie powiązanej z kodem strony. Jest to spowodowane tym, że widok GridView sprawdza Właściwość `Page.IsValid` dla nas i tylko kontynuuje aktualizację tylko wtedy, gdy zwróci wartość `True`.

## <a name="step-3-summarizing-data-entry-problems"></a>Krok 3. podsumowanie problemów z wprowadzaniem danych

Oprócz pięciu kontrolek weryfikacyjnych ASP.NET zawiera [kontrolkę podsumowania walidacji](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), która wyświetla `ErrorMessage` s tych kontrolek sprawdzania poprawności, które wykryły nieprawidłowe dane. Te dane podsumowujące mogą być wyświetlane jako tekst na stronie sieci Web lub przez modalne, po stronie klienta. Pozwól usłudze s ulepszyć ten samouczek, aby dołączyć element MessageBox po stronie klienta podsumowujący wszelkie problemy z walidacją.

Aby to osiągnąć, przeciągnij formant podsumowania walidacji z przybornika do projektanta. Lokalizacja kontrolki podsumowania walidacji jest niezależna, ponieważ będziemy ją skonfigurować tak, aby wyświetlała tylko podsumowanie jako element MessageBox. Po dodaniu kontrolki ustaw jej [właściwość`ShowSummary`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) na `False` i jej [właściwość`ShowMessageBox`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) na `True`. W tym przypadku wszelkie błędy sprawdzania poprawności są podsumowywane w elemencie MessageBox po stronie klienta (patrz rysunek 6).

[![błędy walidacji są sumowane w elemencie MessageBox po stronie klienta](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Ilustracja 6**. błędy sprawdzania poprawności są sumowane w elemencie MessageBox po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób zmniejszania prawdopodobieństwa wyjątków przy użyciu kontrolek weryfikacji, aby aktywnie upewnić się, że nasze dane wejściowe użytkowników są prawidłowe przed podjęciem próby użycia ich w przepływie aktualizacji. Program ASP.NET zapewnia pięć kontrolek sieci Web weryfikacji, które są przeznaczone do inspekcji określonego danych wejściowych kontrolki sieci Web i raportują z powrotem do danych wejściowych. W tym samouczku użyto dwóch z pięciu kontrolek RequiredFieldValidator i CompareValidator, aby upewnić się, że podano nazwę produktu i że cena miała format waluty o wartości większej lub równej zero.

Dodawanie kontrolek weryfikacji do interfejsu edycji DataList s jest tak proste, jak przeciąganie ich do `EditItemTemplate` z przybornika i Ustawianie kilku właściwości. Domyślnie formanty walidacji automatycznie emitują skrypt walidacji po stronie klienta; udostępniają one również weryfikację po stronie serwera na serwerze ogłaszania zwrotnego, przechowując łączny wynik we właściwości `Page.IsValid`. Aby pominąć walidację po stronie klienta po kliknięciu przycisku, element LinkButton lub kliknięto element ImageButton, należy ustawić właściwość Button-`CausesValidation` na `False`. Ponadto przed wykonaniem jakichkolwiek zadań z danymi przesłanymi na stronie ogłaszania zwrotnego upewnij się, że właściwość `Page.IsValid` zwraca `True`.

Wszystkie samouczki edycji elementu DataList zostały zbadane, więc bardzo proste edytowanie interfejsów to pole tekstowe dla nazwy produktu i innej ceny. Interfejs edytowania, jednak może zawierać kombinację różnych kontrolek sieci Web, takich jak kontrolek DropDownList, Calendars, RadioButtons, CheckBoxs i tak dalej. W naszym następnym samouczku dowiesz się, jak utworzyć interfejs, który używa różnych kontrolek sieci Web.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Dennis Patterson, Krzysztof Pespisa i Liz Shulok. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](handling-bll-and-dal-level-exceptions-vb.md)
> [dalej](customizing-the-datalist-s-editing-interface-vb.md)
