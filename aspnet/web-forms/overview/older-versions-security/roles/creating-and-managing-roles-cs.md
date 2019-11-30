---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Tworzenie ról i zarządzanie nimiC#() | Microsoft Docs
author: rick-anderson
description: W tym samouczku przedstawiono kroki niezbędne do skonfigurowania struktury role. Poniżej przedstawiono kompilację stron sieci Web do tworzenia i usuwania ról.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: a7883d0b05f2fa5a3fdac887f8c8b39d70418fb3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595804"
---
# <a name="creating-and-managing-roles-c"></a>Tworzenie ról i zarządzanie nimi (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> W tym samouczku przedstawiono kroki niezbędne do skonfigurowania struktury role. Poniżej przedstawiono kompilację stron sieci Web do tworzenia i usuwania ról.

## <a name="introduction"></a>Wprowadzenie

<a id="_msoanchor_1"> </a>W samouczku dotyczącym [*autoryzacji opartej na użytkownikach*](../membership/user-based-authorization-cs.md) sprawdzono, jak korzystać z autoryzacji adresów URL w celu ograniczenia funkcjonalności niektórych użytkowników z zestawu stron oraz eksplorowania metod deklaratywnych i programistycznych umożliwiających Dostosowywanie funkcji strony ASP.NET na podstawie odwiedzanego użytkownika. Przyznanie uprawnień dostępu do stron lub funkcji na podstawie użytkownika, jednak może być okropnej konserwacji w scenariuszach, w których istnieje wiele kont użytkowników lub często zmieniają się uprawnienia użytkowników. Za każdym razem, gdy użytkownik uzyska lub utraci autoryzację do wykonania określonego zadania, administrator musi zaktualizować odpowiednie reguły autoryzacji adresów URL, znaczniki deklaratywne i kod.

Zwykle ułatwia klasyfikowanie użytkowników do grup lub *ról* , a następnie stosowanie uprawnień do poszczególnych ról. Na przykład większość aplikacji sieci Web ma określony zestaw stron lub zadań, które są zarezerwowane tylko dla użytkowników administracyjnych. Korzystając z metod opisanych w samouczku dotyczącym *autoryzacji na podstawie użytkownika* , dodamy odpowiednie reguły autoryzacji adresów URL, znaczniki deklaratywne i kod, aby umożliwić określonym kontom użytkowników wykonywanie zadań administracyjnych. Ale jeśli został dodany nowy administrator lub jeśli istniejący administrator musi mieć odwołane prawa administracyjne, będziemy musieli zwrócić i zaktualizować pliki konfiguracji oraz strony sieci Web. W przypadku ról można jednak utworzyć rolę o nazwie Administratorzy i przypisać tych zaufanych użytkowników do roli Administratorzy. Następnie dodamy odpowiednie reguły autoryzacji adresów URL, znaczniki deklaratywne i kod, aby umożliwić roli administratorów wykonywanie różnych zadań administracyjnych. W przypadku tej infrastruktury Dodawanie nowych administratorów do lokacji lub usuwanie istniejących jest bardzo proste, w tym m.in. lub usuwających użytkownika z roli Administratorzy. Nie są wymagane żadne zmiany w konfiguracji, znaczników deklaratywnych ani w kodzie.

ASP.NET oferuje strukturę ról do definiowania ról i kojarzenia ich z kontami użytkowników. Za pomocą platformy role możemy tworzyć i usuwać role, dodawać użytkowników do ról i usuwać ich z niej, określać zestaw użytkowników należących do określonej roli i dowiedzieć się, czy użytkownik należy do określonej roli. Po skonfigurowaniu struktury ról można ograniczyć dostęp do stron w oparciu o rolę według ról przy użyciu reguł autoryzacji adresów URL oraz pokazać lub ukryć dodatkowe informacje lub funkcje na stronie na podstawie aktualnie zalogowanych ról użytkownika.

