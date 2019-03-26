---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Otwórz typy w protokole OData v4 Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: microsoft
description: W protokole OData v4 otwarty typ jest strukturą typu, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu. Otwórz...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: f901e5efc38e5cda6eb606b6bc1ecfe7dea3599c
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58423441"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="a3407-104">Otwórz typy w protokole OData v4 Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a3407-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="a3407-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a3407-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a3407-106">W protokole OData v4 *Otwórz typ* jest strukturą typu, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu.</span><span class="sxs-lookup"><span data-stu-id="a3407-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="a3407-107">Typy otwarte umożliwiają bardziej elastyczne modeli danych.</span><span class="sxs-lookup"><span data-stu-id="a3407-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="a3407-108">W tym samouczku pokazano, jak używać typy otwarte w programie ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="a3407-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="a3407-109">Ten samouczek zakłada się, czy znasz już sposób tworzenia punktu końcowego OData w interfejsie API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a3407-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="a3407-110">Jeśli nie, należy zacząć od przeczytania [utworzenie punktu końcowego OData v4](create-an-odata-v4-endpoint.md) pierwszy.</span><span class="sxs-lookup"><span data-stu-id="a3407-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a3407-111">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="a3407-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a3407-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="a3407-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="a3407-113">OData 4</span><span class="sxs-lookup"><span data-stu-id="a3407-113">OData v4</span></span>


<span data-ttu-id="a3407-114">Pierwszy, niektóre terminologii OData:</span><span class="sxs-lookup"><span data-stu-id="a3407-114">First, some OData terminology:</span></span>

- <span data-ttu-id="a3407-115">Typ jednostki: Typem strukturalnym przy użyciu klucza.</span><span class="sxs-lookup"><span data-stu-id="a3407-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="a3407-116">Typ złożony: Typ ze strukturą, bez klucza.</span><span class="sxs-lookup"><span data-stu-id="a3407-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="a3407-117">Otwórz, wpisz: Typ właściwości dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="a3407-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="a3407-118">Typy jednostek i typów złożonych może być otwarty.</span><span class="sxs-lookup"><span data-stu-id="a3407-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="a3407-119">Wartość właściwości dynamicznych może być typ pierwotny, typu złożonego lub typ wyliczenia; lub kolekcja każdego z tych typów.</span><span class="sxs-lookup"><span data-stu-id="a3407-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="a3407-120">Aby uzyskać więcej informacji na temat otwartych typów, zobacz [specyfikacji v4 OData](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="a3407-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="a3407-121">Instalowanie bibliotek OData w sieci Web</span><span class="sxs-lookup"><span data-stu-id="a3407-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="a3407-122">Użyj Menedżera pakietów NuGet do zainstalowania najnowszej biblioteki Web API OData.</span><span class="sxs-lookup"><span data-stu-id="a3407-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="a3407-123">W oknie Konsola Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="a3407-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="a3407-124">Definiowanie typów CLR</span><span class="sxs-lookup"><span data-stu-id="a3407-124">Define the CLR Types</span></span>

<span data-ttu-id="a3407-125">Rozpocznij, definiując modele EDM jako typy CLR.</span><span class="sxs-lookup"><span data-stu-id="a3407-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="a3407-126">Po utworzeniu Entity Data Model (EDM)</span><span class="sxs-lookup"><span data-stu-id="a3407-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="a3407-127">`Category` jest typem wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="a3407-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="a3407-128">`Address` jest to typ złożony.</span><span class="sxs-lookup"><span data-stu-id="a3407-128">`Address` is a complex type.</span></span> <span data-ttu-id="a3407-129">(Go nie ma klucza, więc nie jest typem jednostki.)</span><span class="sxs-lookup"><span data-stu-id="a3407-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="a3407-130">`Customer` nie jest typem jednostki.</span><span class="sxs-lookup"><span data-stu-id="a3407-130">`Customer` is an entity type.</span></span> <span data-ttu-id="a3407-131">(Ma klucz).</span><span class="sxs-lookup"><span data-stu-id="a3407-131">(It has a key.)</span></span>
- <span data-ttu-id="a3407-132">`Press` to otwarty typ złożony.</span><span class="sxs-lookup"><span data-stu-id="a3407-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="a3407-133">`Book` nie jest typem jednostki open.</span><span class="sxs-lookup"><span data-stu-id="a3407-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="a3407-134">Aby utworzyć typu otwartego, typ CLR musi mieć właściwość typu `IDictionary<string, object>`, który zawiera właściwości dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="a3407-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="a3407-135">Budowanie modelu EDM</span><span class="sxs-lookup"><span data-stu-id="a3407-135">Build the EDM Model</span></span>

<span data-ttu-id="a3407-136">Jeśli używasz **ODataConventionModelBuilder** utworzyć EDM, `Press` i `Book` są automatycznie dodawane jako typy otwarte na podstawie obecności `IDictionary<string, object>` właściwości.</span><span class="sxs-lookup"><span data-stu-id="a3407-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="a3407-137">Możesz również tworzyć EDM jawnie, przy użyciu **element ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="a3407-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="a3407-138">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="a3407-138">Add an OData Controller</span></span>

