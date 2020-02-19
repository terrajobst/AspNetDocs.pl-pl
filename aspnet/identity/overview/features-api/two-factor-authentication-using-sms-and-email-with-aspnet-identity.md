---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail przy użyciu ASP.NET Identity-ASP.NET 4. x
author: HaoK
description: W tym samouczku przedstawiono sposób konfigurowania uwierzytelniania dwuskładnikowego (funkcji 2FA) przy użyciu wiadomości SMS i wiadomości e-mail. Ten artykuł został zapisany przez Rick Anderson (@RickAndMSFT), pr...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 527b4392846e60dae0b216fdeabf21fd6618e4d7
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456741"
---
# <a name="two-factorauthentication-using-sms-and-email-with-aspnet-identity"></a>Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail przy użyciu ASP.NET Identity

przez [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> W tym samouczku przedstawiono sposób konfigurowania uwierzytelniania dwuskładnikowego (funkcji 2FA) przy użyciu wiadomości SMS i wiadomości e-mail.
> 
> Ten artykuł został zapisany przez Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung i Suhas Joshi. Próbka NuGet została zapisywana głównie przez Hao Kung.

W tym temacie omówiono następujące zagadnienia:

- [Kompilowanie przykładu tożsamości](#build)
- [Konfigurowanie usługi SMS na potrzeby uwierzytelniania dwuskładnikowego](#SMS)
- [Włączanie uwierzytelniania dwuskładnikowego](#enable2)
- [Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego](#reg)
- [Łączenie kont logowania społecznościowych i lokalnych](#combine)
- [Blokada konta przed atakami polegającymi na rozsile](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Kompilowanie przykładu tożsamości

W tej sekcji użyjesz narzędzia NuGet do pobrania przykładu, z którym będziemy współpracować. Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszy.

> [!NOTE]
> Ostrzeżenie: aby ukończyć ten samouczek, musisz zainstalować program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) .

1. Utwórz nowy ***pusty*** projekt sieci Web ASP.NET.
2. W konsoli Menedżera pakietów wprowadź następujące polecenia:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   W tym samouczku będziemy używać [SendGrid](http://sendgrid.com/) do wysyłania wiadomości E-mail i [Twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) na potrzeby tekstu SMS. Pakiet `Identity.Samples` instaluje kod, z którym będziemy pracować.
3. Ustaw [dla projektu użycie protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcjonalnie*: Postępuj zgodnie z instrukcjami w [samouczku potwierdzającym pocztą e-mail](account-confirmation-and-password-recovery-with-aspnet-identity.md) , aby podłączyć program SendGrid, a następnie uruchomić aplikację i zarejestrować konto e-mail.
5. *Opcjonalne:* Usuń przykładowy kod potwierdzania linku do wiadomości e-mail z przykładu (kod `ViewBag.Link` na kontrolerze konta. Zobacz metody akcji `DisplayEmail` i `ForgotPasswordConfirmation` i widoki Razor).
6. *Opcjonalne:* Usuń kod `ViewBag.Status` z kontrolerów zarządzania i konta oraz z widoków *Views\Account\VerifyCode.cshtml* i *Views\Manage\VerifyPhoneNumber.cshtml* Razor. Możesz również zachować `ViewBag.Status` wyświetlania, aby sprawdzić, jak działa ta aplikacja lokalnie bez konieczności podłączania i wysyłania wiadomości e-mail i SMS.

> [!NOTE]
> Ostrzeżenie: Jeśli zmienisz dowolne z ustawień zabezpieczeń w tym przykładzie, aplikacje produkcji będą musiały zostać poddane inspekcji zabezpieczeń, która jawnie wywoła wprowadzone zmiany.

<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Konfigurowanie usługi SMS na potrzeby uwierzytelniania dwuskładnikowego

Ten samouczek zawiera instrukcje dotyczące korzystania z programu Twilio lub ASPSMS, ale można użyć dowolnego innego dostawcy programu SMS.

1. **Tworzenie konta użytkownika za pomocą dostawcy programu SMS**  
  
   Utwórz konto [Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .
2. **Instalowanie dodatkowych pakietów lub Dodawanie odwołań do usług**  
  
   Twilio  
   W konsoli Menedżera pakietów wprowadź następujące polecenie:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Należy dodać następujące odwołanie do usługi:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Ulica  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Przestrzeń nazw:  
    `ASPSMSX2`
3. **Rozprowadzenie poświadczeń użytkownika dostawcy programu SMS**  
  
   Twilio  
   Na karcie **pulpit nawigacyjny** konta usługi Twilio Skopiuj **Identyfikator SID konta** i **token uwierzytelniania**.  
  
   ASPSMS:  
   W ustawieniach konta przejdź do **userKey** i skopiuj go wraz z własnym określonym **hasłem**.  
  
   Te wartości zostaną później zapisane w zmiennych `SMSAccountIdentification` i `SMSAccountPassword`.
4. **Określanie SenderID/inicjatora**  
  
   Twilio  
   Na karcie **liczby** Skopiuj numer telefonu Twilio.  
  
   ASPSMS:  
   W menu **odblokowane odblokowywanie** Odblokuj co najmniej jeden inicjator lub wybierz inicjator alfanumeryczny (nieobsługiwany przez wszystkie sieci).  
  
   Ta wartość będzie później przechowywana w zmiennej `SMSAccountFrom`.
5. **Transferowanie poświadczeń dostawcy programu SMS do aplikacji**  
  
   Udostępnienie poświadczeń i numeru telefonu nadawcy dla aplikacji:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. Konto i poświadczenia są dodawane do powyższego kodu, aby zapewnić prostą przykład. Zobacz Jan ATTEN [ASP.NET MVC: Zachowaj ustawienia prywatne z kontroli źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementacja transferu danych do dostawcy programu SMS**  
  
   Skonfiguruj klasę `SmsService` w *aplikacji\_pliku Start\IdentityConfig.cs* .  
  
   W zależności od użytego dostawcy programu SMS Aktywuj **Twilio** lub sekcję **ASPSMS** : 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Uruchom aplikację i zaloguj się przy użyciu wcześniej zarejestrowanego konta.
8. Kliknij swój identyfikator użytkownika, który aktywuje metodę `Index` akcji w kontrolerze `Manage`.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Kliknij pozycję Add (Dodaj).  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. W ciągu kilku sekund otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź ją i naciśnij przycisk **Prześlij**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Widok zarządzanie pokazuje, że Twój numer telefonu został dodany.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Analizowanie kodu

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

Metoda akcji `Index` w kontrolerze `Manage` ustawia komunikat o stanie na podstawie poprzedniej akcji i zawiera linki umożliwiające zmianę hasła lokalnego lub dodanie konta lokalnego. W metodzie `Index` jest również wyświetlany stan lub numer telefonu funkcji 2FA, logowania zewnętrzne, funkcji 2FA włączone i Zapamiętaj funkcji 2FA metodę dla tej przeglądarki (wyjaśniono później). Kliknięcie identyfikatora użytkownika (poczty e-mail) na pasku tytułu nie powoduje przekazania komunikatu. Kliknięcie **numeru telefonu: Usuń** łącze passs `Message=RemovePhoneSuccess` jako ciąg zapytania.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Metoda akcji wyświetla okno dialogowe, w którym można wprowadzić numer telefonu, który może odbierać wiadomości SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Kliknięcie przycisku **Wyślij kod weryfikacyjny** spowoduje opublikowanie numeru telefonu w metodzie akcji post `AddPhoneNumber` http.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

Metoda `GenerateChangePhoneNumberTokenAsync` generuje token zabezpieczający, który zostanie ustawiony w wiadomości SMS. Jeśli usługa programu SMS została skonfigurowana, token jest wysyłany jako ciąg &quot;kod zabezpieczający &lt;token&gt;&quot;. Metoda `SmsService.SendAsync` do jest wywoływana asynchronicznie, a następnie aplikacja zostanie przekierowana do metody akcji `VerifyPhoneNumber` (która wyświetla następujące okno dialogowe), w której można wprowadzić kod weryfikacyjny.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Po wprowadzeniu kodu i kliknięciu przycisku Prześlij kod zostanie opublikowany w metodzie akcji POST protokołu HTTP `VerifyPhoneNumber`.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

Metoda `ChangePhoneNumberAsync` sprawdza opublikowany kod zabezpieczeń. Jeśli kod jest poprawny, numer telefonu zostanie dodany do pola `PhoneNumber` tabeli `AspNetUsers`. Jeśli to wywołanie zakończyło się pomyślnie, wywoływana jest metoda `SignInAsync`:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

Parametr `isPersistent` określa, czy sesja uwierzytelniania jest utrwalana w wielu żądaniach.

Po zmianie profilu zabezpieczeń jest generowana nowa Sygnatura zabezpieczeń, która będzie przechowywana w polu `SecurityStamp` tabeli *AspNetUsers* . Zwróć uwagę, że pole `SecurityStamp` różni się od pliku cookie zabezpieczeń. Plik cookie zabezpieczeń nie jest przechowywany w tabeli `AspNetUsers` (lub w innym miejscu w bazie danych tożsamości). Token pliku cookie zabezpieczeń jest z podpisem własnym przy użyciu funkcji [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i jest tworzony z informacjami o `UserId, SecurityStamp` i czasie wygaśnięcia.

Oprogramowanie pośredniczące cookie sprawdza plik cookie na każdym żądaniu. Metoda `SecurityStampValidator` w klasie `Startup` trafi do bazy danych i okresowo sprawdza sygnaturę zabezpieczeń, jak określono w `validateInterval`. Dzieje się to co 30 minut (w naszym przykładzie), chyba że zmienisz profil zabezpieczeń. Wybrano 30-minutowy interwał minimalizowania podróży do bazy danych.

Metodę `SignInAsync` należy wywołać w przypadku wprowadzenia zmiany w profilu zabezpieczeń. Gdy profil zabezpieczeń ulegnie zmianie, baza danych programu aktualizuje pole `SecurityStamp` i bez wywoływania metody `SignInAsync`, *tylko* do momentu, gdy potok Owin trafi do bazy danych (`validateInterval`). Można to przetestować, zmieniając metodę `SignInAsync`, aby zwracała się natychmiast, i ustawiając właściwość `validateInterval` plików cookie z 30 minut na 5 sekund:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Po wprowadzeniu powyższych zmian w kodzie można zmienić profil zabezpieczeń (na przykład zmieniając stan **dwóch włączonych**), a użytkownik zostanie wylogowany w ciągu 5 sekund, gdy metoda `SecurityStampValidator.OnValidateIdentity` nie powiedzie się. Usuń wiersz Return w metodzie `SignInAsync`, wprowadź inną zmianę profilu zabezpieczeń i nie będzie się wylogować. Metoda `SignInAsync` generuje nowy plik cookie zabezpieczeń.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Włączanie uwierzytelniania dwuskładnikowego

W przykładowej aplikacji należy użyć interfejsu użytkownika, aby włączyć uwierzytelnianie dwuskładnikowe (funkcji 2FA). Aby włączyć funkcji 2FA, kliknij identyfikator użytkownika (alias e-mail) na pasku nawigacyjnym.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Kliknij pozycję Włącz funkcji 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Wyloguj się, a następnie zaloguj się ponownie. Jeśli włączono obsługę poczty e-mail (Zobacz mój [poprzedni samouczek](account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać wiadomość SMS lub e-mail dla usługi funkcji 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Zostanie wyświetlona strona Sprawdź kod, na której można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Kliknięcie pola wyboru **Zapamiętaj tę przeglądarkę** spowoduje zwolnienie z używania funkcji 2FA do logowania się na tym komputerze i w przeglądarce. Włączenie funkcji 2FA i kliknięcie opcji Pamiętaj, że **Ta przeglądarka** zapewni mocną ochronę funkcji 2FA przed złośliwymi użytkownikami próbującymi uzyskać dostęp do Twojego konta, o ile nie mają dostępu do komputera. Można to zrobić na wszystkich maszynach prywatnych, których regularnie używasz. Ustawienie **Zapamiętaj tę przeglądarkę**, aby zwiększyć bezpieczeństwo funkcji 2FA z komputerów, które nie są regularnie używane, i uzyskać wygodę, aby nie mieć możliwości przechodzenia przez funkcji 2FA na własnych komputerach. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego

Podczas tworzenia nowego projektu MVC plik *IdentityConfig.cs* zawiera następujący kod, aby zarejestrować dostawcę uwierzytelniania dwuskładnikowego:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Dodaj numer telefonu do funkcji 2FA

Metoda akcji `AddPhoneNumber` na kontrolerze `Manage` generuje token zabezpieczający i wysyła go do podanego numeru telefonu.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Po wysłaniu tokenu przekierowuje do metody akcji `VerifyPhoneNumber`, w której można wprowadzić kod w celu zarejestrowania programu SMS for funkcji 2FA. Program SMS funkcji 2FA nie jest używany do momentu zweryfikowania numeru telefonu.

## <a name="enabling-2fa"></a>Włączanie funkcji 2FA

Metoda akcji `EnableTFA` umożliwia funkcji 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Należy zwrócić uwagę, że należy wywołać `SignInAsync`, ponieważ wartość Enable funkcji 2FA to zmiana profilu zabezpieczeń. Gdy funkcji 2FA jest włączona, użytkownik będzie musiał użyć usługi funkcji 2FA do zalogowania się przy użyciu funkcji 2FA podejścia do rejestracji (wiadomości SMS i wiadomości e-mail w przykładzie).

Możesz dodać więcej dostawców funkcji 2FA, takich jak generatory kodu QR, lub napisać użytkownika (zobacz [Korzystanie z usługi Google Authenticator z ASP.NET Identity](https://www.jerriepelser.com//blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Kody funkcji 2FA są generowane przy użyciu [algorytmu hasła jednorazowego opartego na czasie](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) , a kody są ważne przez sześć minut. W przypadku wprowadzenia kodu przez więcej niż 6 minut zostanie wyświetlony komunikat o błędzie nieprawidłowego kodu.

<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont logowania społecznościowych i lokalnych

Konta lokalne i społecznościowe można łączyć przez kliknięcie linku do poczty e-mail. W następującej kolejności &quot;RickAndMSFT@gmail.com&quot; jest najpierw tworzony jako logowanie lokalne, ale w pierwszej kolejności można utworzyć konto w trybie społecznościowym, a następnie dodać lokalną nazwę logowania.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Kliknij link **Zarządzaj** . Zwróć uwagę na wartość 0 External (logowanie społeczne) skojarzoną z tym kontem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Kliknij link do innej usługi logowania i zaakceptuj żądania aplikacji. Te dwa konta zostały połączone, ale będzie można zalogować się przy użyciu dowolnego konta. Możesz chcieć, aby użytkownicy mogli dodawać konta lokalne w przypadku, gdy ich usługa uwierzytelniania w sieci społecznościowej nie działa, lub prawdopodobnie utraciły dostęp do konta społecznościowego.

Na poniższej ilustracji, Tomasz to logowanie społecznościowe (które można zobaczyć od **zewnętrznych logowań: 1** pokazane na stronie).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Kliknięcie przycisku **Wybierz hasło** umożliwia dodanie lokalnego logowania skojarzonego z tym samym kontem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Blokada konta przed atakami polegającymi na rozsile

Możesz chronić konta w aplikacji przed atakami słownikowymi, włączając blokadę użytkownika. Następujący kod w metodzie `ApplicationUserManager Create` konfiguruje blokadę:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Powyższy kod włącza blokadę tylko do uwierzytelniania dwuskładnikowego. Można włączyć opcję Zablokuj dla logowań, zmieniając `shouldLockout` na true w metodzie `Login` kontrolera konta, zalecamy nie włączać blokowania dla logowań, ponieważ sprawia, że konto podatne na [ataki logowania.](http://en.wikipedia.org/wiki/Denial-of-service_attack) W przykładowym kodzie blokada jest wyłączona dla konta administratora utworzonego w metodzie `ApplicationDbInitializer Seed`:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Wymaganie od użytkownika posiadania zweryfikowanego konta e-mail

Poniższy kod wymaga, aby użytkownik miał zweryfikowane konto e-mail przed zalogowaniem się:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Jak SignInManager sprawdza wymagania funkcji 2FA

Logowanie lokalne i logowanie w sieci społecznościowej sprawdzają, czy funkcji 2FA jest włączona. Jeśli funkcji 2FA jest włączona, metoda logowania `SignInManager` zwraca `SignInStatus.RequiresVerification`i użytkownik zostanie przekierowany do metody akcji `SendCode`, gdzie będzie musiał wprowadzić kod, aby ukończyć sekwencję logowania. Jeśli użytkownik ma kontrolka rememberMe jest ustawiony w lokalnym pliku cookie użytkowników, `SignInManager` zwróci `SignInStatus.Success` i nie będzie musiał przejść przez funkcji 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Poniższy kod przedstawia metodę akcji `SendCode`. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest tworzony ze wszystkimi metodami funkcji 2FA włączonymi dla użytkownika. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest przenoszona do pomocnika [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) , dzięki czemu użytkownik może wybrać podejście funkcji 2FA (zazwyczaj wiadomości e-mail i SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Gdy użytkownik zaksięguje podejście funkcji 2FA, wywoływana jest metoda działania `HTTP POST SendCode`, `SignInManager` wysyła kod funkcji 2FA, a użytkownik zostaje przekierowany do metody akcji `VerifyCode`, gdzie mogą wprowadzić kod, aby zakończyć logowanie.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Blokada funkcji 2FA

Mimo że można ustawić blokadę konta dla nieudanych prób logowania, to podejście sprawia, że logowanie jest podatne na blokady [systemu DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Zalecamy używanie blokady konta tylko z funkcji 2FA. Po utworzeniu `ApplicationUserManager` przykładowy kod ustawia funkcji 2FA blokady i `MaxFailedAccessAttemptsBeforeLockout` na pięć. Po zalogowaniu się użytkownika (za pomocą konta lokalnego lub konta społecznościowego) każda niepomyślna próba w funkcji 2FA jest przechowywana i jeśli osiągnięto maksymalną liczbę prób, użytkownik zostanie zablokowany przez pięć minut (można ustawić czas blokowania na `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [ASP.NET Identity zalecane zasoby](../getting-started/aspnet-identity-recommended-resources.md) Pełna lista blogów, klipów wideo, samouczków i doskonałych elementów.
- [Aplikacja MVC 5 z logowaniem w serwisach Facebook, Twitter, LinkedIn i Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) również pokazuje, jak dodać informacje o profilu do tabeli users.
- [ASP.NET MVC i Identity 2,0: zrozumienie podstaw](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez John ATTEN.
- [Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Wprowadzenie do produktu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Zapowiedź RTM ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez Pranav Rastogi.
- [ASP.NET Identity 2,0: Konfigurowanie weryfikacji konta i autoryzacji dwuskładnikowej](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Jan ATTEN.
