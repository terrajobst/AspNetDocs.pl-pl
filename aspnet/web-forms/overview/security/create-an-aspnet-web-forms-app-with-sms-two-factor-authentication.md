---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Tworzenie aplikacji ASP.NET Web Forms przy użyciu uwierzytelniania dwuskładnikowego SMS (C#) | Microsoft Docs
author: Erikre
description: W tym samouczku przedstawiono sposób tworzenia aplikacji ASP.NET Web Forms przy użyciu uwierzytelniania dwuskładnikowego. Ten samouczek został zaprojektowany, aby uzupełnić samouczek z tytułem CR...
ms.author: riande
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c9558aca8a655071c0c94ed66433cf721f26c011
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77466429"
---
# <a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Tworzenie aplikacji ASP.NET Web Forms z uwierzytelnianiem dwuskładnikowym za pomocą wiadomości SMS (C#)

Autor [Erik Reitan](https://github.com/Erikre)

[Pobieranie aplikacji ASP.NET Web Forms przy użyciu uwierzytelniania dwuskładnikowego poczty E-mail i wiadomości SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> W tym samouczku przedstawiono sposób tworzenia aplikacji ASP.NET Web Forms przy użyciu uwierzytelniania dwuskładnikowego. Ten samouczek został zaprojektowany, aby uzupełnić Samouczek zatytułowany [tworzenie bezpiecznej aplikacji ASP.NET Web Forms z rejestracją użytkownika, potwierdzeniem wiadomości e-mail i resetowaniem hasła](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Ponadto ten samouczek został oparty na [samouczku MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)Rick Anderson.

## <a name="introduction"></a>Wprowadzenie

Ten samouczek przeprowadzi Cię przez kroki wymagane do utworzenia aplikacji ASP.NET Web Forms, która obsługuje uwierzytelnianie dwuskładnikowe przy użyciu programu Visual Studio. Uwierzytelnianie dwuskładnikowe to dodatkowy krok uwierzytelniania użytkownika. Ten dodatkowy krok generuje unikatowy osobisty numer identyfikacyjny (PIN) podczas logowania. KOD PIN jest często wysyłany do użytkownika jako wiadomość e-mail lub wiadomości SMS. Użytkownik aplikacji wprowadza kod PIN jako dodatkową miarę uwierzytelniania podczas logowania.

### <a name="tutorial-tasks-and-information"></a>Zadania i informacje o samouczku:

- [Tworzenie aplikacji ASP.NET Web Forms](#createWebForms)
- [Konfigurowanie programu SMS i uwierzytelniania dwuskładnikowego](#SMS)
- [Włączanie uwierzytelniania dwuskładnikowego dla zarejestrowanego użytkownika](#use2FA)
- [Dodatkowe zasoby](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Tworzenie aplikacji ASP.NET Web Forms

Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 aktualizację Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszą. Należy również utworzyć konto [Twilio](https://www.twilio.com/try-twilio) , jak wyjaśniono poniżej.

> [!NOTE]
> Ważne: aby ukończyć ten samouczek, musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.

1. Utwórz nowy projekt ( -**plik** &gt; **Nowy projekt**) i wybierz szablon **aplikacji sieci Web ASP.NET** wraz z .NET Framework wersji 4.5.2 z okna dialogowego **Nowy projekt** .
2. W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **formularze sieci Web** . Pozostaw domyślne uwierzytelnianie jako **pojedyncze konta użytkowników**. Następnie kliknij przycisk **OK** , aby utworzyć nowy projekt.  
    okno dialogowe ![nowego projektu ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Włącz SSL (SSL) dla projektu. Postępuj zgodnie z instrukcjami w sekcji **Włączanie protokołu SSL dla projektu** w [wprowadzenie z serii samouczków formularzy sieci Web](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. W programie Visual Studio Otwórz **konsolę Menedżera pakietów** (**Narzędzia** -&gt; menedżer **pakietów NuGet** -&gt; **konsoli Menedżera pakietów**), a następnie wprowadź następujące polecenie:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Konfigurowanie programu SMS i uwierzytelniania dwuskładnikowego

W tym samouczku jest używany program Twilio, ale można użyć dowolnego dostawcy programu SMS.

1. Utwórz konto [Twilio](https://www.twilio.com/try-twilio) .
2. Na karcie **pulpit nawigacyjny** konta usługi Twilio Skopiuj **Identyfikator SID konta** i **token uwierzytelniania.** Później dodasz je do aplikacji.
3. Na karcie **liczby** Skopiuj również **numer telefonu** Twilio.
4. Udostępnij **Identyfikator SID konta**Twilio, **token uwierzytelniania** i **numer telefonu** dla aplikacji. Aby zachować prostotę, te wartości będą przechowywane w pliku *Web. config* . W przypadku wdrażania na platformie Azure można bezpiecznie przechowywać wartości w sekcji **AppSettings** na karcie Konfiguracja witryny sieci Web. Ponadto przy dodawaniu numeru telefonu należy używać tylko cyfr.   
   Zwróć uwagę, że możesz również dodać poświadczenia SendGrid. SendGrid to usługa powiadomień e-mail. Aby uzyskać szczegółowe informacje o sposobie włączania usługi SendGrid, zobacz sekcję "spining up SendGrid" w samouczku zatytułowanym [Create a Secure ASP.NET Web Forms app with User Registration, potwierdzenie poczty e-mail i resetowanie hasła.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. W tym przykładzie konto i poświadczenia są przechowywane w sekcji **AppSettings** w pliku *Web. config* . Na platformie Azure możesz bezpiecznie przechowywać te wartości na karcie **[Konfiguracja](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** w Azure Portal. Aby uzyskać powiązane informacje, zobacz temat Rick Anderson z tytułu [najlepszych rozwiązań dotyczących wdrażania haseł i innych poufnych danych w ASP.NET i na platformie Azure](/aspnet/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
5. Skonfiguruj klasę `SmsService` w *aplikacji\_pliku Start\IdentityConfig.cs* , wprowadzając następujące zmiany wyróżnione kolorem żółtym: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Dodaj następujące instrukcje `using` na początku pliku *IdentityConfig.cs* : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Zaktualizuj plik *Account/manage. aspx* , usuwając wiersze wyróżnione kolorem żółtym:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. W `Page_Load` procedury obsługi kodu *manage.aspx.cs* , Usuń komentarz z wiersza kodu wyróżnionego kolorem żółtym, tak aby pojawił się w następujący sposób: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. W CodeBehind *konta*/*TwoFactorAuthenticationSignIn.aspx.cs*zaktualizuj procedurę obsługi `Page_Load`, dodając następujący kod w żółtym: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Wprowadzając powyższe zmiany w kodzie, DropDownList "dostawcy" z opcjami uwierzytelniania nie będzie resetowany do pierwszej wartości. Dzięki temu użytkownik może pomyślnie wybrać wszystkie opcje, które będą używane podczas uwierzytelniania, a nie tylko w pierwszej kolejności.
10. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *default. aspx* i wybierz pozycję **Ustaw jako stronę startową**.
11. Testując aplikację, najpierw skompiluj aplikację (**Ctrl**+**SHIFT**+**B**), a następnie uruchom aplikację (**F5**) i wybierz pozycję **zarejestruj** , aby utworzyć nowe konto użytkownika, lub wybierz opcję **Zaloguj** , jeśli konto użytkownika zostało już zarejestrowane.
12. Gdy użytkownik (jako użytkownik) zalogował się, kliknij na pasku nawigacyjnym identyfikator użytkownika (adres e-mail), aby wyświetlić stronę **Zarządzanie kontem** (zarządzanie. aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Kliknij przycisk **Dodaj** obok pozycji **numer telefonu** na stronie **Zarządzanie kontem** .  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Dodaj numer telefonu, na który użytkownik chce otrzymywać wiadomości SMS (wiadomości tekstowe), a następnie kliknąć przycisk **Prześlij** .   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    W tym momencie aplikacja będzie używać poświadczeń z *pliku Web. config* do kontaktowania się z usługą Twilio. Komunikat SMS (wiadomość tekstowa) zostanie wysłany do telefonu skojarzonego z kontem użytkownika. Aby sprawdzić, czy komunikat Twilio został wysłany, można wyświetlić pulpit nawigacyjny Twilio.
15. W ciągu kilku sekund telefon skojarzony z kontem użytkownika otrzyma wiadomość tekstową zawierającą kod weryfikacyjny. Wprowadź kod weryfikacyjny i naciśnij przycisk **Prześlij**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Włączanie uwierzytelniania dwuskładnikowego dla zarejestrowanego użytkownika

Na tym etapie dla aplikacji włączono uwierzytelnianie dwuskładnikowe. Aby użytkownik mógł korzystać z uwierzytelniania dwuskładnikowego, może po prostu zmienić ustawienia za pomocą interfejsu użytkownika. 

1. Jako użytkownik aplikacji możesz włączyć uwierzytelnianie dwuskładnikowe dla danego konta, klikając na pasku nawigacyjnym nazwę użytkownika (alias adresu e-mail), aby wyświetlić stronę **Zarządzanie kontem** . Następnie kliknij link **Włącz** , aby włączyć uwierzytelnianie dwuskładnikowe dla konta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Wyloguj się, a następnie zaloguj się ponownie. Jeśli włączono opcję Poczta e-mail, możesz wybrać opcję Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS lub poczty e-mail. Jeśli nie włączono poczty e-mail, zapoznaj się z samouczkiem [tworzenie bezpiecznej aplikacji ASP.NET Web Forms z rejestracją użytkownika, potwierdzeniem wiadomości e-mail i resetowaniem hasła](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Zostanie wyświetlona strona uwierzytelniania dwuetapowego, na której można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Kliknięcie pola wyboru **Zapamiętaj tę przeglądarkę** spowoduje zwolnienie z konieczności używania uwierzytelniania dwuskładnikowego w celu zalogowania się przy użyciu przeglądarki i urządzenia, gdzie zaznaczono pole. Tak długo, jak Złośliwi użytkownicy nie mogą uzyskać dostępu do urządzenia, co umożliwia uwierzytelnianie dwuskładnikowe i klikanie opcji **Zapamiętaj, że ta przeglądarka** zapewnia wygodny dostęp do jednego kroku, przy jednoczesnym zachowaniu silnej ochrony uwierzytelniania dwuskładnikowego dla całego dostępu z niezaufanych urządzeń. Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Linki do ASP.NET Identity zalecanych zasobów](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Wdrażanie bezpiecznej aplikacji ASP.NET Web Forms z członkostwem, uwierzytelnianiem OAuth i SQL Database w witrynie sieci Web platformy Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Seria samouczka formularzy sieci Web ASP.NET — Dodawanie dostawcy OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Seria samouczka formularzy sieci Web ASP.NET — włączenie protokołu SSL dla projektu](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji z projektem](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
