---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routing atrybutów w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554986"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="77b09-102">Routing atrybutów w interfejsie Web API ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="77b09-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="77b09-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="77b09-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="77b09-104">*Routing* to sposób, w jaki interfejs API sieci Web dopasowuje identyfikator URI do akcji.</span><span class="sxs-lookup"><span data-stu-id="77b09-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="77b09-105">Interfejs Web API 2 obsługuje nowy typ routingu nazywany *routingiem atrybutu*.</span><span class="sxs-lookup"><span data-stu-id="77b09-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="77b09-106">Jak nazwa oznacza, funkcja routingu atrybutów używa atrybutów do definiowania tras.</span><span class="sxs-lookup"><span data-stu-id="77b09-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="77b09-107">Routing atrybutu daje większą kontrolę nad identyfikatorami URI w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77b09-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="77b09-108">Można na przykład łatwo tworzyć identyfikatory URI opisujące hierarchie zasobów.</span><span class="sxs-lookup"><span data-stu-id="77b09-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="77b09-109">Wcześniejszy styl routingu o nazwie routing oparty na Konwencji jest nadal w pełni obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="77b09-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="77b09-110">W rzeczywistości można połączyć obie techniki w tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="77b09-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="77b09-111">W tym temacie przedstawiono sposób włączania routingu atrybutów i opisano różne opcje routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="77b09-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="77b09-112">Aby zapoznać się z kompleksowym samouczkiem korzystającym z routingu atrybutów, zobacz [Tworzenie interfejsu API REST z routingiem atrybutów w interfejsie Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="77b09-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77b09-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="77b09-113">Prerequisites</span></span>

<span data-ttu-id="77b09-114">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="77b09-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="77b09-115">Alternatywnie można zainstalować wymagane pakiety przy użyciu Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="77b09-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="77b09-116">W menu **Narzędzia** w programie Visual Studio wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="77b09-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="77b09-117">Wprowadź następujące polecenie w oknie Konsola Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="77b09-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="77b09-118">Dlaczego atrybut routingu?</span><span class="sxs-lookup"><span data-stu-id="77b09-118">Why Attribute Routing?</span></span>

<span data-ttu-id="77b09-119">Pierwsza wersja interfejsu API sieci Web używa routingu *opartego na Konwencji* .</span><span class="sxs-lookup"><span data-stu-id="77b09-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="77b09-120">W tym typie routingu należy zdefiniować jeden lub więcej szablonów tras, które są zasadniczo sparametryzowanemi ciągami.</span><span class="sxs-lookup"><span data-stu-id="77b09-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="77b09-121">Gdy struktura odbiera żądanie, jest zgodna z identyfikatorem URI względem szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="77b09-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="77b09-122">(Aby uzyskać więcej informacji na temat routingu opartego na Konwencji, zobacz [Routing in Web API ASP.NET](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="77b09-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="77b09-123">Jedną z zalet routingu opartego na Konwencji jest to, że szablony są zdefiniowane w jednym miejscu, a reguły routingu są stosowane spójnie na wszystkich kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="77b09-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="77b09-124">Niestety, routing oparty na Konwencji utrudnia obsługę niektórych wzorców identyfikatorów URI, które są wspólne w interfejsach API RESTful.</span><span class="sxs-lookup"><span data-stu-id="77b09-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="77b09-125">Na przykład zasoby często zawierają zasoby podrzędne: klienci mają zamówienia, filmy mają aktorów, książki mają autorów itd.</span><span class="sxs-lookup"><span data-stu-id="77b09-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="77b09-126">W naturalny sposób można tworzyć identyfikatory URI odzwierciedlające te relacje:</span><span class="sxs-lookup"><span data-stu-id="77b09-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="77b09-127">Ten typ identyfikatora URI jest trudny do utworzenia przy użyciu routingu opartego na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="77b09-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="77b09-128">Chociaż można to zrobić, wyniki nie będą skalowane prawidłowo, jeśli istnieje wiele kontrolerów lub typów zasobów.</span><span class="sxs-lookup"><span data-stu-id="77b09-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="77b09-129">Dzięki routingu atrybutów można zdefiniować trasę dla tego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="77b09-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="77b09-130">Wystarczy dodać atrybut do akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="77b09-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="77b09-131">Oto kilka innych wzorców, które ułatwiają Routing atrybutów.</span><span class="sxs-lookup"><span data-stu-id="77b09-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="77b09-132">**Obsługa wersji interfejsu API**</span><span class="sxs-lookup"><span data-stu-id="77b09-132">**API versioning**</span></span>

