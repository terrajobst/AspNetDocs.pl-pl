---
title: Pomocnik tagu pamięci podręcznej w programie ASP.NET Core rozproszonych
author: pkellner
description: Dowiedz się, jak używać Pomocnik tagu rozproszonej pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a5b33451a763c297c6d7885855a321c43435abb4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57071819"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="91486-103">Pomocnik tagu pamięci podręcznej w programie ASP.NET Core rozproszonych</span><span class="sxs-lookup"><span data-stu-id="91486-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="91486-104">Przez [Peter Kellner](http://peterkellner.net) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="91486-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="91486-105">Pomocnik tagu usługi rozproszonej pamięci podręcznej umożliwia znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core, buforując zawartość ze źródłem rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="91486-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="91486-106">Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="91486-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="91486-107">Pomocnik tagu rozproszonej pamięci podręcznej dziedziczy z klasy bazowej tego samego jako Pomocnik tagu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="91486-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="91486-108">Wszystkie [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) atrybuty są dostępne dla Pomocnik tagu rozproszonej.</span><span class="sxs-lookup"><span data-stu-id="91486-108">All of the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) attributes are available to the Distributed Tag Helper.</span></span>

<span data-ttu-id="91486-109">Pomocnik tagu rozproszonej pamięci podręcznej używa [iniekcji konstruktora](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span><span class="sxs-lookup"><span data-stu-id="91486-109">The Distributed Cache Tag Helper uses [constructor injection](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span></span> <span data-ttu-id="91486-110"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Interfejsu jest przekazywana do konstruktora rozproszonych Pomocnik tagu pamięci podręcznej firmy.</span><span class="sxs-lookup"><span data-stu-id="91486-110">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="91486-111">Jeśli nie konkretną implementację `IDistributedCache` jest tworzony w `Startup.ConfigureServices` (*Startup.cs*), Pomocnik tagu rozproszonej pamięci podręcznej przy użyciu tego samego dostawcy w pamięci dane w pamięci podręcznej jako [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="91486-111">If no concrete implementation of `IDistributedCache` is created in `Startup.ConfigureServices` (*Startup.cs*), the Distributed Cache Tag Helper uses the same in-memory provider for storing cached data as the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="91486-112">Rozproszone atrybutów Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="91486-112">Distributed Cache Tag Helper Attributes</span></span>

### <a name="attributes-shared-with-the-cache-tag-helper"></a><span data-ttu-id="91486-113">Atrybuty udostępnione Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="91486-113">Attributes shared with the Cache Tag Helper</span></span>

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

<span data-ttu-id="91486-114">Pomocnik tagu rozproszonej pamięci podręcznej dziedziczy taka sama klasa co Pomocnik tagu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="91486-114">The Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper.</span></span> <span data-ttu-id="91486-115">Aby uzyskać opis tych atrybutów, zobacz [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="91486-115">For descriptions of these attributes, see the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="name"></a><span data-ttu-id="91486-116">nazwa</span><span class="sxs-lookup"><span data-stu-id="91486-116">name</span></span>

| <span data-ttu-id="91486-117">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="91486-117">Attribute Type</span></span> | <span data-ttu-id="91486-118">Przykład</span><span class="sxs-lookup"><span data-stu-id="91486-118">Example</span></span>                               |
| -------------- | ------------------------------------- |
| <span data-ttu-id="91486-119">String</span><span class="sxs-lookup"><span data-stu-id="91486-119">String</span></span>         | `my-distributed-cache-unique-key-101` |

<span data-ttu-id="91486-120">`name` jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="91486-120">`name` is required.</span></span> <span data-ttu-id="91486-121">`name` Atrybut jest używany jako klucz dla każdego wystąpienia przechowywanych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="91486-121">The `name` attribute is used as a key for each stored cache instance.</span></span> <span data-ttu-id="91486-122">Inaczej niż w przypadku Pomocnik tagu pamięci podręcznej przypisuje klucz pamięci podręcznej do każdego wystąpienia, na podstawie nazwy strony Razor i lokalizację w stronę Razor, Pomocnik tagu rozproszonej pamięci podręcznej tylko określa jego klucza na na podstawie atrybutu `name`.</span><span class="sxs-lookup"><span data-stu-id="91486-122">Unlike the Cache Tag Helper that assigns a cache key to each instance based on the Razor page name and location in the Razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`.</span></span>

<span data-ttu-id="91486-123">Przykład:</span><span class="sxs-lookup"><span data-stu-id="91486-123">Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="91486-124">Rozproszone implementacje IDistributedCache Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="91486-124">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="91486-125">Istnieją dwie implementacje <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> wbudowanych w platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="91486-125">There are two implementations of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> built in to ASP.NET Core.</span></span> <span data-ttu-id="91486-126">Jeden opiera się na serwerze SQL Server, a drugi jest oparta na pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="91486-126">One is based on SQL Server, and the other is based on Redis.</span></span> <span data-ttu-id="91486-127">Szczegóły tych implementacji znajduje się w temacie <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="91486-127">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="91486-128">Obu implementacjach obejmują ustawienia wystąpienie `IDistributedCache` w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="91486-128">Both implementations involve setting an instance of `IDistributedCache` in `Startup`.</span></span>

<span data-ttu-id="91486-129">Istnieją żadne atrybuty znacznika, w szczególności związanych z użyciem określonych implementacji `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="91486-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="91486-130">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="91486-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
