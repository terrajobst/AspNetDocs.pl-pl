---
uid: web-api/overview/advanced/http-message-handlers
title: Programy obsługi komunikatów HTTP we wzorcu ASP.NET Web API — ASP.NET 4.x
author: MikeWasson
description: Omówienie obsługi komunikatów HTTP w dla aplikacji ASP.NET Web API platformy ASP.NET 4.x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115548"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="37d7a-103">Programy obsługi komunikatów HTTP we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="37d7a-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="37d7a-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="37d7a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="37d7a-105">A *obsługi wiadomości* to klasa, która odbierze żądanie HTTP, a następnie zwraca odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="37d7a-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="37d7a-106">Programy obsługi komunikatów dziedziczyć abstrakcyjnej **klasa HttpMessageHandler** klasy.</span><span class="sxs-lookup"><span data-stu-id="37d7a-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="37d7a-107">Zazwyczaj szereg programy obsługi komunikatów są połączone.</span><span class="sxs-lookup"><span data-stu-id="37d7a-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="37d7a-108">Pierwszego programu obsługi odebrania żądania HTTP, przetwarza je i zapewnia żądania do następnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="37d7a-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="37d7a-109">W pewnym momencie odpowiedzi jest tworzony i powraca łańcuch.</span><span class="sxs-lookup"><span data-stu-id="37d7a-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="37d7a-110">Ten wzorzec jest nazywany *delegowanie* programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="37d7a-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="37d7a-111">Programy obsługi komunikatów po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="37d7a-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="37d7a-112">Po stronie serwera potok składnika Web API używa obsługi niektórych wbudowanych wiadomości:</span><span class="sxs-lookup"><span data-stu-id="37d7a-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="37d7a-113">**HttpServer** pobiera żądania od hosta.</span><span class="sxs-lookup"><span data-stu-id="37d7a-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="37d7a-114">**HttpRoutingDispatcher** wysyła żądania na podstawie trasy.</span><span class="sxs-lookup"><span data-stu-id="37d7a-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="37d7a-115">**HttpControllerDispatcher** wysyła żądanie do kontrolera internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="37d7a-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="37d7a-116">Niestandardowe programy obsługi można dodać do potoku.</span><span class="sxs-lookup"><span data-stu-id="37d7a-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="37d7a-117">Programy obsługi komunikatów dla zastosowań dobre są odciąż przekrojowe zagadnienia, które działają na poziomie HTTP wiadomości (zamiast akcji kontrolera).</span><span class="sxs-lookup"><span data-stu-id="37d7a-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="37d7a-118">Może na przykład program obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="37d7a-118">For example, a message handler might:</span></span>

- <span data-ttu-id="37d7a-119">Odczytywać lub modyfikować nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="37d7a-119">Read or modify request headers.</span></span>
- <span data-ttu-id="37d7a-120">Dodaj nagłówek odpowiedzi do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="37d7a-120">Add a response header to responses.</span></span>
- <span data-ttu-id="37d7a-121">Sprawdź poprawność żądania, zanim dotrą kontrolera.</span><span class="sxs-lookup"><span data-stu-id="37d7a-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="37d7a-122">Ten diagram przedstawia dwa niestandardowe programy obsługi dodaje do potoku:</span><span class="sxs-lookup"><span data-stu-id="37d7a-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="37d7a-123">Po stronie klienta HttpClient używa także procedury obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="37d7a-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="37d7a-124">Aby uzyskać więcej informacji, zobacz [programy obsługi komunikatów HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="37d7a-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="37d7a-125">Programy obsługi komunikatów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="37d7a-125">Custom Message Handlers</span></span>

<span data-ttu-id="37d7a-126">Aby napisać program obsługi komunikatów niestandardowych, pochodzi od **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="37d7a-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="37d7a-127">Ta metoda ma następujący podpis:</span><span class="sxs-lookup"><span data-stu-id="37d7a-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="37d7a-128">Ta metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="37d7a-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="37d7a-129">Typowa implementacja wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="37d7a-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="37d7a-130">Przetwarzanie komunikatu żądania.</span><span class="sxs-lookup"><span data-stu-id="37d7a-130">Process the request message.</span></span>
2. <span data-ttu-id="37d7a-131">Wywołaj `base.SendAsync` wysyłać żądania do wewnętrznego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="37d7a-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="37d7a-132">Wewnętrzny program obsługi zwraca komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="37d7a-132">The inner handler returns a response message.</span></span> <span data-ttu-id="37d7a-133">(Ten krok jest asynchroniczne).</span><span class="sxs-lookup"><span data-stu-id="37d7a-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="37d7a-134">Przetworzenie odpowiedzi i zwraca go do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="37d7a-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="37d7a-135">Oto przykład prosta:</span><span class="sxs-lookup"><span data-stu-id="37d7a-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="37d7a-136">Wywołanie `base.SendAsync` jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="37d7a-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="37d7a-137">Jeśli program obsługi ma wszelkie prace po tym wywołaniu, użyj **await** — słowo kluczowe, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="37d7a-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>

