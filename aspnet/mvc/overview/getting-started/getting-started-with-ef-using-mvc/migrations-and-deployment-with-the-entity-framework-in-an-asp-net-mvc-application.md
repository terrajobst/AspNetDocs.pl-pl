---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: korzystanie z migracji EF w aplikacji ASP.NET MVC i wdrażanie na platformie Azure'
author: tdykstra
description: W tym samouczku można włączyć migracje Code First i wdrożyć aplikację w chmurze na platformie Azure.
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 989dd0f0e18b338be057b9c5657586eff996d8ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616082"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>Samouczek: korzystanie z migracji EF w aplikacji ASP.NET MVC i wdrażanie na platformie Azure

Do tej pory aplikacja internetowa firmy Contoso University została uruchomiona lokalnie w IIS Express na komputerze deweloperskim. Aby udostępnić rzeczywistą aplikację innym osobom korzystającym z Internetu, należy wdrożyć ją w dostawcy hostingu w sieci Web. W tym samouczku można włączyć migracje Code First i wdrożyć aplikację w chmurze na platformie Azure:

- Włącz Migracje Code First. Funkcja migracji pozwala zmienić model danych i wdrożyć zmiany w środowisku produkcyjnym, aktualizując schemat bazy danych bez konieczności porzucania i ponownego tworzenia bazy danych.
- Wdróż na platformie Azure. Ten krok jest opcjonalny; Możesz kontynuować pracę z pozostałymi samouczkami bez wdrażania projektu.

