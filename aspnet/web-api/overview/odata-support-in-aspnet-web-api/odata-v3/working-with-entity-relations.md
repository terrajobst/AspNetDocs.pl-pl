---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Obsługa relacji jednostek w protokole OData V3 przy użyciu interfejsu Web API 2 | Microsoft Docs
author: MikeWasson
description: 'Większość zestawów danych definiuje relacje między jednostkami: klienci mają zamówienia; książki mają autorów; produkty mają dostawców. Korzystając z protokołu OData, klienci mogą przechodzić...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600312"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="72c1b-104">Obsługa relacji jednostek w protokole OData V3 przy użyciu interfejsu Web API 2</span><span class="sxs-lookup"><span data-stu-id="72c1b-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>

<span data-ttu-id="72c1b-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="72c1b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="72c1b-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="72c1b-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="72c1b-107">Większość zestawów danych definiuje relacje między jednostkami: klienci mają zamówienia; książki mają autorów; produkty mają dostawców.</span><span class="sxs-lookup"><span data-stu-id="72c1b-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="72c1b-108">Korzystając z protokołu OData, klienci mogą nawigować po relacjach jednostek.</span><span class="sxs-lookup"><span data-stu-id="72c1b-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="72c1b-109">W danym produkcie można znaleźć dostawcę.</span><span class="sxs-lookup"><span data-stu-id="72c1b-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="72c1b-110">Można również tworzyć i usuwać relacje.</span><span class="sxs-lookup"><span data-stu-id="72c1b-110">You can also create or remove relationships.</span></span> <span data-ttu-id="72c1b-111">Na przykład można ustawić dostawcę dla produktu.</span><span class="sxs-lookup"><span data-stu-id="72c1b-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="72c1b-112">W tym samouczku pokazano, jak obsługiwać te operacje w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="72c1b-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="72c1b-113">Samouczek jest oparty na samouczku [Tworzenie punktu końcowego OData V3 przy użyciu interfejsu Web API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="72c1b-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="72c1b-114">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="72c1b-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="72c1b-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="72c1b-115">Web API 2</span></span>
> - <span data-ttu-id="72c1b-116">Usługa OData w wersji 3</span><span class="sxs-lookup"><span data-stu-id="72c1b-116">OData Version 3</span></span>
> - <span data-ttu-id="72c1b-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="72c1b-117">Entity Framework 6</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="72c1b-118">Dodawanie jednostki dostawcy</span><span class="sxs-lookup"><span data-stu-id="72c1b-118">Add a Supplier Entity</span></span>

<span data-ttu-id="72c1b-119">Najpierw musimy dodać nowy typ jednostki do naszego źródła danych OData.</span><span class="sxs-lookup"><span data-stu-id="72c1b-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="72c1b-120">Dodamy klasę `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="72c1b-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="72c1b-121">Ta klasa używa ciągu dla klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="72c1b-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="72c1b-122">W przypadku, które mogą być mniej typowe niż użycie klucza liczb całkowitych.</span><span class="sxs-lookup"><span data-stu-id="72c1b-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="72c1b-123">Jednak warto zobaczyć, jak usługa OData obsługuje inne typy kluczy niż liczby całkowite.</span><span class="sxs-lookup"><span data-stu-id="72c1b-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="72c1b-124">Następnie utworzymy relację poprzez dodanie właściwości `Supplier` do klasy `Product`:</span><span class="sxs-lookup"><span data-stu-id="72c1b-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="72c1b-125">Dodaj nowy **nieogólnymi** do klasy `ProductServiceContext`, tak aby Entity Framework obejmował tabelę `Supplier` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="72c1b-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="72c1b-126">W programie WebApiConfig.cs Dodaj jednostkę "dostawcy" do modelu modelu EDM:</span><span class="sxs-lookup"><span data-stu-id="72c1b-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="72c1b-127">Właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="72c1b-127">Navigation Properties</span></span>

