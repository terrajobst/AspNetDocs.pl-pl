---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Wskazówki dotyczące zabezpieczeń dla ASP.NET Web API 2 OData-ASP.NET 4. x
author: MikeWasson
description: Opisuje problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas udostępniania zestawu danych za pomocą protokołu OData dla ASP.NET Web API 2 w ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556498"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="c1e48-103">Wskazówki dotyczące zabezpieczeń dla usługi ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="c1e48-103">Security Guidance for ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="c1e48-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c1e48-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c1e48-105">W tym temacie opisano niektóre problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas udostępniania zestawu danych za pomocą protokołu OData dla ASP.NET Web API 2 w ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="c1e48-105">This topic describes some of the security issues that you should consider when exposing a dataset through OData for ASP.NET Web API 2 on ASP.NET 4.x.</span></span>

## <a name="edm-security"></a><span data-ttu-id="c1e48-106">Zabezpieczenia modelu EDM</span><span class="sxs-lookup"><span data-stu-id="c1e48-106">EDM Security</span></span>

<span data-ttu-id="c1e48-107">Semantyka zapytań jest oparta na modelu Entity Data Model (EDM), a nie na źródłowych typach modeli.</span><span class="sxs-lookup"><span data-stu-id="c1e48-107">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="c1e48-108">Można wykluczyć właściwość z modelu EDM i nie będzie ona widoczna dla zapytania.</span><span class="sxs-lookup"><span data-stu-id="c1e48-108">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="c1e48-109">Załóżmy na przykład, że model zawiera typ pracownika z właściwością wynagrodzeń.</span><span class="sxs-lookup"><span data-stu-id="c1e48-109">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="c1e48-110">Możesz chcieć wykluczyć tę właściwość z modelu EDM, aby ukryć ją od klientów.</span><span class="sxs-lookup"><span data-stu-id="c1e48-110">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="c1e48-111">Istnieją dwa sposoby wykluczenia właściwości z modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="c1e48-111">There are two ways to exclude a property from the EDM.</span></span> <span data-ttu-id="c1e48-112">Można ustawić atrybut **[IgnoreDataMember]** dla właściwości w klasie modelu:</span><span class="sxs-lookup"><span data-stu-id="c1e48-112">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="c1e48-113">Możesz również usunąć właściwość z modelu EDM programowo:</span><span class="sxs-lookup"><span data-stu-id="c1e48-113">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="c1e48-114">Zabezpieczenia zapytań</span><span class="sxs-lookup"><span data-stu-id="c1e48-114">Query Security</span></span>

