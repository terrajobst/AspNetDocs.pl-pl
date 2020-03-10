---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Dodawanie ASP.NET Identity do pustego lub istniejącego projektu Web Forms — ASP.NET 4. x
author: raquelsa
description: W tym samouczku pokazano, jak dodać ASP.NET Identity (system Membership for ASP.NET) do aplikacji ASP.NET. Podczas tworzenia nowego formularza sieci Web lub MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e82951d57f0b8052ee3f6530a7470be7d030206
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584127"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Dodawanie systemu ASP.NET Identity do pustego lub istniejącego projektu Web Forms

> W tym samouczku pokazano, jak dodać [ASP.NET Identity](introduction-to-aspnet-identity.md) (nowy system członkostwa dla ASP.NET) do aplikacji ASP.NET.
> 
> Po utworzeniu nowych formularzy sieci Web lub projektu MVC w programie Visual Studio 2017 RTM przy użyciu poszczególnych kont program Visual Studio zainstaluje wszystkie wymagane pakiety i doda wszystkie niezbędne klasy. Ten samouczek ilustruje procedurę dodawania obsługi ASP.NET Identity do istniejącego projektu formularzy sieci Web lub nowego pustego projektu. Chcę utworzyć konspekt wszystkich pakietów NuGet, które należy zainstalować, oraz klas, które należy dodać. Przejdziemy na przykładowe formularze sieci Web na potrzeby rejestrowania nowych użytkowników i logowania przy wyróżnieniu wszystkich głównych interfejsów API punktu wejścia na potrzeby zarządzania użytkownikami i ich uwierzytelniania. W tym przykładzie zostanie użyta domyślna implementacja usługi SQL Data Storage ASP.NET Identity, która jest oparta na Entity Framework. W tym samouczku będziemy używać usługi LocalDB dla bazy danych SQL.
> 

## <a name="get-started-with-aspnet-identity"></a>Wprowadzenie do ASP.NET Identity

