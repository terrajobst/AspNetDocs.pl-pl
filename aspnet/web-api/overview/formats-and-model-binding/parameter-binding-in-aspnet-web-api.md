---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Powiązanie parametrów w ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Opisuje sposób, w jaki interfejs API sieci Web wiąże parametry i jak dostosować proces powiązania w ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 464cb9b45dc0b62c4da38b7cf612934808854d32
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074907"
---
# <a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="09534-103">Powiązanie parametrów w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="09534-103">Parameter Binding in ASP.NET Web API</span></span>

<span data-ttu-id="09534-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="09534-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="09534-105">W tym artykule opisano, jak interfejs API sieci Web wiąże parametry i jak można dostosować proces powiązania.</span><span class="sxs-lookup"><span data-stu-id="09534-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span> <span data-ttu-id="09534-106">Gdy interfejs API sieci Web wywołuje metodę na kontrolerze, musi ustawić wartości parametrów, proces o nazwie *Binding*.</span><span class="sxs-lookup"><span data-stu-id="09534-106">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span>

<span data-ttu-id="09534-107">Domyślnie interfejs API sieci Web używa następujących reguł do wiązania parametrów:</span><span class="sxs-lookup"><span data-stu-id="09534-107">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="09534-108">Jeśli parametr jest typu prostego, interfejs API sieci Web próbuje uzyskać wartość z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="09534-108">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="09534-109">Typy proste obejmują [typy pierwotne](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) platformy .NET **(int**, **bool**, **Double**itd.), a także przedziały **czasu,** **DateTime**, **GUID**, **Decimal**i **String**oraz dowolny typ z konwerterem *typów, który* można przekonwertować na ciąg.</span><span class="sxs-lookup"><span data-stu-id="09534-109">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="09534-110">(Więcej informacji na temat konwerterów typów w dalszej części).</span><span class="sxs-lookup"><span data-stu-id="09534-110">(More about type converters later.)</span></span>
- <span data-ttu-id="09534-111">W przypadku typów złożonych interfejs API sieci Web próbuje odczytać wartość z treści wiadomości przy użyciu programu [formatującego typu nośnika](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="09534-111">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="09534-112">Oto przykład typowej metody kontrolera API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="09534-112">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="09534-113">Parametr *ID* jest typu &quot;prostego&quot;, dzięki czemu interfejs API sieci Web próbuje uzyskać wartość z identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="09534-113">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="09534-114">Parametr *Item* jest typem złożonym, więc interfejs API sieci Web używa programu formatującego typu nośnika do odczytywania wartości z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="09534-114">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="09534-115">Aby uzyskać wartość z identyfikatora URI, interfejs API sieci Web przegląda dane trasy i ciąg zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="09534-115">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="09534-116">Dane trasy są wypełniane, gdy system routingu analizuje identyfikator URI i dopasowuje go do trasy.</span><span class="sxs-lookup"><span data-stu-id="09534-116">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="09534-117">Aby uzyskać więcej informacji, zobacz [Routing i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="09534-117">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="09534-118">W pozostałej części artykułu pokażę, jak można dostosować proces powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="09534-118">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="09534-119">W przypadku typów złożonych należy jednak wziąć pod uwagę używanie programu formatującego typu nośnika, gdy jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="09534-119">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="09534-120">Kluczową zasadą protokołu HTTP jest to, że zasoby są wysyłane w treści wiadomości, przy użyciu negocjacji zawartości, aby określić reprezentację zasobu.</span><span class="sxs-lookup"><span data-stu-id="09534-120">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="09534-121">Elementy formatujące typy multimedialne zostały zaprojektowane do tego celu dokładnie.</span><span class="sxs-lookup"><span data-stu-id="09534-121">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="09534-122">Korzystanie z [FromUri]</span><span class="sxs-lookup"><span data-stu-id="09534-122">Using [FromUri]</span></span>

<span data-ttu-id="09534-123">Aby wymusić odczytywanie przez internetowy interfejs API typu złożonego z identyfikatora URI, Dodaj atrybut **[FromUri]** do parametru.</span><span class="sxs-lookup"><span data-stu-id="09534-123">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="09534-124">W poniższym przykładzie zdefiniowano typ `GeoPoint` wraz z metodą kontrolera, która pobiera `GeoPoint` z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="09534-124">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="09534-125">Klient może umieścić wartości szerokości i długości geograficznej w ciągu zapytania oraz w interfejsie API sieci Web, aby utworzyć `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="09534-125">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="09534-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="09534-126">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="09534-127">Korzystanie z [FromBody]</span><span class="sxs-lookup"><span data-stu-id="09534-127">Using [FromBody]</span></span>

<span data-ttu-id="09534-128">Aby wymusić odczytanie typu prostego z treści żądania przez internetowy interfejs API, Dodaj atrybut **[FromBody]** do parametru:</span><span class="sxs-lookup"><span data-stu-id="09534-128">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="09534-129">W tym przykładzie interfejs API sieci Web użyje programu formatującego typ nośnika, aby odczytać wartość *nazwy* z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="09534-129">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="09534-130">Oto przykładowe żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="09534-130">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="09534-131">Gdy parametr ma [FromBody], interfejs API sieci Web używa nagłówka Content-Type do wybierania elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="09534-131">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="09534-132">W tym przykładzie typ zawartości to &quot;Application/JSON&quot; i treść żądania jest nieprzetworzonym ciągiem JSON (nie obiektem JSON).</span><span class="sxs-lookup"><span data-stu-id="09534-132">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="09534-133">Co najwyżej jeden parametr może zostać odczytany z treści komunikatu.</span><span class="sxs-lookup"><span data-stu-id="09534-133">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="09534-134">W związku z tym nie będzie to zadziałało:</span><span class="sxs-lookup"><span data-stu-id="09534-134">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="09534-135">Przyczyną tej reguły jest fakt, że treść żądania może być przechowywana w niebuforowanym strumieniu, który można odczytać tylko raz.</span><span class="sxs-lookup"><span data-stu-id="09534-135">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="09534-136">Konwertery typów</span><span class="sxs-lookup"><span data-stu-id="09534-136">Type Converters</span></span>

<span data-ttu-id="09534-137">Można sprawić, aby interfejs API sieci Web traktował klasę jako typ prosty (dzięki czemu interfejs API sieci Web spróbuje powiązać ją z identyfikatorem URI), tworząc obiekt **TypeConverter** i dostarczając konwersję ciągów.</span><span class="sxs-lookup"><span data-stu-id="09534-137">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="09534-138">Poniższy kod przedstawia klasę `GeoPoint`, która reprezentuje punkt geograficzny, oraz obiekt **TypeConverter** , który konwertuje ciągi na wystąpienia `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="09534-138">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="09534-139">Klasa `GeoPoint` ma atrybut **[TypeConverter]** służący do określania konwertera typów.</span><span class="sxs-lookup"><span data-stu-id="09534-139">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="09534-140">(Ten przykład został inspirowany wpisem na blogu Jan, [jak powiązać z obiektami niestandardowymi w sygnaturach akcji w MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)).</span><span class="sxs-lookup"><span data-stu-id="09534-140">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="09534-141">Teraz interfejs API sieci Web będzie traktować `GeoPoint` jako typ prosty, co oznacza, że podejmie próbę powiązania `GeoPoint` parametrów z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="09534-141">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="09534-142">Nie trzeba dołączać **[FromUri]** do parametru.</span><span class="sxs-lookup"><span data-stu-id="09534-142">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="09534-143">Klient może wywołać metodę z identyfikatorem URI podobnym do tego:</span><span class="sxs-lookup"><span data-stu-id="09534-143">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="09534-144">Powiązania modelu</span><span class="sxs-lookup"><span data-stu-id="09534-144">Model Binders</span></span>

<span data-ttu-id="09534-145">Bardziej elastyczną opcją niż konwerter typu jest utworzenie spinacza modelu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="09534-145">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="09534-146">Dzięki modelowi spinacza masz dostęp do elementów, takich jak żądanie HTTP, Opis akcji i wartości pierwotnych z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="09534-146">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="09534-147">Aby utworzyć spinacz modelu, zaimplementuj Interfejs **IModelBinder** .</span><span class="sxs-lookup"><span data-stu-id="09534-147">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="09534-148">Ten interfejs definiuje pojedynczą metodę, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="09534-148">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="09534-149">Oto spinacz modelu dla `GeoPoint` obiektów.</span><span class="sxs-lookup"><span data-stu-id="09534-149">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="09534-150">Spinacz modelu pobiera pierwotne wartości wejściowe od *dostawcy wartości*.</span><span class="sxs-lookup"><span data-stu-id="09534-150">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="09534-151">Ten projekt oddziela dwie odrębne funkcje:</span><span class="sxs-lookup"><span data-stu-id="09534-151">This design separates two distinct functions:</span></span>

- <span data-ttu-id="09534-152">Dostawca wartości pobiera żądanie HTTP i wypełnia słownik par klucz-wartość.</span><span class="sxs-lookup"><span data-stu-id="09534-152">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="09534-153">Model spinacza używa tego słownika do wypełnienia modelu.</span><span class="sxs-lookup"><span data-stu-id="09534-153">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="09534-154">Dostawca wartości domyślnej w interfejsie Web API pobiera wartości z danych trasy i ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="09534-154">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="09534-155">Na przykład jeśli identyfikator URI jest `http://localhost/api/values/1?location=48,-122`, dostawca wartości tworzy następujące pary klucz-wartość:</span><span class="sxs-lookup"><span data-stu-id="09534-155">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="09534-156">Identyfikator = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="09534-156">id = &quot;1&quot;</span></span>
- <span data-ttu-id="09534-157">Lokalizacja = &quot;48 122&quot;</span><span class="sxs-lookup"><span data-stu-id="09534-157">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="09534-158">(Przy założeniu domyślnego szablonu trasy jest &quot;API/{Controller}/{ID}&quot;).</span><span class="sxs-lookup"><span data-stu-id="09534-158">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="09534-159">Nazwa parametru do powiązania jest przechowywana we właściwości **ModelBindingContext. ModelName** .</span><span class="sxs-lookup"><span data-stu-id="09534-159">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="09534-160">Spinacz modelu szuka klucza z tą wartością w słowniku.</span><span class="sxs-lookup"><span data-stu-id="09534-160">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="09534-161">Jeśli wartość istnieje i można ją przekonwertować na `GeoPoint`, spinacz modelu przypisze wartość powiązaną do właściwości **ModelBindingContext. model** .</span><span class="sxs-lookup"><span data-stu-id="09534-161">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="09534-162">Należy zauważyć, że spinacz modelu nie jest ograniczony do konwersji typu prostego.</span><span class="sxs-lookup"><span data-stu-id="09534-162">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="09534-163">W tym przykładzie spinacz modelu najpierw szuka tabeli znanych lokalizacji, a jeśli to się nie powiedzie, używa konwersji typu.</span><span class="sxs-lookup"><span data-stu-id="09534-163">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="09534-164">**Ustawianie spinacza modelu**</span><span class="sxs-lookup"><span data-stu-id="09534-164">**Setting the Model Binder**</span></span>

<span data-ttu-id="09534-165">Istnieje kilka sposobów na ustawienie spinacza modelu.</span><span class="sxs-lookup"><span data-stu-id="09534-165">There are several ways to set a model binder.</span></span> <span data-ttu-id="09534-166">Najpierw można dodać atrybut **[ModelBinder]** do parametru.</span><span class="sxs-lookup"><span data-stu-id="09534-166">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="09534-167">Można również dodać atrybut **[ModelBinder]** do typu.</span><span class="sxs-lookup"><span data-stu-id="09534-167">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="09534-168">Interfejs API sieci Web będzie używać określonego spinacza modelu dla wszystkich parametrów tego typu.</span><span class="sxs-lookup"><span data-stu-id="09534-168">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="09534-169">Na koniec można dodać dostawcę modelu spinacza do **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="09534-169">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="09534-170">Dostawca modelu segregatorów jest po prostu klasą fabryki, która tworzy spinacz modelu.</span><span class="sxs-lookup"><span data-stu-id="09534-170">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="09534-171">Dostawcę można utworzyć, wywodząc się z klasy [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) .</span><span class="sxs-lookup"><span data-stu-id="09534-171">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="09534-172">Jeśli jednak spinacz modelu obsługuje pojedynczy typ, łatwiej jest użyć wbudowanej **SimpleModelBinderProvider**, która jest przeznaczona do tego celu.</span><span class="sxs-lookup"><span data-stu-id="09534-172">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="09534-173">Poniższy kod pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="09534-173">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="09534-174">W przypadku dostawcy powiązań modelu nadal trzeba dodać atrybut **[ModelBinder]** do parametru, aby poinformować internetowy interfejs API, że powinien on używać spinacza modelu, a nie do programu formatującego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="09534-174">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="09534-175">Ale teraz nie musisz określać typu spinacza modelu w atrybucie:</span><span class="sxs-lookup"><span data-stu-id="09534-175">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="09534-176">Dostawcy wartości</span><span class="sxs-lookup"><span data-stu-id="09534-176">Value Providers</span></span>

<span data-ttu-id="09534-177">Określono, że spinacz modelu pobiera wartości z dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="09534-177">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="09534-178">Aby napisać niestandardowego dostawcę wartości, zaimplementuj Interfejs **IValueProvider** .</span><span class="sxs-lookup"><span data-stu-id="09534-178">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="09534-179">Oto przykład, który pobiera wartości z plików cookie w żądaniu:</span><span class="sxs-lookup"><span data-stu-id="09534-179">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="09534-180">Należy również utworzyć fabrykę dostawców wartości, wyprowadzając ją z klasy **ValueProviderFactory** .</span><span class="sxs-lookup"><span data-stu-id="09534-180">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="09534-181">Dodaj fabrykę dostawcy wartości do **HttpConfiguration** w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="09534-181">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="09534-182">Interfejs API sieci Web składa wszystkie dostawców wartości, więc gdy spinacz modelu wywołuje **ValueProvider. GetValue**, spinacz modelu otrzymuje wartość od pierwszego dostawcy wartości, który może go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="09534-182">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="09534-183">Alternatywnie można ustawić fabrykę dostawcy wartości na poziomie parametru przy użyciu atrybutu **ValueProvider** w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="09534-183">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="09534-184">Oznacza to, że interfejs API sieci Web używa powiązania modelu z określoną fabryką dostawcy wartości, a nie do korzystania z innych zarejestrowanych dostawców wartości.</span><span class="sxs-lookup"><span data-stu-id="09534-184">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="09534-185">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="09534-185">HttpParameterBinding</span></span>

<span data-ttu-id="09534-186">Powiązania modelu są konkretnym wystąpieniem bardziej ogólnego mechanizmu.</span><span class="sxs-lookup"><span data-stu-id="09534-186">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="09534-187">Jeśli szukasz atrybutu **[ModelBinder]** , zobaczysz, że pochodzi on z klasy abstrakcyjnej **ParameterBindingAttribute** .</span><span class="sxs-lookup"><span data-stu-id="09534-187">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="09534-188">Ta klasa definiuje pojedynczą metodę, **GetBinding**, która zwraca obiekt **HttpParameterBinding** :</span><span class="sxs-lookup"><span data-stu-id="09534-188">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="09534-189">**HttpParameterBinding** jest odpowiedzialny za powiązanie parametru z wartością.</span><span class="sxs-lookup"><span data-stu-id="09534-189">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="09534-190">W przypadku **[ModelBinder]** atrybut zwraca implementację **HttpParameterBinding** , która używa **IModelBinder** do przeprowadzenia rzeczywistego powiązania.</span><span class="sxs-lookup"><span data-stu-id="09534-190">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="09534-191">Możesz również zaimplementować własne **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="09534-191">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="09534-192">Załóżmy na przykład, że chcesz uzyskać elementy ETag z nagłówków `if-match` i `if-none-match` w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="09534-192">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="09534-193">Zaczniemy od zdefiniowania klasy do reprezentowania elementów ETag.</span><span class="sxs-lookup"><span data-stu-id="09534-193">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="09534-194">Zdefiniujemy również Wyliczenie, aby wskazać, czy ma zostać pobrany element ETag z nagłówka `if-match`, czy z nagłówka `if-none-match`.</span><span class="sxs-lookup"><span data-stu-id="09534-194">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="09534-195">Poniżej znajduje się **HttpParameterBinding** , który pobiera element ETag z żądanego nagłówka i wiąże go z parametrem typu ETag:</span><span class="sxs-lookup"><span data-stu-id="09534-195">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="09534-196">Metoda **ExecuteBindingAsync** wykonuje powiązanie.</span><span class="sxs-lookup"><span data-stu-id="09534-196">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="09534-197">W ramach tej metody Dodaj powiązaną wartość parametru do słownika **ActionArgument** w **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="09534-197">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="09534-198">Jeśli metoda **ExecuteBindingAsync** odczytuje treść komunikatu żądania, Przesłoń Właściwość **WillReadBody** , aby zwracała wartość true.</span><span class="sxs-lookup"><span data-stu-id="09534-198">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="09534-199">Treść żądania może być niebufornym strumieniem, który można tylko raz odczytać, dzięki czemu interfejs API sieci Web wymusza zasadę, którą może odczytać treść komunikatu z najwyżej jednego powiązania.</span><span class="sxs-lookup"><span data-stu-id="09534-199">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>

<span data-ttu-id="09534-200">Aby zastosować niestandardowe **HttpParameterBinding**, można zdefiniować atrybut pochodzący z **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="09534-200">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="09534-201">W przypadku `ETagParameterBinding`zdefiniujemy dwa atrybuty, jeden dla `if-match` nagłówki i jeden dla nagłówków `if-none-match`.</span><span class="sxs-lookup"><span data-stu-id="09534-201">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="09534-202">Oba pochodzą z abstrakcyjnej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="09534-202">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="09534-203">Oto Metoda kontrolera, która używa atrybutu `[IfNoneMatch]`.</span><span class="sxs-lookup"><span data-stu-id="09534-203">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="09534-204">Poza **ParameterBindingAttribute**istnieje inny element Hook służący do dodawania niestandardowego **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="09534-204">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="09534-205">W obiekcie **HttpConfiguration** Właściwość **ParameterBindingRules** jest kolekcją funkcji anonimowych typu (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="09534-205">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anonymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="09534-206">Można na przykład dodać regułę, którą każdy parametr ETag metody GET używa `ETagParameterBinding` z `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="09534-206">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="09534-207">Funkcja powinna zwracać `null` dla parametrów, w których powiązanie nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="09534-207">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="09534-208">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="09534-208">IActionValueBinder</span></span>

<span data-ttu-id="09534-209">Cały proces wiązania parametrów jest kontrolowany przez podłączane usługi **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="09534-209">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="09534-210">Domyślna implementacja **IActionValueBinder** wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="09534-210">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="09534-211">Poszukaj **ParameterBindingAttribute** na parametrze.</span><span class="sxs-lookup"><span data-stu-id="09534-211">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="09534-212">Obejmuje to **[FromBody]** , **[FromUri]** i **[ModelBinder]** lub atrybuty niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="09534-212">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="09534-213">W przeciwnym razie poszukaj w **HttpConfiguration. ParameterBindingRules** funkcji, która zwraca wartość inną niż null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="09534-213">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="09534-214">W przeciwnym razie użyj reguł domyślnych, które zostały opisane wcześniej.</span><span class="sxs-lookup"><span data-stu-id="09534-214">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="09534-215">Jeśli typ parametru to "Simple" lub ma konwerter typu, powiąż z identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="09534-215">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="09534-216">Jest to równoznaczne z umieszczeniem atrybutu **[FromUri]** na parametrze.</span><span class="sxs-lookup"><span data-stu-id="09534-216">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="09534-217">W przeciwnym razie spróbuj odczytać parametr z treści komunikatu.</span><span class="sxs-lookup"><span data-stu-id="09534-217">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="09534-218">Jest to odpowiednik umieszczania **[FromBody]** na parametrze.</span><span class="sxs-lookup"><span data-stu-id="09534-218">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="09534-219">Jeśli chcesz, możesz zastąpić całą usługę **IActionValueBinder** z implementacją niestandardową.</span><span class="sxs-lookup"><span data-stu-id="09534-219">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09534-220">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="09534-220">Additional Resources</span></span>

[<span data-ttu-id="09534-221">Przykład powiązania parametru niestandardowego</span><span class="sxs-lookup"><span data-stu-id="09534-221">Custom Parameter Binding Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/CustomParameterBinding)

<span data-ttu-id="09534-222">Jan zapisał dobrą serię wpisów w blogu na temat powiązania parametrów interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="09534-222">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="09534-223">Jak interfejs API sieci Web wykonuje powiązanie parametrów</span><span class="sxs-lookup"><span data-stu-id="09534-223">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="09534-224">Powiązanie parametrów stylu MVC dla internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="09534-224">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="09534-225">Jak utworzyć powiązanie z obiektami niestandardowymi w sygnaturach akcji w interfejsie API MVC/Web</span><span class="sxs-lookup"><span data-stu-id="09534-225">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="09534-226">Jak utworzyć niestandardowego dostawcę wartości w interfejsie API sieci Web</span><span class="sxs-lookup"><span data-stu-id="09534-226">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="09534-227">Powiązanie parametrów internetowego interfejsu API pod okapem</span><span class="sxs-lookup"><span data-stu-id="09534-227">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
