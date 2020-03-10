---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Tworzenie projektu zespołowego w programie TFS | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639658"
---
# <a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="44608-103">Tworzenie projektu zespołowego na serwerze TFS</span><span class="sxs-lookup"><span data-stu-id="44608-103">Creating a Team Project in TFS</span></span>

<span data-ttu-id="44608-104">Autor [Jason Lewandowski](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="44608-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="44608-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="44608-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="44608-106">W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="44608-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>

<span data-ttu-id="44608-107">Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="44608-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="44608-108">Przegląd zadania</span><span class="sxs-lookup"><span data-stu-id="44608-108">Task Overview</span></span>

<span data-ttu-id="44608-109">Aby zainicjować obsługę administracyjną nowego projektu zespołowego i korzystać z niego w programie TFS, należy wykonać następujące czynności wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="44608-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="44608-110">Udziel uprawnień użytkownikowi, który utworzy nowy projekt zespołowy.</span><span class="sxs-lookup"><span data-stu-id="44608-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="44608-111">Utwórz projekt zespołowy.</span><span class="sxs-lookup"><span data-stu-id="44608-111">Create the team project.</span></span>
- <span data-ttu-id="44608-112">Udziel uprawnień członkom zespołu, którzy będą współpracować nad projektem.</span><span class="sxs-lookup"><span data-stu-id="44608-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="44608-113">Zaewidencjonuj zawartość.</span><span class="sxs-lookup"><span data-stu-id="44608-113">Check in some content.</span></span>

<span data-ttu-id="44608-114">W tym temacie pokazano, jak wykonać te procedury i zidentyfikować użytkowników i role zadań, które mogą być odpowiedzialne za każdą procedurę.</span><span class="sxs-lookup"><span data-stu-id="44608-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="44608-115">Należy pamiętać, że w zależności od struktury organizacji każde z tych zadań może być obowiązkiem innej osoby.</span><span class="sxs-lookup"><span data-stu-id="44608-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="44608-116">W zadaniach i przewodnikach w tym temacie przyjęto założenie, że zainstalowano i skonfigurowano program TFS oraz że kolekcja projektów zespołowych została utworzona w ramach procesu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="44608-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="44608-117">Aby uzyskać więcej informacji na temat tych założeń i bardziej ogólnych informacji w tle dotyczących scenariusza, zobacz [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="44608-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="44608-118">Udziel uprawnień do twórcy projektu zespołowego</span><span class="sxs-lookup"><span data-stu-id="44608-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="44608-119">Aby można było utworzyć nowy projekt zespołowy, potrzebne są następujące uprawnienia:</span><span class="sxs-lookup"><span data-stu-id="44608-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="44608-120">Musisz mieć uprawnienie **Tworzenie nowych projektów** w warstwie aplikacji TFS.</span><span class="sxs-lookup"><span data-stu-id="44608-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="44608-121">To uprawnienie jest zazwyczaj przyznawane przez dodanie użytkowników do grupy TFS **Administratorzy kolekcji projektów** .</span><span class="sxs-lookup"><span data-stu-id="44608-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="44608-122">Grupa globalna **administratorów Team Foundation** obejmuje również to uprawnienie.</span><span class="sxs-lookup"><span data-stu-id="44608-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="44608-123">Musisz mieć uprawnienia do tworzenia nowych witryn zespołu w kolekcji witryn programu SharePoint, która odpowiada kolekcji projektów zespołowych TFS.</span><span class="sxs-lookup"><span data-stu-id="44608-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="44608-124">To uprawnienie jest zazwyczaj udzielane przez dodanie użytkownika do grupy programu SharePoint z uprawnieniami **pełna kontrola** w kolekcji witryn programu SharePoint.</span><span class="sxs-lookup"><span data-stu-id="44608-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="44608-125">Jeśli używasz funkcji SQL Server Reporting Services, musisz być członkiem roli **Menedżer zawartości Team Foundation** w usługach Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="44608-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="44608-126">Kto wykonuje te procedury?</span><span class="sxs-lookup"><span data-stu-id="44608-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="44608-127">Zazwyczaj te procedury są wykonywane przez osobę lub grupę, która administruje wdrożeniem programu TFS.</span><span class="sxs-lookup"><span data-stu-id="44608-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="44608-128">Ponieważ jest to wysoce uprzywilejowany zestaw uprawnień, nowe projekty zespołowe są zwykle tworzone przez niewielki podzbiór użytkowników odpowiedzialnych za administrowanie wdrożeniem programu TFS.</span><span class="sxs-lookup"><span data-stu-id="44608-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="44608-129">Deweloperzy nie będą zwykle przyznawać uprawnień wymaganych do tworzenia nowych projektów zespołowych.</span><span class="sxs-lookup"><span data-stu-id="44608-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="44608-130">Przyznawanie uprawnień w programie TFS</span><span class="sxs-lookup"><span data-stu-id="44608-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="44608-131">Jeśli chcesz umożliwić użytkownikowi tworzenie nowych projektów zespołowych, pierwsze zadanie wysokiego poziomu polega na dodaniu użytkownika do grupy **Administratorzy kolekcji projektów** dla kolekcji projektów zespołowych.</span><span class="sxs-lookup"><span data-stu-id="44608-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="44608-132">**Aby dodać użytkownika do grupy administratorów kolekcji projektów**</span><span class="sxs-lookup"><span data-stu-id="44608-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="44608-133">Na serwerze TFS, w menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft Team Foundation Server 2010**, a następnie kliknij pozycję **Konsola administracyjna serwera Team Foundation**.</span><span class="sxs-lookup"><span data-stu-id="44608-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="44608-134">W widoku drzewa nawigacji rozwiń węzeł **warstwa aplikacji**, a następnie kliknij pozycję **kolekcje projektu zespołowego**.</span><span class="sxs-lookup"><span data-stu-id="44608-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="44608-135">W okienku **kolekcje projektu zespołowego** wybierz kolekcję projektu zespołowego, którą chcesz zarządzać.</span><span class="sxs-lookup"><span data-stu-id="44608-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="44608-136">Na karcie **Ogólne** kliknij opcję **członkostwo w grupie**.</span><span class="sxs-lookup"><span data-stu-id="44608-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="44608-137">W oknie dialogowym **grupy globalne** wybierz grupę **Administratorzy kolekcji projektu** , a następnie kliknij przycisk **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="44608-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="44608-138">W oknie dialogowym **Właściwości grupy Team Foundation Server** wybierz pozycję **użytkownik lub Grupa systemu Windows**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="44608-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="44608-139">W oknie dialogowym **Wybieranie użytkowników, komputerów lub grup** wpisz nazwę użytkownika, dla którego chcesz mieć możliwość tworzenia nowych projektów zespołowych, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="44608-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="44608-140">W oknie dialogowym **Właściwości grupy Team Foundation Server** kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="44608-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="44608-141">W oknie dialogowym **grupy globalne** kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="44608-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="44608-142">Przyznawanie uprawnień w usługach SharePoint</span><span class="sxs-lookup"><span data-stu-id="44608-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="44608-143">Następnie należy przyznać użytkownikowi uprawnienia do tworzenia nowych witryn zespołu w kolekcji witryn programu SharePoint, która odpowiada kolekcji projektów zespołowych TFS.</span><span class="sxs-lookup"><span data-stu-id="44608-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="44608-144">**Aby udzielić uprawnień Pełna kontrola w zbiorze witryn programu SharePoint**</span><span class="sxs-lookup"><span data-stu-id="44608-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="44608-145">W Konsola administracyjna Team Foundation Server na stronie **kolekcje projektu zespołowego** wybierz kolekcję projektu zespołowego, którą chcesz zarządzać.</span><span class="sxs-lookup"><span data-stu-id="44608-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="44608-146">Na karcie **Witryna programu SharePoint** Zanotuj wartość **bieżącego domyślnego adresu URL lokalizacji witryny** .</span><span class="sxs-lookup"><span data-stu-id="44608-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="44608-147">Otwórz program Internet Explorer, a następnie przejdź do adresu URL zanotowanego w kroku 2.</span><span class="sxs-lookup"><span data-stu-id="44608-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="44608-148">Jeśli nie jesteś zalogowany do systemu Windows jako użytkownik, który utworzył kolekcję projektu zespołowego, musisz zalogować się do programu SharePoint jako ten użytkownik, aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="44608-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="44608-149">W menu **Akcje lokacji** kliknij polecenie **Ustawienia lokacji**.</span><span class="sxs-lookup"><span data-stu-id="44608-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="44608-150">Na stronie **Ustawienia witryny** w obszarze **Użytkownicy i uprawnienia**kliknij pozycję **osoby i grupy**.</span><span class="sxs-lookup"><span data-stu-id="44608-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="44608-151">W okienku nawigacji po lewej stronie kliknij pozycję **grupy**.</span><span class="sxs-lookup"><span data-stu-id="44608-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="44608-152">Na stronie **osoby i grupy: wszystkie grupy** kliknij pozycję **Skonfiguruj grupy dla tej witryny**.</span><span class="sxs-lookup"><span data-stu-id="44608-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="44608-153">Może zostać wyświetlony błąd <strong>HTTP 404 nie znaleziono</strong> z powodu podwójnej usterki kodowania http.</span><span class="sxs-lookup"><span data-stu-id="44608-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="44608-154">Jeśli tak się stanie, Zastąp adres URL:</span><span class="sxs-lookup"><span data-stu-id="44608-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="44608-155">`[site_collection_URL]/_layouts/permsetup.aspx` na przykład:</span><span class="sxs-lookup"><span data-stu-id="44608-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="44608-156">Na stronie **Konfigurowanie grup dla tej lokacji** Dodaj użytkownika, który będzie tworzyć projekty zespołowe w grupie **właściciele** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="44608-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="44608-157">Aby uzyskać więcej informacji na temat umożliwiania użytkownikom tworzenia nowych projektów zespołowych w kolekcji projektów zespołowych, zobacz [Ustawianie uprawnień administratora dla kolekcji projektów zespołowych](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="44608-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="44608-158">Tworzenie nowego projektu zespołowego i Dodawanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="44608-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="44608-159">Gdy masz wymagane uprawnienia, możesz użyć okna **Team Explorer** w programie Visual Studio 2010, aby utworzyć nowy projekt zespołowy.</span><span class="sxs-lookup"><span data-stu-id="44608-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="44608-160">To podejście umożliwia kreatorowi gromadzenie wszystkich wymaganych informacji i wykonywanie niezbędnych zadań w programie TFS, SharePoint i SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="44608-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="44608-161">Należy również przyznać uprawnienia do nowego projektu zespołowego członkom zespołu deweloperów, aby umożliwić im Dodawanie i modyfikowanie zawartości.</span><span class="sxs-lookup"><span data-stu-id="44608-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="44608-162">Kto wykonuje te procedury?</span><span class="sxs-lookup"><span data-stu-id="44608-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="44608-163">Zwykle administrator programu TFS lub lider zespołu deweloperów wykonuje te procedury.</span><span class="sxs-lookup"><span data-stu-id="44608-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="44608-164">Utwórz nowy projekt zespołowy</span><span class="sxs-lookup"><span data-stu-id="44608-164">Create a New Team Project</span></span>

<span data-ttu-id="44608-165">W następnej procedurze opisano sposób tworzenia nowego projektu zespołowego w programie TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="44608-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="44608-166">**Aby utworzyć nowy projekt zespołowy**</span><span class="sxs-lookup"><span data-stu-id="44608-166">**To create a new team project**</span></span>

1. <span data-ttu-id="44608-167">W menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft Visual Studio 2010**, kliknij prawym przyciskiem myszy pozycję **Microsoft Visual Studio 2010**, a następnie kliknij polecenie **Uruchom jako administrator**.</span><span class="sxs-lookup"><span data-stu-id="44608-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="44608-168">Jeśli nie uruchomisz programu Visual Studio 2010 jako administrator, Kreator nowego projektu zespołowego zakończy się niepowodzeniem w ostatnim kroku.</span><span class="sxs-lookup"><span data-stu-id="44608-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="44608-169">Jeśli zostanie wyświetlone okno dialogowe **Kontrola konta użytkownika** , kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="44608-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="44608-170">W programie Visual Studio w menu **zespół** kliknij polecenie **Połącz z Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="44608-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="44608-171">Jeśli połączenie z serwerem TFS zostało już skonfigurowane, możesz pominąć kroki 4-7.</span><span class="sxs-lookup"><span data-stu-id="44608-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="44608-172">W oknie dialogowym **połączenie z projektem zespołowym** kliknij pozycję **serwery**.</span><span class="sxs-lookup"><span data-stu-id="44608-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="44608-173">W oknie dialogowym **Dodawanie/usuwanie Team Foundation Server** kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="44608-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="44608-174">W oknie dialogowym **dodawanie Team Foundation Server** Podaj szczegóły wystąpienia programu TFS, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="44608-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="44608-175">W oknie dialogowym **Dodawanie/usuwanie Team Foundation Server** kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="44608-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="44608-176">W oknie dialogowym **Połącz z projektem zespołowym** wybierz wystąpienie TFS, z którym chcesz nawiązać połączenie, wybierz kolekcję projektu zespołowego, do której chcesz dodać, a następnie kliknij przycisk **Połącz**.</span><span class="sxs-lookup"><span data-stu-id="44608-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="44608-177">W oknie **Team Explorer** kliknij prawym przyciskiem myszy kolekcję projektu zespołowego, a następnie kliknij pozycję **Nowy projekt zespołowy**.</span><span class="sxs-lookup"><span data-stu-id="44608-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="44608-178">W oknie dialogowym **Nowy projekt zespołowy** Podaj nazwę i opis projektu zespołowego, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="44608-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="44608-179">Jeśli projekt zespołowy zawiera spacje, podczas korzystania z narzędzia do wdrażania w sieci Web (Web Deploy) Internet Information Services (IIS) w celu wdrożenia pakietów z ścieżki wyjściowej mogą wystąpić pewne problemy.</span><span class="sxs-lookup"><span data-stu-id="44608-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="44608-180">Spacje w ścieżce mogą znacznie trudniejsze uruchamiać Web Deploy polecenia.</span><span class="sxs-lookup"><span data-stu-id="44608-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="44608-181">Na stronie **Wybierz szablon procesu** wybierz szablon procesu, który ma być używany do zarządzania procesem opracowywania, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="44608-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="44608-182">Aby uzyskać więcej informacji na temat szablonów procesów dla TFS, zobacz [Szablony procesów i narzędzia](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="44608-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="44608-183">Na stronie **Ustawienia witryny zespołu** Pozostaw ustawienia domyślne bez zmian, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="44608-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="44608-184">To ustawienie służy do tworzenia lub identyfikowania witryny zespołu programu SharePoint, która jest skojarzona z projektem zespołowym TFS.</span><span class="sxs-lookup"><span data-stu-id="44608-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="44608-185">Zespół programistyczny może używać tej witryny do zarządzania dokumentacją, uczestniczenia w wątkach dyskusji, tworzenia stron typu wiki i wykonywania różnych zadań, które nie są związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="44608-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="44608-186">Aby uzyskać więcej informacji, zobacz [interakcje między produktami programu SharePoint i Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="44608-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="44608-187">Na stronie **Określanie ustawień kontroli źródła** Pozostaw ustawienia domyślne bez zmian, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="44608-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="44608-188">To ustawienie służy do identyfikowania lub tworzenia lokalizacji w hierarchii folderów TFS, która będzie pełnić rolę folderu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="44608-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="44608-189">Na stronie **Potwierdzanie ustawień projektu zespołowego** kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="44608-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="44608-190">Po pomyślnym utworzeniu nowego projektu zespołowego na stronie **projekt zespołowy** kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="44608-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="44608-191">Dodawanie użytkowników do projektu zespołowego</span><span class="sxs-lookup"><span data-stu-id="44608-191">Add Users to a Team Project</span></span>

<span data-ttu-id="44608-192">Po utworzeniu nowego projektu zespołowego możesz udzielić uprawnień użytkownikom, aby mogli rozpocząć dodawanie i współpracę nad zawartością.</span><span class="sxs-lookup"><span data-stu-id="44608-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="44608-193">**Aby dodać użytkowników do projektu zespołowego**</span><span class="sxs-lookup"><span data-stu-id="44608-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="44608-194">W programie Visual Studio 2010, w oknie **Team Explorer** kliknij prawym przyciskiem myszy projekt zespołowy, wskaż polecenie **Ustawienia projektu zespołowego**, a następnie kliknij pozycję członkostwo w **grupie**.</span><span class="sxs-lookup"><span data-stu-id="44608-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="44608-195">Aby umożliwić użytkownikowi dodawanie, modyfikowanie i usuwanie kodu pod kontrolą źródła, należy dodać go do grupy **współautorów** .</span><span class="sxs-lookup"><span data-stu-id="44608-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="44608-196">W oknie dialogowym **grupy projektu** wybierz grupę **współautorów** , a następnie kliknij przycisk **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="44608-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="44608-197">W oknie dialogowym **Właściwości grupy Team Foundation Server** wybierz pozycję **użytkownik lub Grupa systemu Windows**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="44608-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="44608-198">W oknie dialogowym **Wybieranie użytkowników, komputerów lub grup** wpisz nazwę użytkownika, który chcesz dodać do projektu zespołowego, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="44608-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="44608-199">W oknie dialogowym **Właściwości grupy Team Foundation Server** kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="44608-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="44608-200">W oknie dialogowym **grupy projektu** kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="44608-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="44608-201">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="44608-201">Conclusion</span></span>

<span data-ttu-id="44608-202">W tym momencie nowy projekt zespołowy jest gotowy do użycia, a zespół deweloperów może rozpocząć dodawanie zawartości i współpracę w procesie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="44608-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="44608-203">Następny temat, [Dodawanie zawartości do kontroli źródła](adding-content-to-source-control.md), zawiera opis sposobu dodawania zawartości do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="44608-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="44608-204">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="44608-204">Further Reading</span></span>

<span data-ttu-id="44608-205">Aby uzyskać szersze wskazówki dotyczące tworzenia projektów zespołowych w programie TFS, zobacz [Tworzenie projektu zespołowego](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="44608-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="44608-206">Aby uzyskać więcej informacji na temat umożliwiania użytkownikom tworzenia nowych projektów zespołowych w kolekcji projektów zespołowych, zobacz [Ustawianie uprawnień administratora dla kolekcji projektów zespołowych](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="44608-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="44608-207">Aby uzyskać więcej informacji na temat dodawania użytkowników do projektów zespołowych, zobacz [Dodawanie użytkowników do projektów zespołowych](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="44608-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="44608-208">[Poprzednie](configuring-team-foundation-server-for-web-deployment.md)
> [dalej](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="44608-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
