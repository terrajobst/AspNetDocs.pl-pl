---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: Wstawianie nowego rekordu z stopki GridView (C#) | Microsoft Docs
author: rick-anderson
description: Chociaż formant GridView nie udostępnia wbudowanej obsługi wstawiania nowego rekordu danych, w tym samouczku pokazano, jak rozszerzyć widok GridView w celu uwzględnienia...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 6b337fe395a0ed54b03767111e73ea6d6c0ba23a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74591920"
---
# <a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>Wstawianie nowego rekordu w stopce kontrolki GridView (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) lub [Pobierz plik PDF](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> Chociaż formant GridView nie udostępnia wbudowanej obsługi wstawiania nowego rekordu danych, w tym samouczku pokazano, jak rozszerzyć widok GridView w celu uwzględnienia interfejsu wstawiania.

## <a name="introduction"></a>Wprowadzenie

Zgodnie z opisem w temacie [Omówienie wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , formanty w sieci Web GridView, DetailsView i FormView obejmują wbudowane funkcje modyfikacji danych. W przypadku korzystania z formantów deklaracyjnego źródła danych te trzy kontrolki sieci Web można szybko i łatwo skonfigurować do modyfikowania danych i w scenariuszach, bez konieczności pisania jednego wiersza kodu. Niestety tylko formanty DetailsView i FormView zapewniają wbudowane funkcje wstawiania, edytowania i usuwania. Widok GridView oferuje tylko możliwość edytowania i usuwania. Jednak w przypadku małej ilości tłuszczu kątowego można rozszerzyć widok GridView, aby uwzględnić interfejs wstawiania.

W przypadku dodawania funkcji wstawiania do widoku GridView są odpowiedzialni za decydowanie o sposobie dodawania nowych rekordów, tworzeniu interfejsu wstawiania i pisaniu kodu w celu wstawienia nowego rekordu. W tym samouczku dowiesz się, jak dodać interfejs wstawiania do wiersza stopki "GridView s" (patrz rysunek 1). Komórka Stopka dla każdej kolumny zawiera odpowiedni element interfejsu użytkownika zbierania danych (pole tekstowe dla nazwy produktu, DropDownList dla dostawcy itd.). Potrzebujemy również kolumny dla przycisku Dodaj, który po kliknięciu spowoduje, że będzie to powodowało odświeżenie i wstawienie nowego rekordu do tabeli `Products` przy użyciu wartości podanych w wierszu stopka.

[![wiersz stopki zapewnia interfejs do dodawania nowych produktów](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Rysunek 1**: wiersz stopki zawiera interfejs do dodawania nowych produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))

## <a name="step-1-displaying-product-information-in-a-gridview"></a>Krok 1. Wyświetlanie informacji o produkcie w widoku GridView

Przed zawypróbujemym się z tworzeniem interfejsu wstawiania w środkowej stopce s, pozwól, aby najpierw dodać widok GridView do strony, która zawiera listę produktów w bazie danych. Zacznij od otworzenia strony `InsertThroughFooter.aspx` w folderze `EnhancedGridView` i przeciągnij widok GridView z przybornika do projektanta, ustawiając właściwość GridView s `ID` na `Products`. Następnie użyj taga inteligentnego "GridView s", aby powiązać go z nowym elementem ObjectDataSource o nazwie `ProductsDataSource`.

[![utworzyć nowego elementu ObjectDataSource o nazwie ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Rysunek 2**. Utwórz nowy element ObjectDataSource o nazwie `ProductsDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))

Skonfiguruj element ObjectDataSource do używania metody `GetProducts()` `ProductsBLL` Class, aby pobrać informacje o produkcie. Na potrzeby tego samouczka Postaraj się, aby nie dopuścić do dodawania funkcji wstawiania i usuwania. W związku z tym upewnij się, że na liście rozwijanej na karcie Wstawianie jest ustawiona wartość `AddProduct()` i że listy rozwijane na kartach Aktualizuj i Usuń są ustawione na wartość (brak).

[![zmapować metodę addproduct na metodę ObjectDataSource s Insert ()](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Rysunek 3**. mapowanie metody `AddProduct` na metodę `Insert()` ObjectDataSource s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))

[![ustawić listy rozwijane aktualizacji i usuwania na (brak)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Rysunek 4**. Ustawianie list rozwijanych kart Aktualizuj i Usuń na (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))

Po zakończeniu działania Kreatora elementu ObjectDataSource s Configure Data source program Visual Studio automatycznie doda pola do widoku GridView dla każdego z odpowiednich pól danych. Na razie pozostaw wszystkie pola dodane przez program Visual Studio. W dalszej części tego samouczka będziemy wrócić i usunąć niektóre z pól, których wartości nie należy określać podczas dodawania nowego rekordu.

Ze względu na to, że w bazie danych znajdują się blisko 80 produktów, użytkownik będzie musiał przewijać w dół do dołu strony sieci Web, aby dodać nowy rekord. W związku z tym Zezwól na stronicowanie, aby zwiększyć czytelność i dostępność interfejsu wstawiania. Aby włączyć stronicowanie, po prostu zaznacz pole wyboru Włącz stronicowanie ze znacznika inteligentnego GridView.

W tym momencie znaczniki deklaratywne GridView i ObjectDataSource s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]

[![wszystkie pola danych produktu są wyświetlane w widoku strony GridView](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Rysunek 5**. wszystkie pola danych produktu są wyświetlane w widoku strony GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))

## <a name="step-2-adding-a-footer-row"></a>Krok 2. Dodawanie wiersza stopki

Wraz z wierszami nagłówka i danych widok GridView zawiera wiersz stopki. Wiersze nagłówka i stopki są wyświetlane w zależności od wartości właściwości [`ShowHeader`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showheader.aspx) GridView i [`ShowFooter`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showfooter.aspx) . Aby wyświetlić wiersz stopki, po prostu ustaw właściwość `ShowFooter` na `true`. Jak pokazano na rysunku 6, ustawienie właściwości `ShowFooter` na `true` powoduje dodanie wiersza stopki do siatki.

[![wyświetlić wiersza stopki, ustaw ShowFooter na wartość true](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Ilustracja 6**. Aby wyświetlić wiersz stopki, ustaw `ShowFooter` na `True` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))

Zwróć uwagę, że wiersz stopki ma ciemnoczerwony kolor tła. Jest to spowodowane tym, że motyw DataWebControls został utworzony i zastosowany do wszystkich stron z powrotem z samouczka dotyczącego elementu [ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) . W odróżnieniu od `GridView.skin` plik konfiguruje Właściwość `FooterStyle`, która używa klasy CSS `FooterStyle`. Klasa `FooterStyle` jest definiowana w `Styles.css` w następujący sposób:

[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> W poprzednich samouczkach zapoznajemy się z wierszem Stopka "GridView s". W razie potrzeby zapoznaj się z powrotem z [informacjami podsumowującymi wyświetlanymi w samouczku widoku GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) dla odświeżacza.

Po ustawieniu właściwości `ShowFooter` na `true`, poświęć chwilę na wyświetlenie danych wyjściowych w przeglądarce. Obecnie wiersz stopki nie zawiera żadnych kontrolek tekstowych ani sieci Web. W kroku 3 zmodyfikujemy stopkę dla każdego pola GridView, tak aby zawierała odpowiedni interfejs wstawiania.

[![pusty wiersz stopki jest wyświetlany powyżej kontrolek interfejsu stronicowania](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Rysunek 7**: pusty wiersz stopki jest wyświetlany powyżej kontrolek interfejsu stronicowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))

## <a name="step-3-customizing-the-footer-row"></a>Krok 3. Dostosowywanie wiersza stopki

W samouczku w [formancie GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) pokazano, jak znacznie dostosować wyświetlanie konkretnej kolumny GridView przy użyciu używanie TemplateField (w przeciwieństwie do BoundFields lub CheckBoxFields). w obszarze [Dostosowywanie interfejsu modyfikacji danych,](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) który oglądamy przy użyciu używanie TemplateField, można dostosować interfejs edycji w widoku GridView. Odwołaj, że TemplateField składa się z szeregu szablonów, które definiują kombinację znaczników, kontrolek sieci Web i składni wiązania danych używanych w przypadku niektórych typów wierszy. `ItemTemplate`, na przykład Określa szablon używany do wierszy tylko do odczytu, podczas gdy `EditItemTemplate` definiuje szablon dla wiersza edytowalnego.

Wraz z `ItemTemplate` i `EditItemTemplate`, TemplateField również zawiera `FooterTemplate`, który określa zawartość dla wiersza stopki. W związku z tym możemy dodać kontrolki sieci Web, które są konieczne dla każdego pola, wstawiając interfejs do `FooterTemplate`. Aby rozpocząć, przekonwertuj wszystkie pola w widoku GridView na używanie TemplateField. Można to zrobić, klikając łącze Edytuj kolumny w tagu inteligentnym GridView, zaznaczając każde pole w lewym dolnym rogu, a następnie klikając polecenie Konwertuj to pole na link TemplateField.

![Konwertuj każde pole na TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Ilustracja 8**. konwertowanie każdego pola na TemplateField

Kliknięcie pola Konwertuj to pole na TemplateField powoduje zamianę bieżącego typu pola na odpowiednik TemplateField. Na przykład każdy BoundField jest zastępowany przez TemplateField z `ItemTemplate`, który zawiera etykietę, która wyświetla odpowiednie pole danych i `EditItemTemplate`, który wyświetla pole dane w polu tekstowym. `ProductName` BoundField zostały przekonwertowane na następujące znaczniki TemplateField:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

Podobnie `Discontinued` CheckBoxField został przekonwertowany do TemplateField, którego `ItemTemplate` i `EditItemTemplate` zawierają kontrolkę sieci Web CheckBox (z wyłącznym elementem CheckBox `ItemTemplate`). BoundField `ProductID` tylko do odczytu zostały przekonwertowane na TemplateField z kontrolką etykieta w `ItemTemplate` i `EditItemTemplate`. W skrócie, przekonwertowanie istniejącego pola GridView na TemplateField jest szybkim i łatwym sposobem przełączenia na bardziej dostosowywalny TemplateField bez utraty żadnej z istniejących funkcji pól s.

Ze względu na to, że widok GridView przeprowadził obsługę edycji, możesz bezpłatnie usunąć `EditItemTemplate` z każdego TemplateFieldu, pozostawiając tylko `ItemTemplate`. Po wykonaniu tej czynności znacznik deklaratywny GridView powinien wyglądać następująco:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Teraz, gdy każde pole GridView zostało przekonwertowane na TemplateField, możemy wprowadzić odpowiedni interfejs wstawiania do każdego pola `FooterTemplate`. Niektóre pola nie będą miały interfejsu wstawiania (`ProductID`, na przykład); inne zależą od kontrolek sieci Web używanych do zbierania nowych informacji o produkcie.

Aby utworzyć interfejs edytowania, wybierz link Edytuj szablony z tagu inteligentnego GridView. Następnie wybierz z listy rozwijanej odpowiednie pole s `FooterTemplate` i przeciągnij odpowiednią kontrolkę z przybornika do projektanta.

[![dodać odpowiedniego interfejsu wstawiania do każdego pola s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Ilustracja 9**. Dodawanie odpowiedniego interfejsu wstawianego do każdego pola s `FooterTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))

Poniższa lista punktowana wylicza pola GridView, określając interfejs wstawiania do dodania:

- `ProductID` brak.
- `ProductName` dodać pola tekstowego i ustawić jego `ID` na `NewProductName`. Dodaj kontrolkę RequiredFieldValidator, aby upewnić się, że użytkownik wprowadzi wartość nowej nazwy produktu.
- `SupplierID` brak.
- `CategoryID` brak.
- `QuantityPerUnit` dodać pola tekstowego, ustawiając jego `ID` na `NewQuantityPerUnit`.
- `UnitPrice` dodać pola tekstowego o nazwie `NewUnitPrice` i CompareValidator, które zapewniają, że wprowadzona wartość jest wartością waluty większą lub równą zero.
- `UnitsInStock` użyć pola tekstowego, którego `ID` jest ustawiona na `NewUnitsInStock`. Uwzględnij element CompareValidator, który zapewnia, że wprowadzona wartość jest wartością całkowitą większą lub równą zero.
- `UnitsOnOrder` użyć pola tekstowego, którego `ID` jest ustawiona na `NewUnitsOnOrder`. Uwzględnij element CompareValidator, który zapewnia, że wprowadzona wartość jest wartością całkowitą większą lub równą zero.
- `ReorderLevel` użyć pola tekstowego, którego `ID` jest ustawiona na `NewReorderLevel`. Uwzględnij element CompareValidator, który zapewnia, że wprowadzona wartość jest wartością całkowitą większą lub równą zero.
- `Discontinued` dodać pola wyboru, ustawiając jego `ID` na `NewDiscontinued`.
- `CategoryName` dodać DropDownList i ustawić `ID` na `NewCategoryID`. Powiąż go z nowym elementem ObjectDataSource o nazwie `CategoriesDataSource` i skonfiguruj go tak, aby korzystał z metody `GetCategories()` klasy `CategoriesBLL`. DropDownList s `ListItem` s wyświetla pola dane `CategoryName`, używając pola dane `CategoryID` jako ich wartości.
- `SupplierName` dodać DropDownList i ustawić `ID` na `NewSupplierID`. Powiąż go z nowym elementem ObjectDataSource o nazwie `SuppliersDataSource` i skonfiguruj go tak, aby korzystał z metody `GetSuppliers()` klasy `SuppliersBLL`. DropDownList s `ListItem` s wyświetla pola dane `CompanyName`, używając pola dane `SupplierID` jako ich wartości.

Dla każdej kontrolki walidacji Wyczyść Właściwość `ForeColor`, tak aby w przypadku koloru czerwony kolor pierwszego planu `FooterStyle`, w której zostanie użyta wartość "biały pierwszy plan". Należy również użyć właściwości `ErrorMessage`, aby uzyskać szczegółowy opis, ale ustawić właściwość `Text` na gwiazdkę. Aby zapobiec, że tekst kontrolki walidacji nie powoduje zawijania interfejsu do dwóch wierszy, ustaw właściwość `FooterStyle` s `Wrap` na wartość false dla każdej `FooterTemplate` s, która używa kontrolki walidacji. Na koniec Dodaj kontrolkę podsumowania walidacji poniżej widoku GridView i ustaw jej Właściwość `ShowMessageBox` na `true` i jej Właściwość `ShowSummary` na `false`.

W przypadku dodawania nowego produktu musimy podać `CategoryID` i `SupplierID`. Te informacje są przechwytywane przez kontrolek DropDownList w komórkach stopki dla pól `CategoryName` i `SupplierName`. Wybrano używanie tych pól w przeciwieństwie do `CategoryID` i `SupplierID` używanie TemplateField, ponieważ w wierszach danych siatki, użytkownik jest prawdopodobnie bardziej interesujący do wyświetlenia kategorii i nazw dostawców, a nie ich wartości identyfikatorów. Ponieważ wartości `CategoryID` i `SupplierID` są teraz przechwytywane w `CategoryName` i `SupplierName` pole s Wstawianie interfejsów, możemy usunąć `CategoryID` i `SupplierID` używanie TemplateField z widoku GridView.

Podobnie `ProductID` nie jest używany podczas dodawania nowego produktu, więc można także usunąć `ProductID` TemplateField. Należy jednak pozostawić pole `ProductID` w siatce. Oprócz pól textdialogs, kontrolek DropDownList, CheckBoxs i Validation Controls, które tworzą interfejs wstawiania, będzie również potrzebny przycisk Dodaj, który po kliknięciu wykonuje logikę, aby dodać nowy produkt do bazy danych programu. W kroku 4 dołączymy przycisk Dodaj w interfejsie wstawiania w `ProductID` TemplateField s `FooterTemplate`.

Korzystaj bezpłatnie, aby poprawić wygląd różnych pól GridView. Na przykład możesz chcieć sformatować `UnitPrice` wartości jako walutowe, wyrównać pola `UnitsInStock`, `UnitsOnOrder`i `ReorderLevel` i zaktualizować wartości `HeaderText` dla używanie TemplateField.

Po utworzeniu obrotu po wstawieniu interfejsów w `FooterTemplate` s, usunięciu `SupplierID`i `CategoryID` używanie TemplateField i poprawieniu wyglądu siatki przy użyciu formatowania i wyrównywania używanie TemplateField, znaczniki deklaratywne GridView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

W przypadku wyświetlania za pomocą przeglądarki wiersz stopki GridView jest teraz uwzględniał ukończony interfejs wstawiania (patrz rysunek 10). W tym momencie interfejs Wstawiający nie obejmuje środka dla użytkownika wskazującego, że dane zostały wprowadzone do nowego produktu i chcą wstawić nowy rekord do bazy danych programu. Ponadto jeszcze tu dowiesz się, w jaki sposób dane wprowadzone do stopki przekładają się na nowy rekord w bazie danych `Products`. W kroku 4 dowiesz się, jak dodać przycisk Dodaj do interfejsu wstawiania oraz jak wykonać kod na stronie ogłaszania zwrotnego po kliknięciu. Krok 5 pokazuje, jak wstawić nowy rekord przy użyciu danych z stopki.

[![Stopka GridView udostępnia interfejs do dodawania nowego rekordu](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Ilustracja 10**: Stopka GridView zawiera interfejs do dodawania nowego rekordu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))

## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Krok 4. dołączenie przycisku dodawania w interfejsie wstawiania

Musimy dodać przycisk Dodaj w dowolnym miejscu w interfejsie wstawiania, ponieważ wiersz stopki s Wstawiający interfejs aktualnie nie ma środków, aby użytkownik mógł wskazać, że zakończył wprowadzanie nowych informacji o produkcie. Ten element może być umieszczony w jednym z istniejących `FooterTemplate` s lub w tym celu można dodać do siatki nową kolumnę. Na potrzeby tego samouczka należy umieścić przycisk Dodaj w `ProductID` TemplateField s `FooterTemplate`.

W projektancie kliknij link Edytuj szablony w tagu inteligentnym GridView s, a następnie wybierz `FooterTemplate` `ProductID` pól z listy rozwijanej. Dodaj kontrolkę sieci Web przycisku (lub element LinkButton lub kliknięto element ImageButton, jeśli wolisz) do szablonu, ustawiając jej identyfikator na `AddProduct`, jej `CommandName` do wstawienia i Właściwość `Text` do dodania, jak pokazano na rysunku 11.

[![umieścić przycisk Dodaj w FooterTemplate ProductID TemplateField s](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Ilustracja 11**. Umieść przycisk dodaj w `ProductID` TemplateField s `FooterTemplate` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))

Po dołączeniu przycisku Dodaj należy przetestować stronę w przeglądarce. Należy pamiętać, że po kliknięciu przycisku Dodaj z nieprawidłowymi danymi w interfejsie wstawiania, ogłaszanie zwrotne jest krótkie, a kontrolka podsumowania walidacji wskazuje nieprawidłowe dane (Zobacz Rysunek 12). Po wprowadzeniu odpowiednich danych kliknięcie przycisku Dodaj powoduje odświeżenie. Nie dodano żadnego rekordu do bazy danych. Musimy napisać bit kodu, aby faktycznie wykonać operację wstawiania.

[![przycisk Dodaj ogłaszania zwrotnego jest krótki obwód w przypadku nieprawidłowych danych w interfejsie wstawiania](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Rysunek 12**: przycisk dodawania ogłaszania zwrotnego przycisku s jest krótki, jeśli w interfejsie wstawiania są nieprawidłowe dane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))

> [!NOTE]
> Kontrolki walidacji w interfejsie wstawiania nie zostały przypisane do grupy walidacji. Działa to tak długo, jak interfejs wstawiania jest jedynym zestawem formantów walidacji na stronie. Jeśli jednak na stronie znajdują się inne kontrolki weryfikacji (takie jak kontrolki walidacji w interfejsie edycji siatki), kontrolki walidacji w oknie dialogowym Wstawianie interfejsu i Dodawanie `ValidationGroup` powinny mieć przypisaną taką samą wartość, aby skojarzyć te formanty z określoną grupą walidacji. Aby uzyskać więcej informacji na temat partycjonowania formantów walidacji i przycisków na stronie w grupach sprawdzania poprawności, zobacz artykuł dotyczący [przetrwania kontroli weryfikacyjnej w programie ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) .

## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Krok 5. Wstawianie nowego rekordu do tabeli`Products`

Podczas używania wbudowanych funkcji edycji widoku GridView widok GridView automatycznie obsługuje wszystkie czynności niezbędne do wykonania aktualizacji. W szczególności po kliknięciu przycisku Aktualizuj kopiowane wartości wprowadzone z interfejsu edycji do parametrów w kolekcji ObjectDataSource s `UpdateParameters` i rozpoczynają aktualizację przez wywoływanie metody `Update()` ObjectDataSource s. Ponieważ GridView nie zapewnia takiej wbudowanej funkcji wstawiania, musimy zaimplementować kod, który wywołuje metodę ObjectDataSource s `Insert()` i kopiuje wartości z interfejsu wstawiania do kolekcji ObjectDataSource s `InsertParameters`.

Tę logikę wstawiania należy wykonać po kliknięciu przycisku Dodaj. Zgodnie z opisem w sekcji [Dodawanie i reagowanie na przyciski w](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) samouczku GridView, gdy kliknięto przycisk, element LinkButton lub kliknięto element ImageButton w widoku GridView, zdarzenie GridView `RowCommand` jest wyzwalane w przypadku ogłaszania zwrotnego. To zdarzenie jest wyzwalane, czy przycisk, element LinkButton lub kliknięto element ImageButton został dodany jawnie, jak przycisk Dodaj w wierszu stopki lub czy został automatycznie dodany przez GridView (na przykład LinkButtons w górnej części każdej kolumny, gdy jest zaznaczone polecenie Włącz sortowanie) lub LinkButtons w interfejsie stronicowania w przypadku wybrania włączania stronicowania).

W związku z tym, aby odpowiedzieć użytkownikowi na kliknięcie przycisku Dodaj, musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `RowCommand` GridView. Ponieważ to zdarzenie jest wyzwalane za każdym razem, gdy zostanie kliknięty *dowolny* przycisk, element LinkButton lub kliknięto element ImageButton w widoku GridView, bardzo ważne jest, aby kontynuować operację wstawiania logiki, jeśli właściwość `CommandName` przeniesiona do programu obsługi zdarzeń jest mapowana na wartość `CommandName` przycisku Dodaj (INSERT). Ponadto należy również wykonać operację tylko wtedy, gdy kontrolki walidacji raportują prawidłowe dane. Aby to umożliwić, Utwórz procedurę obsługi zdarzeń dla zdarzenia `RowCommand` przy użyciu następującego kodu:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Możesz się zastanawiać, dlaczego obydwie procedury obsługi zdarzeń sprawdzają Właściwość `Page.IsValid`. Czy po zakończeniu nie będzie można pominąć ogłaszania zwrotnego, jeśli w interfejsie wstawiania są podane nieprawidłowe dane? To założenie jest poprawne, o ile użytkownik nie wyłączył kodu JavaScript lub podjął kroki w celu obejścia logiki walidacji po stronie klienta. Krótko mówiąc, nigdy nie należy polegać wyłącznie na weryfikacji po stronie klienta; Sprawdzanie po stronie serwera pod kątem poprawności powinno być zawsze wykonywane przed rozpoczęciem pracy z danymi.

W kroku 1 utworzyliśmy `ProductsDataSource` element ObjectDataSource, tak że jego Metoda `Insert()` została zamapowana na metodę `AddProduct` klasy `ProductsBLL`. Aby wstawić nowy rekord do tabeli `Products`, można po prostu wywołać metodę `Insert()` ObjectDataSource s:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Teraz, gdy metoda `Insert()` została wywołana, wszystko, co pozostało, polega na skopiowaniu wartości z interfejsu wstawiania do parametrów przesyłanych do metody `ProductsBLL` klasy s `AddProduct`. Jak widać w samouczku [badanie zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) , można to zrobić za pomocą zdarzenia `Inserting` elementu ObjectDataSource s. W zdarzeniu `Inserting` musimy programowo odwoływać się do kontrolek z wierszem stopki `Products` GridView s i przypisać ich wartości do kolekcji `e.InputParameters`. Jeśli użytkownik pominie wartość, na przykład pozostawiając `ReorderLevel` pole tekstowe puste, musimy określić, że wartość wstawiona do bazy danych powinna być `NULL`. Ponieważ metoda `AddProducts` akceptuje Typy dopuszczające wartości null dla pól bazy danych dopuszczających wartości null, po prostu Użyj typu dopuszczającego wartość null i ustaw ją na `null` w przypadku pominięcia danych wejściowych użytkownika.

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

Po zakończeniu obsługi zdarzeń `Inserting`, nowe rekordy można dodać do tabeli bazy danych `Products` za pośrednictwem wiersza "Stopka" GridView. Spróbuj dodać kilka nowych produktów.

## <a name="enhancing-and-customizing-the-add-operation"></a>Ulepszanie i dostosowywanie operacji dodawania

Obecnie kliknięcie przycisku Dodaj powoduje dodanie nowego rekordu do tabeli bazy danych, ale nie zawiera żadnych informacji zwrotnych wizualizacji, że rekord został pomyślnie dodany. W idealnym przypadku etykieta kontrolka sieci Web lub alert po stronie klienta poinformuje użytkownika, że ich Wstawianie zakończyło się sukcesem. Jestem tym ćwiczeniem dla czytnika.

Widok GridView używany w tym samouczku nie stosuje żadnego porządku sortowania do wymienionych produktów ani nie zezwala użytkownikowi końcowemu na sortowanie danych. W związku z tym rekordy są uporządkowane w taki sposób, w jaki znajdują się w bazie danych według ich pola klucza podstawowego. Ponieważ każdy nowy rekord ma `ProductID` wartość większą od ostatniej, za każdym razem, gdy nowy produkt zostanie dodany, jest on na końcu siatki. W związku z tym możesz chcieć automatycznie wysłać użytkownika do ostatniej strony GridView po dodaniu nowego rekordu. Można to osiągnąć, dodając następujący wiersz kodu po wywołaniu do `ProductsDataSource.Insert()` w programie obsługi zdarzeń `RowCommand`, aby wskazać, że użytkownik musi zostać wysłany do ostatniej strony po powiązaniu danych w widoku GridView:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage` to zmienna logiczna na poziomie strony, która początkowo ma przypisaną wartość `false`. W programie obsługi zdarzeń `DataBound` GridView, jeśli `SendUserToLastPage` ma wartość false, właściwość `PageIndex` zostanie zaktualizowana w celu wysłania użytkownika do ostatniej strony.

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

Przyczyna ustawienia właściwości `PageIndex` w obsłudze zdarzeń `DataBound` (w przeciwieństwie do programu obsługi zdarzeń `RowCommand`) wynika z faktu, że program obsługi zdarzeń `RowCommand` zgłosił jeszcze nowy rekord do tabeli bazy danych `Products`. W związku z tym w `RowCommand` obsłudze zdarzeń ostatni indeks strony (`PageCount - 1`) reprezentuje ostatni indeks strony *przed* dodaniem nowego produktu. W przypadku większości produktów dodawanych ostatni indeks strony jest taki sam po dodaniu nowego produktu. Jednak gdy dodany produkt prowadzi do nowego indeksu ostatniej strony, w przypadku nieprawidłowej aktualizacji `PageIndex` w programie obsługi zdarzeń `RowCommand`, zostanie ona przełączona na sekundę do ostatniej strony (ostatni indeks strony przed dodaniem nowego produktu), w przeciwieństwie do nowego indeksu ostatniej strony. Ponieważ program obsługi zdarzeń `DataBound` uruchamiany po dodaniu nowego produktu, a dane są ponownie powiązane z siatką, ustawiając właściwość `PageIndex`, wiemy, że będziemy ponownie otrzymywać prawidłowy indeks ostatniej strony.

Na koniec widok GridView używany w tym samouczku jest dość szeroki ze względu na liczbę pól, które muszą być zbierane w celu dodania nowego produktu. Ze względu na tę szerokość może być preferowany pionowy układ widoku DetailsView. Całkowitą szerokość widoku GridView można zmniejszyć, zbierając mniej danych wejściowych. Być może nie musimy zbierać pól `UnitsOnOrder`, `UnitsInStock`i `ReorderLevel` podczas dodawania nowego produktu, w takim przypadku pola te można usunąć z widoku GridView.

Aby dostosować zebrane dane, można użyć jednego z dwóch metod:

- Kontynuuj używanie metody `AddProduct`, która oczekuje wartości dla pól `UnitsOnOrder`, `UnitsInStock`i `ReorderLevel`. W obsłudze zdarzeń `Inserting` Podaj stałe wartości domyślne, które mają być używane dla tych danych wejściowych, które zostały usunięte z interfejsu wstawiania.
- Utwórz nowe Przeciążenie metody `AddProduct` w klasie `ProductsBLL`, która nie akceptuje wejść dla pól `UnitsOnOrder`, `UnitsInStock`i `ReorderLevel`. Następnie na stronie ASP.NET Skonfiguruj element ObjectDataSource, aby używał tego nowego przeciążenia.

Każda opcja będzie działała równie dobrze. W poprzednich samouczkach używali tej ostatniej opcji, tworząc wiele przeciążeń dla metody `UpdateProduct` `ProductsBLL` Class.

## <a name="summary"></a>Podsumowanie

Widok GridView nie zawiera wbudowanych funkcji wstawiania dostępnych w widoku DetailsView i FormView, ale z bitem nakładu pracy można dodać interfejs do wiersza stopki. Aby wyświetlić wiersz stopki w GridView, po prostu ustaw jego właściwość `ShowFooter` na `true`. Zawartość wiersza stopki można dostosować do każdego pola, konwertując pole na TemplateField i dodając interfejs wstawiania do `FooterTemplate`. Jak pokazano w tym samouczku, `FooterTemplate` może zawierać przyciski, pola tekstowe, kontrolek DropDownList, pola wyboru, kontrolki źródła danych do wypełniania kontrolek sieci Web opartych na danych (takich jak kontrolek DropDownList) i kontrolek weryfikacji. Wraz z kontrolkami do zbierania danych wejściowych użytkownika jest wymagany przycisk Dodaj, element LinkButton lub kliknięto element ImageButton.

Po kliknięciu przycisku Dodaj zostanie wywołana metoda `Insert()` ObjectDataSource s, aby rozpocząć Wstawianie przepływu pracy. Element ObjectDataSource następnie wywoła skonfigurowaną metodę Insert (`ProductsBLL` klasy s `AddProduct` Metoda w tym samouczku). Przed wywołaniem metody INSERT należy skopiować wartości z interfejsu GridView ' Wstawianie do kolekcji `InsertParameters` ObjectDataSource s. Można to osiągnąć programowo, odwołując się do formantów sieci Web wstawiania interfejsu w programie obsługi zdarzeń `Inserting` ObjectDataSource s.

W tym samouczku przedstawiono metody umożliwiające zwiększenie wyglądu GridView. Następny zestaw samouczków sprawdzi, jak korzystać z danych binarnych, takich jak obrazy, pliki PDF, dokumenty programu Word itd. oraz formanty sieci Web danych.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Bernadette Leigh. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](adding-a-gridview-column-of-checkboxes-cs.md)
> [dalej](adding-a-gridview-column-of-radio-buttons-vb.md)
