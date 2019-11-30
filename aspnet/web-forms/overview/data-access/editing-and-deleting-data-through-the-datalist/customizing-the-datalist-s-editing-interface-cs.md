---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: Dostosowywanie interfejsu edycji elementu DataList (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku utworzymy bogatszy interfejs do edycji dla elementu DataList, taki, który zawiera kontrolek DropDownList i pole wyboru.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 81f5a7f6737f544f577447f263dbd37dbc8279d9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74623856"
---
# <a name="customizing-the-datalists-editing-interface-c"></a>Dostosowywanie interfejsu edycji kontrolki DataList (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) lub [Pobierz plik PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> W tym samouczku utworzymy bogatszy interfejs do edycji dla elementu DataList, taki, który zawiera kontrolek DropDownList i pole wyboru.

## <a name="introduction"></a>Wprowadzenie

Kontrolki znaczników i sieci Web na liście DataList s `EditItemTemplate` definiują jej interfejs edytowalny. We wszystkich edytowalnych przykładach DataList dodaliśmy do tej pory, edytowalny interfejs został złożony z kontrolek sieci Web typu TextBox. W [poprzednim samouczku](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) Ulepszono środowisko użytkownika w czasie edycji, dodając kontrolki walidacji.

`EditItemTemplate` można rozszerzyć, aby obejmowały kontrolki sieci Web inne niż pole tekstowe, takie jak kontrolek DropDownList, RadioButtonLists, Calendars i tak dalej. Podobnie jak w przypadku pól tekstowych, podczas dostosowywania interfejsu edycji do dołączania innych kontrolek sieci Web należy wykonać następujące czynności:

1. Dodaj formant sieci Web do `EditItemTemplate`.
2. Użyj składni wiązania z danymi, aby przypisać odpowiednią wartość pola danych do odpowiedniej właściwości.
3. W obsłudze zdarzeń `UpdateCommand` program programowo uzyskuje dostęp do wartości kontrolki sieci Web i przekazuje ją do odpowiedniej metody LOGIKI biznesowej.

W tym samouczku utworzymy bogatszy interfejs do edycji dla elementu DataList, taki, który zawiera kontrolek DropDownList i pole wyboru. W szczególności utworzymy element DataList, który zawiera informacje o produkcie i umożliwi aktualizację nazwy produktu, dostawcy, kategorii i stanu wycofanego (patrz rysunek 1).

[![interfejsie edycji zawiera pole tekstowe, dwie kontrolek DropDownList i pole wyboru](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Rysunek 1**: interfejs edycji zawiera pole tekstowe, dwa kontrolek DropDownList i pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))

## <a name="step-1-displaying-product-information"></a>Krok 1. Wyświetlanie informacji o produkcie

Przed utworzeniem interfejsu, który można edytować, należy najpierw skompilować interfejs tylko do odczytu. Aby rozpocząć, Otwórz stronę `CustomizedUI.aspx` z folderu `EditDeleteDataList` i, z projektanta, Dodaj element DataList do strony, ustawiając jej Właściwość `ID` na `Products`. Z tagu inteligentnego DataList s Utwórz nowy element ObjectDataSource. Nazwij ten nowy element ObjectDataSource `ProductsDataSource` i skonfiguruj go tak, aby pobierał dane z metody `GetProducts` klasy `ProductsBLL`. Podobnie jak w przypadku poprzednio edytowalnych samouczków DataList, zaktualizujemy informacje edytowanego produktu, przechodząc bezpośrednio do warstwy logiki biznesowej. W związku z tym Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak).

