---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opcje magazynu danych (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 9357ed5aef39bed501cdac9ac26d46c884d4fae0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617804"
---
# <a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opcje przechowywania danych (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Większość osób służy do relacyjnych baz danych i zapomina inne opcje przechowywania danych podczas projektowania aplikacji w chmurze. Wynikiem może być nieoptymalna wydajność, wysokie koszty lub gorszenie, ponieważ bazy danych [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (nierelacyjne) mogą obsługiwać pewne zadania bardziej wydajniej niż relacyjne bazy danych. Gdy klienci poproszą nas o pomoc w rozwiązywaniu krytycznych problemów z magazynem danych, często jest to spowodowane tym, że mają relacyjną bazę danych, w której jedna z opcji NoSQL będzie działała lepiej. W tych sytuacjach klient byłby lepszy, jeśli zaimplementował rozwiązanie NoSQL przed wdrożeniem aplikacji w środowisku produkcyjnym.

Z drugiej strony może również wystąpić błąd, aby założyć, że baza danych NoSQL może robić wszystko i dostatecznie dobrze. Nie istnieje pojedynczy wybór najlepszego zarządzania danymi dla wszystkich zadań związanych z magazynem danych; różne rozwiązania do zarządzania danymi są zoptymalizowane pod kątem różnych zadań. Najpopularniejsze aplikacje w chmurze mają różne wymagania dotyczące magazynu danych i często są obsługiwane w połączeniu z wieloma rozwiązaniami do magazynowania danych.

Celem tego rozdziału jest udostępnienie szerszego sensu opcji przechowywania danych dostępnych dla aplikacji w chmurze, a także podstawowe wskazówki dotyczące wyboru tych, które pasują do Twojego scenariusza. Najlepiej pamiętać o dostępnych opcjach i pomyśl o ich mocnych i słabych stronach przed rozpoczęciem opracowywania aplikacji. Zmiana opcji magazynu danych w aplikacji produkcyjnej może być niezwykle trudna, na przykład konieczność zmiany aparatu Jet, gdy płaszczyzna jest w locie.

## <a name="data-storage-options-on-azure"></a>Opcje magazynu danych na platformie Azure

Chmura pozwala stosunkowo łatwo korzystać z różnych magazynów danych relacyjnych i NoSQL. Poniżej wymieniono niektóre platformy magazynów danych, których można używać na platformie Azure.

![](data-storage-options/_static/image1.png)

W tabeli przedstawiono cztery typy NoSQL baz danych:

- [Bazy danych klucz/wartość](https://msdn.microsoft.com/library/dn313285.aspx#sec7) przechowują jeden serializowany obiekt dla każdej wartości klucza. Są one przydatne do przechowywania dużych ilości danych, gdzie chcesz uzyskać jeden element dla danej wartości klucza i nie musisz wykonywać zapytania na podstawie innych właściwości elementu.

    [Usługa Azure Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) to baza danych typu klucz/wartość, która działa jak magazyn plików w chmurze, z wartościami klucza odpowiadającymi nazwom folderów i plików. Plik można pobrać według jego folderu i nazwy pliku, a nie przez wyszukiwanie wartości w zawartości pliku.

    [Usługa Azure Table Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) jest również bazą danych typu klucz/wartość. Każda wartość jest nazywana *jednostką* (podobną do wiersza, identyfikowaną przez klucz partycji i klucz wiersza) i zawiera wiele *Właściwości* (podobnie jak kolumny, ale nie wszystkie jednostki w tabeli muszą współdzielić te same kolumny). Wykonywanie zapytań dotyczących kolumn innych niż klucz jest niezwykle niewydajne i należy je unikać. Można na przykład przechowywać dane profilu użytkownika z jedną partycją przechowującą informacje o pojedynczym użytkowniku. Dane, takie jak nazwa użytkownika, skrót hasła, Data urodzenia i tak dalej, można przechowywać w oddzielnych właściwościach jednej jednostki lub w osobnych jednostkach w tej samej partycji. Ale nie chcesz wykonywać zapytania dla wszystkich użytkowników z danym zakresem dat urodzenia i nie możesz wykonać zapytania sprzężenia między tabelą profilu a inną tabelą. Magazyn tabel jest bardziej skalowalny i tańszy niż relacyjna baza danych, ale nie umożliwia wykonywania złożonych zapytań ani sprzężeń.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) są bazami danych kluczy/wartości, w których wartości są *dokumentami*. "Dokument" w tym miejscu nie jest używany w sensie dokumentu Word lub Excel, ale oznacza kolekcję nazwanych pól i wartości, z których każdy może być dokumentem podrzędnym. Na przykład w tabeli historii zamówień dokument zamówienia może zawierać numer zamówienia, datę zamówienia i pola klienta; w polu klient mogą znajdować się pola Nazwa i adres. Baza danych koduje dane pól w formacie, takim jak XML, YAML, JSON lub BSON; można też użyć zwykłego tekstu. Jedną z funkcji, która ustawia bazy danych dokumentów poza bazami danych klucz/wartość, jest możliwość wykonywania zapytań dotyczących pól niebędących kluczami i definiowania indeksów pomocniczych w celu zwiększenia wydajności zapytań. Dzięki tej możliwości baza danych dokumentów jest bardziej odpowiednia dla aplikacji, które muszą pobierać dane na podstawie kryteriów bardziej złożonych niż wartość klucza dokumentu. Na przykład w bazie danych dokumentów historii zamówień sprzedaży można badać różne pola, takie jak identyfikator produktu, identyfikator klienta, nazwa klienta itd. [MongoDB](http://www.mongodb.org/) to popularna baza danych dokumentów.
- [Baza danych rodzin kolumn](https://msdn.microsoft.com/library/dn313285.aspx#sec9) to magazyny danych typu klucz/wartość, które umożliwiają strukturę przechowywania danych w kolekcjach powiązanych kolumn o nazwie rodziny kolumn. Na przykład baza danych spisu może mieć jedną grupę kolumn dla nazwiska osoby (pierwszej, Środkowej, ostatniej), jednej grupy dla adresu osoby oraz jedną grupę informacji o profilu osoby (DOB, płeć itp.). Baza danych może następnie przechowywać każdą rodzinę kolumn w oddzielnej partycji, zachowując jednocześnie wszystkie dane dla jednej osoby związanej z tym samym kluczem. Następnie można odczytać wszystkie informacje o profilu bez konieczności odczytywania wszystkich informacji o nazwie i adresie. [Cassandra](http://cassandra.apache.org/) to popularna baza danych rodziny kolumn.
- [Bazy danych programu Graph](https://msdn.microsoft.com/library/dn313285.aspx#sec10) przechowują informacje jako kolekcje obiektów i relacji. Celem bazy danych programu Graph jest umożliwienie aplikacji wydajne wykonywanie zapytań, które przechodzą przez sieć obiektów i relacje między nimi. Na przykład obiekty mogą być pracownikami w bazie danych kadr i mogą chcieć ułatwić wykonywanie zapytań, takich jak "Znajdź wszystkich pracowników, którzy bezpośrednio lub pośrednio pracują nad Scott". [Neo4j](http://www.neo4j.org/) to popularna baza danych grafu.

W porównaniu z relacyjnymi bazami danych opcje NoSQL oferują znacznie większą skalowalność i opłacalność przechowywania i analizowania danych bez struktury. Kompromis polega na tym, że nie zapewniają bogatej możliwości tworzenia zapytań i niezawodnej integralności danych w relacyjnych bazach danych. NoSQL będzie dobrze działała w przypadku danych dziennika usług IIS, które obejmują duże ilości bez potrzeby przesyłania zapytań. NoSQL nie będzie działała prawidłowo w przypadku transakcji bankowych, co wymaga bezwzględnej spójności danych i obejmuje wiele relacji z innymi danymi związanymi z kontem.

Istnieje również nowsza Kategoria platformy bazy danych o nazwie [NewSQL](http://en.wikipedia.org/wiki/NewSQL) , która łączy skalowalność bazy danych NoSQL z funkcją kwerendy i spójną integralność relacyjnej bazy danych. Bazy danych NewSQL są przeznaczone do obsługi rozproszonego magazynu i przetwarzania zapytań, co jest często trudne do zaimplementowania w bazach danych "OldSQL". [NuoDB](http://www.nuodb.com/) jest przykładem bazy danych NewSQL, która może być używana na platformie Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop i MapReduce

Duże ilości danych, które można przechowywać w bazach danych NoSQL, mogą być trudne do przeanalizowania w odpowiednim czasie. W tym celu można użyć struktury, takiej jak [Hadoop](http://hadoop.apache.org/) , która implementuje funkcje [MapReduce](http://en.wikipedia.org/wiki/MapReduce) . Zasadniczo MapReduce proces jest następujący:

- Ogranicz rozmiar danych, które muszą zostać przetworzone, wybierając z magazynu danych tylko te dane, które mają być analizowane. Załóżmy na przykład, że chcesz znać korzeń bazy użytkowników według roku urodzenia, więc wybierasz tylko lata urodzenia z magazynu danych profilu użytkownika.
- Podziel dane na fragmenty i wysyłaj je do różnych komputerów w celu przetworzenia. Komputer A oblicza liczbę osób z 1950-1959mi datami, komputer B robi 1960-1969 itd. Ta grupa komputerów jest nazywana *klastrem usługi Hadoop*.
- Umieść wyniki każdej części razem po zakończeniu przetwarzania na częściach. Teraz masz stosunkowo krótką listę liczby osób w każdym roku urodzenia i zadania obliczania wartości procentowych na tej ogólnej liście.

Na platformie Azure usługa [HDInsight](https://azure.microsoft.com/services/hdinsight/) umożliwia przetwarzanie, analizowanie i uzyskiwanie nowych szczegółowych informacji z danych Big Data przy użyciu możliwości usługi Hadoop. Na przykład można go użyć do analizowania dzienników serwera sieci Web:

- Włącz rejestrowanie serwera sieci Web na koncie magazynu. Spowoduje to skonfigurowanie platformy Azure do zapisywania dzienników w usłudze BLOB dla każdego żądania HTTP w aplikacji. Usługa BLOB jest zasadniczo magazynem plików w chmurze i integruje dobrze z usługą HDInsight.

    ![Dzienniki do Blob Storage](data-storage-options/_static/image2.png)
- Gdy aplikacja uzyskuje ruch, dzienniki usług IIS serwera sieci Web są zapisywane w magazynie obiektów BLOB.

    ![Dzienniki serwera sieci Web](data-storage-options/_static/image3.png)
- W portalu kliknij kolejno pozycje **nowy** - **Data Services** - **HDInsight** - **szybkie tworzenie**i określ nazwę klastra usługi HDInsight, rozmiar klastra (liczba węzłów danych klastra usługi HDInsight) oraz nazwę użytkownika i hasło klastra usługi HDInsight.

    ![HDInsight](data-storage-options/_static/image4.png)

Teraz można skonfigurować zadania MapReduce do analizowania dzienników i uzyskiwać odpowiedzi na pytania, takie jak:

- Jakie pory dnia moja aplikacja uzyskuje największy ruch?
- Z jakich krajów pochodzi ruch?
- Jaki jest średni dochód dla klubu obszarów, z których pochodzi ruch. (Istnieje Publiczny zestaw danych, który zapewnia dochód z otoczenia według adresu IP i można dopasować go do adresu IP w dziennikach serwera sieci Web).
- Jak dochody z klubu są skorelowane z określonymi stronami lub produktami w witrynie?

Możesz następnie użyć odpowiedzi na pytania, takie jak te, dla reklam docelowych na podstawie prawdopodobieństwa, że klient interesuje lub może kupić określony produkt.

Jak wyjaśniono w [rozdziale Automatyzowanie wszystkiego](automate-everything.md), większość funkcji, które można wykonać w portalu, może być zautomatyzowana i obejmuje Konfigurowanie i wykonywanie zadań analizy usługi HDInsight. Typowy skrypt usługi HDInsight może zawierać następujące czynności:

- Zainicjuj obsługę klastra usługi HDInsight i połącz ją z kontem magazynu w celu uzyskania danych wejściowych usługi BLOB Storage.
- Przekaż pliki wykonywalne zadania MapReduce (. jar lub. exe) do klastra usługi HDInsight.
- Prześlij element MapReduce, który przechowuje dane wyjściowe do magazynu obiektów BLOB.
- Poczekaj na zakończenie zadania.
- Usuń klaster usługi HDInsight.
- Dostęp do danych wyjściowych z usługi BLOB Storage.

Uruchamiając skrypt, który wykonuje to wszystko, można zminimalizować ilość czasu aprowizacji klastra usługi HDInsight, co minimalizuje koszty.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platforma jako usługa (PaaS) a infrastruktura jako usługa (IaaS)

Wymienione wcześniej opcje przechowywania danych obejmują rozwiązania typu "platforma jako usługa" (PaaS) i infrastruktura jako usługa (IaaS). PaaS oznacza, że Zarządzamy infrastrukturą sprzętu i oprogramowania i właśnie korzystasz z usługi. SQL Database to PaaS funkcja platformy Azure. Poproś o bazę danych, a w tle platformy Azure konfiguruje i konfiguruje maszyny wirtualne oraz skonfiguruje na nich bazy danych. Nie masz bezpośredniego dostępu do maszyn wirtualnych i nie musisz zarządzać nimi. IaaS oznacza skonfigurowanie i skonfigurowanie maszyn wirtualnych działających w infrastrukturze centrów danych oraz zarządzanie nimi. Udostępniamy galerię wstępnie skonfigurowanych obrazów maszyn wirtualnych dla typowych konfiguracji maszyn wirtualnych. Można na przykład zainstalować wstępnie skonfigurowane obrazy maszyn wirtualnych dla systemu Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle webWebLogicc Server, Oracle Database itp.

PaaS rozwiązania dotyczące danych, które oferuje platforma Azure:

- Azure SQL Database (znana wcześniej jako SQL Azure). Relacyjna baza danych w chmurze oparta na SQL Server.
- Azure Table Storage. Baza danych key/value NoSQL.
- Magazyn obiektów blob platformy Azure. Magazyn plików w chmurze.

W przypadku IaaS można uruchomić dowolne elementy, które można załadować na maszynę wirtualną, na przykład:

- Relacyjne bazy danych, takie jak SQL Server, Oracle, MySQL, SQL Compact, SQLite lub Postgres.
- Magazyny danych klucza/wartości, takie jak Memcached, Redis, Cassandra i Riak.
- Magazyny danych kolumn, takie jak HBase.
- Bazy danych dokumentów, takie jak MongoDB, RavenDB i CouchDB.
- Bazy danych grafów, takie jak Neo4j.

![Opcje magazynu danych na platformie Azure](data-storage-options/_static/image5.png)

Opcja IaaS zapewnia niemal nieograniczone opcje przechowywania danych, a wiele z nich jest szczególnie łatwa w użyciu, ponieważ można tworzyć maszyny wirtualne przy użyciu wstępnie skonfigurowanych obrazów. Na przykład w portalu zarządzania przejdź do **Virtual Machines**, kliknij kartę **obrazy** , a następnie kliknij przycisk **Przeglądaj magazyn maszyn wirtualnych**.

![Przeglądanie magazynu maszyn wirtualnych](data-storage-options/_static/image6.png)

Zostanie wyświetlona lista [setek wstępnie skonfigurowanych obrazów maszyn wirtualnych](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)i można utworzyć maszynę wirtualną na podstawie obrazu z preinstalowanym systemem zarządzania bazami danych, takiego jak MongoDB, Neo4J, Redis, Cassandra lub CouchDB:

![MongoDB w magazynie maszyny wirtualnej](data-storage-options/_static/image7.png)

Platforma Azure sprawia, że opcje magazynu danych IaaS są łatwe do użycia, ale oferty PaaS oferują wiele korzyści, które sprawiają, że są one bardziej ekonomiczne i praktyczne w wielu scenariuszach:

- Nie musisz tworzyć maszyn wirtualnych, wystarczy użyć portalu lub skryptu w celu skonfigurowania magazynu danych. Jeśli potrzebujesz magazynu danych 200 terabajtów, możesz po prostu kliknąć przycisk lub uruchomić polecenie, a w ciągu kilku sekund, aby móc korzystać z programu.
- Nie jest konieczne zarządzanie maszynami wirtualnymi używanymi przez usługę ani ich stosowanie. Firma Microsoft robi to automatycznie. nie trzeba martwić się o Konfigurowanie infrastruktury pod kątem skalowania lub wysokiej dostępności; Firma Microsoft obsługuje wszystko.
- Nie musisz kupować licencji. opłaty za licencje są uwzględniane w opłatach za usługę.
- Płaci się wyłącznie za użyte zasoby.

Opcje magazynu danych PaaS na platformie Azure obejmują oferty oferowane przez innych dostawców. Na przykład możesz wybrać [dodatek oprogramowanie MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) ze sklepu systemu Azure, aby zainicjować obsługę administracyjną bazy danych MongoDB jako usługi.

## <a name="choosing-a-data-storage-option"></a>Wybieranie opcji magazynu danych

Żadne podejście nie jest właściwe dla wszystkich scenariuszy. Jeśli każda z nich mówi, że ta technologia jest odpowiedzią, najpierw należy zadać pytanie "Jakie są pytania?", ponieważ różne rozwiązania są zoptymalizowane pod kątem różnych elementów. Istnieją pewne korzyści związane z modelem relacyjnym; to dlatego, że jest to bardzo czasochłonne. Ale w programie SQL Server znajdują się również strony z rozwiązaniem NoSQL.

Często wiemy, że najlepiej jest zastosować podejście do kompozycji, w którym można używać programów SQL i NoSQL w jednym rozwiązaniu. Nawet w przypadku, gdy ludzie mówią, że korzystają z NoSQL, w przypadku przechodzenia do szczegółów, co robią często, że korzystają z kilku różnych platform NoSQL: używali [CouchDB](http://wiki.apache.org/couchdb/Introduction), a [Redis](http://redis.io/)i [Riak](http://basho.com/riak/) dla różnych rzeczy. Nawet w usłudze Facebook, która używa NoSQL w szerokim stopniu, używa różnych platform NoSQL dla różnych części usługi. Elastyczność w zakresie mieszania i dopasowywania magazynu danych jest jednym z najważniejszych rzeczy o chmurze, ponieważ jest to łatwe w użyciu wiele rozwiązań danych i integruje je w pojedynczej aplikacji.

Poniżej przedstawiono kilka pytań, które należy wziąć pod uwagę podczas wybierania podejścia:

| Semantyka danych | — Co to jest podstawowa pamięć masowa i dostęp do danych (czy są przechowywane dane relacyjne lub bez struktury)? Dane bez struktury, takie jak pliki multimedialne, najlepiej pasują do magazynu obiektów BLOB. Kolekcja powiązanych danych, takich jak produkty, spisy, dostawcy, zamówienia klientów itp., najlepiej należeć do relacyjnej bazy danych. |
| --- | --- |
| Obsługa zapytań | — Jak łatwo jest wysyłać zapytania o dane? — Jakie typy pytań można wydajnie zażądać? Magazyny danych klucza/wartości są bardzo dobre przy pobieraniu jednego wiersza, który ma wartość kluczową, ale nie jest to dobre dla złożonych zapytań. W przypadku magazynu danych profilu użytkownika, w którym zawsze są uzyskiwane dane dla jednego określonego użytkownika, magazyn danych typu klucz/wartość może być wydajny. w przypadku katalogu produktów, w którym chcesz uzyskać różne grupowania na podstawie różnych atrybutów produktu, relacyjna baza danych może być lepsza. Bazy danych NoSQL umożliwiają wydajne przechowywanie dużych ilości danych, ale trzeba umieścić strukturę bazy danych, w której aplikacja będzie wysyłać zapytania o dane, a tym samym utrudniają wykonywanie zapytań ad hoc. W przypadku relacyjnej bazy danych można utworzyć niemal dowolny rodzaj zapytania. |
| Projekcja funkcjonalna | — Czy można zadać pytania, agregacje itp. czy po stronie serwera? W przypadku uruchomienia opcji wybierz liczbę (\*) z tabeli w programie SQL będzie ona bardzo wydajna, co sprawi, że wszystko działa na serwerze i zwróci szukaną liczbę. Jeśli chcę użyć tego samego obliczenia z magazynu danych NoSQL, który nie obsługuje agregacji, jest to niewydajne "niepowiązane zapytanie" i prawdopodobnie zostanie przekroczony limit czasu. Nawet jeśli zapytanie zakończyło się pomyślnie, chcę pobrać wszystkie dane z serwera do klienta i policzyć wiersze na kliencie. -Jakich języków lub typów wyrażeń można używać? W przypadku relacyjnej bazy danych można używać języka SQL. W przypadku niektórych baz danych NoSQL, takich jak Azure Table Storage, będę korzystać z protokołu [OData](http://www.odata.org/)i wszystko, co można zrobić, jest filtrowanie według klucza podstawowego i pobieranie projekcji (wybierz podzestaw dostępnych pól). |
| Łatwość skalowalności | -Jak często i ile będzie trzeba skalować dane? -Czy platforma natywnie implementuje skalowanie w poziomie? — Jak łatwo można dodać/usunąć pojemność (rozmiar i przepływność)? Relacyjne bazy danych i tabele nie są automatycznie partycjonowane w celu ich skalowalności, dzięki czemu trudno jest skalować poza pewnymi ograniczeniami. NoSQL magazyny danych, takie jak Azure Table Storage, są z nieodłączną partycją i nie ma już ograniczenia dodawania partycji. Możesz łatwo skalować Table Storage do 200 terabajtów, ale maksymalny rozmiar bazy danych dla Azure SQL Database wynosi 500 gigabajtów. Można skalować dane relacyjne, partycjonowanie ich w wielu bazach danych, ale skonfigurowanie aplikacji do obsługi tego modelu obejmuje dużo pracy programistycznej. |
| Instrumentacja i łatwość zarządzania | — Jak łatwa jest platforma do instrumentacji, monitorowania i zarządzania? Konieczne będzie uzyskanie informacji o kondycji i wydajności magazynu danych, dzięki czemu należy wiedzieć, jakie metryki są dostępne dla platformy, i co należy opracować. |
| Operacje | -Jak łatwo jest platforma do wdrożenia i uruchamiania na platformie Azure? PaaS? IaaS? Linux? Table Storage i SQL Database są łatwe do skonfigurowania na platformie Azure. Platformy, które nie są wbudowane rozwiązania Azure PaaS, wymagają większej nakładu pracy. |
| Obsługa interfejsu API | -Czy dostępny jest interfejs API, który ułatwia współdziałanie z platformą? W przypadku usługi Azure Table Service istnieje zestaw SDK z interfejsem API platformy .NET, który obsługuje asynchroniczny model programowania .NET 4,5. Jeśli piszesz aplikację .NET, znacznie łatwiej jest pisać i testować kod dla usługi Azure Table Service w porównaniu do innej platformy magazynu danych typu klucz/wartość, która nie ma interfejsu API lub mniej wszechstronnego. |
| Integralność transakcji i spójność danych | -Czy jest to ważne, aby platforma obsługiwała transakcje w celu zagwarantowania spójności danych? Aby można było śledzić wysyłanie zbiorczych wiadomości e-mail, koszty dotyczące wydajności i niskiej ilości danych mogą być ważniejsze niż automatyczne wsparcie dla transakcji lub integralności referencyjnej na platformie danych, dzięki czemu usługa tabel platformy Azure jest dobrym wyborem. W celu śledzenia sald kont bankowych lub zamówień zakupu platforma relacyjnej bazy danych, która zapewnia duże gwarancje transakcyjne, będzie lepszym wyborem. |
| Ciągłość działalności biznesowej | -Jak łatwe jest tworzenie kopii zapasowej, przywracanie i odzyskiwanie po awarii? Szybsze lub późniejsze dane produkcyjne zostaną uszkodzone i będzie potrzebna funkcja cofania. Relacyjne bazy danych często zawierają bardziej szczegółowe możliwości przywracania, takie jak możliwość przywracania do punktu w czasie. Zrozumienie, jakie funkcje przywracania są dostępne dla każdej z rozważanych platform, jest ważnym czynnikiem, które należy wziąć pod uwagę. |
| Koszty | -Jeśli więcej niż jedna platforma może obsłużyć obciążenie danymi, jak są one porównywane w koszty? Jeśli na przykład używasz ASP.NET Identity, możesz przechowywać dane profilu użytkownika w usłudze Azure Table Service lub Azure SQL Database. Jeśli nie potrzebujesz rozbudowanych obiektów zapytań dla SQL Database, możesz wybrać pozycję tabele platformy Azure w części, ponieważ koszt tego magazynu jest znacznie mniejszy. |

Ogólnie zalecamy zapoznanie się z odpowiedzią na pytania w każdej z tych kategorii przed wybraniem rozwiązań do magazynowania danych.

Ponadto obciążenie może mieć określone wymagania, które niektóre platformy mogą obsługiwać lepiej niż inne. Na przykład:

- Czy Twoja aplikacja wymaga możliwości inspekcji?
- Jakie są wymagania dotyczące eksploatacjii danych — czy są wymagane automatyczne archiwizowanie czy możliwości przeczyszczania?
- Czy masz specjalne potrzeby w zakresie zabezpieczeń? Na przykład dane zawierają informacje OSOBowe (dane osobowe), ale trzeba mieć pewność, że użytkownik jest wykluczony z wyników zapytania.
- Jeśli masz pewne dane, które nie mogą być przechowywane w chmurze w celu zapewnienia prawnego lub technologicznego, może być potrzebna platforma magazynu danych w chmurze, która ułatwia integrację z lokalnym magazynem.

## <a name="demo--using-sql-database-in-azure"></a>Demonstracja — używanie SQL Database na platformie Azure

Poprawka aplikacji IT używa relacyjnej bazy danych do przechowywania zadań. Skrypt tworzenia środowiska Windows PowerShell widoczny w [rozdziale Automatyzuj wszystko](automate-everything.md) tworzy dwa wystąpienia SQL Database. Można je wyświetlić w portalu, klikając kartę **bazy danych SQL** .

![Bazy danych SQL w portalu](data-storage-options/_static/image8.png)

Można również łatwo tworzyć bazy danych za pomocą portalu.

Kliknij pozycję **Nowy--Data Services** -- **SQL Database** -- **szybkie tworzenie**, wprowadź nazwę bazy danych, wybierz serwer, który już znajduje się na koncie, lub Utwórz nowy, a następnie kliknij przycisk **Utwórz SQL Database**.

![Nowa baza danych SQL Database](data-storage-options/_static/image9.png)

Poczekaj kilka sekund, a na platformie Azure jest gotowa baza danych do użycia.

![Utworzono nowe SQL Database](data-storage-options/_static/image10.png)

Dzięki temu platforma Azure wykonuje kilka sekund, co może zająć dzień lub tydzień lub dłużej w środowisku lokalnym. I ponieważ można równie łatwo tworzyć bazy danych w skrypcie lub przy użyciu interfejsu API zarządzania, można dynamicznie skalować w poziomie, rozkładając dane między wiele baz danych, o ile aplikacja została zaprogramowana dla tego programu.

Jest to przykład naszego modelu platformy jako usługi. Nie ma potrzeby zarządzania serwerami. Nie musisz martwić się o kopie zapasowe. Jest ona uruchamiana w wysokiej dostępności — dane w bazie danych są replikowane na trzech serwerach automatycznie. Jeśli maszyna jest już przełączona w tryb failover i utracisz żadne dane. Serwer jest regularnie poprawiany, nie trzeba martwić się o to.

Kliknij przycisk i uzyskaj dokładne parametry połączenia, które są potrzebne, i natychmiast Rozpocznij korzystanie z nowej bazy danych.

![Parametry połączeń](data-storage-options/_static/image11.png)

Pulpit nawigacyjny pokazuje historię połączenia i ilość używanej pamięci masowej.

![Pulpit nawigacyjny SQL Database](data-storage-options/_static/image12.png)

Można zarządzać bazami danych w portalu lub za pomocą narzędzi SQL Server, które już znasz, w tym SQL Server Management Studio (SSMS) i Visual Studio Tools Eksplorator obiektów SQL Server (SSOX) i Eksplorator serwera.

![SSOX](data-storage-options/_static/image13.png)

Kolejnym krokiem jest model cen. Możesz rozpocząć programowanie z bezpłatną bazą danych o rozmiarze 20 MB, a produkcyjna baza danych rozpoczyna się o około $5 miesięcznie. Płacisz tylko za ilość danych przechowywanych w bazie danych, a nie maksymalną pojemność. Nie musisz kupować licencji.

SQL Database jest łatwe do skalowania. W przypadku aplikacji do rozwiązywania IT utworzona w naszym skrypcie automatyzacji baza danych jest ograniczona do 1 rdzeniowym procesorem. Jeśli chcesz skalować ją do 150 rdzeniowym procesorem, możesz po prostu przejść do portalu i zmienić to ustawienie albo wykonać polecenie interfejsu API REST, a w sekundach masz bazę danych 150 rdzeniowym procesorem, w której można wdrożyć dane.

![SQL Database wersje i rozmiary](data-storage-options/_static/image14.png)

Jest to moc chmury umożliwiająca szybkie i łatwe rozpoczęcie korzystania z infrastruktury i natychmiastowego jej rozpoczęcia.

Poprawka aplikacji IT używa dwóch baz danych SQL, jednej do członkostwa (uwierzytelnianie i autoryzacja) oraz jednego dla danych, a to wszystko, co należy zrobić, aby udostępnić je i skalować. Zaobserwowano wcześniej sposób aprowizacji baz danych za pomocą skryptów programu Windows PowerShell, a teraz wiesz również, jak łatwo to zrobić w portalu.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework a bezpośredni dostęp do bazy danych przy użyciu ADO.NET

Poprawka aplikacji IT uzyskuje dostęp do tych baz danych przy użyciu Entity Framework, zalecane przez firmę Microsoft mapowanie ORM (Object-relacyjny) dla aplikacji .NET. ORM to doskonałe narzędzie, które ułatwia produktywność deweloperów, ale produktywność zapewnia spadek wydajności w niektórych scenariuszach. W rzeczywistej aplikacji w chmurze nie będziesz mieć możliwości wyboru między użyciem EF lub ADO.NET bezpośrednio — będziesz używać obu tych metod. W większości przypadków podczas pisania kodu, który współpracuje z bazą danych, uzyskanie maksymalnej wydajności nie jest krytyczne i można skorzystać z uproszczonego kodowania i testowania, które uzyskasz przy użyciu Entity Framework. W sytuacjach, gdy obciążenie EF spowoduje nieakceptowalną wydajność, można napisać i wykonać własne zapytania przy użyciu ADO.NET, najlepiej wywołując procedury składowane.

Niezależnie od metody używanej do uzyskiwania dostępu do bazy danych należy zminimalizować "chattiness", jak to możliwe. Innymi słowy, jeśli możesz uzyskać wszystkie potrzebne dane w jednym większym zestawie wyników zapytania niż dziesiątki lub setki mniejszych, które zwykle są preferowane. Jeśli na przykład chcesz wyświetlić listę studentów i kursów, które są zarejestrowane w usłudze, zazwyczaj lepiej jest uzyskać wszystkie dane w jednym zapytaniu Join zamiast uzyskać uczniów w jednej kwerendzie i wykonując oddzielne zapytania dla kursów poszczególnych uczniów.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Bazy danych SQL i Entity Framework w aplikacji poprawki IT

W aplikacji Poprawka It Klasa `FixItContext`, która pochodzi z klasy Entity Framework `DbContext`, identyfikuje bazę danych i określa tabele w bazie danych. Kontekst określa zestaw jednostek (tabelę) dla zadań, a kod przekazuje do kontekstu nazwę ciągu połączenia. Ta nazwa odwołuje się do parametrów połączenia, które są zdefiniowane w pliku Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Parametry połączenia w pliku *Web. config* mają nazwę AppDB (w tym miejscu wskazują na lokalną bazę danych):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework tworzy tabelę *FixItTasks* na podstawie właściwości zawartych w klasie `FixItTask` Entity. Jest to prosta Klasa POCO (zwykłych obiektów CLR), co oznacza, że nie dziedziczy ani nie ma żadnych zależności w Entity Framework. Ale Entity Framework wie, jak utworzyć tabelę na podstawie jej i wykonać operacje CRUD (Create-Read-Update-Delete) z tym elementem.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabela FixItTasks](data-storage-options/_static/image15.png)

Aplikacja poprawki IT obejmuje interfejs repozytorium, którego używa do operacji CRUD pracujących z magazynem danych.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Należy zauważyć, że metody repozytorium są wszystkie asynchroniczne, dlatego cały dostęp do danych można wykonać w pełni asynchroniczny sposób.

Implementacja repozytorium wywołuje Entity Framework metod asynchronicznych do pracy z danymi, w tym zapytania LINQ, a także dla operacji INSERT, Update i DELETE. Oto przykładowy kod umożliwiający wyszukanie zadania poprawki.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Zauważysz, że w tym miejscu jest również dostępny kod rejestrowania i błędów, który zostanie wyświetlony w dalszej części [rozdziału Monitorowanie i telemetrię](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Wybieranie SQL Database (PaaS) a SQL Server w maszynie wirtualnej (IaaS) na platformie Azure

Bardzo ważne jest, aby uzyskać informacje na temat SQL Server i Azure SQL Database, że podstawowy model programowania dla obu nich jest identyczny. Możesz użyć większości tych samych umiejętności w obu środowiskach. Można nawet użyć bazy danych SQL Server w trakcie programowania i wystąpienia SQL Database w chmurze, co jest sposobem skonfigurowania aplikacji do rozwiązywania problemów.

Alternatywnie można uruchomić te same SQL Server w chmurze, które są uruchamiane lokalnie, instalując je na maszynach wirtualnych IaaS. W przypadku niektórych starszych aplikacji uruchomienie SQL Server na maszynie wirtualnej może być lepszym rozwiązaniem. Ponieważ SQL Server baza danych działa na dedykowanej maszynie wirtualnej, ma więcej dostępnych zasobów niż SQL Database baza danych działająca na serwerze udostępnionym. Oznacza to, że baza danych SQL Server może być większa i nadal wydajna. Ogólnie rzecz biorąc, mniejszy rozmiar bazy danych i rozmiar tabeli, tym lepszy jest przypadek użycia dla SQL Database (PaaS).

Poniżej przedstawiono niektóre wskazówki dotyczące wyboru między tymi dwoma modelami.

| Usługa Azure SQL Database (PaaS) | SQL Server w maszynie wirtualnej (IaaS) |
| --- | --- |
| **Informatycy** — nie musisz tworzyć maszyn wirtualnych, aktualizować ani poprawiać systemu operacyjnego lub SQL ani zarządzać nimi. Platforma Azure robi to za Ciebie. — Wbudowana wysoka dostępność z umową SLA na poziomie bazy danych. — Niski łączny koszt posiadania (TCO), ponieważ płacisz tylko za to, z czego korzystasz (bez licencji). -Dobre do obsługi dużej liczby mniejszych baz danych (&lt;= 500 GB każdego). — Łatwe do dynamicznego tworzenia nowych baz danych, aby umożliwić skalowanie w poziomie. | ***Specjaliści*** -funkcja zgodna z SQL serverami lokalnymi. — Umożliwia wdrożenie SQL Server [wysokiej dostępności za pomocą funkcji AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) w dwóch maszynach wirtualnych z UMOWą SLA na poziomie maszyny wirtualnej. — Masz pełną kontrolę nad sposobem zarządzania programem SQL. — Można ponowne użyć już posiadanych licencji SQL lub zanieść je o godzinę. -Dobre do obsługi mniej, ale większych baz danych (1 TB +). |
| **Wady** — część funkcji w porównaniu do SQL Server lokalnych (brak [integracji środowiska CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [obsługa kompresji](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)itp.) — limit rozmiaru bazy danych równy 500 GB. | ***Wady*** — aktualizacje/poprawki (systemy operacyjne i SQL) są odpowiedzialne za tworzenie i zarządzanie baz danychem (operacje wejścia/wyjścia na sekundę) są ograniczone do około 8000 (za pośrednictwem 16 dysków danych). |

Jeśli chcesz używać SQL Server na maszynie wirtualnej, możesz użyć własnej licencji SQL Server lub płacisz za godzinę. Na przykład w portalu lub za pośrednictwem interfejsu API REST można utworzyć nową maszynę wirtualną przy użyciu obrazu SQL Server.

![Tworzenie maszyny wirtualnej przy użyciu SQL Server](data-storage-options/_static/image16.png)

![Lista SQL Server obrazów maszyn wirtualnych](data-storage-options/_static/image17.png)

Podczas tworzenia maszyny wirtualnej przy użyciu obrazu SQL Serveru opłata za SQL Server jest naliczana zgodnie z godziną na podstawie użycia maszyny wirtualnej. Jeśli masz projekt, który będzie działać tylko przez kilka miesięcy, tańsze jest płatność za godzinę. Jeśli uważasz, że Twój projekt zostanie ostatnio przez lata, tańsze jest zakupienie licencji w sposób zwykle.

## <a name="summary"></a>Podsumowanie

Chmura obliczeniowa ułatwia mieszanie i dopasowywanie metod magazynu danych w celu najlepszego dopasowania do potrzeb aplikacji. W przypadku kompilowania nowej aplikacji należy dokładnie zapoznać się z pytaniami wymienionymi w tym miejscu w celu wybrania metod, które będą nadal działały, gdy aplikacja zostanie powiększona. W [następnym rozdziale](data-partitioning-strategies.md) wyjaśniono niektóre strategie partycjonowania, których można użyć do łączenia wielu metod przechowywania danych.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji, zapoznaj się z następującymi zasobami.

Wybieranie platformy bazy danych:

- [Dostęp do danych dla rozwiązań o wysokiej skalowalności: Używanie funkcji SQL, NoSQL i trwałości Polyglot](https://aka.ms/dag-doc). Książka elektroniczna według wzorców i praktyk firmy Microsoft, które są zgodne z różnymi rodzajami magazynów danych, które są dostępne dla aplikacji w chmurze.
- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/ff898430.aspx). Zobacz przewodnik spójności danych, informacje o replikacji danych i synchronizacji, wzorzec tabeli indeksów, wzorzec widoku materiałowego.
- [Base: alternatywa kwasowa](http://queue.acm.org/detail.cfm?id=1394128). Artykuł dotyczący kompromisów między spójnością i skalowalnością danych.
- [Siedem baz danych w siedem tygodni: Przewodnik dotyczący nowoczesnych baz danych i przenoszenia NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Książka według Eric Redmond i Jim R. Wilson. Zdecydowanie zaleca się wprowadzenie do szerokiego zakresu dostępnych obecnie platform magazynu danych.

Wybór między SQL Server i SQL Database:

- [Premium Preview dla SQL Database wskazówki](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Wprowadzenie do Baza danych SQL — wersja Premium i wskazówki dotyczące sytuacji, w których należy wybrać ją w SQL Database wersje Web i Business.
- [Wytyczne i ograniczenia (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Strona portalu, która łączy się z dokumentacją dotyczącą ograniczeń SQL Database, w tym tych, które koncentrują się na funkcjach SQL Server, których SQL Database nie obsługuje.
- [SQL Server na platformie Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Strona portalu, która łączy się z dokumentacją dotyczącą uruchamiania SQL Server na platformie Azure.
- [Scott Guthrie objaśnia bazy danych SQL na platformie Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6-minutowy film wideo z wprowadzeniem do SQL Database przez Scott Guthrie.
- [Wzorce aplikacji i strategie programowania dla SQL Server w usłudze Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Używanie Entity Framework i SQL Database w aplikacji sieci Web ASP.NET

- [Wprowadzenie z dr 6 przy użyciu MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Seria z dziewięcioma częściami, która przeprowadzi Cię przez proces tworzenia aplikacji MVC, która korzysta z EF i wdraża bazę danych na platformie Azure i SQL Database.
- [ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Wieloczęściowa seria samouczków, w której znajduje się więcej informacji na temat sposobu wdrażania bazy danych za pomocą narzędzia EF Code First.
- [Wdróż aplikację Secure ASP.NET MVC 5 z członkostwem, uwierzytelnianiem OAuth i SQL Database w witrynie sieci Web systemu Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Samouczek krok po kroku, który przeprowadzi Cię przez proces tworzenia aplikacji sieci Web, która używa uwierzytelniania, przechowuje tabele aplikacji w bazie danych członkostwa, modyfikuje schemat bazy danych i wdraża aplikację na platformie Azure.
- [Mapa zawartości dostępu do danych ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Linki do zasobów do pracy z EF i SQL Database.

Korzystanie z MongoDB na platformie Azure:

- [Oprogramowanie MongoLab — MongoDB na platformie Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Na stronie portalu znajduje się dokumentacja dotycząca uruchamiania programu MongoDB na platformie Azure.
- [Utwórz witrynę sieci Web systemu Azure, która łączy się z usługą MongoDB działającą na maszynie wirtualnej na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Samouczek krok po kroku pokazujący, jak korzystać z bazy danych MongoDB w aplikacji sieci Web ASP.NET.

HDInsight (Hadoop na platformie Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Dokumentacja usługi HDInsight w witrynie [Azure](https://azure.microsoft.com/) Portal.
- [Hadoop i HDInsight: dane big data na platformie Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artykuł magazynu MSDN Bruno Terkaly i Ricardo Villalobos, wprowadzając usługę Hadoop na platformie Azure.
- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wzorzec MapReduce.

> [!div class="step-by-step"]
> [Poprzednie](single-sign-on.md)
> [dalej](data-partitioning-strategies.md)