<span data-ttu-id="37d7a-138">Obsługi delegowania można również pominąć wewnętrznym programem obsługi i bezpośrednio tworzyć odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="37d7a-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="37d7a-139">Jeśli delegowanie obsługi tworzy odpowiedź bez wywoływania `base.SendAsync`, żądanie pomija pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="37d7a-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="37d7a-140">Może to być przydatne w przypadku obsługi, która weryfikuje żądanie (Tworzenie odpowiedź o błędzie).</span><span class="sxs-lookup"><span data-stu-id="37d7a-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="37d7a-141">Dodawanie obsługi do potoku</span><span class="sxs-lookup"><span data-stu-id="37d7a-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="37d7a-142">Aby dodać program obsługi komunikatów po stronie serwera, należy dodać program obsługi **HttpConfiguration.MessageHandlers** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="37d7a-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="37d7a-143">Jeśli szablon "Platformy ASP.NET MVC 4 aplikacja sieci Web" są używane do tworzenia projektu, możesz to zrobić to wewnątrz **WebApiConfig** klasy:</span><span class="sxs-lookup"><span data-stu-id="37d7a-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="37d7a-144">Programy obsługi komunikatów są wywoływane w tej samej kolejności, w jakiej występują w **MessageHandlers** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="37d7a-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="37d7a-145">Z powodu zagnieżdżenia, komunikat odpowiedzi jest wysyłane w drugą stronę.</span><span class="sxs-lookup"><span data-stu-id="37d7a-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="37d7a-146">Ostatnią procedurę obsługi jest pierwszy komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="37d7a-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="37d7a-147">Należy zauważyć, że nie musisz ustawić wewnętrzny obsługi; w ramach interfejsu API sieci Web automatycznie nawiązuje połączenie obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="37d7a-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="37d7a-148">Jeśli jesteś [hostingu samodzielnego](../older-versions/self-host-a-web-api.md), Utwórz wystąpienie obiektu **HttpSelfHostConfiguration** klasy i dodawanie obsługi do **MessageHandlers** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="37d7a-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="37d7a-149">Teraz Przyjrzyjmy się kilka przykładów programy obsługi komunikatów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="37d7a-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="37d7a-150">Przykład: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="37d7a-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="37d7a-151">X-HTTP-Method-Override jest niestandardowy nagłówek HTTP.</span><span class="sxs-lookup"><span data-stu-id="37d7a-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="37d7a-152">Jest ona przeznaczona dla klientów, którzy nie mogą wysyłać pewne typy żądań HTTP, takich jak PUT lub DELETE.</span><span class="sxs-lookup"><span data-stu-id="37d7a-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="37d7a-153">Zamiast tego klient wysyła żądanie POST i ustawia dla nagłówka X-HTTP-Method-Override żądanej metody.</span><span class="sxs-lookup"><span data-stu-id="37d7a-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="37d7a-154">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="37d7a-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="37d7a-155">Poniżej przedstawiono program obsługi komunikatów, który dodaje obsługę X-HTTP-Method-Override:</span><span class="sxs-lookup"><span data-stu-id="37d7a-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="37d7a-156">W **SendAsync** metody obsługi sprawdza, czy komunikat żądania jest żądaniem POST i czy zawiera nagłówek X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="37d7a-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="37d7a-157">Jeśli tak, sprawdza poprawność wartości nagłówka, a następnie modyfikuje metoda żądania.</span><span class="sxs-lookup"><span data-stu-id="37d7a-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="37d7a-158">Ponadto program obsługi wyjątku wywołuje `base.SendAsync` do przekazywania wiadomości do następnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="37d7a-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="37d7a-159">Gdy żądanie dotrze **HttpControllerDispatcher** klasy **HttpControllerDispatcher** będzie kierować żądania oparte na metodzie zaktualizowane żądanie.</span><span class="sxs-lookup"><span data-stu-id="37d7a-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="37d7a-160">Przykład: Dodawanie nagłówka odpowiedzi niestandardowych</span><span class="sxs-lookup"><span data-stu-id="37d7a-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="37d7a-161">Poniżej przedstawiono program obsługi komunikatów, umożliwiający dodawanie niestandardowego nagłówka do każdego komunikatu odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="37d7a-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="37d7a-162">Po pierwsze, program obsługi wyjątku wywołuje `base.SendAsync` do przekazania żądania do obsługi wiadomości wewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="37d7a-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="37d7a-163">Wewnętrzny program obsługi zwraca komunikat odpowiedzi, ale robi tak asynchronicznie przy użyciu **zadań&lt;T&gt;**  obiektu.</span><span class="sxs-lookup"><span data-stu-id="37d7a-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="37d7a-164">Komunikat odpowiedzi nie jest dostępna tylko do `base.SendAsync` kończy asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="37d7a-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="37d7a-165">W tym przykładzie użyto **await** — słowo kluczowe do wykonywania pracy asynchronicznie po `SendAsync` kończy.</span><span class="sxs-lookup"><span data-stu-id="37d7a-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="37d7a-166">Jeśli masz na celu platformy .NET Framework 4.0, użyj **zadań**&lt;T&gt;**. ContinueWith** metody:</span><span class="sxs-lookup"><span data-stu-id="37d7a-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="37d7a-167">Przykład: Wyszukiwanie klucza interfejsu API</span><span class="sxs-lookup"><span data-stu-id="37d7a-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="37d7a-168">Niektóre usługi sieci web wymagają klientów uwzględnić klucza interfejsu API w ich żądania.</span><span class="sxs-lookup"><span data-stu-id="37d7a-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="37d7a-169">Poniższy przykład pokazuje, jak program obsługi komunikatów można sprawdzić żądania, aby uzyskać prawidłowy klucz interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="37d7a-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="37d7a-170">Ten program obsługi szuka klucza interfejsu API w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="37d7a-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="37d7a-171">(W tym przykładzie przyjęto założenie, że klucz jest ciąg statyczny.</span><span class="sxs-lookup"><span data-stu-id="37d7a-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="37d7a-172">Rzeczywistej implementacji będzie prawdopodobnie użyć bardziej złożoną walidację). Jeśli ciąg zapytania zawiera klucz, program obsługi przekazuje żądanie do wewnętrznym programem obsługi.</span><span class="sxs-lookup"><span data-stu-id="37d7a-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="37d7a-173">Jeśli żądanie nie ma prawidłowego klucza, program obsługi tworzy komunikat odpowiedzi ze stanem 403, dostęp zabroniony.</span><span class="sxs-lookup"><span data-stu-id="37d7a-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="37d7a-174">W tym przypadku nie wywołuje program obsługi `base.SendAsync`, więc wewnętrznym programem obsługi nigdy nie odbiera żądanie, ani nie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="37d7a-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="37d7a-175">W związku z tym kontrolera można założyć, że wszystkie żądania przychodzące ma prawidłowy klucz interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="37d7a-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="37d7a-176">Jeśli klucz interfejsu API ma zastosowanie tylko do określonych akcji kontrolera, należy wziąć pod uwagę przy użyciu filtru akcji zamiast programu obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="37d7a-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="37d7a-177">Filtry akcji uruchamiania routingu identyfikator URI zostanie wykonane.</span><span class="sxs-lookup"><span data-stu-id="37d7a-177">Action filters run after URI routing is performed.</span></span>

