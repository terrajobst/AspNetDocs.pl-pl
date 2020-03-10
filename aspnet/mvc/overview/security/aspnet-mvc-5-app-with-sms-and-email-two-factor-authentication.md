---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplikacja ASP.NET MVC 5 z uwierzytelnianiem dwuskładnikowym za pomocą wiadomości SMS i wiadomości e-mail | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 5 z uwierzytelnianiem dwuskładnikowym. Należy ukończyć tworzenie aplikacji sieci Web Secure ASP.NET MVC 5 za pomocą...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538445"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplikacja ASP.NET MVC 5 z uwierzytelnianiem dwuskładnikowym za pomocą wiadomości SMS i wiadomości e-mail

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 5 z uwierzytelnianiem dwuskładnikowym. Aby móc kontynuować [, należy utworzyć aplikację sieci Web z funkcją logowania przy użyciu hasła i potwierdzenia poczty e-mail](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) . Ukończoną aplikację można pobrać [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Pobieranie zawiera pomocników debugowania, które umożliwiają testowanie potwierdzania wiadomości e-mail i wiadomości SMS bez konieczności konfigurowania dostawcy poczty e-mail lub programu SMS.
> 
> Ten samouczek został utworzony przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

- [Tworzenie aplikacji ASP.NET MVC](#createMvc)
- [Konfigurowanie usługi SMS na potrzeby uwierzytelniania dwuskładnikowego](#SMS)
- [Włączanie uwierzytelniania dwuskładnikowego](#enable2)
- [Dodatkowe zasoby](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Tworzenie aplikacji ASP.NET MVC

Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.

> [!NOTE]
> Ostrzeżenie: należy ukończyć [tworzenie bezpiecznej aplikacji sieci Web MVC 5 z logowaniem, potwierdzenie poczty e-mail i resetowanie hasła](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem. Aby ukończyć ten samouczek, musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.

1. Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC. Formularze sieci Web obsługują również ASP.NET Identity, więc można wykonać podobne kroki w aplikacji formularzy sieci Web.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Pozostaw domyślne uwierzytelnianie jako **pojedyncze konta użytkowników**. Jeśli chcesz hostować aplikację na platformie Azure, pozostaw zaznaczone pole wyboru. W dalszej części samouczka zostanie wdrożona na platformie Azure. Możesz [bezpłatnie otworzyć konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ustaw [dla projektu użycie protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Ulica  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Przestrzeń nazw:  
    `ASPSMSX2`
3. **Rozprowadzenie poświadczeń użytkownika dostawcy programu SMS**  
  
   Twilio  
   Na karcie **pulpit nawigacyjny** konta usługi Twilio Skopiuj **Identyfikator SID konta** i **token uwierzytelniania**.  
  
   ASPSMS:  
   W ustawieniach konta przejdź do **userKey** i skopiuj go wraz z własnym określonym **hasłem**.  
  
   Te wartości zostaną później zapisane w pliku *Web. config* w kluczach `"SMSAccountIdentification"` i `"SMSAccountPassword"`.
4. **Określanie SenderID/inicjatora**  
  
   Twilio  
   Na karcie **liczby** Skopiuj numer telefonu Twilio.  
  
   ASPSMS:  
   W menu **odblokowane odblokowywanie** Odblokuj co najmniej jeden inicjator lub wybierz inicjator alfanumeryczny (nieobsługiwany przez wszystkie sieci).  
  
   Ta wartość zostanie później przechowana w pliku *Web. config* w ramach klucza `"SMSAccountFrom"`.
5. **Transferowanie poświadczeń dostawcy programu SMS do aplikacji**  
  
   Udostępnienie poświadczeń i numeru telefonu nadawcy dla aplikacji. Aby zachować prostotę, należy przechowywać te wartości w pliku *Web. config* . Po wdrożeniu na platformie Azure można bezpiecznie przechowywać wartości w sekcji **Ustawienia aplikacji** na karcie Konfiguracja witryny sieci Web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. Konto i poświadczenia są dodawane do powyższego kodu, aby zapewnić prostą przykład. Zapoznaj [się z najlepszymi rozwiązaniami dotyczącymi wdrażania haseł i innych poufnych danych w ASP.NET i na platformie Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementacja transferu danych do dostawcy programu SMS**  
  
   Skonfiguruj klasę `SmsService` w *aplikacji\_pliku Start\IdentityConfig.cs* .  
  
   W zależności od użytego dostawcy programu SMS Aktywuj **Twilio** lub sekcję **ASPSMS** : 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aktualizowanie widoku Razor *Views\Manage\Index.cshtml* : (Uwaga: nie usuwaj po prostu komentarzy w kodzie wyjściowym, użyj poniższego kodu).  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Sprawdź, czy `EnableTwoFactorAuthentication` i `DisableTwoFactorAuthentication` metody akcji w `ManageController` mają atrybut[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Uruchom aplikację i zaloguj się przy użyciu wcześniej zarejestrowanego konta.
10. Kliknij swój identyfikator użytkownika, który aktywuje metodę `Index` akcji w kontrolerze `Manage`.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Kliknij przycisk Dodaj.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Metoda akcji wyświetla okno dialogowe, w którym można wprowadzić numer telefonu, który może odbierać wiadomości SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. W ciągu kilku sekund otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź ją i naciśnij przycisk **Prześlij**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Widok zarządzanie pokazuje, że Twój numer telefonu został dodany.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Włączanie uwierzytelniania dwuskładnikowego

W aplikacji wygenerowanej przez szablon należy użyć interfejsu użytkownika, aby włączyć uwierzytelnianie dwuskładnikowe (funkcji 2FA). Aby włączyć funkcji 2FA, kliknij identyfikator użytkownika (alias e-mail) na pasku nawigacyjnym.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Kliknij pozycję Włącz funkcji 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Wyloguj się, a następnie zaloguj się ponownie. Jeśli włączono obsługę poczty e-mail (Zobacz mój [poprzedni samouczek](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać wiadomość SMS lub e-mail dla usługi funkcji 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Zostanie wyświetlona strona Sprawdź kod, na której można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Kliknięcie pola wyboru **Zapamiętaj tę przeglądarkę** spowoduje zwolnienie z używania funkcji 2FA do zalogowania się przy użyciu przeglądarki i urządzenia, w której zaznaczono pole. Tak długo, jak Złośliwi użytkownicy nie mogą uzyskać dostępu do urządzenia, Włączanie funkcji 2FA i klikanie opcji **Zapamiętaj, że ta przeglądarka** zapewnia wygodny dostęp do jednego kroku, przy zachowaniu silnej ochrony funkcji 2FA w celu uzyskania dostępu z niezaufanych urządzeń. Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane.

Ten samouczek zawiera krótkie wprowadzenie do włączania funkcji 2FA na nowej aplikacji ASP.NET MVC. Mój samouczek [dwa-Factor Authentication za pomocą wiadomości SMS i wiadomości e-mail z ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) są szczegółowymi informacjami na temat kodu znajdującego się za przykładem.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail przy użyciu ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Szczegółowe informacje na temat uwierzytelniania dwuskładnikowego
- [Linki do ASP.NET Identity zalecanych zasobów](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Więcej szczegółów na temat odzyskiwania hasła i potwierdzania konta.
- [Aplikacja MVC 5 z logowaniem w serwisach Facebook, Twitter, LinkedIn i Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) W tym samouczku pokazano, jak napisać aplikację ASP.NET MVC 5 z autoryzacją Facebook i usługą Google OAuth 2. Pokazano również, jak dodać dodatkowe dane do bazy danych tożsamości.
- [Wdróż aplikację Secure ASP.NET MVC z członkostwem, uwierzytelnianiem OAuth i SQL Database w sieci Web platformy Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). W tym samouczku dodano wdrożenie platformy Azure, sposób zabezpieczania aplikacji za pomocą ról, jak używać interfejsu API członkostwa do dodawania użytkowników i ról oraz dodatkowych funkcji zabezpieczeń.
- [Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji z projektem](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji z projektem](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Konfigurowanie protokołu SSL w projekcie](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [Jak skonfigurować środowisko deweloperskie C# i ASP.NET MVC](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
