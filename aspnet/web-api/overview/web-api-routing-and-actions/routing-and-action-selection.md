---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing i wybór akcji w interfejsie API sieci Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554888"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="1e895-102">Routing i wybór akcji w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1e895-102">Routing and Action Selection in ASP.NET Web API</span></span>

<span data-ttu-id="1e895-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1e895-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1e895-104">W tym artykule opisano sposób, w jaki interfejs API sieci Web ASP.NET kieruje żądanie HTTP do określonej akcji na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="1e895-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="1e895-105">Ogólne omówienie routingu można znaleźć [w temacie Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1e895-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="1e895-106">Ten artykuł zawiera szczegółowe informacje o procesie routingu.</span><span class="sxs-lookup"><span data-stu-id="1e895-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="1e895-107">Jeśli tworzysz projekt interfejsu API sieci Web i okaże się, że niektóre żądania nie są kierowane w oczekiwany sposób, miejmy nadzieję tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="1e895-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="1e895-108">Routing ma trzy główne etapy:</span><span class="sxs-lookup"><span data-stu-id="1e895-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="1e895-109">Dopasowanie identyfikatora URI do szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="1e895-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="1e895-110">Wybieranie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1e895-110">Selecting a controller.</span></span>
3. <span data-ttu-id="1e895-111">Wybieranie akcji.</span><span class="sxs-lookup"><span data-stu-id="1e895-111">Selecting an action.</span></span>

<span data-ttu-id="1e895-112">Niektóre części procesu można zastąpić własnymi zachowaniami niestandardowymi.</span><span class="sxs-lookup"><span data-stu-id="1e895-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="1e895-113">W tym artykule opisano zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="1e895-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="1e895-114">Na końcu zanotujemy miejsca, w których można dostosować zachowanie.</span><span class="sxs-lookup"><span data-stu-id="1e895-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="1e895-115">Szablony tras</span><span class="sxs-lookup"><span data-stu-id="1e895-115">Route Templates</span></span>

<span data-ttu-id="1e895-116">Szablon trasy wygląda podobnie do ścieżki identyfikatora URI, ale może mieć wartości symboli zastępczych, wskazujący na nawiasy klamrowe:</span><span class="sxs-lookup"><span data-stu-id="1e895-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="1e895-117">Podczas tworzenia trasy można podać wartości domyślne dla niektórych lub wszystkich symboli zastępczych:</span><span class="sxs-lookup"><span data-stu-id="1e895-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="1e895-118">Można również określić ograniczenia, które ograniczają sposób, w jaki segment identyfikatora URI może być zgodny z symbolem zastępczym:</span><span class="sxs-lookup"><span data-stu-id="1e895-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="1e895-119">Struktura próbuje dopasować segmenty w ścieżce URI do szablonu.</span><span class="sxs-lookup"><span data-stu-id="1e895-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="1e895-120">Literały w szablonie muszą dokładnie pasować.</span><span class="sxs-lookup"><span data-stu-id="1e895-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="1e895-121">Symbol zastępczy pasuje do dowolnej wartości, o ile nie określono ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="1e895-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="1e895-122">Struktura nie jest zgodna z innymi częściami identyfikatora URI, takimi jak nazwa hosta lub parametry zapytania.</span><span class="sxs-lookup"><span data-stu-id="1e895-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="1e895-123">Struktura wybiera pierwszą trasę w tabeli tras, która jest zgodna z identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="1e895-124">Istnieją dwa specjalne symbole zastępcze: "{Controller}" i "{Action}".</span><span class="sxs-lookup"><span data-stu-id="1e895-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="1e895-125">"{Controller}" zawiera nazwę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1e895-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="1e895-126">"{Action}" zawiera nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="1e895-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="1e895-127">W przypadku interfejsu API sieci Web zwykła Konwencja to pominięcie "{Action}".</span><span class="sxs-lookup"><span data-stu-id="1e895-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="1e895-128">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="1e895-128">Defaults</span></span>

