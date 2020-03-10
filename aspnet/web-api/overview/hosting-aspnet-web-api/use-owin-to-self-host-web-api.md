---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Korzystanie z OWIN do samoobsługowego hostowania ASP.NET Web API-ASP.NET 4. x
author: rick-anderson
description: Samouczek z kodem pokazujący, jak hostować interfejs API sieci Web ASP.NET w aplikacji konsolowej.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556540"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="ee3ae-103">Korzystanie z OWIN do samoobsługowego hostowania interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ee3ae-103">Use OWIN to Self-Host ASP.NET Web API</span></span> 

> <span data-ttu-id="ee3ae-104">W tym samouczku pokazano, jak hostować interfejs API sieci Web ASP.NET w aplikacji konsolowej przy użyciu OWIN do samodzielnego hostowania struktury internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-104">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="ee3ae-105">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org) (Owin) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-105">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="ee3ae-106">OWIN oddziela aplikację sieci Web od serwera, co sprawia, że OWIN jest idealnym rozwiązaniem do samodzielnego udostępniania aplikacji sieci Web we własnym procesie poza programem IIS.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-106">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ee3ae-107">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="ee3ae-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="ee3ae-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ee3ae-108">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="ee3ae-109">5\.2.7 internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="ee3ae-109">Web API 5.2.7</span></span>

> [!NOTE]
> <span data-ttu-id="ee3ae-110">Pełny kod źródłowy dla tego samouczka można znaleźć w witrynie [GitHub.com/ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span><span class="sxs-lookup"><span data-stu-id="ee3ae-110">You can find the complete source code for this tutorial at [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="ee3ae-111">Tworzenie aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="ee3ae-111">Create a console application</span></span>

<span data-ttu-id="ee3ae-112">W menu **plik** , **Nowy**, a następnie wybierz **projekt**.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-112">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="ee3ae-113">W **obszarze** **Wizualizacja C#** wybierz pozycję **Windows Desktop** , a następnie wybierz pozycję **Aplikacja konsolowa (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="ee3ae-113">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="ee3ae-114">Nadaj projektowi nazwę "OwinSelfhostSample" i wybierz pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-114">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="ee3ae-115">Dodawanie interfejsu API sieci Web i pakietów OWIN</span><span class="sxs-lookup"><span data-stu-id="ee3ae-115">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="ee3ae-116">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-116">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ee3ae-117">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ee3ae-117">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="ee3ae-118">Spowoduje to zainstalowanie pakietu WebAPI OWIN SelfHost oraz wszystkich wymaganych pakietów OWIN.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-118">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="ee3ae-119">Konfigurowanie interfejsu API sieci Web dla samego hosta</span><span class="sxs-lookup"><span data-stu-id="ee3ae-119">Configure Web API for self-host</span></span>

<span data-ttu-id="ee3ae-120">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** **klasę** / , aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-120">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="ee3ae-121">Nadaj klasie nazwę `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-121">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="ee3ae-122">Zastąp cały kod standardowy w tym pliku następującym:</span><span class="sxs-lookup"><span data-stu-id="ee3ae-122">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="ee3ae-123">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="ee3ae-123">Add a Web API controller</span></span>

<span data-ttu-id="ee3ae-124">Następnie Dodaj klasę kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-124">Next, add a Web API controller class.</span></span> <span data-ttu-id="ee3ae-125">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** **klasę** / , aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-125">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="ee3ae-126">Nadaj klasie nazwę `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-126">Name the class `ValuesController`.</span></span>

<span data-ttu-id="ee3ae-127">Zastąp cały kod standardowy w tym pliku następującym:</span><span class="sxs-lookup"><span data-stu-id="ee3ae-127">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="ee3ae-128">Uruchom hosta OWIN i Utwórz żądanie z HttpClient</span><span class="sxs-lookup"><span data-stu-id="ee3ae-128">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="ee3ae-129">Zastąp cały kod standardowy w pliku Program.cs następującym:</span><span class="sxs-lookup"><span data-stu-id="ee3ae-129">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="ee3ae-130">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ee3ae-130">Run the application</span></span>

<span data-ttu-id="ee3ae-131">Aby uruchomić aplikację, naciśnij klawisz F5 w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee3ae-131">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="ee3ae-132">Dane wyjściowe powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="ee3ae-132">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="ee3ae-133">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ee3ae-133">Additional resources</span></span>

[<span data-ttu-id="ee3ae-134">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="ee3ae-134">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="ee3ae-135">Hostowanie interfejsu API sieci Web ASP.NET w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="ee3ae-135">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
