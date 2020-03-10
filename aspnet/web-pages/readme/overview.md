---
uid: web-pages/readme/overview
title: Plik Readme WebMatrix | Microsoft Docs
author: rick-anderson
description: Informacje o wersji programu WebMatrix i ASP.NET Web Pages (Razor) 1,0
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: fac53e935860a90d8f2aa96699d56d66ade3a40f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563470"
---
# <a name="webmatrix-readme"></a>Plik Readme programu WebMatrix

13 stycznia 2011

## <a name="contents"></a>Spis treści

> [!NOTE]
> Ten plik Readme dotyczy wersji 1,0 programu WebMatrix.

- [Omówienie](#Overview)
- [Instalacja](#Installation_Notes)
- [Jak publikować aplikacje](#InstructionsForPublishingApplications)
- [Zmiany i problemy](#ChangesAndIssues)

    - [Instalacja programu WebMatrix 1,0](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [Programu WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalowanie aplikacji](#Known_Issues_Installing_Applications)
    - [Publikowanie aplikacji](#Known_Issues_Publishing_Applications)
- [Aby uzyskać więcej informacji](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Omówienie

> Microsoft WebMatrix 1,0 to bezpłatny stos rozwoju sieci Web, który jest instalowany w ciągu kilku minut. Integruje on serwer sieci Web z bazami danych i strukturami programowania w celu utworzenia jednego, zintegrowanego środowiska. Za pomocą programu WebMatrix można usprawnić wykonywanie kodu, testowanie i publikowanie własnej witryny sieci Web ASP.NET lub PHP lub użyć programu WebMatrix do uruchomienia nowej witryny sieci Web przy użyciu popularnych aplikacji typu "open source", takich jak DotNetNuke, Umbraco, WordPress lub Joomla. Program WebMatrix używa tego samego zaawansowanego serwera sieci Web, aparatu bazy danych i środowiska środowisk, które będą uruchamiać witrynę internetową w Internecie, co sprawia, że przejście od projektowania do produkcji płynnej i bezproblemowe.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalacja

> Aby zainstalować program WebMatrix 1,0, należy najpierw zainstalować [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Po zainstalowaniu Instalatora platformy sieci Web można go użyć do zainstalowania programu WebMatrix.
> 
> Jeśli masz problemy podczas instalacji, zapoznaj się z [tematem Rozwiązywanie problemów z Instalator platformy Microsoft Web](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Jak publikować aplikacje

> Zapoznaj się z [instrukcjami krok po kroku dotyczącymi publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Zmiany i problemy

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemy z instalacją programu WebMatrix 1,0

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: WebMatrix 1,0 jest dostępna tylko na platformach, które obsługują Microsoft .NET Framework 4

> W programie WebMatrix jest wymagany .NET Framework wersja 4. W niektórych przypadkach Instalator WebMatrix 1,0 pozwoli spróbować zainstalować program na platformie, która nie jest częścią obsługiwanego zestawu konfiguracyjnego. W szczególności system Windows Vista bez aktualizacji programu SP1 umożliwi rozpoczęcie instalacji programu WebMatrix, ale składnik .NET Framework 4 zakończy się niepowodzeniem i zablokuje instalację.
> 
> **Obejście**  
> Zainstaluj program na obsługiwanej platformie, w tym:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista z dodatkiem SP1 lub nowszym
> - Windows XP z dodatkiem SP3
> - Windows Server 2003 SP2

#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: nie można zainstalować programu WebMatrix 1,0, jeśli Microsoft Visual Studio 2008 jest zainstalowana bez Microsoft Visual Studio 2008 z dodatkiem SP1

> **Obejście**  
> Zainstaluj [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z centrum pobierania Microsoft.

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problem: Niektóre zestawy dla SQL Server Compact 4,0 nie są zainstalowane w pamięci podręcznej GAC

> Zestawy zarządzane dla SQL Server Compact 4,0 nie są umieszczane w globalnej pamięci podręcznej zestawów (GAC) podczas instalacji SQL Server Compact 4,0 na komputerze 64-bitowym, a komputer ma tylko .NET Framework zainstalowany profil klienta programu 3,5 z dodatkiem SP1. Zarządzane zestawy, które nie są zainstalowane w pamięci podręcznej GAC, to:
> 
> - *System. Data. SqlServerCe. dll* (dostawca ADO.NET)
> - *System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)
> 
> **Obejście**  
> Odinstaluj SQL Server Compact 4,0. Pobierz i zainstaluj pełną wersję .NET Framework 3,5 SP1 z następującej lokalizacji:  
>   
> [Microsoft .NET Framework 3,5 z dodatkiem Service Pack 1 (pełny pakiet)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Następnie ponownie zainstaluj SQL Server Compact 4,0.

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problem: nie można odinstalować SQL Server Compact przy użyciu wiersza polecenia

> Dezinstalacja SQL Server Compact przy użyciu opcji wiersza polecenia nie działa w tej wersji.
> 
> **Obejście**  
> Aby odinstalować Microsoft SQL Server Compact 4,0, użyj *apletu programy i funkcje* w panelu sterowania systemu Windows.

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Strony sieci Web programu ASP.NET

W tej sekcji dokumentu opisano nowe funkcje, zmiany i znane problemy w wersji 1,0 stron sieci Web ASP.NET składnia Razor z programem.

- [Nowe funkcje](#NewFeatures)
- [Wprowadzane](#Changes)
- [Problemy](#Issues)

#### <a id="NewFeatures"></a>Nowe funkcje

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nowość: dodano ustawienie konfiguracji w celu wyłączenia Menedżera pakietów

> Nowy klucz `asp:AdminManagerEnabled` jest dostępny dla elementu `<appSettings>` w pliku *Web. config* , który umożliwia całkowite wyłączenie Menedżera pakietów. Wartość domyślna dla tego elementu to true, co oznacza, że jeśli nie jest uwzględniona w pliku *Web. config* , Menedżer pakietów jest włączony. Aby wyłączyć menedżera pakietów, Dodaj następujący element do pliku *Web. config* w katalogu głównym witryny sieci Web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]

#### <a id="Changes"></a>Wprowadzane

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Zmiana: zmieniono nazwę klucza "Webpages: AdminFolderVirtualPath" na "ASP: AdminFolderVirtualPath"

> Klucz `webPages:AdminFolderVirtualPath`, który można dodać do pliku *Web. config* , aby określić lokalizację Menedżera pakietów została zmieniona na Użyj przestrzeni nazw `asp:` zamiast przestrzeni nazw `webPages`. Jeśli użyto tego elementu, należy zmienić jego nazwę w pliku konfiguracji.

#### <a id="Issues"></a> Znane problemy

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problem: hasła dla użytkowników członkostwa nie są już rozpoznawane

> Algorytm tworzenia i przechowywania haseł związanych z członkostwem (login) został zmieniony na bardziej bezpieczny. W związku z tym hasła przechowywane dla członków (użytkowników) utworzonych w wersjach beta ASP.NET Razor nie będą rozpoznawane. 
> 
> **Obejście problemu** Jeśli lokacja nie została jeszcze umieszczona w środowisku produkcyjnym, usuń rekordy użytkowników z bazy danych członkostwa. Jeśli baza danych jest na żywo, program programowo ponownie generuje istniejące hasła w bazie danych członkostwa.

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: nieoczekiwane zachowanie podczas korzystania z niestandardowej tabeli użytkownika na potrzeby członkostwa

> Aby zainicjować dostawcę członkostwa dla witryny sieci Web ASP.NET Razor, należy wywołać metodę `WebSecurity.InitializeDatabaseConnection`. (W programie WebMatrix szablon witryny Starter zawiera wywołanie tej metody w pliku *\_AppStart. cshtml* ). Jeśli parametr `autoCreateTables` tej metody ma wartość true (domyślnie jest ustawiona wartość true w szablonie witryny Starter), a jeśli Nierozpoznana nazwa tabeli jest przenoszona do metody (drugi parametr), metoda nie zgłasza błędu. Zamiast tego automatycznie tworzy tabelę.
> 
> Może to być problem, jeśli zamierzasz użyć niestandardowej tabeli użytkownika do członkostwa, ale Przekaż nieprawidłową nazwę tabeli do metody `WebSecurity.InitializeDatabaseConnection`. Ponieważ metoda nie jest domyślnie zgłaszana błędem, jeśli określona tabela nie istnieje i ponieważ zamiast niej tworzy nową tabelę, aplikacja może wydawać się niedziałająca. Jednak kod aplikacji, który zależy od niestandardowej tabeli użytkownika (i znajdujących się w nim pól) może zakończyć się niepowodzeniem z nieoczekiwanymi błędami.
> 
> **Obejście**  
> Upewnij się, że nazwa przenoszona przez metodę `InitializeDatabaseConnection` jest zgodna z tabelą profilu użytkownika w bazie danych członkostwa, lub upewnij się, że parametr `autoCreateTables` ma wartość false.

#### <a name="issue-error-message-the-admin-module-requires-access-to-app_data"></a>Problem: komunikat o błędzie "moduł administracyjny wymaga dostępu do ~/App\_danych"

> W pewnych okolicznościach próba utworzenia użytkowników lub pracy z systemem członkostwa ASP.NET może spowodować, że strona wyświetli błąd *moduł administracyjny wymaga dostępu do ~/App\_danych*. Dzieje się tak, jeśli konto, na którym są uruchomione usługi IIS lub IIS Express, nie ma uprawnień do tworzenia i zapisywania do folderu *danych\_aplikacji* w katalogu głównym witryny sieci Web. 
> 
> **Obejście problemu** Utwórz ręcznie folder *danych\_aplikacji* dla witryny sieci Web. Następnie upewnij się, że konto systemu Windows, w którym działa aplikacja (zazwyczaj usługa sieciowa) ma uprawnienia do odczytu i zapisu dla folderów głównych aplikacji oraz dla podfolderów, takich jak dane\_aplikacji. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy związane z SQL Server Express wystąpieniami użytkowników i projektami aplikacji sieci Web ASP.NET](https://support.microsoft.com/kb/2002980).

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "nie można wygenerować wystąpienia użytkownika SQL Server"

> Jeśli aplikacja sieci Web WebMatrix używa SQL Server Express i korzysta z usług IIS 7,5 w systemie Windows 7 lub Windows Server 2008 R2, może zostać wyświetlony komunikat o błędzie z informacją, że SQL Server nie może pobrać lokalnej ścieżki aplikacji użytkownika w czasie wykonywania.
> 
> **Obejście problemu** Upewnij się, że konto systemu Windows, w którym jest uruchomiona aplikacja (zazwyczaj usługa sieciowa) ma uprawnienia do odczytu i zapisu dla folderów głównych aplikacji oraz dla podfolderów, takich jak *aplikacja\_dane*. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy związane z SQL Server Express wystąpieniami użytkowników i projektami aplikacji sieci Web ASP.NET](https://support.microsoft.com/kb/2002980).

#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problem: pliki, które zawierają zasoby Menedżera pakietów lub hasła Menedżera pakietów, są objęte usługami IIS 6,0 i wcześniejszymi

> W przypadku wdrażania aplikacji ASP.NET Web Pages (Razor), która została skompilowana przy użyciu wersji RC2, a aplikacja zawiera plik *Password. txt* lub *packagesources. txt* w obszarze */App\_dane/administrator*, usługi IIS 6,0 będą obsłużyć plik, jeśli jest to wymagane, co może spowodować udostępnienie haseł dla wystąpienia Menedżera pakietów. 
> 
> **Obejście problemu** Zmień nazwę pliku *Password. txt* lub *packagesources. txt* na *Password. config* lub *packagesources. config*. Domyślnie usługi IIS 6,0 nie obsługują plików z rozszerzeniem *. config* . (W usługach IIS 7 nie są obsługiwane żadne pliki w *\_aplikacji folder danych* , więc nie trzeba zmieniać nazw plików).

#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problem: Odinstalowywanie pakietów zainstalowanych przy użyciu wersji beta 3 nie powoduje całkowitego usunięcia składników pakietu

> Jeśli zainstalowano pakiet za pomocą Menedżera pakietów w wersji beta 3, a następnie podjęto próbę odinstalowania go przy użyciu bieżącej wersji, pakiet nie zostanie całkowicie odinstalowany. Użycie przycisku **Odinstaluj** Menedżera pakietów powoduje usunięcie niektórych składników, ale pozostawia kod biblioteki pakietu i nie aktualizuje pliku *Package. config* .
> 
> **Obejście**   
> Wykonaj następujące kroki:  
> 1. Usuń folder *\_Data\packages aplikacji* . Spowoduje to usunięcie wszystkich pakietów.   
> 2. Usuń plik *Packages. config* w folderze głównym witryny sieci Web.

#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problem: w programie Visual Studio wywoływanie Menedżera pakietów opartych na sieci Web powoduje, że aplikacja jest w trybie offline

> Jeśli pracujesz w programie Visual Studio (nie WebMatrix) i używasz funkcji *administratora\_* , aby uruchomić Menedżera pakietów, program Visual Studio przełączy aplikację w tryb offline i opublikuje *aplikację\_w trybie offline. htm* w katalogu głównym witryny sieci Web, co zakłóca możliwość korzystania z Menedżera pakietów.
> 
> [!NOTE]
> Mimo że zwykle widzisz to zachowanie w przypadku korzystania z interfejsu Menedżera pakietów opartych na sieci Web, takie samo zachowanie występuje w przypadku dodawania, usuwania lub modyfikowania plików w folderze *dane\_aplikacji* .
> 
> **Obejście**   
> Aby korzystać z pakietów w programie Visual Studio, użyj rozszerzenia NuGet zamiast Menedżera pakietów opartych na sieci Web. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją programu NuGet](https://docs.microsoft.com/nuget/). Jeśli pracujesz z innymi plikami w folderze *danych\_aplikacji* , rozważ pozostawienie plików w innym miejscu, aby uniknąć tego problemu. Jeśli nie jest to praktyczne, Usuń *aplikację\_pliku offline. htm* ręcznie lub zaczekaj, aż lokacja przewróci do trybu online automatycznie (domyślnie po 30 sekundach).

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: program Visual Studio IntelliSense i szablony projektów są dostępne tylko w ASP.NET MVC w wersji 3

> Instalowanie stron sieci Web ASP.NET nie instaluje również narzędzi dla programu Visual Studio, takich jak IntelliSense i szablony projektów dla aplikacji ASP.NET Web Pages.
> 
> **Obejście problemu** Aby użyć funkcji IntelliSense i szablonów projektu dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstaluj ASP.NET MVC 3 RC przez Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: odczytywanie kanałów informacyjnych lub innych danych zewnętrznych za pośrednictwem serwera proxy

> Jeśli serwer z uruchomioną lokacją znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacji o serwerze proxy w pliku *Web. config* , aby można było odczytywać informacje pochodzące spoza witryny. Jeśli na przykład użyjesz pomocnika `ReCaptcha`, pomocnik komunikuje się z usługą reCAPTCHA, ale może być blokowany przez serwer proxy. Podobnie, kanały informacyjne, które są używane na stronach sieci Web ASP.NET, takie jak źródło danych używane przez Menedżera pakietów, mogą wymagać konfiguracji serwera proxy.
> 
> Jeśli wystąpią problemy podczas pracy z usługą zewnętrzną lub praca z kanałem informacyjnym pakietu, umieść następujące elementy w głównym pliku *Web. config* aplikacji:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Aby uzyskać więcej informacji o konfigurowaniu serwera proxy, zobacz [&lt;proxy&gt; element (Ustawienia sieci)](https://msdn.microsoft.com/library/sa91de1e.aspx) w witrynie MSDN w sieci Web.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Odinstalowywanie .NET Framework w wersji 4 wyłącza ASP.NET stron sieci Web ze składnią Razor

> Jeśli odinstalujesz .NET Framework w wersji 4, a następnie zainstalujesz ją ponownie, ASP.NET strony sieci Web z składnia Razor jest wyłączone. Strony z rozszerzeniem *. cshtml* nie działają poprawnie. Strony sieci Web ASP.NET rejestrują zestaw w głównym pliku *Web. config* maszyny, a usunięcie .NET Framework usuwa ten plik. Ponowne zainstalowanie .NET Framework instaluje nową wersję pliku konfiguracji, ale nie dodaje odwołania do zestawu stron sieci Web ASP.NET.
> 
> **Obejście problemu** Po ponownym zainstalowaniu .NET Framework ponownie zainstaluj strony sieci Web ASP.NET z składnia Razor. Spowoduje to dodanie następującego elementu do pliku *Web. config* w katalogu głównym maszyny, który zwykle znajduje się w następującej lokalizacji:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problem: w przypadku adresów URL bez rozszerzenia nie znajdują się pliki cshtml/. vbhtml w usługach IIS 7 lub IIS 7,5

> W przypadku usług IIS 7 lub IIS 7,5 żądania z adresem URL podobnym do następującego nie mogą znaleźć stron, które mają rozszerzenie *. cshtml* lub *. vbhtml* :  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Problem występuje, ponieważ ponowne zapisywanie adresów URL nie jest domyślnie włączone dla usług IIS 7 lub IIS 7,5. Likeliest scenariusz polega na tym, że problem nie jest wyświetlany podczas testowania lokalnego przy użyciu IIS Express, ale podczas wdrażania witryny sieci Web w witrynie sieci Web hostowanie jest to możliwe.
> 
> **Obejście**
> 
> - Jeśli masz kontrolę nad komputerem serwera, na komputerze serwera Zainstaluj aktualizację, która jest opisana w [aktualizacji, która umożliwia obsługę określonych usług iis 7,0 lub iis 7,5 obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).
> - Jeśli nie masz kontroli nad komputerem serwera (na przykład wdrażasz je w witrynie sieci Web hostingu), Dodaj następujący kod do pliku *Web. config* witryny sieci Web: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: wdrażanie aplikacji na komputerze, na którym nie zainstalowano SQL Server Compact

> Aplikacje zawierające SQL Server Compact bazy danych można uruchamiać na komputerze, na którym SQL Server Compact nie jest zainstalowana. Firma Microsoft WebMatrix 1,0 automatycznie kopiuje te pliki binarne i wykonuje transformacje pliku *Web. config* .
> 
> **Obejście problemu** Jeśli musisz skopiować te pliki i ręcznie wprowadzić zmiany w pliku *Web. config* , wykonaj następujące czynności:
> 
> 1. Skopiuj zestawy aparatów bazy danych do folderu *bin* (i podfolderów) aplikacji na komputerze docelowym:  
> 
>    - Kopiuj *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>      **do** *\Bin*
>    - Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **do** *\Bin\x86*
>    - Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **do** *\Bin\amd64*
> 
> 2. W folderze głównym witryny sieci Web Utwórz lub Otwórz plik *Web. config* . (W programie WebMatrix 1,0 ten typ pliku jest dostępny po kliknięciu przycisku **wszystkie** w oknie dialogowym **Wybierz typ pliku** ).
> 3. Dodaj następujący element jako element podrzędny elementu `<configuration>` (nie wewnątrz elementu `<system.web>`):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: pomocniki "Database" i "WebGrid" nie działają w średnim zaufaniu w Visual Basic

> Jeśli używasz Visual Basic (Tworzenie plików *. vbhtml* ), pomocniki `Database` i `WebGrid` nie będą działały, jeśli aplikacja jest ustawiona do używania średniego zaufania.
> 
> **Obejście**  
> Jeśli używasz programu Visual Studio 2010, możesz rozwiązać ten problem, instalując dodatek Service Pack 1. Do momentu udostępnienia ostatecznej wersji programu SP1 można pobrać wersję beta programu SP1 ze strony [Microsoft Visual Studio 2010 Service Pack 1 beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) w centrum pobierania Microsoft.   
>   
> Jeśli nie jest to praktyczne, lub jeśli nie korzystasz z programu Visual Studio 2010, możesz tymczasowo ustawić aplikację tak, aby korzystała z pełnego zaufania.

#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problem: zasoby "ApplicationPart" są dostępne na zewnątrz

> Jeśli zestaw zawiera obiekty, które pochodzą z klasy `ApplicationPart`, zasoby tego zestawu są uwidaczniane przez klasę `ResourceRouteHandler`. Rozważmy na przykład następujący adres URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> To żądanie pobiera wszystkie ciągi zasobów w zestawie *System. Web. Webpages. Administration. dll* . Pobierane są wszystkie zasoby osadzone (nawet te, które nie są przeznaczone do użycia jako zawartość statyczna). Jeśli zasoby osadzone zawierają informacje poufne, może to stanowić zagrożenie dla bezpieczeństwa. 
> 
> **Obejście**   
> W przypadku tworzenia obiektu **ApplicationPart** upewnij się, że osadzone zasoby skojarzone z tym zestawem obiektu **ApplicationPart** nie zawierają informacji poufnych.

<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Aby uzyskać informacje o problemach z instalacją w programie WebMatrix, zobacz [problemy z instalacją WebMatrix](#Known_Issues_Installation) wcześniej w tym dokumencie.

W tej sekcji dokumentu opisano znane problemy dotyczące środowiska deweloperskiego WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problem: zmiany nazwy użytkownika lub hasła parametrów połączenia z bazą danych w pliku Web. config nie są odzwierciedlone w obszarze roboczym bazy danych

> **Obejście**  
> 
> 1. W pliku *Web. config* Zmień nazwę bazy danych w parametrach połączenia (na przykład Dodaj do niej "1").
> 2. Zapisz plik *Web. config* .
> 3. Kliknij pozycję **bazy danych** i Odśwież.
> 4. Zmień nazwę bazy danych w parametrach połączenia w pliku *Web. config* z powrotem na oryginalną nazwę bazy danych.
> 5. Zapisz plik *Web. config* .
> 6. Kliknij pozycję **bazy danych** i Odśwież.

#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problem: nie można usunąć folderów utworzonych przez program WebMatrix

> Jeśli program WebMatrix jest uruchomiony przy użyciu podwyższonych uprawnień (to oznacza, że uruchomiono polecenie WebMatrix przy użyciu opcji **Uruchom jako administrator** w systemie Windows), foldery tworzone przez program WebMatrix nie mogą zostać usunięte za pomocą Eksploratora Windows.
> 
> **Obejście**  
> Uruchom Eksploratora Windows przy użyciu podwyższonych uprawnień. Wykonaj następujące kroki:  
> 
> 1. W systemie Windows kliknij przycisk **Uruchom**.
> 2. Wprowadź "Eksplorator Windows" i kliknij prawym przyciskiem myszy wpis dla **Eksploratora Windows**.
> 3. Kliknij przycisk **Uruchom jako administrator**. Następnie można usunąć foldery.

#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: w programie WebMatrix 1,0 nie można wykonać niektórych zadań wymagających podniesienia uprawnień

> W programie WebMatrix 1,0 nie jest możliwe wykonanie pewnych zadań wymagających podniesienia uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:
> 
> - W systemie Windows Vista lub Windows 7 użytkownik zalogował się przy użyciu konta, które nie ma uprawnień administracyjnych i kontrola konta użytkownika (UAC) jest wyłączona.
> - Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.
> 
> **Obejście**  
> Większość zadań w programie WebMatrix 1,0 nie wymaga uprawnień administracyjnych. Dla tych, które wykonują operację jako administrator, lub wykonaj następujące czynności:
> 
> - W systemie Windows Vista lub Windows 7 Włącz kontrolę konta użytkownika.
> - W systemie Windows XP Dodaj użytkownika do grupy zabezpieczeń Administratorzy.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Witryna z galerii sieci Web" jest wyłączona

> Opcja **Witryna z galerii sieci Web** jest wyłączona, jeśli nie zainstalowano Instalatora platformy sieci Web 3,0.
> 
> **Obejście**  
> Zainstaluj [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problem: Google Chrome nie jest dostępna jako opcja uruchamiania

> Przeglądarka Google Chrome nie jest wyświetlana na liście przeglądarek w obszarze **Uruchom** na karcie **Narzędzia główne** .
> 
> **Obejście**  
> Niektóre wersje usługi Google Chrome nie rejestrują się prawidłowo przy użyciu funkcji domyślnych programów w systemie Windows. Aby obejść ten element, uruchom program Google Chrome, kliknij menu *Dostosowywanie i kontrolowanie Google Chrome* , kliknij pozycję *Opcje*, a następnie kliknij pozycję *Udostępnij Google Chrome moją domyślną przeglądarkę*.

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problem: okno dialogowe "klucz obcy" nie zezwala na wprowadzenie klucza podstawowego

> Okno dialogowe **klucz obcy** nie pozwala na wprowadzenie nazwy klucza podstawowego z tabeli klucza podstawowego.
> 
> **Obejście**  
> Jest to zamierzone. Nie trzeba wprowadzać nazwy klucza podstawowego z tabeli klucza podstawowego.

#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problem: Funkcja IntelliSense nie jest dostępna w programie WebMatrix dla C#składnia Razor, lub Visual Basic

> Technologia IntelliSense jest obsługiwana w programie WebMatrix dla HTML i CSS. Nie jest to jednak dostępne w innych językach. 
> 
> **Obejście**   
> Brak.

#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problem: Funkcja IntelliSense dla HTML i CSS sugeruje elementy, które nie są kontekstowe.

> Funkcja IntelliSense for Markup w programie WebMatrix obsługuje kod HTML przy użyciu [schematu przejściowego XHTML 1,0](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) i CSS przy użyciu [schematu CSS 2,1](http://www.w3.org/TR/CSS2/). Ponieważ technologia IntelliSense jest oparta na tych konkretnych schematach, można zasugerować niektóre Tagi, atrybuty lub właściwości, które nie są odpowiednie dla bieżącej definicji strony lub stylu. W przypadku języka HTML może również prowadzić do nieoczekiwanych sugestii zawartości, które mogą być interpretowane jako źle sformułowane XHTML (na przykład gdy Tagi nie są zamknięte). Ten problem może być bardziej zauważalny, gdy punkt wstawiania znajduje się wewnątrz niekompletnego tagu; w takim przypadku technologia IntelliSense może zasugerować nowe Tagi otwierające lub zaoferować inne błędne sugestie. 
> 
> **Obejście**   
> W przypadku języka HTML upewnij się, że Pracujesz w dobrze sformułowanej, kompletnej stronie XHTML. W przypadku CSS nie istnieje obejście tego problemu.

#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problem: Funkcja IntelliSense nie jest wywoływana podczas wpisywania

> Czasami funkcja IntelliSense może nie zostać wywołana, ponieważ w edytorze wprowadzono kod HTML lub CSS. W szczególności może się to zdarzyć, gdy punkt wstawiania znajduje się bezpośrednio obok innego elementu lub na końcu pliku. 
> 
> **Obejście**   
> Upewnij się, że występuje odstęp wokół punktu wstawiania i że punkt wstawiania nie znajduje się na końcu pliku. Możesz również ręcznie wywołać technologię IntelliSense, naciskając klawisze CTRL + SPACJA.

#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problem: Brak dostępnego interfejsu użytkownika do wyłączania funkcji IntelliSense

> Program WebMatrix 1,0 nie zapewnia interfejsu użytkownika ani gestu do wyłączania funkcji IntelliSense. 
> 
> **Obejście**   
> Uruchom program WebMatrix przy użyciu następującego polecenia, które obejmuje przełącznik, który wyłącza funkcję IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`

<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>Usługi IIS Express

IIS Express ma swój własny plik Readme, który jest dostępny pod następującym adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact ma swój własny plik Readme, który jest dostępny pod następującym adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Aby uzyskać informacje o problemach związanych z instalowaniem SQL Server Compact w ramach programu WebMatrix, zobacz [problemy dotyczące instalacji programu WebMatrix](#Known_Issues_Installation) wcześniej w tym dokumencie.

### <a id="Known_Issues_Installing_Applications"></a>Instalowanie aplikacji

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: instalacja aplikacji może zająć dużo czasu, jeśli folder Moje dokumenty użytkownika zostanie przekierowany do udziału sieciowego

> **Obejście**  
> Brak. Instalacja aplikacji może potrwać trochę czasu, ale instalacja zostanie poprawna.

### <a id="Known_Issues_Publishing_Applications"></a>Publikowanie aplikacji

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problem: "nie można pobrać wymaganych uprawnień" podczas publikowania bazy danych SQL Compact

> Program WebMatrix nie obsługuje w pełni wdrażania pomocniczych plików binarnych dla SQL Server Compact na serwerze z uruchomionym .NET Framework wersji 3,5 z konfiguracją średniego zaufania.
> 
> **Obejście**  
> Zalecane obejście polega na zainstalowaniu na serwerze .NET Framework 4. Alternatywnie wykonaj następujące czynności:
> 
> 1. Dodaj następujące elementy do sekcji `SecurityClasses` w pliku *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Utwórz nowy zestaw uprawnień w pliku *Web\_MediumTrust. config* z następującymi wymaganymi uprawnieniami:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Zastosuj zestaw uprawnień do SQL Server Compact, umieszczając następujące elementy w pliku *\_MediumTrust. config sieci Web* :
> 
>     [!code-html[Main](overview/samples/sample8.html)]

#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problem: w galerii i aplikacjach sieci Web PhpBB jest wyświetlany komunikat o błędzie "usługa jest niedostępna" po opublikowaniu

> W pewnych okolicznościach opublikowanie aplikacji powoduje błąd "usługa jest niedostępna".
> 
> **Obejście**  
> W programie WebMatrix Dodaj ukośnik odwrotny (\) na końcu nazwy serwera w oknie **Ustawienia publikowania** , a następnie ponownie Opublikuj aplikację.

#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problem: układ i linki witryny sieci Web Moodle są zerwane po opublikowaniu

> Po opublikowaniu aplikacji Moodle aplikacja nie działa prawidłowo.
> 
> **Obejście**  
> W programie WebMatrix Dodaj ukośnik (/) na końcu pola **Nazwa lokacji** w oknie **Ustawienia publikowania** , a następnie ponownie Opublikuj aplikację.

#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problem: publikowanie witryny systemu NopCommerce kończy się niepowodzeniem z powodu błędu bazy danych

> Publikowanie witryny systemu NopCommerce kończy się niepowodzeniem i zgłasza błąd bazy danych, na przykład "Wstaw do tabeli dziennika\_NOP."
> 
> **Obejście**  
> 
> 1. W programie WebMatrix kliknij pozycję **Uruchom** , aby uruchomić witryny systemu NopCommerce lokalnie.
> 2. Zaloguj się na stronie Administracja.
> 3. Kliknij menu **system** .
> 4. Kliknij opcję **Dziennik** .
> 5. Kliknij przycisk **Wyczyść dziennik**.
> 6. Ponownie Opublikuj witryny systemu NopCommerce.

#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problem: podczas pobierania opublikowanej witryny w usłudze CMS SilverStripe jest wyświetlany komunikat o błędzie "HTTP 500 PHP FCGI"

> **Obejście**  
> Po kliknięciu przycisku **Pobierz opublikowaną witrynę**Pomiń `silverstripe-cache/manifest_main` w **wersji zapoznawczej publikowania**. Ten plik jest używany na potrzeby buforowania i jest przeznaczony dla każdego komputera.

#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problem: w przypadku pobierania opublikowanej witryny w polu "błąd serwera w aplikacji"/"jest wyświetlany komunikat o błędzie

> **Obejście**  
> Otwórz plik *Web. config* witryny i Zastąp identyfikator użytkownika i hasło w parametrach połączenia z bazą danych poświadczeniami administratora SQL Server (poświadczeniami "sa").
> 
> Alternatywnie wykonaj następujące kroki, aby nawiązać konto użytkownika, za pomocą którego zalogowano się `db_owner` uprawnień:
> 
> 1. Zainstaluj SQL Server Management Studio przy użyciu Instalatora platformy sieci Web.
> 2. Połącz się z lokalnym wystąpieniem SQL Server Express (domyślnie `.\SQLEXPRESS`).
> 3. Kliknij kolejno pozycje **bazy danych** &gt; *[LocalSubtextDatabase]* &gt; **zabezpieczenia** &gt; **Użytkownicy** &gt; *[localSubtextUser*] (wartość domyślna to `subtextuser`], kliknij prawym przyciskiem myszy, a następnie kliknij pozycję **Właściwości**.
> 4. Wybierz pozycję **db\_Owner** w sekcji członkostwo w roli.

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problem: witryna może nie zadziałać po opublikowaniu, jeśli pole "docelowy adres URL" nie jest poprzedzone prefiksem http://lub https://

> Jeśli docelowy adres URL nie zaczyna się od `http://` lub `https://`, w oknie dialogowym **Ustawienia publikowania** witryna może nie zadziałać po wdrożeniu.
> 
> **Obejście**  
> Przed opublikowaniem witryny upewnij się, że docelowy adres URL w oknie dialogowym **Ustawienia publikowania** zaczyna się od `http://` lub `https://`.

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problem: Publikowanie bazy danych MySQL kończy się niepowodzeniem z powodu błędu "nie można opublikować bazy danych. Może się tak zdarzyć, jeśli zdalna baza danych nie może uruchomić skryptu ".

> Ten błąd może wystąpić z kilku powodów. Jedną z przyczyn tego błędu jest to, że ten błąd występuje, jeśli skrypt bazy danych zawiera znak pojedynczego cudzysłowu ('), a docelowy zestaw znaków bazy danych MySQL nie jest w formacie UTF-8.
> 
> **Obejście**  
> Ustaw domyślny zestaw znaków dla zdalnej bazy danych MySQL na UTF-8.

#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problem: niektóre linki nie są widoczne w programie DotNetNuke po opublikowaniu lub pobraniu witryny

> Jeśli publikujesz lub pobierasz witrynę DotNetNuke, może być konieczne wyczyszczenie pamięci podręcznej w celu pobrania nowych linków do wyświetlania w witrynie.
> 
> **Obejście**
> 
> 1. Zaloguj się jako "host".
> 2. Przejdź do menu hosta, a następnie wybierz pozycję **Ustawienia hosta**.
> 3. Przewiń w dół i w obszarze **Ustawienia zaawansowane**, rozwiń węzeł **Ustawienia wydajności**.
> 4. Kliknij link **Wyczyść pamięć podręczną** dla stron.
> 5. Przejdź do dolnej części strony i ponownie uruchom aplikację.

#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problem: niektóre linki w programie AtomSite są uszkodzone po pobraniu opublikowanej witryny

> **Obejście**  
> W pliku *Service. config* plik *users. config* , a wszystkie pliki *. XML* , Zastąp ciąg adresu URL (na przykład `http://myhost.com/atomsite`) lokalnym (na przykład `http://localhost:1239`).

#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problem: w przypadku aplikacji opartych na bazie MySQL, takich jak WordPress, nie można opublikować i zgłosić błędu bazy danych

> Domyślnie program WebMatrix instaluje MySQL z zestawem znaków UTF-8. W przypadku instalowania programu MySQL we własnym zestawie znaków nie jest to UTF-8 (na przykład Latin1), proces publikowania dla baz danych może zakończyć się niepowodzeniem.
> 
> **Obejście**
> 
> 1. Zmień zestaw znaków dla programu MySQL na UTF-8. (Aby uzyskać szczegółowe informacje, zobacz [zestaw znaków serwera i sortowanie](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) w witrynie MySQL).
> 2. Zainstaluj ponownie aplikację.
> 3. Opublikuj ponownie aplikację.

#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problem: "Pobierz opublikowaną witrynę" kończy się niepowodzeniem w przypadku aplikacji korzystających z konfiguracji opartej na przeglądarce

> Niektóre aplikacje (na przykład Kentico CMS) wymagają uruchomienia ich w przeglądarce w celu przeprowadzenia instalacji po instalacji, takiej jak tworzenie bazy danych. Jeśli opublikujesz aplikację, taką jak ta, bez kończenia konfiguracji opartej na przeglądarce, próba pobrania tej samej lokacji z serwera zdalnego zakończy się niepowodzeniem.
> 
> **Obejście**  
> Zakończ konfigurację opartą na przeglądarce przed opublikowaniem lokacji.

#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problem: "Pobieranie opublikowanej witryny" kończy się niepowodzeniem z powodu błędu bazy danych dla DotNetNuke i Kooboo CMS

> Jeśli spróbujesz pobrać aplikację z serwera i masz poświadczenia administratora w parametrach połączenia bazy danych w oknie dialogowym **Ustawienia publikowania** , w dzienniku publikacji może zostać wyświetlony następujący błąd:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Obejście**  
> W razie potrzeby należy ponownie opublikować lokację (lub opublikować ją) przy użyciu poświadczeń innych niż administrator dla bazy danych.

<a id="More_Info"></a>

## <a name="for-more-information"></a>Aby uzyskać więcej informacji

Aby uzyskać więcej informacji o programie WebMatrix 1,0, zobacz następujące witryny sieci Web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Wszelkie prawa zastrzeżone. [Warunki użytkowania](https://msdn.microsoft.cos/cc300389.aspx).
