---
uid: web-api/overview/advanced/http-message-handlers
title: Procedury obsługi komunikatów HTTP w interfejsie Web API ASP.NET — ASP.NET 4. x
author: MikeWasson
description: Omówienie obsługi komunikatów HTTP w interfejsie Web API ASP.NET dla ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622585"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="81b1b-103">Procedury obsługi komunikatów HTTP w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="81b1b-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="81b1b-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="81b1b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="81b1b-105">*Program obsługi komunikatów* jest klasą, która odbiera żądanie HTTP i zwraca odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="81b1b-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="81b1b-106">Procedury obsługi komunikatów pochodzą od abstrakcyjnej klasy **HttpMessageHandler** .</span><span class="sxs-lookup"><span data-stu-id="81b1b-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="81b1b-107">Zwykle seria programów obsługi komunikatów jest łańcuchowo.</span><span class="sxs-lookup"><span data-stu-id="81b1b-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="81b1b-108">Pierwsza procedura obsługi odbiera żądanie HTTP, wykonuje pewne przetwarzanie i przekazuje żądanie do kolejnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="81b1b-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="81b1b-109">W pewnym momencie zostanie utworzona odpowiedź i przejdzie tworzenie kopii zapasowej łańcucha.</span><span class="sxs-lookup"><span data-stu-id="81b1b-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="81b1b-110">Ten wzorzec jest nazywany programem obsługi *delegowania* .</span><span class="sxs-lookup"><span data-stu-id="81b1b-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="81b1b-111">Procedury obsługi komunikatów po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="81b1b-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="81b1b-112">Po stronie serwera potok interfejsu API sieci Web używa wbudowanych programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="81b1b-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="81b1b-113">**HttpServer** pobiera żądanie od hosta.</span><span class="sxs-lookup"><span data-stu-id="81b1b-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="81b1b-114">**HttpRoutingDispatcher** wysyła żądanie na podstawie trasy.</span><span class="sxs-lookup"><span data-stu-id="81b1b-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="81b1b-115">**HttpControllerDispatcher** wysyła żądanie do kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="81b1b-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="81b1b-116">Możesz dodać niestandardowe programy obsługi do potoku.</span><span class="sxs-lookup"><span data-stu-id="81b1b-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="81b1b-117">Procedury obsługi komunikatów są przydatne w przypadku zagadnień związanych z wycinaniem, które działają na poziomie komunikatów HTTP (a nie w ramach akcji kontrolera).</span><span class="sxs-lookup"><span data-stu-id="81b1b-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="81b1b-118">Na przykład program obsługi komunikatów może:</span><span class="sxs-lookup"><span data-stu-id="81b1b-118">For example, a message handler might:</span></span>

- <span data-ttu-id="81b1b-119">Odczytaj lub zmodyfikuj nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="81b1b-119">Read or modify request headers.</span></span>
- <span data-ttu-id="81b1b-120">Dodaj nagłówek odpowiedzi do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="81b1b-120">Add a response header to responses.</span></span>
- <span data-ttu-id="81b1b-121">Weryfikuj żądania przed osiągnięciem kontrolera.</span><span class="sxs-lookup"><span data-stu-id="81b1b-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="81b1b-122">Ten diagram przedstawia dwa niestandardowe programy obsługi wstawione do potoku:</span><span class="sxs-lookup"><span data-stu-id="81b1b-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="81b1b-123">Po stronie klienta program HttpClient również używa programów obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="81b1b-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="81b1b-124">Aby uzyskać więcej informacji, zobacz [procedury obsługi komunikatów HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="81b1b-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="81b1b-125">Niestandardowe programy obsługi komunikatów</span><span class="sxs-lookup"><span data-stu-id="81b1b-125">Custom Message Handlers</span></span>

<span data-ttu-id="81b1b-126">Aby napisać niestandardową procedurę obsługi komunikatów, pochodzi od **systemu .NET. http. DelegatingHandler** i przesłania metodę **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="81b1b-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="81b1b-127">Ta metoda ma następujący podpis:</span><span class="sxs-lookup"><span data-stu-id="81b1b-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="81b1b-128">Metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="81b1b-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="81b1b-129">Typowa implementacja wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="81b1b-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="81b1b-130">Przetwórz komunikat żądania.</span><span class="sxs-lookup"><span data-stu-id="81b1b-130">Process the request message.</span></span>
2. <span data-ttu-id="81b1b-131">Wywołaj `base.SendAsync`, aby wysłać żądanie do wewnętrznej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="81b1b-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="81b1b-132">Procedura obsługi wewnętrznej zwraca komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="81b1b-132">The inner handler returns a response message.</span></span> <span data-ttu-id="81b1b-133">(Ten krok jest asynchroniczny).</span><span class="sxs-lookup"><span data-stu-id="81b1b-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="81b1b-134">Przetwórz odpowiedź i zwróć ją do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="81b1b-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="81b1b-135">Oto prosty przykład:</span><span class="sxs-lookup"><span data-stu-id="81b1b-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="81b1b-136">Wywołanie `base.SendAsync` jest asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="81b1b-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="81b1b-137">Jeśli program obsługi wykonuje wszelkie działania po tym wywołaniu, użyj słowa kluczowego **await** , jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="81b1b-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>