W tym samouczku przedstawiono kroki niezbędne do skonfigurowania struktury role. Poniżej przedstawiono kompilację stron sieci Web do tworzenia i usuwania ról. <a id="_msoanchor_2"> </a>W samouczku [*Przypisywanie ról do użytkowników*](assigning-roles-to-users-cs.md) dowiesz się, jak dodawać i usuwać użytkowników z ról. W <a id="_msoanchor_3"> </a>samouczku dotyczącym [*autoryzacji opartej na rolach*](role-based-authorization-cs.md) zobaczymy, jak ograniczyć dostęp do stron w poszczególnych rolach, a także jak dostosować funkcje strony w zależności od roli użytkownika. Zacznijmy!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1. Dodawanie nowych stron ASP.NET

W tym samouczku i następnych dwóch będziemy przeanalizować różne funkcje i możliwości związane z rolami. Potrzebujemy serii stron ASP.NET, aby zaimplementować tematy zbadane w ramach tych samouczków. Utwórzmy te strony i zaktualizujesz mapę witryny.

Zacznij od utworzenia nowego folderu w projekcie o nazwie `Roles`. Następnie Dodaj cztery nowe strony ASP.NET do folderu `Roles`, łącząc każdą stronę ze stroną wzorcową `Site.master`. Nazwij strony:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

W tym momencie Eksplorator rozwiązań projektu powinien wyglądać podobnie do zrzutu ekranu pokazanego na rysunku 1.

[do folderu role dodano ![cztery nowe strony](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Rysunek 1**. dodano cztery nowe strony do folderu `Roles` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-and-managing-roles-cs/_static/image3.png))

Każda Strona powinna, w tym momencie, mieć dwie kontrolki zawartości, jedną dla każdej strony głównej: `MainContent` i `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Odwołaj się do `LoginContent` domyślnego znaczników elementu ContentPlaceHolder wyświetla link do logowania lub wylogowywania lokacji w zależności od tego, czy użytkownik jest uwierzytelniony. Obecność `Content2` kontrolki zawartości na stronie ASP.NET zastępuje domyślne znaczniki strony wzorcowej. Zgodnie z opisem w <a id="_msoanchor_4"> </a>temacie Omówienie samouczka [*związanego z uwierzytelnianiem formularzy*](../introduction/an-overview-of-forms-authentication-cs.md) zastępowanie znaczników domyślnych jest przydatne na stronach, w których nie chcesz wyświetlać opcji związanych z logowaniem w lewej kolumnie.

Jednak w przypadku tych czterech stron chcemy wyświetlić domyślne znaczniki strony głównej dla `LoginContent` ContentPlaceHolder. W związku z tym usuń znaczniki deklaratywne dla kontrolki zawartości `Content2`. Po wykonaniu tej czynności każdy znak z czterech stron powinien zawierać tylko jedną kontrolkę zawartości.

Na koniec zaktualizujmy mapę witryny (`Web.sitemap`) tak, aby obejmowała te nowe strony sieci Web. Dodaj następujący kod XML po `<siteMapNode>` dodanej do samouczków dotyczących członkostwa.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Po zaktualizowaniu mapy witryny odwiedź witrynę za pomocą przeglądarki. Jak pokazano na rysunku 2, Nawigacja po lewej stronie zawiera teraz elementy samouczków ról.

[do folderu role dodano ![cztery nowe strony](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Rysunek 2**. do folderu `Roles` dodano cztery nowe strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-and-managing-roles-cs/_static/image6.png))

## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Krok 2. Określanie i Konfigurowanie dostawcy struktury ról

Podobnie jak w przypadku struktury członkostwa, struktura ról jest skompilowana korzystającego modelem dostawcy. Zgodnie z opisem w <a id="_msoanchor_5"> </a>samouczku [*podstawy zabezpieczeń i pomocy technicznej ASP.NET*](../introduction/security-basics-and-asp-net-support-cs.md) .NET Framework są dostarczane z trzema wbudowanymi dostawcami ról: [`AuthorizationStoreRoleProvider`](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx), [`WindowsTokenRoleProvider`](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)i [`SqlRoleProvider`](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Ta seria samouczków koncentruje się na `SqlRoleProvider`, która używa Microsoft SQL Server bazy danych jako magazynu roli.

Poniżej omówiono strukturę ról i `SqlRoleProvider` działają podobnie jak struktura członkostwa i `SqlMembershipProvider`. .NET Framework zawiera klasę `Roles`, która służy jako interfejs API dla struktury role. Klasa `Roles` ma metody statyczne, takie jak `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`i tak dalej. Po wywołaniu jednej z tych metod Klasa `Roles` deleguje wywołanie do skonfigurowanego dostawcy. `SqlRoleProvider` współpracuje z tabelami specyficznymi dla roli (`aspnet_Roles` i `aspnet_UsersInRoles`) w odpowiedzi.

Aby móc korzystać z dostawcy `SqlRoleProvider` w aplikacji, należy określić bazę danych, która ma być używana jako magazyn. `SqlRoleProvider` oczekuje, że określony magazyn ról ma niektóre tabele, widoki i procedury składowane bazy danych. Te wymagane obiekty bazy danych można dodać za pomocą [narzędzia`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). W tym momencie mamy już bazę danych ze schematem niezbędnym dla `SqlRoleProvider`. Po <a id="_msoanchor_6"> </a>powrocie do [*tworzenia schematu członkostwa w SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) samouczku została utworzona baza danych o nazwie `SecurityTutorials.mdf` i użyta `aspnet_regsql.exe` do dodania usług aplikacji, które obejmowały obiekty bazy danych wymagane przez `SqlRoleProvider`. W związku z tym wystarczy poinformować strukturę role, aby włączyć obsługę ról i użyć `SqlRoleProvider` z bazą danych `SecurityTutorials.mdf` jako magazynem roli.