<span data-ttu-id="a3407-139">Następnie dodaj kontrolerze OData.</span><span class="sxs-lookup"><span data-stu-id="a3407-139">Next, add an OData controller.</span></span> <span data-ttu-id="a3407-140">W tym samouczku użyjemy uproszczony kontroler, że obsługuje tylko pobieranie POST żądań i używa listy w pamięci na przechowywanie podmioty.</span><span class="sxs-lookup"><span data-stu-id="a3407-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="a3407-141">Należy zauważyć, że pierwszy `Book` wystąpienia nie ma żadnych właściwości dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="a3407-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="a3407-142">Drugi `Book` wystąpienia ma następujące właściwości dynamicznych:</span><span class="sxs-lookup"><span data-stu-id="a3407-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="a3407-143">Wartość "opublikowany": typ pierwotny</span><span class="sxs-lookup"><span data-stu-id="a3407-143">"Published": Primitive type</span></span>
- <span data-ttu-id="a3407-144">"Autorzy": Kolekcja typów podstawowych</span><span class="sxs-lookup"><span data-stu-id="a3407-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="a3407-145">"OtherCategories": Kolekcja typów wyliczeń.</span><span class="sxs-lookup"><span data-stu-id="a3407-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="a3407-146">Ponadto `Press` właściwość, która `Book` wystąpienia ma następujące właściwości dynamicznych:</span><span class="sxs-lookup"><span data-stu-id="a3407-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="a3407-147">"Blog": typ pierwotny</span><span class="sxs-lookup"><span data-stu-id="a3407-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="a3407-148">"Address": Typ złożony</span><span class="sxs-lookup"><span data-stu-id="a3407-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="a3407-149">Zapytanie metadanych</span><span class="sxs-lookup"><span data-stu-id="a3407-149">Query the Metadata</span></span>

<span data-ttu-id="a3407-150">Do pobierania dokumentu metadanych OData, Wyślij żądanie Pobierz do `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="a3407-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="a3407-151">Treść odpowiedzi powinna wyglądać mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="a3407-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="a3407-152">Z dokumentu metadanych możesz zobaczyć, który:</span><span class="sxs-lookup"><span data-stu-id="a3407-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="a3407-153">Aby uzyskać `Book` i `Press` typów wartości `OpenType` atrybut ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="a3407-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="a3407-154">`Customer` i `Address` typów nie ma tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a3407-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="a3407-155">`Book` Typ jednostki ma trzy właściwości zadeklarowanych: ISBN, tytułu i naciśnij klawisz.</span><span class="sxs-lookup"><span data-stu-id="a3407-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="a3407-156">Nie ma metadanych OData `Book.Properties` właściwości z klasy CLR.</span><span class="sxs-lookup"><span data-stu-id="a3407-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="a3407-157">Podobnie `Press` typ złożony zawiera tylko dwa zadeklarowane właściwości: Nazwa i kategorii.</span><span class="sxs-lookup"><span data-stu-id="a3407-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="a3407-158">Metadane nie zawierają `Press.DynamicProperties` właściwości z klasy CLR.</span><span class="sxs-lookup"><span data-stu-id="a3407-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="a3407-159">Zapytanie jednostki</span><span class="sxs-lookup"><span data-stu-id="a3407-159">Query an Entity</span></span>

<span data-ttu-id="a3407-160">Aby uzyskać książki z ISBN jest równa "978-0-7356-7942-9", Wyślij żądanie Pobierz do `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="a3407-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="a3407-161">Treść odpowiedzi powinny wyglądać podobnie do poniższego.</span><span class="sxs-lookup"><span data-stu-id="a3407-161">The response body should look similar to the following.</span></span> <span data-ttu-id="a3407-162">(Wcięty, aby był bardziej czytelny.)</span><span class="sxs-lookup"><span data-stu-id="a3407-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="a3407-163">Zauważ, że właściwości dynamicznych są dołączone jako śródwierszowego zadeklarowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="a3407-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="a3407-164">WPIS w jednostce</span><span class="sxs-lookup"><span data-stu-id="a3407-164">POST an Entity</span></span>

<span data-ttu-id="a3407-165">Aby dodać jednostkę książki, Wyślij żądanie POST `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="a3407-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="a3407-166">Klienta można ustawić właściwości dynamicznych w ładunku żądania.</span><span class="sxs-lookup"><span data-stu-id="a3407-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="a3407-167">Oto przykładowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="a3407-167">Here is an example request.</span></span> <span data-ttu-id="a3407-168">Należy pamiętać, właściwości "Price" i "Opublikowano".</span><span class="sxs-lookup"><span data-stu-id="a3407-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="a3407-169">Jeśli ustawisz punkt przerwania w metodzie kontrolera widać, że internetowego interfejsu API dodane tych właściwości, aby `Properties` słownika.</span><span class="sxs-lookup"><span data-stu-id="a3407-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="a3407-170">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a3407-170">Additional Resources</span></span>

[<span data-ttu-id="a3407-171">Przykładowe typu OData Open</span><span class="sxs-lookup"><span data-stu-id="a3407-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
