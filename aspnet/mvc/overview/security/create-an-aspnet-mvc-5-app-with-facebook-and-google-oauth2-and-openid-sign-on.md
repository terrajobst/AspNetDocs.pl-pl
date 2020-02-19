---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Tworzenie aplikacji MVC 5 za pomocą usług Facebook, Twitter, LinkedIn i Google OAuth2 Sign-C#in () | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 5, która umożliwia użytkownikom logowanie się przy użyciu protokołu OAuth 2,0 z poświadczeniami z zewnętrznego wstępnego...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457690"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Tworzenie aplikacji ASP.NET MVC 5 z logowaniem OAuth2 za pomocą poświadczeń usług Facebook, Twitter, LinkedIn i Google (C#)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 5, która umożliwia użytkownikom logowanie się przy użyciu protokołu [OAuth 2,0](http://oauth.net/2/) z poświadczeniami od zewnętrznego dostawcy uwierzytelniania, takiego jak Facebook, Twitter, LinkedIn, Microsoft lub Google. Dla uproszczenia ten samouczek koncentruje się na pracy z poświadczeniami z serwisu Facebook i Google.
> 
> Włączenie tych poświadczeń w witrynach sieci Web zapewnia znaczną korzyść, ponieważ miliony użytkowników mają już konta z dostawcami zewnętrznymi. Mogą być bardziej nachylone użytkownicy, którzy nie muszą tworzyć i zapamiętać nowego zestawu poświadczeń.
> 
> Zobacz też [ASP.NET aplikację MVC 5 za pomocą wiadomości SMS i uwierzytelniania dwuskładnikowego poczty e-mail](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> W tym samouczku pokazano również, jak dodać dane profilu dla użytkownika oraz jak dodawać role przy użyciu interfejsu API członkostwa. Ten samouczek został utworzony przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (Obserwuj mnie w serwisie Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

<a id="start"></a>
## <a name="getting-started"></a>Wprowadzenie

Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj program Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszy. Aby uzyskać pomoc dotyczącą usług Dropbox, GitHub, LinkedIn, usługi Instagram, buffer, Salesforce, PAROW, Stack Exchange, TripIt, Twitch, Twitter, Yahoo! i innych, zobacz ten [przykładowy projekt](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Musisz zainstalować program Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszy, aby używać usługi Google OAuth 2 i do debugowania lokalnego bez ostrzeżeń dotyczących protokołu SSL.

Na stronie **startowej** kliknij pozycję **Nowy projekt** lub możesz użyć menu i wybrać polecenie **plik**, a następnie pozycję **Nowy projekt**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Kliknij pozycję **Nowy projekt**, a następnie wybierz pozycję **Wizualizacja C#**  po lewej stronie, a następnie pozycję **Sieć Web** , a następnie wybierz opcję **aplikacja sieci Web ASP.NET**. Nadaj projektowi nazwę "MvcAuth", a następnie kliknij przycisk **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

W oknie dialogowym **Nowy projekt ASP.NET** kliknij pozycję **MVC**. Jeśli uwierzytelnianie nie jest **poszczególnymi kontami użytkowników**, kliknij przycisk **Zmień uwierzytelnianie** i wybierz **poszczególne konta użytkowników**. Sprawdzając **host w chmurze**, aplikacja będzie bardzo łatwa do hostowania na platformie Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Jeśli **w chmurze**wybrano opcję host, wypełnij okno dialogowe Konfigurowanie.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Aktualizowanie do najnowszej wersji oprogramowania pośredniczącego przy użyciu narzędzia NuGet

Użyj Menedżera pakietów NuGet, aby zaktualizować [oprogramowanie pośredniczące Owin](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). W menu po lewej stronie wybierz pozycję **aktualizacje** . Możesz kliknąć przycisk **Aktualizuj wszystko** lub wyszukać tylko pakiety Owin (pokazane na następnym obrazie):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Na poniższej ilustracji są wyświetlane tylko pakiety OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

W konsoli Menedżera pakietów (PMC) można wprowadzić `Update-Package` polecenie, które zaktualizuje wszystkie pakiety.

Naciśnij klawisz **F5** lub **Ctrl + F5** , aby uruchomić aplikację. Na poniższej ilustracji numer portu to 1234. Po uruchomieniu aplikacji zobaczysz inny numer portu.

W zależności od rozmiaru okna przeglądarki może być konieczne kliknięcie ikony nawigacji, aby wyświetlić **informacje dotyczące** **domu**, **kontaktu**, **rejestrowania** i **logowania** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Konfigurowanie protokołu SSL w projekcie

Aby połączyć się z dostawcami uwierzytelniania, takimi jak Google i Facebook, należy skonfigurować usługi IIS-Express do korzystania z protokołu SSL. Ważne jest, aby nadal korzystać z protokołu SSL po zalogowaniu i nie wrócić do protokołu HTTP, plik cookie logowania jest równie tajny jak nazwa użytkownika i hasło oraz bez użycia protokołu SSL, który jest przesyłany w postaci zwykłego tekstu przez sieć. Oprócz tego czas potrzebny do wykonania uzgadniania i zabezpieczenia kanału (czyli zbiorczego działania protokołu HTTPS wolniejszego niż HTTP) przed uruchomieniem potoku MVC, dlatego przekierowanie z powrotem do protokołu HTTP po zalogowaniu nie spowoduje wykonania bieżącego żądania lub przyszłego żądania znacznie szybsze.

1. W **Eksplorator rozwiązań**kliknij projekt **MvcAuth** .
2. Naciśnij klawisz F4, aby wyświetlić właściwości projektu. Alternatywnie, z menu **Widok** można wybrać **okno właściwości**.
3. Zmień wartość **SSL z włączony** na true.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Skopiuj adres URL protokołu SSL (który będzie `https://localhost:44300/`, chyba że utworzono inne projekty SSL).
5. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **MvcAuth** i wybierz polecenie **Właściwości**.
6. Wybierz kartę **Sieć Web** , a następnie wklej adres URL protokołu SSL do pola **adres URL projektu** . Zapisz plik (CTL + S). Ten adres URL będzie potrzebny do konfigurowania aplikacji Facebook i Google Authentication.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Dodaj atrybut [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) do kontrolera `Home`, aby wymagać wszystkie żądania muszą używać protokołu HTTPS. Bardziej bezpiecznym podejściem jest dodanie filtra [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) do aplikacji. Zapoznaj się z sekcją &quot;chronić aplikację przy użyciu protokołu SSL, a&quot; atrybutu Autoryzuj w moim samouczku [Utwórz aplikację ASP.NET MVC z uwierzytelnianiem i bazą danych SQL, a następnie wdróż ją w Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Poniżej przedstawiono część kontrolera macierzystego.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Naciśnij klawisze CTRL+F5, aby uruchomić aplikację. Jeśli certyfikat został zainstalowany w przeszłości, możesz pominąć resztę tej sekcji i przejść do [tworzenia aplikacji Google dla protokołu OAuth 2 i połączyć aplikację z projektem](#goog). w przeciwnym razie postępuj zgodnie z instrukcjami, aby zaufać certyfikatowi z podpisem własnym, który IIS Express wygenerowany.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Przeczytaj okno dialogowe **Ostrzeżenie o zabezpieczeniach** , a następnie kliknij przycisk **tak** , jeśli chcesz zainstalować certyfikat reprezentujący localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Program IE pokazuje stronę *główną* i nie ma ostrzeżeń dotyczących protokołu SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Program Google Chrome akceptuje również certyfikat i wyświetla zawartość HTTPS bez ostrzeżenia. Program Firefox używa własnego magazynu certyfikatów, więc wyświetli ostrzeżenie. W przypadku naszej aplikacji możesz bezpiecznie kliknąć opcję **zapoznaj się z ryzykiem**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji z projektem

> [!WARNING]
> Bieżące instrukcje dotyczące usługi Google OAuth można znaleźć [w temacie Configuring Google Authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Przejdź do [konsoli deweloperów firmy Google](https://console.developers.google.com/).
2. Jeśli projekt nie został wcześniej utworzony, wybierz pozycję **poświadczenia** na karcie po lewej stronie, a następnie wybierz pozycję **Utwórz**.
3. Na karcie po lewej stronie kliknij pozycję **poświadczenia**.
4. Kliknij przycisk **Utwórz poświadczenia** , a następnie **Identyfikator klienta uwierzytelniania OAuth**. 

    1. W oknie dialogowym **Tworzenie identyfikatora klienta** Zachowaj domyślną **aplikację sieci Web** dla typu aplikacji.
    2. Ustaw **autoryzowane źródła kodu JavaScript** na adres URL protokołu SSL użyty powyżej (`https://localhost:44300/`, chyba że utworzono inne projekty SSL)
    3. Ustaw **Autoryzowany identyfikator URI przekierowania** na:  
         `https://localhost:44300/signin-google`
5. Kliknij pozycję menu ekran wyrażania zgody OAuth, a następnie ustaw adres e-mail i nazwę produktu. Po zakończeniu formularza kliknij pozycję **Zapisz**.
6. Kliknij element menu Biblioteka, Wyszukaj pozycję **Google + interfejs API**, kliknij go, a następnie naciśnij pozycję Włącz.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   Na poniższej ilustracji przedstawiono włączone interfejsy API.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. W Menedżerze API usługi Google API przejdź do karty **poświadczenia** , aby uzyskać **Identyfikator klienta**. Pobierz, aby zapisać plik JSON z wpisami tajnymi aplikacji. Skopiuj i wklej **ClientId** oraz **ClientSecret** do metody `UseGoogleAuthentication` znajdującej się w pliku *Startup.auth.cs* w folderze *App_Start* . Podane poniżej wartości **ClientId** i **ClientSecret** są przykładami i nie działają.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. Konto i poświadczenia są dodawane do powyższego kodu, aby zapewnić prostą przykład. Zapoznaj [się z najlepszymi rozwiązaniami dotyczącymi wdrażania haseł i innych poufnych danych w ASP.NET i Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Naciśnij **kombinację klawiszy CTRL + F5** , aby skompilować i uruchomić aplikację. Kliknij link **Zaloguj** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. W obszarze **Użyj innej usługi do zalogowania**się kliknij pozycję **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Jeśli pominięto którykolwiek z powyższych kroków, wystąpi błąd HTTP 401. Sprawdź ponownie powyższe kroki. Jeśli pominięto wymagane ustawienie (na przykład **Nazwa produktu**), Dodaj brakujący element i Zapisz; uwierzytelnienie może potrwać kilka minut.
10. Nastąpi przekierowanie do witryny Google, w której będą wprowadzane poświadczenia.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Po wprowadzeniu poświadczeń zostanie wyświetlony monit o przyznanie uprawnień do aplikacji sieci Web, która została właśnie utworzona:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Kliknij przycisk **Akceptuj**. Nastąpi przekierowanie z powrotem do strony **rejestracji** w aplikacji MvcAuth, w której można zarejestrować swoje konto Google. Istnieje możliwość zmiany nazwy rejestracji lokalnej poczty e-mail używanej na potrzeby konta usługi Gmail, ale zazwyczaj chcesz zachować domyślny alias poczty e-mail (czyli ten, który został użyty do uwierzytelniania). Kliknij pozycję **zarejestruj**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji z projektem

> [!WARNING]
> Bieżące instrukcje dotyczące uwierzytelniania w usłudze Facebook OAuth2 można znaleźć w temacie [Konfigurowanie uwierzytelniania w serwisie Facebook](/aspnet/core/security/authentication/social/facebook-logins)

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Sprawdzanie danych członkostwa

W menu **Widok** kliknij **Eksplorator serwera**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Rozwiń węzeł **DefaultConnection (MvcAuth)** , rozwiń węzeł **tabele**, kliknij prawym przyciskiem myszy pozycję **AspNetUsers** i kliknij polecenie **Pokaż dane tabeli**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dane tabeli AspnetUsers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Dodawanie danych profilu do klasy użytkownika

W tej sekcji dodasz datę urodzenia i miejscowość domu do danych użytkownika podczas rejestracji, jak pokazano na poniższej ilustracji.

![reg z domem i bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Otwórz plik *Models\IdentityModels.cs* i Dodaj właściwości data urodzenia i miejscowość domu:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Otwórz plik *Models\AccountViewModels.cs* oraz właściwości ustaw datę urodzenia i miejscowość w `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Otwórz plik *Controllers\AccountController.cs* i Dodaj kod dla daty urodzenia i miejscowości głównej w `ExternalLoginConfirmation`ej metodzie działania, jak pokazano poniżej:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Dodaj datę urodzenia i miejscowość domu do pliku *Views\Account\ExternalLoginConfirmation.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Usuń bazę danych członkostwa, aby móc ponownie zarejestrować swoje konto w serwisie Facebook przy użyciu aplikacji, a następnie sprawdź, czy możesz dodać nową datę urodzenia i informacje o profilu miejscowości.

W **Eksplorator rozwiązań**kliknij ikonę **Pokaż wszystkie pliki** , a następnie kliknij prawym przyciskiem myszy pozycję *dodaj\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;. mdf* i kliknij polecenie **Usuń**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów** (PMC). Wprowadź następujące polecenia w obszarze PMC.

1. Włącz-migracje
2. Inicjowanie dodawania do migracji
3. Update-Database

Uruchom aplikację, a następnie zaloguj się i zarejestruj niektórych użytkowników za pomocą serwisu FaceBook i Google.

## <a name="examine-the-membership-data"></a>Sprawdzanie danych członkostwa

W menu **Widok** kliknij **Eksplorator serwera**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Kliknij prawym przyciskiem myszy pozycję **AspNetUsers** , a następnie kliknij pozycję **Pokaż dane tabeli**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Poniżej przedstawiono pola `HomeTown` i `BirthDate`.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Wylogowywanie aplikacji i logowanie przy użyciu innego konta

Jeśli zalogujesz się do aplikacji za pomocą usługi Facebook, a następnie wylogujesz się, a następnie spróbujesz ponownie zalogować się przy użyciu innego konta w serwisie Facebook (za pomocą tej samej przeglądarki), nastąpi natychmiastowe zalogowanie do poprzedniego konta w serwisie Facebook, z którego korzystasz. Aby użyć innego konta, należy przejść do serwisu Facebook i wylogować się w serwisie Facebook. Ta sama reguła ma zastosowanie do każdego innego dostawcy uwierzytelniania innej firmy. Alternatywnie możesz zalogować się przy użyciu innego konta przy użyciu innej przeglądarki.

## <a name="next-steps"></a>Następne kroki

Zapoznaj [się z artykułem wprowadzenie do dostawców zabezpieczeń protokołu OAuth usługi Yahoo i LinkedIn dla Owin](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) przez Jerrie Pelser dla usług Yahoo i LinkedIn. Zobacz temat Jerrie dbsocial login Button for ASP.NET MVC 5, aby uzyskać przyciski logowania społecznościowego.

Postępuj zgodnie z moim samouczkiem [Utwórz aplikację ASP.NET MVC z uwierzytelnianiem i bazą danych SQL, a następnie wdróż ją w Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), która kontynuuje ten samouczek i pokaże następujące elementy:

1. Jak wdrożyć aplikację na platformie Azure.
2. Jak zabezpieczyć aplikację za pomocą ról.
3. Jak zabezpieczyć aplikację przy użyciu metody [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) i [Autoryzuj](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtry.
4. Jak dodać użytkowników i role przy użyciu interfejsu API członkostwa.

Prosimy o opinię na temat sposobu, w jaki polubisz ten samouczek, i co możemy ulepszyć. Możesz również zażądać nowych tematów w temacie [jak z kodem](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Możesz nawet zadawać i głosować na nowe funkcje, które zostaną dodane do ASP.NET. Można na przykład zagłosować na narzędzie do [tworzenia użytkowników i ról oraz zarządzania nimi.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Aby uzyskać dobre wyjaśnienie, jak działają usługi uwierzytelniania zewnętrznego ASP.NET, zobacz [zewnętrzne usługi uwierzytelniania](https://asp.net/web-api/overview/security/external-authentication-services)firmy Roberta McMurraya. Artykuł Roberta również szczegółowo opisuje włączenie uwierzytelniania firmy Microsoft i usługi Twitter. [Samouczek doskonały EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) firmy Dykstra pokazuje, jak korzystać z Entity Framework.
