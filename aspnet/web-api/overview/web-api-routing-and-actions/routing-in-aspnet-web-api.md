---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing w interfejsie API sieci Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557611"
---
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="3e5d4-102">Routing w składniku Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3e5d4-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="3e5d4-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3e5d4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3e5d4-104">W tym artykule opisano sposób, w jaki interfejs API sieci Web ASP.NET kieruje żądania HTTP do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="3e5d4-105">Jeśli znasz ASP.NET MVC, routing interfejsu API sieci Web jest bardzo podobny do routingu MVC.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="3e5d4-106">Główną różnicą jest to, że interfejs API sieci Web używa zlecenia HTTP, a nie ścieżki identyfikatora URI, aby wybrać akcję.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="3e5d4-107">W interfejsie API sieci Web można również używać routingu w stylu MVC.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="3e5d4-108">W tym artykule nie założono żadnej znajomości ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="3e5d4-109">Tabele routingu</span><span class="sxs-lookup"><span data-stu-id="3e5d4-109">Routing Tables</span></span>

<span data-ttu-id="3e5d4-110">W interfejsie API sieci Web ASP.NET *kontroler* jest klasą, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="3e5d4-111">Metody publiczne kontrolera są nazywane *metodami akcji* lub po prostu *akcjami*.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="3e5d4-112">Gdy struktura internetowego interfejsu API odbiera żądanie, kieruje żądanie do akcji.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="3e5d4-113">Aby określić akcję do wywołania, struktura używa *tabeli routingu*.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="3e5d4-114">Szablon projektu programu Visual Studio dla interfejsu API sieci Web tworzy trasę domyślną:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="3e5d4-115">Ta trasa jest zdefiniowana w pliku *WebApiConfig.cs* , który znajduje się w *aplikacji\_Start* Directory:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="3e5d4-116">Aby uzyskać więcej informacji na temat klasy `WebApiConfig`, zobacz [Konfigurowanie interfejsu API sieci Web ASP.NET](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3e5d4-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="3e5d4-117">W przypadku samoobsługowego interfejsu API sieci Web należy ustawić tabelę routingu bezpośrednio na obiekcie `HttpSelfHostConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="3e5d4-118">Aby uzyskać więcej informacji, zobacz samoobsługowy [interfejs API sieci Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3e5d4-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="3e5d4-119">Każdy wpis w tabeli routingu zawiera *szablon trasy*.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="3e5d4-120">Domyślnym szablonem trasy dla interfejsu API sieci Web jest &quot;API/{Controller}/{ID}&quot;.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="3e5d4-121">W tym szablonie &quot;&quot; API jest segmentem ścieżki literału, a {Controller} i {ID} są zmiennymi symbolami zastępczymi.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="3e5d4-122">Gdy struktura internetowego interfejsu API odbiera żądanie HTTP, próbuje dopasować identyfikator URI do jednego z szablonów tras w tabeli routingu.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="3e5d4-123">W przypadku braku dopasowania trasy klient odbiera błąd 404.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="3e5d4-124">Na przykład następujące identyfikatory URI pasują do trasy domyślnej:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="3e5d4-125">/api/contacts</span><span class="sxs-lookup"><span data-stu-id="3e5d4-125">/api/contacts</span></span>
- <span data-ttu-id="3e5d4-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="3e5d4-126">/api/contacts/1</span></span>
- <span data-ttu-id="3e5d4-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="3e5d4-127">/api/products/gizmo1</span></span>

<span data-ttu-id="3e5d4-128">Jednak następujący identyfikator URI nie jest zgodny, ponieważ brakuje segmentu &quot;interfejsu API&quot;:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="3e5d4-129">/contacts/1</span><span class="sxs-lookup"><span data-stu-id="3e5d4-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="3e5d4-130">Powodem korzystania z funkcji "API" w marszrucie jest uniknięcie kolizji z routingiem ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="3e5d4-131">Dzięki temu można &quot;/Contacts&quot; przejść do kontrolera MVC i &quot;/API/Contacts&quot; przejdź do kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="3e5d4-132">Oczywiście, jeśli nie podoba Ci się ta konwencja, możesz zmienić domyślną tabelę tras.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="3e5d4-133">Po znalezieniu pasującej trasy interfejs API sieci Web wybiera kontroler i akcję:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="3e5d4-134">Aby znaleźć kontroler, interfejs API sieci Web dodaje&quot; kontrolera &quot;do wartości zmiennej *{Controller}* .</span><span class="sxs-lookup"><span data-stu-id="3e5d4-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="3e5d4-135">Aby znaleźć akcję, interfejs API sieci Web przegląda czasownik HTTP, a następnie szuka akcji, której nazwa zaczyna się od tej nazwy zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="3e5d4-136">Na przykład przy użyciu żądania GET interfejs API sieci Web szuka akcji poprzedzonej &quot;Get&quot;, takiej jak &quot;GetContact&quot; lub &quot;&quot;.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="3e5d4-137">Ta konwencja ma zastosowanie tylko do czasowników GET, POST, PUT, DELETE, szef, OPTIONS i PATCH.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="3e5d4-138">Inne zlecenia HTTP można włączyć przy użyciu atrybutów na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="3e5d4-139">Zobaczymy przykład tej późniejszej.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="3e5d4-140">Inne zmienne zastępcze w szablonie trasy, takie jak *{ID},* są mapowane na parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="3e5d4-141">Spójrzmy na przykład.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-141">Let's look at an example.</span></span> <span data-ttu-id="3e5d4-142">Załóżmy, że definiujesz następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="3e5d4-143">Poniżej przedstawiono niektóre możliwe żądania HTTP wraz z akcją, która jest wywoływana dla każdego z nich:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="3e5d4-144">Czasownik HTTP</span><span class="sxs-lookup"><span data-stu-id="3e5d4-144">HTTP Verb</span></span> | <span data-ttu-id="3e5d4-145">Ścieżka identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="3e5d4-145">URI Path</span></span> | <span data-ttu-id="3e5d4-146">Akcja</span><span class="sxs-lookup"><span data-stu-id="3e5d4-146">Action</span></span> | <span data-ttu-id="3e5d4-147">Parametr</span><span class="sxs-lookup"><span data-stu-id="3e5d4-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e5d4-148">GET</span><span class="sxs-lookup"><span data-stu-id="3e5d4-148">GET</span></span> | <span data-ttu-id="3e5d4-149">interfejsy API/produkty</span><span class="sxs-lookup"><span data-stu-id="3e5d4-149">api/products</span></span> | <span data-ttu-id="3e5d4-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="3e5d4-150">GetAllProducts</span></span> | <span data-ttu-id="3e5d4-151">*dawaj*</span><span class="sxs-lookup"><span data-stu-id="3e5d4-151">*(none)*</span></span> |
| <span data-ttu-id="3e5d4-152">GET</span><span class="sxs-lookup"><span data-stu-id="3e5d4-152">GET</span></span> | <span data-ttu-id="3e5d4-153">interfejsy API/produkty/4</span><span class="sxs-lookup"><span data-stu-id="3e5d4-153">api/products/4</span></span> | <span data-ttu-id="3e5d4-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="3e5d4-154">GetProductById</span></span> | <span data-ttu-id="3e5d4-155">4</span><span class="sxs-lookup"><span data-stu-id="3e5d4-155">4</span></span> |
| <span data-ttu-id="3e5d4-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="3e5d4-156">DELETE</span></span> | <span data-ttu-id="3e5d4-157">interfejsy API/produkty/4</span><span class="sxs-lookup"><span data-stu-id="3e5d4-157">api/products/4</span></span> | <span data-ttu-id="3e5d4-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="3e5d4-158">DeleteProduct</span></span> | <span data-ttu-id="3e5d4-159">4</span><span class="sxs-lookup"><span data-stu-id="3e5d4-159">4</span></span> |
| <span data-ttu-id="3e5d4-160">POST</span><span class="sxs-lookup"><span data-stu-id="3e5d4-160">POST</span></span> | <span data-ttu-id="3e5d4-161">interfejsy API/produkty</span><span class="sxs-lookup"><span data-stu-id="3e5d4-161">api/products</span></span> | <span data-ttu-id="3e5d4-162">*(brak dopasowania)*</span><span class="sxs-lookup"><span data-stu-id="3e5d4-162">*(no match)*</span></span> |  |

<span data-ttu-id="3e5d4-163">Zwróć uwagę, że segment *{ID}* identyfikatora URI (jeśli istnieje) jest mapowany na parametr *ID* akcji.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="3e5d4-164">W tym przykładzie kontroler definiuje dwie metody GET, jeden z parametrem *ID* i jeden bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="3e5d4-165">Należy również pamiętać, że żądanie POST zakończy się niepowodzeniem, ponieważ kontroler nie definiuje metody &quot;post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="3e5d4-166">Różnice routingu</span><span class="sxs-lookup"><span data-stu-id="3e5d4-166">Routing Variations</span></span>

<span data-ttu-id="3e5d4-167">W poprzedniej sekcji opisano mechanizm routingu podstawowego dla interfejsu API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="3e5d4-168">W tej sekcji opisano niektóre odmiany.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="3e5d4-169">czasowniki HTTP</span><span class="sxs-lookup"><span data-stu-id="3e5d4-169">HTTP verbs</span></span>

<span data-ttu-id="3e5d4-170">Zamiast używać konwencji nazewnictwa dla czasowników HTTP, można jawnie określić czasownik HTTP dla akcji, dekorowania nazwy metodę akcji z jednym z następujących atrybutów:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="3e5d4-171">W poniższym przykładzie metoda `FindProduct` jest mapowana na żądania GET:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="3e5d4-172">Aby zezwolić na wiele czasowników HTTP dla akcji lub zezwolić na zlecenia HTTP inne niż GET, PUT, POST, DELETE, NAGŁÓWKowy, OPTIONS i PATCH, Użyj atrybutu `[AcceptVerbs]`, który pobiera listę zleceń HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="3e5d4-173">Routing według nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="3e5d4-173">Routing by Action Name</span></span>

<span data-ttu-id="3e5d4-174">W przypadku domyślnego szablonu routingu interfejs API sieci Web używa zlecenia HTTP do wybrania akcji.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="3e5d4-175">Można jednak również utworzyć trasę, w której nazwa akcji jest uwzględniona w identyfikatorze URI:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="3e5d4-176">W tym szablonie trasy parametr *{Action}* nazywa metodę Action na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="3e5d4-177">Przy użyciu tego stylu routingu użyj atrybutów, aby określić dozwolone zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="3e5d4-178">Załóżmy na przykład, że kontroler ma następującą metodę:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="3e5d4-179">W takim przypadku żądanie GET dla "API/Products/Details" zostanie zmapowane na metodę `Details`.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="3e5d4-180">Ten styl routingu jest podobny do ASP.NET MVC i może być odpowiedni dla interfejsu API w stylu wywołania RPC.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="3e5d4-181">Można zastąpić nazwę akcji przy użyciu atrybutu `[ActionName]`.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="3e5d4-182">W poniższym przykładzie istnieją dwie akcje, które mapują na &quot;API/produkty/miniaturę/*Identyfikator*. Jeden obsługuje funkcję GET, a druga obsługuje wpis:</span><span class="sxs-lookup"><span data-stu-id="3e5d4-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="3e5d4-183">Akcje niedziałające</span><span class="sxs-lookup"><span data-stu-id="3e5d4-183">Non-Actions</span></span>

<span data-ttu-id="3e5d4-184">Aby zapobiec wywołaniu metody jako akcji, Użyj atrybutu `[NonAction]`.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="3e5d4-185">Sygnalizuje to platformę, która nie jest akcją, nawet jeśli w przeciwnym razie będzie zgodna z regułami routingu.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="3e5d4-186">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="3e5d4-186">Further Reading</span></span>

<span data-ttu-id="3e5d4-187">W tym temacie przedstawiono ogólny widok routingu.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="3e5d4-188">Aby uzyskać więcej szczegółów, zobacz [Routing i wybór akcji](routing-and-action-selection.md), który opisuje dokładnie, w jaki sposób architektura dopasowuje identyfikator URI do trasy, wybiera kontroler, a następnie wybiera akcję do wywołania.</span><span class="sxs-lookup"><span data-stu-id="3e5d4-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
