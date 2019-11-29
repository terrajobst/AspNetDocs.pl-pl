---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Wywoływanie usługi OData z klienta platformy .NET (C#) | Microsoft Docs
author: MikeWasson
description: W tym samouczku pokazano, jak wywołać usługę OData z C# aplikacji klienckiej. Wersje oprogramowania używane w samouczku Visual Studio 2013 (działa z Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600381"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="15ed2-104">Wywoływanie usługi protokołu OData z klienta .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="15ed2-104">Calling an OData Service From a .NET Client (C#)</span></span>

<span data-ttu-id="15ed2-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="15ed2-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="15ed2-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="15ed2-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="15ed2-107">W tym samouczku pokazano, jak wywołać usługę OData z C# aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="15ed2-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="15ed2-108">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="15ed2-108">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="15ed2-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (współdziała z programem Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="15ed2-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="15ed2-110">Biblioteka klienta usług danych WCF</span><span class="sxs-lookup"><span data-stu-id="15ed2-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="15ed2-111">Web API 2.</span><span class="sxs-lookup"><span data-stu-id="15ed2-111">Web API 2.</span></span> <span data-ttu-id="15ed2-112">(Przykładowa usługa OData jest tworzona przy użyciu interfejsu Web API 2, ale aplikacja kliencka nie jest zależna od interfejsu API sieci Web).</span><span class="sxs-lookup"><span data-stu-id="15ed2-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>

<span data-ttu-id="15ed2-113">W tym samouczku omówiono tworzenie aplikacji klienckiej, która wywołuje usługę OData.</span><span class="sxs-lookup"><span data-stu-id="15ed2-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="15ed2-114">Usługa OData udostępnia następujące jednostki:</span><span class="sxs-lookup"><span data-stu-id="15ed2-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="15ed2-115">W poniższych artykułach opisano sposób implementacji usługi OData w interfejsie Web API.</span><span class="sxs-lookup"><span data-stu-id="15ed2-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="15ed2-116">(Jednak nie musisz czytać ich, aby poznać ten samouczek).</span><span class="sxs-lookup"><span data-stu-id="15ed2-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="15ed2-117">Tworzenie punktu końcowego OData w interfejsie Web API 2</span><span class="sxs-lookup"><span data-stu-id="15ed2-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="15ed2-118">Relacje jednostek OData w interfejsie Web API 2</span><span class="sxs-lookup"><span data-stu-id="15ed2-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="15ed2-119">Akcje protokołu OData w interfejsie Web API 2</span><span class="sxs-lookup"><span data-stu-id="15ed2-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="15ed2-120">Generuj serwer proxy usługi</span><span class="sxs-lookup"><span data-stu-id="15ed2-120">Generate the Service Proxy</span></span>

<span data-ttu-id="15ed2-121">Pierwszym krokiem jest wygenerowanie serwera proxy usługi.</span><span class="sxs-lookup"><span data-stu-id="15ed2-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="15ed2-122">Serwer proxy usługi jest klasą .NET, która definiuje metody uzyskiwania dostępu do usługi OData.</span><span class="sxs-lookup"><span data-stu-id="15ed2-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="15ed2-123">Serwer proxy tłumaczy wywołania metod na żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="15ed2-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="15ed2-124">Zacznij od otwarcia projektu usługi OData w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15ed2-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="15ed2-125">Naciśnij kombinację klawiszy CTRL + F5, aby uruchomić usługę lokalnie w IIS Express.</span><span class="sxs-lookup"><span data-stu-id="15ed2-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="15ed2-126">Zanotuj adres lokalny, włącznie z numerem portu przypisywanym przez program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15ed2-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="15ed2-127">Ten adres będzie potrzebny podczas tworzenia serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="15ed2-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="15ed2-128">Następnie otwórz inne wystąpienie programu Visual Studio i Utwórz projekt aplikacji konsolowej.</span><span class="sxs-lookup"><span data-stu-id="15ed2-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="15ed2-129">Aplikacja konsolowa będzie naszą aplikacją kliencką OData.</span><span class="sxs-lookup"><span data-stu-id="15ed2-129">The console application will be our OData client application.</span></span> <span data-ttu-id="15ed2-130">(Można również dodać projekt do tego samego rozwiązania, co usługa).</span><span class="sxs-lookup"><span data-stu-id="15ed2-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="15ed2-131">Pozostałe kroki odwołują się do projektu konsoli.</span><span class="sxs-lookup"><span data-stu-id="15ed2-131">The remaining steps refer the console project.</span></span>

<span data-ttu-id="15ed2-132">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy pozycję **odwołania** i wybierz pozycję **Dodaj odwołanie do usługi**.</span><span class="sxs-lookup"><span data-stu-id="15ed2-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="15ed2-133">W oknie dialogowym **Dodaj odwołanie do usługi** wpisz adres usługi OData:</span><span class="sxs-lookup"><span data-stu-id="15ed2-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="15ed2-134">gdzie *port* jest numerem portu.</span><span class="sxs-lookup"><span data-stu-id="15ed2-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="15ed2-135">W **obszarze Przestrzeń nazw**wpisz "ProductService".</span><span class="sxs-lookup"><span data-stu-id="15ed2-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="15ed2-136">Ta opcja definiuje przestrzeń nazw klasy proxy.</span><span class="sxs-lookup"><span data-stu-id="15ed2-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="15ed2-137">Kliknij pozycję **Przejdź**.</span><span class="sxs-lookup"><span data-stu-id="15ed2-137">Click **Go**.</span></span> <span data-ttu-id="15ed2-138">Program Visual Studio odczytuje dokument metadanych OData w celu odnalezienia jednostek w usłudze.</span><span class="sxs-lookup"><span data-stu-id="15ed2-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="15ed2-139">Kliknij przycisk **OK** , aby dodać klasę proxy do projektu.</span><span class="sxs-lookup"><span data-stu-id="15ed2-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="15ed2-140">Utwórz wystąpienie klasy serwera proxy usługi</span><span class="sxs-lookup"><span data-stu-id="15ed2-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="15ed2-141">Wewnątrz metody `Main` Utwórz nowe wystąpienie klasy proxy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="15ed2-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="15ed2-142">Ponownie Użyj rzeczywistego numeru portu, na którym działa usługa.</span><span class="sxs-lookup"><span data-stu-id="15ed2-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="15ed2-143">Po wdrożeniu usługi będziesz używać identyfikatora URI usługi Live.</span><span class="sxs-lookup"><span data-stu-id="15ed2-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="15ed2-144">Nie musisz aktualizować serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="15ed2-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="15ed2-145">Poniższy kod dodaje procedurę obsługi zdarzeń, która drukuje identyfikatory URI żądania do okna konsoli.</span><span class="sxs-lookup"><span data-stu-id="15ed2-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="15ed2-146">Ten krok nie jest wymagany, ale jest interesujący do wyświetlania identyfikatorów URI dla każdego zapytania.</span><span class="sxs-lookup"><span data-stu-id="15ed2-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="15ed2-147">Wysyłanie zapytań do usługi</span><span class="sxs-lookup"><span data-stu-id="15ed2-147">Query the Service</span></span>

<span data-ttu-id="15ed2-148">Poniższy kod pobiera listę produktów z usługi OData.</span><span class="sxs-lookup"><span data-stu-id="15ed2-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="15ed2-149">Zwróć uwagę, że nie musisz pisać żadnego kodu w celu wysłania żądania HTTP lub przeanalizowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="15ed2-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="15ed2-150">Klasa proxy robi to automatycznie podczas wyliczania kolekcji `Container.Products` w pętli **foreach** .</span><span class="sxs-lookup"><span data-stu-id="15ed2-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="15ed2-151">Po uruchomieniu aplikacji dane wyjściowe powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="15ed2-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="15ed2-152">Aby uzyskać jednostkę według identyfikatora, należy użyć klauzuli `where`.</span><span class="sxs-lookup"><span data-stu-id="15ed2-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="15ed2-153">W pozostałej części tego tematu nie będzie wyświetlana cała funkcja `Main`, tylko kod wymagany do wywołania usługi.</span><span class="sxs-lookup"><span data-stu-id="15ed2-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="15ed2-154">Zastosuj opcje zapytania</span><span class="sxs-lookup"><span data-stu-id="15ed2-154">Apply Query Options</span></span>

<span data-ttu-id="15ed2-155">Funkcja OData definiuje [Opcje zapytania](../supporting-odata-query-options.md) , których można użyć do filtrowania, sortowania, stronicowania danych i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="15ed2-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="15ed2-156">W przypadku serwera proxy usługi można zastosować te opcje przy użyciu różnych wyrażeń LINQ.</span><span class="sxs-lookup"><span data-stu-id="15ed2-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="15ed2-157">W tej sekcji pokażę krótkie przykłady.</span><span class="sxs-lookup"><span data-stu-id="15ed2-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="15ed2-158">Aby uzyskać więcej informacji, zobacz temat [uwagi dotyczące LINQ (usługi danych programu WCF)](https://msdn.microsoft.com/library/ee622463.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="15ed2-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="15ed2-159">Filtrowanie ($filter)</span><span class="sxs-lookup"><span data-stu-id="15ed2-159">Filtering ($filter)</span></span>

<span data-ttu-id="15ed2-160">Aby przefiltrować, użyj klauzuli `where`.</span><span class="sxs-lookup"><span data-stu-id="15ed2-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="15ed2-161">Poniższe przykładowe filtry według kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="15ed2-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="15ed2-162">Ten kod odnosi się do poniższego zapytania OData.</span><span class="sxs-lookup"><span data-stu-id="15ed2-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="15ed2-163">Zwróć uwagę, że serwer proxy konwertuje klauzulę `where` na wyrażenie `$filter` OData.</span><span class="sxs-lookup"><span data-stu-id="15ed2-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="15ed2-164">Sortowanie ($orderby)</span><span class="sxs-lookup"><span data-stu-id="15ed2-164">Sorting ($orderby)</span></span>

<span data-ttu-id="15ed2-165">Aby posortować, użyj klauzuli `orderby`.</span><span class="sxs-lookup"><span data-stu-id="15ed2-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="15ed2-166">Poniższy przykład sortuje według ceny, od najwyższego do najniższego.</span><span class="sxs-lookup"><span data-stu-id="15ed2-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="15ed2-167">Oto odpowiednie żądanie OData.</span><span class="sxs-lookup"><span data-stu-id="15ed2-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="15ed2-168">Stronicowanie po stronie klienta ($skip i $top)</span><span class="sxs-lookup"><span data-stu-id="15ed2-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="15ed2-169">W przypadku dużych zestawów jednostek klient może chcieć ograniczyć liczbę wyników.</span><span class="sxs-lookup"><span data-stu-id="15ed2-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="15ed2-170">Na przykład klient może wyświetlić 10 wpisów jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="15ed2-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="15ed2-171">Jest to nazywane *stronicowaniem po stronie klienta*.</span><span class="sxs-lookup"><span data-stu-id="15ed2-171">This is called *client-side paging*.</span></span> <span data-ttu-id="15ed2-172">(Istnieje również [stronicowanie po stronie serwera](../supporting-odata-query-options.md#server-paging), gdzie serwer ogranicza liczbę wyników). Aby wykonać stronicowanie po stronie klienta, użyj metody LINQ **Skip** i **Take** .</span><span class="sxs-lookup"><span data-stu-id="15ed2-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="15ed2-173">Poniższy przykład pomija pierwsze 40 wyników i pobiera następne 10.</span><span class="sxs-lookup"><span data-stu-id="15ed2-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="15ed2-174">Oto odpowiednie żądanie OData:</span><span class="sxs-lookup"><span data-stu-id="15ed2-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="15ed2-175">Wybierz ($select) i rozwiń ($expand)</span><span class="sxs-lookup"><span data-stu-id="15ed2-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="15ed2-176">Aby dołączyć powiązane jednostki, użyj metody `DataServiceQuery<t>.Expand`.</span><span class="sxs-lookup"><span data-stu-id="15ed2-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="15ed2-177">Na przykład, aby uwzględnić `Supplier` dla każdej `Product`:</span><span class="sxs-lookup"><span data-stu-id="15ed2-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="15ed2-178">Oto odpowiednie żądanie OData:</span><span class="sxs-lookup"><span data-stu-id="15ed2-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="15ed2-179">Aby zmienić kształt odpowiedzi, użyj klauzuli LINQ **SELECT** .</span><span class="sxs-lookup"><span data-stu-id="15ed2-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="15ed2-180">Poniższy przykład pobiera tylko nazwę każdego produktu bez innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="15ed2-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="15ed2-181">Oto odpowiednie żądanie OData:</span><span class="sxs-lookup"><span data-stu-id="15ed2-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="15ed2-182">Klauzula SELECT może obejmować powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="15ed2-182">A select clause can include related entities.</span></span> <span data-ttu-id="15ed2-183">W takim przypadku nie wywołuj **rozwinięcia**; serwer proxy automatycznie uwzględnia rozszerzenie w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="15ed2-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="15ed2-184">Poniższy przykład pobiera nazwę i dostawcę każdego produktu.</span><span class="sxs-lookup"><span data-stu-id="15ed2-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="15ed2-185">Oto odpowiednie żądanie OData.</span><span class="sxs-lookup"><span data-stu-id="15ed2-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="15ed2-186">Należy zauważyć, że zawiera on opcję **$expand** .</span><span class="sxs-lookup"><span data-stu-id="15ed2-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="15ed2-187">Aby uzyskać więcej informacji na temat $select i $expand, zobacz [używanie $SELECT, $expand i $Value w interfejsie Web API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="15ed2-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="15ed2-188">Dodaj nową jednostkę</span><span class="sxs-lookup"><span data-stu-id="15ed2-188">Add a New Entity</span></span>

<span data-ttu-id="15ed2-189">Aby dodać nową jednostkę do zestawu jednostek, wywołaj `AddToEntitySet`, gdzie *EntitySet* jest nazwą zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="15ed2-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="15ed2-190">Na przykład `AddToProducts` dodaje nowy `Product` do zestawu jednostek `Products`.</span><span class="sxs-lookup"><span data-stu-id="15ed2-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="15ed2-191">Podczas generowania serwera proxy Usługi danych programu WCF automatycznie tworzy te metody **AddTo** z jednoznacznie określonymi typami.</span><span class="sxs-lookup"><span data-stu-id="15ed2-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="15ed2-192">Aby dodać łącze między dwiema jednostkami, użyj metod **AddLink** i **SetLink** .</span><span class="sxs-lookup"><span data-stu-id="15ed2-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="15ed2-193">Poniższy kod dodaje nowego dostawcę i nowy produkt, a następnie tworzy linki między nimi.</span><span class="sxs-lookup"><span data-stu-id="15ed2-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="15ed2-194">Użyj **AddLink** , gdy właściwość nawigacji jest kolekcją.</span><span class="sxs-lookup"><span data-stu-id="15ed2-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="15ed2-195">W tym przykładzie dodajemy produkt do kolekcji `Products` na dostawcy.</span><span class="sxs-lookup"><span data-stu-id="15ed2-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="15ed2-196">Użyj **SetLink** , gdy właściwość nawigacji jest pojedynczą jednostką.</span><span class="sxs-lookup"><span data-stu-id="15ed2-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="15ed2-197">W tym przykładzie ustawiamy Właściwość `Supplier` produktu.</span><span class="sxs-lookup"><span data-stu-id="15ed2-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="15ed2-198">Aktualizacja/poprawka</span><span class="sxs-lookup"><span data-stu-id="15ed2-198">Update / Patch</span></span>

<span data-ttu-id="15ed2-199">Aby zaktualizować jednostkę, wywołaj metodę **UpdateObject** .</span><span class="sxs-lookup"><span data-stu-id="15ed2-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="15ed2-200">Aktualizacja jest wykonywana po wywołaniu **metody SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="15ed2-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="15ed2-201">Domyślnie WCF wysyła żądanie scalania HTTP.</span><span class="sxs-lookup"><span data-stu-id="15ed2-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="15ed2-202">Opcja **PatchOnUpdate** informuje program WCF, aby zamiast tego wysłać poprawkę http.</span><span class="sxs-lookup"><span data-stu-id="15ed2-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="15ed2-203">Dlaczego warto zastosować poprawki i scalić?</span><span class="sxs-lookup"><span data-stu-id="15ed2-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="15ed2-204">Oryginalna specyfikacja protokołu HTTP 1,1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nie definiuje żadnej metody http z semantyką "częściowej aktualizacji".</span><span class="sxs-lookup"><span data-stu-id="15ed2-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="15ed2-205">W celu obsługi aktualizacji częściowych Specyfikacja OData zdefiniowana przez metodę MERGE.</span><span class="sxs-lookup"><span data-stu-id="15ed2-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="15ed2-206">W 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) zdefiniowano metodę patch dla aktualizacji częściowych.</span><span class="sxs-lookup"><span data-stu-id="15ed2-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="15ed2-207">Część historii w tym [wpisie w blogu](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) znajdziesz w blogu usługi danych programu WCF.</span><span class="sxs-lookup"><span data-stu-id="15ed2-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="15ed2-208">Obecnie poprawka jest preferowana przez SCALAnie.</span><span class="sxs-lookup"><span data-stu-id="15ed2-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="15ed2-209">Kontroler OData utworzony przez szkielet interfejsu API sieci Web obsługuje obie metody.</span><span class="sxs-lookup"><span data-stu-id="15ed2-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>

<span data-ttu-id="15ed2-210">Jeśli chcesz zastąpić całą jednostkę (PUT semantyki), określ opcję **ReplaceOnUpdate** .</span><span class="sxs-lookup"><span data-stu-id="15ed2-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="15ed2-211">Powoduje to, że usługa WCF wysyła żądanie HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="15ed2-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="15ed2-212">Usuwanie jednostki</span><span class="sxs-lookup"><span data-stu-id="15ed2-212">Delete an Entity</span></span>

<span data-ttu-id="15ed2-213">Aby usunąć jednostkę, wywołaj metodę **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="15ed2-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="15ed2-214">Wywołaj akcję OData</span><span class="sxs-lookup"><span data-stu-id="15ed2-214">Invoke an OData Action</span></span>

<span data-ttu-id="15ed2-215">W protokole OData [Akcje](odata-actions.md) są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach.</span><span class="sxs-lookup"><span data-stu-id="15ed2-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="15ed2-216">Mimo że dokument metadanych OData zawiera opis akcji, Klasa proxy nie tworzy żadnych metod z jednoznacznie określonymi typami.</span><span class="sxs-lookup"><span data-stu-id="15ed2-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="15ed2-217">Nadal można wywołać akcję OData przy użyciu metody **wykonywania** generycznego.</span><span class="sxs-lookup"><span data-stu-id="15ed2-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="15ed2-218">Należy jednak znać typy danych parametrów i wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="15ed2-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="15ed2-219">Na przykład akcja `RateProduct` przyjmuje parametr o nazwie "Rating" typu `Int32` i zwraca `double`.</span><span class="sxs-lookup"><span data-stu-id="15ed2-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="15ed2-220">Poniższy kod przedstawia sposób wywołania tej akcji.</span><span class="sxs-lookup"><span data-stu-id="15ed2-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="15ed2-221">Aby uzyskać więcej informacji, zobacz[wywoływanie operacji usługi i akcji](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="15ed2-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="15ed2-222">Jedną z opcji jest zwiększenie klasy **kontenera** w celu zapewnienia metody silnie wpisanej, która wywołuje akcję:</span><span class="sxs-lookup"><span data-stu-id="15ed2-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
