---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: Używanie używanie TemplateField w formancie DetailsView (C#) | Microsoft Docs
author: rick-anderson
description: Te same możliwości używanie TemplateField dostępne w widoku GridView są również dostępne w kontrolce DetailsView. W tym samouczku zostanie wyświetlony jeden produkt...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 55c667800a9fb19d200bcf4b68e2c59ca4ef5d0e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78595663"
---
# <a name="using-templatefields-in-the-detailsview-control-c"></a>Używanie kontrolek TemplateField w kontrolce DetailsView (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) lub [Pobierz plik PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Te same możliwości używanie TemplateField dostępne w widoku GridView są również dostępne w kontrolce DetailsView. W tym samouczku w czasie zostanie wyświetlony jeden produkt z użyciem widoku DetailsView zawierającego używanie TemplateField.

## <a name="introduction"></a>Wprowadzenie

TemplateField oferuje wyższy poziom elastyczności podczas renderowania danych niż BoundField, CheckBoxField, HyperLinkField i inne kontrolki pola danych. W [poprzednim samouczku](using-templatefields-in-the-gridview-control-cs.md) TemplateField w widoku GridView, aby:

- Wyświetla wiele wartości pól danych w jednej kolumnie. W każdym przypadku pola `FirstName` i `LastName` zostały połączone w jedną kolumnę GridView.
- Użyj alternatywnej kontrolki sieci Web, aby wyrazić wartość pola danych. Dowiesz się, jak pokazać wartość `HiredDate` przy użyciu kontrolki Calendar.
- Pokaż informacje o stanie na podstawie danych źródłowych. Gdy tabela `Employees` nie zawiera kolumny zwracającej liczbę dni, w których pracownik przeprowadził zadanie, można było wyświetlić takie informacje w przykładzie GridView w poprzednim samouczku z użyciem metody TemplateField i formatowania.

Te same możliwości używanie TemplateField dostępne w widoku GridView są również dostępne w kontrolce DetailsView. W tym samouczku w czasie zostanie wyświetlony jeden produkt z użyciem widoku DetailsView, który zawiera dwa używanie TemplateField. Pierwszy TemplateField łączy pola danych `UnitPrice`, `UnitsInStock`i `UnitsOnOrder` w jeden wiersz DetailsView. Druga TemplateField będzie wyświetlać wartość pola `Discontinued`, ale używa metody formatowania do wyświetlania wartości "YES", jeśli `Discontinued` jest `true`i "NO" w przeciwnym razie.

[![dwa używanie TemplateField są używane do dostosowywania wyświetlania](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Rysunek 1**: dwa używanie TemplateField są używane do dostosowywania wyświetlania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))

Zacznijmy!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Krok 1. Powiązywanie danych w widoku DetailsView

Zgodnie z opisem w poprzednim samouczku, podczas pracy z używanie TemplateField często najłatwiej zacząć od utworzenia kontrolki DetailsView zawierającej tylko BoundFields, a następnie dodania nowego używanie TemplateField lub przekonwertowania istniejącej BoundFields na używanie TemplateField w razie potrzeby . W związku z tym należy uruchomić ten samouczek przez dodanie widoku DetailsView do strony za pomocą projektanta i powiązanie go z elementem ObjectDataSource, który zwraca listę produktów. Te kroki spowodują utworzenie widoku DetailsView z BoundFields dla każdego z pól wartości innych niż logiczna produktu i CheckBoxField dla jednego pola wartości logicznej (nieobsługiwane).

Otwórz stronę `DetailsViewTemplateField.aspx` i przeciągnij element DetailsView z przybornika do projektanta. Z tagu inteligentnego DetailsView wybierz, aby dodać nową kontrolkę ObjectDataSource, która wywołuje metodę `GetProducts()` klasy `ProductsBLL`.

[![dodać nowej kontrolki ObjectDataSource, która wywołuje metodę getProducts ()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Rysunek 2**. Dodawanie nowej kontrolki ObjectDataSource, która wywołuje metodę `GetProducts()` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))

Dla tego raportu Usuń `ProductID`, `SupplierID`, `CategoryID`i `ReorderLevel` BoundFields. Następnie zmień kolejność BoundFields, tak aby `CategoryName` i `SupplierName` BoundFields pojawiają się bezpośrednio po `ProductName` BoundField. Dowolnie Dostosuj właściwości `HeaderText` i formatowanie BoundFields. Podobnie jak w przypadku widoku GridView, te edycje na poziomie BoundField można wykonywać za pomocą okna dialogowego pola (dostępne przez kliknięcie linku Edytuj pola w tagu inteligentnym DetailsView) lub za pomocą składni deklaracyjnej. Na koniec wyczyść wartości właściwości `Height` i `Width` DetailsView, aby umożliwić rozszerzanie formantu DetailsView na podstawie wyświetlanych danych i zaznacz pole wyboru Włącz stronicowanie w tagu inteligentnym.

Po wprowadzeniu tych zmian znaczniki deklaratywne kontrolki DetailsView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Poświęć chwilę na wyświetlenie strony za pomocą przeglądarki. W tym momencie powinien zostać wyświetlony pojedynczy produkt (Chai) z wierszami przedstawiającymi nazwę produktu, kategorię, dostawcę, cenę, jednostki w magazynie, jednostki w kolejności i stan wycofania.

[![szczegóły produktu są wyświetlane przy użyciu serii BoundFields](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Rysunek 3**. szczegóły produktu są wyświetlane przy użyciu serii BoundFields ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))

## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Krok 2. połączenie ceny, jednostek w magazynie i jednostek w celu przekroczenia jednego wiersza

Element DetailsView ma wiersz dla pól `UnitPrice`, `UnitsInStock`i `UnitsOnOrder`. Można połączyć te pola danych w jeden wiersz z TemplateField przez dodanie nowego TemplateField lub poprzez konwersję jednego z istniejących `UnitPrice`, `UnitsInStock`i `UnitsOnOrder` BoundFields do TemplateField. Mimo że preferuję Konwertowanie istniejących BoundFields, zalecamymy dodanie nowej TemplateField.

Zacznij od kliknięcia linku Edytuj pola w tagu inteligentnym DetailsView, aby wyświetlić okno dialogowe pola. Następnie Dodaj nową TemplateField i ustaw jej Właściwość `HeaderText` na "Price i Inventory" i Przenieś nowy TemplateField, tak aby był umieszczony powyżej `UnitPrice` BoundField.

[![dodać nowego TemplateField do kontrolki DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Ilustracja 4**. Dodawanie nowego TemplateField do kontrolki DetailsView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))

Ponieważ ta nowa TemplateField będzie zawierać wartości aktualnie wyświetlane w `UnitPrice`, `UnitsInStock`i `UnitsOnOrder` BoundFields, usuń je.

Ostatnim zadaniem dla tego kroku jest zdefiniowanie `ItemTemplate` znaczników cen i spisu, które można wykonać za pomocą interfejsu edycji szablonu w widoku DetailsView w projektancie lub ręcznie poprzez składnię deklaratywną formantu. Podobnie jak w przypadku widoku GridView, można uzyskać dostęp do interfejsu edytowania szablonu DetailsView, klikając łącze Edytuj szablony w tagu inteligentnym. W tym miejscu możesz wybrać szablon do edycji z listy rozwijanej, a następnie dodać dowolne kontrolki sieci Web z przybornika.

Na potrzeby tego samouczka Zacznij od dodania kontrolki etykieta do `ItemTemplate`ceny i spisu. Następnie kliknij link Edytuj powiązania danych z tagu inteligentnego kontrolki sieci Web etykiety i powiąż Właściwość `Text` z polem `UnitPrice`.

[![powiązać właściwość text etykiety z polem dane CenaJednostkowa](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Rysunek 5**. powiązanie właściwości `Text` etykiety z polem danych `UnitPrice` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))

## <a name="formatting-the-price-as-a-currency"></a>Formatowanie ceny jako waluty

Dodanie tej opcji oznacza, że cena i TemplateField spisu kontrolki sieci Web będzie teraz wyświetlała tylko cenę wybranego produktu. Rysunek 6 przedstawia zrzut ekranu dotyczący postępu z tego względu, gdy jest wyświetlany za pomocą przeglądarki.

[![cena i TemplateField spisu przedstawiają cenę](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Ilustracja 6**. cena i TemplateField spisu przedstawiają cenę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))

Należy pamiętać, że cena produktu nie jest sformatowana jako waluta. W przypadku BoundField formatowanie jest możliwe przez ustawienie właściwości `HtmlEncode` na `false` i Właściwość `DataFormatString` na `{0:formatSpecifier}`. W przypadku elementu TemplateField jednak wszelkie instrukcje formatowania muszą być określone w składni wiązania danych lub za pomocą metody formatowania zdefiniowanej w obrębie kodu aplikacji (na przykład w klasie z kodem związanym ze stroną ASP.NET).

Aby określić formatowanie składni wiązania danych używanej w kontrolce etykieta sieci Web, Wróć do okna dialogowego Łączenie danych, klikając łącze Edytuj powiązania danych z tagu inteligentnego etykiety. Instrukcje formatowania można wpisać bezpośrednio na liście rozwijanej format lub wybrać jeden z ciągów z zdefiniowanym formatem. Podobnie jak w przypadku właściwości `DataFormatString` BoundField, formatowanie jest określane przy użyciu `{0:formatSpecifier}`.

W polu `UnitPrice` Użyj formatowania waluty określonego przez wybranie odpowiedniej wartości listy rozwijanej lub przez ręczne wpisanie `{0:C}`.

[![sformatować ceny jako walutę](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Rysunek 7**. formatowanie ceny jako waluty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))

W sposób deklaratywny Specyfikacja formatowania jest wskazywana jako drugi parametr w metodach `Bind` lub `Eval`. Ustawienia wprowadzane przez projektanta powodują następujące wyrażenie wiązania danych w znacznikach deklaratywnych:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Dodawanie pozostałych pól danych do TemplateField

W tym momencie Wyświetlono i sformatowano pole danych `UnitPrice` w TemplateField ceny i spisu, ale nadal trzeba wyświetlić pola `UnitsInStock` i `UnitsOnOrder`. Wyświetlmy je w wierszu poniżej ceny i w nawiasach. W interfejsie edycji szablonu w projektancie takie oznakowanie może być dodawane przez pozycjonowanie kursora w szablonie i po prostu wpisanie tekstu do wyświetlenia. Alternatywnie, adiustację można wprowadzać bezpośrednio w składni deklaracyjnej.

Dodaj znacznik statyczny, etykiety kontrolek sieci Web i składnię wiązania danych, tak aby cena i spis TemplateField wyświetlić informacje o cenie i spisie, takie jak:

*Jednostk*  
(**W magazynie/w kolejności:** *UnitsInStock* / *UnitsOnOrder*)

Po wykonaniu tego zadania znaczniki deklaratywne DetailsView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Dzięki tym zmianom dane dotyczące ceny i spisu zostały skonsolidowane w pojedynczy wiersz DetailsView.

[![informacje o cenie i spisie są wyświetlane w pojedynczym wierszu](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Ilustracja 8**. Informacje o cenach i spisie są wyświetlane w pojedynczym wierszu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))

## <a name="step-3-customizing-the-discontinued-field-information"></a>Krok 3. dostosowanie niewycofanych informacji o polach

Kolumna `Discontinued` tabeli `Products` jest wartością bitową, która wskazuje, czy produkt został wycofany. W przypadku powiązania widoku DetailsView (lub GridView) z kontrolką źródła danych pola wartości logicznej, takie jak `Discontinued`, są implementowane jako CheckBoxFields, natomiast pola wartości nielogicznej, takie jak `ProductID`, `ProductName`i tak dalej, są implementowane jako BoundFields. CheckBoxField renderuje jako wyłączone pole wyboru, które jest zaznaczone, jeśli pole danych ma wartość true i usunięto zaznaczenie w przeciwnym razie.

Zamiast wyświetlać CheckBoxField możemy chcieć wyświetlać tekst wskazujący, czy produkt jest nieobsługiwany. Aby to osiągnąć, można usunąć CheckBoxField z widoku DetailsView, a następnie dodać BoundField, których właściwość `DataField` została ustawiona na `Discontinued`. Poświęć chwilę. Po tej zmianie w widoku DetailsView zostanie wyświetlony tekst "true" dla nieaktywnych produktów i "false" dla produktów, które są nadal aktywne.

[![ciągi true i false są używane do wyświetlania stanu wycofania](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Ilustracja 9**. ciągi true i false są używane do wyświetlania stanu wycofania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))

Załóżmy, że nie chcemy używać ciągów "true" lub "false", ale "YES" i "NO". Takie dostosowanie można wykonać przy użyciu metody TemplateField i formatowania. Metoda formatowania może przyjmować dowolną liczbę parametrów wejściowych, ale musi zwrócić kod HTML (jako ciąg), aby wstrzyknąć do szablonu.

Dodaj metodę formatowania do klasy z kodem związanym `DetailsViewTemplateField.aspx` strony o nazwie `DisplayDiscontinuedAsYESorNO`, która akceptuje wartość logiczną jako parametr wejściowy i zwraca ciąg. Zgodnie z opisem w poprzednim samouczku, ta metoda *musi* być oznaczona jako `protected` lub `public`, aby była dostępna z szablonu.

[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Ta metoda sprawdza parametr wejściowy (`discontinued`) i zwraca wartość "YES", jeśli jest `true`, "NO".

> [!NOTE]
> W metodzie formatowania, które zostały sprawdzone w poprzednim samouczku, odwołaj się do pola danych, które może zawierać `NULL` s i w związku z tym sprawdzić, czy wartość właściwości `HiredDate` pracownika miała `NULL` wartość przed uzyskaniem dostępu do właściwości `HiredDate` `EmployeesRow`. Takie sprawdzenie nie jest konieczne, ponieważ kolumna `Discontinued` nigdy nie może mieć przypisanych wartości `NULL` bazy danych. Ponadto jest to dlatego, że metoda może akceptować parametr wejściowy Boolean zamiast akceptować wystąpienie `ProductsRow` lub parametr typu `object`.

Po ukończeniu tej metody formatowania wszystko to, co pozostanie, aby wywołać ją z `ItemTemplate`TemplateField. Aby utworzyć TemplateField, Usuń `Discontinued` BoundField i Dodaj nową TemplateField lub Skonwertuj `Discontinued` BoundField do TemplateField. Następnie w widoku znaczników deklaratywnych Edytuj TemplateField tak, aby zawierał on tylko ItemTemplate, który wywołuje metodę `DisplayDiscontinuedAsYESorNO`, przekazując wartość właściwości `Discontinued` bieżącej instancji `ProductRow`. Dostęp do niego można uzyskać za pomocą metody `Eval`. W każdym przypadku Adiustacja TemplateField powinna wyglądać następująco:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

Spowoduje to wywołanie metody `DisplayDiscontinuedAsYESorNO` podczas renderowania widoku DetailsView, przekazując w wartości `Discontinued` wystąpienia `ProductRow`. Ponieważ metoda `Eval` zwraca wartość typu `object`, ale metoda `DisplayDiscontinuedAsYESorNO` oczekuje parametru wejściowego typu `bool`, Rzutowanie metod `Eval` zwróci wartość na `bool`. Metoda `DisplayDiscontinuedAsYESorNO` zwróci wartość "YES" lub "NO" w zależności od wartości otrzymanej. Zwracana wartość jest wyświetlana w tym wierszu DetailsView (Zobacz Rysunek 10).

[![tak lub żadne wartości nie są teraz wyświetlane w wierszu wycofane](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Rysunek 10**. wartości tak lub nie są teraz wyświetlane w[niezmienionym wierszu (kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))

## <a name="summary"></a>Podsumowanie

TemplateField w formancie DetailsView umożliwia wyższy poziom elastyczności wyświetlania danych niż jest dostępny z innymi kontrolkami pól i jest idealnym rozwiązaniem w sytuacjach, gdy:

- W jednej kolumnie GridView muszą być wyświetlone wiele pól danych
- Dane są najlepiej wyrażone przy użyciu kontrolki sieci Web, a nie zwykłego tekstu.
- Dane wyjściowe są zależne od danych źródłowych, takich jak wyświetlanie metadanych lub ponowne formatowanie danych

Mimo że używanie TemplateField zapewniają większą elastyczność podczas renderowania danych bazowych w widoku DetailsView, dane wyjściowe DetailsView nadal uważają bit boxy, ponieważ każde pole jest renderowane jako wiersz w `<table>`HTML.

Kontrolka FormView oferuje większą elastyczność konfigurowania renderowanych danych wyjściowych. FormView nie zawiera pól, ale nie tylko szereg szablonów (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`itd.). Zobaczymy, jak używać FormView, aby osiągnąć jeszcze większą kontrolę nad renderowanym układem w naszym następnym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Osoba dokonująca przeglądu potencjalnego klienta dla tego samouczka miała wartość Dan Jagers. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-templatefields-in-the-gridview-control-cs.md)
> [dalej](using-the-formview-s-templates-cs.md)
