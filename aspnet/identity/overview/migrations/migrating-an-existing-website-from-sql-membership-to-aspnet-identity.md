---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrowanie istniejącej witryny sieci Web z członkostwa SQL do ASP.NET Identity ASP.NET 4. x
author: Rick-Anderson
description: W tym samouczku przedstawiono procedurę migrowania istniejącej aplikacji sieci Web z danymi użytkownika i roli utworzonych przy użyciu członkostwa SQL do nowych ASP.NET Identity s...
ms.author: riande
ms.date: 12/19/2014
ms.custom: seoapril2019
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 633229cc4311d151121bf6a91b9fa8aeecca1197
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583728"
---
# <a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrowanie istniejącej witryny internetowej z członkostwa SQL do systemu ASP.NET Identity

Autor [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> W tym samouczku przedstawiono procedurę migrowania istniejącej aplikacji sieci Web z danymi użytkownika i roli utworzonych przy użyciu członkostwa SQL w nowym systemie ASP.NET Identity. To podejście obejmuje zmianę istniejącego schematu bazy danych na ten, który jest wymagany przez ASP.NET Identity i hak w stare/nowe klasy do niego. Po zastosowaniu tej metody po migracji bazy danych przyszłe aktualizacje tożsamości będą obsługiwane bezproblemowo.

W tym samouczku utworzymy szablon aplikacji sieci Web (Formularze sieci Web) utworzony za pomocą programu Visual Studio 2010 do tworzenia danych użytkownika i roli. Następnie użyjemy skryptów SQL do przeprowadzenia migracji istniejącej bazy danych do tabel wymaganych przez system tożsamości. Następnie zainstalujemy wymagane pakiety NuGet i dodamy nowe strony zarządzania kontami, które używają systemu tożsamości do zarządzania członkostwem. W ramach testu migracji użytkownicy utworzeni przy użyciu członkostwa SQL powinni mieć możliwość zalogowania się, a nowi użytkownicy powinni mieć możliwość zarejestrowania się. Pełny przykład można znaleźć [tutaj](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/). Zobacz również [Migrowanie z członkostwa ASP.NET do ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Wprowadzenie

### <a name="creating-an-application-with-sql-membership"></a>Tworzenie aplikacji z członkostwem SQL

1. Musimy zacząć od istniejącej aplikacji, która korzysta z członkostwa SQL i ma dane dotyczące użytkowników i ról. Na potrzeby tego artykułu utworzymy aplikację sieci Web w programie Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Za pomocą narzędzia do konfiguracji ASP.NET utwórz 2 użytkowników: **oldAdminUser** i **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Utwórz rolę o nazwie admin i Dodaj "oldAdminUser" jako użytkownika w tej roli.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Utwórz sekcję administratora witryny z wartością default. aspx. Ustaw tag autoryzacja w pliku Web. config, aby umożliwić dostęp tylko użytkownikom w rolach administratora. Więcej informacji można znaleźć tutaj [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Wyświetl bazę danych w Eksplorator serwera, aby zrozumieć tabele utworzone przez system członkostwa SQL. Dane logowania użytkownika są przechowywane w tabelach\_ASPNET i ASPNET\_, podczas gdy dane roli są przechowywane w tabeli role\_ASPNET. Informacje o użytkownikach, w których role są przechowywane w tabeli ASPNET\_UsersInRoles. Aby zarządzać członkostwem w warstwie Podstawowa, wystarczy przenieść informacje w powyższych tabelach do systemu ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrowanie do Visual Studio 2013

1. Zainstaluj Visual Studio Express 2013 dla sieci Web lub Visual Studio 2013 wraz z [najnowszymi aktualizacjami](https://www.microsoft.com/download/details.aspx?id=44921).
2. Otwórz powyższy projekt w zainstalowanej wersji programu Visual Studio. Jeśli na komputerze nie zainstalowano SQL Server Express, podczas otwierania projektu zostanie wyświetlony monit, ponieważ parametry połączenia korzystają z programu SQL Express. Możesz wybrać opcję instalacji programu SQL Express lub obejść, aby zmienić parametry połączenia na LocalDb. W tym artykule zmienimy ją na LocalDb.
3. Otwórz plik Web. config i Zmień parametry połączenia z. SQLExpress do (LocalDb) v 11.0. Usuń "wystąpienie użytkownika = true" z parametrów połączenia.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Otwórz Eksplorator serwera i sprawdź, czy można zaobserwować schemat i dane tabeli.
5. System ASP.NET Identity działa w wersji 4,5 lub nowszej. Przekieruj aplikację do wersji 4,5 lub nowszej.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy.

### <a name="installing-the-nuget-packages"></a>Instalowanie pakietów NuGet

1. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt &gt; **Zarządzaj pakietami NuGet**. W polu wyszukiwania wprowadź ciąg "Asp.net Identity" (tożsamość tożsamości). Wybierz pakiet z listy wyników, a następnie kliknij przycisk Instaluj. Zaakceptuj umowę licencyjną, klikając przycisk "Akceptuję". Należy pamiętać, że ten pakiet zainstaluje pakiety zależności: EntityFramework i Microsoft ASP.NET Identity Core. Podobnie Zainstaluj następujące pakiety (Pomiń ostatnie 4 pakiety OWIN, jeśli nie chcesz włączyć logowania OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrowanie bazy danych do nowego systemu tożsamości

Następnym krokiem jest przeprowadzenie migracji istniejącej bazy danych do schematu wymaganego przez system ASP.NET Identity. Aby to osiągnąć, należy uruchomić skrypt SQL, który zawiera zestaw poleceń umożliwiających tworzenie nowych tabel i Migrowanie istniejących informacji o użytkowniku do nowych tabel. Plik skryptu można znaleźć [tutaj](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Ten plik skryptu jest specyficzny dla tego przykładu. Jeśli schemat tabel utworzonych przy użyciu członkostwa SQL jest dostosowywany lub modyfikowany, należy odpowiednio zmienić skrypty.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Jak wygenerować skrypt SQL na potrzeby migracji schematu

Aby ASP.NET Identity klasy do pracy z danymi istniejących użytkowników, musimy zmigrować schemat bazy danych do tego, co jest wymagane przez ASP.NET Identity. Możemy to zrobić przez dodanie nowych tabel i skopiowanie istniejących informacji do tych tabel. Domyślnie ASP.NET Identity używa EntityFramework do mapowania klas modelu tożsamości z powrotem do bazy danych w celu przechowywania/pobierania informacji. Te klasy modelu implementują podstawowe interfejsy tożsamości definiujące obiekty użytkowników i ról. Tabele i kolumny w bazie danych są oparte na tych klasach modelu. Klasy modelu EntityFramework w 2.1.0 Identity v i ich właściwości są zgodne z definicją poniżej

| **IdentityUser** | **Typ** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Identyfikator | ciąg | Identyfikator | RoleId | ProviderKey | Identyfikator |
| Nazwa użytkownika | ciąg | Name (Nazwa) | UserId | UserId | Claim |
| PasswordHash | ciąg |  |  | LoginProvider | ClaimValue |
| SecurityStamp | ciąg |  |  |  | Identyfikator\_użytkownika |
| Email | ciąg |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | ciąg |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Musimy mieć tabele dla każdego z tych modeli z kolumnami odpowiadającymi właściwościom. Mapowanie między klasami i tabelami jest zdefiniowane w metodzie `OnModelCreating` `IdentityDBContext`. Jest to tzw. Metoda konfiguracji interfejsu API Fluent i więcej informacji można znaleźć [tutaj](https://msdn.microsoft.com/data/jj591617.aspx). Konfiguracja klas została opisana poniżej

| **Określonej** | **Tabela** | **Klucz podstawowy** | **Klucz obcy** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Identyfikator |  |
| IdentityRole | AspnetRoles | Identyfikator |  |
| IdentityUserRole | AspnetUserRole | Identyfikator użytkownika i RoleId | Identyfikator\_użytkownika —&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | Identyfikator użytkownika —&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Identyfikator | Identyfikator\_użytkownika —&gt;AspnetUsers |

Za pomocą tych informacji możemy utworzyć instrukcje SQL, aby utworzyć nowe tabele. Możemy ręcznie napisać każdą instrukcję lub wygenerować cały skrypt za pomocą poleceń programu PowerShell EntityFramework, które możemy edytować zgodnie z potrzebami. W tym celu w programie VS Otwórz **konsolę Menedżera pakietów** z menu Widok lub **Narzędzia** **.**

- Uruchom polecenie "Enable-migrations", aby włączyć migracje EntityFramework.
- Uruchom polecenie "Add-Migration Initial", które tworzy początkowy kod instalatora w celu utworzenia bazy danych C#w/VB.
- Ostatnim krokiem jest uruchomienie polecenia "Update-Database-Script", które generuje skrypt SQL na podstawie klas modelu.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Ten skrypt generowania bazy danych może służyć jako początek, w którym będziemy wprowadzać dodatkowe zmiany w celu dodawania nowych kolumn i kopiowania danych. Zaletą tego jest generowanie `_MigrationHistory` tabeli, która jest używana przez EntityFramework do modyfikowania schematu bazy danych, gdy klasy modelu zmieniają się w przyszłych wersjach tożsamości.

Informacje o użytkowniku członkostwa w programie SQL mają inne właściwości oprócz tych, które są używane w klasie modelu użytkownika tożsamości, w tym adres e-mail, próby hasła, Data ostatniego logowania, Data ostatniego zablokowania itd. Jest to przydatne informacje, które chcemy przenieść do systemu tożsamości. Można to zrobić, dodając dodatkowe właściwości do modelu użytkownika i mapując je z powrotem do kolumn tabeli w bazie danych. Możemy to zrobić przez dodanie klasy, która jest podklasą modelu `IdentityUser`. Można dodać właściwości do tej klasy niestandardowej i edytować skrypt SQL, aby dodać odpowiednie kolumny podczas tworzenia tabeli. Kod dla tej klasy został szczegółowo opisany w artykule. Skrypt SQL służący do tworzenia tabeli `AspnetUsers` po dodaniu nowych właściwości zostałby

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Następnie musimy skopiować istniejące informacje z bazy danych członkostwa SQL do nowo dodanych tabel dla tożsamości. Można to zrobić za pomocą języka SQL, kopiując dane bezpośrednio z jednej tabeli do drugiej. Aby dodać dane do wierszy tabeli, używamy konstrukcji `INSERT INTO [Table]`. Aby skopiować z innej tabeli, możemy użyć instrukcji `INSERT INTO` wraz z instrukcją `SELECT`. Aby uzyskać wszystkie informacje o użytkowniku, których potrzebujemy, aby wykonać zapytanie dotyczące *\_użytkowników* i *ASPNET\_tabele członkostwa* i skopiować dane do tabeli *AspNetUsers* . Używamy `INSERT INTO` i `SELECT` razem z instrukcjami `JOIN` i `LEFT OUTER JOIN`. Aby uzyskać więcej informacji na temat wykonywania zapytań i kopiowania danych między tabelami, Skorzystaj z [tego](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) linku. Ponadto tabele AspnetUserLogins i AspnetUserClaims są puste, aby zacząć od, ponieważ nie ma żadnych informacji w członkostwie SQL, które są domyślnie mapowane na ten program. Jedyne skopiowane informacje dotyczą użytkowników i ról. W przypadku projektu utworzonego w poprzednich krokach zapytanie SQL do kopiowania informacji do tabeli użytkownicy byłyby

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

W powyższej instrukcji języka SQL informacje dotyczące każdego użytkownika z tabel *aspnet\_users* i *ASPNET\_Membership* są kopiowane do kolumn tabeli *AspnetUsers* . Jedyną modyfikacją wykonywaną w tym miejscu jest skopiowanie hasła. Ponieważ algorytm szyfrowania dla haseł w członkostwie w programie SQL Server użył "PasswordSalt" i "PasswordFormat", jest on kopiowany również przy użyciu hasła skrótu, aby można było go użyć do odszyfrowania hasła przez tożsamość. Jest to wyjaśnione dalej w artykule podczas podłączania niestandardowego skrótu hasła.

Ten plik skryptu jest specyficzny dla tego przykładu. W przypadku aplikacji, które mają dodatkowe tabele, deweloperzy mogą postępować zgodnie z podobnym podejściem do dodawania dodatkowych właściwości klasy modelu użytkownika i mapowania ich do kolumn w tabeli AspnetUsers. Aby uruchomić skrypt,

1. Otwórz Eksplorator serwera. Rozwiń połączenie "ApplicationServices", aby wyświetlić tabele. Kliknij prawym przyciskiem myszy węzeł tabele i wybierz opcję "nowe zapytanie"

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. W oknie zapytania skopiuj i wklej cały skrypt SQL z pliku migrations. SQL. Uruchom plik skryptu, naciskając przycisk strzałki "wykonaj".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Odśwież okno Eksplorator serwera. W bazie danych są tworzone pięć nowych tabel.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Poniżej przedstawiono sposób mapowania informacji w tabelach członkostwa SQL do nowego systemu tożsamości.

    Role\_ASPNET —&gt; AspNetRoles

    środowisko ASP\_użytkowników i ASP\_w&gt; AspNetUsers

    ASPNET\_UserInRoles--&gt; AspNetUserRoles

    Zgodnie z opisem w powyższej sekcji tabele AspNetUserClaims i AspNetUserLogins są puste. Pole "rozróżniacz" w tabeli AspNetUser powinno być zgodne z nazwą klasy modelu, która jest zdefiniowana w następnym kroku. Ponadto kolumna PasswordHash ma postać "szyfrowanego hasła | sól hasła | format hasła". Pozwala to na użycie specjalnej logiki kryptograficznej członkostwa w programie SQL, aby można było ponownie używać starych haseł. Wyjaśniono w dalszej części artykułu.

### <a name="creating-models-and-membership-pages"></a>Tworzenie modeli i stron członkostwa

Jak wspomniano wcześniej, funkcja Identity używa Entity Framework, aby komunikować się z bazą danych w celu zapisywanych domyślnie informacji o koncie. Aby można było korzystać z istniejących danych w tabeli, należy utworzyć klasy modeli, które mapują z powrotem do tabel i podłączają je do systemu tożsamości. W ramach kontraktu tożsamości klasy modelu powinny implementować interfejsy zdefiniowane w bibliotece DLL Identity. Core lub można zwiększyć istniejącą implementację tych interfejsów dostępnych w pliku Microsoft. AspNet. Identity. EntityFramework.

W naszym przykładzie tabele AspNetRoles, AspNetUserClaims, AspNetLogins i AspNetUserRole mają kolumny podobne do istniejącej implementacji systemu tożsamości. W związku z tym można ponownie użyć istniejących klas do mapowania na te tabele. Tabela AspNetUser zawiera dodatkowe kolumny, które są używane do przechowywania dodatkowych informacji z tabel członkostwa SQL. Można to zamapować przez utworzenie klasy modelu rozszerzającej istniejącą implementację elementu "IdentityUser" i dodanie dodatkowych właściwości.

1. Utwórz folder models w projekcie i Dodaj użytkownika klasy. Nazwa klasy powinna być zgodna z danymi dodanymi w kolumnie "rozróżniacz" tabeli "AspnetUsers".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Klasa User powinna mieć rozszerzoną klasę IdentityUser, która znajduje się w bibliotece DLL *Microsoft. ASPNET. Identity. EntityFramework* . Zadeklaruj właściwości w klasie, która mapuje z powrotem do kolumn AspNetUser. Właściwości ID, username, PasswordHash i SecurityStamp są zdefiniowane w IdentityUser i dlatego są pomijane. Poniżej znajduje się kod klasy użytkownika, która ma wszystkie właściwości

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Entity Framework DbContext Class jest wymagana w celu utrwalania danych w modelach z powrotem do tabel i pobierania danych z tabel w celu wypełnienia modeli. Plik *Microsoft. ASPNET. Identity. EntityFramework* DLL definiuje klasę IdentityDbContext, która współdziała z tabelami tożsamości w celu pobierania i przechowywania informacji. IdentityDbContext&lt;Tuser&gt; przyjmuje klasę "TUser", która może być dowolną klasą, która rozszerza klasę IdentityUser.

    Utwórz nową klasę ApplicationDBContext, która rozszerza IdentityDbContext w folderze "models", przekazując w klasie "User" utworzonej w kroku 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Zarządzanie użytkownikami w nowym systemie tożsamości odbywa się przy użyciu klasy Usermanager&lt;Tuser&gt; zdefiniowanej w bibliotece DLL *Microsoft. ASPNET. Identity. EntityFramework* . Musimy utworzyć klasę niestandardową, która rozszerza element Usermanager, przekazując w klasie "User" utworzonej w kroku 1.

    W folderze modele Utwórz nową klasę Usermanager, która rozszerza użytkownika&lt;użytkownika&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Hasła użytkowników aplikacji są szyfrowane i przechowywane w bazie danych programu. Algorytm kryptograficzny używany w członkostwie SQL jest inny niż w nowym systemie tożsamości. Aby ponownie użyć starych haseł, należy selektywnie odszyfrowywać hasła podczas logowania starych użytkowników przy użyciu algorytmu członkostwa SQL przy użyciu algorytmu kryptograficznego w tożsamości dla nowych użytkowników.

    Klasa Usermanager ma właściwość "PasswordHasher", która przechowuje wystąpienie klasy implementującej interfejs "IPasswordHasher". Służy do szyfrowania/odszyfrowywania haseł podczas transakcji uwierzytelniania użytkownika. W klasie Usermanager zdefiniowanej w kroku 3 Utwórz nową klasę SQLPasswordHasher i Skopiuj poniższy kod.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Usuń błędy kompilacji, importując przestrzenie nazw System. Text i system. Security. Cryptography.

    Metoda EncodePassword szyfruje hasło zgodnie z domyślną implementacją kryptograficzną członkostwa w programie SQL. Jest to pobierane z biblioteki DLL systemu system. Web. Jeśli stara aplikacja użyła niestandardowej implementacji, powinna zostać odzwierciedlona tutaj. Musimy zdefiniować dwie inne metody *HashPassword* i *VerifyHashedPassword* , które używają metody *EncodePassword* do mieszania danego hasła lub weryfikowania hasła w postaci zwykłego tekstu przy użyciu jednego z istniejących w bazie danych.

    System członkostwa SQL używa PasswordHash, PasswordSalt i PasswordFormat do mieszania hasła wprowadzonego przez użytkowników podczas rejestrowania lub zmiany hasła. Podczas migracji wszystkie trzy pola są przechowywane w kolumnie PasswordHash w tabeli AspNetUser oddzielonej znakiem "|". Gdy użytkownik loguje się, a hasło zawiera te pola, do sprawdzenia hasła używane są Kryptografia członkostwa SQL. w przeciwnym razie użyjemy domyślnego kryptografii systemu tożsamości do zweryfikowania hasła. W ten sposób stara użytkownicy nie będą musieli zmieniać haseł po migracji aplikacji.
5. Zadeklaruj konstruktora dla klasy Usermanager i przekaż go jako SQLPasswordHasher do właściwości w konstruktorze.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Tworzenie nowych stron zarządzania kontami

Następny krok migracji polega na dodaniu stron zarządzania kontami, które pozwolą na rejestrację i zalogowanie użytkownika. Stare strony kont z członkostwa SQL używają kontroli, które nie współpracują z nowym systemem tożsamości. Aby dodać nowe strony zarządzania użytkownikami, postępuj zgodnie z samouczkiem z tego linku [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) rozpoczynając od kroku "Dodawanie formularzy sieci Web do rejestracji użytkowników w aplikacji", ponieważ został już utworzony projekt i dodaliśmy pakiety NuGet.

Musimy wprowadzić pewne zmiany, aby przykład mógł współpracować z projektem.

- W przypadku klas Register.aspx.cs i Login.aspx.cs za pomocą `UserManager` z pakietów tożsamości można utworzyć użytkownika. W tym przykładzie Użyj elementu Usermanager dodanego w folderze models, wykonując kroki wymienione wcześniej.
- Użyj klasy utworzonej zamiast IdentityUser w kodzie Register.aspx.cs i Login.aspx.cs. Ten punkt zaczepienia w naszej niestandardowej klasie użytkownika do systemu tożsamości.
- Część do utworzenia bazy danych może zostać pominięta.
- Deweloper musi ustawić nowy użytkownik, aby pasował do bieżącego identyfikatora aplikacji. Można to zrobić, wykonując zapytania dotyczące tej aplikacji, zanim obiekt użytkownika zostanie utworzony w klasie Register.aspx.cs i ustawiając przed utworzeniem użytkownika.

    Przykład:

    Zdefiniuj metodę na stronie Register.aspx.cs, aby wykonać zapytanie dotyczące tabeli aplikacji ASPNET\_i uzyskać identyfikator aplikacji zgodnie z nazwą aplikacji

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Teraz ustaw tę wartość dla obiektu User

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Użyj starej nazwy użytkownika i hasła, aby zalogować istniejącego użytkownika. Na stronie Rejestracja Utwórz nowego użytkownika. Sprawdź również, czy użytkownicy znajdują się w rolach zgodnie z oczekiwaniami.

Przenoszenie do systemu tożsamości ułatwia użytkownikowi dodawanie uwierzytelniania Open Authentication (OAuth) do aplikacji. Zapoznaj się [z przykładem](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/) z włączonym uwierzytelnianiem OAuth.

## <a name="next-steps"></a>Następne kroki

W tym samouczku pokazano, jak przenieść użytkowników z członkostwa SQL do ASP.NET Identity, ale nie portem danych profilu. W następnym samouczku przejdziemy do przenoszenia danych profilu z członkostwa SQL do nowego systemu tożsamości.

Możesz opuścić opinię w dolnej części tego artykułu.

*Dziękujemy za Dykstra i Rick Anderson w celu przejrzenia artykułu.*
