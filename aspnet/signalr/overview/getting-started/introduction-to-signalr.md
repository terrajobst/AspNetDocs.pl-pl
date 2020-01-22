---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Wprowadzenie do sygnalizującego | Microsoft Docs
author: bradygaster
description: W tym artykule opisano, co to jest sygnał, i niektóre z rozwiązań, które zostały zaprojektowane do utworzenia.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8dbc31a5c8d59fa55dc5b513c1a51d24d18a685f
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519404"
---
# <a name="introduction-to-signalr"></a>Wprowadzenie do usługi SignalR

[Fletcher Patryka](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym artykule opisano, co to jest sygnał, i niektóre z rozwiązań, które zostały zaprojektowane do utworzenia. 
> 
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
> 
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>Co to jest sygnał?

ASP.NET sygnalizujący to biblioteka deweloperów ASP.NET, którzy upraszczają proces dodawania funkcji sieci Web w czasie rzeczywistym do aplikacji. Funkcja sieci Web w czasie rzeczywistym to możliwość natychmiastowego pozyskania przez klienta zawartości z kodem serwerowym, gdy będzie ona dostępna, a nie poczekać, aż klient zażąda nowych danych.

Sygnalizujący może służyć do dodawania dowolnego rodzaju funkcji sieci Web "w czasie rzeczywistym" do aplikacji ASP.NET. Podczas gdy rozmowa jest często używana jako przykład, można wykonać całą dużo więcej. Za każdym razem, gdy użytkownik Odświeża stronę sieci Web, aby zobaczyć nowe dane, lub strona implementuje [długotrwałe sondowanie](http://en.wikipedia.org/wiki/Push_technology#Long_polling) w celu pobrania nowych danych, jest kandydatem do korzystania z usługi sygnalizującej. Przykłady obejmują pulpity nawigacyjne i aplikacje monitorujące, aplikacje do współpracy (takie jak jednoczesne edytowanie dokumentów), aktualizacje postępu zadań i formularze w czasie rzeczywistym.

Program sygnalizujący umożliwia również zupełnie nowe typy aplikacji sieci Web, które wymagają aktualizacji o wysokiej częstotliwości z serwera, na przykład w czasie rzeczywistym.

Sygnalizujący oferuje prosty interfejs API służący do tworzenia zdalnych wywołań procedur (RPC) serwer-klient, które wywołują funkcje języka JavaScript w przeglądarkach klienta (i innych platformach klienckich) z kodu platformy .NET po stronie serwera. Program sygnalizujący obejmuje również interfejs API służący do zarządzania połączeniami (na przykład zdarzenia łączenia i rozłączania) oraz grupowania połączeń.

![Wywoływanie metod przy użyciu sygnalizującego](introduction-to-signalr/_static/image1.png)

Usługa SignalR automatycznie obsługuje zarządzanie połączeniami i umożliwia rozgłaszanie komunikatów do wszystkich połączonych klientów równocześnie, tak w przypadku pokoju rozmów. Można również wysyłać komunikaty do określonych klientów. Połączenie między klientem i serwerem jest trwałe w odróżnieniu od klasycznego połączenia HTTP, które jest nawiązywane ponownie dla każdej komunikacji.

Usługa sygnalizująca obsługuje funkcję "wypychania serwera", w której kod serwera może wywoływać kod klienta w przeglądarce przy użyciu zdalnych wywołań procedur (RPC), a nie modelu odpowiedzi na żądanie, który jest często używany w sieci Web.

Aplikacje sygnalizujące mogą być skalowane do tysięcy klientów przy użyciu wbudowanych dostawców skalowalnych w poziomie i innych firm.

Dostawcy wbudowani obejmują:
* [Service Bus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)
* [SQL Server](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
* [Redis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Redis)

Dostawcy innych firm obejmują:
* [NCache](https://www.alachisoft.com/ncache/asp-net-core-signalr.html).

Sygnalizujący jest otwartym źródłem, dostępnym za poorednictwem usługi [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>Sygnalizujący i WebSocket

Sygnalizujący używa nowego transportu protokołu WebSocket, jeśli jest dostępny, i powraca do starszych transportów, gdy jest to konieczne. Chociaż można napisanie aplikacji przy użyciu protokołu WebSocket bezpośrednio, przy użyciu sygnalizującego oznacza to, że wiele dodatkowych funkcji, które należy zaimplementować, jest już dla Ciebie gotowe. Co ważniejsze, oznacza to, że można pokodować aplikację, aby korzystać z protokołu WebSocket bez konieczności tworzenia oddzielnej ścieżki kodu dla starszych klientów. Sygnalizujący również osłony, które nie mają do obaw o aktualizacje protokołu WebSocket, ponieważ sygnał jest aktualizowany do obsługi zmian w podstawowym transportie, zapewniając spójny interfejs w różnych wersjach protokołu WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transporty i rezerwy

Sygnalizujący jest abstrakcją dla niektórych transportów, które są wymagane do pracy w czasie rzeczywistym między klientem i serwerem. Połączenie sygnalizujące zaczyna się od protokołu HTTP i jest następnie podwyższane do połączenia WebSocket, jeśli jest dostępne. Protokół WebSocket jest idealnym transportem dla sygnalizującego, ponieważ sprawia, że jest to najbardziej wydajne wykorzystanie pamięci serwera, ma najniższe opóźnienie i ma najbardziej podstawowe funkcje (na przykład komunikacja pełnego dupleksu między klientem a serwerem), ale również najbardziej rygorystyczne wymagania: protokół WebSocket wymaga, aby serwer używa systemu Windows Server 2012 lub Windows 8, a .NET Framework 4,5. Jeśli te wymagania nie są spełnione, program sygnalizujący podejmie próbę użycia innych transportów w celu nawiązania połączenia.

### <a name="html-5-transports"></a>Transporty HTML 5

Transporty są zależne od obsługi [języka HTML 5](http://en.wikipedia.org/wiki/HTML5). Jeśli przeglądarka klienta nie obsługuje standardu HTML 5, będą używane starsze transporty.

- **WebSocket** (Jeśli zarówno serwer, jak i przeglądarka wskazują, że mogą obsługiwać protokół WebSocket). Protokół WebSocket jest jedynym transportem, który ustanawia stałe, dwukierunkowe połączenie między klientem a serwerem. Jednak protokół WebSocket ma także najbardziej rygorystyczne wymagania; jest ona w pełni obsługiwana tylko w najnowszych wersjach programu Microsoft Internet Explorer, Google Chrome i Mozilla Firefox. obejmuje tylko częściową implementację w innych przeglądarkach, takich jak Opera i Safari.
- **Zdarzenia wysłane przez serwer**, znane także jako EventSource (Jeśli przeglądarka obsługuje zdarzenia wysłane przez serwer, czyli wszystkie przeglądarki z wyjątkiem programu Internet Explorer).

### <a name="comet-transports"></a>Transporty Comet

Poniższe transporty opierają się na modelu aplikacji sieci Web [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) , w którym przeglądarka lub inny klient przechowuje długotrwałe żądanie HTTP, którego serwer może użyć do wypychania danych do klienta bez żądania klienta.

- **Ramka bez ograniczeń** (tylko w przypadku programu Internet Explorer). Ramka bez ograniczeń tworzy ukryty element IFrame, który wysyła żądanie do punktu końcowego na serwerze, który nie został ukończony. Następnie serwer wysyła do klienta skrypt, który jest natychmiast wykonywany, co zapewnia jednokierunkowe połączenie między serwerem a klientem. Połączenie między klientem a serwerem używa oddzielnego połączenia z serwera do połączenia klienta, jak w przypadku standardowego żądania HTTP, tworzone jest nowe połączenie dla każdej części danych, które należy wysłać.
- **Długoterminowe sondowanie AJAX**. Długotrwałe sondowanie nie powoduje utworzenia połączenia trwałego, ale zamiast tego sonduje serwer przy użyciu żądania, które pozostaje otwarte do momentu odpowiedzi serwera, po upływie którego połączenie zostanie zamknięte i od razu zostanie wysłane nowe połączenie. Może to spowodować pewne opóźnienie podczas resetowania połączenia.

Aby uzyskać więcej informacji na temat tego, które transporty są obsługiwane przez konfiguracje, zobacz [obsługiwane platformy](supported-platforms.md).

### <a name="transport-selection-process"></a>Proces wyboru transportu

Na poniższej liście przedstawiono kroki, które program sygnalizujący używa do podejmowania decyzji o tym, który transport ma być używany.

1. Jeśli przeglądarka jest Internet Explorer 8 lub wcześniejsza, jest używana długa sondowanie.
2. Jeśli skonfigurowano JSONP (to oznacza, że parametr `jsonp` jest ustawiany na `true` po rozpoczęciu połączenia), używane jest długie sondowanie.
3. Jeśli nastąpi nawiązywanie połączenia między domenami (to oznacza, że punkt końcowy sygnalizujący nie znajduje się w tej samej domenie, w której znajduje się Strona hostingu), zostanie użyty protokół WebSocket, jeśli spełnione są następujące kryteria:

   - Klient obsługuje funkcję CORS (współużytkowanie zasobów między źródłami). Aby uzyskać szczegółowe informacje na temat obsługi mechanizmu CORS przez klientów, zobacz [CORS w CanIUse.com](http://www.caniuse.com/CORS).
   - Klient obsługuje protokół WebSocket
   - Serwer obsługuje protokół WebSocket

     Jeśli którekolwiek z tych kryteriów nie są spełnione, zostanie użyte długie sondowanie. Aby uzyskać więcej informacji na temat połączeń między domenami, zobacz [jak ustanowić połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Jeśli JSONP nie jest skonfigurowany i połączenie nie jest nawiązywane między domenami, zostanie użyta wartość WebSocket, jeśli klient i serwer obsługują tę funkcję.
5. Jeśli klient lub serwer nie obsługuje protokołu WebSocket, są używane zdarzenia wysłane przez serwer, jeśli są dostępne.
6. Jeśli zdarzenia wysłane przez serwer nie są dostępne, podejmowana jest próba nieograniczonej ramki.
7. Jeśli ramka bez ograniczeń nie powiedzie się, zostanie użyta długa sondowanie.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Transport monitorujący

Możesz określić, który transport jest używany przez aplikację, włączając rejestrowanie w centrum i otwierając okno konsoli w przeglądarce.

Aby włączyć rejestrowanie zdarzeń centrum w przeglądarce, Dodaj następujące polecenie do aplikacji klienckiej:

`$.connection.hub.logging = true;`

- W programie Internet Explorer otwórz narzędzia deweloperskie, naciskając klawisz F12 i klikając kartę Konsola.

    ![Konsola programu Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- W programie Chrome Otwórz konsolę, naciskając klawisze Ctrl + Shift + J.

    ![Konsola w przeglądarce Google Chrome](introduction-to-signalr/_static/image3.png)

Po otwarciu i włączeniu konsoli można zobaczyć, który transport jest używany przez program sygnalizujący.

![Konsola programu Internet Explorer pokazująca transport WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Określanie transportu

Negocjowanie transportu zajmuje pewien czas i zasoby klienta/serwera. Jeśli są znane możliwości klienta, podczas uruchamiania połączenia klienta można określić transport. Poniższy fragment kodu pokazuje, jak rozpocząć połączenie przy użyciu funkcji wieloportowego sondowania AJAX, tak jak gdyby był wiadomo, że klient nie obsługiwał żadnego innego protokołu:

`connection.start({ transport: 'longPolling' });`

Aby klient mógł wypróbować określone transporty w podanej kolejności, można określić zamówienie rezerwowe. Poniższy fragment kodu ilustruje próbę wykonania protokołu WebSocket i kończy się niepowodzeniem, przechodząc bezpośrednio do długiego sondowania.

`connection.start({ transport: ['webSockets','longPolling'] });`

Stałe ciągów do określania transportów są zdefiniowane w następujący sposób:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Połączenia i centra

Interfejs API sygnalizującego zawiera dwa modele komunikacji między klientami i serwerami: trwałe połączenia i centra.

Połączenie reprezentuje prosty punkt końcowy do wysyłania komunikatów o pojedynczym odbiorcy, pogrupowanych lub emisji. Interfejs API połączenia trwałego (reprezentowany w kodzie .NET przez klasę PersistentConnection) zapewnia deweloperowi bezpośredni dostęp do protokołu komunikacyjnego niskiego poziomu, który jest ujawniany przez sygnalizujące. Użycie modelu komunikacji połączenia będzie znane dla deweloperów, którzy korzystali z interfejsów API opartych na połączeniach, takich jak Windows Communication Foundation.

Koncentrator to bardziej wysoki poziom potoku oparty na interfejsie API połączenia, który umożliwia klientowi i serwerowi bezpośrednie wywoływanie metod. Program sygnalizujący obsługuje wysyłanie między granicami maszyn w taki sposób, aby klienci mogli wywoływać metody na serwerze jako metody lokalne i na odwrót. Korzystanie z modelu komunikacji centrów będzie znane dla deweloperów, którzy korzystali z interfejsów API wywołań zdalnych, takich jak komunikacja zdalna platformy .NET. Użycie centrum umożliwia również przekazywanie parametrów z jednoznacznie określonymi typami do metod, włączając powiązanie modelu.

### <a name="architecture-diagram"></a>Diagram architektury

Na poniższym diagramie przedstawiono relację między centrami, połączeniami trwałymi i podstawowymi technologiami używanymi do transportowania.

![Diagram architektury sygnałów przedstawiający interfejsy API, transporty i klientów](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Jak działają centra

Gdy kod po stronie serwera wywołuje metodę na kliencie, pakiet jest wysyłany przez aktywny transport, który zawiera nazwę i parametry metody do wywołania (gdy obiekt jest wysyłany jako parametr metody, jest serializowany przy użyciu JSON). Następnie klient dopasowuje nazwę metody do metod zdefiniowanych w kodzie po stronie klienta. Jeśli istnieje dopasowanie, Metoda klienta zostanie wykonana przy użyciu deserializowanych danych parametrów.

Wywołanie metody może być monitorowane przy użyciu narzędzi, takich jak [programu Fiddler.](http://fiddler2.com/) Na poniższej ilustracji przedstawiono wywołanie metody wysyłane z serwera sygnalizującego do klienta przeglądarki sieci Web w okienku dzienniki programu Fiddler. Wywołanie metody jest wysyłane z centrum o nazwie `MoveShapeHub`, a wywoływana metoda jest nazywana `updateShape`.

![Widok dziennika programu Fiddler przedstawiający ruch sygnalizujący](introduction-to-signalr/_static/image6.png)

W tym przykładzie nazwa centrum jest identyfikowana przy użyciu parametru `H`; Nazwa metody jest identyfikowana przy użyciu parametru `M`, a dane wysyłane do metody są identyfikowane za pomocą parametru `A`. Aplikacja, która wygenerowała ten komunikat, jest tworzona w samouczku [o wysokiej częstotliwości w czasie rzeczywistym](tutorial-high-frequency-realtime-with-signalr.md) .

### <a name="choosing-a-communication-model"></a>Wybieranie modelu komunikacji

Większość aplikacji powinna używać interfejsu API centrów. Interfejs API połączeń może być używany w następujących okolicznościach:

- Należy określić format rzeczywistej wiadomości wysyłanej.
- Deweloper woli pracować z modelem przesyłania komunikatów i wysyłania wiadomości, a nie z modelem zdalnego wywoływania.
- Istniejąca aplikacja, która używa modelu obsługi komunikatów, jest przeszukiwana do korzystania z programu sygnalizującego.
