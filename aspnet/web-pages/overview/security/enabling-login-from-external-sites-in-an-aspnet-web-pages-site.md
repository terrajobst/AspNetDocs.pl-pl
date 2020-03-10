---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Logowanie przy użyciu witryn zewnętrznych w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule wyjaśniono, jak zalogować się do witryny ASP.NET Web Pages (Razor) za pomocą usługi Facebook, Google, Twitter, Yahoo i innych witryn — czyli sposobu obsługi...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638755"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Logowanie przy użyciu witryn zewnętrznych w witrynie ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak zalogować się do witryny ASP.NET Web Pages (Razor) za pomocą usługi Facebook, Google, Twitter, Yahoo i innych witryn — czyli sposobu obsługi uwierzytelniania OAuth i OpenID Connect w lokacji.
> 
> Zawartość:
> 
> - Jak włączyć logowanie z innych lokacji przy użyciu szablonu witryny WebMatrix Starter.
> 
> Jest to funkcja ASP.NET wprowadzona w artykule:
> 
> - Pomocnik `OAuthWebSecurity`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - WebMatrix 3

Strony sieci Web ASP.NET obejmują obsługę dostawców [OAuth](http://oauth.net/) i [OpenID Connect](http://openid.net/) . Korzystając z tych dostawców, można zezwolić użytkownikom na logowanie się do swojej witryny przy użyciu istniejących poświadczeń z serwisu Facebook, Twitter, Microsoft i Google. Na przykład, aby zalogować się przy użyciu konta w serwisie Facebook, użytkownicy mogą po prostu wybrać ikonę w serwisie Facebook, która przekierowuje je do strony logowania w serwisie Facebook, gdzie wprowadzają informacje o użytkowniku. Następnie mogą skojarzyć logowanie w serwisie Facebook ze swoimi kontami w swojej witrynie. Powiązane ulepszenie funkcji członkostwa stron sieci Web polega na tym, że użytkownicy mogą kojarzyć wiele nazw logowania (w tym logowania z witryn sieci społecznościowych) przy użyciu jednego konta w witrynie sieci Web.

Ten obraz przedstawia stronę logowania z szablonu **witryny startowej** , w którym użytkownik może wybrać ikonę Facebook, Twitter, Google lub Microsoft, aby włączyć logowanie przy użyciu konta zewnętrznego:

![dostawcy zewnętrzni](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Można włączyć członkowstwo OAuth i OpenID Connect przez usunięcie komentarza do kilku wierszy kodu w szablonie **witryny Starter** . Metody i właściwości używane do pracy z dostawcami OAuth i OpenID Connect znajdują się w klasie `WebMatrix.Security.OAuthWebSecurity`. Szablon **witryny Starter** obejmuje pełną infrastrukturę członkostwa, kompletną stronę logowania, bazę danych członkostwa i cały kod, który jest potrzebny użytkownikom do logowania się do witryny przy użyciu poświadczeń lokalnych lub z innej lokacji.

Ta sekcja zawiera przykład sposobu pozwalania użytkownikom na logowanie się z witryn zewnętrznych do witryny opartej na szablonie **witryny Starter** . Po utworzeniu witryny Starter należy wykonać tę czynność (szczegóły znajdują się poniżej):

- W przypadku witryn korzystających z dostawcy OAuth (Facebook, Twitter i Microsoft) utworzysz aplikację w witrynie zewnętrznej. Dzięki temu klucze aplikacji będą potrzebne do wywołania funkcji logowania dla tych lokacji.
- W przypadku witryn korzystających z dostawcy OpenID Connect (Google) nie ma potrzeby tworzenia aplikacji. W przypadku wszystkich tych witryn musisz mieć konto, aby zalogować się i utworzyć aplikacje dla deweloperów.

    > [!NOTE]
    > Aplikacje firmy Microsoft akceptują adres URL na żywo dla działającej witryny sieci Web, więc nie można używać adresu URL lokalnego witryny sieci Web do testowania logowania.
- Edytuj kilka plików w witrynie sieci Web, aby określić odpowiedniego dostawcę uwierzytelniania i przesłać dane logowania do witryny, której chcesz użyć.

Ten artykuł zawiera oddzielne instrukcje dotyczące następujących zadań:

- [Włączanie logowania Google](#To_enable_Google_logins)
- [Włączanie logowania do usługi Facebook](#To_enable_Facebook_logins)
- [Włączanie logowania do usługi Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Włączanie logowania Google

1. Utwórz lub Otwórz witrynę ASP.NET Web Pages, która jest oparta na szablonie witryny WebMatrix Starter.
2. Otwórz stronę *\_AppStart. cshtml* i Usuń komentarz z poniższego wiersza kodu. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testowanie logowania Google

1. Uruchom *domyślną stronę. cshtml* witryny i wybierz przycisk **Zaloguj się** .
2. Na stronie *Logowanie* w sekcji Korzystanie z **innej usługi do logowania** wybierz pozycję **Google** lub **Yahoo** Submit (Prześlij). W tym przykładzie używamy logowania Google. 

    Strona sieci Web przekierowuje żądanie do strony logowania Google.

    ![Logowanie Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Wprowadź poświadczenia dla istniejącego konta Google.
4. Jeśli zostanie wyświetlony monit z pytaniem, czy chcesz zezwolić na używanie przez *hosta lokalnego* informacji z konta, kliknij przycisk **Zezwalaj**.

    Kod używa tokenu Google do uwierzytelniania użytkownika, a następnie wraca do tej strony w witrynie sieci Web. Ta strona umożliwia użytkownikom kojarzenie ich logowania Google z istniejącym kontem w witrynie sieci Web lub rejestrowanie nowego konta w lokacji w celu skojarzenia logowania zewnętrznego z.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Wybierz przycisk **Skojarz** . Przeglądarka powraca do strony głównej aplikacji.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Włączanie logowania do usługi Facebook

1. Przejdź do [witryny deweloperów serwisu Facebook](https://developers.facebook.com/apps) (Zaloguj się, jeśli jeszcze nie jest zalogowany).
2. Wybierz przycisk **Utwórz nową aplikację** , a następnie postępuj zgodnie z monitami, aby nazwa i utworzyć nową aplikację.
3. W sekcji Wybierz, w **jaki sposób aplikacja będzie integrowana z usługą Facebook**, wybierz sekcję **Witryna sieci Web** .
4. Wprowadź wartość w polu **adres URL witryny** , podając adres URL witryny (na przykład `http://www.example.com`). Pole **domena** jest opcjonalne; służy do zapewnienia uwierzytelniania dla całej domeny (na przykład *example.com*). 

    > [!NOTE]
    > Jeśli na komputerze lokalnym jest uruchomiona witryna z adresem URL, takim jak `http://localhost:12345` (gdzie numer jest numerem portu lokalnego), można dodać tę wartość do pola **adres URL witryny** w celu przetestowania lokacji. Jednak w przypadku zmiany numeru portu lokacji lokalnej należy zaktualizować pole **adres URL witryny** aplikacji.
5. Wybierz przycisk **Zapisz zmiany** .
6. Ponownie wybierz kartę **aplikacje** , a następnie Wyświetl stronę początkową swojej aplikacji.
7. Skopiuj wartości **Identyfikator aplikacji** i **klucz tajny** aplikacji dla aplikacji i wklej je do tymczasowego pliku tekstowego. Te wartości zostaną przekazane do dostawcy usługi Facebook w kodzie witryny sieci Web.
8. Zamknij witrynę dewelopera serwisu Facebook.

Teraz wprowadzono zmiany w dwóch stronach w witrynie sieci Web, dzięki czemu użytkownicy będą mogli logować się do witryny przy użyciu swoich kont w serwisie Facebook.

1. Utwórz lub Otwórz witrynę ASP.NET Web Pages, która jest oparta na szablonie witryny WebMatrix Starter.
2. Otwórz stronę *\_AppStart. cshtml* i usuń znaczniki komentarza z kodu dla dostawcy protokołu OAuth w serwisie Facebook. Niekomentowany blok kodu wygląda następująco: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Skopiuj wartość **identyfikatora aplikacji** z aplikacji Facebook jako wartość parametru `appId` (wewnątrz cudzysłowów).
4. Skopiuj wartość **klucza tajnego aplikacji** z aplikacji Facebook jako wartość parametru `appSecret`.
5. Zapisz i zamknij plik.

### <a name="testing-facebook-login"></a>Testowanie logowania w usłudze Facebook

1. Uruchom *domyślną stronę. cshtml* witryny, a następnie wybierz przycisk **Zaloguj** .
2. Na stronie *Logowanie* w sekcji Korzystanie z **innej usługi do logowania** wybierz ikonę **Facebook** . 

    Strona sieci Web przekierowuje żądanie do strony logowania w serwisie Facebook.

    ![Uwierzytelnianie OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Zaloguj się do konta w serwisie Facebook. 

    Kod używa tokenu Facebook do uwierzytelniania użytkownika, a następnie wraca do strony, na której można skojarzyć dane logowania w serwisie Facebook z logowaniem do witryny. Nazwa użytkownika lub adres e-mail są wypełniane w polu **adres e-mail** w formularzu.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Wybierz przycisk **Skojarz** . 

    Przeglądarka powraca do strony głównej, a użytkownik jest zalogowany.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Włączanie logowania do usługi Twitter

1. Przejdź do [witryny Twitter Developers](https://dev.twitter.com/).
2. Wybierz łącze **Utwórz aplikację** , a następnie zaloguj się do lokacji.
3. W formularzu **Utwórz aplikację** Wypełnij pola **Nazwa** i **Opis** .
4. W polu **Witryna sieci Web** wprowadź adres URL witryny (na przykład `http://www.example.com`). 

    > [!NOTE]
    > Jeśli testujesz swoją lokację lokalnie (przy użyciu adresu URL, takiego jak `http://localhost:12345`), serwis Twitter może nie zaakceptować adresu URL. Może jednak być możliwe użycie lokalnego adresu IP sprzężenia zwrotnego (na przykład `http://127.0.0.1:12345`). Upraszcza to proces testowania aplikacji lokalnie. Jednak za każdym razem, gdy zmieniany jest numer portu lokacji lokalnej, należy zaktualizować pole **Witryna sieci Web** aplikacji.
5. W polu **adres URL wywołania zwrotnego** wprowadź adres URL strony w witrynie sieci Web, do której użytkownicy będą mogli powrócić po zalogowaniu się do usługi Twitter. Na przykład w celu wysłania użytkowników do strony głównej witryny startowej (która będzie rozpoznawać swój stan logowania) wprowadź ten sam adres URL wprowadzony w polu **Witryna sieci Web** .
6. Zaakceptuj warunki i wybierz przycisk **Utwórz aplikację w usłudze Twitter** .
7. Na stronie spocznik **Moje aplikacje** Wybierz utworzoną aplikację.
8. Na karcie **szczegóły** przewiń w dół i wybierz przycisk **Utwórz token dostępu** .
9. Na karcie **szczegóły** Skopiuj wartości **klucza klienta** i **wpisu tajnego konsumenta** dla aplikacji i wklej je w tymczasowym pliku tekstowym. Te wartości zostaną przekazane do dostawcy usługi Twitter w kodzie witryny sieci Web.
10. Zamknij witrynę usługi Twitter.

Teraz wprowadzono zmiany w dwóch stronach w witrynie sieci Web, dzięki czemu użytkownicy będą mogli logować się do witryny przy użyciu kont w usłudze Twitter.

1. Utwórz lub Otwórz witrynę ASP.NET Web Pages, która jest oparta na szablonie witryny WebMatrix Starter.
2. Otwórz stronę *\_AppStart. cshtml* i usuń znaczniki komentarza z kodu dla dostawcy protokołu OAuth w serwisie Twitter. Niekomentowany blok kodu wygląda następująco: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Skopiuj wartość **klucza klienta** z aplikacji Twitter jako wartość parametru `consumerKey` (wewnątrz cudzysłowów).
4. Skopiuj wartość **klucza tajnego klienta** z aplikacji Twitter jako wartość parametru `consumerSecret`.
5. Zapisz i zamknij plik.

### <a name="testing-twitter-login"></a>Testowanie logowania do usługi Twitter

1. Uruchom stronę *default. cshtml* w witrynie i wybierz przycisk **Zaloguj** .
2. Na stronie *Logowanie* w sekcji Korzystanie z **innej usługi do logowania** wybierz ikonę **Twitter** . 

    Strona sieci Web przekierowuje żądanie do strony logowania w usłudze Twitter dla utworzonej aplikacji.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Zaloguj się do konta usługi Twitter.
4. Kod używa tokenu usługi Twitter do uwierzytelniania użytkownika, a następnie wraca do strony, na której można skojarzyć dane logowania z kontem witryny sieci Web. Nazwa lub adres e-mail są wypełniane w polu **adres e-mail** w formularzu.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Wybierz przycisk **Skojarz** . 

    Przeglądarka powraca do strony głównej, a użytkownik jest zalogowany.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Dostosowywanie zachowania dla całej witryny](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Dodawanie zabezpieczeń i członkostwa do witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202904)
