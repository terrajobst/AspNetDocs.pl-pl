---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akcje i funkcje w protokole OData v4 przy użyciu ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: W przypadku protokołu OData akcje i funkcje są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach. W tym samouczku pokazano, jak to zrobić...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556225"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="f6a30-104">Akcje i funkcje w protokole OData v4 przy użyciu ASP.NET Web API 2,2</span><span class="sxs-lookup"><span data-stu-id="f6a30-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="f6a30-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f6a30-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f6a30-106">W przypadku protokołu OData akcje i funkcje są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach.</span><span class="sxs-lookup"><span data-stu-id="f6a30-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="f6a30-107">W tym samouczku pokazano, jak dodać akcje i funkcje do punktu końcowego protokołu OData v4 przy użyciu interfejsu Web API 2,2.</span><span class="sxs-lookup"><span data-stu-id="f6a30-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="f6a30-108">Samouczek kompiluje się w samouczku [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="f6a30-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f6a30-109">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="f6a30-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="f6a30-110">Internetowy interfejs API 2,2</span><span class="sxs-lookup"><span data-stu-id="f6a30-110">Web API 2.2</span></span>
> - <span data-ttu-id="f6a30-111">OData 4</span><span class="sxs-lookup"><span data-stu-id="f6a30-111">OData v4</span></span>
> - <span data-ttu-id="f6a30-112">Visual Studio 2013 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="f6a30-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="f6a30-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f6a30-113">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="f6a30-114">Wersje samouczka</span><span class="sxs-lookup"><span data-stu-id="f6a30-114">Tutorial versions</span></span>
>
> <span data-ttu-id="f6a30-115">W przypadku usługi OData w wersji 3 zobacz [Akcje OData w ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="f6a30-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="f6a30-116">Różnica między *akcjami* i *funkcjami* polega na tym, że akcje mogą mieć efekty uboczne, a funkcje nie.</span><span class="sxs-lookup"><span data-stu-id="f6a30-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="f6a30-117">Obie akcje i funkcje mogą zwracać dane.</span><span class="sxs-lookup"><span data-stu-id="f6a30-117">Both actions and functions can return data.</span></span> <span data-ttu-id="f6a30-118">Niektóre zastosowania do akcji obejmują:</span><span class="sxs-lookup"><span data-stu-id="f6a30-118">Some uses for actions include:</span></span>

- <span data-ttu-id="f6a30-119">Złożone transakcje.</span><span class="sxs-lookup"><span data-stu-id="f6a30-119">Complex transactions.</span></span>
- <span data-ttu-id="f6a30-120">Jednoczesne manipulowanie kilkoma jednostkami.</span><span class="sxs-lookup"><span data-stu-id="f6a30-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="f6a30-121">Zezwalanie na aktualizacje tylko do określonych właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="f6a30-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="f6a30-122">Wysyłanie danych, które nie są jednostką.</span><span class="sxs-lookup"><span data-stu-id="f6a30-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="f6a30-123">Funkcje są przydatne do zwracania informacji, które nie są bezpośrednio powiązane z jednostką lub kolekcją.</span><span class="sxs-lookup"><span data-stu-id="f6a30-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="f6a30-124">Akcja (lub funkcja) może być celem pojedynczej jednostki lub kolekcji.</span><span class="sxs-lookup"><span data-stu-id="f6a30-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="f6a30-125">W terminologii OData jest to *powiązanie*.</span><span class="sxs-lookup"><span data-stu-id="f6a30-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="f6a30-126">Możesz również mieć &quot;niepowiązane&quot; akcje/funkcje, które są wywoływane jako operacje statyczne w usłudze.</span><span class="sxs-lookup"><span data-stu-id="f6a30-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="f6a30-127">Przykład: Dodawanie akcji</span><span class="sxs-lookup"><span data-stu-id="f6a30-127">Example: Adding an Action</span></span>

<span data-ttu-id="f6a30-128">Zdefiniujmy akcję do oceniania produktu.</span><span class="sxs-lookup"><span data-stu-id="f6a30-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a30-129">Ten samouczek kompiluje się w samouczku [Tworzenie punktu końcowego OData v4 przy użyciu interfejsu ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="f6a30-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="f6a30-130">Najpierw Dodaj model `ProductRating`, aby reprezentować klasyfikacje.</span><span class="sxs-lookup"><span data-stu-id="f6a30-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="f6a30-131">Dodaj także **nieogólnymi** do klasy `ProductsContext`, tak aby EF utworzy tabelę klasyfikacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f6a30-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="f6a30-132">Dodawanie akcji do modelu EDM</span><span class="sxs-lookup"><span data-stu-id="f6a30-132">Add the Action to the EDM</span></span>

<span data-ttu-id="f6a30-133">W WebApiConfig.cs Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="f6a30-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="f6a30-134">Metoda **EntityTypeConfiguration. Action** dodaje akcję do modelu Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="f6a30-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="f6a30-135">Metoda **parametru** określa typ parametru dla akcji.</span><span class="sxs-lookup"><span data-stu-id="f6a30-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="f6a30-136">Ten kod ustawia również przestrzeń nazw dla modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="f6a30-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="f6a30-137">Przestrzeń nazw ma znaczenie, ponieważ identyfikator URI akcji obejmuje w pełni kwalifikowaną nazwę akcji:</span><span class="sxs-lookup"><span data-stu-id="f6a30-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="f6a30-138">W typowej konfiguracji usług IIS, kropka w tym adresie URL spowoduje zwrócenie błędu 404.</span><span class="sxs-lookup"><span data-stu-id="f6a30-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="f6a30-139">Aby rozwiązać ten problem, Dodaj następującą sekcję do pliku Web. config:</span><span class="sxs-lookup"><span data-stu-id="f6a30-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="f6a30-140">Dodaj metodę kontrolera dla akcji</span><span class="sxs-lookup"><span data-stu-id="f6a30-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="f6a30-141">Aby włączyć akcję &quot;rate&quot;, Dodaj następującą metodę do `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="f6a30-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="f6a30-142">Zwróć uwagę, że nazwa metody jest zgodna z nazwą akcji.</span><span class="sxs-lookup"><span data-stu-id="f6a30-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="f6a30-143">Atrybut **[HTTPPOST]** określa, że metoda jest metodą POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6a30-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="f6a30-144">Aby wywołać akcję, klient wysyła żądanie HTTP POST w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f6a30-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="f6a30-145">Akcja &quot;rate&quot; jest powiązana z wystąpieniami produktu, więc identyfikator URI dla akcji to w pełni kwalifikowana nazwa działania dołączona do identyfikatora URI jednostki.</span><span class="sxs-lookup"><span data-stu-id="f6a30-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="f6a30-146">(Odwołaj, że ustawimy przestrzeń nazw EDM na &quot;ProductService&quot;, więc w pełni kwalifikowana nazwa akcji to &quot;ProductService. rate&quot;).</span><span class="sxs-lookup"><span data-stu-id="f6a30-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="f6a30-147">Treść żądania zawiera parametry akcji jako ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="f6a30-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="f6a30-148">Interfejs API sieci Web automatycznie konwertuje ładunek JSON na obiekt **ODataActionParameters** , który jest tylko słownikiem wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="f6a30-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="f6a30-149">Użyj tego słownika, aby uzyskać dostęp do parametrów w metodzie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f6a30-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="f6a30-150">Jeśli klient wysyła parametry akcji w niewłaściwym formacie, wartość **ModelState. IsValid** jest równa false.</span><span class="sxs-lookup"><span data-stu-id="f6a30-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="f6a30-151">Zaznacz tę flagę w metodzie kontrolera i zwróć błąd, jeśli **IsValid** ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="f6a30-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="f6a30-152">Przykład: Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="f6a30-152">Example: Adding a Function</span></span>

<span data-ttu-id="f6a30-153">Teraz Dodajmy funkcję OData, która zwraca najbardziej kosztowny produkt.</span><span class="sxs-lookup"><span data-stu-id="f6a30-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="f6a30-154">Jak poprzednio, pierwszym krokiem jest dodanie funkcji do modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="f6a30-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="f6a30-155">W WebApiConfig.cs Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="f6a30-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="f6a30-156">W takim przypadku funkcja jest powiązana z kolekcją produkty, a nie z wystąpieniami poszczególnych produktów.</span><span class="sxs-lookup"><span data-stu-id="f6a30-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="f6a30-157">Klienci wywołują funkcję, wysyłając żądanie GET:</span><span class="sxs-lookup"><span data-stu-id="f6a30-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="f6a30-158">Oto Metoda kontrolera dla tej funkcji:</span><span class="sxs-lookup"><span data-stu-id="f6a30-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="f6a30-159">Zwróć uwagę, że nazwa metody jest zgodna z nazwą funkcji.</span><span class="sxs-lookup"><span data-stu-id="f6a30-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="f6a30-160">Atrybut **[narzędzia HttpGet]** określa, że metoda jest metodą HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f6a30-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="f6a30-161">Oto odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="f6a30-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="f6a30-162">Przykład: Dodawanie funkcji niepowiązanej</span><span class="sxs-lookup"><span data-stu-id="f6a30-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="f6a30-163">Poprzedni przykład to funkcja powiązana z kolekcją.</span><span class="sxs-lookup"><span data-stu-id="f6a30-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="f6a30-164">W tym następnym przykładzie utworzymy *niepowiązaną* funkcję.</span><span class="sxs-lookup"><span data-stu-id="f6a30-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="f6a30-165">Niepowiązane funkcje są wywoływane jako operacje statyczne w usłudze.</span><span class="sxs-lookup"><span data-stu-id="f6a30-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="f6a30-166">Funkcja w tym przykładzie zwróci podatek sprzedaży dla danego kodu pocztowego.</span><span class="sxs-lookup"><span data-stu-id="f6a30-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="f6a30-167">W pliku WebApiConfig Dodaj funkcję do modelu EDM:</span><span class="sxs-lookup"><span data-stu-id="f6a30-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="f6a30-168">Zwróć uwagę, że wywołujemy **funkcję** bezpośrednio w **ODataModelBuilder**, a nie na typie lub kolekcji jednostek.</span><span class="sxs-lookup"><span data-stu-id="f6a30-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="f6a30-169">Oznacza to, że funkcja jest niezwiązana z konstruktorem modelu.</span><span class="sxs-lookup"><span data-stu-id="f6a30-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="f6a30-170">Oto Metoda kontrolera implementująca funkcję:</span><span class="sxs-lookup"><span data-stu-id="f6a30-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="f6a30-171">Nie ma znaczenia kontrolera interfejsu API sieci Web, w którym jest umieszczana ta metoda.</span><span class="sxs-lookup"><span data-stu-id="f6a30-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="f6a30-172">Można ją umieścić w `ProductsController`lub zdefiniować oddzielny kontroler.</span><span class="sxs-lookup"><span data-stu-id="f6a30-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="f6a30-173">Atrybut **[ODataRoute]** definiuje szablon identyfikatora URI dla funkcji.</span><span class="sxs-lookup"><span data-stu-id="f6a30-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="f6a30-174">Oto przykładowe żądanie klienta:</span><span class="sxs-lookup"><span data-stu-id="f6a30-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="f6a30-175">Odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="f6a30-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
