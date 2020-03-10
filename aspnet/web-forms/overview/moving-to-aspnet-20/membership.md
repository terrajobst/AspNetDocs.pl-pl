---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Członkostwo | Microsoft Docs
author: microsoft
description: Członkostwo ASP.NET kompiluje się na powodzeniu modelu uwierzytelniania formularzy z ASP.NET 1. x. Uwierzytelnianie za pomocą formularzy ASP.NET zapewnia wygodny sposób incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: da6fc205bd852a818d65425586cec38fdb08d310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642157"
---
# <a name="membership"></a>Członkostwo

przez [firmę Microsoft](https://github.com/microsoft)

> Członkostwo ASP.NET kompiluje się na powodzeniu modelu uwierzytelniania formularzy z ASP.NET 1. x. Uwierzytelnianie za pomocą formularzy ASP.NET to wygodny sposób dołączania formularza logowania do aplikacji ASP.NET i weryfikowania użytkowników w bazie danych lub innym magazynie danych.

Członkostwo ASP.NET kompiluje się na powodzeniu modelu uwierzytelniania formularzy z ASP.NET 1. x. Uwierzytelnianie za pomocą formularzy ASP.NET to wygodny sposób dołączania formularza logowania do aplikacji ASP.NET i weryfikowania użytkowników w bazie danych lub innym magazynie danych. Członkowie klasy FormsAuthentication mogą obsługiwać pliki cookie na potrzeby uwierzytelniania, sprawdzać poprawność logowania, wylogować użytkownika itp. Jednak zaimplementowanie uwierzytelniania formularzy w aplikacji ASP.NET 1. x może wymagać godziwej ilości kodu.

Członkostwo w ASP.NET 2,0 jest głównym przenoszonem za pośrednictwem samego uwierzytelniania formularzy. (Członkostwo jest najbardziej niezawodne w połączeniu z uwierzytelnianiem formularzy, ale używanie uwierzytelniania formularzy nie jest wymagane). Jak zobaczysz, możesz użyć członkostwa ASP.NET i kontrolek logowania w ASP.NET 2,0 do wdrożenia zaawansowanego systemu członkostwa bez konieczności pisania wielu kodów.

## <a name="implementing-membership-in-aspnet-20"></a>Implementowanie członkostwa w ASP.NET 2,0

Członkostwo jest implementowane przez wykonanie czterech kroków. Należy pamiętać, że istnieje wiele podkroków, które są używane, a także opcjonalna konfiguracja, która może być również zaimplementowana. Te kroki są przeznaczone do zilustrowania dużego obrazu konfigurowania członkostwa.

1. Utwórz bazę danych członkostwa (Jeśli SQL Server jest używana jako magazyn członkostwa).
2. Określ opcje członkostwa w plikach konfiguracji aplikacji. (Członkostwo jest domyślnie włączone).
3. Określ typ magazynu członkostwa, którego chcesz użyć. Opcje to: 

    - Microsoft SQL Server (wersja 7,0 lub nowsza)
    - Magazyn Active Directory
    - Niestandardowy dostawca członkostwa
4. Skonfiguruj aplikację do uwierzytelniania formularzy ASP.NET. Po ponownym włączeniu członkostwo jest przeznaczone do skorzystania z uwierzytelniania formularzy, ale używanie uwierzytelniania formularzy nie jest wymagane.
5. Zdefiniuj konta użytkowników dla członkostwa i skonfiguruj role w razie potrzeby.

## <a name="creating-the-membership-database"></a>Tworzenie bazy danych członkostwa

W przypadku korzystania z SQL Server 7,0 lub nowszego jako magazynu członkostwa można skonfigurować bazę danych za pomocą narzędzia regsql języka ASPNET\_(dostępnego najłatwiej w wierszu polecenia programu Visual Studio .NET 2005). Narzędzia regsql ASPNET\_można użyć jako narzędzia wiersza polecenia lub Kreatora graficznego interfejsu użytkownika. Metoda kreatora jest najprostszym sposobem skonfigurowania bazy danych. Aby uzyskać dostęp do kreatora, wystarczy uruchomić następujące polecenie:

`aspnet_regsql W`

Po uruchomieniu tego polecenia zostanie wyświetlony Kreator instalacji SQL Server ASP.NET, jak pokazano poniżej.

![](membership/_static/image1.jpg)

**Rysunek 1**

Kreator instalacji SQL Server ASP.NET tworzy witrynę sieci Web w wystąpieniu określonym w kreatorze. Jednak ASP.NET będzie używać parametrów połączenia w pliku Machine. config do łączenia się z bazą danych. Domyślnie te parametry połączenia wskazują na wystąpienie SQL Server 2005, więc jeśli używasz wystąpienia SQL Server 2000 lub SQL Server 7,0, musisz zmodyfikować parametry połączenia w pliku Machine. config. Te parametry połączenia można znaleźć tutaj:

[!code-xml[Main](membership/samples/sample1.xml)]

Niestety, jeśli nie zmodyfikujesz parametrów połączenia, ASP.NET nie będzie zawierać opisowego błędu. W dalszym ciągu będzie można tylko powiedzieć, że baza danych nie została utworzona. W powyższym przypadku zmodyfikowano parametry połączenia tak, aby wskazywały na moje lokalne wystąpienie SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Określanie konfiguracji i Dodawanie użytkowników i ról

Następnym krokiem w konfigurowaniu członkostwa jest dodanie niezbędnych informacji do pliku Web. config aplikacji. W ASP.NET 1. x Modyfikowanie pliku Web. config było czasami trudne z powodu użycia lowerCamelCase i braku funkcji IntelliSense. Program Visual Studio .NET 2005 ułatwia zadanie przy użyciu funkcji IntelliSense dla plików konfiguracyjnych, ale ASP.NET 2,0 prowadzi jeszcze jeden krok, dostarczając interfejs sieci Web do edytowania plików konfiguracji.

Interfejs sieci Web można uruchomić, klikając przycisk Konfiguracja ASP.NET na pasku narzędzi Eksplorator rozwiązań, jak pokazano poniżej. Możesz również uruchomić interfejs sieci Web za pośrednictwem wyskakujących okienek, które są wyświetlane po wstawieniu kontrolek logowania.

![](membership/_static/image2.jpg)

**Rysunek 2**

Spowoduje to uruchomienie narzędzia administrowania witryną sieci Web ASP.NET poniżej. Administrowanie witryną sieci Web ASP.NET jest interfejsem zawierającym cztery karty, który ułatwia zarządzanie ustawieniami aplikacji. Dostępne są następujące karty:

- **Mowa**
- **Zabezpieczenia** Konfigurowanie użytkowników, ról i dostępu.
- **Aplikacja** Skonfiguruj ustawienia aplikacji.
- **Dostawca** Skonfiguruj i przetestuj dostawcę członkostwa aplikacji.

Narzędzie do administrowania witryną sieci Web pozwala łatwo tworzyć nowych użytkowników, tworzyć nowe role oraz zarządzać użytkownikami i rolami. Ta możliwość nie jest dostępna w interfejsie systemu Windows. Interfejs systemu Windows umożliwia łatwe definiowanie ustawień autoryzacji oraz dodawanie, usuwanie i zarządzanie dostawcami, możliwości, które nie są dostępne w narzędziu do administrowania witryną sieci Web.

Aby uruchomić interfejs systemu Windows, Otwórz przystawkę Internet Information Services, kliknij prawym przyciskiem myszy aplikację, a następnie wybierz polecenie Właściwości. Kliknij kartę ASP.NET, a następnie kliknij przycisk Edytuj konfigurację. (Aby przycisk Edytuj konfigurację został włączony, aplikacja musi działać w ramach ASP.NET 2,0). Możesz również skonfigurować wersję ASP.NET w oknie dialogowym ASP.NET. Zostanie wyświetlone okno dialogowe Ustawienia konfiguracji ASP.NET, jak pokazano poniżej.

![](membership/_static/image3.jpg)

**Rysunek 3**

Na karcie Ogólne są wyświetlane parametry połączeń i ustawienia aplikacji. Wszystkie ustawienia w kursywie są zdefiniowane w pliku konfiguracji nadrzędnej (plik Machine. config lub Web. config na wyższym poziomie), a ustawienia, które nie są kursywą, pochodzą z pliku konfiguracyjnego aplikacji. Jeśli ustawienie zostanie dodane, usunięte lub edytowane na poziomie aplikacji, ASP.NET doda, usunie lub zmodyfikuje ustawienie na poziomach aplikacji Web. config, zamiast usuwać ustawienia z pliku konfiguracji, z którego jest dziedziczona.

Poniżej pokazano kartę Uwierzytelnianie. W tym miejscu zostaną skonfigurowane ustawienia członkostwa. Ustawienia uwierzytelniania formularzy, dostawcy członkostwa i dostawcy ról można skonfigurować w tym miejscu.

![](membership/_static/image4.jpg)

**Rysunek 4**

## <a name="implementing-membership-in-your-application"></a>Implementowanie członkostwa w aplikacji

Najprostszym sposobem implementacji członkostwa w programie ASP.NET 2,0 w aplikacji jest użycie dostarczonych kontrolek logowania. Ta metoda pozwala zaimplementować podstawowe informacje o członkostwie w programie ASP.NET 2,0 bez konieczności pisania kodu.

W programie ASP.NET 2,0 są dostępne następujące kontrolki logowania:

## <a name="login-control"></a>Kontrola logowania

Kontrolka logowania udostępnia interfejs, który ktoś może zalogować się do systemu członkostwa. Udostępnia ona pole tekstowe username i Password oraz przycisk Zaloguj. Wiele innych popularnych funkcji, takich jak link do rejestracji dla osób, które nie zostały jeszcze wykonane, pole wyboru, które umożliwia użytkownikowi automatyczne zalogowanie się do kolejnych wizyt, link do przypomnienia o haśle itd. Wszystkie funkcje kontrolki logowania można dostosować za pomocą właściwości kontrolki.

W ASP.NET 1. x deweloperzy musieli napisać godziwą ilość kodu do przeprowadzenia wyszukiwania podczas korzystania z uwierzytelniania formularzy. Członkostwo w systemie ASP.NET 2,0 umożliwia Weryfikowanie użytkowników bez konieczności pisania kodu. ASP.NET będzie automatycznie wykonywać wyszukiwanie użytkownika. (Jeśli używasz kontrolki logowania bez członkostwa w ASP.NET, możesz użyć metody **OnAuthenticate** do zweryfikowania użytkownika).

## <a name="loginview-control"></a>Widoku logowania — formant

Formant widoku logowania jest formantem z szablonem, który domyślnie udostępnia dwa szablony; AnonymousTemplate i LoggedInTemplate. Wyświetlany szablon jest określany na podstawie tego, czy użytkownik jest zalogowany w systemie członkostwa. Ten formant jest zazwyczaj używany do wyświetlania kontrolki logowania, gdy użytkownik nie zalogował się jeszcze i stanu logowania kontrolki i/lub innych kontrolek logowania, gdy użytkownik zalogował się. Jeśli używasz zarządzania rolami w aplikacji ASP.NET, kontrolka widoku logowania może wyświetlać określony szablon na podstawie roli użytkownika. (Więcej informacji na temat zarządzania rolami ASP.NET zostanie omówione później).

## <a name="passwordrecovery-control"></a>PasswordRecovery — formant

Kontrolka PasswordRecovery umożliwia użytkownikom otrzymywanie wiadomości e-mail przy użyciu bieżącego hasła lub Resetowanie hasła. Wyczyść tekst i zaszyfrowane hasła mogą być odzyskiwane i wysyłane pocztą e-mail do użytkowników. Jeśli hasło jest skrótem, nie można go odzyskać. Zamiast tego użytkownik będzie musiał wykonać Resetowanie hasła.

## <a name="loginstatus-control"></a>Stanu logowania — formant

Formant stanu logowania służy do wyświetlania wskaźnika logowania dla użytkowników, którzy nie są zalogowani, oraz wskaźnika wylogowania do użytkowników, którzy są obecnie zalogowani. Właściwość Request. isauthenticates służy do określania, który wskaźnik ma być wyświetlany. Wskaźnik wyświetlany przez kontrolkę stanu logowania może być tekstem (implementowanym za pomocą właściwości **LoginText** i **LogoutText** ) lub obrazów (implementowanych za pośrednictwem właściwości **LoginImageUrl** i **LogoutImageUrl** ).

Gdy użytkownik loguje się za pośrednictwem formantu stanu logowania, zostaje przekierowany do adresu URL określonego przez właściwość **LogoutPageUrl** . Jeśli ta właściwość nie jest ustawiona, bieżąca strona zostanie odświeżona. Ponieważ lokacja jest prawdopodobnie chroniona przez uwierzytelnianie formularzy, odświeżenie bieżącej strony przekieruje użytkownika na stronę logowania do witryny.

## <a name="loginname-control"></a>LoginName — formant

Kontrolka LoginName wyświetla nazwę użytkownika, który jest aktualnie zalogowany do witryny.

## <a name="createuserwizard-control"></a>Formancie CreateUserWizard — formant

Kontrolka formancie CreateUserWizard zapewnia użytkownikom wygodny sposób rejestracji w systemie członkostwa. Możesz dodać kroki (zaimplementowane jako kolekcja elementów WizardStep) za pośrednictwem interfejsu pokazanego poniżej.

![](membership/_static/image5.jpg)

**Rysunek 5**

Formancie CreateUserWizard to formant z szablonem, który pochodzi od klasy kreatora i zawiera następujące szablony:

- **HeaderTemplate** Ten szablon kontroluje wygląd nagłówka kreatora.
- **Element SideBarTemplate** Ten szablon steruje wyglądem paska bocznego kreatora.
- **StartNavigationTemplate** Ten szablon steruje wyglądem okienka nawigacji w kroku Rozpocznij.
- **StepNavigationTemplate** Ten szablon steruje wyglądem obszaru nawigacji, gdy nie znajduje się w kroku początkowym lub końcowym.
- **FinishNavigationTemplate** Ten szablon steruje wyglądem obszaru nawigacji w kroku zakończenia.

Ponadto dla każdego kroku, który można dodać do kreatora, ASP.NET utworzy szablon niestandardowy zawierający zarówno ContentTemplate, jak i CustomNavigationTemplate dla tego kroku. Aby uzyskać szczegółowe informacje na temat dostosowywania formancie CreateUserWizard, zobacz dokumentację dotyczącą VS.NET 2005:

## <a name="changepassword-control"></a>Kontrolka ChangePassword

Kontrolka ChangePassword umożliwia użytkownikom zmianę hasła. Jeśli właściwość DisplayUserName ma wartość true (domyślnie ma wartość false), użytkownik może zmienić swoje hasło, gdy nie są zalogowani. Jeśli użytkownik *jest* już zalogowany i Właściwość DisplayUserName ma wartość true, użytkownik będzie mógł zmienić hasło innego użytkownika, który nie jest zalogowany, podając identyfikator użytkownika tego użytkownika.

Należy pamiętać, że jeśli chcesz, aby użytkownicy mogli zmieniać hasła bez konieczności logowania, musisz upewnić się, że strona, na której jest wyświetlany formant ChangePassword, zezwala na dostęp anonimowy. Aby zmienić hasło, użytkownicy będą musieli podać stare hasło.

## <a name="role-management"></a>Zarządzanie rolami

Zarządzanie rolami pozwala przypisywać użytkowników do określonej roli, a następnie ograniczyć dostęp do określonych plików lub folderów na podstawie tej roli. Zarządzanie rolami udostępnia również interfejs API, dzięki czemu można programowo określić rolę kogoś lub określić wszystkich użytkowników w określonej roli i odpowiednio odpowiedzieć.

Zarządzanie rolami nie jest wymagane w członkostwie ASP.NET ani nie jest wymagane, aby można było korzystać z zarządzania rolami. Jednak te dwa uzupełniają wszystkie inne dobrze i prawdopodobnie deweloperzy będą ich używać wspólnie ze sobą.

Aby włączyć zarządzanie rolami w aplikacji, wprowadź następujące zmiany w pliku Web. config:

[!code-xml[Main](membership/samples/sample2.xml)]

Gdy atrybut **CacheRolesInCookie** ma wartość true, ASP.NET buforuje członkostwo roli użytkowników w pliku cookie na kliencie. Dzięki temu można wyszukiwać role bez wywoływania do RoleProvider. W przypadku korzystania z tego atrybutu zachęca się deweloperów do zapewnienia, że atrybut **CookieProtection** jest ustawiony na wartość ALL. (Jest to ustawienie domyślne). Dzięki temu dane plików cookie są szyfrowane i pomagają upewnić się, że zawartość plików cookie nie została zmieniona. Role można dodawać przy użyciu narzędzia do administrowania witryną sieci Web. Pozwala to łatwo definiować role, konfigurować dostęp do części witryny na podstawie tych ról i przypisywać użytkowników do ról.

![](membership/_static/image6.jpg)

**Rysunek 6.**

Jak pokazano powyżej, nowe role można dodać, wprowadzając po prostu nazwę roli, a następnie klikając pozycję Dodaj rolę. Istniejące role można zarządzać lub usuwać, klikając odpowiednie łącze na liście istniejących ról.

Podczas zarządzania rolą można dodawać i usuwać użytkowników, jak pokazano poniżej.

![](membership/_static/image7.jpg)

**Rysunek 7**

Zaznaczając pole wyboru użytkownik jest w roli, można łatwo dodać użytkownika do określonej roli. Program ASP.NET automatycznie zaktualizuje swoją bazę danych członkostwa przy użyciu odpowiednich wpisów. Należy również skonfigurować reguły dostępu dla aplikacji. ASP.NET 1. x deweloperzy są zaznajomieni z tą metodą za pośrednictwem elementu&gt; autoryzacji &lt;w pliku Web. config, a ta opcja jest nadal dostępna w ASP.NET 2,0. Można jednak ułatwić zarządzanie regułami dostępu za pomocą narzędzia administrowania witryną sieci Web, jak pokazano poniżej.

![](membership/_static/image8.jpg)

**Rysunek 8.**

W takim przypadku folder administracyjny zostanie wyróżniony (trudno jest zobaczyć, ponieważ narzędzie podświetli je w kolorze szarym), a rola Administratorzy uzyska dostęp. Wszyscy inni użytkownicy są odrzucani. Możesz kliknąć ikonę nagłówka, aby wybrać regułę, a następnie użyć przycisków Przenieś w górę i Przenieś w dół, aby rozmieścić reguły. Podobnie jak w przypadku elementu ASP.NET &lt;Authorization&gt;, reguły są przetwarzane w kolejności, w jakiej są wyświetlane. Innymi słowy, jeśli porządek reguł w powyższym powrocie został cofnięty, nikt nie będzie miał dostępu do folderu administracyjnego, ponieważ pierwsza reguła, którą ASP.NET napotka, będzie regułą, która odmówi wszystkim do tego folderu.

ASP.NET 2,0 dodaje plik Web. config do folderu, dla którego określisz regułę dostępu. Reguły dostępu można edytować za pomocą pliku konfiguracji lub narzędzia do administrowania witryną sieci Web. Innymi słowy narzędzie do administrowania witryną sieci Web jest po prostu interfejsem, za pomocą którego plik konfiguracji można edytować w środowisku przyjaznym dla użytkownika.

## <a name="using-roles-in-code"></a>Korzystanie z ról w kodzie

Interfejs API zarządzania rolami nie został zmieniony od wersji 1. x. Metoda **IsInRole** służy do określenia, czy użytkownik należy do określonej roli.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET tworzy również wystąpienie RolePrincipal jako element członkowski bieżącego kontekstu. Obiekt RolePrincipal może służyć do uzyskania wszystkich ról, do których należy użytkownik:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Używanie RoleGroups z kontrolką widoku logowania

Teraz, gdy masz już świadomość zarządzania rolami i członkostwem, możesz omówić krótko, w jaki sposób formant widoku logowania wykorzystuje tę możliwość w ASP.NET 2,0. Jak wspomniano wcześniej, formant widoku logowania jest formantem z szablonem, który domyślnie zawiera dwa szablony; AnonymousTemplate i LoggedInTemplate. W oknie dialogowym zadania widoku logowania jest link (przedstawiony poniżej), który umożliwia edytowanie RoleGroups.

![](membership/_static/image9.jpg)

**Rysunek 9.**

Każdy obiekt grupy ról zawiera tablicę ciągów, które definiują role, do których odnosi się ta rola. Aby dodać nową rolę do kontrolki widoku logowania, kliknij łącze Edytuj RoleGroups. Na powyższym obrazie można zobaczyć, że dodano nową rolę grupy dla administratorów. Wybierając tę rolę (rolę [0]) z listy rozwijanej widoki, można skonfigurować szablon, który będzie wyświetlany tylko dla członków roli Administratorzy. Na poniższej ilustracji dodano nową rolę grupy, która jest stosowana do członków roli sprzedaż i rola dystrybucji. Spowoduje to dodanie drugiego elementu role do listy rozwijanej widoki w oknie dialogowym zadania widoku logowania, a wszystkie elementy dodane do tego szablonu będą widoczne dla każdego użytkownika w roli sprzedaż lub dystrybucja.

![](membership/_static/image10.jpg)

**Rysunek 10**

## <a name="overriding-the-existing-membership-provider"></a>Zastępowanie istniejącego dostawcy członkostwa

Istnieje kilka sposobów, aby zwiększyć funkcjonalność członkostwa ASP.NET. Najpierw można oczywiście zmienić istniejące funkcje klasy SqlMembershipProvider przez dziedziczenie z niej i zastępowanie jej metod. Na przykład jeśli chcesz zaimplementować własne funkcje podczas tworzenia użytkowników, możesz utworzyć własną klasę, która dziedziczy z SqlMembershipProvider w następujący sposób:

[!code-csharp[Main](membership/samples/sample5.cs)]

Jeśli z drugiej strony chcesz utworzyć własnego dostawcę (aby przechowywać informacje o członkostwie w bazie danych programu Access, na przykład), możesz utworzyć własnego dostawcę.

## <a name="creating-your-own-membership-provider"></a>Tworzenie własnego dostawcy członkostwa

Aby utworzyć własnego dostawcę członkostwa, należy najpierw utworzyć klasę, która dziedziczy z klasy MembershipProvider. Jeśli używasz programu VB.NET, program Visual Studio 2005 doda procedury pośredniczące dla wszystkich metod, które należy przesłonić. Jeśli używasz C#programu, możesz dodać te odcinki.

Należy przesłonić następujące elementy:

- Właściwość ApplicationName
- Funkcja ChangePassword
- ChangePasswordQuestionAndAnswer, funkcja
- Funkcja myFunction
- DeleteUser, funkcja
- Właściwość EnablePasswordReset
- Właściwość EnablePasswordRetrieval
- FindUsersByEmail, funkcja
- FindUsersByName, funkcja
- GetAllUsers, funkcja
- GetNumberOfUsersOnline, funkcja
- GetPassword — funkcja
- GetUser, funkcja
- GetUserNameByEmail, funkcja
- Właściwość MaxInvalidPasswordAttempts
- Właściwość MinRequiredNonAlphanumericCharacters
- Właściwość MinRequiredPasswordLength
- Właściwość PasswordAttemptWindow
- Właściwość PasswordFormat
- Właściwość PasswordStrengthRegularExpression
- Właściwość RequiresQuestionAndAnswer
- Właściwość RequiresUniqueEmail
- ResetPassword, funkcja
- Odblokuj funkcję użytkownika
- UpdateUser, funkcja
- ValidateUser, funkcja

Która całkiem listę do wdrożenia jako C# Deweloper. Łatwiej jest utworzyć klasę w VB.NET bez żadnej implementacji, a następnie użyć reflektora platformy .NET lub podobnego narzędzia do konwersji kodu na C#.

Parametry połączenia i inne właściwości powinny być ustawione na ich wartości domyślne w metodzie Initialize. (Metoda Initialize jest wyzwalana, gdy dostawca jest ładowany w czasie wykonywania). Drugi parametr metody Initialize jest typu System. Collections. wyspecjalizowany. NameValueCollection i jest odwołaniem do &lt;Dodaj&gt; elementu, który jest skojarzony z dostawcą niestandardowym w pliku Web. config. Ten wpis wygląda następująco:

[!code-xml[Main](membership/samples/sample6.xml)]

Oto przykład metody Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Aby zweryfikować użytkownika podczas przesyłania formularza logowania, należy użyć metody ValidateUser. Ta metoda jest wyzwalana, gdy użytkownik kliknie przycisk Zaloguj w kontrolce logowania. Zostanie umieszczony kod, który wykonuje wyszukiwanie użytkownika w tej metodzie.

Jak widać, pisanie własnego dostawcy członkostwa nie jest trudne i pozwala na rozbudowanie tej zaawansowanej funkcjonalności ASP.NET 2,0.
