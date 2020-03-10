---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Autoryzacja oparta na rolachC#() | Microsoft Docs
author: rick-anderson
description: Ten samouczek rozpoczyna się od sposobu, w jaki struktura role kojarzy role użytkownika z jego kontekstem zabezpieczeń. Następnie sprawdzimy, jak zastosować adres URL oparty na rolach...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 46153ab310bdee814baaa53c372fb92f8a23ce11
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627730"
---
# <a name="role-based-authorization-c"></a>Autoryzacja oparta na rolach (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Ten samouczek rozpoczyna się od sposobu, w jaki struktura role kojarzy role użytkownika z jego kontekstem zabezpieczeń. Następnie analizuje sposób zastosowania reguł autoryzacji adresów URL opartych na rolach. Poniżej opisano, jak korzystać z deklaratywnych i programistycznych metod zmieniania wyświetlanych danych oraz funkcji oferowanych przez stronę ASP.NET.

## <a name="introduction"></a>Wprowadzenie

<a id="_msoanchor_1"> </a>W samouczku dotyczącym [*autoryzacji opartej na użytkownikach*](../membership/user-based-authorization-cs.md) pokazano, jak używać autoryzacji adresów URL, aby określić, którzy użytkownicy mogą odwiedzać określony zestaw stron. Z zaledwie małą ilością znaczników w `Web.config`można wydać ASP.NET, aby zezwolić tylko uwierzytelnionym użytkownikom na odwiedzenie strony. Możemy też określić, że dozwolone są tylko użytkownicy Tito i Robert, lub wskazać, że wszyscy uwierzytelnieni użytkownicy z wyjątkiem tego są dozwoleni.

Oprócz autoryzacji adresów URL, oferujemy również metody deklaracyjne i programistyczne służące do kontrolowania wyświetlanych danych oraz funkcji oferowanych przez stronę na podstawie odwiedzanych przez użytkownika. W szczególności utworzyliśmy stronę, która znajduje się na liście zawartości bieżącego katalogu. Każda osoba może odwiedzić Tę stronę, ale tylko uwierzytelnieni użytkownicy mogą wyświetlać zawartość plików i tylko Tito mogą usuwać pliki.

Stosowanie reguł autoryzacji dla poszczególnych użytkowników może wzrosnąć do księgowości okropnej. Bardziej łatwość utrzymania Metoda polega na użyciu autoryzacji opartej na rolach. Dobra wiadomość polega na tym, że narzędzia na naszym odniesieniu do stosowania reguł autoryzacji działają równie dobrze z rolami, jak w przypadku kont użytkowników. Reguły autoryzacji adresów URL mogą określać role zamiast użytkowników. Formant widoku logowania, który renderuje różne dane wyjściowe dla uwierzytelnionych i anonimowych użytkowników, można skonfigurować do wyświetlania różnej zawartości na podstawie ról zalogowanego użytkownika. Interfejs API ról zawiera metody określania ról zalogowanego użytkownika.

Ten samouczek rozpoczyna się od sposobu, w jaki struktura role kojarzy role użytkownika z jego kontekstem zabezpieczeń. Następnie analizuje sposób zastosowania reguł autoryzacji adresów URL opartych na rolach. Poniżej opisano, jak korzystać z deklaratywnych i programistycznych metod zmieniania wyświetlanych danych oraz funkcji oferowanych przez stronę ASP.NET. Zacznijmy!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Zrozumienie, w jaki sposób role są skojarzone z kontekstem zabezpieczeń użytkownika