[![ustawić listy rozwijane aktualizacji, wstawiania i usuwania na (brak)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Rysunek 2**. Ustawianie listy rozwijanej Aktualizuj, Wstaw i Usuń na (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))

Po skonfigurowaniu elementu ObjectDataSource program Visual Studio utworzy domyślną `ItemTemplate` dla elementu DataList, który wyświetla nazwę i wartość każdego zwracanego pola danych. Zmodyfikuj `ItemTemplate` tak, aby szablon zawiera nazwę produktu w `<h4>` element wraz z nazwą kategorii, nazwą dostawcy, ceną i stanem wycofany. Ponadto Dodaj przycisk Edytuj, aby upewnić się, że jego właściwość `CommandName` jest ustawiona na Edycja. Znaczniki deklaratywne dla My `ItemTemplate` są następujące:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Powyższe oznakowanie określa informacje o produkcie przy użyciu &lt;H4&gt; nagłówka dla nazwy produktu i `<table>` czterech kolumn dla pozostałych pól. Klasy `ProductPropertyLabel` i `ProductPropertyValue` CSS zdefiniowane w `Styles.css`zostały omówione w poprzednich samouczkach. Rysunek 3 pokazuje postęp, gdy oglądasz za pomocą przeglądarki.

[![nazwa, dostawca, Kategoria, stan wycofany i cena każdego produktu są wyświetlane](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Rysunek 3**. zostanie wyświetlona nazwa, dostawca, Kategoria, stan wycofany i cena każdego produktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))

## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Krok 2. Dodawanie kontrolek sieci Web do interfejsu edycji

Pierwszym krokiem tworzenia niestandardowego interfejsu edycji elementu DataList jest dodanie wymaganych kontrolek sieci Web do `EditItemTemplate`. W szczególności potrzebujemy DropDownList dla kategorii, innej dla dostawcy i pola wyboru dla stanu wycofania. Ponieważ w tym przykładzie cena produktu nie jest edytowalna, możemy nadal wyświetlać ją przy użyciu kontrolki sieci Web etykieta.

Aby dostosować interfejs edycji, kliknij link Edytuj szablony z taga inteligentnego DataList s i wybierz opcję `EditItemTemplate` z listy rozwijanej. Dodaj DropDownList do `EditItemTemplate` i ustaw `ID` do `Categories`.

[![dodać DropDownList dla kategorii](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Ilustracja 4**. Dodawanie DropDownList dla kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))

Następnie w tagu inteligentnym DropDownList wybierz opcję Wybierz źródło danych i Utwórz nowy element ObjectDataSource o nazwie `CategoriesDataSource`. Skonfiguruj ten element ObjectDataSource, aby używał metody `GetCategories()` `CategoriesBLL` Class (patrz rysunek 5). Następnie Kreator konfiguracji źródła danych DropDownList s będzie monitował o pola danych, które mają być używane dla każdej `ListItem` s `Text` i `Value` właściwości. DropDownList wyświetlić pola danych `CategoryName` i użyj `CategoryID` jako wartości, jak pokazano na rysunku 6.

[![utworzyć nowego elementu ObjectDataSource o nazwie CategoriesDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Rysunek 5**. Utwórz nowy element ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))

[![skonfigurować pola wyświetlania i wartości DropDownList](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Ilustracja 6**. Konfigurowanie pól wyświetlania DropDownList i wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))

Powtórz tę serię kroków, aby utworzyć DropDownList dla dostawców. Ustaw `ID` dla tego DropDownList, aby `Suppliers` i nazwij jego `SuppliersDataSource`ObjectDataSource.

Po dodaniu dwóch kontrolek DropDownList Dodaj pole wyboru dla stanu wycofane i pole tekstowe dla nazwy produktu. Ustaw `ID` s dla pola wyboru i pola tekstowego na odpowiednio `Discontinued` i `ProductName`. Dodaj RequiredFieldValidator, aby upewnić się, że użytkownik poda wartość dla nazwy produktu.

Na koniec Dodaj przyciski Aktualizuj i Anuluj. Należy pamiętać, że w przypadku tych dwóch przycisków jest konieczne, aby ich właściwości `CommandName` zostały odpowiednio ustawione na wartość Update i Cancel.

Możesz jednak bezproblemowo określić interfejs do edycji. Chcę użyć tego samego układu `<table>` z czterema kolumnami z poziomu interfejsu tylko do odczytu, ponieważ ilustruje się następujące składnie deklaracyjne i zrzut ekranu:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]

[![interfejs edytowania jest określany jako interfejs tylko do odczytu](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Rysunek 7**. interfejs edycji jest określany jako interfejs tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))

## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Krok 3. tworzenie procedur obsługi zdarzeń EditCommand i CancelCommand

Obecnie w `EditItemTemplate` nie ma składni powiązań danych (z wyjątkiem `UnitPriceLabel`, która została skopiowana przez Verbatim z `ItemTemplate`). W chwilę dodamy składnię powiązania danych, ale najpierw zezwolimy na tworzenie obsługi zdarzeń dla `EditCommand` i zdarzeń `CancelCommand` dla elementu DataList. Należy odwołać się do funkcji obsługi zdarzeń `EditCommand` jest renderowanie interfejsu edycji elementu DataList, którego przycisk edycji został kliknięty, podczas gdy zadanie `CancelCommand` s ma zwrócić element DataList do stanu sprzed edycji.

Utwórz te dwa programy obsługi zdarzeń i użyj następującego kodu:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

Po wybraniu tych dwóch programów obsługi zdarzeń kliknięcie przycisku Edytuj powoduje wyświetlenie interfejsu edycji, a kliknięcie przycisku Anuluj spowoduje zwrócenie edytowanego elementu do trybu tylko do odczytu. Rysunek 8 pokazuje element DataList po kliknięciu przycisku Edytuj dla Chef Anton s Gumbo mix. Ponieważ nie chcę dodać żadnej składni DataBinding do interfejsu edycji, `ProductName` pole tekstowe jest puste, pole wyboru `Discontinued` niezaznaczone i pierwsze elementy wybrane z `Categories` i `Suppliers` kontrolek DropDownList.

