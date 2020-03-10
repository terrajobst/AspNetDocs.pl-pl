---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Zabezpieczanie interfejsu API sieci Web przy użyciu pojedynczych kont i logowanie lokalne w ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: W tym temacie przedstawiono sposób zabezpieczania internetowego interfejsu API przy użyciu usługi OAuth2 do uwierzytelniania względem bazy danych członkostwa. Wersje oprogramowania używane w samouczku programu Visual Studio 201...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7492c4aa4c2a0a8aeed64c3462bda8fc51f35a6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555196"
---
# <a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Zabezpieczanie interfejsu API sieci Web przy użyciu pojedynczych kont i logowanie lokalne w ASP.NET Web API 2,2

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz przykładową aplikację](https://github.com/MikeWasson/LocalAccountsApp)

> W tym temacie przedstawiono sposób zabezpieczania internetowego interfejsu API przy użyciu usługi OAuth2 do uwierzytelniania względem bazy danych członkostwa.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Internetowy interfejs API 2,2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2,1](../../../identity/index.md)

W Visual Studio 2013 szablon projektu internetowego interfejsu API oferuje trzy opcje uwierzytelniania:

- **Poszczególne konta.** Aplikacja używa bazy danych członkostwa.
- **Konta organizacyjne.** Użytkownicy logują się przy użyciu Azure Active Directory, pakietu Office 365 lub poświadczeń Active Directory lokalnych.
- **Uwierzytelnianie systemu Windows.** Ta opcja jest przeznaczona dla aplikacji intranetowych i używa modułu usług IIS do uwierzytelniania systemu Windows.

