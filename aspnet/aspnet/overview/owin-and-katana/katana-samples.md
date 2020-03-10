---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Przykłady Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584561"
---
# <a name="katana-samples"></a><span data-ttu-id="2ddfa-102">Przykłady projektu Katana</span><span class="sxs-lookup"><span data-stu-id="2ddfa-102">Katana Samples</span></span>

<span data-ttu-id="2ddfa-103">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2ddfa-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="2ddfa-104">Przykłady projektu Katana</span><span class="sxs-lookup"><span data-stu-id="2ddfa-104">Katana Samples</span></span>

<span data-ttu-id="2ddfa-105">**Przykładowe trasy ASP.NET** | [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="2ddfa-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="2ddfa-106">W niektórych aplikacjach chcesz podłączyć składniki OWIN w tabeli tras Asp.Net obok siebie ze składnikami nieOWIN.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="2ddfa-107">Ten przykład pokazuje, jak używać metod rozszerzenia RouteCollection MapOwinPath i MapOwinRoute dostarczonych przez firmę Microsoft. Owin. host. SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="2ddfa-108">**Przykład rozgałęziania potoków** | [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="2ddfa-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="2ddfa-109">Potoki przetwarzania żądań OWIN nie muszą być liniowe, mogą być rozgałęzienia na żądania procesów na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="2ddfa-110">Ten przykład pokazuje, jak utworzyć potok rozgałęzienia na podstawie ścieżek żądań lub innych danych żądania, takich jak nagłówki.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="2ddfa-111">Te składniki są dostępne w pakiecie NuGet Microsoft. Owin. Mapping.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="2ddfa-112">**Niestandardowy przykładowy serwer** | [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="2ddfa-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="2ddfa-113">Pokazuje, jak używać niestandardowego serwera OWIN podczas samodzielnego hostowania OWIN.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="2ddfa-114">**Osadzony przykładowy** | [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span><span class="sxs-lookup"><span data-stu-id="2ddfa-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="2ddfa-115">Niektóre serwery OWIN można uruchamiać w ramach własnego procesu (&quot;samodzielnie hostowane&quot;).</span><span class="sxs-lookup"><span data-stu-id="2ddfa-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="2ddfa-116">Ten przykład pokazuje, jak uruchomić aplikację OWIN przy użyciu narzędzi dostarczonych przez pakiet NuGet Microsoft. Owin. host.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="2ddfa-117">**Przykładowy** [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) | HelloWorld</span><span class="sxs-lookup"><span data-stu-id="2ddfa-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="2ddfa-118">OWIN to Abstrakcja interfejsu API serwera HTTP, która umożliwia przenoszenie aplikacji na różnych serwerach.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="2ddfa-119">W tym przykładzie pokazano, jak napisać aplikację Hello world przy użyciu kilku **prostych otok** wokół Owin abstrakcji RAW i uruchomić ją na serwerze sieci Web, takim jak ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="2ddfa-120">**Hello World Raw | przykład** [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) Owin</span><span class="sxs-lookup"><span data-stu-id="2ddfa-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="2ddfa-121">W tym przykładzie pokazano, jak napisać aplikację Hello world przy użyciu Owin abstrakcji **RAW** i uruchomić ją na serwerze sieci Web, takim jak ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="2ddfa-122">Przykładowy [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) **próbnika** | </span><span class="sxs-lookup"><span data-stu-id="2ddfa-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="2ddfa-123">Pokazuje, w jaki sposób do samoobsługowego sygnalizowania przy użyciu OWIN/Katana.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="2ddfa-124">Aby uzyskać więcej informacji na temat sygnalizacji samoobsługowej, zobacz [Samouczek: program do](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)samoobsługowego hostowania.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="2ddfa-125">**Pliki statyczne przykładowe** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="2ddfa-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="2ddfa-126">Pokazuje, jak obsługiwać żądania HTTP dla plików statycznych przy użyciu OWIN/Katana.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="2ddfa-127">[Kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) | internetowego **interfejsu API** </span><span class="sxs-lookup"><span data-stu-id="2ddfa-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="2ddfa-128">Ten przykład pokazuje, jak hostować OWIN w usługach IIS i dodawać interfejs API sieci Web do potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="2ddfa-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="2ddfa-129">Przykładowy [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) | **gniazda internetowego** </span><span class="sxs-lookup"><span data-stu-id="2ddfa-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="2ddfa-130">Pokazuje, jak obsługiwać gniazda sieci Web w programie OWIN przy użyciu klasy [System .NET. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="2ddfa-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
