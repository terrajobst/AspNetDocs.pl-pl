---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: Buforowanie danych przy uruchamianiu aplikacjiC#() | Microsoft Docs
author: rick-anderson
description: Niektóre dane aplikacji sieci Web są często używane, a niektóre dane będą rzadko używane. Możemy ulepszyć wydajność naszej aplikacji ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b55b0df1b7843120de284891e16178df23fabe
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70386563"
---
# <a name="caching-data-at-application-startup-c"></a>Buforowanie danych przy uruchamianiu aplikacji (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> Niektóre dane aplikacji sieci Web są często używane, a niektóre dane będą rzadko używane. Możemy ulepszyć wydajność naszej aplikacji ASP.NET, ładując z wyprzedzeniem często używane dane, technikę znaną jako pamięć podręczna. W tym samouczku przedstawiono jedno podejście do aktywnego ładowania, które polega na załadowaniu danych do pamięci podręcznej podczas uruchamiania aplikacji.

## <a name="introduction"></a>Wprowadzenie

Dwa poprzednie samouczki szukały buforowania danych w prezentacji i warstwach pamięci podręcznej. W obszarze [buforowanie danych za](caching-data-with-the-objectdatasource-cs.md)pomocą elementu ObjectDataSource oglądamy użycie funkcji pamięci podręcznej ObjectDataSource do buforowania danych w warstwie prezentacji. [Buforowanie danych w architekturze](caching-data-in-the-architecture-cs.md) zbada buforowanie w nowej, oddzielnej warstwie pamięci podręcznej. Oba z tych samouczków używały *reaktywnego ładowania* podczas pracy z pamięcią podręczną danych. Przy próbie ponownego załadowania, za każdym razem, gdy dane są żądane, system najpierw sprawdza, czy znajduje się w pamięci podręcznej. W przeciwnym razie dane pochodzą ze źródła źródłowego, takiego jak baza danych, a następnie są przechowywane w pamięci podręcznej. Główną zaletą dla reaktywnego ładowania jest jego łatwość wdrożenia. Jedną z jej wad jest nierówna wydajność między żądaniami. Załóżmy, że strona, która używa warstwy buforowania z poprzedniego samouczka, umożliwia wyświetlenie informacji o produkcie. Gdy ta strona jest odwiedzana po raz pierwszy lub po raz pierwszy po usunięciu danych z pamięci podręcznej z powodu ograniczeń pamięci lub osiągnięcia określonego czasu wygaśnięcia, dane muszą zostać pobrane z bazy danych. W związku z tym te żądania użytkowników będą trwać dłużej niż żądania użytkowników, które mogą być obsługiwane przez pamięć podręczną.

Funkcja *aktywnego ładowania* zapewnia niezbędną strategię zarządzania pamięcią podręczną, która wygładza wydajność między żądaniami przez załadowanie danych z pamięci podręcznej przed ich zainstalowaniem. Zazwyczaj proaktywne ładowanie używa pewnego procesu, który okresowo sprawdza lub jest powiadamiany, gdy została przekazana aktualizacja danych źródłowych. Następnie ten proces aktualizuje pamięć podręczną, aby zachować jej świeżość. Proaktywne ładowanie jest szczególnie przydatne, jeśli dane podstawowe pochodzą z wolnego połączenia z bazą danych, usługi sieci Web lub innego, szczególnie powolnego źródła danych. Jednak to podejście do aktywnego ładowania jest trudniejsze do zaimplementowania, ponieważ wymaga tworzenia, zarządzania i wdrażania procesu w celu sprawdzenia pod kątem zmian i zaktualizowania pamięci podręcznej.

Inna wersja aktywnego ładowania i typ, który będziemy eksplorować w tym samouczku, ładuje dane do pamięci podręcznej podczas uruchamiania aplikacji. To podejście jest szczególnie przydatne w przypadku buforowania danych statycznych, takich jak rekordy w tabelach odnośników bazy danych.

