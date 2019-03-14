---
title: Wprowadzenie do SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się, jak biblioteki biblioteki SignalR platformy ASP.NET Core ułatwia dodawanie funkcji w czasie rzeczywistym do aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 673efafce60dfa46cb99f9537fda2bca42bf9822
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077564"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="0b5ae-103">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b5ae-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="0b5ae-104">Co to jest SignalR?</span><span class="sxs-lookup"><span data-stu-id="0b5ae-104">What is SignalR?</span></span>

<span data-ttu-id="0b5ae-105">SignalR platformy ASP.NET Core to biblioteka typu open source, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="0b5ae-106">Funkcje sieci web w czasie rzeczywistym umożliwia kodu po stronie serwera do wypychania zawartości do klientów natychmiast.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="0b5ae-107">Dobrymi kandydatami do SignalR:</span><span class="sxs-lookup"><span data-stu-id="0b5ae-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="0b5ae-108">Aplikacje, które wymagają wysokiej częstotliwości aktualizacji z serwera.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="0b5ae-109">Przykładami są gier, sieci społecznościowych, głosowanie, aukcji, map i aplikacji GPS.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="0b5ae-110">Pulpity nawigacyjne i monitorowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="0b5ae-111">Przykłady obejmują firmowe pulpity nawigacyjne natychmiastowej aktualizacji sprzedaży, lub przesyłane alerty.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="0b5ae-112">Aplikacje współpracy.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-112">Collaborative apps.</span></span> <span data-ttu-id="0b5ae-113">Aplikacje tablicy i zespołu spotkania oprogramowania to przykłady aplikacji współpracy.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="0b5ae-114">Aplikacje, które wymagają powiadomienia.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-114">Apps that require notifications.</span></span> <span data-ttu-id="0b5ae-115">Użycie powiadomień, sieci społecznościowych, poczty e-mail, rozmowy, gry, podróży alertów i wielu innych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="0b5ae-116">Biblioteka SignalR udostępnia interfejs API do tworzenia klienta serwera [zdalnych wywołań procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="0b5ae-116">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="0b5ae-117">Zdalnych wywołań procedury wywołują funkcje JavaScript na komputerach klienckich z kodu platformy .NET Core po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="0b5ae-118">Poniżej przedstawiono niektóre funkcje SignalR dla platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0b5ae-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="0b5ae-119">Automatycznie obsługuje zarządzanie połączeniami.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="0b5ae-120">Wysyła komunikaty do wszystkich połączonych klientów jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="0b5ae-121">Na przykład pokoju rozmów.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-121">For example, a chat room.</span></span>
* <span data-ttu-id="0b5ae-122">Wysyła komunikaty do określonych klientów lub grup klientów.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="0b5ae-123">Możliwość skalowania do obsługi ruchu rosnąca.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="0b5ae-124">Źródło znajduje się w [SignalR repozytorium w serwisie GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="0b5ae-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="0b5ae-125">Transporty</span><span class="sxs-lookup"><span data-stu-id="0b5ae-125">Transports</span></span>

<span data-ttu-id="0b5ae-126">SignalR obsługuje kilka technik do obsługi komunikacji w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="0b5ae-126">SignalR supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="0b5ae-127">Obiekty WebSocket</span><span class="sxs-lookup"><span data-stu-id="0b5ae-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="0b5ae-128">Zdarzenia wysłanego przez serwer</span><span class="sxs-lookup"><span data-stu-id="0b5ae-128">Server-Sent Events</span></span>
* <span data-ttu-id="0b5ae-129">Długiego sondowania</span><span class="sxs-lookup"><span data-stu-id="0b5ae-129">Long Polling</span></span>

<span data-ttu-id="0b5ae-130">SignalR automatycznie wybiera najlepszą metodę transportu, który znajduje się w funkcji serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-130">SignalR automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="0b5ae-131">Centra</span><span class="sxs-lookup"><span data-stu-id="0b5ae-131">Hubs</span></span>

<span data-ttu-id="0b5ae-132">Korzysta z biblioteki SignalR *koncentratory* do komunikacji między klientami a serwerami.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-132">SignalR uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="0b5ae-133">Centrum to wysokiego poziomu potok, który umożliwia klientowi i serwerowi wywoływać metody dla siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> <span data-ttu-id="0b5ae-134">SignalR obsługuje wysyłania w granicach maszyny automatycznie, umożliwiając klientom wywoływać metody na serwerze i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="0b5ae-135">Silnie typizowane parametry można przekazać do metody, które umożliwia wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="0b5ae-136">Biblioteka SignalR udostępnia dwa protokoły wbudowanych Centrum: protokół tekstowy dostępny na podstawie JSON i binarny protokołu, na podstawie [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="0b5ae-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="0b5ae-137">MessagePack zazwyczaj tworzy mniejsze wiadomości w porównaniu do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="0b5ae-138">Starsze przeglądarki musi obsługiwać [XHR poziom 2](https://caniuse.com/#feat=xhr2) do obsługi protokołu MessagePack.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="0b5ae-139">Koncentratory wywołać kod po stronie klienta, wysyłając komunikaty, które zawierają nazwę i parametry metody po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="0b5ae-140">Wysyłane jako parametry metody obiekty są deserializacji za pomocą skonfigurowanego protokołu.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="0b5ae-141">Klient próbuje dopasować nazwę metody w kodzie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="0b5ae-142">Jeśli klient znajdzie dopasowanie, wywołuje metodę i przekazuje do niego dane parametru po deserializacji.</span><span class="sxs-lookup"><span data-stu-id="0b5ae-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b5ae-143">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0b5ae-143">Additional resources</span></span>

* [<span data-ttu-id="0b5ae-144">Wprowadzenie do SignalR dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b5ae-144">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="0b5ae-145">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="0b5ae-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="0b5ae-146">Centra</span><span class="sxs-lookup"><span data-stu-id="0b5ae-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0b5ae-147">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="0b5ae-147">JavaScript client</span></span>](xref:signalr/javascript-client)
