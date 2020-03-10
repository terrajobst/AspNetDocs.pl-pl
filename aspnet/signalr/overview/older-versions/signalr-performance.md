---
uid: signalr/overview/older-versions/signalr-performance
title: Wydajność sygnalizująca (sygnał 1. x) | Microsoft Docs
author: bradygaster
description: Wydajność usługi SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 915fd822caae9bbcb0a688c6dd7a5b2bda12c9df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579612"
---
# <a name="signalr-performance-signalr-1x"></a><span data-ttu-id="05a7f-103">Wydajność usługi SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="05a7f-103">SignalR Performance (SignalR 1.x)</span></span>

<span data-ttu-id="05a7f-104">[Fletcher Patryka](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="05a7f-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="05a7f-105">W tym temacie opisano sposób projektowania, mierzenia i poprawiania wydajności w aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="05a7f-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>

<span data-ttu-id="05a7f-106">Aby uzyskać ostatnią prezentację na temat wydajności i skalowania sygnału, zobacz [skalowanie sieci Web w czasie rzeczywistym za pomocą sygnału ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="05a7f-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="05a7f-107">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="05a7f-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="05a7f-108">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="05a7f-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="05a7f-109">Dostrajanie serwera sygnalizującego pod kątem wydajności</span><span class="sxs-lookup"><span data-stu-id="05a7f-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="05a7f-110">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="05a7f-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="05a7f-111">Korzystanie z liczników wydajności sygnałów</span><span class="sxs-lookup"><span data-stu-id="05a7f-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="05a7f-112">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="05a7f-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="05a7f-113">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="05a7f-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="05a7f-114">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="05a7f-114">Design considerations</span></span>

<span data-ttu-id="05a7f-115">W tej sekcji opisano wzorce, które mogą być implementowane podczas projektowania aplikacji sygnalizującej, aby zapewnić, że wydajność nie jest zakłócana przez wygenerowanie niepotrzebnego ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="05a7f-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="05a7f-116">Częstotliwość komunikatów ograniczania przepustowości</span><span class="sxs-lookup"><span data-stu-id="05a7f-116">Throttling message frequency</span></span>

<span data-ttu-id="05a7f-117">Nawet w aplikacji, która wysyła komunikaty z dużą częstotliwością (na przykład aplikacja do gier w czasie rzeczywistym), większość aplikacji nie musi wysyłać więcej niż kilka komunikatów na sekundę.</span><span class="sxs-lookup"><span data-stu-id="05a7f-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="05a7f-118">Aby zmniejszyć ilość ruchu wygenerowanego przez każdego klienta, można zaimplementować pętlę komunikatów, która nie jest częściej niż stała stawka (oznacza to, że do określonej liczby komunikatów będą wysyłane co sekundę, jeśli w tym czasie znajdują się komunikaty terval do wysłania).</span><span class="sxs-lookup"><span data-stu-id="05a7f-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="05a7f-119">W przypadku przykładowej aplikacji, która ogranicza komunikaty do określonej stawki (zarówno z klienta i serwera), zobacz czas [rzeczywisty o wysokiej częstotliwości z sygnalizującym](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="05a7f-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="05a7f-120">Zmniejszanie rozmiaru wiadomości</span><span class="sxs-lookup"><span data-stu-id="05a7f-120">Reducing message size</span></span>

<span data-ttu-id="05a7f-121">Możesz zmniejszyć rozmiar komunikatu sygnalizującego, zmniejszając rozmiar serializowanych obiektów.</span><span class="sxs-lookup"><span data-stu-id="05a7f-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="05a7f-122">W przypadku wysyłania obiektu, który zawiera właściwości, które nie muszą być przesyłane, w kodzie serwera, należy zapobiec serializacji tych właściwości przy użyciu atrybutu `JsonIgnore`.</span><span class="sxs-lookup"><span data-stu-id="05a7f-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="05a7f-123">Nazwy właściwości są również przechowywane w komunikacie. nazwy właściwości można skrócić przy użyciu atrybutu `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="05a7f-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="05a7f-124">Poniższy przykład kodu demonstruje, jak wykluczyć właściwość z wysyłania do klienta i jak skrócić nazwy właściwości:</span><span class="sxs-lookup"><span data-stu-id="05a7f-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="05a7f-125">**Kod serwera .NET, który demonstruje atrybut JsonIgnore, aby wykluczyć dane wysyłane do klienta i atrybut JsonProperty w celu zmniejszenia rozmiaru komunikatu**</span><span class="sxs-lookup"><span data-stu-id="05a7f-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="05a7f-126">Aby zachować czytelność/łatwość utrzymania w kodzie klienta, skrócone nazwy właściwości mogą być ponownie mapowane na przyjazne nazwy po odebraniu wiadomości.</span><span class="sxs-lookup"><span data-stu-id="05a7f-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="05a7f-127">Poniższy przykład kodu ilustruje jeden z możliwych sposobów ponownego mapowania skróconych nazw do większej, przez zdefiniowanie kontraktu komunikatu (mapowanie) i użycie funkcji `reMap` do zastosowania kontraktu do klasy zoptymalizowanego komunikatu:</span><span class="sxs-lookup"><span data-stu-id="05a7f-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="05a7f-128">**Kod JavaScript po stronie klienta, który ponownie mapuje skrócone nazwy właściwości na nazwy czytelne dla człowieka**</span><span class="sxs-lookup"><span data-stu-id="05a7f-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="05a7f-129">Nazwy można skrócić również w komunikatach z klienta do serwera, używając tej samej metody.</span><span class="sxs-lookup"><span data-stu-id="05a7f-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="05a7f-130">Zmniejszenie ilości pamięci (czyli ilości pamięci używanej przez komunikat) obiektu komunikatu może również zwiększyć wydajność.</span><span class="sxs-lookup"><span data-stu-id="05a7f-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="05a7f-131">Na przykład jeśli pełny zakres `int` nie jest wymagany, zamiast tego można użyć `short` lub `byte`.</span><span class="sxs-lookup"><span data-stu-id="05a7f-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="05a7f-132">Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie rozmiaru komunikatów może również rozwiązać problemy z pamięcią serwera.</span><span class="sxs-lookup"><span data-stu-id="05a7f-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="05a7f-133">Dostrajanie serwera sygnalizującego pod kątem wydajności</span><span class="sxs-lookup"><span data-stu-id="05a7f-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="05a7f-134">Poniższe ustawienia konfiguracji mogą służyć do dostrajania serwera w celu zapewnienia lepszej wydajności w aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="05a7f-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="05a7f-135">Aby uzyskać ogólne informacje na temat ulepszania wydajności w aplikacji ASP.NET, zobacz [Poprawianie wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="05a7f-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="05a7f-136">**Ustawienia konfiguracji sygnalizującego**</span><span class="sxs-lookup"><span data-stu-id="05a7f-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="05a7f-137">**DefaultMessageBufferSize**: domyślnie sygnalizujący przechowuje 1000 komunikatów w pamięci na koncentratorze dla każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="05a7f-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="05a7f-138">W przypadku korzystania z dużych komunikatów może to spowodować problemy z pamięcią, które można wyeliminować, zmniejszając tę wartość.</span><span class="sxs-lookup"><span data-stu-id="05a7f-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="05a7f-139">To ustawienie można ustawić w obsłudze zdarzeń `Application_Start` w aplikacji ASP.NET lub w metodzie `Configuration` klasy uruchomieniowej OWIN w aplikacji samohostowanej.</span><span class="sxs-lookup"><span data-stu-id="05a7f-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="05a7f-140">Poniższy przykład demonstruje, jak zmniejszyć tę wartość, aby zmniejszyć rozmiar pamięci aplikacji w celu zmniejszenia ilości używanej pamięci serwera:</span><span class="sxs-lookup"><span data-stu-id="05a7f-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="05a7f-141">**Kod serwera .NET w Global. asax dla zmniejszenia domyślnego rozmiaru buforu komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="05a7f-142">**Ustawienia konfiguracji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="05a7f-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="05a7f-143">**Maksymalna liczba współbieżnych żądań na aplikację**: zwiększenie liczby równoczesnych żądań usług IIS spowoduje zwiększenie zasobów serwera dostępnych do obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="05a7f-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="05a7f-144">Wartość domyślna to 5000; Aby zwiększyć to ustawienie, wykonaj następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień:</span><span class="sxs-lookup"><span data-stu-id="05a7f-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="05a7f-145">**Ustawienia konfiguracji ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="05a7f-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="05a7f-146">Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w pliku `aspnet.config`.</span><span class="sxs-lookup"><span data-stu-id="05a7f-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="05a7f-147">Ten plik znajduje się w jednej z dwóch lokalizacji, w zależności od platformy:</span><span class="sxs-lookup"><span data-stu-id="05a7f-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="05a7f-148">Ustawienia ASP.NET, które mogą poprawić wydajność sygnalizującego, obejmują następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="05a7f-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="05a7f-149">**Maksymalna liczba współbieżnych żądań na procesor CPU**: zwiększenie tego ustawienia może złagodzić wąskie gardła wydajności.</span><span class="sxs-lookup"><span data-stu-id="05a7f-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="05a7f-150">Aby zwiększyć to ustawienie, należy dodać następujące ustawienia konfiguracji do pliku `aspnet.config`:</span><span class="sxs-lookup"><span data-stu-id="05a7f-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="05a7f-151">**Limit kolejki żądań**: gdy całkowita liczba połączeń przekroczy ustawienie `maxConcurrentRequestsPerCPU`, ASP.NET rozpocznie ograniczanie żądań przy użyciu kolejki.</span><span class="sxs-lookup"><span data-stu-id="05a7f-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="05a7f-152">Aby zwiększyć rozmiar kolejki, można zwiększyć ustawienie `requestQueueLimit`.</span><span class="sxs-lookup"><span data-stu-id="05a7f-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="05a7f-153">W tym celu należy dodać następujące ustawienia konfiguracji do węzła `processModel` w `config/machine.config` (zamiast `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="05a7f-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="05a7f-154">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="05a7f-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="05a7f-155">W tej sekcji opisano sposoby znajdowania wąskich gardeł wydajności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05a7f-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="05a7f-156">Weryfikowanie, czy protokół WebSocket jest używany</span><span class="sxs-lookup"><span data-stu-id="05a7f-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="05a7f-157">Chociaż sygnalizujący może używać różnych transportów komunikacji między klientem i serwerem, protokół WebSocket oferuje znaczną wydajność i należy go używać, jeśli klient i serwer obsługują tę opcję.</span><span class="sxs-lookup"><span data-stu-id="05a7f-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="05a7f-158">Aby określić, czy klient i serwer spełniają wymagania dotyczące protokołu WebSocket, zobacz [Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="05a7f-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="05a7f-159">Aby określić, który transport jest używany w aplikacji, możesz użyć narzędzi deweloperskich przeglądarki i sprawdzić dzienniki, aby zobaczyć, jaki transport jest używany do połączenia.</span><span class="sxs-lookup"><span data-stu-id="05a7f-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="05a7f-160">Aby uzyskać informacje na temat korzystania z narzędzi programistycznych przeglądarki w programie Internet Explorer i programie Chrome, zobacz [Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="05a7f-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="05a7f-161">Korzystanie z liczników wydajności sygnałów</span><span class="sxs-lookup"><span data-stu-id="05a7f-161">Using SignalR performance counters</span></span>

<span data-ttu-id="05a7f-162">W tej sekcji opisano sposób włączania i używania liczników wydajności sygnałów, które znajdują się w pakiecie `Microsoft.AspNet.SignalR.Utils`.</span><span class="sxs-lookup"><span data-stu-id="05a7f-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="05a7f-163">Instalowanie sygnalizującer. exe</span><span class="sxs-lookup"><span data-stu-id="05a7f-163">Installing signalr.exe</span></span>

<span data-ttu-id="05a7f-164">Liczniki wydajności można dodać do serwera za pomocą narzędzia o nazwie Sygnalizującer. exe.</span><span class="sxs-lookup"><span data-stu-id="05a7f-164">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="05a7f-165">Aby zainstalować to narzędzie, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="05a7f-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="05a7f-166">W programie Visual Studio wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **Zarządzanie pakietami NuGet dla rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="05a7f-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="05a7f-167">Wyszukaj pozycję **sygnalizujący.** narzędzia i wybierz pozycję Zainstaluj.</span><span class="sxs-lookup"><span data-stu-id="05a7f-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="05a7f-168">Zaakceptuj umowę licencyjną, aby zainstalować pakiet.</span><span class="sxs-lookup"><span data-stu-id="05a7f-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="05a7f-169">Program sygnalizujący. exe zostanie zainstalowany do `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="05a7f-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="05a7f-170">Instalowanie liczników wydajności za pomocą programu Signaler. exe</span><span class="sxs-lookup"><span data-stu-id="05a7f-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="05a7f-171">Aby zainstalować liczniki wydajności sygnałów, uruchom program Sygnalizującer. exe w wierszu polecenia z podwyższonym poziomem uprawnień z następującym parametrem:</span><span class="sxs-lookup"><span data-stu-id="05a7f-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="05a7f-172">Aby usunąć liczniki wydajności sygnałów, uruchom program Signaler. exe w wierszu polecenia z podwyższonym poziomem uprawnień z następującym parametrem:</span><span class="sxs-lookup"><span data-stu-id="05a7f-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="05a7f-173">Liczniki wydajności sygnałów</span><span class="sxs-lookup"><span data-stu-id="05a7f-173">SignalR Performance counters</span></span>

<span data-ttu-id="05a7f-174">Pakiet narzędzi instaluje następujące liczniki wydajności.</span><span class="sxs-lookup"><span data-stu-id="05a7f-174">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="05a7f-175">Liczniki "Total" mierzą liczbę zdarzeń od momentu ostatniego ponownego uruchomienia serwera.</span><span class="sxs-lookup"><span data-stu-id="05a7f-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="05a7f-176">**Metryki połączeń**</span><span class="sxs-lookup"><span data-stu-id="05a7f-176">**Connection metrics**</span></span>

<span data-ttu-id="05a7f-177">Następujące metryki mierzą zdarzenia okresu istnienia połączenia, które wystąpiły.</span><span class="sxs-lookup"><span data-stu-id="05a7f-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="05a7f-178">Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="05a7f-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="05a7f-179">**Połączenia połączone**</span><span class="sxs-lookup"><span data-stu-id="05a7f-179">**Connections Connected**</span></span>
- <span data-ttu-id="05a7f-180">**Połączenia zostały ponownie połączone**</span><span class="sxs-lookup"><span data-stu-id="05a7f-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="05a7f-181">**Połączenia rozłączone**</span><span class="sxs-lookup"><span data-stu-id="05a7f-181">**Connections Disconnected**</span></span>
- <span data-ttu-id="05a7f-182">**Bieżące połączenia**</span><span class="sxs-lookup"><span data-stu-id="05a7f-182">**Connections Current**</span></span>

<span data-ttu-id="05a7f-183">**Metryki komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-183">**Message metrics**</span></span>

<span data-ttu-id="05a7f-184">Następujące metryki mierzą ruch komunikatów generowany przez program sygnalizujący.</span><span class="sxs-lookup"><span data-stu-id="05a7f-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="05a7f-185">**Całkowita liczba odebranych komunikatów połączenia**</span><span class="sxs-lookup"><span data-stu-id="05a7f-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="05a7f-186">**Całkowita liczba wysłanych komunikatów połączenia**</span><span class="sxs-lookup"><span data-stu-id="05a7f-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="05a7f-187">**Odebrane komunikaty połączenia/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="05a7f-188">**Wysłane komunikaty połączenia/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="05a7f-189">**Metryki magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-189">**Message bus metrics**</span></span>

<span data-ttu-id="05a7f-190">Następujące metryki mierzą ruch przez magistralę komunikatów wewnętrznych sygnalizujących, kolejkę, w której są umieszczane wszystkie komunikaty sygnalizujące przychodzące i wychodzące.</span><span class="sxs-lookup"><span data-stu-id="05a7f-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="05a7f-191">Komunikat jest **publikowany** , gdy jest wysyłany lub emitowany.</span><span class="sxs-lookup"><span data-stu-id="05a7f-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="05a7f-192">**Subskrybent** w tym kontekście jest subskrypcją magistrali komunikatów; Ta wartość powinna być równa liczbie klientów i serwera.</span><span class="sxs-lookup"><span data-stu-id="05a7f-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="05a7f-193">**Przydzielony proces roboczy** to składnik, który wysyła dane do aktywnych połączeń; **zajęty proces roboczy** to ten, który aktywnie wysyła komunikat.</span><span class="sxs-lookup"><span data-stu-id="05a7f-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="05a7f-194">**Całkowita liczba odebranych komunikatów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="05a7f-195">**Odebrane komunikaty magistrali komunikatów/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="05a7f-196">**Łączna liczba opublikowanych komunikatów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="05a7f-197">**Opublikowane komunikaty magistrali komunikatów/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="05a7f-198">**Aktualni Subskrybenci magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="05a7f-199">**Łączna liczba subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="05a7f-200">**Subskrybenci magistrali komunikatów/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="05a7f-201">**Pracownicy przydzieleni do magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="05a7f-202">**Pracownikom zajętym przez magistralę komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="05a7f-203">**Bieżące tematy dotyczące magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="05a7f-204">**Metryki błędów**</span><span class="sxs-lookup"><span data-stu-id="05a7f-204">**Error metrics**</span></span>

<span data-ttu-id="05a7f-205">Następujące metryki mierzą błędy generowane przez ruch komunikatów sygnalizujących.</span><span class="sxs-lookup"><span data-stu-id="05a7f-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="05a7f-206">Błędy **rozpoznawania centrów** występują, gdy nie można rozpoznać metody Hub lub Hub.</span><span class="sxs-lookup"><span data-stu-id="05a7f-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="05a7f-207">Błędy **wywołania centrum** to wyjątki zgłoszone podczas wywoływania metody centrum.</span><span class="sxs-lookup"><span data-stu-id="05a7f-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="05a7f-208">Błędy **transportu** to błędy połączeń zgłoszone podczas żądania HTTP lub odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05a7f-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="05a7f-209">**Błędy: wszystkie łącznie**</span><span class="sxs-lookup"><span data-stu-id="05a7f-209">**Errors: All Total**</span></span>
- <span data-ttu-id="05a7f-210">**Błędy: wszystkie/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="05a7f-211">**Błędy: rozdzielczość centrum łącznie**</span><span class="sxs-lookup"><span data-stu-id="05a7f-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="05a7f-212">**Błędy: rozdzielczość centrum/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="05a7f-213">**Błędy: całkowita liczba wywołań koncentratora**</span><span class="sxs-lookup"><span data-stu-id="05a7f-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="05a7f-214">**Błędy: wywołanie koncentratora/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="05a7f-215">**Błędy: łącznie z transportem**</span><span class="sxs-lookup"><span data-stu-id="05a7f-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="05a7f-216">**Błędy: transport/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="05a7f-217">**Metryki skalowania**</span><span class="sxs-lookup"><span data-stu-id="05a7f-217">**Scaleout metrics**</span></span>

<span data-ttu-id="05a7f-218">Następujące metryki mierzą ruch i błędy generowane przez dostawcę skalowania.</span><span class="sxs-lookup"><span data-stu-id="05a7f-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="05a7f-219">**Strumień** w tym kontekście jest jednostką skalowania używaną przez dostawcę skalowania. jest to tabela, jeśli użyto SQL Server, tematu, jeśli jest używany Service Bus, i subskrypcji, jeśli jest używana Redis.</span><span class="sxs-lookup"><span data-stu-id="05a7f-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="05a7f-220">Domyślnie używany jest tylko jeden strumień, ale można go zwiększyć za pomocą konfiguracji w SQL Server i Service Bus.</span><span class="sxs-lookup"><span data-stu-id="05a7f-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="05a7f-221">Strumień **buforowania** to taki, który przeszedł do stanu błędu; gdy strumień jest w stanie awarii, wszystkie komunikaty wysyłane do planu awaryjnego zakończą się niepowodzeniem, dopóki strumień nie będzie już powodował błędów.</span><span class="sxs-lookup"><span data-stu-id="05a7f-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="05a7f-222">**Długość kolejki wysyłania** to liczba komunikatów, które zostały ogłoszone, ale nie zostały jeszcze wysłane.</span><span class="sxs-lookup"><span data-stu-id="05a7f-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="05a7f-223">**Odebrane komunikaty magistrali komunikatów skalowania/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="05a7f-224">**Całkowita liczba strumieni skalowania**</span><span class="sxs-lookup"><span data-stu-id="05a7f-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="05a7f-225">**Otwarte strumienie skalowania**</span><span class="sxs-lookup"><span data-stu-id="05a7f-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="05a7f-226">**Buforowanie strumieni skalowania**</span><span class="sxs-lookup"><span data-stu-id="05a7f-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="05a7f-227">**Łączna liczba błędów skalowania**</span><span class="sxs-lookup"><span data-stu-id="05a7f-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="05a7f-228">**Błędy skalowania/s**</span><span class="sxs-lookup"><span data-stu-id="05a7f-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="05a7f-229">**Długość kolejki wysyłania skalowania**</span><span class="sxs-lookup"><span data-stu-id="05a7f-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="05a7f-230">Aby uzyskać więcej informacji o tym, co to są pomiary, zobacz [sygnalizującer skalowania with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="05a7f-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="05a7f-231">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="05a7f-231">Using other performance counters</span></span>

<span data-ttu-id="05a7f-232">Poniższe liczniki wydajności mogą być również przydatne podczas monitorowania wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05a7f-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="05a7f-233">**Rozmiar**</span><span class="sxs-lookup"><span data-stu-id="05a7f-233">**Memory**</span></span>

- <span data-ttu-id="05a7f-234">Pamięć .NET CLR liczba bajtów we wszystkich stertach (dla w3wp)</span><span class="sxs-lookup"><span data-stu-id="05a7f-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="05a7f-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="05a7f-235">**ASP.NET**</span></span>

- <span data-ttu-id="05a7f-236">ASP. NET\Requests — bieżące</span><span class="sxs-lookup"><span data-stu-id="05a7f-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="05a7f-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="05a7f-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="05a7f-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="05a7f-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="05a7f-239">**TESTY**</span><span class="sxs-lookup"><span data-stu-id="05a7f-239">**CPU**</span></span>

- <span data-ttu-id="05a7f-240">Czas Information\Processor procesora</span><span class="sxs-lookup"><span data-stu-id="05a7f-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="05a7f-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="05a7f-241">**TCP/IP**</span></span>

- <span data-ttu-id="05a7f-242">TCPv6/połączenia zostały ustanowione</span><span class="sxs-lookup"><span data-stu-id="05a7f-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="05a7f-243">TCPv4/połączenia zostały ustanowione</span><span class="sxs-lookup"><span data-stu-id="05a7f-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="05a7f-244">**Usługa sieci Web**</span><span class="sxs-lookup"><span data-stu-id="05a7f-244">**Web Service**</span></span>

- <span data-ttu-id="05a7f-245">Połączenia sieci Web Service\Current</span><span class="sxs-lookup"><span data-stu-id="05a7f-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="05a7f-246">Połączenia sieci Web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="05a7f-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="05a7f-247">**Wątkowość**</span><span class="sxs-lookup"><span data-stu-id="05a7f-247">**Threading**</span></span>

- <span data-ttu-id="05a7f-248">.NET CLR LocksAndThreads\# bieżących wątków logicznych</span><span class="sxs-lookup"><span data-stu-id="05a7f-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="05a7f-249">Wątki środowiska .NET CLR LocksAnd\# bieżących wątków fizycznych</span><span class="sxs-lookup"><span data-stu-id="05a7f-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="05a7f-250">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="05a7f-250">Other Resources</span></span>

<span data-ttu-id="05a7f-251">Więcej informacji na temat monitorowania i dostrajania wydajności ASP.NET można znaleźć w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="05a7f-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="05a7f-252">[Przegląd wydajności ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="05a7f-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="05a7f-253">Użycie wątku ASP.NET w usługach IIS 7,5, IIS 7,0 i IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="05a7f-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="05a7f-254">&lt;element&gt; applicationPool (Ustawienia sieci Web)</span><span class="sxs-lookup"><span data-stu-id="05a7f-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
