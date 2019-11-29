---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Użytkownicy i role w produkcyjnej witrynie sieciC#Web () | Microsoft Docs
author: rick-anderson
description: Narzędzie do administrowania witryną sieci Web ASP.NET (WSAT) udostępnia internetowy interfejs użytkownika służący do konfigurowania ustawień członkostwa i ról oraz do tworzenia, edytowania, a...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: c47bd2c1661f129dd8856916de04b8ba459fbfec
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74616556"
---
# <a name="users-and-roles-on-the-production-website-c"></a>Użytkownicy i role w produkcyjnej witrynie sieciC#Web ()

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> Narzędzie do administrowania witryną sieci Web ASP.NET (WSAT) udostępnia internetowy interfejs użytkownika służący do konfigurowania ustawień członkostwa i ról oraz do tworzenia, edytowania i usuwania użytkowników i ról. Niestety, WSAT działa tylko po odwiedzeniu z hosta lokalnego, co oznacza, że nie można skontaktować się z narzędziem administracyjnym produkcyjnej witryny sieci Web za pośrednictwem przeglądarki. Dobrym sposobem jest obejście tego problemu, które umożliwiają zarządzanie użytkownikami i rolami w środowisku produkcyjnym. Ten samouczek analizuje te obejścia i inne.

## <a name="introduction"></a>Wprowadzenie

W ASP.NET 2,0 wprowadzono wiele *usług aplikacji*, które stanowią pakiet usług blokowych, które można dodać do aplikacji sieci Web. Dodaliśmy usługi członkostwa i ról do witryny internetowej przeglądów książki w obszarze [ *Konfigurowanie witryny sieci Web korzystającej z usługi aplikacji* samouczka](configuring-a-website-that-uses-application-services-cs.md). Usługa członkostwa ułatwia tworzenie kont użytkowników i zarządzanie nimi; Usługa role oferuje interfejs API do kategoryzowania użytkowników w grupach. Witryna przeglądy książek ma trzy konta użytkowników — Scott, Jisun i Alicja — oraz jedną rolę, administratora, z Scott i Jisun w roli administratora.

ASP.NET. Usługi aplikacji sieci nie są powiązane z konkretną implementacją. Zamiast tego należy nakazać usługom aplikacji użycie określonego *dostawcy*, a ten dostawca implementuje usługę przy użyciu określonej technologii. Skonfigurowano aplikację sieci Web przeglądający książki pod kątem używania dostawców `SqlMembershipProvider` i `SqlRoleProvider` dla usług członkostwa i ról. Ci dwaj dostawcy przechowują informacje o koncie użytkownika i roli w bazie danych SQL Server i są najczęściej używanymi dostawcami dla internetowych aplikacji sieci Web hostowanych w firmie hostingu w sieci Web.

Typowym wyzwaniem dla deweloperów korzystającym z usług członkostwa i ról jest zarządzanie użytkownikami i rolami w środowisku produkcyjnym. Jak usunąć konto użytkownika z produkcyjnej witryny sieci Web, dodać nową rolę lub dodać istniejącego użytkownika do istniejącej roli? W tym samouczku przedstawiono różne techniki zarządzania użytkownikami i rolami w produkcyjnej witrynie sieci Web.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Korzystanie z narzędzia do administrowania witryną sieci Web ASP.NET

ASP.NET obejmuje [Narzędzie do administrowania witryną sieci Web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT), które ułatwia tworzenie kont i ról użytkowników oraz zarządzanie nimi, a także Określanie reguł autoryzacji opartych na użytkownikach i rolach. Aby użyć WSAT, kliknij ikonę konfiguracji ASP.NET w Eksplorator rozwiązań lub przejdź do menu witryny sieci Web lub projektu i wybierz opcję Konfiguracja ASP.NET. Jedno z metod uruchamia przeglądarkę internetową i wskazuje na WSAT na adres, na przykład: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT jest podzielony na trzy sekcje:

- **Zabezpieczenia** — zarządzanie użytkownikami, rolami i regułami autoryzacji.
- **ApplicationConfiguration** — w tym miejscu Zarządzaj &lt;AppSettings&gt; i ustawień SMTP. Możesz również przełączyć aplikację w tryb offline i zarządzać ustawieniami debugowania i śledzenia z tego miejsca, a także określić domyślną stronę błędu niestandardowego.
- **ProviderConfiguration** — Skonfiguruj dostawców używanych przez usługi aplikacji.

Sekcja zabezpieczenia (pokazana na **rysunku 1**) zawiera linki do tworzenia nowych użytkowników, zarządzania użytkownikami, tworzenia ról i zarządzania nimi oraz tworzenia reguł dostępu i zarządzania nimi. W tym miejscu możesz dodać nową rolę do systemu, usunąć istniejącego użytkownika lub dodać lub usunąć role z określonego konta użytkownika.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Rysunek 1**. sekcja zabezpieczeń WSAT zawiera opcje zarządzania użytkownikami i rolami  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](users-and-roles-on-the-production-website-cs/_static/image3.png))

Niestety, WSAT jest dostępna tylko lokalnie. Nie można odwiedzić WSAT w zdalnej witrynie produkcyjnej. Jeśli odwiedzasz, `www.yoursite.com/asp.netwebadminfiles/default.aspx` nie znaleziono odpowiedzi 404. Kod, który ma uprawnienia WSAT, używa klas `Membership` i `Roles` w .NET Framework do tworzenia, edytowania i usuwania użytkowników i ról. Te klasy zapoznaj się z informacjami o konfiguracji aplikacji sieci Web w celu określenia dostawcy do użycia; po powrocie do [ *konfigurowania witryny sieci Web korzystającej z usługi aplikacji* samouczka](configuring-a-website-that-uses-application-services-cs.md) skonfigurujemy witrynę internetową Recenzje, aby korzystać z dostawców `SqlMembershipProvider` i `SqlRoleProvider`. To wymagało dodania `<membership>` i `<roleManager>` sekcji do `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Należy zauważyć, że sekcje `<membership>` i `<roleManager>` odwołują się odpowiednio do dostawców `SqlMembershipProvider` i `SqlRoleProvider` w ich atrybutach `type`. Dostawcy te przechowują informacje o użytkowniku i roli w określonej SQL Server bazie danych. Baza danych używana przez tych dostawców jest określana przez atrybut `connectionStringName`, `ReviewsConnectionString`, który jest zdefiniowany w pliku `~/ConfigSections/databaseConnectionStrings.config`. Odwołaj, że plik `databaseConnectionStrings.config` w środowisku deweloperskim zawiera parametry połączenia z bazą danych programistycznych, a plik `databaseConnectionStrings.config` na etapie produkcji zawiera parametry połączenia z produkcyjną bazą danych.

W Nutshell, WSAT musi być dostępna lokalnie za pomocą środowiska programistycznego i współpracuje z informacjami o użytkowniku i roli w bazie danych określonej w pliku `databaseConnectionStrings.config`. W związku z tym, jeśli zmienimy informacje o parametrach połączenia w pliku `databaseConnectionStrings.config` w środowisku programistycznym, można używać WSAT lokalnie do zarządzania użytkownikami i rolami w środowisku produkcyjnym.

Aby zilustrować tę funkcję, Otwórz plik `databaseConnectionStrings.config` w programie Visual Studio w środowisku deweloperskim i Zastąp parametry połączenia z bazą danych programu Development parametrami połączenia z produkcyjną bazą danych. Następnie uruchom WSAT, przejdź na kartę Zabezpieczenia i Dodaj nowego użytkownika o nazwie sam z hasłem "Password!" (mniej znaków cudzysłowu). **Rysunek 2** przedstawia ekran WSAT podczas tworzenia tego konta.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Rysunek 2**. Tworzenie nowego użytkownika o nazwie sam w środowisku produkcyjnym  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](users-and-roles-on-the-production-website-cs/_static/image6.png))

Ponieważ Zmieniono parametry połączenia w `databaseConnectionStrings.config`, aby wskazywały na produkcyjny serwer bazy danych, sam został dodany jako użytkownik w środowisku produkcyjnym. Aby to sprawdzić, należy zmienić parametry połączenia w pliku `databaseConnectionStrings.config` z powrotem do programu Development Database, a następnie odwiedzić stronę `Login.aspx` w środowisku deweloperskim. Spróbuj zalogować się jako sam (zobacz **rysunek 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Rysunek 3**: nie można zalogować się jako sam w środowisku programistycznym  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](users-and-roles-on-the-production-website-cs/_static/image9.png))

Nie można zalogować się jako sam w środowisku programistycznym, ponieważ informacje o koncie użytkownika nie istnieją w lokalnej bazie danych. Zamiast tego została dodana do produkcyjnej bazy danych. Aby to sprawdzić, zapoznaj się z zawartością tabeli `aspnet_Users` w bazach danych programistycznych i produkcyjnych. W środowisku programistycznym dla użytkowników Scott, Jisun i Alicja powinien istnieć tylko trzy rekordy. Jednak tabela `aspnet_Users` w produkcyjnej bazie danych ma cztery rekordy: Scott, Jisun, Alicja i sam. W związku z tym sam może zalogować się za pomocą witryny sieci Web w środowisku produkcyjnym, ale nie za pomocą środowiska deweloperskiego.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Ilustracja 4**. sam zalogowanie się w produkcyjnej witrynie sieci Web  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Nie zapomnij zmienić parametrów połączenia w pliku `databaseConnectionStrings.config` z powrotem do ciągu połączenia z bazą danych deweloperskich po zakończeniu pracy z WSAT, w przeciwnym razie podczas testowania lokacji za pomocą środowiska programistycznego będziesz pracować z danymi produkcyjnymi. Należy również pamiętać, że chociaż opisana przez nas technika pozwala nam używać WSAT do zdalnego zarządzania użytkownikami i rolami, zmiany w dowolnych innych opcjach konfiguracji WSAT (reguły dostępu, ustawienia SMTP, debugowanie i śledzenie itp.) zmodyfikować plik `Web.config`. W związku z tym wszelkie zmiany wprowadzone w ustawieniach dotyczą środowiska programistycznego, a nie środowiska produkcyjnego.

## <a name="creating-custom-user-and-role-management-web-pages"></a>Tworzenie niestandardowych stron sieci Web zarządzania użytkownikami i rolami

WSAT zapewnia wyjście z systemu Box do zarządzania użytkownikami i rolami, ale można je uruchomić lokalnie i wymaga wprowadzenia zmian parametrów połączenia w celu zarządzania użytkownikami i rolami w środowisku produkcyjnym. Większość witryn sieci Web, które obsługują konta użytkowników, zawiera również wiele stron administracyjnych użytkowników i ról, które umożliwiają administratorom zarządzanie użytkownikami i rolami ze stron w tej witrynie. Takie strony administracyjne oparte na sieci Web znacznie ułatwiają zarządzanie użytkownikami i rolami, a także są niezbędne w przypadku witryn, w których może istnieć wielu administratorów lub administratorów, którzy nie mają dostępu do programu Visual Studio lub w tle technicznym do uruchamiania WSAT.

ASP.NET zawiera szereg wbudowanych kontrolek sieci Web związanych z logowaniem, które umożliwiają wdrażanie wielu z tych administracyjnych stron sieci Web jako łatwych do przeciągania i upuszczania. Na przykład możesz utworzyć stronę dla administratorów, aby utworzyć nowe konto użytkownika, przeciągając kontrolkę formancie CreateUserWizard na stronę i ustawiając kilka właściwości. W rzeczywistości Strona do tworzenia użytkowników w WSAT pokazano na **rysunku 2** używa tego samego formantu formancie CreateUserWizard, który można dodać do stron. Ponadto funkcje usług Membership i role Services są dostępne programowo za pomocą klas `Membership` i `Roles` w .NET Framework. Za pomocą tych klas można napisać kod umożliwiający tworzenie, edytowanie i usuwanie użytkowników i ról, a także dodawanie lub usuwanie użytkowników do ról, określanie użytkowników w ramach ról i wykonywanie innych zadań związanych z użytkownikami i rolami.

W obszarze [ *Konfigurowanie witryny sieci Web korzystającej z usługi aplikacji* samouczka](configuring-a-website-that-uses-application-services-cs.md) dodano stronę do `Admin` folderu o nazwie `CreateAccount.aspx`. Ta strona umożliwia administratorowi dodanie nowego konta użytkownika do lokacji i określenie, czy nowo utworzony użytkownik należy do roli administratora (patrz **rysunek 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Rysunek 5**: Administratorzy mogą tworzyć nowe konta użytkowników  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](users-and-roles-on-the-production-website-cs/_static/image15.png))

Aby uzyskać bardziej szczegółowy opis tworzenia stron administracyjnych użytkowników i ról oraz instrukcje krok po kroku dotyczące używania klas `Membership` i `Roles` i kontrolek sieci Web ASP.NET związanych z logowaniem, pamiętaj o odczytaniu [samouczków zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Znajdziesz wskazówki dotyczące sposobu kompilowania stron sieci Web na potrzeby tworzenia nowych kont, tworzenia ról i zarządzania nimi, przypisywania użytkowników do ról oraz innych typowych zadań administracyjnych.

Aby zaimplementować funkcje podobne do WSAT w produkcyjnej witrynie sieci Web, zawsze możesz utworzyć własną serię stron sieci Web, które implementują funkcje WSAT. Aby rozpocząć, zapoznaj się z kodem źródłowym WSAT, który znajduje się w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Innym rozwiązaniem jest użycie Clem Dan WSAT, który jest udostępniany w jego artykule, poprzez najęcie nowego [Narzędzia do administrowania witryną sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan przegląda czytelników przez proces tworzenia niestandardowego narzędzia przypominającego WSAT, zawiera kod źródłowy aplikacji do pobrania (w programie C#) i zawiera instrukcje krok po kroku dotyczące dodawania własnych WSAT do hostowanej witryny sieci Web.

## <a name="summary"></a>Podsumowanie

Narzędzia do administrowania witryną sieci Web ASP.NET (WSAT) można używać razem z usługami aplikacji członkostwa i ról do zarządzania informacjami o użytkowniku i rolach witryny sieci Web. Niestety, WSAT jest dostępna tylko lokalnie i nie można go odwiedzać z produkcyjnej witryny sieci Web. Jednak przez zmianę parametrów połączenia w środowisku programistycznym, aby wskazywał produkcyjną bazę danych, można użyć WSAT do zarządzania użytkownikami i rolami w produkcyjnej witrynie sieci Web.

Mimo że podejście WSAT umożliwia szybkie i łatwe zarządzanie użytkownikami i rolami, wymaga ono uruchomienia WSAT z programu Visual Studio, a także tymczasowych zmian informacji o parametrach połączenia. WSAT umożliwia szybkie zarządzanie użytkownikami i rolami w środowisku produkcyjnym, ale jest to bardzo skomplikowane i nie działa dobrze w przypadku witryn sieci Web z wieloma administratorami lub administratorami, którzy nie mają lub nie znają się w programie Visual Studio i WSAT. Z tego względu większość witryn sieci Web, które obsługują konta użytkowników, zawiera zestaw administracyjnych stron internetowych. Taki zestaw stron sieci Web eliminuje potrzebę WSAT i używany przez różnych użytkowników administracyjnych z dowolnego komputera.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Badanie ASP. Członkostwo, role i profil w sieci](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Stopniowane narzędzie do administrowania witryną sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Omówienie narzędzia do administrowania witryną sieci Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Poprzednie](precompiling-your-website-cs.md)
> [dalej](asp-net-hosting-options-vb.md)