Zalecamy używanie procesu ciągłej integracji z kontrolą źródła dla wdrożenia, ale ten samouczek nie obejmuje tych tematów. Aby uzyskać więcej informacji, zobacz rozdziały [kontroli źródła](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) i [ciągłej integracji](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) umożliwiające [Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Włącz migracje Code First
> * Wdróż aplikację na platformie Azure (opcjonalnie)

## <a name="prerequisites"></a>Wymagania wstępne

- [Elastyczność połączeń i przejmowanie poleceń](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>Włącz migracje Code First

Podczas tworzenia nowej aplikacji model danych jest często zmieniany, a przy każdym zmianie modelu jest on synchronizowany z bazą danych. Skonfigurowano Entity Framework, aby automatycznie porzucić i ponownie utworzyć bazę danych przy każdej zmianie modelu danych. Po dodaniu, usunięciu lub zmianie klas jednostek lub zmianie klasy `DbContext`, przy następnym uruchomieniu aplikacji automatycznie usuwa istniejącą bazę danych, tworzy nową, która jest zgodna z modelem, i wydaje ją z danymi testowymi.

Ta metoda zachowania synchronizacji bazy danych z modelem danych działa prawidłowo, dopóki aplikacja nie zostanie wdrożona w środowisku produkcyjnym. Gdy aplikacja jest uruchomiona w środowisku produkcyjnym, zazwyczaj zapisuje dane, które mają być przechowywane, i nie ma potrzeby utraty wszystkiego przy każdym wprowadzeniu zmiany, takiej jak dodanie nowej kolumny. Funkcja [migracje Code First](https://msdn.microsoft.com/data/jj591621) rozwiązuje ten problem, włączając Code First do aktualizowania schematu bazy danych zamiast upuszczania i ponownego tworzenia bazy danych. W tym samouczku zostanie wdrożona aplikacja i zostanie ona przygotowana do włączenia migracji.

1. Wyłącz inicjatora, który został wcześniej skonfigurowany przez dodanie komentarza lub usunięcie elementu `contexts`, który został dodany do pliku Web. config aplikacji.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Również w pliku *Web. config* aplikacji Zmień nazwę bazy danych w parametrach połączenia na ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Ta zmiana konfiguruje projekt tak, aby podczas pierwszej migracji została utworzona nowa baza danych. Nie jest to wymagane, ale zobaczysz później, dlaczego jest to dobry pomysł.
3. W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.

1. W wierszu `PM>` wprowadź następujące polecenia:

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    Polecenie `enable-migrations` tworzy folder *migracji* w projekcie ContosoUniversity i umieszcza w tym folderze plik *Configuration.cs* , który można edytować w celu skonfigurowania migracji.

    (Jeśli pominięto powyższy krok, który powoduje zmianę nazwy bazy danych, migracja spowoduje znalezienie istniejącej bazy danych i automatyczne wykonanie polecenia `add-migration`. To nie oznacza, że po wdrożeniu bazy danych nie będzie wykonywany test kodu migracji. Później po uruchomieniu polecenia `update-database` nic się nie stanie, ponieważ baza danych już istnieje.)

    Otwórz plik *ContosoUniversity\Migrations\Configuration.cs* . Podobnie jak w przypadku klasy inicjatora, która została wcześniej wykorzystana, Klasa `Configuration` obejmuje metodę `Seed`.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Celem metody [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) jest umożliwienie wstawiania lub aktualizowania danych testowych po Code First tworzenia lub aktualizowania bazy danych. Metoda jest wywoływana, gdy baza danych zostanie utworzona, i za każdym razem, gdy schemat bazy danych zostanie zaktualizowany po zmianie modelu danych.

### <a name="set-up-the-seed-method"></a>Konfigurowanie metody inicjatora

Gdy porzucasz i utworzysz ponownie bazę danych dla każdej zmiany modelu danych, użyj metody `Seed` inicjatora, aby wstawić dane testowe, ponieważ po każdej zmianie modelu baza danych zostanie usunięta i wszystkie dane testowe zostaną utracone. Przy Migracje Code First dane testowe są zachowywane po zmianie bazy danych, więc w tym przypadku dane testowe w metodzie [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) zwykle nie są konieczne. W rzeczywistości nie chcesz, aby program `Seed` wstawiał dane testowe, jeśli będziesz używać migracji do wdrożenia bazy danych w środowisku produkcyjnym, ponieważ metoda `Seed` zostanie uruchomiona w środowisku produkcyjnym. W takim przypadku należy użyć metody `Seed`, aby wstawić do bazy danych tylko dane, które są potrzebne w środowisku produkcyjnym. Na przykład baza danych może zawierać rzeczywiste nazwy działów w tabeli `Department`, gdy aplikacja będzie dostępna w środowisku produkcyjnym.

Na potrzeby tego samouczka będziesz używać migracji do wdrożenia, ale metoda `Seed` będzie wstawiać dane testowe mimo to, aby ułatwić sprawdzenie, jak działają funkcje aplikacji bez konieczności ręcznego wstawiania dużej ilości danych.

1. Zastąp zawartość pliku *Configuration.cs* następującym kodem, który ładuje dane testowe do nowej bazy danych.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Metoda [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) przyjmuje obiekt kontekstu bazy danych jako parametr wejściowy, a kod w metodzie używa tego obiektu do dodawania nowych jednostek do bazy danych. Dla każdego typu jednostki, kod tworzy kolekcję nowych jednostek, dodaje je do odpowiedniej właściwości [nieogólnymi](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) , a następnie zapisuje zmiany w bazie danych. Nie jest konieczne wywoływanie metody [metody SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) po każdej grupie jednostek, podobnie jak w tym miejscu, ale ułatwia to znalezienie źródła problemu w przypadku wystąpienia wyjątku podczas zapisu w bazie danych.

    Niektóre instrukcje, które wstawiają dane, używają metody [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) w celu wykonania operacji "upsert". Ponieważ `Seed` Metoda jest uruchamiana za każdym razem, gdy wykonujesz polecenie `update-database`, zwykle po każdej migracji, nie możesz tylko wstawiać danych, ponieważ wiersze, które próbujesz dodać, będą już znajdować się po pierwszej migracji, która tworzy bazę danych. Operacja "upsert" zapobiega błędom, które mogą wystąpić w przypadku próby wstawienia wiersza, który już istnieje, ale ***zastępuje*** wszelkie zmiany danych, które mogły zostać wprowadzone podczas testowania aplikacji. Dane testowe w niektórych tabelach mogą nie być potrzebne: w niektórych przypadkach, gdy zmieniasz dane podczas testowania, jeśli chcesz, aby zmiany były nadal wykonywane po aktualizacji bazy danych. W takim przypadku należy wykonać operację warunkowego wstawiania: Wstaw wiersz tylko wtedy, gdy jeszcze nie istnieje. Metoda inicjatora używa obu metod.

    Pierwszy parametr przesłany do metody [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) określa właściwość, która ma zostać użyta do sprawdzenia, czy wiersz już istnieje. W przypadku danych studenta testowego, które udostępniasz, właściwość `LastName` może zostać użyta do tego celu, ponieważ każda Ostatnia nazwa na liście jest unikatowa:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Ten kod zakłada, że ostatnie nazwy są unikatowe. W przypadku ręcznego dodania ucznia z duplikatem nazwiska podczas następnego przeprowadzenia migracji zostanie wyświetlony następujący wyjątek:

    **Sekwencja zawiera więcej niż jeden element**

    Aby uzyskać informacje na temat sposobu obsługi nadmiarowych danych, takich jak dwie uczniów o nazwie "Alexander Carson", zobacz artykuł dotyczący umieszczania [i debugowania Entity Framework (EF) baz danych](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Rick Anderson. Aby uzyskać więcej informacji o metodzie `AddOrUpdate`, zobacz temat zapoznaj [się z artykułem dr 4,3 AddOrUpdate with](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) The Julie Lerman.

    Kod, który tworzy jednostki `Enrollment` założono, że masz `ID` wartość w jednostkach w kolekcji `students`, chociaż nie została ustawiona ta właściwość w kodzie, który tworzy kolekcję.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Możesz użyć tutaj właściwości `ID`, ponieważ wartość `ID` jest ustawiana podczas wywoływania `SaveChanges` dla kolekcji `students`. EF automatycznie pobiera wartość klucza podstawowego podczas wstawiania jednostki do bazy danych i aktualizuje właściwość `ID` jednostki w pamięci.

    Kod, który dodaje każdą jednostkę `Enrollment` do zestawu jednostek `Enrollments`, nie używa metody `AddOrUpdate`. Sprawdza, czy jednostka już istnieje i wstawia jednostkę, jeśli nie istnieje. Takie podejście spowoduje zachowanie zmian wprowadzonych w klasie rejestracji za pomocą interfejsu użytkownika aplikacji. Kod jest przełączany przez każdy element członkowski [listy](https://msdn.microsoft.com/library/6sh2ey19.aspx) `Enrollment`i jeśli rejestracja nie zostanie znaleziona w bazie danych, zostanie dodana Rejestracja do bazy danych programu. Przy pierwszym aktualizowaniu bazy danych baza danych będzie pusta, co spowoduje dodanie każdej rejestracji.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Skompiluj projekt.

### <a name="execute-the-first-migration"></a>Wykonaj pierwszą migrację

Po wykonaniu polecenia `add-migration` migracje wygenerowały kod, który mógłby utworzyć bazę danych od podstaw. Ten kod jest również w folderze *migrations* , w pliku o nazwie *&lt;timestamp&gt;\_InitialCreate.cs*. Metoda `Up` klasy `InitialCreate` tworzy tabele bazy danych, które odpowiadają zestawom jednostek modelu danych, a metoda `Down` usuwa je.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Migracja wywołuje metodę `Up`, aby zaimplementować zmiany modelu danych dla migracji. Po wprowadzeniu polecenia w celu wycofania aktualizacji migracja wywołuje metodę `Down`.

Jest to początkowa migracja, która została utworzona po wprowadzeniu polecenia `add-migration InitialCreate`. Parametr (`InitialCreate` w przykładzie) jest używany jako nazwa pliku i może być dowolnie wybrany. zwykle wybierasz słowo lub frazę, która podsumowuje zawartość wykonywaną podczas migracji. Można na przykład nazwać późniejszej migracji &quot;adddepartment&quot;.

W przypadku utworzenia początkowej migracji, gdy baza danych już istnieje, kod tworzenia bazy danych jest generowany, ale nie musi działać, ponieważ baza danych jest już zgodna z modelem danych. Po wdrożeniu aplikacji w innym środowisku, w którym baza danych jeszcze nie istnieje, ten kod zostanie uruchomiony w celu utworzenia bazy danych, więc dobrym pomysłem jest przetestowanie go w pierwszej kolejności. Dlatego zmieniono nazwę bazy danych w parametrach połączenia wcześniejszych&mdash;tak, aby migracje mogły utworzyć nową od podstaw.

1. W oknie **konsola Menedżera pakietów** wprowadź następujące polecenie:

    `update-database`

    `update-database` polecenie uruchamia metodę `Up`, aby utworzyć bazę danych, a następnie uruchamia metodę `Seed`, aby wypełnić bazę danych. Ten sam proces zostanie uruchomiony automatycznie w środowisku produkcyjnym po wdrożeniu aplikacji, jak widać w poniższej sekcji.
2. Użyj **Eksplorator serwera** , aby sprawdzić bazę danych zgodnie z pierwszym samouczkiem i uruchomić aplikację w celu sprawdzenia, czy wszystkie elementy nadal działają tak samo jak wcześniej.

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

Do tej pory aplikacja została uruchomiona lokalnie w IIS Express na komputerze deweloperskim. Aby udostępnić je innym osobom, które będą mogły korzystać z Internetu, należy wdrożyć je dla dostawcy hostingu w sieci Web. W tej części samouczka zostanie wdrożona na platformie Azure. Ta sekcja jest opcjonalna. Możesz pominąć ten proces i przejść do poniższego samouczka lub dostosować instrukcje w tej sekcji, aby wybrać innego wybranego dostawcę hostingu.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Wdrażanie bazy danych za pomocą migracji Code First

Aby wdrożyć bazę danych, użyj Migracje Code First. Podczas tworzenia profilu publikowania, który służy do konfigurowania ustawień do wdrożenia z programu Visual Studio, należy zaznaczyć pole wyboru z etykietą **baza danych aktualizacji**. To ustawienie powoduje, że proces wdrażania automatycznie skonfiguruje plik *Web. config* aplikacji na serwerze docelowym, tak aby Code First używa klasy inicjatora `MigrateDatabaseToLatestVersion`.

Program Visual Studio nie wykonuje żadnych czynności w odniesieniu do bazy danych podczas procesu wdrażania podczas kopiowania projektu na serwer docelowy. Po uruchomieniu wdrożonej aplikacji i uzyskaniu dostępu do bazy danych po raz pierwszy po wdrożeniu Code First sprawdza, czy baza danych jest zgodna z modelem danych. Jeśli wystąpi niezgodność, Code First automatycznie tworzy bazę danych (jeśli jeszcze nie istnieje) lub aktualizuje schemat bazy danych do najnowszej wersji (Jeśli baza danych istnieje, ale nie jest zgodna z modelem). Jeśli aplikacja implementuje metodę `Seed` migracji, Metoda zostanie uruchomiona po utworzeniu bazy danych lub zaktualizowaniu schematu.

Migracja `Seed` Metoda wstawia dane testowe. W przypadku wdrażania w środowisku produkcyjnym należy zmienić metodę `Seed` tak, aby wstawiali tylko dane, które mają zostać wstawione do produkcyjnej bazy danych. Na przykład w bieżącym modelu danych może być konieczne posiadanie rzeczywistych kursów, ale fikcyjnych uczniów w bazie danych programistycznych. Można napisać metodę `Seed`, aby załadować zarówno w fazie tworzenia, jak i dodać komentarz do fikcyjnych uczniów przed wdrożeniem w środowisku produkcyjnym. Możesz też napisać `Seed` metodę, aby załadować tylko kursy i ręcznie wprowadzić fikcyjne uczniów w bazie danych testowych przy użyciu interfejsu użytkownika aplikacji.

### <a name="get-an-azure-account"></a>Pobierz konto platformy Azure

Musisz mieć konto platformy Azure. Jeśli jeszcze tego nie masz, ale masz subskrypcję programu Visual Studio, możesz [aktywować korzyści z subskrypcji](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
). W przeciwnym razie możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Tworzenie witryny sieci Web i bazy danych SQL na platformie Azure

Aplikacja sieci Web na platformie Azure będzie działać w udostępnionym środowisku hostingu, co oznacza, że działa na maszynach wirtualnych, które są udostępniane innym klientom platformy Azure. Udostępnione środowisko hostingu jest tanim sposobem na rozpoczęcie pracy w chmurze. Później, jeśli ruch internetowy zostanie zwiększony, aplikacja może skalować się, aby zaspokoić potrzeby, uruchamiając na dedykowanych maszynach wirtualnych. Aby dowiedzieć się więcej o opcjach cenowych Azure App Service, Przeczytaj [App Service Cennik](https://azure.microsoft.com/pricing/details/app-service/).

Baza danych zostanie wdrożona w usłudze Azure SQL Database. SQL Database jest usługą relacyjnej bazy danych opartą na chmurze, która jest oparta na SQL Server technologiach. Narzędzia i aplikacje, które działają z SQL Server również współpracują z usługą SQL Database.

1. W [Portal zarządzania platformy Azure](https://portal.azure.com)wybierz pozycję **Utwórz zasób** na lewej karcie, a następnie wybierz pozycję **Zobacz wszystko** w **nowym** okienku (lub *bloku*), aby wyświetlić wszystkie dostępne zasoby. Wybierz pozycję **aplikacja sieci Web i baza danych SQL** w sekcji **Sieć** w bloku **wszystko** . Na koniec wybierz pozycję **Utwórz**.

    ![Tworzenie zasobu w witrynie Azure Portal](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   Zostanie otwarty formularz służący do tworzenia nowej **aplikacji sieci Web i zasobu SQL** .

2. Wprowadź ciąg w polu **Nazwa aplikacji** , który ma być unikatowym adresem URL aplikacji. Pełny adres URL będzie zawierać informacje wprowadzone w tym miejscu oraz domyślną domenę usługi Azure App Services (. azurewebsites.net). Jeśli **Nazwa aplikacji** jest już zajęta, Kreator powiadamia o czerwono, a *Nazwa aplikacji jest niedostępna* . Jeśli **Nazwa aplikacji** jest dostępna, zobaczysz zielony znacznik wyboru.

3. W polu **subskrypcja** wybierz subskrypcję platformy Azure, w której ma się znajdować **App Service** .

4. W polu tekstowym **Grupa zasobów** wybierz grupę zasobów lub Utwórz nową. To ustawienie określa centrum danych, w którym zostanie uruchomiona witryna sieci Web. Aby uzyskać więcej informacji na temat grup zasobów, zobacz [grupy zasobów](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Utwórz nowy **Plan App Service** , klikając *sekcję App Service*, **Utwórz nową**i wypełnij **App Service plan** (może być taka sama jak nazwa App Service), **Lokalizacja**i **warstwa cenowa** (dostępna jest bezpłatna opcja).

6. Kliknij przycisk **SQL Database**, a następnie wybierz pozycję **Utwórz nową bazę danych** lub wybierz istniejącą bazę danych.

7. W polu **Nazwa** wprowadź nazwę bazy danych.
8. Kliknij pole **serwer docelowy** , a następnie wybierz pozycję **Utwórz nowy serwer**. Alternatywnie, jeśli wcześniej utworzono serwer, można wybrać ten serwer z listy dostępnych serwerów.
9. Wybierz sekcję **warstwa cenowa** , wybierz opcję *bezpłatna*. Jeśli potrzebne są dodatkowe zasoby, bazę danych można skalować w dowolnym momencie. Aby dowiedzieć się więcej na temat cennika usługi Azure SQL, zobacz [Azure SQL Database Cennik](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Zmodyfikuj [Sortowanie](/sql/relational-databases/collations/collation-and-unicode-support) zgodnie z wymaganiami.
11. Wprowadź **nazwę użytkownika administratora SQL** administrator i **hasło administratora SQL**.

    - Jeśli wybrano opcję **nowy serwer SQL Database**, zdefiniuj nową nazwę i hasło, które będą używane później podczas uzyskiwania dostępu do bazy danych.
    - W przypadku wybrania wcześniej utworzonego serwera, wprowadź poświadczenia dla tego serwera.

12. Zbieranie danych telemetrycznych można włączyć dla App Service przy użyciu Application Insights. W przypadku niewielkiej konfiguracji Application Insights zbiera cenne dane dotyczące zdarzeń, wyjątków, zależności, żądań i śledzenia. Aby dowiedzieć się więcej na temat Application Insights, zobacz [Azure monitor](https://azure.microsoft.com/services/monitor/).
13. Kliknij przycisk **Utwórz** u dołu, aby wskazać, że wszystko zostało zakończone.

    Portal zarządzania powraca do strony pulpit nawigacyjny, a w obszarze **powiadomień** u góry strony zostanie wyświetlona witryna. Po czasie (zwykle krócej niż minutę) istnieje powiadomienie, że wdrożenie zakończyło się pomyślnie. Na pasku nawigacyjnym po lewej stronie w sekcji **App Services** zostanie wyświetlona nowa App Service, a nowa baza danych SQL zostanie wyświetlona w sekcji **bazy danych SQL** .

### <a name="deploy-the-app-to-azure"></a>Wdrażanie aplikacji na platformie Azure

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj** z menu kontekstowego.

2. Na stronie **Wybierz miejsce docelowe publikowania** wybierz **App Service** a następnie **Wybierz pozycję istniejące**, a następnie wybierz pozycję **Publikuj**.

    ![Wybierz stronę docelową publikowania](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Jeśli Twoja subskrypcja platformy Azure nie została wcześniej dodana w programie Visual Studio, wykonaj czynności opisane na ekranie. Te kroki umożliwiają programowi Visual Studio łączenie się z subskrypcją platformy Azure, dzięki czemu lista **App Services** będzie zawierać witrynę sieci Web.

4. Na stronie **App Service** wybierz **subskrypcję** , do której został dodany App Service. W obszarze **Widok**wybierz pozycję **Grupa zasobów**. Rozwiń grupę zasobów, do której dodano App Service, a następnie wybierz App Service. Wybierz **przycisk OK** , aby opublikować aplikację.

5. W oknie **danych wyjściowych** znajdują się informacje o wykonywaniu akcji wdrażania i raporty o pomyślnym zakończeniu wdrożenia.

6. Po pomyślnym wdrożeniu domyślna przeglądarka automatycznie otwiera adres URL wdrożonej witryny sieci Web.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    Twoja aplikacja jest teraz uruchomiona w chmurze.

W tym momencie baza danych *SchoolContext* została utworzona w bazie danych SQL Azure, ponieważ wybrano opcję **Wykonaj migracje Code First (uruchamiana przy uruchamianiu aplikacji)** . Plik *Web. config* w wdrożonej witrynie sieci Web został zmieniony tak, aby inicjator [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) był uruchamiany podczas pierwszego odczytywania lub zapisywania danych w bazie danych (co się stało po wybraniu karty **uczniów** ):

![Fragment pliku Web. config](https://asp.net/media/4367421/mig.png)

Proces wdrażania również utworzył nowe parametry połączenia *(SchoolContext\_DatabasePublish*) dla migracje Code First, które mają być używane do aktualizowania schematu bazy danych i wypełniania bazy danych.

![Parametry połączenia w pliku Web. config](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Wdrożoną wersję pliku Web. config można znaleźć na własnym komputerze w *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Możesz uzyskać dostęp do wdrożonego pliku *Web. config* przy użyciu protokołu FTP. Aby uzyskać instrukcje, zobacz [ASP.NET Web Deployment using Visual Studio: wdrażanie aktualizacji kodu](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Postępuj zgodnie z instrukcjami, które zaczynają się od "Aby użyć narzędzia FTP, potrzebne są trzy rzeczy: adres URL FTP, nazwa użytkownika i hasło".

> [!NOTE]
> Aplikacja sieci Web nie implementuje zabezpieczeń, dlatego każda osoba, która odnajdzie adres URL, może zmienić dane. Aby uzyskać instrukcje dotyczące zabezpieczania witryny sieci Web, zobacz [wdrażanie aplikacji secure ASP.NET MVC z członkostwem, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure](/aspnet/core/security/authorization/secure-data). Można uniemożliwić innym osobom korzystanie z witryny przez zatrzymanie usługi przy użyciu portal zarządzania platformy Azure lub **Eksplorator serwera** w programie Visual Studio.

![Zatrzymaj element menu usługi App Service](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Scenariusze migracji zaawansowanej

Jeśli baza danych zostanie wdrożona automatycznie, zgodnie z opisem w tym samouczku, i wdrożono ją w witrynie sieci Web działającej na wielu serwerach, można uzyskać wiele serwerów próbujących uruchamiać migracje w tym samym czasie. Migracje są niepodzielne, więc jeśli dwa serwery próbują uruchomić tę samą migrację, jeden z nich powiedzie się, a druga zakończy się niepowodzeniem (przy założeniu, że operacje nie będą wykonywane dwa razy). W tym scenariuszu, jeśli chcesz uniknąć tych problemów, możesz wywoływać migracje ręcznie i skonfigurować własny kod, tak aby był tylko raz. Aby uzyskać więcej informacji, zobacz [Uruchamianie i wykonywanie skryptów migracji z kodu](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) na blogu Rowan Miller i [Migrowanie. exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) (w celu wykonywania migracji z wiersza polecenia).

Aby uzyskać informacje o innych scenariuszach migracji, zobacz [migracja zrzut ekranu przedstawiający Series](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="update-specific-migration"></a>Aktualizacja określonej migracji

`update-database -target MigrationName`

`update-database -target MigrationName` polecenie uruchamia migrację dodaną.

## <a name="ignore-migration-changes-to-database"></a>Ignoruj zmiany migracji do bazy danych

`Add-migration MigrationName -ignoreChanges`

`ignoreChanges` tworzy pustą migrację z bieżącym modelem jako migawką.

## <a name="code-first-initializers"></a>Inicjatory Code First

W sekcji Deployment wykorzystano inicjatora [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) . Code First udostępnia również inne inicjatory, w tym [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (wartość domyślna), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (używany wcześniej) i [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Inicjator `DropCreateAlways` może być przydatny do ustawiania warunków dla testów jednostkowych. Możesz również napisać własne inicjatory i można wywołać inicjalizację jawnie, jeśli nie chcesz czekać, aż aplikacja odczyta lub zapisze w bazie danych.

Aby uzyskać więcej informacji na temat inicjatorów, zobacz [Opis inicjatorów bazy danych w Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) i rozdział 6 [Entity Framework programowania](http://shop.oreilly.com/product/0636920022220.do) w podręczniku: Code First przez Julie Lerman i Rowan Miller.

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w temacie [ASP.NET Data Access — zalecane zasoby](xref:whitepapers/aspnet-data-access-content-map).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Włączono migracje Code First
> * Wdrożono aplikację na platformie Azure (opcjonalnie)

Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć bardziej skomplikowany model danych dla aplikacji ASP.NET MVC.
> [!div class="nextstepaction"]
> [Tworzenie bardziej złożonego modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
