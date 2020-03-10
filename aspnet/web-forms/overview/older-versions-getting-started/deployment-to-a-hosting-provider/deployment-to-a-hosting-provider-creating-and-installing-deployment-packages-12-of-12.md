---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: Rozwiązywanie problemów (12 z 12) | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: db8f58e3679e6dea865dadb6f64916032dd9f38c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528197"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: Rozwiązywanie problemów (12 z 12)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, pokazano, jak wdrożyć wersje SQL Server inne niż SQL Server Compact i jak wdrożyć je w witrynach sieci Web systemu Windows Azure, zobacz [ASP.NET Web Deployment za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

Na tej stronie opisano niektóre typowe problemy, które mogą wystąpić podczas wdrażania aplikacji sieci Web ASP.NET za pomocą programu Visual Studio. Dla każdej z nich można określić co najmniej jedną możliwą przyczynę i odpowiednie rozwiązania.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Błąd serwera w aplikacji "/" — bieżące ustawienia błędów niestandardowych uniemożliwiają zdalne wyświetlanie szczegółów błędu

### <a name="scenario"></a>Scenariusz

Po wdrożeniu lokacji programu na hoście zdalnym zostanie wyświetlony komunikat o błędzie, który zawiera informacje o ustawieniu customErrors w pliku Web. config, ale nie wskazuje, co to jest rzeczywista przyczyna błędu:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie ASP.NET wyświetla szczegółowe informacje o błędzie tylko wtedy, gdy aplikacja sieci Web jest uruchomiona na komputerze lokalnym. Zazwyczaj nie ma potrzeby wyświetlania szczegółowych informacji o błędach, gdy aplikacja sieci Web jest publicznie dostępna w Internecie, ponieważ hakerzy mogą korzystać z tych informacji w celu znalezienia luk w zabezpieczeniach aplikacji. Niemniej jednak w przypadku wdrażania witryny lub aktualizacji w lokacji czasami coś się nie powiedzie i należy uzyskać rzeczywisty komunikat o błędzie.

Aby umożliwić aplikacji Wyświetlanie szczegółowych komunikatów o błędach uruchamianych na hoście zdalnym, należy edytować plik Web. config w celu ustawienia trybu `customErrors` wyłączone, ponownie wdrożyć aplikację i uruchomić aplikację w następujący sposób:

1. Jeśli plik Web. config aplikacji zawiera element `customErrors` w elemencie `system.web`, Zmień atrybut `mode` na "off". W przeciwnym razie Dodaj element `customErrors` w elemencie `system.web` z atrybutem `mode` ustawionym na wartość "off", jak pokazano w następującym przykładzie:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Wdróż aplikację.
3. Uruchom aplikację i powtórz wszystkie wcześniej, które spowodowały wystąpienie błędu. Teraz można zobaczyć, jaki jest rzeczywisty komunikat o błędzie.
4. Po rozwiązaniu błędu Przywróć oryginalne ustawienie `customErrors` i ponownie Wdróż aplikację.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Odmowa dostępu na stronie sieci Web, która używa SQL Server Compact

### <a name="scenario"></a>Scenariusz

Podczas wdrażania lokacji korzystającej z SQL Server Compact i uruchamiania strony w wdrożonej lokacji, która uzyskuje dostęp do bazy danych, zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Konto usługi sieciowej na serwerze musi mieć możliwość odczytywania natywnych plików binarnych usługi SQL Server Compact, które znajdują się w folderze *bin\amd64* lub *bin\x86* , ale nie ma uprawnień do odczytu tych folderów. Ustaw uprawnienie Odczyt dla usługi sieciowej w folderze *bin* , aby upewnić się, że uprawnienia zostały rozszerzone do podfolderów.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Nie można odczytać pliku konfiguracji z powodu niewystarczających uprawnień

### <a name="scenario"></a>Scenariusz

