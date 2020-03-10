---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Podstawy zabezpieczeń i obsługa ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Jest to pierwszy samouczek z serii samouczków, które będą eksplorować techniki uwierzytelniania odwiedzających za pomocą formularza sieci Web, co upoważnia dostęp do części...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: 99e986013cb5a923ddb150022013e3a75852ce55
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575342"
---
# <a name="security-basics-and-aspnet-support-vb"></a>Podstawy zabezpieczeń i obsługa platformy ASP.NET (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> Jest to pierwszy samouczek w szeregu samouczków, który będzie eksplorować techniki uwierzytelniania odwiedzających za pomocą formularza sieci Web, autoryzowania dostępu do określonych stron i funkcji oraz zarządzania kontami użytkowników w aplikacji ASP.NET.

## <a name="introduction"></a>Wprowadzenie

Co to jest jedna z forów, handlu elektronicznego witryn, witryny internetowej poczty e-mail, witryny sieci Web portalu i witryny sieci społecznościowej? Wszystkie *konta użytkowników*oferują. Lokacje, które oferują konta użytkowników, muszą mieć pewną liczbę usług. Co najmniej nowi odwiedzający muszą mieć możliwość utworzenia konta, a odwiedzający musi być w stanie zalogować się. Takie aplikacje sieci Web mogą podejmować decyzje na podstawie zalogowanego użytkownika: Niektóre strony i akcje mogą być ograniczone do zalogowanych użytkowników lub do określonego podzbioru użytkowników. Inne strony mogą zawierać informacje specyficzne dla zalogowanego użytkownika lub mogą pokazać więcej lub mniej informacji w zależności od tego, co użytkownik przegląda stronę.

Jest to pierwszy samouczek w szeregu samouczków, który będzie eksplorować techniki uwierzytelniania odwiedzających za pomocą formularza sieci Web, autoryzowania dostępu do określonych stron i funkcji oraz zarządzania kontami użytkowników w aplikacji ASP.NET. W ramach tych samouczków sprawdzimy, jak:

- Identyfikowanie i Logowanie użytkowników w witrynie sieci Web
- Użyj ASP. Struktura przynależności do sieci w celu zarządzania kontami użytkowników
- Tworzenie, aktualizowanie i usuwanie kont użytkowników
- Ograniczanie dostępu do strony sieci Web, katalogu lub określonych funkcji na podstawie zalogowanego użytkownika
- Użyj ASP. Struktura ról w sieci do kojarzenia kont użytkowników z rolami
- Zarządzanie rolami użytkowników
- Ograniczanie dostępu do strony sieci Web, katalogu lub określonych funkcji na podstawie roli zalogowanego użytkownika
- Dostosowuj i zwiększaj ASP. Kontrolki sieci Web zabezpieczeń sieciowych

Te samouczki są zwięzłe i zawierają instrukcje krok po kroku dotyczące dużej ilości zrzutów ekranu, aby przeprowadzić Cię przez proces wizualnie. Każdy samouczek jest dostępny w C# systemach i Visual Basic wersje i zawiera pobieranie pełnego użytego kodu. (Ten pierwszy samouczek koncentruje się na pojęciach związanych z bezpieczeństwem w punkcie widzenia wysokiego poziomu i w związku z tym nie zawiera żadnego skojarzonego kodu).

W tym samouczku omówiono ważne koncepcje dotyczące zabezpieczeń oraz funkcje, które są dostępne w programie ASP.NET, aby pomóc w zaimplementowaniu uwierzytelniania formularzy, autoryzacji, kont użytkowników i ról. Zacznijmy!

> [!NOTE]
> Zabezpieczenia to istotny aspekt każdej aplikacji, która obejmuje decyzje dotyczące fizycznych, technologicznych i zasad oraz wymaga wysokiego stopnia planowania i wiedzy o domenie. Ta seria samouczków nie jest przeznaczona do tworzenia bezpiecznych aplikacji sieci Web. Jest to raczej ukierunkowane na uwierzytelnianie formularzy, autoryzację, konta użytkowników i role. Niektóre koncepcje dotyczące zabezpieczeń, które koncentrują się na tym zagadnieniu, zostały omówione w tej serii, a inne pozostaną niezbadane.

