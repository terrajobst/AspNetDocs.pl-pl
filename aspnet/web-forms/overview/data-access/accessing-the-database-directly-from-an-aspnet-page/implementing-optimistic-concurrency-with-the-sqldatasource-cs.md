---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: Implementowanie optymistycznej współbieżności przy użyciuC#kontrolki SqlDataSource () | Microsoft Docs
author: rick-anderson
description: W tym samouczku zapoznajemy podstawowe informacje o optymistycznej kontroli współbieżności, a następnie zapoznaj się z tematem, jak wdrożyć go przy użyciu formantu kontrolki SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 87fca52e2e8be844411b2fff8382c6002eccbe09
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553236"
---
# <a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implementowanie optymistycznej współbieżności przy użyciu kontrolki SqlDataSource (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) lub [Pobierz plik PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> W tym samouczku zapoznajemy podstawowe informacje o optymistycznej kontroli współbieżności, a następnie zapoznaj się z tematem, jak wdrożyć go przy użyciu formantu kontrolki SqlDataSource.

## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku sprawdzono, jak dodać funkcje wstawiania, aktualizowania i usuwania do kontrolki kontrolki SqlDataSource. Krótko mówiąc, aby zapewnić te funkcje, które są potrzebne do określenia odpowiednich `INSERT`, `UPDATE`lub `DELETE` instrukcji SQL w obszarze Control s `InsertCommand`, `UpdateCommand`lub `DeleteCommand`, wraz z odpowiednimi parametrami w `InsertParameters`, `UpdateParameters`i `DeleteParameters` kolekcje. Mimo że te właściwości i Kolekcje można określić ręcznie, przycisk Zaawansowane Kreator konfigurowania źródła danych oferuje opcję Generuj `INSERT`, `UPDATE`i `DELETE` instrukcji, które będą automatycznie tworzyć te instrukcje na podstawie instrukcji `SELECT`.

Wraz z zaznaczeniem pola wyboru Generuj `INSERT`, `UPDATE`i `DELETE`, zaawansowane opcje generowania instrukcji SQL zawierają opcję Użyj optymistycznej współbieżności (patrz rysunek 1). Po zaznaczeniu tej opcji klauzule `WHERE` w instrukcjach generowanych automatycznie `UPDATE` i `DELETE` są modyfikowane tak, aby wykonywały tylko aktualizację lub usuwanie, jeśli dane źródłowej bazy danych nie zostały zmodyfikowane od czasu ostatniego załadowania danych do siatki.