Po kliknięciu przycisku Publikuj programu Visual Studio w celu wdrożenia aplikacji w usługach IIS na komputerze lokalnym publikowanie nie powiedzie się, a w oknie **danych wyjściowych** zostanie wyświetlony komunikat o błędzie podobny do tego:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Aby skorzystać z jednego kliknięcia przycisku Publikuj w usługach IIS na komputerze lokalnym, należy uruchomić program Visual Studio z uprawnieniami administratora. Zamknij program Visual Studio i uruchom go ponownie z uprawnieniami administratora.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Nie można nawiązać połączenia z komputerem docelowym... Przy użyciu określonego procesu

### <a name="scenario"></a>Scenariusz

Po kliknięciu przycisku Publikuj programu Visual Studio w celu wdrożenia aplikacji publikowanie kończy się niepowodzeniem, a w oknie **danych wyjściowych** zostanie wyświetlony komunikat o błędzie podobny do tego:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Serwer proxy przerywa komunikację z serwerem docelowym. W panelu sterowania systemu Windows lub w programie Internet Explorer wybierz pozycję **Opcje internetowe** i wybierz kartę **połączenia** . W oknie dialogowym **Właściwości internetowe** kliknij przycisk **Ustawienia sieci LAN**. W oknie dialogowym **Ustawienia sieci lokalnej (LAN)** wyczyść pole wyboru **Automatycznie wykryj ustawienia** . Następnie kliknij ponownie przycisk Publikuj.

Jeśli problem będzie nadal występować, skontaktuj się z administratorem systemu, aby określić, co można zrobić za pomocą ustawień serwera proxy lub zapory. Problem występuje, ponieważ Web Deploy używa niestandardowego portu do wdrożenia usługi zarządzania siecią Web (8172); w przypadku innych połączeń Web Deploy używa portu 80. W przypadku wdrażania w ramach dostawcy hostingu innej firmy zwykle używana jest usługa zarządzania siecią Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Domyślna pula aplikacji platformy .NET 4,0 nie istnieje

### <a name="scenario"></a>Scenariusz

Podczas wdrażania aplikacji wymagającej .NET Framework 4 zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

