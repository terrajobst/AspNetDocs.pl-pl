---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Zrozumienie i obsługa zdarzeń okresu istnienia połączenia w sygnale 1. x | Microsoft Docs
author: bradygaster
description: W tym artykule opisano sposób korzystania z zdarzeń udostępnianych przez interfejs API centrów.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 2fb671e730a1d41c07b350bf1d64ac1d0b1be55c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536905"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Zrozumienie i obsługa zdarzeń okresu istnienia połączenia w sygnale 1. x

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ten artykuł zawiera omówienie zdarzeń związanych z połączeniem, ponownym połączeniem i rozłączaniem, które można obsłużyć, oraz ustawień limitu czasu i utrzymywania aktywności, które można skonfigurować.
> 
> W artykule założono, że masz już pewną wiedzę o zdarzeniach dotyczących czasu istnienia sygnalizującego i połączenia. Aby zapoznać się z wprowadzeniem do sygnalizacji, zobacz [sygnalizacja — Omówienie — wprowadzenie](index.md). Aby uzyskać listę zdarzeń okresu istnienia połączenia, zobacz następujące zasoby:
> 
> - [Jak obsłużyć zdarzenia okresu istnienia połączenia w klasie centrów](index.md)
> - [Jak obsługiwać zdarzenia okresu istnienia połączenia w klientach JavaScript](index.md)
> - [Jak obsługiwać zdarzenia okresu istnienia połączenia w klientach platformy .NET](index.md)

## <a name="overview"></a>Omówienie

Ten artykuł zawiera następujące sekcje:

- [Terminologia i scenariusze okresu istnienia połączenia](#terminology)

    - [Połączenia sygnalizujące, połączenia transportowe i połączenia fizyczne](#signalrvstransport)
    - [Scenariusze związane z transportem transportu](#transportdisconnect)
    - [Scenariusze rozłączenia klientów](#clientdisconnect)
    - [Scenariusze odłączenia serwera](#serverdisconnect)
- [Ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive)

    - [Parametru](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [Utrzymywani](#keepalive)
    - [Jak zmienić limit czasu i ustawienia utrzymywania aktywności](#changetimeout)
- [Jak powiadomić użytkownika o rozwiązaniu](#notifydisconnect)
- [Jak ciągle ponownie nawiązać połączenie](#continuousreconnect)
- [Jak rozłączyć klienta w kodzie serwera](#disconnectclientfromserver)

Łącza do tematów dotyczących odwołań do interfejsów API znajdują się w wersji programu .NET 4,5 interfejsu API. Jeśli używasz programu .NET 4, zobacz [temat wersja interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminologia i scenariusze okresu istnienia połączenia

Program obsługi zdarzeń `OnReconnected` w centrum sygnałów można wykonać bezpośrednio po `OnConnected`, ale nie po `OnDisconnected` dla danego klienta. Przyczyną może być ponowne połączenie bez rozłączenia, ponieważ istnieje kilka sposobów, w których słowo "Connection" jest używane w programie sygnalizującym.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Połączenia sygnalizujące, połączenia transportowe i połączenia fizyczne

Ten artykuł rozróżni się między *połączeniami sygnalizującymi*, *połączeniami transportowymi*i *połączeniami fizycznymi*:

- **Połączenie sygnalizujące** odnosi się do logicznej relacji między klientem a adresem URL serwera, obsługiwane przez interfejs API sygnalizujące i jednoznacznie identyfikowane przez identyfikator połączenia. Dane dotyczące tej relacji są obsługiwane przez program sygnalizujący i są używane do nawiązywania połączenia transportowego. Relacje kończą się i sygnalizujący usuwa dane, gdy klient wywołuje metodę `Stop` lub limit czasu zostanie osiągnięty podczas próby ponownego nawiązania połączenia z utraconym transportem przez sygnał.
- **Połączenie transportowe** odwołuje się do logicznej relacji między klientem a serwerem, który jest obsługiwany przez jeden z czterech interfejsów API transportu: WebSockets, zdarzeń wysyłanych przez serwer, ramki nieograniczonej lub długiej sondowania. Program sygnalizujący używa interfejsu API transportu do utworzenia połączenia transportowego, a interfejs API transportu zależy od istnienia fizycznego połączenia sieciowego w celu utworzenia połączenia transportowego. Połączenie transportowe kończy się, gdy sygnał kończy działanie lub gdy interfejs API transportu wykryje, że połączenie fizyczne zostało przerwane.
- **Połączenie fizyczne** dotyczy linków sieci fizycznej — przewodów, sygnałów bezprzewodowych, routerów itp., które ułatwiają komunikację między komputerem klienckim a serwerem. Połączenie fizyczne musi być obecne, aby można było nawiązać połączenie transportowe, a połączenie transportowe zostało nawiązane w celu nawiązania połączenia z sygnałem. Jednak przerwanie połączenia fizycznego nie zawsze powoduje natychmiastowe zakończenie połączenia transportowego lub połączenia sygnalizującego, jak zostało to opisane w dalszej części tego tematu.

Na poniższym diagramie połączenie sygnalizujące jest reprezentowane przez interfejs API centrów i warstwę sygnałów interfejsu API PersistentConnection, połączenie transportowe jest reprezentowane przez warstwę Transports, a połączenie fizyczne jest reprezentowane przez linie między serwerem. i klienci programu.

![Diagram architektury sygnałów](handling-connection-lifetime-events/_static/image1.png)

Po wywołaniu metody `Start` w kliencie sygnalizującym jest dostarczany kod klienta sygnalizującego ze wszystkimi informacjami, które są potrzebne w celu nawiązania połączenia fizycznego z serwerem. Kod klienta sygnalizujący używa tych informacji do utworzenia żądania HTTP i ustanowienia połączenia fizycznego korzystającego z jednej z czterech metod transportu. Jeśli połączenie transportowe nie powiedzie się lub serwer ulegnie awarii, połączenie sygnalizujące nie zostanie natychmiast odrzucone, ponieważ klient nadal ma informacje potrzebne do automatycznego ponownego ustanowienia nowego połączenia transportowego z tym samym adresem URL sygnalizującego. W tym scenariuszu nie jest uwzględniana interwencja z aplikacji użytkownika, a gdy kod klienta sygnalizującego ustanawia nowe połączenie transportowe, nie zostanie uruchomione nowe połączenie sygnalizujące. Ciągłość połączenia sygnalizującego jest odzwierciedlona w tym fakcie, że identyfikator połączenia, który jest tworzony podczas wywoływania metody `Start`, nie zmienia się.

Procedura obsługi zdarzeń `OnReconnected` w centrum jest wykonywana po ponownym nawiązaniu połączenia transportowego po jego utracie. Procedura obsługi zdarzeń `OnDisconnected` jest wykonywana po zakończeniu połączenia sygnalizującego. Połączenie sygnalizujące może kończyć się jednym z następujących sposobów:

- Jeśli klient wywoła metodę `Stop`, do serwera zostanie wysłany komunikat zatrzymania, a zarówno klient, jak i serwer kończą połączenie sygnalizujące.
- Po utracie połączenia między klientem a serwerem klient próbuje ponownie nawiązać połączenie, a serwer czeka na ponowne nawiązanie połączenia przez klienta. Jeśli próba ponownego połączenia nie powiedzie się i upłynie limit czasu rozłączenia, zarówno klient, jak i serwer zakończą połączenie sygnalizujące. Klient przestanie próbować ponownie nawiązać połączenie, a serwer usuwa jego reprezentację połączenia sygnalizującego.
- Jeśli klient przestanie działać bez możliwości wywołania metody `Stop`, serwer czeka, aż klient ponownie nawiąże połączenie, a następnie zakończy połączenie sygnalizujące po upływie limitu czasu rozłączenia.
- Jeśli serwer przestanie działać, klient próbuje ponownie nawiązać połączenie (ponownie utworzyć połączenie transportowe), a następnie zakończy połączenie sygnalizujące po upływie limitu czasu rozłączenia.

Gdy nie występują problemy z połączeniem, a aplikacja użytkownika kończy połączenie sygnalizujące, wywołując metodę `Stop`, połączenie sygnalizujące i rozpoczęcie i zakończenie połączenia transportowego. W poniższych sekcjach opisano bardziej szczegółowo inne scenariusze.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scenariusze związane z transportem transportu

Połączenia fizyczne mogą być wolne lub mogą występować przerwy w łączności. W zależności od czynników, takich jak długość przerwania, połączenie transportowe może zostać porzucone. Program sygnalizujący próbę ponownego nawiązania połączenia transportowego. Czasami interfejs API połączenia transportowego wykrywa przerwanie i opuszcza połączenie transportowe, a sygnalizujące natychmiast, że połączenie zostało utracone. W innych scenariuszach ani interfejs API połączenia transportowego ani sygnał nie są świadome natychmiast, gdy połączenie zostało utracone. W przypadku wszystkich transportów z wyjątkiem długiego sondowania klient sygnalizujący używa funkcji o *nazwie* in, aby sprawdzić, czy nie jest możliwe wykrycie łączności przez interfejs API transportu. Aby uzyskać informacje na temat długich połączeń sondowania, zobacz [Ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive) w dalszej części tego tematu.

Gdy połączenie jest nieaktywne, okresowo wysyła do klienta pakiet utrzymywania aktywności. Od daty zapisania tego artykułu domyślna częstotliwość wynosi co 10 sekund. Przez nasłuchiwanie tych pakietów klienci mogą stwierdzić, czy występuje problem z połączeniem. Jeśli pakiet utrzymywania aktywności nie zostanie odebrany w oczekiwany sposób, po krótkim czasie klient zakłada, że wystąpiły problemy z połączeniem, takie jak spowolnienie lub przerwy. Jeśli w dalszym ciągu nie otrzymasz czasu utrzymywania, klient zakłada, że połączenie zostało porzucone i rozpocznie próbę ponownego nawiązania połączenia.

Na poniższym diagramie przedstawiono zdarzenia klienta i serwera, które zostały zgłoszone w typowym scenariuszu w przypadku problemów z połączeniem fizycznym, które nie są rozpoznawane bezpośrednio przez interfejs API transportu. Diagram ma zastosowanie do następujących sytuacji:

- Transport to obiekty WebSockets, nieskończone ramki lub zdarzenia wysyłane przez serwer.
- Istnieją różne okresy przerwy w połączeniu z siecią fizyczną.
- Interfejs API transportu nie jest świadomy przerw w działaniu, więc usługa sygnalizująca jest zależna od funkcjonalności w celu ich wykrycia.

![Połączenia transportowe](handling-connection-lifetime-events/_static/image2.png)

Jeśli klient przejdzie w tryb ponownego połączenia, ale nie może nawiązać połączenia transportowego w ramach limitu czasu rozłączenia, serwer przerywa połączenie sygnalizujące. W takim przypadku serwer wykonuje metodę `OnDisconnected` centrum i kolejkuje komunikat rozłączenia w celu wysłania go do klienta na wypadek, gdyby klient zarządzał w celu późniejszego nawiązania połączenia. Jeśli klient ponownie nawiązuje połączenie, odbiera polecenie Disconnect i wywołuje metodę `Stop`. W tym scenariuszu `OnReconnected` nie jest wykonywane, gdy klient ponownie nawiązuje połączenie, a `OnDisconnected` nie jest wykonywane, gdy klient wywoła `Stop`. Na poniższym diagramie przedstawiono ten scenariusz.

![Zakłócenia transportu — limit czasu serwera](handling-connection-lifetime-events/_static/image3.png)

Zdarzenia okresu istnienia połączenia sygnalizującego, które mogą zostać zgłoszone na kliencie, są następujące:

- `ConnectionSlow` zdarzenie klienta.

    Uruchamiany, gdy otrzymano wstępną część okresu limitu czasu utrzymywania aktywności od momentu odebrania ostatniej wiadomości lub polecenia ping o utrzymywaniu aktywności. Domyślny okres ostrzegawczy limitu czasu utrzymywania aktywności wynosi 2/3. Limit czasu utrzymywania aktywności wynosi 20 sekund, więc ostrzeżenie występuje przez około 13 sekund.

    Domyślnie serwer wysyła polecenia ping o utrzymywanie aktywności co 10 sekund, a klient sprawdza, czy nie działa polecenie ping o utrzymywanie aktywności co 2 sekundy (jedna trzecia różnica między wartością limitu czasu utrzymywania aktywności a wartością ostrzegawczą limitu czasu utrzymywania aktywności).

    Jeśli interfejs API transportu zostanie poinformowany o rozwiązaniu, sygnalizujący może zostać poinformowany o rozwiązaniu, zanim upłynie okres ostrzegawczy limitu czasu utrzymywania aktywności. W takim przypadku zdarzenie `ConnectionSlow` nie zostanie wywołane, a program sygnalizujący przechodzi bezpośrednio do zdarzenia `Reconnecting`.
- `Reconnecting` zdarzenie klienta.

    Wywoływane, gdy (a) interfejs API transportu wykryje, że połączenie zostało utracone lub (b) upłynął limit czasu utrzymywania aktywności od momentu odebrania ostatniej wiadomości lub polecenia ping o utrzymywaniu aktywności. Kod klienta sygnalizującego rozpoczyna próbę ponownego nawiązania połączenia. To zdarzenie można obsłużyć, jeśli aplikacja ma podejmować pewne działania po utracie połączenia transportowego. Domyślny limit czasu utrzymywania aktywności wynosi obecnie 20 sekund.

    Jeśli kod klienta próbuje wywołać metodę Hub, podczas gdy sygnalizujący jest w trybie ponownego łączenia, sygnalizujący spróbuje wysłać polecenie. W większości przypadków takie próby zakończą się niepowodzeniem, ale w pewnych okolicznościach mogą się one kończyć pomyślnie. Dla zdarzeń wysyłanych przez serwer, ramki bez ograniczeń i długotrwałe transporty, sygnalizujący używa dwóch kanałów komunikacyjnych, które są używane przez klienta do wysyłania komunikatów i tych, które są używane do odbierania komunikatów. Kanał używany do odbioru jest trwale otwarty i jest to ten, który jest zamknięty po przerwaniu połączenia fizycznego. Kanał używany do wysyłania pozostaje dostępny, więc jeśli zostanie przywrócona łączność fizyczna, wywołanie metody z klienta do serwera może zakończyć się pomyślnie przed ponownym ustanowieniem kanału odbiorczego. Wartość zwracana nie zostanie odebrana, dopóki nie zostanie ponownie otwarty kanał używany do odbierania.
- `Reconnected` zdarzenie klienta.

    Uruchamiany po ponownej nawiązaniu połączenia transportowego. Program obsługi zdarzeń `OnReconnected` w centrum wykonuje.
- `Closed` zdarzenie klienta (`disconnected` zdarzenia w języku JavaScript).

    Uruchamiany po upływie limitu czasu rozłączenia, gdy kod klienta sygnalizującego próbuje ponownie nawiązać połączenie po utracie połączenia transportowego. Domyślny limit czasu rozłączenia wynosi 30 sekund. (To zdarzenie jest również wywoływane, gdy połączenie zostanie zakończone, ponieważ metoda `Stop` jest wywoływana).

Przerwania połączenia transportowego, które nie są wykrywane przez interfejs API transportu i nie opóźnią odbierania poleceń ping o utrzymywaniu aktywności z serwera dłużej niż okres ostrzegawczy limitu czasu utrzymywania aktywności, mogą nie spowodować wystąpienia zdarzeń istnienia połączenia.

Niektóre środowiska sieciowe świadomie zamykają połączenia bezczynne, a inna funkcja pakietów utrzymywania aktywności polega na tym, że te sieci wiedzą, że połączenie sygnalizujące jest w użyciu. W skrajnych przypadkach domyślna częstotliwość działania poleceń ping z utrzymywaniem aktywności może być niewystarczająca, aby zapobiec zamkniętym nawiązywaniu połączeń. W takim przypadku można skonfigurować polecenie ping o utrzymywanie aktywności, aby było wysyłane częściej. Aby uzyskać więcej informacji, zobacz [Ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive) w dalszej części tego tematu.

> [!NOTE] 
> 
> [!IMPORTANT]
> Sekwencja zdarzeń opisana tutaj nie jest gwarantowana. Program sygnalizujący podejmuje próbę zgłoszenia zdarzeń okresu istnienia połączenia w przewidywalny sposób zgodnie z tym schematem, ale istnieje wiele wariantów zdarzeń sieciowych i wiele sposobów, w których podstawowe struktury komunikacji, takie jak interfejsy API transportu, obsługują je. Na przykład zdarzenie `Reconnected` może nie zostać zgłoszone, gdy klient ponownie nawiązuje połączenie, lub procedura obsługi `OnConnected` na serwerze może zostać uruchomiona, gdy próba nawiązania połączenia nie powiedzie się. W tym temacie opisano tylko efekty, które zwykle są wytwarzane przez niektóre typowe okoliczności.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scenariusze rozłączenia klientów

W kliencie przeglądarki kod klienta sygnalizującego, który utrzymuje połączenie sygnalizujące, jest uruchamiany w kontekście JavaScript strony sieci Web. To dlatego, że połączenie sygnalizujące ma kończyć się po przejściu z jednej strony do drugiej i dlatego masz wiele połączeń z wieloma identyfikatorami połączenia, jeśli łączysz się z wielu okien przeglądarki lub kart. Gdy użytkownik zamknie okno lub kartę przeglądarki lub nawiguje do nowej strony lub odświeża stronę, połączenie sygnalizujące następuje natychmiast, ponieważ kod klienta sygnalizującego obsługuje to zdarzenie przeglądarki i wywołuje metodę `Stop`. W tych scenariuszach lub na dowolnej platformie klienta, gdy aplikacja wywołuje metodę `Stop`, program obsługi zdarzeń `OnDisconnected` jest wykonywany natychmiast na serwerze, a klient zgłasza zdarzenie `Closed` (zdarzenie ma nazwę `disconnected` w języku JavaScript).

Jeśli aplikacja kliencka lub komputer, na którym działa awaria lub przejdzie do trybu uśpienia (na przykład gdy użytkownik zamknie laptop), serwer nie zostanie poinformowany o tym, co się stało. W miarę jak serwer wie, że utrata klienta może być spowodowana przerwaniem łączności, a klient może próbować ponownie nawiązać połączenie. Dlatego w tych scenariuszach serwer czeka, aby klient mógł ponownie nawiązać połączenie, a `OnDisconnected` nie wykonywał do momentu wygaśnięcia okresu rozłączenia (domyślnie około 30 sekund). Na poniższym diagramie przedstawiono ten scenariusz.

![Awaria komputera klienckiego](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scenariusze odłączenia serwera

Gdy serwer przejdzie w tryb offline — wykonuje ponowny rozruch, Niepowodzenie, odzyskanie domeny aplikacji itp. — wynik może być podobny do utraconego połączenia lub interfejs API transportu i sygnalizujący może się natychmiast dowiedzieć, że serwer został usunięty, a sygnalizujący spróbuje ponownie nawiązać połączenie bez podnoszenia zdarzenia `ConnectionSlow`. Jeśli klient przejdzie w tryb ponownej łączności, a jeśli serwer odzyska lub uruchomi ponownie lub nowy serwer zostanie przełączony w tryb online przed upływem limitu czasu rozłączenia, klient ponownie nawiąże połączenie z przywróconym lub nowym serwerem. W takim przypadku połączenie sygnalizujące kontynuuje się na kliencie i zostanie zgłoszone zdarzenie `Reconnected`. Na pierwszym serwerze `OnDisconnected` nigdy nie są wykonywane i na nowym serwerze `OnReconnected` jest wykonywane, mimo że `OnConnected` nigdy nie były wykonywane dla tego klienta na tym serwerze. (Efekt jest taki sam, jeśli klient ponownie nawiązuje połączenie z tym samym serwerem po ponownym uruchomieniu lub odtworzeniu domeny aplikacji), ponieważ po ponownym uruchomieniu serwera nie ma pamięci z poprzednią aktywnością połączenia. Na poniższym diagramie założono, że interfejs API transportu będzie miał natychmiastową świadomość utraconych połączeń, więc zdarzenie `ConnectionSlow` nie zostanie zgłoszone.

![Awaria serwera i ponowne połączenie](handling-connection-lifetime-events/_static/image5.png)

Jeśli serwer nie stanie się dostępny w ramach limitu czasu rozłączenia, połączenie sygnalizujące się zakończy. W tym scenariuszu na kliencie zostanie zgłoszone zdarzenie `Closed` (`disconnected` na klientach JavaScript), ale na serwerze nie jest nigdy wywoływana `OnDisconnected`. Na poniższym diagramie przyjęto założenie, że interfejs API transportu nie ma informacji o utraconym połączeniu, więc jest wykrywany przez funkcję utrzymywania aktywności sygnału i zostanie zgłoszone zdarzenie `ConnectionSlow`.

![Awaria serwera i przekroczenie limitu czasu](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Ustawienia limitu czasu i utrzymywania aktywności

Wartości domyślne `ConnectionTimeout`, `DisconnectTimeout`i `KeepAlive` są odpowiednie dla większości scenariuszy, ale można je zmienić, jeśli środowisko ma specjalne potrzeby. Na przykład jeśli środowisko sieciowe zamyka połączenia, które są bezczynne przez 5 sekund, może być konieczne zmniejszenie wartości utrzymywania aktywności.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>Parametru

To ustawienie określa czas, w którym połączenie transportowe zostanie otwarte i oczekuje na odpowiedź przed zamknięciem i otwarciem nowego połączenia. Wartość domyślna to 110 sekund.

To ustawienie ma zastosowanie tylko wtedy, gdy funkcja utrzymywania aktywności jest wyłączona, która zwykle dotyczy tylko długiego transportu sondowania. Na poniższym diagramie przedstawiono efekt tego ustawienia dla długiego połączenia transportowego sondowania.

![Długie sondowanie połączenia transportowego](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

To ustawienie reprezentuje czas oczekiwania po utracie połączenia transportowego przed podniesieniem zdarzenia `Disconnected`. Wartość domyślna to 30 sekund. Po ustawieniu `DisconnectTimeout`, `KeepAlive` jest automatycznie ustawiany na 1/3 wartości `DisconnectTimeout`.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

To ustawienie reprezentuje czas oczekiwania przed wysłaniem pakietu w trybie utrzymywania aktywności przez bezczynne połączenie. Wartość domyślna to 10 sekund. Ta wartość nie może być większa niż 1/3 wartości `DisconnectTimeout`.

Jeśli chcesz ustawić zarówno `DisconnectTimeout`, jak i `KeepAlive`, ustaw `KeepAlive` po `DisconnectTimeout`. W przeciwnym razie ustawienie `KeepAlive` zostanie nadpisane, gdy `DisconnectTimeout` automatycznie ustawi `KeepAlive` na 1/3 wartości limitu czasu.

Jeśli chcesz wyłączyć funkcję utrzymywania aktywności, ustaw `KeepAlive` na wartość null. Funkcja utrzymywania aktywności jest automatycznie wyłączana dla długiego transportu sondowania.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Jak zmienić limit czasu i ustawienia utrzymywania aktywności

Aby zmienić wartości domyślne dla tych ustawień, należy ustawić je w `Application_Start` w pliku *Global. asax* , jak pokazano w poniższym przykładzie. Wartości wyświetlane w przykładowym kodzie są takie same jak wartości domyślne.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Jak powiadomić użytkownika o rozwiązaniu

W niektórych aplikacjach możesz chcieć wyświetlić komunikat dla użytkownika w przypadku problemów z łącznością. Istnieje kilka opcji, które należy wykonać, aby to zrobić. Poniższe przykłady kodu są przeznaczone dla klienta JavaScript korzystającego z wygenerowanego serwera proxy.

- Obsłuż zdarzenie `connectionSlow`, aby wyświetlić komunikat, gdy tylko sygnał jest świadomy problemów z połączeniem, przed przejściem do trybu ponownego łączenia.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Obsłuż zdarzenie `reconnecting`, aby wyświetlić komunikat, gdy sygnał zostanie powiadomiony o rozwiązaniu rozłączenia i nastąpi ponowne połączenie z trybem.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Obsłuż zdarzenie `disconnected`, aby wyświetlić komunikat w przypadku przekroczenia limitu czasu próby ponownego nawiązania połączenia. W tym scenariuszu jedynym sposobem ponownego nawiązania połączenia z serwerem jest ponowne uruchomienie połączenia sygnalizującego przez wywołanie metody `Start`, która utworzy nowy identyfikator połączenia. Poniższy przykład kodu używa flagi, aby upewnić się, że powiadomienia są wystawiane dopiero po upływie limitu czasu ponownego połączenia, a nie po normalnym zakończeniu połączenia sygnalizującego spowodowanym wywołaniem metody `Stop`.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Jak ciągle ponownie nawiązać połączenie

W niektórych aplikacjach warto automatycznie ponownie nawiązać połączenie po jego utracie, a próba ponownego nawiązania połączenia przekroczyła limit czasu. W tym celu można wywołać metodę `Start` z programu obsługi zdarzeń `Closed` (program obsługi zdarzeń`disconnected` na klientach JavaScript). Może upłynąć pewien czas przed wywołaniem `Start`, aby uniknąć tego zbyt często, gdy serwer lub połączenie fizyczne są niedostępne. Poniższy przykład kodu dotyczy klienta JavaScript korzystającego z wygenerowanego serwera proxy.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Potencjalnym problemem, które należy znać w przypadku klientów mobilnych, jest to, że ciągłe Ponowne nawiązywanie połączenia, gdy serwer lub połączenie fizyczne nie jest dostępne, może spowodować niepotrzebną utratę baterii.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Jak rozłączyć klienta w kodzie serwera

System sygnalizujący w wersji 1.1.1 nie ma wbudowanego interfejsu API serwera do odłączenia klientów. Istnieją [plany dodawania tych funkcji w przyszłości](https://github.com/SignalR/SignalR/issues/2101). W bieżącej wersji sygnalizującej Najprostszym sposobem rozłączenia klienta z serwera jest zaimplementowanie metody Disconnect na kliencie i wywołanie tej metody z serwera. Poniższy przykładowy kod przedstawia metodę Disconnect dla klienta JavaScript przy użyciu wygenerowanego serwera proxy.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Zabezpieczenia — ani ta metoda odłączania klientów i proponowany wbudowany interfejs API będzie zawierać scenariusz zaatakowana klientów, którzy uruchamiają złośliwy kod, ponieważ klienci mogą ponownie nawiązać połączenie lub kod zaatakowana może usunąć metodę `stopClient` lub zmienić jej działanie. Odpowiednie miejsce na wdrożenie ochrony stanowej "odmowa usługi" (DOS) nie znajduje się w strukturze ani warstwie serwera, ale raczej w infrastrukturze frontonu.
