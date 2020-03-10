---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Utwórz bezpieczną aplikację ASP.NET Web Forms z rejestracją użytkownika, potwierdzeniem wiadomości e-mail i resetowaniem hasła (C#) | Microsoft Docs
author: Erikre
description: W tym samouczku pokazano, jak utworzyć aplikację ASP.NET Web Forms z rejestracją użytkownika, potwierdzeniem adresu e-mail i resetowaniem hasła przy użyciu elementu członkowskiego ASP.NET Identity...
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: af3653bc164810126bc3bf8f1b1794d75642d807
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625154"
---
# <a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Tworzenie bezpiecznej aplikacji ASP.NET Web Forms z rejestracją użytkownika, potwierdzeniem adresu e-mail i resetowaniem hasła (C#)

Autor [Erik Reitan](https://github.com/Erikre)

> W tym samouczku pokazano, jak utworzyć aplikację ASP.NET Web Forms z rejestracją użytkownika, potwierdzeniem adresu e-mail i resetowaniem hasła przy użyciu systemu członkostwa ASP.NET Identity. Ten samouczek został oparty na [samouczku MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)Rick Anderson.

## <a name="introduction"></a>Wprowadzenie

Ten samouczek przeprowadzi Cię przez kroki wymagane do utworzenia aplikacji ASP.NET Web Forms przy użyciu programu Visual Studio i ASP.NET 4,5, aby utworzyć bezpieczną aplikację formularzy sieci Web z rejestracją użytkownika, potwierdzeniem adresu e-mail i resetowaniem hasła.

### <a name="tutorial-tasks-and-information"></a>Zadania i informacje o samouczku:

- [Tworzenie aplikacji ASP.NET Web Forms](#createWebForms)
- [Podłącz do SendGrid](#SG)
- [Wymagaj potwierdzenia wiadomości E-mail przed zalogowaniem](#require)
- [Odzyskiwanie i resetowanie hasła](#reset)
- [Wyślij ponownie link do potwierdzenia wiadomości E-mail](#rsend)
- [Rozwiązywanie problemów z aplikacją](#dbg)
- [Dodatkowe zasoby](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Tworzenie aplikacji ASP.NET Web Forms

Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 aktualizację Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszą.

> [!NOTE]
> Ostrzeżenie: aby ukończyć ten samouczek, musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.

1. Utwórz nowy projekt ( -**pliku** &gt; **Nowy projekt**) i wybierz szablon **aplikacji sieci Web ASP.NET** oraz najnowszą wersję .NET Framework z okna dialogowego **Nowy projekt** .
2. W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **formularze sieci Web** . Pozostaw domyślne uwierzytelnianie jako **pojedyncze konta użytkowników**. Jeśli chcesz hostować aplikację na platformie Azure, pozostaw zaznaczone pole wyboru **host w chmurze** .   
 Następnie kliknij przycisk **OK** , aby utworzyć nowy projekt.  
    okno dialogowe ![nowego projektu ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Włącz SSL (SSL) dla projektu. Postępuj zgodnie z instrukcjami w sekcji **Włączanie protokołu SSL dla projektu** w [wprowadzenie z serii samouczków formularzy sieci Web](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Uruchom aplikację, kliknij link **zarejestruj** i Zarejestruj nowego użytkownika. W tym momencie jedynym sprawdzaniem poprawności na adresie e-mail jest oparty na atrybucie [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) , aby upewnić się, że adres e-mail jest poprawnie sformułowany. Zmodyfikujesz kod, aby dodać potwierdzenie wiadomości e-mail. Zamknij okno przeglądarki.
5. W **Eksplorator serwera** programu Visual Studio (**wyświetl** -&gt; **Eksplorator serwera**), przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz polecenie **Otwórz definicję tabeli**. 

    Na poniższej ilustracji przedstawiono schemat tabeli `AspNetUsers`:

    ![Schemat tabeli AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. W **Eksplorator serwera**kliknij prawym przyciskiem myszy tabelę **AspNetUsers** , a następnie wybierz polecenie **Pokaż dane tabeli**.  
  
    ![Dane tabeli AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 W tym momencie wiadomość e-mail dla zarejestrowanego użytkownika nie została potwierdzona.
7. Kliknij wiersz i wybierz pozycję Usuń, aby usunąć użytkownika. Ponownie dodasz tę wiadomość e-mail w następnym kroku i wyślesz komunikat z potwierdzeniem do adresu e-mail.

## <a name="email-confirmation"></a>Potwierdzenie wiadomości e-mail

Najlepszym rozwiązaniem jest potwierdzenie wiadomości e-mail podczas rejestracji nowego użytkownika w celu zweryfikowania, że nie są one personifikowane przez kogoś innego (oznacza to, że nie zostały zarejestrowane w wiadomości e-mail innej osoby). Załóżmy, że masz forum dyskusyjne, aby zapobiec rejestrowaniu się `"bob@cpandl.com"` w `"joe@contoso.com"`. Bez potwierdzenia wiadomości e-mail `"joe@contoso.com"` może pobrać niechcianą wiadomość e-mail z aplikacji. Załóżmy, że Robert przypadkowo zarejestrowany jako `"bib@cpandl.com"` i nie go, nie będzie mógł korzystać z odzyskiwania hasła, ponieważ aplikacja nie ma poprawnego adresu e-mail. Potwierdzenie poczty e-mail zapewnia tylko ograniczoną ochronę z botów i nie zapewnia ochrony przed określonymi nadawców.

Zazwyczaj chcesz uniemożliwić nowym użytkownikom ogłaszanie danych w witrynie sieci Web przed ich potwierdzeniem za pośrednictwem poczty e-mail, wiadomości SMS lub innego mechanizmu. W poniższych sekcjach zostanie włączone potwierdzenie poczty e-mail i zmodyfikujesz kod, aby uniemożliwić nowo zarejestrowanym użytkownikom logowanie się do momentu potwierdzenia potwierdzenia wiadomości e-mail. Będziesz używać usługi poczty e-mail SendGrid w tym samouczku.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Podłącz do SendGrid

SendGrid zmienił interfejs API, ponieważ ten samouczek został zapisany. Bieżące instrukcje SendGrid można znaleźć w temacie [SendGrid](http://sendgrid.com/) lub [enable Account Confirmation and Password Recovery](xref:security/authentication/accconfirm#enable-account-confirmation-and-password-recovery).

Chociaż w tym samouczku pokazano, jak dodać powiadomienie e-mail za pośrednictwem usługi [SendGrid](http://sendgrid.com/), możesz wysłać wiadomość e-mail przy użyciu protokołu SMTP i innych mechanizmów (zobacz [dodatkowe zasoby](#addRes)).

1. W programie Visual Studio Otwórz **konsolę Menedżera pakietów** (**Narzędzia** -&gt; menedżer **pakietów NuGet** -&gt; **konsoli Menedżera pakietów**), a następnie wprowadź następujące polecenie:  
    `Install-Package SendGrid`
2. Przejdź do [strony rejestracji w usłudze Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) i zarejestruj się, aby skorzystać z bezpłatnego konta SendGrid. Możesz również utworzyć konto bezpłatnej usługi SendGrid bezpośrednio w [witrynie SendGrid](http://www.sendgrid.com).
3. W **Eksplorator rozwiązań** otwórz plik *IdentityConfig.cs* w *aplikacji\_Start* folder i Dodaj następujący kod wyróżniony żółtym do klasy `EmailService`, aby skonfigurować **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Ponadto Dodaj następujące instrukcje `using` na początku pliku *IdentityConfig.cs* : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Aby ten przykład był prosty, należy zapisać wartości konta usługi poczty e-mail w sekcji `appSettings` w pliku *Web. config* . Dodaj następujący kod XML wyróżniony w żółtym do pliku *Web. config* w katalogu głównym projektu:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. W tym przykładzie konto i poświadczenia są przechowywane w sekcji **element appSetting** w pliku *Web. config* . Na platformie Azure możesz bezpiecznie przechowywać te wartości na karcie **[Konfiguracja](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** w Azure Portal. Aby uzyskać powiązane informacje, zobacz temat Rick Anderson z uwzględnieniem [najlepszych rozwiązań dotyczących wdrażania haseł i innych poufnych danych w ASP.NET i na platformie Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Dodaj wartości usługi poczty e-mail w celu odzwierciedlenia wartości uwierzytelniania SendGrid (nazwa użytkownika i hasło), dzięki czemu możesz pomyślnie wysyłać wiadomości e-mail z aplikacji. Upewnij się, że używasz nazwy konta SendGrid zamiast podanego adresu e-mail SendGrid.

### <a name="enable-email-confirmation"></a>Włącz potwierdzenie poczty E-mail

 Aby włączyć potwierdzenie poczty e-mail, zmodyfikuj kod rejestracji, wykonując poniższe kroki.  

1. W folderze *konto* Otwórz *register.aspx.cs* kod i zaktualizuj metodę `CreateUser_Click`, aby włączyć następujące wyróżnione zmiany: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *default. aspx* i wybierz pozycję **Ustaw jako stronę startową**.
3. Uruchom aplikację, naciskając klawisz **F5.** Po wyświetleniu strony kliknij link **zarejestruj** , aby wyświetlić stronę Rejestracja.
4. Wprowadź adres e-mail i hasło, a następnie kliknij przycisk **zarejestruj** , aby wysłać wiadomość e-mail za pośrednictwem SendGrid.  
   Bieżący stan projektu i kodu umożliwi użytkownikowi zalogowanie się po zakończeniu formularza rejestracji, nawet jeśli nie potwierdzono konta.
5. Sprawdź konto e-mail i kliknij link, aby potwierdzić swój adres e-mail.  
   Po przesłaniu formularza rejestracji użytkownik zostanie zalogowany.  
    ![Przykładowa witryna sieci Web — zalogowano](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Wymagaj potwierdzenia wiadomości E-mail przed zalogowaniem

Mimo że zostało potwierdzone konto e-mail, w tym momencie nie trzeba klikać linku zawartego w wiadomości e-mail weryfikacyjnej w celu pełnego zalogowania się. W poniższej sekcji zmodyfikujesz kod wymagający, aby nowi użytkownicy otrzymali potwierdzenie wiadomości e-mail przed ich zalogowaniem się (uwierzytelniony).

1. W **Eksplorator rozwiązań** programu Visual Studio zaktualizuj zdarzenie `CreateUser_Click` w kodzie *register.aspx.cs* zawartym w folderze *accounts* z następującymi wyróżnionymi zmianami: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Zaktualizuj metodę `LogIn` w kodzie *login.aspx.cs* z następującymi wyróżnionymi zmianami: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Uruchom aplikację

 Po zaimplementowaniu kodu w celu sprawdzenia, czy adres e-mail użytkownika został potwierdzony, można sprawdzić funkcjonalność na stronie **Rejestr** i **Logowanie** . 

1. Usuń wszystkie konta z tabeli **AspNetUsers** zawierające alias adresu e-mail, który chcesz przetestować.
2. Uruchom aplikację (**F5**) i sprawdź, czy nie możesz zarejestrować się jako użytkownik, dopóki nie potwierdzisz adresu e-mail.
3. Przed potwierdzeniem nowego konta za pośrednictwem wysyłanej wiadomości e-mail, spróbuj zalogować się przy użyciu nowego konta.  
 Zobaczysz, że nie możesz się zalogować i że musisz mieć potwierdzone konto e-mail.
4. Po potwierdzeniu adresu e-mail Zaloguj się do aplikacji.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Odzyskiwanie i resetowanie hasła

1. W programie Visual Studio Usuń znaki komentarza z metody `Forgot` w kodzie *forgot.aspx.cs* zawartym w folderze *Account* , aby Metoda była wyświetlana w następujący sposób: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Otwórz stronę *login. aspx* . Zastąp znacznik blisko końca sekcji **kontrolka LoginForm** , jak pokazano poniżej: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Otwórz kod *login.aspx.cs* i Usuń komentarz z następującego wiersza kodu wyróżnionego żółtym kolorem programu obsługi zdarzeń `Page_Load`: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Uruchom aplikację, naciskając klawisz **F5.** Po wyświetleniu strony kliknij link **Zaloguj** .
5. Kliknij link nie **pamiętasz hasła?** , aby wyświetlić stronę **zapomniane hasło** .
6. Wprowadź swój adres e-mail, a następnie kliknij przycisk **Prześlij** , aby wysłać wiadomość e-mail na adres, co pozwoli na zresetowanie hasła.   
   Sprawdź konto e-mail i kliknij link, aby wyświetlić stronę **resetowania hasła** .
7. Na stronie **Resetowanie hasła** wprowadź adres e-mail, hasło i potwierdzenie hasła. Następnie naciśnij przycisk **Resetuj** .  
   Po pomyślnym zresetowaniu hasła zostanie wyświetlona strona **zmiany hasła** . Teraz możesz zalogować się przy użyciu nowego hasła.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Wyślij ponownie link do potwierdzenia wiadomości E-mail

Po utworzeniu nowego konta lokalnego przez użytkownika są wysyłane pocztą e-mail link potwierdzający, którego należy użyć przed zalogowaniem się. Jeśli użytkownik przypadkowo usunie wiadomość e-mail z potwierdzeniem lub wiadomość e-mail nie dotarła do Ciebie, będzie potrzebować linku potwierdzenia wysłanej ponownie. Poniższe zmiany kodu pokazują, jak to włączyć.

1. W programie Visual Studio Otwórz kod **login.aspx.cs** i Dodaj następujący program obsługi zdarzeń po obsłudze zdarzeń `LogIn`:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Zmodyfikuj procedurę obsługi zdarzeń `LogIn` w kodzie *login.aspx.cs* , zmieniając kod wyróżniony kolorem żółtym w następujący sposób: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Zaktualizuj stronę *login. aspx* , dodając kod wyróżniony kolorem żółtym w następujący sposób: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Usuń wszystkie konta z tabeli **AspNetUsers** zawierające alias adresu e-mail, który chcesz przetestować.
5. Uruchom aplikację (**F5**) i Zarejestruj swój adres e-mail.
6. Przed potwierdzeniem nowego konta za pośrednictwem wysyłanej wiadomości e-mail, spróbuj zalogować się przy użyciu nowego konta.  
   Zobaczysz, że nie możesz się zalogować i że musisz mieć potwierdzone konto e-mail. Ponadto można teraz ponownie wysłać komunikat z potwierdzeniem do konta e-mail.
7. Wprowadź adres e-mail i hasło, a następnie naciśnij przycisk **potwierdzenia ponownego wysłania** .
8. Po potwierdzeniu adresu e-mail na podstawie nowo wysłanej wiadomości e-mail Zaloguj się do aplikacji.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Rozwiązywanie problemów z aplikacją

Jeśli nie otrzymasz wiadomości e-mail zawierającej link do zweryfikowania poświadczeń:

- Sprawdź folder wiadomości-śmieci lub spamu.
- Zaloguj się do konta SendGrid, a następnie kliknij [link działania e-mail](https://sendgrid.com/logs/index).
- Musisz mieć pewność, że jako wartość *Web. config* użyto nazwy konta użytkownika SendGrid, a nie adresu E-mail konta SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Linki do ASP.NET Identity zalecanych zasobów](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Seria samouczka formularzy sieci Web ASP.NET — Dodawanie dostawcy OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Wdróż bezpieczną aplikację ASP.NET Web Forms z członkostwem, uwierzytelnianiem OAuth i SQL Database, aby Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Seria samouczka formularzy sieci Web ASP.NET — włączenie protokołu SSL dla projektu](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
