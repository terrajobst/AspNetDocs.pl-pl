---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrowanie danych dostawcy uniwersalnego dla członkostwa i profilów użytkowników doC#ASP.NET Identity () — ASP.NET 4. x
author: rustd
description: W tym samouczku opisano kroki, które są niezbędne do migracji danych użytkowników i ról oraz danych profilu użytkownika utworzonych przy użyciu dostawców uniwersalnych istniejącej aplikacji...
ms.author: riande
ms.date: 12/13/2013
ms.custom: seoapril2019
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 31f02a0cec3c531c45c37b7aad8456e01e80b5ea
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456117"
---
# <a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrowanie danych uniwersalnego dostawcy dotyczących członkostwa i profilów użytkowników do systemu ASP.NET Identity (C#)

[Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Robert McMurraya](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> W tym samouczku opisano kroki niezbędne do migracji danych użytkowników i ról oraz danych profilu użytkownika utworzonych przy użyciu uniwersalnych dostawców istniejącej aplikacji do modelu ASP.NET Identity. Podejście wymienione tutaj do migrowania danych profilu użytkownika może być używane również w aplikacji z członkostwem SQL.

W przypadku wydania Visual Studio 2013 zespół ASP.NET wprowadził nowy system ASP.NET Identity i możesz zapoznać się z innymi informacjami o tej [wersji.](../../index.md) Aby przeprowadzić migrację aplikacji sieci Web z [członkostwa SQL do nowego systemu tożsamości](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), w tym artykule przedstawiono procedurę migrowania istniejących aplikacji, które są zgodne z modelem dostawcy w celu zarządzania użytkownikami i rolami w nowym modelu tożsamości. Ten samouczek obejmuje głównie Migrowanie danych profilu użytkownika w celu bezproblemowego podłączania go do nowego systemu. Migrowanie informacji o użytkownikach i rolach jest podobne do członkostwa w programie SQL Server. Podejście do migracji danych profilu może być używane również w aplikacji z członkostwem SQL.

Na przykład zaczniemy od aplikacji sieci Web utworzonej przy użyciu programu Visual Studio 2012 korzystającej z modelu dostawcy. Następnie dodamy kod do zarządzania profilami, zarejestrujesz użytkownika, dodasz dane profilu dla użytkowników, przeprowadzisz migrację schematu bazy danych, a następnie zmienisz aplikację w taki sposób, aby korzystała z systemu tożsamości w celu zarządzania użytkownikami i rolami. W ramach testu migracji użytkownicy utworzeni przy użyciu dostawców uniwersalnych powinni mieć możliwość zalogowania się, a nowi użytkownicy powinni mieć możliwość zarejestrowania się.

> [!NOTE]
> Pełny przykład można znaleźć w [https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations).

## <a name="profile-data-migration-summary"></a>Podsumowanie migracji danych profilu

Przed rozpoczęciem migracji poinformuj nas o tym, jak przechowywać dane profilu w modelu dostawców. Dane profilowe dla użytkowników aplikacji mogą być przechowywane na wiele sposobów, najpopularniejsze między nimi przy użyciu wbudowanych dostawców profilu dostarczonych wraz z dostawcami uniwersalnymi. Kroki obejmują

1. Dodaj klasę, która ma właściwości używane do przechowywania danych profilu.
2. Dodaj klasę rozszerzającą element "ProfileBase" i implementuje metody pobierania powyższych danych profilu dla użytkownika.
3. Włącz Używanie domyślnych dostawców profilów w pliku *Web. config* i zdefiniuj klasę zadeklarowaną w kroku #2, która ma być używana podczas uzyskiwania dostępu do informacji o profilu.

Informacje o profilu są przechowywane jako serializowane dane XML i binarne w tabeli "Profile" w bazie danych.

Po przeprowadzeniu migracji aplikacji tak, aby korzystała z nowego systemu ASP.NET Identity, informacje o profilu są deserializowane i przechowywane jako właściwości klasy użytkownika. Każdą właściwość można następnie zmapować na kolumny w tabeli użytkownika. Zalety tej funkcji jest to, że właściwości mogą być wykorzystywane bezpośrednio przy użyciu klasy użytkownika, a nie muszą one być serializowane i deserializacji informacji o danych każdorazowo podczas uzyskiwania dostępu do niego.

## <a name="getting-started"></a>Wprowadzenie

1. Utwórz nową aplikację formularzy sieci Web w programie ASP.NET 4,5 w programie Visual Studio 2012. Bieżący przykład używa szablonu formularzy sieci Web, ale można również użyć aplikacji MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Utwórz nowy folder "models" do przechowywania informacji o profilu  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Załóżmy na przykład, że firma Microsoft przechowuje datę urodzenia, miasto, Wysokość i wagę użytkownika w profilu. Wysokość i waga są przechowywane jako Klasa niestandardowa o nazwie "PersonalStats". Aby zachować i pobrać profil, potrzebujemy klasy rozszerzającej "ProfileBase". Utwórzmy nową klasę "AppProfile" w celu pobierania i przechowywania informacji o profilu.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Włącz profil w pliku *Web. config* . Wprowadź nazwę klasy, która ma być używana do przechowywania/pobierania informacji o użytkowniku utworzonych w kroku #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Dodaj stronę formularzy sieci Web w folderze "konto", aby uzyskać dane profilu od użytkownika i zapisać je. Kliknij prawym przyciskiem myszy projekt i wybierz pozycję "Dodaj nowy element". Dodaj nową stronę WebForms ze stroną wzorcową "AddProfileData. aspx". Skopiuj następujące elementy w sekcji "kontrolka mainContent":

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Dodaj następujący kod w kodzie:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Dodaj przestrzeń nazw, pod którą zdefiniowano klasę AppProfile, aby usunąć błędy kompilacji.
6. Uruchom aplikację i Utwórz nowego użytkownika o nazwie username "**olduser".** Przejdź do strony "AddProfileData" i Dodaj informacje o profilu dla użytkownika.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Możesz sprawdzić, czy dane są przechowywane jako serializowany kod XML w tabeli "Profile" przy użyciu okna Eksplorator serwera. W programie Visual Studio z menu "widok" Wybierz pozycję "Eksplorator serwera". Powinno być dostępne połączenie danych dla bazy danych zdefiniowanej w pliku *Web. config* . Kliknięcie połączenia danych powoduje wyświetlenie różnych podkategorii. Rozwiń węzeł "tabele", aby wyświetlić różne tabele w bazie danych, a następnie kliknij prawym przyciskiem myszy pozycję "Profile" i wybierz polecenie "Pokaż dane tabeli", aby wyświetlić dane profilu przechowywane w tabeli profilów.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrowanie schematu bazy danych

Aby istniejąca baza danych działała z systemem tożsamości, należy zaktualizować schemat w bazie danych tożsamości, aby umożliwić obsługę pól dodanych do oryginalnej bazy danych. Można to zrobić za pomocą skryptów SQL do tworzenia nowych tabel i kopiowania istniejących informacji. W oknie "Eksplorator serwera" Rozwiń "DefaultConnection", aby wyświetlić tabele. Kliknij prawym przyciskiem myszy pozycję tabele i wybierz polecenie "nowe zapytanie"

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Wklej skrypt SQL z [https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) i uruchom go. W przypadku odświeżenia "DefaultConnection" można zobaczyć, że nowe tabele są dodawane. Możesz sprawdzić dane w tabelach, aby zobaczyć, że informacje zostały zmigrowane.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrowanie aplikacji do użycia ASP.NET Identity

1. Zainstaluj pakiety NuGet, które są zbędne dla ASP.NET Identity:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Więcej informacji na temat zarządzania pakietami NuGet można znaleźć [tutaj](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Aby można było korzystać z istniejących danych w tabeli, należy utworzyć klasy modeli, które mapują z powrotem do tabel i podłączają je do systemu tożsamości. W ramach kontraktu tożsamości klasy modelu powinny implementować interfejsy zdefiniowane w bibliotece DLL Identity. Core lub można zwiększyć istniejącą implementację tych interfejsów dostępnych w pliku Microsoft. AspNet. Identity. EntityFramework. Będziemy używać istniejących klas do roli, logowania użytkowników i oświadczeń użytkowników. Musimy użyć niestandardowego użytkownika dla naszego przykładu. Kliknij prawym przyciskiem myszy projekt i Utwórz nowy folder "IdentityModels". Dodaj nową klasę "User", jak pokazano poniżej:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Zwróć uwagę, że element "ProfileInfo" jest teraz właściwością klasy użytkownika. Dlatego możemy używać klasy użytkownika do bezpośredniego działania z danymi profilu.

Skopiuj pliki z folderów **IdentityModels** i **IdentityAccount** ze źródła pobierania ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Istnieją pozostałe klasy modelu i nowe strony, które są konieczne do zarządzania użytkownikami i rolami przy użyciu ASP.NET Identity interfejsów API. Używane podejście jest podobne do członkostwa w programie SQL i szczegółowe wyjaśnienie można znaleźć [tutaj](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Kopiowanie danych profilu do nowych tabel

Jak wspomniano wcześniej, musimy deserializować dane XML w tabelach profile i zapisać je w kolumnach tabeli AspNetUsers. Nowe kolumny zostały utworzone w tabeli użytkownicy w poprzednim kroku, tak więc wszystkie pozostałe są wypełniane kolumnami zawierającymi niezbędne dane. W tym celu użyjemy aplikacji konsolowej, która jest uruchamiana raz, aby wypełnić nowo utworzone kolumny w tabeli users.

1. Utwórz nową aplikację konsolową w rozwiązaniu kończenia.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Zainstaluj najnowszą wersję pakietu Entity Framework.
3. Dodaj aplikację sieci Web utworzoną powyżej jako odwołanie do aplikacji konsolowej. Aby to zrobić, kliknij projekt, a następnie pozycję "Dodaj odwołania", następnie rozwiązanie, a następnie kliknij projekt i kliknij przycisk OK.
4. Skopiuj poniższy kod z klasy Program.cs. Ta logika odczytuje dane profilu dla każdego użytkownika, serializować je jako obiekt "ProfileInfo" i zapisuje je z powrotem do bazy danych.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Niektóre z używanych modeli są zdefiniowane w folderze "IdentityModels" projektu aplikacji sieci Web, więc należy uwzględnić odpowiednie przestrzenie nazw.
5. Powyższy kod działa w pliku bazy danych w folderze\_danych projektu aplikacji sieci Web utworzonej w poprzednich krokach. Aby odwołać się do programu, należy zaktualizować parametry połączenia w pliku App. config aplikacji konsolowej przy użyciu parametrów połączenia w Web. config aplikacji sieci Web. Podaj także pełną ścieżkę fizyczną we właściwości "AttachDbFilename".
6. Otwórz wiersz polecenia i przejdź do folderu bin powyższej aplikacji konsolowej. Uruchom plik wykonywalny i Przejrzyj dane wyjściowe dziennika, jak pokazano na poniższej ilustracji.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Otwórz tabelę "AspNetUsers" w Eksplorator serwera i sprawdź dane w nowych kolumnach, które zawierają właściwości. Należy je zaktualizować z odpowiednimi wartościami właściwości.

## <a name="verify-functionality"></a>Weryfikuj funkcje

Użyj nowo dodanych stron członkostwa wdrożonych przy użyciu ASP.NET Identity, aby zalogować użytkownika ze starej bazy danych. Użytkownik powinien mieć możliwość zalogowania się przy użyciu tych samych poświadczeń. Wypróbuj inne funkcje, takie jak dodawanie uwierzytelniania OAuth, tworzenie nowego użytkownika, zmiana hasła, Dodawanie ról, Dodawanie użytkowników do ról itp.

Dane profilu dla starego użytkownika i nowych użytkowników powinny być pobierane i przechowywane w tabeli users. Nie można już odwoływać się do starej tabeli.

## <a name="conclusion"></a>Podsumowanie

W tym artykule opisano proces migrowania aplikacji sieci Web, które używają modelu dostawcy w celu członkostwa w ASP.NET Identity. W tym artykule opisano sposób migrowania danych profilu dla użytkowników, którzy mają być podłączani do systemu tożsamości. Poniżej znajdziesz komentarze dotyczące pytań i problemów napotkanych podczas migrowania aplikacji.

*Dziękujemy za Rick Anderson i Robert McMurraya w celu przejrzenia artykułu.*
