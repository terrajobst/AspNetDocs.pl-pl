---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Blob Storage bez struktury (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 2afd4b5cf640eb97080de7e5280409f5e5347731
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583625"
---
# <a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Blob Storage bez struktury (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

W poprzednim rozdziale przedstawiono schematy partycjonowania i wyjaśniono, w jaki sposób Poprawka It zapisuje obrazy w usłudze Azure Storage Blob oraz inne dane zadania w Azure SQL Database. W tym rozdziale szczegółowo przejdziemy do Blob service i pokazano, jak to jest zaimplementowane w kodzie projektu IT.

## <a name="what-is-blob-storage"></a>Co to jest BLOB Storage?

Usługa Azure Storage Blob zapewnia sposób przechowywania plików w chmurze. Blob service ma wiele zalet w porównaniu do przechowywania plików w lokalnym systemie plików sieciowych:

- Jest wysoce skalowalny. Na jednym koncie magazynu można przechowywać [setki terabajtów](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)i można mieć wiele kont magazynu. Niektórzy z największych klientów platformy Azure przechowują setki petabajtów. Program Microsoft SkyDrive używa magazynu obiektów BLOB.
- Jest trwały. Dla każdego pliku przechowywanego w Blob service jest tworzona kopia zapasowa.
- Zapewnia wysoką dostępność. [Umowa SLA dla magazynu](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) niesie obietnice zwiększenia 99,9% lub 99,99% czasu, w zależności od wybranej opcji nadmiarowości geograficznej.
- Jest to funkcja platformy Azure jako usługi (PaaS), która oznacza, że wystarczy przechowywać i pobierać pliki, płacąc wyłącznie za rzeczywistą ilość używanej pamięci masowej, a platforma Azure automatycznie zajmie się konfigurowaniem i zarządzaniem wszystkimi maszynami wirtualnymi i dyskami wymaganymi dla usługi.
- Dostęp do Blob service można uzyskać przy użyciu interfejsu API REST lub interfejsu API języka programowania. Zestawy SDK są dostępne dla platform .NET, Java, Ruby i innych.
- Gdy zapisujesz plik w Blob service, możesz łatwo udostępnić go publicznie za pośrednictwem Internetu.
- Można zabezpieczyć pliki w Blob service tak, aby były dostępne tylko dla autoryzowanych użytkowników. można też udostępnić tokeny dostępu tymczasowego, które udostępniają komuś tylko przez ograniczony czas.

Za każdym razem, gdy tworzysz aplikację dla platformy Azure i chcesz przechowywać wiele danych, które w środowisku lokalnym przechodzą w plikach — takich jak obrazy, filmy wideo, pliki PDF, arkusze kalkulacyjne itp. Blob service.

## <a name="creating-a-storage-account"></a>Tworzenie konta magazynu

Aby rozpocząć pracę z Blob service utworzysz konto magazynu na platformie Azure. W portalu kliknij kolejno pozycje **nowy** -- **Data Services** -- **Magazyn** -- **szybkie tworzenie**, a następnie wprowadź adres URL i lokalizację centrum danych. Lokalizacja centrum danych powinna być taka sama jak aplikacja sieci Web.

![Tworzenie konta magazynu](unstructured-blob-storage/_static/image1.png)

Wybierz region podstawowy, w którym chcesz przechowywać zawartość, a jeśli wybierzesz opcję [replikacji geograficznej](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) , platforma Azure utworzy repliki wszystkich danych w innym centrum danych w innym regionie kraju. Na przykład w przypadku wybrania centrum danych w wersji zachodniej USA, gdy zapisujesz plik w centrum danych zachodnie stany USA, ale w tle platforma Azure skopiuje go również do jednego z innych centrów danych US. Jeśli awaria występuje w jednym regionie kraju, Twoje dane są nadal bezpieczne.

Platforma Azure nie będzie replikować danych w granicach geograficznych: Jeśli lokalizacja główna znajduje się w Stanach Zjednoczonych, pliki są replikowane tylko do innego regionu w Stanach Zjednoczonych; Jeśli Twoja lokalizacja podstawowa to Australia, pliki są replikowane tylko do innego centrum danych w Australii.

Oczywiście można również utworzyć konto magazynu, wykonując polecenia ze skryptu, tak jak wcześniej. Oto polecenie programu Windows PowerShell służące do tworzenia konta magazynu:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Gdy masz konto magazynu, możesz od razu rozpocząć przechowywanie plików w Blob service.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Korzystanie z usługi BLOB Storage w aplikacji poprawki IT

Aplikacja Fix it umożliwia przekazywanie zdjęć.

![Tworzenie rozwiązania do rozwiązywania problemów](unstructured-blob-storage/_static/image2.png)

Po kliknięciu przycisku **Utwórz fixit**aplikacja przekaże określony plik obrazu i zapisze go w BLOB Service.

### <a name="set-up-the-blob-container"></a>Konfigurowanie kontenera obiektów BLOB

Aby można było przechowywać plik w Blob service potrzebny jest *kontener* , w którym będzie przechowywana. Kontener Blob service odnosi się do folderu systemu plików. Skrypty tworzenia środowiska, które zostały zrecenzowane w [rozdziale Automatyzowanie wszystkiego](automate-everything.md) Tworzenie konta magazynu, ale nie tworzą kontenera. Dlatego celem `CreateAndConfigure` metody klasy `PhotoService` jest utworzenie kontenera, jeśli jeszcze nie istnieje. Ta metoda jest wywoływana z metody `Application_Start` w *Global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Nazwa konta magazynu i klucz dostępu są przechowywane w kolekcji `appSettings` pliku *Web. config* , a kod w metodzie `StorageUtils.StorageAccount` używa tych wartości do tworzenia parametrów połączenia i nawiązywania połączenia:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Metoda `CreateAndConfigureAsync` następnie tworzy obiekt, który reprezentuje Blob service, oraz obiekt, który reprezentuje kontener (folder) o nazwie "obrazy" w Blob service:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Jeśli kontener o nazwie "images" jeszcze nie istnieje — to ustawienie będzie prawdziwe przy pierwszym uruchomieniu aplikacji na nowym koncie magazynu — kod tworzy kontener i ustawia uprawnienia do jego udostępniania. (Domyślnie nowe kontenery obiektów BLOB są prywatne i są dostępne tylko dla użytkowników, którzy mają uprawnienia dostępu do konta magazynu).

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Przechowywanie przekazanego zdjęcia w usłudze BLOB Storage

Aby przekazać i zapisać plik obrazu, aplikacja używa interfejsu `IPhotoService` i implementacji interfejsu w klasie `PhotoService`. Plik *PhotoService.cs* zawiera cały kod w aplikacji poprawki IT, który komunikuje się z BLOB Service.

Następująca metoda kontrolera MVC jest wywoływana, gdy użytkownik kliknie pozycję **Utwórz fixit**. W tym kodzie `photoService` odnosi się do wystąpienia klasy `PhotoService`, a `fixittask` odwołuje się do wystąpienia klasy jednostki `FixItTask`, która przechowuje dane dla nowego zadania.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

Metoda `UploadPhotoAsync` w klasie `PhotoService` przechowuje przekazany plik w Blob service i zwraca adres URL wskazujący nowy obiekt BLOB.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Podobnie jak w przypadku metody `CreateAndConfigure` kod nawiązuje połączenie z kontem magazynu i tworzy obiekt, który reprezentuje kontener obiektów BLOB "images", z wyjątkiem tego, że kontener już istnieje.

Następnie tworzy unikatowy identyfikator obrazu, który ma zostać przekazany, łącząc nową wartość identyfikatora GUID z rozszerzeniem pliku:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Następnie kod używa obiektu kontenera obiektów blob i nowego unikatowego identyfikatora do tworzenia obiektu obiektu BLOB, ustawia atrybut dla tego obiektu wskazujący rodzaj pliku, a następnie używa obiektu BLOB do przechowywania pliku w usłudze BLOB Storage.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Na koniec Pobiera adres URL, który odwołuje się do obiektu BLOB. Ten adres URL będzie przechowywany w bazie danych programu i może być używany na stronach poprawki IT w sieci Web do wyświetlania przekazanego obrazu.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Ten adres URL jest przechowywany w bazie danych jako jedna z kolumn tabeli FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Tylko w przypadku adresu URL w bazie danych i obrazów w usłudze BLOB Storage aplikacja do rozwiązywania problemów utrzymuje niewielką, skalowalną i niedrogią bazę danych, podczas gdy są przechowywane obrazy, w których magazyn jest tani i może obsługiwać terabajty lub petabajtów. Jedno konto magazynu może przechowywać setki terabajtów poprawek z zdjęć i płacisz tylko za to, czego używasz. W ten sposób można zacząć wycofać małe miesięczne centy dla pierwszego gigabajta i dodać więcej obrazów dla groszek na dodatkowy gigabajt.

### <a name="display-the-uploaded-file"></a>Wyświetlanie przekazanego pliku

Poprawka aplikacji IT wyświetla przekazany plik obrazu, gdy wyświetla szczegółowe informacje o zadaniu.

![Popraw szczegóły zadania IT ze zdjęciem](unstructured-blob-storage/_static/image3.png)

Aby wyświetlić obraz, cały widok MVC musi zawierać wartość `PhotoUrl` w kodzie HTML wysyłanym do przeglądarki. Serwer sieci Web i baza danych nie używają cykli do wyświetlania obrazu, obsługują tylko kilka bajtów do adresu URL obrazu. W poniższym kodzie Razor `Model` odwołuje się do wystąpienia klasy jednostki `FixItTask`.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Jeśli zobaczysz kod HTML wyświetlanej strony, zobaczysz adres URL wskazujący bezpośrednio na obraz w usłudze BLOB Storage, podobny do tego:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Podsumowanie

Zobaczysz, w jaki sposób Poprawka It zapisuje obrazy w Blob service i tylko adresy URL obrazów w bazie danych SQL. Użycie Blob service powoduje, że baza danych SQL jest znacznie mniejsza niż w przeciwnym razie, umożliwia skalowanie w górę do niemal nieograniczonej liczby zadań i można ją wykonać bez konieczności pisania wielu kodów.

Na koncie magazynu mogą znajdować się setki terabajtów, a koszt magazynu jest znacznie tańszy niż SQL Database magazynu, począwszy od około 3 centów na gigabajt miesięcznie i niewielkiej opłaty za transakcję. Pamiętaj, że nie płacisz za maksymalną pojemność, ale tylko za rzeczywistą ilość danych, dzięki czemu Twoja aplikacja jest gotowa do skalowania, ale nie płacisz za całą dodatkową pojemność.

W [następnym rozdziale](design-to-survive-failures.md) będziemy mówić o ważności aplikacji w chmurze, która może bezpiecznie obsłużyć błędy.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Wprowadzenie do usługi Azure Blob Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog przy użyciu drewna Jan.
- [Jak korzystać z usługi Azure Blob Storage w ramach platformy .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Oficjalna dokumentacja witryny MicrosoftAzure.com. Krótkie wprowadzenie do magazynu obiektów blob, a następnie przykłady kodu przedstawiające sposób nawiązywania połączenia z usługą BLOB Storage, tworzenia kontenerów, przekazywania i pobierania obiektów BLOB itp.
- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria wideo dziewięć części przez Ulrich Homann, Marc Mercuri i marking SIMM. Prezentuje koncepcje wysokiego poziomu i zasady architektury w bardzo dostępnym i interesującym scenariuszu, w tym scenariusze opracowane przez firmę Microsoft Customer Advisory Team (CAT) z rzeczywistymi klientami. Aby zapoznać się z omówieniem usługi Azure Storage i obiektów blob, zobacz temat epizod 5 od 35:13.
- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wzorzec klucza portiera.

> [!div class="step-by-step"]
> [Poprzednie](data-partitioning-strategies.md)
> [dalej](design-to-survive-failures.md)
