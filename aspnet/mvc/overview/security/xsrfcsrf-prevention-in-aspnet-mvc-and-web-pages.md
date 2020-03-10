---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: XSRF/CSRF zapobieganie w ASP.NET MVC i stronach sieci Web | Microsoft Docs
author: Rick-Anderson
description: Sfałszowanie żądań między lokacjami (znane również jako XSRF lub CSRF) jest atakiem na aplikacje hostowane w sieci Web, dzięki czemu złośliwa witryna sieci Web może mieć wpływ na interacti...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 1965063a9b613d0e2857cddcc2165f5fda64ec0c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559354"
---
# <a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Zapobieganie atakom XSRF/CSRF we wzorcach ASP.NET MVC i Web Pages

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Sfałszowanie żądań między lokacjami (znane również jako XSRF lub CSRF) jest atakiem na aplikacje hostowane w sieci Web, dzięki czemu złośliwa witryna sieci Web może mieć wpływ na interakcję między przeglądarką klienta i witryną sieci Web zaufaną w tej przeglądarce. Te ataki są możliwe, ponieważ przeglądarki sieci Web automatycznie będą wysyłać tokeny uwierzytelniania przy użyciu każdego żądania do witryny sieci Web. Przykład kanoniczny to plik cookie uwierzytelniania, taki jak ASP. Bilet uwierzytelniania formularzy netto. Jednak w przypadku witryn sieci Web korzystających z dowolnego mechanizmu uwierzytelniania trwałego (takiego jak uwierzytelnianie systemu Windows, podstawowa i tak dalej) mogą być objęte tymi atakami.
> 
> Atak XSRF różni się od ataku wyłudzania informacji. Ataki wyłudzające informacje wymagają interakcji z ofiarą. W przypadku ataku wyłudzania informacji złośliwa witryna sieci Web będzie naśladować docelową witrynę sieci Web, a ofiara jest niezależna od udostępnienia osobie atakującej. W przypadku ataku XSRF często nie ma potrzeby interakcji z ofiarą. Zamiast tego osoba atakująca polega na tym, że przeglądarka automatycznie wysyła wszystkie odpowiednie pliki cookie do docelowej witryny sieci Web.
> 
> Aby uzyskać więcej informacji, zobacz artykuł [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).

## <a name="anatomy-of-an-attack"></a>Anatomia ataku

Aby zapoznać się z atakiem XSRF, weź pod uwagę użytkownika, który chce wykonywać pewne transakcje bankowości online. Ten użytkownik najpierw odwiedza WoodgroveBank.com i loguje się, w którym momencie nagłówek odpowiedzi będzie zawierać plik cookie uwierzytelniania:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Ponieważ plik cookie uwierzytelniania to plik cookie sesji, zostanie automatycznie wyczyszczony przez przeglądarkę po zakończeniu procesu przeglądarki. Jednak do tego czasu przeglądarka automatycznie uwzględni plik cookie z każdym żądaniem do WoodgroveBank.com. Użytkownik chce teraz przenieść $1000 do innego konta, aby wypełnił formularz w witrynie bankowości, a przeglądarka przetworzy to żądanie na serwerze:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Ponieważ ta operacja ma efekt uboczny (inicjuje transakcję pieniężną), witryna bankowa zdecydowała się na żądanie POST protokołu HTTP w celu zainicjowania tej operacji. Serwer odczytuje token uwierzytelniania z żądania, wyszukuje numer konta bieżącego użytkownika, sprawdza, czy istnieją wystarczające środki, a następnie inicjuje transakcję na koncie docelowym.

Twoja bankowość online została ukończona, ale użytkownik przechodzi poza witrynę bankową i odwiedza inne lokalizacje w sieci Web. Jedna z tych lokacji — fabrikam.com — zawiera następujące znaczniki na stronie osadzonej w &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Spowoduje to, że przeglądarka przeprowadzi to żądanie:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Osoba atakująca korzysta z faktu, że użytkownik może nadal mieć prawidłowy token uwierzytelniania dla docelowej witryny sieci Web i używa małego fragmentu kodu w języku JavaScript, aby spowodować, że przeglądarka automatycznie wprowadzi polecenie HTTP do lokacji docelowej. Jeśli token uwierzytelniania jest nadal ważny, witryna bankowa będzie inicjować transfer $250 do konta osoby atakującej.

