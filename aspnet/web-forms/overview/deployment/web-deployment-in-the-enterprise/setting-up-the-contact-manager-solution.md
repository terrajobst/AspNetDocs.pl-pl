---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Konfigurowanie rozwiązania Contact Manager | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób pobierania i konfigurowania rozwiązania Contact Manager do uruchamiania lokalnego na stacji roboczej dewelopera.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629858"
---
# <a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="61fcd-103">Konfigurowanie rozwiązania Contact Manager</span><span class="sxs-lookup"><span data-stu-id="61fcd-103">Setting Up the Contact Manager Solution</span></span>

<span data-ttu-id="61fcd-104">Autor [Jason Lewandowski](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="61fcd-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="61fcd-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="61fcd-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="61fcd-106">W tym temacie opisano sposób pobierania i konfigurowania rozwiązania Contact Manager do uruchamiania lokalnego na stacji roboczej dewelopera.</span><span class="sxs-lookup"><span data-stu-id="61fcd-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>

## <a name="system-requirements"></a><span data-ttu-id="61fcd-107">Wymagania systemu</span><span class="sxs-lookup"><span data-stu-id="61fcd-107">System Requirements</span></span>

<span data-ttu-id="61fcd-108">Aby uruchomić rozwiązanie Contact Manager lokalnie i wykonać inne zadania opisane w tym samouczku, musisz zainstalować to oprogramowanie na stacji roboczej dewelopera:</span><span class="sxs-lookup"><span data-stu-id="61fcd-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="61fcd-109">Visual Studio 2010 z dodatkiem Service Pack 1, Premium lub Ultimate</span><span class="sxs-lookup"><span data-stu-id="61fcd-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="61fcd-110">Internet Information Services (IIS) 7,5 Express</span><span class="sxs-lookup"><span data-stu-id="61fcd-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="61fcd-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="61fcd-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="61fcd-112">Narzędzie do wdrażania w sieci Web usług IIS (Web Deploy) 2,1 lub nowsze</span><span class="sxs-lookup"><span data-stu-id="61fcd-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="61fcd-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="61fcd-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="61fcd-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="61fcd-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="61fcd-115">Program .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="61fcd-115">.NET Framework 4</span></span>
- <span data-ttu-id="61fcd-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="61fcd-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="61fcd-117">Z wyjątkiem programu Visual Studio 2010 można pobrać i zainstalować najnowsze wersje wszystkich tych produktów i składników za pomocą [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="61fcd-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="61fcd-118">Pobierz i Wyodrębnij rozwiązanie</span><span class="sxs-lookup"><span data-stu-id="61fcd-118">Download and Extract the Solution</span></span>

<span data-ttu-id="61fcd-119">Możesz pobrać przykładową aplikację Contact Manager z galerii kodu MSDN [tutaj](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="61fcd-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="61fcd-120">Konfigurowanie i uruchamianie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="61fcd-120">Configure and Run the Solution</span></span>

<span data-ttu-id="61fcd-121">Aby skonfigurować i uruchomić rozwiązanie Contact Manager na komputerze lokalnym, należy wykonać następujące czynności wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="61fcd-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="61fcd-122">Jeśli jeszcze tego nie zrobiono, utwórz lokalną bazę danych usług aplikacji ASP.NET z włączoną funkcją członkostwa i zarządzania rolami.</span><span class="sxs-lookup"><span data-stu-id="61fcd-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="61fcd-123">Edytuj parametry połączenia w plikach *Web. config* , aby wskazywały lokalne wystąpienie SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="61fcd-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="61fcd-124">Uruchom rozwiązanie z programu Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="61fcd-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="61fcd-125">Pozostała część tej sekcji zawiera więcej wskazówek na temat wykonywania poszczególnych zadań.</span><span class="sxs-lookup"><span data-stu-id="61fcd-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="61fcd-126">**Aby utworzyć bazę danych usług aplikacji**</span><span class="sxs-lookup"><span data-stu-id="61fcd-126">**To create the application services database**</span></span>

1. <span data-ttu-id="61fcd-127">Otwórz wiersz polecenia programu Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="61fcd-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="61fcd-128">W tym celu w menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft Visual Studio 2010**, kliknij pozycję **Visual Studio Tools**, a następnie kliknij pozycję **wiersz polecenia programu Visual Studio (2010)** .</span><span class="sxs-lookup"><span data-stu-id="61fcd-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="61fcd-129">W wierszu polecenia wpisz następujące polecenie, a następnie naciśnij klawisz ENTER:</span><span class="sxs-lookup"><span data-stu-id="61fcd-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="61fcd-130">Użyj przełącznika **– C** , aby określić parametry połączenia dla serwera bazy danych.</span><span class="sxs-lookup"><span data-stu-id="61fcd-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="61fcd-131">Użyj przełącznika **– A** , aby określić funkcje usług aplikacji, które chcesz dodać do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="61fcd-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="61fcd-132">W takim przypadku **m** wskazuje, że chcesz dodać obsługę dostawcy członkostwa i **r** wskazuje, że chcesz dodać obsługę dla menedżera ról.</span><span class="sxs-lookup"><span data-stu-id="61fcd-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="61fcd-133">Użyj przełącznika **– d** , aby określić nazwę bazy danych usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61fcd-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="61fcd-134">Pominięcie tego przełącznika spowoduje utworzenie bazy danych o nazwie domyślnej **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="61fcd-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="61fcd-135">Po pomyślnym utworzeniu bazy danych wiersz polecenia wyświetli potwierdzenie.</span><span class="sxs-lookup"><span data-stu-id="61fcd-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="61fcd-136">Aby uzyskać więcej informacji na temat narzędzia regsql ASPNET\_, zobacz [ASP.NET SQL Server Registration Tool (aspnet\_regsql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="61fcd-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>

<span data-ttu-id="61fcd-137">Następnym krokiem jest upewnienie się, że parametry połączenia w rozwiązaniu Contact Manager wskazują lokalne wystąpienie SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="61fcd-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="61fcd-138">**Aby zaktualizować parametry połączenia**</span><span class="sxs-lookup"><span data-stu-id="61fcd-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="61fcd-139">Otwórz rozwiązanie Contact Manager w programie Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="61fcd-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="61fcd-140">W oknie **Eksplorator rozwiązań** rozwiń projekt **ContactManager. MVC** , a następnie kliknij dwukrotnie węzeł **Web. config** .</span><span class="sxs-lookup"><span data-stu-id="61fcd-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="61fcd-141">Projekt ContactManager. MVC zawiera dwa pliki *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="61fcd-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="61fcd-142">Należy edytować plik poziomu projektu.</span><span class="sxs-lookup"><span data-stu-id="61fcd-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="61fcd-143">W elemencie **connectionStrings** Sprawdź, czy parametry połączenia o nazwie **ApplicationServices** wskazują lokalną bazę danych usług aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="61fcd-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="61fcd-144">W oknie **Eksplorator rozwiązań** rozwiń projekt **ContactManager. Service** , a następnie kliknij dwukrotnie węzeł **Web. config** .</span><span class="sxs-lookup"><span data-stu-id="61fcd-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="61fcd-145">W elemencie **connectionStrings** w parametrach połączenia o nazwie **ContactManagerContext**upewnij się, że właściwość **Źródło danych** jest ustawiona na lokalne wystąpienie SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="61fcd-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="61fcd-146">Nie trzeba zmieniać żadnych innych parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="61fcd-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="61fcd-147">Zapisz wszystkie otwarte pliki.</span><span class="sxs-lookup"><span data-stu-id="61fcd-147">Save all open files.</span></span>

<span data-ttu-id="61fcd-148">Teraz możesz być gotowy do uruchomienia rozwiązania Contact Manager na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="61fcd-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="61fcd-149">Jeśli wykonasz te kroki bez wcześniejszego tworzenia bazy danych usług aplikacji, ASP.NET utworzy bazę danych przy pierwszej próbie utworzenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="61fcd-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="61fcd-150">Jednak ręczne tworzenie bazy danych zapewnia znacznie większą kontrolę nad zestawem funkcji usług aplikacji, który ma być obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="61fcd-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>

<span data-ttu-id="61fcd-151">**Aby uruchomić rozwiązanie Contact Manager**</span><span class="sxs-lookup"><span data-stu-id="61fcd-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="61fcd-152">W programie Visual Studio 2010 naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="61fcd-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="61fcd-153">Program Internet Explorer uruchamia i żąda adresu URL aplikacji Contact Manager ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="61fcd-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="61fcd-154">Domyślnie aplikacja wyświetla stronę **wszystkie kontakty** .</span><span class="sxs-lookup"><span data-stu-id="61fcd-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="61fcd-155">Dodaj kilka kontaktów, a następnie sprawdź, czy aplikacja działa zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="61fcd-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="61fcd-156">Przejdź do `http://localhost:50114/Account/Register` (Dostosuj adres URL, jeśli aplikacja jest obsługiwana na innym porcie).</span><span class="sxs-lookup"><span data-stu-id="61fcd-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="61fcd-157">Dodaj nazwę użytkownika, adres e-mail i hasło, a następnie sprawdź, czy możliwe jest pomyślne zarejestrowanie konta.</span><span class="sxs-lookup"><span data-stu-id="61fcd-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="61fcd-158">Przejdź do `http://localhost:50114/Account/LogOn` (Dostosuj adres URL, jeśli aplikacja jest obsługiwana na innym porcie).</span><span class="sxs-lookup"><span data-stu-id="61fcd-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="61fcd-159">Sprawdź, czy możesz się zalogować, używając właśnie utworzonego konta.</span><span class="sxs-lookup"><span data-stu-id="61fcd-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="61fcd-160">Zamknij program Internet Explorer, aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="61fcd-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="61fcd-161">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="61fcd-161">Conclusion</span></span>

<span data-ttu-id="61fcd-162">W tym momencie rozwiązanie Contact Manager powinno być w pełni skonfigurowane do uruchamiania na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="61fcd-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="61fcd-163">Możesz użyć rozwiązania jako odniesienia podczas pracy w innych tematach w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="61fcd-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="61fcd-164">W następnym temacie, [opisującym plik projektu](understanding-the-project-file.md), wyjaśniono, jak można użyć plików projektu niestandardowych Microsoft Build Engine (MSBuild) w rozwiązaniu Contact Manager do sterowania procesem wdrażania.</span><span class="sxs-lookup"><span data-stu-id="61fcd-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="61fcd-165">[Poprzednie](the-contact-manager-solution.md)
> [dalej](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="61fcd-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