Za każdym razem, gdy żądanie przejdzie do potoku ASP.NET, jest ono skojarzone z kontekstem zabezpieczeń, który obejmuje informacje identyfikujące zleceniodawcę. W przypadku korzystania z uwierzytelniania formularzy bilet uwierzytelniania jest używany jako token tożsamości. <a id="_msoanchor_2"> </a>Zgodnie z opisem w temacie Omówienie konfiguracji [*uwierzytelniania formularzy*](../introduction/an-overview-of-forms-authentication-cs.md) i <a id="_msoanchor_3"> </a> [*uwierzytelniania formularzy oraz*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) samouczków dotyczących tematów zaawansowanych, `FormsAuthenticationModule` jest odpowiedzialny za określenie tożsamości osoby żądającej, którą wykonuje podczas [`AuthenticateRequest` zdarzenia](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Jeśli zostanie znaleziony prawidłowy, niewygasły bilet uwierzytelniania, `FormsAuthenticationModule` dekoduje go, aby upewnić się, że tożsamość żądającego. Tworzy nowy obiekt `GenericPrincipal` i przypisuje go do obiektu `HttpContext.User`. Celem podmiotu zabezpieczeń, takiego jak `GenericPrincipal`, jest zidentyfikowanie nazwy uwierzytelnionego użytkownika i ról, do których należą. Ten cel jest oczywisty przez fakt, że wszystkie obiekty główne mają właściwość `Identity` i metodę `IsInRole(roleName)`. Niemniej jednak `FormsAuthenticationModule`nie interesuje się rejestrowanie informacji o rolach, a tworzony obiekt `GenericPrincipal` nie określa żadnych ról.

Jeśli platforma ról jest włączona, moduł [`RoleManagerModule`](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) http jest uruchamiany po `FormsAuthenticationModule` i identyfikuje role uwierzytelnionego użytkownika podczas [zdarzenia`PostAuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), które zostanie wyzwolone po zdarzeniu `AuthenticateRequest`. Jeśli żądanie pochodzi od uwierzytelnionego użytkownika, `RoleManagerModule` nadpisać `GenericPrincipal` obiektu utworzonego przez `FormsAuthenticationModule` i zastępuje go [obiektem`RolePrincipal`](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). Klasa `RolePrincipal` używa interfejsu API role do określenia ról, do których należy użytkownik.

Rysunek 1 przedstawia przepływ pracy potoku ASP.NET w przypadku korzystania z uwierzytelniania formularzy i struktury ról. `FormsAuthenticationModule` wykonuje pierwsze, identyfikuje użytkownika za pośrednictwem biletu uwierzytelniania i tworzy nowy obiekt `GenericPrincipal`. Następnie `RoleManagerModule` kroków w i zastępuje obiekt `GenericPrincipal` z obiektem `RolePrincipal`.

Jeśli użytkownik anonimowy odwiedzi tę lokację, nie `FormsAuthenticationModule` ani `RoleManagerModule` tworzy obiekt Principal.

[![zdarzenia potoku ASP.NET dla uwierzytelnionego użytkownika podczas korzystania z uwierzytelniania formularzy i struktury ról](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Rysunek 1**: zdarzenia potoku ASP.NET dla uwierzytelnionego użytkownika podczas korzystania z uwierzytelniania formularzy i struktury ról ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-cs/_static/image3.png))

### <a name="caching-role-information-in-a-cookie"></a>Buforowanie informacji o roli w pliku cookie

Metoda `IsInRole(roleName)` `RolePrincipal` obiektu wywołuje `Roles.GetRolesForUser`, aby uzyskać role dla użytkownika w celu ustalenia, czy użytkownik jest członkiem *roli role*. W przypadku korzystania z `SqlRoleProvider`powoduje to zapytanie do bazy danych magazynu ról. W przypadku używania reguł autoryzacji adresów URL opartych na rolach Metoda `IsInRole` `RolePrincipal`zostanie wywołana dla każdego żądania do strony chronionej przez reguły autoryzacji adresów URL opartych na rolach. Zamiast konieczności wyszukiwania informacji o roli w bazie danych na każdym żądaniu, struktura ról zawiera opcję buforowania ról użytkownika w pliku cookie.

Jeśli struktura ról jest skonfigurowana do buforowania ról użytkownika w pliku cookie, `RoleManagerModule` tworzy plik cookie podczas [zdarzenia`EndRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)potoku ASP.NET. Ten plik cookie jest używany w kolejnych żądaniach w `PostAuthenticateRequest`, który jest tworzony podczas tworzenia `RolePrincipal` obiektu. Jeśli plik cookie jest prawidłowy i nie wygasł, dane w pliku cookie są analizowane i używane do wypełniania ról użytkownika, co umożliwia zapisanie `RolePrincipal` z konieczności wywołania klasy `Roles` w celu określenia ról użytkownika. Rysunek 2 przedstawia ten przepływ pracy.

[![informacje o roli użytkownika mogą być przechowywane w pliku cookie w celu zwiększenia wydajności](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Rysunek 2**. Informacje o roli użytkownika mogą być przechowywane w pliku cookie w celu zwiększenia wydajności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-cs/_static/image6.png))

Domyślnie mechanizm plików cookie pamięci podręcznej roli jest wyłączony. Można ją włączyć za pomocą `<roleManager>` znaczników konfiguracji w `Web.config`. Omówiono użycie [elementu`<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx) , aby określić dostawców ról w <a id="_msoanchor_4"> </a>samouczku [*Tworzenie ról i zarządzanie nimi*](creating-and-managing-roles-cs.md) , dzięki czemu ten element powinien już znajdować się w pliku `Web.config` aplikacji. Ustawienia plików cookie pamięci podręcznej roli są określone jako atrybuty elementu `<roleManager>` i są podsumowywane w tabeli 1.

> [!NOTE]
> Ustawienia konfiguracji wymienione w tabeli 1 określają właściwości plików cookie w pamięci podręcznej roli. Aby uzyskać więcej informacji na temat plików cookie, sposobu ich działania i ich różnych właściwości, Przeczytaj [Samouczek dotyczący plików cookie](http://www.quirksmode.org/js/cookies.html).

| <strong>Właściwość</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Opis</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Wartość logiczna wskazująca, czy jest używane buforowanie plików cookie. Wartość domyślna to `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Nazwa pliku cookie pamięci podręcznej roli. Wartość domyślna to ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Ścieżka do pliku cookie nazwy ról. Atrybut path umożliwia deweloperowi ograniczenie zakresu pliku cookie do określonej hierarchii katalogów. Wartość domyślna to "/", która informuje przeglądarkę, aby wysłała plik cookie biletu uwierzytelniania do dowolnego żądania wysłanego do domeny.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Wskazuje, jakie techniki są używane do ochrony plików cookie w pamięci podręcznej roli. Dozwolone wartości to: `All` (wartość domyślna); `Encryption`; `None`; i `Validation`. Aby uzyskać więcej informacji na temat tych <a id="_anchor_5"> </a>poziomów ochrony, zapoznaj się z artykułem krok 3 w [*sekcji konfiguracja uwierzytelniania formularzy i Tematy zaawansowane*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) .                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Wartość logiczna wskazująca, czy do przesyłania pliku cookie uwierzytelniania jest wymagane połączenie SSL. Wartością domyślną jest `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Wartość logiczna wskazująca, czy limit czasu pliku cookie jest resetowany za każdym razem, gdy użytkownik odwiedzi lokację podczas jednej sesji. Wartością domyślną jest `false`. Ta wartość jest przydatna tylko wtedy, gdy `createPersistentCookie` jest ustawiona na `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Określa czas (w minutach), po upływie którego wygasa plik cookie biletu uwierzytelniania. Wartością domyślną jest `30`. Ta wartość jest przydatna tylko wtedy, gdy `createPersistentCookie` jest ustawiona na `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Wartość logiczna określająca, czy plik cookie roli pamięci podręcznej to plik cookie sesji, czy trwały plik cookie. Jeśli `false` (wartość domyślna), używany jest plik cookie sesji, który jest usuwany po zamknięciu przeglądarki. W przypadku `true`jest używany trwały plik cookie; wygaśnie `cookieTimeout` liczbę minut od momentu jego utworzenia lub po poprzedniej wizycie, w zależności od wartości `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Określa wartość domeny pliku cookie. Wartość domyślna to pusty ciąg, który powoduje, że przeglądarka używa domeny, z której został wystawiony (na przykład www.yourdomain.com). W takim przypadku plik cookie <strong>nie</strong> będzie wysyłany podczas przesyłania żądań do poddomen, takich jak admin.yourdomain.com. Jeśli chcesz, aby plik cookie został przesłany do wszystkich poddomen, musisz dostosować atrybut `domain`, ustawiając go na "yourdomain.com".                                                                                                                                                 |
|    `maxCachedResults`     | Określa maksymalną liczbę nazw ról, które są buforowane w pliku cookie. Wartość domyślna to 25. `RoleManagerModule` nie tworzy pliku cookie dla użytkowników, którzy należą do więcej niż `maxCachedResults` ról. W związku z tym Metoda `IsInRole` obiektu `RolePrincipal` będzie używać klasy `Roles` do określenia ról użytkownika. Przyczyna `maxCachedResults` istnieje, ponieważ wielu agentów użytkowników nie zezwala na pliki cookie większe niż 4 096 bajtów. Ten limit ma na celu zmniejszenie prawdopodobieństwa przekroczenia tego ograniczenia rozmiaru. Jeśli masz bardzo długie nazwy ról, warto rozważyć określenie mniejszej wartości `maxCachedResults`. contrariwise, jeśli masz bardzo krótkie nazwy ról, możesz prawdopodobnie zwiększyć tę wartość. |

**Tabela 1:** Opcje konfiguracji plików cookie pamięci podręcznej roli

Skonfigurujmy aplikację do używania plików cookie nietrwałych ról pamięci podręcznej. Aby to osiągnąć, zaktualizuj element `<roleManager>` w `Web.config` w celu uwzględnienia następujących atrybutów związanych z plikiem cookie:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

Zaktualizowano element `<roleManager>` przez dodanie trzech atrybutów: `cacheRolesInCookie`, `createPersistentCookie`i `cookieProtection`. Po ustawieniu `cacheRolesInCookie` na `true`, `RoleManagerModule` będzie teraz automatycznie buforować role użytkownika w pliku cookie zamiast konieczności wyszukiwania informacji o roli użytkownika w każdym żądaniu. Jawnie ustawiam atrybuty `createPersistentCookie` i `cookieProtection` na odpowiednio `false` i `All`. Technicznie nie muszę określać wartości tych atrybutów, ponieważ zostały one przypisane do ich wartości domyślnych, ale umieszczam je w tym miejscu, aby wyraźnie wyczyocić, że nie używam trwałych plików cookie i że plik cookie jest zaszyfrowany i sprawdzony.

To wszystko! Odtąd struktura ról będzie buforować role użytkowników w plikach cookie. Jeśli przeglądarka użytkownika nie obsługuje plików cookie lub jeśli pliki cookie są usuwane lub tracone, to w jakiś sposób nie jest to Wielka okazja — obiekt `RolePrincipal` będzie po prostu używać klasy `Roles` w przypadku, gdy nie jest dostępny żaden plik cookie (lub nieprawidłowy lub wygasły jeden).

> [!NOTE]
> &amp; grupy praktyk firmy Microsoft odradzają korzystanie z plików cookie z trwałych ról pamięci podręcznej. Ponieważ posiadanie plików cookie w pamięci podręcznej roli jest wystarczające do udowodnienia członkostwa w roli, Jeśli haker może w jakiś sposób uzyskać dostęp do pliku cookie prawidłowego użytkownika, może personifikować tego użytkownika. Prawdopodobieństwo wystąpienia tego problemu zwiększa się, jeśli plik cookie zostanie utrwalony w przeglądarce użytkownika. Aby uzyskać więcej informacji na temat tego zalecenia dotyczącego zabezpieczeń, a także innych zagadnień związanych z zabezpieczeniami, zapoznaj się z [listą pytań zabezpieczających dla ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx).

## <a name="step-1-defining-role-based-url-authorization-rules"></a>Krok 1. Definiowanie reguł autoryzacji adresów URL opartych na rolach

Zgodnie z opisem w <a id="_msoanchor_6"> </a>samouczku [*autoryzacji opartego na użytkownikach*](../membership/user-based-authorization-cs.md) , Autoryzacja adresów URL oferuje sposób ograniczenia dostępu do zestawu stron na zasadzie użytkownika lub roli. Reguły autoryzacji adresów URL są pisane w `Web.config` przy użyciu [elementu`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) z elementami podrzędnymi `<allow>` i `<deny>`. Oprócz reguł autoryzacji związanych z użytkownikiem omówionych w poprzednich samouczkach, każdy `<allow>` i `<deny>` elementu podrzędnego mogą również obejmować:

- Określona rola
- Rozdzielana przecinkami lista ról

Na przykład reguły autoryzacji adresów URL udzielą dostępu do tych użytkowników w rolach Administratorzy i kierownicy, ale odmówią dostępu innym osobom:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

Element `<allow>` w powyższych znacznikach stwierdza, że role Administratorzy i kierownicy są dozwolone; element `<deny>` instruuje, że *Wszyscy* użytkownicy są odrzucani.

Skonfigurujmy naszą aplikację, tak aby strony `ManageRoles.aspx`, `UsersAndRoles.aspx`i `CreateUserWizardWithRoles.aspx` były dostępne tylko dla tych użytkowników w roli Administratorzy, podczas gdy strona `RoleBasedAuthorization.aspx` pozostaje dostępna dla wszystkich odwiedzających.

Aby to osiągnąć, Zacznij od dodania pliku `Web.config` do folderu `Roles`.

[![dodać pliku Web. config do katalogu ról](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Rysunek 3**. dodawanie pliku `Web.config` do katalogu `Roles` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image9.png))

Następnie Dodaj następujący znacznik konfiguracji do `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

Element `<authorization>` w sekcji `<system.web>` wskazuje, że tylko użytkownicy w roli Administratorzy mogą uzyskać dostęp do zasobów ASP.NET w katalogu `Roles`. Element `<location>` definiuje alternatywny zestaw reguł autoryzacji adresów URL dla strony `RoleBasedAuthorization.aspx`, co pozwala wszystkim użytkownikom na odwiedzenie strony.

Po zapisaniu zmian w `Web.config`Zaloguj się jako użytkownik, który nie należy do roli Administratorzy, a następnie spróbuj odwiedzić jedną z chronionych stron. `UrlAuthorizationModule` wykryje, że nie masz uprawnień do odwiedzania żądanego zasobu. w związku z tym `FormsAuthenticationModule` przekieruje Cię do strony logowania. Na stronie logowania zostanie następnie przekierowany do strony `UnauthorizedAccess.aspx` (zobacz rysunek 4). To ostateczne przekierowanie ze strony logowania do `UnauthorizedAccess.aspx` występuje z powodu kodu dodanego do strony logowania w kroku 2 <a id="_msoanchor_7"> </a>samouczka dotyczącego [*autoryzacji opartej na użytkownikach*](../membership/user-based-authorization-cs.md) . W szczególności strona logowania automatycznie przekierowuje dowolnego uwierzytelnionego użytkownika do `UnauthorizedAccess.aspx`, jeśli Kolekcja QueryString zawiera parametr `ReturnUrl`, ponieważ ten parametr wskazuje, że użytkownik dotarł do strony logowania po podjęciu próby wyświetlenia strony, której nie Autoryzowano do wyświetlenia.

[![tylko użytkownicy w roli Administratorzy mogą wyświetlać chronione strony](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Ilustracja 4**. Wyświetlanie chronionych stron tylko dla użytkowników z roli Administratorzy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image12.png))

Wyloguj się, a następnie zaloguj się jako użytkownik należący do roli Administratorzy. Teraz powinno być możliwe wyświetlenie trzech chronionych stron.

[![Tito może odwiedzić stronę UsersAndRoles. aspx, ponieważ należy ona do roli administratorów](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Rysunek 5**: Tito może odwiedzić stronę `UsersAndRoles.aspx`, ponieważ znajduje się ona w roli Administratorzy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image15.png))

> [!NOTE]
> Podczas określania reguł autoryzacji adresów URL — dla ról lub użytkowników należy pamiętać, że reguły są analizowane pojedynczo od góry. Po znalezieniu dopasowania użytkownikowi otrzymuje lub odmówiono dostępu, w zależności od tego, czy dopasowanie zostało znalezione w `<allow>` lub `<deny>` elementu. **Jeśli nie zostanie znalezione żadne dopasowanie, użytkownikowi zostanie udzielony dostęp.** W związku z tym, jeśli chcesz ograniczyć dostęp do jednego lub większej liczby kont użytkowników, konieczne jest użycie elementu `<deny>` jako ostatniego elementu w konfiguracji autoryzacji adresu URL. **Jeśli reguły autoryzacji adresów URL nie zawierają** **elementu`<deny>`, wszyscy użytkownicy otrzymają dostęp.** Dokładniejsze omówienie sposobu analizowania reguł autoryzacji adresów URL można znaleźć w sekcji "`UrlAuthorizationModule` Omówienie reguł autoryzacji w celu udzielenia lub odmowy dostępu" w samouczku dotyczącego <a id="_msoanchor_8"> </a> [*autoryzacji opartej na użytkownikach*](../membership/user-based-authorization-cs.md) .

## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Krok 2. Ograniczanie funkcjonalności na podstawie aktualnie zalogowanych ról użytkownika

Autoryzacja adresów URL ułatwia określenie bardzo szczegółowych reguł autoryzacji, które określają, jakie tożsamości są dozwolone i które nie mogą wyświetlać określonej strony (lub wszystkie strony w folderze i jego podfolderach). Jednak w niektórych przypadkach firma Microsoft może chcieć zezwolić wszystkim użytkownikom na odwiedzenie strony, ale ograniczyć funkcjonalność strony na podstawie ról odwiedzanego użytkownika. Może to wiązać się z pokazywaniem lub ukrywaniem danych na podstawie roli użytkownika lub oferowaniu dodatkowych funkcji użytkownikom należącym do danej roli.

Takie szczegółowe reguły autoryzacji oparte na rolach mogą być implementowane w sposób deklaratywny lub programistyczny (lub poprzez kilka kombinacji dwóch). W następnej sekcji zobaczymy, jak zaimplementować deklaratywną autoryzację za pośrednictwem formantu widoku logowania. Po tym będziemy eksplorować techniki programistyczne. Zanim będziemy mogli zapoznać się z zastosowaniem reguł autoryzacji szczegółowych, należy najpierw utworzyć stronę, której funkcjonalność zależy od roli użytkownika.

Utwórzmy stronę, która wyświetla listę wszystkich kont użytkowników w systemie w widoku GridView. Widok GridView będzie zawierać nazwę użytkownika, adres e-mail, datę ostatniego logowania oraz komentarze dotyczące użytkownika. Oprócz wyświetlania informacji o poszczególnych użytkownikach GridView będzie zawierać możliwości edytowania i usuwania. Najpierw utworzymy Tę stronę z funkcją Edytuj i Usuń dostępną dla wszystkich użytkowników. W sekcjach "Using the widoku logowania Control" i "programowe Ograniczanie funkcjonalności" zobaczymy, jak włączyć lub wyłączyć te funkcje w oparciu o rolę odwiedzającego użytkownika.

> [!NOTE]
> ASP.NET Strona, którą zamierzamy skompilować, używa kontrolki GridView, aby wyświetlić konta użytkowników. Ponieważ ta seria samouczków koncentruje się na uwierzytelnianiu formularzy, autoryzacji, kontach użytkowników i rolach, nie chcę poświęcać zbyt dużo czasu na omawianie wewnętrznych czynności kontrolki GridView. Ten samouczek zawiera szczegółowe instrukcje krok po kroku dotyczące konfigurowania tej strony, dlatego nie są w stanie odszukać szczegółowych informacji o tym, dlaczego zostały wykonane pewne wybory, lub jakie konkretne właściwości mają wpływ na renderowane dane wyjściowe. Dokładne badanie kontrolki GridView można znaleźć *[w sekcji Praca z danymi w](../../data-access/index.md)* serii samouczków ASP.NET 2,0.

Zacznij od otwarcia strony `RoleBasedAuthorization.aspx` w folderze `Roles`. Przeciągnij widok GridView ze strony na projektanta i ustaw jego `ID`, aby `UserGrid`. W chwili będziemy napisać kod, który wywoła metodę `Membership.GetAllUsers` i powiąże obiekt `MembershipUserCollection`go z elementem GridView. `MembershipUserCollection` zawiera obiekt `MembershipUser` dla każdego konta użytkownika w systemie. `MembershipUser` obiekty mają właściwości, takie jak `UserName`, `Email`, `LastLoginDate`i tak dalej.

Przed zapisaniem kodu, który wiąże konta użytkowników z siatką, należy najpierw zdefiniować pola GridView. W tagu inteligentnym GridView kliknij link "Edytuj kolumny", aby uruchomić okno dialogowe pola (zobacz rysunek 6). W tym miejscu Usuń zaznaczenie pola wyboru "automatycznie Generuj pola" w lewym dolnym rogu. Ponieważ chcemy, aby ten widok GridView obejmował możliwości edytowania i usuwania, Dodaj CommandField i ustaw jego `ShowEditButton` i `ShowDeleteButton` właściwości na wartość true. Następnie Dodaj cztery pola do wyświetlania właściwości `UserName`, `Email`, `LastLoginDate`i `Comment`. Użyj BoundField dla dwóch właściwości tylko do odczytu (`UserName` i `LastLoginDate`) i używanie TemplateField dla dwóch pól edytowalnych (`Email` i `Comment`).

W pierwszej BoundField jest wyświetlana właściwość `UserName`; Ustaw `HeaderText` i `DataField` właściwości na "UserName". Nie będzie można edytować tego pola, więc ustaw dla jego właściwości `ReadOnly` wartość true. Skonfiguruj `LastLoginDate` BoundField przez ustawienie jego `HeaderText` na "ostatniego logowania" i jego `DataField` na "LastLoginDate". Sformatujmy dane wyjściowe tego BoundField, tak aby była wyświetlana tylko Data (zamiast daty i godziny). Aby to osiągnąć, ustaw właściwość `HtmlEncode` tej BoundField na wartość false, a jej Właściwość `DataFormatString` na "{0:d}". Ustaw również właściwość `ReadOnly` na wartość true.

Ustaw właściwości `HeaderText` dwóch używanie TemplateField na "E-mail" i "komentarz".

[![pola GridView można skonfigurować za pomocą okna dialogowego pola](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Ilustracja 6**. pola GridView można skonfigurować za pomocą okna dialogowego pola ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image18.png))

Teraz należy zdefiniować `ItemTemplate` i `EditItemTemplate` dla używanie TemplateField "E-mail" i "komentarza". Dodaj kontrolkę sieci Web etykieta do każdego `ItemTemplate` s i powiąż odpowiednio właściwości `Text` z właściwościami `Email` i `Comment`.

Dla TemplateField "E-mail" Dodaj pole tekstowe o nazwie `Email` do jego `EditItemTemplate` i powiąż jego właściwość `Text` z właściwością `Email` przy użyciu wielokierunkowego wiązania z danymi. Dodaj RequiredFieldValidator i RegularExpressionValidator do `EditItemTemplate`, aby upewnić się, że użytkownik edytujący Właściwość poczty E-mail wprowadził prawidłowy adres e-mail. W przypadku TemplateField "Comment" Dodaj do `EditItemTemplate`wielowierszowego pola tekstowego o nazwie `Comment`. Ustaw odpowiednio `Columns` i `Rows` pola tekstowego na 40 i 4, a następnie powiąż jego właściwość `Text` z właściwością `Comment` przy użyciu wielokierunkowego wiązania z danymi.

Po skonfigurowaniu tych używanie TemplateField, ich deklaratywne znaczniki powinny wyglądać podobnie do następujących:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Podczas edytowania lub usuwania konta użytkownika należy wiedzieć, że wartość właściwości `UserName` użytkownika. Ustaw właściwość `DataKeyNames` GridView na "UserName", aby te informacje były dostępne za pomocą kolekcji `DataKeys` GridView.

Na koniec Dodaj do strony kontrolkę podsumowania walidacji i ustaw jej Właściwość `ShowMessageBox` na true, a jej Właściwość `ShowSummary` na wartość false. Przy użyciu tych ustawień podsumowania walidacji będzie wyświetlał alert po stronie klienta, jeśli użytkownik spróbuje edytować konto użytkownika z brakującym lub nieprawidłowym adresem e-mail.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Ta strona została ukończona. Następnym zadaniem jest powiązanie zestawu kont użytkowników z GridView. Dodaj metodę o nazwie `BindUserGrid` do klasy z kodem powiązanej ze stroną `RoleBasedAuthorization.aspx`, która wiąże `MembershipUserCollection` zwrócone przez `Membership.GetAllUsers` do `UserGrid` GridView. Wywołaj tę metodę z programu obsługi zdarzeń `Page_Load` na pierwszej stronie odwiedzanie.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Korzystając z tego kodu, odwiedź stronę za pomocą przeglądarki. Jak pokazano na rysunku nr 7, powinien zostać wyświetlony widok GridView zawierający informacje o każdym koncie użytkownika w systemie.

[![GridView UserGrid wyświetla informacje o każdym użytkowniku w systemie](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Rysunek 7**: `UserGrid` GridView wyświetla informacje o każdym użytkowniku w systemie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image21.png))

> [!NOTE]
> `UserGrid` GridView wyświetla listę wszystkich użytkowników w interfejsie bez stronicowania. Ten prosty interfejs siatki nie jest odpowiedni dla scenariuszy, w których istnieje kilka dziesiątych lub większej liczby użytkowników. Jedną z opcji jest skonfigurowanie widoku GridView w celu włączenia stronicowania. Metoda `Membership.GetAllUsers` ma dwa przeciążenia: jeden, który nie akceptuje parametrów wejściowych i zwraca wszystkich użytkowników, który przyjmuje wartości całkowite dla indeksu strony i rozmiaru strony, i zwraca tylko określony podzestaw użytkowników. Drugie Przeciążenie może służyć do bardziej wydajnej strony użytkowników, ponieważ zwraca tylko dokładny podzestaw kont użytkowników, a nie *wszystkie* z nich. Jeśli masz tysiące kont użytkowników, warto wziąć pod uwagę interfejs oparty na filtrze, taki, który pokazuje tylko tych użytkowników, których nazwa użytkownika rozpoczyna się od wybranego znaku, na przykład. [`Membership.FindUsersByName method`](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) doskonale nadaje się do tworzenia interfejsu użytkownika opartego na filtrze. Przyjrzyjmy się tworzeniu takiego interfejsu w przyszłym samouczku.

Kontrolka GridView oferuje wbudowaną edycję i usuwanie obsługi, gdy formant jest powiązany z prawidłowo skonfigurowaną kontrolką źródła danych, taką jak kontrolki SqlDataSource lub ObjectDataSource. Jednak w `UserGrid` GridView dane są powiązane programowo; w związku z tym należy napisać kod, aby wykonać te dwa zadania. W szczególności konieczne będzie utworzenie obsługi zdarzeń dla zdarzeń `RowEditing`, `RowCancelingEdit`, `RowUpdating`i `RowDeleting`, które są wywoływane, gdy osoba odwiedzająca kliknie przycisk Edytuj, Anuluj, zaktualizuj lub Usuń.

Zacznij od utworzenia programów obsługi zdarzeń `RowEditing`, `RowCancelingEdit`i `RowUpdating` dla zdarzeń, a następnie Dodaj następujący kod:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

Obsługa zdarzeń `RowEditing` i `RowCancelingEdit` po prostu ustawi właściwość `EditIndex` GridView, a następnie ponownie powiązać listę kont użytkowników z siatką. Interesujące rzeczy odbywają się w obsłudze zdarzeń `RowUpdating`. Ten program obsługi zdarzeń uruchamia się, upewniając się, że dane są prawidłowe, a następnie pobiera wartość `UserName` edytowanego konta użytkownika z kolekcji `DataKeys`. Pola tekstowe `Email` i `Comment` w dwóch używanie templatefieldach "`EditItemTemplate` s" są następnie programowo przywoływane. Ich `Text` właściwości zawierają edytowane adresy e-mail i komentarze.

Aby zaktualizować konto użytkownika za pośrednictwem interfejsu API członkostwa, musimy najpierw uzyskać informacje o użytkowniku, które możemy wykonać za pośrednictwem wywołania `Membership.GetUser(userName)`. `Email` i `Comment` Właściwości zwróconego obiektu `MembershipUser` zostaną zaktualizowane wartościami wprowadzonymi do dwóch pól tekstowych w interfejsie edycji. Na koniec te modyfikacje są zapisywane z wywołaniem do [`Membership.UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). Procedura obsługi zdarzeń `RowUpdating` zakończona przez przywrócenie widoku GridView do jego interfejsu sprzed edycji.

Następnie Utwórz procedurę obsługi zdarzeń `RowDeleting`, a następnie Dodaj następujący kod:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

Powyższa procedura obsługi zdarzeń uruchamia się, pobierając wartość `UserName` z kolekcji `DataKeys` GridView; Ta `UserName` wartość jest następnie przenoszona do [metody`DeleteUser`](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)klasy członkostwa. Metoda `DeleteUser` usuwa konto użytkownika z systemu, w tym powiązane z nim dane członkostwa (na przykład role, do których należy ten użytkownik). Po usunięciu użytkownika `EditIndex` siatki ma ustawioną wartość-1 (w przypadku kliknięcia przycisku Usuń, gdy inny wiersz był w trybie edycji) i wywoływana jest metoda `BindUserGrid`.

> [!NOTE]
> Po usunięciu konta użytkownika przycisk Usuń nie wymaga żadnych potwierdzeń użytkownika. Zachęcam do dodania formy potwierdzenia użytkownika w celu zmniejszenia prawdopodobieństwa przypadkowego usunięcia konta. Jednym z najprostszych sposobów potwierdzania akcji jest użycie okna dialogowego potwierdzenia po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Dodawanie potwierdzenia po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

Sprawdź, czy ta strona działa zgodnie z oczekiwaniami. Powinno być możliwe edytowanie adresu e-mail i komentarza użytkownika, a także usuwanie dowolnego konta użytkownika. Ponieważ strona `RoleBasedAuthorization.aspx` jest dostępna dla wszystkich użytkowników, wszyscy użytkownicy — nawet anonimowi Goście — mogą odwiedzać tę stronę oraz edytować i usuwać konta użytkowników. Zaktualizujmy Tę stronę, tak aby tylko użytkownicy z rolą kierownicy i Administratorzy mogli edytować adres e-mail i komentarz użytkownika, a tylko Administratorzy mogą usunąć konto użytkownika.

Sekcja "Using the widoku logowania Control" przegląda przy użyciu kontrolki widoku logowania, aby wyświetlić instrukcje specyficzne dla roli użytkownika. Jeśli osoba z roli Administratorzy odwiedzi tę stronę, zobaczymy instrukcje dotyczące sposobu edytowania i usuwania użytkowników. Jeśli użytkownik w roli kierownicy osiągnie Tę stronę, zobaczymy instrukcje dotyczące edytowania użytkowników. Jeśli osoba odwiedzająca jest anonimowa lub nie znajduje się w roli osoby nadzorujące lub Administratorzy, zostanie wyświetlony komunikat z informacją, że nie można edytować ani usunąć informacji o koncie użytkownika. W sekcji "programowe Ograniczanie funkcjonalności" będziemy pisać kod, który programowo pokazuje lub ukrywa przyciski Edytuj i Usuń w zależności od roli użytkownika.

### <a name="using-the-loginview-control"></a>Korzystanie z formantu widoku logowania

Jak widać w poprzednich samouczkach, formant widoku logowania jest przydatny do wyświetlania różnych interfejsów dla użytkowników uwierzytelnionych i anonimowych, ale kontrolka widoku logowania może być również używana do wyświetlania różnych znaczników na podstawie ról użytkownika. Użyjmy formantu widoku logowania, aby wyświetlić różne instrukcje w oparciu o rolę odwiedzającego użytkownika.

Zacznij od dodania elementu widoku logowania powyżej `UserGrid` GridView. Jak wspomniano wcześniej, formant widoku logowania ma dwa wbudowane szablony: `AnonymousTemplate` i `LoggedInTemplate`. Wprowadź krótki komunikat w obu tych szablonach, który informuje użytkownika, że nie można edytować ani usunąć informacji o użytkowniku.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Oprócz `AnonymousTemplate` i `LoggedInTemplate`, formant widoku logowania może zawierać *RoleGroups*, które są szablonami specyficznymi dla ról. Każdy element role zawiera jedną właściwość, `Roles`, która określa role, do których odnosi się dana rola. Właściwość `Roles` można ustawić na jedną rolę (na przykład "Administratorzy") lub na listę ról (na przykład "Administratorzy, opiekunowie").

Aby zarządzać RoleGroups, kliknij link "Edytuj RoleGroups" z tagu inteligentnego kontrolki, aby wyświetlić Edytor kolekcji ról. Dodaj dwa nowe RoleGroups. Ustaw właściwość `Roles` pierwszej roli na "Administratorzy", a druga na "opiekunów".

[![Zarządzanie szablonami specyficznymi dla roli widoku logowania za pomocą edytora kolekcji ról](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Ilustracja 8**. Zarządzanie szablonami specyficznymi dla ról widoku logowania za pomocą edytora kolekcji ról ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image24.png))

Kliknij przycisk OK, aby zamknąć Edytor kolekcji ról; spowoduje to zaktualizowanie deklaratywnego znacznika widoku logowania w celu uwzględnienia sekcji `<RoleGroups>` z `<asp:RoleGroup>` elementem podrzędnym dla każdej roli zdefiniowanej w edytorze kolekcji ról. Ponadto lista rozwijana "widoki" w tagu inteligentnym programu widoku logowania, która początkowo została umieszczona tylko `AnonymousTemplate` i `LoggedInTemplate` — zawiera teraz również dodany RoleGroups.

Edytuj RoleGroups tak, aby użytkownicy w roli kierownicy wyświetlali instrukcje dotyczące edytowania kont użytkowników, podczas gdy użytkownicy w roli Administratorzy są pokazani instrukcje dotyczące edytowania i usuwania. Po wprowadzeniu tych zmian widoku logowania deklaratywny powinien wyglądać podobnie do poniższego.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Po wprowadzeniu tych zmian Zapisz stronę, a następnie odwiedź ją za pomocą przeglądarki. Najpierw odwiedź stronę jako użytkownika anonimowego. Powinien zostać wyświetlony komunikat "użytkownik nie jest zalogowany do systemu. W związku z tym nie można edytować ani usunąć informacji o użytkownikach ". Następnie zaloguj się jako użytkownik uwierzytelniony, ale nie znajduje się on w roli kierownicy ani Administratorzy. W tym czasie powinien zostać wyświetlony komunikat "nie jesteś członkiem roli opiekunów ani administratorów. W związku z tym nie można edytować ani usunąć informacji o użytkownikach ".

Następnie zaloguj się jako użytkownik będący członkiem roli kierownicy. Ten czas powinien być widoczny dla nadzorowanych komunikatów specyficznych dla roli (zobacz rysunek 9). W przypadku zalogowania się jako użytkownik w roli Administratorzy powinien zostać wyświetlony komunikat dotyczący roli Administratorzy (Zobacz Rysunek 10).

[![Bruce jest wyświetlany komunikat dotyczący roli kierownicy](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Rysunek 9**. Bruce jest wyświetlany komunikat specyficzny dla roli kierownicy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-cs/_static/image27.png))

[w ![Tito jest pokazywany komunikat specyficzny dla roli administratorów](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Ilustracja 10**. Tito jest pokazywany komunikat specyficzny dla roli administratorów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image30.png))

Jako że zrzuty ekranu w ilustracjach 9 i 10 pokazują, widoku logowania renderuje tylko jeden szablon, nawet jeśli stosuje się wiele szablonów. Bruce i Tito są zalogowani użytkownicy, ale widoku logowania renderuje tylko pasującą rolę i nie `LoggedInTemplate`. Ponadto Tito należy do ról Administratorzy i kierownicy, ale kontrolka widoku logowania renderuje szablon specyficzny dla roli Administratorzy, a nie dla nadzoru.

Rysunek 11 ilustruje przepływ pracy używany przez formant widoku logowania do określenia szablonu do renderowania. Należy pamiętać, że jeśli określono więcej niż jeden z ról, szablon widoku logowania renderuje *pierwszą* rolę, która pasuje. Innymi słowy, jeśli w pierwszej kolejności umieścisz rolę kierownicy jako pierwszą rolę i administratorów, a następnie Tito odwiedził Tę stronę, zobaczymy komunikat nadzoru.

[![przepływu pracy kontrolki widoku logowania w celu określenia szablonu do renderowania](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Rysunek 11**: przepływ pracy formantu widoku logowania na potrzeby określania szablonu do renderowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-cs/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Programowe Ograniczanie funkcjonalności

Gdy kontrolka widoku logowania wyświetla różne instrukcje w oparciu o rolę użytkownika odwiedzającą stronę, przyciski Edytuj i Anuluj pozostaną widoczne dla wszystkich. Musimy programowo ukryć przyciski Edytuj i Usuń dla anonimowych odwiedzających i użytkowników, którzy znajdują się w roli kierownicy i Administratorzy. Musimy ukryć przycisk Usuń dla wszystkich użytkowników, którzy nie są administratorami. W celu osiągnięcia tego celu napiszemy nieco kod, który programowo odwołuje się do CommandField Edytuj i Usuń LinkButtons i ustawia ich `Visible` właściwości na `false`, w razie potrzeby.

Najprostszym sposobem programistycznego odwoływania się do formantów w CommandField jest najpierw przekonwertowanie go na szablon. Aby to osiągnąć, kliknij link "Edytuj kolumny" w tagu inteligentnym GridView, wybierz CommandField z listy bieżących pól, a następnie kliknij link "Konwertuj to pole na TemplateField". Spowoduje to zamianę CommandField na TemplateField przy użyciu `ItemTemplate` i `EditItemTemplate`. `ItemTemplate` zawiera LinkButtons edycji i usuwania, podczas gdy `EditItemTemplate` za aktualizację i anuluje LinkButtons.

[![skonwertować CommandField na TemplateField](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Ilustracja 12**. konwertowanie CommandField na TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-cs/_static/image36.png))

Zaktualizuj LinkButtons Edytuj i Usuń w `ItemTemplate`, ustawiając ich właściwości `ID` odpowiednio do wartości `EditButton` i `DeleteButton`.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Za każdym razem, gdy dane są powiązane z elementem GridView, GridView wylicza rekordy we właściwości `DataSource` i generuje odpowiadający obiekt `GridViewRow`. Po utworzeniu każdego obiektu `GridViewRow` zdarzenie `RowCreated` zostanie wyzwolone. Aby ukryć przyciski Edytuj i Usuń dla nieautoryzowanych użytkowników, musimy utworzyć procedurę obsługi zdarzeń dla tego zdarzenia i programowo odwoływać się do LinkButtons edycji i usuwania, ustawiając odpowiednio ich właściwości `Visible`.

Utwórz procedurę obsługi zdarzeń `RowCreated` zdarzenie, a następnie Dodaj następujący kod:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Należy pamiętać, że dla *wszystkich* wierszy GridView jest wyzwalane zdarzenie `RowCreated`, w tym nagłówek, stopka, interfejs stronicowania i tak dalej. Chcemy programowo odwoływać się do LinkButtons Edytuj i Usuń, jeśli będziemy omawiać wiersz danych, który nie jest w trybie edycji (ponieważ wiersz w trybie edycji ma przyciski Update i Cancel zamiast edytować i usuwania). To sprawdzanie jest obsługiwane przez instrukcję `if`.

Jeśli chodzi o wiersz danych, który nie jest w trybie edycji, odwołania do edycji i usuwania LinkButtons są odwoływane i ich właściwości `Visible` są ustawiane na podstawie wartości logicznych zwracanych przez metodę `IsInRole(roleName)` obiektu `User`. Obiekt User odwołuje się do podmiotu utworzonego przez `RoleManagerModule`; w związku z tym Metoda `IsInRole(roleName)` używa interfejsu API role do określenia, czy bieżący użytkownik należy do *roli role*.

> [!NOTE]
> Możemy użyć klasy role bezpośrednio, zastępując wywołanie do `User.IsInRole(roleName)` za pomocą wywołania [metody`Roles.IsUserInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Postanowiono użyć metody `IsInRole(roleName)` obiektu Principal w tym przykładzie, ponieważ jest bardziej wydajna niż bezpośrednie używanie interfejsu API ról. Wcześniej w tym samouczku został skonfigurowany Menedżer ról w celu buforowania ról użytkownika w pliku cookie. Te buforowane dane cookie są używane tylko wtedy, gdy wywoływana jest metoda `IsInRole(roleName)` podmiotu zabezpieczeń; bezpośrednie wywołania interfejsu API ról zawsze obejmują podróż do magazynu ról. Nawet jeśli role nie są buforowane w pliku cookie, wywołanie metody `IsInRole(roleName)` obiektu głównego jest zwykle bardziej wydajne, ponieważ gdy jest wywoływana po raz pierwszy podczas żądania, buforuje wyniki. Interfejs API ról, z drugiej strony, nie wykonuje żadnych buforowania. Ponieważ zdarzenie `RowCreated` jest uruchamiane jeden raz dla każdego wiersza w widoku GridView, użycie `User.IsInRole(roleName)` obejmuje tylko jedną podróż do magazynu roli, a `Roles.IsUserInRole(roleName)` wymaga *n* podróży, gdzie *N* to liczba kont użytkowników wyświetlanych w siatce.

Właściwość `Visible` przycisku Edytuj jest ustawiona na `true` Jeśli użytkownik odwiedzający Tę stronę znajduje się w roli Administratorzy lub kierownicy; w przeciwnym razie jest ustawiony na `false`. Właściwość `Visible` przycisku Usuń jest ustawiona na `true` tylko wtedy, gdy użytkownik należy do roli Administratorzy.

Przetestuj Tę stronę za pomocą przeglądarki. Jeśli odwiedzasz stronę jako gość anonimowy lub jako użytkownik, który nie jest kierownikiem ani administratorem, CommandField jest pusty. nadal istnieje, ale jako wąski srebrny bez przycisków Edytuj i Usuń.

> [!NOTE]
> W przypadku odwiedzania strony przez osoby niebędące opiekunami i niebędące administratorami można ukryć CommandField. Jestem tym ćwiczeniem dla czytnika.

[![przyciski Edytuj i Usuń są ukryte dla nienadzorowanych i niebędących administratorami](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Ilustracja 13**. przyciski Edytuj i Usuń są ukryte dla nienadzorowanych i niebędących administratorami ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image39.png))

Jeśli użytkownik należy do roli kierownicy (ale nie do roli administratorów), zobaczy tylko przycisk Edytuj.

[![, gdy przycisk Edytuj jest dostępny dla nadzorców, przycisk Usuń jest ukryty](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Ilustracja 14**. gdy przycisk Edytuj jest dostępny dla nadzorców, przycisk Usuń jest ukryty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image42.png))

A jeśli administrator ma dostęp do przycisków Edytuj i Usuń.

[![przyciski Edytuj i Usuń są dostępne tylko dla administratorów](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Ilustracja 15**. przyciski Edytuj i Usuń są dostępne tylko dla administratorów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](role-based-authorization-cs/_static/image45.png))

## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Krok 3. stosowanie reguł autoryzacji opartych na rolach do klas i metod

W kroku 2 zostały ograniczone możliwości edytowania dla użytkowników w rolach kierownicy i Administratorzy oraz możliwości usuwania wyłącznie dla administratorów. Zostało to osiągnięte przez ukrycie skojarzonych elementów interfejsu użytkownika dla nieautoryzowanych użytkowników za pomocą technik programistycznych. Takie środki nie gwarantują, że nieautoryzowany użytkownik nie będzie mógł wykonać akcji uprzywilejowanej. Mogą istnieć elementy interfejsu użytkownika, które są dodawane później lub zapomniane do ukrycia dla nieautoryzowanych użytkowników. Lub haker może odnaleźć inny sposób, aby uzyskać stronę ASP.NET do wykonania żądanej metody.

Łatwym sposobem zapewnienia dostępu do konkretnej funkcji przez nieautoryzowanego użytkownika jest dekorować tej klasy lub metody z [atrybutem`PrincipalPermission`](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Gdy środowisko uruchomieniowe platformy .NET używa klasy lub wykonuje jedną z jej metod, sprawdza, czy bieżący kontekst zabezpieczeń ma odpowiednie uprawnienia. Atrybut `PrincipalPermission` zapewnia mechanizm, za pomocą którego możemy definiować te reguły.

Przeglądamy użycie atrybutu `PrincipalPermission` z powrotem w <a id="_msoanchor_9"> </a>samouczku [*autoryzacji opartego na użytkownikach*](../membership/user-based-authorization-cs.md) . Zawarto w nim, jak dekorować `SelectedIndexChanged` i `RowDeleting` programu obsługi zdarzeń, aby mogły być wykonywane tylko przez uwierzytelnionych użytkowników i Tito. Atrybut `PrincipalPermission` działa podobnie jak w przypadku ról.

Aby zabronić wykonywania dla nieautoryzowanych użytkowników, poinformujmy o użyciu atrybutu `PrincipalPermission` na `RowUpdating`ach i `RowDeleting` obsługi zdarzeń w widoku GridView. Wystarczy dodać odpowiedni atrybut korzystającego każdej definicji funkcji:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

Atrybut dla programu obsługi zdarzeń `RowUpdating` określa, że tylko użytkownicy w rolach Administratorzy i kierownicy mogą wykonywać procedurę obsługi zdarzeń, gdzie jako atrybut programu obsługi zdarzeń `RowDeleting` ogranicza wykonywanie do użytkowników w roli Administratorzy.

> [!NOTE]
> Atrybut `PrincipalPermission` jest reprezentowany jako Klasa w przestrzeni nazw `System.Security.Permissions`. Pamiętaj, aby dodać instrukcję `using System.Security.Permissions` w górnej części pliku klasy związanej z kodem, aby zaimportować tę przestrzeń nazw.

Jeśli w jakiś sposób nie administrator podejmie próbę wykonania procedury obsługi zdarzeń `RowDeleting` lub w przypadku próby `RowUpdating` wykonania procedury obsługi zdarzeń przez nienadzorujące lub niebędące administratorami, środowisko uruchomieniowe programu .NET utworzy `SecurityException`.

[![, jeśli kontekst zabezpieczeń nie jest autoryzowany do wykonania metody, zostanie zgłoszony wyjątek SecurityException](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Rysunek 16**. Jeśli kontekst zabezpieczeń nie jest autoryzowany do wykonania metody, zostanie zgłoszony `SecurityException` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-cs/_static/image48.png))

Oprócz stron ASP.NET wiele aplikacji ma również architekturę obejmującą różne warstwy, takie jak logika biznesowa i warstwy dostępu do danych. Te warstwy są zwykle implementowane jako biblioteki klas oraz klasy ofert i metody służące do wykonywania funkcji związanych z logiką biznesową i danymi. Atrybut `PrincipalPermission` jest przydatny do stosowania reguł autoryzacji również do tych warstw.

Aby uzyskać więcej informacji na temat używania atrybutu `PrincipalPermission` do definiowania reguł autoryzacji dotyczącej klas i metod, zapoznaj się z wpisem w blogu [Scott Guthrie](https://weblogs.asp.net/scottgu/), [dodając reguły autoryzacji do warstw firmy i danych przy użyciu `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazano, jak określić duże i szczegółowe reguły autoryzacji ziarna na podstawie ról użytkownika. ASP.NET. Funkcja autoryzacji adresów URL sieci umożliwia deweloperom strony określenie, które tożsamości są dozwolone lub odrzucane. Jak widać w <a id="_msoanchor_10"> </a>samouczku dotyczącym [*autoryzacji na podstawie użytkownika*](../membership/user-based-authorization-cs.md) , reguły autoryzacji adresów URL mogą być stosowane do poszczególnych użytkowników. Mogą być również stosowane do poszczególnych ról, jak zostało to opisane w kroku 1 tego samouczka.

Szczegółowe reguły autoryzacji ziarna mogą być stosowane deklaratywnie lub programowo. W kroku 2 została sprawdzona za pomocą funkcji RoleGroups kontrolki widoku logowania, aby renderować różne dane wyjściowe na podstawie ról odwiedzanego użytkownika. Poznasz również sposoby programistycznego określania, czy użytkownik należy do określonej roli i jak odpowiednio dostosować funkcje strony.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Dodawanie reguł autoryzacji do warstw firmy i danych przy użyciu `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Badanie członkostwa, ról i profilu ASP.NET 2.0: Praca z rolami](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista pytań zabezpieczających dla ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Dokumentacja techniczna elementu `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci dla liderów w tym samouczku obejmują suchi Banerjee i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](assigning-roles-to-users-cs.md)
> [dalej](creating-and-managing-roles-vb.md)
