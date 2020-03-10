---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: Dodawanie przycisków i reagowanie na nie w widokuC#GridView () | Microsoft Docs
author: rick-anderson
description: W tym samouczku powiesz się, jak dodać niestandardowe przyciski, zarówno do szablonu, jak i do pól kontrolki GridView lub DetailsView. W szczególności będziemy lecenia...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5c87386e4fe2c53b39162071689f2522dcc6c7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78549645"
---
# <a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Dodawanie przycisków i reagowanie na nie w kontrolce GridView (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) lub [Pobierz plik PDF](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> W tym samouczku powiesz się, jak dodać niestandardowe przyciski, zarówno do szablonu, jak i do pól kontrolki GridView lub DetailsView. W szczególności utworzymy interfejs, który ma FormView, który umożliwi użytkownikowi dostęp do stron przez dostawców.

## <a name="introduction"></a>Wprowadzenie

Chociaż wiele scenariuszy raportowania obejmuje dostęp tylko do odczytu do danych raportu, nie jest to rzadko spotykane w przypadku raportów, które obejmują możliwość wykonywania akcji na podstawie wyświetlanych danych. Zwykle dotyczy to Dodawanie kontrolki sieci Web Button, element LinkButton lub kliknięto element ImageButton przy użyciu każdego rekordu wyświetlanego w raporcie, który po kliknięciu powoduje odświeżenie i wywołanie kodu po stronie serwera. Najbardziej typowym przykładem jest edytowanie i usuwanie danych w oparciu o rekord po rekordzie. W rzeczywistości od momentu, w którym zawarto instrukcje dotyczące [wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , edytowanie i usuwanie jest tak popularne, że formanty GridView, DetailsView i FormView mogą obsługiwać takie funkcje bez konieczności pisania pojedynczego wiersza kodu.

Oprócz przycisków Edytuj i Usuń, formanty GridView, DetailsView i FormView mogą również zawierać przyciski, LinkButtons lub ImageButtons, które po kliknięciu wykonują niestandardową logikę po stronie serwera. W tym samouczku powiesz się, jak dodać niestandardowe przyciski, zarówno do szablonu, jak i do pól kontrolki GridView lub DetailsView. W szczególności utworzymy interfejs, który ma FormView, który umożliwi użytkownikowi dostęp do stron przez dostawców. Dla danego dostawcy FormView wyświetli informacje o dostawcy wraz z kontrolką sieci Web przycisku, która po kliknięciu spowoduje oznaczenie wszystkich skojarzonych produktów jako nieobsługiwane. Ponadto w widoku GridView są wyświetlane produkty dostarczone przez wybranego dostawcę, z każdym wierszem zawierającym przyciski Zwiększ cenę i cena rabatu, które po kliknięciu powodują podnoszenie lub zmniejszenie `UnitPrice` produktu o 10% (patrz rysunek 1).

[![oba elementy FormView i GridView zawierają przyciski, które wykonują akcje niestandardowe](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Rysunek 1**: oba elementy FormView i GridView zawierają przyciski, które wykonują akcje niestandardowe ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))

## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Krok 1. Dodawanie stron sieci Web samouczka przycisku

Zanim sprawdzimy, jak dodać niestandardowe przyciski, Zróbmy najpierw chwilę, aby utworzyć strony ASP.NET w naszym projekcie witryny sieci Web, które będą potrzebne w tym samouczku. Zacznij od dodania nowego folderu o nazwie `CustomButtons`. Następnie Dodaj następujące dwie strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `CustomButtons.aspx`

![Dodaj strony ASP.NET dla samouczków związanych z przyciskami niestandardowymi](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**Rysunek 2**. dodawanie stron ASP.NET dla samouczków związanych z przyciskami niestandardowymi

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `CustomButtons` wyświetli samouczki w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. W związku z tym Dodaj tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Rysunek 3**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))

