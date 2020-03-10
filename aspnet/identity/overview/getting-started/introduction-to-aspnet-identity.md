---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Wprowadzenie do ASP.NET Identity ASP.NET 4. x
author: jongalloway
description: System członkostwa ASP.NET został wprowadzony z ASP.NET 2,0 z powrotem w 2005, a następnie wprowadzono wiele zmian w sposób typowy dla aplikacji sieci Web...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0268dfc16cd2cfb1e79ee14997a4c5eb247af950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583847"
---
# <a name="introduction-to-aspnet-identity"></a>Wprowadzenie do systemu ASP.NET Identity

> System członkostwa ASP.NET został wprowadzony z ASP.NET 2,0 z powrotem w 2005, a następnie wprowadzono wiele zmian w sposób, w jaki aplikacje sieci Web zwykle obsługują uwierzytelnianie i autoryzację. ASP.NET Identity to świeży wygląd systemu członkostwa, który powinien być używany podczas tworzenia nowoczesnych aplikacji dla sieci Web, telefonu lub tabletu.

## <a name="background-membership-in-aspnet"></a>Tło: członkostwo w ASP.NET

### <a name="aspnet-membership"></a>Członkostwo platformy ASP.NET

[Członkostwo ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) zostało zaprojektowane do rozwiązywania wymagań dotyczących członkostwa w lokacji, które były wspólne w 2005, które dotyczyły uwierzytelniania formularzy oraz SQL Server bazy danych dla nazw użytkowników, haseł i danych profilu. Dzisiaj istnieje dużo większa tablica opcji przechowywania danych dla aplikacji sieci Web, a większość deweloperów chce umożliwić swoim lokacjom korzystanie z dostawców tożsamości społecznościowych na potrzeby funkcji uwierzytelniania i autoryzacji. Ograniczenia projektu członkostwa ASP.NET sprawiają, że to przejście jest trudne:

- Schemat bazy danych został zaprojektowany dla SQL Server i nie można go zmienić. Możesz dodać informacje o profilu, ale dodatkowe dane są pakowane do innej tabeli, co utrudnia dostęp do nich za pomocą jakichkolwiek metod, z wyjątkiem interfejsu API dostawcy profilów.
- System dostawcy umożliwia zmianę magazynu danych zapasowych, ale system został zaprojektowany wokół założeń, które są odpowiednie dla relacyjnej bazy danych. Można napisać dostawcę, aby przechowywać informacje o członkostwie w mechanizmie nierelacyjnego magazynu, takim jak tabele usługi Azure Storage, ale należy obejść projekt relacyjny, pisząc wiele kodu i wiele `System.NotImplementedException` wyjątków dla metod, które nie mają zastosowania do baz danych NoSQL.
- Ponieważ funkcja logowania/wylogowywania jest oparta na uwierzytelnianiu formularzy, system członkostwa nie może używać [Owin](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN obejmuje składniki oprogramowania pośredniczącego do uwierzytelniania, w tym obsługę logowania przy użyciu zewnętrznych dostawców tożsamości (takich jak konta Microsoft, Facebook, Google, Twitter) i logowania przy użyciu kont organizacji z Active Directory lokalnego lub Azure Active Directory. OWIN obejmuje również obsługę protokołu OAuth 2,0, JWT i CORS.

### <a name="aspnet-simple-membership"></a>ASP.NET proste członkostwo

[ASP.NET Simple Membership](../../../web-pages/overview/security/16-adding-security-and-membership.md) został opracowany jako system członkostwa dla stron sieci Web ASP.NET. Został on opublikowany w programie WebMatrix i programie Visual Studio 2010 z dodatkiem SP1. Celem prostego członkostwa było ułatwienie dodawania funkcji członkostwa do aplikacji Web Pages.

Proste członkowstwo ułatwia dostosowanie informacji o profilach użytkowników, ale nadal udostępnia inne problemy z członkostwem ASP.NET i ma pewne ograniczenia:

- Trudno było utrwalać dane systemu członkostwa w magazynie nierelacyjnym.
- Nie można używać go z OWIN.
- Nie działa dobrze z istniejącymi dostawcami członkostwa ASP.NET i nie jest rozszerzalny.

### <a name="aspnet-universal-providers"></a>Standardowi dostawcy ASP.NET

[ASP.NET uniwersalnych dostawców](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) opracowano, aby umożliwić utrwalanie informacji o członkostwie w Microsoft Azure SQL Database, a także współpracują z SQL Server Compact. Dostawcy Uniwersalni byli w Entity Framework Code First, co oznacza, że uniwersalnych dostawców można używać do utrwalania danych w magazynie obsługiwanym przez EF. W przypadku uniwersalnych dostawców schemat bazy danych został również oczyszczony.

Dostawcy Uniwersalni są Wbudowani w infrastrukturę członkostwa ASP.NET, więc nadal mają takie same ograniczenia jak w przypadku dostawcy sqlmemberd. Oznacza to, że zostały zaprojektowane dla relacyjnych baz danych i trudno jest dostosować profil i informacje o użytkowniku. Ci dostawcy nadal używają również uwierzytelniania formularzy do logowania i wylogowania.

## <a name="aspnet-identity"></a>ASP.NET Identity

W miarę rozwoju historii członkostwa w ASP.NET przez lata zespół ASP.NET zauczył opinię od klientów.

Założenie, że użytkownicy będą logować się, wprowadzając nazwę użytkownika i hasło, które zostały zarejestrowane we własnej aplikacji, nie są już prawidłowe. Sieć Web staje się bardziej społecznościowa. Użytkownicy pracują ze sobą w czasie rzeczywistym za pomocą kanałów społecznościowych, takich jak Facebook, Twitter i inne witryny sieci Web społecznościowych. Deweloperzy chcą, aby użytkownicy mogli logować się przy użyciu tożsamości społecznościowych, aby mogli mieć bogate środowisko w swoich witrynach sieci Web. Nowoczesne systemy członkostwa muszą włączać logowania oparte na przekierowaniu do dostawców uwierzytelniania, takich jak Facebook, Twitter i inne.

W miarę rozwoju sieci Web, tak jakby były wzorcem rozwoju sieci Web. Testy jednostkowe kodu aplikacji stały się podstawą dla deweloperów aplikacji. W 2008 ASP.NET dodano nową strukturę opartą na wzorcu Model-View-Controller (MVC), w części ułatwiającej deweloperom tworzenie aplikacji weryfikowalne ASP.NET w zespole. Deweloperzy, którzy chcą przetestować jednostkę logiki aplikacji, powinni również mieć możliwość wykonania tej czynności w systemie członkostwa.

Uwzględniając te zmiany w programowaniu aplikacji sieci Web, ASP.NET Identity został opracowany z następującymi celami:

- **Jeden ASP.NET Identity system**

    - ASP.NET Identity można używać ze wszystkimi platformami ASP.NET, takimi jak ASP.NET MVC, formularze sieci Web, strony sieci Web, internetowy interfejs API i sygnalizujący.
    - ASP.NET Identity można używać podczas tworzenia aplikacji sieci Web, telefonów, sklepu lub hybrydowej.
- **Łatwość podłączania w danych profilu dotyczących użytkownika**

    - Masz kontrolę nad schematem informacji o użytkownikach i profilach. Na przykład można łatwo umożliwić systemowi przechowywanie dat urodzenia wprowadzonych przez użytkowników po zarejestrowaniu konta w aplikacji.

- **Kontrola trwałości**

    - Domyślnie system ASP.NET Identity przechowuje wszystkie informacje o użytkowniku w bazie danych. ASP.NET Identity używa Entity Framework Code First do wdrożenia całego mechanizmu trwałości.
    - Ze względu na to, że kontrolujesz schemat bazy danych, typowe zadania, takie jak zmiana nazw tabel lub zmiana typu danych kluczy podstawowych, są proste do wykonania.
    - W łatwy sposób można podłączyć różne mechanizmy magazynu, takie jak SharePoint, usługa tabel usługi Azure Storage, bazy danych NoSQL itp., bez konieczności zgłaszania wyjątków `System.NotImplementedExceptions`.
- **Testowanie jednostkowe**

    - ASP.NET Identity sprawia, że aplikacja sieci Web weryfikowalne więcej jednostek. Można napisać testy jednostkowe dla części aplikacji, które używają ASP.NET Identity.
- **Dostawca roli**

    - Istnieje dostawca roli, który umożliwia ograniczenie dostępu do części aplikacji przez role. Możesz łatwo tworzyć role, takie jak "Administrator" i dodawać użytkowników do ról.
- **Oparte na oświadczeniach**

    - ASP.NET Identity obsługuje uwierzytelnianie oparte na oświadczeniach, gdzie tożsamość użytkownika jest reprezentowana jako zestaw oświadczeń. Oświadczenia umożliwiają deweloperom bardziej dokładniejsze określenie tożsamości użytkownika niż Zezwalanie na role. Członkostwo roli jest tylko wartością logiczną (składową lub niebędącą członkiem), ale może zawierać bogate informacje o tożsamości i członkostwie użytkownika.
- **Dostawcy logowania społecznościowego**

    - W aplikacji można łatwo dodawać usługi logowania do społeczności, takie jak konta Microsoft, Facebook, Twitter, Google i inne, a także przechowywać dane specyficzne dla użytkownika w aplikacji.

- **Integracja OWIN**

    - Uwierzytelnianie ASP.NET jest teraz oparte na oprogramowaniu pośredniczącym OWIN, które może być używane na dowolnym hoście opartym na OWIN. ASP.NET Identity nie ma żadnej zależności od elementu System. Web. Jest to w pełni zgodne środowisko OWIN i może być używane w dowolnej aplikacji hostowanej OWIN.
    - ASP.NET Identity używa uwierzytelniania OWIN do logowania/wylogowywania użytkowników w witrynie sieci Web. Oznacza to, że zamiast używać FormsAuthentication do wygenerowania pliku cookie, aplikacja używa OWIN CookieAuthentication w tym celu.
- **Pakiet NuGet**

    - ASP.NET Identity jest rozpowszechniana jako pakiet NuGet, który jest instalowany w szablonach ASP.NET MVC, Web Forms i Web API, które są dostarczane z programem Visual Studio 2017. Pakiet NuGet można pobrać z galerii NuGet.
    - Zwalnianie ASP.NET Identity jako pakietu NuGet ułatwia zespołowi ASP.NET Iterowanie nowych funkcji i poprawek błędów oraz dostarczanie ich do deweloperów w sposób Agile.

## <a name="get-started-with-aspnet-identity"></a>Wprowadzenie do ASP.NET Identity

ASP.NET Identity jest używany w szablonach projektów programu Visual Studio 2017 dla ASP.NET MVC, formularzy sieci Web, internetowego interfejsu API i SPA. W tym instruktażu przedstawiono sposób, w jaki szablony projektów używają ASP.NET Identity do dodawania funkcji do rejestrowania, logowania i wylogowywania użytkownika.

ASP.NET Identity jest implementowana przy użyciu poniższej procedury. Celem tego artykułu jest zapewnienie ogólnego omówienia ASP.NET Identity; Możesz wykonać instrukcje krok po kroku lub po prostu przeczytać szczegóły. Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji przy użyciu ASP.NET Identity, w tym przy użyciu nowego interfejsu API do dodawania użytkowników, ról i informacji o profilu, zobacz sekcję następne kroki na końcu tego artykułu.

1. Utwórz aplikację ASP.NET MVC z poszczególnymi kontami. ASP.NET Identity można użyć w ASP.NET MVC, formularzach sieci Web, interfejsie API sieci Web, sygnale itp. W tym artykule zaczniemy od aplikacji ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Utworzony projekt zawiera następujące trzy pakiety dla ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Ten pakiet ma Entity Framework implementację ASP.NET Identity, która będzie utrzymywać ASP.NET Identity dane i schemat do SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Ten pakiet ma podstawowe interfejsy dla ASP.NET Identity. Ten pakiet może służyć do pisania implementacji dla ASP.NET Identity, które są przeznaczone dla różnych magazynów trwałości, takich jak Azure Table Storage, bazy danych NoSQL itp.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Ten pakiet zawiera funkcje, które są używane do wtyczki uwierzytelniania OWIN przy użyciu ASP.NET Identity w aplikacjach ASP.NET. Jest on używany podczas dodawania funkcji logowania do aplikacji i wywoływać do programu OWINego uwierzytelnianie plików cookie w celu wygenerowania pliku cookie.
3. Tworzenie użytkownika.  
   Uruchom aplikację, a następnie kliknij link **zarejestruj** , aby utworzyć użytkownika. Na poniższej ilustracji przedstawiono stronę rejestracji, która gromadzi informacje o nazwie użytkownika i haśle.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Gdy użytkownik wybierze przycisk **zarejestruj** , Akcja `Register` kontrolera konta tworzy użytkownika przez wywołanie interfejsu API ASP.NET Identity, jak pokazano poniżej:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Rejestrowanie.  
   Jeśli użytkownik został pomyślnie utworzony, jest zalogowany przez metodę `SignInAsync`.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample6.cs?highlight=12)]

   Metoda `SignInManager.SignInAsync` generuje [Identyfikator oświadczenia](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Ponieważ ASP.NET Identity i OWIN uwierzytelnianie plików cookie to system oparty na oświadczeniach, struktura wymaga, aby aplikacja generowała identyfikator oświadczenia dla użytkownika. Identyfikator oświadczenia zawiera informacje o wszystkich oświadczeniach dla użytkownika, takich jak role, do których należy użytkownik.   
 
5. Wyloguj się.  
   Wybierz łącze **Wyloguj się, aby** wywołać akcję wylogowania w kontrolerze konta. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   W wyróżnionym powyżej kodzie przedstawiono metodę OWIN `AuthenticationManager.SignOut`. Jest to analogiczne do metody [FormsAuthentication. wylogowaniu](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) używanej przez moduł [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) w formularzach sieci Web.

## <a name="components-of-aspnet-identity"></a>Składniki ASP.NET Identity

Na poniższym diagramie przedstawiono składniki systemu ASP.NET Identity ( [Wybierz na diagramie, aby](introduction-to-aspnet-identity/_static/image3.png) go powiększyć). Pakiety w kolorze zielonym składają się na system ASP.NET Identity. Wszystkie inne pakiety są zależnościami, które są konieczne do korzystania z systemu ASP.NET Identity w aplikacjach ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Poniżej znajduje się krótki opis pakietów NuGet, które nie zostały wymienione wcześniej:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Oprogramowanie pośredniczące, które umożliwia aplikacji używanie uwierzytelniania opartego na plikach cookie, podobnie jak w przypadku środowiska ASP. Uwierzytelnianie formularzy netto.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework to technologia dostępu do danych dla relacyjnych baz danych firmy Microsoft.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrowanie z członkostwa do ASP.NET Identity

Mamy nadzieję, że wkrótce udostępnimy wskazówki dotyczące migrowania istniejących aplikacji, które korzystają z członkostwa ASP.NET lub prostego członkostwa w nowym systemie ASP.NET Identity.

## <a name="next-steps"></a>Następne kroki

- [Tworzenie aplikacji ASP.NET MVC 5 za pomocą usługi Facebook i usług Google OAuth2 i OpenID Connect](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Samouczek używa interfejsu API ASP.NET Identity, aby dodać informacje o profilu do bazy danych użytkownika oraz jak przeprowadzić uwierzytelnianie za pomocą usługi Google i serwisu Facebook.
- [Utwórz aplikację ASP.NET MVC z uwierzytelnianiem i bazą danych SQL, a następnie wdróż ją w Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 W tym samouczku pokazano, jak dodać użytkowników i role przy użyciu interfejsu API tożsamości.
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Przykładowa aplikacja, która pokazuje, jak dodawać podstawowe role i obsługę użytkowników oraz jak wykonywać role i zarządzać użytkownikami.
