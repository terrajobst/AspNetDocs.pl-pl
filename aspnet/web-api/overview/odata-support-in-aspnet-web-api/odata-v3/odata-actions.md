---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Obsługa akcji OData w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'W protokole OData akcje są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach. Niektóre zastosowania do akcji obejmują: Implementuj...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600352"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="2712a-104">Obsługa działań OData w programie ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="2712a-104">Supporting OData Actions in ASP.NET Web API 2</span></span>

<span data-ttu-id="2712a-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2712a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2712a-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="2712a-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="2712a-107">W protokole OData *Akcje* są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach.</span><span class="sxs-lookup"><span data-stu-id="2712a-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="2712a-108">Niektóre zastosowania do akcji obejmują:</span><span class="sxs-lookup"><span data-stu-id="2712a-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="2712a-109">Implementowanie złożonych transakcji.</span><span class="sxs-lookup"><span data-stu-id="2712a-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="2712a-110">Jednoczesne manipulowanie kilkoma jednostkami.</span><span class="sxs-lookup"><span data-stu-id="2712a-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="2712a-111">Zezwalanie na aktualizacje tylko do określonych właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="2712a-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="2712a-112">Wysyłanie informacji do serwera, który nie jest zdefiniowany w jednostce.</span><span class="sxs-lookup"><span data-stu-id="2712a-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2712a-113">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="2712a-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2712a-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="2712a-114">Web API 2</span></span>
> - <span data-ttu-id="2712a-115">Usługa OData w wersji 3</span><span class="sxs-lookup"><span data-stu-id="2712a-115">OData Version 3</span></span>
> - <span data-ttu-id="2712a-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2712a-116">Entity Framework 6</span></span>

## <a name="example-rating-a-product"></a><span data-ttu-id="2712a-117">Przykład: Ocena produktu</span><span class="sxs-lookup"><span data-stu-id="2712a-117">Example: Rating a Product</span></span>

<span data-ttu-id="2712a-118">W tym przykładzie chcemy umożliwić użytkownikom ocenianie produktów, a następnie uwidocznienie średniej klasyfikacji dla każdego produktu.</span><span class="sxs-lookup"><span data-stu-id="2712a-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="2712a-119">W bazie danych zostanie przechowana lista ocen, które zostały dostosowane do produktów.</span><span class="sxs-lookup"><span data-stu-id="2712a-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="2712a-120">Oto model, którego możemy użyć do reprezentowania klasyfikacji w Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="2712a-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="2712a-121">Ale nie chcemy, aby klienci OGŁASZAli obiekt `ProductRating` w kolekcji "ratings".</span><span class="sxs-lookup"><span data-stu-id="2712a-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="2712a-122">Intuicyjnie, ocena jest skojarzona z kolekcją produkty, a klient powinien potrzebować tylko do opublikowania wartości klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="2712a-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="2712a-123">W związku z tym zamiast używania normalnych operacji CRUD definiujemy akcję, którą klient może wywołać w produkcie.</span><span class="sxs-lookup"><span data-stu-id="2712a-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="2712a-124">W terminologii OData akcja jest *powiązana* z jednostkami produktu.</span><span class="sxs-lookup"><span data-stu-id="2712a-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="2712a-125">Akcje mają efekty uboczne na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2712a-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="2712a-126">Z tego powodu są wywoływane przy użyciu żądań HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="2712a-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="2712a-127">Akcje mogą zawierać parametry i typy zwracane, które są opisane w metadanych usługi.</span><span class="sxs-lookup"><span data-stu-id="2712a-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="2712a-128">Klient wysyła parametry w treści żądania, a serwer wysyła wartość zwracaną w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2712a-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="2712a-129">Aby wywołać akcję "Oceń produkt", klient wysyła wpis do identyfikatora URI w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2712a-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="2712a-130">Dane w żądaniu POST są po prostu klasyfikacją produktu:</span><span class="sxs-lookup"><span data-stu-id="2712a-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="2712a-131">Zadeklaruj akcję w Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="2712a-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="2712a-132">W konfiguracji internetowego interfejsu API Dodaj akcję do modelu Entity Data Model (EDM):</span><span class="sxs-lookup"><span data-stu-id="2712a-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="2712a-133">Ten kod definiuje "RateProduct" jako akcję, którą można wykonać w jednostkach produktu.</span><span class="sxs-lookup"><span data-stu-id="2712a-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="2712a-134">Deklaruje także, że akcja przyjmuje parametr **int** o nazwie "Rating" i zwraca wartość **int** .</span><span class="sxs-lookup"><span data-stu-id="2712a-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="2712a-135">Dodaj akcję do kontrolera</span><span class="sxs-lookup"><span data-stu-id="2712a-135">Add the Action to the Controller</span></span>

