---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie w usługach IIS jako środowisko testowe — 5 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5d85232ff2cb229d771d517db7173721c9e277bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635129"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie w usługach IIS jako środowisko testowe — 5 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak wdrożyć aplikację sieci Web ASP.NET w usługach IIS na komputerze lokalnym.

Podczas opracowywania aplikacji zwykle testuje się przez uruchomienie go w programie Visual Studio. Domyślnie oznacza to, że używany jest serwer programistyczny programu Visual Studio (znany również jako Cassini). Serwer programistyczny programu Visual Studio ułatwia testowanie podczas opracowywania w programie Visual Studio, ale nie działa tak samo jak w przypadku usług IIS. W związku z tym jest możliwe, że aplikacja będzie działać poprawnie podczas testowania w programie Visual Studio, ale nie powiedzie się, gdy zostanie ona wdrożona w usługach IIS w środowisku hostingu.

Aplikacje można testować bardziej niezawodnie w następujący sposób:

1. Użyj IIS Express lub pełnych usług IIS zamiast serwera deweloperskiego programu Visual Studio podczas testowania w programie Visual Studio podczas opracowywania. Ta metoda zwykle emuluje bardziej precyzyjnie, w jaki sposób lokacja będzie działać w ramach usług IIS. Jednak ta metoda nie przetestuje procesu wdrażania lub sprawdza, czy wynik procesu wdrażania zostanie prawidłowo uruchomiony.
2. Wdróż aplikację w usługach IIS na komputerze deweloperskim, korzystając z tego samego procesu, który będzie używany później do wdrożenia go w środowisku produkcyjnym. Ta metoda sprawdza poprawność procesu wdrażania poza sprawdzeniem, czy aplikacja zostanie prawidłowo uruchomiona w ramach usług IIS.
3. Wdróż aplikację w środowisku testowym, która jest możliwie blisko środowiska produkcyjnego. Ze względu na to, że środowisko produkcyjne dla tych samouczków jest zewnętrznym dostawcą usług hostingowych, idealnym środowiskiem testowym będzie drugie konto z dostawcą hostingu. To drugie konto będzie używane tylko do testowania, ale można je skonfigurować w taki sam sposób, jak konto produkcyjne.

W tym samouczku przedstawiono kroki dla opcji 2. Wskazówki dotyczące opcji 3 są dostępne na końcu samouczka [wdrażanie do środowiska produkcyjnego](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) , a na końcu tego samouczka znajdują się linki do zasobów dla opcji 1.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Konfigurowanie aplikacji do uruchamiania w średnim zaufaniu

Przed zainstalowaniem i wdrożeniem usług IIS zmień ustawienia pliku Web. config w celu zapewnienia, że lokacja będzie działać bardziej jak w typowym udostępnionym środowisku hostingu.

Dostawcy hostingu zazwyczaj uruchamiają witrynę sieci Web w *średnim zaufaniu*, co oznacza, że niektóre rzeczy nie mogą być wykonywane. Na przykład kod aplikacji nie może uzyskać dostępu do rejestru systemu Windows i nie może odczytywać ani zapisywać plików znajdujących się poza hierarchią folderów aplikacji. Domyślnie aplikacja jest uruchamiana z *wysokim zaufaniem* na komputerze lokalnym, co oznacza, że aplikacja może być w stanie wykonywać operacje, które nie powiodą się podczas wdrażania go w środowisku produkcyjnym. W związku z tym, aby środowisko testowe było dokładniej odzwierciedla środowisko produkcyjne, skonfigurujesz aplikację do uruchamiania w średnim zaufaniu.

W pliku Web. config aplikacji Dodaj element **zaufania** do elementu **System. Web** , jak pokazano w tym przykładzie.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Aplikacja zostanie teraz uruchomiona w średnim zaufaniu w usługach IIS, nawet na komputerze lokalnym. To ustawienie umożliwia przechwycenie jak najszybciej, jak to możliwe, wszelkich prób spowodowanych przez kod aplikacji, aby wykonać coś, co nie powiedzie się w środowisku produkcyjnym.

> [!NOTE]
> Jeśli używasz migracje Code First platformy Entity Framework, upewnij się, że masz zainstalowaną wersję 5,0 lub nowszą. W Entity Framework w wersji 4,3 migracja wymaga pełnego zaufania, aby można było zaktualizować schemat bazy danych.

## <a name="installing-iis-and-web-deploy"></a>Instalowanie usług IIS i Web Deploy

Aby można było wdrożyć usługi IIS na komputerze deweloperskim, należy zainstalować usługi IIS i Web Deploy. Nie są one uwzględnione w domyślnej konfiguracji systemu Windows 7. Jeśli zainstalowano już zarówno usługi IIS, jak i Web Deploy, przejdź do następnej sekcji.

