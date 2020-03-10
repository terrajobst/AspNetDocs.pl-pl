---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Buforowanie danych przy uruchamianiu aplikacji (VB) | Microsoft Docs
author: rick-anderson
description: Niektóre dane aplikacji sieci Web są często używane, a niektóre dane będą rzadko używane. Możemy ulepszyć wydajność naszej aplikacji ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c07b565329ab17496d2436f4c35bc4507694ed8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576854"
---
# <a name="caching-data-at-application-startup-vb"></a>Buforowanie danych przy uruchamianiu aplikacji (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> Niektóre dane aplikacji sieci Web są często używane, a niektóre dane będą rzadko używane. Możemy ulepszyć wydajność naszej aplikacji ASP.NET, ładując z wyprzedzeniem często używane dane, technikę znaną jako. W tym samouczku przedstawiono jedno podejście do aktywnego ładowania, które polega na załadowaniu danych do pamięci podręcznej podczas uruchamiania aplikacji.

## <a name="introduction"></a>Wprowadzenie

Dwa poprzednie samouczki szukały buforowania danych w prezentacji i warstwach pamięci podręcznej. W pamięci podręcznej [danych za pomocą elementu ObjectDataSource](caching-data-with-the-objectdatasource-vb.md)oglądamy korzystanie z funkcji buforowania elementów ObjectDataSource do buforowania danych w warstwie prezentacji. [Buforowanie danych w architekturze](caching-data-in-the-architecture-vb.md) zbada buforowanie w nowej, oddzielnej warstwie pamięci podręcznej. Oba z tych samouczków używały *reaktywnego ładowania* podczas pracy z pamięcią podręczną danych. Przy próbie ponownego załadowania, za każdym razem, gdy dane są żądane, system najpierw sprawdza, czy w pamięci podręcznej jest to dysk. W przeciwnym razie dane pochodzą ze źródła źródłowego, takiego jak baza danych, a następnie są przechowywane w pamięci podręcznej. Główną zaletą dla reaktywnego ładowania jest jego łatwość wdrożenia. Jedną z jej wad jest nierówna wydajność między żądaniami. Załóżmy, że strona, która używa warstwy buforowania z poprzedniego samouczka, umożliwia wyświetlenie informacji o produkcie. Gdy ta strona jest odwiedzana po raz pierwszy lub po raz pierwszy po usunięciu danych z pamięci podręcznej z powodu ograniczeń pamięci lub osiągnięcia określonego czasu wygaśnięcia, dane muszą zostać pobrane z bazy danych. W związku z tym te żądania użytkowników będą trwać dłużej niż żądania użytkowników, które mogą być obsługiwane przez pamięć podręczną.

Funkcja *aktywnego ładowania* zapewnia niezbędną strategię zarządzania pamięcią podręczną, która wygładza wydajność między żądaniami, ładując dane z pamięci podręcznej przed zainstalowaniem ich. Zazwyczaj proaktywne ładowanie używa pewnego procesu, który okresowo sprawdza lub jest powiadamiany, gdy została przekazana aktualizacja danych źródłowych. Następnie ten proces aktualizuje pamięć podręczną, aby zachować jej świeżość. Proaktywne ładowanie jest szczególnie przydatne, jeśli dane podstawowe pochodzą z wolnego połączenia z bazą danych, usługi sieci Web lub innego, szczególnie powolnego źródła danych. Jednak to podejście do aktywnego ładowania jest trudniejsze do zaimplementowania, ponieważ wymaga tworzenia, zarządzania i wdrażania procesu w celu sprawdzenia pod kątem zmian i zaktualizowania pamięci podręcznej.

Inna wersja aktywnego ładowania i typ, który będziemy eksplorować w tym samouczku, ładuje dane do pamięci podręcznej podczas uruchamiania aplikacji. To podejście jest szczególnie przydatne w przypadku buforowania danych statycznych, takich jak rekordy w tabelach odnośników bazy danych.

