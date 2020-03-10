---
uid: mvc/overview/deployment/docker
title: Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows
description: Dowiedz się, jak korzystać z istniejącej aplikacji ASP.NET MVC i uruchamiać ją w kontenerze platformy Docker systemu Windows
keywords: Kontenery systemu Windows, Docker, ASP. NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 12/14/2018
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: ef184f4256c20e2a66de8fd2d4f8e67f07d9a086
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583630"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="71a2b-104">Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows</span><span class="sxs-lookup"><span data-stu-id="71a2b-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="71a2b-105">Uruchamianie istniejącej aplikacji opartej na .NET Framework w kontenerze systemu Windows nie wymaga wprowadzania żadnych zmian w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71a2b-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="71a2b-106">Aby uruchomić aplikację w kontenerze systemu Windows, należy utworzyć obraz platformy Docker zawierający aplikację i uruchomić kontener.</span><span class="sxs-lookup"><span data-stu-id="71a2b-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="71a2b-107">W tym temacie wyjaśniono, jak zastosować istniejącą [aplikację ASP.NET MVC](http://www.asp.net/mvc) i wdrożyć ją w kontenerze systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="71a2b-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="71a2b-108">Zacznij od istniejącej aplikacji ASP.NET MVC, a następnie Kompiluj opublikowane elementy zawartości przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71a2b-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="71a2b-109">Użyj platformy Docker, aby utworzyć obraz, który zawiera i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="71a2b-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="71a2b-110">Przejdź do lokacji działającej w kontenerze systemu Windows i sprawdź, czy aplikacja działa.</span><span class="sxs-lookup"><span data-stu-id="71a2b-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="71a2b-111">W tym artykule przyjęto założenie, że masz podstawową wiedzą dotyczącą platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="71a2b-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="71a2b-112">Aby uzyskać informacje dotyczące platformy Docker, przeczytaj artykuł [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Przegląd platformy Docker).</span><span class="sxs-lookup"><span data-stu-id="71a2b-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="71a2b-113">Aplikacja, która zostanie uruchomiona w kontenerze, jest prostą witryną sieci Web, która umożliwia losowe odpowiedzi na pytania.</span><span class="sxs-lookup"><span data-stu-id="71a2b-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="71a2b-114">Ta aplikacja jest podstawową aplikacją MVC bez uwierzytelniania ani magazynu bazy danych. umożliwia skoncentrowanie się na przenoszeniu warstwy sieci Web do kontenera.</span><span class="sxs-lookup"><span data-stu-id="71a2b-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="71a2b-115">W przyszłości tematy pokazują, jak przenieść magazyn trwały i zarządzać nim w aplikacjach kontenerowych.</span><span class="sxs-lookup"><span data-stu-id="71a2b-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="71a2b-116">Przeniesienie aplikacji obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="71a2b-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="71a2b-117">Tworzenie zadania publikowania w celu skompilowania zasobów dla obrazu.</span><span class="sxs-lookup"><span data-stu-id="71a2b-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="71a2b-118">Tworzenie obrazu platformy Docker, na którym będzie uruchamiana aplikacja.</span><span class="sxs-lookup"><span data-stu-id="71a2b-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="71a2b-119">Uruchamianie kontenera platformy Docker z uruchomionym obrazem.</span><span class="sxs-lookup"><span data-stu-id="71a2b-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="71a2b-120">Weryfikowanie aplikacji przy użyciu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="71a2b-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="71a2b-121">[Gotowa aplikacja](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) znajduje się w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="71a2b-121">The [finished application](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71a2b-122">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="71a2b-122">Prerequisites</span></span>

<span data-ttu-id="71a2b-123">Komputer deweloperski musi mieć następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="71a2b-123">The development machine must have the following software:</span></span>

- <span data-ttu-id="71a2b-124">[Rocznicowa Aktualizacja systemu Windows 10](https://www.microsoft.com/software-download/windows10/) (lub nowsza) lub [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (lub nowsza wersja)</span><span class="sxs-lookup"><span data-stu-id="71a2b-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher)</span></span>
- <span data-ttu-id="71a2b-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) — wersja stabilna 1.13.0 lub 1,12 beta 26 (lub nowsze wersje)</span><span class="sxs-lookup"><span data-stu-id="71a2b-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- [<span data-ttu-id="71a2b-126">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="71a2b-126">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> <span data-ttu-id="71a2b-127">Jeśli używasz systemu Windows Server 2016, postępuj zgodnie z instrukcjami dotyczącymi [wdrażania hosta kontenerów — Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="71a2b-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="71a2b-128">Po zainstalowaniu i uruchomieniu programu Docker kliknij prawym przyciskiem myszy jego ikonę na pasku zadań i wybierz pozycję **Switch to Windows containers** (Przełącz na kontenery systemu Windows).</span><span class="sxs-lookup"><span data-stu-id="71a2b-128">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="71a2b-129">Jest to wymagane do uruchomienia obrazów Docker opartych na systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="71a2b-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="71a2b-130">Wykonanie tego polecenia może potrwać kilka sekund:</span><span class="sxs-lookup"><span data-stu-id="71a2b-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="71a2b-131">![Kontener systemu Windows][windows-container]</span><span class="sxs-lookup"><span data-stu-id="71a2b-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="71a2b-132">Publikuj skrypt</span><span class="sxs-lookup"><span data-stu-id="71a2b-132">Publish script</span></span>

<span data-ttu-id="71a2b-133">Zbierz w jednym miejscu wszystkie elementy zawartości konieczne do załadowania do obrazu Docker.</span><span class="sxs-lookup"><span data-stu-id="71a2b-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="71a2b-134">Możesz użyć polecenia **Opublikuj** Visual Studio, aby utworzyć profil publikacji dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71a2b-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="71a2b-135">Ten profil umieści wszystkie zasoby w jednym drzewie katalogów, które zostaną skopiowane do obrazu docelowego w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="71a2b-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="71a2b-136">**Kroki publikowania**</span><span class="sxs-lookup"><span data-stu-id="71a2b-136">**Publish Steps**</span></span>

1. <span data-ttu-id="71a2b-137">Kliknij prawym przyciskiem myszy projekt sieci Web w programie Visual Studio, a następnie wybierz pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="71a2b-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="71a2b-138">Kliknij **przycisk profil niestandardowy**, a następnie wybierz opcję **system plików** jako metodę.</span><span class="sxs-lookup"><span data-stu-id="71a2b-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="71a2b-139">Wybierz katalog.</span><span class="sxs-lookup"><span data-stu-id="71a2b-139">Choose the directory.</span></span> <span data-ttu-id="71a2b-140">Zgodnie z Konwencją pobrany przykład używa `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="71a2b-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="71a2b-141">![Publikuj połączenie][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="71a2b-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="71a2b-142">Otwórz sekcję **Opcje publikowania plików** na karcie **Ustawienia** . Wybierz opcję **prekompilowanie podczas publikowania**.</span><span class="sxs-lookup"><span data-stu-id="71a2b-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="71a2b-143">Ta optymalizacja oznacza, że zostaną skompilowane widoki w kontenerze platformy Docker, kopiując wstępnie skompilowane widoki.</span><span class="sxs-lookup"><span data-stu-id="71a2b-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="71a2b-144">![Ustawienia publikowania][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="71a2b-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="71a2b-145">Kliknij przycisk **Publikuj**, a program Visual Studio skopiuje wszystkie zasoby, których potrzebujesz do folderu docelowego.</span><span class="sxs-lookup"><span data-stu-id="71a2b-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="71a2b-146">Tworzenie obrazu</span><span class="sxs-lookup"><span data-stu-id="71a2b-146">Build the image</span></span>

<span data-ttu-id="71a2b-147">Utwórz nowy plik o nazwie *pliku dockerfile* , aby zdefiniować obraz platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="71a2b-147">Create a new file named *Dockerfile* to define your Docker image.</span></span> <span data-ttu-id="71a2b-148">*Pliku dockerfile* zawiera instrukcje dotyczące tworzenia końcowego obrazu i zawiera wszystkie podstawowe nazwy obrazów, wymagane składniki, aplikację, którą chcesz uruchomić, oraz inne obrazy konfiguracyjne.</span><span class="sxs-lookup"><span data-stu-id="71a2b-148">*Dockerfile* contains instructions to build the final image and includes any base image names, required components, the app you want to run, and other configuration images.</span></span> <span data-ttu-id="71a2b-149">*Pliku dockerfile* to dane wejściowe polecenia `docker build`, które tworzy obraz.</span><span class="sxs-lookup"><span data-stu-id="71a2b-149">*Dockerfile* is the input to the `docker build` command that creates the image.</span></span>

<span data-ttu-id="71a2b-150">W tym ćwiczeniu utworzysz obraz na podstawie obrazu `microsoft/aspnet` znajdującego się w usłudze [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="71a2b-150">For this exercise, you will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="71a2b-151">Obraz podstawowy, `microsoft/aspnet`, jest obrazem systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="71a2b-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="71a2b-152">Zawiera on systemy Windows Server Core, IIS i ASP.NET 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="71a2b-152">It contains Windows Server Core, IIS, and ASP.NET 4.7.2.</span></span> <span data-ttu-id="71a2b-153">Po uruchomieniu tego obrazu w kontenerze zostaną automatycznie uruchomione usługi IIS i zainstalowane witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="71a2b-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="71a2b-154">Pliku dockerfile tworzący obraz wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="71a2b-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="71a2b-155">W tym pliku Dockerfile nie ma polecenia `ENTRYPOINT`.</span><span class="sxs-lookup"><span data-stu-id="71a2b-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="71a2b-156">Nie jest ono potrzebne.</span><span class="sxs-lookup"><span data-stu-id="71a2b-156">You don't need one.</span></span> <span data-ttu-id="71a2b-157">W przypadku korzystania z systemu Windows Server z usługami IIS proces usług IIS jest punktem wejścia, który jest konfigurowany do uruchamiania na podstawowym obrazie ASPNET.</span><span class="sxs-lookup"><span data-stu-id="71a2b-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="71a2b-158">Uruchom polecenie Docker Build, aby utworzyć obraz z uruchomioną aplikacją ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71a2b-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="71a2b-159">W tym celu Otwórz okno programu PowerShell w katalogu projektu i wpisz następujące polecenie w katalogu rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="71a2b-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="71a2b-160">To polecenie spowoduje skompilowanie nowego obrazu przy użyciu instrukcji w pliku dockerfile, nadawanie nazwy (-t znakowanie) obrazu jako mvcrandomanswers.</span><span class="sxs-lookup"><span data-stu-id="71a2b-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="71a2b-161">Może to obejmować ściąganie obrazu podstawowego z usługi [Docker Hub](http://hub.docker.com), a następnie dodanie aplikacji do tego obrazu.</span><span class="sxs-lookup"><span data-stu-id="71a2b-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="71a2b-162">Po zakończeniu tego polecenia można uruchomić `docker images` polecenie, aby wyświetlić informacje o nowym obrazie:</span><span class="sxs-lookup"><span data-stu-id="71a2b-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="71a2b-163">Identyfikator obrazu będzie różny na twoim komputerze.</span><span class="sxs-lookup"><span data-stu-id="71a2b-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="71a2b-164">Teraz Uruchommy aplikację.</span><span class="sxs-lookup"><span data-stu-id="71a2b-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="71a2b-165">Uruchamianie kontenera</span><span class="sxs-lookup"><span data-stu-id="71a2b-165">Start a container</span></span>

<span data-ttu-id="71a2b-166">Uruchom kontener, wykonując następujące `docker run` polecenie:</span><span class="sxs-lookup"><span data-stu-id="71a2b-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="71a2b-167">Argument `-d` informuje platformę Docker o konieczności uruchomienia obrazu w trybie odłączonym.</span><span class="sxs-lookup"><span data-stu-id="71a2b-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="71a2b-168">Oznacza to, że obraz platformy Docker zostanie rozłączony z bieżącą powłoką.</span><span class="sxs-lookup"><span data-stu-id="71a2b-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="71a2b-169">W wielu przykładach platformy Docker można zobaczyć-p, aby zmapować kontener i porty hosta.</span><span class="sxs-lookup"><span data-stu-id="71a2b-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="71a2b-170">Domyślny obraz ASPNET już skonfigurował kontener do nasłuchiwania na porcie 80 i uwidaczniania go.</span><span class="sxs-lookup"><span data-stu-id="71a2b-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span>

<span data-ttu-id="71a2b-171">`--name randomanswers` nadaje nazwę uruchomionemu kontenerowi.</span><span class="sxs-lookup"><span data-stu-id="71a2b-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="71a2b-172">W większości poleceń można użyć tej nazwy zamiast identyfikatora kontenera.</span><span class="sxs-lookup"><span data-stu-id="71a2b-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="71a2b-173">`mvcrandomanswers` to nazwa obrazu do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="71a2b-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="71a2b-174">Weryfikowanie w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="71a2b-174">Verify in the browser</span></span>

<span data-ttu-id="71a2b-175">Po rozpoczęciu kontenera Połącz się z uruchomionym kontenerem przy użyciu `http://localhost` w pokazanym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="71a2b-175">Once the container starts, connect to the running container using `http://localhost` in the example shown.</span></span> <span data-ttu-id="71a2b-176">Wpisz ten adres URL w przeglądarce i zobacz działającą lokację.</span><span class="sxs-lookup"><span data-stu-id="71a2b-176">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="71a2b-177">Niektóre oprogramowanie sieci VPN lub serwera proxy może uniemożliwić przechodzenie do witryny.</span><span class="sxs-lookup"><span data-stu-id="71a2b-177">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="71a2b-178">Możesz tymczasowo ją wyłączyć, aby upewnić się, że kontener działa.</span><span class="sxs-lookup"><span data-stu-id="71a2b-178">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="71a2b-179">Przykładowy katalog w witrynie GitHub zawiera [skrypt programu PowerShell](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) , który wykonuje te polecenia.</span><span class="sxs-lookup"><span data-stu-id="71a2b-179">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="71a2b-180">Otwórz okno programu PowerShell, Zmień katalog na katalog rozwiązania, a następnie wpisz:</span><span class="sxs-lookup"><span data-stu-id="71a2b-180">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="71a2b-181">Powyższe polecenie kompiluje obraz, wyświetla listę obrazów na komputerze i uruchamia kontener.</span><span class="sxs-lookup"><span data-stu-id="71a2b-181">The command above builds the image, displays the list of images on your machine, and starts a container.</span></span>

<span data-ttu-id="71a2b-182">Aby zatrzymać kontener, wydaj polecenie `docker stop`:</span><span class="sxs-lookup"><span data-stu-id="71a2b-182">To stop your container, issue a `docker stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="71a2b-183">Aby usunąć kontener, wydaj polecenie `docker rm`:</span><span class="sxs-lookup"><span data-stu-id="71a2b-183">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Przełącz do kontenera systemu Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publikuj w systemie plików"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Ustawienia publikowania"
