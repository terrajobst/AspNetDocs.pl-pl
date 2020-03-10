---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: Protokół Open Data Protocol (OData) to protokół dostępu do danych dla sieci Web. Usługa OData zapewnia jednolity sposób wykonywania zapytań i manipulowania zestawami danych za pomocą operacji CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598736"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="ff6d2-104">Tworzenie punktu końcowego OData v4 przy użyciu interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ff6d2-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="ff6d2-105">Protokół Open Data Protocol (OData) to protokół dostępu do danych dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="ff6d2-106">Usługa OData zapewnia jednolity sposób wykonywania zapytań i manipulowania zestawami danych za pomocą operacji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).</span><span class="sxs-lookup"><span data-stu-id="ff6d2-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="ff6d2-107">Interfejs API sieci Web ASP.NET obsługuje zarówno v3, jak i v4 protokołu.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="ff6d2-108">Można nawet korzystać z punktu końcowego v4, który działa obok siebie z punktem końcowym v3.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="ff6d2-109">W tym samouczku pokazano, jak utworzyć punkt końcowy protokołu OData w wersji 4, który obsługuje operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ff6d2-110">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="ff6d2-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="ff6d2-111">Internetowy interfejs API 5,2</span><span class="sxs-lookup"><span data-stu-id="ff6d2-111">Web API 5.2</span></span>
> - <span data-ttu-id="ff6d2-112">OData 4</span><span class="sxs-lookup"><span data-stu-id="ff6d2-112">OData v4</span></span>
> - <span data-ttu-id="ff6d2-113">Visual Studio 2017 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/))</span><span class="sxs-lookup"><span data-stu-id="ff6d2-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="ff6d2-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ff6d2-114">Entity Framework 6</span></span>
> - <span data-ttu-id="ff6d2-115">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="ff6d2-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="ff6d2-116">Wersje samouczka</span><span class="sxs-lookup"><span data-stu-id="ff6d2-116">Tutorial versions</span></span>
>
> <span data-ttu-id="ff6d2-117">W przypadku usługi OData w wersji 3 zobacz [Tworzenie punktu końcowego OData V3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="ff6d2-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="ff6d2-118">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff6d2-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="ff6d2-119">W programie Visual Studio w menu **plik** wybierz pozycję **Nowy** **projekt**&gt;.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="ff6d2-120">Rozwiń węzeł **zainstalowane** &gt; **Visual C#**  &gt; **Web**i wybierz szablon **aplikacja sieci Web ASP.NET (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="ff6d2-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="ff6d2-121">Nazwij projekt &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="ff6d2-122">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-122">Select **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="ff6d2-123">Wybierz **pusty** szablon.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-123">Select the **Empty** template.</span></span> <span data-ttu-id="ff6d2-124">W obszarze **Dodaj foldery i podstawowe odwołania dla:** wybierz pozycję **interfejs API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="ff6d2-125">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="ff6d2-126">Instalowanie pakietów OData</span><span class="sxs-lookup"><span data-stu-id="ff6d2-126">Install the OData packages</span></span>

<span data-ttu-id="ff6d2-127">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** &gt; **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="ff6d2-128">W oknie Konsola Menedżera pakietów wpisz:</span><span class="sxs-lookup"><span data-stu-id="ff6d2-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="ff6d2-129">To polecenie powoduje zainstalowanie najnowszych pakietów NuGet OData.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="ff6d2-130">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="ff6d2-130">Add a model class</span></span>

<span data-ttu-id="ff6d2-131">*Model* to obiekt, który reprezentuje jednostkę danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="ff6d2-132">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="ff6d2-133">Z menu kontekstowego wybierz pozycję **Dodaj** **klasę**&gt;.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="ff6d2-134">Według Konwencji klasy modelu są umieszczane w folderze modele, ale nie trzeba stosować tej Konwencji we własnych projektach.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>

<span data-ttu-id="ff6d2-135">Nadaj klasie nazwę `Product`.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-135">Name the class `Product`.</span></span> <span data-ttu-id="ff6d2-136">W pliku Product.cs Zastąp kod standardowy następującymi:</span><span class="sxs-lookup"><span data-stu-id="ff6d2-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="ff6d2-137">Właściwość `Id` jest kluczem jednostki.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="ff6d2-138">Klienci mogą wysyłać zapytania o jednostki według klucza.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-138">Clients can query entities by key.</span></span> <span data-ttu-id="ff6d2-139">Na przykład aby uzyskać produkt o IDENTYFIKATORze 5, identyfikator URI jest `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="ff6d2-140">Właściwość `Id` będzie również kluczem podstawowym w bazie danych zaplecza.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="ff6d2-141">Włącz Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ff6d2-141">Enable Entity Framework</span></span>

<span data-ttu-id="ff6d2-142">W tym samouczku użyjemy Code First Entity Framework (EF), aby utworzyć bazę danych zaplecza.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="ff6d2-143">Protokół OData interfejsu API sieci Web nie wymaga EF.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-143">Web API OData does not require EF.</span></span> <span data-ttu-id="ff6d2-144">Użyj dowolnej warstwy dostępu do danych, która może przetłumaczyć jednostki bazy danych na modele.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-144">Use any data-access layer that can translate database entities into models.</span></span>

<span data-ttu-id="ff6d2-145">Najpierw zainstaluj pakiet NuGet dla programu EF.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="ff6d2-146">W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** &gt; **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="ff6d2-147">W oknie Konsola Menedżera pakietów wpisz:</span><span class="sxs-lookup"><span data-stu-id="ff6d2-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="ff6d2-148">Otwórz plik Web. config i Dodaj następującą sekcję w elemencie **Configuration** , po elemencie **configSections** .</span><span class="sxs-lookup"><span data-stu-id="ff6d2-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="ff6d2-149">To ustawienie powoduje dodanie parametrów połączenia dla bazy danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="ff6d2-150">Ta baza danych będzie używana podczas lokalnego uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="ff6d2-151">Następnie Dodaj klasę o nazwie `ProductsContext` do folderu models:</span><span class="sxs-lookup"><span data-stu-id="ff6d2-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="ff6d2-152">W konstruktorze `"name=ProductsContext"` podaje nazwę ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="ff6d2-153">Konfigurowanie punktu końcowego OData</span><span class="sxs-lookup"><span data-stu-id="ff6d2-153">Configure the OData endpoint</span></span>

<span data-ttu-id="ff6d2-154">Otwórz aplikację pliku\_Start/WebApiConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="ff6d2-155">Dodaj następujące instrukcje **using** :</span><span class="sxs-lookup"><span data-stu-id="ff6d2-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="ff6d2-156">Następnie Dodaj następujący kod do metody **register** :</span><span class="sxs-lookup"><span data-stu-id="ff6d2-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="ff6d2-157">Ten kod wykonuje dwie czynności:</span><span class="sxs-lookup"><span data-stu-id="ff6d2-157">This code does two things:</span></span>

- <span data-ttu-id="ff6d2-158">Tworzy Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="ff6d2-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="ff6d2-159">Dodaje trasę.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-159">Adds a route.</span></span>

<span data-ttu-id="ff6d2-160">EDM to abstrakcyjny model danych.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="ff6d2-161">Modelu EDM służy do tworzenia dokumentu metadanych usługi.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="ff6d2-162">Klasa **ODataConventionModelBuilder** tworzy obiekt EDM przy użyciu domyślnych konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="ff6d2-163">To podejście wymaga najmniejszego kodu.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-163">This approach requires the least code.</span></span> <span data-ttu-id="ff6d2-164">Jeśli chcesz mieć większą kontrolę nad modelu EDM, możesz użyć klasy **ODataModelBuilder** do utworzenia modelu EDM przez dodanie właściwości, kluczy i właściwości nawigacji jawnie.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="ff6d2-165">*Trasa* informuje interfejs API sieci Web, jak kierować żądania HTTP do punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="ff6d2-166">Aby utworzyć trasę protokołu OData w wersji 4, wywołaj metodę rozszerzenia **MapODataServiceRoute** .</span><span class="sxs-lookup"><span data-stu-id="ff6d2-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="ff6d2-167">Jeśli aplikacja ma wiele punktów końcowych OData, utwórz oddzielną trasę dla każdej z nich.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="ff6d2-168">Nadaj każdej trasie unikatową nazwę i prefiks trasy.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="ff6d2-169">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="ff6d2-169">Add the OData controller</span></span>

<span data-ttu-id="ff6d2-170">*Kontroler* jest klasą, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="ff6d2-171">Należy utworzyć oddzielny kontroler dla każdego zestawu jednostek w usłudze OData.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="ff6d2-172">W tym samouczku utworzysz jeden kontroler dla jednostki `Product`.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="ff6d2-173">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz polecenie **Dodaj** **klasę**&gt;.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="ff6d2-174">Nadaj klasie nazwę `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="ff6d2-175">Wersja tego samouczka dla protokołu OData V3 używa tworzenia szkieletu **kontrolera** .</span><span class="sxs-lookup"><span data-stu-id="ff6d2-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="ff6d2-176">Obecnie nie ma szkieletu dla protokołu OData v4.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-176">Currently, there is no scaffolding for OData v4.</span></span>

<span data-ttu-id="ff6d2-177">Zastąp kod standardowy w ProductsController.cs poniższymi.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="ff6d2-178">Kontroler używa klasy `ProductsContext`, aby uzyskać dostęp do bazy danych przy użyciu EF.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="ff6d2-179">Należy zauważyć, że kontroler zastępuje metodę **Dispose** , aby usunąć **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="ff6d2-180">Jest to punkt początkowy dla kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-180">This is the starting point for the controller.</span></span> <span data-ttu-id="ff6d2-181">Następnie dodamy metody dla wszystkich operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="ff6d2-182">Zapytanie dotyczące zestawu jednostek</span><span class="sxs-lookup"><span data-stu-id="ff6d2-182">Query the entity set</span></span>

<span data-ttu-id="ff6d2-183">Dodaj następujące metody do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="ff6d2-184">Bezparametrowa wersja metody `Get` zwraca całą kolekcję produktów.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="ff6d2-185">Metoda `Get` z parametrem *klucza* wyszukuje produkt według jego klucza (w tym przypadku właściwości `Id`).</span><span class="sxs-lookup"><span data-stu-id="ff6d2-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="ff6d2-186">Atrybut **[EnableQuery]** umożliwia klientom modyfikowanie zapytania przy użyciu opcji zapytania, takich jak $filter, $sort i $Page.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="ff6d2-187">Aby uzyskać więcej informacji, zobacz [Obsługa opcji zapytania OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="ff6d2-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="ff6d2-188">Dodawanie jednostki do zestawu jednostek</span><span class="sxs-lookup"><span data-stu-id="ff6d2-188">Add an entity to the entity set</span></span>

<span data-ttu-id="ff6d2-189">Aby umożliwić klientom Dodawanie nowego produktu do bazy danych programu, Dodaj następującą metodę do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="ff6d2-190">Aktualizuj jednostkę</span><span class="sxs-lookup"><span data-stu-id="ff6d2-190">Update an entity</span></span>

<span data-ttu-id="ff6d2-191">Usługa OData obsługuje dwie różne semantyki do aktualizowania jednostki, poprawki i UMIESZCZAnia.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="ff6d2-192">Poprawka wykonuje aktualizację częściową.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-192">PATCH performs a partial update.</span></span> <span data-ttu-id="ff6d2-193">Klient określa tylko właściwości do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="ff6d2-194">Parametr PUT zastępuje całą jednostkę.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="ff6d2-195">Wadą jest to, że klient musi wysyłać wartości dla wszystkich właściwości w jednostce, w tym wartości, które nie są zmieniane.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="ff6d2-196">[Specyfikacja OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) wskazuje, że poprawka jest preferowana.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="ff6d2-197">W każdym przypadku jest to kod dla obu metod PATCH i PUT:</span><span class="sxs-lookup"><span data-stu-id="ff6d2-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="ff6d2-198">W przypadku poprawki kontroler używa typu **Delta&lt;t&gt;** do śledzenia zmian.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="ff6d2-199">Usuwanie jednostki</span><span class="sxs-lookup"><span data-stu-id="ff6d2-199">Delete an entity</span></span>

<span data-ttu-id="ff6d2-200">Aby umożliwić klientom usuwanie produktu z bazy danych programu, Dodaj następującą metodę do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="ff6d2-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
