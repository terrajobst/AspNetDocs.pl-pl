---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Używanie sygnalizującego z Web Apps w Azure App Service | Microsoft Docs
author: bradygaster
description: W tym dokumencie opisano sposób konfigurowania aplikacji sygnalizującej działającej na Microsoft Azure. Wersje oprogramowania używane w samouczku Visual Studio 2013 lub...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558703"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="eb8af-104">Używanie usługi SignalR z funkcją Web Apps w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eb8af-104">Using SignalR with Web Apps in Azure App Service</span></span>

<span data-ttu-id="eb8af-105">[Fletcher Patryka](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="eb8af-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="eb8af-106">W tym dokumencie opisano sposób konfigurowania aplikacji sygnalizującej działającej na Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="eb8af-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="eb8af-107">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="eb8af-107">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="eb8af-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) lub Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="eb8af-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) or Visual Studio 2012</span></span>
> - <span data-ttu-id="eb8af-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="eb8af-109">.NET 4.5</span></span>
> - <span data-ttu-id="eb8af-110">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="eb8af-110">SignalR version 2</span></span>
> - <span data-ttu-id="eb8af-111">Zestaw Azure SDK 2,3 dla Visual Studio 2013 lub 2012</span><span class="sxs-lookup"><span data-stu-id="eb8af-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="eb8af-112">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="eb8af-112">Questions and comments</span></span>
>
> <span data-ttu-id="eb8af-113">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="eb8af-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="eb8af-114">Jeśli masz pytania, które nie są bezpośrednio związane z tym samouczkiem, możesz je ogłosić na [forum ASP.NET sygnalizującego](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/)lub na [forach Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="eb8af-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="eb8af-115">Spis treści</span><span class="sxs-lookup"><span data-stu-id="eb8af-115">Table of Contents</span></span>