![Możesz dodać optymistyczną obsługę współbieżności z okna dialogowego Zaawansowane opcje generowania kodu SQL](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Rysunek 1**: możesz dodać optymistyczną obsługę współbieżności z okna dialogowego Zaawansowane opcje generowania kodu SQL

Z powrotem w samouczku [wdrażanie optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) zbadamy podstawy optymistycznej kontroli współbieżności i sposób dodawania jej do elementu ObjectDataSource. W tym samouczku powrócimy do podstawy optymistycznej kontroli współbieżności, a następnie Dowiedz się, jak zaimplementować ją przy użyciu kontrolki SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Podsumowanie optymistycznej współbieżności

W przypadku aplikacji sieci Web, które umożliwiają wielu użytkownikom jednoczesne edytowanie lub usuwanie tych samych danych, istnieje możliwość, że jeden użytkownik może przypadkowo zastąpić inne zmiany s. W samouczku [wdrażanie optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) podano następujący przykład:

Załóżmy, że dwaj użytkownicy, Jisun i sam odwiedzają stronę w aplikacji, która umożliwia osobom odwiedzającym aktualizowanie i usuwanie produktów za pomocą kontrolki GridView. W tym samym czasie kliknij przycisk Edytuj dla Chai. Jisun zmienia nazwę produktu na Chai herbata i klika przycisk Aktualizuj. Wynikiem sieci jest instrukcja `UPDATE`, która jest wysyłana do bazy danych, która ustawia *wszystkie* pola do zaktualizowania produktu (nawet jeśli Jisun tylko jedno pole, `ProductName`). W tym momencie baza danych ma wartości Chai herbata, kategorie napoje, egzotyczne płyny i tak dalej dla tego konkretnego produktu. Jednak widok GridView na ekranie sam s nadal pokazuje nazwę produktu w edytowalnym wierszu GridView jako Chai. Kilka sekund po zatwierdzeniu zmian Jisun s, sam aktualizuje kategorię do przypraw i klika przycisk Aktualizuj. Spowoduje to wysłanie instrukcji `UPDATE` do bazy danych, która ustawia nazwę produktu na Chai, `CategoryID` do odpowiedniego identyfikatora kategorii przypraw i tak dalej. Jisun s zmiany nazwy produktu zostały nadpisywane.

Na rysunku 2 przedstawiono tę interakcję.

[![, gdy dwaj użytkownicy jednocześnie zaktualizują rekord, a potencjalne dla jednego użytkownika s zmienią się, aby zastąpić inne s](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Rysunek 2**. gdy dwaj użytkownicy jednocześnie zaktualizują rekord, a potencjalne dla jednego użytkownika s zmienią się, aby zastąpić inne s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))

Aby zapobiec niezgięciu tego scenariusza, należy zaimplementować formularz [kontroli współbieżności](http://en.wikipedia.org/wiki/Concurrency_control) . [Optymistyczne współbieżność](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) ten samouczek działa w oparciu o założenie, że w tym czasie mogą wystąpić konflikty współbieżności co teraz, a następnie, większość czasu taka konfliktów nie będzie się powtarzać. W związku z tym, jeśli wystąpi konflikt, optymistyczna kontrola współbieżności po prostu informuje użytkownika o tym, że zmiany mogą zostać zapisane, ponieważ inny użytkownik zmodyfikował te same dane.

> [!NOTE]
> W przypadku aplikacji, w których zakłada się, że wystąpią liczne konflikty współbieżności lub takie konflikty nie są dopuszczalne, zamiast tego można użyć metody pesymistycznej współbieżności. Zapoznaj się z artykułem [wdrażanie optymistycznego samouczka współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) , aby zapoznać się z bardziej dokładną dyskusją na temat współbieżności na serwerze.

Optymistyczna kontrola współbieżności działa przez upewnienie się, że aktualizowany lub usuwany rekord ma takie same wartości jak w przypadku uruchomienia procesu aktualizowania lub usuwania. Na przykład po kliknięciu przycisku Edytuj w edytowalnym widoku GridView wartości rekordu s są odczytywane z bazy danych i wyświetlane w polach tekstowych i innych formantach sieci Web. Te oryginalne wartości są zapisywane przez GridView. Później, po wprowadzeniu zmian przez użytkownika i kliknięciu przycisku Aktualizuj, użyta instrukcja `UPDATE` musi uwzględniać pierwotne wartości oraz nowe wartości i aktualizować rekord bazy danych tylko wtedy, gdy pierwotne wartości edytowane przez użytkownika są identyczne z wartościami nadal w bazie danych. Rysunek 3 przedstawia tę sekwencję zdarzeń.

[Aby można było pomyślnie zaktualizować lub usunąć ![, oryginalne wartości muszą być równe bieżącym wartościom bazy danych](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Rysunek 3**. Aby pomyślnie zaktualizować lub usunąć, oryginalne wartości muszą być równe bieżącym wartościom bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))

Istnieją różne podejścia do implementowania optymistycznej współbieżności (zobacz "Bromberg" [optymistycznej logiki aktualizacji współbieżności](http://www.eggheadcafe.com/articles/20050719.asp) [,](http://www.eggheadcafe.com/articles/pbrombergresume.asp)aby skrócić kilka opcji). Technika używana przez kontrolki SqlDataSource (a także przez zestawy danych typu ADO.NET używany w naszej warstwie dostępu do danych) rozszerza klauzulę `WHERE` w celu uwzględnienia porównania wszystkich oryginalnych wartości. Poniższa instrukcja `UPDATE`, na przykład aktualizuje nazwę i cenę produktu, tylko wtedy, gdy bieżące wartości bazy danych są równe wartościom, które zostały pierwotnie pobrane podczas aktualizowania rekordu w widoku GridView. Parametry `@ProductName` i `@UnitPrice` zawierają nowe wartości wprowadzone przez użytkownika, natomiast `@original_ProductName` i `@original_UnitPrice` zawierają wartości, które zostały początkowo załadowane do widoku GridView po kliknięciu przycisku Edytuj:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Jak zobaczymy w tym samouczku, włączenie optymistycznej kontroli współbieżności za pomocą kontrolki SqlDataSource jest tak proste jak zaznaczenie pola wyboru.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Krok 1. Tworzenie elementu kontrolki SqlDataSource z obsługą optymistycznej współbieżności

Zacznij od otwarcia strony `OptimisticConcurrency.aspx` z folderu `SqlDataSource`. Przeciągnij formant kontrolki SqlDataSource z przybornika do projektanta, ustawienia jego właściwości `ID`, aby `ProductsDataSourceWithOptimisticConcurrency`. Następnie kliknij link Konfiguruj źródło danych z tagu inteligentnego sterowanie s. Na pierwszym ekranie kreatora wybierz opcję pracy z `NORTHWINDConnectionString` i kliknij przycisk Dalej.

[![wybrać NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Ilustracja 4**. Wybierz, aby współpracować z `NORTHWINDConnectionString` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))

Na potrzeby tego przykładu dodamy widok GridView, który umożliwi użytkownikom edytowanie tabeli `Products`. W związku z tym, na ekranie Konfigurowanie instrukcji SELECT wybierz tabelę `Products` z listy rozwijanej i wybierz kolumny `ProductID`, `ProductName`, `UnitPrice`i `Discontinued`, jak pokazano na rysunku 5.

[![z tabeli Products (produkty) Zwróć kolumny IDProduktu, ProductName, CenaJednostkowa i uncontinued](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Rysunek 5**. z tabeli `Products` zwróć kolumny `ProductID`, `ProductName`, `UnitPrice`i `Discontinued` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))

Po wybraniu kolumn kliknij przycisk Zaawansowane, aby wyświetlić okno dialogowe Zaawansowane opcje generowania bazy danych SQL. Sprawdź instrukcje Generate `INSERT`, `UPDATE`i `DELETE` i użyj optymistycznych pól wyboru współbieżności, a następnie kliknij przycisk OK (zapoznaj się z powrotem do rysunku 1 w przypadku zrzutu ekranu). Ukończ pracę kreatora, klikając przycisk Dalej, a następnie przycisk Zakończ.

Po zakończeniu działania Kreatora konfiguracji źródła danych Poświęć chwilę, aby przeanalizować wyniki `DeleteCommand` i `UpdateCommand`, a `DeleteParameters` i `UpdateParameters` kolekcje. Najprostszym sposobem jest kliknięcie karty Źródło w lewym dolnym rogu, aby wyświetlić składnię deklaratywną strony. Znajdziesz `UpdateCommand` wartość:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Z siedem parametrów w kolekcji `UpdateParameters`:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

Podobnie Właściwość `DeleteCommand` i kolekcja `DeleteParameters` powinny wyglądać następująco:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Oprócz rozszerzania `WHERE` klauzule `UpdateCommand` i `DeleteCommand` właściwości (i dodawania dodatkowych parametrów do odpowiednich kolekcji parametrów), wybranie opcji Użyj optymistycznej współbieżności dostosowuje dwie inne właściwości:

- Zmienia [właściwość`ConflictDetection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) z `OverwriteChanges` (domyślnie) na `CompareAllValues`
- Zmienia [właściwość`OldValuesParameterFormatString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) z {0} (domyślnie) na oryginalny\_{0}.

Gdy formant sieci Web danych wywołuje metodę kontrolki SqlDataSource `Update()` lub `Delete()`, zostanie ona przekazana do oryginalnych wartości. Jeśli właściwość kontrolki SqlDataSource s `ConflictDetection` jest ustawiona na `CompareAllValues`, te oryginalne wartości zostaną dodane do polecenia. Właściwość `OldValuesParameterFormatString` zapewnia wzorzec nazewnictwa używany dla tych oryginalnych parametrów wartości. Kreator konfiguracji źródła danych używa oryginalnego\_{0} i nazwy każdego oryginalnego parametru w `UpdateCommand` i `DeleteCommand` właściwości oraz `UpdateParameters` i `DeleteParameters` kolekcje.

> [!NOTE]
> Ponieważ nie korzystamy z funkcji kontrolki SqlDataSource Control s, możesz usunąć Właściwość `InsertCommand` i jej kolekcję `InsertParameters`.

## <a name="correctly-handlingnullvalues"></a>Prawidłowe obsługiwanie wartości`NULL`

Niestety, rozszerzone `UPDATE` i `DELETE` są automatycznie generowane przez Kreatora konfiguracji źródła danych, gdy użycie optymistycznej współbieżności *nie* współdziała z rekordami zawierającymi wartości `NULL`. Aby zobaczyć dlaczego, weź pod uwagę nasze kontrolki SqlDataSource `UpdateCommand`:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

Kolumna `UnitPrice` w tabeli `Products` może mieć wartości `NULL`. Jeśli konkretny rekord ma `NULL` wartość dla `UnitPrice`, `[UnitPrice] = @original_UnitPrice` klauzula `WHERE` jest *zawsze* Szacowana jako FAŁSZ, ponieważ `NULL = NULL` zawsze zwróci wartość false. W związku z tym rekordy, które zawierają wartości `NULL` nie mogą być edytowane ani usuwane, ponieważ `WHERE` instrukcje `UPDATE` i `DELETE` nie zwracają żadnych wierszy do zaktualizowania lub usunięcia.

> [!NOTE]
> Ta usterka została po raz pierwszy zgłoszona firmie Microsoft w czerwcu z 2004 w [kontrolki SqlDataSource generuje nieprawidłowe instrukcje SQL](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) i reportedly zaplanowana w następnej wersji ASP.NET.

Aby rozwiązać ten problem, należy ręcznie zaktualizować klauzule `WHERE` we właściwościach `UpdateCommand` i `DeleteCommand` dla **wszystkich** kolumn, które mogą mieć `NULL` wartości. Ogólnie rzecz biorąc Zmień `[ColumnName] = @original_ColumnName` na:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Tę modyfikację można wykonać bezpośrednio za pośrednictwem znaczników deklaratywnych za pomocą opcji UpdateQuery lub DeleteQuery z okno Właściwości lub za pomocą kart UPDATE i DELETE w polu Określ niestandardową instrukcję SQL lub procedurę składowaną w obszarze Konfigurowanie danych Kreator źródła. Ponownie należy wprowadzić tę modyfikację dla *każdej* kolumny w `UpdateCommand` i `DeleteCommand` s `WHERE`, która może zawierać `NULL` wartości.

Zastosowanie tego do naszego przykładu spowoduje zmodyfikowanie następujących wartości `UpdateCommand` i `DeleteCommand`:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Krok 2. Dodawanie widoku GridView z opcjami edycji i usuwania

Kontrolki SqlDataSource skonfigurowany do obsługi optymistycznej współbieżności, to wszystko, co ma na celu dodanie kontrolki sieci Web danych do strony, która korzysta z tej kontroli współbieżności. Na potrzeby tego samouczka Dodaj widok GridView, który zapewnia funkcje edycji i usuwania. Aby to osiągnąć, przeciągnij widok GridView z przybornika do projektanta i ustaw jego `ID` na `Products`. W tagu inteligentnym GridView powiąż go z `ProductsDataSourceWithOptimisticConcurrency` kontrolką kontrolki SqlDataSource dodaną w kroku 1. Na koniec sprawdź opcje Włącz edytowanie i Włącz usuwanie z tagu inteligentnego.

[![powiązać widok GridView z kontrolki SqlDataSource i Włącz edytowanie i usuwanie](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Ilustracja 6**. powiązanie widoku GridView z kontrolki SqlDataSource i włączanie edytowania i usuwania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))

Po dodaniu widoku GridView skonfiguruj jego wygląd, usuwając `ProductID` BoundField, zmieniając właściwość `ProductName` BoundField s `HeaderText` na produkt i aktualizując `UnitPrice` BoundField tak, aby jej Właściwość `HeaderText` była po prostu cena. W idealnym przypadku ulepszamy interfejs edycji tak, aby zawierał RequiredFieldValidator dla wartości `ProductName` i CompareValidator dla wartości `UnitPrice` (w celu zapewnienia, że s ma poprawnie sformatowaną wartość liczbową). Zapoznaj się z samouczkiem [Dostosowywanie interfejsu modyfikacji danych,](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) aby zapoznać się z bardziej szczegółowymi krokami dostosowywania interfejsu edytowania GridView.

> [!NOTE]
> Stan widoku GridView s musi być włączony, ponieważ oryginalne wartości przesłane z widoku GridView do kontrolki SqlDataSource są przechowywane w stanie widoku.

Po wprowadzeniu tych zmian do widoku GridView, znaczniki deklaracyjne i kontrolki SqlDataSource, powinny wyglądać podobnie do następujących:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Aby wyświetlić optymistyczną kontrolę współbieżności w akcji, Otwórz dwa okna przeglądarki i Załaduj stronę `OptimisticConcurrency.aspx` w obu. Kliknij przyciski edycji pierwszego produktu w obu przeglądarkach. W jednej przeglądarce Zmień nazwę produktu, a następnie kliknij przycisk Aktualizuj. Przeglądarka będzie ogłaszać zwrotnie, a widok GridView powróci do trybu sprzed edycji, pokazując nową nazwę produktu dla edytowanego właśnie rekordu.

W drugim oknie przeglądarki Zmień cenę (ale pozostaw nazwę produktu jako oryginalną), a następnie kliknij przycisk Aktualizuj. Na stronie ogłaszania zwrotnego siatka powraca do trybu sprzed edycji, ale zmiana ceny nie jest zarejestrowana. Druga przeglądarka pokazuje taką samą wartość jak pierwsza Nowa nazwa produktu ze starą ceną. Zmiany wprowadzone w drugim oknie przeglądarki zostały utracone. Ponadto zmiany zostały utracone w sposób cichy, ponieważ nie wystąpił wyjątek lub komunikat informujący o tym, że wystąpiło naruszenie współbieżności.

[![zmiany w drugim oknie przeglądarki zostały utracone w trybie dyskretnym](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Rysunek 7**. zmiany w drugim oknie przeglądarki zostały utracone w trybie dyskretnym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))

Powód, dla którego nie zostały zatwierdzone zmiany w przeglądarce s, jest to spowodowane tym, że klauzula `UPDATE` s `WHERE` odfiltrowana wszystkie rekordy i w związku z tym nie ma wpływu na żadne wiersze. Pozwól, aby ponownie przyjrzeć się instrukcji `UPDATE`:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Gdy drugie okno przeglądarki aktualizuje rekord, oryginalna nazwa produktu określona w klauzuli `WHERE` nie pasuje do istniejącej nazwy produktu (ponieważ została zmieniona przez pierwszą przeglądarkę). W związku z tym, instrukcja `[ProductName] = @original_ProductName` zwraca wartość false, a `UPDATE` nie ma wpływu na żadne rekordy.

> [!NOTE]
> Usuwanie działa w ten sam sposób. Po otwarciu dwóch okien przeglądarki Zacznij od edycji danego produktu z jednym, a następnie zapisując zmiany. Po zapisaniu zmian w jednej przeglądarce kliknij przycisk Usuń dla tego samego produktu w drugim. Ponieważ oryginalne wartości nie są zgodne z klauzulą `DELETE` instrukcji s `WHERE`, usuwanie dyskretne zakończy się niepowodzeniem.

Z perspektywy użytkownika końcowego w drugim oknie przeglądarki po kliknięciu przycisku Aktualizuj siatka wraca do trybu sprzed edycji, ale ich zmiany zostały utracone. Nie ma jednak żadnych opinii wizualnych, że ich zmiany nie zostały nalepki. W idealnym przypadku, jeśli zmiany użytkownika s zostaną utracone do naruszenia współbieżności, powiadomemy o nich i, prawdopodobnie, Zachowaj siatkę w trybie edycji. Poinformuj o tym, jak to zrobić.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Krok 3. Określanie, kiedy nastąpiło naruszenie współbieżności

Ponieważ naruszenie współbieżności odrzuci wprowadzone zmiany, warto ostrzec użytkownika o wystąpieniu naruszenia współbieżności. Aby ostrzec użytkownika, Dodaj kontrolkę sieci Web etykieta w górnej części strony o nazwie `ConcurrencyViolationMessage` której Właściwość `Text` wyświetla następujący komunikat: podjęto próbę zaktualizowania lub usunięcia rekordu, który został jednocześnie zaktualizowany przez innego użytkownika. Przejrzyj zmiany wprowadzone przez innych użytkowników, a następnie ponów próbę aktualizacji lub usunięcia. Ustaw właściwość kontrolki etykieta `CssClass` na ostrzeżenie, która jest klasą CSS zdefiniowaną w `Styles.css`, która wyświetla tekst w kolorze czerwonym, kursywą, pogrubioną i dużą czcionką. Na koniec ustaw właściwości label-`Visible` i `EnableViewState` na `false`. Spowoduje to ukrycie etykiety z wyjątkiem tych ogłaszania zwrotnego, w przypadku których jawnie ustawimy Właściwość `Visible` na `true`.

[![dodać kontrolkę etykieta do strony, aby wyświetlić ostrzeżenie](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Ilustracja 8**. Dodawanie kontrolki etykieta do strony, aby wyświetlić ostrzeżenie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))

Podczas przeprowadzania aktualizacji lub usuwania programy obsługi zdarzeń GridView `RowUpdated` i `RowDeleted` są uruchamiane po przeprowadzeniu przez kontrolę źródła danych żądanej aktualizacji lub usunięcia. Możemy określić liczbę wierszy, na które miało wpływ operacja z tych programów obsługi zdarzeń. Jeśli miało to wartość zero, chcemy wyświetlić etykietę `ConcurrencyViolationMessage`.

Utwórz procedurę obsługi zdarzeń dla zdarzeń `RowUpdated` i `RowDeleted` i Dodaj następujący kod:

[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

W obu obsłudze zdarzeń sprawdzimy Właściwość `e.AffectedRows` i, jeśli jest równa 0, ustaw właściwość `ConcurrencyViolationMessage` etykieta s `Visible` na `true`. W obsłudze zdarzeń `RowUpdated` poinstruujmy również GridView, aby pozostały w trybie edycji, ustawiając właściwość `KeepInEditMode` na true. W tym celu należy ponownie powiązać dane z siatką, tak aby inne dane użytkownika zostały załadowane do interfejsu edycji. W tym celu należy wywołać metodę `DataBind()` GridView.

Jak pokazano na rysunku 9, za każdym razem, gdy występuje naruszenie współbieżności, zostanie wyświetlony komunikat z tymi dwoma obsłudze zdarzeń.

[![komunikat jest wyświetlany w przypadku naruszenia współbieżności](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Ilustracja 9**. komunikat jest wyświetlany w przypadku naruszenia współbieżności ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))

## <a name="summary"></a>Podsumowanie

Podczas tworzenia aplikacji sieci Web, w której wielu równoczesnych użytkowników może edytować te same dane, ważne jest, aby wziąć pod uwagę opcje kontroli współbieżności. Domyślnie kontrolki sieci Web ASP.NET danych i kontrolki źródła danych nie używają żadnej kontroli współbieżności. Jak zostało to opisane w tym samouczku, implementacja optymistycznej kontroli współbieżności za pomocą kontrolki SqlDataSource jest stosunkowo szybka i łatwa. Kontrolki SqlDataSource obsługuje większość żmudne zadania dla dodawania rozszerzonych klauzul `WHERE` do `UPDATE` automatycznie generowanych instrukcji i `DELETE`, ale istnieje kilka subtleties w obsłudze kolumn wartości `NULL`, zgodnie z opisem w sekcji prawidłowe obsługiwane wartości `NULL`.

W tym samouczku zakończymy nasze badanie kontrolki SqlDataSource. Nasze pozostałe samouczki powracają do pracy z danymi przy użyciu warstwy ObjectDataSource i warstwowej.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [dalej](querying-data-with-the-sqldatasource-control-vb.md)
