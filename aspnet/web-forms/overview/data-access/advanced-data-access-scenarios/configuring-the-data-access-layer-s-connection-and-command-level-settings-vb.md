---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Konfigurowanie ustawień na poziomie połączenia i wiersza polecenia warstwy dostępu do danych (VB) | Microsoft Docs
author: rick-anderson
description: TableAdapters w określonym zestawie danych automatycznie zajmie się połączeniem z bazą danych, wydawanie poleceń i wypełnianie DataTable wynikami...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: fa2868fc0dd8acd76f600b47d92adb984ce8d105
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573662"
---
# <a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Konfigurowanie ustawień na poziomie połączenia i poleceń warstwy dostępu do danych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) lub [Pobierz plik PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> TableAdapters w określonym zestawie danych automatycznie zajmie się połączeniem z bazą danych, wydawanie poleceń i wypełnianie DataTable wynikami. Zdarza się jednak, że chcemy zachować te szczegółowe informacje wypróbujemy, a w tym samouczku dowiesz się, jak uzyskać dostęp do ustawień na poziomie polecenia i połączenia bazy danych w TableAdapter.

## <a name="introduction"></a>Wprowadzenie

W całej serii samouczków korzystamy z wpisanych zestawów danych w celu zaimplementowania warstwy dostępu do danych i obiektów firmy naszej architektury warstwowej. Zgodnie z opisem w [pierwszym samouczku](../introduction/creating-a-data-access-layer-vb.md), typy danych typu zestaw danych s służą jako repozytoria dane, podczas gdy TableAdapters działa jako otoki, aby komunikować się z bazą danych w celu pobierania i modyfikowania danych źródłowych. TableAdapters hermetyzuje złożoność związaną z pracą z bazą danych i zapisuje kod, aby połączyć się z bazą danych, wydać polecenie lub wypełnić wyniki do obiektu DataTable.

Istnieją jednak przypadki, w których musimy Burrow do głębokości TableAdapter i napisać kod, który działa bezpośrednio z obiektami ADO.NET. W przypadku [modyfikacji bazy danych zawijania w ramach](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) samouczka transakcji na przykład dodaliśmy metody do TableAdapter na potrzeby rozpoczynania, zatwierdzania i wycofywania transakcji ADO.NET. Metody te używały wewnętrznego, ręcznie utworzonego obiektu `SqlTransaction`, który został przypisany do obiektów `SqlCommand` TableAdapter s.

W tym samouczku sprawdzimy, jak uzyskać dostęp do ustawień na poziomie połączenia z bazą danych i w TableAdapter. W szczególności dodamy funkcję do `ProductsTableAdapter`, która umożliwia dostęp do podstawowych parametrów połączenia i ustawień limitu czasu polecenia.

## <a name="working-with-data-using-adonet"></a>Praca z danymi przy użyciu ADO.NET

