---
uid: webhooks/receiving/handlers
title: Programy obsługi elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Sposób obsługi żądań w elementów Webhook programu ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067643"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="00442-103">Programy obsługi elementów Webhook programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="00442-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="00442-104">Gdy elementy Webhook żądania została zweryfikowana przez odbiorcę elementu WebHook, jest gotowe do przetworzenia przez kod użytkownika.</span><span class="sxs-lookup"><span data-stu-id="00442-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="00442-105">Jest to miejsce *obsługi* pochodzą.</span><span class="sxs-lookup"><span data-stu-id="00442-105">This is where *handlers* come in.</span></span> <span data-ttu-id="00442-106">Programy obsługi pochodzić od [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfejs, ale zazwyczaj używa [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) klasy zamiast wywodzić się bezpośrednio z interfejsu.</span><span class="sxs-lookup"><span data-stu-id="00442-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="00442-107">Żądanie elementu WebHook, mogą być przetwarzane przez jeden lub więcej programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="00442-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="00442-108">Programy obsługi są wywoływane w kolejności, w oparciu o ich odpowiednich *kolejności* właściwość przesyłane z najniższej do najwyższej gdzie kolejność jest liczbą całkowitą proste (sugerowany należeć do zakresu od 1 do 100):</span><span class="sxs-lookup"><span data-stu-id="00442-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Procedura obsługi elementu WebHook kolejność właściwości diagramu](_static/Handlers.png)

<span data-ttu-id="00442-110">Opcjonalnie możesz ustawić programu obsługi *odpowiedzi* właściwość WebHookHandlerContext, co będzie prowadzić przetwarzania do zatrzymania i odpowiedź do wysłania jako odpowiedzi HTTP do elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="00442-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="00442-111">W przypadku powyższych obsługi języka C nie będzie wywoływana, ponieważ ma ona wyższego rzędu niż B i B ustawia odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="00442-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="00442-112">Ustawienie odpowiedzi jest zwykle tylko istotne dla elementów Webhook, gdzie odpowiedzi mogą przenosić informacji do źródłowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="00442-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="00442-113">Jest to na przykład w przypadku elementów Webhook Slack, gdy odpowiedź opublikowaniu powrót do kanału, skąd pochodzą elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="00442-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="00442-114">Programy obsługi można ustawić właściwości odbiorcy, jeśli chcą odbierać elementy Webhook z tego określonego odbiornika.</span><span class="sxs-lookup"><span data-stu-id="00442-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="00442-115">Jeśli nie ustawiają odbiorcy, gdy są wywoływane dla wszystkich z nich.</span><span class="sxs-lookup"><span data-stu-id="00442-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="00442-116">Jeden innym typowym zastosowaniem odpowiedź jest użycie *410 Gone* odpowiedzi, aby wskazać, czy element WebHook nie jest już aktywne i powinny być przesyłane więcej żądań.</span><span class="sxs-lookup"><span data-stu-id="00442-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="00442-117">Domyślnie program obsługi zostanie wywołana przez wszystkich odbiorców elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="00442-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="00442-118">Jednak jeśli *odbiorcy* właściwość jest ustawiona na nazwę programu obsługi, a następnie ten program obsługi subskrybent otrzyma tylko żądania elementu WebHook z tego odbiornika.</span><span class="sxs-lookup"><span data-stu-id="00442-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="00442-119">Przetwarzanie elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="00442-119">Processing a WebHook</span></span>

<span data-ttu-id="00442-120">Gdy program obsługi jest wywoływana, pobiera [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) zawierający informacje o żądanie elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="00442-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="00442-121">Dane, zwykle treści żądania HTTP, są dostępne z *danych* właściwości.</span><span class="sxs-lookup"><span data-stu-id="00442-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="00442-122">Typ danych jest zazwyczaj dane formularza JSON lub HTML, ale można rzutować do bardziej określonego typu, w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="00442-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="00442-123">Na przykład niestandardowych elementów Webhook, generowane przez elementy Webhook ASP.NET mogą być rzutowane na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="00442-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="00442-124">Przetwarzanie umieszczonych w kolejce</span><span class="sxs-lookup"><span data-stu-id="00442-124">Queued Processing</span></span>

<span data-ttu-id="00442-125">Większość nadawców elementu WebHook wyśle element WebHook, jeśli odpowiedź nie są generowane w ciągu kilku sekund.</span><span class="sxs-lookup"><span data-stu-id="00442-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="00442-126">Oznacza to, że programu obsługi, należy wykonać przetwarzanie w tym przedziale czasu, w kolejności, nie na jego ponownie wywołana.</span><span class="sxs-lookup"><span data-stu-id="00442-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="00442-127">Jeśli przetwarzanie zajmuje więcej czasu lub lepszego obsługiwane osobno, a następnie [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) może służyć do przesyłania żądania elementu WebHook do kolejki, na przykład [usługi Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="00442-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="00442-128">Zarys [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) wdrożenia można znaleźć tu:</span><span class="sxs-lookup"><span data-stu-id="00442-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