<span data-ttu-id="2712a-136">Akcja "RateProduct" jest powiązana z jednostkami produktu.</span><span class="sxs-lookup"><span data-stu-id="2712a-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="2712a-137">Aby zaimplementować akcję, Dodaj metodę o nazwie `RateProduct` do kontrolera produktów:</span><span class="sxs-lookup"><span data-stu-id="2712a-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="2712a-138">Zwróć uwagę, że nazwa metody jest zgodna z nazwą akcji w modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="2712a-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="2712a-139">Metoda ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="2712a-139">The method has two parameters:</span></span>

- <span data-ttu-id="2712a-140">*klucz*: klucz produktu do oceniania.</span><span class="sxs-lookup"><span data-stu-id="2712a-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="2712a-141">*Parametry*: słownik wartości parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="2712a-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="2712a-142">Jeśli używasz domyślnych Konwencji routingu, parametr klucza musi mieć nazwę "klucz".</span><span class="sxs-lookup"><span data-stu-id="2712a-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="2712a-143">Ważne jest również uwzględnienie atrybutu **[FromOdataUri]** , jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="2712a-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="2712a-144">Ten atrybut wskazuje, że interfejs API sieci Web ma używać reguł składni OData podczas analizowania klucza z żądania URI.</span><span class="sxs-lookup"><span data-stu-id="2712a-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="2712a-145">Użyj słownika *parametrów* , aby uzyskać parametry akcji:</span><span class="sxs-lookup"><span data-stu-id="2712a-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="2712a-146">Jeśli klient wysyła parametry akcji w prawidłowym formacie, wartość **ModelState. IsValid** ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="2712a-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="2712a-147">W takim przypadku można pobrać wartości parametrów przy użyciu słownika **ODataActionParameters** .</span><span class="sxs-lookup"><span data-stu-id="2712a-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="2712a-148">W tym przykładzie akcja `RateProduct` przyjmuje jeden parametr o nazwie "Rating".</span><span class="sxs-lookup"><span data-stu-id="2712a-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="2712a-149">Metadane akcji</span><span class="sxs-lookup"><span data-stu-id="2712a-149">Action Metadata</span></span>

<span data-ttu-id="2712a-150">Aby wyświetlić metadane usługi, Wyślij żądanie GET do/OData/$metadata.</span><span class="sxs-lookup"><span data-stu-id="2712a-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="2712a-151">Poniżej znajduje się część metadanych, która deklaruje `RateProduct` akcję:</span><span class="sxs-lookup"><span data-stu-id="2712a-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="2712a-152">Element **FunctionImport** deklaruje akcję.</span><span class="sxs-lookup"><span data-stu-id="2712a-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="2712a-153">Większość pól nie ma wyjaśnień, ale dwie są następujące:</span><span class="sxs-lookup"><span data-stu-id="2712a-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="2712a-154">**Isbindd** oznacza, że akcja może być wywoływana w jednostce docelowej, co najmniej jakiś czas.</span><span class="sxs-lookup"><span data-stu-id="2712a-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="2712a-155">**IsAlwaysBindable** oznacza, że akcja zawsze może być wywoływana w jednostce docelowej.</span><span class="sxs-lookup"><span data-stu-id="2712a-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="2712a-156">Różnica polega na tym, że niektóre akcje są zawsze dostępne dla klientów, ale inne akcje mogą zależeć od stanu jednostki.</span><span class="sxs-lookup"><span data-stu-id="2712a-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="2712a-157">Załóżmy na przykład, że zdefiniujesz akcję "Kup".</span><span class="sxs-lookup"><span data-stu-id="2712a-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="2712a-158">Można kupić tylko element, który znajduje się w magazynie.</span><span class="sxs-lookup"><span data-stu-id="2712a-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="2712a-159">Jeśli element jest poza magazynem, klient nie może wywołać tej akcji.</span><span class="sxs-lookup"><span data-stu-id="2712a-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="2712a-160">Podczas definiowania modelu EDM, Metoda **akcji** tworzy zawsze powiązane akcje:</span><span class="sxs-lookup"><span data-stu-id="2712a-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="2712a-161">Porozmawiam w dalszej części tego tematu o akcjach, które nie są zawsze wiązane (nazywane również akcjami *przejściowymi* ).</span><span class="sxs-lookup"><span data-stu-id="2712a-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="2712a-162">Wywoływanie akcji</span><span class="sxs-lookup"><span data-stu-id="2712a-162">Invoking the Action</span></span>

<span data-ttu-id="2712a-163">Teraz zobaczmy, jak klient wywoła tę akcję.</span><span class="sxs-lookup"><span data-stu-id="2712a-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="2712a-164">Załóżmy, że klient chce przekazać klasyfikację 2 do produktu o IDENTYFIKATORze = 4.</span><span class="sxs-lookup"><span data-stu-id="2712a-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="2712a-165">Oto przykładowy komunikat żądania, który używa formatu JSON dla treści żądania:</span><span class="sxs-lookup"><span data-stu-id="2712a-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="2712a-166">Oto komunikat odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="2712a-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="2712a-167">Wiązanie akcji z zestawem jednostek</span><span class="sxs-lookup"><span data-stu-id="2712a-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="2712a-168">W poprzednim przykładzie akcja jest powiązana z pojedynczą jednostką: klient klasyfikuje pojedynczy produkt.</span><span class="sxs-lookup"><span data-stu-id="2712a-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="2712a-169">Można również powiązać akcję z kolekcją jednostek.</span><span class="sxs-lookup"><span data-stu-id="2712a-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="2712a-170">Po prostu wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="2712a-170">Just make the following changes:</span></span>

