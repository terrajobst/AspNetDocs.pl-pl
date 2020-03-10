---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Dodawanie zawartości do kontroli źródła | Microsoft Docs
author: jrjlee
description: W tym temacie wyjaśniono, jak dodać zawartość do kontroli źródła w Team Foundation Server (TFS) 2010. Opisuje sposób dodawania rozwiązań i projektów do zespołu projektu...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634464"
---
# <a name="adding-content-to-source-control"></a><span data-ttu-id="8ecaa-104">Dodawanie zawartości do kontroli źródła</span><span class="sxs-lookup"><span data-stu-id="8ecaa-104">Adding Content to Source Control</span></span>

<span data-ttu-id="8ecaa-105">Autor [Jason Lewandowski](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="8ecaa-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="8ecaa-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="8ecaa-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="8ecaa-107">W tym temacie wyjaśniono, jak dodać zawartość do kontroli źródła w Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="8ecaa-108">Opisano w nim sposób dodawania rozwiązań i projektów do projektu zespołowego w programie TFS i wyjaśniono, jak dodać zależności zewnętrzne, takie jak struktury lub zestawy, do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>

<span data-ttu-id="8ecaa-109">Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="8ecaa-110">Przegląd zadania</span><span class="sxs-lookup"><span data-stu-id="8ecaa-110">Task Overview</span></span>

<span data-ttu-id="8ecaa-111">W większości przypadków każdy członek zespołu deweloperów powinien mieć możliwość dodawania zawartości do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="8ecaa-112">Aby dodać rozwiązanie do kontroli źródła w programie TFS, należy wykonać następujące czynności wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="8ecaa-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="8ecaa-113">Połącz z projektem zespołowym.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-113">Connect to a team project.</span></span>
- <span data-ttu-id="8ecaa-114">Mapuj strukturę folderów projektu zespołowego na serwerze do struktury folderów na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="8ecaa-115">Dodaj rozwiązanie i jego zawartość do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="8ecaa-116">Dodaj wszystkie zależności zewnętrzne do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="8ecaa-117">W tym temacie pokazano, jak wykonać te procedury.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="8ecaa-118">W zadaniach i instruktażach w tym temacie przyjęto założenie, że nowy projekt zespołowy TFS został już utworzony w celu zarządzania zawartością.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="8ecaa-119">Aby uzyskać więcej informacji na temat tworzenia nowego projektu zespołowego, zobacz [Tworzenie projektu zespołowego w programie TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="8ecaa-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="8ecaa-120">Kto wykonuje te procedury?</span><span class="sxs-lookup"><span data-stu-id="8ecaa-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="8ecaa-121">W większości przypadków każdy członek zespołu deweloperów powinien mieć możliwość dodawania i modyfikowania zawartości w określonych projektach zespołowych.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="8ecaa-122">Nawiązywanie połączenia z projektem zespołowym i Tworzenie mapowania folderów</span><span class="sxs-lookup"><span data-stu-id="8ecaa-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="8ecaa-123">Przed dodaniem jakiejkolwiek zawartości do kontroli źródła należy połączyć się z projektem zespołowym i utworzyć mapowanie między strukturą folderów na serwerze i systemem plików na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="8ecaa-124">**Aby połączyć się z projektem zespołowym i zmapować ścieżkę lokalną**</span><span class="sxs-lookup"><span data-stu-id="8ecaa-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="8ecaa-125">Na stacji roboczej dewelopera Otwórz program Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="8ecaa-126">W programie Visual Studio w menu **zespół** kliknij polecenie **Połącz z Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8ecaa-127">Jeśli połączenie z serwerem TFS zostało już skonfigurowane, możesz pominąć kroki 3-6.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="8ecaa-128">W oknie dialogowym **połączenie z projektem zespołowym** kliknij pozycję **serwery**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="8ecaa-129">W oknie dialogowym **Dodawanie/usuwanie Team Foundation Server** kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="8ecaa-130">W oknie dialogowym **dodawanie Team Foundation Server** Podaj szczegóły wystąpienia programu TFS, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="8ecaa-131">W oknie dialogowym **Dodawanie/usuwanie Team Foundation Server** kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="8ecaa-132">W oknie dialogowym **Połącz z projektem zespołowym** wybierz wystąpienie TFS, z którym chcesz nawiązać połączenie, wybierz kolekcję projektu zespołowego, wybierz projekt zespołowy, do którego chcesz dodać, a następnie kliknij przycisk **Połącz**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="8ecaa-133">W oknie **Team Explorer** rozwiń projekt zespołowy, a następnie kliknij dwukrotnie pozycję **Kontrola źródła**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="8ecaa-134">Na karcie **Eksploator kontroli źródła** kliknij pozycję **niezamapowane**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="8ecaa-135">W oknie dialogowym **Mapa** w polu **folder lokalny** przejdź do (lub Utwórz) folder lokalny do działania jako folder główny projektu zespołowego, a następnie kliknij pozycję **Mapuj**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="8ecaa-136">Po wyświetleniu monitu o pobranie plików źródłowych kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="8ecaa-137">Na tym etapie został zmapowany folder po stronie serwera dla projektu zespołowego do folderu lokalnego na stacji roboczej dewelopera.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="8ecaa-138">Pobrano również istniejącą zawartość z projektu zespołowego do lokalnej struktury folderów.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="8ecaa-139">Teraz możesz zacząć dodawać własną zawartość do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="8ecaa-140">Dodaj projekty i rozwiązania do kontroli źródła</span><span class="sxs-lookup"><span data-stu-id="8ecaa-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="8ecaa-141">Aby dodać projekty i rozwiązania do kontroli źródła, należy najpierw przenieść je do folderu mapowanego dla projektu zespołowego na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="8ecaa-142">Następnie możesz zaewidencjonować zawartość, aby zsynchronizować dodatki z serwerem.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="8ecaa-143">**Aby dodać projekty do kontroli źródła**</span><span class="sxs-lookup"><span data-stu-id="8ecaa-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="8ecaa-144">Na stacji roboczej dewelopera Przenieś swoje projekty i rozwiązania do odpowiedniej lokalizacji w mapowanej strukturze folderów dla projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8ecaa-145">Wiele organizacji będzie mieć preferowane podejście do organizowania projektów i rozwiązań w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="8ecaa-146">Aby uzyskać wskazówki dotyczące struktury folderów, zobacz [jak: strukturę folderów kontroli źródła w Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ecaa-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="8ecaa-147">Otwórz rozwiązanie w programie Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="8ecaa-148">W oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij polecenie **Dodaj rozwiązanie do kontroli źródła**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="8ecaa-149">W niektórych przypadkach, w zależności od tego, jak Twoja organizacja lubi zawartość struktury w programie TFS, może być konieczne dodanie projektów do kontroli źródła pojedynczo, aby zapewnić bardziej precyzyjną kontrolę nad sposobem organizowania kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="8ecaa-150">Sprawdź, czy karta **Eksploator kontroli źródła** wyświetla zawartość dodaną w strukturze folderów serwera dla projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="8ecaa-151">Karta **Eksploator kontroli źródła** wyświetla zawartość bez dalszych monitów, ponieważ dodano rozwiązanie do zamapowanego folderu w lokalnym systemie plików.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="8ecaa-152">Jeśli Twoje rozwiązanie znajduje się w niemapowanej lokalizacji, zostanie wyświetlony monit o określenie lokalizacji folderów zarówno w programie TFS, jak i w lokalnym systemie plików.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="8ecaa-153">Na karcie **Eksploator kontroli źródła** w okienku **foldery** kliknij prawym przyciskiem myszy projekt zespołowy (na przykład **ContactManager**), a następnie kliknij polecenie **Zaewidencjonuj oczekujące zmiany**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="8ecaa-154">W oknie dialogowym **zaewidencjonuj — pliki źródłowe** wpisz komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="8ecaa-155">W tym momencie dodaliśmy Twoje rozwiązanie do kontroli źródła w programie TFS.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="8ecaa-156">Dodaj zależności zewnętrzne do kontroli źródła</span><span class="sxs-lookup"><span data-stu-id="8ecaa-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="8ecaa-157">Dodanie projektu lub rozwiązania do kontroli źródła spowoduje również dodanie wszystkich plików i folderów w projekcie lub rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="8ecaa-158">Jednak w wielu przypadkach projekty i rozwiązania opierają się również na zależnościach zewnętrznych, takich jak zestawy lokalne, aby działać prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="8ecaa-159">Należy dodać wszystkie takie zasoby do kontroli źródła, aby umożliwić kompilację zespołową i innych członków zespołu deweloperów pomyślnie skompilowane kod.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="8ecaa-160">Przykładowo struktura folderów dla przykładowego rozwiązania Contact Manager zawiera folder o nazwie Packages.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="8ecaa-161">Zawiera zestaw i różne zasoby pomocnicze dla ADO.NET Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="8ecaa-162">Folder Packages nie jest częścią rozwiązania Contact Manager, ale rozwiązanie nie zostanie pomyślnie skompilowane.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="8ecaa-163">Aby umożliwić kompilację zespołu w celu skompilowania rozwiązania, należy dodać folder Packages do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="8ecaa-164">Dołączenie folderu Packages jest typowe w przypadku dodania Entity Framework lub podobnych zasobów do rozwiązania przy użyciu rozszerzenia NuGet dla programu Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>

<span data-ttu-id="8ecaa-165">**Aby dodać zawartość spoza projektu do kontroli źródła**</span><span class="sxs-lookup"><span data-stu-id="8ecaa-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="8ecaa-166">Upewnij się, że elementy, które mają zostać dodane (na przykład folder Packages) znajdują się w odpowiedniej lokalizacji w zamapowanym folderze w lokalnym systemie plików.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="8ecaa-167">W programie Visual Studio 2010, w oknie **Team Explorer** rozwiń projekt zespołowy, a następnie kliknij dwukrotnie pozycję **Kontrola źródła**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="8ecaa-168">Na karcie **Eksploator kontroli źródła** w okienku **foldery** wybierz folder zawierający element lub elementy, które chcesz dodać.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="8ecaa-169">Kliknij przycisk **Dodaj elementy do folderu** .</span><span class="sxs-lookup"><span data-stu-id="8ecaa-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="8ecaa-170">W oknie dialogowym **Dodaj do kontroli źródła** zaznacz folder lub elementy, które chcesz dodać, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="8ecaa-171">Na karcie **wykluczone elementy** wybierz wszystkie wymagane elementy, które zostały automatycznie wykluczone (na przykład zestawy), a następnie kliknij pozycję **Dołącz element (y)** .</span><span class="sxs-lookup"><span data-stu-id="8ecaa-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="8ecaa-172">Na karcie **elementy do dodania** Sprawdź, czy wszystkie pliki, które mają zostać uwzględnione, znajdują się na liście, a następnie kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="8ecaa-173">W oknie **Eksploator kontroli źródła** kliknij przycisk **Zaewidencjonuj** .</span><span class="sxs-lookup"><span data-stu-id="8ecaa-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="8ecaa-174">W oknie dialogowym **zaewidencjonuj — pliki źródłowe** wpisz komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="8ecaa-175">W tym momencie dodano zależności zewnętrzne dla rozwiązania do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="8ecaa-176">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="8ecaa-176">Conclusion</span></span>

<span data-ttu-id="8ecaa-177">W tym temacie opisano sposób nawiązywania połączenia z projektem zespołowym, mapowania struktury folderów i dodawania zawartości do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="8ecaa-178">Aby uzyskać więcej informacji na temat pracy z elementami w obszarze kontroli źródła, zobacz [Używanie kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ecaa-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="8ecaa-179">W następnym temacie [Konfigurowanie serwera kompilacji TFS na potrzeby wdrażania w sieci Web](configuring-a-tfs-build-server-for-web-deployment.md)zawiera opis sposobu przygotowania serwera kompilacji zespołu TFS do kompilowania i wdrażania rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="8ecaa-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="8ecaa-180">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="8ecaa-180">Further Reading</span></span>

<span data-ttu-id="8ecaa-181">Aby uzyskać bardziej szczegółowe informacje na temat pracy z kontrolą źródła w programie TFS, zobacz [Korzystanie z kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ecaa-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ecaa-182">[Poprzednie](creating-a-team-project-in-tfs.md)
> [dalej](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="8ecaa-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
