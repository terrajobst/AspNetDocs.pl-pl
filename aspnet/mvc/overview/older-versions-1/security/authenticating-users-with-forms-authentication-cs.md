---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Uwierzytelnianie użytkowników za pomocą uwierzytelniania formularzy (C#) | Microsoft Docs
author: microsoft
description: Dowiedz się, jak używać atrybutu [autoryzuje] do ochrony haseł na określonych stronach w aplikacji MVC. Dowiesz się, jak używać administrowania witryną sieci Web...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: bed2eafa47fec25ac04cb07e0037f596494bb7d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541511"
---
# <a name="authenticating-users-with-forms-authentication-c"></a>Uwierzytelnianie użytkowników za pomocą uwierzytelniania formularzy (C#)

przez [firmę Microsoft](https://github.com/microsoft)

> Dowiedz się, jak używać atrybutu [autoryzuje] do ochrony haseł na określonych stronach w aplikacji MVC. Dowiesz się, jak tworzyć użytkowników i role i zarządzać nimi za pomocą narzędzia do administrowania witryną sieci Web. Dowiesz się również, jak skonfigurować, gdzie są przechowywane informacje o koncie użytkownika i roli.

Celem tego samouczka jest wyjaśnienie, jak można używać uwierzytelniania formularzy do ochrony danych w widokach w aplikacjach ASP.NET MVC. Dowiesz się, jak tworzyć użytkowników i role przy użyciu narzędzia do administrowania witryną sieci Web. Dowiesz się również, jak uniemożliwić nieautoryzowanym użytkownikom Wywoływanie akcji kontrolera. Na koniec dowiesz się, jak skonfigurować, gdzie są przechowywane nazwy użytkowników i hasła.

#### <a name="using-the-web-site-administration-tool"></a>Korzystanie z narzędzia do administrowania witryną sieci Web

Przed wykonaniem jakichkolwiek innych czynności należy zacząć od utworzenia niektórych użytkowników i ról. Najprostszym sposobem tworzenia nowych użytkowników i ról jest skorzystanie z narzędzia administracyjnego witryny sieci Web programu Visual Studio 2008. Można uruchomić to narzędzie, wybierając opcję menu **projekt, ASP.NET konfigurację**. Alternatywnie możesz uruchomić narzędzie do administrowania witryną sieci Web, klikając ikonę (dość Scary) młota, która pojawia się u góry okna Eksplorator rozwiązań (patrz rysunek 1).

**Rysunek 1 — Uruchamianie narzędzia do administrowania witryną sieci Web**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

W narzędziu do administrowania witryną sieci Web można tworzyć nowych użytkowników i role, wybierając kartę Zabezpieczenia. Kliknij link **Utwórz użytkownika** , aby utworzyć nowego użytkownika o nazwie Stephen (patrz rysunek 2). Podaj użytkownikowi Stephen wszystkie żądane hasła (na przykład *klucz tajny*).

**Rysunek 2 — Tworzenie nowego użytkownika**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Aby utworzyć nowe role, należy najpierw włączyć role i zdefiniować co najmniej jedną rolę. Włącz role, klikając link **Włącz role** . Następnie Utwórz rolę o nazwie *administratorzy* , klikając link **Utwórz role lub zarządzaj nimi** (patrz rysunek 3).

**Rysunek 3 — Tworzenie nowej roli**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Na koniec Utwórz nowego użytkownika o nazwie Sally i skojarz Sally z rolą Administrator, klikając łącze Utwórz użytkownika i wybierając administratorów podczas tworzenia Sally (patrz rysunek 4).

**Rysunek 4 — Dodawanie użytkownika do roli**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Gdy wszystko jest wymienione i gotowe, powinny mieć dwóch nowych użytkowników o nazwie Stephen i Sally. Należy również mieć nową rolę o nazwie Administratorzy. Sally jest członkiem roli Administratorzy i Stephen nie jest.

#### <a name="requiring-authorization"></a>Wymaganie autoryzacji

Możesz wymagać uwierzytelnienia użytkownika, zanim użytkownik wywoła akcję kontrolera przez dodanie atrybutu [autoryzuje] do akcji. Można zastosować atrybut [autoryzuje] do poszczególnych akcji kontrolera lub zastosować ten atrybut do całej klasy kontrolera.

Na przykład kontroler na liście 1 uwidacznia akcję o nazwie CompanySecrets (). Ponieważ ta akcja jest uzupełniona atrybutem [autoryzuje], tej akcji nie można wywołać, chyba że użytkownik zostanie uwierzytelniony.

**Lista 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Jeśli wywołasz akcję CompanySecrets (), wprowadzając adres URL/Home/CompanySecrets na pasku adresu przeglądarki i nie jesteś użytkownikiem uwierzytelnionym, nastąpi automatyczne przekierowanie do widoku logowania (patrz rysunek 5).

**Rysunek 5 — widok logowania**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Możesz użyć widoku logowania, aby wprowadzić nazwę użytkownika i hasło. Jeśli nie jesteś zarejestrowanym użytkownikiem, możesz kliknąć link **zarejestruj** , aby przejść do widoku Rejestr (patrz rysunek 6). Możesz użyć widoku rejestr, aby utworzyć nowe konto użytkownika.

**Rysunek 6 — widok rejestr**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Po pomyślnym zalogowaniu można zobaczyć widok CompanySecrets (patrz rysunek 7). Domyślnie będziesz nadal logować się do momentu zamknięcia okna przeglądarki.

**Rysunek 7 — Widok CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autoryzacja według nazwy użytkownika lub roli użytkownika

Można użyć atrybutu [autoryzuje], aby ograniczyć dostęp do akcji kontrolera do określonego zestawu użytkowników lub określonego zestawu ról użytkownika. Na przykład zmodyfikowany kontroler macierzysty na liście 2 zawiera dwie nowe akcje o nazwie StephenSecrets () i AdministratorSecrets ().

**Lista 2 — Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Tylko użytkownik mający nazwę użytkownika Stephen może wywoływać akcję StephenSecrets (). Wszyscy inni użytkownicy zostaną przekierowani do widoku logowania. Właściwość Users akceptuje rozdzieloną przecinkami listę nazw kont użytkowników.

Tylko użytkownicy w roli Administratorzy mogą wywoływać akcję AdministratorSecrets (). Na przykład ponieważ Sally jest członkiem grupy Administratorzy, może wywołać akcję AdministratorSecrets (). Wszyscy inni użytkownicy zostaną przekierowani do widoku logowania. Właściwość Role akceptuje rozdzieloną przecinkami listę nazw ról.

#### <a name="configuring-authentication"></a>Konfigurowanie uwierzytelniania

W tym momencie możesz zastanawiać się, gdzie są przechowywane informacje o koncie użytkownika i roli. Domyślnie informacje są przechowywane w bazie danych programu SQL Express (RANU) o nazwie ASPNETDB. mdf znajdującej się w folderze danych aplikacji MVC\_. Ta baza danych jest generowana automatycznie przez platformę ASP.NET po rozpoczęciu korzystania z członkostwa.

Aby wyświetlić bazę danych ASPNETDB. mdf w oknie Eksplorator rozwiązań, należy najpierw wybrać opcję menu Projekt, wyświetlić wszystkie pliki.

Tworzenie aplikacji przy użyciu domyślnej bazy danych SQL Express jest bardzo precyzyjne. Jednak najprawdopodobniej nie chcesz używać domyślnej bazy danych ASPNETDB. mdf dla aplikacji produkcyjnej. W takim przypadku można zmienić miejsce przechowywania informacji o koncie użytkownika, wykonując następujące dwa kroki:

1. Dodawanie Usługi aplikacji obiektów bazy danych do produkcyjnej bazy danych — Zmień parametry połączenia aplikacji tak, aby wskazywały produkcyjną bazę danych

Pierwszym krokiem jest dodanie wszystkich niezbędnych obiektów bazy danych (tabel i procedur składowanych) do produkcyjnej bazy danych programu. Najprostszym sposobem dodawania tych obiektów do nowej bazy danych jest skorzystanie z Kreatora instalacji SQL Server ASP.NET (zobacz rysunek 8). Można uruchomić to narzędzie, otwierając wiersz polecenia programu Visual Studio 2008 z grupy programu Microsoft Visual Studio 2008 i wykonując następujące polecenie w wierszu polecenia:

ASPNET\_regsql

**Rysunek 8 — Kreator instalacji SQL Server ASP.NET**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

Kreator instalacji SQL Server ASP.NET umożliwia wybranie SQL Serverj bazy danych w sieci i zainstalowanie wszystkich obiektów bazy danych wymaganych przez usługi aplikacji ASP.NET. Serwer bazy danych nie musi znajdować się na komputerze lokalnym.

> [!NOTE] 
> 
> Jeśli nie chcesz używać Kreatora instalacji SQL Server ASP.NET, możesz znaleźć skrypty SQL do dodawania obiektów bazy danych usług aplikacji w następującym folderze:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727

Po utworzeniu niezbędnych obiektów bazy danych należy zmodyfikować połączenie z bazą danych używane przez aplikację MVC. Zmodyfikuj parametry połączenia ApplicationServices w pliku konfiguracji sieci Web (Web. config) tak, aby wskazywał produkcyjną bazę danych. Na przykład zmodyfikowane połączenie w liście 3 wskazuje na bazę danych o nazwie MyProductionDB (oryginalne parametry połączenia ApplicationServices zostały oznaczone jako komentarz).

**Lista 3 – Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Konfigurowanie uprawnień bazy danych

W przypadku korzystania ze zintegrowanych zabezpieczeń do łączenia się z bazą danych należy dodać poprawne konto użytkownika systemu Windows jako nazwę logowania do bazy danych. Poprawne konto jest zależne od tego, czy jest używany serwer ASP.NET Development czy Internet Information Services jako serwer sieci Web. Poprawne konto użytkownika zależy również od używanego systemu operacyjnego.

Jeśli używasz serwera ASP.NET Development (domyślny serwer sieci Web używany przez program Visual Studio), aplikacja jest wykonywana w kontekście konta użytkownika systemu Windows. W takim przypadku należy dodać konto użytkownika systemu Windows jako nazwę logowania serwera bazy danych.

Alternatywnie, jeśli używasz Internet Information Services, musisz dodać konto ASPNET lub konto usługi sieciowej/urzędu NT jako nazwę logowania serwera bazy danych. Jeśli używasz systemu Windows XP, Dodaj konto ASPNET jako nazwę logowania do bazy danych. Jeśli używasz nowszego systemu operacyjnego, takiego jak Windows Vista lub Windows Server 2008, Dodaj konto urząd NT/usługa sieciowa jako identyfikator logowania bazy danych.

Nowe konto użytkownika można dodać do bazy danych za pomocą Management Studio Microsoft SQL Server (zobacz rysunek 9).

**Rysunek 9 — Tworzenie nowego Microsoft SQL Server logowania**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Po utworzeniu wymaganej nazwy logowania należy zamapować dane logowania do użytkownika bazy danych z właściwymi rolami bazy danych. Kliknij dwukrotnie nazwę logowania i wybierz kartę mapowanie użytkownika. Wybierz co najmniej jedną rolę bazy danych usług aplikacji. Na przykład w celu uwierzytelniania użytkowników należy włączyć przynależność\_ASPNET\_rolę bazy danych BasicAccess. Aby utworzyć nowych użytkowników, należy włączyć przynależność\_ASPNET\_rolę bazy danych FullAccess (Zobacz Rysunek 10).

**Rysunek 10 — Dodawanie Usługi aplikacji ról bazy danych**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób korzystania z uwierzytelniania formularzy podczas kompilowania aplikacji ASP.NET MVC. Najpierw przedstawiono sposób tworzenia nowych użytkowników i ról dzięki wykorzystaniu narzędzia do administrowania witryną sieci Web. Następnie dowiesz się, jak używać atrybutu [autoryzuje], aby uniemożliwić nieautoryzowanym użytkownikom Wywoływanie akcji kontrolera. Na koniec wiesz już, jak skonfigurować aplikację MVC do przechowywania informacji o użytkownikach i rolach w produkcyjnej bazie danych.

> [!div class="step-by-step"]
> [Dalej](authenticating-users-with-windows-authentication-cs.md)