## <a name="authentication-authorization-user-accounts-and-roles"></a>Uwierzytelnianie, autoryzacja, konta użytkowników i role

Uwierzytelnianie, autoryzacja, konta użytkowników i role to cztery terminy, które będą często używane w tej serii samouczków, dzięki czemu chcę szybko określić te warunki w kontekście zabezpieczeń sieci Web. W modelu klient-serwer, takim jak Internet, istnieje wiele scenariuszy, w których serwer musi identyfikować klienta wysyłającego żądanie. *Uwierzytelnianie* to proces ustalania tożsamości klienta. Klient, który został pomyślnie zidentyfikowany, jest uznawany za *uwierzytelniony*. Niezidentyfikowany klient jest uznawany za *nieuwierzytelniony* lub *anonimowy*.

Bezpieczne systemy uwierzytelniania obejmują co najmniej jeden z następujących trzech aspektów: coś, co [wiesz, lub coś,](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html)co ty masz. Większość aplikacji sieci Web polega na tym, co zna klient, na przykład hasła lub numeru PIN. Informacje używane do identyfikowania użytkownika i hasła, na przykład, są określane jako *poświadczenia*. Ta seria samouczków koncentruje się na *uwierzytelnianiu formularzy*, który jest modelem uwierzytelniania, w którym użytkownicy logują się do witryny, podając swoje poświadczenia w formularzu strony sieci Web. Mamy już ten typ uwierzytelniania. Przejdź do dowolnej witryny handlu elektronicznego. Gdy wszystko będzie gotowe do wyewidencjonowania, pojawi się prośba o zalogowanie się, wprowadzając nazwę użytkownika i hasło do pól tekstowych na stronie sieci Web.

Oprócz identyfikowania klientów serwer może wymagać ograniczenia zasobów lub funkcji dostępnych w zależności od klienta wysyłającego żądanie. *Autoryzacja* to proces ustalania, czy określony użytkownik ma uprawnienia dostępu do określonego zasobu lub funkcji.

*Konto użytkownika* to magazyn służący do utrwalania informacji o konkretnym użytkowniku. Konta użytkowników muszą zawierać minimalne informacje, które jednoznacznie identyfikują użytkownika, takie jak nazwa logowania użytkownika i hasło. Wraz z tymi ważnymi informacjami konta użytkowników mogą zawierać takie elementy, jak: adres e-mail użytkownika; Data i godzina utworzenia konta; Data i godzina ostatniego logowania; Imię i nazwisko; numer telefonu; i adres wysyłkowy. W przypadku korzystania z uwierzytelniania formularzy informacje o koncie użytkownika są zwykle przechowywane w relacyjnej bazie danych, takiej jak Microsoft SQL Server.

Aplikacje sieci Web, które obsługują konta użytkowników, mogą opcjonalnie grupować użytkowników w *rolach*. Rola jest po prostu etykietą zastosowana do użytkownika i zawiera streszczenie do definiowania reguł autoryzacji i funkcji na poziomie strony. Na przykład witryna sieci Web może zawierać rolę administratora z regułami autoryzacji, które uniemożliwiają osobom, ale administratorom dostęp do określonego zestawu stron sieci Web. Ponadto różne strony, które są dostępne dla wszystkich użytkowników (w tym inne niż administratorzy), mogą wyświetlać dodatkowe dane lub oferować dodatkowe funkcje, które są odwiedzane przez użytkowników w roli Administratorzy. Przy użyciu ról można zdefiniować te reguły autoryzacji na podstawie ról, a nie użytkownika.

## <a name="authenticating-users-in-an-aspnet-application"></a>Uwierzytelnianie użytkowników w aplikacji ASP.NET

