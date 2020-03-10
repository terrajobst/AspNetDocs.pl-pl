---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementowanie niestandardowego dostawcy magazynu MySQL ASP.NET Identity — ASP.NET 4. x
author: raquelsa
description: ASP.NET Identity to rozszerzalny system, który umożliwia utworzenie własnego dostawcy magazynu i podłączenie go do aplikacji bez ponowienia pracy z likacji...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 2f0b47d45bce82c71d1864536309f9e2ffed2d63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616915"
---
# <a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementowanie niestandardowego dostawcy magazynu MySQL dla systemu ASP.NET Identity

[Raquel Soares de Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tomasz FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity to rozszerzalny system, który umożliwia utworzenie własnego dostawcy magazynu i podłączenie go do aplikacji bez konieczności ponownego pracy aplikacji. W tym temacie opisano sposób tworzenia dostawcy magazynu MySQL dla ASP.NET Identity. Aby zapoznać się z omówieniem tworzenia niestandardowych dostawców magazynu, zobacz [Omówienie niestandardowych dostawców magazynu dla ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Aby ukończyć ten samouczek, musisz mieć Visual Studio 2013 z aktualizacją Update 2.
> 
> Ten samouczek zawiera następujące instrukcje:
> 
> - Pokaż, jak utworzyć wystąpienie bazy danych MySQL na platformie Azure.
> - Pokaż, jak używać narzędzia klienckiego MySQL (MySQL Workbench) do tworzenia tabel i zarządzania zdalną bazą danych na platformie Azure.
> - Pokaż, jak zastąpić domyślną implementację magazynu ASP.NET Identity przy użyciu implementacji niestandardowej w projekcie aplikacji MVC.
> 
> Ten samouczek został pierwotnie zaprojektowany przez Raquel Soares de Almeida i Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ). Przykładowy projekt został zaktualizowany dla tożsamości 2,0 przez Suhas Joshi. Temat został zaktualizowany dla tożsamości 2,0 przez Tomasz FitzMacken.

## <a name="download-completed-project"></a>Pobierz ukończony projekt

Na końcu tego samouczka będziesz mieć projekt aplikacji MVC z ASP.NET Identity pracy z bazą danych MySQL hostowaną na platformie Azure.

Kompletnego dostawcę magazynu MySQL można pobrać pod adresem [ASPNET. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL).

## <a name="the-steps-you-will-perform"></a>Kroki, które należy wykonać

W tym samouczku wykonasz następujące instrukcje:

1. Tworzenie bazy danych MySQL na platformie Azure
2. Tworzenie tabel ASP.NET Identity w programie MySQL
3. Tworzenie aplikacji MVC i Konfigurowanie jej do korzystania z dostawcy MySQL
4. Uruchamianie aplikacji

Ten temat nie obejmuje architektury ASP.NET Identity i decyzji, które należy wykonać podczas wdrażania dostawcy magazynu klienta. Aby uzyskać te informacje, zobacz [Omówienie niestandardowych dostawców magazynu dla ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Przegląd klas dostawcy magazynu MySQL

Przed przejściem do procedury tworzenia dostawcy magazynu MySQL Przyjrzyjmy się klasom, które tworzą dostawcę magazynu. Potrzebne są klasy, które zarządzają operacjami bazy danych i klasami, które są wywoływane z aplikacji w celu zarządzania użytkownikami i rolami.

### <a name="storage-classes"></a>Klasy magazynu

- [IdentityUser](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs) — zawiera właściwości dla użytkownika.
- [UserStore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs) — zawiera operacje umożliwiające dodawanie, aktualizowanie lub pobieranie użytkowników.
- [IdentityRole](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs) — zawiera właściwości dla ról.
- [RoleStore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) — zawiera operacje umożliwiające dodawanie, usuwanie, aktualizowanie i pobieranie ról.

### <a name="data-access-layer-classes"></a>Klasy warstwy dostępu do danych

Na potrzeby tego przykładu klasy warstwy dostępu do danych zawierają instrukcje SQL do pracy z tabelami; Jednak w kodzie możesz chcieć użyć mapowania obiektowo-relacyjnego (ORM), takiego jak Entity Framework lub NHibernate. W szczególności aplikacja może mieć niską wydajność bez użycia ORM, która obejmuje ładowanie z opóźnieniem i buforowanie obiektów. Aby uzyskać więcej informacji, zobacz [ASP.NET Identity 2,0 bez Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) — zawiera połączenie z bazą danych MySQL oraz metody wykonywania operacji bazy danych. Zarówno UserStore, jak i RoleStore są tworzone przy użyciu wystąpienia tej klasy.
- [Role](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) -zawiera operacje bazy danych dla tabeli, w której są przechowywane role.
- [UserClaimsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje oświadczenia użytkownika.
- [UserLoginsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) — zawiera operacje bazy danych dla tabeli przechowującej informacje logowania użytkownika.
- [UserRoleTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) — zawiera operacje bazy danych dla tabeli przechowującej użytkowników, którym są przypisani role.
- [Userion](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) -zawiera operacje bazy danych dla tabeli, w której są przechowywane użytkownicy.

## <a name="create-a-mysql-database-instance-on-azure"></a>Tworzenie wystąpienia bazy danych MySQL na platformie Azure

1. Zaloguj się do [Azure Portal](https://manage.windowsazure.com/).
2. Kliknij pozycję **+ Nowy** w dolnej części strony, a następnie wybierz pozycję **Zapisz**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. W kreatorze **Wybierz i dodatek** wybierz pozycję **baza danych MySQL ClearDB** , a następnie kliknij strzałkę Dalej w prawym dolnym rogu okna dialogowego.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Zachowaj domyślny plan **bezpłatny** i Zmień **nazwę** na **IdentityMySQLDatabase**. Wybierz region znajdujący się najbliżej siebie, a następnie kliknij strzałkę Dalej.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Kliknij znacznik wyboru, aby zakończyć tworzenie bazy danych.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Po utworzeniu bazy danych można nią zarządzać z poziomu karty **Dodatki** w portalu zarządzania.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Aby uzyskać informacje o połączeniu z bazą danych, kliknij pozycję **Informacje o połączeniu** w dolnej części strony.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Skopiuj parametry połączenia, klikając przycisk Kopiuj i zapisując go, aby można było użyć go później w aplikacji MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Tworzenie tabel ASP.NET Identity w bazie danych MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Instalowanie narzędzia MySQL Workbench do łączenia bazy danych MySQL i zarządzania nią

1. Zainstaluj narzędzie **MySQL Workbench** na [stronie pobierania programu MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Uruchom aplikację i kliknij przycisk **MySQLConnections +** , aby dodać nowe połączenie. Użyj danych parametrów połączenia skopiowanych z bazy danych Azure MySQL utworzonej wcześniej w tym samouczku.
3. Po ustanowieniu połączenia Otwórz nową kartę **zapytania** ; Wklej polecenia z [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) do zapytania i wykonaj je w celu utworzenia tabel bazy danych.
4. Masz teraz wszystkie ASP.NET Identity niezbędne tabele utworzone w bazie danych MySQL hostowanej na platformie Azure, jak pokazano poniżej.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Utwórz projekt aplikacji MVC na podstawie szablonu i skonfiguruj go do korzystania z dostawcy MySQL

W razie potrzeby zainstaluj [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) z aktualizacją Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-github"></a>Pobierz projekt ASP. NET. Identity. MySQL z usługi GitHub

1. Przejdź do adresu URL repozytorium w polu [ASPNET. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL/).
2. Pobierz kod źródłowy.
3. Wyodrębnij plik zip do folderu lokalnego.
4. Otwórz rozwiązanie AspNet. Identity. MySQL i skompiluj je.

### <a name="create-a-new-mvc-application-project-from-template"></a>Utwórz nowy projekt aplikacji MVC na podstawie szablonu

1. Kliknij prawym przyciskiem myszy rozwiązanie **ASPNET. Identity. MySQL** i **Dodaj** **Nowy projekt**
2. W oknie dialogowym **Dodaj nowy projekt** wybierz **pozycję C# Wizualizacja** po lewej stronie, a następnie kliknij pozycję **Sieć Web** , a następnie wybierz opcję **aplikacja sieci Web ASP.NET**. Nazwij projekt **IdentityMySQLDemo**; a następnie kliknij przycisk OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon MVC z opcjami domyślnymi (zawierającymi **indywidualne konta użytkowników** jako metoda uwierzytelniania), a następnie kliknij przycisk **OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt IdentityMySQLDemo i wybierz pozycję **Zarządzaj pakietami NuGet**. W oknie dialogowym Wyszukiwanie pola tekstowego wpisz **Identity. EntityFramework**. Wybierz ten pakiet z listy wyników, a następnie kliknij przycisk **Odinstaluj**. Zostanie wyświetlony monit o odinstalowanie pakietu zależności EntityFramework. Kliknij przycisk tak, ponieważ ten pakiet nie zostanie już w tej aplikacji.
5. Kliknij prawym przyciskiem myszy projekt IdentityMySQLDemo, wybierz pozycję **Dodaj**, **odwołanie, rozwiązanie, projekty,** wybierz projekt ASPNET. Identity. MySQL, a następnie kliknij przycisk **OK**.
6. W projekcie IdentityMySQLDemo Zastąp wszystkie odwołania do  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   elementem  
     `using AspNet.Identity.MySQL;`
7. W IdentityModels.cs Ustaw **ApplicationDbContext** na pochodny od **MySqlDatabase** i Uwzględnij konstruktora, który przyjmuje jeden parametr z nazwą połączenia.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Otwórz plik IdentityConfig.cs. W metodzie **ApplicationUserManager. Create** należy zastąpić Tworzenie wystąpienia elementu usermanager następującym kodem:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Otwórz plik Web. config i Zastąp ciąg DefaultConnection tym wpisem, zastępując wyróżnione wartości parametrami połączenia bazy danych MySQL utworzoną w poprzednich krokach:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Uruchamianie aplikacji i nawiązywanie połączenia z bazą danych MySQL

1. Kliknij prawym przyciskiem myszy projekt **IdentityMySQLDemo** i wybierz pozycję **Ustaw jako projekt startowy** .
2. Naciśnij **kombinację klawiszy CTRL + F5** , aby skompilować i uruchomić aplikację.
3. Kliknij kartę **zarejestruj** w górnej części strony.
4. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij pozycję **zarejestruj**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Nowy użytkownik jest teraz zarejestrowany i zalogowany.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Wróć do narzędzia MySQL Workbench i sprawdź zawartość tabeli **IdentityMySQLDatabase** . Sprawdź, czy w tabeli users wprowadzono wpisy podczas rejestrowania nowych użytkowników.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat włączania innych metod uwierzytelniania w tej aplikacji, zobacz [Tworzenie aplikacji ASP.NET MVC 5 z logowaniem w serwisie Facebook oraz Google OAuth2 i OpenID Connect](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Aby dowiedzieć się, jak zintegrować bazę danych z uwierzytelnianiem OAuth i skonfigurować role umożliwiające ograniczenie dostępu użytkowników do aplikacji, zobacz [wdrażanie aplikacji Secure ASP.NET MVC 5 z członkostwem, uwierzytelnianiem OAuth i SQL Database na platformie Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
