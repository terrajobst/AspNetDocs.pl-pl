---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Przewodnik po interfejsie API centrów 1. x — klient JavaScript | Microsoft Docs
author: bradygaster
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 1,1 w klientach JavaScript, takich jak przeglądarki i program Windows Store (WinJS)...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 24850fe5229490bf600e09ad4718abb575a845fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536443"
---
# <a name="signalr-1x-hubs-api-guide---javascript-client"></a>Podręcznik interfejsu API centrów usługi SignalR 1.x — klient JavaScript

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 1,1 w klientach JavaScript, takich jak przeglądarki i aplikacje ze sklepu Windows (WinJS).
> 
> Interfejs API centrów sygnałów umożliwia wykonywanie zdalnych wywołań procedur (RPC) z serwera do podłączonych klientów i od klientów do serwera programu. W polu kod serwera można zdefiniować metody, które mogą być wywoływane przez klientów, i wywoływanie metod uruchamianych na kliencie. W kodzie klienta należy zdefiniować metody, które mogą być wywoływane z serwera programu, i wywoływanie metod, które są uruchamiane na serwerze. Sygnalizujący, że wszystkie instalacje z klientem do serwera są obsługiwane.
> 
> Sygnalizujący oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych. Aby zapoznać się z wprowadzeniem do sygnałów, centrów i połączeń trwałych lub samouczka, w którym pokazano, jak utworzyć kompletną aplikację sygnalizującą, zobacz [sygnalizującer-wprowadzenie](../getting-started/index.md).

## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Wygenerowany serwer proxy i jego działanie](#genproxy)

    - [Kiedy używać wygenerowanego serwera proxy](#cantusegenproxy)
- [Konfiguracja klienta](#clientsetup)

    - [Jak odwołać się do dynamicznie generowanego serwera proxy](#dynamicproxy)
    - [Jak utworzyć plik fizyczny dla wygenerowanego przez sygnał serwera proxy](#manualproxy)
- [Jak nawiązać połączenie](#establishconnection)

    - [$. Connection. Hub jest tym samym obiektem, który $. hubConnection () tworzy](#connequivalence)
    - [Asynchroniczne wykonywanie metody Start](#asyncstart)
- [Jak nawiązać połączenie między domenami](#crossdomain)
- [Jak skonfigurować połączenie](#configureconnection)

    - [Jak określić parametry ciągu zapytania](#querystring)
    - [Jak określić metodę transportu](#transport)
- [Jak uzyskać serwer proxy dla klasy centrum](#getproxy)
- [Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer](#callclient)
- [Jak wywoływać metody serwera z klienta](#callserver)
- [Jak obsługiwać zdarzenia okresu istnienia połączenia](#connectionlifetime)
- [Jak obsłużyć błędy](#handleerrors)
- [Jak włączyć rejestrowanie po stronie klienta](#logging)

Aby uzyskać dokumentację dotyczącą sposobu programowania serwera lub klientów platformy .NET, zobacz następujące zasoby:

- [Przewodnik interfejsu API centrów sygnałów — serwer](../guide-to-the-api/hubs-api-guide-server.md)
- [Przewodnik interfejsu API centrów sygnałów — klient platformy .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

Łącza do tematów dotyczących odwołań do interfejsów API znajdują się w wersji programu .NET 4,5 interfejsu API. Jeśli używasz programu .NET 4, zobacz [temat wersja interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Wygenerowany serwer proxy i jego działanie

Możesz programować klienta JavaScript, aby komunikować się z usługą sygnalizującą z serwerem proxy lub bez niego. Co to jest serwer proxy upraszcza składnię kodu używanego do nawiązywania połączeń, metody zapisu, które serwer wywołuje, i wywołania metod na serwerze.

Podczas pisania kodu do wywoływania metod serwera wygenerowany serwer proxy umożliwia użycie składni, która wygląda tak, jakby była wykonywana funkcja lokalna: można napisać `serverMethod(arg1, arg2)` zamiast `invoke('serverMethod', arg1, arg2)`. Wygenerowana składnia serwera proxy umożliwia również natychmiastowe i zrozumiały błąd po stronie klienta, jeśli wpiszesz nazwę metody serwera. A jeśli ręcznie utworzysz plik, który definiuje serwery proxy, możesz również uzyskać obsługę technologii IntelliSense do pisania kodu, który wywołuje metody serwera.

Załóżmy na przykład, że na serwerze znajduje się następująca Klasa centrum:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

W poniższych przykładach kodu przedstawiono kod JavaScript, który wygląda na sposób wywoływania metody `NewContosoChatMessage` na serwerze i odbieranie wywołań metody `addContosoChatMessageToPage` z serwera.

**Z wygenerowanym serwerem proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Bez wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Kiedy używać wygenerowanego serwera proxy

Jeśli chcesz zarejestrować wiele obsługi zdarzeń dla metody klienta, którą wywoła serwer, nie możesz użyć wygenerowanego serwera proxy. W przeciwnym razie można wybrać użycie wygenerowanego serwera proxy lub nie na podstawie preferencji kodowania. Jeśli zdecydujesz się nie używać, nie musisz odwoływać się do adresu URL "sygnalizującer/Hub" w `script` w kodzie klienta.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Konfiguracja klienta

Klient JavaScript wymaga odwołań do jQuery i podstawowego pliku języka JavaScript. Wersja jQuery musi być 1.6.4 lub większością nowszych wersji, takich jak 1.7.2, 1.8.2 lub 1.9.1. Jeśli zdecydujesz się użyć wygenerowanego serwera proxy, musisz również odwołać się do pliku JavaScript wygenerowanego przez Sygnałer. Poniższy przykład pokazuje, jakie odwołania mogą wyglądać na stronie HTML używającej wygenerowanego serwera proxy.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Te odwołania muszą być zawarte w tej kolejności: najpierw jQuery, rdzeń sygnalizujący po tym, a serwery proxy sygnalizujące ostatnie.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Jak odwołać się do dynamicznie generowanego serwera proxy

W poprzednim przykładzie odwołanie do wygenerowanego przez sygnalizującego serwera proxy ma dynamicznie generowany kod JavaScript, a nie plik fizyczny. Program sygnalizujący tworzy kod JavaScript dla serwera proxy na bieżąco i służy do obsługi klienta w odpowiedzi na adres URL "/SignalR/Hubs". Jeśli określono inny podstawowy adres URL dla połączeń sygnalizujących na serwerze w metodzie `MapHubs`, adres URL dla dynamicznie generowanego pliku proxy to niestandardowy adres URL z dołączonym do niego elementem "/Hubs".

> [!NOTE]
> W przypadku klientów JavaScript systemu Windows 8 (sklepu Windows) Użyj fizycznego pliku serwera proxy zamiast dynamicznie generowanego. Aby uzyskać więcej informacji, zobacz [jak utworzyć plik fizyczny dla wygenerowanego przez program sygnalizujący serwer proxy](#manualproxy) w dalszej części tego tematu.

W widoku Razor ASP.NET MVC 4, użyj tyldy, aby odwołać się do katalogu głównego aplikacji w dokumentacji pliku proxy:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Aby uzyskać więcej informacji o korzystaniu z programu sygnalizującego w MVC 4, zobacz [wprowadzenie with signaler and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

W widoku Razor ASP.NET MVC 3 Użyj `Url.Content` do odwołania do pliku serwera proxy:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

W aplikacji ASP.NET Web Forms Użyj `ResolveClientUrl` do odwołania do pliku proxy lub Zarejestruj ją za pośrednictwem ScriptManager przy użyciu ścieżki względnej głównej aplikacji (rozpoczynając od tyldy):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Ogólnie rzecz biorąc, Użyj tej samej metody, aby określić adres URL "/SignalR/Hubs", który jest używany dla plików CSS lub JavaScript. Jeśli określisz adres URL bez korzystania z tyldy, w niektórych scenariuszach aplikacja będzie działać poprawnie podczas testowania w programie Visual Studio przy użyciu IIS Express, ale wystąpi błąd 404 podczas wdrażania do pełnych usług IIS. Aby uzyskać więcej informacji, zobacz **rozpoznawanie odwołań do zasobów na poziomie głównym** na [serwerach sieci Web w programie Visual Studio for ASP.NET — projekty sieci Web](https://msdn.microsoft.com/library/58wxa9w5.aspx) w witrynie MSDN.

Po uruchomieniu projektu sieci Web w programie Visual Studio 2012 w trybie debugowania, a jeśli używasz programu Internet Explorer jako przeglądarki, plik proxy można zobaczyć w **Eksplorator rozwiązań** w obszarze **dokumenty skryptu**, jak pokazano na poniższej ilustracji.

![Plik proxy wygenerowany w języku JavaScript w Eksplorator rozwiązań](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Aby wyświetlić zawartość pliku, kliknij dwukrotnie pozycję **centra**. Jeśli nie korzystasz z programu Visual Studio 2012 i przeglądarki Internet Explorer lub jeśli nie jesteś w trybie debugowania, możesz również pobrać zawartość pliku, przechodząc do adresu URL "/signalR/hubs". Na przykład jeśli witryna jest uruchomiona w `http://localhost:56699`, przejdź do `http://localhost:56699/SignalR/hubs` w przeglądarce.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Jak utworzyć plik fizyczny dla wygenerowanego przez sygnał serwera proxy

Jako alternatywę dla dynamicznie generowanego serwera proxy można utworzyć plik fizyczny z kodem serwera proxy i odwołać się do tego pliku. Można to zrobić na potrzeby kontroli nad zachowaniem buforowania lub tworzenia lub w celu uzyskania IntelliSense podczas kodowania wywołań do metod serwera.

Aby utworzyć plik proxy, wykonaj następujące czynności:

1. Zainstaluj pakiet NuGet [Microsoft. ASPNET. signaler.](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .
2. Otwórz wiersz polecenia i przejdź do folderu *Tools* zawierającego plik sygnalizującer. exe. Folder Tools znajduje się w następującej lokalizacji:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Wprowadź następujące polecenie:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Ścieżka do *biblioteki DLL* jest zazwyczaj folderem *bin* w folderze projektu.

    To polecenie tworzy plik o nazwie *Server. js* w tym samym folderze co *sygnalizującer. exe*.
4. Umieść plik *Server. js* w odpowiednim folderze w projekcie, zmień jego nazwę na odpowiedni dla aplikacji i Dodaj odwołanie do niego zamiast odwołania "sygnalizującer/Hub".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Jak nawiązać połączenie

Przed nawiązaniem połączenia należy utworzyć obiekt połączenia, utworzyć serwer proxy i zarejestrować obsługę zdarzeń dla metod, które mogą być wywoływane z serwera. Po skonfigurowaniu programu obsługi serwera proxy i zdarzeń Ustanów połączenie, wywołując metodę `start`.

Jeśli używasz wygenerowanego serwera proxy, nie musisz tworzyć obiektu połączenia w własnym kodzie, ponieważ wygenerowany kod serwera proxy robi to za Ciebie.

<a id="nogenconnection"></a>

**Nawiązywanie połączenia (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Nawiązywanie połączenia (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Przykładowy kod używa domyślnego adresu URL "/SignalR", aby nawiązać połączenie z usługą sygnalizującej. Aby uzyskać informacje na temat sposobu określania innego podstawowego adresu URL, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Zwykle należy zarejestrować procedury obsługi zdarzeń przed wywołaniem metody `start` w celu nawiązania połączenia. Jeśli chcesz zarejestrować niektóre programy obsługi zdarzeń po ustanowieniu połączenia, możesz to zrobić, ale przed wywołaniem metody `start` należy zarejestrować co najmniej jeden z obsługi zdarzeń. Jedną z przyczyn tego problemu jest to, że w aplikacji może być wiele centrów, ale nie chcesz wyzwalać zdarzenia `OnConnected` w każdym centrum, jeśli zamierzasz korzystać tylko z jednego z nich. Gdy połączenie zostanie nawiązane, obecność metody klienta na serwerze proxy centrum wskazuje, co informuje sygnalizujący, aby wyzwolić zdarzenie `OnConnected`. Jeśli nie zarejestrowano żadnych programów obsługi zdarzeń przed wywołaniem metody `start`, będzie można wywołać metody w centrum, ale metoda `OnConnected` centrum nie zostanie wywołana i nie będą wywoływane metody klienta z serwera.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. Connection. Hub jest tym samym obiektem, który $. hubConnection () tworzy

Jak widać w przykładach, gdy używasz wygenerowanego serwera proxy, `$.connection.hub` odwołuje się do obiektu połączenia. Jest to ten sam obiekt, który uzyskuje się, wywołując `$.hubConnection()`, gdy nie korzystasz z wygenerowanego serwera proxy. Wygenerowany kod serwera proxy tworzy połączenie przez wykonanie następującej instrukcji:

![Tworzenie połączenia w wygenerowanym pliku serwera proxy](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

W przypadku korzystania z wygenerowanego serwera proxy można wykonać dowolne czynności `$.connection.hub`, które można wykonać przy użyciu obiektu połączenia, gdy nie używasz wygenerowanego serwera proxy.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Asynchroniczne wykonywanie metody Start

Metoda `start` jest wykonywana asynchronicznie. Zwraca [odroczony obiekt jQuery](http://api.jquery.com/category/deferred-object/), co oznacza, że można dodawać funkcje wywołania zwrotnego przez wywoływanie metod, takich jak `pipe`, `done`i `fail`. Jeśli masz kod, który chcesz wykonać po nawiązaniu połączenia, na przykład wywołanie metody serwera, Umieść ten kod w funkcji wywołania zwrotnego lub wywołaj ją z funkcji wywołania zwrotnego. Metoda wywołania zwrotnego `.done` jest wykonywana po nawiązaniu połączenia i po każdym kodzie, który znajduje się w metodzie obsługi zdarzeń `OnConnected` na serwerze kończy wykonywanie.

Jeśli umieścisz instrukcję "teraz połączony" z poprzedniego przykładu jako następny wiersz kodu po wywołaniu metody `start` (nie w `.done` wywołaniu zwrotnym), wiersz `console.log` zostanie wykonany przed nawiązaniem połączenia, jak pokazano w następującym przykładzie:

![Niewłaściwy sposób pisania kodu, który jest uruchamiany po ustanowieniu połączenia](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Jak nawiązać połączenie między domenami

Zazwyczaj Jeśli przeglądarka ładuje stronę `http://contoso.com`, połączenie sygnalizujące znajduje się w tej samej domenie, w `http://contoso.com/signalr`. Jeśli strona z `http://contoso.com` nawiązać połączenie z `http://fabrikam.com/signalr`, to jest połączenie między domenami. Ze względów bezpieczeństwa połączenia między domenami są domyślnie wyłączone. Aby ustanowić połączenie między domenami, upewnij się, że na serwerze są włączone połączenia między domenami, a następnie określ adres URL połączenia podczas tworzenia obiektu połączenia. Sygnalizujący będzie używać odpowiedniej technologii dla połączeń międzydomenowych, takich jak [JSONP](http://en.wikipedia.org/wiki/JSONP) lub [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Na serwerze Włącz połączenia między domenami, wybierając tę opcję po wywołaniu metody `MapHubs`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

Na kliencie Określ adres URL podczas tworzenia obiektu połączenia (bez wygenerowanego serwera proxy) lub przed wywołaniem metody Start (z wygenerowanym serwerem proxy).

**Kod klienta określający połączenie między domenami (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Kod klienta określający połączenie między domenami (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

W przypadku korzystania z konstruktora `$.hubConnection` nie trzeba uwzględniać `signalr` w adresie URL, ponieważ jest on automatycznie dodawany (chyba że określisz `useDefaultUrl` jako `false`).

Można utworzyć wiele połączeń z różnymi punktami końcowymi.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Nie ustawiaj `jQuery.support.cors` na true w kodzie.
> 
>     ![Nie ustawiaj jQuery. support. CORS na true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     Sygnał obsługuje użycie JSONP lub CORS. Ustawienie wartości true dla `jQuery.support.cors` powoduje wyłączenie funkcji JSONP, ponieważ powoduje to założenie, że przeglądarka obsługuje mechanizm CORS.
> - W przypadku nawiązywania połączenia z adresem URL hosta lokalnego program Internet Explorer 10 nie będzie uwzględniać połączenia między domenami, dzięki czemu aplikacja będzie działała lokalnie z programem IE 10, nawet jeśli na serwerze nie włączono połączeń między domenami.
> - Aby uzyskać informacje o korzystaniu z połączeń między domenami w programie Internet Explorer 9, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Aby uzyskać informacje o korzystaniu z połączeń między domenami z programem Chrome, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Przykładowy kod używa domyślnego adresu URL "/SignalR", aby nawiązać połączenie z usługą sygnalizującej. Aby uzyskać informacje na temat sposobu określania innego podstawowego adresu URL, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Jak skonfigurować połączenie

Przed nawiązaniem połączenia można określić parametry ciągu zapytania lub określić metodę transportu.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Jak określić parametry ciągu zapytania

Jeśli chcesz wysłać dane do serwera podczas łączenia się z klientem, możesz dodać parametry ciągu zapytania do obiektu połączenia. W poniższych przykładach pokazano, jak ustawić parametr ciągu zapytania w kodzie klienta.

**Ustaw wartość ciągu zapytania przed wywołaniem metody Start (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Ustaw wartość ciągu zapytania przed wywołaniem metody Start (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

Poniższy przykład pokazuje, jak odczytać parametr ciągu zapytania w kodzie serwera.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Jak określić metodę transportu

W ramach procesu łączenia klient sygnalizujący zwykle negocjuje z serwerem, aby określić najlepszy transport obsługiwany przez serwer i klienta. Jeśli wiesz już, którego transportu chcesz użyć, możesz pominąć ten proces negocjacji, określając metodę transportu, gdy wywołasz metodę `start`.

**Kod klienta określający metodę transportu (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Kod klienta określający metodę transportu (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Alternatywnie można określić wiele metod transportu w kolejności, w której ma być wykorzystana przez program sygnalizujący:

**Kod klienta określający niestandardowy schemat powrotu do transportu (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Kod klienta określający niestandardowy schemat powrotu do transportu (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Aby określić metodę transportu, można użyć następujących wartości:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

W poniższych przykładach pokazano, jak ustalić, która metoda transportu jest używana przez połączenie.

**Kod klienta, który wyświetla metodę transportu używaną przez połączenie (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Kod klienta, który wyświetla metodę transportu używaną przez połączenie (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Aby uzyskać informacje na temat sprawdzania metody transportu w kodzie serwera, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server — jak uzyskać informacje o kliencie z właściwości Context](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Aby uzyskać więcej informacji na temat transportów i rezerw, zobacz [wprowadzenie do sygnałów-Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Jak uzyskać serwer proxy dla klasy centrum

Każdy utworzony obiekt połączenia hermetyzuje informacje o połączeniu z usługą sygnalizującą, która zawiera jedną lub więcej klas centrów. Aby komunikować się z klasą centrum, należy użyć obiektu serwera proxy, który utworzysz samodzielnie (jeśli nie używasz wygenerowanego serwera proxy) lub który jest generowany dla Ciebie.

Na kliencie nazwa serwera proxy to notacji camelcasea wersja klasy centrum. Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.

**Klasa centrum na serwerze**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Pobierz odwołanie do wygenerowanego serwera proxy klienta dla centrum**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Utwórz serwer proxy klienta dla klasy centrum (bez wygenerowanego serwera proxy)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Jeśli dekorować klasę centrów z atrybutem `HubName`, użyj dokładnej nazwy bez zmieniania wielkości liter.

**Klasa Hub na serwerze z atrybutem HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Pobierz odwołanie do wygenerowanego serwera proxy klienta dla centrum**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Utwórz serwer proxy klienta dla klasy centrum (bez wygenerowanego serwera proxy)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer

Aby zdefiniować metodę, którą serwer może wywoływać z centrum, Dodaj procedurę obsługi zdarzeń do serwera proxy centrum przy użyciu właściwości `client` wygenerowanego serwera proxy lub wywołaj metodę `on`, jeśli nie używasz wygenerowanego serwera proxy. Parametry mogą być obiektami złożonymi.

Dodaj program obsługi zdarzeń przed wywołaniem metody `start` w celu nawiązania połączenia. (Jeśli chcesz dodać programy obsługi zdarzeń po wywołaniu metody `start`, zapoznaj się z uwagami dotyczącymi [sposobu nawiązywania połączenia](#establishconnection) we wcześniejszej części tego dokumentu i użyj składni pokazanej w celu zdefiniowania metody bez użycia wygenerowanego serwera proxy).

W dopasowaniu nazw metod nie jest rozróżniana wielkość liter. Na przykład `Clients.All.addContosoChatMessageToPage` na serwerze wykona `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`lub `addcontosochatmessagetopage` na kliencie.

**Zdefiniuj metodę na kliencie (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Alternatywny sposób definiowania metody na kliencie (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Zdefiniuj metodę na kliencie (bez wygenerowanego serwera proxy lub podczas dodawania po wywołaniu metody startowej)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Kod serwera, który wywołuje metodę klienta**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Następujące przykłady zawierają obiekt złożony jako parametr metody.

**Zdefiniuj metodę na kliencie, która pobiera obiekt złożony (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Zdefiniuj metodę na kliencie, która pobiera obiekt złożony (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Kod serwera, który definiuje obiekt złożony**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Jak wywoływać metody serwera z klienta

Aby wywołać metodę serwera z poziomu klienta, należy użyć właściwości `server` wygenerowanego serwera proxy lub metody `invoke` na serwerze proxy centrum, jeśli nie używasz wygenerowanego serwera proxy. Wartość zwracana lub parametry mogą być obiektami złożonymi.

Przekaż notacji camelcaseą nazwę metody w centrum. Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.

W poniższych przykładach pokazano, jak wywołać metodę serwera, która nie ma wartości zwracanej i jak wywołać metodę serwera, która ma wartość zwracaną.

**Metoda serwera bez atrybutu HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Kod serwera, który definiuje złożony obiekt przekazaną w parametrze**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Jeśli zawarto metodę Hub z atrybutem `HubMethodName`, Użyj tej nazwy bez zmieniania wielkości liter.

**Metoda serwera** z atrybutem HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

W powyższych przykładach pokazano, jak wywołać metodę serwera, która nie zwraca wartości. W poniższych przykładach pokazano, jak wywołać metodę serwera, która ma wartość zwracaną.

**Kod serwera dla metody, która ma wartość zwracaną**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**Klasa magazynowa używana dla** wartości zwracanej

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Jak obsługiwać zdarzenia okresu istnienia połączenia

Program sygnalizujący udostępnia następujące zdarzenia okresu istnienia połączenia, które można obsłużyć:

- `starting`: uruchamiany przed wysłaniem danych przez połączenie.
- `received`: wywoływane po odebraniu dowolnego danych w połączeniu. Dostarcza odebrane dane.
- `connectionSlow`: wywoływane, gdy klient wykrywa powolne lub często porzucane połączenie.
- `reconnecting`: uruchamiany, gdy podstawowy transport zacznie ponownie nawiązać połączenie.
- `reconnected`: wywoływane, gdy podstawowy transport został połączony ponownie.
- `stateChanged`: występuje, gdy zmieni się stan połączenia. Zapewnia stary stan i nowy stan (połączenie, połączenie, ponowne połączenie lub odłączenie).
- `disconnected`: wywoływane, gdy połączenie zostało rozłączone.

Na przykład jeśli chcesz wyświetlać komunikaty ostrzegawcze w przypadku problemów z połączeniem, które mogą powodować zauważalne opóźnienia, obsłuż zdarzenie `connectionSlow`.

**Obsługuj zdarzenie connectionSlow (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Obsługuj zdarzenie connectionSlow (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia w programie sygnalizującym](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Jak obsłużyć błędy

Klient JavaScript sygnalizujący udostępnia zdarzenie `error`, do którego można dodać program obsługi dla. Można również użyć metody niepowodzenia, aby dodać procedurę obsługi dla błędów wynikających z wywołania metody serwera.

Jeśli nie włączysz jawnie szczegółowych komunikatów o błędach na serwerze, obiekt wyjątku zwracany przez sygnalizujący po wystąpieniu błędu zawiera minimalne informacje o błędzie. Na przykład jeśli wywołanie `newContosoChatMessage` nie powiedzie się, komunikat o błędzie w obiekcie Error zawiera "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" wysyłanie szczegółowych komunikatów o błędach do klientów w środowisku produkcyjnym nie jest zalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach w celu rozwiązywania problemów, użyj następującego kodu na serwerze.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

Poniższy przykład pokazuje, jak dodać procedurę obsługi dla zdarzenia błędu.

**Dodaj procedurę obsługi błędów (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Dodaj program obsługi błędów (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Poniższy przykład pokazuje, jak obsłużyć błąd z wywołania metody.

**Obsługa błędu z wywołania metody (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Obsługa błędu z wywołania metody (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Jeśli wywołanie metody nie powiedzie się, zostanie również zgłoszone zdarzenie `error`, więc kod w obsłudze metody `error` i w wywołaniu metody `.fail` może zostać wykonany.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Jak włączyć rejestrowanie po stronie klienta

Aby włączyć rejestrowanie po stronie klienta dla połączenia, należy ustawić właściwość `logging` obiektu połączenia przed wywołaniem metody `start` w celu nawiązania połączenia.

**Włącz rejestrowanie (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Włącz rejestrowanie (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Aby wyświetlić dzienniki, Otwórz narzędzia deweloperskie w przeglądarce i przejdź do karty konsola. Samouczek przedstawiający instrukcje krok po kroku i zrzuty ekranu, które pokazują, jak to zrobić, można znaleźć [w temacie emisja serwera za pomocą sygnału ASP.NET — włączenie rejestrowania](index.md).
