---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Skalowania sygnalizujący z Azure Service Bus | Microsoft Docs
author: bradygaster
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizującego w wersji 2 poprzednie wersje tego tematu dla sygnalizującego 1. x wersji tego tematu,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579178"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="01774-103">SignalR — skalowanie w poziomie z użyciem usługi Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="01774-103">SignalR Scaleout with Azure Service Bus</span></span>

<span data-ttu-id="01774-104">według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="01774-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="01774-105">W tym samouczku zostanie wdrożona aplikacja sygnalizująca w roli sieci Web systemu Windows Azure, przy użyciu planu Service Bus do dystrybucji komunikatów do każdego wystąpienia roli.</span><span class="sxs-lookup"><span data-stu-id="01774-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="01774-106">(Można również użyć planu Service Bus z [aplikacjami sieci Web w programie Azure App Service](https://docs.microsoft.com/azure/app-service-web/)).</span><span class="sxs-lookup"><span data-stu-id="01774-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="01774-107">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="01774-107">Prerequisites:</span></span>

- <span data-ttu-id="01774-108">Konto platformy Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="01774-108">A Windows Azure account.</span></span>
- <span data-ttu-id="01774-109">[Zestaw SDK systemu Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="01774-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="01774-110">Program Visual Studio 2012 lub 2013.</span><span class="sxs-lookup"><span data-stu-id="01774-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="01774-111">Plan usługi Service Bus jest również zgodny z [Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)w wersji 1,1.</span><span class="sxs-lookup"><span data-stu-id="01774-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="01774-112">Nie jest to jednak zgodne z wersją 1,0 Service Bus dla systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="01774-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="01774-113">Ceny</span><span class="sxs-lookup"><span data-stu-id="01774-113">Pricing</span></span>

<span data-ttu-id="01774-114">W przypadku korzystania z programu do przesyłania komunikatów Service Bus</span><span class="sxs-lookup"><span data-stu-id="01774-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="01774-115">Aby uzyskać najnowsze informacje o cenach, zobacz [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="01774-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="01774-116">W czasie tego pisania możesz wysyłać 1 000 000 komunikatów miesięcznie przez mniej niż $1.</span><span class="sxs-lookup"><span data-stu-id="01774-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="01774-117">Plan wyjścia wysyła komunikat usługi Service Bus dla każdego wywołania metody centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="01774-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="01774-118">Istnieją także pewne komunikaty sterujące dotyczące połączeń, rozłączeń, łączenia lub opuszczania grup itd.</span><span class="sxs-lookup"><span data-stu-id="01774-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="01774-119">W większości aplikacji większość ruchu komunikatów będzie stanowić wywołania metody centrum.</span><span class="sxs-lookup"><span data-stu-id="01774-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="01774-120">Omówienie</span><span class="sxs-lookup"><span data-stu-id="01774-120">Overview</span></span>

<span data-ttu-id="01774-121">Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.</span><span class="sxs-lookup"><span data-stu-id="01774-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="01774-122">Użyj Azure Portal systemu Windows, aby utworzyć nową przestrzeń nazw Service Bus.</span><span class="sxs-lookup"><span data-stu-id="01774-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="01774-123">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="01774-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="01774-124">Microsoft. AspNet. Signal</span><span class="sxs-lookup"><span data-stu-id="01774-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="01774-125">[Microsoft. ASPNET. Signal. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) lub [Microsoft. ASPNET. Signal. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="01774-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="01774-126">Tworzenie aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="01774-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="01774-127">Dodaj następujący kod do Startup.cs w celu skonfigurowania planu:</span><span class="sxs-lookup"><span data-stu-id="01774-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="01774-128">Ten kod konfiguruje plan z wartościami domyślnymi dla [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="01774-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="01774-129">Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajność sygnałów: metryki skalowania](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="01774-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="01774-130">Dla każdej aplikacji należy wybrać inną wartość dla "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="01774-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="01774-131">Nie należy używać tej samej wartości w wielu aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="01774-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="01774-132">Tworzenie usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="01774-132">Create the Azure Services</span></span>

<span data-ttu-id="01774-133">Utwórz usługę w chmurze, zgodnie z opisem w temacie [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="01774-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="01774-134">Wykonaj kroki opisane w sekcji "Instrukcje: Tworzenie usługi w chmurze przy użyciu funkcji Szybkie tworzenie".</span><span class="sxs-lookup"><span data-stu-id="01774-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="01774-135">Na potrzeby tego samouczka nie trzeba przekazywać certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="01774-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="01774-136">Utwórz nową przestrzeń nazw Service Bus, zgodnie z opisem w artykule [Service Bus tematy/subskrypcje](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="01774-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="01774-137">Postępuj zgodnie z instrukcjami w sekcji "Tworzenie przestrzeni nazw usługi".</span><span class="sxs-lookup"><span data-stu-id="01774-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="01774-138">Upewnij się, że wybrano ten sam region dla usługi w chmurze i przestrzeni nazw Service Bus.</span><span class="sxs-lookup"><span data-stu-id="01774-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="01774-139">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01774-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="01774-140">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01774-140">Start Visual Studio.</span></span> <span data-ttu-id="01774-141">W menu **Plik** kliknij pozycję **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="01774-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="01774-142">W oknie dialogowym **Nowy projekt** rozwiń **element Wizualizacja C#** .</span><span class="sxs-lookup"><span data-stu-id="01774-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="01774-143">W obszarze **zainstalowane szablony**wybierz pozycję **chmura** , a następnie wybierz pozycję **Usługa w chmurze platformy Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="01774-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="01774-144">Zachowaj domyślne .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="01774-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="01774-145">Nadaj aplikacji nazwę ChatService, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="01774-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="01774-146">W oknie dialogowym **Nowa usługa w chmurze platformy Microsoft Azure** wybierz pozycję ASP.NET Web role.</span><span class="sxs-lookup"><span data-stu-id="01774-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="01774-147">Kliknij przycisk strzałki w prawo ( **&gt;** ), aby dodać rolę do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="01774-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="01774-148">Umieść wskaźnik myszy nad nową rolą, aby ikona ołówka była widoczna.</span><span class="sxs-lookup"><span data-stu-id="01774-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="01774-149">Kliknij tę ikonę, aby zmienić nazwę roli.</span><span class="sxs-lookup"><span data-stu-id="01774-149">Click this icon to rename the role.</span></span> <span data-ttu-id="01774-150">Nazwij rolę "SignalRChat" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="01774-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="01774-151">W oknie dialogowym **Nowy projekt ASP.NET** wybierz pozycję **MVC**, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="01774-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="01774-152">Kreator projektu tworzy dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="01774-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="01774-153">ChatService: ten projekt jest aplikacją systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="01774-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="01774-154">Definiuje role platformy Azure i inne opcje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="01774-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="01774-155">SignalRChat: ten projekt jest projektem ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="01774-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="01774-156">Tworzenie aplikacji czatu dla sygnałów</span><span class="sxs-lookup"><span data-stu-id="01774-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="01774-157">Aby utworzyć aplikację czatu, wykonaj kroki opisane w samouczku [wprowadzenie z sygnałami i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="01774-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="01774-158">Zainstaluj wymagane biblioteki przy użyciu narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="01774-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="01774-159">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="01774-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="01774-160">W oknie **konsola Menedżera pakietów** wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="01774-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="01774-161">Użyj opcji `-ProjectName`, aby zainstalować pakiety w projekcie ASP.NET MVC, a nie w projekcie systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="01774-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="01774-162">Konfigurowanie planu</span><span class="sxs-lookup"><span data-stu-id="01774-162">Configure the Backplane</span></span>

<span data-ttu-id="01774-163">W pliku Startup.cs aplikacji Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="01774-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="01774-164">Teraz musisz uzyskać parametry połączenia usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="01774-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="01774-165">W Azure Portal Wybierz utworzoną przestrzeń nazw usługi Service Bus, a następnie kliknij ikonę klucz dostępu.</span><span class="sxs-lookup"><span data-stu-id="01774-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="01774-166">Skopiuj parametry połączenia do schowka, a następnie wklej je do zmiennej *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="01774-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="01774-167">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="01774-167">Deploy to Azure</span></span>

<span data-ttu-id="01774-168">W Eksplorator rozwiązań rozwiń folder **role** w projekcie ChatService.</span><span class="sxs-lookup"><span data-stu-id="01774-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="01774-169">Kliknij prawym przyciskiem myszy rolę SignalRChat i wybierz pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="01774-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="01774-170">Wybierz kartę **Konfiguracja** . W obszarze **wystąpienia** wybierz pozycję 2.</span><span class="sxs-lookup"><span data-stu-id="01774-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="01774-171">Rozmiar maszyny wirtualnej można również ustawić na **bardzo mały**.</span><span class="sxs-lookup"><span data-stu-id="01774-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="01774-172">Zapisz zmiany.</span><span class="sxs-lookup"><span data-stu-id="01774-172">Save the changes.</span></span>

<span data-ttu-id="01774-173">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="01774-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="01774-174">Wybierz pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="01774-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="01774-175">Jeśli jest to pierwsze publikowanie na platformie Microsoft Azure, musisz pobrać swoje poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="01774-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="01774-176">W kreatorze **publikacji** kliknij pozycję "Zaloguj się, aby pobrać poświadczenia".</span><span class="sxs-lookup"><span data-stu-id="01774-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="01774-177">Spowoduje to wyświetlenie monitu o zalogowanie się do Azure Portal systemu Windows i pobranie pliku ustawień publikowania.</span><span class="sxs-lookup"><span data-stu-id="01774-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="01774-178">Kliknij przycisk **Importuj** i wybierz pobrany plik ustawień publikowania.</span><span class="sxs-lookup"><span data-stu-id="01774-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="01774-179">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="01774-179">Click **Next**.</span></span> <span data-ttu-id="01774-180">W oknie dialogowym **Ustawienia publikowania** w obszarze **Usługa w chmurze**Wybierz utworzoną wcześniej usługę w chmurze.</span><span class="sxs-lookup"><span data-stu-id="01774-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="01774-181">Kliknij przycisk **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="01774-181">Click **Publish**.</span></span> <span data-ttu-id="01774-182">Wdrożenie aplikacji i uruchomienie maszyn wirtualnych może potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="01774-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="01774-183">Teraz po uruchomieniu aplikacji czatu wystąpienia roli komunikują się za pośrednictwem Azure Service Bus przy użyciu Service Bus temacie.</span><span class="sxs-lookup"><span data-stu-id="01774-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="01774-184">Temat jest kolejką komunikatów, która umożliwia korzystanie z wielu subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="01774-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="01774-185">Plan "automatycznie" tworzy temat i subskrypcje.</span><span class="sxs-lookup"><span data-stu-id="01774-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="01774-186">Aby zobaczyć działanie subskrypcje i komunikaty, Otwórz Azure Portal, wybierz przestrzeń nazw Service Bus i kliknij pozycję tematy.</span><span class="sxs-lookup"><span data-stu-id="01774-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="01774-187">Wyświetlenie działania komunikatu na pulpicie nawigacyjnym potrwa kilka minut.</span><span class="sxs-lookup"><span data-stu-id="01774-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="01774-188">Program sygnalizujący zarządza okresem istnienia tematu.</span><span class="sxs-lookup"><span data-stu-id="01774-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="01774-189">Dopóki aplikacja jest wdrożona, nie próbuj ręcznie usuwać tematów ani zmieniać ustawień w temacie.</span><span class="sxs-lookup"><span data-stu-id="01774-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="01774-190">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="01774-190">Troubleshooting</span></span>

<span data-ttu-id="01774-191">**System. InvalidOperationException "jedyne obsługiwane IsolationLevel to" IsolationLevel. Serializable "."**</span><span class="sxs-lookup"><span data-stu-id="01774-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="01774-192">Ten błąd może wystąpić, jeśli poziom transakcji dla operacji jest ustawiony na coś innego niż `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="01774-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="01774-193">Sprawdź, czy żadne operacje nie są wykonywane z innymi poziomami transakcji.</span><span class="sxs-lookup"><span data-stu-id="01774-193">Verify that no operations are being performed with other transaction levels.</span></span>