<span data-ttu-id="81b1b-138">Procedura delegowania może również pominąć wewnętrzną procedurę obsługi i bezpośrednio utworzyć odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="81b1b-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="81b1b-139">Jeśli program obsługi delegowania Tworzy odpowiedź bez wywoływania `base.SendAsync`, żądanie pomija resztę potoku.</span><span class="sxs-lookup"><span data-stu-id="81b1b-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="81b1b-140">Może to być przydatne w przypadku programu obsługi, który sprawdza poprawność żądania (Tworzenie odpowiedzi na błąd).</span><span class="sxs-lookup"><span data-stu-id="81b1b-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="81b1b-141">Dodawanie programu obsługi do potoku</span><span class="sxs-lookup"><span data-stu-id="81b1b-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="81b1b-142">Aby dodać program obsługi komunikatów po stronie serwera, Dodaj program obsługi do kolekcji **HttpConfiguration. MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="81b1b-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="81b1b-143">Jeśli do utworzenia projektu użyto szablonu "aplikacja sieci Web ASP.NET MVC 4", można to zrobić wewnątrz klasy **WebApiConfig** :</span><span class="sxs-lookup"><span data-stu-id="81b1b-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="81b1b-144">Procedury obsługi komunikatów są wywoływane w takiej samej kolejności, w jakiej występują w kolekcji **MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="81b1b-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="81b1b-145">Ponieważ są one zagnieżdżone, komunikat odpowiedzi jest przesyłany w innym kierunku.</span><span class="sxs-lookup"><span data-stu-id="81b1b-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="81b1b-146">Oznacza to, że ostatnim programem obsługi jest pierwszy, aby uzyskać komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="81b1b-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="81b1b-147">Zwróć uwagę, że nie musisz ustawiać wewnętrznych programów obsługi; Struktura internetowego interfejsu API automatycznie łączy programy obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="81b1b-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="81b1b-148">W przypadku samoobsługowego [udostępniania](../older-versions/self-host-a-web-api.md)Utwórz wystąpienie klasy **HttpSelfHostConfiguration** i Dodaj programy obsługi do kolekcji **MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="81b1b-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="81b1b-149">Teraz przyjrzyjmy się kilku przykładom niestandardowych programów obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="81b1b-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="81b1b-150">Przykład: X-HTTP-Method-override</span><span class="sxs-lookup"><span data-stu-id="81b1b-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="81b1b-151">Metoda X-HTTP-Method-override jest niestandardowym nagłówkiem HTTP.</span><span class="sxs-lookup"><span data-stu-id="81b1b-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="81b1b-152">Jest ona przeznaczona dla klientów, którzy nie mogą wysyłać określonych typów żądań HTTP, takich jak PUT lub DELETE.</span><span class="sxs-lookup"><span data-stu-id="81b1b-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="81b1b-153">Zamiast tego klient wysyła żądanie POST i ustawia do odpowiedniej metody nagłówek X-HTTP-Method-override.</span><span class="sxs-lookup"><span data-stu-id="81b1b-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="81b1b-154">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="81b1b-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="81b1b-155">Oto procedura obsługi komunikatów, która dodaje obsługę funkcji X-HTTP-Method-override:</span><span class="sxs-lookup"><span data-stu-id="81b1b-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="81b1b-156">W metodzie **SendAsync** program obsługi sprawdza, czy komunikat żądania jest żądaniem post i czy zawiera nagłówek X-HTTP-Method-override.</span><span class="sxs-lookup"><span data-stu-id="81b1b-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="81b1b-157">Jeśli tak, sprawdza poprawność wartości nagłówka, a następnie modyfikuje metodę żądania.</span><span class="sxs-lookup"><span data-stu-id="81b1b-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="81b1b-158">Na koniec program obsługi wywołuje `base.SendAsync`, aby przekazać komunikat do kolejnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="81b1b-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="81b1b-159">Gdy żądanie osiągnie klasę **HttpControllerDispatcher** , **HttpControllerDispatcher** kieruje żądanie na podstawie zaktualizowanej metody żądania.</span><span class="sxs-lookup"><span data-stu-id="81b1b-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="81b1b-160">Przykład: Dodawanie niestandardowego nagłówka odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="81b1b-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="81b1b-161">Oto procedura obsługi komunikatów, która dodaje niestandardowy nagłówek do każdego komunikatu odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="81b1b-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="81b1b-162">Najpierw program obsługi wywołuje `base.SendAsync`, aby przekazać żądanie do wewnętrznego programu obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="81b1b-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="81b1b-163">Procedura obsługi wewnętrznej zwraca komunikat odpowiedzi, ale robi to asynchronicznie przy użyciu **&lt;zadania t&gt;** obiektu.</span><span class="sxs-lookup"><span data-stu-id="81b1b-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="81b1b-164">Komunikat odpowiedzi nie jest dostępny, dopóki `base.SendAsync` nie zostanie ukończona asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="81b1b-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="81b1b-165">W tym przykładzie za pomocą słowa kluczowego **await** można wykonać pracę asynchroniczną po zakończeniu `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="81b1b-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="81b1b-166">Jeśli jesteś celem .NET Framework 4,0, użyj **zadania**&lt;t&gt; **. ContinueWith** :</span><span class="sxs-lookup"><span data-stu-id="81b1b-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="81b1b-167">Przykład: sprawdzanie klucza interfejsu API</span><span class="sxs-lookup"><span data-stu-id="81b1b-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="81b1b-168">Niektóre usługi sieci Web wymagają od klientów dołączenia klucza interfejsu API do żądania.</span><span class="sxs-lookup"><span data-stu-id="81b1b-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="81b1b-169">Poniższy przykład pokazuje, jak program obsługi komunikatów może sprawdzić żądania dla prawidłowego klucza interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="81b1b-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="81b1b-170">Ta procedura obsługi wyszukuje klucz interfejsu API w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="81b1b-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="81b1b-171">(W tym przykładzie Załóżmy, że klucz jest ciągiem statycznym.</span><span class="sxs-lookup"><span data-stu-id="81b1b-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="81b1b-172">Rzeczywista implementacja prawdopodobnie użyje bardziej złożonej walidacji.) Jeśli ciąg zapytania zawiera klucz, program obsługi przekaże żądanie do wewnętrznej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="81b1b-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="81b1b-173">Jeśli żądanie nie ma prawidłowego klucza, program obsługi tworzy komunikat odpowiedzi ze stanem 403, dostęp zabroniony.</span><span class="sxs-lookup"><span data-stu-id="81b1b-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="81b1b-174">W takim przypadku program obsługi nie wywołuje `base.SendAsync`, więc wewnętrzna procedura obsługi nigdy nie otrzymuje żądania ani nie jest kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="81b1b-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="81b1b-175">W związku z tym kontroler może założyć, że wszystkie żądania przychodzące mają prawidłowy klucz interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="81b1b-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="81b1b-176">Jeśli klucz interfejsu API dotyczy tylko niektórych akcji kontrolera, należy rozważyć użycie filtru akcji zamiast procedury obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="81b1b-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="81b1b-177">Filtry akcji są uruchamiane po wykonaniu routingu URI.</span><span class="sxs-lookup"><span data-stu-id="81b1b-177">Action filters run after URI routing is performed.</span></span>

