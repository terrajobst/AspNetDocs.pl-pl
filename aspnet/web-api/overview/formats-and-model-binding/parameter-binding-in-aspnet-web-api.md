---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Wiązanie parametrów interfejsie ASP.NET Web API — ASP.NET 4.x
author: MikeWasson
description: W tym artykule opisano sposób internetowy interfejs API wiąże parametrów i jak dostosować proces wiązania w programie ASP.NET 4.x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f121f12ce689a079412bbd5392fde4fea863ff1f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59401975"
---
# <a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="074ed-103">Wiązanie parametrów interfejsie Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="074ed-103">Parameter Binding in ASP.NET Web API</span></span>

<span data-ttu-id="074ed-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="074ed-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="074ed-105">W tym artykule opisano, jak interfejs API sieci Web tworzy powiązania parametrów i w jaki sposób dostosować proces wiązania.</span><span class="sxs-lookup"><span data-stu-id="074ed-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span> <span data-ttu-id="074ed-106">Gdy interfejs API sieci Web wywołuje metodę dla kontrolera, należy ustawić w niej wartości parametrów w procesie zwanym *powiązania*.</span><span class="sxs-lookup"><span data-stu-id="074ed-106">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> 

<span data-ttu-id="074ed-107">Domyślnie by powiązać parametry interfejsu API sieci Web stosowane są następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="074ed-107">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="074ed-108">Jeśli parametr jest typu "prosta", interfejs API sieci Web próbuje pobrać wartość z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="074ed-108">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="074ed-109">Proste typy obejmują .NET [typów pierwotnych](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, i tak dalej), oraz **TimeSpan**, **Daty/godziny**, **Guid**, **dziesiętna**, i **ciąg**, *oraz* wszystkie Typ konwertera typów, który można przekonwertować ciągu.</span><span class="sxs-lookup"><span data-stu-id="074ed-109">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="074ed-110">(Więcej informacji na temat konwerterów typów później.)</span><span class="sxs-lookup"><span data-stu-id="074ed-110">(More about type converters later.)</span></span>
- <span data-ttu-id="074ed-111">Dla typów złożonych, interfejs API sieci Web próbuje odczytać wartości z treści wiadomości, za pomocą [program formatujący typ nośnika](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="074ed-111">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="074ed-112">Na przykład poniżej przedstawiono typowe metody kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="074ed-112">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="074ed-113">*Identyfikator* parametr jest &quot;proste&quot; typ, dzięki czemu próbuje pobrać wartość z identyfikatora URI żądania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="074ed-113">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="074ed-114">*Elementu* parametr jest typem złożonym, dzięki czemu można odczytać wartości z treści żądania interfejsu API sieci Web korzysta z elementu formatującego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="074ed-114">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="074ed-115">Aby uzyskać wartość z identyfikatora URI, internetowy interfejs API wygląda w danych trasy i ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="074ed-115">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="074ed-116">Dane trasy jest wypełniana w przypadku routingu system analizuje identyfikator URI i dopasowuje je do trasy.</span><span class="sxs-lookup"><span data-stu-id="074ed-116">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="074ed-117">Aby uzyskać więcej informacji, zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="074ed-117">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="074ed-118">W dalszej części tego artykułu I pokazano, w jaki sposób dostosować procesie powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="074ed-118">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="074ed-119">W przypadku typów złożonych jednak należy rozważyć użycie programy formatujące typy nośnika, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="074ed-119">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="074ed-120">Klucza zasada HTTP jest, że zasoby są wysyłane w treści wiadomości, za pomocą negocjowania zawartości w celu określania reprezentację zasobu.</span><span class="sxs-lookup"><span data-stu-id="074ed-120">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="074ed-121">Programy formatujące typy nośnika zostały zaprojektowane do dokładnie tego celu.</span><span class="sxs-lookup"><span data-stu-id="074ed-121">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="074ed-122">Przy użyciu [FromUri]</span><span class="sxs-lookup"><span data-stu-id="074ed-122">Using [FromUri]</span></span>

<span data-ttu-id="074ed-123">Aby wymusić interfejsu API sieci Web można odczytać to typ złożony z identyfikatora URI, należy dodać **[FromUri]** atrybutu do parametru.</span><span class="sxs-lookup"><span data-stu-id="074ed-123">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="074ed-124">W poniższym przykładzie zdefiniowano `GeoPoint` typu wraz z metody kontrolera, który pobiera `GeoPoint` z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="074ed-124">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="074ed-125">Klienta można umieścić wartości długości i szerokości geograficznej w ciągu zapytania i interfejsu API sieci Web będzie konstruowania za ich pomocą `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="074ed-125">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="074ed-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="074ed-126">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="074ed-127">Przy użyciu [FromBody]</span><span class="sxs-lookup"><span data-stu-id="074ed-127">Using [FromBody]</span></span>

<span data-ttu-id="074ed-128">Aby wymusić odczytu typu prostego z treści żądania interfejsu API sieci Web, należy dodać **[FromBody]** atrybutu do parametru:</span><span class="sxs-lookup"><span data-stu-id="074ed-128">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="074ed-129">W tym przykładzie interfejs API sieci Web użyje elementu formatującego typu nośnika można odczytać wartości z *nazwa* z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="074ed-129">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="074ed-130">Oto przykładowe żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="074ed-130">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="074ed-131">Jeśli parametr ma [FromBody], interfejs API sieci Web używa nagłówka Content-Type, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="074ed-131">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="074ed-132">W tym przykładzie typem zawartości jest &quot;application/json&quot; i treść żądania jest nieprzetworzonego ciągu JSON (a nie obiekt JSON).</span><span class="sxs-lookup"><span data-stu-id="074ed-132">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="074ed-133">Co najwyżej jeden parametr może odczytywać treść komunikatu.</span><span class="sxs-lookup"><span data-stu-id="074ed-133">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="074ed-134">Dlatego ta nie będzie działać:</span><span class="sxs-lookup"><span data-stu-id="074ed-134">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="074ed-135">Przyczyna, dla tej reguły jest, że treść żądania mogą być przechowywane w niebuforowanym strumienia, który może zostać odczytany tylko raz.</span><span class="sxs-lookup"><span data-stu-id="074ed-135">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="074ed-136">Konwertery typu</span><span class="sxs-lookup"><span data-stu-id="074ed-136">Type Converters</span></span>

<span data-ttu-id="074ed-137">Ułatwia traktować klasę jako typu prostego (dzięki czemu interfejs API sieci Web spróbuje powiązać go z identyfikatora URI) interfejsu API sieci Web, tworząc **TypeConverter** i zapewniając konwersji ciągu.</span><span class="sxs-lookup"><span data-stu-id="074ed-137">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="074ed-138">Poniższy kod przedstawia `GeoPoint` klasa, która reprezentuje punkt geograficzne oraz **TypeConverter** konwersji z ciągów do `GeoPoint` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="074ed-138">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="074ed-139">`GeoPoint` Klasy zostanie nadany **[TypeConverter]** atrybutu, aby określić konwertera typów.</span><span class="sxs-lookup"><span data-stu-id="074ed-139">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="074ed-140">(W tym przykładzie został ZAINSPIROWANE wpis w blogu wstrzymania Mike [sposobu tworzenia powiązania do obiektów niestandardowych w podpisach akcji w MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="074ed-140">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="074ed-141">Teraz interfejs API sieci Web będą traktować `GeoPoint` jako typ prosty, czyli podejmie próbę powiązania `GeoPoint` parametry z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="074ed-141">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="074ed-142">Nie musisz uwzględniać **[FromUri]** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="074ed-142">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="074ed-143">Klient może wywołać metody za pomocą identyfikatora URI w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="074ed-143">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="074ed-144">Integratorów modelu</span><span class="sxs-lookup"><span data-stu-id="074ed-144">Model Binders</span></span>

<span data-ttu-id="074ed-145">Opcja bardziej elastyczne niż konwertera typów polega na utworzeniu niestandardowego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="074ed-145">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="074ed-146">Za pomocą integratora modelu masz dostęp do elementów, takich jak żądania HTTP, opis akcji i wartości w wierszach z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="074ed-146">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="074ed-147">Aby utworzyć obiekt wiążący modelu, należy zaimplementować **interfejs IModelBinder** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="074ed-147">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="074ed-148">Ten interfejs definiuje jedną metodę **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="074ed-148">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="074ed-149">Oto integratora modelu dla `GeoPoint` obiektów.</span><span class="sxs-lookup"><span data-stu-id="074ed-149">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="074ed-150">Integrator modelu pobiera nieprzetworzone wartości wejściowe z *dostawcy wartości*.</span><span class="sxs-lookup"><span data-stu-id="074ed-150">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="074ed-151">W tym projekcie oddziela dwie odrębne funkcje:</span><span class="sxs-lookup"><span data-stu-id="074ed-151">This design separates two distinct functions:</span></span>

- <span data-ttu-id="074ed-152">Dostawca wartości przyjmuje żądania HTTP i wypełnia słownik par klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="074ed-152">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="074ed-153">Integrator modelu użyje tego słownika, aby wypełnić modelu.</span><span class="sxs-lookup"><span data-stu-id="074ed-153">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="074ed-154">Dostawca wartości domyślne w interfejsie API sieci Web pobiera wartości z danych trasy i ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="074ed-154">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="074ed-155">Na przykład, jeśli identyfikator URI jest `http://localhost/api/values/1?location=48,-122`, dostawca wartości tworzy następujące pary klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="074ed-155">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="074ed-156">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="074ed-156">id = &quot;1&quot;</span></span>
- <span data-ttu-id="074ed-157">Lokalizacja = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="074ed-157">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="074ed-158">(Czy jestem zakładając, że szablon trasy domyślne, czyli &quot;interfejsu api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="074ed-158">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="074ed-159">Nazwa parametru do powiązania są przechowywane w **ModelBindingContext.ModelName** właściwości.</span><span class="sxs-lookup"><span data-stu-id="074ed-159">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="074ed-160">Integrator modelu szuka klucza o tej wartości w słowniku.</span><span class="sxs-lookup"><span data-stu-id="074ed-160">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="074ed-161">Jeśli wartość istnieje i mogą być konwertowane na `GeoPoint`, integratora modelu przypisuje wartość powiązanych **ModelBindingContext.Model** właściwości.</span><span class="sxs-lookup"><span data-stu-id="074ed-161">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="074ed-162">Należy zauważyć, że integratora modelu nie jest ograniczona do konwersji typu prostego.</span><span class="sxs-lookup"><span data-stu-id="074ed-162">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="074ed-163">W tym przykładzie integratora modelu najpierw przeszukuje tabelę znanej lokalizacji, a jeśli ono zawiedzie, używa konwersji typu.</span><span class="sxs-lookup"><span data-stu-id="074ed-163">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="074ed-164">**Ustawienie integratora modelu**</span><span class="sxs-lookup"><span data-stu-id="074ed-164">**Setting the Model Binder**</span></span>

<span data-ttu-id="074ed-165">Istnieje kilka sposobów, aby ustawić integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="074ed-165">There are several ways to set a model binder.</span></span> <span data-ttu-id="074ed-166">Po pierwsze, możesz dodać **[ModelBinder]** atrybutu do parametru.</span><span class="sxs-lookup"><span data-stu-id="074ed-166">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="074ed-167">Można również dodać **[ModelBinder]** atrybutu typu.</span><span class="sxs-lookup"><span data-stu-id="074ed-167">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="074ed-168">Interfejs API sieci Web użyje integratora modelu określonego dla wszystkich parametrów tego typu.</span><span class="sxs-lookup"><span data-stu-id="074ed-168">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="074ed-169">Na koniec można dodać dostawcę integratora modelu do **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="074ed-169">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="074ed-170">Dostawca integratora modelu to po prostu klasę fabryki, która tworzy integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="074ed-170">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="074ed-171">Możesz utworzyć dostawcę, wynikające z [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) klasy.</span><span class="sxs-lookup"><span data-stu-id="074ed-171">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="074ed-172">Jednak jeśli Twoja integratora modelu obsługuje jeden typ, łatwiej jest użyć wbudowanego **SimpleModelBinderProvider**, który jest przeznaczony do tego celu.</span><span class="sxs-lookup"><span data-stu-id="074ed-172">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="074ed-173">Poniższy kod pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="074ed-173">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="074ed-174">Za pomocą dostawcy powiązanie modelu, nadal musisz dodać **[ModelBinder]** atrybutu do parametru mówić interfejsu API sieci Web, że należy używać w integratora modelu i nie element formatujący typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="074ed-174">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="074ed-175">Ale teraz nie musisz określić typ integratora modelu w atrybucie:</span><span class="sxs-lookup"><span data-stu-id="074ed-175">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="074ed-176">Dostawców wartości</span><span class="sxs-lookup"><span data-stu-id="074ed-176">Value Providers</span></span>

<span data-ttu-id="074ed-177">Wspomniałem, że obiekt wiążący modelu pobiera wartości z dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="074ed-177">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="074ed-178">Aby zapisać dostawcę wartości niestandardowych, należy zaimplementować **IValueProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="074ed-178">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="074ed-179">Oto przykład, która pobiera wartości z plików cookie żądania:</span><span class="sxs-lookup"><span data-stu-id="074ed-179">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="074ed-180">Należy również utworzyć fabryki dostawców wartości, wynikające z **ValueProviderFactory** klasy.</span><span class="sxs-lookup"><span data-stu-id="074ed-180">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="074ed-181">Fabryki dostawców wartości, aby dodać **HttpConfiguration** w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="074ed-181">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="074ed-182">Interfejs API sieci Web Redaguj wszystkich dostawców wartości, więc kiedy wywołuje obiekt wiążący modelu **ValueProvider.GetValue**, integratora modelu otrzymuje wartość z pierwszym dostawcy wartości, które może wygenerować go.</span><span class="sxs-lookup"><span data-stu-id="074ed-182">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="074ed-183">Alternatywnie, można ustawić fabryki dostawców wartości na poziomie parametru przy użyciu **ValueProvider** atrybutu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="074ed-183">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="074ed-184">Oznacza to, internetowy interfejs API wiązania modelu za pomocą fabryki dostawców określoną wartość, a nie używać żadnych innych dostawców zarejestrowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="074ed-184">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="074ed-185">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="074ed-185">HttpParameterBinding</span></span>

<span data-ttu-id="074ed-186">Integratorów modelu są konkretne wystąpienie mechanizmu bardziej ogólnych.</span><span class="sxs-lookup"><span data-stu-id="074ed-186">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="074ed-187">Jeśli przyjrzymy się **[ModelBinder]** atrybutu, zobaczysz, że pochodzi od abstrakcyjnej **ParameterBindingAttribute** klasy.</span><span class="sxs-lookup"><span data-stu-id="074ed-187">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="074ed-188">Ta klasa definiuje jedną metodę **GetBinding**, co powoduje zwrócenie **HttpParameterBinding** obiektu:</span><span class="sxs-lookup"><span data-stu-id="074ed-188">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="074ed-189">**HttpParameterBinding** jest odpowiedzialny za powiązanie parametru z wartością.</span><span class="sxs-lookup"><span data-stu-id="074ed-189">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="074ed-190">W przypadku właściwości **[ModelBinder]**, zwraca ten atrybut **HttpParameterBinding** implementację, która używa **interfejs IModelBinder** przeprowadzić faktycznego wiązania.</span><span class="sxs-lookup"><span data-stu-id="074ed-190">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="074ed-191">Można też wdrożyć własną **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="074ed-191">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="074ed-192">Załóżmy, że chcesz pobrać elementów etag z `if-match` i `if-none-match` nagłówki w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="074ed-192">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="074ed-193">Rozpoczniemy od zdefiniowania klasy do reprezentowania elementów ETag.</span><span class="sxs-lookup"><span data-stu-id="074ed-193">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="074ed-194">Firma Microsoft będzie również zdefiniować wyliczenie, aby wskazać, czy można pobrać elementu ETag z `if-match` nagłówek lub `if-none-match` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="074ed-194">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="074ed-195">Oto **HttpParameterBinding** , pobiera element ETag z żądanego nagłówka i wiąże ją do parametru typu tagu ETag:</span><span class="sxs-lookup"><span data-stu-id="074ed-195">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="074ed-196">**ExecuteBindingAsync** metoda wykonuje wiązanie.</span><span class="sxs-lookup"><span data-stu-id="074ed-196">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="074ed-197">W ramach tej metody, Dodaj wartość powiązano parametr **ActionArgument** słownika **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="074ed-197">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="074ed-198">Jeśli Twoje **ExecuteBindingAsync** metoda odczytuje treści komunikatu żądania, Zastąp **WillReadBody** właściwości, aby zwracała wartość true.</span><span class="sxs-lookup"><span data-stu-id="074ed-198">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="074ed-199">Treść żądania może być Niebuforowane strumienia, które mogą być odczytane tylko raz, więc interfejsu API sieci Web wymusza reguły co najwyżej jednego powiązanie można odczytać treści komunikatu.</span><span class="sxs-lookup"><span data-stu-id="074ed-199">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="074ed-200">Aby zastosować niestandardowy **HttpParameterBinding**, można zdefiniować atrybut, który pochodzi od klasy **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="074ed-200">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="074ed-201">Aby uzyskać `ETagParameterBinding`, zdefiniujemy dwa atrybuty, jeden dla `if-match` nagłówków i jeden dla `if-none-match` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="074ed-201">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="074ed-202">Abstrakcyjna klasa bazowa zarówno dziedziczyć.</span><span class="sxs-lookup"><span data-stu-id="074ed-202">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="074ed-203">Poniżej przedstawiono metody kontrolera, który używa `[IfNoneMatch]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="074ed-203">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="074ed-204">Oprócz **ParameterBindingAttribute**, jest innym haku dodawania niestandardowego **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="074ed-204">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="074ed-205">Na **HttpConfiguration** obiektu **ParameterBindingRules** właściwości to zbiór funkcjami anonimowymi typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="074ed-205">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anonymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="074ed-206">Na przykład można dodać regułę, która korzysta z żadnych parametrów element ETag dla metody GET `ETagParameterBinding` z `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="074ed-206">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="074ed-207">Funkcja powinna zwrócić `null` parametrów, których powiązanie nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="074ed-207">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="074ed-208">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="074ed-208">IActionValueBinder</span></span>

<span data-ttu-id="074ed-209">Cały proces wiązanie parametru jest kontrolowany przez podłączane usługę **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="074ed-209">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="074ed-210">Domyślna implementacja klasy **IActionValueBinder** wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="074ed-210">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="074ed-211">Wyszukaj **ParameterBindingAttribute** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="074ed-211">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="074ed-212">Obejmuje to **[FromBody]**, **[FromUri]**, i **[ModelBinder]**, lub atrybutów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="074ed-212">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="074ed-213">W przeciwnym razie Szukaj w **HttpConfiguration.ParameterBindingRules** dla funkcji, która zwraca wartość inną niż null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="074ed-213">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="074ed-214">W przeciwnym razie Użyj domyślnych reguł, które mogę opisane wcześniej.</span><span class="sxs-lookup"><span data-stu-id="074ed-214">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="074ed-215">Jeśli typ parametru jest "prosta" lub ma konwertera typów, należy powiązać z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="074ed-215">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="074ed-216">Jest to równoważne umieszczenie **[FromUri]** atrybutu w parametrze.</span><span class="sxs-lookup"><span data-stu-id="074ed-216">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="074ed-217">W przeciwnym razie spróbuj odczytywać parametr treści komunikatu.</span><span class="sxs-lookup"><span data-stu-id="074ed-217">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="074ed-218">Jest to równoważne umieszczenie **[FromBody]** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="074ed-218">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="074ed-219">Jeśli chcesz, możesz zamienić całą **IActionValueBinder** usługi przy użyciu niestandardowych implementacji.</span><span class="sxs-lookup"><span data-stu-id="074ed-219">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="074ed-220">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="074ed-220">Additional Resources</span></span>

[<span data-ttu-id="074ed-221">Przykład powiązania parametru niestandardowego</span><span class="sxs-lookup"><span data-stu-id="074ed-221">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="074ed-222">Mike wstrzymania napisał dobre serię wpisów w blogu o wiązanie parametru interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="074ed-222">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="074ed-223">Jak działa interfejs API sieci Web, wiązanie parametrów</span><span class="sxs-lookup"><span data-stu-id="074ed-223">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="074ed-224">Styl MVC wiązanie parametru dla interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="074ed-224">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="074ed-225">Jak powiązać do obiektów niestandardowych w podpisach akcji w interfejsie API sieci Web i MVC</span><span class="sxs-lookup"><span data-stu-id="074ed-225">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="074ed-226">Jak utworzyć dostawcę wartości niestandardowe w interfejsie API sieci Web</span><span class="sxs-lookup"><span data-stu-id="074ed-226">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="074ed-227">Wiązanie parametru internetowy interfejs API kulisy</span><span class="sxs-lookup"><span data-stu-id="074ed-227">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
