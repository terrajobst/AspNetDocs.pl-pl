---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Tworzenie pojedynczych w protokole OData v4 przy użyciu interfejsu Web API 2,2 | Microsoft Docs
author: rick-anderson
description: W tym temacie przedstawiono sposób definiowania pojedynczego elementu w punkcie końcowym OData w interfejsie Web API 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622088"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="9a31d-103">Tworzenie pojedynczego elementu w protokole OData v4 przy użyciu interfejsu Web API 2,2</span><span class="sxs-lookup"><span data-stu-id="9a31d-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="9a31d-104">Autor Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="9a31d-104">by Zoe Luo</span></span>

> <span data-ttu-id="9a31d-105">Tradycyjnie można uzyskać dostęp do jednostki tylko wtedy, gdy została ona Zahermetyzowana wewnątrz zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="9a31d-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="9a31d-106">Jednak usługa OData v4 oferuje dwie dodatkowe opcje, pojedyncze i zawierające, z których każdy obsługuje WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="9a31d-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="9a31d-107">W tym artykule przedstawiono sposób definiowania pojedynczego elementu w punkcie końcowym OData w interfejsie Web API 2,2.</span><span class="sxs-lookup"><span data-stu-id="9a31d-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="9a31d-108">Aby uzyskać informacje na temat tego, co to jest pojedynczy i jak można korzystać z niego, zobacz [Używanie pojedynczych do definiowania podmiotu specjalnego](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a31d-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="9a31d-109">Aby utworzyć punkt końcowy usługi OData v4 w interfejsie API sieci Web, zobacz [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="9a31d-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="9a31d-110">Utworzymy singleton w projekcie interfejsu API sieci Web przy użyciu następującego modelu danych:</span><span class="sxs-lookup"><span data-stu-id="9a31d-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![model danych](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="9a31d-112">Pojedyncze nazwane `Umbrella` zostanie zdefiniowane na podstawie typu `Company`, a zestaw jednostek o nazwie `Employees` zostanie zdefiniowany w oparciu o typ `Employee`.</span><span class="sxs-lookup"><span data-stu-id="9a31d-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="9a31d-113">Rozwiązanie używane w tym samouczku można pobrać z [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="9a31d-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="9a31d-114">Zdefiniowanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="9a31d-114">Define the data model</span></span>

1. <span data-ttu-id="9a31d-115">Zdefiniuj typy CLR.</span><span class="sxs-lookup"><span data-stu-id="9a31d-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="9a31d-116">Generuj model modelu EDM na podstawie typów CLR.</span><span class="sxs-lookup"><span data-stu-id="9a31d-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="9a31d-117">W tym miejscu `builder.Singleton<Company>("Umbrella")` nakazuje konstruktorowi modelu tworzenie pojedynczych nazwanych `Umbrella` w modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="9a31d-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="9a31d-118">Wygenerowane metadane będą wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="9a31d-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="9a31d-119">Z metadanych możemy zobaczyć, że właściwość nawigacji `Company` w zestawie jednostek `Employees` jest powiązana z `Umbrella`pojedynczym.</span><span class="sxs-lookup"><span data-stu-id="9a31d-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="9a31d-120">Powiązanie jest wykonywane automatycznie przez `ODataConventionModelBuilder`, ponieważ tylko `Umbrella` ma typ `Company`.</span><span class="sxs-lookup"><span data-stu-id="9a31d-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="9a31d-121">Jeśli w modelu występuje jakakolwiek niejednoznaczność, można użyć `HasSingletonBinding`, aby jawnie powiązać właściwość nawigacji z pojedynczą; `HasSingletonBinding` ma ten sam efekt, co użycie atrybutu `Singleton` w definicji typu CLR:</span><span class="sxs-lookup"><span data-stu-id="9a31d-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="9a31d-122">Zdefiniuj kontroler singleton</span><span class="sxs-lookup"><span data-stu-id="9a31d-122">Define the singleton controller</span></span>

<span data-ttu-id="9a31d-123">Podobnie jak kontroler obiektów EntitySet, kontroler singleton dziedziczy po `ODataController`, a nazwa kontrolera singleton powinna być `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="9a31d-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="9a31d-124">W celu obsługi różnych rodzajów żądań akcje muszą być wstępnie zdefiniowane w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="9a31d-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="9a31d-125">**Routing atrybutu** jest domyślnie włączony w WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="9a31d-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="9a31d-126">Na przykład, aby zdefiniować akcję do obsługi zapytań `Revenue` z `Company` przy użyciu routingu atrybutów, należy wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9a31d-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="9a31d-127">Jeśli nie chcesz definiować atrybutów dla każdej akcji, po prostu Zdefiniuj swoje działania zgodnie z [konwencjami routingu OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="9a31d-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="9a31d-128">Ponieważ klucz nie jest wymagany do wykonywania zapytań dotyczących pojedynczych, akcje zdefiniowane w kontrolerze pojedynczym różnią się nieco od akcji zdefiniowanych w kontrolerze obiektu EntitySet.</span><span class="sxs-lookup"><span data-stu-id="9a31d-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="9a31d-129">W przypadku odwołania, sygnatury metod dla każdej definicji akcji w kontrolerze singleton są wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="9a31d-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="9a31d-130">Zasadniczo należy to zrobić po stronie usługi.</span><span class="sxs-lookup"><span data-stu-id="9a31d-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="9a31d-131">[Przykładowy projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) zawiera cały kod rozwiązania i klienta OData, który pokazuje, jak używać pojedynczej.</span><span class="sxs-lookup"><span data-stu-id="9a31d-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="9a31d-132">Klient został utworzony przez wykonanie kroków opisanych w temacie [Tworzenie aplikacji klienckiej OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="9a31d-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="9a31d-133">.</span><span class="sxs-lookup"><span data-stu-id="9a31d-133">.</span></span> 

<span data-ttu-id="9a31d-134">*Dzięki Leo hu w odniesieniu do oryginalnej zawartości tego artykułu.*</span><span class="sxs-lookup"><span data-stu-id="9a31d-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