[![kliknięciu przycisku Edytuj wyświetla interfejs edycji](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Ilustracja 8**. kliknięcie przycisku Edytuj wyświetla interfejs edycji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))

## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Krok 4. Dodawanie składni wiązania danych do interfejsu edycji

Aby w interfejsie edycji były wyświetlane bieżące wartości produktu, należy użyć składni DataBinding do przypisania wartości pól danych do odpowiednich wartości kontrolki sieci Web. Składnię DataBinding można zastosować za pośrednictwem projektanta, przechodząc do ekranu Edytowanie szablonów i wybierając łącze Edytuj powiązania danych z tagów inteligentnych formantów sieci Web. Alternatywnie, można dodać składnię wiązania bezpośrednio do znacznika deklaratywnego.

Przypisz wartość pola `ProductName` danych do właściwości `ProductName` TextBox s `Text`, wartości pola `CategoryID` i `SupplierID` danych do `Categories` i `Suppliers` kontrolek DropDownList `SelectedValue` właściwości, a wartość pola dane `Discontinued` do `Discontinued` pole wyboru. Po wprowadzeniu tych zmian przez projektanta lub bezpośrednio za pomocą znaczników deklaratywnych należy ponownie odwiedzić stronę za pomocą przeglądarki, a następnie kliknąć przycisk Edytuj dla Chef Anton s Gumbo mix. Jak pokazano na rysunku 9, składnia wiązania danych dodaliśmy bieżące wartości do pola tekstowego, kontrolek DropDownList i pola wyboru.

[![kliknięciu przycisku Edytuj wyświetla interfejs edycji](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Ilustracja 9**. kliknięcie przycisku Edytuj wyświetla interfejs edycji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))

## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Krok 5. zapisywanie zmian użytkownika w programie obsługi zdarzeń elementu UpdateCommand

Gdy użytkownik edytuje produkt i klika przycisk Aktualizuj, następuje ogłaszanie zwrotne i wyzwalane jest zdarzenie DataList s `UpdateCommand`. W programie obsługi zdarzeń musimy odczytać wartości z formantów sieci Web w `EditItemTemplate` i interfejsie z LOGIKI biznesowej, aby zaktualizować produkt w bazie danych. Jak widać w poprzednich samouczkach, `ProductID` zaktualizowanego produktu są dostępne za pomocą kolekcji `DataKeys`. Pola wprowadzone przez użytkownika są dostępne przez programowe odwołujące się do formantów sieci Web przy użyciu `FindControl("controlID")`, jak pokazano w poniższym kodzie:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

Kod rozpocznie się, sprawdzając Właściwość `Page.IsValid`, aby upewnić się, że wszystkie kontrolki walidacji na stronie są prawidłowe. Jeśli `Page.IsValid` jest `True`, edytowane wartości `ProductID` produktu są odczytywane z kolekcji `DataKeys` i kontrolki sieci Web wprowadzania danych w `EditItemTemplate` są programistycznie przywoływane. Następnie wartości z tych kontrolek sieci Web są odczytywane do zmiennych, które następnie są przesyłane do odpowiedniego przeciążenia `UpdateProduct`. Po zaktualizowaniu danych elementu DataList jest zwracana do jego stanu sprzed edycji.

> [!NOTE]
> W przypadku pominięcia logiki obsługi wyjątków dodanej w samouczku [Obsługa wyjątków logiki biznesowej i dal](handling-bll-and-dal-level-exceptions-cs.md) w celu zachowania kodu i tego przykładu. W ramach ćwiczenia Dodaj tę funkcję po zakończeniu tego samouczka.

## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Krok 6. Obsługa wartości IDKategorii i IDDostawcy o wartości NULL

Baza danych Northwind umożliwia `NULL` wartości dla `Products` tabeli `CategoryID` i `SupplierID` kolumn. Jednak nasz interfejs edycji nie obsługuje obecnie `NULL` wartości. Jeśli spróbujesz edytować produkt, który ma `NULL` wartość dla kolumn `CategoryID` lub `SupplierID`, otrzymamy `ArgumentOutOfRangeException` z komunikatem o błędzie podobnym do: *"Kategorie" ma element SelectedValue, który jest nieprawidłowy, ponieważ nie istnieje na liście elementów.* Ponadto nie ma obecnie możliwości zmiany kategorii produktu ani wartości dostawcy z wartości innej niż`NULL` na `NULL` jeden.

