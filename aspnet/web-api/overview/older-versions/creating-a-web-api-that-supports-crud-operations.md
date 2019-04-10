---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Włączanie operacji CRUD we wzorcu ASP.NET Web API 1 - ASP.NET 4.x
author: MikeWasson
description: Samouczek przedstawia sposób obsługi operacji CRUD usługi HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET dla aplikacji ASP.NET 4.x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 855c3fa35d82173c87d13adb51e10fd13698ade5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381357"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="2faf1-103">Włączanie operacji CRUD we wzorcu ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="2faf1-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="2faf1-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2faf1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2faf1-105">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="2faf1-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="2faf1-106">W tym samouczku pokazano, jak do obsługi operacji CRUD w usłudze HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET dla platformy ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="2faf1-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2faf1-107">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="2faf1-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2faf1-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2faf1-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="2faf1-109">Składnik Web API 1 (dotyczy również Web API 2)</span><span class="sxs-lookup"><span data-stu-id="2faf1-109">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="2faf1-110">Oznacza CRUD &quot;tworzenia, odczytu, aktualizacji i usuwania,&quot; służą do czterech operacji podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2faf1-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="2faf1-111">Wiele usług HTTP również model operacji CRUD, za pośrednictwem REST lub interfejsów API REST podobne.</span><span class="sxs-lookup"><span data-stu-id="2faf1-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="2faf1-112">W tym samouczku utworzysz bardzo prosty internetowego interfejsu API do zarządzania listą produktów.</span><span class="sxs-lookup"><span data-stu-id="2faf1-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="2faf1-113">Każdy produkt będzie zawierać nazwę, cenę i kategorii (takich jak &quot;zabawki&quot; lub &quot;sprzętu&quot;), oraz identyfikator produktu.</span><span class="sxs-lookup"><span data-stu-id="2faf1-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="2faf1-114">Produkty interfejsu API powoduje to udostępnienie następujących metod.</span><span class="sxs-lookup"><span data-stu-id="2faf1-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="2faf1-115">Akcja</span><span class="sxs-lookup"><span data-stu-id="2faf1-115">Action</span></span> | <span data-ttu-id="2faf1-116">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2faf1-116">HTTP method</span></span> | <span data-ttu-id="2faf1-117">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="2faf1-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2faf1-118">Pobierz listę wszystkich produktów</span><span class="sxs-lookup"><span data-stu-id="2faf1-118">Get a list of all products</span></span> | <span data-ttu-id="2faf1-119">GET</span><span class="sxs-lookup"><span data-stu-id="2faf1-119">GET</span></span> | <span data-ttu-id="2faf1-120">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="2faf1-120">/api/products</span></span> |
| <span data-ttu-id="2faf1-121">Pobieranie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="2faf1-121">Get a product by ID</span></span> | <span data-ttu-id="2faf1-122">GET</span><span class="sxs-lookup"><span data-stu-id="2faf1-122">GET</span></span> | <span data-ttu-id="2faf1-123">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="2faf1-123">/api/products/*id*</span></span> |
| <span data-ttu-id="2faf1-124">Pobierz produkt według kategorii</span><span class="sxs-lookup"><span data-stu-id="2faf1-124">Get a product by category</span></span> | <span data-ttu-id="2faf1-125">GET</span><span class="sxs-lookup"><span data-stu-id="2faf1-125">GET</span></span> | <span data-ttu-id="2faf1-126">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="2faf1-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="2faf1-127">Tworzenie nowego produktu</span><span class="sxs-lookup"><span data-stu-id="2faf1-127">Create a new product</span></span> | <span data-ttu-id="2faf1-128">POST</span><span class="sxs-lookup"><span data-stu-id="2faf1-128">POST</span></span> | <span data-ttu-id="2faf1-129">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="2faf1-129">/api/products</span></span> |
| <span data-ttu-id="2faf1-130">Aktualizacja produktu</span><span class="sxs-lookup"><span data-stu-id="2faf1-130">Update a product</span></span> | <span data-ttu-id="2faf1-131">PUT</span><span class="sxs-lookup"><span data-stu-id="2faf1-131">PUT</span></span> | <span data-ttu-id="2faf1-132">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="2faf1-132">/api/products/*id*</span></span> |
| <span data-ttu-id="2faf1-133">Usuwanie produktu</span><span class="sxs-lookup"><span data-stu-id="2faf1-133">Delete a product</span></span> | <span data-ttu-id="2faf1-134">DELETE</span><span class="sxs-lookup"><span data-stu-id="2faf1-134">DELETE</span></span> | <span data-ttu-id="2faf1-135">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="2faf1-135">/api/products/*id*</span></span> |

<span data-ttu-id="2faf1-136">Należy zauważyć, że niektóre identyfikatory URI, Dołącz identyfikator produktu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="2faf1-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="2faf1-137">Na przykład, aby uzyskać produkt o identyfikatorze to 28, klient wysyła żądanie GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="2faf1-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="2faf1-138">Zasoby</span><span class="sxs-lookup"><span data-stu-id="2faf1-138">Resources</span></span>

<span data-ttu-id="2faf1-139">Produkty API definiuje identyfikatorów URI dla dwóch typów zasobów:</span><span class="sxs-lookup"><span data-stu-id="2faf1-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="2faf1-140">Zasób</span><span class="sxs-lookup"><span data-stu-id="2faf1-140">Resource</span></span> | <span data-ttu-id="2faf1-141">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="2faf1-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="2faf1-142">Lista wszystkich produktów.</span><span class="sxs-lookup"><span data-stu-id="2faf1-142">The list of all the products.</span></span> | <span data-ttu-id="2faf1-143">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="2faf1-143">/api/products</span></span> |
| <span data-ttu-id="2faf1-144">Indywidualnych produktów.</span><span class="sxs-lookup"><span data-stu-id="2faf1-144">An individual product.</span></span> | <span data-ttu-id="2faf1-145">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="2faf1-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="2faf1-146">Metody</span><span class="sxs-lookup"><span data-stu-id="2faf1-146">Methods</span></span>

<span data-ttu-id="2faf1-147">Cztery główne metody HTTP (GET, PUT, POST i DELETE) mogą być mapowane na operacje CRUD w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2faf1-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="2faf1-148">GET pobiera reprezentację zasobu o wskazanym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="2faf1-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="2faf1-149">Pobierz powinien mieć żadnych efektów ubocznych, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2faf1-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="2faf1-150">Umieść aktualizuje zasób o wskazanym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="2faf1-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="2faf1-151">PUT można również utworzyć nowy zasób o określonym identyfikatorze URI, jeśli serwer umożliwia klientom określić nowe identyfikatory URI.</span><span class="sxs-lookup"><span data-stu-id="2faf1-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="2faf1-152">W tym samouczku interfejs API nie obsługuje tworzenia za pomocą PUT.</span><span class="sxs-lookup"><span data-stu-id="2faf1-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="2faf1-153">POST utworzy nowy zasób.</span><span class="sxs-lookup"><span data-stu-id="2faf1-153">POST creates a new resource.</span></span> <span data-ttu-id="2faf1-154">Serwer przypisuje identyfikator URI dla nowego obiektu i zwraca ten identyfikator URI jako część komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2faf1-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="2faf1-155">DELETE Usuwa zasób o wskazanym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="2faf1-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="2faf1-156">Uwaga: Metody PUT zastępuje jednostki w całym produkcie.</span><span class="sxs-lookup"><span data-stu-id="2faf1-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="2faf1-157">Oznacza to, że klient oczekuje się, Wyślij pełną reprezentację zaktualizowanego produktu.</span><span class="sxs-lookup"><span data-stu-id="2faf1-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="2faf1-158">Jeśli chcesz obsługiwać aktualizacje częściowe, metoda PATCH jest preferowana.</span><span class="sxs-lookup"><span data-stu-id="2faf1-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="2faf1-159">W tym samouczku nie implementuje poprawki.</span><span class="sxs-lookup"><span data-stu-id="2faf1-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="2faf1-160">Utwórz nowy projekt interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="2faf1-160">Create a New Web API Project</span></span>

<span data-ttu-id="2faf1-161">Uruchom za pomocą programu Visual Studio i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="2faf1-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2faf1-162">Możesz również z menu **Plik** wybrać pozycję **Nowy**, a następnie **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2faf1-163">W okienku **Szablony** wybierz pozycję **Zainstalowane szablony** i rozwiń węzeł **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2faf1-164">W obszarze **Visual C#**, wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="2faf1-165">Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="2faf1-166">Nadaj projektowi nazwę &quot;ProductStore&quot; i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="2faf1-167">W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **interfejsu API sieci Web** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="2faf1-168">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="2faf1-168">Adding a Model</span></span>

<span data-ttu-id="2faf1-169">*Model* jest obiektem, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2faf1-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="2faf1-170">W programie ASP.NET Web API silnie typizowanych obiektów CLR można użyć jako modeli, a ich będzie automatycznie serializowany do XML lub JSON dla klienta.</span><span class="sxs-lookup"><span data-stu-id="2faf1-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="2faf1-171">Dla interfejsu API ProductStore naszych danych składa się z produktów, dlatego utworzymy nową klasę o nazwie `Product`.</span><span class="sxs-lookup"><span data-stu-id="2faf1-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="2faf1-172">Jeśli Eksplorator rozwiązań nie jest jeszcze widoczny, kliknij menu **Widok**, a następnie wybierz pozycję **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="2faf1-173">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** folderu.</span><span class="sxs-lookup"><span data-stu-id="2faf1-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="2faf1-174">Z menu kontekstowego wybierz **Dodaj**, a następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="2faf1-175">Nazwij klasę &quot;Product&quot;.</span><span class="sxs-lookup"><span data-stu-id="2faf1-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="2faf1-176">Dodaj następujące właściwości do `Product` klasy.</span><span class="sxs-lookup"><span data-stu-id="2faf1-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="2faf1-177">Dodawanie repozytorium</span><span class="sxs-lookup"><span data-stu-id="2faf1-177">Adding a Repository</span></span>

<span data-ttu-id="2faf1-178">Musimy zapisują zbiór produktów.</span><span class="sxs-lookup"><span data-stu-id="2faf1-178">We need to store a collection of products.</span></span> <span data-ttu-id="2faf1-179">To dobry pomysł, aby oddzielić kolekcji z naszej implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="2faf1-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="2faf1-180">W ten sposób możemy zmienić magazyn zapasowy bez konieczności ponownego zapisu klasę usługi.</span><span class="sxs-lookup"><span data-stu-id="2faf1-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="2faf1-181">Tego typu konstrukcji jest nazywana *repozytorium* wzorca.</span><span class="sxs-lookup"><span data-stu-id="2faf1-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="2faf1-182">Rozpocznij, definiując ogólny interfejs umożliwiający repozytorium.</span><span class="sxs-lookup"><span data-stu-id="2faf1-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="2faf1-183">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** folderu.</span><span class="sxs-lookup"><span data-stu-id="2faf1-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="2faf1-184">Wybierz **Dodaj**, a następnie wybierz **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="2faf1-185">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń węzeł języka C#.</span><span class="sxs-lookup"><span data-stu-id="2faf1-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="2faf1-186">W języku C#, wybierz **kodu**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-186">Under C#, select **Code**.</span></span> <span data-ttu-id="2faf1-187">Na liście szablonów kodu Wybierz **interfejsu**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="2faf1-188">Nazwa interfejsu &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="2faf1-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="2faf1-189">Dodaj następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="2faf1-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="2faf1-190">Teraz Dodaj klasę do folderu modeli, o nazwie &quot;ProductRepository.&quot; Ta klasa wdroży `IProductRepository` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="2faf1-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="2faf1-191">Dodaj następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="2faf1-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="2faf1-192">Repozytorium przechowuje listy w lokalnej pamięci.</span><span class="sxs-lookup"><span data-stu-id="2faf1-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="2faf1-193">Dotyczy to OK, aby zapoznać się z samouczkiem, ale w rzeczywistej aplikacji będzie przechowywania danych zewnętrznie, albo bazę danych lub w magazynie w chmurze.</span><span class="sxs-lookup"><span data-stu-id="2faf1-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="2faf1-194">Wzorzec repozytorium ułatwi Zmień implementację później.</span><span class="sxs-lookup"><span data-stu-id="2faf1-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="2faf1-195">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="2faf1-195">Adding a Web API Controller</span></span>

<span data-ttu-id="2faf1-196">Użytkownicy mający doświadczenie z platformą ASP.NET MVC, następnie znasz już kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="2faf1-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="2faf1-197">W programie ASP.NET Web API *kontrolera* to klasa, która obsługuje żądania HTTP od klienta.</span><span class="sxs-lookup"><span data-stu-id="2faf1-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="2faf1-198">Kreator nowego projektu utworzone dwa kontrolery automatycznie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="2faf1-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="2faf1-199">Aby je wyświetlić, rozwiń folder kontrolerów w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="2faf1-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="2faf1-200">HomeController to tradycyjne kontrolera ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2faf1-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="2faf1-201">Jest odpowiedzialny za obsługi stron HTML dla witryny, a nie jest bezpośrednio związana z naszego interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="2faf1-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="2faf1-202">ValuesController jest przykład controller WebAPI.</span><span class="sxs-lookup"><span data-stu-id="2faf1-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="2faf1-203">Przejdź dalej i Usuń ValuesController, klikając prawym przyciskiem myszy plik w Eksploratorze rozwiązań i wybierając polecenie **usunąć.**</span><span class="sxs-lookup"><span data-stu-id="2faf1-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="2faf1-204">Teraz Dodaj nowy kontroler, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2faf1-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="2faf1-205">W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy folder kontrolerów (Controllers).</span><span class="sxs-lookup"><span data-stu-id="2faf1-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="2faf1-206">Wybierz pozycję **Dodaj**, a następnie **Kontroler**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="2faf1-207">W **Dodaj kontroler** kreatora, nazwy kontrolera &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="2faf1-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="2faf1-208">W **szablonu** listy rozwijanej wybierz **pusty Kontroler interfejsu API**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="2faf1-209">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="2faf1-210">Nie jest konieczne kontrolerach należy umieścić w folderze o nazwie kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="2faf1-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="2faf1-211">Nazwa folderu nie jest ważna; jest po prostu wygodny sposób organizowania plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="2faf1-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="2faf1-212">**Dodaj kontroler** Kreator utworzy plik o nazwie ProductsController.cs w folderze kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="2faf1-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="2faf1-213">Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie, aby go otworzyć.</span><span class="sxs-lookup"><span data-stu-id="2faf1-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="2faf1-214">Dodaj następujący kod **przy użyciu** instrukcji:</span><span class="sxs-lookup"><span data-stu-id="2faf1-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="2faf1-215">Dodaj pola zawierające **IProductRepository** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="2faf1-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="2faf1-216">Wywoływanie `new ProductRepository()` w kontrolerze nie jest najlepszy projekt, ponieważ wiąże kontroler konkretnej implementacji `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="2faf1-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="2faf1-217">Aby uzyskać lepszym rozwiązaniem, zobacz [za pomocą mechanizmu rozpoznawania zależności dla interfejsu API sieci Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="2faf1-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="2faf1-218">Wprowadzenie do zasobu</span><span class="sxs-lookup"><span data-stu-id="2faf1-218">Getting a Resource</span></span>

<span data-ttu-id="2faf1-219">Interfejs API ProductStore udostępni kilka &quot;odczytu&quot; czynności, jak metod HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="2faf1-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="2faf1-220">Każda akcja odnoszą się do metody w `ProductsController` klasy.</span><span class="sxs-lookup"><span data-stu-id="2faf1-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="2faf1-221">Akcja</span><span class="sxs-lookup"><span data-stu-id="2faf1-221">Action</span></span> | <span data-ttu-id="2faf1-222">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="2faf1-222">HTTP method</span></span> | <span data-ttu-id="2faf1-223">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="2faf1-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2faf1-224">Pobierz listę wszystkich produktów</span><span class="sxs-lookup"><span data-stu-id="2faf1-224">Get a list of all products</span></span> | <span data-ttu-id="2faf1-225">GET</span><span class="sxs-lookup"><span data-stu-id="2faf1-225">GET</span></span> | <span data-ttu-id="2faf1-226">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="2faf1-226">/api/products</span></span> |
| <span data-ttu-id="2faf1-227">Pobieranie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="2faf1-227">Get a product by ID</span></span> | <span data-ttu-id="2faf1-228">GET</span><span class="sxs-lookup"><span data-stu-id="2faf1-228">GET</span></span> | <span data-ttu-id="2faf1-229">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="2faf1-229">/api/products/*id*</span></span> |
| <span data-ttu-id="2faf1-230">Pobierz produkt według kategorii</span><span class="sxs-lookup"><span data-stu-id="2faf1-230">Get a product by category</span></span> | <span data-ttu-id="2faf1-231">GET</span><span class="sxs-lookup"><span data-stu-id="2faf1-231">GET</span></span> | <span data-ttu-id="2faf1-232">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="2faf1-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="2faf1-233">Aby uzyskać listę wszystkich produktów, należy dodać tę metodę w celu `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="2faf1-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="2faf1-234">Nazwa metody rozpoczyna się od &quot;uzyskać&quot;, więc zgodnie z Konwencją, jest on mapowany na żądania GET.</span><span class="sxs-lookup"><span data-stu-id="2faf1-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="2faf1-235">Ponadto, ponieważ metoda nie ma parametrów, jest on mapowany do identyfikatora URI, który nie zawiera *&quot;identyfikator&quot;* segmentów w ścieżce.</span><span class="sxs-lookup"><span data-stu-id="2faf1-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="2faf1-236">Aby uzyskać produkt za pomocą Identyfikatora, należy dodać tę metodę w celu `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="2faf1-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="2faf1-237">Ta nazwa metody również zaczyna się od &quot;uzyskać&quot;, ale metoda ma parametr o nazwie *identyfikator*. Ten parametr jest mapowany na &quot;identyfikator&quot; segmentu ścieżki identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="2faf1-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="2faf1-238">Struktury ASP.NET Web API automatycznie konwertuje identyfikator na prawidłowy typ danych (**int**) dla parametru.</span><span class="sxs-lookup"><span data-stu-id="2faf1-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="2faf1-239">Metoda GetProduct zgłasza wyjątek typu **HttpResponseException** Jeśli *identyfikator* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="2faf1-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="2faf1-240">Wyjątek ten będzie można przetłumaczyć przez platformę błąd 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="2faf1-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="2faf1-241">Na koniec Dodaj metodę, aby znaleźć produkty według kategorii:</span><span class="sxs-lookup"><span data-stu-id="2faf1-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="2faf1-242">Jeśli identyfikator URI żądania zawiera ciąg zapytania, interfejs API sieci Web próbuje dopasować parametry zapytania do parametrów dla metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="2faf1-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="2faf1-243">W związku z tym, identyfikator URI w postaci "interfejsu api/produktów? kategorii =*kategorii*" będzie zmapowana do tej metody.</span><span class="sxs-lookup"><span data-stu-id="2faf1-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="2faf1-244">Tworzenie zasobu</span><span class="sxs-lookup"><span data-stu-id="2faf1-244">Creating a Resource</span></span>

<span data-ttu-id="2faf1-245">Następnie dodamy metodę `ProductsController` klasy w celu utworzenia nowego produktu.</span><span class="sxs-lookup"><span data-stu-id="2faf1-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="2faf1-246">Poniżej przedstawiono prosty implementacji metody:</span><span class="sxs-lookup"><span data-stu-id="2faf1-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="2faf1-247">Należy zwrócić uwagę dwa elementy o tej metodzie:</span><span class="sxs-lookup"><span data-stu-id="2faf1-247">Note two things about this method:</span></span>

- <span data-ttu-id="2faf1-248">Nazwa metody rozpoczyna się od &quot;wpis... &quot;.</span><span class="sxs-lookup"><span data-stu-id="2faf1-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="2faf1-249">Aby utworzyć nowy produkt, klient wysyła żądanie HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="2faf1-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="2faf1-250">Ta metoda przyjmuje parametr typu produktu.</span><span class="sxs-lookup"><span data-stu-id="2faf1-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="2faf1-251">W interfejsie API sieci Web parametrów o złożonych typach są deserializacji z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="2faf1-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="2faf1-252">W związku z tym oczekujemy, że to klientowi wysłanie zserializowana reprezentacja obiektu produktu w formacie XML lub JSON.</span><span class="sxs-lookup"><span data-stu-id="2faf1-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="2faf1-253">Ta implementacja będzie działać, ale nie jest jeszcze ukończone.</span><span class="sxs-lookup"><span data-stu-id="2faf1-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="2faf1-254">W idealnym przypadku prosimy o poświęcenie odpowiedzi HTTP są następujące:</span><span class="sxs-lookup"><span data-stu-id="2faf1-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="2faf1-255">**Kod odpowiedzi:** Domyślnie struktury Web API ustawia kod stanu odpowiedzi 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="2faf1-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="2faf1-256">Jednak zgodnie z protokołem HTTP/1.1, jeśli żądanie POST prowadzi do utworzenia zasobu, serwer powinien odpowiadać ze stanem 201 (utworzono).</span><span class="sxs-lookup"><span data-stu-id="2faf1-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="2faf1-257">**Lokalizacja:** Gdy serwer umożliwia utworzenie zasobu, powinien on zawierać identyfikator URI nowego zasobu w nagłówku Location odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2faf1-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="2faf1-258">ASP.NET Web API można łatwo manipulować komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2faf1-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="2faf1-259">Oto implementacja ulepszone:</span><span class="sxs-lookup"><span data-stu-id="2faf1-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="2faf1-260">Należy zauważyć, że typ zwracany metody jest teraz **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="2faf1-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="2faf1-261">Zwracając **obiektu HttpResponseMessage** zamiast produktu, możemy kontrolować szczegóły komunikatu odpowiedzi HTTP, łącznie z kodem stanu i nagłówek Location.</span><span class="sxs-lookup"><span data-stu-id="2faf1-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="2faf1-262">**CreateResponse** metoda tworzy **obiektu HttpResponseMessage** i automatycznie zapisuje zserializowana reprezentacja obiektu produktu w treści fo komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2faf1-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="2faf1-263">Nie można zweryfikować w tym przykładzie `Product`.</span><span class="sxs-lookup"><span data-stu-id="2faf1-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="2faf1-264">Aby uzyskać informacji o weryfikacji modelu, zobacz [weryfikacji modelu programu ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2faf1-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="2faf1-265">Aktualizowanie zasobu</span><span class="sxs-lookup"><span data-stu-id="2faf1-265">Updating a Resource</span></span>

<span data-ttu-id="2faf1-266">Aktualizowanie produktu za pomocą PUT jest proste:</span><span class="sxs-lookup"><span data-stu-id="2faf1-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="2faf1-267">Nazwa metody rozpoczyna się od &quot;umieścić... &quot;, więc internetowego interfejsu API pasujących do żądania PUT.</span><span class="sxs-lookup"><span data-stu-id="2faf1-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="2faf1-268">Ta metoda przyjmuje dwa parametry: identyfikator produktu i zaktualizowanych produktów.</span><span class="sxs-lookup"><span data-stu-id="2faf1-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="2faf1-269">*Identyfikator* parametru jest pobierana z ścieżka identyfikatora URI i *produktu* parametru jest przeprowadzona z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="2faf1-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="2faf1-270">Domyślnie w ramach interfejsu API sieci Web platformy ASP.NET tworzy typy proste parametrów na podstawie trasy i typów złożonych z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="2faf1-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="2faf1-271">Usuwanie zasobu</span><span class="sxs-lookup"><span data-stu-id="2faf1-271">Deleting a Resource</span></span>

<span data-ttu-id="2faf1-272">Aby usunąć zasób, należy zdefiniować "Usuń..." Metoda.</span><span class="sxs-lookup"><span data-stu-id="2faf1-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="2faf1-273">Jeśli żądanie usunięcia zakończy się powodzeniem, może zwrócić stanu 200 (OK) z treści jednostki opisującą stan; Stan 202 (zaakceptowano), jeśli usunięcie jest nadal oczekujące; Stan lub 204 (Brak zawartości) z bez treści jednostki.</span><span class="sxs-lookup"><span data-stu-id="2faf1-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="2faf1-274">W tym przypadku `DeleteProduct` metoda ma `void` zwracany typ, więc ASP.NET Web API automatycznie przekłada to stan kod 204 (Brak zawartości).</span><span class="sxs-lookup"><span data-stu-id="2faf1-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
