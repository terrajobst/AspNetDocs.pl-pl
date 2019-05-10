---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Programy obsługi komunikatów HttpClient we wzorcu ASP.NET Web API — ASP.NET 4.x
author: MikeWasson
description: Tworzenie programów do obsługi komunikatów niestandardowych dla interfejsu API sieci Web platformy ASP.NET na platformie ASP.NET 4.x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115440"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="54a46-103">Programy obsługi komunikatów HttpClient w interfejsie Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="54a46-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="54a46-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="54a46-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="54a46-105">A *obsługi wiadomości* to klasa, która odbierze żądanie HTTP, a następnie zwraca odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="54a46-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="54a46-106">Zazwyczaj szereg programy obsługi komunikatów są połączone.</span><span class="sxs-lookup"><span data-stu-id="54a46-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="54a46-107">Pierwszego programu obsługi odebrania żądania HTTP, przetwarza je i zapewnia żądania do następnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="54a46-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="54a46-108">W pewnym momencie odpowiedzi jest tworzony i powraca łańcuch.</span><span class="sxs-lookup"><span data-stu-id="54a46-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="54a46-109">Ten wzorzec jest nazywany *delegowanie* programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="54a46-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="54a46-110">Po stronie klienta **HttpClient** klasa używa obsługi wiadomości do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="54a46-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="54a46-111">Domyślny program obsługi jest **HttpClientHandler**, który wysyła żądanie za pośrednictwem sieci i pobiera odpowiedź z serwera.</span><span class="sxs-lookup"><span data-stu-id="54a46-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="54a46-112">Programy obsługi komunikatów niestandardowych można wstawić do potoku klienta:</span><span class="sxs-lookup"><span data-stu-id="54a46-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="54a46-113">Web API platformy ASP.NET używa także procedury obsługi komunikatów po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="54a46-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="54a46-114">Aby uzyskać więcej informacji, zobacz [programów obsługi komunikatów HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="54a46-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="54a46-115">Programy obsługi komunikatów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="54a46-115">Custom Message Handlers</span></span>

<span data-ttu-id="54a46-116">Aby napisać program obsługi komunikatów niestandardowych, pochodzi od **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="54a46-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="54a46-117">W tym miejscu znajduje się sygnatura metody:</span><span class="sxs-lookup"><span data-stu-id="54a46-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="54a46-118">Ta metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="54a46-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="54a46-119">Typowa implementacja wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="54a46-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="54a46-120">Przetwarzanie komunikatu żądania.</span><span class="sxs-lookup"><span data-stu-id="54a46-120">Process the request message.</span></span>
2. <span data-ttu-id="54a46-121">Wywołaj `base.SendAsync` wysyłać żądania do wewnętrznego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="54a46-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="54a46-122">Wewnętrzny program obsługi zwraca komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="54a46-122">The inner handler returns a response message.</span></span> <span data-ttu-id="54a46-123">(Ten krok jest asynchroniczne).</span><span class="sxs-lookup"><span data-stu-id="54a46-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="54a46-124">Przetworzenie odpowiedzi i zwraca go do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="54a46-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="54a46-125">Program obsługi komunikatów, który dodaje niestandardowego nagłówka do żądania wychodzącego można znaleźć w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="54a46-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="54a46-126">Wywołanie `base.SendAsync` jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="54a46-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="54a46-127">Jeśli program obsługi ma wszelkie prace po tym wywołaniu, użyj **await** słowo kluczowe, aby wznowić wykonywanie po zakończeniu działania metody.</span><span class="sxs-lookup"><span data-stu-id="54a46-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="54a46-128">Program obsługi, który rejestruje kody błędów można znaleźć w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="54a46-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="54a46-129">Rejestrowanie, sama nie jest bardzo interesujące, ale w przykładzie pokazano, jak uzyskać w odpowiedzi wewnątrz programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="54a46-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="54a46-130">Dodawanie programów obsługi wiadomości do potoku klienta</span><span class="sxs-lookup"><span data-stu-id="54a46-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="54a46-131">Aby dodać niestandardowe programy obsługi do **HttpClient**, użyj **HttpClientFactory.Create** metody:</span><span class="sxs-lookup"><span data-stu-id="54a46-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="54a46-132">Programy obsługi komunikatów są wywoływane w kolejności, który jest przekazywany do **Utwórz** metody.</span><span class="sxs-lookup"><span data-stu-id="54a46-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="54a46-133">Ponieważ programy obsługi są zagnieżdżone, komunikat odpowiedzi jest wysyłane w drugą stronę.</span><span class="sxs-lookup"><span data-stu-id="54a46-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="54a46-134">Ostatnią procedurę obsługi jest pierwszy komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="54a46-134">That is, the last handler is the first to get the response message.</span></span>