Korzystanie z [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx) jest preferowanym sposobem instalowania usług iis i Web Deploy, ponieważ Instalator platformy sieci Web instaluje zalecaną konfigurację dla usług IIS i automatycznie instaluje wymagania wstępne dla usług iis i Web Deploy w razie potrzeby.

Aby uruchomić Instalatora platformy sieci Web, aby zainstalować usługi IIS i Web Deploy, użyj poniższego linku. Jeśli masz już zainstalowane usługi IIS, Web Deploy lub dowolny z wymaganych składników, Instalator platformy sieci Web zainstaluje tylko te, co nie istnieje.

- [Instalowanie usług IIS i Web Deploy przy użyciu Instalatora WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Ustawianie domyślnej puli aplikacji na .NET 4

Po zainstalowaniu usług IIS Uruchom **Menedżera usług IIS** , aby upewnić się, że .NET Framework wersja 4 jest przypisana do domyślnej puli aplikacji.

Z menu **Start** systemu Windows wybierz polecenie **Uruchom**, wpisz "inetmgr", a następnie kliknij przycisk **OK**. (Jeśli polecenie **Uruchom** nie znajduje się w menu **Start** , możesz nacisnąć klawisz systemu Windows i R, aby go otworzyć. Lub kliknij prawym przyciskiem myszy pasek zadań, kliknij pozycję **Właściwości**, wybierz kartę **menu Start** , kliknij polecenie **Dostosuj**, a następnie wybierz pozycję **Uruchom polecenie**.

W okienku **połączenia** rozwiń węzeł serwera, a następnie wybierz pozycję **Pule aplikacji**. W okienku **Pule aplikacji** , jeśli do programu .NET Framework w wersji 4 zostanie przypisana **Domyślna** , jak pokazano na poniższej ilustracji, przejdź do następnej sekcji.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Jeśli widzisz tylko dwie pule aplikacji, a oba z nich są ustawione na .NET Framework 2,0, musisz zainstalować ASP.NET 4 w usługach IIS:

- Otwórz okno wiersza polecenia, klikając prawym przyciskiem myszy **wiersz polecenia** w menu **Start** systemu Windows i wybierając polecenie **Uruchom jako administrator**. Następnie uruchom polecenie [aspnet\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) , aby zainstalować ASP.NET 4 w usługach IIS przy użyciu następujących poleceń. (W systemach 64-bitowych Zastąp wartość "Framework" "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP. NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    To polecenie tworzy nowe pule aplikacji dla .NET Framework 4, ale domyślna pula aplikacji będzie nadal ustawiona na 2,0. Wdrażasz aplikację, która jest przeznaczona dla platformy .NET 4, do tej puli aplikacji, więc musisz zmienić pulę aplikacji na .NET 4.

Jeśli **Menedżer usług IIS**został zamknięty, uruchom go ponownie, rozwiń węzeł serwera, a następnie kliknij pozycję **Pule aplikacji** , aby ponownie wyświetlić okienko **Pule aplikacji** .

W okienku **Pule aplikacji** kliknij pozycję **Domyślna pula**, a następnie w okienku **Akcje** kliknij pozycję **Ustawienia podstawowe**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

W oknie dialogowym **Edytowanie puli aplikacji** zmień **wersję .NET Framework** na **.NET Framework v 4.0.30319** , a następnie kliknij przycisk **OK**.

[![Selecting_. NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Teraz można przystąpić do publikowania w usługach IIS.

## <a name="publishing-to-iis"></a>Publikowanie w usługach IIS

Istnieje kilka sposobów wdrażania przy użyciu programu Visual Studio 2010 i Web Deploy:

- Użyj programu Visual Studio, a następnie kliknij pozycję Publikuj.
- Utwórz *pakiet wdrożeniowy* i zainstaluj go przy użyciu interfejsu użytkownika Menedżera usług IIS. Pakiet wdrożeniowy składa się z pliku *zip* , który zawiera wszystkie pliki i metadane, które są konieczne do zainstalowania lokacji w usługach IIS.
- Utwórz pakiet wdrożeniowy i zainstaluj go przy użyciu wiersza polecenia.

Proces, w którym nastąpiło przechodzenie w poprzednich samouczkach, aby skonfigurować program Visual Studio do automatyzowania zadań wdrażania, ma zastosowanie do wszystkich tych trzech metod. W tych samouczkach użyjesz pierwszej z tych metod. Aby uzyskać informacje o korzystaniu z pakietów wdrożeniowych, zobacz [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx).

Przed opublikowaniem upewnij się, że program Visual Studio jest uruchomiony w trybie administratora. (W menu **Start** systemu Windows 7 kliknij prawym przyciskiem myszy ikonę używanej wersji programu Visual Studio, a następnie wybierz polecenie **Uruchom jako administrator**.) Tryb administratora jest wymagany do publikowania tylko w przypadku publikowania w usługach IIS na komputerze lokalnym.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity (a nie projekt CONTOSOUNIVERSITY. dal) i wybierz polecenie **Publikuj**.

Zostanie wyświetlony Kreator **publikowania w sieci Web** .

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Z listy rozwijanej wybierz pozycję **&lt;nowy...&gt;** .

W oknie dialogowym **Nowy profil** wprowadź "test", a następnie kliknij przycisk **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Ta nazwa jest taka sama jak w środkowym węźle utworzonego wcześniej pliku transformacji Web. test. config. Ta zgodność polega na tym, że przekształcenia Web. test. config mają być stosowane podczas publikowania przy użyciu tego profilu.

Kreator automatycznie przechodzi do karty **połączenie** .

W polu **adres URL usługi** wprowadź wartość *localhost*.

W polu **Witryna/aplikacja** wprowadź *domyślną witrynę sieci Web/ContosoUniversity*.

W polu **docelowy adres URL** wprowadź `http://localhost/ContosoUniversity`.

Ustawienie **docelowego adresu URL** nie jest wymagane. Gdy program Visual Studio zakończy wdrażanie aplikacji, automatycznie otwiera domyślną przeglądarkę na tym adresie URL. Jeśli nie chcesz, aby przeglądarka była otwierana automatycznie po wdrożeniu, pozostaw to pole puste.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Kliknij pozycję **Sprawdź poprawność połączenia** , aby sprawdzić, czy ustawienia są poprawne i czy można nawiązać połączenie z usługami IIS na komputerze lokalnym.

Zielony znacznik wyboru weryfikuje, czy połączenie zostało nawiązane pomyślnie.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Kliknij przycisk **dalej** , aby przejść do karty **Ustawienia** .

Pole listy rozwijanej **Konfiguracja** określa konfigurację kompilacji do wdrożenia. Wartość domyślna to Release (wersja), co jest potrzebne.

Pozostaw zaznaczone pole wyboru **Usuń dodatkowe pliki w miejscu docelowym** . Ponieważ jest to pierwsze wdrożenie, nie będzie jeszcze żadnych plików w folderze docelowym.

W sekcji **bazy danych** Wprowadź następującą wartość w polu Parametry połączenia dla **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Proces wdrażania umieści te parametry połączenia we wdrożonym pliku Web. config, ponieważ wybrane są **Parametry połączenia w czasie wykonywania** .

W obszarze **SchoolContext**wybierz pozycję **Zastosuj migracje Code First**. Ta opcja powoduje, że proces wdrażania skonfiguruje wdrożony plik Web. config, aby określić inicjator `MigrateDatabaseToLatestVersion`. Ten inicjator automatycznie aktualizuje bazę danych do najnowszej wersji, gdy aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu.

W polu ciąg połączenia dla **DefaultConnection**Wprowadź następującą wartość:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Pozostaw wyczyszczone **aktualizacje bazy danych** . Baza danych członkostwa zostanie wdrożona przez skopiowanie pliku. sdf w aplikacji\_dane i nie chcesz, aby proces wdrażania wykonał inne czynności z tą bazą danych.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Kliknij przycisk **dalej** , aby przejść do karty **wersja zapoznawcza** .

Na karcie **Podgląd** kliknij pozycję **Rozpocznij podgląd** , aby wyświetlić listę plików, które zostaną skopiowane.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Kliknij przycisk **Opublikuj**.

Jeśli program Visual Studio nie jest w trybie administratora, może zostać wyświetlony komunikat o błędzie z informacją o błędzie uprawnień. W takim przypadku Zamknij program Visual Studio, otwórz go w trybie administratora, a następnie ponów próbę opublikowania.

Jeśli program Visual Studio jest w trybie administratora, w oknie **danych wyjściowych** są raportowane pomyślne kompilacje i publikowanie.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Przeglądarka automatycznie otworzy stronę główną firmy Contoso University działającą w usługach IIS na komputerze lokalnym.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Testowanie w środowisku testowym

Należy zauważyć, że wskaźnik środowiska pokazuje "(test)" zamiast "(dev)", który pokazuje, że transformacja *pliku Web. config* dla wskaźnika środowiska zakończyła się pomyślnie.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Uruchom stronę **uczniów** , aby sprawdzić, czy wdrożona baza danych nie ma uczniów. Po wybraniu tej strony może upłynąć kilka minut, ponieważ Code First tworzy bazę danych, a następnie uruchamia metodę `Seed`. (Nie było to zrobione na stronie głównej, ponieważ aplikacja nie próbowała jeszcze uzyskać dostępu do bazy danych).

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Uruchom stronę **instruktorów** , aby sprawdzić, czy Code First wypełniania bazy danych za pomocą instruktora:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Wybierz pozycję **Dodaj uczniów** z menu **uczniów** , Dodaj ucznia, a następnie Wyświetl nowych uczniów na stronie **uczniów** , aby sprawdzić, czy można pomyślnie zapisać w bazie danych:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Z menu **kursy** wybierz pozycję **Aktualizuj kredyty**. Strona **kredyty aktualizacji** wymaga uprawnień administratora, więc zostanie wyświetlona strona **logowania** . Wprowadź utworzone wcześniej poświadczenia konta administratora ("admin" i "pas $ w0rd"). Zostanie wyświetlona strona **kredyty aktualizacji** , która sprawdza, czy konto administratora utworzone w poprzednim samouczku zostało prawidłowo wdrożone w środowisku testowym.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Sprawdź, czy istnieje folder *ELMAH* zawierający tylko plik zastępczy.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Przeglądanie automatycznych zmian w pliku Web. config dla Migracje Code First

Otwórz plik *Web. config* we wdrożonej aplikacji pod adresem *C:\inetpub\wwwroot\ContosoUniversity* i sprawdź, gdzie skonfigurowany proces wdrażania migracje Code First automatycznie aktualizować bazę danych do najnowszej wersji.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Proces wdrażania również utworzył nowe parametry połączenia dla Migracje Code First, które mają być używane wyłącznie do aktualizacji schematu bazy danych:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Te dodatkowe parametry połączenia umożliwiają określenie jednego konta użytkownika dla aktualizacji schematu bazy danych i innego konta użytkownika na potrzeby dostępu do danych aplikacji. Można na przykład przypisać rolę właściciela\_bazy danych do Migracje Code First, a usługi DB\_elementu DataReader i bazy danych\_ról do aplikacji. Jest to typowy wzorzec obrony, który zapobiega zmienianiu schematu bazy danych przez potencjalnie złośliwy kod w aplikacji. (Na przykład może się to zdarzyć w przypadku pomyślnego ataku polegającego na iniekcji SQL). Ten wzorzec nie jest używany przez te samouczki. Nie ma zastosowania do SQL Server Compact i nie ma zastosowania w przypadku migrowania do SQL Server w kolejnym samouczku w tej serii. Witryna Cytanium oferuje tylko jedno konto użytkownika do uzyskiwania dostępu do bazy danych SQL Server utworzonej w witrynie Cytanium. Jeśli możesz zaimplementować ten wzorzec w scenariuszu, możesz to zrobić, wykonując następujące czynności:

1. Na karcie **Ustawienia** Kreatora publikacji w **sieci Web** wprowadź parametry połączenia określające użytkownika z uprawnieniami pełnej aktualizacji schematu bazy danych, a następnie wyczyść pole wyboru **Użyj tych parametrów połączenia w czasie wykonywania** . W wdrożonym pliku Web. config jest to `DatabasePublish` parametry połączenia.
2. Utwórz transformację pliku Web. config dla parametrów połączenia, które mają być używane przez aplikację w czasie wykonywania.

Aplikacja została wdrożona w usługach IIS na komputerze deweloperskim i przetestowana w tym miejscu. Umożliwia to sprawdzenie, czy proces wdrożenia skopiował zawartość aplikacji do odpowiedniej lokalizacji (z wyłączeniem plików, które nie zostały wdrożone), a także Web Deploy skonfigurowanych usług IIS poprawnie podczas wdrażania. W następnym samouczku zostanie uruchomiony jeszcze jeden test, który znajdzie zadanie wdrożenia, które nie zostało jeszcze wykonane: Ustawianie uprawnień do folderu w folderze *ELMAH* .

## <a name="more-information"></a>Więcej informacji

Aby uzyskać informacje na temat uruchamiania usług IIS lub IIS Express w programie Visual Studio, zobacz następujące zasoby:

- [IIS Express przegląd](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) w witrynie IIS.NET.
- [Wprowadzenie IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) na blogu Scott Guthrie.
- [Instrukcje: Określanie serwera sieci Web dla projektów sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Podstawowe różnice między usługami IIS i ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) w witrynie ASP.NET.
- [Przetestuj aplikację ASP.NET MVC lub Web Forms w usługach IIS 7 w ciągu 30 sekund](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) na blogu Rick Anderson. Ten wpis zawiera przykłady dlaczego testowanie za pomocą programu Visual Studio Development Server (Cassini) nie jest tak niezawodne jak testowanie w IIS Express i dlaczego testowanie w IIS Express nie jest tak niezawodne jak testowanie w usługach IIS.

Aby uzyskać informacje o problemach, które mogą wystąpić, gdy aplikacja jest uruchamiana w średnim zaufaniu, zobacz [hosting aplikacji ASP.NET w średnim zaufaniu](http://www.4guysfromrolla.com/articles/100307-1.aspx) w 4 złoczyńców z witryny rolla.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [dalej](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