Struktura ról jest konfigurowana za pomocą &lt;`roleManager`&gt; elementu w pliku `Web.config` aplikacji. Domyślnie obsługa ról jest wyłączona. Aby włączyć tę funkcję, należy ustawić atrybut `enabled` elementu [&gt;&lt;`roleManager`](https://msdn.microsoft.com/library/ms164660.aspx) , aby `true`:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Domyślnie wszystkie aplikacje sieci Web mają dostawcę ról o nazwie `AspNetSqlRoleProvider` typu `SqlRoleProvider`. Ten domyślny dostawca jest zarejestrowany w `machine.config` (znajdujący się w `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Atrybut `connectionStringName` dostawcy określa używany magazyn ról. Dostawca `AspNetSqlRoleProvider` ustawia ten atrybut na `LocalSqlServer`, który jest również zdefiniowany w `machine.config` i wskazuje domyślnie do bazy danych SQL Server 2005 Express Edition w folderze `App_Data` o nazwie `aspnet.mdf`.

W związku z tym, jeśli po prostu włączysz strukturę role bez określania informacji o dostawcy w pliku `Web.config` aplikacji, aplikacja używa domyślnego dostawcy zarejestrowanych ról, `AspNetSqlRoleProvider`. Jeśli baza danych `~/App_Data/aspnet.mdf` nie istnieje, środowisko uruchomieniowe programu ASP.NET automatycznie utworzy ją i doda schemat usług aplikacji. Nie chcemy jednak korzystać z bazy danych `aspnet.mdf`. Zamiast tego chcemy użyć bazy danych `SecurityTutorials.mdf`, która została już utworzona i dodaliśmy do niej schemat usług aplikacji. Tę modyfikację można wykonać na jeden z dwóch sposobów:

- <strong>Określ wartość</strong> <strong>`LocalSqlServer`</strong> <strong>nazwy parametrów połączenia w</strong> <strong>`Web.config`</strong> <strong>.</strong> Zastępując `LocalSqlServer` wartość nazwy parametrów połączenia w `Web.config`, możemy użyć domyślnego dostawcy zarejestrowanej ról (`AspNetSqlRoleProvider`) i prawidłowo współpracować z `SecurityTutorials.mdf` bazą danych. Aby uzyskać więcej informacji na temat tej techniki, zobacz wpis w blogu [Scott Guthrie](https://weblogs.asp.net/scottgu/), [konfigurowanie ASP.NET 2,0 Usługi aplikacji do użycia SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Dodaj nowego zarejestrowanego dostawcę typu</strong> <strong>`SqlRoleProvider`</strong> <strong>i skonfiguruj</strong> ustawienia<strong>`connectionStringName`</strong>, <strong>aby wskazywały</strong> <strong>bazę danych</strong> <strong>`SecurityTutorials.mdf`</strong>. Jest to metoda zalecana i użyta w przypadku <a id="_msoanchor_7"> </a> [*tworzenia schematu członkostwa w SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) samouczku i jest to podejście, które zostanie użyte również w tym samouczku.

Dodaj do pliku `Web.config` następujące znaczniki konfiguracji ról. Ten znacznik rejestruje nowego dostawcę o nazwie `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Powyższy znacznik definiuje `SecurityTutorialsSqlRoleProvider` jako domyślnego dostawcę (za pośrednictwem atrybutu `defaultProvider` w elemencie `<roleManager>`). Ustawia również ustawienie `applicationName` `SecurityTutorialsSqlRoleProvider`na `SecurityTutorials`, która jest tym samym ustawieniem `applicationName` używanym przez dostawcę członkostwa (`SecurityTutorialsSqlMembershipProvider`). Chociaż nie pokazano tego tutaj, [element`<add>`](https://msdn.microsoft.com/library/ms164662.aspx) dla `SqlRoleProvider` może również zawierać atrybut `commandTimeout`, aby określić czas trwania limitu czasu bazy danych (w sekundach). Wartość domyślna to 30.

W przypadku tego oznakowania konfiguracji wszystko jest gotowe do rozpoczęcia korzystania z funkcji roli w naszej aplikacji.

> [!NOTE]
> Powyższe oznakowanie konfiguracji ilustruje użycie &lt;`roleManager`&gt; elementu `enabled` i `defaultProvider`. Istnieje wiele innych atrybutów, które mają wpływ na sposób kojarzenia informacji o rolach przez strukturę role w zależności od użytkownika. Będziemy przeanalizować te ustawienia w <a id="_msoanchor_8"> </a>samouczku dotyczącym [*autoryzacji opartej na rolach*](role-based-authorization-cs.md) .

## <a name="step-3-examining-the-roles-api"></a>Krok 3. Badanie interfejsu API ról

Funkcje struktury ról są udostępniane za pośrednictwem [klasy`Roles`](https://msdn.microsoft.com/library/system.web.security.roles.aspx), która zawiera trzynaście metod statycznych do wykonywania operacji opartych na rolach. Podczas tworzenia i usuwania ról w krokach 4 i 6 będziemy używać metod [`CreateRole`](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) i [`DeleteRole`](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) , które dodają lub usuwają rolę z systemu.

Aby uzyskać listę wszystkich ról w systemie, użyj [metody`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (zobacz krok 5). [Metoda`RoleExists`](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) zwraca wartość logiczną wskazującą, czy istnieje określona rola.

W następnym samouczku sprawdzimy, jak skojarzyć użytkowników z rolami. Metody [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [`AddUserToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [`AddUsersToRole`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)i [`AddUsersToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) klasy `Roles` umożliwiają dodanie jednego lub większej liczby użytkowników do co najmniej jednej roli. Aby usunąć użytkowników z ról, użyj metod [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [`RemoveUserFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [`RemoveUsersFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)lub [`RemoveUsersFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) .

<a id="_msoanchor_9"> </a>W samouczku dotyczącym [*autoryzacji opartej na rolach*](role-based-authorization-cs.md) zawarto instrukcje programistyczne pokazywania lub ukrywania funkcji na podstawie aktualnie zalogowanego użytkownika. W tym celu możemy użyć metod [`FindUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [`GetRolesForUser`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [`GetUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)i [`IsUserInRole`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) klasy `Role`.

> [!NOTE]
> Należy pamiętać, że po wywołaniu jednej z tych metod Klasa `Roles` deleguje wywołanie do skonfigurowanego dostawcy. W naszym przypadku oznacza to, że wywołanie jest wysyłane do `SqlRoleProvider`. `SqlRoleProvider` następnie wykonuje odpowiednią operację bazy danych na podstawie wywołanej metody. Na przykład kod `Roles.CreateRole("Administrators")` powoduje `SqlRoleProvider` wykonywania `aspnet_Roles_CreateRole` procedury składowanej, która wstawia nowy rekord do tabeli `aspnet_Roles` o nazwie Administratorzy.

W pozostałej części tego samouczka pokazano, jak `Roles` klasy `CreateRole`, `GetAllRoles`i `DeleteRole` do zarządzania rolami w systemie.

## <a name="step-4-creating-new-roles"></a>Krok 4. Tworzenie nowych ról

Role oferują sposób arbitralnej grupy użytkowników, a najczęściej takie grupowanie jest używane do bardziej wygodnego sposobu stosowania reguł autoryzacji. Jednak aby używać ról jako mechanizmu autoryzacji, najpierw musimy określić, jakie role istnieją w aplikacji. Niestety, ASP.NET nie zawiera kontrolki CreateRoleWizard. Aby dodać nowe role, należy utworzyć odpowiedni interfejs użytkownika i wywołać interfejs API ról wypróbujemy. Dobrą nowością jest to, że jest to bardzo proste.

> [!NOTE]
> Chociaż nie istnieje CreateRoleWizarda kontrolka sieci Web, istnieje [Narzędzie do administrowania witryną sieci web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), które jest lokalną aplikacją ASP.NET zaprojektowaną w celu ułatwienia wyświetlania konfiguracji aplikacji sieci Web i zarządzania nią. Nie jest to jednak duże wentylator narzędzia do administrowania witryną sieci Web ASP.NET z dwóch przyczyn. Po pierwsze, jest to bit debugowany, a środowisko użytkownika opuszcza dużo potrzebnych. Drugie narzędzie do administrowania witryną sieci Web ASP.NET jest przeznaczone tylko lokalnie, co oznacza, że konieczne będzie skompilowanie własnych stron sieci Web zarządzania rolami, jeśli trzeba zdalnie zarządzać rolami w działającej witrynie. Z tych dwóch powodów ten samouczek i następne zależą od tworzenia niezbędnych narzędzi do zarządzania rolami na stronie sieci Web, a nie polegania na narzędziu do administrowania witryną sieci Web ASP.NET.

Otwórz stronę `ManageRoles.aspx` w folderze `Roles` i Dodaj do niej kontrolkę pole tekstowe i przycisk sieci Web. Ustaw właściwość `ID` kontrolki TextBox na `RoleName`, `ID` i właściwości `Text` przycisku, aby `CreateRoleButton` i utworzyć rolę odpowiednio. W tym momencie znaczniki deklaratywne strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Następnie kliknij dwukrotnie kontrolkę przycisk `CreateRoleButton` w projektancie, aby utworzyć procedurę obsługi zdarzeń `Click`, a następnie Dodaj następujący kod:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Powyższy kod zaczyna od przypisania przyciętej nazwy roli wprowadzonej w polu tekstowym `RoleName` do zmiennej `newRoleName`. Następnie Metoda `RoleExists` klasy `Roles` jest wywoływana, aby określić, czy rola `newRoleName` już istnieje w systemie. Jeśli rola nie istnieje, jest tworzona za pośrednictwem wywołania metody `CreateRole`. Jeśli `CreateRole` Metoda zostanie przeniesiona o nazwę roli, która już istnieje w systemie, zostanie zgłoszony wyjątek `ProviderException`. Z tego powodu kod najpierw sprawdza, czy rola jeszcze nie istnieje w systemie przed wywołaniem `CreateRole`. Procedura obsługi zdarzeń `Click` kończy przez wyczyszczenie właściwości `Text` pola tekstowego `RoleName`.

> [!NOTE]
> Możesz zastanawiać się, co się stanie, jeśli użytkownik nie wprowadzi żadnej wartości w polu tekstowym `RoleName`. Jeśli wartość przeniesiona do metody `CreateRole` jest `null` lub pustego ciągu, zostanie zgłoszony wyjątek. Podobnie, jeśli nazwa roli zawiera przecinek, wyjątek jest wywoływany. W związku z tym strona powinna zawierać kontrolki walidacji, aby upewnić się, że użytkownik wprowadzi rolę i nie zawiera żadnych przecinki. Pozostawiam jako ćwiczenie dla czytnika.

Utwórzmy rolę o nazwie Administratorzy. Odwiedź stronę `ManageRoles.aspx` za pomocą przeglądarki, wpisz w polu Administratorzy (zobacz rysunek 3), a następnie kliknij przycisk Utwórz rolę.

[![utworzyć rolę administratorów](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Rysunek 3**. Tworzenie roli administratorów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-and-managing-roles-cs/_static/image9.png))

Co się dzieje? Następuje ogłaszanie zwrotne, ale nie ma wizualizacji wskazującej, że rola została faktycznie dodana do systemu. Ta strona zostanie zaktualizowana w kroku 5 w celu uwzględnienia opinii wizualnych. Na razie można jednak sprawdzić, czy rola została utworzona, przechodząc do bazy danych `SecurityTutorials.mdf` i wyświetlając dane z tabeli `aspnet_Roles`. Jak pokazano na rysunku 4, tabela `aspnet_Roles` zawiera rekord dla ról tylko dodani Administratorzy.

[![tabela aspnet_Roles ma wiersz dla administratorów](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Rysunek 4**. tabela `aspnet_Roles` ma wiersz dla administratorów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-and-managing-roles-cs/_static/image12.png))

## <a name="step-5-displaying-the-roles-in-the-system"></a>Krok 5. Wyświetlanie ról w systemie

Uzupełnimy stronę `ManageRoles.aspx`, aby dołączyć listę bieżących ról w systemie. Aby to osiągnąć, Dodaj kontrolkę GridView do strony i ustaw jej Właściwość `ID` na `RoleList`. Następnie Dodaj metodę do klasy powiązanej z kodem strony o nazwie `DisplayRolesInGrid` przy użyciu następującego kodu:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

Metoda `GetAllRoles` klasy `Roles` zwraca wszystkie role w systemie jako tablicę ciągów. Ta tablica ciągów jest następnie powiązana z elementem GridView. Aby powiązać listę ról z rolą GridView podczas pierwszego ładowania strony, musimy wywołać metodę `DisplayRolesInGrid` ze strony programu obsługi zdarzeń `Page_Load`. Poniższy kod wywołuje tę metodę podczas pierwszej wizyty strony, ale nie na kolejnych ogłaszania zwrotnego.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Korzystając z tego kodu, odwiedź stronę za pomocą przeglądarki. Jak pokazano na rysunku 5, powinna zostać wyświetlona siatka z pojedynczą kolumną z etykietą. Siatka zawiera wiersz dla roli Administratorzy dodanej w kroku 4.

[![GridView wyświetla role w jednej kolumnie](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Rysunek 5**. widok GridView wyświetla role w pojedynczej kolumnie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-and-managing-roles-cs/_static/image15.png))

W widoku GridView jest wyświetlana kolumna Lone, ponieważ właściwość `AutoGenerateColumns` GridView jest ustawiona na wartość true (domyślnie), co powoduje, że GridView automatycznie tworzy kolumnę dla każdej właściwości w jej `DataSource`. Tablica ma pojedynczą właściwość, która reprezentuje elementy w tablicy, a tym samym kolumnę w widoku GridView.

Gdy wyświetlasz dane za pomocą widoku GridView, wolisz jawnie definiować moje kolumny, a nie mają być niejawnie generowane przez GridView. Jawnie definiując kolumny, które znacznie ułatwiają formatowanie danych, rozmieszczenie kolumn i wykonywanie innych typowych zadań. W związku z tym zaktualizujmy znaczniki deklaratywne GridView, tak aby jego kolumny były jawnie zdefiniowane.

Zacznij od ustawienia właściwości `AutoGenerateColumns` GridView na wartość false. Następnie Dodaj TemplateField do siatki, ustaw jej Właściwość `HeaderText` na role i skonfiguruj jej `ItemTemplate` tak, aby wyświetlała zawartość tablicy. Aby to osiągnąć, Dodaj kontrolkę sieci Web etykieta o nazwie `RoleNameLabel` do `ItemTemplate` i powiąż jej Właściwość `Text` z `Container.DataItem`.

Te właściwości i zawartość `ItemTemplate`można ustawić deklaratywnie lub za pomocą okna dialogowego pola GridView i edytowanie interfejsu szablonów. Aby uzyskać dostęp do okna dialogowego pola, kliknij link Edytuj kolumny w tagu inteligentnym GridView. Następnie usuń zaznaczenie pola wyboru Automatyczne generowanie pól, aby ustawić właściwość `AutoGenerateColumns` na FAŁSZ, a następnie Dodaj TemplateField do widoku GridView, ustawiając jej Właściwość `HeaderText` na role. Aby zdefiniować zawartość `ItemTemplate`, wybierz opcję Edytuj szablony z tagu inteligentnego GridView. Przeciągnij kontrolkę sieci Web etykieta na `ItemTemplate`, ustaw jej Właściwość `ID` na `RoleNameLabel`i skonfiguruj jej ustawienia wiązania, tak aby jej Właściwość `Text` była powiązana z `Container.DataItem`.

Bez względu na to, z jakiego podejścia korzystasz, efekt deklaratywnego znacznika w GridView powinien wyglądać podobnie do poniższego, gdy skończysz.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Zawartość tablicy jest wyświetlana przy użyciu składni wiązania danych `<%# Container.DataItem %>`. Dokładny opis przyczyny użycia tej składni podczas wyświetlania zawartości tablicy powiązanej z elementem GridView wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji na ten temat, zapoznaj się z tematem [Powiązywanie tablicy skalarnej z kontrolką sieci Web danych](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Obecnie `RoleList` GridView jest powiązany tylko z listą ról podczas pierwszej wizyty strony. Musimy odświeżyć siatkę za każdym razem, gdy zostanie dodana nowa rola. W tym celu należy zaktualizować program obsługi zdarzeń `Click` `CreateRoleButton`, tak aby wywoła metodę `DisplayRolesInGrid`, jeśli zostanie utworzona nowa rola.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Teraz, gdy użytkownik doda nową rolę, w `RoleList` GridView zostanie wyświetlona rola dodana po stronie ogłaszania zwrotnego, która zapewnia wizualne Opinie, że rola została pomyślnie utworzona. Aby to zilustrować, odwiedź stronę `ManageRoles.aspx` za pomocą przeglądarki i dodaj rolę o nazwie kierownicy. Po kliknięciu przycisku Utwórz rolę zostanie nastąpi odświeżenie, a siatka zostanie zaktualizowana w celu uwzględnienia administratorów, a także nowej roli, opiekunów.

[![dodano rolę kierownicy](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Ilustracja 6**. Rola kierownicy została dodana ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-and-managing-roles-cs/_static/image18.png))

## <a name="step-6-deleting-roles"></a>Krok 6. Usuwanie ról

W tym momencie użytkownik może utworzyć nową rolę i wyświetlić wszystkie istniejące role ze strony `ManageRoles.aspx`. Zezwól użytkownikom na także usuwanie ról. Metoda `Roles.DeleteRole` ma dwa przeciążenia:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) — usuwa rolę roli *role*. Wyjątek jest zgłaszany, Jeśli rola zawiera co najmniej jednego członka.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) — usuwa rolę roli *role*. Jeśli *throwOnPopulateRole* jest `true`, zostanie zgłoszony wyjątek, Jeśli rola zawiera co najmniej jednego członka. Jeśli *throwOnPopulateRole* jest `false`, rola zostaje usunięta, niezależnie od tego, czy zawiera żadnych członków, czy nie. Wewnętrznie metoda `DeleteRole(roleName)` wywołuje `DeleteRole(roleName, true)`.

Metoda `DeleteRole` generuje również wyjątek, jeśli *rolename* jest `null` lub pusty ciąg lub jeśli *rolename* zawiera przecinek. Jeśli *rolename* nie istnieje w systemie, `DeleteRole` w trybie dyskretnym, nie wywołując wyjątku.

Uzupełnimy widok GridView w `ManageRoles.aspx`, aby uwzględnić przycisk usuwania, który po kliknięciu spowoduje usunięcie wybranej roli. Zacznij od dodania przycisku Usuń do widoku GridView, przechodząc do okna dialogowego pola i dodając przycisk Usuń, który znajduje się w opcji CommandField. Zwolnij przycisk Usuń w lewej kolumnie i ustaw jej Właściwość `DeleteText` na wartość Usuń rolę.

[![dodać przycisk usuwania do widoku GridView RoleList](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Rysunek 7**. Dodawanie przycisku usuwania do `RoleList` GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-and-managing-roles-cs/_static/image21.png))

Po dodaniu przycisku Usuń znaczniki deklaratywne GridView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `RowDeleting` GridView. Jest to zdarzenie zgłaszane w przypadku ogłaszania zwrotnego po kliknięciu przycisku Usuń rolę. Dodaj następujący kod do programu obsługi zdarzeń.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Kod jest uruchamiany przez programowe odwołujące się do `RoleNameLabel` formantu sieci Web w wierszu, którego przycisk usuwania roli został kliknięty. Metoda `Roles.DeleteRole` jest następnie wywoływana, przekazanie do `Text` `RoleNameLabel` i `false`, usuwając rolę niezależnie od tego, czy istnieją wszyscy użytkownicy skojarzeni z tą rolą. Na koniec `RoleList` GridView jest odświeżane, dzięki czemu rola usunięta tylko w siatce nie będzie już widoczna.

> [!NOTE]
> Przycisk Usuń rolę nie wymaga żadnych potwierdzeń użytkownika przed usunięciem roli. Jednym z najprostszych sposobów potwierdzania akcji jest użycie okna dialogowego potwierdzenia po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Dodawanie potwierdzenia po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

## <a name="summary"></a>Podsumowanie

Wiele aplikacji sieci Web ma pewne reguły autoryzacji lub funkcje na poziomie strony, które są dostępne tylko dla niektórych klas użytkowników. Na przykład może istnieć zestaw stron sieci Web, do których mają dostęp tylko Administratorzy. Zamiast definiować te reguły autoryzacji dla poszczególnych użytkowników, często bardziej przydatne do definiowania reguł opartych na roli. Oznacza to, że zamiast jawnie Zezwalanie użytkownikom Scotta i Jisun na dostęp do administracyjnych stron sieci Web, bardziej łatwość utrzymania jest umożliwienie członkom roli Administratorzy dostępu do tych stron, a następnie do oznaczania Scott i Jisun jako użytkowników należących do Rola Administratorzy.

Struktura ról ułatwia tworzenie ról i zarządzanie nimi. W tym samouczku opisano sposób konfigurowania struktury ról do korzystania z `SqlRoleProvider`, która używa Microsoft SQL Server bazy danych jako magazynu roli. Utworzyliśmy również stronę sieci Web, która wyświetla listę istniejących ról w systemie i umożliwia tworzenie nowych ról i istniejących do usunięcia. W kolejnych samouczkach zobaczymy, jak przypisać użytkowników do ról i jak zastosować autoryzację opartą na rolach.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Badanie członkostwa, ról i profilu w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Instrukcje: korzystanie z menedżera ról w ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Dostawcy ról](https://msdn.microsoft.com/library/aa478950.aspx)
- [Stopniowa narzędzie do administrowania witryną sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Dokumentacja techniczna elementu `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)
- [Korzystanie z interfejsów API członkostwa i programu role Manager](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka obejmują Alicja Maziarz, suchi Banerjee i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](assigning-roles-to-users-cs.md)
