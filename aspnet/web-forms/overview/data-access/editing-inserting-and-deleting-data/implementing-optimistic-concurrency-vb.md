---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementowanie optymistycznej współbieżności (VB) | Microsoft Docs
author: rick-anderson
description: W przypadku aplikacji sieci Web, która pozwala wielu użytkownikom edytować dane, istnieje ryzyko, że dwóch użytkowników może jednocześnie edytować te same dane. W tym tutori...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 28c39fe2a290cc3a5b093fdd09de341630606137
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74628695"
---
# <a name="implementing-optimistic-concurrency-vb"></a>Implementowanie optymistycznej współbieżności (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) lub [Pobierz plik PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> W przypadku aplikacji sieci Web, która pozwala wielu użytkownikom edytować dane, istnieje ryzyko, że dwóch użytkowników może jednocześnie edytować te same dane. W tym samouczku wykonamy implementację optymistycznej kontroli współbieżności w celu obsługi tego ryzyka.

## <a name="introduction"></a>Wprowadzenie

W przypadku aplikacji sieci Web, które umożliwiają użytkownikom wyświetlanie danych lub dla tych, które zawierają tylko jednego użytkownika, który może modyfikować dane, nie ma żadnego zagrożenia dla dwóch równoczesnych użytkowników. W przypadku aplikacji sieci Web, które umożliwiają wielu użytkownikom aktualizowanie lub usuwanie danych, istnieje jednak możliwość modyfikacji konfliktów jednego użytkownika z innym współbieżnym użytkownikiem. Bez żadnych zasad współbieżności, gdy dwaj użytkownicy jednocześnie edytują pojedynczy rekord, użytkownik, który zatwierdzi zmiany, zastąpi zmiany wprowadzone w pierwszej kolejności.

Załóżmy na przykład, że dwóch użytkowników, Jisun i sam odwiedzają stronę w naszej aplikacji, która umożliwia odwiedzającym aktualizowanie i usuwanie produktów za pomocą kontrolki GridView. Jednocześnie kliknij przycisk Edytuj w widoku GridView. Jisun zmienia nazwę produktu na "Chai herbata" i klika przycisk Aktualizuj. Wynik netto to instrukcja `UPDATE`, która jest wysyłana do bazy danych, która ustawia *wszystkie* pola, które można aktualizować, mimo że Jisun tylko zaktualizowane jedno pole, `ProductName`). W tym momencie baza danych ma wartości "Chai herbata", "napoje kategorii", "produkty" i tak dalej dla tego konkretnego produktu. Jednak widok GridView na ekranie sam wyświetla nazwę produktu w edytowalnym wierszu GridView jako "Chai". Kilka sekund po przeprowadzeniu zmian Jisun, sam aktualizuje kategorię do przypraw i klika przycisk Aktualizuj. Spowoduje to wysłanie instrukcji `UPDATE` do bazy danych, która ustawia nazwę produktu na "Chai", `CategoryID` do odpowiedniego identyfikatora kategorii napoje i tak dalej. Zmiany nazwy produktu Jisun zostały nadpisywane. Rysunek 1 ilustracja przedstawia tę serię zdarzeń.

[![, gdy dwaj użytkownicy jednocześnie zaktualizują rekord, a potencjalną zmianą jednego użytkownika w celu zastąpienia](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Rysunek 1**. gdy dwaj użytkownicy jednocześnie zaktualizują rekord, a potencjalną zmianą dla jednego użytkownika zastąpią inne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image3.png))

Podobnie, gdy dwaj użytkownicy odwiedzają stronę, jeden użytkownik może być w pośrodku o aktualizację rekordu, gdy zostanie on usunięty przez innego użytkownika. Lub, między kiedy użytkownik załaduje stronę i klika przycisk Usuń, może zmodyfikować zawartość tego rekordu przez innego użytkownika.