<span data-ttu-id="1e895-129">W przypadku podania wartości domyślnych trasa będzie zgodna z identyfikatorem URI, w którym brakuje tych segmentów.</span><span class="sxs-lookup"><span data-stu-id="1e895-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="1e895-130">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1e895-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="1e895-131">Identyfikatory URI `http://localhost/api/products/all` i `http://localhost/api/products` są zgodne z poprzednią trasą.</span><span class="sxs-lookup"><span data-stu-id="1e895-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="1e895-132">W ostatnim identyfikatorze URI w brakującym segmencie `{category}` jest przypisywana wartość domyślna `all`.</span><span class="sxs-lookup"><span data-stu-id="1e895-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="1e895-133">Słownik tras</span><span class="sxs-lookup"><span data-stu-id="1e895-133">Route Dictionary</span></span>

<span data-ttu-id="1e895-134">Jeśli struktura znajdzie dopasowanie dla identyfikatora URI, tworzy słownik zawierający wartość dla każdego symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="1e895-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="1e895-135">Klucze są nazwami symboli zastępczych, bez uwzględnienia nawiasów klamrowych.</span><span class="sxs-lookup"><span data-stu-id="1e895-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="1e895-136">Wartości są pobierane ze ścieżki identyfikatora URI lub z wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="1e895-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="1e895-137">Słownik jest przechowywany w obiekcie **IHttpRouteData** .</span><span class="sxs-lookup"><span data-stu-id="1e895-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="1e895-138">W ramach tej fazy dopasowania trasy specjalne symbole zastępcze "{Controller}" i "{Action}" są traktowane podobnie jak inne symbole zastępcze.</span><span class="sxs-lookup"><span data-stu-id="1e895-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="1e895-139">Są one po prostu przechowywane w słowniku z innymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="1e895-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="1e895-140">Wartością domyślną może być wartość specjalna **RouteParameter. Optional**.</span><span class="sxs-lookup"><span data-stu-id="1e895-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="1e895-141">Jeśli symbol zastępczy zostanie przypisany do tej wartości, wartość nie zostanie dodana do słownika tras.</span><span class="sxs-lookup"><span data-stu-id="1e895-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="1e895-142">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1e895-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="1e895-143">W przypadku ścieżki URI "interfejsy API/produkty" słownik tras będzie zawierać następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1e895-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="1e895-144">Kontroler: "produkty"</span><span class="sxs-lookup"><span data-stu-id="1e895-144">controller: "products"</span></span>
- <span data-ttu-id="1e895-145">Kategoria: "wszystkie"</span><span class="sxs-lookup"><span data-stu-id="1e895-145">category: "all"</span></span>

<span data-ttu-id="1e895-146">Jednak dla "API/produkty/zabawki/123" słownik tras będzie zawierał następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1e895-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="1e895-147">Kontroler: "produkty"</span><span class="sxs-lookup"><span data-stu-id="1e895-147">controller: "products"</span></span>
- <span data-ttu-id="1e895-148">Kategoria: "zabawki"</span><span class="sxs-lookup"><span data-stu-id="1e895-148">category: "toys"</span></span>
- <span data-ttu-id="1e895-149">ID: "123"</span><span class="sxs-lookup"><span data-stu-id="1e895-149">id: "123"</span></span>

<span data-ttu-id="1e895-150">Wartości domyślne mogą również zawierać wartość, która nie występuje w żadnym miejscu szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="1e895-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="1e895-151">Jeśli trasa jest zgodna, ta wartość jest przechowywana w słowniku.</span><span class="sxs-lookup"><span data-stu-id="1e895-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="1e895-152">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1e895-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="1e895-153">Jeśli ścieżka identyfikatora URI to "API/root/8", słownik będzie zawierać dwie wartości:</span><span class="sxs-lookup"><span data-stu-id="1e895-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="1e895-154">Kontroler: "klienci"</span><span class="sxs-lookup"><span data-stu-id="1e895-154">controller: "customers"</span></span>
- <span data-ttu-id="1e895-155">Identyfikator: "8"</span><span class="sxs-lookup"><span data-stu-id="1e895-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="1e895-156">Wybieranie kontrolera</span><span class="sxs-lookup"><span data-stu-id="1e895-156">Selecting a Controller</span></span>

