---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Przewodnik interfejsu API centrów sygnałów ASP.NET — klient platformy .NET (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 2 na klientach platformy .NET, takich jak Windows Store (WinRT), WPF, Silverlight i wady...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 2b22b53c405a865f91b04e677f60b82dd46dbf9b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623803"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="a6683-103">Przewodnik interfejsu API centrów sygnałów ASP.NET — klient platformy .NET (sygnalizujący 1. x)</span><span class="sxs-lookup"><span data-stu-id="a6683-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>

<span data-ttu-id="a6683-104">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a6683-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="a6683-105">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 2 na klientach platformy .NET, takich jak Windows Store (WinRT), WPF, Silverlight i aplikacje konsolowe.</span><span class="sxs-lookup"><span data-stu-id="a6683-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="a6683-106">Interfejs API centrów sygnałów umożliwia wykonywanie zdalnych wywołań procedur (RPC) z serwera do podłączonych klientów i od klientów do serwera programu.</span><span class="sxs-lookup"><span data-stu-id="a6683-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="a6683-107">W polu kod serwera można zdefiniować metody, które mogą być wywoływane przez klientów, i wywoływanie metod uruchamianych na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a6683-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="a6683-108">W kodzie klienta należy zdefiniować metody, które mogą być wywoływane z serwera programu, i wywoływanie metod, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a6683-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="a6683-109">Sygnalizujący, że wszystkie instalacje z klientem do serwera są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a6683-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="a6683-110">Sygnalizujący oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="a6683-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="a6683-111">Aby zapoznać się z wprowadzeniem do sygnałów, centrów i połączeń trwałych lub samouczka, w którym pokazano, jak utworzyć kompletną aplikację sygnalizującą, zobacz [sygnalizującer-wprowadzenie](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="a6683-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="a6683-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a6683-112">Overview</span></span>

<span data-ttu-id="a6683-113">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="a6683-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="a6683-114">Konfiguracja klienta</span><span class="sxs-lookup"><span data-stu-id="a6683-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="a6683-115">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="a6683-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="a6683-116">Połączenia między domenami z klientów Silverlight</span><span class="sxs-lookup"><span data-stu-id="a6683-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="a6683-117">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="a6683-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="a6683-118">Jak ustawić maksymalną liczbę jednoczesnych połączeń w klientach WPF</span><span class="sxs-lookup"><span data-stu-id="a6683-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="a6683-119">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="a6683-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="a6683-120">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="a6683-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="a6683-121">Jak określić nagłówki HTTP</span><span class="sxs-lookup"><span data-stu-id="a6683-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="a6683-122">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="a6683-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="a6683-123">Jak utworzyć serwer proxy centrum</span><span class="sxs-lookup"><span data-stu-id="a6683-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="a6683-124">Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer</span><span class="sxs-lookup"><span data-stu-id="a6683-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="a6683-125">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="a6683-126">Metody z parametrami, Określanie typów parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="a6683-127">Metody z parametrami, określając dynamiczne obiekty parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="a6683-128">Jak usunąć procedurę obsługi</span><span class="sxs-lookup"><span data-stu-id="a6683-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="a6683-129">Jak wywoływać metody serwera z klienta</span><span class="sxs-lookup"><span data-stu-id="a6683-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="a6683-130">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="a6683-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="a6683-131">Jak obsłużyć błędy</span><span class="sxs-lookup"><span data-stu-id="a6683-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="a6683-132">Jak włączyć rejestrowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="a6683-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="a6683-133">Przykłady kodu aplikacji WPF, Silverlight i konsoli dla metod klienta wywoływanych przez serwer</span><span class="sxs-lookup"><span data-stu-id="a6683-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="a6683-134">Przykładowe projekty klienta platformy .NET można znaleźć w następujących zasobach:</span><span class="sxs-lookup"><span data-stu-id="a6683-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="a6683-135">[Gustavo-Armenta/sygnalizujący — przykłady](https://github.com/gustavo-armenta/SignalR-Samples) w GitHub.com (WinRT, Silverlight, przykłady aplikacji konsolowych).</span><span class="sxs-lookup"><span data-stu-id="a6683-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="a6683-136">[DamianEdwards/Signal-MoveShapeDemo/MoveShape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (przykład WPF).</span><span class="sxs-lookup"><span data-stu-id="a6683-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="a6683-137">[Signaler/Microsoft. ASPNET. Signal. Client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) in GitHub.com (przykładowa aplikacja konsoli).</span><span class="sxs-lookup"><span data-stu-id="a6683-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="a6683-138">Aby uzyskać dokumentację dotyczącą sposobu programowania serwera lub klientów JavaScript, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="a6683-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="a6683-139">Przewodnik interfejsu API centrów sygnałów — serwer</span><span class="sxs-lookup"><span data-stu-id="a6683-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="a6683-140">Przewodnik interfejsu API centrów sygnałów — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="a6683-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="a6683-141">Łącza do tematów dotyczących odwołań do interfejsów API znajdują się w wersji programu .NET 4,5 interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a6683-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="a6683-142">Jeśli używasz programu .NET 4, zobacz [temat wersja interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="a6683-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="a6683-143">Konfiguracja klienta</span><span class="sxs-lookup"><span data-stu-id="a6683-143">Client setup</span></span>

<span data-ttu-id="a6683-144">Zainstaluj pakiet NuGet [Microsoft. ASPNET. Signal. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (nie pakiet [Microsoft. ASPNET. signaler](http://nuget.org/packages/microsoft.aspnet.signalr) ).</span><span class="sxs-lookup"><span data-stu-id="a6683-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="a6683-145">Ten pakiet obsługuje klientów WinRT, Silverlight, WPF, aplikacji konsolowych i Windows Phone dla programów .NET 4 i .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="a6683-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="a6683-146">Jeśli wersja usługi sygnalizująca, która jest zainstalowana na kliencie, różni się od wersji programu znajdującej się na serwerze, sygnał jest często możliwy do dostosowania do różnic.</span><span class="sxs-lookup"><span data-stu-id="a6683-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="a6683-147">Na przykład gdy zostanie wydane polecenie sygnalizujące w wersji 2,0 i zostanie ono zainstalowane na serwerze, serwer będzie obsługiwał klientów z zainstalowanym systemem 1.1. x oraz klientami z zainstalowanym 2,0.</span><span class="sxs-lookup"><span data-stu-id="a6683-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="a6683-148">Jeśli różnica między wersją na serwerze i wersją na kliencie jest zbyt duża, sygnalizujący zgłasza wyjątek `InvalidOperationException`, gdy klient próbuje nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="a6683-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="a6683-149">Komunikat o błędzie to "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="a6683-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="a6683-150">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="a6683-150">How to establish a connection</span></span>

<span data-ttu-id="a6683-151">Aby można było nawiązać połączenie, należy utworzyć obiekt `HubConnection` i utworzyć serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="a6683-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="a6683-152">Aby nawiązać połączenie, wywołaj metodę `Start` na obiekcie `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="a6683-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="a6683-153">W przypadku klientów JavaScript należy zarejestrować co najmniej jeden program obsługi zdarzeń przed wywołaniem metody `Start` w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="a6683-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="a6683-154">Nie jest to konieczne w przypadku klientów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="a6683-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="a6683-155">W przypadku klientów JavaScript wygenerowany kod serwera proxy automatycznie tworzy serwery proxy dla wszystkich centrów istniejących na serwerze, a procedura rejestrowania programu obsługi wskazuje, które centra, których Klient zamierza używać.</span><span class="sxs-lookup"><span data-stu-id="a6683-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="a6683-156">Jednak dla klienta platformy .NET można ręcznie utworzyć centra proxy, dlatego moduł sygnalizujący zakłada, że będziesz używać dowolnego centrum, dla którego tworzysz serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="a6683-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="a6683-157">Przykładowy kod używa domyślnego adresu URL "/SignalR", aby nawiązać połączenie z usługą sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="a6683-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="a6683-158">Aby uzyskać informacje na temat sposobu określania innego podstawowego adresu URL, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="a6683-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="a6683-159">Metoda `Start` jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="a6683-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="a6683-160">Aby upewnić się, że kolejne wiersze kodu nie są wykonywane, dopóki połączenie nie zostanie nawiązane, użyj `await` w metodzie asynchronicznej ASP.NET 4,5 lub `.Wait()` w metodzie synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="a6683-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="a6683-161">Nie należy używać `.Wait()` w kliencie WinRT.</span><span class="sxs-lookup"><span data-stu-id="a6683-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="a6683-162">Klasa `HubConnection` jest bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="a6683-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="a6683-163">Połączenia między domenami z klientów Silverlight</span><span class="sxs-lookup"><span data-stu-id="a6683-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="a6683-164">Informacje o sposobie włączania połączeń między domenami z klientów Silverlight można znaleźć w temacie [udostępnianie usługi w granicach domen](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="a6683-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="a6683-165">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="a6683-165">How to configure the connection</span></span>

<span data-ttu-id="a6683-166">Przed nawiązaniem połączenia można określić jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="a6683-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="a6683-167">Limit jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="a6683-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="a6683-168">Parametry ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="a6683-168">Query string parameters.</span></span>
- <span data-ttu-id="a6683-169">Metoda transportu.</span><span class="sxs-lookup"><span data-stu-id="a6683-169">The transport method.</span></span>
- <span data-ttu-id="a6683-170">Nagłówki HTTP.</span><span class="sxs-lookup"><span data-stu-id="a6683-170">HTTP headers.</span></span>
- <span data-ttu-id="a6683-171">Certyfikaty klienta.</span><span class="sxs-lookup"><span data-stu-id="a6683-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="a6683-172">Jak ustawić maksymalną liczbę jednoczesnych połączeń w klientach WPF</span><span class="sxs-lookup"><span data-stu-id="a6683-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="a6683-173">W przypadku klientów WPF może zajść potrzeba zwiększenia maksymalnej liczby jednoczesnych połączeń z wartością domyślną 2.</span><span class="sxs-lookup"><span data-stu-id="a6683-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="a6683-174">Zalecana wartość to 10.</span><span class="sxs-lookup"><span data-stu-id="a6683-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="a6683-175">Aby uzyskać więcej informacji, zobacz [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="a6683-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="a6683-176">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="a6683-176">How to specify query string parameters</span></span>

<span data-ttu-id="a6683-177">Jeśli chcesz wysłać dane do serwera podczas łączenia się z klientem, możesz dodać parametry ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="a6683-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="a6683-178">Poniższy przykład pokazuje, jak ustawić parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="a6683-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="a6683-179">Poniższy przykład pokazuje, jak odczytać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="a6683-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="a6683-180">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="a6683-180">How to specify the transport method</span></span>

<span data-ttu-id="a6683-181">W ramach procesu łączenia klient sygnalizujący zwykle negocjuje z serwerem, aby określić najlepszy transport obsługiwany przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="a6683-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="a6683-182">Jeśli wiesz już, którego transportu chcesz użyć, możesz pominąć ten proces negocjacji.</span><span class="sxs-lookup"><span data-stu-id="a6683-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="a6683-183">Aby określić metodę transportu, przekaż obiekt transportu do metody startowej.</span><span class="sxs-lookup"><span data-stu-id="a6683-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="a6683-184">Poniższy przykład pokazuje, jak określić metodę transportu w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="a6683-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="a6683-185">Przestrzeń nazw [Microsoft. ASPNET. Signal. Client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) zawiera następujące klasy, których można użyć do określenia transportu.</span><span class="sxs-lookup"><span data-stu-id="a6683-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="a6683-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="a6683-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="a6683-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="a6683-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="a6683-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostępne tylko wtedy, gdy serwer i klient używają platformy .NET 4,5).</span><span class="sxs-lookup"><span data-stu-id="a6683-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="a6683-189">[Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automatycznie wybiera najlepszą transport, który jest obsługiwany przez klienta i serwer.</span><span class="sxs-lookup"><span data-stu-id="a6683-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="a6683-190">Jest to domyślny transport.</span><span class="sxs-lookup"><span data-stu-id="a6683-190">This is the default transport.</span></span> <span data-ttu-id="a6683-191">Przekazanie tego w `Start` metodzie ma ten sam skutek, co nie przechodzą w żadnej operacji.</span><span class="sxs-lookup"><span data-stu-id="a6683-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="a6683-192">Transport ForeverFrame nie jest uwzględniony na tej liście, ponieważ jest używany tylko przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a6683-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="a6683-193">Aby uzyskać informacje na temat sprawdzania metody transportu w kodzie serwera, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server — jak uzyskać informacje o kliencie z właściwości Context](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="a6683-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="a6683-194">Aby uzyskać więcej informacji na temat transportów i rezerw, zobacz [wprowadzenie do sygnałów-Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="a6683-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="a6683-195">Jak określić nagłówki HTTP</span><span class="sxs-lookup"><span data-stu-id="a6683-195">How to specify HTTP headers</span></span>

<span data-ttu-id="a6683-196">Aby ustawić nagłówki HTTP, użyj właściwości `Headers` w obiekcie Connection.</span><span class="sxs-lookup"><span data-stu-id="a6683-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="a6683-197">Poniższy przykład pokazuje, jak dodać nagłówek HTTP.</span><span class="sxs-lookup"><span data-stu-id="a6683-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="a6683-198">Jak określić certyfikaty klienta</span><span class="sxs-lookup"><span data-stu-id="a6683-198">How to specify client certificates</span></span>

<span data-ttu-id="a6683-199">Aby dodać certyfikaty klienta, użyj metody `AddClientCertificate` w obiekcie Connection.</span><span class="sxs-lookup"><span data-stu-id="a6683-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="a6683-200">Jak utworzyć serwer proxy centrum</span><span class="sxs-lookup"><span data-stu-id="a6683-200">How to create the Hub proxy</span></span>

<span data-ttu-id="a6683-201">W celu zdefiniowania metod na kliencie, które mogą być wywoływane przez koncentrator z serwera, i aby wywołać metody w centrum na serwerze, Utwórz serwer proxy dla centrum, wywołując `CreateHubProxy` w obiekcie Connection.</span><span class="sxs-lookup"><span data-stu-id="a6683-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="a6683-202">Ciąg przekazany do `CreateHubProxy` jest nazwą klasy centrum lub nazwą określoną przez atrybut `HubName`, jeśli został on użyty na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a6683-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="a6683-203">Przy dopasowywaniu nazwy nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="a6683-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="a6683-204">**Klasa centrum na serwerze**</span><span class="sxs-lookup"><span data-stu-id="a6683-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="a6683-205">**Utwórz serwer proxy klienta dla klasy centrum**</span><span class="sxs-lookup"><span data-stu-id="a6683-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="a6683-206">Jeśli dekorować klasę centrów z atrybutem `HubName`, Użyj tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="a6683-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="a6683-207">**Klasa centrum na serwerze**</span><span class="sxs-lookup"><span data-stu-id="a6683-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="a6683-208">**Utwórz serwer proxy klienta dla klasy centrum**</span><span class="sxs-lookup"><span data-stu-id="a6683-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="a6683-209">Obiekt serwera proxy jest bezpieczny wątkowo.</span><span class="sxs-lookup"><span data-stu-id="a6683-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="a6683-210">W rzeczywistości w przypadku wywołania `HubConnection.CreateHubProxy` wiele razy z tą samą `hubName`można uzyskać ten sam buforowany obiekt `IHubProxy`.</span><span class="sxs-lookup"><span data-stu-id="a6683-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="a6683-211">Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer</span><span class="sxs-lookup"><span data-stu-id="a6683-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="a6683-212">Aby zdefiniować metodę, którą może wywołać serwer, użyj metody `On` serwera proxy, aby zarejestrować procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="a6683-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="a6683-213">W dopasowaniu nazw metod nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="a6683-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="a6683-214">Na przykład `Clients.All.UpdateStockPrice` na serwerze wykona `updateStockPrice`, `updatestockprice`lub `UpdateStockPrice` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a6683-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="a6683-215">Różne platformy klienckie mają różne wymagania dotyczące sposobu pisania kodu metody w celu zaktualizowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a6683-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="a6683-216">Przykłady przedstawiono dla klientów WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="a6683-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="a6683-217">Przykłady aplikacji WPF, Silverlight i konsoli znajdują się w [osobnej sekcji w dalszej części tego tematu](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="a6683-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="a6683-218">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-218">Methods without parameters</span></span>

<span data-ttu-id="a6683-219">Jeśli obsługiwana metoda nie ma parametrów, użyj nieogólnego przeciążenia metody `On`:</span><span class="sxs-lookup"><span data-stu-id="a6683-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="a6683-220">**Kod serwera wywołujący metodę klienta bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="a6683-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="a6683-221">**Kod klienta WinRT dla metody wywoływanej z serwera bez parametrów ([Zobacz przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="a6683-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="a6683-222">Metody z parametrami, Określanie typów parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="a6683-223">Jeśli obsługiwana metoda ma parametry, określ typy parametrów jako typy ogólne metody `On`.</span><span class="sxs-lookup"><span data-stu-id="a6683-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="a6683-224">Istnieją ogólne przeciążenia metody `On`, aby umożliwić określenie maksymalnie 8 parametrów (4 w Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="a6683-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="a6683-225">W poniższym przykładzie jeden parametr jest wysyłany do metody `UpdateStockPrice`.</span><span class="sxs-lookup"><span data-stu-id="a6683-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="a6683-226">**Kod serwera wywołujący metodę klienta z parametrem**</span><span class="sxs-lookup"><span data-stu-id="a6683-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="a6683-227">**Klasa giełdowa użyta dla parametru**</span><span class="sxs-lookup"><span data-stu-id="a6683-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="a6683-228">**Kod klienta WinRT dla metody wywoływanej z serwera z parametrem ([Zobacz przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="a6683-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="a6683-229">Metody z parametrami, określając dynamiczne obiekty parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="a6683-230">Alternatywą dla określania parametrów jako typów ogólnych metody `On` można określić parametry jako obiekty dynamiczne:</span><span class="sxs-lookup"><span data-stu-id="a6683-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="a6683-231">**Kod serwera wywołujący metodę klienta z parametrem**</span><span class="sxs-lookup"><span data-stu-id="a6683-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="a6683-232">**Klasa giełdowa użyta dla parametru**</span><span class="sxs-lookup"><span data-stu-id="a6683-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="a6683-233">**Kod klienta WinRT dla metody wywoływanej z serwera z parametrem przy użyciu obiektu dynamicznego dla parametru ([Zobacz przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="a6683-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="a6683-234">Jak usunąć procedurę obsługi</span><span class="sxs-lookup"><span data-stu-id="a6683-234">How to remove a handler</span></span>

<span data-ttu-id="a6683-235">Aby usunąć procedurę obsługi, wywołaj jej metodę `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="a6683-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="a6683-236">**Kod klienta dla metody wywoływanej z serwera**</span><span class="sxs-lookup"><span data-stu-id="a6683-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="a6683-237">**Kod klienta do usunięcia programu obsługi**</span><span class="sxs-lookup"><span data-stu-id="a6683-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="a6683-238">Jak wywoływać metody serwera z klienta</span><span class="sxs-lookup"><span data-stu-id="a6683-238">How to call server methods from the client</span></span>

<span data-ttu-id="a6683-239">Aby wywołać metodę na serwerze, użyj metody `Invoke` w serwerze proxy centrum.</span><span class="sxs-lookup"><span data-stu-id="a6683-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="a6683-240">Jeśli metoda serwera nie ma zwracanej wartości, użyj nieogólnego przeciążenia metody `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="a6683-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="a6683-241">**Kod serwera dla metody, która nie ma wartości zwracanej**</span><span class="sxs-lookup"><span data-stu-id="a6683-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="a6683-242">**Kod klienta wywołujący metodę, która nie ma wartości zwracanej**</span><span class="sxs-lookup"><span data-stu-id="a6683-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="a6683-243">Jeśli metoda serwera ma wartość zwracaną, określ zwracany typ jako typ ogólny metody `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="a6683-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="a6683-244">**Kod serwera dla metody, która ma wartość zwracaną i przyjmuje parametr typu złożonego**</span><span class="sxs-lookup"><span data-stu-id="a6683-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="a6683-245">**Klasa giełdowa użyta dla parametru i wartości zwracanej**</span><span class="sxs-lookup"><span data-stu-id="a6683-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="a6683-246">**Kod klienta wywołujący metodę, która ma wartość zwracaną i przyjmuje parametr typu złożonego, w metodzie asynchronicznej ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="a6683-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="a6683-247">**Kod klienta wywołujący metodę, która ma wartość zwracaną i pobiera parametr typu złożonego, w metodzie synchronicznej**</span><span class="sxs-lookup"><span data-stu-id="a6683-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="a6683-248">Metoda `Invoke` wykonuje asynchroniczne i zwraca obiekt `Task`.</span><span class="sxs-lookup"><span data-stu-id="a6683-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="a6683-249">Jeśli nie określisz `await` ani `.Wait()`, następnym wierszem kodu zostanie wykonane przed wykonaniem metody wywoływanej.</span><span class="sxs-lookup"><span data-stu-id="a6683-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="a6683-250">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="a6683-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="a6683-251">Program sygnalizujący udostępnia następujące zdarzenia okresu istnienia połączenia, które można obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="a6683-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="a6683-252">`Received`: wywoływane po odebraniu dowolnego danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="a6683-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="a6683-253">Dostarcza odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="a6683-253">Provides the received data.</span></span>
- <span data-ttu-id="a6683-254">`ConnectionSlow`: wywoływane, gdy klient wykrywa powolne lub często porzucane połączenie.</span><span class="sxs-lookup"><span data-stu-id="a6683-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="a6683-255">`Reconnecting`: uruchamiany, gdy podstawowy transport zacznie ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="a6683-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="a6683-256">`Reconnected`: wywoływane, gdy podstawowy transport został połączony ponownie.</span><span class="sxs-lookup"><span data-stu-id="a6683-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="a6683-257">`StateChanged`: występuje, gdy zmieni się stan połączenia.</span><span class="sxs-lookup"><span data-stu-id="a6683-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="a6683-258">Zapewnia poprzedni stan i nowy stan.</span><span class="sxs-lookup"><span data-stu-id="a6683-258">Provides the old state and the new state.</span></span> <span data-ttu-id="a6683-259">Aby uzyskać informacje o wartościach stanu połączenia, zobacz [Wyliczenie ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="a6683-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="a6683-260">`Closed`: wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="a6683-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="a6683-261">Na przykład, jeśli chcesz wyświetlać komunikaty ostrzegawcze dla błędów, które nie są krytyczne, ale powodują sporadyczne problemy z połączeniem, takie jak spowolnienie lub częste usuwanie połączenia, obsłuż zdarzenie `ConnectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="a6683-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="a6683-262">Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia w programie sygnalizującym](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="a6683-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="a6683-263">Jak obsłużyć błędy</span><span class="sxs-lookup"><span data-stu-id="a6683-263">How to handle errors</span></span>

<span data-ttu-id="a6683-264">Jeśli nie włączysz jawnie szczegółowych komunikatów o błędach na serwerze, obiekt wyjątku zwracany przez sygnalizujący po wystąpieniu błędu zawiera minimalne informacje o błędzie.</span><span class="sxs-lookup"><span data-stu-id="a6683-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="a6683-265">Na przykład jeśli wywołanie `newContosoChatMessage` nie powiedzie się, komunikat o błędzie w obiekcie Error zawiera "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" wysyłanie szczegółowych komunikatów o błędach do klientów w środowisku produkcyjnym nie jest zalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach w celu rozwiązywania problemów, użyj następującego kodu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a6683-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="a6683-266">Aby obsłużyć błędy, które wywołuje program sygnalizujący, można dodać procedurę obsługi dla zdarzenia `Error` w obiekcie Connection.</span><span class="sxs-lookup"><span data-stu-id="a6683-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="a6683-267">Aby obsłużyć błędy wywołań metod, zawiń kod w bloku try-catch.</span><span class="sxs-lookup"><span data-stu-id="a6683-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="a6683-268">Jak włączyć rejestrowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="a6683-268">How to enable client-side logging</span></span>

<span data-ttu-id="a6683-269">Aby włączyć rejestrowanie po stronie klienta, należy ustawić właściwości `TraceLevel` i `TraceWriter` w obiekcie Connection.</span><span class="sxs-lookup"><span data-stu-id="a6683-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="a6683-270">Przykłady kodu aplikacji WPF, Silverlight i konsoli dla metod klienta wywoływanych przez serwer</span><span class="sxs-lookup"><span data-stu-id="a6683-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="a6683-271">Przykłady kodu pokazane wcześniej w celu zdefiniowania metod klienta, które mogą być wywoływane przez serwer, mają zastosowanie do klientów WinRT.</span><span class="sxs-lookup"><span data-stu-id="a6683-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="a6683-272">W poniższych przykładach przedstawiono odpowiedni kod dla klientów WPF, Silverlight i aplikacji konsolowych.</span><span class="sxs-lookup"><span data-stu-id="a6683-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="a6683-273">Metody bez parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-273">Methods without parameters</span></span>

<span data-ttu-id="a6683-274">**Kod klienta WPF dla metody wywoływanej z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="a6683-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="a6683-275">**Kod klienta Silverlight dla metody wywoływanej z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="a6683-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="a6683-276">**Kod klienta aplikacji konsolowej dla metody wywoływanej z serwera bez parametrów**</span><span class="sxs-lookup"><span data-stu-id="a6683-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="a6683-277">Metody z parametrami, Określanie typów parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="a6683-278">**Kod klienta WPF dla metody wywoływanej z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="a6683-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="a6683-279">**Kod klienta Silverlight dla metody wywoływanej z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="a6683-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="a6683-280">**Kod klienta aplikacji konsolowej dla metody wywoływanej z serwera z parametrem**</span><span class="sxs-lookup"><span data-stu-id="a6683-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="a6683-281">Metody z parametrami, określając dynamiczne obiekty parametrów</span><span class="sxs-lookup"><span data-stu-id="a6683-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="a6683-282">**Kod klienta WPF dla metody wywoływanej z serwera z parametrem przy użyciu obiektu dynamicznego dla parametru**</span><span class="sxs-lookup"><span data-stu-id="a6683-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="a6683-283">**Kod klienta Silverlight dla metody wywoływanej z serwera z parametrem przy użyciu obiektu dynamicznego dla parametru**</span><span class="sxs-lookup"><span data-stu-id="a6683-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="a6683-284">**Kod klienta aplikacji konsolowej dla metody wywoływanej z serwera z parametrem przy użyciu obiektu dynamicznego dla parametru**</span><span class="sxs-lookup"><span data-stu-id="a6683-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
