---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Tworzenie punktu końcowego OData V3 przy użyciu interfejsu Web API 2 | Microsoft Docs
author: MikeWasson
description: Protokół Open Data Protocol (OData) to protokół dostępu do danych dla sieci Web. Usługa OData zapewnia jednolity sposób tworzenia struktur danych, wykonywania zapytań dotyczących danych i manipulowania danymi...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600432"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="4af08-104">Tworzenie punktu końcowego OData V3 przy użyciu interfejsu Web API 2</span><span class="sxs-lookup"><span data-stu-id="4af08-104">Creating an OData v3 Endpoint with Web API 2</span></span>

<span data-ttu-id="4af08-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4af08-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4af08-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="4af08-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="4af08-107">[Protokół Open Data Protocol](http://www.odata.org/) (OData) to protokół dostępu do danych dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4af08-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="4af08-108">Usługa OData zapewnia jednolity sposób tworzenia struktur danych, wykonywania zapytań dotyczących danych i manipulowania danymi za pomocą operacji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).</span><span class="sxs-lookup"><span data-stu-id="4af08-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="4af08-109">Usługa OData obsługuje zarówno formaty AtomPub (XML), jak i JSON.</span><span class="sxs-lookup"><span data-stu-id="4af08-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="4af08-110">Usługa OData definiuje również sposób ujawniania metadanych dotyczących danych.</span><span class="sxs-lookup"><span data-stu-id="4af08-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="4af08-111">Klienci mogą używać metadanych do wykrywania informacji o typie i relacji dla zestawu danych.</span><span class="sxs-lookup"><span data-stu-id="4af08-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
>
> <span data-ttu-id="4af08-112">Interfejs API sieci Web ASP.NET ułatwia tworzenie punktu końcowego OData dla zestawu danych.</span><span class="sxs-lookup"><span data-stu-id="4af08-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="4af08-113">Można kontrolować dokładne operacje OData obsługiwane przez punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="4af08-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="4af08-114">Można hostować wiele punktów końcowych OData, obok punktów końcowych innych niż OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="4af08-115">Masz pełną kontrolę nad modelem danych, logiką biznesową i warstwą danych.</span><span class="sxs-lookup"><span data-stu-id="4af08-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4af08-116">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="4af08-116">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="4af08-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4af08-117">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="4af08-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="4af08-118">Web API 2</span></span>
> - <span data-ttu-id="4af08-119">Usługa OData w wersji 3</span><span class="sxs-lookup"><span data-stu-id="4af08-119">OData Version 3</span></span>
> - <span data-ttu-id="4af08-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4af08-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="4af08-121">Serwer proxy debugowania sieci Web programu Fiddler (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="4af08-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
>
> <span data-ttu-id="4af08-122">Obsługa protokołu OData interfejsu API sieci Web została dodana w [ASP.NET and Web Tools 2012,2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="4af08-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="4af08-123">Jednak w tym samouczku użyto szkieletu, który został dodany w Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4af08-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>

<span data-ttu-id="4af08-124">W tym samouczku utworzysz prosty punkt końcowy OData, który mogą wykonywać zapytania na klientach.</span><span class="sxs-lookup"><span data-stu-id="4af08-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="4af08-125">Zostanie również utworzony C# klient dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="4af08-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="4af08-126">Po ukończeniu tego samouczka następny zestaw samouczków pokazuje, jak dodać więcej funkcji, w tym relacje między jednostkami, akcje i $expand/$select.</span><span class="sxs-lookup"><span data-stu-id="4af08-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="4af08-127">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4af08-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="4af08-128">Dodaj model jednostki</span><span class="sxs-lookup"><span data-stu-id="4af08-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="4af08-129">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="4af08-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="4af08-130">Dodawanie modelu EDM i trasy</span><span class="sxs-lookup"><span data-stu-id="4af08-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="4af08-131">Wypełnianie bazy danych (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="4af08-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="4af08-132">Eksplorowanie punktu końcowego OData</span><span class="sxs-lookup"><span data-stu-id="4af08-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="4af08-133">Formaty serializacji OData</span><span class="sxs-lookup"><span data-stu-id="4af08-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="4af08-134">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4af08-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="4af08-135">W tym samouczku utworzysz punkt końcowy OData, który obsługuje podstawowe operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="4af08-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="4af08-136">Punkt końcowy uwidacznia pojedynczy zasób, listę produktów.</span><span class="sxs-lookup"><span data-stu-id="4af08-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="4af08-137">W kolejnych samouczkach zostanie dodana większa funkcja.</span><span class="sxs-lookup"><span data-stu-id="4af08-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="4af08-138">Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie startowej.</span><span class="sxs-lookup"><span data-stu-id="4af08-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="4af08-139">Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="4af08-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="4af08-140">W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł Wizualizacja C# .</span><span class="sxs-lookup"><span data-stu-id="4af08-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="4af08-141">W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="4af08-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="4af08-142">Wybierz szablon **aplikacji sieci Web ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="4af08-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="4af08-143">W oknie dialogowym **Nowy projekt ASP.NET** wybierz **pusty** szablon.</span><span class="sxs-lookup"><span data-stu-id="4af08-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="4af08-144">W obszarze &quot;Dodaj foldery i podstawowe odwołania dla...&quot;, sprawdź **internetowy interfejs API**.</span><span class="sxs-lookup"><span data-stu-id="4af08-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="4af08-145">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4af08-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="4af08-146">Dodaj model jednostki</span><span class="sxs-lookup"><span data-stu-id="4af08-146">Add an Entity Model</span></span>

<span data-ttu-id="4af08-147">*Model* to obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4af08-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="4af08-148">W tym samouczku potrzebny jest model, który reprezentuje produkt.</span><span class="sxs-lookup"><span data-stu-id="4af08-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="4af08-149">Model odpowiada typowi obiektu OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="4af08-150">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder modele.</span><span class="sxs-lookup"><span data-stu-id="4af08-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="4af08-151">Z menu kontekstowego wybierz pozycję **Dodaj** , a następnie wybierz pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="4af08-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="4af08-152">W oknie dialogowym **Dodaj nowy** element nazwij klasę &quot;&quot;produktu.</span><span class="sxs-lookup"><span data-stu-id="4af08-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="4af08-153">Według Konwencji klasy modelu są umieszczane w folderze models.</span><span class="sxs-lookup"><span data-stu-id="4af08-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="4af08-154">Nie musisz stosować tej Konwencji we własnych projektach, ale użyjemy jej w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="4af08-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>

<span data-ttu-id="4af08-155">W pliku Product.cs Dodaj następującą definicję klasy:</span><span class="sxs-lookup"><span data-stu-id="4af08-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="4af08-156">Właściwość ID będzie kluczem jednostki.</span><span class="sxs-lookup"><span data-stu-id="4af08-156">The ID property will be the entity key.</span></span> <span data-ttu-id="4af08-157">Klienci mogą badać produkty według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="4af08-157">Clients can query products by ID.</span></span> <span data-ttu-id="4af08-158">To pole będzie również kluczem podstawowym w bazie danych zaplecza.</span><span class="sxs-lookup"><span data-stu-id="4af08-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="4af08-159">Kompiluj projekt teraz.</span><span class="sxs-lookup"><span data-stu-id="4af08-159">Build the project now.</span></span> <span data-ttu-id="4af08-160">W następnym kroku użyjemy szkieletu programu Visual Studio, który używa odbicia, aby znaleźć typ produktu.</span><span class="sxs-lookup"><span data-stu-id="4af08-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="4af08-161">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="4af08-161">Add an OData Controller</span></span>

<span data-ttu-id="4af08-162">*Kontroler* jest klasą, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="4af08-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="4af08-163">Należy zdefiniować osobny kontroler dla każdego zestawu jednostek w usłudze OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="4af08-164">W tym samouczku utworzymy jeden kontroler.</span><span class="sxs-lookup"><span data-stu-id="4af08-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="4af08-165">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers.</span><span class="sxs-lookup"><span data-stu-id="4af08-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="4af08-166">Wybierz pozycję **Dodaj** , a następnie wybierz pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="4af08-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="4af08-167">W oknie dialogowym **Dodawanie szkieletu** wybierz kontroler &quot;Web API 2 OData z akcjami, używając Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="4af08-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="4af08-168">W oknie dialogowym **Dodawanie kontrolera** Nazwij kontroler "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="4af08-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="4af08-169">Zaznacz pole wyboru &quot;Użyj akcji kontrolera asynchronicznego&quot;.</span><span class="sxs-lookup"><span data-stu-id="4af08-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="4af08-170">Z listy rozwijanej **model** wybierz klasę Product (produkt).</span><span class="sxs-lookup"><span data-stu-id="4af08-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="4af08-171">Kliknij przycisk **nowy kontekst danych..** .</span><span class="sxs-lookup"><span data-stu-id="4af08-171">Click the **New data context...** button.</span></span> <span data-ttu-id="4af08-172">Pozostaw domyślną nazwę dla typu kontekstu danych, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4af08-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="4af08-173">Kliknij przycisk Dodaj w oknie dialogowym Dodaj kontroler, aby dodać kontroler.</span><span class="sxs-lookup"><span data-stu-id="4af08-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="4af08-174">Uwaga: Jeśli pojawi się komunikat o błędzie z informacją &quot;Wystąpił błąd podczas pobierania typu...&quot;, upewnij się, że projekt programu Visual Studio został skompilowany po dodaniu klasy Product.</span><span class="sxs-lookup"><span data-stu-id="4af08-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="4af08-175">Szkielet używa odbicia, aby znaleźć klasę.</span><span class="sxs-lookup"><span data-stu-id="4af08-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="4af08-176">Tworzenie szkieletu powoduje dodanie dwóch plików kodu do projektu:</span><span class="sxs-lookup"><span data-stu-id="4af08-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="4af08-177">Products.cs definiuje kontroler internetowego interfejsu API, który implementuje punkt końcowy OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="4af08-178">ProductServiceContext.cs udostępnia metody do wykonywania zapytań względem źródłowej bazy danych przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4af08-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="4af08-179">Dodawanie modelu EDM i trasy</span><span class="sxs-lookup"><span data-stu-id="4af08-179">Add the EDM and Route</span></span>

<span data-ttu-id="4af08-180">W Eksplorator rozwiązań rozwiń węzeł aplikacja\_folder Start i Otwórz plik o nazwie WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="4af08-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="4af08-181">Ta klasa zawiera kod konfiguracyjny dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="4af08-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="4af08-182">Zastąp ten kod następującym:</span><span class="sxs-lookup"><span data-stu-id="4af08-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="4af08-183">Ten kod wykonuje dwie czynności:</span><span class="sxs-lookup"><span data-stu-id="4af08-183">This code does two things:</span></span>

- <span data-ttu-id="4af08-184">Tworzy Entity Data Model (EDM) dla punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="4af08-185">Dodaje trasę dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="4af08-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="4af08-186">EDM to abstrakcyjny model danych.</span><span class="sxs-lookup"><span data-stu-id="4af08-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="4af08-187">Modelu EDM służy do tworzenia dokumentu metadanych i definiowania identyfikatorów URI dla usługi.</span><span class="sxs-lookup"><span data-stu-id="4af08-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="4af08-188">**ODataConventionModelBuilder** tworzy obiekt EDM przy użyciu zestawu domyślnych konwencji nazewnictwa modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="4af08-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="4af08-189">To podejście wymaga najmniejszego kodu.</span><span class="sxs-lookup"><span data-stu-id="4af08-189">This approach requires the least code.</span></span> <span data-ttu-id="4af08-190">Jeśli chcesz mieć większą kontrolę nad modelu EDM, możesz użyć klasy **ODataModelBuilder** do utworzenia modelu EDM przez dodanie właściwości, kluczy i właściwości nawigacji jawnie.</span><span class="sxs-lookup"><span data-stu-id="4af08-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="4af08-191">Metoda **EntitySet** dodaje jednostkę ustawioną do modelu EDM:</span><span class="sxs-lookup"><span data-stu-id="4af08-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="4af08-192">Ciąg "Products" definiuje nazwę zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="4af08-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="4af08-193">Nazwa kontrolera musi być zgodna z nazwą zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="4af08-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="4af08-194">W tym samouczku zestaw jednostek ma nazwę "produkty", a kontroler ma nazwę `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="4af08-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="4af08-195">Jeśli nazwa zestawu jednostek to "ProductSet", należy nazwać kontroler `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="4af08-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="4af08-196">Należy pamiętać, że punkt końcowy może mieć wiele zestawów jednostek.</span><span class="sxs-lookup"><span data-stu-id="4af08-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="4af08-197">Wywołaj element **EntitySet&lt;t&gt;** dla każdego zestawu jednostek, a następnie zdefiniuj odpowiedni kontroler.</span><span class="sxs-lookup"><span data-stu-id="4af08-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="4af08-198">Metoda **MapODataRoute** dodaje trasę do punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="4af08-199">Pierwszy parametr jest przyjazną nazwą dla trasy.</span><span class="sxs-lookup"><span data-stu-id="4af08-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="4af08-200">Klienci usługi nie widzą tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="4af08-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="4af08-201">Drugi parametr jest prefiksem identyfikatora URI dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="4af08-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="4af08-202">Mając ten kod, identyfikator URI dla zestawu jednostek Products to http://<em>hostname</em>/OData/Products.</span><span class="sxs-lookup"><span data-stu-id="4af08-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="4af08-203">Aplikacja może mieć więcej niż jeden punkt końcowy OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="4af08-204">Dla każdego punktu końcowego Wywołaj <strong>MapODataRoute</strong> i podaj unikatową nazwę trasy i unikatowy prefiks identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="4af08-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="4af08-205">Wypełnianie bazy danych (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="4af08-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="4af08-206">W tym kroku będziesz używać Entity Framework do wypełniania bazy danych niektórymi danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="4af08-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="4af08-207">Ten krok jest opcjonalny, ale pozwala na natychmiastowe przetestowanie punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="4af08-208">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="4af08-208">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4af08-209">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4af08-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="4af08-210">Spowoduje to dodanie folderu o nazwie migrations i pliku kodu o nazwie Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="4af08-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="4af08-211">Otwórz ten plik i Dodaj następujący kod do metody `Configuration.Seed`.</span><span class="sxs-lookup"><span data-stu-id="4af08-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="4af08-212">W oknie Konsola Menedżera pakietów wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="4af08-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="4af08-213">Te polecenia generują kod, który tworzy bazę danych, a następnie wykonuje ten kod.</span><span class="sxs-lookup"><span data-stu-id="4af08-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="4af08-214">Eksplorowanie punktu końcowego OData</span><span class="sxs-lookup"><span data-stu-id="4af08-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="4af08-215">W tej sekcji użyjemy [serwera proxy debugowania sieci Web programu Fiddler](http://www.fiddler2.com) do wysyłania żądań do punktu końcowego i badania komunikatów odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4af08-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="4af08-216">Pomoże to zrozumieć możliwości punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="4af08-217">W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowanie.</span><span class="sxs-lookup"><span data-stu-id="4af08-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="4af08-218">Domyślnie program Visual Studio otwiera przeglądarkę w celu `http://localhost:*port*`, gdzie *port* jest numerem portu skonfigurowanym w ustawieniach projektu.</span><span class="sxs-lookup"><span data-stu-id="4af08-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="4af08-219">Numer portu można zmienić w ustawieniach projektu.</span><span class="sxs-lookup"><span data-stu-id="4af08-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="4af08-220">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="4af08-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="4af08-221">W oknie właściwości wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="4af08-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="4af08-222">Wprowadź numer portu w polu **adres URL projektu**.</span><span class="sxs-lookup"><span data-stu-id="4af08-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="4af08-223">Dokument usługi</span><span class="sxs-lookup"><span data-stu-id="4af08-223">Service Document</span></span>

<span data-ttu-id="4af08-224">*Dokument usługi* zawiera listę zestawów jednostek dla punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="4af08-225">Aby uzyskać dokument usługi, Wyślij żądanie GET do głównego identyfikatora URI usługi.</span><span class="sxs-lookup"><span data-stu-id="4af08-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="4af08-226">Korzystając z programu Fiddler, wprowadź następujący identyfikator URI na karcie **układacz** : `http://localhost:port/odata/`, gdzie *port* jest numerem portu.</span><span class="sxs-lookup"><span data-stu-id="4af08-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="4af08-227">Kliknij przycisk **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="4af08-227">Click the **Execute** button.</span></span> <span data-ttu-id="4af08-228">Programu Fiddler wysyła do aplikacji żądanie HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="4af08-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="4af08-229">Odpowiedź powinna zostać wyświetlona na liście sesje sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4af08-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="4af08-230">Jeśli wszystko działa, kod stanu będzie 200.</span><span class="sxs-lookup"><span data-stu-id="4af08-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="4af08-231">Kliknij dwukrotnie odpowiedź na liście sesji sieci Web, aby wyświetlić szczegóły komunikatu odpowiedzi na karcie inspektorzy.</span><span class="sxs-lookup"><span data-stu-id="4af08-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="4af08-232">Nieprzetworzony komunikat odpowiedzi HTTP powinien wyglądać podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="4af08-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="4af08-233">Domyślnie interfejs API sieci Web zwraca dokument usługi w formacie AtomPub.</span><span class="sxs-lookup"><span data-stu-id="4af08-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="4af08-234">Aby poprosić o dane JSON, Dodaj następujący nagłówek do żądania HTTP:</span><span class="sxs-lookup"><span data-stu-id="4af08-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="4af08-235">Teraz odpowiedź HTTP zawiera ładunek JSON:</span><span class="sxs-lookup"><span data-stu-id="4af08-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="4af08-236">Dokument metadanych usługi</span><span class="sxs-lookup"><span data-stu-id="4af08-236">Service Metadata Document</span></span>

<span data-ttu-id="4af08-237">W *dokumencie metadanych usługi* opisano model danych usługi przy użyciu języka XML o nazwie język definicji schematu koncepcyjnego (CSDL).</span><span class="sxs-lookup"><span data-stu-id="4af08-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="4af08-238">Dokument metadanych przedstawia strukturę danych w usłudze i może służyć do generowania kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="4af08-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="4af08-239">Aby uzyskać dokument metadanych, Wyślij żądanie GET do `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="4af08-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="4af08-240">Oto metadane dla punktu końcowego wyświetlanego w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="4af08-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="4af08-241">Zestaw jednostek</span><span class="sxs-lookup"><span data-stu-id="4af08-241">Entity Set</span></span>

<span data-ttu-id="4af08-242">Aby uzyskać zestaw jednostek Products, Wyślij żądanie GET do `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="4af08-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="4af08-243">Jednostka</span><span class="sxs-lookup"><span data-stu-id="4af08-243">Entity</span></span>

<span data-ttu-id="4af08-244">Aby uzyskać indywidualny produkt, Wyślij żądanie GET do `http://localhost:port/odata/Products(1)`, gdzie "1" jest IDENTYFIKATORem produktu.</span><span class="sxs-lookup"><span data-stu-id="4af08-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="4af08-245">Formaty serializacji OData</span><span class="sxs-lookup"><span data-stu-id="4af08-245">OData Serialization Formats</span></span>

<span data-ttu-id="4af08-246">Usługa OData obsługuje kilka formatów serializacji:</span><span class="sxs-lookup"><span data-stu-id="4af08-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="4af08-247">Atom pub (XML)</span><span class="sxs-lookup"><span data-stu-id="4af08-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="4af08-248">Notacja JSON "Light" (wprowadzona w protokole OData V3)</span><span class="sxs-lookup"><span data-stu-id="4af08-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="4af08-249">JSON "verbose" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="4af08-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="4af08-250">Domyślnie interfejs API sieci Web używa formatu AtomPubJSON "Light".</span><span class="sxs-lookup"><span data-stu-id="4af08-250">By default, Web API uses AtomPubJSON "light" format.</span></span>

<span data-ttu-id="4af08-251">Aby uzyskać format AtomPub, należy ustawić nagłówek Accept na wartość "Application/Atom + XML".</span><span class="sxs-lookup"><span data-stu-id="4af08-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="4af08-252">Oto przykład treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="4af08-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="4af08-253">Można zobaczyć jedną z oczywistych niekorzystnych niedogodności w formacie Atom: jest to znacznie bardziej szczegółowe niż oświetlenie JSON.</span><span class="sxs-lookup"><span data-stu-id="4af08-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="4af08-254">Jeśli jednak klient ma świadomość AtomPub, klient może preferować ten format w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="4af08-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="4af08-255">Oto wersja uproszczona JSON tej samej jednostki:</span><span class="sxs-lookup"><span data-stu-id="4af08-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="4af08-256">Format oświetlenia JSON został wprowadzony w wersji 3 protokołu OData.</span><span class="sxs-lookup"><span data-stu-id="4af08-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="4af08-257">W celu zapewnienia zgodności z poprzednimi wersjami klient może zażądać starszego formatu JSON "verbose".</span><span class="sxs-lookup"><span data-stu-id="4af08-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="4af08-258">Aby zażądać pełnego kodu JSON, ustaw nagłówek Accept na `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="4af08-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="4af08-259">Oto pełna wersja:</span><span class="sxs-lookup"><span data-stu-id="4af08-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="4af08-260">Ten format umożliwia przekazanie większej liczby metadanych w treści odpowiedzi, co może zwiększyć znaczne obciążenie całej sesji.</span><span class="sxs-lookup"><span data-stu-id="4af08-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="4af08-261">Ponadto dodaje poziom pośredni przez Zawijanie obiektu we właściwości o nazwie "d".</span><span class="sxs-lookup"><span data-stu-id="4af08-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="4af08-262">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4af08-262">Next Steps</span></span>

- [<span data-ttu-id="4af08-263">Dodawanie relacji jednostek</span><span class="sxs-lookup"><span data-stu-id="4af08-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="4af08-264">Dodawanie akcji OData</span><span class="sxs-lookup"><span data-stu-id="4af08-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="4af08-265">Wywoływanie usługi OData z klienta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="4af08-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