## <a name="per-route-message-handlers"></a><span data-ttu-id="37d7a-178">Programy obsługi komunikatów Route</span><span class="sxs-lookup"><span data-stu-id="37d7a-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="37d7a-179">Programy obsługi w **HttpConfiguration.MessageHandlers** kolekcji są stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="37d7a-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="37d7a-180">Alternatywnie można dodać program obsługi komunikatów do określonej trasy, podczas definiowania trasy:</span><span class="sxs-lookup"><span data-stu-id="37d7a-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="37d7a-181">W tym przykładzie, jeśli pasuje do identyfikatora URI żądania "Route2", żądanie jest wysyłane do `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="37d7a-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="37d7a-182">Na poniższym diagramie przedstawiono potok dla tych na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="37d7a-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="37d7a-183">Należy zauważyć, że `MessageHandler2` zastępuje domyślny **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="37d7a-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="37d7a-184">W tym przykładzie `MessageHandler2` tworzy odpowiedź, a żądania które pasują do "Route2" nigdy nie przechodź do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="37d7a-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="37d7a-185">Dzięki temu można zastąpić całego mechanizmu kontrolera interfejsu API sieci Web przy użyciu własnego niestandardowego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="37d7a-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="37d7a-186">Alternatywnie program obsługi komunikatów dla trasy można delegować do **HttpControllerDispatcher**, który następnie wysyła do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="37d7a-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="37d7a-187">Poniższy kod przedstawia sposób konfigurowania tej trasy:</span><span class="sxs-lookup"><span data-stu-id="37d7a-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