Aby uzyskać więcej informacji na temat tych opcji, zobacz [Tworzenie projektów sieci Web ASP.NET w Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Pojedyncze konta zapewniają dwa sposoby logowania użytkownika:

- **Logowanie lokalne**. Użytkownik rejestruje się w lokacji, wprowadzając nazwę użytkownika i hasło. Aplikacja przechowuje skrót hasła w bazie danych członkostwa. Po zalogowaniu się użytkownika system ASP.NET Identity weryfikuje hasło.
- **Logowanie w sieci społecznościowej**. Użytkownik loguje się przy użyciu usługi zewnętrznej, takiej jak Facebook, Microsoft lub Google. Aplikacja nadal tworzy wpis dla użytkownika w bazie danych członkostwa, ale nie przechowuje żadnych poświadczeń. Użytkownik jest uwierzytelniany przez zalogowanie się do usługi zewnętrznej.

W tym artykule przedstawiono scenariusz logowania lokalnego. W przypadku logowania lokalnego i społecznego interfejs API sieci Web używa OAuth2 do uwierzytelniania żądań. Jednak przepływy poświadczeń są różne dla logowania lokalnego i społecznościowego.

W tym artykule przedstawiono prostą aplikację, która umożliwia użytkownikowi logowanie się i wysyłanie uwierzytelnionych wywołań AJAX do internetowego interfejsu API. Przykładowy kod można pobrać [tutaj](https://github.com/MikeWasson/LocalAccountsApp). W pliku Readme opisano sposób tworzenia przykładu od podstaw w programie Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Przykładowa aplikacja używa odcinania. js do przesyłania danych i jQuery do wysyłania żądań AJAX. Będę koncentrować się na wywołaniach AJAX, więc nie musisz znać separowania. js w tym artykule.

W ten sposób opisano następujące informacje:

- Działanie aplikacji po stronie klienta.
- Co dzieje się na serwerze.
- Ruch HTTP w środku.

Najpierw należy zdefiniować pewną terminologię OAuth2.

- *Zasób*. Niektóre dane, które mogą być chronione.
- *Serwer zasobów*. Serwer, który hostuje zasób.
- *Właściciel zasobu*. Jednostka, która może udzielić uprawnienia dostępu do zasobu. (Zazwyczaj jest to użytkownik).
- *Klient*: aplikacja, która chce uzyskać dostęp do zasobu. W tym artykule klient jest przeglądarką sieci Web.
- *Token dostępu*. Token, który udziela dostępu do zasobu.
- *Token elementu nośnego*. Określony typ tokenu dostępu, z właściwością, która może korzystać z tokenu. Innymi słowy, klient nie potrzebuje klucza kryptograficznego ani innego wpisu tajnego, aby użyć tokenu okaziciela. Z tego powodu tokeny okaziciela powinny być używane tylko w przypadku protokołu HTTPS i powinny mieć stosunkowo krótkie czasy wygaśnięcia.
- *Serwer autoryzacji*. Serwer, który dostarcza tokeny dostępu.

Aplikacja może działać jako serwer autoryzacji i serwer zasobów. Szablon projektu internetowego interfejsu API jest zgodny z tym wzorcem.

## <a name="local-login-credential-flow"></a>Przepływ poświadczeń logowania lokalnego

W przypadku logowania lokalnego interfejs API sieci Web używa [przepływu hasła właściciela zasobu](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) zdefiniowanego w OAuth2.

1. Użytkownik wprowadza nazwę i hasło do klienta.
2. Klient wysyła te poświadczenia do serwera autoryzacji.
3. Serwer autoryzacji uwierzytelnia poświadczenia i zwraca token dostępu.
4. Aby uzyskać dostęp do chronionego zasobu, klient zawiera token dostępu w nagłówku autoryzacji żądania HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

W przypadku wybrania **poszczególnych kont** w szablonie projektu interfejsu API sieci Web projekt zawiera serwer autoryzacji, który sprawdza poprawność poświadczeń użytkownika i tokenów problemów. Na poniższym diagramie przedstawiono ten sam przepływ poświadczeń pod względem składników internetowego interfejsu API.

![](individual-accounts-in-web-api/_static/image4.png)

W tym scenariuszu Kontrolery interfejsu API sieci Web działają jako serwery zasobów. Filtr uwierzytelniania weryfikuje tokeny dostępu, a atrybut **[autoryzuje]** jest używany do ochrony zasobu. Gdy kontroler lub akcja ma atrybut **[autoryzuje]** , wszystkie żądania do tego kontrolera lub akcji muszą zostać uwierzytelnione. W przeciwnym razie autoryzacja zostanie odrzucona, a interfejs API sieci Web zwróci błąd 401 (nieautoryzowany).

Serwer autoryzacji i filtr uwierzytelniania są wywoływane w składniku [oprogramowania pośredniczącego Owin](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) , który obsługuje szczegóły OAuth2. Opiszę projekt bardziej szczegółowo w dalszej części tego samouczka.

## <a name="sending-an-unauthorized-request"></a>Wysyłanie nieautoryzowanego żądania

Aby rozpocząć, uruchom aplikację i kliknij przycisk **Wywołaj interfejs API** . Po zakończeniu żądania w polu **wynik** powinien zostać wyświetlony komunikat o błędzie. Wynika to z faktu, że żądanie nie zawiera tokenu dostępu, więc żądanie nie jest autoryzowane.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Przycisk **interfejsu API wywołania** wysyła żądanie AJAX do ~/API/Values, który wywołuje akcję kontrolera interfejsu API sieci Web. Oto sekcja kodu JavaScript, która wysyła żądanie AJAX. W przykładowej aplikacji cały kod aplikacji JavaScript znajduje się w pliku Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Dopóki użytkownik nie zaloguje się, nie ma tokenu okaziciela ani nagłówka autoryzacji w żądaniu. Powoduje to, że żądanie zwróci błąd 401.

Oto żądanie HTTP. (Użyto [programu Fiddler](http://www.telerik.com/fiddler) do przechwycenia ruchu HTTP).

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Należy zauważyć, że odpowiedź zawiera nagłówek WWW-Authenticate z wyzwaniem ustawionym na Bearer. Wskazuje, że serwer oczekuje tokenu okaziciela.

## <a name="register-a-user"></a>Rejestrowanie użytkownika

W sekcji **zarejestruj** aplikacji wprowadź adres e-mail i hasło, a następnie kliknij przycisk **zarejestruj** .

Nie musisz używać prawidłowego adresu e-mail dla tego przykładu, ale rzeczywista aplikacja potwierdzi adres. (Zobacz [Tworzenie aplikacji sieci Web secure ASP.NET MVC 5 z logowaniem, potwierdzeniem wiadomości e-mail i resetowaniem hasła](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)). Dla hasła Użyj elementu like "Password1!" z wielką literą, małą literą, cyfrą i znakiem innym niż alfanumeryczny. Aby zachować prostą aplikację, po stronie klienta został pozostawiony Walidacja, dlatego jeśli wystąpi problem z formatem hasła, zostanie wyświetlony błąd 400 (Nieprawidłowe żądanie).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Przycisk **register** wysyła żądanie post do ~/API/Account/register/. Treść żądania jest obiektem JSON, który zawiera nazwę i hasło. Oto kod JavaScript, który wysyła żądanie:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Żądanie HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

To żądanie jest obsługiwane przez klasę `AccountController`. Wewnętrznie program `AccountController` używa ASP.NET Identity do zarządzania bazą danych członkostwa.

Jeśli aplikacja jest uruchamiana lokalnie z programu Visual Studio, konta użytkowników są przechowywane w LocalDB w tabeli AspNetUsers. Aby wyświetlić tabele w programie Visual Studio, kliknij menu **Widok** , wybierz pozycję **Eksplorator serwera**, a następnie rozwiń węzeł **połączenia danych**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Uzyskiwanie tokenu dostępu

Do tej pory nie wykonano żadnego uwierzytelniania OAuth, ale teraz zobaczymy serwer autoryzacji OAuth w akcji, gdy wyślemy żądanie tokenu dostępu. W obszarze **Logowanie** w przykładowej aplikacji wprowadź adres e-mail i hasło, a następnie kliknij przycisk **Zaloguj**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Przycisk **Zaloguj** wysyła żądanie do punktu końcowego tokenu. Treść żądania zawiera następujące dane zakodowane w adresie URL:

- Udziel\_typ: "hasło"
- Nazwa użytkownika: &lt;&gt; e-mail użytkownika
- hasło: &lt;hasło&gt;

Oto kod JavaScript, który wysyła żądanie AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Jeśli żądanie zakończy się powodzeniem, serwer autoryzacji zwróci token dostępu w treści odpowiedzi. Zwróć uwagę, że przechowujemy token w magazynie sesji, aby użyć go później podczas wysyłania żądań do interfejsu API. W przeciwieństwie do niektórych form uwierzytelniania (takich jak uwierzytelnianie na podstawie plików cookie), przeglądarka nie będzie automatycznie dołączać tokenu dostępu do kolejnych żądań. Aplikacja musi wykonać tę czynność jawnie. Jest to dobry efekt, ponieważ ogranicza [CSRFe luki w zabezpieczeniach](preventing-cross-site-request-forgery-csrf-attacks.md).

Żądanie HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Zobaczysz, że żądanie zawiera poświadczenia użytkownika. Aby zapewnić bezpieczeństwo warstwy transportu, *należy* użyć protokołu HTTPS.

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

W celu zapewnienia czytelności zwiększę Wcięcie kodu JSON i obciął token dostępu, który jest dość długi.

Właściwości `access_token`, `token_type`i `expires_in` są zdefiniowane przez specyfikację OAuth2. Inne właściwości (`userName`, `.issued`i `.expires`) są przeznaczone tylko do celów informacyjnych. Można znaleźć kod, który dodaje te dodatkowe właściwości w metodzie `TokenEndpoint`, w pliku/Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Wyślij uwierzytelnione żądanie

Teraz, gdy mamy token okaziciela, możemy wysłać uwierzytelnione żądanie do interfejsu API. W tym celu należy ustawić nagłówek autoryzacji w żądaniu. Kliknij ponownie przycisk **Wywołaj interfejs API** , aby zobaczyć ten temat.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Żądanie HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Wyloguj

Ponieważ przeglądarka nie buforuje poświadczeń lub token dostępu, wylogowanie jest po prostu kwestią "zapominanie" tokenu, usuwając go z magazynu sesji:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Zrozumienie szablonu projektu poszczególnych kont

W przypadku wybrania **poszczególnych kont** w szablonie projektu aplikacji sieci Web ASP.NET projekt zawiera następujące pozycje:

- Serwer autoryzacji OAuth2.
- Punkt końcowy interfejsu API sieci Web służący do zarządzania kontami użytkowników
- Model EF służący do przechowywania kont użytkowników.

Poniżej przedstawiono główne klasy aplikacji, które implementują te funkcje:

- `AccountController`. Udostępnia punkt końcowy interfejsu API sieci Web do zarządzania kontami użytkowników. Akcja `Register` jest jedyną, która została użyta w tym samouczku. Inne metody klasy obsługują resetowanie haseł, logowanie społecznościowe i inne funkcje.
- `ApplicationUser`, zdefiniowane w/Models/IdentityModels.cs. Ta klasa jest modelem EF dla kont użytkowników w bazie danych członkostwa.
- `ApplicationUserManager`, zdefiniowane w/App\_Start/IdentityConfig. cs, ta klasa pochodzi od [użytkownikamanager](https://msdn.microsoft.com/library/dn613290.aspx) i wykonuje operacje na kontach użytkowników, takie jak tworzenie nowego użytkownika, weryfikowanie haseł itd. i automatyczne Utrwalanie zmian w bazie danych.
- `ApplicationOAuthProvider`. Ten obiekt jest przyłączany do oprogramowania pośredniczącego OWIN i przetwarza zdarzenia zgłoszone przez oprogramowanie pośredniczące. Pochodzi on z [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Konfigurowanie serwera autoryzacji

W programie StartupAuth.cs Poniższy kod służy do konfigurowania serwera autoryzacji OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

Właściwość `TokenEndpointPath` jest ścieżką URL do punktu końcowego serwera autoryzacji. To jest adres URL, za pomocą którego aplikacja uzyskuje tokeny okaziciela.

Właściwość `Provider` określa dostawcę, który jest podłączany do oprogramowania pośredniczącego OWIN i przetwarza zdarzenia zgłoszone przez oprogramowanie pośredniczące.

Oto podstawowy przepływ, gdy aplikacja chce uzyskać token:

1. Aby uzyskać token dostępu, aplikacja wysyła żądanie do ~/token.
2. Oprogramowanie pośredniczące OAuth `GrantResourceOwnerCredentials` na dostawcy.
3. Dostawca wywołuje `ApplicationUserManager`, aby sprawdzić poprawność poświadczeń i utworzyć tożsamość oświadczeń.
4. Jeśli to się powiedzie, dostawca utworzy bilet uwierzytelniania, który jest używany do generowania tokenu.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Oprogramowanie pośredniczące OAuth nie wie niczego o kontach użytkowników. Dostawca komunikuje się między oprogramowanie pośredniczące a ASP.NET Identity. Aby uzyskać więcej informacji na temat implementowania serwera autoryzacji, zobacz [Owin OAuth 2,0 serwer autoryzacji](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Konfigurowanie interfejsu API sieci Web do używania tokenów okaziciela

W metodzie `WebApiConfig.Register` następujący kod konfiguruje uwierzytelnianie dla potoku interfejsu API sieci Web:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

Klasa **HostAuthenticationFilter** umożliwia uwierzytelnianie przy użyciu tokenów okaziciela.

Metoda **SuppressDefaultHostAuthentication** informuje internetowy interfejs API, aby ignorował wszelkie uwierzytelnienia, które są wykonywane przed przekroczeniem przez żądanie potoku interfejsu API sieci Web przez usługi IIS lub przez oprogramowanie pośredniczące Owin. Dzięki temu można ograniczyć interfejs API sieci Web do uwierzytelniania tylko przy użyciu tokenów okaziciela.

> [!NOTE]
> W szczególności część MVC aplikacji może korzystać z uwierzytelniania formularzy, w którym są przechowywane poświadczenia w pliku cookie. Uwierzytelnianie na podstawie plików cookie wymaga używania tokenów chroniących przed fałszowaniem, aby zapobiec atakom CSRF. Jest to problem dotyczący interfejsów API sieci Web, ponieważ nie istnieje wygodny sposób, aby internetowy interfejs API wysyłał do klienta token chroniący przed fałszerstwem. (Aby uzyskać więcej informacji na temat tego problemu, zobacz [zapobieganie atakom CSRF w interfejsie API sieci Web](preventing-cross-site-request-forgery-csrf-attacks.md)). Wywołanie **SuppressDefaultHostAuthentication** gwarantuje, że interfejs API sieci Web nie jest narażony na ataki CSRF z poświadczeń przechowywanych w plikach cookie.

Gdy klient żąda chronionego zasobu, Oto co dzieje się w potoku interfejsu API sieci Web:

1. Filtr **HostAuthentication** wywołuje oprogramowanie pośredniczące OAuth w celu zweryfikowania tokenu.
2. Oprogramowanie pośredniczące konwertuje token na tożsamość oświadczeń.
3. W tym momencie żądanie jest *uwierzytelniane* , ale nie ma *autoryzacji*.
4. Filtr autoryzacji bada tożsamość oświadczeń. Jeśli oświadczenia autoryzują użytkownika dla tego zasobu, żądanie jest autoryzowane. Domyślnie atrybut **[autoryzuje]** autoryzuje wszelkie uwierzytelnione żądania. Można jednak autoryzować role lub inne oświadczenia. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie i autoryzacja w internetowym interfejsie API](authentication-and-authorization-in-aspnet-web-api.md).
5. Jeśli poprzednie kroki zakończą się pomyślnie, kontroler zwróci chroniony zasób. W przeciwnym razie klient otrzymuje błąd 401 (nieautoryzowany).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Dodatkowe materiały

- [ASP.NET Identity](../../../identity/index.md)
- [Informacje o funkcjach zabezpieczeń w szablonie spa dla VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Wpis w blogu MSDN przez Hongye Sun.
- [Odcięta szablonu poszczególnych kont internetowego interfejsu API — część 2: konta lokalne](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Wpis w blogu według Dominick Baier.
- [Uwierzytelnianie hosta i interfejs API sieci Web za pomocą usługi Owin](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Dobre wyjaśnienie `SuppressDefaultHostAuthentication` i `HostAuthenticationFilter` przez Brock.
- [Dostosowywanie informacji o profilu w ASP.NET Identity szablonów programu VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Wpis w blogu MSDN według Pranav Rastogi.
- [Zarządzanie okresem istnienia żądania dla klasy usermanager w ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Wpis w blogu MSDN przez Suhas Joshi, z dobrym wyjaśnieniem klasy `UserManager`.
