---
uid: webhooks/receiving/handlers
title: Programy obsługi elementów webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Jak obsługiwać żądania w elementach webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637873"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="3fbc9-103">Procedury obsługi elementów webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3fbc9-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="3fbc9-104">Po zweryfikowaniu żądań elementów webhook przez odbiornik elementu webhook jest on gotowy do przetworzenia przez kod użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="3fbc9-105">Jest to miejsce, w którym są one *obsługiwane* .</span><span class="sxs-lookup"><span data-stu-id="3fbc9-105">This is where *handlers* come in.</span></span> <span data-ttu-id="3fbc9-106">Procedury obsługi pochodzą z interfejsu [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) , ale zazwyczaj używają klasy [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) zamiast wyprowadzania bezpośrednio z interfejsu.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="3fbc9-107">Żądanie elementu webhook może być przetwarzane przez jeden lub więcej programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="3fbc9-108">Procedury obsługi są wywoływane w kolejności na podstawie ich właściwości *Order* , która przechodzą z najmniejszego do najwyższego, gdzie Order jest prostą liczbą całkowitą (sugerowaną dla zakresu od 1 do 100):</span><span class="sxs-lookup"><span data-stu-id="3fbc9-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagram właściwości kolejności obsługi elementu webhook](_static/Handlers.png)

<span data-ttu-id="3fbc9-110">Program obsługi może opcjonalnie ustawić właściwość *Response* w WebHookHandlerContext, co spowoduje zatrzymanie przetwarzania i wysłanie odpowiedzi z powrotem jako odpowiedzi HTTP do elementu webhook.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="3fbc9-111">W powyższym przypadku procedura obsługi C nie zostanie wywołana, ponieważ ma wyższą kolejność niż B i B ustawia odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="3fbc9-112">Ustawienie odpowiedzi jest zazwyczaj odpowiednie dla elementów webhook, w których odpowiedź może przenosić informacje z powrotem do źródłowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="3fbc9-113">Dotyczy to na przykład przypadku elementu webhook zapasowych, gdzie odpowiedź jest księgowana z powrotem do kanału, z którego pochodzi element webhook.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="3fbc9-114">Programy obsługi mogą ustawiać właściwość Receiver, jeśli chcą otrzymywać tylko elementy webhook z tego konkretnego odbiorcy.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="3fbc9-115">Jeśli nie ustawili odbiorcy, są one wywoływane dla wszystkich z nich.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="3fbc9-116">Innym typowym wykorzystaniem odpowiedzi jest użycie *410* nieistniejącej odpowiedzi, aby wskazać, że element webhook nie jest już aktywny i nie powinny być przesyłane żadne dalsze żądania.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="3fbc9-117">Domyślnie program obsługi będzie wywoływany przez wszystkie odbiorniki elementu webhook.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="3fbc9-118">Jeśli jednak właściwość *Receiver* jest ustawiona na nazwę programu obsługi, to program obsługi będzie odbierać tylko żądania elementu webhook z tego odbiorcy.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="3fbc9-119">Przetwarzanie elementu webhook</span><span class="sxs-lookup"><span data-stu-id="3fbc9-119">Processing a WebHook</span></span>

<span data-ttu-id="3fbc9-120">Gdy wywoływana jest procedura obsługi, pobiera [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) zawierający informacje o żądaniu elementu webhook.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="3fbc9-121">Dane, zazwyczaj treść żądania HTTP, są dostępne na podstawie właściwości *Data* .</span><span class="sxs-lookup"><span data-stu-id="3fbc9-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="3fbc9-122">Typ danych to zwykle dane JSON lub HTML formularza, ale w razie potrzeby można rzutować na bardziej konkretny typ.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="3fbc9-123">Na przykład niestandardowe elementy webhook wygenerowane przez elementy webhook ASP.NET mogą być rzutowane na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3fbc9-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="3fbc9-124">Przetwarzanie w kolejce</span><span class="sxs-lookup"><span data-stu-id="3fbc9-124">Queued Processing</span></span>

<span data-ttu-id="3fbc9-125">Większość nadawców elementu webhook wyśle ponownie element webhook, jeśli odpowiedź nie zostanie wygenerowana w ciągu kilku sekund kilku.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="3fbc9-126">Oznacza to, że program obsługi musi zakończyć przetwarzanie w tym przedziale czasowym, aby nie był on ponownie wywoływany.</span><span class="sxs-lookup"><span data-stu-id="3fbc9-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="3fbc9-127">Jeśli przetwarzanie trwa dłużej lub jest lepiej obsługiwane oddzielnie, [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) można użyć do przesłania żądania elementu webhook do kolejki, na przykład z [kolejki usługi Azure Storage](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="3fbc9-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="3fbc9-128">Konspekt implementacji [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) jest dostępny tutaj:</span><span class="sxs-lookup"><span data-stu-id="3fbc9-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
