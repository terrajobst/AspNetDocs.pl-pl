---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Za pomocą interfejsu API sieci Web przy użyciu wzorca ASP.NET Web Forms — ASP.NET 4.x
author: MikeWasson
description: Samouczek z kodu krok po kroku, aby dodać interfejs API sieci Web do aplikacji formularzy programu ASP.NET dla aplikacji ASP.NET 4.x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59422580"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="6f5a1-103">Korzystanie z wzorca Web API we wzorcu ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="6f5a1-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="6f5a1-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6f5a1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6f5a1-105">Ten samouczek przeprowadzi Cię przez kroki Aby dodać interfejs API sieci Web do tradycyjnych aplikacji formularzy sieci Web platformy ASP.NET na platformie ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="6f5a1-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6f5a1-106">Overview</span></span>

<span data-ttu-id="6f5a1-107">Chociaż interfejs API sieci Web platformy ASP.NET jest dostarczana z platformą ASP.NET MVC, jest łatwe do dodania interfejsu API sieci Web do tradycyjnych aplikacji formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="6f5a1-108">Aby użyć interfejsu API sieci Web w aplikacji formularzy sieci Web, istnieją dwa podstawowe kroki:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="6f5a1-109">Dodawanie kontrolera interfejsu API sieci Web, która pochodzi od klasy **klasy ApiController** klasy.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="6f5a1-110">Dodaj tabelę tras w celu **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="6f5a1-111">Utwórz projekt formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="6f5a1-111">Create a Web Forms Project</span></span>

<span data-ttu-id="6f5a1-112">Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **Start**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="6f5a1-113">Możesz również z menu **Plik** wybrać pozycję **Nowy**, a następnie **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="6f5a1-114">W okienku **Szablony** wybierz pozycję **Zainstalowane szablony** i rozwiń węzeł **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="6f5a1-115">W obszarze **Visual C#**, wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="6f5a1-116">Na liście szablonów projektu wybierz **aplikacji formularzy sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="6f5a1-117">Wprowadź nazwę dla projektu, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="6f5a1-118">Tworzenie modelu i kontrolera</span><span class="sxs-lookup"><span data-stu-id="6f5a1-118">Create the Model and Controller</span></span>

<span data-ttu-id="6f5a1-119">W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="6f5a1-120">Najpierw Dodaj klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-120">First, add a model class.</span></span> <span data-ttu-id="6f5a1-121">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="6f5a1-122">Nazwa klasy produktu i dodaj następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="6f5a1-123">Następnie dodaj Kontroler interfejsu API sieci Web do projektu., A *kontrolera* jest obiektem, który obsługuje żądania HTTP dla interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="6f5a1-124">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="6f5a1-125">Wybierz **Dodaj nowy element**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="6f5a1-126">W obszarze **zainstalowane szablony**, rozwiń węzeł **Visual C#** i wybierz **Web**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="6f5a1-127">Z listy szablonów wybierz **klasa formantu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="6f5a1-128">Nazwa kontrolera "ProductsController", a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="6f5a1-129">**Dodaj nowy element** Kreator utworzy plik o nazwie ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="6f5a1-130">Usunięcie metod, które uwzględnione kreatora i dodaj następujące metody:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="6f5a1-131">Aby uzyskać więcej informacji o kodzie, w tym kontrolerze, zobacz [wprowadzenie](tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="6f5a1-132">Dodawanie informacji o routingu</span><span class="sxs-lookup"><span data-stu-id="6f5a1-132">Add Routing Information</span></span>

<span data-ttu-id="6f5a1-133">Teraz, dlatego dodamy trasy identyfikator URI tego identyfikatory URI w postaci &quot;/interfejsAPI/produkty/&quot; są kierowane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="6f5a1-134">W **Eksploratora rozwiązań**, kliknij dwukrotnie plik Global.asax można otworzyć pliku związanego z kodem Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="6f5a1-135">Dodaj następujący kod **przy użyciu** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="6f5a1-136">Następnie dodaj następujący kod do **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="6f5a1-137">Aby uzyskać więcej informacji o tabelach routingu, zobacz [routingu ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6f5a1-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="6f5a1-138">Dodaj AJAX po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="6f5a1-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="6f5a1-139">To wszystko, czego potrzebujesz do tworzenia internetowego interfejsu API, które klienci mogą uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="6f5a1-140">Teraz możemy dodać stronę HTML, która używa technologii jQuery do wywołania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="6f5a1-141">Upewnić się, że strona wzorcowa (na przykład *Site.Master*) obejmuje `ContentPlaceHolder` z `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="6f5a1-142">Otwórz plik Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-142">Open the file Default.aspx.</span></span> <span data-ttu-id="6f5a1-143">Zastąp standardowy tekst, który znajduje się w sekcji głównej zawartości, jak pokazano:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="6f5a1-144">Następnie dodaj odwołanie do pliku źródłowego jQuery w `HeaderContent` sekcji:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="6f5a1-145">Uwaga: Możesz łatwo dodać odwołanie do skryptu, przeciągając i upuszczając go z **Eksploratora rozwiązań** do okna edytora kodu.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="6f5a1-146">Poniżej jQuery tag skryptu Dodaj następujący blok skryptu:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="6f5a1-147">Podczas ładowania dokumentu, ten skrypt tworzy żądanie AJAX &quot;interfejsu api/produktów&quot;.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="6f5a1-148">Żądanie zwraca listę produktów, w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="6f5a1-149">Skrypt ten dodaje informacje o produkcie do tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="6f5a1-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="6f5a1-150">Po uruchomieniu aplikacji jej powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="6f5a1-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
