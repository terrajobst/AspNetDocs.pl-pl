---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Przewodnik interfejsu API centrów sygnałów ASP.NET — serwerC#() | Microsoft Docs
author: bradygaster
description: Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API centrów ASP.NETer dla usługi sygnalizującego w wersji 2, z przykładami kodu...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536751"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="61f0c-103">Przewodnik interfejsu API centrów sygnałów ASP.NET — serwerC#()</span><span class="sxs-lookup"><span data-stu-id="61f0c-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>

<span data-ttu-id="61f0c-104">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="61f0c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="61f0c-105">Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API centrów ASP.NETer dla usługi sygnalizującego w wersji 2, z przykładami kodu pokazującymi typowe opcje.</span><span class="sxs-lookup"><span data-stu-id="61f0c-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="61f0c-106">Interfejs API centrów sygnałów umożliwia wykonywanie zdalnych wywołań procedur (RPC) z serwera do podłączonych klientów i od klientów do serwera programu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="61f0c-107">W polu kod serwera można zdefiniować metody, które mogą być wywoływane przez klientów, i wywoływanie metod uruchamianych na kliencie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="61f0c-108">W kodzie klienta należy zdefiniować metody, które mogą być wywoływane z serwera programu, i wywoływanie metod, które są uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="61f0c-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="61f0c-109">Sygnalizujący, że wszystkie instalacje z klientem do serwera są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="61f0c-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="61f0c-110">Sygnalizujący oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="61f0c-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="61f0c-111">Aby zapoznać się z wprowadzeniem do sygnałów, centrów i połączeń trwałych, zobacz [wprowadzenie do sygnalizującego 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="61f0c-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="61f0c-112">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="61f0c-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="61f0c-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="61f0c-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="61f0c-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="61f0c-114">.NET 4.5</span></span>
> - <span data-ttu-id="61f0c-115">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="61f0c-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="61f0c-116">Wersje tematów</span><span class="sxs-lookup"><span data-stu-id="61f0c-116">Topic versions</span></span>
> 
> <span data-ttu-id="61f0c-117">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="61f0c-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="61f0c-118">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="61f0c-118">Questions and comments</span></span>
> 
> <span data-ttu-id="61f0c-119">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="61f0c-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="61f0c-120">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="61f0c-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="61f0c-121">Omówienie</span><span class="sxs-lookup"><span data-stu-id="61f0c-121">Overview</span></span>

<span data-ttu-id="61f0c-122">Ten dokument zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="61f0c-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="61f0c-123">Jak zarejestrować oprogramowanie pośredniczące sygnalizujące</span><span class="sxs-lookup"><span data-stu-id="61f0c-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="61f0c-124">Adres URL/SignalR</span><span class="sxs-lookup"><span data-stu-id="61f0c-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="61f0c-125">Konfigurowanie opcji sygnalizujących</span><span class="sxs-lookup"><span data-stu-id="61f0c-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="61f0c-126">Tworzenie i używanie klas centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="61f0c-127">Okres istnienia obiektu centrum</span><span class="sxs-lookup"><span data-stu-id="61f0c-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="61f0c-128">Notacji CamelCase — wielkość liter w nazwach centrów w klientach JavaScript</span><span class="sxs-lookup"><span data-stu-id="61f0c-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="61f0c-129">Wiele centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="61f0c-130">Centra o jednoznacznie określonym typie</span><span class="sxs-lookup"><span data-stu-id="61f0c-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="61f0c-131">Jak zdefiniować metody w klasie centrów, które mogą być wywoływane przez klientów</span><span class="sxs-lookup"><span data-stu-id="61f0c-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="61f0c-132">Notacji CamelCase — wielkość liter w nazwach metod w klientach JavaScript</span><span class="sxs-lookup"><span data-stu-id="61f0c-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="61f0c-133">Kiedy wykonać asynchronicznie</span><span class="sxs-lookup"><span data-stu-id="61f0c-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="61f0c-134">Definiowanie przeciążeń</span><span class="sxs-lookup"><span data-stu-id="61f0c-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="61f0c-135">Raportowanie postępu z wywołań metod centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="61f0c-136">Jak wywołać metody klienta z klasy Hub</span><span class="sxs-lookup"><span data-stu-id="61f0c-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="61f0c-137">Wybieranie klientów, którzy będą otrzymywać wywołania RPC</span><span class="sxs-lookup"><span data-stu-id="61f0c-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="61f0c-138">Brak weryfikacji w czasie kompilacji dla nazw metod</span><span class="sxs-lookup"><span data-stu-id="61f0c-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="61f0c-139">Dopasowanie nazw metod bez uwzględniania wielkości liter</span><span class="sxs-lookup"><span data-stu-id="61f0c-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="61f0c-140">Wykonywanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="61f0c-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="61f0c-141">Zarządzanie członkostwem w grupie z klasy centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="61f0c-142">Asynchroniczne wykonywanie metod Add i Remove</span><span class="sxs-lookup"><span data-stu-id="61f0c-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="61f0c-143">Trwałość członkostwa w grupie</span><span class="sxs-lookup"><span data-stu-id="61f0c-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="61f0c-144">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="61f0c-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="61f0c-145">Jak obsłużyć zdarzenia okresu istnienia połączenia w klasie centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="61f0c-146">Gdy są wywoływane metody OnConnected, ondisconnected i OnReconnected</span><span class="sxs-lookup"><span data-stu-id="61f0c-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="61f0c-147">Stan wywołującego nie został wypełniony</span><span class="sxs-lookup"><span data-stu-id="61f0c-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="61f0c-148">Jak uzyskać informacje o kliencie z właściwości kontekstu</span><span class="sxs-lookup"><span data-stu-id="61f0c-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="61f0c-149">Jak przekazać stan między klientami a klasą centrum</span><span class="sxs-lookup"><span data-stu-id="61f0c-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="61f0c-150">Jak obsłużyć błędy w klasie centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="61f0c-151">Jak wywoływać metody klienta i zarządzać grupami spoza klasy Hub</span><span class="sxs-lookup"><span data-stu-id="61f0c-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="61f0c-152">Wywoływanie metod klienta</span><span class="sxs-lookup"><span data-stu-id="61f0c-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="61f0c-153">Zarządzanie członkostwem w grupie</span><span class="sxs-lookup"><span data-stu-id="61f0c-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="61f0c-154">Jak włączyć śledzenie</span><span class="sxs-lookup"><span data-stu-id="61f0c-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="61f0c-155">Jak dostosować potok centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="61f0c-156">Aby uzyskać dokumentację dotyczącą sposobu programowania klientów, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="61f0c-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="61f0c-157">Przewodnik interfejsu API centrów sygnałów — klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="61f0c-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="61f0c-158">Przewodnik interfejsu API centrów sygnałów — klient platformy .NET</span><span class="sxs-lookup"><span data-stu-id="61f0c-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="61f0c-159">Składniki serwera dla sygnalizującego 2 są dostępne tylko w programie .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="61f0c-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="61f0c-160">Na serwerach z programem .NET 4,0 musi być używany program sygnalizujący v1. x.</span><span class="sxs-lookup"><span data-stu-id="61f0c-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="61f0c-161">Jak zarejestrować oprogramowanie pośredniczące sygnalizujące</span><span class="sxs-lookup"><span data-stu-id="61f0c-161">How to register SignalR middleware</span></span>