<span data-ttu-id="72c1b-128">Aby uzyskać dostawcę produktu, klient wysyła żądanie GET:</span><span class="sxs-lookup"><span data-stu-id="72c1b-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="72c1b-129">Tutaj "dostawca" jest właściwością nawigacji dla typu `Product`.</span><span class="sxs-lookup"><span data-stu-id="72c1b-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="72c1b-130">W tym przypadku `Supplier` odnosi się do pojedynczego elementu, ale właściwość nawigacji może również zwracać kolekcję (relacja jeden-do-wielu lub wiele-do-wielu).</span><span class="sxs-lookup"><span data-stu-id="72c1b-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="72c1b-131">Aby obsłużyć to żądanie, Dodaj następującą metodę do klasy `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="72c1b-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="72c1b-132">Parametr *klucza* jest kluczem produktu.</span><span class="sxs-lookup"><span data-stu-id="72c1b-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="72c1b-133">Metoda zwraca pokrewną jednostkę&#8212;w tym przypadku wystąpienie `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="72c1b-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="72c1b-134">Nazwa metody i nazwa parametru są oba ważne.</span><span class="sxs-lookup"><span data-stu-id="72c1b-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="72c1b-135">Ogólnie rzecz biorąc, jeśli właściwość nawigacji ma nazwę "X", należy dodać metodę o nazwie "GetX".</span><span class="sxs-lookup"><span data-stu-id="72c1b-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="72c1b-136">Metoda musi przyjmować parametr o nazwie "*Key*", który jest zgodny z typem danych klucza nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="72c1b-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="72c1b-137">Ważne jest również uwzględnienie atrybutu **[FromOdataUri]** w parametrze *klucza* .</span><span class="sxs-lookup"><span data-stu-id="72c1b-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="72c1b-138">Ten atrybut wskazuje, że interfejs API sieci Web ma używać reguł składni OData podczas analizowania klucza z żądania URI.</span><span class="sxs-lookup"><span data-stu-id="72c1b-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="72c1b-139">Tworzenie i usuwanie linków</span><span class="sxs-lookup"><span data-stu-id="72c1b-139">Creating and Deleting Links</span></span>

