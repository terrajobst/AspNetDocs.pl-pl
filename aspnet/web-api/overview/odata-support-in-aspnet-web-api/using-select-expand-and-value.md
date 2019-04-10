---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Za pomocą $select, $expand and $value w programie ASP.NET Web API 2 OData — ASP.NET 4.x
author: MikeWasson
description: Omówienie i przykładowy kod dla $Rozwiń, $select, a $value opcji na liście OData Web API 2 dla programu ASP.NET 4.x.
ms.author: riande
ms.date: 10/11/2013
ms.custom: seoapril2019
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: 8b5d3e87c679a31f1908aa648219ae5c6b701a1f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400701"
---
# <a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="44ac1-103">Za pomocą $select, $expand and $value w programie ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="44ac1-103">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="44ac1-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="44ac1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="44ac1-105">Omówienie i przykładowy kod dla $Rozwiń, $select, a $value opcji na liście OData Web API 2 dla programu ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="44ac1-105">Overview and code samples for the $expand, $select, and $value options in OData Web API 2 for ASP.NET 4.x.</span></span> <span data-ttu-id="44ac1-106">Te opcje umożliwiają klientowi do kontrolowania reprezentacji, która go ponownie z serwera.</span><span class="sxs-lookup"><span data-stu-id="44ac1-106">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="44ac1-107">**$expand** powoduje, że z powiązanymi obiektami być wbudowane w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="44ac1-107">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="44ac1-108">**$select** wybierze podzbiór właściwości do uwzględnienia w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="44ac1-108">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="44ac1-109">**$value** pobiera nieprzetworzonej wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="44ac1-109">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="44ac1-110">Przykład schematu</span><span class="sxs-lookup"><span data-stu-id="44ac1-110">Example Schema</span></span>

<span data-ttu-id="44ac1-111">W tym artykule użyję usługi OData, który definiuje trzy jednostki: Produkt, dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="44ac1-111">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="44ac1-112">Każdy produkt ma jedną kategorię i jednego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="44ac1-112">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="44ac1-113">Poniżej przedstawiono klasy C#, które definiują modeli jednostki:</span><span class="sxs-lookup"><span data-stu-id="44ac1-113">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="44ac1-114">Należy zauważyć, że `Product` klasy definiuje właściwości nawigacji dla `Supplier` i `Category`.</span><span class="sxs-lookup"><span data-stu-id="44ac1-114">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="44ac1-115">`Category` Klasa definiuje właściwość nawigacji dla produktów w każdej kategorii.</span><span class="sxs-lookup"><span data-stu-id="44ac1-115">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="44ac1-116">Aby utworzyć punkt końcowy OData dla tego schematu, użyj szkieletu programu Visual Studio 2013, zgodnie z opisem w [Tworzenie punktu końcowego OData w interfejsie API sieci Web platformy ASP.NET](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="44ac1-116">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="44ac1-117">Dodaj kontrolery oddzielny produkt, kategoria i dostawcy.</span><span class="sxs-lookup"><span data-stu-id="44ac1-117">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="44ac1-118">Włączanie $Rozwiń i $select</span><span class="sxs-lookup"><span data-stu-id="44ac1-118">Enabling $expand and $select</span></span>