<span data-ttu-id="61f0c-162">Aby zdefiniować trasę, która będzie używana przez klientów do łączenia się z centrum, wywołaj metodę `MapSignalR` podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61f0c-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="61f0c-163">`MapSignalR` jest [metodą rozszerzającą](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) dla klasy `OwinExtensions`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="61f0c-164">Poniższy przykład pokazuje, jak zdefiniować trasę centrów sygnałów przy użyciu klasy startowej OWIN.</span><span class="sxs-lookup"><span data-stu-id="61f0c-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="61f0c-165">Jeśli dodajesz funkcję sygnalizującą do aplikacji ASP.NET MVC, upewnij się, że trasa sygnalizująca jest dodawana przed innymi trasami.</span><span class="sxs-lookup"><span data-stu-id="61f0c-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="61f0c-166">Aby uzyskać więcej informacji, zobacz [Samouczek: wprowadzenie z sygnałami 2 i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="61f0c-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="61f0c-167">Adres URL/SignalR</span><span class="sxs-lookup"><span data-stu-id="61f0c-167">The /signalr URL</span></span>

<span data-ttu-id="61f0c-168">Domyślnie adres URL trasy używany przez klientów do łączenia się z centrum to "/SignalR".</span><span class="sxs-lookup"><span data-stu-id="61f0c-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="61f0c-169">(Nie należy mylić tego adresu URL z adresem URL "/SignalR/Hubs", który jest przeznaczony dla automatycznie generowanego pliku języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61f0c-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="61f0c-170">Aby uzyskać więcej informacji na temat wygenerowanego serwera proxy, zobacz [Przewodnik interfejsu API centrów sygnałów — klient JavaScript — wygenerowany serwer proxy i jego zawartość](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="61f0c-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="61f0c-171">Mogą występować nadzwyczajne sytuacje, w których ten podstawowy adres URL nie może być użyteczny dla sygnalizującego; na przykład w projekcie znajduje się folder o nazwie *sygnalizujący* i nie chcesz zmieniać nazwy.</span><span class="sxs-lookup"><span data-stu-id="61f0c-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="61f0c-172">W takim przypadku można zmienić podstawowy adres URL, jak pokazano w poniższych przykładach (Zastąp "/SignalR" w przykładowym kodzie z żądanym adresem URL).</span><span class="sxs-lookup"><span data-stu-id="61f0c-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="61f0c-173">**Kod serwera, który określa adres URL**</span><span class="sxs-lookup"><span data-stu-id="61f0c-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="61f0c-174">**Kod klienta JavaScript, który określa adres URL (z wygenerowanym serwerem proxy)**</span><span class="sxs-lookup"><span data-stu-id="61f0c-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="61f0c-175">**Kod klienta JavaScript, który określa adres URL (bez wygenerowanego serwera proxy)**</span><span class="sxs-lookup"><span data-stu-id="61f0c-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="61f0c-176">**Kod klienta .NET, który określa adres URL**</span><span class="sxs-lookup"><span data-stu-id="61f0c-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="61f0c-177">Konfigurowanie opcji sygnalizujących</span><span class="sxs-lookup"><span data-stu-id="61f0c-177">Configuring SignalR Options</span></span>

<span data-ttu-id="61f0c-178">Przeciążenia metody `MapSignalR` umożliwiają określenie niestandardowego adresu URL, niestandardowego programu rozpoznawania zależności oraz następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="61f0c-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="61f0c-179">Włącz wywołania międzydomenowe za pomocą mechanizmu CORS lub JSONP z klientów przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="61f0c-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="61f0c-180">Zazwyczaj Jeśli przeglądarka ładuje stronę `http://contoso.com`, połączenie sygnalizujące znajduje się w tej samej domenie, w `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="61f0c-181">Jeśli strona z `http://contoso.com` nawiązać połączenie z `http://fabrikam.com/signalr`, to jest połączenie między domenami.</span><span class="sxs-lookup"><span data-stu-id="61f0c-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="61f0c-182">Ze względów bezpieczeństwa połączenia między domenami są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="61f0c-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="61f0c-183">Aby uzyskać więcej informacji, zobacz [Przewodnik dotyczący interfejsu API centrów ASP.NET Signals — JavaScript Client — jak ustanowić połączenie między domenami](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="61f0c-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="61f0c-184">Włącz szczegółowe komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="61f0c-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="61f0c-185">Gdy wystąpią błędy, domyślne zachowanie sygnalizującego jest wysyłane do klientów komunikat powiadomienia bez szczegółowych informacji o tym, co się stało.</span><span class="sxs-lookup"><span data-stu-id="61f0c-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="61f0c-186">Wysyłanie szczegółowych informacji o błędach do klientów nie jest zalecane w środowisku produkcyjnym, ponieważ Złośliwi użytkownicy mogą korzystać z informacji w przypadku ataków na aplikację.</span><span class="sxs-lookup"><span data-stu-id="61f0c-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="61f0c-187">W przypadku rozwiązywania problemów można użyć tej opcji, aby tymczasowo włączyć bardziej informacyjne raportowanie błędów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="61f0c-188">Wyłącz automatycznie generowane pliki proxy JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61f0c-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="61f0c-189">Domyślnie plik JavaScript z serwerami proxy dla klas centrów jest generowany w odpowiedzi na adres URL "/SignalR/Hubs".</span><span class="sxs-lookup"><span data-stu-id="61f0c-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="61f0c-190">Jeśli nie chcesz używać serwerów proxy języka JavaScript lub chcesz wygenerować ten plik ręcznie i zapoznaj się z plikiem fizycznym na klientach, możesz użyć tej opcji, aby wyłączyć generowanie serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61f0c-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="61f0c-191">Aby uzyskać więcej informacji, zobacz [Przewodnik interfejsu API centrów sygnałów — JavaScript Client — jak utworzyć plik fizyczny dla generowanego przez program sygnalizującego serwera proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="61f0c-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="61f0c-192">Poniższy przykład pokazuje, jak określić adres URL połączenia sygnalizującego i te opcje w wywołaniu metody `MapSignalR`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="61f0c-193">Aby określić niestandardowy adres URL, Zastąp ciąg "/SignalR" w przykładzie adresem URL, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="61f0c-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="61f0c-194">Tworzenie i używanie klas centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-194">How to create and use Hub classes</span></span>

<span data-ttu-id="61f0c-195">Aby utworzyć centrum, należy utworzyć klasę, która pochodzi od elementu [Microsoft. ASPNET. signaler. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="61f0c-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="61f0c-196">W poniższym przykładzie przedstawiono prostą klasę centrów dla aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="61f0c-197">W tym przykładzie połączony klient może wywoływać metodę `NewContosoChatMessage`, a w przypadku, gdy dane są wysyłane do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="61f0c-198">Okres istnienia obiektu centrum</span><span class="sxs-lookup"><span data-stu-id="61f0c-198">Hub object lifetime</span></span>

<span data-ttu-id="61f0c-199">Nie można utworzyć wystąpienia klasy centrum ani wywołać jej metod z własnego kodu na serwerze; wszystko to wykonywane przez potok centrów sygnałów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="61f0c-200">Program sygnalizujący tworzy nowe wystąpienie klasy centrów za każdym razem, gdy musi obsłużyć operację centrum, taką jak gdy klient łączy się, rozłącza lub wywołuje metodę wywołania do serwera.</span><span class="sxs-lookup"><span data-stu-id="61f0c-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="61f0c-201">Ponieważ wystąpienia klasy centrów są przejściowe, nie można ich używać do utrzymania stanu z jednego wywołania metody do następnego.</span><span class="sxs-lookup"><span data-stu-id="61f0c-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="61f0c-202">Za każdym razem, gdy serwer odbiera wywołanie metody z klienta, nowe wystąpienie klasy Hub przetwarza komunikat.</span><span class="sxs-lookup"><span data-stu-id="61f0c-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="61f0c-203">Aby zachować stan za pośrednictwem wielu połączeń i wywołań metod, należy użyć innej metody, takiej jak baza danych lub zmienna statyczna w klasie Hub, lub innej klasy, która nie pochodzi od `Hub`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="61f0c-204">W przypadku utrwalania danych w pamięci przy użyciu metody takiej jak zmienna statyczna w klasie centrów dane zostaną utracone podczas odtwarzania domeny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61f0c-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="61f0c-205">Jeśli chcesz wysyłać komunikaty do klientów z własnego kodu, który jest uruchamiany poza klasą Hub, nie można tego zrobić przez utworzenie wystąpienia klasy centrum, ale można to zrobić, pobierając odwołanie do obiektu kontekstu sygnalizującego dla klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="61f0c-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="61f0c-206">Aby uzyskać więcej informacji, zobacz [jak wywoływać metody klienta i zarządzać grupami spoza klasy Hub](#callfromoutsidehub) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="61f0c-207">Notacji CamelCase — wielkość liter w nazwach centrów w klientach JavaScript</span><span class="sxs-lookup"><span data-stu-id="61f0c-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="61f0c-208">Domyślnie klienci języka JavaScript odwołują się do centrów przy użyciu notacji camelcaseej wersji klasy.</span><span class="sxs-lookup"><span data-stu-id="61f0c-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="61f0c-209">Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61f0c-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="61f0c-210">Poprzedni przykład jest określany jako `contosoChatHub` w kodzie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61f0c-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="61f0c-211">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="61f0c-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="61f0c-212">**Klient JavaScript korzystający z wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="61f0c-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="61f0c-213">Jeśli chcesz określić inną nazwę dla klientów do użycia, Dodaj atrybut `HubName`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="61f0c-214">W przypadku korzystania z atrybutu `HubName` nie jest zmieniana nazwa notacji CamelCase przypadku na klientach JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61f0c-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="61f0c-215">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="61f0c-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="61f0c-216">**Klient JavaScript korzystający z wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="61f0c-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="61f0c-217">Multiple Hubs</span><span class="sxs-lookup"><span data-stu-id="61f0c-217">Multiple Hubs</span></span>

<span data-ttu-id="61f0c-218">W aplikacji można definiować wiele klas centrów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="61f0c-219">Po wykonaniu tej czynności połączenie jest udostępniane, ale grupy są osobne:</span><span class="sxs-lookup"><span data-stu-id="61f0c-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="61f0c-220">Wszyscy klienci będą używać tego samego adresu URL, aby nawiązać połączenie sygnalizujące z usługą ("/SignalR" lub niestandardowy adres URL, jeśli został określony) i że połączenie jest używane dla wszystkich centrów zdefiniowanych przez usługę.</span><span class="sxs-lookup"><span data-stu-id="61f0c-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="61f0c-221">Nie ma różnicy wydajności dla wielu centrów w porównaniu do definiowania wszystkich funkcji centrum w jednej klasie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="61f0c-222">Wszystkie centra uzyskują te same informacje o żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="61f0c-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="61f0c-223">Ponieważ wszystkie centra korzystają z tego samego połączenia, jedyną informacją o żądaniu HTTP, którą pobiera serwer, jest to, co znajduje się w oryginalnym żądaniu HTTP, które nawiązuje połączenie sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="61f0c-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="61f0c-224">W przypadku użycia żądania połączenia w celu przekazania informacji z klienta do serwera przez określenie ciągu zapytania nie można podać różnych parametrów zapytania do różnych centrów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="61f0c-225">Wszystkie centra będą otrzymywać te same informacje.</span><span class="sxs-lookup"><span data-stu-id="61f0c-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="61f0c-226">Wygenerowany plik proxy języka JavaScript będzie zawierać serwery proxy dla wszystkich centrów w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="61f0c-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="61f0c-227">Aby uzyskać informacje na temat serwerów proxy języka JavaScript, zobacz [Przewodnik interfejsu API centrów sygnałów-JavaScript Client-wygenerowany serwer proxy i jego zawartość](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="61f0c-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="61f0c-228">Grupy są zdefiniowane w centrach.</span><span class="sxs-lookup"><span data-stu-id="61f0c-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="61f0c-229">W programie Sygnalizującer można zdefiniować nazwane grupy do emisji do podzestawów połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="61f0c-230">Grupy są utrzymywane osobno dla każdego centrum.</span><span class="sxs-lookup"><span data-stu-id="61f0c-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="61f0c-231">Na przykład grupa o nazwie "Administratorzy" będzie zawierać jeden zestaw klientów dla klasy `ContosoChatHub`, a ta sama nazwa grupy odwołuje się do innego zestawu klientów dla klasy `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="61f0c-232">Centra o jednoznacznie określonym typie</span><span class="sxs-lookup"><span data-stu-id="61f0c-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="61f0c-233">Aby zdefiniować interfejs dla metod centrów, do których klient może się odwoływać (i włączyć funkcję IntelliSense w metodach centrum), należy utworzyć centrum z `Hub<T>` (wprowadzone w sygnalizacji 2,1), a nie `Hub`:</span><span class="sxs-lookup"><span data-stu-id="61f0c-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="61f0c-234">Jak zdefiniować metody w klasie centrów, które mogą być wywoływane przez klientów</span><span class="sxs-lookup"><span data-stu-id="61f0c-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="61f0c-235">Aby uwidocznić metodę w centrum, która ma być wywoływana z klienta, zadeklaruj metodę publiczną, jak pokazano w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="61f0c-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="61f0c-236">Można określić typ zwracany i parametry, w tym typy złożone i tablice, tak jak w przypadku dowolnej C# metody.</span><span class="sxs-lookup"><span data-stu-id="61f0c-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="61f0c-237">Wszystkie dane, które są odbierane w parametrach lub zwracane do obiektu wywołującego, są przekazywane między klientem a serwerem przy użyciu JSON, a sygnalizujący obsługuje powiązanie obiektów złożonych i tablic obiektów automatycznie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="61f0c-238">Notacji CamelCase — wielkość liter w nazwach metod w klientach JavaScript</span><span class="sxs-lookup"><span data-stu-id="61f0c-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="61f0c-239">Domyślnie klienci języka JavaScript odwołują się do metod centrów przy użyciu notacji camelcaseej wersji metody.</span><span class="sxs-lookup"><span data-stu-id="61f0c-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="61f0c-240">Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61f0c-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="61f0c-241">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="61f0c-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="61f0c-242">**Klient JavaScript korzystający z wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="61f0c-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="61f0c-243">Jeśli chcesz określić inną nazwę dla klientów do użycia, Dodaj atrybut `HubMethodName`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="61f0c-244">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="61f0c-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="61f0c-245">**Klient JavaScript korzystający z wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="61f0c-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="61f0c-246">Kiedy wykonać asynchronicznie</span><span class="sxs-lookup"><span data-stu-id="61f0c-246">When to execute asynchronously</span></span>

<span data-ttu-id="61f0c-247">Jeśli metoda będzie długotrwała lub musi wykonywać zadania, które mogą być zależne, takie jak wyszukiwanie bazy danych lub wywołanie usługi sieci Web, ustaw metodę Hub asynchronicznie, zwracając [zadanie](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (zamiast elementu `void` Return) lub [zadania&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) obiektu (zamiast `T` zwracanego typu).</span><span class="sxs-lookup"><span data-stu-id="61f0c-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="61f0c-248">Po powrocie obiektu `Task` z metody, sygnalizujący czeka na ukończenie `Task`, a następnie wysyła nieopakowany wynik z powrotem do klienta, więc nie ma żadnych różnic w sposobie kodowania wywołania metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="61f0c-249">Stosowanie metody centrum asynchronicznie zapobiega blokowaniu połączenia w przypadku korzystania z transportu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="61f0c-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="61f0c-250">Gdy metoda centrum jest wykonywana synchronicznie, a transport jest w trybie WebSocket, kolejne wywołania metod znajdujących się w koncentratorze z tego samego klienta są blokowane do momentu zakończenia metody centrum.</span><span class="sxs-lookup"><span data-stu-id="61f0c-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="61f0c-251">Poniższy przykład pokazuje tę samą metodę, która jest zakodowana do uruchamiania synchronicznie lub asynchronicznie, po którym następuje kod klienta JavaScript, który działa w przypadku wywoływania jednej wersji.</span><span class="sxs-lookup"><span data-stu-id="61f0c-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="61f0c-252">**Wykonywane**</span><span class="sxs-lookup"><span data-stu-id="61f0c-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="61f0c-253">**Komunikacji**</span><span class="sxs-lookup"><span data-stu-id="61f0c-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="61f0c-254">**Klient JavaScript korzystający z wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="61f0c-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="61f0c-255">Aby uzyskać więcej informacji o sposobach używania metod asynchronicznych w ASP.NET 4,5, zobacz [Używanie metod asynchronicznych w ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="61f0c-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="61f0c-256">Definiowanie przeciążeń</span><span class="sxs-lookup"><span data-stu-id="61f0c-256">Defining Overloads</span></span>

<span data-ttu-id="61f0c-257">Jeśli chcesz zdefiniować przeciążenia dla metody, liczba parametrów w każdym przeciążeniu musi się różnić.</span><span class="sxs-lookup"><span data-stu-id="61f0c-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="61f0c-258">W przypadku odróżnienia przeciążenia po określeniu różnych typów parametrów, Klasa centrum zostanie skompilowana, ale usługa sygnalizująca zgłosi wyjątek w czasie wykonywania, gdy klienci spróbują wywołać jedno z przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="61f0c-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="61f0c-259">Raportowanie postępu z wywołań metod centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="61f0c-260">Sygnał 2,1 dodaje obsługę [wzorca raportowania postępu](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) wprowadzonego w programie .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="61f0c-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="61f0c-261">Aby zaimplementować Raportowanie postępu, zdefiniuj `IProgress<T>` parametr dla metody centrum, do której Twój klient ma dostęp:</span><span class="sxs-lookup"><span data-stu-id="61f0c-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="61f0c-262">Podczas pisania długotrwałej metody serwera należy użyć asynchronicznego wzorca programowania, takiego jak Async/await, zamiast blokować wątek centrum.</span><span class="sxs-lookup"><span data-stu-id="61f0c-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="61f0c-263">Jak wywołać metody klienta z klasy Hub</span><span class="sxs-lookup"><span data-stu-id="61f0c-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="61f0c-264">Aby wywołać metody klienta z serwera, użyj właściwości `Clients` w metodzie klasy Hub.</span><span class="sxs-lookup"><span data-stu-id="61f0c-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="61f0c-265">Poniższy przykład przedstawia kod serwera, który wywołuje `addNewMessageToPage` na wszystkich połączonych klientach i kod klienta, który definiuje metodę w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61f0c-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="61f0c-266">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="61f0c-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="61f0c-267">Wywołanie metody klienta jest operacją asynchroniczną i zwraca `Task`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-267">Invoking a client method is an asynchronous operation and returns a `Task`.</span></span> <span data-ttu-id="61f0c-268">Użyj `await`:</span><span class="sxs-lookup"><span data-stu-id="61f0c-268">Use `await`:</span></span>

* <span data-ttu-id="61f0c-269">, Aby upewnić się, że komunikat jest wysyłany bez błędu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-269">To ensure the message is sent without error.</span></span> 
* <span data-ttu-id="61f0c-270">Aby włączyć przechwytywanie i obsłużyć błędy w bloku try-catch.</span><span class="sxs-lookup"><span data-stu-id="61f0c-270">To enable catching and handling errors in a try-catch block.</span></span>

<span data-ttu-id="61f0c-271">**Klient JavaScript korzystający z wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="61f0c-271">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="61f0c-272">Nie można uzyskać wartości zwracanej z metody klienta; Składnia, taka jak `int x = Clients.All.add(1,1)`, nie działa.</span><span class="sxs-lookup"><span data-stu-id="61f0c-272">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="61f0c-273">Można określić typy złożone i tablice dla parametrów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-273">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="61f0c-274">Poniższy przykład przekazuje typ złożony do klienta w parametrze metody.</span><span class="sxs-lookup"><span data-stu-id="61f0c-274">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="61f0c-275">**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**</span><span class="sxs-lookup"><span data-stu-id="61f0c-275">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="61f0c-276">**Kod serwera, który definiuje obiekt złożony**</span><span class="sxs-lookup"><span data-stu-id="61f0c-276">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="61f0c-277">**Klient JavaScript korzystający z wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="61f0c-277">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="61f0c-278">Wybieranie klientów, którzy będą otrzymywać wywołania RPC</span><span class="sxs-lookup"><span data-stu-id="61f0c-278">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="61f0c-279">Właściwość klienci zwraca obiekt [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) , który udostępnia kilka opcji określania klientów, którzy będą otrzymywać wywołania RPC:</span><span class="sxs-lookup"><span data-stu-id="61f0c-279">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="61f0c-280">Wszyscy połączeni klienci.</span><span class="sxs-lookup"><span data-stu-id="61f0c-280">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="61f0c-281">Tylko klient wywołujący.</span><span class="sxs-lookup"><span data-stu-id="61f0c-281">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="61f0c-282">Wszyscy klienci z wyjątkiem klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="61f0c-282">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="61f0c-283">Określony klient identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-283">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="61f0c-284">Ten przykład wywołuje `addContosoChatMessageToPage` na kliencie wywołującym i ma ten sam efekt, co użycie `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-284">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="61f0c-285">Wszyscy połączeni klienci z wyjątkiem określonych klientów identyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-285">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="61f0c-286">Wszyscy połączeni klienci w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-286">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="61f0c-287">Wszyscy połączeni klienci w określonej grupie z wyjątkiem określonych klientów identyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-287">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="61f0c-288">Wszyscy połączeni klienci w określonej grupie z wyjątkiem klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="61f0c-288">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="61f0c-289">Określony użytkownik identyfikowany według identyfikatora użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61f0c-289">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="61f0c-290">Domyślnie jest to `IPrincipal.Identity.Name`, ale można to zmienić, [rejestrując implementację IUserIdProvider z hostem globalnym](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="61f0c-290">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="61f0c-291">Wszyscy klienci i grupy na liście identyfikatorów połączeń.</span><span class="sxs-lookup"><span data-stu-id="61f0c-291">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="61f0c-292">Lista grup.</span><span class="sxs-lookup"><span data-stu-id="61f0c-292">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="61f0c-293">Użytkownik według nazwy.</span><span class="sxs-lookup"><span data-stu-id="61f0c-293">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="61f0c-294">Lista nazw użytkowników (wprowadzona w sygnalizacji 2,1).</span><span class="sxs-lookup"><span data-stu-id="61f0c-294">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="61f0c-295">Brak weryfikacji w czasie kompilacji dla nazw metod</span><span class="sxs-lookup"><span data-stu-id="61f0c-295">No compile-time validation for method names</span></span>

<span data-ttu-id="61f0c-296">Określona nazwa metody jest interpretowana jako obiekt dynamiczny, co oznacza, że nie istnieje funkcja IntelliSense ani weryfikacja w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="61f0c-296">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="61f0c-297">Wyrażenie jest oceniane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="61f0c-297">The expression is evaluated at run time.</span></span> <span data-ttu-id="61f0c-298">Gdy wywołanie metody jest wykonywane, sygnalizujący wysyła nazwę metody i wartości parametrów do klienta, a jeśli klient ma metodę zgodną z nazwą, ta metoda jest wywoływana i są do niej przenoszone wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-298">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="61f0c-299">Jeśli na kliencie nie zostanie znaleziona zgodna Metoda, żaden błąd nie zostanie zgłoszony.</span><span class="sxs-lookup"><span data-stu-id="61f0c-299">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="61f0c-300">Aby uzyskać informacje na temat formatu danych przesyłanych do klienta przez program sygnalizujący w tle podczas wywoływania metody klienta, zobacz [wprowadzenie do usługi sygnalizującej](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="61f0c-300">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="61f0c-301">Dopasowanie nazw metod bez uwzględniania wielkości liter</span><span class="sxs-lookup"><span data-stu-id="61f0c-301">Case-insensitive method name matching</span></span>

<span data-ttu-id="61f0c-302">W dopasowaniu nazw metod nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="61f0c-302">Method name matching is case-insensitive.</span></span> <span data-ttu-id="61f0c-303">Na przykład `Clients.All.addContosoChatMessageToPage` na serwerze wykona `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`lub `addContosoChatMessageToPage` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-303">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="61f0c-304">Wykonywanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="61f0c-304">Asynchronous execution</span></span>

<span data-ttu-id="61f0c-305">Wywoływana metoda jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-305">The method that you call executes asynchronously.</span></span> <span data-ttu-id="61f0c-306">Każdy kod, który jest wywoływany po wywołaniu metody do klienta, będzie wykonywany natychmiast bez oczekiwania na zakończenie przesyłania danych do klientów przez program sygnalizujący, chyba że określisz, że kolejne wiersze kodu powinny czekać na ukończenie metody.</span><span class="sxs-lookup"><span data-stu-id="61f0c-306">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="61f0c-307">Poniższy przykład kodu pokazuje, jak wykonać dwie metody klienta sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-307">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="61f0c-308">**Używanie oczekiwania (.NET 4,5)**</span><span class="sxs-lookup"><span data-stu-id="61f0c-308">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="61f0c-309">Jeśli używasz `await`, aby poczekać, aż Metoda klienta zakończy się przed wykonaniem kolejnego wiersza kodu, nie oznacza to, że klienci otrzymają komunikat przed wykonaniem kolejnego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-309">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="61f0c-310">"Uzupełnianie" wywołania metody klienta oznacza tylko, że sygnalizujący zrobił wszystko niezbędne do wysłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="61f0c-310">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="61f0c-311">Jeśli potrzebujesz weryfikacji, że klienci otrzymali komunikat, musisz samodzielnie programować ten mechanizm.</span><span class="sxs-lookup"><span data-stu-id="61f0c-311">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="61f0c-312">Można na przykład zakodować metodę `MessageReceived` w centrum, a w metodzie `addContosoChatMessageToPage` na kliencie można wywołać `MessageReceived` po wykonaniu jakiejkolwiek czynności, którą trzeba wykonać na kliencie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-312">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="61f0c-313">W `MessageReceived` w centrum można wykonać dowolną czynność, która zależy od rzeczywistego odbioru klienta i przetwarzania oryginalnego wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="61f0c-313">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="61f0c-314">Jak używać zmiennej ciągu jako nazwy metody</span><span class="sxs-lookup"><span data-stu-id="61f0c-314">How to use a string variable as the method name</span></span>

<span data-ttu-id="61f0c-315">Jeśli chcesz wywołać metodę klienta przy użyciu zmiennej ciągu jako nazwy metody, Cast `Clients.All` (lub `Clients.Others`, `Clients.Caller`itd.), aby `IClientProxy`, a następnie wywołać wywołania [(MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="61f0c-315">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="61f0c-316">Zarządzanie członkostwem w grupie z klasy centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-316">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="61f0c-317">Grupy w sygnalizacji zapewniają metodę rozgłaszania komunikatów do określonych podzestawów połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-317">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="61f0c-318">Grupa może mieć dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup.</span><span class="sxs-lookup"><span data-stu-id="61f0c-318">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="61f0c-319">Aby zarządzać członkostwem w grupie, użyj metod [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) i [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) dostarczonych przez właściwość `Groups` klasy Hub.</span><span class="sxs-lookup"><span data-stu-id="61f0c-319">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="61f0c-320">W poniższym przykładzie przedstawiono metody `Groups.Add` i `Groups.Remove` używane w metodach centralnych, które są wywoływane przez kod klienta, a następnie kod klienta JavaScript, który je wywołuje.</span><span class="sxs-lookup"><span data-stu-id="61f0c-320">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="61f0c-321">**Serwer**</span><span class="sxs-lookup"><span data-stu-id="61f0c-321">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="61f0c-322">**Klient JavaScript korzystający z wygenerowanego serwera proxy**</span><span class="sxs-lookup"><span data-stu-id="61f0c-322">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="61f0c-323">Nie musisz jawnie tworzyć grup.</span><span class="sxs-lookup"><span data-stu-id="61f0c-323">You don't have to explicitly create groups.</span></span> <span data-ttu-id="61f0c-324">W efekcie Grupa jest tworzona automatycznie przy pierwszym określeniu jej nazwy w wywołaniu `Groups.Add`i zostanie usunięta po usunięciu ostatniego połączenia z członkostwa w nim.</span><span class="sxs-lookup"><span data-stu-id="61f0c-324">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="61f0c-325">Brak interfejsu API do uzyskiwania listy członkostwa w grupie lub listy grup.</span><span class="sxs-lookup"><span data-stu-id="61f0c-325">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="61f0c-326">Program sygnalizujący wysyła komunikaty do klientów i grup na podstawie [modelu pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), a serwer nie zachowuje list grup ani członkostw w grupach.</span><span class="sxs-lookup"><span data-stu-id="61f0c-326">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="61f0c-327">Pozwala to zmaksymalizować skalowalność, ponieważ po dodaniu węzła do kolektywu serwerów sieci Web, każdy stan, który utrzymuje usługa sygnalizująca, musi być propagowany do nowego węzła.</span><span class="sxs-lookup"><span data-stu-id="61f0c-327">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="61f0c-328">Asynchroniczne wykonywanie metod Add i Remove</span><span class="sxs-lookup"><span data-stu-id="61f0c-328">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="61f0c-329">Metody `Groups.Add` i `Groups.Remove` wykonują asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-329">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="61f0c-330">Jeśli chcesz dodać klienta do grupy i natychmiast wysłać komunikat do klienta przy użyciu grupy, musisz upewnić się, że metoda `Groups.Add` została najpierw ukończona.</span><span class="sxs-lookup"><span data-stu-id="61f0c-330">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="61f0c-331">Poniższy przykład kodu pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="61f0c-331">The following code example shows how to do that.</span></span>

<span data-ttu-id="61f0c-332">**Dodawanie klienta do grupy, a następnie wysyłanie komunikatów do klienta**</span><span class="sxs-lookup"><span data-stu-id="61f0c-332">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="61f0c-333">Trwałość członkostwa w grupie</span><span class="sxs-lookup"><span data-stu-id="61f0c-333">Group membership persistence</span></span>

<span data-ttu-id="61f0c-334">Program sygnalizujący śledzi połączenia, a nie użytkowników, dlatego jeśli chcesz, aby użytkownik znajdował się w tej samej grupie za każdym razem, gdy użytkownik nawiąże połączenie, należy wywołać `Groups.Add` za każdym razem, gdy użytkownik ustanowi nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-334">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="61f0c-335">Po tymczasowej utracie łączności czasami program sygnalizujący może automatycznie przywrócić połączenie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-335">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="61f0c-336">W takim przypadku sygnalizujący przywraca to samo połączenie, nie ustanawia nowego połączenia, a więc członkostwo w grupie klienta zostanie automatycznie przywrócone.</span><span class="sxs-lookup"><span data-stu-id="61f0c-336">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="61f0c-337">Jest to możliwe nawet wtedy, gdy tymczasowy przerwanie jest wynikiem ponownego uruchomienia serwera lub błędu, ponieważ stan połączenia dla każdego klienta, w tym członkostwa w grupie, jest przełączany do klienta.</span><span class="sxs-lookup"><span data-stu-id="61f0c-337">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="61f0c-338">Jeśli serwer ulegnie awarii i zostanie zastąpiony przez nowy serwer przed przekroczeniem limitu czasu, klient będzie mógł automatycznie połączyć się z nowym serwerem i ponownie zarejestrować w grupach, które jest członkiem.</span><span class="sxs-lookup"><span data-stu-id="61f0c-338">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="61f0c-339">Gdy połączenie nie może zostać przywrócone automatycznie po utracie łączności lub gdy upłynął limit czasu połączenia lub gdy klient odłączy się (na przykład gdy przeglądarka nawiguje do nowej strony), członkostwo w grupach zostanie utracone.</span><span class="sxs-lookup"><span data-stu-id="61f0c-339">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="61f0c-340">Następnym razem, gdy użytkownik łączy się z nowym połączeniem.</span><span class="sxs-lookup"><span data-stu-id="61f0c-340">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="61f0c-341">Aby zachować członkostwo w grupach, gdy ten sam użytkownik ustanowi nowe połączenie, aplikacja musi śledzić skojarzenia między użytkownikami i grupami oraz przywracać członkostwa w grupach za każdym razem, gdy użytkownik ustanowi nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-341">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="61f0c-342">Więcej informacji o połączeniach i repołączeniach znajduje się [w temacie jak obsłużyć zdarzenia okresu istnienia połączenia w klasie centrów](#connectionlifetime) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-342">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="61f0c-343">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="61f0c-343">Single-user groups</span></span>

<span data-ttu-id="61f0c-344">Aplikacje korzystające z programu sygnalizującego zwykle muszą śledzić skojarzenia między użytkownikami i połączeniami, aby wiedzieć, który użytkownik wysłał komunikat i które użytkownicy powinni otrzymywać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="61f0c-344">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="61f0c-345">Grupy są używane w jednym z dwóch często używanych wzorców w celu wykonania tej operacji.</span><span class="sxs-lookup"><span data-stu-id="61f0c-345">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="61f0c-346">Grupy pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61f0c-346">Single-user groups.</span></span>

    <span data-ttu-id="61f0c-347">Możesz określić nazwę użytkownika jako nazwę grupy i dodać bieżący identyfikator połączenia do grupy za każdym razem, gdy użytkownik łączy się lub ponownie nawiązuje połączenie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-347">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="61f0c-348">Wysyłanie komunikatów do użytkownika, który jest wysyłany do grupy.</span><span class="sxs-lookup"><span data-stu-id="61f0c-348">To send messages to the user you send to the group.</span></span> <span data-ttu-id="61f0c-349">Wadą tej metody jest to, że grupa nie pozwala na ustalenie, czy użytkownik jest w trybie online, czy offline.</span><span class="sxs-lookup"><span data-stu-id="61f0c-349">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="61f0c-350">Śledź skojarzenia między nazwami użytkowników i identyfikatorami połączeń.</span><span class="sxs-lookup"><span data-stu-id="61f0c-350">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="61f0c-351">Można przechowywać skojarzenie między nazwami użytkowników i co najmniej jednym identyfikatorem połączenia w słowniku lub bazie danych i aktualizować przechowywane dane za każdym razem, gdy użytkownik nawiązuje połączenie lub rozłącza.</span><span class="sxs-lookup"><span data-stu-id="61f0c-351">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="61f0c-352">Aby wysyłać komunikaty do użytkownika, należy określić identyfikatory połączeń.</span><span class="sxs-lookup"><span data-stu-id="61f0c-352">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="61f0c-353">Wadą tej metody jest to, że zajmuje więcej pamięci.</span><span class="sxs-lookup"><span data-stu-id="61f0c-353">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="61f0c-354">Jak obsłużyć zdarzenia okresu istnienia połączenia w klasie centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-354">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="61f0c-355">Typowymi przyczynami obsługi zdarzeń okresu istnienia połączenia są śledzenie tego, czy użytkownik jest połączony, i śledzenie skojarzenia między nazwami użytkowników i identyfikatorami połączeń.</span><span class="sxs-lookup"><span data-stu-id="61f0c-355">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="61f0c-356">Aby uruchomić własny kod, gdy klienci nawiązują połączenie lub rozłączają, Zastąp metody wirtualne `OnConnected`, `OnDisconnected`i `OnReconnected` klasy Hub, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-356">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="61f0c-357">Gdy są wywoływane metody OnConnected, ondisconnected i OnReconnected</span><span class="sxs-lookup"><span data-stu-id="61f0c-357">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="61f0c-358">Za każdym razem, gdy przeglądarka nawiguje do nowej strony, należy nawiązać nowe połączenie, co oznacza, że sygnalizujący wykona metodę `OnDisconnected`, a następnie metodę `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-358">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="61f0c-359">Sygnalizujący zawsze tworzy nowy identyfikator połączenia po nawiązaniu nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-359">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="61f0c-360">Metoda `OnReconnected` jest wywoływana, gdy nastąpiło tymczasowe przerwanie łączności, z której sygnalizujący mogą automatycznie odzyskiwać dane, na przykład gdy kabel jest tymczasowo odłączony i ponownie połączony przed upływem limitu czasu połączenia. Metoda `OnDisconnected` jest wywoływana, gdy klient zostanie odłączony, a sygnalizujący nie może automatycznie ponownie nawiązać połączenia, na przykład gdy przeglądarka nawiguje do nowej strony.</span><span class="sxs-lookup"><span data-stu-id="61f0c-360">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="61f0c-361">W związku z tym możliwe jest `OnConnected`, `OnReconnected`, `OnDisconnected`; lub `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-361">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="61f0c-362">Nie zobaczysz sekwencji `OnConnected`, `OnDisconnected``OnReconnected` dla danego połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-362">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="61f0c-363">Metoda `OnDisconnected` nie jest wywoływana w niektórych scenariuszach, na przykład w przypadku awarii serwera lub odtworzenia domeny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61f0c-363">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="61f0c-364">Gdy inny serwer jest w trybie online lub domena aplikacji zakończy jego odtwarzanie, niektórzy klienci mogą być w stanie ponownie nawiązać połączenie i uruchomić zdarzenie `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-364">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="61f0c-365">Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia w programie sygnalizującym](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="61f0c-365">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="61f0c-366">Stan wywołującego nie został wypełniony</span><span class="sxs-lookup"><span data-stu-id="61f0c-366">Caller state not populated</span></span>

<span data-ttu-id="61f0c-367">Metody obsługi zdarzeń okresu istnienia połączenia są wywoływane z serwera, co oznacza, że każdy stan umieszczony w obiekcie `state` na kliencie nie zostanie wypełniony we właściwości `Caller` na serwerze.</span><span class="sxs-lookup"><span data-stu-id="61f0c-367">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="61f0c-368">Aby uzyskać informacje na temat obiektu `state` i właściwości `Caller`, zobacz [jak przekazać stan między klientami a klasą centrum](#passstate) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-368">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="61f0c-369">Jak uzyskać informacje o kliencie z właściwości kontekstu</span><span class="sxs-lookup"><span data-stu-id="61f0c-369">How to get information about the client from the Context property</span></span>

<span data-ttu-id="61f0c-370">Aby uzyskać informacje o kliencie, użyj właściwości `Context` klasy Hub.</span><span class="sxs-lookup"><span data-stu-id="61f0c-370">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="61f0c-371">Właściwość `Context` zwraca obiekt [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) , który zapewnia dostęp do następujących informacji:</span><span class="sxs-lookup"><span data-stu-id="61f0c-371">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="61f0c-372">Identyfikator połączenia klienta wywołującego.</span><span class="sxs-lookup"><span data-stu-id="61f0c-372">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="61f0c-373">Identyfikator połączenia jest identyfikatorem GUID, który jest przypisany przez sygnalizujący (nie można określić wartości w własnym kodzie).</span><span class="sxs-lookup"><span data-stu-id="61f0c-373">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="61f0c-374">Istnieje jeden identyfikator połączenia dla każdego połączenia, a ten sam identyfikator połączenia jest używany przez wszystkie centra, jeśli w aplikacji znajduje się wiele centrów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-374">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="61f0c-375">Dane nagłówka HTTP.</span><span class="sxs-lookup"><span data-stu-id="61f0c-375">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="61f0c-376">Można również uzyskać nagłówki HTTP z `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-376">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="61f0c-377">Przyczyną wielu odwołań do tego samego elementu jest to, że `Context.Headers` został utworzony jako pierwszy, właściwość `Context.Request` została dodana później i `Context.Headers` została zachowana w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="61f0c-377">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="61f0c-378">Dane ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="61f0c-378">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="61f0c-379">Możesz również uzyskać dane ciągu zapytania z `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-379">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="61f0c-380">Ciąg zapytania, który jest pobierany w tej właściwości, jest używany z żądaniem HTTP, które ustanowiło połączenie sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="61f0c-380">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="61f0c-381">Parametry ciągu zapytania można dodać do klienta przez skonfigurowanie połączenia, które jest wygodnym sposobem przekazywania danych dotyczących klienta z klienta do serwera programu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-381">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="61f0c-382">W poniższym przykładzie pokazano jeden ze sposobów dodawania ciągu zapytania do klienta JavaScript w przypadku korzystania z wygenerowanego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61f0c-382">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="61f0c-383">Aby uzyskać więcej informacji na temat ustawiania parametrów ciągu zapytania, zobacz Przewodniki interfejsu API dla klientów [JavaScript](hubs-api-guide-javascript-client.md) i [.NET](hubs-api-guide-net-client.md) .</span><span class="sxs-lookup"><span data-stu-id="61f0c-383">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="61f0c-384">Metodę transportu używaną w połączeniu można znaleźć w danych ciągu zapytania, wraz z innymi wartościami używanymi wewnętrznie przez program sygnalizujący:</span><span class="sxs-lookup"><span data-stu-id="61f0c-384">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="61f0c-385">Wartość `transportMethod` będzie równa "WebSockets", "serverSentEvents", "foreverFrame" lub "longPolling".</span><span class="sxs-lookup"><span data-stu-id="61f0c-385">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="61f0c-386">Należy pamiętać, że jeśli ta wartość zostanie sprawdzona w metodzie obsługi zdarzeń `OnConnected`, w niektórych scenariuszach można początkowo uzyskać wartość transportu, która nie jest ostateczną negocjowaną metodą transportu dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-386">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="61f0c-387">W takim przypadku Metoda zgłosi wyjątek i zostanie wywołana ponownie później podczas ustanowienia końcowej metody transportu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-387">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="61f0c-388">Cookie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-388">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="61f0c-389">Możesz również pobrać pliki cookie z `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-389">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="61f0c-390">Informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="61f0c-390">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="61f0c-391">Obiekt HttpContext dla żądania:</span><span class="sxs-lookup"><span data-stu-id="61f0c-391">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="61f0c-392">Użyj tej metody zamiast uzyskać `HttpContext.Current`, aby uzyskać obiekt `HttpContext` dla połączenia sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="61f0c-392">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="61f0c-393">Jak przekazać stan między klientami a klasą centrum</span><span class="sxs-lookup"><span data-stu-id="61f0c-393">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="61f0c-394">Serwer proxy klienta udostępnia obiekt `state`, w którym można przechowywać dane, które mają być przesyłane do serwera przy użyciu każdego wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="61f0c-394">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="61f0c-395">Na serwerze można uzyskać dostęp do tych danych we właściwości `Clients.Caller` w metodach centralnych wywoływanych przez klientów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-395">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="61f0c-396">Właściwość `Clients.Caller` nie jest wypełniona dla metod obsługi zdarzeń okresu istnienia połączenia `OnConnected`, `OnDisconnected`i `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-396">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="61f0c-397">Tworzenie lub aktualizowanie danych w obiekcie `state` i Właściwość `Clients.Caller` działa w obu kierunkach.</span><span class="sxs-lookup"><span data-stu-id="61f0c-397">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="61f0c-398">Można aktualizować wartości na serwerze i są one przesyłane z powrotem do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-398">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="61f0c-399">Poniższy przykład przedstawia kod klienta JavaScript, który przechowuje stan przesyłania do serwera przy każdym wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="61f0c-399">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="61f0c-400">Poniższy przykład pokazuje odpowiedni kod w kliencie .NET.</span><span class="sxs-lookup"><span data-stu-id="61f0c-400">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="61f0c-401">W klasie centrów możesz uzyskać dostęp do tych danych we właściwości `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-401">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="61f0c-402">Poniższy przykład pokazuje kod, który pobiera stan, do którego odwołuje się w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-402">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="61f0c-403">Ten mechanizm dla stanu utrwalania nie jest przeznaczony dla dużych ilości danych, ponieważ wszystkie elementy, które zostały umieszczone w `state` lub właściwości `Clients.Caller`, są wyłączane z każdym wywołaniem metody.</span><span class="sxs-lookup"><span data-stu-id="61f0c-403">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="61f0c-404">Jest to przydatne w przypadku mniejszych elementów, takich jak nazwy użytkowników lub liczniki.</span><span class="sxs-lookup"><span data-stu-id="61f0c-404">It's useful for smaller items such as user names or counters.</span></span>

<span data-ttu-id="61f0c-405">W VB.NET lub w koncentratorze z jednoznacznie określonym typem nie można uzyskać dostępu do obiektu stanu wywołującego za pomocą `Clients.Caller`; Zamiast tego należy użyć `Clients.CallerState` (wprowadzona w sygnalizacji 2,1):</span><span class="sxs-lookup"><span data-stu-id="61f0c-405">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="61f0c-406">**Używanie CallerState wC#**</span><span class="sxs-lookup"><span data-stu-id="61f0c-406">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="61f0c-407">**Używanie CallerState w Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="61f0c-407">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="61f0c-408">Jak obsłużyć błędy w klasie centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-408">How to handle errors in the Hub class</span></span>

<span data-ttu-id="61f0c-409">Aby obsłużyć błędy występujące w metodach klasy centrum, należy najpierw upewnić się, że wszystkie wyjątki od operacji asynchronicznych (takich jak wywoływanie metod klienta) za pomocą `await`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-409">To handle errors that occur in your Hub class methods, first ensure you "observe" any exceptions from async operations (such as invoking client methods) by using `await`.</span></span> <span data-ttu-id="61f0c-410">Następnie użyj co najmniej jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="61f0c-410">Then use one or more of the following methods:</span></span>

- <span data-ttu-id="61f0c-411">Zawiń kod metody w blokach try-catch i zarejestruj obiekt wyjątku.</span><span class="sxs-lookup"><span data-stu-id="61f0c-411">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="61f0c-412">Na potrzeby debugowania można wysłać wyjątek do klienta, ale ze względów bezpieczeństwa nie zaleca się wysyłania szczegółowych informacji do klientów w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="61f0c-412">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="61f0c-413">Utwórz moduł potoków centrów, który obsługuje metodę [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="61f0c-413">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="61f0c-414">W poniższym przykładzie przedstawiono moduł potoku, który rejestruje błędy, a następnie kod w Startup.cs, który wprowadza moduł do potoku centrów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-414">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="61f0c-415">Użyj klasy `HubException` (wprowadzona w sygnalizacji 2).</span><span class="sxs-lookup"><span data-stu-id="61f0c-415">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="61f0c-416">Ten błąd może zostać zgłoszony z dowolnego wywołania piasty.</span><span class="sxs-lookup"><span data-stu-id="61f0c-416">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="61f0c-417">Konstruktor `HubError` przyjmuje komunikat ciągu i obiekt do przechowywania dodatkowych danych o błędach.</span><span class="sxs-lookup"><span data-stu-id="61f0c-417">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="61f0c-418">Sygnalizujący będzie autoserializować wyjątek i wyśle go do klienta, gdzie zostanie użyty do odrzucenia lub niepowodzenia wywołania metody centrum.</span><span class="sxs-lookup"><span data-stu-id="61f0c-418">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="61f0c-419">Poniższy przykład kodu pokazuje, jak zgłosić `HubException` podczas wywołania centrum oraz jak obsłużyć wyjątek na klientach JavaScript i .NET.</span><span class="sxs-lookup"><span data-stu-id="61f0c-419">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="61f0c-420">**Kod serwera pokazujący klasę HubException**</span><span class="sxs-lookup"><span data-stu-id="61f0c-420">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="61f0c-421">**Kod klienta JavaScript pokazujący odpowiedź na wyrzucanie HubException w centrum**</span><span class="sxs-lookup"><span data-stu-id="61f0c-421">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="61f0c-422">**Kod klienta platformy .NET pokazujący odpowiedź na wyrzucanie HubException w centrum**</span><span class="sxs-lookup"><span data-stu-id="61f0c-422">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="61f0c-423">Więcej informacji o modułach potoku centrów znajduje się w sekcji [jak dostosować potok centrów](#hubpipeline) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="61f0c-423">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="61f0c-424">Jak włączyć śledzenie</span><span class="sxs-lookup"><span data-stu-id="61f0c-424">How to enable tracing</span></span>

<span data-ttu-id="61f0c-425">Aby włączyć śledzenie po stronie serwera, Dodaj element System. Diagnostics do pliku Web. config, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="61f0c-425">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="61f0c-426">Po uruchomieniu aplikacji w programie Visual Studio można wyświetlić dzienniki w oknie **danych wyjściowych** .</span><span class="sxs-lookup"><span data-stu-id="61f0c-426">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="61f0c-427">Jak wywoływać metody klienta i zarządzać grupami spoza klasy Hub</span><span class="sxs-lookup"><span data-stu-id="61f0c-427">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="61f0c-428">Aby wywołać metody klienta z innej klasy niż Klasa Hub, Pobierz odwołanie do obiektu kontekstu sygnalizującego dla centrum i użyj go do wywołania metod na kliencie lub zarządzania grupami.</span><span class="sxs-lookup"><span data-stu-id="61f0c-428">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="61f0c-429">Poniższa przykładowa Klasa `StockTicker` pobiera obiekt kontekstu, zapisuje go w wystąpieniu klasy, zapisuje wystąpienie klasy w statycznej właściwości i używa kontekstu z wystąpienia klasy pojedynczej w celu wywołania metody `updateStockPrice` na klientach podłączonych do centrum o nazwie `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-429">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="61f0c-430">Jeśli konieczne jest wielokrotne użycie kontekstu w obiekcie długim czasie, Pobierz odwołanie raz i Zapisz je, a nie ponownie za każdym razem.</span><span class="sxs-lookup"><span data-stu-id="61f0c-430">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="61f0c-431">Uzyskanie kontekstu polega na tym, że sygnalizujący wyśle komunikaty do klientów w tej samej kolejności, w której metody centrum umożliwiają wywoływanie metody klienta.</span><span class="sxs-lookup"><span data-stu-id="61f0c-431">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="61f0c-432">Aby zapoznać się z samouczkiem, który pokazuje, jak używać kontekstu sygnalizującego dla centrum, zobacz [serwer Broadcast with ASP.NET signaler](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="61f0c-432">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="61f0c-433">Wywoływanie metod klienta</span><span class="sxs-lookup"><span data-stu-id="61f0c-433">Calling client methods</span></span>

<span data-ttu-id="61f0c-434">Można określić, którzy klienci będą otrzymywać wywołania RPC, ale jest mniej opcji niż w przypadku wywołania z klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="61f0c-434">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="61f0c-435">Przyczyną tego jest to, że kontekst nie jest skojarzony z konkretnym wywołaniem z klienta, dlatego wszelkie metody, które wymagają znajomości bieżącego identyfikatora połączenia, takiego jak `Clients.Others`, `Clients.Caller`lub `Clients.OthersInGroup`, nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="61f0c-435">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="61f0c-436">Dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="61f0c-436">The following options are available:</span></span>

- <span data-ttu-id="61f0c-437">Wszyscy połączeni klienci.</span><span class="sxs-lookup"><span data-stu-id="61f0c-437">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="61f0c-438">Określony klient identyfikowany przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-438">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="61f0c-439">Wszyscy połączeni klienci z wyjątkiem określonych klientów identyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-439">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="61f0c-440">Wszyscy połączeni klienci w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="61f0c-440">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="61f0c-441">Wszyscy połączeni klienci w określonej grupie z wyjątkiem określonych klientów identyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="61f0c-441">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="61f0c-442">Jeśli wywołujesz klasę niebędącą centrami z metod w klasie centrów, możesz przekazać bieżący identyfikator połączenia i użyć go z `Clients.Client`, `Clients.AllExcept`lub `Clients.Group`, aby symulować `Clients.Caller`, `Clients.Others`lub `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-442">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="61f0c-443">W poniższym przykładzie Klasa `MoveShapeHub` przekazuje identyfikator połączenia do klasy `Broadcaster`, dzięki czemu Klasa `Broadcaster` może symulować `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="61f0c-443">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="61f0c-444">Zarządzanie członkostwem w grupie</span><span class="sxs-lookup"><span data-stu-id="61f0c-444">Managing group membership</span></span>

<span data-ttu-id="61f0c-445">W przypadku zarządzania grupami te same opcje są takie same jak w klasie centrów.</span><span class="sxs-lookup"><span data-stu-id="61f0c-445">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="61f0c-446">Dodawanie klienta do grupy</span><span class="sxs-lookup"><span data-stu-id="61f0c-446">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="61f0c-447">Usuwanie klienta z grupy</span><span class="sxs-lookup"><span data-stu-id="61f0c-447">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="61f0c-448">Jak dostosować potok centrów</span><span class="sxs-lookup"><span data-stu-id="61f0c-448">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="61f0c-449">Sygnalizujący umożliwia wprowadzanie własnego kodu do potoku centrum.</span><span class="sxs-lookup"><span data-stu-id="61f0c-449">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="61f0c-450">W poniższym przykładzie przedstawiono niestandardowy moduł potoku, który rejestruje każde wywołanie metody przychodzącej odebrane od klienta i wywołania metody wychodzącej wywołanego na kliencie:</span><span class="sxs-lookup"><span data-stu-id="61f0c-450">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="61f0c-451">Następujący kod w pliku *Startup.cs* rejestruje moduł do uruchomienia w potoku centrum:</span><span class="sxs-lookup"><span data-stu-id="61f0c-451">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="61f0c-452">Istnieje wiele różnych metod, które można przesłonić.</span><span class="sxs-lookup"><span data-stu-id="61f0c-452">There are many different methods that you can override.</span></span> <span data-ttu-id="61f0c-453">Aby uzyskać pełną listę, zobacz [metody HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="61f0c-453">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
