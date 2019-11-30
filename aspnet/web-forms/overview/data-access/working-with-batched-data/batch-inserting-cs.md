---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: Wstawianie wsadoweC#() | Microsoft Docs
author: rick-anderson
description: Dowiedz się, jak wstawiać wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika rozszerzono widok GridView, aby umożliwić użytkownikowi wprowadzanie wielu n...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 5dc4d0b6ac9bf3aa2baa54fe9f5d4149494e47d2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74584758"
---
# <a name="batch-inserting-c"></a>Wstawianie w partiach (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) lub [Pobierz plik PDF](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Dowiedz się, jak wstawiać wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika rozszerzono widok GridView, aby umożliwić użytkownikowi wprowadzanie wielu nowych rekordów. W warstwie dostępu do danych zawijamy wiele operacji wstawiania w ramach transakcji, aby upewnić się, że wszystkie operacje wstawiania zakończyły się powodzeniem lub wszystkie wstawienia są wycofywane.

## <a name="introduction"></a>Wprowadzenie

W samouczku [aktualizacji wsadowych](batch-updating-cs.md) sprawdziłem, jak dostosowujesz formant GridView, aby przedstawić interfejs, w którym można było edytować wiele rekordów. Użytkownik odwiedzający stronę może wprowadzić serię zmian, a następnie po kliknięciu jednego przycisku wykonać aktualizację zbiorczą. W przypadku sytuacji, w których użytkownicy często aktualizują wiele rekordów w jednym miejscu, taki interfejs może zapisywać niezliczone kliknięć i przełączanie kontekstu klawiatury do myszy w porównaniu z domyślnymi funkcjami edycji poszczególnych wierszy, które zostały po raz pierwszy omówione w temacie [Omówienie wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) .

To pojęcie można również zastosować w przypadku dodawania rekordów. Załóżmy, że w tym miejscu w firmie Northwind Traders często odbieramy wysyłki od dostawców zawierających kilka produktów dla określonej kategorii. Załóżmy na przykład, że firma Microsoft może otrzymać wydanie sześć różnych produktów herbaty i kawy od handlowców z Warszawy. Jeśli użytkownik wprowadzi sześć produktów po raz za pośrednictwem kontrolki DetailsView, będzie musiał wybrać wiele z tych samych wartości jednocześnie i ponownie: będzie to wymagało wybrania tej samej kategorii (napoje), tego samego dostawcy (dla Warszawy), tej samej wartości zaniechanej ( False) i te same jednostki na wartości kolejności (0). Ten powtarzający się wpis danych nie tylko czasochłonne, ale jest podatny na błędy.

W przypadku niewielkiej pracy możemy utworzyć interfejs wstawiania wsadowego, który umożliwia użytkownikowi wybranie dostawcy i kategorii Raz, wprowadzić serię nazw produktów i ceny jednostkowe, a następnie kliknąć przycisk, aby dodać nowe produkty do bazy danych (patrz rysunek 1). Po dodaniu każdego produktu do pól danych `ProductName` i `UnitPrice` są przypisywane wartości wprowadzone w polach tekstowych, podczas gdy `CategoryID` i `SupplierID` wartości są przypisywane wartości z kontrolek DropDownList w górnej części formularza. Wartości `Discontinued` i `UnitsOnOrder` są ustawiane jako zakodowane odpowiednio wartości `false` i 0.

[![Wstawianie interfejsu wsadowego](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Rysunek 1**: wsadowe Wstawianie interfejsu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image3.png))

W tym samouczku utworzymy stronę, która implementuje interfejs wstawiania wsadowego przedstawiony na rysunku 1. Podobnie jak w przypadku poprzednich dwóch samouczków, umieścimy wstawienia w zakresie transakcji, aby zapewnić niepodzielność. Zacznij korzystać z aplikacji.

## <a name="step-1-creating-the-display-interface"></a>Krok 1. Tworzenie interfejsu wyświetlania

Ten samouczek będzie składać się z jednej strony, która jest podzielona na dwa regiony: region wyświetlania i region wstawiania. Interfejs wyświetlania, który zostanie utworzony w tym kroku, pokazuje produkty w widoku GridView i zawiera przycisk zatytułowany proces dostawy produktu. Po kliknięciu tego przycisku interfejs wyświetlania jest zastępowany interfejsem wstawiania, który jest przedstawiony na rysunku 1. Interfejs ekranu zwraca po kliknięciu przycisku Dodaj produkty z wysyłki lub Anuluj. Utworzymy interfejs wstawiania w kroku 2.

