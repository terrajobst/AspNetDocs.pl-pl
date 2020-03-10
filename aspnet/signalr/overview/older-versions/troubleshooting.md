---
uid: signalr/overview/older-versions/troubleshooting
title: Rozwiązywanie problemów z sygnałem (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: W tym artykule opisano typowe problemy związane z programowaniem aplikacji sygnalizujących.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 3dbf091ac2daa4da477b405852bb4d1316584fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579605"
---
# <a name="signalr-troubleshooting-signalr-1x"></a>Rozwiązywanie problemów z usługą SignalR (SignalR 1.x)

[Fletcher Patryka](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym dokumencie opisano typowe problemy związane z rozwiązywaniem problemów z usługą sygnalizujące.

Ten dokument zawiera następujące sekcje.

- [Wywoływanie metod w trybie dyskretnym klienta i serwera nie powiodło się](#connection)
- [Inne problemy z połączeniem](#other)
- [Kompilacja i błędy po stronie serwera](#server)
- [Problemy z programem Visual Studio](#vs)
- [Problemy Internet Information Services](#iis)
- [Problemy z platformą Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Wywoływanie metod w trybie dyskretnym klienta i serwera nie powiodło się

W tej sekcji opisano możliwe przyczyny niepowodzenia wywołania metody między klientem a serwerem bez znaczącego komunikatu o błędzie. W aplikacji sygnalizującej serwer nie ma informacji o metodach, które implementuje klient; gdy serwer wywołuje metodę klienta, dane o nazwie i parametrze metody są wysyłane do klienta, a metoda jest wykonywana tylko wtedy, gdy istnieje w formacie określonym przez serwer. Jeśli na kliencie nie zostanie znaleziona zgodna Metoda, nic się nie dzieje i na serwerze nie zostanie zgłoszony żaden komunikat o błędzie.

Aby dokładniej zbadać metody klienta, które nie są wywoływane, można włączyć rejestrowanie przed wywołaniem metody Start w centrum, aby zobaczyć, jakie wywołania pochodzą z serwera. Aby włączyć rejestrowanie w aplikacji JavaScript, zobacz [jak włączyć rejestrowanie po stronie klienta (wersja klienta JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Aby włączyć rejestrowanie w aplikacji klienckiej platformy .NET, zobacz [jak włączyć rejestrowanie po stronie klienta (wersja klienta platformy .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Błędnie wpisana Metoda, niepoprawna sygnatura metody lub nieprawidłowa nazwa centrum

Jeśli nazwa lub podpis wywoływanej metody nie dokładnie pasuje do odpowiedniej metody na kliencie, wywołanie zakończy się niepowodzeniem. Sprawdź, czy nazwa metody wywołana przez serwer jest zgodna z nazwą metody na kliencie. Ponadto program sygnalizujący tworzy serwer proxy centrum przy użyciu notacji CamelCase metod, zgodnie z wymaganiami w języku JavaScript, dlatego metoda o nazwie `SendMessage` na serwerze powinna być wywoływana `sendMessage` z serwera proxy klienta. Jeśli używasz atrybutu `HubName` w kodzie po stronie serwera, sprawdź, czy nazwa używana jest zgodna z nazwą używaną do utworzenia centrum na kliencie. Jeśli nie korzystasz z atrybutu `HubName`, sprawdź, czy w nazwie centrum w kliencie JavaScript jest notacji CamelCase — wielkość liter, na przykład chatHub zamiast ChatHub.

### <a name="duplicate-method-name-on-client"></a>Zduplikowana nazwa metody na kliencie

Sprawdź, czy nie masz zduplikowanej metody na kliencie, która różni się tylko wielkością liter. Jeśli aplikacja kliencka ma metodę o nazwie `sendMessage`, sprawdź, czy nie ma również metody o nazwie `SendMessage`.

### <a name="missing-json-parser-on-the-client"></a>Brak parsera JSON na kliencie

Sygnalizujący, że istnieje parser JSON do serializacji wywołań między serwerem a klientem. Jeśli klient nie ma wbudowanego analizatora JSON (na przykład programu Internet Explorer 7), należy dołączyć go do aplikacji. Parser JSON można pobrać [tutaj](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Mieszanie gwiazdy i PersistentConnection

Sygnalizujący używa dwóch modeli komunikacji: Hub i PersistentConnections. Składnia służąca do wywoływania tych dwóch modeli komunikacyjnych jest różna w kodzie klienta. Jeśli dodano centrum w kodzie serwera, sprawdź, czy cały kod klienta używa odpowiedniej składni centrum.

**Kod klienta JavaScript, który tworzy PersistentConnection w kliencie JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Kod klienta języka JavaScript, który tworzy serwer proxy centrum w kliencie JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C#kod serwera, który mapuje trasę na PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C#kod serwera, który mapuje trasę do centrum lub wiele centrów, jeśli masz wiele aplikacji**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Połączenie zostało uruchomione przed dodaniem subskrypcji

Jeśli połączenie centrum zostanie uruchomione przed dodaniem do serwera proxy metod, które mogą być wywoływane z serwera, komunikaty nie będą odbierane. Następujący kod JavaScript nie zostanie prawidłowo uruchomiony w centrum:

**Niepoprawny kod klienta języka JavaScript, który nie zezwala na odbieranie komunikatów centrów**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Zamiast tego Dodaj subskrypcje metod przed wywołaniem metody Start:

**Kod klienta JavaScript, który poprawnie dodaje subskrypcje do centrum**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Brak nazwy metody na serwerze proxy centrum

Sprawdź, czy metoda zdefiniowana na serwerze jest subskrybowana na kliencie. Mimo że serwer definiuje metodę, nadal trzeba ją dodać do serwera proxy klienta. Metody można dodać do serwera proxy klienta w następujący sposób (należy zauważyć, że metoda jest dodawana do `client` członkiem centrum, a nie bezpośrednio w centrum):

**Kod klienta JavaScript, który dodaje metody do serwera proxy centrum**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Metody koncentratora lub koncentratora nie zostały zadeklarowane jako publiczne

Aby były widoczne na kliencie, implementacja i metody centrum muszą być zadeklarowane jako `public`.

### <a name="accessing-hub-from-a-different-application"></a>Uzyskiwanie dostępu do centrum z innej aplikacji

Dostęp do centrów sygnałów można uzyskać tylko za poorednictwem aplikacji, które implementują klientów sygnalizujących. Sygnalizujący nie może współdziałać z innymi bibliotekami komunikacyjnymi (takimi jak usługi sieci Web protokołu SOAP lub WCF). Jeśli na platformie docelowej nie ma dostępnego klienta sygnalizującego, nie będzie można uzyskać dostępu bezpośrednio do punktu końcowego serwera.

### <a name="manually-serializing-data"></a>Ręczne Serializowanie danych

Program sygnalizujący automatycznie użyje kodu JSON do serializacji parametrów metody — nie trzeba nic robić.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Metoda zdalnego koncentratora nie została wykonana na kliencie w funkcji ondisconnected

To zachowanie jest celowe. Gdy `OnDisconnected` jest wywoływana, centrum już wprowadziło stan `Disconnected`, co nie pozwala na wywoływanie dalszych metod centrów.

**C#kod serwera, który prawidłowo wykonuje kod w zdarzeniu ondisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Osiągnięto limit połączeń

W przypadku korzystania z pełnej wersji usług IIS w systemie operacyjnym klienta, na przykład w systemie Windows 7, nałożono limit połączeń 10. W przypadku korzystania z systemu operacyjnego klienta należy zamiast tego ograniczyć używać IIS Express.

### <a name="cross-domain-connection-not-set-up-properly"></a>Połączenie między domenami nie zostało prawidłowo skonfigurowane

Jeśli połączenie między domenami (połączenie, dla którego adres URL sygnalizującego nie znajduje się w tej samej domenie, w której znajduje się Strona hostingu) nie jest prawidłowo skonfigurowane, połączenie może zakończyć się niepowodzeniem bez komunikatu o błędzie. Aby uzyskać informacje na temat włączania komunikacji między domenami, zobacz [jak ustanowić połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Połączenie przy użyciu protokołu NTLM (Active Directory) nie działa w kliencie .NET

Połączenie w aplikacji klienckiej platformy .NET, która używa zabezpieczeń domeny, może się nie powieść, jeśli połączenie nie jest prawidłowo skonfigurowane. Aby użyć sygnalizującego w środowisku domeny, ustaw właściwość połączenie wymagane w następujący sposób:

**C#kod klienta implementujący poświadczenia połączenia**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Inne problemy z połączeniem

W tej sekcji opisano przyczyny i rozwiązania określonych objawów lub komunikaty o błędach występujące podczas połączenia.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Błąd "Rozpocznij musi być wywoływana przed wysłaniem danych"

Ten błąd jest często widoczny, jeśli kod odwołuje się do obiektów sygnalizujących przed rozpoczęciem połączenia. Wireup dla programów obsługi i podobne, które będą wywoływały metody zdefiniowane na serwerze, muszą zostać dodane po zakończeniu połączenia. Należy zauważyć, że wywołanie `Start` jest asynchroniczne, dlatego kod po wywołaniu można wykonać przed jego ukończeniem. Najlepszym sposobem dodawania programów obsługi po rozpoczęciu połączenia jest umieszczenie ich w funkcji wywołania zwrotnego, która jest przenoszona jako parametr do metody Start:

**Kod klienta JavaScript, który poprawnie dodaje programy obsługi zdarzeń odwołujące się do obiektów sygnalizujących**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Ten błąd zostanie również wyświetlony, jeśli połączenie zostanie zatrzymane, gdy nadal istnieją odwołania do obiektów sygnalizujących.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>błąd "301 przeniesione trwale" lub "302 przeniesiono tymczasowo"

Ten błąd może wystąpić, jeśli projekt zawiera folder o nazwie sygnalizujący, co będzie zakłócać tworzenie automatycznie utworzonego serwera proxy. Aby uniknąć tego błędu, nie należy używać folderu o nazwie `SignalR` w aplikacji ani wyłączyć automatycznego generowania serwera proxy. Aby uzyskać więcej informacji, zobacz [wygenerowany serwer proxy i jego działanie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) .

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>błąd "403 zabroniony" w kliencie .NET lub Silverlight

Ten błąd może wystąpić w środowiskach międzydomenowych, w których komunikacja między domenami nie jest prawidłowo włączona. Aby uzyskać informacje na temat włączania komunikacji między domenami, zobacz [jak ustanowić połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Aby nawiązać połączenie między domenami w kliencie Silverlight, zobacz [połączenia między domenami klientów Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>błąd "404 nie znaleziono"

Istnieje kilka przyczyn tego problemu. Sprawdź wszystkie z następujących elementów:

- **Odwołanie do adresu serwera proxy centrum nie jest prawidłowo sformatowane:** Ten błąd jest często widoczny, jeśli odwołanie do wygenerowanego adresu serwera proxy centrum nie jest prawidłowo sformatowane. Sprawdź, czy odwołanie do adresu centrum zostało prawidłowo wykonane. Aby uzyskać szczegółowe informacje [, zobacz jak odwoływać się do dynamicznego wygenerowanego serwera proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) .
- **Dodawanie tras do aplikacji przed dodaniem trasy centrum:** Jeśli aplikacja używa innych tras, sprawdź, czy pierwsza dodana trasa jest wywołaniem do `MapHubs`.

### <a name="500-internal-server-error"></a>"błąd wewnętrzny serwera 500"

Jest to bardzo ogólny błąd, który może mieć szeroką gamę przyczyn. Szczegóły błędu powinny pojawić się w dzienniku zdarzeń serwera lub można je znaleźć za pomocą debugowania serwera programu. Bardziej szczegółowe informacje o błędzie można uzyskać, włączając szczegółowe błędy na serwerze. Aby uzyskać więcej informacji, zobacz [jak obsłużyć błędy w klasie centrum](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Błąd "TypeError: &lt;hubType&gt; jest niezdefiniowany"

Ten błąd spowoduje, że wywołanie `MapHubs` nie zostanie prawidłowo wykonane. Aby uzyskać więcej informacji, zobacz [jak zarejestrować trasę sygnałów i skonfigurować opcje sygnałów](../guide-to-the-api/hubs-api-guide-server.md#route) .

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException był nieobsługiwany przez kod użytkownika

Sprawdź, czy parametry wysyłane do metod nie obejmują typów niemożliwych do serializacji (takich jak dojścia do plików lub połączenia z bazami danych). Jeśli musisz użyć elementów członkowskich w obiekcie po stronie serwera, który nie ma być wysyłany do klienta (w przypadku zabezpieczeń lub z powodu serializacji), Użyj atrybutu `JSONIgnore`.

### <a name="protocol-error-unknown-transport-error"></a>"Błąd protokołu: nieznany transport"

Ten błąd może wystąpić, jeśli klient nie obsługuje transportów używanych przez program sygnalizujący. Zobacz [transporty i rezerwy,](../getting-started/introduction-to-signalr.md#transports) Aby uzyskać informacje o tym, które przeglądarki mogą być używane z sygnalizacyjnym.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Generowanie serwera proxy centrum JavaScript zostało wyłączone".

Ten błąd wystąpi, jeśli `DisableJavaScriptProxies` jest ustawiony, wraz z odwołaniem do dynamicznie generowanego serwera proxy w `signalr/hubs`. Aby uzyskać więcej informacji na temat ręcznego tworzenia serwera proxy, zobacz [wygenerowany serwer proxy i jego działanie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"Identyfikator połączenia jest w niepoprawnym formacie" lub "błąd tożsamości użytkownika nie może ulec zmianie podczas połączenia aktywnego sygnału sygnalizacyjnego"

Ten błąd może wystąpić, jeśli jest używane uwierzytelnianie i klient zostanie wylogowany przed zatrzymaniem połączenia. Rozwiązaniem jest zatrzymanie połączenia sygnalizującego przed zarejestrowaniem klienta.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Nieprzechwycony błąd: Sygnalizującer: nie znaleziono jQuery. Upewnij się, że w bibliotece jQuery występuje odwołanie do pliku sygnalizującego. js.

Klient JavaScript sygnalizujący wymaga uruchomienia technologii jQuery. Sprawdź, czy odwołanie do jQuery jest poprawne, czy użyta ścieżka jest prawidłowa i czy odwołanie do jQuery jest przed odwołaniem do sygnalizującego.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Nieprzechwycony TypeError: nie można odczytać właściwości"&lt;Właściwość&gt;"z niezdefiniowanego"

Przyczyną tego błędu jest nieposiadanie jQuery lub serwer proxy centrów, do których odwołuje się prawidłowy. Sprawdź, czy odwołanie do jQuery i serwer proxy centrów są poprawne, czy użyta ścieżka jest prawidłowa i czy odwołanie do jQuery jest przed odwołaniem do serwera proxy centrów. Domyślne odwołanie do serwera proxy centrów powinno wyglądać następująco:

**Kod po stronie klienta HTML, który poprawnie odwołuje się do serwera proxy centrów**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Błąd "RuntimeBinderException został nieobsłużony przez kod użytkownika"

Ten błąd może wystąpić, gdy zostanie użyte nieprawidłowe Przeciążenie `Hub.On`. Jeśli metoda ma wartość zwracaną, typ zwracany musi być określony jako parametr typu generycznego:

**Metoda zdefiniowana na kliencie (bez wygenerowanego serwera proxy)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Identyfikator połączenia jest niespójny lub przerwania połączenia między ładowaniem strony

To zachowanie jest celowe. Ponieważ obiekt centralny jest hostowany w obiekcie Page, koncentrator jest niszczony, gdy strona zostanie odświeżona. Aplikacja wielostronicowa musi zachować skojarzenie między użytkownikami i identyfikatorami połączeń, aby były spójne między obciążeniami stron. Identyfikatory połączeń mogą być przechowywane na serwerze w obiekcie `ConcurrentDictionary` lub w bazie danych.

### <a name="value-cannot-be-null-error"></a>Błąd "wartość nie może być równa null"

Metody po stronie serwera z opcjonalnymi parametrami nie są obecnie obsługiwane; Jeśli opcjonalny parametr zostanie pominięty, metoda zakończy się niepowodzeniem. Aby uzyskać więcej informacji, zobacz [Parametry opcjonalne](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox nie może nawiązać połączenia z serwerem pod adresem &lt;&gt;" błąd w Firebug

Ten komunikat o błędzie można zobaczyć w Firebug, jeśli negocjowanie transportu WebSocket nie powiedzie się, a zamiast tego jest używany inny transport. To zachowanie jest celowe.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>"Ten certyfikat zdalny jest nieprawidłowy zgodnie z procedurą walidacji" błąd w aplikacji klienckiej .NET

Jeśli serwer wymaga niestandardowych certyfikatów klienta, można dodać certyfikat x509 do połączenia przed jego wysłaniem. Dodaj certyfikat do połączenia przy użyciu `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Połączenie jest opuszczane po upływie limitu czasu uwierzytelniania

To zachowanie jest celowe. Poświadczeń uwierzytelniania nie można modyfikować, jeśli połączenie jest aktywne. Aby odświeżyć poświadczenia, należy zatrzymać i ponownie uruchomić połączenie.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>Metody OnConnected są wywoływane dwukrotnie w przypadku korzystania z technologii jQuery Mobile

Funkcja `initializePage` jQuery Mobile Wymusza ponowne wykonanie skryptów na każdej stronie, tworząc drugie połączenie. Rozwiązania tego problemu obejmują:

- Dołącz odwołanie do aplikacji jQuery Mobile przed plikiem JavaScript.
- Wyłącz funkcję `initializePage`, ustawiając `$.mobile.autoInitializePage = false`.
- Poczekaj na zakończenie inicjowania strony przed rozpoczęciem połączenia.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Komunikaty są opóźnione w aplikacjach Silverlight przy użyciu zdarzeń wysłanych przez serwer

Komunikaty są opóźnione w przypadku używania zdarzeń wysyłanych przez serwer w programie Silverlight. Aby wymusić użycie długich sondowań, użyj następującego polecenia podczas uruchamiania połączenia:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"Odmowa uprawnień" przy użyciu protokołu Frame bez ograniczeń

Jest to znany problem opisany [tutaj](https://github.com/SignalR/SignalR/issues/1963). Ten objaw może być widoczny przy użyciu najnowszej biblioteki JQuery; Obejście polega na obniżeniu poziomu aplikacji do JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Kompilacja i błędy po stronie serwera

 W poniższej sekcji znajdują się możliwe rozwiązania błędów kompilatora i środowiska uruchomieniowego po stronie serwera. 

### <a name="reference-to-hub-instance-is-null"></a>Odwołanie do wystąpienia centrum ma wartość null

Ponieważ dla każdego połączenia tworzone jest wystąpienie centrum, nie można samodzielnie utworzyć wystąpienia centrum w kodzie. Aby wywoływać metody z koncentratora spoza samego centrum, zobacz [jak wywoływać metody klienta i zarządzać grupami spoza klasy Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) , aby uzyskać odwołanie do kontekstu centrum.

### <a name="httpcontextcurrentsession-is-null"></a>Właściwość HTTPContext. Current. Session ma wartość null

To zachowanie jest celowe. Usługa sygnalizująca nie obsługuje stanu sesji ASP.NET, ponieważ włączenie stanu sesji spowoduje przerwanie obsługi komunikatów w trybie dupleksu.

### <a name="no-suitable-method-to-override"></a>Brak odpowiedniej metody do przesłonięcia

Ten błąd może pojawić się, jeśli używasz kodu ze starszej dokumentacji lub blogów. Sprawdź, czy nie odwołują się do nazw metod, które zostały zmienione lub przestarzałe (takie jak `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions. WebSocketServerUrl ma wartość null

To zachowanie jest celowe. Ten element członkowski jest przestarzały i nie należy go używać.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"Sygnał" trasy o nazwie ". centra" jest już w kolekcji tras "błąd"

Ten błąd będzie widoczny, jeśli `MapHubs` jest wywoływana dwukrotnie przez aplikację. Niektóre przykładowe aplikacje wywołują `MapHubs` bezpośrednio w pliku aplikacji globalnej; inne tworzą wywołanie w klasie otoki. Upewnij się, że aplikacja nie wykonuje obu tych czynności.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemy z programem Visual Studio

W tej sekcji opisano problemy występujące w programie Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Węzeł dokumenty skryptu nie jest wyświetlany w Eksplorator rozwiązań

Niektóre z naszych samouczków kierują do węzła "dokumenty skryptu" w Eksplorator rozwiązań podczas debugowania. Ten węzeł jest tworzony przez debuger JavaScript i pojawia się tylko podczas debugowania klientów przeglądarki w programie Internet Explorer; węzeł nie będzie wyświetlany, jeśli są używane przeglądarki Chrome lub Firefox. Debuger JavaScript również nie zostanie uruchomiony, jeśli jest uruchomiony inny debuger klienta, na przykład debuger Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>Sygnalizujący nie działa w programie Visual Studio 2008 lub starszym

To zachowanie jest celowe. Sygnalizujący wymaga .NET Framework 4 lub nowszego; wymaga to, aby aplikacje sygnalizujące były opracowywane w programie Visual Studio 2010 lub nowszym.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemy z usługami IIS

Ta sekcja zawiera problemy z Internet Information Services.

### <a name="web-site-crashes-after-maphubs-call"></a>Awaria witryny sieci Web po wywołaniu MapHubs

Ten problem został rozwiązany w najnowszej wersji programu sygnalizującego. Sprawdź, czy używasz najnowszej wydanej wersji usługi sygnalizującej, aktualizując instalację przy użyciu narzędzia NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemy z platformą Azure

Ta sekcja zawiera problemy z Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Po zmianie nazw tematów nie są odbierane komunikaty z planu platformy Azure.

Tematy używane przez plan Azure nie są przeznaczone do konfiguracji użytkownika.