Microsoft .NET Framework zawiera mnóstwo klas przeznaczonych specjalnie do pracy z danymi. Klasy te, które znajdują się w [przestrzeni nazw`System.Data`](https://msdn.microsoft.com/library/system.data.aspx), są określane jako klasy *ADO.NET* . Niektóre klasy w ramach parasola ADO.NET są powiązane z konkretnym *dostawcą danych*. Dostawcę danych można traktować jako kanał komunikacyjny, który umożliwia przesyłanie informacji między klasami ADO.NET i bazowym magazynem danych. Istnieją uogólnioni dostawcy, takie jak OleDb i ODBC, a także dostawców specjalnie zaprojektowanych dla określonego systemu bazy danych. Na przykład, chociaż można nawiązać połączenie z bazą danych Microsoft SQL Server przy użyciu dostawcy OleDb, dostawca SqlClient jest znacznie bardziej wydajny, ponieważ został zaprojektowany i zoptymalizowany specjalnie dla SQL Server.

Podczas programistycznego uzyskiwania dostępu do danych często używany jest następujący wzorzec:

1. Nawiąż połączenie z bazą danych.
2. Wydaj polecenie.
3. W przypadku zapytań `SELECT` pracy z rekordami powstającymi.

Istnieją osobne klasy ADO.NET do wykonywania każdej z tych kroków. Aby nawiązać połączenie z bazą danych przy użyciu dostawcy SqlClient, na przykład użyj [klasy`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Aby wydać `INSERT`, `UPDATE`, `DELETE`lub `SELECT` do bazy danych, użyj [klasy`SqlCommand`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Oprócz [modyfikacji bazy danych zawijania w ramach](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) samouczka transakcji nie trzeba pisać żadnego ADO.NET kodu wypróbujemy, ponieważ kod TableAdapters automatycznie zawiera funkcje wymagane do nawiązywania połączeń z bazą danych, wydawania poleceń, pobierania danych i wypełniania tych danych w tabelach DataTables. Mogą jednak wystąpić sytuacje, w których musimy dostosować te ustawienia niskiego poziomu. W ciągu następnych kilku kroków sprawdzimy, jak wybierać obiekty ADO.NET używane wewnętrznie przez TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Krok 1: badanie przy użyciu właściwości połączenie

Każda Klasa TableAdapter ma właściwość `Connection`, która określa informacje o połączeniu z bazą danych. Ten typ danych właściwości s i wartość `ConnectionString` są określane przez wybór dokonany w Kreatorze konfiguracji TableAdapter. Odwołaj tę wartość, gdy najpierw dodamy TableAdapter do określonego zestawu danych ten Kreator zapyta nas o źródło bazy danych (patrz rysunek 1). Lista rozwijana w tym pierwszym kroku obejmuje bazy danych określone w pliku konfiguracji, a także wszystkie inne bazy danych w połączeniach danych Eksplorator serwera s. Jeśli baza danych, której chcemy użyć, nie istnieje na liście rozwijanej, można określić nowe połączenie z bazą danych, klikając przycisk nowe połączenie i dostarczając wymagane informacje o połączeniu.

[![pierwszego kroku Kreatora konfiguracji TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Rysunek 1**: pierwszy krok kreatora konfiguracji TableAdapter ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))

Poświęć chwilę na sprawdzenie kodu dla właściwości `Connection` TableAdapter s. Jak wskazano w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) , można wyświetlić wygenerowany automatycznie kod TableAdapter, przechodząc do okna widok klasy, przechodzenie do szczegółów odpowiedniej klasy, a następnie klikając dwukrotnie nazwę elementu członkowskiego.

Przejdź do okna Widok klasy, przechodząc do menu Widok i wybierając Widok klasy (lub wpisując CTRL + SHIFT + C). W górnej połowie okna Widok klasy przejdź do obszaru nazw `NorthwindTableAdapters` i wybierz klasę `ProductsTableAdapter`. Spowoduje to wyświetlenie elementów członkowskich `ProductsTableAdapter` w dolnej połowie Widok klasy, jak pokazano na rysunku 2. Kliknij dwukrotnie Właściwość `Connection`, aby wyświetlić jej kod.

![Kliknij dwukrotnie Właściwość połączenie w Widok klasy, aby wyświetlić wygenerowany automatycznie kod](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Rysunek 2**. Kliknij dwukrotnie Właściwość połączenie w widok klasy, aby wyświetlić wygenerowany automatycznie kod

Właściwość TableAdapter s `Connection` i inny kod związany z połączeniem są następujące:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Po utworzeniu wystąpienia klasy TableAdapter zmienna członkowska `_connection` jest równa `Nothing`. Po uzyskaniu dostępu do właściwości `Connection`, najpierw sprawdza, czy utworzono wystąpienie zmiennej członkowskiej `_connection`. Jeśli tak nie jest, wywoływana jest metoda `InitConnection`, która tworzy wystąpienie `_connection` i ustawia jego właściwość `ConnectionString` na wartość parametrów połączenia określoną w pierwszym kroku Kreatora konfiguracji TableAdapter.

Właściwość `Connection` można także przypisać do obiektu `SqlConnection`. Wykonanie tej operacji spowoduje skojarzenie nowego obiektu `SqlConnection` z każdym z obiektów `SqlCommand` TableAdapter.

## <a name="step-2-exposing-connection-level-settings"></a>Krok 2. udostępnianie ustawień na poziomie połączenia

Informacje o połączeniu powinny pozostać hermetyzowane w TableAdapter i nie będą dostępne dla innych warstw w architekturze aplikacji. Mogą jednak wystąpić sytuacje, w których informacje na poziomie połączenia TableAdapter s muszą być dostępne lub dostosowywalne dla strony zapytania, użytkownika lub ASP.NET.

Niech s rozszerzy `ProductsTableAdapter` w zestawie danych `Northwind` w celu uwzględnienia właściwości `ConnectionString`, która może być używana przez warstwę logiki biznesowej do odczytywania lub zmiany parametrów połączenia używanych przez TableAdapter.

> [!NOTE]
> *Parametry połączenia* to ciąg określający informacje o połączeniu z bazą danych, takie jak dostawca, który ma być używany, Lokalizacja bazy danych, poświadczenia uwierzytelniania i inne ustawienia związane z bazą danych. Aby uzyskać listę wzorców parametrów połączenia używanych przez różne magazyny danych i dostawców, zobacz [connectionStrings.com](http://www.connectionstrings.com/).

Zgodnie z opisem w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) , wpisywane automatycznie klasy zestawu danych s można rozszerzyć za pomocą klas częściowych. Najpierw utwórz nowy podfolder w projekcie o nazwie `ConnectionAndCommandSettings` pod folderem `~/App_Code/DAL`.

![Dodaj podfolder o nazwie ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Rysunek 3**. Dodawanie podfolderu o nazwie `ConnectionAndCommandSettings`

Dodaj nowy plik klasy o nazwie `ProductsTableAdapter.ConnectionAndCommandSettings.vb` i wprowadź następujący kod:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Ta klasa częściowa dodaje właściwość `Public` o nazwie `ConnectionString` do klasy `ProductsTableAdapter`, która umożliwia każdej warstwie odczytanie lub zaktualizowanie parametrów połączenia dla połączenia bazowego TableAdapter.

Z tą częściową klasą utworzono (i zapisano), Otwórz klasę `ProductsBLL`. Przejdź do jednej z istniejących metod i wpisz `Adapter` a następnie naciśnij klawisz okresu, aby wyświetlić funkcję IntelliSense. Powinna zostać wyświetlona nowa Właściwość `ConnectionString` dostępna w IntelliSense, co oznacza, że można programowo odczytać lub dostosować tę wartość z LOGIKI biznesowej.

## <a name="exposing-the-entire-connection-object"></a>Uwidacznianie całego obiektu połączenia

Ta klasa częściowa uwidacznia tylko jedną właściwość powiązanego obiektu połączenia: `ConnectionString`. Jeśli chcesz, aby cały obiekt połączenia był dostępny poza TableAdapterem, możesz alternatywnie zmienić poziom ochrony Właściwość s `Connection`. Wygenerowany automatycznie kod, który został sprawdzony w kroku 1, wykazał, że właściwość TableAdapter `Connection` s jest oznaczona jako `Friend`, co oznacza, że dostęp do nich jest możliwy tylko dla klas w tym samym zestawie. Można to zmienić, jednak za pośrednictwem właściwości `ConnectionModifier` TableAdapter s.

Otwórz zestaw danych `Northwind`, kliknij `ProductsTableAdapter` w projektancie, a następnie przejdź do okno Właściwości. Zostanie wyświetlony `ConnectionModifier` wartość domyślna, `Assembly`. Aby udostępnić Właściwość `Connection` spoza zestawu DataSet zestawu danych, Zmień właściwość `ConnectionModifier` na `Public`.

[![poziom dostępności właściwości połączenia można skonfigurować za pomocą właściwości ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Ilustracja 4**. poziom dostępności właściwości `Connection` s można skonfigurować za pomocą właściwości `ConnectionModifier` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))

Zapisz zestaw danych, a następnie wróć do klasy `ProductsBLL`. Tak jak wcześniej, przejdź do jednej z istniejących metod i wpisz `Adapter` a następnie naciśnij klawisz okresu, aby wyświetlić funkcję IntelliSense. Lista powinna zawierać właściwość `Connection`, co oznacza, że można teraz programowo odczytywać lub przypisywać ustawienia poziomu połączenia z LOGIKI biznesowej.

## <a name="step-3-examining-the-command-related-properties"></a>Krok 3: badanie właściwości związanych z poleceniem

TableAdapter składa się z głównego zapytania, które domyślnie ma automatycznie generowane instrukcje `INSERT`, `UPDATE`i `DELETE`. Te główne instrukcje `INSERT`zapytania, `UPDATE`i `DELETE` są implementowane w kodzie TableAdapter s jako obiekt karty danych ADO.NET za pośrednictwem właściwości `Adapter`. Podobnie jak w przypadku właściwości `Connection`, typ danych `Adapter` Właściwość s jest określany przez używany dostawca danych. Ponieważ te samouczki korzystają z dostawcy SqlClient, właściwość `Adapter` jest typu [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

Właściwość TableAdapter s `Adapter` ma trzy właściwości typu `SqlCommand`, które są używane do wystawiania instrukcji `INSERT`, `UPDATE`i `DELETE`:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Obiekt `SqlCommand` jest odpowiedzialny za wysyłanie określonego zapytania do bazy danych i ma właściwości, takie jak: [`CommandText`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), która zawiera instrukcję SQL ad hoc lub procedurę składowaną do wykonania; i [`Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), który jest kolekcją obiektów `SqlParameter`. Jak pokazano w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) , te obiekty poleceń można dostosować za pomocą okno właściwości.

