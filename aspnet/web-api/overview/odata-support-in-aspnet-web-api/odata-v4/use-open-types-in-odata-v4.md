---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Otwieranie typów w protokole OData v4 przy użyciu interfejsu API sieci Web ASP.NET | Microsoft Docs
author: microsoft
description: W protokole OData v4 typem otwartym jest typ strukturalny, który zawiera właściwości dynamiczne, a także wszystkie właściwości, które są zadeklarowane w definicji typu. Otwórz...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622179"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="0ce7e-104">Typy otwarte w protokole OData v4 z interfejsem API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0ce7e-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="0ce7e-105">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0ce7e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0ce7e-106">W protokole OData v4 *typem otwartym* jest typ strukturalny, który zawiera właściwości dynamiczne, a także wszystkie właściwości, które są zadeklarowane w definicji typu.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="0ce7e-107">Typy otwarte umożliwiają dodawanie elastyczności do modeli danych.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="0ce7e-108">W tym samouczku pokazano, jak używać typów otwartych w usłudze ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="0ce7e-109">W tym samouczku założono, że wiesz już, jak utworzyć punkt końcowy OData w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="0ce7e-110">W przeciwnym razie Zacznij od odczytywania najpierw [Utwórz punkt końcowy OData v4](create-an-odata-v4-endpoint.md) .</span><span class="sxs-lookup"><span data-stu-id="0ce7e-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0ce7e-111">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="0ce7e-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0ce7e-112">Internetowy interfejs API OData 5,3</span><span class="sxs-lookup"><span data-stu-id="0ce7e-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="0ce7e-113">OData 4</span><span class="sxs-lookup"><span data-stu-id="0ce7e-113">OData v4</span></span>

<span data-ttu-id="0ce7e-114">Najpierw niektóre terminologia OData:</span><span class="sxs-lookup"><span data-stu-id="0ce7e-114">First, some OData terminology:</span></span>

