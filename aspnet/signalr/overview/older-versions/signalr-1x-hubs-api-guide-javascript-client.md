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
# <a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="7cc1c-103">Podręcznik interfejsu API centrów usługi SignalR 1.x — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="7cc1c-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>

<span data-ttu-id="7cc1c-104">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7cc1c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="7cc1c-105">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 1,1 w klientach JavaScript, takich jak przeglądarki i aplikacje ze sklepu Windows (WinJS).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="7cc1c-106">Interfejs API centrów sygnałów umożliwia wykonywanie zdalnych wywołań procedur (RPC) z serwera do podłączonych klientów i od klientów do serwera programu.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="7cc1c-107">W polu kod serwera można zdefiniować metody, które mogą być wywoływane przez klientów, i wywoływanie metod uruchamianych na kliencie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="7cc1c-108">W kodzie klienta należy zdefiniować metody, które mogą być wywoływane z serwera programu, i wywoływanie metod, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="7cc1c-109">Sygnalizujący, że wszystkie instalacje z klientem do serwera są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="7cc1c-110">Sygnalizujący oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="7cc1c-111">Aby zapoznać się z wprowadzeniem do sygnałów, centrów i połączeń trwałych lub samouczka, w którym pokazano, jak utworzyć kompletną aplikację sygnalizującą, zobacz [sygnalizującer-wprowadzenie](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="7cc1c-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7cc1c-112">Overview</span></span>

<span data-ttu-id="7cc1c-113">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="7cc1c-114">Wygenerowany serwer proxy i jego działanie</span><span class="sxs-lookup"><span data-stu-id="7cc1c-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="7cc1c-115">Kiedy używać wygenerowanego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="7cc1c-116">Konfiguracja klienta</span><span class="sxs-lookup"><span data-stu-id="7cc1c-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="7cc1c-117">Jak odwołać się do dynamicznie generowanego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="7cc1c-118">Jak utworzyć plik fizyczny dla wygenerowanego przez sygnał serwera proxy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="7cc1c-119">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="7cc1c-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="7cc1c-120">$. Connection. Hub jest tym samym obiektem, który $. hubConnection () tworzy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="7cc1c-121">Asynchroniczne wykonywanie metody Start</span><span class="sxs-lookup"><span data-stu-id="7cc1c-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="7cc1c-122">Jak nawiązać połączenie między domenami</span><span class="sxs-lookup"><span data-stu-id="7cc1c-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="7cc1c-123">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="7cc1c-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="7cc1c-124">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="7cc1c-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="7cc1c-125">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="7cc1c-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="7cc1c-126">Jak uzyskać serwer proxy dla klasy centrum</span><span class="sxs-lookup"><span data-stu-id="7cc1c-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="7cc1c-127">Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer</span><span class="sxs-lookup"><span data-stu-id="7cc1c-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="7cc1c-128">Jak wywoływać metody serwera z klienta</span><span class="sxs-lookup"><span data-stu-id="7cc1c-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="7cc1c-129">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="7cc1c-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="7cc1c-130">Jak obsłużyć błędy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="7cc1c-131">Jak włączyć rejestrowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="7cc1c-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="7cc1c-132">Aby uzyskać dokumentację dotyczącą sposobu programowania serwera lub klientów platformy .NET, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="7cc1c-133">Przewodnik interfejsu API centrów sygnałów — serwer</span><span class="sxs-lookup"><span data-stu-id="7cc1c-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="7cc1c-134">Przewodnik interfejsu API centrów sygnałów — klient platformy .NET</span><span class="sxs-lookup"><span data-stu-id="7cc1c-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="7cc1c-135">Łącza do tematów dotyczących odwołań do interfejsów API znajdują się w wersji programu .NET 4,5 interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="7cc1c-136">Jeśli używasz programu .NET 4, zobacz [temat wersja interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="7cc1c-137">Wygenerowany serwer proxy i jego działanie</span><span class="sxs-lookup"><span data-stu-id="7cc1c-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="7cc1c-138">Możesz programować klienta JavaScript, aby komunikować się z usługą sygnalizującą z serwerem proxy lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="7cc1c-139">Co to jest serwer proxy upraszcza składnię kodu używanego do nawiązywania połączeń, metody zapisu, które serwer wywołuje, i wywołania metod na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="7cc1c-140">Podczas pisania kodu do wywoływania metod serwera wygenerowany serwer proxy umożliwia użycie składni, która wygląda tak, jakby była wykonywana funkcja lokalna: można napisać `serverMethod(arg1, arg2)` zamiast `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="7cc1c-141">Wygenerowana składnia serwera proxy umożliwia również natychmiastowe i zrozumiały błąd po stronie klienta, jeśli wpiszesz nazwę metody serwera.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="7cc1c-142">A jeśli ręcznie utworzysz plik, który definiuje serwery proxy, możesz również uzyskać obsługę technologii IntelliSense do pisania kodu, który wywołuje metody serwera.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="7cc1c-143">Załóżmy na przykład, że na serwerze znajduje się następująca Klasa centrum:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="7cc1c-144">W poniższych przykładach kodu przedstawiono kod JavaScript, który wygląda na sposób wywoływania metody `NewContosoChatMessage` na serwerze i odbieranie wywołań metody `addContosoChatMessageToPage` z serwera.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="7cc1c-145">**Z wygenerowanym serwerem proxy**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="7cc1c-146">**Bez wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="7cc1c-147">Kiedy używać wygenerowanego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-147">When to use the generated proxy</span></span>

<span data-ttu-id="7cc1c-148">Jeśli chcesz zarejestrować wiele obsługi zdarzeń dla metody klienta, którą wywoła serwer, nie możesz użyć wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="7cc1c-149">W przeciwnym razie można wybrać użycie wygenerowanego serwera proxy lub nie na podstawie preferencji kodowania.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="7cc1c-150">Jeśli zdecydujesz się nie używać, nie musisz odwoływać się do adresu URL "sygnalizującer/Hub" w `script` w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="7cc1c-151">Konfiguracja klienta</span><span class="sxs-lookup"><span data-stu-id="7cc1c-151">Client setup</span></span>

<span data-ttu-id="7cc1c-152">Klient JavaScript wymaga odwołań do jQuery i podstawowego pliku języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="7cc1c-153">Wersja jQuery musi być 1.6.4 lub większością nowszych wersji, takich jak 1.7.2, 1.8.2 lub 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="7cc1c-154">Jeśli zdecydujesz się użyć wygenerowanego serwera proxy, musisz również odwołać się do pliku JavaScript wygenerowanego przez Sygnałer.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="7cc1c-155">Poniższy przykład pokazuje, jakie odwołania mogą wyglądać na stronie HTML używającej wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="7cc1c-156">Te odwołania muszą być zawarte w tej kolejności: najpierw jQuery, rdzeń sygnalizujący po tym, a serwery proxy sygnalizujące ostatnie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="7cc1c-157">Jak odwołać się do dynamicznie generowanego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="7cc1c-158">W poprzednim przykładzie odwołanie do wygenerowanego przez sygnalizującego serwera proxy ma dynamicznie generowany kod JavaScript, a nie plik fizyczny.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="7cc1c-159">Program sygnalizujący tworzy kod JavaScript dla serwera proxy na bieżąco i służy do obsługi klienta w odpowiedzi na adres URL "/SignalR/Hubs".</span><span class="sxs-lookup"><span data-stu-id="7cc1c-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="7cc1c-160">Jeśli określono inny podstawowy adres URL dla połączeń sygnalizujących na serwerze w metodzie `MapHubs`, adres URL dla dynamicznie generowanego pliku proxy to niestandardowy adres URL z dołączonym do niego elementem "/Hubs".</span><span class="sxs-lookup"><span data-stu-id="7cc1c-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="7cc1c-161">W przypadku klientów JavaScript systemu Windows 8 (sklepu Windows) Użyj fizycznego pliku serwera proxy zamiast dynamicznie generowanego.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="7cc1c-162">Aby uzyskać więcej informacji, zobacz [jak utworzyć plik fizyczny dla wygenerowanego przez program sygnalizujący serwer proxy](#manualproxy) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="7cc1c-163">W widoku Razor ASP.NET MVC 4, użyj tyldy, aby odwołać się do katalogu głównego aplikacji w dokumentacji pliku proxy:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="7cc1c-164">Aby uzyskać więcej informacji o korzystaniu z programu sygnalizującego w MVC 4, zobacz [wprowadzenie with signaler and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="7cc1c-165">W widoku Razor ASP.NET MVC 3 Użyj `Url.Content` do odwołania do pliku serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="7cc1c-166">W aplikacji ASP.NET Web Forms Użyj `ResolveClientUrl` do odwołania do pliku proxy lub Zarejestruj ją za pośrednictwem ScriptManager przy użyciu ścieżki względnej głównej aplikacji (rozpoczynając od tyldy):</span><span class="sxs-lookup"><span data-stu-id="7cc1c-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="7cc1c-167">Ogólnie rzecz biorąc, Użyj tej samej metody, aby określić adres URL "/SignalR/Hubs", który jest używany dla plików CSS lub JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="7cc1c-168">Jeśli określisz adres URL bez korzystania z tyldy, w niektórych scenariuszach aplikacja będzie działać poprawnie podczas testowania w programie Visual Studio przy użyciu IIS Express, ale wystąpi błąd 404 podczas wdrażania do pełnych usług IIS.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="7cc1c-169">Aby uzyskać więcej informacji, zobacz **rozpoznawanie odwołań do zasobów na poziomie głównym** na [serwerach sieci Web w programie Visual Studio for ASP.NET — projekty sieci Web](https://msdn.microsoft.com/library/58wxa9w5.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="7cc1c-170">Po uruchomieniu projektu sieci Web w programie Visual Studio 2012 w trybie debugowania, a jeśli używasz programu Internet Explorer jako przeglądarki, plik proxy można zobaczyć w **Eksplorator rozwiązań** w obszarze **dokumenty skryptu**, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Plik proxy wygenerowany w języku JavaScript w Eksplorator rozwiązań](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="7cc1c-172">Aby wyświetlić zawartość pliku, kliknij dwukrotnie pozycję **centra**.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="7cc1c-173">Jeśli nie korzystasz z programu Visual Studio 2012 i przeglądarki Internet Explorer lub jeśli nie jesteś w trybie debugowania, możesz również pobrać zawartość pliku, przechodząc do adresu URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="7cc1c-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="7cc1c-174">Na przykład jeśli witryna jest uruchomiona w `http://localhost:56699`, przejdź do `http://localhost:56699/SignalR/hubs` w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="7cc1c-175">Jak utworzyć plik fizyczny dla wygenerowanego przez sygnał serwera proxy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="7cc1c-176">Jako alternatywę dla dynamicznie generowanego serwera proxy można utworzyć plik fizyczny z kodem serwera proxy i odwołać się do tego pliku.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="7cc1c-177">Można to zrobić na potrzeby kontroli nad zachowaniem buforowania lub tworzenia lub w celu uzyskania IntelliSense podczas kodowania wywołań do metod serwera.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="7cc1c-178">Aby utworzyć plik proxy, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="7cc1c-179">Zainstaluj pakiet NuGet [Microsoft. ASPNET. signaler.](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="7cc1c-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="7cc1c-180">Otwórz wiersz polecenia i przejdź do folderu *Tools* zawierającego plik sygnalizującer. exe.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="7cc1c-181">Folder Tools znajduje się w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="7cc1c-182">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="7cc1c-183">Ścieżka do *biblioteki DLL* jest zazwyczaj folderem *bin* w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="7cc1c-184">To polecenie tworzy plik o nazwie *Server. js* w tym samym folderze co *sygnalizującer. exe*.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="7cc1c-185">Umieść plik *Server. js* w odpowiednim folderze w projekcie, zmień jego nazwę na odpowiedni dla aplikacji i Dodaj odwołanie do niego zamiast odwołania "sygnalizującer/Hub".</span><span class="sxs-lookup"><span data-stu-id="7cc1c-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="7cc1c-186">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="7cc1c-186">How to establish a connection</span></span>

<span data-ttu-id="7cc1c-187">Przed nawiązaniem połączenia należy utworzyć obiekt połączenia, utworzyć serwer proxy i zarejestrować obsługę zdarzeń dla metod, które mogą być wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="7cc1c-188">Po skonfigurowaniu programu obsługi serwera proxy i zdarzeń Ustanów połączenie, wywołując metodę `start`.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="7cc1c-189">Jeśli używasz wygenerowanego serwera proxy, nie musisz tworzyć obiektu połączenia w własnym kodzie, ponieważ wygenerowany kod serwera proxy robi to za Ciebie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="7cc1c-190">**Nawiązywanie połączenia (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="7cc1c-191">**Nawiązywanie połączenia (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="7cc1c-192">Przykładowy kod używa domyślnego adresu URL "/SignalR", aby nawiązać połączenie z usługą sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="7cc1c-193">Aby uzyskać informacje na temat sposobu określania innego podstawowego adresu URL, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="7cc1c-194">Zwykle należy zarejestrować procedury obsługi zdarzeń przed wywołaniem metody `start` w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="7cc1c-195">Jeśli chcesz zarejestrować niektóre programy obsługi zdarzeń po ustanowieniu połączenia, możesz to zrobić, ale przed wywołaniem metody `start` należy zarejestrować co najmniej jeden z obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="7cc1c-196">Jedną z przyczyn tego problemu jest to, że w aplikacji może być wiele centrów, ale nie chcesz wyzwalać zdarzenia `OnConnected` w każdym centrum, jeśli zamierzasz korzystać tylko z jednego z nich.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="7cc1c-197">Gdy połączenie zostanie nawiązane, obecność metody klienta na serwerze proxy centrum wskazuje, co informuje sygnalizujący, aby wyzwolić zdarzenie `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="7cc1c-198">Jeśli nie zarejestrowano żadnych programów obsługi zdarzeń przed wywołaniem metody `start`, będzie można wywołać metody w centrum, ale metoda `OnConnected` centrum nie zostanie wywołana i nie będą wywoływane metody klienta z serwera.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="7cc1c-199">$. Connection. Hub jest tym samym obiektem, który $. hubConnection () tworzy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="7cc1c-200">Jak widać w przykładach, gdy używasz wygenerowanego serwera proxy, `$.connection.hub` odwołuje się do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="7cc1c-201">Jest to ten sam obiekt, który uzyskuje się, wywołując `$.hubConnection()`, gdy nie korzystasz z wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="7cc1c-202">Wygenerowany kod serwera proxy tworzy połączenie przez wykonanie następującej instrukcji:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Tworzenie połączenia w wygenerowanym pliku serwera proxy](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="7cc1c-204">W przypadku korzystania z wygenerowanego serwera proxy można wykonać dowolne czynności `$.connection.hub`, które można wykonać przy użyciu obiektu połączenia, gdy nie używasz wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="7cc1c-205">Asynchroniczne wykonywanie metody Start</span><span class="sxs-lookup"><span data-stu-id="7cc1c-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="7cc1c-206">Metoda `start` jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="7cc1c-207">Zwraca [odroczony obiekt jQuery](http://api.jquery.com/category/deferred-object/), co oznacza, że można dodawać funkcje wywołania zwrotnego przez wywoływanie metod, takich jak `pipe`, `done`i `fail`.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="7cc1c-208">Jeśli masz kod, który chcesz wykonać po nawiązaniu połączenia, na przykład wywołanie metody serwera, Umieść ten kod w funkcji wywołania zwrotnego lub wywołaj ją z funkcji wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="7cc1c-209">Metoda wywołania zwrotnego `.done` jest wykonywana po nawiązaniu połączenia i po każdym kodzie, który znajduje się w metodzie obsługi zdarzeń `OnConnected` na serwerze kończy wykonywanie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="7cc1c-210">Jeśli umieścisz instrukcję "teraz połączony" z poprzedniego przykładu jako następny wiersz kodu po wywołaniu metody `start` (nie w `.done` wywołaniu zwrotnym), wiersz `console.log` zostanie wykonany przed nawiązaniem połączenia, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Niewłaściwy sposób pisania kodu, który jest uruchamiany po ustanowieniu połączenia](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="7cc1c-212">Jak nawiązać połączenie między domenami</span><span class="sxs-lookup"><span data-stu-id="7cc1c-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="7cc1c-213">Zazwyczaj Jeśli przeglądarka ładuje stronę `http://contoso.com`, połączenie sygnalizujące znajduje się w tej samej domenie, w `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="7cc1c-214">Jeśli strona z `http://contoso.com` nawiązać połączenie z `http://fabrikam.com/signalr`, to jest połączenie między domenami.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="7cc1c-215">Ze względów bezpieczeństwa połączenia między domenami są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="7cc1c-216">Aby ustanowić połączenie między domenami, upewnij się, że na serwerze są włączone połączenia między domenami, a następnie określ adres URL połączenia podczas tworzenia obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="7cc1c-217">Sygnalizujący będzie używać odpowiedniej technologii dla połączeń międzydomenowych, takich jak [JSONP](http://en.wikipedia.org/wiki/JSONP) lub [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="7cc1c-218">Na serwerze Włącz połączenia między domenami, wybierając tę opcję po wywołaniu metody `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="7cc1c-219">Na kliencie Określ adres URL podczas tworzenia obiektu połączenia (bez wygenerowanego serwera proxy) lub przed wywołaniem metody Start (z wygenerowanym serwerem proxy).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="7cc1c-220">**Kod klienta określający połączenie między domenami (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="7cc1c-221">**Kod klienta określający połączenie między domenami (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="7cc1c-222">W przypadku korzystania z konstruktora `$.hubConnection` nie trzeba uwzględniać `signalr` w adresie URL, ponieważ jest on automatycznie dodawany (chyba że określisz `useDefaultUrl` jako `false`).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="7cc1c-223">Można utworzyć wiele połączeń z różnymi punktami końcowymi.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="7cc1c-224">Nie ustawiaj `jQuery.support.cors` na true w kodzie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Nie ustawiaj jQuery. support. CORS na true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="7cc1c-226">Sygnał obsługuje użycie JSONP lub CORS.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="7cc1c-227">Ustawienie wartości true dla `jQuery.support.cors` powoduje wyłączenie funkcji JSONP, ponieważ powoduje to założenie, że przeglądarka obsługuje mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="7cc1c-228">W przypadku nawiązywania połączenia z adresem URL hosta lokalnego program Internet Explorer 10 nie będzie uwzględniać połączenia między domenami, dzięki czemu aplikacja będzie działała lokalnie z programem IE 10, nawet jeśli na serwerze nie włączono połączeń między domenami.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="7cc1c-229">Aby uzyskać informacje o korzystaniu z połączeń między domenami w programie Internet Explorer 9, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="7cc1c-230">Aby uzyskać informacje o korzystaniu z połączeń między domenami z programem Chrome, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="7cc1c-231">Przykładowy kod używa domyślnego adresu URL "/SignalR", aby nawiązać połączenie z usługą sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="7cc1c-232">Aby uzyskać informacje na temat sposobu określania innego podstawowego adresu URL, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="7cc1c-233">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="7cc1c-233">How to configure the connection</span></span>

<span data-ttu-id="7cc1c-234">Przed nawiązaniem połączenia można określić parametry ciągu zapytania lub określić metodę transportu.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="7cc1c-235">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="7cc1c-235">How to specify query string parameters</span></span>

<span data-ttu-id="7cc1c-236">Jeśli chcesz wysłać dane do serwera podczas łączenia się z klientem, możesz dodać parametry ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="7cc1c-237">W poniższych przykładach pokazano, jak ustawić parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="7cc1c-238">**Ustaw wartość ciągu zapytania przed wywołaniem metody Start (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="7cc1c-239">**Ustaw wartość ciągu zapytania przed wywołaniem metody Start (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="7cc1c-240">Poniższy przykład pokazuje, jak odczytać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="7cc1c-241">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="7cc1c-241">How to specify the transport method</span></span>

<span data-ttu-id="7cc1c-242">W ramach procesu łączenia klient sygnalizujący zwykle negocjuje z serwerem, aby określić najlepszy transport obsługiwany przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="7cc1c-243">Jeśli wiesz już, którego transportu chcesz użyć, możesz pominąć ten proces negocjacji, określając metodę transportu, gdy wywołasz metodę `start`.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="7cc1c-244">**Kod klienta określający metodę transportu (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="7cc1c-245">**Kod klienta określający metodę transportu (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="7cc1c-246">Alternatywnie można określić wiele metod transportu w kolejności, w której ma być wykorzystana przez program sygnalizujący:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="7cc1c-247">**Kod klienta określający niestandardowy schemat powrotu do transportu (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="7cc1c-248">**Kod klienta określający niestandardowy schemat powrotu do transportu (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="7cc1c-249">Aby określić metodę transportu, można użyć następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="7cc1c-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="7cc1c-250">"webSockets"</span></span>
- <span data-ttu-id="7cc1c-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="7cc1c-251">"foreverFrame"</span></span>
- <span data-ttu-id="7cc1c-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="7cc1c-252">"serverSentEvents"</span></span>
- <span data-ttu-id="7cc1c-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="7cc1c-253">"longPolling"</span></span>

<span data-ttu-id="7cc1c-254">W poniższych przykładach pokazano, jak ustalić, która metoda transportu jest używana przez połączenie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="7cc1c-255">**Kod klienta, który wyświetla metodę transportu używaną przez połączenie (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="7cc1c-256">**Kod klienta, który wyświetla metodę transportu używaną przez połączenie (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="7cc1c-257">Aby uzyskać informacje na temat sprawdzania metody transportu w kodzie serwera, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server — jak uzyskać informacje o kliencie z właściwości Context](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="7cc1c-258">Aby uzyskać więcej informacji na temat transportów i rezerw, zobacz [wprowadzenie do sygnałów-Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="7cc1c-259">Jak uzyskać serwer proxy dla klasy centrum</span><span class="sxs-lookup"><span data-stu-id="7cc1c-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="7cc1c-260">Każdy utworzony obiekt połączenia hermetyzuje informacje o połączeniu z usługą sygnalizującą, która zawiera jedną lub więcej klas centrów.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="7cc1c-261">Aby komunikować się z klasą centrum, należy użyć obiektu serwera proxy, który utworzysz samodzielnie (jeśli nie używasz wygenerowanego serwera proxy) lub który jest generowany dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="7cc1c-262">Na kliencie nazwa serwera proxy to notacji camelcasea wersja klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="7cc1c-263">Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="7cc1c-264">**Klasa centrum na serwerze**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="7cc1c-265">**Pobierz odwołanie do wygenerowanego serwera proxy klienta dla centrum**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="7cc1c-266">**Utwórz serwer proxy klienta dla klasy centrum (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="7cc1c-267">Jeśli dekorować klasę centrów z atrybutem `HubName`, użyj dokładnej nazwy bez zmieniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="7cc1c-268">**Klasa Hub na serwerze z atrybutem HubName**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="7cc1c-269">**Pobierz odwołanie do wygenerowanego serwera proxy klienta dla centrum**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="7cc1c-270">**Utwórz serwer proxy klienta dla klasy centrum (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="7cc1c-271">Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer</span><span class="sxs-lookup"><span data-stu-id="7cc1c-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="7cc1c-272">Aby zdefiniować metodę, którą serwer może wywoływać z centrum, Dodaj procedurę obsługi zdarzeń do serwera proxy centrum przy użyciu właściwości `client` wygenerowanego serwera proxy lub wywołaj metodę `on`, jeśli nie używasz wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="7cc1c-273">Parametry mogą być obiektami złożonymi.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="7cc1c-274">Dodaj program obsługi zdarzeń przed wywołaniem metody `start` w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="7cc1c-275">(Jeśli chcesz dodać programy obsługi zdarzeń po wywołaniu metody `start`, zapoznaj się z uwagami dotyczącymi [sposobu nawiązywania połączenia](#establishconnection) we wcześniejszej części tego dokumentu i użyj składni pokazanej w celu zdefiniowania metody bez użycia wygenerowanego serwera proxy).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="7cc1c-276">W dopasowaniu nazw metod nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="7cc1c-277">Na przykład `Clients.All.addContosoChatMessageToPage` na serwerze wykona `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`lub `addcontosochatmessagetopage` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="7cc1c-278">**Zdefiniuj metodę na kliencie (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="7cc1c-279">**Alternatywny sposób definiowania metody na kliencie (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="7cc1c-280">**Zdefiniuj metodę na kliencie (bez wygenerowanego serwera proxy lub podczas dodawania po wywołaniu metody startowej)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="7cc1c-281">**Kod serwera, który wywołuje metodę klienta**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="7cc1c-282">Następujące przykłady zawierają obiekt złożony jako parametr metody.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="7cc1c-283">**Zdefiniuj metodę na kliencie, która pobiera obiekt złożony (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="7cc1c-284">**Zdefiniuj metodę na kliencie, która pobiera obiekt złożony (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="7cc1c-285">**Kod serwera, który definiuje obiekt złożony**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="7cc1c-286">**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="7cc1c-287">Jak wywoływać metody serwera z klienta</span><span class="sxs-lookup"><span data-stu-id="7cc1c-287">How to call server methods from the client</span></span>

<span data-ttu-id="7cc1c-288">Aby wywołać metodę serwera z poziomu klienta, należy użyć właściwości `server` wygenerowanego serwera proxy lub metody `invoke` na serwerze proxy centrum, jeśli nie używasz wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="7cc1c-289">Wartość zwracana lub parametry mogą być obiektami złożonymi.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="7cc1c-290">Przekaż notacji camelcaseą nazwę metody w centrum.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="7cc1c-291">Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="7cc1c-292">W poniższych przykładach pokazano, jak wywołać metodę serwera, która nie ma wartości zwracanej i jak wywołać metodę serwera, która ma wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="7cc1c-293">**Metoda serwera bez atrybutu HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="7cc1c-294">**Kod serwera, który definiuje złożony obiekt przekazaną w parametrze**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="7cc1c-295">**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="7cc1c-296">**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="7cc1c-297">Jeśli zawarto metodę Hub z atrybutem `HubMethodName`, Użyj tej nazwy bez zmieniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="7cc1c-298">**Metoda serwera** z atrybutem HubMethodName</span><span class="sxs-lookup"><span data-stu-id="7cc1c-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="7cc1c-299">**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="7cc1c-300">**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="7cc1c-301">W powyższych przykładach pokazano, jak wywołać metodę serwera, która nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="7cc1c-302">W poniższych przykładach pokazano, jak wywołać metodę serwera, która ma wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="7cc1c-303">**Kod serwera dla metody, która ma wartość zwracaną**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="7cc1c-304">**Klasa magazynowa używana dla** wartości zwracanej</span><span class="sxs-lookup"><span data-stu-id="7cc1c-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="7cc1c-305">**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="7cc1c-306">**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="7cc1c-307">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="7cc1c-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="7cc1c-308">Program sygnalizujący udostępnia następujące zdarzenia okresu istnienia połączenia, które można obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="7cc1c-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="7cc1c-309">`starting`: uruchamiany przed wysłaniem danych przez połączenie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="7cc1c-310">`received`: wywoływane po odebraniu dowolnego danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="7cc1c-311">Dostarcza odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-311">Provides the received data.</span></span>
- <span data-ttu-id="7cc1c-312">`connectionSlow`: wywoływane, gdy klient wykrywa powolne lub często porzucane połączenie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="7cc1c-313">`reconnecting`: uruchamiany, gdy podstawowy transport zacznie ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="7cc1c-314">`reconnected`: wywoływane, gdy podstawowy transport został połączony ponownie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="7cc1c-315">`stateChanged`: występuje, gdy zmieni się stan połączenia.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="7cc1c-316">Zapewnia stary stan i nowy stan (połączenie, połączenie, ponowne połączenie lub odłączenie).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="7cc1c-317">`disconnected`: wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="7cc1c-318">Na przykład jeśli chcesz wyświetlać komunikaty ostrzegawcze w przypadku problemów z połączeniem, które mogą powodować zauważalne opóźnienia, obsłuż zdarzenie `connectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="7cc1c-319">**Obsługuj zdarzenie connectionSlow (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="7cc1c-320">**Obsługuj zdarzenie connectionSlow (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="7cc1c-321">Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia w programie sygnalizującym](index.md).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="7cc1c-322">Jak obsłużyć błędy</span><span class="sxs-lookup"><span data-stu-id="7cc1c-322">How to handle errors</span></span>

<span data-ttu-id="7cc1c-323">Klient JavaScript sygnalizujący udostępnia zdarzenie `error`, do którego można dodać program obsługi dla.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="7cc1c-324">Można również użyć metody niepowodzenia, aby dodać procedurę obsługi dla błędów wynikających z wywołania metody serwera.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="7cc1c-325">Jeśli nie włączysz jawnie szczegółowych komunikatów o błędach na serwerze, obiekt wyjątku zwracany przez sygnalizujący po wystąpieniu błędu zawiera minimalne informacje o błędzie.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="7cc1c-326">Na przykład jeśli wywołanie `newContosoChatMessage` nie powiedzie się, komunikat o błędzie w obiekcie Error zawiera "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" wysyłanie szczegółowych komunikatów o błędach do klientów w środowisku produkcyjnym nie jest zalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach w celu rozwiązywania problemów, użyj następującego kodu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="7cc1c-327">Poniższy przykład pokazuje, jak dodać procedurę obsługi dla zdarzenia błędu.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="7cc1c-328">**Dodaj procedurę obsługi błędów (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="7cc1c-329">**Dodaj program obsługi błędów (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="7cc1c-330">Poniższy przykład pokazuje, jak obsłużyć błąd z wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="7cc1c-331">**Obsługa błędu z wywołania metody (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="7cc1c-332">**Obsługa błędu z wywołania metody (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="7cc1c-333">Jeśli wywołanie metody nie powiedzie się, zostanie również zgłoszone zdarzenie `error`, więc kod w obsłudze metody `error` i w wywołaniu metody `.fail` może zostać wykonany.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="7cc1c-334">Jak włączyć rejestrowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="7cc1c-334">How to enable client-side logging</span></span>

<span data-ttu-id="7cc1c-335">Aby włączyć rejestrowanie po stronie klienta dla połączenia, należy ustawić właściwość `logging` obiektu połączenia przed wywołaniem metody `start` w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="7cc1c-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="7cc1c-336">**Włącz rejestrowanie (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="7cc1c-337">**Włącz rejestrowanie (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="7cc1c-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="7cc1c-338">Aby wyświetlić dzienniki, Otwórz narzędzia deweloperskie w przeglądarce i przejdź do karty konsola. Samouczek przedstawiający instrukcje krok po kroku i zrzuty ekranu, które pokazują, jak to zrobić, można znaleźć [w temacie emisja serwera za pomocą sygnału ASP.NET — włączenie rejestrowania](index.md).</span><span class="sxs-lookup"><span data-stu-id="7cc1c-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