Na koniec Dodaj strony jako wpisy do pliku `Web.sitemap`. W każdym przypadku Dodaj następujące znaczniki po stronicowaniu i sortowaniu `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy do edycji, wstawiania i usuwania samouczków.

![Mapa witryny zawiera teraz wpis do samouczka dotyczącego przycisków niestandardowych](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Rysunek 4**. Mapa witryny zawiera teraz wpis do samouczka dotyczącego przycisków niestandardowych

## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Krok 2. Dodawanie widoku FormView zawierającego listę dostawców

Zacznijmy pracę z tym samouczkiem, dodając FormView, który wyświetla listę dostawców. Zgodnie z opisem we wprowadzeniu, ten formant FormView umożliwi użytkownikowi dostęp do stron przez dostawców, pokazując produkty dostarczone przez dostawcę w widoku GridView. Ponadto ten formant FormView będzie zawierał przycisk, który po kliknięciu spowoduje oznaczenie wszystkich produktów dostawcy jako niewykorzystane. Zanim będziemy wypróbujemy z dodaniem niestandardowego przycisku do FormView, najpierw utworzymy FormView, aby wyświetlał informacje o dostawcy.

Zacznij od otwarcia strony `CustomButtons.aspx` w folderze `CustomButtons`. Dodaj do strony formant FormView, przeciągając go z przybornika do projektanta i ustawiając jego właściwość `ID` na `Suppliers`. W tagu inteligentnym FormView Utwórz nowy element ObjectDataSource o nazwie `SuppliersDataSource`.

[![utworzyć nowego elementu ObjectDataSource o nazwie SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Rysunek 5**. Utwórz nowy element ObjectDataSource o nazwie `SuppliersDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))

Skonfiguruj nowy element ObjectDataSource, który będzie wysyłać zapytania z metody `GetSuppliers()` klasy `SuppliersBLL` (patrz rysunek 6). Ponieważ ten FormView nie udostępnia interfejsu do aktualizowania informacji o dostawcy, wybierz opcję (brak) z listy rozwijanej na karcie aktualizacja.

[![skonfigurować źródło danych tak, aby korzystało z metody getdostawcy () klasy SuppliersBLL](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Ilustracja 6**. Skonfiguruj źródło danych tak, aby korzystało z metody `GetSuppliers()` `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))

Po skonfigurowaniu elementu ObjectDataSource program Visual Studio wygeneruje `InsertItemTemplate`, `EditItemTemplate`i `ItemTemplate` dla widoku FormView. Usuń `InsertItemTemplate` i `EditItemTemplate` i zmodyfikuj `ItemTemplate` tak, aby wyświetlał tylko nazwę firmy dostawcy i numer telefonu. Na koniec Włącz obsługę stronicowania dla FormView, zaznaczając pole wyboru Włącz stronicowanie w tagu inteligentnym (lub ustawiając jego właściwość `AllowPaging` na `True`). Po wprowadzeniu zmian znaczniki deklaratywne strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

Rysunek 7 przedstawia stronę CustomButtons. aspx wyświetlaną w przeglądarce.

[![FormView wyświetla listę pól NazwaFirmy i telefon z aktualnie wybranego dostawcy](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Rysunek 7**. formant FormView wyświetla listę `CompanyName` i `Phone` pól z aktualnie wybranego dostawcy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))

## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Krok 3. Dodawanie widoku GridView zawierającego listę produktów wybranego dostawcy

Przed dodaniem przycisku Zakończ wszystkie produkty do szablonu FormView, najpierw Dodaj widok GridView poniżej widoku FormView, który zawiera listę produktów dostarczonych przez wybranego dostawcę. Aby to osiągnąć, Dodaj widok GridView do strony, ustaw jej Właściwość `ID` na `SuppliersProducts`i Dodaj nowy element ObjectDataSource o nazwie `SuppliersProductsDataSource`.

[![utworzyć nowego elementu ObjectDataSource o nazwie SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Ilustracja 8**. Tworzenie nowego elementu ObjectDataSource o nazwie `SuppliersProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))