Dostępne są trzy strategie [kontroli współbieżności](http://en.wikipedia.org/wiki/Concurrency_control) :

- **Nic nie rób** — Jeśli jednoczesny użytkownik modyfikuje ten sam rekord, pozwól na ostatnie Zatwierdź (zachowanie domyślne)
- [**Współbieżność optymistyczna**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) — zakłada, że podczas gdy mogą wystąpić konflikty współbieżności co teraz, a następnie, większość czasu takie konflikty nie wystąpi. w związku z tym, jeśli wystąpi konflikt, po prostu poinformuj użytkownika o tym, że nie można zapisać zmian, ponieważ inny użytkownik zmodyfikował te same dane
- **Współbieżność pesymistyczna** — założono, że konflikty współbieżności są commonplace i że użytkownicy nie będą tolerowali, że zmiany nie zostały zapisane z powodu współbieżnych działań użytkownika. w związku z tym, gdy jeden użytkownik zaczyna aktualizować rekord, należy go zablokować, aby uniemożliwić innym użytkownikom edytowanie lub usuwanie rekordu, dopóki użytkownik nie zatwierdzi ich modyfikacji

We wszystkich naszych samouczkach użyto domyślnej strategii rozwiązywania współbieżności — w tym Przypuśćmy, że wystąpiła Ostatnia zapis. W tym samouczku opisano sposób implementacji optymistycznej kontroli współbieżności.

> [!NOTE]
> W tej serii samouczków nie będzie wyglądać na przykład pesymistyczne współbieżności. Współbieżność pesymistyczna jest rzadko używana, ponieważ takie blokady, w przypadku nieprawidłowego wypróbowania, mogą uniemożliwić innym użytkownikom aktualizowanie danych. Na przykład jeśli użytkownik zablokuje rekord do edycji, a następnie opuszcza dzień przed jego odblokowaniem, żaden inny użytkownik nie będzie mógł zaktualizować tego rekordu, dopóki oryginalny użytkownik nie zwróci i nie ukończy aktualizacji. W związku z tym w sytuacjach, w których jest używana pesymistyczna współbieżność, zazwyczaj jest przekroczenie limitu czasu, który, jeśli został osiągnięty, anuluje blokadę. Witryny sieci Web sprzedaży biletów, która blokuje konkretną lokalizację dla krótkiego okresu, gdy użytkownik ukończy proces zamawiania, to przykład pesymistycznej kontroli współbieżności.

## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Krok 1. spojrzenie na sposób implementacji optymistycznej współbieżności

Optymistyczna kontrola współbieżności działa przez upewnienie się, że aktualizowany lub usuwany rekord ma takie same wartości jak w przypadku uruchomienia procesu aktualizowania lub usuwania. Na przykład po kliknięciu przycisku Edytuj w edytowalnym widoku GridView wartości rekordu są odczytywane z bazy danych i wyświetlane w polach tekstowych i innych formantach sieci Web. Te oryginalne wartości są zapisywane przez GridView. Później po wprowadzeniu zmian przez użytkownika i kliknięciu przycisku Aktualizuj pierwotne wartości oraz nowe wartości są wysyłane do warstwy logiki biznesowej, a następnie do warstwy dostępu do danych. Warstwa dostępu do danych musi wystawić instrukcję SQL, która zaktualizuje rekord tylko wtedy, gdy pierwotne wartości edytowane przez użytkownika są identyczne z wartościami nadal w bazie danych. Rysunek 2 przedstawia tę sekwencję zdarzeń.

[Aby można było pomyślnie zaktualizować lub usunąć ![, oryginalne wartości muszą być równe bieżącym wartościom bazy danych](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Rysunek 2**. Aby pomyślnie zaktualizować lub usunąć, oryginalne wartości muszą być równe bieżącym wartościom bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image6.png))

Istnieją różne podejścia do implementowania optymistycznej współbieżności (zobacz "Bromberg" [optymistycznej logiki aktualizacji współbieżności](http://www.eggheadcafe.com/articles/20050719.asp) [,](http://peterbromberg.net/)aby skrócić kilka opcji). Zestaw danych typu ADO.NET zawiera jedną implementację, którą można skonfigurować za pomocą tylko znacznika pola wyboru. Włączenie optymistycznej współbieżności dla TableAdapter w określonym zestawie danych rozszerza instrukcje `UPDATE` TableAdapter i `DELETE` w celu uwzględnienia porównania wszystkich oryginalnych wartości w klauzuli `WHERE`. Poniższa instrukcja `UPDATE`, na przykład aktualizuje nazwę i cenę produktu, tylko wtedy, gdy bieżące wartości bazy danych są równe wartościom, które zostały pierwotnie pobrane podczas aktualizowania rekordu w widoku GridView. Parametry `@ProductName` i `@UnitPrice` zawierają nowe wartości wprowadzone przez użytkownika, natomiast `@original_ProductName` i `@original_UnitPrice` zawierają wartości, które zostały początkowo załadowane do widoku GridView po kliknięciu przycisku Edytuj:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Ta instrukcja `UPDATE` została uproszczona w celu zapewnienia czytelności. W tej sytuacji `UnitPrice` się, że klauzula `WHERE` będzie bardziej zastosowana, ponieważ `UnitPrice` może zawierać `NULL` s i sprawdzać, czy `NULL = NULL` zawsze zwraca wartość false (zamiast tego należy użyć `IS NULL`).

Oprócz korzystania z innej bazowej instrukcji `UPDATE`, skonfigurowanie TableAdapter do korzystania z optymistycznej współbieżności również modyfikuje sygnaturę metod bezpośrednich usługi DB. Odwołaj się z naszego pierwszego samouczka, [*tworząc warstwę dostępu do danych*](../introduction/creating-a-data-access-layer-cs.md), że metody usługi DB Direct były tymi, które akceptują listę wartości skalarnych jako parametry wejściowe (a nie z jednoznacznie określonym wystąpieniem DataRow lub DataTable). W przypadku korzystania z optymistycznej współbieżności metody `Update()` i `Delete()` bazy danych Direct zawierają również parametry wejściowe dla oryginalnych wartości. Ponadto kod w logiki biznesowej do użycia wzorca aktualizacji wsadowej (`Update()` przeciążeń metody, które akceptują wiersze Datai DataTable zamiast wartości skalarnych), również muszą być zmienione.

Zamiast rozwijać nasze istniejące DAL TableAdapters, aby użyć optymistycznej współbieżności (co wymagałoby zmiany LOGIKI BIZNESOWEJu na dołączenie), należy utworzyć nowy typ zestawu danych o nazwie `NorthwindOptimisticConcurrency`, do którego zostanie dodany `Products` TableAdapter, który używa optymistycznej współbieżności. Poniżej przedstawiono Tworzenie klasy warstwy logiki biznesowej `ProductsOptimisticConcurrencyBLL`, która ma odpowiednie modyfikacje w celu obsługi optymistycznej współbieżności DAL. Po podaniu tego podstawę będzie można utworzyć stronę ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Krok 2. Tworzenie warstwy dostępu do danych, która obsługuje optymistyczne współbieżność

Aby utworzyć nowy typ zestawu danych, kliknij prawym przyciskiem myszy folder `DAL` w folderze `App_Code` i Dodaj nowy zestaw danych o nazwie `NorthwindOptimisticConcurrency`. Tak jak w pierwszym samouczku, spowoduje to dodanie nowego TableAdapter do określonego zestawu danych, automatyczne uruchomienie Kreatora konfiguracji TableAdapter. Na pierwszym ekranie zostanie wyświetlony monit o określenie bazy danych, z którą ma zostać nawiązane połączenie — Połącz się z tą samą bazą danych Northwind przy użyciu ustawienia `NORTHWNDConnectionString` z `Web.config`.

[![połączyć się z tą samą bazą danych Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Rysunek 3**. Łączenie się z tą samą bazą danych Northwind ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image9.png))

Następnie zostanie wyświetlony monit z pytaniem o sposób wykonywania zapytań dotyczących danych: za pomocą instrukcji SQL ad hoc, nowej procedury składowanej lub istniejącej procedury składowanej. Ponieważ używamy zapytań SQL ad hoc w oryginalnym DAL, należy użyć tej opcji również w tym miejscu.

[![określić dane do pobrania przy użyciu instrukcji SQL ad hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Rysunek 4**. Określanie danych do pobrania przy użyciu instrukcji SQL ad hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image12.png))

Na poniższym ekranie Wprowadź kwerendę SQL, która ma zostać użyta do pobrania informacji o produkcie. Użyjmy dokładnie tego samego zapytania SQL, które jest używane dla `Products` TableAdapter z oryginalnego DALu, który zwraca wszystkie kolumny `Product` wraz z nazwami dostawcy i kategorii produktu:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]

[![używać tego samego zapytania SQL z produktów TableAdapter w oryginalnym DAL](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Rysunek 5**. Użyj tego samego zapytania SQL z `Products` TableAdapter w oryginalnym dal (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image15.png))

Przed przejściem na następny ekran kliknij przycisk Opcje zaawansowane. Aby ta TableAdapter korzystała z optymistycznej kontroli współbieżności, po prostu zaznacz pole wyboru Użyj optymistycznej współbieżności.

[![włączyć optymistyczną kontrolę współbieżności, sprawdzając &quot;użyć pola wyboru optymistyczne współbieżność&quot;](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Ilustracja 6**. Włącz optymistyczną kontrolę współbieżności, zaznaczając pole wyboru "Użyj optymistycznej współbieżności" ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image18.png))

Na koniec wskaż, że TableAdapter powinien używać wzorców dostępu do danych, które wypełniają DataTable i zwracają DataTable; należy również wskazać, że powinny być tworzone metody DB Direct. Zmień nazwę metody dla zwracanego wzorca DataTable z GetData na getProducts, tak aby odzwierciedlała konwencje nazewnictwa używane w oryginalnym DAL.

[![TableAdapter używać wszystkich wzorców dostępu do danych](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Rysunek 7**. Korzystanie z TableAdapter wszystkich wzorców dostępu do danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image21.png))

Po zakończeniu działania kreatora Projektant obiektów DataSet będzie zawierać silnie wpisaną `Products` DataTable i TableAdapter. Poświęć chwilę na zmianę nazwy elementu DataTable z `Products` na `ProductsOptimisticConcurrency`, co można zrobić, klikając prawym przyciskiem myszy pasek tytułu tabeli DataTable i wybierając polecenie Zmień nazwę z menu kontekstowego.

[![element DataTable i TableAdapter zostały dodane do określonego zestawu danych](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Ilustracja 8**. element DataTable i TableAdapter zostały dodane do określonego zestawu danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image24.png))

Aby zobaczyć różnice między `UPDATE` i `DELETE` zapytaniami między `ProductsOptimisticConcurrency` TableAdapter (które korzystają z optymistycznej współbieżności) i produktów TableAdapter (które nie są), kliknij TableAdapter i przejdź do okno Właściwości. We właściwościach `DeleteCommand` i `UpdateCommand` właściwości `CommandText` można zobaczyć rzeczywistą składnię SQL, która jest wysyłana do bazy danych, gdy wywoływana jest metoda aktualizacji lub usuwania DAL. W przypadku `ProductsOptimisticConcurrency` TableAdapter użyto instrukcji `DELETE`:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

W związku z tym, że `DELETE` produktu TableAdapter w oryginalnym DAL jest znacznie prostsze:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Jak widać, klauzula `WHERE` w instrukcji `DELETE` dla TableAdapter, która używa optymistycznej współbieżności, zawiera porównanie między każdą z istniejących wartości kolumn w tabeli `Product` i oryginalnymi wartościami w chwili ostatniego wypełnienia GridView (lub DetailsView lub FormView). Ponieważ wszystkie pola inne niż `ProductID`, `ProductName`i `Discontinued` mogą mieć wartości `NULL`, dodatkowe parametry i testy są uwzględniane w celu poprawnego porównania wartości `NULL` w klauzuli `WHERE`.

Nie będziemy dodawać żadnych dodatkowych tabel danych do optymistycznej z obsługą współbieżności dla tego samouczka, ponieważ nasza strona ASP.NET będzie zawierać tylko aktualizacje i usuwanie informacji o produkcie. Jednak nadal trzeba dodać metodę `GetProductByProductID(productID)` do `ProductsOptimisticConcurrency` TableAdapter.

Aby to osiągnąć, kliknij prawym przyciskiem myszy pasek tytułu TableAdapter (obszar bezpośrednio nad `Fill` i `GetProducts` nazw metod) i wybierz polecenie Dodaj zapytanie z menu kontekstowego. Spowoduje to uruchomienie Kreatora konfiguracji zapytania TableAdapter. Podobnie jak w przypadku konfiguracji początkowej TableAdapter, należy wybrać metodę `GetProductByProductID(productID)` przy użyciu instrukcji SQL ad hoc (patrz rysunek 4). Ponieważ metoda `GetProductByProductID(productID)` zwraca informacje o określonym produkcie, należy wskazać, że to zapytanie jest typu zapytania `SELECT`, który zwraca wiersze.

[![oznaczyć typ zapytania jako &quot;SELECT, który zwraca wiersze&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Rysunek 9**. Oznacz typ zapytania jako "`SELECT`, która zwraca wiersze" ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image27.png))

Na następnym ekranie zostanie wyświetlony monit o zapytanie SQL, które ma być używane, z wstępnie załadowana zapytania domyślnego TableAdapter. Uzupełnij istniejące zapytanie, aby uwzględnić klauzulę `WHERE ProductID = @ProductID`, jak pokazano na rysunku 10.

[![dodać klauzulę WHERE do wstępnie załadowanego zapytania w celu zwrócenia określonego rekordu produktu](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Rysunek 10**. Dodaj klauzulę `WHERE` do wstępnie załadowanego zapytania w celu zwrócenia określonego rekordu produktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image30.png))

Na koniec Zmień nazwy wygenerowanej metody na `FillByProductID` i `GetProductByProductID`.

[![zmienić nazwy metod na FillByProductID i GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Ilustracja 11**. zmiana nazw metod na `FillByProductID` i `GetProductByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image33.png))

Po ukończeniu pracy tego kreatora TableAdapter teraz zawiera dwie metody pobierania danych: `GetProducts()`, które zwracają *wszystkie* produkty; i `GetProductByProductID(productID)`, która zwraca określony produkt.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Krok 3. Tworzenie warstwy logiki biznesowej dla optymistycznej DAL z obsługą współbieżności

Nasze istniejące klasy `ProductsBLL` zawierają przykłady użycia zarówno wzorca aktualizacji wsadowych, jak i bezpośrednich. Metody `AddProduct` i przeciążenia `UpdateProduct` używają wzorca aktualizacji wsadowej, przekazując wystąpienie `ProductRow` do metody aktualizacji TableAdapter. Metoda `DeleteProduct`, z drugiej strony, używa wzorca DB Direct, wywołując metodę `Delete(productID)` TableAdapter.

Przy użyciu nowych `ProductsOptimisticConcurrency` TableAdapter metody DB Direct wymagają teraz przekazywania oryginalnych wartości. Na przykład Metoda `Delete` teraz oczekuje dziesięciu parametrów wejściowych: oryginalnych `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`i `Discontinued`. Używa tych dodatkowych wartości parametrów wejściowych w klauzuli `WHERE` instrukcji `DELETE` wysyłanej do bazy danych, usuwając określony rekord tylko wtedy, gdy bieżące wartości są mapowane na oryginalne.

Gdy sygnatura metody `Update`ej użyta w wzorcu aktualizacji wsadowych nie zmieniła się, kod wymagany do rejestrowania oryginalnych i nowych wartości ma wartość. W związku z tym zamiast próbować używać DAL optymistycznej współbieżności z istniejącą klasą `ProductsBLL`, Utwórzmy nową klasę warstwy logiki biznesowej do pracy z naszym nowym DALem.

Dodaj klasę o nazwie `ProductsOptimisticConcurrencyBLL` do folderu `BLL` w folderze `App_Code`.

![Dodawanie klasy ProductsOptimisticConcurrencyBLL do folderu LOGIKI biznesowej](implementing-optimistic-concurrency-vb/_static/image34.png)

**Ilustracja 12**. dodawanie klasy `ProductsOptimisticConcurrencyBLL` do folderu logiki biznesowej

Następnie Dodaj następujący kod do klasy `ProductsOptimisticConcurrencyBLL`:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Zwróć uwagę, że instrukcja using `NorthwindOptimisticConcurrencyTableAdapters` powyżej początku deklaracji klasy. Przestrzeń nazw `NorthwindOptimisticConcurrencyTableAdapters` zawiera klasę `ProductsOptimisticConcurrencyTableAdapter`, która dostarcza metody DAL. Ponadto przed deklaracją klasy znajdziesz atrybut `System.ComponentModel.DataObject`, który instruuje program Visual Studio, aby dołączył tę klasę do listy rozwijanej Kreatora elementu ObjectDataSource.

Właściwość `Adapter` `ProductsOptimisticConcurrencyBLL`zapewnia szybki dostęp do wystąpienia klasy `ProductsOptimisticConcurrencyTableAdapter` i jest zgodna ze wzorcem używanym w oryginalnych klasach LOGIKI biznesowej (`ProductsBLL`, `CategoriesBLL`itd.). Na koniec Metoda `GetProducts()` po prostu wywołuje metodę `GetProducts()` DAL i zwraca obiekt `ProductsOptimisticConcurrencyDataTable` wypełniony wystąpieniem `ProductsOptimisticConcurrencyRow` dla każdego rekordu produktu w bazie danych.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Usuwanie produktu przy użyciu wzorca DB Direct z optymistyczną współbieżnością

W przypadku korzystania ze wzorca DB Direct dla DAL, który używa optymistycznej współbieżności, metody muszą być przenoszone do nowych i oryginalnych wartości. W przypadku usuwania nie ma nowych wartości, dlatego tylko oryginalne wartości muszą zostać przesłane. W naszym LOGIKI biznesowej należy zaakceptować wszystkie oryginalne parametry jako parametry wejściowe. Przyjrzyjmy się metodzie `DeleteProduct` w klasie `ProductsOptimisticConcurrencyBLL` za pomocą metody DB Direct. Oznacza to, że ta metoda musi przyjąć wszystkie dziesięć pól danych produktu jako parametry wejściowe i przekazać je do DAL, jak pokazano w poniższym kodzie:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Jeśli pierwotne wartości — te wartości, które zostały ostatnio załadowane do GridView (lub DetailsView lub FormView) — różnią się od wartości w bazie danych, gdy użytkownik kliknie przycisk Usuń, klauzula `WHERE` nie będzie zgodna z żadnym rekordem bazy danych i nie ma wpływu na rekordy. W związku z tym Metoda `Delete` TableAdapter zwróci `0`, a metoda `DeleteProduct` LOGIKI biznesowej zwróci `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aktualizowanie produktu przy użyciu wzorca aktualizacji usługi Batch z optymistyczną współbieżnością

Jak wspomniano wcześniej, TableAdapter Metoda `Update` dla wzorca aktualizacji wsadowej ma ten sam podpis metody, niezależnie od tego, czy jest stosowana Optymistyczna współbieżność. Mianowicie, Metoda `Update` oczekuje obiektu DataRow, tablicy danych, elementu DataTable lub określonego typu. Nie ma dodatkowych parametrów wejściowych do określenia oryginalnych wartości. Jest to możliwe, ponieważ element DataTable śledzi pierwotne i zmodyfikowane wartości dla swoich elementów DataRow. Gdy DAL wystawia swoją instrukcję `UPDATE`, parametry `@original_ColumnName` są wypełniane oryginalnymi wartościami, natomiast parametry `@ColumnName` są wypełniane wartościami zmodyfikowanymi przez element DataRow.

W klasie `ProductsBLL` (która korzysta z naszego oryginalnego, nieoptymistycznej współbieżności DAL), podczas korzystania ze wzorca aktualizacji wsadowych do aktualizowania informacji o produkcie nasz kod wykonuje następującą sekwencję zdarzeń:

1. Przeczytaj informacje o produkcie bieżącej bazy danych w wystąpieniu `ProductRow` przy użyciu metody `GetProductByProductID(productID)` TableAdapter
2. Przypisz nowe wartości do wystąpienia `ProductRow` z kroku 1
3. Wywołaj metodę `Update` TableAdapter, przekazując wystąpienie `ProductRow`

Ta sekwencja kroków nie będzie jednak prawidłowo obsługiwać optymistycznej współbieżności, ponieważ `ProductRow` wypełnionej w kroku 1 jest wypełniana bezpośrednio z bazy danych, co oznacza, że oryginalne wartości używane przez ten element DataRow są te, które aktualnie istnieją w bazie danych, a nie te, które zostały powiązane z elementem GridView na początku procesu edycji. Zamiast tego w przypadku używania optymistycznej DAL z obsługą współbieżności należy zmienić przeciążenia metody `UpdateProduct`, aby wykonać następujące czynności:

1. Przeczytaj informacje o produkcie bieżącej bazy danych w wystąpieniu `ProductsOptimisticConcurrencyRow` przy użyciu metody `GetProductByProductID(productID)` TableAdapter
2. Przypisz *oryginalne* wartości do wystąpienia `ProductsOptimisticConcurrencyRow` z kroku 1
3. Wywołaj metodę `AcceptChanges()` `ProductsOptimisticConcurrencyRow` wystąpienia, która instruuje element DataRow, że jego bieżące wartości są "oryginalnymi"
4. Przypisz *nowe* wartości do wystąpienia `ProductsOptimisticConcurrencyRow`
5. Wywołaj metodę `Update` TableAdapter, przekazując wystąpienie `ProductsOptimisticConcurrencyRow`

Krok 1 odczytuje wszystkie bieżące wartości bazy danych dla określonego rekordu produktu. Ten krok jest zbędny w ramach przeciążenia `UpdateProduct`, które aktualizuje *wszystkie* kolumny produktu (ponieważ te wartości są zastępowane w kroku 2), ale są niezbędne dla tych przeciążeń, w których tylko podzbiór wartości kolumn jest przenoszona jako parametry wejściowe. Po przypisaniu oryginalnych wartości do wystąpienia `ProductsOptimisticConcurrencyRow`, wywoływana jest metoda `AcceptChanges()`, która oznacza bieżące wartości DataRow jako pierwotne wartości, które mają być używane w parametrach `@original_ColumnName` w instrukcji `UPDATE`. Następnie nowe wartości parametrów są przypisywane do `ProductsOptimisticConcurrencyRow`, a wreszcie Metoda `Update` jest wywoływana, przekazując w elemencie DataRow.

Poniższy kod przedstawia Przeciążenie `UpdateProduct`, które akceptuje wszystkie pola danych produktu jako parametry wejściowe. Chociaż nie pokazano tego tutaj, Klasa `ProductsOptimisticConcurrencyBLL` uwzględniona w pobraniu dla tego samouczka zawiera również Przeciążenie `UpdateProduct`, które akceptuje tylko nazwę i cenę produktu jako parametry wejściowe.

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Krok 4. przekazanie oryginalnych i nowych wartości ze strony ASP.NET do metod LOGIKI biznesowej

Po ukończeniu DAL i LOGIKI biznesowej wszystko to jest w celu utworzenia strony ASP.NET, która może korzystać z optymistycznej logiki współbieżności wbudowanej w System. W odniesieniu do formantu sieci Web danych (GridView, DetailsView lub FormView) musi zapamiętać jego pierwotne wartości, a element ObjectDataSource musi przekazywać oba zestawy wartości do warstwy logiki biznesowej. Ponadto należy skonfigurować stronę ASP.NET, aby bezpiecznie obsługiwać naruszenia współbieżności.

Zacznij od otwarcia strony `OptimisticConcurrency.aspx` w folderze `EditInsertDelete` i dodania widoku GridView do projektanta, ustawiając jej Właściwość `ID` na `ProductsGrid`. W tagu inteligentnym GridView, należy wybrać opcję Utwórz nowy element ObjectDataSource o nazwie `ProductsOptimisticConcurrencyDataSource`. Ponieważ chcemy, aby ten element ObjectDataSource używał DAL, który obsługuje optymistyczne współbieżność, skonfiguruj go tak, aby korzystał z obiektu `ProductsOptimisticConcurrencyBLL`.

[![, że element ObjectDataSource używa obiektu ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Ilustracja 13**. aby element ObjectDataSource używał obiektu `ProductsOptimisticConcurrencyBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image37.png))

Wybierz metody `GetProducts`, `UpdateProduct`i `DeleteProduct` z list rozwijanych w kreatorze. Dla metody UpdateProduct Użyj przeciążenia, które akceptuje wszystkie pola danych produktu.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Konfigurowanie właściwości kontrolki ObjectDataSource

Po zakończeniu działania kreatora znaczniki deklaratywne elementu ObjectDataSource powinny wyglądać następująco:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Jak widać, kolekcja `DeleteParameters` zawiera wystąpienie `Parameter` dla każdego z dziesięciu parametrów wejściowych w metodzie `DeleteProduct` klasy `ProductsOptimisticConcurrencyBLL`. Podobnie kolekcja `UpdateParameters` zawiera wystąpienie `Parameter` dla każdego z parametrów wejściowych w `UpdateProduct`.

Dla tych wcześniejszych samouczków, które dotyczyły modyfikacji danych, usuniemy Właściwość `OldValuesParameterFormatString` elementu ObjectDataSource w tym momencie, ponieważ ta właściwość wskazuje, że metoda LOGIKI biznesowej oczekuje starych (lub oryginalnych) wartości do przekazania, a także nowych wartości. Ponadto ta wartość właściwości wskazuje nazwy parametrów wejściowych dla oryginalnych wartości. Ponieważ przekazujemy oryginalne wartości do LOGIKI biznesowej, *nie usuwaj* tej właściwości.

> [!NOTE]
> Wartość właściwości `OldValuesParameterFormatString` musi być mapowana na nazwy parametrów wejściowych w LOGIKI biznesowej, które oczekują oryginalnych wartości. Ponieważ zostały nazwane te parametry `original_productName`, `original_supplierID`i tak dalej, można pozostawić wartość właściwości `OldValuesParameterFormatString` jako `original_{0}`. Jeśli jednak parametry wejściowe metody LOGIKI biznesowej mają nazwy, takie jak `old_productName`, `old_supplierID`i tak dalej, należy zaktualizować Właściwość `OldValuesParameterFormatString` na `old_{0}`.

Istnieje jedno ustawienie właściwości końcowej, które musi zostać wykonane, aby element ObjectDataSource prawidłowo przeszedł oryginalne wartości do metod LOGIKI biznesowej. Element ObjectDataSource ma [Właściwość ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) , która może być przypisana do [jednej z dwóch wartości](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` — wartość domyślna; nie wysyła oryginalnych wartości do oryginalnych parametrów wejściowych metod LOGIKI biznesowej
- `CompareAllValues` — wysyła oryginalne wartości do metod LOGIKI biznesowej; Wybierz tę opcję, jeśli używasz optymistycznej współbieżności

Poświęć chwilę, aby ustawić właściwość `ConflictDetection` na `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Konfigurowanie właściwości i pól widoku GridView

Po prawidłowym skonfigurowaniu właściwości elementu ObjectDataSource Przyjrzyjmy się naszej uwagi, aby skonfigurować widok GridView. Po pierwsze, ponieważ chcemy, aby GridView obsługiwał edytowanie i usuwanie, kliknij pole wyboru Włącz edytowanie i Włącz usuwanie w tagu inteligentnym GridView. Spowoduje to dodanie elementu CommandField, którego `ShowEditButton` i `ShowDeleteButton` są ustawione jako `true`.

Po powiązaniu z `ProductsOptimisticConcurrencyDataSource` elementem ObjectDataSource, GridView zawiera pole dla każdego z pól danych produktu. Ten widok GridView można edytować, ale środowisko użytkownika jest dowolne, ale akceptowalne. `CategoryID` i `SupplierID` BoundFields będą renderowane jako pola tekstowe, wymagając od użytkownika podania odpowiedniej kategorii i dostawcy jako numerów IDENTYFIKACYJNych. Nie ma żadnego formatowania dla pól liczbowych i nie ma żadnych kontrolek weryfikacji, aby upewnić się, że nazwa produktu została podana oraz że cena jednostkowa, jednostki w magazynie, jednostki w kolejności i wartości na poziomie zmiany kolejności są prawidłowymi wartościami liczbowymi i są większe lub równe na zero.

Zgodnie z opisem w temacie *Dodawanie kontrolek weryfikacji do edycji i wstawiania interfejsów* i dostosowywania samouczków dotyczących *interfejsu modyfikacji danych* , interfejs użytkownika można dostosować, zastępując BoundFields z używanie TemplateField. Zmodyfikowano ten widok GridView i jego interfejs edycji w następujący sposób:

- Usunięto `ProductID`, `SupplierName`i `CategoryName` BoundFields
- Przekonwertowano `ProductName` BoundField na TemplateField i dodano kontrolkę RequiredFieldValidation.
- Przekonwertowano `CategoryID` i `SupplierID` BoundFields na używanie TemplateField i dostosowano interfejs edycji do używania kontrolek DropDownList zamiast pól tekstowych. W tych używanie TemplateField `ItemTemplates`wyświetlane są pola danych `CategoryName` i `SupplierName`.
- Przekonwertowane `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`i `ReorderLevel` BoundFields na używanie TemplateField i dodane kontrolki CompareValidator.

Ze względu na to, jak można wykonać te zadania w poprzednich samouczkach, w tym miejscu chcę wyświetlić ostateczną składnię deklaratywną i pozostawić implementację jako ćwiczenie.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Jesteśmy bardzo blisko, aby przetworzyć w pełni działający przykład. Istnieje jednak kilka subtletiesów, które wywołują się i spowodują problemy. Ponadto nadal potrzebujemy pewnego interfejsu, który ostrzega użytkownika o wystąpieniu naruszenia współbieżności.

> [!NOTE]
> Aby formant sieci Web danych prawidłowo przekazywać oryginalne wartości do elementu ObjectDataSource (które są następnie przekazywane do LOGIKI biznesowej), ważne jest, aby Właściwość `EnableViewState` GridView była ustawiona na `true` (domyślnie). Jeśli wyłączysz stan widoku, oryginalne wartości zostaną utracone po odświeżeniu.

## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Przekazywanie prawidłowych wartości pierwotnych do elementu ObjectDataSource

Istnieje kilka problemów ze sposobem, w jaki widok GridView został skonfigurowany. Jeśli właściwość `ConflictDetection` elementu ObjectDataSource jest ustawiona na wartość `CompareAllValues` (jako nasza), gdy `Update()` elementu ObjectDataSource lub metoda `Delete()` są wywoływane przez GridView (lub DetailsView lub FormView), element ObjectDataSource próbuje skopiować oryginalne wartości elementu GridView do odpowiednich wystąpień `Parameter`. Zapoznaj się z powrotem do rysunku 2, aby poznać graficzną reprezentację tego procesu.

W szczególnym czasie pierwotne wartości elementu GridView są przypisywane wartościom w dwukierunkowych instrukcjach wiązania danych, gdy dane są powiązane z elementem GridView. W związku z tym ważne jest, aby wszystkie wymagane oryginalne wartości były przechwytywane za pośrednictwem dwukierunkowego wiązania danych i że są one dostarczane w formacie konwersji.

Aby dowiedzieć się, dlaczego jest to ważne, zapoznaj się z naszą stroną w przeglądarce. Zgodnie z oczekiwaniami, GridView wyświetla każdy produkt za pomocą przycisku Edytuj i Usuń w kolumnie z lewej strony.

[![produkty są wymienione w widoku GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Ilustracja 14**. produkty są wymienione w widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image40.png))

Jeśli klikniesz przycisk Usuń dla dowolnego produktu, zostanie zgłoszony `FormatException`.

[![próby usunięcia dowolnych wyników z produktu w FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Ilustracja 15**. próba usunięcia wszystkich wyników produktów w `FormatException` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image43.png))

`FormatException` jest wywoływane, gdy element ObjectDataSource próbuje odczytać w pierwotnej wartości `UnitPrice`. Ponieważ `ItemTemplate` ma `UnitPrice` sformatowaną jako waluta (`<%# Bind("UnitPrice", "{0:C}") %>`), zawiera symbol waluty, na przykład $19,95. `FormatException` występuje, ponieważ element ObjectDataSource próbuje przekonwertować ten ciąg na `decimal`. Aby obejść ten problem, mamy kilka opcji:

- Usuń formatowanie waluty z `ItemTemplate`. Oznacza to, że zamiast używać `<%# Bind("UnitPrice", "{0:C}") %>`, wystarczy użyć `<%# Bind("UnitPrice") %>`. Minusem jest to, że cena nie jest już sformatowana.
- Wyświetl `UnitPrice` sformatowaną jako walutę w `ItemTemplate`, ale Użyj słowa kluczowego `Eval`, aby to zrobić. Odwołaj, że `Eval` wykonuje jednokierunkowe powiązanie danych. Nadal musimy podać wartość `UnitPrice` dla oryginalnych wartości, więc nadal będzie potrzebna dwukierunkowa instrukcja DataBinding w `ItemTemplate`, ale można ją umieścić w kontrolce sieci Web etykiety, której Właściwość `Visible` jest ustawiona na `false`. Możemy użyć następującego znacznika w ItemTemplate:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Usuń formatowanie waluty z `ItemTemplate`przy użyciu `<%# Bind("UnitPrice") %>`. W programie obsługi zdarzeń `RowDataBound` GridView program programowo uzyskuje dostęp do kontrolki sieci Web etykieta, w której zostanie wyświetlona wartość `UnitPrice`, i ustawi jej Właściwość `Text` na sformatowaną wersję.
- Pozostaw `UnitPrice` sformatowany jako walutę. W programie obsługi zdarzeń `RowDeleting` GridView Zastąp istniejącą oryginalną wartość `UnitPrice` ($19,95) wartością rzeczywistą dziesiętną przy użyciu `Decimal.Parse`. Znaleźliśmy, jak wykonać coś podobnego w obsłudze zdarzeń `RowUpdating` w przypadku [*obsługi wyjątków logiki biznesowej i dal w samouczku dotyczącym strony ASP.NET*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) .

W przypadku mojego przykładu wybieramy, aby przejść do drugiego podejścia, dodając kontrolkę sieci Web ukrytej etykiety, której Właściwość `Text` to dane dwukierunkowe powiązane z niesformatowaną `UnitPrice` wartością.

Po rozwiązaniu tego problemu spróbuj ponownie kliknąć przycisk Usuń dla każdego produktu. Tym razem otrzymasz `InvalidOperationException`, gdy element ObjectDataSource podejmie próbę wywołania metody `UpdateProduct` LOGIKI biznesowej.

[![element ObjectDataSource nie może znaleźć metody z parametrami wejściowymi, które chce wysłać](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Ilustracja 16**. Element ObjectDataSource nie może znaleźć metody z parametrami wejściowymi, które chce wysłać ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image46.png))

Oglądając komunikat wyjątku, należy wyczyścić, że element ObjectDataSource chce wywołać metodę LOGIKI biznesowej `DeleteProduct`, która zawiera parametry wejściowe `original_CategoryName` i `original_SupplierName`. Wynika to z faktu, że `ItemTemplate` s dla `CategoryID` i `SupplierID` używanie TemplateField obecnie zawiera instrukcje dwukierunkowego wiązania z polami danych `CategoryName` i `SupplierName`. Zamiast tego musimy uwzględnić instrukcje `Bind` z polami danych `CategoryID` i `SupplierID`. Aby to osiągnąć, Zastąp istniejące instrukcje bind instrukcją `Eval`, a następnie Dodaj ukryte kontrolki etykiety, których właściwości `Text` są powiązane z polami danych `CategoryID` i `SupplierID` przy użyciu wiązania dwukierunkowego, jak pokazano poniżej:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Po wprowadzeniu tych zmian teraz można pomyślnie usunąć i edytować informacje o produkcie. W kroku 5 powiesz się, jak sprawdzić, czy są wykryte naruszenia współbieżności. Jednak za kilka minut spróbuj zaktualizować i usunąć kilka rekordów, aby upewnić się, że aktualizacja i usuwanie dla jednego użytkownika działa zgodnie z oczekiwaniami.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Krok 5. Testowanie obsługi optymistycznej współbieżności

Aby upewnić się, że są wykrywane naruszenia współbieżności (zamiast polegania na wykryciu danych), musimy otworzyć dwa okna przeglądarki na tej stronie. W obu wystąpieniach przeglądarki kliknij przycisk Edytuj dla Chai. Następnie w jednej z przeglądarek Zmień nazwę na "Chai herbata" i kliknij przycisk Aktualizuj. Aktualizacja powinna zakończyć się pomyślnie i zwrócić widok GridView do stanu sprzed edycji, z opcją "Chai herbata" jako nową nazwą produktu.

W innym wystąpieniu okna przeglądarki, jednak pole tekstowe Nazwa produktu nadal zawiera "Chai". W tym drugim oknie przeglądarki zaktualizuj `UnitPrice`, aby `25.00`. Bez optymistycznej obsługi współbieżności, kliknięcie pozycji Aktualizuj w drugim wystąpieniu przeglądarki spowoduje zmianę nazwy produktu z powrotem na "Chai", co powoduje zastąpienie zmian wprowadzonych w pierwszym wystąpieniu przeglądarki. Jednak dzięki optymistycznej współbieżności, kliknięcie przycisku Aktualizuj w drugim wystąpieniu przeglądarki skutkuje wystąpieniem [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).

[![, gdy zostanie wykryte naruszenie współbieżności, zostanie zgłoszony wyjątek DBConcurrencyException](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Ilustracja 17**. w przypadku wykrycia naruszenia współbieżności zostanie zgłoszony `DBConcurrencyException` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image49.png))

`DBConcurrencyException` jest generowany tylko wtedy, gdy jest używany wzorzec aktualizacji wsadowych DAL. Wzorzec DB Direct nie zgłasza wyjątku, tylko wskazuje, że nie wpłynęło to na żadne wiersze. Aby to zilustrować, zwróć widok widoku GridView obu wystąpień przeglądarki do stanu sprzed edycji. Następnie w pierwszym wystąpieniu przeglądarki kliknij przycisk Edytuj, a następnie zmień nazwę produktu z "Chai herbata" z powrotem na "Chai", a następnie kliknij przycisk Aktualizuj. W drugim oknie przeglądarki kliknij przycisk Usuń dla Chai.

Po kliknięciu przycisku Usuń Strona zapisuje zwrotnie, a element GridView wywołuje metodę `Delete()` elementu ObjectDataSource, a element ObjectDataSource wywołuje metodę `DeleteProduct` klasy `ProductsOptimisticConcurrencyBLL`, przekazując wartości pierwotne. Oryginalna wartość `ProductName` dla drugiego wystąpienia przeglądarki to "Chai herbata", która nie jest zgodna z bieżącą wartością `ProductName` w bazie danych. W związku z tym instrukcja `DELETE` wystawiona dla bazy danych wpływa na zero wierszy, ponieważ w bazie danych nie ma rekordu, który spełnia klauzulę `WHERE`. Metoda `DeleteProduct` zwraca `false`, a dane elementu ObjectDataSource są ponownie powiązane z elementem GridView.

Z punktu widzenia użytkownika końcowego klikanie przycisku Usuń dla Chai Tea w drugim oknie przeglądarki spowodowało, że ekran jest odtwarzany, a po odwrocie produkt nadal znajduje się na liście "Chai" (Nazwa produktu została zmieniona przez pierwszą przeglądarkę wystąpienie). Jeśli użytkownik kliknie przycisk Usuń ponownie, usuwanie zakończy się pomyślnie, ponieważ oryginalna wartość `ProductName` GridView ("Chai") będzie teraz zgodna z wartością w bazie danych.

W obu tych przypadkach środowisko użytkownika jest daleko od idealnego. Nie chcemy, aby podczas korzystania ze wzorca aktualizacji wsadowych nittył szczegóły dotyczące wyjątku `DBConcurrencyException`-Gritty. Zachowanie podczas korzystania ze wzorca DB Direct jest nieco mylące, ponieważ polecenie nie powiodło się, ale nie było dokładne wskazanie dlaczego.

Aby rozwiązać te dwa problemy, można utworzyć kontrolki sieci Web etykieta na stronie, która zawiera wyjaśnienie przyczyny niepowodzenia aktualizacji lub usunięcia. W przypadku wzorca aktualizacji wsadowych można określić, czy `DBConcurrencyException` wystąpił wyjątek w obsłudze zdarzeń na poziomie po stronie GridView, wyświetlając etykietę ostrzegawczą w razie potrzeby. W przypadku metody DB Direct możemy przeanalizować wartość zwracaną metody LOGIKI biznesowej (która jest `true`, jeśli wpłynęło to na jeden wiersz, `false` w przeciwnym razie) i wyświetlić komunikat informacyjny zgodnie z wymaganiami.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Krok 6. Dodawanie komunikatów informacyjnych i wyświetlanie ich w celu naruszenia współbieżności

W przypadku naruszenia współbieżności zachowanie jest zależne od tego, czy użyto aktualizacji wsadowej DAL czy wzorca usługi DB. Nasz samouczek używa obu wzorców, z wzorcem aktualizacji wsadowej używanego do aktualizacji i wzorca bazy danych bezpośrednio używanego do usuwania. Aby rozpocząć, Dodajmy do naszej strony dwie kontrolki sieci Web z etykietami, które wyjaśniają, że wystąpiło naruszenie współbieżności podczas próby usunięcia lub aktualizacji danych. Ustaw właściwości `Visible` i `EnableViewState` kontrolki etykieta na `false`; spowoduje to ukrycie ich na każdej stronie, z wyjątkiem tych, w których właściwości `Visible` są programowo ustawione na `true`.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Oprócz ustawiania właściwości `Visible`, `EnabledViewState`i `Text`, ustawiam również właściwość `CssClass` na `Warning`, która powoduje, że etykieta będzie wyświetlana w postaci długiej, czerwonej, kursywy, pogrubionej czcionki. Ta klasa `Warning` CSS została zdefiniowana i dodana do stylów. CSS z powrotem w temacie *badanie zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem* samouczka.

Po dodaniu tych etykiet projektant w programie Visual Studio powinien wyglądać podobnie do rysunku 18.

[do strony dodano ![dwóch kontrolek etykiet](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Ilustracja 18**. do strony dodano dwie kontrolki etykiet ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image52.png))

Przy użyciu tych kontrolek sieci Web etykieta na miejscu jesteśmy gotowi do sprawdzenia, jak ustalić, kiedy nastąpiło naruszenie współbieżności, w którym momencie można ustawić właściwość `Visible` odpowiedniej etykiety na `true`, wyświetlając komunikat informacyjny.

## <a name="handling-concurrency-violations-when-updating"></a>Obsługa naruszeń współbieżności podczas aktualizacji

Najpierw przyjrzyjmy się obsłudze naruszeń współbieżności podczas korzystania ze wzorca aktualizacji wsadowych. Ponieważ takie naruszenia wzorca aktualizacji wsadowych powodują zgłoszenie wyjątku `DBConcurrencyException`, musimy dodać kod do naszej strony ASP.NET, aby określić, czy `DBConcurrencyException` wystąpił wyjątek podczas procesu aktualizacji. W takim przypadku powinien zostać wyświetlony komunikat informujący o tym, że zmiany nie zostały zapisane, ponieważ inny użytkownik zmodyfikował te same dane między rozpoczęciem edytowania rekordu a kliknięciem przycisku Aktualizuj.

Zgodnie z opisem w samouczku *dotyczącym obsługi logiki biznesowej i dal na stronie ASP.NET* , takie wyjątki mogą być wykrywane i pomijane w procedurach obsługi zdarzeń na poziomie. W związku z tym musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `RowUpdated` GridView, które sprawdza, czy zgłoszono wyjątek `DBConcurrencyException`. Ta procedura obsługi zdarzeń została przekazana odwołanie do dowolnego wyjątku, który został zgłoszony podczas procesu aktualizacji, jak pokazano w poniższym kodzie programu obsługi zdarzeń:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

W przypadku wyjątku `DBConcurrencyException` ten program obsługi zdarzeń wyświetla kontrolkę etykieta `UpdateConflictMessage` i wskazuje, że wyjątek został obsłużony. Gdy ten kod jest używany, gdy nastąpi naruszenie współbieżności podczas aktualizowania rekordu, zmiany użytkownika są tracone, ponieważ w tym samym czasie nadpisały modyfikacje innego użytkownika. W szczególności widok GridView jest zwracany do stanu sprzed edycji i powiązany z bieżącymi danymi bazy danych. Spowoduje to zaktualizowanie wiersza GridView przy użyciu zmian innych użytkowników, które wcześniej nie były widoczne. Ponadto kontrolka etykieta `UpdateConflictMessage` będzie wyjaśnić użytkownikowi, co się stało. Ta sekwencja zdarzeń została szczegółowo opisana na rysunku 19.

[![aktualizacje użytkownika s zostaną utracone w wyniku naruszenia współbieżności](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Ilustracja 19**. aktualizacje użytkowników są tracone w przypadku naruszenia współbieżności ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image55.png))

> [!NOTE]
> Alternatywnie, zamiast zwracać widok GridView do stanu sprzed edycji, można opuścić widok GridView w stanie edycji, ustawiając właściwość `KeepInEditMode` zadanego obiektu `GridViewUpdatedEventArgs`d na wartość true. W przypadku podjęcia tego podejścia należy jednak upewnić się, że te dane są ponownie powiązane z tym GridView (przez wywołanie metody `DataBind()`), tak aby wartości innych użytkowników zostały załadowane do interfejsu edycji. Kod dostępny do pobrania w tym samouczku zawiera te dwa wiersze kodu w programie obsługi zdarzeń `RowUpdated`. po prostu usuń znaczniki komentarza z tych wierszy kodu, aby zachować GridView w trybie edycji po naruszeniu współbieżności.

## <a name="responding-to-concurrency-violations-when-deleting"></a>Reagowanie na naruszenia współbieżności podczas usuwania

W przypadku wzorca DB Direct nie ma żadnego wyjątku zgłoszonego w wyniku naruszenia współbieżności. Zamiast tego instrukcja Database po prostu nie wpływa na żadne rekordy, ponieważ klauzula WHERE nie jest zgodna z żadnym rekordem. Wszystkie metody modyfikacji danych utworzone w LOGIKI biznesowej zostały zaprojektowane w taki sposób, że zwracają wartość logiczną wskazującą, czy mają one dokładnie jeden rekord. W związku z tym, aby określić, czy nastąpiło naruszenie współbieżności podczas usuwania rekordu, można sprawdzić wartość zwracaną metody `DeleteProduct` LOGIKI biznesowej.

Wartość zwracana dla metody LOGIKI biznesowej można sprawdzić w obsłudze zdarzeń na poziomie elementu ObjectDataSource za pomocą właściwości `ReturnValue` obiektu `ObjectDataSourceStatusEventArgs` przekazaną do procedury obsługi zdarzeń. Ponieważ chcemy określić wartość zwracaną z metody `DeleteProduct`, musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `Deleted` elementu ObjectDataSource. Właściwość `ReturnValue` jest typu `object` i może być `null`, jeśli wyjątek został zgłoszony i metoda została przerwana przed zwróceniem wartości. W związku z tym należy najpierw upewnić się, że właściwość `ReturnValue` nie jest `null` i jest wartością logiczną. Przy założeniu, że ta kontrola przebiega pomyślnie, zostanie wyświetlona `DeleteConflictMessage` kontrolka etykiety, jeśli `ReturnValue` jest `false`. Można to osiągnąć przy użyciu następującego kodu:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

W przypadku naruszenia współbieżności żądanie usunięcia użytkownika zostało anulowane. Widok GridView zostanie odświeżony, pokazując zmiany, które wystąpiły w tym rekordzie między czasem załadowania strony przez użytkownika a kliknięciem przycisku Usuń. W przypadku takiego naruszenia transpires wyświetlana jest etykieta `DeleteConflictMessage`, co wyjaśnia, co się stało (Zobacz Rysunek 20).

[![Usuwanie użytkownika zostało anulowane w wyniku naruszenia współbieżności](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Ilustracja 20**. usunięcie użytkownika s zostało anulowane w wyniku naruszenia współbieżności ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image58.png))

## <a name="summary"></a>Podsumowanie

Możliwości naruszeń współbieżności istnieją w każdej aplikacji, która pozwala wielu równoczesnym użytkownikom aktualizować lub usuwać dane. Jeśli takie naruszenia nie są uwzględniane w przypadku, gdy dwaj użytkownicy jednocześnie zaktualizują te same dane, w ostatnim zapisie "WINS" zastąpią zmiany wprowadzone przez innych użytkowników. Deweloperzy mogą również zaimplementować optymistyczną lub pesymistyczną kontrolę współbieżności. Optymistyczna kontrola współbieżności zakłada, że naruszenia współbieżności są rzadko i po prostu nie zezwala na polecenie Update lub DELETE, które mogłoby stanowić naruszenie współbieżności. W ramach pesymistycznej kontroli współbieżności założono, że naruszenia współbieżności są częste i po prostu odrzucenie polecenia Update lub DELETE jednego użytkownika nie jest akceptowalne. Dzięki pesymistycznej kontroli współbieżności, aktualizacja rekordu wymaga blokowania, co uniemożliwia innym użytkownikom modyfikowanie lub usuwanie rekordu, gdy jest on zablokowany.

Typ zestaw danych w programie .NET zawiera funkcje umożliwiające obsługę optymistycznej kontroli współbieżności. W szczególności instrukcje `UPDATE` i `DELETE` wystawione dla bazy danych obejmują wszystkie kolumny tabeli, a tym samym zagwarantowanie, że aktualizacja lub usunięcie zostanie wykonane tylko wtedy, gdy bieżące dane rekordu są zgodne z oryginalnymi danymi, które użytkownik musiał podczas przeprowadzania aktualizacji lub usuwania. Po skonfigurowaniu DAL do obsługi optymistycznej współbieżności należy zaktualizować metody LOGIKI biznesowej. Ponadto strona ASP.NET, która wywołuje do LOGIKI biznesowej, musi być skonfigurowana tak, aby element ObjectDataSource pobierał pierwotne wartości z jego formantu sieci Web danych i przekazuje je do LOGIKI biznesowej.

Jak zostało to opisane w tym samouczku, wdrożenie optymistycznej kontroli współbieżności w aplikacji sieci Web ASP.NET obejmuje aktualizację DAL i LOGIKI biznesowej oraz dodanie obsługi na stronie ASP.NET. Bez względu na to, czy ta dodana praca jest poinwestowanym czasem i nakładem pracy, zależy od aplikacji. Jeśli użytkownik nie ma często równoczesnych aktualizacji danych lub dane, które są aktualizowane, różnią się od siebie nawzajem, kontrola współbieżności nie jest problemem z kluczem. Jeśli jednak planujesz pracę wielu użytkowników w Twojej lokacji z tymi samymi danymi, kontrola współbieżności może pomóc zapobiec aktualizacji lub usunięciu z przypadkowego przez jednego użytkownika.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](customizing-the-data-modification-interface-vb.md)
> [dalej](adding-client-side-confirmation-when-deleting-vb.md)
