---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: Aktualizacja wsadowaC#() | Microsoft Docs
author: rick-anderson
description: Dowiedz się, jak aktualizować wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika tworzymy widok GridView, w którym każdy wiersz jest edytowalny. W danych...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: baaaf37c47cc57d90ea579a5c20949bf8cfc7a3c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583413"
---
# <a name="batch-updating-c"></a>Aktualizowanie w partiach (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) lub [Pobierz plik PDF](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Dowiedz się, jak aktualizować wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika tworzymy widok GridView, w którym każdy wiersz jest edytowalny. W warstwie dostępu do danych zawijamy wiele operacji aktualizacji w ramach transakcji, aby upewnić się, że wszystkie aktualizacje zostały wykonane pomyślnie, lub wszystkie aktualizacje zostaną wycofane.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](wrapping-database-modifications-within-a-transaction-cs.md) przedstawiono sposób rozbudowania warstwy dostępu do danych w celu dodania obsługi transakcji bazy danych. Transakcje bazy danych gwarantują, że seria instrukcji modyfikacji danych będzie traktowana jako jedna operacja niepodzielna, co gwarantuje, że wszystkie modyfikacje zakończą się niepowodzeniem lub wszystkie zakończyły się pomyślnie. Dzięki tej funkcji DAL na niskim poziomie wszystko gotowe do przeprowadzenia naszej uwagi w celu utworzenia interfejsów modyfikacji danych wsadowych.

W tym samouczku utworzymy widok GridView, w którym każdy wiersz jest edytowalny (patrz rysunek 1). Ponieważ każdy wiersz jest renderowany w interfejsie edycyjnym, nie ma potrzeby dla kolumny edycji, aktualizacji i przycisków anulowania. Zamiast tego na stronie znajdują się dwa przyciski aktualizacji produktów, które po kliknięciu należy wyliczyć z wierszy GridView i aktualizować bazę danych.

[![można edytować każdego wiersza w widoku GridView](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Rysunek 1**. edytowalny każdy wiersz w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image2.png))

Zacznij korzystać z aplikacji.

> [!NOTE]
> W samouczku [wykonywanie aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) został utworzony interfejs edytowania partii przy użyciu formantu DataList. Ten samouczek różni się od poprzedniej usługi w programie, która korzysta z widoku GridView, a aktualizacja wsadowa jest wykonywana w zakresie transakcji. Po ukończeniu tego samouczka zachęcamy do powrotu do wcześniejszego samouczka i aktualizowania go w celu korzystania z funkcji związanych z transakcjami bazy danych dodanych w poprzednim samouczku.

## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Badanie kroków umożliwiających edytowanie wszystkich wierszy GridView

Zgodnie z opisem w temacie [Omówienie wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , widok GridView oferuje wbudowaną obsługę edytowania danych źródłowych w poszczególnych wierszach. Wewnętrznie, w widoku GridView notatki, jaki wiersz jest edytowalny za pomocą [właściwości`EditIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Gdy widok GridView jest powiązany ze źródłem danych, sprawdza każdy wiersz, aby zobaczyć, czy indeks wiersza jest równy wartości `EditIndex`. Jeśli tak, to pola wiersza s są renderowane przy użyciu ich interfejsów edycji. W przypadku BoundFields interfejs edytowania jest polem tekstowym, którego właściwość `Text` ma przypisaną wartość pola danych określonego przez właściwość `DataField` s. W przypadku używanie TemplateField `EditItemTemplate` jest używana zamiast `ItemTemplate`.

Odwołaj, że przepływ pracy edytowania jest uruchamiany, gdy użytkownik kliknie przycisk Edytuj wiersza. Powoduje to odświeżenie, ustawia właściwość GridView s `EditIndex` na kliknięty indeks Row s i ponownie wiąże dane z siatką. Po kliknięciu przycisku Anuluj wiersza s na stronie ogłaszania zwrotnego `EditIndex` jest ustawiona na wartość `-1` przed ponownym powiązaniem danych z siatką. Ponieważ wiersze GridView s zaczynają indeksować na zero, ustawienie `EditIndex` na `-1` ma wpływ na wyświetlanie widoku GridView w trybie tylko do odczytu.

Właściwość `EditIndex` dobrze sprawdza się w przypadku edycji na wiersz, ale nie jest przeznaczona do edycji wsadowej. Aby można było edytować cały widok GridView, musimy mieć każdy wiersz renderowania przy użyciu interfejsu edycji. Najprostszym sposobem na to jest utworzenie miejsca, w którym każde edytowalne pole jest zaimplementowane jako TemplateField z interfejsem edycji zdefiniowanym w `ItemTemplate`.

W ciągu następnych kilku kroków utworzysz całkowicie edytowalny widok GridView. W kroku 1 zaczniemy od utworzenia widoku GridView i jego elementu ObjectDataSource i przekonwertowania jego BoundFields i CheckBoxField na używanie TemplateField. W krokach 2 i 3 przeniesiemy interfejsy edycji z używanie TemplateField `EditItemTemplate` s do `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Krok 1. Wyświetlanie informacji o produkcie

Przed zajmować się tworzeniem widoku GridView, gdzie są edytowalne wiersze, Zacznij od samego wyświetlenia informacji o produkcie. Otwórz stronę `BatchUpdate.aspx` w folderze `BatchData` i przeciągnij widok GridView z przybornika na projektanta. Ustaw `ID` GridView s na `ProductsGrid` i, z jego tagu inteligentnego, wybierz powiązanie go z nowym elementem ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj element ObjectDataSource, aby pobierać jego dane z metody `GetProducts` `ProductsBLL` Class.

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLL](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Rysunek 2**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-updating-cs/_static/image4.png))