Skonfiguruj ten element ObjectDataSource, aby korzystał z metody `GetProductsBySupplierID(supplierID)` klasy ProductsBLL (zobacz rysunek 9). Ten widok GridView umożliwi dostosowanie ceny produktu, ale nie będzie korzystać z wbudowanej edycji ani usuwania funkcji z widoku GridView. W związku z tym, można ustawić listę rozwijaną na wartość (brak) dla kart aktualizacji, wstawiania i usuwania elementu ObjectDataSource.

[![skonfigurować źródło danych tak, aby korzystało z metody ProductsBLL Class s GetProductsBySupplierID (IDDostawcy)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Ilustracja 9**. Konfigurowanie źródła danych tak, aby korzystało z metody `GetProductsBySupplierID(supplierID)` klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))

Ponieważ metoda `GetProductsBySupplierID(supplierID)` akceptuje parametr wejściowy, Kreator ObjectDataSource monituje nas o źródło wartości tego parametru. Aby przekazać wartość `SupplierID` z widoku FormView, Ustaw listę rozwijaną Źródło parametru na Control i listę rozwijaną ControlID na `Suppliers` (identyfikator FormView utworzony w kroku 2).

[![wskazać, że parametr IDDostawcy powinien pochodzić z kontrolki FormView dostawcy](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Ilustracja 10**. wskazuje, że parametr *`supplierID`* powinien pochodzić z `Suppliers` kontrolki FormView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))

Po ukończeniu działania kreatora ObjectDataSource, GridView będzie zawierać BoundField lub CheckBoxField dla każdego z pól danych produktu. Przytnijmy ten widok, aby pokazać tylko `ProductName` i `UnitPrice` BoundFields wraz z `Discontinued` CheckBoxField; Ponadto sformatujemy `UnitPrice` BoundField w taki sposób, że tekst jest sformatowany jako waluta. Znaczniki deklaratywne GridView i `SuppliersProductsDataSource` elementu ObjectDataSource powinny wyglądać podobnie do następującej:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

W tym momencie nasz samouczek wyświetla raport główny/szczegóły, umożliwiając użytkownikowi wybranie dostawcy z widoku FormView u góry i wyświetlenie produktów dostarczonych przez tego dostawcę za pośrednictwem widoku GridView u dołu. Na rysunku 11 przedstawiono zrzut ekranu przedstawiający Tę stronę podczas wybierania dostawcy Tokio dla sprzedawców z widoku FormView.