- <span data-ttu-id="0ce7e-115">Typ jednostki: typ strukturalny z kluczem.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="0ce7e-116">Typ złożony: typ strukturalny bez klucza.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="0ce7e-117">Typ otwarcia: typ z właściwościami dynamicznymi.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="0ce7e-118">Można otworzyć zarówno typy jednostek, jak i typy złożone.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="0ce7e-119">Wartość właściwości dynamicznej może być typu pierwotnego, typu złożonego lub typu wyliczeniowego; lub kolekcja dowolnego z tych typów.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="0ce7e-120">Aby uzyskać więcej informacji na temat typów otwartych, zobacz [specyfikację OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="0ce7e-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="0ce7e-121">Instalowanie bibliotek usługi Web OData</span><span class="sxs-lookup"><span data-stu-id="0ce7e-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="0ce7e-122">Zainstaluj najnowsze biblioteki OData interfejsu API sieci Web za pomocą Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="0ce7e-123">W oknie Konsola Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="0ce7e-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="0ce7e-124">Definiowanie typów CLR</span><span class="sxs-lookup"><span data-stu-id="0ce7e-124">Define the CLR Types</span></span>

<span data-ttu-id="0ce7e-125">Zacznij od zdefiniowania modeli modelu EDM jako typów CLR.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="0ce7e-126">Po utworzeniu Entity Data Model (EDM)</span><span class="sxs-lookup"><span data-stu-id="0ce7e-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="0ce7e-127">`Category` jest typem wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="0ce7e-128">`Address` jest typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-128">`Address` is a complex type.</span></span> <span data-ttu-id="0ce7e-129">(Nie ma klucza, dlatego nie jest typem jednostki).</span><span class="sxs-lookup"><span data-stu-id="0ce7e-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="0ce7e-130">`Customer` to typ jednostki.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-130">`Customer` is an entity type.</span></span> <span data-ttu-id="0ce7e-131">(Ma klucz).</span><span class="sxs-lookup"><span data-stu-id="0ce7e-131">(It has a key.)</span></span>
- <span data-ttu-id="0ce7e-132">`Press` jest otwartym typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="0ce7e-133">`Book` to otwarty typ jednostki.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="0ce7e-134">Aby utworzyć typ otwarty, typ CLR musi mieć właściwość typu `IDictionary<string, object>`, która zawiera właściwości dynamiczne.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="0ce7e-135">Kompiluj model modelu EDM</span><span class="sxs-lookup"><span data-stu-id="0ce7e-135">Build the EDM Model</span></span>

<span data-ttu-id="0ce7e-136">Jeśli używasz **ODataConventionModelBuilder** do tworzenia modelu EDM, `Press` i `Book` są automatycznie dodawane jako typy otwarte, na podstawie obecności właściwości `IDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="0ce7e-137">Możesz również jawnie skompilować modelu EDM przy użyciu **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="0ce7e-138">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="0ce7e-138">Add an OData Controller</span></span>

<span data-ttu-id="0ce7e-139">Następnie Dodaj kontroler OData.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-139">Next, add an OData controller.</span></span> <span data-ttu-id="0ce7e-140">Na potrzeby tego samouczka użyjesz uproszczonego kontrolera, który obsługuje tylko żądania GET i POST, i używa listy znajdującej się w pamięci do przechowywania jednostek.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="0ce7e-141">Zwróć uwagę, że pierwsze wystąpienie `Book` nie ma właściwości dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="0ce7e-142">Drugie wystąpienie `Book` ma następujące właściwości dynamiczne:</span><span class="sxs-lookup"><span data-stu-id="0ce7e-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="0ce7e-143">"Opublikowany": typ pierwotny</span><span class="sxs-lookup"><span data-stu-id="0ce7e-143">"Published": Primitive type</span></span>
- <span data-ttu-id="0ce7e-144">"Authors": Kolekcja typów pierwotnych</span><span class="sxs-lookup"><span data-stu-id="0ce7e-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="0ce7e-145">"OtherCategories": Kolekcja typów wyliczeniowych.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="0ce7e-146">Ponadto Właściwość `Press` tego wystąpienia `Book` ma następujące właściwości dynamiczne:</span><span class="sxs-lookup"><span data-stu-id="0ce7e-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="0ce7e-147">"Blog": typ pierwotny</span><span class="sxs-lookup"><span data-stu-id="0ce7e-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="0ce7e-148">"Address": typ złożony</span><span class="sxs-lookup"><span data-stu-id="0ce7e-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="0ce7e-149">Zapytanie dotyczące metadanych</span><span class="sxs-lookup"><span data-stu-id="0ce7e-149">Query the Metadata</span></span>

<span data-ttu-id="0ce7e-150">Aby uzyskać dokument metadanych OData, Wyślij żądanie GET do `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="0ce7e-151">Treść odpowiedzi powinna wyglądać podobnie do tego:</span><span class="sxs-lookup"><span data-stu-id="0ce7e-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="0ce7e-152">W dokumencie metadanych można zobaczyć, że:</span><span class="sxs-lookup"><span data-stu-id="0ce7e-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="0ce7e-153">Dla typów `Book` i `Press` wartość atrybutu `OpenType` jest true.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="0ce7e-154">Typy `Customer` i `Address` nie mają tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="0ce7e-155">Typ jednostki `Book` ma trzy zadeklarowane właściwości: ISBN, title i Press.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="0ce7e-156">Metadane OData nie zawierają właściwości `Book.Properties` z klasy CLR.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="0ce7e-157">Podobnie typ złożony `Press` ma tylko dwie zadeklarowane właściwości: Name i Category.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="0ce7e-158">Metadane nie zawierają właściwości `Press.DynamicProperties` z klasy CLR.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="0ce7e-159">Zapytanie o jednostkę</span><span class="sxs-lookup"><span data-stu-id="0ce7e-159">Query an Entity</span></span>

<span data-ttu-id="0ce7e-160">Aby uzyskać książkę z ISBN równą "978-0-7356-7942-9", Wyślij żądanie GET do `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="0ce7e-161">Treść odpowiedzi powinna wyglądać podobnie do poniższego.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-161">The response body should look similar to the following.</span></span> <span data-ttu-id="0ce7e-162">(Z wcięciem, aby zwiększyć czytelność).</span><span class="sxs-lookup"><span data-stu-id="0ce7e-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="0ce7e-163">Zauważ, że właściwości dynamiczne są zawarte w tekście z zadeklarowanymi właściwościami.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="0ce7e-164">Opublikuj jednostkę</span><span class="sxs-lookup"><span data-stu-id="0ce7e-164">POST an Entity</span></span>

<span data-ttu-id="0ce7e-165">Aby dodać jednostkę książki, Wyślij żądanie POST do `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="0ce7e-166">Klient może ustawić właściwości dynamiczne w ładunku żądania.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="0ce7e-167">Oto przykładowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-167">Here is an example request.</span></span> <span data-ttu-id="0ce7e-168">Zanotuj właściwości "price" i "Opublikowano".</span><span class="sxs-lookup"><span data-stu-id="0ce7e-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="0ce7e-169">W przypadku ustawienia punktu przerwania w metodzie kontrolera można zobaczyć, że interfejs API sieci Web dodał te właściwości do słownika `Properties`.</span><span class="sxs-lookup"><span data-stu-id="0ce7e-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="0ce7e-170">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="0ce7e-170">Additional Resources</span></span>

[<span data-ttu-id="0ce7e-171">Przykład typu Open Type protokołu OData</span><span class="sxs-lookup"><span data-stu-id="0ce7e-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
