---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: przygotowanie do wdrożenia bazy danych | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: cdcb3578725c41e3c801afd54e6d34455bc4b281
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618533"
---
# <a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: przygotowanie do wdrożenia bazy danych

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak pobrać projekt gotowy do wdrożenia bazy danych. Struktura bazy danych i niektóre (nie wszystkie) danych w dwóch bazach danych aplikacji muszą zostać wdrożone w środowiskach testowych, przejściowych i produkcyjnych.

Zazwyczaj podczas tworzenia aplikacji należy wprowadzić dane testowe do bazy danych, która nie ma być wdrażana w działającej witrynie. Można jednak również mieć pewne dane produkcyjne, które mają zostać wdrożone. W tym samouczku skonfigurujesz projekt firmy Contoso University i przygotujesz skrypty SQL, aby podczas wdrażania były uwzględniane poprawne dane.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Przykładowa aplikacja używa SQL Server Express LocalDB. SQL Server Express to bezpłatna wersja SQL Server. Jest on często używany podczas opracowywania, ponieważ jest oparty na tym samym aparacie bazy danych co pełne wersje SQL Server. Można testować przy użyciu SQL Server Express i mieć pewność, że aplikacja będzie działać tak samo w środowisku produkcyjnym, z kilkoma wyjątkami dotyczącymi funkcji, które różnią się w zależności od wersji SQL Server.

LocalDB jest specjalnym trybem wykonywania SQL Server Express, który umożliwia współpracę z bazami danych jako plikami *MDF* . Zazwyczaj pliki bazy danych LocalDB są przechowywane w folderze *danych\_aplikacji* projektu sieci Web. Funkcja wystąpienia użytkownika w SQL Server Express umożliwia również pracy z plikami *. mdf* , ale funkcja wystąpienia użytkownika jest przestarzała; w związku z tym LocalDB jest zalecane do pracy z plikami *. mdf* .

Zwykle SQL Server Express nie jest używany dla produkcyjnych aplikacji sieci Web. LocalDB w szczególności nie są zalecane do użycia w środowisku produkcyjnym z aplikacją sieci Web, ponieważ nie jest ona przeznaczona do pracy z usługami IIS.

W programie Visual Studio 2012 LocalDB jest instalowany domyślnie z programem Visual Studio. W programie Visual Studio 2010 i starszych wersjach SQL Server Express (bez LocalDB) jest instalowany domyślnie z programem Visual Studio; oznacza to, że zainstalowano go jako jedno z wymagań wstępnych w [pierwszym samouczku w tej serii](introduction.md).

