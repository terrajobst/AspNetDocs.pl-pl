---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Wykonywanie aktualizacji wsadowych (VB) | Microsoft Docs
author: rick-anderson
description: Dowiedz się, jak utworzyć w pełni edytowalną listę DataList, w której wszystkie elementy znajdują się w trybie edycji, których wartości można zapisać, klikając przycisk "Aktualizuj wszystko" w...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: e54c9fc12da278492b54164cf657eea142a3dae6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78610699"
---
# <a name="performing-batch-updates-vb"></a>Wykonywanie aktualizacji wsadowych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) lub [Pobierz plik PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Dowiedz się, jak utworzyć w pełni edytowalną stronę typu DataList, w której wszystkie elementy są w trybie edycji, których wartości można zapisać, klikając przycisk "Aktualizuj wszystko" na stronie.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) sprawdziłmy, jak utworzyć element DataList na poziomie elementu. Podobnie jak w przypadku standardowego edytowalnego widoku GridView każdy element w elemencie DataList zawiera przycisk edycji, który po kliknięciu sprawia, że element będzie można edytować. Chociaż ta edycja na poziomie elementu działa prawidłowo w przypadku danych, które są aktualizowane tylko sporadycznie, niektóre scenariusze przypadków użycia wymagają od użytkownika edytowania wielu rekordów. Jeśli użytkownik musi edytować dziesiątki rekordów i musi kliknąć pozycję Edytuj, wprowadzić zmiany, a następnie kliknąć przycisk Aktualizuj dla każdej z nich, ilość klikniętych elementów może zakłócać swoją produktywność. W takich sytuacjach lepszym rozwiązaniem jest udostępnienie w pełni edytowalnego elementu DataList, jednego, gdzie *wszystkie* jego elementy znajdują się w trybie edycji. których wartości można edytować, klikając przycisk Aktualizuj wszystko na stronie (patrz rysunek 1).

[![każdy element w w pełni edytowalnej DataList można zmodyfikować](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Rysunek 1**: każdy element w w pełni edytowalnej DataList można zmodyfikować ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-vb/_static/image3.png))

W tym samouczku dowiesz się, jak umożliwić użytkownikom aktualizowanie informacji o adresie dostawców przy użyciu w pełni edytowalnej elementu DataList.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Krok 1. Tworzenie edytowalnego interfejsu użytkownika w ItemTemplate DataList

W poprzednim samouczku, w którym tworzymy standardową, edytowalną na poziomie elementu element DataList, użyto dwóch szablonów:

- `ItemTemplate` zawiera interfejs użytkownika tylko do odczytu (kontrolki sieci Web etykiety do wyświetlania każdej nazwy i ceny produktu).
- `EditItemTemplate` zawiera interfejs użytkownika trybu edycji (dwie kontrolki sieci Web w postaci pola tekstowego).

Właściwość DataList s `EditItemIndex` określa, jakie `DataListItem` (jeśli istnieją) są renderowane przy użyciu `EditItemTemplate`. W szczególności `DataListItem`, których `ItemIndex` wartość zgodna z właściwością DataList s `EditItemIndex` jest renderowany przy użyciu `EditItemTemplate`. Ten model działa dobrze, gdy można edytować tylko jeden element jednocześnie, ale przy tworzeniu w pełni edytowalnego elementu DataList.

W przypadku w pełni edytowalnej wartości DataList chcemy, aby *wszystkie* `DataListItem` s były renderowane przy użyciu interfejsu edytowalnego. Najprostszym sposobem osiągnięcia tego celu jest zdefiniowanie interfejsu edytowalnego w `ItemTemplate`. W przypadku modyfikowania informacji o adresie dostawcy, edytowalny interfejs zawiera nazwę dostawcy jako tekst, a następnie pola tekstowe dla wartości adres, miasto i kraj.

Aby rozpocząć, Otwórz stronę `BatchUpdate.aspx`, Dodaj kontrolkę DataList i ustaw jej Właściwość `ID` na `Suppliers`. W tagu inteligentnym DataList s Dodaj nową kontrolkę ObjectDataSource o nazwie `SuppliersDataSource`.

