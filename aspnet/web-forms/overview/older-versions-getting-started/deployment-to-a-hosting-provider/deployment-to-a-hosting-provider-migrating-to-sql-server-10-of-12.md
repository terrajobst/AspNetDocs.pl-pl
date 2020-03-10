---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: Migrowanie do SQL Server-10 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: c5281a42596d95e725b32e652c75785abe0fd64e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573466"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: Migrowanie do SQL Server-10 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak przeprowadzić migrację z SQL Server Compact do SQL Server. Jednym z powodów, które warto wykonać, jest skorzystanie z funkcji SQL Server, które nie są obsługiwane przez SQL Server Compact, takich jak procedury składowane, wyzwalacze, widoki lub replikacja. Aby uzyskać więcej informacji o różnicach między SQL Server Compact i SQL Server, zobacz samouczek [wdrażania SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express a pełny SQL Server do programowania

Po podjęciu decyzji o przeprowadzeniu uaktualnienia do SQL Server warto użyć SQL Server lub SQL Server Express w środowiskach deweloperskich i testowych. Oprócz różnic w obsłudze narzędzi i funkcji aparatu bazy danych istnieją różnice w implementacji dostawców między SQL Server Compact i innymi wersjami SQL Server. Różnice te mogą spowodować, że ten sam kod generuje różne wyniki. W związku z tym, jeśli zdecydujesz się na zachowanie SQL Server Compact jako bazy danych programistycznych, należy dokładnie przetestować lokację w SQL Server lub SQL Server Express w środowisku testowym przed każdym wdrożeniem do produkcji.