[![pobrać danych produktu przy użyciu metody getproductss](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Rysunek 3**. Pobieranie danych produktu przy użyciu metody `GetProducts` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-updating-cs/_static/image6.png))

Podobnie jak w widoku GridView, funkcje modyfikacji elementu ObjectDataSource są przeznaczone do pracy w poszczególnych wierszach. Aby można było zaktualizować zestaw rekordów, musimy napisać bit kodu w klasie ASP.NET Page s, która tworzy dane i przekazuje je do LOGIKI biznesowej. W związku z tym należy ustawić listy rozwijane na kartach Usuń lub Dodaj, a następnie usunąć do (brak). Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![ustawić listy rozwijane na kartach Aktualizuj, Wstaw i Usuń do (brak).](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Rysunek 4**. Ustawianie list rozwijanych w kartach Aktualizuj, Wstaw i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-updating-cs/_static/image8.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych, znaczniki deklaratywne programu ObjectDataSource powinny wyglądać następująco:

[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Ukończenie kreatora konfigurowania źródła danych powoduje również, że program Visual Studio tworzy BoundFields i CheckBoxField dla pól danych produktu w widoku GridView. W tym samouczku pozwól, aby użytkownicy mogli wyświetlać i edytować nazwę produktu, kategorię, cenę i stan wycofany. Usuń wszystkie pola oprócz `ProductName`, `CategoryName`, `UnitPrice`i `Discontinued` i Zmień nazwę właściwości `HeaderText` pierwszych trzech pól na produkt, kategorię i cenę. Na koniec zaznacz pola wyboru Włącz stronicowanie i Włącz sortowanie w tagów inteligentnych GridView.

W tym momencie widok GridView ma trzy BoundFields (`ProductName`, `CategoryName`i `UnitPrice`) i CheckBoxField (`Discontinued`). Musimy przekonwertować te cztery pola na używanie TemplateField, a następnie przenieść interfejs edycji z TemplateField s `EditItemTemplate` do jego `ItemTemplate`.

> [!NOTE]
> Zbadamy tworzenie i dostosowywanie używanie TemplateField w samouczku [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) . Przeprowadzimy procedurę konwertowania BoundFields i CheckBoxField na używanie TemplateField i definiując ich interfejsy edycji w `ItemTemplate` s, ale w przypadku zablokowania lub potrzeby odświeżacza, nie niechętnie zezwalają, aby odwołać się do tego wcześniejszego samouczka.

W tagu inteligentnym GridView kliknij link Edytuj kolumny, aby otworzyć okno dialogowe pola. Następnie zaznacz każde pole i kliknij łącze Konwertuj to pole na TemplateField.

![Konwertuj istniejące BoundFields i CheckBoxField na TemplateField](batch-updating-cs/_static/image5.gif)

**Rysunek 5**. Konwersja istniejących BoundFields i CheckBoxField na TemplateField

Teraz, gdy każde pole jest TemplateField, przygotowano do przeniesienia interfejsu edycji z `EditItemTemplate` s do `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Krok 2. tworzenie interfejsów`ProductName`,`UnitPrice`i`Discontinued`edytowania

Tworzenie `ProductName`, `UnitPrice`i `Discontinued` interfejsów edytowania jest tematem tego kroku i są dość proste, ponieważ każdy interfejs jest już zdefiniowany w `EditItemTemplate`TemplateField s. Tworzenie `CategoryName` interfejsu edytowania jest nieco bardziej zaliczane, ponieważ musimy utworzyć DropDownList odpowiednich kategorii. Ten interfejs `CategoryName` edytowania jest omawiany w kroku 3.

Zacznij od `ProductName` TemplateField. Kliknij link Edytuj szablony z tagu inteligentnego GridView s i przejdź do szczegółów `ProductName` TemplateField s `EditItemTemplate`. Zaznacz pole tekstowe, skopiuj je do schowka, a następnie wklej je do `ProductName` TemplateField s `ItemTemplate`. Zmień właściwość TextBox s `ID` na `ProductName`.

Następnie Dodaj RequiredFieldValidator do `ItemTemplate`, aby upewnić się, że użytkownik poda wartość dla każdej nazwy produktu. Ustaw właściwość `ControlToValidate` na ProductName, właściwość `ErrorMessage` na wartość należy podać nazwę produktu. i Właściwość `Text`, aby \*. Po wprowadzeniu tych dodatków do `ItemTemplate`ekran powinien wyglądać podobnie do rysunku 6.

[![ProductName TemplateField teraz zawiera pole tekstowe i RequiredFieldValidator](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Ilustracja 6**. `ProductName` TemplateField teraz zawiera pole tekstowe i RequiredFieldValidator ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-updating-cs/_static/image10.png))

Dla interfejsu edycji `UnitPrice` Zacznij od skopiowania pola tekstowego z `EditItemTemplate` do `ItemTemplate`. Następnie umieść $ na początku pola tekstowego i ustaw jego właściwość `ID` na CenaJednostkowa i Właściwość `Columns` na 8.

Dodaj także CompareValidator do `ItemTemplate` `UnitPrice`, aby upewnić się, że wartość wprowadzona przez użytkownika jest prawidłową wartością waluty większą lub równą $0,00. Ustaw właściwość `ControlToValidate` modułu sprawdzania poprawności na wartość CenaJednostkowa, jej Właściwość `ErrorMessage` na wartość, a następnie wprowadź poprawną walutę. Pomiń wszystkie symbole walut. Właściwość `Text` do \*, jej Właściwość `Type` do `Currency`, jej Właściwość `Operator` do `GreaterThanEqual`, a jej Właściwość `ValueToCompare` równa 0.

[![dodać CompareValidator, aby upewnić się, że wprowadzona cena jest wartością nieujemną waluty](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Rysunek 7**. Dodawanie CompareValidator, aby upewnić się, że wprowadzona cena jest wartością nieujemnej waluty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-updating-cs/_static/image12.png))

Dla `Discontinued` TemplateField można użyć pola wyboru już zdefiniowanego w `ItemTemplate`. Po prostu ustaw jej `ID` na niekontynuację i Właściwość `Enabled` do `true`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Krok 3. Tworzenie interfejsu edycji`CategoryName`

Interfejs edycji w `CategoryName` TemplateField s `EditItemTemplate` zawiera pole tekstowe, w którym jest wyświetlana wartość pola danych `CategoryName`. Musimy zastąpić ten element DropDownList, który zawiera listę możliwych kategorii.

> [!NOTE]
> Samouczek [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) zawiera bardziej szczegółowe i kompletne Omówienie dostosowywania szablonu w celu uwzględnienia DropDownList, w przeciwieństwie do pola tekstowego. Po wykonaniu tych czynności są one prezentowane w tersely. Dokładniejsze omówienie tworzenia i konfigurowania kategorii DropDownList można znaleźć w samouczku [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

Przeciągnij DropDownList z przybornika do `ItemTemplate``CategoryName` TemplateField s, ustawiając `ID` na `Categories`. W tym momencie zwykle definiujemy źródło danych kontrolek DropDownList s za pomocą jego tagu inteligentnego, tworząc nowy element ObjectDataSource. Jednak spowoduje to dodanie elementu ObjectDataSource w ramach `ItemTemplate`, co spowoduje utworzenie wystąpienia klasy ObjectDataSource dla każdego wiersza GridView. Zamiast tego należy utworzyć element ObjectDataSource poza używanie TemplateField GridView. Zakończ edycję szablonu i przeciągnij element ObjectDataSource z przybornika do projektanta znajdującego się pod `ProductsDataSource` elemencie ObjectDataSource. Nazwij nowy element ObjectDataSource `CategoriesDataSource` i skonfiguruj go tak, aby korzystał z metody `GetCategories` klasy `CategoriesBLL`.

[![skonfigurować element ObjectDataSource do używania klasy CategoriesBLL](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Ilustracja 8**. Konfigurowanie elementu ObjectDataSource do używania klasy `CategoriesBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-updating-cs/_static/image14.png))

