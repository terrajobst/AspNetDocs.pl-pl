---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie do testowania | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 738318cce442fdc5d58dd1e4c992d4941be2487e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74591240"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie do testowania

Autor [Dykstra](https://github.com/tdykstra)

W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2017. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

Aby zapoznać się z bieżącą wersją wdrożenia na platformie Azure, zobacz [Tworzenie aplikacji internetowej ASP.NET Core na platformie Azure](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="overview"></a>Omówienie

W tym samouczku wdrożono aplikację sieci Web ASP.NET na serwerze Internet Information Server (IIS) na komputerze lokalnym.

Ogólnie podczas opracowywania aplikacji należy ją uruchomić i przetestować w programie Visual Studio. Domyślnie projekty aplikacji sieci Web w programie Visual Studio 2017 używają IIS Express jako serwera deweloperskiego sieci Web. IIS Express zachowuje się jak pełnych usług IIS niż serwer programistyczny programu Visual Studio (znany również jako Cassini), którego program Visual Studio 2017 domyślnie używa. Jednak serwer sieci Web programistyczny działa tak samo jak w przypadku usług IIS. W związku z tym aplikacja może działać i testować prawidłowo w programie Visual Studio, ale kończy się niepowodzeniem, gdy zostanie wdrożona w usługach IIS.

Możesz niezawodnie przetestować aplikację na dwa sposoby:

1. Wdróż aplikację w usługach IIS na komputerze deweloperskim, korzystając z tego samego procesu, który będzie używany później do wdrożenia go w środowisku produkcyjnym.

   Program Visual Studio można skonfigurować tak, aby korzystał z usług IIS podczas uruchamiania projektu sieci Web, ale nie przetestować procesu wdrażania. Ta metoda sprawdza poprawność procesu wdrażania i sprawdzanie, czy aplikacja działa prawidłowo w ramach usług IIS.

2. Wdróż aplikację w środowisku testowym, podobnym do środowiska produkcyjnego. 
 
   Środowisko produkcyjne dla tych samouczków jest Web Apps w Azure App Service. Idealnym środowiskiem testowym jest dodatkowa aplikacja internetowa utworzona w usłudze platformy Azure. Mimo że można go skonfigurować w taki sam sposób jak w przypadku produkcyjnej aplikacji sieci Web, można używać jej tylko do testowania.

Opcja 2 to najbardziej niezawodny sposób na przetestowanie. Jeśli używasz opcji 2, nie musisz koniecznie używać opcji 1. Jeśli jednak wdrażasz w ramach dostawcy hostingu innej firmy, opcja 2 może nie być wykonalna lub być kosztowna, dlatego ta seria samouczków pokazuje obie metody. Wskazówki dotyczące opcji 2 są dostępne w samouczku [wdrażanie do środowiska produkcyjnego](deploying-to-production.md) .

Aby uzyskać więcej informacji o korzystaniu z serwerów sieci Web w programie Visual Studio, zobacz [serwery sieci Web w programie Visual Studio for ASP.NET — projekty sieci Web](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Pobierz początkowy projekt firmy Contoso University

Pobierz i zainstaluj rozwiązanie i projekt programu Visual Studio z uczelnią contoso. To rozwiązanie zawiera ukończony samouczek. 

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Instalowanie usług IIS

Aby wdrożyć usługi IIS na komputerze deweloperskim, upewnij się, że usługi IIS i Web Deploy są zainstalowane. Domyślnie program Visual Studio instaluje Web Deploy, ale usługi IIS nie są uwzględnione w konfiguracji domyślnej systemu Windows 10, Windows 8 lub Windows 7. Jeśli usługi IIS zostały już zainstalowane i domyślna pula aplikacji została już ustawiona na .NET 4, przejdź do [następnej sekcji](#sqlexpress).

1. Zalecamy używanie [Instalatora platformy sieci Web (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) do instalowania usług IIS i Web Deploy. WPI instaluje zalecaną konfigurację usług IIS, która obejmuje usługi IIS i Web Deploy w razie potrzeby wymagania wstępne.

   Jeśli program IIS został już zainstalowany, Web Deploy lub dowolny z wymaganych składników, WPI zainstaluje tylko to, co się nie ma.

   * Użyj Instalatora platformy sieci Web, aby zainstalować usługi IIS i Web Deploy:
    
     ![Instalowanie usług IIS przy użyciu WPI](deploying-to-iis/_static/image30.png)

     ![Instalowanie Web Deploy przy użyciu WPI](deploying-to-iis/_static/image31.png)

     Zobaczysz komunikat informujący o tym, że usługi IIS 7 zostaną zainstalowane. Link działa dla usług IIS 8 w systemie Windows 8. Jednak w przypadku systemu Windows 8 i nowszych wykonaj następujące kroki, aby upewnić się, że zainstalowano program ASP.NET 4,7:

   * Otwórz **Panel sterowania** > **programy** > **programy i funkcje** > **włączać lub wyłączać funkcje systemu Windows**.

   * Rozwiń węzeł **Internet Information Services**, **usługi World Wide Web**i **funkcje projektowania aplikacji**.
   
   * Upewnij się, że wybrano **ASP.NET 4,7** .

     ![Wybierz ASP.NET 4,7](deploying-to-iis/_static/image1a.png)

   * Upewnij się, że jest wybrana opcja **usługi World Wide Web** i **Konsola zarządzania usługami IIS** . Spowoduje to zainstalowanie usług IIS i Menedżera usług IIS.
    
     ![Wybierz World Wide Web usług](deploying-to-iis/_static/image24.png)    
  
   * Wybierz **przycisk OK**. Pojawią się komunikaty okna dialogowego informujące o instalacji.

Po zainstalowaniu usług IIS Uruchom **Menedżera usług IIS** , aby upewnić się, że .NET Framework wersja 4 jest przypisana do domyślnej puli aplikacji.

1. Naciśnij klawisze WINDOWS + R, aby otworzyć okno dialogowe **Uruchamianie** .

   (W systemie Windows 8 lub nowszym wprowadź "Run" na stronie **startowej** . W systemie Windows 7 wybierz opcję **Uruchom** z menu **Start** . Jeśli **Run** nie ma w menu **Start** , kliknij prawym przyciskiem myszy pasek zadań, wybierz polecenie **Właściwości**, wybierz kartę **menu Start** , wybierz opcję **Dostosuj**i wybierz **polecenie Uruchom**.

2. Wprowadź wartość "inetmgr" i wybierz **przycisk OK**.

3. W okienku **połączenia** rozwiń węzeł serwera, a następnie wybierz pozycję **Pule aplikacji**. W okienku **Pule aplikacji** **, jeśli do** programu .NET Framework w wersji 4 zostanie przypisany zestaw na poniższej ilustracji, przejdź do następnej sekcji.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. Jeśli widzisz tylko dwie pule aplikacji i oba są ustawione na .NET Framework 2,0, zainstaluj ASP.NET 4 w usługach IIS.

   W przypadku systemu Windows 8 lub nowszego zapoznaj się z instrukcjami w poprzedniej sekcji, aby upewnić się, że ASP.NET 4,7 jest zainstalowana, lub zobacz [jak zainstalować ASP.NET 4,5 w systemie Windows 8 i Windows Server 2012](https://support.microsoft.com/kb/2736284). W systemie Windows 7 Otwórz okno wiersza polecenia, klikając prawym przyciskiem myszy **wiersz polecenia** w menu **Start** systemu Windows i wybierając polecenie **Uruchom jako administrator**. Uruchom [aspnet\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) , aby zainstalować ASP.NET 4 w usługach IIS przy użyciu następujących poleceń. (W systemach 32-bitowych Zastąp ciąg "Framework64" "platformą").

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   To polecenie tworzy nowe pule aplikacji dla .NET Framework 4, ale domyślna pula aplikacji pozostanie ustawiona na 2,0. Wdrażasz aplikację, która jest przeznaczona dla platformy .NET 4, do tej puli aplikacji, więc Zmień pulę aplikacji na .NET 4.

5. Jeśli **Menedżer usług IIS**został zamknięty, uruchom go ponownie, rozwiń węzeł serwera, a następnie wybierz pozycję **Pule aplikacji**.

6. W okienku **Pule aplikacji** wybierz pozycję **Domyślna pula**. W okienku **Akcje** wybierz pozycję **Ustawienia podstawowe**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. W oknie dialogowym **Edytowanie puli aplikacji** Zmień **wersję środowiska .NET CLR** na **.NET CLR v 4.0.30319**. Wybierz **przycisk OK**.

   ![Selecting_. NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

Teraz można przystąpić do publikowania aplikacji sieci Web w usługach IIS. Należy jednak najpierw utworzyć bazy danych do testowania.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Zainstaluj SQL Server Express

LocalDB nie została zaprojektowana do pracy w usługach IIS, dlatego w środowisku testowym musi być zainstalowany SQL Server Express. Jeśli używasz programu Visual Studio 2010 SQL Server Express, jest on już instalowany domyślnie. Jeśli używasz programu Visual Studio 2012 lub nowszego, zainstaluj SQL Server Express.

Aby zainstalować SQL Server Express, Pobierz i zainstaluj go z [Centrum pobierania: Microsoft SQL Server 2017 Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

Na pierwszej stronie Centrum instalacji programu SQL Server wybierz pozycję **nowy SQL Server Instalacja autonomiczna lub Dodaj funkcje do istniejącej instalacji** i postępuj zgodnie z instrukcjami dotyczącymi akceptowania opcji domyślnych. W Kreatorze instalacji zaakceptuj ustawienia domyślne. Aby uzyskać więcej informacji na temat opcji instalacji, zobacz [instalowanie SQL Server z Kreatora instalacji (Instalator)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Tworzenie SQL Server Express baz danych dla środowiska testowego

Aplikacja firmy Contoso University ma dwie bazy danych: 

1. Baza danych członkostwa 
2. Baza danych aplikacji 

Te bazy danych można wdrożyć w dwóch osobnych bazach danych lub w pojedynczej bazie danych. Łączenie ich sprawia, że tworzenie sprzężeń między nimi jest łatwiejsze. 

W przypadku wdrażania w ramach dostawcy hostingu innej firmy Twój plan hostingu może również przyczynić się do połączenia. Na przykład dostawca może obciążać więcej dla wielu baz danych lub nawet nie zezwalać na więcej niż jedną bazę danych.

W tym samouczku zostaną wdrożone do dwóch baz danych w środowisku testowym oraz do jednej bazy danych w środowiskach przejściowych i produkcyjnych.

W menu **Widok** w programie Visual Studio wybierz pozycję **Eksplorator serwera** (**Eksplorator bazy danych** w Visual Web Developer). Kliknij prawym przyciskiem myszy pozycję **połączenia danych** , a następnie wybierz pozycję **Utwórz nową bazę danych SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

W oknie dialogowym **Utwórz nową bazę danych SQL Server** wprowadź wartość ".\SQLEXPRESS" w polu **Nazwa serwera** i "ASPNET-ContosoUniversity" w polu **Nazwa nowej bazy danych** . Wybierz **przycisk OK**.

![Utwórz ASPNET-ContosoUniversity](deploying-to-iis/_static/image9.png)

Postępuj zgodnie z tą samą procedurą, aby utworzyć nową bazę danych SQL Server Express szkoły o nazwie `ContosoUniversity`.

**Eksplorator serwera** przedstawia dwie nowe bazy danych.

![Nowe bazy danych w Eksplorator serwera](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Utwórz skrypt grantu dla nowych baz danych

Gdy aplikacja jest uruchamiana w usługach IIS na komputerze deweloperskim, aplikacja używa domyślnych poświadczeń puli aplikacji w celu uzyskania dostępu do bazy danych. Jednak domyślnie Pula aplikacji nie ma uprawnień do otwierania baz danych. Oznacza to, że należy uruchomić skrypt w celu udzielenia tego uprawnienia. W tej sekcji utworzysz ten skrypt i uruchomisz go później w celu upewnienia się, że aplikacja może otworzyć bazy danych w programie IIS.

W edytorze tekstów Skopiuj następujące polecenia SQL do nowego pliku i Zapisz je jako *Grant. SQL*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

W programie Visual Studio Otwórz rozwiązanie firmy Contoso University. Kliknij prawym przyciskiem myszy Rozwiązanie (nie jeden z projektów) i wybierz polecenie **Dodaj**. Wybierz pozycję **istniejący element**, przejdź do pozycji *Grant. SQL*i otwórz ją.

> [!NOTE]
> Ten skrypt jest przeznaczony do pracy z SQL Server Express 2012 lub nowszym oraz z ustawieniami usług IIS w systemie Windows 10, Windows 8 lub Windows 7, które są określone w tym samouczku. Jeśli używasz innej wersji SQL Server lub systemu Windows lub jeśli program IIS zostanie skonfigurowany na komputerze inaczej, może być wymagane wprowadzenie zmian w tym skrypcie. Aby uzyskać więcej informacji na temat skryptów SQL Server, zobacz [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Uwaga dotycząca zabezpieczeń** Ten skrypt daje `db_owner` uprawnienia dostępu do bazy danych w czasie wykonywania, która jest dostępna w środowisku produkcyjnym. W niektórych scenariuszach możesz chcieć określić użytkownika, który ma pełne uprawnienia do aktualizacji schematu bazy danych tylko do wdrożenia i określić dla czasu wykonywania inny użytkownik, który ma uprawnienia tylko do odczytu i zapisu danych. Aby uzyskać więcej informacji, zobacz [przeglądanie automatycznych zmian w pliku Web. config dla migracje Code First](#reviewingmigrations) w dalszej części tego samouczka.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Uruchamianie skryptu grantu w bazie danych aplikacji

Profil publikacji można skonfigurować tak, aby uruchamiał skrypt przydzielenia w bazie danych członkostwa podczas wdrażania, ponieważ to wdrożenie bazy danych korzysta z dostawcy [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) . Nie można uruchamiać skryptów podczas wdrażania Migracje Code First, czyli sposobu wdrażania bazy danych aplikacji. Oznacza to, że trzeba ręcznie uruchomić skrypt przed wdrożeniem w bazie danych aplikacji.

1. W programie Visual Studio Otwórz utworzony wcześniej plik *Grant. SQL* .

2. Wybierz pozycję **Połącz**. 

    ![Przycisk Połącz](deploying-to-iis/_static/image11.png)

3. W oknie dialogowym **łączenie z serwerem** wprowadź *.\SQLEXPRESS* jako **nazwę serwera**. Wybierz pozycję **Połącz**.

4. Z listy rozwijanej baza danych wybierz pozycję **ContosoUniversity**. Wybierz pozycję **Wykonaj**. 

   ![](deploying-to-iis/_static/image12.png)

Domyślna tożsamość puli aplikacji ma teraz wystarczające uprawnienia w bazie danych aplikacji, aby Migracje Code First do tworzenia tabel bazy danych podczas uruchamiania aplikacji.

## <a name="publish-to-iis"></a>Publikowanie w usługach IIS

Istnieje kilka sposobów wdrażania programu w usługach IIS przy użyciu programu Visual Studio i Web Deploy:

* Użyj programu Visual Studio, a następnie kliknij pozycję Publikuj.
* Publikowanie z poziomu wiersza polecenia.
* Utwórz pakiet wdrożeniowy i zainstaluj go za pomocą Menedżera usług IIS. Pakiet zawiera plik. zip ze wszystkimi plikami i metadanymi wymaganymi do zainstalowania lokacji w usługach IIS.
* Utwórz pakiet wdrożeniowy i zainstaluj go przy użyciu wiersza polecenia.

Proces, w którym nastąpiło przechodzenie w poprzednich samouczkach, aby skonfigurować program Visual Studio do automatyzowania zadań wdrażania, ma zastosowanie do wszystkich tych metod. W tych samouczkach zostaną użyte dwie pierwsze metody. Aby uzyskać informacje o korzystaniu z pakietów wdrożeniowych, zobacz [wdrażanie aplikacji sieci Web przez tworzenie i instalowanie pakietu wdrożeniowego](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) sieci Web w mapie zawartości Web Deployment dla programu Visual Studio i ASP.NET.

Przed opublikowaniem upewnij się, że program Visual Studio jest uruchomiony w trybie administratora. Jeśli na pasku tytułu nie widzisz **(Administrator)** , Zamknij program Visual Studio. Na stronie **startowej** systemu Windows 8 (lub nowszej) lub menu **Start** systemu Windows 7 kliknij prawym przyciskiem myszy ikonę programu Visual Studio i wybierz polecenie **Uruchom jako administrator**. Tryb administratora jest wymagany tylko do publikowania w przypadku publikowania w usługach IIS na komputerze lokalnym.

### <a name="create-the-publish-profile"></a>Tworzenie profilu publikowania

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **ContosoUniversity** (a nie projekt **ContosoUniversity. dal** ). Wybierz pozycję **Publikuj**. Zostanie wyświetlona strona **Publikowanie** .

2. Wybierz pozycję **Nowy profil**. Zostanie wyświetlone okno dialogowe **Wybieranie elementu docelowego publikowania** .

3. Wybierz opcję **IIS, FTP itp**. Wybierz pozycję **Utwórz profil**. Zostanie wyświetlony Kreator **publikacji** .

   ![Karta profil Kreatora publikacji sieci Web](deploying-to-iis/_static/image26.png)

4. Z menu rozwijanego **Metoda publikowania** wybierz pozycję **Web Deploy**.

5. W obszarze **serwer**wprowadź wartość *localhost*.

6. W obszarze **Nazwa lokacji**wprowadź *wartość domyślna witryna sieci Web/ContosoUniversity*.

7. W obszarze **docelowy adres URL**wprowadź *http://localhost/ContosoUniversity* .

   Ustawienie **docelowego adresu URL** nie jest wymagane. Gdy program Visual Studio zakończy wdrażanie aplikacji, automatycznie otwiera domyślną przeglądarkę na tym adresie URL. Jeśli nie chcesz, aby przeglądarka była otwierana automatycznie po wdrożeniu, pozostaw to pole puste.

8. Wybierz pozycję **Weryfikuj połączenie** , aby sprawdzić, czy ustawienia są poprawne i czy można nawiązać połączenie z usługami IIS na komputerze lokalnym.

   Zielony znacznik wyboru weryfikuje, czy połączenie zostało nawiązane pomyślnie.

   ![Karta połączenia Kreatora publikacji sieci Web](deploying-to-iis/_static/image27.png)

9. Wybierz pozycję **dalej** , aby przejść do karty **Ustawienia** .

10. Pole listy rozwijanej **Konfiguracja** określa konfigurację kompilacji do wdrożenia. Pozostaw ustawioną wartość domyślną **Release**. Nie będziesz wdrażać kompilacji debugowania w tym samouczku.

11. Rozwiń węzeł **Opcje publikowania plików**. Wybierz pozycję **Wyklucz pliki z folderu danych\_aplikacji**.

    W środowisku testowym aplikacja uzyskuje dostęp do baz danych utworzonych w lokalnym wystąpieniu SQL Server Express, a nie plików. mdf w folderze *dane\_aplikacji* .

12. Pozostaw opcję **prekompilowanie podczas publikowania** i **Usuń dodatkowe pliki w miejscu docelowym** wyczyszczone.

    ![Opcje publikowania plików na karcie Ustawienia](deploying-to-iis/_static/image15a.png)

    Prekompilowanie jest opcją przydatną głównie w przypadku dużych witryn. Może skrócić czas uruchamiania podczas pierwszego żądania strony po opublikowaniu lokacji.

    Nie musisz usuwać dodatkowych plików, ponieważ jest to Twoje pierwsze wdrożenie i nie będzie jeszcze żadnych plików w folderze docelowym.

    > [!NOTE] 
    > Jeśli wybierzesz opcję **Usuń dodatkowe pliki w miejscu docelowym** dla kolejnego wdrożenia w tej samej lokacji, upewnij się, że używasz funkcji wersji zapoznawczej, aby zobaczyć, które pliki zostaną usunięte przed wdrożeniem. Oczekiwane zachowanie polega na tym, że Web Deploy usunie pliki na serwerze docelowym, który został usunięty z projektu. Jednak cała struktura folderów w folderach źródłowych i docelowych jest porównywana; w niektórych scenariuszach Web Deploy mogą usunąć pliki, które nie mają być usuwane.
    > 
    > Jeśli na przykład aplikacja sieci Web znajduje się w podfolderze na serwerze podczas wdrażania projektu w folderze głównym, podfolder zostanie usunięty. Może istnieć jeden projekt witryny głównej w contoso.com i inny projekt dla blogu w witrynie contoso.com/blog. Aplikacja w blogu znajduje się w podfolderze. W przypadku wybrania opcji **Usuń dodatkowe pliki w miejscu docelowym** podczas wdrażania lokacji głównej aplikacja blogu zostanie usunięta.
    > 
    > Innym przykładem może zostać nieoczekiwanie usunięte\_aplikacji. Niektóre bazy danych, takie jak SQL Server Compact przechowują pliki bazy danych w folderze danych\_aplikacji. Po wstępnym wdrożeniu nie chcesz kopiować plików bazy danych w kolejnych wdrożeniach, więc wybierz opcję **Wyklucz\_dane aplikacji** na karcie pakowanie/publikowanie w sieci Web. Po wybraniu tej czynności Jeśli **usuniesz dodatkowe pliki w miejscu docelowym** , pliki bazy danych i folder danych\_aplikacji zostaną usunięte przy następnym opublikowaniu.

### <a name="configure-deployment-for-the-membership-database"></a>Skonfiguruj wdrożenie dla bazy danych członkostwa

Poniższe kroki dotyczą bazy danych **DefaultConnection** w sekcji **bazy danych** okna dialogowego.

1. W polu **ciąg połączenia zdalnego** wprowadź następujące parametry połączenia wskazujące nową bazę danych członkostwa SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   Proces wdrażania umieszcza te parametry połączenia we wdrożonym pliku Web. config, ponieważ wybierane są **Parametry połączenia w czasie wykonywania** .

    Możesz również uzyskać parametry połączenia z **Eksplorator serwera**. W **Eksplorator serwera**rozwiń węzeł **połączenia danych** i wybierz **&lt;MachineName&gt;\sqlexpress.ASPNET-ContosoUniversity** Database, a następnie w oknie **Właściwości** skopiuj wartość **Parametry połączenia** . Te parametry połączenia będą mieć jedno dodatkowe ustawienie, które można usunąć: `Pooling=False`.

2. Wybierz pozycję **Aktualizuj bazę danych**.

   Powoduje to, że schemat bazy danych zostanie utworzony w docelowej bazie danych podczas wdrażania. W następnych krokach należy określić dodatkowe skrypty, które należy uruchomić: jeden w celu przyznania dostępu do bazy danych do domyślnej puli aplikacji i jednej do wdrożenia danych.

3. Wybierz pozycję **Konfiguruj aktualizacje bazy danych**.
 
4. W oknie dialogowym **Konfiguruj aktualizacje bazy danych** wybierz pozycję **Dodaj skrypt SQL**. Przejdź do skryptu *Grant. SQL* , który został zapisany wcześniej w folderze rozwiązania.

5. Powtórz ten proces, aby dodać skrypt *ASPNET-Data-dev. SQL* .

   ![Konfigurowanie aktualizacji bazy danych dla członkostwa](deploying-to-iis/_static/image16.png)

6. Wybierz pozycję **Zamknij**.

### <a name="configure-deployment-for-the-application-database"></a>Skonfiguruj wdrożenie dla bazy danych aplikacji

Gdy program Visual Studio wykryje klasę `DbContext` Entity Framework, tworzy wpis w sekcji **baz danych** , w której jest zaznaczone pole wyboru **Wykonaj migracje Code First** zamiast dla **bazy danych aktualizacji** . Na potrzeby tego samouczka użyjesz tego pola wyboru, aby określić wdrożenie Migracje Code First.

W niektórych scenariuszach może być używana `DbContext` baza danych, ale zamiast migracji do wdrożenia bazy danych ma być używany dostawca dbDacFx. W takim przypadku zapoznaj się z tematem [Jak mogę wdrożyć bazę danych Code First bez migracji?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) w artykule ASP.NET Web Deployment — często zadawane pytania w witrynie MSDN.

Poniższe kroki dotyczą bazy danych **SchoolContext** w sekcji **bazy danych** okna dialogowego.

1. W polu **ciąg połączenia zdalnego** wprowadź następujące parametry połączenia wskazujące nową bazę danych aplikacji SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   Proces wdrażania umieszcza te parametry połączenia we wdrożonym pliku Web. config, ponieważ wybierane są **Parametry połączenia w czasie wykonywania** .

   Możesz również uzyskać parametry połączenia z bazą danych aplikacji z **Eksplorator serwera** w taki sam sposób, jak parametry połączenia z bazą danych członkostwa.

2. Wybierz pozycję **wykonaj migracje Code First (uruchamiaj przy uruchamianiu aplikacji)** .

   Ta opcja powoduje, że proces wdrażania skonfiguruje wdrożony plik Web. config, aby określić inicjator `MigrateDatabaseToLatestVersion`. Ten inicjator automatycznie aktualizuje bazę danych do najnowszej wersji, gdy aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu.

### <a name="configure-publish-profile-transforms"></a>Konfiguruj transformacje profilu publikowania

1. Wybierz pozycję **Zamknij**. Wybierz opcję **tak** , gdy zostanie wyświetlony monit o zapisanie zmian.

2. W **Eksplorator rozwiązań**rozwiń węzeł **Właściwości**, rozwiń węzeł **PublishProfiles**.

3. Kliknij prawym przyciskiem myszy *CustomProfile. pubxml* i zmień jej nazwę *test. pubxml*.

4. Kliknij prawym przyciskiem myszy pozycję *test. pubxml*. Wybierz pozycję **Dodaj przekształcenie konfiguracji**.

   ![Menu Dodaj przekształcenie konfiguracji](deploying-to-iis/_static/image17.png)

   Program Visual Studio utworzy plik transformacji *Web. test. config* i otworzy go.

5. W pliku transformacji *Web. test. config* Wstaw następujący kod bezpośrednio po tagu otwierającym konfigurację.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    W przypadku korzystania z profilu publikowania testowego, to przekształcenie ustawia wskaźnik środowiska na "test". W wdrożonej witrynie zobaczysz "(test)" po nagłówku "Contoso University" H1.

6. Zapisz i zamknij plik.

7. Kliknij prawym przyciskiem myszy plik *Web. test. config* , a następnie wybierz polecenie **Podgląd transformacji** , aby upewnić się, że zakodowana transformacja generuje oczekiwane zmiany.

    W oknie **Podgląd pliku Web. config** zostanie wyświetlona informacja o wyniku zastosowania zarówno transformacji *Web. release. config* , jak i *pliku Web. test. config* .

### <a name="preview-the-deployment-updates"></a>Podgląd aktualizacji wdrożenia

1. Otwórz ponownie kreatora **publikowania sieci Web** (kliknij prawym przyciskiem myszy projekt ContosoUniversity, wybierz pozycję **Opublikuj**, a następnie pozycję **Podgląd**).

2. W oknie dialogowym **Podgląd** wybierz pozycję **Rozpocznij podgląd** , aby wyświetlić listę plików, które zostaną skopiowane. 

   ![Podgląd publikacji](deploying-to-iis/_static/image29.png)

   Możesz również wybrać link **Podgląd bazy danych** , aby wyświetlić skrypty, które będą uruchamiane w bazie danych członkostwa. (Żadne skrypty nie są uruchamiane na potrzeby wdrażania Migracje Code First, więc nie ma niczego do przejrzenia bazy danych aplikacji).

3. Wybierz pozycję **Publikuj**.

   Jeśli program Visual Studio nie jest w trybie administratora, może zostać wyświetlony komunikat o błędzie uprawnień. W takim przypadku Zamknij program Visual Studio, otwórz go w trybie administratora, a następnie ponów próbę opublikowania.

   Jeśli program Visual Studio jest w trybie administratora, w oknie **danych wyjściowych** są raportowane pomyślne kompilacje i publikowanie.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Jeśli adres URL został wprowadzony w polu **docelowy adres URL** na karcie **połączenie** profilu publikowania, przeglądarka automatycznie otworzy stronę główną firmy Contoso University działającą w usługach IIS na komputerze.

## <a name="test-in-the-test-environment"></a>Testowanie w środowisku testowym

Należy zauważyć, że wskaźnik środowiska pokazuje "(test)" zamiast "(dev)", który pokazuje, że transformacja *pliku Web. config* dla wskaźnika środowiska zakończyła się pomyślnie.

Uruchom stronę **instruktorów** , aby sprawdzić, czy Code First wypełniania bazy danych danymi instruktora. Po wybraniu tej strony może upłynąć kilka minut, ponieważ Code First tworzy bazę danych, a następnie uruchamia metodę `Seed`. (Nie było to zrobione na stronie głównej, ponieważ aplikacja nie próbowała jeszcze uzyskać dostępu do bazy danych).

Wybierz kartę **studenci** , aby sprawdzić, czy wdrożona baza danych nie ma uczniów.

Wybierz pozycję **Dodaj uczniów** z menu **uczniów** . Dodaj studenta, a następnie Wyświetl nowych uczniów na stronie **uczniów** . Umożliwia to sprawdzenie, czy można pomyślnie zapisywać dane w bazie danych.

Z menu **kursy** wybierz pozycję **Aktualizuj kredyty**. Strona **kredyty aktualizacji** wymaga uprawnień administratora, więc zostanie wyświetlona strona **logowania** . Wprowadź utworzone wcześniej poświadczenia konta administratora ("admin" i "devpwd"). Zostanie wyświetlona strona **kredyty aktualizacji** . Spowoduje to sprawdzenie, czy konto administratora utworzone w poprzednim samouczku zostało prawidłowo wdrożone w środowisku testowym.

Sprawdź, czy folder *ELMAH* istnieje w folderze *c:\inetpub\wwwroot\ContosoUniversity* , w którym znajduje się tylko plik zastępczy.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Przejrzyj automatyczne zmiany w pliku Web. config dla Migracje Code First

Otwórz plik *Web. config* we wdrożonej aplikacji pod adresem *C:\inetpub\wwwroot\ContosoUniversity* i sprawdź, gdzie skonfigurowany proces wdrażania migracje Code First automatycznie aktualizować bazę danych do najnowszej wersji.

![](deploying-to-iis/_static/image21.png)

Proces wdrażania również utworzył nowe parametry połączenia dla Migracje Code First, które mają być używane wyłącznie do aktualizacji schematu bazy danych:

![Database_Publish parametry połączenia](deploying-to-iis/_static/image22.png)

Te dodatkowe parametry połączenia umożliwiają określenie jednego konta użytkownika dla aktualizacji schematu bazy danych i innego konta użytkownika na potrzeby dostępu do danych aplikacji. Można na przykład przypisać rolę **właściciela\_bazy danych** do migracje Code First i **bazy danych\_elementu DataReader** przy użyciu usługi **DB\_role usługi datawriteer** do aplikacji. Jest to typowy wzorzec obrony, który zapobiega zmienianiu schematu bazy danych przez potencjalnie złośliwy kod w aplikacji. (Na przykład może się to zdarzyć w przypadku pomyślnego ataku polegającego na iniekcji SQL). Te samouczki nie używają tego wzorca. Aby zaimplementować ten wzorzec w scenariuszu, wykonaj następujące kroki:

1. W kreatorze **publikowania sieci Web** na karcie **Ustawienia** wprowadź parametry połączenia określające użytkownika z uprawnieniami pełnego dostępu do schematu bazy danych. Wyczyść pole wyboru **Użyj tych parametrów połączenia w czasie wykonywania** . W wdrożonym pliku Web. config jest to `DatabasePublish` parametry połączenia.

2. Utwórz transformację pliku Web. config dla parametrów połączenia, które mają być używane przez aplikację w czasie wykonywania.

## <a name="summary"></a>Podsumowanie

Aplikacja została teraz wdrożona w usługach IIS na komputerze deweloperskim i przetestowana w tym miejscu.

![Strona główna w teście](deploying-to-iis/_static/image23.png)

Umożliwia to sprawdzenie, czy proces wdrożenia skopiował zawartość aplikacji do odpowiedniej lokalizacji (z wyłączeniem plików, które nie zostały wdrożone), a także Web Deploy skonfigurowanych usług IIS poprawnie podczas wdrażania. W następnym samouczku zostanie uruchomiony jeszcze jeden test, który znajdzie zadanie wdrożenia, które nie zostało jeszcze wykonane: Ustawianie uprawnień do folderu w folderze *Elm Ah* .

## <a name="more-information"></a>Więcej informacji

Aby uzyskać informacje na temat uruchamiania usług IIS lub IIS Express w programie Visual Studio, zobacz następujące zasoby:

- [IIS Express przegląd](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) w witrynie IIS.NET.
- [Wprowadzenie IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) na blogu Scott Guthrie.
- [Serwery sieci Web w programie Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Podstawowe różnice między usługami IIS i ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) w witrynie ASP.NET.

Aby uzyskać informacje o problemach, które mogą wystąpić, gdy aplikacja jest uruchamiana w średnim zaufaniu, zobacz [hosting aplikacji ASP.NET w średnim zaufaniu](http://www.4guysfromrolla.com/articles/100307-1.aspx) w czterech złoczyńców z witryny rolla.

> [!div class="step-by-step"]
> [Poprzednie](project-properties.md)
> [dalej](setting-folder-permissions.md)
