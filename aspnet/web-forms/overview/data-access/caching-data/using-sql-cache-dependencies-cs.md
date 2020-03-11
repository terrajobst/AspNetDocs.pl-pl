---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: Używanie zależności pamięci podręcznej SQL (C#) | Microsoft Docs
author: rick-anderson
description: Najprostszą strategią buforowania jest umożliwienie wygasania buforowanych danych po upływie określonego czasu. Ale to proste podejście oznacza, że dane w pamięci podręcznej maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: 5bc27a08e39606c25b8f99d6ea057d2a853f08a6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78550422"
---
# <a name="using-sql-cache-dependencies-c"></a>Używanie zależności pamięci podręcznej SQL (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) lub [Pobierz plik PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> Najprostszą strategią buforowania jest umożliwienie wygasania buforowanych danych po upływie określonego czasu. Jednak to proste podejście oznacza, że dane w pamięci podręcznej nie są powiązane ze swoim źródłowym źródłem danych, co spowodowało niedawne dane, które są przechowywane zbyt długo lub bieżące dane wygasłe. Lepszym rozwiązaniem jest użycie klasy SqlCacheDependency, aby dane były przechowywane w pamięci podręcznej do momentu modyfikacji danych źródłowych w bazie danych SQL. W tym samouczku pokazano, jak to zrobić.

## <a name="introduction"></a>Wprowadzenie

Techniki buforowania badane w [danych pamięci](caching-data-with-the-objectdatasource-cs.md) podręcznej z danymi ObjectDataSource i pamięci podręcznej w samouczkach [architektury](caching-data-in-the-architecture-cs.md) używały czasu wygaśnięcia w celu wykluczenia danych z pamięci podręcznej po upływie określonego okresu. To podejście jest najprostszym sposobem zrównoważenia wzrostu wydajności pamięci podręcznej w celu uzyskania nieodświeżoności danych. Wybierając czas wygaśnięcia ( *x* s), deweloperzy stron mogą korzystać z zalet pamięci podręcznej tylko przez *x* s, ale można łatwo zawiesić, że jej dane nigdy nie będą przestarzałe dłużej niż maksymalnie *x* sekund. Oczywiście w przypadku danych statycznych *x* można rozszerzyć na okres istnienia aplikacji sieci Web, co zostało zbadane w samouczku [uruchamiania aplikacji](caching-data-at-application-startup-cs.md) .

W przypadku buforowania danych bazy danych czas wygaśnięcia oparty na czasie jest często wybierany do łatwego użycia, ale jest często nieodpowiednie rozwiązanie. W idealnym przypadku dane bazy danych będą przechowywane w pamięci podręcznej do momentu zmodyfikowania danych źródłowych w bazie danych programu; tylko wtedy, gdy pamięć podręczna zostanie wykluczona. Takie podejście maksymalizuje korzyści wynikające z wydajności buforowania i minimalizuje czas trwania starych danych. Jednak aby można było korzystać z tych korzyści, należy wziąć pod uwagę pewne systemy, które wiedzą, kiedy dane bazowe bazy danych zostały zmodyfikowane i wykluczają odpowiadające im elementy z pamięci podręcznej. Przed ASP.NET 2,0 deweloperzy stron byli zobowiązani do wdrożenia tego systemu.

ASP.NET 2,0 udostępnia [klasę`SqlCacheDependency`](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) i infrastrukturę niezbędną do określenia, Kiedy nastąpiła zmiana w bazie danych, aby można było wykluczyć odpowiednie elementy w pamięci podręcznej. Istnieją dwie techniki określania, kiedy dane bazowe uległy zmianie: powiadomienia i sondowania. Po omówieniu różnic między powiadomieniami i sondowaniem utworzymy infrastrukturę potrzebną do obsługi sondowania, a następnie Dowiedz się, jak używać klasy `SqlCacheDependency` w scenariuszach deklaratywnych i programistycznych.

## <a name="understanding-notification-and-polling"></a>Informacje o powiadomieniach i sondowaniu

Istnieją dwie techniki, których można użyć do określenia, kiedy dane w bazie danych zostały zmodyfikowane: powiadomienia i sondowania. W przypadku powiadomienia baza danych programu automatycznie alarmuje środowisko uruchomieniowe ASP.NET, gdy wyniki określonego zapytania zostały zmienione od czasu ostatniego wykonania zapytania. w tym momencie elementy pamięci podręcznej skojarzone z zapytaniem są wykluczane. W przypadku sondowania serwer bazy danych przechowuje informacje o tym, kiedy określone tabele zostały ostatnio zaktualizowane. Środowisko uruchomieniowe ASP.NET okresowo sonduje bazę danych, aby sprawdzić, jakie tabele uległy zmianie od momentu wprowadzenia ich do pamięci podręcznej. Te tabele, dla których zmodyfikowano dane, zostały wykluczone z usuniętych elementów pamięci podręcznej.

Opcja powiadamiania wymaga mniej konfiguracji niż sondowanie i jest bardziej szczegółowy, ponieważ śledzi zmiany na poziomie zapytania, a nie na poziomie tabeli. Niestety, powiadomienia są dostępne tylko w pełnych wersjach Microsoft SQL Server 2005 (tj. wersji innych niż Express). Można jednak użyć opcji sondowania dla wszystkich wersji Microsoft SQL Server od 7,0 do 2005. Ponieważ te samouczki korzystają z wersji Express SQL Server 2005, będziemy skupić się na konfigurowaniu i używaniu opcji sondowania. Zapoznaj się z sekcją więcej informacji na końcu tego samouczka, aby uzyskać więcej zasobów na temat możliwości powiadomień SQL Server 2005 s.

W przypadku sondowania baza danych musi być skonfigurowana w taki sposób, aby zawierała tabelę o nazwie `AspNet_SqlCacheTablesForChangeNotification`, która ma trzy kolumny — `tableName`, `notificationCreated`i `changeId`. Ta tabela zawiera wiersz dla każdej tabeli zawierającej dane, które mogą być potrzebne w zależności od pamięci podręcznej SQL w aplikacji sieci Web. Kolumna `tableName` określa nazwę tabeli, podczas gdy `notificationCreated` wskazuje datę i godzinę dodania wiersza do tabeli. Kolumna `changeId` jest typu `int` i ma wartość początkową 0. Wartość jest zwiększana wraz z każdą modyfikacją tabeli.

Oprócz tabeli `AspNet_SqlCacheTablesForChangeNotification` baza danych musi również obejmować Wyzwalacze w każdej tabeli, która może pojawić się w zależności od pamięci podręcznej SQL. Te wyzwalacze są wykonywane za każdym razem, gdy wiersz zostanie wstawiony, zaktualizowany lub usunięty, i zwiększyć wartość tabeli s `changeId` w `AspNet_SqlCacheTablesForChangeNotification`.

Środowisko uruchomieniowe ASP.NET śledzi bieżące `changeId` tabeli podczas buforowania danych przy użyciu obiektu `SqlCacheDependency`. Baza danych jest okresowo sprawdzana, a wszystkie `SqlCacheDependency` obiekty, których `changeId` różnią się od wartości w bazie danych, są wykluczone, ponieważ różnią się `changeId` wartość wskazuje, że tabela została zmieniona, ponieważ dane były buforowane.

## <a name="step-1-exploring-theaspnet_regsqlexecommand-line-program"></a>Krok 1. Eksplorowanie programu`aspnet_regsql.exe`wiersza polecenia

Z podejściem sondowania baza danych musi być skonfigurowana w taki sposób, aby zawierała opisaną powyżej infrastrukturę: wstępnie zdefiniowaną tabelę (`AspNet_SqlCacheTablesForChangeNotification`), kilku procedur składowanych i wyzwalaczy dla każdej tabeli, która może być używana w zależnościach pamięci podręcznej SQL w aplikacji sieci Web. Te tabele, procedury składowane i wyzwalacze mogą być tworzone za pomocą wiersza polecenia `aspnet_regsql.exe`, które znajdują się w folderze `$WINDOWS$\Microsoft.NET\Framework\version`. Aby utworzyć tabelę `AspNet_SqlCacheTablesForChangeNotification` i skojarzone procedury składowane, uruchom następujące polecenie w wierszu polecenia:

[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Aby wykonać te polecenia, określona nazwa logowania bazy danych musi znajdować się w roli [`db_securityadmin`](https://msdn.microsoft.com/library/ms188685.aspx) i [`db_ddladmin`](https://msdn.microsoft.com/library/ms190667.aspx) . Aby sprawdzić, czy program T-SQL został wysłany do bazy danych przez `aspnet_regsql.exe` wiersza polecenia, zapoznaj się z [wpisem w blogu](http://scottonwriting.net/sowblog/posts/10709.aspx).

Aby na przykład dodać infrastrukturę do sondowania do Microsoft SQL Server bazy danych o nazwie `pubs` na serwerze bazy danych o nazwie `ScottsServer` przy użyciu uwierzytelniania systemu Windows, przejdź do odpowiedniego katalogu i w wierszu polecenia wprowadź:

[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Po dodaniu infrastruktury na poziomie bazy danych musimy dodać wyzwalacze do tych tabel, które będą używane w zależnościach pamięci podręcznej SQL. Ponownie Użyj `aspnet_regsql.exe` wiersza polecenia, ale Określ nazwę tabeli przy użyciu przełącznika `-t` i zamiast używania przełącznika `-ed` użyć `-et`, takich jak:

[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Aby dodać wyzwalacze do `authors` i `titles` tabel w bazie danych `pubs` na `ScottsServer`, użyj:

[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

W tym samouczku dodasz wyzwalacze do tabel `Products`, `Categories`i `Suppliers`. Przejrzyjmy konkretną składnię wiersza polecenia w kroku 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inapp_data"></a>Krok 2. odwoływanie się do bazy danych Microsoft SQL Server 2005 Express Edition w programie`App_Data`

Program wiersza polecenia `aspnet_regsql.exe` wymaga bazy danych i nazwy serwera, aby można było dodać niezbędną infrastrukturę sondowania. Ale co to jest baza danych i nazwa serwera dla bazy danych Microsoft SQL Server 2005 Express, która znajduje się w folderze `App_Data`? Zamiast znajdować informacje o nazwach baz danych i serwerów, wiemy, że najprostszym podejściem jest dołączenie bazy danych do wystąpienia bazy danych `localhost\SQLExpress` i zmiana nazwy danych przy użyciu [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Jeśli na maszynie zainstalowano jedną z pełnych wersji SQL Server 2005, na komputerze zainstalowano już SQL Server Management Studio. Jeśli masz tylko wersję Express, możesz pobrać bezpłatną [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Zacznij od zamykania programu Visual Studio. Następnie Otwórz SQL Server Management Studio i wybierz połączenie z serwerem `localhost\SQLExpress` przy użyciu uwierzytelniania systemu Windows.

![Dołącz do serwera localhost\SQLExpress](using-sql-cache-dependencies-cs/_static/image1.gif)

**Rysunek 1**. dołączanie do serwera `localhost\SQLExpress`

Po nawiązaniu połączenia z serwerem program Management Studio będzie wyświetlał serwer i ma podfoldery dla baz danych, zabezpieczeń i tak dalej. Kliknij prawym przyciskiem myszy folder Databases, a następnie wybierz opcję Attach (Dołącz). Spowoduje to wyświetlenie okna dialogowego dołączanie baz danych (patrz rysunek 2). Kliknij przycisk Dodaj i wybierz folder `NORTHWND.MDF` Database w folderze `App_Data` aplikacji sieci Web.

[![dołączyć NORTHWND. Baza danych MDF z folderu App_Data](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Rysunek 2**. dołączanie bazy danych `NORTHWND.MDF` z folderu `App_Data` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image2.png))

Spowoduje to dodanie bazy danych do folderu Databases. Nazwa bazy danych może być pełną ścieżką do pliku bazy danych lub pełną ścieżką z [identyfikatorem GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Aby uniknąć konieczności wpisywania tego długiej nazwy bazy danych przy użyciu narzędzia wiersza polecenia ASPNET\_regsql. exe, Zmień nazwę bazy danych na bardziej przyjazną dla człowieka, klikając prawym przyciskiem myszy podłączoną do bazy danych i wybierając polecenie Zmień nazwę. Zmieniono nazwę mojej bazy danych na Samouczeky datasamouczków.

![Zmień nazwę dołączonej bazy danych na bardziej przyjazną dla człowieka](using-sql-cache-dependencies-cs/_static/image3.gif)

**Rysunek 3**. zmiana nazwy dołączonej bazy danych na bardziej przyjazną dla człowieka

## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Krok 3. Dodawanie infrastruktury sondowania do bazy danych Northwind

Teraz, po dołączeniu bazy danych `NORTHWND.MDF` z folderu `App_Data`, przygotujemy się do dodania infrastruktury sondowania. Przy założeniu, że zmieniono nazwę bazy danych na Samouczeky, uruchom następujące cztery polecenia:

[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Po uruchomieniu tych czterech poleceń kliknij prawym przyciskiem myszy nazwę bazy danych w Management Studio, przejdź do podmenu zadania, a następnie wybierz polecenie Odłącz. Następnie zamknij Management Studio i ponownie otwórz program Visual Studio.

Po ponownym otwarciu programu Visual Studio przejdź do bazy danych za pomocą Eksplorator serwera. Zanotuj nową tabelę (`AspNet_SqlCacheTablesForChangeNotification`), nowe procedury składowane i Wyzwalacze w tabelach `Products`, `Categories`i `Suppliers`.

![Baza danych zawiera teraz niezbędną infrastrukturę sondowania](using-sql-cache-dependencies-cs/_static/image4.gif)

**Rysunek 4**. baza danych zawiera teraz niezbędną infrastrukturę sondowania

## <a name="step-4-configuring-the-polling-service"></a>Krok 4. Konfigurowanie usługi sondowania

Po utworzeniu wymaganych tabel, wyzwalaczy i procedur składowanych w bazie danych, ostatnim krokiem jest skonfigurowanie usługi sondowania, która odbywa się za pośrednictwem `Web.config` przez określenie baz danych do użycia oraz częstotliwość sondowania w milisekundach. Poniższy znacznik sonduje bazę danych Northwind co sekundę.

[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

Wartość `name` w elemencie `<add>` (NorthwindDB) kojarzy nazwę, którą można odczytać przez człowieka z określoną bazą danych. Podczas pracy z zależnościami w pamięci podręcznej SQL należy odwołać się do nazwy bazy danych zdefiniowanej w tym miejscu oraz tabeli, na której opierają się dane buforowane. Zobaczymy, jak używać klasy `SqlCacheDependency` do programistycznego kojarzenia zależności pamięci podręcznej SQL z danymi buforowanymi w kroku 6.

Po ustanowieniu zależności pamięci podręcznej SQL system sondowania będzie łączył się z bazami danych zdefiniowanymi w `<databases>` elementy co `pollTime` milisekund i wykonać `AspNet_SqlCachePollingStoredProcedure` procedurę składowaną. Ta procedura składowana — która została dodana z powrotem w kroku 3 przy użyciu narzędzia wiersza polecenia `aspnet_regsql.exe` — zwraca wartości `tableName` i `changeId` dla każdego rekordu w `AspNet_SqlCacheTablesForChangeNotification`. Nieaktualne zależności pamięci podręcznej SQL są wykluczone z pamięci podręcznej.

Ustawienie `pollTime` wprowadza kompromis między wydajnością i nieodświeżonością danych. Mała wartość `pollTime` zwiększa liczbę żądań do bazy danych, ale szybciej wyklucza nieodświeżone dane z pamięci podręcznej. Większa `pollTime` wartość zmniejsza liczbę żądań bazy danych, ale zwiększa opóźnienie między zmianą danych zaplecza a pokrewnymi elementami pamięci podręcznej. Na szczęście żądanie bazy danych wykonuje prostą procedurę składowaną, która zwraca tylko kilka wierszy z prostej, lekkiej tabeli. Ale należy eksperymentować z różnymi `pollTime` wartościami, aby znaleźć idealny kompromis między dostępem do bazy danych i nieodświeżonością danych aplikacji. Najmniejsza wartość `pollTime` dozwolona to 500.

> [!NOTE]
> Powyższy przykład zawiera jedną wartość `pollTime` w elemencie `<sqlCacheDependency>`, ale opcjonalnie można określić wartość `pollTime` w elemencie `<add>`. Jest to przydatne, jeśli określono wiele baz danych i chcesz dostosować częstotliwość sondowania na bazę danych.

## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Krok 5. deklaracyjne współdziałanie z zależnościami pamięci podręcznej SQL

W krokach od 1 do 4 dodaliśmy procedurę konfigurowania niezbędnej infrastruktury bazy danych i konfigurowania systemu sondowania. Dzięki tej infrastrukturze można teraz dodawać elementy do pamięci podręcznej danych ze skojarzoną zależnością pamięci podręcznej SQL przy użyciu technik programistycznych lub deklaratywnych. W tym kroku sprawdzimy, jak deklaratywnie współpracować z zależnościami pamięci podręcznej SQL. W kroku 6 zaobserwujemy podejście programistyczne.

[Informacje o buforowaniu za pomocą](caching-data-with-the-objectdatasource-cs.md) samouczka elementu ObjectDataSource zostały omówione w ramach deklaracyjnej pamięci podręcznej elementu ObjectDataSource. Po prostu ustawiając właściwość `EnableCaching` na `true` i Właściwość `CacheDuration` do pewnego interwału czasu, element ObjectDataSource automatycznie buforuje dane zwrócone z obiektu bazowego dla określonego interwału. Element ObjectDataSource może również korzystać z co najmniej jednej zależności pamięci podręcznej SQL.

Aby zademonstrować używanie zależności pamięci podręcznej SQL w sposób deklaratywny, Otwórz stronę `SqlCacheDependencies.aspx` w folderze `Caching` i przeciągnij widok GridView z przybornika do projektanta. Ustaw `ID` GridView s na `ProductsDeclarative` i, z jego tagu inteligentnego, wybierz powiązanie go z nowym elementem ObjectDataSource o nazwie `ProductsDataSourceDeclarative`.

[![utworzyć nowego elementu ObjectDataSource o nazwie ProductsDataSourceDeclarative](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Rysunek 5**. Utwórz nowy element ObjectDataSource o nazwie `ProductsDataSourceDeclarative` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image4.png))

Skonfiguruj element ObjectDataSource do używania klasy `ProductsBLL` i Ustaw listę rozwijaną na karcie Wybierz, aby `GetProducts()`. Na karcie Aktualizacja wybierz Przeciążenie `UpdateProduct` przy użyciu trzech parametrów wejściowych — `productName`, `unitPrice`i `productID`. Ustaw listę rozwijaną na (brak) na kartach Wstawianie i usuwanie.

[![użyć przeciążenia UpdateProduct z trzema parametrami wejściowymi](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Ilustracja 6**. Używanie przeciążenia UpdateProduct z trzema parametrami wejściowymi ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image6.png))

[![ustawić dla listy rozwijanej wartość (brak) dla kart Wstawianie i usuwanie](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Rysunek 7**. Ustaw listę rozwijaną na (brak) dla kart INSERT i Delete ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image8.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio utworzy BoundFields i CheckBoxFields w widoku GridView dla każdego z pól danych. Usuń wszystkie pola, ale `ProductName`, `CategoryName`i `UnitPrice`i sformatuj te pola jako dopasowane. W tagu inteligentnym GridView zaznacz pola wyboru Włącz stronicowanie, Włącz sortowanie i Włącz edycję. Program Visual Studio ustawi właściwość `OldValuesParameterFormatString` ObjectDataSource s na `original_{0}`. Aby funkcja edytuj widok GridView działała prawidłowo, Usuń tę właściwość całkowicie ze składni deklaracyjnej lub ustaw ją z powrotem na wartość domyślną, `{0}`.

Na koniec Dodaj kontrolkę sieci Web etykieta powyżej widoku GridView i ustaw jej Właściwość `ID` na `ODSEvents` i jej Właściwość `EnableViewState` na `false`. Po wprowadzeniu tych zmian znaczniki deklaratywne strony powinny wyglądać podobnie do poniższego. Zwróć uwagę na to, że w obszarach GridView zostały wykonane różne dostosowania estetyczne, które nie są niezbędne do zademonstrowania funkcji zależności pamięci podręcznej SQL.

[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `Selecting` ObjectDataSource s i Dodaj następujący kod:

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Odwołaj się, że zdarzenie ObjectDataSource s `Selecting` wyzwalane tylko podczas pobierania danych z jego obiektu źródłowego. Jeśli element ObjectDataSource uzyskuje dostęp do danych z własnej pamięci podręcznej, to zdarzenie nie zostanie wyzwolone.

Teraz odwiedź tę stronę za pomocą przeglądarki. Ponieważ jeszcze nie zaimplementowano żadnego buforowania, za każdym razem, gdy użytkownik wyświetla, sortuje lub edytuje siatkę, na stronie powinna zostać wyświetlona treść, wybierając pozycję wypchnięcie zdarzenia, jak pokazano na rysunku 8.

[![zdarzenie elementu ObjectDataSource-SELECT wyzwalane za każdym razem, gdy GridView zostanie stronicowane, edytowane lub posortowane](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Ilustracja 8**. zdarzenie elementu ObjectDataSource s `Selecting` wyzwalane za każdym razem, gdy GridView jest stronicowane, edytowane lub posortowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image10.png))

Jak pokazano w [danych dotyczących buforowania za pomocą](caching-data-with-the-objectdatasource-cs.md) samouczka elementu ObjectDataSource, ustawienie właściwości `EnableCaching` na `true` powoduje, że element ObjectDataSource buforuje dane przez czas trwania określony przez jego właściwość `CacheDuration`. Element ObjectDataSource ma również [właściwość`SqlCacheDependency`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), która dodaje co najmniej jedną zależność usługi SQL cache do danych w pamięci podręcznej przy użyciu wzorca:

[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

Gdzie *DatabaseName* jest nazwą bazy danych określoną w atrybucie `name` elementu `<add>` w `Web.config`, a *TableName* jest nazwą tabeli bazy danych. Na przykład, aby utworzyć element ObjectDataSource, który przechowuje dane w pamięci podręcznej na podstawie nieokreślonych wartości w zależności od bazy danych SQL, w tabeli `Products` Northwind, ustaw właściwość ObjectDataSource s `EnableCaching` na `true` i jej Właściwość `SqlCacheDependency` na NorthwindDB: Products.

> [!NOTE]
> Można użyć zależności pamięci podręcznej SQL *oraz* czasu wygaśnięcia opartego na czasie przez ustawienie `EnableCaching` na `true`, `CacheDuration` do interwału czasu i `SqlCacheDependency` do bazy danych i nazw tabel. Element ObjectDataSource będzie wykluczać swoje dane po osiągnięciu czasu wygaśnięcia lub gdy system sondowania wskazuje, że bazowe dane bazy danych uległy zmianie, w zależności od tego, co się dzieje.

W widoku GridView w `SqlCacheDependencies.aspx` są wyświetlane dane z dwóch tabel — `Products` i `Categories` (pole `CategoryName` produktu jest pobierane za pośrednictwem `JOIN` w `Categories`). W związku z tym chcemy określić dwie zależności pamięci podręcznej SQL: NorthwindDB: Products; NorthwindDB: kategorie.

[![skonfigurować element ObjectDataSource do obsługi buforowania przy użyciu zależności pamięci podręcznej SQL w produktach i kategoriach](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Rysunek 9**. Konfigurowanie elementu ObjectDataSource do obsługi buforowania przy użyciu zależności pamięci podręcznej SQL na `Products` i `Categories` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image12.png))

Po skonfigurowaniu elementu ObjectDataSource do obsługi buforowania należy ponownie odwiedzić stronę za pomocą przeglądarki. Po ponownym uruchomieniu zdarzenie wybierania tekstu powinno pojawić się na pierwszej stronie odwiedzania, ale powinno być odrzucane podczas stronicowania, sortowania lub klikania przycisków Edytuj lub Anuluj. Jest to spowodowane tym, że po załadowaniu danych do pamięci podręcznej SQLs, pozostaje ona tam, dopóki nie zostaną zmodyfikowane `Products` lub `Categories` tabele lub dane zostaną zaktualizowane za pomocą widoku GridView.

Po przeprowadzeniu stronicowania przez siatkę i zanotowaniu braku wygenerowanego tekstu zdarzenia Otwórz nowe okno przeglądarki i przejdź do samouczka podstawy w sekcji Edytowanie, wstawianie i usuwanie (`~/EditInsertDelete/Basics.aspx`). Zaktualizuj nazwę lub cenę produktu. Następnie z pierwszego okna przeglądarki Wyświetl inną stronę danych, posortuj siatkę lub kliknij przycisk Edytuj wiersz s. Tym razem wywołane zdarzenie wybierania powinno pojawić się ponownie, ponieważ dane źródłowej bazy danych zostały zmodyfikowane (patrz rysunek 10). Jeśli tekst nie jest wyświetlany, poczekaj chwilę i spróbuj ponownie. Należy pamiętać, że usługa sondowania sprawdza zmiany w tabeli `Products` co `pollTime` milisekundy, więc istnieje opóźnienie między aktualizacją danych źródłowych a przełączaniem danych w pamięci podręcznej.

[![modyfikowanie tabeli Products wyklucza buforowane dane produktu](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Ilustracja 10**. Modyfikowanie tabeli Products wyklucza buforowane dane produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image14.png))

## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Krok 6. programistyczne praca z klasą`SqlCacheDependency`

[Dane dotyczące buforowania w](caching-data-in-the-architecture-cs.md) samouczku architektury były zgodne z korzyściami z używania oddzielnej warstwy buforowania w architekturze, zamiast ścisłego sprzęgania pamięci podręcznej z elementem ObjectDataSource. W tym samouczku utworzyliśmy klasę `ProductsCL`, aby zademonstrować programistyczną pracę z pamięcią podręczną danych. Aby korzystać z zależności pamięci podręcznej SQL w warstwie buforowania, należy użyć klasy `SqlCacheDependency`.

W przypadku systemu sondowania obiekt `SqlCacheDependency` musi być skojarzony z konkretną parą bazy danych i tabeli. Poniższy kod na przykład tworzy obiekt `SqlCacheDependency` na podstawie tabeli Northwind Database s `Products`:

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

Dwa parametry wejściowe do konstruktora `SqlCacheDependency` s są odpowiednio nazwami baz danych i tabel. Podobnie jak w przypadku właściwości `SqlCacheDependency` ObjectDataSource s, użyta nazwa bazy danych jest taka sama jak wartość określona w atrybucie `name` elementu `<add>` w `Web.config`. Nazwa tabeli to rzeczywista nazwa tabeli bazy danych.

Aby skojarzyć `SqlCacheDependency` z elementem dodanym do pamięci podręcznej danych, użyj jednego z przeciążeń metody `Insert`, które akceptują zależność. Poniższy kod dodaje *wartość* do pamięci podręcznej danych przez czas nieokreślony, ale kojarzy ją z `SqlCacheDependency`ą w tabeli `Products`. W skrócie *wartość* pozostanie w pamięci podręcznej, dopóki nie zostanie wykluczona z powodu ograniczeń pamięci lub ponieważ system sondowania wykrył, że tabela `Products` została zmieniona od czasu jej buforowania.

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

Klasa `ProductsCL` buforowanie warstwy s obecnie buforuje dane z tabeli `Products` przy użyciu czasu wygaśnięcia 60 sekund. Zezwól usłudze s na aktualizację tej klasy, tak aby korzystała z zależności pamięci podręcznej SQL. Metoda `ProductsCL` klasy s `AddCacheItem`, która jest odpowiedzialna za Dodawanie danych do pamięci podręcznej, obecnie zawiera następujący kod:

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Zaktualizuj ten kod, aby użyć obiektu `SqlCacheDependency` zamiast zależności pamięci podręcznej `MasterCacheKeyArray`:

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Aby przetestować tę funkcję, Dodaj widok GridView do strony pod istniejącym `ProductsDeclarative` GridView. Ustaw ten nowy `ID` GridView, aby `ProductsProgrammatic` i, za pomocą jego tagu inteligentnego, powiąż go z nowym elementem ObjectDataSource o nazwie `ProductsDataSourceProgrammatic`. Skonfiguruj element ObjectDataSource do używania klasy `ProductsCL`, ustawiając listy rozwijane na kartach wybierz i Aktualizuj, aby odpowiednio `GetProducts` i `UpdateProduct`.

[![skonfigurować element ObjectDataSource do używania klasy ProductsCL](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Ilustracja 11**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsCL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image16.png))

[![wybrać metodę getProducts z listy rozwijanej wybierz kartę s](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Ilustracja 12**. wybierz metodę `GetProducts` z listy rozwijanej wybierz kartę s ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image18.png))

[![wybrać metodę UpdateProduct z listy rozwijanej Aktualizuj kartę](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Ilustracja 13**. Wybierz metodę UpdateProduct z listy rozwijanej Aktualizuj kartę ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image20.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio utworzy BoundFields i CheckBoxFields w widoku GridView dla każdego z pól danych. Podobnie jak w przypadku pierwszego widoku GridView dodanego do tej strony, Usuń wszystkie pola, ale `ProductName`, `CategoryName`i `UnitPrice`i sformatuj te pola jako dopasowane. W tagu inteligentnym GridView zaznacz pola wyboru Włącz stronicowanie, Włącz sortowanie i Włącz edycję. Podobnie jak w przypadku `ProductsDataSourceDeclarative` ObjectDataSource, program Visual Studio ustawi właściwość `OldValuesParameterFormatString` `ProductsDataSourceProgrammatic` ObjectDataSource s na `original_{0}`. Aby funkcja edytuj widok GridView działała prawidłowo, należy ustawić tę właściwość z powrotem do `{0}` (lub usunąć przypisanie właściwości ze składni deklaracyjnej w ogóle).

Po wykonaniu tych zadań, wyniki deklaratywnego GridView i ObjectDataSource, powinny wyglądać następująco:

[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

Aby przetestować zależność pamięci podręcznej SQL w warstwie buforowania, ustaw punkt przerwania w metodzie `AddCacheItem` `ProductCL` Class, a następnie Rozpocznij debugowanie. Podczas pierwszej wizyty `SqlCacheDependencies.aspx`punkt przerwania powinien zostać osiągnięty, gdy dane są żądane po raz pierwszy, i umieszczane w pamięci podręcznej. Następnie przejdź do innej strony w widoku GridView lub posortuj jedną z kolumn. Powoduje to, że GridView będzie ponawiać kwerendę swoich danych, ale dane powinny znajdować się w pamięci podręcznej, ponieważ tabela bazy danych `Products` nie została zmodyfikowana. Jeśli dane nie zostaną odnalezione w pamięci podręcznej, upewnij się, że na komputerze jest dostępna wystarczająca ilość pamięci i spróbuj ponownie.

Po przeprowadzeniu stronicowania na kilku stronach widoku GridView Otwórz drugie okno przeglądarki i przejdź do samouczka podstawy w sekcji Edytowanie, wstawianie i usuwanie (`~/EditInsertDelete/Basics.aspx`). Zaktualizuj rekord z tabeli Products, a następnie w pierwszym oknie przeglądarki Wyświetl nową stronę lub kliknij jeden z nagłówków sortowania.

W tym scenariuszu zobaczysz jedną z dwóch rzeczy: punkt przerwania zostanie osiągnięty, co oznacza, że buforowane dane zostały wykluczone z powodu zmiany w bazie danych; lub punkt przerwania nie zostanie trafiony, co oznacza, że `SqlCacheDependencies.aspx` wyświetla teraz nieodświeżone dane. Jeśli punkt przerwania nie zostanie trafiony, prawdopodobnie jest to spowodowane tym, że usługa sondowania nie została jeszcze uruchomiona od czasu zmiany danych. Należy pamiętać, że usługa sondowania sprawdza zmiany w tabeli `Products` co `pollTime` milisekundy, więc istnieje opóźnienie między aktualizacją danych źródłowych a przełączaniem danych w pamięci podręcznej.

> [!NOTE]
> To opóźnienie jest prawdopodobnie wyświetlane podczas edytowania jednego z produktów za pomocą widoku GridView w `SqlCacheDependencies.aspx`. W oknie [dane buforowania w](caching-data-in-the-architecture-cs.md) samouczku dotyczącym architektury dodaliśmy zależność pamięci podręcznej `MasterCacheKeyArray`, aby upewnić się, że dane edytowane za pomocą metody `ProductsCL` klasy s `UpdateProduct` zostały wykluczone z pamięci podręcznej. Jednak ta zależność pamięci podręcznej została zastąpiona w przypadku modyfikacji metody `AddCacheItem` wcześniej w tym kroku, w związku z czym Klasa `ProductsCL` nadal będzie wyświetlać dane buforowane do momentu, gdy system sondowania zmieni się na tabelę `Products`. Zobaczymy, jak ponownie wprowadzić zależność pamięci podręcznej `MasterCacheKeyArray` w kroku 7.

## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Krok 7: Kojarzenie wielu zależności z buforowanym elementem

Odwołaj, że zależność pamięci podręcznej `MasterCacheKeyArray` jest używana w celu upewnienia się, że *wszystkie* dane związane z produktem są wykluczone z pamięci podręcznej w przypadku zaktualizowania dowolnego elementu skojarzonego z nim. Na przykład Metoda `GetProductsByCategoryID(categoryID)` buforuje `ProductsDataTables` wystąpień dla każdej unikatowej wartości *IDkategorii* . Jeśli jeden z tych obiektów zostanie wykluczony, zależność pamięci podręcznej `MasterCacheKeyArray` gwarantuje, że pozostałe również zostaną usunięte. Bez tej zależności pamięci podręcznej, gdy dane buforowane są modyfikowane, istnieje możliwość, że inne buforowane dane produktu mogą być nieaktualne. W związku z tym ważne jest, aby zachować zależność pamięci podręcznej `MasterCacheKeyArray` przy użyciu zależności pamięci podręcznej SQL. Jednak Metoda `Insert`a pamięci podręcznej danych umożliwia tylko pojedynczy obiekt zależności.

Ponadto podczas pracy z zależnościami w pamięci podręcznej SQL może być konieczne skojarzenie wielu tabel bazy danych jako zależności. Na przykład `ProductsDataTable` pamięci podręcznej w klasie `ProductsCL` zawiera kategorię i nazwy dostawców dla każdego produktu, ale metoda `AddCacheItem` używa tylko zależności od `Products`. W takiej sytuacji, jeśli użytkownik zaktualizuje nazwę kategorii lub dostawcy, zapisane w pamięci podręcznej dane o produkcie będą pozostawały nieaktualne. W związku z tym chcemy, aby dane przechowywane w pamięci podręcznej były zależne od nie tylko `Products` tabeli, ale również w tabelach `Categories` i `Suppliers`.

[Klasa`AggregateCacheDependency`](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) zapewnia metodę kojarzenia wielu zależności z elementem pamięci podręcznej. Zacznij od utworzenia wystąpienia `AggregateCacheDependency`. Następnie Dodaj zestaw zależności przy użyciu metody `AggregateCacheDependency` s `Add`. Po zakończeniu wstawiania elementu do pamięci podręcznej danych, należy przekazać wystąpienie `AggregateCacheDependency`. Gdy *dowolna* z wystąpień `AggregateCacheDependency` wystąpienia s ulegnie zmianie, buforowany element zostanie wykluczony.

Poniżej przedstawiono zaktualizowany kod dla metody `AddCacheItem` `ProductsCL` Class. Metoda tworzy `MasterCacheKeyArray` zależność pamięci podręcznej wraz z obiektami `SqlCacheDependency` dla tabel `Products`, `Categories`i `Suppliers`. Są one połączone w jeden `AggregateCacheDependency` obiektu o nazwie `aggregateDependencies`, który jest następnie przesyłany do metody `Insert`.

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Przetestuj ten nowy kod. Teraz zmiany w tabelach `Products`, `Categories`lub `Suppliers` powodują wykluczenia danych z pamięci podręcznej. Ponadto Metoda `UpdateProduct` `ProductsCL` klasy s, która jest wywoływana podczas edytowania produktu przez GridView, wyklucza zależność pamięci podręcznej `MasterCacheKeyArray`, co powoduje, że buforowana `ProductsDataTable` zostanie wykluczona, a dane zostaną ponownie pobrane przy następnym żądaniu.

> [!NOTE]
> Zależności pamięci podręcznej SQL mogą być również używane z [buforowaniem danych wyjściowych](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Aby zapoznać się z prezentacją tej funkcji, zobacz: [Używanie buforowania danych wyjściowych ASP.NET z SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).

## <a name="summary"></a>Podsumowanie

W przypadku buforowania danych bazy danych dane będą pozostawać w pamięci podręcznej, dopóki nie zostaną zmodyfikowane w bazie danych. W przypadku ASP.NET 2,0 zależności pamięci podręcznej SQL można tworzyć i używać w scenariuszach deklaratywnych i programistycznych. Jednym z wyzwań związanych z tym podejściem jest odnajdowanie, gdy dane zostały zmodyfikowane. Pełne wersje Microsoft SQL Server 2005 udostępniają funkcje powiadamiania, które mogą ostrzec aplikację w przypadku zmiany wyniku zapytania. W przypadku wersji Express SQL Server 2005 i starszych wersji SQL Server należy użyć systemu sondowania. Na szczęście skonfigurowanie niezbędnej infrastruktury sondowania jest dość proste.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Używanie powiadomień o zapytaniach w Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Tworzenie powiadomienia o zapytaniach](https://msdn.microsoft.com/library/ms188669.aspx)
- [Buforowanie w ASP.NET z klasą `SqlCacheDependency`](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Narzędzie rejestracji SQL Server ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Omówienie `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci ołowiu dla tego samouczka były Marko zakresem, Teresa Murphy i Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-at-application-startup-cs.md)
> [dalej](caching-data-with-the-objectdatasource-vb.md)
