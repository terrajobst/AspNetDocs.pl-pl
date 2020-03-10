---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Włączanie śledzenia sygnałów | Microsoft Docs
author: bradygaster
description: W tym dokumencie opisano sposób włączania i konfigurowania śledzenia dla serwerów i klientów sygnalizujących. Śledzenie umożliwia wyświetlanie informacji diagnostycznych dotyczących zdarzeń...
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 34fe2cdb10c4b41a6e8cac7fb1741d53c02dfc80
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578835"
---
# <a name="enabling-signalr-tracing"></a>Włączanie śledzenia usługi SignalR

Autor [FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym dokumencie opisano sposób włączania i konfigurowania śledzenia dla serwerów i klientów sygnalizujących. Funkcja śledzenia umożliwia wyświetlanie informacji diagnostycznych dotyczących zdarzeń w aplikacji sygnalizującej.
>
> Ten temat został pierwotnie wpisany przez Fletcher Patryka.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4.5
> - Sygnalizujący wersja 2
>
>
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

Gdy śledzenie jest włączone, aplikacja sygnalizująca tworzy wpisy dziennika dla zdarzeń. Można rejestrować zdarzenia z zarówno klienta, jak i serwera programu. Śledzenie na serwerze rejestruje zdarzenia połączenia, dostawcy skalowania i magistrali komunikatów. Śledzenie na kliencie rejestruje zdarzenia połączenia. W przypadku sygnalizacji 2,1 i nowszych śledzenie klienta rejestruje pełną zawartość komunikatów wywołania centrum.

## <a name="contents"></a>Spis treści

- [Włączanie śledzenia na serwerze](#server)

    - [Rejestrowanie zdarzeń serwera w plikach tekstowych](#server_text)
    - [Rejestrowanie zdarzeń serwera w dzienniku zdarzeń](#server_eventlog)
- [Włączanie śledzenia w kliencie .NET (aplikacje klasyczne systemu Windows)](#net_client)

    - [Rejestrowanie zdarzeń klienta pulpitu w konsoli programu](#desktop_console)
    - [Rejestrowanie zdarzeń klienta pulpitu w pliku tekstowym](#desktop_text)
- [Włączanie śledzenia w przypadku klientów z Windows Phone 8](#phone)

    - [Rejestrowanie zdarzeń klienta Windows Phone w interfejsie użytkownika](#phone_ui)
    - [Rejestrowanie zdarzeń klienta Windows Phone w konsoli debugowania](#phone_debug)
- [Włączanie śledzenia w kliencie JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Włączanie śledzenia na serwerze

Śledzenie można włączyć na serwerze w pliku konfiguracji aplikacji (App. config lub Web. config w zależności od typu projektu). Należy określić kategorie zdarzeń, które mają być rejestrowane. W pliku konfiguracji należy również określić, czy zdarzenia mają być rejestrowane w pliku tekstowym, dzienniku zdarzeń systemu Windows, czy w dzienniku niestandardowym przy użyciu implementacji [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Kategorie zdarzeń serwera obejmują następujące rodzaje komunikatów:

| Element źródłowy | Komunikaty |
| --- | --- |
| SignalR.SqlMessageBus | Konfiguracja dostawcy skalowania, operacja bazy danych, błąd i zdarzenia limitu czasu dla magistrali komunikatów SQL |
| SignalR.ServiceBusMessageBus | Tworzenie tematu dostawcy skalowania i subskrypcja, błąd i obsługa komunikatów w usłudze Service Bus |
| SignalR.RedisMessageBus | Połączenie dostawcy Redis skalowania, odłączenie i zdarzenia błędów |
| SignalR.ScaleoutMessageBus | Zdarzenia skalowania Messaging |
| SignalR.Transports.WebSocketTransport | Zdarzenia połączenia transportowego protokołu WebSocket, odłączenia, obsługi komunikatów i błędów |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents połączenie transportowe, odłączenie, komunikaty i zdarzenia błędów |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame połączenie transportowe, odłączenie, komunikaty i zdarzenia błędów |
| SignalR.Transports.LongPollingTransport | LongPolling połączenie transportowe, odłączenie, komunikaty i zdarzenia błędów |
| SignalR.Transports.TransportHeartBeat | Zdarzenia połączenia transportowego, rozłączenia i utrzymywania aktywności |
| SignalR.ReflectedHubDescriptorProvider | Zdarzenia odnajdywania centrum |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Rejestrowanie zdarzeń serwera w plikach tekstowych

Poniższy kod pokazuje, jak włączyć śledzenie dla każdej kategorii zdarzenia. Ten przykład służy do konfigurowania aplikacji do rejestrowania zdarzeń w plikach tekstowych.

**Kod serwera XML do włączania śledzenia**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

W powyższym kodzie wpis `SignalRSwitch` określa kod [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) używany dla zdarzeń wysyłanych do określonego dziennika. W tym przypadku jest ustawiony na `Verbose`, co oznacza, że wszystkie komunikaty debugowania i śledzenia są rejestrowane.

Poniższe dane wyjściowe przedstawiają wpisy z pliku `transports.log.txt` aplikacji przy użyciu powyższego pliku konfiguracji. Przedstawia nowe połączenie, usunięte połączenie i zdarzenia pulsu transportu.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Rejestrowanie zdarzeń serwera w dzienniku zdarzeń

Aby rejestrować zdarzenia w dzienniku zdarzeń, a nie w pliku tekstowym, należy zmienić wartości wpisów w węźle `sharedListeners`. Poniższy kod przedstawia sposób rejestrowania zdarzeń serwera w dzienniku zdarzeń:

**Kod serwera XML do rejestrowania zdarzeń w dzienniku zdarzeń**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Zdarzenia są rejestrowane w dzienniku aplikacji i są dostępne za pomocą Podgląd zdarzeń, jak pokazano poniżej:

![Podgląd zdarzeń wyświetlania dzienników sygnalizujących](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> W przypadku korzystania z dziennika zdarzeń ustaw wartość **TraceLevel** na **błąd** , aby zachować możliwość zarządzania komunikatami.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Włączanie śledzenia w kliencie .NET (aplikacje klasyczne systemu Windows)

Klient platformy .NET może rejestrować zdarzenia do konsoli programu, pliku tekstowego lub do dziennika niestandardowego przy użyciu implementacji elementu [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Aby włączyć rejestrowanie w kliencie .NET, ustaw właściwość `TraceLevel` połączenia na wartość [tracelevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) i Właściwość `TraceWriter` na prawidłowe wystąpienie elementu [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) .

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Rejestrowanie zdarzeń klienta pulpitu w konsoli programu

Poniższy C# kod przedstawia sposób rejestrowania zdarzeń w kliencie .NET w konsoli programu:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Rejestrowanie zdarzeń klienta pulpitu w pliku tekstowym

Poniższy C# kod pokazuje, jak rejestrować zdarzenia w kliencie .NET do pliku tekstowego:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Poniższe dane wyjściowe przedstawiają wpisy z pliku `ClientLog.txt` aplikacji przy użyciu powyższego pliku konfiguracji. Pokazuje klienta łączącego się z serwerem, a centrum wywołuje metodę klienta o nazwie `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Włączanie śledzenia w przypadku klientów z Windows Phone 8

Aplikacje sygnalizujące dla aplikacji Windows Phone używają tego samego klienta platformy .NET jako aplikacji klasycznych, ale [konsola. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) i zapisywanie do pliku z [StreamWriter —](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nie są dostępne. Zamiast tego należy utworzyć niestandardową implementację elementu [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) do śledzenia.

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Rejestrowanie zdarzeń klienta Windows Phone w interfejsie użytkownika

[Baza kodu sygnalizująca](https://github.com/SignalR/SignalR/archive/master.zip) zawiera przykład Windows Phone, który zapisuje dane wyjściowe śledzenia do [bloku](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) text, przy użyciu niestandardowej implementacji [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) o nazwie `TextBlockWriter`. Tę klasę można znaleźć w projekcie **Samples/Microsoft. ASPNET. Signal. Client. WP8. Samples** . Podczas tworzenia wystąpienia `TextBlockWriter`należy przekazać bieżącą [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)i [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) , gdzie zostanie utworzony [blok TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) do użycia dla danych wyjściowych śledzenia:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Dane wyjściowe śledzenia będą następnie zapisywane w nowym [bloku TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) utworzonym w [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) :

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Rejestrowanie zdarzeń klienta Windows Phone w konsoli debugowania

Aby wysłać dane wyjściowe do konsoli debugowania, a nie interfejsu użytkownika, należy utworzyć implementację [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) , która zapisuje w oknie debugowania, i przypisać ją do właściwości [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) połączenia:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Informacje o śledzeniu zostaną następnie zapisywane w oknie debugowania w programie Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Włączanie śledzenia w kliencie JavaScript

Aby włączyć rejestrowanie po stronie klienta dla połączenia, należy ustawić właściwość `logging` obiektu połączenia przed wywołaniem metody `start` w celu nawiązania połączenia.

**Kod JavaScript klienta umożliwiający włączenie śledzenia do konsoli przeglądarki (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Kod JavaScript klienta umożliwiający włączenie śledzenia do konsoli przeglądarki (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Po włączeniu śledzenia klient JavaScript rejestruje zdarzenia w konsoli przeglądarki. Aby uzyskać dostęp do konsoli przeglądarki, zobacz [monitorowanie Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Poniższy zrzut ekranu pokazuje klienta JavaScript sygnalizującego z włączonym śledzeniem. Przedstawia zdarzenia połączeń i wywołań centrów w konsoli przeglądarki:

![Zdarzenia śledzenia sygnałów w konsoli przeglądarki](enabling-signalr-tracing/_static/image4.png)