W przeciwieństwie do SQL Server Compact, SQL Server Express to ten sam aparat bazy danych i używa tego samego dostawcy platformy .NET jak pełnych SQL Server. Podczas testowania za pomocą SQL Server Express można mieć pewność, że uzyskasz te same wyniki, co w przypadku SQL Server. Większość z tych samych narzędzi bazy danych może być używana z SQL Server Express, których można używać z SQL Server (istotny wyjątek [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)) i obsługuje inne funkcje SQL Server, takie jak procedury składowane, widoki, wyzwalacze i replikacja. (Zazwyczaj należy używać pełnych SQL Server w produkcyjnej witrynie sieci Web. SQL Server Express można uruchamiać w udostępnionym środowisku hostingu, ale nie została ona zaprojektowana dla tego programu, a wielu dostawców hostingu nie obsługuje tej funkcji.

W przypadku korzystania z programu Visual Studio 2012 zwykle wybiera się SQL Server Express LocalDB dla środowiska deweloperskiego, ponieważ jest to co jest instalowane domyślnie z programem Visual Studio. Jednak LocalDB nie działa w usługach IIS, dlatego dla środowiska testowego należy użyć SQL Server lub SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Łączenie baz danych w zależności od ich oddzielenia

Aplikacja firmy Contoso University ma dwie SQL Server Compact bazy danych: bazę danych członkostwa (*ASPNET. sdf*) i bazę danych aplikacji (*szkoła. sdf*). Podczas migracji można migrować te bazy danych do dwóch oddzielnych baz danych lub do pojedynczej bazy danych. Warto połączyć je w celu ułatwienia sprzężenia bazy danych między bazą danych aplikacji i bazą danych członkostwa. Twój plan hostingu może również stanowić powód, aby połączyć się z nimi. Na przykład dostawca hostingu może obciążać więcej dla wielu baz danych lub nawet nie zezwalać na więcej niż jedną bazę danych. Jest to przypadek z kontem hostingu usługi Cytanium w wersji Lite używanym w ramach tego samouczka, który umożliwia tylko jedną bazę danych SQL Server.

W tym samouczku przeprowadzisz migrację dwóch baz danych w następujący sposób:

- Migruj do dwóch LocalDB baz danych w środowisku deweloperskim.
- Przeprowadź migrację do dwóch baz danych SQL Server Express w środowisku testowym.
- Przeprowadź migrację do jednej połączonej pełnej bazy danych SQL Server w środowisku produkcyjnym.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalowanie SQL Server Express

Program SQL Server Express jest automatycznie instalowany domyślnie z programem Visual Studio 2010, ale domyślnie nie jest instalowany z programem Visual Studio 2012. Aby zainstalować SQL Server 2012 Express, kliknij poniższy link

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Wybierz pozycję *plk/x64/sqlexpr\_x64\_plk. exe* lub *plk/x86/sqlexpr\_x86\_plk. exe*, a w Kreatorze instalacji zaakceptuj ustawienia domyślne. Aby uzyskać więcej informacji na temat opcji instalacji, zobacz [instalowanie SQL Server 2012 w Kreatorze instalacji (Instalator)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Tworzenie SQL Server Express baz danych dla środowiska testowego

Następnym krokiem jest utworzenie bazy danych ASP.NET i szkoły.

Z menu **Widok** wybierz pozycję **Eksplorator serwera** (**Eksplorator bazy danych** w Visual Web Developer), a następnie kliknij prawym przyciskiem myszy pozycję **połączenia danych** i wybierz polecenie **Utwórz nową SQL Server bazę danych**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

W oknie dialogowym **Utwórz nową bazę danych SQL Server** wprowadź wartość ".\SQLEXPRESS" w polu **Nazwa serwera** i "ASPNET-test" w polu **Nazwa nowej bazy danych** , a następnie kliknij przycisk **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Postępuj zgodnie z tą samą procedurą, aby utworzyć nową bazę danych szkoły SQL Server Express o nazwie "Szkoła-test".

(Do tych nazw baz danych są dołączane "testy", ponieważ w przyszłości utworzysz dodatkowe wystąpienie każdej bazy danych dla środowiska programistycznego i musisz mieć możliwość odróżnienia dwóch zestawów baz danych).

**Eksplorator serwera** teraz pokazuje dwie nowe bazy danych.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Tworzenie skryptu dotacji dla nowych baz danych

Gdy aplikacja jest uruchamiana w usługach IIS na komputerze deweloperskim, aplikacja uzyskuje dostęp do bazy danych przy użyciu poświadczeń domyślnej puli aplikacji. Jednak domyślnie tożsamość puli aplikacji nie ma uprawnień do otwierania baz danych. Aby udzielić tego uprawnienia, należy uruchomić skrypt. W tej sekcji utworzysz skrypt, który zostanie uruchomiony później, aby upewnić się, że aplikacja może otworzyć bazy danych w programie IIS.

W folderze *SolutionFiles* rozwiązania, który został utworzony w samouczku [wdrażanie do środowiska produkcyjnego](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) , Utwórz nowy plik SQL o nazwie *Grant. SQL*. Skopiuj następujące polecenia SQL do pliku, a następnie Zapisz i zamknij plik:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Ten skrypt jest przeznaczony do pracy z SQL Server 2008 i z ustawieniami usług IIS w systemie Windows 7, zgodnie z opisem w tym samouczku. Jeśli używasz innej wersji SQL Server lub systemu Windows lub jeśli program IIS zostanie skonfigurowany na komputerze inaczej, może być wymagane wprowadzenie zmian w tym skrypcie. Aby uzyskać więcej informacji na temat skryptów SQL Server, zobacz [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** Ten skrypt daje użytkownikowi\_uprawnienia właściciela bazy danych, który uzyskuje dostęp do tej samej bazie w czasie wykonywania, czyli w środowisku produkcyjnym. W niektórych scenariuszach możesz określić użytkownika, który ma pełne uprawnienia do aktualizacji schematu bazy danych tylko do wdrożenia, i określić dla czasu wykonywania inny użytkownik, który ma uprawnienia tylko do odczytu i zapisu danych. Aby uzyskać więcej informacji, zobacz **przeglądanie automatycznych zmian w pliku Web. config dla migracje Code First** w [ramach wdrażania do usług IIS jako środowiska testowego](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).

## <a name="configuring-database-deployment-for-the-test-environment"></a>Konfigurowanie wdrażania bazy danych dla środowiska testowego

Następnie skonfigurujesz program Visual Studio tak, aby wykonał następujące zadania dla każdej bazy danych:

- Wygeneruj skrypt SQL, który tworzy strukturę źródłowej bazy danych (tabele, kolumny, ograniczenia itp.) w docelowej bazie danych.
- Wygeneruj skrypt SQL, który wstawia dane źródłowej bazy danych do tabel w docelowej bazie danych.
- Uruchom wygenerowane skrypty i utworzony skrypt przydzielenia w docelowej bazie danych.

Otwórz okno **właściwości projektu** i wybierz kartę **pakowanie/publikowanie SQL** .

Upewnij się, że na liście rozwijanej **Konfiguracja** została wybrana wartość **Active (wersja)** lub **Zwolnij** .

Kliknij pozycję **Włącz tę stronę**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

Karta **pakiet/publikowanie SQL** jest zwykle wyłączona, ponieważ określa starszą metodę wdrażania. W przypadku większości scenariuszy należy skonfigurować wdrażanie bazy danych w kreatorze **publikowania w sieci Web** . Migracja z SQL Server Compact do SQL Server lub SQL Server Express jest szczególnym przypadkiem, dla którego ta metoda jest dobrym rozwiązaniem.

Kliknij pozycję **Importuj z pliku Web. config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Program Visual Studio szuka parametrów połączenia w pliku *Web. config* , znajduje jeden dla bazy danych członkostwa i jeden dla bazy danych szkoły i dodaje wiersz odpowiadający poszczególnym parametrom połączenia w tabeli **wpisów bazy danych** . Znalezione parametry połączenia są dla istniejących SQL Server Compact baz danych, a następnym krokiem będzie skonfigurowanie sposobu i miejsca wdrożenia tych baz danych.

W sekcji **szczegóły wpisu bazy** danych wprowadź ustawienia wdrażania bazy danych poniżej tabeli **wpisy** bazy danych. Ustawienia wyświetlane w sekcji **szczegóły wpisu bazy danych** dotyczą dowolnego wiersza w tabeli **wpisy bazy danych** , jak pokazano na poniższej ilustracji.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurowanie ustawień wdrożenia dla bazy danych członkostwa

Wybierz wiersz **DefaultConnection-Deployment** w tabeli **wpisy bazy danych** , aby skonfigurować ustawienia, które mają zastosowanie do bazy danych członkostwa.

W polu **Parametry połączenia dla docelowej bazy danych**wprowadź parametry połączenia wskazujące nową bazę danych członkostwa SQL Server Express. Parametry połączenia, które są potrzebne, można uzyskać z **Eksplorator serwera**. W **Eksplorator serwera**rozwiń węzeł **połączenia danych** i wybierz bazę danych **aspnetTest** , a następnie w oknie **Właściwości** skopiuj wartość **Parametry połączenia** .

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Te same parametry połączenia są odtwarzane tutaj:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Skopiuj i wklej te parametry połączenia do **parametrów połączenia dla docelowej bazy danych** na karcie **pakowanie/publikowanie SQL** .

Upewnij się, że wybrano **dane i/lub schemat z istniejącej bazy danych** . Powoduje to, że skrypty SQL mają być automatycznie generowane i uruchamiane w docelowej bazie danych.

**Parametry połączenia dla wartości źródłowej bazy danych** są wyodrębniane z pliku *Web. config* i wskazują na bazę danych SQL Server Compact deweloperskich. Jest to źródłowa baza danych, która zostanie użyta do wygenerowania skryptów, które będą uruchamiane później w docelowej bazie danych. Ponieważ chcesz wdrożyć wersję produkcyjną bazy danych, Zmień wartość "aspnet-Dev. sdf" na "aspnet-Prod. sdf".

Zmień **Opcje skryptów bazy danych** ze **schematu tylko** na **schemat i dane**, ponieważ chcesz skopiować dane (konta i role użytkowników), a także strukturę bazy danych.

Aby skonfigurować wdrożenie do uruchamiania utworzonych wcześniej skryptów dotacji, należy dodać je do sekcji **skrypty bazy danych** . Kliknij przycisk **Dodaj skrypt**, a następnie w oknie dialogowym **Dodawanie skryptów SQL** przejdź do folderu, w którym zapisano skrypt Grant (jest to folder zawierający plik rozwiązania). Wybierz plik o nazwie *Grant. SQL*, a następnie kliknij przycisk **Otwórz**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Ustawienia wiersza **DefaultConnection-Deployment** w **wpisach bazy danych** wyglądają teraz tak, jak na poniższej ilustracji:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurowanie ustawień wdrożenia dla bazy danych szkoły

Następnie wybierz wiersz **SchoolContext-Deployment** w tabeli **wpisy bazy danych** w celu skonfigurowania ustawień wdrożenia dla bazy danych szkoły.

Możesz użyć tej samej metody, która została wcześniej użyta w celu uzyskania parametrów połączenia dla nowej bazy danych SQL Server Express. Skopiuj te parametry połączenia do **parametrów połączenia dla docelowej bazy danych** na karcie **pakowanie/publikowanie SQL** .

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Upewnij się, że wybrano **dane i/lub schemat z istniejącej bazy danych** .

**Parametry połączenia dla wartości źródłowej bazy danych** są wyodrębniane z pliku *Web. config* i wskazują na bazę danych SQL Server Compact deweloperskich. Zmień wartość "School-Dev. sdf" na "School-Prod. sdf", aby wdrożyć wersję produkcyjną bazy danych. (Nigdy nie utworzono pliku School-Prod. sdf w folderze danych\_aplikacji, więc skopiujesz go ze środowiska testowego do folderu dane\_aplikacji w folderze projektu ContosoUniversity).

Zmień **Opcje obsługi skryptów bazy danych** na **schemat i dane**.

Należy również uruchomić skrypt, aby przyznać uprawnienia do odczytu i zapisu dla tej bazy danych w tożsamości puli aplikacji, więc Dodaj plik skryptu *Grant. SQL* tak, jak w przypadku bazy danych członkostwa.

Po zakończeniu ustawienia dla wiersza **SchoolContext-Deployment** w **pozycji bazy danych** wyglądają podobnie jak na poniższej ilustracji:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Zapisz zmiany na karcie **pakowanie/publikowanie SQL** .

Skopiuj plik *School-prod. sdf* z folderu *danych c:\inetpub\wwwroot\ContosoUniversity\App\_* do folderu *\_danych aplikacji* w projekcie ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Określanie trybu transakcyjnego dla skryptu dotacji

Proces wdrażania generuje skrypty, które wdrażają schemat bazy danych i dane. Domyślnie te skrypty są uruchamiane w transakcji. Jednak skrypty niestandardowe (takie jak skrypty grantu) domyślnie nie są uruchamiane w transakcji. Jeśli proces wdrażania miesza tryby transakcji, może wystąpić błąd limitu czasu, gdy skrypty są uruchamiane podczas wdrażania. W tej sekcji edytujesz plik projektu w celu skonfigurowania skryptów niestandardowych do uruchamiania w transakcji.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **ContosoUniversity** i wybierz polecenie **Zwolnij projekt**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Następnie ponownie kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj ContosoUniversity. csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Edytor programu Visual Studio pokazuje zawartość XML pliku projektu. Zwróć uwagę, że istnieje kilka `PropertyGroup` elementów. (W obrazie zawartość `PropertyGroup` elementów została pominięta.)

![Okno edytora plików projektu](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Pierwszy, który nie ma `Condition` atrybutu, jest przeznaczony dla ustawień, które są stosowane niezależnie od konfiguracji kompilacji. Jeden `PropertyGroup` element ma zastosowanie tylko do konfiguracji kompilacji debugowania (należy zauważyć, że atrybut `Condition`), jeden ma zastosowanie tylko do konfiguracji kompilacji wydania, a jeden ma zastosowanie tylko do konfiguracji kompilacji testowej. W ramach elementu `PropertyGroup` w konfiguracji kompilacji wydania zostanie wyświetlony element `PublishDatabaseSettings`, który zawiera ustawienia wprowadzone na karcie **pakowanie/publikowanie SQL** . Istnieje element `Object`, który odpowiada każdemu z określonych skryptów grantu (należy zauważyć dwa wystąpienia "Grant. SQL"). Domyślnie atrybut `Transacted` elementu `Source` dla każdego skryptu przydzielenia jest `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Zmień wartość atrybutu `Transacted` elementu `Source` na `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Zapisz i zamknij plik projektu, a następnie kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** a następnie wybierz polecenie **Załaduj ponownie projekt**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Konfigurowanie transformacji Web. config dla parametrów połączenia

Parametry połączenia dla nowych baz danych SQL Express wprowadzonych na karcie **pakiet/publikowanie SQL** są używane przez Web Deploy tylko do aktualizowania docelowej bazy danych podczas wdrażania. Nadal trzeba skonfigurować przekształcenia *Web. config* , tak aby parametry połączenia we wdrożonym pliku *Web. config* wskazywały nowe bazy danych SQL Server Express. (W przypadku używania karty **pakowanie/publikowanie SQL** nie można skonfigurować parametrów połączenia w profilu publikowania).

Otwórz *plik Web. test. config* i Zastąp element `connectionStrings` elementem `connectionStrings` w poniższym przykładzie. (Upewnij się, że kopiujesz tylko element connectionStrings, a nie otaczający kod, który jest wyświetlany w tym miejscu, aby zapewnić kontekst).

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Ten kod powoduje, że atrybuty `connectionString` i `providerName` każdego elementu `add` zostaną zastąpione we wdrożonym pliku *Web. config* . Te parametry połączenia nie są identyczne z tymi, które zostały wprowadzone w zakładce **pakowanie/publikowanie SQL** . Ustawienie "MultipleActiveResultSets = true" zostało dodane do nich, ponieważ jest wymagane dla Entity Framework i dostawców uniwersalnych.

## <a name="installing-sql-server-compact"></a>Instalowanie SQL Server Compact

Pakiet NuGet SqlServerCompact zapewnia zestawy aparatów bazy danych SQL Server Compact dla aplikacji firmy Contoso University. Ale teraz nie jest to aplikacja, ale Web Deploy, która musi mieć możliwość odczytywania SQL Server Compact baz danych, aby można było uruchamiać skrypty w SQL Server bazach danych. Aby umożliwić Web Deploy odczytywanie SQL Server Compact baz danych, zainstaluj SQL Server Compact na komputerze deweloperskim, używając następującego linku: [Microsoft SQL Server Compact 4,0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Wdrażanie w środowisku testowym

Aby można było publikować w środowisku testowym, należy utworzyć profil publikowania skonfigurowany do korzystania z karty **pakiet/publikowanie SQL** na potrzeby publikowania bazy danych zamiast ustawień bazy danych profilu publikowania.

Najpierw usuń istniejący profil testu.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij pozycję **Publikuj**.

Wybierz kartę **profil** .

Kliknij pozycję **Zarządzaj profilami**.

Wybierz pozycję **test**, kliknij pozycję **Usuń**, a następnie kliknij przycisk **Zamknij**.

Zamknij kreatora **publikacji w sieci Web** , aby zapisać tę zmianę.

Następnie utwórz nowy profil testu i użyj go do opublikowania projektu.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij pozycję **Publikuj**.

Wybierz kartę **profil** .

Wybierz pozycję **&lt;nowy...&gt;** z listy rozwijanej, a następnie wprowadź "test" jako nazwę profilu.

W polu **adres URL usługi** wprowadź wartość *localhost*.

W polu **Witryna/aplikacja** wprowadź *domyślną witrynę sieci Web/ContosoUniversity*.

W polu **docelowy adres URL** wprowadź `http://localhost/ContosoUniversity/`.

Kliknij przycisk **Dalej**.

Na karcie **Ustawienia** zostanie wyświetlone ostrzeżenie, że została skonfigurowana karta **pakowanie/publikowanie SQL** i umożliwia ona przesłonięcie ich przez kliknięcie pozycji Włącz nowe ulepszenia publikowania bazy danych. W przypadku tego wdrożenia nie chcesz przesłonić ustawień na karcie **pakiet/publikacja SQL** , więc kliknij przycisk **dalej**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Komunikat na karcie **Podgląd** wskazuje, że **nie wybrano żadnych baz danych do opublikowania**, ale oznacza to, że publikowanie bazy danych nie jest skonfigurowane w profilu publikowania.

Kliknij przycisk **Opublikuj**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Program Visual Studio wdroży aplikację i otworzy przeglądarkę na stronie głównej witryny w środowisku testowym. Uruchom stronę instruktorów, aby zobaczyć, że są wyświetlane te same dane, które zostały wcześniej wyświetlone. Uruchom stronę **Dodawanie uczniów** , Dodaj nowego ucznia, a następnie Wyświetl nowych uczniów na stronie **uczniów** . Spowoduje to sprawdzenie, czy można zaktualizować bazę danych. Wybierz stronę **Aktualizowanie kredytów** (musisz się zalogować), aby sprawdzić, czy baza danych członkostwa została wdrożona i czy masz do niej dostęp.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Tworzenie bazy danych SQL Server dla środowiska produkcyjnego

Teraz, gdy wdrożono go w środowisku testowym, można przystąpić do konfigurowania wdrożenia do produkcji. Począwszy od środowiska testowego, należy utworzyć bazę danych do wdrożenia. W miarę odwoływania się z przeglądu plan hostingu Cytanium w języku Lite zezwala tylko na pojedynczą bazę danych SQL Server, więc zostanie skonfigurowana tylko jedna baza danych, a nie dwie. Wszystkie tabele i dane z baz danych SQL Server Compact członkostwa i szkoły zostaną wdrożone w jednej SQL Server bazie danych w środowisku produkcyjnym.

Przejdź do panelu sterowania Cytanium w [http://panel.cytanium.com](http://panel.cytanium.com). Przytrzymaj mysz nad **bazami danych** , a następnie kliknij **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

Na stronie **SQL Server 2008** kliknij pozycję **Utwórz bazę danych**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nazwij bazę danych "Szkoła" i kliknij przycisk **Zapisz**. (Strona automatycznie dodaje prefiks "contosou", więc rzeczywista nazwa to "contosouSchool").

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Na tej samej stronie kliknij pozycję **Utwórz użytkownika**. Na serwerach Cytanium zamiast korzystania ze zintegrowanych zabezpieczeń systemu Windows i umożliwiających tożsamość puli aplikacji otwieranie bazy danych, utworzysz użytkownika, który ma uprawnienia do otwierania bazy danych. Poświadczenia użytkownika zostaną dodane do parametrów połączenia, które przechodzą w produkcyjny plik *Web. config* . W tym kroku utworzysz te poświadczenia.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Wypełnij pola wymagane na stronie **właściwości użytkownika SQL** :

- Wprowadź wartość "ContosoUniversityUser" jako nazwę.
- Wprowadź hasło.
- Wybierz **contosouSchool** jako domyślną bazę danych.
- Zaznacz pole wyboru **contosouSchool** .

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Konfigurowanie wdrażania bazy danych dla środowiska produkcyjnego

Teraz możesz przystąpić do konfigurowania ustawień wdrażania bazy danych na karcie **pakowanie/publikowanie SQL** , tak jak wcześniej dla środowiska testowego.

Otwórz okno **właściwości projektu** , wybierz kartę **pakowanie/publikowanie SQL** i upewnij się, że na liście rozwijanej **Konfiguracja** wybrano pozycję **Active (wersja)** lub **Zwolnij** .

W przypadku konfigurowania ustawień wdrożenia dla każdej bazy danych, kluczową różnicą między działaniem w środowisku produkcyjnym i testowym jest konfiguracja parametrów połączenia. Dla środowiska testowego wprowadzono różne parametry połączenia z bazą danych docelowych, ale dla środowiska produkcyjnego parametry połączenia docelowego będą takie same dla obu baz danych. Wynika to z faktu, że wdrażasz obie bazy danych w jednej bazie danych w środowisku produkcyjnym.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurowanie ustawień wdrożenia dla bazy danych członkostwa

Aby skonfigurować ustawienia, które mają zastosowanie do bazy danych członkostwa, wybierz wiersz **DefaultConnection-Deployment** w tabeli **wpisy bazy danych** .

W polu **Parametry połączenia dla docelowej bazy danych**wprowadź parametry połączenia wskazujące nową utworzoną SQL Server produkcyjnej bazy danych. Parametry połączenia można uzyskać z powitalnej wiadomości e-mail. Odpowiednia część wiadomości e-mail zawiera następujące przykładowe parametry połączenia:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Po zamianie trzech zmiennych, potrzebne parametry połączenia wyglądają następująco:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Skopiuj i wklej te parametry połączenia do **parametrów połączenia dla docelowej bazy danych** na karcie **pakowanie/publikowanie SQL** .

Upewnij się, że **dane ściągania i/lub schemat z istniejącej bazy danych** są nadal zaznaczone, a **Opcje skryptów bazy danych** nadal są **schematem i danymi**.

W polu **skrypty bazy danych** wyczyść pole wyboru obok skryptu Grant. SQL.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurowanie ustawień wdrożenia dla bazy danych szkoły

Następnie wybierz wiersz **SchoolContext-Deployment** w tabeli **wpisy bazy danych** w celu skonfigurowania ustawień bazy danych szkoły.

Skopiuj te same parametry połączenia do **parametrów połączenia dla docelowej bazy danych** , która została skopiowana do tego pola dla bazy danych członkostwa.

Upewnij się, że **dane ściągania i/lub schemat z istniejącej bazy danych** są nadal zaznaczone, a **Opcje skryptów bazy danych** nadal są **schematem i danymi**.

W polu **skrypty bazy danych** wyczyść pole wyboru obok skryptu Grant. SQL.

Zapisz zmiany na karcie **pakowanie/publikowanie SQL** .

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Konfigurowanie transformacji Web. config dla parametrów połączenia do produkcyjnych baz danych

Następnie skonfigurujesz przekształcenia *Web. config* , tak aby parametry połączenia we wdrożonym pliku *Web. config* wskazywały nową produkcyjną bazę danych. Parametry połączenia wprowadzone na karcie **pakiet/Publikuj SQL** na potrzeby Web Deploy są takie same, jak w przypadku tej aplikacji, z wyjątkiem dodania opcji `MultipleResultSets`.

Otwórz *plik Web. Product. config* i Zastąp element `connectionStrings` elementem `connectionStrings`, który wygląda podobnie do poniższego przykładu. (Skopiuj tylko `connectionStrings` element, a nie otaczające znaczniki, które są podane w celu wyświetlenia kontekstu).

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Czasami widzisz porady z informacją, że należy zawsze szyfrować parametry połączenia w pliku *Web. config* . Może to być odpowiednie w przypadku wdrożenia na serwerach w sieci firmowej. W przypadku wdrażania w udostępnionym środowisku hostingu, chociaż użytkownik ufa praktykom dotyczącym zabezpieczeń dostawcy hostingu i nie jest konieczne ani praktyczne do szyfrowania parametrów połączenia.

## <a name="deploying-to-the-production-environment"></a>Wdrażanie w środowisku produkcyjnym

Teraz wszystko jest gotowe do wdrożenia w środowisku produkcyjnym. Web Deploy odczyta SQL Server Compact bazy danych w aplikacji projektu\_folderze *danych* i ponownie utworzysz wszystkie tabele i dane w produkcyjnej SQL Server bazie danych. Aby można było opublikować program przy użyciu ustawień karty **sieci Web pakowanie/publikowanie** , należy utworzyć nowy profil publikowania dla środowiska produkcyjnego.

Najpierw usuń istniejący profil produkcyjny zgodnie z wcześniejszym profilem testowym.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij pozycję **Publikuj**.

Wybierz kartę **profil** .

Kliknij pozycję **Zarządzaj profilami**.

Wybierz pozycję **produkcja**, kliknij pozycję **Usuń**, a następnie kliknij przycisk **Zamknij**.

Zamknij kreatora **publikacji w sieci Web** , aby zapisać tę zmianę.

Następnie utwórz nowy profil produkcyjny i użyj go do opublikowania projektu.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij pozycję **Publikuj**.

Wybierz kartę **profil** .

Kliknij przycisk **Importuj**, a następnie wybierz pobrany wcześniej plik. publishsettings.

Na karcie **połączenie** Zmień **docelowy adres URL** na prawidłowy tymczasowy adres URL, który w tym przykładzie jest http://contosouniversity.com.vserver01.cytanium.com.

Zmień nazwę profilu na produkcyjny. (Wybierz kartę **profil** , a następnie kliknij pozycję **Zarządzaj profilami** , aby to zrobić).

Zamknij kreatora **publikacji w sieci Web** , aby zapisać zmiany.

W prawdziwej aplikacji, w której baza danych była aktualizowana w środowisku produkcyjnym, przed opublikowaniem należy wykonać dwa dodatkowe kroki:

1. Przekaż *aplikację\_offline. htm*, jak pokazano w samouczku [wdrażanie do środowiska produkcyjnego](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) .
2. Użyj funkcji **Menedżer plików** w panelu sterowania Cytanium, aby skopiować pliki *ASPNET-prod. sdf* i *School-prod. sdf* z lokacji produkcyjnej do folderu *danych\_aplikacji* w projekcie ContosoUniversity. Dzięki temu dane wdrażane do nowej bazy danych SQL Server zawierają najnowsze aktualizacje wprowadzone przez produkcyjną witrynę sieci Web.

Na pasku narzędzi **kliknij przycisk Publikuj w sieci Web** , upewnij się, że wybrano pozycję Profil **produkcyjny** , a następnie kliknij pozycję **Publikuj**.

Jeśli aplikacja została przekazana <em>\_offline. htm</em> przed opublikowaniem, musisz użyć narzędzia <strong>Menedżer plików</strong> w panelu sterowania Cytanium, aby usunąć <em>aplikację\_offline.</em> htm przed przetestem. Możesz również w tym samym czasie usunąć pliki <em>. sdf</em> z folderu <em>danych\_aplikacji</em> .

Teraz możesz otworzyć przeglądarkę i przejść do adresu URL witryny publicznej w celu przetestowania aplikacji w taki sam sposób, jak po wdrożeniu w środowisku testowym.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Przełączanie do SQL Server Express LocalDB w trakcie opracowywania

Jak wyjaśniono w przeglądzie, najlepszym rozwiązaniem jest użycie tego samego aparatu bazy danych w środowisku programistycznym, który jest używany w testach i produkcyjnych. (Należy pamiętać, że zalety korzystania z SQL Server Express podczas opracowywania polega na tym, że baza danych będzie działała tak samo w środowiskach programistycznych, testowych i produkcyjnych). W tej sekcji skonfigurujesz projekt ContosoUniversity do użycia SQL Server Express LocalDB podczas uruchamiania aplikacji z programu Visual Studio.

Najprostszym sposobem przeprowadzenia migracji jest umożliwienie Code First i systemu członkostwa w tworzeniu nowych baz danych programistycznych. Zastosowanie tej metody do migracji wymaga wykonania trzech kroków:

1. Zmień parametry połączenia, aby określić nowe bazy danych SQL Express LocalDB.
2. Uruchom narzędzie do administrowania witryną sieci Web, aby utworzyć użytkownika administratora. Spowoduje to utworzenie bazy danych członkostwa.
3. Za pomocą polecenia Migracje Code First Update-Database Utwórz i wypełnianie bazy danych aplikacji.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aktualizowanie parametrów połączenia w pliku Web. config

Otwórz plik *Web. config* i zastąp `connectionStrings` element następującym kodem:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Tworzenie bazy danych członkostwa

W **Eksplorator rozwiązań**wybierz projekt ContosoUniversity, a następnie kliknij pozycję **Konfiguracja ASP.NET** w menu **projekt** .

Wybierz kartę Zabezpieczenia.

Kliknij przycisk **Utwórz role lub zarządzaj nimi**, a następnie Utwórz rolę **administratora** .

Wróć do karty zabezpieczenia.

Kliknij przycisk **Utwórz użytkownika**, a następnie zaznacz pole wyboru **administrator** i Utwórz użytkownika o nazwie admin.

Zamknij **Narzędzie do administrowania witryną sieci Web**.

### <a name="creating-the-school-database"></a>Tworzenie bazy danych szkoły

Otwórz okno konsoli Menedżera pakietów.

Z listy rozwijanej **Projekt domyślny** wybierz projekt CONTOSOUNIVERSITY. dal.

Wprowadź następujące polecenie:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migracje Code First stosuje migrację początkową, która tworzy bazę danych, a następnie stosuje migrację DataUrodzenia, a następnie uruchamia metodę inicjatora.

Uruchom witrynę, naciskając klawisz CONTROL-F5. Tak jak w przypadku środowisk testowych i produkcyjnych, Uruchom stronę **Dodawanie uczniów** , Dodaj nowego ucznia, a następnie Wyświetl nowych uczniów na stronie **uczniów** . Umożliwia to sprawdzenie, czy baza danych szkoły została utworzona i zainicjowana oraz że masz do niej dostęp do odczytu i zapisu.

Wybierz stronę **Aktualizuj kredyty** i zaloguj się, aby sprawdzić, czy baza danych członkostwa została wdrożona i czy masz do niej dostęp. Jeśli nie przeprowadzisz migracji kont użytkowników, Utwórz konto administratora, a następnie wybierz stronę **Aktualizuj kredyty** , aby sprawdzić, czy działa.

## <a name="cleaning-up-sql-server-compact-files"></a>Czyszczenie SQL Server Compact plików

Nie są już potrzebne pliki i pakiety NuGet dołączone do obsługi SQL Server Compact. Jeśli chcesz (ten krok nie jest wymagany), możesz wyczyścić niepotrzebne pliki i odwołania.

W **Eksplorator rozwiązań**Usuń pliki *. sdf* z folderu *\_aplikacji* i *amd64* i *x86* z folderu *bin* .

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy Rozwiązanie (nie jeden z projektów), a następnie kliknij pozycję **Zarządzaj pakietami NuGet dla rozwiązania**.

W lewym okienku okna dialogowego **Zarządzanie pakietami NuGet** wybierz pozycję **zainstalowane pakiety**.

Wybierz pakiet **EntityFramework. SqlServerCompact** , a następnie kliknij pozycję **Zarządzaj**.

W oknie dialogowym **Wybierz projekty** są wybrane oba projekty. Aby odinstalować pakiet w obu projektach, wyczyść oba pola wyboru, a następnie kliknij przycisk **OK**.

W oknie dialogowym z pytaniem, czy chcesz odinstalować pakiety zależne, kliknij przycisk nie. Jeden z nich to pakiet Entity Framework, który należy zachować.

Postępuj zgodnie z tą samą procedurą, aby odinstalować pakiet **SqlServerCompact** . (Pakiety muszą zostać odinstalowane w tej kolejności, ponieważ pakiet **EntityFramework. SqlServerCompact** zależy od pakietu **SqlServerCompact** ).

Pomyślnie wykonano migrację do SQL Server Express i pełnych SQL Server. W następnym samouczku wprowadzisz inną zmianę bazy danych i zobaczysz, jak wdrożyć zmiany w bazie danych, gdy bazy danych testowych i produkcyjnych używają SQL Server Express i pełnych SQL Server.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
