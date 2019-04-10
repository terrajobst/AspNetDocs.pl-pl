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
ms.openlocfilehash: bd52396064cd7007ee17705ba86b02aaf27cb4f0
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59401728"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="3d192-103">Programy obsługi komunikatów HttpClient w interfejsie Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3d192-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="3d192-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3d192-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3d192-105">A *obsługi wiadomości* to klasa, która odbierze żądanie HTTP, a następnie zwraca odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d192-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="3d192-106">Zazwyczaj szereg programy obsługi komunikatów są połączone.</span><span class="sxs-lookup"><span data-stu-id="3d192-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="3d192-107">Pierwszego programu obsługi odebrania żądania HTTP, przetwarza je i zapewnia żądania do następnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="3d192-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="3d192-108">W pewnym momencie odpowiedzi jest tworzony i powraca łańcuch.</span><span class="sxs-lookup"><span data-stu-id="3d192-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="3d192-109">Ten wzorzec jest nazywany *delegowanie* programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3d192-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="3d192-110">Po stronie klienta **HttpClient** klasa używa obsługi wiadomości do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="3d192-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="3d192-111">Domyślny program obsługi jest **HttpClientHandler**, który wysyła żądanie za pośrednictwem sieci i pobiera odpowiedź z serwera.</span><span class="sxs-lookup"><span data-stu-id="3d192-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="3d192-112">Programy obsługi komunikatów niestandardowych można wstawić do potoku klienta:</span><span class="sxs-lookup"><span data-stu-id="3d192-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="3d192-113">Web API platformy ASP.NET używa także procedury obsługi komunikatów po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="3d192-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="3d192-114">Aby uzyskać więcej informacji, zobacz [programów obsługi komunikatów HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="3d192-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="3d192-115">Programy obsługi komunikatów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="3d192-115">Custom Message Handlers</span></span>

<span data-ttu-id="3d192-116">Aby napisać program obsługi komunikatów niestandardowych, pochodzi od **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="3d192-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="3d192-117">W tym miejscu znajduje się sygnatura metody:</span><span class="sxs-lookup"><span data-stu-id="3d192-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="3d192-118">Ta metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="3d192-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="3d192-119">Typowa implementacja wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="3d192-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="3d192-120">Przetwarzanie komunikatu żądania.</span><span class="sxs-lookup"><span data-stu-id="3d192-120">Process the request message.</span></span>
2. <span data-ttu-id="3d192-121">Wywołaj `base.SendAsync` wysyłać żądania do wewnętrznego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3d192-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="3d192-122">Wewnętrzny program obsługi zwraca komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3d192-122">The inner handler returns a response message.</span></span> <span data-ttu-id="3d192-123">(Ten krok jest asynchroniczne).</span><span class="sxs-lookup"><span data-stu-id="3d192-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="3d192-124">Przetworzenie odpowiedzi i zwraca go do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="3d192-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="3d192-125">Program obsługi komunikatów, który dodaje niestandardowego nagłówka do żądania wychodzącego można znaleźć w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3d192-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="3d192-126">Wywołanie `base.SendAsync` jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="3d192-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="3d192-127">Jeśli program obsługi ma wszelkie prace po tym wywołaniu, użyj **await** słowo kluczowe, aby wznowić wykonywanie po zakończeniu działania metody.</span><span class="sxs-lookup"><span data-stu-id="3d192-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="3d192-128">Program obsługi, który rejestruje kody błędów można znaleźć w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3d192-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="3d192-129">Rejestrowanie, sama nie jest bardzo interesujące, ale w przykładzie pokazano, jak uzyskać w odpowiedzi wewnątrz programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3d192-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="3d192-130">Dodawanie programów obsługi wiadomości do potoku klienta</span><span class="sxs-lookup"><span data-stu-id="3d192-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="3d192-131">Aby dodać niestandardowe programy obsługi do **HttpClient**, użyj **HttpClientFactory.Create** metody:</span><span class="sxs-lookup"><span data-stu-id="3d192-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="3d192-132">Programy obsługi komunikatów są wywoływane w kolejności, który jest przekazywany do **Utwórz** metody.</span><span class="sxs-lookup"><span data-stu-id="3d192-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="3d192-133">Ponieważ programy obsługi są zagnieżdżone, komunikat odpowiedzi jest wysyłane w drugą stronę.</span><span class="sxs-lookup"><span data-stu-id="3d192-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="3d192-134">Ostatnią procedurę obsługi jest pierwszy komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3d192-134">That is, the last handler is the first to get the response message.</span></span>
