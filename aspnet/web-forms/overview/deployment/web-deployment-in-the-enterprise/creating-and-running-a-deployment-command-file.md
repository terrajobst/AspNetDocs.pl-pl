---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Tworzenie i uruchamianie pliku poleceń wdrażania | Microsoft Docs
author: jrjlee
description: W tym temacie opisano, jak utworzyć plik poleceń, który umożliwi uruchomienie wdrożenia przy użyciu plików projektu Microsoft Build Engine (MSBuild) jako jednego kroku, ponownie...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634310"
---
# <a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="2fa78-103">Tworzenie i uruchamianie pliku poleceń wdrażania</span><span class="sxs-lookup"><span data-stu-id="2fa78-103">Creating and Running a Deployment Command File</span></span>

<span data-ttu-id="2fa78-104">Autor [Jason Lewandowski](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="2fa78-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="2fa78-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="2fa78-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="2fa78-106">W tym temacie opisano sposób tworzenia pliku poleceń, który umożliwia uruchomienie wdrożenia przy użyciu plików projektu Microsoft Build Engine (MSBuild) jako jednego kroku powtarzalnego procesu.</span><span class="sxs-lookup"><span data-stu-id="2fa78-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>

<span data-ttu-id="2fa78-107">Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe [](the-contact-manager-solution.md) rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, które ma realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2fa78-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="2fa78-108">Metoda wdrażania w tym samouczku jest oparta na podejściu do pliku projektu podzielonego opisanego w artykule [Opis procesu kompilacji](understanding-the-build-process.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska.</span><span class="sxs-lookup"><span data-stu-id="2fa78-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="2fa78-109">W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2fa78-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="2fa78-110">Przegląd procesu</span><span class="sxs-lookup"><span data-stu-id="2fa78-110">Process Overview</span></span>

<span data-ttu-id="2fa78-111">W tym temacie dowiesz się, jak utworzyć i uruchomić plik poleceń, który używa tych plików projektu, aby przeprowadzić powtarzające wdrożenie w środowisku docelowym.</span><span class="sxs-lookup"><span data-stu-id="2fa78-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="2fa78-112">Zasadniczo plik poleceń musi zawierać polecenie MSBuild, które:</span><span class="sxs-lookup"><span data-stu-id="2fa78-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="2fa78-113">Informuje program MSBuild, aby wykonał plik niezależny od *Publish. proj* środowiska.</span><span class="sxs-lookup"><span data-stu-id="2fa78-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="2fa78-114">Informuje plik *Publish. proj* , który plik zawiera ustawienia projektu specyficzne dla środowiska i miejsce, w którym można go znaleźć.</span><span class="sxs-lookup"><span data-stu-id="2fa78-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="2fa78-115">Utwórz polecenie MSBuild</span><span class="sxs-lookup"><span data-stu-id="2fa78-115">Create an MSBuild Command</span></span>

<span data-ttu-id="2fa78-116">Zgodnie z opisem w temacie [proces kompilowania](understanding-the-build-process.md), plik&#x2014;projektu specyficzny dla środowiska, na przykład *ENV-dev. proj*&#x2014;, został zaprojektowany do zaimportowania do pliku Environment-niezależny od *Publish. proj* w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2fa78-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="2fa78-117">Razem te dwa pliki zawierają pełny zestaw instrukcji, które informują program MSBuild, jak skompilować i wdrożyć rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="2fa78-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="2fa78-118">Plik *Publish. proj* używa elementu **Import** w celu zaimportowania pliku projektu specyficznego dla środowiska.</span><span class="sxs-lookup"><span data-stu-id="2fa78-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

<span data-ttu-id="2fa78-119">W związku z tym, gdy używasz programu MSBuild. exe do kompilowania i wdrażania rozwiązania Contact Manager, musisz:</span><span class="sxs-lookup"><span data-stu-id="2fa78-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="2fa78-120">Uruchom program MSBuild. exe w pliku *Publish. proj* .</span><span class="sxs-lookup"><span data-stu-id="2fa78-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="2fa78-121">Określ lokalizację pliku projektu specyficznego dla środowiska, dostarczając parametr wiersza polecenia o nazwie **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="2fa78-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="2fa78-122">Aby to zrobić, polecenie MSBuild powinno wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="2fa78-122">To do this, your MSBuild command should resemble this:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

<span data-ttu-id="2fa78-123">W tym miejscu jest to prosty krok, aby przejść do powtarzalnego wdrożenia z jednym etapem.</span><span class="sxs-lookup"><span data-stu-id="2fa78-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="2fa78-124">Wystarczy dodać polecenie MSBuild do pliku. cmd.</span><span class="sxs-lookup"><span data-stu-id="2fa78-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="2fa78-125">W rozwiązaniu Contact Manager folder publikowania zawiera plik o nazwie *Publish-dev. cmd* , który dokładnie robi to.</span><span class="sxs-lookup"><span data-stu-id="2fa78-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> <span data-ttu-id="2fa78-126">Przełącznik **/FL** instruuje program MSBuild, aby utworzył plik dziennika o nazwie *MSBuild. log* w katalogu roboczym, w którym został wywołany program MSBuild. exe.</span><span class="sxs-lookup"><span data-stu-id="2fa78-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>

<span data-ttu-id="2fa78-127">Aby wdrożyć lub wdrożyć ponownie rozwiązanie Contact Manager, wystarczy uruchomić plik *Publish-dev. cmd* .</span><span class="sxs-lookup"><span data-stu-id="2fa78-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="2fa78-128">Po uruchomieniu pliku MSBuild będzie:</span><span class="sxs-lookup"><span data-stu-id="2fa78-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="2fa78-129">Kompiluj wszystkie projekty w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="2fa78-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="2fa78-130">Generuj pakiety internetowe z możliwością wdrażania dla projektów aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2fa78-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="2fa78-131">Generuj pliki. DbSchema i. deploymanifest dla projektów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2fa78-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="2fa78-132">Wdróż pakiety sieci Web na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2fa78-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="2fa78-133">Wdróż bazę danych na serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2fa78-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="2fa78-134">Uruchamianie wdrożenia</span><span class="sxs-lookup"><span data-stu-id="2fa78-134">Run the Deployment</span></span>

<span data-ttu-id="2fa78-135">Po utworzeniu pliku poleceń dla środowiska docelowego powinno być możliwe ukończenie całego wdrożenia, po prostu uruchamiając plik.</span><span class="sxs-lookup"><span data-stu-id="2fa78-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="2fa78-136">**Aby wdrożyć rozwiązanie Contact Manager w środowisku testowym**</span><span class="sxs-lookup"><span data-stu-id="2fa78-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="2fa78-137">Na stacji roboczej dewelopera Otwórz Eksploratora Windows, a następnie przejdź do lokalizacji pliku *Publish-dev. cmd* .</span><span class="sxs-lookup"><span data-stu-id="2fa78-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="2fa78-138">Kliknij dwukrotnie plik, aby go uruchomić.</span><span class="sxs-lookup"><span data-stu-id="2fa78-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="2fa78-139">Jeśli zostanie wyświetlone okno dialogowe **Otwórz plik — ostrzeżenie o zabezpieczeniach** , kliknij przycisk **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="2fa78-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="2fa78-140">Jeśli ustawienia konfiguracji i serwery testowe są prawidłowo skonfigurowane, w oknie wiersza polecenia zostanie wyświetlony komunikat **kompilacja zakończyła się powodzeniem** , gdy MSBuild zakończy przetwarzanie plików projektu.</span><span class="sxs-lookup"><span data-stu-id="2fa78-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="2fa78-141">Jeśli po raz pierwszy wdrożono rozwiązanie w tym środowisku, należy dodać konto testowego serwera sieci Web do roli bazy danych **\_** i bazy danych, **\_** a **w bazie danych** .</span><span class="sxs-lookup"><span data-stu-id="2fa78-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="2fa78-142">Ta procedura została opisana w artykule [Konfigurowanie serwera bazy danych do publikowania Web Deploy](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="2fa78-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="2fa78-143">Po utworzeniu bazy danych wystarczy przypisać te uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="2fa78-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="2fa78-144">Domyślnie proces kompilacji nie utworzy ponownie bazy danych na każdym wdrożeniu&#x2014;, a następnie przeprowadzi porównanie istniejącej bazy danych z najnowszym schematem i wprowadzi tylko wymagane zmiany.</span><span class="sxs-lookup"><span data-stu-id="2fa78-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="2fa78-145">W związku z tym należy tylko zmapować te role bazy danych przy pierwszym wdrożeniu rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="2fa78-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="2fa78-146">Otwórz program Internet Explorer i przejdź do adresu URL aplikacji Contact Manager (na przykład `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="2fa78-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="2fa78-147">Sprawdź, czy aplikacja działa zgodnie z oczekiwaniami, i możesz dodać kontakty.</span><span class="sxs-lookup"><span data-stu-id="2fa78-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="2fa78-148">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="2fa78-148">Conclusion</span></span>

<span data-ttu-id="2fa78-149">Tworzenie pliku poleceń zawierającego instrukcje programu MSBuild umożliwia szybkie i łatwe tworzenie i wdrażanie rozwiązań obejmujących wiele projektów w konkretnym środowisku docelowym.</span><span class="sxs-lookup"><span data-stu-id="2fa78-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="2fa78-150">Jeśli musisz wielokrotnie wdrożyć rozwiązanie w wielu środowiskach docelowych, możesz utworzyć wiele plików poleceń.</span><span class="sxs-lookup"><span data-stu-id="2fa78-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="2fa78-151">W każdym pliku poleceń MSBuild polecenie kompiluje ten sam plik projektu uniwersalnego, ale określi inny plik projektu specyficzny dla środowiska.</span><span class="sxs-lookup"><span data-stu-id="2fa78-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="2fa78-152">Na przykład plik poleceń do opublikowania w środowisku deweloperskim lub testowym może zawierać to polecenie MSBuild:</span><span class="sxs-lookup"><span data-stu-id="2fa78-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

<span data-ttu-id="2fa78-153">Plik polecenia do opublikowania w środowisku przejściowym może zawierać to polecenie MSBuild:</span><span class="sxs-lookup"><span data-stu-id="2fa78-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> <span data-ttu-id="2fa78-154">Aby uzyskać wskazówki dotyczące dostosowywania plików projektu specyficznych dla środowiska dla własnych środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="2fa78-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>

<span data-ttu-id="2fa78-155">Możesz również dostosować proces kompilacji dla każdego środowiska, zastępując właściwości lub ustawiając różne inne przełączniki w poleceniu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="2fa78-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="2fa78-156">Aby uzyskać więcej informacji, zobacz [Dokumentacja wiersza polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="2fa78-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2fa78-157">[Poprzednie](deploying-database-projects.md)
> [dalej](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="2fa78-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
