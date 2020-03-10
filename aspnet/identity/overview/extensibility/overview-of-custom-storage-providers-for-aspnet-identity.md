---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Przegląd niestandardowych dostawców magazynu dla ASP.NET Identity ASP.NET 4. x
author: Rick-Anderson
description: ASP.NET Identity to rozszerzalny system, który umożliwia utworzenie własnego dostawcy magazynu i podłączenie go do aplikacji bez ponowienia pracy z likacji...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 21baedf6285b411f89627df9ca25d47a2a42e387
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584414"
---
# <a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Omówienie niestandardowych dostawców magazynu dla systemu ASP.NET Identity

Autor [FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity to rozszerzalny system, który umożliwia utworzenie własnego dostawcy magazynu i podłączenie go do aplikacji bez konieczności ponownego pracy aplikacji. W tym temacie opisano sposób tworzenia niestandardowego dostawcy magazynu dla ASP.NET Identity. Obejmuje to ważne koncepcje tworzenia własnego dostawcy magazynu, ale nie jest to przewodnik krok po kroku dotyczący wdrażania niestandardowego dostawcy magazynu.
> 
> Przykład implementacji niestandardowego dostawcy magazynu znajduje się w temacie [implementacja niestandardowego dostawcy magazynu ASP.NET Identity MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Ten temat został zaktualizowany dla ASP.NET Identity 2,0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Visual Studio 2013 z aktualizacją Update 2
> - ASP.NET Identity 2

## <a name="introduction"></a>Wprowadzenie

Domyślnie system ASP.NET Identity przechowuje informacje o użytkowniku w SQL Server bazie danych, a następnie tworzy bazę danych przy użyciu Code First Entity Framework. W przypadku wielu aplikacji to podejście działa prawidłowo. Jednak warto użyć innego typu mechanizmu trwałości, takiego jak Azure Table Storage lub już istnieją tabele bazy danych z bardzo inną strukturą niż domyślna implementacja. W obu przypadkach można napisać niestandardowego dostawcę dla mechanizmu magazynu i podłączyć go do aplikacji.

ASP.NET Identity jest uwzględniany domyślnie w wielu Visual Studio 2013 szablonach. Aktualizacje do ASP.NET Identity można uzyskać przy użyciu [pakietu NuGet Microsoft ASPNET Identity EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Ten temat zawiera następujące sekcje:

- [Omówienie architektury](#architecture)
- [Zrozumienie przechowywanych danych](#data)
- [Tworzenie warstwy dostępu do danych](#dal)
- [Dostosowywanie klasy użytkownika](#user)
- [Dostosowywanie magazynu użytkowników](#userstore)
- [Dostosowywanie klasy roli](#role)
- [Dostosowywanie magazynu ról](#rolestore)
- [Zmień konfigurację aplikacji tak, aby korzystała z nowego dostawcy magazynu](#reconfigure)
- [Inne implementacje niestandardowych dostawców magazynu](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Omówienie architektury

ASP.NET Identity składa się z klas o nazwie menedżerowie i sklepy. Menedżerowie są klasami wysokiego poziomu, których deweloperzy aplikacji używają do wykonywania operacji, takich jak tworzenie użytkownika w systemie ASP.NET Identity. Magazyny są klasy niższego poziomu, które określają, jak są utrwalane jednostki, takie jak użytkownicy i role. Magazyny są ściśle powiązane z mechanizmem trwałości, ale menedżerowie są niepołączeni z magazynów, co oznacza, że można zastąpić mechanizm trwałości bez zakłócania całej aplikacji.

Na poniższym diagramie przedstawiono, w jaki sposób aplikacja sieci Web współdziała z menedżerami, i przechowuje interakcję z warstwą dostępu do danych.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Aby utworzyć niestandardowego dostawcę magazynu dla ASP.NET Identity, należy utworzyć źródło danych, warstwę dostępu do danych i klasy magazynu, które współpracują z tą warstwą dostępu do danych. Można nadal używać tych samych interfejsów API Menedżera do wykonywania operacji na danych użytkownika, ale teraz dane są zapisywane w innym systemie magazynu.

Nie trzeba dostosowywać klas Menedżera, ponieważ podczas tworzenia nowego wystąpienia elementu Usermanager lub roleManager udostępniasz typ klasy użytkownika i przekazujesz wystąpienie klasy magazynu jako argument. Takie podejście umożliwia podłączenie niestandardowych klas do istniejącej struktury. Zobaczysz, jak utworzyć wystąpienie klasy Usermanager i roleManager przy użyciu dostosowanych klas magazynu w sekcji [Zmień konfigurację aplikacji tak, aby korzystała z nowego dostawcy magazynu](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Zrozumienie przechowywanych danych

Aby zaimplementować niestandardowego dostawcę magazynu, należy zrozumieć typy danych używanych w ASP.NET Identity i zdecydować, które funkcje są odpowiednie dla aplikacji.

| Dane | Opis |
| --- | --- |
| Użytkownicy | Zarejestrowani użytkownicy witryny sieci Web. Zawiera identyfikator użytkownika i nazwę użytkownika. Może zawierać skrót hasła, jeśli użytkownicy logują się przy użyciu poświadczeń specyficznych dla danej lokacji (zamiast używać poświadczeń z witryny zewnętrznej, takiej jak Facebook) i sygnatury zabezpieczeń, aby wskazać, czy wszystkie zmiany w poświadczeniach użytkownika zostały zmienione. Może również zawierać adres e-mail, numer telefonu, czy włączone jest uwierzytelnianie dwuskładnikowe, bieżącą liczbę nieudanych logowań oraz czy konto zostało zablokowane. |
| Oświadczenia użytkowników | Zestaw instrukcji (lub oświadczeń) o użytkowniku, który reprezentuje tożsamość użytkownika. Można włączyć lepsze wyrażenie tożsamości użytkownika, niż można osiągnąć za pomocą ról. |
| Logowania użytkowników | Informacje o zewnętrznym dostawcy uwierzytelniania (na przykład Facebook) do użycia podczas logowania użytkownika. |
| Role | Grupy autoryzacji dla witryny. Zawiera identyfikator roli i nazwę roli (na przykład "admin" lub "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Tworzenie warstwy dostępu do danych

W tym temacie założono, że znasz mechanizm trwałości, który ma być używany, oraz sposób tworzenia jednostek dla tego mechanizmu. Ten temat nie zawiera szczegółowych informacji o sposobie tworzenia repozytoriów lub klas dostępu do danych; Zamiast tego zawiera pewne sugestie dotyczące decyzji projektowych, które należy podjąć podczas pracy z ASP.NET Identity.

Podczas projektowania repozytoriów dla niestandardowego dostawcy magazynu istnieje dużo swobody. Dla funkcji, które mają być używane w aplikacji, wystarczy utworzyć repozytoria. Na przykład, jeśli nie używasz ról w aplikacji, nie musisz tworzyć magazynu dla ról lub ról użytkownika. Twoja technologia i istniejąca infrastruktura mogą wymagać struktury, która różni się od domyślnej implementacji ASP.NET Identity. W warstwie dostępu do danych można zapewnić logikę do pracy ze strukturą repozytoriów.

Aby uzyskać implementację repozytoriów danych programu MySQL dla ASP.NET Identity 2,0, zobacz [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

W warstwie dostępu do danych można zapewnić logikę, aby zapisać dane z ASP.NET Identity ze źródłem danych. Warstwa dostępu do danych dla niestandardowego dostawcy magazynu może zawierać następujące klasy służące do przechowywania informacji o użytkowniku i roli.

| Klasa | Opis | Przykład |
| --- | --- | --- |
| Kontekst | Hermetyzuje informacje w celu nawiązania połączenia z mechanizmem trwałości i wykonywania zapytań. Ta klasa jest centralną warstwą dostępu do danych. Inne klasy danych będą wymagały wystąpienia tej klasy do wykonywania ich operacji. Należy również zainicjować klasy magazynu przy użyciu wystąpienia tej klasy. | [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Magazyn użytkowników | Przechowuje i pobiera informacje o użytkowniku (takie jak nazwa użytkownika i skrót hasła). | [Użytkownik (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Magazyn ról | Przechowuje i pobiera informacje o rolach (takie jak nazwa roli). | [Roleing (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Magazyn UserClaims | Przechowuje i pobiera informacje o użytkowniku (takie jak typ i wartość żądania). | [UserClaimsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Magazyn UserLogins | Przechowuje i pobiera informacje logowania użytkownika (na przykład zewnętrzny dostawca uwierzytelniania). | [UserLoginsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Przestrzeń dyskowa UserRole | Przechowuje i pobiera role, do których użytkownik jest przypisany. | [UserRoleTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Ponownie wystarczy zaimplementować klasy, które mają być używane w aplikacji.

W klasach dostępu do danych dostarcza się kod służący do wykonywania operacji na danych dla określonego mechanizmu trwałości. Na przykład w ramach implementacji programu MySQL Klasa users zawiera metodę wstawiania nowego rekordu do tabeli bazy danych użytkowników. Zmienna o nazwie `_database` jest wystąpieniem klasy MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Po utworzeniu klas dostępu do danych należy utworzyć klasy magazynu, które wywołują konkretne metody z warstwy dostępu do danych.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Dostosowywanie klasy użytkownika

Podczas wdrażania własnego dostawcy magazynu należy utworzyć klasę użytkownika, która jest równoważna z klasą [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) w przestrzeni nazw [Microsoft. ASP. NET. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

Na poniższym diagramie przedstawiono klasę IdentityUser, którą należy utworzyć, oraz interfejs do zaimplementowania w tej klasie.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

Interfejs [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) definiuje właściwości, które użytkownikmanager próbuje wywołać podczas wykonywania żądanych operacji. Interfejs zawiera dwie właściwości — ID i UserName. Interfejs [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) umożliwia określenie typu klucza użytkownika za pomocą ogólnego parametru **TKey** . Typ właściwości ID pasuje do wartości parametru TKey.

Program Identity Framework udostępnia również interfejs [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) (bez parametru generycznego), jeśli chcesz użyć wartości ciągu dla klucza.

Klasa IdentityUser implementuje IUser i zawiera wszelkie dodatkowe właściwości lub konstruktorów dla użytkowników w witrynie sieci Web. W poniższym przykładzie pokazano klasę IdentityUser, która używa wartości całkowitej dla klucza. Pole ID ma wartość **int** , aby odpowiadało wartości parametru generycznego. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Aby uzyskać kompletną implementację, zobacz [IdentityUser (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Dostosowywanie magazynu użytkowników

Utworzysz również klasę UserStore, która dostarcza metody dla wszystkich operacji na danych użytkownika. Ta klasa jest równoważna z klasą [UserStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) w przestrzeni nazw [Microsoft. ASP. NET. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) . W klasie UserStore zaimplementowano [IUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) i wszystkie opcjonalne interfejsy. Należy wybrać opcjonalne interfejsy do wdrożenia na podstawie funkcji, które mają być używane w aplikacji.

Na poniższej ilustracji przedstawiono klasę UserStore, która musi zostać utworzona, oraz odpowiednie interfejsy.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Domyślny szablon projektu w programie Visual Studio zawiera kod, który zakłada, że wiele opcjonalnych interfejsów zostało zaimplementowanych w magazynie użytkownika. Jeśli używasz szablonu domyślnego z niestandardowym magazynem użytkowników, musisz zaimplementować opcjonalne interfejsy w magazynie użytkownika lub zmienić kod szablonu, aby nie wywoływać metod w interfejsach, które nie zostały zaimplementowane.

 Poniższy przykład pokazuje prostą klasę magazynu użytkownika. Parametr generyczny **TUser** Pobiera typ klasy użytkownika, która jest zwykle klasą IdentityUser, która została zdefiniowana. Parametr generyczny **TKey** Pobiera typ klucza użytkownika. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 W tym przykładzie Konstruktor, który pobiera parametr o nazwie *baza danych* typu ExampleDatabase, jest tylko ilustracją, jak przekazać do klasy dostępu do danych. Na przykład w implementacji programu MySQL ten Konstruktor przyjmuje parametr typu MySQLDatabase. 

W klasie UserStore używane są klasy dostępu do danych, które zostały utworzone w celu wykonywania operacji. Na przykład w implementacji programu MySQL Klasa UserStore ma metodę UtwórzRekord, która używa wystąpienia obiektu użytkownika, aby wstawić nowy rekord. Metoda **INSERT** dla obiektu **userion** jest taka sama jak metoda, która została pokazana w poprzedniej sekcji. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfejsy do wdrożenia podczas dostosowywania magazynu użytkownika

Na następnym obrazie przedstawiono szczegółowe informacje o funkcjach zdefiniowanych w poszczególnych interfejsach. Wszystkie opcjonalne interfejsy dziedziczą z IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  Interfejs [IUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) jest interfejsem, który należy zaimplementować w magazynie użytkownika. Definiuje on metody tworzenia, aktualizowania, usuwania i pobierania użytkowników.
- **IUserClaimStore**  
  [IUserClaimStore&lt;TUser, TKey interfejs&gt;](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) definiuje metody, które należy zaimplementować w magazynie użytkownika, aby umożliwić oświadczenia użytkowników. Zawiera ona metody lub Dodawanie, usuwanie i pobieranie oświadczeń użytkowników.
- **IUserLoginStore**  
  [IUserLoginStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) definiuje metody, które należy zaimplementować w magazynie użytkownika, aby umożliwić zewnętrznym dostawcom uwierzytelniania. Zawiera metody dodawania, usuwania i pobierania identyfikatorów logowania użytkowników oraz metoda pobierania użytkownika na podstawie informacji logowania.
- **IUserRoleStore**  
  [IUserRoleStore&lt;TKey, TUser interfejs&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) definiuje metody, które należy zaimplementować w magazynie użytkownika, aby zamapować użytkownika na rolę. Zawiera on metody dodawania, usuwania i pobierania ról użytkownika oraz metodę sprawdzania, czy użytkownik jest przypisany do roli.
- **IUserPasswordStore**  
  [IUserPasswordStore&lt;TUser, TKey interfejs&gt;](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) definiuje metody, które należy zaimplementować w magazynie użytkownika, aby utrwalać hasła z mieszaniem. Zawiera on metody pobierania i ustawiania skrótu hasła oraz metodę, która wskazuje, czy użytkownik ustawił hasło.
- **IUserSecurityStampStore**  
  [IUserSecurityStampStore&lt;TUser, TKey interfejs&gt;](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) definiuje metody, które należy zaimplementować w magazynie użytkownika, aby można było określić, czy informacje o koncie użytkownika zostały zmienione. Ta sygnatura jest aktualizowana, gdy użytkownik zmienia hasło lub dodaje lub usuwa logowania. Zawiera on metody pobierania i ustawiania sygnatury zabezpieczeń.
- **IUserTwoFactorStore**  
  [IUserTwoFactorStore&lt;TUser, TKey interfejs&gt;](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) definiuje metody, które należy zaimplementować w celu wdrożenia uwierzytelniania dwuskładnikowego. Zawiera on metody umożliwiające pobieranie i określanie, czy uwierzytelnianie dwuskładnikowe jest włączone dla użytkownika.
- **IUserPhoneNumberStore**  
  [IUserPhoneNumberStore&lt;TUser, TKey interfejs&gt;](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) definiuje metody, które należy zaimplementować w celu przechowywania numerów telefonów użytkowników. Zawiera on metody pobierania i ustawiania numeru telefonu oraz tego, czy numer telefonu został potwierdzony.
- **IUserEmailStore**  
  [IUserEmailStore&lt;TUser, TKey interfejs&gt;](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) definiuje metody, które należy zaimplementować, aby przechowywać adresy e-mail użytkowników. Zawiera on metody pobierania i ustawiania adresu e-mail oraz tego, czy wiadomość e-mail została potwierdzona.
- **IUserLockoutStore**  
  [IUserLockoutStore&lt;TUser, TKey interfejs&gt;](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) definiuje metody, które należy zaimplementować, aby przechowywać informacje o blokowaniu konta. Zawiera metody pobierania bieżącej liczby nieudanych prób dostępu, uzyskiwania i określania, czy konto może być zablokowane, pobieranie i Ustawianie daty zakończenia blokady, zwiększanie liczby nieudanych prób i resetowanie liczby nieudanych prób.
- **IQueryableUserStore**  
  [IQueryableUserStore&lt;TUser, TKey interfejs&gt;](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) definiuje elementy członkowskie, które należy zaimplementować, aby zapewnić magazyn użytkowników queryable. Zawiera właściwość, która posiada użytkowników queryable.

  Należy zaimplementować interfejsy, które są potrzebne w aplikacji; takie jak, IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore i IUserSecurityStampStore, jak pokazano poniżej. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Aby uzyskać pełną implementację (w tym wszystkie interfejsy), zobacz [UserStore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin i IdentityUserRole

Przestrzeń nazw Microsoft. AspNet. Identity. EntityFramework zawiera implementacje klas [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)i [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) . Jeśli używasz tych funkcji, możesz chcieć utworzyć własne wersje tych klas i zdefiniować właściwości aplikacji. Czasami jednak bardziej wydajne jest, aby nie ładować tych jednostek do pamięci podczas wykonywania podstawowych operacji (takich jak dodawanie lub usuwanie roszczeń użytkownika). Zamiast tego klasy magazynu zaplecza mogą wykonywać te operacje bezpośrednio w źródle danych. Na przykład Metoda UserStore. GetClaimsAsync () może wywołać userClaimTable. FindByUserId (użytkownika. ID) metoda umożliwiająca bezpośrednie wykonanie zapytania w tej tabeli i zwrócenie listy oświadczeń.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Dostosowywanie klasy roli

Podczas implementowania własnego dostawcy magazynu należy utworzyć klasę roli, która jest równoważna z klasą [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) w przestrzeni nazw [Microsoft. ASP. NET. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

Na poniższym diagramie przedstawiono klasę IdentityRole, którą należy utworzyć, oraz interfejs do zaimplementowania w tej klasie.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

Interfejs [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) definiuje właściwości, które są podejmowane przez rolęmanager podczas wykonywania żądanych operacji. Interfejs zawiera dwie właściwości-ID i Name. Interfejs [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) umożliwia określenie typu klucza dla roli za pomocą ogólnego parametru **TKey** . Typ właściwości ID pasuje do wartości parametru TKey.

Program Identity Framework udostępnia również interfejs [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) (bez parametru generycznego), jeśli chcesz użyć wartości ciągu dla klucza.

W poniższym przykładzie pokazano klasę IdentityRole, która używa wartości całkowitej dla klucza. Pole ID ma wartość int, aby odpowiadało wartości parametru generycznego. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Aby uzyskać kompletną implementację, zobacz [IdentityRole (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Dostosowywanie magazynu ról

Utworzysz również klasę RoleStore, która dostarcza metody dla wszystkich operacji na danych na rolach. Ta klasa jest równoważna z klasą [RoleStore&lt;TRole&gt;](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) w przestrzeni nazw Microsoft. ASP. NET. Identity. EntityFramework. W klasie RoleStore zaimplementowano [IRoleStore&lt;TRole, TKey&gt;](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) i opcjonalnie [IQueryableRoleStore&lt;TRole, TKey&gt;](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) interfejs.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Poniższy przykład przedstawia klasę magazynu ról. Parametr generyczny TRole Pobiera typ klasy roli, która jest zwykle klasą IdentityRole, która została zdefiniowana. Parametr generyczny TKey Pobiera typ klucza roli. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  Interfejs [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) definiuje metody do zaimplementowania w klasie magazynu ról. Zawiera metody tworzenia, aktualizowania, usuwania i pobierania ról.
- **RoleStore&lt;TRole&gt;**  
  Aby dostosować RoleStore, należy utworzyć klasę, która implementuje interfejs IRoleStore. Musisz zaimplementować tę klasę tylko, jeśli chcesz używać ról w systemie. Konstruktor, który pobiera parametr o nazwie *baza danych* typu ExampleDatabase, jest tylko ilustracją, jak przekazać do klasy dostępu do danych. Na przykład w implementacji programu MySQL ten Konstruktor przyjmuje parametr typu MySQLDatabase.  
  
  Aby uzyskać kompletną implementację, zobacz [RoleStore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Zmień konfigurację aplikacji tak, aby korzystała z nowego dostawcy magazynu

Nowy dostawca magazynu został zaimplementowany. Teraz musisz skonfigurować aplikację do korzystania z tego dostawcy magazynu. Jeśli domyślny dostawca magazynu został uwzględniony w projekcie, należy usunąć domyślnego dostawcę i zamienić go na dostawcę.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Zastąp domyślnego dostawcę magazynu w projekcie MVC

1. W oknie **Zarządzanie pakietami NuGet** odinstaluj pakiet **Microsoft ASP.NET Identity EntityFramework** . Ten pakiet można znaleźć, wyszukując w zainstalowanych pakietach Identity. EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) zostanie wyświetlony monit, jeśli chcesz również odinstalować Entity Framework. Jeśli nie potrzebujesz go w innych częściach aplikacji, możesz go odinstalować.
2. W pliku IdentityModels.cs w folderze models Usuń lub Skomentuj klasy **ApplicationUser** i **ApplicationDbContext** . W aplikacji MVC można usunąć cały plik IdentityModels.cs. W aplikacji formularzy sieci Web Usuń dwie klasy, ale upewnij się, że utrzymujesz klasę pomocnika, która znajduje się również w pliku IdentityModels.cs.
3. Jeśli Twój dostawca magazynu znajduje się w osobnym projekcie, Dodaj odwołanie do niego w aplikacji sieci Web.
4. Zastąp wszystkie odwołania do `using Microsoft.AspNet.Identity.EntityFramework;` za pomocą instrukcji using dla przestrzeni nazw dostawcy magazynu.
5. W klasie **Startup.auth.cs** Zmień metodę **ConfigureAuth** , tak aby korzystała z jednego wystąpienia odpowiedniego kontekstu. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. W folderze Start\_aplikacji Otwórz **IdentityConfig.cs**. W klasie ApplicationUserManager Zmień metodę **Create** , aby zwracała Menedżera użytkowników, który korzysta z niestandardowego magazynu użytkownika. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Zastąp wszystkie odwołania do **ApplicationUser** z **IdentityUser**.
8. Domyślny projekt zawiera niektóre elementy członkowskie klasy użytkownika, które nie są zdefiniowane w interfejsie IUser; takie jak Poczta E-mail, PasswordHash i GenerateUserIdentityAsync. Jeśli klasa użytkownika nie ma tych elementów członkowskich, należy wdrożyć je lub zmienić kod, który używa tych elementów członkowskich.
9. Jeśli utworzono wystąpienia elementu roleManager, Zmień ten kod, aby użyć nowej klasy RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Projekt domyślny został zaprojektowany dla klasy użytkownika, która ma wartość ciągu dla klucza. Jeśli klasa użytkownika ma inny typ klucza (na przykład liczba całkowita), należy zmienić projekt, aby współpracował z typem. Zobacz [Zmień klucz podstawowy dla użytkowników w ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. W razie potrzeby Dodaj parametry połączenia do pliku Web. config.

<a id="other"></a>
## <a name="other-resources"></a>Inne zasoby

- Blog: [implementowanie ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Samouczek i kod GIT: [Simple. Data ASP.NET — dostawca tożsamości](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Samouczek:[Konfigurowanie podstawowych kont tożsamości i wskazywanie ich w zewnętrznej bazie danych](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Przez [@xivSolutions](https://twitter.com/xivSolutions).
- Samouczek[: implementowanie niestandardowego dostawcy magazynu ASP.NET Identity MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent jednostek](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) według [SoftFluent](http://www.softfluent.com/)
- [Table Storage systemu Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) — Kuba Randall.
- Azure Table Storage: [ASPNET. Identity. TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) przez [@stuartleeks](https://twitter.com/stuartleeks).
- [CouchDB/Cloud w Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elastyczna ciąg[h: tożsamość elastyczna](https://github.com/bmbsqd/elastic-identity) według BombSquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathana Sheely Jonathana Sheely.
- [NHibernate. ASPNET. Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) przez Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) [@tourismgeek](https://twitter.com/tourismgeek).
- [RavenDB. ASPNET. Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) przez [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis. ASPNET. Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Szablony T4 do generowania kodu EF dla magazynu użytkownika "Database First": [ASPNET. Identity. EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
