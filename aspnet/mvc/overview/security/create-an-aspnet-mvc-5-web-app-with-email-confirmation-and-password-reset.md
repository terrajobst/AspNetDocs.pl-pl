---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Tworzenie aplikacji sieci Web Secure ASP.NET MVC 5 z logowaniem, potwierdzeniem wiadomości e-mail i resetowaniem hasła (C#) | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć aplikację internetową ASP.NET MVC 5 z potwierdzeniem poczty e-mail i resetowaniem hasła przy użyciu systemu członkostwa ASP.NET Identity. Twój urząd certyfikacji...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 07f5b290b73f75000e6f29fe09e4dc25e144452f
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74899694"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Tworzenie bezpiecznej aplikacji internetowej ASP.NET MVC 5 z logowaniem, potwierdzeniem adresu e-mail i resetowaniem hasła (C#)

Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))

W tym samouczku pokazano, jak utworzyć aplikację internetową ASP.NET MVC 5 z potwierdzeniem poczty e-mail i resetowaniem hasła przy użyciu systemu członkostwa ASP.NET Identity.

Aby uzyskać zaktualizowaną wersję tego samouczka korzystającego z platformy .NET Core, zobacz [potwierdzenie konta i odzyskiwanie hasła w ASP.NET Core [/ASPNET/Core/Security/Authentication/accconfirm).

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Tworzenie aplikacji ASP.NET MVC

Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.

> [!NOTE]
> Ostrzeżenie: aby ukończyć ten samouczek, musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.

1. Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC. Formularze sieci Web obsługują również ASP.NET Identity, więc można wykonać podobne kroki w aplikacji formularzy sieci Web.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Pozostaw domyślne uwierzytelnianie jako **pojedyncze konta użytkowników**. Jeśli chcesz hostować aplikację na platformie Azure, pozostaw zaznaczone pole wyboru. W dalszej części samouczka zostanie wdrożona na platformie Azure. Możesz [bezpłatnie otworzyć konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ustaw [dla projektu użycie protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Uruchom aplikację, kliknij link **zarejestruj** i zarejestruj użytkownika. W tym momencie jedynym sprawdzaniem poprawności w wiadomości e-mail jest atrybut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
5. W Eksplorator serwera przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz polecenie **Otwórz definicję tabeli**.

    Na poniższej ilustracji przedstawiono schemat `AspNetUsers`:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Kliknij prawym przyciskiem myszy tabelę **AspNetUsers** , a następnie wybierz pozycję **Pokaż dane tabeli**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 W tym momencie wiadomość e-mail nie została potwierdzona.
7. Kliknij wiersz i wybierz pozycję Usuń. Ponownie dodasz tę wiadomość e-mail w następnym kroku i wyślesz wiadomość e-mail z potwierdzeniem.

## <a name="email-confirmation"></a>Potwierdzenie wiadomości e-mail

Najlepszym rozwiązaniem jest potwierdzenie wiadomości e-mail nowej rejestracji użytkownika w celu zweryfikowania, że nie są one personifikowane przez kogoś innego (oznacza to, że nie zostały zarejestrowane w wiadomości e-mail innej osoby). Załóżmy, że masz forum dyskusyjne, aby zapobiec rejestrowaniu się `"bob@example.com"` w `"joe@contoso.com"`. Bez potwierdzenia wiadomości e-mail `"joe@contoso.com"` może pobrać niechcianą wiadomość e-mail z aplikacji. Załóżmy, że Robert przypadkowo zarejestrowany jako `"bib@example.com"` i nie go, nie będzie mógł korzystać z odzyskiwania hasła, ponieważ aplikacja nie ma poprawnego adresu e-mail. Potwierdzenie poczty e-mail zapewnia tylko ograniczoną ochronę z botów i nie zapewnia ochrony przed określonymi nadawców, którzy mają wiele roboczych aliasów poczty e-mail, których mogą używać do rejestracji.

Zazwyczaj chcesz uniemożliwić nowym użytkownikom ogłaszanie danych w witrynie sieci Web przed ich potwierdzeniem za pośrednictwem poczty e-mail, wiadomości SMS lub innego mechanizmu. <a id="build"></a>W poniższych sekcjach zostanie włączone potwierdzenie poczty e-mail i zmodyfikujesz kod, aby uniemożliwić nowo zarejestrowanym użytkownikom logowanie się do momentu potwierdzenia potwierdzenia wiadomości e-mail.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Podłącz do SendGrid

Instrukcje w tej sekcji nie są aktualne. Aby uzyskać zaktualizowane instrukcje, zobacz [Konfigurowanie dostawcy poczty E-mail SendGrid](/aspnet/core/security/authentication/accconfirm#configure-email-provider) .

Chociaż w tym samouczku pokazano, jak dodać powiadomienie e-mail za pośrednictwem usługi [SendGrid](http://sendgrid.com/), możesz wysłać wiadomość e-mail przy użyciu protokołu SMTP i innych mechanizmów (zobacz [dodatkowe zasoby](#addRes)).

1. W konsoli Menedżera pakietów wprowadź następujące polecenie: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Przejdź do [strony rejestracji w usłudze Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) i zarejestruj się, aby skorzystać z bezpłatnego konta SendGrid. Skonfiguruj SendGrid przez dodanie kodu podobnego do poniższego w *App_Start/identityconfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Należy dodać następujące elementy:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Aby zachować ten przykład, zapiszemy ustawienia aplikacji w pliku *Web. config* :

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. Konto i poświadczenia są przechowywane w element appSetting. Na platformie Azure możesz bezpiecznie przechowywać te wartości na karcie **[Konfiguracja](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** w Azure Portal. Zapoznaj [się z najlepszymi rozwiązaniami dotyczącymi wdrażania haseł i innych poufnych danych w ASP.NET i na platformie Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

### <a name="enable-email-confirmation-in-the-account-controller"></a>Włącz potwierdzenie poczty e-mail na kontrolerze konta

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Sprawdź, czy plik *Views\Account\ConfirmEmail.cshtml* ma poprawną składnię Razor. (Znak @ w pierwszym wierszu może być niedostępny. ).

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Uruchom aplikację i kliknij link Zarejestruj. Po przesłaniu formularza rejestracji użytkownik jest zalogowany.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Sprawdź konto e-mail i kliknij link, aby potwierdzić swój adres e-mail.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Wymagaj potwierdzenia wiadomości e-mail przed zalogowaniem

Obecnie po zakończeniu procesu rejestracji użytkownik jest zalogowany. Zazwyczaj chcesz potwierdzić swój adres e-mail przed zarejestrowaniem ich w usłudze. W poniższej sekcji zmodyfikujemy kod, aby wymagać od nowych użytkowników potwierdzenia wiadomości e-mail przed ich zalogowaniem się (uwierzytelniony). Zaktualizuj metodę `HttpPost Register` przy użyciu następujących wyróżnionych zmian:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Poprzez komentowanie metody `SignInAsync` użytkownik nie będzie zalogowany przez rejestrację. Wiersz `TempData["ViewBagLink"] = callbackUrl;` może służyć do [debugowania aplikacji](#dbg) i testowania rejestracji bez wysyłania wiadomości e-mail. `ViewBag.Message` jest używany do wyświetlania instrukcji Confirm. [Przykład pobierania](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) zawiera kod służący do testowania potwierdzenia wiadomości e-mail bez konfigurowania poczty e-mail i może być również używany do debugowania aplikacji.

Utwórz plik `Views\Shared\Info.cshtml` i Dodaj następujący znacznik Razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Dodaj [atrybut Autoryzuj](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) do metody akcji `Contact` kontrolera głównego. Możesz kliknąć link **kontakt** , aby sprawdzić, czy użytkownicy anonimowi nie mają dostępu, a uwierzytelniony użytkownicy mają dostęp.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Należy również zaktualizować metodę `HttpPost Login` akcji:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Zaktualizuj widok *Views\Shared\Error.cshtml* , aby wyświetlić komunikat o błędzie:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Usuń wszystkie konta z tabeli **AspNetUsers** zawierające alias adresu e-mail, z którym chcesz przeprowadzić test. Uruchom aplikację i sprawdź, czy nie możesz się zalogować, dopóki nie potwierdzisz adresu e-mail. Po potwierdzeniu adresu e-mail kliknij link **kontaktowy** .

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Odzyskiwanie/Resetowanie hasła

Usuń znaki komentarza z `HttpPost ForgotPassword`ej metody akcji na kontrolerze konta:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Usuń znaki komentarza z `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) w pliku widoku Razor *Views\Account\Login.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Na stronie logowania zostanie teraz wyświetlony link służący do resetowania hasła.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Wyślij ponownie link do potwierdzenia wiadomości e-mail

Po utworzeniu nowego konta lokalnego przez użytkownika są wysyłane pocztą e-mail link potwierdzający, którego należy użyć przed zalogowaniem się. Jeśli użytkownik przypadkowo usunie wiadomość e-mail z potwierdzeniem lub wiadomość e-mail nie dotarła do Ciebie, będzie potrzebować linku potwierdzenia wysłanej ponownie. Poniższe zmiany kodu pokazują, jak to włączyć.

Dodaj następującą metodę pomocnika na końcu pliku *Controllers\AccountController.cs* :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Zaktualizuj metodę Register, aby korzystała z nowego pomocnika:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Zaktualizuj metodę logowania, aby ponownie wysłać hasło, jeśli konto użytkownika nie zostało potwierdzone:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont logowania społecznościowych i lokalnych

Konta lokalne i społecznościowe można łączyć przez kliknięcie linku do poczty e-mail. W następującej kolejności **RickAndMSFT@gmail.com** jest najpierw tworzony jako logowanie lokalne, ale w pierwszej kolejności można utworzyć konto w trybie społecznościowym, a następnie dodać lokalną nazwę logowania.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Kliknij link **Zarządzaj** . Zwróć uwagę na **zewnętrzne nazwy logowania: 0** skojarzonych z tym kontem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Kliknij link do innej usługi logowania i zaakceptuj żądania aplikacji. Te dwa konta zostały połączone, ale będzie można zalogować się przy użyciu dowolnego konta. Możesz chcieć, aby użytkownicy mogli dodawać konta lokalne w przypadku, gdy ich usługa uwierzytelniania w sieci społecznościowej nie działa, lub prawdopodobnie utraciły dostęp do konta społecznościowego.

Na poniższej ilustracji, Tomasz to logowanie społecznościowe (które można zobaczyć od **zewnętrznych logowań: 1** pokazane na stronie).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Kliknięcie przycisku **Wybierz hasło** umożliwia dodanie lokalnego logowania skojarzonego z tym samym kontem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Bardziej szczegółowe potwierdzenie wiadomości e-mail

W tym temacie znajdują się informacje o [potwierdzeniu konta i odzyskiwaniu hasła z użyciem ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) .

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debugowanie aplikacji

Jeśli nie otrzymasz wiadomości e-mail zawierającej link:

- Sprawdź folder wiadomości-śmieci lub spamu.
- Zaloguj się do konta SendGrid, a następnie kliknij [link działania e-mail](https://sendgrid.com/logs/index).

Aby przetestować łącze weryfikacji bez poczty e-mail, Pobierz [ukończony przykład](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Na stronie zostanie wyświetlony link potwierdzenia i kody potwierdzające.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Linki do ASP.NET Identity zalecanych zasobów](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Więcej szczegółów na temat odzyskiwania hasła i potwierdzania konta.
- [Aplikacja MVC 5 z logowaniem w serwisach Facebook, Twitter, LinkedIn i Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) W tym samouczku pokazano, jak napisać aplikację ASP.NET MVC 5 z autoryzacją Facebook i usługą Google OAuth 2. Pokazano również, jak dodać dodatkowe dane do bazy danych tożsamości.
- [Wdróż aplikację Secure ASP.NET MVC z członkostwem, uwierzytelnianiem OAuth i SQL Database na platformie Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). W tym samouczku dodano wdrożenie platformy Azure, sposób zabezpieczania aplikacji za pomocą ról, jak używać interfejsu API członkostwa do dodawania użytkowników i ról oraz dodatkowych funkcji zabezpieczeń.
- [Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji z projektem](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji z projektem](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Konfigurowanie protokołu SSL w projekcie](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
