---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Włączanie żądań między źródłami w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Pokazuje, jak obsługiwać udostępnianie zasobów między źródłami (CORS) w interfejsie Web API ASP.NET.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555707"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>Włączanie żądań między źródłami w programie ASP.NET Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

> Zabezpieczenia przeglądarki uniemożliwiają stronie internetowej wysyłanie żądań AJAX do innej domeny. To ograniczenie jest nazywane *zasadami tego samego źródła*i uniemożliwia złośliwym lokacjom odczytywanie poufnych danych z innej lokacji. Czasami jednak może być konieczne, aby inne Lokacje wywoływały internetowy interfejs API.
>
> [Udostępnianie zasobów między źródłami](http://www.w3.org/TR/cors/) (CORS) jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła. Przy użyciu mechanizmu CORS serwer może jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych. Mechanizm CORS jest bezpieczniejszy i bardziej elastyczny niż wcześniejsze techniki, takie jak [JSONP](http://en.wikipedia.org/wiki/JSONP). W tym samouczku pokazano, jak włączyć mechanizm CORS w aplikacji interfejsu API sieci Web.
>
> ## <a name="software-used-in-the-tutorial"></a>Oprogramowanie używane w samouczku
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Internetowy interfejs API 2,2

## <a name="introduction"></a>Wprowadzenie

W tym samouczku przedstawiono obsługę mechanizmu CORS w interfejsie API sieci Web ASP.NET. Zaczniemy od utworzenia dwóch projektów ASP.NET — jeden o nazwie "WebService", który hostuje kontroler interfejsu API sieci Web, a drugi o nazwie "WebClient", który wywołuje usługę WebService. Ponieważ dwie aplikacje są hostowane w różnych domenach, żądanie AJAX od klienta WebClient do usługi WebService jest żądaniem między źródłami.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Co to jest "to samo Źródło"?

Dwa adresy URL mają te same źródła, jeśli mają identyczne schematy, hosty i porty. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Te dwa adresy URL mają te same źródła:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Te adresy URL mają różne źródła niż poprzednie dwa:

- `http://example.net` — inna domena
- `http://example.com:9000/foo.html` — inny port
- `https://example.com/foo.html` — inny schemat
- `http://www.example.com/foo.html` — inna poddomena

> [!NOTE]
> Program Internet Explorer nie traktuje portu podczas porównywania źródeł.

## <a name="create-the-webservice-project"></a>Tworzenie projektu usługi WebService

> [!NOTE]
> W tej sekcji założono, że wiesz już, jak tworzyć projekty interfejsu API sieci Web. Jeśli nie, zobacz [wprowadzenie z interfejsem API sieci Web ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Uruchom program Visual Studio i Utwórz nowy projekt **aplikacji sieci Web ASP.NET (.NET Framework)** .
2. W oknie dialogowym **Nowa aplikacja sieci Web ASP.NET** wybierz szablon **pusty** projekt. W obszarze **Dodaj foldery i podstawowe odwołania dla programu**zaznacz pole wyboru **internetowy interfejs API** .

   ![Nowe okno dialogowe projektu ASP.NET w programie Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Dodaj kontroler interfejsu API sieci Web o nazwie `TestController` przy użyciu następującego kodu:

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. Aplikację można uruchomić lokalnie lub wdrożyć na platformie Azure. (W przypadku zrzutów ekranu w tym samouczku aplikacja zostanie wdrożona w Azure App Service Web Apps). Aby sprawdzić, czy interfejs API sieci Web działa, przejdź do `http://hostname/api/test/`, gdzie *hostname* jest domeną, w której została wdrożona aplikacja. Powinien pojawić się tekst odpowiedzi, &quot;GET: Komunikat testowy&quot;.

   ![Przeglądarka sieci Web wyświetlająca komunikat testowy](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>Tworzenie projektu WebClient

1. Utwórz inny projekt **aplikacji sieci Web ASP.NET (.NET Framework)** i wybierz szablon projektu **MVC** . Opcjonalnie możesz wybrać pozycję **Zmień uwierzytelnianie** > **bez uwierzytelniania**. Nie musisz uwierzytelniać się w tym samouczku.

   ![Szablon MVC w nowym oknie dialogowym projektu ASP.NET w programie Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. W **Eksplorator rozwiązań**Otwórz plik *widoki/Home/index. cshtml*. Zastąp kod w tym pliku zgodnie z poniższym przykładem:

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   Dla zmiennej *serviceUrl* Użyj identyfikatora URI aplikacji usługi WebService.

3. Uruchom aplikację WebClient lokalnie lub opublikuj ją w innej witrynie sieci Web.

Po kliknięciu przycisku "Wypróbuj" zostanie przesłane żądanie AJAX do aplikacji usługi WebService przy użyciu metody HTTP wymienionej w polu listy rozwijanej (GET, POST lub PUT). Pozwala to na badanie różnych żądań między źródłami. Obecnie aplikacja usługi WebService nie obsługuje mechanizmu CORS, więc po kliknięciu przycisku zostanie wyświetlony komunikat o błędzie.

![Błąd "try" w przeglądarce](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Jeśli monitorujesz ruch HTTP w narzędziu takim jak [programu Fiddler](https://www.telerik.com/fiddler), zobaczysz, że przeglądarka wysyła żądanie Get, a żądanie jest pomyślne, ale wywołanie AJAX zwraca błąd. Ważne jest, aby zrozumieć, że zasady tego samego źródła nie uniemożliwiają *wysłania* żądania przez przeglądarkę. Zamiast tego uniemożliwia aplikacji wyświetlanie *odpowiedzi*.

![Debuger sieci Web programu Fiddler pokazujący żądania sieci Web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>Włączanie mechanizmu CORS

Teraz włączmy mechanizm CORS w aplikacji usługi WebService. Najpierw Dodaj pakiet NuGet CORS. W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

To polecenie instaluje najnowszy pakiet i aktualizuje wszystkie zależności, w tym biblioteki podstawowych interfejsów API sieci Web. Użyj flagi `-Version`, aby określić wersję docelową. Pakiet CORS wymaga interfejsu Web API 2,0 lub nowszego.

Otwórz aplikację pliku *\_Start/WebApiConfig. cs*. Dodaj następujący kod do metody **WebApiConfig. Register** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Następnie Dodaj atrybut **[EnableCors]** do klasy `TestController`:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

W przypadku parametru *Origins* Użyj identyfikatora URI, w którym wdrożono aplikację WebClient. Pozwala to na żądania między źródłami z klienta WebClient, mimo że nie zezwala na wszystkie inne żądania między domenami. Później opisano parametry dla **[EnableCors]** w bardziej szczegółowy sposób.

Nie uwzględniaj ukośnika na końcu adresu URL *początkowych* elementów.

Wdróż ponownie zaktualizowaną aplikację sieci Web. Nie musisz aktualizować klienta WebClient. Teraz żądanie AJAX od klienta WebClient powinno zakończyć się pomyślnie. Metody GET, PUT i POST są dozwolone.

![Przeglądarka sieci Web przedstawiająca pomyślne wiadomości testowe](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>Jak działa mechanizm CORS

W tej sekcji opisano, co się dzieje w żądaniu CORS, na poziomie komunikatów HTTP. Ważne jest, aby zrozumieć, jak działa mechanizm CORS, dzięki czemu można prawidłowo skonfigurować atrybut **[EnableCors]** i rozwiązywać problemy, jeśli elementy nie działają zgodnie z oczekiwaniami.

Specyfikacja CORS zawiera kilka nowych nagłówków HTTP, które umożliwiają żądania między źródłami. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia te nagłówki automatycznie dla żądań cross-Origin; nie musisz wykonywać żadnych dodatkowych czynności w kodzie JavaScript.

Oto przykład żądania między źródłami. Nagłówek "Origin" zapewnia domenę lokacji, która żąda żądania.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Jeśli serwer zezwala na żądanie, ustawia nagłówek Access-Control-Allow-Origin. Wartość tego nagłówka jest zgodna z nagłówkiem pierwotnym lub jest wartością wieloznaczną "\*", co oznacza, że wszystkie źródła są dozwolone.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Jeśli odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX zakończy się niepowodzeniem. W programie przeglądarka nie zezwala na żądanie. Nawet jeśli serwer zwróci pomyślną odpowiedź, przeglądarka nie udostępni odpowiedzi dla aplikacji klienckiej.

**Żądania wstępnego lotu**

W przypadku niektórych żądań CORS przeglądarka wysyła dodatkowe żądanie o nazwie "żądanie wstępne", zanim wyśle ono rzeczywiste żądanie dotyczące zasobu.

Jeśli spełnione są następujące warunki, przeglądarka może pominąć żądanie wstępne:

- Metoda żądania jest GET, główna lub OPUBLIKOWANa, *a*
- Aplikacja nie ustawia żadnego nagłówka żądania innego niż Accept, Accept-Language, Content-language, Content-Type lub Last-Event-ID *oraz*
- Nagłówek Content-Type (jeśli jest ustawiony) jest jednym z następujących:

    - application/x-www-form-urlencoded
    - wieloczęściowe/formularz-dane
    - tekst/zwykły

Zasada dotycząca nagłówków żądań dotyczy nagłówków, które są ustawiane przez aplikację przez wywołanie **setRequestHeader** w obiekcie **XMLHttpRequest** . (Specyfikacja CORS wywołuje te "nagłówki żądania autorów"). Reguła nie ma zastosowania do nagłówków, które można ustawić w *przeglądarce* , takich jak User-Agent, Host lub Content-Length.

Oto przykład żądania wstępnego:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Żądanie przed inspekcją używa metody HTTP OPTIONS. Zawiera dwa specjalne nagłówki:

- Access-Control-Request-Method: metoda HTTP, która będzie używana dla rzeczywistego żądania.
- Access-Control-Request-Heads: lista nagłówków żądań, które *aplikacja* ustawi w rzeczywistym żądaniu. (Ponownie nie obejmuje to nagłówków, które są ustawiane w przeglądarce).

Oto przykład odpowiedzi, przy założeniu, że serwer zezwala na żądanie:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Odpowiedź zawiera nagłówek Access-Control-Allow-Methods, który zawiera listę dozwolonych metod i opcjonalnie nagłówek Access-Control-Allow-Heads, który zawiera listę dozwolonych nagłówków. W przypadku pomyślnego przeprowadzenia żądania wstępnego przeglądarka wyśle rzeczywiste żądanie zgodnie z wcześniejszym opisem.

Narzędzia często używane do testowania punktów końcowych z żądaniami opcji wstępnego lotu (na przykład [programu Fiddler](https://www.telerik.com/fiddler) i [Poster](https://www.getpostman.com/)) nie domyślnie wysyłają wymaganych nagłówków opcji. Upewnij się, że nagłówki `Access-Control-Request-Method` i `Access-Control-Request-Headers` są wysyłane wraz z żądaniem, a nagłówki opcji docierają do aplikacji za pomocą usług IIS.

Aby skonfigurować usługi IIS do zezwalania aplikacji ASP.NET na odbieranie i obsługę żądań opcji, Dodaj następującą konfigurację do pliku *Web. config* aplikacji w sekcji `<system.webServer><handlers>`:

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

Usunięcie `OPTIONSVerbHandler` uniemożliwia Obsługiwanie żądań opcji przez usługi IIS. Zastąpienie `ExtensionlessUrlHandler-Integrated-4.0` zezwala na dostęp do aplikacji, ponieważ domyślna Rejestracja modułu zezwala na żądania GET, głowy, POST i DEBUG z adresami URL bez rozszerzenia.

## <a name="scope-rules-for-enablecors"></a>Reguły zakresu dla [EnableCors]

Można włączyć funkcję CORS na akcję, na kontroler lub globalnie dla wszystkich kontrolerów internetowego interfejsu API w aplikacji.

**Na akcję**

Aby włączyć mechanizm CORS dla jednej akcji, należy ustawić atrybut **[EnableCors]** dla metody Action. Poniższy przykład umożliwia włączenie mechanizmu CORS tylko dla metody `GetItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Na kontroler**

Jeśli ustawisz **[EnableCors]** w klasie kontrolera, ma ona zastosowanie do wszystkich akcji na kontrolerze. Aby wyłączyć mechanizm CORS dla akcji, Dodaj atrybut **[DisableCors]** do akcji. Poniższy przykład włącza funkcję CORS dla każdej metody z wyjątkiem `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalnie**

Aby włączyć mechanizm CORS dla wszystkich kontrolerów interfejsu API sieci Web w aplikacji, należy przekazać wystąpienie **EnableCorsAttribute** do metody **EnableCors** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Jeśli ustawisz atrybut w więcej niż jednym zakresie, kolejność pierwszeństwa to:

1. Akcja
2. Kontroler
3. Globalny

## <a name="set-the-allowed-origins"></a>Ustaw dozwolone źródła

Parametr *Origins* atrybutu **[EnableCors]** określa, które źródła mogą uzyskać dostęp do zasobu. Wartość jest rozdzielaną przecinkami listą dozwolonych źródeł.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Można również użyć symbolu wieloznacznego "\*", aby zezwolić na żądania z dowolnego źródła.

Należy uważnie rozważyć przed zezwoleniem na żądania z dowolnego źródła. Oznacza to, że dosłownie każda witryna sieci Web może wykonywać wywołania AJAX do internetowego interfejsu API.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>Ustaw dozwolone metody HTTP

Parametr *Methods* atrybutu **[EnableCors]** określa, które metody http mogą uzyskać dostęp do zasobu. Aby zezwolić na wszystkie metody, użyj wartości wieloznacznej "\*". Poniższy przykład umożliwia tylko żądania GET i POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>Ustaw nagłówki dozwolonych żądań

W tym artykule opisano wcześniej, jak żądanie wstępnego lotu może zawierać nagłówek Access-Control-Request-Heads, zawierający listę nagłówków HTTP ustawionych przez aplikację (nazywanych dalej "nagłówkami żądań autorów"). Parametr *Heads* atrybutu **[EnableCors]** określa, które nagłówki żądań autora są dozwolone. Aby zezwolić na nagłówki, ustaw *nagłówki* na "\*". Aby dozwolonych określone nagłówki, ustaw *nagłówki* na listę dozwolonych nagłówków rozdzielanych przecinkami:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Jednak przeglądarki nie są w pełni spójne w sposobie ustawiania dostępu-nagłówka-żądania-nagłówków. Na przykład program Chrome obecnie zawiera "Źródło". FireFox nie zawiera standardowych nagłówków, takich jak "Akceptuj", nawet gdy aplikacja ustawia je w skrypcie.

Jeśli ustawisz *nagłówki* na inne niż "\*", należy uwzględnić co najmniej "Akceptuj", "Content-Type" i "Origin" oraz wszystkie niestandardowe nagłówki, które mają być obsługiwane.

## <a name="set-the-allowed-response-headers"></a>Ustaw nagłówki dozwolonych odpowiedzi

Domyślnie przeglądarka nie uwidacznia wszystkich nagłówków odpowiedzi dla aplikacji. Domyślnie dostępne są nagłówki odpowiedzi:

- Cache-Control
- Content-Language
- Content-Type
- Wygasł
- Last-Modified
- Pragm

Specyfikacja CORS wywołuje te [proste nagłówki odpowiedzi](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Aby udostępnić inne nagłówki dla aplikacji, ustaw parametr *exposedHeaders* **[EnableCors]** .

W poniższym przykładzie metoda `Get` kontrolera ustawia niestandardowy nagłówek o nazwie "X-Custom-Header". Domyślnie przeglądarka nie ujawnia tego nagłówka w żądaniu między źródłami. Aby uzyskać dostęp do nagłówka, Dołącz symbol "X-Custom-Header" w *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Przekaż poświadczenia w żądaniach między źródłami

Poświadczenia wymagają specjalnej obsługi w żądaniu CORS. Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem między źródłami. Poświadczenia obejmują pliki cookie oraz schematy uwierzytelniania HTTP. Aby wysłać poświadczenia z żądaniem między źródłami, klient musi ustawić atrybut **XMLHttpRequest. withCredentials** na wartość true.

Bezpośrednie używanie elementu **XMLHttpRequest** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

W platformie jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Ponadto serwer musi zezwalać na poświadczenia. Aby zezwolić na poświadczenia między źródłami w interfejsie API sieci Web, ustaw właściwość **SupportsCredentials** na wartość true w atrybucie **[EnableCors]** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Jeśli ta właściwość ma wartość true, odpowiedź HTTP będzie zawierać nagłówek Access-Control-Allow-Credentials. Ten nagłówek nakazuje przeglądarce, że serwer zezwala na poświadczenia dla żądania między źródłami.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka Access-Control-Allow-Credentials, przeglądarka nie udostępni odpowiedzi aplikacji, a żądanie AJAX zakończy się niepowodzeniem.

Należy zachować ostrożność w przypadku ustawienia **SupportsCredentials** na true, ponieważ oznacza to, że witryna sieci Web w innej domenie może wysyłać poświadczenia zalogowanego użytkownika do internetowego interfejsu API w imieniu użytkownika, bez wiedzy użytkownika. Specyfikacja CORS określa również, że *źródła* ustawień &quot;\*&quot; jest nieprawidłowe, jeśli **SupportsCredentials** ma wartość true.

## <a name="custom-cors-policy-providers"></a>Niestandardowi dostawcy zasad CORS

Atrybut **[EnableCors]** implementuje interfejs **ICorsPolicyProvider** . Aby zapewnić własną implementację, można utworzyć klasę, która pochodzi od **atrybutu** i implementuje **ICorsPolicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Teraz można zastosować atrybut, który można umieścić w dowolnym miejscu **[EnableCors]** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Na przykład niestandardowy dostawca zasad CORS może odczytać ustawienia z pliku konfiguracyjnego.

Zamiast używać atrybutów, można zarejestrować obiekt **ICorsPolicyProviderFactory** , który tworzy obiekty **ICorsPolicyProvider** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Aby ustawić **ICorsPolicyProviderFactory**, wywołaj metodę rozszerzenia **SetCorsPolicyProviderFactory** przy uruchamianiu w następujący sposób:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Obsługa przeglądarki

Pakiet CORS interfejsu API sieci Web jest technologią po stronie serwera. Przeglądarka użytkownika wymaga również obsługi mechanizmu CORS. Na szczęście bieżące wersje wszystkich głównych przeglądarek obejmują [obsługę mechanizmu CORS](http://caniuse.com/cors).