Gdy użytkownik wprowadzi adres URL w oknie adresu przeglądarki lub kliknie łącze, przeglądarka wysyła żądanie [http (Hypertext Transfer Protocol)](http://en.wikipedia.org/wiki/HTTP) do serwera sieci Web dla określonej zawartości, może być stroną ASP.NET, obrazem, plikiem JavaScript lub innym typem zawartości. Serwer sieci Web jest z zadaniami zwracających żądaną zawartość. W tym celu należy określić liczbę rzeczy dotyczących żądania, w tym informacje o tym, kto zgłosił żądanie, oraz o tym, czy tożsamość jest autoryzowana do pobrania wymaganej zawartości.

Domyślnie przeglądarki wysyłają żądania HTTP, które nie zawierają żadnych informacji identyfikacyjnych. Jeśli jednak przeglądarka zawiera informacje o uwierzytelnianiu, serwer sieci Web uruchamia przepływ pracy uwierzytelniania, który próbuje zidentyfikować klienta wysyłającego żądanie. Kroki przepływu pracy uwierzytelniania zależą od typu uwierzytelniania używanego przez aplikację sieci Web. ASP.NET obsługuje trzy typy uwierzytelniania: Windows, Passport i Forms. Ta seria samouczków koncentruje się na uwierzytelnianiu formularzy, ale Poświęć kilka minut, aby porównać i kontrastować magazyny i przepływy użytkowników uwierzytelniania systemu Windows.

### <a name="authentication-via-windows-authentication"></a>Uwierzytelnianie za pośrednictwem uwierzytelniania systemu Windows

Przepływ pracy uwierzytelniania systemu Windows używa jednej z następujących technik uwierzytelniania:

- Uwierzytelnianie podstawowe
- Uwierzytelnianie szyfrowane
- Uwierzytelnianie zintegrowane systemu Windows

Wszystkie trzy techniki działają w sposób niedrogi: po nadejściu nieautoryzowanego żądania anonimowego serwer sieci Web wysyła odpowiedź HTTP, która wskazuje, że autoryzacja jest wymagana do kontynuowania. Następnie w przeglądarce zostanie wyświetlone modalne okno dialogowe, które wyświetla komunikat o nazwie użytkownika i hasła (patrz rysunek 1). Te informacje są następnie wysyłane z powrotem do serwera sieci Web za pośrednictwem nagłówka HTTP.

![Modalne okno dialogowe z prośbą o poświadczenia użytkownika](security-basics-and-asp-net-support-vb/_static/image1.png)

**Rysunek 1**. modalne okno dialogowe z prośbą o poświadczenia użytkownika

Podane poświadczenia są weryfikowane względem magazynu użytkowników systemu Windows na serwerze sieci Web. Oznacza to, że każdy uwierzytelniony użytkownik w aplikacji sieci Web musi mieć konto systemu Windows w organizacji. Jest to commonplace w scenariuszach z intranetem. W przypadku korzystania z uwierzytelniania zintegrowanego systemu Windows w ustawieniach sieci intranet przeglądarka automatycznie udostępnia serwerowi sieci Web poświadczenia używane do logowania się do sieci, przez co pomija okno dialogowe pokazane na rysunku 1. Chociaż uwierzytelnianie systemu Windows jest doskonałe dla aplikacji intranetowych, zazwyczaj nie jest możliwe w przypadku aplikacji internetowych, ponieważ nie ma potrzeby tworzenia kont systemu Windows dla każdego użytkownika, który zarejestruje się w lokacji.

### <a name="authentication-via-forms-authentication"></a>Uwierzytelnianie za pomocą uwierzytelniania formularzy

Z drugiej strony uwierzytelnianie formularzy jest idealnym rozwiązaniem w przypadku aplikacji internetowych sieci Web. Odwołaj to uwierzytelnianie formularzy identyfikuje użytkownika, monitując o wprowadzenie ich poświadczeń za pomocą formularza sieci Web. W związku z tym, gdy użytkownik próbuje uzyskać dostęp do nieautoryzowanego zasobu, zostaną automatycznie przekierowani do strony logowania, na której można wprowadzić swoje poświadczenia. Przesłane poświadczenia są następnie sprawdzane pod kątem niestandardowego magazynu użytkowników — zazwyczaj jest to baza danych.

Po sprawdzeniu przesłanych poświadczeń zostanie utworzony *bilet uwierzytelniania formularzy* dla użytkownika. Ten bilet wskazuje, że użytkownik został uwierzytelniony i zawiera informacje o tożsamości, takie jak nazwa użytkownika. Bilet uwierzytelniania formularzy jest (zazwyczaj) przechowywany jako plik cookie na komputerze klienckim. W związku z tym, kolejne wizyty w witrynie sieci Web zawierają bilet uwierzytelniania formularzy w żądaniu HTTP, dzięki czemu aplikacja internetowa może identyfikować użytkownika po zalogowaniu się.

Rysunek 2 ilustruje przepływ pracy uwierzytelniania formularzy z punktu Vantage wysokiego poziomu. Zwróć uwagę, jak elementy uwierzytelniania i autoryzacji w ASP.NET działają jako dwie osobne jednostki. System uwierzytelniania formularzy identyfikuje użytkownika (lub raporty, które są anonimowe). System autoryzacji decyduje o tym, czy użytkownik ma dostęp do żądanego zasobu. Jeśli użytkownik nie ma autoryzacji (jak przedstawiono na rysunku 2 podczas próby anonimowego odwiedzenia ProtectedPage. aspx), system autoryzacji zgłasza odmowę dostępu użytkownika, powodując automatyczne przekierowanie użytkownika do strony logowania przez system uwierzytelniania formularzy.

Po pomyślnym zalogowaniu się użytkownika kolejne żądania HTTP obejmują bilet uwierzytelniania formularzy. System uwierzytelniania formularzy jedynie identyfikuje użytkownika — jest to system autoryzacji, który określa, czy użytkownik może uzyskać dostęp do żądanego zasobu.

![Przepływ pracy uwierzytelniania formularzy](security-basics-and-asp-net-support-vb/_static/image2.png)

**Rysunek 2**. przepływ pracy uwierzytelniania formularzy

W następnych dwóch samouczkach zostanie Dig na uwierzytelnianie formularzy, które są bardziej szczegółowe,[Omówienie konfiguracji uwierzytelniania formularzy](an-overview-of-forms-authentication-vb.md) i [uwierzytelniania formularzy oraz zaawansowanych tematów](forms-authentication-configuration-and-advanced-topics-vb.md). Aby uzyskać więcej informacji na temat środowiska ASP. Opcje uwierzytelniania w sieci, zobacz [ASP.NET Authentication](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Ograniczanie dostępu do stron sieci Web, katalogów i funkcji strony

ASP.NET zawiera dwa sposoby określenia, czy określony użytkownik ma uprawnienia dostępu do określonego pliku lub katalogu:

- **Autoryzacja plików** — ponieważ strony ASP.NET i usługi sieci Web są zaimplementowane jako pliki, które znajdują się w systemie plików serwera sieci Web, dostęp do tych plików można określić za pomocą list Access Control (ACL). Autoryzacja plików jest najczęściej używana z uwierzytelnianiem systemu Windows, ponieważ listy ACL są uprawnieniami, które dotyczą kont systemu Windows. W przypadku korzystania z uwierzytelniania formularzy wszystkie żądania na poziomie systemu operacyjnego i systemu plików są wykonywane przez to samo konto systemu Windows, niezależnie od tego, czy użytkownik odwiedzający witrynę.
- **Autoryzacja adresu URL**— z AUTORYZACJĄ adresu URL, deweloper strony określa reguły autoryzacji w pliku Web. config. Te reguły autoryzacji określają, którzy użytkownicy lub które role mogą uzyskać dostęp, lub nie mogą uzyskać dostępu do określonych stron lub katalogów w aplikacji.

Autoryzacja plików i autoryzacja adresów URL definiują reguły autoryzacji umożliwiające dostęp do określonej strony ASP.NET lub wszystkich stron ASP.NET w określonym katalogu. Korzystając z tych technik, możemy wydać ASP.NET, aby odmówić żądań do określonej strony dla określonego użytkownika lub zezwolić na dostęp do zestawu użytkowników i uniemożliwić dostęp innym osobom. Jakie są scenariusze, w których wszyscy użytkownicy mogą uzyskać dostęp do strony, ale funkcjonalność strony zależy od użytkownika? Na przykład wiele witryn, które obsługują konta użytkowników, zawiera strony, które wyświetlają różne zawartość lub dane dla użytkowników uwierzytelnionych i użytkowników anonimowych. Użytkownik anonimowy może zobaczyć link do logowania się do witryny, podczas gdy uwierzytelniony użytkownik zobaczy komunikat, na przykład, Witamy w powrocie, *username* oraz link do wylogowania. Inny przykład: podczas wyświetlania elementu w witrynie aukcyjnej zobaczysz różne informacje w zależności od tego, czy jesteś licytantem, czy z licytacją tego elementu.

Takie dostosowania na poziomie strony można wykonać deklaratywnie lub programowo. Aby wyświetlić inną zawartość anonimową niż użytkownicy uwierzytelnieni, wystarczy przeciągnąć [kontrolkę widoku logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) na stronę i wprowadzić odpowiednią zawartość do swoich szablonów AnonymousTemplate i LoggedInTemplate. Alternatywnie można programowo określić, czy bieżące żądanie jest uwierzytelniane, kto jest użytkownikiem i jakie role należą do (jeśli istnieją). Te informacje służą do wyświetlania lub ukrywania kolumn w siatce lub panelach na stronie.

Ta seria zawiera trzy samouczki, które koncentrują się na autoryzacji. ***Autoryzacja oparta na użytkownikach***analizuje sposób ograniczania dostępu do strony lub stron w katalogu dla określonych kont użytkowników; ***Autoryzacja oparta na rolach*** sprawdza podawanie reguł autoryzacji na poziomie roli; na koniec w samouczku ***Wyświetlanie zawartości opartej na aktualnie zalogowanym użytkowniku*** eksploruje się modyfikowanie zawartości i funkcjonalności określonej strony na podstawie użytkownika odwiedzającego stronę. Aby uzyskać więcej informacji na temat środowiska ASP. Opcje autoryzacji sieci, zobacz [ASP.NET Authorization](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Konta i role użytkowników

ASP.NET. Uwierzytelnianie formularzy w sieci to infrastruktura umożliwiająca użytkownikom zalogowanie się w lokacji i zapamiętanie ich stanu uwierzytelnionego między wizytami stron. I autoryzacja adresów URL oferuje strukturę do ograniczania dostępu do określonych plików lub folderów w aplikacji ASP.NET. Żadna funkcja nie oferuje jednak metody przechowywania informacji o kontach użytkowników ani zarządzania rolami.

Przed ASP.NET 2,0 deweloperzy byli zobowiązani do tworzenia własnych magazynów użytkowników i ról. Znajdowały się one również w punkcie zaczepienia projektowania interfejsów użytkownika i pisania kodu dla najważniejszych stron związanych z kontem użytkownika, takich jak strona logowania i strona, aby utworzyć nowe konto, między innymi. Bez wbudowanej struktury konta użytkownika w programie ASP.NET każdy deweloper implementujący konta użytkowników musiał dotrzeć do swoich własnych decyzji projektowych na pytaniach, takich jak, Jak mogę przechowywać hasła lub inne poufne informacje? Jakie wytyczne należy nakładać na długość i siłę hasła?

Obecnie implementowanie kont użytkowników w aplikacji ASP.NET jest znacznie prostsze dzięki *platformie członkostwa* i wbudowanym kontrolkom sieci Web logowania. Platforma członkostwa jest kilku klas w [przestrzeni nazw System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) , która zapewnia funkcje do wykonywania podstawowych zadań związanych z kontem użytkownika. Klasa klucza w środowisku członkostwa jest [klasą członkostwa](https://msdn.microsoft.com/library/system.web.security.membership.aspx), która ma metody takie jak:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Platforma członkowska korzysta z [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), który czyści interfejs API struktury członkostwa od jego implementacji. Dzięki temu deweloperzy mogą używać wspólnego interfejsu API, ale mogą korzystać z implementacji, która spełnia niestandardowe potrzeby aplikacji. W skrócie Klasa Membership definiuje podstawowe funkcje platformy (metody, właściwości i zdarzenia), ale w rzeczywistości nie dostarcza żadnych szczegółów implementacji. Zamiast tego metody klasy członkostwa wywołują skonfigurowanego dostawcę, który wykonuje rzeczywistą prace. Na przykład, gdy wywoływana jest metoda klasy członkostwa, klasa członkowska nie zna szczegółów magazynu użytkownika. Nie wiadomo, czy użytkownicy są obsługiwani w bazie danych, czy w pliku XML, czy też w innym magazynie. Klasa Membership sprawdza konfigurację aplikacji sieci Web w celu określenia dostawcy, do którego ma zostać delegowane wywołanie, oraz że Klasa dostawcy jest odpowiedzialna za rzeczywiste utworzenie nowego konta użytkownika w odpowiednim magazynie użytkownika. Ta interakcja jest pokazany na rysunku 3.

Firma Microsoft dostarcza dwie klasy dostawcy członkostwa w .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) — implementuje interfejs API członkostwa w serwerach Active Directory i Active Directory Application Mode (ADAM).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) — implementuje interfejs API członkostwa w bazie danych SQL Server.

Ta seria samouczków koncentruje się wyłącznie na SqlMembershipProvider.

[![model dostawcy umożliwia nieprzerwane dołączenie różnych implementacji do struktury](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Rysunek 03**: model dostawcy umożliwia bezproblemowe podłączanie różnych implementacji do struktury ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](security-basics-and-asp-net-support-vb/_static/image5.png))

Korzystanie z modelu dostawcy polega na tym, że alternatywne implementacje mogą być opracowane przez firmę Microsoft, innych producentów lub indywidualnych deweloperów i bezproblemowo podłączone do struktury członkostwa. Na przykład firma Microsoft wyłączyła [dostawcę członkostwa baz danych programu Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Aby uzyskać więcej informacji o dostawcach członkostwa, zapoznaj się z [zestawem narzędzi dostawcy](https://msdn.microsoft.com/asp.net/aa336558.aspx), zawierającym wskazówki dotyczące dostawców członkostwa, przykładowe Dostawcy niestandardowi, ponad 100 stron dokumentacji dotyczącej modelu dostawcy oraz kompletny kod źródłowy dla wbudowanych dostawców członkostwa (tj. ActiveDirectoryMembershipProvider i SqlMembershipProvider).

W ASP.NET 2,0 wprowadzono również strukturę role. Podobnie jak w przypadku struktury członkostwa, struktura ról jest skompilowana korzystającego modelem dostawcy. Jego interfejs API jest udostępniany za pośrednictwem [klasy role](https://msdn.microsoft.com/library/system.web.security.roles.aspx) , a .NET Framework są dostarczane przez trzy klasy dostawców:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) — zarządza informacjami o rolach w magazynie zasad autoryzacji, takich jak Active Directory lub Adam.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) — implementuje role w bazie danych SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) — kojarzy informacje o rolach na podstawie grupy systemu Windows gościa. Ta metoda jest zwykle używana z uwierzytelnianiem systemu Windows.

Ta seria samouczków koncentruje się wyłącznie na SqlRoleProvider.

Ponieważ model dostawcy zawiera pojedynczy interfejs API do przesyłania dalej (klasy członkostwa i ról), możliwe jest tworzenie funkcji wokół tego interfejsu API bez konieczności zajmowania się szczegółami implementacji — są one obsługiwane przez dostawców wybranych przez tę stronę pisał. Ten ujednolicony interfejs API umożliwia dostawcom firmy Microsoft i innych firm Tworzenie formantów sieci Web, które są interfejsami z platformami członkostwa i ról. ASP.NET dostarcza wiele [kontrolek sieci Web logowania](https://msdn.microsoft.com/library/ms178329.aspx) do implementacji wspólnych interfejsów użytkownika kont użytkowników. Na przykład [kontrolka logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) monituje użytkownika o poświadczenia, zweryfikuje je, a następnie rejestruje je za pomocą uwierzytelniania formularzy. [Formant widoku logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) oferuje szablony do wyświetlania różnych znaczników dla użytkowników anonimowych i użytkowników uwierzytelnionych, a także inne znaczniki w zależności od roli użytkownika. I [formant formancie CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) udostępnia interfejs użytkownika krok po kroku do tworzenia nowego konta użytkownika.

Poniżej omówiono różne kontrolki logowania, które współpracują z platformami członkostwa i ról. Większość kontrolek logowania można zaimplementować bez konieczności pisania jednego wiersza kodu. Sprawdzimy te kontrolki bardziej szczegółowo w przyszłych samouczkach, w tym technik rozszerzania i dostosowywania ich funkcjonalności.

## <a name="summary"></a>Podsumowanie

Wszystkie aplikacje sieci Web, które obsługują konta użytkowników, wymagają podobnych funkcji, takich jak: możliwość zalogowania się użytkowników i zapamiętania ich stanu logowania między wizytami stron; Strona sieci Web dla nowych odwiedzających do utworzenia konta; i możliwość dewelopera strony, aby określić, jakie zasoby, dane i funkcje są dostępne dla użytkowników lub ról. Zadania uwierzytelniania i autoryzowania użytkowników oraz zarządzania kontami użytkowników i rolami są niezwykle łatwe w użyciu w aplikacjach ASP.NET, dzięki czemu uwierzytelnianie formularzy, Autoryzacja adresów URL oraz struktury członkostwa i ról.

W ciągu kilku następnych samouczków sprawdzimy te aspekty, tworząc działającą aplikację sieci Web od podstaw w sposób krok po kroku. W następnym dwa samouczku szczegółowo omówiono uwierzytelnianie formularzy. Zobaczysz przepływ pracy uwierzytelniania formularzy w akcji, DISSECT bilet uwierzytelniania formularzy, zapoznaj się z zagadnieniami dotyczącymi zabezpieczeń i zobacz, jak skonfigurować system uwierzytelniania formularzy — wszystkie podczas kompilowania aplikacji sieci Web, która umożliwia odwiedzającym logowanie się i wylogowywanie.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Członkostwo w ASP.NET 2,0, role, uwierzytelnianie formularzy i zasoby zabezpieczeń](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Wytyczne dotyczące zabezpieczeń ASP.NET 2,0](https://msdn.microsoft.com/library/ms998258.aspx)
- [Uwierzytelnianie ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autoryzacja ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [ASP.NET — informacje o kontrolkach](https://msdn.microsoft.com/library/ms178329.aspx)
- [Badanie członkostwa, ról i profilu w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Jak: Zabezpieczanie mojej witryny przy użyciu członkostwa i ról?](https://asp.net/learn/videos/video-45.aspx) Plików
- [Wprowadzenie do członkostwa](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Witryna MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2,0 — zabezpieczenia, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zestaw narzędzi dostawcy](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Osoba dokonująca przeglądu potencjalnych klientów dla tego samouczka została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka obejmują Alicja Maziarz, Jan suru i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](forms-authentication-configuration-and-advanced-topics-cs.md)
> [dalej](an-overview-of-forms-authentication-vb.md)
