---
uid: whitepapers/overview
title: Oficjalne dokumenty | Microsoft Docs
author: rick-anderson
description: Na tej stronie znajdują się oficjalne dokumenty ułatwiające zainstalowanie i skonfigurowanie ASP.NET oraz ułatwiające pisanie bezpiecznych, szybko i elastycznych aplikacji ASP.NET.
ms.author: riande
ms.date: 11/15/2011
ms.assetid: d5e79470-01f2-4d65-8077-11c3e10a6784
msc.legacyurl: /whitepapers
msc.type: content
ms.openlocfilehash: 1a3a9fe5d685d4b38efe666fc88ff57016482ada
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640855"
---
# <a name="whitepapers"></a>Oficjalne dokumenty

> Na tej stronie znajdują się oficjalne dokumenty ułatwiające zainstalowanie i skonfigurowanie ASP.NET oraz ułatwiające pisanie bezpiecznych, szybko i elastycznych aplikacji ASP.NET.
> 
> - [ASP.NET 4](#aspnet4)
> - [Oficjalne dokumenty dotyczące zabezpieczeń ASP.NET](#security)
> - [Oficjalne dokumenty dotyczące instalacji i konfiguracji](#setup)
> - [Oficjalne dokumenty SQL Server](#sql)
> - [Oficjalne dokumenty](#general)

<a id="aspnet4"></a>
## <a name="aspnet-4"></a>ASP.NET 4

Informacje dotyczące ASP.NET 4 i programu Visual Studio 2010.

[ASP.NET MVC 4 — Informacje o wersji](mvc4-beta-release-notes.md "MVC4-Release-uwagi")

W tym dokumencie opisano nowe funkcje i ulepszenia wprowadzone w ASP.NET MVC 4 Developer Preview dla programu Visual Studio 2010, a także informacje o instalacji i znane problemy.

[ASP.NET MVC 3 — informacje o wersji](mvc3-release-notes.md "MvC3-Release-uwagi")

W tym dokumencie opisano nowe funkcje i ulepszenia wprowadzone w ASP.NET MVC 3, a także informacje o instalacji i znane problemy.

[Omówienie projektowania aplikacji internetowych na platformie ASP.NET 4 i w programie Visual Studio 2010](aspnet4/index.md "aspnet4")

W .NET Framework w wersji 4 są dostępne liczne atrakcyjne zmiany dotyczące usługi ASP.NET. Ten dokument zawiera omówienie wielu nowych funkcji, które są zawarte w nadchodzącym wydaniu.

[Zmiany w ASP.NET 4 beta 2](aspnet4/breaking-changes.md "zakłócenia — zmiany")

Ten dokument zawiera informacje o zmianach wprowadzonych dla wydania .NET Framework w wersji 4 beta 2 (czyli wersji ASP.NET 4 beta 2), które mogą mieć wpływ na aplikacje, które zostały utworzone przy użyciu wcześniejszych wersji, w tym wydanie ASP.NET 4 beta 1.

[Co nowego we wzorcu ASP.NET MVC 2](what-is-new-in-aspnet-mvc.md "Co nowego w ASPNET MVC")

W tym dokumencie opisano nowe funkcje i ulepszenia wprowadzone w ASP.NET MVC 2.

[Uaktualnianie wzorca ASP.NET MVC 1.0 do ASP.NET MVC 2](aspnet-mvc2-upgrade-notes.md "ASPNET-MVC2-upgrade-uwagi")

ASP.NET MVC 2 można zainstalować równolegle z ASP.NET MVC 1,0 na tym samym serwerze. Dzięki temu deweloperzy aplikacji mogą elastycznie wybierać, kiedy uaktualnić aplikację ASP.NET MVC 1,0 do ASP.NET MVC 2. Ten dokument descibes zarówno sposób uaktualniania ręcznego, jak i kreatora w wizualizacji...

<a id="security"></a>
## <a name="aspnet-security-whitepapers"></a>Oficjalne dokumenty dotyczące zabezpieczeń ASP.NET

Bezpieczeństwo jest ważnym aspektem aplikacji internetowych, a te oficjalne dokumenty omawiają, jak projektować i implementować bezpieczne aplikacje ASP.NET.

[Instrumentacja ASP.NET 2,0 aplikacji na potrzeby zabezpieczeń](https://msdn.microsoft.com/library/ms998325.aspx)

W tym przykładzie pokazano, jak używać niestandardowych zdarzeń monitorowania kondycji w celu Instrumentacji aplikacji ASP.NET do śledzenia zdarzeń i operacji związanych z zabezpieczeniami. ASP.NET wersja 2,0 zapewnia monitorowanie kondycji, które obejmuje instrumentację dla wielu standardów...

[Wykonaj przegląd wdrożenia zabezpieczeń dla ASP.NET 2,0](https://msdn.microsoft.com/library/ms998367.aspx)

W tym przykładzie pokazano, jak wykonać przegląd wdrożenia zabezpieczeń dla aplikacji ASP.NET 2,0 w celu zidentyfikowania potencjalnych luk w zabezpieczeniach wprowadzonych przez nieodpowiednie ustawienia konfiguracji. Większość procesu przeglądu obejmuje wykonywanie operacji...

[Korzystanie z ADAM dla ról w ASP.NET 2,0](https://msdn.microsoft.com/library/ms998331.aspx)

W tym przykładzie pokazano, jak można opracowywać witrynę sieci Web ASP.NET, która używa trybu aplikacji Active Directory (ADAM) do przechowywania ról ASP.NET. Pokazano, jak skonfigurować ADAM i Magazyn zasad programu Authorization Manager (AzMan), jak utworzyć nowe role i...

[Korzystanie z Menedżera autoryzacji (AzMan) z ASP.NET 2,0](https://msdn.microsoft.com/library/ms998336.aspx)

W tym temacie pokazano, jak używać Menedżera autoryzacji (AzMan) w połączeniu z interfejsem API menedżera ról ASP.NET do zarządzania rolami, sprawdzania przynależności do roli użytkownika i autoryzacji ról do wykonywania określonych operacji w magazynie zasad AzMan. Jak to zrobić...

[Korzystanie z członkostwa w ASP.NET 2,0](https://msdn.microsoft.com/library/ms998347.aspx)

W tym temacie przedstawiono sposób korzystania z funkcji członkostwa w aplikacjach ASP.NET w wersji 2,0. Pokazuje, jak używać dwóch różnych dostawców członkostwa: ActiveDirectoryMembershipProvider i SqlMembershipProvider. Funkcja członkostwa...

[Korzystanie z menedżera ról w ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)

W tym przykładzie przedstawiono sposób korzystania z menedżera ról ASP.NET 2,0. Menedżer ról upraszcza zadanie zarządzania rolami i wykonywania autoryzacji opartej na rolach w aplikacji. Pokazano, jak skonfigurować różnych dostawców ról do użycia z...

[Używanie uwierzytelniania systemu Windows w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998358.aspx)

W tym temacie opisano sposób konfigurowania i używania uwierzytelniania systemu Windows w aplikacji sieci Web ASP.NET. Uwierzytelnianie systemu Windows jest preferowanym podejściem, gdy użytkownicy są częścią domeny systemu Windows. Takie podejście umożliwia korzystanie z istniejącego magazynu tożsamości...

[Wykonaj przegląd kodu zabezpieczeń dla kodu zarządzanego (działanie linii bazowej)](https://msdn.microsoft.com/library/ms998364.aspx)

W tym przykładzie pokazano, jak przeprowadzać przeglądy kodu zabezpieczeń. Ten moduł przedstawia kroki związane z działaniem oraz techniki analizowania wyników. Użyj tej metody z "listą pytań zabezpieczających: kod zarządzany (.NET Framework 2,0)"...

[Wykonaj przegląd wdrożenia zabezpieczeń dla ASP.NET 2,0](https://msdn.microsoft.com/library/ms998367.aspx)

W tym przykładzie pokazano, jak wykonać przegląd wdrożenia zabezpieczeń dla aplikacji ASP.NET 2,0 w celu zidentyfikowania potencjalnych luk w zabezpieczeniach wprowadzonych przez nieodpowiednie ustawienia konfiguracji. Większość procesu przeglądu obejmuje wykonywanie operacji...

[Implementowanie delegowania protokołu Kerberos dla systemu Windows 2000](https://msdn.microsoft.com/library/aa302400.aspx)

Delegowanie protokołu Kerberos umożliwia przekazanie uwierzytelnionej tożsamości między wieloma warstwami fizycznymi aplikacji w celu zapewnienia obsługi uwierzytelniania i autoryzacji podrzędnej. W tym przykładzie przedstawiono kroki konfiguracji wymagane do wykonania tej czynności.

[Korzystanie z personifikacji i delegowania w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998351.aspx)

W tym temacie pokazano, jak i kiedy należy używać personifikacji w aplikacjach ASP.NET 2,0. Domyślnie Personifikacja jest wyłączona i dostęp do zasobów można uzyskać przy użyciu tożsamości procesu aplikacji sieci Web ASP.NET. Można jednak użyć...

[Tworzenie modelu zagrożeń dla aplikacji sieci Web w czasie projektowania](https://msdn.microsoft.com/library/ms978527.aspx)

W tym temacie opisano sposób tworzenia modelu zagrożeń dla aplikacji sieci Web. Działanie modelowania zagrożeń ułatwia Modelowanie projektu zabezpieczeń, dzięki czemu możesz ujawniać potencjalne wady i luki w zabezpieczeniach przed inwestycją...

### <a name="forms-authentication"></a>Uwierzytelnianie formularzy

[Ochrona uwierzytelniania formularzy w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)

W tym przykładzie przedstawiono sposób bezpiecznego konfigurowania i używania uwierzytelniania formularzy przy użyciu aplikacji ASP.NET 2,0. Kluczowe czynniki, które należy wziąć pod uwagę, obejmują odpowiednie zabezpieczenie biletu uwierzytelniania i zabezpieczanie magazynu tożsamości użytkowników oraz dostęp do tego magazynu. Przyciski ...

[Używanie uwierzytelniania formularzy Active Directory w ASP.NET 2,0](https://msdn.microsoft.com/library/ms998360.aspx)

W tym przykładzie pokazano, jak używać uwierzytelniania formularzy w usłudze Microsoft® Active Directory® Directory za pomocą ActiveDirectoryMembershipProvider. Instrukcje przedstawiające sposób konfigurowania dostawcy i tworzenia i uwierzytelniania użytkowników...

[Używanie uwierzytelniania formularzy Active Directory w wielu domenach w systemie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998345.aspx)

W tym przykładzie pokazano, jak używać uwierzytelniania formularzy w usłudze Microsoft® Active Directory® Directory za pomocą ActiveDirectoryMembershipProvider. Instrukcje przedstawiające sposób konfigurowania dostawcy i tworzenia i uwierzytelniania użytkowników...

[Używanie uwierzytelniania formularzy SQL Server w ASP.NET 2,0](https://msdn.microsoft.com/library/ms998317.aspx)

W tym przykładzie pokazano, jak można użyć uwierzytelniania formularzy przy użyciu dostawcy członkostwa SQL Server. Uwierzytelnianie formularzy przy użyciu SQL Server jest najbardziej odpowiednie w sytuacjach, gdy użytkownicy aplikacji nie są częścią domeny systemu Windows, a w efekcie,...

[Tworzenie obiektów GenericPrincipal — z uwierzytelnianiem formularzy w ASP.NET 1,1](https://msdn.microsoft.com/library/aa302399.aspx)

W tym przykładzie pokazano, jak utworzyć i obsłużyć obiekty GenericPrincipal — i FormsIdentity podczas korzystania z uwierzytelniania formularzy.

[Używanie uwierzytelniania formularzy Active Directory w ASP.NET 1,1](https://msdn.microsoft.com/library/aa302397.aspx)

W tym artykule opisano sposób wdrażania uwierzytelniania formularzy w Active Directory magazynie poświadczeń.

[Używanie uwierzytelniania formularzy SQL Server w ASP.NET 1,1](https://msdn.microsoft.com/library/aa302398.aspx)

W tym przykładzie pokazano, jak zaimplementować uwierzytelnianie formularzy na SQL Server magazynie poświadczeń. Pokazano w nim także, jak przechowywać skróty haseł w bazie danych programu.

### <a name="user-input-data-validation"></a>Sprawdzanie poprawności danych wejściowych użytkownika

[Żądanie weryfikacji — zapobieganie atakom skryptów](request-validation.md "żądanie — Walidacja")

W tym dokumencie opisano funkcję walidacji żądania ASP.NET, gdzie Domyślnie aplikacja nie może przetwarzać niekodowanej zawartości HTML przesłanej do serwera. Ta funkcja walidacji żądania może zostać wyłączona, gdy aplikacja jest...

[Zapobiegaj skryptom między lokacjami w ASP.NET](https://msdn.microsoft.com/library/ms998274.aspx)

W tym przykładzie przedstawiono sposób, w jaki można ułatwić ochronę aplikacji ASP.NET przed atakami na skrypty między lokacjami przy użyciu właściwych technik walidacji danych wejściowych i kodowania danych wyjściowych. Opisano w nim również różne mechanizmy ochrony, których można użyć w...

[Ochrona przed iniekcją kodu SQL w ASP.NET](https://msdn.microsoft.com/library/ms998271.aspx)

W ten sposób przedstawiono kilka sposobów, aby pomóc w ochronie aplikacji ASP.NET przed atakami polegającymi na iniekcji kodu SQL. Iniekcja kodu SQL może wystąpić, gdy aplikacja używa danych wejściowych do konstruowania dynamicznych instrukcji SQL lub gdy używa procedur składowanych do łączenia się z...

[Używanie wyrażeń regularnych w celu ograniczenia danych wejściowych w ASP.NET](https://msdn.microsoft.com/library/ms998267.aspx)

W tym przykładzie pokazano, jak można używać wyrażeń regularnych w aplikacjach ASP.NET w celu ograniczenia niezaufanych danych wejściowych. Wyrażenia regularne to dobry sposób na zweryfikowanie pól tekstowych, takich jak nazwy, adresy, numery telefonów i inne informacje o użytkowniku. Możesz użyć...

### <a name="code-access-security"></a>Zabezpieczenia dostępu kodu

[Korzystanie z zabezpieczeń dostępu kodu w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998326.aspx)

W tym przykładzie pokazano, jak wybrać odpowiedni poziom zaufania dla aplikacji i w razie potrzeby, jak utworzyć niestandardowy plik zasad zabezpieczeń dostępu kodu ASP.NET w celu zdefiniowania niestandardowego poziomu zaufania. Możesz użyć innego zaufania zabezpieczenia dostępu kodu...

[Tworzenie niestandardowego uprawnienia do szyfrowania](https://msdn.microsoft.com/library/aa302362.aspx)

W tym artykule opisano sposób tworzenia niestandardowego uprawnienia dostępu do kodu do sterowania dostępem programistycznym do funkcji niezarządzanego szyfrowania, które zapewnia interfejs API usługi Win32® Data Protection (DPAPI). Użyj tego niestandardowego uprawnienia z zarządzaną otoką DPAPI...

[Używanie zasad zabezpieczeń dostępu kodu w celu ograniczenia zestawu](https://msdn.microsoft.com/library/aa302361.aspx)

Administrator może skonfigurować zasady zabezpieczeń dostępu kodu w celu ograniczenia operacji kodu .NET Framework (zestawy). W tym kroku opisano sposób konfigurowania zasad zabezpieczeń dostępu kodu w celu ograniczenia możliwości zestawu do wykonywania operacji we/wy na plikach i ograniczania...

### <a name="communications-security"></a>Bezpieczeństwo komunikacji

[Konfigurowanie protokołu SSL na serwerze sieci Web](https://msdn.microsoft.com/library/aa302411.aspx)

Serwer sieci Web musi być skonfigurowany pod kątem protokołu SSL w celu obsługi połączeń HTTPS z aplikacji klienckich. W tym przykładzie pokazano, jak skonfigurować protokół SSL na serwerze sieci Web.

[Konfigurowanie certyfikatów klienta](https://msdn.microsoft.com/library/aa302412.aspx)

Usługi IIS obsługują uwierzytelnianie certyfikatu klienta. W tym przykładzie pokazano, jak skonfigurować aplikację sieci Web tak, aby wymagała certyfikatów klienta. Przedstawiono w nim także sposób instalowania certyfikatu na komputerze klienckim i używania go podczas wywoływania aplikacji sieci Web.

[Używanie protokołu IPSec do filtrowania portów i uwierzytelniania](https://msdn.microsoft.com/library/aa302366.aspx)

Zabezpieczenia protokołu internetowego (IPSec) to protokół, a nie usługa, która zapewnia szyfrowanie, integralność i usługi uwierzytelniania dla ruchu sieciowego opartego na protokole IP. Ponieważ protokół IPSec zapewnia ochronę serwer-serwer, można użyć protokołu IPSec do licznika zagrożeń wewnętrznych...

[Użyj protokołu IPSec, aby zapewnić bezpieczną komunikację między dwoma serwerami](https://msdn.microsoft.com/library/aa302413.aspx)

Protokół IPSec jest technologią zapewnianą przez system Windows 2000, która umożliwia tworzenie szyfrowanych kanałów między dwoma serwerami. Protokołu IPSec można używać do filtrowania ruchu IP i uwierzytelniania serwerów. W tym przykładzie przedstawiono sposób konfigurowania protokołu IPSec w celu zapewnienia bezpiecznego (szyfrowanego)...

[Zabezpieczanie komunikacji z usługą SQL Server przy użyciu protokołu SSL](https://msdn.microsoft.com/library/aa302414.aspx)

Często ważne jest, aby aplikacje mogły zabezpieczyć dane przesyłane do i z serwera bazy danych SQL Server. Za pomocą SQL Server można utworzyć zaszyfrowany kanał przy użyciu protokołu SSL. W tym przykładzie pokazano, jak zainstalować certyfikat na serwerze bazy danych,...

[Wywoływanie usługi sieci Web przy użyciu certyfikatów klienta z ASP.NET 1,1](https://msdn.microsoft.com/library/aa302408.aspx)

W tym artykule opisano, jak można przekazać certyfikat klienta do usługi sieci Web w celu uwierzytelnienia z aplikacji sieci Web ASP.NET lub z aplikacji Windows Forms. Certyfikat klienta można zainstalować w magazynie komputera lokalnego lub w magazynie użytkownika. If...

[Wywoływanie usługi sieci Web przy użyciu protokołu SSL z ASP.NET 1,1](https://msdn.microsoft.com/library/aa302409.aspx)

Szyfrowanie SSL (SSL) może służyć do zagwarantowania integralności i poufności komunikatów przekazywanych do i z usługi sieci Web. W tym przykładzie przedstawiono sposób użycia protokołu SSL z usługami sieci Web.

### <a name="cryptography"></a>Kryptografia

[Tworzenie biblioteki DPAPI w programie .NET 1,1](https://msdn.microsoft.com/library/aa302402.aspx)

W tym przykładzie przedstawiono sposób tworzenia zarządzanej biblioteki klas, która udostępnia funkcje DPAPI aplikacjom, które chcą szyfrować dane, na przykład parametry połączenia bazy danych i poświadczenia konta.

[Tworzenie biblioteki szyfrowania w programie .NET 1,1](https://msdn.microsoft.com/library/aa302405.aspx)

W tym przykładzie przedstawiono sposób tworzenia zarządzanej biblioteki klas w celu zapewnienia funkcjonalności szyfrowania dla aplikacji. Umożliwia aplikacji wybranie algorytmu szyfrowania. Obsługiwane algorytmy to DES, Triple DES, RC2 i Rijndael.

[Przechowywanie zaszyfrowanych parametrów połączenia w rejestrze w ASP.NET 1,1](https://msdn.microsoft.com/library/aa302406.aspx)

Aplikacje mogą wybrać przechowywanie zaszyfrowanych danych, takich jak parametry połączenia i poświadczenia konta w rejestrze systemu Windows. W tym temacie pokazano, jak przechowywać i pobierać zaszyfrowane ciągi w rejestrze.

[Korzystanie z interfejsu DPAPI (magazynu komputerowego) z ASP.NET 1,1](https://msdn.microsoft.com/library/aa302403.aspx)

W tym przykładzie pokazano, jak używać interfejsu DPAPI z aplikacji sieci Web ASP.NET lub usługi sieci Web do szyfrowania poufnych danych.

[Używanie interfejsu DPAPI (User Store) z ASP.NET 1,1 z usługami przedsiębiorstwa](https://msdn.microsoft.com/library/aa302404.aspx)

W tym przykładzie pokazano, jak używać interfejsu DPAPI z aplikacji sieci Web ASP.NET lub usługi do szyfrowania poufnych danych. W tym sposób użyto interfejsu DPAPI z magazynem użytkowników, który wymaga użycia składnika usług dla przedsiębiorstw poza procesem.

<a id="setup"></a>
## <a name="installation-and-setup-whitepapers"></a>Oficjalne dokumenty dotyczące instalacji i konfiguracji

Te oficjalne dokumenty zawierają instrukcje krok po kroku dotyczące instalowania i konfigurowania ASP.NET na serwerze.

[Tworzenie konta usługi dla aplikacji ASP.NET 2,0](https://msdn.microsoft.com/library/ms998297.aspx)

W tym przykładzie pokazano, jak utworzyć i skonfigurować niestandardowe konto usługi z najniższym uprzywilejowaniem w celu uruchomienia aplikacji sieci Web ASP.NET. Domyślnie aplikacja ASP.NET w systemach Microsoft Windows Server 2003 i IIS 6,0 jest uruchamiana przy użyciu wbudowanej usługi sieciowej...

[Zwiększ bezpieczeństwo podczas hostowania wielu aplikacji w ASP.NET 2,0](https://msdn.microsoft.com/library/aa480478.aspx)

W tym temacie pokazano, jak można izolować wiele aplikacji od siebie i z udostępnionych zasobów systemowych w środowisku hostingu w sieci Web. Środowisko hostingu może być serwerem sieci Web udostępnianym przez usługodawcę internetowego (ISP), który hostuje wiele...

[Użyj średniego zaufania w ASP.NET 2,0](https://msdn.microsoft.com/library/ms998341.aspx)

W tym temacie pokazano, jak skonfigurować aplikacje sieci Web ASP.NET do uruchamiania w średnim zaufaniu. Jeśli na tym samym serwerze jest hostowana wiele aplikacji, można użyć zabezpieczeń dostępu kodu i średniego poziomu zaufania, aby zapewnić izolację aplikacji. Przez ustawienie...

[Korzystanie z konta usługi sieciowej w celu uzyskania dostępu do zasobów w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998320.aspx)

W tym przykładzie pokazano, jak można użyć konta komputera usługi NT AUTHORITY\Network do uzyskiwania dostępu do zasobów lokalnych i sieciowych. Domyślnie w systemie Windows Server 2003 aplikacje ASP.NET są uruchamiane przy użyciu tożsamości tego konta. Jest to najmniej uprzywilejowane...

[Używanie przejścia protokołu i delegowania ograniczonego w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998355.aspx)

W tym przykładzie przedstawiono sposób konfigurowania i używania przejścia protokołu oraz delegowania ograniczonego, aby umożliwić aplikacji ASP.NET dostęp do zasobów sieciowych podczas personifikacji oryginalnego obiektu wywołującego. System operacyjny Microsoft® Windows® 2000...

[Wykonywanie równoczesne aplikacji z programem .NET Framework 1.0 i 1.1 na platformie ASP.NET](side-by-side-with-10.md "obok siebie z 1,0")

W tym dokumencie opisano sposób instalowania programów .NET 1,0 i .NET 1,1 na komputerze, co pozwala na uruchamianie aplikacji sieci Web ASP.NET w dowolnej wersji platformy.

[Platforma ASP.NET odmawia dostępu do katalogów usług IIS](denied-access-to-iis-directories.md "Odmowa dostępu do katalogów usług IIS")

W tym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji ASP.NET zwróci błąd, "odmowa dostępu do katalogu *DirectoryName* . Nie można rozpocząć monitorowania zmian w katalogu. "

[Uruchamianie platformy ASP.NET 1.1 z usługami IIS 6.0](aspnet-and-iis6.md "ASPNET i usług IIS 6")

Mimo że system Windows Server 2003 obejmuje zarówno IIS 6,0, jak i ASP.NET 1,1, te składniki są domyślnie wyłączone. W tym dokumencie opisano sposób włączania usług IIS 6,0 i ASP.NET 1,1 oraz zalecane jest użycie kilku ustawień konfiguracji w celu optymalnego...

[Naprawa błędu "niedostępność aplikacji serwera" po zastosowaniu aktualizacji zabezpieczeń dla programu IE](ms03-32-issue.md "MS03 — wersja 32-problem")

W tym dokumencie opisano poprawkę, która rozwiązuje problem z aktualizacją zabezpieczeń MS03-32 dla programu Internet Explorer, która ma wpływ na aplikacje ASP.NET 1,0 działające w systemie Windows XP Professional.

[Utwórz konto niestandardowe do uruchomienia ASP.NET 1,1](https://msdn.microsoft.com/library/aa302396.aspx)

Aplikacje sieci Web ASP.NET są zwykle uruchamiane przy użyciu wbudowanego konta ASPNET. W takim przypadku warto zamiast tego użyć konta niestandardowego. W tym artykule opisano sposób tworzenia najmniejszego uprzywilejowanego konta lokalnego do uruchamiania aplikacji sieci Web ASP.NET. Przyciski ...

### <a name="configuration"></a>Konfiguracja

[Konfigurowanie klucza komputera w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx)

W tym temacie objaśniono &lt;machineKey&gt; elementu w pliku Web. config i pokazano, jak skonfigurować element &lt;machineKey&gt;, aby kontrolować weryfikację i szyfrowanie elementów ViewState, biletów uwierzytelniania formularzy i plików cookie ról. Stan widoku jest podpisany...

[Szyfrowanie sekcji konfiguracyjnych w programie ASP.NET 2,0 przy użyciu funkcji DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)

W tym temacie przedstawiono sposób użycia dostawcy konfiguracji chronionej przez interfejs API ochrony danych (DPAPI) systemu Windows oraz narzędzia ASPNET\_regiis. exe w celu zaszyfrowania sekcji plików konfiguracji. Do... można użyć narzędzia ASPNET\_regiis. exe.

[Szyfruj sekcje konfiguracyjne w ASP.NET 2,0 przy użyciu RSA](https://msdn.microsoft.com/library/ms998283.aspx)

W tym temacie pokazano, jak używać dostawcy konfiguracji chronionej przez RSA i narzędzia ASPNET\_regiis. exe, aby szyfrować sekcje plików konfiguracji. Za pomocą narzędzia ASPNET\_regiis. exe można szyfrować poufne dane, takie jak parametry połączenia, przechowywane w...

[Korzystanie z personifikacji i delegowania w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998351.aspx)

W tym temacie pokazano, jak i kiedy należy używać personifikacji w aplikacjach ASP.NET 2,0. Domyślnie Personifikacja jest wyłączona i dostęp do zasobów można uzyskać przy użyciu tożsamości procesu aplikacji sieci Web ASP.NET. Można jednak użyć...

<a id="sql"></a>
## <a name="sql-server-whitepapers"></a>Oficjalne dokumenty SQL Server

Mimo że ASP.NET współpracuje z różnymi bazami danych, te oficjalne dokumenty szukają w celu nawiązania połączenia z aplikacjami ASP.NET do SQL Server.

[Nawiązywanie połączenia z usługą SQL Server przy użyciu uwierzytelniania SQL w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998300.aspx)

W tym przykładzie przedstawiono sposób bezpiecznego łączenia aplikacji ASP.NET z usługą Microsoft® SQL Server™, gdy uwierzytelnianie dostępu do bazy danych korzysta z natywnego uwierzytelniania SQL. Uwierzytelnianie systemu Windows jest zalecanym sposobem nawiązywania połączenia z usługą SQL Server...

[Nawiązywanie połączenia z usługą SQL Server przy użyciu uwierzytelniania systemu Windows w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998292.aspx)

W tym przykładzie przedstawiono sposób nawiązywania połączenia z SQL Server 2000 przy użyciu konta usługi systemu Windows z aplikacji ASP.NET w wersji 2,0. Jeśli jest to możliwe, należy użyć uwierzytelniania systemu Windows zamiast uwierzytelniania SQL, ponieważ nie należy przechowywać poświadczeń w...

[Zabezpieczanie komunikacji przy użyciu protokołu SSL SQL Server 2000](https://msdn.microsoft.com/library/aa302414.aspx)

Często ważne jest, aby aplikacje mogły zabezpieczyć dane przesyłane do i z serwera bazy danych SQL Server. Za pomocą SQL Server można utworzyć zaszyfrowany kanał przy użyciu protokołu SSL. W tym przykładzie przedstawiono sposób instalowania certyfikatu na serwerze bazy danych,...

<a id="general"></a>
## <a name="general-whitepapers"></a>Oficjalne dokumenty

W poniższej analizie przypadku opisano proces służący do migrowania witryn sieci Web społeczności .NET firmy Microsoft ze tradycyjnego środowiska hostingu do Microsoft Azure.

[Migrowanie witryn sieci Web ASP.NET i IIS.NET społeczności firmy Microsoft do Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=400656)

Te oficjalne dokumenty obejmują różne tematy dotyczące ASP.NET.

[Korzystanie z monitorowania kondycji w programie ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx)

W tym przykładzie pokazano, jak używać monitorowania kondycji do Instrumentacji aplikacji dla zdarzenia niestandardowego. Aby utworzyć niestandardowe zdarzenie monitorowania kondycji, należy utworzyć klasę pochodzącą z elementu System. Web. Management. WebBaseEvent, skonfigurować&gt; &lt;healthMonitoring...

[Zaimplementuj zarządzanie poprawkami](https://msdn.microsoft.com/library/aa302364.aspx)

W tym temacie objaśniono zarządzanie poprawkami, w tym sposób zapewnienia aktualności jednego lub wielu serwerów. Dodatkowe oprogramowanie nie jest wymagane, z wyjątkiem narzędzi dostępnych do pobrania przez firmę Microsoft. Zasady działania i zabezpieczeń powinny przyjąć zarządzanie poprawkami...

[Formanty serwera ASP.NET dla programu Silverlight w zestawie SDK Silverlight 3](https://go.microsoft.com/fwlink/?LinkId=153377)

Formanty serwera ASP.NET dla programu Silverlight ("ASP.NET Silverlight Controls"), które są kontrolkami MediaPlayer ASP.NET i Silverlight, zostały usunięte z zestawu Silverlight SDK dla programu Silverlight w wersji 3. Ten dokument zawiera wskazówki dla deweloperów, którzy pracowali z tymi ASP.NETami...

[Tworzenie aplikacji sieci Web o wysokiej wydajności](https://devexpress.com/act)

Dowiedz się, jak korzystać z nowych funkcji w bibliotece ASP.NET AJAX do kompilowania aplikacji sieci Web o wysokiej wydajności