> [!NOTE]
> Dokładniejsze Omówienie różnic między aktywnym i reaktywnym ładowaniem, a także listami informatyków, wad i zaleceń dotyczących implementacji można znaleźć w sekcji [Zarządzanie zawartością pamięci podręcznej](https://msdn.microsoft.com/library/ms978503.aspx) w [przewodniku po architekturze platformy .NET. Aplikacje struktury](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Krok 1. Określanie, które dane mają być buforowane przy uruchamianiu aplikacji

Przykłady buforowania korzystające z ponownej ładowania, które zostały zbadane w poprzednich dwóch samouczkach, działają prawidłowo i dane mogą być okresowo zmieniane i nie trwają exorbitantly. Ale jeśli dane w pamięci podręcznej nigdy nie ulegną zmianie, wygaśnięcie czasu używane przez ponowne ładowanie jest zbędne. Podobnie, jeśli dane przechowywane w pamięci podręcznej zajmują zbyt dużo czasu na wygenerowanie, wówczas Ci użytkownicy, których żądania znajdą pustą pamięć podręczną, będą musieli nadmiernie poczekać podczas pobierania danych źródłowych. Należy rozważyć buforowanie danych statycznych i danych, które trwają wyjątkowo długo do generowania podczas uruchamiania aplikacji.

Bazy danych mają wiele dynamicznych, często zmieniających się wartości, a większość z nich również ma godziwą ilość danych statycznych. Na przykład praktycznie wszystkie modele danych mają jedną lub więcej kolumn, które zawierają określoną wartość z ustalonego zestawu opcji. Tabela bazy danych może mieć kolumnę, której zestaw wartości może być w języku angielskim, hiszpańskim, francuskim, japońskim i tak dalej. `PrimaryLanguage` `Patients` Często, te typy kolumn są implementowane przy użyciu *tabel odnośników*. Zamiast przechowywania ciągu w języku angielskim lub francuskim w `Patients` tabeli, tworzona jest druga tabela, która ma często dwie kolumny — unikatowy identyfikator i opis ciągu — z rekordem dla każdej możliwej wartości. `PrimaryLanguage` Kolumna`Patients` w tabeli przechowuje odpowiedni unikatowy identyfikator w tabeli odnośników. Na rysunku 1 język podstawowy pacjenta John Nowak jest w języku angielskim, a Ed Johnsonem jest rosyjski.

![Tabela języków jest tabelą odnośników używaną w tabeli pacjentów](caching-data-at-application-startup-cs/_static/image1.png)

**Rysunek 1**. Tabela jest tabelą odnośników używaną `Patients` w tabeli `Languages`

Interfejs użytkownika służący do edytowania lub tworzenia nowego pacjenta będzie zawierać listę rozwijaną z dozwolonych języków wypełnianych przez rekordy w `Languages` tabeli. Bez buforowania, za każdym razem, gdy ten interfejs jest odwiedzany `Languages` , system musi wykonać zapytanie do tabeli. Jest to wasteful i niepotrzebne, ponieważ wartości tabeli odnośników zmieniają się bardzo rzadko, jeśli kiedykolwiek wcześniej.

Możemy buforować `Languages` dane przy użyciu tych samych, reaktywnych technik ładowania, które zostały sprawdzone w poprzednich samouczkach. Ponowne ładowanie nie jest jednak zgodne z upływem czasu, który nie jest wymagany w przypadku danych tabeli odnośników statycznych. Buforowanie przy użyciu ponownej ładowania jest lepiej niż w przypadku braku buforowania, najlepszym rozwiązaniem jest załadowanie danych tabeli wyszukiwania do pamięci podręcznej podczas uruchamiania aplikacji.

W tym samouczku pokazano, jak buforować dane tabeli odnośników i inne informacje statyczne.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Krok 2. Badanie różnych sposobów buforowania danych

Informacje mogą być programowane w pamięci podręcznej w aplikacji ASP.NET przy użyciu różnych metod. W poprzednich samouczkach zaobserwowano już korzystanie z pamięci podręcznej danych. Alternatywnie obiekty mogą być programowane w pamięci podręcznej przy użyciu *statycznych elementów członkowskich* lub *stanu aplikacji*.

Podczas pracy z klasą, zazwyczaj należy najpierw utworzyć wystąpienie klasy przed uzyskaniem dostępu do jej elementów członkowskich. Na przykład w celu wywołania metody z jednej z klas w naszej warstwie logiki biznesowej należy najpierw utworzyć wystąpienie klasy:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Przed wywołaniem funkcji *SomeMethod* lub pracy z *SomeProperty*należy najpierw utworzyć wystąpienie `new` klasy za pomocą słowa kluczowego. *SomeMethod* i *SomeProperty* są skojarzone z określonym wystąpieniem. Okres istnienia tych członków jest powiązany z okresem istnienia skojarzonego z nim obiektu. *Statyczne składowe*, z drugiej strony, to zmienne, właściwości i metody, które są współużytkowane przez *wszystkie* wystąpienia klasy, a w związku z tym mają okres istnienia, tak długo, jak Klasa. Statyczne elementy członkowskie są oznaczane słowem kluczowym `static`.

Oprócz statycznych elementów członkowskich dane mogą być buforowane przy użyciu stanu aplikacji. Każda aplikacja ASP.NET obsługuje kolekcję nazwa/wartość, która jest współużytkowana przez wszystkich użytkowników i strony aplikacji. Dostęp do tej kolekcji można uzyskać przy [ `HttpContext` ](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)użyciu [ `Application` właściwości](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)klasy i używanej z klasy ASP.NET strony z kodem, tak więc:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

Pamięć podręczna danych zapewnia znacznie bogatszy interfejs API służący do buforowania danych, zapewniając mechanizmy dla wygasających czasu i zależności, priorytety elementów pamięci podręcznej i tak dalej. Ze statycznymi składowymi i stanem aplikacji takie funkcje muszą być ręcznie dodawane przez dewelopera strony. W przypadku buforowania danych podczas uruchamiania aplikacji przez okres istnienia aplikacji Niemniej jednak zalety pamięci podręcznej danych to Moot. W tym samouczku zaobserwujemy kod, który używa wszystkich trzech technik buforowania danych statycznych.

## <a name="step-3-caching-thesupplierstable-data"></a>Krok 3. Buforowanie danych`Suppliers`tabeli

Tabele bazy danych Northwind zaimplementowane do daty nie zawierają żadnych tradycyjnych tabel odnośników. Cztery tabeli DataTables zaimplementowane w naszej DAL wszystkie tabele modelu, których wartości nie są statyczne. Zamiast poświęcać czas na dodanie nowej tabeli DataTable do dal, a następnie nowej klasy i metod do logiki biznesowej, na potrzeby tego samouczka się, że `Suppliers` dane tabeli są statyczne. W związku z tym możemy buforować te dane przy uruchamianiu aplikacji.

Aby rozpocząć, Utwórz nową klasę o nazwie `StaticCache.cs` `CL` w folderze.

![Tworzenie klasy StaticCache.cs w folderze CL](caching-data-at-application-startup-cs/_static/image2.png)

**Rysunek 2**. `StaticCache.cs` Utwórz klasę`CL` w folderze

Musimy dodać metodę, która ładuje dane przy uruchamianiu do odpowiedniego magazynu pamięci podręcznej, a także metod, które zwracają dane z tej pamięci podręcznej.

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

Powyższy kod używa statycznej zmiennej składowej `suppliers`,, aby przechowywać wyniki `SuppliersBLL` z `GetSuppliers()` metody klasy, która jest wywoływana z `LoadStaticCache()` metody. `LoadStaticCache()` Metoda jest przeznaczona do wywołania podczas uruchamiania aplikacji. Gdy te dane zostaną załadowane podczas uruchamiania aplikacji, każda Strona, która wymaga pracy z danymi dostawcy, może wywołać `StaticCache` `GetSuppliers()` metodę klasy. W związku z tym połączenie z bazą danych, aby uzyskać dostawców, odbywa się tylko raz, podczas uruchamiania aplikacji.

Zamiast używać statycznej zmiennej składowej jako magazynu pamięci podręcznej, możemy użyć Alternatywnie stanu aplikacji lub pamięci podręcznej danych. Poniższy kod przedstawia klasę reprezentowaną przez narzędzie do korzystania z stanu aplikacji:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

W `LoadStaticCache()`programie Informacje o dostawcach są przechowywane w *kluczu*zmiennej aplikacji. Jest zwracany jako odpowiedni typ (`Northwind.SuppliersDataTable`) z. `GetSuppliers()` Gdy stan aplikacji jest dostępny w klasach ASP.NET stron za pomocą `Application["key"]`programu, w architekturze, do których należy użyć `HttpContext.Current.Application["key"]` , aby uzyskać aktualność `HttpContext`.

Podobnie pamięć podręczna danych może być używana jako magazyn pamięci podręcznej, jak pokazano w poniższym kodzie:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Aby dodać element do pamięci podręcznej danych bez czasu wygaśnięcia opartego na czasie, użyj `System.Web.Caching.Cache.NoAbsoluteExpiration` wartości `System.Web.Caching.Cache.NoSlidingExpiration` i jako parametrów wejściowych. To konkretne Przeciążenie `Insert` metody pamięci podręcznej danych zostało zaznaczone, aby można było określić *priorytet* elementu pamięci podręcznej. Priorytet jest używany do określenia elementów, które mają być oczyszczane z pamięci podręcznej, gdy ilość dostępnej pamięci jest niska. Tutaj korzystamy z priorytetu `NotRemovable`, który zapewnia, że ten element pamięci podręcznej nie zostanie usunięty.

> [!NOTE]
> Ten samouczek zawiera implementację `StaticCache` klasy przy użyciu podejścia statycznej zmiennej składowej. Kod dla technik dotyczących stanu aplikacji i pamięci podręcznej danych jest dostępny w komentarzach w pliku klasy.

## <a name="step-4-executing-code-at-application-startup"></a>Krok 4. Wykonywanie kodu podczas uruchamiania aplikacji

Aby wykonać kod podczas pierwszego uruchomienia aplikacji sieci Web, musimy utworzyć specjalny plik o nazwie `Global.asax`. Ten plik może zawierać programy obsługi zdarzeń dla zdarzeń aplikacji, sesji i na poziomie żądania. w tym miejscu można dodać kod, który będzie wykonywany przy każdym uruchomieniu aplikacji.

Aby dodać `Global.asax` plik do katalogu głównego aplikacji sieci Web, kliknij prawym przyciskiem myszy nazwę projektu witryny sieci Web w Eksplorator rozwiązań programu Visual Studio i wybierz polecenie Dodaj nowy element. W oknie dialogowym Dodaj nowy element wybierz pozycję Globalna Klasa aplikacji typ, a następnie kliknij przycisk Dodaj.

> [!NOTE]
> Jeśli masz `Global.asax` już plik w projekcie, typ elementu globalnej klasy aplikacji nie będzie wyświetlany w oknie dialogowym Dodaj nowy element.

[![Dodaj plik Global. asax do katalogu głównego aplikacji sieci Web](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Rysunek 3**. Dodaj plik do katalogu głównego aplikacji sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze)](caching-data-at-application-startup-cs/_static/image5.png) `Global.asax`

Domyślny `Global.asax` szablon pliku zawiera pięć metod w tagu po stronie `<script>` serwera:

- **`Application_Start`** wykonuje się po pierwszym uruchomieniu aplikacji sieci Web
- **`Application_End`** uruchamiany podczas zamykania aplikacji
- **`Application_Error`** wykonuje się za każdym razem, gdy nieobsługiwany wyjątek osiągnie aplikację
- **`Session_Start`** wykonuje się, gdy zostanie utworzona nowa sesja
- **`Session_End`** uruchamiany, gdy sesja wygasła lub została porzucona

Procedura `Application_Start` obsługi zdarzeń jest wywoływana tylko raz podczas cyklu życia aplikacji. Aplikacja uruchamia się przy pierwszym żądaniu zasobu ASP.NET z aplikacji i kontynuuje działanie do momentu ponownego uruchomienia aplikacji, co może się zdarzyć, modyfikując zawartość `/Bin` folderu, modyfikując `Global.asax`, modyfikując zawartość w `App_Code` folderze lub `Web.config` modyfikacja pliku, między innymi przyczynami. Więcej szczegółowych informacji na temat cyklu życia aplikacji można znaleźć w temacie [ASP.NET cykl życia aplikacji](https://msdn.microsoft.com/library/ms178473.aspx) .

W przypadku tych samouczków wystarczy dodać kod do `Application_Start` metody, aby usunąć inne. W `Application_Start`, po prostu `StaticCache` Wywołaj `LoadStaticCache()` metodę klasy, która będzie ładować i przechować informacje o dostawcy:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

To wszystko. Przy uruchamianiu `LoadStaticCache()` aplikacji Metoda powiąże informacje o dostawcy z logiki biznesowej i zapisze ją w statycznej zmiennej składowej (lub dowolnego magazynu pamięci podręcznej, który został `StaticCache` zakończony przy użyciu w klasie). Aby sprawdzić to zachowanie, ustaw punkt przerwania w `Application_Start` metodzie i uruchom aplikację. Należy zauważyć, że punkt przerwania został trafiony podczas uruchamiania aplikacji. Kolejne żądania nie powodują `Application_Start` jednak wykonania metody.

[![Użyj punktu przerwania, aby sprawdzić, czy program obsługi zdarzeń Application_Start jest wykonywany](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Rysunek 4**. Użyj punktu przerwania, aby sprawdzić `Application_Start` , czy program obsługi zdarzeń jest wykonywany ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-at-application-startup-cs/_static/image8.png))

> [!NOTE]
> Jeśli `Application_Start` punkt przerwania nie zostanie osiągnięty podczas pierwszego uruchomienia debugowania, jest to spowodowane tym, że aplikacja została już uruchomiona. Wymuś ponowne uruchomienie aplikacji, modyfikując `Global.asax` pliki lub `Web.config` i spróbuj ponownie. Możesz po prostu dodać (lub usunąć) pusty wiersz na końcu jednego z tych plików, aby szybko ponownie uruchomić aplikację.

## <a name="step-5-displaying-the-cached-data"></a>Krok 5. Wyświetlanie danych w pamięci podręcznej

W tym momencie `StaticCache` Klasa ma wersję danych dostawcy buforowaną podczas uruchamiania aplikacji, do której można uzyskać dostęp za pomocą jej `GetSuppliers()` metody. Aby można było korzystać z tych danych z warstwy prezentacji, możemy użyć elementu ObjectDataSource lub programowo wywołać `StaticCache` `GetSuppliers()` metodę klasy z klasy z kodem związanym ze stroną ASP.NET. Przyjrzyjmy się za pomocą kontrolek ObjectDataSource i GridView, aby wyświetlić informacje o buforowanym dostawcy.

Zacznij od otwarcia `AtApplicationStartup.aspx` strony `Caching` w folderze. Przeciągnij widok GridView z przybornika do projektanta, ustawiając jego `ID` właściwość na. `Suppliers` Następnie w tagu inteligentnym GridView wybierz opcję utworzenia nowego elementu ObjectDataSource o nazwie `SuppliersCachedDataSource`. Skonfiguruj element ObjectDataSource, aby korzystał `StaticCache` z `GetSuppliers()` metody klasy.

[![Skonfiguruj element ObjectDataSource, aby używał klasy StaticCache](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Rysunek 5**. Skonfiguruj element ObjectDataSource, aby używał `StaticCache` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-at-application-startup-cs/_static/image11.png))

[![Użyj metody getdostawcs (), aby pobrać dane z pamięci podręcznej](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Rysunek 6**. Użyj metody, aby pobrać dane z pamięci podręcznej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze)](caching-data-at-application-startup-cs/_static/image14.png) `GetSuppliers()`

Po zakończeniu działania kreatora program Visual Studio automatycznie doda BoundFields dla każdego z pól danych w `SuppliersDataTable`. Znaczniki deklaratywne GridView i ObjectDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

Rysunek 7 przedstawia stronę wyświetlaną w przeglądarce. Dane wyjściowe są takie same, gdyby zostały pobrane z `SuppliersBLL` klasy logiki biznesowej, ale `StaticCache` użycie klasy zwraca dane dostawcy jako buforowane podczas uruchamiania aplikacji. Aby sprawdzić to zachowanie, można ustawić `StaticCache` punkty przerwania w `GetSuppliers()` metodzie klasy.

[![Dane dostawcy w pamięci podręcznej są wyświetlane w widoku GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Rysunek 7**. Dane dostawcy w pamięci podręcznej są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-at-application-startup-cs/_static/image17.png))

## <a name="summary"></a>Podsumowanie

Większość każdego modelu danych zawiera godziwą ilość danych statycznych, zazwyczaj zaimplementowaną w postaci tabel odnośników. Ponieważ te informacje są statyczne, nie ma potrzeby ciągłego uzyskiwania dostępu do bazy danych przy każdym wyświetleniu tych informacji. Ponadto ze względu na jego charakter statyczny, gdy buforowanie danych nie jest konieczne. W tym samouczku przedstawiono sposób wykonywania takich danych i buforowania ich w pamięci podręcznej danych, stanie aplikacji oraz statycznej zmiennej składowej. Te informacje są buforowane podczas uruchamiania aplikacji i pozostają w pamięci podręcznej przez cały okres istnienia aplikacji.

W tym samouczku i ostatnich dwóch zawarto już zapoznaniu się z buforowaniem danych na potrzeby okresu istnienia aplikacji, a także za korzystanie z wygasania opartego na czasie. W przypadku buforowania danych bazy danych czas wygaśnięcia oparty na czasie może być mniejszy niż idealny. Zamiast okresowego opróżniania pamięci podręcznej, najlepszym rozwiązaniem jest wykluczenie tylko buforowanego elementu, gdy dane źródłowej bazy danych są modyfikowane. Jest to idealne rozwiązanie przy użyciu zależności pamięci podręcznej SQL, które będziemy analizować w następnym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można się z [ mitchell@4GuysFromRolla.comnią skontaktować.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć [http://ScottOnWriting.NET](http://ScottOnWriting.NET)pod adresem.

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Teresa Murphy i Zack Kowalski. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzedni](caching-data-in-the-architecture-cs.md)Następny
> [](using-sql-cache-dependencies-cs.md)
