---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Skalowania sygnalizujący z Redis (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5079cf3fa773faeac911eddc75dc66e94ad98bb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536562"
---
# <a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="05b4e-102">SignalR — skalowanie w poziomie z użyciem pamięci podręcznej Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="05b4e-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>

<span data-ttu-id="05b4e-103">według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="05b4e-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="05b4e-104">W tym samouczku będziesz używać [Redis](http://redis.io/) do dystrybuowania komunikatów w aplikacji sygnalizującej wdrożonej na dwóch oddzielnych WYSTĄPIENIACH usług IIS.</span><span class="sxs-lookup"><span data-stu-id="05b4e-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="05b4e-105">Redis jest magazynem wartości w pamięci.</span><span class="sxs-lookup"><span data-stu-id="05b4e-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="05b4e-106">Obsługuje również system obsługi komunikatów z modelem publikowania/subskrybowania.</span><span class="sxs-lookup"><span data-stu-id="05b4e-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="05b4e-107">Funkcja Rediser programu sygnalizującego używa funkcji pub/sub do przesyłania dalej komunikatów do innych serwerów.</span><span class="sxs-lookup"><span data-stu-id="05b4e-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="05b4e-108">W tym samouczku zostaną użyte trzy serwery:</span><span class="sxs-lookup"><span data-stu-id="05b4e-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="05b4e-109">Dwa serwery z systemem Windows, które będą używane do wdrażania aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="05b4e-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="05b4e-110">Jeden serwer z systemem Linux, który będzie używany do uruchamiania Redis.</span><span class="sxs-lookup"><span data-stu-id="05b4e-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="05b4e-111">W przypadku zrzutów ekranu w tym samouczku użyto Ubuntu 12,04 TLS.</span><span class="sxs-lookup"><span data-stu-id="05b4e-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="05b4e-112">Jeśli nie masz trzech serwerów fizycznych do użycia, możesz utworzyć maszyny wirtualne w funkcji Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="05b4e-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="05b4e-113">Innym rozwiązaniem jest utworzenie maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="05b4e-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="05b4e-114">Chociaż w tym samouczku jest stosowana Oficjalna implementacja Redis, istnieje również [port systemu Windows Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="05b4e-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="05b4e-115">Konfiguracja i konfiguracja są różne, ale w przeciwnym razie kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="05b4e-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="05b4e-116">Usługa sygnalizująca skalowania z Redis nie obsługuje klastrów Redis.</span><span class="sxs-lookup"><span data-stu-id="05b4e-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="05b4e-117">Omówienie</span><span class="sxs-lookup"><span data-stu-id="05b4e-117">Overview</span></span>

<span data-ttu-id="05b4e-118">Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.</span><span class="sxs-lookup"><span data-stu-id="05b4e-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="05b4e-119">Zainstaluj program Redis i uruchom serwer Redis.</span><span class="sxs-lookup"><span data-stu-id="05b4e-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="05b4e-120">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="05b4e-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="05b4e-121">Microsoft. AspNet. Signal</span><span class="sxs-lookup"><span data-stu-id="05b4e-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="05b4e-122">Microsoft. AspNet. Signal. Redis</span><span class="sxs-lookup"><span data-stu-id="05b4e-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="05b4e-123">Tworzenie aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="05b4e-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="05b4e-124">Dodaj następujący kod do szablonu Global. asax, aby skonfigurować plan:</span><span class="sxs-lookup"><span data-stu-id="05b4e-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="05b4e-125">Ubuntu na funkcji Hyper-V</span><span class="sxs-lookup"><span data-stu-id="05b4e-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="05b4e-126">Korzystając z funkcji Windows Hyper-V, można łatwo utworzyć maszynę wirtualną Ubuntu w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="05b4e-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="05b4e-127">Pobierz plik ISO Ubuntu z [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="05b4e-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="05b4e-128">W funkcji Hyper-V Dodaj nową maszynę wirtualną.</span><span class="sxs-lookup"><span data-stu-id="05b4e-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="05b4e-129">W kroku **Podłączanie wirtualnego dysku twardego** wybierz pozycję **Utwórz wirtualny dysk twardy**.</span><span class="sxs-lookup"><span data-stu-id="05b4e-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="05b4e-130">W kroku **Opcje instalacji** wybierz pozycję **plik obrazu (. ISO)** , kliknij przycisk **Przeglądaj**, a następnie przejdź do Ubuntu instalacji ISO.</span><span class="sxs-lookup"><span data-stu-id="05b4e-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="05b4e-131">Zainstaluj Redis</span><span class="sxs-lookup"><span data-stu-id="05b4e-131">Install Redis</span></span>

<span data-ttu-id="05b4e-132">Postępuj zgodnie z instrukcjami w [http://redis.io/download](http://redis.io/download) , aby pobrać i skompilować Redis.</span><span class="sxs-lookup"><span data-stu-id="05b4e-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="05b4e-133">Spowoduje to skompilowanie plików binarnych Redis w katalogu `src`.</span><span class="sxs-lookup"><span data-stu-id="05b4e-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="05b4e-134">Domyślnie Redis nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="05b4e-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="05b4e-135">Aby ustawić hasło, edytuj plik `redis.conf`, który znajduje się w katalogu głównym kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="05b4e-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="05b4e-136">(Utwórz kopię zapasową pliku przed edycją!) Dodaj następującą dyrektywę do `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="05b4e-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="05b4e-137">Teraz uruchom serwer Redis:</span><span class="sxs-lookup"><span data-stu-id="05b4e-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="05b4e-138">Otwórz port 6379, który jest domyślnym portem, który Redis nasłuchuje.</span><span class="sxs-lookup"><span data-stu-id="05b4e-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="05b4e-139">(Numer portu można zmienić w pliku konfiguracyjnym).</span><span class="sxs-lookup"><span data-stu-id="05b4e-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="05b4e-140">Tworzenie aplikacji sygnalizującej</span><span class="sxs-lookup"><span data-stu-id="05b4e-140">Create the SignalR Application</span></span>

<span data-ttu-id="05b4e-141">Utwórz aplikację sygnalizującą, wykonując jeden z następujących samouczków:</span><span class="sxs-lookup"><span data-stu-id="05b4e-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="05b4e-142">Wprowadzenie z sygnałem</span><span class="sxs-lookup"><span data-stu-id="05b4e-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="05b4e-143">Wprowadzenie z sygnalizacyjnym i MVC 4</span><span class="sxs-lookup"><span data-stu-id="05b4e-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="05b4e-144">Następnie zmodyfikujemy aplikację czatu, aby obsługiwała skalowania z Redis.</span><span class="sxs-lookup"><span data-stu-id="05b4e-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="05b4e-145">Najpierw Dodaj pakiet NuGet Rediser do projektu.</span><span class="sxs-lookup"><span data-stu-id="05b4e-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="05b4e-146">W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="05b4e-146">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="05b4e-147">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="05b4e-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="05b4e-148">Następnie otwórz plik Global. asax.</span><span class="sxs-lookup"><span data-stu-id="05b4e-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="05b4e-149">Dodaj następujący kod do **aplikacji\_Start** :</span><span class="sxs-lookup"><span data-stu-id="05b4e-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="05b4e-150">"serwer" jest nazwą serwera, na którym działa Redis.</span><span class="sxs-lookup"><span data-stu-id="05b4e-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="05b4e-151">*port* jest numerem portu</span><span class="sxs-lookup"><span data-stu-id="05b4e-151">*port* is the port number</span></span>
- <span data-ttu-id="05b4e-152">"hasło" to hasło zdefiniowane w pliku Redis. conf.</span><span class="sxs-lookup"><span data-stu-id="05b4e-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="05b4e-153">"Nazwa_aplikacji" to dowolny ciąg.</span><span class="sxs-lookup"><span data-stu-id="05b4e-153">"AppName" is any string.</span></span> <span data-ttu-id="05b4e-154">Program sygnalizujący tworzy kanał Redis pub/Sub o tej nazwie.</span><span class="sxs-lookup"><span data-stu-id="05b4e-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="05b4e-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="05b4e-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="05b4e-156">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="05b4e-156">Deploy and Run the Application</span></span>

<span data-ttu-id="05b4e-157">Przygotuj wystąpienia systemu Windows Server, aby wdrożyć aplikację sygnalizującą.</span><span class="sxs-lookup"><span data-stu-id="05b4e-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="05b4e-158">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="05b4e-158">Add the IIS role.</span></span> <span data-ttu-id="05b4e-159">Uwzględnij funkcje "projektowanie aplikacji", w tym protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="05b4e-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="05b4e-160">Dołącz także usługę zarządzania (wymienioną w obszarze "narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="05b4e-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="05b4e-161">**Zainstaluj Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="05b4e-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="05b4e-162">Po uruchomieniu Menedżera usług IIS zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub [pobranie instalatora](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="05b4e-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="05b4e-163">W Instalatorze platformy Wyszukaj Web Deploy i zainstaluj Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="05b4e-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="05b4e-164">Sprawdź, czy usługa zarządzania siecią Web jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="05b4e-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="05b4e-165">Jeśli nie, uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="05b4e-165">If not, start the service.</span></span> <span data-ttu-id="05b4e-166">(Jeśli nie widzisz usługi zarządzania siecią Web na liście usług systemu Windows, upewnij się, że usługa zarządzania została zainstalowana po dodaniu roli usług IIS.)</span><span class="sxs-lookup"><span data-stu-id="05b4e-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="05b4e-167">Domyślnie usługa zarządzania siecią Web nasłuchuje na porcie TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="05b4e-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="05b4e-168">W zaporze systemu Windows utwórz nową regułę ruchu przychodzącego zezwalającą na ruch TCP na porcie 8172.</span><span class="sxs-lookup"><span data-stu-id="05b4e-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="05b4e-169">Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="05b4e-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="05b4e-170">(Jeśli przechowujesz maszyny wirtualne na platformie Azure, możesz to zrobić bezpośrednio w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="05b4e-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="05b4e-171">Zobacz [, jak skonfigurować punkty końcowe na maszynie wirtualnej](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).</span><span class="sxs-lookup"><span data-stu-id="05b4e-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="05b4e-172">Teraz możesz przystąpić do wdrażania projektu programu Visual Studio z komputera deweloperskiego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="05b4e-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="05b4e-173">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="05b4e-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="05b4e-174">Aby uzyskać bardziej szczegółową dokumentację dotyczącą wdrażania w sieci Web, zobacz [Web Deployment Content Map for Visual Studio i ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="05b4e-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="05b4e-175">Jeśli aplikacja zostanie wdrożona na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobaczyć, że każdy z nich odbiera komunikaty sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="05b4e-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="05b4e-176">(Oczywiście w środowisku produkcyjnym dwa serwery byłyby za modułem równoważenia obciążenia).</span><span class="sxs-lookup"><span data-stu-id="05b4e-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="05b4e-177">Jeśli chcesz wiedzieć chcesz zobaczyć komunikaty wysyłane do Redis, możesz użyć klienta **Redis-CLI** , który jest instalowany z Redis.</span><span class="sxs-lookup"><span data-stu-id="05b4e-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