<span data-ttu-id="77b09-133">W tym przykładzie "/API/V1/Products" byłby kierowany do innego kontrolera niż "/API/v2/Products".</span><span class="sxs-lookup"><span data-stu-id="77b09-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="77b09-134">**Przeciążone segmenty URI**</span><span class="sxs-lookup"><span data-stu-id="77b09-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="77b09-135">W tym przykładzie "1" jest numerem zamówienia, ale "oczekujące" są mapowane do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="77b09-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="77b09-136">**Wiele typów parametrów**</span><span class="sxs-lookup"><span data-stu-id="77b09-136">**Multiple parameter types**</span></span>

<span data-ttu-id="77b09-137">W tym przykładzie "1" jest numerem zamówienia, ale "2013/06/16" określa datę.</span><span class="sxs-lookup"><span data-stu-id="77b09-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="77b09-138">Włączanie routingu atrybutów</span><span class="sxs-lookup"><span data-stu-id="77b09-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="77b09-139">Aby włączyć routing atrybutów, wywołaj **MapHttpAttributeRoutes** podczas konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="77b09-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="77b09-140">Ta metoda rozszerzenia jest zdefiniowana w klasie **System. Web. http. HttpConfigurationExtensions** .</span><span class="sxs-lookup"><span data-stu-id="77b09-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="77b09-141">Routing atrybutów można łączyć z routingiem [opartym na Konwencji](routing-in-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="77b09-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="77b09-142">Aby zdefiniować trasy oparte na Konwencji, wywołaj metodę **MapHttpRoute** .</span><span class="sxs-lookup"><span data-stu-id="77b09-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="77b09-143">Aby uzyskać więcej informacji na temat konfigurowania interfejsu API sieci Web, zobacz [konfigurowanie ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="77b09-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="77b09-144">Uwaga: Migrowanie z interfejsu API sieci Web 1</span><span class="sxs-lookup"><span data-stu-id="77b09-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="77b09-145">W systemach wcześniejszych niż Web API 2 szablon projektu interfejsu API sieci Web został wygenerowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="77b09-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="77b09-146">Jeśli jest włączona funkcja routingu atrybutu, ten kod zgłosi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="77b09-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="77b09-147">Jeśli uaktualniasz istniejący projekt internetowego interfejsu API w celu używania routingu atrybutów, pamiętaj o zaktualizowaniu tego kodu konfiguracji do poniższego:</span><span class="sxs-lookup"><span data-stu-id="77b09-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="77b09-148">Aby uzyskać więcej informacji, zobacz [Konfigurowanie internetowego interfejsu API z hostingiem ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="77b09-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="77b09-149">Dodawanie atrybutów trasy</span><span class="sxs-lookup"><span data-stu-id="77b09-149">Adding Route Attributes</span></span>

<span data-ttu-id="77b09-150">Oto przykład trasy zdefiniowanej przy użyciu atrybutu:</span><span class="sxs-lookup"><span data-stu-id="77b09-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="77b09-151">Ciąg &quot;Customers/{customerId}/Orders&quot; jest szablonem identyfikatora URI dla trasy.</span><span class="sxs-lookup"><span data-stu-id="77b09-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="77b09-152">Interfejs API sieci Web próbuje dopasować identyfikator URI żądania do szablonu.</span><span class="sxs-lookup"><span data-stu-id="77b09-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="77b09-153">W tym przykładzie "klienci" i "zamówienia" są segmentami literału, a "{customerId}" jest parametrem zmiennej.</span><span class="sxs-lookup"><span data-stu-id="77b09-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="77b09-154">Następujące identyfikatory URI byłyby zgodne z tym szablonem:</span><span class="sxs-lookup"><span data-stu-id="77b09-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="77b09-155">Można ograniczyć Dopasowywanie przy użyciu [ograniczeń](#constraints), opisanych w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="77b09-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="77b09-156">Zwróć uwagę, że parametr &quot;{customerId}&quot; w szablonie trasy jest zgodny z nazwą parametru *customerId* w metodzie.</span><span class="sxs-lookup"><span data-stu-id="77b09-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="77b09-157">Gdy interfejs API sieci Web wywołuje akcję kontrolera, próbuje powiązać parametry trasy.</span><span class="sxs-lookup"><span data-stu-id="77b09-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="77b09-158">Na przykład jeśli identyfikator URI jest `http://example.com/customers/1/orders`, interfejs API sieci Web próbuje powiązać wartość "1" z parametrem *IDKlienta* w akcji.</span><span class="sxs-lookup"><span data-stu-id="77b09-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="77b09-159">Szablon identyfikatora URI może mieć kilka parametrów:</span><span class="sxs-lookup"><span data-stu-id="77b09-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="77b09-160">Wszystkie metody kontrolera, które nie mają atrybutu trasy, używają routingu opartego na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="77b09-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="77b09-161">W ten sposób można połączyć oba typy routingu w tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="77b09-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="77b09-162">Metody HTTP</span><span class="sxs-lookup"><span data-stu-id="77b09-162">HTTP Methods</span></span>

<span data-ttu-id="77b09-163">Interfejs API sieci Web wybiera także akcje oparte na metodzie HTTP żądania (GET, POST itp.).</span><span class="sxs-lookup"><span data-stu-id="77b09-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="77b09-164">Domyślnie interfejs API sieci Web szuka dopasowania bez uwzględniania wielkości liter na początku nazwy metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="77b09-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="77b09-165">Na przykład Metoda kontrolera o nazwie `PutCustomers` dopasowuje żądanie HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="77b09-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="77b09-166">Tę Konwencję można zastąpić, dekorowania nazwy metodę z dowolnym z następujących atrybutów:</span><span class="sxs-lookup"><span data-stu-id="77b09-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="77b09-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="77b09-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="77b09-168">**Narzędzia HttpGet**</span><span class="sxs-lookup"><span data-stu-id="77b09-168">**[HttpGet]**</span></span>
- <span data-ttu-id="77b09-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="77b09-169">**[HttpHead]**</span></span>
- <span data-ttu-id="77b09-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="77b09-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="77b09-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="77b09-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="77b09-172">**HTTPPOST**</span><span class="sxs-lookup"><span data-stu-id="77b09-172">**[HttpPost]**</span></span>
- <span data-ttu-id="77b09-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="77b09-173">**[HttpPut]**</span></span>

<span data-ttu-id="77b09-174">Poniższy przykład mapuje metodę "xmlbook" na żądanie HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="77b09-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="77b09-175">Dla wszystkich innych metod HTTP, w tym niestandardowymi metodami, należy użyć atrybutu **AcceptVerbs** , który pobiera listę metod http.</span><span class="sxs-lookup"><span data-stu-id="77b09-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="77b09-176">Prefiksy tras</span><span class="sxs-lookup"><span data-stu-id="77b09-176">Route Prefixes</span></span>

<span data-ttu-id="77b09-177">Często trasy na kontrolerze są uruchamiane z tym samym prefiksem.</span><span class="sxs-lookup"><span data-stu-id="77b09-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="77b09-178">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="77b09-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="77b09-179">Można ustawić wspólny prefiks dla całego kontrolera przy użyciu atrybutu **[RoutePrefix]** :</span><span class="sxs-lookup"><span data-stu-id="77b09-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="77b09-180">Użyj tyldy (~) w atrybucie metody, aby zastąpić prefiks trasy:</span><span class="sxs-lookup"><span data-stu-id="77b09-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="77b09-181">Prefiks trasy może zawierać parametry:</span><span class="sxs-lookup"><span data-stu-id="77b09-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="77b09-182">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="77b09-182">Route Constraints</span></span>

<span data-ttu-id="77b09-183">Ograniczenia trasy umożliwiają ograniczenie sposobu dopasowywania parametrów w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="77b09-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="77b09-184">Ogólna składnia jest &quot;{Parameter: Constraint}&quot;.</span><span class="sxs-lookup"><span data-stu-id="77b09-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="77b09-185">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="77b09-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="77b09-186">W tym miejscu Pierwsza trasa zostanie wybrana tylko wtedy, gdy identyfikator &quot;&quot; segment identyfikatora URI jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="77b09-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="77b09-187">W przeciwnym razie zostanie wybrana druga trasa.</span><span class="sxs-lookup"><span data-stu-id="77b09-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="77b09-188">Poniższa tabela zawiera listę ograniczeń, które są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="77b09-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="77b09-189">Typu</span><span class="sxs-lookup"><span data-stu-id="77b09-189">Constraint</span></span> | <span data-ttu-id="77b09-190">Opis</span><span class="sxs-lookup"><span data-stu-id="77b09-190">Description</span></span> | <span data-ttu-id="77b09-191">Przykład</span><span class="sxs-lookup"><span data-stu-id="77b09-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="77b09-192">alfa</span><span class="sxs-lookup"><span data-stu-id="77b09-192">alpha</span></span> | <span data-ttu-id="77b09-193">Dopasowuje wielkie lub małe litery alfabetu łacińskiego (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="77b09-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="77b09-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="77b09-194">{x:alpha}</span></span> |
| <span data-ttu-id="77b09-195">bool</span><span class="sxs-lookup"><span data-stu-id="77b09-195">bool</span></span> | <span data-ttu-id="77b09-196">Dopasowuje wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="77b09-196">Matches a Boolean value.</span></span> | <span data-ttu-id="77b09-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="77b09-197">{x:bool}</span></span> |
| <span data-ttu-id="77b09-198">datetime</span><span class="sxs-lookup"><span data-stu-id="77b09-198">datetime</span></span> | <span data-ttu-id="77b09-199">Dopasowuje wartość **daty i godziny** .</span><span class="sxs-lookup"><span data-stu-id="77b09-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="77b09-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="77b09-200">{x:datetime}</span></span> |
| <span data-ttu-id="77b09-201">decimal</span><span class="sxs-lookup"><span data-stu-id="77b09-201">decimal</span></span> | <span data-ttu-id="77b09-202">Dopasowuje wartość dziesiętną.</span><span class="sxs-lookup"><span data-stu-id="77b09-202">Matches a decimal value.</span></span> | <span data-ttu-id="77b09-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="77b09-203">{x:decimal}</span></span> |
| <span data-ttu-id="77b09-204">double</span><span class="sxs-lookup"><span data-stu-id="77b09-204">double</span></span> | <span data-ttu-id="77b09-205">Dopasowuje 64-bitową wartość zmiennoprzecinkową.</span><span class="sxs-lookup"><span data-stu-id="77b09-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="77b09-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="77b09-206">{x:double}</span></span> |
| <span data-ttu-id="77b09-207">float</span><span class="sxs-lookup"><span data-stu-id="77b09-207">float</span></span> | <span data-ttu-id="77b09-208">Dopasowuje 32-bitową wartość zmiennoprzecinkową.</span><span class="sxs-lookup"><span data-stu-id="77b09-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="77b09-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="77b09-209">{x:float}</span></span> |
| <span data-ttu-id="77b09-210">guid</span><span class="sxs-lookup"><span data-stu-id="77b09-210">guid</span></span> | <span data-ttu-id="77b09-211">Dopasowuje wartość identyfikatora GUID.</span><span class="sxs-lookup"><span data-stu-id="77b09-211">Matches a GUID value.</span></span> | <span data-ttu-id="77b09-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="77b09-212">{x:guid}</span></span> |
| <span data-ttu-id="77b09-213">int</span><span class="sxs-lookup"><span data-stu-id="77b09-213">int</span></span> | <span data-ttu-id="77b09-214">Dopasowuje 32-bitową wartość całkowitą.</span><span class="sxs-lookup"><span data-stu-id="77b09-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="77b09-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="77b09-215">{x:int}</span></span> |
| <span data-ttu-id="77b09-216">{1&gt;length&lt;1}</span><span class="sxs-lookup"><span data-stu-id="77b09-216">length</span></span> | <span data-ttu-id="77b09-217">Dopasowuje ciąg o określonej długości lub w określonym zakresie długości.</span><span class="sxs-lookup"><span data-stu-id="77b09-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="77b09-218">{x:length (6)} {x:length (1, 20)}</span><span class="sxs-lookup"><span data-stu-id="77b09-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="77b09-219">long</span><span class="sxs-lookup"><span data-stu-id="77b09-219">long</span></span> | <span data-ttu-id="77b09-220">Dopasowuje 64-bitową wartość całkowitą.</span><span class="sxs-lookup"><span data-stu-id="77b09-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="77b09-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="77b09-221">{x:long}</span></span> |
| <span data-ttu-id="77b09-222">maks.</span><span class="sxs-lookup"><span data-stu-id="77b09-222">max</span></span> | <span data-ttu-id="77b09-223">Dopasowuje liczbę całkowitą z maksymalną wartością.</span><span class="sxs-lookup"><span data-stu-id="77b09-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="77b09-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="77b09-224">{x:max(10)}</span></span> |
| <span data-ttu-id="77b09-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="77b09-225">maxlength</span></span> | <span data-ttu-id="77b09-226">Dopasowuje ciąg z maksymalną długością.</span><span class="sxs-lookup"><span data-stu-id="77b09-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="77b09-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="77b09-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="77b09-228">min.</span><span class="sxs-lookup"><span data-stu-id="77b09-228">min</span></span> | <span data-ttu-id="77b09-229">Dopasowuje liczbę całkowitą z wartością minimalną.</span><span class="sxs-lookup"><span data-stu-id="77b09-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="77b09-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="77b09-230">{x:min(10)}</span></span> |
| <span data-ttu-id="77b09-231">minLength</span><span class="sxs-lookup"><span data-stu-id="77b09-231">minlength</span></span> | <span data-ttu-id="77b09-232">Dopasowuje ciąg o długości minimalnej.</span><span class="sxs-lookup"><span data-stu-id="77b09-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="77b09-233">{x:minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="77b09-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="77b09-234">range</span><span class="sxs-lookup"><span data-stu-id="77b09-234">range</span></span> | <span data-ttu-id="77b09-235">Dopasowuje liczbę całkowitą w zakresie wartości.</span><span class="sxs-lookup"><span data-stu-id="77b09-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="77b09-236">{x:Range (10, 50)}</span><span class="sxs-lookup"><span data-stu-id="77b09-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="77b09-237">regex</span><span class="sxs-lookup"><span data-stu-id="77b09-237">regex</span></span> | <span data-ttu-id="77b09-238">Dopasowuje wyrażenie regularne.</span><span class="sxs-lookup"><span data-stu-id="77b09-238">Matches a regular expression.</span></span> | <span data-ttu-id="77b09-239">{x:Regex (^ \d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="77b09-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="77b09-240">Należy zauważyć, że niektóre ograniczenia, takie jak &quot;minimum&quot;, przyjmują argumenty w nawiasach.</span><span class="sxs-lookup"><span data-stu-id="77b09-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="77b09-241">Do parametru można zastosować wiele ograniczeń rozdzielonych dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="77b09-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="77b09-242">Niestandardowe ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="77b09-242">Custom Route Constraints</span></span>

<span data-ttu-id="77b09-243">Można utworzyć niestandardowe ograniczenia trasy, implementując Interfejs **IHttpRouteConstraint** .</span><span class="sxs-lookup"><span data-stu-id="77b09-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="77b09-244">Na przykład następujące ograniczenie ogranicza parametr do wartości innej niż zero Integer.</span><span class="sxs-lookup"><span data-stu-id="77b09-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="77b09-245">Poniższy kod przedstawia sposób zarejestrowania ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="77b09-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="77b09-246">Teraz można zastosować ograniczenie w ramach tras:</span><span class="sxs-lookup"><span data-stu-id="77b09-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="77b09-247">Możesz również zastąpić całą klasę **DefaultInlineConstraintResolver** przez implementację interfejsu **IInlineConstraintResolver** .</span><span class="sxs-lookup"><span data-stu-id="77b09-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="77b09-248">Wykonanie tej operacji spowoduje zastąpienie wszystkich wbudowanych ograniczeń, chyba że implementacja **IInlineConstraintResolver** w ich ramach nie dodaje.</span><span class="sxs-lookup"><span data-stu-id="77b09-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="77b09-249">Opcjonalne parametry URI i wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="77b09-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="77b09-250">Można określić parametr identyfikatora URI jako opcjonalny, dodając znak zapytania do parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="77b09-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="77b09-251">Jeśli parametr trasy jest opcjonalny, należy zdefiniować wartość domyślną dla parametru metody.</span><span class="sxs-lookup"><span data-stu-id="77b09-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="77b09-252">W tym przykładzie `/api/books/locale/1033` i `/api/books/locale` zwracać tego samego zasobu.</span><span class="sxs-lookup"><span data-stu-id="77b09-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="77b09-253">Alternatywnie możesz określić wartość domyślną w szablonie trasy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="77b09-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="77b09-254">Jest to prawie tak samo jak w poprzednim przykładzie, ale podczas stosowania wartości domyślnej występuje niewielka różnica zachowania.</span><span class="sxs-lookup"><span data-stu-id="77b09-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="77b09-255">W pierwszym przykładzie ("{LCID: int?}") wartość domyślna 1033 jest przypisywana bezpośrednio do parametru metody, dlatego parametr będzie miał dokładną wartość.</span><span class="sxs-lookup"><span data-stu-id="77b09-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="77b09-256">W drugim przykładzie ("{LCID: int = 1033}") wartość domyślna "1033" przechodzi przez proces powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="77b09-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="77b09-257">Model segregatora domyślnego spowoduje przekonwertowanie "1033" na wartość liczbową 1033.</span><span class="sxs-lookup"><span data-stu-id="77b09-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="77b09-258">Można jednak podłączyć spinacz modelu niestandardowego, który może coś się różnić.</span><span class="sxs-lookup"><span data-stu-id="77b09-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="77b09-259">(W większości przypadków, jeśli nie masz niestandardowych powiązań modelu w potoku, te dwa formularze będą równoważne).</span><span class="sxs-lookup"><span data-stu-id="77b09-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="77b09-260">Nazwy tras</span><span class="sxs-lookup"><span data-stu-id="77b09-260">Route Names</span></span>

<span data-ttu-id="77b09-261">W interfejsie Web API Każda trasa ma nazwę.</span><span class="sxs-lookup"><span data-stu-id="77b09-261">In Web API, every route has a name.</span></span> <span data-ttu-id="77b09-262">Nazwy tras są przydatne do generowania linków, dzięki czemu można dołączyć łącze w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="77b09-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="77b09-263">Aby określić nazwę trasy, ustaw właściwość **Nazwa** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="77b09-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="77b09-264">Poniższy przykład pokazuje, jak ustawić nazwę trasy, a także użyć nazwy trasy podczas generowania linku.</span><span class="sxs-lookup"><span data-stu-id="77b09-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="77b09-265">Kolejność tras</span><span class="sxs-lookup"><span data-stu-id="77b09-265">Route Order</span></span>

<span data-ttu-id="77b09-266">Gdy struktura próbuje dopasować identyfikator URI przy użyciu trasy, szacuje trasy w określonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="77b09-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="77b09-267">Aby określić kolejność, ustaw właściwość **Order** dla atrybutu Route.</span><span class="sxs-lookup"><span data-stu-id="77b09-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="77b09-268">Niższe wartości są oceniane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="77b09-268">Lower values are evaluated first.</span></span> <span data-ttu-id="77b09-269">Domyślna wartość zamówienia to zero.</span><span class="sxs-lookup"><span data-stu-id="77b09-269">The default order value is zero.</span></span>

<span data-ttu-id="77b09-270">Poniżej przedstawiono sposób określania sumy porządkowania:</span><span class="sxs-lookup"><span data-stu-id="77b09-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="77b09-271">Porównaj Właściwość **Order** atrybutu Route.</span><span class="sxs-lookup"><span data-stu-id="77b09-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="77b09-272">Sprawdź każdy segment identyfikatora URI w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="77b09-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="77b09-273">Dla każdego segmentu porządkuj w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="77b09-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="77b09-274">Segmenty literału.</span><span class="sxs-lookup"><span data-stu-id="77b09-274">Literal segments.</span></span>
    2. <span data-ttu-id="77b09-275">Parametry trasy z ograniczeniami.</span><span class="sxs-lookup"><span data-stu-id="77b09-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="77b09-276">Parametry trasy bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="77b09-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="77b09-277">Wieloznaczne segmenty parametrów z ograniczeniami.</span><span class="sxs-lookup"><span data-stu-id="77b09-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="77b09-278">Wieloznaczne segmenty parametrów bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="77b09-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="77b09-279">W przypadku równego powiązania trasy są uporządkowane według porównania ciągów porządkowych bez uwzględniania wielkości liter ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="77b09-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="77b09-280">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="77b09-280">Here is an example.</span></span> <span data-ttu-id="77b09-281">Załóżmy, że definiujesz następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="77b09-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="77b09-282">Te trasy są uporządkowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="77b09-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="77b09-283">zamówienia/szczegóły</span><span class="sxs-lookup"><span data-stu-id="77b09-283">orders/details</span></span>
2. <span data-ttu-id="77b09-284">zamówienia/{ID}</span><span class="sxs-lookup"><span data-stu-id="77b09-284">orders/{id}</span></span>
3. <span data-ttu-id="77b09-285">zamówienia/{CustomerName}</span><span class="sxs-lookup"><span data-stu-id="77b09-285">orders/{customerName}</span></span>
4. <span data-ttu-id="77b09-286">Orders/{\*Date}</span><span class="sxs-lookup"><span data-stu-id="77b09-286">orders/{\*date}</span></span>
5. <span data-ttu-id="77b09-287">zamówienia/oczekujące</span><span class="sxs-lookup"><span data-stu-id="77b09-287">orders/pending</span></span>

<span data-ttu-id="77b09-288">Zwróć uwagę, że "Details" jest segmentem literału i pojawia się przed "{ID}", ale "Pending" pojawia się jako Last, ponieważ właściwość **Order** ma wartość 1.</span><span class="sxs-lookup"><span data-stu-id="77b09-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="77b09-289">(W tym przykładzie zakłada się, że nie ma żadnych klientów o nazwie "Details" lub "Pending".</span><span class="sxs-lookup"><span data-stu-id="77b09-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="77b09-290">Ogólnie rzecz biorąc, spróbuj uniknąć niejednoznacznych tras.</span><span class="sxs-lookup"><span data-stu-id="77b09-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="77b09-291">W tym przykładzie lepszym szablonem trasy dla `GetByCustomer` jest "Customers/{CustomerName}")</span><span class="sxs-lookup"><span data-stu-id="77b09-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