[![pobrać danych kategorii za pomocą metody getcategorys](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Ilustracja 9**. Pobieranie danych kategorii za pomocą metody `GetCategories` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image16.png))

Ponieważ ten element ObjectDataSource jest używany wyłącznie do pobierania danych, Ustaw listę rozwijaną na kartach Aktualizuj i Usuń na (brak). Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![ustawić listy rozwijane na kartach Aktualizuj i Usuń do (brak)](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Ilustracja 10**. Ustawianie list rozwijanych na kartach Aktualizuj i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-updating-cs/_static/image18.png))

Po zakończeniu działania kreatora, znaczniki deklaratywne `CategoriesDataSource` s powinny wyglądać następująco:

[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

Po utworzeniu i skonfigurowaniu `CategoriesDataSource` Wróć do `CategoryName` TemplateField s `ItemTemplate` i z taga inteligentnego DropDownList s kliknij link wybierz źródło danych. W Kreatorze konfiguracji źródła danych wybierz opcję `CategoriesDataSource` z pierwszej listy rozwijanej i wybierz, aby `CategoryName` do wyświetlania i `CategoryID` jako wartość.

[![powiązać DropDownList z CategoriesDataSource](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Ilustracja 11**. powiązanie DropDownList z `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image20.png))

W tym momencie `Categories` DropDownList wyświetla wszystkie kategorie, ale nie jest jeszcze automatycznie wybierać odpowiedniej kategorii dla produktu powiązanego z wierszem GridView. Aby to osiągnąć, należy ustawić `Categories` DropDownList s `SelectedValue` na wartość `CategoryID` produktu. Kliknij link Edytuj powiązania danych z tagu inteligentnego DropDownList s i Skojarz właściwość `SelectedValue` z polem danych `CategoryID`, jak pokazano na rysunku 12.

![Powiąż wartość IDKategorii produktu z właściwością DropDownList s SelectedValue](batch-updating-cs/_static/image12.gif)

**Rysunek 12**. Powiąż wartość `CategoryID` produktu z właściwością DropDownList s `SelectedValue`

Ten ostatni problem pozostaje: Jeśli produkt nie ma określonej wartości `CategoryID`, instrukcja DataBinding na `SelectedValue` spowoduje wyjątek. Wynika to z faktu, że DropDownList zawiera tylko elementy dla kategorii i nie oferuje opcji dla tych produktów, które mają `NULL` wartość bazy danych dla `CategoryID`. Aby to naprawić, ustaw właściwość DropDownList `AppendDataBoundItems` s na `true` i Dodaj nowy element do DropDownList, pomijając Właściwość `Value` ze składni deklaracyjnej. Oznacza to, że składnia deklaracyjne `Categories` DropDownList s wygląda następująco:

[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Zauważ, jak `<asp:ListItem Value="">` — wybierz jeden — ma atrybut `Value` jawnie ustawiony jako pusty ciąg. Zapoznaj się z artykułem [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) , aby zapoznać się z bardziej dokładną dyskusją na temat tego, dlaczego dodatkowy element DropDownList jest wymagany do obsługi przypadku `NULL` i dlaczego przypisanie właściwości `Value` do pustego ciągu jest niezbędne.

> [!NOTE]
> W tym miejscu istnieje potencjalne problemy dotyczące wydajności i skalowalności. Ponieważ każdy wiersz ma DropDownList, który używa `CategoriesDataSource` jako źródła danych, Metoda `CategoriesBLL` klasy s `GetCategories` będzie wywoływana *n* razy na stronie, gdzie *n* to liczba wierszy w GridView. Te *n* wywołania do `GetCategories` powodują, że w *n* zapytania do bazy danych. Ten wpływ na bazę danych można zmniejszyć przez buforowanie zwracanych kategorii w pamięci podręcznej żądania lub za pośrednictwem warstwy buforowania przy użyciu zależności pamięci podręcznej SQL lub bardzo krótki czas wygaśnięcia. Aby uzyskać więcej informacji na temat opcji buforowania na żądanie, zobacz [`HttpContext.Items` magazynu pamięci podręcznej na żądanie](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).

## <a name="step-4-completing-the-editing-interface"></a>Krok 4. Korzystanie z interfejsu edycji

Wprowadziliśmy wiele zmian w szablonach GridView i nie wstrzymuje się w celu wyświetlenia naszego postępu. Poświęć chwilę na wyświetlenie postępu w przeglądarce. Jak pokazano na rysunku 13, każdy wiersz jest renderowany przy użyciu `ItemTemplate`, który zawiera interfejs edycji komórki.

[![każdy wiersz GridView jest edytowalny](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Ilustracja 13**. Każdy wiersz GridView jest edytowalny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image22.png))

Istnieje kilka drobnych problemów z formatowaniem, które należy zachować na tym etapie. Najpierw należy zauważyć, że wartość `UnitPrice` zawiera cztery dziesiętne punkty. Aby rozwiązać ten problem, Wróć do `UnitPrice` TemplateField s `ItemTemplate` i z taga HTML TextBox s kliknij łącze Edytuj powiązania danych. Następnie określ, że właściwość `Text` powinna być sformatowana jako liczba.

![Sformatuj właściwość Text jako liczbę](batch-updating-cs/_static/image14.gif)

**Ilustracja 14**. formatowanie właściwości `Text` jako liczby

Po drugie, Zezwól na wyśrodkowanie pola wyboru w kolumnie `Discontinued` (zamiast wyrównania do lewej). Kliknij pozycję Edytuj kolumny w tagu inteligentnym GridView s i wybierz `Discontinued` TemplateField z listy pól w lewym dolnym rogu. Przejdź do szczegółów `ItemStyle` i ustaw właściwość `HorizontalAlign` na Wyśrodkuj, jak pokazano na rysunku 15.

![Wyśrodkuj pole wyboru](batch-updating-cs/_static/image15.gif)

**Ilustracja 15**. wyśrodkowanie pola wyboru `Discontinued`

Następnie Dodaj kontrolkę podsumowania walidacji do strony i ustaw jej Właściwość `ShowMessageBox` na `true` i jej Właściwość `ShowSummary` na `false`. Dodaj również kontrolki sieci Web przycisku, które po kliknięciu spowodują zaktualizowanie zmian użytkownika. W tym celu Dodaj dwa kontrolki sieci Web Button, jeden nad GridView i jeden poniżej, ustawienie obydwu formantów `Text` właściwości, aby zaktualizować produkty.

Ponieważ interfejs edytowania GridView jest zdefiniowany w używanie TemplateField `ItemTemplate` s, `EditItemTemplate` s są zbędne i mogą zostać usunięte.

Po wprowadzeniu powyżej określonego formatowania, dodania kontrolek przycisk i usunięciu niepotrzebnych `EditItemTemplate` s, składnia deklaracyjnej strony powinna wyglądać następująco:

[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

Rysunek 16 pokazuje Tę stronę, gdy oglądasz za pomocą przeglądarki po dodaniu kontrolek sieci Web przycisku i wprowadzeniu zmian formatowania.

[![Strona zawiera teraz dwa przyciski aktualizacji produktów](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Ilustracja 16**. Strona zawiera teraz dwa przyciski aktualizacji produktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-updating-cs/_static/image24.png))

## <a name="step-5-updating-the-products"></a>Krok 5. aktualizowanie produktów

Gdy użytkownik odwiedza tę stronę, wprowadzi zmiany, a następnie klika jeden z dwóch przycisków aktualizacji produktów. W tym momencie musimy w jakiś sposób zapisać wartości wprowadzone przez użytkownika dla każdego wiersza w wystąpieniu `ProductsDataTable`, a następnie przekazać je do metody LOGIKI biznesowej, która następnie przekaże to wystąpienie `ProductsDataTable` do metody `UpdateWithTransaction` DAL. Metoda `UpdateWithTransaction`, która została utworzona w [poprzednim samouczku](wrapping-database-modifications-within-a-transaction-cs.md), zapewnia, że partia zmian zostanie zaktualizowana jako operacja niepodzielna.

Utwórz metodę o nazwie `BatchUpdate` w `BatchUpdate.aspx.cs` i Dodaj następujący kod:

[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Ta metoda rozpoczyna się od pobrania wszystkich produktów z powrotem w `ProductsDataTable` za pośrednictwem wywołania metody `GetProducts` LOGIKI biznesowej s. Następnie wylicza `ProductGrid` [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx)GridView. Kolekcja `Rows` zawiera [wystąpienie`GridViewRow`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) dla każdego wiersza wyświetlanego w widoku GridView. Ponieważ pokazujemy co najwyżej dziesięć wierszy na stronie, kolekcja `Rows` GridView nie będzie zawierać więcej niż dziesięciu elementów.

Dla każdego wiersza `ProductID` jest pobierana z kolekcji `DataKeys` i wybierana jest odpowiednia `ProductsRow` z `ProductsDataTable`. Cztery kontrolki danych wejściowych TemplateField są programowo przywoływane i ich wartości przypisane do właściwości wystąpienia `ProductsRow` s. Po zastosowaniu wszystkich wartości wiersza w wierszu GridView w celu zaktualizowania `ProductsDataTable`są one przenoszone do metody `UpdateWithTransaction` LOGIKI biznesowej s, która zgodnie z poprzednią częścią samouczka, po prostu wywołuje metodę `UpdateWithTransaction` DAL s.

Algorytm aktualizacji wsadowych użyty w tym samouczku aktualizuje każdy wiersz w `ProductsDataTable`, który odnosi się do wiersza w widoku GridView, niezależnie od tego, czy informacje o produkcie zostały zmienione. Chociaż takie aktualizacje niewidome nie są zwykle przyczyną problemów z wydajnością, mogą powodować zbędne rekordy w przypadku ponownego inspekcji zmian w tabeli bazy danych. Z powrotem w samouczku [wykonywanie aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) został zbadany interfejs aktualizacji partii z elementem DataList i dodany kod, który zaktualizuje tylko te rekordy, które zostały rzeczywiście zmodyfikowane przez użytkownika. W razie potrzeby możesz korzystać z technik [przeprowadzania aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) w celu zaktualizowania kodu w tym samouczku.

> [!NOTE]
> Po powiązaniu źródła danych z GridView przez jego tag inteligentny program Visual Studio automatycznie przypisuje wartość klucza podstawowego źródła danych do właściwości GridView `DataKeyNames`. Jeśli element ObjectDataSource nie został powiązany ze znacznikiem GridView za pomocą tagu inteligentnego GridView s, jak opisano w kroku 1, należy ręcznie ustawić właściwość GridView s `DataKeyNames` na wartość ProductID w celu uzyskania dostępu do `ProductID` wartości dla każdego wiersza za pomocą kolekcji `DataKeys`.

Kod używany w `BatchUpdate` jest podobny do tego, który został użyty w metodach `UpdateProduct` LOGIKI biznesowej. główna różnica polega na tym, że w metodach `UpdateProduct` tylko pojedyncze wystąpienie `ProductRow` jest pobierane z architektury. Kod, który przypisuje właściwości `ProductRow`, jest taki sam między metodami `UpdateProducts` i kodem w pętli `foreach` w `BatchUpdate`, zgodnie z ogólnym wzorcem.

Aby ukończyć ten samouczek, musimy wywołać metodę `BatchUpdate`, gdy zostanie kliknięty jeden z przycisków aktualizacji produktów. Utwórz programy obsługi zdarzeń dla zdarzeń `Click` tych dwóch kontrolek przycisków i Dodaj następujący kod do programów obsługi zdarzeń:

[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

Pierwsze wywołanie jest nawiązywane do `BatchUpdate`. Następnie `ClientScript property` służy do iniekcji kodu JavaScript, który będzie wyświetlał element MessageBox, który odczytuje produkty, które zostały zaktualizowane.

Poświęć chwilę na przetestowanie tego kodu. Odwiedź witrynę `BatchUpdate.aspx` za pomocą przeglądarki, Edytuj kilka wierszy, a następnie kliknij jeden z przycisków Aktualizuj produkty. Przy założeniu, że nie występują błędy walidacji danych wejściowych, zobaczysz element MessageBox, który odczytuje produkty, które zostały zaktualizowane. Aby sprawdzić niepodzielność aktualizacji, rozważ dodanie ograniczenia `CHECK` losowego, takiego jak taki, który nie zezwala na `UnitPrice` wartości 1234,56. Następnie z `BatchUpdate.aspx`edytować wiele rekordów, pamiętając o ustawieniu jednej z wartości `UnitPrice` produktu na wartość zabroniona (1234,56). Powinno to spowodować błąd po kliknięciu przycisku Aktualizuj produkty z innymi zmianami w trakcie tej operacji wsadowej z powrotem do ich oryginalnych wartości.

## <a name="an-alternativebatchupdatemethod"></a>Alternatywna metoda`BatchUpdate`

Właśnie zbadana Metoda `BatchUpdate` pobiera *wszystkie* produkty z metody logiki biznesowej s `GetProducts` a następnie aktualizuje tylko te rekordy, które są wyświetlane w widoku GridView. Takie podejście jest idealne, jeśli GridView nie używa stronicowania, ale jeśli tak, mogą być setki, tysiące lub dziesiątki tysięcy produktów, ale tylko dziesięć wierszy w GridView. W takim przypadku pobieranie wszystkich produktów z bazy danych tylko do modyfikacji 10 z nich jest mniejsze niż idealne.

W przypadku tych typów sytuacji Rozważ użycie następującej metody `BatchUpdateAlternate`:

[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate` uruchamia się, tworząc nowe puste `ProductsDataTable` o nazwie `products`. Następnie kroki w kolekcji `Rows` GridView i dla każdego wiersza pobiera informacje o produkcie przy użyciu metody `GetProductByProductID(productID)` LOGIKI biznesowej s. Pobrane wystąpienie `ProductsRow` ma zaktualizowane właściwości w taki sam sposób jak `BatchUpdate`, ale po zaktualizowaniu wiersza, który jest importowany do `products``ProductsDataTable` za pomocą [metody`ImportRow(DataRow)`](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx)DataTable s.

Po zakończeniu `foreach` pętli `products` zawiera jedno wystąpienie `ProductsRow` dla każdego wiersza w widoku GridView. Ponieważ każde wystąpienie `ProductsRow` zostało dodane do `products` (zamiast aktualizacji), jeśli nie przejdziemy do metody `UpdateWithTransaction`, `ProductsTableAdapter` spróbuje wstawić każdy rekord do bazy danych. Zamiast tego musimy określić, że każdy z tych wierszy został zmodyfikowany (nie został dodany).

Można to osiągnąć przez dodanie nowej metody do LOGIKI biznesowej o nazwie `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, pokazane poniżej, ustawia `RowState` każdego wystąpienia `ProductsRow` w `ProductsDataTable`, aby `Modified`, a następnie przekazuje `ProductsDataTable` do metody DAL s `UpdateWithTransaction`.

[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Podsumowanie

Widok GridView zawiera wbudowane możliwości edytowania na wiersz, ale nie obsługuje tworzenia w pełni edytowalnych interfejsów. Jak zostało to opisane w tym samouczku, takie interfejsy są możliwe, ale wymagają one bitu pracy. Aby utworzyć widok GridView, w którym każdy wiersz jest edytowalny, musimy przekonwertować pola GridView w używanie TemplateField i zdefiniować interfejs edycji w `ItemTemplate` s. Ponadto należy dodać do strony wszystkie kontrolki sieci Web typu Button, niezależnie od widoku GridView. Te przyciski `Click` programy obsługi zdarzeń muszą wyliczyć kolekcję GridView `Rows`, przechować zmiany w `ProductsDataTable`i przekazać zaktualizowane informacje do odpowiedniej metody LOGIKI biznesowej.

W następnym samouczku zobaczymy, jak utworzyć interfejs do usuwania wsadowego. W szczególności każdy wiersz GridView będzie zawierać pole wyboru, a nie polecenie Aktualizuj wszystkie przyciski, a my usunie przyciski z zaznaczonymi wierszami.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Teresa Murphy i David suru. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](wrapping-database-modifications-within-a-transaction-cs.md)
> [dalej](batch-deleting-cs.md)
