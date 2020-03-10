---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Dziedziczenie typu złożonego w protokole OData v4 przy użyciu interfejsu API sieci Web ASP.NET | Microsoft Docs
author: microsoft
description: Zgodnie ze specyfikacją OData v4, typ złożony może dziedziczyć z innego typu złożonego. (Typ złożony jest typem strukturalnym bez klucza). Internetowy interfejs API...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556309"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="37697-104">Dziedziczenie typu złożonego w protokole OData v4 przy użyciu interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="37697-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="37697-105">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="37697-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="37697-106">Zgodnie ze [specyfikacją](http://www.odata.org/documentation/odata-version-4-0/)OData v4, typ złożony może dziedziczyć z innego typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="37697-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="37697-107">(Typ *złożony* jest typem strukturalnym bez klucza). Internetowy interfejs API OData 5,3 obsługuje dziedziczenie typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="37697-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="37697-108">W tym temacie przedstawiono sposób tworzenia modelu Entity Data Model (EDM) przy użyciu złożonych typów dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="37697-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="37697-109">Aby uzyskać pełny kod źródłowy, zobacz [przykładowy dziedziczenie typu złożonego OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="37697-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="37697-110">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="37697-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="37697-111">Internetowy interfejs API OData 5,3</span><span class="sxs-lookup"><span data-stu-id="37697-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="37697-112">OData 4</span><span class="sxs-lookup"><span data-stu-id="37697-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="37697-113">Hierarchia modelu</span><span class="sxs-lookup"><span data-stu-id="37697-113">Model Hierarchy</span></span>

<span data-ttu-id="37697-114">Aby zilustrować dziedziczenie typu złożonego, użyjemy następującej hierarchii klas.</span><span class="sxs-lookup"><span data-stu-id="37697-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="37697-115">`Shape` jest abstrakcyjnym typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="37697-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="37697-116">`Rectangle`, `Triangle`i `Circle` są typami złożonymi pochodnymi `Shape`, a `RoundRectangle` pochodzą z `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="37697-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="37697-117">`Window` jest typem jednostki i zawiera wystąpienie `Shape`.</span><span class="sxs-lookup"><span data-stu-id="37697-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="37697-118">Oto klasy CLR, które definiują te typy.</span><span class="sxs-lookup"><span data-stu-id="37697-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="37697-119">Kompiluj model modelu EDM</span><span class="sxs-lookup"><span data-stu-id="37697-119">Build the EDM Model</span></span>

<span data-ttu-id="37697-120">Aby utworzyć modelu EDM, można użyć **ODataConventionModelBuilder**, który wnioskuje relacje dziedziczenia z typów CLR.</span><span class="sxs-lookup"><span data-stu-id="37697-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="37697-121">Możesz również jawnie skompilować modelu EDM przy użyciu **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="37697-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="37697-122">Zajmuje to więcej kodu, ale daje większą kontrolę nad modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="37697-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="37697-123">Te dwa przykłady tworzą ten sam schemat modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="37697-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="37697-124">Dokument metadanych</span><span class="sxs-lookup"><span data-stu-id="37697-124">Metadata Document</span></span>

<span data-ttu-id="37697-125">Oto dokument metadanych OData przedstawiający dziedziczenie typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="37697-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="37697-126">W dokumencie metadanych można zobaczyć, że:</span><span class="sxs-lookup"><span data-stu-id="37697-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="37697-127">Typ złożony `Shape` jest abstrakcyjny.</span><span class="sxs-lookup"><span data-stu-id="37697-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="37697-128">`Rectangle`, `Triangle`i `Circle` typ złożony mają `Shape`typ podstawowy.</span><span class="sxs-lookup"><span data-stu-id="37697-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="37697-129">Typ `RoundRectangle` ma `Rectangle`typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="37697-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="37697-130">Rzutowanie typów złożonych</span><span class="sxs-lookup"><span data-stu-id="37697-130">Casting Complex Types</span></span>

<span data-ttu-id="37697-131">Rzutowanie na typy złożone jest teraz obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="37697-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="37697-132">Na przykład następujące zapytanie rzutuje `Shape` na `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="37697-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="37697-133">Oto ładunek odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="37697-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