Podczas tworzenia strony z dwoma interfejsami tylko jeden z nich jest widoczny w danym momencie, każdy interfejs jest zwykle umieszczany w [kontrolce sieci Web panelu](http://www.w3schools.com/aspnet/control_panel.asp), który służy jako kontener dla innych formantów. W związku z tym nasza strona ma dwa kontrolki panel dla każdego interfejsu.

Zacznij od otworzenia strony `BatchInsert.aspx` w folderze `BatchData` i przeciągnij panel z przybornika do projektanta (patrz rysunek 2). Ustaw właściwość `ID` panelu s na `DisplayInterface`. Podczas dodawania panelu do projektanta, jego właściwości `Height` i `Width` są odpowiednio ustawione na 50px i 125px. Wyczyść te wartości właściwości z okno Właściwości.

[![przeciąganie panelu z przybornika do projektanta](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Rysunek 2**. przeciągnięcie panelu z przybornika do projektanta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-inserting-cs/_static/image6.png))

Następnie przeciągnij przycisk i formant GridView do panelu. Ustaw właściwość Button `ID` na `ProcessShipment` i jej Właściwość `Text`, aby przetwarzać wysyłkę produktu. Ustaw właściwość GridView s `ID` na `ProductsGrid` i, z jej tagu inteligentnego, powiąż ją z nowym elementem ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj element ObjectDataSource, aby ściągał swoje dane z metody `GetProducts` `ProductsBLL` Class. Ponieważ ten element GridView jest używany tylko do wyświetlania danych, Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak). Kliknij przycisk Zakończ, aby zakończyć pracę Kreatora konfiguracji źródła danych.

[![wyświetlić dane zwrócone przez metodę getProducts klasy ProductsBLL](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Rysunek 3**. Wyświetlanie danych zwróconych z metody `GetProducts` `ProductsBLL` Class ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image9.png))

[![ustawić listy rozwijane na kartach Aktualizuj, Wstaw i Usuń do (brak).](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Rysunek 4**. Ustawianie list rozwijanych w kartach Aktualizuj, Wstaw i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-inserting-cs/_static/image12.png))

Po zakończeniu działania kreatora ObjectDataSource Visual Studio doda BoundFields i CheckBoxField dla pól danych produktu. Usuń wszystkie pola oprócz `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`i `Discontinued`. Korzystaj z bezpłatnych dostosowań. Postanowiono sformatować pole `UnitPrice` jako wartość walutową, zmienić kolejność pól i zmienić nazwy kilku pól `HeaderText` wartości. Skonfiguruj również widok GridView, aby obejmował obsługę stronicowania i sortowania, zaznaczając pola wyboru Włącz stronicowanie i Włącz sortowanie w tagu inteligentnym GridView.

Po dodaniu kontrolek panel, Button, GridView i ObjectDataSource i dostosowaniu pól GridView, znaczniki deklaratywne strony powinny wyglądać podobnie do poniższego:

