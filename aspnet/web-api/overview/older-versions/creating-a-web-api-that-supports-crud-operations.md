---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Włączanie operacji CRUD w ASP.NET Web API 1-ASP.NET 4. x
author: MikeWasson
description: W samouczku pokazano, jak obsługiwać operacje CRUD w usłudze HTTP przy użyciu internetowego interfejsu API ASP.NET dla ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600334"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="1c65c-103">Włączanie operacji CRUD w interfejsie Web API ASP.NET 1</span><span class="sxs-lookup"><span data-stu-id="1c65c-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="1c65c-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1c65c-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1c65c-105">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="1c65c-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="1c65c-106">W tym samouczku pokazano, jak obsługiwać operacje CRUD w usłudze HTTP przy użyciu internetowego interfejsu API ASP.NET dla ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="1c65c-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1c65c-107">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="1c65c-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1c65c-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1c65c-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="1c65c-109">Interfejs API sieci Web 1 (działa również z interfejsem API sieci Web 2)</span><span class="sxs-lookup"><span data-stu-id="1c65c-109">Web API 1 (also works with Web API 2)</span></span>

<span data-ttu-id="1c65c-110">CRUD &quot;tworzenie, odczytywanie, aktualizowanie i usuwanie&quot; które są cztery podstawowe operacje bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1c65c-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="1c65c-111">Wiele usług HTTP modeluje również operacje CRUD za pośrednictwem interfejsów API REST lub REST.</span><span class="sxs-lookup"><span data-stu-id="1c65c-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="1c65c-112">W tym samouczku utworzysz prosty internetowy interfejs API do zarządzania listą produktów.</span><span class="sxs-lookup"><span data-stu-id="1c65c-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="1c65c-113">Każdy produkt będzie zawierać nazwę, cenę i kategorię (na przykład &quot;zabawki&quot; lub &quot;&quot;sprzętu) oraz identyfikator produktu.</span><span class="sxs-lookup"><span data-stu-id="1c65c-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="1c65c-114">Interfejs API produktów będzie uwidaczniał następujące metody.</span><span class="sxs-lookup"><span data-stu-id="1c65c-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="1c65c-115">Akcja</span><span class="sxs-lookup"><span data-stu-id="1c65c-115">Action</span></span> | <span data-ttu-id="1c65c-116">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="1c65c-116">HTTP method</span></span> | <span data-ttu-id="1c65c-117">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="1c65c-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1c65c-118">Pobierz listę wszystkich produktów</span><span class="sxs-lookup"><span data-stu-id="1c65c-118">Get a list of all products</span></span> | <span data-ttu-id="1c65c-119">Pobierz</span><span class="sxs-lookup"><span data-stu-id="1c65c-119">GET</span></span> | <span data-ttu-id="1c65c-120">/api/products</span><span class="sxs-lookup"><span data-stu-id="1c65c-120">/api/products</span></span> |
| <span data-ttu-id="1c65c-121">Pobierz produkt według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="1c65c-121">Get a product by ID</span></span> | <span data-ttu-id="1c65c-122">Pobierz</span><span class="sxs-lookup"><span data-stu-id="1c65c-122">GET</span></span> | <span data-ttu-id="1c65c-123">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="1c65c-123">/api/products/*id*</span></span> |
| <span data-ttu-id="1c65c-124">Pobierz produkt według kategorii</span><span class="sxs-lookup"><span data-stu-id="1c65c-124">Get a product by category</span></span> | <span data-ttu-id="1c65c-125">Pobierz</span><span class="sxs-lookup"><span data-stu-id="1c65c-125">GET</span></span> | <span data-ttu-id="1c65c-126">/API/Products? Category =*Kategoria*</span><span class="sxs-lookup"><span data-stu-id="1c65c-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="1c65c-127">Utwórz nowy produkt</span><span class="sxs-lookup"><span data-stu-id="1c65c-127">Create a new product</span></span> | <span data-ttu-id="1c65c-128">POUBOJOWEGO</span><span class="sxs-lookup"><span data-stu-id="1c65c-128">POST</span></span> | <span data-ttu-id="1c65c-129">/api/products</span><span class="sxs-lookup"><span data-stu-id="1c65c-129">/api/products</span></span> |
| <span data-ttu-id="1c65c-130">Aktualizowanie produktu</span><span class="sxs-lookup"><span data-stu-id="1c65c-130">Update a product</span></span> | <span data-ttu-id="1c65c-131">Ubrani</span><span class="sxs-lookup"><span data-stu-id="1c65c-131">PUT</span></span> | <span data-ttu-id="1c65c-132">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="1c65c-132">/api/products/*id*</span></span> |
| <span data-ttu-id="1c65c-133">Usuwanie produktu</span><span class="sxs-lookup"><span data-stu-id="1c65c-133">Delete a product</span></span> | <span data-ttu-id="1c65c-134">DELETE</span><span class="sxs-lookup"><span data-stu-id="1c65c-134">DELETE</span></span> | <span data-ttu-id="1c65c-135">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="1c65c-135">/api/products/*id*</span></span> |

<span data-ttu-id="1c65c-136">Zwróć uwagę, że niektóre identyfikatory URI zawierają identyfikator produktu w ścieżce.</span><span class="sxs-lookup"><span data-stu-id="1c65c-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="1c65c-137">Na przykład aby uzyskać produkt o IDENTYFIKATORze 28, klient wysyła żądanie GET dla `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="1c65c-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="1c65c-138">Resources</span><span class="sxs-lookup"><span data-stu-id="1c65c-138">Resources</span></span>

<span data-ttu-id="1c65c-139">Interfejs API produktów definiuje identyfikatory URI dla dwóch typów zasobów:</span><span class="sxs-lookup"><span data-stu-id="1c65c-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="1c65c-140">Zasób</span><span class="sxs-lookup"><span data-stu-id="1c65c-140">Resource</span></span> | <span data-ttu-id="1c65c-141">{1&gt;URI&lt;1}</span><span class="sxs-lookup"><span data-stu-id="1c65c-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="1c65c-142">Lista wszystkich produktów.</span><span class="sxs-lookup"><span data-stu-id="1c65c-142">The list of all the products.</span></span> | <span data-ttu-id="1c65c-143">/api/products</span><span class="sxs-lookup"><span data-stu-id="1c65c-143">/api/products</span></span> |
| <span data-ttu-id="1c65c-144">Pojedynczy produkt.</span><span class="sxs-lookup"><span data-stu-id="1c65c-144">An individual product.</span></span> | <span data-ttu-id="1c65c-145">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="1c65c-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="1c65c-146">Metody</span><span class="sxs-lookup"><span data-stu-id="1c65c-146">Methods</span></span>

<span data-ttu-id="1c65c-147">Cztery główne metody HTTP (GET, PUT, POST i DELETE) można mapować na operacje CRUD w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1c65c-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="1c65c-148">Pobiera reprezentację zasobu pod określonym identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="1c65c-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="1c65c-149">POBIERANIE nie powinno mieć żadnych efektów ubocznych na serwerze.</span><span class="sxs-lookup"><span data-stu-id="1c65c-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="1c65c-150">UMIESZCZENIE aktualizacji zasobu o określonym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="1c65c-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="1c65c-151">Opcję PUT można także użyć do utworzenia nowego zasobu o określonym identyfikatorze URI, jeśli serwer zezwala klientom na określanie nowych identyfikatorów URI.</span><span class="sxs-lookup"><span data-stu-id="1c65c-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="1c65c-152">W tym samouczku interfejs API nie obsługuje tworzenia przez funkcję PUT.</span><span class="sxs-lookup"><span data-stu-id="1c65c-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="1c65c-153">Po utworzeniu nowego zasobu.</span><span class="sxs-lookup"><span data-stu-id="1c65c-153">POST creates a new resource.</span></span> <span data-ttu-id="1c65c-154">Serwer przypisuje identyfikator URI nowego obiektu i zwraca ten identyfikator URI jako część komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1c65c-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="1c65c-155">DELETE Usuwa zasób o określonym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="1c65c-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="1c65c-156">Uwaga: Metoda PUT zastępuje całą jednostkę produktu.</span><span class="sxs-lookup"><span data-stu-id="1c65c-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="1c65c-157">Oznacza to, że klient powinien wysłać kompletną reprezentację zaktualizowanego produktu.</span><span class="sxs-lookup"><span data-stu-id="1c65c-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="1c65c-158">Jeśli chcesz obsługiwać aktualizacje częściowe, preferowana jest metoda PATCH.</span><span class="sxs-lookup"><span data-stu-id="1c65c-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="1c65c-159">Ten samouczek nie implementuje poprawki.</span><span class="sxs-lookup"><span data-stu-id="1c65c-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="1c65c-160">Utwórz nowy projekt internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="1c65c-160">Create a New Web API Project</span></span>

<span data-ttu-id="1c65c-161">Zacznij od uruchomienia programu Visual Studio i wybierz pozycję **Nowy projekt** na stronie **startowej** .</span><span class="sxs-lookup"><span data-stu-id="1c65c-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="1c65c-162">Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="1c65c-163">W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  .</span><span class="sxs-lookup"><span data-stu-id="1c65c-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="1c65c-164">W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="1c65c-165">Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="1c65c-166">Nazwij projekt &quot;ProductStore&quot; a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="1c65c-167">W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **Web API** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="1c65c-168">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="1c65c-168">Adding a Model</span></span>

<span data-ttu-id="1c65c-169">*Model* to obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c65c-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="1c65c-170">W interfejsie API sieci Web ASP.NET można używać silnie określonych obiektów CLR jako modeli i automatycznie serializować je do XML lub JSON dla klienta.</span><span class="sxs-lookup"><span data-stu-id="1c65c-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="1c65c-171">W przypadku interfejsu API ProductStore nasze dane składają się z produktów, więc utworzymy nową klasę o nazwie `Product`.</span><span class="sxs-lookup"><span data-stu-id="1c65c-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="1c65c-172">Jeśli Eksplorator rozwiązań nie jest jeszcze widoczna, kliknij menu **Widok** i wybierz pozycję **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="1c65c-173">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder **modele** .</span><span class="sxs-lookup"><span data-stu-id="1c65c-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="1c65c-174">Z menu kontekstowego wybierz pozycję **Dodaj**, a następnie wybierz pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="1c65c-175">Nazwij klasę &quot;&quot;produktu.</span><span class="sxs-lookup"><span data-stu-id="1c65c-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="1c65c-176">Dodaj następujące właściwości do klasy `Product`.</span><span class="sxs-lookup"><span data-stu-id="1c65c-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="1c65c-177">Dodawanie repozytorium</span><span class="sxs-lookup"><span data-stu-id="1c65c-177">Adding a Repository</span></span>

<span data-ttu-id="1c65c-178">Musimy przechowywać kolekcję produktów.</span><span class="sxs-lookup"><span data-stu-id="1c65c-178">We need to store a collection of products.</span></span> <span data-ttu-id="1c65c-179">Dobrym pomysłem jest oddzielenie kolekcji od naszej implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="1c65c-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="1c65c-180">Dzięki temu można zmienić magazyn zapasowy bez ponownego zapisywania klasy usługi.</span><span class="sxs-lookup"><span data-stu-id="1c65c-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="1c65c-181">Ten typ projektu nazywa się wzorcem *repozytorium* .</span><span class="sxs-lookup"><span data-stu-id="1c65c-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="1c65c-182">Zacznij od zdefiniowania ogólnego interfejsu dla repozytorium.</span><span class="sxs-lookup"><span data-stu-id="1c65c-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="1c65c-183">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder **modele** .</span><span class="sxs-lookup"><span data-stu-id="1c65c-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="1c65c-184">Wybierz pozycję **Dodaj**, a następnie wybierz pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="1c65c-185">W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń C# węzeł.</span><span class="sxs-lookup"><span data-stu-id="1c65c-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="1c65c-186">W C#obszarze Wybierz pozycję **kod**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-186">Under C#, select **Code**.</span></span> <span data-ttu-id="1c65c-187">Na liście szablonów kodu wybierz pozycję **interfejs**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="1c65c-188">Nazwij interfejs &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="1c65c-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="1c65c-189">Dodaj następującą implementację:</span><span class="sxs-lookup"><span data-stu-id="1c65c-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="1c65c-190">Teraz Dodaj kolejną klasę do folderu models o nazwie &quot;ProductRepository.&quot; Ta klasa zaimplementuje interfejs `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="1c65c-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="1c65c-191">Dodaj następującą implementację:</span><span class="sxs-lookup"><span data-stu-id="1c65c-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="1c65c-192">Repozytorium zachowuje listę w pamięci lokalnej.</span><span class="sxs-lookup"><span data-stu-id="1c65c-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="1c65c-193">Jest to prawidłowe dla samouczka, ale w rzeczywistej aplikacji dane są przechowywane zewnętrznie, w bazie danych lub w magazynie w chmurze.</span><span class="sxs-lookup"><span data-stu-id="1c65c-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="1c65c-194">Wzorzec repozytorium będzie łatwiej zmienić implementację później.</span><span class="sxs-lookup"><span data-stu-id="1c65c-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="1c65c-195">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="1c65c-195">Adding a Web API Controller</span></span>

<span data-ttu-id="1c65c-196">Jeśli pracujesz z ASP.NET MVC, masz już znane kontrolery.</span><span class="sxs-lookup"><span data-stu-id="1c65c-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="1c65c-197">W interfejsie API sieci Web ASP.NET *kontroler* jest klasą, która obsługuje żądania HTTP od klienta.</span><span class="sxs-lookup"><span data-stu-id="1c65c-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="1c65c-198">Kreator nowego projektu utworzył dwa kontrolery dla Ciebie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="1c65c-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="1c65c-199">Aby je wyświetlić, rozwiń folder controllers w Eksplorator rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="1c65c-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="1c65c-200">HomeController jest tradycyjnym kontrolerem ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1c65c-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="1c65c-201">Jest on odpowiedzialny za obsługę stron HTML dla witryny i nie jest bezpośrednio związany z naszym interfejsem API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1c65c-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="1c65c-202">ValuesController jest przykładowym kontrolerem WebAPI.</span><span class="sxs-lookup"><span data-stu-id="1c65c-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="1c65c-203">Przejdź dalej i Usuń ValuesController, klikając prawym przyciskiem myszy plik w Eksplorator rozwiązań i wybierając pozycję **Usuń.**</span><span class="sxs-lookup"><span data-stu-id="1c65c-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="1c65c-204">Teraz Dodaj nowy kontroler w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1c65c-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="1c65c-205">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder controllers.</span><span class="sxs-lookup"><span data-stu-id="1c65c-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="1c65c-206">Wybierz pozycję **Dodaj** , a następnie wybierz pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="1c65c-207">W kreatorze **dodawania kontrolera** nazwij kontroler &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="1c65c-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="1c65c-208">Z listy rozwijanej **szablon** wybierz pozycję **pusty kontroler interfejsu API**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="1c65c-209">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="1c65c-210">Nie jest konieczne umieszczenie kontrolerów w folderze o nazwie controllers.</span><span class="sxs-lookup"><span data-stu-id="1c65c-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="1c65c-211">Nazwa folderu nie jest ważna. jest to po prostu wygodny sposób organizowania plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="1c65c-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>

<span data-ttu-id="1c65c-212">Kreator **dodawania kontrolera** utworzy plik o nazwie ProductsController.cs w folderze controllers.</span><span class="sxs-lookup"><span data-stu-id="1c65c-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="1c65c-213">Jeśli ten plik nie jest jeszcze otwarty, kliknij go dwukrotnie, aby go otworzyć.</span><span class="sxs-lookup"><span data-stu-id="1c65c-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="1c65c-214">Dodaj następującą instrukcję **using** :</span><span class="sxs-lookup"><span data-stu-id="1c65c-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="1c65c-215">Dodaj pole, które zawiera wystąpienie **IProductRepository** .</span><span class="sxs-lookup"><span data-stu-id="1c65c-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="1c65c-216">Wywoływanie `new ProductRepository()` w kontrolerze nie jest najlepszym projektem, ponieważ łączy kontroler z określoną implementacją `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="1c65c-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="1c65c-217">Aby uzyskać lepsze podejście, zobacz [Korzystanie z programu rozpoznawania zależności interfejsu API sieci Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="1c65c-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>

## <a name="getting-a-resource"></a><span data-ttu-id="1c65c-218">Pobieranie zasobu</span><span class="sxs-lookup"><span data-stu-id="1c65c-218">Getting a Resource</span></span>

<span data-ttu-id="1c65c-219">Interfejs API ProductStore będzie uwidaczniał kilka &quot;operacji odczytu&quot; jako metody HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="1c65c-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="1c65c-220">Każda akcja będzie odpowiadać metodzie w klasie `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="1c65c-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="1c65c-221">Akcja</span><span class="sxs-lookup"><span data-stu-id="1c65c-221">Action</span></span> | <span data-ttu-id="1c65c-222">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="1c65c-222">HTTP method</span></span> | <span data-ttu-id="1c65c-223">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="1c65c-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1c65c-224">Pobierz listę wszystkich produktów</span><span class="sxs-lookup"><span data-stu-id="1c65c-224">Get a list of all products</span></span> | <span data-ttu-id="1c65c-225">Pobierz</span><span class="sxs-lookup"><span data-stu-id="1c65c-225">GET</span></span> | <span data-ttu-id="1c65c-226">/api/products</span><span class="sxs-lookup"><span data-stu-id="1c65c-226">/api/products</span></span> |
| <span data-ttu-id="1c65c-227">Pobierz produkt według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="1c65c-227">Get a product by ID</span></span> | <span data-ttu-id="1c65c-228">Pobierz</span><span class="sxs-lookup"><span data-stu-id="1c65c-228">GET</span></span> | <span data-ttu-id="1c65c-229">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="1c65c-229">/api/products/*id*</span></span> |
| <span data-ttu-id="1c65c-230">Pobierz produkt według kategorii</span><span class="sxs-lookup"><span data-stu-id="1c65c-230">Get a product by category</span></span> | <span data-ttu-id="1c65c-231">Pobierz</span><span class="sxs-lookup"><span data-stu-id="1c65c-231">GET</span></span> | <span data-ttu-id="1c65c-232">/API/Products? Category =*Kategoria*</span><span class="sxs-lookup"><span data-stu-id="1c65c-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="1c65c-233">Aby uzyskać listę wszystkich produktów, należy dodać tę metodę do klasy `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="1c65c-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="1c65c-234">Nazwa metody rozpoczyna się od &quot;Get&quot;, tak więc według Konwencji mapuje się na żądania GET.</span><span class="sxs-lookup"><span data-stu-id="1c65c-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="1c65c-235">Ponadto, ponieważ metoda nie ma parametrów, mapuje na identyfikator URI, który nie zawiera *&quot;identyfikatora&quot;* segmentu w ścieżce.</span><span class="sxs-lookup"><span data-stu-id="1c65c-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="1c65c-236">Aby uzyskać produkt według identyfikatora, należy dodać tę metodę do klasy `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="1c65c-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="1c65c-237">Ta nazwa metody rozpoczyna się również od &quot;Get&quot;, ale metoda ma parametr o nazwie *ID*. Ten parametr jest mapowany do &quot;identyfikator&quot; segmentu ścieżki URI.</span><span class="sxs-lookup"><span data-stu-id="1c65c-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="1c65c-238">Struktura internetowego interfejsu API ASP.NET automatycznie konwertuje identyfikator do poprawnego typu danych (**int**) dla parametru.</span><span class="sxs-lookup"><span data-stu-id="1c65c-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="1c65c-239">Metoda getproduct zwraca wyjątek typu **HttpResponseException** , jeśli *Identyfikator* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="1c65c-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="1c65c-240">Ten wyjątek zostanie przetłumaczony przez platformę na błąd 404 (nie można odnaleźć).</span><span class="sxs-lookup"><span data-stu-id="1c65c-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="1c65c-241">Na koniec Dodaj metodę, aby znaleźć produkty według kategorii:</span><span class="sxs-lookup"><span data-stu-id="1c65c-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="1c65c-242">Jeśli identyfikator URI żądania zawiera ciąg zapytania, interfejs API sieci Web próbuje dopasować parametry zapytania do parametrów w metodzie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1c65c-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="1c65c-243">W związku z tym identyfikator URI formularza "API/productss? Category =*Category*" zostanie zmapowany na tę metodę.</span><span class="sxs-lookup"><span data-stu-id="1c65c-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="1c65c-244">Tworzenie zasobu</span><span class="sxs-lookup"><span data-stu-id="1c65c-244">Creating a Resource</span></span>

<span data-ttu-id="1c65c-245">Następnie dodamy metodę do klasy `ProductsController`, aby utworzyć nowy produkt.</span><span class="sxs-lookup"><span data-stu-id="1c65c-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="1c65c-246">Oto prosta implementacja metody:</span><span class="sxs-lookup"><span data-stu-id="1c65c-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="1c65c-247">Zwróć uwagę na dwie rzeczy dotyczące tej metody:</span><span class="sxs-lookup"><span data-stu-id="1c65c-247">Note two things about this method:</span></span>

- <span data-ttu-id="1c65c-248">Nazwa metody rozpoczyna się od &quot;post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="1c65c-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="1c65c-249">Aby utworzyć nowy produkt, klient wysyła żądanie HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="1c65c-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="1c65c-250">Metoda przyjmuje parametr typu produkt.</span><span class="sxs-lookup"><span data-stu-id="1c65c-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="1c65c-251">W interfejsie API sieci Web parametry o typach złożonych są deserializowane od treści żądania.</span><span class="sxs-lookup"><span data-stu-id="1c65c-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="1c65c-252">W związku z tym oczekujemy, że klient wyśle serializowaną reprezentację obiektu produktu w formacie XML lub JSON.</span><span class="sxs-lookup"><span data-stu-id="1c65c-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="1c65c-253">Ta implementacja będzie działała, ale nie została ukończona.</span><span class="sxs-lookup"><span data-stu-id="1c65c-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="1c65c-254">Najlepiej, aby odpowiedź HTTP obejmowała następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="1c65c-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="1c65c-255">**Kod odpowiedzi:** Domyślnie Struktura interfejsu API sieci Web ustawia kod stanu odpowiedzi na 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="1c65c-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="1c65c-256">Jednak zgodnie z protokołem HTTP/1.1, gdy żądanie POST powoduje utworzenie zasobu, serwer powinien odpowiedzieć ze stanem 201 (utworzony).</span><span class="sxs-lookup"><span data-stu-id="1c65c-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="1c65c-257">**Lokalizacja:** Gdy serwer tworzy zasób, powinien zawierać identyfikator URI nowego zasobu w nagłówku lokalizacji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1c65c-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="1c65c-258">Interfejs API sieci Web ASP.NET ułatwia manipulowanie komunikatem odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="1c65c-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="1c65c-259">Ulepszona implementacja:</span><span class="sxs-lookup"><span data-stu-id="1c65c-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="1c65c-260">Zwróć uwagę, że zwracany typ metody to teraz **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="1c65c-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="1c65c-261">Zwracając **HttpResponseMessage** zamiast produktu, możemy kontrolować szczegóły komunikatu odpowiedzi HTTP, w tym kod stanu i nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="1c65c-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="1c65c-262">Metoda **onresponse** tworzy **HttpResponseMessage** i automatycznie zapisuje serializowaną reprezentację obiektu produktu w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1c65c-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="1c65c-263">Ten przykład nie sprawdza poprawności `Product`.</span><span class="sxs-lookup"><span data-stu-id="1c65c-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="1c65c-264">Aby uzyskać informacje na temat weryfikacji modelu, zobacz [Walidacja modelu w ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1c65c-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

## <a name="updating-a-resource"></a><span data-ttu-id="1c65c-265">Aktualizowanie zasobu</span><span class="sxs-lookup"><span data-stu-id="1c65c-265">Updating a Resource</span></span>

<span data-ttu-id="1c65c-266">Aktualizacja produktu z funkcją PUT jest prosta:</span><span class="sxs-lookup"><span data-stu-id="1c65c-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="1c65c-267">Nazwa metody rozpoczyna się od &quot;Put...&quot;, więc interfejs API sieci Web dopasowuje go do UMIESZCZAnia żądań.</span><span class="sxs-lookup"><span data-stu-id="1c65c-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="1c65c-268">Metoda przyjmuje dwa parametry, identyfikator produktu i zaktualizowany produkt.</span><span class="sxs-lookup"><span data-stu-id="1c65c-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="1c65c-269">Parametr *ID* jest pobierany ze ścieżki identyfikatora URI, a parametr *produktu* jest deserializowany od treści żądania.</span><span class="sxs-lookup"><span data-stu-id="1c65c-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="1c65c-270">Domyślnie Struktura interfejsu API sieci Web ASP.NET przyjmuje proste typy parametrów z tras i typów złożonych z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="1c65c-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="1c65c-271">Usuwanie zasobu</span><span class="sxs-lookup"><span data-stu-id="1c65c-271">Deleting a Resource</span></span>

<span data-ttu-id="1c65c-272">Aby usunąć zasób, zdefiniuj "Usuwanie..." Method.</span><span class="sxs-lookup"><span data-stu-id="1c65c-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="1c65c-273">Jeśli żądanie usunięcia powiedzie się, może zwrócić status 200 (OK) o treści jednostki opisującej stan; stan 202 (zaakceptowany), Jeśli usunięcie nadal oczekuje; lub status 204 (brak zawartości) bez treści jednostki.</span><span class="sxs-lookup"><span data-stu-id="1c65c-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="1c65c-274">W tym przypadku Metoda `DeleteProduct` ma `void` typ zwracany, więc interfejs API sieci Web ASP.NET automatycznie przetłumaczy ten element na kod stanu 204 (brak zawartości).</span><span class="sxs-lookup"><span data-stu-id="1c65c-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
