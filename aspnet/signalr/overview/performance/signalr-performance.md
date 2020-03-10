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
# <a name="signalr-performance"></a><span data-ttu-id="bccfa-103">Wydajność usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="bccfa-103">SignalR Performance</span></span>

<span data-ttu-id="bccfa-104">[Fletcher Patryka](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="bccfa-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="bccfa-105">W tym temacie opisano sposób projektowania, mierzenia i poprawiania wydajności w aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="bccfa-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="bccfa-106">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="bccfa-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="bccfa-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bccfa-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="bccfa-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="bccfa-108">.NET 4.5</span></span>
> - <span data-ttu-id="bccfa-109">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="bccfa-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="bccfa-110">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="bccfa-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="bccfa-111">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="bccfa-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="bccfa-112">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="bccfa-112">Questions and comments</span></span>
>
> <span data-ttu-id="bccfa-113">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="bccfa-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="bccfa-114">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="bccfa-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="bccfa-115">Aby uzyskać ostatnią prezentację na temat wydajności i skalowania sygnału, zobacz [skalowanie sieci Web w czasie rzeczywistym za pomocą sygnału ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="bccfa-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="bccfa-116">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="bccfa-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="bccfa-117">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="bccfa-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="bccfa-118">Dostrajanie serwera sygnalizującego pod kątem wydajności</span><span class="sxs-lookup"><span data-stu-id="bccfa-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="bccfa-119">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="bccfa-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="bccfa-120">Korzystanie z liczników wydajności sygnałów</span><span class="sxs-lookup"><span data-stu-id="bccfa-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="bccfa-121">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="bccfa-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="bccfa-122">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="bccfa-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="bccfa-123">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="bccfa-123">Design considerations</span></span>

<span data-ttu-id="bccfa-124">W tej sekcji opisano wzorce, które mogą być implementowane podczas projektowania aplikacji sygnalizującej, aby zapewnić, że wydajność nie jest zakłócana przez wygenerowanie niepotrzebnego ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="bccfa-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="bccfa-125">Częstotliwość komunikatów ograniczania przepustowości</span><span class="sxs-lookup"><span data-stu-id="bccfa-125">Throttling message frequency</span></span>

<span data-ttu-id="bccfa-126">Nawet w aplikacji, która wysyła komunikaty z dużą częstotliwością (na przykład aplikacja do gier w czasie rzeczywistym), większość aplikacji nie musi wysyłać więcej niż kilka komunikatów na sekundę.</span><span class="sxs-lookup"><span data-stu-id="bccfa-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="bccfa-127">Aby zmniejszyć ilość ruchu wygenerowanego przez każdego klienta, można zaimplementować pętlę komunikatów, która nie jest częściej niż stała stawka (oznacza to, że do określonej liczby komunikatów będą wysyłane co sekundę, jeśli w tym czasie znajdują się komunikaty terval do wysłania).</span><span class="sxs-lookup"><span data-stu-id="bccfa-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="bccfa-128">W przypadku przykładowej aplikacji, która ogranicza komunikaty do określonej stawki (zarówno z klienta i serwera), zobacz czas [rzeczywisty o wysokiej częstotliwości z sygnalizującym](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="bccfa-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="bccfa-129">Zmniejszanie rozmiaru wiadomości</span><span class="sxs-lookup"><span data-stu-id="bccfa-129">Reducing message size</span></span>

<span data-ttu-id="bccfa-130">Możesz zmniejszyć rozmiar komunikatu sygnalizującego, zmniejszając rozmiar serializowanych obiektów.</span><span class="sxs-lookup"><span data-stu-id="bccfa-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="bccfa-131">W przypadku wysyłania obiektu, który zawiera właściwości, które nie muszą być przesyłane, w kodzie serwera, należy zapobiec serializacji tych właściwości przy użyciu atrybutu `JsonIgnore`.</span><span class="sxs-lookup"><span data-stu-id="bccfa-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="bccfa-132">Nazwy właściwości są również przechowywane w komunikacie. nazwy właściwości można skrócić przy użyciu atrybutu `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="bccfa-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="bccfa-133">Poniższy przykład kodu demonstruje, jak wykluczyć właściwość z wysyłania do klienta i jak skrócić nazwy właściwości:</span><span class="sxs-lookup"><span data-stu-id="bccfa-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="bccfa-134">**Kod serwera .NET, który demonstruje atrybut JsonIgnore, aby wykluczyć dane wysyłane do klienta i atrybut JsonProperty w celu zmniejszenia rozmiaru komunikatu**</span><span class="sxs-lookup"><span data-stu-id="bccfa-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="bccfa-135">Aby zachować czytelność/łatwość utrzymania w kodzie klienta, skrócone nazwy właściwości mogą być ponownie mapowane na przyjazne nazwy po odebraniu wiadomości.</span><span class="sxs-lookup"><span data-stu-id="bccfa-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="bccfa-136">Poniższy przykład kodu ilustruje jeden z możliwych sposobów ponownego mapowania skróconych nazw do większej, przez zdefiniowanie kontraktu komunikatu (mapowanie) i użycie funkcji `reMap` do zastosowania kontraktu do klasy zoptymalizowanego komunikatu:</span><span class="sxs-lookup"><span data-stu-id="bccfa-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="bccfa-137">**Kod JavaScript po stronie klienta, który ponownie mapuje skrócone nazwy właściwości na nazwy czytelne dla człowieka**</span><span class="sxs-lookup"><span data-stu-id="bccfa-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="bccfa-138">Nazwy można skrócić również w komunikatach z klienta do serwera, używając tej samej metody.</span><span class="sxs-lookup"><span data-stu-id="bccfa-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="bccfa-139">Zmniejszenie ilości pamięci (czyli ilości pamięci używanej przez komunikat) obiektu komunikatu może również zwiększyć wydajność.</span><span class="sxs-lookup"><span data-stu-id="bccfa-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="bccfa-140">Na przykład jeśli pełny zakres `int` nie jest wymagany, zamiast tego można użyć `short` lub `byte`.</span><span class="sxs-lookup"><span data-stu-id="bccfa-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="bccfa-141">Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie rozmiaru komunikatów może również rozwiązać problemy z pamięcią serwera.</span><span class="sxs-lookup"><span data-stu-id="bccfa-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="bccfa-142">Dostrajanie serwera sygnalizującego pod kątem wydajności</span><span class="sxs-lookup"><span data-stu-id="bccfa-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="bccfa-143">Poniższe ustawienia konfiguracji mogą służyć do dostrajania serwera w celu zapewnienia lepszej wydajności w aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="bccfa-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="bccfa-144">Aby uzyskać ogólne informacje na temat ulepszania wydajności w aplikacji ASP.NET, zobacz [Poprawianie wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="bccfa-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="bccfa-145">**Ustawienia konfiguracji sygnalizującego**</span><span class="sxs-lookup"><span data-stu-id="bccfa-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="bccfa-146">**DefaultMessageBufferSize**: domyślnie sygnalizujący przechowuje 1000 komunikatów w pamięci na koncentratorze dla każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="bccfa-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="bccfa-147">W przypadku korzystania z dużych komunikatów może to spowodować problemy z pamięcią, które można wyeliminować, zmniejszając tę wartość.</span><span class="sxs-lookup"><span data-stu-id="bccfa-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="bccfa-148">To ustawienie można ustawić w obsłudze zdarzeń `Application_Start` w aplikacji ASP.NET lub w metodzie `Configuration` klasy uruchomieniowej OWIN w aplikacji samohostowanej.</span><span class="sxs-lookup"><span data-stu-id="bccfa-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="bccfa-149">Poniższy przykład demonstruje, jak zmniejszyć tę wartość, aby zmniejszyć rozmiar pamięci aplikacji w celu zmniejszenia ilości używanej pamięci serwera:</span><span class="sxs-lookup"><span data-stu-id="bccfa-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="bccfa-150">**Kod programu .NET Server w Startup.cs na potrzeby zmniejszania domyślnego rozmiaru buforu komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="bccfa-151">**Ustawienia konfiguracji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="bccfa-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="bccfa-152">**Maksymalna liczba współbieżnych żądań na aplikację**: zwiększenie liczby równoczesnych żądań usług IIS spowoduje zwiększenie zasobów serwera dostępnych do obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="bccfa-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="bccfa-153">Wartość domyślna to 5000; Aby zwiększyć to ustawienie, wykonaj następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień:</span><span class="sxs-lookup"><span data-stu-id="bccfa-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="bccfa-154">**ApplicationPool queuelength**: jest to maksymalna liczba żądań, które są kolejki http. sys dla puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bccfa-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="bccfa-155">Gdy kolejka jest pełna, nowe żądania otrzymują odpowiedź "Usługa niedostępna" w usłudze 503.</span><span class="sxs-lookup"><span data-stu-id="bccfa-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="bccfa-156">Wartość domyślna to 1000.</span><span class="sxs-lookup"><span data-stu-id="bccfa-156">The default value is 1000.</span></span>

    <span data-ttu-id="bccfa-157">Skrócenie długości kolejki procesu roboczego w puli aplikacji obsługującej aplikację spowoduje zapełnienie zasobów pamięci.</span><span class="sxs-lookup"><span data-stu-id="bccfa-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="bccfa-158">Aby uzyskać więcej informacji, zobacz [Zarządzanie, dostrajanie i Konfigurowanie pul aplikacji](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="bccfa-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="bccfa-159">**Ustawienia konfiguracji ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="bccfa-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="bccfa-160">Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w pliku `aspnet.config`.</span><span class="sxs-lookup"><span data-stu-id="bccfa-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="bccfa-161">Ten plik znajduje się w jednej z dwóch lokalizacji, w zależności od platformy:</span><span class="sxs-lookup"><span data-stu-id="bccfa-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="bccfa-162">Ustawienia ASP.NET, które mogą poprawić wydajność sygnalizującego, obejmują następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="bccfa-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="bccfa-163">**Maksymalna liczba współbieżnych żądań na procesor CPU**: zwiększenie tego ustawienia może złagodzić wąskie gardła wydajności.</span><span class="sxs-lookup"><span data-stu-id="bccfa-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="bccfa-164">Aby zwiększyć to ustawienie, należy dodać następujące ustawienia konfiguracji do pliku `aspnet.config`:</span><span class="sxs-lookup"><span data-stu-id="bccfa-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="bccfa-165">**Limit kolejki żądań**: gdy całkowita liczba połączeń przekroczy ustawienie `maxConcurrentRequestsPerCPU`, ASP.NET rozpocznie ograniczanie żądań przy użyciu kolejki.</span><span class="sxs-lookup"><span data-stu-id="bccfa-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="bccfa-166">Aby zwiększyć rozmiar kolejki, można zwiększyć ustawienie `requestQueueLimit`.</span><span class="sxs-lookup"><span data-stu-id="bccfa-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="bccfa-167">W tym celu należy dodać następujące ustawienia konfiguracji do węzła `processModel` w `config/machine.config` (zamiast `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="bccfa-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="bccfa-168">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="bccfa-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="bccfa-169">W tej sekcji opisano sposoby znajdowania wąskich gardeł wydajności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bccfa-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="bccfa-170">Weryfikowanie, czy protokół WebSocket jest używany</span><span class="sxs-lookup"><span data-stu-id="bccfa-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="bccfa-171">Chociaż sygnalizujący może używać różnych transportów komunikacji między klientem i serwerem, protokół WebSocket oferuje znaczną wydajność i należy go używać, jeśli klient i serwer obsługują tę opcję.</span><span class="sxs-lookup"><span data-stu-id="bccfa-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="bccfa-172">Aby określić, czy klient i serwer spełniają wymagania dotyczące protokołu WebSocket, zobacz [Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="bccfa-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="bccfa-173">Aby określić, który transport jest używany w aplikacji, możesz użyć narzędzi deweloperskich przeglądarki i sprawdzić dzienniki, aby zobaczyć, jaki transport jest używany do połączenia.</span><span class="sxs-lookup"><span data-stu-id="bccfa-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="bccfa-174">Aby uzyskać informacje na temat korzystania z narzędzi programistycznych przeglądarki w programie Internet Explorer i programie Chrome, zobacz [Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="bccfa-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="bccfa-175">Korzystanie z liczników wydajności sygnałów</span><span class="sxs-lookup"><span data-stu-id="bccfa-175">Using SignalR performance counters</span></span>

<span data-ttu-id="bccfa-176">W tej sekcji opisano sposób włączania i używania liczników wydajności sygnałów, które znajdują się w pakiecie `Microsoft.AspNet.SignalR.Utils`.</span><span class="sxs-lookup"><span data-stu-id="bccfa-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="bccfa-177">Instalowanie sygnalizującer. exe</span><span class="sxs-lookup"><span data-stu-id="bccfa-177">Installing signalr.exe</span></span>

<span data-ttu-id="bccfa-178">Liczniki wydajności można dodać do serwera za pomocą narzędzia o nazwie Sygnalizującer. exe.</span><span class="sxs-lookup"><span data-stu-id="bccfa-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="bccfa-179">Aby zainstalować to narzędzie, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="bccfa-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="bccfa-180">W programie Visual Studio wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **Zarządzanie pakietami NuGet dla rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="bccfa-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="bccfa-181">Wyszukaj pozycję **sygnalizujący.** narzędzia i wybierz pozycję Zainstaluj.</span><span class="sxs-lookup"><span data-stu-id="bccfa-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="bccfa-182">Zaakceptuj umowę licencyjną, aby zainstalować pakiet.</span><span class="sxs-lookup"><span data-stu-id="bccfa-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="bccfa-183">Program sygnalizujący. exe zostanie zainstalowany do `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="bccfa-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="bccfa-184">Instalowanie liczników wydajności za pomocą programu Signaler. exe</span><span class="sxs-lookup"><span data-stu-id="bccfa-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="bccfa-185">Aby zainstalować liczniki wydajności sygnałów, uruchom program Sygnalizującer. exe w wierszu polecenia z podwyższonym poziomem uprawnień z następującym parametrem:</span><span class="sxs-lookup"><span data-stu-id="bccfa-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="bccfa-186">Aby usunąć liczniki wydajności sygnałów, uruchom program Signaler. exe w wierszu polecenia z podwyższonym poziomem uprawnień z następującym parametrem:</span><span class="sxs-lookup"><span data-stu-id="bccfa-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="bccfa-187">Liczniki wydajności sygnałów</span><span class="sxs-lookup"><span data-stu-id="bccfa-187">SignalR Performance counters</span></span>

<span data-ttu-id="bccfa-188">Pakiet narzędzi instaluje następujące liczniki wydajności.</span><span class="sxs-lookup"><span data-stu-id="bccfa-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="bccfa-189">Liczniki "Total" mierzą liczbę zdarzeń od momentu ostatniego ponownego uruchomienia serwera.</span><span class="sxs-lookup"><span data-stu-id="bccfa-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="bccfa-190">**Metryki połączeń**</span><span class="sxs-lookup"><span data-stu-id="bccfa-190">**Connection metrics**</span></span>

<span data-ttu-id="bccfa-191">Następujące metryki mierzą zdarzenia okresu istnienia połączenia, które wystąpiły.</span><span class="sxs-lookup"><span data-stu-id="bccfa-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="bccfa-192">Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="bccfa-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="bccfa-193">**Połączenia połączone**</span><span class="sxs-lookup"><span data-stu-id="bccfa-193">**Connections Connected**</span></span>
- <span data-ttu-id="bccfa-194">**Połączenia zostały ponownie połączone**</span><span class="sxs-lookup"><span data-stu-id="bccfa-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="bccfa-195">**Połączenia rozłączone**</span><span class="sxs-lookup"><span data-stu-id="bccfa-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="bccfa-196">**Bieżące połączenia**</span><span class="sxs-lookup"><span data-stu-id="bccfa-196">**Connections Current**</span></span>

<span data-ttu-id="bccfa-197">**Metryki komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-197">**Message metrics**</span></span>

<span data-ttu-id="bccfa-198">Następujące metryki mierzą ruch komunikatów generowany przez program sygnalizujący.</span><span class="sxs-lookup"><span data-stu-id="bccfa-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="bccfa-199">**Całkowita liczba odebranych komunikatów połączenia**</span><span class="sxs-lookup"><span data-stu-id="bccfa-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="bccfa-200">**Całkowita liczba wysłanych komunikatów połączenia**</span><span class="sxs-lookup"><span data-stu-id="bccfa-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="bccfa-201">**Odebrane komunikaty połączenia/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="bccfa-202">**Wysłane komunikaty połączenia/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="bccfa-203">**Metryki magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-203">**Message bus metrics**</span></span>

<span data-ttu-id="bccfa-204">Następujące metryki mierzą ruch przez magistralę komunikatów wewnętrznych sygnalizujących, kolejkę, w której są umieszczane wszystkie komunikaty sygnalizujące przychodzące i wychodzące.</span><span class="sxs-lookup"><span data-stu-id="bccfa-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="bccfa-205">Komunikat jest **publikowany** , gdy jest wysyłany lub emitowany.</span><span class="sxs-lookup"><span data-stu-id="bccfa-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="bccfa-206">**Subskrybent** w tym kontekście jest subskrypcją magistrali komunikatów; Ta wartość powinna być równa liczbie klientów i serwera.</span><span class="sxs-lookup"><span data-stu-id="bccfa-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="bccfa-207">**Przydzielony proces roboczy** to składnik, który wysyła dane do aktywnych połączeń; **zajęty proces roboczy** to ten, który aktywnie wysyła komunikat.</span><span class="sxs-lookup"><span data-stu-id="bccfa-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="bccfa-208">**Całkowita liczba odebranych komunikatów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="bccfa-209">**Odebrane komunikaty magistrali komunikatów/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="bccfa-210">**Łączna liczba opublikowanych komunikatów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="bccfa-211">**Opublikowane komunikaty magistrali komunikatów/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="bccfa-212">**Aktualni Subskrybenci magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="bccfa-213">**Łączna liczba subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="bccfa-214">**Subskrybenci magistrali komunikatów/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="bccfa-215">**Pracownicy przydzieleni do magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="bccfa-216">**Pracownikom zajętym przez magistralę komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="bccfa-217">**Bieżące tematy dotyczące magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="bccfa-218">**Metryki błędów**</span><span class="sxs-lookup"><span data-stu-id="bccfa-218">**Error metrics**</span></span>

<span data-ttu-id="bccfa-219">Następujące metryki mierzą błędy generowane przez ruch komunikatów sygnalizujących.</span><span class="sxs-lookup"><span data-stu-id="bccfa-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="bccfa-220">Błędy **rozpoznawania centrów** występują, gdy nie można rozpoznać metody Hub lub Hub.</span><span class="sxs-lookup"><span data-stu-id="bccfa-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="bccfa-221">Błędy **wywołania centrum** to wyjątki zgłoszone podczas wywoływania metody centrum.</span><span class="sxs-lookup"><span data-stu-id="bccfa-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="bccfa-222">Błędy **transportu** to błędy połączeń zgłoszone podczas żądania HTTP lub odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bccfa-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="bccfa-223">**Błędy: wszystkie łącznie**</span><span class="sxs-lookup"><span data-stu-id="bccfa-223">**Errors: All Total**</span></span>
- <span data-ttu-id="bccfa-224">**Błędy: wszystkie/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="bccfa-225">**Błędy: rozdzielczość centrum łącznie**</span><span class="sxs-lookup"><span data-stu-id="bccfa-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="bccfa-226">**Błędy: rozdzielczość centrum/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="bccfa-227">**Błędy: całkowita liczba wywołań koncentratora**</span><span class="sxs-lookup"><span data-stu-id="bccfa-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="bccfa-228">**Błędy: wywołanie koncentratora/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="bccfa-229">**Błędy: łącznie z transportem**</span><span class="sxs-lookup"><span data-stu-id="bccfa-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="bccfa-230">**Błędy: transport/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="bccfa-231">**Metryki skalowania**</span><span class="sxs-lookup"><span data-stu-id="bccfa-231">**Scaleout metrics**</span></span>

<span data-ttu-id="bccfa-232">Następujące metryki mierzą ruch i błędy generowane przez dostawcę skalowania.</span><span class="sxs-lookup"><span data-stu-id="bccfa-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="bccfa-233">**Strumień** w tym kontekście jest jednostką skalowania używaną przez dostawcę skalowania. jest to tabela, jeśli użyto SQL Server, tematu, jeśli jest używany Service Bus, i subskrypcji, jeśli jest używana Redis.</span><span class="sxs-lookup"><span data-stu-id="bccfa-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="bccfa-234">Każdy strumień gwarantuje uporządkowane operacje odczytu i zapisu; pojedynczy strumień to potencjalne wąskie gardło skali, dzięki czemu można zwiększyć liczbę strumieni, aby zmniejszyć wąskie gardło.</span><span class="sxs-lookup"><span data-stu-id="bccfa-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="bccfa-235">Jeśli używanych jest wiele strumieni, sygnalizujący automatycznie dystrybuuje komunikaty (fragmentu) w ramach tych strumieni w taki sposób, że komunikaty wysyłane z dowolnego połączenia są w tej samej kolejności.</span><span class="sxs-lookup"><span data-stu-id="bccfa-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="bccfa-236">Ustawienie [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) określa długość kolejki wysyłania skalowania obsługiwanej przez program sygnalizujący.</span><span class="sxs-lookup"><span data-stu-id="bccfa-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="bccfa-237">Ustawienie wartości większej niż 0 spowoduje umieszczenie wszystkich komunikatów w kolejce wysyłania w celu ich wysłania do skonfigurowanego planu obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="bccfa-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="bccfa-238">Jeśli rozmiar kolejki przekracza skonfigurowaną długość, kolejne wywołania do wysłania zakończą się niepowodzeniem [, dopóki liczba](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) komunikatów w kolejce jest mniejsza niż wartość ustawienia.</span><span class="sxs-lookup"><span data-stu-id="bccfa-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="bccfa-239">Kolejkowanie jest domyślnie wyłączone, ponieważ wdrożone plany są zwykle mają własne miejsce w kolejce lub sterowanie przepływem.</span><span class="sxs-lookup"><span data-stu-id="bccfa-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="bccfa-240">W przypadku SQL Server, pule połączeń skutecznie ograniczają liczbę operacji wysyłania w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="bccfa-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="bccfa-241">Domyślnie tylko jeden strumień jest używany dla SQL Server i Redis, pięć strumieni jest używanych do Service Bus, a kolejkowanie jest wyłączone, ale te ustawienia można zmienić za pomocą konfiguracji w SQL Server i Service Bus:</span><span class="sxs-lookup"><span data-stu-id="bccfa-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="bccfa-242">**Kod serwera .NET służący do konfigurowania liczby tabel i długości kolejki dla SQL Server**</span><span class="sxs-lookup"><span data-stu-id="bccfa-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="bccfa-243">**Kod serwera .NET służący do konfigurowania liczby tematów i długości kolejki dla Service Bus**</span><span class="sxs-lookup"><span data-stu-id="bccfa-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="bccfa-244">Strumień **buforowania** to taki, który przeszedł do stanu błędu; gdy strumień jest w stanie awarii, wszystkie komunikaty wysyłane do planu awaryjnego zakończą się niepowodzeniem, dopóki strumień nie będzie już powodował błędów.</span><span class="sxs-lookup"><span data-stu-id="bccfa-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="bccfa-245">**Długość kolejki wysyłania** to liczba komunikatów, które zostały ogłoszone, ale nie zostały jeszcze wysłane.</span><span class="sxs-lookup"><span data-stu-id="bccfa-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="bccfa-246">**Odebrane komunikaty magistrali komunikatów skalowania/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="bccfa-247">**Całkowita liczba strumieni skalowania**</span><span class="sxs-lookup"><span data-stu-id="bccfa-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="bccfa-248">**Otwarte strumienie skalowania**</span><span class="sxs-lookup"><span data-stu-id="bccfa-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="bccfa-249">**Buforowanie strumieni skalowania**</span><span class="sxs-lookup"><span data-stu-id="bccfa-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="bccfa-250">**Łączna liczba błędów skalowania**</span><span class="sxs-lookup"><span data-stu-id="bccfa-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="bccfa-251">**Błędy skalowania/s**</span><span class="sxs-lookup"><span data-stu-id="bccfa-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="bccfa-252">**Długość kolejki wysyłania skalowania**</span><span class="sxs-lookup"><span data-stu-id="bccfa-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="bccfa-253">Aby uzyskać więcej informacji o tym, co to są pomiary, zobacz [sygnalizującer skalowania with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="bccfa-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="bccfa-254">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="bccfa-254">Using other performance counters</span></span>

<span data-ttu-id="bccfa-255">Poniższe liczniki wydajności mogą być również przydatne podczas monitorowania wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bccfa-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="bccfa-256">**Rozmiar**</span><span class="sxs-lookup"><span data-stu-id="bccfa-256">**Memory**</span></span>

- <span data-ttu-id="bccfa-257">Pamięć .NET CLR\\# bajtów we wszystkich stertach (dla w3wp)</span><span class="sxs-lookup"><span data-stu-id="bccfa-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="bccfa-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="bccfa-258">**ASP.NET**</span></span>

- <span data-ttu-id="bccfa-259">ASP. NET\Requests — bieżące</span><span class="sxs-lookup"><span data-stu-id="bccfa-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="bccfa-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="bccfa-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="bccfa-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="bccfa-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="bccfa-262">**TESTY**</span><span class="sxs-lookup"><span data-stu-id="bccfa-262">**CPU**</span></span>

- <span data-ttu-id="bccfa-263">Czas Information\Processor procesora</span><span class="sxs-lookup"><span data-stu-id="bccfa-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="bccfa-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="bccfa-264">**TCP/IP**</span></span>

- <span data-ttu-id="bccfa-265">TCPv6/połączenia zostały ustanowione</span><span class="sxs-lookup"><span data-stu-id="bccfa-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="bccfa-266">TCPv4/połączenia zostały ustanowione</span><span class="sxs-lookup"><span data-stu-id="bccfa-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="bccfa-267">**Usługa sieci Web**</span><span class="sxs-lookup"><span data-stu-id="bccfa-267">**Web Service**</span></span>

- <span data-ttu-id="bccfa-268">Połączenia sieci Web Service\Current</span><span class="sxs-lookup"><span data-stu-id="bccfa-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="bccfa-269">Połączenia sieci Web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="bccfa-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="bccfa-270">**Wątkowość**</span><span class="sxs-lookup"><span data-stu-id="bccfa-270">**Threading**</span></span>

- <span data-ttu-id="bccfa-271">Zablokowanie i wątki środowiska .NET CLR\\# bieżących wątków logicznych</span><span class="sxs-lookup"><span data-stu-id="bccfa-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="bccfa-272">Blokady i wątki środowiska .NET CLR\\# bieżących wątków fizycznych</span><span class="sxs-lookup"><span data-stu-id="bccfa-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="bccfa-273">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="bccfa-273">Other Resources</span></span>

<span data-ttu-id="bccfa-274">Więcej informacji na temat monitorowania i dostrajania wydajności ASP.NET można znaleźć w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="bccfa-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="bccfa-275">[Przegląd wydajności ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="bccfa-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="bccfa-276">Użycie wątku ASP.NET w usługach IIS 7,5, IIS 7,0 i IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="bccfa-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="bccfa-277">&lt;element&gt; applicationPool (Ustawienia sieci Web)</span><span class="sxs-lookup"><span data-stu-id="bccfa-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