<span data-ttu-id="1e895-157">Wybór kontrolera jest obsługiwany przez metodę **IHttpControllerSelector. SelectController** .</span><span class="sxs-lookup"><span data-stu-id="1e895-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="1e895-158">Ta metoda przyjmuje wystąpienie **HttpRequestMessage** i zwraca **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="1e895-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="1e895-159">Domyślna implementacja jest dostarczana przez klasę **DefaultHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="1e895-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="1e895-160">Ta klasa używa prostego algorytmu:</span><span class="sxs-lookup"><span data-stu-id="1e895-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="1e895-161">Poszukaj w słowniku tras klucza "Controller".</span><span class="sxs-lookup"><span data-stu-id="1e895-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="1e895-162">Wypełnij wartość tego klucza i dołącz ciąg "Controller", aby uzyskać nazwę typu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1e895-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="1e895-163">Poszukaj kontrolera internetowego interfejsu API o tej nazwie typu.</span><span class="sxs-lookup"><span data-stu-id="1e895-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="1e895-164">Na przykład jeśli słownik tras zawiera parę klucz-wartość "Controller" = "Products", typ kontrolera to "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="1e895-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="1e895-165">Jeśli nie ma pasującego typu lub wiele dopasowań, struktura zwraca błąd do klienta.</span><span class="sxs-lookup"><span data-stu-id="1e895-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="1e895-166">W kroku 3 **DefaultHttpControllerSelector** używa interfejsu **IHttpControllerTypeResolver** , aby uzyskać listę typów kontrolerów interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1e895-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="1e895-167">Domyślna implementacja **IHttpControllerTypeResolver** zwraca wszystkie klasy publiczne, które implementują **IHttpController**, (b) nie są abstrakcyjne i (c) mają nazwę kończącą się na "Controller".</span><span class="sxs-lookup"><span data-stu-id="1e895-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="1e895-168">Wybór akcji</span><span class="sxs-lookup"><span data-stu-id="1e895-168">Action Selection</span></span>

