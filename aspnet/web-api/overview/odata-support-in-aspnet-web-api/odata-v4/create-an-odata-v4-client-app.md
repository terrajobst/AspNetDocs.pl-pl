---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Tworzenie aplikacji klienckiej OData v4 (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556295"
---
# <a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="e7b88-102">Tworzenie aplikacji klienckiej OData 4 (C#)</span><span class="sxs-lookup"><span data-stu-id="e7b88-102">Create an OData v4 Client App (C#)</span></span>

<span data-ttu-id="e7b88-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e7b88-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e7b88-104">W poprzednim samouczku utworzono podstawową usługę OData, która obsługuje operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="e7b88-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="e7b88-105">Teraz Utwórzmy klienta usługi.</span><span class="sxs-lookup"><span data-stu-id="e7b88-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="e7b88-106">Uruchom nowe wystąpienie programu Visual Studio i Utwórz nowy projekt aplikacji konsolowej.</span><span class="sxs-lookup"><span data-stu-id="e7b88-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="e7b88-107">W oknie dialogowym **Nowy projekt** wybierz pozycję **zainstalowane** &gt; **Szablony** &gt; **Visual C#**  &gt; **Windows Desktop**, a następnie wybierz szablon **aplikacja konsoli** .</span><span class="sxs-lookup"><span data-stu-id="e7b88-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="e7b88-108">Nazwij projekt &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="e7b88-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="e7b88-109">Możesz również dodać aplikację konsolową do tego samego rozwiązania programu Visual Studio, które zawiera usługę OData.</span><span class="sxs-lookup"><span data-stu-id="e7b88-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>

## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="e7b88-110">Instalowanie generatora kodów klienta OData</span><span class="sxs-lookup"><span data-stu-id="e7b88-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="e7b88-111">W menu **Narzędzia** wybierz pozycję **rozszerzenia i aktualizacje**.</span><span class="sxs-lookup"><span data-stu-id="e7b88-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="e7b88-112">Wybierz pozycję **Online** &gt; **Visual Studio Gallery**.</span><span class="sxs-lookup"><span data-stu-id="e7b88-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="e7b88-113">W polu wyszukiwania Wyszukaj &quot;&quot;generatora kodu klienta OData.</span><span class="sxs-lookup"><span data-stu-id="e7b88-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="e7b88-114">Kliknij przycisk **Pobierz** , aby zainstalować VSIX.</span><span class="sxs-lookup"><span data-stu-id="e7b88-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="e7b88-115">Może zostać wyświetlony monit o ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e7b88-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="e7b88-116">Uruchom lokalnie usługę OData</span><span class="sxs-lookup"><span data-stu-id="e7b88-116">Run the OData Service Locally</span></span>

<span data-ttu-id="e7b88-117">Uruchom projekt ProductService z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e7b88-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="e7b88-118">Domyślnie program Visual Studio uruchamia przeglądarkę w katalogu głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e7b88-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="e7b88-119">Zanotuj identyfikator URI; będzie ona potrzebna w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="e7b88-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="e7b88-120">Pozostaw uruchomioną aplikację.</span><span class="sxs-lookup"><span data-stu-id="e7b88-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="e7b88-121">W przypadku umieszczenia obu projektów w tym samym rozwiązaniu upewnij się, że projekt ProductService został uruchomiony bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="e7b88-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="e7b88-122">W następnym kroku należy zachować uruchomioną usługę podczas modyfikowania projektu aplikacji konsolowej.</span><span class="sxs-lookup"><span data-stu-id="e7b88-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>

## <a name="generate-the-service-proxy"></a><span data-ttu-id="e7b88-123">Generuj serwer proxy usługi</span><span class="sxs-lookup"><span data-stu-id="e7b88-123">Generate the Service Proxy</span></span>

<span data-ttu-id="e7b88-124">Serwer proxy usługi jest klasą .NET, która definiuje metody uzyskiwania dostępu do usługi OData.</span><span class="sxs-lookup"><span data-stu-id="e7b88-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="e7b88-125">Serwer proxy tłumaczy wywołania metod na żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7b88-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="e7b88-126">Klasa proxy zostanie utworzona przez uruchomienie [szablonu T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7b88-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="e7b88-127">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="e7b88-127">Right-click the project.</span></span> <span data-ttu-id="e7b88-128">Wybierz pozycję **dodaj** &gt; **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="e7b88-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="e7b88-129">W oknie dialogowym **Dodaj nowy element** wybierz pozycję **elementy C# wizualne** &gt; **kod** &gt; **klienta OData**.</span><span class="sxs-lookup"><span data-stu-id="e7b88-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="e7b88-130">Nazwij szablon &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="e7b88-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="e7b88-131">Kliknij przycisk **Dodaj** i kliknij ostrzeżenie o zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="e7b88-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="e7b88-132">W tym momencie zostanie wyświetlony komunikat o błędzie, który można zignorować.</span><span class="sxs-lookup"><span data-stu-id="e7b88-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="e7b88-133">Program Visual Studio automatycznie uruchamia szablon, ale w pierwszej kolejności szablon wymaga pewnych ustawień konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e7b88-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="e7b88-134">Otwórz plik ProductClient. OData. config. W elemencie `Parameter` Wklej w identyfikatorze URI z projektu ProductService (poprzedni krok).</span><span class="sxs-lookup"><span data-stu-id="e7b88-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="e7b88-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e7b88-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="e7b88-136">Uruchom ponownie szablon.</span><span class="sxs-lookup"><span data-stu-id="e7b88-136">Run the template again.</span></span> <span data-ttu-id="e7b88-137">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy plik ProductClient.tt i wybierz polecenie **Uruchom narzędzie niestandardowe**.</span><span class="sxs-lookup"><span data-stu-id="e7b88-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="e7b88-138">Szablon tworzy plik kodu o nazwie ProductClient.cs, który definiuje serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="e7b88-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="e7b88-139">Podczas tworzenia aplikacji, jeśli zmienisz punkt końcowy OData, uruchom ponownie szablon, aby zaktualizować serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="e7b88-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="e7b88-140">Używanie serwera proxy usługi do wywoływania usługi OData</span><span class="sxs-lookup"><span data-stu-id="e7b88-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="e7b88-141">Otwórz plik Program.cs i Zastąp kod standardowy następującym.</span><span class="sxs-lookup"><span data-stu-id="e7b88-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="e7b88-142">Zastąp wartość *SERVICEURI* identyfikatorem URI usługi wcześniejszym.</span><span class="sxs-lookup"><span data-stu-id="e7b88-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="e7b88-143">Po uruchomieniu aplikacji należy wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="e7b88-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