Oprócz jego głównego zapytania, TableAdapter może zawierać zmienną liczbę metod, które po wywołaniu wysyłają określone polecenie do bazy danych. Główny obiekt polecenia zapytania s i obiekty poleceń dla wszystkich dodatkowych metod są przechowywane we właściwości `CommandCollection` s TableAdapter.

Poświęć chwilę na wyszukanie kodu wygenerowanego przez `ProductsTableAdapter` w zestawie danych `Northwind` dla tych dwóch właściwości i pomocniczych zmiennych składowych i metod pomocnika:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Kod dla właściwości `Adapter` i `CommandCollection` blisko śladuje, że właściwość `Connection`. Istnieją zmienne składowe, które zawierają obiekty używane przez właściwości. Właściwości `Get` metod dostępu zaczynają się, sprawdzając, czy odpowiadająca zmienna elementu członkowskiego jest `Nothing`. W takim przypadku wywoływana jest metoda inicjująca, która tworzy wystąpienie zmiennej składowej i przypisuje podstawowe właściwości powiązane z poleceniem.

## <a name="step-4-exposing-command-level-settings"></a>Krok 4. udostępnianie ustawień na poziomie poleceń

W idealnym przypadku informacje na poziomie poleceń powinny pozostać hermetyzowane w ramach warstwy dostępu do danych. Jeśli te informacje będą potrzebne w innych warstwach architektury, można je uwidocznić za pomocą klasy częściowej, podobnie jak w przypadku ustawień poziomu połączenia.