<span data-ttu-id="72c1b-140">Usługa OData obsługuje tworzenie lub usuwanie relacji między dwoma jednostkami.</span><span class="sxs-lookup"><span data-stu-id="72c1b-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="72c1b-141">W terminologii OData relacja jest "link".</span><span class="sxs-lookup"><span data-stu-id="72c1b-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="72c1b-142">Każdy link ma identyfikator URI z *jednostką*/$Links/*jednostką*.</span><span class="sxs-lookup"><span data-stu-id="72c1b-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="72c1b-143">Na przykład łącze od produktu do dostawcy wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="72c1b-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="72c1b-144">Aby utworzyć nowe łącze, klient wysyła żądanie POST do identyfikatora URI linku.</span><span class="sxs-lookup"><span data-stu-id="72c1b-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="72c1b-145">Treść żądania jest identyfikatorem URI jednostki docelowej.</span><span class="sxs-lookup"><span data-stu-id="72c1b-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="72c1b-146">Załóżmy na przykład, że istnieje dostawca z kluczem "CTSO".</span><span class="sxs-lookup"><span data-stu-id="72c1b-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="72c1b-147">Aby utworzyć łącze "Product (1)" do "dostawca (" CTSO "), klient wysyła żądanie podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="72c1b-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="72c1b-148">Aby usunąć łącze, klient wysyła żądanie usunięcia do identyfikatora URI linku.</span><span class="sxs-lookup"><span data-stu-id="72c1b-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="72c1b-149">**Tworzenie linków**</span><span class="sxs-lookup"><span data-stu-id="72c1b-149">**Creating Links**</span></span>

<span data-ttu-id="72c1b-150">Aby umożliwić klientowi tworzenie linków dostawcy produktu, Dodaj następujący kod do klasy `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="72c1b-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="72c1b-151">Ta metoda przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="72c1b-151">This method takes three parameters:</span></span>

- <span data-ttu-id="72c1b-152">*klucz*: klucz do jednostki nadrzędnej (produkt)</span><span class="sxs-lookup"><span data-stu-id="72c1b-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="72c1b-153">*navigationProperty*: Nazwa właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="72c1b-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="72c1b-154">W tym przykładzie jedyną prawidłową właściwością nawigacji jest "dostawca".</span><span class="sxs-lookup"><span data-stu-id="72c1b-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="72c1b-155">*link*: identyfikator URI OData jednostki powiązanej.</span><span class="sxs-lookup"><span data-stu-id="72c1b-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="72c1b-156">Ta wartość jest pobierana z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="72c1b-156">This value is taken from the request body.</span></span> <span data-ttu-id="72c1b-157">Na przykład identyfikator URI linku może mieć wartość "`http://localhost/odata/Suppliers('CTSO')`, co oznacza dostawcę o IDENTYFIKATORze" CTSO ".</span><span class="sxs-lookup"><span data-stu-id="72c1b-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="72c1b-158">Metoda używa linku, aby wyszukać dostawcę.</span><span class="sxs-lookup"><span data-stu-id="72c1b-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="72c1b-159">Jeśli zostanie znaleziony pasujący dostawca, Metoda ustawia właściwość `Product.Supplier` i zapisuje wynik do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72c1b-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="72c1b-160">Najtrudniejszą częścią jest analiza identyfikatora URI linku.</span><span class="sxs-lookup"><span data-stu-id="72c1b-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="72c1b-161">Zasadniczo należy zasymulować wynik wysłania żądania GET do tego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="72c1b-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="72c1b-162">W poniższej metodzie pomocnika pokazano, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="72c1b-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="72c1b-163">Metoda wywołuje proces routingu interfejsu API sieci Web i przywraca wystąpienie **ODataPath** reprezentujące przeanalizowane ścieżki OData.</span><span class="sxs-lookup"><span data-stu-id="72c1b-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="72c1b-164">Dla identyfikatora URI łącza jeden z segmentów powinien być kluczem jednostki.</span><span class="sxs-lookup"><span data-stu-id="72c1b-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="72c1b-165">(Jeśli nie, klient wysłał nieprawidłowy identyfikator URI).</span><span class="sxs-lookup"><span data-stu-id="72c1b-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="72c1b-166">**Usuwanie linków**</span><span class="sxs-lookup"><span data-stu-id="72c1b-166">**Deleting Links**</span></span>

<span data-ttu-id="72c1b-167">Aby usunąć łącze, Dodaj następujący kod do klasy `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="72c1b-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="72c1b-168">W tym przykładzie właściwość nawigacji jest pojedynczą jednostką `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="72c1b-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="72c1b-169">Jeśli właściwość nawigacji jest kolekcją, identyfikator URI służący do usunięcia linku musi zawierać klucz powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="72c1b-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="72c1b-170">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="72c1b-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="72c1b-171">To żądanie usuwa zamówienie 1 od klienta 1.</span><span class="sxs-lookup"><span data-stu-id="72c1b-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="72c1b-172">W takim przypadku Metoda DeleteLink będzie miała następujący podpis:</span><span class="sxs-lookup"><span data-stu-id="72c1b-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="72c1b-173">Parametr *relatedKey* zwraca klucz jednostki powiązanej.</span><span class="sxs-lookup"><span data-stu-id="72c1b-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="72c1b-174">Tak więc w metodzie `DeleteLink` należy odszukać jednostkę podstawową według parametru *klucza* , znaleźć powiązaną jednostkę przez parametr *relatedKey* , a następnie usunąć skojarzenie.</span><span class="sxs-lookup"><span data-stu-id="72c1b-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="72c1b-175">W zależności od modelu danych może być konieczne zaimplementowanie obu wersji `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="72c1b-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="72c1b-176">Interfejs API sieci Web będzie wywoływał poprawną wersję na podstawie identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="72c1b-176">Web API will call the correct version based on the request URI.</span></span>