1. Zacznij od zainstalowania i uruchomienia [programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Wybierz pozycję **Nowy projekt** na stronie startowej lub użyj menu i wybierz pozycję **plik**, a następnie **Nowy projekt**.
3. W okienku po lewej stronie rozwiń **pozycję C#Wizualizacja** , a następnie wybierz pozycję **Sieć Web**, a następnie **ASP.NET aplikację sieci Web (.NET Framework)** . Nadaj projektowi nazwę "WebFormsIdentity" i wybierz pozycję **OK**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. W oknie dialogowym **Nowy projekt ASP.NET** wybierz **pusty** szablon.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Zauważ, że przycisk **Zmień uwierzytelnianie** jest wyłączony i w tym szablonie nie jest dostępna żadna obsługa uwierzytelniania. Szablony Web Forms, MVC i Web API umożliwiają wybranie podejścia uwierzytelniania.

## <a name="add-identity-packages-to-your-app"></a>Dodawanie pakietów tożsamości do aplikacji

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**. Wyszukaj i zainstaluj pakiet **Microsoft. ASPNET. Identity. EntityFramework** . 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Należy pamiętać, że ten pakiet zainstaluje pakiety zależności: **EntityFramework** i **Microsoft ASP.NET Identity Core**.

## <a name="add-a-web-form-to-register-users"></a>Dodaj formularz sieci Web, aby zarejestrować użytkowników

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj**, a następnie **formularz sieci Web**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. W oknie dialogowym **Określanie nazwy dla elementu** Nazwij nowy **Rejestr**formularzy sieci Web, a następnie wybierz przycisk **OK** .
3. Zastąp znaczniki w wygenerowanym pliku *register. aspx* poniższym kodem. Zmiany kodu są wyróżnione. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Jest to tylko uproszczona wersja pliku *register. aspx* , który jest tworzony podczas tworzenia nowego projektu ASP.NET Web Forms. Znacznik powyżej dodaje pola formularza i przycisk umożliwiający zarejestrowanie nowego użytkownika.
4. Otwórz plik *register.aspx.cs* i Zastąp zawartość pliku następującym kodem:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Powyższy kod jest uproszczoną wersją pliku *register.aspx.cs* , który jest tworzony podczas tworzenia nowego projektu ASP.NET Web Forms.
    > 2. Klasa *IdentityUser* jest domyślną implementacją EntityFramework interfejsu *IUser* . Interfejs *IUser* jest minimalnym interfejsem użytkownika na ASP.NET Identity Core.
    > 3. Klasa *UserStore* to domyślna implementacja EntityFramework magazynu użytkownika. Ta klasa implementuje minimalne interfejsy ASP.NET Identity rdzeń: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* i *IUserRoleStore*.
    > 4. Klasa *usermanager* uwidacznia interfejsy API powiązane z użytkownikiem, które będą automatycznie zapisywać zmiany w *UserStore*.
    > 5. Klasa *IdentityResult* reprezentuje wynik operacji tożsamości.
5. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj**, **Dodaj folder ASP.NET** , a następnie **aplikacja\_dane**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Otwórz plik *Web. config* i Dodaj wpis parametrów połączenia dla bazy danych, która będzie używana do przechowywania informacji o użytkowniku. Baza danych zostanie utworzona w czasie wykonywania przez EntityFramework dla jednostek tożsamości. Parametry połączenia są podobne do tych, które zostały utworzone podczas tworzenia nowego projektu formularzy sieci Web. Wyróżniony kod pokazuje znacznik, który należy dodać:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > W przypadku programu Visual Studio 2015 lub nowszego Zastąp `(localdb)\v11.0` z `(localdb)\MSSQLLocalDB` w parametrach połączenia.
    
7. Kliknij prawym przyciskiem myszy plik *register. aspx* w projekcie i wybierz polecenie **Ustaw jako stronę startową**. Naciśnij kombinację klawiszy CTRL + F5, aby skompilować i uruchomić aplikację sieci Web. Wprowadź nową nazwę użytkownika i hasło, a następnie wybierz pozycję **zarejestruj**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity obsługuje walidację, a w tym przykładzie można sprawdzić domyślne zachowanie funkcji sprawdzania poprawności użytkowników i haseł pochodzących z pakietu Identity Core. Domyślny moduł walidacji dla użytkownika (`UserValidator`) ma właściwość `AllowOnlyAlphanumericUserNames`, która ma ustawioną wartość domyślną `true`. Domyślny moduł sprawdzania poprawności hasła (`MinimumLengthValidator`) gwarantuje, że hasło ma co najmniej 6 znaków. Te moduły sprawdzania poprawności są właściwościami `UserManager`, które mogą zostać zastąpione, jeśli chcesz mieć niestandardową weryfikację

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Sprawdź bazę danych tożsamości LocalDb i tabele wygenerowane przez Entity Framework

1. W menu **Widok** wybierz **Eksplorator serwera**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Rozwiń węzeł **DefaultConnection (WebFormsIdentity)** , rozwiń węzeł **tabele**, kliknij prawym przyciskiem myszy pozycję **AspNetUsers** , a następnie wybierz polecenie **Pokaż dane tabeli**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Konfigurowanie aplikacji na potrzeby uwierzytelniania OWIN

W tym momencie dodaliśmy obsługę tworzenia użytkowników. Teraz będziemy demonstrować, jak możemy dodać uwierzytelnianie, aby zalogować użytkownika. ASP.NET Identity używa oprogramowania pośredniczącego Microsoft OWIN Authentication do uwierzytelniania formularzy. Uwierzytelnianie plików cookie OWIN to mechanizm uwierzytelniania oparty na plikach cookie i oświadczenia, który może być używany przez dowolną platformę hostowaną w usłudze [Owin](https://msdn.microsoft.com/magazine/dn451439.aspx) lub IIS. Przy użyciu tego modelu można używać tych samych pakietów uwierzytelniania dla wielu platform, w tym ASP.NET MVC i Web Forms. Aby uzyskać więcej informacji o programie Project Katana i sposobach uruchamiania oprogramowania pośredniczącego w niezależny od hosta, zobacz [wprowadzenie z projektem Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Instalowanie pakietów uwierzytelniania w aplikacji

1. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**. Wyszukaj i zainstaluj pakiet ***Microsoft. ASPNET. Identity. Owin*** . 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Wyszukaj i zainstaluj pakiet ***Microsoft. Owin. host. SystemWeb*** .

    > [!NOTE]
    > Pakiet **Microsoft. ASPNET. Identity. Owin** zawiera zestaw klas rozszerzeń Owin do zarządzania i konfigurowania oprogramowania pośredniczącego uwierzytelniania Owin, które ma być używane przez pakiety ASP.NET Identity Core.
    > Pakiet **Microsoft. Owin. host. SystemWeb** zawiera serwer Owin, który umożliwia uruchamianie aplikacji opartych na Owin w usługach IIS przy użyciu potoku żądań ASP.NET. Aby uzyskać więcej informacji [, zobacz Owin oprogramowania pośredniczącego w zintegrowanym potoku usług IIS](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Dodaj klasy konfiguracji uruchamiania i uwierzytelniania OWIN

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, wybierz polecenie **Dodaj**, a następnie **Dodaj nowy element**. W oknie dialogowym Wyszukiwanie pola tekstowego wpisz "*Owin*". Nadaj klasie nazwę "*Startup*" i wybierz pozycję **Dodaj**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. W pliku Startup.cs Dodaj wyróżniony kod przedstawiony poniżej, aby skonfigurować uwierzytelnianie OWIN plików cookie.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Ta klasa zawiera atrybut `OwinStartup` służący do określania klasy startowej OWIN. Każda aplikacja OWIN ma klasę uruchomieniową, w której można określić składniki dla potoku aplikacji. Aby uzyskać więcej informacji na temat tego modelu, zobacz [wykrywanie klasy startowej Owin](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) .

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Dodawanie formularzy sieci Web na potrzeby rejestrowania i logowania użytkowników

1. Otwórz plik *register.aspx.cs* i Dodaj następujący kod, który loguje się do użytkownika po pomyślnym zarejestrowaniu.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Ponieważ ASP.NET Identity i OWIN uwierzytelnianie plików cookie to system oparty na oświadczeniach, struktura wymaga, aby Deweloper aplikacji generował [Identyfikator oświadczenia](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) dla użytkownika. Identyfikator oświadczenia zawiera informacje o wszystkich oświadczeniach dla użytkownika, takich jak role, do których należy użytkownik. Możesz również dodać więcej oświadczeń dla użytkownika na tym etapie.
    > - Użytkownik może zalogować się przy użyciu metody AuthenticationManager z OWIN i wywoływać `SignIn` i przekazywać w identyfikator oświadczenia, jak pokazano powyżej. Ten kod zarejestruje użytkownika i wygeneruje również plik cookie. To wywołanie jest analogiczne do [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) używane przez moduł [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
2. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, wybierz polecenie **Dodaj**, a następnie **formularz sieci Web**. Nadaj nazwę **logowania**formularza sieci Web.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Zastąp zawartość pliku *login. aspx* następującym kodem:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Zastąp zawartość pliku *login.aspx.cs* następującym:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load` teraz sprawdza stan bieżącego użytkownika i podejmuje akcję na podstawie jego stanu `Context.User.Identity.IsAuthenticated`.
    >   **Wyświetl zalogowany użytkownik nazwa użytkownika** Microsoft ASP.NET: IIdentity Identity Framework dodał metody rozszerzenia do [System. Security. Principal.](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) , które umożliwiają uzyskanie `UserName` i `UserId` dla zalogowanego użytkownika. Te metody rozszerzające są zdefiniowane w zestawie `Microsoft.AspNet.Identity.Core`. Te metody rozszerzające są zamiennikiem [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Metoda logowania: Metoda `This` zastępuje poprzednią metodę `CreateUser_Click` w tym przykładzie i teraz loguje się do użytkownika po pomyślnym utworzeniu użytkownika.   
    >   Program Microsoft OWIN Framework dodał metody rozszerzenia na `System.Web.HttpContext`, które umożliwiają uzyskanie odwołania do `IOwinContext`. Te metody rozszerzające są zdefiniowane w zestawie `Microsoft.Owin.Host.SystemWeb`. Klasa `OwinContext` uwidacznia Właściwość `IAuthenticationManager`, która reprezentuje funkcje oprogramowania pośredniczącego uwierzytelniania dostępne w bieżącym żądaniu. Użytkownik może zalogować się przy użyciu `AuthenticationManager` z OWIN i wywołując `SignIn` i przekazywać w `ClaimsIdentity` jak pokazano powyżej. Ponieważ ASP.NET Identity i OWIN uwierzytelnianie plików cookie to system oparty na oświadczeniach, struktura wymaga, aby aplikacja generowała `ClaimsIdentity` dla użytkownika. `ClaimsIdentity` zawiera informacje o wszystkich oświadczeniach dla użytkownika, takich jak role, do których należy użytkownik. Możesz również dodać więcej oświadczeń dla użytkownika na tym etapie ten kod zarejestruje użytkownika i wygeneruje również plik cookie. To wywołanie jest analogiczne do [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) używane przez moduł [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
    > - `SignOut` Metoda: pobiera odwołanie do `AuthenticationManager` z OWIN i wywołuje `SignOut`. Jest to analogiczny do metody [FormsAuthentication. wylogowaniu](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) używanej przez moduł [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
5. Naciśnij **kombinację klawiszy CTRL + F5** , aby skompilować i uruchomić aplikację sieci Web. Wprowadź nową nazwę użytkownika i hasło, a następnie wybierz pozycję **zarejestruj**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Uwaga: w tym momencie nowy użytkownik zostanie utworzony i zalogowany.
6. Wybierz przycisk **Wyloguj** . Nastąpi przekierowanie do formularza logowania.
7. Wprowadź nieprawidłową nazwę użytkownika lub hasło, a następnie wybierz przycisk **Zaloguj się** . 
   Metoda `UserManager.Find` zwróci wartość null i zostanie wyświetlony komunikat o błędzie: " *Nieprawidłowa nazwa użytkownika lub hasło* " zostanie wyświetlona.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
