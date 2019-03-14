---
title: SignalR HubContext
author: bradygaster
description: Dowiedz się, jak używać usługi ASP.NET Core SignalR HubContext wysyłania powiadomień do klientów z poza koncentratora.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2018
uid: signalr/hubcontext
ms.openlocfilehash: 73cf2c9d30ed5e409a75827fdab1f22b20427884
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57066158"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="93f23-103">Wysyłanie komunikatów z poza Centrum</span><span class="sxs-lookup"><span data-stu-id="93f23-103">Send messages from outside a hub</span></span>

<span data-ttu-id="93f23-104">Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="93f23-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="93f23-105">Centrum SignalR to Abstrakcja core do wysyłania wiadomości do klientów dołączonych do serwera biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="93f23-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="93f23-106">Istnieje również możliwość wysyłać komunikaty z innych miejsc w aplikacji przy użyciu `IHubContext` usługi.</span><span class="sxs-lookup"><span data-stu-id="93f23-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="93f23-107">W tym artykule wyjaśniono, jak uzyskać dostęp biblioteki SignalR `IHubContext` do wysyłania powiadomień do klientów z poza koncentratora.</span><span class="sxs-lookup"><span data-stu-id="93f23-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="93f23-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="93f23-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="93f23-109">Pobierz wystąpienia IHubContext</span><span class="sxs-lookup"><span data-stu-id="93f23-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="93f23-110">W biblioteki SignalR platformy ASP.NET Core, uzyskujesz dostęp do wystąpienia `IHubContext` za pomocą iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="93f23-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="93f23-111">Może wprowadzać wystąpienie `IHubContext` do kontrolera, oprogramowanie pośredniczące lub inna usługa DI.</span><span class="sxs-lookup"><span data-stu-id="93f23-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="93f23-112">Aby wysyłać komunikaty do klientów, należy użyć wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="93f23-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="93f23-113">To różni się od ASP.NET 4.x SignalR, w której używane GlobalHost w celu zapewnienia dostępu do `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="93f23-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="93f23-114">Platforma ASP.NET Core ma strukturę iniekcji zależności, która eliminuje potrzebę tym globalnego pojedynczym wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="93f23-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="93f23-115">Wstrzykiwanie wystąpienia IHubContext w kontrolerze</span><span class="sxs-lookup"><span data-stu-id="93f23-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="93f23-116">Może wprowadzać wystąpienie `IHubContext` do kontrolera, dodając go do konstruktora:</span><span class="sxs-lookup"><span data-stu-id="93f23-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="93f23-117">Teraz dzięki dostępowi do wystąpienia `IHubContext`, można wywoływać metod koncentratora, tak, jakby były w Centrum sam.</span><span class="sxs-lookup"><span data-stu-id="93f23-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="93f23-118">Pobierz wystąpienia IHubContext w oprogramowaniu pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="93f23-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="93f23-119">Dostęp do `IHubContext` w ramach potoku oprogramowania pośredniczącego w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="93f23-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(async (context, next) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="93f23-120">Kiedy metodach koncentratora są wywoływane z poza `Hub` klasy, nie ma żadnych wywołujący skojarzone z wywołania.</span><span class="sxs-lookup"><span data-stu-id="93f23-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="93f23-121">Dlatego nie ma dostępu do `ConnectionId`, `Caller`, i `Others` właściwości.</span><span class="sxs-lookup"><span data-stu-id="93f23-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

### <a name="inject-a-strongly-typed-hubcontext"></a><span data-ttu-id="93f23-122">Wstrzykiwanie HubContext silnie typizowane</span><span class="sxs-lookup"><span data-stu-id="93f23-122">Inject a strongly-typed HubContext</span></span>

<span data-ttu-id="93f23-123">Można wstrzyknąć HubContext silnie typizowane, upewnij się, dziedziczy Centrum `Hub<T>`.</span><span class="sxs-lookup"><span data-stu-id="93f23-123">To inject a strongly-typed HubContext, ensure your Hub inherits from `Hub<T>`.</span></span> <span data-ttu-id="93f23-124">Wstrzyknięcia go przy użyciu `IHubContext<THub, T>` interfejsu zamiast `IHubContext<THub>`.</span><span class="sxs-lookup"><span data-stu-id="93f23-124">Inject it using the `IHubContext<THub, T>` interface rather than `IHubContext<THub>`.</span></span>

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public ChatController(IHubContext<ChatHub, IChatClient> chatHubContext)
    {
        _strongChatHubContext = chatHubContext;
    }

    public async Task SendMessage(string message)
    {
        await _strongChatHubContext.Clients.All.ReceiveMessage(message);
    }
}
```

## <a name="related-resources"></a><span data-ttu-id="93f23-125">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="93f23-125">Related resources</span></span>

* [<span data-ttu-id="93f23-126">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="93f23-126">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="93f23-127">Centra</span><span class="sxs-lookup"><span data-stu-id="93f23-127">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="93f23-128">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="93f23-128">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