### <a name="ineffective-mitigations"></a>Nieskuteczne środki zaradcze

Należy pamiętać, że w powyższym scenariuszu fakt, że WoodgroveBank.com był uzyskiwany za pośrednictwem protokołu SSL i miał plik cookie z uwierzytelnianiem SSL, był za mały, aby zablokować atak. Osoba atakująca może określić [schemat URI](http://en.wikipedia.org/wiki/URI_scheme) (https) w jego &lt;formularzu&gt; element, a przeglądarka będzie nadal wysyłać niewygasłe pliki cookie do lokacji docelowej, o ile te pliki cookie są spójne z SCHEMATem URI zamierzonego obiektu docelowego.

Jednym z nich może być to, że użytkownik nie musi odwiedzać niezaufanych witryn, ponieważ odwiedzający mogą pozostać w trybie online tylko w przypadku, gdy tylko Zaufane witryny. Jest to pewne rozwiązanie, ale niestety to zalecenie nie zawsze jest praktyczne. Być może użytkownik "ufa" w lokalnej witrynie wiadomości ConsolidatedMessenger. ConsolidatedMessenger.com i nastąpi odwiedzenie tej lokacji, ale ta witryna ma lukę w zabezpieczeniach, która umożliwia osobie atakującej wstrzyknięcie tego samego fragmentu kodu, który był uruchomiony w fabrikam.com.

Można sprawdzić, czy żądania przychodzące mają [nagłówek odwołującego](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) odwołujący się do Twojej domeny. Spowoduje to zatrzymanie żądań przypadkowego przesłanych z domeny innej firmy. Jednak niektórzy użytkownicy wyłączają nagłówek osoby odwołującej przeglądarki w celu zachowania poufności informacji, a osoby atakujące mogą czasami sfałszować ten nagłówek, jeśli ofiarą zainstalowano pewne niezabezpieczone oprogramowanie. Weryfikowanie [nagłówka odwołującego](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) nie jest traktowane jako bezpieczne podejście uniemożliwiające ataki XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Środki zaradcze XSRF środowiska uruchomieniowego stosu sieci Web

Środowisko uruchomieniowe stosu sieci Web ASP.NET używa wariantu [wzorca tokenu synchronizatora](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#synchronizer-token-pattern) do obrony przed atakami XSRF. Ogólna postać wzorca tokenów synchronizatora polega na tym, że dwa tokeny XSRF są przesyłane do serwera z każdym WPISem HTTP (oprócz tokenu uwierzytelniania): jeden token jako plik cookie, a drugi jako wartość formularza. Wartości tokenów wygenerowane przez środowisko uruchomieniowe ASP.NET nie są deterministyczne ani przewidywalne przez osobę atakującą. Po przesłaniu tokenów serwer zezwoli na żądanie, tylko wtedy, gdy oba tokeny przechodzą sprawdzanie porównania.

*Token sesji* weryfikacji żądania XSRF jest przechowywany jako plik cookie protokołu HTTP i obecnie zawiera następujące informacje w jego ładunku:

- Token zabezpieczający składający się z losowego identyfikatora 128-bitowego.   
 Na poniższej ilustracji przedstawiono token sesji weryfikacji żądania XSRF wyświetlany z narzędziami deweloperskimi programu Internet Explorer F12: (należy zauważyć, że jest to bieżąca implementacja, nawet prawdopodobnie, aby zmienić).

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*Token pola* jest przechowywany jako `<input type="hidden" />` i zawiera następujące informacje w jego ładunku:

- Nazwa użytkownika logowania (w przypadku uwierzytelnienia).
- Wszelkie dodatkowe dane dostarczone przez [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Ładunki tokenów XSRF są szyfrowane i podpisane, więc nie można wyświetlić nazwy użytkownika podczas korzystania z narzędzi do badania tokenów. Gdy aplikacja sieci Web ma znaczenie ASP.NET 4,0, usługi kryptograficzne są udostępniane przez procedurę [machineKey. Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) . Gdy aplikacja sieci Web ma wartość ASP.NET 4,5 lub wyższą, usługi kryptograficzne są udostępniane przez procedurę [machineKey. Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) , która oferuje lepszą wydajność, rozszerzalność i bezpieczeństwo. Więcej informacji można znaleźć w następujących wpisach w blogu:

- [Udoskonalenia kryptograficzne w ASP.NET 4,5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Udoskonalenia kryptograficzne w ASP.NET 4,5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Udoskonalenia kryptograficzne w ASP.NET 4,5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generowanie tokenów

Aby wygenerować tokeny XSRF, wywołaj metodę [@Html.AntiForgeryToken](https://msdn.microsoft.com/library/dd470175.aspx) z widoku MVC lub @AntiForgery.GetHtml() ze strony Razor. Środowisko uruchomieniowe następnie wykona następujące czynności:

1. Jeśli bieżące żądanie HTTP zawiera już token sesji XSRF (plik cookie XSRF \_\_RequestVerificationToken), token zabezpieczający zostanie wyodrębniony z tego elementu. Jeśli żądanie HTTP nie zawiera tokenu sesji XSRF lub jeśli wyodrębnienie tokenu zabezpieczeń nie powiedzie się, zostanie wygenerowany nowy losowy token antyXSRF.
2. Token pola XSRF jest generowany przy użyciu tokenu zabezpieczającego z kroku (1) powyżej i tożsamości bieżącego zalogowanego użytkownika. (Aby uzyskać więcej informacji na temat określania tożsamości użytkownika, zobacz sekcję **[scenariusze z specjalną pomocą techniczną](#_Scenarios_with_special)** poniżej). Ponadto, jeśli [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) jest skonfigurowany, środowisko uruchomieniowe wywoła metodę [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) i uwzględni zwracany ciąg w tokenie pola. (Aby uzyskać więcej informacji, zobacz sekcję **[Konfiguracja i rozszerzalność](#_Configuration_and_extensibility)** ).
3. Jeśli nowy token XSRF został wygenerowany w kroku (1), zostanie utworzony nowy token sesji, który będzie zawierał i zostanie dodany do kolekcji wychodzących plików cookie protokołu HTTP. Token pola z kroku (2) zostanie opakowany w `<input type="hidden" />` element, a ten znacznik HTML będzie wartością zwracaną `Html.AntiForgeryToken()` lub `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Sprawdzanie poprawności tokenów

Aby zweryfikować przychodzące tokeny XSRF, deweloper zawiera atrybut [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) w jego akcji lub kontrolerze MVC lub wywołuje `@AntiForgery.Validate()` ze strony Razor. Środowisko uruchomieniowe wykona następujące czynności:

1. Token sesji przychodzących i token pola są odczytywane i token XSRF wyodrębniony z każdego z nich. Tokeny XSRF muszą być identyczne na krok (2) w procedurze generacji.
2. Jeśli bieżący użytkownik jest uwierzytelniony, jego nazwa użytkownika jest porównywana z nazwą użytkownika przechowywaną w tokenie pola. Nazwy użytkowników muszą być zgodne.
3. Jeśli [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) jest skonfigurowany, środowisko uruchomieniowe wywołuje metodę *ValidateAdditionalData* . Metoda musi zwracać wartość logiczną *prawda*.

Jeśli sprawdzanie poprawności zakończy się pomyślnie, żądanie może zostać wznowione. Jeśli walidacja nie powiedzie się, struktura zgłosi *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Warunki niepowodzenia

Począwszy od programu ASP.NET Web Stack Runtime v2, wszelkie *HttpAntiForgeryException* , które są generowane podczas weryfikacji, zawierają szczegółowe informacje na temat tego, co poszło źle. Obecnie zdefiniowane warunki błędu to:

- Token sesji lub token formularza nie występuje w żądaniu.
- Token sesji lub token formularza nie można odczytać. Najbardziej prawdopodobną przyczyną jest farma z niezgodnymi wersjami środowiska uruchomieniowego stosu sieci Web ASP.NET lub farmy, w których element&gt; &lt;machineKey w pliku Web. config różni się między maszynami. Możesz użyć narzędzia, takiego jak programu Fiddler, aby wymusić ten wyjątek poprzez naruszenie tokenu antyXSRFowego.
- Zamieniono token sesji i token pola.
- Token sesji i token pola zawierają niezgodne tokeny zabezpieczające.
- Nazwa użytkownika osadzona w tokenie pola nie jest zgodna z nazwą bieżącego zalogowanego użytkownika.
- Metoda *[IAntiForgeryAdditionalDataProvider. ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* zwróciła *wartość false*.

Urządzenia chroniące przed XSRFą mogą również przeprowadzać dodatkowe sprawdzanie podczas generowania tokenu lub walidacji, a błędy podczas tych testów mogą spowodować zgłoszenie wyjątków. Aby uzyskać więcej informacji, zobacz sekcję [WIF/ACS/uwierzytelnianie i rozszerzalność opartą na oświadczeniach](#_WIF_ACS) . **[](#_Configuration_and_extensibility)**

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scenariusze z specjalną pomocą techniczną

### <a name="anonymous-authentication"></a>Uwierzytelnianie anonimowe

System XSRF zawiera specjalną pomoc techniczną dla użytkowników anonimowych, gdzie "anonimowa" jest definiowana jako użytkownik, gdzie Właściwość *IIdentity. isauthenticate* zwraca *wartość false*. Scenariusze obejmują zapewnienie ochrony XSRF na stronie logowania (przed uwierzytelnieniem użytkownika) oraz niestandardowych schematów uwierzytelniania, w których aplikacja używa mechanizmu innego niż *IIdentity* do identyfikowania użytkowników.

Aby obsługiwać te scenariusze, należy odwołać się, że tokeny sesji i pól są przyłączone przez token zabezpieczający, który jest 128-bitowym losowo generowanym identyfikatorem nieprzezroczystym. Ten token zabezpieczający służy do śledzenia sesji poszczególnych użytkowników w trakcie nawigowania po lokacji, dlatego efektywnie służy do przeznaczenie anonimowego identyfikatora. Pusty ciąg jest używany zamiast nazwy użytkownika dla procedur generowania i walidacji opisanych powyżej.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF/ACS/uwierzytelnianie oparte na oświadczeniach

Zwykle klasy *IIdentity* wbudowane w .NET Framework mają właściwość, która *IIdentity.Name* jest wystarczająca, aby jednoznacznie identyfikować określonego użytkownika w danej aplikacji. Na przykład funkcja *FormsIdentity.Name* zwraca nazwę użytkownika przechowywaną w bazie danych członkostwa (która jest unikatowa dla wszystkich aplikacji w zależności od tej bazy danych), *WindowsIdentity.Name* zwraca tożsamość użytkownika i tak dalej. Systemy te nie tylko zapewniają uwierzytelnianie; *identyfikują* one również użytkowników do aplikacji.

Uwierzytelnianie oparte na oświadczeniach, z drugiej strony, nie wymaga identyfikacji określonego użytkownika. Zamiast tego typy *ClaimsPrincipal* i *Identyfikator oświadczenia* są skojarzone z zestawem wystąpień *oświadczeń* , w przypadku których poszczególne oświadczenia mogą mieć wartość "is 18 + lat wieku" lub "jest administratorem" do wszystkich innych. Ponieważ użytkownik nie musi być zidentyfikowany, środowisko uruchomieniowe nie może użyć właściwości *ClaimsIdentity.Name* jako unikatowego identyfikatora dla danego użytkownika. Zespół widzi rzeczywiste przykłady, w których *ClaimsIdentity.Name* zwraca *wartość null*, zwraca przyjazną nazwę (Display) lub w inny sposób zwraca ciąg, który nie jest odpowiedni do użycia jako unikatowy identyfikator użytkownika.

Wiele wdrożeń korzystających z uwierzytelniania opartego na oświadczeniach korzysta z [usługi Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS). Usługa ACS umożliwia deweloperom konfigurowanie indywidualnych *dostawców tożsamości* (na przykład usług AD FS, dostawcy konta Microsoft, dostawców OpenID Connect, takich jak Yahoo! itp.), a także dostawców tożsamości zwracają *identyfikatory nazw*. Identyfikatory nazw mogą zawierać dane osobowe, takie jak adres e-mail, lub mogą być anonimowe jak prywatny identyfikator osobisty (PPID). Bez względu na to, że krotka (dostawca tożsamości, identyfikator nazwy) wystarczająco służy jako odpowiedni token śledzenia dla danego użytkownika podczas przeglądania witryny, dlatego środowisko uruchomieniowe stosu internetowego ASP.NET może używać krotki zamiast nazwy użytkownika podczas generowania i Weryfikowanie tokenów pól XSRF. Określone identyfikatory URI dla dostawcy tożsamości i identyfikatora nazwy są następujące:

- `https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(zobacz tę [stronę dokumentacji ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) , aby uzyskać więcej informacji).

Podczas generowania lub weryfikowania tokenu środowisko uruchomieniowe stosu sieci Web ASP.NET rozpocznie tworzenie powiązań z typami:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (dla zestawu SDK WIF).
- `System.Security.Claims.ClaimsIdentity` (dla programu .NET 4,5).

Jeśli te typy istnieją, a w przypadku *IIIIdentity* bieżącego użytkownika lub podklasy jednego z tych typów, funkcja ochrony przed XSRF będzie używać krotki (dostawcy tożsamości, identyfikatora nazwy) zamiast nazwy użytkownika podczas generowania i weryfikowania tokenów. Jeśli taka spójna kolekcja nie jest obecna, żądanie zakończy się niepowodzeniem z powodu błędu opisującego dewelopera, jak skonfigurować system XSRF, aby zrozumieć konkretny mechanizm uwierzytelniania oparty na oświadczeniach w użyciu. Aby uzyskać więcej informacji, zobacz sekcję **[Konfiguracja i rozszerzalność](#_Configuration_and_extensibility)** .

### <a name="oauth--openid-authentication"></a>Uwierzytelnianie OAuth/OpenID Connect

Na koniec Funkcja Anti-XSRF ma specjalną obsługę aplikacji korzystających z uwierzytelniania OAuth lub OpenID Connect. Ta obsługa jest oparta na algorytmie heurystycznym: Jeśli bieżąca *IIdentity.Name* rozpoczyna się od http://lub https://, porównywanie nazw użytkowników zostanie wykonane przy użyciu funkcji porównującej porządkowej, a nie domyślnej funkcji porównującej OrdinalIgnoreCase.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Konfiguracja i rozszerzalność

Czasami deweloperzy mogą chcieć mieć ściślejszą kontrolę nad generowaniem i sprawdzaniem poprawności XSRF. Na przykład może to być niepożądane zachowanie domyślnych zachowań stron MVC i Webpages w celu automatycznego dodawania plików cookie HTTP do odpowiedzi Istnieją dwa interfejsy API, które ułatwiają to:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

Metoda *Gettokens* przyjmuje jako wejściowy istniejący token sesji weryfikacji żądania XSRF (który może mieć wartość null) i tworzy jako dane wyjściowe nowy token sesji weryfikacji żądania XSRF i token pola. Tokeny są po prostu nieprzezroczyste ciągi bez dekoracji; wartość *formToken* nie będzie opakowana na wystąpienie, w tagu&gt; Input &lt;. Wartość *newCookieToken* może być równa null; w takim przypadku wartość *oldCookieToken* jest nadal ważna i nie trzeba ustawiać nowego pliku cookie odpowiedzi. Obiekt wywołujący *Gettokens* jest odpowiedzialny za utrwalanie wszelkich niezbędnych plików cookie odpowiedzi lub generowanie wszelkich niezbędnych oznaczeń. sama metoda *Gettokens* nie zmieni odpowiedzi jako efektu bocznego. Metoda *Validate* pobiera przychodzące tokeny sesji i pól oraz uruchamia wyżej wymienioną logikę walidacji.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Deweloper może skonfigurować system antyXSRFowy z poziomu aplikacji Start\_. Konfiguracja jest programowa. Właściwości statycznego typu *AntiForgeryConfig* są opisane poniżej. Większość użytkowników korzystających z oświadczeń chce ustawić właściwość UniqueClaimTypeIdentifier.

| **Właściwość** | **Opis** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) , który zapewnia dodatkowe dane podczas generowania tokenu i zużywa dodatkowe dane podczas weryfikacji tokenu. Wartość domyślna to *null*. Aby uzyskać więcej informacji, zobacz sekcję [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) . |
| **Plik CookieName** | Ciąg, który zawiera nazwę pliku cookie HTTP, który jest używany do przechowywania tokenu sesji XSRF. Jeśli ta wartość nie jest ustawiona, nazwa zostanie wygenerowana automatycznie na podstawie wdrożonej ścieżki wirtualnej aplikacji. Wartość domyślna to *null*. |
| **RequireSsl** | Wartość logiczna określająca, czy tokeny XSRF są wymagane do przesłania przez kanał zabezpieczony za pośrednictwem protokołu SSL. Jeśli ta wartość jest *równa true*, wszystkie automatycznie generowane pliki cookie będą miały ustawioną flagę "Secure", a interfejsy API XSRF będą zgłaszane w przypadku wywołania z żądania, które nie jest przesyłane za pośrednictwem protokołu SSL. Wartość domyślna to *false*. |
| **SuppressIdentityHeuristicChecks** | Wartość logiczna określająca, czy system antyXSRF powinien dezaktywować swoją obsługę tożsamości opartych na oświadczeniach. Jeśli ta wartość jest *równa true*, system przyjmie, że *IIdentity.Name* jest odpowiednie do użycia jako unikatowy identyfikator dla użytkownika i nie spróbuje specjalnej wielkości liter *IClaimsIdentity* lub *ClClaimsIdentity* zgodnie z opisem w sekcji [uwierzytelnianie na podstawie WIF/ACS/oświadczenia](#_WIF_ACS) . Wartością domyślną jest `false`. |
| **UniqueClaimTypeIdentifier** | Ciąg, który wskazuje, który typ zgłoszenia jest odpowiedni do użycia jako unikatowy identyfikator dla każdego użytkownika. Jeśli ta wartość jest ustawiona, a bieżący *IIdentity* jest oparty na oświadczeniach, system podejmie próbę wyodrębnienia oświadczenia typu określonego przez *UniqueClaimTypeIdentifier*, a odpowiednia wartość zostanie użyta zamiast nazwy użytkownika podczas generowania tokenu pola. Jeśli typ zgłoszenia nie zostanie znaleziony, system nie będzie mógł wykonać żądania. Wartość domyślna to *null*, co oznacza, że system powinien używać spójnej kolekcji (dostawcy tożsamości, identyfikatora nazwy), jak opisano wcześniej w miejscu nazwy użytkownika. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

Typ *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* umożliwia deweloperom zwiększenie zachowania systemu antyXSRFowego przez dwukrotne wyzwolenie dodatkowych danych w każdym tokenie. Metoda *GetAdditionalData* jest wywoływana za każdym razem, gdy generowany jest token pola, a zwracana wartość jest osadzona w wygenerowanym tokenie. Realizator może zwrócić sygnaturę czasową, identyfikator jednorazowy lub inną wartość żądaną przez tę metodę.

Podobnie Metoda *ValidateAdditionalData* jest wywoływana za każdym razem, gdy token pola jest sprawdzany, a ciąg "dodatkowe dane", który został osadzony w tokenie jest przekazano do metody. Procedura walidacji może zaimplementować limit czasu (sprawdzając bieżący czas w czasie, gdy token został utworzony), procedurę sprawdzania nonce lub inną żądaną logikę.

## <a name="design-decisions-and-security-considerations"></a>Decyzje projektowe i zagadnienia dotyczące zabezpieczeń

Token zabezpieczający łączący tokeny sesji i pola jest technicznie tylko konieczny podczas próby ochrony anonimowych/nieuwierzytelnionych użytkowników przed atakami XSRF. Po uwierzytelnieniu użytkownika token uwierzytelniania (przypuszczalnie przesyłane w postaci pliku cookie) może być używany jako jedna połowa pary tokenów synchronizatora. Istnieją jednak prawidłowe scenariusze ochrony stron logowania trafień przez nieuwierzytelnionych użytkowników, a logika XSRF była łatwiejsza przez zawsze generowanie i weryfikowanie tokenów zabezpieczających nawet dla użytkowników uwierzytelnionych. Ponadto zapewnia dodatkową ochronę w przypadku, gdy osoba atakująca kiedykolwiek narusza zabezpieczenia tokenu pola, ponieważ ustawienie lub odgadnięcie tokenu sesji byłyby innym progiem dla ataku.

Deweloperzy powinni ostrożnie korzystać, gdy wiele aplikacji jest hostowanych w jednej domenie. Na przykład mimo że *example1.cloudapp.NET* i *example2.cloudapp.NET* są różnymi hostami, istnieje niejawna relacja zaufania między wszystkimi hostami w domenie *\*. cloudapp.NET* . Niejawna relacja zaufania [umożliwia niezaufanym hostom wpływanie na wszystkie inne pliki cookie](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (zasady tego samego źródła, które zarządza żądaniami AJAX, nie muszą być stosowane do plików cookie HTTP). Środowisko uruchomieniowe stosu sieci Web ASP.NET zapewnia pewne środki zaradcze, że nazwa użytkownika jest osadzona w tokenie pola, więc nawet jeśli złośliwa poddomena może zastąpić token sesji, nie będzie w stanie wygenerować prawidłowego tokenu pola dla użytkownika. Jednakże, jeśli są one hostowane w takich środowiskach, wbudowane procedury XSRF nadal nie mogą chronić przed przejęciem lub logowaniem do sesji.

Procedury chroniące przed XSRFą obecnie nie są obrony przed [clickjacking](https://www.owasp.org/index.php/Clickjacking). Aplikacje, które chcą się chronić przed clickjackingem, mogą łatwo to zrobić, wysyłając nagłówek SAMEORIGIN z każdą odpowiedzią. Ten nagłówek jest obsługiwany przez wszystkie ostatnie przeglądarki. Aby uzyskać więcej informacji, zobacz [blog IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [blog SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)i [OWASP](https://www.owasp.org/index.php/Clickjacking). Środowisko uruchomieniowe stosu sieci Web ASP.NET może w niektórych przyszłych wersjach sprawić, że pomocniki narzędzi XSRF i stron sieci Web będą automatycznie ustawiać ten nagłówek, dzięki czemu aplikacje są automatycznie chronione przed atakami.

Deweloperzy sieci Web powinni nadal upewnić się, że ich lokacja nie jest narażona na ataki XSS. Ataki XSS są bardzo wydajne, a pomyślnie wykorzystujące luki w zabezpieczeniach spowodują również przerwanie obrony ASP.NET środowiska uruchomieniowego stosu sieci Web na ataki XSRF.

## <a name="acknowledgment"></a>Dziękowanie

[@LeviBroderick](https://twitter.com/LeviBroderick), którzy zapisały wiele ASP.NET kodu zabezpieczeń, aby uzyskać więcej informacji.
