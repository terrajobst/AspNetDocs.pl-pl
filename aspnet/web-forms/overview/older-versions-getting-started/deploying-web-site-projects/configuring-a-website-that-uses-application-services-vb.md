---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Konfigurowanie witryny sieci Web korzystającej z Usługi aplikacji (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET w wersji 2,0 wprowadzono serię usług aplikacji, które są częścią .NET Framework i służą jako pakiet usług blokowych yo...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 19e7258b558372259c7554a36c6ad73ce572dfa8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588633"
---
# <a name="configuring-a-website-that-uses-application-services-vb"></a>Konfigurowanie witryny internetowej, która korzysta z usług aplikacji (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> W ASP.NET w wersji 2,0 wprowadzono szereg usług aplikacji, które są częścią .NET Framework i służą jako pakiet usług blokowych, których można użyć do dodawania zaawansowanych funkcji aplikacji sieci Web. W tym samouczku przedstawiono sposób konfigurowania witryny sieci Web w środowisku produkcyjnym w celu korzystania z usług aplikacji i rozwiązywania typowych problemów związanych z zarządzaniem kontami i rolami użytkowników w środowisku produkcyjnym.

## <a name="introduction"></a>Wprowadzenie

W ASP.NET w wersji 2,0 wprowadzono szereg *usług aplikacji*, które są częścią .NET Framework i służą jako pakiet usług blokowych, których można użyć do dodawania zaawansowanych funkcji aplikacji sieci Web. Usługi aplikacji obejmują:

- **Członkostwo** — interfejs API służący do tworzenia kont użytkowników i zarządzania nimi.
- **Role** — interfejs API służący do kategoryzowania użytkowników w grupach.
- **Profil** — interfejs API służący do przechowywania niestandardowej zawartości specyficznej dla użytkownika.
- **Mapa witryny** — interfejs API służący do definiowania struktury logicznej lokacji w formie hierarchii, która może być następnie wyświetlana za pośrednictwem kontrolek nawigacji, takich jak menu i struktury nawigacyjne.
- **Personalizacja** — interfejs API do obsługi preferencji dostosowywania najczęściej używany z [*składnikami Web Part*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitorowanie kondycji** — interfejs API służący do monitorowania wydajności, zabezpieczeń, błędów i innych metryk kondycji systemu dla działającej aplikacji sieci Web.

Interfejsy API usług aplikacji nie są powiązane z określoną implementacją. Zamiast tego należy nakazać usługom aplikacji użycie określonego *dostawcy*, a ten dostawca implementuje usługę przy użyciu określonej technologii. Najczęściej używani dostawcy dla internetowych aplikacji sieci Web hostowanych w firmie hostingu w sieci Web to dostawcy używający implementacji bazy danych SQL Server. Na przykład `SqlMembershipProvider` jest dostawcą interfejsu API członkostwa, który przechowuje informacje o koncie użytkownika w Microsoft SQL Server bazie danych.

Korzystanie z usług aplikacji i dostawców SQL Server dodaje pewne wyzwania podczas wdrażania aplikacji. W przypadku programów do uruchamiania obiekty bazy danych usług aplikacji muszą być poprawnie utworzone zarówno na potrzeby tworzenia, jak i produkcyjnych baz danych. Istnieją także ważne ustawienia konfiguracji, które należy wykonać.

> [!NOTE]
> Interfejsy API usług aplikacji zostały zaprojektowane przy użyciu [*modelu dostawcy*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), wzorca projektowego, który umożliwia szczegółowe informacje implementacji interfejsu API s w czasie wykonywania. .NET Framework dostarcza wielu dostawców usług aplikacji, których można użyć, takich jak `SqlMembershipProvider` i `SqlRoleProvider`, które są dostawcami dla interfejsów API członkostwa i ról, które używają implementacji bazy danych SQL Server. Możesz również utworzyć dostawcę niestandardowego i podłączyć go. W rzeczywistości aplikacja sieci Web przeglądający książkę zawiera już niestandardowego dostawcę dla interfejsu API mapy witryny (`ReviewSiteMapProvider`), który konstruuje mapę witryny na podstawie danych w tabelach `Genres` i `Books` w bazie danych.

Ten samouczek rozpoczyna się od rozbudowanej aplikacji sieci Web przeglądający książkę w celu korzystania z interfejsów API członkostwa i ról. Następnie przeprowadza proces wdrażania aplikacji sieci Web, która korzysta z usług aplikacji z implementacją bazy danych SQL Server i ma na celu rozwiązywanie typowych problemów związanych z zarządzaniem kontami i rolami użytkowników w środowisku produkcyjnym.

## <a name="updates-to-the-book-reviews-application"></a>Aktualizacje aplikacji do przeglądu książki

W porównaniu do poprzednich samouczków książka przegląda aplikację sieci Web została zaktualizowana z statycznej witryny internetowej na dynamiczną, opartą na danych aplikację sieci Web z zestawem stron administracyjnych służących do zarządzania gatunkami i przeglądami. Jednak ta sekcja administracyjna nie jest obecnie chroniona — każdy użytkownik, który zna (lub odgadnięcie) adres URL strony administracyjnej, może Waltz i tworzyć, edytować lub usuwać przeglądy w naszej witrynie. Typowym sposobem ochrony niektórych części witryny jest zaimplementowanie kont użytkowników, a następnie użycie reguł autoryzacji adresów URL w celu ograniczenia dostępu do określonych użytkowników lub ról. Książka przegląda aplikację sieci Web, która jest dostępna do pobrania w ramach tego samouczka, obsługuje konta i role użytkowników. Ma jedną rolę zdefiniowaną jako administrator i tylko użytkownicy w tej roli mogą uzyskiwać dostęp do stron administracyjnych.

> [!NOTE]
> Po utworzeniu trzech kont użytkowników w książce: Scott, Jisun i Alicja. Wszyscy trzy użytkownicy mają to samo hasło: **hasło!** Scott i Jisun znajdują się w roli administratora, Alicja nie jest. Strony nieadministracyjne lokacji nie są nadal dostępne dla użytkowników anonimowych. Oznacza to, że nie musisz logować się, aby odwiedzić witrynę, chyba że chcesz ją administrować. w takim przypadku musisz zalogować się jako użytkownik w roli administratora.

Strona wzorcowa przeglądy książki aplikacji została zaktualizowana w taki sposób, aby zawierała inny interfejs użytkownika dla uwierzytelnionych i anonimowych użytkowników. Jeśli użytkownik anonimowy odwiedzi witrynę, zobaczy link logowania w prawym górnym rogu. Uwierzytelniony użytkownik widzi komunikat "Witaj wstecz, *username*!" i link do wylogowania. Istnieje również strona logowania (`~/Login.aspx`), która zawiera kontrolkę sieci Web logowania, która zapewnia interfejs użytkownika i logikę do uwierzytelniania gościa. Tylko Administratorzy mogą tworzyć nowe konta. (Istnieją strony do tworzenia kont użytkowników i zarządzania nimi w folderze `~/Admin`).

### <a name="configuring-the-membership-and-roles-apis"></a>Konfigurowanie członkostwa i interfejsów API ról

Aplikacja sieci Web przegląda książkę korzysta z interfejsów API członkostwa i ról do obsługi kont użytkowników oraz grupowania tych użytkowników w role (tj. rola administratora). Klasy dostawcy `SqlMembershipProvider` i `SqlRoleProvider` są używane, ponieważ chcą przechowywać informacje o kontach i rolach w SQL Server bazie danych.

> [!NOTE]
> Ten samouczek nie jest przeznaczony do szczegółowej analizy podczas konfigurowania aplikacji sieci Web do obsługi interfejsów API członkostwa i ról. Aby uzyskać szczegółowe informacje na temat tych interfejsów API i czynności, które należy wykonać w celu skonfigurowania witryny sieci Web do korzystania z nich, Przeczytaj [*samouczki zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Aby użyć usług aplikacji z SQL Server bazą danych, należy najpierw dodać obiekty bazy danych używane przez tych dostawców do bazy danych, w której mają być przechowywane informacje o koncie i roli użytkownika. Te wymagane obiekty bazy danych obejmują różne tabele, widoki i procedury składowane. O ile nie określono inaczej, klasy dostawcy `SqlMembershipProvider` i `SqlRoleProvider` używają bazy danych SQL Server Express Edition o nazwie `ASPNETDB` znajdującej się w folderze Application-`App_Data`. Jeśli taka baza danych nie istnieje, jest automatycznie tworzona z niezbędnymi obiektami bazy danych przez tych dostawców w czasie wykonywania.

Jest to możliwe, a zazwyczaj idealne, aby utworzyć obiekty bazy danych usług aplikacji w tej samej bazie danych, w której przechowywane są dane specyficzne dla aplikacji witryny sieci Web. .NET Framework jest dostarczany przy użyciu narzędzia o nazwie `aspnet_regsql.exe`, które instaluje obiekty bazy danych w określonej bazie danych. To narzędzie zostało usunięte i użyte do dodania tych obiektów do bazy danych `Reviews.mdf` w folderze `App_Data` (baza danych programistycznych). Zobaczmy, jak używać tego narzędzia w dalszej części tego samouczka, gdy dodamy te obiekty do produkcyjnej bazy danych.

W przypadku dodawania obiektów bazy danych usług aplikacji do bazy danych innej niż `ASPNETDB` należy dostosować konfiguracje klas dostawcy `SqlMembershipProvider` i `SqlRoleProvider`, tak aby korzystały z odpowiedniej bazy danych. Aby dostosować dostawcę członkostwa, Dodaj [ *&lt;członkostwo&gt; elementu*](https://msdn.microsoft.com/library/1b9hw62f.aspx) w sekcji `<system.web>` w `Web.config`; Użyj [*elementu&lt;rolemanager&gt;* ](https://msdn.microsoft.com/library/ms164660.aspx) , aby skonfigurować dostawcę ról. Poniższy fragment kodu jest pobierany z książki przeglądowej aplikacji `Web.config` i zawiera konfigurację ustawień interfejsów API członkostwa i ról. Należy zauważyć, że zarówno Zarejestruj nowego dostawcę — `ReviewMembership`, jak i `ReviewRole`, które używają odpowiednio dostawców `SqlMembershipProvider` i `SqlRoleProvider`.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

Element `<authentication>` `Web.config` pliku został również skonfigurowany do obsługi uwierzytelniania opartego na formularzach.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Ograniczanie dostępu do stron administracyjnych

ASP.NET ułatwia udzielanie lub odmawianie dostępu do określonego pliku lub folderu przez użytkownika lub przez rolę za pomocą funkcji *autoryzacji adresów URL* . (Zawarto krótko omówić autoryzację adresów URL w *podstawowych różnicach między usługami IIS a samouczkiem serwera ASP.NET Development* i pokazano, jak usługi iis i ASP.NET Development Server stosują reguły autoryzacji adresów URL inaczej w przypadku zawartości statycznej i dynamicznej.) Ponieważ chcemy zabronić dostępu do folderu `~/Admin` z wyjątkiem tych użytkowników w roli administratora, musimy dodać do tego folderu reguły autoryzacji adresów URL. W szczególnych przypadkach reguły autoryzacji adresów URL muszą zezwalać użytkownikom w roli administratora i odmawiać wszystkim innym użytkownikom. W tym celu należy dodać plik `Web.config` do folderu `~/Admin` z następującą zawartością:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Aby uzyskać więcej informacji na temat funkcji autoryzacji adresów URL ASP.NET s i sposobu jej używania do sprawdzania reguł autoryzacji dla użytkowników i ról, pamiętaj o zapoznaniu się z informacjami o autoryzacjach i [*rolach autoryzacji*](../../older-versions-security/roles/role-based-authorization-vb.md) opartych na [*użytkownikach*](../../older-versions-security/membership/user-based-authorization-vb.md) ze wszystkich [*samouczków zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Wdrażanie aplikacji sieci Web korzystającej z Usługi aplikacji

W przypadku wdrażania witryny sieci Web, która korzysta z usług aplikacji i dostawcy przechowującego informacje o usługach aplikacji w bazie danych, należy koniecznie utworzyć obiekty bazy danych niezbędne przez usługi aplikacji w produkcyjnej bazie danych. Początkowo baza danych produkcyjna nie zawiera tych obiektów, więc podczas pierwszego wdrożenia aplikacji (lub po jej wdrożeniu po raz pierwszy po dodaniu usług aplikacji) należy wykonać dodatkowe czynności, aby uzyskać te wymagane obiekty bazy danych na produkcyjna baza danych.

Podczas wdrażania witryny sieci Web, która korzysta z usług aplikacji, może wystąpić inne wyzwanie, jeśli zamierzasz przeprowadzić replikację kont użytkowników utworzonych w środowisku programistycznym w środowisku produkcyjnym. W zależności od konfiguracji członkostwa i ról istnieje możliwość, że nawet w przypadku pomyślnego skopiowania kont użytkowników, które zostały utworzone w środowisku programistycznym w produkcyjnej bazie danych, użytkownicy nie mogą zalogować się do aplikacji sieci Web w produkcji. Zaobserwujemy przyczynę tego problemu i omawiamy, jak zapobiegać jego wystąpieniu.

ASP.NET jest dostarczany z świetnym [*narzędziem do administrowania witryną sieci Web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) , które można uruchomić z programu Visual Studio i umożliwia zarządzanie kontami użytkowników, rolami i regułami autoryzacji za pośrednictwem interfejsu opartego na sieci Web. Niestety, WSAT działa tylko w przypadku lokalnych witryn sieci Web, co oznacza, że nie można jej użyć do zdalnego zarządzania kontami użytkowników, rolami i regułami autoryzacji dla aplikacji sieci Web w środowisku produkcyjnym. Zobaczymy różne sposoby implementacji zachowania przypominającego WSAT z produkcyjnej witryny sieci Web.

### <a name="adding-the-database-objects-using-aspnet_regsqlexe"></a>Dodawanie obiektów bazy danych przy użyciu ASPNET\_regsql. exe

Samouczek *wdrażania bazy danych* przedstawia sposób kopiowania tabel i danych z bazy danych programistycznych do produkcyjnej bazy danych. te techniki mogą być używane do kopiowania obiektów bazy danych usług aplikacji do produkcyjnej bazy danych. Kolejną opcją jest narzędzie `aspnet_regsql.exe`, które dodaje lub usuwa obiekty bazy danych usług aplikacji z bazy danych.

> [!NOTE]
> Narzędzie `aspnet_regsql.exe` tworzy obiekty bazy danych w określonej bazie danych. Nie migruje danych w tych obiektach bazy danych z bazy danych programistycznych do produkcyjnej bazy danych programu. Jeśli chcesz skopiować informacje o koncie użytkownika i roli w bazie danych programistycznych do produkcyjnej bazy danych, Użyj technik opisanych w samouczku *wdrażanie bazy danych* .

Pozwól, jak dodać obiekty bazy danych do produkcyjnej bazy danych przy użyciu narzędzia `aspnet_regsql.exe`. Zacznij od otwarcia Eksploratora Windows i przejściu do katalogu .NET Framework w wersji 2,0 na komputerze,% WINDIR% \ Microsoft. NET\Framework\v2.0.50727. Należy znaleźć narzędzie `aspnet_regsql.exe`. Tego narzędzia można użyć w wierszu polecenia, ale zawiera on również graficzny interfejs użytkownika. Kliknij dwukrotnie plik `aspnet_regsql.exe`, aby uruchomić jego składnik graficzny.

Narzędzie uruchamia się, wyświetlając ekran powitalny wyjaśniający jego przeznaczenie. Kliknij przycisk Dalej, aby przejść do ekranu "Wybierz opcję instalacji", która jest pokazana na rysunku 1. W tym miejscu możesz wybrać opcję dodania obiektów bazy danych usług aplikacji lub usunięcia ich z bazy danych. Ponieważ chcemy dodać te obiekty do produkcyjnej bazy danych, wybierz opcję "Konfiguruj SQL Server dla usług aplikacji", a następnie kliknij przycisk Dalej.

[![wybrać, aby skonfigurować SQL Server Usługi aplikacji](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Rysunek 1**. Wybierz, aby skonfigurować SQL Server usługi aplikacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))

Na ekranie "Wybierz serwer i bazę danych" pojawia się prośba o informacje umożliwiające nawiązanie połączenia z bazą danych. Wprowadź serwer bazy danych, poświadczenia zabezpieczeń i nazwę bazy danych dostarczonej przez firmę hostingową sieci Web, a następnie kliknij przycisk Dalej.

> [!NOTE]
> Po wprowadzeniu serwera bazy danych i poświadczeń może wystąpić błąd podczas rozszerzania listy rozwijanej baza danych. Narzędzie `aspnet_regsql.exe` wysyła zapytanie do tabeli systemu `sysdatabases` w celu pobrania listy baz danych na serwerze, ale niektóre firmy obsługujące hosting w sieci Web zablokują serwery baz danych, dzięki czemu te informacje nie będą dostępne publicznie. Jeśli zostanie wyświetlony ten błąd, można wpisać nazwę bazy danych bezpośrednio do listy rozwijanej.

[![podawanie narzędzia informacjami o połączeniu z bazą danych](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Rysunek 2**. podawanie narzędzia informacjami o połączeniu z bazą danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))

Kolejny ekran zawiera podsumowanie działań, które mają zostać wykonane, w tym informacje o tym, że obiekty bazy danych usług aplikacji zostaną dodane do określonej bazy danych. Kliknij przycisk Dalej, aby zakończyć tę akcję. Po kilku chwilach zostanie wyświetlony ekran ostatni z zanotowaniem, że obiekty bazy danych zostały dodane (patrz rysunek 3).

[![powodzenie! Do produkcyjnej bazy danych zostały dodane obiekty Usługi aplikacji Database](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Rysunek 3**. sukces! Obiekty Usługi aplikacji Database zostały dodane do produkcyjnej bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))

Aby sprawdzić, czy obiekty bazy danych usług aplikacji zostały pomyślnie dodane do produkcyjnej bazy danych, Otwórz SQL Server Management Studio i nawiąż połączenie z produkcyjną bazą danych. Jak pokazano na rysunku 4, w bazie danych programu znajdują się tabele bazy danych usług aplikacji, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`i tak dalej.

[![upewnić się, że obiekty bazy danych zostały dodane do produkcyjnej bazy danych](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Rysunek 4**. potwierdzenie, że obiekty bazy danych zostały dodane do produkcyjnej bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))

Wystarczy użyć narzędzia `aspnet_regsql.exe` podczas wdrażania aplikacji sieci Web po raz pierwszy lub po raz pierwszy po rozpoczęciu korzystania z usług aplikacji. Gdy te obiekty bazy danych znajdują się w produkcyjnej bazie danych, nie trzeba ich ponownie dodawać ani modyfikować.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopiowanie kont użytkowników z programowania do produkcji

W przypadku używania klas dostawcy `SqlMembershipProvider` i `SqlRoleProvider` do przechowywania informacji o usługach aplikacji w bazie danych SQL Server informacje o kontach i rolach użytkownika są przechowywane w różnych tabelach bazy danych, w tym `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`i `aspnet_UsersInRoles`, między innymi. Jeśli podczas tworzenia utworzysz konta użytkowników w środowisku deweloperskim, można replikować te konta użytkowników w produkcji, kopiując odpowiednie rekordy z odpowiednich tabel bazy danych. Jeśli do wdrożenia obiektów bazy danych usług aplikacji użyto Kreatora publikacji bazy danych, być może wybrano również opcję skopiowania rekordów, co spowoduje, że konta użytkowników utworzone w ramach programowania również będą dostępne w środowisku produkcyjnym. Jednak w zależności od ustawień konfiguracji może się okazać, że Ci użytkownicy, których konta zostały utworzone podczas opracowywania i skopiowane do środowiska produkcyjnego, nie mogą zalogować się z produkcyjnej witryny sieci Web. Co daje?

Klasy dostawcy `SqlMembershipProvider` i `SqlRoleProvider` zostały zaprojektowane w taki sposób, że pojedyncza baza danych może służyć jako magazyn użytkowników dla wielu aplikacji, w których każda aplikacja może korzystać z nakładających się nazw użytkowników i ról o tej samej nazwie. Aby zapewnić taką elastyczność, baza danych przechowuje listę aplikacji w tabeli `aspnet_Applications`, a każdy użytkownik jest skojarzony z jedną z tych aplikacji. W tabeli `aspnet_Users` tabela zawiera kolumnę `ApplicationId`, która łączy każdego użytkownika do rekordu w tabeli `aspnet_Applications`.

Oprócz kolumny `ApplicationId` tabela `aspnet_Applications` zawiera również kolumnę `ApplicationName`, która zapewnia bardziej przyjazną dla człowieka nazwę aplikacji. Gdy witryna sieci Web próbuje współpracować z kontem użytkownika, na przykład sprawdzając poprawność poświadczeń użytkownika na stronie logowania, musi poinformować o klasie `SqlMembershipProvider`, z którą aplikacja ma współpracować. Zwykle jest to realizowane przez podanie nazwy aplikacji, a ta wartość pochodzi z konfiguracji dostawcy s w `Web.config` — w odróżnieniu od atrybutu `applicationName`.

Ale co się stanie, jeśli atrybut `applicationName` nie zostanie określony w `Web.config`? W takim przypadku system członkostwa używa ścieżki katalogu głównego aplikacji jako wartości `applicationName`. Jeśli atrybut `applicationName` nie jest jawnie ustawiony w `Web.config`, istnieje możliwość, że środowisko deweloperskie i środowisko produkcyjne używają innego katalogu głównego aplikacji i dlatego zostaną skojarzone z różnymi nazwami aplikacji w usługach aplikacji. Jeśli taka niezgodność występuje, użytkownicy utworzeni w środowisku deweloperskim będą mieć `ApplicationId` wartość, która nie jest zgodna z wartością `ApplicationId` dla środowiska produkcyjnego. Wynika to z faktu, że użytkownicy nie będą mogli się zalogować.

> [!NOTE]
> Jeśli znajdziesz się w tej sytuacji — z kontami użytkowników skopiowanymi do produkcji z niezgodną `ApplicationId` wartością — można napisać zapytanie, aby zaktualizować te nieprawidłowe wartości `ApplicationId` do `ApplicationId` używanych w środowisku produkcyjnym. Po zaktualizowaniu użytkownicy, których konta zostały utworzone w środowisku deweloperskim, będą mogli zalogować się do aplikacji sieci Web na etapie produkcji.

Dobra wiadomość polega na tym, że istnieje prosty krok, aby upewnić się, że dwa środowiska używają tego samego `ApplicationId` — jawnie ustaw atrybut `applicationName` w `Web.config` dla wszystkich dostawców usług aplikacji. Jawnie ustawimy atrybut `applicationName` na "BookReviews" w `<membership>` i `<roleManager>` elementy, jak ten fragment kodu z `Web.config`.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Aby uzyskać więcej informacji na temat ustawiania atrybutu `applicationName` i ich racjonalnego uzasadnienia, zapoznaj się z wpisem na blogu [*Scott Guthrie*](https://weblogs.asp.net/scottgu/) s, a następnie [*zawsze ustaw właściwość ApplicationName podczas konfigurowania członkostwa ASP.NET i innych dostawców*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Zarządzanie kontami użytkowników w środowisku produkcyjnym

Narzędzie do administrowania witryną sieci Web ASP.NET (WSAT) ułatwia tworzenie kont użytkowników i zarządzanie nimi, Definiowanie i stosowanie ról oraz sprawdzanie reguł autoryzacji opartych na użytkownikach i rolach. Możesz uruchomić WSAT z programu Visual Studio, przechodząc do Eksplorator rozwiązań i klikając ikonę konfiguracji ASP.NET lub wybierając pozycję witryna sieci Web lub projektu i wybierając element menu Konfiguracja ASP.NET. Niestety, WSAT może współpracował tylko z lokalnymi witrynami sieci Web. W związku z tym nie można używać WSAT z poziomu stacji roboczej do zarządzania witryną sieci Web w środowisku produkcyjnym.

Dobra wiadomość polega na tym, że wszystkie funkcje udostępniane przez WSAT są dostępne programowo za pomocą interfejsów API członkostwa i ról. Ponadto wiele ekranów WSAT korzysta ze standardowych kontrolek związanych z logowaniem ASP.NET. Krótko mówiąc, możesz dodać strony ASP.NET do witryny sieci Web, które oferują niezbędne funkcje zarządzania.

Odwołuje się do wcześniejszego samouczka, który zaktualizował aplikację sieci Web, aby zawierała folder `~/Admin`, a ten folder został skonfigurowany tak, aby zezwalał tylko na użytkowników w roli administratora. Po dodaniu strony do tego folderu o nazwie `CreateAccount.aspx`, z której administrator może utworzyć nowe konto użytkownika. Ta strona używa formantu formancie CreateUserWizard, aby wyświetlić interfejs użytkownika i logikę zaplecza w celu utworzenia nowego konta użytkownika. Co więcej, I dostosowałem formant, aby zawierał pole wyboru, które monituje o to, czy nowy użytkownik powinien również zostać dodany do roli administratora (patrz rysunek 5). Za pomocą małej ilości pracy można utworzyć niestandardowy zestaw stron, który implementuje zadania związane z zarządzaniem użytkownikami i rolami, które w przeciwnym razie byłyby udostępniane przez WSAT.

> [!NOTE]
> Aby uzyskać więcej informacji o korzystaniu z interfejsów API członkostwa i ról wraz z kontrolkami sieci Web ASP.NET związanymi z logowaniem, pamiętaj o odczytaniu [*samouczków zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Aby uzyskać więcej informacji na temat dostosowywania formantu formancie CreateUserWizard, zapoznaj się z tematem [*Tworzenie kont użytkowników*](../../older-versions-security/membership/creating-user-accounts-vb.md) i [*przechowywanie dodatkowych samouczków dotyczących informacji o użytkownikach*](../../older-versions-security/membership/storing-additional-user-information-vb.md) lub zapoznaj się z artykułem [*Erich Peterson*](http://www.erichpeterson.com/) s, [*dostosowując kontrolkę formancie CreateUserWizard*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).

[Administratorzy ![mogą tworzyć nowe konta użytkowników](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Rysunek 5**: Administratorzy mogą tworzyć nowe konta użytkowników ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))

Jeśli potrzebujesz pełnej funkcjonalności WSAT, Wyewidencjonuj [*własne narzędzie do administrowania witryną sieci Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), w którym autor Dan Clem przeprowadzi proces tworzenia niestandardowego narzędzia przypominającego WSAT. Dan udostępnia swój kod źródłowy aplikacji (w programie C#) i zawiera instrukcje krok po kroku dotyczące dodawania go do hostowanej witryny sieci Web.

## <a name="summary"></a>Podsumowanie

Podczas wdrażania aplikacji sieci Web, która korzysta z implementacji bazy danych usług aplikacji, należy najpierw upewnić się, że produkcyjna baza danych ma wymagane obiekty bazy danych. Te obiekty można dodawać przy użyciu technik omówionych w samouczku *wdrażanie bazy danych* . Alternatywnie możesz użyć narzędzia `aspnet_regsql.exe`, jak zostało to opisane w tym samouczku. Inne wyzwania zachęcamy do wyśrodkowania synchronizacji nazwy aplikacji używanej w środowiskach deweloperskich i produkcyjnych (co jest ważne, jeśli chcesz, aby użytkownicy i role utworzone w środowisku programistycznym były prawidłowe dla produkcji) i techniki dla Zarządzanie użytkownikami i rolami w środowisku produkcyjnym.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [*Narzędzie rejestracji SQL Server ASP.NET (aspnet_regsql. exe)* ](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Tworzenie bazy danych Usługi aplikacji dla SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Tworzenie schematu członkostwa w SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Badanie członkostwa, ról i profilu ASP.NET s*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Stopniowane narzędzie do administrowania witryną sieci Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Omówienie narzędzia do administrowania witryną sieci Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Poprzednie](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [dalej](strategies-for-database-development-and-deployment-vb.md)
