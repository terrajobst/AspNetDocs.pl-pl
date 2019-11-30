---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Dostosowywanie interfejsu modyfikacji danych (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku pokazano, jak dostosować interfejs edytowalnego widoku GridView, zastępując standardowe pole tekstowe i kontrolki CheckBox alternati...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: af68d1a0b744a2c1fd9d21a8bf6165bafa4de683
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624235"
---
# <a name="customizing-the-data-modification-interface-c"></a>Dostosowywanie interfejsu modyfikacji danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) lub [Pobierz plik PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> W tym samouczku zawarto informacje na temat dostosowywania interfejsu edytowalnego widoku GridView przez zastąpienie standardowego pola tekstowego i kontrolek CheckBox alternatywnymi wejściowymi formantami sieci Web.

## <a name="introduction"></a>Wprowadzenie

BoundFields i CheckBoxFields używane przez kontrolki GridView i DetailsView upraszczają proces modyfikowania danych ze względu na możliwość renderowania interfejsów tylko do odczytu, edytowalnych i umożliwiających Wstawianie. Te interfejsy mogą być renderowane bez konieczności dodawania dodatkowych znaczników lub kodu deklaratywnego. Jednak interfejsy BoundField i CheckBoxField nie szerszym często są używane w rzeczywistych scenariuszach. W celu dostosowania interfejsu edytowalnego lub umożliwiającego wstawienie w widoku GridView lub DetailsView należy zamiast tego użyć TemplateField.

W [poprzednim samouczku](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) pokazano, jak dostosować interfejsy modyfikacji danych, dodając kontrolki sieci Web walidacji. W tym samouczku pokazano, jak dostosować rzeczywiste kontrolki sieci Web zbierania danych, zastępując standardowe pole tekstowe i kontrolki pola wyboru BoundField i CheckBoxField z alternatywnymi kontrolkami sieci Web. W szczególności utworzymy edytowalny widok GridView, który umożliwia zaktualizowanie produktu nazwa, Kategoria, dostawca i stan wycofany. Podczas edytowania określonego wiersza pola Kategoria i dostawca będą renderowane jako kontrolek DropDownList, zawierające zestaw dostępnych kategorii i dostawców do wyboru. Ponadto zamienimy domyślne pole wyboru CheckBoxField z kontrolką RadioButtonList, która oferuje dwie opcje: "Active" i "uncontinueed".

[![interfejs edytowania GridView zawiera kontrolek DropDownList i RadioButtons](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Rysunek 1**. interfejs edytowania GridView zawiera kontrolek DropDownList i RadioButtons ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image3.png))

## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Krok 1. Tworzenie odpowiedniego przeciążenia`UpdateProduct`

W tym samouczku utworzymy edytowalny widok GridView, który umożliwia edytowanie nazwy produktu, kategorii, dostawcy i nieprawidłowego stanu. W związku z tym potrzebujemy `UpdateProduct` Przeciążenie, które akceptuje pięć parametrów wejściowych te cztery wartości produktu i `ProductID`. Podobnie jak w przypadku poprzednich przeciążeń:

1. Pobierz informacje o produkcie z bazy danych dla określonego `ProductID`,
2. Zaktualizuj pola `ProductName`, `CategoryID`, `SupplierID`i `Discontinued`, a następnie
3. Wyślij żądanie aktualizacji do DAL za pomocą metody `Update()` TableAdapter.

W przypadku usługi zwięzłości w przypadku tego konkretnego przeciążenia pominięto kontrolę reguły biznesowej, która gwarantuje, że produkt oznaczony jako wycofany nie jest jedynym produktem oferowanym przez jego dostawcę. Możesz ją dodać w, jeśli wolisz, lub, w idealnym przypadku, Refaktoryzacja logiki do oddzielnej metody.

Poniższy kod przedstawia nowe Przeciążenie `UpdateProduct` w klasie `ProductsBLL`:

[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>Krok 2. podtworzenie edytowalnego widoku GridView

Po dodaniu przeciążenia `UpdateProduct` jesteśmy gotowi do utworzenia naszego edytowalnego widoku GridView. Otwórz stronę `CustomizedUI.aspx` w folderze `EditInsertDelete` i Dodaj kontrolkę GridView do projektanta. Następnie utwórz nowy element ObjectDataSource z tagu inteligentnego GridView. Skonfiguruj element ObjectDataSource w taki sposób, aby pobierał informacje o produkcie za pomocą metody `GetProducts()` klasy `ProductBLL` i zaktualizować dane produktu przy użyciu przeciążenia `UpdateProduct`, który właśnie został utworzony. Na kartach Wstawianie i usuwanie wybierz pozycję (brak) z listy rozwijanej.

[![skonfigurować element ObjectDataSource do używania właśnie utworzonego przeciążenia UpdateProduct](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Rysunek 2**. Konfigurowanie elementu ObjectDataSource do używania właśnie utworzonego przeciążenia `UpdateProduct` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image6.png))

Jak widać w całym samouczku modyfikującym dane, składnia deklaracyjnego elementu ObjectDataSource utworzonego przez program Visual Studio przypisuje Właściwość `OldValuesParameterFormatString` do `original_{0}`. Oczywiście nie współpracują z naszą warstwą logiki biznesowej, ponieważ nasze metody nie oczekują, że oryginalna `ProductID` wartość do przekazania. W związku z tym, jak zrobiono w poprzednich samouczkach, poświęć chwilę na usunięcie tego przypisania właściwości ze składni deklaracyjnej lub zamiast tego ustaw wartość tej właściwości na `{0}`.

Po tej zmianie znaczniki deklaratywne elementu ObjectDataSource powinny wyglądać następująco:

[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Należy zauważyć, że właściwość `OldValuesParameterFormatString` została usunięta i że istnieje `Parameter` w kolekcji `UpdateParameters` dla każdego z parametrów wejściowych oczekiwanych przez nasze Przeciążenie `UpdateProduct`.

Gdy element ObjectDataSource jest skonfigurowany do aktualizowania tylko podzbioru wartości produktu, w widoku GridView są obecnie wyświetlane *wszystkie* pola produktu. Zapoznaj się z chwilą, aby edytować widok GridView w taki sposób, aby:

- Zawiera tylko `ProductName`, `SupplierName`, `CategoryName` BoundFields i `Discontinued` CheckBoxField
- Pola `CategoryName` i `SupplierName`, które pojawią się przed (po lewej stronie) `Discontinued` CheckBoxField
- Właściwość `CategoryName` i `SupplierName` BoundFields "`HeaderText` jest odpowiednio ustawiona na" Category "i" dostawca "
- Obsługa edycji jest włączona (zaznacz pole wyboru Włącz edycję w tagu inteligentnym GridView)

Po wprowadzeniu tych zmian Projektant będzie wyglądać podobnie do rysunku 3, z składni deklaracyjnej GridView pokazanej poniżej.

[![usunąć niepotrzebne pola z widoku GridView](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Rysunek 3**. Usuwanie niepotrzebnych pól z widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image9.png))

[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

W tym momencie zachowanie tylko do odczytu widoku GridView zostało zakończone. Podczas przeglądania danych każdy produkt jest renderowany jako wiersz w widoku GridView, pokazując nazwę produktu, kategorię, dostawcę i stan wycofany.

[![interfejs GridView tylko do odczytu został ukończony](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Ilustracja 4**. interfejs GridView "tylko do odczytu" został ukończony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image12.png))

> [!NOTE]
> Zgodnie z opisem w temacie [Omówienie wstawiania, aktualizowania i usuwania danych](an-overview-of-inserting-updating-and-deleting-data-cs.md), należy pamiętać, że stan widoku GridView jest włączony (domyślne zachowanie). Jeśli ustawisz właściwość GridView s `EnableViewState` na `false`, zostanie uruchomione ryzyko przypadkowego usunięcia lub edycji rekordów przez współbieżnych użytkowników. Zobacz [Ostrzeżenie: problem współbieżności z elementem ASP.NET 2,0 gridviews/DetailsView/FormView, który obsługuje edytowanie i/lub usuwanie stanu widoku,](http://scottonwriting.net/sowblog/posts/10054.aspx) Aby uzyskać więcej informacji.

## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Krok 3. Używanie DropDownList dla interfejsów edytowania kategorii i dostawcy

Odwołaj, że obiekt `ProductsRow` zawiera właściwości `CategoryID`, `CategoryName`, `SupplierID`i `SupplierName`, które zapewniają rzeczywiste wartości identyfikatora klucza obcego w tabeli `Products` bazy danych i odpowiednie wartości `Name` w tabelach `Categories` i `Suppliers`. `CategoryID` `ProductRow`i `SupplierID` mogą być odczytywane i zapisywane w, podczas gdy właściwości `CategoryName` i `SupplierName` są oznaczone jako tylko do odczytu.

Ze względu na stan tylko do odczytu właściwości `CategoryName` i `SupplierName`, odpowiednie BoundFields mają ustawioną właściwość `ReadOnly` na `true`, uniemożliwiając modyfikację tych wartości podczas edytowania wiersza. Mimo że można ustawić właściwość `ReadOnly` na `false`, renderowanie `CategoryName` i `SupplierName` BoundFields jako pól tekstowych podczas edycji, takie podejście spowoduje wyjątek, gdy użytkownik próbuje zaktualizować produkt, ponieważ nie ma żadnego przeciążenia `UpdateProduct`, które pobiera `CategoryName` i `SupplierName` dane wejściowe. W rzeczywistości nie chcemy tworzyć takiego przeciążenia z dwóch powodów:

- Tabela `Products` nie zawiera pól `SupplierName` lub `CategoryName`, ale `SupplierID` i `CategoryID`. W związku z tym chcemy, aby nasza metoda przekazała te wartości identyfikatorów, a nie ich wartości tabel odnośników.
- Wymaganie od użytkownika wpisania nazwy dostawcy lub kategorii jest mniejsze niż idealne, ponieważ wymaga od użytkownika znajomości dostępnych kategorii i dostawców oraz ich prawidłowych pisowni.

Pola dostawca i Kategoria powinny wyświetlać nazwy kategorii i dostawców w trybie tylko do odczytu (jak teraz) i listę rozwijaną odpowiednich opcji podczas edytowania. Korzystając z listy rozwijanej, użytkownik końcowy może szybko zobaczyć, jakie kategorie i dostawcy są dostępne do wyboru, i ułatwić wybór.

Aby zapewnić takie zachowanie, musimy przekonwertować `SupplierName` i `CategoryName` BoundFields na używanie TemplateField, którego `ItemTemplate` emitują wartości `SupplierName` i `CategoryName`, których `EditItemTemplate` używa formantu DropDownList w celu wyświetlenia listy dostępnych kategorii i dostawców.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Dodawanie`Categories`i`Suppliers`kontrolek DropDownList

Zacznij od przekonwertowania `SupplierName` i `CategoryName` BoundFields na używanie TemplateField przez: kliknięcie łącza Edytuj kolumny w tagu inteligentnym GridView; wybranie BoundField z listy w lewym dolnym rogu; i kliknięcie linku "Konwertuj to pole na TemplateField". Proces konwersji spowoduje utworzenie TemplateField z zarówno `ItemTemplate`, jak i `EditItemTemplate`, jak pokazano w składni deklaracyjnej poniżej:

[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

Ponieważ BoundField został oznaczony jako tylko do odczytu, zarówno `ItemTemplate`, jak i `EditItemTemplate` zawierają kontrolkę sieci Web etykiety, której Właściwość `Text` jest powiązana z odpowiednim polem danych (`CategoryName`w powyższej składni). Musimy zmodyfikować `EditItemTemplate`, zastępując formant sieci Web etykiety kontrolką DropDownList.

Jak widać w poprzednich samouczkach, szablon można edytować za pomocą projektanta lub bezpośrednio ze składni deklaratywnej. Aby edytować go za pomocą projektanta, kliknij link Edytuj szablony z tagu inteligentnego GridView i wybierz, aby współpracować z `EditItemTemplate`pola kategorii. Usuń formant sieci Web etykiety i zastąp go kontrolką DropDownList, ustawiając właściwość identyfikatora DropDownList na `Categories`.

[![usunąć TexBox i dodać DropDownList do EditItemTemplate](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Rysunek 5**. Usuń TexBox i Dodaj DropDownList do `EditItemTemplate` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image15.png))

Następnym razem musimy wypełnić DropDownListą dostępne kategorie. Kliknij link wybierz źródło danych z tagu inteligentnego DropDownList i wybierz opcję Utwórz nowy element ObjectDataSource o nazwie `CategoriesDataSource`.

[![utworzyć nową kontrolkę ObjectDataSource o nazwie CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Ilustracja 6**. Utwórz nową kontrolkę ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image18.png))

Aby ten element ObjectDataSource zwracał wszystkie kategorie, powiąż go z metodą `GetCategories()` klasy `CategoriesBLL`.

[![powiązać element ObjectDataSource z metodą GetCategories () CategoriesBLL](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Rysunek 7**. powiązanie elementu ObjectDataSource z metodą `GetCategories()` `CategoriesBLL`([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image21.png))

Na koniec Skonfiguruj ustawienia DropDownList w taki sposób, aby pole `CategoryName` było wyświetlane w każdym DropDownList `ListItem` z polem `CategoryID` używanym jako wartość.

[![wyświetlić pola CategoryName i IDKategorii użyte jako wartość](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Ilustracja 8**. wyświetlanie pola `CategoryName` i `CategoryID` używany jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image24.png))

Po wprowadzeniu tych zmian znaczniki deklaratywne dla `EditItemTemplate` w `CategoryName` TemplateField będą zawierać zarówno element DropDownList, jak i element ObjectDataSource:

[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> DropDownList w `EditItemTemplate` musi mieć włączony stan widoku. Wkrótce dodamy składnię wiązania danych do DropDownListej składni i poleceń DataBinding, takich jak `Eval()` i `Bind()` mogą występować tylko w kontrolkach, których stan widoku jest włączony.

Powtórz te kroki, aby dodać DropDownList o nazwie `Suppliers` do `EditItemTemplate``SupplierName` TemplateField. Obejmuje to dodawanie DropDownList do `EditItemTemplate` i Tworzenie innego elementu ObjectDataSource. Element ObjectDataSource `Suppliers` DropDownList należy jednak skonfigurować w taki sposób, aby wywoływał metodę `GetSuppliers()` klasy `SuppliersBLL`. Ponadto należy skonfigurować `Suppliers` DropDownList do wyświetlania pola `CompanyName` i użyć pola `SupplierID` jako wartości dla `ListItem` s.

Po dodaniu kontrolek DropDownList do dwóch `EditItemTemplate` s Załaduj stronę w przeglądarce i kliknij przycisk Edytuj dla produktu Cajun Chef Anton. Jak pokazano na rysunku 9, Kategoria produktu i kolumny dostawcy są renderowane jako listy rozwijane zawierające dostępne kategorie i dostawców do wyboru. Należy jednak pamiętać, że *pierwsze* elementy w obu listach rozwijanych są domyślnie zaznaczone (napoje dla kategorii i egzotycznych płynów jako dostawca), nawet jeśli sezon Cajun Chef Anton jest przyprawą oferowaną przez nowe Orleans Cajun.

[![pierwszy element na liście rozwijanej jest domyślnie zaznaczony](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Ilustracja 9**. pierwszy element na liście rozwijanej jest domyślnie zaznaczony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image27.png))

Ponadto, jeśli klikniesz przycisk Aktualizuj, zobaczysz, że wartości `CategoryID` i `SupplierID` produktu są ustawione na `NULL`. Oba te niepożądane zachowania są spowodowane tym, że kontrolek DropDownList w `EditItemTemplate` s nie są powiązane z żadnymi polami danych z bazowego produktu.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Powiązywanie kontrolek DropDownList z polami danych`CategoryID`i`SupplierID`

W celu uzyskania listy rozwijanej Kategoria i dostawca edytowanego produktu Ustaw odpowiednie wartości i aby te wartości były wysyłane z powrotem do metody `UpdateProduct` LOGIKI biznesowej po kliknięciu przycisku Aktualizuj, musimy powiązać właściwości `SelectedValue` "kontrolek DropDownList" z `CategoryID` i `SupplierID` pól danych przy użyciu wiązania dwukierunkowego. Aby to osiągnąć za pomocą `Categories` DropDownList, można dodać `SelectedValue='<%# Bind("CategoryID") %>'` bezpośrednio do składni deklaratywnej.

Alternatywnie można ustawić powiązania danych DropDownList, edytując szablon za pośrednictwem projektanta, a następnie klikając łącze Edytuj powiązania danych z taga inteligentnego DropDownList. Następnie wskaż, że właściwość `SelectedValue` powinna być powiązana z polem `CategoryID` za pomocą wiązania dwukierunkowego (patrz rysunek 10). Powtórz proces deklaratywny lub projektanta, aby powiązać pole danych `SupplierID` z `Suppliers` DropDownList.

[![powiązać IDKategorii z właściwością SelectedValue DropDownList za pomocą wiązania dwukierunkowego](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Ilustracja 10**. powiąż `CategoryID` z właściwością `SelectedValue` DropDownList, korzystając z dwukierunkowego wiązania z danymi ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image30.png))

Po zastosowaniu powiązań do właściwości `SelectedValue` dwóch kontrolek DropDownList, kategorie edytowanego produktu i kolumny dostawcy będą domyślnie dotyczyły wartości bieżącego produktu. Po kliknięciu przycisku Aktualizuj wartości `CategoryID` i `SupplierID` wybranego elementu listy rozwijanej zostaną przesłane do metody `UpdateProduct`. Rysunek 11 przedstawia samouczek po dodaniu instrukcji wiązania z danymi; Zwróć uwagę na to, jak wybrane elementy listy rozwijanej dla Chef Antona Cajun są prawidłowo przyłączane i nowe Orleans Cajun.

[![bieżącej kategorii edytowanego produktu i wartości dostawcy są domyślnie zaznaczone](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Ilustracja 11**: bieżąca Kategoria i wartość dostawcy edytowanego produktu są domyślnie zaznaczone ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image33.png))

## <a name="handlingnullvalues"></a>Obsługa wartości`NULL`

`CategoryID` i `SupplierID` kolumny w tabeli `Products` mogą być `NULL`, ale kontrolek DropDownList w `EditItemTemplate` s nie zawierają elementu listy, aby reprezentować wartość `NULL`. Ma to dwa konsekwencje:

- Użytkownik nie może użyć naszego interfejsu, aby zmienić kategorię produktu lub dostawcę z wartości innej niż`NULL` na `NULL` jeden
- Jeśli produkt ma `CategoryID` `NULL` lub `SupplierID`, kliknięcie przycisku Edytuj spowoduje powstanie wyjątku. Wynika to z faktu, że wartość `NULL` zwrócona przez `CategoryID` (lub `SupplierID`) w instrukcji `Bind()` nie jest mapowana na wartość w DropDownList (DropDownList zgłasza wyjątek, gdy jej Właściwość `SelectedValue` jest ustawiona na wartość *nie* w kolekcji elementów listy).

Aby można było obsłużyć `NULL` `CategoryID` i `SupplierID` wartości, musimy dodać kolejną `ListItem` do każdego DropDownListu, aby reprezentować wartość `NULL`. W przypadku [filtrowania wzorzec/szczegóły przy użyciu](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczka DropDownList przedstawiono sposób dodawania dodatkowych `ListItem` do DropDownList powiązanej z danymi, które dotyczą ustawiania właściwości `AppendDataBoundItems` DropDownList w celu `true` i ręcznego dodawania dodatkowych `ListItem`. W tym poprzednim samouczku dodaliśmy `ListItem` z `Value` `-1`. Logika wiązania danych w ASP.NET, jednak automatycznie konwertuje pusty ciąg na wartość `NULL` i vice versa. W związku z tym w tym samouczku `ListItem``Value` być pustym ciągiem.

Zacznij od ustawienia właściwości `AppendDataBoundItems` kontrolek DropDownList "na `true`. Następnie Dodaj `NULL` `ListItem` przez dodanie następującego elementu `<asp:ListItem>` do każdego DropDownListu, aby znaczniki deklaratywne wyglądały następująco:

[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

Jako wartość tekstową dla tego `ListItem`wybrano opcję "(None)", ale można ją zmienić tak, aby była ciągiem pustym.

> [!NOTE]
> Jak widać w przypadku *filtrowania wzorzec/szczegóły przy użyciu* samouczka DropDownList, `ListItem` s można dodać do DropDownList za pomocą projektanta, klikając Właściwość `Items` DropDownList w okno właściwości (co spowoduje wyświetlenie edytora kolekcji `ListItem`). Należy jednak pamiętać o dodaniu `ListItem` `NULL` w tym samouczku za pomocą składni deklaracyjnej. Jeśli używasz edytora kolekcji `ListItem`, wygenerowana składnia deklaracyjne spowoduje całkowite pominięcie ustawienia `Value` przy przypisaniu pustego ciągu, tworząc znaczniki deklaratywne, takie jak: `<asp:ListItem>(None)</asp:ListItem>`. Chociaż może to wyglądać nieszkodliwie, brakująca wartość powoduje, że DropDownList używa wartości właściwości `Text` w tym miejscu. Oznacza to, że w przypadku wybrania `NULL` `ListItem` zostanie podjęta próba przypisania wartości "(brak)" do `CategoryID`, co spowoduje wyjątek. Po ustawieniu jawnie `Value=""`wartość `NULL` zostanie przypisana do `CategoryID`, gdy zostanie wybrany `NULL` `ListItem`.

Powtórz te kroki dla dostawców DropDownList.

Dzięki tej dodatkowej `ListItem`interfejs edytowania może teraz przypisywać wartości `NULL` do `CategoryID` produktu i pól `SupplierID`, jak pokazano na rysunku 12.

[![wybierz (brak), aby przypisać wartość NULL dla kategorii lub dostawcy produktu](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Rysunek 12**. Wybierz opcję (brak), aby przypisać wartość `NULL` dla kategorii lub dostawcy produktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image36.png))

## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Krok 4. Używanie elementów RadioButton dla stanu wycofanego

Obecnie pole danych `Discontinued` produkty jest wyrażane przy użyciu CheckBoxField, które renderuje wyłączone pole wyboru dla wierszy tylko do odczytu i pole wyboru Enabled dla edytowanego wiersza. Chociaż ten interfejs użytkownika jest często odpowiedni, można go dostosować w razie konieczności przy użyciu TemplateField. Na potrzeby tego samouczka zmienimy CheckBoxField na TemplateField, który używa kontrolki RadioButtonList z dwoma opcjami "Active" i "Unchanged", z których użytkownik może określić wartość `Discontinued` produktu.

Zacznij od przekonwertowania `Discontinued` CheckBoxField na TemplateField, co spowoduje utworzenie TemplateField z `ItemTemplate` i `EditItemTemplate`. Oba szablony zawierają pole wyboru z właściwością `Checked` powiązaną z polem danych `Discontinued`, jedyną różnicą między tymi, że właściwość `Enabled` pola wyboru `ItemTemplate`jest ustawiona na `false`.

Zastąp pole wyboru zarówno `ItemTemplate`, jak i `EditItemTemplate` za pomocą kontrolki RadioButtonList, ustawiając właściwości RadioButtonLists "`ID` na `DiscontinuedChoice`. Następnie wskaż, że każdy element RadioButtonLists powinien zawierać dwa przyciski radiowe, jeden z etykietą "Active" z wartością "false" i jedną etykietą "nieobsługiwane" z wartością "true". Aby to osiągnąć, możesz wprowadzić `<asp:ListItem>` elementów bezpośrednio poprzez składnię deklaratywną lub użyć edytora kolekcji `ListItem` z projektanta. Rysunek 13 przedstawia Edytor kolekcji `ListItem` po określeniu dwóch opcji przycisków radiowych.

[![Dodaj](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Ilustracja 13**. Dodawanie opcji "Active" i "uncontinued" do RadioButtonList ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image39.png))

Ponieważ RadioButtonList w `ItemTemplate` nie należy edytować, ustaw jej Właściwość `Enabled` na `false`, pozostawiając Właściwość `Enabled` na `true` (wartość domyślna) dla RadioButtonList w `EditItemTemplate`. Spowoduje to, że przyciski radiowe w nieedytowanym wierszu są tylko do odczytu, ale zezwoli użytkownikowi na zmianę wartości RadioButton dla edytowanego wiersza.

Nadal musimy przypisać właściwości `SelectedValue` formantów RadioButtonList, aby wybrać odpowiedni przycisk radiowy na podstawie pola dane `Discontinued` produktu. Podobnie jak w przypadku kontrolek dropdownlistego we wcześniejszej części tego samouczka, tę składnię wiązania danych można dodać bezpośrednio do znaczników deklaratywnych lub za pomocą linku Edytuj dane powiązania w tagach inteligentnych RadioButtonLists.

Po dodaniu dwóch RadioButtonLists i ich skonfigurowaniu znaczniki deklaratywne `Discontinued` TemplateField powinny wyglądać następująco:

[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Po wprowadzeniu tych zmian kolumna `Discontinued` została przekształcona z listy pól wyboru na listę par przycisków radiowych (zobacz rysunek 14). Podczas edytowania produktu jest wybierany odpowiedni przycisk radiowy, a stan wycofanego produktu można zaktualizować, wybierając inny przycisk radiowy i klikając polecenie Aktualizuj.

[![pola wyboru zamienione zostały zastąpione przez pary przycisków radiowych](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Ilustracja 14**. pola wyboru wycofane zostały zastąpione przez pary przycisków radiowych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-data-modification-interface-cs/_static/image42.png))

> [!NOTE]
> Ponieważ kolumna `Discontinued` w bazie danych `Products` nie może mieć wartości `NULL`, nie trzeba martwić się o przechwytywanie `NULL` informacji w interfejsie. Jeśli jednak kolumna `Discontinued` może zawierać `NULL` wartości, chcemy dodać trzeci przycisk radiowy do listy z `Value` ustawionym na pusty ciąg (`Value=""`), podobnie jak w przypadku kategorii i dostawcy kontrolek DropDownList.

## <a name="summary"></a>Podsumowanie

Mimo że BoundField i CheckBoxField automatycznie renderują interfejsy tylko do odczytu, edycji i wstawiania, nie mają możliwości dostosowywania. Często jednak należy dostosować edytowanie lub wstawianie interfejsu, może dodać kontrolki weryfikacji (jak zostało to opisane w poprzednim samouczku) lub przez dostosowanie interfejsu użytkownika zbierania danych (jak zostało to opisane w tym samouczku). Dostosowanie interfejsu za pomocą TemplateField można zsumować w następujących krokach:

1. Dodaj TemplateField lub przekonwertuj istniejące BoundField lub CheckBoxField na TemplateField
2. Rozszerzanie interfejsu zgodnie z wymaganiami
3. Powiąż odpowiednie pola danych z nowo dodanymi kontrolkami sieci Web za pomocą wiązania dwukierunkowego

Oprócz wbudowanych kontrolek sieci Web ASP.NET można także dostosować szablony TemplateField z niestandardowymi, skompilowanymi kontrolkami serwera i kontrolkami użytkownika.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [dalej](implementing-optimistic-concurrency-cs.md)
