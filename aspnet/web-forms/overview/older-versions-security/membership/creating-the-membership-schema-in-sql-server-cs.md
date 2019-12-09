---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Tworzenie schematu członkostwa w SQL Server (C#) | Microsoft Docs
author: rick-anderson
description: Ten samouczek zaczyna się od zbadania technik dodawania niezbędnego schematu do bazy danych w celu użycia SqlMembershipProvider. Oto Wi...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 97623e7c13ab7799b9dadbb8e52be8e0cd99e252
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594993"
---
# <a name="creating-the-membership-schema-in-sql-server-c"></a>Tworzenie schematu członkostwa w programie SQL Server (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Ten samouczek zaczyna się od zbadania technik dodawania niezbędnego schematu do bazy danych w celu użycia SqlMembershipProvider. Po tym sprawdzimy tabele kluczy w schemacie i omawiają ich znaczenie i znaczenie. W tym samouczku zawarto informacje na temat sposobu informowania aplikacji ASP.NET, która powinna być używana przez platformę członkowską.

## <a name="introduction"></a>Wprowadzenie

Poprzednie dwa samouczki sprawdzają się przy użyciu uwierzytelniania formularzy, aby identyfikować odwiedzających witrynę sieci Web. Platforma uwierzytelniania formularzy ułatwia deweloperom rejestrowanie użytkownika w witrynie sieci Web i zapamiętanie ich na stronach na stronie za pomocą biletów uwierzytelniania. Klasa `FormsAuthentication` obejmuje metody generowania biletu i dodawania go do plików cookie użytkownika. `FormsAuthenticationModule` analizuje wszystkie żądania przychodzące i dla tych z prawidłowym biletem uwierzytelniania tworzy i kojarzy `GenericPrincipal` i obiekt `FormsIdentity` z bieżącym żądaniem. Uwierzytelnianie formularzy jest tylko mechanizmem przyznawania biletu uwierzytelniania do osoby odwiedzającej podczas logowania się i, w przypadku kolejnych żądań, analizowanie tego biletu w celu ustalenia tożsamości użytkownika. Aby aplikacja sieci Web obsługiwała konta użytkowników, nadal musimy wdrożyć magazyn użytkowników i dodać funkcje do sprawdzania poprawności poświadczeń, rejestrowania nowych użytkowników i wyposażono innych zadań związanych z kontem użytkownika.

Przed ASP.NET 2,0 deweloperzy znajdowały się w punkcie zaczepienia dotyczącym wdrażania wszystkich tych zadań związanych z kontem użytkownika. Na szczęście zespół ASP.NET uznał to brak i wprowadził strukturę członkostwa z ASP.NET 2,0. Platforma członkostwa jest zestawem klas w .NET Framework, które udostępniają interfejs programistyczny służący do wykonywania podstawowych zadań związanych z kontem użytkownika. Ta struktura jest skompilowana korzystającego [model dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), dzięki czemu deweloperzy mogą podłączyć dostosowaną implementację do standardowego interfejsu API.

Zgodnie z opisem w <a id="Tutorial1"> </a>samouczku [*podstawy zabezpieczeń i pomocy technicznej ASP.NET*](../introduction/security-basics-and-asp-net-support-cs.md) .NET Framework są dostarczane z dwoma wbudowanymi dostawcami członkostwa: [`ActiveDirectoryMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) i [`SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Jak nazywa się to, `SqlMembershipProvider` używa bazy danych Microsoft SQL Server jako magazynu użytkownika. Aby można było korzystać z tego dostawcy w aplikacji, musimy poinformować dostawcę o bazie danych, która ma być używana jako magazyn. Jak to możliwe, `SqlMembershipProvider` oczekuje, że baza danych magazynu użytkownika będzie mieć pewne tabele, widoki i procedury składowane bazy danych. Musimy dodać ten oczekiwany schemat do wybranej bazy danych.

Ten samouczek zaczyna się od zbadania technik dodawania niezbędnego schematu do bazy danych w celu użycia `SqlMembershipProvider`. Po tym sprawdzimy tabele kluczy w schemacie i omawiają ich znaczenie i znaczenie. W tym samouczku zawarto informacje na temat sposobu informowania aplikacji ASP.NET, która powinna być używana przez platformę członkowską.

Zacznijmy!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Krok 1. decydowanie o miejscu, w którym ma zostać umieszczony magazyn użytkownika

Dane aplikacji ASP.NET są często przechowywane w wielu tabelach w bazie danych. Podczas implementowania schematu bazy danych `SqlMembershipProvider` należy zdecydować, czy umieścić schemat członkostwa w tej samej bazie danych co dane aplikacji, czy w alternatywnej bazie danych.

Zaleca się lokalizowanie schematu członkostwa w tej samej bazie danych co dane aplikacji z następujących powodów:

- **Łatwość konserwacji** aplikacji, której dane są hermetyzowane w jednej bazie danych, ułatwiają zrozumienie, konserwację i wdrażanie aplikacji, która ma dwie osobne bazy danych.
- **Integralność relacyjna** poprzez lokalizowanie tabel związanych z członkostwem w tej samej bazie danych co tabele aplikacji możliwe jest ustanowienie [ograniczeń klucza obcego](http://en.wikipedia.org/wiki/Foreign_key) między kluczami podstawowymi w tabelach związanych z członkostwem i powiązanych tabelach aplikacji.

Oddzielanie danych ze sklepu użytkownika i aplikacji do oddzielnych baz danych ma sens tylko w przypadku wielu aplikacji, które używają oddzielnych baz danych, ale muszą mieć wspólny magazyn użytkowników.

### <a name="creating-a-database"></a>Tworzenie bazy danych

Aplikacja, która została zbudowana, ponieważ drugi samouczek nie wymagał jeszcze bazy danych. W przypadku magazynu użytkowników potrzebujemy tej usługi już teraz. Utwórzmy ją, a następnie dodamy do niej schemat wymagany przez dostawcę `SqlMembershipProvider` (zobacz krok 2).

> [!NOTE]
> W tej serii samouczków będziemy używać bazy danych [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) do przechowywania tabel aplikacji i schematu `SqlMembershipProvider`. Ta decyzja została podjęta z dwóch powodów: najpierw z powodu jej bezpłatnej wersji — wersja Express jest najbardziej dostępną wersją SQL Server 2005; Ponadto bazy danych SQL Server 2005 Express Edition mogą być umieszczane bezpośrednio w folderze `App_Data` aplikacji sieci Web, dzięki czemu cinch bazę danych i aplikację sieci Web w jednym pliku ZIP i wdrożyć ją ponownie bez żadnych specjalnych instrukcji instalacji ani opcji konfiguracji. Jeśli wolisz korzystać z usługi SQL Server w wersji innej niż Express, śmiało. Kroki są praktycznie identyczne. Schemat `SqlMembershipProvider` będzie działał z dowolnymi wersjami Microsoft SQL Server 2000 i.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder `App_Data` i wybierz polecenie Dodaj nowy element. (Jeśli nie widzisz folderu `App_Data` w projekcie, kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań, wybierz polecenie Dodaj folder ASP.NET i wybierz pozycję `App_Data`). W oknie dialogowym Dodaj nowy element wybierz opcję dodania nowego SQL Database o nazwie `SecurityTutorials.mdf`. W tym samouczku dodamy schemat `SqlMembershipProvider` do tej bazy danych. w kolejnych samouczkach utworzymy dodatkowe tabele do przechwytywania danych aplikacji.

[![dodać nową SQL Database o nazwie SecurityTutorials. mdf do folderu App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Rysunek 1**. dodawanie nowego SQL Database o nazwie `SecurityTutorials.mdf` Database do folderu `App_Data` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))

Dodanie bazy danych do folderu `App_Data` powoduje automatyczne uwzględnienie jej w widoku Eksplorator bazy danych. (W wersji innej niż Express programu Visual Studio Eksplorator bazy danych jest nazywana Eksplorator serweraą). Przejdź do Eksplorator bazy danych i rozwiń dodaną `SecurityTutorials` bazę danych. Jeśli nie widzisz Eksplorator bazy danych na ekranie, przejdź do menu Widok i wybierz polecenie Eksplorator bazy danych lub naciśnij klawisze CTRL + ALT + S. Jak pokazano na rysunku 2, `SecurityTutorials` baza danych jest pusta — nie zawiera żadnych tabel, brak widoków ani procedur składowanych.

[![baza danych SecurityTutorials jest obecnie pusta](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Rysunek 2**. baza danych `SecurityTutorials` jest obecnie pusta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))

## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Krok 2. Dodawanie schematu`SqlMembershipProvider`do bazy danych

`SqlMembershipProvider` wymaga zainstalowania określonego zestawu tabel, widoków i procedur składowanych w bazie danych magazynu użytkownika. Te wymagane obiekty bazy danych można dodać za pomocą [narzędzia`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). Ten plik znajduje się w folderze `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`.

> [!NOTE]
> Narzędzie `aspnet_regsql.exe` oferuje zarówno funkcję wiersza polecenia, jak i graficzny interfejs użytkownika. Interfejs graficzny jest bardziej przyjazny dla użytkownika i jest tym, co będziemy analizować w tym samouczku. Interfejs wiersza polecenia jest przydatny, gdy dodawanie schematu `SqlMembershipProvider` musi być zautomatyzowane, na przykład w skryptach kompilacji lub scenariuszach zautomatyzowanych testów.

Narzędzie `aspnet_regsql.exe` służy do dodawania lub usuwania *usług aplikacji ASP.NET* w określonej SQL Server bazie danych. Usługi aplikacji ASP.NET obejmują schematy dla `SqlMembershipProvider` i `SqlRoleProvider`, a także schematy dla dostawców opartych na bazie danych SQL dla innych platform ASP.NET 2,0. Musimy dostarczyć dwie bity informacji do narzędzia `aspnet_regsql.exe`:

- Czy chcesz dodać lub usunąć usługi aplikacji, i
- Baza danych, z której chcesz dodać lub usunąć schemat usług aplikacji

W przypadku monitowania o użycie bazy danych narzędzie `aspnet_regsql.exe` prosi nas o podanie nazwy serwera, na którym znajduje się baza danych, poświadczenia zabezpieczeń służące do nawiązywania połączenia z bazą danych i nazwę bazy danych. W przypadku korzystania z wersji innej niż Express SQL Server należy znać te informacje, ponieważ są to te same informacje, które należy podać za pośrednictwem parametrów połączenia podczas pracy z bazą danych za pośrednictwem strony sieci Web ASP.NET. Jednak określenie serwera i nazwy bazy danych w przypadku korzystania z SQL Server 2005 Express Editionj bazy danych w folderze `App_Data`, jest to nieco bardziej zaliczane.

W poniższej sekcji przedstawiono prostą metodę określania nazwy serwera i bazy danych bazy danych SQL Server 2005 Express Edition w folderze `App_Data`. Jeśli nie używasz SQL Server 2005 Express Edition nie możesz przejść do sekcji Instalowanie Usługi aplikacji.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theapp_datafolder"></a>Określanie nazwy serwera i bazy danych dla bazy danych SQL Server 2005 Express Edition w folderze`App_Data`

Aby użyć narzędzia `aspnet_regsql.exe` musi znać nazwy serwera i bazy danych. Nazwa serwera jest `localhost\InstanceName`. Najprawdopodobniej nazwa *InstanceName* jest `SQLExpress`. Jeśli jednak zainstalowano SQL Server 2005 Express Edition ręcznie (oznacza to, że nie został on zainstalowany automatycznie podczas instalacji programu Visual Studio), można wybrać inną nazwę wystąpienia.

Nazwa bazy danych to bit trickier do określenia. Bazy danych w folderze `App_Data` zwykle mają nazwę bazy danych, która zawiera [globalnie unikatowy identyfikator](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) wraz ze ścieżką do pliku bazy danych. Musimy określić tę nazwę bazy danych w celu dodania schematu usług aplikacji za pomocą `aspnet_regsql.exe`.

Najprostszym sposobem sprawdzenia nazwy bazy danych jest przeanalizowanie jej za pomocą SQL Server Management Studio. SQL Server Management Studio udostępnia interfejs graficzny służący do zarządzania bazami danych SQL Server 2005, ale nie jest dostarczany z wersją Express SQL Server 2005. Dobrą wiadomośćą jest [możliwość pobrania](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) bezpłatnej wersji Express SQL Server Management Studio.

> [!NOTE]
> Jeśli na pulpicie jest również zainstalowana wersja programu w wersji innej niż Express SQL Server 2005, zostanie prawdopodobnie zainstalowana pełna wersja Management Studio. Możesz użyć pełnej wersji, aby określić nazwę bazy danych, wykonując te same czynności, które opisano poniżej dla wersji Express.

Zacznij od zamykania programu Visual Studio, aby upewnić się, że wszystkie blokady narzucone przez program Visual Studio w pliku bazy danych są zamknięte. Następnie uruchom SQL Server Management Studio i nawiąż połączenie z bazą danych `localhost\InstanceName` SQL Server 2005 Express Edition. Jak wspomniano wcześniej, prawdopodobnie nazwa wystąpienia jest `SQLExpress`. Dla opcji uwierzytelnianie wybierz pozycję Uwierzytelnianie systemu Windows.

[![połączyć się z wystąpieniem SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Rysunek 3**. Nawiązywanie połączenia z wystąpieniem SQL Server 2005 Express Edition ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))

Po nawiązaniu połączenia z wystąpieniem SQL Server 2005 Express Edition Management Studio wyświetla foldery baz danych, ustawień zabezpieczeń, obiektów serwera i tak dalej. Po rozwinięciu karty bazy danych zobaczysz, że baza danych `SecurityTutorials.mdf` *nie* jest zarejestrowana w wystąpieniu bazy danych — musimy najpierw dołączyć bazę danych.

Kliknij prawym przyciskiem myszy folder Databases, a następnie wybierz polecenie Dołącz z menu kontekstowego. Zostanie wyświetlone okno dialogowe Dołączanie baz danych. W tym miejscu kliknij przycisk Dodaj, przejdź do bazy danych `SecurityTutorials.mdf` i kliknij przycisk OK. Rysunek 4 przedstawia okno dialogowe Dołączanie baz danych po wybraniu bazy danych `SecurityTutorials.mdf`. Rysunek 5 zawiera Management Studio Eksplorator obiektów po pomyślnym dołączeniu bazy danych.

[![dołączyć bazę danych SecurityTutorials. mdf](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Ilustracja 4**. dołączanie bazy danych `SecurityTutorials.mdf` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))

[![baza danych SecurityTutorials. mdf pojawia się w folderze Databases](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Rysunek 5**. baza danych `SecurityTutorials.mdf` zostanie wyświetlona w folderze bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))

Jak pokazano na rysunku 5, baza danych `SecurityTutorials.mdf` ma raczej nazwę abstruse. Zmieńmy ją na bardziej zapamiętaną nazwę (i łatwiejszy do pisania). Kliknij prawym przyciskiem myszy bazę danych, wybierz polecenie Zmień nazwę z menu kontekstowego i zmień jego nazwę na `SecurityTutorialsDatabase`. Nie powoduje to zmiany nazwy pliku, a tylko nazwy, która jest wykorzystywana przez bazę do SQL Server.

[![zmienić nazwy bazy danych na SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Ilustracja 6**. zmiana nazwy bazy danych na `SecurityTutorialsDatabase`([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))

W tym momencie wiemy, że nazwy serwera i bazy danych dla `SecurityTutorials.mdf` pliku bazy danych: odpowiednio `localhost\InstanceName` i `SecurityTutorialsDatabase`. Teraz można przystąpić do instalowania usług aplikacji za pomocą narzędzia `aspnet_regsql.exe`.

### <a name="installing-the-application-services"></a>Instalowanie Usługi aplikacji

Aby uruchomić narzędzie `aspnet_regsql.exe`, przejdź do menu Start i wybierz polecenie Uruchom. Wprowadź `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` w polu tekstowym, a następnie kliknij przycisk OK. Alternatywnie możesz użyć Eksploratora Windows, aby przejść do odpowiedniego folderu i kliknąć dwukrotnie plik `aspnet_regsql.exe`. Każde podejście będzie w tej samej postaci netto.

Uruchomienie narzędzia `aspnet_regsql.exe` bez argumentów wiersza polecenia uruchamia Kreatora instalacji programu ASP.NET SQL Server graficzny interfejs użytkownika. Kreator ułatwia dodawanie lub usuwanie usług aplikacji ASP.NET w określonej bazie danych. Pierwszy ekran kreatora przedstawiony na rysunku 7 zawiera opis przeznaczenia tego narzędzia.

[![użyć Kreatora instalacji programu ASP.NET SQL Server, aby dodać schemat członkostwa](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Rysunek 7**. Użyj kreatora instalacji SQL Server ASP.NET, aby dodać schemat członkostwa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))

Drugi krok w Kreatorze pyta o to, czy chcemy dodać usługi aplikacji, czy je usunąć. Ponieważ chcemy dodać tabele, widoki i procedury składowane niezbędne do `SqlMembershipProvider`, wybierz opcję Konfiguruj SQL Server dla usług aplikacji. Później, jeśli chcesz usunąć ten schemat z bazy danych, uruchom ponownie tego kreatora, ale zamiast tego wybierz opcję Usuń informacje o usługach aplikacji z istniejącej bazy danych.

[![wybrać opcję Konfiguruj SQL Server dla Usługi aplikacji](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Ilustracja 8**. Wybierz opcję Skonfiguruj SQL Server dla usługi aplikacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))

Trzeci krok powoduje wyświetlanie informacji o bazie danych: Nazwa serwera, informacje o uwierzytelnianiu i nazwa bazy danych. Jeśli podczas pracy z tym samouczkiem dodano `SecurityTutorials.mdf` bazę danych do `App_Data`, dołączono ją do `localhost\InstanceName`i zmieniono jej nazwę na `SecurityTutorialsDatabase`, a następnie użyj następujących wartości:

- Serwer: `localhost\InstanceName`
- Uwierzytelnianie systemu Windows
- Baza danych: `SecurityTutorialsDatabase`

[![wprowadzić informacje o bazie danych](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Ilustracja 9**. Wprowadzanie informacji o bazie danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))

Po wprowadzeniu informacji o bazie danych kliknij przycisk Dalej. Ostatni krok podsumowuje kroki, które zostaną wykonane. Kliknij przycisk Dalej, aby zainstalować usługi aplikacji, a następnie Zakończ pracę z kreatorem.

> [!NOTE]
> Jeśli użyto Management Studio, aby dołączyć bazę danych i zmienić nazwę pliku bazy danych, pamiętaj o odłączeniu bazy danych i zamknięciu Management Studio przed ponownym otwarciem programu Visual Studio. Aby odłączyć bazę danych `SecurityTutorialsDatabase`, kliknij prawym przyciskiem myszy nazwę bazy danych, a następnie w menu zadania wybierz polecenie Odłącz.

Po zakończeniu pracy Kreatora Wróć do programu Visual Studio i przejdź do Eksplorator bazy danych. Rozwiń folder tabele. Powinna zostać wyświetlona seria tabel, których nazwy rozpoczynają się od prefiksu `aspnet_`. Podobnie różne widoki i procedury składowane można znaleźć w folderach widoki i procedury składowane. Te obiekty bazy danych tworzą schemat usług aplikacji. W kroku 3 sprawdzimy obiekty z bazami danych specyficznych dla członkostwa i ról.

[![różne tabele, widoki i procedury składowane zostały dodane do bazy danych](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Ilustracja 10**. do bazy danych dodano różne tabele, widoki i procedury składowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))

> [!NOTE]
> Graficzny interfejs użytkownika narzędzia `aspnet_regsql.exe` instaluje cały schemat usług aplikacji. Ale podczas wykonywania `aspnet_regsql.exe` z poziomu wiersza polecenia można określić, jakie składniki usług aplikacji zainstalować (lub usunąć). W związku z tym, jeśli chcesz dodać tylko tabele, widoki i procedury składowane niezbędne dla dostawców `SqlMembershipProvider` i `SqlRoleProvider`, uruchom `aspnet_regsql.exe` z wiersza polecenia. Alternatywnie można ręcznie uruchomić odpowiedni podzbiór skryptów tworzenia T-SQL, które są używane przez `aspnet_regsql.exe`. Skrypty te znajdują się w folderze `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` z nazwami, takimi jak `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`i tak dalej.

W tym momencie utworzyliśmy obiekty bazy danych, które są konieczne przez `SqlMembershipProvider`. Jednak nadal musimy poinstruować platformę członkowską, że powinna ona używać `SqlMembershipProvider` (w przeciwieństwie do `ActiveDirectoryMembershipProvider`) i czy `SqlMembershipProvider` powinien korzystać z bazy danych `SecurityTutorials`. Zapoznaj się z informacjami na temat określania dostawcy, który ma być używany, oraz sposobu dostosowywania ustawień wybranego dostawcy w kroku 4. Najpierw przyjrzyjmy się dokładnie utworzonym obiektom bazy danych.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Krok 3. Zapoznaj się z podstawowymi tabelami schematu

Podczas pracy z platformami członkostwa i ról w aplikacji ASP.NET szczegóły implementacji są hermetyzowane przez dostawcę. W przyszłych samouczkach będziemy interfejsować z tymi platformami za pośrednictwem klas `Membership` i `Roles` .NET Framework. W przypadku korzystania z tych interfejsów API wysokiego poziomu nie ma potrzeby wypróbujemy z szczegółami niskiego poziomu, takich jak zapytania są wykonywane lub jakie tabele są modyfikowane przez `SqlMembershipProvider` i `SqlRoleProvider`.

Z tego względu możemy bezpiecznie używać platform członkostwa i ról bez eksplorowania schematu bazy danych utworzonego w kroku 2. Jednak podczas tworzenia tabel do przechowywania danych aplikacji może być konieczne utworzenie jednostek odnoszących się do użytkowników lub ról. Podczas ustanawiania ograniczeń klucza obcego między tabelami danych aplikacji a tymi tabelami utworzonymi w kroku 2, pomocne jest zapoznanie się ze schematami `SqlMembershipProvider` i `SqlRoleProvider`. Ponadto w niektórych rzadkich sytuacjach może być konieczne, aby interfejs użytkownika i rola były przechowywane bezpośrednio na poziomie bazy danych (zamiast za pomocą klas `Membership` lub `Roles`).

### <a name="partitioning-the-user-store-into-applications"></a>Partycjonowanie magazynu użytkowników w aplikacjach

Struktury członkostwa i ról zaprojektowano w taki sposób, że jeden użytkownik i magazyn ról mogą być współużytkowane przez wiele różnych aplikacji. Aplikacja ASP.NET, która korzysta z struktur członkostwa lub ról, musi określać partycję aplikacji, która ma być używana. W krótkim przypadku wiele aplikacji sieci Web może korzystać z tych samych magazynów użytkowników i ról. Rysunek 11 przedstawia magazyny użytkowników i ról, które są podzielone na trzy aplikacje: HRSite, CustomerSite i SalesSite. Każdy z tych trzech aplikacji sieci Web ma własnych unikatowych użytkowników i ról, a jednocześnie wszystkie fizycznie przechowują swoje konta użytkownika i informacje o roli w tych samych tabelach bazy danych.

[Konta użytkowników ![mogą być partycjonowane w wielu aplikacjach](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Ilustracja 11**. konta użytkowników mogą być podzielone na partycje w wielu aplikacjach ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))

Tabela `aspnet_Applications` określa, co definiuje te partycje. Każda aplikacja, która korzysta z bazy danych do przechowywania informacji o kontach użytkowników, jest reprezentowana przez wiersz w tej tabeli. Tabela `aspnet_Applications` ma cztery kolumny: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`i `Description`. `ApplicationId` jest typu [`uniqueidentifier`](https://msdn.microsoft.com/library/ms187942.aspx) i jest kluczem podstawowym tabeli; `ApplicationName` zapewnia unikatową przyjazną nazwę dla każdej aplikacji.

Inne i powiązane z rolami tabele łączy z powrotem do pola `ApplicationId` w `aspnet_Applications`. Na przykład tabela `aspnet_Users`, która zawiera rekord dla każdego konta użytkownika, ma pole klucza obcego `ApplicationId`; Ditto dla tabeli `aspnet_Roles`. Pole `ApplicationId` w tych tabelach Określa partycję aplikacji, do której należy konto użytkownika lub rola.

### <a name="storing-user-account-information"></a>Przechowywanie informacji o koncie użytkownika

Informacje o koncie użytkownika są przechowywane w dwóch tabelach: `aspnet_Users` i `aspnet_Membership`. Tabela `aspnet_Users` zawiera pola, które zawierają podstawowe informacje o koncie użytkownika. Trzy najbardziej odpowiednie kolumny są następujące:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` jest kluczem podstawowym (i typu `uniqueidentifier`). `UserName` jest typu `nvarchar(256)` i, wraz z hasłem, tworzy poświadczenia użytkownika. (Hasło użytkownika jest przechowywane w tabeli `aspnet_Membership`). `ApplicationId` łączy konto użytkownika z określoną aplikacją w programie `aspnet_Applications`. Istnieje [ograniczenie`UNIQUE`](https://msdn.microsoft.com/library/ms191166.aspx) złożonego dla kolumn `UserName` i `ApplicationId`. Gwarantuje to, że w danej aplikacji każda nazwa użytkownika jest unikatowa, ale umożliwia użycie tego samego `UserName` w różnych aplikacjach.

Tabela `aspnet_Membership` zawiera dodatkowe informacje o koncie użytkownika, takie jak hasło użytkownika, adres e-mail, Data i godzina ostatniego logowania itd. Istnieje taka sama zgodność między rekordami w tabelach `aspnet_Users` i `aspnet_Membership`. Ta relacja jest zapewniana przez pole `UserId` w `aspnet_Membership`, które służy jako klucz podstawowy tabeli. Podobnie jak w przypadku tabeli `aspnet_Users`, `aspnet_Membership` zawiera pole `ApplicationId`, które łączy te informacje z określoną partycją aplikacji.

### <a name="securing-passwords"></a>Zabezpieczanie haseł

Informacje o haśle są przechowywane w tabeli `aspnet_Membership`. `SqlMembershipProvider` umożliwia przechowywanie haseł w bazie danych przy użyciu jednej z następujących trzech technik:

- **Wyczyść** — hasło jest przechowywane w bazie danych w postaci zwykłego tekstu. Zdecydowanie odradzam korzystanie z tej opcji. Jeśli baza danych została naruszona, nastąpi hakerowi, który znajdzie tylne drzwi lub pracownika disgruntled, który ma dostęp do bazy danych — każda z nich ma poświadczenia dla każdego pojedynczego użytkownika.
- **Skrót** -hasła są tworzone przy użyciu skrótu jednokierunkowego i losowo generowanej wartości soli. Ta wartość skrótu (wraz z tablicą) jest przechowywana w bazie danych.
- **Zaszyfrowana** — zaszyfrowana wersja hasła jest przechowywana w bazie danych programu.

Stosowana technika przechowywania haseł zależy od ustawień `SqlMembershipProvider` określonych w `Web.config`. Przyjrzyjmy się dostosowywaniu ustawień `SqlMembershipProvider` w kroku 4. Domyślnym zachowaniem jest przechowywanie skrótu hasła.

Kolumny odpowiedzialne za przechowywanie hasła są `Password`, `PasswordFormat`i `PasswordSalt`. `PasswordFormat` to pole typu `int`, którego wartość wskazuje technikę używaną do przechowywania hasła: 0 dla Clear; 1 dla wartości skrótu; 2 dla zaszyfrowanej. `PasswordSalt` ma przypisany losowo wygenerowany ciąg, niezależnie od używanej techniki magazynu haseł; wartość `PasswordSalt` jest używana tylko podczas obliczania skrótu hasła. Na koniec kolumna `Password` zawiera rzeczywiste dane hasła, być hasłem w postaci zwykłego tekstu, skrótem hasła lub zaszyfrowanym hasłem.

Tabela 1 ilustruje, jak te trzy kolumny mogą wyglądać podobnie do różnych technik magazynowania podczas przechowywania hasła. .

| **Technika magazynu&lt;\_o3a\_p/&gt;** | **Hasło&lt;\_o3a\_p/&gt;** | **PasswordFormat&lt;\_o3a\_p/&gt;** | **PasswordSalt&lt;\_o3a\_p/&gt;** |
| --- | --- | --- | --- |
| Wyczyść | Hasło! | 0 | tTnkPlesqissc2y2SMEygA = = |
| Skrótu | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM = | 1 | wFgjUfhdUFOCKQiI61vtiQ = = |
| Szyfrowane | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/AA/oqAXGLHJNBw = = |

**Tabela 1**: przykładowe wartości pól związanych z hasłem podczas zapisywania hasła.

> [!NOTE]
> Określony algorytm szyfrowania lub wyznaczania wartości skrótu używany przez `SqlMembershipProvider` jest określany na podstawie ustawień w elemencie `<machineKey>`. Ten element konfiguracji został omówiony w kroku 3 <a id="Tutorial3"> </a>samouczka [*konfiguracja uwierzytelniania formularzy i Tematy zaawansowane*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) .

### <a name="storing-roles-and-role-associations"></a>Przechowywanie ról i skojarzeń ról

Struktura ról umożliwia deweloperom Definiowanie zestawu ról i określanie, którzy użytkownicy należą do ról. Te informacje są przechwytywane w bazie danych przez dwie tabele: `aspnet_Roles` i `aspnet_UsersInRoles`. Każdy rekord w tabeli `aspnet_Roles` reprezentuje rolę dla określonej aplikacji. Podobnie jak w przypadku tabeli `aspnet_Users` tabela `aspnet_Roles` ma trzy kolumny dotyczące naszej dyskusji:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` jest kluczem podstawowym (i typu `uniqueidentifier`). `RoleName` jest typu `nvarchar(256)`. A `ApplicationId` łączy konto użytkownika z określoną aplikacją w `aspnet_Applications`. W kolumnach `RoleName` i `ApplicationId` istnieje ograniczenie `UNIQUE` złożone, co gwarantuje, że w danej aplikacji każda nazwa roli jest unikatowa.

Tabela `aspnet_UsersInRoles` służy jako mapowanie między użytkownikami i rolami. Istnieją tylko dwie kolumny — `UserId` i `RoleId` — i razem składają się na złożony klucz podstawowy.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Krok 4. Określanie dostawcy i dostosowywanie jego ustawień

Wszystkie platformy, które obsługują model dostawcy, takie jak struktury członkostwa i ról — nie zawierają szczegółów implementacji, a zamiast tego deleguje odpowiedzialność do klasy dostawcy. W przypadku struktury członkostwa Klasa `Membership` definiuje interfejs API do zarządzania kontami użytkowników, ale nie współdziała bezpośrednio z żadnym magazynem użytkowników. Zamiast tego metoda klasy `Membership` przekazuje żądanie do skonfigurowanego dostawcy — będziemy używać `SqlMembershipProvider`. Gdy wywołamy jedną z metod w klasie `Membership`, jak platforma członkowska wie o delegowaniu wywołania do `SqlMembershipProvider`?

Klasa `Membership` ma [właściwość`Providers`](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) , która zawiera odwołanie do wszystkich zarejestrowanych klas dostawcy dostępnych do użycia przez platformę członkowską. Każdy zarejestrowany dostawca ma skojarzoną nazwę i typ. Nazwa oferuje zrozumiały dla człowieka sposób odwołującego się do określonego dostawcy w kolekcji `Providers`, podczas gdy typ identyfikuje klasę dostawcy. Ponadto każdy zarejestrowany dostawca może uwzględnić ustawienia konfiguracji. Ustawienia konfiguracji dla struktury członkostwa obejmują `passwordFormat` i `requiresUniqueEmail`między wieloma innymi. Zobacz tabelę 2, aby uzyskać pełną listę ustawień konfiguracji używanych przez `SqlMembershipProvider`.

Zawartość właściwości `Providers` jest określana za pomocą ustawień konfiguracji aplikacji sieci Web. Domyślnie wszystkie aplikacje sieci Web mają dostawcę o nazwie `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`. Ten domyślny dostawca członkostwa jest zarejestrowany w `machine.config` (znajdujący się w `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Jak pokazano powyżej, [element`<membership>`](https://msdn.microsoft.com/library/1b9hw62f.aspx) definiuje ustawienia konfiguracji dla struktury członkostwa, natomiast [element podrzędny`<providers>`](https://msdn.microsoft.com/library/6d4936ht.aspx) określa zarejestrowanych dostawców. Dostawców można dodawać lub usuwać za pomocą elementów [`<add>`](https://msdn.microsoft.com/library/whae3t94.aspx) lub [`<remove>`](https://msdn.microsoft.com/library/aykw9a6d.aspx) ; Użyj elementu [`<clear>`](https://msdn.microsoft.com/library/t062y6yc.aspx) , aby usunąć wszystkich aktualnie zarejestrowanych dostawców. Jak pokazano powyżej, `machine.config` dodaje dostawcę o nazwie `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`.

Oprócz atrybutów `name` i `type`, element `<add>` zawiera atrybuty, które definiują wartości różnych ustawień konfigurowania. Tabela 2 zawiera dostępne ustawienia konfiguracji specyficzne dla `SqlMembershipProvider`wraz z opisem każdego z nich.

> [!NOTE]
> Wszystkie wartości domyślne zanotowane w tabeli 2 odnoszą się do wartości domyślnych zdefiniowanych w klasie `SqlMembershipProvider`. Należy zauważyć, że nie wszystkie ustawienia konfiguracji w `AspNetSqlMembershipProvider` są zgodne z wartościami domyślnymi klasy `SqlMembershipProvider`. Na przykład jeśli nie zostanie określony w dostawcy członkostwa, ustawienie `requiresUniqueEmail` domyślnie wartość true. Jednak `AspNetSqlMembershipProvider` przesłania tę wartość domyślną przez jawne określenie wartości `false`.

| **Ustawianie&lt;\_o3a\_p/&gt;** | **Opis&lt;\_o3a\_p/&gt;** |
| --- | --- |
| `ApplicationName` | Należy przypomnieć, że platforma członkowska pozwala na partycjonowanie pojedynczego magazynu użytkowników w wielu aplikacjach. To ustawienie wskazuje nazwę partycji aplikacji używanej przez dostawcę członkostwa. Jeśli ta wartość nie jest jawnie określona, zostanie ustawiona w czasie wykonywania na wartość wirtualnej ścieżki katalogu głównego aplikacji. |
| `commandTimeout` | Określa wartość limitu czasu polecenia SQL (w sekundach). Wartość domyślna to 30. |
| `connectionStringName` | Nazwa parametrów połączenia w elemencie `<connectionStrings>`, który ma zostać użyty do nawiązania połączenia z bazą danych magazynu użytkownika. Ta wartość jest wymagana. |
| `description` | Zapewnia przyjazny dla człowieka opis zarejestrowanego dostawcy. |
| `enablePasswordRetrieval` | Określa, czy użytkownicy mogą pobierać zapomniane hasło. Wartość domyślna to `false`. |
| `enablePasswordReset` | Wskazuje, czy użytkownicy mogą resetować swoje hasła. Wartość domyślna to `true`. |
| `maxInvalidPasswordAttempts` | Maksymalna liczba nieudanych prób logowania, które mogą wystąpić dla danego użytkownika w określonym `passwordAttemptWindow` przed zablokowaniem użytkownika. Wartość domyślna to 5. |
| `minRequiredNonalphanumericCharacters` | Minimalna liczba znaków innych niż alfanumeryczne, które muszą występować w haśle użytkownika. Ta wartość musi należeć do zakresu od 0 do 128; wartość domyślna to 1. |
| `minRequiredPasswordLength` | Minimalna liczba znaków wymagana w haśle. Ta wartość musi należeć do zakresu od 0 do 128; wartość domyślna to 7. |
| `name` | Nazwa zarejestrowanego dostawcy. Ta wartość jest wymagana. |
| `passwordAttemptWindow` | Liczba minut, przez które są śledzone nieudane próby logowania. Jeśli użytkownik poda nieprawidłowe poświadczenia logowania `maxInvalidPasswordAttempts` razy w tym określonym oknie, są one zablokowane. Wartość domyślna to 10. |
| `PasswordFormat` | Format przechowywania haseł: `Clear`, `Hashed`lub `Encrypted`. Wartość domyślna to `Hashed`. |
| `passwordStrengthRegularExpression` | Jeśli ta wartość jest określona, to wyrażenie regularne służy do oszacowania siły hasła wybranego przez użytkownika podczas tworzenia nowego konta lub zmiany hasła. Wartością domyślną jest ciąg pusty. |
| `requiresQuestionAndAnswer` | Określa, czy użytkownik musi odpowiedzieć na swoje pytanie zabezpieczające podczas pobierania lub resetowania hasła. Wartość domyślna to `true`. |
| `requiresUniqueEmail` | Wskazuje, czy wszystkie konta użytkowników w danej partycji aplikacji muszą mieć unikatowy adres e-mail. Wartość domyślna to `true`. |
| `type` | Określa typ dostawcy. Ta wartość jest wymagana. |

**Tabela 2**. ustawienia konfiguracji członkostwa i `SqlMembershipProvider`

Oprócz `AspNetSqlMembershipProvider`inni dostawcy członkostwa mogą być zarejestrowani w zależności od aplikacji przez dodanie podobnego znacznika do pliku `Web.config`.

> [!NOTE]
> Struktura ról działa tak samo w ten sam sposób: w `machine.config` istnieje domyślny zarejestrowany dostawca ról, a zarejestrowany dostawca może być dostosowany w zależności od aplikacji w `Web.config`. W przyszłości będziemy szczegółowo analizować strukturę role i jej znaczniki konfiguracyjne.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Dostosowywanie ustawień`SqlMembershipProvider`

Domyślny `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) ma ustawiony atrybut `connectionStringName` na `LocalSqlServer`. Podobnie jak w przypadku dostawcy `AspNetSqlMembershipProvider` nazwy parametrów połączenia `LocalSqlServer` są zdefiniowane w `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Jak widać, te parametry połączenia definiują bazę danych SQL 2005 Express Edition znajdującą się w lokalizacji | Katalog DataDirectory | aspnetdb. mdf. Ciąg | Katalog DataDirectory | został przetłumaczony w czasie wykonywania, aby wskazywał katalog `~/App_Data/`, więc ścieżkę bazy danych | Usługi DataDirectory | aspnetdb. mdf "tłumaczy na `~/App_Data`/`aspnet.mdf`.

Jeśli nie podano żadnych informacji o dostawcy członkostwa w pliku `Web.config` aplikacji, aplikacja używa domyślnego zarejestrowanego dostawcy członkostwa, `AspNetSqlMembershipProvider`. Jeśli baza danych `~/App_Data/aspnet.mdf` nie istnieje, środowisko uruchomieniowe programu ASP.NET automatycznie utworzy ją i doda schemat usług aplikacji. Nie chcemy jednak korzystać z bazy danych `aspnet.mdf`. Zamiast tego chcemy użyć bazy danych `SecurityTutorials.mdf` utworzonej w kroku 2. Tę modyfikację można wykonać na jeden z dwóch sposobów:

- <strong>Określ wartość</strong> <strong>`LocalSqlServer`</strong> <strong>nazwy parametrów połączenia w</strong> <strong>`Web.config`</strong> <strong>.</strong> Zastępując `LocalSqlServer` wartość nazwy parametrów połączenia w `Web.config`, możemy użyć domyślnego zarejestrowanego dostawcy członkostwa (`AspNetSqlMembershipProvider`) i prawidłowo współpracować z `SecurityTutorials.mdf` bazą danych. To podejście jest odpowiednie w przypadku zawartości z ustawieniami konfiguracyjnymi określonymi przez `AspNetSqlMembershipProvider`. Aby uzyskać więcej informacji na temat tej techniki, zobacz wpis w blogu [Scott Guthrie](https://weblogs.asp.net/scottgu/), [konfigurowanie ASP.NET 2,0 Usługi aplikacji do użycia SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Dodaj nowego zarejestrowanego dostawcę typu</strong> <strong>`SqlMembershipProvider`</strong> <strong>i skonfiguruj</strong> ustawienia<strong>`connectionStringName`</strong>, <strong>aby wskazywały</strong> <strong>bazę danych</strong> <strong>`SecurityTutorials.mdf`</strong>. Takie podejście jest przydatne w scenariuszach, w których chcesz dostosować inne właściwości konfiguracji oprócz parametrów połączenia z bazą danych. We własnych projektach zawsze korzystamy z tego podejścia ze względu na jego elastyczność i czytelność.

Zanim będziemy mogli dodać nowego zarejestrowanego dostawcę, który odwołuje się do bazy danych `SecurityTutorials.mdf`, najpierw musimy dodać odpowiednią wartość parametrów połączenia w sekcji `<connectionStrings>` w `Web.config`. Poniższy znacznik dodaje nowe parametry połączenia o nazwie `SecurityTutorialsConnectionString`, które odwołują się do bazy danych SQL Server 2005 Express Edition `SecurityTutorials.mdf` w folderze `App_Data`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Jeśli używasz alternatywnego pliku bazy danych, zaktualizuj parametry połączenia zgodnie z wymaganiami. Aby uzyskać więcej informacji na temat tworzenia poprawnych parametrów połączenia, zapoznaj się z [connectionStrings.com](http://www.connectionstrings.com/).

Następnie Dodaj następujący znacznik konfiguracji członkostwa do pliku `Web.config`. Ten znacznik rejestruje nowego dostawcę o nazwie `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Oprócz rejestrowania dostawcy `SecurityTutorialsSqlMembershipProvider` powyższe znaczniki definiuje `SecurityTutorialsSqlMembershipProvider` jako domyślny dostawca (za pośrednictwem atrybutu `defaultProvider` w elemencie `<membership>`). Odwołaj, że platforma członkowska może mieć wielu zarejestrowanych dostawców. Ponieważ `AspNetSqlMembershipProvider` jest zarejestrowany jako pierwszy dostawca w `machine.config`, służy jako domyślny dostawca, chyba że poinformujemy inaczej.

Obecnie nasza aplikacja ma dwóch zarejestrowanych dostawców: `AspNetSqlMembershipProvider` i `SecurityTutorialsSqlMembershipProvider`. Jednak przed zarejestrowaniem dostawcy `SecurityTutorialsSqlMembershipProvider` mógł zostać wyczyszczony wszyscy wcześniej zarejestrowani dostawcy przez dodanie [elementu`<clear />`](https://msdn.microsoft.com/library/t062y6yc.aspx) bezpośrednio przed elementem `<add>`. Spowoduje to wyczyszczenie `AspNetSqlMembershipProvider` z listy zarejestrowanych dostawców, co oznacza, że `SecurityTutorialsSqlMembershipProvider` będzie jedyny zarejestrowany dostawca członkostwa. Jeśli korzystamy z tego podejścia, nie trzeba oznaczyć `SecurityTutorialsSqlMembershipProvider` jako domyślnego dostawcy, ponieważ będzie to jedyny zarejestrowany dostawca członkostwa. Aby uzyskać więcej informacji na temat korzystania z `<clear />`, zobacz [używanie `<clear />` podczas dodawania dostawców](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Należy zauważyć, że ustawienie `connectionStringName` `SecurityTutorialsSqlMembershipProvider`odwołuje się do dodawanej `SecurityTutorialsConnectionString` nazwy parametrów połączenia i że ustawienie `applicationName` zostało ustawione na wartość SecurityTutorials. Ponadto ustawienie `requiresUniqueEmail` zostało ustawione na `true`. Wszystkie inne opcje konfiguracji są identyczne z wartościami w `AspNetSqlMembershipProvider`. Jeśli chcesz, możesz wprowadzić dowolne modyfikacje konfiguracji w tym miejscu. Można na przykład wzmocnić siłę hasła, wymagając dwóch znaków innych niż alfanumeryczne zamiast jednej lub przez zwiększenie długości hasła do ośmiu znaków zamiast siedmiu.

> [!NOTE]
> Należy przypomnieć, że platforma członkowska pozwala na partycjonowanie pojedynczego magazynu użytkowników w wielu aplikacjach. Ustawienie `applicationName` dostawcy członkostwa wskazuje, której aplikacji używa dostawca podczas pracy z magazynem użytkowników. Ważne jest, aby jawnie ustawić wartość ustawienia konfiguracji `applicationName`, ponieważ jeśli `applicationName` nie jest jawnie ustawiona, jest przypisywana do wirtualnej ścieżki katalogu głównego aplikacji sieci Web w czasie wykonywania. Działa to tak długo, jak wirtualna ścieżka katalogu głównego aplikacji nie zmienia się, ale jeśli aplikacja zostanie przeniesiona do innej ścieżki, ustawienie `applicationName` zostanie zmienione. W takim przypadku dostawca członkostwa rozpocznie pracę z inną partycją aplikacji niż została wcześniej użyta. Konta użytkowników utworzone przed przeniesieniem będą znajdować się w innej partycji aplikacji, a użytkownicy nie będą już mogli logować się do lokacji. Aby zapoznać się z bardziej szczegółowymi dyskusjami na ten temat, zobacz [zawsze ustawiać właściwość `applicationName` podczas konfigurowania członkostwa ASP.NET 2,0 i innych dostawców](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Podsumowanie

W tym momencie mamy bazę danych ze skonfigurowanymi usługami aplikacji (`SecurityTutorials.mdf`) i skonfigurowano naszą aplikację sieci Web tak, aby platforma członkowska korzystała z zarejestrowanego dostawcy `SecurityTutorialsSqlMembershipProvider`. Ten zarejestrowany dostawca jest typu `SqlMembershipProvider` i ma `connectionStringName` ustawiony na odpowiednie parametry połączenia (`SecurityTutorialsConnectionString`), a jego wartość `applicationName` jawnie ustawiona.

Teraz jesteśmy gotowi do korzystania z platformy członkostwa z naszej aplikacji. W następnym samouczku sprawdzimy, jak utworzyć nowe konta użytkowników. Poniższe informacje pozwolą na uwierzytelnianie użytkowników, wykonywanie autoryzacji opartej na użytkownikach i przechowywanie dodatkowych informacji dotyczących użytkowników w bazie danych.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Zawsze ustawiaj Właściwość `applicationName` podczas konfigurowania członkostwa ASP.NET 2,0 i innych dostawców](https://weblogs.asp.net/scottgu/443634)
- [Konfigurowanie Usługi aplikacji ASP.NET 2,0 do użycia SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Pobierz SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Badanie członkostwa, ról i profilu ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Element `<add>` dla dostawców członkostwa](https://msdn.microsoft.com/library/whae3t94.aspx)
- [Element `<membership>`](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [Element `<providers>` dla członkostwa](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Używanie `<clear />` podczas dodawania dostawców](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Praca bezpośrednio z `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenia wideo dotyczące tematów zawartych w tym samouczku

- [Objaśnienie członkostwa platformy ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Konfigurowanie programu SQL do pracy ze schematami członkostwa](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modyfikowanie ustawień członkostwa w domyślnym schemacie członkostwa](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Alicja Maziarz. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Next](creating-user-accounts-cs.md)
