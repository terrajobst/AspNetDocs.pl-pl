---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relacje jednostek w protokole OData v4 przy użyciu ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: 'Większość zestawów danych definiuje relacje między jednostkami: klienci mają zamówienia; książki mają autorów; produkty mają dostawców. Korzystając z protokołu OData, klienci mogą przechodzić...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598694"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="8be08-104">Relacje jednostek w protokole OData v4 przy użyciu ASP.NET Web API 2,2</span><span class="sxs-lookup"><span data-stu-id="8be08-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="8be08-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8be08-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8be08-106">Większość zestawów danych definiuje relacje między jednostkami: klienci mają zamówienia; książki mają autorów; produkty mają dostawców.</span><span class="sxs-lookup"><span data-stu-id="8be08-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="8be08-107">Korzystając z protokołu OData, klienci mogą nawigować po relacjach jednostek.</span><span class="sxs-lookup"><span data-stu-id="8be08-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="8be08-108">W danym produkcie można znaleźć dostawcę.</span><span class="sxs-lookup"><span data-stu-id="8be08-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="8be08-109">Można również tworzyć i usuwać relacje.</span><span class="sxs-lookup"><span data-stu-id="8be08-109">You can also create or remove relationships.</span></span> <span data-ttu-id="8be08-110">Na przykład można ustawić dostawcę dla produktu.</span><span class="sxs-lookup"><span data-stu-id="8be08-110">For example, you can set the supplier for a product.</span></span>
>
> <span data-ttu-id="8be08-111">W tym samouczku pokazano, jak obsługiwać te operacje w protokole OData v4 przy użyciu interfejsu API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8be08-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="8be08-112">Samouczek jest oparty na samouczku [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8be08-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8be08-113">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="8be08-113">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="8be08-114">Internetowy interfejs API 2,1</span><span class="sxs-lookup"><span data-stu-id="8be08-114">Web API 2.1</span></span>
> - <span data-ttu-id="8be08-115">OData 4</span><span class="sxs-lookup"><span data-stu-id="8be08-115">OData v4</span></span>
> - <span data-ttu-id="8be08-116">Visual Studio 2013 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="8be08-116">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="8be08-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8be08-117">Entity Framework 6</span></span>
> - <span data-ttu-id="8be08-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8be08-118">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="8be08-119">Wersje samouczka</span><span class="sxs-lookup"><span data-stu-id="8be08-119">Tutorial versions</span></span>
>
> <span data-ttu-id="8be08-120">W przypadku protokołu OData w wersji 3 zobacz temat [Obsługa relacji jednostek w protokole OData V3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="8be08-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="8be08-121">Dodawanie jednostki dostawcy</span><span class="sxs-lookup"><span data-stu-id="8be08-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="8be08-122">Samouczek jest oparty na samouczku [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8be08-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="8be08-123">Najpierw potrzebujemy powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="8be08-123">First, we need a related entity.</span></span> <span data-ttu-id="8be08-124">Dodaj klasę o nazwie `Supplier` w folderze modele.</span><span class="sxs-lookup"><span data-stu-id="8be08-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="8be08-125">Dodaj właściwość nawigacji do klasy `Product`:</span><span class="sxs-lookup"><span data-stu-id="8be08-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="8be08-126">Dodaj nowy **nieogólnymi** do klasy `ProductsContext`, tak aby Entity Framework obejmował tabelę dostawców w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8be08-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="8be08-127">W programie WebApiConfig.cs Dodaj &quot;dostawców&quot; do modelu danych jednostki:</span><span class="sxs-lookup"><span data-stu-id="8be08-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="8be08-128">Dodawanie kontrolera dostawcy</span><span class="sxs-lookup"><span data-stu-id="8be08-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="8be08-129">Dodaj klasę `SuppliersController` do folderu controllers.</span><span class="sxs-lookup"><span data-stu-id="8be08-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="8be08-130">Nie pokazuje, jak dodać operacje CRUD dla tego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8be08-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="8be08-131">Kroki są takie same jak dla kontrolera produktów (zobacz [Tworzenie punktu końcowego OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="8be08-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="8be08-132">Pobieranie powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="8be08-132">Getting Related Entities</span></span>

<span data-ttu-id="8be08-133">Aby uzyskać dostawcę produktu, klient wysyła żądanie GET:</span><span class="sxs-lookup"><span data-stu-id="8be08-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="8be08-134">Aby obsłużyć to żądanie, Dodaj następującą metodę do klasy `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="8be08-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="8be08-135">Ta metoda używa domyślnej konwencji nazewnictwa</span><span class="sxs-lookup"><span data-stu-id="8be08-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="8be08-136">Nazwa metody: GetX, gdzie X jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8be08-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="8be08-137">Nazwa parametru: *klucz*</span><span class="sxs-lookup"><span data-stu-id="8be08-137">Parameter name: *key*</span></span>

<span data-ttu-id="8be08-138">Jeśli przestrzegasz tej konwencji nazewnictwa, interfejs API sieci Web automatycznie mapuje żądanie HTTP do metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8be08-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="8be08-139">Przykładowe żądanie HTTP:</span><span class="sxs-lookup"><span data-stu-id="8be08-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="8be08-140">Przykładowa odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="8be08-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="8be08-141">Pobieranie powiązanej kolekcji</span><span class="sxs-lookup"><span data-stu-id="8be08-141">Getting a related collection</span></span>

<span data-ttu-id="8be08-142">W poprzednim przykładzie produkt ma jednego dostawcę.</span><span class="sxs-lookup"><span data-stu-id="8be08-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="8be08-143">Właściwość nawigacji może również zwracać kolekcję.</span><span class="sxs-lookup"><span data-stu-id="8be08-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="8be08-144">Poniższy kod pobiera produkty dla dostawcy:</span><span class="sxs-lookup"><span data-stu-id="8be08-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="8be08-145">W tym przypadku metoda zwraca interfejs **IQueryable** zamiast **SingleResult&lt;t&gt;**</span><span class="sxs-lookup"><span data-stu-id="8be08-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="8be08-146">Przykładowe żądanie HTTP:</span><span class="sxs-lookup"><span data-stu-id="8be08-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="8be08-147">Przykładowa odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="8be08-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="8be08-148">Tworzenie relacji między jednostkami</span><span class="sxs-lookup"><span data-stu-id="8be08-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="8be08-149">Usługa OData obsługuje tworzenie lub usuwanie relacji między dwoma istniejącymi jednostkami.</span><span class="sxs-lookup"><span data-stu-id="8be08-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="8be08-150">W terminologii OData w wersji v4 relacja jest &quot;&quot;odwołania.</span><span class="sxs-lookup"><span data-stu-id="8be08-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="8be08-151">(W protokole OData V3 relacja została wywołana jako *link*.</span><span class="sxs-lookup"><span data-stu-id="8be08-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="8be08-152">Różnice między protokołami nie mają znaczenia dla tego samouczka.)</span><span class="sxs-lookup"><span data-stu-id="8be08-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="8be08-153">Odwołanie ma własny identyfikator URI z formularzem `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="8be08-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="8be08-154">Na przykład poniżej znajduje się identyfikator URI służący do adresowania odwołania między produktem a jego dostawcą:</span><span class="sxs-lookup"><span data-stu-id="8be08-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="8be08-155">Aby dodać relację, klient wysyła żądanie POST lub PUT na ten adres.</span><span class="sxs-lookup"><span data-stu-id="8be08-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="8be08-156">Umieść, jeśli właściwość nawigacji jest pojedynczą jednostką, taką jak `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="8be08-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="8be08-157">Opublikuj, jeśli właściwość nawigacji jest kolekcją, taką jak `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="8be08-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="8be08-158">Treść żądania zawiera identyfikator URI innej jednostki w relacji.</span><span class="sxs-lookup"><span data-stu-id="8be08-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="8be08-159">Oto przykładowe żądanie:</span><span class="sxs-lookup"><span data-stu-id="8be08-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="8be08-160">W tym przykładzie klient wysyła żądanie PUT do `/Products(6)/Supplier/$ref`, czyli $ref URI dla `Supplier` produktu o IDENTYFIKATORze 6.</span><span class="sxs-lookup"><span data-stu-id="8be08-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="8be08-161">Jeśli żądanie zakończy się powodzeniem, serwer wysyła odpowiedź 204 (brak zawartości):</span><span class="sxs-lookup"><span data-stu-id="8be08-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="8be08-162">Poniżej przedstawiono metodę kontrolera umożliwiającą dodanie relacji do `Product`:</span><span class="sxs-lookup"><span data-stu-id="8be08-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="8be08-163">Parametr *navigationProperty* określa relację do ustawienia.</span><span class="sxs-lookup"><span data-stu-id="8be08-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="8be08-164">(Jeśli istnieje więcej niż jedna właściwość nawigacji w jednostce, można dodać więcej instrukcji `case`.)</span><span class="sxs-lookup"><span data-stu-id="8be08-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="8be08-165">Parametr *linku* zawiera identyfikator URI dostawcy.</span><span class="sxs-lookup"><span data-stu-id="8be08-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="8be08-166">Interfejs API sieci Web automatycznie analizuje treść żądania, aby uzyskać wartość dla tego parametru.</span><span class="sxs-lookup"><span data-stu-id="8be08-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="8be08-167">Aby wyszukać dostawcę, potrzebujemy identyfikatora (lub klucza), który jest częścią parametru *linku* .</span><span class="sxs-lookup"><span data-stu-id="8be08-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="8be08-168">W tym celu należy użyć następującej metody pomocnika:</span><span class="sxs-lookup"><span data-stu-id="8be08-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="8be08-169">Zasadniczo ta metoda używa biblioteki OData do dzielenia ścieżki URI na segmenty, wyszukiwanie segmentu, który zawiera klucz, i konwertowanie klucza na poprawny typ.</span><span class="sxs-lookup"><span data-stu-id="8be08-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="8be08-170">Usuwanie relacji między jednostkami</span><span class="sxs-lookup"><span data-stu-id="8be08-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="8be08-171">Aby usunąć relację, klient wysyła żądanie HTTP DELETE do $ref URI:</span><span class="sxs-lookup"><span data-stu-id="8be08-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="8be08-172">Oto Metoda kontrolera, aby usunąć relację między produktem a dostawcą:</span><span class="sxs-lookup"><span data-stu-id="8be08-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="8be08-173">W tym przypadku `Product.Supplier` jest &quot;1&quot; końcu relacji 1-do-wielu, więc można usunąć relację tylko przez ustawienie `Product.Supplier` na `null`.</span><span class="sxs-lookup"><span data-stu-id="8be08-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="8be08-174">Na &quot;wielu&quot; końcu relacji klient musi określić powiązaną jednostkę do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="8be08-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="8be08-175">W tym celu klient wysyła identyfikator URI powiązanej jednostki w ciągu zapytania żądania.</span><span class="sxs-lookup"><span data-stu-id="8be08-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="8be08-176">Na przykład, aby usunąć "produkt 1" z "dostawca 1":</span><span class="sxs-lookup"><span data-stu-id="8be08-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="8be08-177">Aby można było obsługiwać ten interfejs API sieci Web, musimy uwzględnić dodatkowy parametr w metodzie `DeleteRef`.</span><span class="sxs-lookup"><span data-stu-id="8be08-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="8be08-178">Poniżej przedstawiono metodę kontrolera do usunięcia produktu z relacji `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="8be08-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="8be08-179">Parametr *klucza* jest kluczem dostawcy, a parametr *relatedKey* jest kluczem produktu do usunięcia z relacji `Products`.</span><span class="sxs-lookup"><span data-stu-id="8be08-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="8be08-180">Należy zauważyć, że interfejs API sieci Web automatycznie pobiera klucz z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="8be08-180">Note that Web API automatically gets the key from the query string.</span></span>