[![wybrane produkty dostawcy są wyświetlane w widoku GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Ilustracja 11**. produkty wybranego dostawcy są wyświetlane w widoku GridView (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))

## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Krok 4. Tworzenie metod DAL i LOGIKI biznesowej w celu zaprzestania korzystania ze wszystkich produktów dla dostawcy

Zanim będziemy mogli dodać przycisk do FormView, który po kliknięciu nie będzie kontynuował wszystkich produktów dostawcy, najpierw musimy dodać metodę do zarówno DAL, jak i LOGIKI biznesowej, które wykonują tę akcję. W szczególności ta metoda będzie nazywana `DiscontinueAllProductsForSupplier(supplierID)`. Po kliknięciu przycisku FormView wywołamy tę metodę w warstwie logiki biznesowej, przechodząc do `SupplierID`wybranego dostawcy; LOGIKI biznesowej następnie wywoła do odpowiedniej metody warstwy dostępu do danych, która wystawia do bazy danych instrukcję `UPDATE`, która nie będzie kontynuować określonych produktów dostawcy.

Zgodnie z naszymi poprzednimi samouczkami będziemy używać podejścia dolnego, rozpoczynając od tworzenia metody DAL, a następnie metody LOGIKI biznesowej i na końcu implementując funkcję na stronie ASP.NET. Otwórz zestaw danych `Northwind.xsd` określonym w folderze `App_Code/DAL` i Dodaj nową metodę do `ProductsTableAdapter` (kliknij prawym przyciskiem myszy `ProductsTableAdapter` i wybierz polecenie Dodaj zapytanie). Spowoduje to wyświetlenie Kreatora konfiguracji zapytania TableAdapter, który przeprowadzi Cię przez proces dodawania nowej metody. Zacznij od wskazywania, że nasza metoda DAL będzie używać ad-hoc instrukcji SQL.

[![utworzyć metodę DAL przy użyciu instrukcji SQL ad hoc](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Ilustracja 12**. Tworzenie metody dal przy użyciu instrukcji SQL ad hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))

Następnie Kreator poprosi nas o typ zapytania, który ma zostać utworzony. Ponieważ metoda `DiscontinueAllProductsForSupplier(supplierID)` będzie wymagała aktualizacji tabeli bazy danych `Products`, ustawienie pola `Discontinued` na 1 dla wszystkich produktów dostarczonych przez określony *`supplierID`* wymaga utworzenia zapytania, które aktualizuje dane.

[![wybrać typu zapytania UPDATE](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Ilustracja 13**. Wybierz typ zapytania Update ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))

Na następnym ekranie kreatora znajduje się TableAdapter istniejącej instrukcji `UPDATE`, która aktualizuje wszystkie pola zdefiniowane w `Products` DataTable. Zastąp ten tekst zapytania następującą instrukcją:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

Po wprowadzeniu tego zapytania i kliknięciu przycisku Dalej na ostatnim ekranie zostanie wyświetlony monit o podanie nazwy nowej metody `DiscontinueAllProductsForSupplier`. Ukończ pracę kreatora, klikając przycisk Zakończ. Po powrocie do projektanta obiektów DataSet powinna zostać wyświetlona nowa metoda w `ProductsTableAdapter` o nazwie `DiscontinueAllProductsForSupplier(@SupplierID)`.

[![nazwę nowej metody DAL DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Ilustracja 14**. Nazwa nowej metody dal `DiscontinueAllProductsForSupplier` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))

Przy użyciu metody `DiscontinueAllProductsForSupplier(supplierID)` utworzonej w warstwie dostępu do danych następnym zadaniem jest utworzenie metody `DiscontinueAllProductsForSupplier(supplierID)` w warstwie logiki biznesowej. Aby to zrobić, Otwórz plik klasy `ProductsBLL` i Dodaj następujące elementy:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Ta metoda po prostu wywołuje metodę `DiscontinueAllProductsForSupplier(supplierID)` w DAL, przekazując do podanej wartości parametru *`supplierID`* . Jeśli istnieją jakieś reguły biznesowe, które zezwalają na wycofanie produktów dostawcy w pewnych okolicznościach, te reguły powinny zostać zaimplementowane w tym miejscu w LOGIKI biznesowej.

> [!NOTE]
> W przeciwieństwie do `UpdateProduct` przeciążeń w klasie `ProductsBLL`, podpis metody `DiscontinueAllProductsForSupplier(supplierID)` nie zawiera atrybutu `DataObjectMethodAttribute` (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Wyklucza to metodę `DiscontinueAllProductsForSupplier(supplierID)` z listy rozwijanej Kreatora konfiguracji źródła danych elementu ObjectDataSource na karcie aktualizacja. Ten atrybut został pominięty, ponieważ będziemy wywoływana metodę `DiscontinueAllProductsForSupplier(supplierID)` bezpośrednio z programu obsługi zdarzeń na naszej stronie ASP.NET.

## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Krok 5. Dodawanie przycisku zaniechania wszystkich produktów do widoku FormView

Po zakończeniu `DiscontinueAllProductsForSupplier(supplierID)`j metody w LOGIKI biznesowej i DAL, ostatnim krokiem do dodania możliwości zaprzestania działania wszystkich produktów dla wybranego dostawcy jest dodanie kontrolki sieci Web Button do `ItemTemplate`FormView. Dodajmy ten przycisk poniżej numeru telefonu dostawcy z tekstem przycisku, przechodź wszystkie produkty i `ID` wartość właściwości `DiscontinueAllProductsForSupplier`. Kontrolkę sieci Web tego przycisku można dodać za pomocą projektanta, klikając łącze Edytuj szablony w tagu inteligentnym FormView (patrz rysunek 15) lub bezpośrednio za pomocą składni deklaracyjnej.

[![dodać kontrolkę sieci Web przycisk nie Kontynuuj wszystkie produkty do widoku FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Ilustracja 15**. Dodawanie kontrolki sieci Web przycisk nie Kontynuuj wszystkie produkty do `ItemTemplate` FormView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))

Gdy przycisk zostanie kliknięty przez użytkownika odwiedzającego stronę, następuje odświeżenie i [zdarzenie`ItemCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) FormView. Aby wykonać niestandardowy kod w odpowiedzi na kliknięty przycisk, można utworzyć procedurę obsługi zdarzeń dla tego zdarzenia. Należy zrozumieć, że zdarzenie `ItemCommand` wyzwalane za każdym razem, gdy *dowolny* przycisk, element LinkButton lub formant sieci Web kliknięto element ImageButton jest kliknięty w widoku FormView. Oznacza to, że gdy użytkownik przejdzie z jednej strony do drugiego w widoku FormView, wyzwalane jest zdarzenie `ItemCommand`. Ta sama czynność, gdy użytkownik kliknie pozycję Nowy, edytuj lub Usuń w widoku FormView, który obsługuje wstawianie, aktualizowanie i usuwanie.

Ponieważ `ItemCommand` wyzwalane niezależnie od tego, jaki przycisk jest kliknięty, w programie obsługi zdarzeń musimy określić, czy przycisk zaniechania wszystkich produktów został kliknięty, czy był jakiś inny przycisk. Aby to osiągnąć, można ustawić wartość właściwości `CommandName` formantu sieci Web dla przycisku. Gdy przycisk zostanie kliknięty, ta wartość `CommandName` jest przenoszona do programu obsługi zdarzeń `ItemCommand`, dzięki czemu można określić, czy przycisk zaniechania wszystkich produktów był kliknięty. Dla właściwości `CommandName` przycisku zaniech wszystkie produkty wybierz wartość DiscontinueProducts.

Na koniec Użyjmy okna dialogowego potwierdzenia po stronie klienta, aby upewnić się, że użytkownik naprawdę chce przestać korzystać z produktów wybranego dostawcy. Jak widać w temacie [Dodawanie potwierdzenia po stronie klienta podczas usuwania](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) samouczka, można to zrobić za pomocą bitu języka JavaScript. W szczególności ustaw właściwość OnClientClick kontrolki sieci Web przycisku na `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Po wprowadzeniu tych zmian, składnia deklaratywna FormView powinna wyglądać następująco:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemCommand` FormView. W tym obsłudze zdarzeń musimy najpierw określić, czy przycisk zaniechania wszystkich produktów został kliknięty. Jeśli tak, chcemy utworzyć wystąpienie klasy `ProductsBLL` i wywołać jej metodę `DiscontinueAllProductsForSupplier(supplierID)`, przekazując w `SupplierID` wybranego FormView:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Należy pamiętać, że `SupplierID` bieżącego wybranego dostawcy w widoku FormView można uzyskać przy użyciu [właściwości`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)FormView. Właściwość `SelectedValue` zwraca pierwszą wartość klucza danych dla rekordu wyświetlanego w widoku FormView. [Właściwość`DataKeyNames`](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx)FormView, która wskazuje pola danych, z których są pobierane wartości klucza danych, zostały automatycznie ustawione na `SupplierID` przez program Visual Studio w przypadku powiązania elementu ObjectDataSource z powrotem do FormView w kroku 2.

Po utworzeniu programu obsługi zdarzeń `ItemCommand` Poświęć chwilę na przetestowanie strony. Przejdź do dostawcy Cooperativa de Quesos "Las Cabras" (jest to piąty dostawca w widoku FormView dla mnie). Ten dostawca oferuje dwa produkty, Queso Cabrales i Queso Manchego La pastora, które *nie* zostały wycofane.

Załóżmy, że Cooperativa de Quesos "Las Cabras" został usunięty z firmy i w związku z tym jego produkty mają być wycofane. Kliknij przycisk zaniechanie wszystkich produktów. Spowoduje to wyświetlenie okna dialogowego potwierdzenia po stronie klienta (zobacz rysunek 16).

[![Cooperativa de Quesos Las Cabras oferuje dwa aktywne produkty](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**Rysunek 16**: Cooperativa de Quesos Las Cabras dostarcza dwa aktywne produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))

Jeśli klikniesz przycisk OK w oknie dialogowym potwierdzenia po stronie klienta, zostanie przesłane odpowiednie polecenie, co spowoduje odświeżenie zdarzenia `ItemCommand` FormView. Utworzona procedura obsługi zdarzeń zostanie następnie wykonana, wywołująca metodę `DiscontinueAllProductsForSupplier(supplierID)` i zaniechania zarówno Queso Cabrales, jak i Queso Manchego La pastora.

Jeśli użytkownik wyłączył stan widoku GridView, jest on ponownie powiązany z bazowym magazynem danych na każdym ogłaszaniu zwrotnym i w związku z tym natychmiast zostanie zaktualizowany w celu odzwierciedlenia, że te dwa produkty są teraz nieaktywne (Zobacz Rysunek 17). Jeśli jednak w widoku GridView nie wyłączono stanu widoku, należy ręcznie ponownie powiązać dane z widokiem GridView po wprowadzeniu tej zmiany. Aby to osiągnąć, po wywołaniu metody `DiscontinueAllProductsForSupplier(supplierID)` po prostu wywołaj metodę `DataBind()` GridView.

[![po kliknięciu przycisku nie Kontynuuj wszystkie produkty produkty dostawcy zostaną odpowiednio zaktualizowane.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Ilustracja 17**. po kliknięciu przycisku nie Kontynuuj wszystkie produkty produkty dostawcy są odpowiednio aktualizowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))

## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Krok 6. Tworzenie przeciążenia UpdateProduct w warstwie logiki biznesowej w celu dostosowania ceny produktu

Podobnie jak w przypadku przycisku Zastąp wszystkie produkty w widoku FormView, aby dodać przyciski do zwiększania i zmniejszania cen produktu w widoku GridView musimy najpierw dodać odpowiednią warstwę dostępu do danych i metody warstwy logiki biznesowej. Ponieważ mamy już metodę, która aktualizuje pojedynczy wiersz produktu w DAL, możemy zapewnić takie funkcje, tworząc nowe Przeciążenie dla metody `UpdateProduct` w LOGIKI biznesowej.

Nasze przeciążenia `UpdateProduct` przeszły w kilka kombinacji pól produktu jako wartości wejściowe skalarne, a następnie zaktualizowali tylko te pola dla określonego produktu. W przypadku tego przeciążenia różnimy się nieco od tego standardu, a zamiast tego przejdziemy do `ProductID` produktu i wartości procentowej, według której ma zostać dostosowany `UnitPrice` (w przeciwieństwie do przekazywania nowych, skorygowanych `UnitPrice` samego siebie). Takie podejście upraszcza kod, który musimy napisać w klasie z kodem ASP.NET strony, ponieważ nie musimy bother, aby określić bieżącą `UnitPrice`produktu.

Poniżej przedstawiono Przeciążenie `UpdateProduct` dla tego samouczka:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

To Przeciążenie pobiera informacje o określonym produkcie za pomocą metody `GetProductByProductID(productID)` DAL. Następnie sprawdza, czy `UnitPrice` produktu ma przypisaną `NULL` wartość bazy danych. Jeśli tak jest, Cena pozostaje niezmieniona. Jeśli jednak istnieje`NULL` wartość `UnitPrice`, Metoda aktualizuje `UnitPrice` produktu o określoną wartość procentową (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Krok 7. Dodawanie przycisków Zwiększ i Zmniejsz do widoku GridView

Elementy GridView (i DetailsView) składają się z kolekcji pól. Oprócz BoundFields, CheckBoxFields i używanie TemplateField, ASP.NET zawiera ButtonField, który, jak jego nazwa oznacza, jest renderowany jako kolumna z przyciskiem, element LinkButton lub kliknięto element ImageButton dla każdego wiersza. Podobnie jak w widoku FormView, kliknięcie *dowolnego* przycisku w obszarze przyciski stronicowania GridView, przyciski Edytuj lub Usuń, przyciski sortowania i tak dalej powodują odświeżenie i wygenerowanie [zdarzenia`RowCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)GridView.

ButtonField ma właściwość `CommandName`, która przypisuje określoną wartość do poszczególnych przycisków `CommandName` właściwości. Podobnie jak w przypadku widoku FormView wartość `CommandName` jest używana przez procedurę obsługi zdarzeń `RowCommand`, aby określić, który przycisk został kliknięty.

Dodajmy dwa nowe ButtonFields do widoku GridView, jeden z ceną tekstu przycisku + 10%, a drugą z ceną tekstu — 10%. Aby dodać te ButtonFields, kliknij link Edytuj kolumny w tagu inteligentnym GridView, wybierz typ pola ButtonField z listy w lewym górnym rogu, a następnie kliknij przycisk Dodaj.

![Dodaj dwa ButtonFields do widoku GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**Ilustracja 18**. Dodawanie dwóch ButtonFields do widoku GridView

Przenieś dwie ButtonFields tak, aby były wyświetlane jako pierwsze dwa pola GridView. Następnie ustaw właściwości `Text` tych dwóch ButtonFields na cenę + 10% i cena-10%, a właściwości `CommandName` odpowiednio na IncreasePrice i DecreasePrice. Domyślnie ButtonField renderuje swoją kolumnę przycisków jako LinkButtons. Można to zmienić, jednak za pomocą [właściwości`ButtonType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)ButtonField. Załóżmy, że te dwie ButtonFields są renderowane jako zwykłe przyciski push; w związku z tym ustaw właściwość `ButtonType` na `Button`. Rysunek 19 pokazuje okno dialogowe pola po wprowadzeniu tych zmian; Poniżej znajduje się znacznik deklaratywny GridView.

![Konfigurowanie właściwości ButtonFields, CommandName i ButtonType](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**Ilustracja 19**. konfigurowanie właściwości `Text`, `CommandName`i `ButtonType` ButtonFields

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

Po utworzeniu tych ButtonFields ostatnim krokiem jest utworzenie programu obsługi zdarzeń dla zdarzenia `RowCommand` GridView. Ta procedura obsługi zdarzeń, jeśli została uruchomiona, ponieważ kliknięto przyciski Price + 10% lub Price-10%, muszą określić `ProductID` dla wiersza, którego przycisk został kliknięty, a następnie wywołać metodę `UpdateProduct` klasy `ProductsBLL`, przekazując odpowiednie `UnitPrice` dopasowanie procentowe wraz z `ProductID`. Poniższy kod wykonuje następujące zadania:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Aby określić `ProductID` dla wiersza, którego kliknięto przycisk cena + 10% lub cena — 10%, należy zapoznać się z kolekcją `DataKeys` GridView. Ta kolekcja zawiera wartości pól określonych we właściwości `DataKeyNames` dla każdego wiersza GridView. Ponieważ właściwość `DataKeyNames` GridView została ustawiona na ProductID przez program Visual Studio podczas wiązania elementu ObjectDataSource z GridView, `DataKeys(rowIndex).Value` udostępnia `ProductID` dla określonego *RowIndex*.

ButtonField jest automatycznie przekazywana w *RowIndex* wiersza, którego przycisk został kliknięty za pomocą parametru `e.CommandArgument`. W związku z tym, aby określić `ProductID` dla wiersza, którego kliknięto przycisk cena + 10% lub cena — 10%, użyjemy opcji: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Podobnie jak w przypadku przycisku Zakończ wszystkie produkty, jeśli stan widoku GridView został wyłączony, element GridView jest przełączany do podstawowego magazynu danych na każdym ogłaszaniu zwrotnym i w związku z tym natychmiast zostanie zaktualizowany w celu odzwierciedlenia zmiany ceny, która wynika z kliknięcia jeden z przycisków. Jeśli jednak w widoku GridView nie wyłączono stanu widoku, należy ręcznie ponownie powiązać dane z widokiem GridView po wprowadzeniu tej zmiany. Aby to osiągnąć, po wywołaniu metody `UpdateProduct` po prostu wywołaj metodę `DataBind()` GridView.

Ilustracja 20 przedstawia stronę podczas wyświetlania produktów dostarczonych przez Homestead Grandma Kelly. Rysunek 21 pokazuje wyniki po dwukrotnym kliknięciu przycisku cena + 10% w przypadku rozprzestrzenienia Boysenberry Grandma oraz przycisku cena-10% raz dla Northwoods Cranberry.

[![GridView obejmuje cenę + 10% i cenę — 10% przycisków](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Ilustracja 20**. widok GridView obejmuje cenę + 10% i cenę — 10% przycisków ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))

[![ceny pierwszego i trzeciego produktu zostały zaktualizowane za pomocą przycisków cena + 10% i cena — 10%](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Ilustracja 21**. ceny pierwszego i trzeciego produktu zostały zaktualizowane za pomocą przycisków cena + 10% i cena — 10% ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))

> [!NOTE]
> W widoku GridView (i DetailsView) mogą być również przypisane przyciski, LinkButtons lub ImageButtons do ich używanie TemplateField. Podobnie jak w przypadku BoundField, te przyciski, gdy kliknięto, spowodują wypadek odświeżenia i wygenerowanie zdarzenia `RowCommand` GridView. Podczas dodawania przycisków w TemplateField, jednak `CommandArgument` przycisku nie jest automatycznie ustawiany na indeks wiersza, gdy jest używany ButtonFields. Jeśli musisz określić indeks wiersza przycisku, który został kliknięty w ramach procedury obsługi zdarzeń `RowCommand`, musisz ręcznie ustawić właściwość `CommandArgument` przycisku w swojej składni deklaracyjnej w TemplateField, używając kodu takiego jak:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`.

## <a name="summary"></a>Podsumowanie

Wszystkie kontrolki GridView, DetailsView i FormView mogą zawierać przyciski, LinkButtons lub ImageButtons. Takie przyciski po kliknięciu powodują wystąpienie zwrotne i wywołają zdarzenie `ItemCommand` w kontrolkach FormView i DetailsView oraz zdarzenia `RowCommand` w widoku GridView. Te kontrolki sieci Web danych mają wbudowaną funkcję obsługi typowych akcji związanych z poleceniami, takich jak usuwanie i edytowanie rekordów. Można jednak również użyć przycisków, które po kliknięciu reagują na wykonywanie własnego niestandardowego kodu.

Aby to osiągnąć, należy utworzyć procedurę obsługi zdarzeń dla zdarzenia `ItemCommand` lub `RowCommand`. W tym obsłudze zdarzeń najpierw sprawdzimy wartość przychodzącą `CommandName`, aby określić, który przycisk został kliknięty, a następnie podejmij odpowiednią akcję niestandardową. W tym samouczku przedstawiono sposób użycia przycisków i ButtonFields w celu zaniechania wszystkich produktów dla określonego dostawcy lub zwiększenia lub zmniejszenia ceny określonego produktu o 10%.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Dalej](adding-and-responding-to-buttons-to-a-gridview-vb.md)