Aby uzyskać więcej informacji na temat wersji SQL Server, w tym LocalDB, zobacz następujące zasoby [działające z bazami danych SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework i dostawcy Uniwersalni

W przypadku dostępu do bazy danych aplikacja firmy Contoso University wymaga, aby następujące oprogramowanie zostało wdrożone z aplikacją, ponieważ nie jest ona uwzględniona w .NET Framework:

- [ASP.NET uniwersalnych dostawców](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (umożliwia systemowi członkostwa ASP.NET używanie Azure SQL Database)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Ponieważ to oprogramowanie jest dołączone do pakietów NuGet, projekt jest już skonfigurowany, tak aby wymagane zestawy zostały wdrożone z projektem. (Linki wskazują bieżące wersje tych pakietów, które mogą być nowsze niż zainstalowane w projekcie początkowym pobrane dla tego samouczka).

W przypadku wdrażania w ramach dostawcy hostingu innej firmy zamiast platformy Azure upewnij się, że używasz Entity Framework 5,0 lub nowszego. Wcześniejsze wersje Migracje Code First wymagają pełnego zaufania, a większość dostawców hostingu będzie uruchamiać aplikację w średnim zaufaniu. Aby uzyskać więcej informacji na temat średniego zaufania, zobacz samouczek [wdrażanie w usługach IIS jako środowisko testowe](deploying-to-iis.md) .

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Konfigurowanie Migracje Code First wdrażania bazy danych aplikacji

Baza danych aplikacji firmy Contoso University jest zarządzana przez Code First i wdrażana przy użyciu Migracje Code First. Omówienie wdrażania bazy danych za pomocą Migracje Code First można znaleźć [w pierwszym samouczku w tej serii](introduction.md).

W przypadku wdrażania bazy danych aplikacji zazwyczaj nie można wdrożyć bazy danych programistycznych ze wszystkimi danymi w środowisku produkcyjnym, ponieważ większość danych jest prawdopodobnie tylko do celów testowych. Na przykład nazwy uczniów w testowej bazie danych są fikcyjne. Z drugiej strony często nie można wdrożyć tylko struktury bazy danych bez danych. Niektóre dane w bazie danych testowych mogą być rzeczywistymi danymi i muszą znajdować się w nich, gdy użytkownicy zaczynają korzystać z aplikacji. Na przykład baza danych może mieć tabelę zawierającą prawidłowe wartości klasy lub rzeczywiste nazwy działów.

Aby zasymulować ten typowy scenariusz, należy skonfigurować metodę `Seed` Migracje Code First, która wstawia do bazy danych tylko dane, które mają być w środowisku produkcyjnym. Ta metoda `Seed` nie powinna wstawiać danych testowych, ponieważ zostanie uruchomiona w środowisku produkcyjnym po Code First tworzenia bazy danych w środowisku produkcyjnym.

We wcześniejszych wersjach Code First przed udostępnieniem migracji była wspólna dla `Seed` metod wstawiania danych testowych również, ponieważ w przypadku każdej zmiany modelu podczas programowania baza danych musiała zostać całkowicie usunięta i ponownie utworzona od podstaw. Przy Migracje Code First dane testowe są zachowywane po zmianie bazy danych, więc w tym dane testowe w metodzie `Seed` nie są konieczne. Pobrany projekt korzysta z metody dołączania wszystkich danych w metodzie `Seed` klasy inicjatora. Ten samouczek obejmuje wyłączenie tej klasy inicjatora i włączenie migracji. Następnie należy zaktualizować metodę `Seed` w klasie konfiguracji migracji, tak aby wstawia tylko dane, które mają zostać wstawione w środowisku produkcyjnym.

Na poniższym diagramie przedstawiono schemat bazy danych aplikacji:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Na potrzeby tych samouczków zakłada się, że tabele `Student` i `Enrollment` powinny być puste podczas pierwszego wdrożenia lokacji. Inne tabele zawierają dane, które mają zostać wstępnie załadowane, gdy aplikacja się na żywo.

### <a name="disable-the-initializer"></a>Wyłącz inicjatora

Ponieważ będziesz używać Migracje Code First, nie musisz używać inicjatora Code First `DropCreateDatabaseIfModelChanges`. Kod dla tego inicjatora znajduje się w pliku *SchoolInitializer.cs* w projekcie CONTOSOUNIVERSITY. dal. Ustawienie w `appSettings` elemencie pliku *Web. config* powoduje, że ten inicjator jest uruchamiany za każdym razem, gdy aplikacja próbuje uzyskać dostęp do bazy danych po raz pierwszy:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Otwórz plik *Web. config* aplikacji i Usuń lub skomentuj element `add`, który określa klasę inicjatora Code First. `appSettings` element wygląda teraz następująco:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Innym sposobem określenia klasy inicjatora jest wykonanie wywołania `Database.SetInitializer` w metodzie `Application_Start` w pliku *Global. asax* . W przypadku włączenia funkcji migracji w projekcie, który używa tej metody do określenia inicjatora, Usuń ten wiersz kodu.

> [!NOTE]
> Jeśli używasz Visual Studio 2013, Dodaj następujące kroki między krokami 2 i 3: (a) w obszarze PMC wprowadź "Update-Package EntityFramework-Version 6.1.1", aby uzyskać aktualną wersję EF. Następnie (b) Skompiluj projekt, aby uzyskać listę błędów kompilacji i rozwiązać te problemy. Usuń instrukcje using dla przestrzeni nazw, które już nie istnieją, kliknij prawym przyciskiem myszy i kliknij przycisk Rozwiąż, aby dodać instrukcje using, gdy są potrzebne, i Zmień wystąpienia elementu System. Data. EntityState na system. Data. Entity. EntityState.

### <a name="enable-code-first-migrations"></a>Włącz Migracje Code First

1. Upewnij się, że projekt ContosoUniversity (nie ContosoUniversity. DAL) jest ustawiony jako projekt startowy. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz pozycję **Ustaw jako projekt startowy**. Migracje Code First będzie wyglądać w projekcie startowym w celu znalezienia parametrów połączenia z bazą danych.
2. W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. W górnej części okna **konsoli Menedżera pakietów** wybierz pozycję CONTOSOUNIVERSITY. dal jako projekt domyślny, a następnie w wierszu `PM>` wprowadź "Enable-migrations".

    ![Enable-migrations — polecenie](preparing-databases/_static/image4.png)

    (Jeśli zostanie wyświetlony komunikat o błędzie z informacją, że polecenie *enable-migrations* nie jest rozpoznawane, wprowadź polecenie *Update-Package EntityFramework-reinstall* i ponów próbę).

    To polecenie tworzy folder *migracji* w projekcie CONTOSOUNIVERSITY. dal i umieszcza w tym folderze dwa pliki: plik *Configuration.cs* , który służy do konfigurowania migracji, oraz plik *InitialCreate.cs* dla pierwszej migracji, która tworzy bazę danych.

    ![Folder migracji](preparing-databases/_static/image5.png)

    Projekt DAL został wybrany na liście rozwijanej **domyślny projekt** **konsoli Menedżera pakietów** , ponieważ polecenie `enable-migrations` musi być wykonane w projekcie, który zawiera klasę Code First Context. Gdy Klasa znajduje się w projekcie biblioteki klas, Migracje Code First szuka parametrów połączenia bazy danych w projekcie startowym dla rozwiązania. W rozwiązaniu ContosoUniversity projekt sieci Web został ustawiony jako projekt startowy. Jeśli nie chcesz wyznaczyć projektu, który ma parametry połączenia jako projekt startowy w programie Visual Studio, możesz określić projekt startowy w poleceniu programu PowerShell. Aby wyświetlić składnię polecenia, wprowadź polecenie `get-help enable-migrations`.

    `enable-migrations` polecenie automatycznie utworzył pierwszą migrację, ponieważ baza danych już istnieje. Alternatywą jest utworzenie bazy danych przez migracje. Aby to zrobić, należy użyć **Eksplorator serwera** lub **Eksplorator obiektów SQL Server** do usunięcia bazy danych ContosoUniversity przed włączeniem migracji. Po włączeniu migracji należy ręcznie utworzyć pierwszą migrację, wprowadzając polecenie "Add-Migration InitialCreate". Następnie można utworzyć bazę danych, wprowadzając polecenie "Update-Database".

### <a name="set-up-the-seed-method"></a>Konfigurowanie metody inicjatora

Na potrzeby tego samouczka dodasz stałe dane przez dodanie kodu do metody `Seed` klasy Migracje Code First `Configuration`. Migracje Code First wywołuje metodę `Seed` po każdej migracji.

Ponieważ metoda `Seed` jest uruchamiana po każdej migracji, po pierwszej migracji istnieją już dane znajdujące się w tabelach. Aby obsłużyć tę sytuację, użyjesz metody `AddOrUpdate`, aby zaktualizować wiersze, które zostały już wstawione, lub Wstaw je, jeśli jeszcze nie istnieją. Metoda `AddOrUpdate` może nie być najlepszym wyborem dla Twojego scenariusza. Aby uzyskać więcej informacji, zobacz temat zapoznaj [się z opcją dr 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

1. Otwórz plik *Configuration.cs* i Zastąp komentarze w metodzie `Seed` następującym kodem:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Odwołania do `List` mają w nich czerwoną falistej linie, ponieważ nie masz jeszcze instrukcji `using` dla swojej przestrzeni nazw. Kliknij prawym przyciskiem myszy jedno z wystąpień `List` i kliknij polecenie **Rozwiąż**, a następnie kliknij pozycję **System. Collections. Generic**.

    ![Rozwiązywanie przy użyciu instrukcji using](preparing-databases/_static/image6.png)

    To zaznaczenie menu dodaje następujący kod do instrukcji `using` w górnej części pliku.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Naciśnij kombinację klawiszy CTRL-SHIFT-B, aby skompilować projekt.

Projekt jest teraz gotowy do wdrożenia bazy danych *ContosoUniversity* . Po wdrożeniu aplikacji przy pierwszym uruchomieniu i przejściu do strony, która uzyskuje dostęp do bazy danych, Code First utworzy bazę danych i uruchomimy tę `Seed` metodę.

> [!NOTE]
> Dodawanie kodu do metody `Seed` jest jednym z wielu sposobów wstawiania stałych danych do bazy danych. Alternatywą jest dodanie kodu do `Up` i `Down` metod każdej klasy migracji. Metody `Up` i `Down` zawierają kod implementujący zmiany w bazie danych. Przykłady tych elementów znajdziesz w samouczku [wdrażanie aktualizacji bazy danych](deploying-a-database-update.md) .
> 
> Możesz również napisać kod, który wykonuje instrukcje SQL, za pomocą metody `Sql`. Na przykład jeśli dodano kolumnę budżetu do tabeli dział i chcesz zainicjować wszystkie budżety działu do $1 000,00 w ramach migracji, można dodać następujący wiersz kodu do metody `Up` dla tej migracji:
> 
> `Sql("UPDATE Department SET Budget = 1000");`

## <a name="create-scripts-for-membership-database-deployment"></a>Tworzenie skryptów dla wdrożenia bazy danych członkostwa

Aplikacja firmy Contoso University używa systemu członkostwa ASP.NET i uwierzytelniania formularzy do uwierzytelniania i autoryzowania użytkowników. Strona **kredyty dotyczące aktualizacji** jest dostępna tylko dla użytkowników, którzy znajdują się w roli administratora.

Uruchom aplikację i kliknij pozycję **kursy**, a następnie kliknij pozycję **Aktualizuj kredyty**.

![Kliknij przycisk Aktualizuj kredyty](preparing-databases/_static/image7.png)

Strona **logowania** zostanie wyświetlona, ponieważ strona **kredyty aktualizacji** wymaga uprawnień administracyjnych.

Wprowadź nazwę użytkownika *admin* i *devpwd* jako hasło, a następnie kliknij przycisk **Zaloguj**.

![Strona logowania](preparing-databases/_static/image8.png)

Zostanie wyświetlona strona **kredyty z aktualizacją** .

![Strona kredytów aktualizacji](preparing-databases/_static/image9.png)

Informacje o użytkownikach i rolach są w bazie danych *ASPNET-ContosoUniversity* , która jest określona przez parametry połączenia **DefaultConnection** w pliku *Web. config* .

Ta baza danych nie jest zarządzana przez Entity Framework Code First, dlatego nie można użyć migracji do wdrożenia. Za pomocą dostawcy dbDacFx można wdrożyć schemat bazy danych, a następnie skonfigurować profil publikacji w celu uruchomienia skryptu, który będzie wstawiać dane początkowe do tabel bazy danych.

> [!NOTE]
> Nowy system członkostwa ASP.NET (teraz o nazwie ASP.NET Identity) został wprowadzony z Visual Studio 2013. Nowy system umożliwia zachowanie tabel aplikacji i członkostwa w tej samej bazie danych. można także użyć Migracje Code First do wdrożenia obu tych programów. Aplikacja Przykładowa używa wcześniejszego systemu członkostwa ASP.NET, którego nie można wdrożyć przy użyciu Migracje Code First. Procedury wdrażania tej bazy danych członkostwa mają zastosowanie również do dowolnego innego scenariusza, w którym aplikacja musi wdrożyć bazę danych SQL Server, która nie została utworzona przez Entity Framework Code First.

Tutaj zwykle nie chcesz, aby te same dane były opracowywane w środowisku produkcyjnym. Podczas wdrażania lokacji po raz pierwszy często wyklucza się większość lub wszystkie utworzone konta użytkowników do testowania. W związku z tym pobrany projekt ma dwie bazy danych członkostwa: *ASPNET-ContosoUniversity. mdf* z użytkownikami programistycznymi i *ASPNET-ContosoUniversity-prod. mdf* z użytkownikami produkcyjnymi. W tym samouczku nazwy użytkowników są takie same w obu bazach danych : Administrator *i administrator*. Obaj użytkownicy mają hasło *devpwd* w bazie danych deweloperskich i *prodpwd* w produkcyjnej bazie danych.

Wdrażasz użytkowników programistycznych w środowisku testowym i użytkownikom produkcyjnym do przemieszczania i produkcji. Aby to zrobić, utworzysz dwa skrypty SQL w tym samouczku, jeden do programowania i jeden dla środowiska produkcyjnego, a następnie w dalszych samouczkach skonfigurujesz proces publikowania w celu ich uruchomienia.

> [!NOTE]
> Baza danych członkostwa przechowuje skrót hasła konta. Aby wdrożyć konta z jednego komputera do drugiego, należy się upewnić, że procedury tworzenia skrótów nie generują różnych skrótów na serwerze docelowym niż na komputerze źródłowym. Wygenerują one te same skróty w przypadku używania ASP.NET uniwersalnych dostawców, o ile nie zmienimy domyślnego algorytmu. Domyślny algorytm to HMACSHA256 i jest określony w atrybucie **walidacji** elementu **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** w pliku Web. config.

Skrypty wdrażania danych można tworzyć ręcznie przy użyciu programu SQL Server Management Studio (SSMS) lub narzędzia innej firmy. W tej pozostałej części tego samouczka pokazano, jak to zrobić w programie SSMS, ale jeśli nie chcesz instalować i korzystać z programu SSMS, możesz pobrać skrypty z ukończonej wersji projektu i przejść do sekcji, w której są przechowywane w folderze rozwiązania.

Aby zainstalować program SSMS, zainstaluj go z [Centrum pobierania: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) , klikając [ENU\x64\SQLManagementStudio\_x64\_plk. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) lub [ENU\x86\SQLManagementStudio\_x86\_plk. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). W przypadku wybrania niewłaściwego rozwiązania dla systemu instalacja nie powiedzie się i można spróbować jeszcze jeden.

(Należy pamiętać, że jest to 600 MB. Zainstalowanie programu może zająć dużo czasu i będzie wymagało ponownego uruchomienia komputera).

Na pierwszej stronie Centrum instalacji SQL Server kliknij pozycję **nowy SQL Server instalację autonomiczną lub Dodaj funkcje do istniejącej instalacji**, a następnie postępuj zgodnie z instrukcjami, akceptując domyślne opcje.

### <a name="create-the-development-database-script"></a>Tworzenie skryptu bazy danych programistycznych

1. Uruchom narzędzie SSMS.
2. W oknie dialogowym **łączenie z serwerem** wprowadź *(LocalDB) \V11.0* jako **nazwę serwera**, pozostaw **uwierzytelnianie** ustawione na **uwierzytelnianie systemu Windows**, a następnie kliknij przycisk **Połącz**.

    ![Serwer programu SSMS nawiązuje połączenie z serwerem](preparing-databases/_static/image10.png)
3. W oknie **Eksplorator obiektów** rozwiń węzeł **bazy danych**, kliknij prawym przyciskiem myszy pozycję **ASPNET-ContosoUniversity**, kliknij pozycję **zadania**, a następnie kliknij pozycję **Generuj skrypty**.

    ![Narzędzie SSMS Generuj skrypty](preparing-databases/_static/image11.png)
4. W oknie dialogowym **generowanie i publikowanie skryptów** kliknij pozycję **Ustaw opcje obsługi skryptów**.

    Możesz pominąć krok **Wybierz obiekty** , ponieważ wartością domyślną jest **skrypt cała baza danych i wszystkie obiekty bazy danych** .
5. Kliknij pozycję **Zaawansowane**.

    ![Opcje skryptów programu SSMS](preparing-databases/_static/image12.png)
6. W oknie dialogowym **Zaawansowane opcje obsługi skryptów** przewiń w dół do obszaru **dane w polu skrypt**, a następnie kliknij opcję **tylko dane** na liście rozwijanej.
7. Zmień **skrypt Database** na **wartość false**. Instrukcje USE nie są prawidłowe dla Azure SQL Database i nie są potrzebne do wdrożenia SQL Server Express w środowisku testowym.

    ![Tylko dane skryptu programu SSMS, brak instrukcji USE](preparing-databases/_static/image13.png)
8. Kliknij przycisk **OK**.
9. W oknie dialogowym **generowanie i publikowanie skryptów** pole **Nazwa pliku** określa miejsce, w którym zostanie utworzony skrypt. Zmień ścieżkę do folderu rozwiązania (folder, w którym znajduje się plik ContosoUniversity. sln) i nazwę pliku *ASPNET-Data-dev. SQL*.
10. Kliknij przycisk **dalej** , aby przejść do karty **Podsumowanie** , a następnie ponownie kliknij przycisk **dalej** , aby utworzyć skrypt.

    ![Utworzono skrypt programu SSMS](preparing-databases/_static/image14.png)
11. Kliknij przycisk **Zakończ**.

### <a name="create-the-production-database-script"></a>Tworzenie skryptu produkcyjnej bazy danych

Ponieważ projekt nie został uruchomiony z produkcyjną bazą danych, nie jest jeszcze dołączony do wystąpienia LocalDB. W związku z tym musisz najpierw dołączyć bazę danych.

1. W **Eksplorator obiektów**SSMS kliknij prawym przyciskiem myszy pozycję **bazy danych** , a następnie kliknij pozycję **Dołącz**.

    ![Dołącz do programu SSMS](preparing-databases/_static/image15.png)
2. W oknie dialogowym **dołączanie baz danych** kliknij pozycję **Dodaj** , a następnie przejdź do pliku *ASPNET-ContosoUniversity-prod. mdf* w folderze *dane\_aplikacji* .

     ![Plik MDF Dodaj do dołączenia](preparing-databases/_static/image16.png)
3. Kliknij przycisk **OK**.
4. Postępuj zgodnie z tą samą procedurą, która została wcześniej użyta do utworzenia skryptu dla pliku produkcyjnego. Nazwij plik skryptu *ASPNET-Data-prod. SQL*.

## <a name="summary"></a>Podsumowanie

Obie bazy danych są teraz gotowe do wdrożenia i masz dwa skrypty wdrażania danych w folderze rozwiązania.

![Skrypty wdrażania danych](preparing-databases/_static/image17.png)

W poniższym samouczku skonfigurujesz ustawienia projektu, które mają wpływ na wdrażanie, i konfigurujesz automatyczne przekształcenia pliku *Web. config* dla ustawień, które muszą być inne w wdrożonej aplikacji.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji o narzędziu NuGet, zobacz [Manage Project librarys with NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) i [NuGet](http://docs.nuget.org/docs/start-here/overview). Jeśli nie chcesz używać narzędzia NuGet, musisz dowiedzieć się, jak przeanalizować pakiet NuGet, aby określić, co jest wykonywane podczas instalacji. (Na przykład może to spowodować skonfigurowanie transformacji *Web. config* , skonfigurowanie skryptów programu PowerShell do uruchamiania w czasie kompilacji itp.). Aby dowiedzieć się więcej na temat działania programu NuGet, zobacz [Tworzenie i publikowanie pakietu](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) oraz [pliku konfiguracji i przekształceń kodu źródłowego](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Poprzednie](introduction.md)
> [dalej](web-config-transformations.md)
