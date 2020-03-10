---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Korzystanie z interfejsu API sieci Web z ASP.NET Web Forms — ASP.NET 4. x
author: MikeWasson
description: Samouczek z kodem krok po kroku, aby dodać internetowy interfejs API do aplikacji ASP.NET Forms dla ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556778"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="e6a9f-103">Używanie składnika Web API z formularzami sieci Web platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e6a9f-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="e6a9f-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e6a9f-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e6a9f-105">Ten samouczek przeprowadzi Cię przez procedurę dodawania internetowego interfejsu API do tradycyjnej aplikacji ASP.NET Web Forms w ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="e6a9f-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="e6a9f-106">Overview</span></span>

<span data-ttu-id="e6a9f-107">Mimo że interfejs API sieci Web ASP.NET jest spakowany przy użyciu ASP.NET MVC, można łatwo dodać internetowy interfejs API do tradycyjnej aplikacji formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="e6a9f-108">Aby korzystać z interfejsu API sieci Web w aplikacji formularzy sieci Web, należy wykonać dwa główne kroki:</span><span class="sxs-lookup"><span data-stu-id="e6a9f-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="e6a9f-109">Dodaj kontroler interfejsu API sieci Web pochodzący z klasy **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="e6a9f-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="e6a9f-110">Dodaj tabelę tras do metody **Start\_aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="e6a9f-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="e6a9f-111">Tworzenie projektu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="e6a9f-111">Create a Web Forms Project</span></span>

<span data-ttu-id="e6a9f-112">Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **startowej** .</span><span class="sxs-lookup"><span data-stu-id="e6a9f-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="e6a9f-113">Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="e6a9f-114">W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  .</span><span class="sxs-lookup"><span data-stu-id="e6a9f-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e6a9f-115">W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e6a9f-116">Na liście szablonów projektu wybierz pozycję **aplikacja formularzy sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="e6a9f-117">Wprowadź nazwę projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="e6a9f-118">Tworzenie modelu i kontrolera</span><span class="sxs-lookup"><span data-stu-id="e6a9f-118">Create the Model and Controller</span></span>

<span data-ttu-id="e6a9f-119">Ten samouczek używa tych samych klas modelu i kontrolera co samouczek [wprowadzenie](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="e6a9f-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="e6a9f-120">Najpierw Dodaj klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-120">First, add a model class.</span></span> <span data-ttu-id="e6a9f-121">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj klasę**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="e6a9f-122">Nadaj klasie produktowi i Dodaj następującą implementację:</span><span class="sxs-lookup"><span data-stu-id="e6a9f-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="e6a9f-123">Następnie Dodaj kontroler interfejsu API sieci Web do projektu. *kontroler* jest obiektem, który obsługuje żądania HTTP dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="e6a9f-124">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e6a9f-125">Wybierz pozycję **Dodaj nowy element**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="e6a9f-126">W obszarze **zainstalowane szablony**rozwiń **pozycję C# Wizualizacja** i wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="e6a9f-127">Następnie z listy szablonów wybierz pozycję **Klasa kontrolera interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="e6a9f-128">Nazwij kontroler "ProductsController" i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="e6a9f-129">Kreator **dodawania nowego elementu** utworzy plik o nazwie ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="e6a9f-130">Usuń metody dołączone do kreatora i Dodaj następujące metody:</span><span class="sxs-lookup"><span data-stu-id="e6a9f-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="e6a9f-131">Aby uzyskać więcej informacji na temat kodu w tym kontrolerze, zobacz samouczek [wprowadzenie](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="e6a9f-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="e6a9f-132">Dodawanie informacji o routingu</span><span class="sxs-lookup"><span data-stu-id="e6a9f-132">Add Routing Information</span></span>

<span data-ttu-id="e6a9f-133">Następnie dodamy trasę identyfikatora URI, aby identyfikatory URI formularza &quot;/API/Products/&quot; są kierowane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="e6a9f-134">W **Eksplorator rozwiązań**kliknij dwukrotnie pozycję Global. asax, aby otworzyć plik związany z kodem Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="e6a9f-135">Dodaj następującą instrukcję **using** .</span><span class="sxs-lookup"><span data-stu-id="e6a9f-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="e6a9f-136">Następnie Dodaj następujący kod do **aplikacji\_Start** :</span><span class="sxs-lookup"><span data-stu-id="e6a9f-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="e6a9f-137">Aby uzyskać więcej informacji na temat tabel routingu, zobacz [Routing in Web API ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e6a9f-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="e6a9f-138">Dodawanie technologii AJAX po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="e6a9f-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="e6a9f-139">To wszystko, co jest potrzebne do utworzenia internetowego interfejsu API, do którego klienci mogą uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="e6a9f-140">Teraz dodamy do wywołania interfejsu API stronę HTML, która używa technologii jQuery.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="e6a9f-141">Upewnij się, że strona wzorcowa (na przykład *site. Master*) zawiera `ContentPlaceHolder` z `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="e6a9f-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="e6a9f-142">Otwórz plik default. aspx.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-142">Open the file Default.aspx.</span></span> <span data-ttu-id="e6a9f-143">Zastąp tekst standardowy, który znajduje się w sekcji głównej zawartości, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="e6a9f-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="e6a9f-144">Następnie Dodaj odwołanie do pliku źródłowego jQuery w sekcji `HeaderContent`:</span><span class="sxs-lookup"><span data-stu-id="e6a9f-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="e6a9f-145">Uwaga: możesz łatwo dodać odwołanie do skryptu, przeciągając i upuszczając plik z **Eksplorator rozwiązań** do okna edytora kodu.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="e6a9f-146">Poniżej tagu skryptu jQuery Dodaj następujący blok skryptu:</span><span class="sxs-lookup"><span data-stu-id="e6a9f-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="e6a9f-147">Po załadowaniu dokumentu ten skrypt wykonuje żądanie AJAX, aby &quot;&quot;API/produkty.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="e6a9f-148">Żądanie zwraca listę produktów w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="e6a9f-149">Skrypt dodaje informacje o produkcie do tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="e6a9f-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="e6a9f-150">Po uruchomieniu aplikacji powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="e6a9f-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
