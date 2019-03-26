---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Włączanie operacji CRUD we wzorcu ASP.NET Web API 1 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku pokazano, jak do obsługi operacji CRUD w usłudze HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET. Wersje oprogramowania używanych w samouczek Visual Studio 2012 Web AP...
ms.author: riande
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: f3cb0004075ef7687ca1096bd407c342b4d0b7be
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58423758"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="0c9d0-104">Włączanie operacji CRUD we wzorcu ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="0c9d0-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="0c9d0-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0c9d0-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0c9d0-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="0c9d0-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="0c9d0-107">W tym samouczku pokazano, jak do obsługi operacji CRUD w usłudze HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0c9d0-108">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="0c9d0-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0c9d0-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="0c9d0-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="0c9d0-110">Składnik Web API 1 (dotyczy również Web API 2)</span><span class="sxs-lookup"><span data-stu-id="0c9d0-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="0c9d0-111">Oznacza CRUD &quot;tworzenia, odczytu, aktualizacji i usuwania,&quot; służą do czterech operacji podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="0c9d0-112">Wiele usług HTTP również model operacji CRUD, za pośrednictwem REST lub interfejsów API REST podobne.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="0c9d0-113">W tym samouczku utworzysz bardzo prosty internetowego interfejsu API do zarządzania listą produktów.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="0c9d0-114">Każdy produkt będzie zawierać nazwę, cenę i kategorii (takich jak &quot;zabawki&quot; lub &quot;sprzętu&quot;), oraz identyfikator produktu.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="0c9d0-115">Produkty interfejsu API powoduje to udostępnienie następujących metod.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="0c9d0-116">Akcja</span><span class="sxs-lookup"><span data-stu-id="0c9d0-116">Action</span></span> | <span data-ttu-id="0c9d0-117">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="0c9d0-117">HTTP method</span></span> | <span data-ttu-id="0c9d0-118">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="0c9d0-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0c9d0-119">Pobierz listę wszystkich produktów</span><span class="sxs-lookup"><span data-stu-id="0c9d0-119">Get a list of all products</span></span> | <span data-ttu-id="0c9d0-120">GET</span><span class="sxs-lookup"><span data-stu-id="0c9d0-120">GET</span></span> | <span data-ttu-id="0c9d0-121">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="0c9d0-121">/api/products</span></span> |
| <span data-ttu-id="0c9d0-122">Pobieranie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="0c9d0-122">Get a product by ID</span></span> | <span data-ttu-id="0c9d0-123">GET</span><span class="sxs-lookup"><span data-stu-id="0c9d0-123">GET</span></span> | <span data-ttu-id="0c9d0-124">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="0c9d0-124">/api/products/*id*</span></span> |
| <span data-ttu-id="0c9d0-125">Pobierz produkt według kategorii</span><span class="sxs-lookup"><span data-stu-id="0c9d0-125">Get a product by category</span></span> | <span data-ttu-id="0c9d0-126">GET</span><span class="sxs-lookup"><span data-stu-id="0c9d0-126">GET</span></span> | <span data-ttu-id="0c9d0-127">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="0c9d0-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="0c9d0-128">Tworzenie nowego produktu</span><span class="sxs-lookup"><span data-stu-id="0c9d0-128">Create a new product</span></span> | <span data-ttu-id="0c9d0-129">POST</span><span class="sxs-lookup"><span data-stu-id="0c9d0-129">POST</span></span> | <span data-ttu-id="0c9d0-130">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="0c9d0-130">/api/products</span></span> |
| <span data-ttu-id="0c9d0-131">Aktualizacja produktu</span><span class="sxs-lookup"><span data-stu-id="0c9d0-131">Update a product</span></span> | <span data-ttu-id="0c9d0-132">PUT</span><span class="sxs-lookup"><span data-stu-id="0c9d0-132">PUT</span></span> | <span data-ttu-id="0c9d0-133">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="0c9d0-133">/api/products/*id*</span></span> |
| <span data-ttu-id="0c9d0-134">Usuwanie produktu</span><span class="sxs-lookup"><span data-stu-id="0c9d0-134">Delete a product</span></span> | <span data-ttu-id="0c9d0-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="0c9d0-135">DELETE</span></span> | <span data-ttu-id="0c9d0-136">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="0c9d0-136">/api/products/*id*</span></span> |

<span data-ttu-id="0c9d0-137">Należy zauważyć, że niektóre identyfikatory URI, Dołącz identyfikator produktu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="0c9d0-138">Na przykład, aby uzyskać produkt o identyfikatorze to 28, klient wysyła żądanie GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="0c9d0-139">Zasoby</span><span class="sxs-lookup"><span data-stu-id="0c9d0-139">Resources</span></span>

<span data-ttu-id="0c9d0-140">Produkty API definiuje identyfikatorów URI dla dwóch typów zasobów:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="0c9d0-141">Zasób</span><span class="sxs-lookup"><span data-stu-id="0c9d0-141">Resource</span></span> | <span data-ttu-id="0c9d0-142">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="0c9d0-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="0c9d0-143">Lista wszystkich produktów.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-143">The list of all the products.</span></span> | <span data-ttu-id="0c9d0-144">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="0c9d0-144">/api/products</span></span> |
| <span data-ttu-id="0c9d0-145">Indywidualnych produktów.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-145">An individual product.</span></span> | <span data-ttu-id="0c9d0-146">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="0c9d0-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="0c9d0-147">Metody</span><span class="sxs-lookup"><span data-stu-id="0c9d0-147">Methods</span></span>

<span data-ttu-id="0c9d0-148">Cztery główne metody HTTP (GET, PUT, POST i DELETE) mogą być mapowane na operacje CRUD w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="0c9d0-149">GET pobiera reprezentację zasobu o wskazanym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="0c9d0-150">Pobierz powinien mieć żadnych efektów ubocznych, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="0c9d0-151">Umieść aktualizuje zasób o wskazanym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="0c9d0-152">PUT można również utworzyć nowy zasób o określonym identyfikatorze URI, jeśli serwer umożliwia klientom określić nowe identyfikatory URI.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="0c9d0-153">W tym samouczku interfejs API nie obsługuje tworzenia za pomocą PUT.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="0c9d0-154">POST utworzy nowy zasób.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-154">POST creates a new resource.</span></span> <span data-ttu-id="0c9d0-155">Serwer przypisuje identyfikator URI dla nowego obiektu i zwraca ten identyfikator URI jako część komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="0c9d0-156">DELETE Usuwa zasób o wskazanym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="0c9d0-157">Uwaga: Metody PUT zastępuje jednostki w całym produkcie.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="0c9d0-158">Oznacza to, że klient oczekuje się, Wyślij pełną reprezentację zaktualizowanego produktu.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="0c9d0-159">Jeśli chcesz obsługiwać aktualizacje częściowe, metoda PATCH jest preferowana.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="0c9d0-160">W tym samouczku nie implementuje poprawki.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="0c9d0-161">Utwórz nowy projekt interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="0c9d0-161">Create a New Web API Project</span></span>

<span data-ttu-id="0c9d0-162">Uruchom za pomocą programu Visual Studio i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="0c9d0-163">Możesz również z menu **Plik** wybrać pozycję **Nowy**, a następnie **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="0c9d0-164">W okienku **Szablony** wybierz pozycję **Zainstalowane szablony** i rozwiń węzeł **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="0c9d0-165">W obszarze **Visual C#**, wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="0c9d0-166">Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="0c9d0-167">Nadaj projektowi nazwę &quot;ProductStore&quot; i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="0c9d0-168">W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **interfejsu API sieci Web** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="0c9d0-169">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="0c9d0-169">Adding a Model</span></span>

<span data-ttu-id="0c9d0-170">*Model* jest obiektem, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="0c9d0-171">W programie ASP.NET Web API silnie typizowanych obiektów CLR można użyć jako modeli, a ich będzie automatycznie serializowany do XML lub JSON dla klienta.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="0c9d0-172">Dla interfejsu API ProductStore naszych danych składa się z produktów, dlatego utworzymy nową klasę o nazwie `Product`.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="0c9d0-173">Jeśli Eksplorator rozwiązań nie jest jeszcze widoczny, kliknij menu **Widok**, a następnie wybierz pozycję **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="0c9d0-174">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** folderu.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="0c9d0-175">Wybierz z menu kontekstowego, **Dodaj**, a następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="0c9d0-176">Nazwij klasę &quot;Product&quot;.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="0c9d0-177">Dodaj następujące właściwości do `Product` klasy.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="0c9d0-178">Dodawanie repozytorium</span><span class="sxs-lookup"><span data-stu-id="0c9d0-178">Adding a Repository</span></span>

<span data-ttu-id="0c9d0-179">Musimy zapisują zbiór produktów.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-179">We need to store a collection of products.</span></span> <span data-ttu-id="0c9d0-180">To dobry pomysł, aby oddzielić kolekcji z naszej implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="0c9d0-181">W ten sposób możemy zmienić magazyn zapasowy bez konieczności ponownego zapisu klasę usługi.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="0c9d0-182">Tego typu konstrukcji jest nazywana *repozytorium* wzorca.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="0c9d0-183">Rozpocznij, definiując ogólny interfejs umożliwiający repozytorium.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="0c9d0-184">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** folderu.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="0c9d0-185">Wybierz **Dodaj**, a następnie wybierz **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="0c9d0-186">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń węzeł języka C#.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="0c9d0-187">W języku C#, wybierz **kodu**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-187">Under C#, select **Code**.</span></span> <span data-ttu-id="0c9d0-188">Na liście szablonów kodu Wybierz **interfejsu**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="0c9d0-189">Nazwa interfejsu &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="0c9d0-190">Dodaj następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="0c9d0-191">Teraz Dodaj klasę do folderu modeli, o nazwie &quot;ProductRepository.&quot; Ta klasa wdroży `IProductRepository` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="0c9d0-192">Dodaj następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="0c9d0-193">Repozytorium przechowuje listy w lokalnej pamięci.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="0c9d0-194">Dotyczy to OK, aby zapoznać się z samouczkiem, ale w rzeczywistej aplikacji będzie przechowywania danych zewnętrznie, albo bazę danych lub w magazynie w chmurze.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="0c9d0-195">Wzorzec repozytorium ułatwi Zmień implementację później.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="0c9d0-196">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="0c9d0-196">Adding a Web API Controller</span></span>

<span data-ttu-id="0c9d0-197">Użytkownicy mający doświadczenie z platformą ASP.NET MVC, następnie znasz już kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="0c9d0-198">W programie ASP.NET Web API *kontrolera* to klasa, która obsługuje żądania HTTP od klienta.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="0c9d0-199">Kreator nowego projektu utworzone dwa kontrolery automatycznie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="0c9d0-200">Aby je wyświetlić, rozwiń folder kontrolerów w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="0c9d0-201">HomeController to tradycyjne kontrolera ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="0c9d0-202">Jest odpowiedzialny za obsługi stron HTML dla witryny, a nie jest bezpośrednio związana z naszego interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="0c9d0-203">ValuesController jest przykład controller WebAPI.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="0c9d0-204">Przejdź dalej i Usuń ValuesController, klikając prawym przyciskiem myszy plik w Eksploratorze rozwiązań i wybierając polecenie **usunąć.**</span><span class="sxs-lookup"><span data-stu-id="0c9d0-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="0c9d0-205">Teraz Dodaj nowy kontroler, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="0c9d0-206">W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy folder kontrolerów (Controllers).</span><span class="sxs-lookup"><span data-stu-id="0c9d0-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="0c9d0-207">Wybierz pozycję **Dodaj**, a następnie **Kontroler**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="0c9d0-208">W **Dodaj kontroler** kreatora, nazwy kontrolera &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="0c9d0-209">W **szablonu** listy rozwijanej wybierz **pusty Kontroler interfejsu API**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="0c9d0-210">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="0c9d0-211">Nie jest konieczne kontrolerach należy umieścić w folderze o nazwie kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-211">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="0c9d0-212">Nazwa folderu nie jest ważna; jest po prostu wygodny sposób organizowania plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="0c9d0-213">**Dodaj kontroler** Kreator utworzy plik o nazwie ProductsController.cs w folderze kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="0c9d0-214">Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie, aby go otworzyć.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="0c9d0-215">Dodaj następujący kod **przy użyciu** instrukcji:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="0c9d0-216">Dodaj pola zawierające **IProductRepository** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="0c9d0-217">Wywoływanie `new ProductRepository()` w kontrolerze nie jest najlepszy projekt, ponieważ wiąże kontroler konkretnej implementacji `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="0c9d0-218">Aby uzyskać lepszym rozwiązaniem, zobacz [za pomocą mechanizmu rozpoznawania zależności dla interfejsu API sieci Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="0c9d0-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="0c9d0-219">Wprowadzenie do zasobu</span><span class="sxs-lookup"><span data-stu-id="0c9d0-219">Getting a Resource</span></span>

<span data-ttu-id="0c9d0-220">Interfejs API ProductStore udostępni kilka &quot;odczytu&quot; czynności, jak metod HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="0c9d0-221">Każda akcja odnoszą się do metody w `ProductsController` klasy.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="0c9d0-222">Akcja</span><span class="sxs-lookup"><span data-stu-id="0c9d0-222">Action</span></span> | <span data-ttu-id="0c9d0-223">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="0c9d0-223">HTTP method</span></span> | <span data-ttu-id="0c9d0-224">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="0c9d0-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0c9d0-225">Pobierz listę wszystkich produktów</span><span class="sxs-lookup"><span data-stu-id="0c9d0-225">Get a list of all products</span></span> | <span data-ttu-id="0c9d0-226">GET</span><span class="sxs-lookup"><span data-stu-id="0c9d0-226">GET</span></span> | <span data-ttu-id="0c9d0-227">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="0c9d0-227">/api/products</span></span> |
| <span data-ttu-id="0c9d0-228">Pobieranie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="0c9d0-228">Get a product by ID</span></span> | <span data-ttu-id="0c9d0-229">GET</span><span class="sxs-lookup"><span data-stu-id="0c9d0-229">GET</span></span> | <span data-ttu-id="0c9d0-230">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="0c9d0-230">/api/products/*id*</span></span> |
| <span data-ttu-id="0c9d0-231">Pobierz produkt według kategorii</span><span class="sxs-lookup"><span data-stu-id="0c9d0-231">Get a product by category</span></span> | <span data-ttu-id="0c9d0-232">GET</span><span class="sxs-lookup"><span data-stu-id="0c9d0-232">GET</span></span> | <span data-ttu-id="0c9d0-233">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="0c9d0-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="0c9d0-234">Aby uzyskać listę wszystkich produktów, należy dodać tę metodę w celu `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="0c9d0-235">Nazwa metody rozpoczyna się od &quot;uzyskać&quot;, więc zgodnie z Konwencją, jest on mapowany na żądania GET.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="0c9d0-236">Ponadto, ponieważ metoda nie ma parametrów, jest on mapowany do identyfikatora URI, który nie zawiera *&quot;identyfikator&quot;* segmentów w ścieżce.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="0c9d0-237">Aby uzyskać produkt za pomocą Identyfikatora, należy dodać tę metodę w celu `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="0c9d0-238">Ta nazwa metody również zaczyna się od &quot;uzyskać&quot;, ale metoda ma parametr o nazwie *identyfikator*. Ten parametr jest mapowany na &quot;identyfikator&quot; segmentu ścieżki identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="0c9d0-239">Struktury ASP.NET Web API automatycznie konwertuje identyfikator na prawidłowy typ danych (**int**) dla parametru.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="0c9d0-240">Metoda GetProduct zgłasza wyjątek typu **HttpResponseException** Jeśli *identyfikator* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="0c9d0-241">Wyjątek ten będzie można przetłumaczyć przez platformę błąd 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="0c9d0-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="0c9d0-242">Na koniec Dodaj metodę, aby znaleźć produkty według kategorii:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="0c9d0-243">Jeśli identyfikator URI żądania zawiera ciąg zapytania, interfejs API sieci Web próbuje dopasować parametry zapytania do parametrów dla metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="0c9d0-244">W związku z tym, identyfikator URI w postaci "interfejsu api/produktów? kategorii =*kategorii*" będzie zmapowana do tej metody.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="0c9d0-245">Tworzenie zasobu</span><span class="sxs-lookup"><span data-stu-id="0c9d0-245">Creating a Resource</span></span>

<span data-ttu-id="0c9d0-246">Następnie dodamy metodę `ProductsController` klasy w celu utworzenia nowego produktu.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="0c9d0-247">Poniżej przedstawiono prosty implementacji metody:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="0c9d0-248">Należy zwrócić uwagę dwa elementy o tej metodzie:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-248">Note two things about this method:</span></span>

- <span data-ttu-id="0c9d0-249">Nazwa metody rozpoczyna się od &quot;wpis... &quot;.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="0c9d0-250">Aby utworzyć nowy produkt, klient wysyła żądanie HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="0c9d0-251">Ta metoda przyjmuje parametr typu produktu.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="0c9d0-252">W interfejsie API sieci Web parametrów o złożonych typach są deserializacji z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="0c9d0-253">W związku z tym oczekujemy, że to klientowi wysłanie zserializowana reprezentacja obiektu produktu w formacie XML lub JSON.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="0c9d0-254">Ta implementacja będzie działać, ale nie jest jeszcze ukończone.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="0c9d0-255">W idealnym przypadku prosimy o poświęcenie odpowiedzi HTTP są następujące:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="0c9d0-256">**Kod odpowiedzi:** Domyślnie struktury Web API ustawia kod stanu odpowiedzi 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="0c9d0-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="0c9d0-257">Jednak zgodnie z protokołem HTTP/1.1, jeśli żądanie POST prowadzi do utworzenia zasobu, serwer powinien odpowiadać ze stanem 201 (utworzono).</span><span class="sxs-lookup"><span data-stu-id="0c9d0-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="0c9d0-258">**Lokalizacja:** Gdy serwer umożliwia utworzenie zasobu, powinien on zawierać identyfikator URI nowego zasobu w nagłówku Location odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="0c9d0-259">ASP.NET Web API można łatwo manipulować komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="0c9d0-260">Oto implementacja ulepszone:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="0c9d0-261">Należy zauważyć, że typ zwracany metody jest teraz **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="0c9d0-262">Zwracając **obiektu HttpResponseMessage** zamiast produktu, możemy kontrolować szczegóły komunikatu odpowiedzi HTTP, łącznie z kodem stanu i nagłówek Location.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="0c9d0-263">**CreateResponse** metoda tworzy **obiektu HttpResponseMessage** i automatycznie zapisuje zserializowana reprezentacja obiektu produktu w treści fo komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="0c9d0-264">Nie można zweryfikować w tym przykładzie `Product`.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="0c9d0-265">Aby uzyskać informacji o weryfikacji modelu, zobacz [weryfikacji modelu programu ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="0c9d0-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="0c9d0-266">Aktualizowanie zasobu</span><span class="sxs-lookup"><span data-stu-id="0c9d0-266">Updating a Resource</span></span>

<span data-ttu-id="0c9d0-267">Aktualizowanie produktu za pomocą PUT jest proste:</span><span class="sxs-lookup"><span data-stu-id="0c9d0-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="0c9d0-268">Nazwa metody rozpoczyna się od &quot;umieścić... &quot;, więc internetowego interfejsu API pasujących do żądania PUT.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="0c9d0-269">Ta metoda przyjmuje dwa parametry: identyfikator produktu i zaktualizowanych produktów.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="0c9d0-270">*Identyfikator* parametru jest pobierana z ścieżka identyfikatora URI i *produktu* parametru jest przeprowadzona z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="0c9d0-271">Domyślnie w ramach interfejsu API sieci Web platformy ASP.NET tworzy typy proste parametrów na podstawie trasy i typów złożonych z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="0c9d0-272">Usuwanie zasobu</span><span class="sxs-lookup"><span data-stu-id="0c9d0-272">Deleting a Resource</span></span>

<span data-ttu-id="0c9d0-273">Aby usunąć zasób, należy zdefiniować "Usuń..." Metoda.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-273">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="0c9d0-274">Jeśli żądanie usunięcia zakończy się powodzeniem, może zwrócić stanu 200 (OK) z treści jednostki opisującą stan; Stan 202 (zaakceptowano), jeśli usunięcie jest nadal oczekujące; Stan lub 204 (Brak zawartości) z bez treści jednostki.</span><span class="sxs-lookup"><span data-stu-id="0c9d0-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="0c9d0-275">W tym przypadku `DeleteProduct` metoda ma `void` zwracany typ, więc ASP.NET Web API automatycznie przekłada to stan kod 204 (Brak zawartości).</span><span class="sxs-lookup"><span data-stu-id="0c9d0-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