<span data-ttu-id="44ac1-119">W programie Visual Studio 2013 rusztowania Web API OData tworzy kontroler, automatycznie obsługuje $expand i $select.</span><span class="sxs-lookup"><span data-stu-id="44ac1-119">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="44ac1-120">Odwołanie, poniżej przedstawiono wymagania dotyczące obsługi $Rozwiń i $select w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="44ac1-120">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="44ac1-121">Dla kolekcji, kontroler firmy `Get` metoda musi zwracać **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="44ac1-121">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="44ac1-122">W przypadku pojedynczych jednostek zwracają **SingleResult&lt;T&gt;**, gdzie T jest **IQueryable** zawierający zero lub jedną jednostkę.</span><span class="sxs-lookup"><span data-stu-id="44ac1-122">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="44ac1-123">Ponadto dekoracji swoje `Get` metod z **[Queryable]** atrybutu, jak pokazano w poprzednich fragmentach kodu.</span><span class="sxs-lookup"><span data-stu-id="44ac1-123">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="44ac1-124">Alternatywnie wywołać **EnableQuerySupport** na **HttpConfiguration** obiektu podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="44ac1-124">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="44ac1-125">(Aby uzyskać więcej informacji, zobacz [włączenie opcji zapytania OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="44ac1-125">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="44ac1-126">Using $expand</span><span class="sxs-lookup"><span data-stu-id="44ac1-126">Using $expand</span></span>

<span data-ttu-id="44ac1-127">Kiedy wykonujesz zapytanie OData jednostki lub kolekcji, domyślną odpowiedź nie zawiera powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="44ac1-127">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="44ac1-128">Na przykład poniżej przedstawiono odpowiedź domyślną dla zestawu jednostek kategorie:</span><span class="sxs-lookup"><span data-stu-id="44ac1-128">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="44ac1-129">Jak widać, odpowiedź nie obejmuje wszystkie produkty, nawet jeśli jednostka kategoria zawiera łącze nawigacyjne produktów.</span><span class="sxs-lookup"><span data-stu-id="44ac1-129">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="44ac1-130">Jednak klient może używać $Rozwiń, aby uzyskać listę produktów dla każdej kategorii.</span><span class="sxs-lookup"><span data-stu-id="44ac1-130">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="44ac1-131">$Expand opcja znajduje się w ciągu zapytania żądania:</span><span class="sxs-lookup"><span data-stu-id="44ac1-131">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="44ac1-132">Teraz serwer będzie zawierać produktów dla każdej kategorii, wbudowany z kategorii.</span><span class="sxs-lookup"><span data-stu-id="44ac1-132">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="44ac1-133">Oto ładunek odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="44ac1-133">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="44ac1-134">Należy zauważyć, że każdy wpis w tablicy "wartość" zawiera listę produktów.</span><span class="sxs-lookup"><span data-stu-id="44ac1-134">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="44ac1-135">$Rozwiń opcję przyjmuje rozdzielonych przecinkami lista właściwości nawigacyjne do rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="44ac1-135">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="44ac1-136">Następujące żądanie rozwija kategorii i dostawcy dla produktu.</span><span class="sxs-lookup"><span data-stu-id="44ac1-136">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="44ac1-137">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="44ac1-137">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="44ac1-138">Możesz rozwinąć więcej niż jednego poziomu właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="44ac1-138">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="44ac1-139">Poniższy przykład obejmuje wszystkie produkty dla kategorii, a także dostawcy dla każdego produktu.</span><span class="sxs-lookup"><span data-stu-id="44ac1-139">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="44ac1-140">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="44ac1-140">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="44ac1-141">Domyślnie interfejs API sieci Web ogranicza głębokość rozszerzenia maksymalną na 2.</span><span class="sxs-lookup"><span data-stu-id="44ac1-141">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="44ac1-142">Która zapobiega wysyłaniu złożonych żądań, takich jak przez klienta `$expand=Orders/OrderDetails/Product/Supplier/Region`, który może być nieefektywne w celu wykonywania zapytań i tworzenia dużych odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="44ac1-142">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="44ac1-143">Aby zastąpić domyślne, należy ustawić **MaxExpansionDepth** właściwość **[Queryable]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="44ac1-143">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="44ac1-144">Aby uzyskać więcej informacji na temat $ rozwiń węzeł opcji, zobacz [rozwiń System stosowanie opcji zapytania ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) w oficjalnej dokumentacji OData.</span><span class="sxs-lookup"><span data-stu-id="44ac1-144">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="44ac1-145">Za pomocą $select</span><span class="sxs-lookup"><span data-stu-id="44ac1-145">Using $select</span></span>

<span data-ttu-id="44ac1-146">Opcja $select określa podzbiór właściwości do uwzględnienia w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="44ac1-146">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="44ac1-147">Na przykład można pobrać tylko nazwa i cena każdego produktu, należy użyć następującej kwerendy:</span><span class="sxs-lookup"><span data-stu-id="44ac1-147">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="44ac1-148">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="44ac1-148">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="44ac1-149">Można połączyć $select i $expand w jednym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="44ac1-149">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="44ac1-150">Upewnij się, że będą zawierać właściwości rozszerzonej w opcji $select.</span><span class="sxs-lookup"><span data-stu-id="44ac1-150">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="44ac1-151">Na przykład następujące żądanie pobiera nazwę produktu i dostawcy.</span><span class="sxs-lookup"><span data-stu-id="44ac1-151">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="44ac1-152">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="44ac1-152">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="44ac1-153">Możesz również wybrać właściwości w ramach właściwości rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="44ac1-153">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="44ac1-154">Następujące żądanie rozwija produktów i wybiera nazwę kategorii, a także nazwę produktu.</span><span class="sxs-lookup"><span data-stu-id="44ac1-154">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="44ac1-155">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="44ac1-155">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="44ac1-156">Aby uzyskać więcej informacji na temat opcji $select, zobacz [wybierz opcję zapytania systemu ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) w oficjalnej dokumentacji OData.</span><span class="sxs-lookup"><span data-stu-id="44ac1-156">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="44ac1-157">Pobieranie właściwości poszczególne jednostki ($value)</span><span class="sxs-lookup"><span data-stu-id="44ac1-157">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="44ac1-158">Istnieją dwa sposoby dla klienta OData można pobrać wybranej właściwości z jednostką.</span><span class="sxs-lookup"><span data-stu-id="44ac1-158">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="44ac1-159">Klienta można uzyskać wartość w formacie OData lub uzyskaj nieprzetworzonej wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="44ac1-159">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="44ac1-160">Następujące żądanie pobiera właściwości w formacie OData.</span><span class="sxs-lookup"><span data-stu-id="44ac1-160">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="44ac1-161">Poniżej przedstawiono przykładową odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="44ac1-161">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="44ac1-162">Aby uzyskać nieprzetworzonej wartości właściwości, należy dołączyć $value do identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="44ac1-162">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="44ac1-163">Poniżej przedstawiono odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="44ac1-163">Here is the response.</span></span> <span data-ttu-id="44ac1-164">Należy zauważyć, że typ zawartości "text/plain" nie jest kodem JSON.</span><span class="sxs-lookup"><span data-stu-id="44ac1-164">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="44ac1-165">Aby obsługiwać te zapytania w kontrolerze OData, Dodaj metodę o nazwie `GetProperty`, gdzie `Property` jest nazwą właściwości.</span><span class="sxs-lookup"><span data-stu-id="44ac1-165">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="44ac1-166">Na przykład, czy nazwę metody pobierania właściwości Name `GetName`.</span><span class="sxs-lookup"><span data-stu-id="44ac1-166">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="44ac1-167">Metoda powinna zwrócić wartość tej właściwości:</span><span class="sxs-lookup"><span data-stu-id="44ac1-167">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