Ponieważ TableAdapter ma tylko jedną właściwość `Connection`, kod do udostępnienia ustawień na poziomie połączenia jest dość oczywisty. Elementy są nieco bardziej skomplikowane podczas modyfikowania ustawień na poziomie polecenia, ponieważ TableAdapter może mieć wiele obiektów poleceń — `InsertCommand`, `UpdateCommand`i `DeleteCommand`oraz zmienną liczbę obiektów poleceń we właściwości `CommandCollection`. Podczas aktualizowania ustawień na poziomie polecenia te ustawienia będą musiały zostać przekazane do wszystkich obiektów poleceń.

Załóżmy na przykład, że wystąpiły pewne zapytania w TableAdapter, które trwały długotrwały czas. W przypadku użycia TableAdapter do wykonania jednego z tych zapytań możemy chcieć zwiększyć [właściwość`CommandTimeout`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)obiektu polecenia. Ta właściwość określa liczbę sekund oczekiwania na wykonanie polecenia oraz wartość domyślną 30.

Aby zezwolić na dostosowanie właściwości `CommandTimeout` przez LOGIKI biznesowej, Dodaj następującą metodę `Public` do `ProductsDataTable` przy użyciu pliku klasy częściowej utworzonej w kroku 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Ta metoda może zostać wywołana z warstwy LOGIKI biznesowej lub prezentacji, aby ustawić limit czasu polecenia dla wszystkich poleceń przez to wystąpienie TableAdapter.

> [!NOTE]
> Właściwości `Adapter` i `CommandCollection` są oznaczone jako `Private`, co oznacza, że można uzyskać do nich dostęp tylko z kodu w TableAdapter. W przeciwieństwie do właściwości `Connection` te Modyfikatory dostępu nie są konfigurowane. W związku z tym, jeśli trzeba uwidocznić właściwości poziomu poleceń dla innych warstw w architekturze, należy użyć podejścia klasy częściowej omówionego powyżej, aby zapewnić `Public` metodę lub właściwość, która odczytuje lub zapisuje dane do obiektów poleceń `Private`.

## <a name="summary"></a>Podsumowanie

TableAdapters w określonym zestawie danych pozwala hermetyzować szczegóły i złożoność dostępu do danych. Korzystając z TableAdapters, nie musimy martwić się o pisanie kodu ADO.NET, aby połączyć się z bazą danych, wydać polecenie lub wypełnić wyniki do elementu DataTable. Jest ona automatycznie obsługiwana dla nas.

Mogą jednak wystąpić sytuacje, w których musimy dostosować ADO.NET charakterystyczne dla niskiego poziomu, takie jak zmiana parametrów połączenia lub wartości limitu czasu połączenia domyślnego lub polecenia. TableAdapter automatycznie generował `Connection`, `Adapter`i `CommandCollection` właściwości, ale domyślnie są to `Friend` lub `Private`. Te informacje wewnętrzne można uwidocznić, rozszerzając TableAdapter przy użyciu klas częściowych w celu uwzględnienia `Public` metod lub właściwości. Alternatywnie modyfikator dostępu właściwości TableAdapter `Connection` można skonfigurować za pomocą właściwości `ConnectionModifier` s.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Burnadette Leigh, S Ren Jacob Lauritsen, Teresa Murphy i Hilton Geisenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](working-with-computed-columns-vb.md)
> [dalej](protecting-connection-strings-and-other-configuration-information-vb.md)
