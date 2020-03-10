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
# <a name="enabling-signalr-tracing"></a><span data-ttu-id="068a7-104">Włączanie śledzenia usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="068a7-104">Enabling SignalR Tracing</span></span>

<span data-ttu-id="068a7-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="068a7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="068a7-106">W tym dokumencie opisano sposób włączania i konfigurowania śledzenia dla serwerów i klientów sygnalizujących.</span><span class="sxs-lookup"><span data-stu-id="068a7-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="068a7-107">Funkcja śledzenia umożliwia wyświetlanie informacji diagnostycznych dotyczących zdarzeń w aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="068a7-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="068a7-108">Ten temat został pierwotnie wpisany przez Fletcher Patryka.</span><span class="sxs-lookup"><span data-stu-id="068a7-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="068a7-109">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="068a7-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="068a7-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="068a7-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="068a7-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="068a7-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="068a7-112">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="068a7-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="068a7-113">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="068a7-113">Questions and comments</span></span>
>
> <span data-ttu-id="068a7-114">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="068a7-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="068a7-115">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="068a7-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="068a7-116">Gdy śledzenie jest włączone, aplikacja sygnalizująca tworzy wpisy dziennika dla zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="068a7-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="068a7-117">Można rejestrować zdarzenia z zarówno klienta, jak i serwera programu.</span><span class="sxs-lookup"><span data-stu-id="068a7-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="068a7-118">Śledzenie na serwerze rejestruje zdarzenia połączenia, dostawcy skalowania i magistrali komunikatów.</span><span class="sxs-lookup"><span data-stu-id="068a7-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="068a7-119">Śledzenie na kliencie rejestruje zdarzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="068a7-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="068a7-120">W przypadku sygnalizacji 2,1 i nowszych śledzenie klienta rejestruje pełną zawartość komunikatów wywołania centrum.</span><span class="sxs-lookup"><span data-stu-id="068a7-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="068a7-121">Spis treści</span><span class="sxs-lookup"><span data-stu-id="068a7-121">Contents</span></span>

