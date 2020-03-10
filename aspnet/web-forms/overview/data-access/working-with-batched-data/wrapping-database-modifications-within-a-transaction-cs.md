---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Zawijanie modyfikacji bazy danych w ramachC#transakcji () | Microsoft Docs
author: rick-anderson
description: Ten samouczek to pierwsze z czterech, które sprawdzają, jak aktualizować, usuwać i wstawiać partie danych. W tym samouczku dowiesz się, w jaki sposób transakcje bazy danych zezwalają...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: da69e466a5b506b869dc8fc0624f3e6a479199a8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78604336"
---
# <a name="wrapping-database-modifications-within-a-transaction-c"></a>Opakowywanie modyfikacji bazy danych w ramach transakcji (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) lub [Pobierz plik PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Ten samouczek to pierwsze z czterech, które sprawdzają, jak aktualizować, usuwać i wstawiać partie danych. W tym samouczku dowiesz się, jak transakcje bazy danych umożliwiają przeprowadzanie modyfikacji wsadowych jako operacji niepodzielnych, co gwarantuje, że wszystkie kroki zakończą się pomyślnie lub wszystkie kroki zakończą się niepowodzeniem.

## <a name="introduction"></a>Wprowadzenie

W miarę jak zaczynamy od [omówienia samouczka wstawiania, aktualizowania i usuwania danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , widok GridView zawiera wbudowaną obsługę funkcji edytowania i usuwania na poziomie wiersza. Za pomocą kilku kliknięć myszą można utworzyć interfejs zaawansowanej modyfikacji danych bez pisania wiersza kodu, o ile będziesz mieć zawartość z edytowaniem i usuwaniem w poszczególnych wierszach. Jednak w niektórych scenariuszach jest to niewystarczające i musimy udostępnić użytkownikom możliwość edytowania lub usuwania partii rekordów.

Na przykład większość klientów poczty e-mail opartych na sieci Web używa siatki do wyświetlania wszystkich komunikatów, w których każdy wiersz zawiera pole wyboru wraz z informacjami dotyczącymi wiadomości e-mail (podmiot, nadawca itd.). Ten interfejs umożliwia użytkownikowi usunięcie wielu komunikatów, sprawdzając je, a następnie klikając przycisk Usuń wybrane wiadomości. Interfejs edytowania partii jest idealny w sytuacjach, w których użytkownicy często edytują wiele różnych rekordów. Zamiast wymuszać, aby użytkownik klikał polecenie Edytuj, wprowadzić zmiany, a następnie kliknąć przycisk Aktualizuj dla każdego rekordu, który należy zmodyfikować, interfejs edycji wsadowej renderuje każdy wiersz za pomocą interfejsu edycji. Użytkownik może szybko zmodyfikować zestaw wierszy, które należy zmienić, a następnie zapisać te zmiany, klikając przycisk Aktualizuj wszystko. W tym zestawie samouczków sprawdzimy, jak tworzyć interfejsy umożliwiające Wstawianie, edytowanie i usuwanie partii danych.

Podczas wykonywania operacji wsadowych ważne jest, aby określić, czy powinno być możliwe, aby niektóre operacje w partii działały prawidłowo, podczas gdy inne zakończą się niepowodzeniem. Rozważ usunięcie interfejsu z grupy — co powinno się zdarzyć, jeśli pierwszy wybrany rekord zostanie usunięty pomyślnie, ale drugi z nich nie powiedzie się z powodu naruszenia ograniczenia klucza obcego? Czy po usunięciu pierwszego rekordu s należy wykonać wycofywanie lub czy jest on akceptowalny do usunięcia pierwszego rekordu?

Jeśli operacja wsadowa ma być traktowana jako [operacja niepodzielna](http://en.wikipedia.org/wiki/Atomic_operation), po wykonaniu jednej z tych czynności wszystkie kroki lub wszystkie kroki zakończą się niepowodzeniem, należy rozszerzyć warstwę dostępu do danych w celu uwzględnienia obsługi [transakcji bazy danych](http://en.wikipedia.org/wiki/Database_transaction). Transakcje bazy danych gwarantują niepodzielność zestawu `INSERT`, `UPDATE`i `DELETE` wykonanych w ramach parasola transakcji i są funkcją obsługiwaną przez większość nowoczesnych systemów baz danych.

W tym samouczku dowiesz się, jak zwiększyć DAL do korzystania z transakcji bazy danych. Kolejne samouczki zapoznają się z implementacją stron sieci Web służących do wstawiania, aktualizowania i usuwania interfejsów przez operacje wsadowe. Zacznij korzystać z aplikacji.

> [!NOTE]
> Gdy modyfikujesz dane w transakcji wsadowej, niepodzielność nie zawsze jest konieczna. W niektórych scenariuszach może być możliwe zaakceptowanie niektórych modyfikacji danych, a inne w tej samej partii nie powiodą się, na przykład podczas usuwania zestawu wiadomości e-mail z internetowego klienta poczty e-mail. Jeśli wystąpi błąd bazy danych w połowie przez proces usuwania, prawdopodobnie jest on niedozwolony, ponieważ te rekordy przetworzone bez błędów pozostaną usunięte. W takich przypadkach DAL nie musi być modyfikowana do obsługi transakcji bazy danych. Istnieją inne scenariusze operacji wsadowych, jednak w przypadku których niepodzielność jest istotna. Gdy klient przenosi środki z jednego konta bankowego do innego, należy wykonać dwie operacje: środki muszą zostać odjęte od pierwszego konta, a następnie dodane do drugiego. Podczas gdy Bank nie ma na uwadze, że pierwszy krok zakończy się powodzeniem, ale drugi krok nie powiedzie się, jego klienci rozumieją się jako niebędące. Zachęcam do pracy z tym samouczkiem i implementowania ulepszeń DAL w celu obsługi transakcji bazy danych, nawet jeśli nie planujesz używania ich w partii Wstawianie, aktualizowanie i usuwanie interfejsów, które będziemy tworzyć w następujących trzech samouczkach.

## <a name="an-overview-of-transactions"></a>Przegląd transakcji

Większość baz danych obejmuje obsługę *transakcji*, co umożliwia grupowanie wielu poleceń bazy danych w pojedynczą jednostkę logiczną pracy. Polecenia bazy danych składające się na transakcję są gwarantowane jako niepodzielne, co oznacza, że wszystkie polecenia będą kończyć się niepowodzeniem lub wszystkie z nich zakończą się pomyślnie.

Ogólnie rzecz biorąc, transakcje są implementowane za pośrednictwem instrukcji SQL przy użyciu następującego wzorca:

1. Wskaż początek transakcji.
2. Wykonaj instrukcje SQL, które składają się na transakcję.
3. Jeśli wystąpi błąd w jednej z instrukcji w kroku 2, wycofaj tę transakcję.
4. Jeśli wszystkie instrukcje z kroku 2 zakończą pracę bez błędu, Zatwierdź transakcję.

Instrukcje SQL służące do tworzenia, zatwierdzania i wycofywania transakcji można wprowadzać ręcznie podczas pisania skryptów SQL lub tworzenia procedur składowanych lub poprzez programowe oznaczanie przy użyciu ADO.NET lub klas w [przestrzeni nazw`System.Transactions`](https://msdn.microsoft.com/library/system.transactions.aspx). W tym samouczku sprawdzimy tylko Zarządzanie transakcjami przy użyciu usługi ADO.NET. W przyszłym samouczku dowiesz się, jak używać procedur składowanych w warstwie dostępu do danych, po czym przepoznajemy instrukcje SQL dotyczące tworzenia, wycofywania i zatwierdzania transakcji. W międzyczasie zapoznaj się z tematem [Zarządzanie transakcjami w SQL Server procedury składowane](http://www.4guysfromrolla.com/webtech/080305-1.shtml) , aby uzyskać więcej informacji.

> [!NOTE]
> [Klasa`TransactionScope`](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) w przestrzeni nazw `System.Transactions` umożliwia deweloperom programistyczne Zawijanie szeregu instrukcji w ramach transakcji i obejmuje obsługę złożonych transakcji obejmujących wiele źródeł, takich jak dwie różne bazy danych, a nawet heterogeniczne typy magazynów danych, takie jak baza danych Microsoft SQL Server, baza danych Oracle i usługa sieci Web. Zamierzam używać transakcji ADO.NET dla tego samouczka, a nie klasy `TransactionScope`, ponieważ ADO.NET jest bardziej szczegółowy dla transakcji bazy danych i, w wielu przypadkach, jest znacznie mniej obciążający zasoby. Ponadto w niektórych scenariuszach Klasa `TransactionScope` korzysta z usługi Microsoft Distributed Transaction Coordinator (MSDTC). Problemy związane z konfiguracją, implementacją i wydajnością w ramach usługi MSDTC to raczej wyspecjalizowany i zaawansowany temat oraz wykraczający poza zakres tych samouczków.

Podczas pracy z dostawcą SqlClient w programie ADO.NET transakcje są inicjowane za pomocą wywołania [metody`BeginTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx) [klasy`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) , która zwraca [obiekt`SqlTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Instrukcje modyfikacji danych, które korzeń transakcji, są umieszczane w bloku `try...catch`. Jeśli wystąpi błąd w instrukcji w bloku `try`, wykonanie przenosi do bloku `catch`, w którym transakcja może zostać wycofana za pośrednictwem [metody `SqlTransaction``Rollback`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)obiektu. Jeśli wszystkie instrukcje zakończą się pomyślnie, wywołanie metody `SqlTransaction` Object s [`Commit`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) na końcu bloku `try` powoduje zatwierdzenie transakcji. Poniższy fragment kodu ilustruje ten wzorzec. Zobacz temat [zachowywanie spójności bazy danych za pomocą transakcji](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) , aby uzyskać dodatkową składnię i przykłady użycia transakcji z ADO.NET.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Domyślnie TableAdapters w określonym zestawie danych nie korzysta z transakcji. Aby zapewnić pomoc techniczną dla transakcji, musimy rozszerzyć klasy TableAdapter w celu uwzględnienia dodatkowych metod, które używają powyższego wzorca do wykonywania serii instrukcji modyfikacji danych w zakresie transakcji. W kroku 2 zobaczymy, jak za pomocą klas częściowych dodać te metody.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Krok 1. Tworzenie pracy z danymi wsadowymi na stronach sieci Web

Przed rozpoczęciem eksplorowania sposobu rozszerzania DAL o obsługę transakcji bazy danych poczekaj chwilę na utworzenie stron sieci Web ASP.NET, które będą potrzebne w tym samouczku, oraz trzy poniższe informacje. Zacznij od dodania nowego folderu o nazwie `BatchData`, a następnie Dodaj następujące strony ASP.NET, kojarząc każdą stronę ze stroną wzorcową `Site.master`.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`

![Dodaj strony ASP.NET dla samouczków związanych z kontrolki SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Rysunek 1**. dodawanie stron ASP.NET dla samouczków związanych z kontrolki SqlDataSource

Podobnie jak w przypadku innych folderów, `Default.aspx` będzie używać kontrolki użytkownika `SectionLevelTutorialListing.ascx` do wyświetlania samouczków w sekcji. W związku z tym należy dodać tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na stronę widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))

Na koniec Dodaj te cztery strony jako wpisy do pliku `Web.sitemap`. W tym celu należy dodać następujące znaczniki po dostosowaniu `<siteMapNode>`mapy witryny:

[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy do pracy z samouczkami danych wsadowych.

![Mapa witryny zawiera teraz wpisy umożliwiające pracę z samouczkami danych wsadowych](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Rysunek 3**. Mapa witryny zawiera teraz wpisy dotyczące pracy z samouczkami danych wsadowych

## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Krok 2: aktualizowanie warstwy dostępu do danych w celu obsługi transakcji bazy danych

Zgodnie z opisem w pierwszym samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md), typ zestawu danych w naszym dal składa się z tabel DataTables i TableAdapters. Tabele danych przechowują dane, podczas gdy TableAdapters udostępnia funkcje odczytujące dane z bazy danych do tabel DataTables, aby zaktualizować bazę danych przy użyciu zmian wprowadzonych w tabelach danych i tak dalej. Odwołaj się, że TableAdapters udostępnia dwa wzorce do aktualizowania danych, które są określane jako aktualizacja wsadowa i baza danych. Ze wzorcem aktualizacji wsadowych TableAdapter jest przenoszona do zestawu danych, DataTable lub kolekcji DataRows. Te dane są wyliczane i dla każdego wstawionego, zmodyfikowanego lub usuniętego wiersza jest wykonywane `InsertCommand`, `UpdateCommand`lub `DeleteCommand`. W przypadku wzorca DB-Direct TableAdapter zamiast tego przekazuje wartości kolumn niezbędnych do wstawiania, aktualizowania lub usuwania pojedynczego rekordu. Metoda wzorca DB Direct używa tych wartości zakończono do wykonywania odpowiednich instrukcji `InsertCommand`, `UpdateCommand`lub `DeleteCommand`.

Bez względu na używany wzorzec aktualizacji TableAdapters automatycznie generowane metody nie używają transakcji. Domyślnie każdy INSERT, Update lub DELETE wykonywany przez TableAdapter jest traktowany jako pojedyncza operacja dyskretna. Załóżmy na przykład, że wzorzec DB-Direct jest używany przez jakiś kod w LOGIKI biznesowej do wstawiania dziesięciu rekordów do bazy danych. Ten kod wywoła metodę TableAdapter s `Insert` dziesięć razy. Jeśli pierwsze pięć wstawek zakończyło się pomyślnie, ale szósty z nich spowodowało wyjątek, pierwsze pięć wstawionych rekordów pozostanie w bazie danych. Podobnie, Jeśli wzorzec aktualizacji wsadowych jest używany do wykonywania operacji wstawiania, aktualizacji i usuwania do wstawionych, zmodyfikowanych i usuniętych wierszy w elemencie DataTable, jeśli pierwsze kilka modyfikacji zakończyło się powodzeniem, ale później wystąpił błąd, te wcześniejsze modyfikacje, które Zakończono pozostanie w bazie danych.

W niektórych scenariuszach chcemy zapewnić niepodzielność w szeregu modyfikacji. Aby to osiągnąć, należy ręcznie rozłożyć TableAdapter przez dodanie nowych metod, które wykonują `InsertCommand`, `UpdateCommand`i `DeleteCommand` s w ramach parasola transakcji. W obszarze [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) wykorzystano [klasy częściowe](http://en.wikipedia.org/wiki/Partial_type) w celu rozbudowania funkcji tabel danych w danym typie. Ta technika może być również używana z TableAdapters.

Typ zestawu danych `Northwind.xsd` znajduje się w podfolderze `App_Code` folderu s `DAL`. Utwórz podfolder w folderze `DAL` o nazwie `TransactionSupport` i Dodaj nowy plik klasy o nazwie `ProductsTableAdapter.TransactionSupport.cs` (zobacz rysunek 4). Ten plik będzie przechowywać częściową implementację `ProductsTableAdapter`, która obejmuje metody wykonywania modyfikacji danych przy użyciu transakcji.

![Dodawanie folderu o nazwie TransactionSupport i pliku klasy o nazwie ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Rysunek 4**. Dodawanie folderu o nazwie `TransactionSupport` i pliku klasy o nazwie `ProductsTableAdapter.TransactionSupport.cs`

Wprowadź następujący kod do pliku `ProductsTableAdapter.TransactionSupport.cs`:

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

Słowo kluczowe `partial` w deklaracji klasy wskazuje kompilatorowi, że członkowie dodani w ramach programu mają być dodawani do klasy `ProductsTableAdapter` w przestrzeni nazw `NorthwindTableAdapters`. Zanotuj instrukcję `using System.Data.SqlClient` w górnej części pliku. Ponieważ TableAdapter został skonfigurowany do korzystania z dostawcy SqlClient, wewnętrznie używa obiektu [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) , aby wystawić swoje polecenia w bazie danych. W związku z tym musimy użyć klasy `SqlTransaction`, aby rozpocząć transakcję, a następnie zatwierdzić ją lub wycofać. Jeśli używasz magazynu danych innego niż Microsoft SQL Server, musisz użyć odpowiedniego dostawcy.

Te metody zapewniają bloki konstrukcyjne, które są konieczne do uruchamiania, wycofywania i zatwierdzania transakcji. Są one oznaczone `public`, co umożliwia ich używanie z poziomu `ProductsTableAdapter`, z innej klasy w DAL lub z innej warstwy w architekturze, takiej jak LOGIKI biznesowej. `BeginTransaction` otwiera `SqlConnection` wewnętrznej TableAdapter s (jeśli jest to konieczne), rozpoczyna transakcję i przypisuje do właściwości `Transaction` i dołącza transakcję do wewnętrznych `SqlDataAdapter` obiektów `SqlCommand`. przed zamknięciem wewnętrznego obiektu `Rollback` `CommitTransaction` i `RollbackTransaction` wywołać odpowiednio `Transaction` metody `Commit` i `Connection`.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Krok 3. Dodawanie metod do aktualizowania i usuwania danych w ramach parasola transakcji

Po zakończeniu tych metod, będziemy gotowi do dodawania metod do `ProductsDataTable` lub LOGIKI biznesowej, które wykonują szereg poleceń w ramach parasola transakcji. Poniższa metoda wykorzystuje wzorzec aktualizacji usługi Batch do aktualizowania wystąpienia `ProductsDataTable` przy użyciu transakcji. Uruchamia transakcję, wywołując metodę `BeginTransaction`, a następnie używa bloku `try...catch`, aby wydać instrukcje modyfikacji danych. Jeśli wywołanie metody `Adapter` Object s `Update` powoduje wyjątek, wykonanie zostanie przesłane do bloku `catch`, w którym transakcja zostanie wycofana, a wyjątek zostanie ponownie wygenerowany. Odwołaj, że metoda `Update` implementuje wzorzec aktualizacji wsadowej, wyliczając wiersze podanej `ProductsDataTable` i wykonując niezbędne `InsertCommand`, `UpdateCommand`i `DeleteCommand` s. Jeśli którekolwiek z tych poleceń spowoduje błąd, transakcja zostanie wycofana, cofając poprzednie modyfikacje wprowadzone w okresie istnienia transakcji. Jeśli instrukcja `Update` zakończy się bez błędu, transakcja zostanie zatwierdzona w całości.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Dodaj metodę `UpdateWithTransaction` do klasy `ProductsTableAdapter` za pomocą klasy częściowej w `ProductsTableAdapter.TransactionSupport.cs`. Alternatywnie ta metoda może zostać dodana do klasy `ProductsBLL` logiki biznesowej z kilkoma drobnymi zmianami składni. Mianowicie, słowo kluczowe this w `this.BeginTransaction()`, `this.CommitTransaction()`i `this.RollbackTransaction()` musiało zostać zastąpione przez `Adapter` (przywoływanie, że `Adapter` jest nazwą właściwości w `ProductsBLL` typu `ProductsTableAdapter`).

Metoda `UpdateWithTransaction` używa wzorca aktualizacji wsadowych, ale w ramach transakcji może być również używana seria wywołań DB-Direct, jak pokazano w poniższej metodzie. Metoda `DeleteProductsWithTransaction` akceptuje jako wejściowy `List<T>` typu `int`, które są `ProductID` s do usunięcia. Metoda inicjuje transakcję za pośrednictwem wywołania do `BeginTransaction`, a następnie w bloku `try` wykonuje iterację przez podaną listę wywołującą metodę wzorca DB-Direct `Delete` dla każdej wartości `ProductID`. Jeśli którekolwiek z wywołań `Delete` nie powiedzie się, kontrola zostanie przeniesiona do bloku `catch`, w którym transakcja jest wycofywana, a wyjątek zostanie ponownie wygenerowany. Jeśli wszystkie wywołania `Delete` powiodło się, transakcja zostanie zatwierdzona. Dodaj tę metodę do klasy `ProductsBLL`.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Stosowanie transakcji w wielu TableAdapters

Kod powiązany z transakcją opisany w tym samouczku pozwala na obsługę wielu instrukcji w odniesieniu do `ProductsTableAdapter`, które mają być traktowane jako operacja niepodzielna. Ale co zrobić, jeśli wielokrotne modyfikacje w różnych tabelach bazy danych muszą być wykonywane niepodzielnie? Na przykład podczas usuwania kategorii możemy najpierw chcieć ponownie przypisać bieżące produkty do innej kategorii. Te dwa kroki ponownie przypisują produkty i usuwając kategorię należy wykonać jako operację niepodzielną. Ale `ProductsTableAdapter` zawiera tylko metody modyfikowania tabeli `Products`, a `CategoriesTableAdapter` zawiera tylko metody modyfikowania tabeli `Categories`. Tak jak transakcja może obejmować zarówno TableAdapters?

Jedną z opcji jest dodanie metody do `CategoriesTableAdapter` o nazwie `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` i posiadanie tej metody wywołującej procedurę składowaną, która ponownie przypisuje produkty i usuwa kategorię w zakresie transakcji zdefiniowanej w ramach procedury składowanej. Zapoznaj się z artykułami dotyczącymi sposobu rozpoczynania, zatwierdzania i wycofywania transakcji w procedurach składowanych w przyszłym samouczku.

Innym rozwiązaniem jest utworzenie klasy pomocnika w DAL, która zawiera metodę `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`. Ta metoda utworzy wystąpienie `CategoriesTableAdapter` i `ProductsTableAdapter`, a następnie ustawi te dwie TableAdapters `Connection` właściwości na to samo wystąpienie `SqlConnection`. W tym momencie jeden z dwóch TableAdapters inicjuje transakcję z wywołaniem do `BeginTransaction`. Metody TableAdapters do ponownego przypisania produktów i usuwania kategorii zostałyby wywołane w bloku `try...catch` z transakcją zatwierdzoną lub wycofaną w razie potrzeby.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Krok 4. Dodawanie metody`UpdateWithTransaction`do warstwy logiki biznesowej

W kroku 3 dodaliśmy metodę `UpdateWithTransaction` do `ProductsTableAdapter` w DAL. Należy dodać odpowiednią metodę do LOGIKI biznesowej. Mimo że warstwa prezentacji może wywoływać bezpośrednio do DAL, aby wywołać metodę `UpdateWithTransaction`, te samouczki miały na celu zdefiniowanie architektury warstwowej, która umożliwia izolowanie DAL z warstwy prezentacji. Z tego względu behooves nam, aby kontynuować to podejście.

Otwórz plik klasy `ProductsBLL` i Dodaj metodę o nazwie `UpdateWithTransaction`, która po prostu wywołuje do odpowiedniej metody DAL. Teraz istnieją dwie nowe metody w `ProductsBLL`: `UpdateWithTransaction`, które zostały dodane i `DeleteProductsWithTransaction`, które zostały dodane w kroku 3.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Te metody nie uwzględniają atrybutu `DataObjectMethodAttribute` przypisanego do większości innych metod w klasie `ProductsBLL`, ponieważ te metody będą wywoływana bezpośrednio z klas ASP.NET z kodem. Odwołaj ten `DataObjectMethodAttribute` służy do flagowania metod, które powinny być wyświetlane w Kreatorze konfiguracji źródła danych "ObjectDataSource s" i na karcie (Wybierz, zaktualizuj, Wstaw lub Usuń). Ponieważ GridView nie ma wbudowanej obsługi operacji edytowania lub usuwania wsadowego, musimy programowo wywołać te metody zamiast używać podejścia deklaracyjnego bez kodu.

## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Krok 5: aktualizowanie niepodzielne danych bazy danych z warstwy prezentacji

Aby zilustrować wpływ transakcji na aktualizację partii rekordów, należy utworzyć interfejs użytkownika, który wyświetla listę wszystkich produktów w widoku GridView i zawiera kontrolkę sieci Web przycisku, która po kliknięciu ponownie przypisze produkty `CategoryID` wartości. W szczególności zmiana przypisania kategorii będzie postępować tak, aby pierwsze kilka produktów miało prawidłową wartość `CategoryID`, podczas gdy inne celowo całkowicie przypisaną nieistniejącą `CategoryID` wartość. Jeśli podjęto próbę zaktualizowania bazy danych przy użyciu produktu, którego `CategoryID` nie pasuje do istniejącej kategorii `CategoryID`, nastąpi naruszenie ograniczenia klucza obcego i zostanie zgłoszony wyjątek. Co widzimy w tym przykładzie, że podczas korzystania z transakcji wyjątek zgłoszony przez naruszenie ograniczenia klucza obcego spowoduje wycofanie poprzednich prawidłowych `CategoryID` zmian. Jednak gdy nie jest używana transakcja, modyfikacje w kategoriach początkowych pozostaną niezmienione.

Zacznij od otworzenia strony `Transactions.aspx` w folderze `BatchData` i przeciągnij widok GridView z przybornika na projektanta. Ustaw jej `ID` na `Products` i, z jego tagu inteligentnego, powiąż go z nowym elementem ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj element ObjectDataSource, aby ściągał swoje dane z metody `GetProducts` `ProductsBLL` Class. Będzie to widok GridView tylko do odczytu, więc ustaw listę rozwijaną z kart UPDATE, INSERT i DELETE na wartość (brak), a następnie kliknij przycisk Zakończ.

[![rysunek 5: Konfigurowanie elementu ObjectDataSource do używania metody getProducts klasy ProductsBLL](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Rysunek 5**. Rysunek 5: Konfigurowanie elementu ObjectDataSource do używania metody `GetProducts` `ProductsBLL` klasy s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))

[![ustawić listy rozwijane na kartach Aktualizuj, Wstaw i Usuń do (brak).](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Ilustracja 6**. Ustawianie list rozwijanych w kartach Aktualizuj, Wstaw i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio utworzy BoundFields i CheckBoxField dla pól danych produktu. Usuń wszystkie te pola z wyjątkiem `ProductID`, `ProductName`, `CategoryID`i `CategoryName`, a następnie zmień nazwę `ProductName` i `CategoryName` BoundFields `HeaderText` właściwości odpowiednio na produkt i kategorię. W tagu inteligentnym zaznacz opcję Włącz stronicowanie. Po wprowadzeniu tych zmian znaczniki deklaratywne GridView i ObjectDataSource s powinny wyglądać następująco:

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Następnie Dodaj trzy kontrolki sieci Web przycisków powyżej widoku GridView. Ustaw właściwość Text pierwszego przycisku, aby odświeżyć siatkę, drugi s do modyfikacji kategorii (z TRANSAKCJą) i trzecią wartość s, aby zmodyfikować kategorie (bez transakcji).

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

W tym momencie widok Projekt w programie Visual Studio powinna wyglądać podobnie do zrzutu ekranu pokazanego na rysunku 7.

[![Strona zawiera kontrolki sieci Web widok GridView i trzy](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Rysunek 7**. Strona zawiera kontrolki sieci Web widoku GridView i trzy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))

Utwórz programy obsługi zdarzeń dla każdego z trzech `Click` zdarzeń i użyj następującego kodu:

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

Program obsługi zdarzeń `Click` przycisku Odśwież po prostu ponownie wiąże dane z GridView, wywołując `Products`ą metodę `DataBind` GridView.

Druga procedura obsługi zdarzeń ponownie przypisze produkty `CategoryID` s i używa nowej metody transakcji z LOGIKI biznesowej, aby przeprowadzić aktualizacje bazy danych w ramach parasola transakcji. Należy zauważyć, że każdy produkt `CategoryID` ma arbitralnie ustawioną taką samą wartość jak jej `ProductID`. Będzie to poprawne dla pierwszych kilku produktów, ponieważ te produkty mają `ProductID` wartości, które mają być mapowane na prawidłowe `CategoryID` s. Jednak po rozpoczęciu `ProductID` s zbyt duży nie ma już zastosowania `ProductID` s i `CategoryID` s.

Trzecia procedura obsługi zdarzeń `Click` aktualizuje produkty `CategoryID` s w ten sam sposób, ale wysyła aktualizację do bazy danych przy użyciu domyślnej metody `Update` `ProductsTableAdapter` s. Ta metoda `Update` nie otacza serii poleceń w ramach transakcji, dlatego te zmiany zostaną wprowadzone przed pierwszym napotkanym błędem naruszenia ograniczenia klucza obcego.

Aby zademonstrować to zachowanie, odwiedź tę stronę za pomocą przeglądarki. Początkowo powinna zostać wyświetlona pierwsza strona danych, jak pokazano na rysunku 8. Następnie kliknij przycisk Modyfikuj kategorie (z TRANSAKCJą). Spowoduje to odświeżenie i próbę zaktualizowania wszystkich produktów `CategoryID` wartości, ale spowoduje naruszenie ograniczenia klucza obcego (zobacz rysunek 9).

[![produkty są wyświetlane w widoku GridView strony](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Ilustracja 8**. produkty są wyświetlane w widoku GridView strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))

[![ponowne przypisanie kategorii powoduje naruszenie ograniczenia klucza obcego](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Rysunek 9**. ponowne przypisanie kategorii powoduje naruszenie ograniczenia klucza obcego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))

Teraz naciśnij przycisk Wstecz przeglądarki, a następnie kliknij przycisk Odśwież siatkę. Po odświeżeniu danych powinny zostać wyświetlone dokładnie te same dane wyjściowe, jak pokazano na rysunku 8. Oznacza to, że mimo że niektóre produkty `CategoryID` s zostały zmienione na wartości prawne i zaktualizowane w bazie danych, zostały wycofane, gdy wystąpiło naruszenie ograniczenia klucza obcego.

Teraz spróbuj kliknąć przycisk Modyfikuj kategorie (bez transakcji). Spowoduje to ten sam błąd naruszenia ograniczenia klucza obcego (zobacz rysunek 9), ale tym razem te produkty, których wartości `CategoryID` zostały zmienione na wartość prawną, nie zostaną wycofane. Naciśnij przycisk Wstecz przeglądarki, a następnie przycisk Odśwież siatkę. Jak pokazano na rysunku 10, `CategoryID` s pierwszych ośmiu produktów zostały ponownie przypisane. Na przykład, na rysunku 8, zmian miał `CategoryID` 1, ale na rysunku 10 jego s został ponownie przypisany do 2.

[![niektóre produkty IDKategorii zostały zaktualizowane, podczas gdy inne nie były](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Ilustracja 10**. niektóre produkty `CategoryID` wartości zostały zaktualizowane, podczas gdy inne nie były ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))

## <a name="summary"></a>Podsumowanie

Domyślnie metody TableAdapter s nie zawijają instrukcji wykonywanej bazy danych w zakresie transakcji, ale z małą ilością pracy możemy dodać metody, które spowodują utworzenie, zatwierdzenie i wycofanie transakcji. W tym samouczku utworzyliśmy trzy takie metody w klasie `ProductsTableAdapter`: `BeginTransaction`, `CommitTransaction`i `RollbackTransaction`. Dowiesz się, jak korzystać z tych metod wraz z blokiem `try...catch`, aby dane były nieatomowe. W szczególności utworzyliśmy metodę `UpdateWithTransaction` w `ProductsTableAdapter`, która używa wzorca aktualizacji wsadowych do wykonywania niezbędnych modyfikacji w wierszach podanej `ProductsDataTable`. Dodaliśmy również metodę `DeleteProductsWithTransaction` do klasy `ProductsBLL` w LOGIKI biznesowej, która akceptuje `List` wartości `ProductID` jako dane wejściowe i wywołuje metodę wzorca DB-Direct `Delete` dla każdego `ProductID`. Obie metody uruchamiają się, tworząc transakcję, a następnie wykonując instrukcje modyfikacji danych w bloku `try...catch`. Jeśli wystąpi wyjątek, transakcja zostanie wycofana, w przeciwnym razie jest zatwierdzona.

Krok 5 ilustruje efekt transakcyjnych aktualizacji partii i aktualizacji wsadowych, które pozostały do użycia transakcji. W następnych trzech samouczkach utworzymy podstawę w tym samouczku i utworzysz interfejsy użytkownika do wykonywania aktualizacji wsadowych, usunięć i wstawiania.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Obsługa spójności bazy danych z transakcjami](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Zarządzanie transakcjami w SQL Server procedurach składowanych](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Łatwe w obsłudze transakcje: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [Elementy TransactionScope i adaptery](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Używanie Oracle Database transakcji w programie .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Dave Gardner, Hilton Giesenow i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Dalej](batch-updating-cs.md)
