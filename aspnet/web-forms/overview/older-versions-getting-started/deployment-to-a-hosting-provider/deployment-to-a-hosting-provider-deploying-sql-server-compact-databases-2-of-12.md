---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie SQL Server Compact baz danych — 2 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 56ceabc79947967846d342354fd033510be5f05a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568118"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie SQL Server Compact baz danych — 2 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak skonfigurować dwie SQL Server Compact bazy danych i aparat bazy danych do wdrożenia.

W przypadku dostępu do bazy danych aplikacja firmy Contoso University wymaga, aby następujące oprogramowanie zostało wdrożone z aplikacją, ponieważ nie jest ona uwzględniona w .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (aparat bazy danych).
- [ASP.NET uniwersalnych dostawców](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (co umożliwia systemowi członkostwa w ASP.NET używanie SQL Server Compact)
- [Entity Framework 5,0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First z migracjami).

Należy również wdrożyć strukturę bazy danych i niektóre (nie wszystkie) danych w dwóch bazach danych aplikacji. Zazwyczaj podczas tworzenia aplikacji należy wprowadzić dane testowe do bazy danych, która nie ma być wdrażana w działającej witrynie. Można jednak również wprowadzić pewne dane produkcyjne, które chcesz wdrożyć. W tym samouczku skonfigurujesz projekt firmy Contoso University, dzięki czemu wymagane oprogramowanie i odpowiednie dane zostaną uwzględnione podczas wdrażania.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact a SQL Server Express

Przykładowa aplikacja używa SQL Server Compact 4,0. Ten aparat bazy danych jest stosunkowo nową opcją dla witryn sieci Web; wcześniejsze wersje SQL Server Compact nie działają w środowisku hostingu w sieci Web. SQL Server Compact oferuje kilka korzyści w porównaniu z bardziej typowym scenariuszem tworzenia przy użyciu SQL Server Express i wdrażania w pełnych SQL Server. W zależności od wybranego dostawcy hostingu SQL Server Compact może być tańszy do wdrożenia, ponieważ niektórzy dostawcy są obciążeni dodatkową obsługą pełnej SQL Server bazy danych. Nie ma dodatkowych opłat za SQL Server Compact, ponieważ można wdrożyć sam aparat bazy danych jako część aplikacji sieci Web.

Należy jednak pamiętać o jego ograniczeniach. SQL Server Compact nie obsługuje procedur składowanych, wyzwalaczy, widoków ani replikacji. (Aby zapoznać się z pełną listą funkcji SQL Server, które nie są obsługiwane przez SQL Server Compact, zobacz [różnice między SQL Server Compact i SQL Server](https://msdn.microsoft.com/library/bb896140.aspx)). Ponadto niektóre narzędzia, których można użyć do manipulowania schematami i danymi w bazach danych SQL Server Express i SQL Server, nie działają w SQL Server Compact. Na przykład nie można używać narzędzi danych SQL Server Management Studio ani SQL Server w programie Visual Studio z bazami danych SQL Server Compact. Dostępne są inne opcje pracy z bazami danych SQL Server Compact:

- Możesz użyć Eksplorator serwera w programie Visual Studio, który oferuje ograniczoną funkcjonalność manipulowania bazami danych dla SQL Server Compact.
- Możesz użyć funkcji manipulowania bazą danych programu [WebMatrix](https://www.microsoft.com/web/webmatrix/), która zawiera więcej funkcji niż Eksplorator serwera.
- Można używać relatywnie w pełni funkcjonalnych narzędzi innych firm lub typu open source, takich jak [przybornik SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) i [Narzędzie SQL Compact Data i skrypt schematu](https://github.com/ErikEJ/SqlCeToolbox).
- Można napisać i uruchomić własne skrypty języka DDL (Data Definition Language), aby manipulować schematem bazy danych.

Możesz rozpocząć od SQL Server Compact, a następnie później przeprowadzić uaktualnienie w miarę potrzeb. W kolejnych samouczkach w tej serii pokazano, jak przeprowadzić migrację z SQL Server Compact do SQL Server Express i SQL Server. Jeśli jednak tworzysz nową aplikację i oczekujesz, że SQL Server w najbliższej przyszłości, prawdopodobnie najlepiej zacząć od SQL Server lub SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Konfigurowanie aparatu bazy danych SQL Server Compact na potrzeby wdrożenia

Oprogramowanie wymagane do uzyskania dostępu do danych w aplikacji z uczelnią contoso zostało dodane przez zainstalowanie następujących pakietów NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System. Web. Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET Universal Providers)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework. SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Linki wskazują bieżące wersje tych pakietów, które mogą być nowsze niż zainstalowane w początkowym projekcie pobranym dla tego samouczka. W przypadku wdrażania dla dostawcy hostingu upewnij się, że używasz Entity Framework 5,0 lub nowszego. Wcześniejsze wersje Migracje Code First wymagają pełnego zaufania, a w wielu dostawców hostingu aplikacja będzie działać w średnim zaufaniu. Aby uzyskać więcej informacji na temat średniego zaufania, zobacz samouczek [wdrażanie w usługach IIS jako środowisko testowe](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

Instalacja pakietu NuGet ogólnie przyjmuje wszystkie informacje, które są potrzebne w celu wdrożenia tego oprogramowania w aplikacji. W niektórych przypadkach obejmuje to zadania, takie jak zmiana pliku Web. config i Dodawanie skryptów programu PowerShell, które są uruchamiane po każdym skompilowaniu rozwiązania. **Aby dodać obsługę dowolnej z tych funkcji (takich jak SQL Server Compact i Entity Framework) bez korzystania z programu NuGet, należy się upewnić, że należy wiedzieć, jaka jest instalacja pakietu NuGet, dzięki czemu można wykonać te same czynności ręcznie.**

Istnieje jeden wyjątek, w którym program NuGet nie zajmie się wszystkimi wszystkimi czynnościami, które należy wykonać, aby zapewnić pomyślne wdrożenie. Pakiet NuGet SqlServerCompact dodaje skrypt po kompilacji do projektu, który kopiuje zestawy natywne do wersji *x86* i *amd64* w folderze *bin* projektu, ale skrypt nie uwzględnia tych folderów w projekcie. W związku z tym Web Deploy nie skopiują ich do docelowej witryny sieci Web, chyba że zostaną ręcznie dołączone do projektu. (To zachowanie wynika z domyślnej konfiguracji wdrożenia; inna opcja, której nie można użyć w tych samouczkach, to zmiana ustawienia kontrolującego takie zachowanie. Ustawienie, które można zmienić, dotyczy **tylko plików potrzebnych do uruchomienia aplikacji** w obszarze **elementy do wdrożenia** na karcie **pakowanie/publikowanie w sieci Web** okna **właściwości projektu** . Zmiana tego ustawienia nie jest ogólnie zalecana, ponieważ może to spowodować wdrożenie wielu plików w środowisku produkcyjnym niż jest to konieczne. Aby uzyskać więcej informacji o alternatywach, zobacz Przewodnik [konfigurowania właściwości projektu](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) .)

Skompiluj projekt, a następnie w **Eksplorator rozwiązań** kliknij pozycję **Pokaż wszystkie pliki** , jeśli jeszcze nie zostało to zrobione. Może być również konieczne kliknięcie przycisku **Odśwież**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Rozwiń folder **bin** , aby wyświetlić foldery **amd64** i **x86** , a następnie wybierz te foldery, kliknij prawym przyciskiem myszy, a następnie wybierz opcję **Dołącz do projektu**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Ikony folderu zmieniają się, aby pokazać, że folder został uwzględniony w projekcie.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Konfigurowanie Migracje Code First wdrażania bazy danych aplikacji

W przypadku wdrażania bazy danych aplikacji zazwyczaj nie można wdrożyć bazy danych programistycznych ze wszystkimi danymi w środowisku produkcyjnym, ponieważ większość danych jest prawdopodobnie tylko do celów testowych. Na przykład nazwy uczniów w testowej bazie danych są fikcyjne. Z drugiej strony często nie można wdrożyć tylko struktury bazy danych bez danych. Niektóre dane w bazie danych testowych mogą być rzeczywistymi danymi i muszą znajdować się w nich, gdy użytkownicy zaczynają korzystać z aplikacji. Na przykład baza danych może mieć tabelę zawierającą prawidłowe wartości klasy lub rzeczywiste nazwy działów.

Aby zasymulować ten typowy scenariusz, należy skonfigurować metodę Migracje Code First inicjatora, która wstawia do bazy danych tylko dane, które mają być w środowisku produkcyjnym. Ta metoda inicjatora nie wstawia danych testowych, ponieważ zostanie uruchomiona w środowisku produkcyjnym po Code First tworzenia bazy danych w środowisku produkcyjnym.

We wcześniejszych wersjach Code First przed udostępnieniem migracji była ona powszechna dla metod inicjatorów, aby wstawić dane testowe również, ponieważ w przypadku każdej zmiany modelu podczas programowania baza danych musiała zostać całkowicie usunięta i ponownie utworzona od podstaw. Przy Migracje Code First dane testowe są zachowywane po zmianie bazy danych, więc w tym dane testowe w metodzie inicjatora nie są konieczne. Pobrany projekt korzysta z metody poprzedzającej migrację, która obejmuje wszystkie dane w metodzie nasion klasy inicjatora. Ten samouczek obejmuje wyłączenie klasy inicjatora i włączenie migracji. Następnie należy zaktualizować metodę inicjatora w klasie konfiguracji migracji, tak aby wstawia tylko dane, które mają zostać wstawione w środowisku produkcyjnym.

Na poniższym diagramie przedstawiono schemat bazy danych aplikacji:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Na potrzeby tych samouczków zakłada się, że tabele `Student` i `Enrollment` powinny być puste podczas pierwszego wdrożenia lokacji. Inne tabele zawierają dane, które mają zostać wstępnie załadowane, gdy aplikacja się na żywo.

Ponieważ będziesz używać Migracje Code First, nie musisz już używać inicjatora Code First **DropCreateDatabaseIfModelChanges** . Kod dla tego inicjatora znajduje się w pliku SchoolInitializer.cs w projekcie ContosoUniversity. DAL. Ustawienie w pliku Web. config w elemencie **AppSettings** powoduje uruchomienie tego inicjatora za każdym razem, gdy aplikacja próbuje uzyskać dostęp do bazy danych po raz pierwszy:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Otwórz plik Web. config aplikacji i Usuń element określający klasę inicjatora Code First z elementu appSettings. Element appSettings wygląda teraz następująco:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Innym sposobem określenia klasy inicjatora jest wykonanie wywołania `Database.SetInitializer` w metodzie `Application_Start` w pliku *Global. asax* . W przypadku włączenia funkcji migracji w projekcie, który używa tej metody do określenia inicjatora, Usuń ten wiersz kodu.

Następnie Włącz Migracje Code First.

Pierwszym krokiem jest upewnienie się, że projekt ContosoUniversity jest ustawiony jako projekt startowy. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz pozycję **Ustaw jako projekt startowy**. Migracje Code First będzie wyglądać w projekcie startowym w celu znalezienia parametrów połączenia z bazą danych.

W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Konsola menedżera pakietów**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

W górnej części okna **konsoli Menedżera pakietów** wybierz pozycję CONTOSOUNIVERSITY. dal jako projekt domyślny, a następnie w wierszu `PM>` wprowadź "Enable-migrations".

![Włącz-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

To polecenie tworzy plik *Configuration.cs* w nowym folderze *migracji* w projekcie ContosoUniversity. dal.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Wybrano projekt DAL, ponieważ należy wykonać polecenie "Enable-migrations" w projekcie, który zawiera klasę kontekstu Code First. Gdy Klasa znajduje się w projekcie biblioteki klas, Migracje Code First szuka parametrów połączenia bazy danych w projekcie startowym dla rozwiązania. W rozwiązaniu ContosoUniversity projekt sieci Web został ustawiony jako projekt startowy. (Jeśli nie chcesz wyznaczyć projektu, który ma parametry połączenia jako projekt startowy w programie Visual Studio, możesz określić projekt startowy w poleceniu programu PowerShell. Aby wyświetlić składnię polecenia dla polecenia Enable-migrations, można wprowadzić polecenie "Get-Help Enable-migrations".

Otwórz plik Configuration.cs i Zastąp komentarze w metodzie `Seed` następującym kodem:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Odwołania do `List` mają w nich czerwoną falistej linie, ponieważ nie masz jeszcze instrukcji `using` dla swojej przestrzeni nazw. Kliknij prawym przyciskiem myszy jedno z wystąpień `List` i kliknij polecenie **Rozwiąż**, a następnie kliknij pozycję **System. Collections. Generic**.

![Rozwiązywanie przy użyciu instrukcji using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

To zaznaczenie menu dodaje następujący kod do instrukcji `using` w górnej części pliku.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Dodawanie kodu do metody `Seed` jest jednym z wielu sposobów wstawiania stałych danych do bazy danych. Alternatywą jest dodanie kodu do `Up` i `Down` metod każdej klasy migracji. Metody `Up` i `Down` zawierają kod implementujący zmiany w bazie danych. Przykłady tych elementów znajdziesz w samouczku [wdrażanie aktualizacji bazy danych](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .
> 
> Możesz również napisać kod, który wykonuje instrukcje SQL, za pomocą metody `Sql`. Na przykład jeśli dodano kolumnę budżetu do tabeli dział i chcesz zainicjować wszystkie budżety działu do $1 000,00 w ramach migracji, można dodać następujący wiersz kodu do metody `Up` dla tej migracji:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Ten przykład pokazano w tym samouczku używa metody `AddOrUpdate` w metodzie `Seed` klasy Migracje Code First `Configuration`. Migracje Code First wywołuje metodę `Seed` po każdej migracji, a ta metoda aktualizuje wiersze, które zostały już wstawione lub wstawia je, jeśli jeszcze nie istnieją. Metoda `AddOrUpdate` może nie być najlepszym wyborem dla Twojego scenariusza. Aby uzyskać więcej informacji, zobacz temat zapoznaj [się z opcją dr 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

Naciśnij kombinację klawiszy CTRL-SHIFT-B, aby skompilować projekt.

Następnym krokiem jest utworzenie klasy `DbMigration` dla migracji początkowej. Ta migracja umożliwia utworzenie nowej bazy danych, dlatego należy usunąć już istniejącą bazę danych. Bazy danych SQL Server Compact są zawarte w plikach *. sdf* w folderze *danych\_aplikacji* . W **Eksplorator rozwiązań**rozwiń węzeł *aplikacje\_dane* w projekcie ContosoUniversity, aby wyświetlić dwie bazy danych SQL Server Compact, które są reprezentowane przez pliki *SDF* .

Kliknij prawym przyciskiem myszy plik *szkoły. sdf* , a następnie kliknij polecenie **Usuń**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

W oknie **konsola Menedżera pakietów** wprowadź polecenie "Add-Migration Initial", aby utworzyć początkową migrację i nadaj jej nazwę "Initial".

![Dodaj migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migracje Code First tworzy inny plik klasy w folderze *migracji* , a ta klasa zawiera kod, który tworzy schemat bazy danych.

W **konsoli Menedżera pakietów**wprowadź polecenie "Update-Database", aby utworzyć bazę danych i uruchomić metodę **inicjatora** .

![Update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Jeśli zostanie wyświetlony komunikat o błędzie informujący o tym, że tabela już istnieje i nie można jej utworzyć), prawdopodobnie aplikacja została uruchomiona po usunięciu bazy danych programu i przed wykonaniem `update-database`. W takim przypadku Usuń plik *szkoły. sdf* ponownie, a następnie ponów próbę wykonania polecenia `update-database`.

Uruchom aplikację. Teraz Strona Students jest pusta, ale strona instruktorzy zawiera instruktorów. Jest to co uzyskasz w środowisku produkcyjnym po wdrożeniu aplikacji.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Projekt jest teraz gotowy do wdrożenia bazy danych *szkoły* .

## <a name="creating-a-membership-database-for-deployment"></a>Tworzenie bazy danych członkostwa na potrzeby wdrożenia

Aplikacja firmy Contoso University używa systemu członkostwa ASP.NET i uwierzytelniania formularzy do uwierzytelniania i autoryzowania użytkowników. Jedna z jej stron jest dostępna tylko dla administratorów. Aby wyświetlić tę stronę, uruchom aplikację i wybierz pozycję **Aktualizuj kredyty** w menu wysuwanym w obszarze **kursy**. W aplikacji zostanie wyświetlona strona **logowania** , ponieważ tylko Administratorzy są uprawnieni do korzystania ze strony **aktualizacji kredytów** .

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Zaloguj się jako "Administrator" przy użyciu hasła "pas $ w0rd" (Zwróć uwagę na liczbę zero zamiast litery "o" w "w0rd"). Po zalogowaniu zostanie wyświetlona strona **kredyty z aktualizacją** .

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Podczas wdrażania lokacji po raz pierwszy często wyklucza się większość lub wszystkie utworzone konta użytkowników do testowania. W takim przypadku należy wdrożyć konto administratora i nie konta użytkowników. Zamiast ręcznego usuwania kont testowych należy utworzyć nową bazę danych członkostwa, która ma tylko jedno konto użytkownika administratora, które jest potrzebne w środowisku produkcyjnym.

> [!NOTE]
> Baza danych członkostwa przechowuje skrót hasła konta. Aby wdrożyć konta z jednego komputera do drugiego, należy się upewnić, że procedury tworzenia skrótów nie generują różnych skrótów na serwerze docelowym niż na komputerze źródłowym. Wygenerują one te same skróty w przypadku używania ASP.NET uniwersalnych dostawców, o ile nie zmienimy domyślnego algorytmu. Domyślny algorytm to HMACSHA256 i jest określony w atrybucie **walidacji** elementu **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** w pliku Web. config.

Baza danych członkostwa nie jest obsługiwana przez Migracje Code First i nie ma automatycznego inicjatora, który jest przeznaczony dla bazy danych z kontami testów (w przypadku bazy danych szkoły). W związku z tym, aby zachować dostęp do danych testowych, należy wykonać kopię testowej bazy danych przed utworzeniem nowej.

W **Eksplorator rozwiązań**Zmień nazwę pliku *ASPNET. sdf* w folderze *\_danych aplikacji* na *ASPNET-dev. sdf*. (Nie twórz kopii, po prostu zmień jej nazwę) — w danej chwili utworzysz nową bazę danych.

W **Eksplorator rozwiązań**upewnij się, że wybrano projekt sieci Web (ContosoUniversity, a nie CONTOSOUNIVERSITY. dal). Następnie w menu **projekt** wybierz pozycję **Konfiguracja ASP.NET** , aby uruchomić **Narzędzie do administrowania witryną sieci Web**(Wat).

Wybierz kartę **Zabezpieczenia**.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Kliknij przycisk **Utwórz role lub zarządzaj nimi** i dodaj rolę **administratora** .

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Przejdź z powrotem do karty **zabezpieczenia** , kliknij pozycję **Utwórz użytkownika**i Dodaj użytkownika "Administrator" jako administratora. Przed kliknięciem przycisku **Utwórz użytkownika** na stronie **Tworzenie użytkownika** upewnij się, że jest zaznaczone pole wyboru **administrator** . Hasło użyte w tym samouczku to "pas $ w0rd" i można wprowadzić dowolny adres e-mail.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Zamknij okno przeglądarki. W **Eksplorator rozwiązań**kliknij przycisk Odśwież, aby wyświetlić nowy plik *ASPNET. sdf* .

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Kliknij prawym przyciskiem myszy pozycję **ASPNET. sdf** i wybierz pozycję **include w projekcie**.

## <a name="distinguishing-development-from-production-databases"></a>Rozróżnianie programowania od produkcyjnych baz danych

W tej sekcji zmienisz nazwy baz danych tak, aby wersje deweloperskie były School-Dev. sdf i aspnet-Dev. sdf, a wersja produkcyjna to School-Prod. sdf i aspnet-Prod. sdf. Nie jest to konieczne, ale może to pomóc w zapewnieniu pomylenia wersji testowych i produkcyjnych baz danych.

W **Eksplorator rozwiązań**kliknij przycisk **Odśwież** i rozwiń folder dane\_aplikacji, aby zobaczyć utworzoną wcześniej bazę danych szkoły. kliknij go prawym przyciskiem myszy, a następnie wybierz opcję **Dołącz do projektu**.

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Zmień nazwę *ASPNET. sdf* na *ASPNET-prod. sdf*.

Zmień nazwę pliku *szkoła. sdf* na *School-dev. sdf*.

Po uruchomieniu aplikacji w programie Visual Studio nie chcesz używać wersji- *produkcyjnej* plików bazy danych, które mają być używane w wersjach *deweloperskich* . W związku z tym należy zmienić parametry połączenia w pliku Web. config, tak aby wskazywały na *deweloperskie* wersje baz danych. (Plik School-Prod. SDF nie został utworzony, ale jest to prawidłowe, ponieważ Code First utworzy tę bazę danych w środowisku produkcyjnym przy pierwszym uruchomieniu aplikacji).

Otwórz plik Web. config aplikacji i Znajdź parametry połączenia:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Zmień wartość "ASPNET. sdf" na "aspnet-Dev. sdf" i zmień wartość "Szkoła. sdf" na "School-Dev. sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Aparat bazy danych SQL Server Compact i obie bazy danych są teraz gotowe do wdrożenia. W poniższym samouczku pokazano, jak skonfigurować automatyczne przekształcenia plików *Web. config* dla ustawień, które muszą być różne w środowiskach programistycznych, testowych i produkcyjnych. (Wśród ustawień, które muszą zostać zmienione, są parametry połączenia, ale te zmiany zostaną skonfigurowane później podczas tworzenia profilu publikowania).

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji o narzędziu NuGet, zobacz [Manage Project librarys with NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) i [NuGet](http://docs.nuget.org/docs/start-here/overview). Jeśli nie chcesz używać narzędzia NuGet, musisz dowiedzieć się, jak przeanalizować pakiet NuGet, aby określić, co jest wykonywane podczas instalacji. (Na przykład może to spowodować skonfigurowanie transformacji *Web. config* , skonfigurowanie skryptów programu PowerShell do uruchamiania w czasie kompilacji itp.). Aby dowiedzieć się więcej na temat działania programu NuGet, zobacz temat szczególnie [Tworzenie i publikowanie pakietu](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) oraz [plików konfiguracji i przekształceń kodu źródłowego](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [dalej](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