- [<span data-ttu-id="eb8af-116">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="eb8af-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="eb8af-117">Wdrażanie aplikacji sieci Web sygnalizującej w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eb8af-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="eb8af-118">Włączanie obiektów WebSockets na Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eb8af-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="eb8af-119">Korzystanie z Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="eb8af-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="eb8af-120">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="eb8af-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="eb8af-121">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="eb8af-121">Introduction</span></span>

<span data-ttu-id="eb8af-122">Sygnalizujący ASP.NET może służyć do przeprowadzenia nowego poziomu interakcji między serwerami i klientami sieci Web lub .NET.</span><span class="sxs-lookup"><span data-stu-id="eb8af-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="eb8af-123">Na platformie Azure aplikacje sygnalizujące mogą korzystać z wysoce dostępnych, skalowalnych i wydajnych środowisk, które działają w chmurze.</span><span class="sxs-lookup"><span data-stu-id="eb8af-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="eb8af-124">Wdrażanie aplikacji sieci Web sygnalizującej w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eb8af-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="eb8af-125">Sygnalizujący nie dodaje szczególnych komplikacji do wdrożenia aplikacji na platformie Azure, a wdrożenie na serwerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="eb8af-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="eb8af-126">Aplikacja, która korzysta z programu sygnalizującego, może być hostowana na platformie Azure bez wprowadzania zmian w konfiguracji ani innych ustawień (w przypadku obsługi usługi WebSockets znajduje się [w temacie Włączanie obiektów WebSockets na Azure App Service](#websocket) poniżej). W tym samouczku zostanie wdrożona aplikacja utworzona w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="eb8af-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="eb8af-127">**Wymagania wstępne**</span><span class="sxs-lookup"><span data-stu-id="eb8af-127">**Prerequisites**</span></span>

- <span data-ttu-id="eb8af-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="eb8af-128">Visual Studio 2013.</span></span> <span data-ttu-id="eb8af-129">Jeśli nie masz programu Visual Studio, Visual Studio 2013 Express for Web jest dołączany do instalacji zestawu Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="eb8af-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="eb8af-130">[Zestaw Azure sdk 2,3 dla Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) lub [Azure SDK 2,3 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="eb8af-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="eb8af-131">Do ukończenia tego samouczka potrzebna jest subskrypcja platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="eb8af-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="eb8af-132">Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)lub [zarejestrować się w celu uzyskania subskrypcji wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eb8af-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="eb8af-133">Wdrażanie aplikacji sieci Web sygnalizującej na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="eb8af-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="eb8af-134">Ukończ [samouczek wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md)lub Pobierz gotowy projekt z [galerii kodu](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="eb8af-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="eb8af-135">W programie Visual Studio wybierz kolejno opcje **kompilacja**i **rozmowa z czatem**.</span><span class="sxs-lookup"><span data-stu-id="eb8af-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="eb8af-136">W oknie dialogowym "Publikowanie w sieci Web" Wybierz pozycję "witryny sieci Web systemu Windows Azure".</span><span class="sxs-lookup"><span data-stu-id="eb8af-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Wybieranie witryn sieci Web systemu Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="eb8af-138">Jeśli nie zalogowano się do konto Microsoft, kliknij przycisk **Zaloguj się...** w oknie dialogowym "Wybierz istniejącą witrynę sieci Web" i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="eb8af-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Wybierz istniejącą witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image2.png)    ![Logowanie do platformy Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="eb8af-141">W oknie dialogowym "Wybierz istniejącą witrynę sieci Web" kliknij pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="eb8af-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nowa witryna sieci Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="eb8af-143">W oknie dialogowym "Tworzenie witryny w systemie Windows Azure" Wprowadź unikatową nazwę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eb8af-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="eb8af-144">Wybierz region znajdujący się najbliżej siebie na liście rozwijanej region.</span><span class="sxs-lookup"><span data-stu-id="eb8af-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="eb8af-145">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="eb8af-145">Click **Create**.</span></span>

    ![Tworzenie witryny na platformie Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="eb8af-147">W oknie dialogowym "Publikowanie w sieci Web" kliknij pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="eb8af-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Publikuj witrynę](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="eb8af-149">Gdy aplikacja zakończyła publikowanie, aplikacja rozmowy sygnalizująca hostowana w Azure App Service Web Apps zostanie otwarta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="eb8af-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Otwieranie witryny w przeglądarce](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="eb8af-151">Włączanie obiektów WebSockets na Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="eb8af-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="eb8af-152">Obiekty WebSockets muszą być jawnie włączone w aplikacji sieci Web, aby można było ich używać w aplikacji sygnalizującej; w przeciwnym razie będą używane inne protokoły (zobacz [transporty i rezerwy](../getting-started/introduction-to-signalr.md#transports) , aby uzyskać szczegółowe informacje).</span><span class="sxs-lookup"><span data-stu-id="eb8af-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="eb8af-153">Aby można było używać obiektów WebSockets w Azure App Service Web Apps, włącz je w sekcji konfiguracji aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="eb8af-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="eb8af-154">Aby to zrobić, Otwórz aplikację sieci Web w [usłudze Azure Portal zarządzania](https://manage.windowsazure.com/)i wybierz pozycję Konfiguruj.</span><span class="sxs-lookup"><span data-stu-id="eb8af-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Karta Konfigurowanie](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="eb8af-156">W górnej części strony konfiguracji upewnij się, że dla aplikacji sieci Web jest używany program .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="eb8af-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Ustawienie programu .NET Framework w wersji 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="eb8af-158">Na stronie Konfiguracja w obszarze Ustawienia **WebSockets** wybierz pozycję **włączone**.</span><span class="sxs-lookup"><span data-stu-id="eb8af-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Ustawienie obiektów WebSockets: włączone](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="eb8af-160">W dolnej części strony konfiguracja wybierz pozycję **Zapisz** , aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="eb8af-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Zapisz ustawienia](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="eb8af-162">Korzystanie z Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="eb8af-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="eb8af-163">W przypadku korzystania z wielu wystąpień aplikacji sieci Web, a użytkownicy tych wystąpień muszą współistnieć ze sobą (w związku z tym, że na przykład komunikaty czatu utworzone w jednym wystąpieniu mogą dotrzeć do użytkowników połączonych z innymi wystąpieniami), Azure Redis Cache w aplikacji musi być zaimplementowany [Plan](../performance/scaleout-with-redis.md) samodzielny.</span><span class="sxs-lookup"><span data-stu-id="eb8af-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="eb8af-164">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="eb8af-164">Next Steps</span></span>

<span data-ttu-id="eb8af-165">Aby uzyskać więcej informacji na Web Apps w Azure App Service, zobacz [omówienie Web Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="eb8af-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