ASP.NET 4 nie jest zainstalowana w usługach IIS. Jeśli wdrażany serwer jest komputerem deweloperskim i ma zainstalowany program Visual Studio 2010, ASP.NET 4 jest zainstalowany na komputerze, ale może nie być zainstalowany w usługach IIS. Na serwerze, na którym wdrażasz program, Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zainstaluj ASP.NET 4 w usługach IIS, uruchamiając następujące polecenia:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Może być również konieczne ręczne ustawienie wersji .NET Framework domyślnej puli aplikacji. Aby uzyskać więcej informacji, zobacz samouczek [wdrażanie w usługach IIS jako środowisko testowe](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Format ciągu inicjującego nie jest zgodny ze specyfikacją, rozpoczynając od indeksu 0.

### <a name="scenario"></a>Scenariusz

Po wdrożeniu aplikacji przy użyciu funkcji publikowania jednego kliknięcia podczas uruchamiania strony, która uzyskuje dostęp do bazy danych, zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Otwórz plik *Web. config* w wdrożonej lokacji i sprawdź, czy wartości parametrów połączenia zaczynają się od `$(ReplaceableToken_`, jak w poniższym przykładzie:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Jeśli parametry połączenia wyglądają podobnie jak w tym przykładzie, należy edytować plik projektu i dodać następującą właściwość do elementu `PropertyGroup`, który jest przeznaczony dla wszystkich konfiguracji kompilacji:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Następnie ponownie Wdróż aplikację.

## <a name="http-500-internal-server-error"></a>Błąd wewnętrzny serwera HTTP 500

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej lokacji zostanie wyświetlony następujący komunikat o błędzie bez określonych informacji wskazujących przyczynę błędu:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Istnieje wiele przyczyn błędów 500, ale jedną z możliwych przyczyn jest umieszczenie elementu XML w niewłaściwym miejscu w jednym z plików transformacji XML. Na przykład ten błąd występuje, jeśli umieścisz transformację, która wstawia element `<location>` w obszarze `<system.web>` zamiast bezpośrednio w `<configuration>`. W takim przypadku rozwiązanie jest odpowiednie do skorygowania pliku transformacji XML i ponownego wdrożenia.

## <a name="http-50021-internal-server-error"></a>Błąd wewnętrzny serwera HTTP 500,21

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej lokacji zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Wdrożona witryna jest przeznaczona dla ASP.NET 4, ale ASP.NET 4 nie jest zarejestrowana w usługach IIS na serwerze. Na serwerze otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zarejestruj ASP.NET 4, uruchamiając następujące polecenia:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Może być również konieczne ręczne ustawienie wersji .NET Framework domyślnej puli aplikacji. Aby uzyskać więcej informacji, zobacz samouczek [wdrażanie w usługach IIS jako środowisko testowe](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

## <a name="login-failed-opening-sql-server-express-database-in-app_data"></a>Nie można otworzyć SQL Server Express bazy danych w aplikacji\_dane

### <a name="scenario"></a>Scenariusz

Zaktualizowano parametry połączenia pliku *Web. config* w taki sposób, aby wskazywały bazę danych SQL Server Express jako plik *mdf* w *aplikacji\_folderu danych* , a przy pierwszym uruchomieniu aplikacji zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Nazwa pliku *. mdf* nie może być zgodna z nazwą żadnej SQL Server Expressj bazy danych, która kiedykolwiek istniała na komputerze, nawet jeśli plik *MDF* został usunięty z wcześniej istniejącej bazy danych. Zmień nazwę pliku *MDF* na nazwę, która nigdy nie była używana jako nazwa bazy danych, a następnie Zmień plik *Web. config* , aby używał nowej nazwy. Alternatywnie można użyć [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) do usuwania istniejących SQL Server Express baz danych.

## <a name="model-compatibility-cannot-be-checked"></a>Nie można sprawdzić zgodności modelu

### <a name="scenario"></a>Scenariusz

Zaktualizowano parametry połączenia pliku *Web. config* , aby wskazywały nową bazę danych SQL Server Express i przy pierwszym uruchomieniu aplikacji zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Jeśli nazwa bazy danych wprowadzona w pliku Web. config była kiedykolwiek używana wcześniej na komputerze, baza danych może już istnieć z niektórymi tabelami. Wybierz nową nazwę, która nie została wcześniej użyta na komputerze, i Zmień plik *Web. config* , aby wskazywał na użycie tej nowej nazwy bazy danych. Alternatywnie można użyć [narzędzia SQL Server Express](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) lub [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) , aby usunąć istniejącą bazę danych.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Błąd SQL, gdy skrypt próbuje utworzyć użytkowników lub role

### <a name="scenario"></a>Scenariusz

Używasz wdrożenia bazy danych skonfigurowanego na karcie **pakowanie/publikowanie SQL** , skrypty SQL, które są uruchamiane podczas wdrażania, obejmują tworzenie użytkownika lub Tworzenie poleceń ról, a wykonywanie skryptu kończy się niepowodzeniem, gdy te polecenia są wykonywane. Może pojawić się więcej szczegółowych komunikatów, takich jak następujące:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Jeśli ten błąd występuje po skonfigurowaniu wdrożenia bazy danych w kreatorze **publikacji sieci Web** , a nie na karcie **pakowanie/publikowanie SQL** , Utwórz wątek na forum [Konfiguracja i wdrażanie](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) , a rozwiązanie zostanie dodane do tej strony rozwiązywania problemów.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Konto użytkownika używane do wdrożenia nie ma uprawnień do tworzenia użytkowników lub ról. Na przykład firma hostingowa może przypisać role `db_datareader`, `db_datawriter`i `db_ddladmin` do konta użytkownika, które konfiguruje Cię dla Ciebie. Są one wystarczające do tworzenia większości obiektów bazy danych, ale nie do tworzenia użytkowników lub ról. Jednym ze sposobów uniknięcia błędu jest wykluczenie użytkowników i ról z wdrażania bazy danych. Można to zrobić, edytując `PreSource` element dla automatycznie generowanego skryptu bazy danych, aby zawierał następujące atrybuty:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Aby uzyskać informacje na temat edytowania elementu `PreSource` w pliku projektu, zobacz [How to: Edit Settings Deployment w pliku projektu](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Jeśli użytkownicy lub role w Twojej bazie danych programistycznych muszą znajdować się w docelowej bazie danych, skontaktuj się z dostawcą hostingu w celu uzyskania pomocy.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server błąd limitu czasu podczas uruchamiania skryptów niestandardowych podczas wdrażania

### <a name="scenario"></a>Scenariusz

Określono niestandardowe skrypty SQL do uruchomienia podczas wdrażania, a po ich uruchomieniu Web Deploy przekroczenia limitu czasu.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Uruchamianie wielu skryptów z różnymi trybami transakcji może spowodować błędy limitu czasu. Domyślnie automatycznie generowane skrypty są uruchamiane w transakcji, ale skrypty niestandardowe nie są obsługiwane. W przypadku wybrania opcji **Pobierz dane i/lub schemat z istniejącej bazy danych** na karcie **pakowanie/publikowanie SQL** i w przypadku dodania niestandardowego skryptu SQL należy zmienić ustawienia transakcji w niektórych skryptach, tak aby wszystkie skrypty używały tych samych ustawień transakcji. Aby uzyskać więcej informacji, zobacz [jak: Wdrażanie bazy danych za pomocą projektu aplikacji sieci Web](https://msdn.microsoft.com/library/dd465343.aspx).

Jeśli skonfigurowano ustawienia transakcji w taki sposób, że wszystkie są takie same, ale nadal pojawiają się te błędy, możliwe jest obejście skryptów osobno. W siatce **skryptów bazy danych** na karcie **pakowanie/publikowanie** SQL Usuń zaznaczenie pola wyboru **Dołącz** dla skryptu, który powoduje błąd limitu czasu, a następnie opublikuj projekt. Następnie wróć do siatki **skryptów bazy danych** , zaznacz pole wyboru **Dodaj** ten skrypt, a następnie wyczyść pola wyboru **Dołącz** dla innych skryptów. Następnie opublikuj projekt ponownie. Tym razem podczas publikowania tylko wybrany skrypt niestandardowy zostanie uruchomiony.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Dane strumienia manifestu lokacji nie są jeszcze dostępne

### <a name="scenario"></a>Scenariusz

Podczas instalowania pakietu przy użyciu pliku *Deploy. cmd* z opcją `t` (test) zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Komunikat o błędzie oznacza, że polecenie nie może utworzyć raportu testowego. Jednak polecenie może zostać uruchomione w przypadku użycia opcji `y` (rzeczywista instalacja). Komunikat wskazuje, że wystąpił problem z uruchomieniem polecenia w trybie testowym.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Ta aplikacja wymaga ManagedRuntimeVersion v 4.0

### <a name="scenario"></a>Scenariusz

Podczas próby wdrożenia zostanie wyświetlony następujący komunikat o błędzie:

 Błąd: dane strumienia "sitemanifest/dbFullSql [@path=" C:\TEMP\AdventureWorksGrant.sql "]/sqlScript" nie są jeszcze dostępne. Dla puli aplikacji, której próbujesz użyć, właściwość "managedRuntimeVersion" ma wartość "v 2.0". Ta aplikacja wymaga wersji "v 4.0". 

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

ASP.NET 4 nie jest zainstalowana w usługach IIS. Jeśli wdrażany serwer jest komputerem deweloperskim i ma zainstalowany program Visual Studio 2010, ASP.NET 4 jest zainstalowany na komputerze, ale może nie być zainstalowany w usługach IIS. Na serwerze, na którym wdrażasz program, Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i zainstaluj ASP.NET 4 w usługach IIS, uruchamiając następujące polecenia:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Nie można rzutować Microsoft. Web. Deployment. DeploymentProviderOptions

### <a name="scenario"></a>Scenariusz

Podczas wdrażania pakietu zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Podjęto próbę wdrożenia z Menedżera usług IIS przy użyciu interfejsu użytkownika Web Deploy 1,1 na serwerze, na którym zainstalowano Web Deploy 2,0. Jeśli używasz narzędzia administracji zdalnej usług IIS do wdrożenia przez zaimportowanie pakietu, zaznacz okno dialogowe **nowe funkcje dostępne** po nawiązaniu połączenia. (To okno dialogowe może być wyświetlane tylko raz podczas pierwszego nawiązywania połączenia. Aby wyczyścić połączenie i zacząć od nowa, Zamknij Menedżera usług IIS i uruchom go ponownie, wprowadzając `inetmgr /reset` w wierszu polecenia.) Jeśli jedna z wymienionych funkcji jest **Web Deploy interfejsie użytkownika**i ma numer wersji niższą niż 8, na serwerze, na którym wdrażana jest wersja, może być zainstalowanych zarówno 1,1, jak i 2,0 wersji programu Web Deploy. Aby można było wdrożyć program z klienta z zainstalowanym systemem 2,0, na serwerze musi być zainstalowany tylko Web Deploy 2,0. Aby rozwiązać ten problem, należy skontaktować się z dostawcą hostingu.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Nie można załadować natywnych składników SQL Server Compact

### <a name="scenario"></a>Scenariusz

Po uruchomieniu wdrożonej lokacji zostanie wyświetlony następujący komunikat o błędzie:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Wdrożona witryna nie ma podfolderów *amd64* i *x86* z natywnymi zestawami w ramach folderu *bin* aplikacji. Na komputerze, na którym zainstalowano SQL Server Compact, zestawy natywne znajdują się w *folderze C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Najlepszym sposobem na uzyskanie odpowiednich plików do odpowiednich folderów w projekcie programu Visual Studio jest zainstalowanie pakietu NuGet SqlServerCompact. Instalacja pakietu dodaje skrypt po kompilacji w celu skopiowania zestawów natywnych do *amd64* i *x86*. Jednak aby można było wdrożyć te elementy, należy je ręcznie uwzględnić w projekcie. Aby uzyskać więcej informacji, zobacz samouczek [wdrażania SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Błąd "ścieżka jest nieprawidłowa" po wdrożeniu aplikacji Entity Framework Code First

### <a name="scenario"></a>Scenariusz

Wdrażasz aplikację korzystającą z migracje Code First platformy Entity Framework i systemu DBMS, takich jak SQL Server Compact, która przechowuje swoją bazę danych w pliku w folderze danych\_aplikacji. Migracje Code First skonfigurowany do tworzenia bazy danych po pierwszym wdrożeniu. Po uruchomieniu aplikacji zostanie wyświetlony komunikat o błędzie podobny do następującego:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Code First próbuje utworzyć bazy danych, ale aplikacja\_folderu danych nie istnieje. W aplikacji nie masz żadnych plików\_folderze *danych* podczas wdrażania lub wybrano opcję **wyklucz dane\_aplikacji** na karcie **pakiet/publikowanie w sieci Web** okna **właściwości projektu** . Proces wdrażania nie utworzy folderu na serwerze, jeśli w folderze nie ma plików do skopiowania na serwer. Jeśli baza danych została już skonfigurowana w lokacji, proces wdrażania spowoduje usunięcie plików i *aplikacji\_* samego folderu danych w przypadku wybrania opcji **Usuń dodatkowe pliki w miejscu docelowym** w profilu publikowania. Aby rozwiązać ten problem, Umieść plik zastępczy, taki jak plik. txt w folderze *danych\_aplikacji* , upewnij się, że nie wybrano pola **wykluczanie danych aplikacji\_** i ponowne wdrożenie. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Nie można użyć obiektu COM, który został oddzielony od jego podstawowej otoki RCW".

### <a name="scenario"></a>Scenariusz

Pomyślnie zainstalowano aplikację do wdrożenia za pomocą jednego kliknięcia, a następnie uruchamiasz ten błąd:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Zamknięcie i ponowne uruchomienie programu Visual Studio jest zwykle wszystkie wymagane do rozwiązania tego błędu.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Wdrożenie nie powiodło się, ponieważ poświadczenia użytkownika używane do publikowania nie mają urzędu setACL

### <a name="scenario"></a>Scenariusz

Publikowanie kończy się niepowodzeniem z powodu błędu, który wskazuje, że nie masz uprawnienia do ustawiania uprawnień do folderu (konto użytkownika, którego używasz, nie ma uprawnień setACL).

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie program Visual Studio ustawia uprawnienia do odczytu w folderze głównym witryny i uprawnienia do zapisu w folderze danych\_aplikacji. Jeśli wiadomo, że uprawnienia domyślne w folderach lokacji są poprawne i nie trzeba ich ustawiać, należy wyłączyć to zachowanie, dodając **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** do pliku profilu publikowania (w celu wpływu na pojedynczy profil) lub do pliku WPP. targets (aby mieć wpływ na wszystkie profile). Informacje o sposobach edytowania tych plików znajdują się [w temacie How to: Edit Deployment Settings in profil (. pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Błędy odmowy dostępu, gdy aplikacja próbuje zapisać do folderu aplikacji

### <a name="scenario"></a>Scenariusz

Wystąpił błąd aplikacji podczas próby utworzenia lub edycji pliku w jednym z folderów aplikacji, ponieważ nie ma on uprawnień do zapisu dla tego folderu.

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Domyślnie program Visual Studio ustawia uprawnienia do odczytu w folderze głównym witryny i uprawnienia do zapisu w folderze danych\_aplikacji. Jeśli aplikacja wymaga dostępu do zapisu w podfolderze, można ustawić uprawnienia dla tego folderu, jak pokazano w [ustawieniu uprawnienia folderów](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) i wdrażanie go w samouczkach [środowiska produkcyjnego](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . Jeśli aplikacja wymaga dostępu do zapisu w folderze głównym witryny, musisz uniemożliwić jej ustawienie dostępu tylko do odczytu w folderze głównym, dodając **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** do pliku profilu publikacji (aby mieć wpływ na pojedynczy profil) lub do pliku WPP. targets (aby mieć wpływ na wszystkie profile). Informacje o sposobach edytowania tych plików znajdują się [w temacie How to: Edit Deployment Settings in profil (. pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Błąd konfiguracji — atrybut targetFramework odwołuje się do wersji, która jest nowsza niż zainstalowana wersja .NET Framework

### <a name="scenario"></a>Scenariusz

Pomyślnie opublikowano projekt sieci Web, który jest przeznaczony dla ASP.NET 4,5, ale po uruchomieniu aplikacji (z trybem `customErrors` ustawionym na wartość "off" w pliku Web. config) zostanie wyświetlony następujący błąd:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

W polu błędu źródła strony błędu wyróżniono następujący wiersz z pliku Web. config jako przyczynę błędu:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Możliwa przyczyna i rozwiązanie

Serwer nie obsługuje ASP.NET 4,5. Skontaktuj się z dostawcą hostingu, aby określić, kiedy i w jaki sposób ma zostać dodana obsługa ASP.NET 4,5. Jeśli uaktualnienie serwera nie jest opcją, należy wdrożyć projekt sieci Web, który jest przeznaczony dla ASP.NET 4 lub wcześniejszych. Jeśli projekt sieci Web ASP.NET 4 lub wcześniejszy zostanie wdrożony w tym samym miejscu docelowym, zaznacz pole wyboru **Usuń dodatkowe pliki w miejscu docelowym** na karcie **Ustawienia** kreatora **publikacji w sieci Web** . Jeśli nie wybierzesz opcji **Usuń dodatkowe pliki w miejscu docelowym**, będziesz nadal otrzymywać stronę błędu konfiguracji.

Okna **Właściwości** projektu zawierają listę rozwijaną platformy docelowej, ale nie można rozwiązać tego problemu przez zmianę wartości z **.NET Framework 4,5** na **.NET Framework 4**. Jeśli zmienisz platformę docelową na wcześniejszą wersję Framework, projekt nadal będzie zawierał odwołania do zestawów wersji platformy w przyszłości i nie zostanie uruchomiony. Musisz ręcznie zmienić te odwołania lub utworzyć nowy projekt, który jest przeznaczony dla .NET Framework 4 lub wcześniejszych. Aby uzyskać więcej informacji, zobacz [.NET Framework określania elementów docelowych dla witryn sieci Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Wstecz](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