## <a name="per-route-message-handlers"></a><span data-ttu-id="81b1b-178">Procedury obsługi komunikatów na trasie</span><span class="sxs-lookup"><span data-stu-id="81b1b-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="81b1b-179">Procedury obsługi w kolekcji **HttpConfiguration. MessageHandlers** mają zastosowanie globalnie.</span><span class="sxs-lookup"><span data-stu-id="81b1b-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="81b1b-180">Alternatywnie można dodać program obsługi komunikatów do określonej trasy podczas definiowania trasy:</span><span class="sxs-lookup"><span data-stu-id="81b1b-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="81b1b-181">W tym przykładzie, jeśli identyfikator URI żądania pasuje do "Route2", żądanie jest wysyłane do `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="81b1b-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="81b1b-182">Na poniższym diagramie przedstawiono potok dla tych dwóch tras:</span><span class="sxs-lookup"><span data-stu-id="81b1b-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="81b1b-183">Zwróć uwagę, że `MessageHandler2` zastępuje domyślny **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="81b1b-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="81b1b-184">W tym przykładzie `MessageHandler2` tworzy odpowiedź i żądania, które pasują do "Route2", nigdy nie przechodzą do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="81b1b-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="81b1b-185">Dzięki temu można zamienić cały mechanizm kontrolera internetowego interfejsu API na własny niestandardowy punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="81b1b-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="81b1b-186">Alternatywnie program obsługi komunikatów na trasie może delegować do **HttpControllerDispatcher**, który następnie wysyła do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="81b1b-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="81b1b-187">Poniższy kod przedstawia sposób konfigurowania tej trasy:</span><span class="sxs-lookup"><span data-stu-id="81b1b-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
