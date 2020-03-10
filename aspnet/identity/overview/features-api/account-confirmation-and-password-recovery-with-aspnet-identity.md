---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Potwierdzenie konta & Odzyskiwanie hasła ASP.NET Identity (C#) — ASP.NET 4. x
author: HaoK
description: Przed wykonaniem tego samouczka należy najpierw zakończyć tworzenie aplikacji sieci Web Secure ASP.NET MVC 5 z logowaniem, potwierdzeniem i resetowaniem hasła wiadomości e-mail. Ten samouczek...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b2c88280df39aa81d60f9508910e8fe5d6db6b8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616817"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET IdentityC#()

> Przed wykonaniem tego samouczka należy najpierw zakończyć [Tworzenie aplikacji sieci Web secure ASP.NET MVC 5 z logowaniem, potwierdzeniem i resetowaniem hasła wiadomości e-mail](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Ten samouczek zawiera więcej szczegółów i pokazuje, jak skonfigurować adres e-mail dla potwierdzenia konta lokalnego i umożliwić użytkownikom resetowanie zapomnianego hasła w ASP.NET Identity.

Konto użytkownika lokalnego wymaga, aby użytkownik utworzył hasło do konta, a to hasło jest przechowywane (bezpiecznie) w aplikacji sieci Web. ASP.NET Identity również obsługuje konta społecznościowe, które nie wymagają od użytkownika utworzenia hasła dla aplikacji. [Konta społecznościowe](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) korzystają z innych firm (np. Google, Twitter, Facebook lub Microsoft) w celu uwierzytelniania użytkowników. W tym temacie omówiono następujące zagadnienia:

- [Utwórz aplikację ASP.NET MVC](#createMvc) i Eksploruj ASP.NET Identity funkcje.
- [Kompiluj przykład tożsamości](#build)
- [Skonfiguruj potwierdzenie poczty e-mail](#email)

Nowi użytkownicy rejestrują swoje aliasy e-mail, co powoduje utworzenie konta lokalnego.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Wybranie przycisku Zarejestruj powoduje wysłanie wiadomości e-mail z potwierdzeniem z tokenem weryfikacji na adres e-mail.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Użytkownik wysyła wiadomość e-mail z tokenem potwierdzającym dla swojego konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Wybranie linku umożliwia potwierdzenie konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Odzyskiwanie/Resetowanie hasła

Użytkownicy lokalni, którzy zapomnili hasła, mogą mieć token zabezpieczający wysyłany do swojego konta e-mail, co umożliwia im Resetowanie hasła.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Użytkownik wkrótce otrzyma wiadomość e-mail z linkiem umożliwiającym zresetowanie hasła.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Wybranie linku spowoduje przejście do strony Reset.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Wybranie przycisku **Resetuj** spowoduje potwierdzenie resetowania hasła.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Tworzenie aplikacji internetowej platformy ASP.NET

Zacznij od zainstalowania i uruchomienia [programu Visual Studio 2017](https://visualstudio.microsoft.com/).

1. Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC. Formularze sieci Web obsługują również ASP.NET Identity, więc można wykonać podobne kroki w aplikacji formularzy sieci Web.
2. Zmień uwierzytelnianie na **konta poszczególnych użytkowników**.
3. Uruchom aplikację, wybierz łącze **zarejestruj** i zarejestruj użytkownika. W tym momencie jedynym sprawdzaniem poprawności w wiadomości e-mail jest atrybut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
4. W Eksplorator serwera przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz polecenie **Otwórz definicję tabeli**.

    Na poniższej ilustracji przedstawiono schemat `AspNetUsers`:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Kliknij prawym przyciskiem myszy tabelę **AspNetUsers** , a następnie wybierz polecenie **Pokaż dane tabeli**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   W tym momencie wiadomość e-mail nie została potwierdzona.

Domyślny magazyn danych dla ASP.NET Identity jest Entity Framework, ale można go skonfigurować tak, aby korzystał z innych magazynów danych i dodać dodatkowe pola. Zobacz sekcję [dodatkowe zasoby](#addRes) na końcu tego samouczka.

[Klasa startowa Owin](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) jest wywoływana, gdy aplikacja zostanie uruchomiona i wywoła metodę `ConfigureAuth` w *aplikacji\_Start\Startup.auth.cs*, która konfiguruje potok Owin i inicjuje ASP.NET Identity. Przeanalizuj metodę `ConfigureAuth`. Każde wywołanie `CreatePerOwinContext` rejestruje wywołanie zwrotne (zapisane w `OwinContext`), które zostanie wywołane raz na żądanie w celu utworzenia wystąpienia określonego typu. Można ustawić punkt przerwania w konstruktorze i metodę `Create` każdego typu (`ApplicationDbContext, ApplicationUserManager`) i sprawdzić, czy są one wywoływane dla każdego żądania. Wystąpienie `ApplicationDbContext` i `ApplicationUserManager` jest przechowywane w kontekście OWIN, do którego można uzyskać dostęp w całej aplikacji. ASP.NET Identity podłączane do potoku OWIN za pomocą oprogramowania pośredniczącego plików cookie. Aby uzyskać więcej informacji, zobacz [Zarządzanie okresem istnienia żądania dla klasy usermanager w ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Po zmianie profilu zabezpieczeń jest generowana nowa Sygnatura zabezpieczeń, która będzie przechowywana w polu `SecurityStamp` tabeli *AspNetUsers* . Zwróć uwagę, że pole `SecurityStamp` różni się od pliku cookie zabezpieczeń. Plik cookie zabezpieczeń nie jest przechowywany w tabeli `AspNetUsers` (lub w innym miejscu w bazie danych tożsamości). Token pliku cookie zabezpieczeń jest z podpisem własnym przy użyciu funkcji [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i jest tworzony z informacjami o `UserId, SecurityStamp` i czasie wygaśnięcia.

Oprogramowanie pośredniczące cookie sprawdza plik cookie na każdym żądaniu. Metoda `SecurityStampValidator` w klasie `Startup` trafi do bazy danych i okresowo sprawdza sygnaturę zabezpieczeń, jak określono w `validateInterval`. Dzieje się to co 30 minut (w naszym przykładzie), chyba że zmienisz profil zabezpieczeń. Wybrano 30-minutowy interwał minimalizowania podróży do bazy danych. Aby uzyskać więcej informacji, zobacz [Samouczek dotyczący usługi uwierzytelniania dwuskładnikowego](index.md) .

Na komentarze w kodzie Metoda `UseCookieAuthentication` obsługuje uwierzytelnianie plików cookie. Pole `SecurityStamp` i skojarzony kod zapewniają dodatkową warstwę zabezpieczeń dla aplikacji, gdy zmienisz hasło, wylogujesz się z przeglądarki, w której się zarejestrowano. Metoda `SecurityStampValidator.OnValidateIdentity` umożliwia aplikacji Weryfikowanie tokenu zabezpieczającego, gdy użytkownik loguje się, który jest używany podczas zmiany hasła lub korzystania z logowania zewnętrznego. Jest to konieczne, aby upewnić się, że wszystkie tokeny (pliki cookie) wygenerowane przy użyciu starego hasła są unieważnione. W przykładowym projekcie, jeśli zmienisz hasło dla użytkowników, zostanie wygenerowany nowy token dla użytkownika, wszystkie poprzednie tokeny są unieważnione i pole `SecurityStamp` zostanie zaktualizowane.

System tożsamości umożliwia skonfigurowanie aplikacji w taki sposób, aby po zmianie profilu zabezpieczeń użytkowników (na przykład gdy użytkownik zmieni hasło lub zmiany skojarzone z logowaniem (na przykład z serwisu Facebook, Google, konto Microsoft itp.), użytkownik zostanie wylogowany ze wszystkich wystąpienia przeglądarki. Na przykład poniższy obraz przedstawia [pojedynczą aplikację przykładową wylogowaniu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) , która umożliwia użytkownikowi wylogowanie się ze wszystkich wystąpień przeglądarki (w tym przypadku IE, Firefox i Chrome) przez wybranie jednego przycisku. Alternatywnie przykład umożliwia wylogowanie się tylko z określonego wystąpienia przeglądarki.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

W ramach [pojedynczej przykładowej aplikacji wylogowaniu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) pokazano, jak ASP.NET Identity umożliwia ponowne wygenerowanie tokenu zabezpieczającego. Jest to konieczne, aby upewnić się, że wszystkie tokeny (pliki cookie) wygenerowane przy użyciu starego hasła są unieważnione. Ta funkcja zapewnia dodatkową warstwę zabezpieczeń aplikacji. Gdy zmienisz hasło, nastąpi wylogowanie, gdzie zalogowano się do tej aplikacji.

Plik *\_aplikacji Start\IdentityConfig.cs* zawiera klasy `ApplicationUserManager`, `EmailService` i `SmsService`. Klasy `EmailService` i `SmsService` każdy implementuje interfejs `IIdentityMessageService`, dlatego w każdej klasie istnieją popularne metody konfigurowania poczty e-mail i wiadomości SMS. Chociaż w tym samouczku pokazano, jak dodać powiadomienie e-mail za pośrednictwem usługi [SendGrid](http://sendgrid.com/), możesz wysłać wiadomość e-mail przy użyciu protokołu SMTP i innych mechanizmów.

Klasa `Startup` zawiera również płytę kotłową, która umożliwia dodawanie nazw logowania do społeczności (Facebook, Twitter itp.), zobacz My samouczek [MVC 5 app with Facebook, Twitter, LinkedIn i Google OAuth2 Signing](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) , aby uzyskać więcej informacji.

Zapoznaj się z klasą `ApplicationUserManager`, która zawiera informacje o tożsamości użytkowników i konfiguruje następujące funkcje:

- Wymagania dotyczące siły hasła.
- Zablokuj użytkownika (próby i godziny).
- Uwierzytelnianie dwuskładnikowe (funkcji 2FA). Będę pokryć funkcji 2FA i SMS w innym samouczku.
- Podłączanie usług poczty e-mail i SMS. (Będę obejmować program SMS w innym samouczku).

Klasa `ApplicationUserManager` dziedziczy z klasy generycznej `UserManager<ApplicationUser>`. `ApplicationUser` pochodzi od [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` pochodzi z klasy generycznej `IdentityUser`:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Powyższe właściwości pokrywają się z właściwościami w tabeli `AspNetUsers`, jak pokazano powyżej.

Argumenty ogólne w `IUser` umożliwiają uzyskanie klasy przy użyciu różnych typów klucza podstawowego. Zobacz przykład [ChangePK](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) , który pokazuje, jak zmienić klucz podstawowy z ciągu na int lub GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) jest zdefiniowany w *Models\IdentityModels.cs* jako:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Wyróżniony powyżej kod generuje [Identyfikator oświadczenia](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Uwierzytelnianie plików cookie ASP.NET Identity i OWIN jest oparte na oświadczeniach, dlatego struktura wymaga, aby aplikacja generowała `ClaimsIdentity` dla użytkownika. `ClaimsIdentity` zawiera informacje o wszystkich oświadczeniach dla użytkownika, takie jak nazwa użytkownika, wiek i role, do których należy użytkownik. Możesz również dodać więcej oświadczeń dla użytkownika na tym etapie.

Metoda `AuthenticationManager.SignIn` OWIN przekazuje `ClaimsIdentity` i oznakuje użytkownika:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Aplikacja MVC 5 z logowaniem w serwisach Facebook, Twitter, LinkedIn i Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) pokazuje, w jaki sposób można dodać dodatkowe właściwości do klasy `ApplicationUser`.

## <a name="email-confirmation"></a>Potwierdzenie wiadomości e-mail

Dobrym pomysłem jest potwierdzenie wysłania wiadomości e-mail przez nowego użytkownika w celu zweryfikowania, że nie są one personifikowane przez kogoś innego (oznacza to, że nie zostały zarejestrowane w wiadomości e-mail innej osoby). Załóżmy, że masz forum dyskusyjne, aby zapobiec rejestrowaniu się `"bob@example.com"` w `"joe@contoso.com"`. Bez potwierdzenia wiadomości e-mail `"joe@contoso.com"` może pobrać niechcianą wiadomość e-mail z aplikacji. Załóżmy, że Robert przypadkowo zarejestrowany jako `"bib@example.com"` i nie go, nie będzie mógł korzystać z odzyskiwania hasła, ponieważ aplikacja nie ma poprawnego adresu e-mail. Potwierdzenie poczty e-mail zapewnia tylko ograniczoną ochronę z botów i nie zapewnia ochrony przed określonymi nadawców, którzy mają wiele roboczych aliasów poczty e-mail, których mogą używać do rejestracji. W poniższym przykładzie użytkownik nie będzie mógł zmienić swojego hasła, dopóki konto nie zostanie potwierdzone (przez wybranie linku potwierdzenia otrzymanego w ramach konta e-mail, które zarejestrowano). Ten przepływ pracy można zastosować do innych scenariuszy, na przykład wysyłając link do potwierdzenia i zresetowania hasła dla nowych kont utworzonych przez administratora, wysyłając użytkownikowi wiadomość e-mail po zmianie profilu i tak dalej. Zazwyczaj chcesz uniemożliwić nowym użytkownikom ogłaszanie danych w witrynie sieci Web przed ich potwierdzeniem za pośrednictwem poczty e-mail, wiadomości SMS lub innego mechanizmu. <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>Kompiluj bardziej kompletny przykład

W tej sekcji użyjesz narzędzia NuGet do pobrania pełniejszego przykładu, z którym będziemy współpracować.

1. Utwórz nowy ***pusty*** projekt sieci Web ASP.NET.
2. W konsoli Menedżera pakietów wprowadź następujące polecenia: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   W tym samouczku będziemy używać [SendGrid](http://sendgrid.com/) do wysyłania wiadomości e-mail. Pakiet `Identity.Samples` instaluje kod, z którym będziemy pracować.
3. Ustaw [dla projektu użycie protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Przetestuj Tworzenie konta lokalnego, uruchamiając aplikację, wybierając łącze **zarejestruj** i ogłaszając formularz rejestracji.
5. Wybierz link e-mail z pokazem, który symuluje potwierdzenie wiadomości e-mail.
6. Usuń przykładowy kod potwierdzania linku do wiadomości e-mail z przykładu (kod `ViewBag.Link` na kontrolerze konta. Zobacz metody akcji `DisplayEmail` i `ForgotPasswordConfirmation` i widoki Razor).

> [!WARNING]
> Jeśli zmienisz dowolne z ustawień zabezpieczeń w tym przykładzie, aplikacje produkcji będą musiały zostać poddane inspekcji zabezpieczeń, która jawnie wywoła wprowadzone zmiany.

## <a name="examine-the-code-in-app_startidentityconfigcs"></a>Sprawdzanie kodu w aplikacji\_Start\IdentityConfig.cs

Przykład pokazuje, jak utworzyć konto i dodać je do roli *administratora* . Wiadomość e-mail powinna zostać zamieniona na przykład za pomocą wiadomości e-mail, która będzie używana dla konta administratora. Najprostszym sposobem na utworzenie konta administratora jest programowo w metodzie `Seed`. Mamy nadzieję, że w przyszłości znajdziesz narzędzie umożliwiające tworzenie i administrowanie użytkownikami i rolami. Przykładowy kod umożliwia tworzenie użytkowników i ról oraz zarządzanie nimi, ale musisz mieć konto administratora, aby uruchamiać role i strony administratora użytkowników. W tym przykładzie konto administratora jest tworzone, gdy baza danych jest wypełniania.

Zmień hasło i Zmień nazwę na konto, na którym można odbierać powiadomienia e-mail.

> [!WARNING]
> Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym.

Jak wspomniano wcześniej, wywołanie `app.CreatePerOwinContext` w klasie startowej dodaje wywołania zwrotne do metody `Create` zawartości bazy danych aplikacji, Menedżera użytkowników i klas menedżerów ról. Potok OWIN wywołuje metodę `Create` w tych klasach dla każdego żądania i przechowuje kontekst dla każdej klasy. Kontroler konta uwidacznia Menedżera użytkowników z kontekstu HTTP (który zawiera kontekst OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Gdy użytkownik rejestruje konto lokalne, Metoda `HTTP Post Register` jest wywoływana:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

W powyższym kodzie użyto danych modelu do utworzenia nowego konta użytkownika przy użyciu wprowadzonego adresu e-mail i hasła. Jeśli alias poczty e-mail znajduje się w magazynie danych, tworzenie konta kończy się niepowodzeniem, a formularz zostanie wyświetlony ponownie. Metoda `GenerateEmailConfirmationTokenAsync` tworzy bezpieczny token potwierdzania i zapisuje ją w magazynie danych ASP.NET Identity. Metoda [URL. Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) tworzy łącze zawierające `UserId` i Token potwierdzenia. Po wysłaniu tego linku do użytkownika zostanie wysłana wiadomość e-mail z informacją, że użytkownik może wybrać link w aplikacji poczty e-mail, aby potwierdzić swoje konto.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Skonfiguruj potwierdzenie poczty e-mail

Przejdź do [strony rejestracji w usłudze Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) i zarejestruj się w celu uzyskania bezpłatnego konta. Dodaj kod podobny do poniższego, aby skonfigurować SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Klienci poczty e-mail często akceptują tylko wiadomości tekstowe (bez HTML). Należy podać komunikat w postaci tekstowej i HTML. W powyższym przykładzie SendGrid jest to realizowane przy użyciu `myMessage.Text` i `myMessage.Html` kodu pokazanego powyżej.

Poniższy kod przedstawia sposób wysyłania wiadomości e-mail przy użyciu klasy [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) , w której `message.Body` zwraca tylko link.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. Konto i poświadczenia są przechowywane w element appSetting. Na platformie Azure możesz bezpiecznie przechowywać te wartości na karcie **[Konfiguracja](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** w Azure Portal. Zapoznaj [się z najlepszymi rozwiązaniami dotyczącymi wdrażania haseł i innych poufnych danych w ASP.NET i na platformie Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

Wprowadź swoje poświadczenia SendGrid, uruchom aplikację, zarejestruj się przy użyciu aliasu e-mail, aby wybrać link Potwierdź w wiadomości e-mail. Aby dowiedzieć się, jak to zrobić za pomocą konta e-mail [Outlook.com](http://outlook.com) , zobacz [ C# konfiguracja SMTP John ATTEN dla Outlook.com hosta SMTP](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) i jego[ASP.NET Identity 2,0: Konfigurowanie weryfikacji konta i wpisów autoryzacji dwuskładnikowej](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) .

Gdy użytkownik wybierze przycisk **zarejestruj** , wiadomość e-mail z potwierdzeniem zawierającą token walidacji jest wysyłana na adres e-mail.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Użytkownik wysyła wiadomość e-mail z tokenem potwierdzającym dla swojego konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Analizowanie kodu

Poniższy kod przedstawia metodę `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Metoda kończy się niepowodzeniem, jeśli adres e-mail użytkownika nie został potwierdzony. Jeśli błąd został ogłoszony dla nieprawidłowego adresu e-mail, złośliwi użytkownicy mogą użyć tych informacji w celu znalezienia prawidłowych userId (aliasów e-mail) do ataku.

Poniższy kod przedstawia metodę `ConfirmEmail` na kontrolerze konta, która jest wywoływana, gdy użytkownik wybierze link potwierdzenia w wysyłanej do nich wiadomości e-mail:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Gdy użyto zapomnianego tokenu hasła, zostanie on unieważniony. Następująca zmiana kodu w metodzie `Create` (w pliku *\_aplikacji Start\IdentityConfig.cs* ) powoduje, że tokeny wygasną w ciągu 3 godzin.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Po powyższym kodzie, zapomniane hasło i tokeny potwierdzenia wiadomości e-mail wygasną za 3 godziny. Domyślny `TokenLifespan` to jeden dzień.

Poniższy kod przedstawia metodę potwierdzenia wiadomości e-mail:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Aby zwiększyć bezpieczeństwo aplikacji, ASP.NET Identity obsługuje uwierzytelnianie dwuskładnikowe (funkcji 2FA). Zobacz [ASP.NET Identity 2,0: Konfigurowanie weryfikacji konta i autoryzacji dwuskładnikowej](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Jan ATTEN. Mimo że można ustawić blokadę konta dla nieudanych prób logowania, to podejście sprawia, że logowanie jest podatne na blokady [systemu DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Zalecamy używanie blokady konta tylko z funkcji 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Aplikacja MVC 5 z logowaniem w serwisach Facebook, Twitter, LinkedIn i Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) również pokazuje, jak dodać informacje o profilu do tabeli users.
- [ASP.NET MVC i Identity 2,0: zrozumienie podstaw](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez John ATTEN.
- [Wprowadzenie do produktu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Zapowiedź RTM ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez Pranav Rastogi.
