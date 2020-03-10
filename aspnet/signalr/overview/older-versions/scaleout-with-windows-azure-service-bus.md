---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Skalowania sygnalizujący z Azure Service Bus (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558416"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="532b7-102">SignalR — skalowanie w poziomie z użyciem usługi Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="532b7-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>

<span data-ttu-id="532b7-103">według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="532b7-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="532b7-104">W tym samouczku zostanie wdrożona aplikacja sygnalizująca w roli sieci Web systemu Windows Azure, przy użyciu planu Service Bus do dystrybucji komunikatów do każdego wystąpienia roli.</span><span class="sxs-lookup"><span data-stu-id="532b7-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="532b7-105">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="532b7-105">Prerequisites:</span></span>

- <span data-ttu-id="532b7-106">Konto platformy Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="532b7-106">A Windows Azure account.</span></span>
- <span data-ttu-id="532b7-107">[Zestaw SDK systemu Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="532b7-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="532b7-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="532b7-108">Visual Studio 2012.</span></span>

<span data-ttu-id="532b7-109">Plan usługi Service Bus jest również zgodny z [Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)w wersji 1,1.</span><span class="sxs-lookup"><span data-stu-id="532b7-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="532b7-110">Nie jest to jednak zgodne z wersją 1,0 Service Bus dla systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="532b7-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="532b7-111">Ceny</span><span class="sxs-lookup"><span data-stu-id="532b7-111">Pricing</span></span>

<span data-ttu-id="532b7-112">W przypadku korzystania z programu do przesyłania komunikatów Service Bus</span><span class="sxs-lookup"><span data-stu-id="532b7-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="532b7-113">Aby uzyskać najnowsze informacje o cenach, zobacz [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="532b7-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="532b7-114">W czasie tego pisania możesz wysyłać 1 000 000 komunikatów miesięcznie przez mniej niż $1.</span><span class="sxs-lookup"><span data-stu-id="532b7-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="532b7-115">Plan wyjścia wysyła komunikat usługi Service Bus dla każdego wywołania metody centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="532b7-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="532b7-116">Istnieją także pewne komunikaty sterujące dotyczące połączeń, rozłączeń, łączenia lub opuszczania grup itd.</span><span class="sxs-lookup"><span data-stu-id="532b7-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="532b7-117">W większości aplikacji większość ruchu komunikatów będzie stanowić wywołania metody centrum.</span><span class="sxs-lookup"><span data-stu-id="532b7-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="532b7-118">Omówienie</span><span class="sxs-lookup"><span data-stu-id="532b7-118">Overview</span></span>

<span data-ttu-id="532b7-119">Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.</span><span class="sxs-lookup"><span data-stu-id="532b7-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="532b7-120">Użyj Azure Portal systemu Windows, aby utworzyć nową przestrzeń nazw Service Bus.</span><span class="sxs-lookup"><span data-stu-id="532b7-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="532b7-121">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="532b7-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="532b7-122">Microsoft. AspNet. Signal</span><span class="sxs-lookup"><span data-stu-id="532b7-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="532b7-123">Microsoft. AspNet. Signal. ServiceBus</span><span class="sxs-lookup"><span data-stu-id="532b7-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="532b7-124">Tworzenie aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="532b7-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="532b7-125">Dodaj następujący kod do szablonu Global. asax, aby skonfigurować plan:</span><span class="sxs-lookup"><span data-stu-id="532b7-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="532b7-126">Dla każdej aplikacji należy wybrać inną wartość dla "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="532b7-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="532b7-127">Nie należy używać tej samej wartości w wielu aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="532b7-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="532b7-128">Tworzenie usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="532b7-128">Create the Azure Services</span></span>

<span data-ttu-id="532b7-129">Utwórz usługę w chmurze, zgodnie z opisem w temacie [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="532b7-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="532b7-130">Wykonaj kroki opisane w sekcji "Instrukcje: Tworzenie usługi w chmurze przy użyciu funkcji Szybkie tworzenie".</span><span class="sxs-lookup"><span data-stu-id="532b7-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="532b7-131">Na potrzeby tego samouczka nie trzeba przekazywać certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="532b7-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="532b7-132">Utwórz nową przestrzeń nazw Service Bus, zgodnie z opisem w artykule [Service Bus tematy/subskrypcje](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="532b7-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="532b7-133">Postępuj zgodnie z instrukcjami w sekcji "Tworzenie przestrzeni nazw usługi".</span><span class="sxs-lookup"><span data-stu-id="532b7-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="532b7-134">Upewnij się, że wybrano ten sam region dla usługi w chmurze i przestrzeni nazw Service Bus.</span><span class="sxs-lookup"><span data-stu-id="532b7-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="532b7-135">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="532b7-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="532b7-136">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="532b7-136">Start Visual Studio.</span></span> <span data-ttu-id="532b7-137">W menu **Plik** kliknij pozycję **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="532b7-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="532b7-138">W oknie dialogowym **Nowy projekt** rozwiń **element Wizualizacja C#** .</span><span class="sxs-lookup"><span data-stu-id="532b7-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="532b7-139">W obszarze **zainstalowane szablony**wybierz pozycję **chmura** , a następnie wybierz pozycję **Usługa w chmurze platformy Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="532b7-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="532b7-140">Zachowaj domyślne .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="532b7-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="532b7-141">Nadaj aplikacji nazwę ChatService, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="532b7-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="532b7-142">W oknie dialogowym **Nowa usługa w chmurze platformy Microsoft Azure** wybierz rolę sieci Web ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="532b7-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="532b7-143">Kliknij przycisk strzałki w prawo ( **&gt;** ), aby dodać rolę do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="532b7-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="532b7-144">Umieść wskaźnik myszy nad nową rolą, aby ikona ołówka była widoczna.</span><span class="sxs-lookup"><span data-stu-id="532b7-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="532b7-145">Kliknij tę ikonę, aby zmienić nazwę roli.</span><span class="sxs-lookup"><span data-stu-id="532b7-145">Click this icon to rename the role.</span></span> <span data-ttu-id="532b7-146">Nazwij rolę "SignalRChat" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="532b7-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="532b7-147">W kreatorze **nowego projektu ASP.NET MVC 4** wybierz pozycję **aplikacja internetowa**.</span><span class="sxs-lookup"><span data-stu-id="532b7-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="532b7-148">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="532b7-148">Click **OK**.</span></span> <span data-ttu-id="532b7-149">Kreator projektu tworzy dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="532b7-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="532b7-150">ChatService: ten projekt jest aplikacją systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="532b7-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="532b7-151">Definiuje role platformy Azure i inne opcje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="532b7-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="532b7-152">SignalRChat: ten projekt jest projektem ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="532b7-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="532b7-153">Tworzenie aplikacji czatu dla sygnałów</span><span class="sxs-lookup"><span data-stu-id="532b7-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="532b7-154">Aby utworzyć aplikację czatu, wykonaj kroki opisane w samouczku [wprowadzenie z sygnałami i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="532b7-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="532b7-155">Zainstaluj wymagane biblioteki przy użyciu narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="532b7-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="532b7-156">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="532b7-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="532b7-157">W oknie **konsola Menedżera pakietów** wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="532b7-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="532b7-158">Użyj opcji `-ProjectName`, aby zainstalować pakiety w projekcie ASP.NET MVC, a nie w projekcie systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="532b7-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="532b7-159">Konfigurowanie planu</span><span class="sxs-lookup"><span data-stu-id="532b7-159">Configure the Backplane</span></span>

<span data-ttu-id="532b7-160">W pliku Global. asax aplikacji Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="532b7-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="532b7-161">Teraz musisz uzyskać parametry połączenia usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="532b7-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="532b7-162">W Azure Portal Wybierz utworzoną przestrzeń nazw usługi Service Bus, a następnie kliknij ikonę klucz dostępu.</span><span class="sxs-lookup"><span data-stu-id="532b7-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="532b7-163">Skopiuj parametry połączenia do schowka, a następnie wklej je do zmiennej *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="532b7-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="532b7-164">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="532b7-164">Deploy to Azure</span></span>

<span data-ttu-id="532b7-165">W Eksplorator rozwiązań rozwiń folder **role** w projekcie ChatService.</span><span class="sxs-lookup"><span data-stu-id="532b7-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="532b7-166">Kliknij prawym przyciskiem myszy rolę SignalRChat i wybierz pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="532b7-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="532b7-167">Wybierz kartę **Konfiguracja** . W obszarze **wystąpienia** wybierz pozycję 2.</span><span class="sxs-lookup"><span data-stu-id="532b7-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="532b7-168">Rozmiar maszyny wirtualnej można również ustawić na **bardzo mały**.</span><span class="sxs-lookup"><span data-stu-id="532b7-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="532b7-169">Zapisz zmiany.</span><span class="sxs-lookup"><span data-stu-id="532b7-169">Save the changes.</span></span>

<span data-ttu-id="532b7-170">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="532b7-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="532b7-171">Wybierz pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="532b7-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="532b7-172">Jeśli jest to pierwsze publikowanie na platformie Microsoft Azure, musisz pobrać swoje poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="532b7-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="532b7-173">W kreatorze **publikacji** kliknij pozycję "Zaloguj się, aby pobrać poświadczenia".</span><span class="sxs-lookup"><span data-stu-id="532b7-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="532b7-174">Spowoduje to wyświetlenie monitu o zalogowanie się do Azure Portal systemu Windows i pobranie pliku ustawień publikowania.</span><span class="sxs-lookup"><span data-stu-id="532b7-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="532b7-175">Kliknij przycisk **Importuj** i wybierz pobrany plik ustawień publikowania.</span><span class="sxs-lookup"><span data-stu-id="532b7-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="532b7-176">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="532b7-176">Click **Next**.</span></span> <span data-ttu-id="532b7-177">W oknie dialogowym **Ustawienia publikowania** w obszarze **Usługa w chmurze**Wybierz utworzoną wcześniej usługę w chmurze.</span><span class="sxs-lookup"><span data-stu-id="532b7-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="532b7-178">Kliknij przycisk **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="532b7-178">Click **Publish**.</span></span> <span data-ttu-id="532b7-179">Wdrożenie aplikacji i uruchomienie maszyn wirtualnych może potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="532b7-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="532b7-180">Teraz po uruchomieniu aplikacji czatu wystąpienia roli komunikują się za pośrednictwem Azure Service Bus przy użyciu Service Bus temacie.</span><span class="sxs-lookup"><span data-stu-id="532b7-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="532b7-181">Temat jest kolejką komunikatów, która umożliwia korzystanie z wielu subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="532b7-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="532b7-182">Plan "automatycznie" tworzy temat i subskrypcje.</span><span class="sxs-lookup"><span data-stu-id="532b7-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="532b7-183">Aby zobaczyć działanie subskrypcje i komunikaty, Otwórz Azure Portal, wybierz przestrzeń nazw Service Bus i kliknij pozycję tematy.</span><span class="sxs-lookup"><span data-stu-id="532b7-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="532b7-184">Wyświetlenie działania komunikatu na pulpicie nawigacyjnym potrwa kilka minut.</span><span class="sxs-lookup"><span data-stu-id="532b7-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="532b7-185">Program sygnalizujący zarządza okresem istnienia tematu.</span><span class="sxs-lookup"><span data-stu-id="532b7-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="532b7-186">Dopóki aplikacja jest wdrożona, nie próbuj ręcznie usuwać tematów ani zmieniać ustawień w temacie.</span><span class="sxs-lookup"><span data-stu-id="532b7-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