- [<span data-ttu-id="068a7-122">Włączanie śledzenia na serwerze</span><span class="sxs-lookup"><span data-stu-id="068a7-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="068a7-123">Rejestrowanie zdarzeń serwera w plikach tekstowych</span><span class="sxs-lookup"><span data-stu-id="068a7-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="068a7-124">Rejestrowanie zdarzeń serwera w dzienniku zdarzeń</span><span class="sxs-lookup"><span data-stu-id="068a7-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="068a7-125">Włączanie śledzenia w kliencie .NET (aplikacje klasyczne systemu Windows)</span><span class="sxs-lookup"><span data-stu-id="068a7-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="068a7-126">Rejestrowanie zdarzeń klienta pulpitu w konsoli programu</span><span class="sxs-lookup"><span data-stu-id="068a7-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="068a7-127">Rejestrowanie zdarzeń klienta pulpitu w pliku tekstowym</span><span class="sxs-lookup"><span data-stu-id="068a7-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="068a7-128">Włączanie śledzenia w przypadku klientów z Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="068a7-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="068a7-129">Rejestrowanie zdarzeń klienta Windows Phone w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="068a7-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="068a7-130">Rejestrowanie zdarzeń klienta Windows Phone w konsoli debugowania</span><span class="sxs-lookup"><span data-stu-id="068a7-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="068a7-131">Włączanie śledzenia w kliencie JavaScript</span><span class="sxs-lookup"><span data-stu-id="068a7-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="068a7-132">Włączanie śledzenia na serwerze</span><span class="sxs-lookup"><span data-stu-id="068a7-132">Enabling tracing on the server</span></span>

<span data-ttu-id="068a7-133">Śledzenie można włączyć na serwerze w pliku konfiguracji aplikacji (App. config lub Web. config w zależności od typu projektu). Należy określić kategorie zdarzeń, które mają być rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="068a7-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="068a7-134">W pliku konfiguracji należy również określić, czy zdarzenia mają być rejestrowane w pliku tekstowym, dzienniku zdarzeń systemu Windows, czy w dzienniku niestandardowym przy użyciu implementacji [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="068a7-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="068a7-135">Kategorie zdarzeń serwera obejmują następujące rodzaje komunikatów:</span><span class="sxs-lookup"><span data-stu-id="068a7-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="068a7-136">Element źródłowy</span><span class="sxs-lookup"><span data-stu-id="068a7-136">Source</span></span> | <span data-ttu-id="068a7-137">Komunikaty</span><span class="sxs-lookup"><span data-stu-id="068a7-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="068a7-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="068a7-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="068a7-139">Konfiguracja dostawcy skalowania, operacja bazy danych, błąd i zdarzenia limitu czasu dla magistrali komunikatów SQL</span><span class="sxs-lookup"><span data-stu-id="068a7-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="068a7-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="068a7-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="068a7-141">Tworzenie tematu dostawcy skalowania i subskrypcja, błąd i obsługa komunikatów w usłudze Service Bus</span><span class="sxs-lookup"><span data-stu-id="068a7-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="068a7-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="068a7-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="068a7-143">Połączenie dostawcy Redis skalowania, odłączenie i zdarzenia błędów</span><span class="sxs-lookup"><span data-stu-id="068a7-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="068a7-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="068a7-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="068a7-145">Zdarzenia skalowania Messaging</span><span class="sxs-lookup"><span data-stu-id="068a7-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="068a7-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="068a7-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="068a7-147">Zdarzenia połączenia transportowego protokołu WebSocket, odłączenia, obsługi komunikatów i błędów</span><span class="sxs-lookup"><span data-stu-id="068a7-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="068a7-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="068a7-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="068a7-149">ServerSentEvents połączenie transportowe, odłączenie, komunikaty i zdarzenia błędów</span><span class="sxs-lookup"><span data-stu-id="068a7-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="068a7-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="068a7-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="068a7-151">ForeverFrame połączenie transportowe, odłączenie, komunikaty i zdarzenia błędów</span><span class="sxs-lookup"><span data-stu-id="068a7-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="068a7-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="068a7-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="068a7-153">LongPolling połączenie transportowe, odłączenie, komunikaty i zdarzenia błędów</span><span class="sxs-lookup"><span data-stu-id="068a7-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="068a7-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="068a7-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="068a7-155">Zdarzenia połączenia transportowego, rozłączenia i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="068a7-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="068a7-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="068a7-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="068a7-157">Zdarzenia odnajdywania centrum</span><span class="sxs-lookup"><span data-stu-id="068a7-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="068a7-158">Rejestrowanie zdarzeń serwera w plikach tekstowych</span><span class="sxs-lookup"><span data-stu-id="068a7-158">Logging server events to text files</span></span>

<span data-ttu-id="068a7-159">Poniższy kod pokazuje, jak włączyć śledzenie dla każdej kategorii zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="068a7-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="068a7-160">Ten przykład służy do konfigurowania aplikacji do rejestrowania zdarzeń w plikach tekstowych.</span><span class="sxs-lookup"><span data-stu-id="068a7-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="068a7-161">**Kod serwera XML do włączania śledzenia**</span><span class="sxs-lookup"><span data-stu-id="068a7-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="068a7-162">W powyższym kodzie wpis `SignalRSwitch` określa kod [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) używany dla zdarzeń wysyłanych do określonego dziennika.</span><span class="sxs-lookup"><span data-stu-id="068a7-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="068a7-163">W tym przypadku jest ustawiony na `Verbose`, co oznacza, że wszystkie komunikaty debugowania i śledzenia są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="068a7-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="068a7-164">Poniższe dane wyjściowe przedstawiają wpisy z pliku `transports.log.txt` aplikacji przy użyciu powyższego pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="068a7-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="068a7-165">Przedstawia nowe połączenie, usunięte połączenie i zdarzenia pulsu transportu.</span><span class="sxs-lookup"><span data-stu-id="068a7-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="068a7-166">Rejestrowanie zdarzeń serwera w dzienniku zdarzeń</span><span class="sxs-lookup"><span data-stu-id="068a7-166">Logging server events to the event log</span></span>

<span data-ttu-id="068a7-167">Aby rejestrować zdarzenia w dzienniku zdarzeń, a nie w pliku tekstowym, należy zmienić wartości wpisów w węźle `sharedListeners`.</span><span class="sxs-lookup"><span data-stu-id="068a7-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="068a7-168">Poniższy kod przedstawia sposób rejestrowania zdarzeń serwera w dzienniku zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="068a7-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="068a7-169">**Kod serwera XML do rejestrowania zdarzeń w dzienniku zdarzeń**</span><span class="sxs-lookup"><span data-stu-id="068a7-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="068a7-170">Zdarzenia są rejestrowane w dzienniku aplikacji i są dostępne za pomocą Podgląd zdarzeń, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="068a7-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Podgląd zdarzeń wyświetlania dzienników sygnalizujących](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="068a7-172">W przypadku korzystania z dziennika zdarzeń ustaw wartość **TraceLevel** na **błąd** , aby zachować możliwość zarządzania komunikatami.</span><span class="sxs-lookup"><span data-stu-id="068a7-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="068a7-173">Włączanie śledzenia w kliencie .NET (aplikacje klasyczne systemu Windows)</span><span class="sxs-lookup"><span data-stu-id="068a7-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="068a7-174">Klient platformy .NET może rejestrować zdarzenia do konsoli programu, pliku tekstowego lub do dziennika niestandardowego przy użyciu implementacji elementu [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="068a7-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="068a7-175">Aby włączyć rejestrowanie w kliencie .NET, ustaw właściwość `TraceLevel` połączenia na wartość [tracelevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) i Właściwość `TraceWriter` na prawidłowe wystąpienie elementu [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) .</span><span class="sxs-lookup"><span data-stu-id="068a7-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="068a7-176">Rejestrowanie zdarzeń klienta pulpitu w konsoli programu</span><span class="sxs-lookup"><span data-stu-id="068a7-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="068a7-177">Poniższy C# kod przedstawia sposób rejestrowania zdarzeń w kliencie .NET w konsoli programu:</span><span class="sxs-lookup"><span data-stu-id="068a7-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="068a7-178">Rejestrowanie zdarzeń klienta pulpitu w pliku tekstowym</span><span class="sxs-lookup"><span data-stu-id="068a7-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="068a7-179">Poniższy C# kod pokazuje, jak rejestrować zdarzenia w kliencie .NET do pliku tekstowego:</span><span class="sxs-lookup"><span data-stu-id="068a7-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="068a7-180">Poniższe dane wyjściowe przedstawiają wpisy z pliku `ClientLog.txt` aplikacji przy użyciu powyższego pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="068a7-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="068a7-181">Pokazuje klienta łączącego się z serwerem, a centrum wywołuje metodę klienta o nazwie `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="068a7-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="068a7-182">Włączanie śledzenia w przypadku klientów z Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="068a7-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="068a7-183">Aplikacje sygnalizujące dla aplikacji Windows Phone używają tego samego klienta platformy .NET jako aplikacji klasycznych, ale [konsola. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) i zapisywanie do pliku z [StreamWriter —](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="068a7-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="068a7-184">Zamiast tego należy utworzyć niestandardową implementację elementu [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) do śledzenia.</span><span class="sxs-lookup"><span data-stu-id="068a7-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="068a7-185">Rejestrowanie zdarzeń klienta Windows Phone w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="068a7-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="068a7-186">[Baza kodu sygnalizująca](https://github.com/SignalR/SignalR/archive/master.zip) zawiera przykład Windows Phone, który zapisuje dane wyjściowe śledzenia do [bloku](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) text, przy użyciu niestandardowej implementacji [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) o nazwie `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="068a7-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="068a7-187">Tę klasę można znaleźć w projekcie **Samples/Microsoft. ASPNET. Signal. Client. WP8. Samples** .</span><span class="sxs-lookup"><span data-stu-id="068a7-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="068a7-188">Podczas tworzenia wystąpienia `TextBlockWriter`należy przekazać bieżącą [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)i [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) , gdzie zostanie utworzony [blok TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) do użycia dla danych wyjściowych śledzenia:</span><span class="sxs-lookup"><span data-stu-id="068a7-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="068a7-189">Dane wyjściowe śledzenia będą następnie zapisywane w nowym [bloku TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) utworzonym w [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) :</span><span class="sxs-lookup"><span data-stu-id="068a7-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="068a7-190">Rejestrowanie zdarzeń klienta Windows Phone w konsoli debugowania</span><span class="sxs-lookup"><span data-stu-id="068a7-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="068a7-191">Aby wysłać dane wyjściowe do konsoli debugowania, a nie interfejsu użytkownika, należy utworzyć implementację [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) , która zapisuje w oknie debugowania, i przypisać ją do właściwości [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) połączenia:</span><span class="sxs-lookup"><span data-stu-id="068a7-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="068a7-192">Informacje o śledzeniu zostaną następnie zapisywane w oknie debugowania w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="068a7-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="068a7-193">Włączanie śledzenia w kliencie JavaScript</span><span class="sxs-lookup"><span data-stu-id="068a7-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="068a7-194">Aby włączyć rejestrowanie po stronie klienta dla połączenia, należy ustawić właściwość `logging` obiektu połączenia przed wywołaniem metody `start` w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="068a7-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="068a7-195">**Kod JavaScript klienta umożliwiający włączenie śledzenia do konsoli przeglądarki (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="068a7-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="068a7-196">**Kod JavaScript klienta umożliwiający włączenie śledzenia do konsoli przeglądarki (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="068a7-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="068a7-197">Po włączeniu śledzenia klient JavaScript rejestruje zdarzenia w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="068a7-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="068a7-198">Aby uzyskać dostęp do konsoli przeglądarki, zobacz [monitorowanie Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="068a7-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="068a7-199">Poniższy zrzut ekranu pokazuje klienta JavaScript sygnalizującego z włączonym śledzeniem.</span><span class="sxs-lookup"><span data-stu-id="068a7-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="068a7-200">Przedstawia zdarzenia połączeń i wywołań centrów w konsoli przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="068a7-200">It shows connection and hub invocation events in the browser console:</span></span>

![Zdarzenia śledzenia sygnałów w konsoli przeglądarki](enabling-signalr-tracing/_static/image4.png)
