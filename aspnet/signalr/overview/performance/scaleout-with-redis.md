---
uid: signalr/overview/performance/scaleout-with-redis
title: Skalowania sygnalizujący Redis | Microsoft Docs
author: bradygaster
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizującego w wersji 2 poprzednie wersje tego tematu, aby uzyskać informacje o wcześniejszych wersjach programu...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579227"
---
# <a name="signalr-scaleout-with-redis"></a><span data-ttu-id="24194-103">SignalR — skalowanie w poziomie z użyciem pamięci podręcznej Redis</span><span class="sxs-lookup"><span data-stu-id="24194-103">SignalR Scaleout with Redis</span></span>

<span data-ttu-id="24194-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="24194-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="24194-105">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="24194-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="24194-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="24194-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="24194-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="24194-107">.NET 4.5</span></span>
> - <span data-ttu-id="24194-108">Sygnalizacja w wersji 2,4</span><span class="sxs-lookup"><span data-stu-id="24194-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="24194-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="24194-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="24194-110">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="24194-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="24194-111">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="24194-111">Questions and comments</span></span>
>
> <span data-ttu-id="24194-112">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="24194-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="24194-113">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="24194-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="24194-114">W tym samouczku będziesz używać [Redis](http://redis.io/) do dystrybuowania komunikatów w aplikacji sygnalizującej wdrożonej na dwóch oddzielnych WYSTĄPIENIACH usług IIS.</span><span class="sxs-lookup"><span data-stu-id="24194-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="24194-115">Redis jest magazynem wartości w pamięci.</span><span class="sxs-lookup"><span data-stu-id="24194-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="24194-116">Obsługuje również system obsługi komunikatów z modelem publikowania/subskrybowania.</span><span class="sxs-lookup"><span data-stu-id="24194-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="24194-117">Funkcja Rediser programu sygnalizującego używa funkcji pub/sub do przesyłania dalej komunikatów do innych serwerów.</span><span class="sxs-lookup"><span data-stu-id="24194-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="24194-118">W tym samouczku zostaną użyte trzy serwery:</span><span class="sxs-lookup"><span data-stu-id="24194-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="24194-119">Dwa serwery z systemem Windows, które będą używane do wdrażania aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="24194-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="24194-120">Jeden serwer z systemem Linux, który będzie używany do uruchamiania Redis.</span><span class="sxs-lookup"><span data-stu-id="24194-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="24194-121">W przypadku zrzutów ekranu w tym samouczku użyto Ubuntu 12,04 TLS.</span><span class="sxs-lookup"><span data-stu-id="24194-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="24194-122">Jeśli nie masz trzech serwerów fizycznych do użycia, możesz utworzyć maszyny wirtualne w funkcji Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="24194-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="24194-123">Innym rozwiązaniem jest utworzenie maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="24194-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="24194-124">Chociaż w tym samouczku jest stosowana Oficjalna implementacja Redis, istnieje również [port systemu Windows Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="24194-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="24194-125">Konfiguracja i konfiguracja są różne, ale w przeciwnym razie kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="24194-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="24194-126">Usługa sygnalizująca skalowania z Redis nie obsługuje klastrów Redis.</span><span class="sxs-lookup"><span data-stu-id="24194-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="24194-127">Omówienie</span><span class="sxs-lookup"><span data-stu-id="24194-127">Overview</span></span>

<span data-ttu-id="24194-128">Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.</span><span class="sxs-lookup"><span data-stu-id="24194-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="24194-129">Zainstaluj program Redis i uruchom serwer Redis.</span><span class="sxs-lookup"><span data-stu-id="24194-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="24194-130">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="24194-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="24194-131">Microsoft. AspNet. Signal</span><span class="sxs-lookup"><span data-stu-id="24194-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="24194-132">Microsoft. AspNet. Signal. StackExchangeRedis</span><span class="sxs-lookup"><span data-stu-id="24194-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="24194-133">Tworzenie aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="24194-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="24194-134">Dodaj następujący kod do Startup.cs w celu skonfigurowania planu:</span><span class="sxs-lookup"><span data-stu-id="24194-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="24194-135">Ubuntu na funkcji Hyper-V</span><span class="sxs-lookup"><span data-stu-id="24194-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="24194-136">Korzystając z funkcji Windows Hyper-V, można łatwo utworzyć maszynę wirtualną Ubuntu w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="24194-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="24194-137">Pobierz plik ISO Ubuntu z [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="24194-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="24194-138">W funkcji Hyper-V Dodaj nową maszynę wirtualną.</span><span class="sxs-lookup"><span data-stu-id="24194-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="24194-139">W kroku **Podłączanie wirtualnego dysku twardego** wybierz pozycję **Utwórz wirtualny dysk twardy**.</span><span class="sxs-lookup"><span data-stu-id="24194-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="24194-140">W kroku **Opcje instalacji** wybierz pozycję **plik obrazu (. ISO)** , kliknij przycisk **Przeglądaj**, a następnie przejdź do Ubuntu instalacji ISO.</span><span class="sxs-lookup"><span data-stu-id="24194-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="24194-141">Zainstaluj Redis</span><span class="sxs-lookup"><span data-stu-id="24194-141">Install Redis</span></span>

<span data-ttu-id="24194-142">Postępuj zgodnie z instrukcjami w [http://redis.io/download](http://redis.io/download) , aby pobrać i skompilować Redis.</span><span class="sxs-lookup"><span data-stu-id="24194-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="24194-143">Spowoduje to skompilowanie plików binarnych Redis w katalogu `src`.</span><span class="sxs-lookup"><span data-stu-id="24194-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="24194-144">Domyślnie Redis nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="24194-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="24194-145">Aby ustawić hasło, edytuj plik `redis.conf`, który znajduje się w katalogu głównym kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="24194-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="24194-146">(Utwórz kopię zapasową pliku przed edycją!) Dodaj następującą dyrektywę do `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="24194-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="24194-147">Teraz uruchom serwer Redis:</span><span class="sxs-lookup"><span data-stu-id="24194-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="24194-148">Otwórz port 6379, który jest domyślnym portem, który Redis nasłuchuje.</span><span class="sxs-lookup"><span data-stu-id="24194-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="24194-149">(Numer portu można zmienić w pliku konfiguracyjnym).</span><span class="sxs-lookup"><span data-stu-id="24194-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="24194-150">Tworzenie aplikacji sygnalizującej</span><span class="sxs-lookup"><span data-stu-id="24194-150">Create the SignalR Application</span></span>

<span data-ttu-id="24194-151">Utwórz aplikację sygnalizującą, wykonując jeden z następujących samouczków:</span><span class="sxs-lookup"><span data-stu-id="24194-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="24194-152">Wprowadzenie z sygnałem 2,0</span><span class="sxs-lookup"><span data-stu-id="24194-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="24194-153">Wprowadzenie z sygnałami 2,0 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="24194-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="24194-154">Następnie zmodyfikujemy aplikację czatu, aby obsługiwała skalowania z Redis.</span><span class="sxs-lookup"><span data-stu-id="24194-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="24194-155">Najpierw Dodaj pakiet NuGet `Microsoft.AspNet.SignalR.StackExchangeRedis` do projektu.</span><span class="sxs-lookup"><span data-stu-id="24194-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="24194-156">W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="24194-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="24194-157">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="24194-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="24194-158">Następnie otwórz plik Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="24194-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="24194-159">Dodaj następujący kod do metody **konfiguracji** :</span><span class="sxs-lookup"><span data-stu-id="24194-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="24194-160">"serwer" jest nazwą serwera, na którym działa Redis.</span><span class="sxs-lookup"><span data-stu-id="24194-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="24194-161">*port* jest numerem portu</span><span class="sxs-lookup"><span data-stu-id="24194-161">*port* is the port number</span></span>
- <span data-ttu-id="24194-162">"hasło" to hasło zdefiniowane w pliku Redis. conf.</span><span class="sxs-lookup"><span data-stu-id="24194-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="24194-163">"Nazwa_aplikacji" to dowolny ciąg.</span><span class="sxs-lookup"><span data-stu-id="24194-163">"AppName" is any string.</span></span> <span data-ttu-id="24194-164">Program sygnalizujący tworzy kanał Redis pub/Sub o tej nazwie.</span><span class="sxs-lookup"><span data-stu-id="24194-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="24194-165">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="24194-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="24194-166">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="24194-166">Deploy and Run the Application</span></span>

<span data-ttu-id="24194-167">Przygotuj wystąpienia systemu Windows Server, aby wdrożyć aplikację sygnalizującą.</span><span class="sxs-lookup"><span data-stu-id="24194-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="24194-168">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="24194-168">Add the IIS role.</span></span> <span data-ttu-id="24194-169">Uwzględnij funkcje "projektowanie aplikacji", w tym protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24194-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="24194-170">Dołącz także usługę zarządzania (wymienioną w obszarze "narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="24194-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="24194-171">**Zainstaluj Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="24194-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="24194-172">Po uruchomieniu Menedżera usług IIS zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub [pobranie instalatora](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="24194-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="24194-173">W Instalatorze platformy Wyszukaj Web Deploy i zainstaluj Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="24194-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="24194-174">Sprawdź, czy usługa zarządzania siecią Web jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="24194-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="24194-175">Jeśli nie, uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="24194-175">If not, start the service.</span></span> <span data-ttu-id="24194-176">(Jeśli nie widzisz usługi zarządzania siecią Web na liście usług systemu Windows, upewnij się, że usługa zarządzania została zainstalowana po dodaniu roli usług IIS.)</span><span class="sxs-lookup"><span data-stu-id="24194-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="24194-177">Domyślnie usługa zarządzania siecią Web nasłuchuje na porcie TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="24194-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="24194-178">W zaporze systemu Windows utwórz nową regułę ruchu przychodzącego zezwalającą na ruch TCP na porcie 8172.</span><span class="sxs-lookup"><span data-stu-id="24194-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="24194-179">Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="24194-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="24194-180">(Jeśli przechowujesz maszyny wirtualne na platformie Azure, możesz to zrobić bezpośrednio w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="24194-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="24194-181">Zobacz [, jak skonfigurować punkty końcowe na maszynie wirtualnej](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).</span><span class="sxs-lookup"><span data-stu-id="24194-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="24194-182">Teraz możesz przystąpić do wdrażania projektu programu Visual Studio z komputera deweloperskiego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="24194-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="24194-183">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="24194-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="24194-184">Aby uzyskać bardziej szczegółową dokumentację dotyczącą wdrażania w sieci Web, zobacz [Web Deployment Content Map for Visual Studio i ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="24194-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="24194-185">Jeśli aplikacja zostanie wdrożona na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobaczyć, że każdy z nich odbiera komunikaty sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="24194-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="24194-186">(Oczywiście w środowisku produkcyjnym dwa serwery byłyby za modułem równoważenia obciążenia).</span><span class="sxs-lookup"><span data-stu-id="24194-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="24194-187">Jeśli chcesz wiedzieć chcesz zobaczyć komunikaty wysyłane do Redis, możesz użyć klienta **Redis-CLI** , który jest instalowany z Redis.</span><span class="sxs-lookup"><span data-stu-id="24194-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