Aby obsłużyć `NULL` wartości dla kategorii i dostawcy kontrolek DropDownList, musimy dodać dodatkowe `ListItem`. Chcę użyć (brak) jako wartości `Text` dla tego `ListItem`, ale możesz zmienić ją na coś innego (na przykład pustego ciągu), jeśli lubisz. Na koniec należy pamiętać o ustawieniu `AppendDataBoundItems` kontrolek DropDownList na `True`; Jeśli zapomnisz to zrobić, kategorie i dostawcy powiązane z DropDownList zastąpią statycznie dodaną `ListItem`.

Po wprowadzeniu tych zmian znacznik kontrolek DropDownList w `EditItemTemplate` DataList s powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Statyczne `ListItem` s można dodać do DropDownList za pomocą projektanta lub bezpośrednio za pomocą składni deklaracyjnej. Podczas dodawania elementu DropDownList do reprezentowania bazy danych `NULL` wartość należy dodać `ListItem` za pomocą składni deklaratywnej. Jeśli używasz edytora kolekcji `ListItem` w projektancie, wygenerowana składnia deklaracyjne spowoduje całkowite pominięcie ustawienia `Value` przy przypisaniu pustego ciągu, tworząc znaczniki deklaratywne, takie jak: `<asp:ListItem>(None)</asp:ListItem>`. Chociaż może to wyglądać nieszkodliwie, brak `Value` powoduje, że DropDownList używają wartości właściwości `Text` w tym miejscu. Oznacza to, że w przypadku wybrania `NULL` `ListItem` zostanie podjęta próba przypisania wartości (brak) do pola dane produktu (`CategoryID` lub `SupplierID`w tym samouczku), co spowoduje wyjątek. Po ustawieniu jawnie `Value=""`wartość `NULL` zostanie przypisana do pola dane produktu po wybraniu `NULL` `ListItem`.

Poświęć chwilę na wyświetlenie postępu w przeglądarce. Podczas edytowania produktu należy zauważyć, że `Categories` i `Suppliers` kontrolek DropDownList mają opcję (None) na początku DropDownList.

[![kategorie i dostawcy kontrolek DropDownList zawierać opcję (brak)](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Ilustracja 10**. `Categories` i `Suppliers` kontrolek DropDownList zawierają opcję (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))

Aby zapisać opcję (brak) jako bazę danych `NULL` wartość, musimy wrócić do programu obsługi zdarzeń `UpdateCommand`. Zmień zmienne `categoryIDValue` i `supplierIDValue` na wartości null integer i przypisz im wartość inną niż `Nothing` tylko wtedy, gdy DropDownList `SelectedValue` s nie jest pustym ciągiem:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

W przypadku tej zmiany wartość `Nothing` zostanie przeniesiona do metody `UpdateProduct` LOGIKI biznesowej, jeśli użytkownik wybrał opcję (brak) z jednej z list rozwijanych, która odnosi się do `NULL` wartości bazy danych.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia bardziej złożonego interfejsu edycji elementu DataList, który zawiera trzy różne wejściowe kontrolki sieci Web, pole tekstowe, dwa kontrolek DropDownList i pole wyboru wraz z kontrolkami walidacji. Podczas kompilowania interfejsu edycji czynności są takie same, niezależnie od używanej kontrolki sieci Web: Zacznij od dodania formantów sieci Web do elementu DataList s `EditItemTemplate`; Użyj składni wiązania z danymi, aby przypisać odpowiednie wartości pól danych do odpowiednich właściwości kontrolki sieci Web; w programie obsługi zdarzeń `UpdateCommand` program programowo uzyskuje dostęp do kontrolek sieci Web i ich odpowiednich właściwości, przekazując ich wartości do LOGIKI biznesowej.

Podczas tworzenia interfejsu edycji, bez względu na to, czy składa się on z tylko pól tekstowych, czy też kolekcji różnych kontrolek sieci Web, należy pamiętać o prawidłowym obsłudze wartości `NULL` bazy danych. Podczas ewidencjonowania aktywności dla `NULL` s należy koniecznie, aby nie tylko prawidłowo wyświetlić istniejącą `NULL` wartość w interfejsie edycji, ale również oferować metodę oznaczania wartości jako `NULL`. W przypadku kontrolek DropDownList w elementach datalists zwykle oznacza to dodanie statycznej `ListItem`, której Właściwość `Value` jest jawnie ustawiona na pusty ciąg (`Value=""`) i dodanie bitu kodu do procedury obsługi zdarzeń `UpdateCommand`, aby określić, czy `NULL``ListItem` został wybrany.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Dennis Patterson, David suru i Randy Schmidt. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [dalej](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
