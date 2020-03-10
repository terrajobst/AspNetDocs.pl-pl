---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Przewodnik interfejsu API centrów sygnałów ASP.NET — klient JavaScript | Microsoft Docs
author: bradygaster
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 2 w klientach JavaScript, takich jak przeglądarki i Sklep Windows (WinJS) applicat...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536660"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="4eb1e-103">Przewodnik interfejsu API centrów sygnałów ASP.NET — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="4eb1e-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="4eb1e-104">Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 2 w klientach JavaScript, takich jak przeglądarki i aplikacje ze sklepu Windows (WinJS).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="4eb1e-105">Interfejs API centrów sygnałów umożliwia wykonywanie zdalnych wywołań procedur (RPC) z serwera do podłączonych klientów i od klientów do serwera programu.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="4eb1e-106">W polu kod serwera można zdefiniować metody, które mogą być wywoływane przez klientów, i wywoływanie metod uruchamianych na kliencie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="4eb1e-107">W kodzie klienta należy zdefiniować metody, które mogą być wywoływane z serwera programu, i wywoływanie metod, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="4eb1e-108">Sygnalizujący, że wszystkie instalacje z klientem do serwera są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="4eb1e-109">Sygnalizujący oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="4eb1e-110">Aby zapoznać się z wprowadzeniem do sygnałów, centrów i połączeń trwałych, zobacz [wprowadzenie do usługi sygnalizującer](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="4eb1e-111">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="4eb1e-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="4eb1e-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4eb1e-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="4eb1e-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4eb1e-113">.NET 4.5</span></span>
> - <span data-ttu-id="4eb1e-114">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="4eb1e-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="4eb1e-115">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="4eb1e-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="4eb1e-116">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="4eb1e-117">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="4eb1e-117">Questions and comments</span></span>
>
> <span data-ttu-id="4eb1e-118">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4eb1e-119">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="4eb1e-120">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4eb1e-120">Overview</span></span>

<span data-ttu-id="4eb1e-121">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="4eb1e-122">Wygenerowany serwer proxy i jego działanie</span><span class="sxs-lookup"><span data-stu-id="4eb1e-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="4eb1e-123">Kiedy używać wygenerowanego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="4eb1e-124">Konfiguracja klienta</span><span class="sxs-lookup"><span data-stu-id="4eb1e-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="4eb1e-125">Jak odwołać się do dynamicznie generowanego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="4eb1e-126">Jak utworzyć plik fizyczny dla wygenerowanego przez sygnał serwera proxy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="4eb1e-127">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="4eb1e-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="4eb1e-128">$. Connection. Hub jest tym samym obiektem, który $. hubConnection () tworzy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="4eb1e-129">Asynchroniczne wykonywanie metody Start</span><span class="sxs-lookup"><span data-stu-id="4eb1e-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="4eb1e-130">Jak nawiązać połączenie między domenami</span><span class="sxs-lookup"><span data-stu-id="4eb1e-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="4eb1e-131">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="4eb1e-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="4eb1e-132">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="4eb1e-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="4eb1e-133">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="4eb1e-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="4eb1e-134">Jak uzyskać serwer proxy dla klasy centrum</span><span class="sxs-lookup"><span data-stu-id="4eb1e-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="4eb1e-135">Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer</span><span class="sxs-lookup"><span data-stu-id="4eb1e-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="4eb1e-136">Jak wywoływać metody serwera z klienta</span><span class="sxs-lookup"><span data-stu-id="4eb1e-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="4eb1e-137">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="4eb1e-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="4eb1e-138">Jak obsłużyć błędy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="4eb1e-139">Jak włączyć rejestrowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="4eb1e-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="4eb1e-140">Aby uzyskać dokumentację dotyczącą sposobu programowania serwera lub klientów platformy .NET, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="4eb1e-141">Przewodnik interfejsu API centrów sygnałów — serwer</span><span class="sxs-lookup"><span data-stu-id="4eb1e-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="4eb1e-142">Przewodnik interfejsu API centrów sygnałów — klient platformy .NET</span><span class="sxs-lookup"><span data-stu-id="4eb1e-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="4eb1e-143">Składnik sygnalizujący 2 serwer jest dostępny tylko w programie .NET 4,5 (chociaż na platformie .NET 4,0 istnieje klient .NET).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="4eb1e-144">Wygenerowany serwer proxy i jego działanie</span><span class="sxs-lookup"><span data-stu-id="4eb1e-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="4eb1e-145">Możesz programować klienta JavaScript, aby komunikować się z usługą sygnalizującą z serwerem proxy lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="4eb1e-146">Co to jest serwer proxy upraszcza składnię kodu używanego do nawiązywania połączeń, metody zapisu, które serwer wywołuje, i wywołania metod na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="4eb1e-147">Podczas pisania kodu do wywoływania metod serwera wygenerowany serwer proxy umożliwia użycie składni, która wygląda tak, jakby była wykonywana funkcja lokalna: można napisać `serverMethod(arg1, arg2)` zamiast `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="4eb1e-148">Wygenerowana składnia serwera proxy umożliwia również natychmiastowe i zrozumiały błąd po stronie klienta, jeśli wpiszesz nazwę metody serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="4eb1e-149">A jeśli ręcznie utworzysz plik, który definiuje serwery proxy, możesz również uzyskać obsługę technologii IntelliSense do pisania kodu, który wywołuje metody serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="4eb1e-150">Załóżmy na przykład, że na serwerze znajduje się następująca Klasa centrum:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="4eb1e-151">W poniższych przykładach kodu przedstawiono kod JavaScript, który wygląda na sposób wywoływania metody `NewContosoChatMessage` na serwerze i odbieranie wywołań metody `addContosoChatMessageToPage` z serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="4eb1e-152">**Z wygenerowanym serwerem proxy**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="4eb1e-153">**Bez wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="4eb1e-154">Kiedy używać wygenerowanego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-154">When to use the generated proxy</span></span>

<span data-ttu-id="4eb1e-155">Jeśli chcesz zarejestrować wiele obsługi zdarzeń dla metody klienta, którą wywoła serwer, nie możesz użyć wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="4eb1e-156">W przeciwnym razie można wybrać użycie wygenerowanego serwera proxy lub nie na podstawie preferencji kodowania.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="4eb1e-157">Jeśli zdecydujesz się nie używać, nie musisz odwoływać się do adresu URL "sygnalizującer/Hub" w `script` w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="4eb1e-158">Konfiguracja klienta</span><span class="sxs-lookup"><span data-stu-id="4eb1e-158">Client setup</span></span>

<span data-ttu-id="4eb1e-159">Klient JavaScript wymaga odwołań do jQuery i podstawowego pliku języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="4eb1e-160">Wersja jQuery musi być 1.6.4 lub większością nowszych wersji, takich jak 1.7.2, 1.8.2 lub 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="4eb1e-161">Jeśli zdecydujesz się użyć wygenerowanego serwera proxy, musisz również odwołać się do pliku JavaScript wygenerowanego przez Sygnałer.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="4eb1e-162">Poniższy przykład pokazuje, jakie odwołania mogą wyglądać na stronie HTML używającej wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="4eb1e-163">Te odwołania muszą być zawarte w tej kolejności: najpierw jQuery, rdzeń sygnalizujący po tym, a serwery proxy sygnalizujące ostatnie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="4eb1e-164">Jak odwołać się do dynamicznie generowanego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="4eb1e-165">W poprzednim przykładzie odwołanie do wygenerowanego przez sygnalizującego serwera proxy ma dynamicznie generowany kod JavaScript, a nie plik fizyczny.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="4eb1e-166">Program sygnalizujący tworzy kod JavaScript dla serwera proxy na bieżąco i służy do obsługi klienta w odpowiedzi na adres URL "/SignalR/Hubs".</span><span class="sxs-lookup"><span data-stu-id="4eb1e-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="4eb1e-167">Jeśli określono inny podstawowy adres URL dla połączeń sygnalizujących na serwerze w metodzie `MapSignalR`, adres URL dla dynamicznie generowanego pliku proxy to niestandardowy adres URL z dołączonym do niego elementem "/Hubs".</span><span class="sxs-lookup"><span data-stu-id="4eb1e-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="4eb1e-168">W przypadku klientów JavaScript systemu Windows 8 (sklepu Windows) Użyj fizycznego pliku serwera proxy zamiast dynamicznie generowanego.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="4eb1e-169">Aby uzyskać więcej informacji, zobacz [jak utworzyć plik fizyczny dla wygenerowanego przez program sygnalizujący serwer proxy](#manualproxy) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="4eb1e-170">W widoku Razor ASP.NET MVC 4 lub 5, użyj tyldy, aby odwołać się do katalogu głównego aplikacji w dokumentacji pliku proxy:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="4eb1e-171">Aby uzyskać więcej informacji o korzystaniu z programu sygnalizującego w MVC 5, zobacz [wprowadzenie z sygnalizującer i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="4eb1e-172">W widoku Razor ASP.NET MVC 3 Użyj `Url.Content` do odwołania do pliku serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="4eb1e-173">W aplikacji ASP.NET Web Forms Użyj `ResolveClientUrl` do odwołania do pliku proxy lub Zarejestruj ją za pośrednictwem ScriptManager przy użyciu ścieżki względnej głównej aplikacji (rozpoczynając od tyldy):</span><span class="sxs-lookup"><span data-stu-id="4eb1e-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="4eb1e-174">Ogólnie rzecz biorąc, Użyj tej samej metody, aby określić adres URL "/SignalR/Hubs", który jest używany dla plików CSS lub JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="4eb1e-175">Jeśli określisz adres URL bez korzystania z tyldy, w niektórych scenariuszach aplikacja będzie działać poprawnie podczas testowania w programie Visual Studio przy użyciu IIS Express, ale wystąpi błąd 404 podczas wdrażania do pełnych usług IIS.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="4eb1e-176">Aby uzyskać więcej informacji, zobacz **rozpoznawanie odwołań do zasobów na poziomie głównym** na [serwerach sieci Web w programie Visual Studio for ASP.NET — projekty sieci Web](https://msdn.microsoft.com/library/58wxa9w5.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="4eb1e-177">Po uruchomieniu projektu sieci Web w programie Visual Studio 2017 w trybie debugowania, a jeśli używasz programu Internet Explorer jako przeglądarki, plik proxy można zobaczyć w **Eksplorator rozwiązań** w obszarze **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="4eb1e-178">Aby wyświetlić zawartość pliku, kliknij dwukrotnie pozycję **centra**.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="4eb1e-179">Jeśli nie korzystasz z programu Visual Studio 2012 lub 2013 i przeglądarki Internet Explorer lub jeśli nie jesteś w trybie debugowania, możesz również pobrać zawartość pliku, przechodząc do adresu URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="4eb1e-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="4eb1e-180">Na przykład jeśli witryna jest uruchomiona w `http://localhost:56699`, przejdź do `http://localhost:56699/SignalR/hubs` w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="4eb1e-181">Jak utworzyć plik fizyczny dla wygenerowanego przez sygnał serwera proxy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="4eb1e-182">Jako alternatywę dla dynamicznie generowanego serwera proxy można utworzyć plik fizyczny z kodem serwera proxy i odwołać się do tego pliku.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="4eb1e-183">Można to zrobić na potrzeby kontroli nad zachowaniem buforowania lub tworzenia lub w celu uzyskania IntelliSense podczas kodowania wywołań do metod serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="4eb1e-184">Aby utworzyć plik proxy, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="4eb1e-185">Zainstaluj pakiet NuGet [Microsoft. ASPNET. signaler.](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="4eb1e-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="4eb1e-186">Otwórz wiersz polecenia i przejdź do folderu *Tools* zawierającego plik sygnalizującer. exe.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="4eb1e-187">Folder Tools znajduje się w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="4eb1e-188">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="4eb1e-189">Ścieżka do *biblioteki DLL* jest zazwyczaj folderem *bin* w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="4eb1e-190">To polecenie tworzy plik o nazwie *Server. js* w tym samym folderze co *sygnalizującer. exe*.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="4eb1e-191">Umieść plik *Server. js* w odpowiednim folderze w projekcie, zmień jego nazwę na odpowiedni dla aplikacji i Dodaj odwołanie do niego zamiast odwołania "sygnalizującer/Hub".</span><span class="sxs-lookup"><span data-stu-id="4eb1e-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="4eb1e-192">Jak nawiązać połączenie</span><span class="sxs-lookup"><span data-stu-id="4eb1e-192">How to establish a connection</span></span>

<span data-ttu-id="4eb1e-193">Przed nawiązaniem połączenia należy utworzyć obiekt połączenia, utworzyć serwer proxy i zarejestrować obsługę zdarzeń dla metod, które mogą być wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="4eb1e-194">Po skonfigurowaniu programu obsługi serwera proxy i zdarzeń Ustanów połączenie, wywołując metodę `start`.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="4eb1e-195">Jeśli używasz wygenerowanego serwera proxy, nie musisz tworzyć obiektu połączenia w własnym kodzie, ponieważ wygenerowany kod serwera proxy robi to za Ciebie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="4eb1e-196">**Nawiązywanie połączenia (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="4eb1e-197">**Nawiązywanie połączenia (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="4eb1e-198">Przykładowy kod używa domyślnego adresu URL "/SignalR", aby nawiązać połączenie z usługą sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="4eb1e-199">Aby uzyskać informacje na temat sposobu określania innego podstawowego adresu URL, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="4eb1e-200">Domyślnie lokalizacja centrum to bieżący serwer; Jeśli łączysz się z innym serwerem, określ adres URL przed wywołaniem metody `start`, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="4eb1e-201">Zwykle należy zarejestrować procedury obsługi zdarzeń przed wywołaniem metody `start` w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="4eb1e-202">Jeśli chcesz zarejestrować niektóre programy obsługi zdarzeń po ustanowieniu połączenia, możesz to zrobić, ale przed wywołaniem metody `start` należy zarejestrować co najmniej jeden z obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="4eb1e-203">Jedną z przyczyn tego problemu jest to, że w aplikacji może być wiele centrów, ale nie chcesz wyzwalać zdarzenia `OnConnected` w każdym centrum, jeśli zamierzasz korzystać tylko z jednego z nich.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="4eb1e-204">Gdy połączenie zostanie nawiązane, obecność metody klienta na serwerze proxy centrum wskazuje, co informuje sygnalizujący, aby wyzwolić zdarzenie `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="4eb1e-205">Jeśli nie zarejestrowano żadnych programów obsługi zdarzeń przed wywołaniem metody `start`, będzie można wywołać metody w centrum, ale metoda `OnConnected` centrum nie zostanie wywołana i nie będą wywoływane metody klienta z serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="4eb1e-206">$. Connection. Hub jest tym samym obiektem, który $. hubConnection () tworzy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="4eb1e-207">Jak widać w przykładach, gdy używasz wygenerowanego serwera proxy, `$.connection.hub` odwołuje się do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="4eb1e-208">Jest to ten sam obiekt, który uzyskuje się, wywołując `$.hubConnection()`, gdy nie korzystasz z wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="4eb1e-209">Wygenerowany kod serwera proxy tworzy połączenie przez wykonanie następującej instrukcji:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Tworzenie połączenia w wygenerowanym pliku serwera proxy](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="4eb1e-211">W przypadku korzystania z wygenerowanego serwera proxy można wykonać dowolne czynności `$.connection.hub`, które można wykonać przy użyciu obiektu połączenia, gdy nie używasz wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="4eb1e-212">Asynchroniczne wykonywanie metody Start</span><span class="sxs-lookup"><span data-stu-id="4eb1e-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="4eb1e-213">Metoda `start` jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="4eb1e-214">Zwraca [odroczony obiekt jQuery](http://api.jquery.com/category/deferred-object/), co oznacza, że można dodawać funkcje wywołania zwrotnego przez wywoływanie metod, takich jak `pipe`, `done`i `fail`.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="4eb1e-215">Jeśli masz kod, który chcesz wykonać po nawiązaniu połączenia, na przykład wywołanie metody serwera, Umieść ten kod w funkcji wywołania zwrotnego lub wywołaj ją z funkcji wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="4eb1e-216">Metoda wywołania zwrotnego `.done` jest wykonywana po nawiązaniu połączenia i po każdym kodzie, który znajduje się w metodzie obsługi zdarzeń `OnConnected` na serwerze kończy wykonywanie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="4eb1e-217">Jeśli umieścisz instrukcję "teraz połączony" z poprzedniego przykładu jako następny wiersz kodu po wywołaniu metody `start` (nie w `.done` wywołaniu zwrotnym), wiersz `console.log` zostanie wykonany przed nawiązaniem połączenia, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Niewłaściwy sposób pisania kodu, który jest uruchamiany po ustanowieniu połączenia](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="4eb1e-219">Jak nawiązać połączenie między domenami</span><span class="sxs-lookup"><span data-stu-id="4eb1e-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="4eb1e-220">Zazwyczaj Jeśli przeglądarka ładuje stronę `http://contoso.com`, połączenie sygnalizujące znajduje się w tej samej domenie, w `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="4eb1e-221">Jeśli strona z `http://contoso.com` nawiązać połączenie z `http://fabrikam.com/signalr`, to jest połączenie między domenami.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="4eb1e-222">Ze względów bezpieczeństwa połączenia między domenami są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="4eb1e-223">W sygnale 1. x żądania między domenami były kontrolowane przez jedną flagę EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="4eb1e-224">Ta flaga steruje żądaniami JSONP i CORS.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="4eb1e-225">Aby zapewnić większą elastyczność, cała obsługa mechanizmu CORS została usunięta ze składnika serwerowego sygnalizującego (klienci języka JavaScript nadal używają mechanizmu CORS normalnie, jeśli wykryje, że jest on obsługiwany przez przeglądarkę), a nowe oprogramowanie pośredniczące OWIN zostało udostępnione do obsługi tych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="4eb1e-226">Jeśli na kliencie jest wymagane JSONP (aby obsługiwać żądania między domenami we wcześniejszych przeglądarkach), należy je włączyć jawnie przez ustawienie `EnableJSONP` na obiekcie `HubConfiguration`, aby `true`, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="4eb1e-227">JSONP jest domyślnie wyłączona, ponieważ jest mniej bezpieczny niż mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="4eb1e-228">**Dodawanie Microsoft. Owin. CORS do projektu:** Aby zainstalować tę bibliotekę, uruchom następujące polecenie w konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="4eb1e-229">To polecenie spowoduje dodanie wersji 2.1.0 pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="4eb1e-230">Wywoływanie UseCors</span><span class="sxs-lookup"><span data-stu-id="4eb1e-230">Calling UseCors</span></span>

 <span data-ttu-id="4eb1e-231">Poniższy fragment kodu przedstawia sposób implementacji połączeń między domenami w programie sygnalizującym 2.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="4eb1e-232">**Implementowanie żądań międzydomenowych w sygnale 2**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="4eb1e-233">Poniższy kod ilustruje sposób włączania funkcji CORS lub JSONP w projekcie sygnalizującym 2.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="4eb1e-234">Ten przykładowy kod używa `Map` i `RunSignalR` zamiast `MapSignalR`, dzięki czemu oprogramowanie typu CORS działa tylko dla żądań sygnalizujących, które wymagają obsługi mechanizmu CORS (a nie dla całego ruchu w ścieżce określonej w `MapSignalR`). Mapy można także użyć dla dowolnego innego oprogramowania pośredniczącego, które musi zostać uruchomione dla określonego prefiksu adresu URL, a nie dla całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="4eb1e-235">Nie ustawiaj `jQuery.support.cors` na true w kodzie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Nie ustawiaj jQuery. support. CORS na true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="4eb1e-237">Program sygnalizujący obsługuje korzystanie z mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="4eb1e-238">Ustawienie wartości true dla `jQuery.support.cors` powoduje wyłączenie funkcji JSONP, ponieważ powoduje to założenie, że przeglądarka obsługuje mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="4eb1e-239">W przypadku nawiązywania połączenia z adresem URL hosta lokalnego program Internet Explorer 10 nie będzie uwzględniać połączenia między domenami, dzięki czemu aplikacja będzie działała lokalnie z programem IE 10, nawet jeśli na serwerze nie włączono połączeń między domenami.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="4eb1e-240">Aby uzyskać informacje o korzystaniu z połączeń między domenami w programie Internet Explorer 9, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="4eb1e-241">Aby uzyskać informacje o korzystaniu z połączeń między domenami z programem Chrome, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="4eb1e-242">Przykładowy kod używa domyślnego adresu URL "/SignalR", aby nawiązać połączenie z usługą sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="4eb1e-243">Aby uzyskać informacje na temat sposobu określania innego podstawowego adresu URL, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="4eb1e-244">Jak skonfigurować połączenie</span><span class="sxs-lookup"><span data-stu-id="4eb1e-244">How to configure the connection</span></span>

<span data-ttu-id="4eb1e-245">Przed nawiązaniem połączenia można określić parametry ciągu zapytania lub określić metodę transportu.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="4eb1e-246">Jak określić parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="4eb1e-246">How to specify query string parameters</span></span>

<span data-ttu-id="4eb1e-247">Jeśli chcesz wysłać dane do serwera podczas łączenia się z klientem, możesz dodać parametry ciągu zapytania do obiektu połączenia.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="4eb1e-248">W poniższych przykładach pokazano, jak ustawić parametr ciągu zapytania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="4eb1e-249">**Ustaw wartość ciągu zapytania przed wywołaniem metody Start (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="4eb1e-250">**Ustaw wartość ciągu zapytania przed wywołaniem metody Start (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="4eb1e-251">Poniższy przykład pokazuje, jak odczytać parametr ciągu zapytania w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="4eb1e-252">Jak określić metodę transportu</span><span class="sxs-lookup"><span data-stu-id="4eb1e-252">How to specify the transport method</span></span>

<span data-ttu-id="4eb1e-253">W ramach procesu łączenia klient sygnalizujący zwykle negocjuje z serwerem, aby określić najlepszy transport obsługiwany przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="4eb1e-254">Jeśli wiesz już, którego transportu chcesz użyć, możesz pominąć ten proces negocjacji, określając metodę transportu, gdy wywołasz metodę `start`.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="4eb1e-255">**Kod klienta określający metodę transportu (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="4eb1e-256">**Kod klienta określający metodę transportu (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="4eb1e-257">Alternatywnie można określić wiele metod transportu w kolejności, w której ma być wykorzystana przez program sygnalizujący:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="4eb1e-258">**Kod klienta określający niestandardowy schemat powrotu do transportu (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="4eb1e-259">**Kod klienta określający niestandardowy schemat powrotu do transportu (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="4eb1e-260">Aby określić metodę transportu, można użyć następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="4eb1e-261">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="4eb1e-261">"webSockets"</span></span>
- <span data-ttu-id="4eb1e-262">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="4eb1e-262">"foreverFrame"</span></span>
- <span data-ttu-id="4eb1e-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="4eb1e-263">"serverSentEvents"</span></span>
- <span data-ttu-id="4eb1e-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="4eb1e-264">"longPolling"</span></span>

<span data-ttu-id="4eb1e-265">W poniższych przykładach pokazano, jak ustalić, która metoda transportu jest używana przez połączenie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="4eb1e-266">**Kod klienta, który wyświetla metodę transportu używaną przez połączenie (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="4eb1e-267">**Kod klienta, który wyświetla metodę transportu używaną przez połączenie (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="4eb1e-268">Aby uzyskać informacje na temat sprawdzania metody transportu w kodzie serwera, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server — jak uzyskać informacje o kliencie z właściwości Context](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="4eb1e-269">Aby uzyskać więcej informacji na temat transportów i rezerw, zobacz [wprowadzenie do sygnałów-Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="4eb1e-270">Jak uzyskać serwer proxy dla klasy centrum</span><span class="sxs-lookup"><span data-stu-id="4eb1e-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="4eb1e-271">Każdy utworzony obiekt połączenia hermetyzuje informacje o połączeniu z usługą sygnalizującą, która zawiera jedną lub więcej klas centrów.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="4eb1e-272">Aby komunikować się z klasą centrum, należy użyć obiektu serwera proxy, który utworzysz samodzielnie (jeśli nie używasz wygenerowanego serwera proxy) lub który jest generowany dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="4eb1e-273">Na kliencie nazwa serwera proxy to notacji camelcasea wersja klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="4eb1e-274">Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="4eb1e-275">**Klasa centrum na serwerze**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="4eb1e-276">**Pobierz odwołanie do wygenerowanego serwera proxy klienta dla centrum**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="4eb1e-277">**Utwórz serwer proxy klienta dla klasy centrum (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="4eb1e-278">Jeśli dekorować klasę centrów z atrybutem `HubName`, użyj dokładnej nazwy bez zmieniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="4eb1e-279">**Klasa Hub na serwerze z atrybutem HubName**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="4eb1e-280">**Pobierz odwołanie do wygenerowanego serwera proxy klienta dla centrum**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="4eb1e-281">**Utwórz serwer proxy klienta dla klasy centrum (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="4eb1e-282">Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer</span><span class="sxs-lookup"><span data-stu-id="4eb1e-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="4eb1e-283">Aby zdefiniować metodę, którą serwer może wywoływać z centrum, Dodaj procedurę obsługi zdarzeń do serwera proxy centrum przy użyciu właściwości `client` wygenerowanego serwera proxy lub wywołaj metodę `on`, jeśli nie używasz wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="4eb1e-284">Parametry mogą być obiektami złożonymi.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="4eb1e-285">Dodaj program obsługi zdarzeń przed wywołaniem metody `start` w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="4eb1e-286">(Jeśli chcesz dodać programy obsługi zdarzeń po wywołaniu metody `start`, zapoznaj się z uwagami dotyczącymi [sposobu nawiązywania połączenia](#establishconnection) we wcześniejszej części tego dokumentu i użyj składni pokazanej w celu zdefiniowania metody bez użycia wygenerowanego serwera proxy).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="4eb1e-287">W dopasowaniu nazw metod nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="4eb1e-288">Na przykład `Clients.All.addContosoChatMessageToPage` na serwerze wykona `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`lub `addcontosochatmessagetopage` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="4eb1e-289">**Zdefiniuj metodę na kliencie (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="4eb1e-290">**Alternatywny sposób definiowania metody na kliencie (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="4eb1e-291">**Zdefiniuj metodę na kliencie (bez wygenerowanego serwera proxy lub podczas dodawania po wywołaniu metody startowej)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="4eb1e-292">**Kod serwera, który wywołuje metodę klienta**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="4eb1e-293">Następujące przykłady zawierają obiekt złożony jako parametr metody.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="4eb1e-294">**Zdefiniuj metodę na kliencie, która pobiera obiekt złożony (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="4eb1e-295">**Zdefiniuj metodę na kliencie, która pobiera obiekt złożony (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="4eb1e-296">**Kod serwera, który definiuje obiekt złożony**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="4eb1e-297">**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="4eb1e-298">Jak wywoływać metody serwera z klienta</span><span class="sxs-lookup"><span data-stu-id="4eb1e-298">How to call server methods from the client</span></span>

<span data-ttu-id="4eb1e-299">Aby wywołać metodę serwera z poziomu klienta, należy użyć właściwości `server` wygenerowanego serwera proxy lub metody `invoke` na serwerze proxy centrum, jeśli nie używasz wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="4eb1e-300">Wartość zwracana lub parametry mogą być obiektami złożonymi.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="4eb1e-301">Przekaż notacji camelcaseą nazwę metody w centrum.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="4eb1e-302">Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="4eb1e-303">W poniższych przykładach pokazano, jak wywołać metodę serwera, która nie ma wartości zwracanej i jak wywołać metodę serwera, która ma wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="4eb1e-304">**Metoda serwera bez atrybutu HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="4eb1e-305">**Kod serwera, który definiuje złożony obiekt przekazaną w parametrze**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="4eb1e-306">**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="4eb1e-307">**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="4eb1e-308">Jeśli zawarto metodę Hub z atrybutem `HubMethodName`, Użyj tej nazwy bez zmieniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="4eb1e-309">**Metoda serwera** z atrybutem HubMethodName</span><span class="sxs-lookup"><span data-stu-id="4eb1e-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="4eb1e-310">**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="4eb1e-311">**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="4eb1e-312">W powyższych przykładach pokazano, jak wywołać metodę serwera, która nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="4eb1e-313">W poniższych przykładach pokazano, jak wywołać metodę serwera, która ma wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="4eb1e-314">**Kod serwera dla metody, która ma wartość zwracaną**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="4eb1e-315">**Klasa magazynowa używana dla** wartości zwracanej</span><span class="sxs-lookup"><span data-stu-id="4eb1e-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="4eb1e-316">**Kod klienta wywołujący metodę serwera (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="4eb1e-317">**Kod klienta wywołujący metodę serwera (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="4eb1e-318">Jak obsługiwać zdarzenia okresu istnienia połączenia</span><span class="sxs-lookup"><span data-stu-id="4eb1e-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="4eb1e-319">Program sygnalizujący udostępnia następujące zdarzenia okresu istnienia połączenia, które można obsłużyć:</span><span class="sxs-lookup"><span data-stu-id="4eb1e-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="4eb1e-320">`starting`: uruchamiany przed wysłaniem danych przez połączenie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="4eb1e-321">`received`: wywoływane po odebraniu dowolnego danych w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="4eb1e-322">Dostarcza odebrane dane.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-322">Provides the received data.</span></span>
- <span data-ttu-id="4eb1e-323">`connectionSlow`: wywoływane, gdy klient wykrywa powolne lub często porzucane połączenie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="4eb1e-324">`reconnecting`: uruchamiany, gdy podstawowy transport zacznie ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="4eb1e-325">`reconnected`: wywoływane, gdy podstawowy transport został połączony ponownie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="4eb1e-326">`stateChanged`: występuje, gdy zmieni się stan połączenia.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="4eb1e-327">Zapewnia stary stan i nowy stan (połączenie, połączenie, ponowne połączenie lub odłączenie).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="4eb1e-328">`disconnected`: wywoływane, gdy połączenie zostało rozłączone.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="4eb1e-329">Na przykład jeśli chcesz wyświetlać komunikaty ostrzegawcze w przypadku problemów z połączeniem, które mogą powodować zauważalne opóźnienia, obsłuż zdarzenie `connectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="4eb1e-330">**Obsługuj zdarzenie connectionSlow (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="4eb1e-331">**Obsługuj zdarzenie connectionSlow (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="4eb1e-332">Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia w programie sygnalizującym](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="4eb1e-333">Jak obsłużyć błędy</span><span class="sxs-lookup"><span data-stu-id="4eb1e-333">How to handle errors</span></span>

<span data-ttu-id="4eb1e-334">Klient JavaScript sygnalizujący udostępnia zdarzenie `error`, do którego można dodać program obsługi dla.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="4eb1e-335">Można również użyć metody niepowodzenia, aby dodać procedurę obsługi dla błędów wynikających z wywołania metody serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="4eb1e-336">Jeśli nie włączysz jawnie szczegółowych komunikatów o błędach na serwerze, obiekt wyjątku zwracany przez sygnalizujący po wystąpieniu błędu zawiera minimalne informacje o błędzie.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="4eb1e-337">Na przykład jeśli wywołanie `newContosoChatMessage` nie powiedzie się, komunikat o błędzie w obiekcie Error zawiera "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" wysyłanie szczegółowych komunikatów o błędach do klientów w środowisku produkcyjnym nie jest zalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach w celu rozwiązywania problemów, użyj następującego kodu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="4eb1e-338">Poniższy przykład pokazuje, jak dodać procedurę obsługi dla zdarzenia błędu.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="4eb1e-339">**Dodaj procedurę obsługi błędów (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="4eb1e-340">**Dodaj program obsługi błędów (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="4eb1e-341">Poniższy przykład pokazuje, jak obsłużyć błąd z wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="4eb1e-342">**Obsługa błędu z wywołania metody (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="4eb1e-343">**Obsługa błędu z wywołania metody (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="4eb1e-344">Jeśli wywołanie metody nie powiedzie się, zostanie również zgłoszone zdarzenie `error`, więc kod w obsłudze metody `error` i w wywołaniu metody `.fail` może zostać wykonany.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="4eb1e-345">Jak włączyć rejestrowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="4eb1e-345">How to enable client-side logging</span></span>

<span data-ttu-id="4eb1e-346">Aby włączyć rejestrowanie po stronie klienta dla połączenia, należy ustawić właściwość `logging` obiektu połączenia przed wywołaniem metody `start` w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="4eb1e-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="4eb1e-347">**Włącz rejestrowanie (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="4eb1e-348">**Włącz rejestrowanie (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="4eb1e-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="4eb1e-349">Aby wyświetlić dzienniki, Otwórz narzędzia deweloperskie w przeglądarce i przejdź do karty konsola. Samouczek przedstawiający instrukcje krok po kroku i zrzuty ekranu, które pokazują, jak to zrobić, można znaleźć [w temacie emisja serwera za pomocą sygnału ASP.NET — włączenie rejestrowania](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="4eb1e-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