> [!NOTE]
> Dokładniejsze Omówienie różnic między aktywnym i reaktywnym ładowaniem, a także listami informatyków, wad i zaleceń dotyczących implementacji znajduje się w sekcji [Zarządzanie zawartością pamięci podręcznej](https://msdn.microsoft.com/library/ms978503.aspx) w [przewodniku po architekturze buforowania dla .NET Framework aplikacji](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Krok 1. Określanie, jakie dane mają być buforowane podczas uruchamiania aplikacji

Przykłady buforowania korzystające z ponownej ładowania, które zostały zbadane w poprzednich dwóch samouczkach, działają prawidłowo i dane mogą być okresowo zmieniane i nie trwają exorbitantly. Ale jeśli dane w pamięci podręcznej nigdy nie ulegną zmianie, wygaśnięcie czasu używane przez ponowne ładowanie jest zbędne. Podobnie, jeśli dane przechowywane w pamięci podręcznej zajmują zbyt dużo czasu na wygenerowanie, wówczas Ci użytkownicy, których żądania znajdą pustą pamięć podręczną, będą musieli nadmiernie poczekać podczas pobierania danych źródłowych. Należy rozważyć buforowanie danych statycznych i danych, które trwają wyjątkowo długo do generowania podczas uruchamiania aplikacji.

Bazy danych mają wiele dynamicznych, często zmieniających się wartości, a większość z nich również ma godziwą ilość danych statycznych. Na przykład praktycznie wszystkie modele danych mają jedną lub więcej kolumn, które zawierają określoną wartość z ustalonego zestawu opcji. Tabela `Patients` bazy danych może mieć kolumnę `PrimaryLanguage`, której zestaw wartości może być w języku angielskim, hiszpańskim, francuskim, japońskim i tak dalej. Często, te typy kolumn są implementowane przy użyciu *tabel odnośników*. Zamiast przechowywania ciągu w języku angielskim lub francuskim w tabeli `Patients` jest tworzona druga tabela, która ma często dwie kolumny — unikatowy identyfikator i opis ciągu — z rekordem dla każdej możliwej wartości. Kolumna `PrimaryLanguage` w tabeli `Patients` przechowuje odpowiedni unikatowy identyfikator w tabeli odnośników. Na rysunku 1 język podstawowy pacjenta John nos jest w języku angielskim, a Ed Johnsonem s jest rosyjski.

![Tabela języków jest tabelą odnośników używaną w tabeli pacjentów](caching-data-at-application-startup-vb/_static/image1.png)

**Rysunek 1**: tabela `Languages` jest tabelą odnośników używaną przez tabelę `Patients`

Interfejs użytkownika służący do edytowania lub tworzenia nowego pacjenta będzie zawierać listę rozwijaną z dozwolonych języków wypełnianych przez rekordy w tabeli `Languages`. Bez buforowania, za każdym razem, gdy ten interfejs jest odwiedzany, system musi wykonać zapytanie do tabeli `Languages`. Jest to wasteful i niepotrzebne, ponieważ wartości tabeli odnośników zmieniają się bardzo rzadko, jeśli kiedykolwiek wcześniej.

Możemy buforować `Languages` dane przy użyciu tych samych, reaktywnych technik ładowania, które zostały sprawdzone w poprzednich samouczkach. Ponowne ładowanie nie jest jednak zgodne z upływem czasu, który nie jest wymagany w przypadku danych tabeli odnośników statycznych. Buforowanie przy użyciu ponownej ładowania jest lepiej niż w przypadku braku buforowania, najlepszym rozwiązaniem jest załadowanie danych tabeli wyszukiwania do pamięci podręcznej podczas uruchamiania aplikacji.

W tym samouczku pokazano, jak buforować dane tabeli odnośników i inne informacje statyczne.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Krok 2: badanie różnych sposobów buforowania danych

Informacje mogą być programowane w pamięci podręcznej w aplikacji ASP.NET przy użyciu różnych metod. W poprzednich samouczkach zaobserwowano już korzystanie z pamięci podręcznej danych. Alternatywnie obiekty mogą być programowane w pamięci podręcznej przy użyciu *statycznych elementów członkowskich* lub *stanu aplikacji*.

Podczas pracy z klasą, zazwyczaj należy najpierw utworzyć wystąpienie klasy przed uzyskaniem dostępu do jej elementów członkowskich. Na przykład w celu wywołania metody z jednej z klas w naszej warstwie logiki biznesowej należy najpierw utworzyć wystąpienie klasy:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Przed wywołaniem *SomeMethod* lub współpracują z *SomeProperty*należy najpierw utworzyć wystąpienie klasy za pomocą słowa kluczowego `New`. *SomeMethod* i *SomeProperty* są skojarzone z określonym wystąpieniem. Okres istnienia tych członków jest powiązany z okresem istnienia skojarzonego z nim obiektu. *Statyczne składowe*, z drugiej strony, to zmienne, właściwości i metody, które są współużytkowane przez *wszystkie* wystąpienia klasy, a w związku z tym mają okres istnienia, tak długo, jak Klasa. Statyczne elementy członkowskie są oznaczane przez `Shared`słowa kluczowego.

Oprócz statycznych elementów członkowskich dane mogą być buforowane przy użyciu stanu aplikacji. Każda aplikacja ASP.NET przechowuje kolekcję nazw/wartości, które są współużytkowane przez wszystkich użytkowników i strony aplikacji. Dostęp do tej kolekcji można uzyskać przy użyciu właściwości [`HttpContext` klasy](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [`Application`](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)i używanej ze strony z kodem "ASP.NET Page s", takiej jak:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

Pamięć podręczna danych zapewnia znacznie bogatszy interfejs API służący do buforowania danych, zapewniając mechanizmy dla wygasających czasu i zależności, priorytety elementów pamięci podręcznej i tak dalej. Ze statycznymi składowymi i stanem aplikacji takie funkcje muszą być ręcznie dodawane przez dewelopera strony. W przypadku buforowania danych przy uruchamianiu aplikacji przez okres istnienia aplikacji, jednak zalety pamięci podręcznej danych to Moot. W tym samouczku zaobserwujemy kod, który używa wszystkich trzech technik buforowania danych statycznych.

## <a name="step-3-caching-thesupplierstable-data"></a>Krok 3. buforowanie danych tabeli`Suppliers`

Tabele bazy danych Northwind zaimplementowane do daty nie zawierają żadnych tradycyjnych tabel odnośników. Cztery tabeli DataTables zaimplementowane w naszej DAL wszystkie tabele modelu, których wartości nie są statyczne. Zamiast poświęcać czas na dodanie nowej tabeli DataTable do DAL, a następnie nowej klasy i metod do LOGIKI biznesowej, w tym samouczku, że dane tabeli `Suppliers` są statyczne. W związku z tym możemy buforować te dane przy uruchamianiu aplikacji.

Aby rozpocząć, Utwórz nową klasę o nazwie `StaticCache.cs` w folderze `CL`.

![Tworzenie klasy StaticCache. vb w folderze CL](caching-data-at-application-startup-vb/_static/image2.png)

**Rysunek 2**. tworzenie klasy `StaticCache.vb` w folderze `CL`

Musimy dodać metodę, która ładuje dane przy uruchamianiu do odpowiedniego magazynu pamięci podręcznej, a także metod, które zwracają dane z tej pamięci podręcznej.

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Powyższy kod używa statycznej zmiennej składowej `suppliers`, aby przechowywać wyniki z metody `GetSuppliers()` klasy `SuppliersBLL`, która jest wywoływana z metody `LoadStaticCache()`. Metoda `LoadStaticCache()` ma być wywoływana podczas uruchamiania aplikacji. Gdy te dane zostaną załadowane podczas uruchamiania aplikacji, każda Strona, która musi pracować z danymi dostawcy, może wywołać metodę `GetSuppliers()` `StaticCache` klasy s. W związku z tym połączenie z bazą danych, aby uzyskać dostawców, odbywa się tylko raz, podczas uruchamiania aplikacji.

Zamiast używać statycznej zmiennej składowej jako magazynu pamięci podręcznej, możemy użyć Alternatywnie stanu aplikacji lub pamięci podręcznej danych. Poniższy kod przedstawia klasę reprezentowaną przez narzędzie do korzystania z stanu aplikacji:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

W `LoadStaticCache()`informacje o dostawcach są przechowywane w *kluczu*zmiennej aplikacji. Element s został zwrócony jako odpowiedni typ (`Northwind.SuppliersDataTable`) z `GetSuppliers()`. Gdy stan aplikacji jest dostępny w klasach ASP.NET stron w kodzie, przy użyciu `Application("key")`, w architekturze należy użyć `HttpContext.Current.Application("key")`, aby uzyskać bieżące `HttpContext`.

Podobnie pamięć podręczna danych może być używana jako magazyn pamięci podręcznej, jak pokazano w poniższym kodzie:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Aby dodać element do pamięci podręcznej danych bez wygaśnięcia opartego na czasie, użyj wartości `System.Web.Caching.Cache.NoAbsoluteExpiration` i `System.Web.Caching.Cache.NoSlidingExpiration` jako parametrów wejściowych. To konkretne Przeciążenie metody `Insert` pamięci podręcznej danych została wybrana, aby można było określić *priorytet* elementu pamięci podręcznej. Priorytet jest używany do określenia elementów, które mają być oczyszczane z pamięci podręcznej, gdy ilość dostępnej pamięci jest niska. Tutaj korzystamy z priorytetu `NotRemovable`, co gwarantuje, że ten element pamięci podręcznej nie zostanie usunięty.

> [!NOTE]
> Ten samouczek s Pobierz implementuje klasę `StaticCache` przy użyciu podejścia statycznej zmiennej składowej. Kod dla technik dotyczących stanu aplikacji i pamięci podręcznej danych jest dostępny w komentarzach w pliku klasy.

## <a name="step-4-executing-code-at-application-startup"></a>Krok 4. wykonywanie kodu podczas uruchamiania aplikacji

Aby wykonać kod podczas pierwszego uruchomienia aplikacji sieci Web, musimy utworzyć specjalny plik o nazwie `Global.asax`. Ten plik może zawierać programy obsługi zdarzeń dla zdarzeń aplikacji, sesji i na poziomie żądania. w tym miejscu można dodać kod, który będzie wykonywany przy każdym uruchomieniu aplikacji.

Dodaj plik `Global.asax` do katalogu głównego aplikacji sieci Web, klikając prawym przyciskiem myszy nazwę projektu witryny sieci Web w programie Visual Studio s Eksplorator rozwiązań i wybierając polecenie Dodaj nowy element. W oknie dialogowym Dodaj nowy element wybierz pozycję Globalna Klasa aplikacji typ, a następnie kliknij przycisk Dodaj.

> [!NOTE]
> Jeśli masz już plik `Global.asax` w projekcie, typ elementu globalnej klasy aplikacji nie będzie wyświetlany w oknie dialogowym Dodaj nowy element.

[![dodać pliku Global. asax do katalogu głównego aplikacji sieci Web](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Rysunek 3**. dodawanie pliku `Global.asax` do katalogu głównego aplikacji sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image5.png))

Domyślny szablon pliku `Global.asax` zawiera pięć metod w tagu `<script>` po stronie serwera:

- **`Application_Start`** wykonywane po pierwszym uruchomieniu aplikacji sieci Web
- **`Application_End`** jest uruchamiany podczas zamykania aplikacji
- **`Application_Error`** wykonuje się za każdym razem, gdy nieobsługiwany wyjątek osiągnie aplikację
- **`Session_Start`** wykonywane po utworzeniu nowej sesji
- **`Session_End`** uruchamiany, gdy sesja wygasła lub została porzucona

Procedura obsługi zdarzeń `Application_Start` jest wywoływana tylko raz podczas cyklu życia aplikacji. Aplikacja uruchamia się przy pierwszym żądaniu zasobu ASP.NET z aplikacji i kontynuuje działanie do momentu ponownego uruchomienia aplikacji, co może być spowodowane przez modyfikację zawartości folderu `/Bin`, zmodyfikowanie `Global.asax`, zmodyfikowanie zawartości w folderze `App_Code` lub zmodyfikowanie `Web.config` pliku, między innymi przyczynami. Więcej szczegółowych informacji na temat cyklu życia aplikacji można znaleźć w temacie [ASP.NET cykl życia aplikacji](https://msdn.microsoft.com/library/ms178473.aspx) .

W przypadku tych samouczków wystarczy dodać kod do metody `Application_Start`, więc możesz usunąć inne. W `Application_Start`, po prostu wywołaj metodę `LoadStaticCache()` klasy `StaticCache`, która będzie ładować i przechować informacje o dostawcy:

[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Wszystko, co wszystko jest gotowe! Przy uruchamianiu aplikacji `LoadStaticCache()` Metoda pokryje informacje o dostawcy z LOGIKI biznesowej i zapisze je w statycznej zmiennej składowej (lub dowolnego magazynu pamięci podręcznej, który został zakończony przy użyciu w klasie `StaticCache`). Aby sprawdzić to zachowanie, ustaw punkt przerwania w metodzie `Application_Start` i uruchom aplikację. Należy zauważyć, że punkt przerwania został trafiony podczas uruchamiania aplikacji. Kolejne żądania nie powodują jednak wykonania metody `Application_Start`.

[![użyć punktu przerwania, aby sprawdzić, czy program obsługi zdarzeń Application_Start jest wykonywany](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Rysunek 4**. Aby sprawdzić, czy program obsługi zdarzeń `Application_Start` jest wykonywany, użyj punktu przerwania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image8.png))

> [!NOTE]
> Jeśli punkt przerwania `Application_Start` nie zostanie osiągnięty podczas pierwszego uruchomienia debugowania, jest to spowodowane tym, że aplikacja została już uruchomiona. Wymuś ponowne uruchomienie aplikacji, modyfikując `Global.asax` lub `Web.config` plików, a następnie spróbuj ponownie. Możesz po prostu dodać (lub usunąć) pusty wiersz na końcu jednego z tych plików, aby szybko ponownie uruchomić aplikację.

## <a name="step-5-displaying-the-cached-data"></a>Krok 5. Wyświetlanie danych z pamięci podręcznej

W tym momencie Klasa `StaticCache` ma wersję danych dostawcy buforowaną podczas uruchamiania aplikacji, do której można uzyskać dostęp za pomocą metody `GetSuppliers()`. Aby można było korzystać z tych danych z warstwy prezentacji, możemy użyć elementu ObjectDataSource lub programowo wywołać metodę `StaticCache` Class s `GetSuppliers()` z klasy z kodem ASP.NET Page s. Zezwól na używanie formantów ObjectDataSource i GridView do wyświetlania informacji o buforowanym dostawcy.

Zacznij od otwarcia strony `AtApplicationStartup.aspx` w folderze `Caching`. Przeciągnij widok GridView z przybornika do projektanta, ustawiając jego właściwość `ID` na `Suppliers`. Następnie w tagu inteligentnym GridView ' Wybierz, aby utworzyć nowy element ObjectDataSource o nazwie `SuppliersCachedDataSource`. Skonfiguruj element ObjectDataSource, aby używał metody `GetSuppliers()` `StaticCache` Class.

[![skonfigurować element ObjectDataSource do używania klasy StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Rysunek 5**. Konfigurowanie elementu ObjectDataSource do używania klasy `StaticCache` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image11.png))

[![użyć metody getdostawcy (), aby pobrać dane z pamięci podręcznej](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Ilustracja 6**. użyj metody `GetSuppliers()`, aby pobrać dane z pamięci podręcznej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image14.png))

Po zakończeniu działania kreatora program Visual Studio automatycznie doda BoundFields dla każdego z pól danych w `SuppliersDataTable`. Znaczniki deklaratywne GridView i ObjectDataSource s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Rysunek 7 przedstawia stronę wyświetlaną w przeglądarce. Dane wyjściowe są takie same, gdyby zostały pobrane z klasy LOGIKI biznesowej s `SuppliersBLL`, ale użycie klasy `StaticCache` zwraca dane dostawcy jako buforowane podczas uruchamiania aplikacji. Aby sprawdzić to zachowanie, można ustawić punkty przerwania w metodzie `GetSuppliers()` `StaticCache` Class.

[![dane dostawcy w pamięci podręcznej są wyświetlane w widoku GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Rysunek 7**. dane dostawcy w pamięci podręcznej są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-at-application-startup-vb/_static/image17.png))

## <a name="summary"></a>Podsumowanie

Większość każdego modelu danych zawiera godziwą ilość danych statycznych, zazwyczaj zaimplementowaną w postaci tabel odnośników. Ponieważ te informacje są statyczne, nie ma powodu stałego dostępu do bazy danych za każdym razem, gdy te informacje muszą być wyświetlane. Ponadto ze względu na jego charakter statyczny, gdy buforowanie danych nie jest konieczne. W tym samouczku przedstawiono sposób wykonywania takich danych i buforowania ich w pamięci podręcznej danych, stanie aplikacji oraz statycznej zmiennej składowej. Te informacje są buforowane przy uruchamianiu aplikacji i pozostają w pamięci podręcznej przez cały okres istnienia aplikacji.

W tym samouczku i w ciągu ostatnich dwóch będziemy korzystać z pamięci podręcznej na potrzeby okresu istnienia aplikacji, a także przy użyciu wygasających czasu. W przypadku buforowania danych bazy danych czas wygaśnięcia oparty na czasie może być mniejszy niż idealny. Zamiast okresowego opróżniania pamięci podręcznej, najlepszym rozwiązaniem jest wykluczenie tylko buforowanego elementu, gdy dane źródłowej bazy danych są modyfikowane. Jest to idealne rozwiązanie przy użyciu zależności pamięci podręcznej SQL, które będziemy analizować w następnym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Teresa Murphy i Zack Kowalski. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-in-the-architecture-vb.md)
> [dalej](using-sql-cache-dependencies-vb.md)
