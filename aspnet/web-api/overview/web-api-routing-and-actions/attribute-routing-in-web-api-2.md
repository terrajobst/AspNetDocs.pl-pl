---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atrybut routingu we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 22eb2fd748d52ec95e813ada8b1bf3b4826ad573
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068903"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="d153e-102">Atrybut routingu we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="d153e-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="d153e-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d153e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d153e-104">*Routing* jest jak internetowy interfejs API dopasowuje się do identyfikatora URI do akcji.</span><span class="sxs-lookup"><span data-stu-id="d153e-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="d153e-105">Składnik Web API 2 obsługuje nowy typ routingu, o nazwie *trasowanie atrybutów*.</span><span class="sxs-lookup"><span data-stu-id="d153e-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="d153e-106">Jak wskazuje nazwa, routing atrybutu używa atrybutów do definiowania trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="d153e-107">Trasowanie atrybutów zapewnia większą kontrolę nad identyfikatory URI w interfejsie API sieci web.</span><span class="sxs-lookup"><span data-stu-id="d153e-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="d153e-108">Na przykład można łatwo utworzyć identyfikatory URI, które opisują hierarchie zasobów.</span><span class="sxs-lookup"><span data-stu-id="d153e-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="d153e-109">Wcześniej styl routingu o nazwie oparty na Konwencji routingu jest nadal w pełni obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d153e-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="d153e-110">W rzeczywistości można łączyć z obu tych technik, w tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="d153e-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="d153e-111">W tym temacie przedstawiono sposób włączania trasowanie atrybutów, a w tym artykule opisano różne opcje trasowanie atrybutów.</span><span class="sxs-lookup"><span data-stu-id="d153e-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="d153e-112">Aby uzyskać samouczek end-to-end, który używa trasowanie atrybutów, zobacz [Tworzenie interfejsu API REST przy użyciu atrybutu routingu w sieci Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="d153e-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d153e-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d153e-113">Prerequisites</span></span>

<span data-ttu-id="d153e-114">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="d153e-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="d153e-115">Można również użyć Menedżera pakietów NuGet, aby zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="d153e-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="d153e-116">Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="d153e-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d153e-117">Wprowadź następujące polecenie w oknie Konsola Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="d153e-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="d153e-118">Dlaczego atrybutu routingu?</span><span class="sxs-lookup"><span data-stu-id="d153e-118">Why Attribute Routing?</span></span>

<span data-ttu-id="d153e-119">Pierwsza wersja interfejsu API sieci Web używane *oparty na Konwencji* routingu.</span><span class="sxs-lookup"><span data-stu-id="d153e-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="d153e-120">W tym typie routingu należy zdefiniować jedną lub więcej szablonów tras, które są po prostu sparametryzowane ciągów.</span><span class="sxs-lookup"><span data-stu-id="d153e-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="d153e-121">Struktura odbiera żądanie, jest zgodna identyfikator URI dla szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="d153e-122">(Aby uzyskać więcej informacji na temat routingu opartego na Konwencji zobacz [routingu ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="d153e-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="d153e-123">Jedną z zalet routing oparty na Konwencji jest szablonów są definiowane w jednym miejscu i reguły routingu są stosowane spójnie we wszystkich kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="d153e-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="d153e-124">Niestety routing oparty na Konwencji utrudnia do obsługi niektórych wzorców identyfikatora URI, które są wspólne w interfejsów API RESTful.</span><span class="sxs-lookup"><span data-stu-id="d153e-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="d153e-125">Na przykład zasoby często zawierają zasoby podrzędne: Klienci mają zamówienia, filmy mają aktorów, książki mają autorzy i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="d153e-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="d153e-126">Jest naturalnym do tworzenia identyfikatorów URI, które odzwierciedlają te relacje:</span><span class="sxs-lookup"><span data-stu-id="d153e-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="d153e-127">Tego rodzaju identyfikatora URI jest trudne do utworzenia przy użyciu routingu opartego na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="d153e-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="d153e-128">Mimo że można to zrobić, wyniki nie skalują się dobrze, jeśli masz wiele kontrolerów lub typów zasobów.</span><span class="sxs-lookup"><span data-stu-id="d153e-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="d153e-129">Za pomocą atrybutu routingu jest prosta, aby zdefiniować trasę dla tego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="d153e-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="d153e-130">Możesz po prostu dodaj atrybut do akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="d153e-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="d153e-131">Poniżej przedstawiono niektóre wzorce, które atrybutu routingu powoduje, że łatwo.</span><span class="sxs-lookup"><span data-stu-id="d153e-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="d153e-132">**Przechowywanie wersji interfejsu API**</span><span class="sxs-lookup"><span data-stu-id="d153e-132">**API versioning**</span></span>

<span data-ttu-id="d153e-133">W tym przykładzie "/ api/v1/produktów" byłaby przekierowane do innego kontrolera niż "/ api/2/produktów".</span><span class="sxs-lookup"><span data-stu-id="d153e-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="d153e-134">**Przeciążona segmentów identyfikatora URI**</span><span class="sxs-lookup"><span data-stu-id="d153e-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="d153e-135">W tym przykładzie "1" jest numer zamówienia, ale "pending" mapuje do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="d153e-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="d153e-136">**Wieloma typami parametrów**</span><span class="sxs-lookup"><span data-stu-id="d153e-136">**Multiple parameter types**</span></span>

<span data-ttu-id="d153e-137">W tym przykładzie "1" jest numer zamówienia, ale "06/2013/16" Określa wartość typu date.</span><span class="sxs-lookup"><span data-stu-id="d153e-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="d153e-138">Włączanie trasowanie atrybutów</span><span class="sxs-lookup"><span data-stu-id="d153e-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="d153e-139">Aby włączyć routing atrybutów, należy wywołać **MapHttpAttributeRoutes** podczas konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d153e-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="d153e-140">Ta metoda rozszerzenia jest zdefiniowany w **System.Web.Http.HttpConfigurationExtensions** klasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="d153e-141">Routing atrybut może być łączone z [oparty na Konwencji](routing-in-aspnet-web-api.md) routingu.</span><span class="sxs-lookup"><span data-stu-id="d153e-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="d153e-142">Aby zdefiniować oparty na Konwencji tras, należy wywołać **MapHttpRoute** metody.</span><span class="sxs-lookup"><span data-stu-id="d153e-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="d153e-143">Aby uzyskać więcej informacji na temat konfigurowania interfejsu API sieci Web, zobacz [Konfigurowanie wzorca ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="d153e-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="d153e-144">Uwaga: Migrowanie z interfejsu Web API 1</span><span class="sxs-lookup"><span data-stu-id="d153e-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="d153e-145">Przed Web API 2 szablony projektu interfejsu API sieci Web wygenerowany kod w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d153e-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="d153e-146">Jeśli atrybut routing jest włączony, ten kod spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="d153e-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="d153e-147">Jeśli uaktualnisz istniejący projekt interfejsu API sieci Web do użycia trasowanie atrybutów, upewnij się zaktualizować ten kod konfiguracji do następujących:</span><span class="sxs-lookup"><span data-stu-id="d153e-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="d153e-148">Aby uzyskać więcej informacji, zobacz [Konfigurowanie internetowego interfejsu API z hostingu platformy ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="d153e-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="d153e-149">Dodawanie atrybutów trasy</span><span class="sxs-lookup"><span data-stu-id="d153e-149">Adding Route Attributes</span></span>

<span data-ttu-id="d153e-150">Oto przykład trasy definiowane przy użyciu atrybutu:</span><span class="sxs-lookup"><span data-stu-id="d153e-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="d153e-151">Ciąg &quot;klientów / {customerId} / orders&quot; jest szablon identyfikatora URI dla danej trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="d153e-152">Interfejs API sieci Web próbuje dopasować identyfikatora URI żądania do szablonu.</span><span class="sxs-lookup"><span data-stu-id="d153e-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="d153e-153">W tym przykładzie "klientów" i "orders" to segmentów literału, a "{customerId}" jest parametrem zmiennej.</span><span class="sxs-lookup"><span data-stu-id="d153e-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="d153e-154">Następujące identyfikatory URI będzie odpowiadać tego szablonu:</span><span class="sxs-lookup"><span data-stu-id="d153e-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="d153e-155">Można ograniczyć za pomocą dopasowywania [ograniczenia](#constraints), które zostały opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="d153e-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="d153e-156">Należy zauważyć, że &quot;{customerId}&quot; parametrów w szablonie trasy jest zgodna z nazwą *customerId* parametru w metodzie.</span><span class="sxs-lookup"><span data-stu-id="d153e-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="d153e-157">Gdy internetowy interfejs API wywołuje akcji kontrolera, próbuje powiązania parametrów trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="d153e-158">Na przykład, jeśli identyfikator URI jest `http://example.com/customers/1/orders`, interfejs API sieci Web próbuje można powiązać wartości "1" *customerId* parametru w akcji.</span><span class="sxs-lookup"><span data-stu-id="d153e-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="d153e-159">Szablon identyfikatora URI może mieć kilka parametrów:</span><span class="sxs-lookup"><span data-stu-id="d153e-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="d153e-160">Wszystkimi metodami kontrolera, które nie mają atrybutu trasy Użyj, routing oparty na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="d153e-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="d153e-161">W ten sposób można połączyć oba rodzaje routingu w tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="d153e-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="d153e-162">Metody HTTP</span><span class="sxs-lookup"><span data-stu-id="d153e-162">HTTP Methods</span></span>

<span data-ttu-id="d153e-163">Interfejs API sieci Web również wybiera akcje na podstawie metody HTTP żądania (GET, POST itp.).</span><span class="sxs-lookup"><span data-stu-id="d153e-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="d153e-164">Domyślnie internetowy interfejs API wygląda dopasowanie bez uwzględniania wielkości liter z początku nazwy metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d153e-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="d153e-165">Na przykład metody kontroler o nazwie `PutCustomers` dopasowuje żądanie HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="d153e-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="d153e-166">Możesz zastąpić tę Konwencję przez urządzanie metody z dowolnym następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="d153e-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="d153e-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="d153e-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="d153e-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="d153e-168">**[HttpGet]**</span></span>
- <span data-ttu-id="d153e-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="d153e-169">**[HttpHead]**</span></span>
- <span data-ttu-id="d153e-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="d153e-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="d153e-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="d153e-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="d153e-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="d153e-172">**[HttpPost]**</span></span>
- <span data-ttu-id="d153e-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="d153e-173">**[HttpPut]**</span></span>

<span data-ttu-id="d153e-174">Poniższy przykład mapuje metoda CreateBook żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d153e-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="d153e-175">Wszystkie inne metody HTTP, łącznie z metod niestandardowych do używania **AcceptVerbs** atrybut, który przyjmuje listę metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="d153e-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="d153e-176">Prefiksy trasy</span><span class="sxs-lookup"><span data-stu-id="d153e-176">Route Prefixes</span></span>

<span data-ttu-id="d153e-177">Często trasy w kontrolerze wszystkie identyfikatory rozpoczynają się przy użyciu tego samego prefiksu.</span><span class="sxs-lookup"><span data-stu-id="d153e-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="d153e-178">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d153e-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="d153e-179">Możesz ustawić Wspólny prefiks dla całego kontrolera, używając **[RoutePrefix]** atrybutu:</span><span class="sxs-lookup"><span data-stu-id="d153e-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="d153e-180">Użyj tyldy (~) dla atrybutu metody, aby przesłonić prefiks trasy:</span><span class="sxs-lookup"><span data-stu-id="d153e-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="d153e-181">Prefiks trasy może zawierać parametry:</span><span class="sxs-lookup"><span data-stu-id="d153e-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="d153e-182">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="d153e-182">Route Constraints</span></span>

<span data-ttu-id="d153e-183">Ograniczenia trasy pozwalają na określenie, jak są dopasowywane parametry w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="d153e-184">Ogólna składnia jest &quot;{parametr: ograniczenie}&quot;.</span><span class="sxs-lookup"><span data-stu-id="d153e-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="d153e-185">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d153e-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="d153e-186">W tym miejscu pierwsza trasa będzie można wybrać tylko jeśli &quot;identyfikator&quot; segmencie identyfikatora URI jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="d153e-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="d153e-187">W przeciwnym razie zostanie wybrany druga trasa.</span><span class="sxs-lookup"><span data-stu-id="d153e-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="d153e-188">W poniższej tabeli wymieniono ograniczenia, które są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d153e-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="d153e-189">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="d153e-189">Constraint</span></span> | <span data-ttu-id="d153e-190">Opis</span><span class="sxs-lookup"><span data-stu-id="d153e-190">Description</span></span> | <span data-ttu-id="d153e-191">Przykład</span><span class="sxs-lookup"><span data-stu-id="d153e-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d153e-192">alfa</span><span class="sxs-lookup"><span data-stu-id="d153e-192">alpha</span></span> | <span data-ttu-id="d153e-193">Dopasowuje wielkie lub małe litery alfabetu łacińskiego (a – z, A – Z)</span><span class="sxs-lookup"><span data-stu-id="d153e-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="d153e-194">{x: alfa}</span><span class="sxs-lookup"><span data-stu-id="d153e-194">{x:alpha}</span></span> |
| <span data-ttu-id="d153e-195">bool</span><span class="sxs-lookup"><span data-stu-id="d153e-195">bool</span></span> | <span data-ttu-id="d153e-196">Dopasowuje wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="d153e-196">Matches a Boolean value.</span></span> | <span data-ttu-id="d153e-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="d153e-197">{x:bool}</span></span> |
| <span data-ttu-id="d153e-198">datetime</span><span class="sxs-lookup"><span data-stu-id="d153e-198">datetime</span></span> | <span data-ttu-id="d153e-199">Dopasowuje **daty/godziny** wartość.</span><span class="sxs-lookup"><span data-stu-id="d153e-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="d153e-200">{x: Data i godzina}</span><span class="sxs-lookup"><span data-stu-id="d153e-200">{x:datetime}</span></span> |
| <span data-ttu-id="d153e-201">decimal</span><span class="sxs-lookup"><span data-stu-id="d153e-201">decimal</span></span> | <span data-ttu-id="d153e-202">Dopasowuje wartość dziesiętną.</span><span class="sxs-lookup"><span data-stu-id="d153e-202">Matches a decimal value.</span></span> | <span data-ttu-id="d153e-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="d153e-203">{x:decimal}</span></span> |
| <span data-ttu-id="d153e-204">double</span><span class="sxs-lookup"><span data-stu-id="d153e-204">double</span></span> | <span data-ttu-id="d153e-205">Dopasowuje wartość zmiennoprzecinkową 64-bitowych.</span><span class="sxs-lookup"><span data-stu-id="d153e-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="d153e-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="d153e-206">{x:double}</span></span> |
| <span data-ttu-id="d153e-207">float</span><span class="sxs-lookup"><span data-stu-id="d153e-207">float</span></span> | <span data-ttu-id="d153e-208">Dopasowuje wartość zmiennoprzecinkowa 32-bitowych.</span><span class="sxs-lookup"><span data-stu-id="d153e-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="d153e-209">{x: float}</span><span class="sxs-lookup"><span data-stu-id="d153e-209">{x:float}</span></span> |
| <span data-ttu-id="d153e-210">Identyfikator GUID</span><span class="sxs-lookup"><span data-stu-id="d153e-210">guid</span></span> | <span data-ttu-id="d153e-211">Dopasowuje wartość identyfikatora GUID.</span><span class="sxs-lookup"><span data-stu-id="d153e-211">Matches a GUID value.</span></span> | <span data-ttu-id="d153e-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="d153e-212">{x:guid}</span></span> |
| <span data-ttu-id="d153e-213">int</span><span class="sxs-lookup"><span data-stu-id="d153e-213">int</span></span> | <span data-ttu-id="d153e-214">Odpowiada wartości 32-bitową liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="d153e-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="d153e-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="d153e-215">{x:int}</span></span> |
| <span data-ttu-id="d153e-216">length</span><span class="sxs-lookup"><span data-stu-id="d153e-216">length</span></span> | <span data-ttu-id="d153e-217">Dopasowuje ciąg znaków o określonej długości lub w określonym zakresie długości.</span><span class="sxs-lookup"><span data-stu-id="d153e-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="d153e-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="d153e-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="d153e-219">long</span><span class="sxs-lookup"><span data-stu-id="d153e-219">long</span></span> | <span data-ttu-id="d153e-220">Dopasowuje wartość 64-bitową liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="d153e-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="d153e-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="d153e-221">{x:long}</span></span> |
| <span data-ttu-id="d153e-222">max</span><span class="sxs-lookup"><span data-stu-id="d153e-222">max</span></span> | <span data-ttu-id="d153e-223">Reprezentuje liczbą całkowitą, przy czym wartość maksymalna.</span><span class="sxs-lookup"><span data-stu-id="d153e-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="d153e-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="d153e-224">{x:max(10)}</span></span> |
| <span data-ttu-id="d153e-225">Element MaxLength</span><span class="sxs-lookup"><span data-stu-id="d153e-225">maxlength</span></span> | <span data-ttu-id="d153e-226">Dopasowuje ciąg o maksymalnej długości.</span><span class="sxs-lookup"><span data-stu-id="d153e-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="d153e-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="d153e-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="d153e-228">min</span><span class="sxs-lookup"><span data-stu-id="d153e-228">min</span></span> | <span data-ttu-id="d153e-229">Reprezentuje liczbą całkowitą o określonej wartości minimalnej.</span><span class="sxs-lookup"><span data-stu-id="d153e-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="d153e-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="d153e-230">{x:min(10)}</span></span> |
| <span data-ttu-id="d153e-231">Element MinLength</span><span class="sxs-lookup"><span data-stu-id="d153e-231">minlength</span></span> | <span data-ttu-id="d153e-232">Dopasowuje ciąg o długości minimalnej.</span><span class="sxs-lookup"><span data-stu-id="d153e-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="d153e-233">{x:minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="d153e-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="d153e-234">range</span><span class="sxs-lookup"><span data-stu-id="d153e-234">range</span></span> | <span data-ttu-id="d153e-235">Pasuje do liczby całkowitej mającej zakresu wartości.</span><span class="sxs-lookup"><span data-stu-id="d153e-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="d153e-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="d153e-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="d153e-237">regex</span><span class="sxs-lookup"><span data-stu-id="d153e-237">regex</span></span> | <span data-ttu-id="d153e-238">Pasuje do wyrażenia regularnego.</span><span class="sxs-lookup"><span data-stu-id="d153e-238">Matches a regular expression.</span></span> | <span data-ttu-id="d153e-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="d153e-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="d153e-240">Zwróć uwagę niektórych ograniczeń, takich jak &quot;min&quot;, przyjmują argumentów w nawiasach.</span><span class="sxs-lookup"><span data-stu-id="d153e-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="d153e-241">Można zastosować wiele ograniczeń za pomocą parametru oddzieloną dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="d153e-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="d153e-242">Ograniczenia trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="d153e-242">Custom Route Constraints</span></span>

<span data-ttu-id="d153e-243">Można utworzyć ograniczenia trasy niestandardowe, implementując **IHttpRouteConstraint** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="d153e-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="d153e-244">Na przykład następujące ograniczenia ogranicza parametr na wartość całkowitą od zera.</span><span class="sxs-lookup"><span data-stu-id="d153e-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="d153e-245">Poniższy kod przedstawia sposób rejestrowania ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="d153e-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="d153e-246">Teraz można zastosować ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="d153e-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="d153e-247">Możesz również zastąpić całą **DefaultInlineConstraintResolver** klasy przez zaimplementowanie **IInlineConstraintResolver** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="d153e-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="d153e-248">To spowoduje zastąpienie wszystkie wbudowane ograniczenia, chyba że implementacja **IInlineConstraintResolver** specjalnie dodaje je.</span><span class="sxs-lookup"><span data-stu-id="d153e-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="d153e-249">Parametry opcjonalne identyfikatora URI i wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="d153e-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="d153e-250">Istnieje możliwość parametru identyfikatora URI opcjonalne, dodając znak zapytania do parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="d153e-251">Jeśli parametr trasy jest opcjonalny, zdefiniuj wartość domyślną dla parametru metody.</span><span class="sxs-lookup"><span data-stu-id="d153e-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="d153e-252">W tym przykładzie `/api/books/locale/1033` i `/api/books/locale` zwracać ten sam zasób.</span><span class="sxs-lookup"><span data-stu-id="d153e-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="d153e-253">Alternatywnie można określić wartość domyślną wewnątrz szablonu trasy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d153e-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="d153e-254">Jest prawie taki sam, jak w poprzednim przykładzie, ale istnieje niewielka różnica zachowanie po zastosowaniu wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="d153e-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="d153e-255">W pierwszym przykładzie ("{lcid?}") wartość domyślna 1033, to przypisać bezpośrednio do parametru metody, tak aby było to dokładna wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="d153e-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="d153e-256">W drugim przykładzie ("{lcid = 1033}"), wartość domyślna "1033" przechodzi przez proces wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="d153e-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="d153e-257">Integrator modelu domyślne przekonwertuje "1033" na wartość liczbową 1033.</span><span class="sxs-lookup"><span data-stu-id="d153e-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="d153e-258">Można jednak dodatku niestandardowego integratora modelu, który może zrobić coś inaczej.</span><span class="sxs-lookup"><span data-stu-id="d153e-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="d153e-259">(W większości przypadków, chyba że masz niestandardowe integratorów modeli w potoku, dwa formularze będą równoważne).</span><span class="sxs-lookup"><span data-stu-id="d153e-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="d153e-260">Nazwy tras</span><span class="sxs-lookup"><span data-stu-id="d153e-260">Route Names</span></span>

<span data-ttu-id="d153e-261">W interfejsie API sieci Web co trasy o nazwie.</span><span class="sxs-lookup"><span data-stu-id="d153e-261">In Web API, every route has a name.</span></span> <span data-ttu-id="d153e-262">Nazwy tras przydają się podczas generowania łączy, tak że można uwzględnić łącze w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d153e-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="d153e-263">Aby określić nazwę trasy, ustaw **nazwa** właściwości w atrybucie.</span><span class="sxs-lookup"><span data-stu-id="d153e-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="d153e-264">Poniższy przykład pokazuje, jak ustawić nazwę trasy, a także jak używać nazwy trasy podczas generowania łącza.</span><span class="sxs-lookup"><span data-stu-id="d153e-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="d153e-265">Kolejność trasy</span><span class="sxs-lookup"><span data-stu-id="d153e-265">Route Order</span></span>

<span data-ttu-id="d153e-266">Kiedy struktura próbuje dopasować identyfikatora URI z trasą, ocenia trasy w określonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="d153e-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="d153e-267">Aby określić kolejność, ustaw **kolejności** właściwość atrybut trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="d153e-268">Niższe wartości są obliczane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="d153e-268">Lower values are evaluated first.</span></span> <span data-ttu-id="d153e-269">Wartość domyślna kolejności wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="d153e-269">The default order value is zero.</span></span>

<span data-ttu-id="d153e-270">Poniżej przedstawiono sposób ustalania kolejności, łączna liczba:</span><span class="sxs-lookup"><span data-stu-id="d153e-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="d153e-271">Porównaj **kolejności** właściwość atrybut trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="d153e-272">Spójrz na każdym segmencie identyfikatora URI w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="d153e-273">Dla każdego segmentu kolejność w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d153e-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="d153e-274">Literał segmenty.</span><span class="sxs-lookup"><span data-stu-id="d153e-274">Literal segments.</span></span>
    2. <span data-ttu-id="d153e-275">Parametry trasy z ograniczeniami.</span><span class="sxs-lookup"><span data-stu-id="d153e-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="d153e-276">Parametry trasy bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="d153e-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="d153e-277">Symbol wieloznaczny parametr segmentów z ograniczeniami.</span><span class="sxs-lookup"><span data-stu-id="d153e-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="d153e-278">Symbol wieloznaczny segmenty parametru bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="d153e-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="d153e-279">W przypadku tie trasy są uporządkowane według porównania bez uwzględniania wielkości liter ciągu porządkowe ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="d153e-280">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="d153e-280">Here is an example.</span></span> <span data-ttu-id="d153e-281">Załóżmy, że należy zdefiniować następujące kontrolera:</span><span class="sxs-lookup"><span data-stu-id="d153e-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="d153e-282">Te trasy są uporządkowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="d153e-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="d153e-283">Szczegóły/zamówień</span><span class="sxs-lookup"><span data-stu-id="d153e-283">orders/details</span></span>
2. <span data-ttu-id="d153e-284">zamówienia / {id}</span><span class="sxs-lookup"><span data-stu-id="d153e-284">orders/{id}</span></span>
3. <span data-ttu-id="d153e-285">zamówienia / {customerName}</span><span class="sxs-lookup"><span data-stu-id="d153e-285">orders/{customerName}</span></span>
4. <span data-ttu-id="d153e-286">zamówienia / {\*date}</span><span class="sxs-lookup"><span data-stu-id="d153e-286">orders/{\*date}</span></span>
5. <span data-ttu-id="d153e-287">zamówienia / oczekujące</span><span class="sxs-lookup"><span data-stu-id="d153e-287">orders/pending</span></span>

<span data-ttu-id="d153e-288">Zwróć uwagę, że "szczegóły" jest literał segmentu i pojawia się przed "{id}", ale "oczekujące" pojawia się ostatnio ponieważ **kolejności** właściwość ma wartość 1.</span><span class="sxs-lookup"><span data-stu-id="d153e-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="d153e-289">(W tym przykładzie przyjęto założenie, są nie klientów o nazwie "szczegóły" lub "pending".</span><span class="sxs-lookup"><span data-stu-id="d153e-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="d153e-290">Ogólnie rzecz biorąc Staraj się unikać niejednoznaczne trasy.</span><span class="sxs-lookup"><span data-stu-id="d153e-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="d153e-291">W tym przykładzie lepsze szablon trasy dla `GetByCustomer` jest "klientów / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="d153e-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