[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

Należy zauważyć, że znaczniki dla przycisku i widoku GridView pojawiają się w tagach otwierających i zamykających `<asp:Panel>`. Ponieważ te kontrolki znajdują się w panelu `DisplayInterface`, można je ukryć przez ustawienie właściwości `Visible` panelu s na `false`. Krok 3 sprawdza programowo zmianę właściwości `Visible` panelu i w odpowiedzi na przycisk kliknij, aby wyświetlić jeden interfejs podczas ukrywania drugiego.

Poświęć chwilę na wyświetlenie postępu w przeglądarce. Jak pokazano na rysunku 5, powinien zostać wyświetlony przycisk przetwórz dostawy produktu powyżej widoku GridView, który wyświetla dziesięć produktów w danym momencie.

[![GridView wyświetla listę produktów i oferuje funkcje sortowania i stronicowania](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Ilustracja 5**. element GridView wyświetla listę produktów i oferuje funkcje sortowania i stronicowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-inserting-cs/_static/image15.png))

## <a name="step-2-creating-the-inserting-interface"></a>Krok 2. Tworzenie interfejsu wstawiania

Po zakończeniu interfejsu wyświetlania, gotowy do utworzenia interfejsu wstawiania. Na potrzeby tego samouczka Utwórz interfejs wstawiania, który będzie monitował o pojedynczy dostawca i wartość kategorii, a następnie umożliwi użytkownikowi wprowadzenie do pięciu nazw produktów i wartości cen jednostkowych. Za pomocą tego interfejsu użytkownik może dodać jeden do pięciu nowych produktów, które współużytkują tę samą kategorię i dostawcę, ale mają unikatowe nazwy produktów i ceny.

Zacznij od przeciągnięcia panelu z przybornika do projektanta, umieszczając go pod istniejącym panelem `DisplayInterface`. Ustaw właściwość `ID` tego nowo dodany panel, aby `InsertingInterface` i ustawić jej Właściwość `Visible` na `false`. Dodamy kod, który ustawia właściwość `Visible` panelu `InsertingInterface` na `true` w kroku 3. Wyczyść również wartości właściwości Panel s `Height` i `Width`.

Następnie musimy utworzyć interfejs wstawiania, który został pokazany na rysunku 1. Ten interfejs można utworzyć za pomocą różnych technik HTML, ale będziemy używać dość prostego: tabeli zawierającej cztery kolumny, siedem wierszy.

> [!NOTE]
> Gdy wprowadzasz znaczniki dla elementów `<table>` HTML, wolisz korzystać z widoku źródła. Mimo że program Visual Studio ma narzędzia do dodawania `<table>` elementów za pomocą projektanta, Projektant wydaje się wszystko zbyt skłonne, aby wstrzyknąć nieproszony `style` ustawień do znaczników. Po utworzeniu znacznika `<table>`, zazwyczaj wracam do projektanta, aby dodać kontrolki sieci Web i ustawić ich właściwości. Podczas tworzenia tabel z wstępnie określonymi kolumnami i preferowanymi wierszami przy użyciu statycznego kodu HTML zamiast [kontrolki sieci Web tabeli](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) , ponieważ dowolnych kontrolek sieci Web umieszczonych w formancie sieci Web tabeli można uzyskać dostęp tylko przy użyciu wzorca `FindControl("controlID")`. Należy jednak użyć formantów sieci Web tabeli do dynamicznego rozmiaru tabel (dla których wiersze lub kolumny są oparte na pewnych kryteriach określonych przez użytkownika), ponieważ kontrolka sieci Web tabeli może być skonstruowana programowo.

Wprowadź następujące znaczniki w `<asp:Panel>` tagów `InsertingInterface` panelu:

[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

Ten `<table>` znacznik nie zawiera jeszcze żadnych kontrolek sieci Web, dodamy je na chwilę. Należy zauważyć, że każdy element `<tr>` zawiera określone ustawienie klasy CSS: `BatchInsertHeaderRow` wiersza nagłówka, w którym zostanie wykontrolek DropDownList dostawca i Kategoria. `BatchInsertFooterRow` wiersza stopki, w którym zostaną umieszczone przyciski Dodaj produkty z wysyłki i Anuluj; i zmienne `BatchInsertRow` i `BatchInsertAlternatingRow` dla wierszy, które będą zawierać kontrolki TextBox produktu i ceny jednostkowej. W pliku `Styles.css` zostały utworzone odpowiednie klasy CSS, aby zapewnić Wstawianie interfejsu podobnego do kontrolek GridView i DetailsView, które są używane w tym samouczku. Te klasy CSS przedstawiono poniżej.

[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Po wprowadzeniu tej oznaczeń Wróć do widok Projekt. Ta `<table>` powinna być wyświetlana jako tabela z czterema wierszami, siedem wierszy w projektancie, jak pokazano na rysunku 6.

[![Wstawianie interfejsu składa się z czterech kolumn, siedmiu wierszy tabeli](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Rysunek 6**. Wstawianie interfejsu składa się z czterech kolumn, siedmiu wierszy tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image18.png))

Teraz wszystko jest gotowe do dodania formantów sieci Web do interfejsu wstawiania. Przeciągnij dwie kontrolek DropDownList z przybornika do odpowiednich komórek w tabeli, a drugi dla dostawcy i dla kategorii.

Ustaw właściwość DropDownList dostawcy `ID` na `Suppliers` i powiąż ją z nowym elementem ObjectDataSource o nazwie `SuppliersDataSource`. Skonfiguruj nowy element ObjectDataSource do pobrania danych z metody `SuppliersBLL` Class `GetSuppliers` i Ustaw listę rozwijaną karta Aktualizuj na (brak). Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![skonfigurować element ObjectDataSource do używania metody getdostawca klasy SuppliersBLL](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Rysunek 7**. Konfigurowanie elementu ObjectDataSource do używania metody `GetSuppliers` `SuppliersBLL` klasy s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image21.png))

`Suppliers` DropDownList wyświetlić pola dane `CompanyName` i użyj pola dane `SupplierID` jako wartości `ListItem` s.

[![wyświetlić pola danych NazwaFirmy i użyć IDDostawcy jako wartości](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Ilustracja 8**. Wyświetl `CompanyName` pole danych i użyj `SupplierID` jako wartości (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image24.png))

Nazwij drugi DropDownList `Categories` i powiąż go z nowym elementem ObjectDataSource o nazwie `CategoriesDataSource`. Skonfiguruj `CategoriesDataSource` element ObjectDataSource, aby używał metody `GetCategories` klasy `CategoriesBLL`. Ustaw listę rozwijaną na kartach Aktualizuj i Usuń na (brak), a następnie kliknij przycisk Zakończ, aby zakończyć pracę kreatora. Na koniec DropDownList wyświetlić pole danych `CategoryName` i użyj `CategoryID` jako wartości.

Po dodaniu tych dwóch kontrolek DropDownList i powiązaniu odpowiednich skonfigurowanych ObjectDataSourceów ekran powinien wyglądać podobnie do rysunku 9.

[![wiersz nagłówka zawiera teraz dostawców i kategorie kontrolek DropDownList](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Ilustracja 9**: wiersz nagłówka zawiera teraz `Suppliers` i `Categories` kontrolek DropDownList ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-inserting-cs/_static/image27.png))

Teraz należy utworzyć pola tekstowe, aby zebrać nazwę i cenę każdego nowego produktu. Przeciągnij kontrolkę TextBox z przybornika na projektanta dla każdej z pięciu wierszy Product Name i Price. Ustaw właściwości `ID` pól tekstowych na `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`itd.

Dodaj CompareValidator po każdej z pól tekstowych cen jednostkowych, ustawiając właściwość `ControlToValidate` na odpowiednie `ID`. Ustaw również właściwość `Operator` na `GreaterThanEqual`, `ValueToCompare` na 0, a `Type` na `Currency`. Te ustawienia instruują CompareValidator, aby upewnić się, że cena, jeśli została wprowadzona, jest prawidłową wartością waluty, która jest większa lub równa zero. Ustaw właściwość `Text` na \*, a `ErrorMessage` Cena musi być większa lub równa zero. Ponadto Pomiń wszystkie symbole walut.

> [!NOTE]
> Interfejs wstawiania nie zawiera żadnych kontrolek RequiredFieldValidator, mimo że pole `ProductName` w tabeli `Products` Database nie zezwala na wartości `NULL`. Jest to spowodowane tym, że chcemy pozwolić, aby użytkownik wprowadził do pięciu produktów. Na przykład, jeśli użytkownik musiał podać nazwę produktu i cenę jednostkową dla pierwszych trzech wierszy, pozostawiając ostatnie dwa wiersze puste, firma Microsoft dodaje trzy nowe produkty do systemu. Ponieważ jest wymagane `ProductName`, należy przeprowadzić programowo kontrolę, aby upewnić się, że w przypadku wprowadzenia ceny jednostkowej podanej wartości nazwy produktu. Zajmiemy się tym sprawdzaniem w kroku 4.

Podczas sprawdzania poprawności danych wejściowych użytkownika CompareValidator raportuje nieprawidłowe dane, jeśli wartość zawiera symbol waluty. Dodaj $ przed każdą z pól tekstowych cen jednostkowych, aby traktować jako wizualną kontrolkę, która powoduje, że użytkownik pomija symbol waluty podczas wprowadzania ceny.

Na koniec Dodaj kontrolkę podsumowania walidacji w panelu `InsertingInterface`, ustawienia właściwości `ShowMessageBox` do `true` i jej właściwości `ShowSummary` do `false`. Jeśli przy użyciu tych ustawień użytkownik wprowadzi nieprawidłową wartość ceny jednostkowej, obok kontrolek TextBox pojawia się gwiazdka, a podsumowania walidacji wyświetli komunikat o błędzie, który określono wcześniej.

W tym momencie ekran powinien wyglądać podobnie do rysunku 10.

[![Wstawianie interfejsu zawiera teraz pola tekstowe dla nazw produktów i cen](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Ilustracja 10**. interfejs wstawiania zawiera teraz pola tekstowe dla nazw produktów i cen ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-inserting-cs/_static/image30.png))

Następnie musimy dodać przyciski Dodaj produkty z wysyłki i Anuluj do wiersza stopki. Przeciągnij dwa kontrolki przycisków z przybornika do stopki wstawianego interfejsu, ustawiając przyciski `ID` właściwości na `AddProducts` i `CancelButton` i `Text` właściwości, aby dodać produkty z wysyłki i anulować odpowiednio. Ponadto ustaw właściwość `CancelButton` Control s `CausesValidation` na `false`.

Na koniec musimy dodać kontrolkę sieci Web etykieta, która będzie wyświetlać komunikaty o stanie dla obu interfejsów. Na przykład, gdy użytkownik pomyślnie doda nową wysyłkę produktów, chcemy wrócić do interfejsu wyświetlania i wyświetlić komunikat potwierdzenia. Jeśli jednak użytkownik dostarczy cenę nowego produktu, ale opuszcza nazwę produktu, musimy wyświetlić komunikat ostrzegawczy, ponieważ pole `ProductName` jest wymagane. Ponieważ potrzebujemy, aby ten komunikat był wyświetlany dla obu interfejsów, umieść go w górnej części strony spoza paneli.

Przeciągnij kontrolkę Web Label z przybornika do góry strony w projektancie. Ustaw właściwość `ID` na `StatusLabel`, wyczyść Właściwość `Text` i ustaw właściwości `Visible` i `EnableViewState` na `false`. Jak widać w poprzednich samouczkach, ustawienie właściwości `EnableViewState` na `false` pozwala nam zmieniać wartości właściwości etykieta s i automatycznie przywracać je do wartości domyślnych podczas kolejnego ogłaszania zwrotnego. Upraszcza to kod pokazujący komunikat o stanie w odpowiedzi na część akcji użytkownika, która znika po kolejnym ogłaszaniu zwrotnym. Na koniec ustaw właściwość `StatusLabel` Control s `CssClass` na Warning, która jest nazwą klasy CSS zdefiniowanej w `Styles.css`, która wyświetla tekst w postaci długiej, kursywy, pogrubienia, czerwonej czcionki.

Rysunek 11 pokazuje projektanta programu Visual Studio po dodaniu i skonfigurowaniu etykiety.

[![umieścić formant StatusLabel powyżej dwóch kontrolek panelu](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Ilustracja 11**. umieszczenie kontrolki `StatusLabel` powyżej dwóch kontrolek panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image33.png))

## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Krok 3. przełączenie między wyświetlaniem i wstawianiem interfejsów

W tym momencie ukończymy znaczniki dotyczące wyświetlania i wstawiania interfejsów, ale nadal pozostawiłem dwa zadania:

- Przełączanie między wyświetlaniem i wstawianiem interfejsów
- Dodawanie produktów w wysyłce do bazy danych

Obecnie interfejs wyświetlania jest widoczny, ale interfejs wstawiania jest ukryty. Wynika to z faktu, że właściwość `Visible` panelu `DisplayInterface` jest ustawiona na `true` (wartość domyślna), podczas gdy właściwość `InsertingInterface` panelu s `Visible` jest ustawiona na `false`. Aby przełączać się między dwoma interfejsami, należy po prostu przełączyć każdą wartość właściwości `Visible` sterowania.

Chcemy przenieść z interfejsu wyświetlania do interfejsu wstawiania, gdy zostanie kliknięty przycisk Przetwarzaj produkt dostawy. W związku z tym Utwórz procedurę obsługi zdarzeń dla tego przycisku s `Click` zdarzenie, które zawiera następujący kod:

[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Ten kod po prostu ukrywa panel `DisplayInterface` i pokazuje panel `InsertingInterface`.

Następnie utwórz programy obsługi zdarzeń dla kontrolek Dodaj produkty z wysyłki i Anuluj w interfejsie wstawiania. Po kliknięciu jednego z tych przycisków musimy wrócić do interfejsu wyświetlania. Utwórz procedury obsługi zdarzeń `Click` dla obu formantów Button, aby wywoływały `ReturnToDisplayInterface`, metodę, którą dodamy na chwilę. Oprócz ukrywania panelu `InsertingInterface` i wyświetlania panelu `DisplayInterface`, Metoda `ReturnToDisplayInterface` musi zwrócić kontrolki sieci Web do ich stanu sprzed edycji. Obejmuje to ustawienie właściwości `SelectedIndex` kontrolek DropDownList na 0 i wyczyszczenie właściwości `Text` formantów TextBox.

> [!NOTE]
> Zastanów się, co może się zdarzyć, jeśli nie powrócisz formantów do stanu sprzed edycji przed powrotem do interfejsu wyświetlania. Użytkownik może kliknąć przycisk przetwórz wysyłkę produktu, wprowadzić produkty z wysyłki, a następnie kliknąć pozycję Dodaj produkty z wysyłki. Spowoduje to dodanie produktów i zwrócenie tego użytkownika do interfejsu wyświetlania. W tym momencie użytkownik może chcieć dodać kolejną wysyłkę. Po kliknięciu przycisku przetwórz dostawy produktu powrócisz do interfejsu wstawianego, ale wartości DropDownList i TextBox będą nadal wypełniane poprzednimi wartościami.

[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Oba procedury obsługi zdarzeń `Click` po prostu wywołują metodę `ReturnToDisplayInterface`, ale powrócimy do procedury obsługi zdarzeń Dodaj produkty z wysyłki `Click` w kroku 4 i Dodaj kod, aby zapisać produkty. `ReturnToDisplayInterface` uruchamia się, zwracając `Suppliers` i `Categories` kontrolek DropDownList do ich pierwszej opcji. Dwie stałe `firstControlID` i `lastControlID` oznaczają wartości indeksu początkową i końcową używane podczas nazywania pól tekstowych i cen jednostkowych w interfejsie wstawiania i są używane w granicach pętli `for`, która ustawia `Text` właściwości kontrolek TextBox z powrotem do pustego ciągu. Na koniec panele `Visible` właściwości są resetowane, aby interfejs wstawiania był ukryty i wyświetlany interfejs wyświetlania.

Poświęć chwilę na przetestowanie tej strony w przeglądarce. Podczas pierwszego odwiedzania strony powinien zostać wyświetlony interfejs wyświetlania, jak pokazano na rysunku 5. Kliknij przycisk Przetwarzaj wysyłkę produktu. Strona zostanie odstawiona na odświeżenie, a teraz zobaczysz interfejs wstawiania, jak pokazano na rysunku 12. Kliknięcie przycisku Dodaj produkty z wysyłki lub Anuluj powoduje powrót do interfejsu wyświetlania.

> [!NOTE]
> Podczas wyświetlania interfejsu wstawiania, poświęć chwilę na przetestowanie CompareValidators w polach tekstowych ceny jednostkowej. Gdy klikniesz przycisk Dodaj produkty z wysyłki po stronie klienta, zobaczysz ostrzeżenie o nieprawidłowych wartościach walutowych lub cenach o wartości mniejszej od zera.

[![Wstawianie interfejsu jest wyświetlane po kliknięciu przycisku przetwórz dostawy produktu](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Ilustracja 12**. po kliknięciu przycisku przetwórz produkt ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-inserting-cs/_static/image36.png)) zostanie wyświetlony interfejs wstawiania.

## <a name="step-4-adding-the-products"></a>Krok 4. Dodawanie produktów

Wszystkie te, które pozostaną w tym samouczku, mają na celu zapisanie produktów w bazie danych programu obsługi zdarzeń `Click` Dodaj produkty z wysyłki. Można to osiągnąć przez utworzenie `ProductsDataTable` i dodanie wystąpienia `ProductsRow` dla każdej dostarczonej nazwy produktu. Po dodaniu tych `ProductsRow` s wykonamy wywołanie metody `ProductsBLL` klasy s `UpdateWithTransaction` przekazywaniu w `ProductsDataTable`. Odwołaj tę metodę `UpdateWithTransaction`, która została utworzona z powrotem w przypadku [modyfikacji zawijania bazy danych w ramach](wrapping-database-modifications-within-a-transaction-cs.md) samouczka transakcji, przekazuje `ProductsDataTable` do metody `UpdateWithTransaction` `ProductsTableAdapter` s. Z tego miejsca zostanie uruchomiona ADO.NET transakcji, a TableAdapter wystawia instrukcję `INSERT` do bazy danych dla każdej dodanej `ProductsRow` w elemencie DataTable. Przy założeniu, że wszystkie produkty są dodawane bez błędu, transakcja zostanie zatwierdzona, w przeciwnym razie zostanie wycofana.

Kod dla przycisku Dodaj produkty z wysyłki s `Click` programu obsługi zdarzeń musi także wykonać trochę sprawdzania błędów. Ponieważ nie ma żadnych RequiredFieldValidators w interfejsie wstawiania, użytkownik może wprowadzić cenę dla produktu, pomijając jego nazwę. Ponieważ wymagana jest nazwa produktu, jeśli taki warunek nie ma załamania, musimy ostrzec użytkownika i nie kontynuować operacji wstawiania. Pełny kod procedury obsługi zdarzeń `Click` następujący:

[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

Program obsługi zdarzeń jest uruchamiany przez zagwarantowanie, że właściwość `Page.IsValid` zwraca wartość `true`. Jeśli zwróci `false`, oznacza to, że co najmniej jedna CompareValidators zgłasza nieprawidłowe dane; w takim przypadku nie chcemy próbować wstawiać wprowadzonych produktów lub wystąpi wyjątek podczas próby przypisania wartości ceny jednostkowej wprowadzonej przez użytkownika do właściwości `UnitPrice` `ProductsRow` s.

Następnie zostanie utworzone nowe wystąpienie `ProductsDataTable` (`products`). Pętla `for` jest używana do iteracji w postaci pól tekstowych i cen jednostkowych, a właściwości `Text` są odczytywane w zmiennych lokalnych `productName` i `unitPrice`. Jeśli użytkownik wprowadzi wartość w polu cena jednostkowa, ale nie dla odpowiadającej mu nazwy produktu, `StatusLabel` wyświetli komunikat, jeśli podano cenę jednostkową, należy również uwzględnić nazwę produktu, a program obsługi zdarzeń zostanie zakończony.

W przypadku podanej nazwy produktu nowe wystąpienie `ProductsRow` jest tworzone przy użyciu metody `ProductsDataTable` s `NewProductsRow`. Ta nowa Właściwość `ProductsRow` wystąpienia s `ProductName` jest ustawiona na bieżącą nazwę produktu, podczas gdy właściwości `SupplierID` i `CategoryID` są przypisane do właściwości `SelectedValue` kontrolek DropDownList w nagłówku Wstawianie interfejsu s. Jeśli użytkownik wprowadził wartość dla ceny produktu s, zostanie przypisany do właściwości `ProductsRow` wystąpienia s `UnitPrice`; w przeciwnym razie właściwość zostanie pozostawiona nieprzypisane, co spowoduje `NULL` wartość `UnitPrice` w bazie danych. Na koniec właściwości `Discontinued` i `UnitsOnOrder` są przypisywane do zakodowanych wartości `false` i 0 odpowiednio.

Po przypisaniu właściwości do wystąpienia `ProductsRow` zostanie ono dodane do `ProductsDataTable`.

Po zakończeniu pętli `for` sprawdzimy, czy zostały dodane jakieś produkty. Użytkownik może po każdym kliknięciu dodać produkty z wysyłki przed wprowadzeniem jakichkolwiek nazw produktów lub cen. Jeśli w `ProductsDataTable`istnieje co najmniej jeden produkt, wywoływana jest metoda `ProductsBLL` klasy s `UpdateWithTransaction`. Następnie dane zostaną przepowiązane z `ProductsGrid` GridView, aby nowo dodane produkty pojawiły się w interfejsie wyświetlania. `StatusLabel` został zaktualizowany, aby wyświetlić komunikat z potwierdzeniem i `ReturnToDisplayInterface` jest wywoływany, ukrywając interfejs wstawiania i pokazując interfejs wyświetlania.

Jeśli nie wprowadzono żadnych produktów, interfejs wstawiania pozostaje wyświetlany, ale komunikat nie został dodany. Wprowadź nazwy produktów i ceny jednostkowe w polach tekstowych.

Rysunek s 13, 14 i 15 Pokaż interfejsy wstawiania i wyświetlania w działaniu. Na rysunku 13 użytkownik wprowadził wartość ceny jednostkowej bez odpowiadającej jej nazwy produktu. Ilustracja 14 przedstawia interfejs wyświetlania po pomyślnym dodaniu trzech nowych produktów, natomiast rysunek 15 pokazuje dwa z nowo dodanych produktów w GridView (trzecia strona znajduje się na poprzedniej stronie).

[![Nazwa produktu jest wymagana, gdy zostanie wprowadzona cena jednostkowa](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Ilustracja 13**. Nazwa produktu jest wymagana podczas wprowadzania ceny jednostkowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image39.png))

[![dodano trzy nowe veggies dla dostawcy Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Ilustracja 14**. Dodano trzy nowe veggies dla dostawcy Mayumi s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-inserting-cs/_static/image42.png))

[![nowe produkty znajdują się na ostatniej stronie widoku GridView](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Ilustracja 15**. nowe produkty znajdują się na ostatniej stronie widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](batch-inserting-cs/_static/image45.png))

> [!NOTE]
> Logika wstawiania wsadowego użyta w tym samouczku otacza Wstawianie w ramach zakresu transakcji. Aby to sprawdzić, celowo całkowicie wprowadzić błąd na poziomie bazy danych. Na przykład zamiast przypisywać nową właściwość `ProductsRow` wystąpienia s `CategoryID` do wybranej wartości w `Categories` DropDownList, przypisz ją do wartości podobnej do `i * 5`. Tutaj `i` jest indeksator pętli i ma wartości z zakresu od 1 do 5. W związku z tym podczas dodawania co najmniej dwóch produktów w usłudze Batch pierwszy produkt będzie miał prawidłową wartość `CategoryID` (5), ale kolejne produkty będą mieć `CategoryID` wartości, które nie pasują do `CategoryID` wartości w tabeli `Categories`. Efektem netto jest to, że podczas gdy pierwsze `INSERT` powiedzie się, kolejne z nich zakończą się niepowodzeniem z naruszeniem ograniczenia klucza obcego. Ponieważ wstawka wsadowa jest niepodzielna, pierwsze `INSERT` zostanie wycofane, zwracając bazę danych do stanu przed rozpoczęciem procesu tworzenia wsadowego.

## <a name="summary"></a>Podsumowanie

W ten sposób i poprzednie dwa samouczki utworzyły interfejsy umożliwiające aktualizowanie, usuwanie i wstawianie partii danych, z których korzystamy z obsługi transakcji dodanych do warstwy dostępu do danych w ramach samouczka [transakcji](wrapping-database-modifications-within-a-transaction-cs.md) . W niektórych scenariuszach takie przetwarzanie wsadowe interfejsów użytkownika znacznie poprawia wydajność użytkowników końcowych dzięki wykorzystaniu liczby kliknięć, ogłaszania zwrotnego i przełączania kontekstu klawiatury na mysz, zachowując jednocześnie integralność danych źródłowych.

Ten samouczek kończy pracę z danymi wsadowymi. W następnym zestawie samouczków przedstawiono różne scenariusze zaawansowanej warstwy dostępu do danych, w tym używanie procedur składowanych w metodach TableAdapter, Konfigurowanie ustawień na poziomie połączenia i poleceń w DAL, szyfrowanie parametrów połączenia itp.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci dla liderów w tym samouczku zostały Hilton Giesenow i S Ren Jacob Lauritsen. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](batch-deleting-cs.md)
> [dalej](wrapping-database-modifications-within-a-transaction-vb.md)
