---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Zawieranie w protokole OData v4 przy użyciu interfejsu Web API 2,2 | Microsoft Docs
author: rick-anderson
description: Tradycyjnie można uzyskać dostęp do jednostki tylko wtedy, gdy została ona Zahermetyzowana wewnątrz zestawu jednostek. Jednak usługa OData v4 udostępnia dwie dodatkowe opcje, pojedyncze i con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525124"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="e0c0e-104">Zawieranie w protokole OData v4 przy użyciu interfejsu Web API 2,2</span><span class="sxs-lookup"><span data-stu-id="e0c0e-104">Containment in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="e0c0e-105">według Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="e0c0e-105">by Jinfu Tan</span></span>

> <span data-ttu-id="e0c0e-106">Tradycyjnie można uzyskać dostęp do jednostki tylko wtedy, gdy została ona Zahermetyzowana wewnątrz zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="e0c0e-107">Jednak usługa OData v4 oferuje dwie dodatkowe opcje, pojedyncze i zawierające, z których każdy obsługuje WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="e0c0e-108">W tym temacie pokazano, jak zdefiniować zawieranie w punkcie końcowym OData w WebApi 2,2.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="e0c0e-109">Aby uzyskać więcej informacji na temat zawierania, zobacz artykuł [zawieranie jest dostarczane z użyciem protokołu OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0c0e-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="e0c0e-110">Aby utworzyć punkt końcowy usługi OData v4 w interfejsie API sieci Web, zobacz [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e0c0e-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="e0c0e-111">Najpierw utworzymy model domeny zawierania w usłudze OData przy użyciu tego modelu danych:</span><span class="sxs-lookup"><span data-stu-id="e0c0e-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Model danych](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="e0c0e-113">Konto zawiera wiele PaymentInstruments (PI), ale nie definiujemy zestawu jednostek dla PI.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="e0c0e-114">Zamiast tego PIs można uzyskać dostęp tylko za pomocą konta.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="e0c0e-115">Rozwiązanie używane w tym temacie można pobrać z [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="e0c0e-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="e0c0e-116">Definiowanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="e0c0e-116">Defining the data model</span></span>

1. <span data-ttu-id="e0c0e-117">Zdefiniuj typy CLR.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="e0c0e-118">Atrybut `Contained` jest używany dla właściwości nawigacji zawierania.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="e0c0e-119">Generuj model modelu EDM na podstawie typów CLR.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="e0c0e-120">`ODataConventionModelBuilder` będzie obsługiwał Kompilowanie modelu EDM, jeśli atrybut `Contained` zostanie dodany do odpowiedniej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="e0c0e-121">Jeśli właściwość jest typem kolekcji, zostanie również utworzona funkcja `GetCount(string NameContains)`.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="e0c0e-122">Wygenerowane metadane będą wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="e0c0e-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="e0c0e-123">Atrybut `ContainsTarget` wskazuje, że właściwość nawigacji jest zawiera.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="e0c0e-124">Zdefiniuj kontroler zawierający zestaw jednostek</span><span class="sxs-lookup"><span data-stu-id="e0c0e-124">Define the containing entity set controller</span></span>

<span data-ttu-id="e0c0e-125">Zawarte jednostki nie mają własnego kontrolera; Akcja jest zdefiniowana w kontrolerze zawierającym zestaw jednostek.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="e0c0e-126">W tym przykładzie istnieje AccountsController, ale nie PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="e0c0e-127">Jeśli ścieżka OData ma 4 lub więcej segmentów, tylko Routing atrybutów działa tak, jak `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` na powyższym kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="e0c0e-128">W przeciwnym razie zarówno atrybut, jak i konwencjonalny Routing działa: na przykład, `GetPayInPIs(int key)` dopasowuje `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="e0c0e-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="e0c0e-129">*Dzięki Leo hu w odniesieniu do oryginalnej zawartości tego artykułu.*</span><span class="sxs-lookup"><span data-stu-id="e0c0e-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
