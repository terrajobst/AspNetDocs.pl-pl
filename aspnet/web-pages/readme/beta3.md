---
uid: web-pages/readme/beta3
title: Web Matrix i ASP.NET Web Pages (Razor) beta 3 wersja Readme | Microsoft Docs
author: rick-anderson
description: Plik Readme dla programu WebMatrix i wzorca ASP.NET Web Pages (Razor) w wersji Beta 3
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628899"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Plik Readme dla programu WebMatrix i wzorca ASP.NET Web Pages (Razor) w wersji Beta 3

> Plik Readme dla programu WebMatrix i wzorca ASP.NET Web Pages (Razor) w wersji Beta 3

9 listopada 2010

## <a name="contents"></a>Spis treści

- [Omówienie](#Overview)
- [Instalacja](#Installation_Notes)
- [Nowe funkcje, zmiany i znane problemy w wersji beta 3](#Known_Issues)

    - [Problemy z instalacją programu WebMatrix](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalowanie aplikacji](#Known_Issues_Installing_Applications)
    - [Publikowanie aplikacji](#Known_Issues_Publishing_Applications)
    - [Inne problemy](#Known_Issues_Other_Issues)
- [Aby uzyskać więcej informacji](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Omówienie

> Microsoft WebMatrix beta to bezpłatny stos rozwoju sieci Web, który jest instalowany w ciągu kilku minut. Integruje on serwer sieci Web z bazami danych i strukturami programowania w celu utworzenia jednego, zintegrowanego środowiska. Za pomocą programu WebMatrix beta można usprawnić wykonywanie kodu, testować i publikować własną witrynę sieci Web ASP.NET lub PHP lub użyć programu WebMatrix beta do uruchamiania nowej witryny sieci Web przy użyciu popularnych aplikacji typu "open source", takich jak DotNetNuke, Umbraco, WordPress lub Joomla. Program WebMatrix beta korzysta z tego samego zaawansowanego serwera sieci Web, aparatu bazy danych i środowiska struktur, które będą uruchamiać witrynę internetową w Internecie, co sprawia, że przejście od projektowania do produkcji jest płynne i bezproblemowe.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalacja

> Aby zainstalować program WebMatrix beta 3, należy użyć [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Po zainstalowaniu Instalatora platformy sieci Web można go użyć do zainstalowania programu WebMatrix beta 3.
> 
> Jeśli masz problemy podczas instalacji, zapoznaj się z [tematem Rozwiązywanie problemów z Instalator platformy Microsoft Web](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instrukcje dotyczące publikowania aplikacji

> Zapoznaj się z [instrukcjami krok po kroku dotyczącymi publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nowe funkcje, zmiany, andKnown problemy

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalacja programu WebMatrix beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: WebMatrix beta 3 jest dostępna tylko na platformach, które obsługują Microsoft .NET Framework 4

> W programie WebMatrix beta jest wymagany .NET Framework wersja 4. W niektórych przypadkach Instalator programu WebMatrix beta podejmie próbę instalacji na platformie, która nie jest częścią obsługiwanego zestawu konfiguracyjnego. W szczególności system Windows Vista bez aktualizacji programu SP1 umożliwi rozpoczęcie instalacji programu WebMatrix beta, ale składnik .NET Framework 4 zakończy się niepowodzeniem i zablokuje instalację.
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

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: nie można zainstalować programu WebMatrix beta 3, jeśli Microsoft Visual Studio 2008 jest zainstalowana bez Microsoft Visual Studio 2008 SP1

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

W tej sekcji dokumentu opisano nowe funkcje, zmiany i znane problemy w wersji beta 3 ASP.NET stron sieci Web z programem składnia Razor.

- [Nowe funkcje](#NewFeatures)
- [Wprowadzane](#Changes)
- [Problemy](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nowe funkcje w wersji beta 3 dla ASP.NET stron sieci Web ze składnią Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>New: Metoda "html. RAW" renderuje niezakodowane znaczniki

> Nowa metoda `Html.Raw` umożliwia renderowanie znaczników HTML jako znaczników zamiast renderowania zakodowanych danych wyjściowych. (Domyślnie ASP.NET Razor koduje ciągi przed renderowaniem.) Składnia jest następująca:
> 
> `Html.Raw(value)`
> 
> Poniższy przykład pokazuje, jak używać `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Zmiany w wersji beta 3 dla ASP.NET stron sieci Web ze składnią Razor

#### <a name="change-hrefattribute-method-removed"></a>Zmiana: Metoda "Hrefattribute" została usunięta

> Metoda `HrefAttribute` klasy `WebPage` została usunięta. Ten pomocnik został użyty do zakodowania niebezpiecznych znaków w adresach URL. Nie jest już wymagane, ponieważ ASP.NET Razor automatycznie koduje ciągi. (Użyj nowej metody `Html.Raw`, aby renderować niezakodowane ciągi).

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Zmiana: Składnia niezmienionych pomocników "@helper"

> W wersji beta 3 ASP.NET zmienia sposób analizowania pomocników, które są tworzone przy użyciu składni `@helper`. W zasadzie składnia `@helper` jest teraz analizowana jako blok kodu, a nie jako blok znaczników, które mogą zawierać kod. W związku z tym kod wewnątrz pomocnika nie musi być ujęty w bloki `@{ }`. Odwrotnie, Adiustacja wewnątrz pomocnika musi być jawnie dołączona do elementów HTML lub w tagach ASP.NET Razor `<text></text>`.
> 
> Na przykład następująca składnia `@helper` działa w wersji beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> W wersji beta 3 należy zmienić ten pomocnik, aby wyglądał jak w poniższym przykładzie:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Należy zauważyć, że `@{ }` znaki wokół początkowego kodu w Pomocniku nie są już używane. Dzieje się tak, ponieważ zawartość pomocników jest traktowana jako blok kodu domyślnie. Pomocnik renderuje znacznik, który rozpoczyna się od tagu otwierającego `<a>`. Jeśli pomocnik musi renderować zwykły tekst lub Tagi, które nie zawierają taga zamykającego (na przykład tagów `<meta>`), zawartość do renderowania musi znajdować się w `<text></text>` tagach.

#### <a name="change-webpagecontexthttpcontext-removed"></a>Change: "WebPageContext.HttpContext" removed

> Właściwość `WebPageContext.HttpContext` została usunięta. Zamiast tego użyj polecenia cmdlet `HttpContext.Current`. (Właściwość `WebPageContext.HttpContext` po prostu zawinięte).

#### <a name="change-facebook-helper-moved-to-new-package"></a>Zmiana: pomocnik "Facebook" został przeniesiony do nowego pakietu

> Pomocnik `Facebook` został przeniesiony do biblioteki *Facebook. pomocnika* , w tym pomocnika `Facebook` i dodatkowych funkcji. Tę bibliotekę należy zainstalować jako oddzielny pakiet, zgodnie z opisem w temacie "Instalowanie pomocników z menedżerem pakietów" w samouczku [wprowadzenie ze stronami ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Zmiana: członkostwo, rola i typy zabezpieczeń są przenoszone do nowego zestawu

> Następujące typy zostały przeniesione do zestawu `WebMatrix.WebData`:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Zmiana: Klasa "TagBuilder" została przeniesiona do zestawu System. Web. Webpages. dll

> Klasa języka r `TagBuilde` została przeniesiona do zestawu System. Web. Webpages. dll. Wcześniej było to w zestawie, który był częścią ASP.NET MVC. Ta zmiana oznacza, że nie trzeba instalować ASP.NET MVC, aby można było użyć klasy `TagBuilder`.
> 
> Jednak Klasa nadal znajduje się w przestrzeni nazw `System.Web.Mvc`. Aby można było użyć klasy `TagBuilder` (na przykład w niestandardowym Pomocniku Razor ASP.NET), należy odwołać się do przestrzeni nazw (na przykład przez dodanie `@using System.Web.Mvc` do kodu).

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Zmiana: zmieniono składnię walidacji żądania; Usunięto klasę "Walidacja"

> W wersji beta 3, aby wyłączyć walidację dla pojedynczego pola lub zestawu pól, można wywołać metodę `Validation.Exclude`, przekazując nazwę lub nazwy pól, które mają zostać wykluczone z walidacji. Nowa składnia jest dostępna w wersji beta 3 w celu obejścia walidacji. Metoda `Validation` użyta w wersji beta 3 została usunięta.
> 
> > [!NOTE]
> > Jeśli nie wyłączysz walidacji żądania, jeśli użytkownicy spróbują przekazać znaczniki HTML (na przykład przy użyciu edytora tekstu sformatowanego na stronie), witryna internetowa zgłosi błąd, jak *potencjalnie niebezpieczne żądanie. wartość formularza została wykryta przez klienta* , a dane wejściowe użytkownika nie są akceptowane. Jeśli wyłączysz weryfikację żądań, musisz ręcznie sprawdzić dane wprowadzane przez użytkownika, aby upewnić się, że nie zawiera potencjalnie niebezpiecznego znacznika lub skryptu korzystającego ze podobnej do [biblioteki skryptów Microsoft Anti-Cross Site Scripting Library v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Aby wyłączyć automatyczne sprawdzanie poprawności żądań, wywołaj metodę `Request.Unvalidated`, przekazując ją do nazwy pola lub innego obiektu post, dla którego chcesz pominąć sprawdzanie poprawności żądania. Tej metody można użyć do obejścia walidacji dla wszystkich elementów w `Form`, `QueryString`, `Cookies`i `ServerVariables` kolekcji. W poniższych przykładach pokazano, jak używać metody `Unvalidated`:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Znane problemy dotyczące ASP.NET stron sieci Web ze składnią Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: nieoczekiwane zachowanie podczas korzystania z niestandardowej tabeli użytkownika na potrzeby członkostwa

> Aby zainicjować dostawcę członkostwa dla witryny sieci Web ASP.NET Razor, należy wywołać metodę `WebSecurity.InitializeDatabaseConnection`. (W programie WebMatrix szablon witryny Starter zawiera wywołanie tej metody w pliku *\_AppStart. cshtml* ). Jeśli parametr `autoCreateTables` tej metody ma wartość true (domyślnie jest ustawiona wartość true w szablonie witryny Starter), a jeśli Nierozpoznana nazwa tabeli jest przenoszona do metody (drugi parametr), metoda nie zgłasza błędu. Zamiast tego automatycznie tworzy tabelę.
> 
> Może to być problem, jeśli zamierzasz użyć niestandardowej tabeli użytkownika do członkostwa, ale Przekaż nieprawidłową nazwę tabeli do metody `WebSecurity.InitializeDatabaseConnection`. Ponieważ metoda nie jest domyślnie zgłaszana błędem, jeśli określona tabela nie istnieje i ponieważ zamiast niej tworzy nową tabelę, aplikacja może wydawać się niedziałająca. Jednak kod aplikacji, który zależy od niestandardowej tabeli użytkownika (i znajdujących się w nim pól) może zakończyć się niepowodzeniem z nieoczekiwanymi błędami.
> 
> **Obejście**  
> Upewnij się, że nazwa przenoszona przez metodę `InitializeDatabaseConnection` jest zgodna z tabelą profilu użytkownika w bazie danych członkostwa, lub upewnij się, że parametr `autoCreateTables` ma wartość false.

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "nie można wygenerować wystąpienia użytkownika SQL Server"

> Jeśli aplikacja sieci Web WebMatrix używa SQL Server Express i korzysta z usług IIS 7,5 w systemie Windows 7 lub Windows Server 2008 R2, może zostać wyświetlony komunikat o błędzie z informacją, że SQL Server nie może pobrać lokalnej ścieżki aplikacji użytkownika w czasie wykonywania.
> 
> **Obejście problemu** Upewnij się, że konto systemu Windows, w którym jest uruchomiona aplikacja (zazwyczaj usługa sieciowa) ma uprawnienia do odczytu i zapisu dla folderów głównych aplikacji oraz dla podfolderów, takich jak *aplikacja\_dane*. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy związane z SQL Server Express wystąpieniami użytkowników i projektami aplikacji sieci Web ASP.NET](https://support.microsoft.com/kb/2002980).

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problem: w programie Visual Studio obszary nazw dla zestawów niestandardowych (dll) nie są importowane automatycznie

> Jeśli używasz niestandardowych zestawów w projekcie w programie Visual Studio, obszary nazw zadeklarowane w tych zestawach nie są automatycznie importowane w czasie projektowania. W związku z tym odwołania do typów niestandardowych mogą nie być rozpoznawane w czasie projektowania i są oznaczane jako nierozpoznane w programie Visual Studio (przy użyciu "zygzak"). Ten problem występuje tylko w czasie projektowania w programie Visual Studio; sama aplikacja działa prawidłowo.
> 
> **Obejście**  
> Dołącz instrukcję `using` (`imports` w Visual Basic), która odwołuje się do jednostek, które nie są rozpoznawane w czasie projektowania.

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: program Visual Studio IntelliSense i szablony projektów są dostępne tylko w ASP.NET MVC w wersji 3

> Instalowanie stron sieci Web ASP.NET nie instaluje również narzędzi dla programu Visual Studio, takich jak IntelliSense i szablony projektów dla aplikacji ASP.NET Web Pages.
> 
> **Obejście problemu** Aby użyć funkcji IntelliSense i szablonów projektu dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstaluj ASP.NET MVC 3 RC przez Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problem: nie można odnaleźć klasy&gt; pomocnika&lt;"błąd

> Po uaktualnieniu do wersji beta 3 może zostać wyświetlony błąd, że nie można odnaleźć klasy pomocnika (na przykład klasy `Facebook`). Począwszy od wersji beta 2 i kontynuując wersję beta 3, pomocnicy zostały przesunięte do pakietów, które należy jawnie zainstalować. Istniejące lokacje nie są uaktualnione w celu uwzględnienia tych pakietów; obejmuje to lokacje w folderach *\Moje Documents\IISExpress* lub *\Moje Dokumenty\moje witryny sieci Web* . W szczególności zobaczysz ten błąd, jeśli używasz domyślnej lokacji w *witrynach* (WebSite1), która zawiera odwołanie do pomocnika `Twitter`.
> 
> **Obejście**  
> Należy skomentować wywołania do dowolnych pomocników w lokacji, uruchomić stronę *administratora\_* i zainstalować pakiet lub pakiety zawierające pomocników, których chcesz użyć. Po zainstalowaniu pakietu można usunąć komentarz do wierszy, które odwołują się do pomocników.

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problem: wdrażanie zestawów ASP.NET w wersji beta 3 do folderu bin może nie zadziałać w witrynach hostingu

> W przypadku wdrażania witryny internetowej ASP.NET Web Pages w lokacji hostingu i wdrożenia zestawów ASP.NET Razor beta 3 do folderu *bin* lokacji mogą wystąpić błędy, w tym następujące:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Taka sytuacja może wystąpić, jeśli dostawca hostingu zainstalował ASP.NET strony sieci Web beta 1 zestawów w globalnej pamięci podręcznej aplikacji serwera. Zestawy w pamięci podręcznej GAC mają pierwszeństwo przed zestawami zainstalowanymi lokalnie w folderze *bin* .
> 
> **Obejście problemu** Skontaktuj się z dostawcą hostingu, aby upewnić się, że wykryte błędy są spowodowane konfliktem między wersjami zestawów a dostawcą. W takim przypadku należy zażądać, aby dostawca hostingu zaktualizował zestawy w pamięci podręcznej GAC serwera.

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: odczytywanie kanałów informacyjnych lub innych danych zewnętrznych za pośrednictwem serwera proxy

> Jeśli serwer z uruchomioną lokacją znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacji o serwerze proxy w pliku *Web. config* , aby można było odczytywać informacje pochodzące spoza witryny. Jeśli na przykład użyjesz pomocnika `ReCaptcha`, pomocnik komunikuje się z usługą reCAPTCHA, ale może być blokowany przez serwer proxy. Podobnie, kanały informacyjne, które są używane na stronach sieci Web ASP.NET, takie jak źródło danych używane przez Menedżera pakietów, mogą wymagać konfiguracji serwera proxy.
> 
> Jeśli wystąpią problemy podczas pracy z usługą zewnętrzną lub praca z kanałem informacyjnym pakietu, umieść następujące elementy w głównym pliku *Web. config* aplikacji:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Aby uzyskać więcej informacji o konfigurowaniu serwera proxy, zobacz [&lt;proxy&gt; element (Ustawienia sieci)](https://msdn.microsoft.com/library/sa91de1e.aspx) w witrynie MSDN w sieci Web.

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problem: nie można załadować pliku "Microsoft. Web. Infrastructure. dll"

> Jeśli wcześniej zainstalowano wersję beta 1 stron sieci Web ASP.NET z składnia Razor a następnie Zainstaluj wersję beta 3, wszystkie odpowiednie zestawy są instalowane w pamięci GAC z wyjątkiem *Microsoft. Web. Infrastructure. dll*. W związku z tym po uruchomieniu stron Razor ASP.NET zostanie wyświetlony komunikat o błędzie informujący o tym, że nie można załadować *pliku Microsoft. Web. Infrastructure. dll* .
> 
> Ten problem nie występuje, jeśli wersja beta 3 została załadowana na czystym komputerze.
> 
> **Obejście**  
> W panelu sterowania odinstaluj ASP.NET strony sieci Web. Następnie zainstaluj ponownie wersję beta 3.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Odinstalowywanie .NET Framework w wersji 4 wyłącza ASP.NET stron sieci Web ze składnią Razor

> Jeśli odinstalujesz .NET Framework w wersji 4, a następnie zainstalujesz ją ponownie, ASP.NET strony sieci Web z składnia Razor jest wyłączone. Strony z rozszerzeniem *. cshtml* nie działają poprawnie. Strony sieci Web ASP.NET rejestrują zestaw w głównym pliku *Web. config* maszyny, a usunięcie .NET Framework usuwa ten plik. Ponowne zainstalowanie .NET Framework instaluje nową wersję pliku konfiguracji, ale nie dodaje odwołania do zestawu stron sieci Web ASP.NET.
> 
> **Obejście problemu** Po ponownym zainstalowaniu .NET Framework ponownie zainstaluj strony sieci Web ASP.NET z składnia Razor. Spowoduje to dodanie następującego elementu do pliku *Web. config* w katalogu głównym maszyny, który zwykle znajduje się w następującej lokalizacji:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problem: aplikacje wdrożone wcześniej z zestawami ASP.NET w folderze bin zawierają błędy

> Podczas wdrażania kopie zestawów stron sieci Web ASP.NET (na przykład *Microsoft. Webpages. dll*) do folderu *bin* witryny sieci Web na serwerze. (Mogło to nastąpiło automatycznie podczas wdrażania lub, ponieważ deweloper jawnie skopiował zestawy). Jeśli jednak wersja beta 3 jest zainstalowana, występują błędy, takie jak błędy, które nie mogą zostać znalezione. Dzieje się tak, ponieważ wiele typów stron sieci Web ASP.NET zostało przeniesionych do różnych obszarów nazw dla wersji beta 3.
> 
> **Obejście**   
> Usuń zaznaczenie folderu *bin* wdrożonej aplikacji, skopiuj nowe zestawy do folderu (lub ponownie Wdróż aplikację), a następnie uruchom aplikację.

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
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problem: korzystanie z projektu aplikacji sieci Web lub ASP.NET MVC i ASP.NET stron sieci Web w tej samej aplikacji

> Jeśli używasz ASP.NET stron sieci Web w projekcie aplikacji sieci Web lub aplikacji ASP.NET MVC, może zostać wyświetlony błąd, którego nie można znaleźć *WebPageHttpApplication* .
> 
> **Obejście**  
> Jeśli wystąpi ten błąd, Zmień klasę bazową, z której pochodzi aplikacja. W pliku *Global. asax* Zmień następujący wiersz:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Do tego:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> W efekcie ta zmiana została wprowadzona w przypadku wersji beta 1 programu ASP.NET stron sieci Web z programem składnia Razor.

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: wdrażanie aplikacji na komputerze, na którym nie zainstalowano SQL Server Compact

> Aplikacje zawierające SQL Server Compact bazy danych można uruchamiać na komputerze, na którym SQL Server Compact nie jest zainstalowana. Program Microsoft WebMatrix beta 3 automatycznie kopiuje te pliki binarne i wykonuje transformacje pliku *Web. config* .
> 
> **Obejście problemu** Jeśli musisz skopiować te pliki i ręcznie wprowadzić zmiany w pliku *Web. config* , wykonaj następujące czynności:
> 
> 1. Skopiuj zestawy aparatów bazy danych do folderu *bin* (i podfolderów) aplikacji na komputerze docelowym: 
> 
>     - Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **do** *\Bin*
>     - Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* * **do** *\Bin\x86*
>     - Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **do** *\Bin\amd64*
> 2. W folderze głównym witryny sieci Web Utwórz lub Otwórz plik *Web. config* . (W programie WebMatrix beta 3 ten typ pliku jest dostępny po kliknięciu przycisku **wszystkie** w oknie dialogowym **Wybierz typ pliku** ).
> 3. Dodaj następujący element jako element podrzędny elementu **&lt;configuration&gt;** (nie wewnątrz elementu **&lt;system. Web&gt;** ):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: pomocniki bazy danych i WebGrid nie działają w średnim zaufaniu w Visual Basic

> Jeśli używasz Visual Basic (Tworzenie plików *. vbhtml* ), pomocniki `Database` i `WebGrid` nie będą działały, jeśli aplikacja jest ustawiona do używania średniego zaufania.
> 
> **Obejście**  
> Tymczasowo Ustaw aplikację tak, aby korzystała z pełnego zaufania.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problem: Właściwość "Szyfruj" nie została rozpoznana

> SQL Server Compact 4,0 nie rozpoznaje właściwości `Encrypt` klasy `SqlCeConnection`. Nie należy używać tej właściwości do szyfrowania plików bazy danych. Właściwość `Encrypt` była przestarzała w wersji SQL Server Compact 3,5 i została zachowana tylko w celu zapewnienia zgodności z poprzednimi wersjami. 
> 
> **Obejście**  
> Użyj właściwości `Encryption Mode` klasy `SqlCeConnection`, aby szyfrować pliki bazy danych SQL Server Compact 4,0. Poniższy przykład przedstawia sposób tworzenia zaszyfrowanej bazy danych SQL Server Compact 4,0 przy użyciu właściwości `Encryption Mode`:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Aby zmienić tryb szyfrowania istniejącej bazy danych SQL Server Compact 4,0, wykonaj następujące czynności:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Aby zaszyfrować niezaszyfrowaną bazę danych SQL Server Compact 4,0, wykonaj następujące czynności:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problem: biblioteki środowiska C++ uruchomieniowego programu Microsoft Visual 2008 są wymagane

> Natywne biblioteki DLL SQL Server Compact 4,0 potrzebują bibliotek środowiska uruchomieniowego Microsoft Visual C++ 2008 (x86, IA64 i x64) z dodatkiem Service Pack 1.
> 
> **Obejście**  
> Zainstaluj .NET Framework 3,5 z dodatkiem SP1. Spowoduje to również zainstalowanie bibliotek C++ środowiska uruchomieniowego Visual 2008 z dodatkiem SP1. Biblioteki można pobrać z następującej lokalizacji:   
>   
> [Aktualizacja zabezpieczeń C++ ATL pakietu redystrybucyjnego programu Microsoft Visual 2008 Service Pack 1](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Należy zauważyć, że zainstalowanie .NET Framework 2,0, 3,0 lub 4 nie *instaluje bibliotek* środowiska uruchomieniowego Visual C++ 2008 SP1.

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problem: Jeśli zainstalowano SQL Server Compact przed zainstalowaniem .NET Framework na komputerze, jego niezmienna nazwa dostawcy nie jest zarejestrowana w pliku Machine. config .NET Framework

> SQL Server Compact można zainstalować na komputerze, na którym nie zainstalowano .NET Framework, ponieważ SQL Server Compact wymaga programu .NET Framework. Jeśli przed zainstalowaniem programu SQL Server Compact nie zostanie zainstalowana żadna .NET Framework wersja 3,5 ani 4, Instalator SQL Server Compact nie zarejestruje nazwy niezmiennej dostawcy w pliku *Machine. config* . Wszystkie aplikacje, które opierają się na wpisie SQL Server Compact w pliku *Machine. config* , zakończą się niepowodzeniem. Wpis rejestracji nazwy niezmiennej w *pliku Machine. config* wygląda następująco:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Obejście**  
> Odinstaluj SQL Server Compact 4,0 CTP1. Pobierz i zainstaluj pełne wersje .NET Framework z następującej lokalizacji:
> 
> [Microsoft .NET Framework 3,5 z dodatkiem Service Pack 1 (pełny pakiet)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Wydanie w Microsoft .NET Framework 4,0 (pełen pakiet)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Następnie zainstaluj ponownie [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalowanie aplikacji

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: instalacja aplikacji może zająć dużo czasu, jeśli folder Moje dokumenty użytkownika zostanie przekierowany do udziału sieciowego

> **Obejście**  
> Brak. Instalacja aplikacji może potrwać trochę czasu, ale instalacja zostanie poprawna.

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publikowanie aplikacji

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

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Inne problemy

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problem: wyszukiwanie/filtrowanie nie działa w raportach dla elementu Grupuj według: typ problemu

> Po uruchomieniu raportu dla witryny, jeśli wprowadzisz tekst w polu *Filtruj według adresu URL* , a następnie klikniesz przycisk *Wyszukaj*, nic się nie dzieje. Jest to spowodowane tym, że ta kontrolka nie działa, podczas gdy *Grupa według* stanu raportu jest ustawiona na wartość *typ problemu*, co jest ustawieniem domyślnym.
> 
> **Obejście problemu** Na karcie *Grupuj według* na wstążce kliknij pozycję *adres URL* , aby zgrupować wpisy według ich źródłowego adresu URL. Pole tekstowe i przycisk służący do filtrowania wpisów są funkcjonalne w tym stanie.

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problem: uruchamianie aplikacji WCF nie powiodło się z IIS Express

> Przechodzenie do aplikacji WCF spowoduje wystąpienie błędu podobne do następującego:
> 
> *Nie można załadować pliku lub zestawu "Microsoft. Web. Administration, version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" lub jednej z jego zależności. System nie może znaleźć określonego pliku.*
> 
> Dzieje się tak, ponieważ IIS Express wersja beta nie obsługuje domyślnie programu WCF.
> 
> **Obejście problemu** Użyj jednego z następujących obejść (obejście #2 wymaga systemu Microsoft Windows Vista lub nowszego):
> 
> 
> 1. Skopiuj zestawy *Microsoft. Web. dll* i *Microsoft. Web. Administration. dll* z lokalizacji instalacji programu WebMatrix do katalogu *bin* aplikacji WCF. Domyślnie program WebMatrix jest instalowany w podfolderze *Microsoft WebMatrix* w folderze *Program Files* .
> 2. W systemie Microsoft Windows Vista lub nowszym Utwórz link symboliczny do zestawów w katalogu *bin* przy użyciu następujących poleceń. (Takie podejście ma zaletę, że nie tworzy kopii zestawów).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Zainstaluj dwa zestawy w pamięci podręcznej GAC. W wierszu polecenia z podwyższonym poziomem uprawnień uruchom następujące polecenia:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: w programie WebMatrix beta 3 nie można wykonać określonych zadań wymagających podniesienia uprawnień

> Program WebMatrix beta 3 nie może wykonać niektórych zadań wymagających podniesienia uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:
> 
> - W systemie Windows Vista lub Windows 7 użytkownik zalogował się przy użyciu konta, które nie ma uprawnień administracyjnych i kontrola konta użytkownika (UAC) jest wyłączona.
> - Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.
> 
> **Obejście**  
> Większość zadań w programie WebMatrix beta 3 nie wymaga uprawnień administracyjnych. Dla tych, które wykonują operację jako administrator, lub wykonaj następujące czynności:
> 
> - W systemie Windows Vista lub Windows 7 Włącz kontrolę konta użytkownika.
> - W systemie Windows XP Dodaj użytkownika do grupy zabezpieczeń Administratorzy.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Witryna z galerii sieci Web" jest wyłączona

> Opcja **Witryna z galerii sieci Web** jest wyłączona, jeśli nie zainstalowano Instalatora platformy sieci Web 3,0.
> 
> **Obejście**  
> Zainstaluj [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problem: w systemie Windows Server 2003, IIS Express nie zostanie uruchomiony dla użytkownika niebędącego administratorami

> W systemie Windows Server 2003 podczas uruchamiania strony lub uruchamiania IIS Express, IIS Express nie zostanie uruchomiona. W przypadku stron sieci Web zostanie wyświetlony komunikat o błędzie informujący o tym, że aplikacja została uruchomiona przez użytkownika niebędącego administratorami.
> 
> **Obejście**  
> Uruchom aplikację WebMatrix beta 3 jako użytkownik administracyjny. Aby uzyskać więcej informacji, zobacz następujący artykuł bazy wiedzy:  
>   
> [Aplikacja uruchamiana przez nieadministracyjnego użytkownika nie może nasłuchiwać ruchu HTTP komputera, na którym aplikacja jest uruchomiona w systemie Windows Vista, Windows Server 2003 lub Windows XP.](https://support.microsoft.com/kb/939786)

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

#### <a name="issue-the-relationships-button-is-disabled"></a>Problem: przycisk "relacje" jest wyłączony

> Przycisk **relacje** na karcie **tabela** w obszarze roboczym **bazy danych** jest wyłączony dla SQL Server Compact baz danych.
> 
> **Obejście**  
> Brak. SQL Server Compact nie obsługuje relacji między tabelami.

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problem: sparametryzowane zapytania SQL zgłaszają wyjątki

> W SQL Server Compact 4,0, jeśli nie określisz typu danych, takiego jak `SqlDbType` lub `DbType` dla parametrów w zapytaniach parametrycznych, zostanie zgłoszony wyjątek, gdy zostanie uruchomione zapytanie.
> 
> **Obejście**  
> Jawnie ustaw typ danych dla parametrów, takich jak `SqlDbType` lub `DbType`. Jest to krytyczne w przypadku typów danych obiektów BLOB (`image` i `ntext`). Użyj kodu w następujący sposób:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a>Aby uzyskać więcej informacji

Aby uzyskać więcej informacji o programie WebMatrix beta 3, zobacz następujące witryny sieci Web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

---

© 2010 Microsoft Corporation. Wszelkie prawa zastrzeżone. [Warunki użytkowania](https://msdn.microsoft.cos/cc300389.aspx).