<span data-ttu-id="2712a-171">W modelu EDM Dodaj akcję do właściwości **kolekcji** jednostki.</span><span class="sxs-lookup"><span data-stu-id="2712a-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="2712a-172">W metodzie kontrolera Pomiń parametr *klucza* .</span><span class="sxs-lookup"><span data-stu-id="2712a-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="2712a-173">Teraz klient wywołuje akcję dla zestawu jednostek Products:</span><span class="sxs-lookup"><span data-stu-id="2712a-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="2712a-174">Akcje z parametrami kolekcji</span><span class="sxs-lookup"><span data-stu-id="2712a-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="2712a-175">Akcje mogą mieć parametry, które pobierają kolekcję wartości.</span><span class="sxs-lookup"><span data-stu-id="2712a-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="2712a-176">W modelu EDM Użyj **CollectionParameter&lt;t&gt;** , aby zadeklarować parametr.</span><span class="sxs-lookup"><span data-stu-id="2712a-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="2712a-177">Deklaruje on parametr o nazwie "ratings", który pobiera kolekcję wartości **int** .</span><span class="sxs-lookup"><span data-stu-id="2712a-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="2712a-178">W metodzie kontrolera nadal otrzymujesz wartość parametru z obiektu **ODataActionParameters** , ale teraz wartość jest typu **ICollection&lt;int&gt;** wartość:</span><span class="sxs-lookup"><span data-stu-id="2712a-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="2712a-179">Akcje przejściowe</span><span class="sxs-lookup"><span data-stu-id="2712a-179">Transient Actions</span></span>

<span data-ttu-id="2712a-180">W przykładzie "RateProduct" użytkownicy zawsze mogą ocenić produkt, więc akcja jest zawsze dostępna.</span><span class="sxs-lookup"><span data-stu-id="2712a-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="2712a-181">Jednak niektóre akcje zależą od stanu jednostki.</span><span class="sxs-lookup"><span data-stu-id="2712a-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="2712a-182">Na przykład w usłudze najmu wideo akcja "wyewidencjonowywanie" nie zawsze jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="2712a-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="2712a-183">(Jest to zależne od tego, czy kopia tego wideo jest dostępna). Akcja tego typu jest nazywana akcją *przejściową* .</span><span class="sxs-lookup"><span data-stu-id="2712a-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="2712a-184">W metadanych usługi przejściowa akcja ma **IsAlwaysBindable** równa false.</span><span class="sxs-lookup"><span data-stu-id="2712a-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="2712a-185">Jest to rzeczywista wartość domyślna, więc metadane będą wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="2712a-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="2712a-186">Oto dlaczego te kwestie: Jeśli akcja jest przejściowa, serwer musi poinformować klienta, gdy akcja jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="2712a-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="2712a-187">Robi to poprzez dołączenie linku do akcji w jednostce.</span><span class="sxs-lookup"><span data-stu-id="2712a-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="2712a-188">Oto przykład dla jednostki filmu:</span><span class="sxs-lookup"><span data-stu-id="2712a-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="2712a-189">Właściwość "#CheckOut" zawiera link do akcji wyewidencjonowywania.</span><span class="sxs-lookup"><span data-stu-id="2712a-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="2712a-190">Jeśli akcja jest niedostępna, serwer pomija link.</span><span class="sxs-lookup"><span data-stu-id="2712a-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="2712a-191">Aby zadeklarować przejściową akcję w modelu EDM, wywołaj metodę **TransientAction** :</span><span class="sxs-lookup"><span data-stu-id="2712a-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="2712a-192">Ponadto należy podać funkcję, która zwraca link akcji dla danej jednostki.</span><span class="sxs-lookup"><span data-stu-id="2712a-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="2712a-193">Ustaw tę funkcję, wywołując **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="2712a-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="2712a-194">Funkcję można napisać jako wyrażenie lambda:</span><span class="sxs-lookup"><span data-stu-id="2712a-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="2712a-195">Jeśli akcja jest dostępna, wyrażenie lambda zwraca link do akcji.</span><span class="sxs-lookup"><span data-stu-id="2712a-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="2712a-196">Serializator OData zawiera ten link, gdy serializować jednostkę.</span><span class="sxs-lookup"><span data-stu-id="2712a-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="2712a-197">Gdy akcja jest niedostępna, funkcja zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="2712a-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2712a-198">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="2712a-198">Additional Resources</span></span>

[<span data-ttu-id="2712a-199">Przykład akcji OData</span><span class="sxs-lookup"><span data-stu-id="2712a-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
