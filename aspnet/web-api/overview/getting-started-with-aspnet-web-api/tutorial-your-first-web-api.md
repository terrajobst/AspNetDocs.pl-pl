---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Wprowadzenie do usługi ASP.NET Web API 2 (C#) — ASP.NET 4. x
author: MikeWasson
description: Samouczek z kodem. Użyj interfejsu API sieci Web ASP.NET, aby utworzyć internetowy interfejs API, który zwraca listę produktów.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2717d93f47be9d4a6548731d8deeca312b25f39f
ms.sourcegitcommit: 9e3ca74997a67c18589729d4b7303799905473eb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084055"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="cce18-104">Wprowadzenie do usługi ASP.NET Web API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="cce18-104">Get Started with ASP.NET Web API 2 (C#)</span></span>

<span data-ttu-id="cce18-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cce18-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cce18-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="cce18-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="cce18-107">W tym samouczku użyjesz internetowego interfejsu API platformy ASP.NET do tworzenia internetowego interfejsu API, który zwraca listę produktów.</span><span class="sxs-lookup"><span data-stu-id="cce18-107">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

<span data-ttu-id="cce18-108">Protokół HTTP nie jest używany tylko do obsługi stron internetowych.</span><span class="sxs-lookup"><span data-stu-id="cce18-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="cce18-109">Protokół HTTP to również zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi.</span><span class="sxs-lookup"><span data-stu-id="cce18-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="cce18-110">Protokół HTTP jest prosty, elastyczny i uniwersalny.</span><span class="sxs-lookup"><span data-stu-id="cce18-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="cce18-111">Prawie każda platforma obsługuje bibliotekę HTTP, więc usługi HTTP mogą być stosowane dla szerokiej gamy klientów takich jak przeglądarki, urządzenia przenośne czy tradycyjne aplikacje komputerowe.</span><span class="sxs-lookup"><span data-stu-id="cce18-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="cce18-112">Internetowy interfejs API platformy ASP.NET to architektura służąca do tworzenia internetowych interfejsów API w programie .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="cce18-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> 

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cce18-113">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="cce18-113">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="cce18-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="cce18-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="cce18-115">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="cce18-115">Web API 2</span></span>

<span data-ttu-id="cce18-116">Aby uzyskać nowszą wersję tego samouczka [, zobacz Tworzenie internetowego interfejsu API za pomocą ASP.NET Core i programu Visual Studio dla systemu Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) .</span><span class="sxs-lookup"><span data-stu-id="cce18-116">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="cce18-117">Tworzenie projektu interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="cce18-117">Create a Web API Project</span></span>

<span data-ttu-id="cce18-118">W tym samouczku użyjesz internetowego interfejsu API platformy ASP.NET do tworzenia internetowego interfejsu API, który zwraca listę produktów.</span><span class="sxs-lookup"><span data-stu-id="cce18-118">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="cce18-119">Strona sieci Web frontonu używa technologii jQuery do wyświetlania wyników.</span><span class="sxs-lookup"><span data-stu-id="cce18-119">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="cce18-120">Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **startowej** .</span><span class="sxs-lookup"><span data-stu-id="cce18-120">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="cce18-121">Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="cce18-121">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="cce18-122">W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  .</span><span class="sxs-lookup"><span data-stu-id="cce18-122">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="cce18-123">W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="cce18-123">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="cce18-124">Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="cce18-124">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="cce18-125">Nadaj projektowi nazwę "ProductsApp" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="cce18-125">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="cce18-126">W oknie dialogowym **Nowy projekt ASP.NET** wybierz **pusty** szablon.</span><span class="sxs-lookup"><span data-stu-id="cce18-126">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="cce18-127">W obszarze &quot;Dodaj foldery i podstawowe odwołania dla&quot;Sprawdź, czy jest **internetowy interfejs API**.</span><span class="sxs-lookup"><span data-stu-id="cce18-127">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="cce18-128">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="cce18-128">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="cce18-129">Projekt interfejsu API sieci Web można również utworzyć przy użyciu szablonu&quot; &quot;Web API.</span><span class="sxs-lookup"><span data-stu-id="cce18-129">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="cce18-130">Szablon internetowego interfejsu API używa platformy ASP.NET MVC do udostępniania stron pomocy do interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="cce18-130">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="cce18-131">Używam pustego szablonu na potrzeby tego samouczka, ponieważ chcę pokazać internetowy interfejs API bez platformy MVC.</span><span class="sxs-lookup"><span data-stu-id="cce18-131">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="cce18-132">Ogólnie rzecz biorąc, nie trzeba znać platformy ASP.NET MVC, aby korzystać z internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="cce18-132">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="cce18-133">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="cce18-133">Adding a Model</span></span>

<span data-ttu-id="cce18-134">*Model* to obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cce18-134">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="cce18-135">Internetowy interfejs API platformy ASP.NET może automatycznie zserializować model do formatu JSON, XML lub innego, a następnie wpisać te dane serializowane w treści komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="cce18-135">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="cce18-136">Jeśli klient może odczytać format serializacji, może również wykonywać deserializację obiektu.</span><span class="sxs-lookup"><span data-stu-id="cce18-136">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="cce18-137">Większość klientów może analizować albo format XML, albo JSON.</span><span class="sxs-lookup"><span data-stu-id="cce18-137">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="cce18-138">Ponadto klient może wskazać pożądany format, ustawiając nagłówek Accept w komunikacie żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="cce18-138">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="cce18-139">Zacznijmy od utworzenia prostego modelu reprezentującego produkt.</span><span class="sxs-lookup"><span data-stu-id="cce18-139">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="cce18-140">Jeśli Eksplorator rozwiązań nie jest jeszcze widoczna, kliknij menu **Widok** i wybierz pozycję **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="cce18-140">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="cce18-141">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele.</span><span class="sxs-lookup"><span data-stu-id="cce18-141">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="cce18-142">Z menu kontekstowego wybierz pozycję **Dodaj** , a następnie wybierz pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="cce18-142">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="cce18-143">Nazwij klasę &quot;&quot;produktu.</span><span class="sxs-lookup"><span data-stu-id="cce18-143">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="cce18-144">Dodaj następujące właściwości do klasy `Product`.</span><span class="sxs-lookup"><span data-stu-id="cce18-144">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="cce18-145">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="cce18-145">Adding a Controller</span></span>

<span data-ttu-id="cce18-146">W interfejsie API sieci Web *kontroler* jest obiektem, który obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="cce18-146">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="cce18-147">Dodamy kontroler, który może zwrócić listę produktów lub pojedynczy produkt określony przez identyfikator.</span><span class="sxs-lookup"><span data-stu-id="cce18-147">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="cce18-148">Jeśli używasz platformy ASP.NET MVC, znasz już kontrolery.</span><span class="sxs-lookup"><span data-stu-id="cce18-148">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="cce18-149">Kontrolery internetowego interfejsu API są podobne do kontrolerów MVC, ale dziedziczą klasę **ApiController** zamiast klasy **Controller** .</span><span class="sxs-lookup"><span data-stu-id="cce18-149">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="cce18-150">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder controllers.</span><span class="sxs-lookup"><span data-stu-id="cce18-150">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="cce18-151">Wybierz pozycję **Dodaj** , a następnie wybierz pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="cce18-151">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="cce18-152">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler interfejsu API sieci Web — puste**.</span><span class="sxs-lookup"><span data-stu-id="cce18-152">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="cce18-153">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="cce18-153">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="cce18-154">W oknie dialogowym **Dodawanie kontrolera** nazwij kontroler &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="cce18-154">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="cce18-155">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="cce18-155">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="cce18-156">Funkcja tworzenia szkieletów utworzy plik o nazwie ProductsController.cs w folderze kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="cce18-156">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="cce18-157">Kontroler nie musi być umieszczany w folderze kontrolerów (Controllers).</span><span class="sxs-lookup"><span data-stu-id="cce18-157">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="cce18-158">Nazwy folderów ułatwiają tylko organizowanie plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="cce18-158">The folder name is just a convenient way to organize your source files.</span></span>

<span data-ttu-id="cce18-159">Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie, aby go otworzyć.</span><span class="sxs-lookup"><span data-stu-id="cce18-159">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="cce18-160">Zastąp kod w tym pliku zgodnie z poniższym przykładem:</span><span class="sxs-lookup"><span data-stu-id="cce18-160">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="cce18-161">W tym przykładzie w celu uproszczenia produkty są przechowywane w stałej tablicy wewnątrz klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="cce18-161">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="cce18-162">Oczywiście w rzeczywistej aplikacji przesyłane jest zapytanie do bazy danych lub innego zewnętrznego źródła danych.</span><span class="sxs-lookup"><span data-stu-id="cce18-162">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="cce18-163">Kontroler definiuje dwie metody, które zwracają produkty:</span><span class="sxs-lookup"><span data-stu-id="cce18-163">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="cce18-164">Metoda `GetAllProducts` zwraca całą listę produktów jako typ **&gt;produktu IEnumerable&lt;** .</span><span class="sxs-lookup"><span data-stu-id="cce18-164">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="cce18-165">Metoda `GetProduct` wyszukuje pojedynczy produkt według jego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="cce18-165">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="cce18-166">Gotowe.</span><span class="sxs-lookup"><span data-stu-id="cce18-166">That's it!</span></span> <span data-ttu-id="cce18-167">Masz działający internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="cce18-167">You have a working web API.</span></span> <span data-ttu-id="cce18-168">Każda metoda na kontrolerze odnosi się do co najmniej jednego identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="cce18-168">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="cce18-169">Controller — Metoda</span><span class="sxs-lookup"><span data-stu-id="cce18-169">Controller Method</span></span> | <span data-ttu-id="cce18-170">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="cce18-170">URI</span></span> |
| --- | --- |
| <span data-ttu-id="cce18-171">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="cce18-171">GetAllProducts</span></span> | <span data-ttu-id="cce18-172">/api/products</span><span class="sxs-lookup"><span data-stu-id="cce18-172">/api/products</span></span> |
| <span data-ttu-id="cce18-173">Getproduct</span><span class="sxs-lookup"><span data-stu-id="cce18-173">GetProduct</span></span> | <span data-ttu-id="cce18-174">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="cce18-174">/api/products/*id*</span></span> |

<span data-ttu-id="cce18-175">Dla metody `GetProduct` *Identyfikator* identyfikatora URI jest symbolem zastępczym.</span><span class="sxs-lookup"><span data-stu-id="cce18-175">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="cce18-176">Na przykład aby uzyskać produkt o IDENTYFIKATORze 5, identyfikator URI jest `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="cce18-176">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="cce18-177">Aby uzyskać więcej informacji o sposobie, w jaki interfejs API sieci Web kieruje żądania HTTP do metod kontrolera, zobacz [Routing in Web api ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="cce18-177">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="cce18-178">Wywoływanie interfejsu API sieci Web przy użyciu języków JavaScript i jQuery</span><span class="sxs-lookup"><span data-stu-id="cce18-178">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="cce18-179">W tej sekcji dodamy stronę HTML używającą technologii AJAX do wywołania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="cce18-179">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="cce18-180">Użyjemy technologii jQuery do wykonania wywołań AJAX oraz do zaktualizowania wyników na stronie.</span><span class="sxs-lookup"><span data-stu-id="cce18-180">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="cce18-181">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj**, a następnie wybierz pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="cce18-181">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="cce18-182">W oknie dialogowym **Dodaj nowy element** wybierz węzeł **sieci Web** w obszarze **Wizualizacja C#** , a następnie wybierz element **strony HTML** .</span><span class="sxs-lookup"><span data-stu-id="cce18-182">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="cce18-183">Nazwij stronę &quot;index. html&quot;.</span><span class="sxs-lookup"><span data-stu-id="cce18-183">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="cce18-184">Zamień zawartość pliku na następującą:</span><span class="sxs-lookup"><span data-stu-id="cce18-184">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="cce18-185">Istnieje kilka sposobów uzyskania biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="cce18-185">There are several ways to get jQuery.</span></span> <span data-ttu-id="cce18-186">W tym przykładzie użyto usługi [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="cce18-186">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="cce18-187">Można go również pobrać z [http://jquery.com/](http://jquery.com/), a szablon projektu "Web API" ASP.NET "również obejmuje jQuery.</span><span class="sxs-lookup"><span data-stu-id="cce18-187">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="cce18-188">Pobieranie listy produktów</span><span class="sxs-lookup"><span data-stu-id="cce18-188">Getting a List of Products</span></span>

<span data-ttu-id="cce18-189">Aby uzyskać listę produktów, Wyślij żądanie HTTP GET w celu &quot;&quot;/API/Products.</span><span class="sxs-lookup"><span data-stu-id="cce18-189">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="cce18-190">Funkcja jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) wysyła żądanie AJAX.</span><span class="sxs-lookup"><span data-stu-id="cce18-190">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="cce18-191">Odpowiedź zawiera tablicę obiektów JSON.</span><span class="sxs-lookup"><span data-stu-id="cce18-191">The response contains array of JSON objects.</span></span> <span data-ttu-id="cce18-192">Funkcja `done` określa wywołanie zwrotne, które jest wywoływane, jeśli żądanie zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="cce18-192">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="cce18-193">W wywołaniu zwrotnym aktualizujemy model DOM informacjami o produkcie.</span><span class="sxs-lookup"><span data-stu-id="cce18-193">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="cce18-194">Pobieranie produktu według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="cce18-194">Getting a Product By ID</span></span>

<span data-ttu-id="cce18-195">Aby uzyskać produkt według identyfikatora, Wyślij żądanie HTTP GET do &quot;*Identyfikator* /API/Products/&quot;, gdzie *ID* jest identyfikatorem produktu.</span><span class="sxs-lookup"><span data-stu-id="cce18-195">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="cce18-196">Nadal dzwonimy do `getJSON`, aby wysłać żądanie AJAX, ale ten czas jest umieszczany w IDENTYFIKATORze URI żądania.</span><span class="sxs-lookup"><span data-stu-id="cce18-196">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="cce18-197">Odpowiedź na to żądanie jest reprezentacją JSON jednego produktu.</span><span class="sxs-lookup"><span data-stu-id="cce18-197">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="cce18-198">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cce18-198">Running the Application</span></span>

<span data-ttu-id="cce18-199">Naciśnij klawisz F5, aby rozpocząć debugowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cce18-199">Press F5 to start debugging the application.</span></span> <span data-ttu-id="cce18-200">Strona internetowa powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="cce18-200">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="cce18-201">Aby uzyskać produkt według identyfikatora, wprowadź identyfikator i kliknij przycisk Wyszukaj:</span><span class="sxs-lookup"><span data-stu-id="cce18-201">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="cce18-202">W przypadku wprowadzenia nieprawidłowego identyfikatora serwer zwróci błąd HTTP:</span><span class="sxs-lookup"><span data-stu-id="cce18-202">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="cce18-203">Używanie klawisza F12 do wyświetlania żądania i odpowiedzi HTTP</span><span class="sxs-lookup"><span data-stu-id="cce18-203">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="cce18-204">Podczas pracy z usługą HTTP może być bardzo przydatne, aby wyświetlić żądania HTTP i komunikaty odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="cce18-204">When you are working with an HTTP service, it can be very useful to see the HTTP request and response messages.</span></span> <span data-ttu-id="cce18-205">Można to zrobić w programie Internet Explorer 9 przy użyciu narzędzi deweloperskich uruchamianych za pomocą klawisza F12.</span><span class="sxs-lookup"><span data-stu-id="cce18-205">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="cce18-206">W programie Internet Explorer 9 Naciśnij klawisz **F12** , aby otworzyć narzędzia.</span><span class="sxs-lookup"><span data-stu-id="cce18-206">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="cce18-207">Kliknij kartę **Sieć** , a następnie naciśnij pozycję **Rozpocznij przechwytywanie**.</span><span class="sxs-lookup"><span data-stu-id="cce18-207">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="cce18-208">Teraz wróć do strony sieci Web, a następnie naciśnij klawisz **F5** , aby ponownie załadować stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cce18-208">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="cce18-209">Program Internet Explorer będzie przechwytywać ruch HTTP między przeglądarką a serwerem internetowym.</span><span class="sxs-lookup"><span data-stu-id="cce18-209">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="cce18-210">Widok podsumowania pokazuje cały ruch sieciowy dla strony:</span><span class="sxs-lookup"><span data-stu-id="cce18-210">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="cce18-211">Zlokalizuj wpis dla względnego identyfikatora URI "api/products//".</span><span class="sxs-lookup"><span data-stu-id="cce18-211">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="cce18-212">Wybierz ten wpis, a następnie kliknij pozycję **Przejdź do widoku szczegółowego**.</span><span class="sxs-lookup"><span data-stu-id="cce18-212">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="cce18-213">Widok szczegółowy zawiera karty, na których są wyświetlane nagłówki i zawartość żądań oraz odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="cce18-213">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="cce18-214">Na przykład po kliknięciu karty **nagłówki żądań** można zobaczyć, że klient zażądał &quot;Application/JSON&quot; w nagłówku Accept.</span><span class="sxs-lookup"><span data-stu-id="cce18-214">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="cce18-215">Po kliknięciu karty Treść odpowiedzi możesz zobaczyć, jak lista produktów została zserializowana do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="cce18-215">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="cce18-216">Inne przeglądarki mają podobną funkcję.</span><span class="sxs-lookup"><span data-stu-id="cce18-216">Other browsers have similar functionality.</span></span> <span data-ttu-id="cce18-217">Innym przydatnym narzędziem jest [programu Fiddler](http://www.fiddler2.com/fiddler2/), serwer proxy debugowania sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cce18-217">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="cce18-218">Służy on do wyświetlania ruchu HTTP, a także do tworzenia żądań HTTP, co daje pełną kontrolę nad nagłówkami HTTP w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="cce18-218">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="cce18-219">Wyświetlanie aplikacji działającej na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="cce18-219">See this App Running on Azure</span></span>

<span data-ttu-id="cce18-220">Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo?</span><span class="sxs-lookup"><span data-stu-id="cce18-220">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="cce18-221">Pełną wersję aplikacji możesz wdrożyć na koncie platformy Azure — wystarczy kliknąć poniższy przycisk.</span><span class="sxs-lookup"><span data-stu-id="cce18-221">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="cce18-222">Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="cce18-222">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="cce18-223">Jeśli nie masz jeszcze konta, możesz:</span><span class="sxs-lookup"><span data-stu-id="cce18-223">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="cce18-224">[Bezpłatnie Otwórz konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — Uzyskaj kredyty, których możesz użyć do wypróbowania płatnych usług platformy Azure, a nawet po ich użyciu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="cce18-224">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="cce18-225">[Aktywuj korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) — subskrypcja MSDN daje środki na korzystanie z płatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="cce18-225">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cce18-226">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="cce18-226">Next Steps</span></span>

- <span data-ttu-id="cce18-227">Aby zapoznać się z bardziej kompletnym przykładem usługi HTTP, która obsługuje akcje POST, PUT i DELETE oraz zapisywać dane w bazie danych, zobacz [Używanie interfejsu Web API 2 z Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="cce18-227">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="cce18-228">Aby uzyskać więcej informacji na temat tworzenia płynnych i odpowiadających aplikacji sieci Web na podstawie usługi HTTP, zobacz [ASP.NET aplikacji jednostronicowej](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="cce18-228">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="cce18-229">Aby uzyskać informacje na temat sposobu wdrażania projektu sieci Web programu Visual Studio w celu Azure App Service, zobacz [Tworzenie aplikacji sieci web ASP.NET w Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="cce18-229">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
