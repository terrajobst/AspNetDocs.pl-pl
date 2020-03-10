---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrożenie wiersza polecenia | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630922"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="07b93-103">ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrożenie wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="07b93-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>

<span data-ttu-id="07b93-104">Autor [Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="07b93-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="07b93-105">Pobierz projekt początkowy</span><span class="sxs-lookup"><span data-stu-id="07b93-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="07b93-106">W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="07b93-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="07b93-107">Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="07b93-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="07b93-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="07b93-108">Overview</span></span>

<span data-ttu-id="07b93-109">W tym samouczku pokazano, jak wywoływać potok publikacji w sieci Web w programie Visual Studio z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="07b93-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="07b93-110">Jest to przydatne w scenariuszach, w których [proces wdrażania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) ma być zautomatyzowany, zamiast ręcznie w programie Visual Studio, zazwyczaj przy użyciu [systemu kontroli wersji kodu źródłowego](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="07b93-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="07b93-111">Wprowadź zmianę do wdrożenia</span><span class="sxs-lookup"><span data-stu-id="07b93-111">Make a change to deploy</span></span>

<span data-ttu-id="07b93-112">Obecnie Strona informacje zawiera kod szablonu.</span><span class="sxs-lookup"><span data-stu-id="07b93-112">Currently the About page displays the template code.</span></span>

![Informacje o stronie z kodem szablonu](command-line-deployment/_static/image1.png)

<span data-ttu-id="07b93-114">Spowoduje to zamianę na kod, który wyświetla podsumowanie rejestracji ucznia.</span><span class="sxs-lookup"><span data-stu-id="07b93-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="07b93-115">Otwórz stronę *about. aspx* , Usuń wszystkie znaczniki wewnątrz elementu `MainContent` `Content` i Wstaw następujące znaczniki w miejscu:</span><span class="sxs-lookup"><span data-stu-id="07b93-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="07b93-116">Uruchom projekt i wybierz stronę **informacje** .</span><span class="sxs-lookup"><span data-stu-id="07b93-116">Run the project and select the **About** page.</span></span>

![Informacje o stronie](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="07b93-118">Wdróż do testowania przy użyciu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="07b93-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="07b93-119">Nie zostanie wdrożona inna zmiana bazy danych, dlatego należy wyłączyć wdrażanie bazy danych dbDacFx dla bazy danych ASPNET-ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="07b93-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="07b93-120">Otwórz kreatora **publikacji w sieci Web** i w każdym z trzech profilów publikowania wyczyść pole wyboru **Aktualizuj bazę danych** na karcie **Ustawienia** .</span><span class="sxs-lookup"><span data-stu-id="07b93-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="07b93-121">Na stronie startowej systemu Windows 8 Wyszukaj **wiersz polecenia dla deweloperów VS2012**.</span><span class="sxs-lookup"><span data-stu-id="07b93-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="07b93-122">Kliknij prawym przyciskiem myszy ikonę **wiersz polecenia dla deweloperów dla VS2012** , a następnie kliknij pozycję **Uruchom jako administrator**.</span><span class="sxs-lookup"><span data-stu-id="07b93-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="07b93-123">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżką do pliku rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="07b93-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="07b93-124">Program MSBuild kompiluje rozwiązanie i wdraża je w środowisku testowym.</span><span class="sxs-lookup"><span data-stu-id="07b93-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Dane wyjściowe wiersza polecenia](command-line-deployment/_static/image3.png)

<span data-ttu-id="07b93-126">Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, a następnie kliknij stronę **informacje** , aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="07b93-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="07b93-127">Jeśli nie utworzono uczniów w teście, zobaczysz pustą stronę w nagłówku **statystyki treści ucznia** .</span><span class="sxs-lookup"><span data-stu-id="07b93-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="07b93-128">Przejdź do strony **uczniów** , kliknij pozycję **Dodaj uczniów**i Dodaj wybranych uczniów, a następnie wróć do strony **informacje** , aby wyświetlić statystyki uczniów.</span><span class="sxs-lookup"><span data-stu-id="07b93-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Informacje o stronie w środowisku testowym](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="07b93-130">Opcje wiersza polecenia klucza</span><span class="sxs-lookup"><span data-stu-id="07b93-130">Key command line options</span></span>

<span data-ttu-id="07b93-131">Wprowadzone polecenie przekazało ścieżkę pliku rozwiązania i dwie właściwości do programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="07b93-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="07b93-132">Wdrażanie rozwiązania i wdrażanie pojedynczych projektów</span><span class="sxs-lookup"><span data-stu-id="07b93-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="07b93-133">Określenie pliku rozwiązania powoduje skompilowanie wszystkich projektów w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="07b93-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="07b93-134">Jeśli masz wiele projektów sieci Web w rozwiązaniu, stosuje się następujące zachowanie MSBuild:</span><span class="sxs-lookup"><span data-stu-id="07b93-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="07b93-135">Właściwości określone w wierszu polecenia są przesyłane do każdego projektu.</span><span class="sxs-lookup"><span data-stu-id="07b93-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="07b93-136">W związku z tym każdy projekt sieci Web musi mieć profil publikacji o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="07b93-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="07b93-137">Jeśli określisz `/p:PublishProfile=Test`, każdy projekt sieci Web musi mieć profil publikacji o nazwie *test*.</span><span class="sxs-lookup"><span data-stu-id="07b93-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="07b93-138">Możesz pomyślnie opublikować jeden projekt, gdy inny nie będzie jeszcze kompilować.</span><span class="sxs-lookup"><span data-stu-id="07b93-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="07b93-139">Aby uzyskać więcej informacji, zobacz StackOverflow wątku programu [MSBuild kończy się niepowodzeniem z dwoma pakietami](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="07b93-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="07b93-140">Jeśli określisz indywidualny projekt zamiast rozwiązania, musisz dodać parametr, który określa wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="07b93-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="07b93-141">Jeśli używasz programu Visual Studio 2012, wiersz polecenia będzie wyglądać podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="07b93-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="07b93-142">Numer wersji dla programu Visual Studio 2010 to 10,0.</span><span class="sxs-lookup"><span data-stu-id="07b93-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="07b93-143">Aby uzyskać więcej informacji, zobacz blog programu [Visual Studio — zgodność i VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="07b93-143">For more information, see [Visual Studio project compatibility and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="07b93-144">Określanie profilu publikowania</span><span class="sxs-lookup"><span data-stu-id="07b93-144">Specifying the publish profile</span></span>

<span data-ttu-id="07b93-145">Możesz określić profil publikacji według nazwy lub pełną ścieżkę do pliku *. pubxml* , jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="07b93-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="07b93-146">Metody publikowania w sieci Web obsługiwane w przypadku publikowania w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="07b93-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="07b93-147">Trzy metody publikowania są obsługiwane w przypadku publikowania w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="07b93-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="07b93-148">`MSDeploy` — Publikowanie przy użyciu Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="07b93-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="07b93-149">`Package` — Publikuj przez utworzenie pakietu Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="07b93-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="07b93-150">Należy zainstalować pakiet niezależnie od polecenia MSBuild, które go tworzy.</span><span class="sxs-lookup"><span data-stu-id="07b93-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="07b93-151">`FileSystem` — Publikuj przez kopiowanie plików do określonego folderu.</span><span class="sxs-lookup"><span data-stu-id="07b93-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="07b93-152">Określanie konfiguracji i platformy kompilacji</span><span class="sxs-lookup"><span data-stu-id="07b93-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="07b93-153">Konfiguracja kompilacji i platforma musi być ustawiona w programie Visual Studio lub w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="07b93-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="07b93-154">Profile publikowania obejmują właściwości o nazwie `LastUsedBuildConfiguration` i `LastUsedPlatform`, ale nie można ustawić tych właściwości w celu określenia sposobu kompilowania projektu.</span><span class="sxs-lookup"><span data-stu-id="07b93-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="07b93-155">Aby uzyskać więcej informacji, zobacz [MSBuild: jak ustawić właściwość konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="07b93-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="07b93-156">Wdrażanie w środowisku przejściowym</span><span class="sxs-lookup"><span data-stu-id="07b93-156">Deploy to staging</span></span>

<span data-ttu-id="07b93-157">Aby wdrożyć na platformie Azure, musisz dodać hasło do wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="07b93-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="07b93-158">Jeśli hasło zostało zapisane w profilu publikowania w programie Visual Studio, zostało zapisane w postaci zaszyfrowanej w pliku *. pubxml. User* .</span><span class="sxs-lookup"><span data-stu-id="07b93-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="07b93-159">Ten plik nie jest używany przez program MSBuild podczas wdrażania wiersza polecenia, więc musisz przekazać hasło w parametrze wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="07b93-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="07b93-160">Skopiuj wymagane hasło z pliku *publishsettings* , który został wcześniej pobrany dla tymczasowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="07b93-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="07b93-161">Hasło jest wartością atrybutu `userPWD` dla elementu Web Deploy `publishProfile`.</span><span class="sxs-lookup"><span data-stu-id="07b93-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web Deploy hasło](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="07b93-163">Na stronie startowej systemu Windows 8 Wyszukaj pozycję **wiersz polecenia dla deweloperów for VS2012**, a następnie kliknij ikonę, aby otworzyć wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="07b93-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="07b93-164">(Nie musisz otwierać go jako administrator tego czasu, ponieważ nie są wdrażane w usługach IIS na komputerze lokalnym).</span><span class="sxs-lookup"><span data-stu-id="07b93-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="07b93-165">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżką do pliku rozwiązania i hasłem hasła:</span><span class="sxs-lookup"><span data-stu-id="07b93-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="07b93-166">Należy zauważyć, że ten wiersz polecenia zawiera dodatkowy parametr: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="07b93-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="07b93-167">W trakcie pisania tego samouczka Właściwość `AllowUntrustedCertificate` musi być ustawiona podczas publikowania na platformie Azure z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="07b93-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="07b93-168">Gdy poprawka dla tej usterki zostanie wydana, ten parametr nie jest potrzebny.</span><span class="sxs-lookup"><span data-stu-id="07b93-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="07b93-169">Otwórz przeglądarkę i przejdź do adresu URL witryny tymczasowej, a następnie kliknij stronę **informacje** , aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="07b93-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="07b93-170">Jak widać wcześniej w środowisku testowym, może być konieczne utworzenie niektórych uczniów, aby wyświetlić statystyki na stronie **informacje** .</span><span class="sxs-lookup"><span data-stu-id="07b93-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="07b93-171">Wdrażanie w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="07b93-171">Deploy to production</span></span>

<span data-ttu-id="07b93-172">Proces wdrażania w środowisku produkcyjnym jest podobny do procesu przemieszczania.</span><span class="sxs-lookup"><span data-stu-id="07b93-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="07b93-173">Skopiuj wymagane hasło z pliku *publishsettings* , który został pobrany wcześniej dla produkcyjnej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="07b93-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="07b93-174">Otwórz **wiersz polecenia dla deweloperów dla VS2012**.</span><span class="sxs-lookup"><span data-stu-id="07b93-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="07b93-175">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżką do pliku rozwiązania i hasłem hasła:</span><span class="sxs-lookup"><span data-stu-id="07b93-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="07b93-176">W przypadku rzeczywistej lokacji produkcyjnej, jeśli wprowadzono również zmiany w bazie danych, należy zwykle skopiować *aplikację\_pliku offline. htm* do lokacji przed wdrożeniem i usunąć ją po pomyślnym wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="07b93-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="07b93-177">Otwórz przeglądarkę i przejdź do adresu URL witryny tymczasowej, a następnie kliknij stronę **informacje** , aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="07b93-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="07b93-178">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="07b93-178">Summary</span></span>

<span data-ttu-id="07b93-179">Aktualizacja aplikacji została wdrożona przy użyciu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="07b93-179">You have now deployed an application update by using the command line.</span></span>

![Informacje o stronie w środowisku testowym](command-line-deployment/_static/image6.png)

<span data-ttu-id="07b93-181">W następnym samouczku zobaczysz przykład sposobu rozszerania potoku publikowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="07b93-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="07b93-182">W przykładzie pokazano, jak wdrożyć pliki, które nie są uwzględnione w projekcie.</span><span class="sxs-lookup"><span data-stu-id="07b93-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="07b93-183">[Poprzednie](deploying-a-database-update.md)
> [dalej](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="07b93-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