<span data-ttu-id="1e895-169">Po wybraniu kontrolera, struktura wybiera akcję przez wywołanie metody **IHttpActionSelector. SelectAction** .</span><span class="sxs-lookup"><span data-stu-id="1e895-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="1e895-170">Ta metoda pobiera **HttpControllerContext** i zwraca **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="1e895-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="1e895-171">Domyślna implementacja jest dostarczana przez klasę **ApiControllerActionSelector** .</span><span class="sxs-lookup"><span data-stu-id="1e895-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="1e895-172">Aby wybrać akcję, będzie ona wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="1e895-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="1e895-173">Metoda HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="1e895-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="1e895-174">Symbol zastępczy "{Action}" w szablonie trasy, jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="1e895-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="1e895-175">Parametry akcji na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="1e895-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="1e895-176">Przed przystąpieniem do algorytmu wyboru musimy zrozumieć pewne kwestie dotyczące akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1e895-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="1e895-177">**Które metody kontrolera są uznawane za "akcje"?**</span><span class="sxs-lookup"><span data-stu-id="1e895-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="1e895-178">Po wybraniu akcji struktura sprawdza tylko publiczne metody wystąpienia na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="1e895-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="1e895-179">Oprócz tego wyklucza metody ["name Special"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (konstruktory, zdarzenia, przeciążenia operatora itd.) i metody dziedziczone z klasy **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="1e895-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="1e895-180">**Metody HTTP.**</span><span class="sxs-lookup"><span data-stu-id="1e895-180">**HTTP Methods.**</span></span> <span data-ttu-id="1e895-181">Struktura wybiera tylko te akcje, które pasują do metody HTTP żądania, określone w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1e895-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="1e895-182">Można określić metodę HTTP z atrybutem: **AcceptVerbs**, **HttpDelete**, **Narzędzia HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HTTPPOST**lub **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="1e895-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="1e895-183">W przeciwnym razie, jeśli nazwa metody kontrolera rozpoczyna się od "Get", "post", "Put", "Delete", "Start", "Options" lub "patch", wówczas akcja obsługuje tę metodę HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e895-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="1e895-184">Jeśli żaden z powyższych metod obsługuje wpis POST.</span><span class="sxs-lookup"><span data-stu-id="1e895-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="1e895-185">**Powiązania parametrów.**</span><span class="sxs-lookup"><span data-stu-id="1e895-185">**Parameter Bindings.**</span></span> <span data-ttu-id="1e895-186">Powiązanie parametru to sposób, w jaki interfejs API sieci Web tworzy wartość dla parametru.</span><span class="sxs-lookup"><span data-stu-id="1e895-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="1e895-187">Oto domyślna reguła dla powiązania parametrów:</span><span class="sxs-lookup"><span data-stu-id="1e895-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="1e895-188">Typy proste są pobierane z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="1e895-189">Typy złożone są pobierane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="1e895-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="1e895-190">Typy proste obejmują wszystkie [.NET Framework typy pierwotne](https://msdn.microsoft.com/library/system.type.isprimitive), a także **DateTime**, **Decimal**, **GUID**, **String**i **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="1e895-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="1e895-191">Dla każdej akcji, co najwyżej jeden parametr może odczytać treść żądania.</span><span class="sxs-lookup"><span data-stu-id="1e895-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="1e895-192">Istnieje możliwość zastąpienia domyślnych reguł powiązań.</span><span class="sxs-lookup"><span data-stu-id="1e895-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="1e895-193">Zobacz [powiązanie parametrów WebAPI pod okapem](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e895-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>

<span data-ttu-id="1e895-194">W tym tle jest to algorytm wyboru akcji.</span><span class="sxs-lookup"><span data-stu-id="1e895-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="1e895-195">Utwórz listę wszystkich akcji na kontrolerze, które pasują do metody żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e895-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="1e895-196">Jeśli słownik trasy ma wpis "Action", Usuń akcje, których nazwa nie jest zgodna z tą wartością.</span><span class="sxs-lookup"><span data-stu-id="1e895-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="1e895-197">Spróbuj dopasować parametry akcji do identyfikatora URI w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1e895-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="1e895-198">Dla każdej akcji należy uzyskać listę parametrów, które są typu prostego, gdzie powiązanie pobiera parametr z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="1e895-199">Wyklucz parametry opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="1e895-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="1e895-200">Na tej liście spróbuj znaleźć dopasowanie dla każdej nazwy parametru w słowniku trasy lub w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="1e895-201">W dopasowaniach jest rozróżniana wielkość liter i nie zależą od kolejności parametrów.</span><span class="sxs-lookup"><span data-stu-id="1e895-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="1e895-202">Wybierz akcję, w której każdy parametr na liście ma dopasowanie w identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="1e895-203">Jeśli więcej niż jedna akcja spełnia te kryteria, wybierz ją z największą liczbą pasujących parametrów.</span><span class="sxs-lookup"><span data-stu-id="1e895-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="1e895-204">Ignoruj akcje z atrybutem **[Unactions]** .</span><span class="sxs-lookup"><span data-stu-id="1e895-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="1e895-205">#3 kroków jest prawdopodobnie najbardziej mylące.</span><span class="sxs-lookup"><span data-stu-id="1e895-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="1e895-206">Podstawowym pomysłem jest to, że parametr może pobrać jego wartość z identyfikatora URI, z treści żądania lub z niestandardowego powiązania.</span><span class="sxs-lookup"><span data-stu-id="1e895-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="1e895-207">W przypadku parametrów, które pochodzą z identyfikatora URI, chcemy upewnić się, że identyfikator URI rzeczywiście zawiera wartość dla tego parametru, w ścieżce (za pośrednictwem słownika tras) lub w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="1e895-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="1e895-208">Rozważmy na przykład następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1e895-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="1e895-209">Parametr *ID* jest powiązany z identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="1e895-210">W związku z tym ta akcja może być zgodna tylko z identyfikatorem URI zawierającym wartość "ID" w słowniku trasy lub w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="1e895-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="1e895-211">Parametry opcjonalne są wyjątkiem, ponieważ są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="1e895-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="1e895-212">W przypadku parametru opcjonalnego jest to prawidłowe, Jeśli powiązanie nie może pobrać wartości z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="1e895-213">Typy złożone są wyjątkiem z innego powodu.</span><span class="sxs-lookup"><span data-stu-id="1e895-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="1e895-214">Typ złożony można powiązać tylko z identyfikatorem URI za pomocą niestandardowego powiązania.</span><span class="sxs-lookup"><span data-stu-id="1e895-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="1e895-215">Jednak w takim przypadku struktura nie może się z wyprzedzeniem wiedzieć, czy parametr zostałby powiązany z określonym identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="1e895-216">Aby się dowiedzieć, należy wywołać powiązanie.</span><span class="sxs-lookup"><span data-stu-id="1e895-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="1e895-217">Celem algorytmu wyboru jest wybranie akcji z opisu statycznego przed wywołaniem jakichkolwiek powiązań.</span><span class="sxs-lookup"><span data-stu-id="1e895-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="1e895-218">W związku z tym typy złożone są wykluczone z zgodnego algorytmu.</span><span class="sxs-lookup"><span data-stu-id="1e895-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="1e895-219">Po wybraniu akcji wszystkie powiązania parametrów są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="1e895-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="1e895-220">Podsumowanie:</span><span class="sxs-lookup"><span data-stu-id="1e895-220">Summary:</span></span>

- <span data-ttu-id="1e895-221">Akcja musi być zgodna z metodą HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="1e895-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="1e895-222">Nazwa akcji musi być zgodna z wpisem "Action" w słowniku trasy, jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="1e895-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="1e895-223">Dla każdego parametru akcji, jeśli parametr jest pobierany z identyfikatora URI, nazwa parametru musi być znaleziona w słowniku trasy lub w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="1e895-224">(Parametry opcjonalne i parametry o typach złożonych są wykluczone).</span><span class="sxs-lookup"><span data-stu-id="1e895-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="1e895-225">Spróbuj dopasować największą liczbę parametrów.</span><span class="sxs-lookup"><span data-stu-id="1e895-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="1e895-226">Najlepszym dopasowaniem może być Metoda bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="1e895-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="1e895-227">Przykład rozszerzony</span><span class="sxs-lookup"><span data-stu-id="1e895-227">Extended Example</span></span>

<span data-ttu-id="1e895-228">Rozsyłan</span><span class="sxs-lookup"><span data-stu-id="1e895-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="1e895-229">Kontroler:</span><span class="sxs-lookup"><span data-stu-id="1e895-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="1e895-230">Żądanie HTTP:</span><span class="sxs-lookup"><span data-stu-id="1e895-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="1e895-231">Dopasowanie trasy</span><span class="sxs-lookup"><span data-stu-id="1e895-231">Route Matching</span></span>

<span data-ttu-id="1e895-232">Identyfikator URI pasuje do trasy o nazwie "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="1e895-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="1e895-233">Słownik tras zawiera następujące wpisy:</span><span class="sxs-lookup"><span data-stu-id="1e895-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="1e895-234">Kontroler: "produkty"</span><span class="sxs-lookup"><span data-stu-id="1e895-234">controller: "products"</span></span>
- <span data-ttu-id="1e895-235">Identyfikator: "1"</span><span class="sxs-lookup"><span data-stu-id="1e895-235">id: "1"</span></span>

<span data-ttu-id="1e895-236">Słownik tras nie zawiera parametrów ciągu zapytania, "Version" i "Details", ale te informacje nadal będą brane pod uwagę podczas wyboru akcji.</span><span class="sxs-lookup"><span data-stu-id="1e895-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="1e895-237">Wybór kontrolera</span><span class="sxs-lookup"><span data-stu-id="1e895-237">Controller Selection</span></span>

<span data-ttu-id="1e895-238">W przypadku wpisu "Controller" w słowniku trasy typ kontrolera jest `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="1e895-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="1e895-239">Wybór akcji</span><span class="sxs-lookup"><span data-stu-id="1e895-239">Action Selection</span></span>

<span data-ttu-id="1e895-240">Żądanie HTTP jest żądaniem GET.</span><span class="sxs-lookup"><span data-stu-id="1e895-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="1e895-241">Akcje kontrolera obsługiwane przez program GET to `GetAll`, `GetById`i `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="1e895-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="1e895-242">Słownik tras nie zawiera wpisu "Action", więc nie musi on pasować do nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="1e895-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="1e895-243">Następnie spróbujemy dopasować nazwy parametrów dla akcji, szukając tylko akcji GET.</span><span class="sxs-lookup"><span data-stu-id="1e895-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="1e895-244">Akcja</span><span class="sxs-lookup"><span data-stu-id="1e895-244">Action</span></span> | <span data-ttu-id="1e895-245">Parametry do dopasowania</span><span class="sxs-lookup"><span data-stu-id="1e895-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="1e895-246">brak</span><span class="sxs-lookup"><span data-stu-id="1e895-246">none</span></span> |
| `GetById` | <span data-ttu-id="1e895-247">#</span><span class="sxs-lookup"><span data-stu-id="1e895-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="1e895-248">Nazwij</span><span class="sxs-lookup"><span data-stu-id="1e895-248">"name"</span></span> |

<span data-ttu-id="1e895-249">Należy zauważyć, że parametr *version* `GetById` nie jest brany pod uwagę, ponieważ jest to parametr opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="1e895-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="1e895-250">Metoda `GetAll` pasuje do siebie.</span><span class="sxs-lookup"><span data-stu-id="1e895-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="1e895-251">Metoda `GetById` jest również zgodna, ponieważ słownik tras zawiera wartość "ID".</span><span class="sxs-lookup"><span data-stu-id="1e895-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="1e895-252">Metoda `FindProductsByName` nie jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="1e895-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="1e895-253">Metoda `GetById` WINS, ponieważ pasuje do jednego parametru, a nie parametrów dla `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="1e895-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="1e895-254">Metoda jest wywoływana z następującymi wartościami parametrów:</span><span class="sxs-lookup"><span data-stu-id="1e895-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="1e895-255">*Identyfikator* = 1</span><span class="sxs-lookup"><span data-stu-id="1e895-255">*id* = 1</span></span>
- <span data-ttu-id="1e895-256">*wersja* = 1,5</span><span class="sxs-lookup"><span data-stu-id="1e895-256">*version* = 1.5</span></span>

<span data-ttu-id="1e895-257">Zwróć uwagę, że mimo że *wersja* nie została użyta w algorytmie wyboru, wartość parametru pochodzi z ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1e895-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="1e895-258">Punkty rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="1e895-258">Extension Points</span></span>

<span data-ttu-id="1e895-259">Interfejs API sieci Web udostępnia punkty rozszerzenia dla niektórych części procesu routingu.</span><span class="sxs-lookup"><span data-stu-id="1e895-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="1e895-260">Interfejs</span><span class="sxs-lookup"><span data-stu-id="1e895-260">Interface</span></span> | <span data-ttu-id="1e895-261">Opis</span><span class="sxs-lookup"><span data-stu-id="1e895-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1e895-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="1e895-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="1e895-263">Wybiera kontroler.</span><span class="sxs-lookup"><span data-stu-id="1e895-263">Selects the controller.</span></span> |
| <span data-ttu-id="1e895-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="1e895-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="1e895-265">Pobiera listę typów kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="1e895-265">Gets the list of controller types.</span></span> <span data-ttu-id="1e895-266">**DefaultHttpControllerSelector** wybiera typ kontrolera z tej listy.</span><span class="sxs-lookup"><span data-stu-id="1e895-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="1e895-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="1e895-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="1e895-268">Pobiera listę zestawów projektu.</span><span class="sxs-lookup"><span data-stu-id="1e895-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="1e895-269">Interfejs **IHttpControllerTypeResolver** używa tej listy do znajdowania typów kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="1e895-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="1e895-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="1e895-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="1e895-271">Tworzy nowe wystąpienia kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1e895-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="1e895-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="1e895-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="1e895-273">Wybiera akcję.</span><span class="sxs-lookup"><span data-stu-id="1e895-273">Selects the action.</span></span> |
| <span data-ttu-id="1e895-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="1e895-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="1e895-275">Wywołuje akcję.</span><span class="sxs-lookup"><span data-stu-id="1e895-275">Invokes the action.</span></span> |

<span data-ttu-id="1e895-276">Aby zapewnić własną implementację dla dowolnego z tych interfejsów, Użyj kolekcji **Services** w obiekcie **HttpConfiguration** :</span><span class="sxs-lookup"><span data-stu-id="1e895-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
