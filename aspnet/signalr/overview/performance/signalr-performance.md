---
uid: signalr/overview/performance/signalr-performance
title: Wydajność sygnalizująca | Microsoft Docs
author: bradygaster
description: Wydajność usługi SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558325"
---
# <a name="signalr-performance"></a>Wydajność usługi SignalR

[Fletcher Patryka](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie opisano sposób projektowania, mierzenia i poprawiania wydajności w aplikacji sygnalizującej.
>
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizujący wersja 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
>
> Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

Aby uzyskać ostatnią prezentację na temat wydajności i skalowania sygnału, zobacz [skalowanie sieci Web w czasie rzeczywistym za pomocą sygnału ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).

Ten temat zawiera następujące sekcje:

- [Zagadnienia dotyczące projektowania](#design)
- [Dostrajanie serwera sygnalizującego pod kątem wydajności](#tuning)
- [Rozwiązywanie problemów z wydajnością](#troubleshooting)
- [Korzystanie z liczników wydajności sygnałów](#perfcounters)
- [Korzystanie z innych liczników wydajności](#othercounters)
- [Inne zasoby](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Zagadnienia dotyczące projektowania

W tej sekcji opisano wzorce, które mogą być implementowane podczas projektowania aplikacji sygnalizującej, aby zapewnić, że wydajność nie jest zakłócana przez wygenerowanie niepotrzebnego ruchu sieciowego.

### <a name="throttling-message-frequency"></a>Częstotliwość komunikatów ograniczania przepustowości

Nawet w aplikacji, która wysyła komunikaty z dużą częstotliwością (na przykład aplikacja do gier w czasie rzeczywistym), większość aplikacji nie musi wysyłać więcej niż kilka komunikatów na sekundę. Aby zmniejszyć ilość ruchu wygenerowanego przez każdego klienta, można zaimplementować pętlę komunikatów, która nie jest częściej niż stała stawka (oznacza to, że do określonej liczby komunikatów będą wysyłane co sekundę, jeśli w tym czasie znajdują się komunikaty terval do wysłania). W przypadku przykładowej aplikacji, która ogranicza komunikaty do określonej stawki (zarówno z klienta i serwera), zobacz czas [rzeczywisty o wysokiej częstotliwości z sygnalizującym](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Zmniejszanie rozmiaru wiadomości

Możesz zmniejszyć rozmiar komunikatu sygnalizującego, zmniejszając rozmiar serializowanych obiektów. W przypadku wysyłania obiektu, który zawiera właściwości, które nie muszą być przesyłane, w kodzie serwera, należy zapobiec serializacji tych właściwości przy użyciu atrybutu `JsonIgnore`. Nazwy właściwości są również przechowywane w komunikacie. nazwy właściwości można skrócić przy użyciu atrybutu `JsonProperty`. Poniższy przykład kodu demonstruje, jak wykluczyć właściwość z wysyłania do klienta i jak skrócić nazwy właściwości:

**Kod serwera .NET, który demonstruje atrybut JsonIgnore, aby wykluczyć dane wysyłane do klienta i atrybut JsonProperty w celu zmniejszenia rozmiaru komunikatu**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Aby zachować czytelność/łatwość utrzymania w kodzie klienta, skrócone nazwy właściwości mogą być ponownie mapowane na przyjazne nazwy po odebraniu wiadomości. Poniższy przykład kodu ilustruje jeden z możliwych sposobów ponownego mapowania skróconych nazw do większej, przez zdefiniowanie kontraktu komunikatu (mapowanie) i użycie funkcji `reMap` do zastosowania kontraktu do klasy zoptymalizowanego komunikatu:

**Kod JavaScript po stronie klienta, który ponownie mapuje skrócone nazwy właściwości na nazwy czytelne dla człowieka**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Nazwy można skrócić również w komunikatach z klienta do serwera, używając tej samej metody.

Zmniejszenie ilości pamięci (czyli ilości pamięci używanej przez komunikat) obiektu komunikatu może również zwiększyć wydajność. Na przykład jeśli pełny zakres `int` nie jest wymagany, zamiast tego można użyć `short` lub `byte`.

Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie rozmiaru komunikatów może również rozwiązać problemy z pamięcią serwera.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Dostrajanie serwera sygnalizującego pod kątem wydajności

Poniższe ustawienia konfiguracji mogą służyć do dostrajania serwera w celu zapewnienia lepszej wydajności w aplikacji sygnalizującej. Aby uzyskać ogólne informacje na temat ulepszania wydajności w aplikacji ASP.NET, zobacz [Poprawianie wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Ustawienia konfiguracji sygnalizującego**

- **DefaultMessageBufferSize**: domyślnie sygnalizujący przechowuje 1000 komunikatów w pamięci na koncentratorze dla każdego połączenia. W przypadku korzystania z dużych komunikatów może to spowodować problemy z pamięcią, które można wyeliminować, zmniejszając tę wartość. To ustawienie można ustawić w obsłudze zdarzeń `Application_Start` w aplikacji ASP.NET lub w metodzie `Configuration` klasy uruchomieniowej OWIN w aplikacji samohostowanej. Poniższy przykład demonstruje, jak zmniejszyć tę wartość, aby zmniejszyć rozmiar pamięci aplikacji w celu zmniejszenia ilości używanej pamięci serwera:

    **Kod programu .NET Server w Startup.cs na potrzeby zmniejszania domyślnego rozmiaru buforu komunikatów**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Ustawienia konfiguracji usług IIS**

- **Maksymalna liczba współbieżnych żądań na aplikację**: zwiększenie liczby równoczesnych żądań usług IIS spowoduje zwiększenie zasobów serwera dostępnych do obsługi żądań. Wartość domyślna to 5000; Aby zwiększyć to ustawienie, wykonaj następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool queuelength**: jest to maksymalna liczba żądań, które są kolejki http. sys dla puli aplikacji. Gdy kolejka jest pełna, nowe żądania otrzymują odpowiedź "Usługa niedostępna" w usłudze 503. Wartość domyślna to 1000.

    Skrócenie długości kolejki procesu roboczego w puli aplikacji obsługującej aplikację spowoduje zapełnienie zasobów pamięci. Aby uzyskać więcej informacji, zobacz [Zarządzanie, dostrajanie i Konfigurowanie pul aplikacji](https://technet.microsoft.com/library/cc745955.aspx).

**Ustawienia konfiguracji ASP.NET**

Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w pliku `aspnet.config`. Ten plik znajduje się w jednej z dwóch lokalizacji, w zależności od platformy:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Ustawienia ASP.NET, które mogą poprawić wydajność sygnalizującego, obejmują następujące elementy:

- **Maksymalna liczba współbieżnych żądań na procesor CPU**: zwiększenie tego ustawienia może złagodzić wąskie gardła wydajności. Aby zwiększyć to ustawienie, należy dodać następujące ustawienia konfiguracji do pliku `aspnet.config`:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limit kolejki żądań**: gdy całkowita liczba połączeń przekroczy ustawienie `maxConcurrentRequestsPerCPU`, ASP.NET rozpocznie ograniczanie żądań przy użyciu kolejki. Aby zwiększyć rozmiar kolejki, można zwiększyć ustawienie `requestQueueLimit`. W tym celu należy dodać następujące ustawienia konfiguracji do węzła `processModel` w `config/machine.config` (zamiast `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Rozwiązywanie problemów z wydajnością

W tej sekcji opisano sposoby znajdowania wąskich gardeł wydajności w aplikacji.

### <a name="verifying-that-websocket-is-being-used"></a>Weryfikowanie, czy protokół WebSocket jest używany

Chociaż sygnalizujący może używać różnych transportów komunikacji między klientem i serwerem, protokół WebSocket oferuje znaczną wydajność i należy go używać, jeśli klient i serwer obsługują tę opcję. Aby określić, czy klient i serwer spełniają wymagania dotyczące protokołu WebSocket, zobacz [Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports). Aby określić, który transport jest używany w aplikacji, możesz użyć narzędzi deweloperskich przeglądarki i sprawdzić dzienniki, aby zobaczyć, jaki transport jest używany do połączenia. Aby uzyskać informacje na temat korzystania z narzędzi programistycznych przeglądarki w programie Internet Explorer i programie Chrome, zobacz [Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Korzystanie z liczników wydajności sygnałów

W tej sekcji opisano sposób włączania i używania liczników wydajności sygnałów, które znajdują się w pakiecie `Microsoft.AspNet.SignalR.Utils`.

### <a name="installing-signalrexe"></a>Instalowanie sygnalizującer. exe

Liczniki wydajności można dodać do serwera za pomocą narzędzia o nazwie Sygnalizującer. exe. Aby zainstalować to narzędzie, wykonaj następujące kroki:

1. W programie Visual Studio wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **Zarządzanie pakietami NuGet dla rozwiązania**
2. Wyszukaj pozycję **sygnalizujący.** narzędzia i wybierz pozycję Zainstaluj.

    ![](signalr-performance/_static/image1.png)
3. Zaakceptuj umowę licencyjną, aby zainstalować pakiet.
4. Program sygnalizujący. exe zostanie zainstalowany do `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Instalowanie liczników wydajności za pomocą programu Signaler. exe

Aby zainstalować liczniki wydajności sygnałów, uruchom program Sygnalizującer. exe w wierszu polecenia z podwyższonym poziomem uprawnień z następującym parametrem:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Aby usunąć liczniki wydajności sygnałów, uruchom program Signaler. exe w wierszu polecenia z podwyższonym poziomem uprawnień z następującym parametrem:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Liczniki wydajności sygnałów

Pakiet narzędzi instaluje następujące liczniki wydajności. Liczniki "Total" mierzą liczbę zdarzeń od momentu ostatniego ponownego uruchomienia serwera.

**Metryki połączeń**

Następujące metryki mierzą zdarzenia okresu istnienia połączenia, które wystąpiły. Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Połączenia połączone**
- **Połączenia zostały ponownie połączone**
- **Połączenia rozłączone**
- **Bieżące połączenia**

**Metryki komunikatów**

Następujące metryki mierzą ruch komunikatów generowany przez program sygnalizujący.

- **Całkowita liczba odebranych komunikatów połączenia**
- **Całkowita liczba wysłanych komunikatów połączenia**
- **Odebrane komunikaty połączenia/s**
- **Wysłane komunikaty połączenia/s**

**Metryki magistrali komunikatów**

Następujące metryki mierzą ruch przez magistralę komunikatów wewnętrznych sygnalizujących, kolejkę, w której są umieszczane wszystkie komunikaty sygnalizujące przychodzące i wychodzące. Komunikat jest **publikowany** , gdy jest wysyłany lub emitowany. **Subskrybent** w tym kontekście jest subskrypcją magistrali komunikatów; Ta wartość powinna być równa liczbie klientów i serwera. **Przydzielony proces roboczy** to składnik, który wysyła dane do aktywnych połączeń; **zajęty proces roboczy** to ten, który aktywnie wysyła komunikat.

- **Całkowita liczba odebranych komunikatów magistrali komunikatów**
- **Odebrane komunikaty magistrali komunikatów/s**
- **Łączna liczba opublikowanych komunikatów magistrali komunikatów**
- **Opublikowane komunikaty magistrali komunikatów/s**
- **Aktualni Subskrybenci magistrali komunikatów**
- **Łączna liczba subskrybentów magistrali komunikatów**
- **Subskrybenci magistrali komunikatów/s**
- **Pracownicy przydzieleni do magistrali komunikatów**
- **Pracownikom zajętym przez magistralę komunikatów**
- **Bieżące tematy dotyczące magistrali komunikatów**

**Metryki błędów**

Następujące metryki mierzą błędy generowane przez ruch komunikatów sygnalizujących. Błędy **rozpoznawania centrów** występują, gdy nie można rozpoznać metody Hub lub Hub. Błędy **wywołania centrum** to wyjątki zgłoszone podczas wywoływania metody centrum. Błędy **transportu** to błędy połączeń zgłoszone podczas żądania HTTP lub odpowiedzi.

- **Błędy: wszystkie łącznie**
- **Błędy: wszystkie/s**
- **Błędy: rozdzielczość centrum łącznie**
- **Błędy: rozdzielczość centrum/s**
- **Błędy: całkowita liczba wywołań koncentratora**
- **Błędy: wywołanie koncentratora/s**
- **Błędy: łącznie z transportem**
- **Błędy: transport/s**

<a id="scaleout_metrics"></a>

**Metryki skalowania**

Następujące metryki mierzą ruch i błędy generowane przez dostawcę skalowania. **Strumień** w tym kontekście jest jednostką skalowania używaną przez dostawcę skalowania. jest to tabela, jeśli użyto SQL Server, tematu, jeśli jest używany Service Bus, i subskrypcji, jeśli jest używana Redis. Każdy strumień gwarantuje uporządkowane operacje odczytu i zapisu; pojedynczy strumień to potencjalne wąskie gardło skali, dzięki czemu można zwiększyć liczbę strumieni, aby zmniejszyć wąskie gardło. Jeśli używanych jest wiele strumieni, sygnalizujący automatycznie dystrybuuje komunikaty (fragmentu) w ramach tych strumieni w taki sposób, że komunikaty wysyłane z dowolnego połączenia są w tej samej kolejności.

Ustawienie [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) określa długość kolejki wysyłania skalowania obsługiwanej przez program sygnalizujący. Ustawienie wartości większej niż 0 spowoduje umieszczenie wszystkich komunikatów w kolejce wysyłania w celu ich wysłania do skonfigurowanego planu obsługi komunikatów. Jeśli rozmiar kolejki przekracza skonfigurowaną długość, kolejne wywołania do wysłania zakończą się niepowodzeniem [, dopóki liczba](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) komunikatów w kolejce jest mniejsza niż wartość ustawienia. Kolejkowanie jest domyślnie wyłączone, ponieważ wdrożone plany są zwykle mają własne miejsce w kolejce lub sterowanie przepływem. W przypadku SQL Server, pule połączeń skutecznie ograniczają liczbę operacji wysyłania w dowolnym momencie.

Domyślnie tylko jeden strumień jest używany dla SQL Server i Redis, pięć strumieni jest używanych do Service Bus, a kolejkowanie jest wyłączone, ale te ustawienia można zmienić za pomocą konfiguracji w SQL Server i Service Bus:

**Kod serwera .NET służący do konfigurowania liczby tabel i długości kolejki dla SQL Server**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**Kod serwera .NET służący do konfigurowania liczby tematów i długości kolejki dla Service Bus**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Strumień **buforowania** to taki, który przeszedł do stanu błędu; gdy strumień jest w stanie awarii, wszystkie komunikaty wysyłane do planu awaryjnego zakończą się niepowodzeniem, dopóki strumień nie będzie już powodował błędów. **Długość kolejki wysyłania** to liczba komunikatów, które zostały ogłoszone, ale nie zostały jeszcze wysłane.

- **Odebrane komunikaty magistrali komunikatów skalowania/s**
- **Całkowita liczba strumieni skalowania**
- **Otwarte strumienie skalowania**
- **Buforowanie strumieni skalowania**
- **Łączna liczba błędów skalowania**
- **Błędy skalowania/s**
- **Długość kolejki wysyłania skalowania**

Aby uzyskać więcej informacji o tym, co to są pomiary, zobacz [sygnalizującer skalowania with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Korzystanie z innych liczników wydajności

Poniższe liczniki wydajności mogą być również przydatne podczas monitorowania wydajności aplikacji.

**Rozmiar**

- Pamięć .NET CLR\\# bajtów we wszystkich stertach (dla w3wp)

**ASP.NET**

- ASP. NET\Requests — bieżące
- ASP.NET\Queued
- ASP.NET\Rejected

**TESTY**

- Czas Information\Processor procesora

**TCP/IP**

- TCPv6/połączenia zostały ustanowione
- TCPv4/połączenia zostały ustanowione

**Usługa sieci Web**

- Połączenia sieci Web Service\Current
- Połączenia sieci Web Service\Maximum

**Wątkowość**

- Zablokowanie i wątki środowiska .NET CLR\\# bieżących wątków logicznych
- Blokady i wątki środowiska .NET CLR\\# bieżących wątków fizycznych

<a id="otherresources"></a>

## <a name="other-resources"></a>Inne zasoby

Więcej informacji na temat monitorowania i dostrajania wydajności ASP.NET można znaleźć w następujących tematach:

- [Przegląd wydajności ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Użycie wątku ASP.NET w usługach IIS 7,5, IIS 7,0 i IIS 6,0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;element&gt; applicationPool (Ustawienia sieci Web)](https://msdn.microsoft.com/library/dd560842.aspx)