<span data-ttu-id="c1e48-115">Złośliwy lub algorytmie klient może utworzyć zapytanie, które zajmuje bardzo dużo czasu na wykonanie.</span><span class="sxs-lookup"><span data-stu-id="c1e48-115">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="c1e48-116">W najgorszym przypadku może to przerwać dostęp do usługi.</span><span class="sxs-lookup"><span data-stu-id="c1e48-116">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="c1e48-117">Atrybut **[Queryable]** jest filtrem akcji, który analizuje, weryfikuje i stosuje zapytanie.</span><span class="sxs-lookup"><span data-stu-id="c1e48-117">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="c1e48-118">Filtr konwertuje opcje zapytania na wyrażenie LINQ.</span><span class="sxs-lookup"><span data-stu-id="c1e48-118">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="c1e48-119">Gdy kontroler OData zwraca typ **IQueryable** , dostawca **IQueryable** LINQ konwertuje wyrażenie LINQ na zapytanie.</span><span class="sxs-lookup"><span data-stu-id="c1e48-119">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="c1e48-120">W związku z tym wydajność jest zależna od dostawcy LINQ, który jest używany, a także dla określonych cech zestawu danych lub schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c1e48-120">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="c1e48-121">Aby uzyskać więcej informacji na temat korzystania z opcji zapytania OData w interfejsie Web API ASP.NET, zobacz [Obsługa opcji zapytania OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="c1e48-121">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="c1e48-122">Jeśli wiesz, że wszyscy klienci są zaufani (na przykład w środowisku przedsiębiorstwa) lub jeśli zestaw danych jest mały, wydajność zapytań może nie być problemem.</span><span class="sxs-lookup"><span data-stu-id="c1e48-122">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="c1e48-123">W przeciwnym razie należy wziąć pod uwagę następujące zalecenia.</span><span class="sxs-lookup"><span data-stu-id="c1e48-123">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="c1e48-124">Przetestuj swoją usługę przy użyciu różnych zapytań i Profiluj bazę danych.</span><span class="sxs-lookup"><span data-stu-id="c1e48-124">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="c1e48-125">Włącz stronicowanie oparte na serwerze, aby uniknąć powrotu dużego zestawu danych w jednym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="c1e48-125">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="c1e48-126">Aby uzyskać więcej informacji, zobacz [stronicowanie oparte na serwerze](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="c1e48-126">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="c1e48-127">Potrzebujesz $filter i $orderby?</span><span class="sxs-lookup"><span data-stu-id="c1e48-127">Do you need $filter and $orderby?</span></span> <span data-ttu-id="c1e48-128">Niektóre aplikacje mogą zezwalać na stronicowanie klienta przy użyciu $top i $skip, ale wyłączyć inne opcje zapytania.</span><span class="sxs-lookup"><span data-stu-id="c1e48-128">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="c1e48-129">Rozważ ograniczenie $orderby do właściwości w indeksie klastrowanym.</span><span class="sxs-lookup"><span data-stu-id="c1e48-129">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="c1e48-130">Sortowanie dużych danych bez indeksu klastrowanego jest powolne.</span><span class="sxs-lookup"><span data-stu-id="c1e48-130">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="c1e48-131">Maksymalna liczba węzłów: Właściwość **MaxNodeCount** na **[Queryable]** ustawia maksymalną liczbę węzłów dozwoloną w drzewie składni $Filter.</span><span class="sxs-lookup"><span data-stu-id="c1e48-131">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="c1e48-132">Wartość domyślna to 100, ale można ustawić niższą wartość, ponieważ może być konieczne spowolnienie kompilowania dużej liczby węzłów.</span><span class="sxs-lookup"><span data-stu-id="c1e48-132">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="c1e48-133">Jest to szczególnie istotne, jeśli używasz LINQ to Objects (tj., zapytania LINQ w kolekcji w pamięci, bez użycia pośredniego dostawcy LINQ).</span><span class="sxs-lookup"><span data-stu-id="c1e48-133">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="c1e48-134">Rozważ wyłączenie dowolnej funkcji () i wszystkich (), ponieważ mogą one być wolne.</span><span class="sxs-lookup"><span data-stu-id="c1e48-134">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="c1e48-135">Jeśli dowolne właściwości ciągu zawierają duże ciągi&#8212;na przykład, opis produktu lub wpis&#8212;w blogu rozważ wyłączenie funkcji ciągu.</span><span class="sxs-lookup"><span data-stu-id="c1e48-135">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="c1e48-136">Należy rozważyć niezezwalanie na filtrowanie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c1e48-136">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="c1e48-137">Filtrowanie właściwości nawigacji może skutkować przyłączeniem, co może potrwać, w zależności od schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c1e48-137">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="c1e48-138">Poniższy kod przedstawia moduł sprawdzania poprawności zapytań, który uniemożliwia filtrowanie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c1e48-138">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="c1e48-139">Aby uzyskać więcej informacji na temat modułów sprawdzania poprawności zapytań, zobacz [Walidacja zapytań](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="c1e48-139">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="c1e48-140">Rozważ ograniczenie $filter zapytań, pisząc moduł sprawdzania poprawności, który jest dostosowany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c1e48-140">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="c1e48-141">Rozważmy na przykład te dwa zapytania:</span><span class="sxs-lookup"><span data-stu-id="c1e48-141">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="c1e48-142">Wszystkie filmy z aktorami, których nazwisko zaczyna się od "A".</span><span class="sxs-lookup"><span data-stu-id="c1e48-142">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="c1e48-143">Wszystkie filmy wydane w 1994.</span><span class="sxs-lookup"><span data-stu-id="c1e48-143">All movies released in 1994.</span></span>

    <span data-ttu-id="c1e48-144">Jeśli nie są indeksowane przez aktorów, pierwsze zapytanie może wymagać aparatu bazy danych do skanowania całej listy filmów.</span><span class="sxs-lookup"><span data-stu-id="c1e48-144">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="c1e48-145">Drugie zapytanie może być akceptowalne, przy założeniu, że filmy są indeksowane przez rok wydania.</span><span class="sxs-lookup"><span data-stu-id="c1e48-145">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="c1e48-146">Poniższy kod przedstawia moduł sprawdzania poprawności, który umożliwia filtrowanie właściwości "ReleaseYear" i "title", ale nie innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="c1e48-146">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="c1e48-147">Ogólnie rzecz biorąc, należy rozważyć, które funkcje $filter są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="c1e48-147">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="c1e48-148">Jeśli klienci nie potrzebują pełnego wyrazistości $filter, można ograniczyć dozwolone funkcje.</span><span class="sxs-lookup"><span data-stu-id="c1e48-148">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