[![utworzyć nowego elementu ObjectDataSource o nazwie SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Rysunek 2**. Utwórz nowy element ObjectDataSource o nazwie `SuppliersDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](performing-batch-updates-vb/_static/image6.png))

Skonfiguruj element ObjectDataSource do pobierania danych przy użyciu metody `GetSuppliers()` `SuppliersBLL` Class (patrz rysunek 3). Podobnie jak w przypadku poprzedniego samouczka, zamiast aktualizowania informacji o dostawcach za pomocą elementu ObjectDataSource, będziemy współpracować bezpośrednio z warstwą logiki biznesowej. W związku z tym Ustaw listę rozwijaną na wartość (brak) na karcie aktualizacja (zobacz rysunek 4).

[![pobrać informacji o dostawcy za pomocą metody getdostawcs ()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Rysunek 3**. Pobieranie informacji o dostawcy za pomocą metody `GetSuppliers()` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](performing-batch-updates-vb/_static/image9.png))

[![ustawić dla listy rozwijanej wartość (brak) na karcie aktualizacja.](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Rysunek 4**. Ustaw listę rozwijaną na (brak) na karcie aktualizacja ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](performing-batch-updates-vb/_static/image12.png))

Po zakończeniu działania kreatora program Visual Studio automatycznie generuje `ItemTemplate` DataList s, aby wyświetlić każde pole danych zwrócone przez źródło danych w kontrolce sieci Web etykieta. Musimy zmodyfikować ten szablon w taki sposób, aby zamiast niego udostępnił interfejs edycji. `ItemTemplate` można dostosować za pomocą projektanta przy użyciu opcji Edytuj szablony z taga inteligentnego DataList s lub bezpośrednio za pomocą składni deklaracyjnej.

Poświęć chwilę na utworzenie interfejsu edycji, który wyświetla nazwę dostawcy jako tekst, ale zawiera pola tekstowe dla adresu dostawcy, miasta i wartości kraju. Po wprowadzeniu tych zmian składnia deklaracyjnej strony powinna wyglądać podobnie do następującej:

[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Tak jak w poprzednim samouczku, DataList w tym samouczku musi mieć włączony stan widoku.

W `ItemTemplate` I m przy użyciu dwóch nowych klas CSS, `SupplierPropertyLabel` i `SupplierPropertyValue`, które zostały dodane do klasy `Styles.css` i zostały skonfigurowane tak, aby korzystały z tych samych ustawień stylu co `ProductPropertyLabel` i `ProductPropertyValue` klas CSS.

[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Po wprowadzeniu tych zmian odwiedź tę stronę za pomocą przeglądarki. Jak pokazano na rysunku 5, każdy element DataList wyświetla nazwę dostawcy jako tekst i używa pól tekstowych do wyświetlania adresu, miasta i kraju.

[![każdy dostawca w elemencie DataList jest edytowalny](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Rysunek 5**. można edytować każdego dostawcę w elemencie DataList ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](performing-batch-updates-vb/_static/image15.png))

## <a name="step-2-adding-an-update-all-button"></a>Krok 2. Dodawanie przycisku Aktualizuj wszystko

Każdy dostawca na rysunku 5 ma pola adres, miasto i kraj wyświetlane w polu tekstowym, a obecnie nie ma dostępnego przycisku Aktualizuj. Zamiast przycisku Aktualizuj dla każdego elementu z pełnymi edytowalnymi listami danych na stronie jest zwykle pojedynczy przycisk Aktualizuj wszystko, który po kliknięciu aktualizuje *wszystkie* rekordy w elemencie DataList. Na potrzeby tego samouczka Dodaj dwa przyciski aktualizacji wszystkie — jeden w górnej części strony i jeden u dołu (mimo że kliknięcie jednego z nich będzie miało ten sam efekt).

Zacznij od dodania kontrolki sieci Web przycisku powyżej elementu DataList i ustaw jej Właściwość `ID` na `UpdateAll1`. Następnie Dodaj drugi kontrolkę sieci Web przycisku poniżej elementu DataList, ustawiając jej `ID` na `UpdateAll2`. Ustaw właściwości `Text` dla dwóch przycisków, aby zaktualizować wszystkie. Na koniec Utwórz programy obsługi zdarzeń dla obu przycisków `Click` zdarzenia. Zamiast duplikowania logiki aktualizacji w każdym z programów obsługi zdarzeń, należy powiadomić element, który jest logiką do trzeciej metody, `UpdateAllSupplierAddresses`, że programy obsługi zdarzeń po prostu wywołujeją tę trzecią metodę.

[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Rysunek 6 przedstawia stronę po dodaniu przycisków Aktualizuj wszystkie.

[do strony dodano ![dwie przyciski aktualizacji](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Ilustracja 6**. do strony dodano dwa przyciski aktualizacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](performing-batch-updates-vb/_static/image18.png))

## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Krok 3. aktualizowanie wszystkich informacji o adresie dostawcy

Wszystkie elementy DataList są wyświetlane w interfejsie edycji i z dodaniem przycisków Aktualizuj wszystkie, które pozostaną, pisząc kod do wykonania aktualizacji wsadowej. W odniesieniu do każdej z nich musimy przepętlać przez elementy DataList i wywoływać metodę `SuppliersBLL` klasy s `UpdateSupplierAddress` dla każdej z nich.

Do kolekcji wystąpień `DataListItem`, które korzeńą Właściwość DataList, można uzyskać dostęp za pośrednictwem właściwości DataList s [`Items`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). W odniesieniu do `DataListItem`można pobrać odpowiednie `SupplierID` z kolekcji `DataKeys` i programowo odwoływać się do kontrolek sieci Web typu TextBox w `ItemTemplate`, jak ilustruje poniższy kod:

[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Gdy użytkownik kliknie jeden z przycisków Aktualizuj wszystkie, Metoda `UpdateAllSupplierAddresses` iteruje przez poszczególne `DataListItem` w `Suppliers` DataList i wywołuje metodę `SuppliersBLL` klasy s `UpdateSupplierAddress`, przekazując odpowiednie wartości. Wartość, która nie została wprowadzona dla przechodzenia adresu, miasta lub kraju, jest wartością `Nothing` do `UpdateSupplierAddress` (zamiast pustego ciągu), co powoduje `NULL` bazy danych dla źródłowych pól rekordów.

> [!NOTE]
> W ramach ulepszeń można dodać kontrolkę sieci Web etykieta stanu do strony, która zawiera komunikat z potwierdzeniem po wykonaniu aktualizacji wsadowej.

## <a name="updating-only-those-addresses-that-have-been-modified"></a>Aktualizowanie tylko tych adresów, które zostały zmodyfikowane

Algorytm aktualizacji wsadowych używany w tym samouczku wywołuje metodę `UpdateSupplierAddress` dla *każdego* dostawcy w elemencie DataList, niezależnie od tego, czy informacje o adresie zostały zmienione. Chociaż takie aktualizacje niewidome nie są zwykle przyczyną problemów z wydajnością, mogą powodować zbędne rekordy w przypadku ponownego inspekcji zmian w tabeli bazy danych. Na przykład, jeśli używasz wyzwalaczy do rejestrowania wszystkich `UPDATE` s w tabeli `Suppliers` do tabeli inspekcji, za każdym razem, gdy użytkownik kliknie przycisk Aktualizuj wszystko, zostanie utworzony nowy rekord inspekcji dla każdego dostawcy w systemie, niezależnie od tego, czy użytkownik wprowadził jakiekolwiek zmiany.

Klasy ADO.NET DataTable i DataAdapter są przeznaczone do obsługi aktualizacji wsadowych, gdy tylko zmodyfikowane, usunięte i nowe rekordy są wynikiem komunikacji z bazą danych. Każdy wiersz w elemencie DataTable ma [właściwość`RowState`](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) , która wskazuje, czy wiersz został dodany do elementu DataTable, usunięty z niego, zmodyfikowany lub pozostaje niezmieniony. Gdy element DataTable jest wypełniany początkowo, wszystkie wiersze są oznaczone jako niezmienione. Zmiana wartości któregokolwiek z kolumn wiersza s oznacza, że wiersz został zmodyfikowany.

W `SuppliersBLL` klasie aktualizujemy informacje o określonym adresie dostawcy, najpierw odczytując w rekordzie pojedynczego dostawcy do `SuppliersDataTable`, a następnie ustaw wartości kolumn `Address`, `City`i `Country` przy użyciu następującego kodu:

[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Ten kod naively przypisuje wartości zatwierdzono adres, miasto i kraj do `SuppliersRow` w `SuppliersDataTable` niezależnie od tego, czy wartości zostały zmienione. Te modyfikacje powodują, że właściwość `SuppliersRow` s `RowState` zostanie oznaczona jako modyfikowana. Gdy wywoływana jest metoda `Update` dostępu do danych, zauważa, że `SupplierRow` została zmodyfikowana i w związku z tym wysyła polecenie `UPDATE` do bazy danych.

Wyobraź sobie, że dodaliśmy kod do tej metody, aby przypisać tylko wartości w polu adres, miasto i kraj, jeśli różnią się one od istniejących wartości `SuppliersRow` s. W przypadku, gdy adres, miasto i kraj są takie same jak istniejące dane, nie zostaną wprowadzone żadne zmiany, a `SupplierRow` s `RowState` zostaną oznaczone jako niezmienione. Wynikiem jest fakt, że po wywołaniu metody DAL s `Update` nie zostanie wykonane żadne wywołanie bazy danych, ponieważ `SuppliersRow` nie została zmodyfikowana.

Aby przyjąć tę zmianę, Zastąp instrukcje, które nie przypisują wartości adresu zatwierdzono, miasto i kraj z następującym kodem:

[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

W tym dodatkowym kodzie Metoda DAL s `Update` wysyła instrukcję `UPDATE` do bazy danych tylko dla tych rekordów, których wartości związane z adresami uległy zmianie.

Alternatywnie możemy śledzić, czy istnieją różnice między polami adresów zatwierdzono i danymi bazy danych i, jeśli nie ma żadnych, należy po prostu pominąć wywołanie metody DAL s `Update`. Ta metoda działa prawidłowo w przypadku ponownego użycia metody DB Direct, ponieważ metoda DB Direct jest t przekazała wystąpienie `SuppliersRow`, którego `RowState` można sprawdzić w celu ustalenia, czy wywołanie bazy danych jest rzeczywiście potrzebne.

> [!NOTE]
> Za każdym razem, gdy wywoływana jest metoda `UpdateSupplierAddress`, nastąpi wywołanie do bazy danych w celu pobrania informacji o zaktualizowanym rekordzie. Następnie, jeśli istnieją jakiekolwiek zmiany w danych, zostanie wykonane inne wywołanie do bazy danych w celu zaktualizowania wiersza tabeli. Ten przepływ pracy można zoptymalizować przez utworzenie przeciążenia metody `UpdateSupplierAddress`, które akceptuje wystąpienie `EmployeesDataTable`, które zawiera *wszystkie* zmiany ze strony `BatchUpdate.aspx`. Następnie może nawiązać połączenie z bazą danych, aby pobrać wszystkie rekordy z tabeli `Suppliers`. Następnie można wyliczyć te dwa zestawy wyników i można zaktualizować tylko te rekordy, w których wystąpiły zmiany.

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazano, jak utworzyć w pełni edytowalną element DataList, umożliwiając użytkownikowi szybkie modyfikowanie informacji o adresie dla wielu dostawców. Rozpoczęto od zdefiniowania interfejsu edycji kontrolki sieci Web TextBox dla wartości adres dostawcy, miasto i kraj w `ItemTemplate`DataList. Następnie dodaliśmy opcję Aktualizuj wszystkie przyciski powyżej i poniżej elementu DataList. Po dokonaniu zmian przez użytkownika i kliknięciu jednego z przycisków aktualizacji wszystkie `DataListItem` są wyliczane i zostanie wykonane wywołanie metody `UpdateSupplierAddress` `SuppliersBLL` Class s.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Zack Kowalski i Krzysztof Pespisa. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [dalej](handling-bll-and-dal-level-exceptions-vb.md)
