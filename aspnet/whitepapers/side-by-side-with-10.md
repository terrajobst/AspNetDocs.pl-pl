---
uid: whitepapers/side-by-side-with-10
title: ASP.NET Wykonywanie równoczesne .NET Framework 1,0 i 1,1 | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano sposób instalowania na komputerze programu .NET 1,0 i programu .NET 1,1, co umożliwia uruchamianie aplikacji sieci Web ASP.NET w dowolnej wersji ramki...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632973"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="a7fd1-103">Wykonywanie równoczesne aplikacji .NET Framework 1.0 i 1.1 na platformie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a7fd1-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>

> <span data-ttu-id="a7fd1-104">W tym dokumencie opisano sposób instalowania programów .NET 1,0 i .NET 1,1 na komputerze, co pozwala na uruchamianie aplikacji sieci Web ASP.NET w dowolnej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="a7fd1-105">Dotyczy ASP.NET 1,0 i ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="a7fd1-106">W programie ASP.NET aplikacje są uruchamiane obok siebie, gdy są zainstalowane na tym samym komputerze, ale używają różnych wersji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="a7fd1-107">W poniższym temacie opisano sposób konfigurowania aplikacji ASP.NET do wykonywania równoczesnego i zawiera szczegółowe instrukcje:</span><span class="sxs-lookup"><span data-stu-id="a7fd1-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="a7fd1-108">Zachowaj mapowanie aplikacji sieci Web na .NET Framework w wersji 1,0 podczas instalacji</span><span class="sxs-lookup"><span data-stu-id="a7fd1-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="a7fd1-109">Mapowanie aplikacji sieci Web do określonej wersji .NET Framework</span><span class="sxs-lookup"><span data-stu-id="a7fd1-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="a7fd1-110">Znajdź wersję .NET Framework, z której korzysta witryna sieci Web</span><span class="sxs-lookup"><span data-stu-id="a7fd1-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="a7fd1-111">Tradycyjnie, kiedy składnik lub aplikacja są aktualizowane na komputerze, Starsza wersja jest usuwana i zastępowana nowszą wersją.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="a7fd1-112">Jeśli nowa wersja nie jest zgodna z poprzednią wersją, zwykle przerywa inne aplikacje korzystające z składnika lub aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="a7fd1-113">.NET Framework zapewnia obsługę wykonywania równoczesnego, co umożliwia zainstalowanie wielu wersji zestawu lub aplikacji na tym samym komputerze w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="a7fd1-114">Ze względu na to, że można zainstalować wiele wersji jednocześnie, zarządzane aplikacje mogą wybrać wersję, która ma być używana bez wpływu na aplikacje korzystające z innej wersji.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="a7fd1-115">Domyślnie podczas instalacji .NET Framework w wersji 1,1 wszystkie istniejące aplikacje ASP.NET są automatycznie konfigurowane do korzystania z najnowszej wersji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="a7fd1-116">Jeśli nie chcesz, aby aplikacje ASP.NET były domyślnie .NET Framework 1,1, kliknij [tutaj](#1) , aby dowiedzieć się, jak zapobiegać tym podczas instalacji.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="a7fd1-117">Jeśli zaktualizujesz serwer sieci Web do .NET Framework 1,1 i chcesz, aby co najmniej jedna aplikacja sieci Web działała .NET Framework 1,0, musisz zaktualizować mapę skryptu Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="a7fd1-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="a7fd1-118">Mapowanie skryptu jest mechanizmem mapowania rozszerzenia pliku aspx dla określonej aplikacji sieci Web na wersję .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="a7fd1-119">Kliknij [tutaj](#2) , aby dowiedzieć się, jak zmapować aplikację sieci Web na określoną wersję .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="a7fd1-120">Aby sprawdzić, która wersja .NET Framework ma uruchomioną określoną aplikację sieci Web, można użyć programu Internet Information Manager lub narzędzia rejestracji ASP.NET IIS (ASPNET\_regiis. exe).</span><span class="sxs-lookup"><span data-stu-id="a7fd1-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="a7fd1-121">Kliknij [tutaj](#3) , aby dowiedzieć się, jak znaleźć wersję .NET Framework używaną przez witrynę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="a7fd1-122">Podczas migrowania do .NET Framework 1,1 należy wziąć pod uwagę, że każda wersja .NET Framework używa własnego pliku Machine. config.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="a7fd1-123">W związku z tym, jeśli administrator sieci Web wprowadził zmiany w pliku Machine. config, te zmiany muszą zostać zmigrowane do pliku Machine. config .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="a7fd1-124">Utrzymywanie mapowania aplikacji sieci Web na .NET Framework 1,0 podczas instalacji</span><span class="sxs-lookup"><span data-stu-id="a7fd1-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="a7fd1-125">Domyślnie wszystkie istniejące aplikacje ASP.NET są automatycznie konfigurowane ponownie podczas instalacji w celu korzystania z nowszej wersji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="a7fd1-126">Przy użyciu nowszej wersji .NET Framework aplikacje mogą w pełni korzystać z ulepszeń i nowych funkcji dostępnych w nowej wersji.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="a7fd1-127">W tym samym czasie administrator sieci Web, który może potrzebować szczegółowej kontroli nad uaktualnianymi aplikacjami, może zapobiec automatycznemu ponownemu mapowaniu wszystkich istniejących aplikacji ASP.NET podczas instalacji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="a7fd1-128">Aby zapobiec automatycznemu ponownemu mapowaniu całej aplikacji ASP.NET na nowszą wersję .NET Framework, administrator sieci Web może użyć opcji wiersza polecenia/noaspupgrade z programem instalacyjnym Dotnetfx. exe.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="a7fd1-129">**Aby zapobiec łącznym ponownym mapowaniu aplikacji ASP.NET na nowszą wersję**</span><span class="sxs-lookup"><span data-stu-id="a7fd1-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="a7fd1-130">Przejdź do pozycji **Start**.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-130">Go to **Start**.</span></span>
2. <span data-ttu-id="a7fd1-131">Kliknij przycisk **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-131">Click on **run**.</span></span>
3. <span data-ttu-id="a7fd1-132">Wpisz **cmd**.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-132">Type **cmd**.</span></span>
4. <span data-ttu-id="a7fd1-133">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="a7fd1-134">W wierszu polecenia wpisz następujący wiersz, aby rozpocząć instalację .NET Framework: **Dotnetfx. exe/c: "Install/noaspupgrade?** .</span><span class="sxs-lookup"><span data-stu-id="a7fd1-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="a7fd1-135">Kliknij przycisk **tak** w konfiguracji Microsoft .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="a7fd1-136">Spowoduje to rozpoczęcie procesu instalacji .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="a7fd1-137">Mapowanie aplikacji sieci Web do określonej wersji .NET Framework</span><span class="sxs-lookup"><span data-stu-id="a7fd1-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="a7fd1-138">Każda wersja .NET Framework obejmuje wersję narzędzia rejestracji ASP.NET IIS (ASPNET\_regiis. exe).</span><span class="sxs-lookup"><span data-stu-id="a7fd1-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="a7fd1-139">To narzędzie umożliwia administratorom określenie, że aplikacja sieci Web ma być uruchamiana w określonej wersji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="a7fd1-140">Jest to nazywane mapowaniem aplikacji sieci Web na wersję .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="a7fd1-141">Administratorzy muszą wybrać ASPNET\_regiis. exe, który odnosi się do wersji .NET Framework, która zostanie skojarzona z aplikacją sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="a7fd1-142">Na przykład administrator, który chce określić, że witryna sieci Web używa .NET Framework 1,1 musi używać ASPNET\_regiis. exe, który znajduje się w .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="a7fd1-143">ASPNET\_regiis. exe w wersji 1,0 znajduje się w:</span><span class="sxs-lookup"><span data-stu-id="a7fd1-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="a7fd1-144">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="a7fd1-144">C:\WINDOWS\Microsoft.NET\Framework\\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="a7fd1-145">ASPNET\_regiis. exe dla wersji 1, 1 znajduje się w:</span><span class="sxs-lookup"><span data-stu-id="a7fd1-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="a7fd1-146">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="a7fd1-146">C:\WINDOWS\Microsoft.NET\Framework\\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="a7fd1-147">ASPNET\_regiis. exe oferuje dwie opcje mapowania skryptu aplikacji sieci Web:</span><span class="sxs-lookup"><span data-stu-id="a7fd1-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="a7fd1-148">**-s** ustawia mapę skryptu w ścieżce i w jej katalogach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="a7fd1-149">**-SN** ustawia mapę skryptu tylko w ścieżce.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="a7fd1-150">Ścieżka definiuje ścieżkę metadanych IIS aplikacji sieci Web, która jest zdefiniowana w formie W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="a7fd1-151">Na przykład w przypadku aplikacji sieci Web o nazwie Portal znajdującej się w domyślnej witrynie sieci Web ścieżka metabazy to W3SVC/1/ROOT/Portal.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="a7fd1-152">Należy pamiętać, że można również użyć narzędzia o nazwie Edytor metabazy do pobrania ścieżki metabazy.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="a7fd1-153">To narzędzie można pobrać z witryny pomoc techniczna firmy Microsoft w [https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="a7fd1-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="a7fd1-154">Uruchom ASPNET\_regiis. exe-s W3SVC/1/ROOT/Portal, aby zaktualizować mapę skryptu usług IIS portalu i jej podaplikację.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="a7fd1-155">Uruchom ASPNET\_regiis. exe-SN W3SVC/1/ROOT/Portal, aby zaktualizować mapę skryptu usługi IIS portalu bez wpływu na aplikacje w podkatalogach portalu? s.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="a7fd1-156">Znajdź wersję .NET Framework używaną przez aplikację sieci Web</span><span class="sxs-lookup"><span data-stu-id="a7fd1-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="a7fd1-157">Administrator może użyć Service Manager Internet, aby sprawdzić, która wersja .NET Framework ma uruchomioną witrynę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="a7fd1-158">Różne wersje systemu operacyjnego uruchamiają Internet Service Manager inaczej.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="a7fd1-159">Aby uruchomić program Service Manager, wykonaj czynności opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="a7fd1-160">**Aby uruchomić Internet Service Manager**</span><span class="sxs-lookup"><span data-stu-id="a7fd1-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="a7fd1-161">Przejdź do pozycji **Start**.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-161">Go to **Start**.</span></span>
2. <span data-ttu-id="a7fd1-162">Kliknij przycisk **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-162">Click on **run**.</span></span>
3. <span data-ttu-id="a7fd1-163">Wpisz **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="a7fd1-164">W Service Manager Internet wybierz aplikację sieci Web, której wersję .NET Framework chcesz poznać.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="a7fd1-165">Kliknij prawym przyciskiem myszy aplikację sieci Web, a następnie kliknij pozycję **właściwości.**</span><span class="sxs-lookup"><span data-stu-id="a7fd1-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="a7fd1-166">W oknie właściwości wybierz pozycję **Konfiguracja.**</span><span class="sxs-lookup"><span data-stu-id="a7fd1-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="a7fd1-167">W tabeli mapowanie aplikacji wybierz pozycję **. aspx**, a następnie kliknij pozycję **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="a7fd1-168">W polu tekstowym **plik wykonywalny** Przyjrzyj się katalogowi Version przez przewijanie.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="a7fd1-169">Jeśli katalog wersji to v. 1.1.4322, aplikacja jest mapowana na .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="a7fd1-170">Z drugiej strony, jeśli katalog wersji to v 1.0.3705, aplikacja jest mapowana na .NET Framework 1,0.</span><span class="sxs-lookup"><span data-stu-id="a7fd1-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
