---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routingu we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077066"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="7d795-102">Routingu we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="7d795-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="7d795-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7d795-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7d795-104">W tym artykule opisano, jak ASP.NET Web API kieruje żądania HTTP do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7d795-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="7d795-105">Osoby zaznajomione z platformą ASP.NET MVC routingu internetowego interfejsu API jest bardzo podobny do routingu MVC.</span><span class="sxs-lookup"><span data-stu-id="7d795-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="7d795-106">Główną różnicą jest to, że interfejsu API sieci Web użyto zlecenie HTTP, nie ścieżka identyfikatora URI, aby wybrać akcję.</span><span class="sxs-lookup"><span data-stu-id="7d795-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="7d795-107">Umożliwia także MVC stylu routing w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7d795-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="7d795-108">W tym artykule nie zakłada żadnej wiedzy platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7d795-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="7d795-109">Tabele routingu</span><span class="sxs-lookup"><span data-stu-id="7d795-109">Routing Tables</span></span>

<span data-ttu-id="7d795-110">W programie ASP.NET Web API *kontrolera* to klasa, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="7d795-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="7d795-111">Metody publiczne kontrolera są nazywane *metod akcji* lub po prostu *akcje*.</span><span class="sxs-lookup"><span data-stu-id="7d795-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="7d795-112">Gdy struktury Web API odbiera żądanie, kieruje żądanie do operacji.</span><span class="sxs-lookup"><span data-stu-id="7d795-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="7d795-113">Aby ustalić, jaka akcja do wywołania, środowisko wykorzystuje *tabeli routingu*.</span><span class="sxs-lookup"><span data-stu-id="7d795-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="7d795-114">Szablon projektu Visual Studio dla interfejsu API sieci Web tworzy trasy domyślne:</span><span class="sxs-lookup"><span data-stu-id="7d795-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="7d795-115">Ta trasa jest zdefiniowana w *WebApiConfig.cs* pliku, który jest umieszczony w *aplikacji\_Start* katalogu:</span><span class="sxs-lookup"><span data-stu-id="7d795-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="7d795-116">Aby uzyskać więcej informacji na temat `WebApiConfig` klasy, zobacz [Konfigurowanie ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="7d795-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="7d795-117">Jeśli samodzielnego hostowania interfejsu API sieci Web, musisz ustawić w tabeli routingu bezpośrednio `HttpSelfHostConfiguration` obiektu.</span><span class="sxs-lookup"><span data-stu-id="7d795-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="7d795-118">Aby uzyskać więcej informacji, zobacz [samodzielnego hostowania interfejsu Web API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="7d795-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="7d795-119">Każdy wpis w tabeli routingu zawiera *szablon trasy*.</span><span class="sxs-lookup"><span data-stu-id="7d795-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="7d795-120">Domyślny szablon trasy dla interfejsu API sieci Web jest &quot;interfejsu api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="7d795-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="7d795-121">W tym szablonie &quot;api&quot; jest segmentu ścieżki literału, a {controller} i {id} są zmiennymi symbol zastępczy.</span><span class="sxs-lookup"><span data-stu-id="7d795-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="7d795-122">Gdy struktury Web API odbiera żądanie HTTP, próbuje dopasować identyfikatora URI na jednym z szablonów tras w tabeli routingu.</span><span class="sxs-lookup"><span data-stu-id="7d795-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="7d795-123">Jeśli żadna trasa nie pasuje, klient odbiera błąd 404.</span><span class="sxs-lookup"><span data-stu-id="7d795-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="7d795-124">Na przykład następujące identyfikatory URI zgodna trasy domyślnej:</span><span class="sxs-lookup"><span data-stu-id="7d795-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="7d795-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="7d795-125">/api/contacts</span></span>
- <span data-ttu-id="7d795-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="7d795-126">/api/contacts/1</span></span>
- <span data-ttu-id="7d795-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="7d795-127">/api/products/gizmo1</span></span>

<span data-ttu-id="7d795-128">Jednak następujący identyfikator URI nie jest zgodny, ponieważ brakuje &quot;api&quot; segmencie:</span><span class="sxs-lookup"><span data-stu-id="7d795-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="7d795-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="7d795-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="7d795-130">Przyczyna przy użyciu "interfejs api" dla trasy jest uniknąć kolizji z routing na platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7d795-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="7d795-131">Dzięki temu można mieć &quot;/kontaktuje się&quot; przejdź do kontrolera MVC i &quot;/api/contacts&quot; przejdź do kontrolera internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7d795-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="7d795-132">Oczywiście jeśli ta Konwencja nie lubisz, możesz zmienić tabela routingu domyślnego.</span><span class="sxs-lookup"><span data-stu-id="7d795-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="7d795-133">Po znalezieniu pasującego trasy interfejsu API sieci Web wybiera kontroler i akcję:</span><span class="sxs-lookup"><span data-stu-id="7d795-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="7d795-134">Aby odnaleźć kontrolera, dodaje interfejsu API sieci Web &quot;kontrolera&quot; wartość *{controller}* zmiennej.</span><span class="sxs-lookup"><span data-stu-id="7d795-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="7d795-135">Aby znaleźć akcję, internetowy interfejs API patrzy na zlecenie HTTP, a następnie szuka akcji, których nazwy zaczynają się o tej nazwie zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="7d795-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="7d795-136">Na przykład żądanie GET, internetowy interfejs API wyszukuje prefiksem akcję &quot;uzyskać&quot;, takich jak &quot;GetContact&quot; lub &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="7d795-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="7d795-137">Ta Konwencja dotyczy tylko GET, POST, PUT, DELETE, HEAD, opcje i stosowanie poprawek do zlecenia.</span><span class="sxs-lookup"><span data-stu-id="7d795-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="7d795-138">Pozostałe zlecenia HTTP można włączyć za pomocą atrybutów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7d795-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="7d795-139">Później zobaczymy przykład.</span><span class="sxs-lookup"><span data-stu-id="7d795-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="7d795-140">Inne zmienne symbol zastępczy w szablonie trasy, takich jak *{id}* są mapowane do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="7d795-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="7d795-141">Przyjrzyjmy się przykładowi.</span><span class="sxs-lookup"><span data-stu-id="7d795-141">Let's look at an example.</span></span> <span data-ttu-id="7d795-142">Załóżmy, że należy zdefiniować następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="7d795-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="7d795-143">Poniżej przedstawiono niektóre możliwe żądania HTTP wraz z akcji, która zostanie wywołana dla każdego:</span><span class="sxs-lookup"><span data-stu-id="7d795-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="7d795-144">Czasownik HTTP</span><span class="sxs-lookup"><span data-stu-id="7d795-144">HTTP Verb</span></span> | <span data-ttu-id="7d795-145">Ścieżka identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="7d795-145">URI Path</span></span> | <span data-ttu-id="7d795-146">Akcja</span><span class="sxs-lookup"><span data-stu-id="7d795-146">Action</span></span> | <span data-ttu-id="7d795-147">Parametr</span><span class="sxs-lookup"><span data-stu-id="7d795-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7d795-148">GET</span><span class="sxs-lookup"><span data-stu-id="7d795-148">GET</span></span> | <span data-ttu-id="7d795-149">Interfejs API/produktów</span><span class="sxs-lookup"><span data-stu-id="7d795-149">api/products</span></span> | <span data-ttu-id="7d795-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="7d795-150">GetAllProducts</span></span> | <span data-ttu-id="7d795-151">*(Brak)*</span><span class="sxs-lookup"><span data-stu-id="7d795-151">*(none)*</span></span> |
| <span data-ttu-id="7d795-152">GET</span><span class="sxs-lookup"><span data-stu-id="7d795-152">GET</span></span> | <span data-ttu-id="7d795-153">Interfejs API/produkty/4</span><span class="sxs-lookup"><span data-stu-id="7d795-153">api/products/4</span></span> | <span data-ttu-id="7d795-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="7d795-154">GetProductById</span></span> | <span data-ttu-id="7d795-155">4</span><span class="sxs-lookup"><span data-stu-id="7d795-155">4</span></span> |
| <span data-ttu-id="7d795-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="7d795-156">DELETE</span></span> | <span data-ttu-id="7d795-157">Interfejs API/produkty/4</span><span class="sxs-lookup"><span data-stu-id="7d795-157">api/products/4</span></span> | <span data-ttu-id="7d795-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="7d795-158">DeleteProduct</span></span> | <span data-ttu-id="7d795-159">4</span><span class="sxs-lookup"><span data-stu-id="7d795-159">4</span></span> |
| <span data-ttu-id="7d795-160">POST</span><span class="sxs-lookup"><span data-stu-id="7d795-160">POST</span></span> | <span data-ttu-id="7d795-161">Interfejs API/produktów</span><span class="sxs-lookup"><span data-stu-id="7d795-161">api/products</span></span> | <span data-ttu-id="7d795-162">*(Brak dopasowań)*</span><span class="sxs-lookup"><span data-stu-id="7d795-162">*(no match)*</span></span> |  |

<span data-ttu-id="7d795-163">Należy zauważyć, że *{id}* segmencie identyfikatora URI, jeśli jest obecny, jest mapowany na *identyfikator* parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="7d795-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="7d795-164">W tym przykładzie kontrolera definiuje dwie metody GET, jeden z *identyfikator* parametrów i bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="7d795-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="7d795-165">Ponadto dokonany wybór żądanie POST zakończy się niepowodzeniem, ponieważ kontroler nie definiują &quot;wpis... &quot; metody.</span><span class="sxs-lookup"><span data-stu-id="7d795-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="7d795-166">Zmiany routingu</span><span class="sxs-lookup"><span data-stu-id="7d795-166">Routing Variations</span></span>

<span data-ttu-id="7d795-167">W poprzedniej sekcji opisano podstawowy mechanizm routingu ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="7d795-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="7d795-168">W tej sekcji opisano pewne różnice.</span><span class="sxs-lookup"><span data-stu-id="7d795-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="7d795-169">Zlecenia HTTP</span><span class="sxs-lookup"><span data-stu-id="7d795-169">HTTP verbs</span></span>

<span data-ttu-id="7d795-170">Zamiast używania konwencji nazewnictwa dla zleceń HTTP, możesz jawnie określić czasownik HTTP dla akcji przez urządzanie metodę akcji przy użyciu jednego z następujących atrybutów:</span><span class="sxs-lookup"><span data-stu-id="7d795-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="7d795-171">W poniższym przykładzie `FindProduct` metody jest mapowany na żądania pobierania:</span><span class="sxs-lookup"><span data-stu-id="7d795-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="7d795-172">Aby umożliwić wielu poleceń HTTP dla akcji lub Zezwalaj na zlecenia HTTP inne niż GET, PUT, POST, DELETE, HEAD, opcji i poprawek, użyj `[AcceptVerbs]` atrybut, który przyjmuje listę zleceń HTTP.</span><span class="sxs-lookup"><span data-stu-id="7d795-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="7d795-173">Routing, dbając o Nazwa akcji</span><span class="sxs-lookup"><span data-stu-id="7d795-173">Routing by Action Name</span></span>

<span data-ttu-id="7d795-174">Szablon routingu domyślnego interfejsu API sieci Web używa zlecenie HTTP, aby wybrać akcję.</span><span class="sxs-lookup"><span data-stu-id="7d795-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="7d795-175">Jednak można również utworzyć trasę, gdy nazwa akcji znajduje się w identyfikatorze URI:</span><span class="sxs-lookup"><span data-stu-id="7d795-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="7d795-176">W tym szablonie trasy *{action}* nazwy parametrów metody akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7d795-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="7d795-177">Z tym stylem routingu umożliwia określanie dozwolonych poleceń HTTP atrybutów.</span><span class="sxs-lookup"><span data-stu-id="7d795-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="7d795-178">Na przykład załóżmy, że dany kontroler ma następującą metodę:</span><span class="sxs-lookup"><span data-stu-id="7d795-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="7d795-179">W tym przypadku żądania GET dla "interfejsu api/produkty/szczegóły/1" mapującej do `Details` metody.</span><span class="sxs-lookup"><span data-stu-id="7d795-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="7d795-180">Ten styl routingu jest podobny do wzorca ASP.NET MVC i może być odpowiedni dla stylu RPC interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7d795-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="7d795-181">Nazwa akcji można zastąpić za pomocą `[ActionName]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7d795-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="7d795-182">W poniższym przykładzie, istnieją dwie akcje, które mapują &quot;produkty/api/miniatur/*identyfikator*. Obsługuje jeden GET oraz drugi WPIS:</span><span class="sxs-lookup"><span data-stu-id="7d795-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="7d795-183">Bez akcji</span><span class="sxs-lookup"><span data-stu-id="7d795-183">Non-Actions</span></span>

<span data-ttu-id="7d795-184">Aby zapobiec sytuacji, w której metody pobierania wywołana jako akcję, należy użyć `[NonAction]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7d795-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="7d795-185">To sygnalizuje Framework, metoda nie jest działanie, nawet wtedy, gdy w przeciwnym razie dopasowanie nastąpi reguły routingu.</span><span class="sxs-lookup"><span data-stu-id="7d795-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="7d795-186">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="7d795-186">Further Reading</span></span>

<span data-ttu-id="7d795-187">W tym temacie podano ogólny widok routingu.</span><span class="sxs-lookup"><span data-stu-id="7d795-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="7d795-188">Aby uzyskać więcej informacji, zobacz [Routing i wybieranie akcji](routing-and-action-selection.md), która opisuje dokładnie tak jak struktura pasuje do identyfikatora URI do trasy, wybiera kontrolera i następnie wybiera akcję do wywołania.</span><span class="sxs-lookup"><span data-stu-id="7d795-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
